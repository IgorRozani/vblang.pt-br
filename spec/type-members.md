# <a name="type-members"></a><span data-ttu-id="ce7ed-101">Membros de tipos</span><span class="sxs-lookup"><span data-stu-id="ce7ed-101">Type Members</span></span>

<span data-ttu-id="ce7ed-102">Membros de tipo definem locais de armazenamento e o código executável.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-102">Type members define storage locations and executable code.</span></span> <span data-ttu-id="ce7ed-103">Eles podem ser métodos, construtores, constantes, variáveis, propriedades e eventos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-103">They can be methods, constructors, events, constants, variables, and properties.</span></span>

## <a name="interface-method-implementation"></a><span data-ttu-id="ce7ed-104">Implementação do método de interface</span><span class="sxs-lookup"><span data-stu-id="ce7ed-104">Interface Method Implementation</span></span>

<span data-ttu-id="ce7ed-105">Métodos, eventos e propriedades podem implementar membros de interface.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-105">Methods, events, and properties can implement interface members.</span></span> <span data-ttu-id="ce7ed-106">Para implementar um membro de interface, uma declaração de membro Especifica o `Implements` palavra-chave e um ou mais membros de interface de lista.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-106">To implement an interface member, a member declaration specifies the `Implements` keyword and lists one or more interface members.</span></span>

```antlr
ImplementsClause
    : ( 'Implements' ImplementsList )?
    ;

ImplementsList
    : InterfaceMemberSpecifier ( Comma InterfaceMemberSpecifier )*
    ;

InterfaceMemberSpecifier
    : NonArrayTypeName Period IdentifierOrKeyword
    ;
```

<span data-ttu-id="ce7ed-107">Métodos e propriedades que implementam membros de interface são implicitamente `NotOverridable` , a menos que declarado como `MustOverride`, `Overridable`, ou a substituição de outro membro.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-107">Methods and properties that implement interface members are implicitly `NotOverridable` unless declared to be `MustOverride`, `Overridable`, or overriding another member.</span></span> <span data-ttu-id="ce7ed-108">É um erro para um membro de implementação de um membro de interface para ser `Shared`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-108">It is an error for a member implementing an interface member to be `Shared`.</span></span> <span data-ttu-id="ce7ed-109">Acessibilidade de um membro não tem nenhum efeito em sua capacidade de implementar membros de interface.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-109">A member's accessibility has no effect on its ability to implement interface members.</span></span>

<span data-ttu-id="ce7ed-110">Para uma implementação de interface seja válida, lista do tipo recipiente implementa deve nomear uma interface que contém um membro compatível.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-110">For an interface implementation to be valid, the implements list of the containing type must name an interface that contains a compatible member.</span></span> <span data-ttu-id="ce7ed-111">Um membro compatível é um cuja assinatura coincide com a assinatura do membro implementado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-111">A compatible member is one whose signature matches the signature of the implementing member.</span></span> <span data-ttu-id="ce7ed-112">Se uma interface genérica está sendo implementada, o argumento de tipo fornecido na cláusula Implements é substituído na assinatura durante a verificação de compatibilidade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-112">If a generic interface is being implemented, then the type argument supplied in the Implements clause is substituted into the signature when checking compatibility.</span></span> <span data-ttu-id="ce7ed-113">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-113">For example:</span></span>

```vb
Interface I1(Of T)
    Sub F(x As T)
End Interface

Class C1
    Implements I1(Of Integer)

    Sub F(x As Integer) Implements I1(Of Integer).F
    End Sub
End Class

Class C2(Of U)
    Implements I1(Of U)

    Sub F(x As U) Implements I1(Of U).F
    End Sub
End Class
```

<span data-ttu-id="ce7ed-114">Se um evento declarado usando um tipo de delegado é implementar um evento de interface, um evento compatível é um cujo tipo delegado subjacente é do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-114">If an event declared using a delegate type is implementing an interface event, then a compatible event is one whose underlying delegate type is the same type.</span></span> <span data-ttu-id="ce7ed-115">Caso contrário, o evento usa o tipo de delegado do evento de interface que está implementando.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-115">Otherwise, the event uses the delegate type from the interface event it is implementing.</span></span> <span data-ttu-id="ce7ed-116">Se tal evento implementa vários eventos de interface, todos os eventos de interface devem ter o mesmo tipo delegado subjacente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-116">If such an event implements multiple interface events, all the interface events must have the same underlying delegate type.</span></span> <span data-ttu-id="ce7ed-117">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-117">For example:</span></span>

```vb
Interface ClickEvents
    Event LeftClick(x As Integer, y As Integer)
    Event RightClick(x As Integer, y As Integer)
End Interface

Class Button
    Implements ClickEvents

    ' OK. Signatures match, delegate type = ClickEvents.LeftClickHandler.
    Event LeftClick(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick

    ' OK. Signatures match, delegate type = ClickEvents.RightClickHandler.
    Event RightClick(x As Integer, y As Integer) _
        Implements ClickEvents.RightClick
End Class

Class Label
    Implements ClickEvents

    ' Error. Signatures match, but can't be both delegate types.
    Event Click(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick, ClickEvents.RightClick
End Class
```

<span data-ttu-id="ce7ed-118">Um membro de interface na lista implementa é especificado usando um nome de tipo, um período e um identificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-118">An interface member in the implements list is specified using a type name, a period, and an identifier.</span></span> <span data-ttu-id="ce7ed-119">O nome do tipo deve ser uma interface na lista implementa ou uma interface na lista implementa uma interface de base e o identificador deve ser um membro da interface especificada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-119">The type name must be an interface in the implements list or a base interface of an interface in the implements list, and the identifier must be a member of the specified interface.</span></span> <span data-ttu-id="ce7ed-120">Um único membro pode implementar mais de um membro de interface correspondente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-120">A single member can implement more than one matching interface member.</span></span>

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Class Test
    Implements ILeft, IRight

    Sub F() Implements ILeft.F, IRight.F
    End Sub
End Class
```

<span data-ttu-id="ce7ed-121">Se o membro de interface que está sendo implementado não está disponível em todas as interfaces explicitamente implementadas devido a várias heranças de interface, membro implementado deve referenciar explicitamente uma interface de base no qual o membro está disponível.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-121">If the interface member being implemented is unavailable in all explicitly implemented interfaces because of multiple interface inheritance, the implementing member must explicitly reference a base interface on which the member is available.</span></span> <span data-ttu-id="ce7ed-122">Por exemplo, se `I1` e `I2` conter um membro `M`, e `I3` herda `I1` e `I2`, um tipo que implementa `I3` implementará `I1.M` e `I2.M`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-122">For example, if `I1` and `I2` contain a member `M`, and `I3` inherits from `I1` and `I2`, a type implementing `I3` will implement `I1.M` and `I2.M`.</span></span> <span data-ttu-id="ce7ed-123">Se um sombras interface multiplicam membros herdados, terá um tipo de implementação implementar os membros herdados e os membros sombreamento-los.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-123">If an interface shadows multiply inherited members, an implementing type will have to implement the inherited members and the member(s) shadowing them.</span></span>

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

Class Test
    Implements ILeftRight

    Sub LeftF() Implements ILeft.F
    End Sub

    Sub RightF() Implements IRight.F
    End Sub

    Sub LeftRightF() Implements ILeftRight.F
    End Sub
End Class
```

<span data-ttu-id="ce7ed-124">Se a interface que contém o membro de interface ser implementada é genérico, os mesmos argumentos de tipo deve ser fornecida a interface que está sendo implementa.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-124">If the containing interface of the interface member be implemented is generic, the same type arguments as the interface being implements must be supplied.</span></span> <span data-ttu-id="ce7ed-125">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-125">For example:</span></span>

```vb
Interface I1(Of T)
    Function F() As T
End Interface

Class C1
    Implements I1(Of Integer)
    Implements I1(Of Double)

    Function F1() As Integer Implements I1(Of Integer).F
    End Function

    Function F2() As Double Implements I1(Of Double).F
    End Function

    ' Error: I1(Of String) is not implemented by C1
    Function F3() As String Implements I1(Of String).F
    End Function
End Class

Class C2(Of U)
    Implements I1(Of U)

    Function F() As U Implements I1(Of U).F
    End Function
End Class
```


## <a name="methods"></a><span data-ttu-id="ce7ed-126">Métodos</span><span class="sxs-lookup"><span data-stu-id="ce7ed-126">Methods</span></span>

<span data-ttu-id="ce7ed-127">Métodos contêm as declarações de um programa executáveis.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-127">Methods contain the executable statements of a program.</span></span>

```antlr
MethodMemberDeclaration
    : MethodDeclaration
    | ExternalMethodDeclaration
    ;

InterfaceMethodMemberDeclaration
    : InterfaceMethodDeclaration
    ;

MethodDeclaration
    : SubDeclaration
    | MustOverrideSubDeclaration
    | FunctionDeclaration
    | MustOverrideFunctionDeclaration
    ;

InterfaceMethodDeclaration
    : InterfaceSubDeclaration
    | InterfaceFunctionDeclaration
    ;

SubSignature
    : 'Sub' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
    ;

FunctionSignature
    : 'Function' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

SubDeclaration
    : Attributes? ProcedureModifier* SubSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Sub' StatementTerminator
    ;

MustOverrideSubDeclaration
    : Attributes? MustOverrideProcedureModifier+ SubSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceSubDeclaration
    : Attributes? InterfaceProcedureModifier* SubSignature StatementTerminator
    ;

FunctionDeclaration
    : Attributes? ProcedureModifier* FunctionSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Function' StatementTerminator
    ;

MustOverrideFunctionDeclaration
    : Attributes? MustOverrideProcedureModifier+ FunctionSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceFunctionDeclaration
    : Attributes? InterfaceProcedureModifier* FunctionSignature StatementTerminator
    ;

ProcedureModifier
    : AccessModifier | 'Shadows' | 'Shared' | 'Overridable' | 'NotOverridable' | 'Overrides'
    | 'Overloads' | 'Partial' | 'Iterator' | 'Async'
    ;

MustOverrideProcedureModifier
    : ProcedureModifier
    | 'MustOverride'
    ;

InterfaceProcedureModifier
    : 'Shadows' | 'Overloads'
    ;

HandlesOrImplements
    : HandlesClause
    | ImplementsClause
    ;
```

<span data-ttu-id="ce7ed-128">Métodos, que tem uma lista opcional de parâmetros e um valor de retorno opcional, são compartilhados ou não compartilhados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-128">Methods, which have an optional list of parameters and an optional return value, are either shared or not shared.</span></span> <span data-ttu-id="ce7ed-129">Métodos compartilhados são acessados por meio de instâncias da classe ou classe.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-129">Shared methods are accessed through the class or instances of the class.</span></span> <span data-ttu-id="ce7ed-130">Métodos não compartilhados, também chamados de métodos de instância, são acessados por meio de instâncias da classe.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-130">Non-shared methods, also called instance methods, are accessed through instances of the class.</span></span> <span data-ttu-id="ce7ed-131">O exemplo a seguir mostra uma classe `Stack` que tem vários métodos compartilhados (`Clone` e `Flip`), e vários métodos de instância (`Push`, `Pop`, e `ToString`):</span><span class="sxs-lookup"><span data-stu-id="ce7ed-131">The following example shows a class `Stack` that has several shared methods (`Clone` and `Flip`), and several instance methods (`Push`, `Pop`, and `ToString`):</span></span>

```vb
Public Class Stack
    Public Shared Function Clone(s As Stack) As Stack
        ...
    End Function

    Public Shared Function Flip(s As Stack) As Stack
        ...
    End Function

    Public Function Pop() As Object
        ...
    End Function

    Public Sub Push(o As Object)
        ...
    End Sub 

    Public Overrides Function ToString() As String
        ...
    End Function 
End Class 

Module Test
    Sub Main()
        Dim s As Stack = New Stack()
        Dim i As Integer

        While i < 10
            s.Push(i)
        End While

        Dim flipped As Stack = Stack.Flip(s)
        Dim cloned As Stack = Stack.Clone(s)

        Console.WriteLine("Original stack: " & s.ToString())
        Console.WriteLine("Flipped stack: " & flipped.ToString())
        Console.WriteLine("Cloned stack: " & cloned.ToString())
    End Sub
End Module
```

<span data-ttu-id="ce7ed-132">Métodos podem ser sobrecarregados, o que significa que vários métodos podem ter o mesmo nome, desde que eles tenham assinaturas exclusivas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-132">Methods can be overloaded, which means that multiple methods may have the same name so long as they have unique signatures.</span></span> <span data-ttu-id="ce7ed-133">A assinatura de um método consiste em número e tipos de seus parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-133">The signature of a method consists of the number and types of its parameters.</span></span> <span data-ttu-id="ce7ed-134">A assinatura de um método especificamente não inclui o tipo de retorno ou modificadores de parâmetro como opcional, ByRef ou ParamArray.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-134">The signature of a method specifically does not include the return type or parameter modifiers such as Optional, ByRef or ParamArray.</span></span> <span data-ttu-id="ce7ed-135">O exemplo a seguir mostra uma classe com um número de sobrecargas:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-135">The following example shows a class with a number of overloads:</span></span>

```vb
Module Test
    Sub F()
        Console.WriteLine("F()")
    End Sub 

    Sub F(o As Object)
        Console.WriteLine("F(Object)")
    End Sub

    Sub F(value As Integer)
        Console.WriteLine("F(Integer)")
    End Sub 

    Sub F(a As Integer, b As Integer)
        Console.WriteLine("F(Integer, Integer)")
    End Sub 

    Sub F(values() As Integer)
        Console.WriteLine("F(Integer())")
    End Sub 

    Sub G(s As String, Optional s2 As String = 5)
        Console.WriteLine("G(String, Optional String")
    End Sub

    Sub G(s As String)
        Console.WriteLine("G(String)")
    End Sub


    Sub Main()
        F()
        F(1)
        F(CType(1, Object))
        F(1, 2)
        F(New Integer() { 1, 2, 3 })
        G("hello")
        G("hello", "world")
    End Sub
End Module
```

<span data-ttu-id="ce7ed-136">A saída do programa é:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-136">The output of the program is:</span></span>

```vb
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

<span data-ttu-id="ce7ed-137">Sobrecargas que diferem apenas em parâmetros opcionais podem ser usadas para "controle de versão" das bibliotecas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-137">Overloads that differ only in optional parameters can be used for "versioning" of libraries.</span></span> <span data-ttu-id="ce7ed-138">Por exemplo, v1 de uma biblioteca pode incluir uma função com parâmetros opcionais:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-138">For instance, v1 of a library might include a function with optional parameters:</span></span>

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

<span data-ttu-id="ce7ed-139">Em seguida, v2 da biblioteca quer adicionar outro parâmetro opcional "senha", e deseja fazer isso sem interromper a compatibilidade de origem (de modo que os aplicativos usados para direcionar v1 podem ser recompilados) e sem interromper a compatibilidade binária (de modo que os aplicativos usados para referência v1 agora pode fazer referência v2 sem recompilação).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-139">Then v2 of the library wants to add another optional parameter "password", and it wants to do so without breaking source compatibility (so applications that used to target v1 can be recompiled), and without breaking binary compatibility (so applications that used to reference v1 can now reference v2 without recompilation).</span></span> <span data-ttu-id="ce7ed-140">Esta é a aparência v2:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-140">This is how v2 will look:</span></span>

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

<span data-ttu-id="ce7ed-141">Observe que os parâmetros opcionais em uma API pública não são compatíveis com CLS.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-141">Note that optional parameters in a public API are not CLS-compliant.</span></span> <span data-ttu-id="ce7ed-142">No entanto, eles podem ser consumidos pelo menos por Visual Basic e C n º 4 e F #.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-142">However, they can be consumed at least by Visual Basic and C#4 and F#.</span></span>



### <a name="regular-async-and-iterator-method-declarations"></a><span data-ttu-id="ce7ed-143">Regular, assíncrono e iterador declarações de método</span><span class="sxs-lookup"><span data-stu-id="ce7ed-143">Regular, Async and Iterator Method Declarations</span></span>

<span data-ttu-id="ce7ed-144">Há dois tipos de métodos: *sub-rotinas*, que não retornam valores, e *funções*, que fazer.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-144">There are two types of methods: *subroutines*, which do not return values, and *functions*, which do.</span></span> <span data-ttu-id="ce7ed-145">O corpo e `End` construção de um método só pode ser omitida se o método é definido em uma interface ou se tiver o `MustOverride` modificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-145">The body and `End` construct of a method may only be omitted if the method is defined in an interface or has the `MustOverride` modifier.</span></span> <span data-ttu-id="ce7ed-146">Se nenhum tipo de retorno é especificado em uma função e a semântica estrita está sendo usada, ocorre um erro de tempo de compilação; Caso contrário, o tipo é implicitamente `Object` ou o tipo de caractere de tipo do método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-146">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs; otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="ce7ed-147">O domínio de acessibilidade do tipo de retorno e tipos de parâmetro de um método deve ser igual ou um superconjunto do domínio de acessibilidade do método em si.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-147">The accessibility domain of the return type and parameter types of a method must be the same as or a superset of the accessibility domain of the method itself.</span></span>

<span data-ttu-id="ce7ed-148">Um __método regular__ é aquela com nenhum dos dois `Async` nem `Iterator` modificadores.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-148">A __regular method__ is one with neither `Async` nor `Iterator` modifiers.</span></span> <span data-ttu-id="ce7ed-149">Pode ser uma sub-rotina ou uma função.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-149">It may be a subroutine or a function.</span></span> <span data-ttu-id="ce7ed-150">Seção [métodos regulares](statements.md#regular-methods) fornece detalhes sobre o que acontece quando um método regular é invocado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-150">Section [Regular Methods](statements.md#regular-methods) details what happens when a regular method is invoked.</span></span>

<span data-ttu-id="ce7ed-151">Uma __método iterador__ é aquele com o `Iterator` modificador e nenhum `Async` modificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-151">An __iterator method__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="ce7ed-152">Ele deve ser uma função, e seu tipo de retorno deve ser `IEnumerator`, `IEnumerable`, ou `IEnumerator(Of T)` ou `IEnumerable(Of T)` para alguns `T`, e não deve ter `ByRef` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-152">It must be a function, and its return type must be `IEnumerator`, `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and it must have no `ByRef` parameters.</span></span> <span data-ttu-id="ce7ed-153">Seção [métodos de iterador](statements.md#iterator-methods) fornece detalhes sobre o que acontece quando um método iterador é invocado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-153">Section [Iterator Methods](statements.md#iterator-methods) details what happens when an iterator method is invoked.</span></span>

<span data-ttu-id="ce7ed-154">Uma __método assíncrono__ é aquele com o `Async` modificador e nenhum `Iterator` modificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-154">An __async method__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="ce7ed-155">Ele deve ser uma sub-rotina, ou uma função com o tipo de retorno `Task` ou `Task(Of T)` para alguns `T`e deve ter nenhum `ByRef` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-155">It must be either a subroutine, or a function with return type `Task` or `Task(Of T)` for some `T`, and must have no `ByRef` parameters.</span></span> <span data-ttu-id="ce7ed-156">Seção [métodos assíncronos](statements.md#async-methods) fornece detalhes sobre o que acontece quando um método assíncrono é invocado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-156">Section [Async Methods](statements.md#async-methods) details what happens when an async method is invoked.</span></span>

<span data-ttu-id="ce7ed-157">Ele é um erro de tempo de compilação se um método não é um desses três tipos de método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-157">It is a compile-time error if a method is not one of these three kinds of method.</span></span>

<span data-ttu-id="ce7ed-158">Declarações de função e de sub-rotina são especiais em que seu início e instruções end devem cada começar no início de uma linha lógica.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-158">Subroutine and function declarations are special in that their beginning and end statements must each start at the beginning of a logical line.</span></span> <span data-ttu-id="ce7ed-159">Além disso, o corpo de um não -`MustOverride` declaração de função ou sub-rotina deve começar no início de uma linha lógica.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-159">Additionally, the body of a non-`MustOverride` subroutine or function declaration must start at the beginning of a logical line.</span></span> <span data-ttu-id="ce7ed-160">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-160">For example:</span></span>

```vb
Module Test
    ' Illegal: Subroutine doesn't start the line
    Public x As Integer : Sub F() : End Sub

    ' Illegal: First statement doesn't start the line
    Sub G() : Console.WriteLine("G")
    End Sub

    ' Illegal: End Sub doesn't start the line
    Sub H() : End Sub
End Module
```


### <a name="external-method-declarations"></a><span data-ttu-id="ce7ed-161">Declarações de método externo</span><span class="sxs-lookup"><span data-stu-id="ce7ed-161">External Method Declarations</span></span>

<span data-ttu-id="ce7ed-162">Uma declaração de método externo apresenta um novo método cuja implementação é fornecida para o programa externo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-162">An external method declaration introduces a new method whose implementation is provided external to the program.</span></span>

```antlr
ExternalMethodDeclaration
    : ExternalSubDeclaration
    | ExternalFunctionDeclaration
    ;

ExternalSubDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Sub'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )? StatementTerminator
    ;

ExternalFunctionDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Function'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
      StatementTerminator
    ;

ExternalMethodModifier
    : AccessModifier
    | 'Shadows'
    | 'Overloads'
    ;

CharsetModifier
    : 'Ansi' | 'Unicode' | 'Auto'
    ;

LibraryClause
    : 'Lib' StringLiteral
    ;

AliasClause
    : 'Alias' StringLiteral
    ;
```

<span data-ttu-id="ce7ed-163">Como uma declaração de método externo não fornece nenhuma implementação real, ele não tem nenhum corpo de método ou `End` construir.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-163">Because an external method declaration provides no actual implementation, it has no method body or `End` construct.</span></span> <span data-ttu-id="ce7ed-164">Métodos externos são implicitamente compartilhados, pode não ter parâmetros de tipo e podem não manipular eventos ou implementar membros de interface.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-164">External methods are implicitly shared, may not have type parameters, and may not handle events or implement interface members.</span></span> <span data-ttu-id="ce7ed-165">Se nenhum tipo de retorno é especificado em uma função e a semântica estrita está sendo usada, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-165">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="ce7ed-166">Caso contrário, o tipo é implicitamente `Object` ou o tipo de caractere de tipo do método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-166">Otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="ce7ed-167">O domínio de acessibilidade do tipo de retorno e tipos de parâmetro de um método externo deve ser igual ou um superconjunto do domínio de acessibilidade do método externo em si.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-167">The accessibility domain of the return type and parameter types of an external method must be the same as or a superset of the accessibility domain of the external method itself.</span></span>

<span data-ttu-id="ce7ed-168">A cláusula de biblioteca de uma declaração de método externa Especifica o nome do arquivo externo que implementa o método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-168">The library clause of an external method declaration specifies the name of the external file that implements the method.</span></span> <span data-ttu-id="ce7ed-169">A cláusula alias opcional é uma cadeia de caracteres que especifica o ordinal numérico (prefixado por um `#` caractere) ou o nome do método no arquivo externo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-169">The optional alias clause is a string that specifies the numeric ordinal (prefixed by a `#` character) or name of the method in the external file.</span></span> <span data-ttu-id="ce7ed-170">Um modificador de conjunto de caractere único também pode ser especificado, que controla o conjunto de caracteres usado para realizar marshaling de cadeias de caracteres durante uma chamada para o método externo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-170">A single-character set modifier may also be specified, which governs the character set used to marshal strings during a call to the external method.</span></span> <span data-ttu-id="ce7ed-171">O `Unicode` modificador realiza marshaling de todas as cadeias de caracteres para valores Unicode, o `Ansi` modificador realiza marshaling de todas as cadeias de caracteres para valores ANSI e o `Auto` modificador realiza marshaling de cadeias de caracteres de acordo com as regras do .NET Framework com base no nome do método, ou o nome do alias se especificado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-171">The `Unicode` modifier marshals all strings to Unicode values, the `Ansi` modifier marshals all strings to ANSI values, and the `Auto` modifier marshals the strings according to .NET Framework rules based on the name of the method, or the alias name if specified.</span></span> <span data-ttu-id="ce7ed-172">Se nenhum modificador é especificado, o padrão é `Ansi`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-172">If no modifier is specified, the default is `Ansi`.</span></span>

<span data-ttu-id="ce7ed-173">Se `Ansi` ou `Unicode` for especificado, o nome do método é pesquisado no arquivo externo sem modificação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-173">If `Ansi` or `Unicode` is specified, then the method name is looked up in the external file with no modification.</span></span> <span data-ttu-id="ce7ed-174">Se `Auto` for especificado, a pesquisa de nome de método depende da plataforma.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-174">If `Auto` is specified, then method name lookup depends on the platform.</span></span> <span data-ttu-id="ce7ed-175">Se a plataforma é considerada ANSI (por exemplo, o Windows 95, Windows 98, Windows ME), em seguida, o nome do método é pesquisado sem modificação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-175">If the platform is considered to be ANSI (for example, Windows 95, Windows 98, Windows ME), then the method name is looked up with no modification.</span></span> <span data-ttu-id="ce7ed-176">Se a pesquisa falhar, um `A` é acrescentado e repetir a pesquisa.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-176">If the lookup fails, an `A` is appended and the lookup tried again.</span></span> <span data-ttu-id="ce7ed-177">Se a plataforma é considerada como Unicode (por exemplo, Windows NT, Windows 2000, Windows XP), em seguida, um `W` é acrescentado e o nome é pesquisado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-177">If the platform is considered to be Unicode (for example, Windows NT, Windows 2000, Windows XP), then a `W` is appended and the name is looked up.</span></span> <span data-ttu-id="ce7ed-178">Se a pesquisa falhar, a pesquisa será tentada novamente sem o `W`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-178">If the lookup fails, the lookup is tried again without the `W`.</span></span> <span data-ttu-id="ce7ed-179">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-179">For example:</span></span>

```vb
Module Test
    ' All platforms bind to "ExternSub".
    Declare Ansi Sub ExternSub Lib "ExternDLL" ()

    ' All platforms bind to "ExternSub".
    Declare Unicode Sub ExternSub Lib "ExternDLL" ()

    ' ANSI platforms: bind to "ExternSub" then "ExternSubA".
    ' Unicode platforms: bind to "ExternSubW" then "ExternSub".
    Declare Auto Sub ExternSub Lib "ExternDLL" ()
End Module
```

<span data-ttu-id="ce7ed-180">Tipos de dados que está sendo passados para métodos externos são empacotados de acordo com as convenções de marshaling de dados do .NET Framework com uma exceção.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-180">Data types being passed to external methods are marshaled according to the .NET Framework data marshalling conventions with one exception.</span></span> <span data-ttu-id="ce7ed-181">As variáveis que são passadas por valor de cadeia de caracteres (ou seja, `ByVal x As String`) são empacotados para o tipo BSTR de automação OLE, e as alterações feitas para o BSTR no método externo são refletidas no argumento de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-181">String variables that are passed by value (that is, `ByVal x As String`) are marshaled to the OLE Automation BSTR type, and changes made to the BSTR in the external method are reflected back in the string argument.</span></span> <span data-ttu-id="ce7ed-182">Isso ocorre porque o tipo `String` na externa métodos é mutável, e esse marshalling especial imita esse comportamento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-182">This is because the type `String` in external methods is mutable, and this special marshalling mimics that behavior.</span></span> <span data-ttu-id="ce7ed-183">Parâmetros que são passados por referência da cadeia de caracteres (ou seja, `ByRef x As String`) são empacotados como um ponteiro para o tipo BSTR de automação OLE.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-183">String parameters that are passed by reference (i.e. `ByRef x As String`) are marshaled as a pointer to the OLE Automation BSTR type.</span></span> <span data-ttu-id="ce7ed-184">É possível substituir esses comportamentos especiais, especificando o `System.Runtime.InteropServices.MarshalAsAttribute` atributo no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-184">It is possible to override these special behaviors by specifying the `System.Runtime.InteropServices.MarshalAsAttribute` attribute on the parameter.</span></span>

<span data-ttu-id="ce7ed-185">O exemplo demonstra o uso de métodos externos:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-185">The example demonstrates use of external methods:</span></span>

```vb
Class Path
    Declare Function CreateDirectory Lib "kernel32" ( _
        Name As String, sa As SecurityAttributes) As Boolean
    Declare Function RemoveDirectory Lib "kernel32" ( _
        Name As String) As Boolean
    Declare Function GetCurrentDirectory Lib "kernel32" ( _
        BufSize As Integer, Buf As String) As Integer
    Declare Function SetCurrentDirectory Lib "kernel32" ( _
        Name As String) As Boolean
End Class
```


### <a name="overridable-methods"></a><span data-ttu-id="ce7ed-186">Métodos substituíveis</span><span class="sxs-lookup"><span data-stu-id="ce7ed-186">Overridable Methods</span></span>

<span data-ttu-id="ce7ed-187">O `Overridable` modificador indica que um método é substituível.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-187">The `Overridable` modifier indicates that a method is overridable.</span></span> <span data-ttu-id="ce7ed-188">O `Overrides` modificador indica que um método substitui um método substituível de tipo base que tem a mesma assinatura.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-188">The `Overrides` modifier indicates that a method overrides a base-type overridable method that has the same signature.</span></span> <span data-ttu-id="ce7ed-189">O `NotOverridable` modificador indica que um método substituível não pode ser substituído ainda mais.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-189">The `NotOverridable` modifier indicates that an overridable method cannot be further overridden.</span></span> <span data-ttu-id="ce7ed-190">O `MustOverride` modificador indica que um método deve ser substituído em classes derivadas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-190">The `MustOverride` modifier indicates that a method must be overridden in derived classes.</span></span>

<span data-ttu-id="ce7ed-191">Determinadas combinações desses modificadores não são válidas:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-191">Certain combinations of these modifiers are not valid:</span></span>

* <span data-ttu-id="ce7ed-192">`Overridable` e `NotOverridable` são mutuamente exclusivos e não podem ser combinados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-192">`Overridable` and `NotOverridable` are mutually exclusive and cannot be combined.</span></span>

* <span data-ttu-id="ce7ed-193">`MustOverride` implica `Overridable` (e, portanto, não é possível especificá-lo) e não pode ser combinada com `NotOverridable`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-193">`MustOverride` implies `Overridable` (and so cannot specify it) and cannot be combined with `NotOverridable`.</span></span>

* <span data-ttu-id="ce7ed-194">`NotOverridable` não pode ser combinado com `Overridable` ou `MustOverride` e deve ser combinada com `Overrides`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-194">`NotOverridable` cannot be combined with `Overridable` or `MustOverride` and must be combined with `Overrides`.</span></span>

* <span data-ttu-id="ce7ed-195">`Overrides` implica `Overridable` (e, portanto, não é possível especificá-lo) e não pode ser combinada com `MustOverride`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-195">`Overrides` implies `Overridable` (and so cannot specify it) and cannot be combined with `MustOverride`.</span></span>

<span data-ttu-id="ce7ed-196">Também há restrições adicionais em métodos substituíveis:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-196">There are also additional restrictions on overridable methods:</span></span>

* <span data-ttu-id="ce7ed-197">Um `MustOverride` método não pode incluir um corpo de método ou uma `End` construir, não podem substituir outro método e só pode aparecer em `MustInherit` classes.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-197">A `MustOverride` method may not include a method body or an `End` construct, may not override another method, and may only appear in `MustInherit` classes.</span></span>

* <span data-ttu-id="ce7ed-198">Se um método especifica `Overrides` e não há nenhum método de base correspondente, substituir, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-198">If a method specifies `Overrides` and there is no matching base method to override, a compile-time error occurs.</span></span> <span data-ttu-id="ce7ed-199">Um método de substituição não pode especificar `Shadows`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-199">An overriding method may not specify `Shadows`.</span></span>

* <span data-ttu-id="ce7ed-200">Um método não pode substituir outro método, se o domínio de acessibilidade do método de substituição não é igual ao domínio de acessibilidade do método que está sendo substituído.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-200">A method may not override another method if the overriding method's accessibility domain is not equal to the accessibility domain of the method being overridden.</span></span> <span data-ttu-id="ce7ed-201">A única exceção é que um método substituindo um `Protected Friend` método em outro assembly que não tenha `Friend` acesso deve especificar `Protected` (não `Protected Friend`).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-201">The one exception is that a method overriding a `Protected Friend` method in another assembly that does not have `Friend` access must specify `Protected` (not `Protected Friend`).</span></span>

* <span data-ttu-id="ce7ed-202">`Private` métodos podem não estar `Overridable`, `NotOverridable`, ou `MustOverride`, nem talvez elas substituem outros métodos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-202">`Private` methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, nor may they override other methods.</span></span>

* <span data-ttu-id="ce7ed-203">Métodos em `NotInheritable` classes não podem ser declaradas `Overridable` ou `MustOverride`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-203">Methods in `NotInheritable` classes may not be declared `Overridable` or `MustOverride`.</span></span>

<span data-ttu-id="ce7ed-204">O exemplo a seguir ilustra as diferenças entre métodos substituíveis e nonoverridable:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-204">The following example illustrates the differences between overridable and nonoverridable methods:</span></span>

```vb
Class Base
    Public Sub F()
        Console.WriteLine("Base.F")
    End Sub

    Public Overridable Sub G()
        Console.WriteLine("Base.G")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Shadows Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Overrides Sub G()
        Console.WriteLine("Derived.G")
    End Sub
End Class

Module Test
    Sub Main()
        Dim d As Derived = New Derived()
        Dim b As Base = d

        b.F()
        d.F()
        b.G()
        d.G()
    End Sub
End Module
```

<span data-ttu-id="ce7ed-205">No exemplo, a classe `Base` apresenta um método `F` e uma `Overridable` método `G`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-205">In the example, class `Base` introduces a method `F` and an `Overridable` method `G`.</span></span> <span data-ttu-id="ce7ed-206">A classe `Derived` apresenta um novo método `F`, sombreamento, portanto, o herdado `F`e também substitui o método herdado `G`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-206">The class `Derived` introduces a new method `F`, thus shadowing the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="ce7ed-207">O exemplo produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-207">The example produces the following output:</span></span>

```
Base.F
Derived.F
Derived.G
Derived.G
```

<span data-ttu-id="ce7ed-208">Observe que a instrução `b.G()` invoca `Derived.G`, e não `Base.G`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-208">Notice that the statement `b.G()` invokes `Derived.G`, not `Base.G`.</span></span> <span data-ttu-id="ce7ed-209">Isso ocorre porque o tempo de execução de tipo da instância (que é `Derived`) em vez do tipo de tempo de compilação da instância (que é `Base`) determina a implementação real do método para invocar.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-209">This is because the run-time type of the instance (which is `Derived`) rather than the compile-time type of the instance (which is `Base`) determines the actual method implementation to invoke.</span></span>

### <a name="shared-methods"></a><span data-ttu-id="ce7ed-210">Métodos compartilhados</span><span class="sxs-lookup"><span data-stu-id="ce7ed-210">Shared Methods</span></span>

<span data-ttu-id="ce7ed-211">O `Shared` modificador indica um método é um *compartilhado método*.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-211">The `Shared` modifier indicates a method is a *shared method*.</span></span> <span data-ttu-id="ce7ed-212">Um método compartilhado não funciona em uma instância específica de um tipo e pode ser chamado diretamente de um tipo em vez de uma determinada instância de um tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-212">A shared method does not operate on a specific instance of a type and may be invoked directly from a type rather than through a particular instance of a type.</span></span> <span data-ttu-id="ce7ed-213">No entanto, é válido, use uma instância para qualificar um método compartilhado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-213">It is valid, however, to use an instance to qualify a shared method.</span></span> <span data-ttu-id="ce7ed-214">Não é válido para fazer referência a `Me`, `MyClass`, ou `MyBase` em um método compartilhado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-214">It is invalid to refer to `Me`, `MyClass`, or `MyBase` in a shared method.</span></span> <span data-ttu-id="ce7ed-215">Métodos compartilhados não podem ser `Overridable`, `NotOverridable`, ou `MustOverride`, e eles não podem substituir métodos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-215">Shared methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, and they may not override methods.</span></span> <span data-ttu-id="ce7ed-216">Métodos definidos em interfaces e módulos padrão não podem especificar `Shared`, pois eles são implicitamente `Shared` já.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-216">Methods defined in standard modules and interfaces may not specify `Shared`, because they are implicitly `Shared` already.</span></span>

<span data-ttu-id="ce7ed-217">Um método declarado em uma estrutura ou classe sem um `Shared` modificador é um *método de instância*.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-217">A method declared in a structure or class without a `Shared` modifier is an *instance method*.</span></span> <span data-ttu-id="ce7ed-218">Um método de instância opera em uma determinada instância de um tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-218">An instance method operates on a given instance of a type.</span></span> <span data-ttu-id="ce7ed-219">Métodos de instância só pode ser chamados por meio de uma instância de um tipo e pode referir-se à instância por meio de `Me` expressão.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-219">Instance methods can only be invoked through an instance of a type and may refer to the instance through the `Me` expression.</span></span>

<span data-ttu-id="ce7ed-220">O exemplo a seguir ilustra as regras de acesso compartilhado e os membros da instância:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-220">The following example illustrates the rules for accessing shared and instance members:</span></span>

```vb
Class Test
    Private x As Integer
    Private Shared y As Integer

    Sub F()
        x = 1 ' Ok, same as Me.x = 1.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub G()
        x = 1 ' Error, cannot access Me.x.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub Main()
        Dim t As Test = New Test()

        t.x = 1 ' Ok.
        t.y = 1 ' Ok.
        Test.x = 1 ' Error, cannot access instance member through type.
        Test.y = 1 ' Ok.
    End Sub
End Class
```

<span data-ttu-id="ce7ed-221">Método `F` mostra que um membro da função de instância, um identificador pode ser usado para acessar membros de instância e membros compartilhados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-221">Method `F` shows that in an instance function member, an identifier can be used to access both instance members and shared members.</span></span> <span data-ttu-id="ce7ed-222">Método `G` mostra que, em um membro da função compartilhada, ele é um erro ao acessar um membro de instância por meio de um identificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-222">Method `G` shows that in a shared function member, it is an error to access an instance member through an identifier.</span></span> <span data-ttu-id="ce7ed-223">Método `Main` mostra que, em uma expressão de acesso de membro, membros de instância devem ser acessados por meio de instâncias, mas membros compartilhados podem ser acessados por meio de tipos ou instâncias.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-223">Method `Main` shows that in a member access expression, instance members must be accessed through instances, but shared members can be accessed through types or instances.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="ce7ed-224">Parâmetros de método</span><span class="sxs-lookup"><span data-stu-id="ce7ed-224">Method Parameters</span></span>

<span data-ttu-id="ce7ed-225">Um *parâmetro* é uma variável que pode ser usada para passar informações dentro e fora de um método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-225">A *parameter* is a variable that can be used to pass information into and out of a method.</span></span> <span data-ttu-id="ce7ed-226">Parâmetros de um método são declarados por lista de parâmetros do método, que consiste em um ou mais parâmetros separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-226">Parameters of a method are declared by the method's parameter list, which consists of one or more parameters separated by commas.</span></span>

```antlr
ParameterList
    : Parameter ( Comma Parameter )*
    ;

Parameter
    : Attributes? ParameterModifier* ParameterIdentifier ( 'As' TypeName )?
      ( Equals ConstantExpression )?
    ;

ParameterModifier
    : 'ByVal' | 'ByRef' | 'Optional' | 'ParamArray'
    ;

ParameterIdentifier
    : Identifier IdentifierModifiers
    ;
```

<span data-ttu-id="ce7ed-227">Se nenhum tipo é especificado para um parâmetro e a semântica estrita é usada, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-227">If no type is specified for a parameter and strict semantics are used, a compile-time error occurs.</span></span> <span data-ttu-id="ce7ed-228">Caso contrário, o tipo padrão é `Object` ou o tipo de caractere de tipo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-228">Otherwise the default type is `Object` or the type of the parameter's type character.</span></span> <span data-ttu-id="ce7ed-229">Até mesmo em semântica permissiva, se um parâmetro inclui um `As` cláusula, todos os parâmetros devem especificar tipos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-229">Even under permissive semantics, if one parameter includes an `As` clause, all parameters must specify types.</span></span>

<span data-ttu-id="ce7ed-230">Parâmetros são especificados como parâmetros de valor, referência, opcional ou paramarray pelos modificadores `ByVal`, `ByRef`, `Optional`, e `ParamArray`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-230">Parameters are specified as value, reference, optional, or paramarray parameters by the modifiers `ByVal`, `ByRef`, `Optional`, and `ParamArray`, respectively.</span></span> <span data-ttu-id="ce7ed-231">Um parâmetro que não especifica `ByRef` ou `ByVal` assume como padrão `ByVal`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-231">A parameter that does not specify `ByRef` or `ByVal` defaults to `ByVal`.</span></span>

<span data-ttu-id="ce7ed-232">Nomes de parâmetro têm o escopo para todo o corpo do método e sempre são publicamente acessíveis.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-232">Parameter names are scoped to the entire body of the method and are always publicly accessible.</span></span> <span data-ttu-id="ce7ed-233">Uma invocação de método cria uma cópia específica para essa invocação, os parâmetros, e a lista de argumentos da invocação atribui valores ou referências de variável aos parâmetros recém-criado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-233">A method invocation creates a copy, specific to that invocation, of the parameters, and the argument list of the invocation assigns values or variable references to the newly created parameters.</span></span> <span data-ttu-id="ce7ed-234">Como declarações de método externo e declarações de delegado não tem nenhum corpo, nomes de parâmetro duplicados são permitidos em listas de parâmetros, mas não recomendados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-234">Because external method declarations and delegate declarations have no body, duplicate parameter names are allowed in parameter lists, but discouraged.</span></span>

<span data-ttu-id="ce7ed-235">O identificador pode ser seguido pelo modificador anulável nome `?` para indicar que permite valor nulo, e também por modificadores de nome de matriz para indicar que ele é uma matriz.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-235">The identifier may be followed by the nullable name modifier `?` to indicate that it is nullable, and also by array name modifiers to indicate that it is an array.</span></span> <span data-ttu-id="ce7ed-236">Eles podem ser combinados, por exemplo, "`ByVal x?() As Integer`".</span><span class="sxs-lookup"><span data-stu-id="ce7ed-236">They may be combined, e.g. "`ByVal x?() As Integer`".</span></span> <span data-ttu-id="ce7ed-237">Não é permitido usar limites de matriz explícita; Além disso, se o modificador anulável nome está presente, uma `As` cláusula deve estar presente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-237">It is not allowed to use explicit array bounds; also, if the nullable name modifier is present then an `As` clause must be present.</span></span>


#### <a name="value-parameters"></a><span data-ttu-id="ce7ed-238">Parâmetros de valor</span><span class="sxs-lookup"><span data-stu-id="ce7ed-238">Value Parameters</span></span>

<span data-ttu-id="ce7ed-239">Um *parâmetro value* é declarado com uma explícita `ByVal` modificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-239">A *value parameter* is declared with an explicit `ByVal` modifier.</span></span> <span data-ttu-id="ce7ed-240">Se o `ByVal` modificador é usado, o `ByRef` modificador não pode ser especificado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-240">If the `ByVal` modifier is used, the `ByRef` modifier may not be specified.</span></span> <span data-ttu-id="ce7ed-241">Um parâmetro de valor entra em existência com a invocação do membro, o parâmetro pertence e é inicializado com o valor do argumento fornecido na invocação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-241">A value parameter comes into existence with the invocation of the member the parameter belongs to, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="ce7ed-242">Um parâmetro de valor deixa de existir após o retorno do membro.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-242">A value parameter ceases to exist upon return of the member.</span></span>

<span data-ttu-id="ce7ed-243">Um método tem permissão para atribuir novos valores para um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-243">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="ce7ed-244">Essas atribuições afetam apenas o local de armazenamento local representado pelo parâmetro de valor; eles não têm efeito sobre o argumento real fornecido na invocação do método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-244">Such assignments only affect the local storage location represented by the value parameter; they have no effect on the actual argument given in the method invocation.</span></span>

<span data-ttu-id="ce7ed-245">Um parâmetro de valor é usado quando o valor de um argumento é passado para um método e as modificações do parâmetro não afetam o argumento original.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-245">A value parameter is used when the value of an argument is passed into a method, and modifications of the parameter do not impact the original argument.</span></span> <span data-ttu-id="ce7ed-246">Um parâmetro de valor se refere a sua própria variável, que é diferente da variável do argumento correspondente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-246">A value parameter refers to its own variable, one that is distinct from the variable of the corresponding argument.</span></span> <span data-ttu-id="ce7ed-247">Essa variável é inicializada copiando o valor do argumento correspondente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-247">This variable is initialized by copying the value of the corresponding argument.</span></span> <span data-ttu-id="ce7ed-248">O exemplo a seguir mostra um método `F` que tem um parâmetro de valor denominado `p`:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-248">The following example shows a method `F` that has a value parameter named `p`:</span></span>

```vb
Module Test
    Sub F(p As Integer)
        Console.WriteLine("p = " & p)
        p += 1
    End Sub 

    Sub Main()
        Dim a As Integer = 1

        Console.WriteLine("pre: a = " & a)
        F(a)
        Console.WriteLine("post: a = " & a)
    End Sub
End Module
```

<span data-ttu-id="ce7ed-249">O exemplo produz a seguinte saída, mesmo que o parâmetro de valor `p` é modificado:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-249">The example produces the following output, even though the value parameter `p` is modified:</span></span>

```
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a><span data-ttu-id="ce7ed-250">Parâmetros de referência</span><span class="sxs-lookup"><span data-stu-id="ce7ed-250">Reference Parameters</span></span>

<span data-ttu-id="ce7ed-251">Um parâmetro de referência é um parâmetro declarado com um `ByRef` modificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-251">A reference parameter is a parameter declared with a `ByRef` modifier.</span></span> <span data-ttu-id="ce7ed-252">Se o `ByRef` modificador é especificado, o `ByVal` modificador não pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-252">If the `ByRef` modifier is specified, the `ByVal` modifier may not be used.</span></span> <span data-ttu-id="ce7ed-253">Um parâmetro de referência não cria um novo local de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-253">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="ce7ed-254">Em vez disso, um parâmetro de referência representa a variável fornecida como o argumento na invocação do método ou construtor.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-254">Instead, a reference parameter represents the variable given as the argument in the method or constructor invocation.</span></span> <span data-ttu-id="ce7ed-255">Conceitualmente, o valor de um parâmetro de referência é sempre o mesmo que a variável subjacente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-255">Conceptually, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="ce7ed-256">Parâmetros de referência atuam em dois modos, como *aliases* ou por meio das *cópia na cópia-back.*</span><span class="sxs-lookup"><span data-stu-id="ce7ed-256">Reference parameters act in two modes, either as *aliases* or through *copy-in copy-back.*</span></span>

<span data-ttu-id="ce7ed-257">__Aliases.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-257">__Aliases.__</span></span> <span data-ttu-id="ce7ed-258">Um parâmetro de referência é usado quando o parâmetro atua como um alias para um argumento fornecido pelo chamador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-258">A reference parameter is used when the parameter acts as an alias for a caller-provided argument.</span></span> <span data-ttu-id="ce7ed-259">Um parâmetro de referência não em si define uma variável, mas em vez disso, se refere à variável do argumento correspondente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-259">A reference parameter does not itself define a variable, but rather refers to the variable of the corresponding argument.</span></span> <span data-ttu-id="ce7ed-260">As modificações de um parâmetro de referência diretamente e imediatamente um impacto sobre o argumento correspondente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-260">Modifications of a reference parameter directly and immediately impact the corresponding argument.</span></span> <span data-ttu-id="ce7ed-261">O exemplo a seguir mostra um método `Swap` que tem dois parâmetros de referência:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-261">The following example shows a method `Swap` that has two reference parameters:</span></span>

```vb
Module Test
    Sub Swap(ByRef a As Integer, ByRef b As Integer)
        Dim t As Integer = a
        a = b
        b = t
    End Sub 

    Sub Main()
        Dim x As Integer = 1
        Dim y As Integer = 2

        Console.WriteLine("pre: x = " & x & ", y = " & y)
        Swap(x, y)
        Console.WriteLine("post: x = " & x & ", y = " & y)
    End Sub 
End Module
```

<span data-ttu-id="ce7ed-262">A saída do programa é:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-262">The output of the program is:</span></span>

```
pre: x = 1, y = 2
post: x = 2, y = 1
```

<span data-ttu-id="ce7ed-263">Para a invocação de método `Swap` na classe `Main`, `a` representa `x,` e `b` representa `y`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-263">For the invocation of method `Swap` in class `Main`, `a` represents `x,` and `b` represents `y`.</span></span> <span data-ttu-id="ce7ed-264">Portanto, a invocação tem o efeito de troca os valores das `x` e `y`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-264">Thus, the invocation has the effect of swapping the values of `x` and `y`.</span></span>

<span data-ttu-id="ce7ed-265">Em um método que usa parâmetros de referência, é possível que vários nomes representar o mesmo local de armazenamento:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-265">In a method that takes reference parameters, it is possible for multiple names to represent the same storage location:</span></span>

```vb
Module Test
    Private s As String

    Sub F(ByRef a As String, ByRef b As String)
        s = "One"
        a = "Two"
        b = "Three"
    End Sub

    Sub G()
        F(s, s)
    End Sub
End Module
```

<span data-ttu-id="ce7ed-266">No exemplo a invocação de método `F` na `G` passa uma referência a `s` para ambos `a` e `b`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-266">In the example the invocation of method `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="ce7ed-267">Assim, para essa invocação, os nomes `s`, `a`, e `b` todas se referem ao mesmo local de armazenamento e as todas as atribuições de três modificar a variável de instância `s`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-267">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance variable `s`.</span></span>

<span data-ttu-id="ce7ed-268">__Cópia-em-back de cópia.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-268">__Copy-in copy-back.__</span></span> <span data-ttu-id="ce7ed-269">Se o tipo da variável que está sendo passado para um parâmetro de referência não é compatível com o tipo do parâmetro de referência, ou se um não-variável (por exemplo, uma propriedade) é passada como um argumento para um parâmetro de referência, ou se a invocação é associação tardia e, em seguida, um temporário variável é alocada e passado para o parâmetro de referência.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-269">If the type of the variable being passed to a reference parameter is not compatible with the reference parameter's type, or if a non-variable (e.g. a property) is passed as an argument to a reference parameter, or if the invocation is late-bound, then a temporary variable is allocated and passed to the reference parameter.</span></span> <span data-ttu-id="ce7ed-270">O valor que está sendo passado será copiado nessa variável temporária antes que o método é invocado e será copiado novamente para a variável original (se houver um e é gravável) quando o método retorna.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-270">The value being passed in will be copied into this temporary variable before the method is invoked and will be copied back to the original variable (if there is one and if it's writable) when the method returns.</span></span> <span data-ttu-id="ce7ed-271">Assim, um parâmetro de referência não necessariamente pode conter uma referência para o armazenamento exato da variável que está sendo passado, e todas as alterações para o parâmetro de referência podem não ser refletidas na variável até que o método sai.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-271">Thus, a reference parameter may not necessarily contain a reference to the exact storage of the variable being passed in, and any changes to the reference parameter may not be reflected in the variable until the method exits.</span></span> <span data-ttu-id="ce7ed-272">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-272">For example:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(ByRef b As Base)
        b = New Base()
    End Sub

    Property G() As Base
        Get
        End Get
        Set
        End Set
    End Property

    Sub Main()
        Dim d As Derived

        F(G)   ' OK.
        F(d)   ' Throws System.InvalidCastException after F returns.
    End Sub
End Module
```

<span data-ttu-id="ce7ed-273">No caso da primeira invocação da `F`, uma variável temporária é criada e o valor da propriedade `G` é atribuído a ele e passado para `F`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-273">In the case of the first invocation of `F`, a temporary variable is created and the value of the property `G` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="ce7ed-274">Após o retorno de `F`, o valor na variável temporária é atribuído para a propriedade de `G`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-274">Upon return from `F`, the value in the temporary variable is assigned back to the property of `G`.</span></span> <span data-ttu-id="ce7ed-275">No segundo caso, a outra variável temporária é criada e o valor de `d` é atribuído a ele e passado para `F`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-275">In the second case, another temporary variable is created and the value of `d` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="ce7ed-276">Ao retornar de `F`, o valor na variável temporária é convertido de volta para o tipo da variável `Derived`e atribuídos a `d`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-276">When returning from `F`, the value in the temporary variable is cast back to the type of the variable, `Derived`, and assigned to `d`.</span></span> <span data-ttu-id="ce7ed-277">Desde o valor que está sendo passado de volta não pode ser convertido `Derived`, uma exceção será lançada em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-277">Since the value being passed back cannot be cast to `Derived`, an exception is thrown at run time.</span></span>

#### <a name="optional-parameters"></a><span data-ttu-id="ce7ed-278">Parâmetros opcionais</span><span class="sxs-lookup"><span data-stu-id="ce7ed-278">Optional Parameters</span></span>

<span data-ttu-id="ce7ed-279">Um parâmetro opcional é declarado com o `Optional` modificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-279">An optional parameter is declared with the `Optional` modifier.</span></span> <span data-ttu-id="ce7ed-280">Parâmetros que seguem um parâmetro opcional na lista de parâmetros formais também devem ser opcionais; Falha ao especificar o `Optional` modificador nos seguintes parâmetros acionará um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-280">Parameters that follow an optional parameter in the formal parameter list must be optional as well; failure to specify the `Optional` modifier on the following parameters will trigger a compile-time error.</span></span> <span data-ttu-id="ce7ed-281">Um parâmetro opcional de alguns digite tipo anulável `T?` ou tipo não anulável `T` deve especificar uma expressão constante `e` a ser usado como um valor padrão se nenhum argumento for especificado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-281">An optional parameter of some type nullable type `T?` or non-nullable type `T` must specify a constant expression `e` to be used as a default value if no argument is specified.</span></span> <span data-ttu-id="ce7ed-282">Se `e` for avaliada como `Nothing` do tipo Object, em seguida, o valor padrão a *tipo de parâmetro* será usado como o padrão para o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-282">If `e` evaluates to `Nothing` of type Object, then the default value of the *parameter type* will be used as the default for the parameter.</span></span> <span data-ttu-id="ce7ed-283">Caso contrário, `CType(e, T)` deve ser uma expressão constante e ele será interpretado como o padrão para o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-283">Otherwise, `CType(e, T)` must be a constant expression and it is taken as the default for the parameter.</span></span>

<span data-ttu-id="ce7ed-284">Os parâmetros opcionais são a única situação em que um inicializador em um parâmetro é válido.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-284">Optional parameters are the only situation in which an initializer on a parameter is valid.</span></span> <span data-ttu-id="ce7ed-285">A inicialização sempre é feita como parte da expressão de invocação, não no próprio corpo do método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-285">The initialization is always done as a part of the invocation expression, not within the method body itself.</span></span>

```vb
Module Test
    Sub F(x As Integer, Optional y As Integer = 20)
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub

    Sub Main()
        F(10)
        F(30,40)
    End Sub
End Module
```

<span data-ttu-id="ce7ed-286">A saída do programa é:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-286">The output of the program is:</span></span>

```
x = 10, y = 20
x = 30, y = 40
```

<span data-ttu-id="ce7ed-287">Parâmetros opcionais não podem ser especificados em declarações de delegado ou eventos, nem em expressões lambda.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-287">Optional parameters may not be specified in delegate or event declarations, nor in lambda expressions.</span></span>

#### <a name="paramarray-parameters"></a><span data-ttu-id="ce7ed-288">Parâmetros ParamArray</span><span class="sxs-lookup"><span data-stu-id="ce7ed-288">ParamArray Parameters</span></span>

<span data-ttu-id="ce7ed-289">`ParamArray` os parâmetros são declarados com o `ParamArray` modificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-289">`ParamArray` parameters are declared with the `ParamArray` modifier.</span></span> <span data-ttu-id="ce7ed-290">Se o `ParamArray` modificador estiver presente, o `ByVal` modificador deve ser especificado e nenhum outro parâmetro pode usar o `ParamArray` modificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-290">If the `ParamArray` modifier is present, the `ByVal` modifier must be specified, and no other parameter may use the `ParamArray` modifier.</span></span> <span data-ttu-id="ce7ed-291">O `ParamArray` tipo do parâmetro deve ser uma matriz unidimensional, e ele deve ser o último parâmetro na lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-291">The `ParamArray` parameter's type must be a one-dimensional array, and it must be the last parameter in the parameter list.</span></span>

<span data-ttu-id="ce7ed-292">Um `ParamArray` parâmetro representa um número indefinido de parâmetros do tipo do `ParamArray`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-292">A `ParamArray` parameter represents an indeterminate number of parameters of the type of the `ParamArray`.</span></span> <span data-ttu-id="ce7ed-293">Dentro do método em si, um `ParamArray` parâmetro é tratado como seu tipo declarado e não tem nenhuma semântica especial.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-293">Within the method itself, a `ParamArray` parameter is treated as its declared type and has no special semantics.</span></span> <span data-ttu-id="ce7ed-294">Um `ParamArray` parâmetro é opcional implicitamente, com um valor padrão de uma matriz unidimensional vazia do tipo do `ParamArray`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-294">A `ParamArray` parameter is implicitly optional, with a default value of an empty one-dimensional array of the type of the `ParamArray`.</span></span>

<span data-ttu-id="ce7ed-295">Um `ParamArray` permite argumentos a ser especificada em uma das duas maneiras de uma invocação de método:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-295">A `ParamArray` permits arguments to be specified in one of two ways in a method invocation:</span></span>

* <span data-ttu-id="ce7ed-296">O argumento fornecido para um `ParamArray` pode ser uma única expressão de um tipo que é ampliado para o `ParamArray` tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-296">The argument given for a `ParamArray` can be a single expression of a type that widens to the `ParamArray` type.</span></span> <span data-ttu-id="ce7ed-297">Nesse caso, o `ParamArray` funciona exatamente como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-297">In this case, the `ParamArray` acts precisely like a value parameter.</span></span>

* <span data-ttu-id="ce7ed-298">Como alternativa, a invocação pode especificar zero ou mais argumentos para o `ParamArray`, onde cada argumento é uma expressão de um tipo que é implicitamente conversível para o tipo de elemento de `ParamArray`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-298">Alternatively, the invocation can specify zero or more arguments for the `ParamArray`, where each argument is an expression of a type that is implicitly convertible to the element type of the `ParamArray`.</span></span> <span data-ttu-id="ce7ed-299">Nesse caso, a invocação cria uma instância do `ParamArray` tipo com um comprimento correspondente ao número de argumentos, inicializa os elementos da matriz de instância com os valores de argumento fornecido e usa a matriz criada recentemente a instância como o real argumento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-299">In this case, the invocation creates an instance of the `ParamArray` type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="ce7ed-300">Exceto para permitir que um número variável de argumentos em uma invocação um `ParamArray` é precisamente equivalente a um parâmetro de valor do mesmo tipo, como mostra o exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-300">Except for allowing a variable number of arguments in an invocation, a `ParamArray` is precisely equivalent to a value parameter of the same type, as the following example illustrates.</span></span>

```vb
Module Test
    Sub F(ParamArray args() As Integer)
        Dim i As Integer

        Console.Write("Array contains " & args.Length & " elements:")
        For Each i In args
            Console.Write(" " & i)
        Next i
        Console.WriteLine()
    End Sub

    Sub Main()
        Dim a As Integer() = { 1, 2, 3 }

        F(a)
        F(10, 20, 30, 40)
        F()
    End Sub
End Module
```

<span data-ttu-id="ce7ed-301">O exemplo produz a saída</span><span class="sxs-lookup"><span data-stu-id="ce7ed-301">The example produces the output</span></span>

```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="ce7ed-302">A primeira invocação da `F` simplesmente passa a matriz `a` como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-302">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="ce7ed-303">A segunda chamada de `F` automaticamente cria uma matriz de quatro elementos com os valores do elemento fornecido e passa essa instância de matriz como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-303">The second invocation of `F` automatically creates a four-element array with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="ce7ed-304">Da mesma forma, a terceira invocação de `F` cria uma matriz de elemento zero e passa essa instância como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-304">Likewise, the third invocation of `F` creates a zero-element array and passes that instance as a value parameter.</span></span> <span data-ttu-id="ce7ed-305">As invocações de segunda e terceira são precisamente equivalentes a gravação:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-305">The second and third invocations are precisely equivalent to writing:</span></span>

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

<span data-ttu-id="ce7ed-306">`ParamArray` parâmetros não podem ser especificados em declarações de delegado ou evento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-306">`ParamArray` parameters may not be specified in delegate or event declarations.</span></span>

### <a name="event-handling"></a><span data-ttu-id="ce7ed-307">Tratamento de Evento</span><span class="sxs-lookup"><span data-stu-id="ce7ed-307">Event Handling</span></span>

<span data-ttu-id="ce7ed-308">Métodos declarativamente podem manipular eventos gerados por objetos na instância ou variáveis compartilhadas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-308">Methods can declaratively handle events raised by objects in instance or shared variables.</span></span> <span data-ttu-id="ce7ed-309">Para manipular eventos, uma declaração de método Especifica o `Handles` palavra-chave e um ou mais eventos de lista.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-309">To handle events, a method declaration specifies the `Handles` keyword and lists one or more events.</span></span>

```antlr
HandlesClause
    : ( 'Handles' EventHandlesList )?
    ;

EventHandlesList
    : EventMemberSpecifier ( Comma EventMemberSpecifier )*
    ;

EventMemberSpecifier
    : Identifier Period IdentifierOrKeyword
    | 'MyBase' Period IdentifierOrKeyword
    | 'MyClass' Period IdentifierOrKeyword
    | 'Me' Period IdentifierOrKeyword
    ;
```

<span data-ttu-id="ce7ed-310">Um evento no `Handles` lista é especificada por dois identificadores separados por um período:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-310">An event in the `Handles` list is specified by two identifiers separated by a period:</span></span>

* <span data-ttu-id="ce7ed-311">O identificador da primeira deve ser uma instância ou uma variável compartilhada no tipo recipiente que especifica o `WithEvents` modificador ou o `MyBase` ou `MyClass` ou `Me` palavra-chave; caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-311">The first identifier must be an instance or shared variable in the containing type that specifies the `WithEvents` modifier or the `MyBase` or `MyClass` or `Me` keyword; otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="ce7ed-312">Essa variável contém o objeto que irá gerar os eventos manipulados por este método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-312">This variable contains the object that will raise the events handled by this method.</span></span>

* <span data-ttu-id="ce7ed-313">O identificador do segundo deve especificar um membro do tipo do identificador do primeiro.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-313">The second identifier must specify a member of the type of the first identifier.</span></span> <span data-ttu-id="ce7ed-314">O membro deve ser um evento e pode ser compartilhado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-314">The member must be an event, and may be shared.</span></span> <span data-ttu-id="ce7ed-315">Se uma variável compartilhada for especificada para o identificador do primeiro, em seguida, o evento deve ser compartilhado ou ocorrerá um erro.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-315">If a shared variable is specified for the first identifier, then the event must be shared, or an error results.</span></span>

<span data-ttu-id="ce7ed-316">Um método de manipulador `M` é considerado um manipulador de eventos válidos para um evento `E` se a instrução `AddHandler E, AddressOf M` também seria válida.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-316">A handler method `M` is considered a valid event handler for an event `E` if the statement `AddHandler E, AddressOf M` would also be valid.</span></span> <span data-ttu-id="ce7ed-317">Ao contrário de um `AddHandler` instrução, no entanto, os manipuladores de eventos explícito permitem manipulando um evento com um método sem nenhum argumento, independentemente se a semântica estrita está sendo usada ou não:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-317">Unlike an `AddHandler` statement, however, explicit event handlers allow handling an event with a method with no arguments regardless of whether strict semantics are being used or not:</span></span>

```vb
Option Strict On

Class C1
    Event E(x As Integer)
End Class

Class C2
    withEvents C1 As New C1()

    ' Valid
    Sub M1() Handles C1.E
    End Sub

    Sub M2()
        ' Invalid
        AddHandler C1.E, AddressOf M1
    End Sub
End Class
```

<span data-ttu-id="ce7ed-318">Um único membro pode manipular vários eventos correspondentes e vários métodos podem lidar com um único evento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-318">A single member can handle multiple matching events, and multiple methods may handle a single event.</span></span> <span data-ttu-id="ce7ed-319">Acessibilidade de um método não tem nenhum efeito em sua capacidade de manipular eventos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-319">A method's accessibility has no effect on its ability to handle events.</span></span> <span data-ttu-id="ce7ed-320">O exemplo a seguir mostra como um método pode manipular eventos:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-320">The following example shows how a method can handle events:</span></span>

```vb
Class Raiser
    Event E1()

    Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    WithEvents x As Raiser

    Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
        x.Raise()
        x.Raise()
    End Sub
End Module
```

<span data-ttu-id="ce7ed-321">Isso imprimirá:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-321">This will print out:</span></span>

```
Raised
Raised
```

<span data-ttu-id="ce7ed-322">Um tipo herda todos os manipuladores de eventos fornecidos pelo seu tipo base.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-322">A type inherits all event handlers provided by its base type.</span></span> <span data-ttu-id="ce7ed-323">Um tipo derivado não é possível, de forma alguma, alterar os mapeamentos de evento herda de seus tipos base, mas pode adicionar manipuladores adicionais para o evento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-323">A derived type cannot in any way alter the event mappings it inherits from its base types, but may add additional handlers to the event.</span></span>


### <a name="extension-methods"></a><span data-ttu-id="ce7ed-324">Métodos de extensão</span><span class="sxs-lookup"><span data-stu-id="ce7ed-324">Extension Methods</span></span>

<span data-ttu-id="ce7ed-325">Métodos podem ser adicionados aos tipos de fora a declaração de tipo usando *métodos de extensão*.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-325">Methods can be added to types from outside of the type declaration using *extension methods*.</span></span> <span data-ttu-id="ce7ed-326">Métodos de extensão são métodos com o `System.Runtime.CompilerServices.ExtensionAttribute` atributo aplicado a eles.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-326">Extension methods are methods with the `System.Runtime.CompilerServices.ExtensionAttribute` attribute applied to them.</span></span> <span data-ttu-id="ce7ed-327">Eles só podem ser declarados em módulos padrão e devem ter pelo menos um parâmetro, que especifica que o tipo o método estende.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-327">They can only be declared in standard modules and must have at least one parameter, which specifies the type the method extends.</span></span> <span data-ttu-id="ce7ed-328">Por exemplo, o seguinte método de extensão estende o tipo `String`:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-328">For example, the following extension method extends the type `String`:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

<span data-ttu-id="ce7ed-329">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-329">__Note.__</span></span> <span data-ttu-id="ce7ed-330">Embora o Visual Basic exige os métodos de extensão deve ser declarado em um módulo padrão, outras linguagens como c# podem permitir que eles sejam declaradas em outros tipos de tipos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-330">Although Visual Basic requires extension methods to be declared in a standard module, other languages such as C# may allow them to be declared in other kinds of types.</span></span> <span data-ttu-id="ce7ed-331">Desde que os métodos seguem as outras convenções descritas aqui e que o contém tipo não é um tipo genérico aberto e não pode ser instanciado, o Visual Basic reconhecerá os métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-331">As long as the methods follow the other conventions outlined here and the containing type is not an open generic type and cannot be instantiated, Visual Basic will recognize the extension methods.</span></span>

<span data-ttu-id="ce7ed-332">Quando um método de extensão é chamado, ele está sendo invocado na instância é passada para o primeiro parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-332">When an extension method is invoked, the instance it is being invoked on is passed to the first parameter.</span></span> <span data-ttu-id="ce7ed-333">O primeiro parâmetro não pode ser declarado `Optional` ou `ParamArray`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-333">The first parameter cannot be declared `Optional` or `ParamArray`.</span></span> <span data-ttu-id="ce7ed-334">Qualquer tipo, incluindo um parâmetro de tipo, pode aparecer como o primeiro parâmetro de um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-334">Any type, including a type parameter, can appear as the first parameter of an extension method.</span></span> <span data-ttu-id="ce7ed-335">Por exemplo, os seguintes métodos estendem os tipos `Integer()`, qualquer tipo que implemente `System.Collections.Generic.IEnumerable(Of T)`, e qualquer tipo em todos os:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-335">For example, the following methods extend the types `Integer()`, any type that implements `System.Collections.Generic.IEnumerable(Of T)`, and any type at all:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Extensions
    <Extension> _
    Sub PrintArray(a() As Integer)
        ...
    End Sub

    <Extension> _
    Sub PrintList(Of T)(a As IEnumerable(Of T))
        ...
    End Sub

    <Extension> _
    Sub Print(Of T)(a As T)
        ...
    End Sub
End Module
```

<span data-ttu-id="ce7ed-336">Como mostra o exemplo anterior, as interfaces podem ser estendidos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-336">As the previous example shows, interfaces can be extended.</span></span> <span data-ttu-id="ce7ed-337">Métodos de extensão de interface fornecem a implementação do método, para que os tipos que implementam uma interface que tem métodos de extensão definidos nele ainda apenas implementam os membros declarados originalmente pela interface.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-337">Interface extension methods supply the implementation of the method, so types that implement an interface that has extension methods defined on it still only implement the members originally declared by the interface.</span></span> <span data-ttu-id="ce7ed-338">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-338">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Interface IAction
  Sub DoAction()
End Interface

Module IActionExtensions 
    <Extension> _
    Public Sub DoAnotherAction(i As IAction) 
        i.DoAction()
    End Sub
End Module

Class C
  Implements IAction

  Sub DoAction() Implements IAction.DoAction
    ...
  End Sub

  ' ERROR: Cannot implement extension method IAction.DoAnotherAction
  Sub DoAnotherAction() Implements IAction.DoAnotherAction
    ...
  End Sub
End Class
```

<span data-ttu-id="ce7ed-339">Métodos de extensão também podem ter restrições de tipo em seus parâmetros de tipo e, assim como com métodos de não-extensão genéricos, argumento de tipo pode ser inferido:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-339">Extension methods can also have type constraints on their type parameters and, just as with non-extension generic methods, type argument can be inferred:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module IEnumerableComparableExtensions
    <Extension> _
    Public Function Sort(Of T As IComparable(Of T))(i As IEnumerable(Of T)) _
        As IEnumerable(Of T)
        ...
    End Function
End Module
```

<span data-ttu-id="ce7ed-340">Métodos de extensão também podem ser acessados por meio de expressões de instância implícita dentro do tipo que está sendo estendido:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-340">Extension methods can also be accessed through implicit instance expressions within the type being extended:</span></span>

```vb
Imports System.Runtime.CompilerServices

Class C1
    Sub M1()
        Me.M2()
        M2()
    End Sub
End Class

Module C1Extensions
    <Extension>
    Sub M2(c As C1)
        ...
    End Sub
End Module
```

<span data-ttu-id="ce7ed-341">Para fins de acessibilidade, os métodos de extensão também são tratados como membros do módulo padrão que são declarados, eles não têm mais acesso aos membros do tipo que estão estendendo além de acesso eles têm em virtude de seu contexto de declaração.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-341">For the purposes of accessibility, extension methods are also treated as members of the standard module they are declared in -- they have no extra access to the members of the type they are extending beyond the access they have by virtue of their declaration context.</span></span>

<span data-ttu-id="ce7ed-342">Métodos de extensão estão disponíveis somente quando o método de módulo padrão está no escopo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-342">Extensions methods are only available when the standard module method is in scope.</span></span> <span data-ttu-id="ce7ed-343">Caso contrário, o tipo original não será exibida para foram estendidos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-343">Otherwise, the original type will not appear to have been extended.</span></span> <span data-ttu-id="ce7ed-344">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-344">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Namespace N1
    Module C1Extensions
        <Extension> _
        Sub M1(c As C1)
            ...
        End Sub
    End Module
End Namespace

Module Test
    Sub Main()
        Dim c As New C1()

        ' Error: c has no member named "M1"
        c.M1()
    End Sub
End Module
```

<span data-ttu-id="ce7ed-345">Referindo-se a um tipo quando apenas um método de extensão no tipo está disponível ainda produzirá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-345">Referring to a type when only an extension method on the type is available will still produce a compile-time error.</span></span>

<span data-ttu-id="ce7ed-346">É importante observar que os métodos de extensão são considerados membros do tipo em todos os contextos em que os membros são associados, como fortemente tipado `For Each` padrão.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-346">It is important to note that extension methods are considered to be members of the type in all contexts where members are bound, such as the strongly-typed `For Each` pattern.</span></span> <span data-ttu-id="ce7ed-347">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-347">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Class C1Enumerator
    ReadOnly Property Current() As C1
        Get
            ...
        End Get
    End Property

    Function MoveNext() As Boolean
        ...
    End Function
End Class

Module C1Extensions
    <Extension> _
    Function GetEnumerator(c As C1) As C1Enumerator
        ...
    End Function
End Module

Module Test
    Sub Main()
        Dim c As New C1()

        ' Valid
        For Each o As Object In c
            ...
        Next o
    End Sub
End Module
```

<span data-ttu-id="ce7ed-348">Delegados também podem ser criados que se referem aos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-348">Delegates can also be created that refer to extension methods.</span></span> <span data-ttu-id="ce7ed-349">Portanto, o código:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-349">Thus, the code:</span></span>

```vb
Delegate Sub D1()

Module Test
    Sub Main()
        Dim s As String = "Hello, World!"
        Dim d As D1

        d = AddressOf s.Print
        d()
    End Sub
End Module
```

<span data-ttu-id="ce7ed-350">É aproximadamente equivalente a:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-350">is roughly equivalent to:</span></span>

```vb
Delegate Sub D1()

Module Test
    Sub Main()
      Dim s As String = "Hello, World!"
      Dim d As D1

      d = CType([Delegate].CreateDelegate(GetType(D1), s, _
                GetType(StringExtensions).GetMethod("Print")), D1)
      d()
    End Sub
End Module
```

<span data-ttu-id="ce7ed-351">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-351">__Note.__</span></span> <span data-ttu-id="ce7ed-352">Normalmente, o Visual Basic insere uma verificação em uma chamada de método de instância que faz com que um `System.NullReferenceException` ocorra se a instância que o método é invocado em é `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-352">Visual Basic normally inserts a check on an instance method call that causes a `System.NullReferenceException` to occur if the instance the method is being invoked on is `Nothing`.</span></span> <span data-ttu-id="ce7ed-353">No caso de métodos de extensão, não há nenhuma maneira eficiente para inserir essa verificação, portanto, serão necessário verificar explicitamente os métodos de extensão `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-353">In the case of extension methods, there is no efficient way to insert this check, so extension methods will need to explicitly check for `Nothing`.</span></span> 

<span data-ttu-id="ce7ed-354">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-354">__Note.__</span></span> <span data-ttu-id="ce7ed-355">Um tipo de valor será ser boxed quando está sendo passado como um `ByVal` argumento para um parâmetro digitado como uma interface.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-355">A value type will be boxed when being passed as a `ByVal` argument to a parameter typed as an interface.</span></span>  <span data-ttu-id="ce7ed-356">Isso implica que os efeitos colaterais do método de extensão funcionará em uma cópia da estrutura, em vez do original.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-356">This implies that side effects of the extension method will operate on a copy of the structure instead of the original.</span></span> <span data-ttu-id="ce7ed-357">Embora a linguagem não coloca nenhuma restrição sobre o primeiro argumento de um método de extensão, é recomendável que os métodos de extensão não são usados para estender os tipos de valor ou que, ao estender tipos de valor, o primeiro parâmetro é passado `ByRef` para garantir que esse lado efeitos de operam no valor original.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-357">While the language puts no restrictions on the first argument of an extension method, it is recommended that extension methods are not used to extend value types or that when extending value types, the first parameter is passed `ByRef` to ensure that side effects operate on the original value.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="ce7ed-358">Métodos parciais</span><span class="sxs-lookup"><span data-stu-id="ce7ed-358">Partial Methods</span></span>

<span data-ttu-id="ce7ed-359">Um *método parcial* é um método que especifica uma assinatura, mas não o corpo do método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-359">A *partial method* is a method that specifies a signature but not the body of the method.</span></span> <span data-ttu-id="ce7ed-360">O corpo do método pode ser fornecido por outra declaração de método com o mesmo nome e assinatura, muito provavelmente na outra declaração parcial do tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-360">The body of the method can be supplied by another method declaration with the same name and signature, most likely in another partial declaration of the type.</span></span> <span data-ttu-id="ce7ed-361">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-361">For example:</span></span>

<span data-ttu-id="ce7ed-362">a.vb:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-362">a.vb:</span></span>

```vb
' Designer generated code
Public Partial Class MyForm
    Private Partial Sub ValidateControls()
    End Sub

    Public Sub New()
        ' Initialize controls
        ...

        ValidateControls()
    End Sub    
End Class
```

<span data-ttu-id="ce7ed-363">b.vb:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-363">b.vb:</span></span>

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

<span data-ttu-id="ce7ed-364">Neste exemplo, uma declaração parcial da classe `MyForm` declara um método parcial `ValidateControls` sem implementação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-364">In this example, a partial declaration of the class `MyForm` declares a partial method `ValidateControls` with no implementation.</span></span> <span data-ttu-id="ce7ed-365">O construtor na declaração parcial chama o método parcial, mesmo que não há nenhum corpo fornecido no arquivo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-365">The constructor in the partial declaration calls the partial method, even though there is no body supplied in the file.</span></span> <span data-ttu-id="ce7ed-366">A declaração parcial de `MyForm` , em seguida, fornece a implementação do método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-366">The other partial declaration of `MyForm` then supplies the implementation of the method.</span></span>

<span data-ttu-id="ce7ed-367">Métodos parciais podem ser chamados, independentemente se um corpo foi fornecido; não se for fornecido nenhum corpo de método, a chamada será ignorada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-367">Partial methods can be called regardless of whether a body has been supplied; if no method body is supplied, the call is ignored.</span></span> <span data-ttu-id="ce7ed-368">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-368">For example:</span></span>

```vb
Public Class C1
    Private Partial Sub M1()
    End Sub

    Public Sub New()
        ' Since no implementation is supplied, this call will not be made.
        M1()
    End Sub
End Class
```

<span data-ttu-id="ce7ed-369">Ignorado também todas as expressões que são passadas como argumentos para uma chamada de método parcial que é ignorada e não avaliadas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-369">Any expressions that are passed in as arguments to a partial method call that is ignored are ignored also and not evaluated.</span></span> <span data-ttu-id="ce7ed-370">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-370">(__Note.__</span></span> <span data-ttu-id="ce7ed-371">Isso significa que os métodos parciais são uma maneira muito eficiente de fornecer o comportamento que é definido entre dois tipos parciais, como os métodos parciais têm sem custos se elas não são usadas.)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-371">This means that partial methods are a very efficient way of providing behavior that is defined across two partial types, since the partial methods have no cost if they are not used.)</span></span>

<span data-ttu-id="ce7ed-372">Declaração de método parcial deve ser declarada como `Private` e deve ser sempre uma sub-rotina sem declarações em seu corpo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-372">The partial method declaration must be declared as `Private` and must always be a subroutine with no statements in its body.</span></span> <span data-ttu-id="ce7ed-373">Métodos parciais não podem se implementar métodos de interface, embora o método que fornece o corpo pode.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-373">Partial methods cannot themselves implement interface methods, although the method that supplies their body can.</span></span>

<span data-ttu-id="ce7ed-374">Somente um método pode fornecer um corpo de um método parcial.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-374">Only one method can supply a body to a partial method.</span></span> <span data-ttu-id="ce7ed-375">Um método fornecendo um corpo de um método parcial deve ter a mesma assinatura que o método parcial, as mesmas restrições sobre quaisquer parâmetros de tipo, mesmo modificadores de declaração e o mesmo parâmetro e os nomes de parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-375">A method supplying a body to a partial method must have the same signature as the partial method, the same constraints on any type parameters, the same declaration modifiers, and the same parameter and type parameter names.</span></span> <span data-ttu-id="ce7ed-376">Atributos de método parcial e o método que fornece seu corpo são mesclados, assim como todos os atributos nos parâmetros dos métodos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-376">Attributes on the partial method and the method that supplies its body are merged, as are any attributes on the methods' parameters.</span></span> <span data-ttu-id="ce7ed-377">Da mesma forma, a lista de eventos que lidam com os métodos é mesclada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-377">Similarly, the list of events that the methods handle is merged.</span></span> <span data-ttu-id="ce7ed-378">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-378">For example:</span></span>

```vb
Class C1
    Event E1()
    Event E2()

    Private Partial Sub S() Handles Me.E1
    End Sub

    ' Handles both E1 and E2
    Private Sub S() Handles Me.E2
        ...
    End Sub
End Class
```

## <a name="constructors"></a><span data-ttu-id="ce7ed-379">Construtores</span><span class="sxs-lookup"><span data-stu-id="ce7ed-379">Constructors</span></span>

<span data-ttu-id="ce7ed-380">*Construtores* são métodos especiais que permitem o controle sobre a inicialização.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-380">*Constructors* are special methods that allow control over initialization.</span></span> <span data-ttu-id="ce7ed-381">Eles são executados depois que o programa é iniciado ou quando uma instância de um tipo é criada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-381">They are run after the program begins or when an instance of a type is created.</span></span> <span data-ttu-id="ce7ed-382">Ao contrário de outros membros, construtores não são herdados e não introduzem um nome para o espaço de declaração de um tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-382">Unlike other members, constructors are not inherited and do not introduce a name into a type's declaration space.</span></span> <span data-ttu-id="ce7ed-383">Construtores podem ser invocados somente por expressões de criação do objeto ou do .NET Framework. eles podem nunca ser chamados diretamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-383">Constructors may only be invoked by object-creation expressions or by the .NET Framework; they may never be directly invoked.</span></span>

<span data-ttu-id="ce7ed-384">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-384">__Note.__</span></span> <span data-ttu-id="ce7ed-385">Construtores têm a mesma restrição no posicionamento de linha que tem as sub-rotinas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-385">Constructors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="ce7ed-386">A instrução de início, a instrução end e o bloco devem aparecer no início de uma linha lógica.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-386">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
ConstructorMemberDeclaration
    : Attributes? ConstructorModifier* 'Sub' 'New'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Sub' StatementTerminator
    ;

ConstructorModifier
    : AccessModifier
    | 'Shared'
    ;
```

### <a name="instance-constructors"></a><span data-ttu-id="ce7ed-387">Construtores de instâncias</span><span class="sxs-lookup"><span data-stu-id="ce7ed-387">Instance Constructors</span></span>

<span data-ttu-id="ce7ed-388">*Construtores de instância* inicializar instâncias de um tipo e são executados pelo .NET Framework quando uma instância é criada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-388">*Instance constructors* initialize instances of a type and are run by the .NET Framework when an instance is created.</span></span> <span data-ttu-id="ce7ed-389">A lista de parâmetros de um construtor está sujeito às mesmas regras que a lista de parâmetros de um método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-389">The parameter list of a constructor is subject to the same rules as the parameter list of a method.</span></span> <span data-ttu-id="ce7ed-390">Construtores de instância podem estar sobrecarregados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-390">Instance constructors may be overloaded.</span></span>

<span data-ttu-id="ce7ed-391">Todos os construtores em tipos de referência devem chamar outro construtor.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-391">All constructors in reference types must invoke another constructor.</span></span> <span data-ttu-id="ce7ed-392">Se a invocação explícita, ele deve ser a primeira instrução no corpo do método de construtor.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-392">If the invocation is explicit, it must be the first statement in the constructor method body.</span></span> <span data-ttu-id="ce7ed-393">A instrução pode invocar outro dos construtores de instância do tipo – por exemplo, `Me.New(...)` ou `MyClass.New(...)` – ou se ele não é uma estrutura, ela pode invocar um construtor de instância do tipo base do tipo – por exemplo, `MyBase.New(...)`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-393">The statement can either invoke another of the type's instance constructors -- for example, `Me.New(...)` or `MyClass.New(...)` -- or if it is not a structure it can invoke an instance constructor of the type's base type -- for example, `MyBase.New(...)`.</span></span> <span data-ttu-id="ce7ed-394">Não é válido para um construtor chamar a mesmo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-394">It is invalid for a constructor to invoke itself.</span></span> <span data-ttu-id="ce7ed-395">Se um construtor omite uma chamada para outro construtor, `MyBase.New()` é implícito.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-395">If a constructor omits a call to another constructor, `MyBase.New()` is implicit.</span></span> <span data-ttu-id="ce7ed-396">Se não houver nenhum construtor sem parâmetros de tipo base, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-396">If there is no parameterless base type constructor, a compile-time error occurs.</span></span> <span data-ttu-id="ce7ed-397">Porque `Me` não é considerado ser construído até após a chamada para um construtor de classe base, os parâmetros para uma instrução de invocação do construtor não podem referenciar `Me`, `MyClass`, ou `MyBase` implícita ou explicitamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-397">Because `Me` is not considered to be constructed until after the call to a base class constructor, the parameters to a constructor invocation statement cannot reference `Me`, `MyClass`, or `MyBase` implicitly or explicitly.</span></span>

<span data-ttu-id="ce7ed-398">Quando a primeira instrução de um construtor é da forma `MyBase.New(...)`, o construtor implicitamente executa as inicializações especificadas pelos inicializadores de variável das variáveis de instância declarados no tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-398">When a constructor's first statement is of the form `MyBase.New(...)`, the constructor implicitly performs the initializations specified by the variable initializers of the instance variables declared in the type.</span></span> <span data-ttu-id="ce7ed-399">Isso corresponde a uma sequência de atribuições que são executadas imediatamente depois de invocar o construtor do tipo base direta.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-399">This corresponds to a sequence of assignments that are executed immediately after invoking the direct base type constructor.</span></span> <span data-ttu-id="ce7ed-400">Essa ordenação garante que todas as variáveis de instância de base são inicializadas por seus inicializadores de variável antes de quaisquer declarações que têm acesso à instância são executadas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-400">Such ordering ensures that all base instance variables are initialized by their variable initializers before any statements that have access to the instance are executed.</span></span> <span data-ttu-id="ce7ed-401">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-401">For example:</span></span>

```vb
Class A
    Protected x As Integer = 1
End Class

Class B
    Inherits A

    Private y As Integer = x

    Public Sub New()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Class
```

<span data-ttu-id="ce7ed-402">Quando `New B()` é usado para criar uma instância de `B`, a seguinte saída é produzida:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-402">When `New B()` is used to create an instance of `B`, the following output is produced:</span></span>

```
x = 1, y = 1
```

<span data-ttu-id="ce7ed-403">O valor de `y` é `1` porque o inicializador de variável é executado depois que o construtor de classe base será invocado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-403">The value of `y` is `1` because the variable initializer is executed after the base class constructor is invoked.</span></span> <span data-ttu-id="ce7ed-404">Inicializadores de variável são executados na ordem textual aparecem na declaração de tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-404">Variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="ce7ed-405">Quando um tipo declara apenas `Private` construtores, não é possível em geral para outros tipos derivam do tipo ou criar instâncias do tipo; a única exceção é tipos aninhados dentro do tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-405">When a type declares only `Private` constructors, it is not possible in general for other types to derive from the type or create instances of the type; the only exception is types nested within the type.</span></span> <span data-ttu-id="ce7ed-406">`Private` construtores são comumente usados em tipos que contêm apenas `Shared` membros.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-406">`Private` constructors are commonly used in types that contain only `Shared` members.</span></span>

<span data-ttu-id="ce7ed-407">Se um tipo não contém nenhuma declaração de construtor de instância, um construtor padrão é fornecido automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-407">If a type contains no instance constructor declarations, a default constructor is automatically provided.</span></span> <span data-ttu-id="ce7ed-408">O construtor padrão simplesmente invoca o construtor sem parâmetros do tipo base direto.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-408">The default constructor simply invokes the parameterless constructor of the direct base type.</span></span> <span data-ttu-id="ce7ed-409">Se o tipo base direto não tem um construtor sem parâmetros acessível, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-409">If the direct base type does not have an accessible parameterless constructor, a compile-time error occurs.</span></span> <span data-ttu-id="ce7ed-410">O tipo de acesso declarado para o construtor padrão é `Public` , a menos que o tipo é `MustInherit`, caso em que o construtor padrão é `Protected`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-410">The declared access type for the default constructor is `Public` unless the type is `MustInherit`, in which case the default constructor is `Protected`.</span></span>

<span data-ttu-id="ce7ed-411">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-411">__Note.__</span></span> <span data-ttu-id="ce7ed-412">O acesso padrão para um `MustInherit` é o construtor padrão do tipo `Protected` porque `MustInherit` classes não podem ser criadas diretamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-412">The default access for a `MustInherit` type's default constructor is `Protected` because `MustInherit` classes cannot be created directly.</span></span> <span data-ttu-id="ce7ed-413">Portanto, não há nenhum ponto de tornar o construtor padrão `Public`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-413">So there is no point in making the default constructor `Public`.</span></span>

<span data-ttu-id="ce7ed-414">No exemplo a seguir, um construtor padrão é fornecido porque a classe não contém nenhuma declaração de construtor:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-414">In the following example a default constructor is provided because the class contains no constructor declarations:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

<span data-ttu-id="ce7ed-415">Assim, o exemplo é precisamente equivalente ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-415">Thus, the example is precisely equivalent to the following:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

<span data-ttu-id="ce7ed-416">Construtores padrão que são emitidos em um designer gerado classe marcada com o atributo `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` chamará o método `Sub InitializeComponent()`, se ele existir, após a chamada para o construtor base.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-416">Default constructors that are emitted into a designer generated class marked with the attribute `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` will call the method `Sub InitializeComponent()`, if it exists, after the call to the base constructor.</span></span> <span data-ttu-id="ce7ed-417">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-417">(__Note.__</span></span> <span data-ttu-id="ce7ed-418">Isso permite que os arquivos gerados designer, como aqueles criados pelo criador do WinForms, omita o construtor no arquivo do designer.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-418">This allows designer generated files, such as those created by the WinForms designer, to omit the constructor in the designer file.</span></span> <span data-ttu-id="ce7ed-419">Isso permite que o programador deve especificá-lo por conta própria, se desejarem.)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-419">This enables the programmer to specify it themselves, if they so choose.)</span></span>

### <a name="shared-constructors"></a><span data-ttu-id="ce7ed-420">Construtores de compartilhado</span><span class="sxs-lookup"><span data-stu-id="ce7ed-420">Shared Constructors</span></span>

<span data-ttu-id="ce7ed-421">*Construtores compartilhados* inicializar um tipo compartilhado variáveis; eles são executados depois que o programa começa a ser executado, mas antes de quaisquer referências a um membro do tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-421">*Shared constructors* initialize a type's shared variables; they are run after the program begins executing, but before any references to a member of the type.</span></span> <span data-ttu-id="ce7ed-422">Um construtor compartilhado Especifica o `Shared` modificador, a menos que ele está em um módulo padrão, caso em que o `Shared` modificador é implícita.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-422">A shared constructor specifies the `Shared` modifier, unless it is in a standard module in which case the `Shared` modifier is implied.</span></span>

<span data-ttu-id="ce7ed-423">Ao contrário de construtores de instância, construtores compartilhados tem acesso implícito de público, não têm parâmetros e não podem chamar outros construtores.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-423">Unlike instance constructors, shared constructors have implicit public access, have no parameters, and may not call other constructors.</span></span> <span data-ttu-id="ce7ed-424">Antes da primeira instrução em um construtor compartilhado, o construtor compartilhado implicitamente executa as inicializações especificadas pelos inicializadores de variável das variáveis compartilhadas declarados no tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-424">Before the first statement in a shared constructor, the shared constructor implicitly performs the initializations specified by the variable initializers of the shared variables declared in the type.</span></span> <span data-ttu-id="ce7ed-425">Isso corresponde a uma sequência de atribuições que são executados imediatamente após a entrada para o construtor.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-425">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor.</span></span> <span data-ttu-id="ce7ed-426">Os inicializadores de variável são executados na ordem textual aparecem na declaração de tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-426">The variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="ce7ed-427">A exemplo a seguir mostra um `Employee` classe com um construtor compartilhado que inicializa uma variável compartilhada:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-427">The following example shows an `Employee` class with a shared constructor that initializes a shared variable:</span></span>

```vb
Imports System.Data

Class Employee
    Private Shared ds As DataSet

    Shared Sub New()
        ds = New DataSet()
    End Sub

    Public Name As String
    Public Salary As Decimal
End Class
```

<span data-ttu-id="ce7ed-428">Existe um construtor compartilhado separado para cada tipo genérico fechado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-428">A separate shared constructor exists for each closed generic type.</span></span> <span data-ttu-id="ce7ed-429">Porque o construtor compartilhado é executado exatamente uma vez para cada tipo fechado, ele é um local conveniente para impor verificações de tempo de execução no parâmetro de tipo não podem ser verificados em tempo de compilação por meio de restrições.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-429">Because the shared constructor is executed exactly once for each closed type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints.</span></span> <span data-ttu-id="ce7ed-430">Por exemplo, o tipo a seguir usa um construtor compartilhado para impor que o parâmetro de tipo é `Integer` ou `Double`:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-430">For example, the following type uses a shared constructor to enforce that the type parameter is `Integer` or `Double`:</span></span>

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

<span data-ttu-id="ce7ed-431">Exatamente quando os construtores compartilhados são executados é principalmente implementação depende, embora várias garantias são fornecidas, se um construtor compartilhado é definido explicitamente:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-431">Exactly when shared constructors are run is mostly implementation dependent, though several guarantees are provided if a shared constructor is explicitly defined:</span></span>

* <span data-ttu-id="ce7ed-432">Construtores compartilhados são executados antes do primeiro acesso a qualquer campo estático do tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-432">Shared constructors are run before the first access to any static field of the type.</span></span>

* <span data-ttu-id="ce7ed-433">Construtores compartilhados são executados antes da primeira invocação de qualquer método estático do tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-433">Shared constructors are run before the first invocation of any static method of the type.</span></span>

* <span data-ttu-id="ce7ed-434">Construtores compartilhados são executados antes da primeira invocação de qualquer construtor para o tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-434">Shared constructors are run before the first invocation of any constructor for the type.</span></span>

<span data-ttu-id="ce7ed-435">As garantias acima não se aplicam a situação em que um construtor compartilhado é implicitamente criado para inicializadores compartilhados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-435">The above guarantees do not apply in the situation where a shared constructor is implicitly created for shared initializers.</span></span> <span data-ttu-id="ce7ed-436">A saída do exemplo a seguir é incerta, porque a ordem exata de carregamento e, portanto, do construtor compartilhado execução não foi definida:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-436">The output from the following example is uncertain, because the exact ordering of loading and therefore of shared constructor execution is not defined:</span></span>

```vb
Module Test
    Sub Main()
        A.F()
        B.F()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("A.F")
    End Sub
End Class

Class B
    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("B.F")
    End Sub
End Class
```

<span data-ttu-id="ce7ed-437">A saída pode ser um destes procedimentos:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-437">The output could be either of the following:</span></span>

```
Init A
A.F
Init B
B.F
```

<span data-ttu-id="ce7ed-438">ou</span><span class="sxs-lookup"><span data-stu-id="ce7ed-438">or</span></span>

```
Init B
Init A
A.F
B.F
```

<span data-ttu-id="ce7ed-439">Por outro lado, o exemplo a seguir produz saída previsível.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-439">By contrast, the following example produces predictable output.</span></span> <span data-ttu-id="ce7ed-440">Observe que o `Shared` construtor da classe `A` nunca é executado, mesmo que classe `B` deriva dela:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-440">Note that the `Shared` constructor for the class `A` never executes, even though class `B` derives from it:</span></span>

```vb
Module Test
    Sub Main()
        B.G()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub
End Class

Class B
    Inherits A

    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub G()
        Console.WriteLine("B.G")
    End Sub
End Class
```

<span data-ttu-id="ce7ed-441">A saída é:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-441">The output is:</span></span>

```
Init B
B.G
```

<span data-ttu-id="ce7ed-442">Também é possível criar dependências circulares que permitem `Shared` variáveis com inicializadores de variável a ser observada no seu padrão valor de estado, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-442">It is also possible to construct circular dependencies that allow `Shared` variables with variable initializers to be observed in their default value state, as in the following example:</span></span>

```vb
Class A
    Public Shared X As Integer = B.Y + 1
End Class

Class B
    Public Shared Y As Integer = A.X + 1

    Shared Sub Main()
        Console.WriteLine("X = " & A.X & ", Y = " & B.Y)
    End Sub
End Class
```

<span data-ttu-id="ce7ed-443">Isso produz a saída:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-443">This produces the output:</span></span>

```
X = 1, Y = 2
```

<span data-ttu-id="ce7ed-444">Para executar o `Main` método, o sistema carrega primeiro classe `B`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-444">To execute the `Main` method, the system first loads class `B`.</span></span> <span data-ttu-id="ce7ed-445">O `Shared` construtor da classe `B` prossegue para calcular o valor inicial da `Y`, quais recursivamente faz com que a classe `A` a ser carregado porque o valor de `A.X` é referenciado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-445">The `Shared` constructor of class `B` proceeds to compute the initial value of `Y`, which recursively causes class `A` to be loaded because the value of `A.X` is referenced.</span></span> <span data-ttu-id="ce7ed-446">O `Shared` construtor da classe `A` por sua vez prossegue para calcular o valor inicial da `X`e fazer buscas caso o *padrão* valor `Y`, que é zero.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-446">The `Shared` constructor of class `A` in turn proceeds to compute the initial value of `X`, and in doing so fetches the *default* value of `Y`, which is zero.</span></span> <span data-ttu-id="ce7ed-447">`A.X` Assim, é inicializado para `1`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-447">`A.X` is thus initialized to `1`.</span></span> <span data-ttu-id="ce7ed-448">O processo de carregamento `A` , em seguida, for concluído, retornando para o cálculo do valor inicial de `Y`, cujo resultado se torna `2`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-448">The process of loading `A` then completes, returning to the calculation of the initial value of `Y`, the result of which becomes `2`.</span></span>

<span data-ttu-id="ce7ed-449">Tinha a `Main` método em vez disso, foi localizado na classe `A`, o exemplo seria ter produziu a saída a seguir:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-449">Had the `Main` method instead been located in class `A`, the example would have produced the following output:</span></span>

```
X = 2, Y = 1
```

<span data-ttu-id="ce7ed-450">Evitar referências circulares no `Shared` inicializadores de variável, pois ela é geralmente impossível determinar a ordem na quais as classes que contêm essas referências são carregados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-450">Avoid circular references in `Shared` variable initializers since it is generally impossible to determine the order in which classes containing such references are loaded.</span></span>

## <a name="events"></a><span data-ttu-id="ce7ed-451">Eventos</span><span class="sxs-lookup"><span data-stu-id="ce7ed-451">Events</span></span>

<span data-ttu-id="ce7ed-452">Eventos são usados para notificar o código de uma ocorrência específica.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-452">Events are used to notify code of a particular occurrence.</span></span> <span data-ttu-id="ce7ed-453">Uma declaração de evento consiste em um identificador, um tipo de delegado ou uma lista de parâmetros e um opcional `Implements` cláusula.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-453">An event declaration consists of an identifier, either a delegate type or a parameter list, and an optional `Implements` clause.</span></span>

```antlr
EventMemberDeclaration
    : RegularEventMemberDeclaration
    | CustomEventMemberDeclaration
    ;

RegularEventMemberDeclaration
    : Attributes? EventModifiers* 'Event'
      Identifier ParametersOrType ImplementsClause? StatementTerminator
    ;

InterfaceEventMemberDeclaration
    : Attributes? InterfaceEventModifiers* 'Event'
      Identifier ParametersOrType StatementTerminator
    ;

ParametersOrType
    : ( OpenParenthesis ParameterList? CloseParenthesis )?
    | 'As' NonArrayTypeName
    ;

EventModifiers
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    ;

InterfaceEventModifiers
    : 'Shadows'
    ;
```

<span data-ttu-id="ce7ed-454">Se um tipo de delegado for especificado, o tipo de delegado não pode ter um tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-454">If a delegate type is specified, the delegate type may not have a return type.</span></span> <span data-ttu-id="ce7ed-455">Se uma lista de parâmetro for especificada, ele não pode conter `Optional` ou `ParamArray` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-455">If a parameter list is specified, it may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="ce7ed-456">Domínio de acessibilidade dos tipos de parâmetro e/ou tipo de delegado deve ser igual ou um superconjunto do domínio de acessibilidade de evento em si.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-456">The accessibility domain of the parameter types and/or delegate type must be the same as, or a superset of, the accessibility domain of the event itself.</span></span> <span data-ttu-id="ce7ed-457">Eventos podem ser compartilhados, especificando o `Shared` modificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-457">Events may be shared by specifying the `Shared` modifier.</span></span>

<span data-ttu-id="ce7ed-458">O nome do membro adicionado ao espaço de declaração do tipo, além de uma declaração de evento declara implicitamente vários outros membros.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-458">In addition to the member name added to the type's declaration space, an event declaration implicitly declares several other members.</span></span> <span data-ttu-id="ce7ed-459">Dado um evento chamado `X`, os seguintes membros são adicionados ao espaço de declaração:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-459">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="ce7ed-460">Se a forma da declaração é uma declaração de método, uma classe delegate aninhado chamado `XEventHandler` é introduzido.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-460">If the form of the declaration is a method declaration, a nested delegate class named `XEventHandler` is introduced.</span></span> <span data-ttu-id="ce7ed-461">A classe delegate aninhado corresponde à declaração de método e tem a mesma acessibilidade que o evento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-461">The nested delegate class matches the method declaration and has the same accessibility as the event.</span></span> <span data-ttu-id="ce7ed-462">Os atributos na lista de parâmetros se aplicam aos parâmetros da classe delegada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-462">The attributes in the parameter list apply to the parameters of the delegate class.</span></span>

* <span data-ttu-id="ce7ed-463">Um `Private` variável de instância é digitado como o delegado chamado `XEvent`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-463">A `Private` instance variable typed as the delegate, named `XEvent`.</span></span>

* <span data-ttu-id="ce7ed-464">Dois métodos chamados `add_X` e `remove_X` que não pode ser invocado, substituído ou sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-464">Two methods named `add_X` and `remove_X` which cannot be invoked, overridden or overloaded.</span></span>

<span data-ttu-id="ce7ed-465">Se um tipo de tentativas declarar um nome que corresponda a um dos nomes acima, ocorrerá um erro de tempo de compilação e o implícitas `add_X` e `remove_X` declarações são ignoradas para fins de associação de nome.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-465">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit `add_X` and `remove_X` declarations are ignored for the purposes of name binding.</span></span> <span data-ttu-id="ce7ed-466">Não é possível substituir ou sobrecarga de qualquer um dos membros introduzidos, embora seja possível sombreá-los em tipos derivados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-466">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span> <span data-ttu-id="ce7ed-467">Por exemplo, a declaração de classe</span><span class="sxs-lookup"><span data-stu-id="ce7ed-467">For example, the class declaration</span></span>

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

<span data-ttu-id="ce7ed-468">é equivalente à declaração a seguir</span><span class="sxs-lookup"><span data-stu-id="ce7ed-468">is equivalent to the following declaration</span></span>

```vb
Class Raiser
    Public Delegate Sub ConstructedEventHandler(i As Integer)

    Protected ConstructedEvent As ConstructedEventHandler

    Public Sub add_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Combine(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub

    Public Sub remove_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Remove(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub
End Class
```

<span data-ttu-id="ce7ed-469">Declarar um evento sem especificar um tipo de delegado é a sintaxe mais simples e mais compacta, mas tem a desvantagem de declarar um novo tipo de delegado para cada evento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-469">Declaring an event without specifying a delegate type is the simplest and most compact syntax, but has the disadvantage of declaring a new delegate type for each event.</span></span> <span data-ttu-id="ce7ed-470">Por exemplo, no exemplo a seguir, três tipos de delegado ocultos são criados, mesmo que todos os três eventos tenham a mesma lista de parâmetros:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-470">For example, in the following example, three hidden delegate types are created, even though all three events have the same parameter list:</span></span>

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

<span data-ttu-id="ce7ed-471">No exemplo a seguir, os eventos simplesmente usam o mesmo delegado, `EventHandler`:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-471">In the following example, the events simply use the same delegate, `EventHandler`:</span></span>

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

<span data-ttu-id="ce7ed-472">Eventos podem ser manipulados em uma das duas maneiras: estática ou dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-472">Events can be handled in one of two ways: statically or dynamically.</span></span> <span data-ttu-id="ce7ed-473">Estaticamente a manipulação de eventos é mais simples e requer apenas um `WithEvents` variável e um `Handles` cláusula.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-473">Statically handling events is simpler and only requires a `WithEvents` variable and a `Handles` clause.</span></span> <span data-ttu-id="ce7ed-474">No exemplo a seguir, a classe `Form1` estaticamente manipula o evento `Click` do objeto `Button`:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-474">In the following example, class `Form1` statically handles the event `Click` of object `Button`:</span></span>

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

<span data-ttu-id="ce7ed-475">Dinamicamente a manipulação de eventos é mais complexo porque o evento deve ser explicitamente conectado e desconectado no código.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-475">Dynamically handling events is more complex because the event must be explicitly connected and disconnected to in code.</span></span> <span data-ttu-id="ce7ed-476">A instrução `AddHandler` adiciona um manipulador para um evento e a instrução `RemoveHandler` remove um manipulador para um evento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-476">The statement `AddHandler` adds a handler for an event, and the statement `RemoveHandler` removes a handler for an event.</span></span> <span data-ttu-id="ce7ed-477">O exemplo a seguir mostra uma classe `Form1` que adiciona `Button1_Click` como um manipulador de eventos `Button1`do `Click` evento:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-477">The next example shows a class `Form1` that adds `Button1_Click` as an event handler for `Button1`'s `Click` event:</span></span>

```vb
Public Class Form1
    Public Sub New()
        ' Add Button1_Click as an event handler for Button1's Click event.
        AddHandler Button1.Click, AddressOf Button1_Click
    End Sub 

    Private Button1 As Button = New Button()

    Sub Button1_Click(sender As Object, e As EventArgs)
        Console.WriteLine("Button1 was clicked!")
    End Sub

    Public Sub Disconnect()
        RemoveHandler Button1.Click, AddressOf Button1_Click
    End Sub 
End Class
```

<span data-ttu-id="ce7ed-478">No método `Disconnect`, o manipulador de eventos é removido.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-478">In method `Disconnect`, the event handler is removed.</span></span>


### <a name="custom-events"></a><span data-ttu-id="ce7ed-479">Eventos personalizados</span><span class="sxs-lookup"><span data-stu-id="ce7ed-479">Custom Events</span></span>

<span data-ttu-id="ce7ed-480">Conforme discutido na seção anterior, declarações de evento definem implicitamente um campo, uma `add_` método e um `remove_` método que são usados para manter o controle de manipuladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-480">As discussed in the previous section, event declarations implicitly define a field, an `add_` method, and a `remove_` method that are used to keep track of event handlers.</span></span> <span data-ttu-id="ce7ed-481">Em algumas situações, no entanto, pode ser desejável para fornecer o código personalizado para manipuladores de eventos de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-481">In some situations, however, it may be desirable to provide custom code for tracking event handlers.</span></span> <span data-ttu-id="ce7ed-482">Por exemplo, se uma classe define quarenta eventos dos quais somente alguns serão sempre tratadas, usando uma tabela de hash em vez de quarenta campos para acompanhar os manipuladores para cada evento podem ser mais eficientes.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-482">For example, if a class defines forty events of which only a few will ever be handled, using a hash table instead of forty fields to track the handlers for each event may be more efficient.</span></span> <span data-ttu-id="ce7ed-483">*Eventos personalizados* permitir que o `add_X` e `remove_X` métodos seja definido explicitamente, o que permite que um armazenamento personalizado para manipuladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-483">*Custom events* allow the `add_X` and `remove_X` methods to be defined explicitly, which enables custom storage for event handlers.</span></span>

<span data-ttu-id="ce7ed-484">Eventos personalizados são declarados da mesma forma que eventos que especificam um tipo de delegado são declarados com a exceção de que a palavra-chave `Custom` deve preceder o `Event` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-484">Custom events are declared in the same way that events that specify a delegate type are declared, with the exception that the keyword `Custom` must precede the `Event` keyword.</span></span> <span data-ttu-id="ce7ed-485">Uma declaração de evento personalizado contém três declarações: uma `AddHandler` declaração, um `RemoveHandler` declaração e um `RaiseEvent` declaração.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-485">A custom event declaration contains three declarations: an `AddHandler` declaration, a `RemoveHandler` declaration and a `RaiseEvent` declaration.</span></span> <span data-ttu-id="ce7ed-486">Nenhuma das declarações podem ter qualquer modificador, embora possam ter atributos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-486">None of the declarations can have any modifiers, although they can have attributes.</span></span>

```antlr
CustomEventMemberDeclaration
    : Attributes? EventModifiers* 'Custom' 'Event'
      Identifier 'As' TypeName ImplementsClause? StatementTerminator
      EventAccessorDeclaration+
      'End' 'Event' StatementTerminator
    ;

EventAccessorDeclaration
    : AddHandlerDeclaration
    | RemoveHandlerDeclaration
    | RaiseEventDeclaration
    ;

AddHandlerDeclaration
    : Attributes? 'AddHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'AddHandler' StatementTerminator
    ;

RemoveHandlerDeclaration
    : Attributes? 'RemoveHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RemoveHandler' StatementTerminator
    ;

RaiseEventDeclaration
    : Attributes? 'RaiseEvent'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RaiseEvent' StatementTerminator
    ;
```

<span data-ttu-id="ce7ed-487">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-487">For example:</span></span>

```vb
Class Test
    Private Handlers As EventHandler

    Public Custom Event TestEvent As EventHandler
        AddHandler(value As EventHandler)
            Handlers = CType([Delegate].Combine(Handlers, value), _
                EventHandler)
        End AddHandler

        RemoveHandler(value as EventHandler)
            Handlers = CType([Delegate].Remove(Handlers, value), _
                EventHandler)
        End RemoveHandler

        RaiseEvent(sender As Object, e As EventArgs)
            Dim TempHandlers As EventHandler = Handlers

            If TempHandlers IsNot Nothing Then
                TempHandlers(sender, e)
            End If
        End RaiseEvent
    End Event
End Class
```

<span data-ttu-id="ce7ed-488">O `AddHandler` e `RemoveHandler` declaração assumir um `ByVal` parâmetro, que deve ser do tipo de delegado do evento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-488">The `AddHandler` and `RemoveHandler` declaration take one `ByVal` parameter, which must be of the delegate type of the event.</span></span> <span data-ttu-id="ce7ed-489">Quando um `AddHandler` ou `RemoveHandler` instrução é executada (ou um `Handles` cláusula automaticamente manipula um evento), a declaração correspondente será chamada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-489">When an `AddHandler` or `RemoveHandler` statement is executed (or a `Handles` clause automatically handles an event), the corresponding declaration will be called.</span></span> <span data-ttu-id="ce7ed-490">O `RaiseEvent` declaração usa os mesmos parâmetros que o delegado do evento e será chamada quando um `RaiseEvent` instrução é executada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-490">The `RaiseEvent` declaration takes the same parameters as the event delegate and will be called when a `RaiseEvent` statement is executed.</span></span> <span data-ttu-id="ce7ed-491">Todas as declarações de devem ser fornecidas e são consideradas sub-rotinas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-491">All of the declarations must be provided and are considered to be subroutines.</span></span>

<span data-ttu-id="ce7ed-492">Observe que `AddHandler`, `RemoveHandler` e `RaiseEvent` declarações têm a mesma restrição no posicionamento de linha que tem as sub-rotinas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-492">Note that `AddHandler`, `RemoveHandler` and `RaiseEvent` declarations have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="ce7ed-493">A instrução de início, a instrução end e o bloco devem aparecer no início de uma linha lógica.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-493">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

<span data-ttu-id="ce7ed-494">O nome do membro adicionado ao espaço de declaração do tipo, além de uma declaração de evento personalizado declara implicitamente vários outros membros.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-494">In addition to the member name added to the type's declaration space, a custom event declaration implicitly declares several other members.</span></span> <span data-ttu-id="ce7ed-495">Dado um evento chamado `X`, os seguintes membros são adicionados ao espaço de declaração:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-495">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="ce7ed-496">Um método chamado `add_X`, correspondente à `AddHandler` declaração.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-496">A method named `add_X`, corresponding to the `AddHandler` declaration.</span></span>

* <span data-ttu-id="ce7ed-497">Um método chamado `remove_X`, correspondente à `RemoveHandler` declaração.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-497">A method named `remove_X`, corresponding to the `RemoveHandler` declaration.</span></span>

* <span data-ttu-id="ce7ed-498">Um método chamado `fire_X`, correspondente à `RaiseEvent` declaração.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-498">A method named `fire_X`, corresponding to the `RaiseEvent` declaration.</span></span>

<span data-ttu-id="ce7ed-499">Se um tipo de tentativas declarar um nome que corresponda a um dos nomes acima, ocorrerá um erro de tempo de compilação e as declarações implícitas são ignoradas para fins de associação de nome.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-499">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit declarations are all ignored for the purposes of name binding.</span></span> <span data-ttu-id="ce7ed-500">Não é possível substituir ou sobrecarga de qualquer um dos membros introduzidos, embora seja possível sombreá-los em tipos derivados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-500">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span>

<span data-ttu-id="ce7ed-501">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-501">__Note.__</span></span> <span data-ttu-id="ce7ed-502">`Custom` não é uma palavra reservada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-502">`Custom` is not a reserved word.</span></span>

#### <a name="custom-events-in-winrt-assemblies"></a><span data-ttu-id="ce7ed-503">Eventos personalizados em assemblies de WinRT</span><span class="sxs-lookup"><span data-stu-id="ce7ed-503">Custom events in WinRT assemblies</span></span>

<span data-ttu-id="ce7ed-504">A partir do Microsoft Visual Basic 11.0, eventos declarados em um arquivo compilado com `/target:winmdobj`, declarado em uma interface em um arquivo desse tipo e, em seguida, implementado em outro lugar, são tratados de maneira um pouco diferente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-504">As of Microsoft Visual Basic 11.0, events declared in a file compiled with `/target:winmdobj`, or declared in an interface in such a file and then implemented elsewhere, are treated a little differently.</span></span>

* <span data-ttu-id="ce7ed-505">Ferramentas externas usadas para criar o winmd normalmente permitir somente determinados tipos de delegado, como `System.EventHandler(Of T)` ou `System.TypedEventHandle(Of T, U)`e não permitirá a outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-505">External tools used to build the winmd will typically allow only certain delegate types such as `System.EventHandler(Of T)` or `System.TypedEventHandle(Of T, U)`, and will disallow others.</span></span>

* <span data-ttu-id="ce7ed-506">O `XEvent` campo tem um tipo `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` onde `T` é o tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-506">The `XEvent` field has type `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` where `T` is the delegate type.</span></span>

* <span data-ttu-id="ce7ed-507">Retorna o acessador AddHandler um `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`, e o acessador RemoveHandler usa um único parâmetro do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-507">The AddHandler accessor returns a `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`, and the RemoveHandler accessor takes a single parameter of the same type.</span></span>

<span data-ttu-id="ce7ed-508">Aqui está um exemplo de um evento personalizado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-508">Here is an example of such a custom event.</span></span>

```vb
Imports System.Runtime.InteropServices.WindowsRuntime

Public NotInheritable Class ClassInWinMD
    Private XEvent As EventRegistrationTokenTable(Of EventHandler(Of Integer))

    Public Custom Event X As EventHandler(Of Integer)
        AddHandler(handler As EventHandler(Of Integer))
            Return EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                   GetOrCreateEventRegistrationTokenTable(XEvent).
                   AddEventHandler(handler)
        End AddHandler

        RemoveHandler(token As EventRegistrationToken)
            EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                RemoveEventHandler(token)
        End RemoveHandler

        RaiseEvent(sender As Object, i As Integer)
            Dim table = EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                InvocationList
            If table IsNot Nothing Then table(sender, i)
        End RaiseEvent
    End Event
End Class
```


## <a name="constants"></a><span data-ttu-id="ce7ed-509">Constantes</span><span class="sxs-lookup"><span data-stu-id="ce7ed-509">Constants</span></span>

<span data-ttu-id="ce7ed-510">Um *constante* é um valor constante que é um membro de um tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-510">A *constant* is a constant value that is a member of a type.</span></span>

```antlr
ConstantMemberDeclaration
    : Attributes? ConstantModifier* 'Const' ConstantDeclarators StatementTerminator
    ;

ConstantModifier
    : AccessModifier
    | 'Shadows'
    ;

ConstantDeclarators
    : ConstantDeclarator ( Comma ConstantDeclarator )*
    ;

ConstantDeclarator
    : Identifier ( 'As' TypeName )? Equals ConstantExpression StatementTerminator
    ;
```

<span data-ttu-id="ce7ed-511">Constantes são implicitamente compartilhadas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-511">Constants are implicitly shared.</span></span> <span data-ttu-id="ce7ed-512">Se a declaração contém um `As` cláusula, a cláusula Especifica o tipo do membro introduzido pela declaração.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-512">If the declaration contains an `As` clause, the clause specifies the type of the member introduced by the declaration.</span></span> <span data-ttu-id="ce7ed-513">Se o tipo for omitido, o tipo da constante é inferido.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-513">If the type is omitted then the type of the constant is inferred.</span></span> <span data-ttu-id="ce7ed-514">O tipo de uma constante só pode ser um tipo primitivo ou `Object`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-514">The type of a constant may only be a primitive type or `Object`.</span></span> <span data-ttu-id="ce7ed-515">Se uma constante é digitada como `Object` e não há nenhum caractere de tipo, o tipo real da constante será o tipo da expressão constante.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-515">If a constant is typed as `Object` and there is no type character, the real type of the constant will be the type of the constant expression.</span></span> <span data-ttu-id="ce7ed-516">Caso contrário, o tipo da constante é o tipo de caractere de tipo de constante.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-516">Otherwise, the type of the constant is the type of the constant's type character.</span></span>

<span data-ttu-id="ce7ed-517">O exemplo a seguir mostra uma classe chamada `Constants` que tem duas constantes públicas:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-517">The following example shows a class named `Constants` that has two public constants:</span></span>

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

<span data-ttu-id="ce7ed-518">Constantes podem ser acessadas por meio da classe, como no exemplo a seguir, que imprime os valores de `Constants.A` e `Constants.B`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-518">Constants can be accessed through the class, as in the following example, which prints out the values of `Constants.A` and `Constants.B`.</span></span>

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

<span data-ttu-id="ce7ed-519">Uma declaração de constante que declara várias constantes é equivalente a várias declarações de constantes únicos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-519">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants.</span></span> <span data-ttu-id="ce7ed-520">O exemplo a seguir declara três constantes em uma declaração de estrutura.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-520">The following example declares three constants in one declaration statement.</span></span>

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

<span data-ttu-id="ce7ed-521">Essa declaração é equivalente ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-521">This declaration is equivalent to the following:</span></span>

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

<span data-ttu-id="ce7ed-522">O domínio de acessibilidade do tipo da constante deve ser igual ou um superconjunto do domínio de acessibilidade da própria constante.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-522">The accessibility domain of the type of the constant must be the same as or a superset of the accessibility domain of the constant itself.</span></span> <span data-ttu-id="ce7ed-523">A expressão de constante deve produzir um valor de tipo de constante ou de um tipo que é implicitamente conversível no tipo de constante.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-523">The constant expression must yield a value of the constant's type or of a type that is implicitly convertible to the constant's type.</span></span> <span data-ttu-id="ce7ed-524">A expressão de constante não pode ser circular; ou seja, uma constante não pode ser definida em termos de em si.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-524">The constant expression may not be circular; that is, a constant may not be defined in terms of itself.</span></span>

<span data-ttu-id="ce7ed-525">O compilador avalia automaticamente as declarações de constante na ordem apropriada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-525">The compiler automatically evaluates the constant declarations in the appropriate order.</span></span> <span data-ttu-id="ce7ed-526">No exemplo a seguir, o compilador primeiro avalia `Y`, em seguida, `Z`e, finalmente, `X`, produzindo os valores 10, 11 e 12, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-526">In the following example, the compiler first evaluates `Y`, then `Z`, and finally `X`, producing the values 10, 11, and 12, respectively.</span></span>

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

<span data-ttu-id="ce7ed-527">Quando um nome simbólico para um valor constante é desejado, mas o tipo do valor não é permitido em uma declaração de constante ou quando o valor não pode ser calculado em tempo de compilação por uma expressão constante, uma variável somente leitura pode ser usada em vez disso.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-527">When a symbolic name for a constant value is desired, but the type of the value is not permitted in a constant declaration or when the value cannot be computed at compile time by a constant expression, a read-only variable may be used instead.</span></span>


## <a name="instance-and-shared-variables"></a><span data-ttu-id="ce7ed-528">Variáveis compartilhadas e instância</span><span class="sxs-lookup"><span data-stu-id="ce7ed-528">Instance and Shared Variables</span></span>

<span data-ttu-id="ce7ed-529">Uma instância ou uma variável compartilhada é um membro de um tipo que pode armazenar informações.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-529">An instance or shared variable is a member of a type that can store information.</span></span>

```antlr
VariableMemberDeclaration
    : Attributes? VariableModifier+ VariableDeclarators StatementTerminator
    ;

VariableModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'ReadOnly'
    | 'WithEvents'
    | 'Dim'
    ;

VariableDeclarators
    : VariableDeclarator ( Comma VariableDeclarator )*
    ;

VariableDeclarator
    : VariableIdentifiers 'As' ObjectCreationExpression
    | VariableIdentifiers ( 'As' TypeName )? ( Equals Expression )?
    ;

VariableIdentifiers
    : VariableIdentifier ( Comma VariableIdentifier )*
    ;

VariableIdentifier
    : Identifier IdentifierModifiers
    ;
```

<span data-ttu-id="ce7ed-530">O `Dim` modificador deve ser especificado se nenhum modificador forem especificados, mas poderá ser omitido, caso contrário.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-530">The `Dim` modifier must be specified if no modifiers are specified, but may be omitted otherwise.</span></span> <span data-ttu-id="ce7ed-531">Uma única declaração de variável pode incluir múltiplos declaradores variáveis; cada variável declarador introduz uma nova instância ou um membro compartilhado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-531">A single variable declaration may include multiple variable declarators; each variable declarator introduces a new instance or shared member.</span></span>

<span data-ttu-id="ce7ed-532">Se um inicializador for especificado, apenas uma instância ou uma variável compartilhada pode ser declarado pela variável declarador:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-532">If an initializer is specified, only one instance or shared variable may be declared by the variable declarator:</span></span>

```vb
Class Test
    Dim a, b, c, d As Integer = 10  ' Invalid: multiple initialization
End Class
```

<span data-ttu-id="ce7ed-533">Essa restrição não se aplica para inicializadores de objeto:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-533">This restriction does not apply to object initializers:</span></span>

```vb
Class Test
    Dim a, b, c, d As New Collection() ' OK
End Class
```

<span data-ttu-id="ce7ed-534">Uma variável declarada com o `Shared` modificador é uma *variável compartilhada*.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-534">A variable declared with the `Shared` modifier is a *shared variable*.</span></span> <span data-ttu-id="ce7ed-535">Uma variável compartilhada identifica exatamente um local de armazenamento, independentemente do número de instâncias do tipo que são criados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-535">A shared variable identifies exactly one storage location regardless of the number of instances of the type that are created.</span></span> <span data-ttu-id="ce7ed-536">Uma variável compartilhada entra em existência quando um programa começa a ser executado e deixa de existir quando o programa é encerrado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-536">A shared variable comes into existence when a program begins executing, and ceases to exist when the program terminates.</span></span>

<span data-ttu-id="ce7ed-537">Uma variável compartilhada é compartilhada somente entre instâncias de um determinado tipo genérico fechado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-537">A shared variable is shared only among instances of a particular closed generic type.</span></span> <span data-ttu-id="ce7ed-538">Por exemplo, o programa:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-538">For example, the program:</span></span>

```vb
Class C(Of V) 
    Shared InstanceCount As Integer = 0

    Public Sub New()  
        InstanceCount += 1 
    End Sub

    Public Shared ReadOnly Property Count() As Integer 
        Get
            Return InstanceCount
        End Get
    End Property
End Class

Class Application 
    Shared Sub Main() 
        Dim x1 As New C(Of Integer)()
        Console.WriteLine(C(Of Integer).Count)

        Dim x2 As New C(Of Double)() 
        Console.WriteLine(C(Of Integer).Count)

        Dim x3 As New C(Of Integer)() 
        Console.WriteLine(C(Of Integer).Count)
    End Sub
End Class
```

<span data-ttu-id="ce7ed-539">Imprime:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-539">Prints out:</span></span>

```
1
1
2
```

<span data-ttu-id="ce7ed-540">Uma variável declarada sem a `Shared` modificador é chamado um *variável de instância*.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-540">A variable declared without the `Shared` modifier is called an *instance variable*.</span></span> <span data-ttu-id="ce7ed-541">Cada instância de uma classe contém uma cópia separada de todas as variáveis de instância da classe.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-541">Every instance of a class contains a separate copy of all instance variables of the class.</span></span> <span data-ttu-id="ce7ed-542">Uma variável de instância de um tipo de referência entra em existência quando uma nova instância do que o tipo é criado e deixa de existir quando não houver nenhuma referência a essa instância e o `Finalize` método foi executado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-542">An instance variable of a reference type comes into existence when a new instance of that type is created, and ceases to exist when there are no references to that instance and the `Finalize` method has executed.</span></span> <span data-ttu-id="ce7ed-543">Uma variável de instância de um tipo de valor tem exatamente a mesma duração como a variável à qual ele pertence.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-543">An instance variable of a value type has exactly the same lifetime as the variable to which it belongs.</span></span> <span data-ttu-id="ce7ed-544">Em outras palavras, quando uma variável de um tipo de valor entra em existência ou deixa de existir, então, faz a variável de instância do tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-544">In other words, when a variable of a value type comes into existence or ceases to exist, so does the instance variable of the value type.</span></span>

<span data-ttu-id="ce7ed-545">Se o declarador contiver um `As` cláusula, a cláusula Especifica o tipo dos membros apresentados pela declaração.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-545">If the declarator contains an `As` clause, the clause specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="ce7ed-546">Se o tipo for omitido e a semântica estrita está sendo usada, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-546">If the type is omitted and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="ce7ed-547">Caso contrário, o tipo dos membros é implicitamente `Object` ou o tipo de caractere de tipo dos membros.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-547">Otherwise the type of the members is implicitly `Object` or the type of the members' type character.</span></span>

<span data-ttu-id="ce7ed-548">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-548">__Note.__</span></span> <span data-ttu-id="ce7ed-549">Não há nenhuma ambiguidade na sintaxe: se um declarador omite um tipo, ele sempre usará o tipo de um declarador a seguir.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-549">There is no ambiguity in the syntax: if a declarator omits a type, it will always use the type of a following declarator.</span></span>

<span data-ttu-id="ce7ed-550">O domínio de acessibilidade de uma instância ou o tipo da variável compartilhada ou tipo de elemento de matriz deve ser igual ou um superconjunto do domínio de acessibilidade da instância ou uma variável compartilhada em si.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-550">The accessibility domain of an instance or shared variable's type or array element type must be the same as or a superset of the accessibility domain of the instance or shared variable itself.</span></span>

<span data-ttu-id="ce7ed-551">A exemplo a seguir mostra uma `Color` classe que tem variáveis de instância interna denominadas `redPart`, `greenPart`, e `bluePart`:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-551">The following example shows a `Color` class that has internal instance variables named `redPart`, `greenPart`, and `bluePart`:</span></span>

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub
End Class
```


### <a name="read-only-variables"></a><span data-ttu-id="ce7ed-552">Variáveis somente leitura</span><span class="sxs-lookup"><span data-stu-id="ce7ed-552">Read-Only Variables</span></span>

<span data-ttu-id="ce7ed-553">Quando uma instância ou declaração de variável compartilhada inclui um `ReadOnly` modificador, atribuições para as variáveis introduzidas pela declaração podem ocorrer somente como parte da declaração ou em um construtor na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-553">When an instance or shared variable declaration includes a `ReadOnly` modifier, assignments to the variables introduced by the declaration may only occur as part of the declaration or in a constructor in the same class.</span></span> <span data-ttu-id="ce7ed-554">Especificamente, as atribuições para uma instância somente leitura ou uma variável compartilhada são permitidas somente nas seguintes situações:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-554">Specifically, assignments to a read-only instance or shared variable are permitted only in the following situations:</span></span>

* <span data-ttu-id="ce7ed-555">A declaração de variável que apresenta a instância ou uma variável compartilhada (com a inclusão de um inicializador de variável na declaração).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-555">In the variable declaration that introduces the instance or shared variable (by including a variable initializer in the declaration).</span></span>

* <span data-ttu-id="ce7ed-556">Para uma variável de instância, nos construtores de instância da classe que contém a declaração de variável.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-556">For an instance variable, in the instance constructors of the class that contains the variable declaration.</span></span> <span data-ttu-id="ce7ed-557">A variável de instância só pode ser acessada de forma não qualificada ou por meio `Me` ou `MyClass`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-557">The instance variable can only be accessed in an unqualified manner or through `Me` or `MyClass`.</span></span>

* <span data-ttu-id="ce7ed-558">Para uma variável compartilhada, no construtor da classe compartilhado que contém a declaração de variável compartilhada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-558">For a shared variable, in the shared constructor of the class that contains the shared variable declaration.</span></span>

<span data-ttu-id="ce7ed-559">Uma variável compartilhada somente leitura é útil quando um nome simbólico para um valor constante for desejado, mas quando o tipo do valor não é permitido em uma declaração de constante ou quando o valor não pode ser calculado em tempo de compilação por uma expressão constante.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-559">A shared read-only variable is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a constant declaration, or when the value cannot be computed at compile time by a constant expression.</span></span>

<span data-ttu-id="ce7ed-560">Um exemplo de como a primeira maneira tal aplicativo, na qual cor variáveis compartilhadas são declaradas `ReadOnly` para impedir que eles estão sendo alterados por outros programas:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-560">An example of the first such application follows, in which color shared variables are declared `ReadOnly` to prevent them from being changed by other programs:</span></span>

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub 

    Public Shared ReadOnly Red As Color = New Color(&HFF, 0, 0)
    Public Shared ReadOnly Blue As Color = New Color(0, &HFF, 0)
    Public Shared ReadOnly Green As Color = New Color(0, 0, &HFF)
    Public Shared ReadOnly White As Color = New Color(&HFF, &HFF, &HFF)
End Class
```

<span data-ttu-id="ce7ed-561">Constantes e variáveis compartilhadas somente leitura têm semânticas diferentes.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-561">Constants and read-only shared variables have different semantics.</span></span> <span data-ttu-id="ce7ed-562">Quando uma expressão fizer referência a uma constante, o valor da constante é obtido em tempo de compilação, mas quando uma expressão fizer referência a uma variável compartilhada somente leitura, o valor da variável compartilhada não é obtido até o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-562">When an expression references a constant, the value of the constant is obtained at compile time, but when an expression references a read-only shared variable, the value of the shared variable is not obtained until run time.</span></span> <span data-ttu-id="ce7ed-563">Considere o aplicativo a seguir, que consiste em dois programas separados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-563">Consider the following application, which consists of two separate programs.</span></span>

<span data-ttu-id="ce7ed-564">File1.vb:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-564">file1.vb:</span></span>

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

<span data-ttu-id="ce7ed-565">File2.vb:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-565">file2.vb:</span></span>

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

<span data-ttu-id="ce7ed-566">Os namespaces `Program1` e `Program2` denotam os dois programas que são compilados separadamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-566">The namespaces `Program1` and `Program2` denote two programs that are compiled separately.</span></span> <span data-ttu-id="ce7ed-567">Como variável `Program1.Utils.X` é declarado como `Shared ReadOnly`, a saída de valor a `Console.WriteLine` instrução não é conhecida em tempo de compilação, mas em vez disso, é obtida em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-567">Because variable `Program1.Utils.X` is declared as `Shared ReadOnly`, the value output by the `Console.WriteLine` statement is not known at compile time, but rather is obtained at run time.</span></span> <span data-ttu-id="ce7ed-568">Portanto, se o valor de `X` é alterada e `Program1` é recompilado, o `Console.WriteLine` instrução produzirá o novo valor, mesmo se `Program2` não é recompilado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-568">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` is not recompiled.</span></span> <span data-ttu-id="ce7ed-569">No entanto, se `X` tivesse sido uma constante, o valor da `X` seria obtido no momento `Program2` foi compilado e estaria afetado pelas alterações na `Program1` até `Program2` foi recompilado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-569">However, if `X` had been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would have remained unaffected by changes in `Program1` until `Program2` was recompiled.</span></span>

### <a name="withevents-variables"></a><span data-ttu-id="ce7ed-570">Variáveis WithEvents</span><span class="sxs-lookup"><span data-stu-id="ce7ed-570">WithEvents Variables</span></span>

<span data-ttu-id="ce7ed-571">Um tipo pode declarar que manipula um conjunto de eventos gerados por um de sua instância ou a variáveis compartilhadas declarando a instância ou uma variável compartilhada que gera os eventos com o `WithEvents` modificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-571">A type can declare that it handles some set of events raised by one of its instance or shared variables by declaring the instance or shared variable that raises the events with the `WithEvents` modifier.</span></span> <span data-ttu-id="ce7ed-572">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-572">For example:</span></span>

```vb
Class Raiser
    Public Event E1()

    Public Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Public Sub Main()
        x = New Raiser()
    End Sub
End Module
```

<span data-ttu-id="ce7ed-573">Neste exemplo, o método `E1Handler` manipula o evento `E1` que é gerado pela instância do tipo `Raiser` armazenado na variável de instância `x`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-573">In this example, the method `E1Handler` handles the event `E1` that is raised by the instance of the type `Raiser` stored in the instance variable `x`.</span></span>

<span data-ttu-id="ce7ed-574">O `WithEvents` modificador faz com que a variável a ser renomeado com um sublinhado à esquerda e substituído por uma propriedade o mesmo nome que faz o vínculo de evento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-574">The `WithEvents` modifier causes the variable to be renamed with a leading underscore and replaced with a property of the same name that does the event hookup.</span></span> <span data-ttu-id="ce7ed-575">Por exemplo, se for o nome da variável `F`, ele é renomeado para `_F` e uma propriedade `F` é declarado implicitamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-575">For example, if the variable's name is `F`, it is renamed to `_F` and a property `F` is implicitly declared.</span></span> <span data-ttu-id="ce7ed-576">Se houver uma colisão entre o nome da variável nova e outra declaração, será relatado um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-576">If there is a collision between the variable's new name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="ce7ed-577">Todos os atributos aplicados à variável são transferidos para a variável renomeada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-577">Any attributes applied to the variable are carried over to the renamed variable.</span></span>

<span data-ttu-id="ce7ed-578">A propriedade implícita criada por um `WithEvents` declaração se encarrega de interceptação e unhooking os manipuladores de eventos relevantes.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-578">The implicit property created by a `WithEvents` declaration takes care of hooking and unhooking the relevant event handlers.</span></span> <span data-ttu-id="ce7ed-579">Quando um valor é atribuído à variável, a propriedade primeiro chama o `remove` método para o evento na instância atualmente na variável (unhooking o manipulador de eventos existente, se houver).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-579">When a value is assigned to the variable, the property first calls the `remove` method for the event on the instance currently in the variable (unhooking the existing event handler, if any).</span></span> <span data-ttu-id="ce7ed-580">Em seguida, a atribuição é feita e a propriedade chama o `add` método para o evento na nova instância da variável (conectar o novo manipulador de eventos).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-580">Next the assignment is made, and the property calls the `add` method for the event on the new instance in the variable (hooking up the new event handler).</span></span> <span data-ttu-id="ce7ed-581">O código a seguir é equivalente ao código acima para o módulo padrão `Test`:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-581">The following code is equivalent to the code above for the standard module `Test`:</span></span>

```vb
Module Test
    Private _x As Raiser

    Public Property x() As Raiser
        Get
            Return _x
        End Get

        Set (Value As Raiser)
            ' Unhook any existing handlers.
            If _x IsNot Nothing Then
                RemoveHandler _x.E1, AddressOf E1Handler
            End If

            ' Change value.
            _x = Value

            ' Hook-up new handlers.
            If _x IsNot Nothing Then
                AddHandler _x.E1, AddressOf E1Handler
            End If
        End Set
    End Property

    Sub E1Handler()
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
    End Sub
End Module
```

<span data-ttu-id="ce7ed-582">Não é válido para declarar uma instância ou uma variável compartilhada como `WithEvents` se a variável é tipada como uma estrutura.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-582">It is not valid to declare an instance or shared variable as `WithEvents` if the variable is typed as a structure.</span></span> <span data-ttu-id="ce7ed-583">Além disso, `WithEvents` não pode ser especificado em uma estrutura, e `WithEvents` e `ReadOnly` não podem ser combinados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-583">In addition, `WithEvents` may not be specified in a structure, and `WithEvents` and `ReadOnly` cannot be combined.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="ce7ed-584">Inicializadores de variável</span><span class="sxs-lookup"><span data-stu-id="ce7ed-584">Variable Initializers</span></span>

<span data-ttu-id="ce7ed-585">Instância e declarações de variável compartilhadas em classes e instância declarações de variável (mas declarações de variável não compartilhadas) em estruturas podem incluir inicializadores de variável.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-585">Instance and shared variable declarations in classes and instance variable declarations (but not shared variable declarations) in structures may include variable initializers.</span></span> <span data-ttu-id="ce7ed-586">Para `Shared` variáveis, inicializadores de variável correspondem a instruções de atribuição que são executadas depois que o programa é iniciado, mas antes de `Shared` variável é referenciada pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-586">For `Shared` variables, variable initializers correspond to assignment statements that are executed after the program begins, but before the `Shared` variable is first referenced.</span></span> <span data-ttu-id="ce7ed-587">Por exemplo, variáveis de inicializadores de variável correspondem às instruções de atribuição que são executadas quando uma instância da classe é criada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-587">For instance variables, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span> <span data-ttu-id="ce7ed-588">Estruturas não podem ter inicializadores de variável de instância porque seus construtores sem parâmetros não podem ser modificados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-588">Structures cannot have instance variable initializers because their parameterless constructors cannot be modified.</span></span>

<span data-ttu-id="ce7ed-589">Considere o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-589">Consider the following example:</span></span>

```vb
Class Test
    Public Shared x As Double = Math.Sqrt(2.0)
    Public i As Integer = 100
    Public s As String = "Hello"
End Class

Module TestModule
    Sub Main()
        Dim a As New Test()

        Console.WriteLine("x = " & Test.x & ", i = " & a.i & ", s = " & a.s)
    End Sub
End Module
```

<span data-ttu-id="ce7ed-590">O exemplo produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-590">The example produces the following output:</span></span>

```
x = 1.4142135623731, i = 100, s = Hello
```

<span data-ttu-id="ce7ed-591">Uma atribuição para `x` ocorre quando a classe é carregada e as atribuições a `i` e `s` ocorrem quando uma nova instância da classe é criada.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-591">An assignment to `x` occurs when the class is loaded, and assignments to `i` and `s` occur when a new instance of the class is created.</span></span>

<span data-ttu-id="ce7ed-592">É útil pensar em inicializadores de variável como instruções de atribuição que são inseridas automaticamente no bloco de construtor do tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-592">It is useful to think of variable initializers as assignment statements that are automatically inserted in the block of the type's constructor.</span></span> <span data-ttu-id="ce7ed-593">O exemplo a seguir contém várias inicializadores de variável de instância.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-593">The following example contains several instance variable initializers.</span></span>

```vb
Class A
    Private x As Integer = 1
    Private y As Integer = -1
    Private count As Integer

    Public Sub New()
        count = 0
    End Sub

    Public Sub New(n As Integer)
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double = Math.Sqrt(2.0)
    Private items As ArrayList = New ArrayList(100)
    Private max As Integer

    Public Sub New()
        Me.New(100)
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1)
        max = n
    End Sub
End Class
```

<span data-ttu-id="ce7ed-594">O exemplo corresponde ao código mostrado abaixo, onde cada comentário indica que uma instrução inserida automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-594">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement.</span></span>

```vb
Class A
    Private x, y, count As Integer

    Public Sub New()
        MyBase.New ' Invoke object() constructor.
        x = 1 ' This is a variable initializer.
        y = -1 ' This is a variable initializer.
        count = 0
    End Sub

    Public Sub New(n As Integer)
        MyBase.New ' Invoke object() constructor. 
        x = 1 ' This is a variable initializer.
        y = - 1 ' This is a variable initializer.
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double
    Private items As ArrayList
    Private max As Integer

    Public Sub New()
        Me.New(100) 
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1) 
        sqrt2 = Math.Sqrt(2.0) ' This is a variable initializer.
        items = New ArrayList(100) ' This is a variable initializer.
        max = n
    End Sub
End Class
```

<span data-ttu-id="ce7ed-595">Todas as variáveis são inicializadas como o valor padrão de seu tipo antes de qualquer variável inicializadores são executados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-595">All variables are initialized to the default value of their type before any variable initializers are executed.</span></span> <span data-ttu-id="ce7ed-596">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-596">For example:</span></span>

```vb
Class Test
    Public Shared b As Boolean
    Public i As Integer
End Class

Module TestModule
    Sub Main()
        Dim t As New Test()
        Console.WriteLine("b = " & Test.b & ", i = " & t.i)
    End Sub
End Module
```

<span data-ttu-id="ce7ed-597">Porque `b` é inicializado automaticamente para seu valor padrão quando a classe é carregada e `i` é automaticamente inicializado com seu valor padrão quando uma instância da classe é criada, o código anterior produz a saída a seguir:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-597">Because `b` is automatically initialized to its default value when the class is loaded and `i` is automatically initialized to its default value when an instance of the class is created, the preceding code produces the following output:</span></span>

```
b = False, i = 0
```

<span data-ttu-id="ce7ed-598">Cada inicializador de variável deve produzir um valor de tipo de variável ou de um tipo que é implicitamente conversível para o tipo da variável.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-598">Each variable initializer must yield a value of the variable's type or of a type that is implicitly convertible to the variable's type.</span></span> <span data-ttu-id="ce7ed-599">Um inicializador de variável pode ser circular ou fazer referência a uma variável que será inicializada depois dele, nesse caso o valor da variável referenciada é seu valor padrão para os fins do inicializador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-599">A variable initializer may be circular or refer to a variable that will be initialized after it, in which case the value of the referenced variable is its default value for the purposes of the initializer.</span></span> <span data-ttu-id="ce7ed-600">Tal um inicializador é de valor duvidosa.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-600">Such an initializer is of dubious value.</span></span>

<span data-ttu-id="ce7ed-601">Há três formas de inicializadores de variável: inicializadores regulares, inicializadores de tamanho da matriz e inicializadores de objeto.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-601">There are three forms of variable initializers: regular initializers, array-size initializers, and object initializers.</span></span> <span data-ttu-id="ce7ed-602">Os primeiros dois formulários aparecem após um sinal de igual que segue o nome de tipo, os dois últimos são parte da declaração em si.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-602">The first two forms appear after an equal sign that follows the type name, the latter two are part of the declaration itself.</span></span> <span data-ttu-id="ce7ed-603">Somente um formulário de inicializador pode ser usado em qualquer declaração em particular.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-603">Only one form of initializer may be used on any particular declaration.</span></span>

#### <a name="regular-initializers"></a><span data-ttu-id="ce7ed-604">Inicializadores regulares</span><span class="sxs-lookup"><span data-stu-id="ce7ed-604">Regular Initializers</span></span>

<span data-ttu-id="ce7ed-605">Um *inicializador regular* é uma expressão que é implicitamente conversível para o tipo da variável.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-605">A *regular initializer* is an expression that is implicitly convertible to the type of the variable.</span></span> <span data-ttu-id="ce7ed-606">Aparece após um sinal de igual que segue o nome do tipo e deve ser classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-606">It appears after an equal sign that follows the type name and must be classified as a value.</span></span> <span data-ttu-id="ce7ed-607">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-607">For example:</span></span>

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

<span data-ttu-id="ce7ed-608">Este programa produz a saída:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-608">This program produces the output:</span></span>

```vb
x = 10, y = 20
```

<span data-ttu-id="ce7ed-609">Se uma declaração de variável tiver um inicializador regular, apenas uma única variável pode ser declarada por vez.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-609">If a variable declaration has a regular initializer, then only a single variable can be declared at a time.</span></span> <span data-ttu-id="ce7ed-610">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-610">For example:</span></span>

```vb
Module Test
    Sub Main()
        ' OK, only one variable declared at a time.
        Dim x As Integer = 10, y As Integer = 20

        ' Error: Can't initialize multiple variables at once.
        Dim a, b As Integer = 10
    End Sub
End Module
```

#### <a name="object-initializers"></a><span data-ttu-id="ce7ed-611">Inicializadores de objeto</span><span class="sxs-lookup"><span data-stu-id="ce7ed-611">Object Initializers</span></span>

<span data-ttu-id="ce7ed-612">Uma *inicializador de objeto* é especificado usando uma expressão de criação de objeto no lugar do nome do tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-612">An *object initializer* is specified using an object creation expression in the place of the type name.</span></span> <span data-ttu-id="ce7ed-613">Um inicializador de objeto é equivalente a um inicializador regular de atribuir o resultado da expressão de criação de objeto para a variável.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-613">An object initializer is equivalent to a regular initializer assigning the result of the object creation expression to the variable.</span></span> <span data-ttu-id="ce7ed-614">So</span><span class="sxs-lookup"><span data-stu-id="ce7ed-614">So</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As New Test(10)
    End Sub
End Module
```

<span data-ttu-id="ce7ed-615">equivale a</span><span class="sxs-lookup"><span data-stu-id="ce7ed-615">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test(10)
    End Sub
End Module
```

<span data-ttu-id="ce7ed-616">Os parênteses em um inicializador de objeto é sempre interpretado como a lista de argumentos para o construtor e nunca como modificadores de tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-616">The parenthesis in an object initializer is always interpreted as the argument list for the constructor and never as array type modifiers.</span></span> <span data-ttu-id="ce7ed-617">Um nome de variável com um inicializador de objeto não pode ter um modificador de tipo de matriz ou um modificador de tipo anulável.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-617">A variable name with an object initializer cannot have an array type modifier or a nullable type modifier.</span></span>

#### <a name="array-size-initializers"></a><span data-ttu-id="ce7ed-618">Inicializadores de tamanho da matriz</span><span class="sxs-lookup"><span data-stu-id="ce7ed-618">Array-Size Initializers</span></span>

<span data-ttu-id="ce7ed-619">Uma *inicializador de tamanho da matriz* é um modificador no nome da variável que oferece um conjunto de limites superiores de dimensão indicada por expressões.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-619">An *array-size initializer* is a modifier on the name of the variable that gives a set of dimension upper bounds denoted by expressions.</span></span>

```antlr
ArraySizeInitializationModifier
    : OpenParenthesis BoundList CloseParenthesis ArrayTypeModifiers?
    ;

BoundList
    : Bound ( Comma Bound )*
    ;

Bound
    : Expression
    | '0' 'To' Expression
    ;
```

<span data-ttu-id="ce7ed-620">As expressões de limite superior deve ser classificadas como valores e deve ser implicitamente conversíveis para `Integer`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-620">The upper bound expressions must be classified as values and must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="ce7ed-621">O conjunto de limites superiores é equivalente a um inicializador de variável de uma expressão de criação de matriz com os limites superiores determinados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-621">The set of upper bounds is equivalent to a variable initializer of an array-creation expression with the given upper bounds.</span></span> <span data-ttu-id="ce7ed-622">O número de dimensões do tipo de matriz é inferido do inicializador de matriz de tamanho.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-622">The number of dimensions of the array type is inferred from the array size initializer.</span></span> <span data-ttu-id="ce7ed-623">So</span><span class="sxs-lookup"><span data-stu-id="ce7ed-623">So</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5, 10) As Integer
    End Sub
End Module
```

<span data-ttu-id="ce7ed-624">equivale a</span><span class="sxs-lookup"><span data-stu-id="ce7ed-624">is equivalent to</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer(,) = New Integer(5, 10) {}
    End Sub
End Module
```

<span data-ttu-id="ce7ed-625">Todos os limites superiores do deve ser igual ou maior que -1 e todas as dimensões devem ter um limite superior especificado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-625">All upper bounds must be equal to or greater than -1, and all dimensions must have an upper bound specified.</span></span> <span data-ttu-id="ce7ed-626">Se o tipo de elemento da matriz que está sendo inicializado em si for um tipo de matriz, os modificadores de tipo de matriz ir à direita do inicializador de tamanho da matriz.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-626">If the element type of the array being initialized is itself an array type, the array-type modifiers go to the right of the array-size initializer.</span></span> <span data-ttu-id="ce7ed-627">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="ce7ed-627">For example</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

<span data-ttu-id="ce7ed-628">declara uma variável local `x` cujo tipo é uma matriz bidimensional de matrizes tridimensionais de `Integer`inicializado para uma matriz com os limites do `0..5` na primeira dimensão e `0..10` na segunda dimensão.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-628">declares a local variable `x` whose type is a two-dimensional array of three-dimensional arrays of `Integer`, initialized to an array with bounds of `0..5` in the first dimension and `0..10` in the second dimension.</span></span> <span data-ttu-id="ce7ed-629">Não é possível usar um inicializador de tamanho de matriz para inicializar os elementos de uma variável cujo tipo é uma matriz de matrizes.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-629">It is not possible to use an array size initializer to initialize the elements of a variable whose type is an array of arrays.</span></span>

<span data-ttu-id="ce7ed-630">Uma declaração de variável com um inicializador de tamanho da matriz não pode ter um modificador de tipo de matriz em seu tipo ou um inicializador regular.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-630">A variable declaration with an array-size initializer cannot have an array type modifier on its type or a regular initializer.</span></span>


### <a name="systemmarshalbyrefobject-classes"></a><span data-ttu-id="ce7ed-631">Classes de System. MarshalByRefObject</span><span class="sxs-lookup"><span data-stu-id="ce7ed-631">System.MarshalByRefObject Classes</span></span>

<span data-ttu-id="ce7ed-632">As classes que derivam da classe `System.MarshalByRefObject` têm o marshaling realizado entre limites de contexto usando proxies (isto é, por referência) e não por meio de cópia (ou seja, por valor).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-632">Classes that derive from the class `System.MarshalByRefObject` are marshaled across context boundaries using proxies (that is, by reference) rather than through copying (that is, by value).</span></span> <span data-ttu-id="ce7ed-633">Isso significa que uma instância dessa classe não pode ser uma instância de true, mas em vez disso, pode ser apenas um stub que realiza marshaling de variável acessa e chama o método de um limite de contexto.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-633">This means that an instance of such a class may not be a true instance but instead may just be a stub that marshals variable accesses and method calls across a context boundary.</span></span>

<span data-ttu-id="ce7ed-634">Como resultado, não é possível criar uma referência para o local de armazenamento de variáveis definidas em tais classes.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-634">As a result, it is not possible to create a reference to the storage location of variables defined on such classes.</span></span> <span data-ttu-id="ce7ed-635">Isso significa que as variáveis digitadas como classes derivadas de `System.MarshalByRefObject` não pode ser passado para fazer referência a parâmetros e métodos e variáveis de variáveis, digitadas como tipos de valor não podem ser acessados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-635">This means that variables typed as classes derived from `System.MarshalByRefObject` cannot be passed to reference parameters, and methods and variables of variables typed as value types may not be accessed.</span></span> <span data-ttu-id="ce7ed-636">Em vez disso, o Visual Basic trata as variáveis definidas em tais classes como se fossem propriedades (já que as restrições são os mesmos em Propriedades).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-636">Instead, Visual Basic treats variables defined on such classes as if they were properties (since the restrictions are the same on properties).</span></span>

<span data-ttu-id="ce7ed-637">Há uma exceção a essa regra: um membro implicitamente ou explicitamente qualificado com `Me` é isento das restrições acima, pois `Me` sempre é garantido para ser um objeto real, não um proxy.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-637">There is one exception to this rule: a member implicitly or explicitly qualified with `Me` is exempt from the above restrictions, because `Me` is always guaranteed to be an actual object, not a proxy.</span></span>

## <a name="properties"></a><span data-ttu-id="ce7ed-638">Propriedades</span><span class="sxs-lookup"><span data-stu-id="ce7ed-638">Properties</span></span>

<span data-ttu-id="ce7ed-639">*Propriedades* são uma extensão natural das variáveis; elas são denominadas membros com tipos associados e a sintaxe para acessar variáveis e propriedades é o mesmo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-639">*Properties* are a natural extension of variables; both are named members with associated types, and the syntax for accessing variables and properties is the same.</span></span> <span data-ttu-id="ce7ed-640">Ao contrário de variáveis, no entanto, as propriedades não denotam locais de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-640">Unlike variables, however, properties do not denote storage locations.</span></span> <span data-ttu-id="ce7ed-641">Em vez disso, as propriedades têm *acessadores*, que especifica as instruções a executar para ler ou gravar seus valores.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-641">Instead, properties have *accessors*, which specify the statements to execute in order to read or write their values.</span></span>

<span data-ttu-id="ce7ed-642">Propriedades são definidas com declarações de propriedade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-642">Properties are defined with property declarations.</span></span> <span data-ttu-id="ce7ed-643">A primeira parte de uma declaração de propriedade é semelhante a uma declaração de campo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-643">The first part of a property declaration resembles a field declaration.</span></span> <span data-ttu-id="ce7ed-644">A segunda parte inclui um `Get` acessador e/ou um `Set` acessador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-644">The second part includes a `Get` accessor and/or a `Set` accessor.</span></span>

```antlr
PropertyMemberDeclaration
    : RegularPropertyMemberDeclaration
    | MustOverridePropertyMemberDeclaration
    | AutoPropertyMemberDeclaration
    ;

PropertySignature
    : 'Property'
      Identifier ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

RegularPropertyMemberDeclaration
    : Attributes? PropertyModifier* PropertySignature
      ImplementsClause? LineTerminator
      PropertyAccessorDeclaration+
      'End' 'Property' StatementTerminator
    ;

MustOverridePropertyMemberDeclaration
    : Attributes? MustOverridePropertyModifier+ PropertySignature
      ImplementsClause? StatementTerminator
    ;

AutoPropertyMemberDeclaration
    : Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )? ( Equals Expression )?
      ImplementsClause? LineTerminator
    | Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      'As' Attributes? 'New'
      ( NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )? )?
      ObjectCreationExpressionInitializer?
      ImplementsClause? LineTerminator
    ;

InterfacePropertyMemberDeclaration
    : Attributes? InterfacePropertyModifier* PropertySignature StatementTerminator
    ;

AutoPropertyModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'Overridable'
    | 'NotOverridable'
    | 'Overrides'
    | 'Overloads'
    ;

PropertyModifier
    : AutoPropertyModifier
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    | 'Iterator'
    ;

MustOverridePropertyModifier
    : PropertyModifier
    | 'MustOverride'
    ;

InterfacePropertyModifier
    : 'Shadows'
    | 'Overloads'
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    ;

PropertyAccessorDeclaration
    : PropertyGetDeclaration
    | PropertySetDeclaration
    ;
```

<span data-ttu-id="ce7ed-645">No exemplo a seguir, o `Button` classe define um `Caption` propriedade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-645">In the example below, the `Button` class defines a `Caption` property.</span></span>

```vb
Public Class Button
    Private captionValue As String

    Public Property Caption() As String
        Get
            Return captionValue
        End Get

        Set (Value As String)
            captionValue = value
            Repaint()
        End Set
    End Property

    ...
End Class
```

<span data-ttu-id="ce7ed-646">Com base nas `Button` classe acima, o seguinte é um exemplo de uso do `Caption` propriedade:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-646">Based on the `Button` class above, the following is an example of use of the `Caption` property:</span></span>

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

<span data-ttu-id="ce7ed-647">Aqui, o `Set` acessador é invocado, atribuindo um valor para a propriedade e o `Get` acessador é invocado por fazer referência à propriedade em uma expressão.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-647">Here, the `Set` accessor is invoked by assigning a value to the property, and the `Get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="ce7ed-648">Se nenhum tipo é especificado para uma propriedade e a semântica estrita está sendo usada, ocorre um erro de tempo de compilação; Caso contrário, o tipo da propriedade é implicitamente `Object` ou o tipo de caractere de tipo da propriedade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-648">If no type is specified for a property and strict semantics are being used, a compile-time error occurs; otherwise the type of the property is implicitly `Object` or the type of the property's type character.</span></span> <span data-ttu-id="ce7ed-649">Uma declaração de propriedade pode conter um `Get` acessador, que recupera o valor da propriedade, um `Set` acessador, que armazena o valor da propriedade, ou ambos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-649">A property declaration may contain either a `Get` accessor, which retrieves the value of the property, a `Set` accessor, which stores the value of the property, or both.</span></span> <span data-ttu-id="ce7ed-650">Porque uma propriedade declara implicitamente métodos, uma propriedade pode ser declarada com os mesmos modificadores como um método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-650">Because a property implicitly declares methods, a property may be declared with the same modifiers as a method.</span></span> <span data-ttu-id="ce7ed-651">Se a propriedade é definida em uma interface ou definida com o `MustOverride` modificador, o corpo de propriedade e o `End` constructo deve ser omitido; caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-651">If the property is defined in an interface or defined with the `MustOverride` modifier, the property body and the `End` construct must be omitted; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="ce7ed-652">A lista de parâmetros de índice compõe a assinatura da propriedade, portanto, as propriedades podem ser sobrecarregadas em parâmetros de índice, mas não no tipo da propriedade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-652">The index parameter list makes up the signature of the property, so properties may be overloaded on index parameters but not on the type of the property.</span></span> <span data-ttu-id="ce7ed-653">A lista de parâmetros de índice é da mesma maneira que um método regular.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-653">The index parameter list is the same as for a regular method.</span></span> <span data-ttu-id="ce7ed-654">No entanto, nenhum dos parâmetros podem ser modificadas com o `ByRef` modificador e nenhum deles pode ser nomeado `Value` (que é reservado para o parâmetro de valor implícito no `Set` acessador).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-654">However, none of the parameters may be modified with the `ByRef` modifier and none of them may be named `Value` (which is reserved for the implicit value parameter in the `Set` accessor).</span></span>

<span data-ttu-id="ce7ed-655">Uma propriedade pode ser declarada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-655">A property may be declared as follows:</span></span>

* <span data-ttu-id="ce7ed-656">Se a propriedade não especifica nenhum modificador de tipo de propriedade, a propriedade deve ter uma `Get` acessador e um `Set` acessador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-656">If the property specifies no property type modifier, the property must have both a `Get` accessor and a `Set` accessor.</span></span> <span data-ttu-id="ce7ed-657">A propriedade deve ser uma propriedade de leitura / gravação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-657">The property is said to be a read-write property.</span></span>

* <span data-ttu-id="ce7ed-658">Se a propriedade especifica o `ReadOnly` modificador, a propriedade deve ter um `Get` acessador e não pode ter um `Set` acessador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-658">If the property specifies the `ReadOnly` modifier, the property must have a `Get` accessor and may not have a `Set` accessor.</span></span> <span data-ttu-id="ce7ed-659">A propriedade deve ser de propriedade somente leitura.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-659">The property is said to be read-only property.</span></span> <span data-ttu-id="ce7ed-660">Ele é um erro de tempo de compilação para uma propriedade somente leitura ser o destino de uma atribuição.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-660">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>

* <span data-ttu-id="ce7ed-661">Se a propriedade especifica o `WriteOnly` modificador, a propriedade deve ter um `Set` acessador e não pode ter um `Get` acessador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-661">If the property specifies the `WriteOnly` modifier, the property must have a `Set` accessor and may not have a `Get` accessor.</span></span> <span data-ttu-id="ce7ed-662">A propriedade deve ser de propriedade somente gravação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-662">The property is said to be write-only property.</span></span> <span data-ttu-id="ce7ed-663">Ele é um erro de tempo de compilação para fazer referência a uma propriedade somente gravação em uma expressão, exceto como o destino de uma atribuição ou como um argumento para um método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-663">It is a compile-time error to reference a write-only property in an expression except as the target of an assignment or as an argument to a method.</span></span>

<span data-ttu-id="ce7ed-664">O `Get` e `Set` acessadores de uma propriedade não são membros diferentes e não é possível declarar os acessadores de uma propriedade separadamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-664">The `Get` and `Set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="ce7ed-665">O exemplo a seguir não declara uma única propriedade de leitura / gravação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-665">The following example does not declare a single read-write property.</span></span> <span data-ttu-id="ce7ed-666">Em vez disso, ele declara duas propriedades com o mesmo nome, um somente leitura e somente gravação:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-666">Rather, it declares two properties with the same name, one read-only and one write-only:</span></span>

```vb
Class A
    Private nameValue As String

    ' Error, contains a duplicate member name.
    Public ReadOnly Property Name() As String 
        Get
            Return nameValue
        End Get
    End Property

    ' Error, contains a duplicate member name.
    Public WriteOnly Property Name() As String 
        Set (Value As String)
            nameValue = value
        End Set
    End Property
End Class
```

<span data-ttu-id="ce7ed-667">Como dois membros declarados na mesma classe não podem ter o mesmo nome, o exemplo causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-667">Since two members declared in the same class cannot have the same name, the example causes a compile-time error.</span></span>

<span data-ttu-id="ce7ed-668">Por padrão, a acessibilidade de uma propriedade `Get` e `Set` acessadores é o mesmo que a acessibilidade da propriedade em si.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-668">By default, the accessibility of a property's `Get` and `Set` accessors is the same as the accessibility of the property itself.</span></span> <span data-ttu-id="ce7ed-669">No entanto, o `Get` e `Set` acessadores também podem especificar a acessibilidade separadamente da propriedade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-669">However, the `Get` and `Set` accessors can also specify accessibility separately from the property.</span></span> <span data-ttu-id="ce7ed-670">Nesse caso, a acessibilidade de um acessador deve ser mais restritiva do que a acessibilidade da propriedade e apenas um acessador pode ter um nível de acessibilidade diferente da propriedade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-670">In that case, the accessibility of an accessor must be more restrictive than the accessibility of the property and only one accessor can have a different accessibility level from the property.</span></span> <span data-ttu-id="ce7ed-671">Tipos de acesso são considerados mais ou menos restritivos da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-671">Access types are considered more or less restrictive as follows:</span></span>

* <span data-ttu-id="ce7ed-672">`Private` é mais restritivo do que `Public`, `Protected Friend`, `Protected`, ou `Friend`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-672">`Private` is more restrictive than `Public`, `Protected Friend`, `Protected`, or `Friend`.</span></span>

* <span data-ttu-id="ce7ed-673">`Friend` é mais restritivo do que `Protected Friend` ou `Public`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-673">`Friend` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="ce7ed-674">`Protected` é mais restritivo do que `Protected Friend` ou `Public`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-674">`Protected` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="ce7ed-675">`Protected Friend` é mais restritivo que `Public`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-675">`Protected Friend` is more restrictive than `Public`.</span></span>

<span data-ttu-id="ce7ed-676">Quando um dos acessadores da propriedade é acessível, mas o outro não é, a propriedade é tratada como se fosse somente leitura ou somente gravação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-676">When one of a property's accessors is accessible but the other one is not, the property is treated as if it was read-only or write-only.</span></span> <span data-ttu-id="ce7ed-677">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-677">For example:</span></span>

```vb
Class A
    Public Property P() As Integer
        Get
            ...
        End Get

        Private Set (Value As Integer)
            ...
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim a As A = New A()

        ' Error: A.P is read-only in this context.
        a.P = 10
    End Sub
End Module
```

<span data-ttu-id="ce7ed-678">Quando um tipo derivado é sombra de uma propriedade, a propriedade derivada oculta a propriedade sombreada em relação à leitura e gravação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-678">When a derived type shadows a property, the derived property hides the shadowed property with respect to both reading and writing.</span></span> <span data-ttu-id="ce7ed-679">No exemplo a seguir, o `P` propriedade na `B` oculta a `P` propriedade no `A` em relação à leitura e gravação:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-679">In the following example, the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing:</span></span>

```vb
Class A
    Public WriteOnly Property P() As Integer
        Set (Value As Integer)
        End Set
    End Property
End Class

Class B
    Inherits A

    Public Shadows ReadOnly Property P() As Integer
       Get
       End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As B = New B

        B.P = 10     ' Error, B.P is read-only.
    End Sub
End Module
```

<span data-ttu-id="ce7ed-680">O domínio de acessibilidade do tipo de retorno ou tipos de parâmetro deve ser igual ou um superconjunto do domínio de acessibilidade da propriedade em si.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-680">The accessibility domain of the return type or parameter types must be the same as or a superset of the accessibility domain of the property itself.</span></span> <span data-ttu-id="ce7ed-681">Uma propriedade só pode ter uma `Set` acessador e um `Get` acessador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-681">A property may only have one `Set` accessor and one `Get` accessor.</span></span>

<span data-ttu-id="ce7ed-682">Exceto pelas diferenças na sintaxe de declaração e chamada `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, e `MustInherit` propriedades se comportam exatamente como `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, e `MustInherit` métodos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-682">Except for differences in declaration and invocation syntax, `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` properties behave exactly like `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` methods.</span></span> <span data-ttu-id="ce7ed-683">Quando uma propriedade é substituída, a propriedade de substituição deve ser do mesmo tipo (leitura-gravação, somente leitura, somente gravação).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-683">When a property is overridden, the overriding property must be of the same type (read-write, read-only, write-only).</span></span> <span data-ttu-id="ce7ed-684">Uma `Overridable` propriedade não pode conter um `Private` acessador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-684">An `Overridable` property cannot contain a `Private` accessor.</span></span>

<span data-ttu-id="ce7ed-685">No exemplo a seguir `X` é um `Overridable` propriedade somente leitura, `Y` é uma `Overridable` leitura / gravação de propriedade, e `Z` é um `MustOverride` propriedade de leitura / gravação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-685">In the following example `X` is an `Overridable` read-only property, `Y` is an `Overridable` read-write property, and `Z` is a `MustOverride` read-write property.</span></span>

```vb
MustInherit Class A
    Private _y As Integer

    Public Overridable ReadOnly Property X() As Integer
        Get
            Return 0
        End Get
    End Property

    Public Overridable Property Y() As Integer
        Get
            Return _y
         End Get
        Set (Value As Integer)
            _y = value
        End Set
    End Property

    Public MustOverride Property Z() As Integer
End Class
```

<span data-ttu-id="ce7ed-686">Porque `Z` está `MustOverride`, o que contém a classe `A` deve ser declarado `MustInherit`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-686">Because `Z` is `MustOverride`, the containing class `A` must be declared `MustInherit`.</span></span>

<span data-ttu-id="ce7ed-687">Por outro lado, uma classe que deriva da classe `A` é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-687">By contrast, a class that derives from class `A` is shown below:</span></span>

```vb
Class B
    Inherits A

    Private _z As Integer

    Public Overrides ReadOnly Property X() As Integer
        Get
            Return MyBase.X + 1
        End Get
    End Property

    Public Overrides Property Y() As Integer
        Get
            Return MyBase.Y
        End Get
        Set (Value As Integer)
            If value < 0 Then
                MyBase.Y = 0
            Else
                MyBase.Y = Value
            End If
        End Set
    End Property

    Public Overrides Property Z() As Integer
        Get
            Return _z
        End Get
        Set (Value As Integer)
            _z = Value
        End Set
    End Property
End Class
```

<span data-ttu-id="ce7ed-688">Aqui, as declarações de propriedades `X`,`Y`, e `Z` substituir as propriedades de base.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-688">Here, the declarations of properties `X`,`Y`, and `Z` override the base properties.</span></span> <span data-ttu-id="ce7ed-689">Cada declaração de propriedade corresponde exatamente os modificadores de acessibilidade, o tipo e o nome da propriedade herdada correspondente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-689">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="ce7ed-690">O `Get` acessador de propriedade `X` e o `Set` acessador de propriedade `Y` usam o `MyBase` palavra-chave para acessar as propriedades herdadas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-690">The `Get` accessor of property `X` and the `Set` accessor of property `Y` use the `MyBase` keyword to access the inherited properties.</span></span> <span data-ttu-id="ce7ed-691">A declaração de propriedade `Z` substitui o `MustOverride` propriedade – assim, há não pendentes `MustOverride` membros na classe `B`, e `B` tem permissão para ser uma classe regular.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-691">The declaration of property `Z` overrides the `MustOverride` property -- thus, there are no outstanding `MustOverride` members in class `B`, and `B` is permitted to be a regular class.</span></span>

<span data-ttu-id="ce7ed-692">Propriedades podem ser usadas para atrasar a inicialização de um recurso até o momento em que ele é referenciado pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-692">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="ce7ed-693">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-693">For example:</span></span>

```vb
Imports System.IO

Public Class ConsoleStreams
    Private Shared reader As TextReader
    Private Shared writer As TextWriter
    Private Shared errors As TextWriter

    Public Shared ReadOnly Property [In]() As TextReader
        Get
            If reader Is Nothing Then
                reader = Console.In
            End If
            Return reader
        End Get
    End Property

    Public Shared ReadOnly Property Out() As TextWriter
        Get
            If writer Is Nothing Then
                writer = Console.Out
            End If
            Return writer
        End Get
    End Property

    Public Shared ReadOnly Property [Error]() As TextWriter
        Get
            If errors Is Nothing Then
                errors = Console.Error
            End If
            Return errors
        End Get
    End Property
End Class
```

<span data-ttu-id="ce7ed-694">O `ConsoleStreams` classe contém três propriedades: `In`, `Out`, e `Error`, que representam a entrada padrão, saída e dispositivos de erro, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-694">The `ConsoleStreams` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="ce7ed-695">Ao expor esses membros como propriedades, o `ConsoleStreams` classe pode atrasar sua inicialização até que eles são realmente usados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-695">By exposing these members as properties, the `ConsoleStreams` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="ce7ed-696">Por exemplo, após a primeira referência a `Out` propriedade, como na `ConsoleStreams.Out.WriteLine("hello, world")`, subjacente `TextWriter` para o dispositivo de saída é inicializado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-696">For example, upon first referencing the `Out` property, as in `ConsoleStreams.Out.WriteLine("hello, world")`, the underlying `TextWriter` for the output device is initialized.</span></span> <span data-ttu-id="ce7ed-697">Porém, se o aplicativo não faz referência à `In` e `Error` propriedades, em seguida, não há objetos são criados para esses dispositivos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-697">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>


### <a name="get-accessor-declarations"></a><span data-ttu-id="ce7ed-698">Obter declarações de acessador</span><span class="sxs-lookup"><span data-stu-id="ce7ed-698">Get Accessor Declarations</span></span>

<span data-ttu-id="ce7ed-699">Um `Get` acessador (getter) é declarado por meio de uma propriedade `Get` declaração.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-699">A `Get` accessor (getter) is declared by using a property `Get` declaration.</span></span> <span data-ttu-id="ce7ed-700">Uma propriedade `Get` declaração consiste na palavra-chave `Get` seguido de um bloco de instruções.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-700">A property `Get` declaration consists of the keyword `Get` followed by a statement block.</span></span> <span data-ttu-id="ce7ed-701">Dada uma propriedade chamada `P`, um `Get` declaração do acessador declara implicitamente um método com o nome `get_P` com o mesmos modificadores, o tipo e a lista de parâmetros como a propriedade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-701">Given a property named `P`, a `Get` accessor declaration implicitly declares a method with the name `get_P` with the same modifiers, type, and parameter list as the property.</span></span> <span data-ttu-id="ce7ed-702">Se o tipo contém uma declaração com esse nome, um erro de tempo de compilação, mas a declaração implícita é ignorada para fins de associação de nome.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-702">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="ce7ed-703">Uma variável local especial, que é declarada implicitamente no `Get` espaço de declaração do corpo do acessador com o mesmo nome que a propriedade representa o valor retornado da propriedade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-703">A special local variable, which is implicitly declared in the `Get` accessor body's declaration space with the same name as the property, represents the return value of the property.</span></span> <span data-ttu-id="ce7ed-704">A variável local tem semântica de resolução de nome especial quando usadas em expressões.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-704">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="ce7ed-705">Se a variável local é usada em um contexto que espera uma expressão que é classificada como um grupo de método, como uma expressão de invocação, em seguida, o nome é resolvido para a função em vez da variável local.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-705">If the local variable is used in a context that expects an expression that is classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="ce7ed-706">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-706">For example:</span></span>

```vb
ReadOnly Property F(i As Integer) As Integer
    Get
        If i = 0 Then
            F = 1    ' Sets the return value.
        Else
            F = F(i - 1) ' Recursive call.
        End If
    End Get
End Property
```

<span data-ttu-id="ce7ed-707">O uso de parênteses pode causar situações ambíguas (como `F(1)` onde `F` é uma propriedade cujo tipo é uma matriz unidimensional).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-707">The use of parentheses can cause ambiguous situations (such as `F(1)` where `F` is a property whose type is a one-dimensional array).</span></span> <span data-ttu-id="ce7ed-708">Em todas as situações ambíguas, o nome é resolvido para a propriedade em vez da variável local.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-708">In all ambiguous situations, the name resolves to the property rather than the local variable.</span></span> <span data-ttu-id="ce7ed-709">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-709">For example:</span></span>

```vb
ReadOnly Property F(i As Integer) As Integer()
    Get
        If i = 0 Then
            F = new Integer(2) { 1, 2, 3 }
        Else
            F = F(i - 1) ' Recursive call, not index.
        End If
    End Get
End Property
```

<span data-ttu-id="ce7ed-710">Quando o controle deixa de fluxo de `Get` corpo do acessador, o valor da variável local é passado para a expressão de invocação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-710">When control flow leaves the `Get` accessor body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="ce7ed-711">Como invocar um `Get` acessador é conceitualmente equivalente ao ler o valor de uma variável, ele é considerado um estilo ruim de programação para `Get` acessadores para ter efeito colateral observável, conforme ilustrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-711">Because invoking a `Get` accessor is conceptually equivalent to reading the value of a variable, it is considered bad programming style for `Get` accessors to have observable side effects, as illustrated in the following example:</span></span>

```vb
Class Counter
    Private Value As Integer

    Public ReadOnly Property NextValue() As Integer
        Get
            Value += 1
            Return Value
        End Get
    End Property
End Class
```

<span data-ttu-id="ce7ed-712">O valor da `NextValue` propriedade depende do número de vezes que a propriedade foi acessada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-712">The value of the `NextValue` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="ce7ed-713">Assim, acessar a propriedade produz um efeito colateral observável, e a propriedade em vez disso, deve ser implementada como um método.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-713">Thus, accessing the property produces an observable side effect, and the property should instead be implemented as a method.</span></span>

<span data-ttu-id="ce7ed-714">A convenção de "sem efeitos colaterais" para `Get` acessadores não significa que `Get` acessadores sempre devem ser escritos para simplesmente retornar valores armazenados em variáveis.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-714">The "no side effects" convention for `Get` accessors does not mean that `Get` accessors should always be written to simply return values stored in variables.</span></span> <span data-ttu-id="ce7ed-715">Na verdade, `Get` acessadores geralmente calcular o valor de uma propriedade ao acessar diversas variáveis ou invocar métodos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-715">Indeed, `Get` accessors often compute the value of a property by accessing multiple variables or invoking methods.</span></span> <span data-ttu-id="ce7ed-716">No entanto, projetado corretamente `Get` acessador não executará nenhuma ação que causam alterações observáveis no estado do objeto.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-716">However, a properly designed `Get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="ce7ed-717">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-717">__Note.__</span></span> <span data-ttu-id="ce7ed-718">`Get` acessadores têm a mesma restrição no posicionamento de linha que tem as sub-rotinas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-718">`Get` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="ce7ed-719">A instrução de início, a instrução end e o bloco devem aparecer no início de uma linha lógica.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-719">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a><span data-ttu-id="ce7ed-720">Declarações de acessador de conjunto</span><span class="sxs-lookup"><span data-stu-id="ce7ed-720">Set Accessor Declarations</span></span>

<span data-ttu-id="ce7ed-721">Um `Set` acessador (setter) é declarado usando uma declaração de propriedade de conjunto.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-721">A `Set` accessor (setter) is declared by using a property set declaration.</span></span> <span data-ttu-id="ce7ed-722">Uma declaração de propriedade de conjunto consiste na palavra-chave `Set`, uma lista de parâmetros opcional e um bloco de instruções.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-722">A property set declaration consists of the keyword `Set`, an optional parameter list, and a statement block.</span></span> <span data-ttu-id="ce7ed-723">Dada uma propriedade chamada `P`, uma declaração de setter declara implicitamente um método com o nome `set_P` com o mesmos modificadores e lista de parâmetros como a propriedade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-723">Given a property named `P`, a setter declaration implicitly declares a method with the name `set_P` with the same modifiers and parameter list as the property.</span></span> <span data-ttu-id="ce7ed-724">Se o tipo contém uma declaração com esse nome, um erro de tempo de compilação, mas a declaração implícita é ignorada para fins de associação de nome.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-724">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="ce7ed-725">Se uma lista de parâmetro for especificada, ele deve ter um membro, esse membro não deve ter nenhum modificador exceto `ByVal`, e seu tipo deve ser o mesmo que o tipo da propriedade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-725">If a parameter list is specified, it must have one member, that member must have no modifiers except `ByVal`, and its type must be the same as the type of the property.</span></span> <span data-ttu-id="ce7ed-726">O parâmetro representa o valor da propriedade que está sendo definido.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-726">The parameter represents the property value being set.</span></span> <span data-ttu-id="ce7ed-727">Se o parâmetro for omitido, um parâmetro chamado `Value` é declarado implicitamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-727">If the parameter is omitted, a parameter named `Value` is implicitly declared.</span></span>

<span data-ttu-id="ce7ed-728">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-728">__Note.__</span></span> <span data-ttu-id="ce7ed-729">`Set` acessadores têm a mesma restrição no posicionamento de linha que tem as sub-rotinas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-729">`Set` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="ce7ed-730">A instrução de início, a instrução end e o bloco devem aparecer no início de uma linha lógica.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-730">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a><span data-ttu-id="ce7ed-731">Propriedades padrão</span><span class="sxs-lookup"><span data-stu-id="ce7ed-731">Default Properties</span></span>

<span data-ttu-id="ce7ed-732">Uma propriedade que especifica o modificador `Default` é chamado de um *propriedade padrão*.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-732">A property that specifies the modifier `Default` is called a *default property*.</span></span> <span data-ttu-id="ce7ed-733">Qualquer tipo que permite que as propriedades pode ter uma propriedade padrão, incluindo interfaces.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-733">Any type that allows properties may have a default property, including interfaces.</span></span> <span data-ttu-id="ce7ed-734">A propriedade padrão pode ser referenciada sem precisar qualificar a instância com o nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-734">The default property may be referenced without having to qualify the instance with the name of the property.</span></span> <span data-ttu-id="ce7ed-735">Assim, dada a classe</span><span class="sxs-lookup"><span data-stu-id="ce7ed-735">Thus, given a class</span></span>

```vb
Class Test
    Public Default ReadOnly Property Item(i As Integer) As Integer
        Get
            Return i
        End Get
    End Property
End Class
```

<span data-ttu-id="ce7ed-736">O código</span><span class="sxs-lookup"><span data-stu-id="ce7ed-736">the code</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x(10)
    End Sub
End Module
```

<span data-ttu-id="ce7ed-737">equivale a</span><span class="sxs-lookup"><span data-stu-id="ce7ed-737">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x.Item(10)
    End Sub
End Module
```

<span data-ttu-id="ce7ed-738">Depois que uma propriedade é declarada `Default`, todas as propriedades sobrecarregadas nesse nome na hierarquia de herança tornam-se a propriedade padrão, se eles foram declarados `Default` ou não.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-738">Once a property is declared `Default`, all of the properties overloaded on that name in the inheritance hierarchy become the default property, whether they have been declared `Default` or not.</span></span> <span data-ttu-id="ce7ed-739">Declarar uma propriedade `Default` em uma classe derivada quando a classe base declarar uma propriedade padrão por outro nome não exige qualquer outros modificadores, como `Shadows` ou `Overrides`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-739">Declaring a property `Default` in a derived class when the base class declared a default property by another name does not require any other modifiers such as `Shadows` or `Overrides`.</span></span> <span data-ttu-id="ce7ed-740">Isso é porque a propriedade padrão não tem nenhuma identidade ou assinatura e, portanto, não pode ser sombreado ou sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-740">This is because the default property has no identity or signature and so cannot be shadowed or overloaded.</span></span> <span data-ttu-id="ce7ed-741">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-741">For example:</span></span>

```vb
Class Base
    Public ReadOnly Default Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Base = " & i)
        End Get
    End Property
End Class

Class Derived
    Inherits Base

    ' This hides Item, but does not change the default property.
    Public Shadows ReadOnly Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Derived = " & i)
        End Get
    End Property
End Class

Class MoreDerived
    Inherits Derived

    ' This declares a new default property, but not Item.
    ' This does not need to be declared Shadows
    Public ReadOnly Default Property Value(i As Integer) As Integer
        Get
            Console.WriteLine("MoreDerived = " & i)
        End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = New MoreDerived()
        Dim y As Integer
        Dim z As Derived = x

        y = x(10)        ' Calls MoreDerived.Value.
        y = x.Item(10)   ' Calls Derived.Item
        y = z(10)        ' Calls Base.Item
    End Sub
End Module
```

<span data-ttu-id="ce7ed-742">Este programa produzirá a saída:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-742">This program will produce the output:</span></span>

```
MoreDerived = 10
Derived = 10
Base = 10
```

<span data-ttu-id="ce7ed-743">Todas as propriedades padrão declaradas dentro de um tipo deve ter o mesmo nome e, para maior clareza, deve especificar o `Default` modificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-743">All default properties declared within a type must have the same name and, for clarity, must specify the `Default` modifier.</span></span> <span data-ttu-id="ce7ed-744">Como uma propriedade padrão sem parâmetros de índice resultaria em uma situação ambígua durante a atribuição de instâncias da classe recipiente, as propriedades padrão devem ter parâmetros de índice.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-744">Because a default property with no index parameters would cause an ambiguous situation when assigning instances of the containing class, default properties must have index parameters.</span></span> <span data-ttu-id="ce7ed-745">Além disso, se uma propriedade sobrecarregada em um determinado nome inclui o `Default` modificador, todas as propriedades sobrecarregadas nesse nome deverá especificá-lo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-745">Furthermore, if one property overloaded on a particular name includes the `Default` modifier, all properties overloaded on that name must specify it.</span></span> <span data-ttu-id="ce7ed-746">As propriedades padrão não podem ser `Shared`, e não deve ser pelo menos um acessador da propriedade `Private`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-746">Default properties may not be `Shared`, and at least one accessor of the property must not be `Private`.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="ce7ed-747">Propriedades implementadas automaticamente</span><span class="sxs-lookup"><span data-stu-id="ce7ed-747">Automatically Implemented Properties</span></span>

<span data-ttu-id="ce7ed-748">Se uma propriedade omite a declaração de qualquer acessadores, uma implementação da propriedade será fornecida automaticamente, a menos que a propriedade é declarada em uma interface ou é declarada `MustOverride`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-748">If a property omits declaration of any accessors, an implementation of the property will be automatically supplied unless the property is declared in an interface or is declared `MustOverride`.</span></span> <span data-ttu-id="ce7ed-749">Somente as propriedades de leitura/gravação sem argumentos podem ser implementadas automaticamente; Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-749">Only read/write properties with no arguments can be automatically implemented; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="ce7ed-750">Uma propriedade implementada automaticamente `x`, até mesmo uma substituição de outra propriedade, que apresenta uma variável local privada `_x` com o mesmo tipo da propriedade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-750">An automatically implemented property `x`, even one overriding another property, introduces a private local variable `_x` with the same type as the property.</span></span> <span data-ttu-id="ce7ed-751">Se houver uma colisão entre o nome da variável local e outra declaração, será relatado um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-751">If there is a collision between the local variable's name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="ce7ed-752">A propriedade implementada automaticamente `Get` acessador retorna o valor de local e a propriedade `Set` acessador que define o valor do local.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-752">The automatically implemented property's `Get` accessor returns the value of the local and the property's `Set` accessor that sets the value of the local.</span></span> <span data-ttu-id="ce7ed-753">Por exemplo, a declaração:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-753">For example, the declaration:</span></span>

```vb
Public Property x() As Integer
```

<span data-ttu-id="ce7ed-754">É aproximadamente equivalente a:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-754">is roughly equivalent to:</span></span>

```vb
Private _x As Integer
Public Property x() As Integer
    Get
        Return _x
    End Get
    Set (value As Integer)
        _x = value
    End Set
End Property
```

<span data-ttu-id="ce7ed-755">Assim como acontece com declarações de variável, uma propriedade implementada automaticamente pode incluir um inicializador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-755">As with variable declarations, an automatically implemented property can include an initializer.</span></span> <span data-ttu-id="ce7ed-756">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-756">For example:</span></span>

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

<span data-ttu-id="ce7ed-757">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-757">__Note.__</span></span> <span data-ttu-id="ce7ed-758">Quando uma propriedade implementada automaticamente é inicializada, ele é inicializado por meio da propriedade, não o campo subjacente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-758">When an automatically implemented property is initialized, it is initialized through the property, not the underlying field.</span></span> <span data-ttu-id="ce7ed-759">Isso é então substituindo propriedades pode interceptar a inicialização, se necessário.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-759">This is so overriding properties can intercept the initialization if they need to.</span></span>

<span data-ttu-id="ce7ed-760">Inicializadores de matriz são permitidos em propriedades implementadas automaticamente, exceto que não há nenhuma maneira de especificar os limites da matriz explicitamente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-760">Array initializers are allowed on automatically implemented properties, except that there is no way to specify the array bounds explicitly.</span></span>  <span data-ttu-id="ce7ed-761">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-761">For example:</span></span>

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a><span data-ttu-id="ce7ed-762">Propriedades de iterador</span><span class="sxs-lookup"><span data-stu-id="ce7ed-762">Iterator Properties</span></span>

<span data-ttu-id="ce7ed-763">Uma *propriedade de iterador* é uma propriedade com o `Iterator` modificador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-763">An *iterator property* is a property with the `Iterator` modifier.</span></span> <span data-ttu-id="ce7ed-764">Ele é usado pelo mesmo motivo que um método iterador (seção [métodos de iterador](statements.md#iterator-methods)) é usado, como uma maneira conveniente para gerar uma sequência, que podem ser consumidos pelo `For Each` instrução.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-764">It is used for the same reason an iterator method (Section [Iterator Methods](statements.md#iterator-methods)) is used -- as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="ce7ed-765">O `Get` acessador de propriedade de um iterador é interpretado da mesma forma como um método iterador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-765">The `Get` accessor of an iterator property is interpreted in the same way as an iterator method.</span></span>

<span data-ttu-id="ce7ed-766">A propriedade de um iterador deve ter um explícito `Get` acessador e seu tipo devem ser `IEnumerator`, ou `IEnumerable`, ou `IEnumerator(Of T)` ou `IEnumerable(Of T)` para alguns `T`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-766">An iterator property must have an explicit `Get` accessor, and its type must be `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`.</span></span>

<span data-ttu-id="ce7ed-767">Aqui está um exemplo de uma propriedade de iterador:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-767">Here is an example of an iterator property:</span></span>

```vb
Class Family
    Property Daughters As New List(Of String) From {"Beth", "Diane"}
    Property Sons As New List(Of String) From {"Abe", "Carl"}

    ReadOnly Iterator Property Children As IEnumerable(Of String)
        Get
            For Each name In Daughters : Yield name : Next
            For Each name In Sons : Yield name : Next
        End Get
    End Property
End Class

Module Module1
    Sub Main()
        Dim x As New Family
        For Each c In x.Children
            Console.WriteLine(c) ' prints Beth, Diane, Abe, Carl
        Next
    End Sub
End Module
```

## <a name="operators"></a><span data-ttu-id="ce7ed-768">Operadores</span><span class="sxs-lookup"><span data-stu-id="ce7ed-768">Operators</span></span>

<span data-ttu-id="ce7ed-769">*Operadores* são métodos que definem o significado de um operador existente do Visual Basic para a classe continente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-769">*Operators* are methods that define the meaning of an existing Visual Basic operator for the containing class.</span></span> <span data-ttu-id="ce7ed-770">Quando o operador é aplicado à classe em uma expressão, o operador é compilado em uma chamada para o método de operador definido na classe.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-770">When the operator is applied to the class in an expression, the operator is compiled into a call to the operator method defined in the class.</span></span> <span data-ttu-id="ce7ed-771">Definindo um operador para uma classe é também conhecido como *sobrecarga* o operador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-771">Defining an operator for a class is also known as *overloading* the operator.</span></span>

```antlr
OperatorDeclaration
    : Attributes? OperatorModifier* 'Operator' OverloadableOperator
      OpenParenthesis ParameterList CloseParenthesis
      ( 'As' Attributes? TypeName )? LineTerminator
      Block?
      'End' 'Operator' StatementTerminator
    ;

OperatorModifier
    : 'Public' | 'Shared' | 'Overloads' | 'Shadows' | 'Widening' | 'Narrowing'
    ;

OverloadableOperator
    : '+' | '-' | '*' | '/' | '\\' | '&' | 'Like' | 'Mod' | 'And' | 'Or' | 'Xor'
    | '^' | '<' '<' | '>' '>' | '=' | '<' '>' | '>' | '<' | '>' '=' | '<' '='
    | 'Not' | 'IsTrue' | 'IsFalse' | 'CType'
    ;
```

<span data-ttu-id="ce7ed-772">Não é possível sobrecarregar um operador que já existe; Na prática, isso principalmente se aplica aos operadores de conversão.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-772">It is not possible to overload an operator that already exists; in practice, this primarily applies to conversion operators.</span></span> <span data-ttu-id="ce7ed-773">Por exemplo, não é possível sobrecarregar a conversão de uma classe derivada em uma classe base:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-773">For example, it is not possible to overload the conversion from a derived class to a base class:</span></span>

```vb
Class Base
End Class

Class Derived
    ' Cannot redefine conversion from Derived to Base,
    ' conversion will be ignored.
    Public Shared Widening Operator CType(s As Derived) As Base
        ...
    End Operator
End Class
```

<span data-ttu-id="ce7ed-774">Operadores também podem ser sobrecarregados no sentido comum da palavra:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-774">Operators can also be overloaded in the common sense of the word:</span></span>

```vb
Class Base
    Public Shared Widening Operator CType(b As Base) As Integer
        ...
    End Operator

    Public Shared Narrowing Operator CType(i As Integer) As Base
        ...
    End Operator
End Class
```

<span data-ttu-id="ce7ed-775">Declarações de operador não adicione explicitamente nomes ao espaço de declaração do tipo recipiente. No entanto, eles declarar implicitamente um método correspondente, começando com os caracteres "op_".</span><span class="sxs-lookup"><span data-stu-id="ce7ed-775">Operator declarations do not explicitly add names to the containing type's declaration space; however they do implicitly declare a corresponding method starting with the characters "op_".</span></span> <span data-ttu-id="ce7ed-776">As seções a seguir listam os nomes de método correspondentes com cada operador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-776">The following sections list the corresponding method names with each operator.</span></span>

<span data-ttu-id="ce7ed-777">Há três classes de operadores que podem ser definidos: unário operadores, operadores binários e operadores de conversão.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-777">There are three classes of operators that can be defined: unary operators, binary operators and conversion operators.</span></span> <span data-ttu-id="ce7ed-778">Todas as declarações de operador compartilham algumas restrições:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-778">All operator declarations share certain restrictions:</span></span>

* <span data-ttu-id="ce7ed-779">Declarações do operador devem ser sempre `Public` e `Shared`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-779">Operator declarations must always be `Public` and `Shared`.</span></span> <span data-ttu-id="ce7ed-780">O `Public` modificador pode ser omitido em contextos em que o modificador será assumido.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-780">The `Public` modifier can be omitted in contexts where the modifier will be assumed.</span></span>

* <span data-ttu-id="ce7ed-781">Os parâmetros de um operador não podem ser declarados `ByRef`, `Optional` ou `ParamArray`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-781">The parameters of an operator cannot be declared `ByRef`, `Optional` or `ParamArray`.</span></span>

* <span data-ttu-id="ce7ed-782">O tipo de pelo menos um dos operandos ou o valor de retorno deve ser o tipo que contém o operador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-782">The type of at least one of the operands or the return value must be the type that contains the operator.</span></span>

* <span data-ttu-id="ce7ed-783">Não há nenhuma variável de retorno de função definida para operadores.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-783">There is no function return variable defined for operators.</span></span> <span data-ttu-id="ce7ed-784">Portanto, o `Return` declaração deve ser usada para retornar valores de um corpo de operador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-784">Therefore, the `Return` statement must be used to return values from an operator body.</span></span>

<span data-ttu-id="ce7ed-785">A única exceção a essas restrições se aplica a tipos de valor anuláveis.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-785">The only exception to these restrictions applies to nullable value types.</span></span> <span data-ttu-id="ce7ed-786">Como tipos de valor anuláveis não possuem uma definição de tipo real, um tipo de valor pode declarar operadores definidos pelo usuário para a versão que permite valor nula do tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-786">Since nullable value types do not have an actual type definition, a value type can declare user-defined operators for the nullable version of the type.</span></span> <span data-ttu-id="ce7ed-787">Ao determinar se um tipo pode declarar um operador definido pelo usuário específico, o `?` modificadores são descartados pela primeira vez em todos os tipos envolvidos na declaração para fins de verificação de validade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-787">When determining whether a type can declare a particular user-defined operator, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="ce7ed-788">Essa atenuação não se aplica ao tipo de retorno do `IsTrue` e `IsFalse` operadores; eles ainda devem retornar `Boolean`, e não `Boolean?`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-788">This relaxation does not apply to the return type of the `IsTrue` and `IsFalse` operators; they must still return `Boolean`, not `Boolean?`.</span></span>

<span data-ttu-id="ce7ed-789">Precedência e associatividade do operador não podem ser modificados por uma declaração do operador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-789">The precedence and associativity of an operator cannot be modified by an operator declaration.</span></span>

<span data-ttu-id="ce7ed-790">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-790">__Note.__</span></span> <span data-ttu-id="ce7ed-791">Os operadores têm a mesma restrição no posicionamento de linha que tem as sub-rotinas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-791">Operators have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="ce7ed-792">A instrução de início, a instrução end e o bloco devem aparecer no início de uma linha lógica.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-792">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>


### <a name="unary-operators"></a><span data-ttu-id="ce7ed-793">Operadores unários</span><span class="sxs-lookup"><span data-stu-id="ce7ed-793">Unary Operators</span></span>

<span data-ttu-id="ce7ed-794">Os seguintes operadores unários podem ser sobrecarregados:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-794">The following unary operators can be overloaded:</span></span>

* <span data-ttu-id="ce7ed-795">O operador unário de adição `+` (método correspondente: `op_UnaryPlus`)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-795">The unary plus operator `+` (corresponding method: `op_UnaryPlus`)</span></span>

* <span data-ttu-id="ce7ed-796">O operador de subtração unário `-` (método correspondente: `op_UnaryNegation`)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-796">The unary minus operator `-` (corresponding method: `op_UnaryNegation`)</span></span>

* <span data-ttu-id="ce7ed-797">A lógica `Not` operador (método correspondente: `op_OnesComplement`)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-797">The logical `Not` operator (corresponding method: `op_OnesComplement`)</span></span>

* <span data-ttu-id="ce7ed-798">O `IsTrue` e `IsFalse` operadores (métodos correspondentes: `op_True`, `op_False`)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-798">The `IsTrue` and `IsFalse` operators (corresponding methods: `op_True`, `op_False`)</span></span>

<span data-ttu-id="ce7ed-799">Todos os operadores unários sobrecarregado deve receber um único parâmetro do tipo recipiente e pode retornar qualquer tipo, exceto para `IsTrue` e `IsFalse`, que deve retornar `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-799">All overloaded unary operators must take a single parameter of the containing type and may return any type, except for `IsTrue` and `IsFalse`, which must return `Boolean`.</span></span> <span data-ttu-id="ce7ed-800">Se o tipo recipiente for um tipo genérico, os parâmetros de tipo devem corresponder os parâmetros de tipo do tipo recipiente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-800">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="ce7ed-801">Por exemplo,</span><span class="sxs-lookup"><span data-stu-id="ce7ed-801">For example,</span></span>

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

<span data-ttu-id="ce7ed-802">Se um tipo de uma das sobrecargas `IsTrue` ou `IsFalse`, em seguida, ele deverá sobrecarregar o outro também.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-802">If a type overloads one of `IsTrue` or `IsFalse`, then it must overload the other as well.</span></span> <span data-ttu-id="ce7ed-803">Se apenas um está sobrecarregado, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-803">If only one is overloaded, a compile-time error results.</span></span>

<span data-ttu-id="ce7ed-804">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-804">__Note.__</span></span> <span data-ttu-id="ce7ed-805">`IsTrue` e `IsFalse` não são palavras reservadas.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-805">`IsTrue` and `IsFalse` are not reserved words.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="ce7ed-806">Operadores binários</span><span class="sxs-lookup"><span data-stu-id="ce7ed-806">Binary Operators</span></span>

<span data-ttu-id="ce7ed-807">Os seguintes operadores binários podem ser sobrecarregados:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-807">The following binary operators can be overloaded:</span></span>

* <span data-ttu-id="ce7ed-808">A adição `+`, subtração `-`, multiplicação `*`, divisão `/`, divisão integral `\`, módulo `Mod` e a exponenciação `^` operadores (método correspondente: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-808">The addition `+`, subtraction `-`, multiplication `*`, division `/`, integral division `\`, modulo `Mod` and exponentiation `^` operators (corresponding method: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)</span></span>

* <span data-ttu-id="ce7ed-809">Os operadores relacionais `=`, `<>`, `<`, `>`, `<=`, `>=` (métodos correspondentes: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, `op_LessThanOrEqual` , `op_GreaterThanOrEqual`).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-809">The relational operators `=`, `<>`, `<`, `>`, `<=`, `>=` (corresponding methods: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, `op_LessThanOrEqual`, `op_GreaterThanOrEqual`).</span></span> <span data-ttu-id="ce7ed-810">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-810">__Note.__</span></span> <span data-ttu-id="ce7ed-811">Enquanto o operador de igualdade pode ser sobrecarregado, o operador de atribuição (usado somente em instruções de atribuição) não pode ser sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-811">While the equality operator can be overloaded, the assignment operator (used only in assignment statements) cannot be overloaded.</span></span>

* <span data-ttu-id="ce7ed-812">O `Like` operador (método correspondente: `op_Like`)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-812">The `Like` operator (corresponding method: `op_Like`)</span></span>

* <span data-ttu-id="ce7ed-813">O operador de concatenação `&` (método correspondente: `op_Concatenate`)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-813">The concatenation operator `&` (corresponding method: `op_Concatenate`)</span></span>

* <span data-ttu-id="ce7ed-814">A lógica `And`, `Or` e `Xor` operadores (métodos correspondentes: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-814">The logical `And`, `Or` and `Xor` operators (corresponding methods: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)</span></span>

* <span data-ttu-id="ce7ed-815">Os operadores shift `<<` e `>>` (métodos correspondentes: `op_LeftShift`, `op_RightShift`)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-815">The shift operators `<<` and `>>` (corresponding methods: `op_LeftShift`, `op_RightShift`)</span></span>

<span data-ttu-id="ce7ed-816">Todos os operadores binários sobrecarregados devem levar o tipo recipiente como um dos parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-816">All overloaded binary operators must take the containing type as one of the parameters.</span></span> <span data-ttu-id="ce7ed-817">Se o tipo recipiente for um tipo genérico, os parâmetros de tipo devem corresponder os parâmetros de tipo do tipo recipiente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-817">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="ce7ed-818">Os operadores shift ainda mais restringem essa regra para exigir que o primeiro parâmetro seja do tipo recipiente; o segundo parâmetro sempre deve ser do tipo `Integer`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-818">The shift operators further restrict this rule to require the first parameter to be of the containing type; the second parameter must always be of type `Integer`.</span></span>

<span data-ttu-id="ce7ed-819">Os seguintes operadores binários devem ser declarados em pares:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-819">The following binary operators must be declared in pairs:</span></span>

* <span data-ttu-id="ce7ed-820">Operador `=` e operador `<>`</span><span class="sxs-lookup"><span data-stu-id="ce7ed-820">Operator `=` and operator `<>`</span></span>

* <span data-ttu-id="ce7ed-821">Operador `>` e operador `<`</span><span class="sxs-lookup"><span data-stu-id="ce7ed-821">Operator `>` and operator `<`</span></span>

* <span data-ttu-id="ce7ed-822">Operador `>=` e operador `<=`</span><span class="sxs-lookup"><span data-stu-id="ce7ed-822">Operator `>=` and operator `<=`</span></span>

<span data-ttu-id="ce7ed-823">Se um do par é declarado, em seguida, o outro também deve ser declarado com parâmetros e tipos de retorno correspondentes, ou ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-823">If one of the pair is declared, then the other must also be declared with matching parameter and return types, or a compile-time error will result.</span></span> <span data-ttu-id="ce7ed-824">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-824">(__Note.__</span></span> <span data-ttu-id="ce7ed-825">A finalidade da exigência de emparelhado declarações de operadores relacionais é tentar garantir que pelo menos um nível mínimo de consistência lógica em operadores sobrecarregados.)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-825">The purpose of requiring paired declarations of relational operators is to try and ensure at least a minimum level of logical consistency in overloaded operators.)</span></span>

<span data-ttu-id="ce7ed-826">Em contraste com os operadores relacionais, a sobrecarga da divisão e operadores de divisão integral é recomendado, embora não um erro.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-826">In contrast to the relational operators, overloading both the division and integral division operators is strongly discouraged, although not an error.</span></span> <span data-ttu-id="ce7ed-827">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ce7ed-827">(__Note.__</span></span> <span data-ttu-id="ce7ed-828">Em geral, os dois tipos de divisão devem ser totalmente distintos: é um tipo que oferece suporte à divisão integral (nesse caso, ele deve dar suporte a `\`) ou não (nesse caso, ele deve dar suporte a `/`).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-828">In general, the two types of division should be entirely distinct: a type that supports division is either integral (in which case it should support `\`) or not (in which case it should support `/`).</span></span> <span data-ttu-id="ce7ed-829">Consideramos tornando um erro ao definir os dois operadores, mas porque suas linguagens em geral não fazem distinção entre dois tipos de divisão da forma faz do Visual Basic, nós achamos que era mais segura permitir que a prática, mas não recomendamos-lo.)</span><span class="sxs-lookup"><span data-stu-id="ce7ed-829">We considered making it an error to define both operators, but because their languages do not generally distinguish between two types of division the way Visual Basic does, we felt it was safest to allow the practice but strongly discourage it.)</span></span>

<span data-ttu-id="ce7ed-830">Operadores não podem ser sobrecarregados diretamente de atribuição composta.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-830">Compound assignment operators cannot be overloaded directly.</span></span> <span data-ttu-id="ce7ed-831">Em vez disso, quando o operador binário correspondente está sobrecarregado, o operador de atribuição composta usará o operador sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-831">Instead, when the corresponding binary operator is overloaded, the compound assignment operator will use the overloaded operator.</span></span> <span data-ttu-id="ce7ed-832">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-832">For example:</span></span>

```vb
Structure Complex
    ...

    Public Shared Operator +(x As Complex, y As Complex) _
        As Complex
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim c1, c2 As Complex
        ' Calls the overloaded + operator
        c1 += c2
    End Sub
End Module
```

### <a name="conversion-operators"></a><span data-ttu-id="ce7ed-833">Operadores de conversão</span><span class="sxs-lookup"><span data-stu-id="ce7ed-833">Conversion Operators</span></span>

<span data-ttu-id="ce7ed-834">Operadores de conversão definem novos conversões entre tipos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-834">Conversion operators define new conversions between types.</span></span> <span data-ttu-id="ce7ed-835">Essas novas conversões são chamadas *conversões definidas pelo usuário*.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-835">These new conversions are called *user-defined conversions*.</span></span> <span data-ttu-id="ce7ed-836">Um operador de conversão converte de um tipo de fonte, indicado pelo tipo de parâmetro de operador de conversão, para um tipo de destino, indicado pelo tipo de retorno do operador de conversão.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-836">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span> <span data-ttu-id="ce7ed-837">Conversões devem ser classificadas como ampliação ou redução.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-837">Conversions must be classified as either widening or narrowing.</span></span> <span data-ttu-id="ce7ed-838">Uma declaração do operador de conversão que inclui o `Widening` palavra-chave introduz uma conversão de ampliação definida pelo usuário (método correspondente: `op_Implicit`).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-838">A conversion operator declaration that includes the `Widening` keyword introduces a user-defined widening conversion (corresponding method: `op_Implicit`).</span></span> <span data-ttu-id="ce7ed-839">Uma declaração do operador de conversão que inclui o `Narrowing` palavra-chave introduz uma conversão redutora definidos pelo usuário (método correspondente: `op_Explicit`).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-839">A conversion operator declaration that includes the `Narrowing` keyword introduces a user-defined narrowing conversion (corresponding method: `op_Explicit`).</span></span>

<span data-ttu-id="ce7ed-840">Em geral, conversões de ampliação definida pelo usuário devem ser criadas para nunca lançam exceções e perder informações.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-840">In general, user-defined widening conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="ce7ed-841">Se uma conversão definida pelo usuário pode causar exceções (por exemplo, porque o argumento de origem está fora do intervalo) ou perda de informações (por exemplo, descartando os bits de ordem superior) e, em seguida, essa conversão deve ser definida como uma conversão de estreitamento.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-841">If a user-defined conversion can cause exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as a narrowing conversion.</span></span> <span data-ttu-id="ce7ed-842">No exemplo:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-842">In the example:</span></span>

```vb
Structure Digit
    Dim value As Byte

    Public Sub New(value As Byte)
        if value < 0 OrElse value > 9 Then Throw New ArgumentException()
        Me.value = value
    End Sub

    Public Shared Widening Operator CType(d As Digit) As Byte
        Return d.value
    End Operator

    Public Shared Narrowing Operator CType(b As Byte) As Digit
        Return New Digit(b)
    End Operator
End Structure
```

<span data-ttu-id="ce7ed-843">a conversão de `Digit` à `Byte` é uma conversão de ampliação porque nunca gera exceções ou perde informações, mas a conversão de `Byte` ao `Digit` é uma conversão redutora desde `Digit` pode representar apenas um subconjunto de possíveis valores de um `Byte`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-843">the conversion from `Digit` to `Byte` is a widening conversion because it never throws exceptions or loses information, but the conversion from `Byte` to `Digit` is a narrowing conversion since `Digit` can only represent a subset of the possible values of a `Byte`.</span></span>

<span data-ttu-id="ce7ed-844">Ao contrário de todos os outros membros de tipo que podem ser sobrecarregados, a assinatura de um operador de conversão inclui o tipo de destino da conversão.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-844">Unlike all other type members that can be overloaded, the signature of a conversion operator includes the target type of the conversion.</span></span> <span data-ttu-id="ce7ed-845">Isso é o único membro de tipo para o qual o tipo de retorno participa na assinatura.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-845">This is the only type member for which the return type participates in the signature.</span></span> <span data-ttu-id="ce7ed-846">Ampliação ou redução de classificação de um operador de conversão, no entanto, não é parte da assinatura do operador.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-846">The widening or narrowing classification of a conversion operator, however, is not part of the operator's signature.</span></span> <span data-ttu-id="ce7ed-847">Portanto, uma classe ou estrutura não pode declarar um operador de conversão de ampliação e um operador de conversão de estreitamento com a mesma fonte e tipos de destino.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-847">Thus, a class or structure cannot declare both a widening conversion operator and a narrowing conversion operator with the same source and target types.</span></span>

<span data-ttu-id="ce7ed-848">Um operador de conversão definida pelo usuário deve converter para ou do tipo recipiente – por exemplo, é possível que uma classe `C` para definir uma conversão de `C` para `Integer` bidirecionalmente `Integer` para `C`, mas não de `Integer` para `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-848">A user-defined conversion operator must convert either to or from the containing type -- for example, it is possible for a class `C` to define a conversion from `C` to `Integer` and from `Integer` to `C`, but not from `Integer` to `Boolean`.</span></span> <span data-ttu-id="ce7ed-849">Se o tipo recipiente for um tipo genérico, os parâmetros de tipo devem corresponder os parâmetros de tipo do tipo recipiente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-849">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="ce7ed-850">Além disso, não é possível redefinir uma conversão de intrínseca (ou seja, não-definido pelo usuário).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-850">Also, it is not possible to redefine an intrinsic (i.e. non-user-defined) conversion.</span></span> <span data-ttu-id="ce7ed-851">Como resultado, um tipo não pode declarar uma conversão em que:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-851">As a result, a type cannot declare a conversion where:</span></span>

* <span data-ttu-id="ce7ed-852">O tipo de origem e o tipo de destino são os mesmos.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-852">The source type and the destination type are the same.</span></span>

* <span data-ttu-id="ce7ed-853">O tipo de origem e o tipo de destino não são o tipo que define o operador de conversão.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-853">Both the source type and the destination type are not the type that defines the conversion operator.</span></span>

* <span data-ttu-id="ce7ed-854">O tipo de origem ou o tipo de destino é um tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-854">The source type or the destination type is an interface type.</span></span>

* <span data-ttu-id="ce7ed-855">O tipo de fonte e tipos de destino são relacionados por herança (incluindo `Object`).</span><span class="sxs-lookup"><span data-stu-id="ce7ed-855">The source type and destination types are related by inheritance (including `Object`).</span></span>

<span data-ttu-id="ce7ed-856">A única exceção a essas regras se aplica a tipos de valor anuláveis.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-856">The only exception to these rules applies to nullable value types.</span></span> <span data-ttu-id="ce7ed-857">Como tipos de valor anuláveis não possuem uma definição de tipo real, um tipo de valor pode declarar conversões definidas pelo usuário para a versão que permite valor nula do tipo.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-857">Since nullable value types do not have an actual type definition, a value type can declare user-defined conversions for the nullable version of the type.</span></span> <span data-ttu-id="ce7ed-858">Ao determinar se um tipo pode declarar uma conversão definida pelo usuário específica, o `?` modificadores são descartados pela primeira vez em todos os tipos envolvidos na declaração para fins de verificação de validade.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-858">When determining whether a type can declare a particular user-defined conversion, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="ce7ed-859">Portanto, a declaração a seguir é válida porque `S` pode definir uma conversão de `S` para `T`:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-859">Thus, the following declaration is valid because `S` can define a conversion from `S` to `T`:</span></span>

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Widening Operator CType(ByVal v As S?) As T
    ...
    End Operator
End Structure
```

<span data-ttu-id="ce7ed-860">A declaração a seguir não é válida, no entanto, como estrutura `S` não é possível definir uma conversão de `S` para `S`:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-860">The following declaration is not valid, however, because structure `S` cannot define a conversion from `S` to `S`:</span></span>

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a><span data-ttu-id="ce7ed-861">Mapeamento de operador</span><span class="sxs-lookup"><span data-stu-id="ce7ed-861">Operator Mapping</span></span>

<span data-ttu-id="ce7ed-862">Porque o conjunto de operadores que dá suporte ao Visual Basic pode não coincidir com o conjunto de operadores que outras linguagens no .NET Framework, alguns operadores são mapeadas especialmente em outros operadores quando está sendo definido ou usado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-862">Because the set of operators that Visual Basic supports may not exactly match the set of operators that other languages on the .NET Framework, some operators are mapped specially onto other operators when being defined or used.</span></span> <span data-ttu-id="ce7ed-863">Especificamente:</span><span class="sxs-lookup"><span data-stu-id="ce7ed-863">Specifically:</span></span>

* <span data-ttu-id="ce7ed-864">Definindo um operador de divisão integral definirá automaticamente um operador de divisão regulares (utilizável apenas a partir de outras linguagens) que chamará o operador de divisão integral.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-864">Defining an integral division operator will automatically define a regular division operator (usable only from other languages) that will call the integral division operator.</span></span>

* <span data-ttu-id="ce7ed-865">Sobrecarregando o `Not`, `And`, e `Or` operadores serão sobrecarregar somente o operador bit a bit da perspectiva de outras linguagens que distinguir entre os operadores lógicos e bit a bit.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-865">Overloading the `Not`, `And`, and `Or` operators will overload only the bitwise operator from the perspective of other languages that distinguish between logical and bitwise operators.</span></span>

* <span data-ttu-id="ce7ed-866">Uma classe que sobrecarrega apenas os operadores lógicos em uma linguagem que faz distinção entre os operadores lógicos e bit a bit (ou seja, um que usa linguagens `op_LogicalNot`, `op_LogicalAnd`, e `op_LogicalOr` para `Not`, `And`e `Or`, respectivamente) terão seus operadores lógicos mapeados para os operadores lógicos do Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-866">A class that overloads only the logical operators in a language that distinguishes between logical and bitwise operators (i.e. a languages that uses `op_LogicalNot`, `op_LogicalAnd`, and `op_LogicalOr` for `Not`, `And`, and `Or`, respectively) will have their logical operators mapped onto the Visual Basic logical operators.</span></span> <span data-ttu-id="ce7ed-867">Se os operadores lógicos e bit a bit são sobrecarregados, somente os operadores bit a bit serão usados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-867">If both the logical and bitwise operators are overloaded, only the bitwise operators will be used.</span></span>

* <span data-ttu-id="ce7ed-868">Sobrecarregando o `<<` e `>>` operadores serão sobrecarregar apenas os operadores com sinal da perspectiva de outras linguagens que distinguir entre operadores shift assinados e não assinados.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-868">Overloading the `<<` and `>>` operators will overload only the signed operators from the perspective of other languages that distinguish between signed and unsigned shift operators.</span></span>

* <span data-ttu-id="ce7ed-869">Uma classe que sobrecarrega um operador de deslocamento não assinado somente terá o operador de deslocamento não assinado mapeado para o operador de deslocamento do Visual Basic correspondente.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-869">A class that overloads only an unsigned shift operator will have the unsigned shift operator mapped onto the corresponding Visual Basic shift operator.</span></span> <span data-ttu-id="ce7ed-870">Se ambos os um operador shift sem sinal e assinado está sobrecarregado, somente o operador de deslocamento assinado será usado.</span><span class="sxs-lookup"><span data-stu-id="ce7ed-870">If both an unsigned and signed shift operator is overloaded, only the signed shift operator will be used.</span></span>
