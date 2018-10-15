# <a name="expressions"></a>Expressões

Uma expressão é uma sequência de operandos e operadores que especifica uma computação de um valor, ou que designa uma variável ou constante. Este capítulo define a sintaxe, a ordem de avaliação dos operandos e operadores e o significado das expressões.

```antlr
Expression
    : SimpleExpression
    | TypeExpression
    | MemberAccessExpression
    | DictionaryAccessExpression
    | InvocationExpression
    | IndexExpression
    | NewExpression
    | CastExpression
    | OperatorExpression
    | ConditionalExpression
    | LambdaExpression
    | QueryExpression
    | XMLLiteralExpression
    | XMLMemberAccessExpression
    ;
```

## <a name="expression-classifications"></a>Classificações de expressão

Cada expressão é classificado como um dos seguintes:

* *Um valor.* Cada valor tem um tipo associado.

* *Uma variável.* Cada variável tem um tipo associado, ou seja, o tipo declarado da variável.

* *Um namespace.* Uma expressão com essa classificação só pode aparecer como o lado esquerdo de um acesso de membro. Em qualquer outro contexto, uma expressão classificada como um namespace causa um erro de tempo de compilação.

* *Um tipo.* Uma expressão com essa classificação só pode aparecer como o lado esquerdo de um acesso de membro. Em qualquer outro contexto, uma classificado como um tipo de expressão causa um erro de tempo de compilação.

* *Um grupo de métodos,* que é um conjunto de métodos sobrecarregados no mesmo nome. Um grupo de método pode ter uma expressão de destino associado e uma lista de argumentos de tipo associado.

* *Um ponteiro de método,* que representa o local de um método. Um ponteiro de método pode ter uma expressão de destino associado e uma lista de argumentos de tipo associado.

* *Um método lambda,* que é um método anônimo.

* *Um grupo de propriedades,* que é um conjunto de propriedades sobrecarregado no mesmo nome. Um grupo de propriedade pode ter uma expressão de destino associados.

* *Um acesso de propriedade.* Todo acesso de propriedade tem um tipo associado, ou seja, o tipo da propriedade. Um acesso de propriedade pode ter uma expressão de destino associados.

* *Um acesso de associação tardia,* que representa um método ou propriedade acesso adiado até o tempo de execução. Um acesso de associação tardia pode ter uma expressão de destino associado e uma lista de argumentos de tipo associado. O tipo de um acesso de associação tardia é sempre `Object`.

* *Acesso de um evento.* Acesso de cada evento tem um tipo associado, ou seja, o tipo do evento. Um acesso de evento pode ter uma expressão de destino associados. Um acesso de evento pode aparecer como o primeiro argumento do `RaiseEvent`, `AddHandler`, e `RemoveHandler` instruções. Em qualquer outro contexto, uma expressão classificada como um acesso de evento causa um erro de tempo de compilação.

* *Uma matriz literal,* que representa os valores iniciais de uma matriz cujo tipo ainda não foi determinado.

* *Void.* Isso ocorre quando a expressão é uma invocação de uma sub-rotina ou uma expressão de operador de espera nenhum resultado. Uma expressão classificada como void só é válida no contexto de uma instrução de chamada ou uma instrução de espera.

* *Um valor padrão.* Somente o literal `Nothing` produz essa classificação.

O resultado final de uma expressão geralmente é um valor ou uma variável, com as outras categorias de expressões que funcionam como valores intermediários são permitidos somente em determinados contextos.

Observe que as expressões cujo tipo é um parâmetro de tipo podem ser usadas em instruções e expressões que exigem o tipo de uma expressão para ter determinadas características (como sendo um tipo de referência, o tipo de valor, derivando de algum tipo, etc.) se as restrições impostas no parâmetro de tipo atender a essas características.

### <a name="expression-reclassification"></a>Reclassificação de expressão

Normalmente, quando uma expressão é usada em um contexto que requer uma classificação diferente da expressão, um erro de tempo de compilação ocorre – por exemplo, a tentativa de atribuir um valor para um literal. No entanto, em muitos casos é possível alterar a classificação de uma expressão pelo processo de *reclassificação*.

Se a reclassificação for bem-sucedida, a reclassificação é julgada como ampliação ou redução. A menos que indicado o contrário, todos os reclassifications nessa lista são de ampliação.

Os seguintes tipos de expressões podem ser reclassificados:

* Uma variável pode ser reclassificada como um valor. O valor armazenado na variável é procurado.

* Um grupo de método pode ser reclassificado como um valor. A expressão de grupo de método é interpretada como uma expressão de invocação com a expressão de destino associado e a lista de parâmetros de tipo e o parênteses vazios (ou seja, `f` é interpretado como `f()` e `f(Of Integer)` é interpretado como `f(Of Integer)()`). Este reclassificação pode resultar na expressão que está sendo ainda mais reclassificado como nula.

* Um ponteiro de método pode ser reclassificado como um valor. Este reclassificação só pode ocorrer no contexto de uma conversão em que o tipo de destino é conhecido. A expressão de ponteiro de método é interpretada como o argumento para uma expressão de instanciação de delegado do tipo apropriado com a lista de argumentos de tipo associado. Por exemplo:
    
    ```vb
    Delegate Sub D(i As Integer)
    
    Module Test
        Sub F(i As Integer)
        End Sub
    
        Sub Main()
            Dim del As D
    
            ' The next two lines are equivalent.
            del = AddressOf F
            del = New D(AddressOf F)
        End Sub
    End Module
    ```

* Um método lambda pode ser reclassificado como um valor. Se a reclassificação ocorre no contexto de uma conversão em que o tipo de destino é conhecido, um dos dois reclassifications pode ocorrer:
    
  1. Se o tipo de destino for um tipo de delegado, o método lambda é interpretado como o argumento para uma expressão de construção de delegado do tipo apropriado.
    
  2. Se o tipo de destino for `System.Linq.Expressions.Expression(Of T)`, e `T` é um tipo de delegado, em seguida, o método lambda é interpretado como se ele estava sendo usado na expressão de delegado de construção para `T` e, em seguida, convertida em uma árvore de expressão.
    
  Um método de lambda assíncrono ou iterador só pode ser interpretado como o argumento para uma expressão de delegado construção, se o delegado não tem nenhum parâmetro ByRef.
    
  Se a conversão de qualquer um dos tipos de parâmetro do representante para os tipos de parâmetro correspondente do lambda é uma conversão de redução, em seguida, reclassificação é considerada de estreitamento; Caso contrário, ele está ampliando.
    
  __Observação.__ A conversão exata entre os métodos de lambda e árvores de expressão não pode ser corrigida entre versões do compilador e está além do escopo desta especificação. Para Microsoft Visual Basic 11.0, todas as expressões lambda podem ser convertidas para árvores de expressão de acordo com as seguintes restrições: (1) 1.  Somente as expressões lambda de linha única sem parâmetros ByRef podem ser convertidas em árvores de expressão. Da linha única `Sub` lambdas, instruções de invocação somente podem ser convertidas para árvores de expressão. (2) expressões de tipo anônimo não podem ser convertidas para árvores de expressão, se um inicializador de campo anterior é usado para inicializar um inicializador de campo subsequentes, por exemplo, `New With {.a=1, .b=.a}`. (3) expressões do inicializador de objeto não podem ser convertidas em árvores de expressão se um membro do objeto atual que está sendo inicializado é usado em um dos inicializadores de campo, por exemplo, `New C1 With {.a=1, .b=.Method1()}`. (4) expressões de criação a matriz multidimensional só podem ser convertidas para árvores de expressão, se eles declararem explicitamente o seu tipo de elemento. (5) Late expressões de associação de não podem ser convertidas em árvores de expressão. (6) quando uma variável ou um campo é passado como ByRef como uma expressão de invocação, mas não tem exatamente o mesmo tipo que o parâmetro ByRef, ou quando uma propriedade é passada ByRef, a semântica normal do VB é que uma cópia do argumento é passada ByRef e seu valor final é então copiado  volta para a variável ou campo ou propriedade. Em árvores de expressão, o retorno de cópia não acontece. (7) todas as essas restrições se aplicam às expressões de lambda aninhada também.
    
  Se o tipo de destino não for conhecido, o método lambda é interpretado como o argumento para uma expressão de instanciação de delegado de um tipo de delegado anônimo com a mesma assinatura do método lambda. Se a semântica estrita está sendo usada e o tipo de qualquer um dos parâmetros são omitidos, ocorre um erro de tempo de compilação; Caso contrário, `Object` é substituída para qualquer tipo de parâmetro ausentes. Por exemplo:
    
  ```vb
  Module Test
      Sub Main()
          ' Type of x will be equivalent to Func(Of Object, Object, Object)
          Dim x = Function(a, b) a + b
  
          ' Type of y will be equivalent to Action(Of Object, Object)
          Dim y = Sub(a, b) Console.WriteLine(a + b)
      End Sub
  End Module
  ```

* Um grupo de propriedades pode ser reclassificado como um acesso de propriedade. A expressão de grupo de propriedade é interpretada como uma expressão de índice com parênteses vazios (ou seja, `f` é interpretado como `f()`).

* Um acesso de propriedade pode ser reclassificado como um valor. A expressão de acesso de propriedade é interpretada como uma expressão de invocação do `Get` acessador da propriedade. Se a propriedade não tem getter, ocorrerá um erro de tempo de compilação.

* Um acesso de associação tardia pode ser reclassificado como um método de associação tardia ou acesso de propriedade de associação tardia. Em uma situação em que um acesso de associação tardia pode ser reclassificado como um método de acesso e um acesso de propriedade, é preferível a reclassificação para um acesso de propriedade.

* Um acesso de associação tardia pode ser reclassificado como um valor.

* Um literal de matriz pode ser reclassificado como um valor. O tipo do valor é determinado da seguinte maneira:

  1. Se a reclassificação ocorre no contexto de uma conversão em que o tipo de destino é conhecido e o tipo de destino é um tipo de matriz, o literal de matriz é reclassificado como um valor do tipo T(). Se o tipo de destino for `System.Collections.Generic.IList(Of T)`, `IReadOnlyList(Of T)`, `ICollection(Of T)`, `IReadOnlyCollection(Of T)`, ou `IEnumerable(Of T)`e o literal de matriz tem um nível de aninhamento e, em seguida, o literal de matriz é reclassificado como um valor do tipo `T()`.

  2. Caso contrário, o literal de matriz é reclassificado como um valor cujo tipo é que uma matriz de classificação igual ao nível de aninhamento é usada, com o tipo de elemento determinado pelo tipo dominante dos elementos no inicializador; Se nenhum tipo dominante puder ser determinado, `Object` é usado. Por exemplo:

     ```vb
     ' x Is GetType(Double(,,))
     Dim x = { { { 1, 2.0 }, { 3, 4 } }, { { 5, 6 }, { 7, 8 } } }.GetType()
        
     ' y Is GetType(Integer())
     Dim y = { 1, 2, 3 }.GetType()
        
     ' z Is GetType(Object())
     Dim z = { 1, "2" }.GetType()
        
     ' Error: Inconsistent nesting
     Dim a = { { 10 }, { 20, 30 } }.GetType()
     ```

  __Observação.__ Há uma pequena alteração no comportamento entre a versão 9.0 e versão 10.0 do idioma. Anteriores à 10.0, inicializadores de elemento de matriz não afetou a inferência de tipo de variável local e agora eles fazem isso. Portanto, `Dim a() = { 1, 2, 3 }` seria ter inferido `Object()` como o tipo de `a` na versão 9.0 da linguagem e `Integer()` na versão 10.0.

  Em seguida, a reclassificação reinterpreta o literal de matriz como uma expressão de criação de matriz. Portanto, os exemplos:

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  são equivalentes a:

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  A reclassificação é julgada como redução se qualquer conversão de uma expressão de elemento para o tipo de elemento da matriz é restritiva; Caso contrário, ela é considerada de ampliação.

* O valor padrão `Nothing` pode ser reclassificado como um valor. Em um contexto no qual o tipo de destino é conhecido, o resultado é o valor padrão do tipo de destino. Em um contexto em que o tipo de destino não é conhecido, o resultado é um valor nulo do tipo `Object`.

Uma expressão de namespace, expressão de tipo, expressão de acesso de evento ou expressão void não pode ser reclassificado. Vários reclassifications podem ser feitas ao mesmo tempo. Por exemplo:

```vb
Module Test
    Sub F(i As Integer)
    End Sub

    ReadOnly Property P() As Integer
        Get
        End Get
    End Sub

    Sub Main()
        F(P)
    End Property
End Module
```

Nesse caso, a propriedade de expressão de grupo `P` é reclassificado pela primeira vez de um grupo de propriedades para um acesso de propriedade e, em seguida, reclassificado de um acesso de propriedade para um valor. O menor número de reclassifications é executado para alcançar uma classificação válida no contexto.

## <a name="constant-expressions"></a>Expressões constantes

Um *expressão constante* é uma expressão cujo valor pode ser completamente avaliado em tempo de compilação.

```antlr
ConstantExpression
    : Expression
    ;
```

O tipo de uma expressão constante pode ser `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Char`, `Single`, `Double`, `Decimal`, `Date`, `Boolean`, `String`, `Object`, ou qualquer tipo de enumeração. As seguintes construções são permitidas em expressões constantes:

* Literais (incluindo `Nothing`).

* Referências aos membros de tipo de constante ou locais de constante.

* Referências aos membros de tipos de enumeração.

* Subexpressões entre parênteses.

* Expressões de coerção, fornecido o tipo de destino é um dos tipos listados acima. Coerções de e para `String` são uma exceção a essa regra e são permitidos apenas em valores nulos porque `String` conversões são sempre feitas na cultura atual do ambiente de execução em tempo de execução. Observe que as expressões de constante coerção só podem usar conversões intrínseco.

* O `+`, `-` e `Not` operadores unários, fornecido o operando e o resultado é de um tipo listado acima.

* O `+`, `-`, `*`, `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, `And`, `Or`, `Xor`, `AndAlso`, `OrElse`, `=`, `<`, `>`, `<>`, `<=`, e `=>` fornecido de operadores binários, cada operando e o resultado é de um tipo listado acima.

* O operador condicional se, desde que cada operando e um resultado seja de um tipo listado acima.

* As seguintes funções de tempo de execução: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr` se o valor da constante estiver entre 0 e 128; `Microsoft.VisualBasic.Strings.AscW` se a cadeia de caracteres constante não estiver vazia; `Microsoft.VisualBasic.Strings.Asc` se a cadeia de caracteres constante não estiver vazia.

São as seguintes construções *não* permitidos em expressões constantes:

* A associação implícita por meio de um `With` contexto.

Expressões constantes do tipo integral (`ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `SByte`, ou `Byte`) pode ser convertido implicitamente em um tipo integral mais estreito, e expressões constantes do tipo `Double` pode ser convertido implicitamente em `Single`, desde que o valor da expressão constante é dentro do intervalo de tipo de destino. Essas conversões de redução são permitidas, independentemente se a semântica estrita ou permissiva está sendo usada.


## <a name="late-bound-expressions"></a>Expressões de associação tardia

Quando o destino de uma expressão de acesso de membro ou uma expressão de índice é do tipo `Object`, o processamento da expressão pode ser adiado até o tempo de execução. Adiar o processamento dessa maneira é chamado *ligação tardia*. Permite a associação tardia `Object` variáveis a serem usadas em um *sem especificação de tipo* forma, onde todas as resoluções de membros se baseia no tipo de tempo de execução real do valor na variável. Se a semântica estrita é especificada pelo ambiente de compilação ou por `Option Strict`, associação tardia causa um erro de tempo de compilação. Membros não públicos são ignorados ao fazer a ligação tardia, incluindo para fins de resolução de sobrecarga. Observe que, ao contrário do caso de associação inicial, invocando ou acessar um `Shared` membro tardia fará com que o destino de invocação a ser avaliada em tempo de execução. Se a expressão for uma expressão de invocação de um membro definido em `System.Object`, associação tardia não ocorrerá.

Em geral, os acessos de associação tardia são resolvidos em tempo de execução pesquisando o identificador no tipo de tempo de execução real da expressão. Se a pesquisa de membro de associação tardia falhar em tempo de execução, um `System.MissingMemberException` exceção é lançada. Como pesquisa de membro de associação tardia é feita apenas desativar o tipo de tempo de execução da expressão de destino associado, o tipo de tempo de execução de um objeto nunca é uma interface. Portanto, é impossível acessar membros de interface em uma expressão de acesso de membro de associação tardia.

Os argumentos para um acesso de membro de associação tardia são avaliados na ordem em que eles aparecem na expressão de acesso de membro: não a ordem na qual os parâmetros são declarados no membro de associação tardia. O exemplo a seguir ilustra essa diferença:

```vb
Class C
    Public Sub f(ByVal x As Integer, ByVal y As Integer)
    End Sub
End Class

Module Module1
    Sub Main()
        Console.Write("Early-bound: ")
        Dim c As C = New C
        c.f(y:=t("y"), x:=t("x"))

        Console.Write(vbCrLf & "Late-bound: ")
        Dim o As Object = New C
        o.f(y:=t("y"), x:=t("x"))
    End Sub

    Function t(ByVal s As String) As Integer
        Console.Write(s)
        Return 0
    End Function
End Module
```

Esse código exibe:

```
Early-bound: xy
Late-bound: yx
```

Como a resolução de sobrecarga com associação tardia é feita no tipo dos argumentos de tempo de execução, é possível que uma expressão pode produzir resultados diferentes com base em se ele será avaliado no tempo de execução ou tempo de compilação. O exemplo a seguir ilustra essa diferença:

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(b As Base)
        Console.WriteLine("F(Base)")
    End Sub

    Sub F(d As Derived)
        Console.WriteLine("F(Derived)")
    End Sub

    Sub Main()
        Dim b As Base = New Derived()
        Dim o As Object = b

        F(b)
        F(o)
    End Sub
End Module
```

Esse código exibe:

```
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a>Expressões simples

Expressões simples são literais, expressões entre parênteses, expressões de instância ou expressões de nome simples.

```antlr
SimpleExpression
    : LiteralExpression
    | ParenthesizedExpression
    | InstanceExpression
    | SimpleNameExpression
    | AddressOfExpression
    ;
```

### <a name="literal-expressions"></a>Expressões literais

Avaliam expressões literais para o valor representado pelo literal. Uma expressão literal é classificada como um valor, exceto para o literal `Nothing`, que é classificado como um valor padrão.

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a>Expressões entre parênteses

Uma expressão entre parênteses consiste em uma expressão entre parênteses. Uma expressão entre parênteses é classificada como um valor e a expressão anexada deve ser classificada como um valor. Uma expressão entre parênteses é avaliada como o valor da expressão entre parênteses.

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a>Expressões de instância

Uma *instância expressão* é a palavra-chave `Me`. Ele só pode ser usado dentro do corpo de um acessador de propriedade, construtor ou método não compartilhado. Ele é classificado como um valor. A palavra-chave `Me` representa a instância do tipo que contém o acessador de método ou propriedade que está sendo executado. Se um construtor chama explicitamente o outro construtor (seção [construtores](type-members.md#constructors)), `Me` não pode ser usado até que essa chamada de construtor, porque a instância ainda não foi construída.

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a>Expressões de nome simples

Um *expressão de nome simples* consiste em um único identificador seguido por uma lista de argumentos de tipo opcionais.

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

O nome é resolvido e classificado por "nome resolução regras simples":

1.  Começando com imediatamente delimitador bloquear e continuando com cada delimitador bloco externo (se houver), se o identificador corresponder ao nome de uma variável local, a variável estática, a constante local, método de parâmetro ou parâmetro de tipo, em seguida, o identificador se refere à Entidade correspondente.

    Se o identificador corresponde a uma variável local, uma variável estática ou uma constante local e uma lista de argumentos de tipo foi fornecida, ocorrerá um erro de tempo de compilação. Se o identificador corresponde a um parâmetro de tipo de método e uma lista de argumentos de tipo foi fornecida, nenhuma correspondência ocorrer e continua a resolução. Se o identificador corresponder a uma variável local, a variável local correspondida é a função implícita ou `Get` variável local de retorno do acessador e a expressão fizer parte de uma expressão de invocação, instrução de chamada, ou um `AddressOf` expressão, em seguida, Nenhuma correspondência ocorrer e continua de resolução.

    A expressão é classificada como uma variável, se for uma variável local, uma variável estática ou um parâmetro. A expressão é classificada como um tipo se ele for um parâmetro de tipo de método. A expressão é classificada como um valor se for um local constante.

2.  Para cada tipo aninhado que contém a expressão, desde o mais interno e vai mais externo, se uma pesquisa do identificador no tipo gera uma correspondência com um membro acessível:

    21. Se o membro de tipo correspondente for um parâmetro de tipo, o resultado é classificado como um tipo e é o parâmetro de tipo correspondente. Se uma lista de argumentos de tipo foi fornecida, nenhuma correspondência ocorrer e continua de resolução.
    22. Caso contrário, se o tipo é o tipo delimitador imediatamente e a pesquisa identifica um membro de tipo não compartilhada, em seguida, o resultado é o mesmo que um acesso de membro do formulário `Me.E(Of A)`, onde `E` é o identificador e `A` é a lista de argumentos de tipo , se houver.
    23. Caso contrário, o resultado é exatamente o mesmo que um acesso de membro do formulário `T.E(Of A)`, onde `T` é o tipo que contém o membro correspondente, `E` é o identificador e `A` é a lista de argumentos de tipo, se houver. Nesse caso, ele é um erro para o identificador para se referir a um membro não compartilhado.

3.  Para cada namespace aninhado, começando em mais interna e vai para o namespace mais externo, faça o seguinte:

    31. Se o namespace contém um tipo acessível com o nome fornecido e tem o mesmo número de parâmetros de tipo, conforme foi fornecido na lista de argumentos de tipo, se houver, em seguida, o identificador se refere a esse tipo e é classificado como um tipo.
    32. Caso contrário, se nenhum tipo de lista de argumentos foi fornecido e o namespace contém um membro de namespace com o nome fornecido, em seguida, o identificador refere-se ao namespace e é classificado como um namespace.
    33. Caso contrário, se o namespace contém um ou mais módulos padrão acessíveis e uma pesquisa de nome do identificador de membro produz uma correspondência acessível em exatamente um módulo padrão, em seguida, o resultado é exatamente o mesmo que um acesso de membro do formulário `M.E(Of A)`, em que `M` é o módulo padrão que contém o membro correspondente, `E` é o identificador e `A` é a lista de argumentos de tipo, se houver. Se o identificador corresponder a membros de tipo acessível em mais de um módulo padrão, ocorrerá um erro de tempo de compilação.

4.  Se o arquivo de origem tem um ou mais aliases de importação, e o identificador corresponde ao nome de um deles, o identificador se refere a esse namespace ou tipo. Se uma lista de argumentos de tipo for fornecida, ocorrerá um erro de tempo de compilação.

5. Se o arquivo de origem que contém a referência de nome tem um ou mais importações:

    51. Se o identificador corresponder exatamente em um importe o nome de um tipo acessível com o mesmo número de parâmetros de tipo, conforme foi fornecido na lista de argumentos de tipo, se houver, ou um membro de tipo, em seguida, o identificador se refere a esse tipo ou membro de tipo. Se o identificador corresponde em mais de uma importação ao nome de um tipo acessível com o mesmo número de parâmetros de tipo foi fornecido na lista de argumentos de tipo, se houver, ou um membro de tipo acessível, um erro de tempo de compilação ocorre.
    52. Caso contrário, se nenhum tipo de lista de argumentos foi fornecido e o identificador corresponde na importação de exatamente um nome de um namespace com tipos acessíveis, em seguida, o identificador se refere esse namespace. Se nenhum tipo de lista de argumentos foi fornecido e o identificador corresponde em mais de uma importação, o nome de um namespace com tipos acessíveis, ocorrerá um erro de tempo de compilação.
    53. Caso contrário, se as importações contêm um ou mais módulos padrão acessíveis e uma pesquisa de nome do identificador de membro produz uma correspondência acessível em exatamente um módulo padrão, em seguida, o resultado é exatamente o mesmo que um acesso de membro do formulário `M.E(Of A)`, onde `M` é o módulo padrão que contém o membro correspondente, `E` é o identificador e `A` é a lista de argumentos de tipo, se houver. Se o identificador corresponder a membros de tipo acessível em mais de um módulo padrão, ocorrerá um erro de tempo de compilação.

6.  Se o ambiente de compilação define um ou mais aliases de importação, e o identificador corresponde ao nome de um deles, o identificador se refere a esse namespace ou tipo. Se uma lista de argumentos de tipo for fornecida, ocorrerá um erro de tempo de compilação.

7. Se o ambiente de compilação define uma ou mais importações:

    71. Se o identificador corresponder exatamente em um importe o nome de um tipo acessível com o mesmo número de parâmetros de tipo, conforme foi fornecido na lista de argumentos de tipo, se houver, ou um membro de tipo, em seguida, o identificador se refere a esse tipo ou membro de tipo. Se o identificador corresponde em mais de uma importação ao nome de um tipo acessível com o mesmo número de parâmetros de tipo foi fornecido na lista de argumentos de tipo, se houver, ou um membro de tipo, um erro de tempo de compilação ocorre.
    72. Caso contrário, se nenhum tipo de lista de argumentos foi fornecido e o identificador corresponde na importação de exatamente um nome de um namespace com tipos acessíveis, em seguida, o identificador se refere esse namespace. Se nenhum tipo de lista de argumentos foi fornecido e o identificador corresponde em mais de uma importação, o nome de um namespace com tipos acessíveis, ocorrerá um erro de tempo de compilação.
    73. Caso contrário, se as importações contêm um ou mais módulos padrão acessíveis e uma pesquisa de nome do identificador de membro produz uma correspondência acessível em exatamente um módulo padrão, em seguida, o resultado é exatamente o mesmo que um acesso de membro do formulário `M.E(Of A)`, onde `M` é o módulo padrão que contém o membro correspondente, `E` é o identificador e `A` é a lista de argumentos de tipo, se houver. Se o identificador corresponder a membros de tipo acessível em mais de um módulo padrão, ocorrerá um erro de tempo de compilação.

8. Caso contrário, o nome fornecido pelo identificador é indefinido.

Uma expressão de nome simples que não está definida é um erro de tempo de compilação.

Normalmente, um nome pode ocorrer apenas uma vez em um namespace específico. No entanto, como namespaces podem ser declarados em vários assemblies do .NET, é possível ter uma situação em que dois assemblies definem um tipo com o mesmo nome totalmente qualificado. Nesse caso, um tipo declarado no conjunto atual de arquivos de origem é preferível ao longo de um tipo declarado em um assembly .NET externo. Caso contrário, o nome é ambíguo e não há nenhuma maneira de resolver a ambiguidade do nome.


### <a name="addressof-expressions"></a>Expressões AddressOf

Um `AddressOf` expressão é usada para produzir um ponteiro de método. A expressão consiste o `AddressOf` palavra-chave e uma expressão que deve ser classificada como um grupo de método ou um acesso de associação tardia. O grupo de método não pode se referir a construtores.

O resultado é classificado como um ponteiro de método, com a mesma expressão de destino associados e o tipo lista de argumentos (se houver) como o grupo de método.

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a>Expressões de tipo

Um *digite a expressão* é um `GetType` expressão, uma `TypeOf...Is` expressão, uma `Is` expressão, ou um `GetXmlNamespace` expressão.

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a>Expressões de GetType

Um `GetType` expressão consiste na palavra-chave `GetType` e o nome de um tipo.

```antlr
GetTypeExpression
    : 'GetType' OpenParenthesis GetTypeTypeName CloseParenthesis
    ;

GetTypeTypeName
    : TypeName
    | QualifiedOpenTypeName
    ;

QualifiedOpenTypeName
    : Identifier TypeArityList? (Period IdentifierOrKeyword TypeArityList?)*
    | 'Global' Period IdentifierOrKeyword TypeArityList?
      (Period IdentifierOrKeyword TypeArityList?)*
    ;

TypeArityList
    : OpenParenthesis 'Of' CommaList? CloseParenthesis
    ;

CommaList
    : Comma Comma*
    ;
```

Um `GetType` expressão é classificada como um valor, e seu valor é a reflexão (`System.Type`) a classe que representa seu *GetTypeTypeName*. Se o *GetTypeTypeName* é um parâmetro de tipo, a expressão retornará o `System.Type` objeto que corresponde ao argumento de tipo fornecido para o parâmetro de tipo em tempo de execução.

O *GetTypeTypeName* é especial de duas maneiras:

* Ele tem permissão para ser `System.Void`, o único lugar no idioma em que esse nome de tipo pode ser referenciado.

* Pode ser um tipo genérico construído com os argumentos de tipo omitido. Isso permite que o `GetType` expressão para retornar o `System.Type` objeto que corresponde ao tipo genérico em si.

O exemplo a seguir demonstra o `GetType` expressão:

```vb
Module Test
    Sub Main()
        Dim t As Type() = { GetType(Integer), GetType(System.Int32), _
            GetType(String), GetType(Double()) }
        Dim i As Integer

        For i = 0 To t.Length - 1
            Console.WriteLine(t(i).Name)
        Next i
    End Sub
End Module
```

A saída resultante é:

```
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a>TypeOf... Expressões

Um `TypeOf...Is` expressão é usada para verificar se o tipo de tempo de execução de um valor é compatível com um determinado tipo. O primeiro operando deve ser classificado como um valor, não pode ser um método lambda reclassificados e deve ser de um tipo de referência ou um tipo de parâmetro de tipo sem restrição. O segundo operando deve ser um nome de tipo. O resultado da expressão é classificado como um valor e um `Boolean` valor. A expressão é avaliada como `True` se o tipo de tempo de execução do operando tiver uma identidade, padrão, referência, matriz, tipo de valor ou conversão de parâmetro de tipo para o tipo `False` caso contrário. Ocorrerá um erro de tempo de compilação se não existir nenhuma conversão entre o tipo da expressão e o tipo específico.

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a>Expressões

Uma `Is` ou `IsNot` expressão é usada para fazer uma comparação de igualdade de referência.

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

Cada expressão deve ser classificada como um valor e o tipo de cada expressão deve ser um tipo de referência, um tipo de parâmetro de tipo sem restrição ou um tipo de valor anulável. Se o tipo de uma expressão for um tipo de parâmetro de tipo sem restrição ou um tipo de valor anulável, no entanto, a outra expressão deve ser o literal `Nothing`.

O resultado é classificado como um valor e é digitado como `Boolean`. Uma `Is` operação é avaliada como `True` se ambos os valores se referem à mesma instância ou os dois valores forem `Nothing`, ou `False` caso contrário. Uma `IsNot` operação é avaliada como `False` se ambos os valores se referem à mesma instância ou os dois valores forem `Nothing`, ou `True` caso contrário.


### <a name="getxmlnamespace-expressions"></a>Expressões de GetXmlNamespace

Um `GetXmlNamespace` expressão consiste na palavra-chave `GetXmlNamespace` e o nome de um namespace XML declarado pelo ambiente de compilação ou de arquivo de origem.

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

Uma `GetXmlNamespace` expressão é classificada como um valor, e seu valor é uma instância do `System.Xml.Linq.XNamespace` que representa o *XMLNamespaceName*. Se esse tipo não estiver disponível, em seguida, ocorrerá um erro de tempo de compilação.

Por exemplo:

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim db = GetXmlNamespace(db)

        ' The following are equivalent
        Dim customer1 = _
            New System.Xml.Linq.XElement(db + "customer", "Bob")
        Dim customer2 = <db:customer>Bob</>
    End Sub
End Module
```

Tudo entre os parênteses é considerado parte do nome do namespace, portanto, se aplicam as regras de XML em torno de coisas como espaços em branco. Por exemplo:

```vb
Imports <xmlns:db-ns="http://example.org/database">

Module Test
    Sub Main()

        ' Error, XML name expected
        Dim db1 = GetXmlNamespace( db-ns )

        ' Error, ')' expected
        Dim db2 = GetXmlNamespace(db _
            )

        ' OK.
        Dim db3 = GetXmlNamespace(db-ns)
    End Sub
End Module
```

A expressão de namespace XML também pode ser omitida, caso em que a expressão retorna o objeto que representa o namespace XML padrão.


## <a name="member-access-expressions"></a>Expressões de acesso de membro

Uma expressão de acesso de membro é usada para acessar um membro de uma entidade.

```antlr
MemberAccessExpression
    : MemberAccessBase? Period IdentifierOrKeyword
      ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;

MemberAccessBase
    : Expression
    | NonArrayTypeName
    | 'Global'
    | 'MyClass'
    | 'MyBase'
    ;
```

Um acesso de membro do formulário `E.I(Of A)`, onde `E` é uma expressão, um nome de tipo não matriz, a palavra-chave `Global`, ou não especificado e `I` é um identificador com uma lista de argumentos de tipo opcional `A`é avaliado e classificado da seguinte maneira:

1. Se `E` for omitido, em seguida, a expressão imediatamente contenha `With` instrução seja substituída por `E` e o acesso de membro é executado. Se não houver nenhum contendo `With` declaração, um erro de tempo de compilação ocorre.

2. Se `E` é classificado como um namespace ou `E` é a palavra-chave `Global`, em seguida, a pesquisa de membro é feita no contexto de namespace especificado. Se `I` é o nome de um membro acessível do namespace com o mesmo número de parâmetros de tipo como foi fornecido na lista de argumentos de tipo, se houver, em seguida, o resultado é que o membro. O resultado é classificado como um namespace ou um tipo, dependendo do membro. Caso contrário, ocorrerá um erro de tempo de compilação.

3. Se `E` é um tipo ou uma expressão classificado como um tipo, em seguida, a pesquisa de membro é feita no contexto do tipo especificado. Se `I` é o nome de um membro acessível do `E`, em seguida, `E.I` é avaliado e classificados da seguinte maneira:

    31. Se `I` é a palavra-chave `New` e `E` não é uma enumeração, em seguida, ocorre um erro de tempo de compilação.
    32. Se `I` identifica um tipo com o mesmo número de parâmetros de tipo, como foi fornecido na lista de argumentos de tipo, se houver, em seguida, o resultado será aquele tipo.
    33. Se `I` identifica um ou mais métodos, o resultado será um grupo de métodos com lista de argumentos de tipo associado e nenhuma expressão de destino associados.
    34. Se `I` identifica uma ou mais propriedades e nenhum tipo de lista de argumentos foi fornecida e, em seguida, o resultado é um grupo de propriedades sem expressão de destino associados.
    35. Se `I` identifica uma variável compartilhada e nenhum tipo de lista de argumentos foi fornecida e, em seguida, o resultado é uma variável ou um valor. Se a variável é somente leitura e a referência ocorre fora do construtor compartilhado do tipo no qual a variável é declarada, o resultado é o valor da variável compartilhada `I` em `E`. Caso contrário, o resultado é a variável compartilhada `I` em `E`.
    36. Se `I` identifica um evento compartilhado e nenhum tipo de lista de argumentos foi fornecida, o resultado é um evento de acesso sem expressão de destino associados.
    37. Se `I` identifica uma constante e nenhum tipo de lista de argumentos foi fornecida e, em seguida, o resultado é o valor dessa constante.
    38. Se `I` identifica um membro de enumeração e nenhum tipo de lista de argumentos foi fornecida e, em seguida, o resultado é o valor desse membro de enumeração.
    39. Caso contrário, `E.I` é uma referência de membro inválido e ocorre um erro de tempo de compilação.

4. Se `E` é classificado como uma variável ou um valor, o tipo do qual está `T`, em seguida, a pesquisa de membro é feita no contexto de `T`. Se `I` é o nome de um membro acessível do `T`, em seguida, `E.I` é avaliado e classificados da seguinte maneira:

    41. Se `I` é a palavra-chave `New`, `E` é `Me`, `MyBase`, ou `MyClass`e foi fornecido nenhum argumento de tipo e, em seguida, o resultado é um grupo de métodos que representam os construtores de instância do tipo de `E`com uma expressão de destino associados do `E` e nenhuma lista de argumentos de tipo. Caso contrário, ocorrerá um erro de tempo de compilação.
    42. Se `I` identifica um ou mais métodos, incluindo métodos de extensão se `T` não está `Object`, em seguida, o resultado é um grupo de métodos com lista de argumentos de tipo associado e uma expressão de destino associados do `E`.
    43. Se `I` identifica uma ou mais propriedades e nenhum tipo de argumentos foram fornecidos, o resultado será um grupo de propriedades com uma expressão de destino associados do `E`.
    44. Se `I` identifica uma variável compartilhada ou uma variável de instância e nenhum tipo de argumentos foram fornecidos, em seguida, o resultado é uma variável ou um valor. Se a variável é somente leitura e a referência ocorre fora de um construtor da classe na qual a variável é declarada apropriada para o tipo de variável (compartilhado ou de instância), o resultado é o valor da variável `I` no objeto referenciado por `E`. Se `T` é um tipo de referência, em seguida, o resultado é a variável `I` no objeto referenciado pelo `E`. Caso contrário, se `T` é um tipo de valor e a expressão `E` é classificado como uma variável, o resultado é uma variável; caso contrário, o resultado é um valor.
    45. Se `I` identifica um evento e nenhum tipo de argumentos foram fornecidos, o resultado é um evento de acesso com uma expressão de destino associados do `E`.
    46. Se `I` identifica uma constante e nenhum tipo de argumentos foram fornecidos, o resultado será o valor dessa constante.
    47. Se `I` identifica um membro de enumeração e nenhum tipo de argumentos foram fornecidos, o resultado será o valor desse membro de enumeração.
    48. Se `T` está `Object`, em seguida, o resultado é uma pesquisa de membro de associação tardia classificada como um acesso de associação tardia com a lista de argumentos de tipo associado e uma expressão de destino associados do `E`.

5. Caso contrário, `E.I` é uma referência de membro inválido e ocorre um erro de tempo de compilação.

Um acesso de membro do formulário `MyClass.I(Of A)` é equivalente a `Me.I(Of A)`, mas todos os membros acessados nele são tratados como se os membros são não pode ser substituído. Portanto, o membro acessado não será afetado pelo tipo de tempo de execução do valor no qual o membro está sendo acessado.

Um acesso de membro do formulário `MyBase.I(Of A)` é equivalente a `CType(Me, T).I(Of A)` onde `T` é o tipo base direto do tipo que contém a expressão de acesso de membro. Todas as invocações de método nele são tratadas como se o método que está sendo invocado é não pode ser substituído. Essa forma de acesso de membro também é chamada de um *acesso de base*.

O exemplo a seguir demonstra como `Me`, `MyBase` e `MyClass` relacionados:

```vb
Class Base
    Public Overridable Sub F()
        Console.WriteLine("Base.F")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Overrides Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Sub G()
        MyClass.F()
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Overrides Sub F()
        Console.WriteLine("MoreDerived.F")
    End Sub

    Public Sub H()
        MyBase.F()
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = new MoreDerived()

        x.F()
        x.G()
        x.H()
    End Sub

End Module
```

Esse código imprime:

```
MoreDerived.F
Derived.F
Derived.F
```

Quando uma expressão de acesso de membro começa com a palavra-chave `Global`, a palavra-chave representa o namespace sem nome externo, que é útil em situações em que uma declaração se sobrepõe a um namespace delimitador. O `Global` permite a palavra-chave "escape" out para o namespace mais externo nessa situação. Por exemplo:

```vb
Class System
End Class

Module Test
    Sub Main()
        ' Error: Class System does not contain Console
        System.Console.WriteLine("Hello, world!") 


        ' Legal, binds to System in outermost namespace
        Global.System.Console.WriteLine("Hello, world!") 
    End Sub
End Module
```

No exemplo acima, a primeira chamada de método é inválida porque o identificador `System` associa à classe `System`, não o namespace `System`. A única maneira de acessar o `System` namespace é usar `Global` pulem-out para o namespace mais externo.

Se o membro que está sendo acessado for compartilhado, qualquer expressão no lado esquerdo do período é supérfluo e não é avaliada, a menos que o acesso de membro é feito associação tardia. Por exemplo, considere o seguinte código:

```vb
Class C
    Public Shared F As Integer = 10
End Class

Module Test
    Public Function ReturnC() As C
        Console.WriteLine("Returning a new instance of C.")
        Return New C()
    End Function

    Public Sub Main()
        Console.WriteLine("The value of F is: " & ReturnC().F)
    End Sub
End Module
```

Ele imprime `The value of F is: 10` porque a função `ReturnC` não precisa ser chamado para fornecer uma instância do `C` para acessar o membro compartilhado `F`.


### <a name="identical-type-and-member-names"></a>Tipo idêntico e nomes de membro

Não é incomum para os membros de nome usando o mesmo nome como seu tipo. Nessa situação, no entanto, ocultação de nome inconveniente pode ocorrer:

```vb
Enum Color
    Red
    Green
    Yellow
End Enum

Class Test
    ReadOnly Property Color() As Color
        Get
            Return Color.Red
        End Get
    End Property

    Shared Function DefaultColor() As Color
        Return Color.Green    ' Binds to the instance property!
    End Function
End Class
```

No exemplo anterior, o nome simples `Color` em `DefaultColor` associado à propriedade de instância em vez do tipo. Como um membro de instância não pode ser referenciado em um membro compartilhado, isso normalmente seria um erro.

No entanto, uma regra especial permite o acesso ao tipo nesse caso. Se a expressão de base de uma expressão de acesso de membro é um nome simples e associa a uma constante, campo, propriedade, variável local ou parâmetro cujo tipo tem o mesmo nome, a expressão de base pode referir-se para o membro ou tipo. Isso nunca pode resultar em ambiguidade porque os membros que podem ser acessados fora deles são os mesmos.

### <a name="default-instances"></a>Instâncias padrão

Em algumas situações, classes derivadas de uma comum geralmente de classe de base ou sempre tem apenas uma única instância. Por exemplo, a maioria dos windows mostrados em uma interface do usuário só tem uma instância que mostrando na tela a qualquer momento. Para simplificar o trabalho com esses tipos de classes, Visual Basic pode gerar automaticamente *instâncias padrão* das classes que fornecem uma instância única e facilmente referenciada para cada classe.

Instâncias padrão são sempre criadas para um *família* de tipos em vez de para um tipo específico. Portanto, em vez de criar uma instância padrão para uma classe Form1 que deriva de Form, instâncias padrão são criadas para todas as classes derivadas do formulário. Isso significa que cada classe individual que é derivada da classe base não precisa ser especialmente marcados para ter uma instância padrão.

A instância padrão de uma classe é representada por uma propriedade gerada pelo compilador que retorna a instância padrão dessa classe. A propriedade gerada como um membro de uma classe chamada a *classe grupo* que gerencia a alocação e a destruição de instâncias padrão para todas as classes derivadas da classe base específica. Por exemplo, todas as propriedades de instância padrão de classes derivados de `Form` podem ser coletadas no `MyForms` classe. Se uma instância da classe de grupo é retornada pela expressão `My.Forms`, em seguida, o código a seguir acessa as instâncias padrão de classes derivadas `Form1` e `Form2`:

```vb
Class Form1
    Inherits Form
    Public x As Integer
End Class

Class Form2
    Inherits Form
    Public y As Integer
End Class

Module Main
    Sub Main()
        My.Forms.Form1.x = 10
        Console.WriteLine(My.Forms.Form2.y)
    End Sub
End Module
```

Instâncias padrão não serão criadas até que a primeira referência a eles; buscando a propriedade que representa a instância padrão faz com que a instância padrão a ser criado se ele já não tiver sido criado ou foi definido como `Nothing`. Para testar a existência de uma instância padrão, quando uma instância padrão é o destino de uma `Is` ou `IsNot` operador, a instância padrão não será criada. Portanto, é possível testar se uma instância padrão é `Nothing` ou alguma outra referência sem fazer com que a instância padrão a ser criado.

Instâncias padrão destinam-se tornar mais fácil referir-se à instância padrão de fora da classe que tem a instância padrão. Usando uma instância padrão de dentro de uma classe que define que ele pode causar uma confusão sobre qual instância está sendo referenciada, ou seja, a instância padrão ou a instância atual. Por exemplo, o código a seguir modifica apenas o valor `x` na instância padrão, mesmo que ele está sendo chamado de outra instância. Portanto, o código seria imprimir o valor `5` em vez de `10`:

```vb
Class Form1
    Inherits Form

    Public x As Integer = 5

    Public Sub ChangeX()
        Form1.x = 10
    End Sub
End Class

Module Main
    Sub Main()
        Dim f As Form1 = New Form1()
        f.ChangeX()
        Console.WriteLine(f.x)
    End Sub
End Module
```

Para evitar esse tipo de confusão, não é válido para se referir a uma instância padrão de dentro de um método de instância do tipo da instância padrão.

#### <a name="default-instances-and-type-names"></a>Instâncias padrão e nomes de tipo

Uma instância padrão também possa ser acessada diretamente por meio do nome do seu tipo. Nesse caso, em qualquer contexto de expressão em que o nome do tipo não é permitido a expressão `E`, onde `E` representa o nome totalmente qualificado da classe com uma instância padrão, é alterado para `E'`, onde `E'` representa uma expressão que a propriedade de instância padrão de busca. Por exemplo, se o padrão de instâncias de classes derivadas de `Form` permitir o acesso a instância padrão pelo nome do tipo, em seguida, o código a seguir é equivalente ao código no exemplo anterior:

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

Isso também significa que uma instância padrão que é acessível por meio do nome do seu tipo também pode ser atribuída pelo nome do tipo. Por exemplo, o código a seguir define a instância padrão do `Form1` para `Nothing`:

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

Observe que o significado dos `E.I` foram `E` representa uma classe e `I` representa um membro compartilhado não é alterado. Essa expressão ainda acessa o membro compartilhado diretamente da instância da classe e não faz referência a instância padrão.

#### <a name="group-classes"></a>Classes de grupo

O `Microsoft.VisualBasic.MyGroupCollectionAttribute` atributo indica a classe de grupo para uma família de instâncias padrão. O atributo tem quatro parâmetros:

* O parâmetro `TypeToCollect` Especifica a classe base para o grupo. Todas as classes podem ser instanciadas sem parâmetros de tipo aberto que derivam de um tipo com esse nome (independentemente de parâmetros de tipo) terão automaticamente uma instância padrão.

* O parâmetro `CreateInstanceMethodName` Especifica o método a ser chamado na classe de grupo para criar uma nova instância em uma propriedade de instância padrão.

* O parâmetro `DisposeInstanceMethodName` Especifica o método a ser chamado na classe de grupo de descarte de uma propriedade de instância padrão, se a propriedade de instância padrão é atribuída o valor `Nothing`.

* O parâmetro `DefaultInstanceAlias` especificidades a expressão `E'` para substituir o nome de classe se as instâncias padrão são acessíveis diretamente por meio de seu nome de tipo. Se esse parâmetro for `Nothing` ou uma cadeia de caracteres vazia, instâncias padrão em que esse tipo de grupo não são acessíveis diretamente por meio do nome do seu tipo. (__Observação.__ Em todas as implementações atuais da linguagem Visual Basic, o `DefaultInstanceAlias` parâmetro é ignorado, exceto no código fornecido pelo compilador.)

Vários tipos podem ser coletados no mesmo grupo, separando os nomes dos tipos e métodos nos três primeiros parâmetros usando vírgulas. Deve haver o mesmo número de itens em cada parâmetro e os elementos de lista são correspondidos na ordem. Por exemplo, a seguinte declaração de atributo coleta os tipos que derivam de `C1`, `C2` ou `C3` em um único grupo:

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

A assinatura do método create deve estar no formato `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`. O método dispose deve estar no formato `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`. Assim, a classe de grupo para o exemplo na seção anterior poderia ser declarada da seguinte maneira:

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub
End Class
```

Se um arquivo de origem declarado uma classe derivada `Form1`, a classe gerada grupo seria equivalente a:

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub

    Private m_Form1 As Form1

    Public Property Form1() As Form1
        Get
            Return Create(m_Form1)
        End Get
        Set (Value As Form1)
            If Value IsNot Nothing AndAlso Value IsNot m_Form1 Then
                Throw New ArgumentException( _
                    "Property can only be set to Nothing.")
            End If
            Dispose(m_Form1)
        End Set
    End Property
End Class
```

### <a name="extension-method-collection"></a>Coleção de método de extensão

Expressão de acesso a métodos de extensão para o membro `E.I` são coletados, reunindo todos os métodos de extensão com o nome `I` que estão disponíveis no contexto atual:

1. Em primeiro lugar, cada tipo aninhado que contém a expressão é verificado, começando em mais interna e vai mais externo.
2. Em seguida, cada namespace aninhado é verificado, começando em mais interna e vai para o namespace mais externo.
3. Em seguida, as importações no arquivo de origem são verificadas.
4. Em seguida, as importações definidas pelo ambiente de compilação são verificadas.

Um método de extensão é coletado somente se houver uma nativa conversão de ampliação do tipo de expressão de destino para o tipo do primeiro parâmetro do método de extensão. E, ao contrário de associação de expressão regular nome simples, a pesquisa coleta *todos os* métodos de extensão; a coleção não é interrompida quando um método de extensão for encontrado. Por exemplo:

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class


Namespace N1
    Module N1C1Extensions
        <Extension> _
        Sub M1(c As C1, x As Integer)
        End Sub
    End Module
End Namespace

Namespace N1.N2
    Module N2C1Extensions
        <Extension> _
        Sub M1(c As C1, y As Double)
        End Sub
    End Module
End Namespace

Namespace N1.N2.N3
    Module Test
        Sub Main()
            Dim x As New C1()

            ' Calls N1C1Extensions.M1
            x.M1(10)
        End Sub
    End Module
End Namespace
```

Neste exemplo, embora `N2C1Extensions.M1` for encontrado antes `N1C1Extensions.M1`, ambas são consideradas como métodos de extensão. Depois que todos os métodos de extensão forem coletados, eles são então *via currying*. Currying usa o destino da chamada do método de extensão e aplica-se a chamada do método de extensão, resultando em uma nova assinatura de método com o primeiro parâmetro removido (porque ele foi especificado). Por exemplo:

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(x As Integer, y As Integer)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(x As Integer, y As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(y As Integer)
        '        Ext2.M(y As Double)
        v.M(10)
    End Sub
End Module
```

No exemplo acima, o resultado na forma curried da aplicação `v` à `Ext1.M` é a assinatura do método `Sub M(y As Integer)`.

Além de remover o primeiro parâmetro do método de extensão, currying também remove quaisquer parâmetros de tipo de método que fazem parte do tipo do primeiro parâmetro. Quando currying um método de extensão com o parâmetro de tipo de método, a inferência de tipo é aplicada ao primeiro parâmetro e o resultado é fixa para quaisquer parâmetros de tipo são inferidos. Se a inferência de tipo falhar, o método é ignorado. Por exemplo:

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(Of T, U)(x As T, y As U)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(Of T)(x As T, y As T)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(Of U)(y As U)
        '        Ext2.M(y As Integer)
        v.M(10)
    End Sub
End Module
```

No exemplo acima, o resultado na forma curried da aplicação `v` à `Ext1.M` é a assinatura de método `Sub M(Of U)(y As U)`, porque o parâmetro de tipo `T` é inferido como resultado o currying e agora foi corrigido. Porque o parâmetro de tipo `U` não foi inferida como parte do currying, ele permanecerá um parâmetro de abertura. Da mesma forma, porque o parâmetro de tipo `T` é inferido como resultado da aplicação `v` à `Ext2.M`, o tipo do parâmetro `y` se torna fixo como `Integer`. Ele não será inferido para ser de qualquer outro tipo. Quando currying a assinatura, todas as restrições, exceto para `New` restrições também são aplicadas. Se as restrições não são atendidas ou dependem de um tipo que não foi inferido como parte do currying, o método de extensão é ignorado. Por exemplo:

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M1(Of T As Structure)(x As T, y As Integer)
    End Sub

    <Extension> _
    Sub M2(Of T As U, U)(x As T, y As U)
    End Sub
End Module

Module Main
    Sub Test()
        Dim s As String = "abc"

        ' Error: String does not satisfy the Structure constraint
        s.M1(10)

        ' Error: T depends on U, which cannot be inferred
        s.M2(10)
    End Sub
End Module
```

__Observação.__ Uma das principais razões para fazer currying dos métodos de extensão é que ele permite que as expressões de consulta inferir o tipo da iteração antes de avaliar os argumentos para um método de padrão de consulta. Como a maioria dos métodos padrão de consulta demorar expressões lambda, que exigem inferência de tipos próprios, isso simplifica bastante o processo de avaliar uma expressão de consulta.

Ao contrário de herança da interface normal, os métodos de extensão que estendem as duas interfaces que não estão relacionados uns aos outros estão disponíveis, desde que eles não têm a mesma assinatura na forma curried:

```vb
Imports System.Runtime.CompilerServices

Interface I1
End Interface

Interface I2
End Interface

Class C1
    Implements I1, I2
End Class

Module I1Ext
    <Extension> _
    Sub M1(i As I1, x As Integer)
    End Sub

    <Extension> _
    Sub M2(i As I1, x As Integer)
    End Sub
End Module

Module I2Ext
    <Extension> _
    Sub M1(i As I2, x As Integer)
    End Sub

    <Extension> _
    Sub M2(I As I2, x As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim c As New C1()

        ' Error: M is ambiguous between I1Ext.M1 and I2Ext.M1.
        c.M1(10)

        ' Calls I1Ext.M2
        c.M2(10)
    End Sub
End Module
```

Por fim, é importante lembrar-se de que métodos não são considerados ao fazer a associação tardia de extensão:

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a>Expressões de acesso de membro de dicionário

Um *expressão de acesso de membro de dicionário* é usada para pesquisar um membro de uma coleção. Um acesso de membro de dicionário assume a forma de `E!I`, onde `E` é uma expressão que é classificada como um valor e `I` é um identificador.

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

O tipo da expressão deve ter uma propriedade default indexada por um único `String` parâmetro. A expressão de acesso de membro de dicionário `E!I` é transformado em expressão `E.D("I")`, onde `D` é a propriedade padrão do `E`. Por exemplo:

```vb
Class Keys
    Public ReadOnly Default Property Item(s As String) As Integer
        Get
            Return 10
        End Get
    End Property 
End Class

Module Test
    Sub Main()
        Dim x As Keys = new Keys()
        Dim y As Integer
        ' The two statements are equivalent.
        y = x!abc
        y = x("abc")
    End Sub
End Module
```

Se um ponto de exclamação for especificado com nenhuma expressão, ela contenha imediatamente `With` instrução será assumida. Se não houver nenhum contendo `With` declaração, um erro de tempo de compilação ocorre.


## <a name="invocation-expressions"></a>Expressões de Invocação

Uma expressão de invocação consiste em um destino de invocação e uma lista de argumentos opcionais.

```antlr
InvocationExpression
    : Expression ( OpenParenthesis ArgumentList? CloseParenthesis )?
    ;

ArgumentList
    : PositionalArgumentList
    | PositionalArgumentList Comma NamedArgumentList
    | NamedArgumentList
    ;

PositionalArgumentList
    : Expression? ( Comma Expression? )*
    ;

NamedArgumentList
    : IdentifierOrKeyword ColonEquals Expression
      ( Comma IdentifierOrKeyword ColonEquals Expression )*
    ;
```

A expressão de destino deve ser classificada como um grupo de método ou um valor cujo tipo é um tipo delegado. Se a expressão de destino for um valor cujo tipo é um tipo de delegado, o destino da expressão de invocação se torna o grupo de método para o `Invoke` membro do tipo delegado e a expressão de destino torna-se a expressão de destino associados do método grupo.

Uma lista de argumentos tem duas seções: argumentos posicionais e argumentos nomeados. *Argumentos posicionais* são expressões e devem preceder argumentos nomeados. *Argumentos nomeados* começar com um identificador que pode corresponder a palavras-chave, seguidas por `:=` e uma expressão.

Se o grupo de método contém apenas um método acessível, incluindo métodos de instância e a extensão e esse método não leva argumentos e é uma função, em seguida, o grupo de métodos é interpretado como uma expressão de invocação com uma lista de argumentos vazia e o resultado é usado como o destino de uma expressão de invocação com as listas de argumento fornecido. Por exemplo:

```vb
Class C1
    Function M1() As Integer()
        Return New Integer() { 1, 2, 3 }
    End Sub
End Class

Module Test
    Sub Main()
        Dim c As New C1()

        ' Prints 3
        Console.WriteLine(c.M1(2))
    End Sub
End Module
```

Caso contrário, a resolução de sobrecarga é aplicada aos métodos para escolher o método mais aplicável para a lista de argumento fornecido (s). Se o método mais aplicável é uma função, em seguida, o resultado da expressão de invocação é classificado como um valor tipado como o tipo de retorno da função. Se o método mais aplicável é uma sub-rotina, o resultado é classificado como nulas. Se o método mais aplicável é um método parcial que não tem nenhum corpo, em seguida, a expressão de invocação é ignorada e o resultado é classificado como nulas.

Para uma expressão de invocação de associação inicial, os argumentos são avaliados na ordem em que os parâmetros correspondentes são declarados no método de destino. Uma expressão de acesso de membro de associação tardia, eles são avaliados na ordem em que aparecem na expressão de acesso de membro: consulte a seção [expressões de associação Late](expressions.md#late-bound-expressions).

## <a name="overloaded-method-resolution"></a>Método sobrecarregado de resolução:
Para resolução de sobrecarga, a especificidade dos membros/tipos recebe um argumento de lista, de forma geral, a aplicabilidade à lista de argumentos, passando argumentos e argumentos de separação para parâmetros opcionais, métodos condicionais e Inferência de argumento: consulte a seção [ Resolução de sobrecarga](overload-resolution.md).

## <a name="index-expressions"></a>Expressões de índice

Uma *expressão de índice* resulta em um elemento de matriz ou reclassifica um grupo de propriedades em um acesso de propriedade. Uma expressão de índice consiste, em ordem, uma expressão, um parêntese de abertura, uma lista de argumentos de índice e um parêntese de fechamento.

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

O destino da expressão do índice deve ser classificado como um valor ou um grupo de propriedades. Uma expressão de índice é processada da seguinte maneira:

* Se a expressão de destino é classificada como um valor e se seu tipo não é um tipo de matriz `Object`, ou `System.Array`, o tipo deve ter uma propriedade padrão. O índice é executado em um grupo de propriedade que representa todas as propriedades padrão do tipo. Embora não seja válido para declarar uma propriedade sem parâmetros padrão no Visual Basic, outras linguagens podem permitir que declarar essa propriedade. Consequentemente, a indexação de uma propriedade sem argumentos é permitida.

* Se a expressão resulta em um valor de um tipo de matriz, o número de argumentos na lista de argumentos deve ser o mesmo que a classificação do tipo de matriz e não pode incluir os argumentos nomeados. Se qualquer um dos índices são inválidos em tempo de execução, um `System.IndexOutOfRangeException` exceção é lançada. Cada expressão deve ser implicitamente conversível para o tipo `Integer`. O resultado da expressão do índice é a variável no índice especificado e é classificado como uma variável.

* Se a expressão é classificada como um grupo de propriedades, a resolução de sobrecarga é usada para determinar se uma das propriedades é aplicável à lista de argumentos de índice. Se o grupo de propriedades contém apenas uma propriedade que tem um `Get` acessador e se esse acessador não usa nenhum argumento, o grupo de propriedades é interpretado como uma expressão de índice com uma lista de argumentos vazia. O resultado é usado como o destino da expressão do índice atual. Se nenhuma propriedade é aplicável, ocorrerá um erro de tempo de compilação. Caso contrário, a expressão resulta em um acesso de propriedade com a expressão de destino associado (se houver) do grupo de propriedades.

* Se a expressão é classificada como um grupo de propriedades de associação tardia ou como um valor cujo tipo seja `Object` ou `System.Array`, o processamento de expressão do índice é adiado até o tempo de execução e a indexação é associação tardia. Os resultados da expressão em um acesso de propriedade de associação tardia digitado como `Object`. A expressão de destino associadas é a expressão de destino, se ele for um valor ou a expressão de destino associados do grupo de propriedades. Em tempo de execução a expressão é processada da seguinte maneira:

* Se a expressão é classificada como um grupo de propriedades de associação tardia, a expressão pode resultar em um grupo de métodos, um grupo de propriedades ou um valor (se o membro for uma instância ou uma variável compartilhada). Se o resultado é um grupo de método ou propriedade, a resolução de sobrecarga é aplicada ao grupo para determinar o método correto para a lista de argumentos. Se a resolução de sobrecarga falhar, um `System.Reflection.AmbiguousMatchException` exceção é lançada. Em seguida, o resultado é processado como um acesso de propriedade ou como uma invocação e o resultado é retornado. Se for a invocação de uma sub-rotina, o resultado é `Nothing`.

* Se o tipo de tempo de execução da expressão de destino é um tipo de matriz ou `System.Array`, o resultado da expressão do índice é o valor da variável no índice especificado.

* Caso contrário, o tipo de tempo de execução da expressão deve ter uma propriedade padrão e o índice é executado no grupo de propriedade que representa todas as propriedades padrão no tipo. Se o tipo não tem nenhuma propriedade padrão, em seguida, um `System.MissingMemberException` exceção é lançada.


## <a name="new-expressions"></a>Novas expressões

O `New` operador é usado para criar novas instâncias de tipos. Há quatro formas de `New` expressões:

* Expressões de criação do objeto são usadas para criar novas instâncias de tipos de classes e tipos de valor.

* Expressões de criação de matriz são usadas para criar novas instâncias dos tipos de matriz.

* Expressões de criação de delegado (que não têm uma sintaxe distinta das expressões de criação de objeto) são usadas para criar novas instâncias do representante de tipos.

* Expressões de criação de objeto anônimos são usadas para criar novas instâncias de tipos de classe anônima.

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

Um `New` expressão é classificada como um valor e o resultado é a nova instância do tipo.


### <a name="object-creation-expressions"></a>Expressões de criação do objeto

Uma expressão de criação de objeto é usada para criar uma nova instância de um tipo de classe ou um tipo de estrutura.

```antlr
ObjectCreationExpression
    : 'New' NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )?
      ObjectCreationExpressionInitializer?
    ;

ObjectCreationExpressionInitializer
    : ObjectMemberInitializer
    | ObjectCollectionInitializer
    ;

ObjectMemberInitializer
    : 'With' OpenCurlyBrace FieldInitializerList CloseCurlyBrace
    ;

FieldInitializerList
    : FieldInitializer ( Comma FieldInitializer )*
    ;

FieldInitializer
    : 'Key'? ('.' IdentifierOrKeyword Equals )? Expression
    ;

ObjectCollectionInitializer
    : 'From' CollectionInitializer
    ;

CollectionInitializer
    : OpenCurlyBrace CollectionElementList? CloseCurlyBrace
    ;

CollectionElementList
    : CollectionElement ( Comma CollectionElement )*
    ;

CollectionElement
    : Expression
    | CollectionInitializer
    ;
```

O tipo de uma expressão de criação do objeto deve ser um tipo de classe, um tipo de estrutura ou um parâmetro de tipo com uma `New` restrição e não pode ser um `MustInherit` classe. Considerando uma expressão de criação de objeto do formulário `New T(A)`, onde `T` é um tipo de classe ou tipo de estrutura e `A` é uma lista de argumentos opcionais, resolução de sobrecarga determina o construtor correto de `T` chamar. Um parâmetro de tipo com um `New` restrição é considerada como tendo um único construtor sem parâmetros. Se nenhum construtor for chamado, ocorre um erro de tempo de compilação; Caso contrário, a expressão resulta na criação de uma nova instância da `T` usando o construtor escolhido. Se não houver nenhum argumento, os parênteses podem ser omitidos.

Em que uma instância é alocada depende se a instância é um tipo de classe ou um tipo de valor. `New` instâncias de tipos de classe são criadas no heap de sistema, enquanto as novas instâncias de tipos de valor são criadas diretamente na pilha.

Uma expressão de criação de objetos, opcionalmente, pode especificar uma lista de inicializadores de membro depois os argumentos de construtor. Esses inicializadores de membro são prefixados com a palavra-chave `With`, e a lista de inicializadores é interpretada como se fosse no contexto de um `With` instrução. Por exemplo, dada a classe:

```vb
Class Customer
    Dim Name As String
    Dim Address As String
End Class
```

O código:

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = "123 Main St." }
    End Sub
End Module
```

É aproximadamente equivalente a:

```vb
Module Test
    Sub Main()
        Dim x, _t1 As Customer

        _t1 = New Customer()
        With _t1
            .Name = "Bob Smith"
            .Address = "123 Main St."
        End With

        x = _t1
    End Sub
End Module
```

Cada inicializador deve especificar um nome a ser atribuído e o nome deve ser um não -`ReadOnly` variável de instância ou a propriedade do tipo que está sendo construído; o acesso de membro não será atrasado associado se o tipo que está sendo construído é `Object`. Inicializadores não podem usar o `Key` palavra-chave. Cada membro em um tipo só pode ser inicializado uma vez. As expressões de inicializador, no entanto, podem referir-se entre si. Por exemplo:

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

Os inicializadores são atribuídos à esquerda para a direita, portanto, se um inicializador se refere a um membro que ainda não foi inicializado, ele verá o valor que a variável de instância após o construtor ser executado:

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

Inicializadores podem ser aninhados:

```vb
Class Customer
    Dim Name As String
    Dim Address As Address
    Dim Age As Integer
End Class

Class Address
    Dim Street As String
    Dim City As String
    Dim State As String
    Dim ZIP As String
End Class

Module Test
    Sub Main()
        Dim c As New Customer() With { _
            .Name = "John Smith", _
            .Address = New Address() With { _
                .Street = "23 Main St.", _
                .City = "Peoria", _
                .State = "IL", _
                .ZIP = "13934" }, _
            .Age = 34 }
    End Sub
End Module
```

Se o tipo que está sendo criado é um tipo de coleção e tem um método de instância nomeado `Add` (incluindo métodos de extensão e métodos compartilhados), em seguida, a expressão de criação do objeto pode especificar um inicializador de coleção, o prefixo da palavra-chave `From`. Uma expressão de criação do objeto não é possível especificar um inicializador de membro e um inicializador de coleção. Cada elemento no inicializador de coleção é passado como um argumento para uma invocação do `Add` função. Por exemplo:

```vb
Dim list = New List(Of Integer)() From { 1, 2, 3, 4 }
```

equivale a:

```vb
Dim list = New List(Of Integer)()
list.Add(1)
list.Add(2)
list.Add(3)
```

Se um elemento é um inicializador de coleção, cada elemento do inicializador de coleção de subpropriedades será passado como um argumento individual para o `Add` função. Por exemplo, o seguinte:

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

equivale a:

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

Essa expansão sempre é feita e só é feito um nível de profundidade; Depois disso, os inicializadores de subpropriedades são considerados literais de matriz. Por exemplo:

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a>Expressões de matriz

Uma expressão de matriz é usada para criar uma nova instância de um tipo de matriz. Há dois tipos de expressões de matriz: expressões de criação de matriz e literais de matriz.

#### <a name="array-creation-expressions"></a>Expressões de criação de matriz

Se for fornecido um modificador de inicialização de tamanho de matriz, o tipo de matriz resultante será derivado excluindo cada um dos argumentos individuais da lista de argumentos de inicialização do tamanho de matriz. O valor de cada argumento determina o limite superior da dimensão correspondente na instância de matriz recém-alocada. Se a expressão tiver um inicializador de coleção vazio, cada argumento na lista de argumentos deve ser uma constante, e os comprimentos de dimensão e de classificação especificados pela lista de expressão devem corresponder do inicializador de coleção.

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

Se um modificador de inicialização de tamanho de matriz não for fornecido, o nome do tipo deve ser um tipo de matriz e o inicializador de coleção deve estar vazia ou ter o mesmo número de níveis de aninhamento, como a classificação do tipo de matriz especificado. Todos os elementos em nível de aninhamento mais interno devem ser implicitamente conversíveis para o tipo de elemento da matriz e devem ser classificados como um valor. O número de elementos em cada inicializador de coleção aninhada sempre deve ser consistente com o tamanho das outras coleções do mesmo nível. Os comprimentos de dimensão individuais são inferidos do número de elementos em cada um dos níveis de aninhamento correspondentes do inicializador de coleção. Se o inicializador de coleção estiver vazio, o tamanho de cada dimensão é zero.

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

O nível de aninhamento mais externo de um inicializador de coleção corresponde à dimensão mais à esquerda de uma matriz, e o nível de aninhamento mais interno corresponde à dimensão mais à direita. O Exemplo:

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

é equivalente ao seguinte:

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

Se o inicializador de coleção está vazia (ou seja, um que contém chaves), mas nenhuma lista de inicializador e os limites do conhecidos as dimensões da matriz que está sendo inicializado, o inicializador de coleção vazia representa uma instância de matriz de tamanho especificado em que todos os elementos foram inicializados com valor de padrão do tipo de elemento. Se os limites das dimensões da matriz que está sendo inicializado não são conhecidos, o inicializador de coleção vazia representa uma instância de matriz no qual todas as dimensões são de tamanho zero.

Classificação de uma instância de matriz e o comprimento de cada dimensão são constantes para o tempo de vida inteiro da instância. Em outras palavras, não é possível alterar a classificação de uma instância de matriz existente, nem é possível redimensionar suas dimensões.

#### <a name="array-literals"></a>Literais de matriz

Um literal de matriz denota uma matriz cujo tipo de elemento, classificação e os limites é inferida com uma combinação de contexto da expressão e um inicializador de coleção. Isso é explicado na seção [reclassificação expressão](expressions.md#expression-reclassification).

```antlr
ArrayExpression
    : ArrayCreationExpression
    | ArrayLiteralExpression
    ;

ArrayCreationExpression
    : 'New' NonArrayTypeName ArrayNameModifier CollectionInitializer
    ;

ArrayLiteralExpression
    : CollectionInitializer
    ;
```

Por exemplo:

```vb
' array of integers
Dim a = {1, 2, 3}

' array of shorts
Dim b = {1S, 2S, 3S}

' array of shorts whose type is taken from the context
Dim c As Short() = {1, 2, 3}

' array of type Integer(,)
Dim d = {{1, 0}, {0, 1}}

' jagged array of rank ()()
Dim e = {({1, 0}), ({0, 1})}

' error: inconsistent rank
Dim f = {{1}, {2, 3}}

' error: inconsistent rank
Dim g = {1, {2}}
```

O formato e os requisitos para o inicializador de coleção em um literal de matriz é exatamente o mesmo que para o inicializador de coleção em uma expressão de criação de matriz.

__Observação.__ Um literal de matriz não cria a matriz por si só; em vez disso, é a reclassificação da expressão em um valor que faz com que a matriz a ser criado. Por exemplo, a conversão `CType(new Integer() {1,2,3}, Short())` não é possível porque não há nenhuma conversão de `Integer()` à `Short()`; mas a expressão `CType({1,2,3},Short())` é possível porque ele reclassifica primeiro o literal de matriz para a expressão de criação de matriz `New Short() {1,2,3}`.


### <a name="delegate-creation-expressions"></a>Expressões de criação de delegado

Uma expressão de criação de delegado é usada para criar uma nova instância de um tipo delegado. O argumento de uma expressão de criação de delegado deve ser uma expressão classificada como um ponteiro de método ou um método lambda.

Se o argumento for um ponteiro de método, um dos métodos referenciados pelo ponteiro de método deve ser aplicado à assinatura do tipo delegado. Um método `M` é aplicável a um tipo de delegado `D` se:

* `M` não é `Partial` ou tem um corpo.

* Ambos `M` e `D` são funções, ou `D` é uma sub-rotina.

* `M` e `D` têm o mesmo número de parâmetros.

* Os tipos de parâmetro de `M` possuem uma conversão do tipo de parâmetro correspondente do `D`e os modificadores (ou seja, `ByRef`, `ByVal`) correspondem.

* O tipo de retorno `M`, se houver, tem uma conversão para o tipo de retorno de `D`.

Se o ponteiro do método faz referência a um acesso de associação tardia, em seguida, o acesso de associação tardia é considerado para uma função que tem o mesmo número de parâmetros do tipo delegado.

Se a semântica estrita não está sendo usada e há apenas um método referenciado pelo ponteiro de método, mas ela não é aplicável devido ao fato de que ele não tem nenhum parâmetro e o tipo de delegado, o método é considerado aplicável e a parâmetros ou valor de retorno um Re simplesmente ignorados. Por exemplo:

```vb
Delegate Sub F(x As Integer)

Module Test
    Sub M()
    End Sub

    Sub Main()
        ' Valid
        Dim x As F = AddressOf M
    End Sub
End Module
```

__Observação.__ Essa atenuação só é permitida quando a semântica estrita não está sendo usada por causa de métodos de extensão. Como os métodos de extensão são considerados somente se um método regular não era aplicável, é possível para um método de instância sem parâmetros para ocultar um método de extensão com parâmetros com a finalidade de construção de delegado.

Se mais de um método referenciado pelo ponteiro de método é aplicável ao tipo de delegado, em seguida, a resolução de sobrecarga é usada para escolher entre os métodos de candidato. Os tipos dos parâmetros para o delegado são usados como os tipos dos argumentos para fins de resolução de sobrecarga. Se nenhum candidato de um método é mais aplicável, ocorrerá um erro de tempo de compilação. No exemplo a seguir, a variável local é inicializada com um delegado que se refere à segunda `Square` método porque esse método é mais aplicável ao tipo de retorno e a assinatura de `DoubleFunc`.

```vb
Delegate Function DoubleFunc(x As Double) As Double

Module Test
    Function Square(x As Single) As Single
        Return x * x
    End Function 

    Function Square(x As Double) As Double
        Return x * x
    End Function

    Sub Main()
        Dim a As New DoubleFunc(AddressOf Square)
    End Sub
End Module
```

Tinha a segunda `Square` método não estado presente, a primeira `Square` método foi escolhido. Se a semântica estrita é especificada pelo ambiente de compilação ou por `Option Strict`, em seguida, um erro de tempo de compilação ocorrerá se o método mais específico referenciado pelo ponteiro de método é mais estreito do que a assinatura do delegado. Um método `M` é considerada mais estreitas do que um tipo de delegado `D` se:

* Um tipo de parâmetro `M` tem uma conversão de ampliação para o tipo de parâmetro correspondente de `D`.

* Ou, o tipo de retorno, se houver, do `M` tem uma conversão redutora para o tipo de retorno de `D`.

Se os argumentos de tipo estão associados com o ponteiro de método, apenas os métodos com o mesmo número de argumentos de tipo são considerados. Se nenhum argumento de tipo está associado com o ponteiro de método, a inferência de tipo é usada ao fazer a correspondência de assinaturas em relação a um método genérico. Ao contrário de outra inferência de tipo normal, o tipo de retorno do delegado é usado quando inferir argumentos de tipo, mas tipos de retorno não ainda são considerados ao determinar a sobrecarga menos genérica. O exemplo a seguir mostra as duas maneiras de fornecer um argumento de tipo para uma expressão de criação de delegado:

```vb
Delegate Function D(s As String, i As Integer) As Integer
Delegate Function E() As Integer

Module Test
    Public Function F(Of T)(s As String, t1 As T) As T
    End Function

    Public Function G(Of T)() As T
    End Function

    Sub Main()
        Dim d1 As D = AddressOf f(Of Integer)    ' OK, type arg explicit
        Dim d2 As D = AddressOf f                ' OK, type arg inferred

        Dim e1 As E = AddressOf g(Of Integer)    ' OK, type arg explicit
        Dim e2 As E = AddressOf g                ' OK, infer from return
  End Sub
End Module
```

No exemplo acima, um tipo de delegado não genéricos foi instanciado usando um método genérico. Também é possível criar uma instância de um tipo de delegado construído usando um método genérico. Por exemplo:

```vb
Delegate Function Predicate(Of U)(u1 As U, u2 As U) As Boolean

Module Test
    Function Compare(Of T)(t1 As List(of T), t2 As List(of T)) As Boolean
        ...
    End Function

    Sub Main()
        Dim p As Predicate(Of List(Of Integer))
        p = AddressOf Compare(Of Integer)
    End Sub
End Module
```

Se o argumento para a expressão de criação de delegado é um método lambda, o método lambda deve ser aplicado à assinatura do tipo delegado. Um método lambda `L` é aplicável a um tipo de delegado `D` se:

* Se `L` tem parâmetros, `D` tem o mesmo número de parâmetros. (Se `L` não tem parâmetros, os parâmetros de `D` são ignoradas.)

* Os tipos de parâmetro de `L` cada uma tem uma conversão para o tipo de parâmetro correspondente do `D`e os modificadores (ou seja, `ByRef`, `ByVal`) correspondem.

* Se `D` é uma função, o tipo de retorno `L` tem uma conversão para o tipo de retorno de `D`. (Se `D` é uma sub-rotina, o valor de retorno `L` será ignorado.)

Se o parâmetro de tipo de um parâmetro de `L` for omitido, em seguida, o tipo do parâmetro correspondente no `D` é inferido; se o parâmetro de `L` tem modificadores de nome que permitem valor nulo ou matriz, um erro de tempo de compilação resulta. Uma vez todos os tipos de parâmetro de `L` estão disponíveis, em seguida, o tipo da expressão no método lambda é inferido. Por exemplo:

```vb
Delegate Function F(x As Integer, y As Long) As Long

Module Test
    Sub Main()
        ' b inferred to Integer, c and return type inferred to Long
        Dim a As F = Function(b, c) b + c

        ' e and return type inferred to Integer, f inferred to Long
        Dim d As F = Function(e, f) e + CInt(f)
    End Sub
End Module
```

Em algumas situações em que assinatura do delegado corresponde exatamente o método lambda ou a assinatura de método, o .NET Framework pode não oferecer suporte a criação de delegado nativamente. Nessa situação, uma expressão lambda de método é usada para corresponder os dois métodos. Por exemplo:

```vb
Delegate Function IntFunc(x As Integer) As Integer

Module Test
    Function SquareString(x As String) As String
        Return CInt(x) * CInt(x)
    End Function 

    Sub Main()
        ' The following two lines are equivalent
        Dim a As New IntFunc(AddressOf SquareString)
        Dim b As New IntFunc( _
            Function(x As Integer) CInt(SquareString(CStr(x))))
    End Sub
End Module
```

O resultado de uma expressão de criação de delegado é uma instância de delegado que se refere ao método de correspondência com a expressão de destino associado (se houver) da expressão de ponteiro de método. Se a expressão de destino é digitada como um tipo de valor, em seguida, o tipo de valor é copiado para o heap do sistema porque um delegado só pode apontar para um método de um objeto no heap. O método e o objeto ao qual se refere a um delegado permanecem constantes para o tempo de vida inteiro do delegado. Em outras palavras, não é possível alterar o destino ou o objeto de um delegado após ele ter sido criado.

### <a name="anonymous-object-creation-expressions"></a>Expressões de criação de objeto anônimo

Uma expressão de criação de objetos com inicializadores de membro também pode omitir o nome do tipo totalmente.

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

Nesse caso, um tipo anônimo é construído com base nos tipos e nomes dos membros inicializados como parte da expressão. Por exemplo:

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

O tipo criado por uma expressão de criação de objeto anônimos é uma classe que não tem nome, herda diretamente de `Object`, e tem um conjunto de propriedades com o mesmo nome que os membros atribuídos a na lista de inicializadores de membro. O tipo de cada propriedade é inferido usando as mesmas regras como inferência de tipo de variável local. Tipos anônimos gerados também substituem `ToString`, retornando uma representação de cadeia de caracteres de todos os membros e seus valores. (O formato exato da cadeia de caracteres está além do escopo desta especificação).

Por padrão, as propriedades geradas pelo tipo anônimo são leitura / gravação. É possível marcar uma propriedade de tipo anônimo como somente leitura usando o `Key` modificador. O `Key` modificador Especifica que o campo pode ser usado para identificar exclusivamente o valor que representa o tipo anônimo. Além de tornar a propriedade somente leitura, ele também faz com que o tipo anônimo substituir `Equals` e `GetHashCode` e para implementar a interface `System.IEquatable(Of T)` (preenchendo o tipo anônimo para `T`). Os membros são definidos da seguinte maneira:

`Function Equals(obj As Object) As Boolean` e `Function Equals(val As T) As Boolean` são implementados, validando as duas instâncias do mesmo tipo e, em seguida, comparando cada `Key` membro usando `Object.Equals`. Se todos os `Key` membros forem iguais, então `Equals` retorna `True`; caso contrário, `Equals` retorna `False`.

`Function GetHashCode() As Integer` é implementado, de modo que que, se `Equals` é verdadeiro para duas instâncias do tipo anônimo, em seguida, `GetHashCode` retornará o mesmo valor. O hash começa com um valor de semente e em seguida, para cada `Key` membro, na ordem multiplica o hash por 31 e adiciona a `Key` o valor de hash do membro (fornecido pelo `GetHashCode`) se o membro não é um tipo de referência ou tipo de valor anulável com o valor de `Nothing`.

Por exemplo, o tipo criado na instrução:

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

cria uma classe que aproximadamente tem esta aparência (embora a implementação exata pode variar):

```vb
Friend NotInheritable Class $Anonymous1
    Implements IEquatable(Of $Anonymous1)

    Private ReadOnly _zipCode As Integer
    Private _state As String

    Public Sub New(zipCode As Integer, state As String)
        _zipCode = zipcode
        _state = state
    End Sub

    Public ReadOnly Property ZipCode As Integer
        Get
            Return _zipCode
        End Get
    End Property

    Public Property State As String
        Get
            Return _state
        End Get
        Set (value As Integer)
            _state = value
        End Set
    End Property

    Public Overrides Function Equals(obj As Object) As Boolean
        Dim val As $Anonymous1 = TryCast(obj, $Anonymous1)
        Return Equals(val)
    End Function

    Public Overloads Function Equals(val As $Anonymous1) As Boolean _
        Implements IEquatable(Of $Anonymous1).Equals

        If val Is Nothing Then 
            Return False
        End If

        If Not Object.Equals(_zipCode, val._zipCode) Then 
            Return False
        End If

        Return True
    End Function

    Public Overrides Function GetHashCode() As Integer
        Dim hash As Integer = 0

        hash = hash Xor _zipCode.GetHashCode()

        Return hash
    End Function

    Public Overrides Function ToString() As String
        Return "{ Key .ZipCode = " & _zipCode & ", .State = " & _state & " }"
    End Function
End Class
```

Para simplificar a situação em que um tipo anônimo é criado a partir de campos de outro tipo, os nomes de campo podem ser inferidos diretamente de expressões nos seguintes casos:

* Uma expressão de nome simples `x` infere o nome `x`.

* Uma expressão de acesso de membro `x.y` infere o nome `y`.

* Uma expressão de pesquisa de dicionário `x!y` infere o nome `y`.

* Uma expressão de chamada ou de índice sem argumentos `x()` infere o nome `x`.

* Uma expressão de acesso de membro XML `x.<y>`, `x...<y>`, `x.@y` infere o nome `y`.

* Uma expressão de acesso de membro XML que é o destino de uma expressão de acesso de membro `x.<y>.z` infere o nome `z`.

* Uma expressão de acesso de membro XML que é o destino de uma expressão de chamada ou de índice sem argumentos `x.<y>.z()` infere o nome `z`.

* Uma expressão de acesso de membro XML que é o destino de uma expressão de índice ou de invocação `x.<y>(0)` infere o nome `y`.

O inicializador é interpretado como uma atribuição da expressão para o nome deduzido. Por exemplo, os inicializadores a seguir são equivalentes:

```vb
Class Address
    Public Street As String
    Public City As String
    Public State As String
    Public ZIP As String
End Class

Class C1
    Sub Test(a As Address)
        Dim cityState1 = New With { .City = a.City, .State = a.State }
        Dim cityState2 = New With { a.City, a.State }
    End Sub
End Class
```

Se um nome de membro é inferido, que está em conflito com um membro existente do tipo, como `GetHashCode`, em seguida, ocorre um erro de tempo de compilação. Ao contrário de inicializadores de membro regular, expressões de criação de objeto anônimos não permitem membro inicializadores ter referências circulares, ou para fazer referência a um membro antes que ela foi inicializada. Por exemplo:

```vb
Module Test
    Sub Main()
        ' Error: Circular references
        Dim x = New With { .a = .b, .b = .a }

        ' Error: Referring to .b before it has been assigned to
        Dim y = New With { .a = .b, .b = 10 }

        ' Error: Referring to .a before it has been assigned to
        Dim z = New With { .a = .a }
    End Sub
End Module
```

Se duas expressões de criação de classe anônima ocorrem dentro do mesmo método e produzem a mesma forma resultante – se a ordem das propriedades, nomes de propriedade e propriedade tipos correspondem todos – eles serão ambos se referem à mesma classe anônima. O escopo do método de uma instância ou uma variável com um inicializador de membro compartilhado é o construtor no qual a variável é inicializada.

__Observação.__ É possível que um compilador pode optar por unificar anônimo tipos Além disso, tal como no nível de assembly, mas isso não pode ser usado no momento.


## <a name="cast-expressions"></a>Expressões de conversão

Uma expressão de conversão converte uma expressão em um determinado tipo. Palavras-chave de conversão específica forçar expressões em tipos primitivos. Três palavras-chave cast geral, `CType`, `TryCast` e `DirectCast`, força uma expressão em um tipo.

```antlr
CastExpression
    : 'DirectCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'TryCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'CType' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | CastTarget OpenParenthesis Expression CloseParenthesis
    ;

CastTarget
    : 'CBool' | 'CByte' | 'CChar'  | 'CDate'  | 'CDec' | 'CDbl' | 'CInt'
    | 'CLng'  | 'CObj'  | 'CSByte' | 'CShort' | 'CSng' | 'CStr' | 'CUInt'
    | 'CULng' | 'CUShort'
    ;
```

`DirectCast` e `TryCast` ter comportamentos especiais. Por isso, eles suportam apenas conversões nativas. Além disso, o tipo de destino em um `TryCast` expressão não pode ser um tipo de valor. Operadores de conversão não são considerados quando definidos pelo usuário `DirectCast` ou `TryCast` é usado. (__Observação.__ A conversão definida que `DirectCast` e `TryCast` suporte são restritas porque eles implementam conversões "CLR nativo". A finalidade `DirectCast` é fornecer a funcionalidade da instrução "conversão unboxing", enquanto a finalidade de `TryCast` é fornecer a funcionalidade da instrução "isinst". Uma vez que eles mapeiam para as instruções do CLR, oferecer suporte a conversões não é diretamente compatível com o CLR acabaria com a finalidade pretendida.)

`DirectCast` Converte as expressões que são digitadas como `Object` diferentemente `CType`. Ao converter uma expressão do tipo `Object` cujo tipo de tempo de execução é um tipo de valor primitivo `DirectCast` lança uma `System.InvalidCastException` exceção se o tipo especificado não é igual ao tipo de tempo de execução da expressão ou um `System.NullReferenceException` se a expressão é avaliada como `Nothing`. (__Observação.__ Conforme observado acima, `DirectCast` mapeia diretamente para a instrução CLR "conversão unboxing" quando o tipo da expressão é `Object`. Em contraste, `CType` se transforma em uma chamada para um auxiliar de tempo de execução para fazer a conversão para que as conversões entre tipos primitivos podem ter suporte. No caso de quando um `Object` expressão está sendo convertida em um tipo de valor primitivo e o tipo de correspondência de instância real do tipo de destino, `DirectCast` será consideravelmente mais rápido que `CType`.)

`TryCast` converte expressões, mas não lançará uma exceção se a expressão não pode ser convertida para o tipo de destino. Em vez disso, `TryCast` resultará em `Nothing` se a expressão não pode ser convertida em tempo de execução. (__Observação.__ Conforme observado acima, `TryCast` mapeia diretamente para a instrução de CLR "isinst". Ao combinar a verificação de tipo e a conversão em uma única operação, `TryCast` pode ser mais barato do que fazer uma `TypeOf ... Is` e, em seguida, um `CType`.)

Por exemplo:

```vb
Interface ITest
    Sub Test()
End Interface

Module Test
    Sub Convert(o As Object)
        Dim i As ITest = TryCast(o, ITest)

        If i IsNot Nothing Then
            i.Test()
        End If
    End Sub
End Module
```

Se não existe conversão do tipo da expressão no tipo especificado, ocorrerá um erro de tempo de compilação. Caso contrário, a expressão é classificada como um valor e o resultado é o valor produzido pela conversão.


## <a name="operator-expressions"></a>Expressões do operador

Há dois tipos de operadores. *Operadores unários* usam um operando e usar a notação de prefixo (por exemplo, `-x`). *Operadores binários* usam dois operandos e usar a notação de infixo (por exemplo, `x + y`). Com exceção dos operadores relacionais, que sempre resultam em `Boolean`, um operador definido para um tipo específico de resultados nesse tipo. Os operandos para um operador sempre devem ser classificados como um valor. o resultado de uma expressão do operador é classificado como um valor.

```antlr
OperatorExpression
    : ArithmeticOperatorExpression
    | RelationalOperatorExpression
    | LikeOperatorExpression
    | ConcatenationOperatorExpression
    | ShortCircuitLogicalOperatorExpression
    | LogicalOperatorExpression
    | ShiftOperatorExpression
    | AwaitOperatorExpression
    ;
```

### <a name="operator-precedence-and-associativity"></a>Precedência e associatividade do operador

Quando uma expressão contiver vários operadores binários, o *precedência* dos operadores controla a ordem na qual os operadores binários individuais são avaliados. Por exemplo, a expressão `x + y * z` é avaliada como `x + (y * z)` porque o operador `*` tem precedência maior do que o operador `+`. A tabela a seguir lista os operadores binários em ordem decrescente de precedência:


| __Categoria__     | __Operadores__                                          | 
|------------------|--------------------------------------------------------|
| Primária          | Todas as expressões de não-operador                           |
| Await            | `Await`                                                |
| Exponenciação   | `^`                                                    |
| Negação unária   | `+`, `-`                                               |
| Multiplicativo   | `*`, `/`                                               |
| Divisão de inteiros | `\`                                                    |
| Módulo          | `Mod`                                                  |
| Aditivo         | `+`, `-`                                               |
| Concatenação    | `&`                                                    |
| Shift            | `<<`, `>>`                                             |
| Relacional       | `=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot` |
| NOT lógico      | `Not`                                                  |
| AND lógico      | `And`, `AndAlso`                                       |
| OR lógico       | `Or`, `OrElse`                                         |
| XOR lógico      | `Xor`                                                  |

Quando uma expressão contém dois operadores com a mesma precedência, o *associatividade* dos operadores controla a ordem na qual as operações são executadas. Todos os operadores binários são associativos à esquerda, o que significa que operações são executadas da esquerda para a direita. Precedência e associatividade podem ser controladas usando expressões entre parênteses.

### <a name="object-operands"></a>Operandos de objeto

Além dos tipos regulares compatíveis com cada operador, todos os operadores de suportam a operandos do tipo `Object`. Operadores aplicadas a `Object` operandos são tratados da mesma forma para chamadas de método feitas no `Object` valores: uma chamada de método de associação tardia pode ser escolhido, caso em que o tipo de tempo de execução dos operandos, em vez do tipo de tempo de compilação, determina a validade e o tipo de operação. Se a semântica estrita é especificada pelo ambiente de compilação ou pelo `Option Strict`, os operadores com operandos do tipo `Object` causa um erro de tempo de compilação, exceto para o `TypeOf...Is`, `Is` e `IsNot` operadores.

Quando a resolução de operador determina que uma operação deve ser realizada associação tardia, o resultado da operação é o resultado da aplicação do operador para os tipos de operando, se os tipos de tempo de execução dos operandos são tipos que são suportados pelo operador. O valor `Nothing` é tratado como o valor padrão do tipo do operando em uma expressão de operador binário. Em uma expressão de operador unário, ou se ambos os operandos forem `Nothing` em uma expressão de operador binário, é o tipo de operação `Integer` ou o único tipo de resultado do operador, se o operador não resulta em `Integer`. O resultado da operação, em seguida, sempre é convertido para `Object`. Se os tipos de operando não tem nenhum operador válido, um `System.InvalidCastException` exceção é lançada. Conversões em tempo de execução são realizadas sem levar em consideração se elas são implícitas ou explícitas.

Se o resultado de uma operação binária numérico produza uma exceção de estouro (independentemente se a verificação de estouro de inteiro está ativado ou desativado), em seguida, o tipo de resultado será promovido para o próximo tipo numérico mais amplo, se possível. Por exemplo, considere o seguinte código:

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

Imprime o seguinte resultado:

```
System.Int16 = 512
```

Se nenhum tipo numérico maior está disponível para manter o número, um `System.OverflowException` exceção é lançada.

### <a name="operator-resolution"></a>Operador de resolução

Dado um tipo de operador e um conjunto de operandos, resolução de operador determina qual operador a ser usado para os operandos. Durante a resolução de operadores, operadores definidos pelo usuário serão considerados primeiro, usando as seguintes etapas:

1. Primeiro, todos os operadores de candidato são coletados. Os operadores de candidato são todos os operadores definidos pelo usuário do tipo de operador específico no tipo de origem e todos os operadores definidos pelo usuário do tipo determinado no tipo de destino. Se o tipo de origem e o tipo de destino estiverem relacionadas, os operadores comuns somente são considerados uma vez.

2. Em seguida, a resolução de sobrecarga é aplicada para os operadores e operandos para selecionar o operador mais específico. No caso de operadores binários, isso pode resultar em uma chamada de associação tardia.

Ao coletar os operadores de candidato para um tipo `T?`, os operadores de tipo `T` são usados em vez disso. Qualquer um dos `T`do definido pelo usuário que envolvem tipos de valor não nulo somente operadores também são elevadas. Um operador elevado usa a versão que permite valor nula de qualquer tipo de valor com a exceção os tipos de retorno de `IsTrue` e `IsFalse` (que deve ser `Boolean`). Elevadas operadores são avaliados, convertendo os operandos para sua versão não anuláveis, em seguida, avaliando o operador definido pelo usuário e, em seguida, converter o resultado de tipo para a versão que permite valor nula. Se estiver operando ether `Nothing`, o resultado da expressão é um valor de `Nothing` digitado como a versão que permite valor nula do tipo de resultado. Por exemplo:

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Operator +(ByVal op1 As S, ByVal op2 As T) As T
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As S?
        Dim y, z As T?

        ' Valid, as S + T = T is lifted to S? + T? = T?
        z = x + y 
    End Sub
End Module
```

Se o operador é um operador binário e um dos operandos é o tipo de referência, o operador também é elevado, mas nenhuma associação para o operador produz um erro. Por exemplo:

```vb
Structure S1
    Public F1 As Integer

    Public Shared Operator +(left As S1, right As String) As S1
       ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim a? As S1
        Dim s As String
        
        ' Error: '+' is not defined for S1? and String
        a = a + s
    End Sub
End Module
```

__Observação.__ Essa regra existe porque tem havido consideração se desejamos adicionar tipos de referência de propagação nula em uma versão futura, no qual o comportamento no caso de operadores binários entre os dois tipos de caso seria alterado.

Assim como acontece com conversões, operadores definidos pelo usuário sempre têm preferência sobre operadores canceladas.

Quando a resolução de operadores sobrecarregados, pode haver diferenças entre as classes definidas no Visual Basic e aquelas definidas em outras linguagens:

* Em outras linguagens `Not`, `And`, e `Or` pode estar sobrecarregado como operadores lógicos e operadores bit a bit. Após a importação de um assembly externo, o formulário será aceito como uma sobrecarga válida para usar esses operadores. No entanto, para um tipo que define operadores lógicos e bit a bit, apenas a implementação de bit a bit será considerada.

* Em outras linguagens `>>` e `<<` pode estar sobrecarregado como operadores com sinal e sem sinal de operadores. Após a importação de um assembly externo, o formulário será aceito como uma sobrecarga válida. No entanto, para um tipo que define operadores assinados e não assinados, apenas a implementação de com sinal será considerada.

* Se nenhum operador definido pelo usuário é o mais específico aos operandos, serão considerados operadores intrínseco. Se nenhum operador intrínseco está definido para os operandos e qualquer operando tem o tipo de objeto, em seguida, o operador será resolvido tardia; Caso contrário, um erro de tempo de compilação ocorre.

Em versões anteriores do Visual Basic, se houve exatamente um operando do tipo objeto e nenhum operador aplicável de definidas pelo usuário e nenhum operador intrínseco aplicável, em seguida, ele foi um erro. A partir de 11 de Visual Basic, agora ele está resolvido associação tardia. Por exemplo:

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

Um tipo `T` que tem um intrínseco operador também define esse mesmo operador para `T?`. O resultado do operador no `T?` será o mesmo que para `T`, exceto que se qualquer operando for `Nothing`, o resultado do operador será `Nothing` (ou seja, o valor nulo é propagado). Para fins de resolver o tipo de uma operação, o `?` é removido de qualquer operandos que tenham-los, o tipo de operação é determinado e um `?` é adicionado para o tipo da operação se qualquer um dos operandos eram tipos de valor anulável. Por exemplo:

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

Cada operador lista os tipos intrínsecos, para que ele é definido e do tipo de operação executada considerando os tipos de operando. O resultado do tipo de uma operação intrínseco segue estas regras gerais:

* Se todos os operandos forem do mesmo tipo e o operador está definido para o tipo, não ocorre nenhuma conversão, e o operador para esse tipo é usado.

* Qualquer operando cujo tipo não está definido para o operador é convertido usando as seguintes etapas e o operador é resolvido em relação a novos tipos:

  * O operando é convertido para o próximo maior tipo que está definido para o operador e o operando, e para que ele é convertido implicitamente.

  * Se não houver nenhum tal tipo, o operando é convertido para o tipo do próximo mais restrito que é definido para o operador e o operando e para que ele é convertido implicitamente.

  * Se houver nenhum tal tipo ou a conversão não pode ocorrer, ocorrerá um erro de tempo de compilação.

* Caso contrário, os operandos são convertidos para o maior dos tipos de operando e o operador para esse tipo é usado. Se o tipo de operando mais restrito não pode ser convertido implicitamente no tipo de operador mais amplo, ocorrerá um erro de tempo de compilação.

Apesar dessas regras gerais, no entanto, há um número de casos especiais, indicadas nas tabelas de resultados do operador.

__Observação.__ Por motivos de formatação, as tabelas do tipo de operador abreviar nomes predefinidos para os dois primeiros caracteres. Então, "por" é `Byte`, é "Interface do usuário" `UInteger`, é "St" `String`, etc. "Err" significa que há uma operação definida para os tipos de operando determinado.

## <a name="arithmetic-operators"></a>Operadores aritméticos

O `*`, `/`, `\`, `^`, `Mod`, `+`, e `-` operadores são os *operadores aritméticos*.

```antlr
ArithmeticOperatorExpression
    : UnaryPlusExpression
    | UnaryMinusExpression
    | AdditionOperatorExpression
    | SubtractionOperatorExpression
    | MultiplicationOperatorExpression
    | DivisionOperatorExpression
    | ModuloOperatorExpression
    | ExponentOperatorExpression
    ;
```

Operações de aritméticas de ponto flutuante podem ser executadas com precisão maior do que o tipo de resultado da operação. Por exemplo, algumas arquiteturas de hardware oferecem suporte a um tipo de ponto flutuante "extended" ou "long double" com o maior intervalo e a precisão do que o `Double` digite e implicitamente executar todas as operações de ponto flutuantes usando esse tipo de precisão mais alta. Arquiteturas de hardware podem ser feitas para realizar operações de ponto flutuante com menos precisão somente custo excessiva no desempenho. em vez de exigir uma implementação perder o desempenho e precisão, o Visual Basic permite que o tipo de maior precisão a ser usado para todas as operações de ponto flutuantes. Além de fornecer resultados mais precisos, isso raramente tem efeitos mensuráveis. No entanto, em expressões do formulário `x * y / z`, em que a multiplicação produz um resultado que está fora os `Double` intervalo, mas a divisão subsequente leva o resultados temporários de volta para o `Double` de intervalo, o fato de que a expressão é avaliadas em um intervalo maior-formato pode causar um resultado finito para ser produzidos em vez de infinito.


### <a name="unary-plus-operator"></a>Operador de adição unário

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

O operador unário de adição é definido para o `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Single`, `Double`, e `Decimal` tipos .

__Tipo de operação:__


| __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | Por | Sh | US | No | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer | Ob | 


### <a name="unary-minus-operator"></a>Operador de subtração unário

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

O operador de subtração unário é definido para os seguintes tipos:

`SByte`, `Short`, `Integer` e `Long`. O resultado é calculado subtraindo-se o operando do zero. Se a verificação de estouro de inteiro estiver ligado e o valor do operando é o negativo máximo `SByte`, `Short`, `Integer`, ou `Long`, um `System.OverflowException` exceção é lançada. Caso contrário, se o valor do operando é o negativo máximo `SByte`, `Short`, `Integer`, ou `Long`, o resultado é o mesmo valor, e o estouro não será relatado.

`Single` e `Double`. O resultado é o valor do operando com o sinal invertido, incluindo os valores de 0 e infinito. Se o operando for NaN, o resultado também é NaN.

`Decimal`. O resultado é calculado subtraindo-se o operando do zero.

__Tipo de operação:__

| __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | Sh | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer | Ob | 


### <a name="addition-operator"></a>Operador de adição

O operador de adição calcula a soma dos dois operandos.

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

O operador de adição é definido para os seguintes tipos:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, e `Long`. Se a verificação de estouro de inteiro estiver ligado e a soma está fora do intervalo do tipo de resultado, um `System.OverflowException` exceção é lançada. Caso contrário, estouros não são relatados e quaisquer bits de ordem superior significativos do resultado são descartados.

* `Single` e `Double`. A soma é calculada de acordo com as regras de aritmética do IEEE 754.

* `Decimal`. Se o valor resultante é muito grande para ser representado no formato decimal, um `System.OverflowException` exceção é lançada. Se o valor do resultado for muito pequeno para ser representado no formato decimal, o resultado é 0.

* `String`. Os dois `String` operandos são concatenados.

* `Date`. O `System.DateTime` tipo define os operadores de adição sobrecarregado. Porque `System.DateTime` é equivalente a intrínseca `Date` tipo, esses operadores também está disponível a `Date` tipo.

__Tipo de operação:__

|        | __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __bO__ | Sh | SB | Sh | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __SB__ |    | SB | Sh | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __Por__ |    |    | Por | Sh | US | No | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __Sh__ |    |    |    | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __NÓS__ |    |    |    |    | US | No | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __In__ |    |    |    |    |    | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __INTERFACE DO USUÁRIO__ |    |    |    |    |    |    | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __Alemanha__ |    |    |    |    |    |    |    |    |    | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __si__ |    |    |    |    |    |    |    |    |    |    | si | fazer | Err | Err | fazer | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | fazer | Err | Err | fazer | Ob | 
| __Acelerador de desenvolvimento__ |    |    |    |    |    |    |    |    |    |    |    |    | ST  | Err | ST | Ob | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | ST  | ST | Ob | 
| __ST__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | ST | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Ob | 


### <a name="subtraction-operator"></a>Operador de subtração

O operador de subtração subtrai o segundo operando do primeiro operando.

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

O operador de subtração é definido para os seguintes tipos:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, e `Long`. Se a verificação de estouro de inteiro estiver ligado e a diferença está fora do intervalo do tipo de resultado, um `System.OverflowException` exceção é lançada. Caso contrário, estouros não são relatados e quaisquer bits de ordem superior significativos do resultado são descartados.

* `Single` e `Double`. A diferença é calculada de acordo com as regras de aritmética do IEEE 754.

* `Decimal`. Se o valor resultante é muito grande para ser representado no formato decimal, um `System.OverflowException` exceção é lançada. Se o valor do resultado for muito pequeno para ser representado no formato decimal, o resultado é 0.

* `Date`. O `System.DateTime` tipo define os operadores de subtração sobrecarregado. Porque `System.DateTime` é equivalente a intrínseca `Date` tipo, esses operadores também está disponível a `Date` tipo.

__Tipo de operação:__

|        | __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bO__ | Sh | SB | Sh | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __SB__ |    | SB | Sh | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Por__ |    |    | Por | Sh | US | No | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Sh__ |    |    |    | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __NÓS__ |    |    |    |    | US | No | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __In__ |    |    |    |    |    | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __INTERFACE DO USUÁRIO__ |    |    |    |    |    |    | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Alemanha__ |    |    |    |    |    |    |    |    |    | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | si | fazer | Err | Err | fazer  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | fazer | Err | Err | fazer  | Ob  | 
| __Acelerador de desenvolvimento__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __ST__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | fazer  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="multiplication-operator"></a>Operador de multiplicação

O operador de multiplicação calcula o produto dos dois operandos.

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

O operador de multiplicação é definido para os seguintes tipos:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, e `Long`. Se a verificação de estouro de inteiro estiver ligado e o produto está fora do intervalo do tipo de resultado, um `System.OverflowException` exceção é lançada. Caso contrário, estouros não são relatados e quaisquer bits de ordem superior significativos do resultado são descartados.

* `Single` e `Double`. O produto é calculado de acordo com as regras de aritmética do IEEE 754.

* `Decimal`. Se o valor resultante é muito grande para ser representado no formato decimal, um `System.OverflowException` exceção é lançada. Se o valor do resultado for muito pequeno para ser representado no formato decimal, o resultado é 0.

__Tipo de operação:__

|        | __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bO__ | Sh | SB | Sh | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __SB__ |    | SB | Sh | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Por__ |    |    | Por | Sh | US | No | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Sh__ |    |    |    | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __NÓS__ |    |    |    |    | US | No | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __In__ |    |    |    |    |    | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __INTERFACE DO USUÁRIO__ |    |    |    |    |    |    | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Alemanha__ |    |    |    |    |    |    |    |    |    | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | si | fazer | Err | Err | fazer  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | fazer | Err | Err | fazer  | Ob  | 
| __Acelerador de desenvolvimento__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __ST__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | fazer  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="division-operators"></a>Operadores de divisão

O quociente de dois operandos de computação de operadores de divisão. Há dois operadores de divisão: o operador de divisão (ponto flutuante) regular e o operador de divisão de inteiro.

```antlr
DivisionOperatorExpression
    : FPDivisionOperatorExpression
    | IntegerDivisionOperatorExpression
    ;

FPDivisionOperatorExpression
    : Expression '/' LineTerminator? Expression
    ;

IntegerDivisionOperatorExpression
    : Expression '\\' LineTerminator? Expression
    ;
```

O operador de divisão regular é definido para os seguintes tipos:

* `Single` e `Double`. O quociente é calculado de acordo com as regras de aritmética do IEEE 754.

* `Decimal`. Se o valor do operando à direita for zero, um `System.DivideByZeroException` exceção é lançada. Se o valor resultante é muito grande para ser representado no formato decimal, um `System.OverflowException` exceção é lançada. Se o valor do resultado for muito pequeno para ser representado no formato decimal, o resultado é zero. A escala do resultado, antes de qualquer arredondamento, é a escala mais próxima à escala preferencial que preserva um resultado igual ao resultado exato.  A escala preferencial é a escala do primeiro operando menor a escala do segundo operando.

Acordo com as regras de resolução de operador normal, divisão regular puramente entre os operandos de tipos, como `Byte`, `Short`, `Integer`, e `Long` faria com que ambos os operandos para ser convertido no tipo `Decimal`. No entanto, ao fazer a resolução de operador no operador de divisão quando nenhum tipo é `Decimal`, `Double` é considerada mais estreitas do que `Decimal`. Essa convenção é seguida porque `Double` divisão é mais eficiente que `Decimal` divisão.

__Tipo de operação:__

|        | __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bO__ | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __SB__ |    | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Por__ |    |    | fazer | fazer | fazer | fazer | fazer | fazer | fazer | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Sh__ |    |    |    | fazer | fazer | fazer | fazer | fazer | fazer | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __NÓS__ |    |    |    |    | fazer | fazer | fazer | fazer | fazer | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __In__ |    |    |    |    |    | fazer | fazer | fazer | fazer | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __INTERFACE DO USUÁRIO__ |    |    |    |    |    |    | fazer | fazer | fazer | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | fazer | fazer | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | fazer | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Alemanha__ |    |    |    |    |    |    |    |    |    | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | si | fazer | Err | Err | fazer  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | fazer | Err | Err | fazer  | Ob  | 
| __Acelerador de desenvolvimento__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __ST__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | fazer  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 

O operador de divisão de inteiro é definido para `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, e `Long`. Se o valor do operando à direita for zero, um `System.DivideByZeroException` exceção é lançada. A divisão Arredonda o resultado em direção a zero e o valor absoluto do resultado é o maior inteiro possíveis que é menor que o valor absoluto do quociente de dois operandos. O resultado é zero ou positivo quando os dois operandos tiverem o mesmo sinal e zero ou negativo quando os dois operandos tiverem oposta sinais. Se o operando esquerdo é o negativo máximo `SByte`, `Short`, `Integer`, ou `Long`, e o operando direito é `-1`, ocorre um estouro; se a verificação de estouro de inteiro está ativada, um `System.OverflowException` exceção é lançada. Caso contrário, o estouro não é relatado e, em vez disso, o resultado é o valor do operando esquerdo.

__Observação.__ Como os dois operandos de tipos sem sinal sempre será zero ou positivo, o resultado é sempre zero ou positivo.  Como o resultado da expressão sempre será menor ou igual ao maior dos dois operandos, não é possível que ocorra um estouro.  Como tal, a verificação de estouro de inteiro não é executada para a divisão de inteiro com dois inteiros sem sinal. O resultado é o tipo do operando à esquerda.


__Tipo de operação:__

|        | __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bO__ | Sh | SB | Sh | Sh | No | No | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __SB__ |    | SB | Sh | Sh | No | No | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Por__ |    |    | Por | Sh | US | No | Interface de Usuário | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Sh__ |    |    |    | Sh | No | No | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __NÓS__ |    |    |    |    | US | No | Interface de Usuário | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __In__ |    |    |    |    |    | No | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __INTERFACE DO USUÁRIO__ |    |    |    |    |    |    | Interface de Usuário | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Alemanha__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Ob  | 
| __Acelerador de desenvolvimento__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __ST__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="mod-operator"></a>Operador Mod

O `Mod` (operador módulo) calcula o resto da divisão entre dois operandos.

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

O `Mod` operador está definido para os seguintes tipos:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` e `Long`. O resultado de `x Mod y` é o valor produzido por `x - (x \ y) * y`. Se `y` for zero, um `System.DivideByZeroException` exceção é lançada. O operador de módulo nunca faz com que um estouro.

* `Single` e `Double`. O restante é calculado de acordo com as regras de aritmética do IEEE 754.

* `Decimal`. Se o valor do operando à direita for zero, um `System.DivideByZeroException` exceção é lançada. Se o valor resultante é muito grande para ser representado no formato decimal, um `System.OverflowException` exceção é lançada. Se o valor do resultado for muito pequeno para ser representado no formato decimal, o resultado é zero.

__Tipo de operação:__

|        | __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bO__ | Sh | SB | Sh | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __SB__ |    | SB | Sh | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Por__ |    |    | Por | Sh | US | No | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Sh__ |    |    |    | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __NÓS__ |    |    |    |    | US | No | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __In__ |    |    |    |    |    | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __INTERFACE DO USUÁRIO__ |    |    |    |    |    |    | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __Alemanha__ |    |    |    |    |    |    |    |    |    | Alemanha | si | fazer | Err | Err | fazer  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | si | fazer | Err | Err | fazer  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | fazer | Err | Err | fazer  | Ob  | 
| __Acelerador de desenvolvimento__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __ST__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | fazer  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="exponentiation-operator"></a>Operador de exponenciação

O operador de exponenciação calcula o primeiro operando elevado à potência do segundo operando.

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

O operador de exponenciação é definido para o tipo `Double`. O valor é calculado de acordo com as regras de aritmética do IEEE 754.

__Tipo de operação:__

|        | __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bO__ | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | Err | Err | fazer  | Ob  | 
| __SB__ |    | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | Err | Err | fazer  | Ob  | 
| __Por__ |    |    | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | Err | Err | fazer  | Ob  | 
| __Sh__ |    |    |    | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | Err | Err | fazer  | Ob  | 
| __NÓS__ |    |    |    |    | fazer | fazer | fazer | fazer | fazer | fazer | fazer | fazer | Err | Err | fazer  | Ob  | 
| __In__ |    |    |    |    |    | fazer | fazer | fazer | fazer | fazer | fazer | fazer | Err | Err | fazer  | Ob  | 
| __INTERFACE DO USUÁRIO__ |    |    |    |    |    |    | fazer | fazer | fazer | fazer | fazer | fazer | Err | Err | fazer  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | fazer | fazer | fazer | fazer | fazer | Err | Err | fazer  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | fazer | fazer | fazer | fazer | Err | Err | fazer  | Ob  | 
| __Alemanha__ |    |    |    |    |    |    |    |    |    | fazer | fazer | fazer | Err | Err | fazer  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | fazer | fazer | Err | Err | fazer  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | fazer | Err | Err | fazer  | Ob  | 
| __Acelerador de desenvolvimento__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __ST__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | fazer  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


## <a name="relational-operators"></a>Operadores relacionais

O *operadores relacionais* comparar valores a qualquer outra. Os operadores de comparação são `=`, `<>`, `<`, `>`, `<=`, e `>=`.

```antlr
RelationalOperatorExpression
    : Expression '=' LineTerminator? Expression
    | Expression '<' '>' LineTerminator? Expression
    | Expression '<' LineTerminator? Expression
    | Expression '>' LineTerminator? Expression
    | Expression '<' '=' LineTerminator? Expression
    | Expression '>' '=' LineTerminator? Expression
    ;
```

Todos os operadores relacionais resultam em um `Boolean` valor.

Os operadores relacionais têm o seguinte significado geral:

* O `=` operador testa se os dois operandos forem iguais.

* O `<>` operador testa se os dois operandos não são iguais.

* O `<` operador testa se o primeiro operando for menor que o segundo operando.

* O `>` operador testa se o primeiro operando for maior que o segundo operando.

* O `<=` operador testa se o primeiro operando for menor ou igual ao segundo operando.

* O `>=` operador testa se o primeiro operando for maior que ou igual ao segundo operando.

Os operadores relacionais são definidos para os seguintes tipos:

* `Boolean`. Os operadores comparam os valores dos dois operandos de verdade. `True` é considerado menor que `False`, que corresponde a com seus valores numéricos.

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, e `Long`. Os operadores comparam os valores numéricos dos dois operandos integrais.

* `Single` e `Double`. Os operadores comparam os operandos de acordo com as regras do padrão IEEE 754.

* `Decimal`. Os operadores comparam os valores numéricos dos dois operandos decimais.

* `Date`. Os operadores retornam o resultado de comparar os dois valores de data/hora.

* `Char`. Os operadores retornam o resultado da comparação dos dois valores de Unicode.

* `String`. Os operadores retornam o resultado de comparar os dois valores usando uma comparação binária ou uma comparação de texto. A comparação usada é determinada pelo ambiente de compilação e o `Option Compare` instrução. Uma comparação binária determina se o valor de Unicode numérico de cada caractere em cada cadeia de caracteres é o mesmo. Uma comparação de texto faz uma comparação de texto Unicode com base na cultura atual em uso no .NET Framework. Ao fazer uma comparação de cadeia de caracteres, um valor nulo é equivalente à cadeia de caracteres literal `""`.

__Tipo de operação:__
        
|        | __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __bO__ | bO | SB | Sh | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | bO | Ob | 
| __SB__ |    | SB | Sh | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __Por__ |    |    | Por | Sh | US | No | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __Sh__ |    |    |    | Sh | No | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __NÓS__ |    |    |    |    | US | No | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __In__ |    |    |    |    |    | No | Lo | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __INTERFACE DO USUÁRIO__ |    |    |    |    |    |    | Interface de Usuário | Lo | UL | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Alemanha | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __Alemanha__ |    |    |    |    |    |    |    |    |    | Alemanha | si | fazer | Err | Err | fazer | Ob | 
| __si__ |    |    |    |    |    |    |    |    |    |    | si | fazer | Err | Err | fazer | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | fazer | Err | Err | fazer | Ob | 
| __Acelerador de desenvolvimento__ |    |    |    |    |    |    |    |    |    |    |    |    | Acelerador de desenvolvimento  | Err | Acelerador de desenvolvimento | Ob | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | CH  | ST | Ob | 
| __ST__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | ST | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Ob | 


## <a name="like-operator"></a>Operador Like

O `Like` operador determina se uma cadeia de caracteres corresponde a um determinado padrão.

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

O `Like` operador está definido para o `String` tipo. O primeiro operando é a cadeia de caracteres que está sendo correspondida e o segundo operando é o padrão ao qual corresponder. O padrão é composto por caracteres Unicode. As seguintes sequências de caracteres têm significados especiais:

* O caractere `?` corresponde a qualquer caractere único.

* O caractere `*` corresponde a zero ou mais caracteres.

* O caractere `#` corresponde a qualquer dígito único (0-9).

* Uma lista de caracteres entre colchetes (`[ab...]`) corresponde a qualquer caractere único na lista.

* Uma lista de caracteres entre colchetes e prefixado por um ponto de exclamação (`[!ab...]`) corresponde a qualquer caractere único não está na lista de caracteres.

* Dois caracteres em uma lista de caracteres separados por um hífen (`-`) Especifique um intervalo de Unicode caracteres começando com o primeiro caractere e terminando com o segundo caractere. Se o segundo caractere não for mais tarde na ordem de classificação que o primeiro caractere, ocorrerá uma exceção de tempo de execução. Um hífen que aparece no início ou no final de uma lista de caracteres Especifica em si.

Para corresponder o colchete esquerdo de caracteres especiais (`[`), ponto de interrogação (`?`), sinal numérico (`#`) e o asterisco (`*`), colchetes necessário colocá-los. O colchete direito (`]`) não pode ser usado dentro de um grupo para corresponder em si, mas pode ser usado fora de um grupo como um caractere individual. A sequência de caracteres `[]` é considerado como a cadeia de caracteres literal `""`. 

Observe que as comparações de cadeias de caracteres e a ordenação para listas de caractere são dependentes do tipo de comparações que está sendo usado. Se estiverem sendo usadas comparações binárias, comparações de cadeias de caracteres e a ordenação baseiam-se nos valores Unicode numéricos. Se as comparações de texto estiverem sendo usadas, comparações de cadeias de caracteres e a ordenação baseiam-se na localidade atual que está sendo usada no .NET Framework.

Em alguns idiomas, os caracteres especiais nos alfabeto representam dois caracteres distintos e vice-versa. Por exemplo, vários idiomas usam o caractere `æ` para representar os caracteres `a` e `e` quando eles forem exibidos juntos, enquanto os caracteres `^` e `O` pode ser usado para representar o caractere `Ô`. Ao usar comparações de cadeias de texto, o `Like` operador reconhece esses equivalências culturais. Nesse caso, uma ocorrência de um único caractere especial no padrão ou cadeia de caracteres corresponde a sequência de dois caracteres equivalente em outra cadeia de caracteres. Da mesma forma, um caractere especial no padrão entre colchetes (por si só, em uma lista ou em um intervalo) corresponde à sequência de dois caracteres equivalente na cadeia de caracteres e vice-versa.

Em um `Like` expressão em que ambos os operandos forem `Nothing` ou um operando tem uma conversão intrínseco `String` e o outro operando for `Nothing`, `Nothing` é tratado como se fosse a cadeia de caracteres vazia literal `""`.

__Tipo de operação:__

|        | __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __bO__ | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __SB__ |    | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __Por__ |    |    | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __Sh__ |    |    |    | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __NÓS__ |    |    |    |    | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __In__ |    |    |    |    |    | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __INTERFACE DO USUÁRIO__ |    |    |    |    |    |    | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __Lo__ |    |    |    |    |    |    |    | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __Alemanha__ |    |    |    |    |    |    |    |    |    | ST | ST | ST | ST | ST | ST | Ob | 
| __si__ |    |    |    |    |    |    |    |    |    |    | ST | ST | ST | ST | ST | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | ST | ST | ST | ST | Ob | 
| __Acelerador de desenvolvimento__ |    |    |    |    |    |    |    |    |    |    |    |    | ST | ST | ST | Ob | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |    | ST | ST | Ob | 
| __ST__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | ST | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Ob | 


## <a name="concatenation-operator"></a>Operador de concatenação

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

O *operador de concatenação* está definido para todos os tipos intrínsecos, incluindo as versões que permitem valor nulas dos tipos de valor intrínseco. Ele também é definido para concatenação entre os tipos mencionados acima e `System.DBNull`, que é tratado como um `Nothing` cadeia de caracteres. O operador de concatenação converte todos os seus operandos para `String`; na expressão, todas as conversões para `String` são considerados para ampliação, independentemente se a semântica estrita é usada. Um `System.DBNull` valor é convertido para o literal `Nothing` digitado como `String`. Um tipo de valor anulável cujo valor é `Nothing` também é convertido para o literal `Nothing` digitado como `String`, em vez de gerar um erro de tempo de execução.

Uma operação de concatenação resulta em uma cadeia de caracteres que é a concatenação dos dois operandos na ordem da esquerda para a direita. O valor `Nothing` é tratado como se fosse a cadeia de caracteres vazia literal `""`.

__Tipo de operação:__

|        | __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __bO__ | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __SB__ |    | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __Por__ |    |    | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __Sh__ |    |    |    | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __NÓS__ |    |    |    |    | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __In__ |    |    |    |    |    | ST | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __INTERFACE DO USUÁRIO__ |    |    |    |    |    |    | ST | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __Lo__ |    |    |    |    |    |    |    | ST | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | ST | ST | ST | ST | ST | ST | ST | Ob | 
| __Alemanha__ |    |    |    |    |    |    |    |    |    | ST | ST | ST | ST | ST | ST | Ob | 
| __si__ |    |    |    |    |    |    |    |    |    |    | ST | ST | ST | ST | ST | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | ST | ST | ST | ST | Ob | 
| __Acelerador de desenvolvimento__ |    |    |    |    |    |    |    |    |    |    |    |    | ST | ST | ST | Ob | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |    | ST | ST | Ob | 
| __ST__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | ST | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Ob | 


## <a name="logical-operators"></a>Operadores Lógicos

O `And`, `Not`, `Or`, e `Xor` operadores são chamados de operadores lógicos.

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

Os operadores lógicos são avaliados da seguinte maneira:

* Para o `Boolean` tipo:

  * Uma lógica `And` operação é executada em dois operandos.

  * Uma lógica `Not` operação é executada em seu operando.

  * Uma lógica `Or` operação é executada em dois operandos.

  * Uma lógica exclusiva -`Or` operação é executada em dois operandos.

* Para `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`e todos os tipos enumerados, a operação especificada é executada em cada bit da representação binária das dois de operador (es):

  * `And`: O bit de resultado será 1 se os dois bits forem 1; Caso contrário, o bit de resultado é 0.

  * `Not`: O bit de resultado será 1 se o bit for 0; Caso contrário, o bit de resultado será 1.

  * `Or`: O bit de resultado será 1 se qualquer bit for 1; Caso contrário, o bit de resultado é 0.

  * `Xor`: O bit de resultado será 1 se qualquer bit for 1, mas não ambos os bits; Caso contrário, o bit de resultado é 0 (ou seja, 1 `Xor` 1 = 0, 1 `Xor` 1 = 0).

* Quando os operadores lógicos `And` e `Or` são elevadas para o tipo `Boolean?`, eles são estendidos para abranger o booliana lógica de três valores assim:

  * `And` é avaliada como true se ambos os operandos forem true; False se um dos operandos for false; `Nothing` caso contrário.

  * `Or` é avaliada como true se qualquer operando for true; False é que ambos os operandos forem falsos; `Nothing` caso contrário.

Por exemplo:

```vb
Module Test
    Sub Main()
        Dim x?, y? As Boolean

        x = Nothing
        y = True 

        If x Or y Then
            ' Will execute
        End If
    End Sub
End Module
```

__Observação.__ O ideal é que os operadores lógicos `And` e `Or` seria eliminada usando lógica de três valores para qualquer tipo que pode ser usado em uma expressão booleana (ou seja, um tipo que implementa `IsTrue` e `IsFalse`), da mesma forma que `AndAlso` e `OrElse` curto-circuito em qualquer tipo que pode ser usado em uma expressão booleana. Infelizmente, três valores levantando só será aplicada aos `Boolean?`, portanto, tipos definidos pelo usuário que desejam a lógica de três valores devem fazer isso manualmente, definindo `And` e `Or` operadores para a versão que permite valor nula.

Não há estouros são possíveis dessas operações. Os operadores de tipo enumerado fazer a operação bit a bit no tipo subjacente do tipo enumerado, mas o valor de retorno é o tipo enumerado.

__Tipo de operação:__

| __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| bO | SB | Por | Sh | US | No | Interface de Usuário | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Ob | 

__E, ou então, o tipo de operação Xor:__

|        | __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bO__ | bO | SB | Sh | Sh | No | No | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | bO  | Ob  | 
| __SB__ |    | SB | Sh | Sh | No | No | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Por__ |    |    | Por | Sh | US | No | Interface de Usuário | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Sh__ |    |    |    | Sh | No | No | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __NÓS__ |    |    |    |    | US | No | Interface de Usuário | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __In__ |    |    |    |    |    | No | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __INTERFACE DO USUÁRIO__ |    |    |    |    |    |    | Interface de Usuário | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Alemanha__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Ob  | 
| __Acelerador de desenvolvimento__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __ST__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="short-circuiting-logical-operators"></a>Operadores lógicos curto-circuito

O `AndAlso` e `OrElse` operadores são as versões Short-circuiting o `And` e `Or` operadores lógicos.

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

Por conta do comportamento curto-circuito, o segundo operando não será avaliado em tempo de execução se o resultado do operador for conhecido depois de avaliar o primeiro operando.

Os operadores lógicos Short-circuiting são avaliados da seguinte maneira:

* Se o primeiro operando em uma `AndAlso` operação é avaliada como `False` ou retorna verdadeiro de seu `IsFalse` operador, a expressão retorna o primeiro operando. Caso contrário, o segundo operando é avaliado e uma lógica `And` operação é executada em dois resultados.

* Se o primeiro operando em uma `OrElse` operação é avaliada como `True` ou retorna verdadeiro de seu `IsTrue` operador, a expressão retorna o primeiro operando. Caso contrário, o segundo operando é avaliado e uma lógica `Or` operação é executada em seus resultados de dois.

O `AndAlso` e `OrElse` operadores são definidos para o tipo `Boolean`, ou para qualquer tipo `T` que sobrecarrega os operadores a seguir:

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

bem como a sobrecarga correspondente `And` ou `Or` operador:

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

Ao avaliar a `AndAlso` ou `OrElse` operadores, o primeiro operando é avaliado apenas uma vez, e o segundo operando não é avaliado ou avaliado apenas uma vez. Por exemplo, considere o seguinte código:

```vb
Module Test
    Function TrueValue() As Boolean
        Console.Write(" True")
        Return True
    End Function

    Function FalseValue() As Boolean
        Console.Write(" False")
        Return False
    End Function

    Sub Main()
        Console.Write("And:")
        If FalseValue() And TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("Or:")
        If TrueValue() Or FalseValue() Then
        End If
        Console.WriteLine()

        Console.Write("AndAlso:")
        If FalseValue() AndAlso TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("OrElse:")
        If TrueValue() OrElse FalseValue() Then
        End If
        Console.WriteLine()
    End Sub
End Module
```

Imprime o seguinte resultado:

```
And: False True
Or: True False
AndAlso: False
OrElse: True
```

Na forma de elevadas a `AndAlso` e `OrElse` operadores, se o primeiro operando for um valor nulo `Boolean?`, em seguida, o segundo operando é avaliado, mas o resultado é sempre um valor nulo `Boolean?`.

__Tipo de operação:__

|        | __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bO__ | bO | bO | bO | bO | bO | bO | bO | bO | bO | bO | bO | bO | Err | Err | bO  | Ob  | 
| __SB__ |    | bO | bO | bO | bO | bO | bO | bO | bO | bO | bO | bO | Err | Err | bO  | Ob  | 
| __Por__ |    |    | bO | bO | bO | bO | bO | bO | bO | bO | bO | bO | Err | Err | bO  | Ob  | 
| __Sh__ |    |    |    | bO | bO | bO | bO | bO | bO | bO | bO | bO | Err | Err | bO  | Ob  | 
| __NÓS__ |    |    |    |    | bO | bO | bO | bO | bO | bO | bO | bO | Err | Err | bO  | Ob  | 
| __In__ |    |    |    |    |    | bO | bO | bO | bO | bO | bO | bO | Err | Err | bO  | Ob  | 
| __INTERFACE DO USUÁRIO__ |    |    |    |    |    |    | bO | bO | bO | bO | bO | bO | Err | Err | bO  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | bO | bO | bO | bO | bO | Err | Err | bO  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | bO | bO | bO | bO | Err | Err | bO  | Ob  | 
| __Alemanha__ |    |    |    |    |    |    |    |    |    | bO | bO | bO | Err | Err | bO  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | bO | bO | Err | Err | bO  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | bO | Err | Err | bO  | Ob  | 
| __Acelerador de desenvolvimento__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __ST__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | bO  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


## <a name="shift-operators"></a>Operadores Shift

Os operadores binários `<<` e `>>` executar operações de mudança de bits.

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

Os operadores são definidos para o `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` e `Long` tipos. Ao contrário dos outros operadores binários, o tipo de resultado de uma operação de deslocamento é determinado como se o operador foi um operador unário com apenas o operando esquerdo. O tipo do operando direito deve ser implicitamente conversível para `Integer` e não é usada para determinar o tipo de resultado da operação.

O `<<` operador faz com que os bits no primeiro operando a ser deslocado à esquerda do número de casas especificado pelo valor de deslocamento. Os bits de ordem superior fora do intervalo do tipo de resultado são descartados e as posições de bits vagas de ordem inferior são preenchidos com zeros.

O `>>` faz com que o operador os bits no primeiro operando a ser deslocado com o botão direito o número de casas especificado pelo valor de deslocamento. Os bits de ordem inferior são descartados e as posições de bits vagas de ordem superior são definidas como zero se o operando esquerdo for positivo ou a um negativo. Se o operando esquerdo for do tipo `Byte`, `UShort`, `UInteger`, ou `ULong` os bits de ordem superior vagas são preenchidas com zeros.

Os operadores shift desviam os bits da representação subjacente do primeiro operando pelo valor do segundo operando. Se o valor do segundo operando é maior que o número de bits no primeiro operando, ou é negativo, o valor de deslocamento é computado como `RightOperand And SizeMask` onde `SizeMask` é:

| __Tipo de LeftOperand__  | __SizeMask__ | 
|-----------------------|--------------|
| `Byte`, `SByte`       | 7 (`&H7`)    | 
| `UShort`, `Short`     | 15 (`&HF`)   | 
| `UInteger`, `Integer` | 31 (`&H1F`)  | 
| `ULong`, `Long`       | 63 (`&H3F`)  | 

Se o valor de deslocamento for zero, o resultado da operação é idêntico ao valor do primeiro operando. Não há estouros são possíveis dessas operações.

__Tipo de operação:__


| __bO__ | __SB__ | __Por__ | __Sh__ | __NÓS__ | __In__ | __INTERFACE DO USUÁRIO__ | __Lo__ | __UL__ | __Alemanha__ | __si__ | __Do__ | __Acelerador de desenvolvimento__  | __CH__  | __ST__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | Por | Sh | US | No | Interface de Usuário | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Ob | 


## <a name="boolean-expressions"></a>Expressões boolianas

Uma expressão booleana é uma expressão que pode ser testada para ver se ela for verdadeira ou se for falsa.

```antlr
BooleanExpression
    : Expression
    ;
```

Um tipo `T` pode ser usado em uma expressão booleana se, na ordem de preferência:

* `T` é `Boolean` ou `Boolean?`

* `T` tem uma conversão de ampliação `Boolean`

* `T` tem uma conversão de ampliação `Boolean?`

* `T` define dois operadores de pseudo `IsTrue` e `IsFalse`.

* `T` tem uma conversão de estreitamento `Boolean?` que não envolve uma conversão de `Boolean` para `Boolean?`.

* `T` tem uma conversão redutora para `Boolean`.

__Observação.__ É interessante observar que, se `Option Strict` estiver desativado, uma expressão que tem uma conversão de estreitamento `Boolean` serão aceitas sem um erro de tempo de compilação, mas a linguagem ainda preferirão um `IsTrue` operador se ele existir. Isso ocorre porque `Option Strict` altera apenas o que é e não é aceito pela linguagem e nunca altera o significado real de uma expressão. Portanto, `IsTrue` deve sempre ser preferível em vez de uma conversão de redução, independentemente de `Option Strict`.

Por exemplo, a seguinte classe não define uma conversão de ampliação para `Boolean`. Como resultado, seu uso na `If` instrução faz com que uma chamada para o `IsTrue` operador.

```vb
Class MyBool
    Public Shared Widening Operator CType(b As Boolean) As MyBool
        ...
    End Operator

    Public Shared Narrowing Operator CType(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsTrue(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsFalse(b As MyBool) As Boolean
        ...
    End Operator
End Class

Module Test
    Sub Main()
        Dim b As New MyBool

        If b Then Console.WriteLine("True")
    End Sub
End Module
```

Se uma expressão booleana for digitada como ou convertida em `Boolean` ou `Boolean?`, em seguida, ele é verdadeiro se o valor é `True` e false caso contrário.

Caso contrário, uma expressão booliana chama o `IsTrue` operador e retorna `True` se o operador retornado `True`; caso contrário, será false (mas nunca chama o `IsFalse` operador).

No exemplo a seguir, `Integer` tem uma conversão de estreitamento `Boolean`, portanto, um valor nulo `Integer?` tem uma conversão redutora para ambos `Boolean?` (produzindo um valor nulo `Boolean`) e, ao `Boolean` (que gera uma exceção). A conversão de estreitamento `Boolean?` é preferível e, portanto, o valor de "`i`" como um valor booliano a expressão é, portanto, `False`.

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a>Expressões lambda

Um *expressão lambda* define um método anônimo chamado um *método lambda*. Métodos de lambda tornam fácil passar métodos "na linha" para outros métodos que usam tipos de delegado.

```antlr
LambdaExpression
    : SingleLineLambda
    | MultiLineLambda
    ;

SingleLineLambda
    : LambdaModifier* 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? Expression
    | 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? Statement
    ;

MultiLineLambda
    : MultiLineFunctionLambda
    | MultiLineSubLambda
    ;

MultiLineFunctionLambda
    : LambdaModifier? 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? ( 'As' TypeName )? LineTerminator
      Block
      'End' 'Function'
    ;

MultiLineSubLambda
    : LambdaModifier? 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block
      'End' 'Sub'
    ;

LambdaModifier
    : 'Async' | 'Iterator'
    ;
```

O Exemplo:

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, Function(x As Integer) x * 2)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```

imprimirá:

```
2 4 6 8
```

Uma expressão lambda começa com os modificadores opcionais `Async` ou `Iterator`, seguido da palavra-chave `Function` ou `Sub` e uma lista de parâmetros. Parâmetros em uma expressão lambda não podem ser declarados `Optional` ou `ParamArray` e não pode ter atributos. Ao contrário dos métodos regulares, omitir um tipo de parâmetro para um método lambda não inferir automaticamente `Object`. Em vez disso, quando um método lambda é reclassificado, os tipos de parâmetro omitido e `ByRef` modificadores são inferidos do tipo de destino. No exemplo anterior, a expressão lambda poderia ter sido escrita como `Function(x) x * 2`, e ele seria inferido do tipo de `x` ser `Integer` quando o método lambda foi usado para criar uma instância das `IntFunc` tipo de delegado. Ao contrário de inferência de tipos de variável local, se um parâmetro de método lambda omite um tipo, mas tem uma matriz ou o modificador anulável nome, ocorrerá um erro de tempo de compilação.

Um __expressão lambda regular__ é aquela com nenhum dos dois `Async` nem `Iterator` modificadores.

Uma __expressão lambda de iterador__ é aquele com o `Iterator` modificador e nenhum `Async` modificador. Ele deve ser uma função. Quando ele é reclassificado como um valor, ele só pode ser reclassificado para um valor do tipo de delegado, cujo tipo de retorno é `IEnumerator`, ou `IEnumerable`, ou `IEnumerator(Of T)` ou `IEnumerable(Of T)` para alguns `T`, e que não tem nenhum parâmetro ByRef.

Uma __expressão lambda assíncrona__ é aquele com o `Async` modificador e nenhum `Iterator` modificador. Um lambda de sub async só pode ser reclassificado para um valor do tipo de delegado sub sem parâmetros ByRef. Um lambda de função async só pode ser reclassificado para um valor do tipo de delegado de função cujo tipo de retorno será `Task` ou `Task(Of T)` para alguns `T`, e que não tem nenhum parâmetro ByRef.

Expressões lambda podem ser linha única ou várias linhas. Linha única `Function` expressões lambda contenham uma única expressão que representa o valor retornado do método lambda. Linha única `Sub` expressões lambda contêm uma única instrução sem seu fechamento `StatementTerminator`. Por exemplo:

```vb
Module Test
    Sub Do(a() As Integer, action As Action(Of Integer))
        For index As Integer = 0 To a.Length - 1
            action(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Do(a, Sub(x As Integer) Console.WriteLine(x))
    End Sub
End Module
```

Linha única lambda constrói bind menos firmemente que todas as outras expressões e instruções. Assim, por exemplo, "`Function() x + 5`"é equivalente a"`Function() (x+5)"` em vez de"`(Function() x) + 5`". Para evitar ambiguidade, uma única linha `Sub` expressão lambda não pode conter uma instrução Dim ou uma instrução de declaração de rótulo. Além disso, a menos que ele é colocado entre parênteses, uma única linha `Sub` expressão lambda não pode ser seguido imediatamente por dois-pontos ":", um operador de acesso de membro ".", um operador de acesso de membro de dicionário "!" ou um parêntese de abertura "(". Ele não pode conter qualquer instrução do bloco (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) nem `OnError` nem `Resume`.

__Observação.__ Na expressão lambda `Function(i) x=i`, o corpo é interpretado como um *expressão* (quais testes se `x` e `i` são iguais). Mas, na expressão lambda `Sub(i) x=i`, o corpo é interpretado como uma instrução (que atribui `i` para `x`).

Uma expressão lambda de várias linhas contém um bloco de instruções e deve terminar com um número apropriado `End` instrução (ou seja, `End Function` ou `End Sub`). Assim como acontece com métodos regulares, um método lambda de várias linhas `Function` ou `Sub` instrução e `End` instruções devem ser em suas próprias linhas. Por exemplo:

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

Várias linhas `Function` expressões lambda pode declarar um tipo de retorno, mas não é possível colocar atributos nele. Se um multilinha `Function` expressão lambda não declara um tipo de retorno, mas o tipo de retorno pode ser inferido a partir do contexto no qual a expressão lambda é usada e, em seguida, esse tipo de retorno é usado. Caso contrário, o tipo de retorno da função é calculado da seguinte maneira:

* Em uma expressão lambda regular, o tipo de retorno é o tipo dominante das expressões em todos os `Return` instruções no bloco de instrução.

* Em uma expressão de lambda async, é o tipo de retorno `Task(Of T)` onde `T` é o tipo dominante das expressões em todos os `Return` instruções no bloco de instrução.

* Em uma expressão lambda de iterador, é o tipo de retorno `IEnumerable(Of T)` onde `T` é o tipo dominante das expressões em todos os `Yield` instruções no bloco de instrução.

Por exemplo:

```vb
Function f(min As Integer, max As Integer) As IEnumerable(Of Integer)
    If min > max Then Throw New ArgumentException()
    Dim x = Iterator Function()
                  For i = min To max
                    Yield i
                Next
               End Function

    ' infers x to be a delegate with return type IEnumerable(Of Integer)
    Return x()
End Function
```

Em todos os casos, se não houver nenhuma `Return` (respectivamente `Yield`) instruções, ou se não há nenhum tipo dominante entre eles e semântica estrita está sendo usada, ocorrerá um erro de tempo de compilação; caso contrário, o tipo dominante será implicitamente `Object`.

Observe que o tipo de retorno é calculado de todos os `Return` instruções, mesmo se eles não estão acessíveis. Por exemplo:

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

Não há nenhuma variável de retorno implícito, pois não há nenhum nome da variável.

Os blocos de instrução dentro de expressões lambda de várias linhas têm as seguintes restrições:

* `On Error` e `Resume` instruções não são permitidas, embora `Try` instruções são permitidas.

* Variáveis locais estáticas não podem ser declarados em expressões lambda de várias linhas.

* Não é possível ramificar para dentro ou fora do bloco de instrução de uma expressão lambda de várias linhas, embora as regras normais de ramificação aplicam dentro dele. Por exemplo:

  ```vb
  Label1:
  Dim x = Sub()
                 ' Error: Cannot branch out
                 GoTo Label1

                 ' OK: Wholly within the lamba.
                 GoTo Label2:
            Label2:
            End Sub

  ' Error: Cannot branch in
  GoTo Label2
  ```

Uma expressão lambda é aproximadamente equivalente a um método anônimo declarado no tipo recipiente. O exemplo inicial é aproximadamente equivalente a:

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Function $Lambda1(x As Integer) As Integer
        Return x * 2
    End Function

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, AddressOf $Lambda1)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```


### <a name="closures"></a>Fechamentos

Expressões lambda têm acesso a todas as variáveis no escopo, inclusive variáveis locais ou parâmetros definidos nas expressões lambda e de método recipientes. Quando uma expressão lambda faz referência a um parâmetro ou variável local, a expressão lambda captura a variável que está sendo chamada em um fechamento. Um fechamento é um objeto que reside no heap, em vez de na pilha e, quando uma variável é capturada, todas as referências à variável são redirecionadas para o fechamento. Isso permite que as expressões lambda continuar fazer referência a parâmetros e variáveis locais, mesmo depois que o método que contém for concluído. Por exemplo:

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D
        Dim x As Integer = 10
        Return Function() x
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

É aproximadamente equivalente a:

```vb
Module Test
    Delegate Function D() As Integer

    Class $Closure1
        Public x As Integer

        Function $Lambda1() As Integer
            Return x
        End Function
    End Class

    Function M() As D
        Dim c As New $Closure1()
        c.x = 10
        Return AddressOf c.$Lambda1
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

Um fechamento captura uma nova cópia de um local variável cada vez que ele entra no bloco em que a variável local é declarada, mas a nova cópia é inicializada com o valor da cópia anterior, se houver. Por exemplo:

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D()
        Dim a(9) As D

        For i As Integer = 0 To 9
            Dim x
            a(i) = Function() x
            x += 1
        Next i

        Return a
    End Function

    Sub Main()
        Dim y() As D = M()

        For i As Integer = 0 To 9
            Console.Write(y(i)() & " ")
        Next i
    End Sub
End Module
```

Imprime

```
1 2 3 4 5 6 7 8 9 10
```

Em vez de

```
9 9 9 9 9 9 9 9 9 9
```

Como fechamentos precisam ser inicializado ao inserir um bloco, ele não tem permissão para `GoTo` em um bloco com um fechamento de fora desse bloco, embora ele tem permissão para `Resume` em um bloco com um fechamento. Por exemplo:

```vb
Module Test
    Sub Main()
        Dim a = 10

        If a = 10 Then
L1:
            Dim x = Function() a

            ' Valid, source is within block
            GoTo L2
L2:
        End If

        ' ERROR: target is inside block with closure
        GoTo L1
    End Sub
End Module
```

Porque eles não podem ser capturados em um fechamento, o código a seguir não pode aparecer dentro de uma expressão lambda:

* Parâmetros de referência.

* Expressões de instância (`Me`, `MyClass`, `MyBase`), se o tipo de `Me` não é uma classe.

Os membros de uma expressão de criação de tipo anônimo, se a expressão lambda é parte da expressão. Por exemplo:

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

`ReadOnly` variáveis de instância em construtores de instância ou `ReadOnly` compartilhado variáveis em construtores compartilhados onde as variáveis são usadas em um contexto sem valor. Por exemplo:

```vb
Class C1
    ReadOnly F1 As Integer

    Sub New()
        ' Valid, doesn't modify F1
        Dim x = Function() F1

        ' Error, tries to modify F1
        Dim f = Function() ModifyValue(F1)
    End Sub

    Sub ModifyValue(ByRef x As Integer)
    End Sub
End Class
```

## <a name="query-expressions"></a>Expressões de consulta

Um *expressão de consulta* é uma expressão que se aplica a uma série de *operadores de consulta* aos elementos de um *passível de consulta* coleção. Por exemplo, a expressão a seguir usa uma coleção de `Customer` objetos e retorna os nomes de todos os clientes no estado de Washington:

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

Uma expressão de consulta deve começar com uma `From` ou um `Aggregate` operador e pode terminar com qualquer operador de consulta. O resultado de uma expressão de consulta é classificado como um valor. o tipo de resultado da expressão depende o tipo de resultado do último operador de consulta na expressão.

```antlr
QueryExpression
    : FromOrAggregateQueryOperator QueryOperator*
    ;

FromOrAggregateQueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    ;

QueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    | SelectQueryOperator
    | DistinctQueryOperator
    | WhereQueryOperator
    | OrderByQueryOperator
    | PartitionQueryOperator
    | LetQueryOperator
    | GroupByQueryOperator
    | JoinOrGroupJoinQueryOperator
    ;

JoinOrGroupJoinQueryOperator
    : JoinQueryOperator
    | GroupJoinQueryOperator
    ;
```

### <a name="range-variables"></a>Variáveis de intervalo

Alguns operadores de consulta apresentam um tipo especial de variável chamada de um *variável de intervalo*. Variáveis de intervalo não são variáveis real; em vez disso, eles representam os valores individuais durante a avaliação da consulta ao longo de coleções de entrada.

```antlr
CollectionRangeVariableDeclarationList
    : CollectionRangeVariableDeclaration ( Comma CollectionRangeVariableDeclaration )*
    ;

CollectionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? 'In' LineTerminator? Expression
    ;

ExpressionRangeVariableDeclarationList
    : ExpressionRangeVariableDeclaration ( Comma ExpressionRangeVariableDeclaration )*
    ;

ExpressionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? Equals Expression
    ;
```

Variáveis de intervalo estão no escopo do operador de consulta introduzindo até o final de uma expressão de consulta, ou para um operador de consulta, como `Select` que oculta-os. Por exemplo, na consulta a seguir

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

o `From` operador apresenta uma variável de intervalo de consulta `cust` digitado como `Customer` que representa cada cliente no `Customers` coleção. O seguinte `Where` operador de consulta, em seguida, se refere à variável de intervalo `cust` na expressão de filtro para determinar se deve filtrar um cliente individual da coleção resultante.

Há dois tipos de variáveis de intervalo: *variáveis de intervalo de coleta* e *variáveis de intervalo de expressão*. Variáveis de intervalo da coleção levar seus valores dos elementos das coleções que estão sendo consultados. A expressão de coleção em uma declaração de variável de intervalo de coleta deve ser classificada como um valor cujo tipo é passível de consulta. Se o tipo de uma variável de intervalo de coleta for omitido, ele é inferido como o tipo de elemento da expressão de coleção, ou `Object` se a expressão de coleção não tem um tipo de elemento (ou seja, apenas define um `Cast` método). Se a expressão de coleção não é passível de consulta (ou seja, o tipo de elemento da coleção não pode ser inferido), resultados de um erro de tempo de compilação.

Uma variável de intervalo de expressão é uma variável de intervalo cujo valor é calculado por uma expressão em vez de uma coleção. No exemplo a seguir, o `Select` consulta operador apresenta uma variável de intervalo de expressão denominada `cityState` calculado a partir de dois campos:

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

Uma variável de intervalo de expressão não é necessário para fazer referência a outra variável de intervalo, embora essa variável pode ser de valor duvidosa. A expressão atribuída a uma variável de intervalo da expressão deve ser classificada como um valor e deve ser implicitamente conversível para o tipo da variável de intervalo, se fornecido.

Somente em um operador permitem uma variável de intervalo de expressão terá seu tipo especificado. Em outros operadores, ou se seu tipo não for especificado, então a inferência de tipo de variável local é usada para determinar o tipo da variável de intervalo.

Uma variável de intervalo deve seguir as regras para declarar variáveis locais em relação aos sombreamento. Portanto, uma variável de intervalo não é possível ocultar o nome de uma variável local ou parâmetro no método delimitador ou outra variável de intervalo (a menos que o operador de consulta especificamente oculta todas as variáveis de alcance atuais no escopo).


### <a name="queryable-types"></a>Tipos passíveis de consulta

Expressões de consulta são implementadas, convertendo a expressão em chamadas para métodos bem conhecidos em um tipo de coleção. Esses métodos bem-definidos definem o tipo de elemento da coleção passível de consulta, bem como os tipos de resultado dos operadores de consulta executadas na coleção. Cada operador de consulta Especifica o método ou métodos que o operador de consulta é geralmente traduzido, embora a conversão específica é dependente de implementação. Os métodos são fornecidos na especificação usando um formato geral que se parece com:

```vb
Function Select(selector As Func(Of T, R)) As CR
```

O seguinte se aplica aos métodos:

* O método deve ser uma instância ou um membro de extensão do tipo de coleção e deve ser acessível.

* O método pode ser genérico, desde que seja possível inferir a todos os argumentos de tipo.

* O método pode ser sobrecarregado, caso em que a resolução de sobrecarga é usada para determinar o método usar.

* Outro tipo de delegado pode ser usado no lugar de delegado `Func` de tipo, desde que ele tem a mesma assinatura, incluindo o tipo de retorno, como a correspondência `Func` tipo.

* O tipo `System.Linq.Expressions.Expression(Of D)` pode ser usado no lugar de delegado `Func` tipo, desde que `D` é um tipo de delegado que tem a mesma assinatura, incluindo o tipo de retorno, como a correspondência `Func` tipo.

* O tipo `T` representa o tipo de elemento da coleção de entrada. Todos os métodos definidos por um tipo de coleção devem ter o mesmo tipo de elemento de entrada para o tipo de coleção a ser passível de consulta.

* O tipo `S` representa o tipo de elemento da segunda coleção de entrada, no caso de operadores de consulta que executar junções.

* O tipo `K` representa um tipo de chave no caso de operadores de consulta que têm um conjunto de variáveis de intervalo que agem como chaves.

* O tipo `N` representa um tipo que é usado como um tipo numérico (embora ele ainda poderia ser um tipo definido pelo usuário e não um tipo numérico intrínseco).

* O tipo `B` representa um tipo que pode ser usado em uma expressão booleana.

* O tipo `R` representa o tipo de elemento da coleção de resultado, se o operador de consulta produz um conjunto de resultados. `R` depende do número de variáveis de intervalo no escopo na conclusão do operador de consulta. Se uma variável de intervalo único está no escopo, em seguida, `R` é o tipo dessa variável de intervalo. No exemplo

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  o resultado da consulta será um tipo de coleção com um tipo de elemento `String`. Se diversas variáveis de intervalo estão no escopo, então `R` é um tipo anônimo que contém todas as variáveis de intervalo no escopo de `Key` campos. No exemplo:

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  o resultado da consulta será um tipo de coleção com um tipo de elemento de um tipo anônimo com uma propriedade somente leitura denominada `Name` do tipo `String` e uma propriedade somente leitura chamada `ProductName` do tipo `String`.

  Dentro de uma expressão de consulta, são gerados para conter variáveis de intervalo de tipos anônimos *transparente*, que significa que variáveis de alcance sempre está disponíveis sem qualificação. Por exemplo, no exemplo anterior as variáveis de intervalo `c` e `o` podem ser acessados sem qualificação no `Select` consultar operador, mesmo que o tipo de elemento da coleção de entrada foi um tipo anônimo.

* O tipo `CX` representa um tipo de coleção, não necessariamente o tipo de coleção de entrada, cujo tipo de elemento é algum tipo `X`.

Um tipo de coleção passível de consulta deve atender a uma das condições a seguir, em ordem de preferência:

* Ele deve definir uma em conformidade com `Select` método.

* Ele deve ter um dos métodos a seguir

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  que pode ser chamado para obter uma coleção passível de consulta. Se ambos os métodos são fornecidos, `AsQueryable` é preferível `AsEnumerable`.

* Ele deve ter um método

  ```vb
  Function Cast(Of T)() As CT
  ```

  que pode ser chamado com o tipo da variável de intervalo para produzir uma coleção passível de consulta.

Como determinar o tipo de elemento de uma coleção ocorre independentemente de uma invocação de método real, não é possível determinar a aplicabilidade de métodos específicos. Portanto, ao determinar o tipo de elemento de uma coleção, se houver métodos de instância que correspondem aos métodos bem conhecidos, quaisquer métodos de extensão que correspondem aos métodos bem conhecidos são ignorados.

Conversão de operador de consulta ocorre na ordem em que os operadores de consulta ocorrem na expressão. Não é necessário para um objeto de coleção implementar todos os métodos necessários para todos os operadores de consulta, embora cada objeto da coleção deve suportar pelo menos o `Select` operador de consulta. Se um método necessário não estiver presente, ocorrerá um erro de tempo de compilação. Ao associar nomes de método conhecido, não métodos são ignorados para fins de herança múltipla em interfaces e o método de extensão de associação, embora a semântica de sombreamento ainda se aplicam. Por exemplo:

```vb
Class Q1
    Public Function [Select](selector As Func(Of Integer, Integer)) As Q1
    End Function
End Class

Class Q2
    Inherits Q1

    Public [Select] As Integer
End Class

Module Test
    Sub Main()
        Dim qs As New Q2()

        ' Error: Q2.Select still hides Q1.Select
        Dim zs = From q In qs Select q
    End Sub
End Module
```

### <a name="default-query-indexer"></a>Indexador de consulta padrão

Cada tipo de coleção consultável cujo tipo de elemento é `T` e ainda não tiver um padrão propriedade é considerada como tendo uma propriedade padrão no seguinte formato geral:

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

A propriedade padrão só pode ser referida usando a sintaxe de acesso de propriedade padrão; a propriedade padrão não pode ser referenciada por nome. Por exemplo:

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

Se o tipo de coleção não tem um `ElementAtOrDefault` membro, um erro de tempo de compilação ocorrerá.

### <a name="from-query-operator"></a>Operador de consulta

O `From` consulta operador apresenta uma variável de intervalo da coleção que representa os membros individuais de uma coleção a ser consultado.

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

Por exemplo, a expressão de consulta:

```vb
From c As Customer In Customers ...
```

pode ser pensada como equivalente a

```vb
For Each c As Customer In Customers
        ...
Next c
```

Quando um `From` operador de consulta declara coleta várias variáveis de intervalo ou não é a primeira `From` é do operador de consulta na expressão de consulta, cada nova variável de intervalo de coleta *cruzada Unido* ao conjunto existente de variáveis de intervalo. O resultado é que a consulta é avaliada sobre o produto cruzado de todos os elementos nas coleções associadas. Por exemplo, a expressão:

```vb
From c In Customers _
From e In Employees _
...
```

pode ser pensada como equivalente à:

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

e é exatamente equivalente a:

```vb
From c In Customers, e In Employees ...
```

As variáveis de intervalo introduzidas nos operadores de consulta anterior podem ser usadas em uma posterior `From` operador de consulta. Por exemplo, na expressão de consulta a seguir o segundo `From` operador de consulta se refere ao valor da variável de intervalo primeiro:

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

Diversas variáveis de intervalo em um `From` múltiplos ou operador de consulta `From` operadores de consulta são suportados apenas se o tipo de coleção contém uma ou mais dos seguintes métodos:

```vb
Function SelectMany(selector As Func(Of T, CR)) As CR
Function SelectMany(selector As Func(Of T, CS), _
                          resultsSelector As Func(Of T, S, R)) As CR
```

O código

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs, y In ys ...
```

é geralmente traduzido para

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

__Observação.__ `From` não é uma palavra reservada.


### <a name="join-query-operator"></a>Operador de consulta JOIN

O `Join` consulta operador une as variáveis de intervalo existentes com uma nova variável de intervalo de coleta, produzindo uma única coleção cujos elementos foram Unidos juntos com base em uma expressão de igualdade.

```antlr
JoinQueryOperator
    : LineTerminator? 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
    ;

JoinConditionList
    : JoinCondition ( 'And' LineTerminator? JoinCondition )*
    ;

JoinCondition
    : Expression 'Equals' LineTerminator? Expression
    ;
```

Por exemplo:

```vb
Dim customersAndOrders = _
    From cust In Customers _
    Join ord In Orders On cust.ID Equals ord.CustomerID
```

A expressão de igualdade é mais restrita do que uma expressão regular de igualdade:

* As duas expressões devem ser classificadas como um valor.

* As duas expressões devem fazer referência a pelo menos uma variável de intervalo.

* A variável de intervalo declarado na junção operador de consulta deve ser referenciada por uma das expressões, e que a expressão não deve fazer referência qualquer outra variável de intervalo.

Se os tipos das duas expressões não são exatamente do mesmo tipo, em seguida

* Se o operador de igualdade é definido para os dois tipos, as duas expressões são implicitamente conversíveis para ele e não é `Object`, em seguida, converter as duas expressões para aquele tipo.

* Caso contrário, se houver um tipo dominante que ambas as expressões podem ser convertidas implicitamente em, converta as duas expressões para aquele tipo.

* Caso contrário, ocorrerá um erro de tempo de compilação.

As expressões são comparadas usando valores de hash (ou seja, chamando `GetHashCode()`) em vez de usar operadores de igualdade para maior eficiência. Um `Join` operador de consulta pode fazer várias junções ou condições de igualdade no mesmo operador. Um `Join` operador de consulta é suportado apenas se o tipo de coleção contém um método:

```vb
Function Join(inner As CS, _
                  outerSelector As Func(Of T, K), _
                  innerSelector As Func(Of S, K), _
                  resultSelector As Func(Of T, S, R)) As CR
```

O código

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Join y In ys On x Equals y _
            ...
```

é geralmente traduzido para

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.Join( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

__Observação.__ `Join`, `On` e `Equals` não são palavras reservadas.


### <a name="let-query-operator"></a>Permitir que o operador de consulta

O `Let` operador apresenta uma variável de intervalo de expressão de consulta. Isso permite calcular um valor intermediário depois que será usado várias vezes nos operadores de consulta posteriores.

```antlr
LetQueryOperator
    : LineTerminator? 'Let' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Por exemplo:

```vb
Dim taxedPrices = _
    From o In Orders _
    Let tax = o.Price * 0.088 _
    Where tax > 3.50 _
    Select o.Price, tax, total = o.Price + tax
```

pode ser pensada como equivalente à:

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

Um `Let` operador de consulta é suportado apenas se o tipo de coleção contém um método:

```vb
Function Select(selector As Func(Of T, R)) As CR
```

O código

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Let y = x * 10 _
            ...
```

é geralmente traduzido para

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a>Selecione o operador de consulta

O `Select` operador de consulta é como o `Let` operador de consulta em que ele apresenta as variáveis de intervalo de expressão; no entanto, um `Select` consulta operador oculta as variáveis de intervalo disponível no momento, em vez de adicioná-los. Além disso, o tipo de uma variável de intervalo de expressão introduzido por um `Select` operador de consulta é sempre inferido usando regras de inferência de tipo de variável local; um tipo explícito não pode ser especificado e se nenhum tipo pode ser inferido, ocorrerá um erro de tempo de compilação.

```antlr
SelectQueryOperator
    : LineTerminator? 'Select' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Por exemplo, na consulta:

```vb
Dim smiths = _
    From cust In Customers _
    Select name = cust.name _
    Where name.EndsWith("Smith")
```

o `Where` operador de consulta tem acesso apenas à `name` variável de intervalo introduzido pelo `Select` operador; se o `Where` operador tentasse se referir `cust`, teria ocorrido um erro de tempo de compilação.

Em vez de especificar explicitamente os nomes das variáveis de intervalo, um `Select` consulta operador pode inferir os nomes das variáveis de intervalo, usando as mesmas regras como expressões de criação de objeto do tipo anônimo. Por exemplo:

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

Se o nome da variável de intervalo não for fornecido e um nome não pode ser inferido, ocorrerá um erro de tempo de compilação. Se o `Select` operador de consulta contém apenas uma única expressão, nenhum erro ocorrerá se um nome para essa variável de intervalo não pode ser inferido, mas a variável de intervalo é sem nome.  Por exemplo:

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

Se não houver uma ambiguidade em um `Select` operador de consulta entre atribuir um nome a uma variável de intervalo e uma expressão de igualdade, a atribuição de nome é preferencial. Por exemplo:

```vb
Dim badCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select name = c.Name ' Creates a range variable named "name"


Dim goodCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select match = (name = c.Name)
```

Cada expressão no `Select` operador de consulta deve ser classificada como um valor. Um `Select` operador de consulta tem suporte apenas se o tipo de coleção contém um método:

```vb
Function Select(selector As Func(Of T, R)) As CR
```

O código

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Select x, y = x * 10 _
            ...
```

é geralmente traduzido para

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a>Operador de consulta DISTINCT

O `Distinct` consulta operador restringe os valores em uma coleção somente para aqueles com valores distintos, conforme determinado comparando o tipo de elemento quanto à igualdade.

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

Por exemplo, a consulta:

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

retornará apenas uma linha para cada emparelhamento distintos de preço de nome e a ordem do cliente, mesmo se o cliente tiver vários pedidos com o mesmo preço. Um `Distinct` operador de consulta tem suporte apenas se o tipo de coleção contém um método:

```vb
Function Distinct() As CT
```

O código

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Distinct _
            ...
```

é geralmente traduzido para

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

__Observação.__ `Distinct` não é uma palavra reservada.


### <a name="where-query-operator"></a>Em que o operador de consulta

O `Where` operador restringe os valores em uma coleção para aqueles que atendem a uma determinada condição de consulta.

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

Um `Where` operador de consulta usa uma expressão booliana que é avaliada para cada conjunto de valores de variável de intervalo; se o valor da expressão for true, os valores aparecem na coleção de saída, caso contrário, os valores são ignorados. Por exemplo, a expressão de consulta:

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

pode ser pensada como equivalente para o loop aninhado

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

Um `Where` operador de consulta tem suporte apenas se o tipo de coleção contém um método:

```vb
Function Where(predicate As Func(Of T, B)) As CT
```

O código

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Where x < 10 _
            ...
```

é geralmente traduzido para

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

__Observação.__ `Where` não é uma palavra reservada.


### <a name="partition-query-operators"></a>Operadores de consulta de partição

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

O `Take` consultar resultados do operador no primeiro `n` elementos de uma coleção. Quando usado com o `While` modificador, o `Take` resultados do operador no primeiro `n` elementos de uma coleção que atendem uma expressão booliana. O `Skip` operador ignora os primeiros `n` elementos de uma coleção e, em seguida, retorna o resto da coleção.  Quando usado em conjunto com o `While` modificador, o `Skip` operador ignora os primeiros `n` elementos de uma coleção que atendem a uma expressão booliana e, em seguida, retorna o restante da coleção. As expressões em uma `Take` ou `Skip` operador de consulta deve ser classificada como um valor.

Um `Take` operador de consulta tem suporte apenas se o tipo de coleção contém um método:

```vb
Function Take(count As N) As CT
```

Um `Skip` operador de consulta tem suporte apenas se o tipo de coleção contém um método:

```vb
Function Skip(count As N) As CT
```

Um `Take While` operador de consulta tem suporte apenas se o tipo de coleção contém um método:

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

Um `Skip While` operador de consulta tem suporte apenas se o tipo de coleção contém um método:

```vb
Function SkipWhile(predicate As Func(Of T, B)) As CT
```

O código

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Skip 10 _
            Take 5 _
            Skip While x < 10 _
            Take While x > 5 _
            ...
```

é geralmente traduzido para

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

__Observação.__ `Take` e `Skip` não são palavras reservadas.


### <a name="order-by-query-operator"></a>Ordem pelo operador de consulta

O `Order By` operador de consulta classifica os valores que aparecem nas variáveis de intervalo. 

```antlr
OrderByQueryOperator
    : LineTerminator? 'Order' 'By' LineTerminator? OrderExpressionList
    ;

OrderExpressionList
    : OrderExpression ( Comma OrderExpression )*
    ;

OrderExpression
    : Expression Ordering?
    ;

Ordering
    : 'Ascending' | 'Descending'
    ;
```

Um `Order By` operador de consulta usa expressões que especificam os valores de chave que devem ser usados para ordenar as variáveis de iteração. Por exemplo, a consulta a seguir retorna produtos classificados por preço:

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

Uma ordenação pode ser marcada como `Ascending`, caso em que valores menores vêm antes de valores maiores, ou `Descending`, caso em que os valores maiores vêm antes valores menores. O padrão se nenhuma for especificada uma ordenação é `Ascending`. Por exemplo, a consulta a seguir retorna os produtos classificados pelo preço com o produto mais caro primeiro:

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

O `Order By` consulta operador pode especificar várias expressões de ordenação, caso em que a coleção é ordenada de forma aninhada. Por exemplo, a consulta a seguir ordena os clientes por estado, em seguida, por cidade dentro de cada estado e, em seguida, por CEP em cada cidade:

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

As expressões em um `Order By` operador de consulta deve ser classificada como um valor. Um `Order By` operador de consulta tem suporte apenas se o tipo de coleção contém uma ou mais dos seguintes métodos:

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

O tipo de retorno `CT` deve ser um *coleção ordenada*. Uma coleção ordenada é um tipo de coleção que contém um ou ambos os métodos:

```vb
Function ThenBy(keySelector As Func(Of T, K)) As CT
Function ThenByDescending(keySelector As Func(Of T, K)) As CT
```

O código

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Order By x Ascending, x Mod 2 Descending _
            ...
```

é geralmente traduzido para

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

__Observação.__ Porque os operadores de consulta simplesmente mapeiam sintaxe para os métodos que implementam uma operação de consulta específica, a preservação da ordem não é determinada pelo idioma e é determinada pela implementação do próprio operador. Isso é muito semelhante aos operadores definidos pelo usuário em que a implementação para sobrecarregar o operador de adição para um tipo numérico definido pelo usuário não pode executar qualquer coisa que se assemelha a uma adição. É claro que, para preservar a previsibilidade, implementar algo que não coincide com as expectativas dos usuários não é recomendado.

__Observação.__ `Order` e `By` não são palavras reservadas.


### <a name="group-by-query-operator"></a>Agrupar por operador de consulta

O `Group By` as variáveis de intervalo no escopo com base em uma ou mais expressões de grupos de operador de consulta e, em seguida, gera novas variáveis de alcance com base em um desses agrupamentos.

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Por exemplo, a consulta a seguir agrupa todos os clientes por `State`e, em seguida, calcula a idade de contagem e média de cada grupo:

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

O `Group By` operador tem três cláusulas de consulta: opcional `Group` cláusula, a `By` cláusula e o `Into` cláusula. O `Group` cláusula tem a mesma sintaxe e o efeito de uma `Select` consultar operador, exceto que ele afeta somente as variáveis de intervalo disponíveis na `Into` cláusula e não o `By` cláusula. Por exemplo:

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

O `By` cláusula declara as variáveis de intervalo são usadas como valores de chave na operação de agrupamento de expressão. O `Into` cláusula permite a declaração da expressão de variáveis de intervalo que calculam agregações em cada um dos grupos formados pelo `By` cláusula. Dentro de `Into` cláusula, a variável de intervalo de expressão pode ser atribuída somente uma expressão que é uma invocação de método de um *função de agregação*. Uma função de agregação é uma função no tipo de coleção do grupo (que pode não ser necessariamente o mesmo tipo de coleção da coleção original), que se parece com qualquer um dos seguintes métodos:

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

Se uma função de agregação usa um argumento do delegado, a expressão de invocação pode ter uma expressão de argumento que deve ser classificada como um valor.  A expressão de argumento pode usar as variáveis de intervalo que estão no escopo. dentro da chamada para uma função de agregação, essas variáveis de intervalo representam os valores no grupo sendo formada, nem todos os valores na coleção. Por exemplo, no exemplo original nesta seção o `Average` função calcula a média de idades de clientes por estado, em vez de todos os clientes juntos.

Todos os tipos de coleção são considerados como tendo a função de agregação `Group` definidos nele, que não usa nenhum parâmetro e simplesmente retorna o grupo. Outras funções de agregação padrão que pode fornecer a um tipo de coleção são:

`Count` e `LongCount`, qual retornar a contagem de elementos no grupo ou a contagem dos elementos no grupo que atendem uma expressão booliana. `Count` e `LongCount` são suportados apenas se o tipo de coleção contém um dos métodos:

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

`Sum`, que retorna a soma de uma expressão em todos os elementos no grupo. `Sum` há suporte para apenas se o tipo de coleção contiver um dos métodos:

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

`Min` Retorna o valor mínimo de uma expressão em todos os elementos no grupo. `Min` há suporte para apenas se o tipo de coleção contiver um dos métodos:

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

`Max`, que retorna o valor máximo de uma expressão em todos os elementos no grupo. `Max` há suporte para apenas se o tipo de coleção contiver um dos métodos:

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

`Average`, que retorna a média de uma expressão em todos os elementos no grupo. `Average` há suporte para apenas se o tipo de coleção contiver um dos métodos:

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

`Any`, que determina se um grupo contém membros ou se uma expressão booliana for verdadeira para qualquer elemento no grupo. `Any` Retorna um valor que pode ser usado em uma expressão booleana e tem suporte apenas se o tipo de coleção contém um dos métodos:

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

`All`, que determina se uma expressão booleana é verdadeira para todos os elementos no grupo. `All` Retorna um valor que pode ser usado em uma expressão booleana e tem suporte apenas se o tipo de coleção contém um método:

```vb
Function All(predicate As Func(Of T, B)) As B
```

Depois de um `Group By` operador de consulta, as variáveis de intervalo anteriormente no escopo são ocultadas, e as variáveis de intervalo introduzidos pelo `By` e `Into` cláusulas estão disponíveis. Um `Group By` operador de consulta tem suporte apenas se o tipo de coleção contém o método:

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

Em declarações de variável de intervalo de `Group` cláusula são suportados apenas se o tipo de coleção contém o método:

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      elementSelector As Func(Of T, S), _
                      resultSelector As Func(Of K, CS, R)) As CR
```

O código

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Group y = x * 10, z = x / 10 By evenOdd = x Mod 2 _
            Into Sum(y), Average(z) _
            ...
```

é geralmente traduzido para

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.GroupBy( _
        Function(x As Integer) x Mod 2, _
        Function(x As Integer) New With {.y = x * 10, .z = x / 10}, _
        Function(evenOdd, group) New With { _
            evenOdd, _
            .Sum = group.Sum(Function(e) e.y), _
            .Average = group.Average(Function(e) e.z)})...
```

__Observação.__ `Group`, `By`, e `Into` não são palavras reservadas.


### <a name="aggregate-query-operator"></a>Operador de consulta de agregação

O `Aggregate` operador de consulta executa uma função semelhante como o `Group By` operador, exceto que ele permite que os grupos que já tiverem sido formados, agregando. Porque o grupo de já ter sido formado, o `Into` cláusula de um `Aggregate` operador não oculta as variáveis de intervalo no escopo de consulta (dessa forma, `Aggregate` é mais parecido com um `Let`, e `Group By` é mais parecido com um `Select`).

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Por exemplo, a consulta a seguir agrega o total de todos os pedidos feitos pelos clientes em Washington:

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

O resultado dessa consulta é uma coleção de tipo cujo elemento é um tipo anônimo com uma propriedade denominada `cust` tipada como `Customer` e uma propriedade chamada `Sum` digitada como `Integer`.

Diferentemente `Group By`, os operadores de consulta adicionais podem ser colocados entre o `Aggregate` e `Into` cláusulas. Entre um `Aggregate` cláusula e o término do `Into` cláusula, todas as variáveis de intervalo em escopo, incluindo aqueles declarados pelo `Aggregate` cláusula pode ser usada. Por exemplo, as seguinte consulta agregações colocado a soma total de todos os pedidos por clientes em Washington antes de 2006:

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

O `Aggregate` operador também pode ser usado para iniciar uma expressão de consulta. Nesse caso, o resultado da expressão de consulta será o único valor calculado pelo `Into` cláusula. Por exemplo, a consulta a seguir calcula a soma de todos os totais de ordem antes de 1º de janeiro de 2006:

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

O resultado da consulta é um único `Integer` valor. Um `Aggregate` operador de consulta está sempre disponível (embora a função de agregação deve ser também está disponível para a expressão a ser válido). O código

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

é geralmente traduzido para

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

__Observação.__ `Aggregate` e `Into` não são palavras reservadas.


### <a name="group-join-query-operator"></a>Operador de consulta de junção de grupo

O `Group Join` operador de consulta combina as funções do `Join` e `Group By` operadores de consulta em um único operador. `Group Join` une duas coleções com base na correspondência de chaves extraídas de elementos, agrupar todos os elementos que correspondem a um determinado elemento no lado esquerdo da junção no lado direito da junção. Portanto, o operador produz um conjunto de resultados hierárquicos.

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Por exemplo, a consulta a seguir produz elementos que contêm o nome de um único cliente, um grupo de todos os seus pedidos e a quantidade total de todos os pedidos:

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

O resultado da consulta é uma coleção cujo tipo de elemento é um tipo anônimo com três propriedades: `Name`tipada como `String`, `Orders` digitada como uma coleção cujo tipo de elemento é `Order`, e `OrdersTotal`, digitado como `Integer`. Um `Group Join` operador de consulta tem suporte apenas se o tipo de coleção contém o método:

```vb
Function GroupJoin(inner As CS, _
                         outerSelector As Func(Of T, K), _
                         innerSelector As Func(Of S, K), _
                         resultSelector As Func(Of T, CS, R)) As CR
```

O código

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Group Join y in ys On x Equals y _
            Into g = Group _
            ...
```

é geralmente traduzido para

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.GroupJoin( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x, group) New With {x, .g = group})...
```

__Observação.__ `Group`, `Join`, e `Into` não são palavras reservadas.


## <a name="conditional-expressions"></a>Expressões condicionais

Uma condicional `If` expressão testa uma expressão e retorna um valor.

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

Ao contrário de `IIF` função de tempo de execução, no entanto, uma expressão condicional só avalia seus operandos se necessário. Assim, por exemplo, a expressão `If(c Is Nothing, c.Name, "Unknown")` não lançará uma exceção se o valor da `c` é `Nothing`. A expressão condicional tem duas formas: um que usa dois operandos e outro que usa três operandos.

Se três operandos forem fornecidos, todos os três expressões devem ser classificadas como valores, e o primeiro operando deve ser uma expressão booliana. Se o resultado é a expressão for verdadeira, a segunda expressão será o resultado do operador, caso contrário, a terceira expressão vai ser o resultado do operador. O tipo de resultado da expressão é o tipo dominante entre os tipos da segunda e terceira expressão. Se não houver nenhum tipo dominante, ocorrerá um erro de tempo de compilação.

Se dois operandos forem fornecidos, ambos os operandos devem ser classificados como valores, e o primeiro operando deve ser um tipo de referência ou um tipo de valor anulável. A expressão `If(x, y)` , em seguida, é avaliada como se foi a expressão `If(x IsNot Nothing, x, y)`, com duas exceções. Em primeiro lugar, a primeira expressão é só avaliada uma vez e em segundo lugar, se o tipo do segundo operando é um tipo de valor não anulável e tipo do primeiro operando for, o `?` é removido do tipo do primeiro operando ao determinar o tipo dominante para o tipo de resultado da expressão. Por exemplo:

```vb
Module Test
    Sub Main()
        Dim x?, y As Integer
        Dim a?, b As Long

        a = If(x, a)        ' Result type: Long?
        y = If(x, 0)        ' Result type: Integer
    End Sub
End Module
```

Em ambos os formatos de expressão, se um operando for `Nothing`, seu tipo não é usado para determinar o tipo dominante. No caso da expressão `If(<expression>, Nothing, Nothing)`, o tipo dominante é considerado `Object`.


## <a name="xml-literal-expressions"></a>Expressões literais do XML

Uma expressão literal do XML representa um XML (eXtensible Markup Language) valor de 1,0.

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

O resultado de uma expressão literal do XML é um valor tipado como um dos tipos do `System.Xml.Linq` namespace. Se os tipos no namespace não estiverem disponíveis, em seguida, uma expressão literal do XML causará um erro de tempo de compilação. Os valores são gerados por meio de chamadas de construtor convertidas da expressão literal XML. Por exemplo, o código:

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

é aproximadamente equivalente ao código:

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

Uma expressão literal do XML pode assumir a forma de um documento XML, um elemento XML, uma instrução de processamento de XML, um comentário XML ou uma seção CDATA.

__Observação.__ Essa especificação contém apenas suficiente de uma descrição de XML para descrever o comportamento da linguagem Visual Basic. Para obter mais informações sobre o XML podem ser encontradas em http://www.w3.org/TR/REC-xml/.


### <a name="lexical-rules"></a>Regras lexicais

```antlr
XMLCharacter
    : '<Unicode tab character (0x0009)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode carriage return character (0x000D)>'
    | '<Unicode characters 0x0020 - 0xD7FF>'
    | '<Unicode characters 0xE000 - 0xFFFD>'
    | '<Unicode characters 0x10000 - 0x10FFFF>'
    ;

XMLString
    : XMLCharacter+
    ;

XMLWhitespace
    : XMLWhitespaceCharacter+
    ;

XMLWhitespaceCharacter
    : '<Unicode carriage return character (0x000D)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode space character (0x0020)>'
    | '<Unicode tab character (0x0009)>'
    ;

XMLNameCharacter
    : XMLLetter
    | XMLDigit
    | '.'
    | '-'
    | '_'
    | ':'
    | XMLCombiningCharacter
    | XMLExtender
    ;

XMLNameStartCharacter
    : XMLLetter
    | '_'
    | ':'
    ;

XMLName
    : XMLNameStartCharacter XMLNameCharacter*
    ;

XMLLetter
    : '<Unicode character as defined in the Letter production of the XML 1.0 specification>'
    ;

XMLDigit
    : '<Unicode character as defined in the Digit production of the XML 1.0 specification>'
    ;

XMLCombiningCharacter
    : '<Unicode character as defined in the CombiningChar production of the XML 1.0 specification>'
    ;

XMLExtender
    : '<Unicode character as defined in the Extender production of the XML 1.0 specification>'
    ;
```

Expressões literais do XML são interpretadas usando regras lexicais de XML em vez de regras lexicais de regular código do Visual Basic. Geralmente, os dois conjuntos de regras diferem das seguintes maneiras:

* Espaço em branco é significativo em XML. Como resultado, a gramática de expressões literais do XML explicitamente estados em que o espaço em branco é permitido. Espaço em branco não é preservado, exceto quando ele ocorre no contexto de dados de caracteres dentro de um elemento. Por exemplo:

  ```vb
  ' The following element preserves no whitespace
  Dim e1 = _
      <customer>
          <name>Bob</>
      </>

  ' The following element preserves all of the whitespace
  Dim e2 = _
      <customer>
          Bob
      </>
  ```

* Espaço em branco finais de linha XML é normalizado de acordo com a especificação do XML.

* XML diferencia maiúsculas de minúsculas. Palavras-chave devem corresponder maiusculas e minúsculas exatamente, caso contrário, ocorrerá um erro de tempo de compilação.

* Terminadores de linha são considerados espaço em branco em XML. Como resultado, nenhum caractere de continuação de linha é necessários em expressões literais do XML.

* XML não aceita caracteres de largura total. Se forem usados caracteres de largura inteira, ocorrerá um erro de tempo de compilação.


### <a name="embedded-expressions"></a>Expressões inseridas

Expressões literais do XML podem conter *expressões incorporadas*. Uma expressão inserida é uma expressão do Visual Basic que é avaliada e usada para preencher um ou mais valores no local de expressão incorporada.

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

Por exemplo, o código a seguir coloca a cadeia de caracteres `John Smith` como o valor do elemento XML:

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

Expressões podem ser inseridas em um número de contextos. Por exemplo, o código a seguir gera um elemento denominado `customer`:

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

Cada contexto onde uma expressão inserida pode ser usada Especifica os tipos que serão aceitos. Quando estiver dentro do contexto da parte de expressão de uma expressão inserida, as regras normais de léxicas para código do Visual Basic ainda aplicam isso, por exemplo, as continuações das linhas devem ser usadas:

```vb
' Visual Basic expression uses line continuation, XML does not
Dim element As System.Xml.Linq.XElement = _
    <<%= name & _
          name %>>John 
                     Smith</>
```


### <a name="xml-documents"></a>Documentos XML

```antlr
XMLDocument
    : XMLDocumentPrologue XMLMisc* XMLDocumentBody XMLMisc*
    ;

XMLDocumentPrologue
    : '<' '?' 'xml' XMLVersion XMLEncoding? XMLStandalone? XMLWhitespace? '?' '>'
    ;

XMLVersion
    : XMLWhitespace 'version' XMLWhitespace? '=' XMLWhitespace? XMLVersionNumberValue
    ;

XMLVersionNumberValue
    : SingleQuoteCharacter '1' '.' '0' SingleQuoteCharacter
    | DoubleQuoteCharacter '1' '.' '0' DoubleQuoteCharacter
    ;

XMLEncoding
    : XMLWhitespace 'encoding' XMLWhitespace? '=' XMLWhitespace? XMLEncodingNameValue
    ;

XMLEncodingNameValue
    : SingleQuoteCharacter XMLEncodingName SingleQuoteCharacter
    | DoubleQuoteCharacter XMLEncodingName DoubleQuoteCharacter
    ;

XMLEncodingName
    : XMLLatinAlphaCharacter XMLEncodingNameCharacter*
    ;

XMLEncodingNameCharacter
    : XMLUnderscoreCharacter
    | XMLLatinAlphaCharacter
    | XMLNumericCharacter
    | XMLPeriodCharacter
    | XMLDashCharacter
    ;

XMLLatinAlphaCharacter
    : '<Unicode Latin alphabetic character (0x0041-0x005a, 0x0061-0x007a)>'
    ;

XMLNumericCharacter
    : '<Unicode digit character (0x0030-0x0039)>'
    ;

XMLHexNumericCharacter
    : XMLNumericCharacter
    | '<Unicode Latin hex alphabetic character (0x0041-0x0046, 0x0061-0x0066)>'
    ;

XMLPeriodCharacter
    : '<Unicode period character (0x002e)>'
    ;

XMLUnderscoreCharacter
    : '<Unicode underscore character (0x005f)>'
    ;

XMLDashCharacter
    : '<Unicode dash character (0x002d)>'
    ;

XMLStandalone
    : XMLWhitespace 'standalone' XMLWhitespace? '=' XMLWhitespace? XMLYesNoValue
    ;

XMLYesNoValue
    : SingleQuoteCharacter XMLYesNo SingleQuoteCharacter
    | DoubleQuoteCharacter XMLYesNo DoubleQuoteCharacter
    ;

XMLYesNo
    : 'yes'
    | 'no'
    ;

XMLMisc
    : XMLComment
    | XMLProcessingInstruction
    | XMLWhitespace
    ;

XMLDocumentBody
    : XMLElement
    | XMLEmbeddedExpression
    ;
```

Um documento XML resulta em um valor digitado como `System.Xml.Linq.XDocument`. Ao contrário da especificação XML 1.0, os documentos XML em expressões literais do XML são necessárias para especificar o prólogo do documento XML; Expressões literais do XML sem o prólogo do documento XML são interpretadas como suas entidades individuais. Por exemplo:

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

Um documento XML pode conter uma expressão inserida cujo tipo pode ser qualquer tipo; em tempo de execução, no entanto, o objeto deve satisfazer os requisitos do `XDocument` construtor ou um erro de tempo de execução ocorrerá.

Ao contrário de XML regular, expressões de documento XML não têm suporte DTDs (declarações de tipo de documento). Além disso, o atributo de codificação, se fornecido, será ignorado, pois a codificação da expressão literal Xml é sempre o mesmo que a codificação do arquivo de origem em si.

__Observação.__ Embora o atributo de codificação é ignorado, ele é o atributo ainda são válido para manter a capacidade de incluir todos os documentos Xml 1.0 válidos no código-fonte.


### <a name="xml-elements"></a>Elementos XML

```antlr
XMLElement
    : XMLEmptyElement
    | XMLElementStart XMLContent XMLElementEnd
    ;

XMLEmptyElement
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '/' '>'
    ;

XMLElementStart
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '>'
    ;

XMLElementEnd
    : '<' '/' '>'
    | '<' '/' XMLQualifiedName XMLWhitespace? '>'
    ;

XMLContent
    : XMLCharacterData? ( XMLNestedContent XMLCharacterData? )+
    ;

XMLCharacterData
    : '<Any XMLCharacterDataString that does not contain the string "]]>">'
    ;

XMLCharacterDataString
    : '<Any Unicode character except < or &>'+
    ;

XMLNestedContent
    : XMLElement
    | XMLReference
    | XMLCDATASection
    | XMLProcessingInstruction
    | XMLComment
    | XMLEmbeddedExpression
    ;

XMLAttribute
    : XMLWhitespace XMLAttributeName XMLWhitespace? '=' XMLWhitespace? XMLAttributeValue
    | XMLWhitespace XMLEmbeddedExpression
    ;

XMLAttributeName
    : XMLQualifiedNameOrExpression
    | XMLNamespaceAttributeName
    ;

XMLAttributeValue
    : DoubleQuoteCharacter XMLAttributeDoubleQuoteValueCharacter* DoubleQuoteCharacter
    | SingleQuoteCharacter XMLAttributeSingleQuoteValueCharacter* SingleQuoteCharacter
    | XMLEmbeddedExpression
    ;

XMLAttributeDoubleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or DoubleQuoteCharacter>'
    | XMLReference
    ;

XMLAttributeSingleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or SingleQuoteCharacter>'
    | XMLReference
    ;

XMLReference
    : XMLEntityReference
    | XMLCharacterReference
    ;

XMLEntityReference
    : '&' XMLEntityName ';'
    ;

XMLEntityName
    : 'lt' | 'gt' | 'amp' | 'apos' | 'quot'
    ;

XMLCharacterReference
    : '&' '#' XMLNumericCharacter+ ';'
    | '&' '#' 'x' XMLHexNumericCharacter+ ';'
    ;
```

Um elemento XML resulta em um valor digitado como `System.Xml.Linq.XElement`. Ao contrário de XML regular, elementos XML podem omitir o nome na marca de fechamento e elemento aninhado mais atual será fechado. Por exemplo:

```vb
Dim name = <name>Bob</>
```

Declarações em um resultado do elemento XML em valores de tipo de atributo `System.Xml.Linq.XAttribute`. Valores de atributo são normalizados de acordo com a especificação do XML. Quando o valor de um atributo é `Nothing` o atributo não será criado, portanto, não terá a expressão de valor do atributo a ser verificada para `Nothing`. Por exemplo:

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

Elementos e atributos XML podem conter expressões aninhadas nos seguintes locais:

O nome do elemento, nesse caso, a expressão inserida deve ser um valor de um tipo implicitamente conversível para `System.Xml.Linq.XName`. Por exemplo:

```vb
Dim name = <<%= "name" %>>Bob</>
```

O nome de um atributo do elemento, nesse caso, a expressão inserida deve ser um valor de um tipo implicitamente conversível para `System.Xml.Linq.XName`. Por exemplo:

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

O valor de um atributo do elemento, nesse caso a expressão inserida pode ser um valor de qualquer tipo. Por exemplo:

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

Um atributo do elemento, nesse caso a expressão inserida pode ser um valor de qualquer tipo. Por exemplo:

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

O conteúdo do elemento, nesse caso a expressão inserida pode ser um valor de qualquer tipo. Por exemplo:

```vb
Dim name = <name><%= "Bob" %></>
```

Se for o tipo de expressão inserida `Object()`, a matriz será passada como um paramarray para o `XElement` construtor.


### <a name="xml-namespaces"></a>Namespaces XML

Elementos XML podem conter declarações de namespace XML, conforme definido pela especificação XML 1.0 de namespaces.

```antlr
XMLNamespaceAttributeName
    : XMLPrefixedNamespaceAttributeName
    | XMLDefaultNamespaceAttributeName
    ;

XMLPrefixedNamespaceAttributeName
    : 'xmlns' ':' XMLNamespaceName
    ;

XMLDefaultNamespaceAttributeName
    : 'xmlns'
    ;

XMLNamespaceName
    : XMLNamespaceNameStartCharacter XMLNamespaceNameCharacter*
    ;

XMLNamespaceNameStartCharacter
    : '<Any XMLNameCharacter except :>'
    ;

XMLNamespaceNameCharacter
    : XMLLetter
    | '_'
    ;

XMLQualifiedNameOrExpression
    : XMLQualifiedName
    | XMLEmbeddedExpression
    ;

XMLQualifiedName
    : XMLPrefixedName
    | XMLUnprefixedName
    ;

XMLPrefixedName
    : XMLNamespaceName ':' XMLNamespaceName
    ;

XMLUnprefixedName
    : XMLNamespaceName
    ;
```

As restrições sobre como definir os namespaces `xml` e `xmlns` são impostas e gerarão erros de tempo de compilação. Declarações de namespace XML não podem ter uma expressão inserida para seu valor; o valor fornecido deve ser um literal de cadeia não vazia. Por exemplo:

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

__Observação.__ Essa especificação contém apenas suficiente de uma descrição do namespace XML para descrever o comportamento da linguagem Visual Basic. Para obter mais informações sobre namespaces XML podem ser encontradas em http://www.w3.org/TR/REC-xml-names/.

Nomes de elementos e atributos XML podem ser qualificados com o uso de nomes de namespace. Namespaces são associados como no XML regular, com a exceção que importa qualquer namespace declarada no nível de arquivo são considerados para ser declarada em um contexto de circunscrição a declaração, que é colocada por qualquer importação de namespace declarada pela compilação ambiente. Se um nome de namespace não for encontrado, ocorrerá um erro de tempo de compilação. Por exemplo:

```vb
Imports System.Xml.Linq
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        ' Binds to the imported namespace above.
        Dim c1 = <db:customer>Bob</>

        ' Binds to the namespace declaration in the element
        Dim c2 = _
            <db:customer xmlns:db="http://example.org/database-other">Mary</>

        ' Binds to the inner namespace declaration
        Dim c3 = _
            <database xmlns:db="http://example.org/database-one">
                <db:customer xmlns:db="http://example.org/database-two">Joe</>
            </>

        ' Error: namespace db2 cannot be found
        Dim c4 = _
            <db2:customer>Jim</>
    End Sub
End Module
```

Namespaces XML declarados em um elemento não se aplicam aos literais XML dentro de expressões inseridas. Por exemplo:

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

__Observação.__ Isso ocorre porque a expressão inserida pode ser qualquer coisa, incluindo uma chamada de função. Se a chamada de função continha uma expressão literal do XML, ele não está claro se os programadores esperaria que o namespace de XML a ser aplicado ou ignorado.


### <a name="xml-processing-instructions"></a>Instruções de processamento de XML

Uma instrução de processamento de XML resulta em um valor digitado como `System.Xml.Linq.XProcessingInstruction`. Instruções de processamento de XML não podem conter expressões incorporadas, como eles são uma sintaxe válida dentro de instrução de processamento.

```antlr
XMLProcessingInstruction
    : '<' '?' XMLProcessingTarget ( XMLWhitespace XMLProcessingValue? )? '?' '>'
    ;

XMLProcessingTarget
    : '<Any XMLName except a casing permutation of the string "xml">'
    ;

XMLProcessingValue
    : '<Any XMLString that does not contain a question-mark followed by ">">'
    ;
```

### <a name="xml-comments"></a>Comentários XML

Um comentário XML resulta em um valor digitado como `System.Xml.Linq.XComment`. Comentários XML não podem conter expressões incorporadas, como eles são uma sintaxe válida com o comentário.

```antlr
XMLComment
    : '<' '!' '-' '-' XMLCommentCharacter* '-' '-' '>'
    ;

XMLCommentCharacter
    : '<Any XMLCharacter except dash (0x002D)>'
    | '-' '<Any XMLCharacter except dash (0x002D)>'
    ;
```

### <a name="cdata-sections"></a>Seções CDATA

Uma seção CDATA resulta em um valor digitado como `System.Xml.Linq.XCData`. As seções CDATA não podem conter expressões incorporadas, como eles são uma sintaxe válida dentro da seção CDATA.

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a>Expressões de acesso de membro XML

Uma expressão de acesso de membro XML acessa os membros de um valor XML.

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

Há três tipos de expressões de acesso de membro XML:

* *Acesso de elemento*, no qual um nome XML segue um único ponto. Por exemplo:

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  Acesso de elemento é mapeado para a função:

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  Portanto, o exemplo acima é equivalente a:

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* *Acesso de atributo*, no qual um identificador Visual Basic segue um ponto e no logon ou um XML o nome segue um ponto e um sinal de arroba. Por exemplo:

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  Mapas de acesso de atributo para a função:

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  Portanto, o exemplo acima é equivalente a:

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  __Observação.__ O `AttributeValue` método de extensão (bem como a propriedade de extensão relacionados `Value`) não está definido atualmente em qualquer assembly. Se os membros de extensão são necessários, eles são automaticamente definidos no assembly que está sendo produzido.

* *Acesso de seus descendentes*, no qual um nomes XML segue nos três pontos. Por exemplo:

  ```vb
  Dim company = _
      <company>
          <customers>
              <customer>Bob</>
              <customer>Mary</>
              <customer>Joe</>
          </>
      </>
  Dim customers = company...<customer>
  ```

  Acesso de seus descendentes é mapeado para a função:

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  Portanto, o exemplo acima é equivalente a:

  ```vb
  Dim customers = company.Descendants("customer")
  ```

A expressão de base de uma expressão de acesso de membro XML deve ser um valor e deve ser do tipo:

* Se um elemento ou seus descendentes acessar, `System.Xml.Linq.XContainer` ou um tipo derivado, ou `System.Collections.Generic.IEnumerable(Of T)` ou um tipo derivado, onde `T` é `System.Xml.Linq.XContainer` ou um tipo derivado.

* Se um acesso de atributo `System.Xml.Linq.XElement` ou um tipo derivado, ou `System.Collections.Generic.IEnumerable(Of T)` ou um tipo derivado, onde `T` é `System.Xml.Linq.XElement` ou um tipo derivado.

Nomes em expressões de acesso de membro XML não podem estar vazios. Eles podem ser qualificado, usando qualquer namespace definido pelo importações de namespace. Por exemplo:

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim customer = _
            <db:customer>
                <db:name>Bob</>
            </>
        Dim name = customer.<db:name>
    End Sub
End Module
```

Espaço em branco não é permitido após o dot(s) em uma expressão de acesso de membro XML ou entre os colchetes angulares e o nome. Por exemplo:

```vb
Dim customer = _
    <customer age="30">
        <name>Bob</>
    </>
' All the following are error cases
Dim age = customer.@ age
Dim name = customer.< name >
Dim names = customer...< name >
```

Se os tipos no `System.Xml.Linq` namespace não estão disponíveis e, em seguida, uma expressão de acesso de membro XML causará um erro de tempo de compilação.


## <a name="await-operator"></a>Operador Await

O operador de espera está relacionado a métodos assíncronos, que são descritos na seção [métodos assíncronos](statements.md#async-methods).

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

`Await` é uma palavra reservada se a expressão de lambda ou método imediatamente delimitador no qual ele aparece tem um `Async` modificador e se o `Await` aparece depois que `Async` modificador; é não reservado em outro lugar. Também é reservado em diretivas de pré-processador. O operador de espera só é permitido no corpo de um método ou expressões lambda em que ele é uma palavra reservada. Dentro do método ou lambda imediatamente delimitador, uma expressão await não pode ocorrer dentro do corpo de uma `Catch` ou `Finally` bloquear nou dentro o corpo de um `SyncLock` instrução nem dentro uma expressão de consulta.

O operador await toma uma única expressão que deve ser classificado como um valor e cujo tipo deve ser um *aguardável* tipo, ou `Object`. Se seu tipo for `Object` e em seguida, todo o processamento é adiada até o tempo de execução. Um tipo `C` será considerada awaitable, se todos os itens a seguir forem verdadeiras:

* `C` contém um método de extensão ou de instância acessível nomeado `GetAwaiter` que não tiver nenhum argumento e que retorne algum tipo `E`;

* `E` contém uma propriedade de instância ou extensão legível chamada `IsCompleted` que não leva argumentos e tem um tipo booleano;

* `E` contém um método de extensão ou de instância acessível chamado `GetResult` que não usa argumentos;

* `E` implemente `System.Runtime.CompilerServices.INotifyCompletion` ou `ICriticalNotifyCompletion`.

Se `GetResult` era um `Sub`, em seguida, a expressão await é classificada como void. Caso contrário, a expressão await é classificada como um valor e seu tipo é o tipo de retorno de `GetResult` método.

Aqui está um exemplo de uma classe que pode ser colocada em espera:

```vb
Class MyTask(Of T)
    Function GetAwaiter() As MyTaskAwaiter(Of T)
        Return New MyTaskAwaiter With {.m_Task = Me}
    End Function

    ...
End Class

Structure MyTaskAwaiter(Of T)
    Implements INotifyCompletion

    Friend m_Task As MyTask(Of T)

    ReadOnly Property IsCompleted As Boolean
        Get
            Return m_Task.IsCompleted
        End Get
    End Property

    Sub OnCompleted(r As Action) Implements INotifyCompletion.OnCompleted
        ' r is the "resumptionDelegate"
        Dim sc = SynchronizationContext.Current
        If sc Is Nothing Then
            m_Task.ContinueWith(Sub() r())
        Else
            m_Task.ContinueWith(Sub() sc.Post(Sub() r(), Nothing))
        End If
    End Sub

    Function GetResult() As T
        If m_Task.IsCanceled Then Throw New TaskCanceledException(m_Task)
        If m_Task.IsFaulted Then Throw m_Task.Exception.InnerException
        Return m_Task.Result
    End Function
End Structure
```

__Observação.__ Autores de biblioteca são recomendados para seguir o padrão de invocação de delegado de continuação no mesmo `SynchronizationContext` como seus `OnCompleted` foi invocado em. Além disso, o delegado de continuação não deve ser executado em forma síncrona o `OnCompleted` método, pois isso pode levar a estouro de pilha: em vez disso, o delegado deve ser enfileirado para execução subsequente.

Quando o controle fluxo atinge um `Await` operador, o comportamento é o seguinte.

1.  O `GetAwaiter` método do operando await é invocado. O resultado da invocação this é chamado de *awaiter*.

2.  O awaiter `IsCompleted` propriedade é recuperada. Se o resultado for true, em seguida:

    21. O `GetResult` método de awaiter é invocado. Se `GetResult` era uma função, em seguida, o valor da expressão await é o valor de retorno dessa função.

3.  Se a propriedade IsCompleted não for true, em seguida:

    31. Qualquer um dos `ICriticalNotifyCompletion.UnsafeOnCompleted` é invocado no awaiter (se o tipo do awaiter `E` implementa `ICriticalNotifyCompletion`) ou `INotifyCompletion.OnCompleted` (caso contrário). Em ambos os casos passa um *delegado de continuação* associado à instância atual do método assíncrono.

    32. O ponto de controle da instância de método assíncrono atual está suspenso e controlar fluxo retoma na *chamador atual* (definidas na seção [métodos assíncronos](statements.md#async-methods)).

    33. Se o delegado de continuação é invocado posteriormente,

        331. o delegado de continuação primeiro restaura `System.Threading.Thread.CurrentThread.ExecutionContext` ao que era no momento `OnCompleted` foi chamado,
        332. em seguida, ele retoma o fluxo de controle no ponto de controle da instância de método assíncrono (consulte a seção [métodos assíncronos](statements.md#async-methods)),
        333. em que ele chama o `GetResult` método awaiter, como no 2.1 acima.

Se o operando da expressão await tem o tipo de objeto, em seguida, esse comportamento é adiado até o tempo de execução:

- Etapa 1 é realizada pela chamada GetAwaiter() sem argumentos; ele pode se associar, portanto, em tempo de execução para métodos de instância que usam parâmetros opcionais.
- Etapa 2 é realizada, recuperando a propriedade IsCompleted() sem argumentos e pela tentativa de uma conversão intrínseco em booliano.
- Etapa 3.a é realizado pela tentativa `TryCast(awaiter, ICriticalNotifyCompletion)`, e se isso falhar, em seguida, `DirectCast(awaiter, INotifyCompletion)`.

O delegado de continuação passado 3.a pode ser invocado somente uma vez. Se for chamado mais de uma vez, o comportamento será indefinido.

