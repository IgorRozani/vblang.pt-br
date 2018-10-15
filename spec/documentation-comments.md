# <a name="documentation-comments"></a><span data-ttu-id="524ee-101">Comentários de documentação</span><span class="sxs-lookup"><span data-stu-id="524ee-101">Documentation Comments</span></span>

<span data-ttu-id="524ee-102">Comentários de documentação são especialmente formatados comentários na fonte que podem ser analisados para produzir documentação sobre o código que eles estão conectados.</span><span class="sxs-lookup"><span data-stu-id="524ee-102">Documentation comments are specially formatted comments in the source that can be analyzed to produce documentation about the code they are attached to.</span></span> <span data-ttu-id="524ee-103">O formato básico para comentários da documentação é XML.</span><span class="sxs-lookup"><span data-stu-id="524ee-103">The basic format for documentation comments is XML.</span></span> <span data-ttu-id="524ee-104">Quando o código de compilação com comentários de documentação, o compilador poderá opcionalmente emitir um arquivo XML que representa a soma total de comentários de documentação na fonte.</span><span class="sxs-lookup"><span data-stu-id="524ee-104">When the compiling code with documentation comments, the compiler may optionally emit an XML file that represents the sum total of the documentation comments in the source.</span></span> <span data-ttu-id="524ee-105">Esse arquivo XML, em seguida, pode ser usado por outras ferramentas para produzir documentação impressa ou online.</span><span class="sxs-lookup"><span data-stu-id="524ee-105">This XML file can then be used by other tools to produce printed or online documentation.</span></span>

<span data-ttu-id="524ee-106">Este capítulo descreve os comentários de documento e recomendado marcas XML para usar com comentários de documento.</span><span class="sxs-lookup"><span data-stu-id="524ee-106">This chapter describes document comments and recommended XML tags to use with document comments.</span></span>

## <a name="documentation-comment-format"></a><span data-ttu-id="524ee-107">Formato de comentário de documentação</span><span class="sxs-lookup"><span data-stu-id="524ee-107">Documentation Comment Format</span></span>

<span data-ttu-id="524ee-108">Comentários de documento são comentários especiais que começam com `'''`, três aspas marcas.</span><span class="sxs-lookup"><span data-stu-id="524ee-108">Document comments are special comments that begin with `'''`, three single quote marks.</span></span> <span data-ttu-id="524ee-109">Eles devem preceder imediatamente o tipo (por exemplo, uma classe, delegado ou interface) ou o membro de tipo (por exemplo, um campo, evento, propriedade ou método) que eles documentam.</span><span class="sxs-lookup"><span data-stu-id="524ee-109">They must immediately precede the type (such as a class, delegate, or interface) or type member (such as a field, event, property, or method) that they document.</span></span> <span data-ttu-id="524ee-110">Um comentário de documento em uma declaração de método parcial será substituído pelo comentário de documento em que o método que fornece o corpo, se houver um.</span><span class="sxs-lookup"><span data-stu-id="524ee-110">A document comment on a partial method declaration will be replaced by the document comment on the method that supplies its body, if there is one.</span></span> <span data-ttu-id="524ee-111">Todos os comentários de documento adjacentes são adicionados juntos para produzir um comentário de documento único.</span><span class="sxs-lookup"><span data-stu-id="524ee-111">All adjacent document comments are appended together to produce a single document comment.</span></span> <span data-ttu-id="524ee-112">Se não houver um caractere de espaço em branco posterior a `'''` caracteres, em seguida, esse caractere de espaço em branco não está incluído na concatenação.</span><span class="sxs-lookup"><span data-stu-id="524ee-112">If there is a whitespace character following the `'''` characters, then that whitespace character is not included in the concatenation.</span></span> <span data-ttu-id="524ee-113">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="524ee-113">For example:</span></span>

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
   ''' <remarks>
   ''' Method <c>Draw</c> renders the point.
   ''' </remarks>
   Sub Draw()
   End Sub
End Class
```

<span data-ttu-id="524ee-114">Comentário da documentação deve ser XML bem formado de acordo com http://www.w3.org/TR/REC-xml.</span><span class="sxs-lookup"><span data-stu-id="524ee-114">Documentation comments must be well formed XML according to http://www.w3.org/TR/REC-xml.</span></span> <span data-ttu-id="524ee-115">Se o XML não está bem formado, um aviso será gerado e o arquivo de documentação conterá um comentário dizendo que foi encontrado um erro.</span><span class="sxs-lookup"><span data-stu-id="524ee-115">If the XML is not well formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="524ee-116">Embora os desenvolvedores são livres para criar seu próprio conjunto de marcas, um conjunto recomendado é definido na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="524ee-116">Although developers are free to create their own set of tags, a recommended set is defined in the next section.</span></span> <span data-ttu-id="524ee-117">Algumas das marcas recomendadas têm significado especial:</span><span class="sxs-lookup"><span data-stu-id="524ee-117">Some of the recommended tags have special meanings:</span></span>

* <span data-ttu-id="524ee-118">O `<param>` marca é usada para descrever parâmetros.</span><span class="sxs-lookup"><span data-stu-id="524ee-118">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="524ee-119">O parâmetro especificado por um `<param>` marca deve existir e todos os parâmetros do membro do tipo devem ser descritos no comentário de documentação.</span><span class="sxs-lookup"><span data-stu-id="524ee-119">The parameter specified by a `<param>` tag must exist and all parameters of the type member must be described in the documentation comment.</span></span> <span data-ttu-id="524ee-120">Se uma dessas condições não for verdadeira, o compilador emite um aviso.</span><span class="sxs-lookup"><span data-stu-id="524ee-120">If either condition is not true, the compiler issues a warning.</span></span>

* <span data-ttu-id="524ee-121">O atributo `cref` pode ser anexado a qualquer marca para fornecer uma referência a um elemento de código.</span><span class="sxs-lookup"><span data-stu-id="524ee-121">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="524ee-122">O elemento de código deve existir; em tempo de compilação o compilador substitui o nome com a cadeia de caracteres de ID que representa o membro.</span><span class="sxs-lookup"><span data-stu-id="524ee-122">The code element must exist; at compile-time the compiler replaces the name with the ID string representing the member.</span></span> <span data-ttu-id="524ee-123">Se o elemento de código não existir, o compilador emite um aviso.</span><span class="sxs-lookup"><span data-stu-id="524ee-123">If the code element does not exist, the compiler issues a warning.</span></span> <span data-ttu-id="524ee-124">Ao procurar por um nome descritas em uma `cref` do atributo, a compilador respeita `Imports` instruções que aparecem no arquivo de origem que contém.</span><span class="sxs-lookup"><span data-stu-id="524ee-124">When looking for a name described in a `cref` attribute, the compiler respects `Imports` statements that appear within the containing source file.</span></span>

* <span data-ttu-id="524ee-125">O `<summary>` marca se destina a ser usado por um visualizador de documentação para exibir informações adicionais sobre um tipo ou membro.</span><span class="sxs-lookup"><span data-stu-id="524ee-125">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>

<span data-ttu-id="524ee-126">Observe que o arquivo de documentação fornece informações completas sobre um tipo e membros, apenas o que está contido nos comentários de documento.</span><span class="sxs-lookup"><span data-stu-id="524ee-126">Note that the documentation file does not provide full information about a type and members, only what is contained in the document comments.</span></span> <span data-ttu-id="524ee-127">Para obter mais informações sobre um tipo ou membro, o arquivo de documentação deve ser usado em conjunto com a reflexão no membro ou tipo real.</span><span class="sxs-lookup"><span data-stu-id="524ee-127">To get more information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="524ee-128">Marcações recomendadas</span><span class="sxs-lookup"><span data-stu-id="524ee-128">Recommended tags</span></span>

<span data-ttu-id="524ee-129">O gerador de documentação deve aceitar e processar qualquer marca que seja válida de acordo com as regras do XML.</span><span class="sxs-lookup"><span data-stu-id="524ee-129">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="524ee-130">As seguintes marcas fornecem as funcionalidades geralmente usadas na documentação do usuário:</span><span class="sxs-lookup"><span data-stu-id="524ee-130">The following tags provide commonly used functionality in user documentation:</span></span>

<span data-ttu-id="524ee-131">`<c>` Define o texto em uma fonte de código</span><span class="sxs-lookup"><span data-stu-id="524ee-131">`<c>` Sets text in a code-like font</span></span>

<span data-ttu-id="524ee-132">`<code>` Define uma ou mais linhas de saída de programa ou código de origem em uma fonte de código</span><span class="sxs-lookup"><span data-stu-id="524ee-132">`<code>` Sets one or more lines of source code or program output in a code-like font</span></span>

<span data-ttu-id="524ee-133">`<example>` Indica um exemplo</span><span class="sxs-lookup"><span data-stu-id="524ee-133">`<example>` Indicates an example</span></span>

<span data-ttu-id="524ee-134">`<exception>` Identifica um método pode lançar exceções</span><span class="sxs-lookup"><span data-stu-id="524ee-134">`<exception>` Identifies the exceptions a method can throw</span></span>

<span data-ttu-id="524ee-135">`<include>` Inclui um documento XML externo</span><span class="sxs-lookup"><span data-stu-id="524ee-135">`<include>` Includes an external XML document</span></span>

<span data-ttu-id="524ee-136">`<list>` Cria uma lista ou tabela</span><span class="sxs-lookup"><span data-stu-id="524ee-136">`<list>` Creates a list or table</span></span>

<span data-ttu-id="524ee-137">`<para>` Permite que a estrutura a ser adicionado ao texto</span><span class="sxs-lookup"><span data-stu-id="524ee-137">`<para>` Permits structure to be added to text</span></span>

<span data-ttu-id="524ee-138">`<param>` Descreve um parâmetro para um método ou construtor</span><span class="sxs-lookup"><span data-stu-id="524ee-138">`<param>` Describes a parameter for a method or constructor</span></span>

<span data-ttu-id="524ee-139">`<paramref>` Identifica se uma palavra é um nome de parâmetro</span><span class="sxs-lookup"><span data-stu-id="524ee-139">`<paramref>` Identifies that a word is a parameter name</span></span>

<span data-ttu-id="524ee-140">`<permission>` Documenta a acessibilidade de segurança de membro</span><span class="sxs-lookup"><span data-stu-id="524ee-140">`<permission>` Documents the security accessibility of a member</span></span>

<span data-ttu-id="524ee-141">`<remarks>` Descreve um tipo</span><span class="sxs-lookup"><span data-stu-id="524ee-141">`<remarks>` Describes a type</span></span>

<span data-ttu-id="524ee-142">`<returns>` Descreve o valor de retorno de um método</span><span class="sxs-lookup"><span data-stu-id="524ee-142">`<returns>` Describes the return value of a method</span></span>

<span data-ttu-id="524ee-143">`<see>` Especifica um link</span><span class="sxs-lookup"><span data-stu-id="524ee-143">`<see>` Specifies a link</span></span>

<span data-ttu-id="524ee-144">`<seealso>` Gera uma entrada Consulte também</span><span class="sxs-lookup"><span data-stu-id="524ee-144">`<seealso>` Generates a See Also entry</span></span>

<span data-ttu-id="524ee-145">`<summary>` Descreve um membro de um tipo</span><span class="sxs-lookup"><span data-stu-id="524ee-145">`<summary>` Describes a member of a type</span></span>

<span data-ttu-id="524ee-146">`<typeparam>` Descreve um parâmetro de tipo</span><span class="sxs-lookup"><span data-stu-id="524ee-146">`<typeparam>` Describes a type parameter</span></span>

<span data-ttu-id="524ee-147">`<value>` Descreve uma propriedade</span><span class="sxs-lookup"><span data-stu-id="524ee-147">`<value>` Describes a property</span></span>

### <a name="ltcgt"></a><span data-ttu-id="524ee-148">&lt;c&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-148">&lt;c&gt;</span></span>

<span data-ttu-id="524ee-149">Essa marca Especifica que um fragmento de texto dentro uma descrição deve usar uma fonte semelhante à usada para um bloco de código.</span><span class="sxs-lookup"><span data-stu-id="524ee-149">This tag specifies that a fragment of text within a description should use a font like that used for a block of code.</span></span> <span data-ttu-id="524ee-150">(Para linhas de código real, use `<code>`.)</span><span class="sxs-lookup"><span data-stu-id="524ee-150">(For lines of actual code, use `<code>`.)</span></span>

<span data-ttu-id="524ee-151">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-151">__Syntax:__</span></span>

```xml
<c>text to be set like code</c>
```

<span data-ttu-id="524ee-152">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-152">__Example:__</span></span>

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltcodegt"></a><span data-ttu-id="524ee-153">&lt;Código&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-153">&lt;code&gt;</span></span>

<span data-ttu-id="524ee-154">Essa marca Especifica que uma ou mais linhas de saída de programa ou código de origem devem usar uma fonte de largura fixa.</span><span class="sxs-lookup"><span data-stu-id="524ee-154">This tag specifies that one or more lines of source code or program output should use a fixed-width font.</span></span> <span data-ttu-id="524ee-155">(Para fragmentos de código pequeno, use `<c>`.)</span><span class="sxs-lookup"><span data-stu-id="524ee-155">(For small code fragments, use `<c>`.)</span></span>

<span data-ttu-id="524ee-156">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-156">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="524ee-157">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-157">__Example:__</span></span>

```vb
''' <summary>
''' This method changes the point's location by the given x- and 
''' y-offsets.
''' <example>
''' For example:
''' <code>
'''    Dim p As Point = New Point(3,5)
'''    p.Translate(-1,3)
''' </code>
''' results in <c>p</c>'s having the value (2,8).
''' </example>
''' </summary>
Public Sub Translate(x As Integer, y As Integer)
    Me.x += x
    Me.y += y
End Sub
```

### <a name="ltexamplegt"></a><span data-ttu-id="524ee-158">&lt;example&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-158">&lt;example&gt;</span></span>

<span data-ttu-id="524ee-159">Essa marca permite que o código de exemplo de um comentário para mostrar como um elemento pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="524ee-159">This tag allows example code within a comment to show how an element can be used.</span></span> <span data-ttu-id="524ee-160">Normalmente, isso envolverá o uso da marca `<code>` também.</span><span class="sxs-lookup"><span data-stu-id="524ee-160">Ordinarily, this will involve use of the tag `<code>` as well.</span></span>

<span data-ttu-id="524ee-161">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-161">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="524ee-162">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-162">__Example:__</span></span>

<span data-ttu-id="524ee-163">Para ver um exemplo, consulte `<code>`.</span><span class="sxs-lookup"><span data-stu-id="524ee-163">See `<code>` for an example.</span></span>

### <a name="ltexceptiongt"></a><span data-ttu-id="524ee-164">&lt;exception&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-164">&lt;exception&gt;</span></span>

<span data-ttu-id="524ee-165">Essa marca fornece uma maneira para documentar um método pode lançar exceções.</span><span class="sxs-lookup"><span data-stu-id="524ee-165">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="524ee-166">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-166">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="524ee-167">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-167">__Example:__</span></span>

```vb
Public Module DataBaseOperations
    ''' <exception cref="MasterFileFormatCorruptException"></exception>
    ''' <exception cref="MasterFileLockedOpenException"></exception>
    Public Sub ReadRecord(flag As Integer)
        If Flag = 1 Then
            Throw New MasterFileFormatCorruptException()
        ElseIf Flag = 2 Then
            Throw New MasterFileLockedOpenException()
        End If
        ' ...
    End Sub
End Module
```

### <a name="ltincludegt"></a><span data-ttu-id="524ee-168">&lt;include&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-168">&lt;include&gt;</span></span>

<span data-ttu-id="524ee-169">Essa marca é usada para incluir informações de um documento XML bem formado externo.</span><span class="sxs-lookup"><span data-stu-id="524ee-169">This tag is used to include information from an external well-formed XML document.</span></span> <span data-ttu-id="524ee-170">Uma expressão XPath é aplicada ao documento XML para especificar quais XML deve ser incluído do documento.</span><span class="sxs-lookup"><span data-stu-id="524ee-170">An XPath expression is applied to the XML document to specify what XML should be included from the document.</span></span> <span data-ttu-id="524ee-171">O `<include>` marca, em seguida, é substituída pelo XML selecionado do documento externo.</span><span class="sxs-lookup"><span data-stu-id="524ee-171">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="524ee-172">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-172">__Syntax:__</span></span>

```xml
<include file="filename" path="xpath">
```

<span data-ttu-id="524ee-173">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-173">__Example:__</span></span>

<span data-ttu-id="524ee-174">Se o código-fonte contiver uma declaração semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="524ee-174">If the source code contained a declaration like the following:</span></span>

```vb
''' <include file="docs.xml" path="extra/class[@name="IntList"]/*" />
```

<span data-ttu-id="524ee-175">e o arquivo externo docs.xml tinha o conteúdo a seguir</span><span class="sxs-lookup"><span data-stu-id="524ee-175">and the external file docs.xml had the following contents</span></span>

```xml
<?xml version="1.0"?>
<extra>
    <class name="IntList">
        <summary>
            Contains a list of integers.
        </summary>
    </class>
    <class name="StringList">
        <summary>
            Contains a list of strings.
        </summary>
    </class>
</extra>
```

<span data-ttu-id="524ee-176">em seguida, a documentação do mesma é a saída como se o código-fonte contido:</span><span class="sxs-lookup"><span data-stu-id="524ee-176">then the same documentation is output as if the source code contained:</span></span>

```xml
''' <summary>
''' Contains a list of integers.
''' </summary>
```

### <a name="ltlistgt"></a><span data-ttu-id="524ee-177">&lt;list&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-177">&lt;list&gt;</span></span>

<span data-ttu-id="524ee-178">Essa marca é usada para criar uma lista ou tabela de itens.</span><span class="sxs-lookup"><span data-stu-id="524ee-178">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="524ee-179">Ele pode conter um `<listheader>` bloco para definir a linha de cabeçalho de uma tabela ou uma definição de lista.</span><span class="sxs-lookup"><span data-stu-id="524ee-179">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="524ee-180">(Ao definir uma tabela, apenas uma entrada para o termo no título precisa ser fornecida.)</span><span class="sxs-lookup"><span data-stu-id="524ee-180">(When defining a table, only an entry for term in the heading need be supplied.)</span></span>

<span data-ttu-id="524ee-181">Cada item na lista é especificado com um `<item>` bloco.</span><span class="sxs-lookup"><span data-stu-id="524ee-181">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="524ee-182">Ao criar uma lista de definições, o termo e a descrição devem ser especificados.</span><span class="sxs-lookup"><span data-stu-id="524ee-182">When creating a definition list, both term and description must be specified.</span></span> <span data-ttu-id="524ee-183">No entanto, para uma tabela, lista com marcadores ou lista numerada, a descrição somente precisa ser especificada.</span><span class="sxs-lookup"><span data-stu-id="524ee-183">However, for a table, bulleted list, or numbered list, only description need be specified.</span></span>

<span data-ttu-id="524ee-184">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-184">__Syntax:__</span></span>

```xml
<list type="bullet" | "number" | "table">
    <listheader>
        <term>term</term>
        <description>description</description>
    </listheader>
    <item>
        <term>term</term>
        <description>description</description>
    </item>
    ...
    <item>
        <term>term</term>
        <description>description</description>
    </item>
</list>
```

<span data-ttu-id="524ee-185">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-185">__Example:__</span></span>

```vb
Public Class TestClass
    ''' <remarks>
    ''' Here is an example of a bulleted list:
    ''' <list type="bullet">
    '''     <item>
    '''        <description>Item 1.</description>
    '''     </item>
    '''     <item>
    '''         <description>Item 2.</description>
    '''     </item>
    ''' </list>
    ''' </remarks>
    Public Shared Sub Main()
    End Sub
End Class
```

### <a name="ltparagt"></a><span data-ttu-id="524ee-186">&lt;para&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-186">&lt;para&gt;</span></span>

<span data-ttu-id="524ee-187">Essa marca é para uso dentro de outras marcas, como `<remarks>` ou `<returns>`e permite que a estrutura a ser adicionado ao texto.</span><span class="sxs-lookup"><span data-stu-id="524ee-187">This tag is for use inside other tags, such as `<remarks>` or `<returns>`, and permits structure to be added to text.</span></span>

<span data-ttu-id="524ee-188">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-188">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="524ee-189">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-189">__Example:__</span></span>

```vb
''' <summary>
''' This is the entry point of the Point class testing program.
''' <para>This program tests each method and operator, and
''' is intended to be run after any non-trvial maintenance has
''' been performed on the Point class.</para>
''' </summary>
Public Shared Sub Main()
End Sub
```

### <a name="ltparamgt"></a><span data-ttu-id="524ee-190">&lt;param&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-190">&lt;param&gt;</span></span>

<span data-ttu-id="524ee-191">Essa marca descreve um parâmetro para um método, um construtor ou uma propriedade indexada.</span><span class="sxs-lookup"><span data-stu-id="524ee-191">This tag describes a parameter for a method, constructor, or indexed property.</span></span>

<span data-ttu-id="524ee-192">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-192">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="524ee-193">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-193">__Example:__</span></span>

```vb
''' <summary>
''' This method changes the point's location to the given
''' coordinates.
''' </summary>
''' <param name="x"><c>x</c> is the new x-coordinate.</param>
''' <param name="y"><c>y</c> is the new y-coordinate.</param>
Public Sub Move(x As Integer, y As Integer)
    Me.x = x
    Me.y = y
End Sub
```

### <a name="ltparamrefgt"></a><span data-ttu-id="524ee-194">&lt;paramref&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-194">&lt;paramref&gt;</span></span>

<span data-ttu-id="524ee-195">Essa marca indica que uma palavra é um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="524ee-195">This tag indicates that a word is a parameter.</span></span> <span data-ttu-id="524ee-196">O arquivo de documentação pode ser processado para formatar esse parâmetro de alguma forma distinta.</span><span class="sxs-lookup"><span data-stu-id="524ee-196">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="524ee-197">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-197">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="524ee-198">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-198">__Example:__</span></span>

```vb
''' <summary>
''' This constructor initializes the new Point to
''' (<paramref name="x"/>,<paramref name="y"/>).
''' </summary>
''' <param name="x"><c>x</c> is the new Point's x-coordinate.</param>
''' <param name="y"><c>y</c> is the new Point's y-coordinate.</param>
Public Sub New(x As Integer, y As Integer)
    Me.x = x
    Me.y = y
End Sub
```

### <a name="ltpermissiongt"></a><span data-ttu-id="524ee-199">&lt;permission&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-199">&lt;permission&gt;</span></span>

<span data-ttu-id="524ee-200">Essa marca documenta a acessibilidade de segurança de membro</span><span class="sxs-lookup"><span data-stu-id="524ee-200">This tag documents the security accessibility of a member</span></span>

<span data-ttu-id="524ee-201">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-201">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="524ee-202">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-202">__Example:__</span></span>

```vb
''' <permission cref="System.Security.PermissionSet">Everyone can
''' access this method.</permission>
Public Shared Sub Test()
End Sub
```

### <a name="ltremarksgt"></a><span data-ttu-id="524ee-203">&lt;remarks&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-203">&lt;remarks&gt;</span></span>

<span data-ttu-id="524ee-204">Essa marca Especifica informações gerais sobre um tipo.</span><span class="sxs-lookup"><span data-stu-id="524ee-204">This tag specifies overview information about a type.</span></span> <span data-ttu-id="524ee-205">(Use `<summary>` para descrever os membros de um tipo.)</span><span class="sxs-lookup"><span data-stu-id="524ee-205">(Use `<summary>` to describe the members of a type.)</span></span>

<span data-ttu-id="524ee-206">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-206">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="524ee-207">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-207">__Example:__</span></span>

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltreturnsgt"></a><span data-ttu-id="524ee-208">&lt;returns&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-208">&lt;returns&gt;</span></span>

<span data-ttu-id="524ee-209">Essa marca descreve o valor de retorno de um método.</span><span class="sxs-lookup"><span data-stu-id="524ee-209">This tag describes the return value of a method.</span></span>

<span data-ttu-id="524ee-210">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-210">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="524ee-211">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-211">__Example:__</span></span>

```vb
''' <summary>
''' Report a point's location as a string.
''' </summary>
''' <returns>
''' A string representing a point's location, in the form (x,y), without
''' any leading, training, or embedded whitespace.
''' </returns>
Public Overrides Function ToString() As String
    Return "(" & x & "," & y & ")"
End Sub
```

### <a name="ltseegt"></a><span data-ttu-id="524ee-212">&lt;see&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-212">&lt;see&gt;</span></span>

<span data-ttu-id="524ee-213">Essa marca permite que um link para ser especificado dentro do texto.</span><span class="sxs-lookup"><span data-stu-id="524ee-213">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="524ee-214">(Use `<seealso>` para indicar o texto que deve aparecer em uma seção Consulte também.)</span><span class="sxs-lookup"><span data-stu-id="524ee-214">(Use `<seealso>` to indicate text that is to appear in a See Also section.)</span></span>

<span data-ttu-id="524ee-215">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-215">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="524ee-216">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-216">__Example:__</span></span>

```vb
''' <summary>
''' This method changes the point's location to the given
''' coordinates.
''' </summary>
''' <see cref="Translate"/>
Public Sub Move(x As Integer, y As Integer)
    Me.x = x
    Me.y = y
End Sub

''' <summary>
''' This method changes the point's location by the given x- and
''' y-offsets.
''' </summary>
''' <see cref="Move"/>
Public Sub Translate(x As Integer, y As Integer)
    Me.x += x
    Me.y += y
End Sub
```

### <a name="ltseealsogt"></a><span data-ttu-id="524ee-217">&lt;seealso&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-217">&lt;seealso&gt;</span></span>

<span data-ttu-id="524ee-218">Essa marca gera uma entrada para a seção Consulte também.</span><span class="sxs-lookup"><span data-stu-id="524ee-218">This tag generates an entry for the See Also section.</span></span> <span data-ttu-id="524ee-219">(Use `<see>` para especificar um link de dentro do texto.)</span><span class="sxs-lookup"><span data-stu-id="524ee-219">(Use `<see>` to specify a link from within text.)</span></span>

<span data-ttu-id="524ee-220">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-220">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="524ee-221">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-221">__Example:__</span></span>

```vb
''' <summary>
''' This method determines whether two Points have the same location.
''' </summary>
''' <seealso cref="operator=="/>
''' <seealso cref="operator!="/>
Public Overrides Function Equals(o As Object) As Boolean
    ' ...
End Function
```

### <a name="ltsummarygt"></a><span data-ttu-id="524ee-222">&lt;summary&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-222">&lt;summary&gt;</span></span>

<span data-ttu-id="524ee-223">Essa marca descreve um membro de tipo.</span><span class="sxs-lookup"><span data-stu-id="524ee-223">This tag describes a type member.</span></span> <span data-ttu-id="524ee-224">(Use `<remarks>` para descrever um tipo em si.)</span><span class="sxs-lookup"><span data-stu-id="524ee-224">(Use `<remarks>` to describe a type itself.)</span></span>

<span data-ttu-id="524ee-225">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-225">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="524ee-226">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-226">__Example:__</span></span>

```vb
''' <summary>
''' This constructor initializes the new Point to (0,0).
''' </summary>
Public Sub New()
    Me.New(0,0)
End Sub
```

### <a name="lttypeparamgt"></a><span data-ttu-id="524ee-227">&lt;typeparam&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-227">&lt;typeparam&gt;</span></span>

<span data-ttu-id="524ee-228">Essa marca descreve um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="524ee-228">This tag describes a type parameter.</span></span>

<span data-ttu-id="524ee-229">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-229">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="524ee-230">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-230">__Example:__</span></span>

```vb
''' <typeparam name="T">
''' The base item type. Must implement IComparable.
''' </typeparam>
Public Class ItemManager(Of T As IComparable)
End Class
```

### <a name="ltvaluegt"></a><span data-ttu-id="524ee-231">&lt;value&gt;</span><span class="sxs-lookup"><span data-stu-id="524ee-231">&lt;value&gt;</span></span>

<span data-ttu-id="524ee-232">Essa marca descreve uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="524ee-232">This tag describes a property.</span></span>

<span data-ttu-id="524ee-233">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="524ee-233">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="524ee-234">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="524ee-234">__Example:__</span></span>

```vb
''' <value>
''' Property <c>X</c> represents the point's x-coordinate.
''' </value>
Public Property X() As Integer
    Get
        Return _x
    End Get
    Set (Value As Integer)
        _x = Value
    End Set
End Property
```

## <a name="id-strings"></a><span data-ttu-id="524ee-235">Cadeias de caracteres de ID</span><span class="sxs-lookup"><span data-stu-id="524ee-235">ID Strings</span></span>

<span data-ttu-id="524ee-236">Ao gerar o arquivo de documentação, o compilador gera uma cadeia de caracteres de ID para cada elemento no código-fonte que é marcado com um comentário de documentação que identifica com exclusividade.</span><span class="sxs-lookup"><span data-stu-id="524ee-236">When generating the documentation file, the compiler generates an ID string for each element in the source code that is tagged with a documentation comment that uniquely identifies it.</span></span> <span data-ttu-id="524ee-237">Essa cadeia de caracteres de ID pode ser usada por ferramentas externas para identificar qual elemento em um assembly compilado corresponde para o comentário de documento.</span><span class="sxs-lookup"><span data-stu-id="524ee-237">This ID string can be used by external tools to identify which element in a compiled assembly corresponds to the document comment.</span></span>

<span data-ttu-id="524ee-238">Cadeias de caracteres de identificação são geradas da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="524ee-238">ID strings are generated as follows:</span></span>

<span data-ttu-id="524ee-239">Nenhum espaço em branco é colocado na cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="524ee-239">No white space is placed in the string.</span></span>

<span data-ttu-id="524ee-240">A primeira parte da cadeia de caracteres identifica o tipo de membro documentado, por meio de um único caractere seguido por dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="524ee-240">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="524ee-241">Os seguintes tipos de membros são definidos com o caractere correspondente em parênteses depois dela: campos de eventos (E) (F), métodos, incluindo construtores e operadores (M), N (namespaces), propriedades (P) e tipos (T).</span><span class="sxs-lookup"><span data-stu-id="524ee-241">The following kinds of members are defined, with the corresponding character in parenthesis after it: events (E), fields (F), methods including constructors and operators (M), namespaces (N), properties (P) and types (T).</span></span> <span data-ttu-id="524ee-242">Um ponto de exclamação (!) indica que ocorreu um erro ao gerar a cadeia de caracteres de ID e o restante da cadeia de caracteres fornece informações sobre o erro.</span><span class="sxs-lookup"><span data-stu-id="524ee-242">An exclamation point (!) indicates an error occurred while generating the ID string, and the rest of the string provides information about the error.</span></span>

<span data-ttu-id="524ee-243">A segunda parte da cadeia de caracteres é o nome totalmente qualificado do elemento, começando no namespace global.</span><span class="sxs-lookup"><span data-stu-id="524ee-243">The second part of the string is the fully qualified name of the element, starting at the global namespace.</span></span> <span data-ttu-id="524ee-244">O nome do elemento, seu tipo (s) delimitador e o namespace são separados por pontos.</span><span class="sxs-lookup"><span data-stu-id="524ee-244">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="524ee-245">Se o nome do próprio item tiver pontos, eles serão substituídos pelo sustenido (#).</span><span class="sxs-lookup"><span data-stu-id="524ee-245">If the name of the item itself has periods, they are replaced by the pound sign (#).</span></span> <span data-ttu-id="524ee-246">(Ele é considerado que o elemento não tem esse caractere em seu nome.) O nome de um tipo com parâmetros de tipo termina com um acento grave (') seguido por um número que representa o número de parâmetros de tipo no tipo.</span><span class="sxs-lookup"><span data-stu-id="524ee-246">(It is assumed that no element has this character in its name.) The name of a type with type parameters ends with a backquote (\`) followed by a number that represents the number of type parameters on the type.</span></span> <span data-ttu-id="524ee-247">É importante lembrar que porque tipos aninhados têm acesso aos parâmetros de tipo dos tipos que os contém, tipos aninhados implicitamente contêm parâmetros de tipo de seus tipos de recipientes e esses tipos são contados em seus totais de parâmetro de tipo neste caso.</span><span class="sxs-lookup"><span data-stu-id="524ee-247">It is important to remember that because nested types have access to the type parameters of the types containing them, nested types implicitly contain the type parameters of their containing types, and those types are counted in their type parameter totals in this case.</span></span>

<span data-ttu-id="524ee-248">Para métodos e propriedades com argumentos, da seguinte maneira lista de argumentos entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="524ee-248">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="524ee-249">Para aqueles sem argumentos, os parênteses serão omitidos.</span><span class="sxs-lookup"><span data-stu-id="524ee-249">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="524ee-250">Os argumentos são separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="524ee-250">The arguments are separated by commas.</span></span> <span data-ttu-id="524ee-251">A codificação de cada argumento é o mesmo que uma assinatura da CLI, da seguinte maneira: argumentos são representados por seus nomes totalmente qualificados.</span><span class="sxs-lookup"><span data-stu-id="524ee-251">The encoding of each argument is the same as a CLI signature, as follows: Arguments are represented by their fully qualified name.</span></span> <span data-ttu-id="524ee-252">Por exemplo, `Integer` se torna `System.Int32`, `String` se torna `System.String`, `Object` torna-se `System.Object`e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="524ee-252">For example, `Integer` becomes `System.Int32`, `String` becomes `System.String`, `Object` becomes `System.Object`, and so on.</span></span> <span data-ttu-id="524ee-253">Argumentos tendo o `ByRef` modificador tem um ' @' após seu nome de tipo.</span><span class="sxs-lookup"><span data-stu-id="524ee-253">Arguments having the `ByRef` modifier have a '@' following their type name.</span></span> <span data-ttu-id="524ee-254">Argumentos com o `ByVal`, `Optional` ou `ParamArray` modificador não ter nenhuma anotação especial.</span><span class="sxs-lookup"><span data-stu-id="524ee-254">Arguments having the `ByVal`, `Optional` or `ParamArray` modifier have no special notation.</span></span> <span data-ttu-id="524ee-255">Os argumentos que são matrizes são representados como `[lowerbound:size, ..., lowerbound:size]` em que o número de vírgulas é a classificação - 1, e os limites inferior e o tamanho de cada dimensão, se conhecidos, são representados no formato decimal.</span><span class="sxs-lookup"><span data-stu-id="524ee-255">Arguments that are arrays are represented as `[lowerbound:size, ..., lowerbound:size]` where the number of commas is the rank - 1, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="524ee-256">Se um limite inferior ou o tamanho não for especificado, ele é omitido.</span><span class="sxs-lookup"><span data-stu-id="524ee-256">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="524ee-257">Se o limite e o tamanho inferiores de uma determinada dimensão forem omitidos, o ':' será omitido também.</span><span class="sxs-lookup"><span data-stu-id="524ee-257">If the lower bound and size for a particular dimension are omitted, the ':' is omitted as well.</span></span> <span data-ttu-id="524ee-258">Matrizes de matrizes são representadas por um "`[]`" por nível.</span><span class="sxs-lookup"><span data-stu-id="524ee-258">Arrays of arrays are represented by one "`[]`" per level.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="524ee-259">Exemplos de cadeia de caracteres de ID</span><span class="sxs-lookup"><span data-stu-id="524ee-259">ID string examples</span></span>

<span data-ttu-id="524ee-260">Os exemplos seguintes mostram um fragmento de código do VB, juntamente com a ID de cadeia de caracteres produzido a partir de cada elemento de origem pode ocupar um comentário de documentação:</span><span class="sxs-lookup"><span data-stu-id="524ee-260">The following examples each show a fragment of VB code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

<span data-ttu-id="524ee-261">Tipos são representados usando seus nomes totalmente qualificados.</span><span class="sxs-lookup"><span data-stu-id="524ee-261">Types are represented using their fully qualified name.</span></span>

```vb
Enum Color
    Red
    Blue
    Green
End Enum

Namespace Acme
    Interface IProcess
    End Interface

    Structure ValueType
        ...
    End Structure

    Class Widget
        Public Class NestedClass
        End Class

        Public Interface IMenuItem
        End Interface

        Public Delegate Sub Del(i As Integer)

        Public Enum Direction
            North
            South
            East
            West
        End Enum
    End Class
End Namespace

"T:Color"
"T:Acme.IProcess"
"T:Acme.ValueType"
"T:Acme.Widget"
"T:Acme.Widget.NestedClass"
"T:Acme.Widget.IMenuItem"
"T:Acme.Widget.Del"
"T:Acme.Widget.Direction"
```

<span data-ttu-id="524ee-262">Campos são representados por seus nomes totalmente qualificados.</span><span class="sxs-lookup"><span data-stu-id="524ee-262">Fields are represented by their fully qualified name.</span></span>

```vb
Namespace Acme
    Structure ValueType
        Private total As Integer
    End Structure

    Class Widget
        Public Class NestedClass
            Private value As Integer
        End Class

        Private message As String
        Private Shared defaultColor As Color
        Private Const PI As Double = 3.14159
        Protected ReadOnly monthlyAverage As Double
        Private array1() As Long
        Private array2(,) As Widget
    End Class
End Namespace

"F:Acme.ValueType.total"
"F:Acme.Widget.NestedClass.value"
"F:Acme.Widget.message"
"F:Acme.Widget.defaultColor"
"F:Acme.Widget.PI"
"F:Acme.Widget.monthlyAverage"
"F:Acme.Widget.array1"
"F:Acme.Widget.array2"
```

<span data-ttu-id="524ee-263">Construtores.</span><span class="sxs-lookup"><span data-stu-id="524ee-263">Constructors.</span></span>

```vb
Namespace Acme
    Class Widget
        Shared Sub New()
        End Sub

        Public Sub New()
        End Sub

        Public Sub New(s As String)
        End Sub
    End Class
End Namespace

"M:Acme.Widget.#cctor"
"M:Acme.Widget.#ctor"
"M:Acme.Widget.#ctor(System.String)"
```

<span data-ttu-id="524ee-264">Métodos.</span><span class="sxs-lookup"><span data-stu-id="524ee-264">Methods.</span></span>

```vb
Namespace Acme
    Structure ValueType
        Public Sub M(i As Integer)
        End Sub
    End Structure

    Class Widget
        Public Class NestedClass
            Public Sub M(i As Integer)
            End Sub
        End Class

        Public Shared Sub M0()
        End Sub

        Public Sub M1(c As Char, ByRef f As Float, _
            ByRef v As ValueType)
        End Sub

        Public Sub M2(x1() As Short, x2(,) As Integer, _
            x3()() As Long)
        End Sub

        Public Sub M3(x3()() As Long, x4()(,,) As Widget)
        End Sub

        Public Sub M4(Optional i As Integer = 1)

        Public Sub M5(ParamArray args() As Object)
        End Sub
    End Class
End Namespace

"M:Acme.ValueType.M(System.Int32)"
"M:Acme.Widget.NestedClass.M(System.Int32)"
"M:Acme.Widget.M0"
"M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
"M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
"M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
"M:Acme.Widget.M4(System.Int32)"
"M:Acme.Widget.M5(System.Object[])"
```

<span data-ttu-id="524ee-265">Propriedades.</span><span class="sxs-lookup"><span data-stu-id="524ee-265">Properties.</span></span>

```vb
Namespace Acme
    Class Widget
        Public Property Width() As Integer
            Get
            End Get
            Set (Value As Integer)
            End Set
        End Property

        Public Default Property Item(i As Integer) As Integer
            Get
            End Get
            Set (Value As Integer)
            End Set
        End Property

        Public Default Property Item(s As String, _
            i As Integer) As Integer
            Get
            End Get
            Set (Value As Integer)
            End Set
        End Property
    End Class
End Namespace

"P:Acme.Widget.Width"
"P:Acme.Widget.Item(System.Int32)"
"P:Acme.Widget.Item(System.String,System.Int32)"
```

<span data-ttu-id="524ee-266">Eventos</span><span class="sxs-lookup"><span data-stu-id="524ee-266">Events</span></span>   

```vb
Namespace Acme
    Class Widget
        Public Event AnEvent As EventHandler
        Public Event AnotherEvent()
    End Class
End Namespace

"E:Acme.Widget.AnEvent"
"E:Acme.Widget.AnotherEvent"
```

<span data-ttu-id="524ee-267">Operadores.</span><span class="sxs-lookup"><span data-stu-id="524ee-267">Operators.</span></span>

```vb
Namespace Acme
    Class Widget
        Public Shared Operator +(x As Widget) As Widget
        End Operator

        Public Shared Operator +(x1 As Widget, x2 As Widget) As Widget
        End Operator
    End Class
End Namespace

"M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
"M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
```

<span data-ttu-id="524ee-268">Operadores de conversão têm à direita `~` seguido pelo tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="524ee-268">Conversion operators have a trailing `~` followed by the return type.</span></span>

```vb
Namespace Acme
    Class Widget
        Public Shared Narrowing Operator CType(x As Widget) As _
            Integer
        End Operator

        Public Shared Widening Operator CType(x As Widget) As Long
        End Operator
    End Class
End Namespace

"M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
"M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
```

## <a name="documentation-comments-example"></a><span data-ttu-id="524ee-269">Exemplo de comentários de documentação</span><span class="sxs-lookup"><span data-stu-id="524ee-269">Documentation comments example</span></span>

<span data-ttu-id="524ee-270">O exemplo a seguir mostra o código-fonte de um `Point` classe:</span><span class="sxs-lookup"><span data-stu-id="524ee-270">The following example shows the source code of a `Point` class:</span></span>

```vb
Namespace Graphics
    ''' <remarks>
    ''' Class <c>Point</c> models a point in a two-dimensional
    ''' plane.
    ''' </remarks>
    Public Class Point
        ''' <summary>
        ''' Instance variable <c>x</c> represents the point's x-coordinate.
        ''' </summary>
        Private _x As Integer

        ''' <summary>
        ''' Instance variable <c>y</c> represents the point's y-coordinate.
        ''' </summary>
        Private _y As Integer

        ''' <value>
        ''' Property <c>X</c> represents the point's x-coordinate.
        ''' </value>
        Public Property X() As Integer
            Get
                Return _x
            End Get
            Set(Value As Integer)
                _x = Value
            End Set
        End Property

        ''' <value>
        ''' Property <c>Y</c> represents the point's y-coordinate.
        ''' </value>
        Public Property Y() As Integer
            Get
                Return _y
            End Get
            Set(Value As Integer)
                _y = Value
            End Set
        End Property

        ''' <summary>
        ''' This constructor initializes the new Point to (0,0).
        ''' </summary>
        Public Sub New()
            Me.New(0, 0)
        End Sub

        ''' <summary>
        ''' This constructor initializes the new Point to
        ''' (<paramref name="x"/>,<paramref name="y"/>).
        ''' </summary>
        ''' <param name="x"><c>x</c> is the new Point's
        ''' x-coordinate.</param>
        ''' <param name="y"><c>y</c> is the new Point's
        ''' y-coordinate.</param>
        Public Sub New(x As Integer, y As Integer)
            Me.X = x
            Me.Y = y
        End Sub

        ''' <summary>
        ''' This method changes the point's location to the given
        ''' coordinates.
        ''' </summary>
        ''' <param name="x"><c>x</c> is the new x-coordinate.</param>
        ''' <param name="y"><c>y</c> is the new y-coordinate.</param>
        ''' <see cref="Translate"/>
        Public Sub Move(x As Integer, y As Integer)
            Me.X = x
            Me.Y = y
        End Sub

        ''' <summary>
        ''' This method changes the point's location by the given x- and
        ''' y-offsets.
        ''' <example>
        ''' For example:
        ''' <code>
        '''    Dim p As Point = New Point(3, 5)
        '''    p.Translate(-1, 3)
        ''' </code>
        ''' results in <c>p</c>'s having the value (2,8).
        ''' </example>
        ''' </summary>
        ''' <param name="x"><c>x</c> is the relative x-offset.</param>
        ''' <param name="y"><c>y</c> is the relative y-offset.</param>
        ''' <see cref="Move"/>
        Public Sub Translate(x As Integer, y As Integer)
            Me.X += x
            Me.Y += y
        End Sub

        ''' <summary>
        ''' This method determines whether two Points have the same
        ''' location.
        ''' </summary>
        ''' <param name="o"><c>o</c> is the object to be compared to the
        ''' current object.</param>
        ''' <returns>
        ''' True if the Points have the same location and they have the
        ''' exact same type; otherwise, false.
        ''' </returns>
        ''' <seealso cref="Operator op_Equality"/>
        ''' <seealso cref="Operator op_Inequality"/>
        Public Overrides Function Equals(o As Object) As Boolean
            If o Is Nothing Then
                Return False
            End If
            If o Is Me Then
                Return True
            End If
            If Me.GetType() Is o.GetType() Then
                Dim p As Point = CType(o, Point)
                Return (X = p.X) AndAlso (Y = p.Y)
            End If
            Return False
        End Function

        ''' <summary>
        ''' Report a point's location as a string.
        ''' </summary>
        ''' <returns>
        ''' A string representing a point's location, in the form
        ''' (x,y), without any leading, training, or embedded whitespace.
        ''' </returns>
        Public Overrides Function ToString() As String
            Return "(" & X & "," & Y & ")"
        End Function

        ''' <summary>
        ''' This operator determines whether two Points have the
        ''' same location.
        ''' </summary>
        ''' <param name="p1"><c>p1</c> is the first Point to be compared.
        ''' </param>
        ''' <param name="p2"><c>p2</c> is the second Point to be compared.
        ''' </param>
        ''' <returns>
        ''' True if the Points have the same location and they 
        ''' have the exact same type; otherwise, false.
        ''' </returns>
        ''' <seealso cref="Equals"/>
        ''' <seealso cref="op_Inequality"/>
        Public Shared Operator =(p1 As Point, p2 As Point) As Boolean
            If p1 Is Nothing OrElse p2 Is Nothing Then
                Return False
            End If
            If p1.GetType() Is p2.GetType() Then
                Return (p1.X = p2.X) AndAlso (p1.Y = p2.Y)
            End If
            Return False
        End Operator

        ''' <summary>
        ''' This operator determines whether two Points have the
        ''' same location.
        ''' </summary>
        ''' <param name="p1"><c>p1</c> is the first Point to be comapred.
        ''' </param>
        ''' <param name="p2"><c>p2</c> is the second Point to be compared.
        ''' </param>
        ''' <returns>
        ''' True if the Points do not have the same location and
        ''' the exact same type; otherwise, false.
        ''' </returns>
        ''' <seealso cref="Equals"/>
        ''' <seealso cref="op_Equality"/>
        Public Shared Operator <>(p1 As Point, p2 As Point) As Boolean
            Return Not p1 = p2
        End Operator

        ''' <summary>
        ''' This is the entry point of the Point class testing program.
        ''' <para>This program tests each method and operator, and
        ''' is intended to be run after any non-trvial maintenance has
        ''' been performed on the Point class.</para>
        ''' </summary>
        Public Shared Sub Main()
            ' class test code goes here
        End Sub
    End Class
End Namespace
```

<span data-ttu-id="524ee-271">Aqui está a saída produzida quando é fornecido o código-fonte para a classe `Point`, conforme mostrado acima:</span><span class="sxs-lookup"><span data-stu-id="524ee-271">Here is the output produced when given the source code for class `Point`, shown above:</span></span>

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <remarks>Class <c>Point</c> models a point in a
            two-dimensional plane. </remarks>
        </member>
        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>
        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>
        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
            (0,0).</summary>
        </member>
        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="x"/>,<paramref name="y"/>).</summary>
            <param><c>x</c> is the new Point's x-coordinate.</param>
            <param><c>y</c> is the new Point's y-coordinate.</param>
        </member>
        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>x</c> is the new x-coordinate.</param>
            <param><c>y</c> is the new y-coordinate.</param>
            <see cref=
            "M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>
        <member name=
        "M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by the given
            x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>x</c> is the relative x-offset.</param>
            <param><c>y</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>
        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the
            same location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.</param>
            <returns>True if the Points have the same location and they
            have the exact same type; otherwise, false.</returns>
            <seealso cref=
            "M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"
            />
            <seealso cref=
           "M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"
            />
        </member>
        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y), without any leading, training, or embedded
            whitespace.</returns>
        </member>
        <member name=
        "M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they
            have the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso cref=
           "M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"
            />
        </member>
        <member name=
        "M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso cref=
            "M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"
            />
        </member>
        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trvial maintenance has
            been performed on the Point class.</para>
            </summary>
        </member>
        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>
        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
