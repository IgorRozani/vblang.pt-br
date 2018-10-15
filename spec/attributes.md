# <a name="attributes"></a>Atributos

A linguagem Visual Basic permite que o programador deve especificar modificadores em declarações, que representam informações sobre as entidades que está sendo declarado. Por exemplo, associar um método de classe com os modificadores `Public`, `Protected`, `Friend`, `Protected Friend`, ou `Private` especifica sua acessibilidade.

Além dos modificadores definidos pelo idioma, o Visual Basic também permite aos programadores criar novo modificadores, chamados *atributos*e usá-los ao declarar novas entidades. Esses modificadores de novo, que são definidos por meio da declaração de classes de atributos, em seguida, são atribuídos a entidades por meio *blocos de atributo*.

__Observação.__ Atributos podem ser recuperados em tempo de execução por meio de APIs de reflexão do .NET Framework. Essas APIs estão fora do escopo desta especificação.

Por exemplo, uma estrutura pode definir um `Help` atributo que pode ser colocado em elementos do programa, como classes e métodos para fornecer um mapeamento de elementos do programa para a documentação, como demonstra o exemplo a seguir:

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class HelpAttribute
    Inherits Attribute

    Public Sub New(urlValue As String)
        Me.UrlValue = urlValue
    End Sub

    Public Topic As String
    Private UrlValue As String

    Public ReadOnly Property Url() As String
        Get
            Return UrlValue
        End Get
    End Property
End Class
```

O exemplo define uma classe de atributo nomeada `HelpAttribute`, ou `Help` de forma abreviada, que tem um parâmetro posicional (`UrlValue`) e um argumento nomeado (`Topic`).

O exemplo a seguir mostra vários usos do atributo:

```vb
<Help("http://www.example.com/.../Class1.htm")> _
Public Class Class1
    <Help("http://www.example.com/.../Class1.htm", Topic:="F")> _
    Public Sub F()
    End Sub
End Class
```

O exemplo a seguir verifica se `Class1` tem uma `Help` atributo e grava associado `Topic` e `Url` valores se o atributo estiver presente.

```vb
Module Test
    Sub Main()
        Dim type As Type = GetType(Class1)
        Dim arr() As Object = _
            type.GetCustomAttributes(GetType(HelpAttribute), True)

        If arr.Length = 0 Then
            Console.WriteLine("Class1 has no Help attribute.")
        Else
            Dim ha As HelpAttribute = CType(arr(0), HelpAttribute)
            Console.WriteLine("Url = " & ha.Url & ", Topic = " & ha.Topic)
        End If
    End Sub
End Module
```

## <a name="attribute-classes"></a>Classes de atributo

Uma *classe de atributo* é uma classe não genérica que deriva `System.Attribute` e não é `MustInherit`. A classe de atributo pode ter um `System.AttributeUsage` atributo que declara que o atributo é válido, se ele pode ser usado várias vezes em uma declaração e se ela é herdada. O exemplo a seguir define uma classe de atributo nomeada `SimpleAttribute` que podem ser colocados em declarações de classe e declarações de interface:

```vb
<AttributeUsage(AttributeTargets.Class Or AttributeTargets.Interface)> _
Public Class SimpleAttribute
    Inherits System.Attribute
End Class
```

O exemplo a seguir mostra alguns usos do `Simple` atributo. Embora a classe de atributo é nomeada `SimpleAttribute`, usa esse atributo pode omitir o `Attribute` sufixo, reduzindo, portanto, o nome a ser `Simple`:

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

Se o atributo não tem um `System.AttributeUsage`, em seguida, o atributo pode ser colocado em qualquer destino (equivalente a `AttributeTargets.All`). O `System.AttributeUsage` atributo tem um inicializador de variável, `AllowMultiple`, que especifica se o atributo indicado pode ser especificado mais de uma vez para uma determinada declaração. Se `AllowMultiple` de um atributo estiver `True`, é um *classe de atributo de uso de vários*e pode ser especificado mais de uma vez em uma declaração. Se `AllowMultiple` de um atributo estiver `False` ou não for especificado para um atributo, é um *uso classe de atributo único*e podem ser especificadas no máximo uma vez em uma declaração.

O exemplo a seguir define uma classe de atributo de uso de vários chamada `AuthorAttribute`:

```vb
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=True)> _
Public Class AuthorAttribute
    Inherits System.Attribute

    Private _Value As String

    Public Sub New(value As String)
        Me._Value = value
    End Sub

    Public ReadOnly Property Value() As String
        Get
            Return _Value
        End Get
    End Property
End Class
```

O exemplo mostra uma declaração de classe com dois usos do `Author` atributo:

```vb
<Author("Maria Hammond"), Author("Ramesh Meyyappan")> _
Class Class1
End Class
```

O `System.AttributeUsage` atributo tem uma variável de instância pública, `Inherited`, que especifica se o atributo, quando especificado em um tipo base, também é herdado pelos tipos que derivam deste tipo de base. Se o `Inherited` variável de instância pública não for inicializado, um valor padrão de `False` é usado. Propriedades e eventos não herdam atributos, embora fazem os métodos definidos por propriedades e eventos. Interfaces não herdam atributos.

Se um atributo de uso único é herdado e especificado em um tipo derivado, o atributo especificado no tipo derivado substitui o atributo herdado. Se um atributo de uso de vários é herdado e especificado em um tipo derivado, ambos os atributos são especificados no tipo derivado. Por exemplo:

```vb
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=True, _
                Inherited:=True) > _
Class MultiUseAttribute
    Inherits System.Attribute

    Public Sub New(value As Boolean)
    End Sub
End Class

<AttributeUsage(AttributeTargets.Class, Inherited:=True)> _
Class SingleUseAttribute
    Inherits Attribute

    Public Sub New(value As Boolean)
    End Sub
End Class

<SingleUse(True), MultiUse(True)> Class Base
End Class

' Derived has three attributes defined on it: SingleUse(False),
' MultiUse(True) and MultiUse(False)
<SingleUse(False), MultiUse(False)> _
Class Derived
    Inherits Base
End Class
```

Os parâmetros posicionais do atributo são definidos pelos parâmetros dos construtores públicos da classe de atributo. Parâmetros posicionais devem ser `ByVal` e não pode especificar `ByRef`. Propriedades e variáveis de instância pública são definidas por propriedades públicas de leitura / gravação ou variáveis de instância da classe de atributo. Os tipos que podem ser usados em propriedades e variáveis de instância pública e parâmetros posicionais são restritos a tipos de atributo. Um tipo é um tipo de atributo se ele for um dos seguintes:

* Qualquer tipo primitivo, exceto `Date` e `Decimal`.

* O tipo `Object`.

* O tipo `System.Type`.

* Um tipo enumerado, desde que ele e os tipos na qual ele está aninhado (se houver) tenham `Public` acessibilidade.

* Uma matriz unidimensional de um dos tipos anteriores nesta lista.

## <a name="attribute-blocks"></a>Blocos de atributo

Os atributos são especificados em *blocos de atributo*. Cada bloco de atributo é delimitado por colchetes angulares ("<>"), e vários atributos podem ser especificados em uma lista separada por vírgulas dentro de um bloco de atributo ou em vários blocos de atributo. A ordem na qual os atributos são especificados não é significativa. Por exemplo, os blocos de atributo `<A, B>`, `<B, A>`, `<A> <B>` e `<B> <A>` são todas equivalentes.

```antlr
Attributes
    : AttributeBlock+
    ;

AttributeBlock
    : LineTerminator? '<' AttributeList LineTerminator? '>' LineTerminator?
    ;

AttributeList
    : Attribute ( Comma Attribute )*
    ;

Attribute
    : ( AttributeModifier ':' )? SimpleTypeName
    ( OpenParenthesis AttributeArguments? CloseParenthesis )?
    ;

AttributeModifier
    : 'Assembly' | 'Module'
    ;
```

Um atributo não pode ser especificado em um tipo de declaração não tiver suporte e os atributos de uso único não podem ser especificados mais de uma vez em um bloco de atributo. O exemplo a seguir faz com que ambos os erros devido à tentativa de usar `HelpString` na interface `Interface1` e mais de uma vez na declaração de `Class1`.

```vb
<AttributeUsage(AttributeTargets.Class)> _
Public Class HelpStringAttribute
    Inherits System.Attribute

    Private InternalValue As String

    Public Sub New(value As String)
        Me.InternalValue = value
    End Sub

    Public ReadOnly Property Value() As String
        Get
            Return InternalValue
        End Get
    End Property
End Class

' Error: HelpString only applies to classes.
<HelpString("Description of Interface1")> _
Interface Interface1
    Sub Sub1()
End Interface

' Error: HelpString is single-use.
<HelpString("Description of Class1"), _
    HelpString("Another description of Class1")> _
Public Class Class1
End Class
```

Um atributo consiste em um modificador de atributo opcional, um nome de atributo, uma lista opcional de argumentos posicionais e inicializadores de variável ou propriedade. Se não houver nenhum parâmetro ou inicializadores, os parênteses que podem ser omitidos. Se um atributo tem um modificador, ela deve estar em um bloco de atributo na parte superior de um arquivo de origem.

Se um arquivo de origem contiver um bloco de atributo na parte superior do arquivo que especifica atributos para o assembly ou módulo que contém o arquivo de origem, cada atributo no bloco de atributo deve ser precedido por ambos os `Assembly` ou `Module` modificador e uma dois-pontos.


### <a name="attribute-names"></a>Nomes de atributo

O nome de um atributo especifica uma classe de atributo. Por convenção, as classes de atributos são nomeadas com o sufixo `Attribute`. Usos de um atributo podem incluir ou omitir esse sufixo. Consequentemente o nome de uma classe de atributo que corresponde a um identificador de atributo é o identificador em si ou a concatenação do identificador qualificado e `Attribute`. Quando o compilador resolve um nome de atributo, ele acrescenta `Attribute` para o nome e tente a pesquisa. Se essa pesquisa falhar, o compilador tentará a pesquisa sem o sufixo. Por exemplo, usa uma classe de atributo `SimpleAttribute` pode omitir o `Attribute` sufixo, reduzindo, portanto, o nome a ser `Simple`:

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

O exemplo acima é semanticamente equivalente ao seguinte:

```vb
<SimpleAttribute> Class Class1
End Class

<SimpleAttribute> Interface Interface1
End Interface
```

Em geral, os atributos são nomeados com o sufixo `Attribute` são preferenciais. O exemplo a seguir mostra duas classes de atributo nomeadas `T` e `T``Attribute`.

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class T
    Inherits System.Attribute
End Class

<AttributeUsage(AttributeTargets.All)> _
Public Class TAttribute
    Inherits System.Attribute
End Class

' Refers to TAttribute.
<T> Class Class1 
End Class

' Refers to TAttribute.
<TAttribute> Class Class2 
End Class
```

Bloco de atributo `<T>` e o bloco de atributo `<TAttribute>` fazer referência à classe de atributo nomeada `TAttribute`. Não é possível usar `T` como um atributo até que você remova a declaração de classe `TAttribute`.

### <a name="attribute-arguments"></a>Argumentos de atributo

Argumentos para um atributo podem assumir duas formas: *argumentos posicionais* e *inicializadores de variável ou propriedade de instância*. Quaisquer argumentos posicionais para o atributo devem preceder os inicializadores de variável ou propriedade de instância. Um argumento posicional consiste em uma expressão constante, uma expressão de criação de matriz unidimensional ou um `GetType` expressão. Um inicializador de variável ou propriedade de instância consiste em um identificador, que pode corresponder a palavras-chave, seguido por dois pontos e um sinal de igual e terminar com uma expressão constante ou um `GetType` expressão.

Dado um atributo com a classe de atributo `T`, lista de argumentos posicionais `P`e a lista de inicializador de variável ou propriedade de instância `N`, essas etapas determinam se os argumentos são válidos:

1. Siga as etapas de processamento de tempo de compilação para compilar uma expressão do formulário `New T(P)`. Isso resulta em um erro de tempo de compilação ou determina um construtor no `T` que é mais aplicável à lista de argumentos.

2. Se o construtor determinado na etapa 1 tem parâmetros que não são tipos de atributo ou está inacessível no site de declaração, ocorrerá um erro de tempo de compilação.

3. Para cada inicializador de variável ou propriedade de instância `Arg` na `N`, vamos `Name` ser o identificador do inicializador de variável ou propriedade de instância `Arg`. `Name` deve identificar um não -`Shared`gravável, `Public` propriedade sem parâmetros ou variável de instância em `T` cujo tipo é um tipo de atributo. Se `T` tem uma propriedade, ou não existe variável de instância, ocorre um erro de tempo de compilação.

Por exemplo:

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class GeneralAttribute
    Inherits Attribute

    Public Sub New(x As Integer)
    End Sub

    Public Sub New(x As Double)
    End Sub

    Public y As Type

    Public Property z As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

' Calls the first constructor.
<General(10, z:=30, y:=GetType(Integer))> _
Class C1
End Class

' Calls the second constructor.
<General(10.5, z:=10)> _
Class C2
End Class
```

Parâmetros de tipo não podem ser usados em qualquer lugar em argumentos de atributo. No entanto, os tipos construídos podem ser usados:

```vb
<AttributeUsage(AttributeTargets.All)> _
Class A 
   Inherits System.Attribute 

   Public Sub New(t As Type)
   End Sub 
End Class

Class List(Of T) 
    ' Error: attribute argument cannot use type parameter
    <A(GetType(T))> Dim t1 As T 

    ' OK: closed type
    <A(GetType(List(Of Integer)))> Dim y As Integer
End Class
```


```antlr
AttributeArguments
    : AttributePositionalArgumentList
    | AttributePositionalArgumentList Comma VariablePropertyInitializerList
    | VariablePropertyInitializerList
    ;

AttributePositionalArgumentList
    : AttributeArgumentExpression? ( Comma AttributeArgumentExpression? )*
    ;

VariablePropertyInitializerList
    : VariablePropertyInitializer ( Comma VariablePropertyInitializer )*
    ;

VariablePropertyInitializer
    : IdentifierOrKeyword ColonEquals AttributeArgumentExpression
    ;

AttributeArgumentExpression
    : ConstantExpression
    | GetTypeExpression
    | ArrayExpression
    ;
```