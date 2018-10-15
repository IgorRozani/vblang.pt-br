### <a name="overloaded-method-resolution"></a><span data-ttu-id="ff672-101">Resolução do método sobrecarregado</span><span class="sxs-lookup"><span data-stu-id="ff672-101">Overloaded Method Resolution</span></span>

<span data-ttu-id="ff672-102">Na prática, as regras para determinar a resolução de sobrecarga devem encontrar a sobrecarga que é "mais próxima" para os argumentos reais fornecidos.</span><span class="sxs-lookup"><span data-stu-id="ff672-102">In practice, the rules for determining overload resolution are intended to find the overload that is "closest" to the actual arguments supplied.</span></span> <span data-ttu-id="ff672-103">Se houver um método cujos tipos de parâmetro correspondem aos tipos de argumento, em seguida, esse método é obviamente o mais próximo.</span><span class="sxs-lookup"><span data-stu-id="ff672-103">If there is a method whose parameter types match the argument types, then that method is obviously the closest.</span></span> <span data-ttu-id="ff672-104">Bloqueando que, um método é mais próximo do que outro se todos os seus tipos de parâmetro são mais estreitos do que (ou igual a) os tipos de parâmetro de outro método.</span><span class="sxs-lookup"><span data-stu-id="ff672-104">Barring that, one method is closer than another if all of its parameter types are narrower than (or the same as) the parameter types of the other method.</span></span> <span data-ttu-id="ff672-105">Se os parâmetros do método, nem são mais estreitos do que o outro, em seguida, não é possível para determinar qual método é mais próximo para os argumentos.</span><span class="sxs-lookup"><span data-stu-id="ff672-105">If neither method's parameters are narrower than the other, then there is no way for to determine which method is closer to the arguments.</span></span>

<span data-ttu-id="ff672-106">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ff672-106">__Note.__</span></span> <span data-ttu-id="ff672-107">Resolução de sobrecarga não leva em conta o tipo de retorno esperado do método.</span><span class="sxs-lookup"><span data-stu-id="ff672-107">Overload resolution does not take into account the expected return type of the method.</span></span> 

<span data-ttu-id="ff672-108">Observe também que, devido à sintaxe do parâmetro nomeado, a ordem dos parâmetros formais e reais pode não ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="ff672-108">Also note that because of the named parameter syntax, the ordering of the actual and formal parameters may not be the same.</span></span>

<span data-ttu-id="ff672-109">Dado um grupo de método, o método mais aplicável no grupo para uma lista de argumentos é determinado usando as etapas a seguir.</span><span class="sxs-lookup"><span data-stu-id="ff672-109">Given a method group, the most applicable method in the group for an argument list is determined using the following steps.</span></span> <span data-ttu-id="ff672-110">Se, após a aplicação de uma determinada etapa, nenhum membro permanecem no conjunto, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ff672-110">If, after applying a particular step, no members remain in the set, then a compile-time error occurs.</span></span> <span data-ttu-id="ff672-111">Se apenas um membro permanece no conjunto, esse membro é o membro mais aplicável.</span><span class="sxs-lookup"><span data-stu-id="ff672-111">If only one member remains in the set, then that member is the most applicable member.</span></span> <span data-ttu-id="ff672-112">As etapas são:</span><span class="sxs-lookup"><span data-stu-id="ff672-112">The steps are:</span></span>

1.  <span data-ttu-id="ff672-113">Primeiro, não se tiver sido fornecido nenhum argumento de tipo, aplique inferência de tipo sobre quaisquer métodos que têm parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="ff672-113">First, if no type arguments have been supplied, apply type inference to any methods which have type parameters.</span></span> <span data-ttu-id="ff672-114">Se a inferência de tipo for bem-sucedida para um método, os argumentos de tipo inferidos são usados para esse método específico.</span><span class="sxs-lookup"><span data-stu-id="ff672-114">If type inference succeeds for a method, then the inferred type arguments are used for that particular method.</span></span> <span data-ttu-id="ff672-115">Se a inferência de tipo falhar para um método, esse método é eliminado do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-115">If type inference fails for a method, then that method is eliminated from the set.</span></span>

2.  <span data-ttu-id="ff672-116">Em seguida, eliminar todos os membros do conjunto que estão inacessível ou não aplicável (seção [aplicabilidade à lista de argumentos](overload-resolution.md#applicability-to-argument-list)) à lista de argumentos</span><span class="sxs-lookup"><span data-stu-id="ff672-116">Next, eliminate all members from the set that are inaccessible or not applicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) to the argument list</span></span>

3.  <span data-ttu-id="ff672-117">Em seguida, se um ou mais argumentos forem `AddressOf` ou expressões lambda, em seguida, calcule a *níveis relaxamento de delegado* para cada argumento tal como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="ff672-117">Next, if one or more arguments are `AddressOf` or lambda expressions, then calculate the *delegate relaxation levels* for each such argument as below.</span></span> <span data-ttu-id="ff672-118">Se o pior (menor) delegar nível relaxamento `N` é pior do que o nível mais baixo de relaxamento de delegado no `M`, eliminar, em seguida, `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-118">If the worst (lowest) delegate relaxation level in `N` is worse than the lowest delegate relaxation level in `M`, then eliminate `N` from the set.</span></span> <span data-ttu-id="ff672-119">Os níveis de relaxamento de delegado são da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ff672-119">The delegate relaxation levels are as follows:</span></span>

    31. <span data-ttu-id="ff672-120">*Nível de relaxamento de delegado de erro* - - se a `AddressOf` ou lambda não pode ser convertido para o tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="ff672-120">*Error delegate relaxation level* -- if the `AddressOf` or lambda cannot be converted to the delegate type.</span></span>
  
    32. <span data-ttu-id="ff672-121">*Relaxamento de delegado do tipo de retorno ou parâmetros de estreitamento* – se o argumento for `AddressOf` ou um lambda com um tipo de declaração e a conversão de seu tipo de retorno para o delegado de retorno tipo é restritiva; ou se o argumento for um lambda regular e a conversão de qualquer uma das suas expressões de retorno para o delegado de retorno é de tipo de restrição ou se o argumento for um lambda async e o delegado de retorno tipo é `Task(Of T)` e a conversão de qualquer uma das suas expressões de retorno para `T` é restritiva; ou se o argumento é um lambda de iterador e tipo de retorno do delegado `IEnumerator(Of T)` ou `IEnumerable(Of T)` e a conversão de qualquer um dos seus operandos yield para `T` é restritiva.</span><span class="sxs-lookup"><span data-stu-id="ff672-121">*Narrowing delegate relaxation of return type or parameters* -- if the argument is `AddressOf` or a lambda with a declared type and the conversion from its return type to the delegate return type is narrowing; or if the argument is a regular lambda and the conversion from any of its return expressions to the delegate return type is narrowing, or if the argument is an async lambda and the delegate return type is `Task(Of T)` and the conversion from any of its return expressions to `T` is narrowing; or if the argument is an iterator lambda and the delegate return type `IEnumerator(Of T)` or `IEnumerable(Of T)` and the conversion from any of its yield operands to `T` is narrowing.</span></span>

    33. <span data-ttu-id="ff672-122">*Relaxamento de delegado para delegar sem assinatura de ampliação* - - que é do tipo de delegado `System.Delegate` ou `System.MultiCastDelegate` ou `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="ff672-122">*Widening delegate relaxation to delegate without signature* -- if delegate type is `System.Delegate` or `System.MultiCastDelegate` or `System.Object`.</span></span>

    34. <span data-ttu-id="ff672-123">*Descartar o relaxamento de delegado de retorno ou argumentos* – se o argumento for `AddressOf` ou um lambda com um tipo de retorno declarado e o tipo de delegado não tem um tipo de retorno; ou se o argumento for um lambda com uma ou mais expressões e o tipo de delegado de retorno não tem um tipo de retorno; ou se o argumento for `AddressOf` ou lambda sem parâmetros e o tipo de delegado tem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ff672-123">*Drop return or arguments delegate relaxation* -- if the argument is `AddressOf` or a lambda with a declared return type and the delegate type lacks a return type; or if the argument is a lambda with one or more return expressions and the delegate type lacks a return type; or if the argument is `AddressOf` or lambda with no parameters and the delegate type has parameters.</span></span>

    35. <span data-ttu-id="ff672-124">*Relaxamento de delegado do tipo de retorno de ampliação* – se o argumento for `AddressOf` ou lambda com um tipo de retorno declarado, e há uma conversão de ampliação do seu tipo de retorno do delegado; ou se o argumento for um lambda regular em que o conversão de todas as expressões de retornadas para o tipo de retorno do delegado é de ampliação ou a identidade com pelo menos uma ampliação; ou se o argumento for um lambda async e o delegado é `Task(Of T)` ou `Task` e a conversão de todas as expressões de retornadas para `T` / `Object` respectivamente é de ampliação ou a identidade pelo menos uma ampliação; ou se o argumento é um lambda de iterador e é o delegado `IEnumerator(Of T)` ou `IEnumerable(Of T)` ou `IEnumerator` ou `IEnumerable` e a conversão de todas as expressões de retornadas para `T` / `Object` está ampliando ou identidade com pelo menos um de ampliação.</span><span class="sxs-lookup"><span data-stu-id="ff672-124">*Widening delegate relaxation of return type* -- if the argument is `AddressOf` or a lambda with a declared return type, and there is a widening conversion from its return type to that of the delegate; or if the argument is a regular lambda where the conversion from all return expressions to the delegate return type is widening or identity with at least one widening; or if the argument is an async lambda and the delegate is `Task(Of T)` or `Task` and the conversion from all return expressions to `T`/`Object` respectively is widening or identity with at least one widening; or if the argument is an iterator lambda and the delegate is `IEnumerator(Of T)` or `IEnumerable(Of T)` or `IEnumerator` or `IEnumerable` and the conversion from all return expressions to `T`/`Object` is widening or identity with at least one widening.</span></span>

    36. <span data-ttu-id="ff672-125">*Relaxamento de delegado de identidade* – se o argumento for um `AddressOf` ou um lambda que corresponde ao delegado exatamente, sem nenhum ampliação ou redução ou descarte de parâmetros ou retorna ou produz. Em seguida, se alguns membros do conjunto de conversões de estreitamento não exigindo para ser aplicáveis a qualquer um dos argumentos, em seguida, elimine todos os membros que fazem.</span><span class="sxs-lookup"><span data-stu-id="ff672-125">*Identity delegate relaxation* -- if the argument is an `AddressOf` or a lambda which matches the delegate exactly, with no widening or narrowing or dropping of parameters or returns or yields.Next, if some members of the set do not requiring narrowing conversions to be applicable to any of the arguments, then eliminate all members that do.</span></span> <span data-ttu-id="ff672-126">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-126">For example:</span></span>

    ```vb
    Sub f(x As Object)
    End Sub

    Sub f(x As Short)
    End Sub

    Sub f(x As Short())
    End Sub

    f("5") ' picks the Object overload, since String->Short is narrowing
    f(5)   ' picks the Object overload, since Integer->Short is narrowing
    f({5}) ' picks the Object overload, since Integer->Short is narrowing
    f({})  ' a tie-breaker rule subsequent to [3] picks the Short() overload

    ```

4.  <span data-ttu-id="ff672-127">Em seguida, a eliminação é feita com base na redução da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="ff672-127">Next, elimination is done based on narrowing as follows.</span></span> <span data-ttu-id="ff672-128">(Observe que, se Option Strict for On, em seguida, todos os membros que exigem a restrição já foi considerados inaplicáveis (seção [aplicabilidade à lista de argumentos](overload-resolution.md#applicability-to-argument-list)) e removido pela etapa 2.)</span><span class="sxs-lookup"><span data-stu-id="ff672-128">(Note that, if Option Strict is On, then all members that require narrowing have already been judged inapplicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) and removed by Step 2.)</span></span>

    41. <span data-ttu-id="ff672-129">Se alguns membros de instância do conjunto de exigem apenas onde o tipo de expressão de argumento é de conversões de estreitamento `Object`, em seguida, eliminar todos os outros membros.</span><span class="sxs-lookup"><span data-stu-id="ff672-129">If some instance members of the set only require narrowing conversions where the argument expression type is `Object`, then eliminate all other members.</span></span>
    42. <span data-ttu-id="ff672-130">Se o conjunto contém mais de um membro que exige a restringir apenas do `Object`, em seguida, a expressão de destino de invocação é reclassificada como um método de associação tardia de acesso (e recebe um erro se o tipo que contém o grupo de método é uma interface, ou se qualquer um dos os membros aplicáveis foram membros de extensão).</span><span class="sxs-lookup"><span data-stu-id="ff672-130">If the set contains more than one member which requires narrowing only from `Object`, then the invocation target expression is reclassified as a late-bound method access (and an error is given if the type containing the method group is an interface, or if any of the applicable members were extension members).</span></span>
    43. <span data-ttu-id="ff672-131">Se houver qualquer candidatos que exigem apenas o estreitamento de literais numéricos, em seguida, escolheu mais específica entre todos os candidatos restantes pelas etapas a seguir.</span><span class="sxs-lookup"><span data-stu-id="ff672-131">If there are any candidates that only require narrowing from numeric literals, then chose the most specific among all remaining candidates by the steps below.</span></span> <span data-ttu-id="ff672-132">Se o vencedor requer apenas estreitamento de literais numéricos, em seguida, ele é escolhido como o resultado da resolução de sobrecarga; Caso contrário, ele é um erro.</span><span class="sxs-lookup"><span data-stu-id="ff672-132">If the winner requires only narrowing from numeric literals, then it is picked as the result of overload resolution; otherwise it is an error.</span></span>

    <span data-ttu-id="ff672-133">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ff672-133">__Note.__</span></span> <span data-ttu-id="ff672-134">A justificativa para essa regra é que, se um programa é fracamente tipado (ou seja, a maioria ou todas as variáveis são declaradas como `Object`), a resolução de sobrecarga pode ser difícil quando várias conversões de `Object` são reduzidas.</span><span class="sxs-lookup"><span data-stu-id="ff672-134">The justification for this rule is that if a program is loosely-typed (that is, most or all variables are declared as `Object`), overload resolution can be difficult when many conversions from `Object` are narrowing.</span></span> <span data-ttu-id="ff672-135">Em vez de ter a resolução de sobrecarga falha em muitas situações (exigir a rigidez de tipos dos argumentos para a chamada de método), a resolução sobrecarregado apropriado método a ser chamado é adiado até o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ff672-135">Rather than have the overload resolution fail in many situations (requiring strong typing of the arguments to the method call), resolution the appropriate overloaded method to call is deferred until run time.</span></span> <span data-ttu-id="ff672-136">Isso permite que a chamada tipagem seja bem-sucedida sem conversões adicionais.</span><span class="sxs-lookup"><span data-stu-id="ff672-136">This allows the loosely-typed call to succeed without additional casts.</span></span> <span data-ttu-id="ff672-137">Um efeito colateral indesejado-isso, no entanto, é executar a chamada de associação tardia requer conversão para o destino de chamada `Object`.</span><span class="sxs-lookup"><span data-stu-id="ff672-137">An unfortunate side-effect of this, however, is that performing the late-bound call requires casting the call target to `Object`.</span></span> <span data-ttu-id="ff672-138">No caso de um valor de estrutura, isso significa que o valor deve ser boxed a um temporário.</span><span class="sxs-lookup"><span data-stu-id="ff672-138">In the case of a structure value, this means that the value must be boxed to a temporary.</span></span> <span data-ttu-id="ff672-139">Se o método chamado eventualmente tenta alterar um campo da estrutura, essa alteração serão perdida quando o método retorna.</span><span class="sxs-lookup"><span data-stu-id="ff672-139">If the method eventually called tries to change a field of the structure, this change will be lost once the method returns.</span></span> <span data-ttu-id="ff672-140">Interfaces são excluídos dessa regra especial porque a associação tardia sempre é resolvido em relação aos membros do tempo de execução classe ou estrutura de tipo, que pode ter nomes diferentes que os membros das interfaces que elas implementam.</span><span class="sxs-lookup"><span data-stu-id="ff672-140">Interfaces are excluded from this special rule because late binding always resolves against the members of the runtime class or structure type, which may have different names than the members of the interfaces they implement.</span></span>

5.  <span data-ttu-id="ff672-141">Em seguida, se nenhum método de instância permanecem no conjunto que não necessitam de redução, elimine todos os métodos de extensão do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-141">Next, if any instance methods remain in the set which do not require narrowing, then eliminate all extension methods from the set.</span></span> <span data-ttu-id="ff672-142">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-142">For example:</span></span>

    ```vb
    Imports System.Runtime.CompilerServices

    Class C3
        Sub M1(d As Integer)
        End Sub
    End Class

    Module C3Extensions
        <Extension> _
        Sub M1(c3 As C3, c As Long)
        End Sub

        <Extension> _
        Sub M1(c3 As C3, c As Short)
        End Sub
    End Module

    Module Test
        Sub Main()
            Dim c As New C3()
            Dim sVal As Short = 10
            Dim lVal As Long = 20

            ' Calls C3.M1, since C3.M1 is applicable.
            c.M1(sVal)

            ' Calls C3Extensions.M1 since C3.M1 requires a narrowing conversion
            c.M1(lVal)
        End Sub
    End Module
    ```

    <span data-ttu-id="ff672-143">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ff672-143">__Note.__</span></span> <span data-ttu-id="ff672-144">Métodos de extensão são ignorados, se houver métodos de instância aplicável para garantir que a adição de uma importação (que pode trazer novos métodos de extensão para o escopo) não causará uma chamada em um método de instância existente para associar novamente para um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="ff672-144">Extension methods are ignored if there are applicable instance methods to guarantee that adding an import (that might bring new extension methods into scope) will not cause a call on an existing instance method to rebind to an extension method.</span></span> <span data-ttu-id="ff672-145">Dado o escopo amplo de alguns métodos de extensão (ou seja, aquelas definidas em interfaces e/ou parâmetros de tipo), isso é uma abordagem mais segura para associação aos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="ff672-145">Given the broad scope of some extension methods (i.e. those defined on interfaces and/or type parameters), this is a safer approach to binding to extension methods.</span></span>

6.  <span data-ttu-id="ff672-146">Em seguida, if, considerando qualquer dois membros do conjunto de `M` e `N`, `M` é mais *específico* (seção [especificidade dos membros/tipos dada uma lista de argumentos](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) que `N`dada a lista de argumentos, eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-146">Next, if, given any two members of the set `M` and `N`, `M` is more *specific* (Section [Specificity of members/types given an argument list](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) than `N` given the argument list, eliminate `N` from the set.</span></span> <span data-ttu-id="ff672-147">Se mais de um membro permanece no conjunto e os membros restantes não são igualmente específicos recebe a lista de argumentos, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="ff672-147">If more than one member remains in the set and the remaining members are not equally specific given the argument list, a compile-time error results.</span></span>

7.  <span data-ttu-id="ff672-148">Caso contrário, considerando qualquer dois membros do conjunto de `M` e `N`, aplique as seguintes regras de desempate em ordem:</span><span class="sxs-lookup"><span data-stu-id="ff672-148">Otherwise, given any two members of the set, `M` and `N`, apply the following tie-breaking rules, in order:</span></span>

    71. <span data-ttu-id="ff672-149">Se `M` não tem um parâmetro ParamArray mas `N` faz, ou se ambos fazem mas `M` passa menos argumentos para o parâmetro ParamArray que `N` , em seguida, eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-149">If `M` does not have a ParamArray parameter but `N` does, or if both do but `M` passes fewer arguments into the ParamArray parameter than `N` does, then eliminate `N` from the set.</span></span> <span data-ttu-id="ff672-150">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-150">For example:</span></span>

        ```vb
        Module Test
            Sub F(a As Object, ParamArray b As Object())
                Console.WriteLine("F(Object, Object())")
            End Sub

            Sub F(a As Object, b As Object, ParamArray c As Object())
                Console.WriteLine("F(Object, Object, Object())")
            End Sub

           Sub G(Optional a As Object = Nothing)
              Console.WriteLine("G(Object)")
           End Sub

           Sub G(ParamArray a As Object())
              Console.WriteLine("G(Object())")
           End Sub    Sub Main()
                F(1)
                F(1, 2)
                F(1, 2, 3)
              G()
            End Sub
        End Module
        ```

        <span data-ttu-id="ff672-151">O exemplo acima produz a saída a seguir:</span><span class="sxs-lookup"><span data-stu-id="ff672-151">The above example produces the following output:</span></span>

        ```
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        <span data-ttu-id="ff672-152">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ff672-152">__Note.__</span></span> <span data-ttu-id="ff672-153">Quando uma classe declara um método com um parâmetro paramarray, não é incomum para incluir também algumas das formas expandidas como métodos regulares.</span><span class="sxs-lookup"><span data-stu-id="ff672-153">When a class declares a method with a paramarray parameter, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="ff672-154">Ao fazer isso, é possível evitar a alocação de uma matriz que ocorre quando uma forma expandida de um método com um parâmetro paramarray instância é invocada.</span><span class="sxs-lookup"><span data-stu-id="ff672-154">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a paramarray parameter is invoked.</span></span>

    72. <span data-ttu-id="ff672-155">Se `M` é definido em um tipo mais derivado `N`, eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-155">If `M` is defined in a more derived type than `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="ff672-156">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-156">For example:</span></span>

        ```vb
        Class Base
            Sub F(Of T, U)(x As T, y As U)
            End Sub
        End Class

        Class Derived
            Inherits Base

            Overloads Sub F(Of T, U)(x As U, y As T)
            End Sub
        End Class

        Module Test
            Sub Main()
                Dim d As New Derived()

                ' Calls Derived.F
                d.F(10, 10)
            End Sub
        End Module
        ```

        <span data-ttu-id="ff672-157">Essa regra também se aplica aos tipos de métodos de extensão são definidos no.</span><span class="sxs-lookup"><span data-stu-id="ff672-157">This rule also applies to the types that extension methods are defined on.</span></span> <span data-ttu-id="ff672-158">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-158">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Class Base
        End Class

        Class Derived
            Inherits Base
        End Class

        Module BaseExt
            <Extension> _
            Sub M(b As Base, x As Integer)
            End Sub
        End Module

        Module DerivedExt
            <Extension> _
            Sub M(d As Derived, x As Integer)
            End Sub
        End Module

        Module Test
            Sub Main()
                Dim b As New Base()
                Dim d As New Derived()

                ' Calls BaseExt.M
                b.M(10)

                ' Calls DerivedExt.M 
                d.M(10)
            End Sub
        End Module
        ```

    73. <span data-ttu-id="ff672-159">Se `M` e `N` são os métodos de extensão e o tipo de destino `M` é uma classe ou estrutura e o tipo de destino `N` é uma interface, eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-159">If `M` and `N` are extension methods and the target type of `M` is a class or structure and the target type of `N` is an interface, eliminate `N` from the set.</span></span> <span data-ttu-id="ff672-160">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-160">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Interface I1
        End Interface

        Class C1
            Implements I1
        End Class

        Module Ext1
            <Extension> _
            Sub M(i As I1, x As Integer)
            End Sub
        End Module

        Module Ext2
            <Extension> _
            Sub M(c As C1, y As Integer)
            End Sub
        End Module

        Module Test
            Sub Main()
                Dim c As New C1()

                ' Calls Ext2.M, because Ext1.M is hidden since it extends
                ' an interface.
                c.M(10)

                ' Calls Ext1.M
                CType(c, I1).M(10)
            End Sub
        End Module
        ```

    74. <span data-ttu-id="ff672-161">Se `M` e `N` são os métodos de extensão e o tipo de destino `M` e `N` sejam idênticos após a substituição de parâmetro de tipo e o tipo de destino `M` antes do tipo de substituição de parâmetro não contém Digite os parâmetros, mas o tipo de destino `N` , tem menos parâmetros de tipo que o tipo de destino `N`, eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-161">If `M` and `N` are extension methods, and the target type of `M` and `N` are identical after type parameter substitution, and the target type of `M` before type parameter substitution does not contain type parameters but the target type of `N` does, then has fewer type parameters than the target type of `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="ff672-162">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-162">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Module Module1
            Sub Main()
                Dim x As Integer = 1
                x.f(1) ' Calls first "f" extension method

                Dim y As New Dictionary(Of Integer, Integer)
                y.g(1) ' Ambiguity error
            End Sub

            <Extension()> Sub f(x As Integer, z As Integer)
            End Sub

            <Extension()> Sub f(Of T)(x As T, z As T)
            End Sub

            <Extension()> Sub g(Of T)(y As Dictionary(Of T, Integer), z As T)
            End Sub

            <Extension()> Sub g(Of T)(y As Dictionary(Of T, T), z As T)
            End Sub
        End Module
        ```

    75. <span data-ttu-id="ff672-163">Antes do tipo de argumentos tiverem sido substituídos, se `M` está *menos genérica* (seção [generalidade](overload-resolution.md#genericity)) que `N`, eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-163">Before type arguments have been substituted, if `M` is *less generic* (Section [Genericity](overload-resolution.md#genericity)) than `N`, eliminate `N` from the set.</span></span>

    76. <span data-ttu-id="ff672-164">Se `M` não é um método de extensão e `N` é, eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-164">If `M` is not an extension method and `N` is, eliminate `N` from the set.</span></span>

    77. <span data-ttu-id="ff672-165">Se `M` e `N` são métodos de extensão e `M` foi encontrado antes `N` (seção [coleção de método de extensão](overload-resolution.md#extension-method-collection)), eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-165">If `M` and `N` are extension methods and `M` was found before `N` (Section [Extension Method Collection](overload-resolution.md#extension-method-collection)), eliminate `N` from the set.</span></span> <span data-ttu-id="ff672-166">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-166">For example:</span></span>

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
                Sub M1(c As C1, y As Integer)
                End Sub
            End Module
        End Namespace

        Namespace N1.N2.N3
            Module Test
                Sub Main()
                    Dim x As New C1()

                    ' Calls N2C1Extensions.M1
                    x.M1(10)
                End Sub
            End Module
        End Namespace
        ```

        <span data-ttu-id="ff672-167">Se os métodos de extensão foram encontrados na mesma etapa, esses métodos de extensão são ambíguos.</span><span class="sxs-lookup"><span data-stu-id="ff672-167">If the extension methods were found in the same step, then those extension methods are ambiguous.</span></span> <span data-ttu-id="ff672-168">A chamada pode sempre ser a ambiguidade removida usando o nome do módulo padrão que contém o método de extensão e chamando o método de extensão como se fosse um membro regular.</span><span class="sxs-lookup"><span data-stu-id="ff672-168">The call may always be disambiguated using the name of the standard module containing the extension method and calling the extension method as if it was a regular member.</span></span> <span data-ttu-id="ff672-169">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-169">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Class C1
        End Class

        Module C1ExtA
            <Extension> _
            Sub M(c As C1)
            End Sub
        End Module

        Module C1ExtB
            <Extension> _
            Sub M(c As C1)
            End Sub
        End Module

        Module Main
            Sub Test()
                Dim c As New C1()

                C1.M()               ' Ambiguous between C1ExtA.M and BExtB.M
                C1ExtA.M(c)          ' Calls C1ExtA.M
                C1ExtB.M(c)          ' Calls C1ExtB.M
            End Sub
        End Module
        ```

    78. <span data-ttu-id="ff672-170">Se `M` e `N` ambos necessários inferência de tipo para produzir os argumentos de tipo, e `M` não exigia determinando o tipo dominante para qualquer um dos argumentos de tipo (ou seja, cada os argumentos de tipo inferidos para um único tipo), mas `N`fez, eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-170">If `M` and `N` both required type inference to produce type arguments, and `M` did not require determining the dominant type for any of its type arguments (i.e. each the type arguments inferred to a single type), but `N` did, eliminate `N` from the set.</span></span>

        <span data-ttu-id="ff672-171">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ff672-171">__Note.__</span></span> <span data-ttu-id="ff672-172">Essa regra garante que resolução de sobrecarga que teve êxito em versões anteriores (onde inferir vários tipos para um argumento de tipo causaria um erro), continue produzir os mesmos resultados.</span><span class="sxs-lookup"><span data-stu-id="ff672-172">This rule ensures that overload resolution that succeeded in previous versions (where inferring multiple types for a type argument would cause an error), continue to produce the same results.</span></span>

    79. <span data-ttu-id="ff672-173">Se a resolução de sobrecarga está sendo feita para resolver o destino de uma expressão de criação de delegado de um `AddressOf` expressão e tanto o delegado e `M` são funções enquanto `N` é uma sub-rotina, eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-173">If overload resolution is being done to resolve the target of a delegate-creation expression from an `AddressOf` expression, and both the delegate and `M` are functions while `N` is a subroutine, eliminate `N` from the set.</span></span> <span data-ttu-id="ff672-174">Da mesma forma, se tanto o delegado e `M` são as sub-rotinas, enquanto `N` é uma função, eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-174">Likewise, if both the delegate and `M` are subroutines, while `N` is a function, eliminate `N` from the set.</span></span>

    710. <span data-ttu-id="ff672-175">Se `M` não tiver usado os padrões de parâmetro opcional no lugar de argumentos explícitos, mas `N` , em seguida, fez, eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-175">If `M` did not use any optional parameter defaults in place of explicit arguments, but `N` did, then eliminate `N` from the set.</span></span>

    711. <span data-ttu-id="ff672-176">Antes do tipo de argumentos tiverem sido substituídos, se `M` tem *maior profundidade de generalidade* (seção [generalidade](overload-resolution.md#genericity)) que `N`, em seguida, eliminar `N` do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ff672-176">Before type arguments have been substituted, if `M` has *greater depth of genericity* (Section [Genericity](overload-resolution.md#genericity)) than `N`, then eliminate `N` from the set.</span></span>

8. <span data-ttu-id="ff672-177">Caso contrário, a chamada é ambígua e ocorrer um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ff672-177">Otherwise, the call is ambiguous and a compile-time error occurs.</span></span>

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a><span data-ttu-id="ff672-178">Especificidade dos membros/tipos dada uma lista de argumentos</span><span class="sxs-lookup"><span data-stu-id="ff672-178">Specificity of members/types given an argument list</span></span>

<span data-ttu-id="ff672-179">Um membro `M` é considerado *igualmente específicos* como `N`, dada uma lista de argumentos `A`, se suas assinaturas são as mesmas ou se cada parâmetro de tipo no `M` é o mesmo que o correspondente tipo de parâmetro em `N`.</span><span class="sxs-lookup"><span data-stu-id="ff672-179">A member `M` is considered *equally specific* as `N`, given an argument-list `A`, if their signatures are the same or if each parameter type in `M` is the same as the corresponding parameter type in `N`.</span></span>

<span data-ttu-id="ff672-180">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ff672-180">__Note.__</span></span> <span data-ttu-id="ff672-181">Dois membros podem terminar em um grupo de método com a mesma assinatura devido a métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="ff672-181">Two members can end up in a method group with the same signature due to extension methods.</span></span> <span data-ttu-id="ff672-182">Dois membros podem também ser igualmente específicos, mas não têm a mesma assinatura devido a parâmetros de tipo ou expansão paramarray.</span><span class="sxs-lookup"><span data-stu-id="ff672-182">Two members can also be equally specific but not have the same signature due to type parameters or paramarray expansion.</span></span>

<span data-ttu-id="ff672-183">Um membro `M` é considerado *mais específicos* que `N` se suas assinaturas são diferentes e pelo menos um parâmetro de tipo na `M` é mais específica que um tipo de parâmetro no `N`e nenhum tipo de parâmetro na `N` é mais específica que um tipo de parâmetro no `M`.</span><span class="sxs-lookup"><span data-stu-id="ff672-183">A member `M` is considered *more specific* than `N` if their signatures are different and at least one parameter type in `M` is more specific than a parameter type in `N`, and no parameter type in `N` is more specific than a parameter type in `M`.</span></span> <span data-ttu-id="ff672-184">Dado um par de parâmetros `Mj` e `Nj` que corresponde ao argumento `Aj`, o tipo de `Mj` é considerado *mais específicos* que o tipo do `Nj` se um dos seguintes condições for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="ff672-184">Given a pair of parameters `Mj` and `Nj` that matches an argument `Aj`, the type of `Mj` is considered *more specific* than the type of `Nj` if one of the following conditions is true:</span></span>

* <span data-ttu-id="ff672-185">Existe uma conversão de ampliação do tipo de `Mj` para o tipo `Nj`.</span><span class="sxs-lookup"><span data-stu-id="ff672-185">There exists a widening conversion from the type of `Mj` to the type `Nj`.</span></span> <span data-ttu-id="ff672-186">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ff672-186">(__Note.__</span></span> <span data-ttu-id="ff672-187">Porque os tipos de parâmetros estão sendo comparados sem levar em consideração o argumento real nesse caso, o conversão de ampliação de expressões de constante para um tipo numérico que do valor se encaixa não é considerado neste caso.)</span><span class="sxs-lookup"><span data-stu-id="ff672-187">Because parameters types are being compared without regard to the actual argument in this case, the widening conversion from constant expressions to a numeric type the value fits into is not considered in this case.)</span></span>

* <span data-ttu-id="ff672-188">`Aj` é o literal `0`, `Mj` é um tipo numérico e `Nj` é um tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="ff672-188">`Aj` is the literal `0`, `Mj` is a numeric type and `Nj` is an enumerated type.</span></span> <span data-ttu-id="ff672-189">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ff672-189">(__Note.__</span></span> <span data-ttu-id="ff672-190">Essa regra é necessária porque o literal `0` é ampliado para qualquer tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="ff672-190">This rule is necessary because the literal `0` widens to any enumerated type.</span></span> <span data-ttu-id="ff672-191">Uma vez que um tipo enumerado é ampliado para seu tipo subjacente, isso significa que resolução de sobrecarga em `0` será, por padrão, prefira tipos enumerados tipos numéricos.</span><span class="sxs-lookup"><span data-stu-id="ff672-191">Since an enumerated type widens to its underlying type, this means that overload resolution on `0` will, by default, prefer enumerated types over numeric types.</span></span> <span data-ttu-id="ff672-192">Recebemos muitos comentários que esse comportamento foi contra-intuitivo.)</span><span class="sxs-lookup"><span data-stu-id="ff672-192">We received a lot of feedback that this behavior was counterintuitive.)</span></span>

* <span data-ttu-id="ff672-193">`Mj` e `Nj` são ambos os tipos numéricos, e `Mj` vem anteriores ao `Nj` na lista `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`.</span><span class="sxs-lookup"><span data-stu-id="ff672-193">`Mj` and `Nj` are both numeric types, and `Mj` comes earlier than `Nj` in the list `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`.</span></span> <span data-ttu-id="ff672-194">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ff672-194">(__Note.__</span></span> <span data-ttu-id="ff672-195">A regra sobre os tipos numéricos é útil porque os tipos numéricos assinados e não assinados de um tamanho específico têm apenas reduzir conversões entre eles.</span><span class="sxs-lookup"><span data-stu-id="ff672-195">The rule about the numeric types is useful because the signed and unsigned numeric types of a particular size only have narrowing conversions between them.</span></span> <span data-ttu-id="ff672-196">A regra acima quebra o vínculo entre os dois tipos em favor do tipo numérico mais "natural".</span><span class="sxs-lookup"><span data-stu-id="ff672-196">The above rule breaks the tie between the two types in favor of the more "natural" numeric type.</span></span> <span data-ttu-id="ff672-197">Isso é particularmente importante quando fazendo a resolução de sobrecarga em um tipo que é ampliado para os dois os assinados e tipos numéricos de um tamanho específico, por exemplo, um literal numérico que se encaixa em ambos.)</span><span class="sxs-lookup"><span data-stu-id="ff672-197">This is particularly important when doing overload resolution on a type that widens to both the signed and unsigned numeric types of a particular size, for example, a numeric literal that fits into both.)</span></span>

* <span data-ttu-id="ff672-198">`Mj` e `Nj` é delegado tipos de função e o tipo de retorno `Mj` é mais específico do que o tipo de retorno `Nj` se `Aj` é classificado como um método lambda, e `Mj` ou `Nj` é `System.Linq.Expressions.Expression(Of T)`, em seguida, o argumento de tipo do tipo (supondo que ele é um tipo delegado) é substituído para o tipo que estão sendo comparado.</span><span class="sxs-lookup"><span data-stu-id="ff672-198">`Mj` and `Nj` are delegate function types and the return type of `Mj` is more specific than the return type of `Nj` If `Aj` is classified as a lambda method, and `Mj` or `Nj` is `System.Linq.Expressions.Expression(Of T)`, then the type argument of the type (assuming it is a delegate type) is substituted for the type being compared.</span></span>

* <span data-ttu-id="ff672-199">`Mj` é idêntico ao tipo de `Aj`, e `Nj` não é.</span><span class="sxs-lookup"><span data-stu-id="ff672-199">`Mj` is identical to the type of `Aj`, and `Nj` is not.</span></span> <span data-ttu-id="ff672-200">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ff672-200">(__Note.__</span></span> <span data-ttu-id="ff672-201">É interessante observar que a regra anterior é ligeiramente diferente do c#, em que c# requer que os tipos de função de delegado têm listas de parâmetro idênticos antes de comparar tipos de retorno, enquanto o Visual Basic não.)</span><span class="sxs-lookup"><span data-stu-id="ff672-201">It is interesting to note that the previous rule differs slightly from C#, in that C# requires that the delegate function types have identical parameter lists before comparing return types, while Visual Basic does not.)</span></span>

#### <a name="genericity"></a><span data-ttu-id="ff672-202">Generalidade</span><span class="sxs-lookup"><span data-stu-id="ff672-202">Genericity</span></span>

<span data-ttu-id="ff672-203">Um membro `M` é determinado como *menos genérica* de um membro `N` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ff672-203">A member `M` is determined to be *less generic* than a member `N` as follows:</span></span>

1. <span data-ttu-id="ff672-204">Se, para cada par de parâmetros de correspondência `Mj` e `Nj`, `Mj` é menor que ou igualmente genérico que `Nj` em relação aos parâmetros de tipo no método e pelo menos um `Mj` é menos genérica com respeito ao tipo parâmetros do método.</span><span class="sxs-lookup"><span data-stu-id="ff672-204">If, for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the method, and at least one `Mj` is less generic with respect to type parameters on the method.</span></span>
2. <span data-ttu-id="ff672-205">Caso contrário, se para cada par de parâmetros de correspondência `Mj` e `Nj`, `Mj` é menor que ou igualmente genérico que `Nj` em relação aos parâmetros de tipo no tipo e pelo menos um `Mj` é menos genérica com respeito ao tipo parâmetros do tipo, em seguida `M` é menos genérica que `N`.</span><span class="sxs-lookup"><span data-stu-id="ff672-205">Otherwise, if for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the type, and at least one `Mj` is less generic with respect to type parameters on the type, then `M` is less generic than `N`.</span></span>

<span data-ttu-id="ff672-206">Um parâmetro `M` é considerada igualmente genérico a um parâmetro `N` se seus tipos `Mt` e `Nt` fazem referência aos parâmetros de tipo ou ambos não faz referência aos parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="ff672-206">A parameter `M` is considered to be equally generic to a parameter `N` if their types `Mt` and `Nt` both refer to type parameters or both don't refer to type parameters.</span></span> <span data-ttu-id="ff672-207">`M` é considerado a ser menos genérica que `N` se `Mt` não faz referência a um parâmetro de tipo e `Nt` faz.</span><span class="sxs-lookup"><span data-stu-id="ff672-207">`M` is considered to be less generic than `N` if `Mt` does not refer to a type parameter and `Nt` does.</span></span>

<span data-ttu-id="ff672-208">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-208">For example:</span></span>

```vb
Class C1(Of T)
    Sub S1(Of U)(x As U, y As T)
    End Sub

    Sub S1(Of U)(x As U, y As U)
    End Sub

    Sub S2(x As Integer, y As T)
    End Sub

    Sub S2(x As T, y As T)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As C1(Of Integer) = New C1(Of Integer)

        x.S1(10, 10)    ' Calls S1(U, T)
        x.S2(10, 10)    ' Calls S2(Integer, T)
    End Sub
End Module
```

<span data-ttu-id="ff672-209">Parâmetros de tipo de método de extensão que foram corrigidos durante currying são considerados parâmetros de tipo no tipo, não os parâmetros de tipo no método.</span><span class="sxs-lookup"><span data-stu-id="ff672-209">Extension method type parameters that were fixed during currying are considered type parameters on the type, not type parameters on the method.</span></span> <span data-ttu-id="ff672-210">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-210">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M1(Of T, U)(x As T, y As U, z As U)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M1(Of T, U)(x As T, y As U, z As T)
    End Sub
End Module

Module Test
    Sub Main()
        Dim i As Integer = 10

        i.M1(10, 10)
    End Sub
End Module
```

#### <a name="depth-of-genericity"></a><span data-ttu-id="ff672-211">Profundidade de forma geral</span><span class="sxs-lookup"><span data-stu-id="ff672-211">Depth of genericity</span></span>

<span data-ttu-id="ff672-212">Um membro `M` é determinado como ter *maior profundidade de generalidade* de um membro `N` se, para cada par de parâmetros correspondentes `Mj` e `Nj`, `Mj` tem maior ou igual a *profundidade de generalidade* que `Nj`e pelo menos um `Mj` é tem maior profundidade de forma geral.</span><span class="sxs-lookup"><span data-stu-id="ff672-212">A member `M` is determined to have *greater depth of genericity* than a member `N` if, for each pair of matching parameters  `Mj` and `Nj`, `Mj` has greater or equal *depth of genericity* than `Nj`, and at least one `Mj` is has greater depth of genericity.</span></span> <span data-ttu-id="ff672-213">Profundidade de generalidade é definida da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ff672-213">Depth of genericity is defined as follows:</span></span>

* <span data-ttu-id="ff672-214">Algo diferente de um parâmetro de tipo tem maior profundidade de generalidade de um parâmetro de tipo;</span><span class="sxs-lookup"><span data-stu-id="ff672-214">Anything other than a type parameter has greater depth of genericity than a type parameter;</span></span>

* <span data-ttu-id="ff672-215">Recursivamente, um tipo construído tem maior profundidade de generalidade que outro tipo construído (com o mesmo número de argumentos de tipo) se pelo menos um argumento de tipo tem uma profundidade maior de generalidade e nenhum argumento de tipo menor profundidade que o tipo correspondente argumento na outra.</span><span class="sxs-lookup"><span data-stu-id="ff672-215">Recursively, a constructed type has greater depth of genericity than another constructed type (with the same number of type arguments) if at least one type argument has greater depth of genericity and no type argument has less depth than the corresponding type argument in the other.</span></span>

* <span data-ttu-id="ff672-216">Um tipo de matriz tem maior profundidade de generalidade que outro tipo de matriz (com o mesmo número de dimensões), se o tipo de elemento da primeira tem maior profundidade de forma geral que o tipo de elemento da segunda.</span><span class="sxs-lookup"><span data-stu-id="ff672-216">An array type has greater depth of genericity than another array type (with the same number of dimensions) if the element type of the first has greater depth of genericity than the element type of the second.</span></span>

<span data-ttu-id="ff672-217">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-217">For example:</span></span>

```vb
Module Test

    Sub f(Of T)(x As Task(Of T))
    End Sub

    Sub f(Of T)(x As T)
    End Sub

    Sub Main()
        Dim x As Task(Of Integer) = Nothing
        f(x)            ' Calls the first overload
    End Sub
End Module
```

### <a name="applicability-to-argument-list"></a><span data-ttu-id="ff672-218">Aplicabilidade à lista de argumentos</span><span class="sxs-lookup"><span data-stu-id="ff672-218">Applicability To Argument List</span></span>

<span data-ttu-id="ff672-219">É um método *aplicável* a um conjunto de argumentos nomeados, se o método pode ser chamado usando as listas de argumento, argumentos posicionais e argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="ff672-219">A method is *applicable* to a set of type arguments, positional arguments, and named arguments if the method can be invoked using the argument lists.</span></span> <span data-ttu-id="ff672-220">As listas são comparadas com o parâmetro de listas de argumentos da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ff672-220">The argument lists are matched against the parameter lists as follows:</span></span>

1. <span data-ttu-id="ff672-221">Primeiro, corresponde cada argumento posicional na ordem à lista de parâmetros de método.</span><span class="sxs-lookup"><span data-stu-id="ff672-221">First, match each positional argument in order to the list of method parameters.</span></span> <span data-ttu-id="ff672-222">Se houver mais argumentos posicionais que os parâmetros e o último parâmetro não é um paramarray, o método não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="ff672-222">If there are more positional arguments than parameters and the last parameter is not a paramarray, the method is not applicable.</span></span> <span data-ttu-id="ff672-223">Caso contrário, o parâmetro paramarray é expandido com parâmetros do tipo de elemento paramarray para corresponder ao número de argumentos posicionais.</span><span class="sxs-lookup"><span data-stu-id="ff672-223">Otherwise, the paramarray parameter is expanded with parameters of the paramarray element type to match the number of positional arguments.</span></span> <span data-ttu-id="ff672-224">Se um argumento posicional for omitido, que deveria ir para um paramarray, o método não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="ff672-224">If a positional argument is omitted that would go into a paramarray, the method is not applicable.</span></span>
2. <span data-ttu-id="ff672-225">Em seguida, corresponde cada argumento nomeado para um parâmetro com o nome fornecido.</span><span class="sxs-lookup"><span data-stu-id="ff672-225">Next, match each named argument to a parameter with the given name.</span></span> <span data-ttu-id="ff672-226">Se um dos argumentos nomeados não corresponde, corresponde a um parâmetro paramarray ou corresponde a um argumento que já foi correspondido com o outro argumento posicional ou nomeado, o método não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="ff672-226">If one of the named arguments fails to match, matches a paramarray parameter, or matches an argument already matched with another positional or named argument, the method is not applicable.</span></span>
3. <span data-ttu-id="ff672-227">Em seguida, se tiverem sido especificados argumentos de tipo, eles são correspondidos em relação à lista de parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="ff672-227">Next, if type arguments have been specified, they are matched against the type parameter list .</span></span> <span data-ttu-id="ff672-228">Se as duas listas não tiverem o mesmo número de elementos, o método não é aplicável, a menos que a lista de argumentos de tipo está vazia.</span><span class="sxs-lookup"><span data-stu-id="ff672-228">If the two lists do not have the same number of elements, the method is not applicable, unless the type argument list is empty.</span></span> <span data-ttu-id="ff672-229">Se a lista de argumentos de tipo estiver vazia, inferência de tipo é usada para tentar inferir a lista de argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="ff672-229">If the type argument list is empty, type inference is used to try and infer the type argument list.</span></span> <span data-ttu-id="ff672-230">Se a inferência de tipo falhar, o método não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="ff672-230">If type inferencing fails, the method is not applicable.</span></span> <span data-ttu-id="ff672-231">Caso contrário, os argumentos de tipo são preenchidos no lugar de parâmetros de tipo na assinatura. Se os parâmetros que não foram correspondidos não são opcionais, o método não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="ff672-231">Otherwise, the type arguments are filled in the place of the type parameters in the signature.If parameters that have not been matched are not optional, the method is not applicable.</span></span>
4. <span data-ttu-id="ff672-232">Se as expressões do argumento não são implicitamente conversíveis para os tipos dos parâmetros forem iguais, então o método não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="ff672-232">If the argument expressions are not implicitly convertible to the types of the parameters they match, then the method is not applicable.</span></span>
5. <span data-ttu-id="ff672-233">Se um parâmetro é ByRef, e não há uma conversão implícita do tipo do parâmetro para o tipo do argumento, o método não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="ff672-233">If a parameter is ByRef, and there is not an implicit conversion from the type of the parameter to the type of the argument, then the method is not applicable.</span></span>
6. <span data-ttu-id="ff672-234">Se os argumentos de tipo violam as restrições do método (incluindo os argumentos de tipo inferido da etapa 3), o método não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="ff672-234">If type arguments violate the method's constraints (including the inferred type arguments from Step 3), the method is not applicable.</span></span> <span data-ttu-id="ff672-235">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-235">For example:</span></span>

```vb
Module Module1
    Sub Main()
        f(Of Integer)(New Exception)
        ' picks the first overload (narrowing),
        ' since the second overload (widening) violates constraints 
    End Sub

    Sub f(Of T)(x As IComparable)
    End Sub

    Sub f(Of T As Class)(x As Object)
    End Sub
End Module
```

<span data-ttu-id="ff672-236">Se uma expressão de argumento único corresponde a um parâmetro paramarray e o tipo da expressão do argumento é conversível para o tipo do parâmetro paramarray e o tipo de elemento paramarray, o método é aplicável em ambas as suas expandidas e não expandidas formas, com duas exceções.</span><span class="sxs-lookup"><span data-stu-id="ff672-236">If a single argument expression matches a paramarray parameter and the type of the argument expression is convertible to both the type of the paramarray parameter and the paramarray element type, the method is applicable in both its expanded and unexpanded forms, with two exceptions.</span></span> <span data-ttu-id="ff672-237">Se a conversão do tipo da expressão do argumento para o tipo de paramarray é restritiva, em seguida, o método só é aplicável em sua forma expandida.</span><span class="sxs-lookup"><span data-stu-id="ff672-237">If the conversion from the type of the argument expression to the paramarray type is narrowing, then the method is only applicable in its expanded form.</span></span> <span data-ttu-id="ff672-238">Se a expressão de argumento é o literal `Nothing`, em seguida, o método só é aplicável em sua forma não expandida.</span><span class="sxs-lookup"><span data-stu-id="ff672-238">If the argument expression is the literal `Nothing`, then the method is only applicable in its unexpanded form.</span></span> <span data-ttu-id="ff672-239">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ff672-239">For example:</span></span>

```vb
Module Test
    Sub F(ParamArray a As Object())
        Dim o As Object

        For Each o In a
            Console.Write(o.GetType().FullName)
            Console.Write(" ")
        Next o
        Console.WriteLine()
    End Sub

    Sub Main()
        Dim a As Object() = { 1, "Hello", 123.456 }
        Dim o As Object = a

        F(a)
        F(CType(a, Object))
        F(o)
        F(CType(o, Object()))
    End Sub
End Module
```

<span data-ttu-id="ff672-240">O exemplo acima produz a saída a seguir:</span><span class="sxs-lookup"><span data-stu-id="ff672-240">The above example produces the following output:</span></span>

```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="ff672-241">Nas primeiros e últimas invocações do `F`, a forma normal de `F` é aplicável porque uma conversão de ampliação existe do tipo de argumento para o tipo de parâmetro (ambos são do tipo `Object()`), e o argumento é passado como um valor comum parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ff672-241">In the first and last invocations of `F`, the normal form of `F` is applicable because a widening conversion exists from the argument type to the parameter type (both are of type `Object()`), and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="ff672-242">Em que as invocações de segunda e terceira, a forma normal de `F` não é aplicável porque nenhuma conversão de ampliação existe do tipo de argumento para o tipo de parâmetro (conversões de `Object` para `Object()` são reduzidas).</span><span class="sxs-lookup"><span data-stu-id="ff672-242">In the second and third invocations, the normal form of `F` is not applicable because no widening conversion exists from the argument type to the parameter type (conversions from `Object` to `Object()` are narrowing).</span></span> <span data-ttu-id="ff672-243">No entanto, a forma expandida do `F` é aplicável e um elemento de um `Object()` é criado pela invocação.</span><span class="sxs-lookup"><span data-stu-id="ff672-243">However, the expanded form of `F` is applicable, and a one-element `Object()` is created by the invocation.</span></span> <span data-ttu-id="ff672-244">O único elemento da matriz é inicializado com o valor do argumento fornecido (que por si só é uma referência a um `Object()`).</span><span class="sxs-lookup"><span data-stu-id="ff672-244">The single element of the array is initialized with the given argument value (which itself is a reference to an `Object()`).</span></span>

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a><span data-ttu-id="ff672-245">Passando argumentos e argumentos para parâmetros opcionais de separação</span><span class="sxs-lookup"><span data-stu-id="ff672-245">Passing Arguments, and Picking Arguments for Optional Parameters</span></span>

<span data-ttu-id="ff672-246">Se um parâmetro é um parâmetro de valor, a expressão do argumento correspondente deve ser classificada como um valor.</span><span class="sxs-lookup"><span data-stu-id="ff672-246">If a parameter is a value parameter, the matching argument expression must be classified as a value.</span></span> <span data-ttu-id="ff672-247">O valor é convertido para o tipo do parâmetro e passado como o parâmetro em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ff672-247">The value is converted to the type of the parameter and passed in as the parameter at run time.</span></span> <span data-ttu-id="ff672-248">Se o parâmetro é um parâmetro de referência e a expressão do argumento correspondente é classificada como uma variável cujo tipo é o mesmo que o parâmetro, em seguida, uma referência à variável é passada como o parâmetro em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ff672-248">If the parameter is a reference parameter and the matching argument expression is classified as a variable whose type is the same as the parameter, then a reference to the variable is passed in as the parameter at run time.</span></span>

<span data-ttu-id="ff672-249">Caso contrário, se a expressão do argumento correspondente é classificada como uma variável, o valor ou o acesso de propriedade, uma variável temporária do tipo do parâmetro é alocada.</span><span class="sxs-lookup"><span data-stu-id="ff672-249">Otherwise, if the matching argument expression is classified as a variable, value, or property access, then a temporary variable of the type of the parameter is allocated.</span></span> <span data-ttu-id="ff672-250">Antes da chamada do método em tempo de execução, a expressão de argumento é reclassificada como um valor, convertida para o tipo do parâmetro e atribuída à variável temporária.</span><span class="sxs-lookup"><span data-stu-id="ff672-250">Before the method invocation at run time, the argument expression is reclassified as a value, converted to the type of the parameter, and assigned to the temporary variable.</span></span> <span data-ttu-id="ff672-251">Em seguida, uma referência à variável temporária é passada como o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ff672-251">Then a reference to the temporary variable is passed in as the parameter.</span></span> <span data-ttu-id="ff672-252">Depois que a chamada do método é avaliada, se a expressão de argumento é classificada como um acesso de variável ou propriedade, variável temporária é atribuído para a expressão de variável ou expressão de acesso de propriedade.</span><span class="sxs-lookup"><span data-stu-id="ff672-252">After the method invocation is evaluated, if the argument expression is classified as a variable or property access, the temporary variable is assigned to the variable expression or the property access expression.</span></span> <span data-ttu-id="ff672-253">Se a expressão de acesso de propriedade não tiver nenhum `Set` acessador e, em seguida, a atribuição não é executada.</span><span class="sxs-lookup"><span data-stu-id="ff672-253">If the property access expression has no `Set` accessor, then the assignment is not performed.</span></span>

<span data-ttu-id="ff672-254">Para parâmetros opcionais em que um argumento não foi fornecido, o compilador escolhe argumentos, conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="ff672-254">For optional parameters where an argument has not been provided, the compiler picks arguments as described below.</span></span> <span data-ttu-id="ff672-255">Em todos os casos ele testa o tipo de parâmetro após a substituição de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="ff672-255">In all cases it tests against the parameter type after generic type substitution.</span></span>

* <span data-ttu-id="ff672-256">Se o parâmetro opcional tem o atributo `System.Runtime.CompilerServices.CallerLineNumber`e a invocação é de um local no código-fonte e um literal numérico que representa o número da linha do local tem uma conversão intrínseco para o tipo de parâmetro, então o literal numérico é usado.</span><span class="sxs-lookup"><span data-stu-id="ff672-256">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerLineNumber`, and the invocation is from a location in source code, and a numeric literal representing that location's line number has an intrinsic conversion to the parameter type, then the numeric literal is used.</span></span> <span data-ttu-id="ff672-257">Se a invocação abrange várias linhas, a opção de linha a ser usada é dependente da implementação.</span><span class="sxs-lookup"><span data-stu-id="ff672-257">If the invocation spans multiple lines, then the choice of which line to use is implementation-dependent.</span></span>

* <span data-ttu-id="ff672-258">Se o parâmetro opcional tem o atributo `System.Runtime.CompilerServices.CallerFilePath`e a invocação é de um local no código-fonte e um literal de cadeia de caracteres que representa o caminho do arquivo do local tem uma conversão intrínseco para o tipo de parâmetro, o literal de cadeia de caracteres é usado.</span><span class="sxs-lookup"><span data-stu-id="ff672-258">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerFilePath`, and the invocation is from a location in source code, and a string literal representing that location's file path has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="ff672-259">O formato do caminho do arquivo é dependente da implementação.</span><span class="sxs-lookup"><span data-stu-id="ff672-259">The format of the file path is implementation-dependent.</span></span>

* <span data-ttu-id="ff672-260">Se o parâmetro opcional tem o atributo `System.Runtime.CompilerServices.CallerMemberName`e a invocação é dentro do corpo de um membro de tipo ou em um atributo aplicado a qualquer parte desse membro de tipo e um literal de cadeia de caracteres que representa o nome do membro que tem uma conversão intrínseco para o parâmetro Digite, em seguida, o literal de cadeia de caracteres é usado.</span><span class="sxs-lookup"><span data-stu-id="ff672-260">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerMemberName`, and the invocation is within the body of a type member or in an attribute applied to any part of that type member, and a string literal representing that member name has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="ff672-261">Para invocações que fazem parte de acessadores de propriedade ou manipuladores de eventos personalizados, em seguida, o nome do membro usado é da propriedade ou evento em si.</span><span class="sxs-lookup"><span data-stu-id="ff672-261">For invocations that are part of property accessors or custom event handlers, then the member name used is that of the property or event itself.</span></span> <span data-ttu-id="ff672-262">Para invocações que fazem parte de um operador ou construtor, em seguida, é usado um nome específico da implementação.</span><span class="sxs-lookup"><span data-stu-id="ff672-262">For invocations that are part of an operator or constructor, then an implementation-specific name is used.</span></span>

<span data-ttu-id="ff672-263">Se nenhum dos acima para aplicar, em seguida, valor padrão do parâmetro opcional é usado (ou `Nothing` se nenhum valor padrão é fornecido).</span><span class="sxs-lookup"><span data-stu-id="ff672-263">If none of the above apply, then the optional parameter's default value is used (or `Nothing` if no default value is supplied).</span></span> <span data-ttu-id="ff672-264">E se mais de um dos acima para aplicar, em seguida, a escolha de qual deles usar depende da implementação.</span><span class="sxs-lookup"><span data-stu-id="ff672-264">And if more than one of the above apply, then the choice of which to use is implementation-dependent.</span></span>

<span data-ttu-id="ff672-265">O `CallerLineNumber` e `CallerFilePath` atributos são úteis para registro em log.</span><span class="sxs-lookup"><span data-stu-id="ff672-265">The `CallerLineNumber` and `CallerFilePath` attributes are useful for logging.</span></span> <span data-ttu-id="ff672-266">O `CallerMemberName` é útil para implementar `INotifyPropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="ff672-266">The `CallerMemberName` is useful for implementing `INotifyPropertyChanged`.</span></span> <span data-ttu-id="ff672-267">Estes são exemplos.</span><span class="sxs-lookup"><span data-stu-id="ff672-267">Here are examples.</span></span>

```vb
Sub Log(msg As String,
        <CallerFilePath> Optional file As String = Nothing,
        <CallerLineNumber> Optional line As Integer? = Nothing)
    Console.WriteLine("{0}:{1} - {2}", file, line, msg)
End Sub

WriteOnly Property p As Integer
    Set(value As Integer)
        Notify(_p, value)
    End Set
End Property

Private _p As Integer

Sub Notify(Of T As IEquatable(Of T))(ByRef v1 As T, v2 As T,
        <CallerMemberName> Optional prop As String = Nothing)
    If v1 IsNot Nothing AndAlso v1.Equals(v2) Then Return
    If v1 Is Nothing AndAlso v2 Is Nothing Then Return
    v1 = v2
    RaiseEvent PropertyChanged(Me, New PropertyChangedEventArgs(prop))
End Sub
```

<span data-ttu-id="ff672-268">Além dos parâmetros opcionais acima, o Microsoft Visual Basic também reconhece alguns parâmetros opcionais adicionais se eles são importados dos metadados (ou seja, de uma referência DLL):</span><span class="sxs-lookup"><span data-stu-id="ff672-268">In addition to the optional parameters above, Microsoft Visual Basic also recognizes some additional optional parameters if they are imported from metadata (i.e. from a DLL reference):</span></span>

* <span data-ttu-id="ff672-269">Após a importação dos metadados, o Visual Basic também trata o parâmetro `<Optional>` como uma indicação que o parâmetro é opcional: dessa forma, é possível importar uma declaração que tem um parâmetro opcional, mas nenhum valor padrão, mesmo que isso não pode ser expressa usando o `Optional` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="ff672-269">Upon importing from metadata, Visual Basic also treats the parameter `<Optional>` as indicative that the parameter is optional: in this way it is possible to import a declaration which has an optional parameter but no default value, even though this can't be expressed using the `Optional` keyword.</span></span>

* <span data-ttu-id="ff672-270">Se o parâmetro opcional tem o atributo `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`e o valor numérico literal 1 ou 0 tem uma conversão para o tipo de parâmetro e, em seguida, o compilador usa como argumento o literal 1 se `Option Compare Text` está em vigor ou os 0 se o literal `Optional Compare Binary` está em vigor.</span><span class="sxs-lookup"><span data-stu-id="ff672-270">If the optional parameter has the attribute `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`, and the numeric literal 1 or 0 has a conversion to the parameter type, then the compiler uses as argument either the literal 1 if `Option Compare Text` is in effect, or the literal 0 if `Optional Compare Binary` is in effect.</span></span>

* <span data-ttu-id="ff672-271">Se o parâmetro opcional tem o atributo `System.Runtime.CompilerServices.IDispatchConstantAttribute`, e ele tem o tipo `Object`e ele não especifica um valor padrão e, em seguida, o compilador usa o argumento `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.</span><span class="sxs-lookup"><span data-stu-id="ff672-271">If the optional parameter has the attribute `System.Runtime.CompilerServices.IDispatchConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.</span></span>

* <span data-ttu-id="ff672-272">Se o parâmetro opcional tem o atributo `System.Runtime.CompilerServices.IUnknownConstantAttribute`, e ele tem o tipo `Object`e ele não especifica um valor padrão e, em seguida, o compilador usa o argumento `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.</span><span class="sxs-lookup"><span data-stu-id="ff672-272">If the optional parameter has the attribute `System.Runtime.CompilerServices.IUnknownConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.</span></span>

* <span data-ttu-id="ff672-273">Se o parâmetro opcional tem um tipo `Object`e ele não especifica um valor padrão e, em seguida, o compilador usa o argumento `System.Reflection.Missing.Value`.</span><span class="sxs-lookup"><span data-stu-id="ff672-273">If the optional parameter has type `Object`, and it does not specify a default value, then the compiler uses the argument `System.Reflection.Missing.Value`.</span></span>

### <a name="conditional-methods"></a><span data-ttu-id="ff672-274">Métodos condicionais</span><span class="sxs-lookup"><span data-stu-id="ff672-274">Conditional Methods</span></span>

<span data-ttu-id="ff672-275">Se o método de destino ao qual se refere a uma expressão de invocação é uma sub-rotina que não é um membro de uma interface e se o método tem um ou mais `System.Diagnostics.ConditionalAttribute` atributos, avaliação da expressão depende de constantes de compilação condicional definidas no Esse ponto no arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="ff672-275">If the target method to which an invocation expression refers is a subroutine that is not a member of an interface and if the method has one or more `System.Diagnostics.ConditionalAttribute` attributes, evaluation of the expression depends on the conditional compilation constants defined at that point in the source file.</span></span> <span data-ttu-id="ff672-276">Cada instância do atributo que especifica uma cadeia de caracteres que nomeia uma constante de compilação condicional.</span><span class="sxs-lookup"><span data-stu-id="ff672-276">Each instance of the attribute specifies a string, which names a conditional compilation constant.</span></span> <span data-ttu-id="ff672-277">Cada constante de compilação condicional é avaliada como se fosse parte de uma instrução de compilação condicional.</span><span class="sxs-lookup"><span data-stu-id="ff672-277">Each conditional compilation constant is evaluated as if it were part of a conditional compilation statement.</span></span> <span data-ttu-id="ff672-278">Se a constante é avaliada como `True`, a expressão é avaliada normalmente em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ff672-278">If the constant evaluates to `True`, the expression is evaluated normally at run time.</span></span> <span data-ttu-id="ff672-279">Se a constante é avaliada como `False`, a expressão não será avaliada.</span><span class="sxs-lookup"><span data-stu-id="ff672-279">If the constant evaluates to `False`, the expression is not evaluated at all.</span></span>

<span data-ttu-id="ff672-280">Quando você está procurando o atributo, a declaração mais derivada de um método substituível é verificada.</span><span class="sxs-lookup"><span data-stu-id="ff672-280">When looking for the attribute, the most derived declaration of an overridable method is checked.</span></span>

<span data-ttu-id="ff672-281">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ff672-281">__Note.__</span></span> <span data-ttu-id="ff672-282">O atributo não é válido em funções ou métodos de interface e será ignorado se especificado em um desses tipos de método.</span><span class="sxs-lookup"><span data-stu-id="ff672-282">The attribute is not valid on functions or interface methods and is ignored if specified on either kind of method.</span></span> <span data-ttu-id="ff672-283">Assim, os métodos condicionais só serão exibidas nas instruções de invocação.</span><span class="sxs-lookup"><span data-stu-id="ff672-283">Thus, conditional methods will only appear in invocation statements.</span></span>

### <a name="type-argument-inference"></a><span data-ttu-id="ff672-284">Inferência de argumento de tipo</span><span class="sxs-lookup"><span data-stu-id="ff672-284">Type Argument Inference</span></span>

<span data-ttu-id="ff672-285">Quando um método com parâmetros de tipo é chamado sem especificar argumentos de tipo *inferência de argumento* é usado para tentar inferir argumentos de tipo para a chamada.</span><span class="sxs-lookup"><span data-stu-id="ff672-285">When a method with type parameters is called without specifying type arguments, *type argument inference* is used to try and infer type arguments for the call.</span></span> <span data-ttu-id="ff672-286">Isso permite uma sintaxe mais natural a ser usado para chamar um método com parâmetros de tipo quando os argumentos de tipo podem ser inferidos superficialmente.</span><span class="sxs-lookup"><span data-stu-id="ff672-286">This allows a more natural syntax to be used for calling a method with type parameters when the type arguments can be trivially inferred.</span></span> <span data-ttu-id="ff672-287">Por exemplo, dada a seguinte declaração de método:</span><span class="sxs-lookup"><span data-stu-id="ff672-287">For example, given the following method declaration:</span></span>

```vb
Module Util
    Function Choose(Of T)(b As Boolean, first As T, second As T) As T
        If b Then
            Return first
        Else
            Return second
        End If
    End Function
End Class
```

<span data-ttu-id="ff672-288">é possível invocar o `Choose` método sem especificar explicitamente um argumento de tipo:</span><span class="sxs-lookup"><span data-stu-id="ff672-288">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

<span data-ttu-id="ff672-289">Por meio de inferência de argumento de tipo, os argumentos de tipo `Integer` e `String` são determinados dos argumentos para o método.</span><span class="sxs-lookup"><span data-stu-id="ff672-289">Through type argument inference, the type arguments `Integer` and `String` are determined from the arguments to the method.</span></span>

<span data-ttu-id="ff672-290">Ocorre a inferência de argumento de tipo *antes de* reclassificação da expressão é executada em métodos de lambda ou ponteiros de método na lista de argumentos, uma vez que a reclassificação desses dois tipos de expressões pode exigir o tipo dos parâmetro a ser conhecido.</span><span class="sxs-lookup"><span data-stu-id="ff672-290">Type argument inference occurs *before* expression reclassification is performed on lambda methods or method pointers in the argument list, since reclassification of those two kinds of expressions may require the type of the parameter to be known.</span></span>  <span data-ttu-id="ff672-291">Dado um conjunto de argumentos `A1,...,An`, um conjunto de parâmetros de correspondência `P1,...,Pn` e um conjunto de método parâmetros de tipo `T1,...,Tn`, as dependências entre os argumentos e os parâmetros de tipo de método são coletadas primeiro da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ff672-291">Given a set of arguments `A1,...,An`, a set of matching parameters `P1,...,Pn` and a set of method type parameters `T1,...,Tn`, the dependencies between the arguments and method type parameters are first collected as follows:</span></span>

* <span data-ttu-id="ff672-292">Se `An` é o `Nothing` literal, sem dependências são geradas.</span><span class="sxs-lookup"><span data-stu-id="ff672-292">If `An` is the `Nothing` literal, no dependencies are generated.</span></span>

* <span data-ttu-id="ff672-293">Se `An` é um método lambda e o tipo de `Pn` é um tipo delegado construído ou `System.Linq.Expressions.Expression(Of T)`, onde `T` é um tipo de delegado construído,</span><span class="sxs-lookup"><span data-stu-id="ff672-293">If `An` is a lambda method and the type of `Pn` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type,</span></span>

* <span data-ttu-id="ff672-294">Se o tipo de um parâmetro de método lambda será inferido do tipo do parâmetro correspondente `Pn`, e o tipo do parâmetro depende de um parâmetro de tipo do método `Tn`, em seguida, `An` tem uma dependência no `Tn`.</span><span class="sxs-lookup"><span data-stu-id="ff672-294">If the type of a lambda method parameter will be inferred from the type of the corresponding parameter `Pn`, and the type of the parameter depends on a method type parameter `Tn`, then `An` has a dependency on `Tn`.</span></span>

* <span data-ttu-id="ff672-295">Se o tipo de um parâmetro de método lambda é especificado e o tipo do parâmetro correspondente `Pn` depende de um parâmetro de tipo do método `Tn`, em seguida, `Tn` tem uma dependência no `An`.</span><span class="sxs-lookup"><span data-stu-id="ff672-295">If the type of a lambda method parameter is specified and the type of the corresponding parameter `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="ff672-296">Se o tipo de retorno de `Pn` depende de um parâmetro de tipo do método `Tn`, em seguida, `Tn` tem uma dependência no `An`.</span><span class="sxs-lookup"><span data-stu-id="ff672-296">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="ff672-297">Se `An` é um ponteiro de método e o tipo de `Pn` é um tipo de delegado construído,</span><span class="sxs-lookup"><span data-stu-id="ff672-297">If `An` is a method pointer and the type of `Pn` is a constructed delegate type,</span></span>

* <span data-ttu-id="ff672-298">Se o tipo de retorno de `Pn` depende de um parâmetro de tipo do método `Tn`, em seguida, `Tn` tem uma dependência no `An`.</span><span class="sxs-lookup"><span data-stu-id="ff672-298">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="ff672-299">Se `Pn` é um tipo construído e o tipo de `Pn` depende de um parâmetro de tipo de método `Tn`, em seguida, `Tn` tem uma dependência no `An`.</span><span class="sxs-lookup"><span data-stu-id="ff672-299">If `Pn` is a constructed type and the type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="ff672-300">Caso contrário, nenhuma dependência é gerada.</span><span class="sxs-lookup"><span data-stu-id="ff672-300">Otherwise, no dependency is generated.</span></span>

<span data-ttu-id="ff672-301">Depois de coletar dependências, os argumentos que não têm dependências são eliminados.</span><span class="sxs-lookup"><span data-stu-id="ff672-301">After collecting dependencies, any arguments that have no dependencies are eliminated.</span></span> <span data-ttu-id="ff672-302">Se quaisquer parâmetros de tipo de método não tem nenhuma dependência de saída (ou seja, o parâmetro de tipo de método não depende de um argumento), em seguida, inferência de tipo falhará.</span><span class="sxs-lookup"><span data-stu-id="ff672-302">If any method type parameters have no outgoing dependencies (i.e. the method type parameter does not depend on an argument), then type inference fails.</span></span> <span data-ttu-id="ff672-303">Caso contrário, os argumentos restantes e os parâmetros de tipo de método são agrupados em componentes fortemente conectados.</span><span class="sxs-lookup"><span data-stu-id="ff672-303">Otherwise, the remaining arguments and method type parameters are grouped into strongly connected components.</span></span> <span data-ttu-id="ff672-304">Um componente fortemente conectado é um conjunto de argumentos e parâmetros de tipo de método, em que qualquer elemento no componente está acessível por meio de dependências em outros elementos.</span><span class="sxs-lookup"><span data-stu-id="ff672-304">A strongly connected component is a set of arguments and method type parameters, where any element in the component is reachable via dependencies on other elements.</span></span>

<span data-ttu-id="ff672-305">Os componentes fortemente conectados são, em seguida, topologicamente classificados e processados em ordem topológica:</span><span class="sxs-lookup"><span data-stu-id="ff672-305">The strongly connected components are then topologically sorted and processed in topological order:</span></span>

* <span data-ttu-id="ff672-306">Se o componente com rigidez de tipos contiver apenas um elemento,</span><span class="sxs-lookup"><span data-stu-id="ff672-306">If the strongly typed component contains only one element,</span></span>

  * <span data-ttu-id="ff672-307">Se o elemento já foi marcado completo, ignorá-la.</span><span class="sxs-lookup"><span data-stu-id="ff672-307">If the element has already been marked complete, skip it.</span></span>

  * <span data-ttu-id="ff672-308">Se o elemento é um argumento, adicione dicas de tipo do argumento para os parâmetros de tipo de método que dependem dele e marcam o elemento como concluído.</span><span class="sxs-lookup"><span data-stu-id="ff672-308">If the element is an argument, then add type hints from the argument to the method type parameters that depend on it and mark the element as complete.</span></span> <span data-ttu-id="ff672-309">Se o argumento for um método lambda com parâmetros que ainda precisam inferido tipos, inferir `Object` para os tipos desses parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ff672-309">If the argument is a lambda method with parameters that still need inferred types, then infer `Object` for the types of those parameters.</span></span>

  * <span data-ttu-id="ff672-310">Se o elemento é um parâmetro de tipo de método, em seguida, inferir o parâmetro de tipo de método para ser o tipo dominante entre as dicas de tipo de argumento e marcar o elemento como concluído.</span><span class="sxs-lookup"><span data-stu-id="ff672-310">If the element is a method type parameter, then infer the method type parameter to be the dominant type among the argument type hints and mark the element as complete.</span></span> <span data-ttu-id="ff672-311">Se uma dica de tipo tiver uma restrição de elemento de matriz, apenas as conversões que são válidas entre matrizes do tipo especificado são consideradas (conversões de matriz ou seja, covariante e intrínseco).</span><span class="sxs-lookup"><span data-stu-id="ff672-311">If a type hint has an array element restriction on it, then only conversions that are valid between arrays of the given type are considered (i.e. covariant and intrinsic array conversions).</span></span> <span data-ttu-id="ff672-312">Se uma dica de tipo tiver uma restrição de argumento genérico, somente conversões de identidade são consideradas.</span><span class="sxs-lookup"><span data-stu-id="ff672-312">If a type hint has a generic argument restriction on it, then only identity conversions are considered.</span></span> <span data-ttu-id="ff672-313">Se nenhum tipo dominante pode ser escolhido, a inferência de tipos falha.</span><span class="sxs-lookup"><span data-stu-id="ff672-313">If no dominant type can be chosen, inference fails.</span></span> <span data-ttu-id="ff672-314">Se quaisquer tipos de argumento do método lambda dependem este parâmetro de tipo de método, o tipo é propagado para o método lambda.</span><span class="sxs-lookup"><span data-stu-id="ff672-314">If any lambda method argument types depend on this method type parameter, the type is propagated to the lambda method.</span></span>

* <span data-ttu-id="ff672-315">Se o componente com rigidez de tipos contiver mais de um elemento, o componente contém um ciclo.</span><span class="sxs-lookup"><span data-stu-id="ff672-315">If the strongly typed component contains more than one element, then the component contains a cycle.</span></span>

  * <span data-ttu-id="ff672-316">Para cada parâmetro de tipo de método que é um elemento no componente, se o parâmetro de tipo de método depende de um argumento que não é marcada como concluído, converter essa dependência em uma declaração que será verificada no final do processo de inferência de tipos.</span><span class="sxs-lookup"><span data-stu-id="ff672-316">For each method type parameter that is an element in the component, if the method type parameter depends on an argument that is not marked complete, convert that dependency into an assertion that will be checked at the end of the inference process.</span></span>

  * <span data-ttu-id="ff672-317">Reinicie o processo de inferência de tipos no ponto em que os componentes com rigidez de tipos foram determinados.</span><span class="sxs-lookup"><span data-stu-id="ff672-317">Restart the inference process at the point at which the strongly typed components were determined.</span></span>

<span data-ttu-id="ff672-318">Se a inferência de tipo for bem-sucedida para todos os parâmetros de tipo de método, todas as dependências que foram alteradas em asserções são verificadas.</span><span class="sxs-lookup"><span data-stu-id="ff672-318">If type inference succeeds for all of the method type parameters, then any dependencies that were changed into assertions are checked.</span></span> <span data-ttu-id="ff672-319">Uma asserção terá êxito se o tipo do argumento é implicitamente conversível para o tipo inferido do parâmetro de tipo de método.</span><span class="sxs-lookup"><span data-stu-id="ff672-319">An assertion succeeds if the type of the argument is implicitly convertible to the inferred type of the method type parameter.</span></span> <span data-ttu-id="ff672-320">Se uma asserção falha, inferência de argumento de tipo falhará.</span><span class="sxs-lookup"><span data-stu-id="ff672-320">If an assertion fails, then type argument inference fails.</span></span>

<span data-ttu-id="ff672-321">Dado um tipo de argumento `Ta` para um argumento `A` e um tipo de parâmetro `Tp` para um parâmetro `P`, dicas de tipo são geradas da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ff672-321">Given an argument type `Ta` for an argument `A` and a parameter type `Tp` for a parameter `P`, type hints are generated as follows:</span></span>

* <span data-ttu-id="ff672-322">Se `Tp` não envolve quaisquer parâmetros de tipo de método e nenhuma dica é gerada.</span><span class="sxs-lookup"><span data-stu-id="ff672-322">If `Tp` does not involve any method type parameters then no hints are generated.</span></span>

* <span data-ttu-id="ff672-323">Se `Tp` e `Ta` são tipos de matriz da mesma classificação e, em seguida, substitua `Ta` e `Tp` com os tipos de elemento de `Ta` e `Tp` e reiniciar o processo com uma restrição de elemento de matriz.</span><span class="sxs-lookup"><span data-stu-id="ff672-323">If `Tp` and `Ta` are array types of the same rank, then replace `Ta` and `Tp` with the element types of `Ta` and `Tp` and restart this process with an array element restriction.</span></span>

* <span data-ttu-id="ff672-324">Se `Tp` é um parâmetro de tipo de método, em seguida, `Ta` é adicionado como uma dica de tipo com a restrição atual, se houver.</span><span class="sxs-lookup"><span data-stu-id="ff672-324">If `Tp` is a method type parameter, then `Ta` is added as a type hint with the current restriction, if any.</span></span>

* <span data-ttu-id="ff672-325">Se `A` é um método lambda e `Tp` é um tipo delegado construído ou `System.Linq.Expressions.Expression(Of T)`, onde `T` é um tipo de delegado construído, para cada tipo de parâmetro do método lambda `TL` e correspondente do tipo de parâmetro delegado`TD`, substitua `Ta` com `TL` e `Tp` com `TD` e reinicie o processo sem restrição.</span><span class="sxs-lookup"><span data-stu-id="ff672-325">If `A` is a lambda method and `Tp` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type, for each lambda method parameter type `TL` and corresponding delegate parameter type `TD`, replace `Ta` with `TL` and `Tp` with `TD` and restart the process with no restriction.</span></span> <span data-ttu-id="ff672-326">Em seguida, substitua `Ta` com o tipo de retorno do método lambda e:</span><span class="sxs-lookup"><span data-stu-id="ff672-326">Then, replace `Ta` with the return type of the lambda method and:</span></span>

  * <span data-ttu-id="ff672-327">Se `A` é um método lambda regular, substitua `Tp` com o tipo de retorno do tipo de delegado;</span><span class="sxs-lookup"><span data-stu-id="ff672-327">if `A` is a regular lambda method, replace `Tp` with the return type of the delegate type;</span></span>
  * <span data-ttu-id="ff672-328">Se `A` é um método de lambda assíncrono e o tipo de retorno do tipo de delegado tem o formato `Task(Of T)` para alguns `T`, substitua `Tp` com que `T`;</span><span class="sxs-lookup"><span data-stu-id="ff672-328">if `A` is an async lambda method and the return type of the delegate type has form `Task(Of T)` for some `T`, replace `Tp` with that `T`;</span></span>
  * <span data-ttu-id="ff672-329">Se `A` é um método de lambda de iterador e o tipo de retorno do tipo de delegado tem o formato `IEnumerator(Of T)` ou `IEnumerable(Of T)` para alguns `T`, substitua `Tp` com que `T`.</span><span class="sxs-lookup"><span data-stu-id="ff672-329">if `A` is an iterator lambda method and the return type of the delegate type has form `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, replace `Tp` with that `T`.</span></span>
  * <span data-ttu-id="ff672-330">Em seguida, reinicie o processo sem restrição.</span><span class="sxs-lookup"><span data-stu-id="ff672-330">Next, restart the process with no restriction.</span></span>

* <span data-ttu-id="ff672-331">Se `A` é um ponteiro de método e `Tp` é um construído tipo delegate, use os tipos de parâmetro de `Tp` determinar qual método apontado é mais aplicável às `Tp`.</span><span class="sxs-lookup"><span data-stu-id="ff672-331">If `A` is a method pointer and `Tp` is a constructed delegate type, use the parameter types of `Tp` to determine which method pointed is most applicable to `Tp`.</span></span> <span data-ttu-id="ff672-332">Se houver um método que é mais aplicável, substitua `Ta` com o tipo de retorno do método e `Tp` com o tipo de retorno do tipo de delegado e reiniciar o processo sem restrição.</span><span class="sxs-lookup"><span data-stu-id="ff672-332">If there is a method that is most applicable, replace `Ta` with the return type of the method and `Tp` with the return type of the delegate type and restart the process with no restriction.</span></span>

* <span data-ttu-id="ff672-333">Caso contrário, `Tp` deve ser um tipo construído.</span><span class="sxs-lookup"><span data-stu-id="ff672-333">Otherwise, `Tp` must be a constructed type.</span></span> <span data-ttu-id="ff672-334">Considerando `TG`, o tipo genérico de `Tp`,</span><span class="sxs-lookup"><span data-stu-id="ff672-334">Given `TG`, the generic type of `Tp`,</span></span>

  * <span data-ttu-id="ff672-335">Se `Ta` é `TG`, herda `TG`, ou implementa o tipo `TG` exatamente uma vez, em seguida, para cada argumento de tipo correspondente `Tax` do `Ta` e `Tpx` de `Tp`, substitua `Ta` com `Tax` e `Tp` com `Tpx` e reinicie o processo com uma restrição de argumento genérico.</span><span class="sxs-lookup"><span data-stu-id="ff672-335">If `Ta` is `TG`, inherits from `TG`, or implements the type `TG` exactly once, then for each matching type argument `Tax` from `Ta` and `Tpx` from `Tp`, replace `Ta` with `Tax` and `Tp` with `Tpx` and restart the process with a generic argument restriction.</span></span>

  * <span data-ttu-id="ff672-336">Caso contrário, a inferência de tipo falhará para o método genérico.</span><span class="sxs-lookup"><span data-stu-id="ff672-336">Otherwise, type inference fails for the generic method.</span></span>

<span data-ttu-id="ff672-337">O sucesso da inferência de tipo, em e de si mesmo, garante que o método é aplicável.</span><span class="sxs-lookup"><span data-stu-id="ff672-337">The success of type inference does not, in and of itself, guarantee that the method is applicable.</span></span>
