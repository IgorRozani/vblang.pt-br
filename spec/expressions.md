# <a name="expressions"></a><span data-ttu-id="ab3a1-101">Expressões</span><span class="sxs-lookup"><span data-stu-id="ab3a1-101">Expressions</span></span>

<span data-ttu-id="ab3a1-102">Uma expressão é uma sequência de operandos e operadores que especifica uma computação de um valor, ou que designa uma variável ou constante.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-102">An expression is a sequence of operators and operands that specifies a computation of a value, or that designates a variable or constant.</span></span> <span data-ttu-id="ab3a1-103">Este capítulo define a sintaxe, a ordem de avaliação dos operandos e operadores e o significado das expressões.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

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

## <a name="expression-classifications"></a><span data-ttu-id="ab3a1-104">Classificações de expressão</span><span class="sxs-lookup"><span data-stu-id="ab3a1-104">Expression Classifications</span></span>

<span data-ttu-id="ab3a1-105">Cada expressão é classificado como um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-105">Every expression is classified as one of the following:</span></span>

* <span data-ttu-id="ab3a1-106">*Um valor.*</span><span class="sxs-lookup"><span data-stu-id="ab3a1-106">*A value.*</span></span> <span data-ttu-id="ab3a1-107">Cada valor tem um tipo associado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-107">Every value has an associated type.</span></span>

* <span data-ttu-id="ab3a1-108">*Uma variável.*</span><span class="sxs-lookup"><span data-stu-id="ab3a1-108">*A variable.*</span></span> <span data-ttu-id="ab3a1-109">Cada variável tem um tipo associado, ou seja, o tipo declarado da variável.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-109">Every variable has an associated type, namely the declared type of the variable.</span></span>

* <span data-ttu-id="ab3a1-110">*Um namespace.*</span><span class="sxs-lookup"><span data-stu-id="ab3a1-110">*A namespace.*</span></span> <span data-ttu-id="ab3a1-111">Uma expressão com essa classificação só pode aparecer como o lado esquerdo de um acesso de membro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-111">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="ab3a1-112">Em qualquer outro contexto, uma expressão classificada como um namespace causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>

* <span data-ttu-id="ab3a1-113">*Um tipo.*</span><span class="sxs-lookup"><span data-stu-id="ab3a1-113">*A type.*</span></span> <span data-ttu-id="ab3a1-114">Uma expressão com essa classificação só pode aparecer como o lado esquerdo de um acesso de membro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-114">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="ab3a1-115">Em qualquer outro contexto, uma classificado como um tipo de expressão causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>

* <span data-ttu-id="ab3a1-116">*Um grupo de métodos,* que é um conjunto de métodos sobrecarregados no mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-116">*A method group,* which is a set of methods overloaded on the same name.</span></span> <span data-ttu-id="ab3a1-117">Um grupo de método pode ter uma expressão de destino associado e uma lista de argumentos de tipo associado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-117">A method group may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="ab3a1-118">*Um ponteiro de método,* que representa o local de um método.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-118">*A method pointer,* which represents the location of a method.</span></span> <span data-ttu-id="ab3a1-119">Um ponteiro de método pode ter uma expressão de destino associado e uma lista de argumentos de tipo associado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-119">A method pointer may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="ab3a1-120">*Um método lambda,* que é um método anônimo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-120">*A lambda method,* which is an anonymous method.</span></span>

* <span data-ttu-id="ab3a1-121">*Um grupo de propriedades,* que é um conjunto de propriedades sobrecarregado no mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-121">*A property group,* which is a set of properties overloaded on the same name.</span></span> <span data-ttu-id="ab3a1-122">Um grupo de propriedade pode ter uma expressão de destino associados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-122">A property group may have an associated target expression.</span></span>

* <span data-ttu-id="ab3a1-123">*Um acesso de propriedade.*</span><span class="sxs-lookup"><span data-stu-id="ab3a1-123">*A property access.*</span></span> <span data-ttu-id="ab3a1-124">Todo acesso de propriedade tem um tipo associado, ou seja, o tipo da propriedade.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-124">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="ab3a1-125">Um acesso de propriedade pode ter uma expressão de destino associados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-125">A property access may have an associated target expression.</span></span>

* <span data-ttu-id="ab3a1-126">*Um acesso de associação tardia,* que representa um método ou propriedade acesso adiado até o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-126">*A late-bound access,* which represents a method or property access deferred until run-time.</span></span> <span data-ttu-id="ab3a1-127">Um acesso de associação tardia pode ter uma expressão de destino associado e uma lista de argumentos de tipo associado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-127">A late-bound access may have an associated target expression and an associated type argument list.</span></span> <span data-ttu-id="ab3a1-128">O tipo de um acesso de associação tardia é sempre `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-128">The type of a late-bound access is always `Object`.</span></span>

* <span data-ttu-id="ab3a1-129">*Acesso de um evento.*</span><span class="sxs-lookup"><span data-stu-id="ab3a1-129">*An event access.*</span></span> <span data-ttu-id="ab3a1-130">Acesso de cada evento tem um tipo associado, ou seja, o tipo do evento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="ab3a1-131">Um acesso de evento pode ter uma expressão de destino associados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-131">An event access may have an associated target expression.</span></span> <span data-ttu-id="ab3a1-132">Um acesso de evento pode aparecer como o primeiro argumento do `RaiseEvent`, `AddHandler`, e `RemoveHandler` instruções.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-132">An event access may appear as the first argument of the `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements.</span></span> <span data-ttu-id="ab3a1-133">Em qualquer outro contexto, uma expressão classificada como um acesso de evento causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>

* <span data-ttu-id="ab3a1-134">*Uma matriz literal,* que representa os valores iniciais de uma matriz cujo tipo ainda não foi determinado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-134">*An array literal,* which represents the initial values of an array whose type has not yet been determined.</span></span>

* <span data-ttu-id="ab3a1-135">*Void.*</span><span class="sxs-lookup"><span data-stu-id="ab3a1-135">*Void.*</span></span> <span data-ttu-id="ab3a1-136">Isso ocorre quando a expressão é uma invocação de uma sub-rotina ou uma expressão de operador de espera nenhum resultado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-136">This occurs when the expression is an invocation of a subroutine, or an await operator expression with no result.</span></span> <span data-ttu-id="ab3a1-137">Uma expressão classificada como void só é válida no contexto de uma instrução de chamada ou uma instrução de espera.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-137">An expression classified as void is only valid in the context of an invocation statement or an await statement.</span></span>

* <span data-ttu-id="ab3a1-138">*Um valor padrão.*</span><span class="sxs-lookup"><span data-stu-id="ab3a1-138">*A default value.*</span></span> <span data-ttu-id="ab3a1-139">Somente o literal `Nothing` produz essa classificação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-139">Only the literal `Nothing` produces this classification.</span></span>

<span data-ttu-id="ab3a1-140">O resultado final de uma expressão geralmente é um valor ou uma variável, com as outras categorias de expressões que funcionam como valores intermediários são permitidos somente em determinados contextos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-140">The final result of an expression is usually a value or a variable, with the other categories of expressions functioning as intermediate values that are only permitted in certain contexts.</span></span>

<span data-ttu-id="ab3a1-141">Observe que as expressões cujo tipo é um parâmetro de tipo podem ser usadas em instruções e expressões que exigem o tipo de uma expressão para ter determinadas características (como sendo um tipo de referência, o tipo de valor, derivando de algum tipo, etc.) se as restrições impostas no parâmetro de tipo atender a essas características.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-141">Note that expressions whose type is a type parameter can be used in statements and expressions that require the type of an expression to have certain characteristics (such as being a reference type, value type, deriving from some type, etc.) if the constraints imposed on the type parameter satisfy those characteristics.</span></span>

### <a name="expression-reclassification"></a><span data-ttu-id="ab3a1-142">Reclassificação de expressão</span><span class="sxs-lookup"><span data-stu-id="ab3a1-142">Expression Reclassification</span></span>

<span data-ttu-id="ab3a1-143">Normalmente, quando uma expressão é usada em um contexto que requer uma classificação diferente da expressão, um erro de tempo de compilação ocorre – por exemplo, a tentativa de atribuir um valor para um literal.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-143">Normally, when an expression is used in a context that requires a classification different from that of the expression, a compile-time error occurs -- for example, attempting to assign a value to a literal.</span></span> <span data-ttu-id="ab3a1-144">No entanto, em muitos casos é possível alterar a classificação de uma expressão pelo processo de *reclassificação*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-144">However, in many cases it is possible to change an expression's classification through the process of *reclassification*.</span></span>

<span data-ttu-id="ab3a1-145">Se a reclassificação for bem-sucedida, a reclassificação é julgada como ampliação ou redução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-145">If reclassification succeeds, then the reclassification is judged as widening or narrowing.</span></span> <span data-ttu-id="ab3a1-146">A menos que indicado o contrário, todos os reclassifications nessa lista são de ampliação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-146">Unless otherwise noted, all the reclassifications in this list are widening.</span></span>

<span data-ttu-id="ab3a1-147">Os seguintes tipos de expressões podem ser reclassificados:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-147">The following types of expressions can be reclassified:</span></span>

* <span data-ttu-id="ab3a1-148">Uma variável pode ser reclassificada como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-148">A variable can be reclassified as a value.</span></span> <span data-ttu-id="ab3a1-149">O valor armazenado na variável é procurado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-149">The value stored in the variable is fetched.</span></span>

* <span data-ttu-id="ab3a1-150">Um grupo de método pode ser reclassificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-150">A method group can be reclassified as a value.</span></span> <span data-ttu-id="ab3a1-151">A expressão de grupo de método é interpretada como uma expressão de invocação com a expressão de destino associado e a lista de parâmetros de tipo e o parênteses vazios (ou seja, `f` é interpretado como `f()` e `f(Of Integer)` é interpretado como `f(Of Integer)()`).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-151">The method group expression is interpreted as an invocation expression with the associated target expression and type parameter list, and empty parentheses (that is, `f` is interpreted as `f()` and `f(Of Integer)` is interpreted as `f(Of Integer)()`).</span></span> <span data-ttu-id="ab3a1-152">Este reclassificação pode resultar na expressão que está sendo ainda mais reclassificado como nula.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-152">This reclassification may result in the expression being further reclassified as void.</span></span>

* <span data-ttu-id="ab3a1-153">Um ponteiro de método pode ser reclassificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-153">A method pointer can be reclassified as a value.</span></span> <span data-ttu-id="ab3a1-154">Este reclassificação só pode ocorrer no contexto de uma conversão em que o tipo de destino é conhecido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-154">This reclassification can only occur in the context of a conversion where the target type is known.</span></span> <span data-ttu-id="ab3a1-155">A expressão de ponteiro de método é interpretada como o argumento para uma expressão de instanciação de delegado do tipo apropriado com a lista de argumentos de tipo associado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-155">The method pointer expression is interpreted as the argument to a delegate instantiation expression of the appropriate type with the associated type argument list.</span></span> <span data-ttu-id="ab3a1-156">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-156">For example:</span></span>
    
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

* <span data-ttu-id="ab3a1-157">Um método lambda pode ser reclassificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-157">A lambda method can be reclassified as a value.</span></span> <span data-ttu-id="ab3a1-158">Se a reclassificação ocorre no contexto de uma conversão em que o tipo de destino é conhecido, um dos dois reclassifications pode ocorrer:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-158">If the reclassification occurs in the context of a conversion where the target type is known, then one of two reclassifications can occur:</span></span>
    
  1. <span data-ttu-id="ab3a1-159">Se o tipo de destino for um tipo de delegado, o método lambda é interpretado como o argumento para uma expressão de construção de delegado do tipo apropriado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-159">If the target type is a delegate type, the lambda method is interpreted as the argument to a delegate-construction expression of the appropriate type.</span></span>
    
  2. <span data-ttu-id="ab3a1-160">Se o tipo de destino for `System.Linq.Expressions.Expression(Of T)`, e `T` é um tipo de delegado, em seguida, o método lambda é interpretado como se ele estava sendo usado na expressão de delegado de construção para `T` e, em seguida, convertida em uma árvore de expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-160">If the target type is `System.Linq.Expressions.Expression(Of T)`, and `T` is a delegate type, then the lambda method is interpreted as if it was being used in delegate-construction expression for `T` and then converted to an expression tree.</span></span>
    
  <span data-ttu-id="ab3a1-161">Um método de lambda assíncrono ou iterador só pode ser interpretado como o argumento para uma expressão de delegado construção, se o delegado não tem nenhum parâmetro ByRef.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-161">An async or iterator lambda method may only be interpreted as the argument to a delegate-construction expression, if the delegate has no ByRef parameters.</span></span>
    
  <span data-ttu-id="ab3a1-162">Se a conversão de qualquer um dos tipos de parâmetro do representante para os tipos de parâmetro correspondente do lambda é uma conversão de redução, em seguida, reclassificação é considerada de estreitamento; Caso contrário, ele está ampliando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-162">If conversion from any of the delegate's parameter types to the corresponding lambda parameter types is a narrowing conversion, then the reclassification is judged as narrowing; otherwise it is widening.</span></span>
    
  <span data-ttu-id="ab3a1-163">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-163">__Note.__</span></span> <span data-ttu-id="ab3a1-164">A conversão exata entre os métodos de lambda e árvores de expressão não pode ser corrigida entre versões do compilador e está além do escopo desta especificação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-164">The exact translation between lambda methods and expression trees may not be fixed between versions of the compiler and is beyond the scope of this specification.</span></span> <span data-ttu-id="ab3a1-165">Para Microsoft Visual Basic 11.0, todas as expressões lambda podem ser convertidas para árvores de expressão de acordo com as seguintes restrições: (1) 1.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-165">For Microsoft Visual Basic 11.0, all lambda expressions may be converted to expression trees subject to the following restrictions: (1) 1.</span></span>  <span data-ttu-id="ab3a1-166">Somente as expressões lambda de linha única sem parâmetros ByRef podem ser convertidas em árvores de expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-166">Only single-line lambda expressions without ByRef parameters may be converted to expression trees.</span></span> <span data-ttu-id="ab3a1-167">Da linha única `Sub` lambdas, instruções de invocação somente podem ser convertidas para árvores de expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-167">Of the single-line `Sub` lambdas, only invocation statements may be converted to expression trees.</span></span> <span data-ttu-id="ab3a1-168">(2) expressões de tipo anônimo não podem ser convertidas para árvores de expressão, se um inicializador de campo anterior é usado para inicializar um inicializador de campo subsequentes, por exemplo, `New With {.a=1, .b=.a}`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-168">(2) Anonymous type expressions cannot be converted to expression trees if an earlier field initializer is used to initialize a subsequent field initializer, e.g. `New With {.a=1, .b=.a}`.</span></span> <span data-ttu-id="ab3a1-169">(3) expressões do inicializador de objeto não podem ser convertidas em árvores de expressão se um membro do objeto atual que está sendo inicializado é usado em um dos inicializadores de campo, por exemplo, `New C1 With {.a=1, .b=.Method1()}`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-169">(3) Object initializer expressions cannot be converted to expression trees if a member of the current object being initialized is used in one of the field initializers, e.g. `New C1 With {.a=1, .b=.Method1()}`.</span></span> <span data-ttu-id="ab3a1-170">(4) expressões de criação a matriz multidimensional só podem ser convertidas para árvores de expressão, se eles declararem explicitamente o seu tipo de elemento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-170">(4) Multi-dimensional array creation expressions can only be converted to expression trees if they declare their element type explicitly.</span></span> <span data-ttu-id="ab3a1-171">(5) Late expressões de associação de não podem ser convertidas em árvores de expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-171">(5) Late-binding expressions cannot be converted to expression trees.</span></span> <span data-ttu-id="ab3a1-172">(6) quando uma variável ou um campo é passado como ByRef como uma expressão de invocação, mas não tem exatamente o mesmo tipo que o parâmetro ByRef, ou quando uma propriedade é passada ByRef, a semântica normal do VB é que uma cópia do argumento é passada ByRef e seu valor final é então copiado  volta para a variável ou campo ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-172">(6) When a variable or field is passed ByRef to an invocation expression but does not have exactly the same type as the ByRef parameter, or when a property is passed ByRef, normal VB semantics are that a copy of the argument is passed ByRef and its final value is then copied back into the variable or field or property.</span></span> <span data-ttu-id="ab3a1-173">Em árvores de expressão, o retorno de cópia não acontece.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-173">In expression trees, the copy-back does not happen.</span></span> <span data-ttu-id="ab3a1-174">(7) todas as essas restrições se aplicam às expressões de lambda aninhada também.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-174">(7) All these restrictions apply to nested lambda expressions as well.</span></span>
    
  <span data-ttu-id="ab3a1-175">Se o tipo de destino não for conhecido, o método lambda é interpretado como o argumento para uma expressão de instanciação de delegado de um tipo de delegado anônimo com a mesma assinatura do método lambda.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-175">If the target type is not known, then the lambda method is interpreted as the argument to a delegate instantiation expression of an anonymous delegate type with the same signature of the lambda method.</span></span> <span data-ttu-id="ab3a1-176">Se a semântica estrita está sendo usada e o tipo de qualquer um dos parâmetros são omitidos, ocorre um erro de tempo de compilação; Caso contrário, `Object` é substituída para qualquer tipo de parâmetro ausentes.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-176">If strict semantics are being used and the type of any of the parameters are omitted, a compile-time error occurs; otherwise, `Object` is substituted for any missing parameter type.</span></span> <span data-ttu-id="ab3a1-177">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-177">For example:</span></span>
    
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

* <span data-ttu-id="ab3a1-178">Um grupo de propriedades pode ser reclassificado como um acesso de propriedade.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-178">A property group can be reclassified as a property access.</span></span> <span data-ttu-id="ab3a1-179">A expressão de grupo de propriedade é interpretada como uma expressão de índice com parênteses vazios (ou seja, `f` é interpretado como `f()`).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-179">The property group expression is interpreted as an index expression with empty parentheses (that is, `f` is interpreted as `f()`).</span></span>

* <span data-ttu-id="ab3a1-180">Um acesso de propriedade pode ser reclassificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-180">A property access can be reclassified as a value.</span></span> <span data-ttu-id="ab3a1-181">A expressão de acesso de propriedade é interpretada como uma expressão de invocação do `Get` acessador da propriedade.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-181">The property access expression is interpreted as an invocation expression of the `Get` accessor of the property.</span></span> <span data-ttu-id="ab3a1-182">Se a propriedade não tem getter, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-182">If the property has no getter, then a compile-time error occurs.</span></span>

* <span data-ttu-id="ab3a1-183">Um acesso de associação tardia pode ser reclassificado como um método de associação tardia ou acesso de propriedade de associação tardia.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-183">A late-bound access can be reclassified as a late-bound method or late-bound property access.</span></span> <span data-ttu-id="ab3a1-184">Em uma situação em que um acesso de associação tardia pode ser reclassificado como um método de acesso e um acesso de propriedade, é preferível a reclassificação para um acesso de propriedade.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-184">In a situation where a late-bound access can be reclassified both as a method access and as a property access, reclassification to a property access is preferred.</span></span>

* <span data-ttu-id="ab3a1-185">Um acesso de associação tardia pode ser reclassificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-185">A late-bound access can be reclassified as a value.</span></span>

* <span data-ttu-id="ab3a1-186">Um literal de matriz pode ser reclassificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-186">An array literal can be reclassified as a value.</span></span> <span data-ttu-id="ab3a1-187">O tipo do valor é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-187">The type of the value is determined as follows:</span></span>

  1. <span data-ttu-id="ab3a1-188">Se a reclassificação ocorre no contexto de uma conversão em que o tipo de destino é conhecido e o tipo de destino é um tipo de matriz, o literal de matriz é reclassificado como um valor do tipo T().</span><span class="sxs-lookup"><span data-stu-id="ab3a1-188">If the reclassification occurs in the context of a conversion where the target type is known and the target type is an array type, then the array literal is reclassified as a value of type T().</span></span> <span data-ttu-id="ab3a1-189">Se o tipo de destino for `System.Collections.Generic.IList(Of T)`, `IReadOnlyList(Of T)`, `ICollection(Of T)`, `IReadOnlyCollection(Of T)`, ou `IEnumerable(Of T)`e o literal de matriz tem um nível de aninhamento e, em seguida, o literal de matriz é reclassificado como um valor do tipo `T()`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-189">If the target type is `System.Collections.Generic.IList(Of T)`, `IReadOnlyList(Of T)`, `ICollection(Of T)`, `IReadOnlyCollection(Of T)`, or `IEnumerable(Of T)`, and the array literal has one level of nesting, then the array literal is reclassified as a value of type `T()`.</span></span>

  2. <span data-ttu-id="ab3a1-190">Caso contrário, o literal de matriz é reclassificado como um valor cujo tipo é que uma matriz de classificação igual ao nível de aninhamento é usada, com o tipo de elemento determinado pelo tipo dominante dos elementos no inicializador; Se nenhum tipo dominante puder ser determinado, `Object` é usado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-190">Otherwise, the array literal is reclassified to a value whose type is an array of rank equal to the level of nesting is used, with element type determined by the dominant type of the elements in the initializer; if no dominant type can be determined, `Object` is used.</span></span> <span data-ttu-id="ab3a1-191">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-191">For example:</span></span>

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

  <span data-ttu-id="ab3a1-192">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-192">__Note.__</span></span> <span data-ttu-id="ab3a1-193">Há uma pequena alteração no comportamento entre a versão 9.0 e versão 10.0 do idioma.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-193">There is a slight change in behavior between version 9.0 and version 10.0 of the language.</span></span> <span data-ttu-id="ab3a1-194">Anteriores à 10.0, inicializadores de elemento de matriz não afetou a inferência de tipo de variável local e agora eles fazem isso.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-194">Prior to 10.0, array element initializers did not affect local variable type inference and now they do.</span></span> <span data-ttu-id="ab3a1-195">Portanto, `Dim a() = { 1, 2, 3 }` seria ter inferido `Object()` como o tipo de `a` na versão 9.0 da linguagem e `Integer()` na versão 10.0.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-195">So `Dim a() = { 1, 2, 3 }` would have inferred `Object()` as the type of `a` in version 9.0 of the language and `Integer()` in version 10.0.</span></span>

  <span data-ttu-id="ab3a1-196">Em seguida, a reclassificação reinterpreta o literal de matriz como uma expressão de criação de matriz.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-196">The reclassification then reinterprets the array literal as an array-creation expression.</span></span> <span data-ttu-id="ab3a1-197">Portanto, os exemplos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-197">So the examples:</span></span>

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  <span data-ttu-id="ab3a1-198">são equivalentes a:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-198">are equivalent to:</span></span>

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  <span data-ttu-id="ab3a1-199">A reclassificação é julgada como redução se qualquer conversão de uma expressão de elemento para o tipo de elemento da matriz é restritiva; Caso contrário, ela é considerada de ampliação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-199">The reclassification is judged as narrowing if any conversion from an element expression to the array element type is narrowing; otherwise it is judged as widening.</span></span>

* <span data-ttu-id="ab3a1-200">O valor padrão `Nothing` pode ser reclassificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-200">The default value `Nothing` can be reclassified as a value.</span></span> <span data-ttu-id="ab3a1-201">Em um contexto no qual o tipo de destino é conhecido, o resultado é o valor padrão do tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-201">In a context where the target type is known, the result is the default value of the target type.</span></span> <span data-ttu-id="ab3a1-202">Em um contexto em que o tipo de destino não é conhecido, o resultado é um valor nulo do tipo `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-202">In a context where the target type is not known, the result is a null value of type `Object`.</span></span>

<span data-ttu-id="ab3a1-203">Uma expressão de namespace, expressão de tipo, expressão de acesso de evento ou expressão void não pode ser reclassificado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-203">A namespace expression, type expression, event access expression, or void expression cannot be reclassified.</span></span> <span data-ttu-id="ab3a1-204">Vários reclassifications podem ser feitas ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-204">Multiple reclassifications can be done at the same time.</span></span> <span data-ttu-id="ab3a1-205">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-205">For example:</span></span>

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

<span data-ttu-id="ab3a1-206">Nesse caso, a propriedade de expressão de grupo `P` é reclassificado pela primeira vez de um grupo de propriedades para um acesso de propriedade e, em seguida, reclassificado de um acesso de propriedade para um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-206">In this case, the property group expression `P` is first reclassified from a property group to a property access and then reclassified from a property access to a value.</span></span> <span data-ttu-id="ab3a1-207">O menor número de reclassifications é executado para alcançar uma classificação válida no contexto.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-207">The fewest number of reclassifications are performed to reach a valid classification in the context.</span></span>

## <a name="constant-expressions"></a><span data-ttu-id="ab3a1-208">Expressões constantes</span><span class="sxs-lookup"><span data-stu-id="ab3a1-208">Constant Expressions</span></span>

<span data-ttu-id="ab3a1-209">Um *expressão constante* é uma expressão cujo valor pode ser completamente avaliado em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-209">A *constant expression* is an expression whose value can be fully evaluated at compile time.</span></span>

```antlr
ConstantExpression
    : Expression
    ;
```

<span data-ttu-id="ab3a1-210">O tipo de uma expressão constante pode ser `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Char`, `Single`, `Double`, `Decimal`, `Date`, `Boolean`, `String`, `Object`, ou qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-210">The type of a constant expression can be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Char`, `Single`, `Double`, `Decimal`, `Date`, `Boolean`, `String`, `Object`, or any enumeration type.</span></span> <span data-ttu-id="ab3a1-211">As seguintes construções são permitidas em expressões constantes:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-211">The following constructs are permitted in constant expressions:</span></span>

* <span data-ttu-id="ab3a1-212">Literais (incluindo `Nothing`).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-212">Literals (including `Nothing`).</span></span>

* <span data-ttu-id="ab3a1-213">Referências aos membros de tipo de constante ou locais de constante.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-213">References to constant type members or constant locals.</span></span>

* <span data-ttu-id="ab3a1-214">Referências aos membros de tipos de enumeração.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-214">References to members of enumeration types.</span></span>

* <span data-ttu-id="ab3a1-215">Subexpressões entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-215">Parenthesized subexpressions.</span></span>

* <span data-ttu-id="ab3a1-216">Expressões de coerção, fornecido o tipo de destino é um dos tipos listados acima.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-216">Coercion expressions, provided the target type is one of the types listed above.</span></span> <span data-ttu-id="ab3a1-217">Coerções de e para `String` são uma exceção a essa regra e são permitidos apenas em valores nulos porque `String` conversões são sempre feitas na cultura atual do ambiente de execução em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-217">Coercions to and from `String` are an exception to this rule and are only allowed on null values because `String` conversions are always done in the current culture of the execution environment at run time.</span></span> <span data-ttu-id="ab3a1-218">Observe que as expressões de constante coerção só podem usar conversões intrínseco.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-218">Note that constant coercion expressions can only ever use intrinsic conversions.</span></span>

* <span data-ttu-id="ab3a1-219">O `+`, `-` e `Not` operadores unários, fornecido o operando e o resultado é de um tipo listado acima.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-219">The `+`, `-` and `Not` unary operators, provided the operand and result is of a type listed above.</span></span>

* <span data-ttu-id="ab3a1-220">O `+`, `-`, `*`, `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, `And`, `Or`, `Xor`, `AndAlso`, `OrElse`, `=`, `<`, `>`, `<>`, `<=`, e `=>` fornecido de operadores binários, cada operando e o resultado é de um tipo listado acima.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-220">The `+`, `-`, `*`, `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, `And`, `Or`, `Xor`, `AndAlso`, `OrElse`, `=`, `<`, `>`, `<>`, `<=`, and `=>` binary operators, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="ab3a1-221">O operador condicional se, desde que cada operando e um resultado seja de um tipo listado acima.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-221">The conditional operator If, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="ab3a1-222">As seguintes funções de tempo de execução: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr` se o valor da constante estiver entre 0 e 128; `Microsoft.VisualBasic.Strings.AscW` se a cadeia de caracteres constante não estiver vazia; `Microsoft.VisualBasic.Strings.Asc` se a cadeia de caracteres constante não estiver vazia.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-222">The following run-time functions: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr` if the constant value is between 0 and 128; `Microsoft.VisualBasic.Strings.AscW` if the constant string is not empty; `Microsoft.VisualBasic.Strings.Asc` if the constant string is not empty.</span></span>

<span data-ttu-id="ab3a1-223">São as seguintes construções *não* permitidos em expressões constantes:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-223">The following constructs are *not* permitted in constant expressions:</span></span>

* <span data-ttu-id="ab3a1-224">A associação implícita por meio de um `With` contexto.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-224">Implicit binding through a `With` context.</span></span>

<span data-ttu-id="ab3a1-225">Expressões constantes do tipo integral (`ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `SByte`, ou `Byte`) pode ser convertido implicitamente em um tipo integral mais estreito, e expressões constantes do tipo `Double` pode ser convertido implicitamente em `Single`, desde que o valor da expressão constante é dentro do intervalo de tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-225">Constant expressions of an integral type (`ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `SByte`, or `Byte`) can be implicitly converted to a narrower integral type, and constant expressions of type `Double` can be implicitly converted to `Single`, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="ab3a1-226">Essas conversões de redução são permitidas, independentemente se a semântica estrita ou permissiva está sendo usada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-226">These narrowing conversions are allowed regardless of whether permissive or strict semantics are being used.</span></span>


## <a name="late-bound-expressions"></a><span data-ttu-id="ab3a1-227">Expressões de associação tardia</span><span class="sxs-lookup"><span data-stu-id="ab3a1-227">Late-Bound Expressions</span></span>

<span data-ttu-id="ab3a1-228">Quando o destino de uma expressão de acesso de membro ou uma expressão de índice é do tipo `Object`, o processamento da expressão pode ser adiado até o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-228">When the target of a member access expression or index expression is of type `Object`, the processing of the expression may be deferred until run time.</span></span> <span data-ttu-id="ab3a1-229">Adiar o processamento dessa maneira é chamado *ligação tardia*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-229">Deferring processing this way is called *late binding*.</span></span> <span data-ttu-id="ab3a1-230">Permite a associação tardia `Object` variáveis a serem usadas em um *sem especificação de tipo* forma, onde todas as resoluções de membros se baseia no tipo de tempo de execução real do valor na variável.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-230">Late binding allows `Object` variables to be used in a *typeless* way, where all resolution of members is based on the actual run-time type of the value in the variable.</span></span> <span data-ttu-id="ab3a1-231">Se a semântica estrita é especificada pelo ambiente de compilação ou por `Option Strict`, associação tardia causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-231">If strict semantics are specified by the compilation environment or by `Option Strict`, late binding causes a compile-time error.</span></span> <span data-ttu-id="ab3a1-232">Membros não públicos são ignorados ao fazer a ligação tardia, incluindo para fins de resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-232">Non-public members are ignored when doing late-binding, including for the purposes of overload resolution.</span></span> <span data-ttu-id="ab3a1-233">Observe que, ao contrário do caso de associação inicial, invocando ou acessar um `Shared` membro tardia fará com que o destino de invocação a ser avaliada em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-233">Note that, unlike the early-bound case, invoking or accessing a `Shared` member late-bound will cause the invocation target to be evaluated at run time.</span></span> <span data-ttu-id="ab3a1-234">Se a expressão for uma expressão de invocação de um membro definido em `System.Object`, associação tardia não ocorrerá.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-234">If the expression is an invocation expression for a member defined on `System.Object`, late binding will not take place.</span></span>

<span data-ttu-id="ab3a1-235">Em geral, os acessos de associação tardia são resolvidos em tempo de execução pesquisando o identificador no tipo de tempo de execução real da expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-235">In general, late-bound accesses are resolved at run time by looking up the identifier on the actual run-time type of the expression.</span></span> <span data-ttu-id="ab3a1-236">Se a pesquisa de membro de associação tardia falhar em tempo de execução, um `System.MissingMemberException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-236">If late-bound member lookup fails at run time, a `System.MissingMemberException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-237">Como pesquisa de membro de associação tardia é feita apenas desativar o tipo de tempo de execução da expressão de destino associado, o tipo de tempo de execução de um objeto nunca é uma interface.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-237">Because late-bound member lookup is done solely off the run-time type of the associated target expression, an object's run-time type is never an interface.</span></span> <span data-ttu-id="ab3a1-238">Portanto, é impossível acessar membros de interface em uma expressão de acesso de membro de associação tardia.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-238">Therefore, it is impossible to access interface members in a late-bound member access expression.</span></span>

<span data-ttu-id="ab3a1-239">Os argumentos para um acesso de membro de associação tardia são avaliados na ordem em que eles aparecem na expressão de acesso de membro: não a ordem na qual os parâmetros são declarados no membro de associação tardia.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-239">The arguments to a late-bound member access are evaluated in the order they appear in the member access expression: not the order in which parameters are declared in the late-bound member.</span></span> <span data-ttu-id="ab3a1-240">O exemplo a seguir ilustra essa diferença:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-240">The following example illustrates this difference:</span></span>

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

<span data-ttu-id="ab3a1-241">Esse código exibe:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-241">This code displays:</span></span>

```
Early-bound: xy
Late-bound: yx
```

<span data-ttu-id="ab3a1-242">Como a resolução de sobrecarga com associação tardia é feita no tipo dos argumentos de tempo de execução, é possível que uma expressão pode produzir resultados diferentes com base em se ele será avaliado no tempo de execução ou tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-242">Because late-bound overload resolution is done on the run-time type of the arguments, it is possible that an expression might produce different results based on whether it is evaluated at compile time or run time.</span></span> <span data-ttu-id="ab3a1-243">O exemplo a seguir ilustra essa diferença:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-243">The following example illustrates this difference:</span></span>

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

<span data-ttu-id="ab3a1-244">Esse código exibe:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-244">This code displays:</span></span>

```
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a><span data-ttu-id="ab3a1-245">Expressões simples</span><span class="sxs-lookup"><span data-stu-id="ab3a1-245">Simple Expressions</span></span>

<span data-ttu-id="ab3a1-246">Expressões simples são literais, expressões entre parênteses, expressões de instância ou expressões de nome simples.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-246">Simple expressions are literals, parenthesized expressions, instance expressions, or simple name expressions.</span></span>

```antlr
SimpleExpression
    : LiteralExpression
    | ParenthesizedExpression
    | InstanceExpression
    | SimpleNameExpression
    | AddressOfExpression
    ;
```

### <a name="literal-expressions"></a><span data-ttu-id="ab3a1-247">Expressões literais</span><span class="sxs-lookup"><span data-stu-id="ab3a1-247">Literal Expressions</span></span>

<span data-ttu-id="ab3a1-248">Avaliam expressões literais para o valor representado pelo literal.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-248">Literal expressions evaluate to the value represented by the literal.</span></span> <span data-ttu-id="ab3a1-249">Uma expressão literal é classificada como um valor, exceto para o literal `Nothing`, que é classificado como um valor padrão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-249">A literal expression is classified as a value, except for the literal `Nothing`, which is classified as a default value.</span></span>

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a><span data-ttu-id="ab3a1-250">Expressões entre parênteses</span><span class="sxs-lookup"><span data-stu-id="ab3a1-250">Parenthesized Expressions</span></span>

<span data-ttu-id="ab3a1-251">Uma expressão entre parênteses consiste em uma expressão entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-251">A parenthesized expression consists of an expression enclosed in parentheses.</span></span> <span data-ttu-id="ab3a1-252">Uma expressão entre parênteses é classificada como um valor e a expressão anexada deve ser classificada como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-252">A parenthesized expression is classified as a value, and the enclosed expression must be classified as a value.</span></span> <span data-ttu-id="ab3a1-253">Uma expressão entre parênteses é avaliada como o valor da expressão entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-253">A parenthesized expression evaluates to the value of the expression within the parentheses.</span></span>

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a><span data-ttu-id="ab3a1-254">Expressões de instância</span><span class="sxs-lookup"><span data-stu-id="ab3a1-254">Instance Expressions</span></span>

<span data-ttu-id="ab3a1-255">Uma *instância expressão* é a palavra-chave `Me`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-255">An *instance expression* is the keyword `Me`.</span></span> <span data-ttu-id="ab3a1-256">Ele só pode ser usado dentro do corpo de um acessador de propriedade, construtor ou método não compartilhado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-256">It may only be used within the body of a non-shared method, constructor, or property accessor.</span></span> <span data-ttu-id="ab3a1-257">Ele é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-257">It is classified as a value.</span></span> <span data-ttu-id="ab3a1-258">A palavra-chave `Me` representa a instância do tipo que contém o acessador de método ou propriedade que está sendo executado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-258">The keyword `Me` represents the instance of the type containing the method or property accessor being executed.</span></span> <span data-ttu-id="ab3a1-259">Se um construtor chama explicitamente o outro construtor (seção [construtores](type-members.md#constructors)), `Me` não pode ser usado até que essa chamada de construtor, porque a instância ainda não foi construída.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-259">If a constructor explicitly invokes another constructor (Section [Constructors](type-members.md#constructors)), `Me` cannot be used until after that constructor call, because the instance has not yet been constructed.</span></span>

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a><span data-ttu-id="ab3a1-260">Expressões de nome simples</span><span class="sxs-lookup"><span data-stu-id="ab3a1-260">Simple Name Expressions</span></span>

<span data-ttu-id="ab3a1-261">Um *expressão de nome simples* consiste em um único identificador seguido por uma lista de argumentos de tipo opcionais.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-261">A *simple name expression* consists of a single identifier followed by an optional type argument list.</span></span>

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

<span data-ttu-id="ab3a1-262">O nome é resolvido e classificado por "nome resolução regras simples":</span><span class="sxs-lookup"><span data-stu-id="ab3a1-262">The name is resolved and classified by the following "simple name resolution rules":</span></span>

1.  <span data-ttu-id="ab3a1-263">Começando com imediatamente delimitador bloquear e continuando com cada delimitador bloco externo (se houver), se o identificador corresponder ao nome de uma variável local, a variável estática, a constante local, método de parâmetro ou parâmetro de tipo, em seguida, o identificador se refere à Entidade correspondente.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-263">Starting with the immediately enclosing block and continuing with each enclosing outer block (if any), if the identifier matches the name of a local variable, static variable, constant local, method type parameter, or parameter, then the identifier refers to the matching entity.</span></span>

    <span data-ttu-id="ab3a1-264">Se o identificador corresponde a uma variável local, uma variável estática ou uma constante local e uma lista de argumentos de tipo foi fornecida, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-264">If the identifier matches a local variable, static variable, or constant local and a type argument list was provided, a compile-time error occurs.</span></span> <span data-ttu-id="ab3a1-265">Se o identificador corresponde a um parâmetro de tipo de método e uma lista de argumentos de tipo foi fornecida, nenhuma correspondência ocorrer e continua a resolução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-265">If the identifier matches a method type parameter and a type argument list was provided, no match occurs and resolution continues.</span></span> <span data-ttu-id="ab3a1-266">Se o identificador corresponder a uma variável local, a variável local correspondida é a função implícita ou `Get` variável local de retorno do acessador e a expressão fizer parte de uma expressão de invocação, instrução de chamada, ou um `AddressOf` expressão, em seguida, Nenhuma correspondência ocorrer e continua de resolução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-266">If the identifier matches a local variable, the local variable matched is the implicit function or `Get` accessor return local variable, and the expression is part of an invocation expression, invocation statement, or an `AddressOf` expression, then no match occurs and resolution continues.</span></span>

    <span data-ttu-id="ab3a1-267">A expressão é classificada como uma variável, se for uma variável local, uma variável estática ou um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-267">The expression is classified as a variable if it is a local variable, static variable, or parameter.</span></span> <span data-ttu-id="ab3a1-268">A expressão é classificada como um tipo se ele for um parâmetro de tipo de método.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-268">The expression is classified as a type if it is a method type parameter.</span></span> <span data-ttu-id="ab3a1-269">A expressão é classificada como um valor se for um local constante.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-269">The expression is classified as a value if it is a constant local.</span></span>

2.  <span data-ttu-id="ab3a1-270">Para cada tipo aninhado que contém a expressão, desde o mais interno e vai mais externo, se uma pesquisa do identificador no tipo gera uma correspondência com um membro acessível:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-270">For each nested type containing the expression, starting from the innermost and going to the outermost, if a lookup of the identifier in the type produces a match with an accessible member:</span></span>

    21. <span data-ttu-id="ab3a1-271">Se o membro de tipo correspondente for um parâmetro de tipo, o resultado é classificado como um tipo e é o parâmetro de tipo correspondente.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-271">If the matching type member is a type parameter, then the result is classified as a type and is the matching type parameter.</span></span> <span data-ttu-id="ab3a1-272">Se uma lista de argumentos de tipo foi fornecida, nenhuma correspondência ocorrer e continua de resolução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-272">If a type argument list was provided, no match occurs and resolution continues.</span></span>
    22. <span data-ttu-id="ab3a1-273">Caso contrário, se o tipo é o tipo delimitador imediatamente e a pesquisa identifica um membro de tipo não compartilhada, em seguida, o resultado é o mesmo que um acesso de membro do formulário `Me.E(Of A)`, onde `E` é o identificador e `A` é a lista de argumentos de tipo , se houver.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-273">Otherwise, if the type is the immediately enclosing type and the lookup identifies a non-shared type member, then the result is the same as a member access of the form `Me.E(Of A)`, where `E` is the identifier and `A` is the type argument list, if any.</span></span>
    23. <span data-ttu-id="ab3a1-274">Caso contrário, o resultado é exatamente o mesmo que um acesso de membro do formulário `T.E(Of A)`, onde `T` é o tipo que contém o membro correspondente, `E` é o identificador e `A` é a lista de argumentos de tipo, se houver.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-274">Otherwise, the result is exactly the same as a member access of the form `T.E(Of A)`, where `T` is the type containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="ab3a1-275">Nesse caso, ele é um erro para o identificador para se referir a um membro não compartilhado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-275">In this case, it is an error for the identifier to refer to a non-shared member.</span></span>

3.  <span data-ttu-id="ab3a1-276">Para cada namespace aninhado, começando em mais interna e vai para o namespace mais externo, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-276">For each nested namespace, starting from the innermost and going to the outermost namespace, do the following:</span></span>

    31. <span data-ttu-id="ab3a1-277">Se o namespace contém um tipo acessível com o nome fornecido e tem o mesmo número de parâmetros de tipo, conforme foi fornecido na lista de argumentos de tipo, se houver, em seguida, o identificador se refere a esse tipo e é classificado como um tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-277">If the namespace contains an accessible type with the given name and has the same number of type parameters as was supplied in the type argument list, if any, then the identifier refers to that type and is classified as a type.</span></span>
    32. <span data-ttu-id="ab3a1-278">Caso contrário, se nenhum tipo de lista de argumentos foi fornecido e o namespace contém um membro de namespace com o nome fornecido, em seguida, o identificador refere-se ao namespace e é classificado como um namespace.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-278">Otherwise, if no type argument list was supplied and the namespace contains a namespace member with the given name, then the identifier refers to that namespace and is classified as a namespace.</span></span>
    33. <span data-ttu-id="ab3a1-279">Caso contrário, se o namespace contém um ou mais módulos padrão acessíveis e uma pesquisa de nome do identificador de membro produz uma correspondência acessível em exatamente um módulo padrão, em seguida, o resultado é exatamente o mesmo que um acesso de membro do formulário `M.E(Of A)`, em que `M` é o módulo padrão que contém o membro correspondente, `E` é o identificador e `A` é a lista de argumentos de tipo, se houver.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-279">Otherwise, if the namespace contains one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="ab3a1-280">Se o identificador corresponder a membros de tipo acessível em mais de um módulo padrão, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-280">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

4.  <span data-ttu-id="ab3a1-281">Se o arquivo de origem tem um ou mais aliases de importação, e o identificador corresponde ao nome de um deles, o identificador se refere a esse namespace ou tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-281">If the source file has one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="ab3a1-282">Se uma lista de argumentos de tipo for fornecida, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-282">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="ab3a1-283">Se o arquivo de origem que contém a referência de nome tem um ou mais importações:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-283">If the source file containing the name reference has one or more imports:</span></span>

    51. <span data-ttu-id="ab3a1-284">Se o identificador corresponder exatamente em um importe o nome de um tipo acessível com o mesmo número de parâmetros de tipo, conforme foi fornecido na lista de argumentos de tipo, se houver, ou um membro de tipo, em seguida, o identificador se refere a esse tipo ou membro de tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-284">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="ab3a1-285">Se o identificador corresponde em mais de uma importação ao nome de um tipo acessível com o mesmo número de parâmetros de tipo foi fornecido na lista de argumentos de tipo, se houver, ou um membro de tipo acessível, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-285">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or an accessible type member, a compile-time error occurs.</span></span>
    52. <span data-ttu-id="ab3a1-286">Caso contrário, se nenhum tipo de lista de argumentos foi fornecido e o identificador corresponde na importação de exatamente um nome de um namespace com tipos acessíveis, em seguida, o identificador se refere esse namespace.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-286">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="ab3a1-287">Se nenhum tipo de lista de argumentos foi fornecido e o identificador corresponde em mais de uma importação, o nome de um namespace com tipos acessíveis, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-287">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    53. <span data-ttu-id="ab3a1-288">Caso contrário, se as importações contêm um ou mais módulos padrão acessíveis e uma pesquisa de nome do identificador de membro produz uma correspondência acessível em exatamente um módulo padrão, em seguida, o resultado é exatamente o mesmo que um acesso de membro do formulário `M.E(Of A)`, onde `M` é o módulo padrão que contém o membro correspondente, `E` é o identificador e `A` é a lista de argumentos de tipo, se houver.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-288">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="ab3a1-289">Se o identificador corresponder a membros de tipo acessível em mais de um módulo padrão, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-289">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

6.  <span data-ttu-id="ab3a1-290">Se o ambiente de compilação define um ou mais aliases de importação, e o identificador corresponde ao nome de um deles, o identificador se refere a esse namespace ou tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-290">If the compilation environment defines one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="ab3a1-291">Se uma lista de argumentos de tipo for fornecida, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-291">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="ab3a1-292">Se o ambiente de compilação define uma ou mais importações:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-292">If the compilation environment defines one or more imports:</span></span>

    71. <span data-ttu-id="ab3a1-293">Se o identificador corresponder exatamente em um importe o nome de um tipo acessível com o mesmo número de parâmetros de tipo, conforme foi fornecido na lista de argumentos de tipo, se houver, ou um membro de tipo, em seguida, o identificador se refere a esse tipo ou membro de tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-293">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="ab3a1-294">Se o identificador corresponde em mais de uma importação ao nome de um tipo acessível com o mesmo número de parâmetros de tipo foi fornecido na lista de argumentos de tipo, se houver, ou um membro de tipo, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-294">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, a compile-time error occurs.</span></span>
    72. <span data-ttu-id="ab3a1-295">Caso contrário, se nenhum tipo de lista de argumentos foi fornecido e o identificador corresponde na importação de exatamente um nome de um namespace com tipos acessíveis, em seguida, o identificador se refere esse namespace.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-295">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="ab3a1-296">Se nenhum tipo de lista de argumentos foi fornecido e o identificador corresponde em mais de uma importação, o nome de um namespace com tipos acessíveis, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-296">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    73. <span data-ttu-id="ab3a1-297">Caso contrário, se as importações contêm um ou mais módulos padrão acessíveis e uma pesquisa de nome do identificador de membro produz uma correspondência acessível em exatamente um módulo padrão, em seguida, o resultado é exatamente o mesmo que um acesso de membro do formulário `M.E(Of A)`, onde `M` é o módulo padrão que contém o membro correspondente, `E` é o identificador e `A` é a lista de argumentos de tipo, se houver.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-297">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member,  `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="ab3a1-298">Se o identificador corresponder a membros de tipo acessível em mais de um módulo padrão, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-298">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="ab3a1-299">Caso contrário, o nome fornecido pelo identificador é indefinido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-299">Otherwise, the name given by the identifier is undefined.</span></span>

<span data-ttu-id="ab3a1-300">Uma expressão de nome simples que não está definida é um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-300">A simple name expression that is undefined is a compile-time error.</span></span>

<span data-ttu-id="ab3a1-301">Normalmente, um nome pode ocorrer apenas uma vez em um namespace específico.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-301">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="ab3a1-302">No entanto, como namespaces podem ser declarados em vários assemblies do .NET, é possível ter uma situação em que dois assemblies definem um tipo com o mesmo nome totalmente qualificado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-302">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="ab3a1-303">Nesse caso, um tipo declarado no conjunto atual de arquivos de origem é preferível ao longo de um tipo declarado em um assembly .NET externo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-303">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="ab3a1-304">Caso contrário, o nome é ambíguo e não há nenhuma maneira de resolver a ambiguidade do nome.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-304">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>


### <a name="addressof-expressions"></a><span data-ttu-id="ab3a1-305">Expressões AddressOf</span><span class="sxs-lookup"><span data-stu-id="ab3a1-305">AddressOf Expressions</span></span>

<span data-ttu-id="ab3a1-306">Um `AddressOf` expressão é usada para produzir um ponteiro de método.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-306">An `AddressOf` expression is used to produce a method pointer.</span></span> <span data-ttu-id="ab3a1-307">A expressão consiste o `AddressOf` palavra-chave e uma expressão que deve ser classificada como um grupo de método ou um acesso de associação tardia.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-307">The expression consists of the `AddressOf` keyword and an expression that must be classified as a method group or a late-bound access.</span></span> <span data-ttu-id="ab3a1-308">O grupo de método não pode se referir a construtores.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-308">The method group cannot refer to constructors.</span></span>

<span data-ttu-id="ab3a1-309">O resultado é classificado como um ponteiro de método, com a mesma expressão de destino associados e o tipo lista de argumentos (se houver) como o grupo de método.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-309">The result is classified as a method pointer, with the same associated target expression and type argument list (if any) as the method group.</span></span>

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a><span data-ttu-id="ab3a1-310">Expressões de tipo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-310">Type Expressions</span></span>

<span data-ttu-id="ab3a1-311">Um *digite a expressão* é um `GetType` expressão, uma `TypeOf...Is` expressão, uma `Is` expressão, ou um `GetXmlNamespace` expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-311">A *type expression* is a `GetType` expression, a `TypeOf...Is` expression, an `Is` expression, or a `GetXmlNamespace` expression.</span></span>

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a><span data-ttu-id="ab3a1-312">Expressões de GetType</span><span class="sxs-lookup"><span data-stu-id="ab3a1-312">GetType Expressions</span></span>

<span data-ttu-id="ab3a1-313">Um `GetType` expressão consiste na palavra-chave `GetType` e o nome de um tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-313">A `GetType` expression consists of the keyword `GetType` and the name of a type.</span></span>

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

<span data-ttu-id="ab3a1-314">Um `GetType` expressão é classificada como um valor, e seu valor é a reflexão (`System.Type`) a classe que representa seu *GetTypeTypeName*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-314">A `GetType` expression is classified as a value, and its value is the reflection (`System.Type`) class that represents its *GetTypeTypeName*.</span></span> <span data-ttu-id="ab3a1-315">Se o *GetTypeTypeName* é um parâmetro de tipo, a expressão retornará o `System.Type` objeto que corresponde ao argumento de tipo fornecido para o parâmetro de tipo em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-315">If the *GetTypeTypeName* is a type parameter, the expression will return the `System.Type` object that corresponds to the type argument supplied for the type parameter at run-time.</span></span>

<span data-ttu-id="ab3a1-316">O *GetTypeTypeName* é especial de duas maneiras:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-316">The *GetTypeTypeName* is special in two ways:</span></span>

* <span data-ttu-id="ab3a1-317">Ele tem permissão para ser `System.Void`, o único lugar no idioma em que esse nome de tipo pode ser referenciado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-317">It is allowed to be `System.Void`, the only place in the language where this type name may be referenced.</span></span>

* <span data-ttu-id="ab3a1-318">Pode ser um tipo genérico construído com os argumentos de tipo omitido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-318">It may be a constructed generic type with the type arguments omitted.</span></span> <span data-ttu-id="ab3a1-319">Isso permite que o `GetType` expressão para retornar o `System.Type` objeto que corresponde ao tipo genérico em si.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-319">This allows the `GetType` expression to return the `System.Type` object that corresponds to the generic type itself.</span></span>

<span data-ttu-id="ab3a1-320">O exemplo a seguir demonstra o `GetType` expressão:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-320">The following example demonstrates the `GetType` expression:</span></span>

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

<span data-ttu-id="ab3a1-321">A saída resultante é:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-321">The resulting output is:</span></span>

```
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a><span data-ttu-id="ab3a1-322">TypeOf... Expressões</span><span class="sxs-lookup"><span data-stu-id="ab3a1-322">TypeOf...Is Expressions</span></span>

<span data-ttu-id="ab3a1-323">Um `TypeOf...Is` expressão é usada para verificar se o tipo de tempo de execução de um valor é compatível com um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-323">A `TypeOf...Is` expression is used to check whether the run-time type of a value is compatible with a given type.</span></span> <span data-ttu-id="ab3a1-324">O primeiro operando deve ser classificado como um valor, não pode ser um método lambda reclassificados e deve ser de um tipo de referência ou um tipo de parâmetro de tipo sem restrição.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-324">The first operand must be classified as a value, cannot be a reclassified lambda method, and must be of a reference type or an unconstrained type parameter type.</span></span> <span data-ttu-id="ab3a1-325">O segundo operando deve ser um nome de tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-325">The second operand must be a type name.</span></span> <span data-ttu-id="ab3a1-326">O resultado da expressão é classificado como um valor e um `Boolean` valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-326">The result of the expression is classified as a value and is a `Boolean` value.</span></span> <span data-ttu-id="ab3a1-327">A expressão é avaliada como `True` se o tipo de tempo de execução do operando tiver uma identidade, padrão, referência, matriz, tipo de valor ou conversão de parâmetro de tipo para o tipo `False` caso contrário.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-327">The expression evaluates to `True` if the run-time type of the operand has an identity, default, reference, array, value type, or type parameter conversion to the type, `False` otherwise.</span></span> <span data-ttu-id="ab3a1-328">Ocorrerá um erro de tempo de compilação se não existir nenhuma conversão entre o tipo da expressão e o tipo específico.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-328">A compile-time error occurs if no conversion exists between the type of the expression and the specific type.</span></span>

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a><span data-ttu-id="ab3a1-329">Expressões</span><span class="sxs-lookup"><span data-stu-id="ab3a1-329">Is Expressions</span></span>

<span data-ttu-id="ab3a1-330">Uma `Is` ou `IsNot` expressão é usada para fazer uma comparação de igualdade de referência.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-330">An `Is` or `IsNot` expression is used to do a reference equality comparison.</span></span>

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

<span data-ttu-id="ab3a1-331">Cada expressão deve ser classificada como um valor e o tipo de cada expressão deve ser um tipo de referência, um tipo de parâmetro de tipo sem restrição ou um tipo de valor anulável.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-331">Each expression must be classified as a value and the type of each expression must be a reference type, an unconstrained type parameter type, or a nullable value type.</span></span> <span data-ttu-id="ab3a1-332">Se o tipo de uma expressão for um tipo de parâmetro de tipo sem restrição ou um tipo de valor anulável, no entanto, a outra expressão deve ser o literal `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-332">If the type of one expression is an unconstrained type parameter type or nullable value type, however, the other expression must be the literal `Nothing`.</span></span>

<span data-ttu-id="ab3a1-333">O resultado é classificado como um valor e é digitado como `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-333">The result is classified as a value and is typed as `Boolean`.</span></span> <span data-ttu-id="ab3a1-334">Uma `Is` operação é avaliada como `True` se ambos os valores se referem à mesma instância ou os dois valores forem `Nothing`, ou `False` caso contrário.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-334">An `Is` operation evaluates to `True` if both values refer to the same instance or both values are `Nothing`, or `False` otherwise.</span></span> <span data-ttu-id="ab3a1-335">Uma `IsNot` operação é avaliada como `False` se ambos os valores se referem à mesma instância ou os dois valores forem `Nothing`, ou `True` caso contrário.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-335">An `IsNot` operation evaluates to `False` if both values refer to the same instance or both values are `Nothing`, or `True` otherwise.</span></span>


### <a name="getxmlnamespace-expressions"></a><span data-ttu-id="ab3a1-336">Expressões de GetXmlNamespace</span><span class="sxs-lookup"><span data-stu-id="ab3a1-336">GetXmlNamespace Expressions</span></span>

<span data-ttu-id="ab3a1-337">Um `GetXmlNamespace` expressão consiste na palavra-chave `GetXmlNamespace` e o nome de um namespace XML declarado pelo ambiente de compilação ou de arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-337">A `GetXmlNamespace` expression consists of the keyword `GetXmlNamespace` and the name of an XML namespace declared by the source file or compilation environment.</span></span>

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

<span data-ttu-id="ab3a1-338">Uma `GetXmlNamespace` expressão é classificada como um valor, e seu valor é uma instância do `System.Xml.Linq.XNamespace` que representa o *XMLNamespaceName*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-338">An `GetXmlNamespace` expression is classified as a value, and its value is an instance of `System.Xml.Linq.XNamespace` that represents the *XMLNamespaceName*.</span></span> <span data-ttu-id="ab3a1-339">Se esse tipo não estiver disponível, em seguida, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-339">If that type is not available, then a compile-time error will occur.</span></span>

<span data-ttu-id="ab3a1-340">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-340">For example:</span></span>

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

<span data-ttu-id="ab3a1-341">Tudo entre os parênteses é considerado parte do nome do namespace, portanto, se aplicam as regras de XML em torno de coisas como espaços em branco.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-341">Everything between the parentheses is considered part of the namespace name, so XML rules around things such as whitespace apply.</span></span> <span data-ttu-id="ab3a1-342">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-342">For example:</span></span>

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

<span data-ttu-id="ab3a1-343">A expressão de namespace XML também pode ser omitida, caso em que a expressão retorna o objeto que representa o namespace XML padrão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-343">The XML namespace expression can also be omitted, in which case the expression returns the object that represents the default XML namespace.</span></span>


## <a name="member-access-expressions"></a><span data-ttu-id="ab3a1-344">Expressões de acesso de membro</span><span class="sxs-lookup"><span data-stu-id="ab3a1-344">Member Access Expressions</span></span>

<span data-ttu-id="ab3a1-345">Uma expressão de acesso de membro é usada para acessar um membro de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-345">A member access expression is used to access a member of an entity.</span></span>

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

<span data-ttu-id="ab3a1-346">Um acesso de membro do formulário `E.I(Of A)`, onde `E` é uma expressão, um nome de tipo não matriz, a palavra-chave `Global`, ou não especificado e `I` é um identificador com uma lista de argumentos de tipo opcional `A`é avaliado e classificado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-346">A member access of the form `E.I(Of A)`, where `E` is an expression, a non-array type name, the keyword `Global`, or omitted and `I` is an identifier with an optional type argument list `A`, is evaluated and classified as follows:</span></span>

1. <span data-ttu-id="ab3a1-347">Se `E` for omitido, em seguida, a expressão imediatamente contenha `With` instrução seja substituída por `E` e o acesso de membro é executado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-347">If `E` is omitted, then the expression from the immediately containing `With` statement is substituted for `E` and the member access is performed.</span></span> <span data-ttu-id="ab3a1-348">Se não houver nenhum contendo `With` declaração, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-348">If there is no containing `With` statement, a compile-time error occurs.</span></span>

2. <span data-ttu-id="ab3a1-349">Se `E` é classificado como um namespace ou `E` é a palavra-chave `Global`, em seguida, a pesquisa de membro é feita no contexto de namespace especificado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-349">If `E` is classified as a namespace or `E` is the keyword `Global`, then the member lookup is done in the context of the specified namespace.</span></span> <span data-ttu-id="ab3a1-350">Se `I` é o nome de um membro acessível do namespace com o mesmo número de parâmetros de tipo como foi fornecido na lista de argumentos de tipo, se houver, em seguida, o resultado é que o membro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-350">If `I` is the name of an accessible member of that namespace with the same number of type parameters as was supplied in the type argument list, if any, then the result is that member.</span></span> <span data-ttu-id="ab3a1-351">O resultado é classificado como um namespace ou um tipo, dependendo do membro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-351">The result is classified as a namespace or a type depending on the member.</span></span> <span data-ttu-id="ab3a1-352">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-352">Otherwise, a compile-time error occurs.</span></span>

3. <span data-ttu-id="ab3a1-353">Se `E` é um tipo ou uma expressão classificado como um tipo, em seguida, a pesquisa de membro é feita no contexto do tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-353">If `E` is a type or an expression classified as a type, then the member lookup is done in the context of the specified type.</span></span> <span data-ttu-id="ab3a1-354">Se `I` é o nome de um membro acessível do `E`, em seguida, `E.I` é avaliado e classificados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-354">If `I` is the name of an accessible member of `E`, then `E.I` is evaluated and classified as follows:</span></span>

    31. <span data-ttu-id="ab3a1-355">Se `I` é a palavra-chave `New` e `E` não é uma enumeração, em seguida, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-355">If `I` is the keyword `New` and `E` is not an enumeration then a compile-time error occurs.</span></span>
    32. <span data-ttu-id="ab3a1-356">Se `I` identifica um tipo com o mesmo número de parâmetros de tipo, como foi fornecido na lista de argumentos de tipo, se houver, em seguida, o resultado será aquele tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-356">If `I` identifies a type with the same number of type parameters as was supplied in the type argument list, if any, then the result is that type.</span></span>
    33. <span data-ttu-id="ab3a1-357">Se `I` identifica um ou mais métodos, o resultado será um grupo de métodos com lista de argumentos de tipo associado e nenhuma expressão de destino associados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-357">If `I` identifies one or more methods, then the result is a method group with the associated type argument list and no associated target expression.</span></span>
    34. <span data-ttu-id="ab3a1-358">Se `I` identifica uma ou mais propriedades e nenhum tipo de lista de argumentos foi fornecida e, em seguida, o resultado é um grupo de propriedades sem expressão de destino associados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-358">If `I` identifies one or more properties and no type argument list was supplied, then the result is a property group with no associated target expression.</span></span>
    35. <span data-ttu-id="ab3a1-359">Se `I` identifica uma variável compartilhada e nenhum tipo de lista de argumentos foi fornecida e, em seguida, o resultado é uma variável ou um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-359">If `I` identifies a shared variable and no type argument list was supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="ab3a1-360">Se a variável é somente leitura e a referência ocorre fora do construtor compartilhado do tipo no qual a variável é declarada, o resultado é o valor da variável compartilhada `I` em `E`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-360">If the variable is read-only, and the reference occurs outside the shared constructor of the type in which the variable is declared, then the result is the value of the shared variable `I` in `E`.</span></span> <span data-ttu-id="ab3a1-361">Caso contrário, o resultado é a variável compartilhada `I` em `E`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-361">Otherwise, the result is the shared variable `I` in `E`.</span></span>
    36. <span data-ttu-id="ab3a1-362">Se `I` identifica um evento compartilhado e nenhum tipo de lista de argumentos foi fornecida, o resultado é um evento de acesso sem expressão de destino associados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-362">If `I` identifies a shared event and no type argument list was supplied, the result is an event access with no associated target expression.</span></span>
    37. <span data-ttu-id="ab3a1-363">Se `I` identifica uma constante e nenhum tipo de lista de argumentos foi fornecida e, em seguida, o resultado é o valor dessa constante.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-363">If `I` identifies a constant and no type argument list was supplied, then the result is the value of that constant.</span></span>
    38. <span data-ttu-id="ab3a1-364">Se `I` identifica um membro de enumeração e nenhum tipo de lista de argumentos foi fornecida e, em seguida, o resultado é o valor desse membro de enumeração.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-364">If `I` identifies an enumeration member and no type argument list was supplied, then the result is the value of that enumeration member.</span></span>
    39. <span data-ttu-id="ab3a1-365">Caso contrário, `E.I` é uma referência de membro inválido e ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-365">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

4. <span data-ttu-id="ab3a1-366">Se `E` é classificado como uma variável ou um valor, o tipo do qual está `T`, em seguida, a pesquisa de membro é feita no contexto de `T`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-366">If `E` is classified as a variable or value, the type of which is `T`, then the member lookup is done in the context of `T`.</span></span> <span data-ttu-id="ab3a1-367">Se `I` é o nome de um membro acessível do `T`, em seguida, `E.I` é avaliado e classificados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-367">If `I` is the name of an accessible member of `T`, then `E.I` is evaluated and classified as follows:</span></span>

    41. <span data-ttu-id="ab3a1-368">Se `I` é a palavra-chave `New`, `E` é `Me`, `MyBase`, ou `MyClass`e foi fornecido nenhum argumento de tipo e, em seguida, o resultado é um grupo de métodos que representam os construtores de instância do tipo de `E`com uma expressão de destino associados do `E` e nenhuma lista de argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-368">If `I` is the keyword `New`, `E` is  `Me`, `MyBase`, or `MyClass`, and no type arguments were supplied, then the result is a method group representing the instance constructors of the type of `E` with an associated target expression of `E` and no type argument list.</span></span> <span data-ttu-id="ab3a1-369">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-369">Otherwise, a compile-time error occurs.</span></span>
    42. <span data-ttu-id="ab3a1-370">Se `I` identifica um ou mais métodos, incluindo métodos de extensão se `T` não está `Object`, em seguida, o resultado é um grupo de métodos com lista de argumentos de tipo associado e uma expressão de destino associados do `E`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-370">If `I` identifies one or more methods, including extension methods if `T` is not `Object`, then the result is a method group with the associated type argument list and an associated target expression of `E`.</span></span>
    43. <span data-ttu-id="ab3a1-371">Se `I` identifica uma ou mais propriedades e nenhum tipo de argumentos foram fornecidos, o resultado será um grupo de propriedades com uma expressão de destino associados do `E`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-371">If `I` identifies one or more properties and no type arguments were supplied, then the result is a property group with an associated target expression of `E`.</span></span>
    44. <span data-ttu-id="ab3a1-372">Se `I` identifica uma variável compartilhada ou uma variável de instância e nenhum tipo de argumentos foram fornecidos, em seguida, o resultado é uma variável ou um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-372">If `I` identifies a shared variable or an instance variable and no type arguments were supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="ab3a1-373">Se a variável é somente leitura e a referência ocorre fora de um construtor da classe na qual a variável é declarada apropriada para o tipo de variável (compartilhado ou de instância), o resultado é o valor da variável `I` no objeto referenciado por `E`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-373">If the variable is read-only, and the reference occurs outside a constructor of the class in which the variable is declared appropriate for the kind of variable (shared or instance), then the result is the value of the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="ab3a1-374">Se `T` é um tipo de referência, em seguida, o resultado é a variável `I` no objeto referenciado pelo `E`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-374">If `T` is a reference type, then the result is the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="ab3a1-375">Caso contrário, se `T` é um tipo de valor e a expressão `E` é classificado como uma variável, o resultado é uma variável; caso contrário, o resultado é um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-375">Otherwise, if `T` is a value type and the expression `E` is classified as a variable, the result is a variable; otherwise the result is a value.</span></span>
    45. <span data-ttu-id="ab3a1-376">Se `I` identifica um evento e nenhum tipo de argumentos foram fornecidos, o resultado é um evento de acesso com uma expressão de destino associados do `E`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-376">If `I` identifies an event and no type arguments were supplied, the result is an event access with an associated target expression of `E`.</span></span>
    46. <span data-ttu-id="ab3a1-377">Se `I` identifica uma constante e nenhum tipo de argumentos foram fornecidos, o resultado será o valor dessa constante.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-377">If `I` identifies a constant and no type arguments were supplied, then the result is the value of that constant.</span></span>
    47. <span data-ttu-id="ab3a1-378">Se `I` identifica um membro de enumeração e nenhum tipo de argumentos foram fornecidos, o resultado será o valor desse membro de enumeração.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-378">If `I` identifies an enumeration member and no type arguments were supplied, then the result is the value of that enumeration member.</span></span>
    48. <span data-ttu-id="ab3a1-379">Se `T` está `Object`, em seguida, o resultado é uma pesquisa de membro de associação tardia classificada como um acesso de associação tardia com a lista de argumentos de tipo associado e uma expressão de destino associados do `E`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-379">If `T` is `Object`, then the result is a late-bound member lookup classified as a late-bound access with the associated type argument list and an associated target expression of `E`.</span></span>

5. <span data-ttu-id="ab3a1-380">Caso contrário, `E.I` é uma referência de membro inválido e ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-380">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

<span data-ttu-id="ab3a1-381">Um acesso de membro do formulário `MyClass.I(Of A)` é equivalente a `Me.I(Of A)`, mas todos os membros acessados nele são tratados como se os membros são não pode ser substituído.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-381">A member access of the form `MyClass.I(Of A)` is equivalent to `Me.I(Of A)`, but all members accessed on it are treated as if the members are non-overridable.</span></span> <span data-ttu-id="ab3a1-382">Portanto, o membro acessado não será afetado pelo tipo de tempo de execução do valor no qual o membro está sendo acessado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-382">Thus, the member accessed will not be affected by the run-time type of the value on which the member is being accessed.</span></span>

<span data-ttu-id="ab3a1-383">Um acesso de membro do formulário `MyBase.I(Of A)` é equivalente a `CType(Me, T).I(Of A)` onde `T` é o tipo base direto do tipo que contém a expressão de acesso de membro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-383">A member access of the form `MyBase.I(Of A)` is equivalent to `CType(Me, T).I(Of A)` where `T` is the direct base type of the type containing the member access expression.</span></span> <span data-ttu-id="ab3a1-384">Todas as invocações de método nele são tratadas como se o método que está sendo invocado é não pode ser substituído.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-384">All method invocations on it are treated as if the method being invoked is non-overridable.</span></span> <span data-ttu-id="ab3a1-385">Essa forma de acesso de membro também é chamada de um *acesso de base*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-385">This form of member access is also called a *base access*.</span></span>

<span data-ttu-id="ab3a1-386">O exemplo a seguir demonstra como `Me`, `MyBase` e `MyClass` relacionados:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-386">The following example demonstrates how `Me`, `MyBase` and `MyClass` relate:</span></span>

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

<span data-ttu-id="ab3a1-387">Esse código imprime:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-387">This code prints out:</span></span>

```
MoreDerived.F
Derived.F
Derived.F
```

<span data-ttu-id="ab3a1-388">Quando uma expressão de acesso de membro começa com a palavra-chave `Global`, a palavra-chave representa o namespace sem nome externo, que é útil em situações em que uma declaração se sobrepõe a um namespace delimitador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-388">When a member access expression begins with the keyword `Global`, the keyword represents the outermost unnamed namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="ab3a1-389">O `Global` permite a palavra-chave "escape" out para o namespace mais externo nessa situação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-389">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="ab3a1-390">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-390">For example:</span></span>

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

<span data-ttu-id="ab3a1-391">No exemplo acima, a primeira chamada de método é inválida porque o identificador `System` associa à classe `System`, não o namespace `System`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-391">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="ab3a1-392">A única maneira de acessar o `System` namespace é usar `Global` pulem-out para o namespace mais externo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-392">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span>

<span data-ttu-id="ab3a1-393">Se o membro que está sendo acessado for compartilhado, qualquer expressão no lado esquerdo do período é supérfluo e não é avaliada, a menos que o acesso de membro é feito associação tardia.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-393">If the member being accessed is shared, any expression on the left side of the period is superfluous and is not evaluated unless the member access is done late-bound.</span></span> <span data-ttu-id="ab3a1-394">Por exemplo, considere o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-394">For example, consider the following code:</span></span>

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

<span data-ttu-id="ab3a1-395">Ele imprime `The value of F is: 10` porque a função `ReturnC` não precisa ser chamado para fornecer uma instância do `C` para acessar o membro compartilhado `F`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-395">It prints `The value of F is: 10` because the function `ReturnC` does not need to be called to provide an instance of `C` to access the shared member `F`.</span></span>


### <a name="identical-type-and-member-names"></a><span data-ttu-id="ab3a1-396">Tipo idêntico e nomes de membro</span><span class="sxs-lookup"><span data-stu-id="ab3a1-396">Identical Type and Member Names</span></span>

<span data-ttu-id="ab3a1-397">Não é incomum para os membros de nome usando o mesmo nome como seu tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-397">It is not uncommon to name members using the same name as their type.</span></span> <span data-ttu-id="ab3a1-398">Nessa situação, no entanto, ocultação de nome inconveniente pode ocorrer:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-398">In that situation, however, inconvenient name hiding can occur:</span></span>

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

<span data-ttu-id="ab3a1-399">No exemplo anterior, o nome simples `Color` em `DefaultColor` associado à propriedade de instância em vez do tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-399">In the previous example, the simple name `Color` in `DefaultColor` binds to the instance property instead of the type.</span></span> <span data-ttu-id="ab3a1-400">Como um membro de instância não pode ser referenciado em um membro compartilhado, isso normalmente seria um erro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-400">Because an instance member cannot be referenced in a shared member, this would normally be an error.</span></span>

<span data-ttu-id="ab3a1-401">No entanto, uma regra especial permite o acesso ao tipo nesse caso.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-401">However, a special rule allows access to the type in this case.</span></span> <span data-ttu-id="ab3a1-402">Se a expressão de base de uma expressão de acesso de membro é um nome simples e associa a uma constante, campo, propriedade, variável local ou parâmetro cujo tipo tem o mesmo nome, a expressão de base pode referir-se para o membro ou tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-402">If the base expression of a member access expression is a simple name and binds to a constant, field, property, local variable or parameter whose type has the same name, then the base expression can refer either to the member or the type.</span></span> <span data-ttu-id="ab3a1-403">Isso nunca pode resultar em ambiguidade porque os membros que podem ser acessados fora deles são os mesmos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-403">This can never result in ambiguity because the members that can be accessed off of either one are the same.</span></span>

### <a name="default-instances"></a><span data-ttu-id="ab3a1-404">Instâncias padrão</span><span class="sxs-lookup"><span data-stu-id="ab3a1-404">Default Instances</span></span>

<span data-ttu-id="ab3a1-405">Em algumas situações, classes derivadas de uma comum geralmente de classe de base ou sempre tem apenas uma única instância.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-405">In some situations, classes derived from a common base class usually or always have only a single instance.</span></span> <span data-ttu-id="ab3a1-406">Por exemplo, a maioria dos windows mostrados em uma interface do usuário só tem uma instância que mostrando na tela a qualquer momento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-406">For example, most windows shown in a user interface only ever have one instance showing on the screen at any time.</span></span> <span data-ttu-id="ab3a1-407">Para simplificar o trabalho com esses tipos de classes, Visual Basic pode gerar automaticamente *instâncias padrão* das classes que fornecem uma instância única e facilmente referenciada para cada classe.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-407">To simplify working with these types of classes, Visual Basic can automatically generate *default instances* of the classes that provide a single, easily referenced instance for each class.</span></span>

<span data-ttu-id="ab3a1-408">Instâncias padrão são sempre criadas para um *família* de tipos em vez de para um tipo específico.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-408">Default instances are always created for a *family* of types rather than for one particular type.</span></span> <span data-ttu-id="ab3a1-409">Portanto, em vez de criar uma instância padrão para uma classe Form1 que deriva de Form, instâncias padrão são criadas para todas as classes derivadas do formulário.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-409">So instead of creating a default instance for a class Form1 that derives from Form, default instances are created for all classes derived from Form.</span></span> <span data-ttu-id="ab3a1-410">Isso significa que cada classe individual que é derivada da classe base não precisa ser especialmente marcados para ter uma instância padrão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-410">This means that each individual class that derives from the base class does not have to be specially marked to have a default instance.</span></span>

<span data-ttu-id="ab3a1-411">A instância padrão de uma classe é representada por uma propriedade gerada pelo compilador que retorna a instância padrão dessa classe.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-411">The default instance of a class is represented by a compiler-generated property that returns the default instance of that class.</span></span> <span data-ttu-id="ab3a1-412">A propriedade gerada como um membro de uma classe chamada a *classe grupo* que gerencia a alocação e a destruição de instâncias padrão para todas as classes derivadas da classe base específica.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-412">The property generated as a member of a class called the *group class* that manages allocating and destroying default instances for all classes derived from the particular base class.</span></span> <span data-ttu-id="ab3a1-413">Por exemplo, todas as propriedades de instância padrão de classes derivados de `Form` podem ser coletadas no `MyForms` classe.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-413">For example, all of the default instance properties of classes derived from `Form` may be collected in the `MyForms` class.</span></span> <span data-ttu-id="ab3a1-414">Se uma instância da classe de grupo é retornada pela expressão `My.Forms`, em seguida, o código a seguir acessa as instâncias padrão de classes derivadas `Form1` e `Form2`:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-414">If an instance of the group class is returned by the expression `My.Forms`, then the following code accesses the default instances of derived classes `Form1` and `Form2`:</span></span>

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

<span data-ttu-id="ab3a1-415">Instâncias padrão não serão criadas até que a primeira referência a eles; buscando a propriedade que representa a instância padrão faz com que a instância padrão a ser criado se ele já não tiver sido criado ou foi definido como `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-415">Default instances will not be created until the first reference to them; fetching the property representing the default instance causes the default instance to be created if it has not already been created or has been set to `Nothing`.</span></span> <span data-ttu-id="ab3a1-416">Para testar a existência de uma instância padrão, quando uma instância padrão é o destino de uma `Is` ou `IsNot` operador, a instância padrão não será criada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-416">To allow testing for the existence of a default instance, when a default instance is the target of an `Is` or `IsNot` operator, the default instance will not be created.</span></span> <span data-ttu-id="ab3a1-417">Portanto, é possível testar se uma instância padrão é `Nothing` ou alguma outra referência sem fazer com que a instância padrão a ser criado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-417">Thus, it is possible to test whether a default instance is `Nothing` or some other reference without causing the default instance to be created.</span></span>

<span data-ttu-id="ab3a1-418">Instâncias padrão destinam-se tornar mais fácil referir-se à instância padrão de fora da classe que tem a instância padrão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-418">Default instances are intended to make it easy to refer to the default instance from outside of the class that has the default instance.</span></span> <span data-ttu-id="ab3a1-419">Usando uma instância padrão de dentro de uma classe que define que ele pode causar uma confusão sobre qual instância está sendo referenciada, ou seja, a instância padrão ou a instância atual.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-419">Using a default instance from within a class that defines it might cause confusion as to which instance is being referred to, i.e. the default instance or the current instance.</span></span> <span data-ttu-id="ab3a1-420">Por exemplo, o código a seguir modifica apenas o valor `x` na instância padrão, mesmo que ele está sendo chamado de outra instância.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-420">For example, the following code modifies only the value `x` in the default instance, even though it is being called from another instance.</span></span> <span data-ttu-id="ab3a1-421">Portanto, o código seria imprimir o valor `5` em vez de `10`:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-421">Thus the code would print the value `5` instead of `10`:</span></span>

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

<span data-ttu-id="ab3a1-422">Para evitar esse tipo de confusão, não é válido para se referir a uma instância padrão de dentro de um método de instância do tipo da instância padrão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-422">To prevent this kind of confusion, it is not valid to refer to a default instance from within an instance method of the default instance's type.</span></span>

#### <a name="default-instances-and-type-names"></a><span data-ttu-id="ab3a1-423">Instâncias padrão e nomes de tipo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-423">Default Instances and Type Names</span></span>

<span data-ttu-id="ab3a1-424">Uma instância padrão também possa ser acessada diretamente por meio do nome do seu tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-424">A default instance may also be accessible directly through its type's name.</span></span> <span data-ttu-id="ab3a1-425">Nesse caso, em qualquer contexto de expressão em que o nome do tipo não é permitido a expressão `E`, onde `E` representa o nome totalmente qualificado da classe com uma instância padrão, é alterado para `E'`, onde `E'` representa uma expressão que a propriedade de instância padrão de busca.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-425">In this case, in any expression context where the type name is not allowed the expression `E`, where `E` represents the fully qualified name of the class with a default instance, is changed to `E'`, where `E'` represents an expression that fetches the default instance property.</span></span> <span data-ttu-id="ab3a1-426">Por exemplo, se o padrão de instâncias de classes derivadas de `Form` permitir o acesso a instância padrão pelo nome do tipo, em seguida, o código a seguir é equivalente ao código no exemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-426">For example, if default instances for classes derived from `Form` allow accessing the default instance through the type name, then the following code is equivalent to the code in the previous example:</span></span>

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

<span data-ttu-id="ab3a1-427">Isso também significa que uma instância padrão que é acessível por meio do nome do seu tipo também pode ser atribuída pelo nome do tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-427">This also means that a default instance that is accessible through its type's name is also assignable through the type name.</span></span> <span data-ttu-id="ab3a1-428">Por exemplo, o código a seguir define a instância padrão do `Form1` para `Nothing`:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-428">For example, the following code sets the default instance of `Form1` to `Nothing`:</span></span>

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

<span data-ttu-id="ab3a1-429">Observe que o significado dos `E.I` foram `E` representa uma classe e `I` representa um membro compartilhado não é alterado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-429">Note that the meaning of `E.I` were `E` represents a class and `I` represents a shared member does not change.</span></span> <span data-ttu-id="ab3a1-430">Essa expressão ainda acessa o membro compartilhado diretamente da instância da classe e não faz referência a instância padrão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-430">Such an expression still accesses the shared member directly off of the class instance and does not reference the default instance.</span></span>

#### <a name="group-classes"></a><span data-ttu-id="ab3a1-431">Classes de grupo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-431">Group Classes</span></span>

<span data-ttu-id="ab3a1-432">O `Microsoft.VisualBasic.MyGroupCollectionAttribute` atributo indica a classe de grupo para uma família de instâncias padrão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-432">The `Microsoft.VisualBasic.MyGroupCollectionAttribute` attribute indicates the group class for a family of default instances.</span></span> <span data-ttu-id="ab3a1-433">O atributo tem quatro parâmetros:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-433">The attribute has four parameters:</span></span>

* <span data-ttu-id="ab3a1-434">O parâmetro `TypeToCollect` Especifica a classe base para o grupo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-434">The parameter `TypeToCollect` specifies the base class for the group.</span></span> <span data-ttu-id="ab3a1-435">Todas as classes podem ser instanciadas sem parâmetros de tipo aberto que derivam de um tipo com esse nome (independentemente de parâmetros de tipo) terão automaticamente uma instância padrão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-435">All instantiable classes without open type parameters that derive from a type with this name (regardless of type parameters) will automatically have a default instance.</span></span>

* <span data-ttu-id="ab3a1-436">O parâmetro `CreateInstanceMethodName` Especifica o método a ser chamado na classe de grupo para criar uma nova instância em uma propriedade de instância padrão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-436">The parameter `CreateInstanceMethodName` specifies the method to call in the group class to create a new instance in a default instance property.</span></span>

* <span data-ttu-id="ab3a1-437">O parâmetro `DisposeInstanceMethodName` Especifica o método a ser chamado na classe de grupo de descarte de uma propriedade de instância padrão, se a propriedade de instância padrão é atribuída o valor `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-437">The parameter `DisposeInstanceMethodName` specifies the method to call in the group class to dispose of a default instance property if the default instance property is assigned the value `Nothing`.</span></span>

* <span data-ttu-id="ab3a1-438">O parâmetro `DefaultInstanceAlias` especificidades a expressão `E'` para substituir o nome de classe se as instâncias padrão são acessíveis diretamente por meio de seu nome de tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-438">The parameter `DefaultInstanceAlias` specifics the expression `E'` to substitute for the class name if the default instances are accessible directly through their type name.</span></span> <span data-ttu-id="ab3a1-439">Se esse parâmetro for `Nothing` ou uma cadeia de caracteres vazia, instâncias padrão em que esse tipo de grupo não são acessíveis diretamente por meio do nome do seu tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-439">If this parameter is `Nothing` or an empty string, default instances on this group type are not accessible directly through their type's name.</span></span> <span data-ttu-id="ab3a1-440">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-440">(__Note.__</span></span> <span data-ttu-id="ab3a1-441">Em todas as implementações atuais da linguagem Visual Basic, o `DefaultInstanceAlias` parâmetro é ignorado, exceto no código fornecido pelo compilador.)</span><span class="sxs-lookup"><span data-stu-id="ab3a1-441">In all current implementations of the Visual Basic language, the `DefaultInstanceAlias` parameter is ignored, except in compiler-provided code.)</span></span>

<span data-ttu-id="ab3a1-442">Vários tipos podem ser coletados no mesmo grupo, separando os nomes dos tipos e métodos nos três primeiros parâmetros usando vírgulas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-442">Multiple types can be collected into the same group by separating the names of the types and methods in the first three parameters using commas.</span></span> <span data-ttu-id="ab3a1-443">Deve haver o mesmo número de itens em cada parâmetro e os elementos de lista são correspondidos na ordem.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-443">There must be the same number of items in each parameter, and the list elements are matched in order.</span></span> <span data-ttu-id="ab3a1-444">Por exemplo, a seguinte declaração de atributo coleta os tipos que derivam de `C1`, `C2` ou `C3` em um único grupo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-444">For example, the following attribute declaration collects types that derive from `C1`, `C2` or `C3` into a single group:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

<span data-ttu-id="ab3a1-445">A assinatura do método create deve estar no formato `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-445">The signature of the create method must be of the form `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`.</span></span> <span data-ttu-id="ab3a1-446">O método dispose deve estar no formato `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-446">The dispose method must be of the form `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`.</span></span> <span data-ttu-id="ab3a1-447">Assim, a classe de grupo para o exemplo na seção anterior poderia ser declarada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-447">Thus, the group class for the example in the preceding section could be declared as follows:</span></span>

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

<span data-ttu-id="ab3a1-448">Se um arquivo de origem declarado uma classe derivada `Form1`, a classe gerada grupo seria equivalente a:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-448">If a source file declared a derived class `Form1`, the generated group class would be equivalent to:</span></span>

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

### <a name="extension-method-collection"></a><span data-ttu-id="ab3a1-449">Coleção de método de extensão</span><span class="sxs-lookup"><span data-stu-id="ab3a1-449">Extension Method Collection</span></span>

<span data-ttu-id="ab3a1-450">Expressão de acesso a métodos de extensão para o membro `E.I` são coletados, reunindo todos os métodos de extensão com o nome `I` que estão disponíveis no contexto atual:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-450">Extension methods for the member access expression `E.I` are collected by gathering all of the extension methods with the name `I` that are available in the current context:</span></span>

1. <span data-ttu-id="ab3a1-451">Em primeiro lugar, cada tipo aninhado que contém a expressão é verificado, começando em mais interna e vai mais externo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-451">First, each nested type containing the expression is checked, starting from the innermost and going to the outermost.</span></span>
2. <span data-ttu-id="ab3a1-452">Em seguida, cada namespace aninhado é verificado, começando em mais interna e vai para o namespace mais externo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-452">Then, each nested namespace is checked, starting from the innermost and going to the outermost namespace.</span></span>
3. <span data-ttu-id="ab3a1-453">Em seguida, as importações no arquivo de origem são verificadas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-453">Then, the imports in the source file are checked.</span></span>
4. <span data-ttu-id="ab3a1-454">Em seguida, as importações definidas pelo ambiente de compilação são verificadas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-454">Then, the imports defined by the compilation environment are checked.</span></span>

<span data-ttu-id="ab3a1-455">Um método de extensão é coletado somente se houver uma nativa conversão de ampliação do tipo de expressão de destino para o tipo do primeiro parâmetro do método de extensão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-455">An extension method is collected only if there is a widening native conversion from the target expression type to the type of the first parameter of the extension method.</span></span> <span data-ttu-id="ab3a1-456">E, ao contrário de associação de expressão regular nome simples, a pesquisa coleta *todos os* métodos de extensão; a coleção não é interrompida quando um método de extensão for encontrado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-456">And unlike regular simple name expression binding, the search collects *all* extension methods; the collection does not stop when an extension method is found.</span></span> <span data-ttu-id="ab3a1-457">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-457">For example:</span></span>

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

<span data-ttu-id="ab3a1-458">Neste exemplo, embora `N2C1Extensions.M1` for encontrado antes `N1C1Extensions.M1`, ambas são consideradas como métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-458">In this example, even though `N2C1Extensions.M1` is found before `N1C1Extensions.M1`, they both are considered as extension methods.</span></span> <span data-ttu-id="ab3a1-459">Depois que todos os métodos de extensão forem coletados, eles são então *via currying*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-459">Once all of the extension methods have been collected, they are then *curried*.</span></span> <span data-ttu-id="ab3a1-460">Currying usa o destino da chamada do método de extensão e aplica-se a chamada do método de extensão, resultando em uma nova assinatura de método com o primeiro parâmetro removido (porque ele foi especificado).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-460">Currying takes the target of the extension method call and applies it to the extension method call, resulting in a new method signature with the first parameter removed (because it has been specified).</span></span> <span data-ttu-id="ab3a1-461">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-461">For example:</span></span>

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

<span data-ttu-id="ab3a1-462">No exemplo acima, o resultado na forma curried da aplicação `v` à `Ext1.M` é a assinatura do método `Sub M(y As Integer)`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-462">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(y As Integer)`.</span></span>

<span data-ttu-id="ab3a1-463">Além de remover o primeiro parâmetro do método de extensão, currying também remove quaisquer parâmetros de tipo de método que fazem parte do tipo do primeiro parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-463">In addition to removing the first parameter of the extension method, currying also removes any method type parameters that are a part of the type of the first parameter.</span></span> <span data-ttu-id="ab3a1-464">Quando currying um método de extensão com o parâmetro de tipo de método, a inferência de tipo é aplicada ao primeiro parâmetro e o resultado é fixa para quaisquer parâmetros de tipo são inferidos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-464">When currying an extension method with method type parameter, type inference is applied to the first parameter and the result is fixed for any type parameters that are inferred.</span></span> <span data-ttu-id="ab3a1-465">Se a inferência de tipo falhar, o método é ignorado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-465">If type inference fails, the method is ignored.</span></span> <span data-ttu-id="ab3a1-466">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-466">For example:</span></span>

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

<span data-ttu-id="ab3a1-467">No exemplo acima, o resultado na forma curried da aplicação `v` à `Ext1.M` é a assinatura de método `Sub M(Of U)(y As U)`, porque o parâmetro de tipo `T` é inferido como resultado o currying e agora foi corrigido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-467">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(Of U)(y As U)`, because the type parameter `T` is inferred as a result of the currying and is now fixed.</span></span> <span data-ttu-id="ab3a1-468">Porque o parâmetro de tipo `U` não foi inferida como parte do currying, ele permanecerá um parâmetro de abertura.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-468">Because the type parameter `U` was not inferred as a part of the currying, it remains an open parameter.</span></span> <span data-ttu-id="ab3a1-469">Da mesma forma, porque o parâmetro de tipo `T` é inferido como resultado da aplicação `v` à `Ext2.M`, o tipo do parâmetro `y` se torna fixo como `Integer`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-469">Similarly, because the type parameter `T` is inferred as a result of applying `v` to `Ext2.M`, the type of parameter `y` becomes fixed as `Integer`.</span></span> <span data-ttu-id="ab3a1-470">Ele não será inferido para ser de qualquer outro tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-470">It will not be inferred to be any other type.</span></span> <span data-ttu-id="ab3a1-471">Quando currying a assinatura, todas as restrições, exceto para `New` restrições também são aplicadas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-471">When currying the signature, all constraints except for `New` constraints are also applied.</span></span> <span data-ttu-id="ab3a1-472">Se as restrições não são atendidas ou dependem de um tipo que não foi inferido como parte do currying, o método de extensão é ignorado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-472">If the constraints are not satisfied, or depend on a type that was not inferred as a part of currying, the extension method is ignored.</span></span> <span data-ttu-id="ab3a1-473">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-473">For example:</span></span>

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

<span data-ttu-id="ab3a1-474">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-474">__Note.__</span></span> <span data-ttu-id="ab3a1-475">Uma das principais razões para fazer currying dos métodos de extensão é que ele permite que as expressões de consulta inferir o tipo da iteração antes de avaliar os argumentos para um método de padrão de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-475">One of the main reasons for doing currying of extension methods is that it allows query expressions to infer the type of the iteration before evaluating the arguments to a query pattern method.</span></span> <span data-ttu-id="ab3a1-476">Como a maioria dos métodos padrão de consulta demorar expressões lambda, que exigem inferência de tipos próprios, isso simplifica bastante o processo de avaliar uma expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-476">Since most query pattern methods take lambda expressions, which require type inference themselves, this greatly simplifies the process of evaluating a query expression.</span></span>

<span data-ttu-id="ab3a1-477">Ao contrário de herança da interface normal, os métodos de extensão que estendem as duas interfaces que não estão relacionados uns aos outros estão disponíveis, desde que eles não têm a mesma assinatura na forma curried:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-477">Unlike normal interface inheritance, extension methods that extend two interfaces that do not relate to one another are available, as long as they do not have the same curried signature:</span></span>

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

<span data-ttu-id="ab3a1-478">Por fim, é importante lembrar-se de que métodos não são considerados ao fazer a associação tardia de extensão:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-478">Finally, it is important to remember that extension methods are not considered when doing late binding:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a><span data-ttu-id="ab3a1-479">Expressões de acesso de membro de dicionário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-479">Dictionary Member Access Expressions</span></span>

<span data-ttu-id="ab3a1-480">Um *expressão de acesso de membro de dicionário* é usada para pesquisar um membro de uma coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-480">A *dictionary member access expression* is used to look up a member of a collection.</span></span> <span data-ttu-id="ab3a1-481">Um acesso de membro de dicionário assume a forma de `E!I`, onde `E` é uma expressão que é classificada como um valor e `I` é um identificador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-481">A dictionary member access takes the form of `E!I`, where `E` is an expression that is classified as a value and `I` is an identifier.</span></span>

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

<span data-ttu-id="ab3a1-482">O tipo da expressão deve ter uma propriedade default indexada por um único `String` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-482">The type of the expression must have a default property indexed by a single `String` parameter.</span></span> <span data-ttu-id="ab3a1-483">A expressão de acesso de membro de dicionário `E!I` é transformado em expressão `E.D("I")`, onde `D` é a propriedade padrão do `E`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-483">The dictionary member access expression `E!I` is transformed into the expression `E.D("I")`, where `D` is the default property of `E`.</span></span> <span data-ttu-id="ab3a1-484">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-484">For example:</span></span>

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

<span data-ttu-id="ab3a1-485">Se um ponto de exclamação for especificado com nenhuma expressão, ela contenha imediatamente `With` instrução será assumida.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-485">If an exclamation point is specified with no expression, the expression from the immediately containing `With` statement is assumed.</span></span> <span data-ttu-id="ab3a1-486">Se não houver nenhum contendo `With` declaração, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-486">If there is no containing `With` statement, a compile-time error occurs.</span></span>


## <a name="invocation-expressions"></a><span data-ttu-id="ab3a1-487">Expressões de Invocação</span><span class="sxs-lookup"><span data-stu-id="ab3a1-487">Invocation Expressions</span></span>

<span data-ttu-id="ab3a1-488">Uma expressão de invocação consiste em um destino de invocação e uma lista de argumentos opcionais.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-488">An invocation expression consists of an invocation target and an optional argument list.</span></span>

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

<span data-ttu-id="ab3a1-489">A expressão de destino deve ser classificada como um grupo de método ou um valor cujo tipo é um tipo delegado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-489">The target expression must be classified as a method group or a value whose type is a delegate type.</span></span> <span data-ttu-id="ab3a1-490">Se a expressão de destino for um valor cujo tipo é um tipo de delegado, o destino da expressão de invocação se torna o grupo de método para o `Invoke` membro do tipo delegado e a expressão de destino torna-se a expressão de destino associados do método grupo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-490">If the target expression is a value whose type is a delegate type, then the target of the invocation expression becomes the method group for the `Invoke` member of the delegate type and the target expression becomes the associated target expression of the method group.</span></span>

<span data-ttu-id="ab3a1-491">Uma lista de argumentos tem duas seções: argumentos posicionais e argumentos nomeados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-491">An argument list has two sections: positional arguments and named arguments.</span></span> <span data-ttu-id="ab3a1-492">*Argumentos posicionais* são expressões e devem preceder argumentos nomeados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-492">*Positional arguments* are expressions and must precede any named arguments.</span></span> <span data-ttu-id="ab3a1-493">*Argumentos nomeados* começar com um identificador que pode corresponder a palavras-chave, seguidas por `:=` e uma expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-493">*Named arguments* start with an identifier that can match keywords, followed by `:=` and an expression.</span></span>

<span data-ttu-id="ab3a1-494">Se o grupo de método contém apenas um método acessível, incluindo métodos de instância e a extensão e esse método não leva argumentos e é uma função, em seguida, o grupo de métodos é interpretado como uma expressão de invocação com uma lista de argumentos vazia e o resultado é usado como o destino de uma expressão de invocação com as listas de argumento fornecido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-494">If the method group only contains one accessible method, including both instance and extension methods, and that method takes no arguments and is a function, then the method group is interpreted as an invocation expression with an empty argument list and the result is used as the target of an invocation expression with the provided argument list(s).</span></span> <span data-ttu-id="ab3a1-495">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-495">For example:</span></span>

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

<span data-ttu-id="ab3a1-496">Caso contrário, a resolução de sobrecarga é aplicada aos métodos para escolher o método mais aplicável para a lista de argumento fornecido (s).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-496">Otherwise, overload resolution is applied to the methods to pick the most applicable method for the given argument list(s).</span></span> <span data-ttu-id="ab3a1-497">Se o método mais aplicável é uma função, em seguida, o resultado da expressão de invocação é classificado como um valor tipado como o tipo de retorno da função.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-497">If the most applicable method is a function, then the result of the invocation expression is classified as a value typed as the return type of the function.</span></span> <span data-ttu-id="ab3a1-498">Se o método mais aplicável é uma sub-rotina, o resultado é classificado como nulas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-498">If the most applicable method is a subroutine, then the result is classified as void.</span></span> <span data-ttu-id="ab3a1-499">Se o método mais aplicável é um método parcial que não tem nenhum corpo, em seguida, a expressão de invocação é ignorada e o resultado é classificado como nulas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-499">If the most applicable method is a partial method that has no body, then the invocation expression is ignored and the result is classified as void.</span></span>

<span data-ttu-id="ab3a1-500">Para uma expressão de invocação de associação inicial, os argumentos são avaliados na ordem em que os parâmetros correspondentes são declarados no método de destino.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-500">For an early-bound invocation expression, the arguments are evaluated in the order in which the corresponding parameters are declared in the target method.</span></span> <span data-ttu-id="ab3a1-501">Uma expressão de acesso de membro de associação tardia, eles são avaliados na ordem em que aparecem na expressão de acesso de membro: consulte a seção [expressões de associação Late](expressions.md#late-bound-expressions).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-501">For a late-bound member access expression, they are evaluated in the order in which they appear in the member access expression: see Section [Late-Bound Expressions](expressions.md#late-bound-expressions).</span></span>

## <a name="overloaded-method-resolution"></a><span data-ttu-id="ab3a1-502">Método sobrecarregado de resolução:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-502">Overloaded Method Resolution:</span></span>
<span data-ttu-id="ab3a1-503">Para resolução de sobrecarga, a especificidade dos membros/tipos recebe um argumento de lista, de forma geral, a aplicabilidade à lista de argumentos, passando argumentos e argumentos de separação para parâmetros opcionais, métodos condicionais e Inferência de argumento: consulte a seção [ Resolução de sobrecarga](overload-resolution.md).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-503">For Overload Resolution, Specificity of members/types given an argument list, Genericity, Applicability to Argument List, Passing Arguments, and Picking Arguments for Optional Parameters, Conditional Methods, and Type Argument Inference: see Section [Overload Resolution](overload-resolution.md).</span></span>

## <a name="index-expressions"></a><span data-ttu-id="ab3a1-504">Expressões de índice</span><span class="sxs-lookup"><span data-stu-id="ab3a1-504">Index Expressions</span></span>

<span data-ttu-id="ab3a1-505">Uma *expressão de índice* resulta em um elemento de matriz ou reclassifica um grupo de propriedades em um acesso de propriedade.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-505">An *index expression* results in an array element or reclassifies a property group into a property access.</span></span> <span data-ttu-id="ab3a1-506">Uma expressão de índice consiste, em ordem, uma expressão, um parêntese de abertura, uma lista de argumentos de índice e um parêntese de fechamento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-506">An index expression consists of, in order, an expression, an opening parenthesis, an index argument list, and a closing parenthesis.</span></span>

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

<span data-ttu-id="ab3a1-507">O destino da expressão do índice deve ser classificado como um valor ou um grupo de propriedades.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-507">The target of the index expression must be classified as either a property group or a value.</span></span> <span data-ttu-id="ab3a1-508">Uma expressão de índice é processada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-508">An index expression is processed as follows:</span></span>

* <span data-ttu-id="ab3a1-509">Se a expressão de destino é classificada como um valor e se seu tipo não é um tipo de matriz `Object`, ou `System.Array`, o tipo deve ter uma propriedade padrão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-509">If the target expression is classified as a value and if its type is not an array type, `Object`, or `System.Array`, the type must have a default property.</span></span> <span data-ttu-id="ab3a1-510">O índice é executado em um grupo de propriedade que representa todas as propriedades padrão do tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-510">The index is performed on a property group that represents all of the default properties of the type.</span></span> <span data-ttu-id="ab3a1-511">Embora não seja válido para declarar uma propriedade sem parâmetros padrão no Visual Basic, outras linguagens podem permitir que declarar essa propriedade.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-511">Although it is not valid to declare a parameterless default property in Visual Basic, other languages may allow declaring such a property.</span></span> <span data-ttu-id="ab3a1-512">Consequentemente, a indexação de uma propriedade sem argumentos é permitida.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-512">Consequently, indexing a property with no arguments is allowed.</span></span>

* <span data-ttu-id="ab3a1-513">Se a expressão resulta em um valor de um tipo de matriz, o número de argumentos na lista de argumentos deve ser o mesmo que a classificação do tipo de matriz e não pode incluir os argumentos nomeados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-513">If the expression results in a value of an array type, the number of arguments in the argument list must be the same as the rank of the array type and may not include named arguments.</span></span> <span data-ttu-id="ab3a1-514">Se qualquer um dos índices são inválidos em tempo de execução, um `System.IndexOutOfRangeException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-514">If any of the indexes are invalid at run time, a `System.IndexOutOfRangeException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-515">Cada expressão deve ser implicitamente conversível para o tipo `Integer`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-515">Each expression must be implicitly convertible to type `Integer`.</span></span> <span data-ttu-id="ab3a1-516">O resultado da expressão do índice é a variável no índice especificado e é classificado como uma variável.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-516">The result of the index expression is the variable at the specified index and is classified as a variable.</span></span>

* <span data-ttu-id="ab3a1-517">Se a expressão é classificada como um grupo de propriedades, a resolução de sobrecarga é usada para determinar se uma das propriedades é aplicável à lista de argumentos de índice.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-517">If the expression is classified as a property group, overload resolution is used to determine whether one of the properties is applicable to the index argument list.</span></span> <span data-ttu-id="ab3a1-518">Se o grupo de propriedades contém apenas uma propriedade que tem um `Get` acessador e se esse acessador não usa nenhum argumento, o grupo de propriedades é interpretado como uma expressão de índice com uma lista de argumentos vazia.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-518">If the property group only contains one property that has a `Get` accessor and if that accessor takes no arguments, then the property group is interpreted as an index expression with an empty argument list.</span></span> <span data-ttu-id="ab3a1-519">O resultado é usado como o destino da expressão do índice atual.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-519">The result is used as the target of the current index expression.</span></span> <span data-ttu-id="ab3a1-520">Se nenhuma propriedade é aplicável, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-520">If no properties are applicable, then a compile-time error occurs.</span></span> <span data-ttu-id="ab3a1-521">Caso contrário, a expressão resulta em um acesso de propriedade com a expressão de destino associado (se houver) do grupo de propriedades.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-521">Otherwise, the expression results in a property access with the associated target expression (if any) of the property group.</span></span>

* <span data-ttu-id="ab3a1-522">Se a expressão é classificada como um grupo de propriedades de associação tardia ou como um valor cujo tipo seja `Object` ou `System.Array`, o processamento de expressão do índice é adiado até o tempo de execução e a indexação é associação tardia.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-522">If the expression is classified as a late-bound property group or as a value whose type is `Object` or `System.Array`, the processing of the index expression is deferred until run time and the indexing is late-bound.</span></span> <span data-ttu-id="ab3a1-523">Os resultados da expressão em um acesso de propriedade de associação tardia digitado como `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-523">The expression results in a late-bound property access typed as `Object`.</span></span> <span data-ttu-id="ab3a1-524">A expressão de destino associadas é a expressão de destino, se ele for um valor ou a expressão de destino associados do grupo de propriedades.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-524">The associated target expression is either the target expression, if it is a value, or the associated target expression of the property group.</span></span> <span data-ttu-id="ab3a1-525">Em tempo de execução a expressão é processada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-525">At run time the expression is processed as follows:</span></span>

* <span data-ttu-id="ab3a1-526">Se a expressão é classificada como um grupo de propriedades de associação tardia, a expressão pode resultar em um grupo de métodos, um grupo de propriedades ou um valor (se o membro for uma instância ou uma variável compartilhada).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-526">If the expression is classified as a late-bound property group, the expression may result in a method group, a property group, or a value (if the member is an instance or shared variable).</span></span> <span data-ttu-id="ab3a1-527">Se o resultado é um grupo de método ou propriedade, a resolução de sobrecarga é aplicada ao grupo para determinar o método correto para a lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-527">If the result is a method group or property group, overload resolution is applied to the group to determine the correct method for the argument list.</span></span> <span data-ttu-id="ab3a1-528">Se a resolução de sobrecarga falhar, um `System.Reflection.AmbiguousMatchException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-528">If overload resolution fails, a `System.Reflection.AmbiguousMatchException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-529">Em seguida, o resultado é processado como um acesso de propriedade ou como uma invocação e o resultado é retornado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-529">Then the result is processed either as a property access or as an invocation and the result is returned.</span></span> <span data-ttu-id="ab3a1-530">Se for a invocação de uma sub-rotina, o resultado é `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-530">If the invocation is of a subroutine, the result is `Nothing`.</span></span>

* <span data-ttu-id="ab3a1-531">Se o tipo de tempo de execução da expressão de destino é um tipo de matriz ou `System.Array`, o resultado da expressão do índice é o valor da variável no índice especificado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-531">If the run-time type of the target expression is an array type or `System.Array`, the result of the index expression is the value of the variable at the specified index.</span></span>

* <span data-ttu-id="ab3a1-532">Caso contrário, o tipo de tempo de execução da expressão deve ter uma propriedade padrão e o índice é executado no grupo de propriedade que representa todas as propriedades padrão no tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-532">Otherwise, the run-time type of the expression must have a default property and the index is performed on the property group that represents all of the default properties on the type.</span></span> <span data-ttu-id="ab3a1-533">Se o tipo não tem nenhuma propriedade padrão, em seguida, um `System.MissingMemberException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-533">If the type has no default property, then a `System.MissingMemberException` exception is thrown.</span></span>


## <a name="new-expressions"></a><span data-ttu-id="ab3a1-534">Novas expressões</span><span class="sxs-lookup"><span data-stu-id="ab3a1-534">New Expressions</span></span>

<span data-ttu-id="ab3a1-535">O `New` operador é usado para criar novas instâncias de tipos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-535">The `New` operator is used to create new instances of types.</span></span> <span data-ttu-id="ab3a1-536">Há quatro formas de `New` expressões:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-536">There are four forms of `New` expressions:</span></span>

* <span data-ttu-id="ab3a1-537">Expressões de criação do objeto são usadas para criar novas instâncias de tipos de classes e tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-537">Object-creation expressions are used to create new instances of class types and value types.</span></span>

* <span data-ttu-id="ab3a1-538">Expressões de criação de matriz são usadas para criar novas instâncias dos tipos de matriz.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-538">Array-creation expressions are used to create new instances of array types.</span></span>

* <span data-ttu-id="ab3a1-539">Expressões de criação de delegado (que não têm uma sintaxe distinta das expressões de criação de objeto) são usadas para criar novas instâncias do representante de tipos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-539">Delegate-creation expressions (which do not have a distinct syntax from object-creation expressions) are used to create new instances of delegate types.</span></span>

* <span data-ttu-id="ab3a1-540">Expressões de criação de objeto anônimos são usadas para criar novas instâncias de tipos de classe anônima.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-540">Anonymous object-creation expressions are used to create new instances of anonymous class types.</span></span>

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

<span data-ttu-id="ab3a1-541">Um `New` expressão é classificada como um valor e o resultado é a nova instância do tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-541">A `New` expression is classified as a value and the result is the new instance of the type.</span></span>


### <a name="object-creation-expressions"></a><span data-ttu-id="ab3a1-542">Expressões de criação do objeto</span><span class="sxs-lookup"><span data-stu-id="ab3a1-542">Object-Creation Expressions</span></span>

<span data-ttu-id="ab3a1-543">Uma expressão de criação de objeto é usada para criar uma nova instância de um tipo de classe ou um tipo de estrutura.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-543">An object-creation expression is used to create a new instance of a class type or a structure type.</span></span>

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

<span data-ttu-id="ab3a1-544">O tipo de uma expressão de criação do objeto deve ser um tipo de classe, um tipo de estrutura ou um parâmetro de tipo com uma `New` restrição e não pode ser um `MustInherit` classe.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-544">The type of an object creation expression must be a class type, a structure type, or a type parameter with a `New` constraint and cannot be a `MustInherit` class.</span></span> <span data-ttu-id="ab3a1-545">Considerando uma expressão de criação de objeto do formulário `New T(A)`, onde `T` é um tipo de classe ou tipo de estrutura e `A` é uma lista de argumentos opcionais, resolução de sobrecarga determina o construtor correto de `T` chamar.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-545">Given an object creation expression of the form `New T(A)`, where `T` is a class type or structure type and `A` is an optional argument list, overload resolution determines the correct constructor of `T` to call.</span></span> <span data-ttu-id="ab3a1-546">Um parâmetro de tipo com um `New` restrição é considerada como tendo um único construtor sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-546">A type parameter with a `New` constraint is considered to have a single, parameterless constructor.</span></span> <span data-ttu-id="ab3a1-547">Se nenhum construtor for chamado, ocorre um erro de tempo de compilação; Caso contrário, a expressão resulta na criação de uma nova instância da `T` usando o construtor escolhido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-547">If no constructor is callable, a compile-time error occurs; otherwise the expression results in the creation of a new instance of `T` using the chosen constructor.</span></span> <span data-ttu-id="ab3a1-548">Se não houver nenhum argumento, os parênteses podem ser omitidos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-548">If there are no arguments, the parentheses may be omitted.</span></span>

<span data-ttu-id="ab3a1-549">Em que uma instância é alocada depende se a instância é um tipo de classe ou um tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-549">Where an instance is allocated depends on whether the instance is a class type or a value type.</span></span> <span data-ttu-id="ab3a1-550">`New` instâncias de tipos de classe são criadas no heap de sistema, enquanto as novas instâncias de tipos de valor são criadas diretamente na pilha.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-550">`New` instances of class types are created on the system heap, while new instances of value types are created directly on the stack.</span></span>

<span data-ttu-id="ab3a1-551">Uma expressão de criação de objetos, opcionalmente, pode especificar uma lista de inicializadores de membro depois os argumentos de construtor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-551">An object-creation expression can optionally specify a list of member initializers after the constructor arguments.</span></span> <span data-ttu-id="ab3a1-552">Esses inicializadores de membro são prefixados com a palavra-chave `With`, e a lista de inicializadores é interpretada como se fosse no contexto de um `With` instrução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-552">These member initializers are prefixed with the keyword `With`, and the initializer list is interpreted as if it was in the context of a `With` statement.</span></span> <span data-ttu-id="ab3a1-553">Por exemplo, dada a classe:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-553">For example, given the class:</span></span>

```vb
Class Customer
    Dim Name As String
    Dim Address As String
End Class
```

<span data-ttu-id="ab3a1-554">O código:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-554">The code:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = "123 Main St." }
    End Sub
End Module
```

<span data-ttu-id="ab3a1-555">É aproximadamente equivalente a:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-555">Is roughly equivalent to:</span></span>

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

<span data-ttu-id="ab3a1-556">Cada inicializador deve especificar um nome a ser atribuído e o nome deve ser um não -`ReadOnly` variável de instância ou a propriedade do tipo que está sendo construído; o acesso de membro não será atrasado associado se o tipo que está sendo construído é `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-556">Each initializer must specify a name to assign, and the name must be a non-`ReadOnly` instance variable or property of the type being constructed; the member access will not be late bound if the type being constructed is `Object`.</span></span> <span data-ttu-id="ab3a1-557">Inicializadores não podem usar o `Key` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-557">Initializers may not use the `Key` keyword.</span></span> <span data-ttu-id="ab3a1-558">Cada membro em um tipo só pode ser inicializado uma vez.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-558">Each member in a type can only be initialized once.</span></span> <span data-ttu-id="ab3a1-559">As expressões de inicializador, no entanto, podem referir-se entre si.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-559">The initializer expressions, however, may refer to each other.</span></span> <span data-ttu-id="ab3a1-560">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-560">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="ab3a1-561">Os inicializadores são atribuídos à esquerda para a direita, portanto, se um inicializador se refere a um membro que ainda não foi inicializado, ele verá o valor que a variável de instância após o construtor ser executado:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-561">The initializers are assigned left-to-right, so if an initializer refers to a member that has not been initialized yet, it will see whatever value the instance variable after the constructor ran:</span></span>

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="ab3a1-562">Inicializadores podem ser aninhados:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-562">Initializers can be nested:</span></span>

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

<span data-ttu-id="ab3a1-563">Se o tipo que está sendo criado é um tipo de coleção e tem um método de instância nomeado `Add` (incluindo métodos de extensão e métodos compartilhados), em seguida, a expressão de criação do objeto pode especificar um inicializador de coleção, o prefixo da palavra-chave `From`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-563">If the type being created is a collection type and has an instance method named `Add` (including extension methods and shared methods), then the object-creation expression can specify a collection initializer prefixed by the keyword `From`.</span></span> <span data-ttu-id="ab3a1-564">Uma expressão de criação do objeto não é possível especificar um inicializador de membro e um inicializador de coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-564">An object-creation expression cannot specify both a member initializer and a collection initializer.</span></span> <span data-ttu-id="ab3a1-565">Cada elemento no inicializador de coleção é passado como um argumento para uma invocação do `Add` função.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-565">Each element in the collection initializer is passed as an argument to an invocation of the `Add` function.</span></span> <span data-ttu-id="ab3a1-566">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-566">For example:</span></span>

```vb
Dim list = New List(Of Integer)() From { 1, 2, 3, 4 }
```

<span data-ttu-id="ab3a1-567">equivale a:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-567">is equivalent to:</span></span>

```vb
Dim list = New List(Of Integer)()
list.Add(1)
list.Add(2)
list.Add(3)
```

<span data-ttu-id="ab3a1-568">Se um elemento é um inicializador de coleção, cada elemento do inicializador de coleção de subpropriedades será passado como um argumento individual para o `Add` função.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-568">If an element is a collection initializer itself, each element of the sub-collection initializer will be passed as an individual argument to the `Add` function.</span></span> <span data-ttu-id="ab3a1-569">Por exemplo, o seguinte:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-569">For example, the following:</span></span>

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

<span data-ttu-id="ab3a1-570">equivale a:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-570">is equivalent to:</span></span>

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

<span data-ttu-id="ab3a1-571">Essa expansão sempre é feita e só é feito um nível de profundidade; Depois disso, os inicializadores de subpropriedades são considerados literais de matriz.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-571">This expansion is always done and is only ever done one level deep; after that, sub-initializers are considered array literals.</span></span> <span data-ttu-id="ab3a1-572">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-572">For example:</span></span>

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a><span data-ttu-id="ab3a1-573">Expressões de matriz</span><span class="sxs-lookup"><span data-stu-id="ab3a1-573">Array Expressions</span></span>

<span data-ttu-id="ab3a1-574">Uma expressão de matriz é usada para criar uma nova instância de um tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-574">An array expression is used to create a new instance of an array type.</span></span> <span data-ttu-id="ab3a1-575">Há dois tipos de expressões de matriz: expressões de criação de matriz e literais de matriz.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-575">There are two types of array expressions: array creation expressions, and array literals.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="ab3a1-576">Expressões de criação de matriz</span><span class="sxs-lookup"><span data-stu-id="ab3a1-576">Array creation expressions</span></span>

<span data-ttu-id="ab3a1-577">Se for fornecido um modificador de inicialização de tamanho de matriz, o tipo de matriz resultante será derivado excluindo cada um dos argumentos individuais da lista de argumentos de inicialização do tamanho de matriz.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-577">If an array size initialization modifier is supplied, the resulting array type is derived by deleting each of the individual arguments from the array size initialization argument list.</span></span> <span data-ttu-id="ab3a1-578">O valor de cada argumento determina o limite superior da dimensão correspondente na instância de matriz recém-alocada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-578">The value of each argument determines the upper bound of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="ab3a1-579">Se a expressão tiver um inicializador de coleção vazio, cada argumento na lista de argumentos deve ser uma constante, e os comprimentos de dimensão e de classificação especificados pela lista de expressão devem corresponder do inicializador de coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-579">If the expression has a non-empty collection initializer, each argument in the argument list must be a constant, and the rank and dimension lengths specified by the expression list must match those of the collection initializer.</span></span>

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

<span data-ttu-id="ab3a1-580">Se um modificador de inicialização de tamanho de matriz não for fornecido, o nome do tipo deve ser um tipo de matriz e o inicializador de coleção deve estar vazia ou ter o mesmo número de níveis de aninhamento, como a classificação do tipo de matriz especificado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-580">If an array size initialization modifier is not supplied, then the type name must be an array type and the collection initializer must be empty or have the same number of levels of nesting as the rank of the specified array type.</span></span> <span data-ttu-id="ab3a1-581">Todos os elementos em nível de aninhamento mais interno devem ser implicitamente conversíveis para o tipo de elemento da matriz e devem ser classificados como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-581">All of the elements in the innermost nesting level must be implicitly convertible to the element type of the array and must be classified as a value.</span></span> <span data-ttu-id="ab3a1-582">O número de elementos em cada inicializador de coleção aninhada sempre deve ser consistente com o tamanho das outras coleções do mesmo nível.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-582">The number of elements in each nested collection initializer must always be consistent with the size of the other collections at the same level.</span></span> <span data-ttu-id="ab3a1-583">Os comprimentos de dimensão individuais são inferidos do número de elementos em cada um dos níveis de aninhamento correspondentes do inicializador de coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-583">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the collection initializer.</span></span> <span data-ttu-id="ab3a1-584">Se o inicializador de coleção estiver vazio, o tamanho de cada dimensão é zero.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-584">If the collection initializer is empty, the length of each dimension is zero.</span></span>

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

<span data-ttu-id="ab3a1-585">O nível de aninhamento mais externo de um inicializador de coleção corresponde à dimensão mais à esquerda de uma matriz, e o nível de aninhamento mais interno corresponde à dimensão mais à direita.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-585">The outermost nesting level of a collection initializer corresponds to the leftmost dimension of an array, and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="ab3a1-586">O Exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-586">The example:</span></span>

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

<span data-ttu-id="ab3a1-587">é equivalente ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-587">Is equivalent to the following:</span></span>

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

<span data-ttu-id="ab3a1-588">Se o inicializador de coleção está vazia (ou seja, um que contém chaves), mas nenhuma lista de inicializador e os limites do conhecidos as dimensões da matriz que está sendo inicializado, o inicializador de coleção vazia representa uma instância de matriz de tamanho especificado em que todos os elementos foram inicializados com valor de padrão do tipo de elemento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-588">If the collection initializer is empty (that is, one that contains curly braces but no initializer list) and the bounds of the dimensions of the array being initialized are known, the empty collection initializer represents an array instance of the specified size where all the elements have been initialized to the element type's default value.</span></span> <span data-ttu-id="ab3a1-589">Se os limites das dimensões da matriz que está sendo inicializado não são conhecidos, o inicializador de coleção vazia representa uma instância de matriz no qual todas as dimensões são de tamanho zero.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-589">If the bounds of the dimensions of the array being initialized are not known, the empty collection initializer represents an array instance in which all dimensions are size zero.</span></span>

<span data-ttu-id="ab3a1-590">Classificação de uma instância de matriz e o comprimento de cada dimensão são constantes para o tempo de vida inteiro da instância.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-590">An array instance's rank and length of each dimension are constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="ab3a1-591">Em outras palavras, não é possível alterar a classificação de uma instância de matriz existente, nem é possível redimensionar suas dimensões.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-591">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

#### <a name="array-literals"></a><span data-ttu-id="ab3a1-592">Literais de matriz</span><span class="sxs-lookup"><span data-stu-id="ab3a1-592">Array Literals</span></span>

<span data-ttu-id="ab3a1-593">Um literal de matriz denota uma matriz cujo tipo de elemento, classificação e os limites é inferida com uma combinação de contexto da expressão e um inicializador de coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-593">An array literal denotes an array whose element type, rank, and bounds are inferred from a combination of the expression context and a collection initializer.</span></span> <span data-ttu-id="ab3a1-594">Isso é explicado na seção [reclassificação expressão](expressions.md#expression-reclassification).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-594">This is explained in Section [Expression Reclassification](expressions.md#expression-reclassification).</span></span>

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

<span data-ttu-id="ab3a1-595">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-595">For example:</span></span>

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

<span data-ttu-id="ab3a1-596">O formato e os requisitos para o inicializador de coleção em um literal de matriz é exatamente o mesmo que para o inicializador de coleção em uma expressão de criação de matriz.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-596">The format and requirements for the collection initializer in an array literal is exactly the same as that for the collection initializer in an array creation expression.</span></span>

<span data-ttu-id="ab3a1-597">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-597">__Note.__</span></span> <span data-ttu-id="ab3a1-598">Um literal de matriz não cria a matriz por si só; em vez disso, é a reclassificação da expressão em um valor que faz com que a matriz a ser criado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-598">An array literal does not create the array in and of itself; instead, it is the reclassification of the expression into a value that causes the array to be created.</span></span> <span data-ttu-id="ab3a1-599">Por exemplo, a conversão `CType(new Integer() {1,2,3}, Short())` não é possível porque não há nenhuma conversão de `Integer()` à `Short()`; mas a expressão `CType({1,2,3},Short())` é possível porque ele reclassifica primeiro o literal de matriz para a expressão de criação de matriz `New Short() {1,2,3}`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-599">For instance, the conversion `CType(new Integer() {1,2,3}, Short())` is not possible because there is no conversion from `Integer()` to `Short()`; but the expression `CType({1,2,3},Short())` is possible because it first reclassifies the array literal into the array creation expression `New Short() {1,2,3}`.</span></span>


### <a name="delegate-creation-expressions"></a><span data-ttu-id="ab3a1-600">Expressões de criação de delegado</span><span class="sxs-lookup"><span data-stu-id="ab3a1-600">Delegate-Creation Expressions</span></span>

<span data-ttu-id="ab3a1-601">Uma expressão de criação de delegado é usada para criar uma nova instância de um tipo delegado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-601">A delegate-creation expression is used to create a new instance of a delegate type.</span></span> <span data-ttu-id="ab3a1-602">O argumento de uma expressão de criação de delegado deve ser uma expressão classificada como um ponteiro de método ou um método lambda.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-602">The argument of a delegate-creation expression must be an expression classified as a method pointer or a lambda method.</span></span>

<span data-ttu-id="ab3a1-603">Se o argumento for um ponteiro de método, um dos métodos referenciados pelo ponteiro de método deve ser aplicado à assinatura do tipo delegado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-603">If the argument is a method pointer, one of the methods referenced by the method pointer must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="ab3a1-604">Um método `M` é aplicável a um tipo de delegado `D` se:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-604">A method `M` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="ab3a1-605">`M` não é `Partial` ou tem um corpo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-605">`M` is not `Partial` or has a body.</span></span>

* <span data-ttu-id="ab3a1-606">Ambos `M` e `D` são funções, ou `D` é uma sub-rotina.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-606">Both `M` and `D` are functions, or `D` is a subroutine.</span></span>

* <span data-ttu-id="ab3a1-607">`M` e `D` têm o mesmo número de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-607">`M` and `D` have the same number of parameters.</span></span>

* <span data-ttu-id="ab3a1-608">Os tipos de parâmetro de `M` possuem uma conversão do tipo de parâmetro correspondente do `D`e os modificadores (ou seja, `ByRef`, `ByVal`) correspondem.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-608">The parameter types of `M` each have a conversion from the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="ab3a1-609">O tipo de retorno `M`, se houver, tem uma conversão para o tipo de retorno de `D`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-609">The return type of `M`, if any, has a conversion to the return type of `D`.</span></span>

<span data-ttu-id="ab3a1-610">Se o ponteiro do método faz referência a um acesso de associação tardia, em seguida, o acesso de associação tardia é considerado para uma função que tem o mesmo número de parâmetros do tipo delegado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-610">If the method pointer references a late-bound access, then the late-bound access is assumed to be to a function that has the same number of parameters as the delegate type.</span></span>

<span data-ttu-id="ab3a1-611">Se a semântica estrita não está sendo usada e há apenas um método referenciado pelo ponteiro de método, mas ela não é aplicável devido ao fato de que ele não tem nenhum parâmetro e o tipo de delegado, o método é considerado aplicável e a parâmetros ou valor de retorno um Re simplesmente ignorados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-611">If strict semantics are not being used and there is only one method referenced by the method pointer, but it is not applicable due to the fact that it has no parameters and the delegate type does, then the method is considered applicable and the parameters or return value are simply ignored.</span></span> <span data-ttu-id="ab3a1-612">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-612">For example:</span></span>

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

<span data-ttu-id="ab3a1-613">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-613">__Note.__</span></span> <span data-ttu-id="ab3a1-614">Essa atenuação só é permitida quando a semântica estrita não está sendo usada por causa de métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-614">This relaxation is only allowed when strict semantics are not being used because of extension methods.</span></span> <span data-ttu-id="ab3a1-615">Como os métodos de extensão são considerados somente se um método regular não era aplicável, é possível para um método de instância sem parâmetros para ocultar um método de extensão com parâmetros com a finalidade de construção de delegado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-615">Because extension methods are only considered if a regular method was not applicable, it is possible for an instance method with no parameters to hide an extension method with parameters for the purpose of delegate construction.</span></span>

<span data-ttu-id="ab3a1-616">Se mais de um método referenciado pelo ponteiro de método é aplicável ao tipo de delegado, em seguida, a resolução de sobrecarga é usada para escolher entre os métodos de candidato.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-616">If more than one method referenced by the method pointer is applicable to the delegate type, then overload resolution is used to pick between the candidate methods.</span></span> <span data-ttu-id="ab3a1-617">Os tipos dos parâmetros para o delegado são usados como os tipos dos argumentos para fins de resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-617">The types of the parameters to the delegate are used as the types of the arguments for the purposes of overload resolution.</span></span> <span data-ttu-id="ab3a1-618">Se nenhum candidato de um método é mais aplicável, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-618">If no one method candidate is most applicable, a compile-time error occurs.</span></span> <span data-ttu-id="ab3a1-619">No exemplo a seguir, a variável local é inicializada com um delegado que se refere à segunda `Square` método porque esse método é mais aplicável ao tipo de retorno e a assinatura de `DoubleFunc`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-619">In the following example, the local variable is initialized with a delegate that refers to the second `Square` method because that method is more applicable to the signature and return type of `DoubleFunc`.</span></span>

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

<span data-ttu-id="ab3a1-620">Tinha a segunda `Square` método não estado presente, a primeira `Square` método foi escolhido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-620">Had the second `Square` method not been present, the first `Square` method would have been chosen.</span></span> <span data-ttu-id="ab3a1-621">Se a semântica estrita é especificada pelo ambiente de compilação ou por `Option Strict`, em seguida, um erro de tempo de compilação ocorrerá se o método mais específico referenciado pelo ponteiro de método é mais estreito do que a assinatura do delegado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-621">If strict semantics are specified by the compilation environment or by `Option Strict`, then a compile-time error occurs if the most specific method referenced by the method pointer is narrower than the delegate signature.</span></span> <span data-ttu-id="ab3a1-622">Um método `M` é considerada mais estreitas do que um tipo de delegado `D` se:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-622">A method `M` is considered narrower than a delegate type `D` if:</span></span>

* <span data-ttu-id="ab3a1-623">Um tipo de parâmetro `M` tem uma conversão de ampliação para o tipo de parâmetro correspondente de `D`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-623">A parameter type of `M` has a widening conversion to the corresponding parameter type of `D`.</span></span>

* <span data-ttu-id="ab3a1-624">Ou, o tipo de retorno, se houver, do `M` tem uma conversão redutora para o tipo de retorno de `D`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-624">Or, the return type, if any, of `M` has a narrowing conversion to the return type of `D`.</span></span>

<span data-ttu-id="ab3a1-625">Se os argumentos de tipo estão associados com o ponteiro de método, apenas os métodos com o mesmo número de argumentos de tipo são considerados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-625">If type arguments are associated with the method pointer, only methods with the same number of type arguments are considered.</span></span> <span data-ttu-id="ab3a1-626">Se nenhum argumento de tipo está associado com o ponteiro de método, a inferência de tipo é usada ao fazer a correspondência de assinaturas em relação a um método genérico.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-626">If no type arguments are associated with the method pointer, type inference is used when matching signatures against a generic method.</span></span> <span data-ttu-id="ab3a1-627">Ao contrário de outra inferência de tipo normal, o tipo de retorno do delegado é usado quando inferir argumentos de tipo, mas tipos de retorno não ainda são considerados ao determinar a sobrecarga menos genérica.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-627">Unlike other normal type inference, the return type of the delegate is used when inferring type arguments, but return types are still not considered when determining the least generic overload.</span></span> <span data-ttu-id="ab3a1-628">O exemplo a seguir mostra as duas maneiras de fornecer um argumento de tipo para uma expressão de criação de delegado:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-628">The following example shows both ways of supplying a type argument to a delegate-creation expression:</span></span>

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

<span data-ttu-id="ab3a1-629">No exemplo acima, um tipo de delegado não genéricos foi instanciado usando um método genérico.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-629">In the above example, a non-generic delegate type was instantiated using a generic method.</span></span> <span data-ttu-id="ab3a1-630">Também é possível criar uma instância de um tipo de delegado construído usando um método genérico.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-630">It is also possible to create an instance of a constructed delegate type using a generic method.</span></span> <span data-ttu-id="ab3a1-631">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-631">For example:</span></span>

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

<span data-ttu-id="ab3a1-632">Se o argumento para a expressão de criação de delegado é um método lambda, o método lambda deve ser aplicado à assinatura do tipo delegado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-632">If the argument to the delegate-creation expression is a lambda method, the lambda method must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="ab3a1-633">Um método lambda `L` é aplicável a um tipo de delegado `D` se:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-633">A lambda method `L` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="ab3a1-634">Se `L` tem parâmetros, `D` tem o mesmo número de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-634">If `L` has parameters, `D` has the same number of parameters.</span></span> <span data-ttu-id="ab3a1-635">(Se `L` não tem parâmetros, os parâmetros de `D` são ignoradas.)</span><span class="sxs-lookup"><span data-stu-id="ab3a1-635">(If `L` has no parameters, the parameters of `D` are ignored.)</span></span>

* <span data-ttu-id="ab3a1-636">Os tipos de parâmetro de `L` cada uma tem uma conversão para o tipo de parâmetro correspondente do `D`e os modificadores (ou seja, `ByRef`, `ByVal`) correspondem.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-636">The parameter types of `L` each have a conversion to the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="ab3a1-637">Se `D` é uma função, o tipo de retorno `L` tem uma conversão para o tipo de retorno de `D`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-637">If `D` is a function, the return type of `L` has a conversion to the return type of `D`.</span></span> <span data-ttu-id="ab3a1-638">(Se `D` é uma sub-rotina, o valor de retorno `L` será ignorado.)</span><span class="sxs-lookup"><span data-stu-id="ab3a1-638">(If `D` is a subroutine, the return value of `L` is ignored.)</span></span>

<span data-ttu-id="ab3a1-639">Se o parâmetro de tipo de um parâmetro de `L` for omitido, em seguida, o tipo do parâmetro correspondente no `D` é inferido; se o parâmetro de `L` tem modificadores de nome que permitem valor nulo ou matriz, um erro de tempo de compilação resulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-639">If the parameter type of a parameter of `L` is omitted, then the type of the corresponding parameter in `D` is inferred; if the parameter of `L` has array or nullable name modifiers, a compile-time error results.</span></span> <span data-ttu-id="ab3a1-640">Uma vez todos os tipos de parâmetro de `L` estão disponíveis, em seguida, o tipo da expressão no método lambda é inferido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-640">Once all of the parameter types of `L` are available, then the type of the expression in the lambda method is inferred.</span></span> <span data-ttu-id="ab3a1-641">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-641">For example:</span></span>

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

<span data-ttu-id="ab3a1-642">Em algumas situações em que assinatura do delegado corresponde exatamente o método lambda ou a assinatura de método, o .NET Framework pode não oferecer suporte a criação de delegado nativamente.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-642">In some situations where delegate signature does not exactly match the lambda method or method signature, the .NET Framework may not support the delegate creation natively.</span></span> <span data-ttu-id="ab3a1-643">Nessa situação, uma expressão lambda de método é usada para corresponder os dois métodos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-643">In that situation, a lambda method expression is used to match the two methods.</span></span> <span data-ttu-id="ab3a1-644">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-644">For example:</span></span>

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

<span data-ttu-id="ab3a1-645">O resultado de uma expressão de criação de delegado é uma instância de delegado que se refere ao método de correspondência com a expressão de destino associado (se houver) da expressão de ponteiro de método.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-645">The result of a delegate-creation expression is a delegate instance that refers to the matching method with the associated target expression (if any) from the method pointer expression.</span></span> <span data-ttu-id="ab3a1-646">Se a expressão de destino é digitada como um tipo de valor, em seguida, o tipo de valor é copiado para o heap do sistema porque um delegado só pode apontar para um método de um objeto no heap.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-646">If the target expression is typed as a value type, then the value type is copied onto the system heap because a delegate can only point to a method of an object on the heap.</span></span> <span data-ttu-id="ab3a1-647">O método e o objeto ao qual se refere a um delegado permanecem constantes para o tempo de vida inteiro do delegado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-647">The method and object to which a delegate refers remain constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="ab3a1-648">Em outras palavras, não é possível alterar o destino ou o objeto de um delegado após ele ter sido criado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-648">In other words, it is not possible to change the target or object of a delegate after it has been created.</span></span>

### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="ab3a1-649">Expressões de criação de objeto anônimo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-649">Anonymous Object-Creation Expressions</span></span>

<span data-ttu-id="ab3a1-650">Uma expressão de criação de objetos com inicializadores de membro também pode omitir o nome do tipo totalmente.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-650">An object-creation expression with member initializers can also omit the type name entirely.</span></span>

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

<span data-ttu-id="ab3a1-651">Nesse caso, um tipo anônimo é construído com base nos tipos e nomes dos membros inicializados como parte da expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-651">In that case, an anonymous type is constructed based on the types and names of the members initialized as a part of the expression.</span></span> <span data-ttu-id="ab3a1-652">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-652">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

<span data-ttu-id="ab3a1-653">O tipo criado por uma expressão de criação de objeto anônimos é uma classe que não tem nome, herda diretamente de `Object`, e tem um conjunto de propriedades com o mesmo nome que os membros atribuídos a na lista de inicializadores de membro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-653">The type created by an anonymous object-creation expression is a class that has no name, inherits directly from `Object`, and has a set of properties with the same name as the members assigned to in the member initializer list.</span></span> <span data-ttu-id="ab3a1-654">O tipo de cada propriedade é inferido usando as mesmas regras como inferência de tipo de variável local.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-654">The type of each property is inferred using the same rules as local variable type inference.</span></span> <span data-ttu-id="ab3a1-655">Tipos anônimos gerados também substituem `ToString`, retornando uma representação de cadeia de caracteres de todos os membros e seus valores.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-655">Generated anonymous types also override `ToString`, returning a string representation of all members and their values.</span></span> <span data-ttu-id="ab3a1-656">(O formato exato da cadeia de caracteres está além do escopo desta especificação).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-656">(The exact format of this string is beyond the scope of this specification).</span></span>

<span data-ttu-id="ab3a1-657">Por padrão, as propriedades geradas pelo tipo anônimo são leitura / gravação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-657">By default, the properties generated by the anonymous type are read-write.</span></span> <span data-ttu-id="ab3a1-658">É possível marcar uma propriedade de tipo anônimo como somente leitura usando o `Key` modificador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-658">It is possible to mark an anonymous type property as read-only by using the `Key` modifier.</span></span> <span data-ttu-id="ab3a1-659">O `Key` modificador Especifica que o campo pode ser usado para identificar exclusivamente o valor que representa o tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-659">The `Key` modifier specifies that the field can be used to uniquely identify the value the anonymous type represents.</span></span> <span data-ttu-id="ab3a1-660">Além de tornar a propriedade somente leitura, ele também faz com que o tipo anônimo substituir `Equals` e `GetHashCode` e para implementar a interface `System.IEquatable(Of T)` (preenchendo o tipo anônimo para `T`).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-660">In addition to making the property read-only, it also causes the anonymous type to override `Equals` and  `GetHashCode` and to implement the interface `System.IEquatable(Of T)` (filling in the anonymous type for `T`).</span></span> <span data-ttu-id="ab3a1-661">Os membros são definidos da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-661">The members are defined as follows:</span></span>

<span data-ttu-id="ab3a1-662">`Function Equals(obj As Object) As Boolean` e `Function Equals(val As T) As Boolean` são implementados, validando as duas instâncias do mesmo tipo e, em seguida, comparando cada `Key` membro usando `Object.Equals`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-662">`Function Equals(obj As Object) As Boolean` and `Function Equals(val As T) As Boolean` are implemented by validating that the two instances are of the same type and then comparing each `Key` member using `Object.Equals`.</span></span> <span data-ttu-id="ab3a1-663">Se todos os `Key` membros forem iguais, então `Equals` retorna `True`; caso contrário, `Equals` retorna `False`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-663">If all `Key` members are equal, then `Equals` returns `True`, otherwise `Equals` returns `False`.</span></span>

<span data-ttu-id="ab3a1-664">`Function GetHashCode() As Integer` é implementado, de modo que que, se `Equals` é verdadeiro para duas instâncias do tipo anônimo, em seguida, `GetHashCode` retornará o mesmo valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-664">`Function GetHashCode() As Integer` is implemented such that that if `Equals` is true for two instances of the anonymous type, then `GetHashCode` will return the same value.</span></span> <span data-ttu-id="ab3a1-665">O hash começa com um valor de semente e em seguida, para cada `Key` membro, na ordem multiplica o hash por 31 e adiciona a `Key` o valor de hash do membro (fornecido pelo `GetHashCode`) se o membro não é um tipo de referência ou tipo de valor anulável com o valor de `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-665">The hash starts with a seed value and then, for each `Key` member, in order multiplies the hash by 31 and adds the `Key` member's hash value (provided by `GetHashCode`) if the member is not a reference type or nullable value type with the value of `Nothing`.</span></span>

<span data-ttu-id="ab3a1-666">Por exemplo, o tipo criado na instrução:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-666">For example, the type created in the statement:</span></span>

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

<span data-ttu-id="ab3a1-667">cria uma classe que aproximadamente tem esta aparência (embora a implementação exata pode variar):</span><span class="sxs-lookup"><span data-stu-id="ab3a1-667">creates a class that looks approximately like this (although exact implementation may vary):</span></span>

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

<span data-ttu-id="ab3a1-668">Para simplificar a situação em que um tipo anônimo é criado a partir de campos de outro tipo, os nomes de campo podem ser inferidos diretamente de expressões nos seguintes casos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-668">To simplify the situation where an anonymous type is created from the fields of another type, field names can be inferred directly from expressions in the following cases:</span></span>

* <span data-ttu-id="ab3a1-669">Uma expressão de nome simples `x` infere o nome `x`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-669">A simple name expression `x` infers the name `x`.</span></span>

* <span data-ttu-id="ab3a1-670">Uma expressão de acesso de membro `x.y` infere o nome `y`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-670">A member access expression `x.y` infers the name `y`.</span></span>

* <span data-ttu-id="ab3a1-671">Uma expressão de pesquisa de dicionário `x!y` infere o nome `y`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-671">A dictionary lookup expression `x!y` infers the name `y`.</span></span>

* <span data-ttu-id="ab3a1-672">Uma expressão de chamada ou de índice sem argumentos `x()` infere o nome `x`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-672">An invocation or index expression with no arguments `x()` infers the name `x`.</span></span>

* <span data-ttu-id="ab3a1-673">Uma expressão de acesso de membro XML `x.<y>`, `x...<y>`, `x.@y` infere o nome `y`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-673">An XML member access expression `x.<y>`, `x...<y>`, `x.@y` infers the name `y`.</span></span>

* <span data-ttu-id="ab3a1-674">Uma expressão de acesso de membro XML que é o destino de uma expressão de acesso de membro `x.<y>.z` infere o nome `z`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-674">An XML member access expression that is the target of a member access expression `x.<y>.z` infers the name `z`.</span></span>

* <span data-ttu-id="ab3a1-675">Uma expressão de acesso de membro XML que é o destino de uma expressão de chamada ou de índice sem argumentos `x.<y>.z()` infere o nome `z`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-675">An XML member access expression that is the target of an invocation or index expression with no arguments `x.<y>.z()` infers the name `z`.</span></span>

* <span data-ttu-id="ab3a1-676">Uma expressão de acesso de membro XML que é o destino de uma expressão de índice ou de invocação `x.<y>(0)` infere o nome `y`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-676">An XML member access expression that is the target of an invocation or index expression `x.<y>(0)` infers the name `y`.</span></span>

<span data-ttu-id="ab3a1-677">O inicializador é interpretado como uma atribuição da expressão para o nome deduzido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-677">The initializer is interpreted as an assignment of the expression to the inferred name.</span></span> <span data-ttu-id="ab3a1-678">Por exemplo, os inicializadores a seguir são equivalentes:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-678">For example, the following initializers are equivalent:</span></span>

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

<span data-ttu-id="ab3a1-679">Se um nome de membro é inferido, que está em conflito com um membro existente do tipo, como `GetHashCode`, em seguida, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-679">If a member name is inferred that conflicts with an existing member of the type, such as `GetHashCode`, then a compile time error occurs.</span></span> <span data-ttu-id="ab3a1-680">Ao contrário de inicializadores de membro regular, expressões de criação de objeto anônimos não permitem membro inicializadores ter referências circulares, ou para fazer referência a um membro antes que ela foi inicializada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-680">Unlike regular member initializers, anonymous object-creation expressions do not allow member initializers to have circular references, or to refer to a member before it has been initialized.</span></span> <span data-ttu-id="ab3a1-681">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-681">For example:</span></span>

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

<span data-ttu-id="ab3a1-682">Se duas expressões de criação de classe anônima ocorrem dentro do mesmo método e produzem a mesma forma resultante – se a ordem das propriedades, nomes de propriedade e propriedade tipos correspondem todos – eles serão ambos se referem à mesma classe anônima.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-682">If two anonymous class creation expressions occur within the same method and yield the same resulting shape -- if the property order, property names, and property types all match -- they will both refer to the same anonymous class.</span></span> <span data-ttu-id="ab3a1-683">O escopo do método de uma instância ou uma variável com um inicializador de membro compartilhado é o construtor no qual a variável é inicializada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-683">The method scope of an instance or shared member variable with an initializer is the constructor in which the variable is initialized.</span></span>

<span data-ttu-id="ab3a1-684">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-684">__Note.__</span></span> <span data-ttu-id="ab3a1-685">É possível que um compilador pode optar por unificar anônimo tipos Além disso, tal como no nível de assembly, mas isso não pode ser usado no momento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-685">It is possible that a compiler may choose to unify anonymous types further, such as at the assembly level, but this cannot be relied upon at this time.</span></span>


## <a name="cast-expressions"></a><span data-ttu-id="ab3a1-686">Expressões de conversão</span><span class="sxs-lookup"><span data-stu-id="ab3a1-686">Cast Expressions</span></span>

<span data-ttu-id="ab3a1-687">Uma expressão de conversão converte uma expressão em um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-687">A cast expression coerces an expression to a given type.</span></span> <span data-ttu-id="ab3a1-688">Palavras-chave de conversão específica forçar expressões em tipos primitivos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-688">Specific cast keywords coerce expressions into the primitive types.</span></span> <span data-ttu-id="ab3a1-689">Três palavras-chave cast geral, `CType`, `TryCast` e `DirectCast`, força uma expressão em um tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-689">Three general cast keywords, `CType`, `TryCast` and `DirectCast`, coerce an expression into a type.</span></span>

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

<span data-ttu-id="ab3a1-690">`DirectCast` e `TryCast` ter comportamentos especiais.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-690">`DirectCast` and `TryCast` have special behaviors.</span></span> <span data-ttu-id="ab3a1-691">Por isso, eles suportam apenas conversões nativas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-691">Because of this, they only support native conversions.</span></span> <span data-ttu-id="ab3a1-692">Além disso, o tipo de destino em um `TryCast` expressão não pode ser um tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-692">Additionally, the target type in a `TryCast` expression cannot be a value type.</span></span> <span data-ttu-id="ab3a1-693">Operadores de conversão não são considerados quando definidos pelo usuário `DirectCast` ou `TryCast` é usado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-693">User-defined conversion operators are not considered when `DirectCast` or `TryCast` is used.</span></span> <span data-ttu-id="ab3a1-694">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-694">(__Note.__</span></span> <span data-ttu-id="ab3a1-695">A conversão definida que `DirectCast` e `TryCast` suporte são restritas porque eles implementam conversões "CLR nativo".</span><span class="sxs-lookup"><span data-stu-id="ab3a1-695">The conversion set that `DirectCast` and `TryCast` support are restricted because they implement "native CLR" conversions.</span></span> <span data-ttu-id="ab3a1-696">A finalidade `DirectCast` é fornecer a funcionalidade da instrução "conversão unboxing", enquanto a finalidade de `TryCast` é fornecer a funcionalidade da instrução "isinst".</span><span class="sxs-lookup"><span data-stu-id="ab3a1-696">The purpose of `DirectCast` is to provide the functionality of the "unbox" instruction, while the purpose of `TryCast` is to provide the functionality of the "isinst" instruction.</span></span> <span data-ttu-id="ab3a1-697">Uma vez que eles mapeiam para as instruções do CLR, oferecer suporte a conversões não é diretamente compatível com o CLR acabaria com a finalidade pretendida.)</span><span class="sxs-lookup"><span data-stu-id="ab3a1-697">Since they map onto CLR instructions, supporting conversions not directly supported by the CLR would defeat the intended purpose.)</span></span>

<span data-ttu-id="ab3a1-698">`DirectCast` Converte as expressões que são digitadas como `Object` diferentemente `CType`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-698">`DirectCast` converts expressions that are typed as `Object` differently than `CType`.</span></span> <span data-ttu-id="ab3a1-699">Ao converter uma expressão do tipo `Object` cujo tipo de tempo de execução é um tipo de valor primitivo `DirectCast` lança uma `System.InvalidCastException` exceção se o tipo especificado não é igual ao tipo de tempo de execução da expressão ou um `System.NullReferenceException` se a expressão é avaliada como `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-699">When converting an expression of type `Object` whose run-time type is a primitive value type, `DirectCast` throws a `System.InvalidCastException` exception if the specified type is not the same as the run-time type of the expression or a `System.NullReferenceException` if the expression evaluates to `Nothing`.</span></span> <span data-ttu-id="ab3a1-700">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-700">(__Note.__</span></span> <span data-ttu-id="ab3a1-701">Conforme observado acima, `DirectCast` mapeia diretamente para a instrução CLR "conversão unboxing" quando o tipo da expressão é `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-701">As noted above, `DirectCast` maps directly onto the CLR instruction "unbox" when the type of the expression is `Object`.</span></span> <span data-ttu-id="ab3a1-702">Em contraste, `CType` se transforma em uma chamada para um auxiliar de tempo de execução para fazer a conversão para que as conversões entre tipos primitivos podem ter suporte.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-702">In contrast, `CType` turns into a call to a runtime helper to do the conversion so that conversions between primitive types can be supported.</span></span> <span data-ttu-id="ab3a1-703">No caso de quando um `Object` expressão está sendo convertida em um tipo de valor primitivo e o tipo de correspondência de instância real do tipo de destino, `DirectCast` será consideravelmente mais rápido que `CType`.)</span><span class="sxs-lookup"><span data-stu-id="ab3a1-703">In the case when an `Object` expression is being converted to a primitive value type and the type of the actual instance match the target type, `DirectCast` will be significantly faster than `CType`.)</span></span>

<span data-ttu-id="ab3a1-704">`TryCast` converte expressões, mas não lançará uma exceção se a expressão não pode ser convertida para o tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-704">`TryCast` converts expressions but does not throw an exception if the expression cannot be converted to the target type.</span></span> <span data-ttu-id="ab3a1-705">Em vez disso, `TryCast` resultará em `Nothing` se a expressão não pode ser convertida em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-705">Instead, `TryCast` will result in `Nothing` if the expression cannot be converted at runtime.</span></span> <span data-ttu-id="ab3a1-706">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-706">(__Note.__</span></span> <span data-ttu-id="ab3a1-707">Conforme observado acima, `TryCast` mapeia diretamente para a instrução de CLR "isinst".</span><span class="sxs-lookup"><span data-stu-id="ab3a1-707">As noted above, `TryCast` maps directly onto the CLR instruction "isinst".</span></span> <span data-ttu-id="ab3a1-708">Ao combinar a verificação de tipo e a conversão em uma única operação, `TryCast` pode ser mais barato do que fazer uma `TypeOf ... Is` e, em seguida, um `CType`.)</span><span class="sxs-lookup"><span data-stu-id="ab3a1-708">By combining the type check and the conversion into a single operation, `TryCast` can be cheaper than doing a `TypeOf ... Is` and then a `CType`.)</span></span>

<span data-ttu-id="ab3a1-709">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-709">For example:</span></span>

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

<span data-ttu-id="ab3a1-710">Se não existe conversão do tipo da expressão no tipo especificado, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-710">If no conversion exists from the type of the expression to the specified type, a compile-time error occurs.</span></span> <span data-ttu-id="ab3a1-711">Caso contrário, a expressão é classificada como um valor e o resultado é o valor produzido pela conversão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-711">Otherwise, the expression is classified as a value and the result is the value produced by the conversion.</span></span>


## <a name="operator-expressions"></a><span data-ttu-id="ab3a1-712">Expressões do operador</span><span class="sxs-lookup"><span data-stu-id="ab3a1-712">Operator Expressions</span></span>

<span data-ttu-id="ab3a1-713">Há dois tipos de operadores.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-713">There are two kinds of operators.</span></span> <span data-ttu-id="ab3a1-714">*Operadores unários* usam um operando e usar a notação de prefixo (por exemplo, `-x`).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-714">*Unary operators* take one operand and use prefix notation (for example, `-x`).</span></span> <span data-ttu-id="ab3a1-715">*Operadores binários* usam dois operandos e usar a notação de infixo (por exemplo, `x + y`).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-715">*Binary operators* take two operands and use infix notation (for example, `x + y`).</span></span> <span data-ttu-id="ab3a1-716">Com exceção dos operadores relacionais, que sempre resultam em `Boolean`, um operador definido para um tipo específico de resultados nesse tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-716">With the exception of the relational operators, which always result in `Boolean`, an operator defined for a particular type results in that type.</span></span> <span data-ttu-id="ab3a1-717">Os operandos para um operador sempre devem ser classificados como um valor. o resultado de uma expressão do operador é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-717">The operands to an operator must always be classified as a value; the result of an operator expression is classified as a value.</span></span>

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

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="ab3a1-718">Precedência e associatividade do operador</span><span class="sxs-lookup"><span data-stu-id="ab3a1-718">Operator Precedence and Associativity</span></span>

<span data-ttu-id="ab3a1-719">Quando uma expressão contiver vários operadores binários, o *precedência* dos operadores controla a ordem na qual os operadores binários individuais são avaliados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-719">When an expression contains multiple binary operators, the *precedence* of the operators controls the order in which the individual binary operators are evaluated.</span></span> <span data-ttu-id="ab3a1-720">Por exemplo, a expressão `x + y * z` é avaliada como `x + (y * z)` porque o operador `*` tem precedência maior do que o operador `+`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-720">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span> <span data-ttu-id="ab3a1-721">A tabela a seguir lista os operadores binários em ordem decrescente de precedência:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-721">The following table lists the binary operators in descending order of precedence:</span></span>


| <span data-ttu-id="ab3a1-722">__Categoria__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-722">__Category__</span></span>     | <span data-ttu-id="ab3a1-723">__Operadores__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-723">__Operators__</span></span>                                          | 
|------------------|--------------------------------------------------------|
| <span data-ttu-id="ab3a1-724">Primária</span><span class="sxs-lookup"><span data-stu-id="ab3a1-724">Primary</span></span>          | <span data-ttu-id="ab3a1-725">Todas as expressões de não-operador</span><span class="sxs-lookup"><span data-stu-id="ab3a1-725">All non-operator expressions</span></span>                           |
| <span data-ttu-id="ab3a1-726">Await</span><span class="sxs-lookup"><span data-stu-id="ab3a1-726">Await</span></span>            | `Await`                                                |
| <span data-ttu-id="ab3a1-727">Exponenciação</span><span class="sxs-lookup"><span data-stu-id="ab3a1-727">Exponentiation</span></span>   | `^`                                                    |
| <span data-ttu-id="ab3a1-728">Negação unária</span><span class="sxs-lookup"><span data-stu-id="ab3a1-728">Unary negation</span></span>   | <span data-ttu-id="ab3a1-729">`+`, `-`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-729">`+`, `-`</span></span>                                               |
| <span data-ttu-id="ab3a1-730">Multiplicativo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-730">Multiplicative</span></span>   | <span data-ttu-id="ab3a1-731">`*`, `/`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-731">`*`, `/`</span></span>                                               |
| <span data-ttu-id="ab3a1-732">Divisão de inteiros</span><span class="sxs-lookup"><span data-stu-id="ab3a1-732">Integer division</span></span> | `\`                                                    |
| <span data-ttu-id="ab3a1-733">Módulo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-733">Modulus</span></span>          | `Mod`                                                  |
| <span data-ttu-id="ab3a1-734">Aditivo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-734">Additive</span></span>         | <span data-ttu-id="ab3a1-735">`+`, `-`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-735">`+`, `-`</span></span>                                               |
| <span data-ttu-id="ab3a1-736">Concatenação</span><span class="sxs-lookup"><span data-stu-id="ab3a1-736">Concatenation</span></span>    | `&`                                                    |
| <span data-ttu-id="ab3a1-737">Shift</span><span class="sxs-lookup"><span data-stu-id="ab3a1-737">Shift</span></span>            | <span data-ttu-id="ab3a1-738">`<<`, `>>`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-738">`<<`, `>>`</span></span>                                             |
| <span data-ttu-id="ab3a1-739">Relacional</span><span class="sxs-lookup"><span data-stu-id="ab3a1-739">Relational</span></span>       | <span data-ttu-id="ab3a1-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span></span> |
| <span data-ttu-id="ab3a1-741">NOT lógico</span><span class="sxs-lookup"><span data-stu-id="ab3a1-741">Logical NOT</span></span>      | `Not`                                                  |
| <span data-ttu-id="ab3a1-742">AND lógico</span><span class="sxs-lookup"><span data-stu-id="ab3a1-742">Logical AND</span></span>      | <span data-ttu-id="ab3a1-743">`And`, `AndAlso`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-743">`And`, `AndAlso`</span></span>                                       |
| <span data-ttu-id="ab3a1-744">OR lógico</span><span class="sxs-lookup"><span data-stu-id="ab3a1-744">Logical OR</span></span>       | <span data-ttu-id="ab3a1-745">`Or`, `OrElse`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-745">`Or`, `OrElse`</span></span>                                         |
| <span data-ttu-id="ab3a1-746">XOR lógico</span><span class="sxs-lookup"><span data-stu-id="ab3a1-746">Logical XOR</span></span>      | `Xor`                                                  |

<span data-ttu-id="ab3a1-747">Quando uma expressão contém dois operadores com a mesma precedência, o *associatividade* dos operadores controla a ordem na qual as operações são executadas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-747">When an expression contains two operators with the same precedence, the *associativity* of the operators controls the order in which the operations are performed.</span></span> <span data-ttu-id="ab3a1-748">Todos os operadores binários são associativos à esquerda, o que significa que operações são executadas da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-748">All binary operators are left-associative, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="ab3a1-749">Precedência e associatividade podem ser controladas usando expressões entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-749">Precedence and associativity can be controlled using parenthetical expressions.</span></span>

### <a name="object-operands"></a><span data-ttu-id="ab3a1-750">Operandos de objeto</span><span class="sxs-lookup"><span data-stu-id="ab3a1-750">Object Operands</span></span>

<span data-ttu-id="ab3a1-751">Além dos tipos regulares compatíveis com cada operador, todos os operadores de suportam a operandos do tipo `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-751">In addition to the regular types supported by each operator, all operators support operands of type `Object`.</span></span> <span data-ttu-id="ab3a1-752">Operadores aplicadas a `Object` operandos são tratados da mesma forma para chamadas de método feitas no `Object` valores: uma chamada de método de associação tardia pode ser escolhido, caso em que o tipo de tempo de execução dos operandos, em vez do tipo de tempo de compilação, determina a validade e o tipo de operação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-752">Operators applied to `Object` operands are handled similarly to method calls made on `Object` values: a late-bound method call might be chosen, in which case the run-time type of the operands, rather than the compile-time type, determines the validity and type of the operation.</span></span> <span data-ttu-id="ab3a1-753">Se a semântica estrita é especificada pelo ambiente de compilação ou pelo `Option Strict`, os operadores com operandos do tipo `Object` causa um erro de tempo de compilação, exceto para o `TypeOf...Is`, `Is` e `IsNot` operadores.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-753">If strict semantics are specified by the compilation environment or by `Option Strict`, any operators with operands of type `Object` cause a compile-time error, except for the `TypeOf...Is`, `Is` and `IsNot` operators.</span></span>

<span data-ttu-id="ab3a1-754">Quando a resolução de operador determina que uma operação deve ser realizada associação tardia, o resultado da operação é o resultado da aplicação do operador para os tipos de operando, se os tipos de tempo de execução dos operandos são tipos que são suportados pelo operador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-754">When operator resolution determines that an operation should be performed late-bound, the outcome of the operation is the result of applying the operator to the operand types if the run-time types of the operands are types that are supported by the operator.</span></span> <span data-ttu-id="ab3a1-755">O valor `Nothing` é tratado como o valor padrão do tipo do operando em uma expressão de operador binário.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-755">The value `Nothing` is treated as the default value of the type of the other operand in a binary operator expression.</span></span> <span data-ttu-id="ab3a1-756">Em uma expressão de operador unário, ou se ambos os operandos forem `Nothing` em uma expressão de operador binário, é o tipo de operação `Integer` ou o único tipo de resultado do operador, se o operador não resulta em `Integer`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-756">In a unary operator expression, or if both operands are `Nothing` in a binary operator expression, the type of the operation is `Integer` or the only result type of the operator, if the operator does not result in `Integer`.</span></span> <span data-ttu-id="ab3a1-757">O resultado da operação, em seguida, sempre é convertido para `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-757">The result of the operation is always then cast back to `Object`.</span></span> <span data-ttu-id="ab3a1-758">Se os tipos de operando não tem nenhum operador válido, um `System.InvalidCastException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-758">If the operand types have no valid operator, a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-759">Conversões em tempo de execução são realizadas sem levar em consideração se elas são implícitas ou explícitas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-759">Conversions at run time are done without regard to whether they are implicit or explicit.</span></span>

<span data-ttu-id="ab3a1-760">Se o resultado de uma operação binária numérico produza uma exceção de estouro (independentemente se a verificação de estouro de inteiro está ativado ou desativado), em seguida, o tipo de resultado será promovido para o próximo tipo numérico mais amplo, se possível.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-760">If the result of a numeric binary operation would produce an overflow exception (regardless of whether integer overflow checking is on or off), then the result type is promoted to the next wider numeric type, if possible.</span></span> <span data-ttu-id="ab3a1-761">Por exemplo, considere o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-761">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

<span data-ttu-id="ab3a1-762">Imprime o seguinte resultado:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-762">It prints the following result:</span></span>

```
System.Int16 = 512
```

<span data-ttu-id="ab3a1-763">Se nenhum tipo numérico maior está disponível para manter o número, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-763">If no wider numeric type is available to hold the number, a `System.OverflowException` exception is thrown.</span></span>

### <a name="operator-resolution"></a><span data-ttu-id="ab3a1-764">Operador de resolução</span><span class="sxs-lookup"><span data-stu-id="ab3a1-764">Operator Resolution</span></span>

<span data-ttu-id="ab3a1-765">Dado um tipo de operador e um conjunto de operandos, resolução de operador determina qual operador a ser usado para os operandos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-765">Given an operator type and a set of operands, operator resolution determines which operator to use for the operands.</span></span> <span data-ttu-id="ab3a1-766">Durante a resolução de operadores, operadores definidos pelo usuário serão considerados primeiro, usando as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-766">When resolving operators, user-defined operators will be considered first, using the following steps:</span></span>

1. <span data-ttu-id="ab3a1-767">Primeiro, todos os operadores de candidato são coletados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-767">First, all of the candidate operators are collected.</span></span> <span data-ttu-id="ab3a1-768">Os operadores de candidato são todos os operadores definidos pelo usuário do tipo de operador específico no tipo de origem e todos os operadores definidos pelo usuário do tipo determinado no tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-768">The candidate operators are all of the user-defined operators of the particular operator type in the source type and all of the user-defined operators of the particular type in the target type.</span></span> <span data-ttu-id="ab3a1-769">Se o tipo de origem e o tipo de destino estiverem relacionadas, os operadores comuns somente são considerados uma vez.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-769">If the source type and destination type are related, common operators are only considered once.</span></span>

2. <span data-ttu-id="ab3a1-770">Em seguida, a resolução de sobrecarga é aplicada para os operadores e operandos para selecionar o operador mais específico.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-770">Then, overload resolution is applied to the operators and operands to select the most specific operator.</span></span> <span data-ttu-id="ab3a1-771">No caso de operadores binários, isso pode resultar em uma chamada de associação tardia.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-771">In the case of binary operators, this may result in a late-bound call.</span></span>

<span data-ttu-id="ab3a1-772">Ao coletar os operadores de candidato para um tipo `T?`, os operadores de tipo `T` são usados em vez disso.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-772">When collecting the candidate operators for a type `T?`, the operators of type `T` are used instead.</span></span> <span data-ttu-id="ab3a1-773">Qualquer um dos `T`do definido pelo usuário que envolvem tipos de valor não nulo somente operadores também são elevadas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-773">Any of `T`'s user-defined operators that involve only non-nullable value types are also lifted.</span></span> <span data-ttu-id="ab3a1-774">Um operador elevado usa a versão que permite valor nula de qualquer tipo de valor com a exceção os tipos de retorno de `IsTrue` e `IsFalse` (que deve ser `Boolean`).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-774">A lifted operator uses the nullable version of any value types, with the exception the return types of `IsTrue` and `IsFalse` (which must be `Boolean`).</span></span> <span data-ttu-id="ab3a1-775">Elevadas operadores são avaliados, convertendo os operandos para sua versão não anuláveis, em seguida, avaliando o operador definido pelo usuário e, em seguida, converter o resultado de tipo para a versão que permite valor nula.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-775">Lifted operators are evaluated by converting the operands to their non-nullable version, then evaluating the user-defined operator and then converting the result type to its nullable version.</span></span> <span data-ttu-id="ab3a1-776">Se estiver operando ether `Nothing`, o resultado da expressão é um valor de `Nothing` digitado como a versão que permite valor nula do tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-776">If ether operand is `Nothing`, the result of the expression is a value of `Nothing` typed as the nullable version of the result type.</span></span> <span data-ttu-id="ab3a1-777">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-777">For example:</span></span>

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

<span data-ttu-id="ab3a1-778">Se o operador é um operador binário e um dos operandos é o tipo de referência, o operador também é elevado, mas nenhuma associação para o operador produz um erro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-778">If the operator is a binary operator and one of the operands is reference type, the operator is also lifted, but any binding to the operator produces an error.</span></span> <span data-ttu-id="ab3a1-779">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-779">For example:</span></span>

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

<span data-ttu-id="ab3a1-780">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-780">__Note.__</span></span> <span data-ttu-id="ab3a1-781">Essa regra existe porque tem havido consideração se desejamos adicionar tipos de referência de propagação nula em uma versão futura, no qual o comportamento no caso de operadores binários entre os dois tipos de caso seria alterado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-781">This rule exists because there has been consideration whether we wish to add null-propagating reference types in a future version, in which case the behavior in the case of binary operators between the two types would change.</span></span>

<span data-ttu-id="ab3a1-782">Assim como acontece com conversões, operadores definidos pelo usuário sempre têm preferência sobre operadores canceladas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-782">As with conversions, user-defined operators are always preferred over lifted operators.</span></span>

<span data-ttu-id="ab3a1-783">Quando a resolução de operadores sobrecarregados, pode haver diferenças entre as classes definidas no Visual Basic e aquelas definidas em outras linguagens:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-783">When resolving overloaded operators, there may be differences between classes defined in Visual Basic and those defined in other languages:</span></span>

* <span data-ttu-id="ab3a1-784">Em outras linguagens `Not`, `And`, e `Or` pode estar sobrecarregado como operadores lógicos e operadores bit a bit.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-784">In other languages, `Not`, `And`, and `Or` may be overloaded both as logical operators and bitwise operators.</span></span> <span data-ttu-id="ab3a1-785">Após a importação de um assembly externo, o formulário será aceito como uma sobrecarga válida para usar esses operadores.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-785">Upon import from an external assembly, either form is accepted as a valid overload for these operators.</span></span> <span data-ttu-id="ab3a1-786">No entanto, para um tipo que define operadores lógicos e bit a bit, apenas a implementação de bit a bit será considerada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-786">However, for a type which defines both logical and bitwise operators, only the bitwise implementation will be considered.</span></span>

* <span data-ttu-id="ab3a1-787">Em outras linguagens `>>` e `<<` pode estar sobrecarregado como operadores com sinal e sem sinal de operadores.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-787">In other languages, `>>` and `<<` may be overloaded both as signed operators and unsigned operators.</span></span> <span data-ttu-id="ab3a1-788">Após a importação de um assembly externo, o formulário será aceito como uma sobrecarga válida.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-788">Upon import from an external assembly, either form is accepted as a valid overload.</span></span> <span data-ttu-id="ab3a1-789">No entanto, para um tipo que define operadores assinados e não assinados, apenas a implementação de com sinal será considerada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-789">However, for a type which defines both signed and unsigned operators, only the signed implementation will be considered.</span></span>

* <span data-ttu-id="ab3a1-790">Se nenhum operador definido pelo usuário é o mais específico aos operandos, serão considerados operadores intrínseco.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-790">If no user-defined operator is most specific to the operands, then intrinsic operators will be considered.</span></span> <span data-ttu-id="ab3a1-791">Se nenhum operador intrínseco está definido para os operandos e qualquer operando tem o tipo de objeto, em seguida, o operador será resolvido tardia; Caso contrário, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-791">If no intrinsic operator is defined for the operands and either operand has type Object then the operator will be resolved late-bound; otherwise,  a compile-time error results.</span></span>

<span data-ttu-id="ab3a1-792">Em versões anteriores do Visual Basic, se houve exatamente um operando do tipo objeto e nenhum operador aplicável de definidas pelo usuário e nenhum operador intrínseco aplicável, em seguida, ele foi um erro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-792">In prior versions of Visual Basic, if there was exactly one operand of type Object, and no applicable user-defined operators, and no applicable intrinsic operators, then it was an error.</span></span> <span data-ttu-id="ab3a1-793">A partir de 11 de Visual Basic, agora ele está resolvido associação tardia.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-793">As of Visual Basic 11, it is now resolved late-bound.</span></span> <span data-ttu-id="ab3a1-794">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-794">For example:</span></span>

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

<span data-ttu-id="ab3a1-795">Um tipo `T` que tem um intrínseco operador também define esse mesmo operador para `T?`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-795">A type `T` that has an intrinsic operator also defines that same operator for `T?`.</span></span> <span data-ttu-id="ab3a1-796">O resultado do operador no `T?` será o mesmo que para `T`, exceto que se qualquer operando for `Nothing`, o resultado do operador será `Nothing` (ou seja, o valor nulo é propagado).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-796">The result of the operator on `T?` will be the same as for `T`, except that if either operand is `Nothing`, the result of the operator will be `Nothing` (i.e. the null value is propagated).</span></span> <span data-ttu-id="ab3a1-797">Para fins de resolver o tipo de uma operação, o `?` é removido de qualquer operandos que tenham-los, o tipo de operação é determinado e um `?` é adicionado para o tipo da operação se qualquer um dos operandos eram tipos de valor anulável.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-797">For the purposes of resolving the type of an operation, the `?` is removed from any operands that have them, the type of the operation is determined, and a `?` is added to the type of the operation if any of the operands were nullable value types.</span></span> <span data-ttu-id="ab3a1-798">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-798">For example:</span></span>

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

<span data-ttu-id="ab3a1-799">Cada operador lista os tipos intrínsecos, para que ele é definido e do tipo de operação executada considerando os tipos de operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-799">Each operator lists the intrinsic types it is defined for and the type of the operation performed given the operand types.</span></span> <span data-ttu-id="ab3a1-800">O resultado do tipo de uma operação intrínseco segue estas regras gerais:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-800">The result of type of a intrinsic operation follows these general rules:</span></span>

* <span data-ttu-id="ab3a1-801">Se todos os operandos forem do mesmo tipo e o operador está definido para o tipo, não ocorre nenhuma conversão, e o operador para esse tipo é usado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-801">If all operands are of the same type, and the operator is defined for the type, then no conversion occurs and the operator for that type is used.</span></span>

* <span data-ttu-id="ab3a1-802">Qualquer operando cujo tipo não está definido para o operador é convertido usando as seguintes etapas e o operador é resolvido em relação a novos tipos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-802">Any operand whose type is not defined for the operator is converted using the following steps and the operator is resolved against the new types:</span></span>

  * <span data-ttu-id="ab3a1-803">O operando é convertido para o próximo maior tipo que está definido para o operador e o operando, e para que ele é convertido implicitamente.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-803">The operand is converted to the next widest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="ab3a1-804">Se não houver nenhum tal tipo, o operando é convertido para o tipo do próximo mais restrito que é definido para o operador e o operando e para que ele é convertido implicitamente.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-804">If there is no such type, then the operand is converted to the next narrowest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="ab3a1-805">Se houver nenhum tal tipo ou a conversão não pode ocorrer, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-805">If there is no such type or the conversion cannot occur, a compile-time error occurs.</span></span>

* <span data-ttu-id="ab3a1-806">Caso contrário, os operandos são convertidos para o maior dos tipos de operando e o operador para esse tipo é usado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-806">Otherwise, the operands are converted to the wider of the operand types and the operator for that type is used.</span></span> <span data-ttu-id="ab3a1-807">Se o tipo de operando mais restrito não pode ser convertido implicitamente no tipo de operador mais amplo, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-807">If the narrower operand type cannot be implicitly converted to the wider operator type, a compile-time error occurs.</span></span>

<span data-ttu-id="ab3a1-808">Apesar dessas regras gerais, no entanto, há um número de casos especiais, indicadas nas tabelas de resultados do operador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-808">Despite these general rules, however, there are a number of special cases called out in the operator results tables.</span></span>

<span data-ttu-id="ab3a1-809">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-809">__Note.__</span></span> <span data-ttu-id="ab3a1-810">Por motivos de formatação, as tabelas do tipo de operador abreviar nomes predefinidos para os dois primeiros caracteres.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-810">For formatting reasons, the operator type tables abbreviate the predefined names to their first two characters.</span></span> <span data-ttu-id="ab3a1-811">Então, "por" é `Byte`, é "Interface do usuário" `UInteger`, é "St" `String`, etc. "Err" significa que há uma operação definida para os tipos de operando determinado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-811">So "By" is `Byte`, "UI" is `UInteger`, "St" is `String`, etc. "Err" means that there is no operation defined for the given operand types.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="ab3a1-812">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="ab3a1-812">Arithmetic Operators</span></span>

<span data-ttu-id="ab3a1-813">O `*`, `/`, `\`, `^`, `Mod`, `+`, e `-` operadores são os *operadores aritméticos*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-813">The `*`, `/`, `\`, `^`, `Mod`, `+`, and `-` operators are the *arithmetic operators*.</span></span>

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

<span data-ttu-id="ab3a1-814">Operações de aritméticas de ponto flutuante podem ser executadas com precisão maior do que o tipo de resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-814">Floating-point arithmetic operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="ab3a1-815">Por exemplo, algumas arquiteturas de hardware oferecem suporte a um tipo de ponto flutuante "extended" ou "long double" com o maior intervalo e a precisão do que o `Double` digite e implicitamente executar todas as operações de ponto flutuantes usando esse tipo de precisão mais alta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-815">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `Double` type, and implicitly perform all floating-point operations using this higher-precision type.</span></span> <span data-ttu-id="ab3a1-816">Arquiteturas de hardware podem ser feitas para realizar operações de ponto flutuante com menos precisão somente custo excessiva no desempenho. em vez de exigir uma implementação perder o desempenho e precisão, o Visual Basic permite que o tipo de maior precisão a ser usado para todas as operações de ponto flutuantes.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-816">Hardware architectures can be made to perform floating-point operations with less precision only at excessive cost in performance; rather than require an implementation to forfeit both performance and precision, Visual Basic allows the higher-precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="ab3a1-817">Além de fornecer resultados mais precisos, isso raramente tem efeitos mensuráveis.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-817">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="ab3a1-818">No entanto, em expressões do formulário `x * y / z`, em que a multiplicação produz um resultado que está fora os `Double` intervalo, mas a divisão subsequente leva o resultados temporários de volta para o `Double` de intervalo, o fato de que a expressão é avaliadas em um intervalo maior-formato pode causar um resultado finito para ser produzidos em vez de infinito.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-818">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `Double` range, but the subsequent division brings the temporary result back into the `Double` range, the fact that the expression is evaluated in a higher-range format may cause a finite result to be produced instead of infinity.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="ab3a1-819">Operador de adição unário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-819">Unary Plus Operator</span></span>

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

<span data-ttu-id="ab3a1-820">O operador unário de adição é definido para o `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Single`, `Double`, e `Decimal` tipos .</span><span class="sxs-lookup"><span data-stu-id="ab3a1-820">The unary plus operator is defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Single`, `Double`, and `Decimal` types.</span></span>

<span data-ttu-id="ab3a1-821">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-821">__Operation Type:__</span></span>


| <span data-ttu-id="ab3a1-822">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-822">__Bo__</span></span> | <span data-ttu-id="ab3a1-823">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-823">__SB__</span></span> | <span data-ttu-id="ab3a1-824">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-824">__By__</span></span> | <span data-ttu-id="ab3a1-825">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-825">__Sh__</span></span> | <span data-ttu-id="ab3a1-826">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-826">__US__</span></span> | <span data-ttu-id="ab3a1-827">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-827">__In__</span></span> | <span data-ttu-id="ab3a1-828">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-828">__UI__</span></span> | <span data-ttu-id="ab3a1-829">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-829">__Lo__</span></span> | <span data-ttu-id="ab3a1-830">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-830">__UL__</span></span> | <span data-ttu-id="ab3a1-831">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-831">__De__</span></span> | <span data-ttu-id="ab3a1-832">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-832">__Si__</span></span> | <span data-ttu-id="ab3a1-833">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-833">__Do__</span></span> | <span data-ttu-id="ab3a1-834">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-834">__Da__</span></span>  | <span data-ttu-id="ab3a1-835">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-835">__Ch__</span></span>  | <span data-ttu-id="ab3a1-836">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-836">__St__</span></span> | <span data-ttu-id="ab3a1-837">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-837">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="ab3a1-838">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-838">Sh</span></span> | <span data-ttu-id="ab3a1-839">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-839">SB</span></span> | <span data-ttu-id="ab3a1-840">Por</span><span class="sxs-lookup"><span data-stu-id="ab3a1-840">By</span></span> | <span data-ttu-id="ab3a1-841">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-841">Sh</span></span> | <span data-ttu-id="ab3a1-842">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-842">US</span></span> | <span data-ttu-id="ab3a1-843">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-843">In</span></span> | <span data-ttu-id="ab3a1-844">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-844">UI</span></span> | <span data-ttu-id="ab3a1-845">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-845">Lo</span></span> | <span data-ttu-id="ab3a1-846">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-846">UL</span></span> | <span data-ttu-id="ab3a1-847">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-847">De</span></span> | <span data-ttu-id="ab3a1-848">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-848">Si</span></span> | <span data-ttu-id="ab3a1-849">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-849">Do</span></span> | <span data-ttu-id="ab3a1-850">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-850">Err</span></span> | <span data-ttu-id="ab3a1-851">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-851">Err</span></span> | <span data-ttu-id="ab3a1-852">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-852">Do</span></span> | <span data-ttu-id="ab3a1-853">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-853">Ob</span></span> | 


### <a name="unary-minus-operator"></a><span data-ttu-id="ab3a1-854">Operador de subtração unário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-854">Unary Minus Operator</span></span>

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

<span data-ttu-id="ab3a1-855">O operador de subtração unário é definido para os seguintes tipos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-855">The unary minus operator is defined for the following types:</span></span>

<span data-ttu-id="ab3a1-856">`SByte`, `Short`, `Integer` e `Long`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-856">`SByte`, `Short`, `Integer`, and `Long`.</span></span> <span data-ttu-id="ab3a1-857">O resultado é calculado subtraindo-se o operando do zero.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-857">The result is computed by subtracting the operand from zero.</span></span> <span data-ttu-id="ab3a1-858">Se a verificação de estouro de inteiro estiver ligado e o valor do operando é o negativo máximo `SByte`, `Short`, `Integer`, ou `Long`, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-858">If integer overflow checking is on and the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-859">Caso contrário, se o valor do operando é o negativo máximo `SByte`, `Short`, `Integer`, ou `Long`, o resultado é o mesmo valor, e o estouro não será relatado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-859">Otherwise, if the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, the result is that same value, and the overflow is not reported.</span></span>

<span data-ttu-id="ab3a1-860">`Single` e `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-860">`Single` and `Double`.</span></span> <span data-ttu-id="ab3a1-861">O resultado é o valor do operando com o sinal invertido, incluindo os valores de 0 e infinito.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-861">The result is the value of the operand with its sign inverted, including the values 0 and Infinity.</span></span> <span data-ttu-id="ab3a1-862">Se o operando for NaN, o resultado também é NaN.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-862">If the operand is NaN, the result is also NaN.</span></span>

<span data-ttu-id="ab3a1-863">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-863">`Decimal`.</span></span> <span data-ttu-id="ab3a1-864">O resultado é calculado subtraindo-se o operando do zero.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-864">The result is computed by subtracting the operand from zero.</span></span>

<span data-ttu-id="ab3a1-865">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-865">__Operation Type:__</span></span>

| <span data-ttu-id="ab3a1-866">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-866">__Bo__</span></span> | <span data-ttu-id="ab3a1-867">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-867">__SB__</span></span> | <span data-ttu-id="ab3a1-868">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-868">__By__</span></span> | <span data-ttu-id="ab3a1-869">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-869">__Sh__</span></span> | <span data-ttu-id="ab3a1-870">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-870">__US__</span></span> | <span data-ttu-id="ab3a1-871">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-871">__In__</span></span> | <span data-ttu-id="ab3a1-872">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-872">__UI__</span></span> | <span data-ttu-id="ab3a1-873">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-873">__Lo__</span></span> | <span data-ttu-id="ab3a1-874">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-874">__UL__</span></span> | <span data-ttu-id="ab3a1-875">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-875">__De__</span></span> | <span data-ttu-id="ab3a1-876">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-876">__Si__</span></span> | <span data-ttu-id="ab3a1-877">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-877">__Do__</span></span> | <span data-ttu-id="ab3a1-878">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-878">__Da__</span></span>  | <span data-ttu-id="ab3a1-879">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-879">__Ch__</span></span>  | <span data-ttu-id="ab3a1-880">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-880">__St__</span></span> | <span data-ttu-id="ab3a1-881">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-881">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="ab3a1-882">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-882">Sh</span></span> | <span data-ttu-id="ab3a1-883">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-883">SB</span></span> | <span data-ttu-id="ab3a1-884">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-884">Sh</span></span> | <span data-ttu-id="ab3a1-885">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-885">Sh</span></span> | <span data-ttu-id="ab3a1-886">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-886">In</span></span> | <span data-ttu-id="ab3a1-887">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-887">In</span></span> | <span data-ttu-id="ab3a1-888">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-888">Lo</span></span> | <span data-ttu-id="ab3a1-889">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-889">Lo</span></span> | <span data-ttu-id="ab3a1-890">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-890">De</span></span> | <span data-ttu-id="ab3a1-891">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-891">De</span></span> | <span data-ttu-id="ab3a1-892">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-892">Si</span></span> | <span data-ttu-id="ab3a1-893">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-893">Do</span></span> | <span data-ttu-id="ab3a1-894">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-894">Err</span></span> | <span data-ttu-id="ab3a1-895">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-895">Err</span></span> | <span data-ttu-id="ab3a1-896">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-896">Do</span></span> | <span data-ttu-id="ab3a1-897">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-897">Ob</span></span> | 


### <a name="addition-operator"></a><span data-ttu-id="ab3a1-898">Operador de adição</span><span class="sxs-lookup"><span data-stu-id="ab3a1-898">Addition Operator</span></span>

<span data-ttu-id="ab3a1-899">O operador de adição calcula a soma dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-899">The addition operator computes the sum of the two operands.</span></span>

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

<span data-ttu-id="ab3a1-900">O operador de adição é definido para os seguintes tipos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-900">The addition operator is defined for the following types:</span></span>

* <span data-ttu-id="ab3a1-901">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, e `Long`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-901">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="ab3a1-902">Se a verificação de estouro de inteiro estiver ligado e a soma está fora do intervalo do tipo de resultado, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-902">If integer overflow checking is on and the sum is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-903">Caso contrário, estouros não são relatados e quaisquer bits de ordem superior significativos do resultado são descartados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-903">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="ab3a1-904">`Single` e `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-904">`Single` and `Double`.</span></span> <span data-ttu-id="ab3a1-905">A soma é calculada de acordo com as regras de aritmética do IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-905">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="ab3a1-906">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-906">`Decimal`.</span></span> <span data-ttu-id="ab3a1-907">Se o valor resultante é muito grande para ser representado no formato decimal, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-907">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-908">Se o valor do resultado for muito pequeno para ser representado no formato decimal, o resultado é 0.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-908">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="ab3a1-909">`String`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-909">`String`.</span></span> <span data-ttu-id="ab3a1-910">Os dois `String` operandos são concatenados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-910">The two `String` operands are concatenated together.</span></span>

* <span data-ttu-id="ab3a1-911">`Date`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-911">`Date`.</span></span> <span data-ttu-id="ab3a1-912">O `System.DateTime` tipo define os operadores de adição sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-912">The `System.DateTime` type defines overloaded addition operators.</span></span> <span data-ttu-id="ab3a1-913">Porque `System.DateTime` é equivalente a intrínseca `Date` tipo, esses operadores também está disponível a `Date` tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-913">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="ab3a1-914">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-914">__Operation Type:__</span></span>

|        | <span data-ttu-id="ab3a1-915">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-915">__Bo__</span></span> | <span data-ttu-id="ab3a1-916">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-916">__SB__</span></span> | <span data-ttu-id="ab3a1-917">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-917">__By__</span></span> | <span data-ttu-id="ab3a1-918">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-918">__Sh__</span></span> | <span data-ttu-id="ab3a1-919">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-919">__US__</span></span> | <span data-ttu-id="ab3a1-920">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-920">__In__</span></span> | <span data-ttu-id="ab3a1-921">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-921">__UI__</span></span> | <span data-ttu-id="ab3a1-922">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-922">__Lo__</span></span> | <span data-ttu-id="ab3a1-923">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-923">__UL__</span></span> | <span data-ttu-id="ab3a1-924">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-924">__De__</span></span> | <span data-ttu-id="ab3a1-925">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-925">__Si__</span></span> | <span data-ttu-id="ab3a1-926">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-926">__Do__</span></span> | <span data-ttu-id="ab3a1-927">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-927">__Da__</span></span>  | <span data-ttu-id="ab3a1-928">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-928">__Ch__</span></span>  | <span data-ttu-id="ab3a1-929">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-929">__St__</span></span> | <span data-ttu-id="ab3a1-930">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-930">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="ab3a1-931">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-931">__Bo__</span></span> | <span data-ttu-id="ab3a1-932">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-932">Sh</span></span> | <span data-ttu-id="ab3a1-933">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-933">SB</span></span> | <span data-ttu-id="ab3a1-934">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-934">Sh</span></span> | <span data-ttu-id="ab3a1-935">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-935">Sh</span></span> | <span data-ttu-id="ab3a1-936">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-936">In</span></span> | <span data-ttu-id="ab3a1-937">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-937">In</span></span> | <span data-ttu-id="ab3a1-938">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-938">Lo</span></span> | <span data-ttu-id="ab3a1-939">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-939">Lo</span></span> | <span data-ttu-id="ab3a1-940">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-940">De</span></span> | <span data-ttu-id="ab3a1-941">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-941">De</span></span> | <span data-ttu-id="ab3a1-942">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-942">Si</span></span> | <span data-ttu-id="ab3a1-943">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-943">Do</span></span> | <span data-ttu-id="ab3a1-944">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-944">Err</span></span> | <span data-ttu-id="ab3a1-945">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-945">Err</span></span> | <span data-ttu-id="ab3a1-946">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-946">Do</span></span> | <span data-ttu-id="ab3a1-947">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-947">Ob</span></span> | 
| <span data-ttu-id="ab3a1-948">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-948">__SB__</span></span> |    | <span data-ttu-id="ab3a1-949">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-949">SB</span></span> | <span data-ttu-id="ab3a1-950">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-950">Sh</span></span> | <span data-ttu-id="ab3a1-951">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-951">Sh</span></span> | <span data-ttu-id="ab3a1-952">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-952">In</span></span> | <span data-ttu-id="ab3a1-953">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-953">In</span></span> | <span data-ttu-id="ab3a1-954">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-954">Lo</span></span> | <span data-ttu-id="ab3a1-955">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-955">Lo</span></span> | <span data-ttu-id="ab3a1-956">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-956">De</span></span> | <span data-ttu-id="ab3a1-957">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-957">De</span></span> | <span data-ttu-id="ab3a1-958">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-958">Si</span></span> | <span data-ttu-id="ab3a1-959">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-959">Do</span></span> | <span data-ttu-id="ab3a1-960">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-960">Err</span></span> | <span data-ttu-id="ab3a1-961">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-961">Err</span></span> | <span data-ttu-id="ab3a1-962">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-962">Do</span></span> | <span data-ttu-id="ab3a1-963">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-963">Ob</span></span> | 
| <span data-ttu-id="ab3a1-964">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-964">__By__</span></span> |    |    | <span data-ttu-id="ab3a1-965">Por</span><span class="sxs-lookup"><span data-stu-id="ab3a1-965">By</span></span> | <span data-ttu-id="ab3a1-966">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-966">Sh</span></span> | <span data-ttu-id="ab3a1-967">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-967">US</span></span> | <span data-ttu-id="ab3a1-968">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-968">In</span></span> | <span data-ttu-id="ab3a1-969">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-969">UI</span></span> | <span data-ttu-id="ab3a1-970">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-970">Lo</span></span> | <span data-ttu-id="ab3a1-971">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-971">UL</span></span> | <span data-ttu-id="ab3a1-972">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-972">De</span></span> | <span data-ttu-id="ab3a1-973">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-973">Si</span></span> | <span data-ttu-id="ab3a1-974">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-974">Do</span></span> | <span data-ttu-id="ab3a1-975">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-975">Err</span></span> | <span data-ttu-id="ab3a1-976">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-976">Err</span></span> | <span data-ttu-id="ab3a1-977">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-977">Do</span></span> | <span data-ttu-id="ab3a1-978">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-978">Ob</span></span> | 
| <span data-ttu-id="ab3a1-979">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-979">__Sh__</span></span> |    |    |    | <span data-ttu-id="ab3a1-980">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-980">Sh</span></span> | <span data-ttu-id="ab3a1-981">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-981">In</span></span> | <span data-ttu-id="ab3a1-982">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-982">In</span></span> | <span data-ttu-id="ab3a1-983">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-983">Lo</span></span> | <span data-ttu-id="ab3a1-984">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-984">Lo</span></span> | <span data-ttu-id="ab3a1-985">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-985">De</span></span> | <span data-ttu-id="ab3a1-986">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-986">De</span></span> | <span data-ttu-id="ab3a1-987">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-987">Si</span></span> | <span data-ttu-id="ab3a1-988">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-988">Do</span></span> | <span data-ttu-id="ab3a1-989">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-989">Err</span></span> | <span data-ttu-id="ab3a1-990">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-990">Err</span></span> | <span data-ttu-id="ab3a1-991">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-991">Do</span></span> | <span data-ttu-id="ab3a1-992">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-992">Ob</span></span> | 
| <span data-ttu-id="ab3a1-993">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-993">__US__</span></span> |    |    |    |    | <span data-ttu-id="ab3a1-994">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-994">US</span></span> | <span data-ttu-id="ab3a1-995">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-995">In</span></span> | <span data-ttu-id="ab3a1-996">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-996">UI</span></span> | <span data-ttu-id="ab3a1-997">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-997">Lo</span></span> | <span data-ttu-id="ab3a1-998">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-998">UL</span></span> | <span data-ttu-id="ab3a1-999">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-999">De</span></span> | <span data-ttu-id="ab3a1-1000">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1000">Si</span></span> | <span data-ttu-id="ab3a1-1001">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1001">Do</span></span> | <span data-ttu-id="ab3a1-1002">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1002">Err</span></span> | <span data-ttu-id="ab3a1-1003">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1003">Err</span></span> | <span data-ttu-id="ab3a1-1004">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1004">Do</span></span> | <span data-ttu-id="ab3a1-1005">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1005">Ob</span></span> | 
| <span data-ttu-id="ab3a1-1006">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1006">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ab3a1-1007">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1007">In</span></span> | <span data-ttu-id="ab3a1-1008">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1008">Lo</span></span> | <span data-ttu-id="ab3a1-1009">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1009">Lo</span></span> | <span data-ttu-id="ab3a1-1010">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1010">De</span></span> | <span data-ttu-id="ab3a1-1011">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1011">De</span></span> | <span data-ttu-id="ab3a1-1012">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1012">Si</span></span> | <span data-ttu-id="ab3a1-1013">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1013">Do</span></span> | <span data-ttu-id="ab3a1-1014">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1014">Err</span></span> | <span data-ttu-id="ab3a1-1015">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1015">Err</span></span> | <span data-ttu-id="ab3a1-1016">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1016">Do</span></span> | <span data-ttu-id="ab3a1-1017">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1017">Ob</span></span> | 
| <span data-ttu-id="ab3a1-1018">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1018">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1019">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1019">UI</span></span> | <span data-ttu-id="ab3a1-1020">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1020">Lo</span></span> | <span data-ttu-id="ab3a1-1021">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1021">UL</span></span> | <span data-ttu-id="ab3a1-1022">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1022">De</span></span> | <span data-ttu-id="ab3a1-1023">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1023">Si</span></span> | <span data-ttu-id="ab3a1-1024">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1024">Do</span></span> | <span data-ttu-id="ab3a1-1025">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1025">Err</span></span> | <span data-ttu-id="ab3a1-1026">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1026">Err</span></span> | <span data-ttu-id="ab3a1-1027">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1027">Do</span></span> | <span data-ttu-id="ab3a1-1028">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1028">Ob</span></span> | 
| <span data-ttu-id="ab3a1-1029">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1029">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1030">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1030">Lo</span></span> | <span data-ttu-id="ab3a1-1031">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1031">De</span></span> | <span data-ttu-id="ab3a1-1032">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1032">De</span></span> | <span data-ttu-id="ab3a1-1033">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1033">Si</span></span> | <span data-ttu-id="ab3a1-1034">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1034">Do</span></span> | <span data-ttu-id="ab3a1-1035">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1035">Err</span></span> | <span data-ttu-id="ab3a1-1036">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1036">Err</span></span> | <span data-ttu-id="ab3a1-1037">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1037">Do</span></span> | <span data-ttu-id="ab3a1-1038">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1038">Ob</span></span> | 
| <span data-ttu-id="ab3a1-1039">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1039">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1040">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1040">UL</span></span> | <span data-ttu-id="ab3a1-1041">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1041">De</span></span> | <span data-ttu-id="ab3a1-1042">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1042">Si</span></span> | <span data-ttu-id="ab3a1-1043">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1043">Do</span></span> | <span data-ttu-id="ab3a1-1044">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1044">Err</span></span> | <span data-ttu-id="ab3a1-1045">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1045">Err</span></span> | <span data-ttu-id="ab3a1-1046">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1046">Do</span></span> | <span data-ttu-id="ab3a1-1047">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1047">Ob</span></span> | 
| <span data-ttu-id="ab3a1-1048">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1048">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1049">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1049">De</span></span> | <span data-ttu-id="ab3a1-1050">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1050">Si</span></span> | <span data-ttu-id="ab3a1-1051">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1051">Do</span></span> | <span data-ttu-id="ab3a1-1052">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1052">Err</span></span> | <span data-ttu-id="ab3a1-1053">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1053">Err</span></span> | <span data-ttu-id="ab3a1-1054">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1054">Do</span></span> | <span data-ttu-id="ab3a1-1055">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1055">Ob</span></span> | 
| <span data-ttu-id="ab3a1-1056">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1056">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1057">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1057">Si</span></span> | <span data-ttu-id="ab3a1-1058">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1058">Do</span></span> | <span data-ttu-id="ab3a1-1059">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1059">Err</span></span> | <span data-ttu-id="ab3a1-1060">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1060">Err</span></span> | <span data-ttu-id="ab3a1-1061">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1061">Do</span></span> | <span data-ttu-id="ab3a1-1062">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1062">Ob</span></span> | 
| <span data-ttu-id="ab3a1-1063">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1063">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1064">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1064">Do</span></span> | <span data-ttu-id="ab3a1-1065">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1065">Err</span></span> | <span data-ttu-id="ab3a1-1066">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1066">Err</span></span> | <span data-ttu-id="ab3a1-1067">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1067">Do</span></span> | <span data-ttu-id="ab3a1-1068">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1068">Ob</span></span> | 
| <span data-ttu-id="ab3a1-1069">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1069">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1070">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1070">St</span></span>  | <span data-ttu-id="ab3a1-1071">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1071">Err</span></span> | <span data-ttu-id="ab3a1-1072">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1072">St</span></span> | <span data-ttu-id="ab3a1-1073">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1073">Ob</span></span> | 
| <span data-ttu-id="ab3a1-1074">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1074">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ab3a1-1075">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1075">St</span></span>  | <span data-ttu-id="ab3a1-1076">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1076">St</span></span> | <span data-ttu-id="ab3a1-1077">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1077">Ob</span></span> | 
| <span data-ttu-id="ab3a1-1078">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1078">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ab3a1-1079">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1079">St</span></span> | <span data-ttu-id="ab3a1-1080">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1080">Ob</span></span> | 
| <span data-ttu-id="ab3a1-1081">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1081">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="ab3a1-1082">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1082">Ob</span></span> | 


### <a name="subtraction-operator"></a><span data-ttu-id="ab3a1-1083">Operador de subtração</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1083">Subtraction Operator</span></span>

<span data-ttu-id="ab3a1-1084">O operador de subtração subtrai o segundo operando do primeiro operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1084">The subtraction operator subtracts the second operand from the first operand.</span></span>

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

<span data-ttu-id="ab3a1-1085">O operador de subtração é definido para os seguintes tipos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1085">The subtraction operator is defined for the following types:</span></span>

* <span data-ttu-id="ab3a1-1086">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, e `Long`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1086">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="ab3a1-1087">Se a verificação de estouro de inteiro estiver ligado e a diferença está fora do intervalo do tipo de resultado, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1087">If integer overflow checking is on and the difference is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-1088">Caso contrário, estouros não são relatados e quaisquer bits de ordem superior significativos do resultado são descartados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1088">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="ab3a1-1089">`Single` e `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1089">`Single` and `Double`.</span></span> <span data-ttu-id="ab3a1-1090">A diferença é calculada de acordo com as regras de aritmética do IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1090">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="ab3a1-1091">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1091">`Decimal`.</span></span> <span data-ttu-id="ab3a1-1092">Se o valor resultante é muito grande para ser representado no formato decimal, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1092">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-1093">Se o valor do resultado for muito pequeno para ser representado no formato decimal, o resultado é 0.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1093">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="ab3a1-1094">`Date`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1094">`Date`.</span></span> <span data-ttu-id="ab3a1-1095">O `System.DateTime` tipo define os operadores de subtração sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1095">The `System.DateTime` type defines overloaded subtraction operators.</span></span> <span data-ttu-id="ab3a1-1096">Porque `System.DateTime` é equivalente a intrínseca `Date` tipo, esses operadores também está disponível a `Date` tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1096">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="ab3a1-1097">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1097">__Operation Type:__</span></span>

|        | <span data-ttu-id="ab3a1-1098">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1098">__Bo__</span></span> | <span data-ttu-id="ab3a1-1099">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1099">__SB__</span></span> | <span data-ttu-id="ab3a1-1100">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1100">__By__</span></span> | <span data-ttu-id="ab3a1-1101">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1101">__Sh__</span></span> | <span data-ttu-id="ab3a1-1102">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1102">__US__</span></span> | <span data-ttu-id="ab3a1-1103">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1103">__In__</span></span> | <span data-ttu-id="ab3a1-1104">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1104">__UI__</span></span> | <span data-ttu-id="ab3a1-1105">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1105">__Lo__</span></span> | <span data-ttu-id="ab3a1-1106">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1106">__UL__</span></span> | <span data-ttu-id="ab3a1-1107">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1107">__De__</span></span> | <span data-ttu-id="ab3a1-1108">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1108">__Si__</span></span> | <span data-ttu-id="ab3a1-1109">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1109">__Do__</span></span> | <span data-ttu-id="ab3a1-1110">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1110">__Da__</span></span>  | <span data-ttu-id="ab3a1-1111">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1111">__Ch__</span></span>  | <span data-ttu-id="ab3a1-1112">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1112">__St__</span></span> | <span data-ttu-id="ab3a1-1113">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1113">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ab3a1-1114">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1114">__Bo__</span></span> | <span data-ttu-id="ab3a1-1115">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1115">Sh</span></span> | <span data-ttu-id="ab3a1-1116">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1116">SB</span></span> | <span data-ttu-id="ab3a1-1117">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1117">Sh</span></span> | <span data-ttu-id="ab3a1-1118">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1118">Sh</span></span> | <span data-ttu-id="ab3a1-1119">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1119">In</span></span> | <span data-ttu-id="ab3a1-1120">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1120">In</span></span> | <span data-ttu-id="ab3a1-1121">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1121">Lo</span></span> | <span data-ttu-id="ab3a1-1122">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1122">Lo</span></span> | <span data-ttu-id="ab3a1-1123">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1123">De</span></span> | <span data-ttu-id="ab3a1-1124">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1124">De</span></span> | <span data-ttu-id="ab3a1-1125">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1125">Si</span></span> | <span data-ttu-id="ab3a1-1126">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1126">Do</span></span> | <span data-ttu-id="ab3a1-1127">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1127">Err</span></span> | <span data-ttu-id="ab3a1-1128">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1128">Err</span></span> | <span data-ttu-id="ab3a1-1129">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1129">Do</span></span>  | <span data-ttu-id="ab3a1-1130">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1130">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1131">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1131">__SB__</span></span> |    | <span data-ttu-id="ab3a1-1132">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1132">SB</span></span> | <span data-ttu-id="ab3a1-1133">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1133">Sh</span></span> | <span data-ttu-id="ab3a1-1134">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1134">Sh</span></span> | <span data-ttu-id="ab3a1-1135">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1135">In</span></span> | <span data-ttu-id="ab3a1-1136">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1136">In</span></span> | <span data-ttu-id="ab3a1-1137">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1137">Lo</span></span> | <span data-ttu-id="ab3a1-1138">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1138">Lo</span></span> | <span data-ttu-id="ab3a1-1139">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1139">De</span></span> | <span data-ttu-id="ab3a1-1140">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1140">De</span></span> | <span data-ttu-id="ab3a1-1141">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1141">Si</span></span> | <span data-ttu-id="ab3a1-1142">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1142">Do</span></span> | <span data-ttu-id="ab3a1-1143">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1143">Err</span></span> | <span data-ttu-id="ab3a1-1144">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1144">Err</span></span> | <span data-ttu-id="ab3a1-1145">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1145">Do</span></span>  | <span data-ttu-id="ab3a1-1146">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1146">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1147">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1147">__By__</span></span> |    |    | <span data-ttu-id="ab3a1-1148">Por</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1148">By</span></span> | <span data-ttu-id="ab3a1-1149">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1149">Sh</span></span> | <span data-ttu-id="ab3a1-1150">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1150">US</span></span> | <span data-ttu-id="ab3a1-1151">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1151">In</span></span> | <span data-ttu-id="ab3a1-1152">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1152">UI</span></span> | <span data-ttu-id="ab3a1-1153">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1153">Lo</span></span> | <span data-ttu-id="ab3a1-1154">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1154">UL</span></span> | <span data-ttu-id="ab3a1-1155">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1155">De</span></span> | <span data-ttu-id="ab3a1-1156">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1156">Si</span></span> | <span data-ttu-id="ab3a1-1157">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1157">Do</span></span> | <span data-ttu-id="ab3a1-1158">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1158">Err</span></span> | <span data-ttu-id="ab3a1-1159">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1159">Err</span></span> | <span data-ttu-id="ab3a1-1160">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1160">Do</span></span>  | <span data-ttu-id="ab3a1-1161">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1161">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1162">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1162">__Sh__</span></span> |    |    |    | <span data-ttu-id="ab3a1-1163">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1163">Sh</span></span> | <span data-ttu-id="ab3a1-1164">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1164">In</span></span> | <span data-ttu-id="ab3a1-1165">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1165">In</span></span> | <span data-ttu-id="ab3a1-1166">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1166">Lo</span></span> | <span data-ttu-id="ab3a1-1167">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1167">Lo</span></span> | <span data-ttu-id="ab3a1-1168">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1168">De</span></span> | <span data-ttu-id="ab3a1-1169">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1169">De</span></span> | <span data-ttu-id="ab3a1-1170">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1170">Si</span></span> | <span data-ttu-id="ab3a1-1171">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1171">Do</span></span> | <span data-ttu-id="ab3a1-1172">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1172">Err</span></span> | <span data-ttu-id="ab3a1-1173">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1173">Err</span></span> | <span data-ttu-id="ab3a1-1174">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1174">Do</span></span>  | <span data-ttu-id="ab3a1-1175">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1175">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1176">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1176">__US__</span></span> |    |    |    |    | <span data-ttu-id="ab3a1-1177">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1177">US</span></span> | <span data-ttu-id="ab3a1-1178">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1178">In</span></span> | <span data-ttu-id="ab3a1-1179">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1179">UI</span></span> | <span data-ttu-id="ab3a1-1180">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1180">Lo</span></span> | <span data-ttu-id="ab3a1-1181">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1181">UL</span></span> | <span data-ttu-id="ab3a1-1182">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1182">De</span></span> | <span data-ttu-id="ab3a1-1183">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1183">Si</span></span> | <span data-ttu-id="ab3a1-1184">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1184">Do</span></span> | <span data-ttu-id="ab3a1-1185">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1185">Err</span></span> | <span data-ttu-id="ab3a1-1186">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1186">Err</span></span> | <span data-ttu-id="ab3a1-1187">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1187">Do</span></span>  | <span data-ttu-id="ab3a1-1188">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1188">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1189">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1189">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ab3a1-1190">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1190">In</span></span> | <span data-ttu-id="ab3a1-1191">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1191">Lo</span></span> | <span data-ttu-id="ab3a1-1192">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1192">Lo</span></span> | <span data-ttu-id="ab3a1-1193">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1193">De</span></span> | <span data-ttu-id="ab3a1-1194">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1194">De</span></span> | <span data-ttu-id="ab3a1-1195">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1195">Si</span></span> | <span data-ttu-id="ab3a1-1196">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1196">Do</span></span> | <span data-ttu-id="ab3a1-1197">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1197">Err</span></span> | <span data-ttu-id="ab3a1-1198">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1198">Err</span></span> | <span data-ttu-id="ab3a1-1199">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1199">Do</span></span>  | <span data-ttu-id="ab3a1-1200">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1200">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1201">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1201">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1202">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1202">UI</span></span> | <span data-ttu-id="ab3a1-1203">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1203">Lo</span></span> | <span data-ttu-id="ab3a1-1204">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1204">UL</span></span> | <span data-ttu-id="ab3a1-1205">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1205">De</span></span> | <span data-ttu-id="ab3a1-1206">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1206">Si</span></span> | <span data-ttu-id="ab3a1-1207">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1207">Do</span></span> | <span data-ttu-id="ab3a1-1208">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1208">Err</span></span> | <span data-ttu-id="ab3a1-1209">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1209">Err</span></span> | <span data-ttu-id="ab3a1-1210">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1210">Do</span></span>  | <span data-ttu-id="ab3a1-1211">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1211">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1212">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1212">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1213">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1213">Lo</span></span> | <span data-ttu-id="ab3a1-1214">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1214">De</span></span> | <span data-ttu-id="ab3a1-1215">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1215">De</span></span> | <span data-ttu-id="ab3a1-1216">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1216">Si</span></span> | <span data-ttu-id="ab3a1-1217">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1217">Do</span></span> | <span data-ttu-id="ab3a1-1218">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1218">Err</span></span> | <span data-ttu-id="ab3a1-1219">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1219">Err</span></span> | <span data-ttu-id="ab3a1-1220">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1220">Do</span></span>  | <span data-ttu-id="ab3a1-1221">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1221">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1222">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1222">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1223">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1223">UL</span></span> | <span data-ttu-id="ab3a1-1224">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1224">De</span></span> | <span data-ttu-id="ab3a1-1225">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1225">Si</span></span> | <span data-ttu-id="ab3a1-1226">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1226">Do</span></span> | <span data-ttu-id="ab3a1-1227">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1227">Err</span></span> | <span data-ttu-id="ab3a1-1228">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1228">Err</span></span> | <span data-ttu-id="ab3a1-1229">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1229">Do</span></span>  | <span data-ttu-id="ab3a1-1230">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1230">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1231">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1231">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1232">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1232">De</span></span> | <span data-ttu-id="ab3a1-1233">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1233">Si</span></span> | <span data-ttu-id="ab3a1-1234">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1234">Do</span></span> | <span data-ttu-id="ab3a1-1235">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1235">Err</span></span> | <span data-ttu-id="ab3a1-1236">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1236">Err</span></span> | <span data-ttu-id="ab3a1-1237">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1237">Do</span></span>  | <span data-ttu-id="ab3a1-1238">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1238">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1239">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1239">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1240">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1240">Si</span></span> | <span data-ttu-id="ab3a1-1241">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1241">Do</span></span> | <span data-ttu-id="ab3a1-1242">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1242">Err</span></span> | <span data-ttu-id="ab3a1-1243">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1243">Err</span></span> | <span data-ttu-id="ab3a1-1244">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1244">Do</span></span>  | <span data-ttu-id="ab3a1-1245">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1245">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1246">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1246">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1247">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1247">Do</span></span> | <span data-ttu-id="ab3a1-1248">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1248">Err</span></span> | <span data-ttu-id="ab3a1-1249">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1249">Err</span></span> | <span data-ttu-id="ab3a1-1250">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1250">Do</span></span>  | <span data-ttu-id="ab3a1-1251">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1251">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1252">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1252">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1253">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1253">Err</span></span> | <span data-ttu-id="ab3a1-1254">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1254">Err</span></span> | <span data-ttu-id="ab3a1-1255">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1255">Err</span></span> | <span data-ttu-id="ab3a1-1256">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1256">Err</span></span> | 
| <span data-ttu-id="ab3a1-1257">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1257">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ab3a1-1258">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1258">Err</span></span> | <span data-ttu-id="ab3a1-1259">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1259">Err</span></span> | <span data-ttu-id="ab3a1-1260">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1260">Err</span></span> | 
| <span data-ttu-id="ab3a1-1261">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1261">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ab3a1-1262">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1262">Do</span></span>  | <span data-ttu-id="ab3a1-1263">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1263">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1264">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1264">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ab3a1-1265">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1265">Ob</span></span>  | 


### <a name="multiplication-operator"></a><span data-ttu-id="ab3a1-1266">Operador de multiplicação</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1266">Multiplication Operator</span></span>

<span data-ttu-id="ab3a1-1267">O operador de multiplicação calcula o produto dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1267">The multiplication operator computes the product of two operands.</span></span>

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

<span data-ttu-id="ab3a1-1268">O operador de multiplicação é definido para os seguintes tipos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1268">The multiplication operator is defined for the following types:</span></span>

* <span data-ttu-id="ab3a1-1269">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, e `Long`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1269">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="ab3a1-1270">Se a verificação de estouro de inteiro estiver ligado e o produto está fora do intervalo do tipo de resultado, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1270">If integer overflow checking is on and the product is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-1271">Caso contrário, estouros não são relatados e quaisquer bits de ordem superior significativos do resultado são descartados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1271">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="ab3a1-1272">`Single` e `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1272">`Single` and `Double`.</span></span> <span data-ttu-id="ab3a1-1273">O produto é calculado de acordo com as regras de aritmética do IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1273">The product is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="ab3a1-1274">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1274">`Decimal`.</span></span> <span data-ttu-id="ab3a1-1275">Se o valor resultante é muito grande para ser representado no formato decimal, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1275">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-1276">Se o valor do resultado for muito pequeno para ser representado no formato decimal, o resultado é 0.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1276">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

<span data-ttu-id="ab3a1-1277">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1277">__Operation Type:__</span></span>

|        | <span data-ttu-id="ab3a1-1278">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1278">__Bo__</span></span> | <span data-ttu-id="ab3a1-1279">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1279">__SB__</span></span> | <span data-ttu-id="ab3a1-1280">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1280">__By__</span></span> | <span data-ttu-id="ab3a1-1281">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1281">__Sh__</span></span> | <span data-ttu-id="ab3a1-1282">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1282">__US__</span></span> | <span data-ttu-id="ab3a1-1283">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1283">__In__</span></span> | <span data-ttu-id="ab3a1-1284">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1284">__UI__</span></span> | <span data-ttu-id="ab3a1-1285">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1285">__Lo__</span></span> | <span data-ttu-id="ab3a1-1286">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1286">__UL__</span></span> | <span data-ttu-id="ab3a1-1287">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1287">__De__</span></span> | <span data-ttu-id="ab3a1-1288">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1288">__Si__</span></span> | <span data-ttu-id="ab3a1-1289">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1289">__Do__</span></span> | <span data-ttu-id="ab3a1-1290">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1290">__Da__</span></span>  | <span data-ttu-id="ab3a1-1291">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1291">__Ch__</span></span>  | <span data-ttu-id="ab3a1-1292">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1292">__St__</span></span> | <span data-ttu-id="ab3a1-1293">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1293">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ab3a1-1294">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1294">__Bo__</span></span> | <span data-ttu-id="ab3a1-1295">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1295">Sh</span></span> | <span data-ttu-id="ab3a1-1296">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1296">SB</span></span> | <span data-ttu-id="ab3a1-1297">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1297">Sh</span></span> | <span data-ttu-id="ab3a1-1298">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1298">Sh</span></span> | <span data-ttu-id="ab3a1-1299">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1299">In</span></span> | <span data-ttu-id="ab3a1-1300">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1300">In</span></span> | <span data-ttu-id="ab3a1-1301">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1301">Lo</span></span> | <span data-ttu-id="ab3a1-1302">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1302">Lo</span></span> | <span data-ttu-id="ab3a1-1303">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1303">De</span></span> | <span data-ttu-id="ab3a1-1304">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1304">De</span></span> | <span data-ttu-id="ab3a1-1305">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1305">Si</span></span> | <span data-ttu-id="ab3a1-1306">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1306">Do</span></span> | <span data-ttu-id="ab3a1-1307">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1307">Err</span></span> | <span data-ttu-id="ab3a1-1308">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1308">Err</span></span> | <span data-ttu-id="ab3a1-1309">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1309">Do</span></span>  | <span data-ttu-id="ab3a1-1310">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1310">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1311">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1311">__SB__</span></span> |    | <span data-ttu-id="ab3a1-1312">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1312">SB</span></span> | <span data-ttu-id="ab3a1-1313">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1313">Sh</span></span> | <span data-ttu-id="ab3a1-1314">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1314">Sh</span></span> | <span data-ttu-id="ab3a1-1315">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1315">In</span></span> | <span data-ttu-id="ab3a1-1316">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1316">In</span></span> | <span data-ttu-id="ab3a1-1317">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1317">Lo</span></span> | <span data-ttu-id="ab3a1-1318">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1318">Lo</span></span> | <span data-ttu-id="ab3a1-1319">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1319">De</span></span> | <span data-ttu-id="ab3a1-1320">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1320">De</span></span> | <span data-ttu-id="ab3a1-1321">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1321">Si</span></span> | <span data-ttu-id="ab3a1-1322">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1322">Do</span></span> | <span data-ttu-id="ab3a1-1323">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1323">Err</span></span> | <span data-ttu-id="ab3a1-1324">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1324">Err</span></span> | <span data-ttu-id="ab3a1-1325">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1325">Do</span></span>  | <span data-ttu-id="ab3a1-1326">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1326">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1327">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1327">__By__</span></span> |    |    | <span data-ttu-id="ab3a1-1328">Por</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1328">By</span></span> | <span data-ttu-id="ab3a1-1329">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1329">Sh</span></span> | <span data-ttu-id="ab3a1-1330">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1330">US</span></span> | <span data-ttu-id="ab3a1-1331">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1331">In</span></span> | <span data-ttu-id="ab3a1-1332">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1332">UI</span></span> | <span data-ttu-id="ab3a1-1333">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1333">Lo</span></span> | <span data-ttu-id="ab3a1-1334">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1334">UL</span></span> | <span data-ttu-id="ab3a1-1335">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1335">De</span></span> | <span data-ttu-id="ab3a1-1336">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1336">Si</span></span> | <span data-ttu-id="ab3a1-1337">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1337">Do</span></span> | <span data-ttu-id="ab3a1-1338">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1338">Err</span></span> | <span data-ttu-id="ab3a1-1339">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1339">Err</span></span> | <span data-ttu-id="ab3a1-1340">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1340">Do</span></span>  | <span data-ttu-id="ab3a1-1341">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1341">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1342">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1342">__Sh__</span></span> |    |    |    | <span data-ttu-id="ab3a1-1343">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1343">Sh</span></span> | <span data-ttu-id="ab3a1-1344">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1344">In</span></span> | <span data-ttu-id="ab3a1-1345">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1345">In</span></span> | <span data-ttu-id="ab3a1-1346">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1346">Lo</span></span> | <span data-ttu-id="ab3a1-1347">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1347">Lo</span></span> | <span data-ttu-id="ab3a1-1348">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1348">De</span></span> | <span data-ttu-id="ab3a1-1349">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1349">De</span></span> | <span data-ttu-id="ab3a1-1350">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1350">Si</span></span> | <span data-ttu-id="ab3a1-1351">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1351">Do</span></span> | <span data-ttu-id="ab3a1-1352">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1352">Err</span></span> | <span data-ttu-id="ab3a1-1353">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1353">Err</span></span> | <span data-ttu-id="ab3a1-1354">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1354">Do</span></span>  | <span data-ttu-id="ab3a1-1355">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1355">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1356">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1356">__US__</span></span> |    |    |    |    | <span data-ttu-id="ab3a1-1357">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1357">US</span></span> | <span data-ttu-id="ab3a1-1358">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1358">In</span></span> | <span data-ttu-id="ab3a1-1359">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1359">UI</span></span> | <span data-ttu-id="ab3a1-1360">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1360">Lo</span></span> | <span data-ttu-id="ab3a1-1361">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1361">UL</span></span> | <span data-ttu-id="ab3a1-1362">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1362">De</span></span> | <span data-ttu-id="ab3a1-1363">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1363">Si</span></span> | <span data-ttu-id="ab3a1-1364">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1364">Do</span></span> | <span data-ttu-id="ab3a1-1365">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1365">Err</span></span> | <span data-ttu-id="ab3a1-1366">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1366">Err</span></span> | <span data-ttu-id="ab3a1-1367">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1367">Do</span></span>  | <span data-ttu-id="ab3a1-1368">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1368">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1369">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1369">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ab3a1-1370">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1370">In</span></span> | <span data-ttu-id="ab3a1-1371">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1371">Lo</span></span> | <span data-ttu-id="ab3a1-1372">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1372">Lo</span></span> | <span data-ttu-id="ab3a1-1373">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1373">De</span></span> | <span data-ttu-id="ab3a1-1374">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1374">De</span></span> | <span data-ttu-id="ab3a1-1375">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1375">Si</span></span> | <span data-ttu-id="ab3a1-1376">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1376">Do</span></span> | <span data-ttu-id="ab3a1-1377">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1377">Err</span></span> | <span data-ttu-id="ab3a1-1378">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1378">Err</span></span> | <span data-ttu-id="ab3a1-1379">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1379">Do</span></span>  | <span data-ttu-id="ab3a1-1380">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1380">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1381">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1381">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1382">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1382">UI</span></span> | <span data-ttu-id="ab3a1-1383">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1383">Lo</span></span> | <span data-ttu-id="ab3a1-1384">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1384">UL</span></span> | <span data-ttu-id="ab3a1-1385">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1385">De</span></span> | <span data-ttu-id="ab3a1-1386">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1386">Si</span></span> | <span data-ttu-id="ab3a1-1387">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1387">Do</span></span> | <span data-ttu-id="ab3a1-1388">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1388">Err</span></span> | <span data-ttu-id="ab3a1-1389">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1389">Err</span></span> | <span data-ttu-id="ab3a1-1390">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1390">Do</span></span>  | <span data-ttu-id="ab3a1-1391">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1391">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1392">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1392">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1393">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1393">Lo</span></span> | <span data-ttu-id="ab3a1-1394">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1394">De</span></span> | <span data-ttu-id="ab3a1-1395">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1395">De</span></span> | <span data-ttu-id="ab3a1-1396">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1396">Si</span></span> | <span data-ttu-id="ab3a1-1397">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1397">Do</span></span> | <span data-ttu-id="ab3a1-1398">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1398">Err</span></span> | <span data-ttu-id="ab3a1-1399">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1399">Err</span></span> | <span data-ttu-id="ab3a1-1400">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1400">Do</span></span>  | <span data-ttu-id="ab3a1-1401">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1401">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1402">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1402">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1403">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1403">UL</span></span> | <span data-ttu-id="ab3a1-1404">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1404">De</span></span> | <span data-ttu-id="ab3a1-1405">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1405">Si</span></span> | <span data-ttu-id="ab3a1-1406">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1406">Do</span></span> | <span data-ttu-id="ab3a1-1407">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1407">Err</span></span> | <span data-ttu-id="ab3a1-1408">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1408">Err</span></span> | <span data-ttu-id="ab3a1-1409">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1409">Do</span></span>  | <span data-ttu-id="ab3a1-1410">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1410">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1411">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1411">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1412">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1412">De</span></span> | <span data-ttu-id="ab3a1-1413">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1413">Si</span></span> | <span data-ttu-id="ab3a1-1414">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1414">Do</span></span> | <span data-ttu-id="ab3a1-1415">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1415">Err</span></span> | <span data-ttu-id="ab3a1-1416">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1416">Err</span></span> | <span data-ttu-id="ab3a1-1417">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1417">Do</span></span>  | <span data-ttu-id="ab3a1-1418">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1418">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1419">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1419">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1420">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1420">Si</span></span> | <span data-ttu-id="ab3a1-1421">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1421">Do</span></span> | <span data-ttu-id="ab3a1-1422">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1422">Err</span></span> | <span data-ttu-id="ab3a1-1423">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1423">Err</span></span> | <span data-ttu-id="ab3a1-1424">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1424">Do</span></span>  | <span data-ttu-id="ab3a1-1425">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1425">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1426">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1426">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1427">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1427">Do</span></span> | <span data-ttu-id="ab3a1-1428">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1428">Err</span></span> | <span data-ttu-id="ab3a1-1429">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1429">Err</span></span> | <span data-ttu-id="ab3a1-1430">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1430">Do</span></span>  | <span data-ttu-id="ab3a1-1431">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1431">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1432">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1432">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1433">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1433">Err</span></span> | <span data-ttu-id="ab3a1-1434">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1434">Err</span></span> | <span data-ttu-id="ab3a1-1435">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1435">Err</span></span> | <span data-ttu-id="ab3a1-1436">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1436">Err</span></span> | 
| <span data-ttu-id="ab3a1-1437">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1437">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ab3a1-1438">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1438">Err</span></span> | <span data-ttu-id="ab3a1-1439">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1439">Err</span></span> | <span data-ttu-id="ab3a1-1440">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1440">Err</span></span> | 
| <span data-ttu-id="ab3a1-1441">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1441">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ab3a1-1442">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1442">Do</span></span>  | <span data-ttu-id="ab3a1-1443">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1443">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1444">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1444">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ab3a1-1445">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1445">Ob</span></span>  | 


### <a name="division-operators"></a><span data-ttu-id="ab3a1-1446">Operadores de divisão</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1446">Division Operators</span></span>

<span data-ttu-id="ab3a1-1447">O quociente de dois operandos de computação de operadores de divisão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1447">Division operators compute the quotient of two operands.</span></span> <span data-ttu-id="ab3a1-1448">Há dois operadores de divisão: o operador de divisão (ponto flutuante) regular e o operador de divisão de inteiro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1448">There are two division operators: the regular (floating-point) division operator and the integer division operator.</span></span>

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

<span data-ttu-id="ab3a1-1449">O operador de divisão regular é definido para os seguintes tipos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1449">The regular division operator is defined for the following types:</span></span>

* <span data-ttu-id="ab3a1-1450">`Single` e `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1450">`Single` and `Double`.</span></span> <span data-ttu-id="ab3a1-1451">O quociente é calculado de acordo com as regras de aritmética do IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1451">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="ab3a1-1452">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1452">`Decimal`.</span></span> <span data-ttu-id="ab3a1-1453">Se o valor do operando à direita for zero, um `System.DivideByZeroException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1453">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-1454">Se o valor resultante é muito grande para ser representado no formato decimal, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1454">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-1455">Se o valor do resultado for muito pequeno para ser representado no formato decimal, o resultado é zero.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1455">If the result value is too small to represent in the decimal format, the result is zero.</span></span> <span data-ttu-id="ab3a1-1456">A escala do resultado, antes de qualquer arredondamento, é a escala mais próxima à escala preferencial que preserva um resultado igual ao resultado exato.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1456">The scale of the result, before any rounding, is the closest scale to the preferred scale which will preserve a result equal to the exact result.</span></span>  <span data-ttu-id="ab3a1-1457">A escala preferencial é a escala do primeiro operando menor a escala do segundo operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1457">The preferred scale is the scale of the first operand less the scale of the second operand.</span></span>

<span data-ttu-id="ab3a1-1458">Acordo com as regras de resolução de operador normal, divisão regular puramente entre os operandos de tipos, como `Byte`, `Short`, `Integer`, e `Long` faria com que ambos os operandos para ser convertido no tipo `Decimal`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1458">According to normal operator resolution rules, regular division purely between operands of types such as `Byte`, `Short`, `Integer`, and `Long` would cause both operands to be converted to type `Decimal`.</span></span> <span data-ttu-id="ab3a1-1459">No entanto, ao fazer a resolução de operador no operador de divisão quando nenhum tipo é `Decimal`, `Double` é considerada mais estreitas do que `Decimal`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1459">However, when doing operator resolution on the division operator when neither type is `Decimal`, `Double` is considered narrower than `Decimal`.</span></span> <span data-ttu-id="ab3a1-1460">Essa convenção é seguida porque `Double` divisão é mais eficiente que `Decimal` divisão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1460">This convention is followed because `Double` division is more efficient than `Decimal` division.</span></span>

<span data-ttu-id="ab3a1-1461">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1461">__Operation Type:__</span></span>

|        | <span data-ttu-id="ab3a1-1462">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1462">__Bo__</span></span> | <span data-ttu-id="ab3a1-1463">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1463">__SB__</span></span> | <span data-ttu-id="ab3a1-1464">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1464">__By__</span></span> | <span data-ttu-id="ab3a1-1465">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1465">__Sh__</span></span> | <span data-ttu-id="ab3a1-1466">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1466">__US__</span></span> | <span data-ttu-id="ab3a1-1467">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1467">__In__</span></span> | <span data-ttu-id="ab3a1-1468">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1468">__UI__</span></span> | <span data-ttu-id="ab3a1-1469">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1469">__Lo__</span></span> | <span data-ttu-id="ab3a1-1470">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1470">__UL__</span></span> | <span data-ttu-id="ab3a1-1471">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1471">__De__</span></span> | <span data-ttu-id="ab3a1-1472">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1472">__Si__</span></span> | <span data-ttu-id="ab3a1-1473">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1473">__Do__</span></span> | <span data-ttu-id="ab3a1-1474">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1474">__Da__</span></span>  | <span data-ttu-id="ab3a1-1475">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1475">__Ch__</span></span>  | <span data-ttu-id="ab3a1-1476">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1476">__St__</span></span> | <span data-ttu-id="ab3a1-1477">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1477">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ab3a1-1478">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1478">__Bo__</span></span> | <span data-ttu-id="ab3a1-1479">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1479">Do</span></span> | <span data-ttu-id="ab3a1-1480">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1480">Do</span></span> | <span data-ttu-id="ab3a1-1481">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1481">Do</span></span> | <span data-ttu-id="ab3a1-1482">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1482">Do</span></span> | <span data-ttu-id="ab3a1-1483">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1483">Do</span></span> | <span data-ttu-id="ab3a1-1484">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1484">Do</span></span> | <span data-ttu-id="ab3a1-1485">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1485">Do</span></span> | <span data-ttu-id="ab3a1-1486">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1486">Do</span></span> | <span data-ttu-id="ab3a1-1487">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1487">Do</span></span> | <span data-ttu-id="ab3a1-1488">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1488">De</span></span> | <span data-ttu-id="ab3a1-1489">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1489">Si</span></span> | <span data-ttu-id="ab3a1-1490">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1490">Do</span></span> | <span data-ttu-id="ab3a1-1491">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1491">Err</span></span> | <span data-ttu-id="ab3a1-1492">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1492">Err</span></span> | <span data-ttu-id="ab3a1-1493">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1493">Do</span></span>  | <span data-ttu-id="ab3a1-1494">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1494">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1495">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1495">__SB__</span></span> |    | <span data-ttu-id="ab3a1-1496">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1496">Do</span></span> | <span data-ttu-id="ab3a1-1497">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1497">Do</span></span> | <span data-ttu-id="ab3a1-1498">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1498">Do</span></span> | <span data-ttu-id="ab3a1-1499">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1499">Do</span></span> | <span data-ttu-id="ab3a1-1500">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1500">Do</span></span> | <span data-ttu-id="ab3a1-1501">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1501">Do</span></span> | <span data-ttu-id="ab3a1-1502">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1502">Do</span></span> | <span data-ttu-id="ab3a1-1503">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1503">Do</span></span> | <span data-ttu-id="ab3a1-1504">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1504">De</span></span> | <span data-ttu-id="ab3a1-1505">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1505">Si</span></span> | <span data-ttu-id="ab3a1-1506">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1506">Do</span></span> | <span data-ttu-id="ab3a1-1507">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1507">Err</span></span> | <span data-ttu-id="ab3a1-1508">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1508">Err</span></span> | <span data-ttu-id="ab3a1-1509">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1509">Do</span></span>  | <span data-ttu-id="ab3a1-1510">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1510">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1511">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1511">__By__</span></span> |    |    | <span data-ttu-id="ab3a1-1512">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1512">Do</span></span> | <span data-ttu-id="ab3a1-1513">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1513">Do</span></span> | <span data-ttu-id="ab3a1-1514">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1514">Do</span></span> | <span data-ttu-id="ab3a1-1515">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1515">Do</span></span> | <span data-ttu-id="ab3a1-1516">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1516">Do</span></span> | <span data-ttu-id="ab3a1-1517">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1517">Do</span></span> | <span data-ttu-id="ab3a1-1518">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1518">Do</span></span> | <span data-ttu-id="ab3a1-1519">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1519">De</span></span> | <span data-ttu-id="ab3a1-1520">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1520">Si</span></span> | <span data-ttu-id="ab3a1-1521">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1521">Do</span></span> | <span data-ttu-id="ab3a1-1522">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1522">Err</span></span> | <span data-ttu-id="ab3a1-1523">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1523">Err</span></span> | <span data-ttu-id="ab3a1-1524">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1524">Do</span></span>  | <span data-ttu-id="ab3a1-1525">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1525">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1526">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1526">__Sh__</span></span> |    |    |    | <span data-ttu-id="ab3a1-1527">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1527">Do</span></span> | <span data-ttu-id="ab3a1-1528">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1528">Do</span></span> | <span data-ttu-id="ab3a1-1529">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1529">Do</span></span> | <span data-ttu-id="ab3a1-1530">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1530">Do</span></span> | <span data-ttu-id="ab3a1-1531">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1531">Do</span></span> | <span data-ttu-id="ab3a1-1532">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1532">Do</span></span> | <span data-ttu-id="ab3a1-1533">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1533">De</span></span> | <span data-ttu-id="ab3a1-1534">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1534">Si</span></span> | <span data-ttu-id="ab3a1-1535">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1535">Do</span></span> | <span data-ttu-id="ab3a1-1536">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1536">Err</span></span> | <span data-ttu-id="ab3a1-1537">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1537">Err</span></span> | <span data-ttu-id="ab3a1-1538">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1538">Do</span></span>  | <span data-ttu-id="ab3a1-1539">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1539">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1540">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1540">__US__</span></span> |    |    |    |    | <span data-ttu-id="ab3a1-1541">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1541">Do</span></span> | <span data-ttu-id="ab3a1-1542">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1542">Do</span></span> | <span data-ttu-id="ab3a1-1543">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1543">Do</span></span> | <span data-ttu-id="ab3a1-1544">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1544">Do</span></span> | <span data-ttu-id="ab3a1-1545">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1545">Do</span></span> | <span data-ttu-id="ab3a1-1546">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1546">De</span></span> | <span data-ttu-id="ab3a1-1547">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1547">Si</span></span> | <span data-ttu-id="ab3a1-1548">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1548">Do</span></span> | <span data-ttu-id="ab3a1-1549">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1549">Err</span></span> | <span data-ttu-id="ab3a1-1550">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1550">Err</span></span> | <span data-ttu-id="ab3a1-1551">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1551">Do</span></span>  | <span data-ttu-id="ab3a1-1552">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1552">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1553">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1553">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ab3a1-1554">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1554">Do</span></span> | <span data-ttu-id="ab3a1-1555">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1555">Do</span></span> | <span data-ttu-id="ab3a1-1556">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1556">Do</span></span> | <span data-ttu-id="ab3a1-1557">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1557">Do</span></span> | <span data-ttu-id="ab3a1-1558">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1558">De</span></span> | <span data-ttu-id="ab3a1-1559">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1559">Si</span></span> | <span data-ttu-id="ab3a1-1560">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1560">Do</span></span> | <span data-ttu-id="ab3a1-1561">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1561">Err</span></span> | <span data-ttu-id="ab3a1-1562">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1562">Err</span></span> | <span data-ttu-id="ab3a1-1563">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1563">Do</span></span>  | <span data-ttu-id="ab3a1-1564">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1564">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1565">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1565">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1566">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1566">Do</span></span> | <span data-ttu-id="ab3a1-1567">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1567">Do</span></span> | <span data-ttu-id="ab3a1-1568">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1568">Do</span></span> | <span data-ttu-id="ab3a1-1569">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1569">De</span></span> | <span data-ttu-id="ab3a1-1570">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1570">Si</span></span> | <span data-ttu-id="ab3a1-1571">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1571">Do</span></span> | <span data-ttu-id="ab3a1-1572">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1572">Err</span></span> | <span data-ttu-id="ab3a1-1573">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1573">Err</span></span> | <span data-ttu-id="ab3a1-1574">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1574">Do</span></span>  | <span data-ttu-id="ab3a1-1575">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1575">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1576">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1577">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1577">Do</span></span> | <span data-ttu-id="ab3a1-1578">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1578">Do</span></span> | <span data-ttu-id="ab3a1-1579">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1579">De</span></span> | <span data-ttu-id="ab3a1-1580">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1580">Si</span></span> | <span data-ttu-id="ab3a1-1581">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1581">Do</span></span> | <span data-ttu-id="ab3a1-1582">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1582">Err</span></span> | <span data-ttu-id="ab3a1-1583">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1583">Err</span></span> | <span data-ttu-id="ab3a1-1584">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1584">Do</span></span>  | <span data-ttu-id="ab3a1-1585">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1585">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1586">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1586">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1587">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1587">Do</span></span> | <span data-ttu-id="ab3a1-1588">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1588">De</span></span> | <span data-ttu-id="ab3a1-1589">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1589">Si</span></span> | <span data-ttu-id="ab3a1-1590">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1590">Do</span></span> | <span data-ttu-id="ab3a1-1591">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1591">Err</span></span> | <span data-ttu-id="ab3a1-1592">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1592">Err</span></span> | <span data-ttu-id="ab3a1-1593">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1593">Do</span></span>  | <span data-ttu-id="ab3a1-1594">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1594">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1595">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1595">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1596">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1596">De</span></span> | <span data-ttu-id="ab3a1-1597">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1597">Si</span></span> | <span data-ttu-id="ab3a1-1598">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1598">Do</span></span> | <span data-ttu-id="ab3a1-1599">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1599">Err</span></span> | <span data-ttu-id="ab3a1-1600">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1600">Err</span></span> | <span data-ttu-id="ab3a1-1601">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1601">Do</span></span>  | <span data-ttu-id="ab3a1-1602">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1602">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1603">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1603">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1604">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1604">Si</span></span> | <span data-ttu-id="ab3a1-1605">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1605">Do</span></span> | <span data-ttu-id="ab3a1-1606">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1606">Err</span></span> | <span data-ttu-id="ab3a1-1607">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1607">Err</span></span> | <span data-ttu-id="ab3a1-1608">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1608">Do</span></span>  | <span data-ttu-id="ab3a1-1609">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1609">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1610">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1610">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1611">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1611">Do</span></span> | <span data-ttu-id="ab3a1-1612">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1612">Err</span></span> | <span data-ttu-id="ab3a1-1613">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1613">Err</span></span> | <span data-ttu-id="ab3a1-1614">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1614">Do</span></span>  | <span data-ttu-id="ab3a1-1615">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1615">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1616">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1616">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1617">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1617">Err</span></span> | <span data-ttu-id="ab3a1-1618">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1618">Err</span></span> | <span data-ttu-id="ab3a1-1619">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1619">Err</span></span> | <span data-ttu-id="ab3a1-1620">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1620">Err</span></span> | 
| <span data-ttu-id="ab3a1-1621">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1621">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ab3a1-1622">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1622">Err</span></span> | <span data-ttu-id="ab3a1-1623">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1623">Err</span></span> | <span data-ttu-id="ab3a1-1624">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1624">Err</span></span> | 
| <span data-ttu-id="ab3a1-1625">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1625">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ab3a1-1626">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1626">Do</span></span>  | <span data-ttu-id="ab3a1-1627">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1627">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1628">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1628">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ab3a1-1629">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1629">Ob</span></span>  | 

<span data-ttu-id="ab3a1-1630">O operador de divisão de inteiro é definido para `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, e `Long`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1630">The integer division operator is defined for `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="ab3a1-1631">Se o valor do operando à direita for zero, um `System.DivideByZeroException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1631">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-1632">A divisão Arredonda o resultado em direção a zero e o valor absoluto do resultado é o maior inteiro possíveis que é menor que o valor absoluto do quociente de dois operandos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1632">The division rounds the result towards zero, and the absolute value of the result is the largest possible integer that is less than the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="ab3a1-1633">O resultado é zero ou positivo quando os dois operandos tiverem o mesmo sinal e zero ou negativo quando os dois operandos tiverem oposta sinais.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1633">The result is zero or positive when the two operands have the same sign, and zero or negative when the two operands have opposite signs.</span></span> <span data-ttu-id="ab3a1-1634">Se o operando esquerdo é o negativo máximo `SByte`, `Short`, `Integer`, ou `Long`, e o operando direito é `-1`, ocorre um estouro; se a verificação de estouro de inteiro está ativada, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1634">If the left operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, and the right operand is `-1`, an overflow occurs; if integer overflow checking is on, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-1635">Caso contrário, o estouro não é relatado e, em vez disso, o resultado é o valor do operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1635">Otherwise, the overflow is not reported and the result is instead the value of the left operand.</span></span>

<span data-ttu-id="ab3a1-1636">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1636">__Note.__</span></span> <span data-ttu-id="ab3a1-1637">Como os dois operandos de tipos sem sinal sempre será zero ou positivo, o resultado é sempre zero ou positivo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1637">As the two operands for unsigned types will always be zero or positive, the result is always zero or positive.</span></span>  <span data-ttu-id="ab3a1-1638">Como o resultado da expressão sempre será menor ou igual ao maior dos dois operandos, não é possível que ocorra um estouro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1638">As the result of the expression will always be less than or equal to the largest of the two operands, it is not possible for an overflow to occur.</span></span>  <span data-ttu-id="ab3a1-1639">Como tal, a verificação de estouro de inteiro não é executada para a divisão de inteiro com dois inteiros sem sinal.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1639">As such integer overflow checking is not performed for integer divide with two unsigned integers.</span></span> <span data-ttu-id="ab3a1-1640">O resultado é o tipo do operando à esquerda.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1640">The result is the type as that of the left operand.</span></span>


<span data-ttu-id="ab3a1-1641">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1641">__Operation Type:__</span></span>

|        | <span data-ttu-id="ab3a1-1642">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1642">__Bo__</span></span> | <span data-ttu-id="ab3a1-1643">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1643">__SB__</span></span> | <span data-ttu-id="ab3a1-1644">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1644">__By__</span></span> | <span data-ttu-id="ab3a1-1645">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1645">__Sh__</span></span> | <span data-ttu-id="ab3a1-1646">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1646">__US__</span></span> | <span data-ttu-id="ab3a1-1647">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1647">__In__</span></span> | <span data-ttu-id="ab3a1-1648">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1648">__UI__</span></span> | <span data-ttu-id="ab3a1-1649">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1649">__Lo__</span></span> | <span data-ttu-id="ab3a1-1650">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1650">__UL__</span></span> | <span data-ttu-id="ab3a1-1651">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1651">__De__</span></span> | <span data-ttu-id="ab3a1-1652">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1652">__Si__</span></span> | <span data-ttu-id="ab3a1-1653">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1653">__Do__</span></span> | <span data-ttu-id="ab3a1-1654">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1654">__Da__</span></span>  | <span data-ttu-id="ab3a1-1655">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1655">__Ch__</span></span>  | <span data-ttu-id="ab3a1-1656">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1656">__St__</span></span> | <span data-ttu-id="ab3a1-1657">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1657">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ab3a1-1658">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1658">__Bo__</span></span> | <span data-ttu-id="ab3a1-1659">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1659">Sh</span></span> | <span data-ttu-id="ab3a1-1660">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1660">SB</span></span> | <span data-ttu-id="ab3a1-1661">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1661">Sh</span></span> | <span data-ttu-id="ab3a1-1662">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1662">Sh</span></span> | <span data-ttu-id="ab3a1-1663">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1663">In</span></span> | <span data-ttu-id="ab3a1-1664">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1664">In</span></span> | <span data-ttu-id="ab3a1-1665">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1665">Lo</span></span> | <span data-ttu-id="ab3a1-1666">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1666">Lo</span></span> | <span data-ttu-id="ab3a1-1667">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1667">Lo</span></span> | <span data-ttu-id="ab3a1-1668">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1668">Lo</span></span> | <span data-ttu-id="ab3a1-1669">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1669">Lo</span></span> | <span data-ttu-id="ab3a1-1670">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1670">Lo</span></span> | <span data-ttu-id="ab3a1-1671">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1671">Err</span></span> | <span data-ttu-id="ab3a1-1672">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1672">Err</span></span> | <span data-ttu-id="ab3a1-1673">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1673">Lo</span></span>  | <span data-ttu-id="ab3a1-1674">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1674">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1675">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1675">__SB__</span></span> |    | <span data-ttu-id="ab3a1-1676">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1676">SB</span></span> | <span data-ttu-id="ab3a1-1677">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1677">Sh</span></span> | <span data-ttu-id="ab3a1-1678">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1678">Sh</span></span> | <span data-ttu-id="ab3a1-1679">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1679">In</span></span> | <span data-ttu-id="ab3a1-1680">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1680">In</span></span> | <span data-ttu-id="ab3a1-1681">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1681">Lo</span></span> | <span data-ttu-id="ab3a1-1682">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1682">Lo</span></span> | <span data-ttu-id="ab3a1-1683">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1683">Lo</span></span> | <span data-ttu-id="ab3a1-1684">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1684">Lo</span></span> | <span data-ttu-id="ab3a1-1685">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1685">Lo</span></span> | <span data-ttu-id="ab3a1-1686">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1686">Lo</span></span> | <span data-ttu-id="ab3a1-1687">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1687">Err</span></span> | <span data-ttu-id="ab3a1-1688">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1688">Err</span></span> | <span data-ttu-id="ab3a1-1689">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1689">Lo</span></span>  | <span data-ttu-id="ab3a1-1690">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1690">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1691">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1691">__By__</span></span> |    |    | <span data-ttu-id="ab3a1-1692">Por</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1692">By</span></span> | <span data-ttu-id="ab3a1-1693">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1693">Sh</span></span> | <span data-ttu-id="ab3a1-1694">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1694">US</span></span> | <span data-ttu-id="ab3a1-1695">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1695">In</span></span> | <span data-ttu-id="ab3a1-1696">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1696">UI</span></span> | <span data-ttu-id="ab3a1-1697">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1697">Lo</span></span> | <span data-ttu-id="ab3a1-1698">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1698">UL</span></span> | <span data-ttu-id="ab3a1-1699">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1699">Lo</span></span> | <span data-ttu-id="ab3a1-1700">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1700">Lo</span></span> | <span data-ttu-id="ab3a1-1701">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1701">Lo</span></span> | <span data-ttu-id="ab3a1-1702">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1702">Err</span></span> | <span data-ttu-id="ab3a1-1703">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1703">Err</span></span> | <span data-ttu-id="ab3a1-1704">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1704">Lo</span></span>  | <span data-ttu-id="ab3a1-1705">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1705">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1706">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1706">__Sh__</span></span> |    |    |    | <span data-ttu-id="ab3a1-1707">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1707">Sh</span></span> | <span data-ttu-id="ab3a1-1708">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1708">In</span></span> | <span data-ttu-id="ab3a1-1709">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1709">In</span></span> | <span data-ttu-id="ab3a1-1710">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1710">Lo</span></span> | <span data-ttu-id="ab3a1-1711">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1711">Lo</span></span> | <span data-ttu-id="ab3a1-1712">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1712">Lo</span></span> | <span data-ttu-id="ab3a1-1713">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1713">Lo</span></span> | <span data-ttu-id="ab3a1-1714">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1714">Lo</span></span> | <span data-ttu-id="ab3a1-1715">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1715">Lo</span></span> | <span data-ttu-id="ab3a1-1716">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1716">Err</span></span> | <span data-ttu-id="ab3a1-1717">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1717">Err</span></span> | <span data-ttu-id="ab3a1-1718">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1718">Lo</span></span>  | <span data-ttu-id="ab3a1-1719">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1719">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1720">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1720">__US__</span></span> |    |    |    |    | <span data-ttu-id="ab3a1-1721">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1721">US</span></span> | <span data-ttu-id="ab3a1-1722">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1722">In</span></span> | <span data-ttu-id="ab3a1-1723">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1723">UI</span></span> | <span data-ttu-id="ab3a1-1724">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1724">Lo</span></span> | <span data-ttu-id="ab3a1-1725">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1725">UL</span></span> | <span data-ttu-id="ab3a1-1726">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1726">Lo</span></span> | <span data-ttu-id="ab3a1-1727">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1727">Lo</span></span> | <span data-ttu-id="ab3a1-1728">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1728">Lo</span></span> | <span data-ttu-id="ab3a1-1729">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1729">Err</span></span> | <span data-ttu-id="ab3a1-1730">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1730">Err</span></span> | <span data-ttu-id="ab3a1-1731">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1731">Lo</span></span>  | <span data-ttu-id="ab3a1-1732">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1732">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1733">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1733">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ab3a1-1734">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1734">In</span></span> | <span data-ttu-id="ab3a1-1735">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1735">Lo</span></span> | <span data-ttu-id="ab3a1-1736">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1736">Lo</span></span> | <span data-ttu-id="ab3a1-1737">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1737">Lo</span></span> | <span data-ttu-id="ab3a1-1738">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1738">Lo</span></span> | <span data-ttu-id="ab3a1-1739">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1739">Lo</span></span> | <span data-ttu-id="ab3a1-1740">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1740">Lo</span></span> | <span data-ttu-id="ab3a1-1741">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1741">Err</span></span> | <span data-ttu-id="ab3a1-1742">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1742">Err</span></span> | <span data-ttu-id="ab3a1-1743">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1743">Lo</span></span>  | <span data-ttu-id="ab3a1-1744">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1744">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1745">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1745">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1746">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1746">UI</span></span> | <span data-ttu-id="ab3a1-1747">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1747">Lo</span></span> | <span data-ttu-id="ab3a1-1748">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1748">UL</span></span> | <span data-ttu-id="ab3a1-1749">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1749">Lo</span></span> | <span data-ttu-id="ab3a1-1750">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1750">Lo</span></span> | <span data-ttu-id="ab3a1-1751">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1751">Lo</span></span> | <span data-ttu-id="ab3a1-1752">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1752">Err</span></span> | <span data-ttu-id="ab3a1-1753">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1753">Err</span></span> | <span data-ttu-id="ab3a1-1754">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1754">Lo</span></span>  | <span data-ttu-id="ab3a1-1755">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1755">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1756">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1756">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1757">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1757">Lo</span></span> | <span data-ttu-id="ab3a1-1758">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1758">Lo</span></span> | <span data-ttu-id="ab3a1-1759">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1759">Lo</span></span> | <span data-ttu-id="ab3a1-1760">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1760">Lo</span></span> | <span data-ttu-id="ab3a1-1761">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1761">Lo</span></span> | <span data-ttu-id="ab3a1-1762">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1762">Err</span></span> | <span data-ttu-id="ab3a1-1763">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1763">Err</span></span> | <span data-ttu-id="ab3a1-1764">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1764">Lo</span></span>  | <span data-ttu-id="ab3a1-1765">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1765">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1766">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1766">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1767">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1767">UL</span></span> | <span data-ttu-id="ab3a1-1768">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1768">Lo</span></span> | <span data-ttu-id="ab3a1-1769">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1769">Lo</span></span> | <span data-ttu-id="ab3a1-1770">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1770">Lo</span></span> | <span data-ttu-id="ab3a1-1771">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1771">Err</span></span> | <span data-ttu-id="ab3a1-1772">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1772">Err</span></span> | <span data-ttu-id="ab3a1-1773">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1773">Lo</span></span>  | <span data-ttu-id="ab3a1-1774">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1774">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1775">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1775">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1776">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1776">Lo</span></span> | <span data-ttu-id="ab3a1-1777">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1777">Lo</span></span> | <span data-ttu-id="ab3a1-1778">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1778">Lo</span></span> | <span data-ttu-id="ab3a1-1779">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1779">Err</span></span> | <span data-ttu-id="ab3a1-1780">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1780">Err</span></span> | <span data-ttu-id="ab3a1-1781">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1781">Lo</span></span>  | <span data-ttu-id="ab3a1-1782">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1782">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1783">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1783">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1784">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1784">Lo</span></span> | <span data-ttu-id="ab3a1-1785">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1785">Lo</span></span> | <span data-ttu-id="ab3a1-1786">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1786">Err</span></span> | <span data-ttu-id="ab3a1-1787">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1787">Err</span></span> | <span data-ttu-id="ab3a1-1788">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1788">Lo</span></span>  | <span data-ttu-id="ab3a1-1789">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1789">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1790">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1790">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1791">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1791">Lo</span></span> | <span data-ttu-id="ab3a1-1792">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1792">Err</span></span> | <span data-ttu-id="ab3a1-1793">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1793">Err</span></span> | <span data-ttu-id="ab3a1-1794">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1794">Lo</span></span>  | <span data-ttu-id="ab3a1-1795">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1795">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1796">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1796">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1797">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1797">Err</span></span> | <span data-ttu-id="ab3a1-1798">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1798">Err</span></span> | <span data-ttu-id="ab3a1-1799">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1799">Err</span></span> | <span data-ttu-id="ab3a1-1800">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1800">Err</span></span> | 
| <span data-ttu-id="ab3a1-1801">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1801">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ab3a1-1802">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1802">Err</span></span> | <span data-ttu-id="ab3a1-1803">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1803">Err</span></span> | <span data-ttu-id="ab3a1-1804">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1804">Err</span></span> | 
| <span data-ttu-id="ab3a1-1805">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1805">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ab3a1-1806">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1806">Lo</span></span>  | <span data-ttu-id="ab3a1-1807">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1807">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1808">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ab3a1-1809">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1809">Ob</span></span>  | 


### <a name="mod-operator"></a><span data-ttu-id="ab3a1-1810">Operador Mod</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1810">Mod Operator</span></span>

<span data-ttu-id="ab3a1-1811">O `Mod` (operador módulo) calcula o resto da divisão entre dois operandos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1811">The `Mod` (modulo) operator computes the remainder of the division between two operands.</span></span>

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

<span data-ttu-id="ab3a1-1812">O `Mod` operador está definido para os seguintes tipos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1812">The `Mod` operator is defined for the following types:</span></span>

* <span data-ttu-id="ab3a1-1813">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` e `Long`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1813">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long`.</span></span> <span data-ttu-id="ab3a1-1814">O resultado de `x Mod y` é o valor produzido por `x - (x \ y) * y`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1814">The result of `x Mod y` is the value produced by `x - (x \ y) * y`.</span></span> <span data-ttu-id="ab3a1-1815">Se `y` for zero, um `System.DivideByZeroException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1815">If `y` is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-1816">O operador de módulo nunca faz com que um estouro.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1816">The modulo operator never causes an overflow.</span></span>

* <span data-ttu-id="ab3a1-1817">`Single` e `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1817">`Single` and `Double`.</span></span> <span data-ttu-id="ab3a1-1818">O restante é calculado de acordo com as regras de aritmética do IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1818">The remainder is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="ab3a1-1819">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1819">`Decimal`.</span></span> <span data-ttu-id="ab3a1-1820">Se o valor do operando à direita for zero, um `System.DivideByZeroException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1820">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-1821">Se o valor resultante é muito grande para ser representado no formato decimal, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1821">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ab3a1-1822">Se o valor do resultado for muito pequeno para ser representado no formato decimal, o resultado é zero.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1822">If the result value is too small to represent in the decimal format, the result is zero.</span></span>

<span data-ttu-id="ab3a1-1823">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1823">__Operation Type:__</span></span>

|        | <span data-ttu-id="ab3a1-1824">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1824">__Bo__</span></span> | <span data-ttu-id="ab3a1-1825">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1825">__SB__</span></span> | <span data-ttu-id="ab3a1-1826">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1826">__By__</span></span> | <span data-ttu-id="ab3a1-1827">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1827">__Sh__</span></span> | <span data-ttu-id="ab3a1-1828">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1828">__US__</span></span> | <span data-ttu-id="ab3a1-1829">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1829">__In__</span></span> | <span data-ttu-id="ab3a1-1830">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1830">__UI__</span></span> | <span data-ttu-id="ab3a1-1831">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1831">__Lo__</span></span> | <span data-ttu-id="ab3a1-1832">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1832">__UL__</span></span> | <span data-ttu-id="ab3a1-1833">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1833">__De__</span></span> | <span data-ttu-id="ab3a1-1834">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1834">__Si__</span></span> | <span data-ttu-id="ab3a1-1835">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1835">__Do__</span></span> | <span data-ttu-id="ab3a1-1836">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1836">__Da__</span></span>  | <span data-ttu-id="ab3a1-1837">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1837">__Ch__</span></span>  | <span data-ttu-id="ab3a1-1838">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1838">__St__</span></span> | <span data-ttu-id="ab3a1-1839">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1839">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ab3a1-1840">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1840">__Bo__</span></span> | <span data-ttu-id="ab3a1-1841">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1841">Sh</span></span> | <span data-ttu-id="ab3a1-1842">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1842">SB</span></span> | <span data-ttu-id="ab3a1-1843">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1843">Sh</span></span> | <span data-ttu-id="ab3a1-1844">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1844">Sh</span></span> | <span data-ttu-id="ab3a1-1845">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1845">In</span></span> | <span data-ttu-id="ab3a1-1846">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1846">In</span></span> | <span data-ttu-id="ab3a1-1847">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1847">Lo</span></span> | <span data-ttu-id="ab3a1-1848">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1848">Lo</span></span> | <span data-ttu-id="ab3a1-1849">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1849">De</span></span> | <span data-ttu-id="ab3a1-1850">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1850">De</span></span> | <span data-ttu-id="ab3a1-1851">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1851">Si</span></span> | <span data-ttu-id="ab3a1-1852">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1852">Do</span></span> | <span data-ttu-id="ab3a1-1853">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1853">Err</span></span> | <span data-ttu-id="ab3a1-1854">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1854">Err</span></span> | <span data-ttu-id="ab3a1-1855">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1855">Do</span></span>  | <span data-ttu-id="ab3a1-1856">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1856">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1857">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1857">__SB__</span></span> |    | <span data-ttu-id="ab3a1-1858">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1858">SB</span></span> | <span data-ttu-id="ab3a1-1859">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1859">Sh</span></span> | <span data-ttu-id="ab3a1-1860">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1860">Sh</span></span> | <span data-ttu-id="ab3a1-1861">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1861">In</span></span> | <span data-ttu-id="ab3a1-1862">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1862">In</span></span> | <span data-ttu-id="ab3a1-1863">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1863">Lo</span></span> | <span data-ttu-id="ab3a1-1864">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1864">Lo</span></span> | <span data-ttu-id="ab3a1-1865">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1865">De</span></span> | <span data-ttu-id="ab3a1-1866">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1866">De</span></span> | <span data-ttu-id="ab3a1-1867">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1867">Si</span></span> | <span data-ttu-id="ab3a1-1868">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1868">Do</span></span> | <span data-ttu-id="ab3a1-1869">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1869">Err</span></span> | <span data-ttu-id="ab3a1-1870">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1870">Err</span></span> | <span data-ttu-id="ab3a1-1871">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1871">Do</span></span>  | <span data-ttu-id="ab3a1-1872">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1872">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1873">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1873">__By__</span></span> |    |    | <span data-ttu-id="ab3a1-1874">Por</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1874">By</span></span> | <span data-ttu-id="ab3a1-1875">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1875">Sh</span></span> | <span data-ttu-id="ab3a1-1876">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1876">US</span></span> | <span data-ttu-id="ab3a1-1877">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1877">In</span></span> | <span data-ttu-id="ab3a1-1878">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1878">UI</span></span> | <span data-ttu-id="ab3a1-1879">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1879">Lo</span></span> | <span data-ttu-id="ab3a1-1880">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1880">UL</span></span> | <span data-ttu-id="ab3a1-1881">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1881">De</span></span> | <span data-ttu-id="ab3a1-1882">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1882">Si</span></span> | <span data-ttu-id="ab3a1-1883">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1883">Do</span></span> | <span data-ttu-id="ab3a1-1884">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1884">Err</span></span> | <span data-ttu-id="ab3a1-1885">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1885">Err</span></span> | <span data-ttu-id="ab3a1-1886">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1886">Do</span></span>  | <span data-ttu-id="ab3a1-1887">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1887">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1888">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1888">__Sh__</span></span> |    |    |    | <span data-ttu-id="ab3a1-1889">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1889">Sh</span></span> | <span data-ttu-id="ab3a1-1890">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1890">In</span></span> | <span data-ttu-id="ab3a1-1891">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1891">In</span></span> | <span data-ttu-id="ab3a1-1892">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1892">Lo</span></span> | <span data-ttu-id="ab3a1-1893">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1893">Lo</span></span> | <span data-ttu-id="ab3a1-1894">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1894">De</span></span> | <span data-ttu-id="ab3a1-1895">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1895">De</span></span> | <span data-ttu-id="ab3a1-1896">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1896">Si</span></span> | <span data-ttu-id="ab3a1-1897">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1897">Do</span></span> | <span data-ttu-id="ab3a1-1898">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1898">Err</span></span> | <span data-ttu-id="ab3a1-1899">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1899">Err</span></span> | <span data-ttu-id="ab3a1-1900">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1900">Do</span></span>  | <span data-ttu-id="ab3a1-1901">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1901">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1902">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1902">__US__</span></span> |    |    |    |    | <span data-ttu-id="ab3a1-1903">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1903">US</span></span> | <span data-ttu-id="ab3a1-1904">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1904">In</span></span> | <span data-ttu-id="ab3a1-1905">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1905">UI</span></span> | <span data-ttu-id="ab3a1-1906">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1906">Lo</span></span> | <span data-ttu-id="ab3a1-1907">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1907">UL</span></span> | <span data-ttu-id="ab3a1-1908">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1908">De</span></span> | <span data-ttu-id="ab3a1-1909">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1909">Si</span></span> | <span data-ttu-id="ab3a1-1910">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1910">Do</span></span> | <span data-ttu-id="ab3a1-1911">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1911">Err</span></span> | <span data-ttu-id="ab3a1-1912">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1912">Err</span></span> | <span data-ttu-id="ab3a1-1913">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1913">Do</span></span>  | <span data-ttu-id="ab3a1-1914">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1914">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1915">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1915">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ab3a1-1916">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1916">In</span></span> | <span data-ttu-id="ab3a1-1917">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1917">Lo</span></span> | <span data-ttu-id="ab3a1-1918">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1918">Lo</span></span> | <span data-ttu-id="ab3a1-1919">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1919">De</span></span> | <span data-ttu-id="ab3a1-1920">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1920">De</span></span> | <span data-ttu-id="ab3a1-1921">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1921">Si</span></span> | <span data-ttu-id="ab3a1-1922">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1922">Do</span></span> | <span data-ttu-id="ab3a1-1923">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1923">Err</span></span> | <span data-ttu-id="ab3a1-1924">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1924">Err</span></span> | <span data-ttu-id="ab3a1-1925">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1925">Do</span></span>  | <span data-ttu-id="ab3a1-1926">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1926">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1927">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1927">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1928">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1928">UI</span></span> | <span data-ttu-id="ab3a1-1929">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1929">Lo</span></span> | <span data-ttu-id="ab3a1-1930">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1930">UL</span></span> | <span data-ttu-id="ab3a1-1931">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1931">De</span></span> | <span data-ttu-id="ab3a1-1932">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1932">Si</span></span> | <span data-ttu-id="ab3a1-1933">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1933">Do</span></span> | <span data-ttu-id="ab3a1-1934">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1934">Err</span></span> | <span data-ttu-id="ab3a1-1935">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1935">Err</span></span> | <span data-ttu-id="ab3a1-1936">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1936">Do</span></span>  | <span data-ttu-id="ab3a1-1937">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1937">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1938">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1938">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1939">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1939">Lo</span></span> | <span data-ttu-id="ab3a1-1940">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1940">De</span></span> | <span data-ttu-id="ab3a1-1941">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1941">De</span></span> | <span data-ttu-id="ab3a1-1942">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1942">Si</span></span> | <span data-ttu-id="ab3a1-1943">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1943">Do</span></span> | <span data-ttu-id="ab3a1-1944">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1944">Err</span></span> | <span data-ttu-id="ab3a1-1945">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1945">Err</span></span> | <span data-ttu-id="ab3a1-1946">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1946">Do</span></span>  | <span data-ttu-id="ab3a1-1947">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1947">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1948">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1948">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1949">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1949">UL</span></span> | <span data-ttu-id="ab3a1-1950">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1950">De</span></span> | <span data-ttu-id="ab3a1-1951">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1951">Si</span></span> | <span data-ttu-id="ab3a1-1952">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1952">Do</span></span> | <span data-ttu-id="ab3a1-1953">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1953">Err</span></span> | <span data-ttu-id="ab3a1-1954">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1954">Err</span></span> | <span data-ttu-id="ab3a1-1955">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1955">Do</span></span>  | <span data-ttu-id="ab3a1-1956">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1956">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1957">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1957">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1958">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1958">De</span></span> | <span data-ttu-id="ab3a1-1959">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1959">Si</span></span> | <span data-ttu-id="ab3a1-1960">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1960">Do</span></span> | <span data-ttu-id="ab3a1-1961">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1961">Err</span></span> | <span data-ttu-id="ab3a1-1962">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1962">Err</span></span> | <span data-ttu-id="ab3a1-1963">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1963">Do</span></span>  | <span data-ttu-id="ab3a1-1964">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1964">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1965">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1965">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1966">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1966">Si</span></span> | <span data-ttu-id="ab3a1-1967">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1967">Do</span></span> | <span data-ttu-id="ab3a1-1968">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1968">Err</span></span> | <span data-ttu-id="ab3a1-1969">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1969">Err</span></span> | <span data-ttu-id="ab3a1-1970">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1970">Do</span></span>  | <span data-ttu-id="ab3a1-1971">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1971">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1972">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1972">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1973">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1973">Do</span></span> | <span data-ttu-id="ab3a1-1974">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1974">Err</span></span> | <span data-ttu-id="ab3a1-1975">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1975">Err</span></span> | <span data-ttu-id="ab3a1-1976">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1976">Do</span></span>  | <span data-ttu-id="ab3a1-1977">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1977">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1978">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1978">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-1979">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1979">Err</span></span> | <span data-ttu-id="ab3a1-1980">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1980">Err</span></span> | <span data-ttu-id="ab3a1-1981">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1981">Err</span></span> | <span data-ttu-id="ab3a1-1982">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1982">Err</span></span> | 
| <span data-ttu-id="ab3a1-1983">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1983">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ab3a1-1984">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1984">Err</span></span> | <span data-ttu-id="ab3a1-1985">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1985">Err</span></span> | <span data-ttu-id="ab3a1-1986">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1986">Err</span></span> | 
| <span data-ttu-id="ab3a1-1987">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1987">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ab3a1-1988">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1988">Do</span></span>  | <span data-ttu-id="ab3a1-1989">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1989">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-1990">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1990">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ab3a1-1991">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1991">Ob</span></span>  | 


### <a name="exponentiation-operator"></a><span data-ttu-id="ab3a1-1992">Operador de exponenciação</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1992">Exponentiation Operator</span></span>

<span data-ttu-id="ab3a1-1993">O operador de exponenciação calcula o primeiro operando elevado à potência do segundo operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1993">The exponentiation operator computes the first operand raised to the power of the second operand.</span></span>

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

<span data-ttu-id="ab3a1-1994">O operador de exponenciação é definido para o tipo `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1994">The exponentiation operator is defined for type `Double`.</span></span> <span data-ttu-id="ab3a1-1995">O valor é calculado de acordo com as regras de aritmética do IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1995">The value is computed according to the rules of IEEE 754 arithmetic.</span></span>

<span data-ttu-id="ab3a1-1996">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1996">__Operation Type:__</span></span>

|        | <span data-ttu-id="ab3a1-1997">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1997">__Bo__</span></span> | <span data-ttu-id="ab3a1-1998">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1998">__SB__</span></span> | <span data-ttu-id="ab3a1-1999">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-1999">__By__</span></span> | <span data-ttu-id="ab3a1-2000">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2000">__Sh__</span></span> | <span data-ttu-id="ab3a1-2001">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2001">__US__</span></span> | <span data-ttu-id="ab3a1-2002">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2002">__In__</span></span> | <span data-ttu-id="ab3a1-2003">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2003">__UI__</span></span> | <span data-ttu-id="ab3a1-2004">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2004">__Lo__</span></span> | <span data-ttu-id="ab3a1-2005">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2005">__UL__</span></span> | <span data-ttu-id="ab3a1-2006">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2006">__De__</span></span> | <span data-ttu-id="ab3a1-2007">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2007">__Si__</span></span> | <span data-ttu-id="ab3a1-2008">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2008">__Do__</span></span> | <span data-ttu-id="ab3a1-2009">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2009">__Da__</span></span>  | <span data-ttu-id="ab3a1-2010">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2010">__Ch__</span></span>  | <span data-ttu-id="ab3a1-2011">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2011">__St__</span></span> | <span data-ttu-id="ab3a1-2012">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2012">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ab3a1-2013">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2013">__Bo__</span></span> | <span data-ttu-id="ab3a1-2014">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2014">Do</span></span> | <span data-ttu-id="ab3a1-2015">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2015">Do</span></span> | <span data-ttu-id="ab3a1-2016">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2016">Do</span></span> | <span data-ttu-id="ab3a1-2017">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2017">Do</span></span> | <span data-ttu-id="ab3a1-2018">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2018">Do</span></span> | <span data-ttu-id="ab3a1-2019">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2019">Do</span></span> | <span data-ttu-id="ab3a1-2020">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2020">Do</span></span> | <span data-ttu-id="ab3a1-2021">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2021">Do</span></span> | <span data-ttu-id="ab3a1-2022">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2022">Do</span></span> | <span data-ttu-id="ab3a1-2023">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2023">Do</span></span> | <span data-ttu-id="ab3a1-2024">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2024">Do</span></span> | <span data-ttu-id="ab3a1-2025">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2025">Do</span></span> | <span data-ttu-id="ab3a1-2026">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2026">Err</span></span> | <span data-ttu-id="ab3a1-2027">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2027">Err</span></span> | <span data-ttu-id="ab3a1-2028">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2028">Do</span></span>  | <span data-ttu-id="ab3a1-2029">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2029">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2030">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2030">__SB__</span></span> |    | <span data-ttu-id="ab3a1-2031">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2031">Do</span></span> | <span data-ttu-id="ab3a1-2032">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2032">Do</span></span> | <span data-ttu-id="ab3a1-2033">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2033">Do</span></span> | <span data-ttu-id="ab3a1-2034">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2034">Do</span></span> | <span data-ttu-id="ab3a1-2035">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2035">Do</span></span> | <span data-ttu-id="ab3a1-2036">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2036">Do</span></span> | <span data-ttu-id="ab3a1-2037">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2037">Do</span></span> | <span data-ttu-id="ab3a1-2038">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2038">Do</span></span> | <span data-ttu-id="ab3a1-2039">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2039">Do</span></span> | <span data-ttu-id="ab3a1-2040">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2040">Do</span></span> | <span data-ttu-id="ab3a1-2041">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2041">Do</span></span> | <span data-ttu-id="ab3a1-2042">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2042">Err</span></span> | <span data-ttu-id="ab3a1-2043">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2043">Err</span></span> | <span data-ttu-id="ab3a1-2044">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2044">Do</span></span>  | <span data-ttu-id="ab3a1-2045">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2045">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2046">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2046">__By__</span></span> |    |    | <span data-ttu-id="ab3a1-2047">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2047">Do</span></span> | <span data-ttu-id="ab3a1-2048">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2048">Do</span></span> | <span data-ttu-id="ab3a1-2049">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2049">Do</span></span> | <span data-ttu-id="ab3a1-2050">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2050">Do</span></span> | <span data-ttu-id="ab3a1-2051">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2051">Do</span></span> | <span data-ttu-id="ab3a1-2052">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2052">Do</span></span> | <span data-ttu-id="ab3a1-2053">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2053">Do</span></span> | <span data-ttu-id="ab3a1-2054">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2054">Do</span></span> | <span data-ttu-id="ab3a1-2055">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2055">Do</span></span> | <span data-ttu-id="ab3a1-2056">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2056">Do</span></span> | <span data-ttu-id="ab3a1-2057">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2057">Err</span></span> | <span data-ttu-id="ab3a1-2058">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2058">Err</span></span> | <span data-ttu-id="ab3a1-2059">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2059">Do</span></span>  | <span data-ttu-id="ab3a1-2060">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2060">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2061">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2061">__Sh__</span></span> |    |    |    | <span data-ttu-id="ab3a1-2062">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2062">Do</span></span> | <span data-ttu-id="ab3a1-2063">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2063">Do</span></span> | <span data-ttu-id="ab3a1-2064">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2064">Do</span></span> | <span data-ttu-id="ab3a1-2065">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2065">Do</span></span> | <span data-ttu-id="ab3a1-2066">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2066">Do</span></span> | <span data-ttu-id="ab3a1-2067">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2067">Do</span></span> | <span data-ttu-id="ab3a1-2068">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2068">Do</span></span> | <span data-ttu-id="ab3a1-2069">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2069">Do</span></span> | <span data-ttu-id="ab3a1-2070">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2070">Do</span></span> | <span data-ttu-id="ab3a1-2071">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2071">Err</span></span> | <span data-ttu-id="ab3a1-2072">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2072">Err</span></span> | <span data-ttu-id="ab3a1-2073">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2073">Do</span></span>  | <span data-ttu-id="ab3a1-2074">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2074">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2075">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2075">__US__</span></span> |    |    |    |    | <span data-ttu-id="ab3a1-2076">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2076">Do</span></span> | <span data-ttu-id="ab3a1-2077">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2077">Do</span></span> | <span data-ttu-id="ab3a1-2078">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2078">Do</span></span> | <span data-ttu-id="ab3a1-2079">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2079">Do</span></span> | <span data-ttu-id="ab3a1-2080">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2080">Do</span></span> | <span data-ttu-id="ab3a1-2081">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2081">Do</span></span> | <span data-ttu-id="ab3a1-2082">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2082">Do</span></span> | <span data-ttu-id="ab3a1-2083">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2083">Do</span></span> | <span data-ttu-id="ab3a1-2084">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2084">Err</span></span> | <span data-ttu-id="ab3a1-2085">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2085">Err</span></span> | <span data-ttu-id="ab3a1-2086">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2086">Do</span></span>  | <span data-ttu-id="ab3a1-2087">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2087">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2088">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2088">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ab3a1-2089">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2089">Do</span></span> | <span data-ttu-id="ab3a1-2090">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2090">Do</span></span> | <span data-ttu-id="ab3a1-2091">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2091">Do</span></span> | <span data-ttu-id="ab3a1-2092">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2092">Do</span></span> | <span data-ttu-id="ab3a1-2093">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2093">Do</span></span> | <span data-ttu-id="ab3a1-2094">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2094">Do</span></span> | <span data-ttu-id="ab3a1-2095">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2095">Do</span></span> | <span data-ttu-id="ab3a1-2096">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2096">Err</span></span> | <span data-ttu-id="ab3a1-2097">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2097">Err</span></span> | <span data-ttu-id="ab3a1-2098">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2098">Do</span></span>  | <span data-ttu-id="ab3a1-2099">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2099">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2100">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2100">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2101">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2101">Do</span></span> | <span data-ttu-id="ab3a1-2102">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2102">Do</span></span> | <span data-ttu-id="ab3a1-2103">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2103">Do</span></span> | <span data-ttu-id="ab3a1-2104">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2104">Do</span></span> | <span data-ttu-id="ab3a1-2105">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2105">Do</span></span> | <span data-ttu-id="ab3a1-2106">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2106">Do</span></span> | <span data-ttu-id="ab3a1-2107">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2107">Err</span></span> | <span data-ttu-id="ab3a1-2108">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2108">Err</span></span> | <span data-ttu-id="ab3a1-2109">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2109">Do</span></span>  | <span data-ttu-id="ab3a1-2110">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2110">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2111">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2111">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2112">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2112">Do</span></span> | <span data-ttu-id="ab3a1-2113">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2113">Do</span></span> | <span data-ttu-id="ab3a1-2114">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2114">Do</span></span> | <span data-ttu-id="ab3a1-2115">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2115">Do</span></span> | <span data-ttu-id="ab3a1-2116">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2116">Do</span></span> | <span data-ttu-id="ab3a1-2117">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2117">Err</span></span> | <span data-ttu-id="ab3a1-2118">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2118">Err</span></span> | <span data-ttu-id="ab3a1-2119">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2119">Do</span></span>  | <span data-ttu-id="ab3a1-2120">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2120">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2121">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2121">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2122">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2122">Do</span></span> | <span data-ttu-id="ab3a1-2123">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2123">Do</span></span> | <span data-ttu-id="ab3a1-2124">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2124">Do</span></span> | <span data-ttu-id="ab3a1-2125">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2125">Do</span></span> | <span data-ttu-id="ab3a1-2126">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2126">Err</span></span> | <span data-ttu-id="ab3a1-2127">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2127">Err</span></span> | <span data-ttu-id="ab3a1-2128">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2128">Do</span></span>  | <span data-ttu-id="ab3a1-2129">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2129">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2130">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2130">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2131">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2131">Do</span></span> | <span data-ttu-id="ab3a1-2132">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2132">Do</span></span> | <span data-ttu-id="ab3a1-2133">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2133">Do</span></span> | <span data-ttu-id="ab3a1-2134">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2134">Err</span></span> | <span data-ttu-id="ab3a1-2135">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2135">Err</span></span> | <span data-ttu-id="ab3a1-2136">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2136">Do</span></span>  | <span data-ttu-id="ab3a1-2137">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2137">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2138">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2138">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2139">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2139">Do</span></span> | <span data-ttu-id="ab3a1-2140">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2140">Do</span></span> | <span data-ttu-id="ab3a1-2141">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2141">Err</span></span> | <span data-ttu-id="ab3a1-2142">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2142">Err</span></span> | <span data-ttu-id="ab3a1-2143">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2143">Do</span></span>  | <span data-ttu-id="ab3a1-2144">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2144">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2145">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2145">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2146">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2146">Do</span></span> | <span data-ttu-id="ab3a1-2147">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2147">Err</span></span> | <span data-ttu-id="ab3a1-2148">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2148">Err</span></span> | <span data-ttu-id="ab3a1-2149">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2149">Do</span></span>  | <span data-ttu-id="ab3a1-2150">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2150">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2151">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2151">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2152">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2152">Err</span></span> | <span data-ttu-id="ab3a1-2153">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2153">Err</span></span> | <span data-ttu-id="ab3a1-2154">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2154">Err</span></span> | <span data-ttu-id="ab3a1-2155">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2155">Err</span></span> | 
| <span data-ttu-id="ab3a1-2156">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2156">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ab3a1-2157">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2157">Err</span></span> | <span data-ttu-id="ab3a1-2158">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2158">Err</span></span> | <span data-ttu-id="ab3a1-2159">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2159">Err</span></span> | 
| <span data-ttu-id="ab3a1-2160">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2160">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ab3a1-2161">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2161">Do</span></span>  | <span data-ttu-id="ab3a1-2162">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2162">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2163">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2163">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ab3a1-2164">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2164">Ob</span></span>  | 


## <a name="relational-operators"></a><span data-ttu-id="ab3a1-2165">Operadores relacionais</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2165">Relational Operators</span></span>

<span data-ttu-id="ab3a1-2166">O *operadores relacionais* comparar valores a qualquer outra.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2166">The *relational operators* compare values to one other.</span></span> <span data-ttu-id="ab3a1-2167">Os operadores de comparação são `=`, `<>`, `<`, `>`, `<=`, e `>=`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2167">The comparison operators are `=`, `<>`, `<`, `>`, `<=`, and `>=`.</span></span>

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

<span data-ttu-id="ab3a1-2168">Todos os operadores relacionais resultam em um `Boolean` valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2168">All of the relational operators result in a `Boolean` value.</span></span>

<span data-ttu-id="ab3a1-2169">Os operadores relacionais têm o seguinte significado geral:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2169">The relational operators have the following general meaning:</span></span>

* <span data-ttu-id="ab3a1-2170">O `=` operador testa se os dois operandos forem iguais.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2170">The `=` operator tests whether the two operands are equal.</span></span>

* <span data-ttu-id="ab3a1-2171">O `<>` operador testa se os dois operandos não são iguais.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2171">The `<>` operator tests whether the two operands are not equal.</span></span>

* <span data-ttu-id="ab3a1-2172">O `<` operador testa se o primeiro operando for menor que o segundo operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2172">The `<` operator tests whether the first operand is less than the second operand.</span></span>

* <span data-ttu-id="ab3a1-2173">O `>` operador testa se o primeiro operando for maior que o segundo operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2173">The `>` operator tests whether the first operand is greater than the second operand.</span></span>

* <span data-ttu-id="ab3a1-2174">O `<=` operador testa se o primeiro operando for menor ou igual ao segundo operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2174">The `<=` operator tests whether the first operand is less than or equal to the second operand.</span></span>

* <span data-ttu-id="ab3a1-2175">O `>=` operador testa se o primeiro operando for maior que ou igual ao segundo operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2175">The `>=` operator tests whether the first operand is greater than or equal to the second operand.</span></span>

<span data-ttu-id="ab3a1-2176">Os operadores relacionais são definidos para os seguintes tipos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2176">The relational operators are defined for the following types:</span></span>

* <span data-ttu-id="ab3a1-2177">`Boolean`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2177">`Boolean`.</span></span> <span data-ttu-id="ab3a1-2178">Os operadores comparam os valores dos dois operandos de verdade.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2178">The operators compare the truth values of the two operands.</span></span> <span data-ttu-id="ab3a1-2179">`True` é considerado menor que `False`, que corresponde a com seus valores numéricos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2179">`True` is considered to be less than `False`, which matches with their numeric values.</span></span>

* <span data-ttu-id="ab3a1-2180">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, e `Long`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2180">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="ab3a1-2181">Os operadores comparam os valores numéricos dos dois operandos integrais.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2181">The operators compare the numeric values of the two integral operands.</span></span>

* <span data-ttu-id="ab3a1-2182">`Single` e `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2182">`Single` and `Double`.</span></span> <span data-ttu-id="ab3a1-2183">Os operadores comparam os operandos de acordo com as regras do padrão IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2183">The operators compare the operands according to the rules of the IEEE 754 standard.</span></span>

* <span data-ttu-id="ab3a1-2184">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2184">`Decimal`.</span></span> <span data-ttu-id="ab3a1-2185">Os operadores comparam os valores numéricos dos dois operandos decimais.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2185">The operators compare the numeric values of the two decimal operands.</span></span>

* <span data-ttu-id="ab3a1-2186">`Date`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2186">`Date`.</span></span> <span data-ttu-id="ab3a1-2187">Os operadores retornam o resultado de comparar os dois valores de data/hora.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2187">The operators return the result of comparing the two date/time values.</span></span>

* <span data-ttu-id="ab3a1-2188">`Char`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2188">`Char`.</span></span> <span data-ttu-id="ab3a1-2189">Os operadores retornam o resultado da comparação dos dois valores de Unicode.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2189">The operators return the result of comparing the two Unicode values.</span></span>

* <span data-ttu-id="ab3a1-2190">`String`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2190">`String`.</span></span> <span data-ttu-id="ab3a1-2191">Os operadores retornam o resultado de comparar os dois valores usando uma comparação binária ou uma comparação de texto.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2191">The operators return the result of comparing the two values using either a binary comparison or a text comparison.</span></span> <span data-ttu-id="ab3a1-2192">A comparação usada é determinada pelo ambiente de compilação e o `Option Compare` instrução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2192">The comparison used is determined by the compilation environment and the `Option Compare` statement.</span></span> <span data-ttu-id="ab3a1-2193">Uma comparação binária determina se o valor de Unicode numérico de cada caractere em cada cadeia de caracteres é o mesmo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2193">A binary comparison determines whether the numeric Unicode value of each character in each string is the same.</span></span> <span data-ttu-id="ab3a1-2194">Uma comparação de texto faz uma comparação de texto Unicode com base na cultura atual em uso no .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2194">A text comparison does a Unicode text comparison based on the current culture in use on the .NET Framework.</span></span> <span data-ttu-id="ab3a1-2195">Ao fazer uma comparação de cadeia de caracteres, um valor nulo é equivalente à cadeia de caracteres literal `""`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2195">When doing a string comparison, a null value is equivalent to the string literal `""`.</span></span>

<span data-ttu-id="ab3a1-2196">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2196">__Operation Type:__</span></span>
        
|        | <span data-ttu-id="ab3a1-2197">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2197">__Bo__</span></span> | <span data-ttu-id="ab3a1-2198">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2198">__SB__</span></span> | <span data-ttu-id="ab3a1-2199">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2199">__By__</span></span> | <span data-ttu-id="ab3a1-2200">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2200">__Sh__</span></span> | <span data-ttu-id="ab3a1-2201">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2201">__US__</span></span> | <span data-ttu-id="ab3a1-2202">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2202">__In__</span></span> | <span data-ttu-id="ab3a1-2203">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2203">__UI__</span></span> | <span data-ttu-id="ab3a1-2204">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2204">__Lo__</span></span> | <span data-ttu-id="ab3a1-2205">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2205">__UL__</span></span> | <span data-ttu-id="ab3a1-2206">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2206">__De__</span></span> | <span data-ttu-id="ab3a1-2207">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2207">__Si__</span></span> | <span data-ttu-id="ab3a1-2208">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2208">__Do__</span></span> | <span data-ttu-id="ab3a1-2209">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2209">__Da__</span></span>  | <span data-ttu-id="ab3a1-2210">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2210">__Ch__</span></span>  | <span data-ttu-id="ab3a1-2211">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2211">__St__</span></span> | <span data-ttu-id="ab3a1-2212">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2212">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="ab3a1-2213">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2213">__Bo__</span></span> | <span data-ttu-id="ab3a1-2214">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2214">Bo</span></span> | <span data-ttu-id="ab3a1-2215">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2215">SB</span></span> | <span data-ttu-id="ab3a1-2216">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2216">Sh</span></span> | <span data-ttu-id="ab3a1-2217">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2217">Sh</span></span> | <span data-ttu-id="ab3a1-2218">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2218">In</span></span> | <span data-ttu-id="ab3a1-2219">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2219">In</span></span> | <span data-ttu-id="ab3a1-2220">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2220">Lo</span></span> | <span data-ttu-id="ab3a1-2221">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2221">Lo</span></span> | <span data-ttu-id="ab3a1-2222">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2222">De</span></span> | <span data-ttu-id="ab3a1-2223">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2223">De</span></span> | <span data-ttu-id="ab3a1-2224">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2224">Si</span></span> | <span data-ttu-id="ab3a1-2225">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2225">Do</span></span> | <span data-ttu-id="ab3a1-2226">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2226">Err</span></span> | <span data-ttu-id="ab3a1-2227">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2227">Err</span></span> | <span data-ttu-id="ab3a1-2228">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2228">Bo</span></span> | <span data-ttu-id="ab3a1-2229">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2229">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2230">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2230">__SB__</span></span> |    | <span data-ttu-id="ab3a1-2231">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2231">SB</span></span> | <span data-ttu-id="ab3a1-2232">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2232">Sh</span></span> | <span data-ttu-id="ab3a1-2233">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2233">Sh</span></span> | <span data-ttu-id="ab3a1-2234">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2234">In</span></span> | <span data-ttu-id="ab3a1-2235">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2235">In</span></span> | <span data-ttu-id="ab3a1-2236">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2236">Lo</span></span> | <span data-ttu-id="ab3a1-2237">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2237">Lo</span></span> | <span data-ttu-id="ab3a1-2238">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2238">De</span></span> | <span data-ttu-id="ab3a1-2239">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2239">De</span></span> | <span data-ttu-id="ab3a1-2240">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2240">Si</span></span> | <span data-ttu-id="ab3a1-2241">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2241">Do</span></span> | <span data-ttu-id="ab3a1-2242">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2242">Err</span></span> | <span data-ttu-id="ab3a1-2243">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2243">Err</span></span> | <span data-ttu-id="ab3a1-2244">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2244">Do</span></span> | <span data-ttu-id="ab3a1-2245">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2245">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2246">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2246">__By__</span></span> |    |    | <span data-ttu-id="ab3a1-2247">Por</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2247">By</span></span> | <span data-ttu-id="ab3a1-2248">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2248">Sh</span></span> | <span data-ttu-id="ab3a1-2249">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2249">US</span></span> | <span data-ttu-id="ab3a1-2250">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2250">In</span></span> | <span data-ttu-id="ab3a1-2251">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2251">UI</span></span> | <span data-ttu-id="ab3a1-2252">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2252">Lo</span></span> | <span data-ttu-id="ab3a1-2253">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2253">UL</span></span> | <span data-ttu-id="ab3a1-2254">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2254">De</span></span> | <span data-ttu-id="ab3a1-2255">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2255">Si</span></span> | <span data-ttu-id="ab3a1-2256">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2256">Do</span></span> | <span data-ttu-id="ab3a1-2257">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2257">Err</span></span> | <span data-ttu-id="ab3a1-2258">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2258">Err</span></span> | <span data-ttu-id="ab3a1-2259">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2259">Do</span></span> | <span data-ttu-id="ab3a1-2260">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2260">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2261">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2261">__Sh__</span></span> |    |    |    | <span data-ttu-id="ab3a1-2262">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2262">Sh</span></span> | <span data-ttu-id="ab3a1-2263">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2263">In</span></span> | <span data-ttu-id="ab3a1-2264">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2264">In</span></span> | <span data-ttu-id="ab3a1-2265">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2265">Lo</span></span> | <span data-ttu-id="ab3a1-2266">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2266">Lo</span></span> | <span data-ttu-id="ab3a1-2267">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2267">De</span></span> | <span data-ttu-id="ab3a1-2268">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2268">De</span></span> | <span data-ttu-id="ab3a1-2269">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2269">Si</span></span> | <span data-ttu-id="ab3a1-2270">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2270">Do</span></span> | <span data-ttu-id="ab3a1-2271">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2271">Err</span></span> | <span data-ttu-id="ab3a1-2272">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2272">Err</span></span> | <span data-ttu-id="ab3a1-2273">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2273">Do</span></span> | <span data-ttu-id="ab3a1-2274">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2274">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2275">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2275">__US__</span></span> |    |    |    |    | <span data-ttu-id="ab3a1-2276">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2276">US</span></span> | <span data-ttu-id="ab3a1-2277">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2277">In</span></span> | <span data-ttu-id="ab3a1-2278">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2278">UI</span></span> | <span data-ttu-id="ab3a1-2279">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2279">Lo</span></span> | <span data-ttu-id="ab3a1-2280">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2280">UL</span></span> | <span data-ttu-id="ab3a1-2281">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2281">De</span></span> | <span data-ttu-id="ab3a1-2282">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2282">Si</span></span> | <span data-ttu-id="ab3a1-2283">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2283">Do</span></span> | <span data-ttu-id="ab3a1-2284">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2284">Err</span></span> | <span data-ttu-id="ab3a1-2285">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2285">Err</span></span> | <span data-ttu-id="ab3a1-2286">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2286">Do</span></span> | <span data-ttu-id="ab3a1-2287">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2287">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2288">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2288">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ab3a1-2289">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2289">In</span></span> | <span data-ttu-id="ab3a1-2290">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2290">Lo</span></span> | <span data-ttu-id="ab3a1-2291">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2291">Lo</span></span> | <span data-ttu-id="ab3a1-2292">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2292">De</span></span> | <span data-ttu-id="ab3a1-2293">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2293">De</span></span> | <span data-ttu-id="ab3a1-2294">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2294">Si</span></span> | <span data-ttu-id="ab3a1-2295">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2295">Do</span></span> | <span data-ttu-id="ab3a1-2296">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2296">Err</span></span> | <span data-ttu-id="ab3a1-2297">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2297">Err</span></span> | <span data-ttu-id="ab3a1-2298">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2298">Do</span></span> | <span data-ttu-id="ab3a1-2299">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2299">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2300">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2300">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2301">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2301">UI</span></span> | <span data-ttu-id="ab3a1-2302">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2302">Lo</span></span> | <span data-ttu-id="ab3a1-2303">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2303">UL</span></span> | <span data-ttu-id="ab3a1-2304">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2304">De</span></span> | <span data-ttu-id="ab3a1-2305">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2305">Si</span></span> | <span data-ttu-id="ab3a1-2306">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2306">Do</span></span> | <span data-ttu-id="ab3a1-2307">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2307">Err</span></span> | <span data-ttu-id="ab3a1-2308">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2308">Err</span></span> | <span data-ttu-id="ab3a1-2309">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2309">Do</span></span> | <span data-ttu-id="ab3a1-2310">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2310">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2311">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2311">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2312">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2312">Lo</span></span> | <span data-ttu-id="ab3a1-2313">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2313">De</span></span> | <span data-ttu-id="ab3a1-2314">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2314">De</span></span> | <span data-ttu-id="ab3a1-2315">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2315">Si</span></span> | <span data-ttu-id="ab3a1-2316">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2316">Do</span></span> | <span data-ttu-id="ab3a1-2317">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2317">Err</span></span> | <span data-ttu-id="ab3a1-2318">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2318">Err</span></span> | <span data-ttu-id="ab3a1-2319">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2319">Do</span></span> | <span data-ttu-id="ab3a1-2320">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2320">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2321">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2321">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2322">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2322">UL</span></span> | <span data-ttu-id="ab3a1-2323">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2323">De</span></span> | <span data-ttu-id="ab3a1-2324">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2324">Si</span></span> | <span data-ttu-id="ab3a1-2325">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2325">Do</span></span> | <span data-ttu-id="ab3a1-2326">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2326">Err</span></span> | <span data-ttu-id="ab3a1-2327">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2327">Err</span></span> | <span data-ttu-id="ab3a1-2328">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2328">Do</span></span> | <span data-ttu-id="ab3a1-2329">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2329">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2330">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2330">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2331">Alemanha</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2331">De</span></span> | <span data-ttu-id="ab3a1-2332">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2332">Si</span></span> | <span data-ttu-id="ab3a1-2333">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2333">Do</span></span> | <span data-ttu-id="ab3a1-2334">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2334">Err</span></span> | <span data-ttu-id="ab3a1-2335">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2335">Err</span></span> | <span data-ttu-id="ab3a1-2336">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2336">Do</span></span> | <span data-ttu-id="ab3a1-2337">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2337">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2338">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2338">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2339">si</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2339">Si</span></span> | <span data-ttu-id="ab3a1-2340">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2340">Do</span></span> | <span data-ttu-id="ab3a1-2341">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2341">Err</span></span> | <span data-ttu-id="ab3a1-2342">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2342">Err</span></span> | <span data-ttu-id="ab3a1-2343">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2343">Do</span></span> | <span data-ttu-id="ab3a1-2344">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2344">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2345">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2345">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2346">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2346">Do</span></span> | <span data-ttu-id="ab3a1-2347">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2347">Err</span></span> | <span data-ttu-id="ab3a1-2348">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2348">Err</span></span> | <span data-ttu-id="ab3a1-2349">fazer</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2349">Do</span></span> | <span data-ttu-id="ab3a1-2350">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2350">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2351">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2351">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2352">Acelerador de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2352">Da</span></span>  | <span data-ttu-id="ab3a1-2353">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2353">Err</span></span> | <span data-ttu-id="ab3a1-2354">Acelerador de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2354">Da</span></span> | <span data-ttu-id="ab3a1-2355">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2355">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2356">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2356">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ab3a1-2357">CH</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2357">Ch</span></span>  | <span data-ttu-id="ab3a1-2358">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2358">St</span></span> | <span data-ttu-id="ab3a1-2359">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2359">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2360">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2360">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ab3a1-2361">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2361">St</span></span> | <span data-ttu-id="ab3a1-2362">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2362">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2363">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2363">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="ab3a1-2364">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2364">Ob</span></span> | 


## <a name="like-operator"></a><span data-ttu-id="ab3a1-2365">Operador Like</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2365">Like Operator</span></span>

<span data-ttu-id="ab3a1-2366">O `Like` operador determina se uma cadeia de caracteres corresponde a um determinado padrão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2366">The `Like` operator determines whether a string matches a given pattern.</span></span>

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

<span data-ttu-id="ab3a1-2367">O `Like` operador está definido para o `String` tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2367">The `Like` operator is defined for the `String` type.</span></span> <span data-ttu-id="ab3a1-2368">O primeiro operando é a cadeia de caracteres que está sendo correspondida e o segundo operando é o padrão ao qual corresponder.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2368">The first operand is the string being matched, and the second operand is the pattern to match against.</span></span> <span data-ttu-id="ab3a1-2369">O padrão é composto por caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2369">The pattern is made up of Unicode characters.</span></span> <span data-ttu-id="ab3a1-2370">As seguintes sequências de caracteres têm significados especiais:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2370">The following character sequences have special meanings:</span></span>

* <span data-ttu-id="ab3a1-2371">O caractere `?` corresponde a qualquer caractere único.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2371">The character `?` matches any single character.</span></span>

* <span data-ttu-id="ab3a1-2372">O caractere `*` corresponde a zero ou mais caracteres.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2372">The character `*` matches zero or more characters.</span></span>

* <span data-ttu-id="ab3a1-2373">O caractere `#` corresponde a qualquer dígito único (0-9).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2373">The character `#` matches any single digit (0-9).</span></span>

* <span data-ttu-id="ab3a1-2374">Uma lista de caracteres entre colchetes (`[ab...]`) corresponde a qualquer caractere único na lista.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2374">A list of characters surrounded by brackets (`[ab...]`) matches any single character in the list.</span></span>

* <span data-ttu-id="ab3a1-2375">Uma lista de caracteres entre colchetes e prefixado por um ponto de exclamação (`[!ab...]`) corresponde a qualquer caractere único não está na lista de caracteres.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2375">A list of characters surrounded by brackets and prefixed by an exclamation point (`[!ab...]`) matches any single character not in the character list.</span></span>

* <span data-ttu-id="ab3a1-2376">Dois caracteres em uma lista de caracteres separados por um hífen (`-`) Especifique um intervalo de Unicode caracteres começando com o primeiro caractere e terminando com o segundo caractere.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2376">Two characters in a character list separated by a hyphen (`-`) specify a range of Unicode characters starting with the first character and ending with the second character.</span></span> <span data-ttu-id="ab3a1-2377">Se o segundo caractere não for mais tarde na ordem de classificação que o primeiro caractere, ocorrerá uma exceção de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2377">If the second character is not later in the sort order than the first character, a run-time exception occurs.</span></span> <span data-ttu-id="ab3a1-2378">Um hífen que aparece no início ou no final de uma lista de caracteres Especifica em si.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2378">A hyphen that appears at the beginning or end of a character list specifies itself.</span></span>

<span data-ttu-id="ab3a1-2379">Para corresponder o colchete esquerdo de caracteres especiais (`[`), ponto de interrogação (`?`), sinal numérico (`#`) e o asterisco (`*`), colchetes necessário colocá-los.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2379">To match the special characters left bracket (`[`), question mark (`?`), number sign (`#`), and asterisk (`*`), brackets must enclose them.</span></span> <span data-ttu-id="ab3a1-2380">O colchete direito (`]`) não pode ser usado dentro de um grupo para corresponder em si, mas pode ser usado fora de um grupo como um caractere individual.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2380">The right bracket (`]`) cannot be used within a group to match itself, but it can be used outside a group as an individual character.</span></span> <span data-ttu-id="ab3a1-2381">A sequência de caracteres `[]` é considerado como a cadeia de caracteres literal `""`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2381">The character sequence `[]` is considered to be the string literal `""`.</span></span> 

<span data-ttu-id="ab3a1-2382">Observe que as comparações de cadeias de caracteres e a ordenação para listas de caractere são dependentes do tipo de comparações que está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2382">Note that character comparisons and ordering for character lists are dependent on the type of comparisons being used.</span></span> <span data-ttu-id="ab3a1-2383">Se estiverem sendo usadas comparações binárias, comparações de cadeias de caracteres e a ordenação baseiam-se nos valores Unicode numéricos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2383">If binary comparisons are being used, character comparisons and ordering are based on the numeric Unicode values.</span></span> <span data-ttu-id="ab3a1-2384">Se as comparações de texto estiverem sendo usadas, comparações de cadeias de caracteres e a ordenação baseiam-se na localidade atual que está sendo usada no .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2384">If text comparisons are being used, character comparisons and ordering are based on the current locale being used on the .NET Framework.</span></span>

<span data-ttu-id="ab3a1-2385">Em alguns idiomas, os caracteres especiais nos alfabeto representam dois caracteres distintos e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2385">In some languages, special characters in the alphabet represent two separate characters and vice versa.</span></span> <span data-ttu-id="ab3a1-2386">Por exemplo, vários idiomas usam o caractere `æ` para representar os caracteres `a` e `e` quando eles forem exibidos juntos, enquanto os caracteres `^` e `O` pode ser usado para representar o caractere `Ô`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2386">For example, several languages use the character `æ` to represent the characters `a` and `e` when they appear together, while the characters `^` and `O` can be used to represent the character `Ô`.</span></span> <span data-ttu-id="ab3a1-2387">Ao usar comparações de cadeias de texto, o `Like` operador reconhece esses equivalências culturais.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2387">When using text comparisons, the `Like` operator recognizes such cultural equivalences.</span></span> <span data-ttu-id="ab3a1-2388">Nesse caso, uma ocorrência de um único caractere especial no padrão ou cadeia de caracteres corresponde a sequência de dois caracteres equivalente em outra cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2388">In that case, an occurrence of the single special character in either pattern or string matches the equivalent two-character sequence in the other string.</span></span> <span data-ttu-id="ab3a1-2389">Da mesma forma, um caractere especial no padrão entre colchetes (por si só, em uma lista ou em um intervalo) corresponde à sequência de dois caracteres equivalente na cadeia de caracteres e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2389">Similarly, a single special character in pattern enclosed in brackets (by itself, in a list, or in a range) matches the equivalent two-character sequence in the string and vice versa.</span></span>

<span data-ttu-id="ab3a1-2390">Em um `Like` expressão em que ambos os operandos forem `Nothing` ou um operando tem uma conversão intrínseco `String` e o outro operando for `Nothing`, `Nothing` é tratado como se fosse a cadeia de caracteres vazia literal `""`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2390">In a `Like` expression where both operands are `Nothing` or one operand has an intrinsic conversion to `String` and the other operand is `Nothing`, `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="ab3a1-2391">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2391">__Operation Type:__</span></span>

|        | <span data-ttu-id="ab3a1-2392">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2392">__Bo__</span></span> | <span data-ttu-id="ab3a1-2393">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2393">__SB__</span></span> | <span data-ttu-id="ab3a1-2394">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2394">__By__</span></span> | <span data-ttu-id="ab3a1-2395">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2395">__Sh__</span></span> | <span data-ttu-id="ab3a1-2396">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2396">__US__</span></span> | <span data-ttu-id="ab3a1-2397">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2397">__In__</span></span> | <span data-ttu-id="ab3a1-2398">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2398">__UI__</span></span> | <span data-ttu-id="ab3a1-2399">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2399">__Lo__</span></span> | <span data-ttu-id="ab3a1-2400">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2400">__UL__</span></span> | <span data-ttu-id="ab3a1-2401">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2401">__De__</span></span> | <span data-ttu-id="ab3a1-2402">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2402">__Si__</span></span> | <span data-ttu-id="ab3a1-2403">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2403">__Do__</span></span> | <span data-ttu-id="ab3a1-2404">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2404">__Da__</span></span>  | <span data-ttu-id="ab3a1-2405">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2405">__Ch__</span></span>  | <span data-ttu-id="ab3a1-2406">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2406">__St__</span></span> | <span data-ttu-id="ab3a1-2407">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2407">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="ab3a1-2408">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2408">__Bo__</span></span> | <span data-ttu-id="ab3a1-2409">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2409">St</span></span> | <span data-ttu-id="ab3a1-2410">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2410">St</span></span> | <span data-ttu-id="ab3a1-2411">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2411">St</span></span> | <span data-ttu-id="ab3a1-2412">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2412">St</span></span> | <span data-ttu-id="ab3a1-2413">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2413">St</span></span> | <span data-ttu-id="ab3a1-2414">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2414">St</span></span> | <span data-ttu-id="ab3a1-2415">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2415">St</span></span> | <span data-ttu-id="ab3a1-2416">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2416">St</span></span> | <span data-ttu-id="ab3a1-2417">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2417">St</span></span> | <span data-ttu-id="ab3a1-2418">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2418">St</span></span> | <span data-ttu-id="ab3a1-2419">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2419">St</span></span> | <span data-ttu-id="ab3a1-2420">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2420">St</span></span> | <span data-ttu-id="ab3a1-2421">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2421">St</span></span> | <span data-ttu-id="ab3a1-2422">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2422">St</span></span> | <span data-ttu-id="ab3a1-2423">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2423">St</span></span> | <span data-ttu-id="ab3a1-2424">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2424">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2425">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2425">__SB__</span></span> |    | <span data-ttu-id="ab3a1-2426">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2426">St</span></span> | <span data-ttu-id="ab3a1-2427">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2427">St</span></span> | <span data-ttu-id="ab3a1-2428">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2428">St</span></span> | <span data-ttu-id="ab3a1-2429">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2429">St</span></span> | <span data-ttu-id="ab3a1-2430">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2430">St</span></span> | <span data-ttu-id="ab3a1-2431">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2431">St</span></span> | <span data-ttu-id="ab3a1-2432">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2432">St</span></span> | <span data-ttu-id="ab3a1-2433">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2433">St</span></span> | <span data-ttu-id="ab3a1-2434">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2434">St</span></span> | <span data-ttu-id="ab3a1-2435">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2435">St</span></span> | <span data-ttu-id="ab3a1-2436">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2436">St</span></span> | <span data-ttu-id="ab3a1-2437">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2437">St</span></span> | <span data-ttu-id="ab3a1-2438">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2438">St</span></span> | <span data-ttu-id="ab3a1-2439">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2439">St</span></span> | <span data-ttu-id="ab3a1-2440">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2440">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2441">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2441">__By__</span></span> |    |    | <span data-ttu-id="ab3a1-2442">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2442">St</span></span> | <span data-ttu-id="ab3a1-2443">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2443">St</span></span> | <span data-ttu-id="ab3a1-2444">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2444">St</span></span> | <span data-ttu-id="ab3a1-2445">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2445">St</span></span> | <span data-ttu-id="ab3a1-2446">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2446">St</span></span> | <span data-ttu-id="ab3a1-2447">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2447">St</span></span> | <span data-ttu-id="ab3a1-2448">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2448">St</span></span> | <span data-ttu-id="ab3a1-2449">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2449">St</span></span> | <span data-ttu-id="ab3a1-2450">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2450">St</span></span> | <span data-ttu-id="ab3a1-2451">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2451">St</span></span> | <span data-ttu-id="ab3a1-2452">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2452">St</span></span> | <span data-ttu-id="ab3a1-2453">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2453">St</span></span> | <span data-ttu-id="ab3a1-2454">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2454">St</span></span> | <span data-ttu-id="ab3a1-2455">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2455">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2456">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2456">__Sh__</span></span> |    |    |    | <span data-ttu-id="ab3a1-2457">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2457">St</span></span> | <span data-ttu-id="ab3a1-2458">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2458">St</span></span> | <span data-ttu-id="ab3a1-2459">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2459">St</span></span> | <span data-ttu-id="ab3a1-2460">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2460">St</span></span> | <span data-ttu-id="ab3a1-2461">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2461">St</span></span> | <span data-ttu-id="ab3a1-2462">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2462">St</span></span> | <span data-ttu-id="ab3a1-2463">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2463">St</span></span> | <span data-ttu-id="ab3a1-2464">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2464">St</span></span> | <span data-ttu-id="ab3a1-2465">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2465">St</span></span> | <span data-ttu-id="ab3a1-2466">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2466">St</span></span> | <span data-ttu-id="ab3a1-2467">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2467">St</span></span> | <span data-ttu-id="ab3a1-2468">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2468">St</span></span> | <span data-ttu-id="ab3a1-2469">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2469">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2470">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2470">__US__</span></span> |    |    |    |    | <span data-ttu-id="ab3a1-2471">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2471">St</span></span> | <span data-ttu-id="ab3a1-2472">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2472">St</span></span> | <span data-ttu-id="ab3a1-2473">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2473">St</span></span> | <span data-ttu-id="ab3a1-2474">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2474">St</span></span> | <span data-ttu-id="ab3a1-2475">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2475">St</span></span> | <span data-ttu-id="ab3a1-2476">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2476">St</span></span> | <span data-ttu-id="ab3a1-2477">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2477">St</span></span> | <span data-ttu-id="ab3a1-2478">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2478">St</span></span> | <span data-ttu-id="ab3a1-2479">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2479">St</span></span> | <span data-ttu-id="ab3a1-2480">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2480">St</span></span> | <span data-ttu-id="ab3a1-2481">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2481">St</span></span> | <span data-ttu-id="ab3a1-2482">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2482">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2483">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2483">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ab3a1-2484">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2484">St</span></span> | <span data-ttu-id="ab3a1-2485">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2485">St</span></span> | <span data-ttu-id="ab3a1-2486">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2486">St</span></span> | <span data-ttu-id="ab3a1-2487">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2487">St</span></span> | <span data-ttu-id="ab3a1-2488">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2488">St</span></span> | <span data-ttu-id="ab3a1-2489">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2489">St</span></span> | <span data-ttu-id="ab3a1-2490">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2490">St</span></span> | <span data-ttu-id="ab3a1-2491">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2491">St</span></span> | <span data-ttu-id="ab3a1-2492">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2492">St</span></span> | <span data-ttu-id="ab3a1-2493">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2493">St</span></span> | <span data-ttu-id="ab3a1-2494">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2494">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2495">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2495">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2496">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2496">St</span></span> | <span data-ttu-id="ab3a1-2497">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2497">St</span></span> | <span data-ttu-id="ab3a1-2498">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2498">St</span></span> | <span data-ttu-id="ab3a1-2499">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2499">St</span></span> | <span data-ttu-id="ab3a1-2500">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2500">St</span></span> | <span data-ttu-id="ab3a1-2501">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2501">St</span></span> | <span data-ttu-id="ab3a1-2502">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2502">St</span></span> | <span data-ttu-id="ab3a1-2503">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2503">St</span></span> | <span data-ttu-id="ab3a1-2504">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2504">St</span></span> | <span data-ttu-id="ab3a1-2505">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2505">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2506">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2506">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2507">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2507">St</span></span> | <span data-ttu-id="ab3a1-2508">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2508">St</span></span> | <span data-ttu-id="ab3a1-2509">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2509">St</span></span> | <span data-ttu-id="ab3a1-2510">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2510">St</span></span> | <span data-ttu-id="ab3a1-2511">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2511">St</span></span> | <span data-ttu-id="ab3a1-2512">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2512">St</span></span> | <span data-ttu-id="ab3a1-2513">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2513">St</span></span> | <span data-ttu-id="ab3a1-2514">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2514">St</span></span> | <span data-ttu-id="ab3a1-2515">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2515">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2516">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2516">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2517">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2517">St</span></span> | <span data-ttu-id="ab3a1-2518">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2518">St</span></span> | <span data-ttu-id="ab3a1-2519">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2519">St</span></span> | <span data-ttu-id="ab3a1-2520">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2520">St</span></span> | <span data-ttu-id="ab3a1-2521">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2521">St</span></span> | <span data-ttu-id="ab3a1-2522">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2522">St</span></span> | <span data-ttu-id="ab3a1-2523">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2523">St</span></span> | <span data-ttu-id="ab3a1-2524">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2524">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2525">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2525">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2526">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2526">St</span></span> | <span data-ttu-id="ab3a1-2527">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2527">St</span></span> | <span data-ttu-id="ab3a1-2528">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2528">St</span></span> | <span data-ttu-id="ab3a1-2529">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2529">St</span></span> | <span data-ttu-id="ab3a1-2530">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2530">St</span></span> | <span data-ttu-id="ab3a1-2531">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2531">St</span></span> | <span data-ttu-id="ab3a1-2532">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2532">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2533">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2533">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2534">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2534">St</span></span> | <span data-ttu-id="ab3a1-2535">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2535">St</span></span> | <span data-ttu-id="ab3a1-2536">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2536">St</span></span> | <span data-ttu-id="ab3a1-2537">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2537">St</span></span> | <span data-ttu-id="ab3a1-2538">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2538">St</span></span> | <span data-ttu-id="ab3a1-2539">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2539">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2540">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2540">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2541">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2541">St</span></span> | <span data-ttu-id="ab3a1-2542">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2542">St</span></span> | <span data-ttu-id="ab3a1-2543">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2543">St</span></span> | <span data-ttu-id="ab3a1-2544">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2544">St</span></span> | <span data-ttu-id="ab3a1-2545">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2545">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2546">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2546">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2547">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2547">St</span></span> | <span data-ttu-id="ab3a1-2548">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2548">St</span></span> | <span data-ttu-id="ab3a1-2549">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2549">St</span></span> | <span data-ttu-id="ab3a1-2550">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2550">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2551">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2551">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2552">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2552">St</span></span> | <span data-ttu-id="ab3a1-2553">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2553">St</span></span> | <span data-ttu-id="ab3a1-2554">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2554">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2555">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2555">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2556">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2556">St</span></span> | <span data-ttu-id="ab3a1-2557">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2557">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2558">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2558">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2559">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2559">Ob</span></span> | 


## <a name="concatenation-operator"></a><span data-ttu-id="ab3a1-2560">Operador de concatenação</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2560">Concatenation Operator</span></span>

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

<span data-ttu-id="ab3a1-2561">O *operador de concatenação* está definido para todos os tipos intrínsecos, incluindo as versões que permitem valor nulas dos tipos de valor intrínseco.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2561">The *concatenation operator* is defined for all of the intrinsic types, including the nullable versions of the intrinsic value types.</span></span> <span data-ttu-id="ab3a1-2562">Ele também é definido para concatenação entre os tipos mencionados acima e `System.DBNull`, que é tratado como um `Nothing` cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2562">It is also defined for concatenation between the types mentioned above and `System.DBNull`, which is treated as a `Nothing` string.</span></span> <span data-ttu-id="ab3a1-2563">O operador de concatenação converte todos os seus operandos para `String`; na expressão, todas as conversões para `String` são considerados para ampliação, independentemente se a semântica estrita é usada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2563">The concatenation operator converts all of its operands to `String`; in the expression, all conversions to `String` are considered to be widening, regardless of whether strict semantics are used.</span></span> <span data-ttu-id="ab3a1-2564">Um `System.DBNull` valor é convertido para o literal `Nothing` digitado como `String`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2564">A `System.DBNull` value is converted to the literal `Nothing` typed as `String`.</span></span> <span data-ttu-id="ab3a1-2565">Um tipo de valor anulável cujo valor é `Nothing` também é convertido para o literal `Nothing` digitado como `String`, em vez de gerar um erro de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2565">A nullable value type whose value is `Nothing` is also converted to the literal `Nothing` typed as `String`, rather than throwing a run-time error.</span></span>

<span data-ttu-id="ab3a1-2566">Uma operação de concatenação resulta em uma cadeia de caracteres que é a concatenação dos dois operandos na ordem da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2566">A concatenation operation results in a string that is the concatenation of the two operands in order from left to right.</span></span> <span data-ttu-id="ab3a1-2567">O valor `Nothing` é tratado como se fosse a cadeia de caracteres vazia literal `""`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2567">The value `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="ab3a1-2568">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2568">__Operation Type:__</span></span>

|        | <span data-ttu-id="ab3a1-2569">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2569">__Bo__</span></span> | <span data-ttu-id="ab3a1-2570">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2570">__SB__</span></span> | <span data-ttu-id="ab3a1-2571">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2571">__By__</span></span> | <span data-ttu-id="ab3a1-2572">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2572">__Sh__</span></span> | <span data-ttu-id="ab3a1-2573">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2573">__US__</span></span> | <span data-ttu-id="ab3a1-2574">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2574">__In__</span></span> | <span data-ttu-id="ab3a1-2575">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2575">__UI__</span></span> | <span data-ttu-id="ab3a1-2576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2576">__Lo__</span></span> | <span data-ttu-id="ab3a1-2577">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2577">__UL__</span></span> | <span data-ttu-id="ab3a1-2578">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2578">__De__</span></span> | <span data-ttu-id="ab3a1-2579">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2579">__Si__</span></span> | <span data-ttu-id="ab3a1-2580">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2580">__Do__</span></span> | <span data-ttu-id="ab3a1-2581">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2581">__Da__</span></span>  | <span data-ttu-id="ab3a1-2582">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2582">__Ch__</span></span>  | <span data-ttu-id="ab3a1-2583">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2583">__St__</span></span> | <span data-ttu-id="ab3a1-2584">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2584">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="ab3a1-2585">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2585">__Bo__</span></span> | <span data-ttu-id="ab3a1-2586">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2586">St</span></span> | <span data-ttu-id="ab3a1-2587">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2587">St</span></span> | <span data-ttu-id="ab3a1-2588">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2588">St</span></span> | <span data-ttu-id="ab3a1-2589">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2589">St</span></span> | <span data-ttu-id="ab3a1-2590">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2590">St</span></span> | <span data-ttu-id="ab3a1-2591">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2591">St</span></span> | <span data-ttu-id="ab3a1-2592">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2592">St</span></span> | <span data-ttu-id="ab3a1-2593">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2593">St</span></span> | <span data-ttu-id="ab3a1-2594">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2594">St</span></span> | <span data-ttu-id="ab3a1-2595">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2595">St</span></span> | <span data-ttu-id="ab3a1-2596">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2596">St</span></span> | <span data-ttu-id="ab3a1-2597">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2597">St</span></span> | <span data-ttu-id="ab3a1-2598">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2598">St</span></span> | <span data-ttu-id="ab3a1-2599">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2599">St</span></span> | <span data-ttu-id="ab3a1-2600">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2600">St</span></span> | <span data-ttu-id="ab3a1-2601">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2601">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2602">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2602">__SB__</span></span> |    | <span data-ttu-id="ab3a1-2603">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2603">St</span></span> | <span data-ttu-id="ab3a1-2604">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2604">St</span></span> | <span data-ttu-id="ab3a1-2605">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2605">St</span></span> | <span data-ttu-id="ab3a1-2606">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2606">St</span></span> | <span data-ttu-id="ab3a1-2607">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2607">St</span></span> | <span data-ttu-id="ab3a1-2608">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2608">St</span></span> | <span data-ttu-id="ab3a1-2609">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2609">St</span></span> | <span data-ttu-id="ab3a1-2610">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2610">St</span></span> | <span data-ttu-id="ab3a1-2611">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2611">St</span></span> | <span data-ttu-id="ab3a1-2612">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2612">St</span></span> | <span data-ttu-id="ab3a1-2613">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2613">St</span></span> | <span data-ttu-id="ab3a1-2614">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2614">St</span></span> | <span data-ttu-id="ab3a1-2615">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2615">St</span></span> | <span data-ttu-id="ab3a1-2616">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2616">St</span></span> | <span data-ttu-id="ab3a1-2617">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2617">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2618">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2618">__By__</span></span> |    |    | <span data-ttu-id="ab3a1-2619">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2619">St</span></span> | <span data-ttu-id="ab3a1-2620">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2620">St</span></span> | <span data-ttu-id="ab3a1-2621">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2621">St</span></span> | <span data-ttu-id="ab3a1-2622">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2622">St</span></span> | <span data-ttu-id="ab3a1-2623">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2623">St</span></span> | <span data-ttu-id="ab3a1-2624">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2624">St</span></span> | <span data-ttu-id="ab3a1-2625">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2625">St</span></span> | <span data-ttu-id="ab3a1-2626">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2626">St</span></span> | <span data-ttu-id="ab3a1-2627">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2627">St</span></span> | <span data-ttu-id="ab3a1-2628">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2628">St</span></span> | <span data-ttu-id="ab3a1-2629">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2629">St</span></span> | <span data-ttu-id="ab3a1-2630">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2630">St</span></span> | <span data-ttu-id="ab3a1-2631">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2631">St</span></span> | <span data-ttu-id="ab3a1-2632">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2632">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2633">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2633">__Sh__</span></span> |    |    |    | <span data-ttu-id="ab3a1-2634">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2634">St</span></span> | <span data-ttu-id="ab3a1-2635">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2635">St</span></span> | <span data-ttu-id="ab3a1-2636">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2636">St</span></span> | <span data-ttu-id="ab3a1-2637">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2637">St</span></span> | <span data-ttu-id="ab3a1-2638">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2638">St</span></span> | <span data-ttu-id="ab3a1-2639">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2639">St</span></span> | <span data-ttu-id="ab3a1-2640">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2640">St</span></span> | <span data-ttu-id="ab3a1-2641">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2641">St</span></span> | <span data-ttu-id="ab3a1-2642">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2642">St</span></span> | <span data-ttu-id="ab3a1-2643">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2643">St</span></span> | <span data-ttu-id="ab3a1-2644">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2644">St</span></span> | <span data-ttu-id="ab3a1-2645">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2645">St</span></span> | <span data-ttu-id="ab3a1-2646">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2646">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2647">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2647">__US__</span></span> |    |    |    |    | <span data-ttu-id="ab3a1-2648">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2648">St</span></span> | <span data-ttu-id="ab3a1-2649">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2649">St</span></span> | <span data-ttu-id="ab3a1-2650">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2650">St</span></span> | <span data-ttu-id="ab3a1-2651">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2651">St</span></span> | <span data-ttu-id="ab3a1-2652">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2652">St</span></span> | <span data-ttu-id="ab3a1-2653">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2653">St</span></span> | <span data-ttu-id="ab3a1-2654">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2654">St</span></span> | <span data-ttu-id="ab3a1-2655">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2655">St</span></span> | <span data-ttu-id="ab3a1-2656">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2656">St</span></span> | <span data-ttu-id="ab3a1-2657">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2657">St</span></span> | <span data-ttu-id="ab3a1-2658">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2658">St</span></span> | <span data-ttu-id="ab3a1-2659">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2659">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2660">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2660">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ab3a1-2661">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2661">St</span></span> | <span data-ttu-id="ab3a1-2662">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2662">St</span></span> | <span data-ttu-id="ab3a1-2663">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2663">St</span></span> | <span data-ttu-id="ab3a1-2664">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2664">St</span></span> | <span data-ttu-id="ab3a1-2665">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2665">St</span></span> | <span data-ttu-id="ab3a1-2666">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2666">St</span></span> | <span data-ttu-id="ab3a1-2667">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2667">St</span></span> | <span data-ttu-id="ab3a1-2668">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2668">St</span></span> | <span data-ttu-id="ab3a1-2669">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2669">St</span></span> | <span data-ttu-id="ab3a1-2670">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2670">St</span></span> | <span data-ttu-id="ab3a1-2671">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2671">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2672">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2672">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2673">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2673">St</span></span> | <span data-ttu-id="ab3a1-2674">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2674">St</span></span> | <span data-ttu-id="ab3a1-2675">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2675">St</span></span> | <span data-ttu-id="ab3a1-2676">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2676">St</span></span> | <span data-ttu-id="ab3a1-2677">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2677">St</span></span> | <span data-ttu-id="ab3a1-2678">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2678">St</span></span> | <span data-ttu-id="ab3a1-2679">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2679">St</span></span> | <span data-ttu-id="ab3a1-2680">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2680">St</span></span> | <span data-ttu-id="ab3a1-2681">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2681">St</span></span> | <span data-ttu-id="ab3a1-2682">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2682">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2683">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2683">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2684">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2684">St</span></span> | <span data-ttu-id="ab3a1-2685">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2685">St</span></span> | <span data-ttu-id="ab3a1-2686">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2686">St</span></span> | <span data-ttu-id="ab3a1-2687">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2687">St</span></span> | <span data-ttu-id="ab3a1-2688">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2688">St</span></span> | <span data-ttu-id="ab3a1-2689">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2689">St</span></span> | <span data-ttu-id="ab3a1-2690">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2690">St</span></span> | <span data-ttu-id="ab3a1-2691">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2691">St</span></span> | <span data-ttu-id="ab3a1-2692">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2692">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2693">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2693">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2694">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2694">St</span></span> | <span data-ttu-id="ab3a1-2695">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2695">St</span></span> | <span data-ttu-id="ab3a1-2696">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2696">St</span></span> | <span data-ttu-id="ab3a1-2697">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2697">St</span></span> | <span data-ttu-id="ab3a1-2698">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2698">St</span></span> | <span data-ttu-id="ab3a1-2699">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2699">St</span></span> | <span data-ttu-id="ab3a1-2700">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2700">St</span></span> | <span data-ttu-id="ab3a1-2701">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2701">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2702">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2702">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2703">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2703">St</span></span> | <span data-ttu-id="ab3a1-2704">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2704">St</span></span> | <span data-ttu-id="ab3a1-2705">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2705">St</span></span> | <span data-ttu-id="ab3a1-2706">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2706">St</span></span> | <span data-ttu-id="ab3a1-2707">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2707">St</span></span> | <span data-ttu-id="ab3a1-2708">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2708">St</span></span> | <span data-ttu-id="ab3a1-2709">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2709">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2710">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2710">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2711">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2711">St</span></span> | <span data-ttu-id="ab3a1-2712">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2712">St</span></span> | <span data-ttu-id="ab3a1-2713">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2713">St</span></span> | <span data-ttu-id="ab3a1-2714">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2714">St</span></span> | <span data-ttu-id="ab3a1-2715">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2715">St</span></span> | <span data-ttu-id="ab3a1-2716">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2716">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2717">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2717">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2718">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2718">St</span></span> | <span data-ttu-id="ab3a1-2719">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2719">St</span></span> | <span data-ttu-id="ab3a1-2720">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2720">St</span></span> | <span data-ttu-id="ab3a1-2721">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2721">St</span></span> | <span data-ttu-id="ab3a1-2722">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2722">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2723">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2723">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2724">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2724">St</span></span> | <span data-ttu-id="ab3a1-2725">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2725">St</span></span> | <span data-ttu-id="ab3a1-2726">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2726">St</span></span> | <span data-ttu-id="ab3a1-2727">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2727">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2728">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2728">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2729">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2729">St</span></span> | <span data-ttu-id="ab3a1-2730">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2730">St</span></span> | <span data-ttu-id="ab3a1-2731">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2731">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2732">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2732">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2733">ST</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2733">St</span></span> | <span data-ttu-id="ab3a1-2734">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2734">Ob</span></span> | 
| <span data-ttu-id="ab3a1-2735">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2735">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2736">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2736">Ob</span></span> | 


## <a name="logical-operators"></a><span data-ttu-id="ab3a1-2737">Operadores Lógicos</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2737">Logical Operators</span></span>

<span data-ttu-id="ab3a1-2738">O `And`, `Not`, `Or`, e `Xor` operadores são chamados de operadores lógicos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2738">The `And`, `Not`, `Or`, and `Xor` operators are called the logical operators.</span></span>

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

<span data-ttu-id="ab3a1-2739">Os operadores lógicos são avaliados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2739">The logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="ab3a1-2740">Para o `Boolean` tipo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2740">For the `Boolean` type:</span></span>

  * <span data-ttu-id="ab3a1-2741">Uma lógica `And` operação é executada em dois operandos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2741">A logical `And` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="ab3a1-2742">Uma lógica `Not` operação é executada em seu operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2742">A logical `Not` operation is performed on its operand.</span></span>

  * <span data-ttu-id="ab3a1-2743">Uma lógica `Or` operação é executada em dois operandos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2743">A logical `Or` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="ab3a1-2744">Uma lógica exclusiva -`Or` operação é executada em dois operandos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2744">A logical exclusive-`Or` operation is performed on its two operands.</span></span>

* <span data-ttu-id="ab3a1-2745">Para `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`e todos os tipos enumerados, a operação especificada é executada em cada bit da representação binária das dois de operador (es):</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2745">For `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, and all enumerated types, the specified operation is performed on each bit of the binary representation of the two operand(s):</span></span>

  * <span data-ttu-id="ab3a1-2746">`And`: O bit de resultado será 1 se os dois bits forem 1; Caso contrário, o bit de resultado é 0.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2746">`And`: The result bit is 1 if both bits are 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="ab3a1-2747">`Not`: O bit de resultado será 1 se o bit for 0; Caso contrário, o bit de resultado será 1.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2747">`Not`: The result bit is 1 if the bit is 0; otherwise the result bit is 1.</span></span>

  * <span data-ttu-id="ab3a1-2748">`Or`: O bit de resultado será 1 se qualquer bit for 1; Caso contrário, o bit de resultado é 0.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2748">`Or`: The result bit is 1 if either bit is 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="ab3a1-2749">`Xor`: O bit de resultado será 1 se qualquer bit for 1, mas não ambos os bits; Caso contrário, o bit de resultado é 0 (ou seja, 1 `Xor` 1 = 0, 1 `Xor` 1 = 0).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2749">`Xor`: The result bit is 1 if either bit is 1 but not both bits; otherwise the result bit is 0 (that is, 1 `Xor` 0 = 1, 1 `Xor` 1 = 0).</span></span>

* <span data-ttu-id="ab3a1-2750">Quando os operadores lógicos `And` e `Or` são elevadas para o tipo `Boolean?`, eles são estendidos para abranger o booliana lógica de três valores assim:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2750">When the logical operators `And` and `Or` are lifted for the type `Boolean?`, they are extended to encompass three-valued Boolean logic as such:</span></span>

  * <span data-ttu-id="ab3a1-2751">`And` é avaliada como true se ambos os operandos forem true; False se um dos operandos for false; `Nothing` caso contrário.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2751">`And` evaluates to true if both operands are true; false if one of the operands is false; `Nothing` otherwise.</span></span>

  * <span data-ttu-id="ab3a1-2752">`Or` é avaliada como true se qualquer operando for true; False é que ambos os operandos forem falsos; `Nothing` caso contrário.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2752">`Or` evaluates to true if either operand is true; false is both operands are false; `Nothing` otherwise.</span></span>

<span data-ttu-id="ab3a1-2753">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2753">For example:</span></span>

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

<span data-ttu-id="ab3a1-2754">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2754">__Note.__</span></span> <span data-ttu-id="ab3a1-2755">O ideal é que os operadores lógicos `And` e `Or` seria eliminada usando lógica de três valores para qualquer tipo que pode ser usado em uma expressão booleana (ou seja, um tipo que implementa `IsTrue` e `IsFalse`), da mesma forma que `AndAlso` e `OrElse` curto-circuito em qualquer tipo que pode ser usado em uma expressão booleana.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2755">Ideally, the logical operators `And` and `Or` would be lifted using three-valued logic for any type that can be used in a Boolean expression (i.e. a type that implements `IsTrue` and `IsFalse`), in the same way that `AndAlso` and `OrElse` short circuit across any type that can be used in a Boolean expression.</span></span> <span data-ttu-id="ab3a1-2756">Infelizmente, três valores levantando só será aplicada aos `Boolean?`, portanto, tipos definidos pelo usuário que desejam a lógica de três valores devem fazer isso manualmente, definindo `And` e `Or` operadores para a versão que permite valor nula.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2756">Unfortunately, three-valued lifting is only applied to `Boolean?`, so user-defined types that desire three-valued logic must do so manually by defining `And` and `Or` operators for their nullable version.</span></span>

<span data-ttu-id="ab3a1-2757">Não há estouros são possíveis dessas operações.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2757">No overflows are possible from these operations.</span></span> <span data-ttu-id="ab3a1-2758">Os operadores de tipo enumerado fazer a operação bit a bit no tipo subjacente do tipo enumerado, mas o valor de retorno é o tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2758">The enumerated type operators do the bitwise operation on the underlying type of the enumerated type, but the return value is the enumerated type.</span></span>

<span data-ttu-id="ab3a1-2759">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2759">__Not Operation Type:__</span></span>

| <span data-ttu-id="ab3a1-2760">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2760">__Bo__</span></span> | <span data-ttu-id="ab3a1-2761">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2761">__SB__</span></span> | <span data-ttu-id="ab3a1-2762">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2762">__By__</span></span> | <span data-ttu-id="ab3a1-2763">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2763">__Sh__</span></span> | <span data-ttu-id="ab3a1-2764">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2764">__US__</span></span> | <span data-ttu-id="ab3a1-2765">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2765">__In__</span></span> | <span data-ttu-id="ab3a1-2766">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2766">__UI__</span></span> | <span data-ttu-id="ab3a1-2767">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2767">__Lo__</span></span> | <span data-ttu-id="ab3a1-2768">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2768">__UL__</span></span> | <span data-ttu-id="ab3a1-2769">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2769">__De__</span></span> | <span data-ttu-id="ab3a1-2770">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2770">__Si__</span></span> | <span data-ttu-id="ab3a1-2771">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2771">__Do__</span></span> | <span data-ttu-id="ab3a1-2772">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2772">__Da__</span></span>  | <span data-ttu-id="ab3a1-2773">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2773">__Ch__</span></span>  | <span data-ttu-id="ab3a1-2774">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2774">__St__</span></span> | <span data-ttu-id="ab3a1-2775">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2775">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="ab3a1-2776">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2776">Bo</span></span> | <span data-ttu-id="ab3a1-2777">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2777">SB</span></span> | <span data-ttu-id="ab3a1-2778">Por</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2778">By</span></span> | <span data-ttu-id="ab3a1-2779">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2779">Sh</span></span> | <span data-ttu-id="ab3a1-2780">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2780">US</span></span> | <span data-ttu-id="ab3a1-2781">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2781">In</span></span> | <span data-ttu-id="ab3a1-2782">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2782">UI</span></span> | <span data-ttu-id="ab3a1-2783">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2783">Lo</span></span> | <span data-ttu-id="ab3a1-2784">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2784">UL</span></span> | <span data-ttu-id="ab3a1-2785">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2785">Lo</span></span> | <span data-ttu-id="ab3a1-2786">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2786">Lo</span></span> | <span data-ttu-id="ab3a1-2787">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2787">Lo</span></span> | <span data-ttu-id="ab3a1-2788">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2788">Err</span></span> | <span data-ttu-id="ab3a1-2789">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2789">Err</span></span> | <span data-ttu-id="ab3a1-2790">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2790">Lo</span></span> | <span data-ttu-id="ab3a1-2791">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2791">Ob</span></span> | 

<span data-ttu-id="ab3a1-2792">__E, ou então, o tipo de operação Xor:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2792">__And, Or, Xor Operation Type:__</span></span>

|        | <span data-ttu-id="ab3a1-2793">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2793">__Bo__</span></span> | <span data-ttu-id="ab3a1-2794">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2794">__SB__</span></span> | <span data-ttu-id="ab3a1-2795">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2795">__By__</span></span> | <span data-ttu-id="ab3a1-2796">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2796">__Sh__</span></span> | <span data-ttu-id="ab3a1-2797">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2797">__US__</span></span> | <span data-ttu-id="ab3a1-2798">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2798">__In__</span></span> | <span data-ttu-id="ab3a1-2799">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2799">__UI__</span></span> | <span data-ttu-id="ab3a1-2800">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2800">__Lo__</span></span> | <span data-ttu-id="ab3a1-2801">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2801">__UL__</span></span> | <span data-ttu-id="ab3a1-2802">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2802">__De__</span></span> | <span data-ttu-id="ab3a1-2803">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2803">__Si__</span></span> | <span data-ttu-id="ab3a1-2804">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2804">__Do__</span></span> | <span data-ttu-id="ab3a1-2805">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2805">__Da__</span></span>  | <span data-ttu-id="ab3a1-2806">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2806">__Ch__</span></span>  | <span data-ttu-id="ab3a1-2807">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2807">__St__</span></span> | <span data-ttu-id="ab3a1-2808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2808">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ab3a1-2809">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2809">__Bo__</span></span> | <span data-ttu-id="ab3a1-2810">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2810">Bo</span></span> | <span data-ttu-id="ab3a1-2811">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2811">SB</span></span> | <span data-ttu-id="ab3a1-2812">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2812">Sh</span></span> | <span data-ttu-id="ab3a1-2813">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2813">Sh</span></span> | <span data-ttu-id="ab3a1-2814">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2814">In</span></span> | <span data-ttu-id="ab3a1-2815">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2815">In</span></span> | <span data-ttu-id="ab3a1-2816">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2816">Lo</span></span> | <span data-ttu-id="ab3a1-2817">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2817">Lo</span></span> | <span data-ttu-id="ab3a1-2818">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2818">Lo</span></span> | <span data-ttu-id="ab3a1-2819">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2819">Lo</span></span> | <span data-ttu-id="ab3a1-2820">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2820">Lo</span></span> | <span data-ttu-id="ab3a1-2821">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2821">Lo</span></span> | <span data-ttu-id="ab3a1-2822">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2822">Err</span></span> | <span data-ttu-id="ab3a1-2823">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2823">Err</span></span> | <span data-ttu-id="ab3a1-2824">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2824">Bo</span></span>  | <span data-ttu-id="ab3a1-2825">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2825">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2826">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2826">__SB__</span></span> |    | <span data-ttu-id="ab3a1-2827">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2827">SB</span></span> | <span data-ttu-id="ab3a1-2828">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2828">Sh</span></span> | <span data-ttu-id="ab3a1-2829">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2829">Sh</span></span> | <span data-ttu-id="ab3a1-2830">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2830">In</span></span> | <span data-ttu-id="ab3a1-2831">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2831">In</span></span> | <span data-ttu-id="ab3a1-2832">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2832">Lo</span></span> | <span data-ttu-id="ab3a1-2833">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2833">Lo</span></span> | <span data-ttu-id="ab3a1-2834">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2834">Lo</span></span> | <span data-ttu-id="ab3a1-2835">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2835">Lo</span></span> | <span data-ttu-id="ab3a1-2836">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2836">Lo</span></span> | <span data-ttu-id="ab3a1-2837">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2837">Lo</span></span> | <span data-ttu-id="ab3a1-2838">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2838">Err</span></span> | <span data-ttu-id="ab3a1-2839">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2839">Err</span></span> | <span data-ttu-id="ab3a1-2840">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2840">Lo</span></span>  | <span data-ttu-id="ab3a1-2841">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2841">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2842">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2842">__By__</span></span> |    |    | <span data-ttu-id="ab3a1-2843">Por</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2843">By</span></span> | <span data-ttu-id="ab3a1-2844">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2844">Sh</span></span> | <span data-ttu-id="ab3a1-2845">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2845">US</span></span> | <span data-ttu-id="ab3a1-2846">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2846">In</span></span> | <span data-ttu-id="ab3a1-2847">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2847">UI</span></span> | <span data-ttu-id="ab3a1-2848">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2848">Lo</span></span> | <span data-ttu-id="ab3a1-2849">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2849">UL</span></span> | <span data-ttu-id="ab3a1-2850">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2850">Lo</span></span> | <span data-ttu-id="ab3a1-2851">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2851">Lo</span></span> | <span data-ttu-id="ab3a1-2852">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2852">Lo</span></span> | <span data-ttu-id="ab3a1-2853">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2853">Err</span></span> | <span data-ttu-id="ab3a1-2854">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2854">Err</span></span> | <span data-ttu-id="ab3a1-2855">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2855">Lo</span></span>  | <span data-ttu-id="ab3a1-2856">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2856">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2857">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2857">__Sh__</span></span> |    |    |    | <span data-ttu-id="ab3a1-2858">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2858">Sh</span></span> | <span data-ttu-id="ab3a1-2859">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2859">In</span></span> | <span data-ttu-id="ab3a1-2860">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2860">In</span></span> | <span data-ttu-id="ab3a1-2861">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2861">Lo</span></span> | <span data-ttu-id="ab3a1-2862">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2862">Lo</span></span> | <span data-ttu-id="ab3a1-2863">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2863">Lo</span></span> | <span data-ttu-id="ab3a1-2864">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2864">Lo</span></span> | <span data-ttu-id="ab3a1-2865">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2865">Lo</span></span> | <span data-ttu-id="ab3a1-2866">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2866">Lo</span></span> | <span data-ttu-id="ab3a1-2867">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2867">Err</span></span> | <span data-ttu-id="ab3a1-2868">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2868">Err</span></span> | <span data-ttu-id="ab3a1-2869">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2869">Lo</span></span>  | <span data-ttu-id="ab3a1-2870">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2870">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2871">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2871">__US__</span></span> |    |    |    |    | <span data-ttu-id="ab3a1-2872">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2872">US</span></span> | <span data-ttu-id="ab3a1-2873">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2873">In</span></span> | <span data-ttu-id="ab3a1-2874">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2874">UI</span></span> | <span data-ttu-id="ab3a1-2875">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2875">Lo</span></span> | <span data-ttu-id="ab3a1-2876">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2876">UL</span></span> | <span data-ttu-id="ab3a1-2877">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2877">Lo</span></span> | <span data-ttu-id="ab3a1-2878">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2878">Lo</span></span> | <span data-ttu-id="ab3a1-2879">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2879">Lo</span></span> | <span data-ttu-id="ab3a1-2880">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2880">Err</span></span> | <span data-ttu-id="ab3a1-2881">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2881">Err</span></span> | <span data-ttu-id="ab3a1-2882">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2882">Lo</span></span>  | <span data-ttu-id="ab3a1-2883">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2883">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2884">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2884">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ab3a1-2885">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2885">In</span></span> | <span data-ttu-id="ab3a1-2886">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2886">Lo</span></span> | <span data-ttu-id="ab3a1-2887">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2887">Lo</span></span> | <span data-ttu-id="ab3a1-2888">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2888">Lo</span></span> | <span data-ttu-id="ab3a1-2889">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2889">Lo</span></span> | <span data-ttu-id="ab3a1-2890">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2890">Lo</span></span> | <span data-ttu-id="ab3a1-2891">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2891">Lo</span></span> | <span data-ttu-id="ab3a1-2892">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2892">Err</span></span> | <span data-ttu-id="ab3a1-2893">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2893">Err</span></span> | <span data-ttu-id="ab3a1-2894">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2894">Lo</span></span>  | <span data-ttu-id="ab3a1-2895">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2895">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2896">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2896">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2897">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2897">UI</span></span> | <span data-ttu-id="ab3a1-2898">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2898">Lo</span></span> | <span data-ttu-id="ab3a1-2899">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2899">UL</span></span> | <span data-ttu-id="ab3a1-2900">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2900">Lo</span></span> | <span data-ttu-id="ab3a1-2901">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2901">Lo</span></span> | <span data-ttu-id="ab3a1-2902">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2902">Lo</span></span> | <span data-ttu-id="ab3a1-2903">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2903">Err</span></span> | <span data-ttu-id="ab3a1-2904">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2904">Err</span></span> | <span data-ttu-id="ab3a1-2905">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2905">Lo</span></span>  | <span data-ttu-id="ab3a1-2906">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2906">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2907">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2907">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2908">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2908">Lo</span></span> | <span data-ttu-id="ab3a1-2909">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2909">Lo</span></span> | <span data-ttu-id="ab3a1-2910">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2910">Lo</span></span> | <span data-ttu-id="ab3a1-2911">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2911">Lo</span></span> | <span data-ttu-id="ab3a1-2912">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2912">Lo</span></span> | <span data-ttu-id="ab3a1-2913">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2913">Err</span></span> | <span data-ttu-id="ab3a1-2914">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2914">Err</span></span> | <span data-ttu-id="ab3a1-2915">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2915">Lo</span></span>  | <span data-ttu-id="ab3a1-2916">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2916">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2917">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2917">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2918">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2918">UL</span></span> | <span data-ttu-id="ab3a1-2919">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2919">Lo</span></span> | <span data-ttu-id="ab3a1-2920">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2920">Lo</span></span> | <span data-ttu-id="ab3a1-2921">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2921">Lo</span></span> | <span data-ttu-id="ab3a1-2922">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2922">Err</span></span> | <span data-ttu-id="ab3a1-2923">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2923">Err</span></span> | <span data-ttu-id="ab3a1-2924">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2924">Lo</span></span>  | <span data-ttu-id="ab3a1-2925">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2925">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2926">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2926">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2927">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2927">Lo</span></span> | <span data-ttu-id="ab3a1-2928">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2928">Lo</span></span> | <span data-ttu-id="ab3a1-2929">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2929">Lo</span></span> | <span data-ttu-id="ab3a1-2930">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2930">Err</span></span> | <span data-ttu-id="ab3a1-2931">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2931">Err</span></span> | <span data-ttu-id="ab3a1-2932">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2932">Lo</span></span>  | <span data-ttu-id="ab3a1-2933">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2933">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2934">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2934">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2935">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2935">Lo</span></span> | <span data-ttu-id="ab3a1-2936">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2936">Lo</span></span> | <span data-ttu-id="ab3a1-2937">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2937">Err</span></span> | <span data-ttu-id="ab3a1-2938">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2938">Err</span></span> | <span data-ttu-id="ab3a1-2939">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2939">Lo</span></span>  | <span data-ttu-id="ab3a1-2940">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2940">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2941">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2941">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2942">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2942">Lo</span></span> | <span data-ttu-id="ab3a1-2943">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2943">Err</span></span> | <span data-ttu-id="ab3a1-2944">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2944">Err</span></span> | <span data-ttu-id="ab3a1-2945">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2945">Lo</span></span>  | <span data-ttu-id="ab3a1-2946">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2946">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2947">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2947">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-2948">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2948">Err</span></span> | <span data-ttu-id="ab3a1-2949">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2949">Err</span></span> | <span data-ttu-id="ab3a1-2950">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2950">Err</span></span> | <span data-ttu-id="ab3a1-2951">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2951">Err</span></span> | 
| <span data-ttu-id="ab3a1-2952">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2952">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ab3a1-2953">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2953">Err</span></span> | <span data-ttu-id="ab3a1-2954">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2954">Err</span></span> | <span data-ttu-id="ab3a1-2955">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2955">Err</span></span> | 
| <span data-ttu-id="ab3a1-2956">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2956">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ab3a1-2957">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2957">Lo</span></span>  | <span data-ttu-id="ab3a1-2958">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2958">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-2959">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2959">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ab3a1-2960">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2960">Ob</span></span>  | 


### <a name="short-circuiting-logical-operators"></a><span data-ttu-id="ab3a1-2961">Operadores lógicos curto-circuito</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2961">Short-circuiting Logical Operators</span></span>

<span data-ttu-id="ab3a1-2962">O `AndAlso` e `OrElse` operadores são as versões Short-circuiting o `And` e `Or` operadores lógicos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2962">The `AndAlso` and `OrElse` operators are the short-circuiting versions of the `And` and `Or` logical operators.</span></span>

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

<span data-ttu-id="ab3a1-2963">Por conta do comportamento curto-circuito, o segundo operando não será avaliado em tempo de execução se o resultado do operador for conhecido depois de avaliar o primeiro operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2963">Because of their short circuiting behavior, the second operand is not evaluated at run time if the operator result is known after evaluating the first operand.</span></span>

<span data-ttu-id="ab3a1-2964">Os operadores lógicos Short-circuiting são avaliados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2964">The short-circuiting logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="ab3a1-2965">Se o primeiro operando em uma `AndAlso` operação é avaliada como `False` ou retorna verdadeiro de seu `IsFalse` operador, a expressão retorna o primeiro operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2965">If the first operand in an `AndAlso` operation evaluates to `False` or returns True from its `IsFalse` operator, the expression returns its first operand.</span></span> <span data-ttu-id="ab3a1-2966">Caso contrário, o segundo operando é avaliado e uma lógica `And` operação é executada em dois resultados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2966">Otherwise, the second operand is evaluated and a logical `And` operation is performed on the two results.</span></span>

* <span data-ttu-id="ab3a1-2967">Se o primeiro operando em uma `OrElse` operação é avaliada como `True` ou retorna verdadeiro de seu `IsTrue` operador, a expressão retorna o primeiro operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2967">If the first operand in an `OrElse` operation evaluates to `True` or returns True from its `IsTrue` operator, the expression returns its first operand.</span></span> <span data-ttu-id="ab3a1-2968">Caso contrário, o segundo operando é avaliado e uma lógica `Or` operação é executada em seus resultados de dois.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2968">Otherwise, the second operand is evaluated and a logical `Or` operation is performed on its two results.</span></span>

<span data-ttu-id="ab3a1-2969">O `AndAlso` e `OrElse` operadores são definidos para o tipo `Boolean`, ou para qualquer tipo `T` que sobrecarrega os operadores a seguir:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2969">The `AndAlso` and `OrElse` operators are defined for the type `Boolean`, or for any type `T` that overloads the following operators:</span></span>

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

<span data-ttu-id="ab3a1-2970">bem como a sobrecarga correspondente `And` ou `Or` operador:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2970">as well as overloading the corresponding `And` or `Or` operator:</span></span>

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

<span data-ttu-id="ab3a1-2971">Ao avaliar a `AndAlso` ou `OrElse` operadores, o primeiro operando é avaliado apenas uma vez, e o segundo operando não é avaliado ou avaliado apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2971">When evaluating the `AndAlso` or `OrElse` operators, the first operand is evaluated only once, and the second operand is either not evaluated or evaluated exactly once.</span></span> <span data-ttu-id="ab3a1-2972">Por exemplo, considere o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2972">For example, consider the following code:</span></span>

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

<span data-ttu-id="ab3a1-2973">Imprime o seguinte resultado:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2973">It prints the following result:</span></span>

```
And: False True
Or: True False
AndAlso: False
OrElse: True
```

<span data-ttu-id="ab3a1-2974">Na forma de elevadas a `AndAlso` e `OrElse` operadores, se o primeiro operando for um valor nulo `Boolean?`, em seguida, o segundo operando é avaliado, mas o resultado é sempre um valor nulo `Boolean?`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2974">In the lifted form of the `AndAlso` and `OrElse` operators, if the first operand was a null `Boolean?`, then the second operand is evaluated but the result is always a null `Boolean?`.</span></span>

<span data-ttu-id="ab3a1-2975">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2975">__Operation Type:__</span></span>

|        | <span data-ttu-id="ab3a1-2976">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2976">__Bo__</span></span> | <span data-ttu-id="ab3a1-2977">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2977">__SB__</span></span> | <span data-ttu-id="ab3a1-2978">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2978">__By__</span></span> | <span data-ttu-id="ab3a1-2979">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2979">__Sh__</span></span> | <span data-ttu-id="ab3a1-2980">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2980">__US__</span></span> | <span data-ttu-id="ab3a1-2981">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2981">__In__</span></span> | <span data-ttu-id="ab3a1-2982">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2982">__UI__</span></span> | <span data-ttu-id="ab3a1-2983">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2983">__Lo__</span></span> | <span data-ttu-id="ab3a1-2984">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2984">__UL__</span></span> | <span data-ttu-id="ab3a1-2985">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2985">__De__</span></span> | <span data-ttu-id="ab3a1-2986">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2986">__Si__</span></span> | <span data-ttu-id="ab3a1-2987">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2987">__Do__</span></span> | <span data-ttu-id="ab3a1-2988">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2988">__Da__</span></span>  | <span data-ttu-id="ab3a1-2989">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2989">__Ch__</span></span>  | <span data-ttu-id="ab3a1-2990">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2990">__St__</span></span> | <span data-ttu-id="ab3a1-2991">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2991">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ab3a1-2992">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2992">__Bo__</span></span> | <span data-ttu-id="ab3a1-2993">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2993">Bo</span></span> | <span data-ttu-id="ab3a1-2994">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2994">Bo</span></span> | <span data-ttu-id="ab3a1-2995">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2995">Bo</span></span> | <span data-ttu-id="ab3a1-2996">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2996">Bo</span></span> | <span data-ttu-id="ab3a1-2997">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2997">Bo</span></span> | <span data-ttu-id="ab3a1-2998">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2998">Bo</span></span> | <span data-ttu-id="ab3a1-2999">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-2999">Bo</span></span> | <span data-ttu-id="ab3a1-3000">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3000">Bo</span></span> | <span data-ttu-id="ab3a1-3001">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3001">Bo</span></span> | <span data-ttu-id="ab3a1-3002">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3002">Bo</span></span> | <span data-ttu-id="ab3a1-3003">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3003">Bo</span></span> | <span data-ttu-id="ab3a1-3004">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3004">Bo</span></span> | <span data-ttu-id="ab3a1-3005">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3005">Err</span></span> | <span data-ttu-id="ab3a1-3006">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3006">Err</span></span> | <span data-ttu-id="ab3a1-3007">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3007">Bo</span></span>  | <span data-ttu-id="ab3a1-3008">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3008">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3009">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3009">__SB__</span></span> |    | <span data-ttu-id="ab3a1-3010">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3010">Bo</span></span> | <span data-ttu-id="ab3a1-3011">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3011">Bo</span></span> | <span data-ttu-id="ab3a1-3012">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3012">Bo</span></span> | <span data-ttu-id="ab3a1-3013">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3013">Bo</span></span> | <span data-ttu-id="ab3a1-3014">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3014">Bo</span></span> | <span data-ttu-id="ab3a1-3015">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3015">Bo</span></span> | <span data-ttu-id="ab3a1-3016">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3016">Bo</span></span> | <span data-ttu-id="ab3a1-3017">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3017">Bo</span></span> | <span data-ttu-id="ab3a1-3018">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3018">Bo</span></span> | <span data-ttu-id="ab3a1-3019">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3019">Bo</span></span> | <span data-ttu-id="ab3a1-3020">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3020">Bo</span></span> | <span data-ttu-id="ab3a1-3021">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3021">Err</span></span> | <span data-ttu-id="ab3a1-3022">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3022">Err</span></span> | <span data-ttu-id="ab3a1-3023">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3023">Bo</span></span>  | <span data-ttu-id="ab3a1-3024">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3024">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3025">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3025">__By__</span></span> |    |    | <span data-ttu-id="ab3a1-3026">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3026">Bo</span></span> | <span data-ttu-id="ab3a1-3027">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3027">Bo</span></span> | <span data-ttu-id="ab3a1-3028">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3028">Bo</span></span> | <span data-ttu-id="ab3a1-3029">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3029">Bo</span></span> | <span data-ttu-id="ab3a1-3030">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3030">Bo</span></span> | <span data-ttu-id="ab3a1-3031">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3031">Bo</span></span> | <span data-ttu-id="ab3a1-3032">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3032">Bo</span></span> | <span data-ttu-id="ab3a1-3033">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3033">Bo</span></span> | <span data-ttu-id="ab3a1-3034">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3034">Bo</span></span> | <span data-ttu-id="ab3a1-3035">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3035">Bo</span></span> | <span data-ttu-id="ab3a1-3036">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3036">Err</span></span> | <span data-ttu-id="ab3a1-3037">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3037">Err</span></span> | <span data-ttu-id="ab3a1-3038">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3038">Bo</span></span>  | <span data-ttu-id="ab3a1-3039">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3039">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3040">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3040">__Sh__</span></span> |    |    |    | <span data-ttu-id="ab3a1-3041">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3041">Bo</span></span> | <span data-ttu-id="ab3a1-3042">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3042">Bo</span></span> | <span data-ttu-id="ab3a1-3043">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3043">Bo</span></span> | <span data-ttu-id="ab3a1-3044">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3044">Bo</span></span> | <span data-ttu-id="ab3a1-3045">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3045">Bo</span></span> | <span data-ttu-id="ab3a1-3046">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3046">Bo</span></span> | <span data-ttu-id="ab3a1-3047">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3047">Bo</span></span> | <span data-ttu-id="ab3a1-3048">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3048">Bo</span></span> | <span data-ttu-id="ab3a1-3049">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3049">Bo</span></span> | <span data-ttu-id="ab3a1-3050">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3050">Err</span></span> | <span data-ttu-id="ab3a1-3051">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3051">Err</span></span> | <span data-ttu-id="ab3a1-3052">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3052">Bo</span></span>  | <span data-ttu-id="ab3a1-3053">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3053">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3054">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3054">__US__</span></span> |    |    |    |    | <span data-ttu-id="ab3a1-3055">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3055">Bo</span></span> | <span data-ttu-id="ab3a1-3056">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3056">Bo</span></span> | <span data-ttu-id="ab3a1-3057">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3057">Bo</span></span> | <span data-ttu-id="ab3a1-3058">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3058">Bo</span></span> | <span data-ttu-id="ab3a1-3059">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3059">Bo</span></span> | <span data-ttu-id="ab3a1-3060">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3060">Bo</span></span> | <span data-ttu-id="ab3a1-3061">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3061">Bo</span></span> | <span data-ttu-id="ab3a1-3062">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3062">Bo</span></span> | <span data-ttu-id="ab3a1-3063">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3063">Err</span></span> | <span data-ttu-id="ab3a1-3064">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3064">Err</span></span> | <span data-ttu-id="ab3a1-3065">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3065">Bo</span></span>  | <span data-ttu-id="ab3a1-3066">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3066">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3067">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3067">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ab3a1-3068">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3068">Bo</span></span> | <span data-ttu-id="ab3a1-3069">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3069">Bo</span></span> | <span data-ttu-id="ab3a1-3070">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3070">Bo</span></span> | <span data-ttu-id="ab3a1-3071">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3071">Bo</span></span> | <span data-ttu-id="ab3a1-3072">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3072">Bo</span></span> | <span data-ttu-id="ab3a1-3073">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3073">Bo</span></span> | <span data-ttu-id="ab3a1-3074">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3074">Bo</span></span> | <span data-ttu-id="ab3a1-3075">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3075">Err</span></span> | <span data-ttu-id="ab3a1-3076">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3076">Err</span></span> | <span data-ttu-id="ab3a1-3077">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3077">Bo</span></span>  | <span data-ttu-id="ab3a1-3078">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3078">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3079">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3079">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ab3a1-3080">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3080">Bo</span></span> | <span data-ttu-id="ab3a1-3081">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3081">Bo</span></span> | <span data-ttu-id="ab3a1-3082">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3082">Bo</span></span> | <span data-ttu-id="ab3a1-3083">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3083">Bo</span></span> | <span data-ttu-id="ab3a1-3084">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3084">Bo</span></span> | <span data-ttu-id="ab3a1-3085">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3085">Bo</span></span> | <span data-ttu-id="ab3a1-3086">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3086">Err</span></span> | <span data-ttu-id="ab3a1-3087">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3087">Err</span></span> | <span data-ttu-id="ab3a1-3088">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3088">Bo</span></span>  | <span data-ttu-id="ab3a1-3089">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3089">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3090">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3090">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-3091">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3091">Bo</span></span> | <span data-ttu-id="ab3a1-3092">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3092">Bo</span></span> | <span data-ttu-id="ab3a1-3093">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3093">Bo</span></span> | <span data-ttu-id="ab3a1-3094">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3094">Bo</span></span> | <span data-ttu-id="ab3a1-3095">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3095">Bo</span></span> | <span data-ttu-id="ab3a1-3096">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3096">Err</span></span> | <span data-ttu-id="ab3a1-3097">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3097">Err</span></span> | <span data-ttu-id="ab3a1-3098">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3098">Bo</span></span>  | <span data-ttu-id="ab3a1-3099">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3099">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3100">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3100">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-3101">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3101">Bo</span></span> | <span data-ttu-id="ab3a1-3102">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3102">Bo</span></span> | <span data-ttu-id="ab3a1-3103">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3103">Bo</span></span> | <span data-ttu-id="ab3a1-3104">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3104">Bo</span></span> | <span data-ttu-id="ab3a1-3105">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3105">Err</span></span> | <span data-ttu-id="ab3a1-3106">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3106">Err</span></span> | <span data-ttu-id="ab3a1-3107">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3107">Bo</span></span>  | <span data-ttu-id="ab3a1-3108">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3108">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3109">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3109">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-3110">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3110">Bo</span></span> | <span data-ttu-id="ab3a1-3111">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3111">Bo</span></span> | <span data-ttu-id="ab3a1-3112">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3112">Bo</span></span> | <span data-ttu-id="ab3a1-3113">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3113">Err</span></span> | <span data-ttu-id="ab3a1-3114">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3114">Err</span></span> | <span data-ttu-id="ab3a1-3115">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3115">Bo</span></span>  | <span data-ttu-id="ab3a1-3116">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3116">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3117">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3117">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-3118">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3118">Bo</span></span> | <span data-ttu-id="ab3a1-3119">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3119">Bo</span></span> | <span data-ttu-id="ab3a1-3120">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3120">Err</span></span> | <span data-ttu-id="ab3a1-3121">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3121">Err</span></span> | <span data-ttu-id="ab3a1-3122">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3122">Bo</span></span>  | <span data-ttu-id="ab3a1-3123">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3123">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3124">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3124">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-3125">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3125">Bo</span></span> | <span data-ttu-id="ab3a1-3126">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3126">Err</span></span> | <span data-ttu-id="ab3a1-3127">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3127">Err</span></span> | <span data-ttu-id="ab3a1-3128">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3128">Bo</span></span>  | <span data-ttu-id="ab3a1-3129">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3129">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3130">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3130">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ab3a1-3131">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3131">Err</span></span> | <span data-ttu-id="ab3a1-3132">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3132">Err</span></span> | <span data-ttu-id="ab3a1-3133">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3133">Err</span></span> | <span data-ttu-id="ab3a1-3134">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3134">Err</span></span> | 
| <span data-ttu-id="ab3a1-3135">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3135">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ab3a1-3136">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3136">Err</span></span> | <span data-ttu-id="ab3a1-3137">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3137">Err</span></span> | <span data-ttu-id="ab3a1-3138">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3138">Err</span></span> | 
| <span data-ttu-id="ab3a1-3139">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3139">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ab3a1-3140">bO</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3140">Bo</span></span>  | <span data-ttu-id="ab3a1-3141">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3141">Ob</span></span>  | 
| <span data-ttu-id="ab3a1-3142">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3142">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ab3a1-3143">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3143">Ob</span></span>  | 


## <a name="shift-operators"></a><span data-ttu-id="ab3a1-3144">Operadores Shift</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3144">Shift Operators</span></span>

<span data-ttu-id="ab3a1-3145">Os operadores binários `<<` e `>>` executar operações de mudança de bits.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3145">The binary operators `<<` and `>>` perform bit shifting operations.</span></span>

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

<span data-ttu-id="ab3a1-3146">Os operadores são definidos para o `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` e `Long` tipos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3146">The operators are defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long` types.</span></span> <span data-ttu-id="ab3a1-3147">Ao contrário dos outros operadores binários, o tipo de resultado de uma operação de deslocamento é determinado como se o operador foi um operador unário com apenas o operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3147">Unlike the other binary operators, the result type of a shift operation is determined as if the operator was a unary operator with just the left operand.</span></span> <span data-ttu-id="ab3a1-3148">O tipo do operando direito deve ser implicitamente conversível para `Integer` e não é usada para determinar o tipo de resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3148">The type of the right operand must be implicitly convertible to `Integer` and is not used in determining the result type of the operation.</span></span>

<span data-ttu-id="ab3a1-3149">O `<<` operador faz com que os bits no primeiro operando a ser deslocado à esquerda do número de casas especificado pelo valor de deslocamento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3149">The `<<` operator causes the bits in the first operand to be shifted left the number of places specified by the shift amount.</span></span> <span data-ttu-id="ab3a1-3150">Os bits de ordem superior fora do intervalo do tipo de resultado são descartados e as posições de bits vagas de ordem inferior são preenchidos com zeros.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3150">The high-order bits outside the range of the result type are discarded and the low-order vacated bit positions are zero-filled.</span></span>

<span data-ttu-id="ab3a1-3151">O `>>` faz com que o operador os bits no primeiro operando a ser deslocado com o botão direito o número de casas especificado pelo valor de deslocamento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3151">The `>>` operator causes the bits in the first operand to be shifted right the number of places specified by the shift amount.</span></span> <span data-ttu-id="ab3a1-3152">Os bits de ordem inferior são descartados e as posições de bits vagas de ordem superior são definidas como zero se o operando esquerdo for positivo ou a um negativo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3152">The low-order bits are discarded and the high-order vacated bit positions are set to zero if the left operand is positive or to one if negative.</span></span> <span data-ttu-id="ab3a1-3153">Se o operando esquerdo for do tipo `Byte`, `UShort`, `UInteger`, ou `ULong` os bits de ordem superior vagas são preenchidas com zeros.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3153">If the left operand is of type `Byte`, `UShort`, `UInteger`, or `ULong` the vacant high-order bits are zero-filled.</span></span>

<span data-ttu-id="ab3a1-3154">Os operadores shift desviam os bits da representação subjacente do primeiro operando pelo valor do segundo operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3154">The shift operators shift the bits of the underlying representation of the first operand by the amount of the second operand.</span></span> <span data-ttu-id="ab3a1-3155">Se o valor do segundo operando é maior que o número de bits no primeiro operando, ou é negativo, o valor de deslocamento é computado como `RightOperand And SizeMask` onde `SizeMask` é:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3155">If the value of the second operand is greater than the number of bits in the first operand, or is negative, then the shift amount is computed as `RightOperand And SizeMask` where `SizeMask` is:</span></span>

| <span data-ttu-id="ab3a1-3156">__Tipo de LeftOperand__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3156">__LeftOperand Type__</span></span>  | <span data-ttu-id="ab3a1-3157">__SizeMask__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3157">__SizeMask__</span></span> | 
|-----------------------|--------------|
| <span data-ttu-id="ab3a1-3158">`Byte`, `SByte`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3158">`Byte`, `SByte`</span></span>       | <span data-ttu-id="ab3a1-3159">7 (`&H7`)</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3159">7 (`&H7`)</span></span>    | 
| <span data-ttu-id="ab3a1-3160">`UShort`, `Short`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3160">`UShort`, `Short`</span></span>     | <span data-ttu-id="ab3a1-3161">15 (`&HF`)</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3161">15 (`&HF`)</span></span>   | 
| <span data-ttu-id="ab3a1-3162">`UInteger`, `Integer`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3162">`UInteger`, `Integer`</span></span> | <span data-ttu-id="ab3a1-3163">31 (`&H1F`)</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3163">31 (`&H1F`)</span></span>  | 
| <span data-ttu-id="ab3a1-3164">`ULong`, `Long`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3164">`ULong`, `Long`</span></span>       | <span data-ttu-id="ab3a1-3165">63 (`&H3F`)</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3165">63 (`&H3F`)</span></span>  | 

<span data-ttu-id="ab3a1-3166">Se o valor de deslocamento for zero, o resultado da operação é idêntico ao valor do primeiro operando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3166">If the shift amount is zero, the result of the operation is identical to the value of the first operand.</span></span> <span data-ttu-id="ab3a1-3167">Não há estouros são possíveis dessas operações.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3167">No overflows are possible from these operations.</span></span>

<span data-ttu-id="ab3a1-3168">__Tipo de operação:__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3168">__Operation Type:__</span></span>


| <span data-ttu-id="ab3a1-3169">__bO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3169">__Bo__</span></span> | <span data-ttu-id="ab3a1-3170">__SB__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3170">__SB__</span></span> | <span data-ttu-id="ab3a1-3171">__Por__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3171">__By__</span></span> | <span data-ttu-id="ab3a1-3172">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3172">__Sh__</span></span> | <span data-ttu-id="ab3a1-3173">__NÓS__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3173">__US__</span></span> | <span data-ttu-id="ab3a1-3174">__In__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3174">__In__</span></span> | <span data-ttu-id="ab3a1-3175">__INTERFACE DO USUÁRIO__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3175">__UI__</span></span> | <span data-ttu-id="ab3a1-3176">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3176">__Lo__</span></span> | <span data-ttu-id="ab3a1-3177">__UL__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3177">__UL__</span></span> | <span data-ttu-id="ab3a1-3178">__Alemanha__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3178">__De__</span></span> | <span data-ttu-id="ab3a1-3179">__si__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3179">__Si__</span></span> | <span data-ttu-id="ab3a1-3180">__Do__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3180">__Do__</span></span> | <span data-ttu-id="ab3a1-3181">__Acelerador de desenvolvimento__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3181">__Da__</span></span>  | <span data-ttu-id="ab3a1-3182">__CH__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3182">__Ch__</span></span>  | <span data-ttu-id="ab3a1-3183">__ST__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3183">__St__</span></span> | <span data-ttu-id="ab3a1-3184">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3184">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="ab3a1-3185">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3185">Sh</span></span> | <span data-ttu-id="ab3a1-3186">SB</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3186">SB</span></span> | <span data-ttu-id="ab3a1-3187">Por</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3187">By</span></span> | <span data-ttu-id="ab3a1-3188">Sh</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3188">Sh</span></span> | <span data-ttu-id="ab3a1-3189">US</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3189">US</span></span> | <span data-ttu-id="ab3a1-3190">No</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3190">In</span></span> | <span data-ttu-id="ab3a1-3191">Interface de Usuário</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3191">UI</span></span> | <span data-ttu-id="ab3a1-3192">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3192">Lo</span></span> | <span data-ttu-id="ab3a1-3193">UL</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3193">UL</span></span> | <span data-ttu-id="ab3a1-3194">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3194">Lo</span></span> | <span data-ttu-id="ab3a1-3195">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3195">Lo</span></span> | <span data-ttu-id="ab3a1-3196">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3196">Lo</span></span> | <span data-ttu-id="ab3a1-3197">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3197">Err</span></span> | <span data-ttu-id="ab3a1-3198">Err</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3198">Err</span></span> | <span data-ttu-id="ab3a1-3199">Lo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3199">Lo</span></span> | <span data-ttu-id="ab3a1-3200">Ob</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3200">Ob</span></span> | 


## <a name="boolean-expressions"></a><span data-ttu-id="ab3a1-3201">Expressões boolianas</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3201">Boolean Expressions</span></span>

<span data-ttu-id="ab3a1-3202">Uma expressão booleana é uma expressão que pode ser testada para ver se ela for verdadeira ou se for falsa.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3202">A Boolean expression is an expression that can be tested to see if it is true or if it is false.</span></span>

```antlr
BooleanExpression
    : Expression
    ;
```

<span data-ttu-id="ab3a1-3203">Um tipo `T` pode ser usado em uma expressão booleana se, na ordem de preferência:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3203">A type `T` can be used in a Boolean expression if, in order of preference:</span></span>

* <span data-ttu-id="ab3a1-3204">`T` é `Boolean` ou `Boolean?`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3204">`T` is `Boolean` or `Boolean?`</span></span>

* <span data-ttu-id="ab3a1-3205">`T` tem uma conversão de ampliação `Boolean`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3205">`T` has a widening conversion to `Boolean`</span></span>

* <span data-ttu-id="ab3a1-3206">`T` tem uma conversão de ampliação `Boolean?`</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3206">`T` has a widening conversion to `Boolean?`</span></span>

* <span data-ttu-id="ab3a1-3207">`T` define dois operadores de pseudo `IsTrue` e `IsFalse`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3207">`T` defines two pseudo operators, `IsTrue` and `IsFalse`.</span></span>

* <span data-ttu-id="ab3a1-3208">`T` tem uma conversão de estreitamento `Boolean?` que não envolve uma conversão de `Boolean` para `Boolean?`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3208">`T` has a narrowing conversion to `Boolean?` that does not involve a conversion from `Boolean` to `Boolean?`.</span></span>

* <span data-ttu-id="ab3a1-3209">`T` tem uma conversão redutora para `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3209">`T` has a narrowing conversion to `Boolean`.</span></span>

<span data-ttu-id="ab3a1-3210">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3210">__Note.__</span></span> <span data-ttu-id="ab3a1-3211">É interessante observar que, se `Option Strict` estiver desativado, uma expressão que tem uma conversão de estreitamento `Boolean` serão aceitas sem um erro de tempo de compilação, mas a linguagem ainda preferirão um `IsTrue` operador se ele existir.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3211">It is interesting to note that if `Option Strict` is off, an expression that has a narrowing conversion to `Boolean` will be accepted without a compile-time error but the language will still prefer an `IsTrue` operator if it exists.</span></span> <span data-ttu-id="ab3a1-3212">Isso ocorre porque `Option Strict` altera apenas o que é e não é aceito pela linguagem e nunca altera o significado real de uma expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3212">This is because `Option Strict` only changes what is and isn't accepted by the language, and never changes the actual meaning of an expression.</span></span> <span data-ttu-id="ab3a1-3213">Portanto, `IsTrue` deve sempre ser preferível em vez de uma conversão de redução, independentemente de `Option Strict`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3213">Thus, `IsTrue` has to always be preferred over a narrowing conversion, regardless of `Option Strict`.</span></span>

<span data-ttu-id="ab3a1-3214">Por exemplo, a seguinte classe não define uma conversão de ampliação para `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3214">For example, the following class does not define a widening conversion to `Boolean`.</span></span> <span data-ttu-id="ab3a1-3215">Como resultado, seu uso na `If` instrução faz com que uma chamada para o `IsTrue` operador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3215">As a result, its use in the `If` statement causes a call to the `IsTrue` operator.</span></span>

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

<span data-ttu-id="ab3a1-3216">Se uma expressão booleana for digitada como ou convertida em `Boolean` ou `Boolean?`, em seguida, ele é verdadeiro se o valor é `True` e false caso contrário.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3216">If a Boolean expression is typed as or converted to `Boolean` or `Boolean?`, then it is true if the value is `True` and false otherwise.</span></span>

<span data-ttu-id="ab3a1-3217">Caso contrário, uma expressão booliana chama o `IsTrue` operador e retorna `True` se o operador retornado `True`; caso contrário, será false (mas nunca chama o `IsFalse` operador).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3217">Otherwise, a Boolean expression calls the `IsTrue` operator and returns `True` if the operator returned `True`; otherwise it is false (but never calls the `IsFalse` operator).</span></span>

<span data-ttu-id="ab3a1-3218">No exemplo a seguir, `Integer` tem uma conversão de estreitamento `Boolean`, portanto, um valor nulo `Integer?` tem uma conversão redutora para ambos `Boolean?` (produzindo um valor nulo `Boolean`) e, ao `Boolean` (que gera uma exceção).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3218">In the following example, `Integer` has a narrowing conversion to `Boolean`, so a null `Integer?` has a narrowing conversion to both `Boolean?` (yielding a null `Boolean`) and to `Boolean` (which throws an exception).</span></span> <span data-ttu-id="ab3a1-3219">A conversão de estreitamento `Boolean?` é preferível e, portanto, o valor de "`i`" como um valor booliano a expressão é, portanto, `False`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3219">The narrowing conversion to `Boolean?` is preferred, and so the value of "`i`" as a Boolean expression is therefore `False`.</span></span>

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a><span data-ttu-id="ab3a1-3220">Expressões lambda</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3220">Lambda Expressions</span></span>

<span data-ttu-id="ab3a1-3221">Um *expressão lambda* define um método anônimo chamado um *método lambda*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3221">A *lambda expression* defines an anonymous method called a *lambda method*.</span></span> <span data-ttu-id="ab3a1-3222">Métodos de lambda tornam fácil passar métodos "na linha" para outros métodos que usam tipos de delegado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3222">Lambda methods make it easy to pass "in-line" methods to other methods that take delegate types.</span></span>

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

<span data-ttu-id="ab3a1-3223">O Exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3223">The example:</span></span>

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

<span data-ttu-id="ab3a1-3224">imprimirá:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3224">will print out:</span></span>

```
2 4 6 8
```

<span data-ttu-id="ab3a1-3225">Uma expressão lambda começa com os modificadores opcionais `Async` ou `Iterator`, seguido da palavra-chave `Function` ou `Sub` e uma lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3225">A lambda expression begins with the optional modifiers `Async` or `Iterator`, followed by the keyword `Function` or `Sub` and a parameter list.</span></span> <span data-ttu-id="ab3a1-3226">Parâmetros em uma expressão lambda não podem ser declarados `Optional` ou `ParamArray` e não pode ter atributos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3226">Parameters in a lambda expression cannot be declared `Optional` or `ParamArray` and cannot have attributes.</span></span> <span data-ttu-id="ab3a1-3227">Ao contrário dos métodos regulares, omitir um tipo de parâmetro para um método lambda não inferir automaticamente `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3227">Unlike regular methods, omitting a parameter type for a lambda method does not automatically infer `Object`.</span></span> <span data-ttu-id="ab3a1-3228">Em vez disso, quando um método lambda é reclassificado, os tipos de parâmetro omitido e `ByRef` modificadores são inferidos do tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3228">Instead, when a lambda method is reclassified, the omitted parameter types and `ByRef` modifiers are inferred from the target type.</span></span> <span data-ttu-id="ab3a1-3229">No exemplo anterior, a expressão lambda poderia ter sido escrita como `Function(x) x * 2`, e ele seria inferido do tipo de `x` ser `Integer` quando o método lambda foi usado para criar uma instância das `IntFunc` tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3229">In the previous example, the lambda expression could have been written as `Function(x) x * 2`, and it would have inferred the type of `x` to be `Integer` when the lambda method was used to create an instance of the `IntFunc` delegate type.</span></span> <span data-ttu-id="ab3a1-3230">Ao contrário de inferência de tipos de variável local, se um parâmetro de método lambda omite um tipo, mas tem uma matriz ou o modificador anulável nome, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3230">Unlike local variable inference, if a lambda method parameter omits a type but has an array or nullable name modifier, a compile-time error occurs.</span></span>

<span data-ttu-id="ab3a1-3231">Um __expressão lambda regular__ é aquela com nenhum dos dois `Async` nem `Iterator` modificadores.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3231">A __regular lambda expression__ is one with neither `Async` nor `Iterator` modifiers.</span></span>

<span data-ttu-id="ab3a1-3232">Uma __expressão lambda de iterador__ é aquele com o `Iterator` modificador e nenhum `Async` modificador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3232">An __iterator lambda expression__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="ab3a1-3233">Ele deve ser uma função.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3233">It must be a function.</span></span> <span data-ttu-id="ab3a1-3234">Quando ele é reclassificado como um valor, ele só pode ser reclassificado para um valor do tipo de delegado, cujo tipo de retorno é `IEnumerator`, ou `IEnumerable`, ou `IEnumerator(Of T)` ou `IEnumerable(Of T)` para alguns `T`, e que não tem nenhum parâmetro ByRef.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3234">When it is reclassified to a value, it can only be reclassified to a value of delegate type whose return type is `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="ab3a1-3235">Uma __expressão lambda assíncrona__ é aquele com o `Async` modificador e nenhum `Iterator` modificador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3235">An __async lambda expression__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="ab3a1-3236">Um lambda de sub async só pode ser reclassificado para um valor do tipo de delegado sub sem parâmetros ByRef.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3236">An async sub lambda may only be reclassified to a value of sub delegate type with no ByRef parameters.</span></span> <span data-ttu-id="ab3a1-3237">Um lambda de função async só pode ser reclassificado para um valor do tipo de delegado de função cujo tipo de retorno será `Task` ou `Task(Of T)` para alguns `T`, e que não tem nenhum parâmetro ByRef.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3237">An async function lambda may only be reclassified to a value of function delegate type whose return type is `Task` or `Task(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="ab3a1-3238">Expressões lambda podem ser linha única ou várias linhas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3238">Lambda expressions can either be single-line or multi-line.</span></span> <span data-ttu-id="ab3a1-3239">Linha única `Function` expressões lambda contenham uma única expressão que representa o valor retornado do método lambda.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3239">Single-line `Function` lambda expressions contain a single expression that represents the value returned from the lambda method.</span></span> <span data-ttu-id="ab3a1-3240">Linha única `Sub` expressões lambda contêm uma única instrução sem seu fechamento `StatementTerminator`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3240">Single-line `Sub` lambda expressions contain a single statement without its closing `StatementTerminator`.</span></span> <span data-ttu-id="ab3a1-3241">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3241">For example:</span></span>

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

<span data-ttu-id="ab3a1-3242">Linha única lambda constrói bind menos firmemente que todas as outras expressões e instruções.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3242">Single-line lambda constructs bind less tightly than all other expressions and statements.</span></span> <span data-ttu-id="ab3a1-3243">Assim, por exemplo, "`Function() x + 5`"é equivalente a"`Function() (x+5)"` em vez de"`(Function() x) + 5`".</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3243">Thus, for example, "`Function() x + 5`" is equivalent to "`Function() (x+5)"` rather than "`(Function() x) + 5`".</span></span> <span data-ttu-id="ab3a1-3244">Para evitar ambiguidade, uma única linha `Sub` expressão lambda não pode conter uma instrução Dim ou uma instrução de declaração de rótulo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3244">To avoid ambiguity, a single-line `Sub` lambda expression may not contain a Dim statement or a label declaration statement.</span></span> <span data-ttu-id="ab3a1-3245">Além disso, a menos que ele é colocado entre parênteses, uma única linha `Sub` expressão lambda não pode ser seguido imediatamente por dois-pontos ":", um operador de acesso de membro ".", um operador de acesso de membro de dicionário "!" ou um parêntese de abertura "(".</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3245">Also, unless it is enclosed in parentheses, a single-line `Sub` lambda expression may not be immediately followed by a colon ":", a member access operator ".", a dictionary member access operator "!" or an open parenthesis "(".</span></span> <span data-ttu-id="ab3a1-3246">Ele não pode conter qualquer instrução do bloco (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) nem `OnError` nem `Resume`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3246">It may not contain any block statement (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) nor `OnError` nor `Resume`.</span></span>

<span data-ttu-id="ab3a1-3247">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3247">__Note.__</span></span> <span data-ttu-id="ab3a1-3248">Na expressão lambda `Function(i) x=i`, o corpo é interpretado como um *expressão* (quais testes se `x` e `i` são iguais).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3248">In the lambda expression `Function(i) x=i`, the body is interpreted as an *expression* (which tests whether `x` and `i` are equal).</span></span> <span data-ttu-id="ab3a1-3249">Mas, na expressão lambda `Sub(i) x=i`, o corpo é interpretado como uma instrução (que atribui `i` para `x`).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3249">But in the lambda expression `Sub(i) x=i`, the body is interpreted as a statement (which assigns `i` to `x`).</span></span>

<span data-ttu-id="ab3a1-3250">Uma expressão lambda de várias linhas contém um bloco de instruções e deve terminar com um número apropriado `End` instrução (ou seja, `End Function` ou `End Sub`).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3250">A multi-line lambda expression contains a statement block and must end with an appropriate `End` statement (i.e. `End Function` or `End Sub`).</span></span> <span data-ttu-id="ab3a1-3251">Assim como acontece com métodos regulares, um método lambda de várias linhas `Function` ou `Sub` instrução e `End` instruções devem ser em suas próprias linhas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3251">As with regular methods, a multi-line lambda method's `Function` or `Sub` statement and `End` statements must be on their own lines.</span></span> <span data-ttu-id="ab3a1-3252">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3252">For example:</span></span>

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

<span data-ttu-id="ab3a1-3253">Várias linhas `Function` expressões lambda pode declarar um tipo de retorno, mas não é possível colocar atributos nele.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3253">Multi-line `Function` lambda expressions can declare a return type but cannot put attributes on it.</span></span> <span data-ttu-id="ab3a1-3254">Se um multilinha `Function` expressão lambda não declara um tipo de retorno, mas o tipo de retorno pode ser inferido a partir do contexto no qual a expressão lambda é usada e, em seguida, esse tipo de retorno é usado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3254">If a multi-line `Function` lambda expression does not declare a return type but the return type can be inferred from the context in which the lambda expression is used , then that return type is used.</span></span> <span data-ttu-id="ab3a1-3255">Caso contrário, o tipo de retorno da função é calculado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3255">Otherwise the return type of the function is calculated as follows:</span></span>

* <span data-ttu-id="ab3a1-3256">Em uma expressão lambda regular, o tipo de retorno é o tipo dominante das expressões em todos os `Return` instruções no bloco de instrução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3256">In a regular lambda expression, the return type is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="ab3a1-3257">Em uma expressão de lambda async, é o tipo de retorno `Task(Of T)` onde `T` é o tipo dominante das expressões em todos os `Return` instruções no bloco de instrução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3257">In an async lambda expression, the return type is `Task(Of T)` where `T` is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="ab3a1-3258">Em uma expressão lambda de iterador, é o tipo de retorno `IEnumerable(Of T)` onde `T` é o tipo dominante das expressões em todos os `Yield` instruções no bloco de instrução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3258">In an iterator lambda expression, the return type is `IEnumerable(Of T)` where `T` is the dominant type of the expressions in all the `Yield` statements in the statement block.</span></span>

<span data-ttu-id="ab3a1-3259">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3259">For example:</span></span>

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

<span data-ttu-id="ab3a1-3260">Em todos os casos, se não houver nenhuma `Return` (respectivamente `Yield`) instruções, ou se não há nenhum tipo dominante entre eles e semântica estrita está sendo usada, ocorrerá um erro de tempo de compilação; caso contrário, o tipo dominante será implicitamente `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3260">In all cases, if there are no `Return` (respectively `Yield`) statements, or if there is no dominant type among them, and strict semantics are being used, a compile-time error occurs; otherwise the dominant type is implicitly `Object`.</span></span>

<span data-ttu-id="ab3a1-3261">Observe que o tipo de retorno é calculado de todos os `Return` instruções, mesmo se eles não estão acessíveis.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3261">Note that the return type is calculated from all `Return` statements, even if they are not reachable.</span></span> <span data-ttu-id="ab3a1-3262">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3262">For example:</span></span>

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

<span data-ttu-id="ab3a1-3263">Não há nenhuma variável de retorno implícito, pois não há nenhum nome da variável.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3263">There is no implicit return variable, as there is no name for the variable.</span></span>

<span data-ttu-id="ab3a1-3264">Os blocos de instrução dentro de expressões lambda de várias linhas têm as seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3264">The statement blocks inside multi-line lambda expressions have the following restrictions:</span></span>

* <span data-ttu-id="ab3a1-3265">`On Error` e `Resume` instruções não são permitidas, embora `Try` instruções são permitidas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3265">`On Error` and `Resume` statements are not allowed, although `Try` statements are allowed.</span></span>

* <span data-ttu-id="ab3a1-3266">Variáveis locais estáticas não podem ser declarados em expressões lambda de várias linhas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3266">Static locals cannot be declared in multi-line lambda expressions.</span></span>

* <span data-ttu-id="ab3a1-3267">Não é possível ramificar para dentro ou fora do bloco de instrução de uma expressão lambda de várias linhas, embora as regras normais de ramificação aplicam dentro dele.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3267">It is not possible to branch into or out of the statement block of a multi-line lambda expression, although the normal branching rules apply within it.</span></span> <span data-ttu-id="ab3a1-3268">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3268">For example:</span></span>

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

<span data-ttu-id="ab3a1-3269">Uma expressão lambda é aproximadamente equivalente a um método anônimo declarado no tipo recipiente.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3269">A lambda expression is roughly equivalent to an anonymous method declared on the containing type.</span></span> <span data-ttu-id="ab3a1-3270">O exemplo inicial é aproximadamente equivalente a:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3270">The initial example is roughly equivalent to:</span></span>

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


### <a name="closures"></a><span data-ttu-id="ab3a1-3271">Fechamentos</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3271">Closures</span></span>

<span data-ttu-id="ab3a1-3272">Expressões lambda têm acesso a todas as variáveis no escopo, inclusive variáveis locais ou parâmetros definidos nas expressões lambda e de método recipientes.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3272">Lambda expressions have access to all of the variables in scope, including local variables or parameters defined in the containing method and lambda expressions.</span></span> <span data-ttu-id="ab3a1-3273">Quando uma expressão lambda faz referência a um parâmetro ou variável local, a expressão lambda captura a variável que está sendo chamada em um fechamento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3273">When a lambda expression refers to a local variable or parameter, the lambda expression captures the variable being referred to into a closure.</span></span> <span data-ttu-id="ab3a1-3274">Um fechamento é um objeto que reside no heap, em vez de na pilha e, quando uma variável é capturada, todas as referências à variável são redirecionadas para o fechamento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3274">A closure is an object that lives on the heap instead of on the stack, and when a variable is captured, all references to the variable are redirected to the closure.</span></span> <span data-ttu-id="ab3a1-3275">Isso permite que as expressões lambda continuar fazer referência a parâmetros e variáveis locais, mesmo depois que o método que contém for concluído.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3275">This enables lambda expressions to continue to refer to local variables and parameters even after the containing method is complete.</span></span> <span data-ttu-id="ab3a1-3276">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3276">For example:</span></span>

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

<span data-ttu-id="ab3a1-3277">É aproximadamente equivalente a:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3277">is roughly equivalent to:</span></span>

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

<span data-ttu-id="ab3a1-3278">Um fechamento captura uma nova cópia de um local variável cada vez que ele entra no bloco em que a variável local é declarada, mas a nova cópia é inicializada com o valor da cópia anterior, se houver.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3278">A closure captures a new copy of a local variable each time it enters the block in which the local variable is declared, but the new copy is initialized with the value of the previous copy, if any.</span></span> <span data-ttu-id="ab3a1-3279">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3279">For example:</span></span>

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

<span data-ttu-id="ab3a1-3280">Imprime</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3280">prints</span></span>

```
1 2 3 4 5 6 7 8 9 10
```

<span data-ttu-id="ab3a1-3281">Em vez de</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3281">instead of</span></span>

```
9 9 9 9 9 9 9 9 9 9
```

<span data-ttu-id="ab3a1-3282">Como fechamentos precisam ser inicializado ao inserir um bloco, ele não tem permissão para `GoTo` em um bloco com um fechamento de fora desse bloco, embora ele tem permissão para `Resume` em um bloco com um fechamento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3282">Because closures have to be initialized when entering a block, it is not allowed to `GoTo` into a block with a closure from outside of that block, although it is allowed to `Resume` into a block with a closure.</span></span> <span data-ttu-id="ab3a1-3283">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3283">For example:</span></span>

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

<span data-ttu-id="ab3a1-3284">Porque eles não podem ser capturados em um fechamento, o código a seguir não pode aparecer dentro de uma expressão lambda:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3284">Because they cannot be captured into a closure, the following cannot appear inside of a lambda expression:</span></span>

* <span data-ttu-id="ab3a1-3285">Parâmetros de referência.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3285">Reference parameters.</span></span>

* <span data-ttu-id="ab3a1-3286">Expressões de instância (`Me`, `MyClass`, `MyBase`), se o tipo de `Me` não é uma classe.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3286">Instance expressions (`Me`, `MyClass`, `MyBase`), if the type of `Me` is not a class.</span></span>

<span data-ttu-id="ab3a1-3287">Os membros de uma expressão de criação de tipo anônimo, se a expressão lambda é parte da expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3287">The members of an anonymous type-creation expression, if the lambda expression is part of the expression.</span></span> <span data-ttu-id="ab3a1-3288">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3288">For example:</span></span>

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

<span data-ttu-id="ab3a1-3289">`ReadOnly` variáveis de instância em construtores de instância ou `ReadOnly` compartilhado variáveis em construtores compartilhados onde as variáveis são usadas em um contexto sem valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3289">`ReadOnly` instance variables in instance constructors or `ReadOnly` shared variables in shared constructors where the variables are used in a non-value context.</span></span> <span data-ttu-id="ab3a1-3290">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3290">For example:</span></span>

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

## <a name="query-expressions"></a><span data-ttu-id="ab3a1-3291">Expressões de consulta</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3291">Query Expressions</span></span>

<span data-ttu-id="ab3a1-3292">Um *expressão de consulta* é uma expressão que se aplica a uma série de *operadores de consulta* aos elementos de um *passível de consulta* coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3292">A *query expression* is an expression that applies a series of *query operators* to the elements of a *queryable* collection.</span></span> <span data-ttu-id="ab3a1-3293">Por exemplo, a expressão a seguir usa uma coleção de `Customer` objetos e retorna os nomes de todos os clientes no estado de Washington:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3293">For example, the following expression takes a collection of `Customer` objects and returns the names of all the customers in the state of Washington:</span></span>

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

<span data-ttu-id="ab3a1-3294">Uma expressão de consulta deve começar com uma `From` ou um `Aggregate` operador e pode terminar com qualquer operador de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3294">A query expression must start with a `From` or an `Aggregate` operator and can end with any query operator.</span></span> <span data-ttu-id="ab3a1-3295">O resultado de uma expressão de consulta é classificado como um valor. o tipo de resultado da expressão depende o tipo de resultado do último operador de consulta na expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3295">The result of a query expression is classified as a value; the result type of the expression depends on the result type of the last query operator in the expression.</span></span>

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

### <a name="range-variables"></a><span data-ttu-id="ab3a1-3296">Variáveis de intervalo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3296">Range Variables</span></span>

<span data-ttu-id="ab3a1-3297">Alguns operadores de consulta apresentam um tipo especial de variável chamada de um *variável de intervalo*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3297">Some query operators introduce a special kind of variable called a *range variable*.</span></span> <span data-ttu-id="ab3a1-3298">Variáveis de intervalo não são variáveis real; em vez disso, eles representam os valores individuais durante a avaliação da consulta ao longo de coleções de entrada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3298">Range variables are not real variables; instead, they represent the individual values during the evaluation of the query over the input collections.</span></span>

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

<span data-ttu-id="ab3a1-3299">Variáveis de intervalo estão no escopo do operador de consulta introduzindo até o final de uma expressão de consulta, ou para um operador de consulta, como `Select` que oculta-os.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3299">Range variables are scoped from the introducing query operator to the end of a query expression, or to a query operator such as `Select` that hides them.</span></span> <span data-ttu-id="ab3a1-3300">Por exemplo, na consulta a seguir</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3300">For example, in the following query</span></span>

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

<span data-ttu-id="ab3a1-3301">o `From` operador apresenta uma variável de intervalo de consulta `cust` digitado como `Customer` que representa cada cliente no `Customers` coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3301">the `From` query operator introduces a range variable `cust` typed as `Customer` that represents each customer in the `Customers` collection.</span></span> <span data-ttu-id="ab3a1-3302">O seguinte `Where` operador de consulta, em seguida, se refere à variável de intervalo `cust` na expressão de filtro para determinar se deve filtrar um cliente individual da coleção resultante.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3302">The following `Where` query operator then refers to the range variable `cust` in the filter expression to determine whether to filter an individual customer out of the resulting collection.</span></span>

<span data-ttu-id="ab3a1-3303">Há dois tipos de variáveis de intervalo: *variáveis de intervalo de coleta* e *variáveis de intervalo de expressão*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3303">There are two types of range variables: *collection range variables* and *expression range variables*.</span></span> <span data-ttu-id="ab3a1-3304">Variáveis de intervalo da coleção levar seus valores dos elementos das coleções que estão sendo consultados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3304">Collection range variables take their values from the elements of the collections being queried.</span></span> <span data-ttu-id="ab3a1-3305">A expressão de coleção em uma declaração de variável de intervalo de coleta deve ser classificada como um valor cujo tipo é passível de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3305">The collection expression in a collection range variable declaration must be classified as a value whose type is queryable.</span></span> <span data-ttu-id="ab3a1-3306">Se o tipo de uma variável de intervalo de coleta for omitido, ele é inferido como o tipo de elemento da expressão de coleção, ou `Object` se a expressão de coleção não tem um tipo de elemento (ou seja, apenas define um `Cast` método).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3306">If the type of a collection range variable is omitted, it is inferred to be the element type of the collection expression, or `Object` if the collection expression does not have an element type (i.e. only defines a `Cast` method).</span></span> <span data-ttu-id="ab3a1-3307">Se a expressão de coleção não é passível de consulta (ou seja, o tipo de elemento da coleção não pode ser inferido), resultados de um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3307">If the collection expression is not queryable (i.e. the element type of the collection cannot be inferred), a compile-time error results.</span></span>

<span data-ttu-id="ab3a1-3308">Uma variável de intervalo de expressão é uma variável de intervalo cujo valor é calculado por uma expressão em vez de uma coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3308">An expression range variable is a range variable whose value is calculated by an expression rather than a collection.</span></span> <span data-ttu-id="ab3a1-3309">No exemplo a seguir, o `Select` consulta operador apresenta uma variável de intervalo de expressão denominada `cityState` calculado a partir de dois campos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3309">In the following example, the `Select` query operator introduces an expression range variable named `cityState` calculated from two fields:</span></span>

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

<span data-ttu-id="ab3a1-3310">Uma variável de intervalo de expressão não é necessário para fazer referência a outra variável de intervalo, embora essa variável pode ser de valor duvidosa.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3310">An expression range variable is not required to reference another range variable, although such a variable may be of dubious value.</span></span> <span data-ttu-id="ab3a1-3311">A expressão atribuída a uma variável de intervalo da expressão deve ser classificada como um valor e deve ser implicitamente conversível para o tipo da variável de intervalo, se fornecido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3311">The expression assigned to an expression range variable must be classified as a value and must be implicitly convertible to the type of the range variable, if given.</span></span>

<span data-ttu-id="ab3a1-3312">Somente em um operador permitem uma variável de intervalo de expressão terá seu tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3312">Only in a Let operator may an expression range variable have its type specified.</span></span> <span data-ttu-id="ab3a1-3313">Em outros operadores, ou se seu tipo não for especificado, então a inferência de tipo de variável local é usada para determinar o tipo da variável de intervalo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3313">In other operators, or if its type is not specified, then local variable type inference is used to determine the type of the range variable.</span></span>

<span data-ttu-id="ab3a1-3314">Uma variável de intervalo deve seguir as regras para declarar variáveis locais em relação aos sombreamento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3314">A range variable must follow the rules for declaring local variables in respect to shadowing.</span></span> <span data-ttu-id="ab3a1-3315">Portanto, uma variável de intervalo não é possível ocultar o nome de uma variável local ou parâmetro no método delimitador ou outra variável de intervalo (a menos que o operador de consulta especificamente oculta todas as variáveis de alcance atuais no escopo).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3315">Thus, a range variable cannot hide the name of a local variable or parameter in the enclosing method or another range variable (unless the query operator specifically hides all current range variables in scope).</span></span>


### <a name="queryable-types"></a><span data-ttu-id="ab3a1-3316">Tipos passíveis de consulta</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3316">Queryable Types</span></span>

<span data-ttu-id="ab3a1-3317">Expressões de consulta são implementadas, convertendo a expressão em chamadas para métodos bem conhecidos em um tipo de coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3317">Query expressions are implemented by translating the expression into calls to well-known methods on a collection type.</span></span> <span data-ttu-id="ab3a1-3318">Esses métodos bem-definidos definem o tipo de elemento da coleção passível de consulta, bem como os tipos de resultado dos operadores de consulta executadas na coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3318">These well-defined methods define the element type of the queryable collection as well as the result types of query operators executed on the collection.</span></span> <span data-ttu-id="ab3a1-3319">Cada operador de consulta Especifica o método ou métodos que o operador de consulta é geralmente traduzido, embora a conversão específica é dependente de implementação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3319">Each query operator specifies the method or methods that the query operator is generally translated into, although the specific translation is implementation dependent.</span></span> <span data-ttu-id="ab3a1-3320">Os métodos são fornecidos na especificação usando um formato geral que se parece com:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3320">The methods are given in the specification using a general format that looks like:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="ab3a1-3321">O seguinte se aplica aos métodos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3321">The following applies to the methods:</span></span>

* <span data-ttu-id="ab3a1-3322">O método deve ser uma instância ou um membro de extensão do tipo de coleção e deve ser acessível.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3322">The method must be an instance or extension member of the collection type and must be accessible.</span></span>

* <span data-ttu-id="ab3a1-3323">O método pode ser genérico, desde que seja possível inferir a todos os argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3323">The method may be generic, provided that is possible to infer all type arguments.</span></span>

* <span data-ttu-id="ab3a1-3324">O método pode ser sobrecarregado, caso em que a resolução de sobrecarga é usada para determinar o método usar.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3324">The method may be overloaded, in which case overload resolution is used to determine the exactly method to use.</span></span>

* <span data-ttu-id="ab3a1-3325">Outro tipo de delegado pode ser usado no lugar de delegado `Func` de tipo, desde que ele tem a mesma assinatura, incluindo o tipo de retorno, como a correspondência `Func` tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3325">Another delegate type may be used in place of the delegate `Func` type, provided that it has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="ab3a1-3326">O tipo `System.Linq.Expressions.Expression(Of D)` pode ser usado no lugar de delegado `Func` tipo, desde que `D` é um tipo de delegado que tem a mesma assinatura, incluindo o tipo de retorno, como a correspondência `Func` tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3326">The type `System.Linq.Expressions.Expression(Of D)` may be used in place of the delegate `Func` type, provided that `D` is a delegate type that has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="ab3a1-3327">O tipo `T` representa o tipo de elemento da coleção de entrada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3327">The type `T` represents the element type of the input collection.</span></span> <span data-ttu-id="ab3a1-3328">Todos os métodos definidos por um tipo de coleção devem ter o mesmo tipo de elemento de entrada para o tipo de coleção a ser passível de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3328">All of the methods defined by a collection type must have the same input element type for the collection type to be queryable.</span></span>

* <span data-ttu-id="ab3a1-3329">O tipo `S` representa o tipo de elemento da segunda coleção de entrada, no caso de operadores de consulta que executar junções.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3329">The type `S` represents the element type of the second input collection in the case of query operators that perform joins.</span></span>

* <span data-ttu-id="ab3a1-3330">O tipo `K` representa um tipo de chave no caso de operadores de consulta que têm um conjunto de variáveis de intervalo que agem como chaves.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3330">The type `K` represents a key type in the case of query operators that have a set of range variables that act as keys.</span></span>

* <span data-ttu-id="ab3a1-3331">O tipo `N` representa um tipo que é usado como um tipo numérico (embora ele ainda poderia ser um tipo definido pelo usuário e não um tipo numérico intrínseco).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3331">The type `N` represents a type that is used as a numeric type (although it could still be a user-defined type and not an intrinsic numeric type).</span></span>

* <span data-ttu-id="ab3a1-3332">O tipo `B` representa um tipo que pode ser usado em uma expressão booleana.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3332">The type `B` represents a type that can be used in a Boolean expression.</span></span>

* <span data-ttu-id="ab3a1-3333">O tipo `R` representa o tipo de elemento da coleção de resultado, se o operador de consulta produz um conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3333">The type `R` represents the element type of the result collection, if the query operator produces a result collection.</span></span> <span data-ttu-id="ab3a1-3334">`R` depende do número de variáveis de intervalo no escopo na conclusão do operador de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3334">`R` depends on the number of range variables in scope at the conclusion of the query operator.</span></span> <span data-ttu-id="ab3a1-3335">Se uma variável de intervalo único está no escopo, em seguida, `R` é o tipo dessa variável de intervalo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3335">If a single range variable is in scope, then `R` is the type of that range variable.</span></span> <span data-ttu-id="ab3a1-3336">No exemplo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3336">In the example</span></span>

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  <span data-ttu-id="ab3a1-3337">o resultado da consulta será um tipo de coleção com um tipo de elemento `String`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3337">the result of the query will be a collection type with an element type of `String`.</span></span> <span data-ttu-id="ab3a1-3338">Se diversas variáveis de intervalo estão no escopo, então `R` é um tipo anônimo que contém todas as variáveis de intervalo no escopo de `Key` campos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3338">If multiple range variables are in scope, then `R` is an anonymous type that contains all of the range variables in scope as `Key` fields.</span></span> <span data-ttu-id="ab3a1-3339">No exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3339">In the example:</span></span>

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  <span data-ttu-id="ab3a1-3340">o resultado da consulta será um tipo de coleção com um tipo de elemento de um tipo anônimo com uma propriedade somente leitura denominada `Name` do tipo `String` e uma propriedade somente leitura chamada `ProductName` do tipo `String`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3340">the result of the query will be a collection type with an element type of an anonymous type with a read-only property named `Name` of type `String` and a read-only property named `ProductName` of type `String`.</span></span>

  <span data-ttu-id="ab3a1-3341">Dentro de uma expressão de consulta, são gerados para conter variáveis de intervalo de tipos anônimos *transparente*, que significa que variáveis de alcance sempre está disponíveis sem qualificação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3341">Within a query expression, anonymous types generated to contain range variables are *transparent*, which means that range variables are always available without qualification.</span></span> <span data-ttu-id="ab3a1-3342">Por exemplo, no exemplo anterior as variáveis de intervalo `c` e `o` podem ser acessados sem qualificação no `Select` consultar operador, mesmo que o tipo de elemento da coleção de entrada foi um tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3342">For example, in the previous example the range variables `c` and `o` could be accessed without qualification in the `Select` query operator, even though the input collection's element type was an anonymous type.</span></span>

* <span data-ttu-id="ab3a1-3343">O tipo `CX` representa um tipo de coleção, não necessariamente o tipo de coleção de entrada, cujo tipo de elemento é algum tipo `X`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3343">The type `CX` represents a collection type, not necessarily the input collection type, whose element type is some type `X`.</span></span>

<span data-ttu-id="ab3a1-3344">Um tipo de coleção passível de consulta deve atender a uma das condições a seguir, em ordem de preferência:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3344">A queryable collection type must satisfy one of the following conditions, in order of preference:</span></span>

* <span data-ttu-id="ab3a1-3345">Ele deve definir uma em conformidade com `Select` método.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3345">It must define a conforming `Select` method.</span></span>

* <span data-ttu-id="ab3a1-3346">Ele deve ter um dos métodos a seguir</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3346">It must have have one of the following methods</span></span>

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  <span data-ttu-id="ab3a1-3347">que pode ser chamado para obter uma coleção passível de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3347">which can be called to obtain a queryable collection.</span></span> <span data-ttu-id="ab3a1-3348">Se ambos os métodos são fornecidos, `AsQueryable` é preferível `AsEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3348">If both methods are provided, `AsQueryable` is preferred over `AsEnumerable`.</span></span>

* <span data-ttu-id="ab3a1-3349">Ele deve ter um método</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3349">It must have a method</span></span>

  ```vb
  Function Cast(Of T)() As CT
  ```

  <span data-ttu-id="ab3a1-3350">que pode ser chamado com o tipo da variável de intervalo para produzir uma coleção passível de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3350">which can be called with the type of the range variable to produce a queryable collection.</span></span>

<span data-ttu-id="ab3a1-3351">Como determinar o tipo de elemento de uma coleção ocorre independentemente de uma invocação de método real, não é possível determinar a aplicabilidade de métodos específicos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3351">Because determining the element type of a collection occurs independently of an actual method invocation, the applicability of specific methods cannot be determined.</span></span> <span data-ttu-id="ab3a1-3352">Portanto, ao determinar o tipo de elemento de uma coleção, se houver métodos de instância que correspondem aos métodos bem conhecidos, quaisquer métodos de extensão que correspondem aos métodos bem conhecidos são ignorados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3352">Thus, when determining the element type of a collection if there are instance methods that match well-known methods, then any extension methods that match well-known methods are ignored.</span></span>

<span data-ttu-id="ab3a1-3353">Conversão de operador de consulta ocorre na ordem em que os operadores de consulta ocorrem na expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3353">Query operator translation occurs in the order in which the query operators occur in the expression.</span></span> <span data-ttu-id="ab3a1-3354">Não é necessário para um objeto de coleção implementar todos os métodos necessários para todos os operadores de consulta, embora cada objeto da coleção deve suportar pelo menos o `Select` operador de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3354">It is not necessary for a collection object to implement all of the methods needed by all the query operators, although every collection object must at least support the `Select` query operator.</span></span> <span data-ttu-id="ab3a1-3355">Se um método necessário não estiver presente, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3355">If a needed method is not present, a compile-time error occurs.</span></span> <span data-ttu-id="ab3a1-3356">Ao associar nomes de método conhecido, não métodos são ignorados para fins de herança múltipla em interfaces e o método de extensão de associação, embora a semântica de sombreamento ainda se aplicam.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3356">When binding well-known method names, non-methods are ignored for the purpose of multiple inheritance in interfaces and extension method binding, although shadowing semantics still apply.</span></span> <span data-ttu-id="ab3a1-3357">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3357">For example:</span></span>

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

### <a name="default-query-indexer"></a><span data-ttu-id="ab3a1-3358">Indexador de consulta padrão</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3358">Default Query Indexer</span></span>

<span data-ttu-id="ab3a1-3359">Cada tipo de coleção consultável cujo tipo de elemento é `T` e ainda não tiver um padrão propriedade é considerada como tendo uma propriedade padrão no seguinte formato geral:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3359">Every queryable collection type whose element type is `T` and does not already have a default property is considered to have a default property of the following general form:</span></span>

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

<span data-ttu-id="ab3a1-3360">A propriedade padrão só pode ser referida usando a sintaxe de acesso de propriedade padrão; a propriedade padrão não pode ser referenciada por nome.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3360">The default property can only be referred to using the default property access syntax; the default property cannot be referred to by name.</span></span> <span data-ttu-id="ab3a1-3361">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3361">For example:</span></span>

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

<span data-ttu-id="ab3a1-3362">Se o tipo de coleção não tem um `ElementAtOrDefault` membro, um erro de tempo de compilação ocorrerá.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3362">If the collection type does not have an `ElementAtOrDefault` member, a compile-time error will occur.</span></span>

### <a name="from-query-operator"></a><span data-ttu-id="ab3a1-3363">Operador de consulta</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3363">From Query Operator</span></span>

<span data-ttu-id="ab3a1-3364">O `From` consulta operador apresenta uma variável de intervalo da coleção que representa os membros individuais de uma coleção a ser consultado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3364">The `From` query operator introduces a collection range variable that represents the individual members of a collection to be queried.</span></span>

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="ab3a1-3365">Por exemplo, a expressão de consulta:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3365">For example, the query expression:</span></span>

```vb
From c As Customer In Customers ...
```

<span data-ttu-id="ab3a1-3366">pode ser pensada como equivalente a</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3366">can be thought of as equivalent to</span></span>

```vb
For Each c As Customer In Customers
        ...
Next c
```

<span data-ttu-id="ab3a1-3367">Quando um `From` operador de consulta declara coleta várias variáveis de intervalo ou não é a primeira `From` é do operador de consulta na expressão de consulta, cada nova variável de intervalo de coleta *cruzada Unido* ao conjunto existente de variáveis de intervalo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3367">When a `From` query operator declares multiple collection range variables or is not the first `From` query operator in the query expression, each new collection range variable is *cross joined* to the existing set of range variables.</span></span> <span data-ttu-id="ab3a1-3368">O resultado é que a consulta é avaliada sobre o produto cruzado de todos os elementos nas coleções associadas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3368">The result is that the query is evaluated over the cross-product of all the elements in the joined collections.</span></span> <span data-ttu-id="ab3a1-3369">Por exemplo, a expressão:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3369">For example, the expression:</span></span>

```vb
From c In Customers _
From e In Employees _
...
```

<span data-ttu-id="ab3a1-3370">pode ser pensada como equivalente à:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3370">can be thought of as equivalent to:</span></span>

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

<span data-ttu-id="ab3a1-3371">e é exatamente equivalente a:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3371">and is exactly equivalent to:</span></span>

```vb
From c In Customers, e In Employees ...
```

<span data-ttu-id="ab3a1-3372">As variáveis de intervalo introduzidas nos operadores de consulta anterior podem ser usadas em uma posterior `From` operador de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3372">The range variables introduced in previous query operators can be used in a later `From` query operator.</span></span> <span data-ttu-id="ab3a1-3373">Por exemplo, na expressão de consulta a seguir o segundo `From` operador de consulta se refere ao valor da variável de intervalo primeiro:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3373">For example, in the following query expression the second `From` query operator refers to the value of the first range variable:</span></span>

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

<span data-ttu-id="ab3a1-3374">Diversas variáveis de intervalo em um `From` múltiplos ou operador de consulta `From` operadores de consulta são suportados apenas se o tipo de coleção contém uma ou mais dos seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3374">Multiple range variables in a `From` query operator or multiple `From` query operators are only supported if the collection type contains one or both of the following methods:</span></span>

```vb
Function SelectMany(selector As Func(Of T, CR)) As CR
Function SelectMany(selector As Func(Of T, CS), _
                          resultsSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="ab3a1-3375">O código</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3375">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs, y In ys ...
```

<span data-ttu-id="ab3a1-3376">é geralmente traduzido para</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3376">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

<span data-ttu-id="ab3a1-3377">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3377">__Note.__</span></span> <span data-ttu-id="ab3a1-3378">`From` não é uma palavra reservada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3378">`From` is not a reserved word.</span></span>


### <a name="join-query-operator"></a><span data-ttu-id="ab3a1-3379">Operador de consulta JOIN</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3379">Join Query Operator</span></span>

<span data-ttu-id="ab3a1-3380">O `Join` consulta operador une as variáveis de intervalo existentes com uma nova variável de intervalo de coleta, produzindo uma única coleção cujos elementos foram Unidos juntos com base em uma expressão de igualdade.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3380">The `Join` query operator joins existing range variables with a new collection range variable, producing a single collection whose elements have been joined together based on an equality expression.</span></span>

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

<span data-ttu-id="ab3a1-3381">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3381">For example:</span></span>

```vb
Dim customersAndOrders = _
    From cust In Customers _
    Join ord In Orders On cust.ID Equals ord.CustomerID
```

<span data-ttu-id="ab3a1-3382">A expressão de igualdade é mais restrita do que uma expressão regular de igualdade:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3382">The equality expression is more restricted than a regular equality expression:</span></span>

* <span data-ttu-id="ab3a1-3383">As duas expressões devem ser classificadas como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3383">Both expressions must be classified as a value.</span></span>

* <span data-ttu-id="ab3a1-3384">As duas expressões devem fazer referência a pelo menos uma variável de intervalo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3384">Both expressions must reference at least one range variable.</span></span>

* <span data-ttu-id="ab3a1-3385">A variável de intervalo declarado na junção operador de consulta deve ser referenciada por uma das expressões, e que a expressão não deve fazer referência qualquer outra variável de intervalo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3385">The range variable declared in the join query operator must be referenced by one of the expressions, and that expression must not reference any other range variables.</span></span>

<span data-ttu-id="ab3a1-3386">Se os tipos das duas expressões não são exatamente do mesmo tipo, em seguida</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3386">If the types of the two expressions are not the exact same type, then</span></span>

* <span data-ttu-id="ab3a1-3387">Se o operador de igualdade é definido para os dois tipos, as duas expressões são implicitamente conversíveis para ele e não é `Object`, em seguida, converter as duas expressões para aquele tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3387">If the equality operator is defined for the two types, both expressions are implicitly convertible to it, and it is not `Object`, then convert both expressions to that type.</span></span>

* <span data-ttu-id="ab3a1-3388">Caso contrário, se houver um tipo dominante que ambas as expressões podem ser convertidas implicitamente em, converta as duas expressões para aquele tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3388">Otherwise, if there is a dominant type that both expressions can be implicitly converted to, then convert both expressions to that type.</span></span>

* <span data-ttu-id="ab3a1-3389">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3389">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="ab3a1-3390">As expressões são comparadas usando valores de hash (ou seja, chamando `GetHashCode()`) em vez de usar operadores de igualdade para maior eficiência.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3390">The expressions are compared using hash values (i.e. by calling `GetHashCode()`) rather than by using equality operators for efficiency.</span></span> <span data-ttu-id="ab3a1-3391">Um `Join` operador de consulta pode fazer várias junções ou condições de igualdade no mesmo operador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3391">A `Join` query operator may do multiple joins or equality conditions in the same operator.</span></span> <span data-ttu-id="ab3a1-3392">Um `Join` operador de consulta é suportado apenas se o tipo de coleção contém um método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3392">A `Join` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Join(inner As CS, _
                  outerSelector As Func(Of T, K), _
                  innerSelector As Func(Of S, K), _
                  resultSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="ab3a1-3393">O código</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3393">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Join y In ys On x Equals y _
            ...
```

<span data-ttu-id="ab3a1-3394">é geralmente traduzido para</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3394">is generally translated to</span></span>

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

<span data-ttu-id="ab3a1-3395">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3395">__Note.__</span></span> <span data-ttu-id="ab3a1-3396">`Join`, `On` e `Equals` não são palavras reservadas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3396">`Join`, `On` and `Equals` are not reserved words.</span></span>


### <a name="let-query-operator"></a><span data-ttu-id="ab3a1-3397">Permitir que o operador de consulta</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3397">Let Query Operator</span></span>

<span data-ttu-id="ab3a1-3398">O `Let` operador apresenta uma variável de intervalo de expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3398">The `Let` query operator introduces an expression range variable.</span></span> <span data-ttu-id="ab3a1-3399">Isso permite calcular um valor intermediário depois que será usado várias vezes nos operadores de consulta posteriores.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3399">This allows calculating an intermediate value once that will be used multiple times in later query operators.</span></span>

```antlr
LetQueryOperator
    : LineTerminator? 'Let' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="ab3a1-3400">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3400">For example:</span></span>

```vb
Dim taxedPrices = _
    From o In Orders _
    Let tax = o.Price * 0.088 _
    Where tax > 3.50 _
    Select o.Price, tax, total = o.Price + tax
```

<span data-ttu-id="ab3a1-3401">pode ser pensada como equivalente à:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3401">can be thought of as equivalent to:</span></span>

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

<span data-ttu-id="ab3a1-3402">Um `Let` operador de consulta é suportado apenas se o tipo de coleção contém um método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3402">A `Let` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="ab3a1-3403">O código</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3403">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Let y = x * 10 _
            ...
```

<span data-ttu-id="ab3a1-3404">é geralmente traduzido para</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3404">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a><span data-ttu-id="ab3a1-3405">Selecione o operador de consulta</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3405">Select Query Operator</span></span>

<span data-ttu-id="ab3a1-3406">O `Select` operador de consulta é como o `Let` operador de consulta em que ele apresenta as variáveis de intervalo de expressão; no entanto, um `Select` consulta operador oculta as variáveis de intervalo disponível no momento, em vez de adicioná-los.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3406">The `Select` query operator is like the `Let` query operator in that it introduces expression range variables; however, a `Select` query operator hides the currently available range variables instead of adding to them.</span></span> <span data-ttu-id="ab3a1-3407">Além disso, o tipo de uma variável de intervalo de expressão introduzido por um `Select` operador de consulta é sempre inferido usando regras de inferência de tipo de variável local; um tipo explícito não pode ser especificado e se nenhum tipo pode ser inferido, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3407">Also, the type of an expression range variable introduced by a `Select` query operator is always inferred using local variable type inference rules; an explicit type cannot be specified, and if no type can be inferred, a compile-time error occurs.</span></span>

```antlr
SelectQueryOperator
    : LineTerminator? 'Select' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="ab3a1-3408">Por exemplo, na consulta:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3408">For example, in the query:</span></span>

```vb
Dim smiths = _
    From cust In Customers _
    Select name = cust.name _
    Where name.EndsWith("Smith")
```

<span data-ttu-id="ab3a1-3409">o `Where` operador de consulta tem acesso apenas à `name` variável de intervalo introduzido pelo `Select` operador; se o `Where` operador tentasse se referir `cust`, teria ocorrido um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3409">the `Where` query operator only has access to the `name` range variable introduced by the `Select` operator; if the `Where` operator had tried to reference `cust`, a compile-time error would have occurred.</span></span>

<span data-ttu-id="ab3a1-3410">Em vez de especificar explicitamente os nomes das variáveis de intervalo, um `Select` consulta operador pode inferir os nomes das variáveis de intervalo, usando as mesmas regras como expressões de criação de objeto do tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3410">Instead of explicitly specifying the names of the range variables, a `Select` query operator can infer the names of the range variables, using the same rules as anonymous type object creation expressions.</span></span> <span data-ttu-id="ab3a1-3411">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3411">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

<span data-ttu-id="ab3a1-3412">Se o nome da variável de intervalo não for fornecido e um nome não pode ser inferido, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3412">If the name of the range variable is not supplied and a name cannot be inferred, a compile-time error occurs.</span></span> <span data-ttu-id="ab3a1-3413">Se o `Select` operador de consulta contém apenas uma única expressão, nenhum erro ocorrerá se um nome para essa variável de intervalo não pode ser inferido, mas a variável de intervalo é sem nome.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3413">If the `Select` query operator contains only a single expression, no error occurs if a name for that range variable cannot be inferred but the range variable is nameless.</span></span>  <span data-ttu-id="ab3a1-3414">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3414">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

<span data-ttu-id="ab3a1-3415">Se não houver uma ambiguidade em um `Select` operador de consulta entre atribuir um nome a uma variável de intervalo e uma expressão de igualdade, a atribuição de nome é preferencial.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3415">If there is an ambiguity in a `Select` query operator between assigning a name to a range variable and an equality expression, the name assignment is preferred.</span></span> <span data-ttu-id="ab3a1-3416">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3416">For example:</span></span>

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

<span data-ttu-id="ab3a1-3417">Cada expressão no `Select` operador de consulta deve ser classificada como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3417">Each expression in the `Select` query operator must be classified as a value.</span></span> <span data-ttu-id="ab3a1-3418">Um `Select` operador de consulta tem suporte apenas se o tipo de coleção contém um método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3418">A `Select` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="ab3a1-3419">O código</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3419">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Select x, y = x * 10 _
            ...
```

<span data-ttu-id="ab3a1-3420">é geralmente traduzido para</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3420">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a><span data-ttu-id="ab3a1-3421">Operador de consulta DISTINCT</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3421">Distinct Query Operator</span></span>

<span data-ttu-id="ab3a1-3422">O `Distinct` consulta operador restringe os valores em uma coleção somente para aqueles com valores distintos, conforme determinado comparando o tipo de elemento quanto à igualdade.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3422">The `Distinct` query operator restricts the values in a collection only to those with distinct values, as determined by comparing the element type for equality.</span></span>

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

<span data-ttu-id="ab3a1-3423">Por exemplo, a consulta:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3423">For example, the query:</span></span>

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

<span data-ttu-id="ab3a1-3424">retornará apenas uma linha para cada emparelhamento distintos de preço de nome e a ordem do cliente, mesmo se o cliente tiver vários pedidos com o mesmo preço.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3424">will only return one row for each distinct pairing of customer name and order price, even if the customer has multiple orders with the same price.</span></span> <span data-ttu-id="ab3a1-3425">Um `Distinct` operador de consulta tem suporte apenas se o tipo de coleção contém um método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3425">A `Distinct` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Distinct() As CT
```

<span data-ttu-id="ab3a1-3426">O código</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3426">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Distinct _
            ...
```

<span data-ttu-id="ab3a1-3427">é geralmente traduzido para</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3427">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

<span data-ttu-id="ab3a1-3428">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3428">__Note.__</span></span> <span data-ttu-id="ab3a1-3429">`Distinct` não é uma palavra reservada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3429">`Distinct` is not a reserved word.</span></span>


### <a name="where-query-operator"></a><span data-ttu-id="ab3a1-3430">Em que o operador de consulta</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3430">Where Query Operator</span></span>

<span data-ttu-id="ab3a1-3431">O `Where` operador restringe os valores em uma coleção para aqueles que atendem a uma determinada condição de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3431">The `Where` query operator restricts the values in a collection to those that satisfy a given condition.</span></span>

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="ab3a1-3432">Um `Where` operador de consulta usa uma expressão booliana que é avaliada para cada conjunto de valores de variável de intervalo; se o valor da expressão for true, os valores aparecem na coleção de saída, caso contrário, os valores são ignorados.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3432">A `Where` query operator takes a Boolean expression that is evaluated for each set of range variable values; if the value of the expression is true, then the values appear in the output collection, otherwise the values are skipped.</span></span> <span data-ttu-id="ab3a1-3433">Por exemplo, a expressão de consulta:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3433">For example, the query expression:</span></span>

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

<span data-ttu-id="ab3a1-3434">pode ser pensada como equivalente para o loop aninhado</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3434">can be thought of as equivalent to the nested loop</span></span>

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

<span data-ttu-id="ab3a1-3435">Um `Where` operador de consulta tem suporte apenas se o tipo de coleção contém um método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3435">A `Where` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Where(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="ab3a1-3436">O código</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3436">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Where x < 10 _
            ...
```

<span data-ttu-id="ab3a1-3437">é geralmente traduzido para</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3437">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

<span data-ttu-id="ab3a1-3438">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3438">__Note.__</span></span> <span data-ttu-id="ab3a1-3439">`Where` não é uma palavra reservada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3439">`Where` is not a reserved word.</span></span>


### <a name="partition-query-operators"></a><span data-ttu-id="ab3a1-3440">Operadores de consulta de partição</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3440">Partition Query Operators</span></span>

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="ab3a1-3441">O `Take` consultar resultados do operador no primeiro `n` elementos de uma coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3441">The `Take` query operator results in the first `n` elements of a collection.</span></span> <span data-ttu-id="ab3a1-3442">Quando usado com o `While` modificador, o `Take` resultados do operador no primeiro `n` elementos de uma coleção que atendem uma expressão booliana.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3442">When used with the `While` modifier, the `Take` operator results in the first `n` elements of a collection that satisfy a Boolean expression.</span></span> <span data-ttu-id="ab3a1-3443">O `Skip` operador ignora os primeiros `n` elementos de uma coleção e, em seguida, retorna o resto da coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3443">The `Skip` operator skips the first `n` elements of a collection and then returns the remainder of the collection.</span></span>  <span data-ttu-id="ab3a1-3444">Quando usado em conjunto com o `While` modificador, o `Skip` operador ignora os primeiros `n` elementos de uma coleção que atendem a uma expressão booliana e, em seguida, retorna o restante da coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3444">When used in conjunction with the `While` modifier, the `Skip` operator skips the first `n` elements of a collection that satisfy a Boolean expression and then returns the rest of the collection.</span></span> <span data-ttu-id="ab3a1-3445">As expressões em uma `Take` ou `Skip` operador de consulta deve ser classificada como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3445">The expressions in a `Take` or `Skip` query operator must be classified as a value.</span></span>

<span data-ttu-id="ab3a1-3446">Um `Take` operador de consulta tem suporte apenas se o tipo de coleção contém um método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3446">A `Take` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Take(count As N) As CT
```

<span data-ttu-id="ab3a1-3447">Um `Skip` operador de consulta tem suporte apenas se o tipo de coleção contém um método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3447">A `Skip` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Skip(count As N) As CT
```

<span data-ttu-id="ab3a1-3448">Um `Take While` operador de consulta tem suporte apenas se o tipo de coleção contém um método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3448">A `Take While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="ab3a1-3449">Um `Skip While` operador de consulta tem suporte apenas se o tipo de coleção contém um método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3449">A `Skip While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function SkipWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="ab3a1-3450">O código</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3450">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Skip 10 _
            Take 5 _
            Skip While x < 10 _
            Take While x > 5 _
            ...
```

<span data-ttu-id="ab3a1-3451">é geralmente traduzido para</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3451">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

<span data-ttu-id="ab3a1-3452">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3452">__Note.__</span></span> <span data-ttu-id="ab3a1-3453">`Take` e `Skip` não são palavras reservadas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3453">`Take` and `Skip` are not reserved words.</span></span>


### <a name="order-by-query-operator"></a><span data-ttu-id="ab3a1-3454">Ordem pelo operador de consulta</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3454">Order By Query Operator</span></span>

<span data-ttu-id="ab3a1-3455">O `Order By` operador de consulta classifica os valores que aparecem nas variáveis de intervalo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3455">The `Order By` query operator orders the values that appear in the range variables.</span></span> 

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

<span data-ttu-id="ab3a1-3456">Um `Order By` operador de consulta usa expressões que especificam os valores de chave que devem ser usados para ordenar as variáveis de iteração.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3456">An `Order By` query operator takes expressions that specify the key values that should be used to order the iteration variables.</span></span> <span data-ttu-id="ab3a1-3457">Por exemplo, a consulta a seguir retorna produtos classificados por preço:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3457">For example, the following query returns products sorted by price:</span></span>

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

<span data-ttu-id="ab3a1-3458">Uma ordenação pode ser marcada como `Ascending`, caso em que valores menores vêm antes de valores maiores, ou `Descending`, caso em que os valores maiores vêm antes valores menores.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3458">An ordering can be marked as `Ascending`, in which case smaller values come before larger values, or `Descending`, in which case larger values come before smaller values.</span></span> <span data-ttu-id="ab3a1-3459">O padrão se nenhuma for especificada uma ordenação é `Ascending`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3459">The default for an ordering if none is specified is `Ascending`.</span></span> <span data-ttu-id="ab3a1-3460">Por exemplo, a consulta a seguir retorna os produtos classificados pelo preço com o produto mais caro primeiro:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3460">For example, the following query returns products sorted by price with the most expensive product first:</span></span>

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

<span data-ttu-id="ab3a1-3461">O `Order By` consulta operador pode especificar várias expressões de ordenação, caso em que a coleção é ordenada de forma aninhada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3461">The `Order By` query operator may specify multiple expressions for ordering, in which case the collection is ordered in a nested manner.</span></span> <span data-ttu-id="ab3a1-3462">Por exemplo, a consulta a seguir ordena os clientes por estado, em seguida, por cidade dentro de cada estado e, em seguida, por CEP em cada cidade:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3462">For example, the following query orders customers by state, then by city within each state and then by ZIP code within each city:</span></span>

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

<span data-ttu-id="ab3a1-3463">As expressões em um `Order By` operador de consulta deve ser classificada como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3463">The expressions in an `Order By` query operator must be classified as a value.</span></span> <span data-ttu-id="ab3a1-3464">Um `Order By` operador de consulta tem suporte apenas se o tipo de coleção contém uma ou mais dos seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3464">An `Order By` query operator is supported only if the collection type contains one or both of the following methods:</span></span>

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="ab3a1-3465">O tipo de retorno `CT` deve ser um *coleção ordenada*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3465">The return type `CT` must be an *ordered collection*.</span></span> <span data-ttu-id="ab3a1-3466">Uma coleção ordenada é um tipo de coleção que contém um ou ambos os métodos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3466">An ordered collection is a collection type that contains one or both of the methods:</span></span>

```vb
Function ThenBy(keySelector As Func(Of T, K)) As CT
Function ThenByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="ab3a1-3467">O código</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3467">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Order By x Ascending, x Mod 2 Descending _
            ...
```

<span data-ttu-id="ab3a1-3468">é geralmente traduzido para</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3468">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

<span data-ttu-id="ab3a1-3469">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3469">__Note.__</span></span> <span data-ttu-id="ab3a1-3470">Porque os operadores de consulta simplesmente mapeiam sintaxe para os métodos que implementam uma operação de consulta específica, a preservação da ordem não é determinada pelo idioma e é determinada pela implementação do próprio operador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3470">Because query operators simply map syntax to methods that implement a particular query operation, order preservation is not dictated by the language and is determined by the implementation of the operator itself.</span></span> <span data-ttu-id="ab3a1-3471">Isso é muito semelhante aos operadores definidos pelo usuário em que a implementação para sobrecarregar o operador de adição para um tipo numérico definido pelo usuário não pode executar qualquer coisa que se assemelha a uma adição.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3471">This is very similar to user-defined operators in that the implementation to overload the addition operator for a user-defined numeric type may not perform anything resembling an addition.</span></span> <span data-ttu-id="ab3a1-3472">É claro que, para preservar a previsibilidade, implementar algo que não coincide com as expectativas dos usuários não é recomendado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3472">Of course, to preserve predictability, implementing something that does not match user expectations is not recommended.</span></span>

<span data-ttu-id="ab3a1-3473">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3473">__Note.__</span></span> <span data-ttu-id="ab3a1-3474">`Order` e `By` não são palavras reservadas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3474">`Order` and `By` are not reserved words.</span></span>


### <a name="group-by-query-operator"></a><span data-ttu-id="ab3a1-3475">Agrupar por operador de consulta</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3475">Group By Query Operator</span></span>

<span data-ttu-id="ab3a1-3476">O `Group By` as variáveis de intervalo no escopo com base em uma ou mais expressões de grupos de operador de consulta e, em seguida, gera novas variáveis de alcance com base em um desses agrupamentos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3476">The `Group By` query operator groups the range variables in scope based on one or more expressions, and then produces new range variables based on those groupings.</span></span>

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="ab3a1-3477">Por exemplo, a consulta a seguir agrupa todos os clientes por `State`e, em seguida, calcula a idade de contagem e média de cada grupo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3477">For example, the following query groups all customers by `State`, and then computes the count and average age of each group:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

<span data-ttu-id="ab3a1-3478">O `Group By` operador tem três cláusulas de consulta: opcional `Group` cláusula, a `By` cláusula e o `Into` cláusula.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3478">The `Group By` query operator has three clauses: the optional `Group` clause, the `By` clause, and the `Into` clause.</span></span> <span data-ttu-id="ab3a1-3479">O `Group` cláusula tem a mesma sintaxe e o efeito de uma `Select` consultar operador, exceto que ele afeta somente as variáveis de intervalo disponíveis na `Into` cláusula e não o `By` cláusula.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3479">The `Group` clause has the same syntax and effect as a `Select` query operator, except that it only affects the range variables available in the `Into` clause and not the `By` clause.</span></span> <span data-ttu-id="ab3a1-3480">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3480">For example:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

<span data-ttu-id="ab3a1-3481">O `By` cláusula declara as variáveis de intervalo são usadas como valores de chave na operação de agrupamento de expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3481">The `By` clause declares expression range variables that are used as key values in the grouping operation.</span></span> <span data-ttu-id="ab3a1-3482">O `Into` cláusula permite a declaração da expressão de variáveis de intervalo que calculam agregações em cada um dos grupos formados pelo `By` cláusula.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3482">The `Into` clause allows the declaration of expression range variables that calculate aggregations over each of the groups formed by the `By` clause.</span></span> <span data-ttu-id="ab3a1-3483">Dentro de `Into` cláusula, a variável de intervalo de expressão pode ser atribuída somente uma expressão que é uma invocação de método de um *função de agregação*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3483">Within the `Into` clause, the expression range variable can only be assigned an expression which is a method invocation of an *aggregate function*.</span></span> <span data-ttu-id="ab3a1-3484">Uma função de agregação é uma função no tipo de coleção do grupo (que pode não ser necessariamente o mesmo tipo de coleção da coleção original), que se parece com qualquer um dos seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3484">An aggregate function is a function on the collection type of the group (which may not necessarily be the same collection type of the original collection) which looks like either of the following methods:</span></span>

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

<span data-ttu-id="ab3a1-3485">Se uma função de agregação usa um argumento do delegado, a expressão de invocação pode ter uma expressão de argumento que deve ser classificada como um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3485">If an aggregate function takes a delegate argument, then the invocation expression can have an argument expression that must be classified as a value.</span></span>  <span data-ttu-id="ab3a1-3486">A expressão de argumento pode usar as variáveis de intervalo que estão no escopo. dentro da chamada para uma função de agregação, essas variáveis de intervalo representam os valores no grupo sendo formada, nem todos os valores na coleção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3486">The argument expression can use the range variables that are in scope; within the call to an aggregate function, those range variables represent the values in the group being formed, not all of the values in the collection.</span></span> <span data-ttu-id="ab3a1-3487">Por exemplo, no exemplo original nesta seção o `Average` função calcula a média de idades de clientes por estado, em vez de todos os clientes juntos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3487">For example, in the original example in this section the `Average` function calculates the average of the customers' ages per state rather than for all of the customers together.</span></span>

<span data-ttu-id="ab3a1-3488">Todos os tipos de coleção são considerados como tendo a função de agregação `Group` definidos nele, que não usa nenhum parâmetro e simplesmente retorna o grupo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3488">All collection types are considered to have the aggregate function `Group` defined on it, which takes no parameters and simply returns the group.</span></span> <span data-ttu-id="ab3a1-3489">Outras funções de agregação padrão que pode fornecer a um tipo de coleção são:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3489">Other standard aggregate functions that a collection type may provide are:</span></span>

<span data-ttu-id="ab3a1-3490">`Count` e `LongCount`, qual retornar a contagem de elementos no grupo ou a contagem dos elementos no grupo que atendem uma expressão booliana.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3490">`Count` and `LongCount`, which return the count of the elements in the group or the count of the elements in the group that satisfy a Boolean expression.</span></span> <span data-ttu-id="ab3a1-3491">`Count` e `LongCount` são suportados apenas se o tipo de coleção contém um dos métodos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3491">`Count` and `LongCount` are supported only if the collection type contains one of the methods:</span></span>

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

<span data-ttu-id="ab3a1-3492">`Sum`, que retorna a soma de uma expressão em todos os elementos no grupo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3492">`Sum`, which returns the sum of an expression across all the elements in the group.</span></span> <span data-ttu-id="ab3a1-3493">`Sum` há suporte para apenas se o tipo de coleção contiver um dos métodos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3493">`Sum` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

<span data-ttu-id="ab3a1-3494">`Min` Retorna o valor mínimo de uma expressão em todos os elementos no grupo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3494">`Min` which returns the minimum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="ab3a1-3495">`Min` há suporte para apenas se o tipo de coleção contiver um dos métodos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3495">`Min` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

<span data-ttu-id="ab3a1-3496">`Max`, que retorna o valor máximo de uma expressão em todos os elementos no grupo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3496">`Max`, which returns the maximum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="ab3a1-3497">`Max` há suporte para apenas se o tipo de coleção contiver um dos métodos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3497">`Max` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

<span data-ttu-id="ab3a1-3498">`Average`, que retorna a média de uma expressão em todos os elementos no grupo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3498">`Average`, which returns the average of an expression across all the elements in the group.</span></span> <span data-ttu-id="ab3a1-3499">`Average` há suporte para apenas se o tipo de coleção contiver um dos métodos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3499">`Average` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

<span data-ttu-id="ab3a1-3500">`Any`, que determina se um grupo contém membros ou se uma expressão booliana for verdadeira para qualquer elemento no grupo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3500">`Any`, which determines whether a group contains members or if a Boolean expression is true for any element in the group.</span></span> <span data-ttu-id="ab3a1-3501">`Any` Retorna um valor que pode ser usado em uma expressão booleana e tem suporte apenas se o tipo de coleção contém um dos métodos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3501">`Any` returns a value that can be used in a Boolean expression and is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="ab3a1-3502">`All`, que determina se uma expressão booleana é verdadeira para todos os elementos no grupo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3502">`All`, which determines whether a Boolean expression is true for all elements in the group.</span></span> <span data-ttu-id="ab3a1-3503">`All` Retorna um valor que pode ser usado em uma expressão booleana e tem suporte apenas se o tipo de coleção contém um método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3503">`All` returns a value that can be used in a Boolean expression and is supported only if the collection type contains a method:</span></span>

```vb
Function All(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="ab3a1-3504">Depois de um `Group By` operador de consulta, as variáveis de intervalo anteriormente no escopo são ocultadas, e as variáveis de intervalo introduzidos pelo `By` e `Into` cláusulas estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3504">After a `Group By` query operator, the range variables previously in scope are hidden, and the range variables introduced by the `By` and `Into` clauses are available.</span></span> <span data-ttu-id="ab3a1-3505">Um `Group By` operador de consulta tem suporte apenas se o tipo de coleção contém o método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3505">A `Group By` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

<span data-ttu-id="ab3a1-3506">Em declarações de variável de intervalo de `Group` cláusula são suportados apenas se o tipo de coleção contém o método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3506">Range variable declarations in the `Group` clause are supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      elementSelector As Func(Of T, S), _
                      resultSelector As Func(Of K, CS, R)) As CR
```

<span data-ttu-id="ab3a1-3507">O código</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3507">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Group y = x * 10, z = x / 10 By evenOdd = x Mod 2 _
            Into Sum(y), Average(z) _
            ...
```

<span data-ttu-id="ab3a1-3508">é geralmente traduzido para</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3508">is generally translated to</span></span>

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

<span data-ttu-id="ab3a1-3509">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3509">__Note.__</span></span> <span data-ttu-id="ab3a1-3510">`Group`, `By`, e `Into` não são palavras reservadas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3510">`Group`, `By`, and `Into` are not reserved words.</span></span>


### <a name="aggregate-query-operator"></a><span data-ttu-id="ab3a1-3511">Operador de consulta de agregação</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3511">Aggregate Query Operator</span></span>

<span data-ttu-id="ab3a1-3512">O `Aggregate` operador de consulta executa uma função semelhante como o `Group By` operador, exceto que ele permite que os grupos que já tiverem sido formados, agregando.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3512">The `Aggregate` query operator performs a similar function as the `Group By` operator, except it allows aggregating over groups that have already been formed.</span></span> <span data-ttu-id="ab3a1-3513">Porque o grupo de já ter sido formado, o `Into` cláusula de um `Aggregate` operador não oculta as variáveis de intervalo no escopo de consulta (dessa forma, `Aggregate` é mais parecido com um `Let`, e `Group By` é mais parecido com um `Select`).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3513">Because the group has already been formed, the `Into` clause of an `Aggregate` query operator does not hide the range variables in scope (in this way, `Aggregate` is more like a `Let`, and `Group By` is more like a `Select`).</span></span>

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="ab3a1-3514">Por exemplo, a consulta a seguir agrega o total de todos os pedidos feitos pelos clientes em Washington:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3514">For example, the following query aggregates the total of all the orders placed by customers in Washington:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

<span data-ttu-id="ab3a1-3515">O resultado dessa consulta é uma coleção de tipo cujo elemento é um tipo anônimo com uma propriedade denominada `cust` tipada como `Customer` e uma propriedade chamada `Sum` digitada como `Integer`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3515">The result of this query is a collection whose element type is an anonymous type with a property named `cust` typed as `Customer` and a property named `Sum` typed as `Integer`.</span></span>

<span data-ttu-id="ab3a1-3516">Diferentemente `Group By`, os operadores de consulta adicionais podem ser colocados entre o `Aggregate` e `Into` cláusulas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3516">Unlike `Group By`, additional query operators can be placed between the `Aggregate` and `Into` clauses.</span></span> <span data-ttu-id="ab3a1-3517">Entre um `Aggregate` cláusula e o término do `Into` cláusula, todas as variáveis de intervalo em escopo, incluindo aqueles declarados pelo `Aggregate` cláusula pode ser usada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3517">Between an `Aggregate` clause and the end of the `Into` clause, all range variables in scope, including those declared by the `Aggregate` clause can be used.</span></span> <span data-ttu-id="ab3a1-3518">Por exemplo, as seguinte consulta agregações colocado a soma total de todos os pedidos por clientes em Washington antes de 2006:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3518">For example, the following query aggregates the sum total of all the orders placed by customers in Washington before 2006:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

<span data-ttu-id="ab3a1-3519">O `Aggregate` operador também pode ser usado para iniciar uma expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3519">The `Aggregate` operator can also be used to start a query expression.</span></span> <span data-ttu-id="ab3a1-3520">Nesse caso, o resultado da expressão de consulta será o único valor calculado pelo `Into` cláusula.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3520">In this case, the result of the query expression will be the single value computed by the `Into` clause.</span></span> <span data-ttu-id="ab3a1-3521">Por exemplo, a consulta a seguir calcula a soma de todos os totais de ordem antes de 1º de janeiro de 2006:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3521">For example, the following query calculates the sum of all the order totals before January 1st, 2006:</span></span>

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

<span data-ttu-id="ab3a1-3522">O resultado da consulta é um único `Integer` valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3522">The result of the query is a single `Integer` value.</span></span> <span data-ttu-id="ab3a1-3523">Um `Aggregate` operador de consulta está sempre disponível (embora a função de agregação deve ser também está disponível para a expressão a ser válido).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3523">An `Aggregate` query operator is always available (although the aggregate function must be also be available for the expression to be valid).</span></span> <span data-ttu-id="ab3a1-3524">O código</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3524">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

<span data-ttu-id="ab3a1-3525">é geralmente traduzido para</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3525">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

<span data-ttu-id="ab3a1-3526">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3526">__Note.__</span></span> <span data-ttu-id="ab3a1-3527">`Aggregate` e `Into` não são palavras reservadas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3527">`Aggregate` and `Into` are not reserved words.</span></span>


### <a name="group-join-query-operator"></a><span data-ttu-id="ab3a1-3528">Operador de consulta de junção de grupo</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3528">Group Join Query Operator</span></span>

<span data-ttu-id="ab3a1-3529">O `Group Join` operador de consulta combina as funções do `Join` e `Group By` operadores de consulta em um único operador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3529">The `Group Join` query operator combines the functions of the `Join` and `Group By` query operators into a single operator.</span></span> <span data-ttu-id="ab3a1-3530">`Group Join` une duas coleções com base na correspondência de chaves extraídas de elementos, agrupar todos os elementos que correspondem a um determinado elemento no lado esquerdo da junção no lado direito da junção.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3530">`Group Join` joins two collections based on matching keys extracted from the elements, grouping together all of the elements on the right side of the join that match a particular element on the left side of the join.</span></span> <span data-ttu-id="ab3a1-3531">Portanto, o operador produz um conjunto de resultados hierárquicos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3531">Thus, the operator produces a set of hierarchical results.</span></span>

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="ab3a1-3532">Por exemplo, a consulta a seguir produz elementos que contêm o nome de um único cliente, um grupo de todos os seus pedidos e a quantidade total de todos os pedidos:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3532">For example, the following query produces elements that contain a single customer's name, a group of all of their orders, and the total amount of all of those orders:</span></span>

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

<span data-ttu-id="ab3a1-3533">O resultado da consulta é uma coleção cujo tipo de elemento é um tipo anônimo com três propriedades: `Name`tipada como `String`, `Orders` digitada como uma coleção cujo tipo de elemento é `Order`, e `OrdersTotal`, digitado como `Integer`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3533">The result of the query is a collection whose element type is an anonymous type with three properties: `Name`, typed as `String`, `Orders` typed as a collection whose element type is `Order`, and `OrdersTotal`, typed as `Integer`.</span></span> <span data-ttu-id="ab3a1-3534">Um `Group Join` operador de consulta tem suporte apenas se o tipo de coleção contém o método:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3534">A `Group Join` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupJoin(inner As CS, _
                         outerSelector As Func(Of T, K), _
                         innerSelector As Func(Of S, K), _
                         resultSelector As Func(Of T, CS, R)) As CR
```

<span data-ttu-id="ab3a1-3535">O código</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3535">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Group Join y in ys On x Equals y _
            Into g = Group _
            ...
```

<span data-ttu-id="ab3a1-3536">é geralmente traduzido para</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3536">is generally translated to</span></span>

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

<span data-ttu-id="ab3a1-3537">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3537">__Note.__</span></span> <span data-ttu-id="ab3a1-3538">`Group`, `Join`, e `Into` não são palavras reservadas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3538">`Group`, `Join`, and `Into` are not reserved words.</span></span>


## <a name="conditional-expressions"></a><span data-ttu-id="ab3a1-3539">Expressões condicionais</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3539">Conditional Expressions</span></span>

<span data-ttu-id="ab3a1-3540">Uma condicional `If` expressão testa uma expressão e retorna um valor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3540">A conditional `If` expression tests an expression and returns a value.</span></span>

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

<span data-ttu-id="ab3a1-3541">Ao contrário de `IIF` função de tempo de execução, no entanto, uma expressão condicional só avalia seus operandos se necessário.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3541">Unlike the `IIF` runtime function, however, a conditional expression only evaluates its operands if necessary.</span></span> <span data-ttu-id="ab3a1-3542">Assim, por exemplo, a expressão `If(c Is Nothing, c.Name, "Unknown")` não lançará uma exceção se o valor da `c` é `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3542">Thus, for example, the expression `If(c Is Nothing, c.Name, "Unknown")` will not throw an exception if the value of `c` is `Nothing`.</span></span> <span data-ttu-id="ab3a1-3543">A expressão condicional tem duas formas: um que usa dois operandos e outro que usa três operandos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3543">The conditional expression has two forms: one that takes two operands and one that takes three operands.</span></span>

<span data-ttu-id="ab3a1-3544">Se três operandos forem fornecidos, todos os três expressões devem ser classificadas como valores, e o primeiro operando deve ser uma expressão booliana.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3544">If three operands are provided, all three expressions must be classified as values, and the first operand must be a Boolean expression.</span></span> <span data-ttu-id="ab3a1-3545">Se o resultado é a expressão for verdadeira, a segunda expressão será o resultado do operador, caso contrário, a terceira expressão vai ser o resultado do operador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3545">If the result is of the expression is true, then the second expression will be the result of the operator, otherwise the third expression will be the result of the operator.</span></span> <span data-ttu-id="ab3a1-3546">O tipo de resultado da expressão é o tipo dominante entre os tipos da segunda e terceira expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3546">The result type of the expression is the dominant type between the types of the second and third expression.</span></span> <span data-ttu-id="ab3a1-3547">Se não houver nenhum tipo dominante, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3547">If there is no dominant type, then a compile-time error occurs.</span></span>

<span data-ttu-id="ab3a1-3548">Se dois operandos forem fornecidos, ambos os operandos devem ser classificados como valores, e o primeiro operando deve ser um tipo de referência ou um tipo de valor anulável.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3548">If two operands are provided, both of the operands must be classified as values, and the first operand must be either a reference type or a nullable value type.</span></span> <span data-ttu-id="ab3a1-3549">A expressão `If(x, y)` , em seguida, é avaliada como se foi a expressão `If(x IsNot Nothing, x, y)`, com duas exceções.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3549">The expression `If(x, y)` is then evaluated as if was the expression `If(x IsNot Nothing, x, y)`, with two exceptions.</span></span> <span data-ttu-id="ab3a1-3550">Em primeiro lugar, a primeira expressão é só avaliada uma vez e em segundo lugar, se o tipo do segundo operando é um tipo de valor não anulável e tipo do primeiro operando for, o `?` é removido do tipo do primeiro operando ao determinar o tipo dominante para o tipo de resultado da expressão.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3550">First, the first expression is only ever evaluated once, and second, if the second operand's type is a non-nullable value type and the first operand's type is, the `?` is removed from the type of the first operand when determining the dominant type for the result type of the expression.</span></span> <span data-ttu-id="ab3a1-3551">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3551">For example:</span></span>

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

<span data-ttu-id="ab3a1-3552">Em ambos os formatos de expressão, se um operando for `Nothing`, seu tipo não é usado para determinar o tipo dominante.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3552">In both forms of the expression, if an operand is `Nothing`, its type is not used to determine the dominant type.</span></span> <span data-ttu-id="ab3a1-3553">No caso da expressão `If(<expression>, Nothing, Nothing)`, o tipo dominante é considerado `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3553">In the case of the expression `If(<expression>, Nothing, Nothing)`, the dominant type is considered to be `Object`.</span></span>


## <a name="xml-literal-expressions"></a><span data-ttu-id="ab3a1-3554">Expressões literais do XML</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3554">XML Literal Expressions</span></span>

<span data-ttu-id="ab3a1-3555">Uma expressão literal do XML representa um XML (eXtensible Markup Language) valor de 1,0.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3555">An XML literal expression represents an XML (eXtensible Markup Language) 1.0 value.</span></span>

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

<span data-ttu-id="ab3a1-3556">O resultado de uma expressão literal do XML é um valor tipado como um dos tipos do `System.Xml.Linq` namespace.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3556">The result of an XML literal expression is a value typed as one of the types from the `System.Xml.Linq` namespace.</span></span> <span data-ttu-id="ab3a1-3557">Se os tipos no namespace não estiverem disponíveis, em seguida, uma expressão literal do XML causará um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3557">If the types in that namespace are not available, then an XML literal expression will cause a compile-time error.</span></span> <span data-ttu-id="ab3a1-3558">Os valores são gerados por meio de chamadas de construtor convertidas da expressão literal XML.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3558">The values are generated through constructor calls translated from the XML literal expression.</span></span> <span data-ttu-id="ab3a1-3559">Por exemplo, o código:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3559">For example, the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

<span data-ttu-id="ab3a1-3560">é aproximadamente equivalente ao código:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3560">is roughly equivalent to the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

<span data-ttu-id="ab3a1-3561">Uma expressão literal do XML pode assumir a forma de um documento XML, um elemento XML, uma instrução de processamento de XML, um comentário XML ou uma seção CDATA.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3561">An XML literal expression can take the form of an XML document, an XML element, an XML processing instruction, an XML comment, or a CDATA section.</span></span>

<span data-ttu-id="ab3a1-3562">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3562">__Note.__</span></span> <span data-ttu-id="ab3a1-3563">Essa especificação contém apenas suficiente de uma descrição de XML para descrever o comportamento da linguagem Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3563">This specification contains only enough of a description of XML to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="ab3a1-3564">Para obter mais informações sobre o XML podem ser encontradas em http://www.w3.org/TR/REC-xml/.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3564">More information on XML can be found at http://www.w3.org/TR/REC-xml/.</span></span>


### <a name="lexical-rules"></a><span data-ttu-id="ab3a1-3565">Regras lexicais</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3565">Lexical rules</span></span>

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

<span data-ttu-id="ab3a1-3566">Expressões literais do XML são interpretadas usando regras lexicais de XML em vez de regras lexicais de regular código do Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3566">XML literal expressions are interpreted using the lexical rules of XML instead of the lexical rules of regular Visual Basic code.</span></span> <span data-ttu-id="ab3a1-3567">Geralmente, os dois conjuntos de regras diferem das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3567">The two sets of rules generally differ in the following ways:</span></span>

* <span data-ttu-id="ab3a1-3568">Espaço em branco é significativo em XML.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3568">White space is significant in XML.</span></span> <span data-ttu-id="ab3a1-3569">Como resultado, a gramática de expressões literais do XML explicitamente estados em que o espaço em branco é permitido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3569">As a result, the grammar for XML literal expressions explicitly states where white space is allowed.</span></span> <span data-ttu-id="ab3a1-3570">Espaço em branco não é preservado, exceto quando ele ocorre no contexto de dados de caracteres dentro de um elemento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3570">Whitespace is not preserved, except when it occurs in the context of character data within an element.</span></span> <span data-ttu-id="ab3a1-3571">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3571">For example:</span></span>

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

* <span data-ttu-id="ab3a1-3572">Espaço em branco finais de linha XML é normalizado de acordo com a especificação do XML.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3572">XML end-of-line whitespace is normalized according to the XML specification.</span></span>

* <span data-ttu-id="ab3a1-3573">XML diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3573">XML is case-sensitive.</span></span> <span data-ttu-id="ab3a1-3574">Palavras-chave devem corresponder maiusculas e minúsculas exatamente, caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3574">Keywords must match casing exactly, or else a compile-time error will occur.</span></span>

* <span data-ttu-id="ab3a1-3575">Terminadores de linha são considerados espaço em branco em XML.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3575">Line terminators are considered white space in XML.</span></span> <span data-ttu-id="ab3a1-3576">Como resultado, nenhum caractere de continuação de linha é necessários em expressões literais do XML.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3576">As a result, no line-continuation characters are needed in XML literal expressions.</span></span>

* <span data-ttu-id="ab3a1-3577">XML não aceita caracteres de largura total.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3577">XML does not accept full-width characters.</span></span> <span data-ttu-id="ab3a1-3578">Se forem usados caracteres de largura inteira, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3578">If full-width characters are used, a compile-time error will occur.</span></span>


### <a name="embedded-expressions"></a><span data-ttu-id="ab3a1-3579">Expressões inseridas</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3579">Embedded expressions</span></span>

<span data-ttu-id="ab3a1-3580">Expressões literais do XML podem conter *expressões incorporadas*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3580">XML literal expressions can contain *embedded expressions*.</span></span> <span data-ttu-id="ab3a1-3581">Uma expressão inserida é uma expressão do Visual Basic que é avaliada e usada para preencher um ou mais valores no local de expressão incorporada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3581">An embedded expression is a Visual Basic expression that is evaluated and used to fill in one or more values at the location of embedded expression.</span></span>

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

<span data-ttu-id="ab3a1-3582">Por exemplo, o código a seguir coloca a cadeia de caracteres `John Smith` como o valor do elemento XML:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3582">For example, the following code places the string `John Smith` as the value of the XML element:</span></span>

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

<span data-ttu-id="ab3a1-3583">Expressões podem ser inseridas em um número de contextos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3583">Expressions can be embedded in a number of contexts.</span></span> <span data-ttu-id="ab3a1-3584">Por exemplo, o código a seguir gera um elemento denominado `customer`:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3584">For example, the following code produces an element named `customer`:</span></span>

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

<span data-ttu-id="ab3a1-3585">Cada contexto onde uma expressão inserida pode ser usada Especifica os tipos que serão aceitos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3585">Each context where an embedded expression can be used specifies the types that will be accepted.</span></span> <span data-ttu-id="ab3a1-3586">Quando estiver dentro do contexto da parte de expressão de uma expressão inserida, as regras normais de léxicas para código do Visual Basic ainda aplicam isso, por exemplo, as continuações das linhas devem ser usadas:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3586">When within the context of the expression part of an embedded expression, the normal lexical rules for Visual Basic code still apply so, for example, line continuations must be used:</span></span>

```vb
' Visual Basic expression uses line continuation, XML does not
Dim element As System.Xml.Linq.XElement = _
    <<%= name & _
          name %>>John 
                     Smith</>
```


### <a name="xml-documents"></a><span data-ttu-id="ab3a1-3587">Documentos XML</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3587">XML Documents</span></span>

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

<span data-ttu-id="ab3a1-3588">Um documento XML resulta em um valor digitado como `System.Xml.Linq.XDocument`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3588">An XML document results in a value typed as `System.Xml.Linq.XDocument`.</span></span> <span data-ttu-id="ab3a1-3589">Ao contrário da especificação XML 1.0, os documentos XML em expressões literais do XML são necessárias para especificar o prólogo do documento XML; Expressões literais do XML sem o prólogo do documento XML são interpretadas como suas entidades individuais.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3589">Unlike the XML 1.0 specification, XML documents in XML literal expressions are required to specify the XML document prologue; XML literal expressions without the XML document prologue are interpreted as their individual entity.</span></span> <span data-ttu-id="ab3a1-3590">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3590">For example:</span></span>

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

<span data-ttu-id="ab3a1-3591">Um documento XML pode conter uma expressão inserida cujo tipo pode ser qualquer tipo; em tempo de execução, no entanto, o objeto deve satisfazer os requisitos do `XDocument` construtor ou um erro de tempo de execução ocorrerá.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3591">An XML document can contain an embedded expression whose type can be any type; at runtime, however, the object must satisfy the requirements of the `XDocument` constructor or a run-time error will occur.</span></span>

<span data-ttu-id="ab3a1-3592">Ao contrário de XML regular, expressões de documento XML não têm suporte DTDs (declarações de tipo de documento).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3592">Unlike regular XML, XML document expressions do not support DTDs (Document Type Declarations).</span></span> <span data-ttu-id="ab3a1-3593">Além disso, o atributo de codificação, se fornecido, será ignorado, pois a codificação da expressão literal Xml é sempre o mesmo que a codificação do arquivo de origem em si.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3593">Also, the encoding attribute, if supplied, will be ignored since the encoding of the Xml literal expression is always the same as the encoding of the source file itself.</span></span>

<span data-ttu-id="ab3a1-3594">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3594">__Note.__</span></span> <span data-ttu-id="ab3a1-3595">Embora o atributo de codificação é ignorado, ele é o atributo ainda são válido para manter a capacidade de incluir todos os documentos Xml 1.0 válidos no código-fonte.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3595">Although the encoding attribute is ignored, it is still valid attribute in order to maintain the ability to include any valid Xml 1.0 documents in source code.</span></span>


### <a name="xml-elements"></a><span data-ttu-id="ab3a1-3596">Elementos XML</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3596">XML Elements</span></span>

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

<span data-ttu-id="ab3a1-3597">Um elemento XML resulta em um valor digitado como `System.Xml.Linq.XElement`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3597">An XML element results in a value typed as `System.Xml.Linq.XElement`.</span></span> <span data-ttu-id="ab3a1-3598">Ao contrário de XML regular, elementos XML podem omitir o nome na marca de fechamento e elemento aninhado mais atual será fechado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3598">Unlike regular XML, XML elements can omit the name in the closing tag and the current most-nested element will be closed.</span></span> <span data-ttu-id="ab3a1-3599">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3599">For example:</span></span>

```vb
Dim name = <name>Bob</>
```

<span data-ttu-id="ab3a1-3600">Declarações em um resultado do elemento XML em valores de tipo de atributo `System.Xml.Linq.XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3600">Attribute declarations in an XML element result in values typed as `System.Xml.Linq.XAttribute`.</span></span> <span data-ttu-id="ab3a1-3601">Valores de atributo são normalizados de acordo com a especificação do XML.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3601">Attribute values are normalized according to the XML specification.</span></span> <span data-ttu-id="ab3a1-3602">Quando o valor de um atributo é `Nothing` o atributo não será criado, portanto, não terá a expressão de valor do atributo a ser verificada para `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3602">When the value of an attribute is `Nothing` the attribute will not be created, so the attribute value expression will not have to be checked for `Nothing`.</span></span> <span data-ttu-id="ab3a1-3603">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3603">For example:</span></span>

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

<span data-ttu-id="ab3a1-3604">Elementos e atributos XML podem conter expressões aninhadas nos seguintes locais:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3604">XML elements and attributes can contain nested expressions in the following places:</span></span>

<span data-ttu-id="ab3a1-3605">O nome do elemento, nesse caso, a expressão inserida deve ser um valor de um tipo implicitamente conversível para `System.Xml.Linq.XName`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3605">The name of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="ab3a1-3606">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3606">For example:</span></span>

```vb
Dim name = <<%= "name" %>>Bob</>
```

<span data-ttu-id="ab3a1-3607">O nome de um atributo do elemento, nesse caso, a expressão inserida deve ser um valor de um tipo implicitamente conversível para `System.Xml.Linq.XName`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3607">The name of an attribute of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="ab3a1-3608">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3608">For example:</span></span>

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

<span data-ttu-id="ab3a1-3609">O valor de um atributo do elemento, nesse caso a expressão inserida pode ser um valor de qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3609">The value of an attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="ab3a1-3610">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3610">For example:</span></span>

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

<span data-ttu-id="ab3a1-3611">Um atributo do elemento, nesse caso a expressão inserida pode ser um valor de qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3611">An attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="ab3a1-3612">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3612">For example:</span></span>

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

<span data-ttu-id="ab3a1-3613">O conteúdo do elemento, nesse caso a expressão inserida pode ser um valor de qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3613">The content of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="ab3a1-3614">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3614">For example:</span></span>

```vb
Dim name = <name><%= "Bob" %></>
```

<span data-ttu-id="ab3a1-3615">Se for o tipo de expressão inserida `Object()`, a matriz será passada como um paramarray para o `XElement` construtor.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3615">If the type of the embedded expression is `Object()`, the array will be passed as a paramarray to the `XElement` constructor.</span></span>


### <a name="xml-namespaces"></a><span data-ttu-id="ab3a1-3616">Namespaces XML</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3616">XML Namespaces</span></span>

<span data-ttu-id="ab3a1-3617">Elementos XML podem conter declarações de namespace XML, conforme definido pela especificação XML 1.0 de namespaces.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3617">XML elements can contain XML namespace declarations, as defined by the XML namespaces 1.0 specification.</span></span>

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

<span data-ttu-id="ab3a1-3618">As restrições sobre como definir os namespaces `xml` e `xmlns` são impostas e gerarão erros de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3618">The restrictions on defining the namespaces `xml` and `xmlns` are enforced and will produce compile-time errors.</span></span> <span data-ttu-id="ab3a1-3619">Declarações de namespace XML não podem ter uma expressão inserida para seu valor; o valor fornecido deve ser um literal de cadeia não vazia.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3619">XML namespace declarations cannot have an embedded expression for their value; the value supplied must be a non-empty string literal.</span></span> <span data-ttu-id="ab3a1-3620">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3620">For example:</span></span>

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

<span data-ttu-id="ab3a1-3621">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3621">__Note.__</span></span> <span data-ttu-id="ab3a1-3622">Essa especificação contém apenas suficiente de uma descrição do namespace XML para descrever o comportamento da linguagem Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3622">This specification contains only enough of a description of XML namespace to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="ab3a1-3623">Para obter mais informações sobre namespaces XML podem ser encontradas em http://www.w3.org/TR/REC-xml-names/.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3623">More information on XML namespaces can be found at http://www.w3.org/TR/REC-xml-names/.</span></span>

<span data-ttu-id="ab3a1-3624">Nomes de elementos e atributos XML podem ser qualificados com o uso de nomes de namespace.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3624">XML element and attribute names can be qualified using namespace names.</span></span> <span data-ttu-id="ab3a1-3625">Namespaces são associados como no XML regular, com a exceção que importa qualquer namespace declarada no nível de arquivo são considerados para ser declarada em um contexto de circunscrição a declaração, que é colocada por qualquer importação de namespace declarada pela compilação ambiente.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3625">Namespaces are bound as in regular XML, with the exception that any namespace imports declared at the file level are considered to be declared in a context enclosing the declaration, which is itself enclosed by any namespace imports declared by the compilation environment.</span></span> <span data-ttu-id="ab3a1-3626">Se um nome de namespace não for encontrado, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3626">If a namespace name cannot be found, a compile-time error occurs.</span></span> <span data-ttu-id="ab3a1-3627">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3627">For example:</span></span>

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

<span data-ttu-id="ab3a1-3628">Namespaces XML declarados em um elemento não se aplicam aos literais XML dentro de expressões inseridas.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3628">XML namespaces declared in an element do not apply to XML literals inside embedded expressions.</span></span> <span data-ttu-id="ab3a1-3629">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3629">For example:</span></span>

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

<span data-ttu-id="ab3a1-3630">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3630">__Note.__</span></span> <span data-ttu-id="ab3a1-3631">Isso ocorre porque a expressão inserida pode ser qualquer coisa, incluindo uma chamada de função.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3631">This is because the embedded expression can be anything, including a function call.</span></span> <span data-ttu-id="ab3a1-3632">Se a chamada de função continha uma expressão literal do XML, ele não está claro se os programadores esperaria que o namespace de XML a ser aplicado ou ignorado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3632">If the function call contained an XML literal expression, it is not clear whether programmers would expect the XML namespace to be applied or ignored.</span></span>


### <a name="xml-processing-instructions"></a><span data-ttu-id="ab3a1-3633">Instruções de processamento de XML</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3633">XML Processing Instructions</span></span>

<span data-ttu-id="ab3a1-3634">Uma instrução de processamento de XML resulta em um valor digitado como `System.Xml.Linq.XProcessingInstruction`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3634">An XML processing instruction results in a value typed as `System.Xml.Linq.XProcessingInstruction`.</span></span> <span data-ttu-id="ab3a1-3635">Instruções de processamento de XML não podem conter expressões incorporadas, como eles são uma sintaxe válida dentro de instrução de processamento.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3635">XML processing instructions cannot contain embedded expressions, as they are valid syntax within the processing instruction.</span></span>

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

### <a name="xml-comments"></a><span data-ttu-id="ab3a1-3636">Comentários XML</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3636">XML Comments</span></span>

<span data-ttu-id="ab3a1-3637">Um comentário XML resulta em um valor digitado como `System.Xml.Linq.XComment`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3637">An XML comment results in a value typed as `System.Xml.Linq.XComment`.</span></span> <span data-ttu-id="ab3a1-3638">Comentários XML não podem conter expressões incorporadas, como eles são uma sintaxe válida com o comentário.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3638">XML comments cannot contain embedded expressions, as they are valid syntax within the comment.</span></span>

```antlr
XMLComment
    : '<' '!' '-' '-' XMLCommentCharacter* '-' '-' '>'
    ;

XMLCommentCharacter
    : '<Any XMLCharacter except dash (0x002D)>'
    | '-' '<Any XMLCharacter except dash (0x002D)>'
    ;
```

### <a name="cdata-sections"></a><span data-ttu-id="ab3a1-3639">Seções CDATA</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3639">CDATA sections</span></span>

<span data-ttu-id="ab3a1-3640">Uma seção CDATA resulta em um valor digitado como `System.Xml.Linq.XCData`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3640">A CDATA section results in a value typed as `System.Xml.Linq.XCData`.</span></span> <span data-ttu-id="ab3a1-3641">As seções CDATA não podem conter expressões incorporadas, como eles são uma sintaxe válida dentro da seção CDATA.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3641">CDATA sections cannot contain embedded expressions, as they are valid syntax within the CDATA section.</span></span>

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a><span data-ttu-id="ab3a1-3642">Expressões de acesso de membro XML</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3642">XML Member Access Expressions</span></span>

<span data-ttu-id="ab3a1-3643">Uma expressão de acesso de membro XML acessa os membros de um valor XML.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3643">An XML member access expression accesses the members of an XML value.</span></span>

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

<span data-ttu-id="ab3a1-3644">Há três tipos de expressões de acesso de membro XML:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3644">There are three types of XML member access expressions:</span></span>

* <span data-ttu-id="ab3a1-3645">*Acesso de elemento*, no qual um nome XML segue um único ponto.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3645">*Element access*, in which an XML name follows a single dot.</span></span> <span data-ttu-id="ab3a1-3646">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3646">For example:</span></span>

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  <span data-ttu-id="ab3a1-3647">Acesso de elemento é mapeado para a função:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3647">Element access maps to the function:</span></span>

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  <span data-ttu-id="ab3a1-3648">Portanto, o exemplo acima é equivalente a:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3648">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* <span data-ttu-id="ab3a1-3649">*Acesso de atributo*, no qual um identificador Visual Basic segue um ponto e no logon ou um XML o nome segue um ponto e um sinal de arroba.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3649">*Attribute access*, in which a Visual Basic identifier follows a dot and an at sign, or an XML name follows a dot and an at sign.</span></span> <span data-ttu-id="ab3a1-3650">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3650">For example:</span></span>

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  <span data-ttu-id="ab3a1-3651">Mapas de acesso de atributo para a função:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3651">Attribute access maps to the function:</span></span>

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  <span data-ttu-id="ab3a1-3652">Portanto, o exemplo acima é equivalente a:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3652">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  <span data-ttu-id="ab3a1-3653">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3653">__Note.__</span></span> <span data-ttu-id="ab3a1-3654">O `AttributeValue` método de extensão (bem como a propriedade de extensão relacionados `Value`) não está definido atualmente em qualquer assembly.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3654">The `AttributeValue` extension method (as well as the related extension property `Value`) is not currently defined in any assembly.</span></span> <span data-ttu-id="ab3a1-3655">Se os membros de extensão são necessários, eles são automaticamente definidos no assembly que está sendo produzido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3655">If the extension members are needed, they are automatically defined in the assembly being produced.</span></span>

* <span data-ttu-id="ab3a1-3656">*Acesso de seus descendentes*, no qual um nomes XML segue nos três pontos.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3656">*Descendents access*, in which an XML names follows three dots.</span></span> <span data-ttu-id="ab3a1-3657">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3657">For example:</span></span>

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

  <span data-ttu-id="ab3a1-3658">Acesso de seus descendentes é mapeado para a função:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3658">Descendents access maps to the function:</span></span>

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  <span data-ttu-id="ab3a1-3659">Portanto, o exemplo acima é equivalente a:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3659">So the above example is equivalent to:</span></span>

  ```vb
  Dim customers = company.Descendants("customer")
  ```

<span data-ttu-id="ab3a1-3660">A expressão de base de uma expressão de acesso de membro XML deve ser um valor e deve ser do tipo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3660">The base expression of an XML member access expression must be a value and must be of the type:</span></span>

* <span data-ttu-id="ab3a1-3661">Se um elemento ou seus descendentes acessar, `System.Xml.Linq.XContainer` ou um tipo derivado, ou `System.Collections.Generic.IEnumerable(Of T)` ou um tipo derivado, onde `T` é `System.Xml.Linq.XContainer` ou um tipo derivado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3661">If an element or descendents access,  `System.Xml.Linq.XContainer` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XContainer` or a derived type.</span></span>

* <span data-ttu-id="ab3a1-3662">Se um acesso de atributo `System.Xml.Linq.XElement` ou um tipo derivado, ou `System.Collections.Generic.IEnumerable(Of T)` ou um tipo derivado, onde `T` é `System.Xml.Linq.XElement` ou um tipo derivado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3662">If an attribute access, `System.Xml.Linq.XElement` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XElement` or a derived type.</span></span>

<span data-ttu-id="ab3a1-3663">Nomes em expressões de acesso de membro XML não podem estar vazios.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3663">Names in XML member access expressions cannot be empty.</span></span> <span data-ttu-id="ab3a1-3664">Eles podem ser qualificado, usando qualquer namespace definido pelo importações de namespace.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3664">They can be namespace qualified, using any namespaces defined by imports.</span></span> <span data-ttu-id="ab3a1-3665">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3665">For example:</span></span>

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

<span data-ttu-id="ab3a1-3666">Espaço em branco não é permitido após o dot(s) em uma expressão de acesso de membro XML ou entre os colchetes angulares e o nome.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3666">Whitespace is not allowed after the dot(s) in an XML member access expression, or between the angle brackets and the name.</span></span> <span data-ttu-id="ab3a1-3667">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3667">For example:</span></span>

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

<span data-ttu-id="ab3a1-3668">Se os tipos no `System.Xml.Linq` namespace não estão disponíveis e, em seguida, uma expressão de acesso de membro XML causará um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3668">If the types in the `System.Xml.Linq` namespace are not available, then an XML member access expression will cause a compile-time error.</span></span>


## <a name="await-operator"></a><span data-ttu-id="ab3a1-3669">Operador Await</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3669">Await Operator</span></span>

<span data-ttu-id="ab3a1-3670">O operador de espera está relacionado a métodos assíncronos, que são descritos na seção [métodos assíncronos](statements.md#async-methods).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3670">The await operator is related to async methods, which are described in Section [Async Methods](statements.md#async-methods).</span></span>

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

<span data-ttu-id="ab3a1-3671">`Await` é uma palavra reservada se a expressão de lambda ou método imediatamente delimitador no qual ele aparece tem um `Async` modificador e se o `Await` aparece depois que `Async` modificador; é não reservado em outro lugar.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3671">`Await` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Async` modifier, and if the `Await` appears after that `Async` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="ab3a1-3672">Também é reservado em diretivas de pré-processador.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3672">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="ab3a1-3673">O operador de espera só é permitido no corpo de um método ou expressões lambda em que ele é uma palavra reservada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3673">The await operator is only allowed in the body of a method or lambda expressions where it is a reserved word.</span></span> <span data-ttu-id="ab3a1-3674">Dentro do método ou lambda imediatamente delimitador, uma expressão await não pode ocorrer dentro do corpo de uma `Catch` ou `Finally` bloquear nou dentro o corpo de um `SyncLock` instrução nem dentro uma expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3674">Within the immediately enclosing method or lambda, an await expression may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement, nor inside a query expression.</span></span>

<span data-ttu-id="ab3a1-3675">O operador await toma uma única expressão que deve ser classificado como um valor e cujo tipo deve ser um *aguardável* tipo, ou `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3675">The await operator takes a single expression which must be classified as a value and whose type must be an *awaitable* type, or `Object`.</span></span> <span data-ttu-id="ab3a1-3676">Se seu tipo for `Object` e em seguida, todo o processamento é adiada até o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3676">If its type is `Object` then all processing is deferred until run-time.</span></span> <span data-ttu-id="ab3a1-3677">Um tipo `C` será considerada awaitable, se todos os itens a seguir forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3677">A type `C` is said to be awaitable if all of the following are true:</span></span>

* <span data-ttu-id="ab3a1-3678">`C` contém um método de extensão ou de instância acessível nomeado `GetAwaiter` que não tiver nenhum argumento e que retorne algum tipo `E`;</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3678">`C` contains an accessible instance or extension method named `GetAwaiter` which has no arguments and which returns some type `E`;</span></span>

* <span data-ttu-id="ab3a1-3679">`E` contém uma propriedade de instância ou extensão legível chamada `IsCompleted` que não leva argumentos e tem um tipo booleano;</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3679">`E` contains a readable instance or extension property named `IsCompleted` which takes no arguments and has type Boolean;</span></span>

* <span data-ttu-id="ab3a1-3680">`E` contém um método de extensão ou de instância acessível chamado `GetResult` que não usa argumentos;</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3680">`E` contains an accessible instance or extension method named `GetResult` which takes no arguments;</span></span>

* <span data-ttu-id="ab3a1-3681">`E` implemente `System.Runtime.CompilerServices.INotifyCompletion` ou `ICriticalNotifyCompletion`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3681">`E` implements either `System.Runtime.CompilerServices.INotifyCompletion` or `ICriticalNotifyCompletion`.</span></span>

<span data-ttu-id="ab3a1-3682">Se `GetResult` era um `Sub`, em seguida, a expressão await é classificada como void.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3682">If `GetResult` was a `Sub`, then the await expression is classified as void.</span></span> <span data-ttu-id="ab3a1-3683">Caso contrário, a expressão await é classificada como um valor e seu tipo é o tipo de retorno de `GetResult` método.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3683">Otherwise, the await expression is classified as a value and its type is the return type of the `GetResult` method.</span></span>

<span data-ttu-id="ab3a1-3684">Aqui está um exemplo de uma classe que pode ser colocada em espera:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3684">Here is an example of a class that can be awaited:</span></span>

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

<span data-ttu-id="ab3a1-3685">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3685">__Note.__</span></span> <span data-ttu-id="ab3a1-3686">Autores de biblioteca são recomendados para seguir o padrão de invocação de delegado de continuação no mesmo `SynchronizationContext` como seus `OnCompleted` foi invocado em.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3686">Library authors are recommended to follow the pattern that they invoke the continuation delegate on the same `SynchronizationContext` as their `OnCompleted` was itself invoked on.</span></span> <span data-ttu-id="ab3a1-3687">Além disso, o delegado de continuação não deve ser executado em forma síncrona o `OnCompleted` método, pois isso pode levar a estouro de pilha: em vez disso, o delegado deve ser enfileirado para execução subsequente.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3687">Also, the resumption delegate should not be executed synchronously within the `OnCompleted` method since that can lead to stack overflow: instead, the delegate should be queued for subsequent execution.</span></span>

<span data-ttu-id="ab3a1-3688">Quando o controle fluxo atinge um `Await` operador, o comportamento é o seguinte.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3688">When control flow reaches an `Await` operator, behavior is as follows.</span></span>

1.  <span data-ttu-id="ab3a1-3689">O `GetAwaiter` método do operando await é invocado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3689">The `GetAwaiter` method of the await operand is invoked.</span></span> <span data-ttu-id="ab3a1-3690">O resultado da invocação this é chamado de *awaiter*.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3690">The result of this invocation is termed the *awaiter*.</span></span>

2.  <span data-ttu-id="ab3a1-3691">O awaiter `IsCompleted` propriedade é recuperada.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3691">The awaiter's `IsCompleted` property is retrieved.</span></span> <span data-ttu-id="ab3a1-3692">Se o resultado for true, em seguida:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3692">If the result is true then:</span></span>

    21. <span data-ttu-id="ab3a1-3693">O `GetResult` método de awaiter é invocado.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3693">The `GetResult` method of the awaiter is invoked.</span></span> <span data-ttu-id="ab3a1-3694">Se `GetResult` era uma função, em seguida, o valor da expressão await é o valor de retorno dessa função.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3694">If `GetResult` was a function, then the value of the await expression is the return value of this function.</span></span>

3.  <span data-ttu-id="ab3a1-3695">Se a propriedade IsCompleted não for true, em seguida:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3695">If the IsCompleted property isn't true then:</span></span>

    31. <span data-ttu-id="ab3a1-3696">Qualquer um dos `ICriticalNotifyCompletion.UnsafeOnCompleted` é invocado no awaiter (se o tipo do awaiter `E` implementa `ICriticalNotifyCompletion`) ou `INotifyCompletion.OnCompleted` (caso contrário).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3696">Either `ICriticalNotifyCompletion.UnsafeOnCompleted` is invoked on the awaiter (if the awaiter's type `E` implements `ICriticalNotifyCompletion`) or `INotifyCompletion.OnCompleted` (otherwise).</span></span> <span data-ttu-id="ab3a1-3697">Em ambos os casos passa um *delegado de continuação* associado à instância atual do método assíncrono.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3697">In both cases it passes a *resumption delegate* associated with the current instance of the async method.</span></span>

    32. <span data-ttu-id="ab3a1-3698">O ponto de controle da instância de método assíncrono atual está suspenso e controlar fluxo retoma na *chamador atual* (definidas na seção [métodos assíncronos](statements.md#async-methods)).</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3698">The control point of the current async method instance is suspended, and control flow resumes in the *current caller* (defined in Section [Async Methods](statements.md#async-methods)).</span></span>

    33. <span data-ttu-id="ab3a1-3699">Se o delegado de continuação é invocado posteriormente,</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3699">If later the resumption delegate is invoked,</span></span>

        331. <span data-ttu-id="ab3a1-3700">o delegado de continuação primeiro restaura `System.Threading.Thread.CurrentThread.ExecutionContext` ao que era no momento `OnCompleted` foi chamado,</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3700">the resumption delegate first restores `System.Threading.Thread.CurrentThread.ExecutionContext` to what it was at the time `OnCompleted` was called,</span></span>
        332. <span data-ttu-id="ab3a1-3701">em seguida, ele retoma o fluxo de controle no ponto de controle da instância de método assíncrono (consulte a seção [métodos assíncronos](statements.md#async-methods)),</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3701">then it resumes control flow at the control point of the async method instance (see Section [Async Methods](statements.md#async-methods)),</span></span>
        333. <span data-ttu-id="ab3a1-3702">em que ele chama o `GetResult` método awaiter, como no 2.1 acima.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3702">where it calls the `GetResult` method of the awaiter, as in 2.1 above.</span></span>

<span data-ttu-id="ab3a1-3703">Se o operando da expressão await tem o tipo de objeto, em seguida, esse comportamento é adiado até o tempo de execução:</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3703">If the await operand has type Object, then this behavior is deferred until runtime:</span></span>

- <span data-ttu-id="ab3a1-3704">Etapa 1 é realizada pela chamada GetAwaiter() sem argumentos; ele pode se associar, portanto, em tempo de execução para métodos de instância que usam parâmetros opcionais.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3704">Step 1 is accomplished by calling GetAwaiter() with no arguments; it may therefore bind at runtime to instance methods which take optional parameters.</span></span>
- <span data-ttu-id="ab3a1-3705">Etapa 2 é realizada, recuperando a propriedade IsCompleted() sem argumentos e pela tentativa de uma conversão intrínseco em booliano.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3705">Step 2 is accomplished by retrieving the IsCompleted() property with no arguments, and by attempting an intrinsic conversion to Boolean.</span></span>
- <span data-ttu-id="ab3a1-3706">Etapa 3.a é realizado pela tentativa `TryCast(awaiter, ICriticalNotifyCompletion)`, e se isso falhar, em seguida, `DirectCast(awaiter, INotifyCompletion)`.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3706">Step 3.a is accomplished by attempting `TryCast(awaiter, ICriticalNotifyCompletion)`, and if this fails then `DirectCast(awaiter, INotifyCompletion)`.</span></span>

<span data-ttu-id="ab3a1-3707">O delegado de continuação passado 3.a pode ser invocado somente uma vez.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3707">The resumption delegate passed in 3.a may only be invoked once.</span></span> <span data-ttu-id="ab3a1-3708">Se for chamado mais de uma vez, o comportamento será indefinido.</span><span class="sxs-lookup"><span data-stu-id="ab3a1-3708">If it is invoked more than once, the behavior is undefined.</span></span>

