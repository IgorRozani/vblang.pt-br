# <a name="types"></a><span data-ttu-id="0eedd-101">Tipos</span><span class="sxs-lookup"><span data-stu-id="0eedd-101">Types</span></span>

<span data-ttu-id="0eedd-102">São as duas categorias fundamentais de tipos no Visual Basic *tipos de valor* e *tipos de referência*.</span><span class="sxs-lookup"><span data-stu-id="0eedd-102">The two fundamental categories of types in Visual Basic are *value types* and *reference types*.</span></span> <span data-ttu-id="0eedd-103">Estruturas, enumerações e tipos primitivos (exceto as cadeias de caracteres) são tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="0eedd-103">Primitive types (except strings), enumerations, and structures are value types.</span></span> <span data-ttu-id="0eedd-104">Cadeias de caracteres, classes, interfaces, módulos padrão, matrizes e delegados são tipos de referência.</span><span class="sxs-lookup"><span data-stu-id="0eedd-104">Classes, strings, standard modules, interfaces, arrays, and delegates are reference types.</span></span>

<span data-ttu-id="0eedd-105">Cada tipo tem um *valor padrão*, que é o valor atribuído a variáveis desse tipo na inicialização.</span><span class="sxs-lookup"><span data-stu-id="0eedd-105">Every type has a *default value*, which is the value that is assigned to variables of that type upon initialization.</span></span>

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

## <a name="value-types-and-reference-types"></a><span data-ttu-id="0eedd-106">Tipos de valor e referência</span><span class="sxs-lookup"><span data-stu-id="0eedd-106">Value Types and Reference Types</span></span>

<span data-ttu-id="0eedd-107">Embora os tipos de valor e tipos de referência podem ser semelhantes em termos de uso e a sintaxe de declaração, sua semântica é diferentes.</span><span class="sxs-lookup"><span data-stu-id="0eedd-107">Although value types and reference types can be similar in terms of declaration syntax and usage, their semantics are distinct.</span></span>

<span data-ttu-id="0eedd-108">Tipos de referência são armazenados no heap de tempo de execução; só pode ser acessados por meio de uma referência a esse armazenamento.</span><span class="sxs-lookup"><span data-stu-id="0eedd-108">Reference types are stored on the run-time heap; they may only be accessed through a reference to that storage.</span></span> <span data-ttu-id="0eedd-109">Como sempre, os tipos de referência são acessados por meio de referências, seu tempo de vida é gerenciado pelo .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0eedd-109">Because reference types are always accessed through references, their lifetime is managed by the .NET Framework.</span></span> <span data-ttu-id="0eedd-110">Referências pendentes de uma determinada instância são acompanhadas e a instância é destruída somente quando não há mais referências permanecem.</span><span class="sxs-lookup"><span data-stu-id="0eedd-110">Outstanding references to a particular instance are tracked and the instance is destroyed only when no more references remain.</span></span> <span data-ttu-id="0eedd-111">Uma variável do tipo de referência contém uma referência a um valor desse tipo, um valor de um tipo mais derivado ou um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-111">A variable of reference type contains a reference to a value of that type, a value of a more derived type, or a null value.</span></span> <span data-ttu-id="0eedd-112">Um *valor nulo* refere-se a nada; não é possível fazer alguma coisa com um valor nulo, exceto pelo fato atribuí-lo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-112">A *null value* refers to nothing; it is not possible to do anything with a null value except assign it.</span></span> <span data-ttu-id="0eedd-113">Atribuição para uma variável do tipo de referência cria uma cópia da referência em vez de uma cópia do valor referenciado.</span><span class="sxs-lookup"><span data-stu-id="0eedd-113">Assignment to a variable of a reference type creates a copy of the reference rather than a copy of the referenced value.</span></span> <span data-ttu-id="0eedd-114">Para uma variável do tipo de referência, o valor padrão é um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-114">For a variable of a reference type, the default value is a null value.</span></span>

<span data-ttu-id="0eedd-115">Tipos de valor são armazenados diretamente na pilha, dentro de uma matriz ou dentro de outro tipo; o armazenamento deles só pode ser acessado diretamente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-115">Value types are stored directly on the stack, either within an array or within another type; their storage can only be accessed directly.</span></span> <span data-ttu-id="0eedd-116">Como tipos de valor são armazenados diretamente em variáveis, seu tempo de vida é determinado pelo tempo de vida da variável que os contém.</span><span class="sxs-lookup"><span data-stu-id="0eedd-116">Because value types are stored directly within variables, their lifetime is determined by the lifetime of the variable that contains them.</span></span> <span data-ttu-id="0eedd-117">Quando o local que contém uma instância de tipo de valor é destruído, a instância do tipo de valor também é destruída.</span><span class="sxs-lookup"><span data-stu-id="0eedd-117">When the location containing a value type instance is destroyed, the value type instance is also destroyed.</span></span> <span data-ttu-id="0eedd-118">Tipos de valor sempre são acessados diretamente. não é possível criar uma referência a um tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="0eedd-118">Value types are always accessed directly; it is not possible to create a reference to a value type.</span></span> <span data-ttu-id="0eedd-119">Proibir tal referência torna impossível para se referir a uma instância da classe de valor que foi destruída.</span><span class="sxs-lookup"><span data-stu-id="0eedd-119">Prohibiting such a reference makes it impossible to refer to a value class instance that has been destroyed.</span></span> <span data-ttu-id="0eedd-120">Como tipos de valor são sempre `NotInheritable`, uma variável de um tipo de valor sempre contém um valor desse tipo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-120">Because value types are always `NotInheritable`, a variable of a value type always contains a value of that type.</span></span> <span data-ttu-id="0eedd-121">Por isso, o valor de um tipo de valor não pode ser um valor nulo, nem pode referência a um objeto de um tipo mais derivado.</span><span class="sxs-lookup"><span data-stu-id="0eedd-121">Because of this, the value of a value type cannot be a null value, nor can it reference an object of a more derived type.</span></span> <span data-ttu-id="0eedd-122">Atribuição para uma variável de um tipo de valor cria uma cópia do valor que está sendo atribuído.</span><span class="sxs-lookup"><span data-stu-id="0eedd-122">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="0eedd-123">Para uma variável de um tipo de valor, o valor padrão é o resultado da inicialização de cada membro de variável do tipo para seu valor padrão.</span><span class="sxs-lookup"><span data-stu-id="0eedd-123">For a variable of a value type, the default value is the result of initializing each variable member of the type to its default value.</span></span>

<span data-ttu-id="0eedd-124">O exemplo a seguir mostra a diferença entre tipos de referência e tipos de valor:</span><span class="sxs-lookup"><span data-stu-id="0eedd-124">The following example shows the difference between reference types and value types:</span></span>

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

<span data-ttu-id="0eedd-125">A saída do programa é:</span><span class="sxs-lookup"><span data-stu-id="0eedd-125">The output of the program is:</span></span>

```
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="0eedd-126">A atribuição à variável local `val2` não afeta a variável local `val1` porque ambas as variáveis locais são de um tipo de valor (o tipo `Integer`) e cada variável local de um tipo de valor tem seu próprio armazenamento.</span><span class="sxs-lookup"><span data-stu-id="0eedd-126">The assignment to the local variable `val2` does not impact the local variable `val1` because both local variables are of a value type (the type `Integer`) and each local variable of a value type has its own storage.</span></span> <span data-ttu-id="0eedd-127">Por outro lado, a atribuição `ref2.Value = 123;` afeta o objeto que ambos `ref1` e `ref2` referência.</span><span class="sxs-lookup"><span data-stu-id="0eedd-127">In contrast, the assignment `ref2.Value = 123;` affects the object that both `ref1` and `ref2` reference.</span></span>

<span data-ttu-id="0eedd-128">Uma coisa a observar sobre o sistema de tipos do .NET Framework é que, embora as estruturas, enumerações e tipos primitivos (exceto para `String`) são tipos de valor, todos eles herdam de tipos de referência.</span><span class="sxs-lookup"><span data-stu-id="0eedd-128">One thing to note about the .NET Framework type system is that even though structures, enumerations and primitive types (except for `String`) are value types, they all inherit from reference types.</span></span> <span data-ttu-id="0eedd-129">Estruturas e os tipos primitivos herdam do tipo de referência `System.ValueType`, que herda de `Object`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-129">Structures and the primitive types inherit from the reference type `System.ValueType`, which inherits from `Object`.</span></span> <span data-ttu-id="0eedd-130">Tipos enumerados herdam do tipo de referência `System.Enum`, que herda de `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-130">Enumerated types inherit from the reference type `System.Enum`, which inherits from `System.ValueType`.</span></span>

### <a name="nullable-value-types"></a><span data-ttu-id="0eedd-131">Tipos de valor que permitem valor nulo</span><span class="sxs-lookup"><span data-stu-id="0eedd-131">Nullable Value Types</span></span>

<span data-ttu-id="0eedd-132">Para tipos de valor, uma `?` modificador pode ser adicionado a um nome de tipo para representar os *anulável* versão desse tipo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-132">For value types, a `?` modifier can be added to a type name to represent the *nullable* version of that type.</span></span>

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

<span data-ttu-id="0eedd-133">Um tipo de valor anulável pode conter os mesmos valores que a versão não anulável do tipo, bem como o valor nulo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-133">A nullable value type can contain the same values as the non-nullable version of the type as well as the null value.</span></span> <span data-ttu-id="0eedd-134">Assim, para um tipo de valor anulável, atribuindo `Nothing` a uma variável do tipo define o valor da variável como o valor nulo, não o valor zero de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="0eedd-134">Thus, for a nullable value type, assigning `Nothing` to a variable of the type sets the value of the variable to the null value, not the zero value of the value type.</span></span> <span data-ttu-id="0eedd-135">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0eedd-135">For example:</span></span>

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

<span data-ttu-id="0eedd-136">Também é possível declarar uma variável para ser de um tipo de valor anulável, colocando um modificador de tipo que permite valor nulo no nome da variável.</span><span class="sxs-lookup"><span data-stu-id="0eedd-136">A variable may also be declared to be of a nullable value type by putting a nullable type modifier on the variable name.</span></span> <span data-ttu-id="0eedd-137">Para maior clareza, não é válido ter um modificador de tipo que permite valor nulo em um nome de variável e um nome de tipo na mesma declaração.</span><span class="sxs-lookup"><span data-stu-id="0eedd-137">For clarity, it is not valid to have a nullable type modifier on both a variable name and a type name in the same declaration.</span></span> <span data-ttu-id="0eedd-138">Como tipos anuláveis são implementados usando o tipo `System.Nullable(Of T)`, o tipo `T?` é sinônimo de tipo `System.Nullable(Of T)`, e os dois nomes podem ser usados alternadamente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-138">Since nullable types are implemented using the type `System.Nullable(Of T)`, the type `T?` is synonymous to the type `System.Nullable(Of T)`, and the two names can be used interchangeably.</span></span> <span data-ttu-id="0eedd-139">O `?` modificador não pode ser colocado em um tipo que já é anulável; portanto, não é possível declarar o tipo `Integer??` ou `System.Nullable(Of Integer)?`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-139">The `?` modifier cannot be placed on a type that is already nullable; thus, it is not possible to declare the type `Integer??` or `System.Nullable(Of Integer)?`.</span></span>

<span data-ttu-id="0eedd-140">Um tipo de valor anulável `T?` tem os membros de `System.Nullable(Of T)` , bem como operadores ou conversões *eliminada* do tipo subjacente `T` no tipo `T?`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-140">A nullable value type `T?` has the members of `System.Nullable(Of T)` as well as any operators or conversions *lifted* from the underlying type `T` into the type `T?`.</span></span> <span data-ttu-id="0eedd-141">Levantando cópias operadores e conversões de tipo subjacente, na maioria dos casos, substituindo os tipos que permitem valor nulo para tipos de valor não anulável.</span><span class="sxs-lookup"><span data-stu-id="0eedd-141">Lifting copies operators and conversions from the underlying type, in most cases substituting nullable value types for non-nullable value types.</span></span> <span data-ttu-id="0eedd-142">Isso permite que muitas das conversões e operações que se aplicam ao mesmo `T` para aplicar a `T?` também.</span><span class="sxs-lookup"><span data-stu-id="0eedd-142">This allows many of the same conversions and operations that apply to `T` to apply to `T?` as well.</span></span>


## <a name="interface-implementation"></a><span data-ttu-id="0eedd-143">Implementação de interface</span><span class="sxs-lookup"><span data-stu-id="0eedd-143">Interface Implementation</span></span>

<span data-ttu-id="0eedd-144">Estrutura e declarações de classe podem declarar que eles implementam um conjunto de tipos de interface por meio de um ou mais `Implements` cláusulas.</span><span class="sxs-lookup"><span data-stu-id="0eedd-144">Structure and class declarations may declare that they implement a set of interface types through one or more `Implements` clauses.</span></span>

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="0eedd-145">Todos os tipos especificados no `Implements` cláusula deve ser interfaces e o tipo deve implementar todos os membros das interfaces.</span><span class="sxs-lookup"><span data-stu-id="0eedd-145">All the types specified in the `Implements` clause must be interfaces, and the type must implement all members of the interfaces.</span></span> <span data-ttu-id="0eedd-146">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0eedd-146">For example:</span></span>

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

<span data-ttu-id="0eedd-147">Um tipo que implementa uma interface implicitamente também implementa todas as interfaces de base da interface.</span><span class="sxs-lookup"><span data-stu-id="0eedd-147">A type that implements an interface also implicitly implements all of the interface's base interfaces.</span></span> <span data-ttu-id="0eedd-148">Isso é verdadeiro mesmo se o tipo não lista base de todas as interfaces em explicitamente o `Implements` cláusula.</span><span class="sxs-lookup"><span data-stu-id="0eedd-148">This is true even if the type does not explicitly list all base interfaces in the `Implements` clause.</span></span> <span data-ttu-id="0eedd-149">Neste exemplo, o `TextBox` estrutura implementa ambos `IControl` e `ITextBox`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-149">In this example, the `TextBox` structure implements both `IControl` and `ITextBox`.</span></span>

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

<span data-ttu-id="0eedd-150">Declarar que um tipo implementa uma interface por si só não declara que qualquer coisa no espaço de declaração do tipo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-150">Declaring that a type implements an interface in and of itself does not declare anything in the declaration space of the type.</span></span> <span data-ttu-id="0eedd-151">Portanto, é válido para implementar duas interfaces com um método com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="0eedd-151">Thus, it is valid to implement two interfaces with a method by the same name.</span></span>

<span data-ttu-id="0eedd-152">Tipos não é possível implementar um parâmetro de tipo por conta própria, embora ela pode envolver os parâmetros de tipo que estão no escopo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-152">Types cannot implement a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

<span data-ttu-id="0eedd-153">Interfaces genéricas podem ser implementada várias vezes usando argumentos de tipo diferente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-153">Generic interfaces can be implemented multiple times using different type arguments.</span></span> <span data-ttu-id="0eedd-154">No entanto, um tipo genérico não pode implementar uma interface genérica usando um parâmetro de tipo, se o parâmetro de tipo fornecido (independentemente de restrições de tipo) pode sobrepor outra implementação dessa interface.</span><span class="sxs-lookup"><span data-stu-id="0eedd-154">However, a generic type cannot implement a generic interface using a type parameter if the supplied type parameter (regardless of type constraints) could overlap with another implementation of that interface.</span></span> <span data-ttu-id="0eedd-155">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0eedd-155">For example:</span></span>

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


## <a name="primitive-types"></a><span data-ttu-id="0eedd-156">Tipos primitivos</span><span class="sxs-lookup"><span data-stu-id="0eedd-156">Primitive Types</span></span>

<span data-ttu-id="0eedd-157">O *tipos primitivos* são identificadas por meio de palavras-chave, que são aliases para predefinidos tipos no `System` namespace.</span><span class="sxs-lookup"><span data-stu-id="0eedd-157">The *primitive types* are identified through keywords, which are aliases for predefined types in the `System` namespace.</span></span> <span data-ttu-id="0eedd-158">Um tipo primitivo é completamente indistinguível do tipo ele aliases: escrever a palavra reservada `Byte` é exatamente o mesmo que escrever `System.Byte`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-158">A primitive type is completely indistinguishable from the type it aliases: writing the reserved word `Byte` is exactly the same as writing `System.Byte`.</span></span> <span data-ttu-id="0eedd-159">Tipos primitivos são também conhecidos como *tipos intrínsecos*.</span><span class="sxs-lookup"><span data-stu-id="0eedd-159">Primitive types are also known as *intrinsic types*.</span></span>

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


<span data-ttu-id="0eedd-160">Porque um primitivo tipo um tipo regular de aliases, todos os tipos primitivos tem membros.</span><span class="sxs-lookup"><span data-stu-id="0eedd-160">Because a primitive type aliases a regular type, every primitive type has members.</span></span> <span data-ttu-id="0eedd-161">Por exemplo, `Integer` tem os membros declarados na `System.Int32`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-161">For example, `Integer` has the members declared in `System.Int32`.</span></span> <span data-ttu-id="0eedd-162">Literais podem ser tratadas como instâncias de seus tipos correspondentes.</span><span class="sxs-lookup"><span data-stu-id="0eedd-162">Literals can be treated as instances of their corresponding types.</span></span>

* <span data-ttu-id="0eedd-163">Os tipos primitivos diferem de outros tipos de estrutura em que eles permitem que determinadas operações adicionais:</span><span class="sxs-lookup"><span data-stu-id="0eedd-163">The primitive types differ from other structure types in that they permit certain additional operations:</span></span>

* <span data-ttu-id="0eedd-164">Tipos primitivos permitem valores a ser criado ao escrever literais.</span><span class="sxs-lookup"><span data-stu-id="0eedd-164">Primitive types permit values to be created by writing literals.</span></span> <span data-ttu-id="0eedd-165">Por exemplo, `123I` é um literal do tipo `Integer`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-165">For example, `123I` is a literal of type `Integer`.</span></span>

* <span data-ttu-id="0eedd-166">É possível declarar constantes dos tipos primitivos.</span><span class="sxs-lookup"><span data-stu-id="0eedd-166">It is possible to declare constants of the primitive types.</span></span>

* <span data-ttu-id="0eedd-167">Quando os operandos de uma expressão forem todas as constantes de tipo primitivo, é possível que o compilador avaliar a expressão em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0eedd-167">When the operands of an expression are all primitive type constants, it is possible for the compiler to evaluate the expression at compile time.</span></span> <span data-ttu-id="0eedd-168">Essa expressão é conhecida como uma expressão constante.</span><span class="sxs-lookup"><span data-stu-id="0eedd-168">Such an expression is known as a constant expression.</span></span>

<span data-ttu-id="0eedd-169">Visual Basic define os seguintes tipos primitivos:</span><span class="sxs-lookup"><span data-stu-id="0eedd-169">Visual Basic defines the following primitive types:</span></span>

* <span data-ttu-id="0eedd-170">Os tipos de valor integral `Byte` (inteiro sem sinal de 1 byte), `SByte` (inteiro com sinal de 1 byte), `UShort` (inteiro sem sinal de 2 bytes), `Short` (inteiro com sinal de 2 bytes), `UInteger` (inteiro sem sinal de 4 bytes), `Integer` ( inteiro com sinal de 4 bytes), `ULong` (inteiro sem sinal de 8 bytes), e `Long` (inteiro com sinal de 8 bytes).</span><span class="sxs-lookup"><span data-stu-id="0eedd-170">The integral value types `Byte` (1-byte unsigned integer), `SByte` (1-byte signed integer), `UShort` (2-byte unsigned integer), `Short` (2-byte signed integer), `UInteger` (4-byte unsigned integer), `Integer` (4-byte signed integer), `ULong` (8-byte unsigned integer), and `Long` (8-byte signed integer).</span></span> <span data-ttu-id="0eedd-171">Esses tipos são mapeados para `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` e `System.Int64`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-171">These types map to `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` and `System.Int64`, respectively.</span></span> <span data-ttu-id="0eedd-172">O valor padrão de um tipo integral é equivalente a literal `0`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-172">The default value of an integral type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="0eedd-173">Os tipos de valor de ponto flutuante `Single` (ponto flutuante de 4 bytes) e `Double` (ponto flutuante de 8 bytes).</span><span class="sxs-lookup"><span data-stu-id="0eedd-173">The floating-point value types `Single` (4-byte floating point) and `Double` (8-byte floating point).</span></span> <span data-ttu-id="0eedd-174">Esses tipos são mapeados para `System.Single` e `System.Double`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-174">These types map to `System.Single` and `System.Double`, respectively.</span></span> <span data-ttu-id="0eedd-175">O valor padrão de um tipo de ponto flutuante é equivalente a literal `0`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-175">The default value of a floating-point type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="0eedd-176">O `Decimal` tipo (valor decimal de 16 bytes), que mapeia para `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-176">The `Decimal` type (16-byte decimal value), which maps to `System.Decimal`.</span></span> <span data-ttu-id="0eedd-177">O valor padrão de decimal é equivalente a literal `0D`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-177">The default value of decimal is equivalent to the literal `0D`.</span></span>

* <span data-ttu-id="0eedd-178">O `Boolean` tipo, que representa um valor de verdade, normalmente o resultado de uma operação lógica ou relacional de valor.</span><span class="sxs-lookup"><span data-stu-id="0eedd-178">The `Boolean` value type, which represents a truth value, typically the result of a relational or logical operation.</span></span> <span data-ttu-id="0eedd-179">O literal é do tipo `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-179">The literal is of type `System.Boolean`.</span></span> <span data-ttu-id="0eedd-180">O valor padrão de `Boolean` o tipo é equivalente a literal `False`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-180">The default value of the `Boolean` type is equivalent to the literal `False`.</span></span>

* <span data-ttu-id="0eedd-181">O `Date` valor de tipo, que representa uma data e/ou em uma hora e é mapeado para `System.DateTime`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-181">The `Date` value type, which represents a date and/or a time and maps to `System.DateTime`.</span></span> <span data-ttu-id="0eedd-182">O valor padrão de `Date` o tipo é equivalente a literal `# 01/01/0001 12:00:00AM #`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-182">The default value of the `Date` type is equivalent to the literal `# 01/01/0001 12:00:00AM #`.</span></span>

* <span data-ttu-id="0eedd-183">O `Char` valor de tipo, que representa um único caractere Unicode e é mapeado para `System.Char`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-183">The `Char` value type, which represents a single Unicode character and maps to `System.Char`.</span></span> <span data-ttu-id="0eedd-184">O valor padrão de `Char` o tipo é equivalente à expressão de constante `ChrW(0)`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-184">The default value of the `Char` type is equivalent to the constant expression `ChrW(0)`.</span></span>

* <span data-ttu-id="0eedd-185">O `String` tipo de referência, que representa uma sequência de caracteres Unicode e é mapeado para `System.String`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-185">The `String` reference type, which represents a sequence of Unicode characters and maps to `System.String`.</span></span> <span data-ttu-id="0eedd-186">O valor padrão de `String` tipo é um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-186">The default value of the `String` type is a null value.</span></span>



## <a name="enumerations"></a><span data-ttu-id="0eedd-187">Enumerações</span><span class="sxs-lookup"><span data-stu-id="0eedd-187">Enumerations</span></span>

<span data-ttu-id="0eedd-188">*Enumerações* são tipos de valor que herdam de `System.Enum` e simbolicamente representam um conjunto de valores de um dos tipos integrais primitivos.</span><span class="sxs-lookup"><span data-stu-id="0eedd-188">*Enumerations* are value types that inherit from `System.Enum` and symbolically represent a set of values of one of the primitive integral types.</span></span>

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

<span data-ttu-id="0eedd-189">Para um tipo de enumeração `E`, o valor padrão é o valor produzido pela expressão `CType(0, E)`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-189">For an enumeration type `E`, the default value is the value produced by the expression `CType(0, E)`.</span></span>

<span data-ttu-id="0eedd-190">O tipo subjacente de uma enumeração deve ser um tipo integral que pode representar todos os valores de enumerador definidos na enumeração.</span><span class="sxs-lookup"><span data-stu-id="0eedd-190">The underlying type of an enumeration must be an integral type that can represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="0eedd-191">Se um tipo subjacente for especificado, ele deve ser `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, ou um de seus tipos correspondentes no `System` namespace.</span><span class="sxs-lookup"><span data-stu-id="0eedd-191">If an underlying type is specified, it must be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or one of their corresponding types in the `System` namespace.</span></span> <span data-ttu-id="0eedd-192">Se nenhum tipo subjacente for especificado explicitamente, o padrão é `Integer`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-192">If no underlying type is explicitly specified, the default is `Integer`.</span></span>

<span data-ttu-id="0eedd-193">O exemplo a seguir declara uma enumeração com um tipo subjacente de `Long`:</span><span class="sxs-lookup"><span data-stu-id="0eedd-193">The following example declares an enumeration with an underlying type of `Long`:</span></span>

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

<span data-ttu-id="0eedd-194">Um desenvolvedor pode optar por usar um tipo subjacente de `Long`, conforme mostrado no exemplo, para habilitar o uso de valores que estão no intervalo de `Long`, mas não está no intervalo de `Integer`, ou para preservar essa opção para o futuro.</span><span class="sxs-lookup"><span data-stu-id="0eedd-194">A developer might choose to use an underlying type of `Long`, as in the example, to enable the use of values that are in the range of `Long`, but not in the range of `Integer`, or to preserve this option for the future.</span></span>


### <a name="enumeration-members"></a><span data-ttu-id="0eedd-195">Membros de enumeração</span><span class="sxs-lookup"><span data-stu-id="0eedd-195">Enumeration Members</span></span>

<span data-ttu-id="0eedd-196">Os membros de uma enumeração são valores enumerados declarados na enumeração e os membros herdados da classe `System.Enum`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-196">The members of an enumeration are the enumerated values declared in the enumeration and the members inherited from class `System.Enum`.</span></span>

<span data-ttu-id="0eedd-197">O escopo de um membro de enumeração é o corpo da declaração de enumeração.</span><span class="sxs-lookup"><span data-stu-id="0eedd-197">The scope of an enumeration member is the enumeration declaration body.</span></span> <span data-ttu-id="0eedd-198">Isso significa que fora de uma declaração de enumeração, um membro de enumeração sempre deve ser qualificado (a menos que o tipo é especificamente importado para um namespace por meio de uma importação de namespace).</span><span class="sxs-lookup"><span data-stu-id="0eedd-198">This means that outside of an enumeration declaration, an enumeration member must always be qualified (unless the type is specifically imported into a namespace through a namespace import).</span></span>

<span data-ttu-id="0eedd-199">Ordem de declaração para declarações de membro de enumeração é significativo quando valores de expressão de constante são omitidos.</span><span class="sxs-lookup"><span data-stu-id="0eedd-199">Declaration order for enumeration member declarations is significant when constant expression values are omitted.</span></span> <span data-ttu-id="0eedd-200">Membros de enumeração têm implicitamente `Public` acessar apenas; nenhum modificador de acesso é permitidos em declarações de membro de enumeração.</span><span class="sxs-lookup"><span data-stu-id="0eedd-200">Enumeration members implicitly have `Public` access only; no access modifiers are allowed on enumeration member declarations.</span></span>

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a><span data-ttu-id="0eedd-201">Valores de enumeração</span><span class="sxs-lookup"><span data-stu-id="0eedd-201">Enumeration Values</span></span>

<span data-ttu-id="0eedd-202">Os valores enumerados em uma lista de membros de enumeração são declarados como constantes digitado como o tipo subjacente da enumeração, e eles podem aparecer onde quer que as constantes são necessárias.</span><span class="sxs-lookup"><span data-stu-id="0eedd-202">The enumerated values in an enumeration member list are declared as constants typed as the underlying enumeration type, and they can appear wherever constants are required.</span></span> <span data-ttu-id="0eedd-203">Uma definição de membro de enumeração com `=` fornece o valor indicado pela expressão constante de membro associado.</span><span class="sxs-lookup"><span data-stu-id="0eedd-203">An enumeration member definition with `=` gives the associated member the value indicated by the constant expression.</span></span> <span data-ttu-id="0eedd-204">A expressão de constante deve ser avaliada como um tipo integral que é implicitamente conversível no tipo subjacente e deve estar dentro do intervalo de valores que podem ser representados pelo tipo subjacente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-204">The constant expression must evaluate to an integral type that is implicitly convertible to the underlying type and must be within the range of values that can be represented by the underlying type.</span></span> <span data-ttu-id="0eedd-205">O exemplo a seguir está em erro porque os valores de constantes `1.5`, `2.3`, e `3.3` não são implicitamente conversíveis para o tipo integral subjacente `Long` com a semântica estrita.</span><span class="sxs-lookup"><span data-stu-id="0eedd-205">The following example is in error because the constant values `1.5`, `2.3`, and `3.3` are not implicitly convertible to the underlying integral type `Long` with strict semantics.</span></span>

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

<span data-ttu-id="0eedd-206">Vários membros de enumeração podem compartilhar o mesmo valor associado, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="0eedd-206">Multiple enumeration members may share the same associated value, as shown below:</span></span>

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

<span data-ttu-id="0eedd-207">O exemplo mostra uma enumeração que tem dois membros de enumeração – `Blue` e `Max` – que têm o mesmo valor associado.</span><span class="sxs-lookup"><span data-stu-id="0eedd-207">The example shows an enumeration that has two enumeration members -- `Blue` and `Max` -- that have the same associated value.</span></span>

<span data-ttu-id="0eedd-208">Se a primeira definição de valor de enumerador na enumeração não tem nenhum inicializador, o valor da constante correspondente será `0`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-208">If the first enumerator value definition in the enumeration has no initializer, the value of the corresponding constant is `0`.</span></span> <span data-ttu-id="0eedd-209">Uma definição do valor de enumeração sem um inicializador retorna o enumerador o valor obtido ao aumentar o valor do valor de enumeração anterior por `1`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-209">An enumeration value definition without an initializer gives the enumerator the value obtained by increasing the value of the previous enumeration value by `1`.</span></span> <span data-ttu-id="0eedd-210">Esse valor deve ser dentro do intervalo de valores que podem ser representados pelo tipo subjacente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-210">This increased value must be within the range of values that can be represented by the underlying type.</span></span>

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

<span data-ttu-id="0eedd-211">O exemplo acima imprime os valores de enumeração e seus valores associados.</span><span class="sxs-lookup"><span data-stu-id="0eedd-211">The example above prints the enumeration values and their associated values.</span></span> <span data-ttu-id="0eedd-212">A saída é:</span><span class="sxs-lookup"><span data-stu-id="0eedd-212">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="0eedd-213">Os motivos para os valores são da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0eedd-213">The reasons for the values are as follows:</span></span>

* <span data-ttu-id="0eedd-214">O valor de enumeração `Red` recebe automaticamente o valor `0` (porque ele não tem nenhum inicializador e é o primeiro membro do valor de enumeração).</span><span class="sxs-lookup"><span data-stu-id="0eedd-214">The enumeration value `Red` is automatically assigned the value `0` (since it has no initializer and is the first enumeration value member).</span></span>

* <span data-ttu-id="0eedd-215">O valor de enumeração `Green` explicitamente recebe o valor `10`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-215">The enumeration value `Green` is explicitly given the value `10`.</span></span>

* <span data-ttu-id="0eedd-216">O valor de enumeração `Blue` recebe automaticamente o valor maior do que o valor de enumeração textualmente precedente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-216">The enumeration value `Blue` is automatically assigned the value one greater than the enumeration value that textually precedes it.</span></span>

<span data-ttu-id="0eedd-217">A expressão de constante pode direta ou indiretamente usa o valor de seu próprio valor de enumeração associada (ou seja, circularidade na expressão de constante não é permitida).</span><span class="sxs-lookup"><span data-stu-id="0eedd-217">The constant expression may not directly or indirectly use the value of its own associated enumeration value (that is, circularity in the constant expression is not allowed).</span></span> <span data-ttu-id="0eedd-218">O exemplo a seguir é inválido porque as declarações das `A` e `B` são circulares.</span><span class="sxs-lookup"><span data-stu-id="0eedd-218">The following example is invalid because the declarations of `A` and `B` are circular.</span></span>

```vb
Enum Circular
    A = B
    B
End Enum
```

<span data-ttu-id="0eedd-219">`A` depende `B` explicitamente, e `B` depende `A` implicitamente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-219">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

## <a name="classes"></a><span data-ttu-id="0eedd-220">Classes</span><span class="sxs-lookup"><span data-stu-id="0eedd-220">Classes</span></span>

<span data-ttu-id="0eedd-221">Um *classe* é uma estrutura de dados que pode conter membros de dados (constantes, variáveis e eventos), os membros da função (métodos, propriedades, indexadores, operadores e construtores) e tipos aninhados.</span><span class="sxs-lookup"><span data-stu-id="0eedd-221">A *class* is a data structure that may contain data members (constants, variables, and events), function members (methods, properties, indexers, operators, and constructors), and nested types.</span></span> <span data-ttu-id="0eedd-222">As classes são tipos de referência.</span><span class="sxs-lookup"><span data-stu-id="0eedd-222">Classes are reference types.</span></span>

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

<span data-ttu-id="0eedd-223">O exemplo a seguir mostra uma classe que contém cada tipo de membro:</span><span class="sxs-lookup"><span data-stu-id="0eedd-223">The following example shows a class that contains each kind of member:</span></span>

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

<span data-ttu-id="0eedd-224">O exemplo a seguir mostra os usos desses membros:</span><span class="sxs-lookup"><span data-stu-id="0eedd-224">The following example shows uses of these members:</span></span>

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

<span data-ttu-id="0eedd-225">Há dois modificadores de classe específica `MustInherit` e `NotInheritable`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-225">There are two class-specific modifiers, `MustInherit` and `NotInheritable`.</span></span> <span data-ttu-id="0eedd-226">Não é válido para especificar ambos.</span><span class="sxs-lookup"><span data-stu-id="0eedd-226">It is invalid to specify them both.</span></span>


### <a name="class-base-specification"></a><span data-ttu-id="0eedd-227">Especificação de classe Base</span><span class="sxs-lookup"><span data-stu-id="0eedd-227">Class Base Specification</span></span>

<span data-ttu-id="0eedd-228">Uma declaração de classe pode incluir uma especificação de tipo base que define o tipo base direto da classe.</span><span class="sxs-lookup"><span data-stu-id="0eedd-228">A class declaration may include a base type specification that defines the direct base type of the class.</span></span>

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

<span data-ttu-id="0eedd-229">Se uma declaração de classe não tem nenhum tipo de base explícito, o tipo de base direto é implicitamente `Object`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-229">If a class declaration has no explicit base type, the direct base type is implicitly `Object`.</span></span> <span data-ttu-id="0eedd-230">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0eedd-230">For example:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

<span data-ttu-id="0eedd-231">Tipos de base não podem ser um parâmetro de tipo por conta própria, embora ela pode envolver os parâmetros de tipo que estão no escopo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-231">Base types cannot be a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

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

<span data-ttu-id="0eedd-232">Classes só podem derivar `Object` e classes.</span><span class="sxs-lookup"><span data-stu-id="0eedd-232">Classes may only derive from `Object` and classes.</span></span> <span data-ttu-id="0eedd-233">Não é válido para uma classe derivar de `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` ou `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-233">It is invalid for a class to derive from `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` or `System.Delegate`.</span></span> <span data-ttu-id="0eedd-234">Uma classe genérica não pode derivar de `System.Attribute` ou de uma classe que deriva dela.</span><span class="sxs-lookup"><span data-stu-id="0eedd-234">A generic class cannot derive from `System.Attribute` or from a class that derives from it.</span></span>

<span data-ttu-id="0eedd-235">Cada classe tem exatamente uma classe base direta e circularidade na derivação é proibida.</span><span class="sxs-lookup"><span data-stu-id="0eedd-235">Every class has exactly one direct base class, and circularity in derivation is prohibited.</span></span> <span data-ttu-id="0eedd-236">Não é possível derivar de um `NotInheritable` classe e o domínio de acessibilidade da classe base devem ser igual ou um superconjunto do domínio de acessibilidade da própria classe.</span><span class="sxs-lookup"><span data-stu-id="0eedd-236">It is not possible to derive from a `NotInheritable` class, and the accessibility domain of the base class must be the same as or a superset of the accessibility domain of the class itself.</span></span>


### <a name="class-members"></a><span data-ttu-id="0eedd-237">Membros de classe</span><span class="sxs-lookup"><span data-stu-id="0eedd-237">Class Members</span></span>

<span data-ttu-id="0eedd-238">Os membros de uma classe consistem em membros introduzidos por suas declarações de membro de classe e os membros herdados de sua classe base direta.</span><span class="sxs-lookup"><span data-stu-id="0eedd-238">The members of a class consist of the members introduced by its class member declarations and the members inherited from its direct base class.</span></span>

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

<span data-ttu-id="0eedd-239">Uma declaração de membro de classe pode ter `Public`, `Protected`, `Friend`, `Protected Friend`, ou `Private` acesso.</span><span class="sxs-lookup"><span data-stu-id="0eedd-239">A class member declaration may have `Public`, `Protected`, `Friend`, `Protected Friend`, or `Private` access.</span></span> <span data-ttu-id="0eedd-240">Quando uma declaração de membro de classe não inclui um modificador de acesso, a declaração assume como padrão `Public` acesso, a menos que ele seja uma declaração de variável; nesse caso, o padrão é `Private` acesso.</span><span class="sxs-lookup"><span data-stu-id="0eedd-240">When a class member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable declaration; in that case it defaults to `Private` access.</span></span>

<span data-ttu-id="0eedd-241">O escopo de um membro de classe é o corpo da classe na qual ocorre a declaração de membro, mais a lista de restrições de classe (se ela é genérica e tem restrições).</span><span class="sxs-lookup"><span data-stu-id="0eedd-241">The scope of a class member is the class body in which the member declaration occurs, plus the constraint list of that class (if it is generic and has constraints).</span></span> <span data-ttu-id="0eedd-242">Se o membro tiver `Friend` acesso, seu escopo se estende ao corpo da classe de qualquer classe derivada no mesmo programa ou qualquer assembly que foi dado `Friend` acesso, e se o membro tiver `Public`, `Protected`, ou `Protected Friend` acessar seu escopo estende o corpo da classe de qualquer classe derivada em qualquer programa.</span><span class="sxs-lookup"><span data-stu-id="0eedd-242">If the member has `Friend` access, its scope extends to the class body of any derived class in the same program or any assembly that has been given `Friend` access, and if the member has `Public`, `Protected`, or `Protected Friend` access, its scope extends to the class body of any derived class in any program.</span></span>


## <a name="structures"></a><span data-ttu-id="0eedd-243">Estruturas</span><span class="sxs-lookup"><span data-stu-id="0eedd-243">Structures</span></span>

<span data-ttu-id="0eedd-244">*Estruturas* são tipos de valor que herdam de `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-244">*Structures* are value types that inherit from `System.ValueType`.</span></span> <span data-ttu-id="0eedd-245">Estruturas são semelhantes às classes que representam estruturas de dados que podem conter membros de dados e os membros da função.</span><span class="sxs-lookup"><span data-stu-id="0eedd-245">Structures are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="0eedd-246">Diferentemente das classes, no entanto, as estruturas não requerem alocação de heap.</span><span class="sxs-lookup"><span data-stu-id="0eedd-246">Unlike classes, however, structures do not require heap allocation.</span></span>

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

<span data-ttu-id="0eedd-247">No caso de classes, é possível que duas variáveis referenciem o mesmo objeto e, portanto, é possível operações em uma variável afetem o objeto referenciado por outra variável.</span><span class="sxs-lookup"><span data-stu-id="0eedd-247">In the case of classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="0eedd-248">Com estruturas, cada variável tem sua própria cópia do não`Shared` dados, portanto, não é possível que operações em um afetem o outro, como mostra o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="0eedd-248">With structures, the variables each have their own copy of the non-`Shared` data, so it is not possible for operations on one to affect the other, as the following example illustrates:</span></span>

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="0eedd-249">Dada a declaração acima, o código a seguir gera o valor `10`:</span><span class="sxs-lookup"><span data-stu-id="0eedd-249">Given the above declaration the following code outputs the value `10`:</span></span>

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

<span data-ttu-id="0eedd-250">A atribuição de `a` à `b` cria uma cópia do valor, e `b` , portanto, não é afetado pela atribuição para `a.x`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-250">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="0eedd-251">Tinha `Point` em vez disso, foi declarado como uma classe, a saída seria `100` porque `a` e `b` fará referência ao mesmo objeto.</span><span class="sxs-lookup"><span data-stu-id="0eedd-251">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>


### <a name="structure-members"></a><span data-ttu-id="0eedd-252">Membros de estrutura</span><span class="sxs-lookup"><span data-stu-id="0eedd-252">Structure Members</span></span>

<span data-ttu-id="0eedd-253">Os membros de uma estrutura são os membros introduzidos por suas declarações de membro de estrutura e os membros herdados do `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-253">The members of a structure are the members introduced by its structure member declarations and the members inherited from `System.ValueType`.</span></span>

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

<span data-ttu-id="0eedd-254">Cada estrutura tem implicitamente um `Public` construtor de instância sem parâmetros que produz o valor padrão da estrutura.</span><span class="sxs-lookup"><span data-stu-id="0eedd-254">Every structure implicitly has a `Public` parameterless instance constructor that produces the default value of the structure.</span></span> <span data-ttu-id="0eedd-255">Como resultado, não é possível que uma declaração de tipo de estrutura declarar um construtor de instância sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="0eedd-255">As a result, it is not possible for a structure type declaration to declare a parameterless instance constructor.</span></span> <span data-ttu-id="0eedd-256">Um tipo de estrutura, no entanto, pode declarar *parametrizada* instância construtores, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="0eedd-256">A structure type is, however, permitted to declare *parameterized* instance constructors, as in the following example:</span></span>

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="0eedd-257">Dada a declaração acima, as instruções a seguir criam uma `Point` com `x` e `y` inicializados em zero.</span><span class="sxs-lookup"><span data-stu-id="0eedd-257">Given the above declaration, the following statements both create a `Point` with `x` and `y` initialized to zero.</span></span>

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

<span data-ttu-id="0eedd-258">Como estruturas contêm diretamente seus campo valores (em vez de referências a esses valores), estruturas não podem conter campos que direta ou indiretamente fazem referência a mesmos.</span><span class="sxs-lookup"><span data-stu-id="0eedd-258">Because structures directly contain their field values (rather than references to those values), structures cannot contain fields that directly or indirectly reference themselves.</span></span> <span data-ttu-id="0eedd-259">Por exemplo, o código a seguir não é válido:</span><span class="sxs-lookup"><span data-stu-id="0eedd-259">For example, the following code is not valid:</span></span>

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

<span data-ttu-id="0eedd-260">Normalmente, uma declaração de membro de estrutura pode ter apenas `Public`, `Friend`, ou `Private` acesso, mas quando substituindo membros herdados `Object`, `Protected` e `Protected Friend` acesso também pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="0eedd-260">Normally, a structure member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, `Protected` and `Protected Friend` access may also be used.</span></span> <span data-ttu-id="0eedd-261">Quando uma declaração de membro de estrutura não inclui um modificador de acesso, a declaração padrão é `Public` acesso.</span><span class="sxs-lookup"><span data-stu-id="0eedd-261">When a structure member declaration does not include an access modifier, the declaration defaults to `Public` access.</span></span> <span data-ttu-id="0eedd-262">O escopo de um membro declarado por uma estrutura é o corpo de estrutura na qual ocorre a declaração, além das restrições de estrutura (se ele foi genérico e tinha restrições).</span><span class="sxs-lookup"><span data-stu-id="0eedd-262">The scope of a member declared by a structure is the structure body in which the declaration occurs, plus the constraints of that structure (if it was generic and had constraints).</span></span>


## <a name="standard-modules"></a><span data-ttu-id="0eedd-263">Módulos padrão</span><span class="sxs-lookup"><span data-stu-id="0eedd-263">Standard Modules</span></span>

<span data-ttu-id="0eedd-264">Um *módulo padrão* é um tipo cujos membros são implicitamente `Shared` e no escopo para o espaço de declaração de namespace que contém de um módulo padrão, em vez de apenas para a declaração de módulo padrão em si.</span><span class="sxs-lookup"><span data-stu-id="0eedd-264">A *standard module* is a type whose members are implicitly `Shared` and scoped to the declaration space of the standard module's containing namespace, rather than just to the standard module declaration itself.</span></span> <span data-ttu-id="0eedd-265">Módulos padrão nunca podem ser instanciados.</span><span class="sxs-lookup"><span data-stu-id="0eedd-265">Standard modules may never be instantiated.</span></span> <span data-ttu-id="0eedd-266">É um erro para declarar uma variável de um tipo de módulo padrão.</span><span class="sxs-lookup"><span data-stu-id="0eedd-266">It is an error to declare a variable of a standard module type.</span></span>

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

<span data-ttu-id="0eedd-267">Um membro de um módulo padrão tem dois nomes totalmente qualificados, sem o nome do módulo padrão e outra com o nome do módulo padrão.</span><span class="sxs-lookup"><span data-stu-id="0eedd-267">A member of a standard module has two fully qualified names, one without the standard module name and one with the standard module name.</span></span> <span data-ttu-id="0eedd-268">Mais de um módulo padrão em um namespace pode definir um membro com um nome específico; referências não qualificadas para o nome de fora de qualquer módulo são ambíguas.</span><span class="sxs-lookup"><span data-stu-id="0eedd-268">More than one standard module in a namespace may define a member with a particular name; unqualified references to the name outside of either module are ambiguous.</span></span> <span data-ttu-id="0eedd-269">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0eedd-269">For example:</span></span>

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

<span data-ttu-id="0eedd-270">Um módulo só deve ser declarado em um namespace e não pode ser aninhado em outro tipo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-270">A module may only be declared in a namespace and may not be nested in another type.</span></span> <span data-ttu-id="0eedd-271">Módulos padrão não podem implementar interfaces, eles derivam implicitamente `Object`, e eles têm apenas `Shared` construtores.</span><span class="sxs-lookup"><span data-stu-id="0eedd-271">Standard modules may not implement interfaces, they implicitly derive from `Object`, and they have only `Shared` constructors.</span></span>


### <a name="standard-module-members"></a><span data-ttu-id="0eedd-272">Membros de módulo padrão</span><span class="sxs-lookup"><span data-stu-id="0eedd-272">Standard Module Members</span></span>

<span data-ttu-id="0eedd-273">Os membros de um módulo padrão são os membros introduzidos por suas declarações de membro e os membros herdados de `Object`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-273">The members of a standard module are the members introduced by its member declarations and the members inherited from `Object`.</span></span> <span data-ttu-id="0eedd-274">Módulos padrão podem ter qualquer tipo de membro, exceto construtores de instância.</span><span class="sxs-lookup"><span data-stu-id="0eedd-274">Standard modules may have any type of member except instance constructors.</span></span> <span data-ttu-id="0eedd-275">Todos os membros de tipo de módulo padrão são implicitamente `Shared`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-275">All standard module type members are implicitly `Shared`.</span></span>

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

<span data-ttu-id="0eedd-276">Normalmente, uma declaração de membro de módulo padrão só pode ter `Public`, `Friend`, ou `Private` acesso, mas quando substituindo membros herdados `Object`, o `Protected` e `Protected Friend` modificadores de acesso podem ser especificados.</span><span class="sxs-lookup"><span data-stu-id="0eedd-276">Normally, a standard module member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, the `Protected` and `Protected Friend` access modifiers may be specified.</span></span> <span data-ttu-id="0eedd-277">Quando uma declaração de membro de módulo padrão não inclui um modificador de acesso, a declaração assume como padrão `Public` acessar, a menos que ele seja uma variável, cujo padrão é `Private` acesso.</span><span class="sxs-lookup"><span data-stu-id="0eedd-277">When a standard module member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable, which defaults to `Private` access.</span></span>

<span data-ttu-id="0eedd-278">Como mencionado anteriormente, o escopo de um membro de módulo padrão é a declaração que contém a declaração de módulo padrão.</span><span class="sxs-lookup"><span data-stu-id="0eedd-278">As previously noted, the scope of a standard module member is the declaration containing the standard module declaration.</span></span> <span data-ttu-id="0eedd-279">Membros herdados de `Object` não estão incluídos nesse escopo especiais; esses membros não têm nenhum escopo e sempre devem ser qualificados com o nome do módulo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-279">Members inherited from `Object` are not included in this special scoping; those members have no scope and must always be qualified with the name of the module.</span></span> <span data-ttu-id="0eedd-280">Se o membro tiver `Friend` acesso, seu escopo estende-se somente aos membros do namespace declarados no mesmo programa ou assemblies que receberam `Friend` acesso.</span><span class="sxs-lookup"><span data-stu-id="0eedd-280">If the member has `Friend` access, its scope extends only to namespace members declared in the same program or assemblies that have been given `Friend` access.</span></span>


## <a name="interfaces"></a><span data-ttu-id="0eedd-281">Interfaces</span><span class="sxs-lookup"><span data-stu-id="0eedd-281">Interfaces</span></span>

<span data-ttu-id="0eedd-282">*Interfaces* são implementar que outros tipos para garantir que eles oferecem suporte a determinados métodos de tipos de referência.</span><span class="sxs-lookup"><span data-stu-id="0eedd-282">*Interfaces* are reference types that other types implement to guarantee that they support certain methods.</span></span> <span data-ttu-id="0eedd-283">Uma interface diretamente nunca é criada e não tem nenhuma representação real - outros tipos devem ser convertidos para um tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="0eedd-283">An interface is never directly created and has no actual representation -- other types must be converted to an interface type.</span></span> <span data-ttu-id="0eedd-284">Uma interface define um contrato.</span><span class="sxs-lookup"><span data-stu-id="0eedd-284">An interface defines a contract.</span></span> <span data-ttu-id="0eedd-285">Uma classe ou estrutura que implementa uma interface deve cumprir o contrato.</span><span class="sxs-lookup"><span data-stu-id="0eedd-285">A class or structure that implements an interface must adhere to its contract.</span></span>

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


<span data-ttu-id="0eedd-286">O exemplo a seguir mostra uma interface que contém uma propriedade padrão `Item`, um evento `E`, um método `F`e uma propriedade `P`:</span><span class="sxs-lookup"><span data-stu-id="0eedd-286">The following example shows an interface that contains a default property `Item`, an event `E`, a method `F`, and a property `P`:</span></span>

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

<span data-ttu-id="0eedd-287">As interfaces podem empregar a herança múltipla.</span><span class="sxs-lookup"><span data-stu-id="0eedd-287">Interfaces may employ multiple inheritance.</span></span> <span data-ttu-id="0eedd-288">No exemplo a seguir, a interface `IComboBox` herda de ambos `ITextBox` e `IListBox`:</span><span class="sxs-lookup"><span data-stu-id="0eedd-288">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`:</span></span>

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

<span data-ttu-id="0eedd-289">Classes e estruturas podem implementar várias interfaces.</span><span class="sxs-lookup"><span data-stu-id="0eedd-289">Classes and structures can implement multiple interfaces.</span></span> <span data-ttu-id="0eedd-290">No exemplo a seguir, a classe `EditBox` deriva da classe `Control` e implementa ambos `IControl` e `IDataBound`:</span><span class="sxs-lookup"><span data-stu-id="0eedd-290">In the following example, the class `EditBox` derives from the class `Control` and implements both `IControl` and `IDataBound`:</span></span>

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


### <a name="interface-inheritance"></a><span data-ttu-id="0eedd-291">Herança da interface</span><span class="sxs-lookup"><span data-stu-id="0eedd-291">Interface Inheritance</span></span>

<span data-ttu-id="0eedd-292">As interfaces de base de uma interface são as interfaces base explícitas e suas interfaces base.</span><span class="sxs-lookup"><span data-stu-id="0eedd-292">The base interfaces of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="0eedd-293">Em outras palavras, o conjunto de interfaces base é o fechamento transitivo completo as interfaces base explícitas, suas interfaces base explícitas e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="0eedd-293">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="0eedd-294">Se uma declaração de interface não tem nenhuma base de interface explícita, em seguida, não há nenhuma interface base para o tipo--interfaces não herdam `Object` (embora tenham uma conversão de ampliação para `Object`).</span><span class="sxs-lookup"><span data-stu-id="0eedd-294">If an interface declaration has no explicit interface base, then there is no base interface for the type -- interfaces do not inherit from `Object` (although they do have a widening conversion to `Object`).</span></span>

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="0eedd-295">No exemplo a seguir, as interfaces base da `IComboBox` estão `IControl`, `ITextBox`, e `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-295">In the following example, the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

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

<span data-ttu-id="0eedd-296">Uma interface herda todos os membros de suas interfaces de base.</span><span class="sxs-lookup"><span data-stu-id="0eedd-296">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="0eedd-297">Em outras palavras, o `IComboBox` acima da interface herda membros `SetText` e `SetItems` , bem como `Paint`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-297">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="0eedd-298">Uma classe ou estrutura que implementa uma interface implicitamente também implementa todas as interfaces de base da interface.</span><span class="sxs-lookup"><span data-stu-id="0eedd-298">A class or structure that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

<span data-ttu-id="0eedd-299">Se uma interface aparecer mais de uma vez em que o fechamento transitivo de interfaces base, ele apenas contribui com seus membros para a interface derivada uma vez.</span><span class="sxs-lookup"><span data-stu-id="0eedd-299">If an interface appears more than once in the transitive closure of the base interfaces, it only contributes its members to the derived interface once.</span></span> <span data-ttu-id="0eedd-300">Um tipo que implementa que a interface derivada tem apenas implementar os métodos da multiplicação definido uma vez interface base.</span><span class="sxs-lookup"><span data-stu-id="0eedd-300">A type implementing the derived interface only has to implement the methods of the multiply defined base interface once.</span></span> <span data-ttu-id="0eedd-301">No exemplo a seguir `Paint` só precisa ser implementada uma vez, mesmo que a classe implementa `IComboBox` e `IControl`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-301">In the following example, `Paint` only needs to be implemented once, even though the class implements `IComboBox` and `IControl`.</span></span>

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

<span data-ttu-id="0eedd-302">Uma `Inherits` cláusula não tem nenhum efeito em outros `Inherits` cláusulas.</span><span class="sxs-lookup"><span data-stu-id="0eedd-302">An `Inherits` clause has no effect on other `Inherits` clauses.</span></span> <span data-ttu-id="0eedd-303">No exemplo a seguir `IDerived` deve qualificar o nome do `INested` com `IBase`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-303">In the following example, `IDerived` must qualify the name of `INested` with `IBase`.</span></span>

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

<span data-ttu-id="0eedd-304">O domínio de acessibilidade de uma interface de base deve ser igual ou um superconjunto do domínio de acessibilidade da interface em si.</span><span class="sxs-lookup"><span data-stu-id="0eedd-304">The accessibility domain of a base interface must be the same as or a superset of the accessibility domain of the interface itself.</span></span>


### <a name="interface-members"></a><span data-ttu-id="0eedd-305">Membros de interface</span><span class="sxs-lookup"><span data-stu-id="0eedd-305">Interface Members</span></span>

<span data-ttu-id="0eedd-306">Os membros de uma interface consistem em membros introduzidos por suas declarações de membro e os membros herdados de suas interfaces base.</span><span class="sxs-lookup"><span data-stu-id="0eedd-306">The members of an interface consist of the members introduced by its member declarations and the members inherited from its base interfaces.</span></span>

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

<span data-ttu-id="0eedd-307">Embora interfaces não herdam os membros da `Object`, porque cada classe ou estrutura que implementa uma interface herdam `Object`, os membros de `Object`, incluindo métodos de extensão, são considerados membros de uma interface e pode ser chamado em uma interface diretamente sem a necessidade de uma conversão em `Object`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-307">Although interfaces do not inherit members from `Object`, because every class or structure that implements an interface does inherit from `Object`, the members of `Object`, including extension methods, are considered members of an interface and can be called on an interface directly without requiring a cast to `Object`.</span></span> <span data-ttu-id="0eedd-308">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0eedd-308">For example:</span></span>

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

<span data-ttu-id="0eedd-309">Membros de uma interface com o mesmo nome como membros da `Object` implicitamente sombra `Object` membros.</span><span class="sxs-lookup"><span data-stu-id="0eedd-309">Members of an interface with the same name as members of `Object` implicitly shadow `Object` members.</span></span> <span data-ttu-id="0eedd-310">Somente aninhados de tipos, métodos, propriedades e eventos podem ser membros de uma interface.</span><span class="sxs-lookup"><span data-stu-id="0eedd-310">Only nested types, methods, properties, and events may be members of an interface.</span></span> <span data-ttu-id="0eedd-311">Métodos e propriedades não podem ter um corpo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-311">Methods and properties may not have a body.</span></span> <span data-ttu-id="0eedd-312">Membros de interface são implicitamente `Public` e não pode especificar um modificador de acesso.</span><span class="sxs-lookup"><span data-stu-id="0eedd-312">Interface members are implicitly `Public` and may not specify an access modifier.</span></span> <span data-ttu-id="0eedd-313">O escopo de um membro declarado em uma interface é o corpo de interface na qual ocorre a declaração, mais a lista de restrições de interface (se ela é genérica e tem restrições).</span><span class="sxs-lookup"><span data-stu-id="0eedd-313">The scope of a member declared in an interface is the interface body in which the declaration occurs, plus the constraint list of that interface (if it is generic and has constraints).</span></span>


## <a name="arrays"></a><span data-ttu-id="0eedd-314">Matrizes</span><span class="sxs-lookup"><span data-stu-id="0eedd-314">Arrays</span></span>

<span data-ttu-id="0eedd-315">Uma *array* é um tipo de referência que contém variáveis acessadas por meio *índices* correspondente de maneira individual com a ordem das variáveis na matriz.</span><span class="sxs-lookup"><span data-stu-id="0eedd-315">An *array* is a reference type that contains variables accessed through *indices* corresponding in a one-to-one fashion with the order of the variables in the array.</span></span> <span data-ttu-id="0eedd-316">As variáveis contidas em uma matriz, também chamada a *elementos* da matriz, devem ser do mesmo tipo, e esse tipo é chamado de *tipo de elemento* da matriz.</span><span class="sxs-lookup"><span data-stu-id="0eedd-316">The variables contained in an array, also called the *elements* of the array, must all be of the same type, and this type is called the *element type* of the array.</span></span>

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

<span data-ttu-id="0eedd-317">Os elementos de uma matriz passam a existir quando uma instância de matriz é criada e deixam de existir quando a instância de matriz é destruída.</span><span class="sxs-lookup"><span data-stu-id="0eedd-317">The elements of an array come into existence when an array instance is created, and cease to exist when the array instance is destroyed.</span></span> <span data-ttu-id="0eedd-318">Cada elemento de uma matriz é inicializado com o valor padrão de seu tipo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-318">Each element of an array is initialized to the default value of its type.</span></span> <span data-ttu-id="0eedd-319">O tipo `System.Array` é o tipo base de todos os tipos de matriz e não pode ser instanciado.</span><span class="sxs-lookup"><span data-stu-id="0eedd-319">The type `System.Array` is the base type of all array types and may not be instantiated.</span></span> <span data-ttu-id="0eedd-320">Cada tipo de matriz herda os membros declarados pela `System.Array` de tipo e pode ser convertido para ele (e `Object`).</span><span class="sxs-lookup"><span data-stu-id="0eedd-320">Every array type inherits the members declared by the `System.Array` type and is convertible to it (and `Object`).</span></span> <span data-ttu-id="0eedd-321">Um matriz unidimensional do tipo com elemento `T` também implementa as interfaces `System.Collections.Generic.IList(Of T)` e `IReadOnlyList(Of T)`; se `T` é um tipo de referência e, em seguida, o tipo de matriz também implementa `IList(Of U)` e `IReadOnlyList(Of U)` para qualquer `U`que tem uma ampliação fazer referência a conversão de `T`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-321">A one-dimensional array type with element `T` also implements the interfaces `System.Collections.Generic.IList(Of T)` and `IReadOnlyList(Of T)`; if `T` is a reference type, then the array type also implements `IList(Of U)` and `IReadOnlyList(Of U)` for any `U` that has a widening  reference conversion from `T`.</span></span>

<span data-ttu-id="0eedd-322">Uma matriz tem um *classificação* que determina o número de índices associados com cada elemento da matriz.</span><span class="sxs-lookup"><span data-stu-id="0eedd-322">An array has a *rank* that determines the number of indices associated with each array element.</span></span> <span data-ttu-id="0eedd-323">A classificação de uma matriz determina o número de *dimensões* da matriz.</span><span class="sxs-lookup"><span data-stu-id="0eedd-323">The rank of an array determines the number of *dimensions* of the array.</span></span> <span data-ttu-id="0eedd-324">Por exemplo, uma matriz com uma classificação de um é chamada de uma matriz unidimensional e uma matriz com uma classificação maior do que um é chamada de uma matriz multidimensional.</span><span class="sxs-lookup"><span data-stu-id="0eedd-324">For example, an array with a rank of one is called a single-dimensional array, and an array with a rank greater than one is called a multidimensional array.</span></span>

<span data-ttu-id="0eedd-325">O exemplo a seguir cria uma matriz unidimensional de valores inteiros, inicializa os elementos da matriz e, em seguida, imprime cada um deles:</span><span class="sxs-lookup"><span data-stu-id="0eedd-325">The following example creates a single-dimensional array of integer values, initializes the array elements, and then prints each of them out:</span></span>

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

<span data-ttu-id="0eedd-326">O programa produz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="0eedd-326">The program outputs the following:</span></span>

```
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

<span data-ttu-id="0eedd-327">Cada dimensão de uma matriz tem um comprimento associado.</span><span class="sxs-lookup"><span data-stu-id="0eedd-327">Each dimension of an array has an associated length.</span></span> <span data-ttu-id="0eedd-328">Tamanhos da dimensão não fazem parte do tipo de matriz, mas em vez disso, são definidos quando uma instância do tipo de matriz é criada em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="0eedd-328">Dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run time.</span></span> <span data-ttu-id="0eedd-329">O comprimento de uma dimensão determina o intervalo válido de índices para a dimensão: para uma dimensão de comprimento `N`, índices podem variar de zero a `N-1`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-329">The length of a dimension determines the valid range of indices for that dimension: for a dimension of length `N`, indices can range from zero to `N-1`.</span></span> <span data-ttu-id="0eedd-330">Se uma dimensão é de comprimento zero, não há nenhum índices válidos para essa dimensão.</span><span class="sxs-lookup"><span data-stu-id="0eedd-330">If a dimension is of length zero, there are no valid indices for that dimension.</span></span> <span data-ttu-id="0eedd-331">O número total de elementos em uma matriz é o produto dos comprimentos de cada dimensão da matriz.</span><span class="sxs-lookup"><span data-stu-id="0eedd-331">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="0eedd-332">Se qualquer uma das dimensões de uma matriz tem um comprimento de zero, a matriz deve estar vazio.</span><span class="sxs-lookup"><span data-stu-id="0eedd-332">If any of the dimensions of an array has a length of zero, the array is said to be empty.</span></span> <span data-ttu-id="0eedd-333">O tipo de elemento de uma matriz pode ser qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-333">The element type of an array can be any type.</span></span>

<span data-ttu-id="0eedd-334">Tipos de matriz são especificados com a adição de um modificador para um nome de tipo existente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-334">Array types are specified by adding a modifier to an existing type name.</span></span> <span data-ttu-id="0eedd-335">O modificador consiste de um parêntese esquerdo, um conjunto de zero ou mais vírgulas e um parêntese direito.</span><span class="sxs-lookup"><span data-stu-id="0eedd-335">The modifier consists of a left parenthesis, a set of zero or more commas, and a right parenthesis.</span></span> <span data-ttu-id="0eedd-336">O tipo modificado é o tipo de elemento da matriz e o número de dimensões é o número de vírgulas, mais um.</span><span class="sxs-lookup"><span data-stu-id="0eedd-336">The type modified is the element type of the array, and the number of dimensions is the number of commas plus one.</span></span> <span data-ttu-id="0eedd-337">Se mais de um modificador é especificado, o tipo de elemento da matriz é uma matriz.</span><span class="sxs-lookup"><span data-stu-id="0eedd-337">If more than one modifier is specified, then the element type of the array is an array.</span></span> <span data-ttu-id="0eedd-338">Os modificadores são lidos da esquerda para a direita, com o modificador mais à esquerda, sendo a matriz mais externa.</span><span class="sxs-lookup"><span data-stu-id="0eedd-338">The modifiers are read left to right, with the leftmost modifier being the outermost array.</span></span> <span data-ttu-id="0eedd-339">No exemplo</span><span class="sxs-lookup"><span data-stu-id="0eedd-339">In the example</span></span>

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

<span data-ttu-id="0eedd-340">o tipo de elemento da `arr` é uma matriz bidimensional de tridimensionais matrizes de matrizes unidimensionais de `Integer`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-340">the element type of `arr` is a two-dimensional array of three-dimensional arrays of one-dimensional arrays of `Integer`.</span></span>

<span data-ttu-id="0eedd-341">Também é possível declarar uma variável para ser de um tipo de matriz colocando um modificador de tipo de matriz ou um modificador de tamanho da matriz de inicialização no nome da variável.</span><span class="sxs-lookup"><span data-stu-id="0eedd-341">A variable may also be declared to be of an array type by putting an array type modifier or an array-size initialization modifier on the variable name.</span></span> <span data-ttu-id="0eedd-342">Nesse caso, o tipo de elemento da matriz é o tipo de dado na declaração, e as dimensões de matriz são determinadas pelo modificador de nome de variável.</span><span class="sxs-lookup"><span data-stu-id="0eedd-342">In that case, the array element type is the type given in the declaration, and the array dimensions are determined by the variable name modifier.</span></span> <span data-ttu-id="0eedd-343">Para maior clareza, não é válido para ter um modificador de tipo de matriz em um nome de variável e um nome de tipo na mesma declaração.</span><span class="sxs-lookup"><span data-stu-id="0eedd-343">For clarity, it is not valid to have an array type modifier on both a variable name and a type name in the same declaration.</span></span>

<span data-ttu-id="0eedd-344">O exemplo a seguir mostra uma variedade de declarações de variável local que usam tipos de matriz com `Integer` como o tipo de elemento:</span><span class="sxs-lookup"><span data-stu-id="0eedd-344">The following example shows a variety of local variable declarations that use array types with `Integer` as the element type:</span></span>

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

<span data-ttu-id="0eedd-345">Um modificador de nome de tipo de matriz estende a todos os conjuntos de parênteses que o seguem.</span><span class="sxs-lookup"><span data-stu-id="0eedd-345">An array type name modifier extends to all sets of parentheses that follow it.</span></span> <span data-ttu-id="0eedd-346">Isso significa que nas situações em que um conjunto de argumentos entre parênteses é permitido após um nome de tipo, não é possível especificar os argumentos para um nome de tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="0eedd-346">This means that in the situations where a set of arguments enclosed in parenthesis is allowed after a type name, it is not possible to specify the arguments for an array type name.</span></span> <span data-ttu-id="0eedd-347">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0eedd-347">For example:</span></span>

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

<span data-ttu-id="0eedd-348">No último caso, `(3)` é interpretado como parte do nome do tipo em vez de um conjunto de argumentos de construtor.</span><span class="sxs-lookup"><span data-stu-id="0eedd-348">In the last case, `(3)` is interpreted as part of the type name rather than as a set of constructor arguments.</span></span>


## <a name="delegates"></a><span data-ttu-id="0eedd-349">Delegados</span><span class="sxs-lookup"><span data-stu-id="0eedd-349">Delegates</span></span>

<span data-ttu-id="0eedd-350">Um *delegar* é um tipo de referência que se refere a um `Shared` método de um tipo ou a um método de instância de um objeto.</span><span class="sxs-lookup"><span data-stu-id="0eedd-350">A *delegate* is a reference type that refers to a `Shared` method of a type or to an instance method of an object.</span></span>

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 <span data-ttu-id="0eedd-351">O equivalente mais próximo de um delegado em outros idiomas é um ponteiro de função, mas, enquanto um ponteiro de função só pode referenciar `Shared` funções, um delegado podem fazer referência a ambos `Shared` e métodos de instância.</span><span class="sxs-lookup"><span data-stu-id="0eedd-351">The closest equivalent of a delegate in other languages is a function pointer, but whereas a function pointer can only reference `Shared` functions, a delegate can reference both `Shared` and instance methods.</span></span> <span data-ttu-id="0eedd-352">No último caso, o delegado armazena não apenas uma referência ao ponto de entrada do método, mas também uma referência à instância do objeto com o qual invocar o método.</span><span class="sxs-lookup"><span data-stu-id="0eedd-352">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance with which to invoke the method.</span></span>

<span data-ttu-id="0eedd-353">A declaração de delegado pode não ter uma `Handles` cláusula, uma `Implements` cláusula, um corpo de método, ou um `End` construir.</span><span class="sxs-lookup"><span data-stu-id="0eedd-353">The delegate declaration may not have  a `Handles` clause, an `Implements` clause, a method body, or an `End` construct.</span></span> <span data-ttu-id="0eedd-354">A lista de parâmetros da declaração de delegado não pode conter `Optional` ou `ParamArray` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="0eedd-354">The parameter list of the delegate declaration may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="0eedd-355">O domínio de acessibilidade dos tipos de parâmetro e tipo de retorno deve ser igual ou um superconjunto do domínio de acessibilidade do delegado em si.</span><span class="sxs-lookup"><span data-stu-id="0eedd-355">The accessibility domain of the return type and parameter types must be the same as or a superset of the accessibility domain of the delegate itself.</span></span>

<span data-ttu-id="0eedd-356">Os membros de um delegado são os membros herdados da classe `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-356">The members of a delegate are the members inherited from class `System.Delegate`.</span></span> <span data-ttu-id="0eedd-357">Um delegado também define os métodos a seguir:</span><span class="sxs-lookup"><span data-stu-id="0eedd-357">A delegate also defines the following methods:</span></span>

* <span data-ttu-id="0eedd-358">Um construtor que aceita dois parâmetros, uma do tipo `Object` e uma do tipo `System.IntPtr`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-358">A constructor that takes two parameters, one of type `Object` and one of type `System.IntPtr`.</span></span>

* <span data-ttu-id="0eedd-359">Um `Invoke` método que tem a mesma assinatura que o delegado.</span><span class="sxs-lookup"><span data-stu-id="0eedd-359">An `Invoke` method that has the same signature as the delegate.</span></span>

* <span data-ttu-id="0eedd-360">Um `BeginInvoke` método cuja assinatura é a assinatura do delegado, com três diferenças.</span><span class="sxs-lookup"><span data-stu-id="0eedd-360">A `BeginInvoke` method whose signature is the delegate signature, with three differences.</span></span> <span data-ttu-id="0eedd-361">Primeiro, o tipo de retorno é alterado para `System.IAsyncResult`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-361">First, the return type is changed to `System.IAsyncResult`.</span></span> <span data-ttu-id="0eedd-362">Em segundo lugar, dois parâmetros adicionais são adicionados ao final da lista de parâmetros: o primeiro do tipo `System.AsyncCallback` e o segundo, do tipo `Object`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-362">Second, two additional parameters are added to the end of the parameter list: the first of type `System.AsyncCallback` and the second of type `Object`.</span></span> <span data-ttu-id="0eedd-363">E por fim, todos os `ByRef` parâmetros são alterados para ser `ByVal`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-363">And finally, all `ByRef` parameters are changed to be `ByVal`.</span></span>

* <span data-ttu-id="0eedd-364">Um `EndInvoke` cujo tipo de retorno é o mesmo que o delegado do método.</span><span class="sxs-lookup"><span data-stu-id="0eedd-364">An `EndInvoke` method whose return type is the same as the delegate.</span></span> <span data-ttu-id="0eedd-365">Os parâmetros do método são apenas os parâmetros do delegado exatamente que são `ByRef` parâmetros na mesma ordem em que elas ocorrem na assinatura do delegado.</span><span class="sxs-lookup"><span data-stu-id="0eedd-365">The parameters of the method are only the delegate parameters exactly that are `ByRef` parameters, in the same order they occur in the delegate signature.</span></span>  <span data-ttu-id="0eedd-366">Além desses parâmetros, há um parâmetro adicional do tipo `System.IAsyncResult` no final da lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="0eedd-366">In addition to those parameters, there is an additional parameter of type `System.IAsyncResult` at the end of the parameter list.</span></span>

<span data-ttu-id="0eedd-367">Definir e usar delegados em três etapas: declaração, instanciação e chamada.</span><span class="sxs-lookup"><span data-stu-id="0eedd-367">There are three steps in defining and using delegates: declaration, instantiation, and invocation.</span></span>

<span data-ttu-id="0eedd-368">Delegados são declarados usando a sintaxe de declaração de delegado.</span><span class="sxs-lookup"><span data-stu-id="0eedd-368">Delegates are declared using delegate declaration syntax.</span></span> <span data-ttu-id="0eedd-369">O exemplo a seguir declara um delegado chamado `SimpleDelegate` que não usa argumentos:</span><span class="sxs-lookup"><span data-stu-id="0eedd-369">The following example declares a delegate named `SimpleDelegate` that takes no arguments:</span></span>

```vb
Delegate Sub SimpleDelegate()
```

<span data-ttu-id="0eedd-370">O exemplo a seguir cria um `SimpleDelegate` da instância e, em seguida, chama imediatamente o ele:</span><span class="sxs-lookup"><span data-stu-id="0eedd-370">The next example creates a `SimpleDelegate` instance and then immediately calls it:</span></span>

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

<span data-ttu-id="0eedd-371">Não há muito sentido em instanciando um delegado para um método e, em seguida, chamar imediatamente por meio do delegado, como seria mais simples chamar o método diretamente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-371">There is not much point in instantiating a delegate for a method and then immediately calling via the delegate, as it would be simpler to call the method directly.</span></span> <span data-ttu-id="0eedd-372">Delegados mostram sua utilidade quando seu anonimato é usado.</span><span class="sxs-lookup"><span data-stu-id="0eedd-372">Delegates show their usefulness when their anonymity is used.</span></span> <span data-ttu-id="0eedd-373">A exemplo a seguir mostra uma `MultiCall` método que chama repetidamente um `SimpleDelegate` instância:</span><span class="sxs-lookup"><span data-stu-id="0eedd-373">The next example shows a `MultiCall` method that repeatedly calls a `SimpleDelegate` instance:</span></span>

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

<span data-ttu-id="0eedd-374">Não são importante para o `MultiCall` o método que o método de destino para o `SimpleDelegate` é o que acessibilidade esse método tem, ou se o método é `Shared` ou não.</span><span class="sxs-lookup"><span data-stu-id="0eedd-374">It is unimportant to the `MultiCall` method what the target method for the `SimpleDelegate` is, what accessibility this method has, or whether the method is `Shared` or not.</span></span> <span data-ttu-id="0eedd-375">Tudo o que importa é que a assinatura do método de destino é compatível com `SimpleDelegate`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-375">All that matters is that the signature of the target method is compatible with `SimpleDelegate`.</span></span>


## <a name="partial-types"></a><span data-ttu-id="0eedd-376">Tipos parciais</span><span class="sxs-lookup"><span data-stu-id="0eedd-376">Partial types</span></span>

<span data-ttu-id="0eedd-377">Declarações de classe e estrutura podem ser *parcial* declarações.</span><span class="sxs-lookup"><span data-stu-id="0eedd-377">Class and structure declarations can be *partial* declarations.</span></span> <span data-ttu-id="0eedd-378">Uma declaração parcial pode ou não pode descrever completamente o tipo declarado dentro da declaração.</span><span class="sxs-lookup"><span data-stu-id="0eedd-378">A partial declaration may or may not fully describe the declared type within the declaration.</span></span> <span data-ttu-id="0eedd-379">Em vez disso, a declaração de tipo pode ser distribuída entre várias declarações parciais dentro do programa; tipos parciais não podem ser declarados entre os limites do programa.</span><span class="sxs-lookup"><span data-stu-id="0eedd-379">Instead, the declaration of the type may be spread across multiple partial declarations within the program; partial types cannot be declared across program boundaries.</span></span> <span data-ttu-id="0eedd-380">Especifica uma declaração de tipo parcial a `Partial` modificador na declaração.</span><span class="sxs-lookup"><span data-stu-id="0eedd-380">A partial type declaration specifies the `Partial` modifier on the declaration.</span></span> <span data-ttu-id="0eedd-381">Em seguida, quaisquer outras declarações no programa para um tipo com o mesmo nome totalmente qualificado serão mescladas junto com a declaração parcial no tempo de compilação para formar uma única declaração de tipo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-381">Then, any other declarations in the program for a type with the same fully-qualified name will be merged together with the partial declaration at compile-time to form a single type declaration.</span></span> <span data-ttu-id="0eedd-382">Por exemplo, o código a seguir declara uma classe única `Test` com os membros `Test.C1` e `Test.C2`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-382">For example, the following code declares a single class `Test` with members `Test.C1` and `Test.C2`.</span></span>

<span data-ttu-id="0eedd-383">a.vb:</span><span class="sxs-lookup"><span data-stu-id="0eedd-383">a.vb:</span></span>

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

<span data-ttu-id="0eedd-384">b.vb:</span><span class="sxs-lookup"><span data-stu-id="0eedd-384">b.vb:</span></span>

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

<span data-ttu-id="0eedd-385">Ao combinar as declarações de tipo parcial, pelo menos uma das declarações deve ter um `Partial` modificador, caso contrário, resulta um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0eedd-385">When combining partial type declarations, at least one of the declarations must have a `Partial` modifier, otherwise a compile-time error results.</span></span>

<span data-ttu-id="0eedd-386">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0eedd-386">__Note.__</span></span> <span data-ttu-id="0eedd-387">Embora seja possível especificar `Partial` em apenas uma declaração entre várias declarações parciais, é melhor forma especificá-lo em todas as declarações parciais.</span><span class="sxs-lookup"><span data-stu-id="0eedd-387">Although it is possible to specify `Partial` on only one declaration among many partial declarations, it is better form to specify it on all partial declarations.</span></span> <span data-ttu-id="0eedd-388">A situação em que uma declaração parcial está visível, mas um ou mais declarações parciais estão ocultos (como no caso de estender o código gerado por ferramenta), é aceitável para deixar o `Partial` modificador fora da declaração visível especificá-lo, mas no declarações ocultas.</span><span class="sxs-lookup"><span data-stu-id="0eedd-388">In the situation where one partial declaration is visible but one or more partial declarations are hidden (such as the case of extending tool-generated code), it is acceptable to leave the `Partial` modifier off of the visible declaration but specify it on the hidden declarations.</span></span>

<span data-ttu-id="0eedd-389">Somente classes e estruturas podem ser declaradas usando declarações parciais.</span><span class="sxs-lookup"><span data-stu-id="0eedd-389">Only classes and structures can be declared using partial declarations.</span></span> <span data-ttu-id="0eedd-390">A aridade de um tipo é considerada durante a correspondência de declarações parciais juntos: duas classes com o mesmo nome mas com diferentes números de parâmetros de tipo não são consideradas para ser declarações parciais de simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-390">The arity of a type is considered when matching partial declarations together: two classes with the same name but different numbers of type parameters are not considered to be partial declarations of the same time.</span></span> <span data-ttu-id="0eedd-391">Declarações parciais podem especificar atributos, modificadores, de classe `Inherits` instrução ou `Implements` instrução.</span><span class="sxs-lookup"><span data-stu-id="0eedd-391">Partial declarations can specify attributes, class modifiers, `Inherits` statement or `Implements` statement.</span></span> <span data-ttu-id="0eedd-392">Em tempo de compilação, todas as partes das declarações parciais são combinadas em conjunto e usadas como parte da declaração de tipo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-392">At compile time, all of the pieces of the partial declarations are combined together and used as a part of the type declaration.</span></span> <span data-ttu-id="0eedd-393">Se houver qualquer conflito entre os atributos, modificadores, bases, interfaces, ou membros de tipo, resultados de um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0eedd-393">If there are any conflicts between attributes, modifiers, bases, interfaces, or type members, a compile-time error results.</span></span> <span data-ttu-id="0eedd-394">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0eedd-394">For example:</span></span>

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

<span data-ttu-id="0eedd-395">O exemplo anterior declara um tipo `Test1` que é `Public`, herda `Object` e implementa `System.IDisposable` e `System.IComparable`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-395">The previous example declares a type `Test1` that is `Public`, inherits from `Object` and implements `System.IDisposable` and `System.IComparable`.</span></span> <span data-ttu-id="0eedd-396">As declarações parciais de `Test2` causará um erro de tempo de compilação porque uma das declarações diz que `Test2` é `Public` e outro diz que `Test2` é `Private`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-396">The partial declarations of `Test2` will cause a compile-time error because one of the declarations says that `Test2` is `Public` and another says that `Test2` is `Private`.</span></span>

<span data-ttu-id="0eedd-397">Tipos parciais com parâmetros de tipo podem declarar restrições e a variação para os parâmetros de tipo, mas as restrições e a variância de cada declaração parcial devem corresponder.</span><span class="sxs-lookup"><span data-stu-id="0eedd-397">Partial types with type parameters can declare constraints and variance for the type parameters, but the constraints and variance from each partial declaration must match.</span></span> <span data-ttu-id="0eedd-398">Assim, restrições e variância são especiais em que eles não são combinados como outros modificadores automaticamente:</span><span class="sxs-lookup"><span data-stu-id="0eedd-398">Thus, constraints and variance are special in that they are not automatically combined like other modifiers:</span></span>

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

<span data-ttu-id="0eedd-399">O fato de que um tipo é declarado usando várias declarações parciais não afeta as regras de pesquisa de nome dentro do tipo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-399">The fact that a type is declared using multiple partial declarations does not affect the name lookup rules within the type.</span></span> <span data-ttu-id="0eedd-400">Como resultado, uma declaração de tipo parcial pode usar membros declarados em outras declarações de tipo parcial, ou pode implementar métodos em interfaces declaradas em outras declarações de tipo parcial.</span><span class="sxs-lookup"><span data-stu-id="0eedd-400">As a result, a partial type declaration can use members declared in other partial type declarations, or may implement methods on interfaces declared in other partial type declarations.</span></span> <span data-ttu-id="0eedd-401">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0eedd-401">For example:</span></span>

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

<span data-ttu-id="0eedd-402">Tipos aninhados podem ter as declarações parciais.</span><span class="sxs-lookup"><span data-stu-id="0eedd-402">Nested types can have partial declarations as well.</span></span> <span data-ttu-id="0eedd-403">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0eedd-403">For example:</span></span>

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

<span data-ttu-id="0eedd-404">Inicializadores de dentro de uma declaração parcial ainda serão executados em ordem de declaração. No entanto, não há nenhuma garantia de ordem de execução para inicializadores ocorrerem em declarações parciais separadas.</span><span class="sxs-lookup"><span data-stu-id="0eedd-404">Initializers within a partial declaration will still be executed in declaration order; however, there is no guaranteed order of execution for initializers that occur in separate partial declarations.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="0eedd-405">Tipos construídos</span><span class="sxs-lookup"><span data-stu-id="0eedd-405">Constructed Types</span></span>

<span data-ttu-id="0eedd-406">Uma declaração de tipo genérico, por si só, não denota um tipo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-406">A generic type declaration, by itself, does not denote a type.</span></span> <span data-ttu-id="0eedd-407">Em vez disso, uma declaração de tipo genérico pode ser usada como uma "esquema" para muitos tipos diferentes de formulário por meio da aplicação de argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="0eedd-407">Instead, a generic type declaration can be used as a "blueprint" to form many different types by applying type arguments.</span></span> <span data-ttu-id="0eedd-408">Um tipo genérico que tem argumentos de tipo aplicados a ele é chamado uma *tipo construído*.</span><span class="sxs-lookup"><span data-stu-id="0eedd-408">A generic type that has type arguments applied to it is called a *constructed type*.</span></span> <span data-ttu-id="0eedd-409">Os argumentos de tipo em um tipo construído sempre devem satisfazer as restrições colocadas em parâmetros de tipo, que eles correspondem ao.</span><span class="sxs-lookup"><span data-stu-id="0eedd-409">The type arguments in a constructed type must always satisfy the constraints placed on the type parameters they match to.</span></span>

<span data-ttu-id="0eedd-410">Um nome de tipo pode identificar um tipo construído, mesmo que ele não especifica os parâmetros de tipo diretamente.</span><span class="sxs-lookup"><span data-stu-id="0eedd-410">A type name might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="0eedd-411">Isso pode ocorrer em que um tipo é aninhado dentro de uma declaração de classe genérica, e o tipo de instância da declaração do recipiente implicitamente é usado para pesquisa de nome:</span><span class="sxs-lookup"><span data-stu-id="0eedd-411">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup:</span></span>

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

<span data-ttu-id="0eedd-412">Um tipo construído `C(Of T1,...,Tn)` está acessível quando o tipo genérico e todos os argumentos de tipo são acessíveis.</span><span class="sxs-lookup"><span data-stu-id="0eedd-412">A constructed type `C(Of T1,...,Tn)` is accessible when the generic type and all the type arguments are accessible.</span></span> <span data-ttu-id="0eedd-413">Por exemplo, se o tipo genérico `C` está `Public` e todos os argumentos de tipo `T1,...,Tn` são `Public`, em seguida, o tipo construído é `Public`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-413">For instance, if the generic type `C` is `Public` and all of the type arguments `T1,...,Tn` are `Public`, then the constructed type is `Public`.</span></span> <span data-ttu-id="0eedd-414">Se o nome do tipo ou um dos argumentos de tipo estiver `Private`, no entanto, em seguida, a acessibilidade do tipo construído é `Private`.</span><span class="sxs-lookup"><span data-stu-id="0eedd-414">If either the type name or one of the type arguments is `Private`, however, then the accessibility of the constructed type is `Private`.</span></span> <span data-ttu-id="0eedd-415">Se for um argumento de tipo do tipo construído `Protected` e o outro argumento de tipo é `Friend`, em seguida, o tipo construído é acessível somente a classe e suas subclasses neste assembly ou qualquer assembly que tem `Friend` acesso.</span><span class="sxs-lookup"><span data-stu-id="0eedd-415">If one type argument of the constructed type is `Protected` and another type argument is `Friend`, then the constructed type is accessible only in the class and its subclasses in this assembly or any assembly that has been given `Friend` access.</span></span> <span data-ttu-id="0eedd-416">Em outras palavras, o domínio de acessibilidade para um tipo construído é a interseção dos domínios de acessibilidade de suas partes constituintes.</span><span class="sxs-lookup"><span data-stu-id="0eedd-416">In other words, the accessibility domain for a constructed type is the intersection of the accessibility domains of its constituent parts.</span></span>

<span data-ttu-id="0eedd-417">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0eedd-417">__Note.__</span></span> <span data-ttu-id="0eedd-418">O fato de que o domínio de acessibilidade do tipo construído é a interseção de suas partes constituídos tem o efeito colateral interessante da definição de um novo nível de acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="0eedd-418">The fact that the accessibility domain of constructed type is the intersection of its constituted parts has the interesting side effect of defining a new accessibility level.</span></span> <span data-ttu-id="0eedd-419">Um tipo construído que contém um elemento que está `Protected` e um elemento que está `Friend` só pode ser acessado em contextos que podem acessar *ambos* `Friend` *e* `Protected`membros.</span><span class="sxs-lookup"><span data-stu-id="0eedd-419">A constructed type that contains an element that is `Protected` and an element that is `Friend` can only be accessed in contexts that can access *both* `Friend` *and* `Protected` members.</span></span> <span data-ttu-id="0eedd-420">No entanto, não há nenhuma maneira de expressar esse nível de acessibilidade na linguagem, como a acessibilidade `Protected Friend` significa que uma entidade pode ser acessada em um contexto que possa acessar *qualquer* `Friend` *ou* `Protected` membros.</span><span class="sxs-lookup"><span data-stu-id="0eedd-420">However, there is no way to express this accessibility level in the language, as the accessibility `Protected Friend` means that an entity can be accessed in a context that can access *either* `Friend` *or* `Protected` members.</span></span>

<span data-ttu-id="0eedd-421">As interfaces de base, implementados e os membros de tipos construídos são determinados pelo substituindo os argumentos de tipo fornecido para cada ocorrência do parâmetro de tipo no tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="0eedd-421">The base, implemented interfaces and members of constructed types are determined by substituting the supplied type arguments for each occurrence of the type parameter in the generic type.</span></span>

### <a name="open-types-and-closed-types"></a><span data-ttu-id="0eedd-422">Tipos de aberto e fechado</span><span class="sxs-lookup"><span data-stu-id="0eedd-422">Open Types and Closed Types</span></span>

<span data-ttu-id="0eedd-423">Um tipo construído para que um ou mais argumentos de tipo são parâmetros de tipo de um tipo recipiente ou do método é chamado um *abrir tipo*.</span><span class="sxs-lookup"><span data-stu-id="0eedd-423">A constructed type for who one or more type arguments are type parameters of a containing type or method is called an *open type*.</span></span> <span data-ttu-id="0eedd-424">Isso é porque alguns dos parâmetros de tipo do tipo ainda não conhecidos, portanto, a forma real do tipo ainda não é totalmente conhecida.</span><span class="sxs-lookup"><span data-stu-id="0eedd-424">This is because some of the type parameters of the type are still not known, so the actual shape of the type is not yet fully known.</span></span> <span data-ttu-id="0eedd-425">Por outro lado, um tipo genérico, cujos argumentos de tipo são todos os parâmetros de tipo não é chamado um *fechado tipo*.</span><span class="sxs-lookup"><span data-stu-id="0eedd-425">In contrast, a generic type whose type arguments are all non-type parameters is called a *closed type*.</span></span> <span data-ttu-id="0eedd-426">A forma de um tipo fechado sempre totalmente é conhecida.</span><span class="sxs-lookup"><span data-stu-id="0eedd-426">The shape of a closed type is always fully known.</span></span> <span data-ttu-id="0eedd-427">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0eedd-427">For example:</span></span>

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

<span data-ttu-id="0eedd-428">O tipo construído `Base(Of Integer, V)` é um tipo aberto, porque embora o parâmetro de tipo `T` tiver sido fornecido, o parâmetro de tipo `U` foi outro parâmetro de tipo fornecido.</span><span class="sxs-lookup"><span data-stu-id="0eedd-428">The constructed type `Base(Of Integer, V)` is an open type because although the type parameter `T` has been supplied, the type parameter `U` has been supplied another type parameter.</span></span> <span data-ttu-id="0eedd-429">Assim, a forma completa do tipo ainda não é conhecida.</span><span class="sxs-lookup"><span data-stu-id="0eedd-429">Thus, the full shape of the type is not yet known.</span></span> <span data-ttu-id="0eedd-430">O tipo construído `Derived(Of Double)`, no entanto, é um tipo fechado porque todos os parâmetros de tipo na hierarquia de herança foram fornecidos.</span><span class="sxs-lookup"><span data-stu-id="0eedd-430">The constructed type `Derived(Of Double)`, however, is a closed type because all type parameters in the inheritance hierarchy have been supplied.</span></span>

<span data-ttu-id="0eedd-431">Tipos abertos são definidos da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0eedd-431">Open types are defined as follows:</span></span>

* <span data-ttu-id="0eedd-432">Um parâmetro de tipo é um tipo aberto.</span><span class="sxs-lookup"><span data-stu-id="0eedd-432">A type parameter is an open type.</span></span>

* <span data-ttu-id="0eedd-433">Um tipo de matriz é um tipo aberto, se seu tipo de elemento for um tipo aberto.</span><span class="sxs-lookup"><span data-stu-id="0eedd-433">An array type is an open type if its element type is an open type.</span></span>

* <span data-ttu-id="0eedd-434">Um tipo construído é um tipo aberto se um ou mais dos seus argumentos de tipo são um tipo aberto.</span><span class="sxs-lookup"><span data-stu-id="0eedd-434">A constructed type is an open type if one or more of its type arguments are an open type.</span></span>

* <span data-ttu-id="0eedd-435">Um tipo fechado é um tipo que não é um tipo aberto.</span><span class="sxs-lookup"><span data-stu-id="0eedd-435">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="0eedd-436">Como o ponto de entrada do programa não pode estar em um tipo genérico, todos os tipos usados em tempo de execução será tipos fechados.</span><span class="sxs-lookup"><span data-stu-id="0eedd-436">Because the program entry point cannot be in a generic type, all types used at run-time will be closed types.</span></span>

## <a name="special-types"></a><span data-ttu-id="0eedd-437">Tipos especiais</span><span class="sxs-lookup"><span data-stu-id="0eedd-437">Special Types</span></span>

<span data-ttu-id="0eedd-438">O .NET Framework contém um número de classes que são tratados especialmente pelo .NET Framework e a linguagem Visual Basic:</span><span class="sxs-lookup"><span data-stu-id="0eedd-438">The .NET Framework contains a number of classes that are treated specially by the .NET Framework and by the Visual Basic language:</span></span>

<span data-ttu-id="0eedd-439">O tipo `System.Void`, que representa um tipo void no .NET Framework, podem ser diretamente acessados apenas em `GetType` expressões.</span><span class="sxs-lookup"><span data-stu-id="0eedd-439">The type `System.Void`, which represents a void type in the .NET Framework, can be directly referenced only in `GetType` expressions.</span></span>

<span data-ttu-id="0eedd-440">Os tipos `System.RuntimeArgumentHandle`, `System.ArgIterator` e `System.TypedReference` todos podem conter ponteiros na pilha e portanto não pode aparecer no heap do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0eedd-440">The types `System.RuntimeArgumentHandle`, `System.ArgIterator` and `System.TypedReference` all can contain pointers into the stack and so cannot appear on the .NET Framework heap.</span></span> <span data-ttu-id="0eedd-441">Portanto, não podem ser usados como tipos de elemento de matriz, tipos de retorno, tipos de campo, argumentos de tipo genéricos, tipos anuláveis, `ByRef` tipos de parâmetro, o tipo de um valor que está sendo convertido em `Object` ou `System.ValueType`, o destino de uma chamada para a instância os membros `Object` ou `System.ValueType`, ou sobem para um fechamento.</span><span class="sxs-lookup"><span data-stu-id="0eedd-441">Therefore, they cannot be used as array element types, return types, field types, generic type arguments, nullable types, `ByRef` parameter types, the type of a value being converted to `Object` or `System.ValueType`, the target of a call to instance members of `Object` or `System.ValueType`, or lifted into a closure.</span></span>
