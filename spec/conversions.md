# <a name="conversions"></a>Conversões

Conversão é o processo de alteração de um valor de um tipo para outro. Por exemplo, um valor do tipo `Integer` pode ser convertido em um valor do tipo `Double`, ou um valor do tipo `Derived` pode ser convertido em um valor do tipo `Base`, supondo que `Base` e `Derived` são classes e `Derived`herda de `Base`. Conversões de não exigir o valor para alterar (como o último exemplo), ou podem requerer alterações significativas na representação do valor (como no exemplo anterior).

Conversões podem ser de ampliação ou redução. Um *conversão de ampliação* é uma conversão de um tipo em outro tipo de domínio cujo valor é pelo menos tão grande, se não maior que o domínio do valor do tipo original. Conversões ampliadoras nunca devem falhar. Um *conversão de estreitamento* é uma conversão de um tipo em outro tipo de domínio cujo valor é menor do que do tipo original valor de domínio ou suficientemente não relacionadas que extra é necessário ter cuidado ao fazer a conversão (para Por exemplo, durante a conversão de `Integer` para `String`). Reduzir conversões, o que podem envolver a perda de informações, poderá falhar.

A conversão de identidade (ou seja, uma conversão de um tipo a mesmo) e a conversão do valor padrão (ou seja, uma conversão de `Nothing`) são definidos para todos os tipos.

## <a name="implicit-and-explicit-conversions"></a>Conversões implícitas e explícitas

As conversões podem ser *implícita* ou *explícita*. Conversões implícitas acontecem sem nenhuma sintaxe especial. A seguir está um exemplo de conversão implícita de um `Integer` de valor para um `Long` valor:

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

Conversões explícitas, por outro lado, exigem operadores de conversão. Tentativa de fazer uma conversão explícita em um valor sem um operador cast faz com que um erro de tempo de compilação. O exemplo a seguir usa uma conversão explícita para converter um `Long` de valor para um `Integer` valor.

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

O conjunto de conversões implícitas depende do ambiente de compilação e o `Option Strict` instrução. Se a semântica estrita estiverem sendo usada, somente conversões de expansão podem ocorrer implicitamente. Se estiverem sendo usada semântica permissiva, todos os de ampliar e reduzir conversões (em outras palavras, todas as conversões) podem ocorrer implicitamente.

## <a name="boolean-conversions"></a>Conversões de boolianos

Embora `Boolean` não é um tipo numérico, ele tem reduzir conversões de e para os tipos numéricos, como se fosse um tipo enumerado. O literal `True` converte o literal `255` para `Byte`, `65535` para `UShort`, `4294967295` para `UInteger`, `18446744073709551615` para `ULong`e a expressão `-1` para `SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, e `Double`. O literal `False` converte o literal `0`. Converte um valor zero numérico literal `False`. Todos os outros valores numéricos converter para literal `True`.

Há uma conversão redutora de booliano para a cadeia de caracteres, convertendo em qualquer um `System.Boolean.TrueString` ou `System.Boolean.FalseString`. Também há uma conversão redutora de `String` à `Boolean`: se a cadeia de caracteres era igual a `TrueString` ou `FalseString` (a cultura atual, case-insensitively), em seguida, ele usa o valor apropriado; caso contrário, ele tenta analisar a cadeia de caracteres como um tipo numérico (em hexadecimal ou octal se possível, caso contrário, como um float) e usa as regras acima; Caso contrário, ele lança `System.InvalidCastException`.

## <a name="numeric-conversions"></a>Conversões numéricas

Conversões numéricas existirem entre os tipos `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` e `Double`, e todos os tipos enumerados. Quando está sendo convertido, tipos enumerados são tratados como se fossem seus tipos base. Ao converter para um tipo enumerado, o valor de origem não é necessário estar de acordo com o conjunto de valores definidos no tipo enumerado. Por exemplo:

```vb
Enum Values
    One
    Two
    Three
End Enum

Module Test
    Sub Main()
        Dim x As Integer = 5

        ' OK, even though there is no enumerated value for 5.
        Dim y As Values = CType(x, Values)
    End Sub
End Module
```

Conversões numéricas são processadas em tempo de execução da seguinte maneira:

* Para uma conversão de um tipo numérico em um tipo numérico mais amplo, o valor é simplesmente convertido para o tipo mais amplo. Conversões de `UInteger`, `Integer`, `ULong`, `Long`, ou `Decimal` para `Single` ou `Double` são arredondados para mais próximo `Single` ou `Double` valor. Enquanto essa conversão pode causar uma perda de precisão, ele nunca fará com que uma perda de magnitude.

* Para uma conversão de um tipo integral para outro tipo integral ou de `Single`, `Double`, ou `Decimal` para um tipo integral, o resultado depende se a verificação de estouro de inteiro está em:

  *Se o estouro de inteiros que está sendo verificado:*

  * Se a fonte for um tipo integral, a conversão for bem-sucedida, se o argumento de origem está dentro do intervalo do tipo de destino. A conversão gera um `System.OverflowException` exceção se o argumento de origem está fora do intervalo do tipo de destino.

  * Se a fonte estiver `Single`, `Double`, ou `Decimal`, o valor de origem é arredondado para cima ou para baixo até o valor inteiro mais próximo, e esse valor integral se tornará o resultado da conversão. Se o valor de origem é igualmente perto de dois valores integrais, o valor é arredondado para o valor que tem um número par na posição de dígitos menos significativa. Se o valor integral resultante estiver fora do intervalo do tipo de destino, um `System.OverflowException` exceção é lançada.

  *Se o estouro de inteiro não está sendo verificado:*

  * Se a fonte for um tipo integral, a conversão sempre terá êxito e consiste apenas em Descartando os bits mais significativos do valor de source.

  * Se a fonte estiver `Single`, `Double`, ou `Decimal`, a conversão sempre terá êxito e consiste apenas o valor de origem para o valor integral mais próximo de arredondamento. Se o valor de origem é igualmente perto de dois valores integrais, o valor é sempre arredondado para o valor que tem um número par na posição de dígitos menos significativa.

* Para uma conversão de `Double` à `Single`, o `Double` valor é arredondado para mais próximo `Single` valor. Se o `Double` valor for muito pequeno para ser representado como um `Single`, o resultado se torna zero positivo ou negativo de zero. Se o `Double` valor é muito grande para ser representado como um `Single`, o resultado se torna o infinito positivo ou negativo infinito. Se o `Double` valor é `NaN`, o resultado também será `NaN`.

* Para uma conversão de `Single` ou `Double` à `Decimal`, o valor de origem é convertido em `Decimal` representação e arredondado para o próximo número após a 28ª casa decimal, se necessário. Se o valor de origem é muito pequeno para ser representado como um `Decimal`, o resultado será zero. Se o valor de origem estiver `NaN`, infinito, ou muito grande para ser representado como uma `Decimal`, um `System.OverflowException` exceção é lançada.

* Para uma conversão de `Double` à `Single`, o `Double` valor é arredondado para mais próximo `Single` valor. Se o `Double` valor for muito pequeno para ser representado como um `Single`, o resultado se torna zero positivo ou negativo de zero. Se o `Double` valor é muito grande para ser representado como um `Single`, o resultado se torna o infinito positivo ou negativo infinito. Se o `Double` valor é `NaN`, o resultado também será `NaN`.

## <a name="reference-conversions"></a>Conversões de referência

Tipos de referência podem ser convertidos em um tipo base e vice-versa. Conversões de um tipo base para um tipo mais derivado só ter êxito em tempo de execução se o valor que está sendo convertido for um valor nulo, o próprio tipo derivado ou um tipo mais derivado.

Tipos de classe e interface podem ser convertidos para e de qualquer tipo de interface. Conversões entre um tipo e um tipo de interface êxito apenas em tempo de execução se os tipos reais envolvidos têm uma relação de herança ou implementação. Como um tipo de interface sempre conterá uma instância de um tipo que deriva de `Object`, um tipo de interface sempre também pode ser convertido para e de `Object`.

__Observação.__ Não é um erro ao converter um `NotInheritable` classes de e para interfaces que não implementa como classes que representam classes COM podem ter implementações de interface que não são conhecidas até o tempo de execução. 

Se uma conversão de referência falhar em tempo de execução, um `System.InvalidCastException` exceção é lançada.

### <a name="reference-variance-conversions"></a>Conversões de variação de referência

Interfaces genéricas ou delegados podem ter parâmetros de tipo variante que permitem conversões entre variantes compatíveis do tipo. Portanto, em tempo de execução uma conversão de um tipo de classe ou um tipo de interface para um tipo de interface que é compatível com um tipo de interface, ele herda de ou implementa de variante terá êxito. Da mesma forma, tipos de delegado podem ser convertidos para e de variante compatível tipos delegados. Por exemplo, o tipo de delegado

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

permitiria que uma conversão de `F(Of Object, Integer)` para `F(Of String, Integer)`. Ou seja, um delegado `F` que utiliza `Object` pode ser usado com segurança como um delegado `F` que utiliza `String`. Quando o delegado é invocado, o método de destino estará esperando um objeto e uma cadeia de caracteres é um objeto.

Um tipo de interface ou delegado genérico `S(Of S1,...,Sn)` será considerada *variante compatível* com um tipo de interface ou delegado genérico `T(Of T1,...,Tn)` se:

* `S` e `T` são construídos do mesmo tipo genérico `U(Of U1,...,Un)`.

* Para cada parâmetro de tipo `Ux`:

  * Se o parâmetro de tipo foi declarado sem, em seguida, a variação `Sx` e `Tx` deve ser do mesmo tipo.

  * Se o parâmetro de tipo foi declarado `In` e em seguida, deve haver uma ampliação identidade, padrão, referência, matriz ou tipo de conversão de parâmetro de `Sx` para `Tx`.

  * Se o parâmetro de tipo foi declarado `Out` e em seguida, deve haver uma ampliação identidade, padrão, referência, matriz ou tipo de conversão de parâmetro de `Tx` para `Sx`.

Ao converter de uma classe em uma interface genérica com parâmetros de tipo de variante, se a classe implementa mais de uma interface compatível com variante a conversão é ambígua, se não houver uma conversão não-variant. Por exemplo:

```vb
Class Base
End Class

Class Derived1
    Inherits Base
End Class

Class Derived2
    Inherits Base
End Class

Class OneAndTwo
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Class BaseAndOneAndTwo
    Implements IEnumerable(Of Base)
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Module Test
    Sub Main()
        ' Error: conversion is ambiguous
        Dim x As IEnumerable(Of Base) = New OneAndTwo()

        ' OK, will pick up the direct implementation of IEnumerable(Of Base)
        Dim y as IEnumerable(Of Base) = New BaseAndOneAndTwo()
    End Sub
End Module
```

### <a name="anonymous-delegate-conversions"></a>Conversões de delegado anônimo

Quando uma expressão classificada como um método lambda é reclassificada como um valor em um contexto em que não há nenhum tipo de destino (por exemplo, `Dim x = Function(a As Integer, b As Integer) a + b`), ou onde o tipo de destino não é um tipo de delegado, o tipo da expressão resultante é um tipo de delegado anônimo equivalente à assinatura do método lambda. Esse tipo de delegado anônimo tem uma conversão para qualquer tipo de delegado compatível: um tipo de delegado compatível é qualquer tipo de delegado que pode ser criado usando uma expressão de criação de delegado com o tipo de delegado anônimo `Invoke` método como um parâmetro. Por exemplo:

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

Observe que os tipos `System.Delegate` e `System.MulticastDelegate` por si próprios considerados tipos de delegado (mesmo que todos os tipos de delegado herdam deles). Além disso, observe que a conversão do tipo de delegado anônimo para um tipo de delegado compatível não é uma conversão de referência.

## <a name="array-conversions"></a>Conversões de matriz

As conversões que são definidas em matrizes em virtude do fato de que eles são tipos de referência, além de várias conversões de especiais existem para matrizes.

Para quaisquer dois tipos `A` e `B`, se eles forem tipos de referência ou parâmetros de tipo não conhecidos como tipos de valor e se `A` tem uma referência, matriz ou tipo de conversão de parâmetro para `B`, existe uma conversão de uma matriz de tipo de `A` para uma matriz do tipo `B` com a mesma classificação. Essa relação é conhecida como *covariância de matriz*. Covariância de matriz em particular significa que um elemento de uma matriz cujo tipo de elemento é `B` , na verdade, pode ser um elemento de uma matriz cujo tipo de elemento é `A`, desde que ambos `A` e `B` são tipos de referência e esse `B` tem uma conversão de referência ou conversão de matriz para `A`. No exemplo a seguir, a segunda chamada de `F` faz com que um `System.ArrayTypeMismatchException` exceção seja lançada porque o tipo de elemento real do `b` é `String`, e não `Object`:

```vb
Module Test
    Sub F(ByRef x As Object)
    End Sub

    Sub Main()
        Dim a(10) As Object
        Dim b() As Object = New String(10) {}
        F(a(0)) ' OK.
        F(b(1)) ' Not allowed: System.ArrayTypeMismatchException.
   End Sub
End Module
```

Devido a covariância de matriz, atribuições aos elementos de matrizes de tipo de referência incluem uma verificação de tempo de execução que garante que o valor que está sendo atribuído ao elemento de matriz, na verdade, é de um tipo permitido.

```vb
Module Test
    Sub Fill(array() As Object, index As Integer, count As Integer, _
            value As Object)
        Dim i As Integer

        For i = index To (index + count) - 1
            array(i) = value
        Next i
    End Sub

    Sub Main()
        Dim strings(100) As String

        Fill(strings, 0, 101, "Undefined")
        Fill(strings, 0, 10, Nothing)
        Fill(strings, 91, 10, 0)
    End Sub
End Module
```

Neste exemplo, a atribuição ao `array(i)` no método `Fill` implicitamente inclui uma verificação de tempo de execução que garante que o objeto referenciado pela variável `value` seja `Nothing` ou uma instância de um tipo que é compatível com o tipo de elemento real da matriz `array`. No método `Main`, as duas primeiras invocações de método `Fill` bem-sucedida, mas as causas de invocação de terceiro uma `System.ArrayTypeMismatchException` exceção seja lançada após executar a primeira atribuição para `array(i)`. A exceção ocorre porque uma `Integer` não podem ser armazenados em um `String` matriz.

Se um dos tipos de elemento da matriz é um parâmetro de tipo cujo tipo acaba sendo um tipo de valor em tempo de execução, um `System.InvalidCastException` exceção será lançada. Por exemplo:

```vb
Module Test
    Sub F(Of T As U, U)(x() As T)
        Dim y() As U = x
    End Sub

    Sub Main()
        ' F will throw an exception because Integer() cannot be
        ' converted to Object()
        F(New Integer() { 1, 2, 3 })
    End Sub
End Module
```

Também não há conversão entre uma matriz de um tipo enumerado, e uma matriz do tipo enumerado subjacente do tipo ou uma matriz de outro tipo enumerado com o mesmo tipo subjacente, desde que as matrizes têm a mesma classificação.

```vb
Enum Color As Byte
    Red
    Green
    Blue
End Enum

Module Test
    Sub Main()
        Dim a(10) As Color
        Dim b() As Integer
        Dim c() As Byte

        b = a    ' Error: Integer is not the underlying type of Color
        c = a    ' OK
        a = c    ' OK
    End Sub
End Module
```

Neste exemplo, uma matriz de `Color` é convertido em uma matriz de `Byte`, `Color`subjacente do tipo. A conversão para uma matriz de `Integer`, no entanto, ocorrerá um erro porque `Integer` não é do tipo subjacente de `Color`.

Uma matriz de classificação 1 do tipo `A()` também tem uma conversão de matriz para os tipos de interface de coleção `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` e `IEnumerable(Of B)` encontrado no `System.Collections.Generic`, desde que uma das seguintes opções for verdadeira:

- `A` e `B` são ambos referenciam tipos ou parâmetros de tipo não conhecidos como tipos de valor; e `A` tem uma conversão de parâmetro de referência, matriz ou tipo de ampliação para `B`; ou
- `A` e `B` são ambos os tipos enumerados do mesmo tipo subjacente; ou
- um dos `A` e `B` é um tipo enumerado, e o outro é o seu tipo subjacente.

Qualquer matriz de um tipo com qualquer classificação também tem uma conversão de matriz para os tipos de interface de coleção não genérica `IList`, `ICollection` e `IEnumerable` encontrado na `System.Collections`.

É possível iterar sobre as interfaces resultantes usando `For Each`, ou por meio de invocar o `GetEnumerator` métodos diretamente. No caso de matrizes de classificação 1 convertido formas genéricas ou não genérico de `IList` ou `ICollection`, também é possível obter elementos pelo índice. No caso de matrizes de classificação 1 convertidos em formas genéricas ou não genérico de `IList`, também é possível definir elementos pelo índice, sujeito a tempo de execução do mesmo covariância de matriz verifica conforme descrito acima. O comportamento de todos os outros métodos de interface não está definido pela especificação de linguagem do VB; é até o tempo de execução subjacente.

## <a name="value-type-conversions"></a>Conversões de tipo de valor

Um valor de tipo de valor pode ser convertido para um dos seus tipos de referência de base ou um tipo de interface que ele implementa através de um processo chamado *conversão boxing*. Quando um valor de tipo de valor é convertido, o valor é copiado do local onde ele reside na pilha do .NET Framework. Uma referência a esse local no heap, em seguida, é retornada e pode ser armazenada em uma variável de tipo de referência. Essa referência também é conhecida como um *box* instância do tipo de valor. A instância do box tem a mesma semântica que um tipo de referência em vez de um tipo de valor.

Tipos de valor demarcado podem ser convertidos de volta para seu tipo de valor original por meio de um processo chamado *unboxing*. Quando um tipo de valor demarcado é não demarcado, o valor é copiado do heap em um variável local. Daí em diante, ele se comporta como se fosse um tipo de valor. Ao fazer unboxing um tipo de valor, o valor deve ser um valor nulo ou uma instância do tipo de valor. Caso contrário, um `System.InvalidCastException` exceção é lançada. Se o valor for uma instância de um tipo enumerado, esse valor também pode ser desconvertido para o tipo enumerado do tipo subjacente ou outro tipo enumerado que tem o mesmo tipo subjacente. Um valor nulo é tratado como se fosse o literal `Nothing`.

Para dar suporte a anulável tipos de valor, bem, o tipo de valor `System.Nullable(Of T)` é tratado especialmente ao realizar conversões boxing e unboxing. Conversão boxing de um valor do tipo `Nullable(Of T)` resulta em um valor demarcado do tipo `T` se o valor `HasValue` é de propriedade `True` ou um valor de `Nothing` se o valor `HasValue` propriedade é `False`. Desconversão de um valor do tipo `T` à `Nullable(Of T)` resulta em uma instância do `Nullable(Of T)` cujo `Value` propriedade é o valor demarcado e cujo `HasValue` é de propriedade `True`. O valor `Nothing` pode ser submetido à conversão unboxing para `Nullable(Of T)` para qualquer `T` e resulta em um valor cuja `HasValue` é de propriedade `False`. Porque os tipos de valor demarcado se comportam como referência de tipos, é possível criar várias referências para o mesmo valor. Para os tipos primitivos e tipos enumerados, isso é irrelevante porque as instâncias desses tipos são *imutável*. Ou seja, não é possível modificar uma instância demarcada desses tipos, portanto, não é possível observar o fato de que há várias referências para o mesmo valor.

Por outro lado, estruturas, podem ser mutáveis se suas variáveis de instância estão acessíveis ou se seus métodos ou propriedades modificar suas variáveis de instância. Se uma referência a uma estrutura demarcada é usada para modificar a estrutura, todas as referências à estrutura demarcada verá a alteração. Como esse resultado pode ser inesperado, quando um valor digitado como `Object` é copiado de um local para outro valor demarcado tipos automaticamente serão clonados no heap, em vez de simplesmente ter suas referências copiadas. Por exemplo:

```vb
Class Class1
    Public Value As Integer = 0
End Class

Structure Struct1
    Public Value As Integer
End Structure

Module Test
    Sub Main()
        Dim val1 As Object = New Struct1()
        Dim val2 As Object = val1

        val2.Value = 123

        Dim ref1 As Object = New Class1()
        Dim ref2 As Object = ref1

        ref2.Value = 123

        Console.WriteLine("Values: " & val1.Value & ", " & val2.Value)
        Console.WriteLine("Refs: " & ref1.Value & ", " & ref2.Value)
    End Sub
End Module
```

A saída do programa é:

```
Values: 0, 123
Refs: 123, 123
```

A atribuição para o campo da variável local `val2` não afeta o campo da variável local `val1` porque quando o box `Struct1` foi atribuído a `val2`, foi feita uma cópia do valor. Por outro lado, a atribuição `ref2.Value = 123` afeta o objeto que ambos `ref1` e `ref2` referências.

__Observação.__ Cópia da estrutura não é feita para estruturas demarcadas digitadas como `System.ValueType` porque não é possível para ligação tardia fora de `System.ValueType`.

Há uma exceção à regra que boxed tipos serão copiados na atribuição de valor. Se uma referência de tipo de valor demarcado é armazenada dentro de outro tipo, a referência interna não será copiada. Por exemplo:

```vb
Structure Struct1
    Public Value As Object
End Structure

Module Test
    Sub Main()
        Dim val1 As Struct1
        Dim val2 As Struct1

        val1.Value = New Struct1()
        val1.Value.Value = 10

        val2 = val1
        val2.Value.Value = 123
        Console.WriteLine("Values: " & val1.Value.Value & ", " & _
            val2.Value.Value)
    End Sub
End Module
```

A saída do programa é:

```
Values: 123, 123
```

Isso ocorre porque o valor demarcado interno não é copiado quando o valor é copiado. Assim, ambos `val1.Value` e `val2.Value` tem uma referência para o mesmo tipo de valor Demarcado.

__Observação.__ O fato de que os tipos de valor demarcado interna não são copiados é uma limitação do .NET digite sistema – para garantir que todos os tipos de valor demarcado interna foram copiados sempre que um valor do tipo `Object` foi copiado seria extremamente dispendioso.

Como descrito anteriormente, demarcado valor tipos só podem ser desconvertidos para seu tipo original. Tipos primitivos box, no entanto, são tratados especialmente quando digitado como `Object`. Eles podem ser convertidos em qualquer tipo primitivo que eles têm uma conversão. Por exemplo:

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

Normalmente, o box `Integer` valor `5` não pôde ser unboxed em um `Byte` variável. No entanto, porque `Integer` e `Byte` são tipos primitivos e tem uma conversão, a conversão é permitida.

É importante observar que a conversão de um tipo de valor a uma interface é diferente do que um argumento genérico restrito a uma interface. Ao acessar membros de interface em um parâmetro de tipo restrita (ou chamar métodos em `Object`), conversão boxing não ocorre como quando um tipo de valor é convertido em uma interface e um membro de interface é acessado. Por exemplo, suponha que uma interface `ICounter` contém um método `Increment` que pode ser usado para modificar um valor. Se `ICounter` é usado como uma restrição, a implementação do `Increment` método for chamado com uma referência à variável que `Increment` foi chamado, não uma cópia demarcada:

```vb
Interface ICounter
    Sub Increment()
    ReadOnly Property Value() As Integer
End Interface

Structure Counter
    Implements ICounter

    Dim _value As Integer

    Property Value() As Integer Implements ICounter.Value
        Get
            Return _value
        End Get
    End Property

    Sub Increment() Implements ICounter.Increment
       value += 1
    End Sub
End Structure

Module Test
      Sub Test(Of T As ICounter)(x As T)
         Console.WriteLine(x.value)
         x.Increment()                     ' Modify x
         Console.WriteLine(x.value)
         CType(x, ICounter).Increment()    ' Modify boxed copy of x
         Console.WriteLine(x.value)
      End Sub

      Sub Main()
         Dim x As Counter
         Test(x)
      End Sub
End Module
```

A primeira chamada para `Increment` modifica o valor na variável `x`. Isso não é equivalente a segunda chamada para `Increment`, que modifica o valor em uma cópia demarcada do `x`. Assim, a saída do programa é:

```
0
1
1
```

### <a name="nullable-value-type-conversions"></a>Conversões de tipo de valor anulável

Um tipo de valor `T` pode converter de e para a versão que permite valor nula do tipo, `T?`. A conversão de `T?` à `T` lança uma `System.InvalidOperationException` exceção se o valor que está sendo convertido for `Nothing`. Além disso, `T?` tem uma conversão para um tipo `S` se `T` tem uma conversão intrínseco para `S`. E se `S` é um tipo de valor, em seguida, as seguintes conversões intrínsecas existem entre `T?` e `S?`:

* Uma conversão de mesma classificação (redução ou ampliação) da `T?` para `S?`.

* Uma conversão de mesma classificação (redução ou ampliação) da `T` para `S?`.

* Uma conversão redutora de `S?` para `T`.

Por exemplo, uma conversão de ampliação intrínseca existe da `Integer?` à `Long?` porque existe uma conversão de ampliação intrínseco da `Integer` para `Long`:

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

Durante a conversão de `T?` para `S?`, se o valor de `T?` é `Nothing`, em seguida, o valor de `S?` será `Nothing`. Durante a conversão de `S?` à `T` ou `T?` ao `S`, se o valor de `T?` ou `S?` é `Nothing`, um `System.InvalidCastException` exceção será lançada.

Devido ao comportamento do tipo subjacente `System.Nullable(Of T)`, quando um tipo de valor anulável `T?` é convertido, o resultado é um valor demarcado do tipo `T`, não um valor demarcado do tipo `T?`. E, por outro lado, ao fazer unboxing em um tipo de valor anulável `T?`, o valor será encapsulado por `System.Nullable(Of T)`, e `Nothing` será ser desconvertidos para um valor nulo do tipo `T?`. Por exemplo:

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

Um efeito colateral desse comportamento é o tipo de um valor anulável `T?` é exibida implementar todas as interfaces de `T`, porque a conversão de um tipo de valor a uma interface requer que o tipo a ser Demarcado. Como resultado, `T?` pode ser convertido em todas as interfaces que `T` pode ser convertido em. É importante observar, no entanto, que tipo de um valor anulável `T?` não implementa as interfaces de realmente `T` para fins de verificação de restrição genérica ou reflexão. Por exemplo:

```vb
Interface I1
End Interface

Structure T1
    Implements I1
    ...
End Structure

Module Test
    Sub M1(Of T As I1)(ByVal x As T)
    End Sub

    Sub Main()
        Dim x? As T1 = Nothing
        Dim y As I1 = x                ' Valid
        M1(x)                          ' Error: x? does not satisfy I1 constraint
    End Sub
End Module
```

## <a name="string-conversions"></a>Conversões de cadeia de caracteres

Convertendo `Char` em `String` resulta em uma cadeia de caracteres cujo primeiro caractere é o valor do caractere. Convertendo `String` em `Char` resulta em um caractere cujo valor é o primeiro caractere da cadeia de caracteres. Converter uma matriz de `Char` em `String` resulta em uma cadeia de caracteres cujos caracteres são os elementos da matriz. Convertendo `String` em uma matriz de `Char` resulta em uma matriz de caracteres cujos elementos são os caracteres da cadeia de caracteres.

As conversões exatas entre `String` e `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`e vice-versa, estão além do escopo desta especificação e dependem com exceção de um detalhe de implementação. Conversões de cadeia de caracteres sempre considere a cultura atual do ambiente de tempo de execução. Como tal, eles devem ser executados em tempo de execução.

## <a name="widening-conversions"></a>Conversões de expansão

Conversões ampliadoras nunca overflow, mas podem envolver uma perda de precisão. Conversões de expansão são as seguintes conversões:

__Conversões de identidade/padrão__

* De um tipo para si mesmo.

* De um tipo de delegado anônimo gerado para uma reclassificação de método lambda para qualquer tipo de delegado com uma assinatura idêntica.

* Do literal `Nothing` para um tipo.

__Conversões numéricas__

* Partir `Byte` à `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, ou `Double`.

* Partir `SByte` à `Short`, `Integer`, `Long`, `Decimal`, `Single`, ou `Double`.

* Partir `UShort` à `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, ou `Double`.

* Partir `Short` à `Integer`, `Long`, `Decimal`, `Single` ou `Double`.

* Partir `UInteger` à `ULong`, `Long`, `Decimal`, `Single`, ou `Double`.

* Partir `Integer` à `Long`, `Decimal`, `Single` ou `Double`.

* Partir `ULong` à `Decimal`, `Single`, ou `Double`.

* Partir `Long` à `Decimal`, `Single` ou `Double`.

* Partir `Decimal` à `Single` ou `Double`.

* Partir `Single` para `Double`.

* Do literal `0` para um tipo enumerado. (__Observação.__ A conversão de `0` para qualquer tipo enumerado ampliação para simplificar o teste de sinalizadores. Por exemplo, se `Values` é um tipo enumerado com um valor `One`, você pode testar uma variável `v` do tipo `Values` dizendo `(v And Values.One) = 0`.)

* Em um tipo enumerado para seu tipo numérico subjacente ou para um tipo numérico que seu tipo numérico subjacente tem uma conversão de ampliação.

* De uma expressão de constante do tipo `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`, ou `SByte` fornecido para um tipo mais estreito, o valor da expressão constante está dentro do intervalo de tipo de destino. (__Observação.__ Conversões de `UInteger` ou `Integer` à `Single`, `ULong` ou `Long` para `Single` ou `Double`, ou `Decimal` para `Single` ou `Double` pode causar perda de precisão, mas nunca causa a perda de magnitude. As outras conversões ampliadoras numéricas nunca perdem todas as informações.)

__Conversões de referência__

* De um tipo de referência em um tipo base.

* De um tipo de referência para um tipo de interface, fornecidas que o tipo implementa a interface ou uma interface compatível com variante.

* De um tipo de interface para `Object`.

* De um tipo de interface para um tipo de interface compatível com variante.

* Tipo de delegado de um tipo de delegado como uma variante compatível. (__Observação.__ Muitas outras conversões de referência são deduzidos por essas regras. Por exemplo, os delegados anônimos são tipos de referência que herdam de `System.MulticastDelegate`; tipos de matriz são tipos de referência que herdaram `System.Array`; anônimos tipos são tipos de referência que herdam de `System.Object`.)

__Conversões de delegado anônimas__

* De um anônimo gerado para uma reclassificação de método lambda para qualquer tipo de delegado mais amplo de tipo de delegado.

__Conversões de matriz__

* De um tipo de matriz `S` com um tipo de elemento `Se` para um tipo de matriz `T` com um tipo de elemento `Te`, desde que todos os itens a seguir forem verdadeiras:

  * `S` e `T` diferem apenas no tipo de elemento.

  * Ambos `Se` e `Te` são tipos de referência ou parâmetros de tipo que são conhecidos como um tipo de referência.

  * Uma referência, matriz ou tipo de ampliação existe conversão de parâmetro do `Se` para `Te`.

* De um tipo de matriz `S` com um tipo de elemento enumerado `Se` para um tipo de matriz `T` com um tipo de elemento `Te`, desde que todos os itens a seguir forem verdadeiras:

  * `S` e `T` diferem apenas no tipo de elemento.

  * `Te` é o tipo subjacente de `Se`.

* De um tipo de matriz `S` de classificação 1, com um tipo de elemento enumerado `Se`, para `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`, e `IEnumerable(Of Te)`, desde que uma das seguintes opções for verdadeira:

  * Ambos `Se` e `Te` são tipos de referência ou parâmetros de tipo conhecido para ser uma referência de tipo e uma referência de ampliação, matriz ou existe conversão de parâmetro de tipo do `Se` para `Te`; ou

  * `Te` é o tipo subjacente de `Se`; ou

  * `Te` é idêntico a `Se`

__Conversões de tipo de valor__

* De um tipo de valor em um tipo base.

* De um tipo de valor em um tipo de interface que o tipo implementa.

__Conversões de tipo de valor anuláveis__

* De um tipo `T` para o tipo `T?`.

* De um tipo `T?` em um tipo `S?`, em que há uma conversão de ampliação do tipo `T` para o tipo `S`.

* De um tipo `T` em um tipo `S?`, em que há uma conversão de ampliação do tipo `T` para o tipo `S`.

* De um tipo `T?` a uma interface de tipo que o tipo `T` implementa.

__Conversões de cadeia de caracteres__

* Partir `Char` para `String`.

* Partir `Char()` para `String`.

__Conversões de tipo de parâmetro__

* De um parâmetro de tipo para `Object`.

* De um parâmetro de tipo para uma restrição de tipo de interface ou qualquer variante de interface compatível com uma restrição de tipo de interface.

* De um parâmetro de tipo para uma interface implementada por uma restrição de classe.

* De um parâmetro de tipo como uma variante de interface compatível com uma interface implementada por uma restrição de classe.

* De um parâmetro de tipo para uma restrição de classe ou um tipo base da restrição de classe.

* De um parâmetro de tipo `T` a uma restrição de parâmetro de tipo `Tx`, ou qualquer coisa `Tx` tem uma conversão de ampliação.

## <a name="narrowing-conversions"></a>Conversões de redução

Conversões de estreitamento são conversões que não podem ser provado que ele é sempre têm êxito, conversões que são conhecidas como possivelmente perder informações e conversões entre domínios de tipos diferentes o suficiente para justificar a notação de estreitamento. As conversões a seguir são classificadas como conversões de estreitamento:

__Conversões de boolianos__

* Partir `Boolean` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, ou `Double`.

* Partir `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, ou `Double` para `Boolean`.

__Conversões numéricas__

* Partir `Byte` para `SByte`.

* Partir `SByte` à `Byte`, `UShort`, `UInteger`, ou `ULong`.

* Partir `UShort` à `Byte`, `SByte`, ou `Short`.

* Partir `Short` à `Byte`, `SByte`, `UShort`, `UInteger`, ou `ULong`.

* Partir `UInteger` à `Byte`, `SByte`, `UShort`, `Short`, ou `Integer`.

* Partir `Integer` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, ou `ULong`.

* Partir `ULong` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, ou `Long`.

* Partir `Long` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, ou `ULong`.

* Partir `Decimal` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, ou `Long`.

* Partir `Single` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, ou `Decimal`.

* Partir `Double` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, ou `Single`.

* De um tipo numérico em um tipo enumerado.

* Em um tipo enumerado para um tipo numérico seu tipo numérico subjacente tem uma conversão de estreitamento.

* Em um tipo enumerado em outro tipo enumerado.

__Conversões de referência__

* De um tipo de referência para um tipo mais derivado.

* De um tipo de classe em um tipo de interface, fornecido que o tipo de classe não implementa o tipo de interface ou uma variante do tipo de interface compatível com ele.

* De um tipo de interface para um tipo de classe.

* De uma interface tipo para outro tipo de interface, desde que exista há relação de herança entre os dois tipos e fornecidas não são compatíveis com variant.

__Conversões de delegado anônimas__

* De um tipo de delegado anônimo gerado para uma reclassificação de método lambda para qualquer tipo de delegado mais estreita.

__Conversões de matriz__

* De um tipo de matriz `S` com um tipo de elemento `Se`, para um tipo de matriz `T` com um tipo de elemento `Te`, desde que todos os itens a seguir forem verdadeiras:

  * `S` e `T` diferem apenas no tipo de elemento.
  * Ambos `Se` e `Te` são tipos de referência ou parâmetros de tipo não são conhecidos para tipos de valor.
  * Existe uma restrição de referência, matriz ou conversão de tipo de parâmetro a partir `Se` para `Te`.

* De um tipo de matriz `S` com um tipo de elemento `Se` para um tipo de matriz `T` com um tipo de elemento enumerado `Te`, desde que todos os itens a seguir forem verdadeiras:

  * `S` e `T` diferem apenas no tipo de elemento.
  * `Se` é o tipo subjacente de `Te` , ou quando eles são ambos os tipos enumerados diferentes que compartilham o mesmo tipo subjacente.

* De um tipo de matriz `S` de classificação 1, com um tipo de elemento enumerado `Se`, para `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` e `IEnumerable(Of Te)`, desde que uma das seguintes opções for verdadeira:

  * Ambos `Se` e `Te` são tipos de referência ou parâmetros de tipo que são conhecidos como um tipo de referência e existe uma restrição de referência, matriz ou conversão de tipo de parâmetro a partir `Se` para `Te`; ou
  * `Se` é o tipo subjacente de `Te`, ou quando eles são ambos os tipos enumerados diferentes que compartilham o mesmo tipo subjacente.

__Conversões de tipo de valor__

* De um tipo de referência para um tipo mais derivado do valor.

* De um tipo de interface para um tipo de valor, fornecido que o tipo de valor implementa o tipo de interface.

__Conversões de tipo de valor anuláveis__

* De um tipo `T?` em um tipo `T`.

* De um tipo `T?` em um tipo `S?`, em que há uma conversão de restrição do tipo `T` para o tipo `S`.

* De um tipo `T` em um tipo `S?`, em que há uma conversão de restrição do tipo `T` para o tipo `S`.

* De um tipo `S?` em um tipo `T`, em que há uma conversão do tipo `S` para o tipo `T`.

__Conversões de cadeia de caracteres__

* Partir `String` para `Char`.

* Partir `String` para `Char()`.

* Partir `String` para `Boolean` bidirecionalmente `Boolean` para `String`.

* Conversões entre `String` e `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, ou `Double`.

* Partir `String` para `Date` bidirecionalmente `Date` para `String`.

__Conversões de tipo de parâmetro__

* De `Object` para um parâmetro de tipo.

* De um tipo de parâmetro para um tipo de interface, fornecido o parâmetro de tipo não é restrito a interface ou restrito a uma classe que implementa essa interface.

* De um tipo de interface para um parâmetro de tipo.

* De um parâmetro de tipo para um tipo derivado de uma restrição de classe.

* De um parâmetro de tipo `T` a qualquer coisa como uma restrição de parâmetro de tipo `Tx` tem uma conversão de estreitamento.

## <a name="type-parameter-conversions"></a>Conversões de tipo de parâmetro

Conversões de parâmetros de tipo são determinadas pelas restrições, se houver, colocar neles. Um parâmetro de tipo `T` sempre pode ser convertido em si só, para e de `Object`e de e para qualquer tipo de interface. Observe que, se o tipo `T` é um tipo de valor em tempo de execução, convertendo de `T` à `Object` ou um tipo de interface será uma conversão boxing e convertendo de `Object` ou uma interface de tipo para `T` será uma conversão unboxing conversão. Um parâmetro de tipo com uma restrição de classe `C` define conversões adicionais do parâmetro de tipo para `C` e suas classes base e vice-versa. Um parâmetro de tipo `T` com uma restrição de parâmetro de tipo `Tx` define uma conversão `Tx` e qualquer coisa `Tx` converte.

Uma matriz cujo tipo de elemento é um parâmetro de tipo com uma restrição de interface `I` tem as mesmas conversões de matriz covariante como uma matriz cujo tipo de elemento é `I`, desde que o parâmetro de tipo também tem um `Class` ou (restrição de classe como apenas referência tipos de elemento de matriz podem ser covariantes). Uma matriz cujo tipo de elemento é um parâmetro de tipo com uma restrição de classe `C` tem as mesmas conversões de matriz covariante como uma matriz cujo tipo de elemento é `C`.

As regras de conversões acima não permitem conversões de parâmetros de tipo sem restrição para tipos sem interface, que pode ser surpreendente. A razão para isso é evitar confusão sobre a semântica de tais conversões. Por exemplo, considere a seguinte declaração:

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

Se a conversão de `T` à `Integer` eram permitidas, seria possível com facilidade que `X(Of Integer).F(7)` retornaria `7L`. No entanto, não, seria como conversões numéricas são consideradas somente quando os tipos são conhecidos como numéricos em tempo de compilação. Para fazer a semântica clear, o exemplo acima deve ser escrito:

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a>Conversões definidas pelo usuário

*Conversões intrínseco* conversões definidas pelo idioma (ou seja, listado nesta especificação), enquanto *conversões definidas pelo usuário* são definidos pela sobrecarga de `CType` operador. Ao converter entre tipos, se nenhuma conversão intrínseco forem aplicável serão consideradas conversões definidas pelo usuário. Se não houver uma conversão definida pelo usuário que está *mais específica* para os tipos de origem e destino, em seguida, a conversão definida pelo usuário será usada. Caso contrário, um erro de tempo de compilação ocorre. A conversão mais específica é aquele cujo operando é "mais próximo" para o tipo de fonte e cujo tipo de resultado é "mais próximo" para o tipo de destino. Ao determinar quais conversão definida pelo usuário para usar, o conversão de ampliação mais específico será usado; Se nenhuma ampliação conversão é o mais específica, que a conversão de estreitamento mais específica será usada. Se houver não mais específica conversão de estreitamento, em seguida, a conversão é indefinida e ocorre um erro de tempo de compilação.

As seções a seguir abordam como as conversões mais específicas são determinadas. Eles usam os seguintes termos:

Se uma ampliação intrínseca existe conversão de um tipo `A` para um tipo `B`e caso nem `A` nem `B` são interfaces, em seguida, `A` é *englobados* pelo `B`, e `B` *abrange* `A`.

O *mais abrangente* tipo em um conjunto de tipos é o tipo de uma que abrange todos os outros tipos no conjunto. Se nenhum tipo abrange todos os outros tipos, o conjunto não tem nenhum tipo mais abrangente. Em termos de intuitivos, o tipo mais abrangente é o tipo "maior" em set--o um tipo ao qual cada um dos outros tipos pode ser convertida por meio de uma conversão de ampliação.

O *englobados mais* tipo em um conjunto de tipos é o tipo de um determinado contido por todos os outros tipos no conjunto. Se nenhum tipo é abrangido pela todos os outros tipos, o conjunto tem não mais englobados tipo. Em termos de intuitivos, o tipo mais contido é o tipo "menor" em set--o um tipo que pode ser convertido em cada um dos outros tipos por meio de uma conversão de estreitamento.

Ao coletar as conversões definidas pelo usuário do candidato para um tipo de `T?`, os operadores de conversão definida pelo usuário definidos pela `T` são usados em vez disso. Se o tipo que está sendo convertido em também é um tipo de valor anulável, em seguida, qualquer uma das `T`do definidas pelo usuário são elevadas a operadores de conversões que envolvem apenas os tipos de valor não anulável. Um operador de conversão de `T` à `S` é elevado para ser uma conversão de `T?` ao `S?` e é avaliada, convertendo `T?` para `T`, se necessário, em seguida, avaliar a conversão definida pelo usuário operador de `T` à `S` e, em seguida, convertendo `S` para `S?`, se necessário. Se o valor que está sendo convertido estiver `Nothing`, no entanto, um operador de conversão elevadas se converte diretamente em um valor de `Nothing` digitado como `S?`. Por exemplo:

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Legal: y is still null
        x = New T()
        y = x                ' Legal: Converts from T to S
    End Sub
End Module
```

Quando a resolução de conversões, definido pelo usuário operadores de conversões sempre têm preferência sobre os operadores de conversão cancelada. Por exemplo:

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator

    Public Shared Widening Operator CType(ByVal v As T?) As S?
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Calls user-defined conversion, not lifted conversion
    End Sub
End Module
```

Em tempo de execução, a avaliação de uma conversão definida pelo usuário pode envolver até três etapas:

1. Primeiro, o valor é convertido do tipo de fonte para o tipo de operando usando uma conversão intrínseco, se necessário.

2. Em seguida, a conversão definida pelo usuário é invocada.

3. Por fim, o resultado da conversão definida pelo usuário é convertido para o tipo de destino usando uma conversão intrínseco, se necessário.

É importante observar que a avaliação de uma conversão definida pelo usuário nunca envolverá mais de um operador de conversão definida pelo usuário.

### <a name="most-specific-widening-conversion"></a>Mais específico de conversão de ampliação

Determinando o mais específica definida pelo usuário ampliação operador de conversão entre dois tipos é realizado usando as seguintes etapas:

1. Primeiro, todos os operadores de conversão do candidato são coletados. Os operadores de conversão do candidato são todos os operadores definidos pelo usuário de conversão a ampliação no tipo de origem e todos os operadores definidos pelo usuário de conversão a ampliação no tipo de destino.

2. Em seguida, todos os operadores de conversão não aplicáveis são removidos do conjunto. Um operador de conversão é aplicável a um tipo de origem e o tipo de destino se há um operador intrínseco de conversão de ampliação, a do tipo de fonte, para o tipo de operando, e há um operador intrínseco de conversão de ampliação, a do resultado do operador, para o tipo de destino. Se não houver nenhum operador de conversão aplicável, em seguida, há não mais específica conversão de ampliação.

3. Em seguida, o tipo mais específico do código-fonte dos operadores de conversão aplicável é determinado:

   * Se qualquer um dos operadores de conversão converter diretamente do tipo de fonte, o tipo de origem é o tipo mais específico do código-fonte.

   * Caso contrário, o tipo mais específico do código-fonte é o tipo mais contido no conjunto combinado de tipos de fonte dos operadores de conversão. Se não mais englobados tipo pode ser encontrado, em seguida, há não mais específica conversão de ampliação.

4. Em seguida, o tipo de destino mais específico dos operadores de conversão aplicável é determinado:

   * Se qualquer um dos operadores de conversão converte diretamente para o tipo de destino, o tipo de destino é o tipo de destino mais específico.

   * Caso contrário, o tipo mais específico de destino é o tipo mais abrangente no conjunto combinado de tipos de destino dos operadores de conversão. Se nenhum tipo mais abrangente pode ser encontrado, em seguida, há não mais específico conversão de ampliação.

5. Em seguida, se o operador de conversão de exatamente um converte do tipo de fonte mais específico para o tipo de destino mais específico, este é o operador de conversão mais específico. Não se houver mais de um operador desse tipo, há nenhuma conversão de ampliação mais específico.

### <a name="most-specific-narrowing-conversion"></a>Conversão de estreitamento mais específico

Determinando o mais específica definida pelo usuário estreitamento operador de conversão entre dois tipos é realizado usando as seguintes etapas:

1. Primeiro, todos os operadores de conversão do candidato são coletados. Os operadores de conversão do candidato são todos os operadores de conversão definida pelo usuário no tipo de origem e todos os operadores de conversão definida pelo usuário no tipo de destino.

2. Em seguida, todos os operadores de conversão não aplicáveis são removidos do conjunto. Um operador de conversão é aplicável a um tipo de origem e o tipo de destino se há um operador de conversão intrínseco do tipo de fonte para o tipo de operando, e há um operador de conversão intrínseco do resultado do operador para o tipo de destino. Se não houver nenhum operador de conversão aplicável, há nenhuma conversão de estreitamento mais específica.

3. Em seguida, o tipo mais específico do código-fonte dos operadores de conversão aplicável é determinado:

   * Se qualquer um dos operadores de conversão converter diretamente do tipo de fonte, o tipo de origem é o tipo mais específico do código-fonte.

   * Caso contrário, se qualquer um dos operadores de conversão converter de tipos que abranjam o tipo de origem, o tipo mais específico do código-fonte é o tipo mais contido no conjunto combinado de tipos de fonte desses operadores de conversão. Se não mais englobados tipo pode ser encontrado, em seguida, não há nenhuma conversão de estreitamento mais específica.

   * Caso contrário, o tipo mais específico do código-fonte é o tipo mais abrangente no conjunto combinado de tipos de fonte dos operadores de conversão. Se nenhum tipo mais abrangente pode ser encontrado, há nenhuma conversão de estreitamento mais específica.

4. Em seguida, o tipo de destino mais específico dos operadores de conversão aplicável é determinado:

   * Se qualquer um dos operadores de conversão converte diretamente para o tipo de destino, o tipo de destino é o tipo de destino mais específico.

   * Caso contrário, se qualquer um dos operadores de conversão converter em tipos que são englobados pelo tipo de destino, em seguida, o tipo mais específico de destino é o tipo mais abrangente no conjunto combinado de tipos de fonte desses operadores de conversão. Se nenhum tipo mais abrangente pode ser encontrado, há nenhuma conversão de estreitamento mais específica.

   * Caso contrário, o tipo mais específico de destino é o tipo mais contido no conjunto combinado de tipos de destino dos operadores de conversão. Se não mais englobados tipo pode ser encontrado, em seguida, não há nenhuma conversão de estreitamento mais específica.

5. Em seguida, se o operador de conversão de exatamente um converte do tipo de fonte mais específico para o tipo de destino mais específico, este é o operador de conversão mais específico. Se houver mais de um operador desse tipo, há nenhuma conversão de estreitamento mais específica.

## <a name="native-conversions"></a>Conversões nativas

Várias das conversões são classificadas como *conversões nativas* porque eles são suportados nativamente pelo .NET Framework. Essas conversões são aqueles que pode ser otimizada através do uso do `DirectCast` e `TryCast` operadores de conversão, bem como outros comportamentos especiais. As conversões classificadas como conversões nativas são: conversões de identidade, conversões padrão, conversões de referência, conversões de matriz, conversões de tipo de valor e conversões de tipo de parâmetro.

## <a name="dominant-type"></a>Tipo dominante

Dado um conjunto de tipos, geralmente é necessário em situações como inferência de tipo para determinar a *tipo dominante* do conjunto. O tipo dominante de um conjunto de tipos é determinado pelo primeiro remover quaisquer tipos que um ou mais tipos não têm uma conversão implícita em. Se não houver nenhum tipo para a esquerda no momento, não há nenhum tipo dominante. O tipo dominante é, em seguida, a maioria englobados dos tipos restantes. Se não houver mais de um tipo que é mais englobados, em seguida, não há nenhum tipo dominante.
