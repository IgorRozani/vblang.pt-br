# <a name="types"></a>Tipos

São as duas categorias fundamentais de tipos no Visual Basic *tipos de valor* e *tipos de referência*. Estruturas, enumerações e tipos primitivos (exceto as cadeias de caracteres) são tipos de valor. Cadeias de caracteres, classes, interfaces, módulos padrão, matrizes e delegados são tipos de referência.

Cada tipo tem um *valor padrão*, que é o valor atribuído a variáveis desse tipo na inicialização.

```antlr
TypeName
    : ArrayTypeName
    | NonArrayTypeName
    ;

NonArrayTypeName
    : SimpleTypeName
    | NullableTypeName
    ;

SimpleTypeName
    : QualifiedTypeName
    | BuiltInTypeName
    ;

QualifiedTypeName
    : Identifier TypeArguments? (Period IdentifierOrKeyword TypeArguments?)*
    | 'Global' Period IdentifierOrKeyword TypeArguments?
      (Period IdentifierOrKeyword TypeArguments?)*
    ;

TypeArguments
    : OpenParenthesis 'Of' TypeArgumentList CloseParenthesis
    ;

TypeArgumentList
    : TypeName ( Comma TypeName )*
    ;

BuiltInTypeName
    : 'Object'
    | PrimitiveTypeName
    ;

TypeModifier
    : AccessModifier
    | 'Shadows'
    ;

IdentifierModifiers
    : NullableNameModifier? ArrayNameModifier?
    ;
```

## <a name="value-types-and-reference-types"></a>Tipos de valor e referência

Embora os tipos de valor e tipos de referência podem ser semelhantes em termos de uso e a sintaxe de declaração, sua semântica é diferentes.

Tipos de referência são armazenados no heap de tempo de execução; só pode ser acessados por meio de uma referência a esse armazenamento. Como sempre, os tipos de referência são acessados por meio de referências, seu tempo de vida é gerenciado pelo .NET Framework. Referências pendentes de uma determinada instância são acompanhadas e a instância é destruída somente quando não há mais referências permanecem. Uma variável do tipo de referência contém uma referência a um valor desse tipo, um valor de um tipo mais derivado ou um valor nulo. Um *valor nulo* refere-se a nada; não é possível fazer alguma coisa com um valor nulo, exceto pelo fato atribuí-lo. Atribuição para uma variável do tipo de referência cria uma cópia da referência em vez de uma cópia do valor referenciado. Para uma variável do tipo de referência, o valor padrão é um valor nulo.

Tipos de valor são armazenados diretamente na pilha, dentro de uma matriz ou dentro de outro tipo; o armazenamento deles só pode ser acessado diretamente. Como tipos de valor são armazenados diretamente em variáveis, seu tempo de vida é determinado pelo tempo de vida da variável que os contém. Quando o local que contém uma instância de tipo de valor é destruído, a instância do tipo de valor também é destruída. Tipos de valor sempre são acessados diretamente. não é possível criar uma referência a um tipo de valor. Proibir tal referência torna impossível para se referir a uma instância da classe de valor que foi destruída. Como tipos de valor são sempre `NotInheritable`, uma variável de um tipo de valor sempre contém um valor desse tipo. Por isso, o valor de um tipo de valor não pode ser um valor nulo, nem pode referência a um objeto de um tipo mais derivado. Atribuição para uma variável de um tipo de valor cria uma cópia do valor que está sendo atribuído. Para uma variável de um tipo de valor, o valor padrão é o resultado da inicialização de cada membro de variável do tipo para seu valor padrão.

O exemplo a seguir mostra a diferença entre tipos de referência e tipos de valor:

```vb
Class Class1
    Public Value As Integer = 0
End Class

Module Test
    Sub Main()
        Dim val1 As Integer = 0
        Dim val2 As Integer = val1
        val2 = 123
        Dim ref1 As Class1 = New Class1()
        Dim ref2 As Class1 = ref1
        ref2.Value = 123
        Console.WriteLine("Values: " & val1 & ", " & val2)
        Console.WriteLine("Refs: " & ref1.Value & ", " & ref2.Value)
    End Sub
End Module
```

A saída do programa é:

```
Values: 0, 123
Refs: 123, 123
```

A atribuição à variável local `val2` não afeta a variável local `val1` porque ambas as variáveis locais são de um tipo de valor (o tipo `Integer`) e cada variável local de um tipo de valor tem seu próprio armazenamento. Por outro lado, a atribuição `ref2.Value = 123;` afeta o objeto que ambos `ref1` e `ref2` referência.

Uma coisa a observar sobre o sistema de tipos do .NET Framework é que, embora as estruturas, enumerações e tipos primitivos (exceto para `String`) são tipos de valor, todos eles herdam de tipos de referência. Estruturas e os tipos primitivos herdam do tipo de referência `System.ValueType`, que herda de `Object`. Tipos enumerados herdam do tipo de referência `System.Enum`, que herda de `System.ValueType`.

### <a name="nullable-value-types"></a>Tipos de valor que permitem valor nulo

Para tipos de valor, uma `?` modificador pode ser adicionado a um nome de tipo para representar os *anulável* versão desse tipo.

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

Um tipo de valor anulável pode conter os mesmos valores que a versão não anulável do tipo, bem como o valor nulo. Assim, para um tipo de valor anulável, atribuindo `Nothing` a uma variável do tipo define o valor da variável como o valor nulo, não o valor zero de tipo de valor. Por exemplo:

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

Também é possível declarar uma variável para ser de um tipo de valor anulável, colocando um modificador de tipo que permite valor nulo no nome da variável. Para maior clareza, não é válido ter um modificador de tipo que permite valor nulo em um nome de variável e um nome de tipo na mesma declaração. Como tipos anuláveis são implementados usando o tipo `System.Nullable(Of T)`, o tipo `T?` é sinônimo de tipo `System.Nullable(Of T)`, e os dois nomes podem ser usados alternadamente. O `?` modificador não pode ser colocado em um tipo que já é anulável; portanto, não é possível declarar o tipo `Integer??` ou `System.Nullable(Of Integer)?`.

Um tipo de valor anulável `T?` tem os membros de `System.Nullable(Of T)` , bem como operadores ou conversões *eliminada* do tipo subjacente `T` no tipo `T?`. Levantando cópias operadores e conversões de tipo subjacente, na maioria dos casos, substituindo os tipos que permitem valor nulo para tipos de valor não anulável. Isso permite que muitas das conversões e operações que se aplicam ao mesmo `T` para aplicar a `T?` também.


## <a name="interface-implementation"></a>Implementação de interface

Estrutura e declarações de classe podem declarar que eles implementam um conjunto de tipos de interface por meio de um ou mais `Implements` cláusulas.

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

Todos os tipos especificados no `Implements` cláusula deve ser interfaces e o tipo deve implementar todos os membros das interfaces. Por exemplo:

```vb
Interface ICloneable
    Function Clone() As Object
End Interface 

Interface IComparable
    Function CompareTo(other As Object) As Integer
End Interface 

Structure ListEntry
    Implements ICloneable, IComparable

    ...

    Public Function Clone() As Object Implements ICloneable.Clone
        ...
    End Function 

    Public Function CompareTo(other As Object) As Integer _
        Implements IComparable.CompareTo
        ...
    End Function 
End Structure
```

Um tipo que implementa uma interface implicitamente também implementa todas as interfaces de base da interface. Isso é verdadeiro mesmo se o tipo não lista base de todas as interfaces em explicitamente o `Implements` cláusula. Neste exemplo, o `TextBox` estrutura implementa ambos `IControl` e `ITextBox`.

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Structure TextBox
    Implements ITextBox

    ...

    Public Sub Paint() Implements ITextBox.Paint
        ...
    End Sub

    Public Sub SetText(text As String) Implements ITextBox.SetText
        ...
    End Sub
End Structure
```

Declarar que um tipo implementa uma interface por si só não declara que qualquer coisa no espaço de declaração do tipo. Portanto, é válido para implementar duas interfaces com um método com o mesmo nome.

Tipos não é possível implementar um parâmetro de tipo por conta própria, embora ela pode envolver os parâmetros de tipo que estão no escopo.

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

Interfaces genéricas podem ser implementada várias vezes usando argumentos de tipo diferente. No entanto, um tipo genérico não pode implementar uma interface genérica usando um parâmetro de tipo, se o parâmetro de tipo fornecido (independentemente de restrições de tipo) pode sobrepor outra implementação dessa interface. Por exemplo:

```vb
Interface I1(Of T)
End Interface

Class C1
    Implements I1(Of Integer)
    Implements I1(Of Double)    ' OK, no overlap
End Class

Class C2(Of T)
    Implements I1(Of Integer)
    Implements I1(Of T)         ' Error, T could be Integer
End Class
```


## <a name="primitive-types"></a>Tipos primitivos

O *tipos primitivos* são identificadas por meio de palavras-chave, que são aliases para predefinidos tipos no `System` namespace. Um tipo primitivo é completamente indistinguível do tipo ele aliases: escrever a palavra reservada `Byte` é exatamente o mesmo que escrever `System.Byte`. Tipos primitivos são também conhecidos como *tipos intrínsecos*.

```antlr
PrimitiveTypeName
    : NumericTypeName
    | 'Boolean'
    | 'Date'
    | 'Char'
    | 'String'
    ;

NumericTypeName
    : IntegralTypeName
    | FloatingPointTypeName
    | 'Decimal'
    ;

IntegralTypeName
    : 'Byte' | 'SByte' | 'UShort' | 'Short' | 'UInteger'
    | 'Integer' | 'ULong' | 'Long'
    ;

FloatingPointTypeName
    : 'Single' | 'Double'
    ;
```


Porque um primitivo tipo um tipo regular de aliases, todos os tipos primitivos tem membros. Por exemplo, `Integer` tem os membros declarados na `System.Int32`. Literais podem ser tratadas como instâncias de seus tipos correspondentes.

* Os tipos primitivos diferem de outros tipos de estrutura em que eles permitem que determinadas operações adicionais:

* Tipos primitivos permitem valores a ser criado ao escrever literais. Por exemplo, `123I` é um literal do tipo `Integer`.

* É possível declarar constantes dos tipos primitivos.

* Quando os operandos de uma expressão forem todas as constantes de tipo primitivo, é possível que o compilador avaliar a expressão em tempo de compilação. Essa expressão é conhecida como uma expressão constante.

Visual Basic define os seguintes tipos primitivos:

* Os tipos de valor integral `Byte` (inteiro sem sinal de 1 byte), `SByte` (inteiro com sinal de 1 byte), `UShort` (inteiro sem sinal de 2 bytes), `Short` (inteiro com sinal de 2 bytes), `UInteger` (inteiro sem sinal de 4 bytes), `Integer` ( inteiro com sinal de 4 bytes), `ULong` (inteiro sem sinal de 8 bytes), e `Long` (inteiro com sinal de 8 bytes). Esses tipos são mapeados para `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` e `System.Int64`, respectivamente. O valor padrão de um tipo integral é equivalente a literal `0`.

* Os tipos de valor de ponto flutuante `Single` (ponto flutuante de 4 bytes) e `Double` (ponto flutuante de 8 bytes). Esses tipos são mapeados para `System.Single` e `System.Double`, respectivamente. O valor padrão de um tipo de ponto flutuante é equivalente a literal `0`.

* O `Decimal` tipo (valor decimal de 16 bytes), que mapeia para `System.Decimal`. O valor padrão de decimal é equivalente a literal `0D`.

* O `Boolean` tipo, que representa um valor de verdade, normalmente o resultado de uma operação lógica ou relacional de valor. O literal é do tipo `System.Boolean`. O valor padrão de `Boolean` o tipo é equivalente a literal `False`.

* O `Date` valor de tipo, que representa uma data e/ou em uma hora e é mapeado para `System.DateTime`. O valor padrão de `Date` o tipo é equivalente a literal `# 01/01/0001 12:00:00AM #`.

* O `Char` valor de tipo, que representa um único caractere Unicode e é mapeado para `System.Char`. O valor padrão de `Char` o tipo é equivalente à expressão de constante `ChrW(0)`.

* O `String` tipo de referência, que representa uma sequência de caracteres Unicode e é mapeado para `System.String`. O valor padrão de `String` tipo é um valor nulo.



## <a name="enumerations"></a>Enumerações

*Enumerações* são tipos de valor que herdam de `System.Enum` e simbolicamente representam um conjunto de valores de um dos tipos integrais primitivos.

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

Para um tipo de enumeração `E`, o valor padrão é o valor produzido pela expressão `CType(0, E)`.

O tipo subjacente de uma enumeração deve ser um tipo integral que pode representar todos os valores de enumerador definidos na enumeração. Se um tipo subjacente for especificado, ele deve ser `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, ou um de seus tipos correspondentes no `System` namespace. Se nenhum tipo subjacente for especificado explicitamente, o padrão é `Integer`.

O exemplo a seguir declara uma enumeração com um tipo subjacente de `Long`:

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

Um desenvolvedor pode optar por usar um tipo subjacente de `Long`, conforme mostrado no exemplo, para habilitar o uso de valores que estão no intervalo de `Long`, mas não está no intervalo de `Integer`, ou para preservar essa opção para o futuro.


### <a name="enumeration-members"></a>Membros de enumeração

Os membros de uma enumeração são valores enumerados declarados na enumeração e os membros herdados da classe `System.Enum`.

O escopo de um membro de enumeração é o corpo da declaração de enumeração. Isso significa que fora de uma declaração de enumeração, um membro de enumeração sempre deve ser qualificado (a menos que o tipo é especificamente importado para um namespace por meio de uma importação de namespace).

Ordem de declaração para declarações de membro de enumeração é significativo quando valores de expressão de constante são omitidos. Membros de enumeração têm implicitamente `Public` acessar apenas; nenhum modificador de acesso é permitidos em declarações de membro de enumeração.

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a>Valores de enumeração

Os valores enumerados em uma lista de membros de enumeração são declarados como constantes digitado como o tipo subjacente da enumeração, e eles podem aparecer onde quer que as constantes são necessárias. Uma definição de membro de enumeração com `=` fornece o valor indicado pela expressão constante de membro associado. A expressão de constante deve ser avaliada como um tipo integral que é implicitamente conversível no tipo subjacente e deve estar dentro do intervalo de valores que podem ser representados pelo tipo subjacente. O exemplo a seguir está em erro porque os valores de constantes `1.5`, `2.3`, e `3.3` não são implicitamente conversíveis para o tipo integral subjacente `Long` com a semântica estrita.

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

Vários membros de enumeração podem compartilhar o mesmo valor associado, conforme mostrado abaixo:

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

O exemplo mostra uma enumeração que tem dois membros de enumeração – `Blue` e `Max` – que têm o mesmo valor associado.

Se a primeira definição de valor de enumerador na enumeração não tem nenhum inicializador, o valor da constante correspondente será `0`. Uma definição do valor de enumeração sem um inicializador retorna o enumerador o valor obtido ao aumentar o valor do valor de enumeração anterior por `1`. Esse valor deve ser dentro do intervalo de valores que podem ser representados pelo tipo subjacente.

```vb
Enum Color
    Red
    Green = 10
    Blue
End Enum 

Module Test
    Sub Main()
        Console.WriteLine(StringFromColor(Color.Red))
        Console.WriteLine(StringFromColor(Color.Green))
        Console.WriteLine(StringFromColor(Color.Blue))
    End Sub

    Function StringFromColor(c As Color) As String
        Select Case c
            Case Color.Red
                Return String.Format("Red = " & CInt(c))

            Case Color.Green
                Return String.Format("Green = " & CInt(c))

            Case Color.Blue
                Return String.Format("Blue = " & CInt(c))

            Case Else
                Return "Invalid color"
        End Select
    End Function
End Module
```

O exemplo acima imprime os valores de enumeração e seus valores associados. A saída é:

```
Red = 0
Green = 10
Blue = 11
```

Os motivos para os valores são da seguinte maneira:

* O valor de enumeração `Red` recebe automaticamente o valor `0` (porque ele não tem nenhum inicializador e é o primeiro membro do valor de enumeração).

* O valor de enumeração `Green` explicitamente recebe o valor `10`.

* O valor de enumeração `Blue` recebe automaticamente o valor maior do que o valor de enumeração textualmente precedente.

A expressão de constante pode direta ou indiretamente usa o valor de seu próprio valor de enumeração associada (ou seja, circularidade na expressão de constante não é permitida). O exemplo a seguir é inválido porque as declarações das `A` e `B` são circulares.

```vb
Enum Circular
    A = B
    B
End Enum
```

`A` depende `B` explicitamente, e `B` depende `A` implicitamente.

## <a name="classes"></a>Classes

Um *classe* é uma estrutura de dados que pode conter membros de dados (constantes, variáveis e eventos), os membros da função (métodos, propriedades, indexadores, operadores e construtores) e tipos aninhados. As classes são tipos de referência.

```antlr
ClassDeclaration
    : Attributes? ClassModifier* 'Class' Identifier TypeParameterList? StatementTerminator
      ClassBase?
      TypeImplementsClause*
      ClassMemberDeclaration*
      'End' 'Class' StatementTerminator
    ;

ClassModifier
    : TypeModifier
    | 'MustInherit'
    | 'NotInheritable'
    | 'Partial'
    ;
```

O exemplo a seguir mostra uma classe que contém cada tipo de membro:

```vb
Class AClass
    Public Sub New()
        Console.WriteLine("Constructor")
    End Sub

    Public Sub New(value As Integer)
        MyVariable = value
        Console.WriteLine("Constructor")
    End Sub

    Public Const MyConst As Integer = 12
    Public MyVariable As Integer = 34

    Public Sub MyMethod()
        Console.WriteLine("MyClass.MyMethod")
    End Sub

    Public Property MyProperty() As Integer
        Get
            Return MyVariable
        End Get

        Set (value As Integer)
            MyVariable = value
        End Set
    End Property

    Default Public Property Item(index As Integer) As Integer
        Get
            Return 0
        End Get

        Set (value As Integer)
            Console.WriteLine("Item(" & index & ") = " & value)
        End Set
    End Property

    Public Event MyEvent()

    Friend Class MyNestedClass
    End Class 
End Class
```

O exemplo a seguir mostra os usos desses membros:

```vb
Module Test

    ' Event usage.
    Dim WithEvents aInstance As AClass

    Sub Main()
        ' Constructor usage.
        Dim a As AClass = New AClass()
        Dim b As AClass = New AClass(123)

        ' Constant usage.
        Console.WriteLine("MyConst = " & AClass.MyConst)

        ' Variable usage.
        a.MyVariable += 1
        Console.WriteLine("a.MyVariable = " & a.MyVariable)

        ' Method usage.
        a.MyMethod()

        ' Property usage.
        a.MyProperty += 1
        Console.WriteLine("a.MyProperty = " & a.MyProperty)
        a(1) = 1

        ' Event usage.
        aInstance = a
    End Sub 

    Sub MyHandler() Handles aInstance.MyEvent
        Console.WriteLine("Test.MyHandler")
    End Sub 
End Module
```

Há dois modificadores de classe específica `MustInherit` e `NotInheritable`. Não é válido para especificar ambos.


### <a name="class-base-specification"></a>Especificação de classe Base

Uma declaração de classe pode incluir uma especificação de tipo base que define o tipo base direto da classe.

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

Se uma declaração de classe não tem nenhum tipo de base explícito, o tipo de base direto é implicitamente `Object`. Por exemplo:

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

Tipos de base não podem ser um parâmetro de tipo por conta própria, embora ela pode envolver os parâmetros de tipo que estão no escopo.

```vb
Class C1(Of V) 
End Class

Class C2(Of V)
    Inherits V    ' Error, type parameter used as base class 
End Class

Class C3(Of V)
    Inherits C1(Of V)    ' OK: not directly inheriting from V.
End Class
```

Classes só podem derivar `Object` e classes. Não é válido para uma classe derivar de `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` ou `System.Delegate`. Uma classe genérica não pode derivar de `System.Attribute` ou de uma classe que deriva dela.

Cada classe tem exatamente uma classe base direta e circularidade na derivação é proibida. Não é possível derivar de um `NotInheritable` classe e o domínio de acessibilidade da classe base devem ser igual ou um superconjunto do domínio de acessibilidade da própria classe.


### <a name="class-members"></a>Membros de classe

Os membros de uma classe consistem em membros introduzidos por suas declarações de membro de classe e os membros herdados de sua classe base direta.

```antlr
ClassMemberDeclaration
    : NonModuleDeclaration
    | EventMemberDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    | OperatorDeclaration
    ;
```

Uma declaração de membro de classe pode ter `Public`, `Protected`, `Friend`, `Protected Friend`, ou `Private` acesso. Quando uma declaração de membro de classe não inclui um modificador de acesso, a declaração assume como padrão `Public` acesso, a menos que ele seja uma declaração de variável; nesse caso, o padrão é `Private` acesso.

O escopo de um membro de classe é o corpo da classe na qual ocorre a declaração de membro, mais a lista de restrições de classe (se ela é genérica e tem restrições). Se o membro tiver `Friend` acesso, seu escopo se estende ao corpo da classe de qualquer classe derivada no mesmo programa ou qualquer assembly que foi dado `Friend` acesso, e se o membro tiver `Public`, `Protected`, ou `Protected Friend` acessar seu escopo estende o corpo da classe de qualquer classe derivada em qualquer programa.


## <a name="structures"></a>Estruturas

*Estruturas* são tipos de valor que herdam de `System.ValueType`. Estruturas são semelhantes às classes que representam estruturas de dados que podem conter membros de dados e os membros da função. Diferentemente das classes, no entanto, as estruturas não requerem alocação de heap.

```antlr
StructureDeclaration
    : Attributes? StructureModifier* 'Structure' Identifier
      TypeParameterList? StatementTerminator
      TypeImplementsClause*
      StructMemberDeclaration*
      'End' 'Structure' StatementTerminator
    ;

StructureModifier
    : TypeModifier
    | 'Partial'
    ;
```

No caso de classes, é possível que duas variáveis referenciem o mesmo objeto e, portanto, é possível operações em uma variável afetem o objeto referenciado por outra variável. Com estruturas, cada variável tem sua própria cópia do não`Shared` dados, portanto, não é possível que operações em um afetem o outro, como mostra o exemplo a seguir:

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

Dada a declaração acima, o código a seguir gera o valor `10`:

```vb
Module Test
    Sub Main()
        Dim a As New Point(10, 10)
        Dim b As Point = a

        a.x = 100
        Console.WriteLine(b.x)
    End Sub
End Module
```

A atribuição de `a` à `b` cria uma cópia do valor, e `b` , portanto, não é afetado pela atribuição para `a.x`. Tinha `Point` em vez disso, foi declarado como uma classe, a saída seria `100` porque `a` e `b` fará referência ao mesmo objeto.


### <a name="structure-members"></a>Membros de estrutura

Os membros de uma estrutura são os membros introduzidos por suas declarações de membro de estrutura e os membros herdados do `System.ValueType`.

```antlr
StructMemberDeclaration
    : NonModuleDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | EventMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    | OperatorDeclaration
    ;
```

Cada estrutura tem implicitamente um `Public` construtor de instância sem parâmetros que produz o valor padrão da estrutura. Como resultado, não é possível que uma declaração de tipo de estrutura declarar um construtor de instância sem parâmetros. Um tipo de estrutura, no entanto, pode declarar *parametrizada* instância construtores, como no exemplo a seguir:

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

Dada a declaração acima, as instruções a seguir criam uma `Point` com `x` e `y` inicializados em zero.

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

Como estruturas contêm diretamente seus campo valores (em vez de referências a esses valores), estruturas não podem conter campos que direta ou indiretamente fazem referência a mesmos. Por exemplo, o código a seguir não é válido:

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

Normalmente, uma declaração de membro de estrutura pode ter apenas `Public`, `Friend`, ou `Private` acesso, mas quando substituindo membros herdados `Object`, `Protected` e `Protected Friend` acesso também pode ser usado. Quando uma declaração de membro de estrutura não inclui um modificador de acesso, a declaração padrão é `Public` acesso. O escopo de um membro declarado por uma estrutura é o corpo de estrutura na qual ocorre a declaração, além das restrições de estrutura (se ele foi genérico e tinha restrições).


## <a name="standard-modules"></a>Módulos padrão

Um *módulo padrão* é um tipo cujos membros são implicitamente `Shared` e no escopo para o espaço de declaração de namespace que contém de um módulo padrão, em vez de apenas para a declaração de módulo padrão em si. Módulos padrão nunca podem ser instanciados. É um erro para declarar uma variável de um tipo de módulo padrão.

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

Um membro de um módulo padrão tem dois nomes totalmente qualificados, sem o nome do módulo padrão e outra com o nome do módulo padrão. Mais de um módulo padrão em um namespace pode definir um membro com um nome específico; referências não qualificadas para o nome de fora de qualquer módulo são ambíguas. Por exemplo:

```vb
Namespace N1
    Module M1
        Sub S1()
        End Sub

        Sub S2()
        End Sub
    End Module

    Module M2
        Sub S2()
        End Sub
    End Module

    Module M3
        Sub Main()
            S1()       ' Valid: Calls N1.M1.S1.
            N1.S1()    ' Valid: Calls N1.M1.S1.
            S2()       ' Not valid: ambiguous.
            N1.S2()    ' Not valid: ambiguous.
            N1.M2.S2() ' Valid: Calls N1.M2.S2.
        End Sub
    End Module
End Namespace
```

Um módulo só deve ser declarado em um namespace e não pode ser aninhado em outro tipo. Módulos padrão não podem implementar interfaces, eles derivam implicitamente `Object`, e eles têm apenas `Shared` construtores.


### <a name="standard-module-members"></a>Membros de módulo padrão

Os membros de um módulo padrão são os membros introduzidos por suas declarações de membro e os membros herdados de `Object`. Módulos padrão podem ter qualquer tipo de membro, exceto construtores de instância. Todos os membros de tipo de módulo padrão são implicitamente `Shared`.

```antlr
ModuleMemberDeclaration
    : NonModuleDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | EventMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    ;
```

Normalmente, uma declaração de membro de módulo padrão só pode ter `Public`, `Friend`, ou `Private` acesso, mas quando substituindo membros herdados `Object`, o `Protected` e `Protected Friend` modificadores de acesso podem ser especificados. Quando uma declaração de membro de módulo padrão não inclui um modificador de acesso, a declaração assume como padrão `Public` acessar, a menos que ele seja uma variável, cujo padrão é `Private` acesso.

Como mencionado anteriormente, o escopo de um membro de módulo padrão é a declaração que contém a declaração de módulo padrão. Membros herdados de `Object` não estão incluídos nesse escopo especiais; esses membros não têm nenhum escopo e sempre devem ser qualificados com o nome do módulo. Se o membro tiver `Friend` acesso, seu escopo estende-se somente aos membros do namespace declarados no mesmo programa ou assemblies que receberam `Friend` acesso.


## <a name="interfaces"></a>Interfaces

*Interfaces* são implementar que outros tipos para garantir que eles oferecem suporte a determinados métodos de tipos de referência. Uma interface diretamente nunca é criada e não tem nenhuma representação real - outros tipos devem ser convertidos para um tipo de interface. Uma interface define um contrato. Uma classe ou estrutura que implementa uma interface deve cumprir o contrato.

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


O exemplo a seguir mostra uma interface que contém uma propriedade padrão `Item`, um evento `E`, um método `F`e uma propriedade `P`:

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

As interfaces podem empregar a herança múltipla. No exemplo a seguir, a interface `IComboBox` herda de ambos `ITextBox` e `IListBox`:

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Interface IListBox
    Inherits IControl

    Sub SetItems(items() As String)
End Interface 

Interface IComboBox
    Inherits ITextBox, IListBox 
End Interface
```

Classes e estruturas podem implementar várias interfaces. No exemplo a seguir, a classe `EditBox` deriva da classe `Control` e implementa ambos `IControl` e `IDataBound`:

```vb
Interface IDataBound
    Sub Bind(b As Binder)
End Interface 

Public Class EditBox
    Inherits Control
    Implements IControl, IDataBound

    Public Sub Paint() Implements IControl.Paint
        ...
    End Sub

    Public Sub Bind(b As Binder) Implements IDataBound.Bind
        ...
    End Sub
End Class
```


### <a name="interface-inheritance"></a>Herança da interface

As interfaces de base de uma interface são as interfaces base explícitas e suas interfaces base. Em outras palavras, o conjunto de interfaces base é o fechamento transitivo completo as interfaces base explícitas, suas interfaces base explícitas e assim por diante. Se uma declaração de interface não tem nenhuma base de interface explícita, em seguida, não há nenhuma interface base para o tipo--interfaces não herdam `Object` (embora tenham uma conversão de ampliação para `Object`).

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

No exemplo a seguir, as interfaces base da `IComboBox` estão `IControl`, `ITextBox`, e `IListBox`.

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Interface IListBox
    Inherits IControl

    Sub SetItems(items() As String)
End Interface 

Interface IComboBox
    Inherits ITextBox, IListBox 
End Interface
```

Uma interface herda todos os membros de suas interfaces de base. Em outras palavras, o `IComboBox` acima da interface herda membros `SetText` e `SetItems` , bem como `Paint`.

Uma classe ou estrutura que implementa uma interface implicitamente também implementa todas as interfaces de base da interface.

Se uma interface aparecer mais de uma vez em que o fechamento transitivo de interfaces base, ele apenas contribui com seus membros para a interface derivada uma vez. Um tipo que implementa que a interface derivada tem apenas implementar os métodos da multiplicação definido uma vez interface base. No exemplo a seguir `Paint` só precisa ser implementada uma vez, mesmo que a classe implementa `IComboBox` e `IControl`.

```vb
Class ComboBox
    Implements IControl, IComboBox

    Sub SetText(text As String) Implements IComboBox.SetText
    End Sub

    Sub SetItems(items() As String) Implements IComboBox.SetItems
    End Sub

    Sub Print() Implements IComboBox.Paint
    End Sub
End Class
```

Uma `Inherits` cláusula não tem nenhum efeito em outros `Inherits` cláusulas. No exemplo a seguir `IDerived` deve qualificar o nome do `INested` com `IBase`.

```vb
Interface IBase
    Interface INested
        Sub Nested()
    End Interface

    Sub Base()
End Interface

Interface IDerived
    Inherits IBase, INested   ' Error: Must specify IBase.INested.
End Interface
```

O domínio de acessibilidade de uma interface de base deve ser igual ou um superconjunto do domínio de acessibilidade da interface em si.


### <a name="interface-members"></a>Membros de interface

Os membros de uma interface consistem em membros introduzidos por suas declarações de membro e os membros herdados de suas interfaces base.

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

Embora interfaces não herdam os membros da `Object`, porque cada classe ou estrutura que implementa uma interface herdam `Object`, os membros de `Object`, incluindo métodos de extensão, são considerados membros de uma interface e pode ser chamado em uma interface diretamente sem a necessidade de uma conversão em `Object`. Por exemplo:

```vb
Interface I1
End Interface

Class C1
    Implements I1
End Class

Module Test
    Sub Main()
        Dim i As I1 = New C1()
        Dim h As Integer = i.GetHashCode()
    End Sub
End Module
```

Membros de uma interface com o mesmo nome como membros da `Object` implicitamente sombra `Object` membros. Somente aninhados de tipos, métodos, propriedades e eventos podem ser membros de uma interface. Métodos e propriedades não podem ter um corpo. Membros de interface são implicitamente `Public` e não pode especificar um modificador de acesso. O escopo de um membro declarado em uma interface é o corpo de interface na qual ocorre a declaração, mais a lista de restrições de interface (se ela é genérica e tem restrições).


## <a name="arrays"></a>Matrizes

Uma *array* é um tipo de referência que contém variáveis acessadas por meio *índices* correspondente de maneira individual com a ordem das variáveis na matriz. As variáveis contidas em uma matriz, também chamada a *elementos* da matriz, devem ser do mesmo tipo, e esse tipo é chamado de *tipo de elemento* da matriz.

```antlr
ArrayTypeName
    : NonArrayTypeName ArrayTypeModifiers
    ;

ArrayTypeModifiers
    : ArrayTypeModifier+
    ;

ArrayTypeModifier
    : OpenParenthesis RankList? CloseParenthesis
    ;

RankList
    : Comma*
    ;

ArrayNameModifier
    : ArrayTypeModifiers
    | ArraySizeInitializationModifier
    ;
```

Os elementos de uma matriz passam a existir quando uma instância de matriz é criada e deixam de existir quando a instância de matriz é destruída. Cada elemento de uma matriz é inicializado com o valor padrão de seu tipo. O tipo `System.Array` é o tipo base de todos os tipos de matriz e não pode ser instanciado. Cada tipo de matriz herda os membros declarados pela `System.Array` de tipo e pode ser convertido para ele (e `Object`). Um matriz unidimensional do tipo com elemento `T` também implementa as interfaces `System.Collections.Generic.IList(Of T)` e `IReadOnlyList(Of T)`; se `T` é um tipo de referência e, em seguida, o tipo de matriz também implementa `IList(Of U)` e `IReadOnlyList(Of U)` para qualquer `U`que tem uma ampliação fazer referência a conversão de `T`.

Uma matriz tem um *classificação* que determina o número de índices associados com cada elemento da matriz. A classificação de uma matriz determina o número de *dimensões* da matriz. Por exemplo, uma matriz com uma classificação de um é chamada de uma matriz unidimensional e uma matriz com uma classificação maior do que um é chamada de uma matriz multidimensional.

O exemplo a seguir cria uma matriz unidimensional de valores inteiros, inicializa os elementos da matriz e, em seguida, imprime cada um deles:

```vb
Module Test
    Sub Main()
        Dim arr(5) As Integer
        Dim i As Integer

        For i = 0 To arr.Length - 1
            arr(i) = i * i
        Next i

        For i = 0 To arr.Length - 1
            Console.WriteLine("arr(" & i & ") = " & arr(i))
        Next i
    End Sub
End Module
```

O programa produz o seguinte:

```
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

Cada dimensão de uma matriz tem um comprimento associado. Tamanhos da dimensão não fazem parte do tipo de matriz, mas em vez disso, são definidos quando uma instância do tipo de matriz é criada em tempo de execução. O comprimento de uma dimensão determina o intervalo válido de índices para a dimensão: para uma dimensão de comprimento `N`, índices podem variar de zero a `N-1`. Se uma dimensão é de comprimento zero, não há nenhum índices válidos para essa dimensão. O número total de elementos em uma matriz é o produto dos comprimentos de cada dimensão da matriz. Se qualquer uma das dimensões de uma matriz tem um comprimento de zero, a matriz deve estar vazio. O tipo de elemento de uma matriz pode ser qualquer tipo.

Tipos de matriz são especificados com a adição de um modificador para um nome de tipo existente. O modificador consiste de um parêntese esquerdo, um conjunto de zero ou mais vírgulas e um parêntese direito. O tipo modificado é o tipo de elemento da matriz e o número de dimensões é o número de vírgulas, mais um. Se mais de um modificador é especificado, o tipo de elemento da matriz é uma matriz. Os modificadores são lidos da esquerda para a direita, com o modificador mais à esquerda, sendo a matriz mais externa. No exemplo

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

o tipo de elemento da `arr` é uma matriz bidimensional de tridimensionais matrizes de matrizes unidimensionais de `Integer`.

Também é possível declarar uma variável para ser de um tipo de matriz colocando um modificador de tipo de matriz ou um modificador de tamanho da matriz de inicialização no nome da variável. Nesse caso, o tipo de elemento da matriz é o tipo de dado na declaração, e as dimensões de matriz são determinadas pelo modificador de nome de variável. Para maior clareza, não é válido para ter um modificador de tipo de matriz em um nome de variável e um nome de tipo na mesma declaração.

O exemplo a seguir mostra uma variedade de declarações de variável local que usam tipos de matriz com `Integer` como o tipo de elemento:

```vb
Module Test
    Sub Main()
        Dim a1() As Integer    ' Declares 1-dimensional array of integers.
        Dim a2(,) As Integer   ' Declares 2-dimensional array of integers.
        Dim a3(,,) As Integer  ' Declares 3-dimensional array of integers.

        Dim a4 As Integer()    ' Declares 1-dimensional array of integers.
        Dim a5 As Integer(,)   ' Declares 2-dimensional array of integers.
        Dim a6 As Integer(,,)  ' Declares 3-dimensional array of integers.

        ' Declare 1-dimensional array of 2-dimensional arrays of integers 
        Dim a7()(,) As Integer
        ' Declare 2-dimensional array of 1-dimensional arrays of integers.
        Dim a8(,)() As Integer

        Dim a9() As Integer() ' Not allowed.
    End Sub
End Module
```

Um modificador de nome de tipo de matriz estende a todos os conjuntos de parênteses que o seguem. Isso significa que nas situações em que um conjunto de argumentos entre parênteses é permitido após um nome de tipo, não é possível especificar os argumentos para um nome de tipo de matriz. Por exemplo:

```vb
Module Test
    Sub Main()
        ' This calls the Integer constructor.
        Dim x As New Integer(3)

        ' This declares a variable of Integer().
        Dim y As Integer()

        ' This gives an error.
        ' Array sizes can not be specified in a type name.
        Dim z As Integer()(3)
    End Sub
End Module
```

No último caso, `(3)` é interpretado como parte do nome do tipo em vez de um conjunto de argumentos de construtor.


## <a name="delegates"></a>Delegados

Um *delegar* é um tipo de referência que se refere a um `Shared` método de um tipo ou a um método de instância de um objeto.

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 O equivalente mais próximo de um delegado em outros idiomas é um ponteiro de função, mas, enquanto um ponteiro de função só pode referenciar `Shared` funções, um delegado podem fazer referência a ambos `Shared` e métodos de instância. No último caso, o delegado armazena não apenas uma referência ao ponto de entrada do método, mas também uma referência à instância do objeto com o qual invocar o método.

A declaração de delegado pode não ter uma `Handles` cláusula, uma `Implements` cláusula, um corpo de método, ou um `End` construir. A lista de parâmetros da declaração de delegado não pode conter `Optional` ou `ParamArray` parâmetros. O domínio de acessibilidade dos tipos de parâmetro e tipo de retorno deve ser igual ou um superconjunto do domínio de acessibilidade do delegado em si.

Os membros de um delegado são os membros herdados da classe `System.Delegate`. Um delegado também define os métodos a seguir:

* Um construtor que aceita dois parâmetros, uma do tipo `Object` e uma do tipo `System.IntPtr`.

* Um `Invoke` método que tem a mesma assinatura que o delegado.

* Um `BeginInvoke` método cuja assinatura é a assinatura do delegado, com três diferenças. Primeiro, o tipo de retorno é alterado para `System.IAsyncResult`. Em segundo lugar, dois parâmetros adicionais são adicionados ao final da lista de parâmetros: o primeiro do tipo `System.AsyncCallback` e o segundo, do tipo `Object`. E por fim, todos os `ByRef` parâmetros são alterados para ser `ByVal`.

* Um `EndInvoke` cujo tipo de retorno é o mesmo que o delegado do método. Os parâmetros do método são apenas os parâmetros do delegado exatamente que são `ByRef` parâmetros na mesma ordem em que elas ocorrem na assinatura do delegado.  Além desses parâmetros, há um parâmetro adicional do tipo `System.IAsyncResult` no final da lista de parâmetros.

Definir e usar delegados em três etapas: declaração, instanciação e chamada.

Delegados são declarados usando a sintaxe de declaração de delegado. O exemplo a seguir declara um delegado chamado `SimpleDelegate` que não usa argumentos:

```vb
Delegate Sub SimpleDelegate()
```

O exemplo a seguir cria um `SimpleDelegate` da instância e, em seguida, chama imediatamente o ele:

```vb
Module Test
    Sub F()
        System.Console.WriteLine("Test.F")
    End Sub 

    Sub Main()
        Dim d As SimpleDelegate = AddressOf F
        d()
    End Sub 
End Module
```

Não há muito sentido em instanciando um delegado para um método e, em seguida, chamar imediatamente por meio do delegado, como seria mais simples chamar o método diretamente. Delegados mostram sua utilidade quando seu anonimato é usado. A exemplo a seguir mostra uma `MultiCall` método que chama repetidamente um `SimpleDelegate` instância:

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

Não são importante para o `MultiCall` o método que o método de destino para o `SimpleDelegate` é o que acessibilidade esse método tem, ou se o método é `Shared` ou não. Tudo o que importa é que a assinatura do método de destino é compatível com `SimpleDelegate`.


## <a name="partial-types"></a>Tipos parciais

Declarações de classe e estrutura podem ser *parcial* declarações. Uma declaração parcial pode ou não pode descrever completamente o tipo declarado dentro da declaração. Em vez disso, a declaração de tipo pode ser distribuída entre várias declarações parciais dentro do programa; tipos parciais não podem ser declarados entre os limites do programa. Especifica uma declaração de tipo parcial a `Partial` modificador na declaração. Em seguida, quaisquer outras declarações no programa para um tipo com o mesmo nome totalmente qualificado serão mescladas junto com a declaração parcial no tempo de compilação para formar uma única declaração de tipo. Por exemplo, o código a seguir declara uma classe única `Test` com os membros `Test.C1` e `Test.C2`.

a.vb:

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

b.vb:

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

Ao combinar as declarações de tipo parcial, pelo menos uma das declarações deve ter um `Partial` modificador, caso contrário, resulta um erro de tempo de compilação.

__Observação.__ Embora seja possível especificar `Partial` em apenas uma declaração entre várias declarações parciais, é melhor forma especificá-lo em todas as declarações parciais. A situação em que uma declaração parcial está visível, mas um ou mais declarações parciais estão ocultos (como no caso de estender o código gerado por ferramenta), é aceitável para deixar o `Partial` modificador fora da declaração visível especificá-lo, mas no declarações ocultas.

Somente classes e estruturas podem ser declaradas usando declarações parciais. A aridade de um tipo é considerada durante a correspondência de declarações parciais juntos: duas classes com o mesmo nome mas com diferentes números de parâmetros de tipo não são consideradas para ser declarações parciais de simultaneamente. Declarações parciais podem especificar atributos, modificadores, de classe `Inherits` instrução ou `Implements` instrução. Em tempo de compilação, todas as partes das declarações parciais são combinadas em conjunto e usadas como parte da declaração de tipo. Se houver qualquer conflito entre os atributos, modificadores, bases, interfaces, ou membros de tipo, resultados de um erro de tempo de compilação. Por exemplo:

```vb
Public Partial Class Test1
    Implements IDisposable
End Class

Class Test1
    Inherits Object
    Implements IComparable
End Class

Public Partial Class Test2
End Class

Private Partial Class Test2
End Class
```

O exemplo anterior declara um tipo `Test1` que é `Public`, herda `Object` e implementa `System.IDisposable` e `System.IComparable`. As declarações parciais de `Test2` causará um erro de tempo de compilação porque uma das declarações diz que `Test2` é `Public` e outro diz que `Test2` é `Private`.

Tipos parciais com parâmetros de tipo podem declarar restrições e a variação para os parâmetros de tipo, mas as restrições e a variância de cada declaração parcial devem corresponder. Assim, restrições e variância são especiais em que eles não são combinados como outros modificadores automaticamente:

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

O fato de que um tipo é declarado usando várias declarações parciais não afeta as regras de pesquisa de nome dentro do tipo. Como resultado, uma declaração de tipo parcial pode usar membros declarados em outras declarações de tipo parcial, ou pode implementar métodos em interfaces declaradas em outras declarações de tipo parcial. Por exemplo:

```vb
Public Partial Class Test1
    Implements IDisposable

    Private IsDisposed As Boolean = False
End Class

Class Test1
    Private Sub Dispose() Implements IDisposable.Dispose
        If Not IsDisposed Then
            ...
        End If
    End Sub
End Class
```

Tipos aninhados podem ter as declarações parciais. Por exemplo:

```vb
Public Partial Class Test
    Public Partial Class NestedTest
        Public Sub S1()
        End Sub
    End Class
End Class

Public Partial Class Test
    Public Partial Class NestedTest
        Public Sub S2()
        End Sub
    End Class
End Class
```

Inicializadores de dentro de uma declaração parcial ainda serão executados em ordem de declaração. No entanto, não há nenhuma garantia de ordem de execução para inicializadores ocorrerem em declarações parciais separadas.

## <a name="constructed-types"></a>Tipos construídos

Uma declaração de tipo genérico, por si só, não denota um tipo. Em vez disso, uma declaração de tipo genérico pode ser usada como uma "esquema" para muitos tipos diferentes de formulário por meio da aplicação de argumentos de tipo. Um tipo genérico que tem argumentos de tipo aplicados a ele é chamado uma *tipo construído*. Os argumentos de tipo em um tipo construído sempre devem satisfazer as restrições colocadas em parâmetros de tipo, que eles correspondem ao.

Um nome de tipo pode identificar um tipo construído, mesmo que ele não especifica os parâmetros de tipo diretamente. Isso pode ocorrer em que um tipo é aninhado dentro de uma declaração de classe genérica, e o tipo de instância da declaração do recipiente implicitamente é usado para pesquisa de nome:

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

Um tipo construído `C(Of T1,...,Tn)` está acessível quando o tipo genérico e todos os argumentos de tipo são acessíveis. Por exemplo, se o tipo genérico `C` está `Public` e todos os argumentos de tipo `T1,...,Tn` são `Public`, em seguida, o tipo construído é `Public`. Se o nome do tipo ou um dos argumentos de tipo estiver `Private`, no entanto, em seguida, a acessibilidade do tipo construído é `Private`. Se for um argumento de tipo do tipo construído `Protected` e o outro argumento de tipo é `Friend`, em seguida, o tipo construído é acessível somente a classe e suas subclasses neste assembly ou qualquer assembly que tem `Friend` acesso. Em outras palavras, o domínio de acessibilidade para um tipo construído é a interseção dos domínios de acessibilidade de suas partes constituintes.

__Observação.__ O fato de que o domínio de acessibilidade do tipo construído é a interseção de suas partes constituídos tem o efeito colateral interessante da definição de um novo nível de acessibilidade. Um tipo construído que contém um elemento que está `Protected` e um elemento que está `Friend` só pode ser acessado em contextos que podem acessar *ambos* `Friend` *e* `Protected`membros. No entanto, não há nenhuma maneira de expressar esse nível de acessibilidade na linguagem, como a acessibilidade `Protected Friend` significa que uma entidade pode ser acessada em um contexto que possa acessar *qualquer* `Friend` *ou* `Protected` membros.

As interfaces de base, implementados e os membros de tipos construídos são determinados pelo substituindo os argumentos de tipo fornecido para cada ocorrência do parâmetro de tipo no tipo genérico.

### <a name="open-types-and-closed-types"></a>Tipos de aberto e fechado

Um tipo construído para que um ou mais argumentos de tipo são parâmetros de tipo de um tipo recipiente ou do método é chamado um *abrir tipo*. Isso é porque alguns dos parâmetros de tipo do tipo ainda não conhecidos, portanto, a forma real do tipo ainda não é totalmente conhecida. Por outro lado, um tipo genérico, cujos argumentos de tipo são todos os parâmetros de tipo não é chamado um *fechado tipo*. A forma de um tipo fechado sempre totalmente é conhecida. Por exemplo:

```vb
Class Base(Of T, V)
End Class

Class Derived(Of V)
    Inherits Base(Of Integer, V)
End Class

Class MoreDerived
    Inherits Derived(Of Double)
End Class
```

O tipo construído `Base(Of Integer, V)` é um tipo aberto, porque embora o parâmetro de tipo `T` tiver sido fornecido, o parâmetro de tipo `U` foi outro parâmetro de tipo fornecido. Assim, a forma completa do tipo ainda não é conhecida. O tipo construído `Derived(Of Double)`, no entanto, é um tipo fechado porque todos os parâmetros de tipo na hierarquia de herança foram fornecidos.

Tipos abertos são definidos da seguinte maneira:

* Um parâmetro de tipo é um tipo aberto.

* Um tipo de matriz é um tipo aberto, se seu tipo de elemento for um tipo aberto.

* Um tipo construído é um tipo aberto se um ou mais dos seus argumentos de tipo são um tipo aberto.

* Um tipo fechado é um tipo que não é um tipo aberto.

Como o ponto de entrada do programa não pode estar em um tipo genérico, todos os tipos usados em tempo de execução será tipos fechados.

## <a name="special-types"></a>Tipos especiais

O .NET Framework contém um número de classes que são tratados especialmente pelo .NET Framework e a linguagem Visual Basic:

O tipo `System.Void`, que representa um tipo void no .NET Framework, podem ser diretamente acessados apenas em `GetType` expressões.

Os tipos `System.RuntimeArgumentHandle`, `System.ArgIterator` e `System.TypedReference` todos podem conter ponteiros na pilha e portanto não pode aparecer no heap do .NET Framework. Portanto, não podem ser usados como tipos de elemento de matriz, tipos de retorno, tipos de campo, argumentos de tipo genéricos, tipos anuláveis, `ByRef` tipos de parâmetro, o tipo de um valor que está sendo convertido em `Object` ou `System.ValueType`, o destino de uma chamada para a instância os membros `Object` ou `System.ValueType`, ou sobem para um fechamento.
