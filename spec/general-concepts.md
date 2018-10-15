# <a name="general-concepts"></a><span data-ttu-id="86c67-101">Conceitos gerais</span><span class="sxs-lookup"><span data-stu-id="86c67-101">General Concepts</span></span>

<span data-ttu-id="86c67-102">Este capítulo aborda vários conceitos que são necessários para entender a semântica da linguagem do Microsoft Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="86c67-102">This chapter covers a number of concepts that are required to understand the semantics of the Microsoft Visual Basic language.</span></span> <span data-ttu-id="86c67-103">Muitos dos conceitos devem ser familiares para programadores de Visual Basic ou programadores de C/C++, mas suas definições precisas podem ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="86c67-103">Many of the concepts should be familiar to Visual Basic programmers or C/C++ programmers, but their precise definitions may differ.</span></span>

## <a name="declarations"></a><span data-ttu-id="86c67-104">Declarações</span><span class="sxs-lookup"><span data-stu-id="86c67-104">Declarations</span></span>

<span data-ttu-id="86c67-105">Um programa Visual Basic é composto por entidades nomeadas.</span><span class="sxs-lookup"><span data-stu-id="86c67-105">A Visual Basic program is made up of named entities.</span></span> <span data-ttu-id="86c67-106">Essas entidades são apresentadas por meio *declarações* e representam o "significado" do programa.</span><span class="sxs-lookup"><span data-stu-id="86c67-106">These entities are introduced through *declarations* and represent the "meaning" of the program.</span></span>

<span data-ttu-id="86c67-107">Em um nível superior, *namespaces* são entidades que organizam as outras entidades, como tipos e namespaces aninhados.</span><span class="sxs-lookup"><span data-stu-id="86c67-107">At a top level, *namespaces* are entities that organize other entities, such as nested namespaces and types.</span></span> <span data-ttu-id="86c67-108">*Tipos de* são entidades que descrevem os valores e definem o código executável.</span><span class="sxs-lookup"><span data-stu-id="86c67-108">*Types* are entities that describe values and define executable code.</span></span> <span data-ttu-id="86c67-109">Tipos podem conter tipos aninhados e membros de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-109">Types may contain nested types and type members.</span></span> <span data-ttu-id="86c67-110">*Membros de tipo* são constantes, variáveis, métodos, operadores, propriedades, eventos, valores de enumeração e construtores.</span><span class="sxs-lookup"><span data-stu-id="86c67-110">*Type members* are constants, variables, methods, operators, properties, events, enumeration values, and constructors.</span></span>

<span data-ttu-id="86c67-111">Uma entidade que pode conter outras entidades define uma *espaço de declaração*.</span><span class="sxs-lookup"><span data-stu-id="86c67-111">An entity that can contain other entities defines a *declaration space*.</span></span> <span data-ttu-id="86c67-112">Entidades são apresentadas em um espaço de declaração, por meio de declarações ou herança; o espaço de declaração recipiente é chamado das entidades *contexto de declaração*.</span><span class="sxs-lookup"><span data-stu-id="86c67-112">Entities are introduced into a declaration space either through declarations or inheritance; the containing declaration space is called the entities' *declaration context*.</span></span> <span data-ttu-id="86c67-113">Declarar uma entidade em um espaço de declaração por sua vez define um novo espaço de declaração que pode conter declarações de ainda mais as entidades aninhadas; Portanto, as declarações em um programa formam uma hierarquia de espaços de declaração.</span><span class="sxs-lookup"><span data-stu-id="86c67-113">Declaring an entity in a declaration space in turn defines a new declaration space that can contain further nested entity declarations; thus, the declarations in a program form a hierarchy of declaration spaces.</span></span>

<span data-ttu-id="86c67-114">Exceto no caso de membros de tipo sobrecarregado, não é válido para declarações de introduzir as entidades com nomes idênticos do mesmo tipo no mesmo contexto de declaração.</span><span class="sxs-lookup"><span data-stu-id="86c67-114">Except in the case of overloaded type members, it is invalid for declarations to introduce identically named entities of the same kind into the same declaration context.</span></span> <span data-ttu-id="86c67-115">Além disso, um espaço de declaração nunca pode conter diferentes tipos de entidades com o mesmo nome; Por exemplo, um espaço de declaração pode nunca conter uma variável e um método com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="86c67-115">Additionally, a declaration space may never contain different kinds of entities with the same name; for example, a declaration space may never contain a variable and a method by the same name.</span></span>

<span data-ttu-id="86c67-116">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="86c67-116">__Note.__</span></span> <span data-ttu-id="86c67-117">Talvez seja possível em outras linguagens para criar um espaço de declaração que contém tipos diferentes de entidades com o mesmo nome (por exemplo, se a linguagem diferencia maiusculas de minúsculas e permite que as declarações diferentes com base em maiusculas e minúsculas).</span><span class="sxs-lookup"><span data-stu-id="86c67-117">It may be possible in other languages to create a declaration space that contains different kinds of entities with the same name (for example, if the language is case sensitive and allows different declarations based on casing).</span></span> <span data-ttu-id="86c67-118">Nessa situação, a entidade mais acessível está sendo associado a esse nome; Se mais de um tipo de entidade é mais acessível, em seguida, o nome é ambíguo.</span><span class="sxs-lookup"><span data-stu-id="86c67-118">In that situation, the most accessible entity is considered bound to that name; if more than one type of entity is most accessible then the name is ambiguous.</span></span> <span data-ttu-id="86c67-119">`Public` é mais acessível do que `Protected Friend`, `Protected Friend` é mais acessível que `Protected` ou `Friend`, e `Protected` ou `Friend` é mais acessível que `Private`.</span><span class="sxs-lookup"><span data-stu-id="86c67-119">`Public` is more accessible than `Protected Friend`, `Protected Friend` is more accessible than `Protected` or `Friend`, and `Protected` or `Friend` is more accessible than `Private`.</span></span>

<span data-ttu-id="86c67-120">O espaço de declaração de um namespace é "ilimitado," para contribuir com duas declarações de namespace com o mesmo nome totalmente qualificado para o mesmo espaço de declaração.</span><span class="sxs-lookup"><span data-stu-id="86c67-120">The declaration space of a namespace is "open ended," so two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="86c67-121">No exemplo a seguir, as duas declarações de namespace contribuem para o mesmo espaço de declaração, nesse caso, declarando duas classes com nomes totalmente qualificados `Data.Customer` e `Data.Order:`</span><span class="sxs-lookup"><span data-stu-id="86c67-121">In the example below, the two namespace declarations contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Data.Customer` and `Data.Order:`</span></span>

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

<span data-ttu-id="86c67-122">Porque as duas declarações a contribuir com o mesmo espaço de declaração, ocorrerá um erro de tempo de compilação se cada continha uma declaração de uma classe com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="86c67-122">Because the two declarations contribute to the same declaration space, a compile-time error would occur if each contained a declaration of a class with the same name.</span></span>

### <a name="overloading-and-signatures"></a><span data-ttu-id="86c67-123">Assinaturas e sobrecarga</span><span class="sxs-lookup"><span data-stu-id="86c67-123">Overloading and Signatures</span></span>

<span data-ttu-id="86c67-124">A única maneira de declarar entidades com nomes idênticos do mesmo tipo em um espaço de declaração é por meio *sobrecarga*.</span><span class="sxs-lookup"><span data-stu-id="86c67-124">The only way to declare identically named entities of the same kind in a declaration space is through *overloading*.</span></span> <span data-ttu-id="86c67-125">Somente métodos, operadores, construtores de instância e propriedades podem ser sobrecarregadas.</span><span class="sxs-lookup"><span data-stu-id="86c67-125">Only methods, operators, instance constructors, and properties may be overloaded.</span></span>

<span data-ttu-id="86c67-126">Membros de tipo sobrecarregado devem ter assinaturas exclusivas.</span><span class="sxs-lookup"><span data-stu-id="86c67-126">Overloaded type members must possess unique signatures.</span></span> <span data-ttu-id="86c67-127">A assinatura de um membro de tipo consiste do número de parâmetros de tipo e o número e tipos de parâmetros do membro.</span><span class="sxs-lookup"><span data-stu-id="86c67-127">The signature of a type member consists of the number of type parameters, and the number and types of the member's parameters.</span></span> <span data-ttu-id="86c67-128">Operadores de conversão também incluem o tipo de retorno do operador na assinatura.</span><span class="sxs-lookup"><span data-stu-id="86c67-128">Conversion operators also include the return type of the operator in the signature.</span></span>

<span data-ttu-id="86c67-129">A seguir não fazem parte da assinatura de um membro e, portanto, não pode ser sobrecarregadas:</span><span class="sxs-lookup"><span data-stu-id="86c67-129">The following are not part of a member's signature, and hence cannot be overloaded on:</span></span>

* <span data-ttu-id="86c67-130">Modificadores de um membro de tipo (por exemplo, `Shared` ou `Private`).</span><span class="sxs-lookup"><span data-stu-id="86c67-130">Modifiers to a type member (for example, `Shared` or `Private`).</span></span>

* <span data-ttu-id="86c67-131">Modificadores de um parâmetro (por exemplo, `ByVal` ou `ByRef`).</span><span class="sxs-lookup"><span data-stu-id="86c67-131">Modifiers to a parameter (for example, `ByVal` or `ByRef`).</span></span>

* <span data-ttu-id="86c67-132">Os nomes dos parâmetros.</span><span class="sxs-lookup"><span data-stu-id="86c67-132">The names of the parameters.</span></span>

* <span data-ttu-id="86c67-133">O tipo de retorno de um método ou operador (exceto para os operadores de conversão) ou o tipo de elemento de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="86c67-133">The return type of a method or operator (except for conversion operators) or the element type of a property.</span></span>

* <span data-ttu-id="86c67-134">Restrições em um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-134">Constraints on a type parameter.</span></span>

<span data-ttu-id="86c67-135">O exemplo a seguir mostra um conjunto de declarações de método sobrecarregado, juntamente com suas assinaturas.</span><span class="sxs-lookup"><span data-stu-id="86c67-135">The following example shows a set of overloaded method declarations along with their signatures.</span></span> <span data-ttu-id="86c67-136">Essa declaração não é válida, pois várias das declarações de método têm assinaturas idênticas.</span><span class="sxs-lookup"><span data-stu-id="86c67-136">This declaration would not be valid since several of the method declarations have identical signatures.</span></span>

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

<span data-ttu-id="86c67-137">Ele é válido para definir um tipo genérico que pode conter membros com assinaturas idênticas com base nos argumentos de tipo fornecidos.</span><span class="sxs-lookup"><span data-stu-id="86c67-137">It is valid to define a generic type that may contain members with identical signatures based on the type arguments supplied.</span></span> <span data-ttu-id="86c67-138">As regras de resolução de sobrecarga são usadas para tentar resolver a ambiguidade entre essas sobrecargas, embora possa haver situações em que é impossível para resolver a ambiguidade.</span><span class="sxs-lookup"><span data-stu-id="86c67-138">Overload resolution rules are used to try and disambiguate between such overloads, although there may be situations in which it is impossible to disambiguate.</span></span> <span data-ttu-id="86c67-139">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-139">For example:</span></span>

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

## <a name="scope"></a><span data-ttu-id="86c67-140">Escopo</span><span class="sxs-lookup"><span data-stu-id="86c67-140">Scope</span></span>

<span data-ttu-id="86c67-141">O *escopo* do nome da entidade é o conjunto de todos os espaços de declaração dentro da qual é possível referir-se para o nome sem qualificação.</span><span class="sxs-lookup"><span data-stu-id="86c67-141">The *scope* of an entity's name is the set of all declaration spaces within which it is possible to refer to that name without qualification.</span></span> <span data-ttu-id="86c67-142">Em geral, o escopo do nome da entidade é seu contexto de declaração inteira; No entanto, a declaração de uma entidade pode conter declarações aninhadas de entidades com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="86c67-142">In general, the scope of an entity's name is its entire declaration context; however, an entity's declaration may contain nested declarations of entities with the same name.</span></span> <span data-ttu-id="86c67-143">Nesse caso, a entidade aninhado *sombras*, ou oculta, a entidade externa e o acesso à entidade sombreada só é possível por meio de qualificação.</span><span class="sxs-lookup"><span data-stu-id="86c67-143">In that case, the nested entity *shadows*, or hides, the outer entity, and access to the shadowed entity is only possible through qualification.</span></span>

<span data-ttu-id="86c67-144">Sombreamento através de aninhamento ocorre em namespaces ou tipos aninhados dentro de namespaces, tipos aninhados dentro de outros tipos e os corpos de membros de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-144">Shadowing through nesting occurs in namespaces or types nested within namespaces, in types nested within other types, and in the bodies of type members.</span></span> <span data-ttu-id="86c67-145">Sombreamento através de aninhamento de declarações sempre ocorre implicitamente; nenhuma sintaxe explícita é necessária.</span><span class="sxs-lookup"><span data-stu-id="86c67-145">Shadowing through the nesting of declarations always occurs implicitly; no explicit syntax is required.</span></span>

<span data-ttu-id="86c67-146">No exemplo a seguir, dentro de `F` método, a variável de instância `i` é sombreada pela variável local `i`, mas dentro a `G` método, `i` ainda se refere à variável de instância.</span><span class="sxs-lookup"><span data-stu-id="86c67-146">In the following example, within the `F` method, the instance variable `i` is shadowed by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

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

<span data-ttu-id="86c67-147">Quando um nome em um escopo interno oculta um nome em um escopo externo, sombra de todas as ocorrências sobrecarregadas desse nome.</span><span class="sxs-lookup"><span data-stu-id="86c67-147">When a name in an inner scope hides a name in an outer scope, it shadows all overloaded occurrences of that name.</span></span> <span data-ttu-id="86c67-148">No exemplo a seguir, a chamada `F(1)` invoca o `F` declarado em `Inner` porque todas as ocorrências externas de `F` estão ocultos pela declaração interna.</span><span class="sxs-lookup"><span data-stu-id="86c67-148">In the following example, the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="86c67-149">Pelo mesmo motivo, a chamada `F("Hello")` é um erro.</span><span class="sxs-lookup"><span data-stu-id="86c67-149">For the same reason, the call `F("Hello")` is in error.</span></span>

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

## <a name="inheritance"></a><span data-ttu-id="86c67-150">Herança</span><span class="sxs-lookup"><span data-stu-id="86c67-150">Inheritance</span></span>

<span data-ttu-id="86c67-151">Uma relação de herança é um no qual um tipo (o *derivado* tipo) é derivado de outro (o *base* tipo), de modo que o espaço de declaração do tipo derivado implicitamente contém o acessível membros de tipo diferente de construtor e tipos aninhados de seu tipo base.</span><span class="sxs-lookup"><span data-stu-id="86c67-151">An inheritance relationship is one in which one type (the *derived* type) derives from another (the *base* type), such that the derived type's declaration space implicitly contains the accessible non-constructor type members and nested types of its base type.</span></span> <span data-ttu-id="86c67-152">No exemplo a seguir, a classe `A` é a classe base `B`, e `B` é derivado de `A`.</span><span class="sxs-lookup"><span data-stu-id="86c67-152">In the following example, class `A` is the base class of `B`, and `B` is derived from `A`.</span></span>

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

<span data-ttu-id="86c67-153">Uma vez que `A` faz não explicitamente especificar uma classe base, sua classe base é implicitamente `Object`.</span><span class="sxs-lookup"><span data-stu-id="86c67-153">Since `A` does not explicitly specify a base class, its base class is implicitly `Object`.</span></span>

<span data-ttu-id="86c67-154">A seguir é aspectos importantes da herança:</span><span class="sxs-lookup"><span data-stu-id="86c67-154">The following are important aspects of inheritance:</span></span>

* <span data-ttu-id="86c67-155">A herança é transitiva.</span><span class="sxs-lookup"><span data-stu-id="86c67-155">Inheritance is transitive.</span></span> <span data-ttu-id="86c67-156">Se tipo *C* é derivado do tipo *B*e o tipo *B* é derivado do tipo *um*, tipo *C* herda o tipo de membros declarados no tipo *B* , bem como os membros de tipo declarados no tipo *um*.</span><span class="sxs-lookup"><span data-stu-id="86c67-156">If type *C* is derived from type *B*, and type *B* is derived from type *A*, type *C* inherits the type members declared in type *B* as well as the type members declared in type *A*.</span></span>

* <span data-ttu-id="86c67-157">Um tipo derivado se estende, mas não é possível restringir, seu tipo base.</span><span class="sxs-lookup"><span data-stu-id="86c67-157">A derived type extends, but cannot narrow, its base type.</span></span> <span data-ttu-id="86c67-158">Um tipo derivado pode adicionar novos membros de tipo e ele pode sombrear membros de tipo herdado, mas ele não é possível remover a definição de um membro de tipo herdado.</span><span class="sxs-lookup"><span data-stu-id="86c67-158">A derived type can add new type members, and it can shadow inherited type members, but it cannot remove the definition of an inherited type member.</span></span>

* <span data-ttu-id="86c67-159">Como uma instância de um tipo contém todos os membros de tipo de seu tipo base, existe uma conversão sempre de um tipo derivado para seu tipo base.</span><span class="sxs-lookup"><span data-stu-id="86c67-159">Because an instance of a type contains all of the type members of its base type, a conversion always exists from a derived type to its base type.</span></span>

* <span data-ttu-id="86c67-160">Todos os tipos de devem ter um tipo base, exceto pelo tipo `Object`.</span><span class="sxs-lookup"><span data-stu-id="86c67-160">All types must have a base type, except for the type `Object`.</span></span> <span data-ttu-id="86c67-161">Portanto, `Object` é o tipo base definitivo de todos os tipos e todos os tipos podem ser convertidos para ele.</span><span class="sxs-lookup"><span data-stu-id="86c67-161">Thus, `Object` is the ultimate base type of all types, and all types can be converted to it.</span></span>

* <span data-ttu-id="86c67-162">Circularidade na derivação não é permitida.</span><span class="sxs-lookup"><span data-stu-id="86c67-162">Circularity in derivation is not permitted.</span></span> <span data-ttu-id="86c67-163">Ou seja, quando um tipo `B` deriva de um tipo `A`, é um erro para o tipo `A` derivar direta ou indiretamente do tipo `B`.</span><span class="sxs-lookup"><span data-stu-id="86c67-163">That is, when a type `B` derives from a type `A`, it is an error for type `A` to derive directly or indirectly from type `B`.</span></span>

* <span data-ttu-id="86c67-164">Um tipo pode não direta ou indiretamente derivar de um tipo aninhado dentro dele.</span><span class="sxs-lookup"><span data-stu-id="86c67-164">A type may not directly or indirectly derive from a type nested within it.</span></span>

<span data-ttu-id="86c67-165">O exemplo a seguir produz um erro de tempo de compilação porque as classes circularmente dependem uns aos outros.</span><span class="sxs-lookup"><span data-stu-id="86c67-165">The following example produces a compile-time error because the classes circularly depend on each other.</span></span>

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

<span data-ttu-id="86c67-166">O exemplo a seguir também produz um erro de tempo de compilação porque `B` deriva indiretamente de sua classe aninhada `C` por meio da classe `A`.</span><span class="sxs-lookup"><span data-stu-id="86c67-166">The following example also produces a compile-time error because `B` indirectly derives from its nested class `C` through class `A`.</span></span>

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

<span data-ttu-id="86c67-167">O exemplo a seguir não produz um erro porque classe `A` não é derivado da classe `B`.</span><span class="sxs-lookup"><span data-stu-id="86c67-167">The next example does not produce an error because class `A` does not derive from class `B`.</span></span>

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a><span data-ttu-id="86c67-168">MustInherit e Classes NotInheritable</span><span class="sxs-lookup"><span data-stu-id="86c67-168">MustInherit and NotInheritable Classes</span></span>

<span data-ttu-id="86c67-169">Um `MustInherit` classe é um tipo incompleto que pode agir somente como um tipo base.</span><span class="sxs-lookup"><span data-stu-id="86c67-169">A `MustInherit` class is an incomplete type that can act only as a base type.</span></span> <span data-ttu-id="86c67-170">Um `MustInherit` classe não pode ser instanciada, portanto, é um erro usar o `New` operador em um.</span><span class="sxs-lookup"><span data-stu-id="86c67-170">A `MustInherit` class cannot be instantiated, so it is an error to use the `New` operator on one.</span></span> <span data-ttu-id="86c67-171">Ele é válido para declarar variáveis do `MustInherit` classes; só podem ser atribuídos a essas variáveis `Nothing` ou um valor que é de uma classe derivada do `MustInherit` classe.</span><span class="sxs-lookup"><span data-stu-id="86c67-171">It is valid to declare variables of `MustInherit` classes; such variables can only be assigned `Nothing` or a value that is of a class derived from the `MustInherit` class.</span></span>

<span data-ttu-id="86c67-172">Quando uma classe regular é derivada de uma `MustInherit` classe, a classe regular deve substituir tudo herdadas `MustOverride` membros.</span><span class="sxs-lookup"><span data-stu-id="86c67-172">When a regular class is derived from a `MustInherit` class, the regular class must override all inherited `MustOverride` members.</span></span> <span data-ttu-id="86c67-173">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-173">For example:</span></span>

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

<span data-ttu-id="86c67-174">O `MustInherit` classe `A` apresenta uma `MustOverride` método `F`.</span><span class="sxs-lookup"><span data-stu-id="86c67-174">The `MustInherit` class `A` introduces a `MustOverride` method `F`.</span></span> <span data-ttu-id="86c67-175">Classe `B` apresenta um método adicional `G`, mas não fornece uma implementação de `F`.</span><span class="sxs-lookup"><span data-stu-id="86c67-175">Class `B` introduces an additional method `G`, but does not provide an implementation of `F`.</span></span> <span data-ttu-id="86c67-176">Classe `B` , portanto, também devem ser declaradas `MustInherit`.</span><span class="sxs-lookup"><span data-stu-id="86c67-176">Class `B` must therefore also be declared `MustInherit`.</span></span> <span data-ttu-id="86c67-177">Classe `C` substituições `F` e fornece uma implementação real.</span><span class="sxs-lookup"><span data-stu-id="86c67-177">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="86c67-178">Uma vez que há não pendentes `MustOverride` membros na classe `C`, ele não é necessário para ser `MustInherit`.</span><span class="sxs-lookup"><span data-stu-id="86c67-178">Since there are no outstanding `MustOverride` members in class `C`, it is not required to be `MustInherit`.</span></span>

<span data-ttu-id="86c67-179">Um `NotInheritable` é uma classe da qual outra classe não pode ser derivada.</span><span class="sxs-lookup"><span data-stu-id="86c67-179">A `NotInheritable` class is a class from which another class cannot be derived.</span></span> <span data-ttu-id="86c67-180">`NotInheritable` as classes são usadas principalmente para evitar derivação não intencional.</span><span class="sxs-lookup"><span data-stu-id="86c67-180">`NotInheritable` classes are primarily used to prevent unintended derivation.</span></span>

<span data-ttu-id="86c67-181">Neste exemplo, a classe `B` é um erro porque ele tenta derivar a `NotInheritable` classe `A`.</span><span class="sxs-lookup"><span data-stu-id="86c67-181">In this example, class `B` is in error because it attempts to derive from the `NotInheritable` class `A`.</span></span> <span data-ttu-id="86c67-182">Uma classe não pode ser marcado como ambos `MustInherit` e `NotInheritable`.</span><span class="sxs-lookup"><span data-stu-id="86c67-182">A class cannot be marked both `MustInherit` and `NotInheritable`.</span></span>

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a><span data-ttu-id="86c67-183">Interfaces e herança múltipla</span><span class="sxs-lookup"><span data-stu-id="86c67-183">Interfaces and Multiple Inheritance</span></span>

<span data-ttu-id="86c67-184">Ao contrário de outros tipos que derivam apenas um único tipo base, uma interface pode derivar de várias interfaces base.</span><span class="sxs-lookup"><span data-stu-id="86c67-184">Unlike other types, which only derive from a single base type, an interface may derive from multiple base interfaces.</span></span> <span data-ttu-id="86c67-185">Por isso, uma interface pode herdar um membro de tipo com o mesmo nome de diferentes interfaces base.</span><span class="sxs-lookup"><span data-stu-id="86c67-185">Because of this, an interface can inherit an identically named type member from different base interfaces.</span></span> <span data-ttu-id="86c67-186">Nesse caso, o nome de multiplicar herdada não está disponível na interface derivada e referindo-se a qualquer um desses membros de tipo por meio da interface derivada causa um erro de tempo de compilação, independentemente da sobrecarga ou assinaturas.</span><span class="sxs-lookup"><span data-stu-id="86c67-186">In such a case, the multiply-inherited name is not available in the derived interface, and referring to any of those type members through the derived interface causes a compile-time error, regardless of signatures or overloading.</span></span> <span data-ttu-id="86c67-187">Em vez disso, os membros de tipo conflitantes devem ser referenciados através de um nome de interface base.</span><span class="sxs-lookup"><span data-stu-id="86c67-187">Instead, conflicting type members must be referenced through a base interface name.</span></span>

<span data-ttu-id="86c67-188">No exemplo a seguir, as duas primeiras instruções causam erros de tempo de compilação porque o membro herdado multiplicar `Count` não está disponível na interface `IListCounter`:</span><span class="sxs-lookup"><span data-stu-id="86c67-188">In the following example, the first two statements cause compile-time errors because the multiply-inherited member `Count` is not available in interface `IListCounter`:</span></span>

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

<span data-ttu-id="86c67-189">Conforme ilustrado pelo exemplo, a ambiguidade é resolvida com a conversão `x` para o tipo de interface base adequada.</span><span class="sxs-lookup"><span data-stu-id="86c67-189">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="86c67-190">Tal conversão ter sem custos de tempo de execução; Eles consistem simplesmente exibindo a instância como um tipo menos derivado no tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86c67-190">Such casts have no run-time costs; they merely consist of viewing the instance as a less-derived type at compile time.</span></span>

<span data-ttu-id="86c67-191">Quando um membro de tipo único é herdado usando a mesma interface base por meio de vários caminhos, o membro de tipo é tratado como se apenas foram herdada uma vez.</span><span class="sxs-lookup"><span data-stu-id="86c67-191">When a single type member is inherited from the same base interface through multiple paths, the type member is treated as if it were only inherited once.</span></span> <span data-ttu-id="86c67-192">Em outras palavras, a interface derivada contém apenas uma instância de cada membro de tipo herdado de uma interface de base específica.</span><span class="sxs-lookup"><span data-stu-id="86c67-192">In other words, the derived interface only contains one instance of each type member inherited from a particular base interface.</span></span> <span data-ttu-id="86c67-193">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-193">For example:</span></span>

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

<span data-ttu-id="86c67-194">Se um nome de membro de tipo é sombreado em um caminho por meio da hierarquia de herança, em seguida, o nome é sombreado em todos os caminhos.</span><span class="sxs-lookup"><span data-stu-id="86c67-194">If a type member name is shadowed in one path through the inheritance hierarchy, then the name is shadowed in all paths.</span></span> <span data-ttu-id="86c67-195">No exemplo a seguir, o `IBase.F` membro é sombreado pela `ILeft.F` membro, mas não é sombreada em `IRight`:</span><span class="sxs-lookup"><span data-stu-id="86c67-195">In the following example, the `IBase.F` member is shadowed by the `ILeft.F` member, but is not shadowed in `IRight`:</span></span>

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

<span data-ttu-id="86c67-196">A invocação `d.F(1)` seleciona `ILeft.F`, embora `IBase.F` parece não estar sombreada no caminho de acesso que o orienta ao longo das `IRight`.</span><span class="sxs-lookup"><span data-stu-id="86c67-196">The invocation `d.F(1)` selects `ILeft.F`, even though `IBase.F` appears to not be shadowed in the access path that leads through `IRight`.</span></span> <span data-ttu-id="86c67-197">Porque o caminho de acesso de `IDerived` à `ILeft` para `IBase` sombras `IBase.F`, o membro também é sombreado no caminho de acesso de `IDerived` para `IRight` para `IBase`.</span><span class="sxs-lookup"><span data-stu-id="86c67-197">Because the access path from `IDerived` to `ILeft` to `IBase` shadows `IBase.F`, the member is also shadowed in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

### <a name="shadowing"></a><span data-ttu-id="86c67-198">Sombreamento</span><span class="sxs-lookup"><span data-stu-id="86c67-198">Shadowing</span></span>

<span data-ttu-id="86c67-199">Um tipo derivado cobrirá o nome de um membro de tipo herdado, declarando-o novamente.</span><span class="sxs-lookup"><span data-stu-id="86c67-199">A derived type shadows the name of an inherited type member by re-declaring it.</span></span> <span data-ttu-id="86c67-200">Sombreamento de um nome não remove os membros de tipo herdado com esse nome. Ele apenas tornará todos os membros de tipo herdado com esse nome não está disponível na classe derivada.</span><span class="sxs-lookup"><span data-stu-id="86c67-200">Shadowing a name does not remove the inherited type members with that name; it merely makes all of the inherited type members with that name unavailable in the derived class.</span></span> <span data-ttu-id="86c67-201">A declaração de sombreamento pode ser qualquer tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="86c67-201">The shadowing declaration may be any type of entity.</span></span>

<span data-ttu-id="86c67-202">Entidades que podem ser sobrecarregados podem escolher uma das duas formas de sombreamento.</span><span class="sxs-lookup"><span data-stu-id="86c67-202">Entities than can be overloaded can choose one of two forms of shadowing.</span></span> <span data-ttu-id="86c67-203">*Sombreamento por nome* é especificado usando o `Shadows` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="86c67-203">*Shadowing by name* is specified using the `Shadows` keyword.</span></span> <span data-ttu-id="86c67-204">Uma entidade que sombreia por nome oculta tudo com esse nome na classe base, incluindo todas as sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="86c67-204">An entity that shadows by name hides everything by that name in the base class, including all overloads.</span></span> <span data-ttu-id="86c67-205">*Sombreamento por nome e assinatura* é especificado usando o `Overloads` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="86c67-205">*Shadowing by name and signature* is specified using the `Overloads` keyword.</span></span> <span data-ttu-id="86c67-206">Uma entidade que sombreia por nome e assinatura oculta tudo com esse nome com a mesma assinatura que a entidade.</span><span class="sxs-lookup"><span data-stu-id="86c67-206">An entity that shadows by name and signature hides everything by that name with the same signature as the entity.</span></span> <span data-ttu-id="86c67-207">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-207">For example:</span></span>

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

<span data-ttu-id="86c67-208">Sombreamento de um método com um `ParamArray` argumento por nome e assinatura oculta somente a assinatura individual, não é possíveis todas as assinaturas expandidas.</span><span class="sxs-lookup"><span data-stu-id="86c67-208">Shadowing a method with a `ParamArray` argument by name and signature hides only the individual signature, not all possible expanded signatures.</span></span> <span data-ttu-id="86c67-209">Isso é verdadeiro mesmo se a assinatura do método sombreamento coincide com a assinatura não expandida do método sombreada.</span><span class="sxs-lookup"><span data-stu-id="86c67-209">This is true even if the signature of the shadowing method matches the unexpanded signature of the shadowed method.</span></span> <span data-ttu-id="86c67-210">O exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="86c67-210">The following example:</span></span>

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

<span data-ttu-id="86c67-211">Imprime `Base`, embora `Derived.F` tem a mesma assinatura que o formulário não expandida de `Base.F`.</span><span class="sxs-lookup"><span data-stu-id="86c67-211">prints `Base`, even though `Derived.F` has the same signature as the unexpanded form of `Base.F`.</span></span>

<span data-ttu-id="86c67-212">Por outro lado, um método com um `ParamArray` argumento sombreia apenas métodos com a mesma assinatura, nem todas as assinaturas expandidas possíveis.</span><span class="sxs-lookup"><span data-stu-id="86c67-212">Conversely, a method with a `ParamArray` argument only shadows methods with the same signature, not all possible expanded signatures.</span></span> <span data-ttu-id="86c67-213">O exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="86c67-213">The following example:</span></span>

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

<span data-ttu-id="86c67-214">Imprime `Base`, embora `Derived.F` tem um formato expandido que tem a mesma assinatura que `Base.F`.</span><span class="sxs-lookup"><span data-stu-id="86c67-214">prints `Base`, even though `Derived.F` has an expanded form that has the same signature as `Base.F`.</span></span>

<span data-ttu-id="86c67-215">Um método ou propriedade que não especifica de sombreamento `Shadows` ou `Overloads` pressupõe `Overloads` se o método ou propriedade é declarada `Overrides`, `Shadows` caso contrário.</span><span class="sxs-lookup"><span data-stu-id="86c67-215">A shadowing method or property that does not specify `Shadows` or `Overloads` assumes `Overloads` if the method or property is declared `Overrides`, `Shadows` otherwise.</span></span> <span data-ttu-id="86c67-216">Se um membro de um conjunto de entidades sobrecarregados Especifica o `Shadows` ou `Overloads` palavra-chave, todos eles deverá especificá-lo.</span><span class="sxs-lookup"><span data-stu-id="86c67-216">If one member of a set of overloaded entities specifies the `Shadows` or `Overloads` keyword, they all must specify it.</span></span> <span data-ttu-id="86c67-217">O `Shadows` e `Overloads` palavras-chave não podem ser especificadas ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="86c67-217">The `Shadows` and `Overloads` keywords cannot be specified at the same time.</span></span> <span data-ttu-id="86c67-218">Nem `Shadows` nem `Overloads` pode ser especificada em um módulo padrão; os membros em um módulo padrão implicitamente membros herdados de sombra `Object`.</span><span class="sxs-lookup"><span data-stu-id="86c67-218">Neither `Shadows` nor `Overloads` can be specified in a standard module; members in a standard module implicitly shadow members inherited from `Object`.</span></span>

<span data-ttu-id="86c67-219">Ele é válido para o nome de um membro de tipo que foi multiplicar-herdadas por meio da herança de interface (e que não está disponível, assim,), de sombra, portanto, tornando o nome disponíveis na interface derivada.</span><span class="sxs-lookup"><span data-stu-id="86c67-219">It is valid to shadow the name of a type member that has been multiply-inherited through interface inheritance (and which is thereby unavailable), thus making the name available in the derived interface.</span></span>

<span data-ttu-id="86c67-220">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-220">For example:</span></span>

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

<span data-ttu-id="86c67-221">Porque os métodos são permitidos para métodos herdado de sombra, é possível que uma classe para conter várias `Overridable` métodos com a mesma assinatura.</span><span class="sxs-lookup"><span data-stu-id="86c67-221">Because methods are allowed to shadow inherited methods, it is possible for a class to contain several `Overridable` methods with the same signature.</span></span> <span data-ttu-id="86c67-222">Isso não apresenta um problema de ambiguidade, já que somente o método mais derivado é visível.</span><span class="sxs-lookup"><span data-stu-id="86c67-222">This does not present an ambiguity problem, since only the most-derived method is visible.</span></span> <span data-ttu-id="86c67-223">No exemplo a seguir, o `C` e `D` classes contêm dois `Overridable` métodos com a mesma assinatura:</span><span class="sxs-lookup"><span data-stu-id="86c67-223">In the following example, the `C` and `D` classes contain two `Overridable` methods with the same signature:</span></span>

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

<span data-ttu-id="86c67-224">Há dois `Overridable` métodos aqui: um introduzidos pela classe `A` e um introduzidos pela classe `C`.</span><span class="sxs-lookup"><span data-stu-id="86c67-224">There are two `Overridable` methods here: one introduced by class `A` and the one introduced by class `C`.</span></span> <span data-ttu-id="86c67-225">O método introduzido pela classe `C` oculta o método herdado da classe `A`.</span><span class="sxs-lookup"><span data-stu-id="86c67-225">The method introduced by class `C` hides the method inherited from class `A`.</span></span> <span data-ttu-id="86c67-226">Portanto, o `Overrides` declaração de classe `D` substitui o método introduzido pela classe `C`, e não é possível para a classe `D` substituir o método introduzido pela classe `A`.</span><span class="sxs-lookup"><span data-stu-id="86c67-226">Thus, the `Overrides` declaration in class `D` overrides the method introduced by class `C`, and it is not possible for class `D` to override the method introduced by class `A`.</span></span> <span data-ttu-id="86c67-227">O exemplo produz a saída:</span><span class="sxs-lookup"><span data-stu-id="86c67-227">The example produces the output:</span></span>

```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="86c67-228">É possível invocar o oculto `Overridable` método acessando uma instância da classe `D` por meio de um tipo menos derivado em que o método não está oculto.</span><span class="sxs-lookup"><span data-stu-id="86c67-228">It is possible to invoke the hidden `Overridable` method by accessing an instance of class `D` through a less-derived type in which the method is not hidden.</span></span>

<span data-ttu-id="86c67-229">Não é válido para sombrear um `MustOverride` método, pois na maioria dos casos isso tornaria a classe inutilizável.</span><span class="sxs-lookup"><span data-stu-id="86c67-229">It is not valid to shadow a `MustOverride` method, because in most cases this would make the class unusable.</span></span> <span data-ttu-id="86c67-230">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-230">For example:</span></span>

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

<span data-ttu-id="86c67-231">Nesse caso, a classe `MoreDerived` é necessário para substituir o `MustOverride` método `Base.F`, mas porque a classe `Derived` sombras `Base.F`, isso não é possível.</span><span class="sxs-lookup"><span data-stu-id="86c67-231">In this case, the class `MoreDerived` is required to override the `MustOverride` method `Base.F`, but because the class `Derived` shadows `Base.F`, this is not possible.</span></span> <span data-ttu-id="86c67-232">Não é possível declarar válido descendente de `Derived`.</span><span class="sxs-lookup"><span data-stu-id="86c67-232">There is no way to declare a valid descendent of `Derived`.</span></span>

<span data-ttu-id="86c67-233">Em contraste com o sombreamento de um nome de um escopo externo, sombreamento de um nome acessível de um escopo herdado faz com que um aviso a ser relatado, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="86c67-233">In contrast to shadowing a name from an outer scope, shadowing an accessible name from an inherited scope causes a warning to be reported, as in the following example:</span></span>

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

<span data-ttu-id="86c67-234">A declaração do método `F` na classe `Derived` faz com que um aviso a ser relatado.</span><span class="sxs-lookup"><span data-stu-id="86c67-234">The declaration of method `F` in class `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="86c67-235">Sombrear um nome herdado especificamente não é um erro, já que seria que impeçam a evolução separada das classes base.</span><span class="sxs-lookup"><span data-stu-id="86c67-235">Shadowing an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="86c67-236">Por exemplo, a situação acima pode ter ocorrer porque uma versão posterior da classe `Base` introduzido um método `F` que não estava presente em uma versão anterior da classe.</span><span class="sxs-lookup"><span data-stu-id="86c67-236">For example, the above situation might have come about because a later version of class `Base` introduced a method `F` that was not present in an earlier version of the class.</span></span> <span data-ttu-id="86c67-237">A situação acima foi um erro *qualquer* alteração feita em uma classe base em uma biblioteca de classes separadamente com controle de versão pode causar a classes derivadas para se tornar inválido.</span><span class="sxs-lookup"><span data-stu-id="86c67-237">Had the above situation been an error, *any* change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="86c67-238">O aviso causado sombreando um nome herdado pode ser eliminado por meio do uso do `Shadows` ou `Overloads` modificador:</span><span class="sxs-lookup"><span data-stu-id="86c67-238">The warning caused by shadowing an inherited name can be eliminated through use of the `Shadows` or `Overloads` modifier:</span></span>

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

<span data-ttu-id="86c67-239">O `Shadows` modificador indica a intenção de membro herdado de sombra.</span><span class="sxs-lookup"><span data-stu-id="86c67-239">The `Shadows` modifier indicates the intention to shadow the inherited member.</span></span> <span data-ttu-id="86c67-240">Não é um erro para especificar o `Shadows` ou `Overloads` modificador se não houver nenhum nome de membro de tipo de sombra.</span><span class="sxs-lookup"><span data-stu-id="86c67-240">It is not an error to specify the `Shadows` or `Overloads` modifier if there is no type member name to shadow.</span></span>

<span data-ttu-id="86c67-241">Uma declaração de um novo membro sombreia um membro herdado somente dentro do escopo do novo membro, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="86c67-241">A declaration of a new member shadows an inherited member only within the scope of the new member, as in the following example:</span></span>

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

<span data-ttu-id="86c67-242">No exemplo acima, a declaração do método `F` na classe `Derived` sombreia o método `F` que foi herdado da classe `Base`, mas como o novo método `F` na classe `Derived` tem `Private` acesso, seu escopo não estende a classe `MoreDerived`.</span><span class="sxs-lookup"><span data-stu-id="86c67-242">In the example above, the declaration of method `F` in class `Derived` shadows the method `F` that was inherited from class `Base`, but since the new method `F` in class `Derived` has `Private` access, its scope does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="86c67-243">Portanto, a chamada `F()` na `MoreDerived.G` é válida e invocará `Base.F`.</span><span class="sxs-lookup"><span data-stu-id="86c67-243">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span> <span data-ttu-id="86c67-244">No caso de membros de tipo sobrecarregado, todo o conjunto de membros de tipo sobrecarregado é tratado como se tivessem o acesso mais permissível para os fins de sombreamento.</span><span class="sxs-lookup"><span data-stu-id="86c67-244">In the case of overloaded type members, the entire set of overloaded type members is treated as if they all had the most permissive access for the purposes of shadowing.</span></span>

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

<span data-ttu-id="86c67-245">Neste exemplo, mesmo que a declaração de `F()` na `Derived` é declarado com `Private` acessar sobrecarregado `F(Integer)` é declarado com `Public` acesso.</span><span class="sxs-lookup"><span data-stu-id="86c67-245">In this example, even though the declaration of `F()` in `Derived` is declared with `Private` access, the overloaded `F(Integer)` is declared with `Public` access.</span></span> <span data-ttu-id="86c67-246">Portanto, para fins de sombreamento, o nome `F` na `Derived` é tratado como se fosse `Public`, sombra então ambos os métodos `F` em `Base`.</span><span class="sxs-lookup"><span data-stu-id="86c67-246">Therefore, for the purpose of shadowing, the name `F` in `Derived` is treated as if it was `Public`, so both methods shadow `F` in `Base`.</span></span>

## <a name="implementation"></a><span data-ttu-id="86c67-247">Implementação</span><span class="sxs-lookup"><span data-stu-id="86c67-247">Implementation</span></span>

<span data-ttu-id="86c67-248">Uma *implementação* relação existe quando um tipo declara que ele implementa uma interface e o tipo implementa todos os membros de tipo da interface.</span><span class="sxs-lookup"><span data-stu-id="86c67-248">An *implementation* relationship exists when a type declares that it implements an interface and the type implements all the type members of the interface.</span></span> <span data-ttu-id="86c67-249">Um tipo que implementa uma interface específica é conversível para essa interface.</span><span class="sxs-lookup"><span data-stu-id="86c67-249">A type that implements a particular interface is convertible to that interface.</span></span> <span data-ttu-id="86c67-250">Interfaces não podem ser instanciados, mas ele é válido para declarar variáveis de interfaces; Essas variáveis só podem ser atribuídas um valor que é de uma classe que implementa a interface.</span><span class="sxs-lookup"><span data-stu-id="86c67-250">Interfaces cannot be instantiated, but it is valid to declare variables of interfaces; such variables can only be assigned a value that is of a class that implements the interface.</span></span> <span data-ttu-id="86c67-251">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-251">For example:</span></span>

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

<span data-ttu-id="86c67-252">Um tipo que implementa uma interface com membros de tipo herdado multiplicar ainda deve implementar esses métodos, mesmo que eles não podem ser acessados diretamente da interface derivada que está sendo implementado.</span><span class="sxs-lookup"><span data-stu-id="86c67-252">A type implementing an interface with multiply-inherited type members must still implement those methods, even though they cannot be accessed directly from the derived interface being implemented.</span></span> <span data-ttu-id="86c67-253">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-253">For example:</span></span>

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

<span data-ttu-id="86c67-254">Até mesmo `MustInherit` classes devem fornecer implementações de todos os membros de interfaces implementadas; no entanto, pode adiar a implementação desses métodos, declarando-os como `MustOverride`.</span><span class="sxs-lookup"><span data-stu-id="86c67-254">Even `MustInherit` classes must provide implementations of all the members of implemented interfaces; however, they can defer implementation of these methods by declaring them as `MustOverride`.</span></span> <span data-ttu-id="86c67-255">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-255">For example:</span></span>

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

<span data-ttu-id="86c67-256">Um tipo pode optar por implementar novamente uma interface que implementa seu tipo base.</span><span class="sxs-lookup"><span data-stu-id="86c67-256">A type may choose to re-implement an interface that its base type implements.</span></span> <span data-ttu-id="86c67-257">Para reimplementar a interface, o tipo deve declarar explicitamente que ele implementa a interface.</span><span class="sxs-lookup"><span data-stu-id="86c67-257">To re-implement the interface, the type must explicitly state that it implements the interface.</span></span> <span data-ttu-id="86c67-258">Um tipo que implementa novamente uma interface pode optar por reimplementar apenas algumas, mas não todos, os membros da interface – todos os membros não implementados novamente continuam a usar a implementação do tipo base.</span><span class="sxs-lookup"><span data-stu-id="86c67-258">A type re-implementing an interface may choose to re-implement only some, but not all, of the members of the interface -- any members not re-implemented continue to use the base type's implementation.</span></span> <span data-ttu-id="86c67-259">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-259">For example:</span></span>

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

<span data-ttu-id="86c67-260">Este exemplo imprime:</span><span class="sxs-lookup"><span data-stu-id="86c67-260">This example prints:</span></span>

```
TestDerived.DerivedTest1
TestBase.Test2
```

<span data-ttu-id="86c67-261">Quando um tipo derivado implementa uma interface cujas base interfaces são implementadas por tipos de base do tipo derivado, o tipo derivado pode optar por implementar apenas a membros de tipo da interface que já não são implementados pelos tipos de base.</span><span class="sxs-lookup"><span data-stu-id="86c67-261">When a derived type implements an interface whose base interfaces are implemented by the derived type's base types, the derived type can choose to only implement the interface's type members that are not already implemented by the base types.</span></span> <span data-ttu-id="86c67-262">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-262">For example:</span></span>

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

<span data-ttu-id="86c67-263">Um método de interface também pode ser implementado usando um método substituível em um tipo base.</span><span class="sxs-lookup"><span data-stu-id="86c67-263">An interface method can also be implemented using an overridable method in a base type.</span></span> <span data-ttu-id="86c67-264">Nesse caso, um tipo derivado também pode substituir o método substituível e alterar a implementação da interface.</span><span class="sxs-lookup"><span data-stu-id="86c67-264">In that case, a derived type may also override the overridable method and alter the implementation of the interface.</span></span> <span data-ttu-id="86c67-265">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-265">For example:</span></span>

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

### <a name="implementing-methods"></a><span data-ttu-id="86c67-266">Implementação de métodos</span><span class="sxs-lookup"><span data-stu-id="86c67-266">Implementing Methods</span></span>

<span data-ttu-id="86c67-267">Um tipo *implementa* um membro de tipo de uma interface implementada, fornecendo um método com um `Implements` cláusula.</span><span class="sxs-lookup"><span data-stu-id="86c67-267">A type *implements* a type member of an implemented interface by supplying a method with an `Implements` clause.</span></span> <span data-ttu-id="86c67-268">Os membros de dois tipo devem ter o mesmo número de parâmetros, todos os tipos e os modificadores dos parâmetros devem corresponder, incluindo o valor padrão de parâmetros opcionais, o tipo de retorno deve corresponder ao e todas as restrições em parâmetros do método devem corresponder.</span><span class="sxs-lookup"><span data-stu-id="86c67-268">The two type members must have the same number of parameters, all of the types and modifiers of the parameters must match, including the default value of optional parameters, the return type must match, and all of the constraints on method parameters must match.</span></span> <span data-ttu-id="86c67-269">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-269">For example:</span></span>

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

<span data-ttu-id="86c67-270">Um único método pode implementar qualquer número de membros de tipo de interface se atenderem os critérios acima.</span><span class="sxs-lookup"><span data-stu-id="86c67-270">A single method may implement any number of interface type members if they all meet the above criteria.</span></span> <span data-ttu-id="86c67-271">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-271">For example:</span></span>

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

<span data-ttu-id="86c67-272">Ao implementar um método em uma interface genérica, o método de implementação deve fornecer os argumentos de tipo que correspondem aos parâmetros de tipo da interface.</span><span class="sxs-lookup"><span data-stu-id="86c67-272">When implementing a method in a generic interface, the implementing method must supply the type arguments that correspond to the interface's type parameters.</span></span> <span data-ttu-id="86c67-273">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-273">For example:</span></span>

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

<span data-ttu-id="86c67-274">Observe que é possível que uma interface genérica pode não ser implementável do mesmo conjunto de argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-274">Note that it is possible that a generic interface may not be implementable for some set of type arguments.</span></span>

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

## <a name="polymorphism"></a><span data-ttu-id="86c67-275">Polimorfismo</span><span class="sxs-lookup"><span data-stu-id="86c67-275">Polymorphism</span></span>

<span data-ttu-id="86c67-276">*Polimorfismo* fornece a capacidade de variar a implementação de um método ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="86c67-276">*Polymorphism* provides the ability to vary the implementation of a method or property.</span></span> <span data-ttu-id="86c67-277">Com o polimorfismo, o mesmo método ou propriedade pode executar ações diferentes dependendo do tipo de tempo de execução da instância que invoca a ele.</span><span class="sxs-lookup"><span data-stu-id="86c67-277">With polymorphism, the same method or property can perform different actions depending on the run-time type of the instance that invokes it.</span></span> <span data-ttu-id="86c67-278">São chamadas de métodos ou propriedades que são polimórficas *substituível*.</span><span class="sxs-lookup"><span data-stu-id="86c67-278">Methods or properties that are polymorphic are called *overridable*.</span></span> <span data-ttu-id="86c67-279">Por outro lado, a implementação de um método não pode ser substituído ou propriedade é invariável; a implementação é o mesmo se a propriedade ou método é invocada em uma instância da classe na qual ele é declarado ou uma instância de uma classe derivada.</span><span class="sxs-lookup"><span data-stu-id="86c67-279">By contrast, the implementation of a non-overridable method or property is invariant; the implementation is the same whether the method or property is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="86c67-280">Quando um método não pode ser substituído ou propriedade é invocada, o tipo de tempo de compilação da instância é o fator determinante.</span><span class="sxs-lookup"><span data-stu-id="86c67-280">When a non-overridable method or property is invoked, the compile-time type of the instance is the determining factor.</span></span> <span data-ttu-id="86c67-281">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-281">For example:</span></span>

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

<span data-ttu-id="86c67-282">Também pode ser um método substituível `MustOverride`, que significa que ele fornece nenhum corpo de método e deve ser substituído.</span><span class="sxs-lookup"><span data-stu-id="86c67-282">An overridable method may also be `MustOverride`, which means that it provides no method body and must be overridden.</span></span> <span data-ttu-id="86c67-283">`MustOverride` os métodos são permitidos apenas em `MustInherit` classes.</span><span class="sxs-lookup"><span data-stu-id="86c67-283">`MustOverride` methods are only allowed in `MustInherit` classes.</span></span>

<span data-ttu-id="86c67-284">No exemplo a seguir, a classe `Shape` define o conceito abstrato de um objeto de forma geométrica que pode pintar em si:</span><span class="sxs-lookup"><span data-stu-id="86c67-284">In the following example, the class `Shape` defines the abstract notion of a geometrical shape object that can paint itself:</span></span>

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

<span data-ttu-id="86c67-285">O `Paint` método é `MustOverride` porque não há nenhuma implementação padrão significativos.</span><span class="sxs-lookup"><span data-stu-id="86c67-285">The `Paint` method is `MustOverride` because there is no meaningful default implementation.</span></span> <span data-ttu-id="86c67-286">O `Ellipse` e `Box` classes são concretas `Shape` implementações.</span><span class="sxs-lookup"><span data-stu-id="86c67-286">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="86c67-287">Como essas classes não são `MustInherit`, eles são necessários para substituir o `Paint` método e fornecer uma implementação real.</span><span class="sxs-lookup"><span data-stu-id="86c67-287">Because these classes are not `MustInherit`, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="86c67-288">É um erro para um acesso de base fazer referência a um `MustOverride` método, como o exemplo a seguir demonstra:</span><span class="sxs-lookup"><span data-stu-id="86c67-288">It is an error for a base access to reference a `MustOverride` method, as the following example demonstrates:</span></span>

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

<span data-ttu-id="86c67-289">Um erro é relatado para o `MyBase.F()` invocação porque faz referência a um `MustOverride` método.</span><span class="sxs-lookup"><span data-stu-id="86c67-289">An error is reported for the `MyBase.F()` invocation because it references a `MustOverride` method.</span></span>

### <a name="overriding-methods"></a><span data-ttu-id="86c67-290">Substituindo métodos</span><span class="sxs-lookup"><span data-stu-id="86c67-290">Overriding Methods</span></span>

<span data-ttu-id="86c67-291">Um tipo talvez *substituir* um método substituível herdado, declarando um método com o mesmo nome e, assinatura e marcando a declaração com o `Overrides` modificador. Há requisitos adicionais sobre substituição de métodos, listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="86c67-291">A type may *override* an inherited overridable method by declaring a method with the same name and , signature, and marking the declaration with the `Overrides` modifier.There are additional requirements on overriding methods, listed below.</span></span> <span data-ttu-id="86c67-292">Enquanto um `Overridable` declaração de método apresenta um novo método, um `Overrides` declaração de método substitui a implementação herdada do método.</span><span class="sxs-lookup"><span data-stu-id="86c67-292">Whereas an `Overridable` method declaration introduces a new method, an `Overrides` method declaration replaces the inherited implementation of the method.</span></span>

<span data-ttu-id="86c67-293">Um método de substituição pode ser declarado `NotOverridable`, que impede que qualquer substituição ainda mais o método de tipos derivados.</span><span class="sxs-lookup"><span data-stu-id="86c67-293">An overriding method may be declared `NotOverridable`, which prevents any further overriding of the method in derived types.</span></span> <span data-ttu-id="86c67-294">Na verdade, `NotOverridable` classes derivadas de métodos de se tornar não-substituíveis em qualquer outra.</span><span class="sxs-lookup"><span data-stu-id="86c67-294">In effect, `NotOverridable` methods become non-overridable in any further derived classes.</span></span>

<span data-ttu-id="86c67-295">Considere o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="86c67-295">Consider the following example:</span></span>

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

<span data-ttu-id="86c67-296">No exemplo, a classe `B` fornece dois `Overrides` métodos: um método `F` que tem o `NotOverridable` modificador e um método `G` que não faz.</span><span class="sxs-lookup"><span data-stu-id="86c67-296">In the example, class `B` provides two `Overrides` methods: a method `F` that has the `NotOverridable` modifier and a method `G` that does not.</span></span> <span data-ttu-id="86c67-297">Usar o `NotOverridable` modificador impede que a classe `C` de substituindo o método `F`.</span><span class="sxs-lookup"><span data-stu-id="86c67-297">Use of the `NotOverridable` modifier prevents class `C` from further overriding method `F`.</span></span>

<span data-ttu-id="86c67-298">Um método de substituição também pode ser declarado `MustOverride`, mesmo que não está declarado como o método que está substituindo `MustOverride`.</span><span class="sxs-lookup"><span data-stu-id="86c67-298">An overriding method may also be declared `MustOverride`, even if the method that it is overriding is not declared `MustOverride`.</span></span> <span data-ttu-id="86c67-299">Isso requer que a classe continente sejam declaradas `MustInherit` e que qualquer ainda mais as classes derivadas que não são declaradas `MustInherit` deve substituir o método.</span><span class="sxs-lookup"><span data-stu-id="86c67-299">This requires that the containing class be declared `MustInherit` and that any further derived classes that are not declared `MustInherit` must override the method.</span></span> <span data-ttu-id="86c67-300">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-300">For example:</span></span>

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

<span data-ttu-id="86c67-301">No exemplo, a classe `B` substituições `A.F` com um `MustOverride` método.</span><span class="sxs-lookup"><span data-stu-id="86c67-301">In the example, class `B` overrides `A.F` with a `MustOverride` method.</span></span> <span data-ttu-id="86c67-302">Isso significa que todas as classes derivadas de `B` terão de substituir `F`, a menos que elas são declaradas `MustInherit` também.</span><span class="sxs-lookup"><span data-stu-id="86c67-302">This means that any classes derived from `B` will have to override `F`, unless they are declared `MustInherit` as well.</span></span>

<span data-ttu-id="86c67-303">Um erro de tempo de compilação ocorre, a menos que todos os itens a seguir são verdadeiras para um método de substituição:</span><span class="sxs-lookup"><span data-stu-id="86c67-303">A compile-time error occurs unless all of the following are true of an overriding method:</span></span>

* <span data-ttu-id="86c67-304">O contexto da declaração contém um único método herdado acessível com a mesma assinatura e o tipo de retorno (se houver) como o método de substituição.</span><span class="sxs-lookup"><span data-stu-id="86c67-304">The declaration context contains a single accessible inherited method with the same signature and return type (if any) as the overriding method.</span></span>
* <span data-ttu-id="86c67-305">O método herdado que está sendo substituído é substituível.</span><span class="sxs-lookup"><span data-stu-id="86c67-305">The inherited method being overridden is overridable.</span></span> <span data-ttu-id="86c67-306">Em outras palavras, o método herdado que está sendo substituído não é `Shared` ou `NotOverridable`.</span><span class="sxs-lookup"><span data-stu-id="86c67-306">In other words, the inherited method being overridden is not `Shared` or `NotOverridable`.</span></span>
* <span data-ttu-id="86c67-307">O domínio de acessibilidade do método que está sendo declarado é o mesmo que o domínio de acessibilidade do método herdado que está sendo substituído.</span><span class="sxs-lookup"><span data-stu-id="86c67-307">The accessibility domain of the method being declared is the same as the accessibility domain of the inherited method being overridden.</span></span> <span data-ttu-id="86c67-308">Há uma exceção: um `Protected Friend` método deve ser substituído por um `Protected` se o outro método estiver em outro assembly que não tem o método de substituição de método `Friend` acesso.</span><span class="sxs-lookup"><span data-stu-id="86c67-308">There is one exception: a `Protected Friend` method must be overridden by a `Protected` method if the other method is in another assembly that the overriding method does not have `Friend` access to.</span></span>
* <span data-ttu-id="86c67-309">Os parâmetros do método de substituição correspondem aos parâmetros do método substituído no que diz respeito ao uso do `ByVal`, `ByRef`, `ParamArray,` e `Optional` modificadores, incluindo os valores fornecidos para parâmetros opcionais.</span><span class="sxs-lookup"><span data-stu-id="86c67-309">The parameters of the overriding method match the overridden method's parameters in regards to usage of the `ByVal`, `ByRef`, `ParamArray,` and `Optional` modifiers, including the values provided for optional parameters.</span></span>
* <span data-ttu-id="86c67-310">Os parâmetros de tipo de método de substituição correspondem os parâmetros de tipo do método substituído no que diz respeito a restrições de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-310">The type parameters of the overriding method match the overridden method's type parameters in regards to type constraints.</span></span>

<span data-ttu-id="86c67-311">Ao substituir um método em um tipo genérico de base, o método de substituição deve fornecer os argumentos de tipo que correspondem aos parâmetros de tipo base.</span><span class="sxs-lookup"><span data-stu-id="86c67-311">When overriding a method in a base generic type, the overriding method must supply the type arguments that correspond to the base type parameters.</span></span> <span data-ttu-id="86c67-312">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-312">For example:</span></span>

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

<span data-ttu-id="86c67-313">Observe que é possível que um método substituível em uma classe genérica não poderá ser substituído para alguns conjuntos de argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-313">Note that it is possible that an overridable method in a generic class may not be able to be overridden for some sets of type arguments.</span></span> <span data-ttu-id="86c67-314">Se o método for declarado `MustOverride`, isso significa que algumas cadeias de herança podem não ser possíveis.</span><span class="sxs-lookup"><span data-stu-id="86c67-314">If the method is declared `MustOverride`, this means that some inheritance chains may not be possible.</span></span> <span data-ttu-id="86c67-315">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-315">For example:</span></span>

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

<span data-ttu-id="86c67-316">Uma declaração de substituição pode acessar o método base substituído usando um acesso de base, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="86c67-316">An override declaration can access the overridden base method using a base access, as in the following example:</span></span>

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

No exemplo, a invocação de `MyBase.PrintVariables()` na classe `Derived` invoca o `PrintVariables` método declarado na classe `Base`. Um acesso de base desabilita o mecanismo de invocação substituíveis e simplesmente trata o método base como um método não pode ser substituído. <span data-ttu-id="86c67-319">Teve a invocação no `Derived` foi gravado `CType(Me, Base).PrintVariables()`, seria recursivamente invocar o `PrintVariables` método declarado na `Derived`, não aquela declarada em `Base`.</span><span class="sxs-lookup"><span data-stu-id="86c67-319">Had the invocation in `Derived` been written `CType(Me, Base).PrintVariables()`, it would recursively invoke the `PrintVariables` method declared in `Derived`, not the one declared in `Base`.</span></span>

<span data-ttu-id="86c67-320">Somente quando ele inclui um `Overrides` can modificador um método substituem outro método.</span><span class="sxs-lookup"><span data-stu-id="86c67-320">Only when it includes an `Overrides` modifier can a method override another method.</span></span> <span data-ttu-id="86c67-321">Em todos os outros casos, um método com a mesma assinatura que um método herdado sombreia simplesmente o método herdado, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="86c67-321">In all other cases, a method with the same signature as an inherited method simply shadows the inherited method, as in the example below:</span></span>

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

<span data-ttu-id="86c67-322">No exemplo, o método `F` na classe `Derived` não inclui um `Overrides` modificador e, portanto, não substitui o método `F` na classe `Base`.</span><span class="sxs-lookup"><span data-stu-id="86c67-322">In the example, the method `F` in class `Derived` does not include an `Overrides` modifier and therefore does not override method `F` in class `Base`.</span></span> <span data-ttu-id="86c67-323">Em vez disso, o método `F` na classe `Derived` sombreia um método na classe `Base`, e um aviso é relatado como a declaração não inclui um `Shadows` ou `Overloads` modificador.</span><span class="sxs-lookup"><span data-stu-id="86c67-323">Rather, method `F` in class `Derived` shadows the method in class `Base`, and a warning is reported because the declaration does not include a `Shadows` or `Overloads` modifier.</span></span>

<span data-ttu-id="86c67-324">No exemplo a seguir, o método `F` na classe `Derived` sombreia um método substituível `F` herdada da classe `Base`:</span><span class="sxs-lookup"><span data-stu-id="86c67-324">In the following example, method `F` in class `Derived` shadows the overridable method `F` inherited from class `Base`:</span></span>

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

<span data-ttu-id="86c67-325">Desde o novo método `F` na classe `Derived` tem `Private` acesso, seu escopo inclui apenas o corpo da classe da `Derived` e não se estende a classe `MoreDerived`.</span><span class="sxs-lookup"><span data-stu-id="86c67-325">Since the new method `F` in class `Derived` has `Private` access, its scope only includes the class body of `Derived` and does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="86c67-326">A declaração do método `F` na classe `MoreDerived` , portanto, tem permissão para substituir o método `F` herdada da classe `Base`.</span><span class="sxs-lookup"><span data-stu-id="86c67-326">The declaration of method `F` in class `MoreDerived` is therefore permitted to override the method `F` inherited from class `Base`.</span></span>

<span data-ttu-id="86c67-327">Quando um `Overridable` método é invocado, a implementação mais derivada do método de instância é chamada, com base no tipo de instância, independentemente de é a chamada para o método na classe base ou classe derivada.</span><span class="sxs-lookup"><span data-stu-id="86c67-327">When an `Overridable` method is invoked, the most derived implementation of the instance method is called, based on the type of the instance, regardless of whether the call is to the method in the base class or the derived class.</span></span> <span data-ttu-id="86c67-328">A implementação mais derivada de uma `Overridable` método `M` em relação a uma classe `R` é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="86c67-328">The most derived implementation of an `Overridable` method `M` with respect to a class `R` is determined as follows:</span></span>

* <span data-ttu-id="86c67-329">Se `R` contém o apresentando `Overridable` declaração de `M`, esta é a implementação mais derivada de `M`.</span><span class="sxs-lookup"><span data-stu-id="86c67-329">If `R` contains the introducing `Overridable` declaration of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="86c67-330">Caso contrário, se `R` contém uma substituição do `M`, essa é a implementação mais derivada de `M`.</span><span class="sxs-lookup"><span data-stu-id="86c67-330">Otherwise, if `R` contains an override of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="86c67-331">Caso contrário, a maioria implementação derivada da `M` é o mesmo que a classe base direta de `R`.</span><span class="sxs-lookup"><span data-stu-id="86c67-331">Otherwise, the most derived implementation of `M` is the same as that of the direct base class of `R`.</span></span>

## <a name="accessibility"></a><span data-ttu-id="86c67-332">Acessibilidade</span><span class="sxs-lookup"><span data-stu-id="86c67-332">Accessibility</span></span>

<span data-ttu-id="86c67-333">Especifica uma declaração de *acessibilidade* da entidade, ela declara.</span><span class="sxs-lookup"><span data-stu-id="86c67-333">A declaration specifies the *accessibility* of the entity it declares.</span></span> <span data-ttu-id="86c67-334">Acessibilidade de uma entidade não altera o escopo do nome da entidade.</span><span class="sxs-lookup"><span data-stu-id="86c67-334">An entity's accessibility does not change the scope of an entity's name.</span></span> <span data-ttu-id="86c67-335">O *domínio de acessibilidade* de uma declaração é o conjunto de todos os espaços de declaração na qual a entidade declarada é acessível.</span><span class="sxs-lookup"><span data-stu-id="86c67-335">The *accessibility domain* of a declaration is the set of all declaration spaces in which the declared entity is accessible.</span></span>

<span data-ttu-id="86c67-336">São os tipos de cinco acesso `Public`, `Protected`, `Friend`, `Protected Friend`, e `Private`.</span><span class="sxs-lookup"><span data-stu-id="86c67-336">The five access types are `Public`, `Protected`, `Friend`, `Protected Friend`, and `Private`.</span></span> <span data-ttu-id="86c67-337">`Public` é o tipo de acesso mais permissivo, e os outros quatro tipos são todos os subconjuntos de `Public`.</span><span class="sxs-lookup"><span data-stu-id="86c67-337">`Public` is the most permissive access type, and the four other types are all subsets of `Public`.</span></span> <span data-ttu-id="86c67-338">É o tipo de acesso menos permissivo `Private`, e quatro outros tipos de acesso são todos os subconjuntos de `Private`.</span><span class="sxs-lookup"><span data-stu-id="86c67-338">The least permissive access type is `Private`, and the four other access types are all supersets of `Private`.</span></span>

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

<span data-ttu-id="86c67-339">O tipo de acesso para uma declaração é especificado por meio de um modificador de acesso opcional, que pode ser `Public`, `Protected`, `Friend`, `Private`, ou uma combinação de `Protected` e `Friend`.</span><span class="sxs-lookup"><span data-stu-id="86c67-339">The access type for a declaration is specified via an optional access modifier, which can be `Public`, `Protected`, `Friend`, `Private`, or the combination of `Protected` and `Friend`.</span></span> <span data-ttu-id="86c67-340">Se nenhum modificador de acesso for especificado, o tipo de acesso padrão depende do contexto da declaração; os tipos de acesso permitido também dependem do contexto da declaração.</span><span class="sxs-lookup"><span data-stu-id="86c67-340">If no access modifier is specified, the default access type depends on the declaration context; the permitted access types also depend on the declaration context.</span></span>

* <span data-ttu-id="86c67-341">Entidades declaradas com o `Public` modificador ter `Public` acesso.</span><span class="sxs-lookup"><span data-stu-id="86c67-341">Entities declared with the `Public` modifier have `Public` access.</span></span> <span data-ttu-id="86c67-342">Não existem restrições no uso de `Public` entidades.</span><span class="sxs-lookup"><span data-stu-id="86c67-342">There are no restrictions on the use of `Public` entities.</span></span>

* <span data-ttu-id="86c67-343">Entidades declaradas com o `Protected` modificador ter `Protected` acesso.</span><span class="sxs-lookup"><span data-stu-id="86c67-343">Entities declared with the `Protected` modifier have `Protected` access.</span></span> <span data-ttu-id="86c67-344">`Protected` acesso só pode ser especificado em membros de classes (membros de tipo regular e classes aninhadas) ou no `Overridable` membros de estruturas e os módulos padrão (que deve, por definição, ser herdada `System.Object` ou `System.ValueType`).</span><span class="sxs-lookup"><span data-stu-id="86c67-344">`Protected` access can only be specified on members of classes (both regular type members and nested classes) or on `Overridable` members of standard modules and structures (which must, by definition, be inherited from `System.Object` or `System.ValueType`).</span></span> <span data-ttu-id="86c67-345">Um `Protected` membro é acessível por uma classe derivada, desde que o membro não é um membro de instância, ou o acesso ocorre por meio de uma instância da classe derivada.</span><span class="sxs-lookup"><span data-stu-id="86c67-345">A `Protected` member is accessible to a derived class, provided that either the member is not an instance member, or the access takes place through an instance of the derived class.</span></span> <span data-ttu-id="86c67-346">`Protected` acesso não é um superconjunto de `Friend` acesso.</span><span class="sxs-lookup"><span data-stu-id="86c67-346">`Protected` access is not a superset of `Friend` access.</span></span>

* <span data-ttu-id="86c67-347">Entidades declaradas com o `Friend` modificador ter `Friend` acesso.</span><span class="sxs-lookup"><span data-stu-id="86c67-347">Entities declared with the `Friend` modifier have `Friend` access.</span></span> <span data-ttu-id="86c67-348">Uma entidade com `Friend` o acesso é acessível somente dentro do programa que contém a declaração de entidade ou todos os assemblies que receberam `Friend` acessar por meio de `System.Runtime.CompilerServices.InternalsVisibleToAttribute` atributo.</span><span class="sxs-lookup"><span data-stu-id="86c67-348">An entity with `Friend` access is accessible only within the program that contains the entity declaration or any assemblies that have been given `Friend` access through the `System.Runtime.CompilerServices.InternalsVisibleToAttribute` attribute.</span></span>

* <span data-ttu-id="86c67-349">Entidades declaradas com o `Protected Friend` modificadores têm a união de `Protected` e `Friend` acesso.</span><span class="sxs-lookup"><span data-stu-id="86c67-349">Entities declared with the `Protected Friend` modifiers have the union of `Protected` and `Friend` access.</span></span>

* <span data-ttu-id="86c67-350">Entidades declaradas com o `Private` modificador ter `Private` acesso.</span><span class="sxs-lookup"><span data-stu-id="86c67-350">Entities declared with the `Private` modifier have `Private` access.</span></span> <span data-ttu-id="86c67-351">Um `Private` entidade é acessível somente dentro de seu contexto de declaração, incluindo quaisquer entidades aninhadas.</span><span class="sxs-lookup"><span data-stu-id="86c67-351">A `Private` entity is accessible only within its declaration context, including any nested entities.</span></span>

<span data-ttu-id="86c67-352">A acessibilidade em uma declaração não depende da acessibilidade do contexto da declaração.</span><span class="sxs-lookup"><span data-stu-id="86c67-352">The accessibility in a declaration does not depend on the accessibility of the declaration context.</span></span> <span data-ttu-id="86c67-353">Por exemplo, um tipo declarado com `Private` acesso pode conter um membro de tipo `Public` acesso.</span><span class="sxs-lookup"><span data-stu-id="86c67-353">For example, a type declared with `Private` access may contain a type member with `Public` access.</span></span>

<span data-ttu-id="86c67-354">O código a seguir demonstra vários domínios de acessibilidade:</span><span class="sxs-lookup"><span data-stu-id="86c67-354">The following code demonstrates various accessibility domains:</span></span>

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

<span data-ttu-id="86c67-355">As classes e membros neste exemplo tem os domínios de acessibilidade a seguir:</span><span class="sxs-lookup"><span data-stu-id="86c67-355">The classes and members in this example have the following accessibility domains:</span></span>

* <span data-ttu-id="86c67-356">Domínio de acessibilidade de `A` e `A.X` é ilimitado.</span><span class="sxs-lookup"><span data-stu-id="86c67-356">The accessibility domain of `A` and `A.X` is unlimited.</span></span>

* <span data-ttu-id="86c67-357">Domínio de acessibilidade de `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, e `B.C.Y` é o programa de recipiente.</span><span class="sxs-lookup"><span data-stu-id="86c67-357">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the containing program.</span></span>

* <span data-ttu-id="86c67-358">Domínio de acessibilidade de `A.Z` é `A.`</span><span class="sxs-lookup"><span data-stu-id="86c67-358">The accessibility domain of `A.Z` is `A.`</span></span>

* <span data-ttu-id="86c67-359">Domínio de acessibilidade de `B.Z`, `B.D`, `B.D.X`, e `B.D.Y` está `B`, incluindo `B.C` e `B.D`.</span><span class="sxs-lookup"><span data-stu-id="86c67-359">The accessibility domain of `B.Z`, `B.D`, `B.D.X`, and `B.D.Y` is `B`, including `B.C` and `B.D`.</span></span>

* <span data-ttu-id="86c67-360">Domínio de acessibilidade de `B.C.Z` é `B.C`.</span><span class="sxs-lookup"><span data-stu-id="86c67-360">The accessibility domain of `B.C.Z` is `B.C`.</span></span>

* <span data-ttu-id="86c67-361">Domínio de acessibilidade de `B.D.Z` é `B.D`.</span><span class="sxs-lookup"><span data-stu-id="86c67-361">The accessibility domain of `B.D.Z` is `B.D`.</span></span>

<span data-ttu-id="86c67-362">Como mostra o exemplo, o domínio de acessibilidade de um membro nunca é maior do que um tipo contido.</span><span class="sxs-lookup"><span data-stu-id="86c67-362">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="86c67-363">Por exemplo, mesmo que todos os `X` os membros têm `Public` declarado acessibilidade, tudo, exceto `A.X` tem domínios de acessibilidade que são restritos por um tipo de contenção.</span><span class="sxs-lookup"><span data-stu-id="86c67-363">For example, even though all `X` members have `Public` declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="86c67-364">Acesso a `Protected` membros devem ser através de uma instância do tipo derivado para que não relacionados de tipos não podem acessar uns aos outros de instância tem membros protegidos.</span><span class="sxs-lookup"><span data-stu-id="86c67-364">Access to `Protected` instance members must be through an instance of the derived type so that unrelated types cannot gain access to each other's protected members.</span></span> <span data-ttu-id="86c67-365">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-365">For example:</span></span>

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

<span data-ttu-id="86c67-366">No exemplo acima, a classe `Guest` tem acesso apenas às protegido `Password` campo se ele for qualificado com uma instância de `Guest`.</span><span class="sxs-lookup"><span data-stu-id="86c67-366">In the above example, the class `Guest` only has access to the protected `Password` field if it is qualified with an instance of `Guest`.</span></span> <span data-ttu-id="86c67-367">Isso impede que `Guest` obtenham acesso para o `Password` campo de uma `Employee` objeto simplesmente ao convertê-la para `User`.</span><span class="sxs-lookup"><span data-stu-id="86c67-367">This prevents `Guest` from gaining access to the `Password` field of an `Employee` object simply by casting it to `User`.</span></span>

<span data-ttu-id="86c67-368">Para fins de `Protected` acesso de membro em tipos genéricos, o contexto da declaração inclui parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-368">For the purposes of `Protected` member access in generic types, the declaration context includes type parameters.</span></span> <span data-ttu-id="86c67-369">Isso significa que um tipo derivado com um conjunto de argumentos de tipo não tem acesso para o `Protected` membros de um tipo derivado com um conjunto diferente de argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-369">This means that a derived type with one set of type arguments does not have access to the `Protected` members of a derived type with a different set of type arguments.</span></span> <span data-ttu-id="86c67-370">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-370">For example:</span></span>

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

<span data-ttu-id="86c67-371">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="86c67-371">__Note.__</span></span> <span data-ttu-id="86c67-372">A linguagem c# (e possivelmente outras linguagens) permite que um tipo genérico acessar `Protected` membros, independentemente de quais argumentos de tipo são fornecidos.</span><span class="sxs-lookup"><span data-stu-id="86c67-372">The C# language (and possibly other languages) allows a generic type to access `Protected` members regardless of what type arguments are supplied.</span></span> <span data-ttu-id="86c67-373">Isso deve ser mantido em mente ao projetar classes genéricas que contêm `Protected` membros.</span><span class="sxs-lookup"><span data-stu-id="86c67-373">This should be kept in mind when designing generic classes that contain `Protected` members.</span></span>


### <a name="constituent-types"></a><span data-ttu-id="86c67-374">Tipos constituintes</span><span class="sxs-lookup"><span data-stu-id="86c67-374">Constituent Types</span></span>

<span data-ttu-id="86c67-375">O *constituintes tipos* de uma declaração são os tipos que são referenciados pela declaração.</span><span class="sxs-lookup"><span data-stu-id="86c67-375">The *constituent types* of a declaration are the types that are referenced by the declaration.</span></span> <span data-ttu-id="86c67-376">Por exemplo, o tipo de uma constante, o tipo de retorno de um método e os tipos de parâmetro de um construtor são todos os tipos constituintes.</span><span class="sxs-lookup"><span data-stu-id="86c67-376">For example, the type of a constant, the return type of a method and the parameter types of a constructor are all constituent types.</span></span> <span data-ttu-id="86c67-377">O domínio de acessibilidade de um tipo constituinte de uma declaração deve ser igual ou um superconjunto do domínio de acessibilidade da declaração em si.</span><span class="sxs-lookup"><span data-stu-id="86c67-377">The accessibility domain of a constituent type of a declaration must be the same as or a superset of the accessibility domain of the declaration itself.</span></span> <span data-ttu-id="86c67-378">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-378">For example:</span></span>

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

## <a name="type-and-namespace-names"></a><span data-ttu-id="86c67-379">Nomes de Namespace e tipo</span><span class="sxs-lookup"><span data-stu-id="86c67-379">Type and Namespace Names</span></span>

<span data-ttu-id="86c67-380">Muitas construções de linguagem exigem um namespace ou tipo a ser especificado; eles podem ser especificados usando um formulário qualificado de namespace ou nome do tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-380">Many language constructs require a namespace or type to be specified; these can be specified by using a qualified form of the namespace or type's name.</span></span> <span data-ttu-id="86c67-381">Um *nome qualificado* consiste em uma série de identificadores separados por pontos, o identificador à direita de um período é resolvido no espaço de declaração especificado pelo identificador no lado esquerdo do período.</span><span class="sxs-lookup"><span data-stu-id="86c67-381">A *qualified name* consists of a series of identifiers separated by periods; the identifier on the right side of a period is resolved in the declaration space specified by the identifier on the left side of the period.</span></span>

<span data-ttu-id="86c67-382">O *nome totalmente qualificado* de um tipo ou namespace é um nome qualificado que contém o nome de todos os que contém namespaces e tipos.</span><span class="sxs-lookup"><span data-stu-id="86c67-382">The *fully qualified name* of a namespace or type is a qualified name that contains the name of all containing namespaces and types.</span></span> <span data-ttu-id="86c67-383">Em outras palavras, o nome totalmente qualificado de um tipo ou namespace é `N.T`, onde `T` é o nome da entidade e `N` é o nome totalmente qualificado da sua entidade contentora.</span><span class="sxs-lookup"><span data-stu-id="86c67-383">In other words, the fully qualified name of a namespace or type is `N.T`, where `T` is the name of the entity and `N` is the fully qualified name of its containing entity.</span></span>

<span data-ttu-id="86c67-384">O exemplo a seguir mostra várias declarações de namespace e o tipo junto com seus nomes totalmente qualificados associados nos comentários na linha.</span><span class="sxs-lookup"><span data-stu-id="86c67-384">The example below shows several namespace and type declarations together with their associated fully qualified names in in-line comments.</span></span>

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

<span data-ttu-id="86c67-385">Observe que o namespace x. y foi declarada em dois locais diferentes no código-fonte, mas essas duas declarações parciais constituem apenas um único namespace chamado x. y que contém a classe D e a classe E.</span><span class="sxs-lookup"><span data-stu-id="86c67-385">Observe that the namespace X.Y has been declared in two different locations in the source code, but these two partial declarations constitute just a single namespace called X.Y which contains both class D and class E.</span></span>

<span data-ttu-id="86c67-386">Em algumas situações, um nome qualificado pode começar com a palavra-chave `Global`.</span><span class="sxs-lookup"><span data-stu-id="86c67-386">In some situations, a qualified name may begin with the keyword `Global`.</span></span> <span data-ttu-id="86c67-387">A palavra-chave representa o namespace externo sem nome, que é útil em situações em que uma declaração se sobrepõe a um namespace delimitador.</span><span class="sxs-lookup"><span data-stu-id="86c67-387">The keyword represents the unnamed outermost namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="86c67-388">O `Global` permite a palavra-chave "escape" out para o namespace mais externo nessa situação.</span><span class="sxs-lookup"><span data-stu-id="86c67-388">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="86c67-389">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-389">For example:</span></span>

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

<span data-ttu-id="86c67-390">No exemplo acima, a primeira chamada de método é inválida porque o identificador `System` associa à classe `System`, não o namespace `System`.</span><span class="sxs-lookup"><span data-stu-id="86c67-390">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="86c67-391">A única maneira de acessar o `System` namespace é usar `Global` pulem-out para o namespace mais externo.</span><span class="sxs-lookup"><span data-stu-id="86c67-391">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span> <span data-ttu-id="86c67-392">`Global` não pode ser usado em uma `Imports` instrução ou `Namespace` declaração.</span><span class="sxs-lookup"><span data-stu-id="86c67-392">`Global` cannot be used in an `Imports` statement or `Namespace` declaration.</span></span>

<span data-ttu-id="86c67-393">Como outras linguagens podem introduzir tipos e namespaces que correspondem a palavras-chave na linguagem, o Visual Basic reconhece palavras-chave para fazer parte de um nome qualificado, desde que sigam um período.</span><span class="sxs-lookup"><span data-stu-id="86c67-393">Because other languages may introduce types and namespaces that match keywords in the language, Visual Basic recognizes keywords to be part of a qualified name as long as they follow a period.</span></span> <span data-ttu-id="86c67-394">Palavras-chave usadas dessa maneira são tratadas como identificadores.</span><span class="sxs-lookup"><span data-stu-id="86c67-394">Keywords used in this way are treated as identifiers.</span></span> <span data-ttu-id="86c67-395">Por exemplo, o identificador qualificado `X.Default.Class` é um identificador qualificado válido, enquanto `Default.Class` não é.</span><span class="sxs-lookup"><span data-stu-id="86c67-395">For example, the qualified identifier `X.Default.Class` is a valid qualified identifier, while `Default.Class` is not.</span></span>

### <a name="qualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="86c67-396">Resolução de nome qualificado de namespaces e tipos</span><span class="sxs-lookup"><span data-stu-id="86c67-396">Qualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="86c67-397">Dado um nome qualificado de namespace ou tipo do formulário `N.R(Of A)`, onde `R` é o identificador mais à direita no nome qualificado e `A` é uma lista de argumentos de tipo opcional, as etapas a seguir descrevem como determinar a qual namespace ou o nome qualificado do tipo refere-se:</span><span class="sxs-lookup"><span data-stu-id="86c67-397">Given a qualified namespace or type name of the form `N.R(Of A)`, where `R` is the rightmost identifier in the qualified name and `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the qualified name refers:</span></span>

1. <span data-ttu-id="86c67-398">Resolver `N`, usando as regras para a resolução de nome qualificado ou não.</span><span class="sxs-lookup"><span data-stu-id="86c67-398">Resolve `N`, using the rules for either qualified or unqualified name resolution.</span></span>

2. <span data-ttu-id="86c67-399">Se a resolução de `N` falhar ou se resolve para um parâmetro de tipo, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86c67-399">If resolution of `N` fails, or resolves to a type parameter, a compile-time error occurs.</span></span>

3. <span data-ttu-id="86c67-400">Caso contrário, se `R` correspondências de nome de um namespace em N e nenhum argumento de tipo foram fornecidos, ou `R` corresponde a um tipo acessível no `N` com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em seguida, o nome qualificado refere-se a esse namespace ou tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-400">Otherwise, if `R` matches the name of a namespace in N and no type arguments were supplied, or `R` matches an accessible type in `N` with the same number of type parameters as type arguments, if any, then the qualified name refers to that namespace or type.</span></span>

4. <span data-ttu-id="86c67-401">Caso contrário, se `N` contém um ou mais módulos padrão e `R` corresponde ao nome de um tipo acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em exatamente um módulo padrão e, em seguida, o nome qualificado refere-se ao tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-401">Otherwise, if `N` contains one or more standard modules, and `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the qualified name refers to that type.</span></span> <span data-ttu-id="86c67-402">Se `R` corresponde ao nome dos tipos acessíveis com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em mais de um módulo padrão, um erro de tempo de compilação ocorrer.</span><span class="sxs-lookup"><span data-stu-id="86c67-402">If `R` matches the name of accessible types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

5. <span data-ttu-id="86c67-403">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86c67-403">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="86c67-404">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="86c67-404">__Note.__</span></span> <span data-ttu-id="86c67-405">Uma implicação desse processo de resolução é que os membros de tipo não sombra namespaces ou tipos durante a resolução de nomes de namespace ou tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-405">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="86c67-406">Resolução de nomes não qualificados para namespaces e tipos</span><span class="sxs-lookup"><span data-stu-id="86c67-406">Unqualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="86c67-407">Dado um nome não qualificado `R(Of A)`, onde `A` é uma lista de argumentos de tipo opcional, as etapas a seguir descrevem como determinar a qual o namespace ou tipo refere-se o nome não qualificado:</span><span class="sxs-lookup"><span data-stu-id="86c67-407">Given an unqualified name `R(Of A)`, where `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the unqualified name refers:</span></span>

1. <span data-ttu-id="86c67-408">Se R corresponde ao nome de um parâmetro de tipo do método atual e foi fornecido nenhum argumento de tipo, o nome não qualificado se refere a esse parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-408">If R matches the name of a type parameter of the current method, and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>

2.  <span data-ttu-id="86c67-409">Para cada tipo que contém a referência de nome, começando do tipo interno e vai mais externo aninhado:</span><span class="sxs-lookup"><span data-stu-id="86c67-409">For each nested type containing the name reference, starting from the innermost type and going to the outermost:</span></span>
    21. <span data-ttu-id="86c67-410">Se `R` corresponde ao nome de um parâmetro de tipo no tipo atual e nenhum tipo de argumentos foram fornecidos, em seguida, o nome não qualificado se refere a esse parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-410">If `R` matches the name of a type parameter in the current type and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>
    22. <span data-ttu-id="86c67-411">Caso contrário, se `R` correspondências, o nome de um acessível aninhadas de tipo com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em seguida, o nome não qualificado faz referência a esse tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-411">Otherwise, if `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, then the unqualified name refers to that type.</span></span>

3. <span data-ttu-id="86c67-412">Para cada namespace aninhado que contém a referência de nome, a partir do namespace interno e passando para o namespace externo:</span><span class="sxs-lookup"><span data-stu-id="86c67-412">For each nested namespace containing the name reference, starting from the innermost namespace and going to the outermost namespace:</span></span>
    31. <span data-ttu-id="86c67-413">Se `R` correspondências, o nome de um namespace aninhado no namespace atual e nenhuma lista de argumentos de tipo for fornecido, então o nome não qualificado faz referência a esse namespace aninhado.</span><span class="sxs-lookup"><span data-stu-id="86c67-413">If `R` matches the name of a nested namespace in the current namespace and no type argument list is supplied, then the unqualified name refers to that nested namespace.</span></span>
    32. <span data-ttu-id="86c67-414">Caso contrário, se `R` corresponde ao nome de um tipo acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, no namespace atual e, em seguida, o nome não qualificado faz referência a esse tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-414">Otherwise, if `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in the current namespace, then the unqualified name refers to that type.</span></span>
    33. <span data-ttu-id="86c67-415">Caso contrário, se o namespace contém um ou mais acessíveis módulos padrão, e `R` corresponde ao nome de um tipo aninhado acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em exatamente um módulo padrão, então não qualificada nome se refere a esse tipo aninhado.</span><span class="sxs-lookup"><span data-stu-id="86c67-415">Otherwise, if the namespace contains one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that nested type.</span></span> <span data-ttu-id="86c67-416">Se `R` corresponde ao nome de tipos aninhados acessíveis com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em mais de um módulo padrão, um erro de tempo de compilação ocorrer.</span><span class="sxs-lookup"><span data-stu-id="86c67-416">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

4. <span data-ttu-id="86c67-417">Se o arquivo de origem tem um ou mais aliases de importação e `R` corresponde ao nome de um deles, em seguida, o nome não qualificado se refere a esse alias de importação.</span><span class="sxs-lookup"><span data-stu-id="86c67-417">If the source file has one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="86c67-418">Se uma lista de argumentos de tipo for fornecida, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86c67-418">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="86c67-419">Se o arquivo de origem que contém a referência de nome tem um ou mais importações:</span><span class="sxs-lookup"><span data-stu-id="86c67-419">If the source file containing the name reference has one or more imports:</span></span>
    51. <span data-ttu-id="86c67-420">Se `R` corresponde ao nome de um tipo acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em Importar exatamente um e, em seguida, o nome não qualificado se refere a esse tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-420">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="86c67-421">Se `R` corresponde ao nome de um tipo acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em mais de uma importação e todos os não são do mesmo tipo, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86c67-421">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import and all are not the same type, a compile-time error occurs.</span></span>
    52. <span data-ttu-id="86c67-422">Caso contrário, se nenhum tipo de lista de argumentos foi fornecido e `R` corresponde ao nome de um namespace com tipos acessíveis em exatamente uma importação, em seguida, o nome não qualificado faz referência a esse namespace.</span><span class="sxs-lookup"><span data-stu-id="86c67-422">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="86c67-423">Se nenhum tipo de lista de argumentos foi fornecido e `R` correspondências, o nome de um namespace com tipos acessíveis em mais de uma importação e todos os não são o mesmo namespace, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86c67-423">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import and all are not the same namespace, a compile-time error occurs.</span></span>
    53. <span data-ttu-id="86c67-424">Caso contrário, se as importações contêm um ou mais acessíveis módulos padrão, e `R` coincide com o nome de um tipo aninhado acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em exatamente um módulo padrão, em seguida, o nome não qualificado refere-se a esse tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-424">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="86c67-425">Se `R` corresponde ao nome de tipos aninhados acessíveis com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em mais de um módulo padrão, um erro de tempo de compilação ocorrer.</span><span class="sxs-lookup"><span data-stu-id="86c67-425">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

6. <span data-ttu-id="86c67-426">Se o ambiente de compilação define um ou mais aliases de importação e `R` corresponde ao nome de um deles, em seguida, o nome não qualificado se refere a esse alias de importação.</span><span class="sxs-lookup"><span data-stu-id="86c67-426">If the compilation environment defines one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="86c67-427">Se uma lista de argumentos de tipo for fornecida, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86c67-427">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="86c67-428">Se o ambiente de compilação define uma ou mais importações:</span><span class="sxs-lookup"><span data-stu-id="86c67-428">If the compilation environment defines one or more imports:</span></span>
    71. <span data-ttu-id="86c67-429">Se `R` corresponde ao nome de um tipo acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em Importar exatamente um e, em seguida, o nome não qualificado se refere a esse tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-429">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="86c67-430">Se `R` corresponde ao nome de um tipo acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em mais de uma importação, um erro de tempo de compilação ocorrer.</span><span class="sxs-lookup"><span data-stu-id="86c67-430">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import, a compile-time error occurs.</span></span>
    72. <span data-ttu-id="86c67-431">Caso contrário, se nenhum tipo de lista de argumentos foi fornecido e `R` corresponde ao nome de um namespace com tipos acessíveis em exatamente uma importação, em seguida, o nome não qualificado faz referência a esse namespace.</span><span class="sxs-lookup"><span data-stu-id="86c67-431">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="86c67-432">Se nenhum tipo de lista de argumentos foi fornecido e `R` corresponde ao nome de um namespace com tipos acessíveis em mais de uma importação, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86c67-432">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import, a compile-time error occurs.</span></span>
    73. <span data-ttu-id="86c67-433">Caso contrário, se as importações contêm um ou mais acessíveis módulos padrão, e `R` coincide com o nome de um tipo aninhado acessível com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em exatamente um módulo padrão, em seguida, o nome não qualificado refere-se a esse tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-433">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="86c67-434">Se `R` corresponde ao nome de tipos aninhados acessíveis com o mesmo número de parâmetros de tipo como argumentos de tipo, se houver, em mais de um módulo padrão, um erro de tempo de compilação ocorrer.</span><span class="sxs-lookup"><span data-stu-id="86c67-434">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="86c67-435">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86c67-435">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="86c67-436">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="86c67-436">__Note.__</span></span> <span data-ttu-id="86c67-437">Uma implicação desse processo de resolução é que os membros de tipo não sombra namespaces ou tipos durante a resolução de nomes de namespace ou tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-437">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

<span data-ttu-id="86c67-438">Normalmente, um nome pode ocorrer apenas uma vez em um namespace específico.</span><span class="sxs-lookup"><span data-stu-id="86c67-438">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="86c67-439">No entanto, como namespaces podem ser declarados em vários assemblies do .NET, é possível ter uma situação em que dois assemblies definem um tipo com o mesmo nome totalmente qualificado.</span><span class="sxs-lookup"><span data-stu-id="86c67-439">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="86c67-440">Nesse caso, um tipo declarado no conjunto atual de arquivos de origem é preferível ao longo de um tipo declarado em um assembly .NET externo.</span><span class="sxs-lookup"><span data-stu-id="86c67-440">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="86c67-441">Caso contrário, o nome é ambíguo e não há nenhuma maneira de resolver a ambiguidade do nome.</span><span class="sxs-lookup"><span data-stu-id="86c67-441">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>

## <a name="variables"></a><span data-ttu-id="86c67-442">Variáveis</span><span class="sxs-lookup"><span data-stu-id="86c67-442">Variables</span></span>

<span data-ttu-id="86c67-443">Um *variável* representa um local de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="86c67-443">A *variable* represents a storage location.</span></span> <span data-ttu-id="86c67-444">Cada variável tem um tipo que determina quais valores podem ser armazenados na variável.</span><span class="sxs-lookup"><span data-stu-id="86c67-444">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="86c67-445">Porque o Visual Basic é uma linguagem fortemente tipada, cada variável em um programa tem um tipo e a linguagem garante que os valores armazenados em variáveis sempre são do tipo apropriado.</span><span class="sxs-lookup"><span data-stu-id="86c67-445">Because Visual Basic is a type-safe language, every variable in a program has a type and the language guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="86c67-446">As variáveis são inicializadas sempre para o valor padrão de seu tipo antes que qualquer referência à variável pode ser feita.</span><span class="sxs-lookup"><span data-stu-id="86c67-446">Variables are always initialized to the default value of their type before any reference to the variable can be made.</span></span> <span data-ttu-id="86c67-447">Não é possível acessar a memória não inicializada.</span><span class="sxs-lookup"><span data-stu-id="86c67-447">It is not possible to access uninitialized memory.</span></span>

## <a name="generic-types-and-methods"></a><span data-ttu-id="86c67-448">Tipos e métodos genéricos</span><span class="sxs-lookup"><span data-stu-id="86c67-448">Generic Types and Methods</span></span>

<span data-ttu-id="86c67-449">Métodos e tipos (exceto para os módulos padrão e os tipos enumerados) podem declarar *parâmetros de tipo*, que são tipos que não serão fornecidos até que uma instância do tipo é declarado ou o método é invocado.</span><span class="sxs-lookup"><span data-stu-id="86c67-449">Types (except for standard modules and enumerated types) and methods can declare *type parameters*, which are types that will not be provided until an instance of the type is declared or the method is invoked.</span></span> <span data-ttu-id="86c67-450">Tipos e métodos com parâmetros de tipo também são conhecidos como *tipos genéricos* e *métodos genéricos*, respectivamente, porque o tipo ou método deve ser escrito de forma genérica, sem conhecimento específico das tipos que serão fornecidos pelo código que usa o tipo ou método.</span><span class="sxs-lookup"><span data-stu-id="86c67-450">Types and methods with type parameters are also known as *generic types* and *generic methods*, respectively, because the type or method must be written generically, without specific knowledge of the types that will be supplied by code that uses the type or method.</span></span>

<span data-ttu-id="86c67-451">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="86c67-451">__Note.__</span></span> <span data-ttu-id="86c67-452">Neste momento, mesmo que os delegados e métodos podem ser genéricos, propriedades, eventos e operadores não podem ser genéricos próprios.</span><span class="sxs-lookup"><span data-stu-id="86c67-452">At this time, even though methods and delegates can be generic, properties, events and operators cannot be generic themselves.</span></span> <span data-ttu-id="86c67-453">No entanto, eles podem, usar parâmetros de tipo da classe recipiente.</span><span class="sxs-lookup"><span data-stu-id="86c67-453">They may, however, use type parameters from the containing class.</span></span>

<span data-ttu-id="86c67-454">Da perspectiva de tipo genérico ou método, um parâmetro de tipo é um tipo de espaço reservado que será preenchido com um tipo real quando o tipo ou método é usado.</span><span class="sxs-lookup"><span data-stu-id="86c67-454">From the perspective of the generic type or method, a type parameter is a placeholder type that will be filled in with an actual type when the type or method is used.</span></span> <span data-ttu-id="86c67-455">Argumentos de tipo são substituídos por parâmetros de tipo no tipo ou método no ponto em que o tipo ou método é usado.</span><span class="sxs-lookup"><span data-stu-id="86c67-455">Type arguments are substituted for the type parameters in the type or method at the point at which the type or method is used.</span></span> <span data-ttu-id="86c67-456">Por exemplo, uma classe de pilha genérico pode ser implementada como:</span><span class="sxs-lookup"><span data-stu-id="86c67-456">For example, a generic stack class could be implemented as:</span></span>

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

<span data-ttu-id="86c67-457">As declarações que usam o `Stack(Of ItemType)` classe deve fornecer um argumento de tipo para o parâmetro de tipo `ItemType`.</span><span class="sxs-lookup"><span data-stu-id="86c67-457">Declarations that use the `Stack(Of ItemType)` class must supply a type argument for the type parameter `ItemType`.</span></span> <span data-ttu-id="86c67-458">Esse tipo é preenchido, em seguida, independentemente de onde `ItemType` é usado dentro da classe:</span><span class="sxs-lookup"><span data-stu-id="86c67-458">This type is then filled in wherever `ItemType` is used within the class:</span></span>

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

### <a name="type-parameters"></a><span data-ttu-id="86c67-459">Parâmetros de tipo</span><span class="sxs-lookup"><span data-stu-id="86c67-459">Type Parameters</span></span>

<span data-ttu-id="86c67-460">Parâmetros de tipo podem ser fornecidos em declarações de tipo ou método.</span><span class="sxs-lookup"><span data-stu-id="86c67-460">Type parameters may be supplied on type or method declarations.</span></span> <span data-ttu-id="86c67-461">Cada parâmetro de tipo é um identificador que é um espaço reservado para um argumento de tipo que é fornecido para criar um tipo construído ou método.</span><span class="sxs-lookup"><span data-stu-id="86c67-461">Each type parameter is an identifier which is a place-holder for a type argument that is supplied to create a constructed type or method.</span></span> <span data-ttu-id="86c67-462">Por outro lado, um argumento de tipo é o tipo real que é substituído para o parâmetro de tipo quando um tipo genérico ou método é usado.</span><span class="sxs-lookup"><span data-stu-id="86c67-462">By contrast, a type argument is the actual type that is substituted for the type parameter when a generic type or method is used.</span></span>

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

<span data-ttu-id="86c67-463">Cada parâmetro de tipo em uma declaração de tipo ou método define um nome no espaço de declaração desse tipo ou método.</span><span class="sxs-lookup"><span data-stu-id="86c67-463">Each type parameter in a type or method declaration defines a name in the declaration space of that type or method.</span></span> <span data-ttu-id="86c67-464">Portanto, ele não pode ter o mesmo nome que outro parâmetro de tipo, um membro de tipo, um parâmetro de método ou uma variável local.</span><span class="sxs-lookup"><span data-stu-id="86c67-464">Thus, it cannot have the same name as another type parameter, a type member, a method parameter, or a local variable.</span></span> <span data-ttu-id="86c67-465">O escopo de um parâmetro de tipo em um tipo ou método é o tipo de inteiro ou método.</span><span class="sxs-lookup"><span data-stu-id="86c67-465">The scope of a type parameter on a type or method is the entire type or method.</span></span> <span data-ttu-id="86c67-466">Porque os parâmetros de tipo estão no escopo para a declaração de tipo inteiro, tipos aninhados podem usar parâmetros de tipo externo.</span><span class="sxs-lookup"><span data-stu-id="86c67-466">Because type parameters are scoped to the entire type declaration, nested types can use outer type parameters.</span></span> <span data-ttu-id="86c67-467">Isso também significa que os parâmetros de tipo sempre devem ser especificados quando acessar tipos aninhados dentro de tipos genéricos:</span><span class="sxs-lookup"><span data-stu-id="86c67-467">This also means that type parameters must always be specified when accessing types nested inside generic types:</span></span>

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

<span data-ttu-id="86c67-468">Ao contrário de outros membros de uma classe, parâmetros de tipo não são herdados.</span><span class="sxs-lookup"><span data-stu-id="86c67-468">Unlike other members of a class, type parameters are not inherited.</span></span> <span data-ttu-id="86c67-469">Parâmetros de tipo em um tipo só podem ser referenciados por seus nomes simples; em outras palavras, eles não podem ser qualificados com o nome do tipo recipiente.</span><span class="sxs-lookup"><span data-stu-id="86c67-469">Type parameters in a type can only be referred to by their simple name; in other words, they cannot be qualified with the containing type name.</span></span> <span data-ttu-id="86c67-470">Embora seja ruim programação estilo, os parâmetros de tipo em um tipo aninhado podem ocultar um membro ou parâmetro declarado no tipo externo do tipo:</span><span class="sxs-lookup"><span data-stu-id="86c67-470">Although it is bad programming style, the type parameters in a nested type can hide a member or type parameter declared in the outer type:</span></span>

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

<span data-ttu-id="86c67-471">Tipos e métodos podem ser sobrecarregados com base no número de parâmetros de tipo (ou *arity*) que declaram os tipos ou métodos.</span><span class="sxs-lookup"><span data-stu-id="86c67-471">Types and methods may be overloaded based on the number of type parameters (or *arity*) that the types or methods declare.</span></span> <span data-ttu-id="86c67-472">Por exemplo, as seguintes declarações são aceitáveis:</span><span class="sxs-lookup"><span data-stu-id="86c67-472">For example, the following declarations are legal:</span></span>

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

<span data-ttu-id="86c67-473">No caso de tipos, sobrecargas sempre são correspondidas em relação ao número de argumentos de tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="86c67-473">In the case of types, overloads are always matched against the number of type arguments specified.</span></span> <span data-ttu-id="86c67-474">Isso é útil ao usar as classes genéricas e não genéricas juntos no mesmo programa:</span><span class="sxs-lookup"><span data-stu-id="86c67-474">This is useful when using both generic and non-generic classes together in the same program:</span></span>

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

<span data-ttu-id="86c67-475">Regras para métodos sobrecarregados em parâmetros de tipo são abordadas na seção sobre resolução de sobrecarga de método.</span><span class="sxs-lookup"><span data-stu-id="86c67-475">Rules for methods overloaded on type parameters are covered in the section on method overload resolution.</span></span>

<span data-ttu-id="86c67-476">Dentro da declaração de recipiente, parâmetros de tipo são considerados tipos completos.</span><span class="sxs-lookup"><span data-stu-id="86c67-476">Within the containing declaration, type parameters are considered full types.</span></span> <span data-ttu-id="86c67-477">Uma vez que um parâmetro de tipo pode ser instanciado com muitos argumentos de tipo real diferente, parâmetros de tipo têm um pouco diferentes operações e restrições que outros tipos, conforme descrito abaixo:</span><span class="sxs-lookup"><span data-stu-id="86c67-477">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types as described below:</span></span>

* <span data-ttu-id="86c67-478">Um parâmetro de tipo não pode ser usado diretamente para declarar uma classe base ou interface.</span><span class="sxs-lookup"><span data-stu-id="86c67-478">A type parameter cannot be used directly to declare a base class or interface.</span></span>

* <span data-ttu-id="86c67-479">As regras para a pesquisa de membro no tipo de parâmetros dependem de restrições, se houver, é aplicado ao parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-479">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span>

* <span data-ttu-id="86c67-480">As conversões disponíveis para um parâmetro de tipo depende das restrições, se houver, aplicado aos parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-480">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameters.</span></span>

* <span data-ttu-id="86c67-481">Na ausência de um `Structure` restrição, um valor com um tipo representado por um parâmetro de tipo pode ser comparado com `Nothing` usando `Is` e `IsNot`.</span><span class="sxs-lookup"><span data-stu-id="86c67-481">In the absence of a `Structure` constraint, a value with a type represented by a type parameter can be compared with `Nothing` using `Is` and `IsNot`.</span></span>

* <span data-ttu-id="86c67-482">Um parâmetro de tipo só pode ser usado em uma `New` expressão se o parâmetro de tipo é restrito por um `New` ou um `Structure` restrição.</span><span class="sxs-lookup"><span data-stu-id="86c67-482">A type parameter can only be used in a `New` expression if the type parameter is constrained by a `New` or a `Structure` constraint.</span></span>

* <span data-ttu-id="86c67-483">Um parâmetro de tipo não pode ser usado em qualquer lugar dentro de uma exceção de atributo dentro de um `GetType` expressão.</span><span class="sxs-lookup"><span data-stu-id="86c67-483">A type parameter cannot be used anywhere within an attribute exception within a `GetType` expression.</span></span>

* <span data-ttu-id="86c67-484">Parâmetros de tipo podem ser usados como argumentos de tipo para outros tipos genéricos e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="86c67-484">Type parameters can be used as type arguments to other generic types and parameters.</span></span>

<span data-ttu-id="86c67-485">O exemplo a seguir é um tipo genérico que estende o `Stack(Of ItemType)` classe:</span><span class="sxs-lookup"><span data-stu-id="86c67-485">The following example is a generic type that extends the `Stack(Of ItemType)` class:</span></span>

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

<span data-ttu-id="86c67-486">Quando uma declaração fornece um argumento de tipo para `MyStack`, o mesmo argumento de tipo será aplicado a `Stack` também.</span><span class="sxs-lookup"><span data-stu-id="86c67-486">When a declaration supplies a type argument to `MyStack`, the same type argument will be applied to `Stack` as well.</span></span>

<span data-ttu-id="86c67-487">Como um tipo, parâmetros de tipo são puramente um constructo de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86c67-487">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="86c67-488">Em tempo de execução, cada parâmetro de tipo é associado a um tipo de tempo de execução que foi especificado, fornecendo um argumento de tipo para a declaração genérica.</span><span class="sxs-lookup"><span data-stu-id="86c67-488">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic declaration.</span></span> <span data-ttu-id="86c67-489">Portanto, o tipo de uma variável declarada com um parâmetro de tipo, em tempo de execução, será um tipo não genérico ou um tipo construído específico.</span><span class="sxs-lookup"><span data-stu-id="86c67-489">Thus, the type of a variable declared with a type parameter will, at run-time, be a non-generic type or a specific constructed type.</span></span> <span data-ttu-id="86c67-490">O tempo de execução de todas as instruções e expressões que envolvem parâmetros de tipo usa o tipo real que foi fornecido como o argumento de tipo para esse parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86c67-490">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>


### <a name="type-constraints"></a><span data-ttu-id="86c67-491">Restrições de tipo</span><span class="sxs-lookup"><span data-stu-id="86c67-491">Type Constraints</span></span>

<span data-ttu-id="86c67-492">Como um argumento de tipo pode ser qualquer tipo no sistema de tipos, um tipo genérico ou método não é possível fazer suposições sobre um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-492">Because a type argument can be any type in the type system, a generic type or method cannot make any assumptions about a type parameter.</span></span> <span data-ttu-id="86c67-493">Portanto, os membros de um parâmetro de tipo são considerados como os membros do tipo `Object`, uma vez que todos os tipos derivam `Object`.</span><span class="sxs-lookup"><span data-stu-id="86c67-493">Thus, the members of a type parameter are considered to be the members of the type `Object`, since all types derive from `Object`.</span></span>

<span data-ttu-id="86c67-494">No caso de uma coleção, como `Stack(Of ItemType)`, esse fato talvez não seja uma restrição importante, mas pode haver casos em que um tipo genérico pode querer fazer uma suposição sobre os tipos que serão fornecidos como argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-494">In the case of a collection like `Stack(Of ItemType)`, this fact may not be a particularly important restriction, but there may be cases where a generic type may wish to make an assumption about the types that will be supplied as type arguments.</span></span> <span data-ttu-id="86c67-495">*Restrições de tipo* podem ser colocados em parâmetros de tipo que restringirem quais tipos podem ser fornecidos como um parâmetro de tipo e permitir que tipos ou métodos genéricos assumir mais informações sobre parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-495">*Type constraints* can be placed on type parameters that restrict which types can be supplied as a type parameter and allow generic types or methods to assume more about type parameters.</span></span>

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

<span data-ttu-id="86c67-496">Neste exemplo, o `DisposableStack(Of ItemType)` restringe seu parâmetro de tipo para somente os tipos que implementam a interface `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="86c67-496">In this example, the `DisposableStack(Of ItemType)` constrains its type parameter to only types that implement the interface `System.IDisposable`.</span></span> <span data-ttu-id="86c67-497">Como resultado, ela pode implementar um `Dispose` método que descarta todos os objetos ainda deixada na fila.</span><span class="sxs-lookup"><span data-stu-id="86c67-497">As a result, it can implement a `Dispose` method that disposes any objects still left in the queue.</span></span>

<span data-ttu-id="86c67-498">Uma restrição de tipo deve ser uma das restrições especiais `Class`, `Structure`, ou `New`, ou deve ser um tipo `T` onde:</span><span class="sxs-lookup"><span data-stu-id="86c67-498">A type constraint must be one of the special constraints `Class`, `Structure`, or `New`, or it must be a type `T` where:</span></span>

* <span data-ttu-id="86c67-499">`T` é uma classe, uma interface ou um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-499">`T` is a class, an interface, or a type parameter.</span></span>

* <span data-ttu-id="86c67-500">`T` não é `NotInheritable`.</span><span class="sxs-lookup"><span data-stu-id="86c67-500">`T` is not `NotInheritable`.</span></span>

* <span data-ttu-id="86c67-501">`T` não é um dos ou um tipo herdado de um dos seguintes tipos especiais: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, ou `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="86c67-501">`T` is not one of, or a type inherited from one of, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span>

* <span data-ttu-id="86c67-502">`T` não é `Object`.</span><span class="sxs-lookup"><span data-stu-id="86c67-502">`T` is not `Object`.</span></span> <span data-ttu-id="86c67-503">Uma vez que todos os tipos derivam `Object`, tal restrição não terá efeito algum se ele eram permitido.</span><span class="sxs-lookup"><span data-stu-id="86c67-503">Since all types derive from `Object`, such a constraint would have no effect if it were permitted.</span></span>

* <span data-ttu-id="86c67-504">`T` deve ser pelo menos tão acessíveis quanto o tipo genérico ou método que está sendo declarado.</span><span class="sxs-lookup"><span data-stu-id="86c67-504">`T` must be at least as accessible as the generic type or method being declared.</span></span>

<span data-ttu-id="86c67-505">Várias restrições de tipo podem ser especificadas para um único parâmetro de tipo, colocando as restrições de tipo entre chaves (`{}`)...</span><span class="sxs-lookup"><span data-stu-id="86c67-505">Multiple type constraints can be specified for a single type parameter by enclosing the type constraints in curly braces (`{}`)..</span></span> <span data-ttu-id="86c67-506">Restrição de apenas um tipo para um parâmetro de tipo determinado pode ser uma classe.</span><span class="sxs-lookup"><span data-stu-id="86c67-506">Only one type constraint for a given type parameter can be a class.</span></span> <span data-ttu-id="86c67-507">É um erro para combinar uma `Structure` restrição especial com uma restrição de classe nomeada ou a `Class` restrição especial.</span><span class="sxs-lookup"><span data-stu-id="86c67-507">It is an error to combine a `Structure` special constraint with a named class constraint or the `Class` special constraint.</span></span>

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

<span data-ttu-id="86c67-508">Restrições de tipo podem usar os tipos de recipientes ou qualquer um dos parâmetros de tipo dos tipos recipiente.</span><span class="sxs-lookup"><span data-stu-id="86c67-508">Type constraints can use the containing types or any of the containing types' type parameters.</span></span> <span data-ttu-id="86c67-509">No exemplo a seguir, a restrição requer que o argumento de tipo fornecido implementa uma interface genérica que usa a mesmo como um argumento de tipo:</span><span class="sxs-lookup"><span data-stu-id="86c67-509">In the following example, the constraint requires that the type argument supplied implements a generic interface using itself as a type argument:</span></span>

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

<span data-ttu-id="86c67-510">A restrição de tipo especial `Class` restringe o argumento de tipo fornecido para qualquer tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="86c67-510">The special type constraint `Class` constrains the supplied type argument to any reference type.</span></span>

<span data-ttu-id="86c67-511">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="86c67-511">__Note.__</span></span> <span data-ttu-id="86c67-512">A restrição de tipo especial `Class` podem ser atendidas por uma interface.</span><span class="sxs-lookup"><span data-stu-id="86c67-512">The special type constraint `Class` can be satisfied by an interface.</span></span> <span data-ttu-id="86c67-513">E uma estrutura pode implementar uma interface.</span><span class="sxs-lookup"><span data-stu-id="86c67-513">And a structure can implement an interface.</span></span> <span data-ttu-id="86c67-514">Portanto, a restrição `(Of T As U, U As Class)` pode ser satisfeito com uma estrutura de "T" (que não satisfaz a `Class` restrição especial) e "U" uma interface que ele implementa (que atende a `Class` restrição especial).</span><span class="sxs-lookup"><span data-stu-id="86c67-514">Therefore, the constraint `(Of T As U, U As Class)` might be satisfied with "T" a structure (which does not satisfy the `Class` special constraint), and "U" an interface that it implements (which does satisfy the `Class` special constraint).</span></span>

<span data-ttu-id="86c67-515">A restrição de tipo especial `Structure` restringe o argumento de tipo fornecido para qualquer tipo de valor, exceto `System.Nullable(Of T)`.</span><span class="sxs-lookup"><span data-stu-id="86c67-515">The special type constraint `Structure` constrains the supplied type argument to any value type except `System.Nullable(Of T)`.</span></span>

<span data-ttu-id="86c67-516">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="86c67-516">__Note.__</span></span> <span data-ttu-id="86c67-517">Restrições de estrutura não permitem `System.Nullable(Of T)` para que ele não é possível fornecer `System.Nullable(Of T)` como um argumento de tipo a mesmo.</span><span class="sxs-lookup"><span data-stu-id="86c67-517">Structure constraints do not allow `System.Nullable(Of T)` so that it is not possible to supply `System.Nullable(Of T)` as a type argument to itself.</span></span>

<span data-ttu-id="86c67-518">A restrição de tipo especial `New` requer que o argumento de tipo fornecido deve ter um construtor sem parâmetros acessível e não pode ser declarado `MustInherit`.</span><span class="sxs-lookup"><span data-stu-id="86c67-518">The special type constraint `New` requires that the supplied type argument must have an accessible parameterless constructor and cannot be declared `MustInherit`.</span></span> <span data-ttu-id="86c67-519">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-519">For example:</span></span>

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

<span data-ttu-id="86c67-520">Uma restrição de tipo de classe requer que o argumento de tipo fornecido deve ser que digitar como ou herdar dele.</span><span class="sxs-lookup"><span data-stu-id="86c67-520">A class type constraint requires that the supplied type argument must either be that type as or inherit from it.</span></span> <span data-ttu-id="86c67-521">Uma restrição de tipo de interface requer que o argumento de tipo fornecido deve implementar essa interface.</span><span class="sxs-lookup"><span data-stu-id="86c67-521">An interface type constraint requires that the supplied type argument must implement that interface.</span></span> <span data-ttu-id="86c67-522">Uma restrição de parâmetro de tipo exige que o argumento de tipo fornecido deve derivar de ou implementar todos os limites fornecidos para o parâmetro de tipo correspondente.</span><span class="sxs-lookup"><span data-stu-id="86c67-522">A type parameter constraint requires that the supplied type argument must derive from or implement all of the bounds given for the matching type parameter.</span></span> <span data-ttu-id="86c67-523">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-523">For example:</span></span>

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

<span data-ttu-id="86c67-524">Neste exemplo, o parâmetro de tipo `S` na `AddRange` é restrito para o parâmetro de tipo `T` de `List`.</span><span class="sxs-lookup"><span data-stu-id="86c67-524">In this example, the type parameter `S` on `AddRange` is constrained to the type parameter `T` of `List`.</span></span> <span data-ttu-id="86c67-525">Isso significa que um `List(Of Control)` seria restringir `AddRange`do parâmetro para qualquer tipo que herda de ou que é do tipo `Control`.</span><span class="sxs-lookup"><span data-stu-id="86c67-525">This means that a `List(Of Control)` would constrain `AddRange`'s type parameter to any type that is or inherits from `Control`.</span></span>

<span data-ttu-id="86c67-526">Uma restrição de parâmetro de tipo `Of S As T` é resolvido adicionando transitivamente todas `T`do restrições em `S`, em vez das restrições especiais (`Class`, `Structure`, `New`).</span><span class="sxs-lookup"><span data-stu-id="86c67-526">A type parameter constraint `Of S As T` is resolved by transitively adding all of `T`'s constraints onto `S`, other than the special constraints (`Class`, `Structure`, `New`).</span></span> <span data-ttu-id="86c67-527">É um erro ter restrições circulares (por exemplo, `Of S As T, T As S`).</span><span class="sxs-lookup"><span data-stu-id="86c67-527">It is an error to have circular constraints (e.g. `Of S As T, T As S`).</span></span> <span data-ttu-id="86c67-528">É um erro ter uma restrição de parâmetro de tipo que por si só tem o `Structure` restrição.</span><span class="sxs-lookup"><span data-stu-id="86c67-528">It is an error to have a type parameter constraint which itself has the `Structure` constraint.</span></span> <span data-ttu-id="86c67-529">Depois de adicionar restrições, é possível que um número de situações especiais pode ocorrer:</span><span class="sxs-lookup"><span data-stu-id="86c67-529">After adding constraints, it is possible that a number of special situations may occur:</span></span>

* <span data-ttu-id="86c67-530">Se houver várias restrições de classe, a classe mais derivada é considerada a restrição.</span><span class="sxs-lookup"><span data-stu-id="86c67-530">If multiple class constraints exist, the most derived class is considered to be the constraint.</span></span> <span data-ttu-id="86c67-531">Se uma ou mais restrições de classe não têm nenhuma relação de herança, a restrição é insatisfatórios e é um erro.</span><span class="sxs-lookup"><span data-stu-id="86c67-531">If one or more class constraints have no inheritance relationship, the constraint is unsatisfiable and it is an error.</span></span>

 * <span data-ttu-id="86c67-532">Se um parâmetro de tipo combina uma `Structure` restrição especial com uma restrição de classe nomeada ou o `Class` restrição especial, ele é um erro.</span><span class="sxs-lookup"><span data-stu-id="86c67-532">If a type parameter combines a `Structure` special constraint with a named class constraint or the `Class` special constraint, it is an error.</span></span> <span data-ttu-id="86c67-533">Uma restrição de classe pode ser `NotInheritable`, caso em que nenhum tipo derivado de restrição é aceitos, e é um erro.</span><span class="sxs-lookup"><span data-stu-id="86c67-533">A class constraint may be `NotInheritable`, in which case no derived types of that constraint are accepted and it is an error.</span></span>

<span data-ttu-id="86c67-534">O tipo pode ser um dos ou um tipo herdadas, os seguintes tipos especiais: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, ou `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="86c67-534">The type may be one of, or a type inherited from, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="86c67-535">Nesse caso, apenas o tipo ou um tipo herdado dele, é aceita.</span><span class="sxs-lookup"><span data-stu-id="86c67-535">In that case, only the type, or a type inherited from it, is accepted.</span></span> <span data-ttu-id="86c67-536">Um parâmetro de tipo restringido a um desses tipos só pode usar as conversões permitidas pelo `DirectCast` operador.</span><span class="sxs-lookup"><span data-stu-id="86c67-536">A type parameter constrained to one of these types can only use the conversions allowed by the `DirectCast` operator.</span></span> <span data-ttu-id="86c67-537">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-537">For example:</span></span>

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

<span data-ttu-id="86c67-538">Além disso, um parâmetro de tipo restringido a um tipo de valor devido a um das reduções acima não é possível chamar quaisquer métodos definidos nesse tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="86c67-538">Additionally, a type parameter constrained to a value type due to one of the above relaxations cannot call any methods defined on that value type.</span></span> <span data-ttu-id="86c67-539">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-539">For example:</span></span>

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

<span data-ttu-id="86c67-540">Se a restrição, depois de substituição, acaba como um tipo de matriz, qualquer tipo de matriz de covariante é permitido também.</span><span class="sxs-lookup"><span data-stu-id="86c67-540">If the constraint, after substitution, ends up as an array type, any covariant array type is allowed as well.</span></span> <span data-ttu-id="86c67-541">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-541">For example:</span></span>

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

<span data-ttu-id="86c67-542">Um parâmetro de tipo com uma restrição de classe ou interface é considerado para ter os mesmos membros que essa restrição de classe ou interface.</span><span class="sxs-lookup"><span data-stu-id="86c67-542">A type parameter with a class or interface constraint is considered to have the same members as that class or interface constraint.</span></span> <span data-ttu-id="86c67-543">Se um parâmetro de tipo tiver múltiplas restrições, o parâmetro de tipo é considerado para ter a união de todos os membros das restrições.</span><span class="sxs-lookup"><span data-stu-id="86c67-543">If a type parameter has multiple constraints, then the type parameter is considered to have the union of all the members of the constraints.</span></span> <span data-ttu-id="86c67-544">Se houver membros com o mesmo nome em mais de uma restrição, então os membros estão ocultos na seguinte ordem: a restrição de classe oculta membros em restrições de interface, que ocultar membros em `System.ValueType` (se `Structure` restrição for especificada), que oculta os membros em `Object`.</span><span class="sxs-lookup"><span data-stu-id="86c67-544">If there are members with the same name in more than one constraint, then members are hidden in the following order: the class constraint hides members in interface constraints, which hide members in `System.ValueType` (if `Structure` constraint is specified), which hides members in `Object`.</span></span> <span data-ttu-id="86c67-545">Se um membro com o mesmo nome for exibido em mais de uma restrição de interface, o membro não está disponível (como em várias heranças de interface) e o parâmetro de tipo deve ser convertido para a interface desejada.</span><span class="sxs-lookup"><span data-stu-id="86c67-545">If a member with the same name appears in more than one interface constraint the member is unavailable (as in multiple interface inheritance) and the type parameter must be cast to the desired interface.</span></span> <span data-ttu-id="86c67-546">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-546">For example:</span></span>

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

<span data-ttu-id="86c67-547">Ao fornecer parâmetros de tipo como argumentos de tipo, os parâmetros de tipo devem satisfazer as restrições dos parâmetros de tipo correspondente.</span><span class="sxs-lookup"><span data-stu-id="86c67-547">When supplying type parameters as type arguments, the type parameters must satisfy the constraints of the matching type parameters.</span></span>

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

<span data-ttu-id="86c67-548">Valores de um parâmetro de tipo restrita podem ser usados para acessar os membros de instância, incluindo métodos de instância, especificados na restrição.</span><span class="sxs-lookup"><span data-stu-id="86c67-548">Values of a constrained type parameter can be used to access the instance members, including instance methods, specified in the constraint.</span></span>

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

### <a name="type-parameter-variance"></a><span data-ttu-id="86c67-549">Variação de parâmetro de tipo</span><span class="sxs-lookup"><span data-stu-id="86c67-549">Type Parameter Variance</span></span>

<span data-ttu-id="86c67-550">Um parâmetro de tipo em uma interface ou uma declaração de tipo de delegado pode, opcionalmente, especificar uma *modificador de variação*.</span><span class="sxs-lookup"><span data-stu-id="86c67-550">A type parameter in an interface or a delegate type declaration can optionally specify a *variance modifier*.</span></span> <span data-ttu-id="86c67-551">Parâmetros de tipo com modificadores de variação restringem como o parâmetro de tipo pode ser usado no tipo de interface ou delegado, mas permitir que um tipo de interface ou delegado genérico a ser convertido em outro tipo genérico com argumentos de tipo compatível com variante.</span><span class="sxs-lookup"><span data-stu-id="86c67-551">Type parameters with variance modifiers restrict how the type parameter can be used in the interface or delegate type but allow a generic interface or delegate type to be converted to another generic type with variant compatible type arguments.</span></span> <span data-ttu-id="86c67-552">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-552">For example:</span></span>

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

<span data-ttu-id="86c67-553">Interfaces genéricas que têm parâmetros de tipo com modificadores de variação têm várias restrições:</span><span class="sxs-lookup"><span data-stu-id="86c67-553">Generic interfaces that have type parameters with variance modifiers have several restrictions:</span></span>

* <span data-ttu-id="86c67-554">Elas não podem conter uma declaração de evento que especifica uma lista de parâmetros (mas uma declaração de evento personalizado ou uma declaração de evento com um tipo delegado é permitida).</span><span class="sxs-lookup"><span data-stu-id="86c67-554">They cannot contain an event declaration that specifies a parameter list (but a custom event declaration or an event declaration with a delegate type is allowed).</span></span>

* <span data-ttu-id="86c67-555">Eles não podem conter uma classe aninhada, estrutura ou tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="86c67-555">They cannot contain a nested class, structure, or enumerated type.</span></span>

<span data-ttu-id="86c67-556">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="86c67-556">__Note.__</span></span> <span data-ttu-id="86c67-557">Essas restrições são devido ao fato de que os tipos aninhados em tipos genéricos implicitamente copiar os parâmetros genéricos de seu pai.</span><span class="sxs-lookup"><span data-stu-id="86c67-557">These restrictions are due to the fact that types nested in generic types implicitly copy the generic parameters of their parent.</span></span> <span data-ttu-id="86c67-558">No caso de tipos enumerados, estruturas ou classes aninhadas, esses tipos de tipos não podem ter modificadores de variação em seus parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-558">In the case of nested classes, structures, or enumerated types, those kinds of types cannot have variance modifiers on their type parameters.</span></span> <span data-ttu-id="86c67-559">No caso de uma declaração de evento com uma lista de parâmetros, a classe delegate aninhado gerado poderia ter confuso erros quando um tipo que aparece para ser usado em uma `In` posição (ou seja, um tipo de parâmetro), na verdade, é usada em um `Out` posição (ou seja, o tipo de evento).</span><span class="sxs-lookup"><span data-stu-id="86c67-559">In the case of an event declaration with a parameter list, the generated nested delegate class could have confusing errors when a type that appears to be used in an `In` position (i.e. a parameter type) is actually used in an `Out` position (i.e. the type of the event).</span></span>

<span data-ttu-id="86c67-560">É um parâmetro de tipo que é declarado com o modificador Out *covariante*.</span><span class="sxs-lookup"><span data-stu-id="86c67-560">A type parameter that is declared with the Out modifier is *covariant*.</span></span> <span data-ttu-id="86c67-561">Informalmente, um parâmetro de tipo covariante só pode ser usado em uma posição de saída – ou seja, um valor que está sendo retornado do tipo de interface ou delegado – e não pode ser usado em uma posição de entrada.</span><span class="sxs-lookup"><span data-stu-id="86c67-561">Informally, a covariant type parameter can only be used in an output position -- i.e. a value that is being returned from the interface or delegate type -- and cannot be used in an input position.</span></span> <span data-ttu-id="86c67-562">Um tipo `T` é considerado *válido covariantly* se:</span><span class="sxs-lookup"><span data-stu-id="86c67-562">A type `T` is considered to be *valid covariantly* if:</span></span>

* <span data-ttu-id="86c67-563">`T` é uma classe, estrutura ou tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="86c67-563">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="86c67-564">`T` é o tipo de delegado ou interface não genérica.</span><span class="sxs-lookup"><span data-stu-id="86c67-564">`T` is non-generic delegate or interface type.</span></span>

* <span data-ttu-id="86c67-565">`T` é um tipo de matriz cujo tipo de elemento covariantly é válido.</span><span class="sxs-lookup"><span data-stu-id="86c67-565">`T` is an array type whose element type is valid covariantly.</span></span>

* <span data-ttu-id="86c67-566">`T` é um parâmetro de tipo que não foi declarado como um `Out` parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-566">`T` is a type parameter which was not declared as an `Out` type parameter.</span></span>

* <span data-ttu-id="86c67-567">`T` é um tipo de interface ou delegado construído `X(Of P1,...,Pn)` com argumentos de tipo `A1,...,An` , de modo que:</span><span class="sxs-lookup"><span data-stu-id="86c67-567">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="86c67-568">Se `Pi` foi declarado como um parâmetro de tipo Out, em seguida, `Ai` é covariantly válido.</span><span class="sxs-lookup"><span data-stu-id="86c67-568">If `Pi` was declared as an Out type parameter then `Ai` is valid covariantly.</span></span>

  * <span data-ttu-id="86c67-569">Se `Pi` foi declarado como um parâmetro de tipo, em seguida, `Ai` é contravariantly válido.</span><span class="sxs-lookup"><span data-stu-id="86c67-569">If `Pi` was declared as an In type parameter then `Ai` is valid contravariantly.</span></span>

<span data-ttu-id="86c67-570">O exemplo a seguir deve ser válidos covariantly em um tipo de interface ou delegado:</span><span class="sxs-lookup"><span data-stu-id="86c67-570">The following must be valid covariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="86c67-571">A interface base de uma interface.</span><span class="sxs-lookup"><span data-stu-id="86c67-571">The base interface of an interface.</span></span>

* <span data-ttu-id="86c67-572">O tipo de retorno de uma função ou o tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="86c67-572">The return type of a function or the delegate type.</span></span>

* <span data-ttu-id="86c67-573">O tipo de uma propriedade, se houver um `Get` acessador.</span><span class="sxs-lookup"><span data-stu-id="86c67-573">The type of a property if there is a `Get` accessor.</span></span>

* <span data-ttu-id="86c67-574">O tipo de qualquer `ByRef` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86c67-574">The type of any `ByRef` parameter.</span></span>

<span data-ttu-id="86c67-575">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-575">For example:</span></span>

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

<span data-ttu-id="86c67-576">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="86c67-576">__Note.__</span></span> <span data-ttu-id="86c67-577">`Out` não é uma palavra reservada.</span><span class="sxs-lookup"><span data-stu-id="86c67-577">`Out` is not a reserved word.</span></span>

<span data-ttu-id="86c67-578">É um parâmetro de tipo que é declarado com o modificador In *contravariante*.</span><span class="sxs-lookup"><span data-stu-id="86c67-578">A type parameter that is declared with the In modifier is *contravariant*.</span></span> <span data-ttu-id="86c67-579">Informalmente, um parâmetro de tipo contravariante só pode ser usado em uma posição de entrada – ou seja, um valor que está sendo passado para o tipo de interface ou delegado – e não pode ser usado em uma posição de saída.</span><span class="sxs-lookup"><span data-stu-id="86c67-579">Informally, a contravariant type parameter can only be used in an input position -- i.e. a value that is being passed in to the interface or delegate type -- and cannot be used in an output position.</span></span> <span data-ttu-id="86c67-580">Um tipo `T` é considerado *contravariantly válido* se:</span><span class="sxs-lookup"><span data-stu-id="86c67-580">A type `T` is considered to be *valid contravariantly* if:</span></span>

* <span data-ttu-id="86c67-581">`T` é uma classe, estrutura ou tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="86c67-581">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="86c67-582">`T` é um tipo de delegado ou interface não genérica.</span><span class="sxs-lookup"><span data-stu-id="86c67-582">`T` is a non-generic delegate or interface type.</span></span>

* <span data-ttu-id="86c67-583">`T` é um tipo de matriz cujo tipo de elemento é contravariantly válido.</span><span class="sxs-lookup"><span data-stu-id="86c67-583">`T` is an array type whose element type is valid contravariantly.</span></span>

* <span data-ttu-id="86c67-584">`T` é um parâmetro de tipo que não foi declarado como um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86c67-584">`T` is a type parameter which was not declared as an In type parameter.</span></span>

* <span data-ttu-id="86c67-585">`T` é um tipo de interface ou delegado construído `X(Of P1,...,Pn)` com argumentos de tipo `A1,...,An` , de modo que:</span><span class="sxs-lookup"><span data-stu-id="86c67-585">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="86c67-586">Se `Pi` foi declarado como um `Out` , em seguida, o parâmetro de tipo `Ai` é contravariantly válido.</span><span class="sxs-lookup"><span data-stu-id="86c67-586">If `Pi` was declared as an `Out` type parameter then `Ai` is valid contravariantly.</span></span>

  * <span data-ttu-id="86c67-587">Se `Pi` foi declarado como um `In` , em seguida, o parâmetro de tipo `Ai` é covariantly válido.</span><span class="sxs-lookup"><span data-stu-id="86c67-587">If `Pi` was declared as an `In` type parameter then `Ai` is valid covariantly.</span></span>

<span data-ttu-id="86c67-588">A seguir devem ser contravariantly válido em um tipo de interface ou delegado:</span><span class="sxs-lookup"><span data-stu-id="86c67-588">The following must be valid contravariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="86c67-589">O tipo de um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86c67-589">The type of a parameter.</span></span>

* <span data-ttu-id="86c67-590">Uma restrição de tipo em um parâmetro de tipo de método.</span><span class="sxs-lookup"><span data-stu-id="86c67-590">A type constraint on a method type parameter.</span></span>

* <span data-ttu-id="86c67-591">O tipo de uma propriedade, se ele tiver um `Set` acessador.</span><span class="sxs-lookup"><span data-stu-id="86c67-591">The type of a property if it has a `Set` accessor.</span></span>

* <span data-ttu-id="86c67-592">O tipo de um evento.</span><span class="sxs-lookup"><span data-stu-id="86c67-592">The type of an event.</span></span>

<span data-ttu-id="86c67-593">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86c67-593">For example:</span></span>

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

<span data-ttu-id="86c67-594">No caso em que um tipo deve ser válido ser contravariantly e covariantly (como uma propriedade tanto com um `Get` e `Set` acessador ou um `ByRef` parâmetro), um parâmetro de tipo de variante não pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="86c67-594">In the case where a type must be valid be contravariantly and covariantly (such as a property with both a `Get` and `Set` accessor or a `ByRef` parameter), a variant type parameter cannot be used.</span></span>


<span data-ttu-id="86c67-595">Covariância e contravariância dar origem a um problema de ambiguidade"diamond".</span><span class="sxs-lookup"><span data-stu-id="86c67-595">Co- and contra-variance give rise to a "diamond ambiguity problem".</span></span> <span data-ttu-id="86c67-596">Considere o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="86c67-596">Consider the following code:</span></span>

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

<span data-ttu-id="86c67-597">A classe `C` pode ser convertido em `IEnumerable(Of Object)` de duas maneiras, por meio de conversão de covariante do `IEnumerable(Of String)` e por meio de conversão de covariante do `IEnumerable(Of Exception)`.</span><span class="sxs-lookup"><span data-stu-id="86c67-597">The class `C` can be converted to `IEnumerable(Of Object)` in two ways, both through covariant conversion from `IEnumerable(Of String)` and through covariant conversion from `IEnumerable(Of Exception)`.</span></span> <span data-ttu-id="86c67-598">O CLR não especifica qual dos dois métodos serão chamados pelo `c.GetEnumerator()`.</span><span class="sxs-lookup"><span data-stu-id="86c67-598">The CLR does not specify which of the two methods will be called by `c.GetEnumerator()`.</span></span> <span data-ttu-id="86c67-599">Em geral, sempre que uma classe é declarada para implementar uma interface covariante com dois argumentos genéricos diferentes que têm um supertipo comum (por exemplo, nesse caso `String` e `Exception` têm um supertipo comum `Object`), ou uma classe é declarada para implementar uma interface contravariante com dois argumentos genéricos diferentes que têm um subtipo comum, então a ambiguidade é surgirão.</span><span class="sxs-lookup"><span data-stu-id="86c67-599">In general, whenever a class is declared to implement a covariant interface with two different generic arguments that have a common supertype (e.g. in this case `String` and `Exception` have the common supertype `Object`), or a class is declared to implement a contravariant interface with two different generic arguments that have a common subtype, then ambiguity is likely to arise.</span></span> <span data-ttu-id="86c67-600">O compilador dá um aviso sobre tais declarações.</span><span class="sxs-lookup"><span data-stu-id="86c67-600">The compiler gives a warning on such declarations.</span></span>
