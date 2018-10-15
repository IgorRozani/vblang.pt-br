# <a name="general-concepts"></a>Conceitos gerais

Este capítulo aborda vários conceitos que são necessários para entender a semântica da linguagem do Microsoft Visual Basic. Muitos dos conceitos devem ser familiares para programadores de Visual Basic ou programadores de C/C++, mas suas definições precisas podem ser diferentes.

## <a name="declarations"></a>Declarações

Um programa Visual Basic é composto por entidades nomeadas. Essas entidades são apresentadas por meio *declarações* e representam o "significado" do programa.

Em um nível superior, *namespaces* são entidades que organizam as outras entidades, como tipos e namespaces aninhados. *Tipos de* são entidades que descrevem os valores e definem o código executável. Tipos podem conter tipos aninhados e membros de tipo. *Membros de tipo* são constantes, variáveis, métodos, operadores, propriedades, eventos, valores de enumeração e construtores.

Uma entidade que pode conter outras entidades define uma *espaço de declaração*. Entidades são apresentadas em um espaço de declaração, por meio de declarações ou herança; o espaço de declaração recipiente é chamado das entidades *contexto de declaração*. Declarar uma entidade em um espaço de declaração por sua vez define um novo espaço de declaração que pode conter declarações de ainda mais as entidades aninhadas; Portanto, as declarações em um programa formam uma hierarquia de espaços de declaração.

Exceto no caso de membros de tipo sobrecarregado, não é válido para declarações de introduzir as entidades com nomes idênticos do mesmo tipo no mesmo contexto de declaração. Além disso, um espaço de declaração nunca pode conter diferentes tipos de entidades com o mesmo nome; Por exemplo, um espaço de declaração pode nunca conter uma variável e um método com o mesmo nome.

__Observação.__ Talvez seja possível em outras linguagens para criar um espaço de declaração que contém tipos diferentes de entidades com o mesmo nome (por exemplo, se a linguagem diferencia maiusculas de minúsculas e permite que as declarações diferentes com base em maiusculas e minúsculas). Nessa situação, a entidade mais acessível está sendo associado a esse nome; Se mais de um tipo de entidade é mais acessível, em seguida, o nome é ambíguo. `Public` é mais acessível do que `Protected Friend`, `Protected Friend` é mais acessível que `Protected` ou `Friend`, e `Protected` ou `Friend` é mais acessível que `Private`.

O espaço de declaração de um namespace é "ilimitado," para contribuir com duas declarações de namespace com o mesmo nome totalmente qualificado para o mesmo espaço de declaração. No exemplo a seguir, as duas declarações de namespace contribuem para o mesmo espaço de declaração, nesse caso, declarando duas classes com nomes totalmente qualificados `Data.Customer` e `Data.Order:`

```vb
Namespace Data
    Class Customer
    End Class
End Namespace

Namespace Data
    Class Order
    End Class
End Namespace
```

Porque as duas declarações a contribuir com o mesmo espaço de declaração, ocorrerá um erro de tempo de compilação se cada continha uma declaração de uma classe com o mesmo nome.

### <a name="overloading-and-signatures"></a>Assinaturas e sobrecarga

A única maneira de declarar entidades com nomes idênticos do mesmo tipo em um espaço de declaração é por meio *sobrecarga*. Somente métodos, operadores, construtores de instância e propriedades podem ser sobrecarregadas.

Membros de tipo sobrecarregado devem ter assinaturas exclusivas. A assinatura de um membro de tipo consiste do número de parâmetros de tipo e o número e tipos de parâmetros do membro. Operadores de conversão também incluem o tipo de retorno do operador na assinatura.

A seguir não fazem parte da assinatura de um membro e, portanto, não pode ser sobrecarregadas:

* Modificadores de um membro de tipo (por exemplo, `Shared` ou `Private`).

* Modificadores de um parâmetro (por exemplo, `ByVal` ou `ByRef`).

* Os nomes dos parâmetros.

* O tipo de retorno de um método ou operador (exceto para os operadores de conversão) ou o tipo de elemento de uma propriedade.

* Restrições em um parâmetro de tipo.

O exemplo a seguir mostra um conjunto de declarações de método sobrecarregado, juntamente com suas assinaturas. Essa declaração não é válida, pois várias das declarações de método têm assinaturas idênticas.

```vb
Interface ITest
    Sub F1()                              ' Signature is ().
    Sub F2(x As Integer)                  ' Signature is (Integer).
    Sub F3(ByRef x As Integer)            ' Signature is (Integer).
    Sub F4(x As Integer, y As Integer)    ' Signature is (Integer, Integer).
    Function F5(s As String) As Integer   ' Signature is (String).
    Function F6(x As Integer) As Integer  ' Signature is (Integer).
    Sub F7(a() As String)                 ' Signature is (String()).
    Sub F8(ParamArray a() As String)      ' Signature is (String()).
    Sub F9(Of T)()                        ' Signature is !1().
    Sub F10(Of T, U)(x As T, y As U)      ' Signature is !2(!1, !2)
    Sub F11(Of U, T)(x As T, y As U)      ' Signature is !2(!2, !1)
    Sub F12(Of T)(x As T)                 ' Signature is !1(!1)
    Sub F13(Of T As IDisposable)(x As T)  ' Signature is !1(!1)
End Interface
```

Ele é válido para definir um tipo genérico que pode conter membros com assinaturas idênticas com base nos argumentos de tipo fornecidos. As regras de resolução de sobrecarga são usadas para tentar resolver a ambiguidade entre essas sobrecargas, embora possa haver situações em que é impossível para resolver a ambiguidade. Por exemplo:

```vb
Class C(Of T)
    Sub F(x As Integer)
    End Sub

    Sub F(x As T)
    End Sub

    Sub G(Of U)(x As T, y As U)
    End Sub

    Sub G(Of U)(x As U, y As T)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As New C(Of Integer)
        x.F(10)                   ' Calls C(Of T).F(Integer)
        x.G(Of Integer)(10, 10)    ' Error: Can't choose between overloads
    End Sub
End Module
```

## <a name="scope"></a>Escopo

O *escopo* do nome da entidade é o conjunto de todos os espaços de declaração dentro da qual é possível referir-se para o nome sem qualificação. Em geral, o escopo do nome da entidade é seu contexto de declaração inteira; No entanto, a declaração de uma entidade pode conter declarações aninhadas de entidades com o mesmo nome. Nesse caso, a entidade aninhado *sombras*, ou oculta, a entidade externa e o acesso à entidade sombreada só é possível por meio de qualificação.

Sombreamento através de aninhamento ocorre em namespaces ou tipos aninhados dentro de namespaces, tipos aninhados dentro de outros tipos e os corpos de membros de tipo. Sombreamento através de aninhamento de declarações sempre ocorre implicitamente; nenhuma sintaxe explícita é necessária.

No exemplo a seguir, dentro de `F` método, a variável de instância `i` é sombreada pela variável local `i`, mas dentro a `G` método, `i` ainda se refere à variável de instância.

```vb
Class Test
    Private i As Integer = 0

    Sub F()
        Dim i As Integer = 1
    End Sub

    Sub G()
        i = 1
    End Sub
End Class
```

Quando um nome em um escopo interno oculta um nome em um escopo externo, sombra de todas as ocorrências sobrecarregadas desse nome. No exemplo a seguir, a chamada `F(1)` invoca o `F` declarado em `Inner` porque todas as ocorrências externas de `F` estão ocultos pela declaração interna. Pelo mesmo motivo, a chamada `F("Hello")` é um erro.

```vb
Class Outer
    Shared Sub F(i As Integer)
    End Sub

    Shared Sub F(s As String)
    End Sub

    Class Inner
        Shared Sub F(l As Long)
        End Sub

        Sub G()
            F(1) ' Invokes Outer.Inner.F.
            F("Hello") ' Error.
        End Sub
    End Class
End Class
```

## <a name="inheritance"></a>Herança

Uma relação de herança é um no qual um tipo (o *derivado* tipo) é derivado de outro (o *base* tipo), de modo que o espaço de declaração do tipo derivado implicitamente contém o acessível membros de tipo diferente de construtor e tipos aninhados de seu tipo base. No exemplo a seguir, a classe `A` é a classe base `B`, e `B` é derivado de `A`.

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

Uma vez que `A` faz não explicitamente especificar uma classe base, sua classe base é implicitamente `Object`.

A seguir é aspectos importantes da herança:

* A herança é transitiva. Se tipo *C* é derivado do tipo *B*e o tipo *B* é derivado do tipo *um*, tipo *C* herda o tipo de membros declarados no tipo *B* , bem como os membros de tipo declarados no tipo *um*.

* Um tipo derivado se estende, mas não é possível restringir, seu tipo base. Um tipo derivado pode adicionar novos membros de tipo e ele pode sombrear membros de tipo herdado, mas ele não é possível remover a definição de um membro de tipo herdado.

* Como uma instância de um tipo contém todos os membros de tipo de seu tipo base, existe uma conversão sempre de um tipo derivado para seu tipo base.

* Todos os tipos de devem ter um tipo base, exceto pelo tipo `Object`. Portanto, `Object` é o tipo base definitivo de todos os tipos e todos os tipos podem ser convertidos para ele.

* Circularidade na derivação não é permitida. Ou seja, quando um tipo `B` deriva de um tipo `A`, é um erro para o tipo `A` derivar direta ou indiretamente do tipo `B`.

* Um tipo pode não direta ou indiretamente derivar de um tipo aninhado dentro dele.

O exemplo a seguir produz um erro de tempo de compilação porque as classes circularmente dependem uns aos outros.

```vb
Class A
    Inherits B
End Class

Class B
    Inherits C
End Class

Class C
    Inherits A
End Class
```

O exemplo a seguir também produz um erro de tempo de compilação porque `B` deriva indiretamente de sua classe aninhada `C` por meio da classe `A`.

```vb
Class A
    Inherits B.C
End Class

Class B
    Inherits A

    Public Class C
    End Class 
End Class
```

O exemplo a seguir não produz um erro porque classe `A` não é derivado da classe `B`.

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a>MustInherit e Classes NotInheritable

Um `MustInherit` classe é um tipo incompleto que pode agir somente como um tipo base. Um `MustInherit` classe não pode ser instanciada, portanto, é um erro usar o `New` operador em um. Ele é válido para declarar variáveis do `MustInherit` classes; só podem ser atribuídos a essas variáveis `Nothing` ou um valor que é de uma classe derivada do `MustInherit` classe.

Quando uma classe regular é derivada de uma `MustInherit` classe, a classe regular deve substituir tudo herdadas `MustOverride` membros. Por exemplo:

```vb
MustInherit Class A
    Public MustOverride Sub F()
End Class

MustInherit Class B
    Inherits A

    Public Sub G()
    End Sub
End Class 

Class C
    Inherits B

    Public Overrides Sub F()
    End Sub 
End Class
```

O `MustInherit` classe `A` apresenta uma `MustOverride` método `F`. Classe `B` apresenta um método adicional `G`, mas não fornece uma implementação de `F`. Classe `B` , portanto, também devem ser declaradas `MustInherit`. Classe `C` substituições `F` e fornece uma implementação real. Uma vez que há não pendentes `MustOverride` membros na classe `C`, ele não é necessário para ser `MustInherit`.

Um `NotInheritable` é uma classe da qual outra classe não pode ser derivada. `NotInheritable` as classes são usadas principalmente para evitar derivação não intencional.

Neste exemplo, a classe `B` é um erro porque ele tenta derivar a `NotInheritable` classe `A`. Uma classe não pode ser marcado como ambos `MustInherit` e `NotInheritable`.

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a>Interfaces e herança múltipla

Ao contrário de outros tipos que derivam apenas um único tipo base, uma interface pode derivar de várias interfaces base. Por isso, uma interface pode herdar um membro de tipo com o mesmo nome de diferentes interfaces base. Nesse caso, o nome de multiplicar herdada não está disponível na interface derivada e referindo-se a qualquer um desses membros de tipo por meio da interface derivada causa um erro de tempo de compilação, independentemente da sobrecarga ou assinaturas. Em vez disso, os membros de tipo conflitantes devem ser referenciados através de um nome de interface base.

No exemplo a seguir, as duas primeiras instruções causam erros de tempo de compilação porque o membro herdado multiplicar `Count` não está disponível na interface `IListCounter`:

```vb
Interface IList
    Property Count() As Integer
End Interface

Interface ICounter
    Sub Count(i As Integer)
End Interface

Interface IListCounter
    Inherits IList
    Inherits ICounter 
End Interface 

Module Test
    Sub F(x As IListCounter)
        x.Count(1)                  ' Error, Count is not available.
        x.Count = 1                 ' Error, Count is not available.
        CType(x, IList).Count = 1   ' Ok, invokes IList.Count.
        CType(x, ICounter).Count(1) ' Ok, invokes ICounter.Count.
    End Sub 
End Module
```

Conforme ilustrado pelo exemplo, a ambiguidade é resolvida com a conversão `x` para o tipo de interface base adequada. Tal conversão ter sem custos de tempo de execução; Eles consistem simplesmente exibindo a instância como um tipo menos derivado no tempo de compilação.

Quando um membro de tipo único é herdado usando a mesma interface base por meio de vários caminhos, o membro de tipo é tratado como se apenas foram herdada uma vez. Em outras palavras, a interface derivada contém apenas uma instância de cada membro de tipo herdado de uma interface de base específica. Por exemplo:

```vb
Interface IBase
    Sub F(i As Integer)
End Interface

Interface ILeft
    Inherits IBase
End Interface

Interface IRight
    Inherits IBase
End Interface

Interface IDerived
    Inherits ILeft, IRight
End Interface

Class Derived
    Implements IDerived

    ' Only have to implement F once.
    Sub F(i As Integer) Implements IDerived.F
    End Sub
End Class
```

Se um nome de membro de tipo é sombreado em um caminho por meio da hierarquia de herança, em seguida, o nome é sombreado em todos os caminhos. No exemplo a seguir, o `IBase.F` membro é sombreado pela `ILeft.F` membro, mas não é sombreada em `IRight`:

```vb
Interface IBase
    Sub F(i As Integer)
End Interface 

Interface ILeft
    Inherits IBase

    Shadows Sub F(i As Integer)
End Interface 

Interface IRight
    Inherits IBase

    Sub G()
End Interface 

Interface IDerived
    Inherits ILeft, IRight 
End Interface 

Class Test
    Sub H(d As IDerived)
        d.F(1)                  ' Invokes ILeft.F.
        CType(d, IBase).F(1)    ' Invokes IBase.F.
        CType(d, ILeft).F(1)    ' Invokes ILeft.F.
        CType(d, IRight).F(1)   ' Invokes IBase.F.
    End Sub 
End Class
```

A invocação `d.F(1)` seleciona `ILeft.F`, embora `IBase.F` parece não estar sombreada no caminho de acesso que o orienta ao longo das `IRight`. Porque o caminho de acesso de `IDerived` à `ILeft` para `IBase` sombras `IBase.F`, o membro também é sombreado no caminho de acesso de `IDerived` para `IRight` para `IBase`.

### <a name="shadowing"></a>Sombreamento

Um tipo derivado cobrirá o nome de um membro de tipo herdado, declarando-o novamente. Sombreamento de um nome não remove os membros de tipo herdado com esse nome. Ele apenas tornará todos os membros de tipo herdado com esse nome não está disponível na classe derivada. A declaração de sombreamento pode ser qualquer tipo de entidade.

Entidades que podem ser sobrecarregados podem escolher uma das duas formas de sombreamento. *Sombreamento por nome* é especificado usando o `Shadows` palavra-chave. Uma entidade que sombreia por nome oculta tudo com esse nome na classe base, incluindo todas as sobrecargas. *Sombreamento por nome e assinatura* é especificado usando o `Overloads` palavra-chave. Uma entidade que sombreia por nome e assinatura oculta tudo com esse nome com a mesma assinatura que a entidade. Por exemplo:

```vb
Class Base
    Sub F()
    End Sub

    Sub F(i As Integer)
    End Sub

    Sub G()
    End Sub

    Sub G(i As Integer)
    End Sub
End Class

Class Derived
    Inherits Base

    ' Only hides F(Integer).
    Overloads Sub F(i As Integer)
    End Sub

    ' Hides G() and G(Integer).
    Shadows Sub G(i As Integer)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As New Derived()

        x.F() ' Calls Base.F().
        x.G() ' Error: Missing parameter.
    End Sub
End Module
```

Sombreamento de um método com um `ParamArray` argumento por nome e assinatura oculta somente a assinatura individual, não é possíveis todas as assinaturas expandidas. Isso é verdadeiro mesmo se a assinatura do método sombreamento coincide com a assinatura não expandida do método sombreada. O exemplo a seguir:

```vb
Class Base
    Sub F(ParamArray x() As Integer)
        Console.WriteLine("Base")
    End Sub
End Class

Class Derived 
    Inherits Base

    Overloads Sub F(x() As Integer)
        Console.WriteLine("Derived")
    End Sub
End Class

Module Test
    Sub Main
        Dim d As New Derived()
        d.F(10)
    End Sub
End Module
```

Imprime `Base`, embora `Derived.F` tem a mesma assinatura que o formulário não expandida de `Base.F`.

Por outro lado, um método com um `ParamArray` argumento sombreia apenas métodos com a mesma assinatura, nem todas as assinaturas expandidas possíveis. O exemplo a seguir:

```vb
Class Base
    Sub F(x As Integer)
        Console.WriteLine("Base")
    End Sub
End Class

Class Derived
    Inherits Base

    Overloads Sub F(ParamArray x() As Integer)
        Console.WriteLine("Derived")
    End Sub
End Class

Module Test
    Sub Main()
        Dim d As New Derived()
        d.F(10)
    End Sub
End Module
```

Imprime `Base`, embora `Derived.F` tem um formato expandido que tem a mesma assinatura que `Base.F`.

Um método ou propriedade que não especifica de sombreamento `Shadows` ou `Overloads` pressupõe `Overloads` se o método ou propriedade é declarada `Overrides`, `Shadows` caso contrário. Se um membro de um conjunto de entidades sobrecarregados Especifica o `Shadows` ou `Overloads` palavra-chave, todos eles deverá especificá-lo. O `Shadows` e `Overloads` palavras-chave não podem ser especificadas ao mesmo tempo. Nem `Shadows` nem `Overloads` pode ser especificada em um módulo padrão; os membros em um módulo padrão implicitamente membros herdados de sombra `Object`.

Ele é válido para o nome de um membro de tipo que foi multiplicar-herdadas por meio da herança de interface (e que não está disponível, assim,), de sombra, portanto, tornando o nome disponíveis na interface derivada.

Por exemplo:

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight

    Shadows Sub F()
End Interface

Module Test
    Sub G(i As ILeftRight)
        i.F() ' Calls ILeftRight.F.
        CType(i, ILeft).F() ' Calls ILeft.F.
        CType(i, IRight).F() ' Calls IRight.F.
    End Sub
End Module
```

Porque os métodos são permitidos para métodos herdado de sombra, é possível que uma classe para conter várias `Overridable` métodos com a mesma assinatura. Isso não apresenta um problema de ambiguidade, já que somente o método mais derivado é visível. No exemplo a seguir, o `C` e `D` classes contêm dois `Overridable` métodos com a mesma assinatura:

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub 
End Class 

Class B
    Inherits A

    Public Overrides Sub F()
        Console.WriteLine("B.F")
    End Sub 
End Class 

Class C
    Inherits B

    Public Shadows Overridable Sub F()
        Console.WriteLine("C.F")
    End Sub 
End Class 

Class D
    Inherits C

    Public Overrides Sub F()
        Console.WriteLine("D.F")
    End Sub 
End Class 

Module Test
    Sub Main()
        Dim d As New D()
        Dim a As A = d
        Dim b As B = d
        Dim c As C = d
        a.F()
        b.F()
        c.F()
        d.F()
    End Sub 
End Module
```

Há dois `Overridable` métodos aqui: um introduzidos pela classe `A` e um introduzidos pela classe `C`. O método introduzido pela classe `C` oculta o método herdado da classe `A`. Portanto, o `Overrides` declaração de classe `D` substitui o método introduzido pela classe `C`, e não é possível para a classe `D` substituir o método introduzido pela classe `A`. O exemplo produz a saída:

```
B.F
B.F
D.F
D.F
```

É possível invocar o oculto `Overridable` método acessando uma instância da classe `D` por meio de um tipo menos derivado em que o método não está oculto.

Não é válido para sombrear um `MustOverride` método, pois na maioria dos casos isso tornaria a classe inutilizável. Por exemplo:

```vb
MustInherit Class Base
    Public MustOverride Sub F()
End Class

MustInherit Class Derived
    Inherits Base

    Public Shadows Sub F()
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    ' Error: MustOverride method Base.F is not overridden.
End Class
```

Nesse caso, a classe `MoreDerived` é necessário para substituir o `MustOverride` método `Base.F`, mas porque a classe `Derived` sombras `Base.F`, isso não é possível. Não é possível declarar válido descendente de `Derived`.

Em contraste com o sombreamento de um nome de um escopo externo, sombreamento de um nome acessível de um escopo herdado faz com que um aviso a ser relatado, como no exemplo a seguir:

```vb
Class Base
    Public Sub F()
    End Sub

    Private Sub G()
    End Sub 
End Class

Class Derived
    Inherits Base

    Public Sub F() ' Warning: shadowing an inherited name.
    End Sub

    Public Sub G() ' No warning, Base.G is not accessible here.
    End Sub
End Class
```

A declaração do método `F` na classe `Derived` faz com que um aviso a ser relatado. Sombrear um nome herdado especificamente não é um erro, já que seria que impeçam a evolução separada das classes base. Por exemplo, a situação acima pode ter ocorrer porque uma versão posterior da classe `Base` introduzido um método `F` que não estava presente em uma versão anterior da classe. A situação acima foi um erro *qualquer* alteração feita em uma classe base em uma biblioteca de classes separadamente com controle de versão pode causar a classes derivadas para se tornar inválido.

O aviso causado sombreando um nome herdado pode ser eliminado por meio do uso do `Shadows` ou `Overloads` modificador:

```vb
Class Base
    Public Sub F()
    End Sub 
End Class 

Class Derived
    Inherits Base

    Public Shadows Sub F() 'OK.
    End Sub
End Class
```

O `Shadows` modificador indica a intenção de membro herdado de sombra. Não é um erro para especificar o `Shadows` ou `Overloads` modificador se não houver nenhum nome de membro de tipo de sombra.

Uma declaração de um novo membro sombreia um membro herdado somente dentro do escopo do novo membro, como no exemplo a seguir:

```vb
Class Base
    Public Shared Sub F()
    End Sub 
End Class 

Class Derived
    Inherits Base

    Private Shared Shadows Sub F() ' Shadows Base.F in class Derived only.
    End Sub 
End Class 

Class MoreDerived
    Inherits Derived

    Shared Sub G()
        F() ' Invokes Base.F.
    End Sub 
End Class
```

No exemplo acima, a declaração do método `F` na classe `Derived` sombreia o método `F` que foi herdado da classe `Base`, mas como o novo método `F` na classe `Derived` tem `Private` acesso, seu escopo não estende a classe `MoreDerived`. Portanto, a chamada `F()` na `MoreDerived.G` é válida e invocará `Base.F`. No caso de membros de tipo sobrecarregado, todo o conjunto de membros de tipo sobrecarregado é tratado como se tivessem o acesso mais permissível para os fins de sombreamento.

```vb
Class Base
    Public Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Private Shadows Sub F()
    End Sub

    Public Shadows Sub F(i As Integer)
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Sub G()
        F()   ' Error. No accessible member with this signature.
    End Sub
End Class
```

Neste exemplo, mesmo que a declaração de `F()` na `Derived` é declarado com `Private` acessar sobrecarregado `F(Integer)` é declarado com `Public` acesso. Portanto, para fins de sombreamento, o nome `F` na `Derived` é tratado como se fosse `Public`, sombra então ambos os métodos `F` em `Base`.

## <a name="implementation"></a>Implementação

Uma *implementação* relação existe quando um tipo declara que ele implementa uma interface e o tipo implementa todos os membros de tipo da interface. Um tipo que implementa uma interface específica é conversível para essa interface. Interfaces não podem ser instanciados, mas ele é válido para declarar variáveis de interfaces; Essas variáveis só podem ser atribuídas um valor que é de uma classe que implementa a interface. Por exemplo:

```vb
Interface ITestable
    Function Test(value As Byte) As Boolean
End Interface

Class TestableClass
    Implements ITestable

    Function Test(value As Byte) As Boolean Implements ITestable.Test
        Return value > 128
    End Function
End Class

Module Test
    Sub F()
        Dim x As ITestable = New TestableClass
        Dim b As Boolean

        b = x.Test(34)
    End Sub
End Module
```

Um tipo que implementa uma interface com membros de tipo herdado multiplicar ainda deve implementar esses métodos, mesmo que eles não podem ser acessados diretamente da interface derivada que está sendo implementado. Por exemplo:

```vb
Interface ILeft
    Sub Test()
End Interface

Interface IRight
    Sub Test()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight
End Interface

Class LeftRight
    Implements ILeftRight

    ' Has to reference ILeft explicitly.
    Sub TestLeft() Implements ILeft.Test
    End Sub

    ' Has to reference IRight explicitly.
    Sub TestRight() Implements IRight.Test
    End Sub

    ' Error: Test is not available in ILeftRight.
    Sub TestLeftRight() Implements ILeftRight.Test
    End Sub
End Class
```

Até mesmo `MustInherit` classes devem fornecer implementações de todos os membros de interfaces implementadas; no entanto, pode adiar a implementação desses métodos, declarando-os como `MustOverride`. Por exemplo:

```vb
Interface ITest
    Sub Test1()
    Sub Test2()
End Interface

MustInherit Class TestBase
    Implements ITest

    ' Provides an implementation.
    Sub Test1() Implements ITest.Test1
    End Sub

    ' Defers implementation.
    MustOverride Sub Test2() Implements ITest.Test2
End Class

Class TestDerived
    Inherits TestBase

    ' Have to implement MustOverride method.
    Overrides Sub Test2()
    End Sub
End Class
```

Um tipo pode optar por implementar novamente uma interface que implementa seu tipo base. Para reimplementar a interface, o tipo deve declarar explicitamente que ele implementa a interface. Um tipo que implementa novamente uma interface pode optar por reimplementar apenas algumas, mas não todos, os membros da interface – todos os membros não implementados novamente continuam a usar a implementação do tipo base. Por exemplo:

```vb
Class TestBase
    Implements ITest

    Sub Test1() Implements ITest.Test1
        Console.WriteLine("TestBase.Test1")
    End Sub

    Sub Test2() Implements ITest.Test2
        Console.WriteLine("TestBase.Test2")
    End Sub
End Class

Class TestDerived
    Inherits TestBase
    Implements ITest  ' Required to re-implement

    Sub DerivedTest1() Implements ITest.Test1
        Console.WriteLine("TestDerived.DerivedTest1")
    End Sub
End Class

Module Test
    Sub Main()
        Dim Test As ITest = New TestDerived()
        Test.Test1()
        Test.Test2()
    End Sub
End Module
```

Este exemplo imprime:

```
TestDerived.DerivedTest1
TestBase.Test2
```

Quando um tipo derivado implementa uma interface cujas base interfaces são implementadas por tipos de base do tipo derivado, o tipo derivado pode optar por implementar apenas a membros de tipo da interface que já não são implementados pelos tipos de base. Por exemplo:

```vb
Interface IBase
    Sub Base()
End Interface

Interface IDerived
    Inherits IBase

    Sub Derived()
End Interface

Class Base
    Implements IBase

    Public Sub Base() Implements IBase.Base
    End Sub
End Class

Class Derived
    Inherits Base
    Implements IDerived

    ' Required: IDerived.Derived not implemented by Base.
    Public Sub Derived() Implements IDerived.Derived
    End Sub
End Class
```

Um método de interface também pode ser implementado usando um método substituível em um tipo base. Nesse caso, um tipo derivado também pode substituir o método substituível e alterar a implementação da interface. Por exemplo:

```vb
Class Base
    Implements ITest

    Public Sub Test1() Implements ITest.Test1
        Console.WriteLine("TestBase.Test1")
    End Sub

    Public Overridable Sub Test2() Implements ITest.Test2
        Console.WriteLine("TestBase.Test2")
    End Sub
End Class

Class Derived
    Inherits Base

    ' Overrides base implementation.
    Public Overrides Sub Test2()
        Console.WriteLine("TestDerived.Test2")
    End Sub
End Class
```

### <a name="implementing-methods"></a>Implementação de métodos

Um tipo *implementa* um membro de tipo de uma interface implementada, fornecendo um método com um `Implements` cláusula. Os membros de dois tipo devem ter o mesmo número de parâmetros, todos os tipos e os modificadores dos parâmetros devem corresponder, incluindo o valor padrão de parâmetros opcionais, o tipo de retorno deve corresponder ao e todas as restrições em parâmetros do método devem corresponder. Por exemplo:

```vb
Interface ITest
    Sub F(ByRef x As Integer)
    Sub G(Optional y As Integer = 20)
    Sub H(Paramarray z() As Integer)
End Interface

Class Test
    Implements ITest

    ' Error: ByRef/ByVal mismatch.
    Sub F(x As Integer) Implements ITest.F
    End Sub

    ' Error: Defaults do not match.
    Sub G(Optional y As Integer = 10) Implements ITest.G
    End Sub

    ' Error: Paramarray does not match.
    Sub H(z() As Integer) Implements ITest.H
    End Sub
End Class
```

Um único método pode implementar qualquer número de membros de tipo de interface se atenderem os critérios acima. Por exemplo:

```vb
Interface ITest
    Sub F(i As Integer)
    Sub G(i As Integer)
End Interface

Class Test

    Implements ITest

    Sub F(i As Integer) Implements ITest.F, ITest.G
    End Sub
End Class
```

Ao implementar um método em uma interface genérica, o método de implementação deve fornecer os argumentos de tipo que correspondem aos parâmetros de tipo da interface. Por exemplo:

```vb
Interface I1(Of U, V) 
    Sub M(x As U, y As List(Of V)) 
End Interface

Class C1(Of W, X)
    Implements I1(Of W, X)

    ' W corresponds to U and X corresponds to V
    Public Sub M(x As W, y As List(Of X)) Implements I1(Of W, X).M
    End Sub 
End Class

Class C2
    Implements I1(Of String, Integer)

    ' String corresponds to U and Integer corresponds to V
    Public Sub M(x As String, y As List(Of Integer)) _
        Implements I1(Of String, Integer).M
    End Sub
End Class
```

Observe que é possível que uma interface genérica pode não ser implementável do mesmo conjunto de argumentos de tipo.

```vb
Interface I1(Of T, U)
    Sub S1(x As T)
    Sub S1(y As U)
End Interface

Class C1
    ' Unable to implement because I1.S1 has two identical signatures
    Implements I1(Of Integer, Integer)
End Class
```

## <a name="polymorphism"></a>Polimorfismo

*Polimorfismo* fornece a capacidade de variar a implementação de um método ou propriedade. Com o polimorfismo, o mesmo método ou propriedade pode executar ações diferentes dependendo do tipo de tempo de execução da instância que invoca a ele. São chamadas de métodos ou propriedades que são polimórficas *substituível*. Por outro lado, a implementação de um método não pode ser substituído ou propriedade é invariável; a implementação é o mesmo se a propriedade ou método é invocada em uma instância da classe na qual ele é declarado ou uma instância de uma classe derivada. Quando um método não pode ser substituído ou propriedade é invocada, o tipo de tempo de compilação da instância é o fator determinante. Por exemplo:

```vb
Class Base
    Public Overridable Property X() As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

Class Derived
    Inherits Base

    Public Overrides Property X() As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

Module Test
    Sub F()
        Dim Z As Base

        Z = New Base()
        Z.X = 10            ' Calls Base.X
        Z = New Derived()
        Z.X = 10            ' Calls Derived.X
    End Sub
End Module
```

Também pode ser um método substituível `MustOverride`, que significa que ele fornece nenhum corpo de método e deve ser substituído. `MustOverride` os métodos são permitidos apenas em `MustInherit` classes.

No exemplo a seguir, a classe `Shape` define o conceito abstrato de um objeto de forma geométrica que pode pintar em si:

```vb
MustInherit Public Class Shape
    Public MustOverride Sub Paint(g As Graphics, r As Rectangle)
End Class 

Public Class Ellipse
    Inherits Shape

    Public Overrides Sub Paint(g As Graphics, r As Rectangle)
        g.drawEllipse(r)
    End Sub 
End Class 

Public Class Box
    Inherits Shape

    Public Overrides Sub Paint(g As Graphics, r As Rectangle)
        g.drawRect(r)
    End Sub 
End Class
```

O `Paint` método é `MustOverride` porque não há nenhuma implementação padrão significativos. O `Ellipse` e `Box` classes são concretas `Shape` implementações. Como essas classes não são `MustInherit`, eles são necessários para substituir o `Paint` método e fornecer uma implementação real.

É um erro para um acesso de base fazer referência a um `MustOverride` método, como o exemplo a seguir demonstra:

```vb
MustInherit Class A
    Public MustOverride Sub F()
End Class

Class B
    Inherits A

    Public Overrides Sub F()
        MyBase.F() ' Error, MyBase.F is MustOverride.
    End Sub 
End Class
```

Um erro é relatado para o `MyBase.F()` invocação porque faz referência a um `MustOverride` método.

### <a name="overriding-methods"></a>Substituindo métodos

Um tipo talvez *substituir* um método substituível herdado, declarando um método com o mesmo nome e, assinatura e marcando a declaração com o `Overrides` modificador. Há requisitos adicionais sobre substituição de métodos, listados abaixo. Enquanto um `Overridable` declaração de método apresenta um novo método, um `Overrides` declaração de método substitui a implementação herdada do método.

Um método de substituição pode ser declarado `NotOverridable`, que impede que qualquer substituição ainda mais o método de tipos derivados. Na verdade, `NotOverridable` classes derivadas de métodos de se tornar não-substituíveis em qualquer outra.

Considere o exemplo a seguir:

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub

    Public Overridable Sub G()
        Console.WriteLine("A.G")
    End Sub
End Class

Class B
    Inherits A

    Public Overrides NotOverridable Sub F()
        Console.WriteLine("B.F")
    End Sub

    Public Overrides Sub G()
        Console.WriteLine("B.G")
    End Sub
End Class

Class C
    Inherits B

    Public Overrides Sub G()
        Console.WriteLine("C.G")
    End Sub
End Class
```

No exemplo, a classe `B` fornece dois `Overrides` métodos: um método `F` que tem o `NotOverridable` modificador e um método `G` que não faz. Usar o `NotOverridable` modificador impede que a classe `C` de substituindo o método `F`.

Um método de substituição também pode ser declarado `MustOverride`, mesmo que não está declarado como o método que está substituindo `MustOverride`. Isso requer que a classe continente sejam declaradas `MustInherit` e que qualquer ainda mais as classes derivadas que não são declaradas `MustInherit` deve substituir o método. Por exemplo:

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub
End Class

MustInherit Class B
    Inherits A

    Public Overrides MustOverride Sub F()
End Class
```

No exemplo, a classe `B` substituições `A.F` com um `MustOverride` método. Isso significa que todas as classes derivadas de `B` terão de substituir `F`, a menos que elas são declaradas `MustInherit` também.

Um erro de tempo de compilação ocorre, a menos que todos os itens a seguir são verdadeiras para um método de substituição:

* O contexto da declaração contém um único método herdado acessível com a mesma assinatura e o tipo de retorno (se houver) como o método de substituição.
* O método herdado que está sendo substituído é substituível. Em outras palavras, o método herdado que está sendo substituído não é `Shared` ou `NotOverridable`.
* O domínio de acessibilidade do método que está sendo declarado é o mesmo que o domínio de acessibilidade do método herdado que está sendo substituído. Há uma exceção: um `Protected Friend` método deve ser substituído por um `Protected` se o outro método estiver em outro assembly que não tem o método de substituição de método `Friend` acesso.
* Os parâmetros do método de substituição correspondem aos parâmetros do método substituído no que diz respeito ao uso do `ByVal`, `ByRef`, `ParamArray,` e `Optional` modificadores, incluindo os valores fornecidos para parâmetros opcionais.
* Os parâmetros de tipo de método de substituição correspondem os parâmetros de tipo do método substituído no que diz respeito a restrições de tipo.

Ao substituir um método em um tipo genérico de base, o método de substituição deve fornecer os argumentos de tipo que correspondem aos parâmetros de tipo base. Por exemplo:

```vb
Class Base(Of U, V) 
    Public Overridable Sub M(x As U, y As List(Of V)) 
    End Sub
End Class

Class Derived(Of W, X)
    Inherits Base(Of W, X)

    ' W corresponds to U and X corresponds to V
    Public Overrides Sub M(x As W, y As List(Of X)) 
    End Sub 
End Class

Class MoreDerived
    Inherits Derived(Of String, Integer)

    ' String corresponds to U and Integer corresponds to V
    Public Overrides Sub M(x As String, y As List(Of Integer))
    End Sub
End Class
```

Observe que é possível que um método substituível em uma classe genérica não poderá ser substituído para alguns conjuntos de argumentos de tipo. Se o método for declarado `MustOverride`, isso significa que algumas cadeias de herança podem não ser possíveis. Por exemplo:

```vb
MustInherit Class Base(Of T, U)
    Public MustOverride Sub S1(x As T)
    Public MustOverride Sub S1(y As U)
End Class

Class Derived
    Inherits Base(Of Integer, Integer)

    ' Error: Can't override both S1's at once
    Public Overrides Sub S1(x As Integer)
    End Sub
End Class
```

Uma declaração de substituição pode acessar o método base substituído usando um acesso de base, como no exemplo a seguir:

```vb
Class Base
    Private x As Integer

    Public Overridable Sub PrintVariables()
        Console.WriteLine("x = " & x)
    End Sub
End Class

Class Derived
    Inherits Base

    Private y As Integer

    Public Overrides Sub PrintVariables()
        MyBase.PrintVariables()
        Console.WriteLine("y = " & y)
    End Sub
End Class
```

No exemplo, a invocação de `MyBase.PrintVariables()` na classe `Derived` invoca o `PrintVariables` método declarado na classe `Base`. Um acesso de base desabilita o mecanismo de invocação substituíveis e simplesmente trata o método base como um método não pode ser substituído. Teve a invocação no `Derived` foi gravado `CType(Me, Base).PrintVariables()`, seria recursivamente invocar o `PrintVariables` método declarado na `Derived`, não aquela declarada em `Base`.

Somente quando ele inclui um `Overrides` can modificador um método substituem outro método. Em todos os outros casos, um método com a mesma assinatura que um método herdado sombreia simplesmente o método herdado, como no exemplo a seguir:

```vb
Class Base
    Public Overridable Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Public Overridable Sub F() ' Warning, shadowing inherited F().
    End Sub
End Class
```

No exemplo, o método `F` na classe `Derived` não inclui um `Overrides` modificador e, portanto, não substitui o método `F` na classe `Base`. Em vez disso, o método `F` na classe `Derived` sombreia um método na classe `Base`, e um aviso é relatado como a declaração não inclui um `Shadows` ou `Overloads` modificador.

No exemplo a seguir, o método `F` na classe `Derived` sombreia um método substituível `F` herdada da classe `Base`:

```vb
Class Base
    Public Overridable Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Private Shadows Sub F() ' Shadows Base.F within Derived.
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Overrides Sub F() ' Ok, overrides Base.F.
    End Sub
End Class
```

Desde o novo método `F` na classe `Derived` tem `Private` acesso, seu escopo inclui apenas o corpo da classe da `Derived` e não se estende a classe `MoreDerived`. A declaração do método `F` na classe `MoreDerived` , portanto, tem permissão para substituir o método `F` herdada da classe `Base`.

Quando um `Overridable` método é invocado, a implementação mais derivada do método de instância é chamada, com base no tipo de instância, independentemente de é a chamada para o método na classe base ou classe derivada. A implementação mais derivada de uma `Overridable` método `M` em relação a uma classe `R` é determinado da seguinte maneira:

* Se `R` contém o apresentando `Overridable` declaração de `M`, esta é a implementação mais derivada de `M`.

* Caso contrário, se `R` contém uma substituição do `M`, essa é a implementação mais derivada de `M`.

* Caso contrário, a maioria implementação derivada da `M` é o mesmo que a classe base direta de `R`.

## <a name="accessibility"></a>Acessibilidade

Especifica uma declaração de *acessibilidade* da entidade, ela declara. Acessibilidade de uma entidade não altera o escopo do nome da entidade. O *domínio de acessibilidade* de uma declaração é o conjunto de todos os espaços de declaração na qual a entidade declarada é acessível.

São os tipos de cinco acesso `Public`, `Protected`, `Friend`, `Protected Friend`, e `Private`. `Public` é o tipo de acesso mais permissivo, e os outros quatro tipos são todos os subconjuntos de `Public`. É o tipo de acesso menos permissivo `Private`, e quatro outros tipos de acesso são todos os subconjuntos de `Private`.

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

O tipo de acesso para uma declaração é especificado por meio de um modificador de acesso opcional, que pode ser `Public`, `Protected`, `Friend`, `Private`, ou uma combinação de `Protected` e `Friend`. Se nenhum modificador de acesso for especificado, o tipo de acesso padrão depende do contexto da declaração; os tipos de acesso permitido também dependem do contexto da declaração.

* Entidades declaradas com o `Public` modificador ter `Public` acesso. Não existem restrições no uso de `Public` entidades.

* Entidades declaradas com o `Protected` modificador ter `Protected` acesso. `Protected` acesso só pode ser especificado em membros de classes (membros de tipo regular e classes aninhadas) ou no `Overridable` membros de estruturas e os módulos padrão (que deve, por definição, ser herdada `System.Object` ou `System.ValueType`). Um `Protected` membro é acessível por uma classe derivada, desde que o membro não é um membro de instância, ou o acesso ocorre por meio de uma instância da classe derivada. `Protected` acesso não é um superconjunto de `Friend` acesso.

* Entidades declaradas com o `Friend` modificador ter `Friend` acesso. Uma entidade com `Friend` o acesso é acessível somente dentro do programa que contém a declaração de entidade ou todos os assemblies que receberam `Friend` acessar por meio de `System.Runtime.CompilerServices.InternalsVisibleToAttribute` atributo.

* Entidades declaradas com o `Protected Friend` modificadores têm a união de `Protected` e `Friend` acesso.

* Entidades declaradas com o `Private` modificador ter `Private` acesso. Um `Private` entidade é acessível somente dentro de seu contexto de declaração, incluindo quaisquer entidades aninhadas.

A acessibilidade em uma declaração não depende da acessibilidade do contexto da declaração. Por exemplo, um tipo declarado com `Private` acesso pode conter um membro de tipo `Public` acesso.

O código a seguir demonstra vários domínios de acessibilidade:

```vb
Public Class A
    Public Shared X As Integer
    Friend Shared Y As Integer
    Private Shared Z As Integer
End Class

Friend Class B
    Public Shared X As Integer
    Friend Shared Y As Integer
    Private Shared Z As Integer

    Public Class C
        Public Shared X As Integer
        Friend Shared Y As Integer
        Private Shared Z As Integer
    End Class

    Private Class D
        Public Shared X As Integer
        Friend Shared Y As Integer
        Private Shared Z As Integer
    End Class
End Class
```

As classes e membros neste exemplo tem os domínios de acessibilidade a seguir:

* Domínio de acessibilidade de `A` e `A.X` é ilimitado.

* Domínio de acessibilidade de `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, e `B.C.Y` é o programa de recipiente.

* Domínio de acessibilidade de `A.Z` é `A.`

* Domínio de acessibilidade de `B.Z`, `B.D`, `B.D.X`, e `B.D.Y` está `B`, incluindo `B.C` e `B.D`.

* Domínio de acessibilidade de `B.C.Z` é `B.C`.

* Domínio de acessibilidade de `B.D.Z` é `B.D`.

Como mostra o exemplo, o domínio de acessibilidade de um membro nunca é maior do que um tipo contido. Por exemplo, mesmo que todos os `X` os membros têm `Public` declarado acessibilidade, tudo, exceto `A.X` tem domínios de acessibilidade que são restritos por um tipo de contenção.

Acesso a `Protected` membros devem ser através de uma instância do tipo derivado para que não relacionados de tipos não podem acessar uns aos outros de instância tem membros protegidos. Por exemplo:

```vb
Class User
    Protected Password As String
End Class

Class Employee
    Inherits User
End Class

Class Guest
    Inherits User

    Public Function GetPassword(u As User) As String
        ' Error: protected access has to go through derived type.
        Return U.Password
    End Function
End Class
```

No exemplo acima, a classe `Guest` tem acesso apenas às protegido `Password` campo se ele for qualificado com uma instância de `Guest`. Isso impede que `Guest` obtenham acesso para o `Password` campo de uma `Employee` objeto simplesmente ao convertê-la para `User`.

Para fins de `Protected` acesso de membro em tipos genéricos, o contexto da declaração inclui parâmetros de tipo. Isso significa que um tipo derivado com um conjunto de argumentos de tipo não tem acesso para o `Protected` membros de um tipo derivado com um conjunto diferente de argumentos de tipo. Por exemplo:

```vb
Class Base(Of T)
    Protected x As T
End Class

Class Derived(Of T)
    Inherits Base(Of T)

    Public Sub F(y As Derived(Of String))
        ' Error: Derived(Of T) cannot access Derived(Of String)'s 
        '     protected members
        y.x = "a"
    End Sub
End Class
```

__Observação.__ A linguagem c# (e possivelmente outras linguagens) permite que um tipo genérico acessar `Protected` membros, independentemente de quais argumentos de tipo são fornecidos. Isso deve ser mantido em mente ao projetar classes genéricas que contêm `Protected` membros.


### <a name="constituent-types"></a>Tipos constituintes

O *constituintes tipos* de uma declaração são os tipos que são referenciados pela declaração. Por exemplo, o tipo de uma constante, o tipo de retorno de um método e os tipos de parâmetro de um construtor são todos os tipos constituintes. O domínio de acessibilidade de um tipo constituinte de uma declaração deve ser igual ou um superconjunto do domínio de acessibilidade da declaração em si. Por exemplo:

```vb
Public Class X
    Private Class Y
    End Class

    ' Error: Exposing private class Y outside of X.
    Public Function Z() As Y
    End Function

    ' Valid: Not exposing outside of X.
    Private Function A() As Y
    End Function
End Class

Friend Class B
    Private Class C
    End Class

    ' Error: Exposing private class Y outside of B.
    Public Function D() As C
    End Function
End Class
```

## <a name="type-and-namespace-names"></a>Nomes de Namespace e tipo

Muitas construções de linguagem exigem um namespace ou tipo a ser especificado; eles podem ser especificados usando um formulário qualificado de namespace ou nome do tipo. Um *nome qualificado* consiste em uma série de identificadores separados por pontos, o identificador à direita de um período é resolvido no espaço de declaração especificado pelo identificador no lado esquerdo do período.

O *nome totalmente qualificado* de um tipo ou namespace é um nome qualificado que contém o nome de todos os que contém namespaces e tipos. Em outras palavras, o nome totalmente qualificado de um tipo ou namespace é `N.T`, onde `T` é o nome da entidade e `N` é o nome totalmente qualificado da sua entidade contentora.

O exemplo a seguir mostra várias declarações de namespace e o tipo junto com seus nomes totalmente qualificados associados nos comentários na linha.

```vb
Class A            ' A.
End Class

Namespace X        ' X.
    Class B        ' X.B.
        Class C    ' X.B.C.
        End Class
    End Class

    Namespace Y    ' X.Y.
        Class D    ' X.Y.D.
        End Class
    End Namespace 
End Namespace 

Namespace X.Y      ' X.Y.
    Class E        ' X.Y.E.
    End Class
End Namespace
```

Observe que o namespace x. y foi declarada em dois locais diferentes no código-fonte, mas essas duas declarações parciais constituem apenas um único namespace chamado x. y que contém a classe D e a classe E.

Em algumas situações, um nome qualificado pode começar com a palavra-chave `Global`. A palavra-chave representa o namespace externo sem nome, que é útil em situações em que uma declaração se sobrepõe a um namespace delimitador. O `Global` permite a palavra-chave "escape" out para o namespace mais externo nessa situação. Por exemplo:

```vb
Namespace NS1
    Class System
    End Class

    Module Test
        Sub Main()
            ' Error: Class System does not contain Int32
            Dim x As System.Int32


            ' Legal, binds to System in outermost namespace
            Dim y As Global.System.Int32
        End Sub
    End Module
End Namespace
```

No exemplo acima, a primeira chamada de método é inválida porque o identificador `System` associa à classe `System`, não o namespace `System`. A única maneira de acessar o `System` namespace é usar `Global` pulem-out para o namespace mais externo. `Global` não pode ser usado em uma `Imports` instrução ou `Namespace` declaração.

Como outras linguagens podem introduzir tipos e namespaces que correspondem a palavras-chave na linguagem, o Visual Basic reconhece palavras-chave para fazer parte de um nome qualificado, desde que sigam um período. Palavras-chave usadas dessa maneira são tratadas como identificadores. Por exemplo, o identificador qualificado `X.Default.Class` é um identificador qualificado válido, enquanto `Default.Class` não é.

### <a name="qualified-name-resolution-for-namespaces-and-types"></a>Resolução de nome qualificado de namespaces e tipos

Dado um nome qualificado de namespace ou tipo do formulário `N.R(Of A)`, onde `R` é o identificador mais à direita no nome qualificado e `A` é uma lista de argumentos de tipo opcional, as etapas a seguir descrevem como determinar a qual namespace ou o nome qualificado do tipo refere-se:

1. Resolver `N`, usando as regras para a resolução de nome qualificado ou não.

2. Se a resolução de `N` falhar ou se resolve para um parâmetro de tipo, ocorre um erro de tempo de compilação.

3. Caso contrário, se `R` correspondências de nome de um namespace em N e nenhum argumento de tipo foram fornecidos, ou `R` corresponde a um tipo acessível no `N` com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em seguida, o nome qualificado refere-se a esse namespace ou tipo.

4. Caso contrário, se `N` contém um ou mais módulos padrão e `R` corresponde ao nome de um tipo acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em exatamente um módulo padrão e, em seguida, o nome qualificado refere-se ao tipo. Se `R` corresponde ao nome dos tipos acessíveis com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em mais de um módulo padrão, um erro de tempo de compilação ocorrer.

5. Caso contrário, ocorrerá um erro de tempo de compilação.

__Observação.__ Uma implicação desse processo de resolução é que os membros de tipo não sombra namespaces ou tipos durante a resolução de nomes de namespace ou tipo.

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a>Resolução de nomes não qualificados para namespaces e tipos

Dado um nome não qualificado `R(Of A)`, onde `A` é uma lista de argumentos de tipo opcional, as etapas a seguir descrevem como determinar a qual o namespace ou tipo refere-se o nome não qualificado:

1. Se R corresponde ao nome de um parâmetro de tipo do método atual e foi fornecido nenhum argumento de tipo, o nome não qualificado se refere a esse parâmetro de tipo.

2.  Para cada tipo que contém a referência de nome, começando do tipo interno e vai mais externo aninhado:
    21. Se `R` corresponde ao nome de um parâmetro de tipo no tipo atual e nenhum tipo de argumentos foram fornecidos, em seguida, o nome não qualificado se refere a esse parâmetro de tipo.
    22. Caso contrário, se `R` correspondências, o nome de um acessível aninhadas de tipo com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em seguida, o nome não qualificado faz referência a esse tipo.

3. Para cada namespace aninhado que contém a referência de nome, a partir do namespace interno e passando para o namespace externo:
    31. Se `R` correspondências, o nome de um namespace aninhado no namespace atual e nenhuma lista de argumentos de tipo for fornecido, então o nome não qualificado faz referência a esse namespace aninhado.
    32. Caso contrário, se `R` corresponde ao nome de um tipo acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, no namespace atual e, em seguida, o nome não qualificado faz referência a esse tipo.
    33. Caso contrário, se o namespace contém um ou mais acessíveis módulos padrão, e `R` corresponde ao nome de um tipo aninhado acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em exatamente um módulo padrão, então não qualificada nome se refere a esse tipo aninhado. Se `R` corresponde ao nome de tipos aninhados acessíveis com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em mais de um módulo padrão, um erro de tempo de compilação ocorrer.

4. Se o arquivo de origem tem um ou mais aliases de importação e `R` corresponde ao nome de um deles, em seguida, o nome não qualificado se refere a esse alias de importação. Se uma lista de argumentos de tipo for fornecida, ocorrerá um erro de tempo de compilação.

5. Se o arquivo de origem que contém a referência de nome tem um ou mais importações:
    51. Se `R` corresponde ao nome de um tipo acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em Importar exatamente um e, em seguida, o nome não qualificado se refere a esse tipo. Se `R` corresponde ao nome de um tipo acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em mais de uma importação e todos os não são do mesmo tipo, ocorre um erro de tempo de compilação.
    52. Caso contrário, se nenhum tipo de lista de argumentos foi fornecido e `R` corresponde ao nome de um namespace com tipos acessíveis em exatamente uma importação, em seguida, o nome não qualificado faz referência a esse namespace. Se nenhum tipo de lista de argumentos foi fornecido e `R` correspondências, o nome de um namespace com tipos acessíveis em mais de uma importação e todos os não são o mesmo namespace, ocorre um erro de tempo de compilação.
    53. Caso contrário, se as importações contêm um ou mais acessíveis módulos padrão, e `R` coincide com o nome de um tipo aninhado acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em exatamente um módulo padrão, em seguida, o nome não qualificado refere-se a esse tipo. Se `R` corresponde ao nome de tipos aninhados acessíveis com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em mais de um módulo padrão, um erro de tempo de compilação ocorrer.

6. Se o ambiente de compilação define um ou mais aliases de importação e `R` corresponde ao nome de um deles, em seguida, o nome não qualificado se refere a esse alias de importação. Se uma lista de argumentos de tipo for fornecida, ocorrerá um erro de tempo de compilação.

7. Se o ambiente de compilação define uma ou mais importações:
    71. Se `R` corresponde ao nome de um tipo acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em Importar exatamente um e, em seguida, o nome não qualificado se refere a esse tipo. Se `R` corresponde ao nome de um tipo acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em mais de uma importação, um erro de tempo de compilação ocorrer.
    72. Caso contrário, se nenhum tipo de lista de argumentos foi fornecido e `R` corresponde ao nome de um namespace com tipos acessíveis em exatamente uma importação, em seguida, o nome não qualificado faz referência a esse namespace. Se nenhum tipo de lista de argumentos foi fornecido e `R` corresponde ao nome de um namespace com tipos acessíveis em mais de uma importação, ocorre um erro de tempo de compilação.
    73. Caso contrário, se as importações contêm um ou mais acessíveis módulos padrão, e `R` coincide com o nome de um tipo aninhado acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em exatamente um módulo padrão, em seguida, o nome não qualificado refere-se a esse tipo. Se `R` corresponde ao nome de tipos aninhados acessíveis com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em mais de um módulo padrão, um erro de tempo de compilação ocorrer.

8. Caso contrário, ocorrerá um erro de tempo de compilação.

__Observação.__ Uma implicação desse processo de resolução é que os membros de tipo não sombra namespaces ou tipos durante a resolução de nomes de namespace ou tipo.

Normalmente, um nome pode ocorrer apenas uma vez em um namespace específico. No entanto, como namespaces podem ser declarados em vários assemblies do .NET, é possível ter uma situação em que dois assemblies definem um tipo com o mesmo nome totalmente qualificado. Nesse caso, um tipo declarado no conjunto atual de arquivos de origem é preferível ao longo de um tipo declarado em um assembly .NET externo. Caso contrário, o nome é ambíguo e não há nenhuma maneira de resolver a ambiguidade do nome.

## <a name="variables"></a>Variáveis

Um *variável* representa um local de armazenamento. Cada variável tem um tipo que determina quais valores podem ser armazenados na variável. Porque o Visual Basic é uma linguagem fortemente tipada, cada variável em um programa tem um tipo e a linguagem garante que os valores armazenados em variáveis sempre são do tipo apropriado. As variáveis são inicializadas sempre para o valor padrão de seu tipo antes que qualquer referência à variável pode ser feita. Não é possível acessar a memória não inicializada.

## <a name="generic-types-and-methods"></a>Tipos e métodos genéricos

Métodos e tipos (exceto para os módulos padrão e os tipos enumerados) podem declarar *parâmetros de tipo*, que são tipos que não serão fornecidos até que uma instância do tipo é declarado ou o método é invocado. Tipos e métodos com parâmetros de tipo também são conhecidos como *tipos genéricos* e *métodos genéricos*, respectivamente, porque o tipo ou método deve ser escrito de forma genérica, sem conhecimento específico das tipos que serão fornecidos pelo código que usa o tipo ou método.

__Observação.__ Neste momento, mesmo que os delegados e métodos podem ser genéricos, propriedades, eventos e operadores não podem ser genéricos próprios. No entanto, eles podem, usar parâmetros de tipo da classe recipiente.

Da perspectiva de tipo genérico ou método, um parâmetro de tipo é um tipo de espaço reservado que será preenchido com um tipo real quando o tipo ou método é usado. Argumentos de tipo são substituídos por parâmetros de tipo no tipo ou método no ponto em que o tipo ou método é usado. Por exemplo, uma classe de pilha genérico pode ser implementada como:

```vb
Public Class Stack(Of ItemType)
    Protected Items(0 To 99) As ItemType
    Protected CurrentIndex As Integer = 0

    Public Sub Push(data As ItemType)
        If CurrentIndex = 100 Then
            Throw New ArgumentException("Stack is full.")
        End If

        Items(CurrentIndex) = Data
        CurrentIndex += 1
    End Sub

    Public Function Pop() As ItemType
        If CurrentIndex = 0 Then
            Throw New ArgumentException("Stack is empty.")
        End If

        CurrentIndex -= 1
        Return Items(CurrentIndex + 1) 
    End Function
End Class
```

As declarações que usam o `Stack(Of ItemType)` classe deve fornecer um argumento de tipo para o parâmetro de tipo `ItemType`. Esse tipo é preenchido, em seguida, independentemente de onde `ItemType` é usado dentro da classe:

```vb
Option Strict On

Module Test
    Sub Main()
        Dim s1 As New Stack(Of Integer)()
        Dim s2 As New Stack(Of Double)()

        s1.Push(10.10)   ' Error: Stack(Of Integer).Push takes an Integer
        s2.Push(10.10)   ' OK: Stack(Of Double).Push takes a Double
        Console.WriteLine(s2.Pop().GetType().ToString()) ' Prints: Double
    End Sub
End Module
```

### <a name="type-parameters"></a>Parâmetros de tipo

Parâmetros de tipo podem ser fornecidos em declarações de tipo ou método. Cada parâmetro de tipo é um identificador que é um espaço reservado para um argumento de tipo que é fornecido para criar um tipo construído ou método. Por outro lado, um argumento de tipo é o tipo real que é substituído para o parâmetro de tipo quando um tipo genérico ou método é usado.

```antlr
TypeParameterList
    : OpenParenthesis 'Of' TypeParameter ( Comma TypeParameter )* CloseParenthesis
    ;

TypeParameter
    : VarianceModifier? Identifier TypeParameterConstraints?
    ;

VarianceModifier
    : 'In' | 'Out'
    ;
```

Cada parâmetro de tipo em uma declaração de tipo ou método define um nome no espaço de declaração desse tipo ou método. Portanto, ele não pode ter o mesmo nome que outro parâmetro de tipo, um membro de tipo, um parâmetro de método ou uma variável local. O escopo de um parâmetro de tipo em um tipo ou método é o tipo de inteiro ou método. Porque os parâmetros de tipo estão no escopo para a declaração de tipo inteiro, tipos aninhados podem usar parâmetros de tipo externo. Isso também significa que os parâmetros de tipo sempre devem ser especificados quando acessar tipos aninhados dentro de tipos genéricos:

```vb
Public Class Outer(Of T)
    Public Class Inner
        Public Sub F(x As T)
            ...
        End Sub
    End Class
End Class

Module Test
    Sub Main()
        Dim x As New Outer(Of Integer).Inner()
        ...
    End Sub
End Module
```

Ao contrário de outros membros de uma classe, parâmetros de tipo não são herdados. Parâmetros de tipo em um tipo só podem ser referenciados por seus nomes simples; em outras palavras, eles não podem ser qualificados com o nome do tipo recipiente. Embora seja ruim programação estilo, os parâmetros de tipo em um tipo aninhado podem ocultar um membro ou parâmetro declarado no tipo externo do tipo:

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

Tipos e métodos podem ser sobrecarregados com base no número de parâmetros de tipo (ou *arity*) que declaram os tipos ou métodos. Por exemplo, as seguintes declarações são aceitáveis:

```vb
Module C
    Sub M()
    End Sub

    Sub M(Of T)()
    End Sub

    Sub M(Of T, U)()
    End Sub
End Module

Structure C(Of T)
    Dim x As T
End Structure

Class C(Of T, U)
End Class
```

No caso de tipos, sobrecargas sempre são correspondidas em relação ao número de argumentos de tipo especificado. Isso é útil ao usar as classes genéricas e não genéricas juntos no mesmo programa:

```vb
Class Queue 
End Class      

Class Queue(Of T)
End Class

Class X
    Dim q1 As Queue                 ' Non-generic queue
    Dim q2 As Queue(Of Integer)     ' Generic queue
End Class
```

Regras para métodos sobrecarregados em parâmetros de tipo são abordadas na seção sobre resolução de sobrecarga de método.

Dentro da declaração de recipiente, parâmetros de tipo são considerados tipos completos. Uma vez que um parâmetro de tipo pode ser instanciado com muitos argumentos de tipo real diferente, parâmetros de tipo têm um pouco diferentes operações e restrições que outros tipos, conforme descrito abaixo:

* Um parâmetro de tipo não pode ser usado diretamente para declarar uma classe base ou interface.

* As regras para a pesquisa de membro no tipo de parâmetros dependem de restrições, se houver, é aplicado ao parâmetro de tipo.

* As conversões disponíveis para um parâmetro de tipo depende das restrições, se houver, aplicado aos parâmetros de tipo.

* Na ausência de um `Structure` restrição, um valor com um tipo representado por um parâmetro de tipo pode ser comparado com `Nothing` usando `Is` e `IsNot`.

* Um parâmetro de tipo só pode ser usado em uma `New` expressão se o parâmetro de tipo é restrito por um `New` ou um `Structure` restrição.

* Um parâmetro de tipo não pode ser usado em qualquer lugar dentro de uma exceção de atributo dentro de um `GetType` expressão.

* Parâmetros de tipo podem ser usados como argumentos de tipo para outros tipos genéricos e parâmetros.

O exemplo a seguir é um tipo genérico que estende o `Stack(Of ItemType)` classe:

```vb
Class MyStack(Of ItemType)
    Inherits Stack(Of ItemType)

    Public ReadOnly Property Size() As Integer
        Get
            Return CurrentIndex
        End Get
    End Property
End Class
```

Quando uma declaração fornece um argumento de tipo para `MyStack`, o mesmo argumento de tipo será aplicado a `Stack` também.

Como um tipo, parâmetros de tipo são puramente um constructo de tempo de compilação. Em tempo de execução, cada parâmetro de tipo é associado a um tipo de tempo de execução que foi especificado, fornecendo um argumento de tipo para a declaração genérica. Portanto, o tipo de uma variável declarada com um parâmetro de tipo, em tempo de execução, será um tipo não genérico ou um tipo construído específico. O tempo de execução de todas as instruções e expressões que envolvem parâmetros de tipo usa o tipo real que foi fornecido como o argumento de tipo para esse parâmetro.


### <a name="type-constraints"></a>Restrições de tipo

Como um argumento de tipo pode ser qualquer tipo no sistema de tipos, um tipo genérico ou método não é possível fazer suposições sobre um parâmetro de tipo. Portanto, os membros de um parâmetro de tipo são considerados como os membros do tipo `Object`, uma vez que todos os tipos derivam `Object`.

No caso de uma coleção, como `Stack(Of ItemType)`, esse fato talvez não seja uma restrição importante, mas pode haver casos em que um tipo genérico pode querer fazer uma suposição sobre os tipos que serão fornecidos como argumentos de tipo. *Restrições de tipo* podem ser colocados em parâmetros de tipo que restringirem quais tipos podem ser fornecidos como um parâmetro de tipo e permitir que tipos ou métodos genéricos assumir mais informações sobre parâmetros de tipo.

```antlr
TypeParameterConstraints
    : 'As' Constraint
    | 'As' OpenCurlyBrace ConstraintList CloseCurlyBrace
    ;

ConstraintList
    : Constraint ( Comma Constraint )*
    ;

Constraint
    : TypeName
    | 'New'
    | 'Structure'
    | 'Class'
    ;
```


```vb
Public Class DisposableStack(Of ItemType As IDisposable)
    Implements IDisposable

    Private _items(0 To 99) As ItemType
    Private _currentIndex As Integer = 0

    Public Sub Push(data As ItemType)
        ...
    End Sub

    Public Function Pop() As ItemType
        ...
    End Function

    Private Sub Dispose() Implements IDisposable.Dispose
        For Each item As IDisposable In _items
            If item IsNot Nothing Then
                item.Dispose()
            End If
        Next item
    End Sub
End Class
```

Neste exemplo, o `DisposableStack(Of ItemType)` restringe seu parâmetro de tipo para somente os tipos que implementam a interface `System.IDisposable`. Como resultado, ela pode implementar um `Dispose` método que descarta todos os objetos ainda deixada na fila.

Uma restrição de tipo deve ser uma das restrições especiais `Class`, `Structure`, ou `New`, ou deve ser um tipo `T` onde:

* `T` é uma classe, uma interface ou um parâmetro de tipo.

* `T` não é `NotInheritable`.

* `T` não é um dos ou um tipo herdado de um dos seguintes tipos especiais: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, ou `System.ValueType`.

* `T` não é `Object`. Uma vez que todos os tipos derivam `Object`, tal restrição não terá efeito algum se ele eram permitido.

* `T` deve ser pelo menos tão acessíveis quanto o tipo genérico ou método que está sendo declarado.

Várias restrições de tipo podem ser especificadas para um único parâmetro de tipo, colocando as restrições de tipo entre chaves (`{}`)... Restrição de apenas um tipo para um parâmetro de tipo determinado pode ser uma classe. É um erro para combinar uma `Structure` restrição especial com uma restrição de classe nomeada ou a `Class` restrição especial.

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

Restrições de tipo podem usar os tipos de recipientes ou qualquer um dos parâmetros de tipo dos tipos recipiente. No exemplo a seguir, a restrição requer que o argumento de tipo fornecido implementa uma interface genérica que usa a mesmo como um argumento de tipo:

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

A restrição de tipo especial `Class` restringe o argumento de tipo fornecido para qualquer tipo de referência.

__Observação.__ A restrição de tipo especial `Class` podem ser atendidas por uma interface. E uma estrutura pode implementar uma interface. Portanto, a restrição `(Of T As U, U As Class)` pode ser satisfeito com uma estrutura de "T" (que não satisfaz a `Class` restrição especial) e "U" uma interface que ele implementa (que atende a `Class` restrição especial).

A restrição de tipo especial `Structure` restringe o argumento de tipo fornecido para qualquer tipo de valor, exceto `System.Nullable(Of T)`.

__Observação.__ Restrições de estrutura não permitem `System.Nullable(Of T)` para que ele não é possível fornecer `System.Nullable(Of T)` como um argumento de tipo a mesmo.

A restrição de tipo especial `New` requer que o argumento de tipo fornecido deve ter um construtor sem parâmetros acessível e não pode ser declarado `MustInherit`. Por exemplo:

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

Uma restrição de tipo de classe requer que o argumento de tipo fornecido deve ser que digitar como ou herdar dele. Uma restrição de tipo de interface requer que o argumento de tipo fornecido deve implementar essa interface. Uma restrição de parâmetro de tipo exige que o argumento de tipo fornecido deve derivar de ou implementar todos os limites fornecidos para o parâmetro de tipo correspondente. Por exemplo:

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

Neste exemplo, o parâmetro de tipo `S` na `AddRange` é restrito para o parâmetro de tipo `T` de `List`. Isso significa que um `List(Of Control)` seria restringir `AddRange`do parâmetro para qualquer tipo que herda de ou que é do tipo `Control`.

Uma restrição de parâmetro de tipo `Of S As T` é resolvido adicionando transitivamente todas `T`do restrições em `S`, em vez das restrições especiais (`Class`, `Structure`, `New`). É um erro ter restrições circulares (por exemplo, `Of S As T, T As S`). É um erro ter uma restrição de parâmetro de tipo que por si só tem o `Structure` restrição. Depois de adicionar restrições, é possível que um número de situações especiais pode ocorrer:

* Se houver várias restrições de classe, a classe mais derivada é considerada a restrição. Se uma ou mais restrições de classe não têm nenhuma relação de herança, a restrição é insatisfatórios e é um erro.

 * Se um parâmetro de tipo combina uma `Structure` restrição especial com uma restrição de classe nomeada ou o `Class` restrição especial, ele é um erro. Uma restrição de classe pode ser `NotInheritable`, caso em que nenhum tipo derivado de restrição é aceitos, e é um erro.

O tipo pode ser um dos ou um tipo herdadas, os seguintes tipos especiais: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, ou `System.ValueType`. Nesse caso, apenas o tipo ou um tipo herdado dele, é aceita. Um parâmetro de tipo restringido a um desses tipos só pode usar as conversões permitidas pelo `DirectCast` operador. Por exemplo:

```vb
MustInherit Class Base(Of T)
    MustOverride Sub S1(Of U As T)(x As U)
End Class

Class Derived
    Inherits Base(Of Integer)

    ' The constraint of U must be Integer, which is normally not allowed.
    Overrides Sub S1(Of U As Integer)(x As U)
        Dim y As Integer = x    ' OK
        Dim z As Long = x       ' Error: Can't convert
    End Sub
End Class
```

Além disso, um parâmetro de tipo restringido a um tipo de valor devido a um das reduções acima não é possível chamar quaisquer métodos definidos nesse tipo de valor. Por exemplo:

```vb
Class C1(Of T)
    Overridable Sub F(Of G As T)(x As G)
    End Sub
End Class

Class C2
    Inherits C1(Of IntPtr)

    Overrides Sub F(Of G As IntPtr)(ByVal x As G)
        ' Error: Cannot access structure members
         x.ToInt32()
    End Sub
End Class
```

Se a restrição, depois de substituição, acaba como um tipo de matriz, qualquer tipo de matriz de covariante é permitido também. Por exemplo:

```vb
Module Test
    Class B
    End Class

    Class D
        Inherits B
    End Class

    Function F(Of T, U As T)(x As U) As T
        Return x
    End Function

    Sub Main()
        Dim a(9) As B
        Dim b(9) As D

        a = F(Of B(), D())(b)
    End Sub
End Module
```

Um parâmetro de tipo com uma restrição de classe ou interface é considerado para ter os mesmos membros que essa restrição de classe ou interface. Se um parâmetro de tipo tiver múltiplas restrições, o parâmetro de tipo é considerado para ter a união de todos os membros das restrições. Se houver membros com o mesmo nome em mais de uma restrição, então os membros estão ocultos na seguinte ordem: a restrição de classe oculta membros em restrições de interface, que ocultar membros em `System.ValueType` (se `Structure` restrição for especificada), que oculta os membros em `Object`. Se um membro com o mesmo nome for exibido em mais de uma restrição de interface, o membro não está disponível (como em várias heranças de interface) e o parâmetro de tipo deve ser convertido para a interface desejada. Por exemplo:

```vb
Class C1
    Sub S1(x As Integer)
    End Sub
End Class

Interface I1
    Sub S1(x As Integer)
End Interface

Interface I2
    Sub S1(y As Double)
End Interface

Module Test
    Sub T1(Of T As {C1, I1, I2})()
        Dim a As T
        a.S1(10)       ' Calls C1.S1, which is preferred
        a.S1(10.10)    ' Also calls C1.S1, class is still preferred
    End Sub

    Sub T2(Of T As {I1, I2})()
        Dim a As T
        a.S1(10)    ' Error: Call is ambiguous between I1.S1, I2.S1
    End Sub
End Module
```

Ao fornecer parâmetros de tipo como argumentos de tipo, os parâmetros de tipo devem satisfazer as restrições dos parâmetros de tipo correspondente.

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

Valores de um parâmetro de tipo restrita podem ser usados para acessar os membros de instância, incluindo métodos de instância, especificados na restrição.

```vb
Interface IPrintable
    Sub Print()
End Interface

Class Printer(Of V As IPrintable)
    Sub PrintOne(v1 As V)
        V1.Print()
    End Sub
End Class
```

### <a name="type-parameter-variance"></a>Variação de parâmetro de tipo

Um parâmetro de tipo em uma interface ou uma declaração de tipo de delegado pode, opcionalmente, especificar uma *modificador de variação*. Parâmetros de tipo com modificadores de variação restringem como o parâmetro de tipo pode ser usado no tipo de interface ou delegado, mas permitir que um tipo de interface ou delegado genérico a ser convertido em outro tipo genérico com argumentos de tipo compatível com variante. Por exemplo:

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub Main()
        Dim x As IEnumerable(Of Derived) = ...

        ' OK, as IEnumerable(Of Base) is variant compatible
        ' with IEnumerable(Of Derived)
        Dim y As IEnumerable(Of Base) = x
    End Sub
End Module
```

Interfaces genéricas que têm parâmetros de tipo com modificadores de variação têm várias restrições:

* Elas não podem conter uma declaração de evento que especifica uma lista de parâmetros (mas uma declaração de evento personalizado ou uma declaração de evento com um tipo delegado é permitida).

* Eles não podem conter uma classe aninhada, estrutura ou tipo enumerado.

__Observação.__ Essas restrições são devido ao fato de que os tipos aninhados em tipos genéricos implicitamente copiar os parâmetros genéricos de seu pai. No caso de tipos enumerados, estruturas ou classes aninhadas, esses tipos de tipos não podem ter modificadores de variação em seus parâmetros de tipo. No caso de uma declaração de evento com uma lista de parâmetros, a classe delegate aninhado gerado poderia ter confuso erros quando um tipo que aparece para ser usado em uma `In` posição (ou seja, um tipo de parâmetro), na verdade, é usada em um `Out` posição (ou seja, o tipo de evento).

É um parâmetro de tipo que é declarado com o modificador Out *covariante*. Informalmente, um parâmetro de tipo covariante só pode ser usado em uma posição de saída – ou seja, um valor que está sendo retornado do tipo de interface ou delegado – e não pode ser usado em uma posição de entrada. Um tipo `T` é considerado *válido covariantly* se:

* `T` é uma classe, estrutura ou tipo enumerado.

* `T` é o tipo de delegado ou interface não genérica.

* `T` é um tipo de matriz cujo tipo de elemento covariantly é válido.

* `T` é um parâmetro de tipo que não foi declarado como um `Out` parâmetro de tipo.

* `T` é um tipo de interface ou delegado construído `X(Of P1,...,Pn)` com argumentos de tipo `A1,...,An` , de modo que:

  * Se `Pi` foi declarado como um parâmetro de tipo Out, em seguida, `Ai` é covariantly válido.

  * Se `Pi` foi declarado como um parâmetro de tipo, em seguida, `Ai` é contravariantly válido.

O exemplo a seguir deve ser válidos covariantly em um tipo de interface ou delegado:

* A interface base de uma interface.

* O tipo de retorno de uma função ou o tipo de delegado.

* O tipo de uma propriedade, se houver um `Get` acessador.

* O tipo de qualquer `ByRef` parâmetro.

Por exemplo:

```vb
Delegate Function D(Of Out T, U)(x As U) As T

Interface I1(Of Out T)
End Interface

Interface I2(Of Out T)
    Inherits I1(Of T)

    ' OK, T is only used in an Out position
    Function M1(x As I1(Of T)) As T

    ' Error: T is used in an In position
    Function M2(x As T) As T
End Interface
```

__Observação.__ `Out` não é uma palavra reservada.

É um parâmetro de tipo que é declarado com o modificador In *contravariante*. Informalmente, um parâmetro de tipo contravariante só pode ser usado em uma posição de entrada – ou seja, um valor que está sendo passado para o tipo de interface ou delegado – e não pode ser usado em uma posição de saída. Um tipo `T` é considerado *contravariantly válido* se:

* `T` é uma classe, estrutura ou tipo enumerado.

* `T` é um tipo de delegado ou interface não genérica.

* `T` é um tipo de matriz cujo tipo de elemento é contravariantly válido.

* `T` é um parâmetro de tipo que não foi declarado como um parâmetro de tipo.

* `T` é um tipo de interface ou delegado construído `X(Of P1,...,Pn)` com argumentos de tipo `A1,...,An` , de modo que:

  * Se `Pi` foi declarado como um `Out` , em seguida, o parâmetro de tipo `Ai` é contravariantly válido.

  * Se `Pi` foi declarado como um `In` , em seguida, o parâmetro de tipo `Ai` é covariantly válido.

A seguir devem ser contravariantly válido em um tipo de interface ou delegado:

* O tipo de um parâmetro.

* Uma restrição de tipo em um parâmetro de tipo de método.

* O tipo de uma propriedade, se ele tiver um `Set` acessador.

* O tipo de um evento.

Por exemplo:

```vb
Delegate Function D(Of T, In U)(x As U) As T

Interface I1(Of In T)
End Interface

Interface I2(Of In T)
    ' OK, T is only used in an In position
    Sub M1(x As I1(Of T))

    ' Error: T is used in an Out position
    Function M2() As T
End Interface
```

No caso em que um tipo deve ser válido ser contravariantly e covariantly (como uma propriedade tanto com um `Get` e `Set` acessador ou um `ByRef` parâmetro), um parâmetro de tipo de variante não pode ser usado.


Covariância e contravariância dar origem a um problema de ambiguidade"diamond". Considere o código a seguir:

```vb
Class C
    Implements IEnumerable(Of String)
    Implements IEnumerable(Of Exception)
     
    Public Function GetEnumerator1() As IEnumerator(Of String) _
       Implements IEnumerable(Of String).GetEnumerator
       Console.WriteLine("string")
    End Function
     
    Public Function GetEnumerator2() As IEnumerator(Of Exception) _
       Implements IEnumerable(Of Execption).GetEnumerator
       Console.WriteLine("exception")
    End Function
End Class
     
Dim c As IEnumerable(Of Object) = New C
c.GetEnumerator()
```

A classe `C` pode ser convertido em `IEnumerable(Of Object)` de duas maneiras, por meio de conversão de covariante do `IEnumerable(Of String)` e por meio de conversão de covariante do `IEnumerable(Of Exception)`. O CLR não especifica qual dos dois métodos serão chamados pelo `c.GetEnumerator()`. Em geral, sempre que uma classe é declarada para implementar uma interface covariante com dois argumentos genéricos diferentes que têm um supertipo comum (por exemplo, nesse caso `String` e `Exception` têm um supertipo comum `Object`), ou uma classe é declarada para implementar uma interface contravariante com dois argumentos genéricos diferentes que têm um subtipo comum, então a ambiguidade é surgirão. O compilador dá um aviso sobre tais declarações.
