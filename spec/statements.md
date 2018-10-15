# <a name="statements"></a><span data-ttu-id="0200d-101">Instruções</span><span class="sxs-lookup"><span data-stu-id="0200d-101">Statements</span></span>

<span data-ttu-id="0200d-102">Instruções representam o código executável.</span><span class="sxs-lookup"><span data-stu-id="0200d-102">Statements represent executable code.</span></span>

```antlr
Statement
    : LabelDeclarationStatement
    | LocalDeclarationStatement
    | WithStatement
    | SyncLockStatement
    | EventStatement
    | AssignmentStatement
    | InvocationStatement
    | ConditionalStatement
    | LoopStatement
    | ErrorHandlingStatement
    | BranchStatement
    | ArrayHandlingStatement
    | UsingStatement
    | AwaitStatement
    | YieldStatement
    ;
```

<span data-ttu-id="0200d-103">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0200d-103">__Note.__</span></span> <span data-ttu-id="0200d-104">O compilador do Microsoft Visual Basic permite apenas instruções que começam com uma palavra-chave ou um identificador.</span><span class="sxs-lookup"><span data-stu-id="0200d-104">The Microsoft Visual Basic Compiler only allows statements which start with a keyword or an identifier.</span></span> <span data-ttu-id="0200d-105">Assim, por exemplo, a instrução de chamada "`Call (Console).WriteLine`"é permitido, mas a instrução de chamada"`(Console).WriteLine`" não é.</span><span class="sxs-lookup"><span data-stu-id="0200d-105">Thus, for instance, the invocation statement "`Call (Console).WriteLine`" is allowed, but the invocation statement "`(Console).WriteLine`" is not.</span></span>

## <a name="control-flow"></a><span data-ttu-id="0200d-106">Fluxo de controle</span><span class="sxs-lookup"><span data-stu-id="0200d-106">Control Flow</span></span>

<span data-ttu-id="0200d-107">*Fluxo de controle* é a sequência na qual são executadas instruções e expressões.</span><span class="sxs-lookup"><span data-stu-id="0200d-107">*Control flow* is the sequence in which statements and expressions are executed.</span></span> <span data-ttu-id="0200d-108">A ordem de execução depende de determinada instrução ou expressão.</span><span class="sxs-lookup"><span data-stu-id="0200d-108">The order of execution depends on the particular statement or expression.</span></span>

<span data-ttu-id="0200d-109">Por exemplo, ao avaliar um operador de adição (seção [operador de adição](expressions.md#addition-operator)), primeiro o operando esquerdo é avaliado, em seguida, o operando à direita e, em seguida, o operador em si.</span><span class="sxs-lookup"><span data-stu-id="0200d-109">For example, when evaluating an addition operator (Section [Addition Operator](expressions.md#addition-operator)), first the left operand is evaluated, then the right operand, and then the operator itself.</span></span> <span data-ttu-id="0200d-110">Blocos são executados (seção [blocos e rótulos](statements.md#blocks-and-labels)) primeiro executando sua subdeclaração primeiro e, em seguida, continuar individualmente através das instruções do bloco.</span><span class="sxs-lookup"><span data-stu-id="0200d-110">Blocks are executed (Section [Blocks and Labels](statements.md#blocks-and-labels)) by first executing their first substatement, and then proceeding one by one through the statements of the block.</span></span>

<span data-ttu-id="0200d-111">Ordenação implícita em que isso é o conceito de um *ponto de controle*, que é a próxima operação a ser executada.</span><span class="sxs-lookup"><span data-stu-id="0200d-111">Implicit in this ordering is the concept of a *control point*, which is the next operation to be executed.</span></span> <span data-ttu-id="0200d-112">Quando um método é invocado (ou "chamado"), podemos dizer que ele cria uma *instância* do método.</span><span class="sxs-lookup"><span data-stu-id="0200d-112">When a method is invoked (or "called"), we say it creates an *instance* of the method.</span></span> <span data-ttu-id="0200d-113">Uma instância de método consiste em sua própria cópia dos parâmetros do método e variáveis locais e seu próprio ponto de controle.</span><span class="sxs-lookup"><span data-stu-id="0200d-113">A method instance consists of its own copy of the method's parameters and local variables, and its own control point.</span></span>

### <a name="regular-methods"></a><span data-ttu-id="0200d-114">Métodos regulares</span><span class="sxs-lookup"><span data-stu-id="0200d-114">Regular Methods</span></span>

<span data-ttu-id="0200d-115">Aqui está um exemplo de um método regular</span><span class="sxs-lookup"><span data-stu-id="0200d-115">Here is an example of a regular method</span></span>

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

<span data-ttu-id="0200d-116">Quando um método regular é invocado,</span><span class="sxs-lookup"><span data-stu-id="0200d-116">When a regular method is invoked,</span></span>

1. <span data-ttu-id="0200d-117">Primeiro, uma instância do método é criada específica para essa invocação.</span><span class="sxs-lookup"><span data-stu-id="0200d-117">First an instance of the method is created specific to that invocation.</span></span> <span data-ttu-id="0200d-118">Esta instância inclui uma cópia de todos os parâmetros e variáveis locais do método.</span><span class="sxs-lookup"><span data-stu-id="0200d-118">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="0200d-119">Em seguida, todos os seus parâmetros são inicializados para os valores fornecidos e todas as suas variáveis locais para os valores padrão de seus tipos.</span><span class="sxs-lookup"><span data-stu-id="0200d-119">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="0200d-120">No caso de uma `Function`, uma variável local implícita é inicializada também chamado de *variável de retorno de função* cujo nome é o nome da função, cujo tipo é o tipo de retorno da função e cujo valor inicial é o padrão de seu tipo.</span><span class="sxs-lookup"><span data-stu-id="0200d-120">In the case of a `Function`, an implicit local variable is also initialized called the *function return variable* whose name is the function's name, whose type is the return type of the function and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="0200d-121">Ponto de controle da instância de método, em seguida, é definido na primeira instrução do corpo do método e o corpo do método imediatamente começa a executar a partir daí (seção [blocos e rótulos](statements.md#blocks-and-labels)).</span><span class="sxs-lookup"><span data-stu-id="0200d-121">The method instance's control point is then set at the first statement of the method body, and the method body immediately starts to execute from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="0200d-122">Quando o fluxo de controle é encerrada o corpo do método normalmente – por meio de atingir o `End Function` ou `End Sub` que marcar seu fim, ou por meio de um explícito `Return` ou `Exit` statement - fluxo de controle retorna ao chamador da instância do método.</span><span class="sxs-lookup"><span data-stu-id="0200d-122">When control flow exits the method body normally - through reaching the `End Function` or `End Sub` that mark its end, or through an explicit `Return` or `Exit` statement - control flow returns to the caller of the method instance.</span></span> <span data-ttu-id="0200d-123">Se houver uma variável de retorno de função, o resultado da invocação é o valor final dessa variável.</span><span class="sxs-lookup"><span data-stu-id="0200d-123">If there is a function return variable, the result of the invocation is the final value of this variable.</span></span>

<span data-ttu-id="0200d-124">Quando o fluxo de controle sai do corpo do método por meio de uma exceção sem tratamento, essa exceção é propagada para o chamador.</span><span class="sxs-lookup"><span data-stu-id="0200d-124">When control flow exits the method body through an unhandled exception, that exception is propagated to the caller.</span></span>

<span data-ttu-id="0200d-125">Depois que o fluxo de controle foi encerrado, não há quaisquer referências ao vivo para a instância de método.</span><span class="sxs-lookup"><span data-stu-id="0200d-125">After control flow has exited, there are no longer any live references to the method instance.</span></span> <span data-ttu-id="0200d-126">Se a instância de método mantida apenas referências à sua cópia de variáveis locais ou parâmetros, eles podem ser coletadas como lixo.</span><span class="sxs-lookup"><span data-stu-id="0200d-126">If the method instance held the only references to its copy of local variables or parameters, then they may be garbage collected.</span></span>

### <a name="iterator-methods"></a><span data-ttu-id="0200d-127">Métodos de iterador</span><span class="sxs-lookup"><span data-stu-id="0200d-127">Iterator Methods</span></span>

<span data-ttu-id="0200d-128">Métodos de iterador são usados como uma maneira conveniente para gerar uma sequência, que podem ser consumidos pelo `For Each` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-128">Iterator methods are used as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="0200d-129">Métodos de iterador que usam o `Yield` instrução (seção [instrução Yield](statements.md#yield-statement)) para fornecer elementos da sequência.</span><span class="sxs-lookup"><span data-stu-id="0200d-129">Iterator methods use the `Yield` statement (Section [Yield Statement](statements.md#yield-statement)) to provide elements of the sequence.</span></span> <span data-ttu-id="0200d-130">(Um método iterador sem nenhum `Yield` instruções produzirá uma sequência vazia).</span><span class="sxs-lookup"><span data-stu-id="0200d-130">(An iterator method with no `Yield` statements will produce an empty sequence).</span></span> <span data-ttu-id="0200d-131">Aqui está um exemplo de um método iterador:</span><span class="sxs-lookup"><span data-stu-id="0200d-131">Here is an example of an iterator method:</span></span>

```vb
Iterator Function Test() As IEnumerable(Of Integer)
    Console.WriteLine("hello")
    Yield 1
    Yield 2
End Function

Dim en = Test()
For Each x In en          ' prints "hello" before the first x
    Console.WriteLine(x)  ' prints "1" and then "2"
Next
```

<span data-ttu-id="0200d-132">Quando um método iterador é invocado cujo tipo de retorno é `IEnumerator(Of T)`,</span><span class="sxs-lookup"><span data-stu-id="0200d-132">When an iterator method is invoked whose return type is `IEnumerator(Of T)`,</span></span>

1. <span data-ttu-id="0200d-133">Primeiro, uma instância do método iterador é criada específica para essa invocação.</span><span class="sxs-lookup"><span data-stu-id="0200d-133">First an instance of the iterator method is created specific to that invocation.</span></span> <span data-ttu-id="0200d-134">Esta instância inclui uma cópia de todos os parâmetros e variáveis locais do método.</span><span class="sxs-lookup"><span data-stu-id="0200d-134">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="0200d-135">Em seguida, todos os seus parâmetros são inicializados para os valores fornecidos e todas as suas variáveis locais para os valores padrão de seus tipos.</span><span class="sxs-lookup"><span data-stu-id="0200d-135">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="0200d-136">Uma variável local implícita é inicializada também chamado de *variável do iterador atual*, cujo tipo é `T` e cujo valor inicial é o padrão de seu tipo.</span><span class="sxs-lookup"><span data-stu-id="0200d-136">An implicit local variable is also initialized called the *iterator current variable*, whose type is `T` and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="0200d-137">Ponto de controle da instância de método, em seguida, é definido na primeira instrução do corpo do método.</span><span class="sxs-lookup"><span data-stu-id="0200d-137">The method instance's control point is then set at the first statement of the method body.</span></span>
5. <span data-ttu-id="0200d-138">Uma *objeto do iterador* , em seguida, é criado, associado a esta instância de método.</span><span class="sxs-lookup"><span data-stu-id="0200d-138">An *iterator object* is then created, associated with this method instance.</span></span> <span data-ttu-id="0200d-139">O objeto de iterador implementa o tipo de retorno declarado e tem um comportamento conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="0200d-139">The iterator object implements the declared return type and has behavior as described below.</span></span>
6. <span data-ttu-id="0200d-140">Fluxo de controle, em seguida, é retomado *imediatamente* no chamador e o resultado da invocação é o objeto de iterador.</span><span class="sxs-lookup"><span data-stu-id="0200d-140">Control flow is then resumed *immediately* in the caller, and the result of the invocation is the iterator object.</span></span> <span data-ttu-id="0200d-141">Observe que essa transferência é feita sem sair a instância do método iterador e não faz com que finalmente manipuladores executar.</span><span class="sxs-lookup"><span data-stu-id="0200d-141">Note that this transfer is done without exiting the iterator method instance, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="0200d-142">A instância de método ainda é referenciada pelo objeto de iterador, e não será lixo coletado desde que exista uma referência ao vivo para o objeto de iterador.</span><span class="sxs-lookup"><span data-stu-id="0200d-142">The method instance is still referenced by the iterator object, and will not be garbage collected so long as there exists a live reference to the iterator object.</span></span>

<span data-ttu-id="0200d-143">Quando o objeto de iterador `Current` propriedade é acessada, o *variável atual* da invocação é retornada.</span><span class="sxs-lookup"><span data-stu-id="0200d-143">When the iterator object's `Current` property is accessed, the *current variable* of the invocation is returned.</span></span>

<span data-ttu-id="0200d-144">Quando o objeto de iterador `MoveNext` método é invocado, a invocação não cria uma nova instância de método.</span><span class="sxs-lookup"><span data-stu-id="0200d-144">When the iterator object's `MoveNext` method is invoked, the invocation does not create a new method instance.</span></span> <span data-ttu-id="0200d-145">Em vez disso, a instância existente do método é usada (e seu ponto de controle e parâmetros e variáveis locais) - a instância que foi criada quando o método iterador foi invocado pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="0200d-145">Instead the existing method instance is used (and its control point and local variables and parameters) - the instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="0200d-146">Fluxo de controle retoma a execução no ponto de controle dessa instância de método e passa para o corpo do método iterador como de costume.</span><span class="sxs-lookup"><span data-stu-id="0200d-146">Control flow resumes execution at the control point of that method instance, and proceeds through the body of the iterator method as normal.</span></span>

<span data-ttu-id="0200d-147">Quando o objeto de iterador `Dispose` método é invocado, novamente a instância existente do método é usada.</span><span class="sxs-lookup"><span data-stu-id="0200d-147">When the iterator object's `Dispose` method is invoked, again the existing method instance is used.</span></span> <span data-ttu-id="0200d-148">Controlar fluxo continua do ponto de controle dessa instância de método, mas, em seguida, imediatamente se comporta como se um `Exit Function` instrução foram a próxima operação.</span><span class="sxs-lookup"><span data-stu-id="0200d-148">Control flow resumes at the control point of that method instance, but then immediately behaves as if an `Exit Function` statement were the next operation.</span></span>

<span data-ttu-id="0200d-149">As descrições acima do comportamento de invocação de `MoveNext` ou `Dispose` em um objeto de iterador só se aplicam se todos os últimos invocações de `MoveNext` ou `Dispose` nesse objeto de iterador já retornaram aos seus chamadores.</span><span class="sxs-lookup"><span data-stu-id="0200d-149">The above descriptions of behavior for invocation of `MoveNext` or `Dispose` on an iterator object only apply if all previous invocations of `MoveNext` or `Dispose` on that iterator object have already returned to their callers.</span></span> <span data-ttu-id="0200d-150">Se eles ainda não, o comportamento será indefinido.</span><span class="sxs-lookup"><span data-stu-id="0200d-150">If they haven't, then the behavior is undefined.</span></span>

<span data-ttu-id="0200d-151">Quando o fluxo de controle é encerrada o corpo do método iterador normalmente – por meio de atingir o `End Function` que marcar seu fim, ou por meio de um explícito `Return` ou `Exit` instrução – ele deve ter feito isso no contexto de uma invocação de `MoveNext` ou `Dispose` função em um objeto de iterador para retomar a instância do método iterador e ele será usa a instância de método que foi criada quando o método iterador foi invocado pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="0200d-151">When control flow exits the iterator method body normally -- through reaching the `End Function` that mark its end, or through an explicit `Return` or `Exit` statement -- it must have done so in the context of an invocation of `MoveNext` or `Dispose` function on an iterator object to resume the iterator method instance, and it will have been using the method instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="0200d-152">O ponto de controle dessa instância é deixado como o `End Function` instrução e retoma de fluxo de controle no chamador; e se ele tinha sido retomado por uma chamada para `MoveNext` , em seguida, o valor `False` é retornado ao chamador.</span><span class="sxs-lookup"><span data-stu-id="0200d-152">The control point of that instance is left at the `End Function` statement, and control flow resumes in the caller; and if it had been resumed by a call to `MoveNext` then the value `False` is returned to the caller.</span></span>

<span data-ttu-id="0200d-153">Quando o fluxo de controle sai o corpo do método iterador por meio de uma exceção sem tratamento e, em seguida, a exceção é propagado para o chamador, que novamente será uma invocação de `MoveNext` ou de `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="0200d-153">When control flow exits the iterator method body through an unhandled exception, then the exception is propagated to the caller, which again will be either an invocation of `MoveNext` or of `Dispose`.</span></span>

<span data-ttu-id="0200d-154">Assim como para os outros tipos de retorno possíveis de uma função de iterador</span><span class="sxs-lookup"><span data-stu-id="0200d-154">As for the other possible return types of an iterator function,</span></span>

* <span data-ttu-id="0200d-155">Quando um método iterador é invocado é cujo tipo de retorno `IEnumerable(Of T)` para alguns `T`, uma instância é criada pela primeira vez – específica para essa invocação do método iterador – de todos os parâmetros no método, e eles são inicializados com os valores fornecidos.</span><span class="sxs-lookup"><span data-stu-id="0200d-155">When an iterator method is invoked whose return type is `IEnumerable(Of T)` for some `T`, an instance is first created -- specific to that invocation of the iterator method -- of all parameters in the method, and they are initialized with the supplied values.</span></span> <span data-ttu-id="0200d-156">O resultado da invocação é uma um objeto que implementa o tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="0200d-156">The result of the invocation is an an object which implements the return type.</span></span> <span data-ttu-id="0200d-157">Deve este objeto `GetEnumerator` método ser chamado, ele cria uma instância – específica para essa invocação de `GetEnumerator` – de todos os parâmetros e variáveis locais no método.</span><span class="sxs-lookup"><span data-stu-id="0200d-157">Should this object's `GetEnumerator` method be called, it creates an instance -- specific to that invocation of `GetEnumerator` -- of all parameters and local variables in the method.</span></span> <span data-ttu-id="0200d-158">Ele inicializa os parâmetros para os valores já salvados e prossegue para o método iterador acima.</span><span class="sxs-lookup"><span data-stu-id="0200d-158">It initializes the parameters to the values already saved, and proceeds as for the iterator method above.</span></span>
* <span data-ttu-id="0200d-159">Quando um método iterador é invocado cujo tipo de retorno é a interface não genérica `IEnumerator`, o comportamento é exatamente como `IEnumerator(Of Object)`.</span><span class="sxs-lookup"><span data-stu-id="0200d-159">When an iterator method is invoked whose return type is the non-generic interface `IEnumerator`, the behavior is exactly as for `IEnumerator(Of Object)`.</span></span>
* <span data-ttu-id="0200d-160">Quando um método iterador é invocado cujo tipo de retorno é a interface não genérica `IEnumerable`, o comportamento é exatamente como `IEnumerable(Of Object)`.</span><span class="sxs-lookup"><span data-stu-id="0200d-160">When an iterator method is invoked whose return type is the non-generic interface `IEnumerable`, the behavior is exactly as for `IEnumerable(Of Object)`.</span></span>

### <a name="async-methods"></a><span data-ttu-id="0200d-161">Métodos assíncronos</span><span class="sxs-lookup"><span data-stu-id="0200d-161">Async Methods</span></span>

<span data-ttu-id="0200d-162">Os métodos assíncronos são uma maneira conveniente de fazer o trabalho de longa execução sem por exemplo, bloquear a interface do usuário de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0200d-162">Async methods are a convenient way to do long-running work without for example blocking the UI of an application.</span></span> <span data-ttu-id="0200d-163">Async é a abreviação de *Asynchronous* -isso significa que o chamador do método assíncrono retomará sua execução imediatamente, mas a eventual conclusão da instância do método assíncrono pode acontecer em um momento posterior no futuro.</span><span class="sxs-lookup"><span data-stu-id="0200d-163">Async is short for *Asynchronous* - it means that the caller of the async method will resume its execution promptly, but the eventual completion of the async method's instance may happen at some later time in the future.</span></span> <span data-ttu-id="0200d-164">Por convenção, os métodos assíncronos são nomeados com o sufixo "Async".</span><span class="sxs-lookup"><span data-stu-id="0200d-164">By convention async methods are named with the suffix "Async".</span></span>

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

<span data-ttu-id="0200d-165">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0200d-165">__Note.__</span></span> <span data-ttu-id="0200d-166">Os métodos assíncronos são *não* executado em um thread em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="0200d-166">Async methods are *not* run on a background thread.</span></span> <span data-ttu-id="0200d-167">Em vez disso, eles permitem que um método para suspender a mesmo por meio de `Await` operador e o agendamento em si para ser retomada em resposta a um evento.</span><span class="sxs-lookup"><span data-stu-id="0200d-167">Instead they allow a method to suspend itself through the `Await` operator, and schedule itself to be resumed in response to some event.</span></span>

<span data-ttu-id="0200d-168">Quando um método assíncrono é invocado</span><span class="sxs-lookup"><span data-stu-id="0200d-168">When an async method is invoked</span></span>

1. <span data-ttu-id="0200d-169">Primeiro, uma instância do método assíncrono é criada específica para essa invocação.</span><span class="sxs-lookup"><span data-stu-id="0200d-169">First an instance of the async method is created specific to that invocation.</span></span> <span data-ttu-id="0200d-170">Esta instância inclui uma cópia de todos os parâmetros e variáveis locais do método.</span><span class="sxs-lookup"><span data-stu-id="0200d-170">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="0200d-171">Em seguida, todos os seus parâmetros são inicializados para os valores fornecidos e todas as suas variáveis locais para os valores padrão de seus tipos.</span><span class="sxs-lookup"><span data-stu-id="0200d-171">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="0200d-172">No caso de um método assíncrono com o tipo de retorno `Task(Of T)` para alguns `T`, uma variável local implícita é inicializada também chamada a *variável de retorno de tarefa*, cujo tipo é `T` e cujo valor inicial é o padrão de `T`.</span><span class="sxs-lookup"><span data-stu-id="0200d-172">In the case of an async method with return type `Task(Of T)` for some `T`, an implicit local variable is also initialized called the *task return variable*, whose type is `T` and whose initial value is the default of `T`.</span></span>
4. <span data-ttu-id="0200d-173">Se o método assíncrono é uma `Function` com o tipo de retorno `Task` ou `Task(Of T)` para alguns `T`, em seguida, um objeto desse tipo criadas implicitamente, associado com a invocação da atual.</span><span class="sxs-lookup"><span data-stu-id="0200d-173">If the async method is a `Function` with return type `Task` or `Task(Of T)` for some `T`, then an object of that type implicitly created, associated with the current invocation.</span></span> <span data-ttu-id="0200d-174">Isso é chamado de um *objeto async* e representa o trabalho futuro que será feito com a execução da instância do método assíncrono.</span><span class="sxs-lookup"><span data-stu-id="0200d-174">This is called an *async object* and represents the future work that will be done by executing the instance of the async method.</span></span> <span data-ttu-id="0200d-175">Quando o controle continua no chamador desta instância de método assíncrono, o chamador recebe esse objeto async como resultado de sua invocação.</span><span class="sxs-lookup"><span data-stu-id="0200d-175">When control resumes in the caller of this async method instance, the caller will receive this async object as the result of its invocation.</span></span>
5. <span data-ttu-id="0200d-176">Ponto de controle da instância, em seguida, é definido na primeira instrução do corpo do método assíncrono e imediatamente começa a executar o corpo do método a partir daí (seção [blocos e rótulos](statements.md#blocks-and-labels)).</span><span class="sxs-lookup"><span data-stu-id="0200d-176">The instance's control point is then set at the first statement of the async method body, and immediately starts to execute the method body from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="0200d-177">__Delegado de continuação e o chamador atual__</span><span class="sxs-lookup"><span data-stu-id="0200d-177">__Resumption Delegate and Current Caller__</span></span>

<span data-ttu-id="0200d-178">Conforme detalhado na seção [operador Await](expressions.md#await-operator), a execução de um `Await` expressão tem a capacidade de suspender o ponto de controle da instância de método deixando o fluxo de controle para ir em outro lugar.</span><span class="sxs-lookup"><span data-stu-id="0200d-178">As detailed in Section [Await Operator](expressions.md#await-operator), execution of an `Await` expression has the ability to suspend the method instance's control point leaving control flow to go elsewhere.</span></span> <span data-ttu-id="0200d-179">Fluxo de controle pode retomar posteriormente no ponto de controle da mesma instância por meio de invocação de um *delegado de continuação*.</span><span class="sxs-lookup"><span data-stu-id="0200d-179">Control flow can later resume at the same instance's control point through invocation of a *resumption delegate*.</span></span> <span data-ttu-id="0200d-180">Observe que dessa suspensão é feita sem sair do método assíncrono e não faz com que finalmente manipuladores executar.</span><span class="sxs-lookup"><span data-stu-id="0200d-180">Note that this suspension is done without exiting the async method, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="0200d-181">A instância de método ainda é referenciada pelo delegado de continuação e o `Task` ou `Task(Of T)` resultar (se houver), e não será lixo coletado desde que exista uma referência ao vivo para delegar ou resultar.</span><span class="sxs-lookup"><span data-stu-id="0200d-181">The method instance is still referenced by both the resumption delegate and the `Task` or `Task(Of T)` result (if any), and will not be garbage collected so long as there exists a live reference to either delegate or result.</span></span>

<span data-ttu-id="0200d-182">Vale a pena imaginar a instrução `Dim x = Await WorkAsync()` aproximadamente como um atalho sintático para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="0200d-182">It is helpful to imagine the statement `Dim x = Await WorkAsync()` approximately as syntactic shorthand for the following:</span></span>

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

<span data-ttu-id="0200d-183">A seguir, o *chamador atual* do método de instância é definida como o chamador original ou o chamador do delegado de continuação, o que for mais recente.</span><span class="sxs-lookup"><span data-stu-id="0200d-183">In the following, the *current caller* of the method instance is defined as either the original caller, or the caller of the resumption delegate, whichever is more recent.</span></span>

<span data-ttu-id="0200d-184">Quando o fluxo de controle é encerrada o corpo do método async – por meio de atingir o `End Sub` ou `End Function` que marcar seu fim, ou por meio de um explícito `Return` ou `Exit` instrução, ou por meio de uma exceção sem tratamento, o ponto de controle da instância é definido como o final do método.</span><span class="sxs-lookup"><span data-stu-id="0200d-184">When control flow exits the async method body -- through reaching the `End Sub` or `End Function` that mark its end, or through an explicit `Return` or `Exit` statement, or through an unhandled exception -- the instance's control point is set to the end of the method.</span></span> <span data-ttu-id="0200d-185">Em seguida, comportamento depende do tipo de retorno do método assíncrono.</span><span class="sxs-lookup"><span data-stu-id="0200d-185">Behavior then depends on the return type of the async method.</span></span>

* <span data-ttu-id="0200d-186">No caso de um `Async Function` com o tipo de retorno `Task`:</span><span class="sxs-lookup"><span data-stu-id="0200d-186">In the case of an `Async Function` with return type `Task`:</span></span>
  1. <span data-ttu-id="0200d-187">Se o fluxo de controle sai por meio de uma exceção sem tratamento, em seguida, status assíncrono do objeto é definido como `TaskStatus.Faulted` e sua `Exception.InnerException` estiver definida como a exceção (exceto: determinadas exceções definido pela implementação, como `OperationCanceledException` alterá-lo para `TaskStatus.Canceled`).</span><span class="sxs-lookup"><span data-stu-id="0200d-187">If control flow exits through an unhandled exception, then the async object's status is set to `TaskStatus.Faulted` and its `Exception.InnerException` property is set to the exception (except: certain implementation-defined exceptions such as `OperationCanceledException` change it to `TaskStatus.Canceled`).</span></span> <span data-ttu-id="0200d-188">Retoma de fluxo de controle em que o chamador atual.</span><span class="sxs-lookup"><span data-stu-id="0200d-188">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="0200d-189">Caso contrário, o status assíncrono do objeto é definido como `TaskStatus.Completed`.</span><span class="sxs-lookup"><span data-stu-id="0200d-189">Otherwise, the async object's status is set to `TaskStatus.Completed`.</span></span> <span data-ttu-id="0200d-190">Retoma de fluxo de controle em que o chamador atual.</span><span class="sxs-lookup"><span data-stu-id="0200d-190">Control flow resumes in the current caller.</span></span>

     <span data-ttu-id="0200d-191">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0200d-191">(__Note.__</span></span> <span data-ttu-id="0200d-192">Todo ponto de tarefa e o que torna os métodos assíncronos interessante, é que, quando uma tarefa se torna concluído e em seguida, todos os métodos que estavam aguardando ele atualmente terão seus representantes de continuação executadas, ou seja, eles se tornarão desbloqueados.)</span><span class="sxs-lookup"><span data-stu-id="0200d-192">The whole point of Task, and what makes async methods interesting, is that when a task becomes Completed then any methods that were awaiting it will presently have their resumption delegates executed, i.e. they will become unblocked.)</span></span>

* <span data-ttu-id="0200d-193">No caso de um `Async Function` com o tipo de retorno `Task(Of T)` para alguns `T`: o comportamento é como acima, exceto que em exceção não casos, o objeto de async `Result` propriedade também é definida como o valor final da variável de retorno de tarefa.</span><span class="sxs-lookup"><span data-stu-id="0200d-193">In the case of an `Async Function` with return type `Task(Of T)` for some `T`: the behavior is as above, except that in non-exception cases the async object's `Result` property is also set to the final value of the task return variable.</span></span>

* <span data-ttu-id="0200d-194">No caso de um `Async Sub`:</span><span class="sxs-lookup"><span data-stu-id="0200d-194">In the case of an `Async Sub`:</span></span>
  1. <span data-ttu-id="0200d-195">Se o fluxo de controle sai por meio de uma exceção sem tratamento, em seguida, essa exceção é propagada para o ambiente de alguma maneira de específico da implementação.</span><span class="sxs-lookup"><span data-stu-id="0200d-195">If control flow exits through an unhandled exception, then that exception is propagated to the environment in some implementation-specific manner.</span></span> <span data-ttu-id="0200d-196">Retoma de fluxo de controle em que o chamador atual.</span><span class="sxs-lookup"><span data-stu-id="0200d-196">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="0200d-197">Caso contrário, o fluxo de controle simplesmente retoma no chamador atual.</span><span class="sxs-lookup"><span data-stu-id="0200d-197">Otherwise, control flow simply resumes in the current caller.</span></span>

#### <a name="async-sub"></a><span data-ttu-id="0200d-198">Sub Async</span><span class="sxs-lookup"><span data-stu-id="0200d-198">Async Sub</span></span>

<span data-ttu-id="0200d-199">Há alguns comportamentos específicos da Microsoft de um `Async Sub`.</span><span class="sxs-lookup"><span data-stu-id="0200d-199">There is some Microsoft-specific behavior of an `Async Sub`.</span></span>

<span data-ttu-id="0200d-200">Se `SynchronizationContext.Current` é `Nothing` no início da invocação, em seguida, todas as exceções sem tratamento de uma Async Sub serão lançadas para o pool de threads.</span><span class="sxs-lookup"><span data-stu-id="0200d-200">If `SynchronizationContext.Current` is `Nothing` at the start of the invocation, then any unhandled exceptions from an Async Sub will be posted to the Threadpool.</span></span>

<span data-ttu-id="0200d-201">Se `SynchronizationContext.Current` não é `Nothing` no início da chamada, em seguida, `OperationStarted()` é invocado no contexto de antes do início do método e `OperationCompleted()` após o término.</span><span class="sxs-lookup"><span data-stu-id="0200d-201">If `SynchronizationContext.Current` is not `Nothing` at the start of the invocation, then `OperationStarted()` is invoked on that context before the start of the method and `OperationCompleted()` after the end.</span></span> <span data-ttu-id="0200d-202">Além disso, qualquer exceção sem tratamento será lançada para ser relançada no contexto de sincronização.</span><span class="sxs-lookup"><span data-stu-id="0200d-202">Additionally, any unhandled exceptions will be posted to be rethrown on the synchronization context.</span></span>

<span data-ttu-id="0200d-203">Isso significa que em aplicativos de interface do usuário, para um `Async Sub` que é invocado no thread da interface do usuário, todas as exceções que ele não consegue manipular serão ser relançadas o thread de interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="0200d-203">This means that in UI applications, for an `Async Sub` that is invoked on the UI thread, any exceptions it fails to handle will be reposted the UI thread.</span></span>

#### <a name="mutable-structures-in-async-and-iterator-methods"></a><span data-ttu-id="0200d-204">Estruturas mutáveis em métodos assíncrono e iterador</span><span class="sxs-lookup"><span data-stu-id="0200d-204">Mutable structures in async and iterator methods</span></span>

<span data-ttu-id="0200d-205">Estrutura mutável em geral é considerada uma prática inadequada e não têm suporte pelos métodos assíncrono ou iterador.</span><span class="sxs-lookup"><span data-stu-id="0200d-205">Mutable structures in general are considered bad practice, and they are not supported by async or iterator methods.</span></span> <span data-ttu-id="0200d-206">Em particular, cada invocação de um método assíncrono ou iterador em uma estrutura implicitamente opera em um *cópia* dessa estrutura que é copiada em seu momento da invocação.</span><span class="sxs-lookup"><span data-stu-id="0200d-206">In particular, each invocation of an async or iterator method in a structure will implicitly operate on a *copy* of that structure that is copied at its moment of invocation.</span></span> <span data-ttu-id="0200d-207">Assim, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="0200d-207">Thus, for example,</span></span>

```vb
Structure S
       Dim x As Integer
       Async Sub Mutate()
           x = 2
       End Sub
End Structure

Dim s As New S With {.x = 1}
s.Mutate()
Console.WriteLine(s.x)   ' prints "1"
```

### <a name="blocks-and-labels"></a><span data-ttu-id="0200d-208">Blocos e rótulos</span><span class="sxs-lookup"><span data-stu-id="0200d-208">Blocks and Labels</span></span>

<span data-ttu-id="0200d-209">Um grupo de instruções executáveis é chamado de um bloco de instruções.</span><span class="sxs-lookup"><span data-stu-id="0200d-209">A group of executable statements is called a statement block.</span></span> <span data-ttu-id="0200d-210">Execução de um bloco de instrução começa com a primeira instrução no bloco.</span><span class="sxs-lookup"><span data-stu-id="0200d-210">Execution of a statement block begins with the first statement in the block.</span></span> <span data-ttu-id="0200d-211">Após a execução de uma instrução, a próxima instrução em ordem léxica é executada, a menos que uma instrução transfere a execução em outro lugar ou ocorrerá uma exceção.</span><span class="sxs-lookup"><span data-stu-id="0200d-211">Once a statement has been executed, the next statement in lexical order is executed, unless a statement transfers execution elsewhere or an exception occurs.</span></span>

<span data-ttu-id="0200d-212">Dentro de um bloco de instrução, a divisão de instruções em linhas lógicas não é significativa com exceção de instruções de declaração de rótulo.</span><span class="sxs-lookup"><span data-stu-id="0200d-212">Within a statement block, the division of statements on logical lines is not significant with the exception of label declaration statements.</span></span> <span data-ttu-id="0200d-213">Um rótulo é um identificador que identifica uma posição específica dentro do bloco de instrução que pode ser usado como o destino de uma instrução de ramificação, como `GoTo`.</span><span class="sxs-lookup"><span data-stu-id="0200d-213">A label is an identifier that identifies a particular position within the statement block that can be used as the target of a branch statement such as `GoTo`.</span></span>

```antlr
Block
    : Statements*
    ;

LabelDeclarationStatement
    : LabelName ':'
    ;

LabelName
    : Identifier
    | IntLiteral
    ;

Statements
    : Statement? ( ':' Statement? )*
    ;
```


<span data-ttu-id="0200d-214">Instruções de declaração de rótulo devem aparecer no início de uma linha lógica e os rótulos podem ser um identificador ou um literal inteiro.</span><span class="sxs-lookup"><span data-stu-id="0200d-214">Label declaration statements must appear at the beginning of a logical line and labels may be either an identifier or an integer literal.</span></span> <span data-ttu-id="0200d-215">Porque as instruções de declaração de rótulo e instruções de invocação podem consistir em um único identificador, um único identificador no início de uma linha local sempre é considerado uma instrução de declaração de rótulo.</span><span class="sxs-lookup"><span data-stu-id="0200d-215">Because both label declaration statements and invocation statements can consist of a single identifier, a single identifier at the beginning of a local line is always considered a label declaration statement.</span></span> <span data-ttu-id="0200d-216">Instruções de declaração de rótulo sempre devem ser seguidas por dois-pontos, mesmo se nenhuma instrução execute na mesma linha lógica.</span><span class="sxs-lookup"><span data-stu-id="0200d-216">Label declaration statements must always be followed by a colon, even if no statements follow on the same logical line.</span></span>

<span data-ttu-id="0200d-217">Rótulos tenham seu próprio espaço de declaração e não interfiram com outros identificadores.</span><span class="sxs-lookup"><span data-stu-id="0200d-217">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="0200d-218">O exemplo a seguir é válido e usa a variável de nome `x` como um parâmetro e um rótulo.</span><span class="sxs-lookup"><span data-stu-id="0200d-218">The following example is valid and uses the name variable `x` both as a parameter and as a label.</span></span>

```vb
Function F(x As Integer) As Integer
    If x >= 0 Then
        GoTo x
    End If
    x = -x
x: 
    Return x
End Function
```

<span data-ttu-id="0200d-219">O escopo de um rótulo é o corpo do método que o contém.</span><span class="sxs-lookup"><span data-stu-id="0200d-219">The scope of a label is the body of the method containing it.</span></span>

<span data-ttu-id="0200d-220">Para fins de legibilidade, produções de instrução que envolvem vários substatements são tratadas como uma única produção nesta especificação, mesmo que o substatements cada talvez por si mesmos em uma linha rotulada.</span><span class="sxs-lookup"><span data-stu-id="0200d-220">For the sake of readability, statement productions that involve multiple substatements are treated as a single production in this specification, even though the substatements may each be by themselves on a labeled line.</span></span>


### <a name="local-variables-and-parameters"></a><span data-ttu-id="0200d-221">Parâmetros e variáveis locais</span><span class="sxs-lookup"><span data-stu-id="0200d-221">Local Variables and Parameters</span></span>

<span data-ttu-id="0200d-222">Anterior seções detalhe como e quando são criadas instâncias de método e com elas, as cópias das variáveis locais e parâmetros de um método.</span><span class="sxs-lookup"><span data-stu-id="0200d-222">The preceding sections detail how and when method instances are created, and with them the copies of a method's local variables and parameters.</span></span> <span data-ttu-id="0200d-223">Além disso, sempre que o corpo de um loop for inserido, uma nova cópia é feita de cada variável local declarada dentro desse loop, conforme descrito na seção [instruções de Loop](statements.md#loop-statements), e a instância de método agora contém esta cópia de sua variável local em vez disso que a cópia anterior.</span><span class="sxs-lookup"><span data-stu-id="0200d-223">In addition, each time the body of a loop is entered, a new copy is made of each local variable declared inside that loop as described in Section [Loop Statements](statements.md#loop-statements), and the method instance now contains this copy of its local variable rather than the previous copy.</span></span>

<span data-ttu-id="0200d-224">Todos os locais são inicializados para seu valor padrão de tipo.</span><span class="sxs-lookup"><span data-stu-id="0200d-224">All locals are initialized to their type's default value.</span></span> <span data-ttu-id="0200d-225">Parâmetros e variáveis locais são sempre acessíveis publicamente.</span><span class="sxs-lookup"><span data-stu-id="0200d-225">Local variables and parameters are always publicly accessible.</span></span> <span data-ttu-id="0200d-226">É um erro se referir a uma variável local em uma posição textual que precede a sua declaração, como mostra o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="0200d-226">It is an error to refer to a local variable in a textual position that precedes its declaration, as the following example illustrates:</span></span>

```vb
Class A
    Private i As Integer = 0

    Sub F()
        i = 1
        Dim i As Integer       ' Error, use precedes declaration.
        i = 2
    End Sub

    Sub G()
        Dim a As Integer = 1
        Dim b As Integer = a   ' This is valid.
    End Sub
End Class
```

<span data-ttu-id="0200d-227">No `F` método acima, a primeira atribuição para `i` especificamente não se refere ao campo declarado no escopo externo.</span><span class="sxs-lookup"><span data-stu-id="0200d-227">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="0200d-228">Em vez disso, ele se refere à variável local e ele é um erro porque ele textualmente precede a declaração da variável.</span><span class="sxs-lookup"><span data-stu-id="0200d-228">Rather, it refers to the local variable, and it is in error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="0200d-229">No `G` método, uma declaração de variável subsequente se refere a uma variável local declarada em uma declaração de variável anterior na mesma declaração de variável local.</span><span class="sxs-lookup"><span data-stu-id="0200d-229">In the `G` method, a subsequent variable declaration refers to a local variable declared in an earlier variable declaration within the same local variable declaration.</span></span>

<span data-ttu-id="0200d-230">Cada bloco em um método cria um espaço de declaração para variáveis locais.</span><span class="sxs-lookup"><span data-stu-id="0200d-230">Each block in a method creates a declaration space for local variables.</span></span> <span data-ttu-id="0200d-231">Os nomes são introduzidos nesse espaço de declaração, por meio de declarações de variável local no corpo do método e lista de parâmetros do método, que apresenta os nomes no espaço de declaração do bloco externo.</span><span class="sxs-lookup"><span data-stu-id="0200d-231">Names are introduced into this declaration space through local variable declarations in the method body and through the parameter list of the method, which introduces names into the outermost block's declaration space.</span></span> <span data-ttu-id="0200d-232">Blocos não permitem o sombreamento de nomes por meio de aninhamento: depois que um nome declarado em um bloco, o nome pode não ser declarado novamente todos os blocos aninhados.</span><span class="sxs-lookup"><span data-stu-id="0200d-232">Blocks do not allow shadowing of names through nesting: once a name has been declared in a block, the name may not be redeclared in any nested blocks.</span></span>

<span data-ttu-id="0200d-233">Assim, no exemplo a seguir, o `F` e `G` métodos estão em erro porque o nome `i` é declarado em um bloco externo e não pode ser declarado novamente no bloco interno.</span><span class="sxs-lookup"><span data-stu-id="0200d-233">Thus, in the following example, the `F` and `G` methods are in error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="0200d-234">No entanto, o `H` e `I` métodos são válidos porque os dois `i`do são declarados em blocos separados não aninhadas.</span><span class="sxs-lookup"><span data-stu-id="0200d-234">However, the `H` and `I` methods are valid because the two `i`'s are declared in separate non-nested blocks.</span></span>

```vb
Class A
    Sub F()
        Dim i As Integer = 0
        If True Then
               Dim i As Integer = 1
        End If
    End Sub

    Sub G()
        If True Then
            Dim i As Integer = 0
        End If
        Dim i As Integer = 1
    End Sub 

    Sub H()
        If True Then
            Dim i As Integer = 0
        End If
        If True Then
            Dim i As Integer = 1
        End If
    End Sub

    Sub I() 
        For i As Integer = 0 To 9
            H()
        Next i

        For i As Integer = 0 To 9
            H()
        Next i
    End Sub 
End Class
```

<span data-ttu-id="0200d-235">Quando o método é uma função, uma variável local especial é declarada implicitamente em espaço de declaração do corpo do método com o mesmo nome que o método que representa o valor de retorno da função.</span><span class="sxs-lookup"><span data-stu-id="0200d-235">When the method is a function, a special local variable is implicitly declared in the method body's declaration space with the same name as the method representing the return value of the function.</span></span> <span data-ttu-id="0200d-236">A variável local tem semântica de resolução de nome especial quando usadas em expressões.</span><span class="sxs-lookup"><span data-stu-id="0200d-236">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="0200d-237">Se a variável local é usada em um contexto que espera uma expressão classificada como um grupo de método, como uma expressão de invocação, em seguida, o nome é resolvido para a função em vez da variável local.</span><span class="sxs-lookup"><span data-stu-id="0200d-237">If the local variable is used in a context that expects an expression classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="0200d-238">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-238">For example:</span></span>

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

<span data-ttu-id="0200d-239">O uso de parênteses pode causar situações ambíguas (como `F(1)`, onde `F` é uma função cujo tipo de retorno é uma matriz unidimensional); em todas as situações ambíguas, o nome é resolvido para a função em vez da variável local.</span><span class="sxs-lookup"><span data-stu-id="0200d-239">The use of parentheses can cause ambiguous situations (such as `F(1)`, where `F` is a function whose return type is a one-dimensional array); in all ambiguous situations, the name resolves to the function rather than the local variable.</span></span> <span data-ttu-id="0200d-240">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-240">For example:</span></span>

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

<span data-ttu-id="0200d-241">Quando o fluxo de controle deixa o corpo do método, o valor da variável local é passado de volta para a expressão de invocação.</span><span class="sxs-lookup"><span data-stu-id="0200d-241">When control flow leaves the method body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="0200d-242">Se o método for uma sub-rotina, não há nenhum tal variável local implícita e, simplesmente, o controle retorna para a expressão de invocação.</span><span class="sxs-lookup"><span data-stu-id="0200d-242">If the method is a subroutine, there is no such implicit local variable, and control simply returns to the invocation expression.</span></span>

## <a name="local-declaration-statements"></a><span data-ttu-id="0200d-243">Instruções de declaração de local</span><span class="sxs-lookup"><span data-stu-id="0200d-243">Local Declaration Statements</span></span>

<span data-ttu-id="0200d-244">Uma instrução de declaração de local declara uma nova variável local, uma constante local ou uma variável estática.</span><span class="sxs-lookup"><span data-stu-id="0200d-244">A local declaration statement declares a new local variable, local constant, or static variable.</span></span> <span data-ttu-id="0200d-245">*Variáveis locais* e *constantes locais* são equivalentes a variáveis de instância e constantes no escopo para o método e são declarados da mesma maneira.</span><span class="sxs-lookup"><span data-stu-id="0200d-245">*Local variables* and *local constants* are equivalent to instance variables and constants scoped to the method and are declared in the same way.</span></span> <span data-ttu-id="0200d-246">*Variáveis estáticas* são semelhantes às `Shared` variáveis e são declarados usando a `Static` modificador.</span><span class="sxs-lookup"><span data-stu-id="0200d-246">*Static variables* are similar to `Shared` variables and are declared using the `Static` modifier.</span></span>

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

<span data-ttu-id="0200d-247">Variáveis estáticas são locais que reter seu valor entre invocações do método.</span><span class="sxs-lookup"><span data-stu-id="0200d-247">Static variables are locals that retain their value across invocations of the method.</span></span> <span data-ttu-id="0200d-248">Variáveis estáticas declaradas dentro de métodos não compartilhados são por instância: cada instância do tipo que contém o método tem sua própria cópia da variável estática.</span><span class="sxs-lookup"><span data-stu-id="0200d-248">Static variables declared within non-shared methods are per instance: each instance of the type that contains the method has its own copy of the static variable.</span></span> <span data-ttu-id="0200d-249">Variáveis estáticas declaradas dentro de `Shared` métodos são por tipo, há apenas uma cópia da variável estática para todas as instâncias.</span><span class="sxs-lookup"><span data-stu-id="0200d-249">Static variables declared within `Shared` methods are per type; there is only one copy of the static variable for all instances.</span></span> <span data-ttu-id="0200d-250">Enquanto as variáveis locais são inicializadas para seu valor padrão de tipo após cada entrada no método, variáveis estáticas são inicializadas somente para seu valor padrão de tipo quando o tipo ou instância de tipo é inicializada.</span><span class="sxs-lookup"><span data-stu-id="0200d-250">While local variables are initialized to their type's default value upon each entry into the method, static variables are only initialized to their type's default value when the type or type instance is initialized.</span></span> <span data-ttu-id="0200d-251">Variáveis estáticas não podem ser declaradas em estruturas ou métodos genéricos.</span><span class="sxs-lookup"><span data-stu-id="0200d-251">Static variables may not be declared in structures or generic methods.</span></span>

<span data-ttu-id="0200d-252">Variáveis locais, as constantes locais e as variáveis estáticas sempre tem acessibilidade pública e não é podem especificar modificadores de acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="0200d-252">Local variables, local constants, and static variables always have public accessibility and may not specify accessibility modifiers.</span></span> <span data-ttu-id="0200d-253">Se nenhum tipo é especificado em uma instrução de declaração de local, as etapas a seguir determinam o tipo da declaração de local:</span><span class="sxs-lookup"><span data-stu-id="0200d-253">If no type is specified on a local declaration statement, then the following steps determine the type of the local declaration:</span></span>

1. <span data-ttu-id="0200d-254">Se a declaração tem um caractere de tipo, o tipo de caractere de tipo é o tipo da declaração de local.</span><span class="sxs-lookup"><span data-stu-id="0200d-254">If the declaration has a type character, the type of the type character is the type of the local declaration.</span></span>

2. <span data-ttu-id="0200d-255">Se a declaração local é uma constante local, ou se a declaração local é uma variável local com um inicializador e Inferência de tipo de variável local está sendo usada, o tipo da declaração de local é inferido do tipo do inicializador.</span><span class="sxs-lookup"><span data-stu-id="0200d-255">If the local declaration is a local constant, or if the local declaration is a local variable with an initializer and local variable type inference is being used, the type of the local declaration is inferred from the type of the initializer.</span></span> <span data-ttu-id="0200d-256">Se o inicializador refere-se a declaração de local, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-256">If the initializer refers to the local declaration, a compile-time error occurs.</span></span> <span data-ttu-id="0200d-257">(As constantes locais devem ter inicializadores.)</span><span class="sxs-lookup"><span data-stu-id="0200d-257">(Local constants are required to have initializers.)</span></span>

3. <span data-ttu-id="0200d-258">Se a semântica estrita não está sendo usada, o tipo de instrução de declaração de local é implicitamente `Object`.</span><span class="sxs-lookup"><span data-stu-id="0200d-258">If strict semantics are not being used, the type of the local declaration statement is implicitly `Object`.</span></span>

4. <span data-ttu-id="0200d-259">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-259">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="0200d-260">Se nenhum tipo é especificado em uma instrução de declaração de local que tem um tamanho da matriz ou um modificador de tipo de matriz, em seguida, o tipo da declaração de local é uma matriz com a classificação especificada e as etapas anteriores são usadas para determinar o tipo de elemento da matriz.</span><span class="sxs-lookup"><span data-stu-id="0200d-260">If no type is specified on a local declaration statement that has an array size or array type modifier, then the type of the local declaration is an array with the specified rank and the previous steps are used to determine the element type of the array.</span></span> <span data-ttu-id="0200d-261">Se a inferência de tipo de variável local é usada, o tipo do inicializador deve ser um tipo de matriz com a mesma forma de matriz (ou seja, modificadores de tipo de matriz) que a instrução de declaração de local.</span><span class="sxs-lookup"><span data-stu-id="0200d-261">If local variable type inference is used, the type of the initializer must be an array type with the same array shape (i.e. array type modifiers) as the local declaration statement.</span></span> <span data-ttu-id="0200d-262">Observe que é possível que o tipo do elemento deduzido ainda pode ser um tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="0200d-262">Note that it is possible that the inferred element type may still be an array type.</span></span> <span data-ttu-id="0200d-263">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-263">For example:</span></span>

```vb
Option Infer On

Module Test
    Sub Main()
        ' Error: initializer is not an array type
        Dim x() = 1

        ' Type is Integer()
        Dim y() = New Integer() {}

        ' Type is Integer()()
        Dim z() = New Integer()() {}

        ' Type is Integer()()()

        Dim a()() = New Integer()()() {}

        ' Error: initializer does not have same array shape
        Dim b()() = New Integer(,)() {}
    End Sub
End Module
```

<span data-ttu-id="0200d-264">Se nenhum tipo é especificado em uma instrução de declaração de local que tem um modificador de tipo anulável, o tipo da declaração de local é a versão que permite valor nula do tipo inferido ou o tipo inferido em si se ele digitar um valor anulável já.</span><span class="sxs-lookup"><span data-stu-id="0200d-264">If no type is specified on a local declaration statement that has a nullable type modifier, then the type of the local declaration is the nullable version of the inferred type or the inferred type itself if it a nullable value type already.</span></span>  <span data-ttu-id="0200d-265">Se o tipo inferido não for um tipo de valor pode ser anulável, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-265">If the inferred type is not a value type that can be made nullable, a compile-time error occurs.</span></span> <span data-ttu-id="0200d-266">Se um modificador de tipo anulável e um modificador de tipo de tamanho ou uma matriz de matriz são colocados em uma instrução de declaração local sem tipo, em seguida, o modificador de tipo que permite valor nulo é considerado a ser aplicado ao tipo de elemento da matriz e as etapas anteriores são usadas para determinar o eleme tipo do NT.</span><span class="sxs-lookup"><span data-stu-id="0200d-266">If both a nullable type modifier and an array size or array type modifier are placed on a local declaration statement with no type, then the nullable type modifier is considered to apply to the element type of the array and the previous steps are used to determine the element type.</span></span>

<span data-ttu-id="0200d-267">Inicializadores de variável em instruções de declaração de local são equivalentes às instruções de atribuição colocadas no local textual da declaração.</span><span class="sxs-lookup"><span data-stu-id="0200d-267">Variable initializers on local declaration statements are equivalent to assignment statements placed at the textual location of the declaration.</span></span> <span data-ttu-id="0200d-268">Portanto, se as ramificações de execução para a instrução de declaração de local, o inicializador de variável não é executado.</span><span class="sxs-lookup"><span data-stu-id="0200d-268">Thus, if execution branches over the local declaration statement, the variable initializer is not executed.</span></span> <span data-ttu-id="0200d-269">Se a instrução de declaração de local é executada mais de uma vez, o inicializador de variável é executado um número igual de vezes.</span><span class="sxs-lookup"><span data-stu-id="0200d-269">If the local declaration statement is executed more than once, the variable initializer is executed an equal number of times.</span></span> <span data-ttu-id="0200d-270">Variáveis estáticas apenas executar seu inicializador na primeira vez.</span><span class="sxs-lookup"><span data-stu-id="0200d-270">Static variables only execute their initializer the first time.</span></span> <span data-ttu-id="0200d-271">Se ocorrer uma exceção ao inicializar uma variável estática, a variável estática é considerada inicializado com o valor padrão de tipo da variável estática.</span><span class="sxs-lookup"><span data-stu-id="0200d-271">If an exception occurs while initializing a static variable, the static variable is considered initialized with the default value of the static variable's type.</span></span>

<span data-ttu-id="0200d-272">O exemplo a seguir mostra o uso de inicializadores:</span><span class="sxs-lookup"><span data-stu-id="0200d-272">The following example shows the use of initializers:</span></span>

```vb
Module Test
    Sub F()
        Static x As Integer = 5

        Console.WriteLine("Static variable x = " & x)
        x += 1
    End Sub

    Sub Main()
        Dim i As Integer

        For i = 1 to 3
            F()
        Next i

        i = 3
label:
        Dim y As Integer = 8

        If i > 0 Then
            Console.WriteLine("Local variable y = " & y)
            y -= 1
            i -= 1
            GoTo label
        End If
    End Sub
End Module
```

<span data-ttu-id="0200d-273">Esse programa imprime:</span><span class="sxs-lookup"><span data-stu-id="0200d-273">This program prints:</span></span>

```
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

<span data-ttu-id="0200d-274">Inicializadores em variáveis locais estáticas são thread-safe e protegidos contra exceções durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="0200d-274">Initializers on static locals are thread-safe and protected against exceptions during initialization.</span></span> <span data-ttu-id="0200d-275">Se uma exceção ocorrer durante um inicializador de local estático, o local estático terá seu valor padrão e não ser inicializado.</span><span class="sxs-lookup"><span data-stu-id="0200d-275">If an exception occurs during a static local initializer, the static local will have its default value and not be initialized.</span></span> <span data-ttu-id="0200d-276">Um inicializador estático de local</span><span class="sxs-lookup"><span data-stu-id="0200d-276">A static local initializer</span></span>

```vb
Module Test
    Sub F()
        Static x As Integer = 5
    End Sub
End Module
```

<span data-ttu-id="0200d-277">equivale a</span><span class="sxs-lookup"><span data-stu-id="0200d-277">is equivalent to</span></span>

```vb
Imports System.Threading
Imports Microsoft.VisualBasic.CompilerServices

Module Test
    Class InitFlag
        Public State As Short
    End Class

    Private xInitFlag As InitFlag = New InitFlag()

    Sub F()
        Dim x As Integer

        If xInitFlag.State <> 1 Then
            Monitor.Enter(xInitFlag)
            Try
                If xInitFlag.State = 0 Then
                    xInitFlag.State = 2
                    x = 5
                Else If xInitFlag.State = 2 Then
                    Throw New IncompleteInitialization()
                End If
            Finally
                xInitFlag.State = 1
                Monitor.Exit(xInitFlag)
            End Try
        End If
    End Sub
End Module
```

<span data-ttu-id="0200d-278">Variáveis locais, as constantes locais e as variáveis estáticas têm o escopo para a instrução de bloco em que elas são declaradas.</span><span class="sxs-lookup"><span data-stu-id="0200d-278">Local variables, local constants, and static variables are scoped to the statement block in which they are declared.</span></span> <span data-ttu-id="0200d-279">Variáveis estáticas são especiais em que seus nomes podem ser usados somente uma vez ao longo de todo o método.</span><span class="sxs-lookup"><span data-stu-id="0200d-279">Static variables are special in that their names may only be used once throughout the entire method.</span></span> <span data-ttu-id="0200d-280">Por exemplo, não é válido para especificar duas declarações de variável estáticas com o mesmo nome, mesmo se eles estiverem em blocos diferentes.</span><span class="sxs-lookup"><span data-stu-id="0200d-280">For example, it is not valid to specify two static variable declarations with the same name even if they are in different blocks.</span></span>


### <a name="implicit-local-declarations"></a><span data-ttu-id="0200d-281">Declarações locais implícitas</span><span class="sxs-lookup"><span data-stu-id="0200d-281">Implicit Local Declarations</span></span>

<span data-ttu-id="0200d-282">Além das instruções de declaração de local, as variáveis locais também podem ser declaradas implicitamente por meio do uso.</span><span class="sxs-lookup"><span data-stu-id="0200d-282">In addition to local declaration statements, local variables can also be declared implicitly through use.</span></span> <span data-ttu-id="0200d-283">Uma expressão de nome simples que usa um nome que não é resolvido para algo mais declara uma variável local com esse nome.</span><span class="sxs-lookup"><span data-stu-id="0200d-283">A simple name expression that uses a name that does not resolve to something else declares a local variable by that name.</span></span> <span data-ttu-id="0200d-284">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-284">For example:</span></span>

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 10
        y = 20
        Console.WriteLine(x + y)
    End Sub
End Module
```

<span data-ttu-id="0200d-285">Declaração local implícita ocorre apenas em contextos de expressão que podem aceitar uma expressão classificada como uma variável.</span><span class="sxs-lookup"><span data-stu-id="0200d-285">Implicit local declaration only occurs in expression contexts that can accept an expression classified as a variable.</span></span> <span data-ttu-id="0200d-286">A exceção a essa regra é que uma variável local não pode ser implicitamente declarada quando ele é o destino de uma expressão de invocação de função, expressão de indexação ou uma expressão de acesso de membro.</span><span class="sxs-lookup"><span data-stu-id="0200d-286">The exception to this rule is that a local variable may not be implicitly declared when it is the target of a function invocation expression, indexing expression, or a member access expression.</span></span>

<span data-ttu-id="0200d-287">Locais implícitas são tratados como se elas são declaradas no início do método que a contém.</span><span class="sxs-lookup"><span data-stu-id="0200d-287">Implicit locals are treated as if they are declared at the beginning of the containing method.</span></span> <span data-ttu-id="0200d-288">Assim, eles sempre têm escopo para o corpo do método inteira, mesmo se declarados dentro de uma expressão lambda.</span><span class="sxs-lookup"><span data-stu-id="0200d-288">Thus, they are always scoped to the entire method body, even if declared inside of a lambda expression.</span></span> <span data-ttu-id="0200d-289">Por exemplo, o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="0200d-289">For example, the following code:</span></span>

```vb
Option Explicit Off 

Module Test
    Sub Main()
        Dim x = Sub()
                    a = 10
                End Sub
        Dim y = Sub()
                    Console.WriteLine(a)
                End Sub

        x()
        y()
    End Sub
End Module
```

<span data-ttu-id="0200d-290">imprimirá o valor `10`.</span><span class="sxs-lookup"><span data-stu-id="0200d-290">will print the value `10`.</span></span> <span data-ttu-id="0200d-291">Locais implícitas são digitadas como `Object` se nenhum caractere de tipo foi anexado ao nome da variável; caso contrário, o tipo da variável é o tipo de caractere de tipo.</span><span class="sxs-lookup"><span data-stu-id="0200d-291">Implicit locals are typed as `Object` if no type character was attached to the variable name; otherwise the type of the variable is the type of the type character.</span></span> <span data-ttu-id="0200d-292">Inferência de tipo de variável local não é usada para locais implícitas.</span><span class="sxs-lookup"><span data-stu-id="0200d-292">Local variable type inference is not used for implicit locals.</span></span>

<span data-ttu-id="0200d-293">Se a declaração explícita de local é especificada pelo ambiente de compilação ou por `Option Explicit`, todas as variáveis locais devem ser declaradas explicitamente e declaração de variável implícita não é permitida.</span><span class="sxs-lookup"><span data-stu-id="0200d-293">If explicit local declaration is specified by the compilation environment or by `Option Explicit`, all local variables must be explicitly declared and implicit variable declaration is disallowed.</span></span>

## <a name="with-statement"></a><span data-ttu-id="0200d-294">Com instrução</span><span class="sxs-lookup"><span data-stu-id="0200d-294">With Statement</span></span>

<span data-ttu-id="0200d-295">Um `With` instrução permite várias referências aos membros de uma expressão sem especificar a expressão várias vezes.</span><span class="sxs-lookup"><span data-stu-id="0200d-295">A `With` statement allows multiple references to an expression's members without specifying the expression multiple times.</span></span>

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

<span data-ttu-id="0200d-296">A expressão deve ser classificada como um valor e é avaliada uma vez, após a entrada em um bloco.</span><span class="sxs-lookup"><span data-stu-id="0200d-296">The expression must be classified as a value and is evaluated once, upon entry into the block.</span></span> <span data-ttu-id="0200d-297">Dentro de `With` bloco de instrução, uma expressão de acesso de membro ou expressão de acesso de dicionário, começando com um ponto final ou um ponto de exclamação é avaliada como se o `With` expressão antecedem.</span><span class="sxs-lookup"><span data-stu-id="0200d-297">Within the `With` statement block, a member access expression or dictionary access expression starting with a period or an exclamation point is evaluated as if the `With` expression preceded it.</span></span> <span data-ttu-id="0200d-298">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-298">For example:</span></span>

```vb
Structure Test
    Public x As Integer

    Function F() As Integer
        Return 10
    End Sub
End Structure

Module TestModule
    Sub Main()
        Dim y As Test

        With y
            .x = 10
            Console.WriteLine(.x)
            .x = .F()
        End With
    End Sub
End Module
```

<span data-ttu-id="0200d-299">Não é válido se ramificar para um `With` bloco de instruções de fora do bloco.</span><span class="sxs-lookup"><span data-stu-id="0200d-299">It is invalid to branch into a `With` statement block from outside of the block.</span></span>


## <a name="synclock-statement"></a><span data-ttu-id="0200d-300">Instrução SyncLock</span><span class="sxs-lookup"><span data-stu-id="0200d-300">SyncLock Statement</span></span>

<span data-ttu-id="0200d-301">Um `SyncLock` instrução permite que as instruções a serem sincronizados em uma expressão, o que garante que vários threads de execução não executem as mesmas instruções ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="0200d-301">A `SyncLock` statement allows statements to be synchronized on an expression, which ensures that multiple threads of execution do not execute the same statements at the same time.</span></span>

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

<span data-ttu-id="0200d-302">A expressão deve ser classificada como um valor e é avaliada uma vez, após a entrada para o bloco.</span><span class="sxs-lookup"><span data-stu-id="0200d-302">The expression must be classified as a value and is evaluated once, upon entry to the block.</span></span> <span data-ttu-id="0200d-303">Ao inserir o `SyncLock` bloco, o `Shared` método `System.Threading.Monitor.Enter` é chamado na expressão especificada, que bloqueia até que o thread de execução tem um bloqueio exclusivo no objeto retornado pela expressão.</span><span class="sxs-lookup"><span data-stu-id="0200d-303">When entering the `SyncLock` block, the `Shared` method `System.Threading.Monitor.Enter` is called on the specified expression, which blocks until the thread of execution has an exclusive lock on the object returned by the expression.</span></span> <span data-ttu-id="0200d-304">O tipo da expressão em um `SyncLock` instrução deve ser um tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="0200d-304">The type of the expression in a `SyncLock` statement must be a reference type.</span></span> <span data-ttu-id="0200d-305">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-305">For example:</span></span>

```vb
Class Test
    Private count As Integer = 0

    Public Function Add() As Integer
        SyncLock Me
            count += 1
            Add = count
        End SyncLock
    End Function

    Public Function Subtract() As Integer
        SyncLock Me
            count -= 1
            Subtract = count
        End SyncLock
    End Function
End Class
```

<span data-ttu-id="0200d-306">O exemplo acima sincroniza na instância específica da classe `Test` para garantir que não mais do que um thread de execução pode adicionar ou subtrair a partir da variável de contagem de uma vez para uma determinada instância.</span><span class="sxs-lookup"><span data-stu-id="0200d-306">The example above synchronizes on the specific instance of the class `Test` to ensure that no more than one thread of execution can add or subtract from the count variable at a time for a particular instance.</span></span>

<span data-ttu-id="0200d-307">O `SyncLock` bloco está contido implicitamente por um `Try` instrução cuja `Finally` bloquear chamadas a `Shared` método `System.Threading.Monitor.Exit` na expressão.</span><span class="sxs-lookup"><span data-stu-id="0200d-307">The `SyncLock` block is implicitly contained by a `Try` statement whose `Finally` block calls the `Shared` method `System.Threading.Monitor.Exit` on the expression.</span></span> <span data-ttu-id="0200d-308">Isso garante que o bloqueio seja liberado, mesmo quando uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="0200d-308">This ensures the lock is freed even when an exception is thrown.</span></span> <span data-ttu-id="0200d-309">Como resultado, não é válido ramificar em uma `SyncLock` bloquear de fora do bloco e uma `SyncLock` bloco é tratado como uma única instrução para os fins `Resume` e `Resume Next`.</span><span class="sxs-lookup"><span data-stu-id="0200d-309">As a result, it is invalid to branch into a `SyncLock` block from outside of the block, and a `SyncLock` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="0200d-310">O exemplo acima é equivalente ao código a seguir:</span><span class="sxs-lookup"><span data-stu-id="0200d-310">The above example is equivalent to the following code:</span></span>

```vb
Class Test
    Private count As Integer = 0

    Public Function Add() As Integer
        Try
            System.Threading.Monitor.Enter(Me)

            count += 1
            Add = count
        Finally
            System.Threading.Monitor.Exit(Me)
        End Try
    End Function

    Public Function Subtract() As Integer
        Try
            System.Threading.Monitor.Enter(Me)

            count -= 1
            Subtract = count
        Finally
            System.Threading.Monitor.Exit(Me)
        End Try
    End Function
End Class
```


## <a name="event-statements"></a><span data-ttu-id="0200d-311">Instruções Event</span><span class="sxs-lookup"><span data-stu-id="0200d-311">Event Statements</span></span>

<span data-ttu-id="0200d-312">O `RaiseEvent`, `AddHandler`, e `RemoveHandler` instruções geram eventos e manipular eventos dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="0200d-312">The `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements raise events and handle events dynamically.</span></span>

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a><span data-ttu-id="0200d-313">Instrução RaiseEvent</span><span class="sxs-lookup"><span data-stu-id="0200d-313">RaiseEvent Statement</span></span>

<span data-ttu-id="0200d-314">Um `RaiseEvent` instrução notifica os manipuladores de eventos que um determinado evento ocorreu.</span><span class="sxs-lookup"><span data-stu-id="0200d-314">A `RaiseEvent` statement notifies event handlers that a particular event has occurred.</span></span>

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

<span data-ttu-id="0200d-315">A expressão de nome simples em uma `RaiseEvent` instrução é interpretada como uma pesquisa de membro em `Me`.</span><span class="sxs-lookup"><span data-stu-id="0200d-315">The simple name expression in a `RaiseEvent` statement is interpreted as a member lookup on `Me`.</span></span> <span data-ttu-id="0200d-316">Portanto, `RaiseEvent x` é interpretado como se fosse `RaiseEvent Me.x`.</span><span class="sxs-lookup"><span data-stu-id="0200d-316">Thus, `RaiseEvent x` is interpreted as if it were `RaiseEvent Me.x`.</span></span> <span data-ttu-id="0200d-317">O resultado da expressão deve ser classificado como um evento de acesso para um evento definido na classe em si; eventos definidos em tipos de base não podem ser usados em um `RaiseEvent` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-317">The result of the expression must be classified as an event access for an event defined in the class itself; events defined on base types cannot be used in a `RaiseEvent` statement.</span></span>

<span data-ttu-id="0200d-318">O `RaiseEvent` instrução é processada como uma chamada para o `Invoke` método do delegado do evento, usando os parâmetros fornecidos, se houver.</span><span class="sxs-lookup"><span data-stu-id="0200d-318">The `RaiseEvent` statement is processed as a call to the `Invoke` method of the event's delegate, using the supplied parameters, if any.</span></span> <span data-ttu-id="0200d-319">Se o valor do delegado é `Nothing`, nenhuma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="0200d-319">If the delegate's value is `Nothing`, no exception is thrown.</span></span> <span data-ttu-id="0200d-320">Se não houver nenhum argumento, os parênteses podem ser omitidos.</span><span class="sxs-lookup"><span data-stu-id="0200d-320">If there are no arguments, the parentheses may be omitted.</span></span> <span data-ttu-id="0200d-321">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-321">For example:</span></span>

```vb
Class Raiser
    Public Event E1(Count As Integer)

    Public Sub Raise()
        Static RaiseCount As Integer = 0

        RaiseCount += 1
        RaiseEvent E1(RaiseCount)
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler(Count As Integer) Handles x.E1
        Console.WriteLine("Raise #" & Count)
    End Sub

    Public Sub Main()
        x = New Raiser
        x.Raise()        ' Prints "Raise #1".
        x.Raise()        ' Prints "Raise #2".
        x.Raise()        ' Prints "Raise #3".
    End Sub
End Module
```

<span data-ttu-id="0200d-322">A classe `Raiser` acima é equivalente a:</span><span class="sxs-lookup"><span data-stu-id="0200d-322">The class `Raiser` above is equivalent to:</span></span>

```vb
Class Raiser
    Public Event E1(Count As Integer)

    Public Sub Raise()
        Static RaiseCount As Integer = 0
        Dim TemporaryDelegate As E1EventHandler

        RaiseCount += 1

        ' Use a temporary to avoid a race condition.
        TemporaryDelegate = E1Event
        If Not TemporaryDelegate Is Nothing Then
            TemporaryDelegate.Invoke(RaiseCount)
        End If
    End Sub
End Class
```


### <a name="addhandler-and-removehandler-statements"></a><span data-ttu-id="0200d-323">AddHandler e RemoveHandler instruções</span><span class="sxs-lookup"><span data-stu-id="0200d-323">AddHandler and RemoveHandler Statements</span></span>

<span data-ttu-id="0200d-324">Embora a maioria dos manipuladores de eventos são conectados automaticamente por meio de `WithEvents` variáveis, talvez seja necessário adicionar e remover manipuladores de eventos em tempo de execução dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="0200d-324">Although most event handlers are automatically hooked up through `WithEvents` variables, it may be necessary to dynamically add and remove event handlers at run time.</span></span> <span data-ttu-id="0200d-325">`AddHandler` e `RemoveHandler` instruções fazer isso.</span><span class="sxs-lookup"><span data-stu-id="0200d-325">`AddHandler` and `RemoveHandler` statements do this.</span></span>

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

<span data-ttu-id="0200d-326">Cada instrução leva dois argumentos: o primeiro argumento deve ser uma expressão que é classificada como um acesso de evento e o segundo argumento deve ser uma expressão que é classificada como um valor.</span><span class="sxs-lookup"><span data-stu-id="0200d-326">Each statement takes two arguments: the first argument must be an expression that is classified as an event access and the second argument must be an expression that is classified as a value.</span></span> <span data-ttu-id="0200d-327">Tipo do segundo argumento deve ser do tipo de delegado associado com o acesso ao evento.</span><span class="sxs-lookup"><span data-stu-id="0200d-327">The second argument's type must be the delegate type associated with the event access.</span></span> <span data-ttu-id="0200d-328">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-328">For example:</span></span>

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

<span data-ttu-id="0200d-329">Dada a um evento `E,` a instrução chama o relevantes `add_E` ou `remove_E` método na instância para adicionar ou remover o delegado como um manipulador para o evento.</span><span class="sxs-lookup"><span data-stu-id="0200d-329">Given an event `E,` the statement calls the relevant `add_E` or `remove_E` method on the instance to add or remove the delegate as a handler for the event.</span></span> <span data-ttu-id="0200d-330">Assim, o código acima é equivalente a:</span><span class="sxs-lookup"><span data-stu-id="0200d-330">Thus, the above code is equivalent to:</span></span>

```vb
Public Class Form1
    Public Sub New()
        Button1.add_Click(AddressOf Button1_Click)
    End Sub 

    Private Button1 As Button = New Button()

    Sub Button1_Click(sender As Object, e As EventArgs)
        Console.WriteLine("Button1 was clicked!")
    End Sub

    Public Sub Disconnect()
        Button1.remove_Click(AddressOf Button1_Click)
    End Sub
End Class
```


## <a name="assignment-statements"></a><span data-ttu-id="0200d-331">Instruções de atribuição</span><span class="sxs-lookup"><span data-stu-id="0200d-331">Assignment Statements</span></span>

<span data-ttu-id="0200d-332">Uma instrução de atribuição atribui o valor de uma expressão a uma variável.</span><span class="sxs-lookup"><span data-stu-id="0200d-332">An assignment statement assigns the value of an expression to a variable.</span></span> <span data-ttu-id="0200d-333">Há vários tipos de atribuição.</span><span class="sxs-lookup"><span data-stu-id="0200d-333">There are several types of assignment.</span></span>

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a><span data-ttu-id="0200d-334">Instruções de atribuição regular</span><span class="sxs-lookup"><span data-stu-id="0200d-334">Regular Assignment Statements</span></span>

<span data-ttu-id="0200d-335">Uma instrução de atribuição simples armazena o resultado de uma expressão em uma variável.</span><span class="sxs-lookup"><span data-stu-id="0200d-335">A simple assignment statement stores the result of an expression in a variable.</span></span>

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="0200d-336">A expressão no lado esquerdo do operador de atribuição deve ser classificada como uma variável ou um acesso de propriedade, enquanto a expressão no lado direito do operador de atribuição deve ser classificada como um valor.</span><span class="sxs-lookup"><span data-stu-id="0200d-336">The expression on the left side of the assignment operator must be classified as a variable or a property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="0200d-337">O tipo da expressão deve ser implicitamente conversível para o tipo de acesso a variável ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="0200d-337">The type of the expression must be implicitly convertible to the type of the variable or property access.</span></span>

<span data-ttu-id="0200d-338">Se a variável que está sendo atribuída em é um elemento de matriz do tipo de referência, uma verificação de tempo de execução será executada para garantir que a expressão é compatível com o tipo de elemento da matriz.</span><span class="sxs-lookup"><span data-stu-id="0200d-338">If the variable being assigned into is an array element of a reference type, a run-time check will be performed to ensure that the expression is compatible with the array-element type.</span></span> <span data-ttu-id="0200d-339">No exemplo a seguir, a última atribuição faz com que um `System.ArrayTypeMismatchException` ser lançada, porque uma instância do `ArrayList` não podem ser armazenados em um elemento de um `String` matriz.</span><span class="sxs-lookup"><span data-stu-id="0200d-339">In the following example, the last assignment causes a `System.ArrayTypeMismatchException` to be thrown, because an instance of `ArrayList` cannot be stored in an element of a `String` array.</span></span>

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

<span data-ttu-id="0200d-340">Se a expressão no lado esquerdo do operador de atribuição é classificada como uma variável, a instrução de atribuição armazena o valor na variável.</span><span class="sxs-lookup"><span data-stu-id="0200d-340">If the expression on the left side of the assignment operator is classified as a variable, then the assignment statement stores the value in the variable.</span></span> <span data-ttu-id="0200d-341">Se a expressão é classificada como um acesso de propriedade e, em seguida, a instrução de atribuição transforma o acesso de propriedade em uma invocação do `Set` acessador da propriedade com o valor substituído para o parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="0200d-341">If the expression is classified as a property access, then the assignment statement turns the property access into an invocation of the `Set` accessor of the property with the value substituted for the value parameter.</span></span> <span data-ttu-id="0200d-342">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-342">For example:</span></span>

```vb
Module Test
    Private PValue As Integer

    Public Property P As Integer
        Get
            Return PValue
        End Get

        Set (Value As Integer)
            PValue = Value
        End Set
    End Property

    Sub Main()
        ' The following two lines are equivalent.
        P = 10
        set_P(10)
    End Sub
End Module
```

<span data-ttu-id="0200d-343">Se o destino do acesso de variável ou propriedade é digitado como um tipo de valor, mas não classificado como uma variável, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-343">If the target of the variable or property access is typed as a value type but not classified as a variable, a compile-time error occurs.</span></span> <span data-ttu-id="0200d-344">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-344">For example:</span></span>

```vb
Structure S
    Public F As Integer
End Structure

Class C
    Private PValue As S

    Public Property P As S
        Get
            Return PValue
        End Get

        Set (Value As S)
            PValue = Value
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim ct As C = New C()
        Dim rt As Object = new C()

        ' Compile-time error: ct.P not classified as variable.
        ct.P.F = 10

        ' Run-time exception.
        rt.P.F = 10
    End Sub
End Module
```

<span data-ttu-id="0200d-345">Observe que a semântica da atribuição depende do tipo da variável ou propriedade à qual ele está sendo atribuído.</span><span class="sxs-lookup"><span data-stu-id="0200d-345">Note that the semantics of the assignment depend on the type of the variable or property to which it is being assigned.</span></span> <span data-ttu-id="0200d-346">Se a variável à qual ele está sendo atribuído é um tipo de valor, a atribuição copia o valor da expressão para a variável.</span><span class="sxs-lookup"><span data-stu-id="0200d-346">If the variable to which it is being assigned is a value type, the assignment copies the value of the expression into the variable.</span></span> <span data-ttu-id="0200d-347">Se a variável à qual ele está sendo atribuído é um tipo de referência, a atribuição copia a referência, não o próprio valor, a variável.</span><span class="sxs-lookup"><span data-stu-id="0200d-347">If the variable to which it is being assigned is a reference type, the assignment copies the reference, not the value itself, into the variable.</span></span> <span data-ttu-id="0200d-348">Se o tipo da variável for `Object`, a semântica de atribuição é determinada pelo tipo do valor seja um tipo de valor ou um tipo de referência em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="0200d-348">If the type of the variable is `Object`, the assignment semantics are determined by whether the value's type is a value type or a reference type at run time.</span></span>


<span data-ttu-id="0200d-349">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0200d-349">__Note.__</span></span> <span data-ttu-id="0200d-350">Para tipos intrínsecos, como `Integer` e `Date`, semântica de atribuição de referência e valor é os mesmos, porque os tipos são imutáveis.</span><span class="sxs-lookup"><span data-stu-id="0200d-350">For intrinsic types such as `Integer` and `Date`, reference and value assignment semantics are the same because the types are immutable.</span></span> <span data-ttu-id="0200d-351">Como resultado, o idioma é livre para usar a atribuição de referência no box tipos intrínsecos como uma otimização.</span><span class="sxs-lookup"><span data-stu-id="0200d-351">As a result, the language is free to use reference assignment on boxed intrinsic types as an optimization.</span></span> <span data-ttu-id="0200d-352">De uma perspectiva de valor, o resultado é o mesmo.</span><span class="sxs-lookup"><span data-stu-id="0200d-352">From a value perspective, the result is the same.</span></span>

<span data-ttu-id="0200d-353">Porque o caractere de igual (`=`) é usado para atribuição e de igualdade, haja uma ambiguidade entre uma atribuição simples e uma instrução de invocação em situações como `x = y.ToString()`.</span><span class="sxs-lookup"><span data-stu-id="0200d-353">Because the equals character (`=`) is used both for assignment and for equality, there is an ambiguity between a simple assignment and an invocation statement in situations such as `x = y.ToString()`.</span></span> <span data-ttu-id="0200d-354">Nesses casos, a instrução de atribuição prevalece sobre o operador de igualdade.</span><span class="sxs-lookup"><span data-stu-id="0200d-354">In all such cases, the assignment statement takes precedence over the equality operator.</span></span> <span data-ttu-id="0200d-355">Isso significa que o exemplo de expressão é interpretado como `x = (y.ToString())` em vez de `(x = y).ToString()`.</span><span class="sxs-lookup"><span data-stu-id="0200d-355">This means that the example expression is interpreted as `x = (y.ToString())` rather than `(x = y).ToString()`.</span></span>


### <a name="compound-assignment-statements"></a><span data-ttu-id="0200d-356">Instruções de atribuição compostas</span><span class="sxs-lookup"><span data-stu-id="0200d-356">Compound Assignment Statements</span></span>

<span data-ttu-id="0200d-357">Um *instrução de atribuição composta* assume a forma `V op= E` (onde `op` é um operador binário válido).</span><span class="sxs-lookup"><span data-stu-id="0200d-357">A *compound assignment statement* takes the form `V op= E` (where `op` is a valid binary operator).</span></span>

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

<span data-ttu-id="0200d-358">A expressão no lado esquerdo do operador de atribuição deve ser classificada como um acesso de variável ou propriedade, enquanto a expressão no lado direito do operador de atribuição deve ser classificada como um valor.</span><span class="sxs-lookup"><span data-stu-id="0200d-358">The expression on the left side of the assignment operator must be classified as a variable or property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="0200d-359">A instrução de atribuição composta é equivalente à instrução `V = V op E` com a diferença de que a variável no lado esquerdo do operador de atribuição composta só é avaliada uma vez.</span><span class="sxs-lookup"><span data-stu-id="0200d-359">The compound assignment statement is equivalent to the statement `V = V op E` with the difference that the variable on the left side of the compound assignment operator is only evaluated once.</span></span> <span data-ttu-id="0200d-360">O exemplo a seguir demonstra essa diferença:</span><span class="sxs-lookup"><span data-stu-id="0200d-360">The following example demonstrates this difference:</span></span>

```vb
Module Test
    Function GetIndex() As Integer
        Console.WriteLine("Getting index")
        Return 1
    End Function

    Sub Main()
        Dim a(2) As Integer

        Console.WriteLine("Simple assignment")
        a(GetIndex()) = a(GetIndex()) + 1

        Console.WriteLine("Compound assignment")
        a(GetIndex()) += 1
    End Sub
End Module
```

<span data-ttu-id="0200d-361">A expressão `a(GetIndex())` é avaliada duas vezes para atribuição simples, mas somente depois de atribuição composta, portanto, o código imprime:</span><span class="sxs-lookup"><span data-stu-id="0200d-361">The expression `a(GetIndex())` is evaluated twice for simple assignment but only once for compound assignment, so the code prints:</span></span>

```
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a><span data-ttu-id="0200d-362">Instrução MID de atribuição</span><span class="sxs-lookup"><span data-stu-id="0200d-362">Mid Assignment Statement</span></span>

<span data-ttu-id="0200d-363">Um `Mid` instrução de atribuição atribui uma cadeia de caracteres em outra cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="0200d-363">A `Mid` assignment statement assigns a string into another string.</span></span> <span data-ttu-id="0200d-364">O lado esquerdo da atribuição tem a mesma sintaxe que uma chamada para a função `Microsoft.VisualBasic.Strings.Mid`.</span><span class="sxs-lookup"><span data-stu-id="0200d-364">The left side of the assignment has the same syntax as a call to the function `Microsoft.VisualBasic.Strings.Mid`.</span></span>

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="0200d-365">O primeiro argumento é o destino da atribuição e deve ser classificado como uma variável ou um acesso de propriedade cujo tipo é implicitamente conversível para e de `String`.</span><span class="sxs-lookup"><span data-stu-id="0200d-365">The first argument is the target of the assignment and must be classified as a variable or a property access whose type is implicitly convertible to and from `String`.</span></span> <span data-ttu-id="0200d-366">O segundo parâmetro é a posição de início de base 1 que corresponde em que a atribuição deve começar na cadeia de caracteres de destino e deve ser classificada como um valor cujo tipo deve ser implicitamente conversível para `Integer`.</span><span class="sxs-lookup"><span data-stu-id="0200d-366">The second parameter is the 1-based start position that corresponds to where the assignment should begin in the target string and must be classified as a value whose type must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="0200d-367">O terceiro parâmetro opcional é o número de caracteres à direita o valor a atribuir para a cadeia de caracteres de destino e deve ser classificada como um valor cujo tipo é implicitamente conversível para `Integer`.</span><span class="sxs-lookup"><span data-stu-id="0200d-367">The optional third parameter is the number of characters from the right-side value to assign into the target string and must be classified as a value whose type is implicitly convertible to `Integer`.</span></span> <span data-ttu-id="0200d-368">O lado direito é a cadeia de caracteres de origem e deve ser classificado como um valor cujo tipo é implicitamente conversível para `String`.</span><span class="sxs-lookup"><span data-stu-id="0200d-368">The right side is the source string and must be classified as a value whose type is implicitly convertible to `String`.</span></span> <span data-ttu-id="0200d-369">À direita será truncada para o parâmetro de comprimento, se especificado e substitui os caracteres na cadeia de caracteres esquerda, começando na posição inicial.</span><span class="sxs-lookup"><span data-stu-id="0200d-369">The right side is truncated to the length parameter, if specified, and replaces the characters in the left-side string, starting at the start position.</span></span> <span data-ttu-id="0200d-370">Se a cadeia de caracteres do lado direito contiver menos caracteres que o terceiro parâmetro, somente os caracteres da cadeia de caracteres do lado direito serão copiados.</span><span class="sxs-lookup"><span data-stu-id="0200d-370">If the right side string contained fewer characters than the third parameter, only the characters from the right side string will be copied.</span></span>

<span data-ttu-id="0200d-371">O exemplo a seguir exibe `ab123fg`:</span><span class="sxs-lookup"><span data-stu-id="0200d-371">The following example displays `ab123fg`:</span></span>

```vb
Module Test
    Sub Main()
        Dim s1 As String = "abcdefg"
        Dim s2 As String = "1234567"

        Mid$(s1, 3, 3) = s2
        Console.WriteLine(s1)
    End Sub
End Module
```

<span data-ttu-id="0200d-372">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0200d-372">__Note.__</span></span> <span data-ttu-id="0200d-373">`Mid` não é uma palavra reservada.</span><span class="sxs-lookup"><span data-stu-id="0200d-373">`Mid` is not a reserved word.</span></span>


## <a name="invocation-statements"></a><span data-ttu-id="0200d-374">Instruções de invocação</span><span class="sxs-lookup"><span data-stu-id="0200d-374">Invocation Statements</span></span>

<span data-ttu-id="0200d-375">Uma instrução de invocação invoca um método precedido pela palavra-chave a opcional `Call`.</span><span class="sxs-lookup"><span data-stu-id="0200d-375">An invocation statement invokes a method preceded by the optional keyword `Call`.</span></span> <span data-ttu-id="0200d-376">A instrução de invocação é processada da mesma forma que a expressão de invocação de função, com algumas diferenças, indicadas abaixo.</span><span class="sxs-lookup"><span data-stu-id="0200d-376">The invocation statement is processed in the same way as the function invocation expression, with some differences noted below.</span></span> <span data-ttu-id="0200d-377">A expressão de invocação deve ser classificada como um valor ou nulo.</span><span class="sxs-lookup"><span data-stu-id="0200d-377">The invocation expression must be classified as a value or void.</span></span> <span data-ttu-id="0200d-378">Qualquer valor resultante da avaliação da expressão de invocação é descartado.</span><span class="sxs-lookup"><span data-stu-id="0200d-378">Any value resulting from the evaluation of the invocation expression is discarded.</span></span>

<span data-ttu-id="0200d-379">Se o `Call` palavra-chave for omitida e, em seguida, a expressão de invocação deve começar com um identificador ou palavra-chave ou com `.` dentro de um `With` bloco.</span><span class="sxs-lookup"><span data-stu-id="0200d-379">If the `Call` keyword is omitted, then the invocation expression must start with an identifier or keyword, or with `.` inside a `With` block.</span></span> <span data-ttu-id="0200d-380">Assim, por exemplo, "`Call 1.ToString()`" é uma instrução válida, mas "`1.ToString()`" não é.</span><span class="sxs-lookup"><span data-stu-id="0200d-380">Thus, for instance, "`Call 1.ToString()`" is a valid statement but "`1.ToString()`" is not.</span></span> <span data-ttu-id="0200d-381">(Observe que em um contexto de expressão, expressões de invocação também precisam não começar com um identificador.</span><span class="sxs-lookup"><span data-stu-id="0200d-381">(Note that in an expression context, invocation expressions also need not start with an identifier.</span></span> <span data-ttu-id="0200d-382">Por exemplo, "`Dim x = 1.ToString()`" é uma instrução válida).</span><span class="sxs-lookup"><span data-stu-id="0200d-382">For example, "`Dim x = 1.ToString()`" is a valid statement).</span></span>

<span data-ttu-id="0200d-383">Há outra diferença entre as instruções de invocação e expressões de invocação: se uma instrução de invocação inclui uma lista de argumentos, isso sempre será interpretado como a lista de argumentos da invocação.</span><span class="sxs-lookup"><span data-stu-id="0200d-383">There is another difference between the invocation statements and invocation expressions: if an invocation statement includes an argument list, then this is always taken as the argument list of the invocation.</span></span> <span data-ttu-id="0200d-384">O exemplo a seguir ilustra a diferença:</span><span class="sxs-lookup"><span data-stu-id="0200d-384">The following example illustrates the difference:</span></span>

```vb
Module Test
    Sub Main()
        Call {Function() 15}(0)
        ' error: (0) is taken as argument list, but array is not invokable

        Call ({Function() 15}(0))
        ' valid, since the invocation statement has no argument list

        Dim x = {Function() 15}(0)
        ' valid as an expression, since (0) is taken as an array-indexing

        Call f("a")
        ' error: ("a") is taken as argument list to the invocation of f

        Call f()("a")
        ' valid, since () is the argument list for the invocation of f

        Dim y = f("a")
        ' valid as an expression, since f("a") is interpreted as f()("a")
    End Sub

    Sub f() As Func(Of String,String)
        Return Function(x) x
    End Sub
End Module
```

```antlr
InvocationStatement
    : 'Call'? InvocationExpression StatementTerminator
    ;
```

## <a name="conditional-statements"></a><span data-ttu-id="0200d-385">Instruções condicionais</span><span class="sxs-lookup"><span data-stu-id="0200d-385">Conditional Statements</span></span>

<span data-ttu-id="0200d-386">Instruções condicionais permitem execução condicional de instruções com base em expressões avaliadas em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="0200d-386">Conditional statements allow conditional execution of statements based on expressions evaluated at run time.</span></span>

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a><span data-ttu-id="0200d-387">If... Then... Instruções Else</span><span class="sxs-lookup"><span data-stu-id="0200d-387">If...Then...Else Statements</span></span>

<span data-ttu-id="0200d-388">Um `If...Then...Else` instrução é a instrução condicional básica.</span><span class="sxs-lookup"><span data-stu-id="0200d-388">An `If...Then...Else` statement is the basic conditional statement.</span></span>

```antlr
IfStatement
    : BlockIfStatement
    | LineIfThenStatement
    ;

BlockIfStatement
    : 'If' BooleanExpression 'Then'? StatementTerminator
      Block?
      ElseIfStatement*
      ElseStatement?
      'End' 'If' StatementTerminator
    ;

ElseIfStatement
    : ElseIf BooleanExpression 'Then'? StatementTerminator
      Block?
    ;

ElseStatement
    : 'Else' StatementTerminator
      Block?
    ;

LineIfThenStatement
    : 'If' BooleanExpression 'Then' Statements ( 'Else' Statements )? StatementTerminator
    ;
    
ElseIf      
    : 'ElseIf'      
    | 'Else' 'If'   
   ;
```

<span data-ttu-id="0200d-389">Cada expressão em uma `If...Then...Else` instrução deve ser uma expressão booleana, de acordo com a seção [expressões Boolianas](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="0200d-389">Each expression in an `If...Then...Else` statement must be a Boolean expression, as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="0200d-390">(Observação: isso não exige que a expressão para ter um tipo booliano).</span><span class="sxs-lookup"><span data-stu-id="0200d-390">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="0200d-391">Se a expressão na `If` instrução for true, as instruções estão entre o `If` bloco são executados.</span><span class="sxs-lookup"><span data-stu-id="0200d-391">If the expression in the `If` statement is true, the statements enclosed by the `If` block are executed.</span></span> <span data-ttu-id="0200d-392">Se a expressão for false, cada um do `ElseIf` expressões será avaliada.</span><span class="sxs-lookup"><span data-stu-id="0200d-392">If the expression is false, each of the `ElseIf` expressions is evaluated.</span></span> <span data-ttu-id="0200d-393">Se um do `ElseIf` expressões for avaliada como true, o bloco correspondente é executado.</span><span class="sxs-lookup"><span data-stu-id="0200d-393">If one of the `ElseIf` expressions evaluates to true, the corresponding block is executed.</span></span> <span data-ttu-id="0200d-394">Se nenhuma expressão for avaliada como true e houver um `Else` bloco, o `Else` bloco é executado.</span><span class="sxs-lookup"><span data-stu-id="0200d-394">If no expression evaluates to true and there is an `Else` block, the `Else` block is executed.</span></span> <span data-ttu-id="0200d-395">Depois que um bloco da execução, a execução passa para o final do `If...Then...Else` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-395">Once a block finishes executing, execution passes to the end of the `If...Then...Else` statement.</span></span>

<span data-ttu-id="0200d-396">A versão de linha do `If` instrução tem um único conjunto de instruções a serem executadas se a `If` expressão é `True` e um conjunto opcional de instruções a serem executadas se a expressão for `False`.</span><span class="sxs-lookup"><span data-stu-id="0200d-396">The line version of the `If` statement has a single set of statements to be executed if the `If` expression is `True` and an optional set of statements to be executed if the expression is `False`.</span></span> <span data-ttu-id="0200d-397">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-397">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim a As Integer = 10
        Dim b As Integer = 20

        ' Block If statement.
        If a < b Then
            a = b
        Else
            b = a
        End If

        ' Line If statement
        If a < b Then a = b Else b = a
    End Sub
End Module
```

<span data-ttu-id="0200d-398">A versão de linha da se instrução associa menor firmemente que ":" e seu `Else` associa a precedente mais próximo lexicalmente `If` que é permitido pela sintaxe.</span><span class="sxs-lookup"><span data-stu-id="0200d-398">The line version of the If statement binds less tightly than ":", and its `Else` binds to the lexically nearest preceding `If` that is allowed by the syntax.</span></span> <span data-ttu-id="0200d-399">Por exemplo, as duas versões a seguir são equivalentes:</span><span class="sxs-lookup"><span data-stu-id="0200d-399">For example, the following two versions are equivalent:</span></span>

```vb
If True Then _
If True Then Console.WriteLine("a") Else Console.WriteLine("b") _
Else Console.WriteLine("c") : Console.WriteLine("d")

If True Then
    If True Then
        Console.WriteLine("a")
    Else
        Console.WriteLine("b")
    End If
    Console.WriteLine("c") : Console.WriteLine("d")
End If
```

<span data-ttu-id="0200d-400">Todas as instruções que não sejam instruções de declaração de rótulo são permitidas dentro de uma linha `If` instrução, incluindo instruções de blocos.</span><span class="sxs-lookup"><span data-stu-id="0200d-400">All statements other than label declaration statements are allowed inside a line `If` statement, including block statements.</span></span> <span data-ttu-id="0200d-401">No entanto, eles não podem usar LineTerminators como StatementTerminators exceto dentro de expressões lambda de várias linhas.</span><span class="sxs-lookup"><span data-stu-id="0200d-401">However, they may not use LineTerminators as StatementTerminators except inside multi-line lambda expressions.</span></span> <span data-ttu-id="0200d-402">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-402">For example:</span></span>

```vb
' Allowed, since it uses : instead of LineTerminator to separate statements
If b Then With New String("a"(0),5) : Console.WriteLine(.Length) : End With

' Disallowed, since it uses a LineTerminator
If b then With New String("a"(0), 5)
              Console.WriteLine(.Length)
          End With

' Allowed, since it only uses LineTerminator inside a multi-line lambda
If b Then Call Sub()
                   Console.WriteLine("a")
               End Sub.Invoke()
```

### <a name="select-case-statements"></a><span data-ttu-id="0200d-403">Instruções Select Case</span><span class="sxs-lookup"><span data-stu-id="0200d-403">Select Case Statements</span></span>

<span data-ttu-id="0200d-404">Um `Select Case` instrução executa as instruções com base no valor de uma expressão.</span><span class="sxs-lookup"><span data-stu-id="0200d-404">A `Select Case` statement executes statements based on the value of an expression.</span></span>

```antlr
SelectStatement
    : 'Select' 'Case'? Expression StatementTerminator
      CaseStatement*
      CaseElseStatement?
      'End' 'Select' StatementTerminator
    ;

CaseStatement
    : 'Case' CaseClauses StatementTerminator
      Block?
    ;

CaseClauses
    : CaseClause ( Comma CaseClause )*
    ;

CaseClause
    : ( 'Is' LineTerminator? )? ComparisonOperator LineTerminator? Expression
    | Expression ( 'To' Expression )?
    ;

ComparisonOperator
    : '=' | '<' '>' | '<' | '>' | '>' '=' | '<' '='
    ;

CaseElseStatement
    : 'Case' 'Else' StatementTerminator
      Block?
    ;
```

<span data-ttu-id="0200d-405">A expressão deve ser classificada como um valor.</span><span class="sxs-lookup"><span data-stu-id="0200d-405">The expression must be classified as a value.</span></span> <span data-ttu-id="0200d-406">Quando um `Select Case` instrução é executada, o `Select` expressão é avaliada em primeiro lugar e o `Case` instruções, em seguida, são avaliadas na ordem da declaração textual.</span><span class="sxs-lookup"><span data-stu-id="0200d-406">When a `Select Case` statement is executed, the `Select` expression is evaluated first, and the `Case` statements are then evaluated in order of textual declaration.</span></span> <span data-ttu-id="0200d-407">A primeira `Case` instrução que é avaliada como `True` tem seu bloco executado.</span><span class="sxs-lookup"><span data-stu-id="0200d-407">The first `Case` statement that evaluates to `True` has its block executed.</span></span> <span data-ttu-id="0200d-408">Se nenhum `Case` instrução é avaliada como `True` e não há um `Case Else` instrução, esse bloco é executado.</span><span class="sxs-lookup"><span data-stu-id="0200d-408">If no `Case` statement evaluates to `True` and there is a `Case Else` statement, that block is executed.</span></span> <span data-ttu-id="0200d-409">Depois que um bloco tiver concluído a execução, a execução passa para o final do `Select` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-409">Once a block has finished executing, execution passes to the end of the `Select` statement.</span></span>

<span data-ttu-id="0200d-410">Execução de um `Case` bloco não tem permissão para "passar" para a próxima seção switch.</span><span class="sxs-lookup"><span data-stu-id="0200d-410">Execution of a `Case` block is not permitted to "fall through" to the next switch section.</span></span> <span data-ttu-id="0200d-411">Isso impede que uma classe comum de erros que ocorrem em outros idiomas quando um `Case` encerrar instrução acidentalmente é omitido.</span><span class="sxs-lookup"><span data-stu-id="0200d-411">This prevents a common class of bugs that occur in other languages when a `Case` terminating statement is accidentally omitted.</span></span> <span data-ttu-id="0200d-412">O exemplo a seguir ilustra esse comportamento:</span><span class="sxs-lookup"><span data-stu-id="0200d-412">The following example illustrates this behavior:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer = 10

        Select Case x
            Case 5
                Console.WriteLine("x = 5")
            Case 10
                Console.WriteLine("x = 10")
            Case 20 - 10
                Console.WriteLine("x = 20 - 10")
            Case 30
                Console.WriteLine("x = 30")
        End Select
    End Sub
End Module
```

<span data-ttu-id="0200d-413">O código imprime:</span><span class="sxs-lookup"><span data-stu-id="0200d-413">The code prints:</span></span>

```
x = 10
```

<span data-ttu-id="0200d-414">Embora `Case 10` e `Case 20 - 10` para o mesmo valor, selecione `Case 10` é executado porque ele precede `Case 20 - 10` textualmente.</span><span class="sxs-lookup"><span data-stu-id="0200d-414">Although `Case 10` and `Case 20 - 10` select for the same value, `Case 10` is executed because it precedes `Case 20 - 10` textually.</span></span> <span data-ttu-id="0200d-415">Quando a próxima `Case` é atingido, a execução continua após a `Select` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-415">When the next `Case` is reached, execution continues after the `Select` statement.</span></span>

<span data-ttu-id="0200d-416">Um `Case` cláusula pode assumir duas formas.</span><span class="sxs-lookup"><span data-stu-id="0200d-416">A `Case` clause may take two forms.</span></span> <span data-ttu-id="0200d-417">Uma forma é um recurso opcional `Is` palavra-chave, um operador de comparação e uma expressão.</span><span class="sxs-lookup"><span data-stu-id="0200d-417">One form is an optional `Is` keyword, a comparison operator, and an expression.</span></span> <span data-ttu-id="0200d-418">A expressão será convertida para o tipo dos `Select` expressão; se a expressão não é implicitamente conversível no tipo do `Select` expressão, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="0200d-418">The expression is converted to the type of the `Select` expression; if the expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="0200d-419">Se o `Select` expressão é *eletrônico*, é o operador de comparação *Op*e o `Case` expressão é *E1*, o caso é avaliado como *E OP E1*.</span><span class="sxs-lookup"><span data-stu-id="0200d-419">If the `Select` expression is *E*, the comparison operator is *Op*, and the `Case` expression is *E1*, the case is evaluated as *E OP E1*.</span></span> <span data-ttu-id="0200d-420">O operador deve ser válido para os tipos das duas expressões; Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-420">The operator must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="0200d-421">A outra forma é uma expressão opcionalmente seguida da palavra-chave `To` e uma segunda expressão.</span><span class="sxs-lookup"><span data-stu-id="0200d-421">The other form is an expression optionally followed by the keyword `To` and a second expression.</span></span> <span data-ttu-id="0200d-422">As duas expressões são convertidas para o tipo dos `Select` expressão; se qualquer expressão não é implicitamente conversível no tipo do `Select` expressão, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="0200d-422">Both expressions are converted to the type of the `Select` expression; if either expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="0200d-423">Se o `Select` expressão é `E`, a primeira `Case` expressão é `E1`e a segunda `Case` expressão é `E2`, o `Case` é avaliada como `E = E1` (se nenhum `E2`for especificado) ou `(E >= E1) And (E <= E2)`.</span><span class="sxs-lookup"><span data-stu-id="0200d-423">If the `Select` expression is `E`, the first `Case` expression is `E1`, and the second `Case` expression is `E2`, the `Case` is evaluated either as `E = E1` (if no `E2` is specified) or `(E >= E1) And (E <= E2)`.</span></span> <span data-ttu-id="0200d-424">Os operadores devem ser válidos para os tipos das duas expressões; Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-424">The operators must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>


## <a name="loop-statements"></a><span data-ttu-id="0200d-425">Instruções de loop</span><span class="sxs-lookup"><span data-stu-id="0200d-425">Loop Statements</span></span>

<span data-ttu-id="0200d-426">Instruções de loop permitem que a execução repetida de instruções no corpo.</span><span class="sxs-lookup"><span data-stu-id="0200d-426">Loop statements allow repeated execution of the statements in their body.</span></span>

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

<span data-ttu-id="0200d-427">Cada vez que o corpo do loop for inserido, uma nova cópia é feita de todas as variáveis locais declaradas no corpo dessa, inicializado com os valores anteriores das variáveis.</span><span class="sxs-lookup"><span data-stu-id="0200d-427">Each time a loop body is entered, a fresh copy is made of all local variables declared in that body, initialized to the previous values of the variables.</span></span> <span data-ttu-id="0200d-428">Qualquer referência a uma variável dentro do corpo do loop usará a cópia feita mais recentemente.</span><span class="sxs-lookup"><span data-stu-id="0200d-428">Any reference to a variable within the loop body will use the most recently made copy.</span></span> <span data-ttu-id="0200d-429">Este código mostra um exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-429">This code shows an example:</span></span>

```vb
Module Test
    Sub Main()
        Dim lambdas As New List(Of Action)
        Dim x = 1

        For i = 1 To 3
            x = i
            Dim y = x
            lambdas.Add(Sub() Console.WriteLine(x & y))
        Next

        For Each lambda In lambdas
            lambda()
        Next
    End Sub
End Module
```

<span data-ttu-id="0200d-430">O código produz a saída:</span><span class="sxs-lookup"><span data-stu-id="0200d-430">The code produces the output:</span></span>

```
31    32    33
```

<span data-ttu-id="0200d-431">Quando o corpo do loop é executado, ele usa qualquer cópia da variável é atual.</span><span class="sxs-lookup"><span data-stu-id="0200d-431">When the loop body is executed, it uses whichever copy of the variable is current.</span></span> <span data-ttu-id="0200d-432">Por exemplo, a instrução `Dim y = x` refere-se à cópia mais recente do `y` e a cópia original do `x`.</span><span class="sxs-lookup"><span data-stu-id="0200d-432">For example, the statement  `Dim y = x` refers to the latest copy of `y` and the original copy of `x`.</span></span> <span data-ttu-id="0200d-433">E quando uma lambda é criada, ela lembra a qualquer cópia de uma variável era atual no momento em que ele foi criado.</span><span class="sxs-lookup"><span data-stu-id="0200d-433">And when a lambda is created, it remembers whichever copy of a variable was current at the time it was created.</span></span> <span data-ttu-id="0200d-434">Portanto, cada lambda usa a mesma cópia compartilhada da `x`, mas uma outra cópia do `y`.</span><span class="sxs-lookup"><span data-stu-id="0200d-434">Therefore, each lambda uses the same shared copy of `x`, but a different copy of `y`.</span></span> <span data-ttu-id="0200d-435">No final do programa, quando ele executa os lambdas que cópia compartilhada do `x` que todos eles denotam agora está em seu valor final 3.</span><span class="sxs-lookup"><span data-stu-id="0200d-435">At the end of the program, when it executes the lambdas, that shared copy of `x` that they all refer to is now at its final value 3.</span></span>

<span data-ttu-id="0200d-436">Observe que se não houver nenhum lambdas ou expressões de LINQ, em seguida, é impossível saber que uma nova cópia é feita na entrada do loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-436">Note that if there are no lambdas or LINQ expressions, then it's impossible to know that a fresh copy is made on loop entry.</span></span> <span data-ttu-id="0200d-437">Na verdade, as otimizações do compilador evitará fazendo cópias nesse caso.</span><span class="sxs-lookup"><span data-stu-id="0200d-437">Indeed, compiler optimizations will avoid making copies in this case.</span></span> <span data-ttu-id="0200d-438">Observe também que é ilegal `GoTo` em um loop que contém expressões de LINQ ou lambdas.</span><span class="sxs-lookup"><span data-stu-id="0200d-438">Note too that it's illegal to `GoTo` into a loop that contains lambdas or LINQ expressions.</span></span>


### <a name="whileend-while-and-doloop-statements"></a><span data-ttu-id="0200d-439">While... End ao mesmo tempo e não... Instruções de loop</span><span class="sxs-lookup"><span data-stu-id="0200d-439">While...End While and Do...Loop Statements</span></span>

<span data-ttu-id="0200d-440">Um `While` ou `Do` loop loop for baseado em uma expressão booliana.</span><span class="sxs-lookup"><span data-stu-id="0200d-440">A `While` or `Do` loop statement loops based on a Boolean expression.</span></span>

```antlr
WhileStatement
    : 'While' BooleanExpression StatementTerminator
      Block?
      'End' 'While' StatementTerminator
    ;

DoLoopStatement
    : DoTopLoopStatement
    | DoBottomLoopStatement
    ;

DoTopLoopStatement
    : 'Do' ( WhileOrUntil BooleanExpression )? StatementTerminator
      Block?
      'Loop' StatementTerminator
    ;

DoBottomLoopStatement
    : 'Do' StatementTerminator
      Block?
      'Loop' WhileOrUntil BooleanExpression StatementTerminator
    ;

WhileOrUntil
    : 'While' | 'Until'
    ;
```

<span data-ttu-id="0200d-441">Um `While` instrução de loop executa um loop, desde a expressão booleana for avaliada como true; um `Do` instrução loop pode conter uma condição mais complexa.</span><span class="sxs-lookup"><span data-stu-id="0200d-441">A `While` loop statement loops as long as the Boolean expression evaluates to true; a `Do` loop statement may contain a more complex condition.</span></span> <span data-ttu-id="0200d-442">Uma expressão pode ser colocada após o `Do` palavra-chave ou após o `Loop` palavra-chave, mas não após os dois.</span><span class="sxs-lookup"><span data-stu-id="0200d-442">An expression may be placed after the `Do` keyword or after the `Loop` keyword, but not after both.</span></span> <span data-ttu-id="0200d-443">A expressão booliana é avaliada de acordo com a seção [expressões Boolianas](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="0200d-443">The Boolean expression is evaluated as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="0200d-444">(Observação: isso não exige que a expressão para ter um tipo booliano).</span><span class="sxs-lookup"><span data-stu-id="0200d-444">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="0200d-445">Também é válido para não especificar nenhuma expressão Nesse caso, o loop nunca será encerrado.</span><span class="sxs-lookup"><span data-stu-id="0200d-445">It is also valid to specify no expression at all; in that case, the loop will never exit.</span></span> <span data-ttu-id="0200d-446">Se a expressão é posicionada após `Do`, ele será avaliado antes do bloco de loop é executado em cada iteração.</span><span class="sxs-lookup"><span data-stu-id="0200d-446">If the expression is placed after `Do`, it will be evaluated before the loop block is executed on each iteration.</span></span> <span data-ttu-id="0200d-447">Se a expressão é posicionada após `Loop`, ele será avaliado depois que o bloco de loop for executado em cada iteração.</span><span class="sxs-lookup"><span data-stu-id="0200d-447">If the expression is placed after `Loop`, it will be evaluated after the loop block has executed on each iteration.</span></span> <span data-ttu-id="0200d-448">Colocar a expressão após `Loop` , portanto, irá gerar um loop mais que o posicionamento após `Do`.</span><span class="sxs-lookup"><span data-stu-id="0200d-448">Placing the expression after `Loop` will therefore generate one more loop than placement after `Do`.</span></span> <span data-ttu-id="0200d-449">O exemplo a seguir demonstra esse comportamento:</span><span class="sxs-lookup"><span data-stu-id="0200d-449">The following example demonstrates this behavior:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer

        x = 3
        Do While x = 1
            Console.WriteLine("First loop")
        Loop

        Do
            Console.WriteLine("Second loop")
        Loop While x = 1
    End Sub
End Module
```

<span data-ttu-id="0200d-450">O código produz a saída:</span><span class="sxs-lookup"><span data-stu-id="0200d-450">The code produces the output:</span></span>

```
Second Loop
```

<span data-ttu-id="0200d-451">No caso do primeiro loop, a condição é avaliada antes que o loop é executado.</span><span class="sxs-lookup"><span data-stu-id="0200d-451">In the case of the first loop, the condition is evaluated before the loop executes.</span></span> <span data-ttu-id="0200d-452">No caso do segundo loop, a condição é executada depois que o loop é executado.</span><span class="sxs-lookup"><span data-stu-id="0200d-452">In the case of the second loop, the condition is executed after the loop executes.</span></span> <span data-ttu-id="0200d-453">A expressão condicional deve ser prefixada com um `While` palavra-chave ou um `Until` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="0200d-453">The conditional expression must be prefixed with either a `While` keyword or an `Until` keyword.</span></span> <span data-ttu-id="0200d-454">O primeiro interrompe o loop se a condição for avaliada como false, o último quando a condição for avaliada como true.</span><span class="sxs-lookup"><span data-stu-id="0200d-454">The former breaks the loop if the condition evaluates to false, the latter when the condition evaluates to true.</span></span>

<span data-ttu-id="0200d-455">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0200d-455">__Note.__</span></span> <span data-ttu-id="0200d-456">`Until` não é uma palavra reservada.</span><span class="sxs-lookup"><span data-stu-id="0200d-456">`Until` is not a reserved word.</span></span>


### <a name="fornext-statements"></a><span data-ttu-id="0200d-457">Para... Próximas instruções</span><span class="sxs-lookup"><span data-stu-id="0200d-457">For...Next Statements</span></span>

<span data-ttu-id="0200d-458">Um `For...Next` loop for baseado em um conjunto de limites.</span><span class="sxs-lookup"><span data-stu-id="0200d-458">A `For...Next` statement loops based on a set of bounds.</span></span> <span data-ttu-id="0200d-459">Um `For` declaração especifica uma variável de controle de loop, uma expressão de limite inferior, uma expressão de limite superior e uma expressão de valor da etapa opcional.</span><span class="sxs-lookup"><span data-stu-id="0200d-459">A `For` statement specifies a loop control variable, a lower bound expression, an upper bound expression, and an optional step value expression.</span></span> <span data-ttu-id="0200d-460">A variável de controle de loop é especificada por meio de um identificador seguido por um recurso opcional `As` cláusula ou uma expressão.</span><span class="sxs-lookup"><span data-stu-id="0200d-460">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

```antlr
ForStatement
    : 'For' LoopControlVariable Equals Expression 'To' Expression
      ( 'Step' Expression )? StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;

LoopControlVariable
    : Identifier ( IdentifierModifiers 'As' TypeName )?
    | Expression
    ;

NextExpressionList
    : Expression ( Comma Expression )*
    ;
```

<span data-ttu-id="0200d-461">De acordo com as regras a seguir, a variável de controle de loop refere-se para uma nova variável local específica para isso `For...Next` instrução, ou a uma variável já existente, ou como uma expressão.</span><span class="sxs-lookup"><span data-stu-id="0200d-461">As per the following rules, the loop control variable refers either to a new local variable specific to this `For...Next` statement, or to a pre-existing variable, or to an expression.</span></span>

* <span data-ttu-id="0200d-462">Se a variável de controle de loop é um identificador com um `As` cláusula, o identificador define uma nova variável local do tipo especificado em de `As` cláusula, no escopo para todo o `For` loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-462">If the loop control variable is an identifier with an `As` clause, the identifier defines a new local variable of the type specified in the `As` clause, scoped to the entire `For` loop.</span></span>

* <span data-ttu-id="0200d-463">Se a variável de controle de loop é um identificador sem um `As` cláusula e, em seguida, o identificador do primeiro é resolvido usando as regras de resolução de nome simples (consulte a seção [expressões simples de nome](expressions.md#simple-name-expressions)), exceto que dessa ocorrência do o identificador não por si só causaria uma variável local implícita a ser criado (seção [implícita declarações locais](statements.md#implicit-local-declarations)).</span><span class="sxs-lookup"><span data-stu-id="0200d-463">If the loop control variable is an identifier without an `As` clause, then the identifier is first resolved using the simple name resolution rules (see Section [Simple Name Expressions](expressions.md#simple-name-expressions)), excepting that this occurrence of the identifier would not in and of itself cause an implicit local variable to be created (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)).</span></span>

  * <span data-ttu-id="0200d-464">Se essa resolução for bem-sucedida e o resultado é classificado como uma variável, a variável de controle de loop é essa variável já existente.</span><span class="sxs-lookup"><span data-stu-id="0200d-464">If this resolution succeeds and the result is classified as a variable, then the loop control variable is that pre-existing variable.</span></span>

  * <span data-ttu-id="0200d-465">Se a resolução falhar ou se a resolução for bem-sucedida e o resultado é classificado como um tipo, em seguida:</span><span class="sxs-lookup"><span data-stu-id="0200d-465">If resolution fails, or if resolution succeeds and the result is classified as a type, then:</span></span>
    * <span data-ttu-id="0200d-466">Se a inferência de tipo de variável local está sendo usada, o identificador define uma nova variável local cujo tipo é inferido do que o limite e expressões, como todo o escopo de etapa `For` loop;</span><span class="sxs-lookup"><span data-stu-id="0200d-466">if local variable type inference is being used, the identifier defines a new local variable whose type is inferred from the bound and step expressions, scoped to the entire `For` loop;</span></span>
    * <span data-ttu-id="0200d-467">Se inferência de tipo de variável local não está sendo usada, mas a declaração local implícita é, então uma variável local implícita é criada, cujo escopo é o método inteiro (seção [implícita declarações locais](statements.md#implicit-local-declarations)) e a variável de controle de loop refere-se a essa variável pré-existente;</span><span class="sxs-lookup"><span data-stu-id="0200d-467">if local variable type inference is not being used but implicit local declaration is, then an implicit local variable is created whose scope is the entire method (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)), and the loop control variable refers to this pre-existing variable;</span></span>
    * <span data-ttu-id="0200d-468">Se não inferência de tipo de variável local nem implícitas declarações locais são usadas, é um erro.</span><span class="sxs-lookup"><span data-stu-id="0200d-468">if neither local variable type inference nor implicit local declarations are used, it is an error.</span></span>

  * <span data-ttu-id="0200d-469">Se a resolução for bem-sucedida com algo classificado como um tipo nem uma variável, ele é um erro.</span><span class="sxs-lookup"><span data-stu-id="0200d-469">If resolution succeeds with something classified as neither a type nor a variable, it is an error.</span></span>

* <span data-ttu-id="0200d-470">Se a variável de controle de loop for uma expressão, a expressão deve ser classificada como uma variável.</span><span class="sxs-lookup"><span data-stu-id="0200d-470">If the loop control variable is an expression, the expression must be classified as a variable.</span></span>

<span data-ttu-id="0200d-471">Uma variável de controle de loop não pode ser usada por outro delimitador `For...Next` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-471">A loop control variable cannot be used by another enclosing `For...Next` statement.</span></span> <span data-ttu-id="0200d-472">O tipo da variável de controle de loop de um `For` instrução determina o tipo da iteração e deve ser um destes:</span><span class="sxs-lookup"><span data-stu-id="0200d-472">The type of the loop control variable of a `For` statement determines the type of the iteration and must be one of:</span></span>

* <span data-ttu-id="0200d-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span><span class="sxs-lookup"><span data-stu-id="0200d-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span></span>
* <span data-ttu-id="0200d-474">Um tipo enumerado</span><span class="sxs-lookup"><span data-stu-id="0200d-474">An enumerated type</span></span>
* `Object`
* <span data-ttu-id="0200d-475">Um tipo `T` que tem os seguintes operadores, onde `B` é um tipo que pode ser usado em uma expressão booliana:</span><span class="sxs-lookup"><span data-stu-id="0200d-475">A type `T` that has the following operators, where `B` is a type that can be used in a Boolean expression:</span></span>

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

<span data-ttu-id="0200d-476">O limite e expressões de etapa devem ser implicitamente conversíveis para o tipo da variável de controle de loop e devem ser classificadas como valores.</span><span class="sxs-lookup"><span data-stu-id="0200d-476">The bound and step expressions must be implicitly convertible to the type of the loop control variable and must be classified as values.</span></span> <span data-ttu-id="0200d-477">Em tempo de compilação, o tipo da variável de controle de loop é inferido, escolhendo o tipo o maior entre o limite inferior, o limite superior e os tipos de expressão de etapa.</span><span class="sxs-lookup"><span data-stu-id="0200d-477">At compile time, the type of the loop control variable is inferred by choosing the widest type among the lower bound, upper bound, and step expression types.</span></span> <span data-ttu-id="0200d-478">Se não houver nenhuma conversão de ampliação entre dois dos tipos, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-478">If there is no widening conversion between two of the types, then a compile-time error occurs.</span></span>

<span data-ttu-id="0200d-479">No tempo de execução, se o tipo da variável de controle de loop for `Object`, em seguida, o tipo da iteração é inferido a mesma que em tempo de compilação, com duas exceções.</span><span class="sxs-lookup"><span data-stu-id="0200d-479">At run time, if the type of the loop control variable is `Object`, then the type of the iteration is inferred the same as at compile time, with two exceptions.</span></span> <span data-ttu-id="0200d-480">Primeiro, se o limite e expressões de etapa estão todos os tipos integrais, mas não ter nenhum tipo o maior e o tipo o maior que abrange todos os três tipos será inferido.</span><span class="sxs-lookup"><span data-stu-id="0200d-480">First, if the bound and step expressions are all of integral types but have no widest type, then the widest type that encompasses all three types will be inferred.</span></span> <span data-ttu-id="0200d-481">E, segundo, se o tipo da variável de controle de loop é inferido para ser `String`, `Double` será inferido.</span><span class="sxs-lookup"><span data-stu-id="0200d-481">And second, if the type of the loop control variable is inferred to be `String`, `Double` will be inferred instead.</span></span> <span data-ttu-id="0200d-482">Se, em tempo de execução, nenhum tipo de controle de loop pode ser determinado ou se qualquer uma das expressões não pode ser convertido no tipo de controle de loop, um `System.InvalidCastException` ocorrerá.</span><span class="sxs-lookup"><span data-stu-id="0200d-482">If, at run time, no loop control type can be determined or if any of the expressions cannot be converted to the loop control type, a `System.InvalidCastException` will occur.</span></span> <span data-ttu-id="0200d-483">Depois que um tipo de controle de loop for escolhido no início do loop, o mesmo tipo será usado ao longo da iteração, independentemente das alterações feitas ao valor na variável de controle de loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-483">Once a loop control type has been chosen at the beginning of the loop, the same type will be used throughout the iteration, regardless of changes made to the value in the loop control variable.</span></span>

<span data-ttu-id="0200d-484">Um `For` instrução deve ser fechada por uma correspondência `Next` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-484">A `For` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="0200d-485">Um `Next` instrução sem uma variável corresponde ao abrir o mais interno `For` instrução, enquanto um `Next` instrução com uma ou mais variáveis de controle de loop, da esquerda para direita, corresponderá a `For` loops que correspondem a cada variável.</span><span class="sxs-lookup"><span data-stu-id="0200d-485">A `Next` statement without a variable matches the innermost open `For` statement, while a `Next` statement with one or more loop control variables will, from left to right, match the `For` loops that match each variable.</span></span> <span data-ttu-id="0200d-486">Se corresponder a uma variável de um `For` resulta do loop for que não é o loop aninhado mais nesse ponto, um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-486">If a variable matches a `For` loop that is not the most nested loop at that point, a compile-time error results.</span></span>

<span data-ttu-id="0200d-487">No início do loop, três expressões são avaliadas na ordem textual e a expressão de limite inferior é atribuída à variável de controle de loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-487">At the beginning of the loop, the three expressions are evaluated in textual order and the lower bound expression is assigned to the loop control variable.</span></span> <span data-ttu-id="0200d-488">Se o valor da etapa for omitido, é implicitamente o literal `1`, convertido para o tipo da variável de controle de loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-488">If the step value is omitted, it is implicitly the literal `1`, converted to the type of the loop control variable.</span></span> <span data-ttu-id="0200d-489">As três expressões são avaliadas apenas no início do loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-489">The three expressions are only ever evaluated at the beginning of the loop.</span></span>

<span data-ttu-id="0200d-490">No início de cada loop, a variável de controle é comparada para ver se ele for maior que o ponto de extremidade se a expressão de etapa for positivo, ou menor que o ponto de extremidade, se a expressão de etapa for negativa.</span><span class="sxs-lookup"><span data-stu-id="0200d-490">At the beginning of each loop, the control variable is compared to see if it is greater than the end point if the step expression is positive, or less than the end point if the step expression is negative.</span></span> <span data-ttu-id="0200d-491">Se for, o `For` loop é encerrado; caso contrário, o bloco de loop é executado.</span><span class="sxs-lookup"><span data-stu-id="0200d-491">If it is, the `For` loop terminates; otherwise the loop block executes.</span></span> <span data-ttu-id="0200d-492">Se a variável de controle de loop não é um tipo primitivo, o operador de comparação é determinado pelo se a expressão `step >= step - step` é true ou false.</span><span class="sxs-lookup"><span data-stu-id="0200d-492">If the loop control variable is not a primitive type, the comparison operator is determined by whether the expression `step >= step - step` is true or false.</span></span> <span data-ttu-id="0200d-493">No `Next` instrução, o valor da etapa é adicionado à variável de controle e execução retorna para a parte superior do loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-493">At the `Next` statement, the step value is added to the control variable and execution returns to the top of the loop.</span></span>

<span data-ttu-id="0200d-494">Observe que é uma nova cópia da variável de controle de loop *não* criada em cada iteração do bloco de loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-494">Note that a new copy of the loop control variable is *not* created on each iteration of the loop block.</span></span> <span data-ttu-id="0200d-495">Nesse sentido, o `For` instrução difere `For Each` (seção [para cada um... Próximas instruções](statements.md#for-eachnext-statements)).</span><span class="sxs-lookup"><span data-stu-id="0200d-495">In this respect, the `For` statement differs from `For Each` (Section [For Each...Next Statements](statements.md#for-eachnext-statements)).</span></span>

<span data-ttu-id="0200d-496">Não é válido ramificar em uma `For` executar um loop de fora do loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-496">It is not valid to branch into a `For` loop from outside the loop.</span></span>


### <a name="for-eachnext-statements"></a><span data-ttu-id="0200d-497">Para cada um... Próximas instruções</span><span class="sxs-lookup"><span data-stu-id="0200d-497">For Each...Next Statements</span></span>

<span data-ttu-id="0200d-498">Um `For Each...Next` loops de instrução com base nos elementos em uma expressão.</span><span class="sxs-lookup"><span data-stu-id="0200d-498">A `For Each...Next` statement loops based on the elements in an expression.</span></span> <span data-ttu-id="0200d-499">Um `For Each` declaração especifica uma variável de controle de loop e uma expressão do enumerador.</span><span class="sxs-lookup"><span data-stu-id="0200d-499">A `For Each` statement specifies a loop control variable and an enumerator expression.</span></span> <span data-ttu-id="0200d-500">A variável de controle de loop é especificada por meio de um identificador seguido por um recurso opcional `As` cláusula ou uma expressão.</span><span class="sxs-lookup"><span data-stu-id="0200d-500">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

<span data-ttu-id="0200d-501">Seguindo as mesmas regras `For...Next` instruções (seção [para... Próximas instruções](statements.md#fornext-statements)), a variável de controle de loop refere-se para uma nova variável local específica para isso para cada um... Próxima instrução, ou a uma variável já existente, ou como uma expressão.</span><span class="sxs-lookup"><span data-stu-id="0200d-501">Following the same rules as `For...Next` statements (Section [For...Next Statements](statements.md#fornext-statements)), the loop control variable refers either to a new local variable specific to this For Each...Next statement, or to a pre-existing variable, or to an expression.</span></span>

<span data-ttu-id="0200d-502">A expressão do enumerador deve ser classificada como um valor e seu tipo deve ser um tipo de coleção ou `Object`.</span><span class="sxs-lookup"><span data-stu-id="0200d-502">The enumerator expression must be classified as a value and its type must be a collection type or `Object`.</span></span> <span data-ttu-id="0200d-503">Se o tipo da expressão do enumerador é `Object`, em seguida, todo o processamento é adiada até o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="0200d-503">If the type of the enumerator expression is `Object`, then all processing is deferred until run-time.</span></span> <span data-ttu-id="0200d-504">Caso contrário, uma conversão deve existir do tipo de elemento da coleção para o tipo da variável de controle de loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-504">Otherwise, a conversion must exist from the element type of the collection to the type of the loop control variable.</span></span>

<span data-ttu-id="0200d-505">A variável de controle de loop não pode ser usada por outro delimitador `For Each` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-505">The loop control variable cannot be used by another enclosing `For Each` statement.</span></span> <span data-ttu-id="0200d-506">Um `For Each` instrução deve ser fechada por uma correspondência `Next` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-506">A `For Each` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="0200d-507">Um `Next` instrução sem uma variável de controle de loop corresponde ao abrir o mais interno `For Each`.</span><span class="sxs-lookup"><span data-stu-id="0200d-507">A `Next` statement without a loop control variable matches the innermost open `For Each`.</span></span> <span data-ttu-id="0200d-508">Um `Next` instrução com uma ou mais variáveis de controle de loop, da esquerda para direita, corresponderá a `For Each` loops que têm a mesma variável de controle de loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-508">A `Next` statement with one or more loop control variables will, from left to right, match the `For Each` loops that have the same loop control variable.</span></span> <span data-ttu-id="0200d-509">Se corresponder a uma variável de um `For Each` ocorre do loop for que não é o loop aninhado mais nesse ponto, um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-509">If a variable matches a `For Each` loop that is not the most nested loop at that point, a compile-time error occurs.</span></span>

<span data-ttu-id="0200d-510">Um tipo `C` deve ser um *tipo de coleção* se um de:</span><span class="sxs-lookup"><span data-stu-id="0200d-510">A type `C` is said to be a *collection type* if one of:</span></span>

* <span data-ttu-id="0200d-511">Todos os itens a seguir forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="0200d-511">All of the following are true:</span></span>
  * <span data-ttu-id="0200d-512">`C` contém um método de extensão com a assinatura ou uma instância acessível, compartilhados `GetEnumerator()` que retorna um tipo `E`.</span><span class="sxs-lookup"><span data-stu-id="0200d-512">`C` contains an accessible instance, shared or extension method with the signature `GetEnumerator()` that returns a type `E`.</span></span>
  * <span data-ttu-id="0200d-513">`E` contém um método de extensão com a assinatura ou uma instância acessível, compartilhados `MoveNext()` e o tipo de retorno `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="0200d-513">`E` contains an accessible instance, shared or extension method with the signature `MoveNext()` and the return type `Boolean`.</span></span>
  * <span data-ttu-id="0200d-514">`E` contém uma propriedade compartilhada chamada ou instância acessível `Current` que tem um getter.</span><span class="sxs-lookup"><span data-stu-id="0200d-514">`E` contains an accessible instance or shared property named `Current` that has a getter.</span></span> <span data-ttu-id="0200d-515">O tipo dessa propriedade é o tipo de elemento do tipo de coleção.</span><span class="sxs-lookup"><span data-stu-id="0200d-515">The type of this property is the element type of the collection type.</span></span>

* <span data-ttu-id="0200d-516">Ele implementa a interface `System.Collections.Generic.IEnumerable(Of T)`, caso em que o tipo de elemento da coleção é considerado `T`.</span><span class="sxs-lookup"><span data-stu-id="0200d-516">It implements the interface `System.Collections.Generic.IEnumerable(Of T)`, in which case the element type of the collection is considered to be `T`.</span></span>

* <span data-ttu-id="0200d-517">Ele implementa a interface `System.Collections.IEnumerable`, caso em que o tipo de elemento da coleção é considerado `Object`.</span><span class="sxs-lookup"><span data-stu-id="0200d-517">It implements the interface `System.Collections.IEnumerable`, in which case the element type of the collection is considered to be `Object`.</span></span>

<span data-ttu-id="0200d-518">A seguir está um exemplo de uma classe que pode ser enumerado:</span><span class="sxs-lookup"><span data-stu-id="0200d-518">Following is an example of a class that can be enumerated:</span></span>

```vb
Public Class IntegerCollection
    Private integers(10) As Integer

    Public Class IntegerCollectionEnumerator
        Private collection As IntegerCollection
        Private index As Integer = -1

        Friend Sub New(c As IntegerCollection)
            collection = c
        End Sub

        Public Function MoveNext() As Boolean
            index += 1

            Return index <= 10
        End Function

        Public ReadOnly Property Current As Integer
            Get
                If index < 0 OrElse index > 10 Then
                    Throw New System.InvalidOperationException()
                End If

                Return collection.integers(index)
            End Get
        End Property
    End Class

    Public Sub New()
        Dim i As Integer

        For i = 0 To 10
            integers(i) = I
        Next i
    End Sub

    Public Function GetEnumerator() As IntegerCollectionEnumerator
        Return New IntegerCollectionEnumerator(Me)
    End Function
End Class
```

<span data-ttu-id="0200d-519">Antes de inicia o loop, a expressão do enumerador é avaliada.</span><span class="sxs-lookup"><span data-stu-id="0200d-519">Before the loop begins, the enumerator expression is evaluated.</span></span> <span data-ttu-id="0200d-520">Se o tipo da expressão não satisfaz o padrão de design, a expressão é convertida para `System.Collections.IEnumerable` ou `System.Collections.Generic.IEnumerable(Of T)`.</span><span class="sxs-lookup"><span data-stu-id="0200d-520">If the type of the expression does not satisfy the design pattern, then the expression is cast to `System.Collections.IEnumerable` or `System.Collections.Generic.IEnumerable(Of T)`.</span></span> <span data-ttu-id="0200d-521">Se o tipo de expressão implementa a interface genérica, a interface genérica é preferencial em tempo de compilação, mas a interface não genérica é preferencial em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="0200d-521">If the expression type implements the generic interface, the generic interface is preferred at compile-time but the non-generic interface is preferred at run-time.</span></span> <span data-ttu-id="0200d-522">Se o tipo de expressão implementa a interface genérica várias vezes, a instrução será considerada ambígua e ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-522">If the expression type implements the generic interface multiple times, the statement is considered ambiguous and a compile-time error occurs.</span></span>

<span data-ttu-id="0200d-523">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0200d-523">__Note.__</span></span> <span data-ttu-id="0200d-524">A interface não genérica é preferida no caso de tardia acoplado, porque a interface genérica de separação significa que todas as chamadas para os métodos de interface envolveria a parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="0200d-524">The non-generic interface is preferred in the late bound case, because picking the generic interface would mean that all the calls to the interface methods would involve type parameters.</span></span> <span data-ttu-id="0200d-525">Como não é possível saber a correspondência de argumentos de tipo em tempo de execução, todas as chamadas precisaria ser feita usando chamadas de associação tardia.</span><span class="sxs-lookup"><span data-stu-id="0200d-525">Since it is not possible to know the matching type arguments at run-time, all such calls would have to be made using late-bound calls.</span></span> <span data-ttu-id="0200d-526">Isso pode ser mais lento do que chamar a interface não genérica, porque a interface não genérica pode ser chamada usando chamadas de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-526">This would be slower than calling the non-generic interface because the non-generic interface could be called using compile-time calls.</span></span>

<span data-ttu-id="0200d-527">`GetEnumerator` é chamado no valor resultante e o retorne o valor da função é armazenado em um temporário.</span><span class="sxs-lookup"><span data-stu-id="0200d-527">`GetEnumerator` is called on the resulting value and the return value of the function is stored in a temporary.</span></span> <span data-ttu-id="0200d-528">Em seguida, no início de cada iteração, `MoveNext` é chamado em temporárias.</span><span class="sxs-lookup"><span data-stu-id="0200d-528">Then at the beginning of each iteration, `MoveNext` is called on the temporary.</span></span> <span data-ttu-id="0200d-529">Se ele retornar `False`, o loop é encerrado.</span><span class="sxs-lookup"><span data-stu-id="0200d-529">If it returns `False`, the loop terminates.</span></span> <span data-ttu-id="0200d-530">Caso contrário, cada iteração do loop é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0200d-530">Otherwise, each iteration of the loop is executed as follows:</span></span>

1. <span data-ttu-id="0200d-531">Se a variável de controle de loop identificou uma nova variável local (em vez de uma já existente), uma cópia atualizada dessa variável local é criada.</span><span class="sxs-lookup"><span data-stu-id="0200d-531">If the loop control variable identified a new local variable (rather than a pre-existing one), then a fresh copy of this local variable is created.</span></span> <span data-ttu-id="0200d-532">Para a iteração atual, todas as referências dentro do bloco de loop fará referência a essa cópia.</span><span class="sxs-lookup"><span data-stu-id="0200d-532">For the current iteration, all references within the loop block will refer to this copy.</span></span>
2. <span data-ttu-id="0200d-533">O `Current` propriedade é recuperada, forçado para o tipo da variável de controle de loop (independentemente se a conversão é implícita ou explícita) e atribuído à variável de controle de loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-533">The `Current` property is retrieved, coerced to the type of the loop control variable (regardless of whether the conversion is implicit or explicit), and assigned to the loop control variable.</span></span>
3. <span data-ttu-id="0200d-534">O bloco de loop é executado.</span><span class="sxs-lookup"><span data-stu-id="0200d-534">The loop block executes.</span></span>

<span data-ttu-id="0200d-535">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0200d-535">__Note.__</span></span> <span data-ttu-id="0200d-536">Há uma pequena alteração no comportamento entre a versão 10.0 e 11.0 do idioma.</span><span class="sxs-lookup"><span data-stu-id="0200d-536">There is a slight change in behavior between version 10.0 and 11.0 of the language.</span></span> <span data-ttu-id="0200d-537">Antes de 11.0, uma variável de iteração atualizada foi *não* criado para cada iteração do loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-537">Prior to 11.0, a fresh iteration variable was *not* created for each iteration of the loop.</span></span> <span data-ttu-id="0200d-538">Essa diferença é observável somente se a variável de iteração é capturada por um lambda ou uma expressão LINQ que, em seguida, é invocada após o loop:</span><span class="sxs-lookup"><span data-stu-id="0200d-538">This difference is observable only if the iteration variable is captured by a lambda or a LINQ expression which is then invoked after the loop:</span></span>

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

<span data-ttu-id="0200d-539">Até o Visual Basic 10.0, isso produziu um aviso em tempo de compilação e impresso "3" três vezes.</span><span class="sxs-lookup"><span data-stu-id="0200d-539">Up to Visual Basic 10.0, this produced a warning at compile-time and printed "3" three times.</span></span> <span data-ttu-id="0200d-540">Isso foi porque não havia somente uma variável compartilhada por todas as iterações do loop, "x" e todas as três lambdas capturados no mesmo "x" e, em seguida, no momento em que os lambdas foram executados ele mantido, em seguida, o número 3.</span><span class="sxs-lookup"><span data-stu-id="0200d-540">That was because there was only a single variable "x" shared by all iterations of the loop, and all three lambdas captured the same "x", and by the time the lambdas were executed it then held the number 3.</span></span> <span data-ttu-id="0200d-541">A partir do Visual Basic 11.0, ele imprime "1, 2, 3".</span><span class="sxs-lookup"><span data-stu-id="0200d-541">As of Visual Basic 11.0, it prints "1, 2, 3".</span></span> <span data-ttu-id="0200d-542">Isso ocorre porque cada lambda captura uma variável diferente "x".</span><span class="sxs-lookup"><span data-stu-id="0200d-542">That is because each lambda captures a different variable "x".</span></span>


<span data-ttu-id="0200d-543">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0200d-543">__Note.__</span></span> <span data-ttu-id="0200d-544">O elemento atual da iteração é convertido para o tipo da variável de controle de loop, mesmo se a conversão é explícita, pois há um local conveniente para introduzir um operador de conversão na instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-544">The current element of the iteration is converted to the type of the loop control variable even if the conversion is explicit because there is no convenient place to introduce a conversion operator in the statement.</span></span> <span data-ttu-id="0200d-545">Isso tornou-se especialmente problemático ao trabalhar com o tipo agora obsoleta `System.Collections.ArrayList`, porque seu tipo de elemento é `Object`.</span><span class="sxs-lookup"><span data-stu-id="0200d-545">This became particularly troublesome when working with the now-obsolete type `System.Collections.ArrayList`, because its element type is `Object`.</span></span> <span data-ttu-id="0200d-546">Isso teria as conversões necessárias em uma ótima muitos loops, algo que achamos não era ideal.</span><span class="sxs-lookup"><span data-stu-id="0200d-546">This would have required casts in a great many loops, something we felt was not ideal.</span></span> <span data-ttu-id="0200d-547">Ironicamente, genéricos habilitado a criação de uma coleção fortemente tipada, `System.Collections.Generic.List(Of T)`, que pode ter feito nos repensar a esse ponto de design, mas para meu Deus da compatibilidade, isso não pode ser alterado agora.</span><span class="sxs-lookup"><span data-stu-id="0200d-547">Ironically, generics enabled the creation of a strongly-typed collection, `System.Collections.Generic.List(Of T)`, which might have made us rethink this design point, but for compatibility's sake, this cannot be changed now.</span></span>


<span data-ttu-id="0200d-548">Quando o `Next` instrução for atingida, a execução retorna para a parte superior do loop.</span><span class="sxs-lookup"><span data-stu-id="0200d-548">When the `Next` statement is reached, execution returns to the top of the loop.</span></span> <span data-ttu-id="0200d-549">Se uma variável é especificada após o `Next` palavra-chave, ele deve ser o mesmo que a primeira variável após a `For Each`.</span><span class="sxs-lookup"><span data-stu-id="0200d-549">If a variable is specified after the `Next` keyword, it must be the same as the first variable after the `For Each`.</span></span> <span data-ttu-id="0200d-550">Por exemplo, considere o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="0200d-550">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim i As Integer
        Dim c As IntegerCollection = New IntegerCollection()

        For Each i In c
            Console.WriteLine(i)
        Next i
    End Sub
End Module
```

<span data-ttu-id="0200d-551">É equivalente ao seguinte código:</span><span class="sxs-lookup"><span data-stu-id="0200d-551">It is equivalent to the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim i As Integer
        Dim c As IntegerCollection = New IntegerCollection()

        Dim e As IntegerCollection.IntegerCollectionEnumerator

        e = c.GetEnumerator()
        While e.MoveNext()
            i = e.Current

            Console.WriteLine(i)
        End While
    End Sub
End Module
```

<span data-ttu-id="0200d-552">Se o tipo `E` do enumerador implementa `System.IDisposable`, em seguida, o enumerador é descartado ao sair do loop, chamando o `Dispose` método.</span><span class="sxs-lookup"><span data-stu-id="0200d-552">If the type `E` of the enumerator implements `System.IDisposable`, then the enumerator is disposed upon exiting the loop by calling the `Dispose` method.</span></span> <span data-ttu-id="0200d-553">Isso garante que os recursos mantidos pelo enumerador são liberados.</span><span class="sxs-lookup"><span data-stu-id="0200d-553">This ensures that resources held by the enumerator are released.</span></span> <span data-ttu-id="0200d-554">Se o método que contém o `For Each` instrução não usa o tratamento de erro não estruturados, o `For Each` instrução é encapsulado em um `Try` instrução com o `Dispose` método chamado no `Finally` para garantir a limpeza.</span><span class="sxs-lookup"><span data-stu-id="0200d-554">If the method containing the `For Each` statement does not use unstructured error handling, then the `For Each` statement is wrapped in a `Try` statement with the `Dispose` method called in the `Finally` to ensure cleanup.</span></span>

<span data-ttu-id="0200d-555">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0200d-555">__Note.__</span></span> <span data-ttu-id="0200d-556">O `System.Array` é um tipo de coleção e, pois a matriz por todos os tipos derivam `System.Array`, qualquer expressão de tipo de matriz é permitida em um `For Each` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-556">The `System.Array` type is a collection type, and since all array types derive from `System.Array`, any array type expression is permitted in a `For Each` statement.</span></span> <span data-ttu-id="0200d-557">Para matrizes unidimensionais, o `For Each` instrução enumera os elementos de matriz em ordem crescente de índice, começando com o índice 0 e terminando com o índice de comprimento - 1.</span><span class="sxs-lookup"><span data-stu-id="0200d-557">For single-dimensional arrays, the `For Each` statement enumerates the array elements in increasing index order, starting with index 0 and ending with index Length - 1.</span></span> <span data-ttu-id="0200d-558">Para matrizes multidimensionais, os índices da dimensão mais à direita são aumentados pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="0200d-558">For multidimensional arrays, the indices of the rightmost dimension are increased first.</span></span>

<span data-ttu-id="0200d-559">Por exemplo, o código a seguir imprime `1 2 3 4`:</span><span class="sxs-lookup"><span data-stu-id="0200d-559">For example, the following code prints `1 2 3 4`:</span></span>

```vb
Module Test
    Sub Main()
        Dim x(,) As Integer = { { 1, 2 }, { 3, 4 } }
        Dim i As Integer

        For Each i In x
            Console.Write(i & " ")
        Next i
    End Sub
End Module
```

<span data-ttu-id="0200d-560">Não é válido ramificar em uma `For Each` bloco de instruções de fora do bloco.</span><span class="sxs-lookup"><span data-stu-id="0200d-560">It is not valid to branch into a `For Each` statement block from outside the block.</span></span>


## <a name="exception-handling-statements"></a><span data-ttu-id="0200d-561">Instruções de manipulação de exceção</span><span class="sxs-lookup"><span data-stu-id="0200d-561">Exception-Handling Statements</span></span>

<span data-ttu-id="0200d-562">Visual Basic oferece suporte a tratamento de exceções estruturado e tratamento de exceções não estruturados.</span><span class="sxs-lookup"><span data-stu-id="0200d-562">Visual Basic supports structured exception handling and unstructured exception handling.</span></span> <span data-ttu-id="0200d-563">Apenas um estilo de tratamento de exceções pode ser usado em um método, mas o `Error` instrução pode ser usada na manipulação de exceção estruturada.</span><span class="sxs-lookup"><span data-stu-id="0200d-563">Only one style of exception handling may be used in a method, but the `Error` statement may be used in structured exception handling.</span></span> <span data-ttu-id="0200d-564">Se um método usa os dois estilos de tratamento de exceções, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="0200d-564">If a method uses both styles of exception handling, a compile-time error results.</span></span>

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a><span data-ttu-id="0200d-565">Instruções de tratamento de exceções estruturadas</span><span class="sxs-lookup"><span data-stu-id="0200d-565">Structured Exception-Handling Statements</span></span>

<span data-ttu-id="0200d-566">Tratamento de exceções estruturado é um método de tratamento de erros por meio da declaração explícitos blocos dentro do qual determinadas exceções serão tratadas.</span><span class="sxs-lookup"><span data-stu-id="0200d-566">Structured exception handling is a method of handling errors by declaring explicit blocks within which certain exceptions will be handled.</span></span> <span data-ttu-id="0200d-567">Tratamento de exceções estruturado é feito por meio de um `Try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-567">Structured exception handling is done through a `Try` statement.</span></span>

```antlr
StructuredErrorStatement
    : ThrowStatement
    | TryStatement
    ;

TryStatement
    : 'Try' StatementTerminator
      Block?
      CatchStatement*
      FinallyStatement?
      'End' 'Try' StatementTerminator
    ;
```


<span data-ttu-id="0200d-568">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-568">For example:</span></span>

```vb
Module Test
    Sub ThrowException()
        Throw New Exception()
    End Sub

    Sub Main()
        Try
            ThrowException()
        Catch e As Exception
            Console.WriteLine("Caught exception!")
        Finally
            Console.WriteLine("Exiting try.")
        End Try
    End Sub
End Module
```

<span data-ttu-id="0200d-569">Um `Try` instrução é composta por três tipos de blocos: blocos try, catch blocos e finalmente blocos.</span><span class="sxs-lookup"><span data-stu-id="0200d-569">A `Try` statement is made up of three kinds of blocks: try blocks, catch blocks, and finally blocks.</span></span> <span data-ttu-id="0200d-570">Um *bloco try* é um bloco de instrução que contém as instruções a serem executadas.</span><span class="sxs-lookup"><span data-stu-id="0200d-570">A *try block* is a statement block that contains the statements to be executed.</span></span> <span data-ttu-id="0200d-571">Um *bloco catch* é um bloco de instrução que trata uma exceção.</span><span class="sxs-lookup"><span data-stu-id="0200d-571">A *catch block* is a statement block that handles an exception.</span></span> <span data-ttu-id="0200d-572">Um *bloco finally* é um bloco de instrução que contém instruções para ser executado quando o `Try` instrução é finalizada, independentemente de uma exceção tenha ocorrido e foi tratada.</span><span class="sxs-lookup"><span data-stu-id="0200d-572">A *finally block* is a statement block that contains statements to be run when the `Try` statement is exited, regardless of whether an exception has occurred and been handled.</span></span> <span data-ttu-id="0200d-573">Um `Try` instrução, que pode conter apenas uma tentativa de bloco e outro, por fim, bloquear, deve conter pelo menos um bloco catch ou bloco finally.</span><span class="sxs-lookup"><span data-stu-id="0200d-573">A `Try` statement, which can only contain one try block and one finally block, must contain at least one catch block or finally block.</span></span> <span data-ttu-id="0200d-574">Não é válido para transferir a execução em um bloco try, exceto de um bloco catch na mesma instrução explicitamente.</span><span class="sxs-lookup"><span data-stu-id="0200d-574">It is invalid to explicitly transfer execution into a try block except from within a catch block in the same statement.</span></span>


#### <a name="finally-blocks"></a><span data-ttu-id="0200d-575">Blocos Finally</span><span class="sxs-lookup"><span data-stu-id="0200d-575">Finally Blocks</span></span>

<span data-ttu-id="0200d-576">Um `Finally` bloco sempre será executado quando a execução deixar qualquer parte do `Try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-576">A `Finally` block is always executed when execution leaves any part of the `Try` statement.</span></span> <span data-ttu-id="0200d-577">Nenhuma ação explícita é necessária para executar o `Finally` bloco; quando a execução deixa a `Try` instrução, o sistema será executado automaticamente o `Finally` bloquear e, em seguida, transferir a execução para seu destino pretendido.</span><span class="sxs-lookup"><span data-stu-id="0200d-577">No explicit action is required to execute the `Finally` block; when execution leaves the `Try` statement, the system will automatically execute the `Finally` block and then transfer execution to its intended destination.</span></span> <span data-ttu-id="0200d-578">O `Finally` bloco é executado, independentemente de como a execução deixa o `Try` instrução: até o final do `Try` bloco, até o final de um `Catch` bloquear, por meio um `Exit Try` instrução, por meio um `GoTo` instrução, ou por não manipular uma exceção gerada.</span><span class="sxs-lookup"><span data-stu-id="0200d-578">The `Finally` block is executed regardless of how execution leaves the `Try` statement: through the end of the `Try` block, through the end of a `Catch` block, through an `Exit Try` statement, through a `GoTo` statement, or by not handling a thrown exception.</span></span>

<span data-ttu-id="0200d-579">Observe que o `Await` expressão em um método assíncrono e o `Yield` instrução em um método iterador, pode fazer com que o fluxo de controle para a instância de método assíncrono ou iterador de suspensão e retomada em alguma outra instância de método.</span><span class="sxs-lookup"><span data-stu-id="0200d-579">Note that the `Await` expression in an async method, and the `Yield` statement in an iterator method, can cause flow of control to suspend in the async or iterator method instance and resume in some other method instance.</span></span> <span data-ttu-id="0200d-580">No entanto, isso é simplesmente uma suspensão de execução e não envolve sair a instância de método assíncrono respectivo método ou iterador e portanto, não faz com que `Finally` blocos a serem executadas.</span><span class="sxs-lookup"><span data-stu-id="0200d-580">However, this is merely a suspension of execution and does not involve exiting the respective async method or iterator method instance, and so does not cause `Finally` blocks to be executed.</span></span>

<span data-ttu-id="0200d-581">Não é válido para transferir explicitamente a execução em um `Finally` bloquear; também é inválido para transferir a execução de um `Finally` bloquear, exceto por meio de uma exceção.</span><span class="sxs-lookup"><span data-stu-id="0200d-581">It is invalid to explicitly transfer execution into a `Finally` block; it is also invalid to transfer execution out of a `Finally` block except through an exception.</span></span>

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a><span data-ttu-id="0200d-582">Blocos catch</span><span class="sxs-lookup"><span data-stu-id="0200d-582">Catch Blocks</span></span>

<span data-ttu-id="0200d-583">Se ocorrer uma exceção ao processar o `Try` bloquear, cada `Catch` instrução é examinada na ordem textual para determinar se ele trata a exceção.</span><span class="sxs-lookup"><span data-stu-id="0200d-583">If an exception occurs while processing the `Try` block, each `Catch` statement is examined in textual order to determine if it handles the exception.</span></span>

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

<span data-ttu-id="0200d-584">O identificador especificado em um `Catch` cláusula representa a exceção foi lançada.</span><span class="sxs-lookup"><span data-stu-id="0200d-584">The identifier specified in a `Catch` clause represents the exception that has been thrown.</span></span> <span data-ttu-id="0200d-585">Se o identificador contém um `As` cláusula e, em seguida, o identificador é considerado declarado dentro de `Catch` espaço de declaração de local do bloco.</span><span class="sxs-lookup"><span data-stu-id="0200d-585">If the identifier contains an `As` clause, then the identifier is considered to be declared within the `Catch` block's local declaration space.</span></span> <span data-ttu-id="0200d-586">Caso contrário, o identificador deve ser uma variável local (não uma variável estática) que foi definida em um bloco recipiente.</span><span class="sxs-lookup"><span data-stu-id="0200d-586">Otherwise, the identifier must be a local variable (not a static variable) that was defined in a containing block.</span></span>

<span data-ttu-id="0200d-587">Um `Catch` cláusula sem nenhum identificador irá capturar todas as exceções derivadas do `System.Exception`.</span><span class="sxs-lookup"><span data-stu-id="0200d-587">A `Catch` clause with no identifier will catch all exceptions derived from `System.Exception`.</span></span> <span data-ttu-id="0200d-588">Um `Catch` cláusula com um identificador só irá capturar exceções cujos tipos são iguais ou derivam do tipo do identificador.</span><span class="sxs-lookup"><span data-stu-id="0200d-588">A `Catch` clause with an identifier will only catch exceptions whose types are the same as or derived from the type of the identifier.</span></span> <span data-ttu-id="0200d-589">O tipo deve ser `System.Exception`, ou um tipo derivado de `System.Exception`.</span><span class="sxs-lookup"><span data-stu-id="0200d-589">The type must be `System.Exception`, or a type derived from `System.Exception`.</span></span> <span data-ttu-id="0200d-590">Quando uma exceção é detectada, o que é derivada de `System.Exception`, uma referência para o objeto de exceção é armazenada no objeto retornado pela função `Microsoft.VisualBasic.Information.Err`.</span><span class="sxs-lookup"><span data-stu-id="0200d-590">When an exception is caught that derives from `System.Exception`, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="0200d-591">Um `Catch` cláusula com um `When` cláusula somente capturar exceções quando a expressão é avaliada como `True`; o tipo da expressão deve ser uma expressão booliana de acordo com a seção [expressões Boolianas](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="0200d-591">A `Catch` clause with a `When` clause will only catch exceptions when the expression evaluates to `True`; the type of the expression must be a Boolean expression as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="0200d-592">Um `When` cláusula só será aplicada depois de verificar se o tipo da exceção e a expressão pode se referir ao identificador que representa a exceção, como demonstra este exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-592">A `When` clause is only applied after checking the type of the exception, and the expression may refer to the identifier representing the exception, as this example demonstrates:</span></span>

```vb
Module Test
    Sub Main()
        Dim i As Integer = 5

        Try
            Throw New ArgumentException()
        Catch e As OverflowException When i = 5
            Console.WriteLine("First handler")
        Catch e As ArgumentException When i = 4
            Console.WriteLine("Second handler")
        Catch When i = 5
            Console.WriteLine("Third handler")
        End Try

    End Sub
End Module
```

<span data-ttu-id="0200d-593">Este exemplo imprime:</span><span class="sxs-lookup"><span data-stu-id="0200d-593">This example prints:</span></span>

```
Third handler
```

<span data-ttu-id="0200d-594">Se um `Catch` cláusula manipula a exceção, a execução é transferida para o `Catch` bloco.</span><span class="sxs-lookup"><span data-stu-id="0200d-594">If a `Catch` clause handles the exception, execution transfers to the `Catch` block.</span></span> <span data-ttu-id="0200d-595">No final o `Catch` bloquear transferências de execução para a primeira instrução após a `Try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-595">At the end of the `Catch` block, execution transfers to the first statement following the `Try` statement.</span></span> <span data-ttu-id="0200d-596">O `Try` instrução não tratará as exceções geradas em um `Catch` bloco.</span><span class="sxs-lookup"><span data-stu-id="0200d-596">The `Try` statement will not handle any exceptions thrown in a `Catch` block.</span></span> <span data-ttu-id="0200d-597">Se nenhum `Catch` cláusula manipula a exceção, transferências de execução para um local determinado pelo sistema.</span><span class="sxs-lookup"><span data-stu-id="0200d-597">If no `Catch` clause handles the exception, execution transfers to a location determined by the system.</span></span>

<span data-ttu-id="0200d-598">Não é válido para transferir explicitamente a execução em um `Catch` bloco.</span><span class="sxs-lookup"><span data-stu-id="0200d-598">It is invalid to explicitly transfer execution into a `Catch` block.</span></span>

<span data-ttu-id="0200d-599">Os filtros no quando as cláusulas normalmente são avaliadas antes da exceção sendo lançada.</span><span class="sxs-lookup"><span data-stu-id="0200d-599">The filters in When clauses are normally evaluated prior to the exception being thrown.</span></span> <span data-ttu-id="0200d-600">Por exemplo, o seguinte código imprimirá "Filtro, por fim, Catch".</span><span class="sxs-lookup"><span data-stu-id="0200d-600">For instance, the following code will print "Filter, Finally, Catch".</span></span>

```vb
Sub Main()
   Try
       Foo()
   Catch ex As Exception When F()
       Console.WriteLine("Catch")
   End Try
End Sub

Sub Foo()
    Try
        Throw New Exception
    Finally
        Console.WriteLine("Finally")
    End Try
End Sub

Function F() As Boolean
    Console.WriteLine("Filter")
    Return True
End Function
```

 <span data-ttu-id="0200d-601">No entanto, os métodos assíncrono e iterador causam, por fim, todos os blocos dentro deles para ser executado antes de qualquer filtro de fora.</span><span class="sxs-lookup"><span data-stu-id="0200d-601">However, Async and Iterator methods cause all finally blocks inside them to be executed prior to any filters outside.</span></span> <span data-ttu-id="0200d-602">Por exemplo, se o código acima tivesse `Async Sub Foo()`, em seguida, a saída seria "Por fim, filtrar, Catch".</span><span class="sxs-lookup"><span data-stu-id="0200d-602">For instance, if the above code had `Async Sub Foo()`, then the output would be "Finally, Filter, Catch".</span></span>


#### <a name="throw-statement"></a><span data-ttu-id="0200d-603">Instrução Throw</span><span class="sxs-lookup"><span data-stu-id="0200d-603">Throw Statement</span></span>

<span data-ttu-id="0200d-604">O `Throw` instrução gera uma exceção, o que é representada por uma instância de um tipo derivado de `System.Exception`.</span><span class="sxs-lookup"><span data-stu-id="0200d-604">The `Throw` statement raises an exception, which is represented by an instance of a type derived from `System.Exception`.</span></span>

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

<span data-ttu-id="0200d-605">Se a expressão não é classificada como um valor ou não é um tipo derivado de `System.Exception`, em seguida, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-605">If the expression is not classified as a value or is not a type derived from `System.Exception`, then a compile-time error occurs.</span></span> <span data-ttu-id="0200d-606">Se a expressão for avaliada como um valor nulo em tempo de execução, em seguida, um `System.NullReferenceException` exceção é acionada em vez disso.</span><span class="sxs-lookup"><span data-stu-id="0200d-606">If the expression evaluates to a null value at run time, then a `System.NullReferenceException` exception is raised instead.</span></span>

<span data-ttu-id="0200d-607">Um `Throw` declaração pode omitir a expressão dentro de um bloco catch de uma `Try` instrução, desde que não há nenhuma intervenção bloco finally.</span><span class="sxs-lookup"><span data-stu-id="0200d-607">A `Throw` statement may omit the expression within a catch block of a `Try` statement, as long as there is no intervening finally block.</span></span> <span data-ttu-id="0200d-608">Nesse caso, a instrução Relança a exceção sendo manipulada no momento dentro do bloco catch.</span><span class="sxs-lookup"><span data-stu-id="0200d-608">In that case, the statement rethrows the exception currently being handled within the catch block.</span></span> <span data-ttu-id="0200d-609">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-609">For example:</span></span>

```vb
Sub Test(x As Integer)
    Try
        Throw New Exception()
    Catch
        If x = 0 Then
            Throw    ' OK, rethrows exception from above.
        Else
            Try
                If x = 1 Then
                    Throw   ' OK, rethrows exception from above.
                End If
            Finally
                Throw    ' Invalid, inside of a Finally.
            End Try
        End If
    End Try
End Sub
```


### <a name="unstructured-exception-handling-statements"></a><span data-ttu-id="0200d-610">Instruções de manipulação de exceção não estruturadas</span><span class="sxs-lookup"><span data-stu-id="0200d-610">Unstructured Exception-Handling Statements</span></span>

<span data-ttu-id="0200d-611">Tratamento de exceção não estruturados é um método de tratamento de erros, indicando as instruções para ramificar para quando ocorre uma exceção.</span><span class="sxs-lookup"><span data-stu-id="0200d-611">Unstructured exception handling is a method of handling errors by indicating statements to branch to when an exception occurs.</span></span> <span data-ttu-id="0200d-612">Tratamento de exceção não estruturados é implementado usando três instruções: o `Error` instrução, o `On Error` instrução e o `Resume` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-612">Unstructured exception handling is implemented using three statements: the `Error` statement, the `On Error` statement, and the `Resume` statement.</span></span>

```antlr
UnstructuredErrorStatement
    : ErrorStatement
    | OnErrorStatement
    | ResumeStatement
    ;
```

<span data-ttu-id="0200d-613">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-613">For example:</span></span>

```vb
Module Test
    Sub ThrowException()
        Error 5
    End Sub

    Sub Main()
        On Error GoTo GotException

        ThrowException()
        Exit Sub

GotException:
        Console.WriteLine("Caught exception!")
        Resume Next
    End Sub
End Module
```

<span data-ttu-id="0200d-614">Quando um método usa o tratamento de exceções não estruturados, um manipulador de exceção estruturada único é estabelecido para todo o método que captura todas as exceções.</span><span class="sxs-lookup"><span data-stu-id="0200d-614">When a method uses unstructured exception handling, a single structured exception handler is established for the entire method that catches all exceptions.</span></span> <span data-ttu-id="0200d-615">(Observe que em construtores esse manipulador não se estende sobre a chamada para a chamada para `New` no início do construtor.) O método, em seguida, mantém o controle do local mais recente do manipulador de exceção e a exceção mais recente que foi lançada.</span><span class="sxs-lookup"><span data-stu-id="0200d-615">(Note that in constructors this handler does not extend over the call to the call to `New` at the beginning of the constructor.) The method then keeps track of the most recent exception-handler location and the most recent exception that has been thrown.</span></span> <span data-ttu-id="0200d-616">Na entrada para o método, o local do manipulador de exceção e a exceção são definidas como `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="0200d-616">At entry to the method, the exception-handler location and the exception are both set to `Nothing`.</span></span> <span data-ttu-id="0200d-617">Quando uma exceção é gerada em um método que usa tratamento de exceções não estruturados, uma referência para o objeto de exceção é armazenada no objeto retornado pela função `Microsoft.VisualBasic.Information.Err`.</span><span class="sxs-lookup"><span data-stu-id="0200d-617">When an exception is thrown in a method that uses unstructured exception handling, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="0200d-618">Instruções de tratamento de erros não estruturado não são permitidas em métodos de iterador ou assíncrono.</span><span class="sxs-lookup"><span data-stu-id="0200d-618">Unstructured error handling statements are not allowed in iterator or async methods.</span></span>


#### <a name="error-statement"></a><span data-ttu-id="0200d-619">Instrução Error</span><span class="sxs-lookup"><span data-stu-id="0200d-619">Error Statement</span></span>

<span data-ttu-id="0200d-620">Uma `Error` instrução gera uma `System.Exception` exceção contendo um número de exceção do Visual Basic 6.</span><span class="sxs-lookup"><span data-stu-id="0200d-620">An `Error` statement throws a `System.Exception` exception containing a Visual Basic 6 exception number.</span></span> <span data-ttu-id="0200d-621">A expressão deve ser classificada como um valor e seu tipo deve ser implicitamente conversível para `Integer`.</span><span class="sxs-lookup"><span data-stu-id="0200d-621">The expression must be classified as a value and its type must be implicitly convertible to `Integer`.</span></span>

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a><span data-ttu-id="0200d-622">Instrução On Error</span><span class="sxs-lookup"><span data-stu-id="0200d-622">On Error Statement</span></span>

<span data-ttu-id="0200d-623">Um `On Error` instrução modifica o estado de manipulação de exceção mais recente.</span><span class="sxs-lookup"><span data-stu-id="0200d-623">An `On Error` statement modifies the most recent exception-handling state.</span></span>

```antlr
OnErrorStatement
    : 'On' 'Error' ErrorClause StatementTerminator
    ;

ErrorClause
    : 'GoTo' '-' '1'
    | 'GoTo' '0'
    | GoToStatement
    | 'Resume' 'Next'
    ;
```

<span data-ttu-id="0200d-624">Ele pode ser usado em uma das quatro maneiras:</span><span class="sxs-lookup"><span data-stu-id="0200d-624">It may be used in one of four ways:</span></span>

* <span data-ttu-id="0200d-625">`On Error GoTo -1` Redefine a exceção mais recente para `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="0200d-625">`On Error GoTo -1` resets the most recent exception to `Nothing`.</span></span>

* <span data-ttu-id="0200d-626">`On Error GoTo 0` Redefine o local mais recente do manipulador de exceção para `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="0200d-626">`On Error GoTo 0` resets the most recent exception-handler location to `Nothing`.</span></span>

* <span data-ttu-id="0200d-627">`On Error GoTo LabelName` estabelece o rótulo como o local mais recente do manipulador de exceção.</span><span class="sxs-lookup"><span data-stu-id="0200d-627">`On Error GoTo LabelName` establishes the label as the most recent exception-handler location.</span></span> <span data-ttu-id="0200d-628">Essa instrução não pode ser usada em um método que contém uma expressão lambda ou de consulta.</span><span class="sxs-lookup"><span data-stu-id="0200d-628">This statement cannot be used in a method that contains a lambda or query expression.</span></span>

* <span data-ttu-id="0200d-629">`On Error Resume Next` estabelece o `Resume Next` comportamento como o local mais recente do manipulador de exceção.</span><span class="sxs-lookup"><span data-stu-id="0200d-629">`On Error Resume Next` establishes the `Resume Next` behavior as the most recent exception-handler location.</span></span>


#### <a name="resume-statement"></a><span data-ttu-id="0200d-630">Instrução Resume</span><span class="sxs-lookup"><span data-stu-id="0200d-630">Resume Statement</span></span>

<span data-ttu-id="0200d-631">Um `Resume` instrução retorna a execução para a instrução que causou a exceção mais recente.</span><span class="sxs-lookup"><span data-stu-id="0200d-631">A `Resume` statement returns execution to the statement that caused the most recent exception.</span></span>

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

<span data-ttu-id="0200d-632">Se o `Next` modificador é especificado, a execução retorna para a instrução que deveriam ter sido executada após a instrução que causou a exceção mais recente.</span><span class="sxs-lookup"><span data-stu-id="0200d-632">If the `Next` modifier is specified, execution returns to the statement that would have been executed after the statement that caused the most recent exception.</span></span> <span data-ttu-id="0200d-633">Se um nome de rótulo for especificado, a execução retorna para o rótulo.</span><span class="sxs-lookup"><span data-stu-id="0200d-633">If a label name is specified, execution returns to the label.</span></span>

<span data-ttu-id="0200d-634">Porque o `SyncLock` instrução contém um bloco de tratamento de erros estruturado implícito, `Resume` e `Resume Next` ter comportamentos especiais para exceções que ocorrem no `SyncLock` instruções.</span><span class="sxs-lookup"><span data-stu-id="0200d-634">Because the `SyncLock` statement contains an implicit structured error-handling block, `Resume` and `Resume Next` have special behaviors for exceptions that occur in `SyncLock` statements.</span></span> <span data-ttu-id="0200d-635">`Resume` Retorna a execução para o início dos `SyncLock` instrução, enquanto `Resume Next` retorna a execução para a próxima instrução que segue o `SyncLock` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-635">`Resume` returns execution to the beginning of the `SyncLock` statement, while `Resume Next` returns execution to the next statement following the `SyncLock` statement.</span></span> <span data-ttu-id="0200d-636">Por exemplo, considere o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="0200d-636">For example, consider the following code:</span></span>

```vb
Class LockClass
End Class

Module Test
    Sub Main()
        Dim FirstTime As Boolean = True
        Dim Lock As LockClass = New LockClass()

        On Error GoTo Handler

        SyncLock Lock
            Console.WriteLine("Before exception")
            Throw New Exception()
            Console.WriteLine("After exception")
        End SyncLock

        Console.WriteLine("After SyncLock")
        Exit Sub

Handler:
        If FirstTime Then
            FirstTime = False
            Resume
        Else
            Resume Next
        End If
    End Sub
End Module
```

<span data-ttu-id="0200d-637">Ele imprime o resultado a seguir.</span><span class="sxs-lookup"><span data-stu-id="0200d-637">It prints the following result.</span></span>

```
Before exception
Before exception
After SyncLock
```

<span data-ttu-id="0200d-638">A primeira vez que o `SyncLock` instrução, `Resume` retorna a execução para o início do `SyncLock` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-638">The first time through the `SyncLock` statement, `Resume` returns execution to the beginning of the `SyncLock` statement.</span></span> <span data-ttu-id="0200d-639">Na segunda vez por meio de `SyncLock` instrução, `Resume Next` retorna a execução até o final do `SyncLock` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-639">The second time through the `SyncLock` statement, `Resume Next` returns execution to the end of the `SyncLock` statement.</span></span> <span data-ttu-id="0200d-640">`Resume` e `Resume Next` não são permitidas dentro de um `SyncLock` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-640">`Resume` and `Resume Next` are not allowed within a `SyncLock` statement.</span></span>

<span data-ttu-id="0200d-641">Em todos os casos, quando um `Resume` instrução é executada, a exceção mais recente é definida como `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="0200d-641">In all cases, when a `Resume` statement is executed, the most recent exception is set to `Nothing`.</span></span> <span data-ttu-id="0200d-642">Se um `Resume` instrução for executada com nenhuma exceção mais recente, a instrução gera uma `System.Exception` exceção que contém o número de erro do Visual Basic `20` (retomar sem erro).</span><span class="sxs-lookup"><span data-stu-id="0200d-642">If a `Resume` statement is executed with no most recent exception, the statement raises a `System.Exception` exception containing the Visual Basic error number `20` (Resume without error).</span></span>


## <a name="branch-statements"></a><span data-ttu-id="0200d-643">Instruções Branch</span><span class="sxs-lookup"><span data-stu-id="0200d-643">Branch Statements</span></span>

<span data-ttu-id="0200d-644">Instruções Branch modificam o fluxo de execução em um método.</span><span class="sxs-lookup"><span data-stu-id="0200d-644">Branch statements modify the flow of execution in a method.</span></span> <span data-ttu-id="0200d-645">Há seis instruções de ramificação:</span><span class="sxs-lookup"><span data-stu-id="0200d-645">There are six branch statements:</span></span>

1. <span data-ttu-id="0200d-646">Um `GoTo` instrução faz com que a execução transferir para o rótulo especificado no método.</span><span class="sxs-lookup"><span data-stu-id="0200d-646">A `GoTo` statement causes execution to transfer to the specified label in the method.</span></span> <span data-ttu-id="0200d-647">Não é permitido `GoTo` em um `Try`, `Using`, `SyncLock`, `With`, `For` ou `For Each` bloco, nem em qualquer bloco de loop se uma variável local desse bloco é capturada em um lambda ou expressão LINQ.</span><span class="sxs-lookup"><span data-stu-id="0200d-647">It is not allowed to `GoTo` into a `Try`, `Using`, `SyncLock`, `With`, `For` or `For Each` block, nor into any loop block if a local variable of that block is captured in a lambda or LINQ expression.</span></span>
2. <span data-ttu-id="0200d-648">Um `Exit` instrução transfere a execução para a próxima instrução após o término da instrução do bloco imediatamente contido do tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="0200d-648">An `Exit` statement transfers execution to the next statement after the end of the immediately containing block statement of the specified kind.</span></span> <span data-ttu-id="0200d-649">Se o bloco é o bloco de método, o fluxo de controle sai do método, conforme descrito na seção [fluxo de controle](statements.md#control-flow).</span><span class="sxs-lookup"><span data-stu-id="0200d-649">If the block is the method block, then control flow exits the method as described in Section [Control Flow](statements.md#control-flow).</span></span> <span data-ttu-id="0200d-650">Se o `Exit` instrução não está contida dentro do tipo de bloco especificado na instrução, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-650">If the `Exit` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
3. <span data-ttu-id="0200d-651">Um `Continue` instrução transfere a execução até o final da instrução de loop de bloco imediatamente contido do tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="0200d-651">A `Continue` statement transfers execution to the end of the immediately containing block loop statement of the specified kind.</span></span> <span data-ttu-id="0200d-652">Se o `Continue` instrução não está contida dentro do tipo de bloco especificado na instrução, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-652">If the `Continue` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
4. <span data-ttu-id="0200d-653">Um `Stop` instrução faz com que uma exceção do depurador ocorra.</span><span class="sxs-lookup"><span data-stu-id="0200d-653">A `Stop` statement causes a debugger exception to occur.</span></span>
5. <span data-ttu-id="0200d-654">Um `End` instrução encerra o programa.</span><span class="sxs-lookup"><span data-stu-id="0200d-654">An `End` statement terminates the program.</span></span> <span data-ttu-id="0200d-655">Os finalizadores são executados antes do desligamento, mas os blocos finally de qualquer execução atualmente `Try` instruções não são executadas.</span><span class="sxs-lookup"><span data-stu-id="0200d-655">Finalizers are run before shutdown, but the finally blocks of any currently executing `Try` statements are not executed.</span></span> <span data-ttu-id="0200d-656">Essa instrução não pode ser usada em programas que não são executáveis (por exemplo, DLLs).</span><span class="sxs-lookup"><span data-stu-id="0200d-656">This statement may not be used in programs that are not executable (for example, DLLs).</span></span>
6. <span data-ttu-id="0200d-657">Um `Return` instrução com nenhuma expressão é equivalente a um `Exit Sub` ou `Exit Function` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-657">A `Return` statement with no expression is equivalent to an `Exit Sub` or `Exit Function` statement.</span></span> <span data-ttu-id="0200d-658">Um `Return` instrução com uma expressão é permitida somente em um método regular que é uma função ou em um método assíncrono que é uma função com o tipo de retorno `Task(Of T)` para alguns `T`.</span><span class="sxs-lookup"><span data-stu-id="0200d-658">A `Return` statement with an expression is only allowed in a regular method that is a function, or in an async method that is a function with return type `Task(Of T)` for some `T`.</span></span> <span data-ttu-id="0200d-659">A expressão deve ser classificada como um valor que é implicitamente conversível para o *variável de retorno da função* (no caso de métodos regulares) ou o *variável de retorno de tarefa* (no caso de métodos assíncronos) .</span><span class="sxs-lookup"><span data-stu-id="0200d-659">The expression must be classified as a value which is implicitly convertible to the *function return variable* (in the case of regular methods) or to the *task return variable* (in the case of async methods).</span></span> <span data-ttu-id="0200d-660">Seu comportamento é avaliar sua expressão, em seguida, armazená-lo na variável de retorno, em seguida, execute implícito `Exit Function` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-660">Its behavior is to evaluate its expression, then store it in the return variable, then execute an implicit `Exit Function` statement.</span></span>

```antlr
BranchStatement
    : GoToStatement
    | ExitStatement
    | ContinueStatement
    | StopStatement
    | EndStatement
    | ReturnStatement
    ;

GoToStatement
    : 'GoTo' LabelName StatementTerminator
    ;

ExitStatement
    : 'Exit' ExitKind StatementTerminator
    ;

ExitKind
    : 'Do' | 'For' | 'While' | 'Select' | 'Sub' | 'Function' | 'Property' | 'Try'
    ;

ContinueStatement
    : 'Continue' ContinueKind StatementTerminator
    ;

ContinueKind
    : 'Do' | 'For' | 'While'
    ;

StopStatement
    : 'Stop' StatementTerminator
    ;

EndStatement
    : 'End' StatementTerminator
    ;

ReturnStatement
    : 'Return' Expression? StatementTerminator
    ;
```

## <a name="array-handling-statements"></a><span data-ttu-id="0200d-661">Instruções de tratamento de matriz</span><span class="sxs-lookup"><span data-stu-id="0200d-661">Array-Handling Statements</span></span>

<span data-ttu-id="0200d-662">Duas instruções simplificam o trabalho com matrizes: `ReDim` instruções e `Erase` instruções.</span><span class="sxs-lookup"><span data-stu-id="0200d-662">Two statements simplify working with arrays: `ReDim` statements and `Erase` statements.</span></span>

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a><span data-ttu-id="0200d-663">Instrução ReDim</span><span class="sxs-lookup"><span data-stu-id="0200d-663">ReDim Statement</span></span>

<span data-ttu-id="0200d-664">Um `ReDim` instrução instancia novas matrizes.</span><span class="sxs-lookup"><span data-stu-id="0200d-664">A `ReDim` statement instantiates new arrays.</span></span>

```antlr
RedimStatement
    : 'ReDim' 'Preserve'? RedimClauses StatementTerminator
    ;

RedimClauses
    : RedimClause ( Comma RedimClause )*
    ;

RedimClause
    : Expression ArraySizeInitializationModifier
    ;
```

<span data-ttu-id="0200d-665">Cada cláusula na instrução deve ser classificada como uma variável ou um acesso de propriedade cujo tipo é um tipo de matriz ou `Object`e ser seguido por uma lista de limites de matriz.</span><span class="sxs-lookup"><span data-stu-id="0200d-665">Each clause in the statement must be classified as a variable or a property access whose type is an array type or `Object`, and be followed by a list of array bounds.</span></span> <span data-ttu-id="0200d-666">O número de limites deve ser consistente com o tipo da variável; qualquer número de limites é permitido para `Object`.</span><span class="sxs-lookup"><span data-stu-id="0200d-666">The number of the bounds must be consistent with the type of the variable; any number of bounds is allowed for `Object`.</span></span> <span data-ttu-id="0200d-667">Em tempo de execução, uma matriz é instanciada para cada expressão da esquerda para a direita com os limites especificados e, em seguida, é atribuída à variável ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="0200d-667">At run time, an array is instantiated for each expression from left to right with the specified bounds and then assigned to the variable or property.</span></span> <span data-ttu-id="0200d-668">Se for o tipo de variável `Object`, o número de dimensões é o número de dimensões especificadas e o tipo de elemento da matriz é `Object`.</span><span class="sxs-lookup"><span data-stu-id="0200d-668">If the variable type is `Object`, the number of dimensions is the number of dimensions specified, and the array element type is `Object`.</span></span> <span data-ttu-id="0200d-669">Se o número determinado de dimensões é incompatível com a variável ou propriedade em tempo de execução ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0200d-669">If the given number of dimensions is incompatible with the variable or property at run time a compile-time error occurs.</span></span> <span data-ttu-id="0200d-670">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-670">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object
        Dim b() As Byte
        Dim i(,) As Integer

        ' The next two statements are equivalent.
        ReDim o(10,30)
        o = New Object(10, 30) {}

        ' The next two statements are equivalent.
        ReDim b(10)
        b = New Byte(10) {}

        ' Error: Incorrect number of dimensions.
        ReDim i(10, 30, 40)
    End Sub
End Module
```

<span data-ttu-id="0200d-671">Se o `Preserve` palavra-chave for especificado, em seguida, as expressões devem ser classificáveis como um valor e o novo tamanho para cada dimensão, exceto pelo mais à direita deve ser igual ao tamanho da matriz existente.</span><span class="sxs-lookup"><span data-stu-id="0200d-671">If the `Preserve` keyword is specified, then the expressions must also be classifiable as a value, and the new size for each dimension except for the rightmost one must be the same as the size of the existing array.</span></span> <span data-ttu-id="0200d-672">Os valores na matriz existente são copiados para a nova matriz: se a nova matriz for menor, os valores existentes são descartados; Se a nova matriz é maior, os elementos adicionais serão inicializados com o valor padrão do tipo de elemento da matriz.</span><span class="sxs-lookup"><span data-stu-id="0200d-672">The values in the existing array are copied into the new array: if the new array is smaller, the existing values are discarded; if the new array is bigger, the extra elements will be initialized to the default value of the element type of the array.</span></span> <span data-ttu-id="0200d-673">Por exemplo, considere o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="0200d-673">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5, 5) As Integer

        x(3, 3) = 3

        ReDim Preserve x(5, 6)
        Console.WriteLine(x(3, 3) & ", " & x(3, 6))
    End Sub
End Module
```

<span data-ttu-id="0200d-674">Imprime o seguinte resultado:</span><span class="sxs-lookup"><span data-stu-id="0200d-674">It prints the following result:</span></span>

```
3, 0
```

<span data-ttu-id="0200d-675">Se a referência de matriz existente for um valor nulo em tempo de execução, nenhum erro será mostrado.</span><span class="sxs-lookup"><span data-stu-id="0200d-675">If the existing array reference is a null value at run time, no error is given.</span></span> <span data-ttu-id="0200d-676">Diferente da dimensão mais à direita, se o tamanho de uma dimensão for alterada, um `System.ArrayTypeMismatchException` será lançada.</span><span class="sxs-lookup"><span data-stu-id="0200d-676">Other than the rightmost dimension, if the size of a dimension changes, a `System.ArrayTypeMismatchException` will be thrown.</span></span>

<span data-ttu-id="0200d-677">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="0200d-677">__Note.__</span></span> <span data-ttu-id="0200d-678">`Preserve` não é uma palavra reservada.</span><span class="sxs-lookup"><span data-stu-id="0200d-678">`Preserve` is not a reserved word.</span></span>


### <a name="erase-statement"></a><span data-ttu-id="0200d-679">Instrução Erase</span><span class="sxs-lookup"><span data-stu-id="0200d-679">Erase Statement</span></span>

<span data-ttu-id="0200d-680">Uma `Erase` instrução define cada uma das variáveis de matriz ou propriedades especificadas na instrução para `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="0200d-680">An `Erase` statement sets each of the array variables or properties specified in the statement to `Nothing`.</span></span> <span data-ttu-id="0200d-681">Cada expressão na instrução deve ser classificada como um acesso de propriedade ou variável cujo tipo é um tipo de matriz ou `Object`.</span><span class="sxs-lookup"><span data-stu-id="0200d-681">Each expression in the statement must be classified as a variable or property access whose type is an array type or `Object`.</span></span> <span data-ttu-id="0200d-682">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-682">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x() As Integer = New Integer(5) {}

        ' The following two statements are equivalent.
        Erase x
        x = Nothing
    End Sub
End Module
```

```antlr
EraseStatement
    : 'Erase' EraseExpressions StatementTerminator
    ;

EraseExpressions
    : Expression ( Comma Expression )*
    ;
```

## <a name="using-statement"></a><span data-ttu-id="0200d-683">instrução Using</span><span class="sxs-lookup"><span data-stu-id="0200d-683">Using statement</span></span>

<span data-ttu-id="0200d-684">Instâncias de tipos são liberadas automaticamente pelo coletor de lixo quando uma coleção é executada e nenhuma referência ao vivo para a instância é encontrada.</span><span class="sxs-lookup"><span data-stu-id="0200d-684">Instances of types are automatically released by the garbage collector when a collection is run and no live references to the instance are found.</span></span> <span data-ttu-id="0200d-685">Se um tipo for em um recurso particularmente valioso e escasso (como conexões de banco de dados ou identificadores de arquivos), ele não pode ser desejável esperar até a próxima coleta de lixo para limpar uma determinada instância do tipo que não está mais em uso.</span><span class="sxs-lookup"><span data-stu-id="0200d-685">If a type holds on a particularly valuable and scarce resource (such as database connections or file handles), it may not be desirable to wait until the next garbage collection to clean up a particular instance of the type that is no longer in use.</span></span> <span data-ttu-id="0200d-686">Para fornecer uma maneira simples de liberar recursos antes de uma coleção, um tipo pode implementar o `System.IDisposable` interface.</span><span class="sxs-lookup"><span data-stu-id="0200d-686">To provide a lightweight way of releasing resources before a collection, a type may implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="0200d-687">Um tipo que exponha um `Dispose` método que pode ser chamado para forçar a recursos valiosos para ser liberado imediatamente, assim:</span><span class="sxs-lookup"><span data-stu-id="0200d-687">A type that does so exposes a `Dispose` method that can be called to force valuable resources to be released immediately, as such:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As DBConnection = New DBConnection("...")

        ' Do some work
        ...

        x.Dispose()        ' Free the connection
    End Sub
End Module
```

<span data-ttu-id="0200d-688">O `Using` instrução automatiza o processo de aquisição de um recurso, executando um conjunto de instruções e, em seguida, descarte do recurso.</span><span class="sxs-lookup"><span data-stu-id="0200d-688">The `Using` statement automates the process of acquiring a resource, executing a set of statements, and then disposing of the resource.</span></span> <span data-ttu-id="0200d-689">A instrução pode ocorrer de duas formas: em um, o recurso é uma variável local declarada como parte da instrução e tratado como uma instrução de declaração de variável local regular; no outro, o recurso é o resultado de uma expressão.</span><span class="sxs-lookup"><span data-stu-id="0200d-689">The statement can take two forms: in one, the resource is a local variable declared as a part of the statement and treated as a regular local variable declaration statement; in the other, the resource is the result of an expression.</span></span>

```antlr
UsingStatement
    : 'Using' UsingResources StatementTerminator
      Block?
      'End' 'Using' StatementTerminator
    ;

UsingResources
    : VariableDeclarators
    | Expression
    ;
```

<span data-ttu-id="0200d-690">Se o recurso é uma instrução de declaração de variável local, o tipo de declaração de variável local deve ser um tipo que pode ser convertido implicitamente em `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="0200d-690">If the resource is a local variable declaration statement then the type of the local variable declaration must be a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="0200d-691">As variáveis locais declaradas são somente leitura, como escopo o `Using` instrução bloquear e deve incluir um inicializador.</span><span class="sxs-lookup"><span data-stu-id="0200d-691">The declared local variables are read-only, scoped to the `Using` statement block and must include an initializer.</span></span> <span data-ttu-id="0200d-692">Se o recurso é o resultado de uma expressão, a expressão deve ser classificada como um valor e deve ser de um tipo que pode ser convertido implicitamente em `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="0200d-692">If the resource is the result of an expression then the expression must be classified as a value and must be of a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="0200d-693">A expressão é avaliada apenas uma vez, no início da instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-693">The expression is only evaluated once, at the beginning of the statement.</span></span>

<span data-ttu-id="0200d-694">O `Using` bloco implicitamente contido por um `Try` instrução bloco finally chama o método `IDisposable.Dispose` no recurso.</span><span class="sxs-lookup"><span data-stu-id="0200d-694">The `Using` block is implicitly contained by a `Try` statement whose finally block calls the method `IDisposable.Dispose` on the resource.</span></span> <span data-ttu-id="0200d-695">Isso garante que o recurso é descartado, mesmo quando uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="0200d-695">This ensures the resource is disposed even when an exception is thrown.</span></span> <span data-ttu-id="0200d-696">Como resultado, não é válido ramificar em uma `Using` bloquear de fora do bloco e uma `Using` bloco é tratado como uma única instrução para os fins `Resume` e `Resume Next`.</span><span class="sxs-lookup"><span data-stu-id="0200d-696">As a result, it is invalid to branch into a `Using` block from outside of the block, and a `Using` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="0200d-697">Se o recurso estiver `Nothing`, em seguida, nenhuma chamada para `Dispose` é feita.</span><span class="sxs-lookup"><span data-stu-id="0200d-697">If the resource is `Nothing`, then no call to `Dispose` is made.</span></span> <span data-ttu-id="0200d-698">Portanto, o exemplo:</span><span class="sxs-lookup"><span data-stu-id="0200d-698">Thus, the example:</span></span>

```vb
Using f As C = New C()
    ...
End Using
```

<span data-ttu-id="0200d-699">equivale a:</span><span class="sxs-lookup"><span data-stu-id="0200d-699">is equivalent to:</span></span>

```vb
Dim f As C = New C()
Try
    ...
Finally
    If f IsNot Nothing Then
        f.Dispose()
    End If
End Try
```

<span data-ttu-id="0200d-700">Um `Using` instrução que tenha uma instrução de declaração de variável local possa adquirir vários recursos de cada vez, que é equivalente a aninhados `Using` instruções.</span><span class="sxs-lookup"><span data-stu-id="0200d-700">A `Using` statement that has a local variable declaration statement may acquire multiple resources at a time, which is equivalent to nested `Using` statements.</span></span>  <span data-ttu-id="0200d-701">Por exemplo, um `Using` instrução do formulário:</span><span class="sxs-lookup"><span data-stu-id="0200d-701">For example, a `Using` statement of the form:</span></span>

```vb
Using r1 As R = New R(), r2 As R = New R()
    r1.F()
    r2.F()
End Using
```

<span data-ttu-id="0200d-702">equivale a:</span><span class="sxs-lookup"><span data-stu-id="0200d-702">is equivalent to:</span></span>

```vb
Using r1 As R = New R()
    Using r2 As R = New R()
        r1.F()
        r2.F()
    End Using
End Using
```


## <a name="await-statement"></a><span data-ttu-id="0200d-703">Instrução await</span><span class="sxs-lookup"><span data-stu-id="0200d-703">Await Statement</span></span>

<span data-ttu-id="0200d-704">Uma instrução de espera tem a mesma sintaxe de uma expressão de operador de espera (seção [operador Await](expressions.md#await-operator)), só é permitido em métodos que também permitem que expressões await e tem o mesmo comportamento que uma expressão de operador de espera.</span><span class="sxs-lookup"><span data-stu-id="0200d-704">An await statement has the same syntax as an await operator expression (Section [Await Operator](expressions.md#await-operator)), is allowed only in methods that also allow await expressions, and has the same behavior as an await operator expression.</span></span>

<span data-ttu-id="0200d-705">No entanto, ele poderá ser classificado como um valor ou nulo.</span><span class="sxs-lookup"><span data-stu-id="0200d-705">However, it may be classified as either a value or void.</span></span> <span data-ttu-id="0200d-706">Qualquer valor resultante da avaliação da expressão de operador de espera será descartada.</span><span class="sxs-lookup"><span data-stu-id="0200d-706">Any value resulting from evaluation of the await operator expression is discarded.</span></span>

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a><span data-ttu-id="0200d-707">Instrução Yield</span><span class="sxs-lookup"><span data-stu-id="0200d-707">Yield Statement</span></span>

<span data-ttu-id="0200d-708">Instruções yield estão relacionadas aos métodos de iterador, que são descritos na seção [métodos de iterador](statements.md#iterator-methods).</span><span class="sxs-lookup"><span data-stu-id="0200d-708">Yield statements are related to iterator methods, which are described in Section [Iterator Methods](statements.md#iterator-methods).</span></span>

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

<span data-ttu-id="0200d-709">`Yield` é uma palavra reservada se a expressão de lambda ou método imediatamente delimitador no qual ele aparece tem um `Iterator` modificador e se o `Yield` aparece depois que `Iterator` modificador; é não reservado em outro lugar.</span><span class="sxs-lookup"><span data-stu-id="0200d-709">`Yield` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Iterator` modifier, and if the `Yield` appears after that `Iterator` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="0200d-710">Também é reservado em diretivas de pré-processador.</span><span class="sxs-lookup"><span data-stu-id="0200d-710">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="0200d-711">A instrução yield só é permitida no corpo de uma expressão lambda ou método em que ele é uma palavra reservada.</span><span class="sxs-lookup"><span data-stu-id="0200d-711">The yield statement is only allowed in the body of a method or lambda expression where it is a reserved word.</span></span> <span data-ttu-id="0200d-712">Dentro do método ou lambda imediatamente delimitador, a instrução yield pode não ocorrer dentro do corpo de uma `Catch` ou `Finally` bloquear nou dentro do corpo de um `SyncLock` instrução.</span><span class="sxs-lookup"><span data-stu-id="0200d-712">Within the immediately enclosing method or lambda, the yield statement may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement.</span></span>

<span data-ttu-id="0200d-713">A instrução yield leva a uma única expressão que deve ser classificado como um valor e cujo tipo é implicitamente conversível para o tipo dos *variável do iterador atual* (seção [métodos de iterador](statements.md#iterator-methods)) do seu método delimitador do iterador.</span><span class="sxs-lookup"><span data-stu-id="0200d-713">The yield statement takes a single expression which must be classified as a value and whose type is implicitly convertible to the type of the *iterator current variable* (Section [Iterator Methods](statements.md#iterator-methods)) of its enclosing iterator method.</span></span>

<span data-ttu-id="0200d-714">Fluxo de controle só atinge uma `Yield` instrução quando o `MoveNext` método é invocado em um objeto de iterador.</span><span class="sxs-lookup"><span data-stu-id="0200d-714">Control flow only ever reaches a `Yield` statement when the `MoveNext` method is invoked on an iterator object.</span></span> <span data-ttu-id="0200d-715">(Isso ocorre porque uma instância de método iterador só executa suas instruções devido à `MoveNext` ou `Dispose` métodos que está sendo chamados em um objeto de iterador; e o `Dispose` método só executará o código em `Finally` blocos, onde `Yield` não é permitido).</span><span class="sxs-lookup"><span data-stu-id="0200d-715">(This is because an iterator method instance only ever executes its statements due to the `MoveNext` or `Dispose` methods being called on an iterator object; and the `Dispose` method will only ever execute code in `Finally` blocks, where `Yield` is not allowed).</span></span>

<span data-ttu-id="0200d-716">Quando um `Yield` instrução for executada, sua expressão é avaliada e armazenada na *variável do iterador atual* da instância do método iterador associada a esse objeto de iterador.</span><span class="sxs-lookup"><span data-stu-id="0200d-716">When a `Yield` statement is executed, its expression is evaluated and stored in the *iterator current variable* of the iterator method instance associated with that iterator object.</span></span> <span data-ttu-id="0200d-717">O valor `True` é retornado ao chamador `MoveNext`, e o ponto de controle dessa instância é interrompida avançando até a próxima invocação de `MoveNext` no objeto de iterador.</span><span class="sxs-lookup"><span data-stu-id="0200d-717">The value `True` is returned to the invoker of `MoveNext`, and the control point of this instance stops advancing until the next invocation of `MoveNext` on the iterator object.</span></span>

