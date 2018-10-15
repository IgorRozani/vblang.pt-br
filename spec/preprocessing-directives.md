# <a name="preprocessing-directives"></a>Pré-processando diretivas

Depois que um arquivo tenha sido analisado lexicalmente, ocorrem vários tipos de fonte de pré-processamento. A compilação condicional, mais importante, determina qual fonte é processada pela gramática sintática; outros dois tipos de diretivas, diretivas de fonte externa e diretivas de região-- fornecem meta-informações sobre a origem, mas não têm nenhum efeito na compilação.

## <a name="conditional-compilation"></a>Compilação condicional

Compilação condicional controla se as sequências de linhas lógicas são convertidas em código real. No início da compilação condicional, todas as linhas lógicas estão habilitadas; No entanto, colocar as linhas em instruções de compilação condicional pode desabilitar seletivamente essas linhas dentro do arquivo, fazendo com que eles devem ser ignorados durante o restante do processo de compilação.

```antlr
CCStart
    : CCStatement*
    ;

CCStatement
    : CCConstantDeclaration
    | CCIfGroup
    | LogicalLine
    ;

CCExpression
    : LiteralExpression
    | CCParenthesizedExpression
    | CCSimpleNameExpression
    | CCCastExpression
    | CCOperatorExpression
    | CCConditionalExpression
    ;

CCParenthesizedExpression
    : '(' CCExpression ')'
    ;

CCSimpleNameExpression
    : Identifier
    ;

CCCastExpression
    : 'DirectCast' '(' CCExpression ',' TypeName ')'
    | 'TryCast' '(' CCExpression ',' TypeName ')'
    | 'CType' '(' CCExpression ',' TypeName ')'
    | CastTarget '(' CCExpression ')'
    ;

CCOperatorExpression
    : CCUnaryOperator CCExpression
    | CCExpression CCBinaryOperator CCExpression
    ;

CCUnaryOperator
    : '+' | '-' | 'Not'
    ;

CCBinaryOperator
    : '+'     | '-'     | '*'   | '/'       | '\\'     | 'Mod' | '^' | '='
    | '<' '>' | '<'     | '>'   | '<' '='   | '>' '='  | '&'
    | 'And'   | 'Or'    | 'Xor' | 'AndAlso' | 'OrElse'
    | '<' '<' | '>' '>'
    ;

CCConditionalExpression
    : 'If' '(' CCExpression ',' CCExpression ',' CCExpression ')'
    | 'If' '(' CCExpression ',' CCExpression ')'
    ;
```


Por exemplo, o programa

```vb
#Const A = True
#Const B = False

Class C

#If A Then
    Sub F()
    End Sub
#Else
    Sub G()
    End Sub
#End If

#If B Then
    Sub H()
    End Sub
#Else
    Sub I()
    End Sub
#End If

End Class
```

produz a mesma sequência exata dos tokens que o programa

```vb
Class C
    Sub F()
    End Sub

    Sub I()
    End Sub
End Class
```

As expressões de constante permitidas em diretivas de compilação condicional são um subconjunto de expressões de constante gerais.

O pré-processador permite espaço em branco e continuações de linha explícita antes e depois de cada token.


### <a name="conditional-constant-directives"></a>Diretivas de constante condicionais

Instruções condicionais de constante definem constantes que existem em um espaço de declaração separada de compilação condicional no escopo do arquivo de origem.

```antlr
CCConstantDeclaration
    : '#' 'Const' Identifier '=' CCExpression LineTerminator
    ;
```

O espaço de declaração é especial em que nenhuma declaração explícita de constantes de compilação condicional é necessário – constantes condicionais podem ser definidas implicitamente em uma diretiva de compilação condicional.

Antes que está sendo atribuído um valor, uma constante de compilação condicional tem o valor `Nothing`. Quando uma constante de compilação condicional é atribuída um valor, que deve ser uma expressão constante, o tipo da constante torna-se o tipo do valor que está sendo atribuído a ele. Uma constante de compilação condicional pode ser redefinida várias vezes ao longo de um arquivo de origem.

Por exemplo, o código a seguir imprime somente a cadeia de caracteres `about to print value` e o valor de `Test`.

```vb
Module M1
    Sub PrintValue(Test As Integer)

#Const DebugCode = True

#If DebugCode Then
        Console.WriteLine("about to print value")
#End If

#Const DebugCode = False

        Console.WriteLine(Test)

#If DebugCode Then
        Console.WriteLine("printed value")
#End If

    End Sub
End Module
```

O ambiente de compilação também pode definir constantes condicionais em um espaço de declaração de compilação condicional.


### <a name="conditional-compilation-directives"></a>Diretivas de compilação condicional

Diretivas de compilação condicional controlam a compilação condicional.

```antlr
CCIfGroup
    : '#' 'If' CCExpression 'Then'? LineTerminator CCStatement*
    CCElseIfGroup* CCElseGroup? '#' 'End' 'If' LineTerminator
    ;

CCElseIfGroup
    : '#' ElseIf CCExpression 'Then'? LineTerminator CCStatement*
    ;

CCElseGroup
    : '#' 'Else' LineTerminator CCStatement*
    ;
```

Constantes condicional só podem referenciar expressões constantes e constantes de compilação condicional. Cada uma das expressões constantes dentro de um grupo único de compilação condicional é avaliada e convertida para o `Boolean` tipo na ordem textual do primeiro ao último até que um da condicional expressões for avaliada como `True`. Se uma expressão não é conversível para `Boolean`, resultados de um erro de tempo de compilação. Semântica permissiva e comparações de cadeia de caracteres binária são sempre usadas ao avaliar expressões de constantes de compilação condicional, independentemente de qualquer `Option` diretivas ou configurações de ambiente de compilação.

Todas as linhas entre o grupo, inclusive diretivas de compilação condicional aninhada, estão desabilitadas, exceto para as linhas entre a instrução que contém o `True` expressão e a próxima instrução condicional do grupo, ou as linhas entre os `Else`instrução e o `End If` instrução se um `Else` aparece no grupo e todas as expressões são avaliadas como `False`.

Neste exemplo, a chamada para `WriteToLog` no `Trace` diretiva de compilação condicional não será processada porque ao redor `Debug` diretiva de compilação condicional é avaliada como `False`.

```vb
#Const Debug = False   ' Debugging off
#Const Trace = True    ' Tracing on

Class PurchaseTransaction
    Sub Commit()

#If Debug Then
        CheckConsistency()
#If Trace Then
        WriteToLog(Me.ToString())
#End If
#End If
        ...
    End Sub
End Class
```


## <a name="external-source-directives"></a>Diretivas de fonte externa

Um arquivo de origem pode incluir diretivas de fonte externa que indicam um mapeamento entre as linhas de origem e o texto externo à origem.

```antlr
ESDStart
    : ExternalSourceStatement*
    ;

ExternalSourceStatement
    : ExternalSourceGroup
    | LogicalLine
    ;

ExternalSourceGroup
    : '#' 'ExternalSource' '(' StringLiteral ',' IntLiteral ')' LineTerminator
      LogicalLine* '#' 'End' 'ExternalSource' LineTerminator
    ;
```

Diretivas de fonte externa não têm nenhum efeito na compilação e não podem ser aninhadas. Por exemplo:

```vb
Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```


## <a name="region-directives"></a>Diretivas de região

Diretivas de região agrupar linhas de código-fonte, mas não têm nenhum outro efeito na compilação. O grupo inteiro pode ser recolhido e oculto, ou expandido e exibido no ambiente de desenvolvimento integrado (IDE).

```antlr
RegionStart
    : RegionStatement*
    ;

RegionStatement
    : RegionGroup
    | LogicalLine
    ;

RegionGroup
    : '#' 'Region' StringLiteral LineTerminator
      RegionStatement*
      '#' 'End' 'Region' LineTerminator
    ;
```

Regiões podem ser aninhadas. Diretivas de região são especiais em que eles não podem começar nem terminar dentro de um corpo de método, e eles devem respeitar a estrutura de bloco do programa. Por exemplo:

```vb
Module Test
#Region "Startup code - do not edit"
    Sub Main()
    End Sub
#End Region

End Module


' Error due to Region directives breaking the block structure
Class C
#Region "Fred"
End Class
#End Region
```


## <a name="external-checksum-directives"></a>Diretivas de soma de verificação externa

Um arquivo de origem pode incluir uma diretiva de soma de verificação externa que indica quais soma de verificação deve ser emitida para um arquivo referenciado em uma diretiva de fonte externa. Em todos os outros aspectos diretivas de fonte externa não têm efeito na compilação.

```antlr
ExternalChecksumStart
    : ExternalChecksumStatement*
    ;

ExternalChecksumStatement
    : '#' 'ExternalChecksum' '('
      StringLiteral ',' StringLiteral ',' StringLiteral
      ')' LineTerminator
    ;
```

Uma diretiva de soma de verificação externa contém o nome do arquivo do arquivo externo, um identificador global exclusivo (GUID) associado com o arquivo e a soma de verificação para o arquivo. O GUID é especificado como uma constante de cadeia de caracteres no formato "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}", onde x é um dígito hexadecimal. A soma de verificação é especificada como uma constante de cadeia de caracteres do formulário "xxxx...", onde x é um dígito hexadecimal. O número de dígitos em uma soma de verificação deve ser um número par.

Um arquivo externo pode ter várias diretivas de soma de verificação externa associadas a ele, desde que todos os valores GUID e soma de verificação de correspondam exata. Se o nome do arquivo externo corresponde ao nome de um arquivo que está sendo compilado, a soma de verificação será ignorada em favor de cálculo de soma de verificação do compilador.

Por exemplo:

```vb
#ExternalChecksum("c:\wwwroot\inetpub\test.aspx", _
    "{12345678-1234-1234-1234-123456789abc}", _
    "1a2b3c4e5f617239a49b9a9c0391849d34950f923fab9484")

Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```

