# <a name="attributes"></a><span data-ttu-id="7d095-101">Atributos</span><span class="sxs-lookup"><span data-stu-id="7d095-101">Attributes</span></span>

<span data-ttu-id="7d095-102">A linguagem Visual Basic permite que o programador deve especificar modificadores em declarações, que representam informações sobre as entidades que está sendo declarado.</span><span class="sxs-lookup"><span data-stu-id="7d095-102">The Visual Basic language enables the programmer to specify modifiers on declarations, which represent information about the entities being declared.</span></span> <span data-ttu-id="7d095-103">Por exemplo, associar um método de classe com os modificadores `Public`, `Protected`, `Friend`, `Protected Friend`, ou `Private` especifica sua acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="7d095-103">For example, affixing a class method with the modifiers `Public`, `Protected`, `Friend`, `Protected Friend`, or `Private` specifies its accessibility.</span></span>

<span data-ttu-id="7d095-104">Além dos modificadores definidos pelo idioma, o Visual Basic também permite aos programadores criar novo modificadores, chamados *atributos*e usá-los ao declarar novas entidades.</span><span class="sxs-lookup"><span data-stu-id="7d095-104">In addition to the modifiers defined by the language, Visual Basic also enables programmers to create new modifiers, called *attributes*, and to use them when declaring new entities.</span></span> <span data-ttu-id="7d095-105">Esses modificadores de novo, que são definidos por meio da declaração de classes de atributos, em seguida, são atribuídos a entidades por meio *blocos de atributo*.</span><span class="sxs-lookup"><span data-stu-id="7d095-105">These new modifiers, which are defined through the declaration of attribute classes, are then assigned to entities through *attribute blocks*.</span></span>

<span data-ttu-id="7d095-106">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="7d095-106">__Note.__</span></span> <span data-ttu-id="7d095-107">Atributos podem ser recuperados em tempo de execução por meio de APIs de reflexão do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7d095-107">Attributes may be retrieved at run time through the .NET Framework's reflection APIs.</span></span> <span data-ttu-id="7d095-108">Essas APIs estão fora do escopo desta especificação.</span><span class="sxs-lookup"><span data-stu-id="7d095-108">These APIs are outside the scope of this specification.</span></span>

<span data-ttu-id="7d095-109">Por exemplo, uma estrutura pode definir um `Help` atributo que pode ser colocado em elementos do programa, como classes e métodos para fornecer um mapeamento de elementos do programa para a documentação, como demonstra o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="7d095-109">For instance, a framework might define a `Help` attribute that can be placed on program elements such as classes and methods to provide a mapping from program elements to documentation, as the following example demonstrates:</span></span>

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

<span data-ttu-id="7d095-110">O exemplo define uma classe de atributo nomeada `HelpAttribute`, ou `Help` de forma abreviada, que tem um parâmetro posicional (`UrlValue`) e um argumento nomeado (`Topic`).</span><span class="sxs-lookup"><span data-stu-id="7d095-110">The example defines an attribute class named `HelpAttribute`, or `Help` for short, that has one positional parameter (`UrlValue`) and one named argument (`Topic`).</span></span>

<span data-ttu-id="7d095-111">O exemplo a seguir mostra vários usos do atributo:</span><span class="sxs-lookup"><span data-stu-id="7d095-111">The next example shows several uses of the attribute:</span></span>

```vb
<Help("http://www.example.com/.../Class1.htm")> _
Public Class Class1
    <Help("http://www.example.com/.../Class1.htm", Topic:="F")> _
    Public Sub F()
    End Sub
End Class
```

<span data-ttu-id="7d095-112">O exemplo a seguir verifica se `Class1` tem uma `Help` atributo e grava associado `Topic` e `Url` valores se o atributo estiver presente.</span><span class="sxs-lookup"><span data-stu-id="7d095-112">The next example checks to see if `Class1` has a `Help` attribute, and writes out the associated `Topic` and `Url` values if the attribute is present.</span></span>

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

## <a name="attribute-classes"></a><span data-ttu-id="7d095-113">Classes de atributo</span><span class="sxs-lookup"><span data-stu-id="7d095-113">Attribute Classes</span></span>

<span data-ttu-id="7d095-114">Uma *classe de atributo* é uma classe não genérica que deriva `System.Attribute` e não é `MustInherit`.</span><span class="sxs-lookup"><span data-stu-id="7d095-114">An *attribute class* is a non-generic class that derives from `System.Attribute` and is not `MustInherit`.</span></span> <span data-ttu-id="7d095-115">A classe de atributo pode ter um `System.AttributeUsage` atributo que declara que o atributo é válido, se ele pode ser usado várias vezes em uma declaração e se ela é herdada.</span><span class="sxs-lookup"><span data-stu-id="7d095-115">The attribute class may have a `System.AttributeUsage` attribute that declares what the attribute is valid on, whether it may be used multiple times in a declaration, and whether it is inherited.</span></span> <span data-ttu-id="7d095-116">O exemplo a seguir define uma classe de atributo nomeada `SimpleAttribute` que podem ser colocados em declarações de classe e declarações de interface:</span><span class="sxs-lookup"><span data-stu-id="7d095-116">The following example defines an attribute class named `SimpleAttribute` that can be placed on class declarations and interface declarations:</span></span>

```vb
<AttributeUsage(AttributeTargets.Class Or AttributeTargets.Interface)> _
Public Class SimpleAttribute
    Inherits System.Attribute
End Class
```

<span data-ttu-id="7d095-117">O exemplo a seguir mostra alguns usos do `Simple` atributo.</span><span class="sxs-lookup"><span data-stu-id="7d095-117">The next example shows a few uses of the `Simple` attribute.</span></span> <span data-ttu-id="7d095-118">Embora a classe de atributo é nomeada `SimpleAttribute`, usa esse atributo pode omitir o `Attribute` sufixo, reduzindo, portanto, o nome a ser `Simple`:</span><span class="sxs-lookup"><span data-stu-id="7d095-118">Although the attribute class is named `SimpleAttribute`, uses of this attribute may omit the `Attribute` suffix, thus shortening the name to `Simple`:</span></span>

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

<span data-ttu-id="7d095-119">Se o atributo não tem um `System.AttributeUsage`, em seguida, o atributo pode ser colocado em qualquer destino (equivalente a `AttributeTargets.All`).</span><span class="sxs-lookup"><span data-stu-id="7d095-119">If the attribute lacks a `System.AttributeUsage`, then the attribute can be placed on any target (equivalent to `AttributeTargets.All`).</span></span> <span data-ttu-id="7d095-120">O `System.AttributeUsage` atributo tem um inicializador de variável, `AllowMultiple`, que especifica se o atributo indicado pode ser especificado mais de uma vez para uma determinada declaração.</span><span class="sxs-lookup"><span data-stu-id="7d095-120">The `System.AttributeUsage` attribute has a variable initializer, `AllowMultiple`, which specifies whether the indicated attribute can be specified more than once for a given declaration.</span></span> <span data-ttu-id="7d095-121">Se `AllowMultiple` de um atributo estiver `True`, é um *classe de atributo de uso de vários*e pode ser especificado mais de uma vez em uma declaração.</span><span class="sxs-lookup"><span data-stu-id="7d095-121">If `AllowMultiple` for an attribute is `True`, it is a *multiple-use attribute class*, and can be specified more than once on a declaration.</span></span> <span data-ttu-id="7d095-122">Se `AllowMultiple` de um atributo estiver `False` ou não for especificado para um atributo, é um *uso classe de atributo único*e podem ser especificadas no máximo uma vez em uma declaração.</span><span class="sxs-lookup"><span data-stu-id="7d095-122">If `AllowMultiple` for an attribute is `False` or unspecified for an attribute, it is a *single-use attribute class*, and can be specified at most once on a declaration.</span></span>

<span data-ttu-id="7d095-123">O exemplo a seguir define uma classe de atributo de uso de vários chamada `AuthorAttribute`:</span><span class="sxs-lookup"><span data-stu-id="7d095-123">The following example defines a multiple-use attribute class named `AuthorAttribute`:</span></span>

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

<span data-ttu-id="7d095-124">O exemplo mostra uma declaração de classe com dois usos do `Author` atributo:</span><span class="sxs-lookup"><span data-stu-id="7d095-124">The example shows a class declaration with two uses of the `Author` attribute:</span></span>

```vb
<Author("Maria Hammond"), Author("Ramesh Meyyappan")> _
Class Class1
End Class
```

<span data-ttu-id="7d095-125">O `System.AttributeUsage` atributo tem uma variável de instância pública, `Inherited`, que especifica se o atributo, quando especificado em um tipo base, também é herdado pelos tipos que derivam deste tipo de base.</span><span class="sxs-lookup"><span data-stu-id="7d095-125">The `System.AttributeUsage` attribute has a public instance variable, `Inherited`, that specifies whether the attribute, when specified on a base type, is also inherited by types that derive from this base type.</span></span> <span data-ttu-id="7d095-126">Se o `Inherited` variável de instância pública não for inicializado, um valor padrão de `False` é usado.</span><span class="sxs-lookup"><span data-stu-id="7d095-126">If the `Inherited` public instance variable is not initialized, a default value of `False` is used.</span></span> <span data-ttu-id="7d095-127">Propriedades e eventos não herdam atributos, embora fazem os métodos definidos por propriedades e eventos.</span><span class="sxs-lookup"><span data-stu-id="7d095-127">Properties and events do not inherit attributes, although the methods defined by properties and events do.</span></span> <span data-ttu-id="7d095-128">Interfaces não herdam atributos.</span><span class="sxs-lookup"><span data-stu-id="7d095-128">Interfaces do not inherit attributes.</span></span>

<span data-ttu-id="7d095-129">Se um atributo de uso único é herdado e especificado em um tipo derivado, o atributo especificado no tipo derivado substitui o atributo herdado.</span><span class="sxs-lookup"><span data-stu-id="7d095-129">If a single-use attribute is both inherited and specified on a derived type, the attribute specified on the derived type overrides the inherited attribute.</span></span> <span data-ttu-id="7d095-130">Se um atributo de uso de vários é herdado e especificado em um tipo derivado, ambos os atributos são especificados no tipo derivado.</span><span class="sxs-lookup"><span data-stu-id="7d095-130">If a multiple-use attribute is both inherited and specified on a derived type, both attributes are specified on the derived type.</span></span> <span data-ttu-id="7d095-131">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7d095-131">For example:</span></span>

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

<span data-ttu-id="7d095-132">Os parâmetros posicionais do atributo são definidos pelos parâmetros dos construtores públicos da classe de atributo.</span><span class="sxs-lookup"><span data-stu-id="7d095-132">The positional parameters of the attribute are defined by the parameters of the public constructors of the attribute class.</span></span> <span data-ttu-id="7d095-133">Parâmetros posicionais devem ser `ByVal` e não pode especificar `ByRef`.</span><span class="sxs-lookup"><span data-stu-id="7d095-133">Positional parameters must be `ByVal` and may not specify `ByRef`.</span></span> <span data-ttu-id="7d095-134">Propriedades e variáveis de instância pública são definidas por propriedades públicas de leitura / gravação ou variáveis de instância da classe de atributo.</span><span class="sxs-lookup"><span data-stu-id="7d095-134">Public instance variables and properties are defined by public read-write properties or instance variables of the attribute class.</span></span> <span data-ttu-id="7d095-135">Os tipos que podem ser usados em propriedades e variáveis de instância pública e parâmetros posicionais são restritos a tipos de atributo.</span><span class="sxs-lookup"><span data-stu-id="7d095-135">The types that can be used in positional parameters and public instance variables and properties are restricted to attribute types.</span></span> <span data-ttu-id="7d095-136">Um tipo é um tipo de atributo se ele for um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="7d095-136">A type is an attribute type if it is one of the following:</span></span>

* <span data-ttu-id="7d095-137">Qualquer tipo primitivo, exceto `Date` e `Decimal`.</span><span class="sxs-lookup"><span data-stu-id="7d095-137">Any primitive type except for `Date` and `Decimal`.</span></span>

* <span data-ttu-id="7d095-138">O tipo `Object`.</span><span class="sxs-lookup"><span data-stu-id="7d095-138">The type `Object`.</span></span>

* <span data-ttu-id="7d095-139">O tipo `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="7d095-139">The type `System.Type`.</span></span>

* <span data-ttu-id="7d095-140">Um tipo enumerado, desde que ele e os tipos na qual ele está aninhado (se houver) tenham `Public` acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="7d095-140">An enumerated type, provided that it and the types in which it is nested (if any) have `Public` accessibility.</span></span>

* <span data-ttu-id="7d095-141">Uma matriz unidimensional de um dos tipos anteriores nesta lista.</span><span class="sxs-lookup"><span data-stu-id="7d095-141">A one-dimensional array of one of the previous types in this list.</span></span>

## <a name="attribute-blocks"></a><span data-ttu-id="7d095-142">Blocos de atributo</span><span class="sxs-lookup"><span data-stu-id="7d095-142">Attribute Blocks</span></span>

<span data-ttu-id="7d095-143">Os atributos são especificados em *blocos de atributo*.</span><span class="sxs-lookup"><span data-stu-id="7d095-143">Attributes are specified in *attribute blocks*.</span></span> <span data-ttu-id="7d095-144">Cada bloco de atributo é delimitado por colchetes angulares ("<>"), e vários atributos podem ser especificados em uma lista separada por vírgulas dentro de um bloco de atributo ou em vários blocos de atributo.</span><span class="sxs-lookup"><span data-stu-id="7d095-144">Each attribute block is delimited by angle brackets ("<>"), and multiple attributes can be specified in a comma-separated list within an attribute block or in multiple attribute blocks.</span></span> <span data-ttu-id="7d095-145">A ordem na qual os atributos são especificados não é significativa.</span><span class="sxs-lookup"><span data-stu-id="7d095-145">The order in which attributes are specified is not significant.</span></span> <span data-ttu-id="7d095-146">Por exemplo, os blocos de atributo `<A, B>`, `<B, A>`, `<A> <B>` e `<B> <A>` são todas equivalentes.</span><span class="sxs-lookup"><span data-stu-id="7d095-146">For example, the attribute blocks `<A, B>`, `<B, A>`, `<A> <B>` and `<B> <A>` are all equivalent.</span></span>

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

<span data-ttu-id="7d095-147">Um atributo não pode ser especificado em um tipo de declaração não tiver suporte e os atributos de uso único não podem ser especificados mais de uma vez em um bloco de atributo.</span><span class="sxs-lookup"><span data-stu-id="7d095-147">An attribute may not be specified on a kind of declaration it does not support, and single-use attributes may not be specified more than once in an attribute block.</span></span> <span data-ttu-id="7d095-148">O exemplo a seguir faz com que ambos os erros devido à tentativa de usar `HelpString` na interface `Interface1` e mais de uma vez na declaração de `Class1`.</span><span class="sxs-lookup"><span data-stu-id="7d095-148">The example below causes errors both because it attempts to use `HelpString` on the interface `Interface1` and more than once on the declaration of `Class1`.</span></span>

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

<span data-ttu-id="7d095-149">Um atributo consiste em um modificador de atributo opcional, um nome de atributo, uma lista opcional de argumentos posicionais e inicializadores de variável ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="7d095-149">An attribute consists of an optional attribute modifier, an attribute name, an optional list of positional arguments, and variable/property initializers.</span></span> <span data-ttu-id="7d095-150">Se não houver nenhum parâmetro ou inicializadores, os parênteses que podem ser omitidos.</span><span class="sxs-lookup"><span data-stu-id="7d095-150">If there are no parameters or initializers, the parentheses may be omitted.</span></span> <span data-ttu-id="7d095-151">Se um atributo tem um modificador, ela deve estar em um bloco de atributo na parte superior de um arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="7d095-151">If an attribute has a modifier, it must be in an attribute block at the top of a source file.</span></span>

<span data-ttu-id="7d095-152">Se um arquivo de origem contiver um bloco de atributo na parte superior do arquivo que especifica atributos para o assembly ou módulo que contém o arquivo de origem, cada atributo no bloco de atributo deve ser precedido por ambos os `Assembly` ou `Module` modificador e uma dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="7d095-152">If a source file contains an attribute block at the top of the file that specifies attributes for the assembly or module that will contain the source file, each attribute in the attribute block must be prefixed by both the `Assembly` or `Module` modifier and a colon.</span></span>


### <a name="attribute-names"></a><span data-ttu-id="7d095-153">Nomes de atributo</span><span class="sxs-lookup"><span data-stu-id="7d095-153">Attribute Names</span></span>

<span data-ttu-id="7d095-154">O nome de um atributo especifica uma classe de atributo.</span><span class="sxs-lookup"><span data-stu-id="7d095-154">The name of an attribute specifies an attribute class.</span></span> <span data-ttu-id="7d095-155">Por convenção, as classes de atributos são nomeadas com o sufixo `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="7d095-155">By convention, attribute classes are named with the suffix `Attribute`.</span></span> <span data-ttu-id="7d095-156">Usos de um atributo podem incluir ou omitir esse sufixo.</span><span class="sxs-lookup"><span data-stu-id="7d095-156">Uses of an attribute may either include or omit this suffix.</span></span> <span data-ttu-id="7d095-157">Consequentemente o nome de uma classe de atributo que corresponde a um identificador de atributo é o identificador em si ou a concatenação do identificador qualificado e `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="7d095-157">Consequently the name of an attribute class that corresponds to an attribute identifier is either the identifier itself or the concatenation of the qualified identifier and `Attribute`.</span></span> <span data-ttu-id="7d095-158">Quando o compilador resolve um nome de atributo, ele acrescenta `Attribute` para o nome e tente a pesquisa.</span><span class="sxs-lookup"><span data-stu-id="7d095-158">When the compiler resolves an attribute name, it appends `Attribute` to the name and tries the lookup.</span></span> <span data-ttu-id="7d095-159">Se essa pesquisa falhar, o compilador tentará a pesquisa sem o sufixo.</span><span class="sxs-lookup"><span data-stu-id="7d095-159">If that lookup fails, the compiler tries the lookup without the suffix.</span></span> <span data-ttu-id="7d095-160">Por exemplo, usa uma classe de atributo `SimpleAttribute` pode omitir o `Attribute` sufixo, reduzindo, portanto, o nome a ser `Simple`:</span><span class="sxs-lookup"><span data-stu-id="7d095-160">For example, uses of an attribute class `SimpleAttribute` may omit the `Attribute` suffix, thus shortening the name to `Simple`:</span></span>

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

<span data-ttu-id="7d095-161">O exemplo acima é semanticamente equivalente ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="7d095-161">The example above is semantically equivalent to the following:</span></span>

```vb
<SimpleAttribute> Class Class1
End Class

<SimpleAttribute> Interface Interface1
End Interface
```

<span data-ttu-id="7d095-162">Em geral, os atributos são nomeados com o sufixo `Attribute` são preferenciais.</span><span class="sxs-lookup"><span data-stu-id="7d095-162">In general, attributes named with the suffix `Attribute` are preferred.</span></span> <span data-ttu-id="7d095-163">O exemplo a seguir mostra duas classes de atributo nomeadas `T` e `T``Attribute`.</span><span class="sxs-lookup"><span data-stu-id="7d095-163">The following example shows two attribute classes named `T` and `T``Attribute`.</span></span>

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

<span data-ttu-id="7d095-164">Bloco de atributo `<T>` e o bloco de atributo `<TAttribute>` fazer referência à classe de atributo nomeada `TAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7d095-164">Both the attribute block `<T>` and the attribute block `<TAttribute>` refer to the attribute class named `TAttribute`.</span></span> <span data-ttu-id="7d095-165">Não é possível usar `T` como um atributo até que você remova a declaração de classe `TAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7d095-165">It is not possible to use `T` as an attribute until you remove the declaration for class `TAttribute`.</span></span>

### <a name="attribute-arguments"></a><span data-ttu-id="7d095-166">Argumentos de atributo</span><span class="sxs-lookup"><span data-stu-id="7d095-166">Attribute Arguments</span></span>

<span data-ttu-id="7d095-167">Argumentos para um atributo podem assumir duas formas: *argumentos posicionais* e *inicializadores de variável ou propriedade de instância*.</span><span class="sxs-lookup"><span data-stu-id="7d095-167">Arguments to an attribute may take two forms: *positional arguments* and *instance variable/property initializers*.</span></span> <span data-ttu-id="7d095-168">Quaisquer argumentos posicionais para o atributo devem preceder os inicializadores de variável ou propriedade de instância.</span><span class="sxs-lookup"><span data-stu-id="7d095-168">Any positional arguments to the attribute must precede the instance variable/property initializers.</span></span> <span data-ttu-id="7d095-169">Um argumento posicional consiste em uma expressão constante, uma expressão de criação de matriz unidimensional ou um `GetType` expressão.</span><span class="sxs-lookup"><span data-stu-id="7d095-169">A positional argument consists of a constant expression, a one-dimensional array-creation expression or a `GetType` expression.</span></span> <span data-ttu-id="7d095-170">Um inicializador de variável ou propriedade de instância consiste em um identificador, que pode corresponder a palavras-chave, seguido por dois pontos e um sinal de igual e terminar com uma expressão constante ou um `GetType` expressão.</span><span class="sxs-lookup"><span data-stu-id="7d095-170">An instance variable/property initializer consists of an identifier, which can match keywords, followed by a colon and equal sign, and terminated by a constant expression or a `GetType` expression.</span></span>

<span data-ttu-id="7d095-171">Dado um atributo com a classe de atributo `T`, lista de argumentos posicionais `P`e a lista de inicializador de variável ou propriedade de instância `N`, essas etapas determinam se os argumentos são válidos:</span><span class="sxs-lookup"><span data-stu-id="7d095-171">Given an attribute with attribute class `T`, positional argument list `P`, and instance variable/property initializer list `N`, these steps determine whether the arguments are valid:</span></span>

1. <span data-ttu-id="7d095-172">Siga as etapas de processamento de tempo de compilação para compilar uma expressão do formulário `New T(P)`.</span><span class="sxs-lookup"><span data-stu-id="7d095-172">Follow the compile-time processing steps for compiling an expression of the form `New T(P)`.</span></span> <span data-ttu-id="7d095-173">Isso resulta em um erro de tempo de compilação ou determina um construtor no `T` que é mais aplicável à lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="7d095-173">This either results in a compile-time error or determines a constructor on `T` that is most applicable to the argument list.</span></span>

2. <span data-ttu-id="7d095-174">Se o construtor determinado na etapa 1 tem parâmetros que não são tipos de atributo ou está inacessível no site de declaração, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="7d095-174">If the constructor determined in step 1 has parameters that are not attribute types or is inaccessible at the declaration site, a compile-time error occurs.</span></span>

3. <span data-ttu-id="7d095-175">Para cada inicializador de variável ou propriedade de instância `Arg` na `N`, vamos `Name` ser o identificador do inicializador de variável ou propriedade de instância `Arg`.</span><span class="sxs-lookup"><span data-stu-id="7d095-175">For each instance variable/property initializer `Arg` in `N`, let `Name` be the identifier of the instance variable/property initializer `Arg`.</span></span> <span data-ttu-id="7d095-176">`Name` deve identificar um não -`Shared`gravável, `Public` propriedade sem parâmetros ou variável de instância em `T` cujo tipo é um tipo de atributo.</span><span class="sxs-lookup"><span data-stu-id="7d095-176">`Name` must identify a non-`Shared`, writeable, `Public` instance variable or parameterless property on `T` whose type is an attribute type.</span></span> <span data-ttu-id="7d095-177">Se `T` tem uma propriedade, ou não existe variável de instância, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="7d095-177">If `T` has no such instance variable or property, a compile-time error occurs.</span></span>

<span data-ttu-id="7d095-178">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7d095-178">For example:</span></span>

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

<span data-ttu-id="7d095-179">Parâmetros de tipo não podem ser usados em qualquer lugar em argumentos de atributo.</span><span class="sxs-lookup"><span data-stu-id="7d095-179">Type parameters cannot be used anywhere in attribute arguments.</span></span> <span data-ttu-id="7d095-180">No entanto, os tipos construídos podem ser usados:</span><span class="sxs-lookup"><span data-stu-id="7d095-180">However, constructed types may be used:</span></span>

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