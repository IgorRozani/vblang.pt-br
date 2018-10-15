# <a name="documentation-comments"></a>Comentários de documentação

Comentários de documentação são especialmente formatados comentários na fonte que podem ser analisados para produzir documentação sobre o código que eles estão conectados. O formato básico para comentários da documentação é XML. Quando o código de compilação com comentários de documentação, o compilador poderá opcionalmente emitir um arquivo XML que representa a soma total de comentários de documentação na fonte. Esse arquivo XML, em seguida, pode ser usado por outras ferramentas para produzir documentação impressa ou online.

Este capítulo descreve os comentários de documento e recomendado marcas XML para usar com comentários de documento.

## <a name="documentation-comment-format"></a>Formato de comentário de documentação

Comentários de documento são comentários especiais que começam com `'''`, três aspas marcas. Eles devem preceder imediatamente o tipo (por exemplo, uma classe, delegado ou interface) ou o membro de tipo (por exemplo, um campo, evento, propriedade ou método) que eles documentam. Um comentário de documento em uma declaração de método parcial será substituído pelo comentário de documento em que o método que fornece o corpo, se houver um. Todos os comentários de documento adjacentes são adicionados juntos para produzir um comentário de documento único. Se não houver um caractere de espaço em branco posterior a `'''` caracteres, em seguida, esse caractere de espaço em branco não está incluído na concatenação. Por exemplo:

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

Comentário da documentação deve ser XML bem formado de acordo com http://www.w3.org/TR/REC-xml. Se o XML não está bem formado, um aviso será gerado e o arquivo de documentação conterá um comentário dizendo que foi encontrado um erro.

Embora os desenvolvedores são livres para criar seu próprio conjunto de marcas, um conjunto recomendado é definido na próxima seção. Algumas das marcas recomendadas têm significado especial:

* O `<param>` marca é usada para descrever parâmetros. O parâmetro especificado por um `<param>` marca deve existir e todos os parâmetros do membro do tipo devem ser descritos no comentário de documentação. Se uma dessas condições não for verdadeira, o compilador emite um aviso.

* O atributo `cref` pode ser anexado a qualquer marca para fornecer uma referência a um elemento de código. O elemento de código deve existir; em tempo de compilação o compilador substitui o nome com a cadeia de caracteres de ID que representa o membro. Se o elemento de código não existir, o compilador emite um aviso. Ao procurar por um nome descritas em uma `cref` do atributo, a compilador respeita `Imports` instruções que aparecem no arquivo de origem que contém.

* O `<summary>` marca se destina a ser usado por um visualizador de documentação para exibir informações adicionais sobre um tipo ou membro.

Observe que o arquivo de documentação fornece informações completas sobre um tipo e membros, apenas o que está contido nos comentários de documento. Para obter mais informações sobre um tipo ou membro, o arquivo de documentação deve ser usado em conjunto com a reflexão no membro ou tipo real.

## <a name="recommended-tags"></a>Marcações recomendadas

O gerador de documentação deve aceitar e processar qualquer marca que seja válida de acordo com as regras do XML. As seguintes marcas fornecem as funcionalidades geralmente usadas na documentação do usuário:

`<c>` Define o texto em uma fonte de código

`<code>` Define uma ou mais linhas de saída de programa ou código de origem em uma fonte de código

`<example>` Indica um exemplo

`<exception>` Identifica um método pode lançar exceções

`<include>` Inclui um documento XML externo

`<list>` Cria uma lista ou tabela

`<para>` Permite que a estrutura a ser adicionado ao texto

`<param>` Descreve um parâmetro para um método ou construtor

`<paramref>` Identifica se uma palavra é um nome de parâmetro

`<permission>` Documenta a acessibilidade de segurança de membro

`<remarks>` Descreve um tipo

`<returns>` Descreve o valor de retorno de um método

`<see>` Especifica um link

`<seealso>` Gera uma entrada Consulte também

`<summary>` Descreve um membro de um tipo

`<typeparam>` Descreve um parâmetro de tipo

`<value>` Descreve uma propriedade

### <a name="ltcgt"></a>&lt;c&gt;

Essa marca Especifica que um fragmento de texto dentro uma descrição deve usar uma fonte semelhante à usada para um bloco de código. (Para linhas de código real, use `<code>`.)

__Sintaxe:__

```xml
<c>text to be set like code</c>
```

__Exemplo:__

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltcodegt"></a>&lt;Código&gt;

Essa marca Especifica que uma ou mais linhas de saída de programa ou código de origem devem usar uma fonte de largura fixa. (Para fragmentos de código pequeno, use `<c>`.)

__Sintaxe:__

```xml
<code>source code or program output</code>
```

__Exemplo:__

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

### <a name="ltexamplegt"></a>&lt;example&gt;

Essa marca permite que o código de exemplo de um comentário para mostrar como um elemento pode ser usado. Normalmente, isso envolverá o uso da marca `<code>` também.

__Sintaxe:__

```xml
<example>description</example>
```

__Exemplo:__

Para ver um exemplo, consulte `<code>`.

### <a name="ltexceptiongt"></a>&lt;exception&gt;

Essa marca fornece uma maneira para documentar um método pode lançar exceções.

__Sintaxe:__

```xml
<exception cref="member">description</exception>
```

__Exemplo:__

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

### <a name="ltincludegt"></a>&lt;include&gt;

Essa marca é usada para incluir informações de um documento XML bem formado externo. Uma expressão XPath é aplicada ao documento XML para especificar quais XML deve ser incluído do documento. O `<include>` marca, em seguida, é substituída pelo XML selecionado do documento externo.

__Sintaxe:__

```xml
<include file="filename" path="xpath">
```

__Exemplo:__

Se o código-fonte contiver uma declaração semelhante ao seguinte:

```vb
''' <include file="docs.xml" path="extra/class[@name="IntList"]/*" />
```

e o arquivo externo docs.xml tinha o conteúdo a seguir

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

em seguida, a documentação do mesma é a saída como se o código-fonte contido:

```xml
''' <summary>
''' Contains a list of integers.
''' </summary>
```

### <a name="ltlistgt"></a>&lt;list&gt;

Essa marca é usada para criar uma lista ou tabela de itens. Ele pode conter um `<listheader>` bloco para definir a linha de cabeçalho de uma tabela ou uma definição de lista. (Ao definir uma tabela, apenas uma entrada para o termo no título precisa ser fornecida.)

Cada item na lista é especificado com um `<item>` bloco. Ao criar uma lista de definições, o termo e a descrição devem ser especificados. No entanto, para uma tabela, lista com marcadores ou lista numerada, a descrição somente precisa ser especificada.

__Sintaxe:__

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

__Exemplo:__

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

### <a name="ltparagt"></a>&lt;para&gt;

Essa marca é para uso dentro de outras marcas, como `<remarks>` ou `<returns>`e permite que a estrutura a ser adicionado ao texto.

__Sintaxe:__

```xml
<para>content</para>
```

__Exemplo:__

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

### <a name="ltparamgt"></a>&lt;param&gt;

Essa marca descreve um parâmetro para um método, um construtor ou uma propriedade indexada.

__Sintaxe:__

```xml
<param name="name">description</param>
```

__Exemplo:__

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

### <a name="ltparamrefgt"></a>&lt;paramref&gt;

Essa marca indica que uma palavra é um parâmetro. O arquivo de documentação pode ser processado para formatar esse parâmetro de alguma forma distinta.

__Sintaxe:__

```xml
<paramref name="name"/>
```

__Exemplo:__

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

### <a name="ltpermissiongt"></a>&lt;permission&gt;

Essa marca documenta a acessibilidade de segurança de membro

__Sintaxe:__

```xml
<permission cref="member">description</permission>
```

__Exemplo:__

```vb
''' <permission cref="System.Security.PermissionSet">Everyone can
''' access this method.</permission>
Public Shared Sub Test()
End Sub
```

### <a name="ltremarksgt"></a>&lt;remarks&gt;

Essa marca Especifica informações gerais sobre um tipo. (Use `<summary>` para descrever os membros de um tipo.)

__Sintaxe:__

```xml
<remarks>description</remarks>
```

__Exemplo:__

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltreturnsgt"></a>&lt;returns&gt;

Essa marca descreve o valor de retorno de um método.

__Sintaxe:__

```xml
<returns>description</returns>
```

__Exemplo:__

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

### <a name="ltseegt"></a>&lt;see&gt;

Essa marca permite que um link para ser especificado dentro do texto. (Use `<seealso>` para indicar o texto que deve aparecer em uma seção Consulte também.)

__Sintaxe:__

```xml
<see cref="member"/>
```

__Exemplo:__

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

### <a name="ltseealsogt"></a>&lt;seealso&gt;

Essa marca gera uma entrada para a seção Consulte também. (Use `<see>` para especificar um link de dentro do texto.)

__Sintaxe:__

```xml
<seealso cref="member"/>
```

__Exemplo:__

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

### <a name="ltsummarygt"></a>&lt;summary&gt;

Essa marca descreve um membro de tipo. (Use `<remarks>` para descrever um tipo em si.)

__Sintaxe:__

```xml
<summary>description</summary>
```

__Exemplo:__

```vb
''' <summary>
''' This constructor initializes the new Point to (0,0).
''' </summary>
Public Sub New()
    Me.New(0,0)
End Sub
```

### <a name="lttypeparamgt"></a>&lt;typeparam&gt;

Essa marca descreve um parâmetro de tipo.

__Sintaxe:__

```xml
<typeparam name="name">description</typeparam>
```

__Exemplo:__

```vb
''' <typeparam name="T">
''' The base item type. Must implement IComparable.
''' </typeparam>
Public Class ItemManager(Of T As IComparable)
End Class
```

### <a name="ltvaluegt"></a>&lt;value&gt;

Essa marca descreve uma propriedade.

__Sintaxe:__

```xml
<value>property description</value>
```

__Exemplo:__

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

## <a name="id-strings"></a>Cadeias de caracteres de ID

Ao gerar o arquivo de documentação, o compilador gera uma cadeia de caracteres de ID para cada elemento no código-fonte que é marcado com um comentário de documentação que identifica com exclusividade. Essa cadeia de caracteres de ID pode ser usada por ferramentas externas para identificar qual elemento em um assembly compilado corresponde para o comentário de documento.

Cadeias de caracteres de identificação são geradas da seguinte maneira:

Nenhum espaço em branco é colocado na cadeia de caracteres.

A primeira parte da cadeia de caracteres identifica o tipo de membro documentado, por meio de um único caractere seguido por dois-pontos. Os seguintes tipos de membros são definidos com o caractere correspondente em parênteses depois dela: campos de eventos (E) (F), métodos, incluindo construtores e operadores (M), N (namespaces), propriedades (P) e tipos (T). Um ponto de exclamação (!) indica que ocorreu um erro ao gerar a cadeia de caracteres de ID e o restante da cadeia de caracteres fornece informações sobre o erro.

A segunda parte da cadeia de caracteres é o nome totalmente qualificado do elemento, começando no namespace global. O nome do elemento, seu tipo (s) delimitador e o namespace são separados por pontos. Se o nome do próprio item tiver pontos, eles serão substituídos pelo sustenido (#). (Ele é considerado que o elemento não tem esse caractere em seu nome.) O nome de um tipo com parâmetros de tipo termina com um acento grave (') seguido por um número que representa o número de parâmetros de tipo no tipo. É importante lembrar que porque tipos aninhados têm acesso aos parâmetros de tipo dos tipos que os contém, tipos aninhados implicitamente contêm parâmetros de tipo de seus tipos de recipientes e esses tipos são contados em seus totais de parâmetro de tipo neste caso.

Para métodos e propriedades com argumentos, da seguinte maneira lista de argumentos entre parênteses. Para aqueles sem argumentos, os parênteses serão omitidos. Os argumentos são separados por vírgulas. A codificação de cada argumento é o mesmo que uma assinatura da CLI, da seguinte maneira: argumentos são representados por seus nomes totalmente qualificados. Por exemplo, `Integer` se torna `System.Int32`, `String` se torna `System.String`, `Object` torna-se `System.Object`e assim por diante. Argumentos tendo o `ByRef` modificador tem um ' @' após seu nome de tipo. Argumentos com o `ByVal`, `Optional` ou `ParamArray` modificador não ter nenhuma anotação especial. Os argumentos que são matrizes são representados como `[lowerbound:size, ..., lowerbound:size]` em que o número de vírgulas é a classificação - 1, e os limites inferior e o tamanho de cada dimensão, se conhecidos, são representados no formato decimal. Se um limite inferior ou o tamanho não for especificado, ele é omitido. Se o limite e o tamanho inferiores de uma determinada dimensão forem omitidos, o ':' será omitido também. Matrizes de matrizes são representadas por um "`[]`" por nível.

### <a name="id-string-examples"></a>Exemplos de cadeia de caracteres de ID

Os exemplos seguintes mostram um fragmento de código do VB, juntamente com a ID de cadeia de caracteres produzido a partir de cada elemento de origem pode ocupar um comentário de documentação:

Tipos são representados usando seus nomes totalmente qualificados.

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

Campos são representados por seus nomes totalmente qualificados.

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

Construtores.

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

Métodos.

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

Propriedades.

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

Eventos   

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

Operadores.

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

Operadores de conversão têm à direita `~` seguido pelo tipo de retorno.

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

## <a name="documentation-comments-example"></a>Exemplo de comentários de documentação

O exemplo a seguir mostra o código-fonte de um `Point` classe:

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

Aqui está a saída produzida quando é fornecido o código-fonte para a classe `Point`, conforme mostrado acima:

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
