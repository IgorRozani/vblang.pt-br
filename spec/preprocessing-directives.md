# <a name="preprocessing-directives"></a><span data-ttu-id="3dc85-101">Pré-processando diretivas</span><span class="sxs-lookup"><span data-stu-id="3dc85-101">Preprocessing Directives</span></span>

<span data-ttu-id="3dc85-102">Depois que um arquivo tenha sido analisado lexicalmente, ocorrem vários tipos de fonte de pré-processamento.</span><span class="sxs-lookup"><span data-stu-id="3dc85-102">Once a file has been lexically analyzed, several kinds of source preprocessing occur.</span></span> <span data-ttu-id="3dc85-103">A compilação condicional, mais importante, determina qual fonte é processada pela gramática sintática; outros dois tipos de diretivas, diretivas de fonte externa e diretivas de região-- fornecem meta-informações sobre a origem, mas não têm nenhum efeito na compilação.</span><span class="sxs-lookup"><span data-stu-id="3dc85-103">The most important, conditional compilation, determines which source is processed by the syntactic grammar; two other types of directives -- external source directives and region directives -- provide meta-information about the source but have no effect on compilation.</span></span>

## <a name="conditional-compilation"></a><span data-ttu-id="3dc85-104">Compilação condicional</span><span class="sxs-lookup"><span data-stu-id="3dc85-104">Conditional Compilation</span></span>

<span data-ttu-id="3dc85-105">Compilação condicional controla se as sequências de linhas lógicas são convertidas em código real.</span><span class="sxs-lookup"><span data-stu-id="3dc85-105">Conditional compilation controls whether sequences of logical lines are translated into actual code.</span></span> <span data-ttu-id="3dc85-106">No início da compilação condicional, todas as linhas lógicas estão habilitadas; No entanto, colocar as linhas em instruções de compilação condicional pode desabilitar seletivamente essas linhas dentro do arquivo, fazendo com que eles devem ser ignorados durante o restante do processo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3dc85-106">At the beginning of conditional compilation, all logical lines are enabled; however, enclosing lines in conditional compilation statements may selectively disable those lines within the file, causing them to be ignored during the rest of the compilation process.</span></span>

```antlr
CCStart
    : CCStatement*
    ;

CCStatement
    : CCConstantDeclaration
    | CCIfGroup
    | LogicalLine
    ;

CCExpression
    : LiteralExpression
    | CCParenthesizedExpression
    | CCSimpleNameExpression
    | CCCastExpression
    | CCOperatorExpression
    | CCConditionalExpression
    ;

CCParenthesizedExpression
    : '(' CCExpression ')'
    ;

CCSimpleNameExpression
    : Identifier
    ;

CCCastExpression
    : 'DirectCast' '(' CCExpression ',' TypeName ')'
    | 'TryCast' '(' CCExpression ',' TypeName ')'
    | 'CType' '(' CCExpression ',' TypeName ')'
    | CastTarget '(' CCExpression ')'
    ;

CCOperatorExpression
    : CCUnaryOperator CCExpression
    | CCExpression CCBinaryOperator CCExpression
    ;

CCUnaryOperator
    : '+' | '-' | 'Not'
    ;

CCBinaryOperator
    : '+'     | '-'     | '*'   | '/'       | '\\'     | 'Mod' | '^' | '='
    | '<' '>' | '<'     | '>'   | '<' '='   | '>' '='  | '&'
    | 'And'   | 'Or'    | 'Xor' | 'AndAlso' | 'OrElse'
    | '<' '<' | '>' '>'
    ;

CCConditionalExpression
    : 'If' '(' CCExpression ',' CCExpression ',' CCExpression ')'
    | 'If' '(' CCExpression ',' CCExpression ')'
    ;
```


<span data-ttu-id="3dc85-107">Por exemplo, o programa</span><span class="sxs-lookup"><span data-stu-id="3dc85-107">For example, the program</span></span>

```vb
#Const A = True
#Const B = False

Class C

#If A Then
    Sub F()
    End Sub
#Else
    Sub G()
    End Sub
#End If

#If B Then
    Sub H()
    End Sub
#Else
    Sub I()
    End Sub
#End If

End Class
```

<span data-ttu-id="3dc85-108">produz a mesma sequência exata dos tokens que o programa</span><span class="sxs-lookup"><span data-stu-id="3dc85-108">produces the exact same sequence of tokens as the program</span></span>

```vb
Class C
    Sub F()
    End Sub

    Sub I()
    End Sub
End Class
```

<span data-ttu-id="3dc85-109">As expressões de constante permitidas em diretivas de compilação condicional são um subconjunto de expressões de constante gerais.</span><span class="sxs-lookup"><span data-stu-id="3dc85-109">The constant expressions allowed in conditional compilation directives are a subset of general constant expressions.</span></span>

<span data-ttu-id="3dc85-110">O pré-processador permite espaço em branco e continuações de linha explícita antes e depois de cada token.</span><span class="sxs-lookup"><span data-stu-id="3dc85-110">The preprocessor allows whitespace and explicit line continuations before and after every token.</span></span>


### <a name="conditional-constant-directives"></a><span data-ttu-id="3dc85-111">Diretivas de constante condicionais</span><span class="sxs-lookup"><span data-stu-id="3dc85-111">Conditional Constant Directives</span></span>

<span data-ttu-id="3dc85-112">Instruções condicionais de constante definem constantes que existem em um espaço de declaração separada de compilação condicional no escopo do arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="3dc85-112">Conditional constant statements define constants that exist in a separate conditional compilation declaration space scoped to the source file.</span></span>

```antlr
CCConstantDeclaration
    : '#' 'Const' Identifier '=' CCExpression LineTerminator
    ;
```

<span data-ttu-id="3dc85-113">O espaço de declaração é especial em que nenhuma declaração explícita de constantes de compilação condicional é necessário – constantes condicionais podem ser definidas implicitamente em uma diretiva de compilação condicional.</span><span class="sxs-lookup"><span data-stu-id="3dc85-113">The declaration space is special in that no explicit declaration of conditional compilation constants is necessary -- conditional constants can be implicitly defined in a conditional compilation directive.</span></span>

<span data-ttu-id="3dc85-114">Antes que está sendo atribuído um valor, uma constante de compilação condicional tem o valor `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="3dc85-114">Prior to being assigned a value, a conditional compilation constant has the value `Nothing`.</span></span> <span data-ttu-id="3dc85-115">Quando uma constante de compilação condicional é atribuída um valor, que deve ser uma expressão constante, o tipo da constante torna-se o tipo do valor que está sendo atribuído a ele.</span><span class="sxs-lookup"><span data-stu-id="3dc85-115">When a conditional compilation constant is assigned a value, which must be a constant expression, the type of the constant becomes the type of the value being assigned to it.</span></span> <span data-ttu-id="3dc85-116">Uma constante de compilação condicional pode ser redefinida várias vezes ao longo de um arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="3dc85-116">A conditional compilation constant may be redefined multiple times throughout a source file.</span></span>

<span data-ttu-id="3dc85-117">Por exemplo, o código a seguir imprime somente a cadeia de caracteres `about to print value` e o valor de `Test`.</span><span class="sxs-lookup"><span data-stu-id="3dc85-117">For example, the following code prints only the string `about to print value` and the value of `Test`.</span></span>

```vb
Module M1
    Sub PrintValue(Test As Integer)

#Const DebugCode = True

#If DebugCode Then
        Console.WriteLine("about to print value")
#End If

#Const DebugCode = False

        Console.WriteLine(Test)

#If DebugCode Then
        Console.WriteLine("printed value")
#End If

    End Sub
End Module
```

<span data-ttu-id="3dc85-118">O ambiente de compilação também pode definir constantes condicionais em um espaço de declaração de compilação condicional.</span><span class="sxs-lookup"><span data-stu-id="3dc85-118">The compilation environment may also define conditional constants in a conditional compilation declaration space.</span></span>


### <a name="conditional-compilation-directives"></a><span data-ttu-id="3dc85-119">Diretivas de compilação condicional</span><span class="sxs-lookup"><span data-stu-id="3dc85-119">Conditional Compilation Directives</span></span>

<span data-ttu-id="3dc85-120">Diretivas de compilação condicional controlam a compilação condicional.</span><span class="sxs-lookup"><span data-stu-id="3dc85-120">Conditional compilation directives control conditional compilation.</span></span>

```antlr
CCIfGroup
    : '#' 'If' CCExpression 'Then'? LineTerminator CCStatement*
    CCElseIfGroup* CCElseGroup? '#' 'End' 'If' LineTerminator
    ;

CCElseIfGroup
    : '#' ElseIf CCExpression 'Then'? LineTerminator CCStatement*
    ;

CCElseGroup
    : '#' 'Else' LineTerminator CCStatement*
    ;
```

<span data-ttu-id="3dc85-121">Constantes condicional só podem referenciar expressões constantes e constantes de compilação condicional.</span><span class="sxs-lookup"><span data-stu-id="3dc85-121">Conditional constants can only reference constant expressions and conditional compilation constants.</span></span> <span data-ttu-id="3dc85-122">Cada uma das expressões constantes dentro de um grupo único de compilação condicional é avaliada e convertida para o `Boolean` tipo na ordem textual do primeiro ao último até que um da condicional expressões for avaliada como `True`.</span><span class="sxs-lookup"><span data-stu-id="3dc85-122">Each of the constant expressions within a single conditional compilation group is evaluated and converted to the `Boolean` type in textual order from first to last until one of the conditional expressions evaluates to `True`.</span></span> <span data-ttu-id="3dc85-123">Se uma expressão não é conversível para `Boolean`, resultados de um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3dc85-123">If an expression is not convertible to `Boolean`, a compile-time error results.</span></span> <span data-ttu-id="3dc85-124">Semântica permissiva e comparações de cadeia de caracteres binária são sempre usadas ao avaliar expressões de constantes de compilação condicional, independentemente de qualquer `Option` diretivas ou configurações de ambiente de compilação.</span><span class="sxs-lookup"><span data-stu-id="3dc85-124">Permissive semantics and binary string comparisons are always used when evaluating conditional compilation constant expressions, regardless of any `Option` directives or compilation environment settings.</span></span>

<span data-ttu-id="3dc85-125">Todas as linhas entre o grupo, inclusive diretivas de compilação condicional aninhada, estão desabilitadas, exceto para as linhas entre a instrução que contém o `True` expressão e a próxima instrução condicional do grupo, ou as linhas entre os `Else`instrução e o `End If` instrução se um `Else` aparece no grupo e todas as expressões são avaliadas como `False`.</span><span class="sxs-lookup"><span data-stu-id="3dc85-125">All lines enclosed by the group, including nested conditional compilation directives, are disabled except for lines between the statement containing the `True` expression and the next conditional statement of the group, or lines between the `Else` statement and the `End If` statement if an `Else` appears in the group and all of the expressions evaluate to `False`.</span></span>

<span data-ttu-id="3dc85-126">Neste exemplo, a chamada para `WriteToLog` no `Trace` diretiva de compilação condicional não será processada porque ao redor `Debug` diretiva de compilação condicional é avaliada como `False`.</span><span class="sxs-lookup"><span data-stu-id="3dc85-126">In this example, the call to `WriteToLog` in the `Trace` conditional compilation directive is not processed because the surrounding `Debug` conditional compilation directive evaluates to `False`.</span></span>

```vb
#Const Debug = False   ' Debugging off
#Const Trace = True    ' Tracing on

Class PurchaseTransaction
    Sub Commit()

#If Debug Then
        CheckConsistency()
#If Trace Then
        WriteToLog(Me.ToString())
#End If
#End If
        ...
    End Sub
End Class
```


## <a name="external-source-directives"></a><span data-ttu-id="3dc85-127">Diretivas de fonte externa</span><span class="sxs-lookup"><span data-stu-id="3dc85-127">External Source Directives</span></span>

<span data-ttu-id="3dc85-128">Um arquivo de origem pode incluir diretivas de fonte externa que indicam um mapeamento entre as linhas de origem e o texto externo à origem.</span><span class="sxs-lookup"><span data-stu-id="3dc85-128">A source file may include external source directives that indicate a mapping between source lines and text external to the source.</span></span>

```antlr
ESDStart
    : ExternalSourceStatement*
    ;

ExternalSourceStatement
    : ExternalSourceGroup
    | LogicalLine
    ;

ExternalSourceGroup
    : '#' 'ExternalSource' '(' StringLiteral ',' IntLiteral ')' LineTerminator
      LogicalLine* '#' 'End' 'ExternalSource' LineTerminator
    ;
```

<span data-ttu-id="3dc85-129">Diretivas de fonte externa não têm nenhum efeito na compilação e não podem ser aninhadas.</span><span class="sxs-lookup"><span data-stu-id="3dc85-129">External source directives have no effect on compilation and may not be nested.</span></span> <span data-ttu-id="3dc85-130">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3dc85-130">For example:</span></span>

```vb
Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```


## <a name="region-directives"></a><span data-ttu-id="3dc85-131">Diretivas de região</span><span class="sxs-lookup"><span data-stu-id="3dc85-131">Region Directives</span></span>

<span data-ttu-id="3dc85-132">Diretivas de região agrupar linhas de código-fonte, mas não têm nenhum outro efeito na compilação.</span><span class="sxs-lookup"><span data-stu-id="3dc85-132">Region directives group lines of source code but have no other effect on compilation.</span></span> <span data-ttu-id="3dc85-133">O grupo inteiro pode ser recolhido e oculto, ou expandido e exibido no ambiente de desenvolvimento integrado (IDE).</span><span class="sxs-lookup"><span data-stu-id="3dc85-133">The entire group can be collapsed and hidden, or expanded and viewed, in the integrated development environment (IDE).</span></span>

```antlr
RegionStart
    : RegionStatement*
    ;

RegionStatement
    : RegionGroup
    | LogicalLine
    ;

RegionGroup
    : '#' 'Region' StringLiteral LineTerminator
      RegionStatement*
      '#' 'End' 'Region' LineTerminator
    ;
```

<span data-ttu-id="3dc85-134">Regiões podem ser aninhadas.</span><span class="sxs-lookup"><span data-stu-id="3dc85-134">Regions may be nested.</span></span> <span data-ttu-id="3dc85-135">Diretivas de região são especiais em que eles não podem começar nem terminar dentro de um corpo de método, e eles devem respeitar a estrutura de bloco do programa.</span><span class="sxs-lookup"><span data-stu-id="3dc85-135">Region directives are special in that they can neither start nor terminate within a method body, and they must respect the block structure of the program.</span></span> <span data-ttu-id="3dc85-136">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3dc85-136">For example:</span></span>

```vb
Module Test
#Region "Startup code - do not edit"
    Sub Main()
    End Sub
#End Region

End Module


' Error due to Region directives breaking the block structure
Class C
#Region "Fred"
End Class
#End Region
```


## <a name="external-checksum-directives"></a><span data-ttu-id="3dc85-137">Diretivas de soma de verificação externa</span><span class="sxs-lookup"><span data-stu-id="3dc85-137">External Checksum Directives</span></span>

<span data-ttu-id="3dc85-138">Um arquivo de origem pode incluir uma diretiva de soma de verificação externa que indica quais soma de verificação deve ser emitida para um arquivo referenciado em uma diretiva de fonte externa.</span><span class="sxs-lookup"><span data-stu-id="3dc85-138">A source file may include an external checksum directive that indicates what checksum should be emitted for a file referenced in an external source directive.</span></span> <span data-ttu-id="3dc85-139">Em todos os outros aspectos diretivas de fonte externa não têm efeito na compilação.</span><span class="sxs-lookup"><span data-stu-id="3dc85-139">In all other respects external source directives have no effect on compilation.</span></span>

```antlr
ExternalChecksumStart
    : ExternalChecksumStatement*
    ;

ExternalChecksumStatement
    : '#' 'ExternalChecksum' '('
      StringLiteral ',' StringLiteral ',' StringLiteral
      ')' LineTerminator
    ;
```

<span data-ttu-id="3dc85-140">Uma diretiva de soma de verificação externa contém o nome do arquivo do arquivo externo, um identificador global exclusivo (GUID) associado com o arquivo e a soma de verificação para o arquivo.</span><span class="sxs-lookup"><span data-stu-id="3dc85-140">An external checksum directive contains the filename of the external file, a globally unique identifier (GUID) associated with the file and the checksum for the file.</span></span> <span data-ttu-id="3dc85-141">O GUID é especificado como uma constante de cadeia de caracteres no formato "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}", onde x é um dígito hexadecimal.</span><span class="sxs-lookup"><span data-stu-id="3dc85-141">The GUID is specified as a string constant of the form "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}", where x is a hexadecimal digit.</span></span> <span data-ttu-id="3dc85-142">A soma de verificação é especificada como uma constante de cadeia de caracteres do formulário "xxxx...", onde x é um dígito hexadecimal.</span><span class="sxs-lookup"><span data-stu-id="3dc85-142">The checksum is specified as a string constant of the form "xxxx...", where x is a hexadecimal digit.</span></span> <span data-ttu-id="3dc85-143">O número de dígitos em uma soma de verificação deve ser um número par.</span><span class="sxs-lookup"><span data-stu-id="3dc85-143">The number of digits in a checksum must be an even number.</span></span>

<span data-ttu-id="3dc85-144">Um arquivo externo pode ter várias diretivas de soma de verificação externa associadas a ele, desde que todos os valores GUID e soma de verificação de correspondam exata.</span><span class="sxs-lookup"><span data-stu-id="3dc85-144">An external file may have multiple external checksum directives associated with it provided that all of the GUID and checksum values match exactly.</span></span> <span data-ttu-id="3dc85-145">Se o nome do arquivo externo corresponde ao nome de um arquivo que está sendo compilado, a soma de verificação será ignorada em favor de cálculo de soma de verificação do compilador.</span><span class="sxs-lookup"><span data-stu-id="3dc85-145">If the name of the external file matches the name of a file being compiled, the checksum is ignored in favor of the compiler's checksum calculation.</span></span>

<span data-ttu-id="3dc85-146">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3dc85-146">For example:</span></span>

```vb
#ExternalChecksum("c:\wwwroot\inetpub\test.aspx", _
    "{12345678-1234-1234-1234-123456789abc}", _
    "1a2b3c4e5f617239a49b9a9c0391849d34950f923fab9484")

Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```

