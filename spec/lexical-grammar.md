# <a name="lexical-grammar"></a>Gramática lexical

Compilação de um programa Visual Basic primeiro envolve traduzindo o fluxo bruto de caracteres Unicode em um conjunto ordenado de tokens léxicos. Como a linguagem Visual Basic não é um formato livre, o conjunto de tokens, em seguida, é dividido em uma série de linhas lógicas. Um *linha lógica* spans do início do fluxo ou por meio de um terminador de linha para o próxima terminador de linha que não é precedido por uma continuação de linha ou por meio até o final do fluxo.

__Observação.__ Com a introdução de expressões literais XML na versão 9.0 do idioma, Visual Basic não tem mais uma gramática lexical distinta no sentido esse código pode ser indexado sem considerar o contexto sintático do Visual Basic. Isso é devido ao fato de que XML e o Visual Basic têm diferentes regras lexicais e o conjunto de regras lexicais em uso a qualquer momento determinado depende de quais constructo sintático está sendo processado no momento. Essa especificação retém nesta seção gramática lexical como um guia para regras lexicais de regular código do Visual Basic.

```antlr
LogicalLineStart
    : LogicalLine*
    ;

LogicalLine
    : LogicalLineElement* Comment? LineTerminator
    ;

LogicalLineElement
    : WhiteSpace
    | LineContinuation
    | Token
    ;

Token
    : Identifier
    | Keyword
    | Literal
    | Separator
    | Operator
    ;
```

## <a name="characters-and-lines"></a>Caracteres e linhas

Programas em Visual Basic são compostos de caracteres do conjunto de caracteres Unicode.

```antlr
Character:
    '<Any Unicode character except a LineTerminator>'
    ;
```

### <a name="line-terminators"></a>Terminadores de linha

Caracteres de quebra de linha Unicode separam linhas lógicas.

```antlr
LineTerminator
    : '<Unicode 0x00D>'
    | '<Unicode 0x00A>'
    | '<CR>'
    | '<LF>'
    | '<Unicode 0x2028>'
    | '<Unicode 0x2029>'
    ;
```

### <a name="line-continuation"></a>Continuação de linha

Um *continuação de linha* consiste em pelo menos um caractere de espaço em branco que precede imediatamente um único caractere de sublinhado como o último caractere (que não seja espaço em branco) em uma linha de texto. Uma continuação de linha permite que uma linha lógica abranger mais de uma linha física. As continuações das linhas são tratadas como se fossem espaço em branco, mesmo que eles não são.

```antlr
LineContinuation
    : WhiteSpace '_' WhiteSpace* LineTerminator
    ;
```

O programa a seguir mostra alguns continuações de linha:

```vb
Module Test
    Sub Print( _
        Param1 As Integer, _
        Param2 As Integer )

        If (Param1 < Param2) Or _
            (Param1 > Param2) Then
            Console.WriteLine("Not equal")
        End If
    End Function
End Module
```

Alguns locais na gramática sintática permitem *continuações de linha implícitas*. Quando um terminador de linha é encontrado

* Depois de uma vírgula (`,`), abra parênteses (`(`), abra a chave de abertura (`{`), ou abra a expressão inserida (`<%=`)

* Depois de um qualificador de membro (`.` ou `.@` ou `...`), desde que algo está sendo qualificado (ou seja, não está usando implícito `With` contexto)

* antes de um parêntese de fechamento (`)`), feche a chave de fechamento (`}`), ou de expressão incorporada fechar (`%>`)

* Após um menor-que (`<`) em um contexto de atributo

* antes de uma maior-que (`>`) em um contexto de atributo

* Depois de uma maior-que (`>`) em um contexto de atributo de nível de arquivo não

* antes e depois de operadores de consulta (`Where`, `Order`, `Select`, etc.)

* Depois de operadores binários (`+`, `-`, `/`, `*`, etc.) em um contexto de expressão

* Depois de operadores de atribuição (`=`, `:=`, `+=`, `-=`, etc.) em qualquer contexto.

o terminador de linha é tratado como se fosse uma continuação de linha.

```antlr
Comma
    : ',' LineTerminator?
    ;

Period
    : '.' LineTerminator?
    ;

OpenParenthesis
    : '(' LineTerminator?
    ;

CloseParenthesis
    : LineTerminator? ')'
    ;

OpenCurlyBrace
    : '{' LineTerminator?
    ;

CloseCurlyBrace
    : LineTerminator? '}'
    ;

Equals
    : '=' LineTerminator?
    ;

ColonEquals
    : ':' '=' LineTerminator?
    ;
```

Por exemplo, o exemplo anterior também poderia ser escrito como:

```vb
Module Test
    Sub Print(
        Param1 As Integer,
        Param2 As Integer)

        If (Param1 < Param2) Or
            (Param1 > Param2) Then
            Console.WriteLine("Not equal")
        End If
    End Function
End Module
```

Continuação implícita de linha só será inferida diretamente antes ou após o token especificado. Eles não serão inferidos antes ou depois de uma continuação de linha. Por exemplo:

```vb
Dim y = 10
' Error: Expression expected for assignment to x
Dim x = _

y
```

Continuações de linha não serão inferidas em contextos de compilação condicional. (__Observação.__ Essa última restrição é necessária porque o texto em blocos de compilação condicional que não são compilados não precisa ser sintaticamente válido. Assim, texto no bloco de pode acidentalmente "selecionado para cima" da instrução de compilação condicional, especialmente à medida que a linguagem obtém estendida no futuro.)


### <a name="white-space"></a>Espaço em branco

*Espaço em branco* serve apenas para separar os tokens e é ignorado. Linhas de lógicas que contém somente espaços em branco são ignoradas. (__Observação.__
Terminadores de linha não são considerados espaço em branco.)

```antlr
WhiteSpace
    : '<Unicode class Zs>'
    | '<Unicode Tab 0x0009>'
    ;
```

### <a name="comments"></a>Comentários

Um *comentário* começa com um caractere de aspas simples ou a palavra-chave `REM`. Um caractere de aspas simples é um caractere de aspas simples ASCII, um caractere de aspas simples à esquerda do Unicode ou um direito de Unicode caractere de aspas simples. Comentários podem começar em qualquer lugar em uma linha de código-fonte e o final da linha física termina o comentário. O compilador ignora os caracteres entre o início do comentário e o terminador de linha. Consequentemente, os comentários não é possível estender em várias linhas usando continuações de linha.

```antlr
Comment
    : CommentMarker Character*
    ;

CommentMarker
    : SingleQuoteCharacter
    | 'REM'
    ;

SingleQuoteCharacter
    : '\''
    | '<Unicode 0x2018>'
    | '<Unicode 0x2019>'
    ;
```

## <a name="identifiers"></a>Identificadores

Uma *identificador* é um nome. Identificadores de Visual Basic estão em conformidade com o Unicode Standard Annex 15 com uma exceção: identificadores podem começar com um caractere de sublinhado (conector). Se um identificador começa com um sublinhado, ele deve conter pelo menos um outro caractere identificador válido para resolver a ambiguidade de uma continuação de linha.

```antlr
Identifier
    : NonEscapedIdentifier TypeCharacter?
    | Keyword TypeCharacter
    | EscapedIdentifier
    ;

NonEscapedIdentifier
    : '<Any IdentifierName but not Keyword>'
    ;

EscapedIdentifier
    : '[' IdentifierName ']'
    ;

IdentifierName
    : IdentifierStart IdentifierCharacter*
    ;

IdentifierStart
    : AlphaCharacter
    | UnderscoreCharacter IdentifierCharacter
    ;

IdentifierCharacter
    : UnderscoreCharacter
    | AlphaCharacter
    | NumericCharacter
    | CombiningCharacter
    | FormattingCharacter
    ;

AlphaCharacter
    : '<Unicode classes Lu,Ll,Lt,Lm,Lo,Nl>'
    ;

NumericCharacter
    : '<Unicode decimal digit class Nd>'
    ;

CombiningCharacter
    : '<Unicode combining character classes Mn, Mc>'
    ;

FormattingCharacter
    : '<Unicode formatting character class Cf>'
    ;

UnderscoreCharacter
    : '<Unicode connection character class Pc>'
    ;

IdentifierOrKeyword
    : Identifier
    | Keyword
    ;
```

Identificadores normais não podem corresponder a palavras-chave, mas podem identificadores escapados ou identificadores com um caractere de tipo. Uma *identificador de escape* é um identificador delimitado por colchetes. Escape identificadores seguem as mesmas regras de identificadores regulares, exceto que eles podem corresponder a palavras-chave e podem não ter caracteres de tipo.

Este exemplo define uma classe chamada `class` com um método compartilhado denominado `shared` que usa um parâmetro denominado `boolean` e, em seguida, chama o método.

```vb
Class [class]
    Shared Sub [shared]([boolean] As Boolean)
        If [boolean] Then
            Console.WriteLine("true")
        Else
            Console.WriteLine("false")
        End If
    End Sub
End Class

Module [module]
    Sub Main()
        [class].[shared](True)
    End Sub
End Module
```

Identificadores diferenciam maiusculas de minúsculas, portanto, dois identificadores serão considerados para ser o mesmo identificador se eles diferem apenas em maiusculas. (__Observação.__ A mapeamentos de casos individual padrão Unicode são usados durante a comparação de identificadores e os mapeamentos de maiusculas específicas da localidade são ignorados).


### <a name="type-characters"></a>Caracteres de tipo

Um *caractere de tipo* indica o tipo do identificador anterior. O caractere de tipo não é considerado parte do identificador.

```antlr
TypeCharacter
    : IntegerTypeCharacter
    | LongTypeCharacter
    | DecimalTypeCharacter
    | SingleTypeCharacter
    | DoubleTypeCharacter
    | StringTypeCharacter
    ;

IntegerTypeCharacter
    : '%'
    ;

LongTypeCharacter
    : '&'
    ;

DecimalTypeCharacter
    : '@'
    ;

SingleTypeCharacter
    : '!'
    ;

DoubleTypeCharacter
    : '#'
    ;

StringTypeCharacter
    : '$'
    ;
```

Se uma declaração inclui um caractere de tipo, o caractere de tipo deve concordar com o tipo especificado na declaração em si; Caso contrário, ocorrerá um erro de tempo de compilação. Se a declaração omite o tipo (por exemplo, se ele não especifica um `As` cláusula), o caractere de tipo implicitamente é substituído como o tipo da declaração.

Nenhum espaço em branco podem vir entre um identificador e seu caractere de tipo. Não há nenhum caractere de tipo para `Byte`, `SByte`, `UShort`, `Short`, `UInteger` ou `ULong`, devido à falta de caracteres adequados.

Acrescentar um caractere de tipo para um identificador que conceitualmente não tem um tipo (por exemplo, um nome de namespace) ou a um identificador cujo tipo não concorda com o tipo de caractere de tipo faz com que um erro de tempo de compilação.

O exemplo a seguir mostra o uso de caracteres de tipo:

```vb
' The follow line will cause an error: standard modules have no type.
Module Test1#
End Module

Module Test2

    ' This function takes a Long parameter and returns a String.
    Function Func$(Param&)

        ' The following line causes an error because the type character
        ' conflicts with the declared type of Func and Param.
        Func# = CStr(Param@)

        ' The following line is valid.
        Func$ = CStr(Param&)
    End Function
End Module
```

O caractere de tipo `!` apresenta um problema especial que pode ser usado como um caractere de tipo e como um separador na linguagem. Para remover a ambiguidade, um `!` caractere é um caractere de tipo, desde que o caractere seguinte não é possível iniciar um identificador. Se for possível, então o `!` caractere é um separador, não é um caractere de tipo.


## <a name="keywords"></a>Palavras-chave

Um *palavra-chave* é uma palavra que tem um significado especial em um constructo de linguagem. Todas as palavras-chave são reservadas pelo idioma e não podem ser usadas como identificadores, a menos que os identificadores são ignorados. (__Observação.__ `EndIf``GoSub`, `Let`, `Variant`, e `Wend` são mantidos como palavras-chave, embora eles não são mais usados no Visual Basic.)

```antlr
Keyword
    : 'AddHandler'      | 'AddressOf'      | 'Alias'       | 'And'
    | 'AndAlso'         | 'As'             | 'Boolean'     | 'ByRef'
    | 'Byte'            | 'ByVal'          | 'Call'        | 'Case'        
    | 'Catch'           | 'CBool'          | 'CByte'       | 'CChar'       
    | 'CDate'           | 'CDbl'           | 'CDec'        | 'Char'        
    | 'CInt'            | 'Class'          | 'CLng'        | 'CObj'        
    | 'Const'           | 'Continue'       | 'CSByte'      | 'CShort'      
    | 'CSng'            | 'CStr'           | 'CType'       | 'CUInt'       
    | 'CULng'           | 'CUShort'        | 'Date'        | 'Decimal'     
    | 'Declare'         | 'Default'        | 'Delegate'    | 'Dim'         
    | 'DirectCast'      | 'Do'             | 'Double'      | 'Each'        
    | 'Else'            | 'ElseIf'         | 'End'         | 'EndIf'       
    | 'Enum'            | 'Erase'          | 'Error'       | 'Event'       
    | 'Exit'            | 'False'          | 'Finally'     | 'For'         
    | 'Friend'          | 'Function'       | 'Get'         | 'GetType'     
    | 'GetXmlNamespace' | 'Global'         | 'GoSub'       | 'GoTo'        
    | 'Handles'         | 'If'             | 'Implements'  | 'Imports'     
    | 'In'              | 'Inherits'       | 'Integer'     | 'Interface'   
    | 'Is'              | 'IsNot'          | 'Let'         | 'Lib'         
    | 'Like'            | 'Long'           | 'Loop'        | 'Me'          
    | 'Mod'             | 'Module'         | 'MustInherit' | 'MustOverride'
    | 'MyBase'          | 'MyClass'        | 'Namespace'   | 'Narrowing'   
    | 'New'             | 'Next'           | 'Not'         | 'Nothing'     
    | 'NotInheritable'  | 'NotOverridable' | 'Object'      | 'Of'          
    | 'On'              | 'Operator'       | 'Option'      | 'Optional'    
    | 'Or'              | 'OrElse'         | 'Overloads'   | 'Overridable' 
    | 'Overrides'       | 'ParamArray'     | 'Partial'     | 'Private'     
    | 'Property'        | 'Protected'      | 'Public'      | 'RaiseEvent'  
    | 'ReadOnly'        | 'ReDim'          | 'REM'         | 'RemoveHandler'
    | 'Resume'          | 'Return'         | 'SByte'       | 'Select'      
    | 'Set'             | 'Shadows'        | 'Shared'      | 'Short'       
    | 'Single'          | 'Static'         | 'Step'        | 'Stop'        
    | 'String'          | 'Structure'      | 'Sub'         | 'SyncLock'    
    | 'Then'            | 'Throw'          | 'To'          | 'True'        
    | 'Try'             | 'TryCast'        | 'TypeOf'      | 'UInteger'    
    | 'ULong'           | 'UShort'         | 'Using'       | 'Variant'     
    | 'Wend'            | 'When'           | 'While'       | 'Widening'    
    | 'With'            | 'WithEvents'     | 'WriteOnly'   | 'Xor'         
    ;
```

## <a name="literals"></a>Literais

Um *literal* é uma representação textual de um determinado valor de um tipo. Tipos de literal incluem booliano, inteiro, ponto flutuante, cadeia de caracteres, caractere e data.

```antlr
Literal
    : BooleanLiteral
    | IntegerLiteral
    | FloatingPointLiteral
    | StringLiteral
    | CharacterLiteral
    | DateLiteral
    | Nothing
    ;
```

### <a name="boolean-literals"></a>Literais boolianos

`True` e `False` são literais do `Boolean` tipo que são mapeados para o estado true e false, respectivamente.

```antlr
BooleanLiteral
    : 'True' | 'False'
    ;
```

### <a name="integer-literals"></a>Literais inteiros

Literais inteiros podem ser decimal (base 10), hexadecimal (base 16) ou octal (base 8). Um inteiro decimal literal é uma cadeia de caracteres de dígitos decimais (0-9). É um literal hexadecimal `&H` seguido por uma cadeia de caracteres de dígitos hexadecimais (0-9, A-F). É um literal octal `&O` seguido por uma cadeia de caracteres de dígitos octais (0-7). Literais decimais diretamente representam o valor decimal de literal integral, ao passo que literais octais e hexadecimais representam o valor binário do inteiro literal (portanto, `&H8000S` é de -32768, não um erro de estouro).

```antlr
IntegerLiteral
    : IntegralLiteralValue IntegralTypeCharacter?
    ;

IntegralLiteralValue
    : IntLiteral
    | HexLiteral
    | OctalLiteral
    ;

IntegralTypeCharacter
    : ShortCharacter
    | UnsignedShortCharacter
    | IntegerCharacter
    | UnsignedIntegerCharacter
    | LongCharacter
    | UnsignedLongCharacter
    | IntegerTypeCharacter
    | LongTypeCharacter
    ;

ShortCharacter
    : 'S'
    ;

UnsignedShortCharacter
    : 'US'
    ;

IntegerCharacter
    : 'I'
    ;

UnsignedIntegerCharacter
    : 'UI'
    ;

LongCharacter
    : 'L'
    ;

UnsignedLongCharacter
    : 'UL'
    ;

IntLiteral
    : Digit+
    ;

HexLiteral
    : '&' 'H' HexDigit+
    ;

OctalLiteral
    : '&' 'O' OctalDigit+
    ;

Digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

HexDigit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F'
    ;

OctalDigit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7'
    ;
```

O tipo de um literal é determinado por seu valor ou o caractere de tipo a seguir. Se nenhum caractere de tipo for especificado, os valores no intervalo da `Integer` tipo são digitadas como `Integer`; valores fora do intervalo para `Integer` são digitadas como `Long`. Se o tipo de um literal inteiro é de tamanho suficiente para manter o inteiro literal, um erro de tempo de compilação ocorre. (__Observação.__ Não há um caractere de tipo para `Byte` porque o caractere mais natural seria `B`, que é um caractere legal em um literal hexadecimal.)


### <a name="floating-point-literals"></a>Literais de ponto flutuantes

Um literal de ponto flutuante é um literal de inteiro seguido por um ponto decimal opcional (o caractere de ponto de ASCII) e mantissa e um expoente opcional de base 10. Por padrão, um literal de ponto flutuante é do tipo `Double`. Se o `Single`, `Double`, ou `Decimal` caractere de tipo for especificado, o literal é desse tipo. Se o tipo de um literal de ponto flutuante é de tamanho insuficiente para manter o literal de ponto flutuante, um erro de tempo de compilação ocorre.

__Observação.__ Vale a pena observar que o `Decimal` tipo de dados pode codificar os zeros à direita em um valor. A especificação não faz no momento, nenhum comentário sobre se zero à direita um `Decimal` literal deve ser aceito por um compilador.

```antlr
FloatingPointLiteral
    : FloatingPointLiteralValue FloatingPointTypeCharacter?
    | IntLiteral FloatingPointTypeCharacter
    ;

FloatingPointTypeCharacter
    : SingleCharacter
    | DoubleCharacter
    | DecimalCharacter
    | SingleTypeCharacter
    | DoubleTypeCharacter
    | DecimalTypeCharacter
    ;

SingleCharacter
    : 'F'
    ;

DoubleCharacter
    : 'R'
    ;

DecimalCharacter
    : 'D'
    ;

FloatingPointLiteralValue
    : IntLiteral '.' IntLiteral Exponent?
    | '.' IntLiteral Exponent?
    | IntLiteral Exponent
    ;

Exponent
    : 'E' Sign? IntLiteral
    ;

Sign
    : '+'
    | '-'
    ;
```

### <a name="string-literals"></a>Literais da Cadeia de Caracteres

Um literal de cadeia de caracteres é uma sequência de Unicode de zero ou mais caracteres de início e terminando com um caractere de aspas duplas ASCII, Unicode ou um caractere de aspas duplas Unicode à esquerda com o botão direito caractere de aspas duplas. Dentro de uma cadeia de caracteres, uma sequência de dois caracteres de aspas duplas é uma sequência de escape que representa uma aspa dupla na cadeia de caracteres.

```antlr
StringLiteral
    : DoubleQuoteCharacter StringCharacter* DoubleQuoteCharacter
    ;

DoubleQuoteCharacter
    : '"'
    | '<unicode left double-quote 0x201c>'
    | '<unicode right double-quote 0x201D>'
    ;

StringCharacter
    : '<Any character except DoubleQuoteCharacter>'
    | DoubleQuoteCharacter DoubleQuoteCharacter
    ;
```

Uma constante de cadeia de caracteres é do `String` tipo.

```vb
Module Test
    Sub Main()

        ' This prints out: ".
        Console.WriteLine("""")

        ' This prints out: a"b.
        Console.WriteLine("a""b")

        ' This causes a compile error due to mismatched double-quotes.
        Console.WriteLine("a"b")
    End Sub
End Module
```

O compilador tem permissão para substituir uma expressão de cadeia de caracteres constante com uma literal de cadeia de caracteres. Cada cadeia de caracteres literal não resulta necessariamente em uma nova instância de cadeia de caracteres. Quando dois ou mais literais de cadeia de caracteres que são equivalentes de acordo com o operador de igualdade de cadeia de caracteres usando a semântica de comparação binária aparecem no mesmo programa, esses literais de cadeia de caracteres podem se referir à mesma instância de cadeia de caracteres. Por exemplo, a saída do programa a seguir pode retornar `True` porque os dois literais podem se referir à mesma instância de cadeia de caracteres.

```vb
Module Test
    Sub Main()
        Dim a As Object = "he" & "llo"
        Dim b As Object = "hello"
        Console.WriteLine(a Is b)
    End Sub
End Module
```


### <a name="character-literals"></a>Literais de caracteres

Um literal de caractere representa um único caractere Unicode do `Char` tipo. Dois caracteres de aspas duplas é uma sequência de escape que representa o caractere de aspas duplas.

```antlr
CharacterLiteral
    : DoubleQuoteCharacter StringCharacter DoubleQuoteCharacter 'C'
    ;
```


```vb
Module Test
    Sub Main()

        ' This prints out: a.
        Console.WriteLine("a"c)

        ' This prints out: ".
        Console.WriteLine(""""c)
    End Sub
End Module
```


### <a name="date-literals"></a>Literais de data

Um literal de data representa um momento específico no tempo, expressado como um valor da `Date` tipo.

```antlr
DateLiteral
    : '#' WhiteSpace* DateOrTime WhiteSpace* '#'
    ;

DateOrTime
    : DateValue WhiteSpace+ TimeValue
    | DateValue
    | TimeValue
    ;

DateValue
    : MonthValue '/' DayValue '/' YearValue
    | MonthValue '-' DayValue '-' YearValue
    ;

TimeValue
    : HourValue ':' MinuteValue ( ':' SecondValue )? WhiteSpace* AMPM?
    | HourValue WhiteSpace* AMPM
    ;

MonthValue
    : IntLiteral
    ;

DayValue
    : IntLiteral
    ;

YearValue
    : IntLiteral
    ;

HourValue
    : IntLiteral
    ;

MinuteValue
    : IntLiteral
    ;

SecondValue
    : IntLiteral
    ;

AMPM
    : 'AM' | 'PM'
    ;    
```

O literal pode especificar uma data e hora, apenas uma data ou apenas uma vez. Se o valor de data for omitido, em seguida, em 1º de janeiro do ano 1 no calendário gregoriano será assumido. Se o valor de hora for omitido, 12:00:00 AM é assumido.

Para evitar problemas com a interpretação do valor do ano em um valor de data, o valor de ano não pode ser de dois dígitos. Ao expressar uma data no século primeiro AD/CE, zeros à esquerda devem ser especificado.

Um valor de tempo pode ser especificado usando um valor de 24 horas ou um valor de 12 horas; tempo valores que omitem uma `AM` ou `PM` são considerados valores de 24 horas. Se um valor de hora omite os minutos, o literal `0` é usado por padrão. Se um valor de hora omite os segundos, o literal `0` é usado por padrão. Se minutos e o segundo forem omitidos, então `AM` ou `PM` deve ser especificado. Se o valor de data especificado está fora do intervalo da `Date` digitar, ocorre um erro de tempo de compilação.

O exemplo a seguir contém várias literais de data.

```vb
Dim d As Date

d = # 8/23/1970 3:45:39AM #
d = # 8/23/1970 #              ' Date value: 8/23/1970 12:00:00AM.
d = # 3:45:39AM #              ' Date value: 1/1/1 3:45:39AM.
d = # 3:45:39 #                ' Date value: 1/1/1 3:45:39AM.
d = # 13:45:39 #               ' Date value: 1/1/1 1:45:39PM.
d = # 1AM #                    ' Date value: 1/1/1 1:00:00AM.
d = # 13:45:39PM #             ' This date value is not valid.
```


### <a name="nothing"></a>nada

`Nothing` é um literal especial; ele não tem um tipo e pode ser convertido para todos os tipos no sistema de tipos, incluindo parâmetros de tipo. Quando convertido em um tipo específico, ele é o equivalente do valor padrão desse tipo.

```antlr
Nothing
    : 'Nothing'
    ;
```

## <a name="separators"></a>Separadores

Os seguintes caracteres ASCII são separadores:

```antlr
Separator
    : '(' | ')' | '{' | '}' | '!' | '#' | ',' | '.' | ':' | '?'
    ;
```

## <a name="operator-characters"></a>Caracteres de operador

Os seguintes caracteres ASCII ou sequências de caracteres denotam operadores:

```antlr
Operator
    : '&' | '*' | '+' | '-' | '/' | '\\' | '^' | '<' | '=' | '>'
    ;
```

