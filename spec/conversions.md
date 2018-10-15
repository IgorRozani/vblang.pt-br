# <a name="conversions"></a><span data-ttu-id="ab879-101">Conversões</span><span class="sxs-lookup"><span data-stu-id="ab879-101">Conversions</span></span>

<span data-ttu-id="ab879-102">Conversão é o processo de alteração de um valor de um tipo para outro.</span><span class="sxs-lookup"><span data-stu-id="ab879-102">Conversion is the process of changing a value from one type to another.</span></span> <span data-ttu-id="ab879-103">Por exemplo, um valor do tipo `Integer` pode ser convertido em um valor do tipo `Double`, ou um valor do tipo `Derived` pode ser convertido em um valor do tipo `Base`, supondo que `Base` e `Derived` são classes e `Derived`herda de `Base`.</span><span class="sxs-lookup"><span data-stu-id="ab879-103">For example, a value of type `Integer` can be converted to a value of type `Double`, or a value of type `Derived` can be converted to a value of type `Base`, assuming that `Base` and `Derived` are both classes and `Derived` inherits from `Base`.</span></span> <span data-ttu-id="ab879-104">Conversões de não exigir o valor para alterar (como o último exemplo), ou podem requerer alterações significativas na representação do valor (como no exemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="ab879-104">Conversions may not require the value itself to change (as in the latter example), or they may require significant changes in the value representation (as in the former example).</span></span>

<span data-ttu-id="ab879-105">Conversões podem ser de ampliação ou redução.</span><span class="sxs-lookup"><span data-stu-id="ab879-105">Conversions may either be widening or narrowing.</span></span> <span data-ttu-id="ab879-106">Um *conversão de ampliação* é uma conversão de um tipo em outro tipo de domínio cujo valor é pelo menos tão grande, se não maior que o domínio do valor do tipo original.</span><span class="sxs-lookup"><span data-stu-id="ab879-106">A *widening conversion* is a conversion from a type to another type whose value domain is at least as big, if not bigger, than the original type's value domain.</span></span> <span data-ttu-id="ab879-107">Conversões ampliadoras nunca devem falhar.</span><span class="sxs-lookup"><span data-stu-id="ab879-107">Widening conversions should never fail.</span></span> <span data-ttu-id="ab879-108">Um *conversão de estreitamento* é uma conversão de um tipo em outro tipo de domínio cujo valor é menor do que do tipo original valor de domínio ou suficientemente não relacionadas que extra é necessário ter cuidado ao fazer a conversão (para Por exemplo, durante a conversão de `Integer` para `String`).</span><span class="sxs-lookup"><span data-stu-id="ab879-108">A *narrowing conversion* is a conversion from a type to another type whose value domain is either smaller than the original type's value domain or sufficiently unrelated that extra care must be taken when doing the conversion (for example, when converting from `Integer` to `String`).</span></span> <span data-ttu-id="ab879-109">Reduzir conversões, o que podem envolver a perda de informações, poderá falhar.</span><span class="sxs-lookup"><span data-stu-id="ab879-109">Narrowing conversions, which may entail loss of information, can fail.</span></span>

<span data-ttu-id="ab879-110">A conversão de identidade (ou seja, uma conversão de um tipo a mesmo) e a conversão do valor padrão (ou seja, uma conversão de `Nothing`) são definidos para todos os tipos.</span><span class="sxs-lookup"><span data-stu-id="ab879-110">The identity conversion (i.e. a conversion from a type to itself) and default value conversion (i.e. a conversion from `Nothing`) are defined for all types.</span></span>

## <a name="implicit-and-explicit-conversions"></a><span data-ttu-id="ab879-111">Conversões implícitas e explícitas</span><span class="sxs-lookup"><span data-stu-id="ab879-111">Implicit and Explicit Conversions</span></span>

<span data-ttu-id="ab879-112">As conversões podem ser *implícita* ou *explícita*.</span><span class="sxs-lookup"><span data-stu-id="ab879-112">Conversions can be either *implicit* or *explicit*.</span></span> <span data-ttu-id="ab879-113">Conversões implícitas acontecem sem nenhuma sintaxe especial.</span><span class="sxs-lookup"><span data-stu-id="ab879-113">Implicit conversions occur without any special syntax.</span></span> <span data-ttu-id="ab879-114">A seguir está um exemplo de conversão implícita de um `Integer` de valor para um `Long` valor:</span><span class="sxs-lookup"><span data-stu-id="ab879-114">The following is an example of implicit conversion of an `Integer` value to a `Long` value:</span></span>

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

Conversões explícitas, por outro lado, exigem operadores de conversão. Tentativa de fazer uma conversão explícita em um valor sem um operador cast faz com que um erro de tempo de compilação. <span data-ttu-id="ab879-117">O exemplo a seguir usa uma conversão explícita para converter um `Long` de valor para um `Integer` valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-117">The following example uses an explicit conversion to convert a `Long` value to an `Integer` value.</span></span>

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

O conjunto de conversões implícitas depende do ambiente de compilação e o `Option Strict` instrução. Se a semântica estrita estiverem sendo usada, somente conversões de expansão podem ocorrer implicitamente. <span data-ttu-id="ab879-120">Se estiverem sendo usada semântica permissiva, todos os de ampliar e reduzir conversões (em outras palavras, todas as conversões) podem ocorrer implicitamente.</span><span class="sxs-lookup"><span data-stu-id="ab879-120">If permissive semantics are being used, all widening and narrowing conversions (in other words, all conversions) may occur implicitly.</span></span>

## <a name="boolean-conversions"></a><span data-ttu-id="ab879-121">Conversões de boolianos</span><span class="sxs-lookup"><span data-stu-id="ab879-121">Boolean Conversions</span></span>

<span data-ttu-id="ab879-122">Embora `Boolean` não é um tipo numérico, ele tem reduzir conversões de e para os tipos numéricos, como se fosse um tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="ab879-122">Although `Boolean` is not a numeric type, it does have narrowing conversions to and from the numeric types as if it were an enumerated type.</span></span> <span data-ttu-id="ab879-123">O literal `True` converte o literal `255` para `Byte`, `65535` para `UShort`, `4294967295` para `UInteger`, `18446744073709551615` para `ULong`e a expressão `-1` para `SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, e `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-123">The literal `True` converts to the literal `255` for `Byte`, `65535` for `UShort`, `4294967295` for `UInteger`, `18446744073709551615` for `ULong`, and to the expression `-1` for `SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, and `Double`.</span></span> <span data-ttu-id="ab879-124">O literal `False` converte o literal `0`.</span><span class="sxs-lookup"><span data-stu-id="ab879-124">The literal `False` converts to the literal `0`.</span></span> <span data-ttu-id="ab879-125">Converte um valor zero numérico literal `False`.</span><span class="sxs-lookup"><span data-stu-id="ab879-125">A zero numeric value converts to the literal `False`.</span></span> <span data-ttu-id="ab879-126">Todos os outros valores numéricos converter para literal `True`.</span><span class="sxs-lookup"><span data-stu-id="ab879-126">All other numeric values convert to the literal `True`.</span></span>

<span data-ttu-id="ab879-127">Há uma conversão redutora de booliano para a cadeia de caracteres, convertendo em qualquer um `System.Boolean.TrueString` ou `System.Boolean.FalseString`.</span><span class="sxs-lookup"><span data-stu-id="ab879-127">There is a narrowing conversion from Boolean to String, converting to either `System.Boolean.TrueString` or `System.Boolean.FalseString`.</span></span> <span data-ttu-id="ab879-128">Também há uma conversão redutora de `String` à `Boolean`: se a cadeia de caracteres era igual a `TrueString` ou `FalseString` (a cultura atual, case-insensitively), em seguida, ele usa o valor apropriado; caso contrário, ele tenta analisar a cadeia de caracteres como um tipo numérico (em hexadecimal ou octal se possível, caso contrário, como um float) e usa as regras acima; Caso contrário, ele lança `System.InvalidCastException`.</span><span class="sxs-lookup"><span data-stu-id="ab879-128">There is also a narrowing conversion from `String` to `Boolean`: if the string was equal to `TrueString` or `FalseString` (in the current culture, case-insensitively) then it uses the appropriate value; otherwise it attempts to parse the string as a numeric type (in hex or octal if possible, otherwise as a float) and uses the above rules; otherwise it throws `System.InvalidCastException`.</span></span>

## <a name="numeric-conversions"></a><span data-ttu-id="ab879-129">Conversões numéricas</span><span class="sxs-lookup"><span data-stu-id="ab879-129">Numeric Conversions</span></span>

<span data-ttu-id="ab879-130">Conversões numéricas existirem entre os tipos `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` e `Double`, e todos os tipos enumerados.</span><span class="sxs-lookup"><span data-stu-id="ab879-130">Numeric conversions exist between the types `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` and `Double`, and all enumerated types.</span></span> <span data-ttu-id="ab879-131">Quando está sendo convertido, tipos enumerados são tratados como se fossem seus tipos base.</span><span class="sxs-lookup"><span data-stu-id="ab879-131">When being converted, enumerated types are treated as if they were their underlying types.</span></span> <span data-ttu-id="ab879-132">Ao converter para um tipo enumerado, o valor de origem não é necessário estar de acordo com o conjunto de valores definidos no tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="ab879-132">When converting to an enumerated type, the source value is not required to conform to the set of values defined in the enumerated type.</span></span> <span data-ttu-id="ab879-133">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab879-133">For example:</span></span>

```vb
Enum Values
    One
    Two
    Three
End Enum

Module Test
    Sub Main()
        Dim x As Integer = 5

        ' OK, even though there is no enumerated value for 5.
        Dim y As Values = CType(x, Values)
    End Sub
End Module
```

<span data-ttu-id="ab879-134">Conversões numéricas são processadas em tempo de execução da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab879-134">Numeric conversions are processed at run-time as follows:</span></span>

* <span data-ttu-id="ab879-135">Para uma conversão de um tipo numérico em um tipo numérico mais amplo, o valor é simplesmente convertido para o tipo mais amplo.</span><span class="sxs-lookup"><span data-stu-id="ab879-135">For a conversion from a numeric type to a wider numeric type, the value is simply converted to the wider type.</span></span> <span data-ttu-id="ab879-136">Conversões de `UInteger`, `Integer`, `ULong`, `Long`, ou `Decimal` para `Single` ou `Double` são arredondados para mais próximo `Single` ou `Double` valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-136">Conversions from `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal` to `Single` or `Double` are rounded to the nearest `Single` or `Double` value.</span></span> <span data-ttu-id="ab879-137">Enquanto essa conversão pode causar uma perda de precisão, ele nunca fará com que uma perda de magnitude.</span><span class="sxs-lookup"><span data-stu-id="ab879-137">While this conversion may cause a loss of precision, it will never cause a loss of magnitude.</span></span>

* <span data-ttu-id="ab879-138">Para uma conversão de um tipo integral para outro tipo integral ou de `Single`, `Double`, ou `Decimal` para um tipo integral, o resultado depende se a verificação de estouro de inteiro está em:</span><span class="sxs-lookup"><span data-stu-id="ab879-138">For a conversion from an integral type to another integral type, or from `Single`, `Double`, or `Decimal` to an integral type, the result depends on whether integer overflow checking is on:</span></span>

  <span data-ttu-id="ab879-139">*Se o estouro de inteiros que está sendo verificado:*</span><span class="sxs-lookup"><span data-stu-id="ab879-139">*If integer overflow is being checked:*</span></span>

  * <span data-ttu-id="ab879-140">Se a fonte for um tipo integral, a conversão for bem-sucedida, se o argumento de origem está dentro do intervalo do tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab879-140">If the source is an integral type, the conversion succeeds if the source argument is within the range of the destination type.</span></span> <span data-ttu-id="ab879-141">A conversão gera um `System.OverflowException` exceção se o argumento de origem está fora do intervalo do tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab879-141">The conversion throws a `System.OverflowException` exception if the source argument is outside the range of the destination type.</span></span>

  * <span data-ttu-id="ab879-142">Se a fonte estiver `Single`, `Double`, ou `Decimal`, o valor de origem é arredondado para cima ou para baixo até o valor inteiro mais próximo, e esse valor integral se tornará o resultado da conversão.</span><span class="sxs-lookup"><span data-stu-id="ab879-142">If the source is `Single`, `Double`, or `Decimal`, the source value is rounded up or down to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="ab879-143">Se o valor de origem é igualmente perto de dois valores integrais, o valor é arredondado para o valor que tem um número par na posição de dígitos menos significativa.</span><span class="sxs-lookup"><span data-stu-id="ab879-143">If the source value is equally close to two integral values, the value is rounded to the value that has an even number in the least significant digit position.</span></span> <span data-ttu-id="ab879-144">Se o valor integral resultante estiver fora do intervalo do tipo de destino, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab879-144">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` exception is thrown.</span></span>

  <span data-ttu-id="ab879-145">*Se o estouro de inteiro não está sendo verificado:*</span><span class="sxs-lookup"><span data-stu-id="ab879-145">*If integer overflow is not being checked:*</span></span>

  * <span data-ttu-id="ab879-146">Se a fonte for um tipo integral, a conversão sempre terá êxito e consiste apenas em Descartando os bits mais significativos do valor de source.</span><span class="sxs-lookup"><span data-stu-id="ab879-146">If the source is an integral type, the conversion always succeeds and simply consists of discarding the most significant bits of the source value.</span></span>

  * <span data-ttu-id="ab879-147">Se a fonte estiver `Single`, `Double`, ou `Decimal`, a conversão sempre terá êxito e consiste apenas o valor de origem para o valor integral mais próximo de arredondamento.</span><span class="sxs-lookup"><span data-stu-id="ab879-147">If the source is `Single`, `Double`, or `Decimal`, the conversion always succeeds and simply consists of rounding the source value towards the nearest integral value.</span></span> <span data-ttu-id="ab879-148">Se o valor de origem é igualmente perto de dois valores integrais, o valor é sempre arredondado para o valor que tem um número par na posição de dígitos menos significativa.</span><span class="sxs-lookup"><span data-stu-id="ab879-148">If the source value is equally close to two integral values, the value is always rounded to the value that has an even number in the least significant digit position.</span></span>

* <span data-ttu-id="ab879-149">Para uma conversão de `Double` à `Single`, o `Double` valor é arredondado para mais próximo `Single` valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-149">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="ab879-150">Se o `Double` valor for muito pequeno para ser representado como um `Single`, o resultado se torna zero positivo ou negativo de zero.</span><span class="sxs-lookup"><span data-stu-id="ab879-150">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="ab879-151">Se o `Double` valor é muito grande para ser representado como um `Single`, o resultado se torna o infinito positivo ou negativo infinito.</span><span class="sxs-lookup"><span data-stu-id="ab879-151">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="ab879-152">Se o `Double` valor é `NaN`, o resultado também será `NaN`.</span><span class="sxs-lookup"><span data-stu-id="ab879-152">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

* <span data-ttu-id="ab879-153">Para uma conversão de `Single` ou `Double` à `Decimal`, o valor de origem é convertido em `Decimal` representação e arredondado para o próximo número após a 28ª casa decimal, se necessário.</span><span class="sxs-lookup"><span data-stu-id="ab879-153">For a conversion from `Single` or `Double` to `Decimal`, the source value is converted to `Decimal` representation and rounded to the nearest number after the 28th decimal place if required.</span></span> <span data-ttu-id="ab879-154">Se o valor de origem é muito pequeno para ser representado como um `Decimal`, o resultado será zero.</span><span class="sxs-lookup"><span data-stu-id="ab879-154">If the source value is too small to represent as a `Decimal`, the result becomes zero.</span></span> <span data-ttu-id="ab879-155">Se o valor de origem estiver `NaN`, infinito, ou muito grande para ser representado como uma `Decimal`, um `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab879-155">If the source value is `NaN`, infinity, or too large to represent as a `Decimal`, a `System.OverflowException` exception is thrown.</span></span>

* <span data-ttu-id="ab879-156">Para uma conversão de `Double` à `Single`, o `Double` valor é arredondado para mais próximo `Single` valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-156">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="ab879-157">Se o `Double` valor for muito pequeno para ser representado como um `Single`, o resultado se torna zero positivo ou negativo de zero.</span><span class="sxs-lookup"><span data-stu-id="ab879-157">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="ab879-158">Se o `Double` valor é muito grande para ser representado como um `Single`, o resultado se torna o infinito positivo ou negativo infinito.</span><span class="sxs-lookup"><span data-stu-id="ab879-158">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="ab879-159">Se o `Double` valor é `NaN`, o resultado também será `NaN`.</span><span class="sxs-lookup"><span data-stu-id="ab879-159">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

## <a name="reference-conversions"></a><span data-ttu-id="ab879-160">Conversões de referência</span><span class="sxs-lookup"><span data-stu-id="ab879-160">Reference Conversions</span></span>

<span data-ttu-id="ab879-161">Tipos de referência podem ser convertidos em um tipo base e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="ab879-161">Reference types may be converted to a base type, and vice versa.</span></span> <span data-ttu-id="ab879-162">Conversões de um tipo base para um tipo mais derivado só ter êxito em tempo de execução se o valor que está sendo convertido for um valor nulo, o próprio tipo derivado ou um tipo mais derivado.</span><span class="sxs-lookup"><span data-stu-id="ab879-162">Conversions from a base type to a more derived type only succeed at run time if the value being converted is a null value, the derived type itself, or a more derived type.</span></span>

<span data-ttu-id="ab879-163">Tipos de classe e interface podem ser convertidos para e de qualquer tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="ab879-163">Class and interface types can be cast to and from any interface type.</span></span> <span data-ttu-id="ab879-164">Conversões entre um tipo e um tipo de interface êxito apenas em tempo de execução se os tipos reais envolvidos têm uma relação de herança ou implementação.</span><span class="sxs-lookup"><span data-stu-id="ab879-164">Conversions between a type and an interface type only succeed at run time if the actual types involved have an inheritance or implementation relationship.</span></span> <span data-ttu-id="ab879-165">Como um tipo de interface sempre conterá uma instância de um tipo que deriva de `Object`, um tipo de interface sempre também pode ser convertido para e de `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab879-165">Because an interface type will always contain an instance of a type that derives from `Object`, an interface type can also always be cast to and from `Object`.</span></span>

<span data-ttu-id="ab879-166">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab879-166">__Note.__</span></span> <span data-ttu-id="ab879-167">Não é um erro ao converter um `NotInheritable` classes de e para interfaces que não implementa como classes que representam classes COM podem ter implementações de interface que não são conhecidas até o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ab879-167">It is not an error to convert a `NotInheritable` classes to and from interfaces that it does not implement because classes that represent COM classes may have interface implementations that are not known until run time.</span></span> 

<span data-ttu-id="ab879-168">Se uma conversão de referência falhar em tempo de execução, um `System.InvalidCastException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab879-168">If a reference conversion fails at run time, a `System.InvalidCastException` exception is thrown.</span></span>

### <a name="reference-variance-conversions"></a><span data-ttu-id="ab879-169">Conversões de variação de referência</span><span class="sxs-lookup"><span data-stu-id="ab879-169">Reference Variance Conversions</span></span>

<span data-ttu-id="ab879-170">Interfaces genéricas ou delegados podem ter parâmetros de tipo variante que permitem conversões entre variantes compatíveis do tipo.</span><span class="sxs-lookup"><span data-stu-id="ab879-170">Generic interfaces or delegates may have variant type parameters that allow conversions between compatible variants of the type.</span></span> <span data-ttu-id="ab879-171">Portanto, em tempo de execução uma conversão de um tipo de classe ou um tipo de interface para um tipo de interface que é compatível com um tipo de interface, ele herda de ou implementa de variante terá êxito.</span><span class="sxs-lookup"><span data-stu-id="ab879-171">Therefore, at runtime a conversion from a class type or an interface type to an interface type that is variant compatible with an interface type it inherits from or implements will succeed.</span></span> <span data-ttu-id="ab879-172">Da mesma forma, tipos de delegado podem ser convertidos para e de variante compatível tipos delegados.</span><span class="sxs-lookup"><span data-stu-id="ab879-172">Similarly, delegate types can be cast to and from variant compatible delegate types.</span></span> <span data-ttu-id="ab879-173">Por exemplo, o tipo de delegado</span><span class="sxs-lookup"><span data-stu-id="ab879-173">For example, the delegate type</span></span>

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

<span data-ttu-id="ab879-174">permitiria que uma conversão de `F(Of Object, Integer)` para `F(Of String, Integer)`.</span><span class="sxs-lookup"><span data-stu-id="ab879-174">would allow a conversion from `F(Of Object, Integer)` to `F(Of String, Integer)`.</span></span> <span data-ttu-id="ab879-175">Ou seja, um delegado `F` que utiliza `Object` pode ser usado com segurança como um delegado `F` que utiliza `String`.</span><span class="sxs-lookup"><span data-stu-id="ab879-175">That is, a delegate `F` which takes `Object` may be safely used as a delegate `F` which takes `String`.</span></span> <span data-ttu-id="ab879-176">Quando o delegado é invocado, o método de destino estará esperando um objeto e uma cadeia de caracteres é um objeto.</span><span class="sxs-lookup"><span data-stu-id="ab879-176">When the delegate is invoked, the target method will be expecting an object, and a string is an object.</span></span>

<span data-ttu-id="ab879-177">Um tipo de interface ou delegado genérico `S(Of S1,...,Sn)` será considerada *variante compatível* com um tipo de interface ou delegado genérico `T(Of T1,...,Tn)` se:</span><span class="sxs-lookup"><span data-stu-id="ab879-177">A generic delegate or interface type `S(Of S1,...,Sn)` is said to be *variant compatible* with a generic interface or delegate type `T(Of T1,...,Tn)` if:</span></span>

* <span data-ttu-id="ab879-178">`S` e `T` são construídos do mesmo tipo genérico `U(Of U1,...,Un)`.</span><span class="sxs-lookup"><span data-stu-id="ab879-178">`S` and `T` are both constructed from the same generic type `U(Of U1,...,Un)`.</span></span>

* <span data-ttu-id="ab879-179">Para cada parâmetro de tipo `Ux`:</span><span class="sxs-lookup"><span data-stu-id="ab879-179">For each type parameter `Ux`:</span></span>

  * <span data-ttu-id="ab879-180">Se o parâmetro de tipo foi declarado sem, em seguida, a variação `Sx` e `Tx` deve ser do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="ab879-180">If the type parameter was declared without variance then `Sx` and `Tx` must be the same type.</span></span>

  * <span data-ttu-id="ab879-181">Se o parâmetro de tipo foi declarado `In` e em seguida, deve haver uma ampliação identidade, padrão, referência, matriz ou tipo de conversão de parâmetro de `Sx` para `Tx`.</span><span class="sxs-lookup"><span data-stu-id="ab879-181">If the type parameter was declared `In` then there must be a widening identity, default, reference, array, or type parameter conversion from `Sx` to `Tx`.</span></span>

  * <span data-ttu-id="ab879-182">Se o parâmetro de tipo foi declarado `Out` e em seguida, deve haver uma ampliação identidade, padrão, referência, matriz ou tipo de conversão de parâmetro de `Tx` para `Sx`.</span><span class="sxs-lookup"><span data-stu-id="ab879-182">If the type parameter was declared `Out` then there must be a widening identity, default, reference, array, or type parameter conversion from `Tx` to `Sx`.</span></span>

<span data-ttu-id="ab879-183">Ao converter de uma classe em uma interface genérica com parâmetros de tipo de variante, se a classe implementa mais de uma interface compatível com variante a conversão é ambígua, se não houver uma conversão não-variant.</span><span class="sxs-lookup"><span data-stu-id="ab879-183">When converting from a class to a generic interface with variant type parameters, if the class implements more than one variant compatible interface the conversion is ambiguous if there is not a non-variant conversion.</span></span> <span data-ttu-id="ab879-184">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab879-184">For example:</span></span>

```vb
Class Base
End Class

Class Derived1
    Inherits Base
End Class

Class Derived2
    Inherits Base
End Class

Class OneAndTwo
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Class BaseAndOneAndTwo
    Implements IEnumerable(Of Base)
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Module Test
    Sub Main()
        ' Error: conversion is ambiguous
        Dim x As IEnumerable(Of Base) = New OneAndTwo()

        ' OK, will pick up the direct implementation of IEnumerable(Of Base)
        Dim y as IEnumerable(Of Base) = New BaseAndOneAndTwo()
    End Sub
End Module
```

### <a name="anonymous-delegate-conversions"></a><span data-ttu-id="ab879-185">Conversões de delegado anônimo</span><span class="sxs-lookup"><span data-stu-id="ab879-185">Anonymous Delegate Conversions</span></span>

<span data-ttu-id="ab879-186">Quando uma expressão classificada como um método lambda é reclassificada como um valor em um contexto em que não há nenhum tipo de destino (por exemplo, `Dim x = Function(a As Integer, b As Integer) a + b`), ou onde o tipo de destino não é um tipo de delegado, o tipo da expressão resultante é um tipo de delegado anônimo equivalente à assinatura do método lambda.</span><span class="sxs-lookup"><span data-stu-id="ab879-186">When an expression classified as a lambda method is reclassified as a value in a context where there is no target type (for example, `Dim x = Function(a As Integer, b As Integer) a + b`), or where the target type is not a delegate type, the type of the resulting expression is an anonymous delegate type equivalent to the signature of the lambda method.</span></span> <span data-ttu-id="ab879-187">Esse tipo de delegado anônimo tem uma conversão para qualquer tipo de delegado compatível: um tipo de delegado compatível é qualquer tipo de delegado que pode ser criado usando uma expressão de criação de delegado com o tipo de delegado anônimo `Invoke` método como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ab879-187">This anonymous delegate type has a conversion to any compatible delegate type: a compatible delegate type is any delegate type that can be created using a delegate creation expression with the anonymous delegate type's `Invoke` method as a parameter.</span></span> <span data-ttu-id="ab879-188">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab879-188">For example:</span></span>

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

<span data-ttu-id="ab879-189">Observe que os tipos `System.Delegate` e `System.MulticastDelegate` por si próprios considerados tipos de delegado (mesmo que todos os tipos de delegado herdam deles).</span><span class="sxs-lookup"><span data-stu-id="ab879-189">Note that the types `System.Delegate` and `System.MulticastDelegate` are not themselves considered delegate types (even though all delegate types inherit from them).</span></span> <span data-ttu-id="ab879-190">Além disso, observe que a conversão do tipo de delegado anônimo para um tipo de delegado compatível não é uma conversão de referência.</span><span class="sxs-lookup"><span data-stu-id="ab879-190">Also, note that the conversion from anonymous delegate type to a compatible delegate type is not a reference conversion.</span></span>

## <a name="array-conversions"></a><span data-ttu-id="ab879-191">Conversões de matriz</span><span class="sxs-lookup"><span data-stu-id="ab879-191">Array Conversions</span></span>

<span data-ttu-id="ab879-192">As conversões que são definidas em matrizes em virtude do fato de que eles são tipos de referência, além de várias conversões de especiais existem para matrizes.</span><span class="sxs-lookup"><span data-stu-id="ab879-192">Besides the conversions that are defined on arrays by virtue of the fact that they are reference types, several special conversions exist for arrays.</span></span>

<span data-ttu-id="ab879-193">Para quaisquer dois tipos `A` e `B`, se eles forem tipos de referência ou parâmetros de tipo não conhecidos como tipos de valor e se `A` tem uma referência, matriz ou tipo de conversão de parâmetro para `B`, existe uma conversão de uma matriz de tipo de `A` para uma matriz do tipo `B` com a mesma classificação.</span><span class="sxs-lookup"><span data-stu-id="ab879-193">For any two types `A` and `B`, if they are both reference types or type parameters not known to be value types, and if `A` has a reference, array, or type parameter conversion to `B`, a conversion exists from an array of type `A` to an array of type `B` with the same rank.</span></span> <span data-ttu-id="ab879-194">Essa relação é conhecida como *covariância de matriz*.</span><span class="sxs-lookup"><span data-stu-id="ab879-194">This relationship is known as *array covariance*.</span></span> <span data-ttu-id="ab879-195">Covariância de matriz em particular significa que um elemento de uma matriz cujo tipo de elemento é `B` , na verdade, pode ser um elemento de uma matriz cujo tipo de elemento é `A`, desde que ambos `A` e `B` são tipos de referência e esse `B` tem uma conversão de referência ou conversão de matriz para `A`.</span><span class="sxs-lookup"><span data-stu-id="ab879-195">Array covariance in particular means that an element of an array whose element type is `B` may actually be an element of an array whose element type is `A`, provided that both `A` and `B` are reference types and that `B` has a reference conversion or array conversion to `A`.</span></span> <span data-ttu-id="ab879-196">No exemplo a seguir, a segunda chamada de `F` faz com que um `System.ArrayTypeMismatchException` exceção seja lançada porque o tipo de elemento real do `b` é `String`, e não `Object`:</span><span class="sxs-lookup"><span data-stu-id="ab879-196">In the following example, the second invocation of `F` causes a `System.ArrayTypeMismatchException` exception to be thrown because the actual element type of `b` is `String`, not `Object`:</span></span>

```vb
Module Test
    Sub F(ByRef x As Object)
    End Sub

    Sub Main()
        Dim a(10) As Object
        Dim b() As Object = New String(10) {}
        F(a(0)) ' OK.
        F(b(1)) ' Not allowed: System.ArrayTypeMismatchException.
   End Sub
End Module
```

<span data-ttu-id="ab879-197">Devido a covariância de matriz, atribuições aos elementos de matrizes de tipo de referência incluem uma verificação de tempo de execução que garante que o valor que está sendo atribuído ao elemento de matriz, na verdade, é de um tipo permitido.</span><span class="sxs-lookup"><span data-stu-id="ab879-197">Because of array covariance, assignments to elements of reference type arrays include a run-time check that ensures that the value being assigned to the array element is actually of a permitted type.</span></span>

```vb
Module Test
    Sub Fill(array() As Object, index As Integer, count As Integer, _
            value As Object)
        Dim i As Integer

        For i = index To (index + count) - 1
            array(i) = value
        Next i
    End Sub

    Sub Main()
        Dim strings(100) As String

        Fill(strings, 0, 101, "Undefined")
        Fill(strings, 0, 10, Nothing)
        Fill(strings, 91, 10, 0)
    End Sub
End Module
```

<span data-ttu-id="ab879-198">Neste exemplo, a atribuição ao `array(i)` no método `Fill` implicitamente inclui uma verificação de tempo de execução que garante que o objeto referenciado pela variável `value` seja `Nothing` ou uma instância de um tipo que é compatível com o tipo de elemento real da matriz `array`.</span><span class="sxs-lookup"><span data-stu-id="ab879-198">In this example, the assignment to `array(i)` in method `Fill` implicitly includes a run-time check that ensures that the object referenced by the variable `value` is either `Nothing` or an instance of a type that is compatible with the actual element type of array `array`.</span></span> <span data-ttu-id="ab879-199">No método `Main`, as duas primeiras invocações de método `Fill` bem-sucedida, mas as causas de invocação de terceiro uma `System.ArrayTypeMismatchException` exceção seja lançada após executar a primeira atribuição para `array(i)`.</span><span class="sxs-lookup"><span data-stu-id="ab879-199">In method `Main`, the first two invocations of method `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` exception to be thrown upon executing the first assignment to `array(i)`.</span></span> <span data-ttu-id="ab879-200">A exceção ocorre porque uma `Integer` não podem ser armazenados em um `String` matriz.</span><span class="sxs-lookup"><span data-stu-id="ab879-200">The exception occurs because an `Integer` cannot be stored in a `String` array.</span></span>

<span data-ttu-id="ab879-201">Se um dos tipos de elemento da matriz é um parâmetro de tipo cujo tipo acaba sendo um tipo de valor em tempo de execução, um `System.InvalidCastException` exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="ab879-201">If one of the array element types is a type parameter whose type turns out to be a value type at runtime, a `System.InvalidCastException` exception will be thrown.</span></span> <span data-ttu-id="ab879-202">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab879-202">For example:</span></span>

```vb
Module Test
    Sub F(Of T As U, U)(x() As T)
        Dim y() As U = x
    End Sub

    Sub Main()
        ' F will throw an exception because Integer() cannot be
        ' converted to Object()
        F(New Integer() { 1, 2, 3 })
    End Sub
End Module
```

<span data-ttu-id="ab879-203">Também não há conversão entre uma matriz de um tipo enumerado, e uma matriz do tipo enumerado subjacente do tipo ou uma matriz de outro tipo enumerado com o mesmo tipo subjacente, desde que as matrizes têm a mesma classificação.</span><span class="sxs-lookup"><span data-stu-id="ab879-203">Conversions also exist between an array of an enumerated type and an array of the enumerated type's underlying type or an array of another enumerated type with the same underlying type, provided the arrays have the same rank.</span></span>

```vb
Enum Color As Byte
    Red
    Green
    Blue
End Enum

Module Test
    Sub Main()
        Dim a(10) As Color
        Dim b() As Integer
        Dim c() As Byte

        b = a    ' Error: Integer is not the underlying type of Color
        c = a    ' OK
        a = c    ' OK
    End Sub
End Module
```

<span data-ttu-id="ab879-204">Neste exemplo, uma matriz de `Color` é convertido em uma matriz de `Byte`, `Color`subjacente do tipo.</span><span class="sxs-lookup"><span data-stu-id="ab879-204">In this example, an array of `Color` is converted to and from an array of `Byte`, `Color`'s underlying type.</span></span> <span data-ttu-id="ab879-205">A conversão para uma matriz de `Integer`, no entanto, ocorrerá um erro porque `Integer` não é do tipo subjacente de `Color`.</span><span class="sxs-lookup"><span data-stu-id="ab879-205">The conversion to an array of `Integer`, however, will be an error because `Integer` is not the underlying type of `Color`.</span></span>

<span data-ttu-id="ab879-206">Uma matriz de classificação 1 do tipo `A()` também tem uma conversão de matriz para os tipos de interface de coleção `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` e `IEnumerable(Of B)` encontrado no `System.Collections.Generic`, desde que uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="ab879-206">A rank-1 array of type `A()` also has an array conversion to the collection interface types `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` and `IEnumerable(Of B)` found in `System.Collections.Generic`, so long as one of the following is true:</span></span>

- <span data-ttu-id="ab879-207">`A` e `B` são ambos referenciam tipos ou parâmetros de tipo não conhecidos como tipos de valor; e `A` tem uma conversão de parâmetro de referência, matriz ou tipo de ampliação para `B`; ou</span><span class="sxs-lookup"><span data-stu-id="ab879-207">`A` and `B` are both reference types or type parameters not known to be value types; and `A` has a widening reference, array or type parameter conversion to `B`; or</span></span>
- <span data-ttu-id="ab879-208">`A` e `B` são ambos os tipos enumerados do mesmo tipo subjacente; ou</span><span class="sxs-lookup"><span data-stu-id="ab879-208">`A` and `B` are both enumerated types of the same underlying type; or</span></span>
- <span data-ttu-id="ab879-209">um dos `A` e `B` é um tipo enumerado, e o outro é o seu tipo subjacente.</span><span class="sxs-lookup"><span data-stu-id="ab879-209">one of `A` and `B` is an enumerated type, and the other is its underlying type.</span></span>

<span data-ttu-id="ab879-210">Qualquer matriz de um tipo com qualquer classificação também tem uma conversão de matriz para os tipos de interface de coleção não genérica `IList`, `ICollection` e `IEnumerable` encontrado na `System.Collections`.</span><span class="sxs-lookup"><span data-stu-id="ab879-210">Any array of type A with any rank also has an array conversion to the non-generic collection interface types `IList`, `ICollection` and `IEnumerable` found in `System.Collections`.</span></span>

<span data-ttu-id="ab879-211">É possível iterar sobre as interfaces resultantes usando `For Each`, ou por meio de invocar o `GetEnumerator` métodos diretamente.</span><span class="sxs-lookup"><span data-stu-id="ab879-211">It is possible to iterate over the resulting interfaces using `For Each`, or through invoking the `GetEnumerator` methods directly.</span></span> <span data-ttu-id="ab879-212">No caso de matrizes de classificação 1 convertido formas genéricas ou não genérico de `IList` ou `ICollection`, também é possível obter elementos pelo índice.</span><span class="sxs-lookup"><span data-stu-id="ab879-212">In the case of rank-1 arrays converted generic or non-generic forms of `IList` or `ICollection`, it is also possible to get elements by index.</span></span> <span data-ttu-id="ab879-213">No caso de matrizes de classificação 1 convertidos em formas genéricas ou não genérico de `IList`, também é possível definir elementos pelo índice, sujeito a tempo de execução do mesmo covariância de matriz verifica conforme descrito acima.</span><span class="sxs-lookup"><span data-stu-id="ab879-213">In the case of rank-1 arrays converted to generic or non-generic forms of `IList`, it is also possible to set elements by index, subject to the same runtime array covariance checks as described above.</span></span> <span data-ttu-id="ab879-214">O comportamento de todos os outros métodos de interface não está definido pela especificação de linguagem do VB; é até o tempo de execução subjacente.</span><span class="sxs-lookup"><span data-stu-id="ab879-214">The behavior of all other interface methods is undefined by the VB language specification; it is up to the underlying runtime.</span></span>

## <a name="value-type-conversions"></a><span data-ttu-id="ab879-215">Conversões de tipo de valor</span><span class="sxs-lookup"><span data-stu-id="ab879-215">Value Type Conversions</span></span>

<span data-ttu-id="ab879-216">Um valor de tipo de valor pode ser convertido para um dos seus tipos de referência de base ou um tipo de interface que ele implementa através de um processo chamado *conversão boxing*.</span><span class="sxs-lookup"><span data-stu-id="ab879-216">A value type value can be converted to one of its base reference types or an interface type that it implements through a process called *boxing*.</span></span> <span data-ttu-id="ab879-217">Quando um valor de tipo de valor é convertido, o valor é copiado do local onde ele reside na pilha do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ab879-217">When a value type value is boxed, the value is copied from the location where it lives onto the .NET Framework heap.</span></span> <span data-ttu-id="ab879-218">Uma referência a esse local no heap, em seguida, é retornada e pode ser armazenada em uma variável de tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="ab879-218">A reference to this location on the heap is then returned and can be stored in a reference type variable.</span></span> <span data-ttu-id="ab879-219">Essa referência também é conhecida como um *box* instância do tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-219">This reference is also referred to as a *boxed* instance of the value type.</span></span> <span data-ttu-id="ab879-220">A instância do box tem a mesma semântica que um tipo de referência em vez de um tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-220">The boxed instance has the same semantics as a reference type instead of a value type.</span></span>

<span data-ttu-id="ab879-221">Tipos de valor demarcado podem ser convertidos de volta para seu tipo de valor original por meio de um processo chamado *unboxing*.</span><span class="sxs-lookup"><span data-stu-id="ab879-221">Boxed value types can be converted back to their original value type through a process called *unboxing*.</span></span> <span data-ttu-id="ab879-222">Quando um tipo de valor demarcado é não demarcado, o valor é copiado do heap em um variável local.</span><span class="sxs-lookup"><span data-stu-id="ab879-222">When a boxed value type is unboxed, the value is copied from the heap into a variable location.</span></span> <span data-ttu-id="ab879-223">Daí em diante, ele se comporta como se fosse um tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-223">From that point on, it behaves as if it was a value type.</span></span> <span data-ttu-id="ab879-224">Ao fazer unboxing um tipo de valor, o valor deve ser um valor nulo ou uma instância do tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-224">When unboxing a value type, the value must be a null value or an instance of the value type.</span></span> <span data-ttu-id="ab879-225">Caso contrário, um `System.InvalidCastException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="ab879-225">Otherwise a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="ab879-226">Se o valor for uma instância de um tipo enumerado, esse valor também pode ser desconvertido para o tipo enumerado do tipo subjacente ou outro tipo enumerado que tem o mesmo tipo subjacente.</span><span class="sxs-lookup"><span data-stu-id="ab879-226">If the value is an instance of an enumerated type, that value can also be unboxed to the enumerated type's underlying type or another enumerated type that has the same underlying type.</span></span> <span data-ttu-id="ab879-227">Um valor nulo é tratado como se fosse o literal `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ab879-227">A null value is treated as if it were the literal `Nothing`.</span></span>

<span data-ttu-id="ab879-228">Para dar suporte a anulável tipos de valor, bem, o tipo de valor `System.Nullable(Of T)` é tratado especialmente ao realizar conversões boxing e unboxing.</span><span class="sxs-lookup"><span data-stu-id="ab879-228">To support nullable value types well, the value type `System.Nullable(Of T)` is treated specially when doing boxing and unboxing.</span></span> <span data-ttu-id="ab879-229">Conversão boxing de um valor do tipo `Nullable(Of T)` resulta em um valor demarcado do tipo `T` se o valor `HasValue` é de propriedade `True` ou um valor de `Nothing` se o valor `HasValue` propriedade é `False`.</span><span class="sxs-lookup"><span data-stu-id="ab879-229">Boxing a value of type `Nullable(Of T)` results in a boxed value of type `T` if the value's `HasValue` property is `True` or a value of `Nothing` if the value's `HasValue` property is `False`.</span></span> <span data-ttu-id="ab879-230">Desconversão de um valor do tipo `T` à `Nullable(Of T)` resulta em uma instância do `Nullable(Of T)` cujo `Value` propriedade é o valor demarcado e cujo `HasValue` é de propriedade `True`.</span><span class="sxs-lookup"><span data-stu-id="ab879-230">Unboxing a value of type `T` to `Nullable(Of T)` results in an instance of `Nullable(Of T)` whose `Value` property is the boxed value and whose `HasValue` property is `True`.</span></span> <span data-ttu-id="ab879-231">O valor `Nothing` pode ser submetido à conversão unboxing para `Nullable(Of T)` para qualquer `T` e resulta em um valor cuja `HasValue` é de propriedade `False`.</span><span class="sxs-lookup"><span data-stu-id="ab879-231">The value `Nothing` can be unboxed to `Nullable(Of T)` for any `T` and results in a value whose `HasValue` property is `False`.</span></span> <span data-ttu-id="ab879-232">Porque os tipos de valor demarcado se comportam como referência de tipos, é possível criar várias referências para o mesmo valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-232">Because boxed value types behave like reference types, it is possible to create multiple references to the same value.</span></span> <span data-ttu-id="ab879-233">Para os tipos primitivos e tipos enumerados, isso é irrelevante porque as instâncias desses tipos são *imutável*.</span><span class="sxs-lookup"><span data-stu-id="ab879-233">For the primitive types and enumerated types, this is irrelevant because instances of those types are *immutable*.</span></span> <span data-ttu-id="ab879-234">Ou seja, não é possível modificar uma instância demarcada desses tipos, portanto, não é possível observar o fato de que há várias referências para o mesmo valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-234">That is, it is not possible to modify a boxed instance of those types, so it is not possible to observe the fact that there are multiple references to the same value.</span></span>

<span data-ttu-id="ab879-235">Por outro lado, estruturas, podem ser mutáveis se suas variáveis de instância estão acessíveis ou se seus métodos ou propriedades modificar suas variáveis de instância.</span><span class="sxs-lookup"><span data-stu-id="ab879-235">Structures, on the other hand, may be mutable if its instance variables are accessible or if its methods or properties modify its instance variables.</span></span> <span data-ttu-id="ab879-236">Se uma referência a uma estrutura demarcada é usada para modificar a estrutura, todas as referências à estrutura demarcada verá a alteração.</span><span class="sxs-lookup"><span data-stu-id="ab879-236">If one reference to a boxed structure is used to modify the structure, then all references to the boxed structure will see the change.</span></span> <span data-ttu-id="ab879-237">Como esse resultado pode ser inesperado, quando um valor digitado como `Object` é copiado de um local para outro valor demarcado tipos automaticamente serão clonados no heap, em vez de simplesmente ter suas referências copiadas.</span><span class="sxs-lookup"><span data-stu-id="ab879-237">Because this result may be unexpected, when a value typed as `Object` is copied from one location to another boxed value types will automatically be cloned on the heap instead of merely having their references copied.</span></span> <span data-ttu-id="ab879-238">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab879-238">For example:</span></span>

```vb
Class Class1
    Public Value As Integer = 0
End Class

Structure Struct1
    Public Value As Integer
End Structure

Module Test
    Sub Main()
        Dim val1 As Object = New Struct1()
        Dim val2 As Object = val1

        val2.Value = 123

        Dim ref1 As Object = New Class1()
        Dim ref2 As Object = ref1

        ref2.Value = 123

        Console.WriteLine("Values: " & val1.Value & ", " & val2.Value)
        Console.WriteLine("Refs: " & ref1.Value & ", " & ref2.Value)
    End Sub
End Module
```

<span data-ttu-id="ab879-239">A saída do programa é:</span><span class="sxs-lookup"><span data-stu-id="ab879-239">The output of the program is:</span></span>

```
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="ab879-240">A atribuição para o campo da variável local `val2` não afeta o campo da variável local `val1` porque quando o box `Struct1` foi atribuído a `val2`, foi feita uma cópia do valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-240">The assignment to the field of the local variable `val2` does not impact the field of the local variable `val1` because when the boxed `Struct1` was assigned to `val2`, a copy of the value was made.</span></span> <span data-ttu-id="ab879-241">Por outro lado, a atribuição `ref2.Value = 123` afeta o objeto que ambos `ref1` e `ref2` referências.</span><span class="sxs-lookup"><span data-stu-id="ab879-241">In contrast, the assignment `ref2.Value = 123` affects the object that both `ref1` and `ref2` references.</span></span>

<span data-ttu-id="ab879-242">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab879-242">__Note.__</span></span> <span data-ttu-id="ab879-243">Cópia da estrutura não é feita para estruturas demarcadas digitadas como `System.ValueType` porque não é possível para ligação tardia fora de `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="ab879-243">Structure copying is not done for boxed structures typed as `System.ValueType` because it is not possible to late bind off of `System.ValueType`.</span></span>

<span data-ttu-id="ab879-244">Há uma exceção à regra que boxed tipos serão copiados na atribuição de valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-244">There is one exception to the rule that boxed value types will be copied on assignment.</span></span> <span data-ttu-id="ab879-245">Se uma referência de tipo de valor demarcado é armazenada dentro de outro tipo, a referência interna não será copiada.</span><span class="sxs-lookup"><span data-stu-id="ab879-245">If a boxed value type reference is stored within another type, the inner reference will not be copied.</span></span> <span data-ttu-id="ab879-246">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab879-246">For example:</span></span>

```vb
Structure Struct1
    Public Value As Object
End Structure

Module Test
    Sub Main()
        Dim val1 As Struct1
        Dim val2 As Struct1

        val1.Value = New Struct1()
        val1.Value.Value = 10

        val2 = val1
        val2.Value.Value = 123
        Console.WriteLine("Values: " & val1.Value.Value & ", " & _
            val2.Value.Value)
    End Sub
End Module
```

<span data-ttu-id="ab879-247">A saída do programa é:</span><span class="sxs-lookup"><span data-stu-id="ab879-247">The output of the program is:</span></span>

```
Values: 123, 123
```

<span data-ttu-id="ab879-248">Isso ocorre porque o valor demarcado interno não é copiado quando o valor é copiado.</span><span class="sxs-lookup"><span data-stu-id="ab879-248">This is because the inner boxed value is not copied when the value is copied.</span></span> <span data-ttu-id="ab879-249">Assim, ambos `val1.Value` e `val2.Value` tem uma referência para o mesmo tipo de valor Demarcado.</span><span class="sxs-lookup"><span data-stu-id="ab879-249">Thus, both `val1.Value` and `val2.Value` have a reference to the same boxed value type.</span></span>

<span data-ttu-id="ab879-250">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab879-250">__Note.__</span></span> <span data-ttu-id="ab879-251">O fato de que os tipos de valor demarcado interna não são copiados é uma limitação do .NET digite sistema – para garantir que todos os tipos de valor demarcado interna foram copiados sempre que um valor do tipo `Object` foi copiado seria extremamente dispendioso.</span><span class="sxs-lookup"><span data-stu-id="ab879-251">The fact that inner boxed value types are not copied is a limitation of the .NET type system -- to ensure that all inner boxed value types were copied whenever a value of type `Object` was copied would be prohibitively expensive.</span></span>

<span data-ttu-id="ab879-252">Como descrito anteriormente, demarcado valor tipos só podem ser desconvertidos para seu tipo original.</span><span class="sxs-lookup"><span data-stu-id="ab879-252">As previously described, boxed value types can only be unboxed to their original type.</span></span> <span data-ttu-id="ab879-253">Tipos primitivos box, no entanto, são tratados especialmente quando digitado como `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab879-253">Boxed primitive types, however, are treated specially when typed as `Object`.</span></span> <span data-ttu-id="ab879-254">Eles podem ser convertidos em qualquer tipo primitivo que eles têm uma conversão.</span><span class="sxs-lookup"><span data-stu-id="ab879-254">They can be converted to any other primitive type that they have a conversion to.</span></span> <span data-ttu-id="ab879-255">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab879-255">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

<span data-ttu-id="ab879-256">Normalmente, o box `Integer` valor `5` não pôde ser unboxed em um `Byte` variável.</span><span class="sxs-lookup"><span data-stu-id="ab879-256">Normally, the boxed `Integer` value `5` could not be unboxed into a `Byte` variable.</span></span> <span data-ttu-id="ab879-257">No entanto, porque `Integer` e `Byte` são tipos primitivos e tem uma conversão, a conversão é permitida.</span><span class="sxs-lookup"><span data-stu-id="ab879-257">However, because `Integer` and `Byte` are primitive types and have a conversion, the conversion is allowed.</span></span>

<span data-ttu-id="ab879-258">É importante observar que a conversão de um tipo de valor a uma interface é diferente do que um argumento genérico restrito a uma interface.</span><span class="sxs-lookup"><span data-stu-id="ab879-258">It is important to note that converting a value type to an interface is different than a generic argument constrained to an interface.</span></span> <span data-ttu-id="ab879-259">Ao acessar membros de interface em um parâmetro de tipo restrita (ou chamar métodos em `Object`), conversão boxing não ocorre como quando um tipo de valor é convertido em uma interface e um membro de interface é acessado.</span><span class="sxs-lookup"><span data-stu-id="ab879-259">When accessing interface members on a constrained type parameter (or calling methods on `Object`), boxing does not occur as it does when a value type is converted to an interface and an interface member is accessed.</span></span> <span data-ttu-id="ab879-260">Por exemplo, suponha que uma interface `ICounter` contém um método `Increment` que pode ser usado para modificar um valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-260">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="ab879-261">Se `ICounter` é usado como uma restrição, a implementação do `Increment` método for chamado com uma referência à variável que `Increment` foi chamado, não uma cópia demarcada:</span><span class="sxs-lookup"><span data-stu-id="ab879-261">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, not a boxed copy:</span></span>

```vb
Interface ICounter
    Sub Increment()
    ReadOnly Property Value() As Integer
End Interface

Structure Counter
    Implements ICounter

    Dim _value As Integer

    Property Value() As Integer Implements ICounter.Value
        Get
            Return _value
        End Get
    End Property

    Sub Increment() Implements ICounter.Increment
       value += 1
    End Sub
End Structure

Module Test
      Sub Test(Of T As ICounter)(x As T)
         Console.WriteLine(x.value)
         x.Increment()                     ' Modify x
         Console.WriteLine(x.value)
         CType(x, ICounter).Increment()    ' Modify boxed copy of x
         Console.WriteLine(x.value)
      End Sub

      Sub Main()
         Dim x As Counter
         Test(x)
      End Sub
End Module
```

<span data-ttu-id="ab879-262">A primeira chamada para `Increment` modifica o valor na variável `x`.</span><span class="sxs-lookup"><span data-stu-id="ab879-262">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="ab879-263">Isso não é equivalente a segunda chamada para `Increment`, que modifica o valor em uma cópia demarcada do `x`.</span><span class="sxs-lookup"><span data-stu-id="ab879-263">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="ab879-264">Assim, a saída do programa é:</span><span class="sxs-lookup"><span data-stu-id="ab879-264">Thus, the output of the program is:</span></span>

```
0
1
1
```

### <a name="nullable-value-type-conversions"></a><span data-ttu-id="ab879-265">Conversões de tipo de valor anulável</span><span class="sxs-lookup"><span data-stu-id="ab879-265">Nullable Value Type Conversions</span></span>

<span data-ttu-id="ab879-266">Um tipo de valor `T` pode converter de e para a versão que permite valor nula do tipo, `T?`.</span><span class="sxs-lookup"><span data-stu-id="ab879-266">A value type `T` can convert to and from the nullable version of the type, `T?`.</span></span> <span data-ttu-id="ab879-267">A conversão de `T?` à `T` lança uma `System.InvalidOperationException` exceção se o valor que está sendo convertido for `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ab879-267">The conversion from `T?` to `T` throws a `System.InvalidOperationException` exception if the value being converted is `Nothing`.</span></span> <span data-ttu-id="ab879-268">Além disso, `T?` tem uma conversão para um tipo `S` se `T` tem uma conversão intrínseco para `S`.</span><span class="sxs-lookup"><span data-stu-id="ab879-268">Also, `T?` has a conversion to a type `S` if `T` has an intrinsic conversion to `S`.</span></span> <span data-ttu-id="ab879-269">E se `S` é um tipo de valor, em seguida, as seguintes conversões intrínsecas existem entre `T?` e `S?`:</span><span class="sxs-lookup"><span data-stu-id="ab879-269">And if `S` is a value type, then the following intrinsic conversions exist between `T?` and `S?`:</span></span>

* <span data-ttu-id="ab879-270">Uma conversão de mesma classificação (redução ou ampliação) da `T?` para `S?`.</span><span class="sxs-lookup"><span data-stu-id="ab879-270">A conversion of the same classification (narrowing or widening) from `T?` to `S?`.</span></span>

* <span data-ttu-id="ab879-271">Uma conversão de mesma classificação (redução ou ampliação) da `T` para `S?`.</span><span class="sxs-lookup"><span data-stu-id="ab879-271">A conversion of the same classification (narrowing or widening) from `T` to `S?`.</span></span>

* <span data-ttu-id="ab879-272">Uma conversão redutora de `S?` para `T`.</span><span class="sxs-lookup"><span data-stu-id="ab879-272">A narrowing conversion from `S?` to `T`.</span></span>

<span data-ttu-id="ab879-273">Por exemplo, uma conversão de ampliação intrínseca existe da `Integer?` à `Long?` porque existe uma conversão de ampliação intrínseco da `Integer` para `Long`:</span><span class="sxs-lookup"><span data-stu-id="ab879-273">For example, an intrinsic widening conversion exists from `Integer?` to `Long?` because an intrinsic widening conversion exists from `Integer` to `Long`:</span></span>

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

<span data-ttu-id="ab879-274">Durante a conversão de `T?` para `S?`, se o valor de `T?` é `Nothing`, em seguida, o valor de `S?` será `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ab879-274">When converting from `T?` to `S?`, if the value of `T?` is `Nothing`, then the value of `S?` will be `Nothing`.</span></span> <span data-ttu-id="ab879-275">Durante a conversão de `S?` à `T` ou `T?` ao `S`, se o valor de `T?` ou `S?` é `Nothing`, um `System.InvalidCastException` exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="ab879-275">When converting from `S?` to `T` or `T?` to `S`, if the value of `T?` or `S?` is `Nothing`, a `System.InvalidCastException` exception will be thrown.</span></span>

<span data-ttu-id="ab879-276">Devido ao comportamento do tipo subjacente `System.Nullable(Of T)`, quando um tipo de valor anulável `T?` é convertido, o resultado é um valor demarcado do tipo `T`, não um valor demarcado do tipo `T?`.</span><span class="sxs-lookup"><span data-stu-id="ab879-276">Because of the behavior of the underlying type `System.Nullable(Of T)`, when a nullable value type `T?` is boxed, the result is a boxed value of type `T`, not a boxed value of type `T?`.</span></span> <span data-ttu-id="ab879-277">E, por outro lado, ao fazer unboxing em um tipo de valor anulável `T?`, o valor será encapsulado por `System.Nullable(Of T)`, e `Nothing` será ser desconvertidos para um valor nulo do tipo `T?`.</span><span class="sxs-lookup"><span data-stu-id="ab879-277">And, conversely, when unboxing to a nullable value type `T?`, the value will be wrapped by `System.Nullable(Of T)`, and `Nothing` will be unboxed to a null value of type `T?`.</span></span> <span data-ttu-id="ab879-278">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab879-278">For example:</span></span>

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

<span data-ttu-id="ab879-279">Um efeito colateral desse comportamento é o tipo de um valor anulável `T?` é exibida implementar todas as interfaces de `T`, porque a conversão de um tipo de valor a uma interface requer que o tipo a ser Demarcado.</span><span class="sxs-lookup"><span data-stu-id="ab879-279">A side effect of this behavior is that a nullable value type `T?` appears to implement all of the interfaces of `T`, because converting a value type to an interface requires the type to be boxed.</span></span> <span data-ttu-id="ab879-280">Como resultado, `T?` pode ser convertido em todas as interfaces que `T` pode ser convertido em.</span><span class="sxs-lookup"><span data-stu-id="ab879-280">As a result, `T?` is convertible to all the interfaces that `T` is convertible to.</span></span> <span data-ttu-id="ab879-281">É importante observar, no entanto, que tipo de um valor anulável `T?` não implementa as interfaces de realmente `T` para fins de verificação de restrição genérica ou reflexão.</span><span class="sxs-lookup"><span data-stu-id="ab879-281">It is important to note, however, that a nullable value type `T?` does not actually implement the interfaces of `T` for the purposes of generic constraint checking or reflection.</span></span> <span data-ttu-id="ab879-282">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab879-282">For example:</span></span>

```vb
Interface I1
End Interface

Structure T1
    Implements I1
    ...
End Structure

Module Test
    Sub M1(Of T As I1)(ByVal x As T)
    End Sub

    Sub Main()
        Dim x? As T1 = Nothing
        Dim y As I1 = x                ' Valid
        M1(x)                          ' Error: x? does not satisfy I1 constraint
    End Sub
End Module
```

## <a name="string-conversions"></a><span data-ttu-id="ab879-283">Conversões de cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="ab879-283">String Conversions</span></span>

<span data-ttu-id="ab879-284">Convertendo `Char` em `String` resulta em uma cadeia de caracteres cujo primeiro caractere é o valor do caractere.</span><span class="sxs-lookup"><span data-stu-id="ab879-284">Converting `Char` into `String` results in a string whose first character is the character value.</span></span> <span data-ttu-id="ab879-285">Convertendo `String` em `Char` resulta em um caractere cujo valor é o primeiro caractere da cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="ab879-285">Converting `String` into `Char` results in a character whose value is the first character of the string.</span></span> <span data-ttu-id="ab879-286">Converter uma matriz de `Char` em `String` resulta em uma cadeia de caracteres cujos caracteres são os elementos da matriz.</span><span class="sxs-lookup"><span data-stu-id="ab879-286">Converting an array of `Char` into `String` results in a string whose characters are the elements of the array.</span></span> <span data-ttu-id="ab879-287">Convertendo `String` em uma matriz de `Char` resulta em uma matriz de caracteres cujos elementos são os caracteres da cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="ab879-287">Converting `String` into an array of `Char` results in an array of characters whose elements are the characters of the string.</span></span>

<span data-ttu-id="ab879-288">As conversões exatas entre `String` e `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`e vice-versa, estão além do escopo desta especificação e dependem com exceção de um detalhe de implementação.</span><span class="sxs-lookup"><span data-stu-id="ab879-288">The exact conversions between `String` and `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`, and vice versa, are beyond the scope of this specification and are implementation dependent with the exception of one detail.</span></span> <span data-ttu-id="ab879-289">Conversões de cadeia de caracteres sempre considere a cultura atual do ambiente de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ab879-289">String conversions always consider the current culture of the run-time environment.</span></span> <span data-ttu-id="ab879-290">Como tal, eles devem ser executados em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ab879-290">As such, they must be performed at run time.</span></span>

## <a name="widening-conversions"></a><span data-ttu-id="ab879-291">Conversões de expansão</span><span class="sxs-lookup"><span data-stu-id="ab879-291">Widening Conversions</span></span>

<span data-ttu-id="ab879-292">Conversões ampliadoras nunca overflow, mas podem envolver uma perda de precisão.</span><span class="sxs-lookup"><span data-stu-id="ab879-292">Widening conversions never overflow but may entail a loss of precision.</span></span> <span data-ttu-id="ab879-293">Conversões de expansão são as seguintes conversões:</span><span class="sxs-lookup"><span data-stu-id="ab879-293">The following conversions are widening conversions:</span></span>

<span data-ttu-id="ab879-294">__Conversões de identidade/padrão__</span><span class="sxs-lookup"><span data-stu-id="ab879-294">__Identity/Default conversions__</span></span>

* <span data-ttu-id="ab879-295">De um tipo para si mesmo.</span><span class="sxs-lookup"><span data-stu-id="ab879-295">From a type to itself.</span></span>

* <span data-ttu-id="ab879-296">De um tipo de delegado anônimo gerado para uma reclassificação de método lambda para qualquer tipo de delegado com uma assinatura idêntica.</span><span class="sxs-lookup"><span data-stu-id="ab879-296">From an anonymous delegate type generated for a lambda method reclassification to any delegate type with an identical signature.</span></span>

* <span data-ttu-id="ab879-297">Do literal `Nothing` para um tipo.</span><span class="sxs-lookup"><span data-stu-id="ab879-297">From the literal `Nothing` to a type.</span></span>

<span data-ttu-id="ab879-298">__Conversões numéricas__</span><span class="sxs-lookup"><span data-stu-id="ab879-298">__Numeric conversions__</span></span>

* <span data-ttu-id="ab879-299">Partir `Byte` à `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, ou `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-299">From `Byte` to `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ab879-300">Partir `SByte` à `Short`, `Integer`, `Long`, `Decimal`, `Single`, ou `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-300">From `SByte` to `Short`, `Integer`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ab879-301">Partir `UShort` à `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, ou `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-301">From `UShort` to `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ab879-302">Partir `Short` à `Integer`, `Long`, `Decimal`, `Single` ou `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-302">From `Short` to `Integer`, `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="ab879-303">Partir `UInteger` à `ULong`, `Long`, `Decimal`, `Single`, ou `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-303">From `UInteger` to `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ab879-304">Partir `Integer` à `Long`, `Decimal`, `Single` ou `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-304">From `Integer` to `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="ab879-305">Partir `ULong` à `Decimal`, `Single`, ou `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-305">From `ULong` to `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ab879-306">Partir `Long` à `Decimal`, `Single` ou `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-306">From `Long` to `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="ab879-307">Partir `Decimal` à `Single` ou `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-307">From `Decimal` to `Single` or `Double`.</span></span>

* <span data-ttu-id="ab879-308">Partir `Single` para `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-308">From `Single` to `Double`.</span></span>

* <span data-ttu-id="ab879-309">Do literal `0` para um tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="ab879-309">From the literal `0` to an enumerated type.</span></span> <span data-ttu-id="ab879-310">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab879-310">(__Note.__</span></span> <span data-ttu-id="ab879-311">A conversão de `0` para qualquer tipo enumerado ampliação para simplificar o teste de sinalizadores.</span><span class="sxs-lookup"><span data-stu-id="ab879-311">The conversion from `0` to any enumerated type is widening to simplify testing flags.</span></span> <span data-ttu-id="ab879-312">Por exemplo, se `Values` é um tipo enumerado com um valor `One`, você pode testar uma variável `v` do tipo `Values` dizendo `(v And Values.One) = 0`.)</span><span class="sxs-lookup"><span data-stu-id="ab879-312">For example, if `Values` is an enumerated type with a value `One`, you could test a variable `v` of type `Values` by saying `(v And Values.One) = 0`.)</span></span>

* <span data-ttu-id="ab879-313">Em um tipo enumerado para seu tipo numérico subjacente ou para um tipo numérico que seu tipo numérico subjacente tem uma conversão de ampliação.</span><span class="sxs-lookup"><span data-stu-id="ab879-313">From an enumerated type to its underlying numeric type, or to a numeric type that its underlying numeric type has a widening conversion to.</span></span>

* <span data-ttu-id="ab879-314">De uma expressão de constante do tipo `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`, ou `SByte` fornecido para um tipo mais estreito, o valor da expressão constante está dentro do intervalo de tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab879-314">From a constant expression of type `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`, or `SByte` to a narrower type, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="ab879-315">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab879-315">(__Note.__</span></span> <span data-ttu-id="ab879-316">Conversões de `UInteger` ou `Integer` à `Single`, `ULong` ou `Long` para `Single` ou `Double`, ou `Decimal` para `Single` ou `Double` pode causar perda de precisão, mas nunca causa a perda de magnitude.</span><span class="sxs-lookup"><span data-stu-id="ab879-316">Conversions from `UInteger` or `Integer` to `Single`, `ULong` or `Long` to `Single` or `Double`, or `Decimal` to `Single` or `Double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="ab879-317">As outras conversões ampliadoras numéricas nunca perdem todas as informações.)</span><span class="sxs-lookup"><span data-stu-id="ab879-317">The other widening numeric conversions never lose any information.)</span></span>

<span data-ttu-id="ab879-318">__Conversões de referência__</span><span class="sxs-lookup"><span data-stu-id="ab879-318">__Reference conversions__</span></span>

* <span data-ttu-id="ab879-319">De um tipo de referência em um tipo base.</span><span class="sxs-lookup"><span data-stu-id="ab879-319">From a reference type to a base type.</span></span>

* <span data-ttu-id="ab879-320">De um tipo de referência para um tipo de interface, fornecidas que o tipo implementa a interface ou uma interface compatível com variante.</span><span class="sxs-lookup"><span data-stu-id="ab879-320">From a reference type to an interface type, provided that the type implements the interface or a variant compatible interface.</span></span>

* <span data-ttu-id="ab879-321">De um tipo de interface para `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab879-321">From an interface type to `Object`.</span></span>

* <span data-ttu-id="ab879-322">De um tipo de interface para um tipo de interface compatível com variante.</span><span class="sxs-lookup"><span data-stu-id="ab879-322">From an interface type to a variant compatible interface type.</span></span>

* <span data-ttu-id="ab879-323">Tipo de delegado de um tipo de delegado como uma variante compatível.</span><span class="sxs-lookup"><span data-stu-id="ab879-323">From a delegate type to a variant compatible delegate type.</span></span> <span data-ttu-id="ab879-324">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="ab879-324">(__Note.__</span></span> <span data-ttu-id="ab879-325">Muitas outras conversões de referência são deduzidos por essas regras.</span><span class="sxs-lookup"><span data-stu-id="ab879-325">Many other reference conversions are implied by these rules.</span></span> <span data-ttu-id="ab879-326">Por exemplo, os delegados anônimos são tipos de referência que herdam de `System.MulticastDelegate`; tipos de matriz são tipos de referência que herdaram `System.Array`; anônimos tipos são tipos de referência que herdam de `System.Object`.)</span><span class="sxs-lookup"><span data-stu-id="ab879-326">For example, anonymous delegates are reference types that inherit from `System.MulticastDelegate`; array types are reference types that inherit from `System.Array`; anonymous types are reference types that inherit from `System.Object`.)</span></span>

<span data-ttu-id="ab879-327">__Conversões de delegado anônimas__</span><span class="sxs-lookup"><span data-stu-id="ab879-327">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="ab879-328">De um anônimo gerado para uma reclassificação de método lambda para qualquer tipo de delegado mais amplo de tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="ab879-328">From an anonymous delegate type generated for a lambda method reclassification to any wider delegate type.</span></span>

<span data-ttu-id="ab879-329">__Conversões de matriz__</span><span class="sxs-lookup"><span data-stu-id="ab879-329">__Array conversions__</span></span>

* <span data-ttu-id="ab879-330">De um tipo de matriz `S` com um tipo de elemento `Se` para um tipo de matriz `T` com um tipo de elemento `Te`, desde que todos os itens a seguir forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="ab879-330">From an array type `S` with an element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="ab879-331">`S` e `T` diferem apenas no tipo de elemento.</span><span class="sxs-lookup"><span data-stu-id="ab879-331">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="ab879-332">Ambos `Se` e `Te` são tipos de referência ou parâmetros de tipo que são conhecidos como um tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="ab879-332">Both `Se` and `Te` are reference types or are type parameters known to be a reference type.</span></span>

  * <span data-ttu-id="ab879-333">Uma referência, matriz ou tipo de ampliação existe conversão de parâmetro do `Se` para `Te`.</span><span class="sxs-lookup"><span data-stu-id="ab879-333">A widening reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="ab879-334">De um tipo de matriz `S` com um tipo de elemento enumerado `Se` para um tipo de matriz `T` com um tipo de elemento `Te`, desde que todos os itens a seguir forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="ab879-334">From an array type `S` with an enumerated element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="ab879-335">`S` e `T` diferem apenas no tipo de elemento.</span><span class="sxs-lookup"><span data-stu-id="ab879-335">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="ab879-336">`Te` é o tipo subjacente de `Se`.</span><span class="sxs-lookup"><span data-stu-id="ab879-336">`Te` is the underlying type of `Se`.</span></span>

* <span data-ttu-id="ab879-337">De um tipo de matriz `S` de classificação 1, com um tipo de elemento enumerado `Se`, para `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`, e `IEnumerable(Of Te)`, desde que uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="ab879-337">From an array type `S` of rank 1 with an enumerated element type `Se`, to `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`, and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="ab879-338">Ambos `Se` e `Te` são tipos de referência ou parâmetros de tipo conhecido para ser uma referência de tipo e uma referência de ampliação, matriz ou existe conversão de parâmetro de tipo do `Se` para `Te`; ou</span><span class="sxs-lookup"><span data-stu-id="ab879-338">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a widening reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>

  * <span data-ttu-id="ab879-339">`Te` é o tipo subjacente de `Se`; ou</span><span class="sxs-lookup"><span data-stu-id="ab879-339">`Te` is the underlying type of `Se`; or</span></span>

  * <span data-ttu-id="ab879-340">`Te` é idêntico a `Se`</span><span class="sxs-lookup"><span data-stu-id="ab879-340">`Te` is identical to `Se`</span></span>

<span data-ttu-id="ab879-341">__Conversões de tipo de valor__</span><span class="sxs-lookup"><span data-stu-id="ab879-341">__Value Type conversions__</span></span>

* <span data-ttu-id="ab879-342">De um tipo de valor em um tipo base.</span><span class="sxs-lookup"><span data-stu-id="ab879-342">From a value type to a base type.</span></span>

* <span data-ttu-id="ab879-343">De um tipo de valor em um tipo de interface que o tipo implementa.</span><span class="sxs-lookup"><span data-stu-id="ab879-343">From a value type to an interface type that the type implements.</span></span>

<span data-ttu-id="ab879-344">__Conversões de tipo de valor anuláveis__</span><span class="sxs-lookup"><span data-stu-id="ab879-344">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="ab879-345">De um tipo `T` para o tipo `T?`.</span><span class="sxs-lookup"><span data-stu-id="ab879-345">From a type `T` to the type `T?`.</span></span>

* <span data-ttu-id="ab879-346">De um tipo `T?` em um tipo `S?`, em que há uma conversão de ampliação do tipo `T` para o tipo `S`.</span><span class="sxs-lookup"><span data-stu-id="ab879-346">From a type `T?` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="ab879-347">De um tipo `T` em um tipo `S?`, em que há uma conversão de ampliação do tipo `T` para o tipo `S`.</span><span class="sxs-lookup"><span data-stu-id="ab879-347">From a type `T` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="ab879-348">De um tipo `T?` a uma interface de tipo que o tipo `T` implementa.</span><span class="sxs-lookup"><span data-stu-id="ab879-348">From a type `T?` to an interface type that the type `T` implements.</span></span>

<span data-ttu-id="ab879-349">__Conversões de cadeia de caracteres__</span><span class="sxs-lookup"><span data-stu-id="ab879-349">__String conversions__</span></span>

* <span data-ttu-id="ab879-350">Partir `Char` para `String`.</span><span class="sxs-lookup"><span data-stu-id="ab879-350">From `Char` to `String`.</span></span>

* <span data-ttu-id="ab879-351">Partir `Char()` para `String`.</span><span class="sxs-lookup"><span data-stu-id="ab879-351">From `Char()` to `String`.</span></span>

<span data-ttu-id="ab879-352">__Conversões de tipo de parâmetro__</span><span class="sxs-lookup"><span data-stu-id="ab879-352">__Type Parameter conversions__</span></span>

* <span data-ttu-id="ab879-353">De um parâmetro de tipo para `Object`.</span><span class="sxs-lookup"><span data-stu-id="ab879-353">From a type parameter to `Object`.</span></span>

* <span data-ttu-id="ab879-354">De um parâmetro de tipo para uma restrição de tipo de interface ou qualquer variante de interface compatível com uma restrição de tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="ab879-354">From a type parameter to an interface type constraint or any interface variant compatible with an interface type constraint.</span></span>

* <span data-ttu-id="ab879-355">De um parâmetro de tipo para uma interface implementada por uma restrição de classe.</span><span class="sxs-lookup"><span data-stu-id="ab879-355">From a type parameter to an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="ab879-356">De um parâmetro de tipo como uma variante de interface compatível com uma interface implementada por uma restrição de classe.</span><span class="sxs-lookup"><span data-stu-id="ab879-356">From a type parameter to an interface variant compatible with an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="ab879-357">De um parâmetro de tipo para uma restrição de classe ou um tipo base da restrição de classe.</span><span class="sxs-lookup"><span data-stu-id="ab879-357">From a type parameter to a class constraint, or a base type of the class constraint.</span></span>

* <span data-ttu-id="ab879-358">De um parâmetro de tipo `T` a uma restrição de parâmetro de tipo `Tx`, ou qualquer coisa `Tx` tem uma conversão de ampliação.</span><span class="sxs-lookup"><span data-stu-id="ab879-358">From a type parameter `T` to a type parameter constraint `Tx`, or anything `Tx` has a widening conversion to.</span></span>

## <a name="narrowing-conversions"></a><span data-ttu-id="ab879-359">Conversões de redução</span><span class="sxs-lookup"><span data-stu-id="ab879-359">Narrowing Conversions</span></span>

<span data-ttu-id="ab879-360">Conversões de estreitamento são conversões que não podem ser provado que ele é sempre têm êxito, conversões que são conhecidas como possivelmente perder informações e conversões entre domínios de tipos diferentes o suficiente para justificar a notação de estreitamento.</span><span class="sxs-lookup"><span data-stu-id="ab879-360">Narrowing conversions are conversions that cannot be proved to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit narrowing notation.</span></span> <span data-ttu-id="ab879-361">As conversões a seguir são classificadas como conversões de estreitamento:</span><span class="sxs-lookup"><span data-stu-id="ab879-361">The following conversions are classified as narrowing conversions:</span></span>

<span data-ttu-id="ab879-362">__Conversões de boolianos__</span><span class="sxs-lookup"><span data-stu-id="ab879-362">__Boolean conversions__</span></span>

* <span data-ttu-id="ab879-363">Partir `Boolean` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, ou `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-363">From `Boolean` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ab879-364">Partir `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, ou `Double` para `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="ab879-364">From `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double` to `Boolean`.</span></span>

<span data-ttu-id="ab879-365">__Conversões numéricas__</span><span class="sxs-lookup"><span data-stu-id="ab879-365">__Numeric conversions__</span></span>

* <span data-ttu-id="ab879-366">Partir `Byte` para `SByte`.</span><span class="sxs-lookup"><span data-stu-id="ab879-366">From `Byte` to `SByte`.</span></span>

* <span data-ttu-id="ab879-367">Partir `SByte` à `Byte`, `UShort`, `UInteger`, ou `ULong`.</span><span class="sxs-lookup"><span data-stu-id="ab879-367">From `SByte` to `Byte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="ab879-368">Partir `UShort` à `Byte`, `SByte`, ou `Short`.</span><span class="sxs-lookup"><span data-stu-id="ab879-368">From `UShort` to `Byte`, `SByte`, or `Short`.</span></span>

* <span data-ttu-id="ab879-369">Partir `Short` à `Byte`, `SByte`, `UShort`, `UInteger`, ou `ULong`.</span><span class="sxs-lookup"><span data-stu-id="ab879-369">From `Short` to `Byte`, `SByte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="ab879-370">Partir `UInteger` à `Byte`, `SByte`, `UShort`, `Short`, ou `Integer`.</span><span class="sxs-lookup"><span data-stu-id="ab879-370">From `UInteger` to `Byte`, `SByte`, `UShort`, `Short`, or `Integer`.</span></span>

* <span data-ttu-id="ab879-371">Partir `Integer` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, ou `ULong`.</span><span class="sxs-lookup"><span data-stu-id="ab879-371">From `Integer` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="ab879-372">Partir `ULong` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, ou `Long`.</span><span class="sxs-lookup"><span data-stu-id="ab879-372">From `ULong` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `Long`.</span></span>

* <span data-ttu-id="ab879-373">Partir `Long` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, ou `ULong`.</span><span class="sxs-lookup"><span data-stu-id="ab879-373">From `Long` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `ULong`.</span></span>

* <span data-ttu-id="ab879-374">Partir `Decimal` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, ou `Long`.</span><span class="sxs-lookup"><span data-stu-id="ab879-374">From `Decimal` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, or `Long`.</span></span>

* <span data-ttu-id="ab879-375">Partir `Single` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, ou `Decimal`.</span><span class="sxs-lookup"><span data-stu-id="ab879-375">From `Single` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal`.</span></span>

* <span data-ttu-id="ab879-376">Partir `Double` à `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, ou `Single`.</span><span class="sxs-lookup"><span data-stu-id="ab879-376">From `Double` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, or `Single`.</span></span>

* <span data-ttu-id="ab879-377">De um tipo numérico em um tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="ab879-377">From a numeric type to an enumerated type.</span></span>

* <span data-ttu-id="ab879-378">Em um tipo enumerado para um tipo numérico seu tipo numérico subjacente tem uma conversão de estreitamento.</span><span class="sxs-lookup"><span data-stu-id="ab879-378">From an enumerated type to a numeric type its underlying numeric type has a narrowing conversion to.</span></span>

* <span data-ttu-id="ab879-379">Em um tipo enumerado em outro tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="ab879-379">From an enumerated type to another enumerated type.</span></span>

<span data-ttu-id="ab879-380">__Conversões de referência__</span><span class="sxs-lookup"><span data-stu-id="ab879-380">__Reference conversions__</span></span>

* <span data-ttu-id="ab879-381">De um tipo de referência para um tipo mais derivado.</span><span class="sxs-lookup"><span data-stu-id="ab879-381">From a reference type to a more derived type.</span></span>

* <span data-ttu-id="ab879-382">De um tipo de classe em um tipo de interface, fornecido que o tipo de classe não implementa o tipo de interface ou uma variante do tipo de interface compatível com ele.</span><span class="sxs-lookup"><span data-stu-id="ab879-382">From a class type to an interface type, provided the class type does not implement the interface type or an interface type variant compatible with it.</span></span>

* <span data-ttu-id="ab879-383">De um tipo de interface para um tipo de classe.</span><span class="sxs-lookup"><span data-stu-id="ab879-383">From an interface type to a class type.</span></span>

* <span data-ttu-id="ab879-384">De uma interface tipo para outro tipo de interface, desde que exista há relação de herança entre os dois tipos e fornecidas não são compatíveis com variant.</span><span class="sxs-lookup"><span data-stu-id="ab879-384">From an interface type to another interface type, provided there is no inheritance relationship between the two types and provided they are not variant compatible.</span></span>

<span data-ttu-id="ab879-385">__Conversões de delegado anônimas__</span><span class="sxs-lookup"><span data-stu-id="ab879-385">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="ab879-386">De um tipo de delegado anônimo gerado para uma reclassificação de método lambda para qualquer tipo de delegado mais estreita.</span><span class="sxs-lookup"><span data-stu-id="ab879-386">From an anonymous delegate type generated for a lambda method reclassification to any narrower delegate type.</span></span>

<span data-ttu-id="ab879-387">__Conversões de matriz__</span><span class="sxs-lookup"><span data-stu-id="ab879-387">__Array conversions__</span></span>

* <span data-ttu-id="ab879-388">De um tipo de matriz `S` com um tipo de elemento `Se`, para um tipo de matriz `T` com um tipo de elemento `Te`, desde que todos os itens a seguir forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="ab879-388">From an array type `S` with an element type `Se`, to an array type `T` with an element type `Te`, provided that all of the following are true:</span></span>

  * <span data-ttu-id="ab879-389">`S` e `T` diferem apenas no tipo de elemento.</span><span class="sxs-lookup"><span data-stu-id="ab879-389">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="ab879-390">Ambos `Se` e `Te` são tipos de referência ou parâmetros de tipo não são conhecidos para tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-390">Both `Se` and `Te` are reference types or are type parameters not known to be value types.</span></span>
  * <span data-ttu-id="ab879-391">Existe uma restrição de referência, matriz ou conversão de tipo de parâmetro a partir `Se` para `Te`.</span><span class="sxs-lookup"><span data-stu-id="ab879-391">A narrowing reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="ab879-392">De um tipo de matriz `S` com um tipo de elemento `Se` para um tipo de matriz `T` com um tipo de elemento enumerado `Te`, desde que todos os itens a seguir forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="ab879-392">From an array type `S` with an element type `Se` to an array type `T` with an enumerated element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="ab879-393">`S` e `T` diferem apenas no tipo de elemento.</span><span class="sxs-lookup"><span data-stu-id="ab879-393">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="ab879-394">`Se` é o tipo subjacente de `Te` , ou quando eles são ambos os tipos enumerados diferentes que compartilham o mesmo tipo subjacente.</span><span class="sxs-lookup"><span data-stu-id="ab879-394">`Se` is the underlying type of `Te` , or they are both different enumerated types that share the same underlying type.</span></span>

* <span data-ttu-id="ab879-395">De um tipo de matriz `S` de classificação 1, com um tipo de elemento enumerado `Se`, para `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` e `IEnumerable(Of Te)`, desde que uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="ab879-395">From an array type `S` of rank 1 with an enumerated element type `Se`, to `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="ab879-396">Ambos `Se` e `Te` são tipos de referência ou parâmetros de tipo que são conhecidos como um tipo de referência e existe uma restrição de referência, matriz ou conversão de tipo de parâmetro a partir `Se` para `Te`; ou</span><span class="sxs-lookup"><span data-stu-id="ab879-396">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a narrowing reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>
  * <span data-ttu-id="ab879-397">`Se` é o tipo subjacente de `Te`, ou quando eles são ambos os tipos enumerados diferentes que compartilham o mesmo tipo subjacente.</span><span class="sxs-lookup"><span data-stu-id="ab879-397">`Se` is the underlying type of `Te`, or they are both different enumerated types that share the same underlying type.</span></span>

<span data-ttu-id="ab879-398">__Conversões de tipo de valor__</span><span class="sxs-lookup"><span data-stu-id="ab879-398">__Value type conversions__</span></span>

* <span data-ttu-id="ab879-399">De um tipo de referência para um tipo mais derivado do valor.</span><span class="sxs-lookup"><span data-stu-id="ab879-399">From a reference type to a more derived value type.</span></span>

* <span data-ttu-id="ab879-400">De um tipo de interface para um tipo de valor, fornecido que o tipo de valor implementa o tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="ab879-400">From an interface type to a value type, provided the value type implements the interface type.</span></span>

<span data-ttu-id="ab879-401">__Conversões de tipo de valor anuláveis__</span><span class="sxs-lookup"><span data-stu-id="ab879-401">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="ab879-402">De um tipo `T?` em um tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="ab879-402">From a type `T?` to a type `T`.</span></span>

* <span data-ttu-id="ab879-403">De um tipo `T?` em um tipo `S?`, em que há uma conversão de restrição do tipo `T` para o tipo `S`.</span><span class="sxs-lookup"><span data-stu-id="ab879-403">From a type `T?` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="ab879-404">De um tipo `T` em um tipo `S?`, em que há uma conversão de restrição do tipo `T` para o tipo `S`.</span><span class="sxs-lookup"><span data-stu-id="ab879-404">From a type `T` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="ab879-405">De um tipo `S?` em um tipo `T`, em que há uma conversão do tipo `S` para o tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="ab879-405">From a type `S?` to a type `T`, where there is a conversion from the type `S` to the type `T`.</span></span>

<span data-ttu-id="ab879-406">__Conversões de cadeia de caracteres__</span><span class="sxs-lookup"><span data-stu-id="ab879-406">__String conversions__</span></span>

* <span data-ttu-id="ab879-407">Partir `String` para `Char`.</span><span class="sxs-lookup"><span data-stu-id="ab879-407">From `String` to `Char`.</span></span>

* <span data-ttu-id="ab879-408">Partir `String` para `Char()`.</span><span class="sxs-lookup"><span data-stu-id="ab879-408">From `String` to `Char()`.</span></span>

* <span data-ttu-id="ab879-409">Partir `String` para `Boolean` bidirecionalmente `Boolean` para `String`.</span><span class="sxs-lookup"><span data-stu-id="ab879-409">From `String` to `Boolean` and from `Boolean` to `String`.</span></span>

* <span data-ttu-id="ab879-410">Conversões entre `String` e `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, ou `Double`.</span><span class="sxs-lookup"><span data-stu-id="ab879-410">Conversions between `String` and `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ab879-411">Partir `String` para `Date` bidirecionalmente `Date` para `String`.</span><span class="sxs-lookup"><span data-stu-id="ab879-411">From `String` to `Date` and from `Date` to `String`.</span></span>

<span data-ttu-id="ab879-412">__Conversões de tipo de parâmetro__</span><span class="sxs-lookup"><span data-stu-id="ab879-412">__Type Parameter conversions__</span></span>

* <span data-ttu-id="ab879-413">De `Object` para um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="ab879-413">From `Object` to a type parameter.</span></span>

* <span data-ttu-id="ab879-414">De um tipo de parâmetro para um tipo de interface, fornecido o parâmetro de tipo não é restrito a interface ou restrito a uma classe que implementa essa interface.</span><span class="sxs-lookup"><span data-stu-id="ab879-414">From a type parameter to an interface type, provided the type parameter is not constrained to that interface or constrained to a class that implements that interface.</span></span>

* <span data-ttu-id="ab879-415">De um tipo de interface para um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="ab879-415">From an interface type to a type parameter.</span></span>

* <span data-ttu-id="ab879-416">De um parâmetro de tipo para um tipo derivado de uma restrição de classe.</span><span class="sxs-lookup"><span data-stu-id="ab879-416">From a type parameter to a derived type of a class constraint.</span></span>

* <span data-ttu-id="ab879-417">De um parâmetro de tipo `T` a qualquer coisa como uma restrição de parâmetro de tipo `Tx` tem uma conversão de estreitamento.</span><span class="sxs-lookup"><span data-stu-id="ab879-417">From a type parameter `T` to anything a type parameter constraint `Tx` has a narrowing conversion to.</span></span>

## <a name="type-parameter-conversions"></a><span data-ttu-id="ab879-418">Conversões de tipo de parâmetro</span><span class="sxs-lookup"><span data-stu-id="ab879-418">Type Parameter Conversions</span></span>

<span data-ttu-id="ab879-419">Conversões de parâmetros de tipo são determinadas pelas restrições, se houver, colocar neles.</span><span class="sxs-lookup"><span data-stu-id="ab879-419">Type parameters' conversions are determined by the constraints, if any, put on them.</span></span> <span data-ttu-id="ab879-420">Um parâmetro de tipo `T` sempre pode ser convertido em si só, para e de `Object`e de e para qualquer tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="ab879-420">A type parameter `T` can always be converted to itself, to and from `Object`, and to and from any interface type.</span></span> <span data-ttu-id="ab879-421">Observe que, se o tipo `T` é um tipo de valor em tempo de execução, convertendo de `T` à `Object` ou um tipo de interface será uma conversão boxing e convertendo de `Object` ou uma interface de tipo para `T` será uma conversão unboxing conversão.</span><span class="sxs-lookup"><span data-stu-id="ab879-421">Note that if the type `T` is a value type at run-time, converting from `T` to `Object` or an interface type will be a boxing conversion and converting from `Object` or an interface type to `T` will be an unboxing conversion.</span></span> <span data-ttu-id="ab879-422">Um parâmetro de tipo com uma restrição de classe `C` define conversões adicionais do parâmetro de tipo para `C` e suas classes base e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="ab879-422">A type parameter with a class constraint `C` defines additional conversions from the type parameter to `C` and its base classes, and vice versa.</span></span> <span data-ttu-id="ab879-423">Um parâmetro de tipo `T` com uma restrição de parâmetro de tipo `Tx` define uma conversão `Tx` e qualquer coisa `Tx` converte.</span><span class="sxs-lookup"><span data-stu-id="ab879-423">A type parameter `T` with a type parameter constraint `Tx` defines a conversion to `Tx` and anything `Tx` converts to.</span></span>

<span data-ttu-id="ab879-424">Uma matriz cujo tipo de elemento é um parâmetro de tipo com uma restrição de interface `I` tem as mesmas conversões de matriz covariante como uma matriz cujo tipo de elemento é `I`, desde que o parâmetro de tipo também tem um `Class` ou (restrição de classe como apenas referência tipos de elemento de matriz podem ser covariantes).</span><span class="sxs-lookup"><span data-stu-id="ab879-424">An array whose element type is a type parameter with an interface constraint `I` has the same covariant array conversions as an array whose element type is `I`, provided that the type parameter also has a `Class` or class constraint (since only reference array element types can be covariant).</span></span> <span data-ttu-id="ab879-425">Uma matriz cujo tipo de elemento é um parâmetro de tipo com uma restrição de classe `C` tem as mesmas conversões de matriz covariante como uma matriz cujo tipo de elemento é `C`.</span><span class="sxs-lookup"><span data-stu-id="ab879-425">An array whose element type is a type parameter with a class constraint `C` has the same covariant array conversions as an array whose element type is `C`.</span></span>

<span data-ttu-id="ab879-426">As regras de conversões acima não permitem conversões de parâmetros de tipo sem restrição para tipos sem interface, que pode ser surpreendente.</span><span class="sxs-lookup"><span data-stu-id="ab879-426">The above conversions rules do not permit conversions from unconstrained type parameters to non-interface types, which may be surprising.</span></span> <span data-ttu-id="ab879-427">A razão para isso é evitar confusão sobre a semântica de tais conversões.</span><span class="sxs-lookup"><span data-stu-id="ab879-427">The reason for this is to prevent confusion about the semantics of such conversions.</span></span> <span data-ttu-id="ab879-428">Por exemplo, considere a seguinte declaração:</span><span class="sxs-lookup"><span data-stu-id="ab879-428">For example, consider the following declaration:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

<span data-ttu-id="ab879-429">Se a conversão de `T` à `Integer` eram permitidas, seria possível com facilidade que `X(Of Integer).F(7)` retornaria `7L`.</span><span class="sxs-lookup"><span data-stu-id="ab879-429">If the conversion of `T` to `Integer` were permitted, one might easily expect that `X(Of Integer).F(7)` would return `7L`.</span></span> <span data-ttu-id="ab879-430">No entanto, não, seria como conversões numéricas são consideradas somente quando os tipos são conhecidos como numéricos em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab879-430">However, it would not, because numeric conversions are only considered when the types are known to be numeric at compile time.</span></span> <span data-ttu-id="ab879-431">Para fazer a semântica clear, o exemplo acima deve ser escrito:</span><span class="sxs-lookup"><span data-stu-id="ab879-431">In order to make the semantics clear, the above example must instead be written:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a><span data-ttu-id="ab879-432">Conversões definidas pelo usuário</span><span class="sxs-lookup"><span data-stu-id="ab879-432">User-Defined Conversions</span></span>

<span data-ttu-id="ab879-433">*Conversões intrínseco* conversões definidas pelo idioma (ou seja, listado nesta especificação), enquanto *conversões definidas pelo usuário* são definidos pela sobrecarga de `CType` operador.</span><span class="sxs-lookup"><span data-stu-id="ab879-433">*Intrinsic conversions* are conversions defined by the language (i.e. listed in this specification), while *user-defined conversions* are defined by overloading the `CType` operator.</span></span> <span data-ttu-id="ab879-434">Ao converter entre tipos, se nenhuma conversão intrínseco forem aplicável serão consideradas conversões definidas pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="ab879-434">When converting between types, if no intrinsic conversions are applicable then user-defined conversions will be considered.</span></span> <span data-ttu-id="ab879-435">Se não houver uma conversão definida pelo usuário que está *mais específica* para os tipos de origem e destino, em seguida, a conversão definida pelo usuário será usada.</span><span class="sxs-lookup"><span data-stu-id="ab879-435">If there is a user-defined conversion that is *most specific* for the source and target types, then the user-defined conversion will be used.</span></span> <span data-ttu-id="ab879-436">Caso contrário, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="ab879-436">Otherwise, a compile-time error results.</span></span> <span data-ttu-id="ab879-437">A conversão mais específica é aquele cujo operando é "mais próximo" para o tipo de fonte e cujo tipo de resultado é "mais próximo" para o tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab879-437">The most specific conversion is the one whose operand is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="ab879-438">Ao determinar quais conversão definida pelo usuário para usar, o conversão de ampliação mais específico será usado; Se nenhuma ampliação conversão é o mais específica, que a conversão de estreitamento mais específica será usada.</span><span class="sxs-lookup"><span data-stu-id="ab879-438">When determining what user-defined conversion to use, the most specific widening conversion will be used; if no widening conversion is most specific, the most specific narrowing conversion will be used.</span></span> <span data-ttu-id="ab879-439">Se houver não mais específica conversão de estreitamento, em seguida, a conversão é indefinida e ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ab879-439">If there is no most specific narrowing conversion, then the conversion is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="ab879-440">As seções a seguir abordam como as conversões mais específicas são determinadas.</span><span class="sxs-lookup"><span data-stu-id="ab879-440">The following sections cover how the most specific conversions are determined.</span></span> <span data-ttu-id="ab879-441">Eles usam os seguintes termos:</span><span class="sxs-lookup"><span data-stu-id="ab879-441">They use the following terms:</span></span>

<span data-ttu-id="ab879-442">Se uma ampliação intrínseca existe conversão de um tipo `A` para um tipo `B`e caso nem `A` nem `B` são interfaces, em seguida, `A` é *englobados* pelo `B`, e `B` *abrange* `A`.</span><span class="sxs-lookup"><span data-stu-id="ab879-442">If an intrinsic widening conversion exists from a type `A` to a type `B`, and if neither `A` nor `B` are interfaces, then `A` is *encompassed* by `B`, and `B` *encompasses* `A`.</span></span>

<span data-ttu-id="ab879-443">O *mais abrangente* tipo em um conjunto de tipos é o tipo de uma que abrange todos os outros tipos no conjunto.</span><span class="sxs-lookup"><span data-stu-id="ab879-443">The *most encompassing* type in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="ab879-444">Se nenhum tipo abrange todos os outros tipos, o conjunto não tem nenhum tipo mais abrangente.</span><span class="sxs-lookup"><span data-stu-id="ab879-444">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="ab879-445">Em termos de intuitivos, o tipo mais abrangente é o tipo "maior" em set--o um tipo ao qual cada um dos outros tipos pode ser convertida por meio de uma conversão de ampliação.</span><span class="sxs-lookup"><span data-stu-id="ab879-445">In intuitive terms, the most encompassing type is the "largest" type in the set -- the one type to which each of the other types can be converted through a widening conversion.</span></span>

<span data-ttu-id="ab879-446">O *englobados mais* tipo em um conjunto de tipos é o tipo de um determinado contido por todos os outros tipos no conjunto.</span><span class="sxs-lookup"><span data-stu-id="ab879-446">The *most encompassed* type in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="ab879-447">Se nenhum tipo é abrangido pela todos os outros tipos, o conjunto tem não mais englobados tipo.</span><span class="sxs-lookup"><span data-stu-id="ab879-447">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="ab879-448">Em termos de intuitivos, o tipo mais contido é o tipo "menor" em set--o um tipo que pode ser convertido em cada um dos outros tipos por meio de uma conversão de estreitamento.</span><span class="sxs-lookup"><span data-stu-id="ab879-448">In intuitive terms, the most encompassed type is the "smallest" type in the set -- the one type that can be converted to each of the other types through a narrowing conversion.</span></span>

<span data-ttu-id="ab879-449">Ao coletar as conversões definidas pelo usuário do candidato para um tipo de `T?`, os operadores de conversão definida pelo usuário definidos pela `T` são usados em vez disso.</span><span class="sxs-lookup"><span data-stu-id="ab879-449">When collecting the candidate user-defined conversions for a type `T?`, the user-defined conversion operators defined by `T` are used instead.</span></span> <span data-ttu-id="ab879-450">Se o tipo que está sendo convertido em também é um tipo de valor anulável, em seguida, qualquer uma das `T`do definidas pelo usuário são elevadas a operadores de conversões que envolvem apenas os tipos de valor não anulável.</span><span class="sxs-lookup"><span data-stu-id="ab879-450">If the type being converted to is also a nullable value type, then any of `T`'s user-defined conversions operators that involve only non-nullable value types are lifted.</span></span> <span data-ttu-id="ab879-451">Um operador de conversão de `T` à `S` é elevado para ser uma conversão de `T?` ao `S?` e é avaliada, convertendo `T?` para `T`, se necessário, em seguida, avaliar a conversão definida pelo usuário operador de `T` à `S` e, em seguida, convertendo `S` para `S?`, se necessário.</span><span class="sxs-lookup"><span data-stu-id="ab879-451">A conversion operator from `T` to `S` is lifted to be a conversion from `T?` to `S?` and is evaluated by converting `T?` to `T`, if necessary, then evaluating the user-defined conversion operator from `T` to `S` and then converting `S` to `S?`, if necessary.</span></span> <span data-ttu-id="ab879-452">Se o valor que está sendo convertido estiver `Nothing`, no entanto, um operador de conversão elevadas se converte diretamente em um valor de `Nothing` digitado como `S?`.</span><span class="sxs-lookup"><span data-stu-id="ab879-452">If the value being converted is `Nothing`, however, a lifted conversion operator converts directly into a value of `Nothing` typed as `S?`.</span></span> <span data-ttu-id="ab879-453">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab879-453">For example:</span></span>

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Legal: y is still null
        x = New T()
        y = x                ' Legal: Converts from T to S
    End Sub
End Module
```

<span data-ttu-id="ab879-454">Quando a resolução de conversões, definido pelo usuário operadores de conversões sempre têm preferência sobre os operadores de conversão cancelada.</span><span class="sxs-lookup"><span data-stu-id="ab879-454">When resolving conversions, user-defined conversions operators are always preferred over lifted conversion operators.</span></span> <span data-ttu-id="ab879-455">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab879-455">For example:</span></span>

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator

    Public Shared Widening Operator CType(ByVal v As T?) As S?
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Calls user-defined conversion, not lifted conversion
    End Sub
End Module
```

<span data-ttu-id="ab879-456">Em tempo de execução, a avaliação de uma conversão definida pelo usuário pode envolver até três etapas:</span><span class="sxs-lookup"><span data-stu-id="ab879-456">At run-time, evaluating a user-defined conversion can involve up to three steps:</span></span>

1. <span data-ttu-id="ab879-457">Primeiro, o valor é convertido do tipo de fonte para o tipo de operando usando uma conversão intrínseco, se necessário.</span><span class="sxs-lookup"><span data-stu-id="ab879-457">First, the value is converted from the source type to the operand type using an intrinsic conversion, if necessary.</span></span>

2. <span data-ttu-id="ab879-458">Em seguida, a conversão definida pelo usuário é invocada.</span><span class="sxs-lookup"><span data-stu-id="ab879-458">Then, the user-defined conversion is invoked.</span></span>

3. <span data-ttu-id="ab879-459">Por fim, o resultado da conversão definida pelo usuário é convertido para o tipo de destino usando uma conversão intrínseco, se necessário.</span><span class="sxs-lookup"><span data-stu-id="ab879-459">Finally, the result of the user-defined conversion is converted to the target type using an intrinsic conversion, if necessary.</span></span>

<span data-ttu-id="ab879-460">É importante observar que a avaliação de uma conversão definida pelo usuário nunca envolverá mais de um operador de conversão definida pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="ab879-460">It is important to note that evaluation of a user-defined conversion will never involve more than one user-defined conversion operator.</span></span>

### <a name="most-specific-widening-conversion"></a><span data-ttu-id="ab879-461">Mais específico de conversão de ampliação</span><span class="sxs-lookup"><span data-stu-id="ab879-461">Most Specific Widening Conversion</span></span>

<span data-ttu-id="ab879-462">Determinando o mais específica definida pelo usuário ampliação operador de conversão entre dois tipos é realizado usando as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="ab879-462">Determining the most specific user-defined widening conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="ab879-463">Primeiro, todos os operadores de conversão do candidato são coletados.</span><span class="sxs-lookup"><span data-stu-id="ab879-463">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="ab879-464">Os operadores de conversão do candidato são todos os operadores definidos pelo usuário de conversão a ampliação no tipo de origem e todos os operadores definidos pelo usuário de conversão a ampliação no tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab879-464">The candidate conversion operators are all of the user-defined widening conversion operators in the source type and all of the user-defined widening conversion operators in the target type.</span></span>

2. <span data-ttu-id="ab879-465">Em seguida, todos os operadores de conversão não aplicáveis são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ab879-465">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="ab879-466">Um operador de conversão é aplicável a um tipo de origem e o tipo de destino se há um operador intrínseco de conversão de ampliação, a do tipo de fonte, para o tipo de operando, e há um operador intrínseco de conversão de ampliação, a do resultado do operador, para o tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab879-466">A conversion operator is applicable to a source type and target type if there is an intrinsic widening conversion operator from the source type to the operand type and there is an intrinsic widening conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="ab879-467">Se não houver nenhum operador de conversão aplicável, em seguida, há não mais específica conversão de ampliação.</span><span class="sxs-lookup"><span data-stu-id="ab879-467">If there are no applicable conversion operators, then there is no most specific widening conversion.</span></span>

3. <span data-ttu-id="ab879-468">Em seguida, o tipo mais específico do código-fonte dos operadores de conversão aplicável é determinado:</span><span class="sxs-lookup"><span data-stu-id="ab879-468">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="ab879-469">Se qualquer um dos operadores de conversão converter diretamente do tipo de fonte, o tipo de origem é o tipo mais específico do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="ab879-469">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="ab879-470">Caso contrário, o tipo mais específico do código-fonte é o tipo mais contido no conjunto combinado de tipos de fonte dos operadores de conversão.</span><span class="sxs-lookup"><span data-stu-id="ab879-470">Otherwise, the most specific source type is the most encompassed type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="ab879-471">Se não mais englobados tipo pode ser encontrado, em seguida, há não mais específica conversão de ampliação.</span><span class="sxs-lookup"><span data-stu-id="ab879-471">If no most encompassed type can be found, then there is no most specific widening conversion.</span></span>

4. <span data-ttu-id="ab879-472">Em seguida, o tipo de destino mais específico dos operadores de conversão aplicável é determinado:</span><span class="sxs-lookup"><span data-stu-id="ab879-472">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="ab879-473">Se qualquer um dos operadores de conversão converte diretamente para o tipo de destino, o tipo de destino é o tipo de destino mais específico.</span><span class="sxs-lookup"><span data-stu-id="ab879-473">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="ab879-474">Caso contrário, o tipo mais específico de destino é o tipo mais abrangente no conjunto combinado de tipos de destino dos operadores de conversão.</span><span class="sxs-lookup"><span data-stu-id="ab879-474">Otherwise, the most specific target type is the most encompassing type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="ab879-475">Se nenhum tipo mais abrangente pode ser encontrado, em seguida, há não mais específico conversão de ampliação.</span><span class="sxs-lookup"><span data-stu-id="ab879-475">If no most encompassing type can be found, then there is no most specific widening conversion.</span></span>

5. <span data-ttu-id="ab879-476">Em seguida, se o operador de conversão de exatamente um converte do tipo de fonte mais específico para o tipo de destino mais específico, este é o operador de conversão mais específico.</span><span class="sxs-lookup"><span data-stu-id="ab879-476">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="ab879-477">Não se houver mais de um operador desse tipo, há nenhuma conversão de ampliação mais específico.</span><span class="sxs-lookup"><span data-stu-id="ab879-477">If more than one such operator exists, then there is no most specific widening conversion.</span></span>

### <a name="most-specific-narrowing-conversion"></a><span data-ttu-id="ab879-478">Conversão de estreitamento mais específico</span><span class="sxs-lookup"><span data-stu-id="ab879-478">Most Specific Narrowing Conversion</span></span>

<span data-ttu-id="ab879-479">Determinando o mais específica definida pelo usuário estreitamento operador de conversão entre dois tipos é realizado usando as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="ab879-479">Determining the most specific user-defined narrowing conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="ab879-480">Primeiro, todos os operadores de conversão do candidato são coletados.</span><span class="sxs-lookup"><span data-stu-id="ab879-480">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="ab879-481">Os operadores de conversão do candidato são todos os operadores de conversão definida pelo usuário no tipo de origem e todos os operadores de conversão definida pelo usuário no tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab879-481">The candidate conversion operators are all of the user-defined conversion operators in the source type and all of the user-defined conversion operators in the target type.</span></span>

2. <span data-ttu-id="ab879-482">Em seguida, todos os operadores de conversão não aplicáveis são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ab879-482">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="ab879-483">Um operador de conversão é aplicável a um tipo de origem e o tipo de destino se há um operador de conversão intrínseco do tipo de fonte para o tipo de operando, e há um operador de conversão intrínseco do resultado do operador para o tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ab879-483">A conversion operator is applicable to a source type and target type if there is an intrinsic conversion operator from the source type to the operand type and there is an intrinsic conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="ab879-484">Se não houver nenhum operador de conversão aplicável, há nenhuma conversão de estreitamento mais específica.</span><span class="sxs-lookup"><span data-stu-id="ab879-484">If there are no applicable conversion operators, then there is no most specific narrowing conversion.</span></span>

3. <span data-ttu-id="ab879-485">Em seguida, o tipo mais específico do código-fonte dos operadores de conversão aplicável é determinado:</span><span class="sxs-lookup"><span data-stu-id="ab879-485">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="ab879-486">Se qualquer um dos operadores de conversão converter diretamente do tipo de fonte, o tipo de origem é o tipo mais específico do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="ab879-486">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="ab879-487">Caso contrário, se qualquer um dos operadores de conversão converter de tipos que abranjam o tipo de origem, o tipo mais específico do código-fonte é o tipo mais contido no conjunto combinado de tipos de fonte desses operadores de conversão.</span><span class="sxs-lookup"><span data-stu-id="ab879-487">Otherwise, if any of the conversion operators convert from types that encompass the source type, then the most specific source type is the most encompassed type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="ab879-488">Se não mais englobados tipo pode ser encontrado, em seguida, não há nenhuma conversão de estreitamento mais específica.</span><span class="sxs-lookup"><span data-stu-id="ab879-488">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="ab879-489">Caso contrário, o tipo mais específico do código-fonte é o tipo mais abrangente no conjunto combinado de tipos de fonte dos operadores de conversão.</span><span class="sxs-lookup"><span data-stu-id="ab879-489">Otherwise, the most specific source type is the most encompassing type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="ab879-490">Se nenhum tipo mais abrangente pode ser encontrado, há nenhuma conversão de estreitamento mais específica.</span><span class="sxs-lookup"><span data-stu-id="ab879-490">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

4. <span data-ttu-id="ab879-491">Em seguida, o tipo de destino mais específico dos operadores de conversão aplicável é determinado:</span><span class="sxs-lookup"><span data-stu-id="ab879-491">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="ab879-492">Se qualquer um dos operadores de conversão converte diretamente para o tipo de destino, o tipo de destino é o tipo de destino mais específico.</span><span class="sxs-lookup"><span data-stu-id="ab879-492">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="ab879-493">Caso contrário, se qualquer um dos operadores de conversão converter em tipos que são englobados pelo tipo de destino, em seguida, o tipo mais específico de destino é o tipo mais abrangente no conjunto combinado de tipos de fonte desses operadores de conversão.</span><span class="sxs-lookup"><span data-stu-id="ab879-493">Otherwise, if any of the conversion operators convert to types that are encompassed by the target type, then the most specific target type is the most encompassing type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="ab879-494">Se nenhum tipo mais abrangente pode ser encontrado, há nenhuma conversão de estreitamento mais específica.</span><span class="sxs-lookup"><span data-stu-id="ab879-494">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="ab879-495">Caso contrário, o tipo mais específico de destino é o tipo mais contido no conjunto combinado de tipos de destino dos operadores de conversão.</span><span class="sxs-lookup"><span data-stu-id="ab879-495">Otherwise, the most specific target type is the most encompassed type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="ab879-496">Se não mais englobados tipo pode ser encontrado, em seguida, não há nenhuma conversão de estreitamento mais específica.</span><span class="sxs-lookup"><span data-stu-id="ab879-496">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

5. <span data-ttu-id="ab879-497">Em seguida, se o operador de conversão de exatamente um converte do tipo de fonte mais específico para o tipo de destino mais específico, este é o operador de conversão mais específico.</span><span class="sxs-lookup"><span data-stu-id="ab879-497">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="ab879-498">Se houver mais de um operador desse tipo, há nenhuma conversão de estreitamento mais específica.</span><span class="sxs-lookup"><span data-stu-id="ab879-498">If more than one such operator exists, then there is no most specific narrowing conversion.</span></span>

## <a name="native-conversions"></a><span data-ttu-id="ab879-499">Conversões nativas</span><span class="sxs-lookup"><span data-stu-id="ab879-499">Native Conversions</span></span>

<span data-ttu-id="ab879-500">Várias das conversões são classificadas como *conversões nativas* porque eles são suportados nativamente pelo .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ab879-500">Several of the conversions are classified as *native conversions* because they are supported natively by the .NET Framework.</span></span> <span data-ttu-id="ab879-501">Essas conversões são aqueles que pode ser otimizada através do uso do `DirectCast` e `TryCast` operadores de conversão, bem como outros comportamentos especiais.</span><span class="sxs-lookup"><span data-stu-id="ab879-501">These conversions are ones that can be optimized through the use of the `DirectCast` and `TryCast` conversion operators, as well as other special behaviors.</span></span> <span data-ttu-id="ab879-502">As conversões classificadas como conversões nativas são: conversões de identidade, conversões padrão, conversões de referência, conversões de matriz, conversões de tipo de valor e conversões de tipo de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ab879-502">The conversions classified as native conversions are: identity conversions, default conversions, reference conversions, array conversions, value type conversions, and type parameter conversions.</span></span>

## <a name="dominant-type"></a><span data-ttu-id="ab879-503">Tipo dominante</span><span class="sxs-lookup"><span data-stu-id="ab879-503">Dominant Type</span></span>

<span data-ttu-id="ab879-504">Dado um conjunto de tipos, geralmente é necessário em situações como inferência de tipo para determinar a *tipo dominante* do conjunto.</span><span class="sxs-lookup"><span data-stu-id="ab879-504">Given a set of types, it is often necessary in situations such as type inference to determine the *dominant type* of the set.</span></span> <span data-ttu-id="ab879-505">O tipo dominante de um conjunto de tipos é determinado pelo primeiro remover quaisquer tipos que um ou mais tipos não têm uma conversão implícita em.</span><span class="sxs-lookup"><span data-stu-id="ab879-505">The dominant type of a set of types is determined by first removing any types that one or more other types do not have an implicit conversion to.</span></span> <span data-ttu-id="ab879-506">Se não houver nenhum tipo para a esquerda no momento, não há nenhum tipo dominante.</span><span class="sxs-lookup"><span data-stu-id="ab879-506">If there are no types left at this point, there is no dominant type.</span></span> <span data-ttu-id="ab879-507">O tipo dominante é, em seguida, a maioria englobados dos tipos restantes.</span><span class="sxs-lookup"><span data-stu-id="ab879-507">The dominant type is then the most encompassed of the remaining types.</span></span> <span data-ttu-id="ab879-508">Se não houver mais de um tipo que é mais englobados, em seguida, não há nenhum tipo dominante.</span><span class="sxs-lookup"><span data-stu-id="ab879-508">If there is more than one type that is most encompassed, then there is no dominant type.</span></span>
