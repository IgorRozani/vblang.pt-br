# <a name="lexical-grammar"></a><span data-ttu-id="1693a-101">Gramática lexical</span><span class="sxs-lookup"><span data-stu-id="1693a-101">Lexical Grammar</span></span>

<span data-ttu-id="1693a-102">Compilação de um programa Visual Basic primeiro envolve traduzindo o fluxo bruto de caracteres Unicode em um conjunto ordenado de tokens léxicos.</span><span class="sxs-lookup"><span data-stu-id="1693a-102">Compilation of a Visual Basic program first involves translating the raw stream of Unicode characters into an ordered set of lexical tokens.</span></span> <span data-ttu-id="1693a-103">Como a linguagem Visual Basic não é um formato livre, o conjunto de tokens, em seguida, é dividido em uma série de linhas lógicas.</span><span class="sxs-lookup"><span data-stu-id="1693a-103">Because the Visual Basic language is not free-format, the set of tokens is then further divided into a series of logical lines.</span></span> <span data-ttu-id="1693a-104">Um *linha lógica* spans do início do fluxo ou por meio de um terminador de linha para o próxima terminador de linha que não é precedido por uma continuação de linha ou por meio até o final do fluxo.</span><span class="sxs-lookup"><span data-stu-id="1693a-104">A *logical line* spans from either the start of the stream or a line terminator through to the next line terminator that is not preceded by a line continuation or through to the end of the stream.</span></span>

<span data-ttu-id="1693a-105">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="1693a-105">__Note.__</span></span> <span data-ttu-id="1693a-106">Com a introdução de expressões literais XML na versão 9.0 do idioma, Visual Basic não tem mais uma gramática lexical distinta no sentido esse código pode ser indexado sem considerar o contexto sintático do Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="1693a-106">With the introduction of XML literal expressions in version 9.0 of the language, Visual Basic no longer has a distinct lexical grammar in the sense that Visual Basic code can be tokenized without regard to the syntactic context.</span></span> <span data-ttu-id="1693a-107">Isso é devido ao fato de que XML e o Visual Basic têm diferentes regras lexicais e o conjunto de regras lexicais em uso a qualquer momento determinado depende de quais constructo sintático está sendo processado no momento.</span><span class="sxs-lookup"><span data-stu-id="1693a-107">This is due to the fact that XML and Visual Basic have different lexical rules and the set of lexical rules in use at any particular time depends on what syntactic construct is being processed at that moment.</span></span> <span data-ttu-id="1693a-108">Essa especificação retém nesta seção gramática lexical como um guia para regras lexicais de regular código do Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="1693a-108">This specification retains this lexical grammar section as a guide to the lexical rules of regular Visual Basic code.</span></span>

```antlr
LogicalLineStart
    : LogicalLine*
    ;

LogicalLine
    : LogicalLineElement* Comment? LineTerminator
    ;

LogicalLineElement
    : WhiteSpace
    | LineContinuation
    | Token
    ;

Token
    : Identifier
    | Keyword
    | Literal
    | Separator
    | Operator
    ;
```

## <a name="characters-and-lines"></a><span data-ttu-id="1693a-109">Caracteres e linhas</span><span class="sxs-lookup"><span data-stu-id="1693a-109">Characters and Lines</span></span>

<span data-ttu-id="1693a-110">Programas em Visual Basic são compostos de caracteres do conjunto de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="1693a-110">Visual Basic programs are composed of characters from the Unicode character set.</span></span>

```antlr
Character:
    '<Any Unicode character except a LineTerminator>'
    ;
```

### <a name="line-terminators"></a><span data-ttu-id="1693a-111">Terminadores de linha</span><span class="sxs-lookup"><span data-stu-id="1693a-111">Line Terminators</span></span>

<span data-ttu-id="1693a-112">Caracteres de quebra de linha Unicode separam linhas lógicas.</span><span class="sxs-lookup"><span data-stu-id="1693a-112">Unicode line break characters separate logical lines.</span></span>

```antlr
LineTerminator
    : '<Unicode 0x00D>'
    | '<Unicode 0x00A>'
    | '<CR>'
    | '<LF>'
    | '<Unicode 0x2028>'
    | '<Unicode 0x2029>'
    ;
```

### <a name="line-continuation"></a><span data-ttu-id="1693a-113">Continuação de linha</span><span class="sxs-lookup"><span data-stu-id="1693a-113">Line Continuation</span></span>

<span data-ttu-id="1693a-114">Um *continuação de linha* consiste em pelo menos um caractere de espaço em branco que precede imediatamente um único caractere de sublinhado como o último caractere (que não seja espaço em branco) em uma linha de texto.</span><span class="sxs-lookup"><span data-stu-id="1693a-114">A *line continuation* consists of at least one white-space character that immediately precedes a single underscore character as the last character (other than white space) in a text line.</span></span> <span data-ttu-id="1693a-115">Uma continuação de linha permite que uma linha lógica abranger mais de uma linha física.</span><span class="sxs-lookup"><span data-stu-id="1693a-115">A line continuation allows a logical line to span more than one physical line.</span></span> <span data-ttu-id="1693a-116">As continuações das linhas são tratadas como se fossem espaço em branco, mesmo que eles não são.</span><span class="sxs-lookup"><span data-stu-id="1693a-116">Line continuations are treated as if they were white space, even though they are not.</span></span>

```antlr
LineContinuation
    : WhiteSpace '_' WhiteSpace* LineTerminator
    ;
```

<span data-ttu-id="1693a-117">O programa a seguir mostra alguns continuações de linha:</span><span class="sxs-lookup"><span data-stu-id="1693a-117">The following program shows some line continuations:</span></span>

```vb
Module Test
    Sub Print( _
        Param1 As Integer, _
        Param2 As Integer )

        If (Param1 < Param2) Or _
            (Param1 > Param2) Then
            Console.WriteLine("Not equal")
        End If
    End Function
End Module
```

<span data-ttu-id="1693a-118">Alguns locais na gramática sintática permitem *continuações de linha implícitas*.</span><span class="sxs-lookup"><span data-stu-id="1693a-118">Some places in the syntactic grammar allow for *implicit line continuations*.</span></span> <span data-ttu-id="1693a-119">Quando um terminador de linha é encontrado</span><span class="sxs-lookup"><span data-stu-id="1693a-119">When a line terminator is encountered</span></span>

* <span data-ttu-id="1693a-120">Depois de uma vírgula (`,`), abra parênteses (`(`), abra a chave de abertura (`{`), ou abra a expressão inserida (`<%=`)</span><span class="sxs-lookup"><span data-stu-id="1693a-120">after a comma (`,`), open parenthesis (`(`), open curly brace (`{`), or open embedded expression (`<%=`)</span></span>

* <span data-ttu-id="1693a-121">Depois de um qualificador de membro (`.` ou `.@` ou `...`), desde que algo está sendo qualificado (ou seja, não está usando implícito `With` contexto)</span><span class="sxs-lookup"><span data-stu-id="1693a-121">after a member qualifier (`.` or `.@` or `...`), provided that something is being qualified (i.e. is not using an implicit `With` context)</span></span>

* <span data-ttu-id="1693a-122">antes de um parêntese de fechamento (`)`), feche a chave de fechamento (`}`), ou de expressão incorporada fechar (`%>`)</span><span class="sxs-lookup"><span data-stu-id="1693a-122">before a close parenthesis (`)`), close curly brace (`}`), or close embedded expression (`%>`)</span></span>

* <span data-ttu-id="1693a-123">Após um menor-que (`<`) em um contexto de atributo</span><span class="sxs-lookup"><span data-stu-id="1693a-123">after a less-than (`<`) in an attribute context</span></span>

* <span data-ttu-id="1693a-124">antes de uma maior-que (`>`) em um contexto de atributo</span><span class="sxs-lookup"><span data-stu-id="1693a-124">before a greater-than (`>`) in an attribute context</span></span>

* <span data-ttu-id="1693a-125">Depois de uma maior-que (`>`) em um contexto de atributo de nível de arquivo não</span><span class="sxs-lookup"><span data-stu-id="1693a-125">after a greater-than (`>`) in a non-file-level attribute context</span></span>

* <span data-ttu-id="1693a-126">antes e depois de operadores de consulta (`Where`, `Order`, `Select`, etc.)</span><span class="sxs-lookup"><span data-stu-id="1693a-126">before and after query operators (`Where`, `Order`, `Select`, etc.)</span></span>

* <span data-ttu-id="1693a-127">Depois de operadores binários (`+`, `-`, `/`, `*`, etc.) em um contexto de expressão</span><span class="sxs-lookup"><span data-stu-id="1693a-127">after binary operators (`+`, `-`, `/`, `*`, etc.) in an expression context</span></span>

* <span data-ttu-id="1693a-128">Depois de operadores de atribuição (`=`, `:=`, `+=`, `-=`, etc.) em qualquer contexto.</span><span class="sxs-lookup"><span data-stu-id="1693a-128">after assignment operators (`=`, `:=`, `+=`, `-=`, etc.) in any context.</span></span>

<span data-ttu-id="1693a-129">o terminador de linha é tratado como se fosse uma continuação de linha.</span><span class="sxs-lookup"><span data-stu-id="1693a-129">the line terminator is treated as if it was a line continuation.</span></span>

```antlr
Comma
    : ',' LineTerminator?
    ;

Period
    : '.' LineTerminator?
    ;

OpenParenthesis
    : '(' LineTerminator?
    ;

CloseParenthesis
    : LineTerminator? ')'
    ;

OpenCurlyBrace
    : '{' LineTerminator?
    ;

CloseCurlyBrace
    : LineTerminator? '}'
    ;

Equals
    : '=' LineTerminator?
    ;

ColonEquals
    : ':' '=' LineTerminator?
    ;
```

<span data-ttu-id="1693a-130">Por exemplo, o exemplo anterior também poderia ser escrito como:</span><span class="sxs-lookup"><span data-stu-id="1693a-130">For example, the previous example could also be written as:</span></span>

```vb
Module Test
    Sub Print(
        Param1 As Integer,
        Param2 As Integer)

        If (Param1 < Param2) Or
            (Param1 > Param2) Then
            Console.WriteLine("Not equal")
        End If
    End Function
End Module
```

<span data-ttu-id="1693a-131">Continuação implícita de linha só será inferida diretamente antes ou após o token especificado.</span><span class="sxs-lookup"><span data-stu-id="1693a-131">Implicit line continuations will only ever be inferred directly before or after the specified token.</span></span> <span data-ttu-id="1693a-132">Eles não serão inferidos antes ou depois de uma continuação de linha.</span><span class="sxs-lookup"><span data-stu-id="1693a-132">They will not be inferred before or after a line continuation.</span></span> <span data-ttu-id="1693a-133">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="1693a-133">For example:</span></span>

```vb
Dim y = 10
' Error: Expression expected for assignment to x
Dim x = _

y
```

<span data-ttu-id="1693a-134">Continuações de linha não serão inferidas em contextos de compilação condicional.</span><span class="sxs-lookup"><span data-stu-id="1693a-134">Line continuations will not be inferred in conditional compilation contexts.</span></span> <span data-ttu-id="1693a-135">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="1693a-135">(__Note.__</span></span> <span data-ttu-id="1693a-136">Essa última restrição é necessária porque o texto em blocos de compilação condicional que não são compilados não precisa ser sintaticamente válido.</span><span class="sxs-lookup"><span data-stu-id="1693a-136">This last restriction is required because text in conditional compilation blocks that are not compiled do not have to be syntactically valid.</span></span> <span data-ttu-id="1693a-137">Assim, texto no bloco de pode acidentalmente "selecionado para cima" da instrução de compilação condicional, especialmente à medida que a linguagem obtém estendida no futuro.)</span><span class="sxs-lookup"><span data-stu-id="1693a-137">Thus, text in the block might accidentally get "picked up" by the conditional compilation statement, especially as the language gets extended in the future.)</span></span>


### <a name="white-space"></a><span data-ttu-id="1693a-138">Espaço em branco</span><span class="sxs-lookup"><span data-stu-id="1693a-138">White Space</span></span>

<span data-ttu-id="1693a-139">*Espaço em branco* serve apenas para separar os tokens e é ignorado.</span><span class="sxs-lookup"><span data-stu-id="1693a-139">*White space* serves only to separate tokens and is otherwise ignored.</span></span> <span data-ttu-id="1693a-140">Linhas de lógicas que contém somente espaços em branco são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="1693a-140">Logical lines containing only white space are ignored.</span></span> <span data-ttu-id="1693a-141">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="1693a-141">(__Note.__</span></span>
<span data-ttu-id="1693a-142">Terminadores de linha não são considerados espaço em branco.)</span><span class="sxs-lookup"><span data-stu-id="1693a-142">Line terminators are not considered white space.)</span></span>

```antlr
WhiteSpace
    : '<Unicode class Zs>'
    | '<Unicode Tab 0x0009>'
    ;
```

### <a name="comments"></a><span data-ttu-id="1693a-143">Comentários</span><span class="sxs-lookup"><span data-stu-id="1693a-143">Comments</span></span>

<span data-ttu-id="1693a-144">Um *comentário* começa com um caractere de aspas simples ou a palavra-chave `REM`.</span><span class="sxs-lookup"><span data-stu-id="1693a-144">A *comment* begins with a single-quote character or the keyword `REM`.</span></span> <span data-ttu-id="1693a-145">Um caractere de aspas simples é um caractere de aspas simples ASCII, um caractere de aspas simples à esquerda do Unicode ou um direito de Unicode caractere de aspas simples.</span><span class="sxs-lookup"><span data-stu-id="1693a-145">A single-quote character is either an ASCII single-quote character, a Unicode left single-quote character, or a Unicode right single-quote character.</span></span> <span data-ttu-id="1693a-146">Comentários podem começar em qualquer lugar em uma linha de código-fonte e o final da linha física termina o comentário.</span><span class="sxs-lookup"><span data-stu-id="1693a-146">Comments can begin anywhere on a source line, and the end of the physical line ends the comment.</span></span> <span data-ttu-id="1693a-147">O compilador ignora os caracteres entre o início do comentário e o terminador de linha.</span><span class="sxs-lookup"><span data-stu-id="1693a-147">The compiler ignores the characters between the beginning of the comment and the line terminator.</span></span> <span data-ttu-id="1693a-148">Consequentemente, os comentários não é possível estender em várias linhas usando continuações de linha.</span><span class="sxs-lookup"><span data-stu-id="1693a-148">Consequently, comments cannot extend across multiple lines by using line continuations.</span></span>

```antlr
Comment
    : CommentMarker Character*
    ;

CommentMarker
    : SingleQuoteCharacter
    | 'REM'
    ;

SingleQuoteCharacter
    : '\''
    | '<Unicode 0x2018>'
    | '<Unicode 0x2019>'
    ;
```

## <a name="identifiers"></a><span data-ttu-id="1693a-149">Identificadores</span><span class="sxs-lookup"><span data-stu-id="1693a-149">Identifiers</span></span>

<span data-ttu-id="1693a-150">Uma *identificador* é um nome.</span><span class="sxs-lookup"><span data-stu-id="1693a-150">An *identifier* is a name.</span></span> <span data-ttu-id="1693a-151">Identificadores de Visual Basic estão em conformidade com o Unicode Standard Annex 15 com uma exceção: identificadores podem começar com um caractere de sublinhado (conector).</span><span class="sxs-lookup"><span data-stu-id="1693a-151">Visual Basic identifiers conform to the Unicode Standard Annex 15 with one exception: identifiers may begin with an underscore (connector) character.</span></span> <span data-ttu-id="1693a-152">Se um identificador começa com um sublinhado, ele deve conter pelo menos um outro caractere identificador válido para resolver a ambiguidade de uma continuação de linha.</span><span class="sxs-lookup"><span data-stu-id="1693a-152">If an identifier begins with an underscore, it must contain at least one other valid identifier character to disambiguate it from a line continuation.</span></span>

```antlr
Identifier
    : NonEscapedIdentifier TypeCharacter?
    | Keyword TypeCharacter
    | EscapedIdentifier
    ;

NonEscapedIdentifier
    : '<Any IdentifierName but not Keyword>'
    ;

EscapedIdentifier
    : '[' IdentifierName ']'
    ;

IdentifierName
    : IdentifierStart IdentifierCharacter*
    ;

IdentifierStart
    : AlphaCharacter
    | UnderscoreCharacter IdentifierCharacter
    ;

IdentifierCharacter
    : UnderscoreCharacter
    | AlphaCharacter
    | NumericCharacter
    | CombiningCharacter
    | FormattingCharacter
    ;

AlphaCharacter
    : '<Unicode classes Lu,Ll,Lt,Lm,Lo,Nl>'
    ;

NumericCharacter
    : '<Unicode decimal digit class Nd>'
    ;

CombiningCharacter
    : '<Unicode combining character classes Mn, Mc>'
    ;

FormattingCharacter
    : '<Unicode formatting character class Cf>'
    ;

UnderscoreCharacter
    : '<Unicode connection character class Pc>'
    ;

IdentifierOrKeyword
    : Identifier
    | Keyword
    ;
```

<span data-ttu-id="1693a-153">Identificadores normais não podem corresponder a palavras-chave, mas podem identificadores escapados ou identificadores com um caractere de tipo.</span><span class="sxs-lookup"><span data-stu-id="1693a-153">Regular identifiers may not match keywords, but escaped identifiers or identifiers with a type character can.</span></span> <span data-ttu-id="1693a-154">Uma *identificador de escape* é um identificador delimitado por colchetes.</span><span class="sxs-lookup"><span data-stu-id="1693a-154">An *escaped identifier* is an identifier delimited by square brackets.</span></span> <span data-ttu-id="1693a-155">Escape identificadores seguem as mesmas regras de identificadores regulares, exceto que eles podem corresponder a palavras-chave e podem não ter caracteres de tipo.</span><span class="sxs-lookup"><span data-stu-id="1693a-155">Escaped identifiers follow the same rules as regular identifiers except that they may match keywords and may not have type characters.</span></span>

<span data-ttu-id="1693a-156">Este exemplo define uma classe chamada `class` com um método compartilhado denominado `shared` que usa um parâmetro denominado `boolean` e, em seguida, chama o método.</span><span class="sxs-lookup"><span data-stu-id="1693a-156">This example defines a class named `class` with a shared method named `shared` that takes a parameter named `boolean` and then calls the method.</span></span>

```vb
Class [class]
    Shared Sub [shared]([boolean] As Boolean)
        If [boolean] Then
            Console.WriteLine("true")
        Else
            Console.WriteLine("false")
        End If
    End Sub
End Class

Module [module]
    Sub Main()
        [class].[shared](True)
    End Sub
End Module
```

<span data-ttu-id="1693a-157">Identificadores diferenciam maiusculas de minúsculas, portanto, dois identificadores serão considerados para ser o mesmo identificador se eles diferem apenas em maiusculas.</span><span class="sxs-lookup"><span data-stu-id="1693a-157">Identifiers are case insensitive, so two identifiers are considered to be the same identifier if they differ only in case.</span></span> <span data-ttu-id="1693a-158">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="1693a-158">(__Note.__</span></span> <span data-ttu-id="1693a-159">A mapeamentos de casos individual padrão Unicode são usados durante a comparação de identificadores e os mapeamentos de maiusculas específicas da localidade são ignorados).</span><span class="sxs-lookup"><span data-stu-id="1693a-159">The Unicode Standard one-to-one case mappings are used when comparing identifiers and any locale-specific case mappings are ignored.)</span></span>


### <a name="type-characters"></a><span data-ttu-id="1693a-160">Caracteres de tipo</span><span class="sxs-lookup"><span data-stu-id="1693a-160">Type Characters</span></span>

<span data-ttu-id="1693a-161">Um *caractere de tipo* indica o tipo do identificador anterior.</span><span class="sxs-lookup"><span data-stu-id="1693a-161">A *type character* denotes the type of the preceding identifier.</span></span> <span data-ttu-id="1693a-162">O caractere de tipo não é considerado parte do identificador.</span><span class="sxs-lookup"><span data-stu-id="1693a-162">The type character is not considered part of the identifier.</span></span>

```antlr
TypeCharacter
    : IntegerTypeCharacter
    | LongTypeCharacter
    | DecimalTypeCharacter
    | SingleTypeCharacter
    | DoubleTypeCharacter
    | StringTypeCharacter
    ;

IntegerTypeCharacter
    : '%'
    ;

LongTypeCharacter
    : '&'
    ;

DecimalTypeCharacter
    : '@'
    ;

SingleTypeCharacter
    : '!'
    ;

DoubleTypeCharacter
    : '#'
    ;

StringTypeCharacter
    : '$'
    ;
```

<span data-ttu-id="1693a-163">Se uma declaração inclui um caractere de tipo, o caractere de tipo deve concordar com o tipo especificado na declaração em si; Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="1693a-163">If a declaration includes a type character, the type character must agree with the type specified in the declaration itself; otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="1693a-164">Se a declaração omite o tipo (por exemplo, se ele não especifica um `As` cláusula), o caractere de tipo implicitamente é substituído como o tipo da declaração.</span><span class="sxs-lookup"><span data-stu-id="1693a-164">If the declaration omits the type (for example, if it does not specify an `As` clause), the type character is implicitly substituted as the type of the declaration.</span></span>

<span data-ttu-id="1693a-165">Nenhum espaço em branco podem vir entre um identificador e seu caractere de tipo.</span><span class="sxs-lookup"><span data-stu-id="1693a-165">No white space may come between an identifier and its type character.</span></span> <span data-ttu-id="1693a-166">Não há nenhum caractere de tipo para `Byte`, `SByte`, `UShort`, `Short`, `UInteger` ou `ULong`, devido à falta de caracteres adequados.</span><span class="sxs-lookup"><span data-stu-id="1693a-166">There are no type characters for `Byte`, `SByte`, `UShort`, `Short`, `UInteger` or `ULong`, due to a lack of suitable characters.</span></span>

<span data-ttu-id="1693a-167">Acrescentar um caractere de tipo para um identificador que conceitualmente não tem um tipo (por exemplo, um nome de namespace) ou a um identificador cujo tipo não concorda com o tipo de caractere de tipo faz com que um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="1693a-167">Appending a type character to an identifier that conceptually does not have a type (for example, a namespace name) or to an identifier whose type disagrees with the type of the type character causes a compile-time error.</span></span>

<span data-ttu-id="1693a-168">O exemplo a seguir mostra o uso de caracteres de tipo:</span><span class="sxs-lookup"><span data-stu-id="1693a-168">The following example shows the use of type characters:</span></span>

```vb
' The follow line will cause an error: standard modules have no type.
Module Test1#
End Module

Module Test2

    ' This function takes a Long parameter and returns a String.
    Function Func$(Param&)

        ' The following line causes an error because the type character
        ' conflicts with the declared type of Func and Param.
        Func# = CStr(Param@)

        ' The following line is valid.
        Func$ = CStr(Param&)
    End Function
End Module
```

<span data-ttu-id="1693a-169">O caractere de tipo `!` apresenta um problema especial que pode ser usado como um caractere de tipo e como um separador na linguagem.</span><span class="sxs-lookup"><span data-stu-id="1693a-169">The type character `!` presents a special problem in that it can be used both as a type character and as a separator in the language.</span></span> <span data-ttu-id="1693a-170">Para remover a ambiguidade, um `!` caractere é um caractere de tipo, desde que o caractere seguinte não é possível iniciar um identificador.</span><span class="sxs-lookup"><span data-stu-id="1693a-170">To remove ambiguity, a `!` character is a type character as long as the character that follows it cannot start an identifier.</span></span> <span data-ttu-id="1693a-171">Se for possível, então o `!` caractere é um separador, não é um caractere de tipo.</span><span class="sxs-lookup"><span data-stu-id="1693a-171">If it can, then the `!` character is a separator, not a type character.</span></span>


## <a name="keywords"></a><span data-ttu-id="1693a-172">Palavras-chave</span><span class="sxs-lookup"><span data-stu-id="1693a-172">Keywords</span></span>

<span data-ttu-id="1693a-173">Um *palavra-chave* é uma palavra que tem um significado especial em um constructo de linguagem.</span><span class="sxs-lookup"><span data-stu-id="1693a-173">A *keyword* is a word that has special meaning in a language construct.</span></span> <span data-ttu-id="1693a-174">Todas as palavras-chave são reservadas pelo idioma e não podem ser usadas como identificadores, a menos que os identificadores são ignorados.</span><span class="sxs-lookup"><span data-stu-id="1693a-174">All keywords are reserved by the language and may not be used as identifiers unless the identifiers are escaped.</span></span> <span data-ttu-id="1693a-175">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="1693a-175">(__Note.__</span></span> <span data-ttu-id="1693a-176">`EndIf``GoSub`, `Let`, `Variant`, e `Wend` são mantidos como palavras-chave, embora eles não são mais usados no Visual Basic.)</span><span class="sxs-lookup"><span data-stu-id="1693a-176">`EndIf`, `GoSub`, `Let`, `Variant`, and `Wend` are retained as keywords, although they are no longer used in Visual Basic.)</span></span>

```antlr
Keyword
    : 'AddHandler'      | 'AddressOf'      | 'Alias'       | 'And'
    | 'AndAlso'         | 'As'             | 'Boolean'     | 'ByRef'
    | 'Byte'            | 'ByVal'          | 'Call'        | 'Case'        
    | 'Catch'           | 'CBool'          | 'CByte'       | 'CChar'       
    | 'CDate'           | 'CDbl'           | 'CDec'        | 'Char'        
    | 'CInt'            | 'Class'          | 'CLng'        | 'CObj'        
    | 'Const'           | 'Continue'       | 'CSByte'      | 'CShort'      
    | 'CSng'            | 'CStr'           | 'CType'       | 'CUInt'       
    | 'CULng'           | 'CUShort'        | 'Date'        | 'Decimal'     
    | 'Declare'         | 'Default'        | 'Delegate'    | 'Dim'         
    | 'DirectCast'      | 'Do'             | 'Double'      | 'Each'        
    | 'Else'            | 'ElseIf'         | 'End'         | 'EndIf'       
    | 'Enum'            | 'Erase'          | 'Error'       | 'Event'       
    | 'Exit'            | 'False'          | 'Finally'     | 'For'         
    | 'Friend'          | 'Function'       | 'Get'         | 'GetType'     
    | 'GetXmlNamespace' | 'Global'         | 'GoSub'       | 'GoTo'        
    | 'Handles'         | 'If'             | 'Implements'  | 'Imports'     
    | 'In'              | 'Inherits'       | 'Integer'     | 'Interface'   
    | 'Is'              | 'IsNot'          | 'Let'         | 'Lib'         
    | 'Like'            | 'Long'           | 'Loop'        | 'Me'          
    | 'Mod'             | 'Module'         | 'MustInherit' | 'MustOverride'
    | 'MyBase'          | 'MyClass'        | 'Namespace'   | 'Narrowing'   
    | 'New'             | 'Next'           | 'Not'         | 'Nothing'     
    | 'NotInheritable'  | 'NotOverridable' | 'Object'      | 'Of'          
    | 'On'              | 'Operator'       | 'Option'      | 'Optional'    
    | 'Or'              | 'OrElse'         | 'Overloads'   | 'Overridable' 
    | 'Overrides'       | 'ParamArray'     | 'Partial'     | 'Private'     
    | 'Property'        | 'Protected'      | 'Public'      | 'RaiseEvent'  
    | 'ReadOnly'        | 'ReDim'          | 'REM'         | 'RemoveHandler'
    | 'Resume'          | 'Return'         | 'SByte'       | 'Select'      
    | 'Set'             | 'Shadows'        | 'Shared'      | 'Short'       
    | 'Single'          | 'Static'         | 'Step'        | 'Stop'        
    | 'String'          | 'Structure'      | 'Sub'         | 'SyncLock'    
    | 'Then'            | 'Throw'          | 'To'          | 'True'        
    | 'Try'             | 'TryCast'        | 'TypeOf'      | 'UInteger'    
    | 'ULong'           | 'UShort'         | 'Using'       | 'Variant'     
    | 'Wend'            | 'When'           | 'While'       | 'Widening'    
    | 'With'            | 'WithEvents'     | 'WriteOnly'   | 'Xor'         
    ;
```

## <a name="literals"></a><span data-ttu-id="1693a-177">Literais</span><span class="sxs-lookup"><span data-stu-id="1693a-177">Literals</span></span>

<span data-ttu-id="1693a-178">Um *literal* é uma representação textual de um determinado valor de um tipo.</span><span class="sxs-lookup"><span data-stu-id="1693a-178">A *literal* is a textual representation of a particular value of a type.</span></span> <span data-ttu-id="1693a-179">Tipos de literal incluem booliano, inteiro, ponto flutuante, cadeia de caracteres, caractere e data.</span><span class="sxs-lookup"><span data-stu-id="1693a-179">Literal types include Boolean, integer, floating point, string, character, and date.</span></span>

```antlr
Literal
    : BooleanLiteral
    | IntegerLiteral
    | FloatingPointLiteral
    | StringLiteral
    | CharacterLiteral
    | DateLiteral
    | Nothing
    ;
```

### <a name="boolean-literals"></a><span data-ttu-id="1693a-180">Literais boolianos</span><span class="sxs-lookup"><span data-stu-id="1693a-180">Boolean Literals</span></span>

<span data-ttu-id="1693a-181">`True` e `False` são literais do `Boolean` tipo que são mapeados para o estado true e false, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="1693a-181">`True` and `False` are literals of the `Boolean` type that map to the true and false state, respectively.</span></span>

```antlr
BooleanLiteral
    : 'True' | 'False'
    ;
```

### <a name="integer-literals"></a><span data-ttu-id="1693a-182">Literais inteiros</span><span class="sxs-lookup"><span data-stu-id="1693a-182">Integer Literals</span></span>

<span data-ttu-id="1693a-183">Literais inteiros podem ser decimal (base 10), hexadecimal (base 16) ou octal (base 8).</span><span class="sxs-lookup"><span data-stu-id="1693a-183">Integer literals can be decimal (base 10), hexadecimal (base 16), or octal (base 8).</span></span> <span data-ttu-id="1693a-184">Um inteiro decimal literal é uma cadeia de caracteres de dígitos decimais (0-9).</span><span class="sxs-lookup"><span data-stu-id="1693a-184">A decimal integer literal is a string of decimal digits (0-9).</span></span> <span data-ttu-id="1693a-185">É um literal hexadecimal `&H` seguido por uma cadeia de caracteres de dígitos hexadecimais (0-9, A-F).</span><span class="sxs-lookup"><span data-stu-id="1693a-185">A hexadecimal literal is `&H` followed by a string of hexadecimal digits (0-9, A-F).</span></span> <span data-ttu-id="1693a-186">É um literal octal `&O` seguido por uma cadeia de caracteres de dígitos octais (0-7).</span><span class="sxs-lookup"><span data-stu-id="1693a-186">An octal literal is `&O` followed by a string of octal digits (0-7).</span></span> <span data-ttu-id="1693a-187">Literais decimais diretamente representam o valor decimal de literal integral, ao passo que literais octais e hexadecimais representam o valor binário do inteiro literal (portanto, `&H8000S` é de -32768, não um erro de estouro).</span><span class="sxs-lookup"><span data-stu-id="1693a-187">Decimal literals directly represent the decimal value of the integral literal, whereas octal and hexadecimal literals represent the binary value of the integer literal (thus, `&H8000S` is -32768, not an overflow error).</span></span>

```antlr
IntegerLiteral
    : IntegralLiteralValue IntegralTypeCharacter?
    ;

IntegralLiteralValue
    : IntLiteral
    | HexLiteral
    | OctalLiteral
    ;

IntegralTypeCharacter
    : ShortCharacter
    | UnsignedShortCharacter
    | IntegerCharacter
    | UnsignedIntegerCharacter
    | LongCharacter
    | UnsignedLongCharacter
    | IntegerTypeCharacter
    | LongTypeCharacter
    ;

ShortCharacter
    : 'S'
    ;

UnsignedShortCharacter
    : 'US'
    ;

IntegerCharacter
    : 'I'
    ;

UnsignedIntegerCharacter
    : 'UI'
    ;

LongCharacter
    : 'L'
    ;

UnsignedLongCharacter
    : 'UL'
    ;

IntLiteral
    : Digit+
    ;

HexLiteral
    : '&' 'H' HexDigit+
    ;

OctalLiteral
    : '&' 'O' OctalDigit+
    ;

Digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

HexDigit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F'
    ;

OctalDigit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7'
    ;
```

<span data-ttu-id="1693a-188">O tipo de um literal é determinado por seu valor ou o caractere de tipo a seguir.</span><span class="sxs-lookup"><span data-stu-id="1693a-188">The type of a literal is determined by its value or by the following type character.</span></span> <span data-ttu-id="1693a-189">Se nenhum caractere de tipo for especificado, os valores no intervalo da `Integer` tipo são digitadas como `Integer`; valores fora do intervalo para `Integer` são digitadas como `Long`.</span><span class="sxs-lookup"><span data-stu-id="1693a-189">If no type character is specified, values in the range of the `Integer` type are typed as `Integer`; values outside the range for `Integer` are typed as `Long`.</span></span> <span data-ttu-id="1693a-190">Se o tipo de um literal inteiro é de tamanho suficiente para manter o inteiro literal, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="1693a-190">If an integer literal's type is of insufficient size to hold the integer literal, a compile-time error results.</span></span> <span data-ttu-id="1693a-191">(__Observação.__</span><span class="sxs-lookup"><span data-stu-id="1693a-191">(__Note.__</span></span> <span data-ttu-id="1693a-192">Não há um caractere de tipo para `Byte` porque o caractere mais natural seria `B`, que é um caractere legal em um literal hexadecimal.)</span><span class="sxs-lookup"><span data-stu-id="1693a-192">There isn't a type character for `Byte` because the most natural character would be `B`, which is a legal character in a hexadecimal literal.)</span></span>


### <a name="floating-point-literals"></a><span data-ttu-id="1693a-193">Literais de ponto flutuantes</span><span class="sxs-lookup"><span data-stu-id="1693a-193">Floating-Point Literals</span></span>

<span data-ttu-id="1693a-194">Um literal de ponto flutuante é um literal de inteiro seguido por um ponto decimal opcional (o caractere de ponto de ASCII) e mantissa e um expoente opcional de base 10.</span><span class="sxs-lookup"><span data-stu-id="1693a-194">A floating-point literal is an integer literal followed by an optional decimal point (the ASCII period character) and mantissa, and an optional base 10 exponent.</span></span> <span data-ttu-id="1693a-195">Por padrão, um literal de ponto flutuante é do tipo `Double`.</span><span class="sxs-lookup"><span data-stu-id="1693a-195">By default, a floating-point literal is of type `Double`.</span></span> <span data-ttu-id="1693a-196">Se o `Single`, `Double`, ou `Decimal` caractere de tipo for especificado, o literal é desse tipo.</span><span class="sxs-lookup"><span data-stu-id="1693a-196">If the `Single`, `Double`, or `Decimal` type character is specified, the literal is of that type.</span></span> <span data-ttu-id="1693a-197">Se o tipo de um literal de ponto flutuante é de tamanho insuficiente para manter o literal de ponto flutuante, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="1693a-197">If a floating-point literal's type is of insufficient size to hold the floating-point literal, a compile-time error results.</span></span>

<span data-ttu-id="1693a-198">__Observação.__</span><span class="sxs-lookup"><span data-stu-id="1693a-198">__Note.__</span></span> <span data-ttu-id="1693a-199">Vale a pena observar que o `Decimal` tipo de dados pode codificar os zeros à direita em um valor.</span><span class="sxs-lookup"><span data-stu-id="1693a-199">It is worth noting that the `Decimal` data type can encode trailing zeros in a value.</span></span> <span data-ttu-id="1693a-200">A especificação não faz no momento, nenhum comentário sobre se zero à direita um `Decimal` literal deve ser aceito por um compilador.</span><span class="sxs-lookup"><span data-stu-id="1693a-200">The specification currently makes no comment about whether trailing zeros in a `Decimal` literal should be honored by a compiler.</span></span>

```antlr
FloatingPointLiteral
    : FloatingPointLiteralValue FloatingPointTypeCharacter?
    | IntLiteral FloatingPointTypeCharacter
    ;

FloatingPointTypeCharacter
    : SingleCharacter
    | DoubleCharacter
    | DecimalCharacter
    | SingleTypeCharacter
    | DoubleTypeCharacter
    | DecimalTypeCharacter
    ;

SingleCharacter
    : 'F'
    ;

DoubleCharacter
    : 'R'
    ;

DecimalCharacter
    : 'D'
    ;

FloatingPointLiteralValue
    : IntLiteral '.' IntLiteral Exponent?
    | '.' IntLiteral Exponent?
    | IntLiteral Exponent
    ;

Exponent
    : 'E' Sign? IntLiteral
    ;

Sign
    : '+'
    | '-'
    ;
```

### <a name="string-literals"></a><span data-ttu-id="1693a-201">Literais da Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="1693a-201">String Literals</span></span>

<span data-ttu-id="1693a-202">Um literal de cadeia de caracteres é uma sequência de Unicode de zero ou mais caracteres de início e terminando com um caractere de aspas duplas ASCII, Unicode ou um caractere de aspas duplas Unicode à esquerda com o botão direito caractere de aspas duplas.</span><span class="sxs-lookup"><span data-stu-id="1693a-202">A string literal is a sequence of zero or more Unicode characters beginning and ending with an ASCII double-quote character, a Unicode left double-quote character, or a Unicode right double-quote character.</span></span> <span data-ttu-id="1693a-203">Dentro de uma cadeia de caracteres, uma sequência de dois caracteres de aspas duplas é uma sequência de escape que representa uma aspa dupla na cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="1693a-203">Within a string, a sequence of two double-quote characters is an escape sequence representing a double quote in the string.</span></span>

```antlr
StringLiteral
    : DoubleQuoteCharacter StringCharacter* DoubleQuoteCharacter
    ;

DoubleQuoteCharacter
    : '"'
    | '<unicode left double-quote 0x201c>'
    | '<unicode right double-quote 0x201D>'
    ;

StringCharacter
    : '<Any character except DoubleQuoteCharacter>'
    | DoubleQuoteCharacter DoubleQuoteCharacter
    ;
```

<span data-ttu-id="1693a-204">Uma constante de cadeia de caracteres é do `String` tipo.</span><span class="sxs-lookup"><span data-stu-id="1693a-204">A string constant is of the `String` type.</span></span>

```vb
Module Test
    Sub Main()

        ' This prints out: ".
        Console.WriteLine("""")

        ' This prints out: a"b.
        Console.WriteLine("a""b")

        ' This causes a compile error due to mismatched double-quotes.
        Console.WriteLine("a"b")
    End Sub
End Module
```

<span data-ttu-id="1693a-205">O compilador tem permissão para substituir uma expressão de cadeia de caracteres constante com uma literal de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="1693a-205">The compiler is allowed to replace a constant string expression with a string literal.</span></span> <span data-ttu-id="1693a-206">Cada cadeia de caracteres literal não resulta necessariamente em uma nova instância de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="1693a-206">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="1693a-207">Quando dois ou mais literais de cadeia de caracteres que são equivalentes de acordo com o operador de igualdade de cadeia de caracteres usando a semântica de comparação binária aparecem no mesmo programa, esses literais de cadeia de caracteres podem se referir à mesma instância de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="1693a-207">When two or more string literals that are equivalent according to the string equality operator using binary comparison semantics appear in the same program, these string literals may refer to the same string instance.</span></span> <span data-ttu-id="1693a-208">Por exemplo, a saída do programa a seguir pode retornar `True` porque os dois literais podem se referir à mesma instância de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="1693a-208">For instance, the output of the following program may return `True` because the two literals may refer to the same string instance.</span></span>

```vb
Module Test
    Sub Main()
        Dim a As Object = "he" & "llo"
        Dim b As Object = "hello"
        Console.WriteLine(a Is b)
    End Sub
End Module
```


### <a name="character-literals"></a><span data-ttu-id="1693a-209">Literais de caracteres</span><span class="sxs-lookup"><span data-stu-id="1693a-209">Character Literals</span></span>

<span data-ttu-id="1693a-210">Um literal de caractere representa um único caractere Unicode do `Char` tipo.</span><span class="sxs-lookup"><span data-stu-id="1693a-210">A character literal represents a single Unicode character of the `Char` type.</span></span> <span data-ttu-id="1693a-211">Dois caracteres de aspas duplas é uma sequência de escape que representa o caractere de aspas duplas.</span><span class="sxs-lookup"><span data-stu-id="1693a-211">Two double-quote characters is an escape sequence representing the double-quote character.</span></span>

```antlr
CharacterLiteral
    : DoubleQuoteCharacter StringCharacter DoubleQuoteCharacter 'C'
    ;
```


```vb
Module Test
    Sub Main()

        ' This prints out: a.
        Console.WriteLine("a"c)

        ' This prints out: ".
        Console.WriteLine(""""c)
    End Sub
End Module
```


### <a name="date-literals"></a><span data-ttu-id="1693a-212">Literais de data</span><span class="sxs-lookup"><span data-stu-id="1693a-212">Date Literals</span></span>

<span data-ttu-id="1693a-213">Um literal de data representa um momento específico no tempo, expressado como um valor da `Date` tipo.</span><span class="sxs-lookup"><span data-stu-id="1693a-213">A date literal represents a particular moment in time expressed as a value of the `Date` type.</span></span>

```antlr
DateLiteral
    : '#' WhiteSpace* DateOrTime WhiteSpace* '#'
    ;

DateOrTime
    : DateValue WhiteSpace+ TimeValue
    | DateValue
    | TimeValue
    ;

DateValue
    : MonthValue '/' DayValue '/' YearValue
    | MonthValue '-' DayValue '-' YearValue
    ;

TimeValue
    : HourValue ':' MinuteValue ( ':' SecondValue )? WhiteSpace* AMPM?
    | HourValue WhiteSpace* AMPM
    ;

MonthValue
    : IntLiteral
    ;

DayValue
    : IntLiteral
    ;

YearValue
    : IntLiteral
    ;

HourValue
    : IntLiteral
    ;

MinuteValue
    : IntLiteral
    ;

SecondValue
    : IntLiteral
    ;

AMPM
    : 'AM' | 'PM'
    ;    
```

<span data-ttu-id="1693a-214">O literal pode especificar uma data e hora, apenas uma data ou apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="1693a-214">The literal may specify both a date and a time, just a date, or just a time.</span></span> <span data-ttu-id="1693a-215">Se o valor de data for omitido, em seguida, em 1º de janeiro do ano 1 no calendário gregoriano será assumido.</span><span class="sxs-lookup"><span data-stu-id="1693a-215">If the date value is omitted, then January 1 of the year 1 in the Gregorian calendar is assumed.</span></span> <span data-ttu-id="1693a-216">Se o valor de hora for omitido, 12:00:00 AM é assumido.</span><span class="sxs-lookup"><span data-stu-id="1693a-216">If the time value is omitted, then 12:00:00 AM is assumed.</span></span>

<span data-ttu-id="1693a-217">Para evitar problemas com a interpretação do valor do ano em um valor de data, o valor de ano não pode ser de dois dígitos.</span><span class="sxs-lookup"><span data-stu-id="1693a-217">To avoid problems with interpreting the year value in a date value, the year value cannot be two digits.</span></span> <span data-ttu-id="1693a-218">Ao expressar uma data no século primeiro AD/CE, zeros à esquerda devem ser especificado.</span><span class="sxs-lookup"><span data-stu-id="1693a-218">When expressing a date in the first century AD/CE, leading zeros must be specified.</span></span>

<span data-ttu-id="1693a-219">Um valor de tempo pode ser especificado usando um valor de 24 horas ou um valor de 12 horas; tempo valores que omitem uma `AM` ou `PM` são considerados valores de 24 horas.</span><span class="sxs-lookup"><span data-stu-id="1693a-219">A time value may be specified either using a 24-hour value or a 12-hour value; time values that omit an `AM` or `PM` are assumed to be 24-hour values.</span></span> <span data-ttu-id="1693a-220">Se um valor de hora omite os minutos, o literal `0` é usado por padrão.</span><span class="sxs-lookup"><span data-stu-id="1693a-220">If a time value omits the minutes, the literal `0` is used by default.</span></span> <span data-ttu-id="1693a-221">Se um valor de hora omite os segundos, o literal `0` é usado por padrão.</span><span class="sxs-lookup"><span data-stu-id="1693a-221">If a time value omits the seconds, the literal `0` is used by default.</span></span> <span data-ttu-id="1693a-222">Se minutos e o segundo forem omitidos, então `AM` ou `PM` deve ser especificado.</span><span class="sxs-lookup"><span data-stu-id="1693a-222">If both minutes and second are omitted, then `AM` or `PM` must be specified.</span></span> <span data-ttu-id="1693a-223">Se o valor de data especificado está fora do intervalo da `Date` digitar, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="1693a-223">If the date value specified is outside the range of the `Date` type, a compile-time error occurs.</span></span>

<span data-ttu-id="1693a-224">O exemplo a seguir contém várias literais de data.</span><span class="sxs-lookup"><span data-stu-id="1693a-224">The following example contains several date literals.</span></span>

```vb
Dim d As Date

d = # 8/23/1970 3:45:39AM #
d = # 8/23/1970 #              ' Date value: 8/23/1970 12:00:00AM.
d = # 3:45:39AM #              ' Date value: 1/1/1 3:45:39AM.
d = # 3:45:39 #                ' Date value: 1/1/1 3:45:39AM.
d = # 13:45:39 #               ' Date value: 1/1/1 1:45:39PM.
d = # 1AM #                    ' Date value: 1/1/1 1:00:00AM.
d = # 13:45:39PM #             ' This date value is not valid.
```


### <a name="nothing"></a><span data-ttu-id="1693a-225">nada</span><span class="sxs-lookup"><span data-stu-id="1693a-225">Nothing</span></span>

<span data-ttu-id="1693a-226">`Nothing` é um literal especial; ele não tem um tipo e pode ser convertido para todos os tipos no sistema de tipos, incluindo parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="1693a-226">`Nothing` is a special literal; it does not have a type and is convertible to all types in the type system, including type parameters.</span></span> <span data-ttu-id="1693a-227">Quando convertido em um tipo específico, ele é o equivalente do valor padrão desse tipo.</span><span class="sxs-lookup"><span data-stu-id="1693a-227">When converted to a particular type, it is the equivalent of the default value of that type.</span></span>

```antlr
Nothing
    : 'Nothing'
    ;
```

## <a name="separators"></a><span data-ttu-id="1693a-228">Separadores</span><span class="sxs-lookup"><span data-stu-id="1693a-228">Separators</span></span>

<span data-ttu-id="1693a-229">Os seguintes caracteres ASCII são separadores:</span><span class="sxs-lookup"><span data-stu-id="1693a-229">The following ASCII characters are separators:</span></span>

```antlr
Separator
    : '(' | ')' | '{' | '}' | '!' | '#' | ',' | '.' | ':' | '?'
    ;
```

## <a name="operator-characters"></a><span data-ttu-id="1693a-230">Caracteres de operador</span><span class="sxs-lookup"><span data-stu-id="1693a-230">Operator Characters</span></span>

<span data-ttu-id="1693a-231">Os seguintes caracteres ASCII ou sequências de caracteres denotam operadores:</span><span class="sxs-lookup"><span data-stu-id="1693a-231">The following ASCII characters or character sequences denote operators:</span></span>

```antlr
Operator
    : '&' | '*' | '+' | '-' | '/' | '\\' | '^' | '<' | '=' | '>'
    ;
```

