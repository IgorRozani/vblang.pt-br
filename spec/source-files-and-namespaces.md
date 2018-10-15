# <a name="source-files-and-namespaces"></a>Arquivos de origem e Namespaces

Um programa Visual Basic consiste em um ou mais arquivos de origem. Quando um programa é compilado, todos os arquivos de origem são processados juntos; Dessa forma, arquivos de origem podem depender entre si, possivelmente em forma circular, sem a necessidade de declaração de encaminhamento. A ordem textual das declarações no texto do programa geralmente é de nenhum significado.

Um arquivo de origem consiste em um conjunto opcional de instruções de opção, instruções de importação e atributos, que são seguidos por um corpo de namespace. Os atributos, que devem ter o `Assembly` ou `Module` modificador, aplicam-se ao .NET assembly ou módulo produzidos pela compilação. O corpo do arquivo de origem funciona como uma declaração de namespace implícitas para o namespace global, que significa que todas as declarações de nível superior de um arquivo de origem são colocadas no namespace global. Por exemplo:

FileA.vb:

```vb
Class A
End Class
```

FileB.vb:

```vb
Class B
End Class
```

Contribuir com os dois arquivos de origem ao namespace global, nesse caso, declarando duas classes com nomes totalmente qualificados `A` e `B`. Porque os dois arquivos de origem contribuir com o mesmo espaço de declaração, teria sido um erro se cada continha uma declaração de um membro com o mesmo nome.

__Observação.__ O ambiente de compilação pode substituir as declarações de namespace no qual um arquivo de origem é colocado implicitamente.

Exceto onde observado, as instruções dentro de um programa Visual Basic podem ser finalizadas por um terminador de linha ou por dois-pontos.

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

## <a name="program-startup-and-termination"></a>Programa de inicialização e encerramento

Inicialização do programa ocorre quando o ambiente de execução executa um método designado, o que é conhecido como o programa *ponto de entrada*. Esse método de ponto de entrada, que deve sempre ser denominado `Main`, devem ser compartilhados, não podem ser contidas em um tipo genérico, não pode ter o modificador async e deve ter uma das seguintes assinaturas:

```vb
Sub Main()
Sub Main(args() As String)
Function Main() As Integer
Function Main(args() As String) As Integer
```

A acessibilidade do método do ponto de entrada é irrelevante. Se um programa contiver mais de um ponto de entrada adequado, o ambiente de compilação deve designar um como o ponto de entrada. Caso contrário, ocorrerá um erro de tempo de compilação. O ambiente de compilação também pode criar um método de ponto de entrada, se ainda não existir.

Quando um programa começa, se o ponto de entrada tem um parâmetro, o argumento fornecido pelo ambiente de execução contém os argumentos de linha de comando para o programa representados como cadeias de caracteres. Se o ponto de entrada tem um tipo de retorno `Integer`, o valor retornado da função é retornado para o ambiente de execução como o resultado do programa.

Em todos os outros aspectos, métodos do ponto de entrada se comportam da mesma maneira que outros métodos. Quando a execução deixar a invocação de método do ponto de entrada feita pelo ambiente de execução, o programa é encerrado.

## <a name="compilation-options"></a>Opções de compilação

Um arquivo de origem pode especificar opções de compilação na fonte de código usando *instruções de opção*.

```antlr
OptionStatement
    : OptionExplicitStatement
    | OptionStrictStatement
    | OptionCompareStatement
    | OptionInferStatement
    ;
```

Uma `Option` instrução aplica-se somente para o arquivo de origem em que aparece e apenas um de cada tipo de `Option` instrução pode aparecer em um arquivo de origem. Por exemplo:

```vb
Option Strict On
Option Compare Text
Option Strict Off    ' Not allowed, Option Strict is already specified.
Option Compare Text  ' Not allowed, Option Compare is already specified.
```

Há quatro opções de compilação: semântica de tipo estrito, semântica de declaração explícita, semântica de comparação e a semântica de inferência de tipo de variável local. Se um arquivo de origem não inclui um determinado `Option` instrução e, em seguida, o ambiente de compilação determina qual conjunto específico de semântica será usado. Também é uma opção de compilação quinta, verificações de estouro de inteiro, que pode ser especificado somente por meio do ambiente de compilação.


### <a name="option-explicit-statement"></a>Instrução Option Explicit

O `Option Explicit` instrução determina se as variáveis locais podem ser declaradas implicitamente. As palavras-chave `On` ou `Off` pode seguir a instrução; se nenhum for especificado, o padrão é `On`. Se nenhuma instrução for especificada em um arquivo, o ambiente de compilação determina qual será usado.

```antlr
OptionExplicitStatement
    : 'Option' 'Explicit' OnOff? StatementTerminator
    ;

OnOff
    : 'On' | 'Off'
    ;
```


__Observação.__ `Explicit` e `Off` não são palavras reservadas.

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 5 ' Valid because Option Explicit is off.
    End Sub
End Module
```

Neste exemplo, a variável local `x` é declarado implicitamente atribuindo a ele. O tipo de `x` é `Object`.


### <a name="option-strict-statement"></a>Instrução Option Strict

O `Option Strict` instrução determina se as conversões e operações em `Object` são regidos pela semântica de tipo estrito ou permissível e se os tipos são digitados implicitamente como `Object` se nenhum `As` cláusula for especificada. A instrução pode ser seguida por palavras-chave `On` ou `Off`; se nenhum for especificado, o padrão é `On`. Se nenhuma instrução for especificada em um arquivo, o ambiente de compilação determina qual será usado.

```antlr
OptionStrictStatement
    : 'Option' 'Strict' OnOff? StatementTerminator
    ;
```

__Observação.__ `Strict` e `Off` não são palavras reservadas.

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

Em semântica estrita, o código a seguir não é permitidos:

* Conversões de estreitamento sem um operador de conversão explícita.

* Associação tardia.

* Operações de tipo `Object` diferente de `TypeOf`... `Is`, `Is`, e `IsNot`.

* Omitindo o `As` cláusula em uma declaração que não tem um tipo inferido.


### <a name="option-compare-statement"></a>Instrução Option Compare

O `Option Compare` instrução determina a semântica de comparações de cadeia de caracteres. Comparações de cadeia de caracteres são realizadas a usar comparações binárias (em que o valor binário do Unicode de cada caractere é comparado) ou comparações de cadeias de texto (em que o significado lexical de cada caractere é comparado usando a cultura atual). Se nenhuma instrução for especificada em um arquivo, o ambiente de compilação controla o tipo de comparação será usado.

```antlr
OptionCompareStatement
    : 'Option' 'Compare' CompareOption StatementTerminator
    ;

CompareOption
    : 'Binary' | 'Text'
    ;
```

__Observação.__ `Compare`, `Binary`, e `Text` não são palavras reservadas.

```vb
Option Compare Text

Module Test
    Sub Main()
        Console.WriteLine("a" = "A")    ' Prints True.
    End Sub
End Module
```

Nesse caso, a comparação de cadeia de caracteres é feita usando uma comparação de texto que ignora diferenças de maiusculas. Se `Option Compare Binary` tivesse sido especificado, então isso seria impressos `False`.


### <a name="integer-overflow-checks"></a>Verificações de estouro de inteiro

Operações de inteiro podem ser verificadas ou não marcadas para as condições de estouro em tempo de execução. Se as condições de estouro forem verificadas e uma operação de inteiro estoura, uma `System.OverflowException` exceção é lançada. Se as condições de estouro não forem verificadas, estouros de operação de inteiro não gerarão uma exceção. O ambiente de compilação determina se essa opção está ativada ou desativada.

### <a name="option-infer-statement"></a>Instrução Option Infer

O `Option Infer` instrução determina se local declarações de variáveis que não têm `As` cláusula ter um tipo inferido ou usar `Object`. A instrução pode ser seguida por palavras-chave `On` ou `Off`; se nenhum for especificado, o padrão é `On`. Se nenhuma instrução for especificada em um arquivo, o ambiente de compilação determina qual será usado.

```antlr
OptionInferStatement
    : 'Option' 'Infer' OnOff? StatementTerminator
    ;
```

__Observação.__ `Infer` e `Off` não são palavras reservadas.

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


## <a name="imports-statement"></a>Instrução Imports

`Imports` instruções de importar os nomes de entidades para um arquivo de origem, permitindo que os nomes de ser referenciados sem qualificação ou importar um namespace para uso em expressões de XML.

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

Dentro de declarações de membro em um arquivo de origem que contém um `Imports` instrução, os tipos contidos no namespace específico pode ser referenciada diretamente, conforme mostrado no exemplo a seguir:

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

Aqui, no arquivo de origem, os membros de tipo do namespace `N1.N2` estão diretamente disponíveis e, portanto, a classe `N3.B` deriva da classe `N1.N2.A`.

`Imports` as instruções devem aparecer após qualquer `Option` instruções, mas antes de qualquer tipo de declarações. O ambiente de compilação também pode definir implícita `Imports` instruções.

`Imports` instruções de disponibilizar os nomes em um arquivo de origem, mas não declaram qualquer coisa no espaço de declaração do namespace global. O escopo dos nomes importado por um `Imports` instrução se estende pelas declarações de membro de namespace contidas no arquivo de origem. O escopo de um `Imports` instrução especificamente não incluir outros `Imports` instruções, nem inclui outros arquivos de origem. `Imports` as instruções não podem fazer referência umas às outras.

Neste exemplo, a última `Imports` instrução é um erro porque ele não é afetado pelo alias de importação do primeiro.

```vb
Imports R1 = N1 ' OK.
Imports R2 = N1.N2 ' OK.
Imports R3 = R1.N2 ' Error: Can't refer to R1.

Namespace N1.N2
End Namespace
```

__Observação.__ Os nomes de namespace ou tipo que aparecem no `Imports` instruções sempre são tratadas como se fossem totalmente qualificados. Ou seja, o identificador mais à esquerda em um nome de namespace ou tipo sempre é resolvido no namespace global e procede do restante da resolução de acordo com as regras de resolução de nome normal. Isso é o único lugar no idioma que se aplica a regra; a regra garante que um nome não pode ser completamente oculto da qualificação. Sem a regra, se um nome no namespace global estava oculto em um arquivo de origem em particular, seria impossível especificar todos os nomes de namespace de uma maneira qualificada.

Neste exemplo, o `Imports` instrução sempre refere-se ao global `System` namespace e não a classe no arquivo de origem.

```vb
Imports System   ' Imports the namespace, not the class.

Class System
End Class
```


### <a name="import-aliases"></a>Aliases de importação

Uma *importação alias* define um alias para um namespace ou tipo.

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

Aqui, no arquivo de origem, `A` é um alias para `N1.N2.A`e, portanto, a classe `N3.B` deriva da classe `N1.N2.A`. O mesmo efeito pode ser obtido pela criação de um alias `R` para `N1.N2` e, em seguida, referenciar `R.A`:

```vb
Imports R = N1.N2

Namespace N3
    Class B
        Inherits R.A
    End Class
End Namespace
```

O identificador de um alias de importação deve ser exclusivo dentro do espaço de declaração de namespace global (não apenas o namespace global declaração no arquivo de origem no qual o alias de importação é definido), mesmo que não declara um nome do namespace global espaço de declaração.

__Observação.__ Declarações em um módulo não introduzem nomes no espaço de declaração do recipiente. Portanto, é válido para uma declaração em um módulo para ter o mesmo nome como um alias de importação, mesmo que o nome da declaração estará acessível no espaço de declaração do recipiente.

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

Aqui, o namespace global já contém um membro `A`, portanto, é um erro para um alias de importação usar esse identificador. Da mesma forma, é um erro para dois ou mais identificadores de importação no arquivo de origem para declarar aliases com o mesmo nome.

Um alias de importação pode criar um alias para qualquer tipo ou namespace. Acessando um namespace ou tipo por meio de um alias produz exatamente o mesmo resultado que acessar o namespace ou tipo por meio de seu nome declarado.

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

Aqui, os nomes `N1.N2.A`, `R1.N2.A`, e `R2.A` são equivalentes e todos se referir à classe cujo nome totalmente qualificado é `N1.N2.A`.

A importação Especifica o nome exato do namespace ou tipo ao qual ele está criando um alias. Isso deve ser o nome totalmente qualificado exato desse namespace ou tipo: não usa as regras normais de resolução de nomes qualificados (que permitam o acesso aos membros de uma classe base através de uma classe derivada por exemplo).

Se um pontos de alias de importação para um tipo ou namespace que não pode ser resolvida por essas regras, em seguida, a instrução de importação é ignorada (e o compilador dá um aviso).

Além disso, a referência não pode ser para um tipo genérico aberto- - todos os tipos genéricos devem ter argumentos de tipo válido fornecidos e todos os argumentos de tipo devem ser resolvível pelas regras acima. Qualquer associação incorreta de um tipo genérico é um erro.

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

O nome do alias de importação de declarações no arquivo de origem podem sombra.

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

No exemplo anterior, a referência à `R.A` na declaração de `B` causa um erro porque `R` refere-se ao `N3.R`, e não `N1.N2`.

Um alias de importação disponibiliza um alias dentro de um arquivo de origem em particular, mas ela não contribui com os novos membros para o espaço de declaração subjacente. Em outras palavras, um alias de importação não é transitivo, mas em vez disso, afeta apenas o arquivo de origem em que ele ocorre.

File1.vb:

```vb
Imports R = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace
```

File2.vb:

```vb
Class B
    Inherits R.A ' Error, R unknown.
End Class
```

No exemplo acima, pois o escopo do alias de importação que introduz `R` só se estende às declarações no arquivo de origem no qual ele está contido, `R` é desconhecido no segundo arquivo de origem.


### <a name="namespace-imports"></a>Importações de Namespace

Um *importação de namespace* importa todos os membros de um namespace ou tipo, permitindo que o identificador de cada membro do namespace ou tipo a ser usado sem qualificação. No caso de tipos, uma importação do namespace só permite o acesso aos membros do tipo compartilhados sem a necessidade de qualificação do nome da classe. Em particular, ele permite que os membros de tipos enumerados sejam usados sem qualificação.

```antlr
MembersImportsClause
    : TypeName
    ;
```

Por exemplo:

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

Ao contrário de um alias de importação, uma importação de namespace não tem restrições nos nomes importa e pode importar namespaces e tipos cujos identificadores já são declarados no namespace global. Os nomes importados por uma importação regular são sombreados por aliases de importação e de declarações no arquivo de origem.

No exemplo a seguir `A` refere-se ao `N3.A` vez `N1.N2.A` dentro de declarações de membro no `N3` namespace.

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

Quando mais de um namespace importado contém membros com o mesmo nome (e esse nome não for sombreada por um alias de importação ou a declaração), uma referência a esse nome é ambígua e causa um erro de tempo de compilação.

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

No exemplo acima, ambos `N1` e `N2` conter um membro `A`. Porque `N3` importa ambos, referenciando `A` em `N3` causa um erro de tempo de compilação. Nessa situação, o conflito pode ser resolvido por meio da qualificação de referências a `A`, ou com a introdução de um alias de importação que escolhe um determinado `A`, conforme mostrado no exemplo a seguir:

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

Somente namespaces, classes, estruturas, os tipos enumerados e módulos padrão podem ser importados.


### <a name="xml-namespace-imports"></a>Importações de Namespace XML

Uma *importação do namespace XML* define um namespace ou o namespace padrão para expressões não qualificados do XML contidos dentro da unidade de compilação.

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

Por exemplo:

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

Um namespace XML, incluindo o namespace padrão, só pode ser definido uma vez para um conjunto específico de importações. Por exemplo:

```vb
Imports <xmlns:db="http://example.org/database-one">
' Error: namespace db is already defined
Imports <xmlns:db="http://example.org/database-two">
```


## <a name="namespaces"></a>Namespaces

Programas em Visual Basic são organizados com o uso de namespaces. Namespaces tanto internamente organizar um programa como organizar a maneira como os elementos do programa são expostos a outros programas.

Ao contrário de outras entidades, os namespaces são abertas e pode ser declaradas várias vezes dentro do mesmo programa e, em muitos programas, com cada declaração contribuindo com membros para o mesmo namespace. No exemplo a seguir, as duas declarações de namespace contribuem para o mesmo espaço de declaração, declarando duas classes com nomes totalmente qualificados `N1.N2.A` e `N1.N2.B`.

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

Porque as duas declarações a contribuir com o mesmo espaço de declaração, seria um erro se cada continha uma declaração de um membro com o mesmo nome.

Há um namespace global que tem a nenhum nome e cujos namespaces aninhados e tipos sempre podem ser acessados sem qualificação. O escopo de um membro de namespace declarado no namespace global é o texto de programa inteiro. Caso contrário, o escopo de um tipo ou namespace declarado em um namespace cujo nome totalmente qualificado é `N` é o texto do programa de cada namespace do nome totalmente qualificado do namespace cujo correspondente começa com `N` ou é `N` em si. (Observe que um compilador pode optar por colocar as declarações em um namespace específico, por padrão. Isso não altera o fato de que ainda há um namespace global e sem nome.)

Neste exemplo, a classe `B` pode ver a classe `A` porque `B`do namespace `N1.N2.N3` conceitualmente está aninhado dentro do namespace `N1.N2`.

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

### <a name="namespace-declarations"></a>Declarações de namespace

Há três formas de declaração de namespace.

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

O primeiro formulário começa com a palavra-chave `Namespace` seguido por um nome de namespace relacionado. Se o nome do namespace relativo for qualificado, a declaração de namespace é tratada como se ele lexicalmente é aninhado dentro de declarações de namespace correspondente a cada nome no nome qualificado. Por exemplo, os namespaces a seguir são semanticamente equivalentes:

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

O segundo formulário começa com as palavras-chave `Namespace Global`. Ele é tratado como se todas as suas declarações de membro lexicalmente foram colocadas no namespace sem nome global – independentemente de qualquer padrão fornecido pelo ambiente de compilação. Essa forma de declaração de namespace não pode ser lexicalmente aninhada dentro de qualquer outra declaração de namespace.

O terceiro formulário começa com as palavras-chave `Namespace Global` seguido por um identificador qualificado `N`. Ele é tratado como se fosse uma declaração de namespace do primeiro formulário "`Namespace N`" lexicalmente, que foi colocado no namespace sem nome global – independentemente de qualquer padrão fornecido pelo ambiente de compilação. Essa forma de declaração de namespace não pode ser lexicalmente aninhada dentro de qualquer outra declaração de namespace.

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

Ao lidar com os membros de um namespace, não é importante saber onde um determinado membro é declarado. Se os dois programas de definem uma entidade com o mesmo nome no mesmo namespace, a tentativa de resolver o nome do namespace causa um erro de ambiguidade.

Namespaces são por definição `Public`, portanto, uma declaração de namespace não pode incluir qualquer modificador de acesso.


### <a name="namespace-members"></a>Membros do Namespace

Os membros do Namespace só podem ser declarações de namespace e as declarações de tipo. Declarações de tipo podem ter `Public` ou `Friend` acesso. O acesso padrão para tipos é `Friend` acesso.

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
