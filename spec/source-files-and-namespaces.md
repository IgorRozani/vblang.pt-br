# <a name="source-files-and-namespaces"></a><span data-ttu-id="e4341-101">Arquivos de origem e Namespaces</span><span class="sxs-lookup"><span data-stu-id="e4341-101">Source Files and Namespaces</span></span>

<span data-ttu-id="e4341-102">Um programa Visual Basic consiste em um ou mais arquivos de origem.</span><span class="sxs-lookup"><span data-stu-id="e4341-102">A Visual Basic program consists of one or more source files.</span></span> <span data-ttu-id="e4341-103">Quando um programa é compilado, todos os arquivos de origem são processados juntos; Dessa forma, arquivos de origem podem depender entre si, possivelmente em forma circular, sem a necessidade de declaração de encaminhamento.</span><span class="sxs-lookup"><span data-stu-id="e4341-103">When a program is compiled, all of the source files are processed together; thus, source files can depend on each other, possibly in a circular fashion, without any forward-declaration requirement.</span></span> <span data-ttu-id="e4341-104">A ordem textual das declarações no texto do programa geralmente é de nenhum significado.</span><span class="sxs-lookup"><span data-stu-id="e4341-104">The textual order of declarations in the program text is generally of no significance.</span></span>

<span data-ttu-id="e4341-105">Um arquivo de origem consiste em um conjunto opcional de instruções de opção, instruções de importação e atributos, que são seguidos por um corpo de namespace.</span><span class="sxs-lookup"><span data-stu-id="e4341-105">A source file consists of an optional set of option statements, import statements, and attributes, which are followed by a namespace body.</span></span> <span data-ttu-id="e4341-106">Os atributos, que devem ter o `Assembly` ou `Module` modificador, aplicam-se ao .NET assembly ou módulo produzidos pela compilação.</span><span class="sxs-lookup"><span data-stu-id="e4341-106">The attributes, which must each have either the `Assembly` or `Module` modifier, apply to the .NET assembly or module produced by the compilation.</span></span> <span data-ttu-id="e4341-107">O corpo do arquivo de origem funciona como uma declaração de namespace implícitas para o namespace global, que significa que todas as declarações de nível superior de um arquivo de origem são colocadas no namespace global.</span><span class="sxs-lookup"><span data-stu-id="e4341-107">The body of the source file functions as an implicit namespace declaration for the global namespace, meaning that all declarations at the top level of a source file are placed in the global namespace.</span></span> <span data-ttu-id="e4341-108">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e4341-108">For example:</span></span>

<span data-ttu-id="e4341-109">FileA.vb:</span><span class="sxs-lookup"><span data-stu-id="e4341-109">FileA.vb:</span></span>

```vb
Class A
End Class
```

<span data-ttu-id="e4341-110">FileB.vb:</span><span class="sxs-lookup"><span data-stu-id="e4341-110">FileB.vb:</span></span>

```vb
Class B
End Class
```

<span data-ttu-id="e4341-111">Contribuir com os dois arquivos de origem ao namespace global, nesse caso, declarando duas classes com nomes totalmente qualificados `A` e `B`.</span><span class="sxs-lookup"><span data-stu-id="e4341-111">The two source files contribute to the global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="e4341-112">Porque os dois arquivos de origem contribuir com o mesmo espaço de declaração, teria sido um erro se cada continha uma declaração de um membro com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="e4341-112">Because the two source files contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

<span data-ttu-id="e4341-113">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="e4341-113">__Note.__</span></span> <span data-ttu-id="e4341-114">O ambiente de compilação pode substituir as declarações de namespace no qual um arquivo de origem é colocado implicitamente.</span><span class="sxs-lookup"><span data-stu-id="e4341-114">The compilation environment may override the namespace declarations into which a source file is implicitly placed.</span></span>

<span data-ttu-id="e4341-115">Exceto onde observado, as instruções dentro de um programa Visual Basic podem ser finalizadas por um terminador de linha ou por dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="e4341-115">Except where noted, statements within a Visual Basic program can be terminated either by a line terminator or by a colon.</span></span>

```antlr
Start
    : OptionStatement* ImportsStatement* AttributesStatement* NamespaceMemberDeclaration*
    ;

StatementTerminator
    : LineTerminator
    | ':'
    ;

AttributesStatement
    : Attributes StatementTerminator
    ;
```

## <a name="program-startup-and-termination"></a><span data-ttu-id="e4341-116">Programa de inicialização e encerramento</span><span class="sxs-lookup"><span data-stu-id="e4341-116">Program Startup and Termination</span></span>

<span data-ttu-id="e4341-117">Inicialização do programa ocorre quando o ambiente de execução executa um método designado, o que é conhecido como o programa *ponto de entrada*.</span><span class="sxs-lookup"><span data-stu-id="e4341-117">Program startup occurs when the execution environment executes a designated method, which is referred to as the program's *entry point*.</span></span> <span data-ttu-id="e4341-118">Esse método de ponto de entrada, que deve sempre ser denominado `Main`, devem ser compartilhados, não podem ser contidas em um tipo genérico, não pode ter o modificador async e deve ter uma das seguintes assinaturas:</span><span class="sxs-lookup"><span data-stu-id="e4341-118">This entry point method, which must always be named `Main`, must be shared, cannot be contained in a generic type, cannot have the async modifier, and must have one of the following signatures:</span></span>

```vb
Sub Main()
Sub Main(args() As String)
Function Main() As Integer
Function Main(args() As String) As Integer
```

<span data-ttu-id="e4341-119">A acessibilidade do método do ponto de entrada é irrelevante.</span><span class="sxs-lookup"><span data-stu-id="e4341-119">The accessibility of the entry point method is irrelevant.</span></span> <span data-ttu-id="e4341-120">Se um programa contiver mais de um ponto de entrada adequado, o ambiente de compilação deve designar um como o ponto de entrada.</span><span class="sxs-lookup"><span data-stu-id="e4341-120">If a program contains more than one suitable entry point, the compilation environment must designate one as the entry point.</span></span> <span data-ttu-id="e4341-121">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="e4341-121">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="e4341-122">O ambiente de compilação também pode criar um método de ponto de entrada, se ainda não existir.</span><span class="sxs-lookup"><span data-stu-id="e4341-122">The compilation environment may also create an entry point method if one does not exist.</span></span>

<span data-ttu-id="e4341-123">Quando um programa começa, se o ponto de entrada tem um parâmetro, o argumento fornecido pelo ambiente de execução contém os argumentos de linha de comando para o programa representados como cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="e4341-123">When a program begins, if the entry point has a parameter, the argument supplied by the execution environment contains the command-line arguments to the program represented as strings.</span></span> <span data-ttu-id="e4341-124">Se o ponto de entrada tem um tipo de retorno `Integer`, o valor retornado da função é retornado para o ambiente de execução como o resultado do programa.</span><span class="sxs-lookup"><span data-stu-id="e4341-124">If the entry point has a return type of `Integer`, then the value returned from the function is returned to the execution environment as the result of the program.</span></span>

<span data-ttu-id="e4341-125">Em todos os outros aspectos, métodos do ponto de entrada se comportam da mesma maneira que outros métodos.</span><span class="sxs-lookup"><span data-stu-id="e4341-125">In all other respects, entry point methods behave in the same manner as other methods.</span></span> <span data-ttu-id="e4341-126">Quando a execução deixar a invocação de método do ponto de entrada feita pelo ambiente de execução, o programa é encerrado.</span><span class="sxs-lookup"><span data-stu-id="e4341-126">When execution leaves the invocation of the entry point method made by the execution environment, the program terminates.</span></span>

## <a name="compilation-options"></a><span data-ttu-id="e4341-127">Opções de compilação</span><span class="sxs-lookup"><span data-stu-id="e4341-127">Compilation Options</span></span>

<span data-ttu-id="e4341-128">Um arquivo de origem pode especificar opções de compilação na fonte de código usando *instruções de opção*.</span><span class="sxs-lookup"><span data-stu-id="e4341-128">A source file can specify compilation options in the source code using *option statements*.</span></span>

```antlr
OptionStatement
    : OptionExplicitStatement
    | OptionStrictStatement
    | OptionCompareStatement
    | OptionInferStatement
    ;
```

<span data-ttu-id="e4341-129">Uma `Option` instrução aplica-se somente para o arquivo de origem em que aparece e apenas um de cada tipo de `Option` instrução pode aparecer em um arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="e4341-129">An `Option` statement applies only to the source file in which it appears, and only one of each type of `Option` statement may appear in a source file.</span></span> <span data-ttu-id="e4341-130">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e4341-130">For example:</span></span>

```vb
Option Strict On
Option Compare Text
Option Strict Off    ' Not allowed, Option Strict is already specified.
Option Compare Text  ' Not allowed, Option Compare is already specified.
```

<span data-ttu-id="e4341-131">Há quatro opções de compilação: semântica de tipo estrito, semântica de declaração explícita, semântica de comparação e a semântica de inferência de tipo de variável local.</span><span class="sxs-lookup"><span data-stu-id="e4341-131">There are four compilation options: strict type semantics, explicit declaration semantics, comparison semantics, and local variable type inference semantics.</span></span> <span data-ttu-id="e4341-132">Se um arquivo de origem não inclui um determinado `Option` instrução e, em seguida, o ambiente de compilação determina qual conjunto específico de semântica será usado.</span><span class="sxs-lookup"><span data-stu-id="e4341-132">If a source file does not include a particular `Option` statement, then the compilation environment determines which particular set of semantics will be used.</span></span> <span data-ttu-id="e4341-133">Também é uma opção de compilação quinta, verificações de estouro de inteiro, que pode ser especificado somente por meio do ambiente de compilação.</span><span class="sxs-lookup"><span data-stu-id="e4341-133">There is also a fifth compilation option, integer overflow checks, which can only be specified through the compilation environment.</span></span>


### <a name="option-explicit-statement"></a><span data-ttu-id="e4341-134">Instrução Option Explicit</span><span class="sxs-lookup"><span data-stu-id="e4341-134">Option Explicit Statement</span></span>

<span data-ttu-id="e4341-135">O `Option Explicit` instrução determina se as variáveis locais podem ser declaradas implicitamente.</span><span class="sxs-lookup"><span data-stu-id="e4341-135">The `Option Explicit` statement determines whether local variables may be implicitly declared.</span></span> <span data-ttu-id="e4341-136">As palavras-chave `On` ou `Off` pode seguir a instrução; se nenhum for especificado, o padrão é `On`.</span><span class="sxs-lookup"><span data-stu-id="e4341-136">The keywords `On` or `Off` may follow the statement; if neither is specified, the default is `On`.</span></span> <span data-ttu-id="e4341-137">Se nenhuma instrução for especificada em um arquivo, o ambiente de compilação determina qual será usado.</span><span class="sxs-lookup"><span data-stu-id="e4341-137">If no statement is specified in a file, the compilation environment determines which will be used.</span></span>

```antlr
OptionExplicitStatement
    : 'Option' 'Explicit' OnOff? StatementTerminator
    ;

OnOff
    : 'On' | 'Off'
    ;
```


<span data-ttu-id="e4341-138">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="e4341-138">__Note.__</span></span> <span data-ttu-id="e4341-139">`Explicit` e `Off` não são palavras reservadas.</span><span class="sxs-lookup"><span data-stu-id="e4341-139">`Explicit` and `Off` are not reserved words.</span></span>

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 5 ' Valid because Option Explicit is off.
    End Sub
End Module
```

<span data-ttu-id="e4341-140">Neste exemplo, a variável local `x` é declarado implicitamente atribuindo a ele.</span><span class="sxs-lookup"><span data-stu-id="e4341-140">In this example, the local variable `x` is implicitly declared by assigning to it.</span></span> <span data-ttu-id="e4341-141">O tipo de `x` é `Object`.</span><span class="sxs-lookup"><span data-stu-id="e4341-141">The type of `x` is `Object`.</span></span>


### <a name="option-strict-statement"></a><span data-ttu-id="e4341-142">Instrução Option Strict</span><span class="sxs-lookup"><span data-stu-id="e4341-142">Option Strict Statement</span></span>

<span data-ttu-id="e4341-143">O `Option Strict` instrução determina se as conversões e operações em `Object` são regidos pela semântica de tipo estrito ou permissível e se os tipos são digitados implicitamente como `Object` se nenhum `As` cláusula for especificada.</span><span class="sxs-lookup"><span data-stu-id="e4341-143">The `Option Strict` statement determines whether conversions and operations on `Object` are governed by strict or permissive type semantics and whether types are implicitly typed as `Object` if no `As` clause is specified.</span></span> <span data-ttu-id="e4341-144">A instrução pode ser seguida por palavras-chave `On` ou `Off`; se nenhum for especificado, o padrão é `On`.</span><span class="sxs-lookup"><span data-stu-id="e4341-144">The statement may be followed by the keywords `On` or `Off`; if neither is specified, the default is `On`.</span></span> <span data-ttu-id="e4341-145">Se nenhuma instrução for especificada em um arquivo, o ambiente de compilação determina qual será usado.</span><span class="sxs-lookup"><span data-stu-id="e4341-145">If no statement is specified in a file, the compilation environment determines which will be used.</span></span>

```antlr
OptionStrictStatement
    : 'Option' 'Strict' OnOff? StatementTerminator
    ;
```

<span data-ttu-id="e4341-146">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="e4341-146">__Note.__</span></span> <span data-ttu-id="e4341-147">`Strict` e `Off` não são palavras reservadas.</span><span class="sxs-lookup"><span data-stu-id="e4341-147">`Strict` and `Off` are not reserved words.</span></span>

```vb
Option Strict On

Module Test
    Sub Main()
        Dim x ' Error, no type specified.
        Dim o As Object
        Dim b As Byte = o ' Error, narrowing conversion.

        o.F() ' Error, late binding disallowed.
        o = o + 1 ' Error, addition is not defined on Object.
    End Sub
End Module
```

<span data-ttu-id="e4341-148">Em semântica estrita, o código a seguir não é permitidos:</span><span class="sxs-lookup"><span data-stu-id="e4341-148">Under strict semantics, the following are disallowed:</span></span>

* <span data-ttu-id="e4341-149">Conversões de estreitamento sem um operador de conversão explícita.</span><span class="sxs-lookup"><span data-stu-id="e4341-149">Narrowing conversions without an explicit cast operator.</span></span>

* <span data-ttu-id="e4341-150">Associação tardia.</span><span class="sxs-lookup"><span data-stu-id="e4341-150">Late binding.</span></span>

* <span data-ttu-id="e4341-151">Operações de tipo `Object` diferente de `TypeOf`... `Is`, `Is`, e `IsNot`.</span><span class="sxs-lookup"><span data-stu-id="e4341-151">Operations on type `Object` other than `TypeOf`...`Is`, `Is`, and `IsNot`.</span></span>

* <span data-ttu-id="e4341-152">Omitindo o `As` cláusula em uma declaração que não tem um tipo inferido.</span><span class="sxs-lookup"><span data-stu-id="e4341-152">Omitting the `As` clause in a declaration that does not have an inferred type.</span></span>


### <a name="option-compare-statement"></a><span data-ttu-id="e4341-153">Instrução Option Compare</span><span class="sxs-lookup"><span data-stu-id="e4341-153">Option Compare Statement</span></span>

<span data-ttu-id="e4341-154">O `Option Compare` instrução determina a semântica de comparações de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="e4341-154">The `Option Compare` statement determines the semantics of string comparisons.</span></span> <span data-ttu-id="e4341-155">Comparações de cadeia de caracteres são realizadas a usar comparações binárias (em que o valor binário do Unicode de cada caractere é comparado) ou comparações de cadeias de texto (em que o significado lexical de cada caractere é comparado usando a cultura atual).</span><span class="sxs-lookup"><span data-stu-id="e4341-155">String comparisons are carried out either using binary comparisons (in which the binary Unicode value of each character is compared) or text comparisons (in which the lexical meaning of each character is compared using the current culture).</span></span> <span data-ttu-id="e4341-156">Se nenhuma instrução for especificada em um arquivo, o ambiente de compilação controla o tipo de comparação será usado.</span><span class="sxs-lookup"><span data-stu-id="e4341-156">If no statement is specified in a file, the compilation environment controls which type of comparison will be used.</span></span>

```antlr
OptionCompareStatement
    : 'Option' 'Compare' CompareOption StatementTerminator
    ;

CompareOption
    : 'Binary' | 'Text'
    ;
```

<span data-ttu-id="e4341-157">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="e4341-157">__Note.__</span></span> <span data-ttu-id="e4341-158">`Compare`, `Binary`, e `Text` não são palavras reservadas.</span><span class="sxs-lookup"><span data-stu-id="e4341-158">`Compare`, `Binary`, and `Text` are not reserved words.</span></span>

```vb
Option Compare Text

Module Test
    Sub Main()
        Console.WriteLine("a" = "A")    ' Prints True.
    End Sub
End Module
```

<span data-ttu-id="e4341-159">Nesse caso, a comparação de cadeia de caracteres é feita usando uma comparação de texto que ignora diferenças de maiusculas.</span><span class="sxs-lookup"><span data-stu-id="e4341-159">In this case, the string comparison is done using a text comparison that ignores case differences.</span></span> <span data-ttu-id="e4341-160">Se `Option Compare Binary` tivesse sido especificado, então isso seria impressos `False`.</span><span class="sxs-lookup"><span data-stu-id="e4341-160">If `Option Compare Binary` had been specified, then this would have printed `False`.</span></span>


### <a name="integer-overflow-checks"></a><span data-ttu-id="e4341-161">Verificações de estouro de inteiro</span><span class="sxs-lookup"><span data-stu-id="e4341-161">Integer Overflow Checks</span></span>

<span data-ttu-id="e4341-162">Operações de inteiro podem ser verificadas ou não marcadas para as condições de estouro em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="e4341-162">Integer operations can either be checked or not checked for overflow conditions at run time.</span></span> <span data-ttu-id="e4341-163">Se as condições de estouro forem verificadas e uma operação de inteiro estoura, uma `System.OverflowException` exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="e4341-163">If overflow conditions are checked and an integer operation overflows, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="e4341-164">Se as condições de estouro não forem verificadas, estouros de operação de inteiro não gerarão uma exceção.</span><span class="sxs-lookup"><span data-stu-id="e4341-164">If overflow conditions are not checked, integer operation overflows do not throw an exception.</span></span> <span data-ttu-id="e4341-165">O ambiente de compilação determina se essa opção está ativada ou desativada.</span><span class="sxs-lookup"><span data-stu-id="e4341-165">The compilation environment determines whether this option is on or off.</span></span>

### <a name="option-infer-statement"></a><span data-ttu-id="e4341-166">Instrução Option Infer</span><span class="sxs-lookup"><span data-stu-id="e4341-166">Option Infer Statement</span></span>

<span data-ttu-id="e4341-167">O `Option Infer` instrução determina se local declarações de variáveis que não têm `As` cláusula ter um tipo inferido ou usar `Object`.</span><span class="sxs-lookup"><span data-stu-id="e4341-167">The `Option Infer` statement determines whether local variable declarations that have no `As` clause have an inferred type or use `Object`.</span></span> <span data-ttu-id="e4341-168">A instrução pode ser seguida por palavras-chave `On` ou `Off`; se nenhum for especificado, o padrão é `On`.</span><span class="sxs-lookup"><span data-stu-id="e4341-168">The statement may be followed by the keywords `On` or `Off`; if neither is specified, the default is `On`.</span></span> <span data-ttu-id="e4341-169">Se nenhuma instrução for especificada em um arquivo, o ambiente de compilação determina qual será usado.</span><span class="sxs-lookup"><span data-stu-id="e4341-169">If no statement is specified in a file, the compilation environment determines which will be used.</span></span>

```antlr
OptionInferStatement
    : 'Option' 'Infer' OnOff? StatementTerminator
    ;
```

<span data-ttu-id="e4341-170">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="e4341-170">__Note.__</span></span> <span data-ttu-id="e4341-171">`Infer` e `Off` não são palavras reservadas.</span><span class="sxs-lookup"><span data-stu-id="e4341-171">`Infer` and `Off` are not reserved words.</span></span>

```vb
Option Infer On

Module Test
    Sub Main()
        ' The type of x is Integer
        Dim x = 10

        ' The type of y is String
        Dim y = "abc"
    End Sub
End Module
```


## <a name="imports-statement"></a><span data-ttu-id="e4341-172">Instrução Imports</span><span class="sxs-lookup"><span data-stu-id="e4341-172">Imports Statement</span></span>

<span data-ttu-id="e4341-173">`Imports` instruções de importar os nomes de entidades para um arquivo de origem, permitindo que os nomes de ser referenciados sem qualificação ou importar um namespace para uso em expressões de XML.</span><span class="sxs-lookup"><span data-stu-id="e4341-173">`Imports` statements import the names of entities into a source file, allowing the names to be referenced without qualification, or import a namespace for use in XML expressions.</span></span>

```antlr
ImportsStatement
    : 'Imports' ImportsClauses StatementTerminator
    ;

ImportsClauses
    : ImportsClause ( Comma ImportsClause )*
    ;

ImportsClause
    : AliasImportsClause
    | MembersImportsClause
    | XMLNamespaceImportsClause
    ;
```

<span data-ttu-id="e4341-174">Dentro de declarações de membro em um arquivo de origem que contém um `Imports` instrução, os tipos contidos no namespace específico pode ser referenciada diretamente, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="e4341-174">Within member declarations in a source file that contains an `Imports` statement, the types contained in the given namespace can be referenced directly, as seen in the following example:</span></span>

```vb
Imports N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace 

Namespace N3
    Class B
        Inherits A
    End Class
End Namespace
```

<span data-ttu-id="e4341-175">Aqui, no arquivo de origem, os membros de tipo do namespace `N1.N2` estão diretamente disponíveis e, portanto, a classe `N3.B` deriva da classe `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="e4341-175">Here, within the source file, the type members of namespace `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="e4341-176">`Imports` as instruções devem aparecer após qualquer `Option` instruções, mas antes de qualquer tipo de declarações.</span><span class="sxs-lookup"><span data-stu-id="e4341-176">`Imports` statements must appear after any `Option` statements but before any type declarations.</span></span> <span data-ttu-id="e4341-177">O ambiente de compilação também pode definir implícita `Imports` instruções.</span><span class="sxs-lookup"><span data-stu-id="e4341-177">The compilation environment may also define implicit `Imports` statements.</span></span>

<span data-ttu-id="e4341-178">`Imports` instruções de disponibilizar os nomes em um arquivo de origem, mas não declaram qualquer coisa no espaço de declaração do namespace global.</span><span class="sxs-lookup"><span data-stu-id="e4341-178">`Imports` statements make names available in a source file, but do not declare anything in the global namespace's declaration space.</span></span> <span data-ttu-id="e4341-179">O escopo dos nomes importado por um `Imports` instrução se estende pelas declarações de membro de namespace contidas no arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="e4341-179">The scope of the names imported by an `Imports` statement extends over the namespace member declarations contained in the source file.</span></span> <span data-ttu-id="e4341-180">O escopo de um `Imports` instrução especificamente não incluir outros `Imports` instruções, nem inclui outros arquivos de origem.</span><span class="sxs-lookup"><span data-stu-id="e4341-180">The scope of an `Imports` statement specifically does not include other `Imports` statements, nor does it include other source files.</span></span> <span data-ttu-id="e4341-181">`Imports` as instruções não podem fazer referência umas às outras.</span><span class="sxs-lookup"><span data-stu-id="e4341-181">`Imports` statements may not refer to one another.</span></span>

<span data-ttu-id="e4341-182">Neste exemplo, a última `Imports` instrução é um erro porque ele não é afetado pelo alias de importação do primeiro.</span><span class="sxs-lookup"><span data-stu-id="e4341-182">In this example, the last `Imports` statement is in error because it is not affected by the first import alias.</span></span>

```vb
Imports R1 = N1 ' OK.
Imports R2 = N1.N2 ' OK.
Imports R3 = R1.N2 ' Error: Can't refer to R1.

Namespace N1.N2
End Namespace
```

<span data-ttu-id="e4341-183">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="e4341-183">__Note.__</span></span> <span data-ttu-id="e4341-184">Os nomes de namespace ou tipo que aparecem no `Imports` instruções sempre são tratadas como se fossem totalmente qualificados.</span><span class="sxs-lookup"><span data-stu-id="e4341-184">The namespace or type names that appear in `Imports` statements are always treated as if they are fully qualified.</span></span> <span data-ttu-id="e4341-185">Ou seja, o identificador mais à esquerda em um nome de namespace ou tipo sempre é resolvido no namespace global e procede do restante da resolução de acordo com as regras de resolução de nome normal.</span><span class="sxs-lookup"><span data-stu-id="e4341-185">That is, the leftmost identifier in a namespace or type name always resolves in the global namespace and the rest of the resolution proceeds according to normal name resolution rules.</span></span> <span data-ttu-id="e4341-186">Isso é o único lugar no idioma que se aplica a regra; a regra garante que um nome não pode ser completamente oculto da qualificação.</span><span class="sxs-lookup"><span data-stu-id="e4341-186">This is the only place in the language that applies such a rule; the rule ensures that a name cannot be completely hidden from qualification.</span></span> <span data-ttu-id="e4341-187">Sem a regra, se um nome no namespace global estava oculto em um arquivo de origem em particular, seria impossível especificar todos os nomes de namespace de uma maneira qualificada.</span><span class="sxs-lookup"><span data-stu-id="e4341-187">Without the rule, if a name in the global namespace were hidden in a particular source file, it would be impossible to specify any names from that namespace in a qualified way.</span></span>

<span data-ttu-id="e4341-188">Neste exemplo, o `Imports` instrução sempre refere-se ao global `System` namespace e não a classe no arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="e4341-188">In this example, the `Imports` statement always refers to the global `System` namespace, and not the class in the source file.</span></span>

```vb
Imports System   ' Imports the namespace, not the class.

Class System
End Class
```


### <a name="import-aliases"></a><span data-ttu-id="e4341-189">Aliases de importação</span><span class="sxs-lookup"><span data-stu-id="e4341-189">Import Aliases</span></span>

<span data-ttu-id="e4341-190">Uma *importação alias* define um alias para um namespace ou tipo.</span><span class="sxs-lookup"><span data-stu-id="e4341-190">An *import alias* defines an alias for a namespace or type.</span></span>

```antlr
AliasImportsClause
    : Identifier Equals TypeName
    ;
```

```vb
Imports A = N1.N2.A

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class B
        Inherits A
    End Class
End Namespace
```

<span data-ttu-id="e4341-191">Aqui, no arquivo de origem, `A` é um alias para `N1.N2.A`e, portanto, a classe `N3.B` deriva da classe `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="e4341-191">Here, within the source file, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="e4341-192">O mesmo efeito pode ser obtido pela criação de um alias `R` para `N1.N2` e, em seguida, referenciar `R.A`:</span><span class="sxs-lookup"><span data-stu-id="e4341-192">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>

```vb
Imports R = N1.N2

Namespace N3
    Class B
        Inherits R.A
    End Class
End Namespace
```

<span data-ttu-id="e4341-193">O identificador de um alias de importação deve ser exclusivo dentro do espaço de declaração de namespace global (não apenas o namespace global declaração no arquivo de origem no qual o alias de importação é definido), mesmo que não declara um nome do namespace global espaço de declaração.</span><span class="sxs-lookup"><span data-stu-id="e4341-193">The identifier of an import alias must be unique within the declaration space of the global namespace (not just the global namespace declaration in the source file in which the import alias is defined), even though it does not declare a name in the global namespace's declaration space.</span></span>

<span data-ttu-id="e4341-194">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="e4341-194">__Note.__</span></span> <span data-ttu-id="e4341-195">Declarações em um módulo não introduzem nomes no espaço de declaração do recipiente.</span><span class="sxs-lookup"><span data-stu-id="e4341-195">Declarations in a module do not introduce names into the containing declaration space.</span></span> <span data-ttu-id="e4341-196">Portanto, é válido para uma declaração em um módulo para ter o mesmo nome como um alias de importação, mesmo que o nome da declaração estará acessível no espaço de declaração do recipiente.</span><span class="sxs-lookup"><span data-stu-id="e4341-196">Thus, it is valid for a declaration in a module to have the same name as an import alias, even though the declaration's name will be accessible in the containing declaration space.</span></span>

```vb
' Error: Alias A conflicts with typename A
Imports A = N3.A

Class A
End Class

Namespace N3
    Class A
    End Class
End Namespace
```

<span data-ttu-id="e4341-197">Aqui, o namespace global já contém um membro `A`, portanto, é um erro para um alias de importação usar esse identificador.</span><span class="sxs-lookup"><span data-stu-id="e4341-197">Here, the global namespace already contains a member `A`, so it is an error for an import alias to use that identifier.</span></span> <span data-ttu-id="e4341-198">Da mesma forma, é um erro para dois ou mais identificadores de importação no arquivo de origem para declarar aliases com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="e4341-198">It is likewise an error for two or more import aliases in the same source file to declare aliases by the same name.</span></span>

<span data-ttu-id="e4341-199">Um alias de importação pode criar um alias para qualquer tipo ou namespace.</span><span class="sxs-lookup"><span data-stu-id="e4341-199">An import alias can create an alias for any namespace or type.</span></span> <span data-ttu-id="e4341-200">Acessando um namespace ou tipo por meio de um alias produz exatamente o mesmo resultado que acessar o namespace ou tipo por meio de seu nome declarado.</span><span class="sxs-lookup"><span data-stu-id="e4341-200">Accessing a namespace or type through an alias yields exactly the same result as accessing the namespace or type through its declared name.</span></span>

```vb
Imports R1 = N1
Imports R2 = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class B
        Private a As N1.N2.A
        Private b As R1.N2.A
        Private c As R2.A
    End Class
End Namespace
```

<span data-ttu-id="e4341-201">Aqui, os nomes `N1.N2.A`, `R1.N2.A`, e `R2.A` são equivalentes e todos se referir à classe cujo nome totalmente qualificado é `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="e4341-201">Here, the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent, and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="e4341-202">A importação Especifica o nome exato do namespace ou tipo ao qual ele está criando um alias.</span><span class="sxs-lookup"><span data-stu-id="e4341-202">The import specifies the exact name of the namespace or type to which it is creating an alias.</span></span> <span data-ttu-id="e4341-203">Isso deve ser o nome totalmente qualificado exato desse namespace ou tipo: não usa as regras normais de resolução de nomes qualificados (que permitam o acesso aos membros de uma classe base através de uma classe derivada por exemplo).</span><span class="sxs-lookup"><span data-stu-id="e4341-203">This must be the exact fully qualified name of that namespace or type: it does not use the normal rules for qualified name resolution (which for instance allow access to the members of a base class through a derived class).</span></span>

<span data-ttu-id="e4341-204">Se um pontos de alias de importação para um tipo ou namespace que não pode ser resolvida por essas regras, em seguida, a instrução de importação é ignorada (e o compilador dá um aviso).</span><span class="sxs-lookup"><span data-stu-id="e4341-204">If an import alias points to a type or namespace which cannot be resolved by these rules, then the import statement is ignored (and the compiler gives a warning).</span></span>

<span data-ttu-id="e4341-205">Além disso, a referência não pode ser para um tipo genérico aberto- - todos os tipos genéricos devem ter argumentos de tipo válido fornecidos e todos os argumentos de tipo devem ser resolvível pelas regras acima.</span><span class="sxs-lookup"><span data-stu-id="e4341-205">Also, the reference cannot be to an open generic type -- all generic types must have valid type arguments supplied, and all type arguments must be resolvable by the rules above.</span></span> <span data-ttu-id="e4341-206">Qualquer associação incorreta de um tipo genérico é um erro.</span><span class="sxs-lookup"><span data-stu-id="e4341-206">Any incorrect binding of a generic type is an error.</span></span>

```vb
Imports A = G              ' error: since G is an open generic type
Imports B = G(Of Integer)  ' okay
Imports C = Derived.Nested ' warning: Derived.Nested isn't itself a type
Imports D = G(Of Derived.Nested) ' error: Derived.Nested isn't found

Class G(Of T) : End Class

Class Base
    Class Nested : End Class
End Class

Class Derived : Inherits Base
End Class

Module Module1
    Sub Main()
        Dim x As C               ' error: "C" wasn't succesfully defined
        Dim y As Derived.Nested  ' okay
    End Sub
End Module
```

<span data-ttu-id="e4341-207">O nome do alias de importação de declarações no arquivo de origem podem sombra.</span><span class="sxs-lookup"><span data-stu-id="e4341-207">Declarations in the source file may shadow the import alias name.</span></span>

```vb
Imports R = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class R
    End Class

    Class B
        Inherits R.A  ' Error, R has no member A
    End Class
End Namespace
```

<span data-ttu-id="e4341-208">No exemplo anterior, a referência à `R.A` na declaração de `B` causa um erro porque `R` refere-se ao `N3.R`, e não `N1.N2`.</span><span class="sxs-lookup"><span data-stu-id="e4341-208">In the preceding example the reference to `R.A` in the declaration of `B` causes an error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="e4341-209">Um alias de importação disponibiliza um alias dentro de um arquivo de origem em particular, mas ela não contribui com os novos membros para o espaço de declaração subjacente.</span><span class="sxs-lookup"><span data-stu-id="e4341-209">An import alias makes an alias available within a particular source file, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="e4341-210">Em outras palavras, um alias de importação não é transitivo, mas em vez disso, afeta apenas o arquivo de origem em que ele ocorre.</span><span class="sxs-lookup"><span data-stu-id="e4341-210">In other words, an import alias is not transitive, but rather affects only the source file in which it occurs.</span></span>

<span data-ttu-id="e4341-211">File1.vb:</span><span class="sxs-lookup"><span data-stu-id="e4341-211">File1.vb:</span></span>

```vb
Imports R = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace
```

<span data-ttu-id="e4341-212">File2.vb:</span><span class="sxs-lookup"><span data-stu-id="e4341-212">File2.vb:</span></span>

```vb
Class B
    Inherits R.A ' Error, R unknown.
End Class
```

<span data-ttu-id="e4341-213">No exemplo acima, pois o escopo do alias de importação que introduz `R` só se estende às declarações no arquivo de origem no qual ele está contido, `R` é desconhecido no segundo arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="e4341-213">In the above example, because the scope of the import alias that introduces `R` only extends to declarations in the source file in which it is contained, `R` is unknown in the second source file.</span></span>


### <a name="namespace-imports"></a><span data-ttu-id="e4341-214">Importações de Namespace</span><span class="sxs-lookup"><span data-stu-id="e4341-214">Namespace Imports</span></span>

<span data-ttu-id="e4341-215">Um *importação de namespace* importa todos os membros de um namespace ou tipo, permitindo que o identificador de cada membro do namespace ou tipo a ser usado sem qualificação.</span><span class="sxs-lookup"><span data-stu-id="e4341-215">A *namespace import* imports all of the members of a namespace or type, allowing the identifier of each member of the namespace or type to be used without qualification.</span></span> <span data-ttu-id="e4341-216">No caso de tipos, uma importação do namespace só permite o acesso aos membros do tipo compartilhados sem a necessidade de qualificação do nome da classe.</span><span class="sxs-lookup"><span data-stu-id="e4341-216">In the case of types, a namespace import only allows access to the shared members of the type without requiring qualification of the class name.</span></span> <span data-ttu-id="e4341-217">Em particular, ele permite que os membros de tipos enumerados sejam usados sem qualificação.</span><span class="sxs-lookup"><span data-stu-id="e4341-217">In particular, it allows the members of enumerated types to be used without qualification.</span></span>

```antlr
MembersImportsClause
    : TypeName
    ;
```

<span data-ttu-id="e4341-218">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e4341-218">For example:</span></span>

```vb
Imports Colors

Enum Colors
    Red
    Green
    Blue
End Enum

Module M1
    Sub Main()
        Dim c As Colors = Red
    End Sub
End Module
```

<span data-ttu-id="e4341-219">Ao contrário de um alias de importação, uma importação de namespace não tem restrições nos nomes importa e pode importar namespaces e tipos cujos identificadores já são declarados no namespace global.</span><span class="sxs-lookup"><span data-stu-id="e4341-219">Unlike an import alias, a namespace import has no restrictions on the names it imports and may import namespaces and types whose identifiers are already declared within the global namespace.</span></span> <span data-ttu-id="e4341-220">Os nomes importados por uma importação regular são sombreados por aliases de importação e de declarações no arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="e4341-220">The names imported by a regular import are shadowed by import aliases and declarations in the source file.</span></span>

<span data-ttu-id="e4341-221">No exemplo a seguir `A` refere-se ao `N3.A` vez `N1.N2.A` dentro de declarações de membro no `N3` namespace.</span><span class="sxs-lookup"><span data-stu-id="e4341-221">In the following example, `A` refers to `N3.A` rather than `N1.N2.A` within member declarations in the `N3` namespace.</span></span>

```vb
Imports N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class A
    End Class

    Class B
        Inherits A
    End Class
End Namespace
```

<span data-ttu-id="e4341-222">Quando mais de um namespace importado contém membros com o mesmo nome (e esse nome não for sombreada por um alias de importação ou a declaração), uma referência a esse nome é ambígua e causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="e4341-222">When more than one imported namespace contains members by the same name (and that name is not otherwise shadowed by an import alias or declaration), a reference to that name is ambiguous and causes a compile-time error.</span></span>

```vb
Imports N1
Imports N2

Namespace N1
    Class A
    End Class
End Namespace 

Namespace N2
    Class A
    End Class
End Namespace 

Namespace N3
    Class B
        Inherits A ' Error, A is ambiguous.
    End Class
End Namespace
```

<span data-ttu-id="e4341-223">No exemplo acima, ambos `N1` e `N2` conter um membro `A`.</span><span class="sxs-lookup"><span data-stu-id="e4341-223">In the above example, both `N1` and `N2` contain a member `A`.</span></span> <span data-ttu-id="e4341-224">Porque `N3` importa ambos, referenciando `A` em `N3` causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="e4341-224">Because `N3` imports both, referencing `A` in `N3` causes a compile-time error.</span></span> <span data-ttu-id="e4341-225">Nessa situação, o conflito pode ser resolvido por meio da qualificação de referências a `A`, ou com a introdução de um alias de importação que escolhe um determinado `A`, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="e4341-225">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing an import alias that picks a particular `A`, as in the following example:</span></span>

```vb
Imports N1
Imports N2
Imports A = N1.A

Namespace N3
    Class B
        Inherits A ' A means N1.A.
    End Class
End Namespace
```

<span data-ttu-id="e4341-226">Somente namespaces, classes, estruturas, os tipos enumerados e módulos padrão podem ser importados.</span><span class="sxs-lookup"><span data-stu-id="e4341-226">Only namespaces, classes, structures, enumerated types, and standard modules may be imported.</span></span>


### <a name="xml-namespace-imports"></a><span data-ttu-id="e4341-227">Importações de Namespace XML</span><span class="sxs-lookup"><span data-stu-id="e4341-227">XML Namespace Imports</span></span>

<span data-ttu-id="e4341-228">Uma *importação do namespace XML* define um namespace ou o namespace padrão para expressões não qualificados do XML contidos dentro da unidade de compilação.</span><span class="sxs-lookup"><span data-stu-id="e4341-228">An *XML namespace import* defines a namespace or the default namespace for unqualified XML expressions contained within the compilation unit.</span></span>

```antlr
XMLNamespaceImportsClause
    : '<' XMLNamespaceAttributeName XMLWhitespace? Equals XMLWhitespace?
      XMLNamespaceValue '>'
    ;

XMLNamespaceValue
    : DoubleQuoteCharacter XMLAttributeDoubleQuoteValueCharacter* DoubleQuoteCharacter
    | SingleQuoteCharacter XMLAttributeSingleQuoteValueCharacter* SingleQuoteCharacter
    ;
```

<span data-ttu-id="e4341-229">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e4341-229">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        ' db namespace is "http://example.org/database"
        Dim x = <db:customer><db:Name>Bob</></>

        Console.WriteLine(x.<db:Name>)
    End Sub
End Module
```

<span data-ttu-id="e4341-230">Um namespace XML, incluindo o namespace padrão, só pode ser definido uma vez para um conjunto específico de importações.</span><span class="sxs-lookup"><span data-stu-id="e4341-230">An XML namespace, including the default namespace, can only be defined once for a particular set of imports.</span></span> <span data-ttu-id="e4341-231">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e4341-231">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database-one">
' Error: namespace db is already defined
Imports <xmlns:db="http://example.org/database-two">
```


## <a name="namespaces"></a><span data-ttu-id="e4341-232">Namespaces</span><span class="sxs-lookup"><span data-stu-id="e4341-232">Namespaces</span></span>

<span data-ttu-id="e4341-233">Programas em Visual Basic são organizados com o uso de namespaces.</span><span class="sxs-lookup"><span data-stu-id="e4341-233">Visual Basic programs are organized using namespaces.</span></span> <span data-ttu-id="e4341-234">Namespaces tanto internamente organizar um programa como organizar a maneira como os elementos do programa são expostos a outros programas.</span><span class="sxs-lookup"><span data-stu-id="e4341-234">Namespaces both internally organize a program as well as organize the way program elements are exposed to other programs.</span></span>

<span data-ttu-id="e4341-235">Ao contrário de outras entidades, os namespaces são abertas e pode ser declaradas várias vezes dentro do mesmo programa e, em muitos programas, com cada declaração contribuindo com membros para o mesmo namespace.</span><span class="sxs-lookup"><span data-stu-id="e4341-235">Unlike other entities, namespaces are open-ended, and may be declared multiple times within the same program and across many programs, with each declaration contributing members to the same namespace.</span></span> <span data-ttu-id="e4341-236">No exemplo a seguir, as duas declarações de namespace contribuem para o mesmo espaço de declaração, declarando duas classes com nomes totalmente qualificados `N1.N2.A` e `N1.N2.B`.</span><span class="sxs-lookup"><span data-stu-id="e4341-236">In the following example, the two namespace declarations contribute to the same declaration space, declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span>

```vb
Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N1.N2
    Class B
    End Class
End Namespace
```

<span data-ttu-id="e4341-237">Porque as duas declarações a contribuir com o mesmo espaço de declaração, seria um erro se cada continha uma declaração de um membro com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="e4341-237">Because the two declarations contribute to the same declaration space, it would be an error if each contained a declaration of a member with the same name.</span></span>

<span data-ttu-id="e4341-238">Há um namespace global que tem a nenhum nome e cujos namespaces aninhados e tipos sempre podem ser acessados sem qualificação.</span><span class="sxs-lookup"><span data-stu-id="e4341-238">There is a global namespace that has no name and whose nested namespaces and types can always be accessed without qualification.</span></span> <span data-ttu-id="e4341-239">O escopo de um membro de namespace declarado no namespace global é o texto de programa inteiro.</span><span class="sxs-lookup"><span data-stu-id="e4341-239">The scope of a namespace member declared in the global namespace is the entire program text.</span></span> <span data-ttu-id="e4341-240">Caso contrário, o escopo de um tipo ou namespace declarado em um namespace cujo nome totalmente qualificado é `N` é o texto do programa de cada namespace do nome totalmente qualificado do namespace cujo correspondente começa com `N` ou é `N` em si.</span><span class="sxs-lookup"><span data-stu-id="e4341-240">Otherwise, the scope of a type or namespace declared in a namespace whose fully qualified name is `N` is the program text of each namespace whose corresponding namespace's fully qualified name begins with `N` or is `N` itself.</span></span> <span data-ttu-id="e4341-241">(Observe que um compilador pode optar por colocar as declarações em um namespace específico, por padrão.</span><span class="sxs-lookup"><span data-stu-id="e4341-241">(Note that a compiler can choose to put declarations in a particular namespace by default.</span></span> <span data-ttu-id="e4341-242">Isso não altera o fato de que ainda há um namespace global e sem nome.)</span><span class="sxs-lookup"><span data-stu-id="e4341-242">This does not alter the fact that there is still a global, unnamed namespace.)</span></span>

<span data-ttu-id="e4341-243">Neste exemplo, a classe `B` pode ver a classe `A` porque `B`do namespace `N1.N2.N3` conceitualmente está aninhado dentro do namespace `N1.N2`.</span><span class="sxs-lookup"><span data-stu-id="e4341-243">In this example, the class `B` can see the class `A` because `B`'s namespace `N1.N2.N3` is conceptually nested within the namespace `N1.N2`.</span></span>

```vb
Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N1.N2.N3
    Class B
        Inherits A
    End Class
End Namespace
```

### <a name="namespace-declarations"></a><span data-ttu-id="e4341-244">Declarações de namespace</span><span class="sxs-lookup"><span data-stu-id="e4341-244">Namespace Declarations</span></span>

<span data-ttu-id="e4341-245">Há três formas de declaração de namespace.</span><span class="sxs-lookup"><span data-stu-id="e4341-245">There are three forms of namespace declaration.</span></span>

```antlr
NamespaceDeclaration
    : 'Namespace' NamespaceName StatementTerminator
      NamespaceMemberDeclaration*
      'End' 'Namespace' StatementTerminator
    ;

NamespaceName
    : RelativeNamespaceName
    | 'Global'
    | 'Global' '.' RelativeNamespaceName
    ;

RelativeNamespaceName
    : Identifier ( Period IdentifierOrKeyword )*
    ;
```

<span data-ttu-id="e4341-246">O primeiro formulário começa com a palavra-chave `Namespace` seguido por um nome de namespace relacionado.</span><span class="sxs-lookup"><span data-stu-id="e4341-246">The first form starts with the keyword `Namespace` followed by a relative namespace name.</span></span> <span data-ttu-id="e4341-247">Se o nome do namespace relativo for qualificado, a declaração de namespace é tratada como se ele lexicalmente é aninhado dentro de declarações de namespace correspondente a cada nome no nome qualificado.</span><span class="sxs-lookup"><span data-stu-id="e4341-247">If the relative namespace name is qualified, the namespace declaration is treated as if it is lexically nested within namespace declarations corresponding to each name in the qualified name.</span></span> <span data-ttu-id="e4341-248">Por exemplo, os namespaces a seguir são semanticamente equivalentes:</span><span class="sxs-lookup"><span data-stu-id="e4341-248">For example, the following two namespaces are semantically equivalent:</span></span>

```vb
Namespace N1.N2
    Class A
    End Class

    Class B
    End Class
End Namespace 

Namespace N1
    Namespace N2
        Class A
        End Class

        Class B
        End Class
    End Namespace
End Namespace
```

<span data-ttu-id="e4341-249">O segundo formulário começa com as palavras-chave `Namespace Global`.</span><span class="sxs-lookup"><span data-stu-id="e4341-249">The second form starts with the keywords `Namespace Global`.</span></span> <span data-ttu-id="e4341-250">Ele é tratado como se todas as suas declarações de membro lexicalmente foram colocadas no namespace sem nome global – independentemente de qualquer padrão fornecido pelo ambiente de compilação.</span><span class="sxs-lookup"><span data-stu-id="e4341-250">It is treated as if all its member declarations were lexically placed in the global unnamed namespace -- regardless of any defaults provided by the compilation environment.</span></span> <span data-ttu-id="e4341-251">Essa forma de declaração de namespace não pode ser lexicalmente aninhada dentro de qualquer outra declaração de namespace.</span><span class="sxs-lookup"><span data-stu-id="e4341-251">This form of namespace declaration may not be lexically nested within any other namespace declaration.</span></span>

<span data-ttu-id="e4341-252">O terceiro formulário começa com as palavras-chave `Namespace Global` seguido por um identificador qualificado `N`.</span><span class="sxs-lookup"><span data-stu-id="e4341-252">The third form starts with the keywords `Namespace Global` followed by a qualified identifier `N`.</span></span> <span data-ttu-id="e4341-253">Ele é tratado como se fosse uma declaração de namespace do primeiro formulário "`Namespace N`" lexicalmente, que foi colocado no namespace sem nome global – independentemente de qualquer padrão fornecido pelo ambiente de compilação.</span><span class="sxs-lookup"><span data-stu-id="e4341-253">It is treated as if it were a namespace declaration of the first form "`Namespace N`" that was lexically placed in the global unnamed namespace -- regardless of any defaults provided by the compilation environment.</span></span> <span data-ttu-id="e4341-254">Essa forma de declaração de namespace não pode ser lexicalmente aninhada dentro de qualquer outra declaração de namespace.</span><span class="sxs-lookup"><span data-stu-id="e4341-254">This form of namespace declaration may not be lexically nested within any other namespace declaration.</span></span>

```vb
Namespace Global       ' Puts N1.A in the global namespace
    Namespace N1
        Class A
        End Class
    End Namespace
End Namespace

Namespace Global.N1    ' Equivalent to the above
    Class A
    End Class
End Namespace

Namespace N1           ' May or may not be equivalent to the above,
    Class A            ' depending on defaults provided by the
    End Class          ' compilation environment
End Namespace
```

<span data-ttu-id="e4341-255">Ao lidar com os membros de um namespace, não é importante saber onde um determinado membro é declarado.</span><span class="sxs-lookup"><span data-stu-id="e4341-255">When dealing with the members of a namespace, it is not important where a particular member is declared.</span></span> <span data-ttu-id="e4341-256">Se os dois programas de definem uma entidade com o mesmo nome no mesmo namespace, a tentativa de resolver o nome do namespace causa um erro de ambiguidade.</span><span class="sxs-lookup"><span data-stu-id="e4341-256">If two programs define an entity with the same name in the same namespace, attempting to resolve the name in the namespace causes an ambiguity error.</span></span>

<span data-ttu-id="e4341-257">Namespaces são por definição `Public`, portanto, uma declaração de namespace não pode incluir qualquer modificador de acesso.</span><span class="sxs-lookup"><span data-stu-id="e4341-257">Namespaces are by definition `Public`, so a namespace declaration cannot include any access modifiers.</span></span>


### <a name="namespace-members"></a><span data-ttu-id="e4341-258">Membros do Namespace</span><span class="sxs-lookup"><span data-stu-id="e4341-258">Namespace Members</span></span>

<span data-ttu-id="e4341-259">Os membros do Namespace só podem ser declarações de namespace e as declarações de tipo.</span><span class="sxs-lookup"><span data-stu-id="e4341-259">Namespace members can only be namespace declarations and type declarations.</span></span> <span data-ttu-id="e4341-260">Declarações de tipo podem ter `Public` ou `Friend` acesso.</span><span class="sxs-lookup"><span data-stu-id="e4341-260">Type declarations may have `Public` or `Friend` access.</span></span> <span data-ttu-id="e4341-261">O acesso padrão para tipos é `Friend` acesso.</span><span class="sxs-lookup"><span data-stu-id="e4341-261">The default access for types is `Friend` access.</span></span>

```antlr
NamespaceMemberDeclaration
    : NamespaceDeclaration
    | TypeDeclaration
    ;

TypeDeclaration
    : ModuleDeclaration
    | NonModuleDeclaration
    ;

NonModuleDeclaration
    : EnumDeclaration
    | StructureDeclaration
    | InterfaceDeclaration
    | ClassDeclaration
    | DelegateDeclaration
    ;
```
