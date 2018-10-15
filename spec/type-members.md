# <a name="type-members"></a>Membros de tipos

Membros de tipo definem locais de armazenamento e o código executável. Eles podem ser métodos, construtores, constantes, variáveis, propriedades e eventos.

## <a name="interface-method-implementation"></a>Implementação do método de interface

Métodos, eventos e propriedades podem implementar membros de interface. Para implementar um membro de interface, uma declaração de membro Especifica o `Implements` palavra-chave e um ou mais membros de interface de lista.

```antlr
ImplementsClause
    : ( 'Implements' ImplementsList )?
    ;

ImplementsList
    : InterfaceMemberSpecifier ( Comma InterfaceMemberSpecifier )*
    ;

InterfaceMemberSpecifier
    : NonArrayTypeName Period IdentifierOrKeyword
    ;
```

Métodos e propriedades que implementam membros de interface são implicitamente `NotOverridable` , a menos que declarado como `MustOverride`, `Overridable`, ou a substituição de outro membro. É um erro para um membro de implementação de um membro de interface para ser `Shared`. Acessibilidade de um membro não tem nenhum efeito em sua capacidade de implementar membros de interface.

Para uma implementação de interface seja válida, lista do tipo recipiente implementa deve nomear uma interface que contém um membro compatível. Um membro compatível é um cuja assinatura coincide com a assinatura do membro implementado. Se uma interface genérica está sendo implementada, o argumento de tipo fornecido na cláusula Implements é substituído na assinatura durante a verificação de compatibilidade. Por exemplo:

```vb
Interface I1(Of T)
    Sub F(x As T)
End Interface

Class C1
    Implements I1(Of Integer)

    Sub F(x As Integer) Implements I1(Of Integer).F
    End Sub
End Class

Class C2(Of U)
    Implements I1(Of U)

    Sub F(x As U) Implements I1(Of U).F
    End Sub
End Class
```

Se um evento declarado usando um tipo de delegado é implementar um evento de interface, um evento compatível é um cujo tipo delegado subjacente é do mesmo tipo. Caso contrário, o evento usa o tipo de delegado do evento de interface que está implementando. Se tal evento implementa vários eventos de interface, todos os eventos de interface devem ter o mesmo tipo delegado subjacente. Por exemplo:

```vb
Interface ClickEvents
    Event LeftClick(x As Integer, y As Integer)
    Event RightClick(x As Integer, y As Integer)
End Interface

Class Button
    Implements ClickEvents

    ' OK. Signatures match, delegate type = ClickEvents.LeftClickHandler.
    Event LeftClick(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick

    ' OK. Signatures match, delegate type = ClickEvents.RightClickHandler.
    Event RightClick(x As Integer, y As Integer) _
        Implements ClickEvents.RightClick
End Class

Class Label
    Implements ClickEvents

    ' Error. Signatures match, but can't be both delegate types.
    Event Click(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick, ClickEvents.RightClick
End Class
```

Um membro de interface na lista implementa é especificado usando um nome de tipo, um período e um identificador. O nome do tipo deve ser uma interface na lista implementa ou uma interface na lista implementa uma interface de base e o identificador deve ser um membro da interface especificada. Um único membro pode implementar mais de um membro de interface correspondente.

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Class Test
    Implements ILeft, IRight

    Sub F() Implements ILeft.F, IRight.F
    End Sub
End Class
```

Se o membro de interface que está sendo implementado não está disponível em todas as interfaces explicitamente implementadas devido a várias heranças de interface, membro implementado deve referenciar explicitamente uma interface de base no qual o membro está disponível. Por exemplo, se `I1` e `I2` conter um membro `M`, e `I3` herda `I1` e `I2`, um tipo que implementa `I3` implementará `I1.M` e `I2.M`. Se um sombras interface multiplicam membros herdados, terá um tipo de implementação implementar os membros herdados e os membros sombreamento-los.

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight

    Shadows Sub F()
End Interface

Class Test
    Implements ILeftRight

    Sub LeftF() Implements ILeft.F
    End Sub

    Sub RightF() Implements IRight.F
    End Sub

    Sub LeftRightF() Implements ILeftRight.F
    End Sub
End Class
```

Se a interface que contém o membro de interface ser implementada é genérico, os mesmos argumentos de tipo deve ser fornecida a interface que está sendo implementa. Por exemplo:

```vb
Interface I1(Of T)
    Function F() As T
End Interface

Class C1
    Implements I1(Of Integer)
    Implements I1(Of Double)

    Function F1() As Integer Implements I1(Of Integer).F
    End Function

    Function F2() As Double Implements I1(Of Double).F
    End Function

    ' Error: I1(Of String) is not implemented by C1
    Function F3() As String Implements I1(Of String).F
    End Function
End Class

Class C2(Of U)
    Implements I1(Of U)

    Function F() As U Implements I1(Of U).F
    End Function
End Class
```


## <a name="methods"></a>Métodos

Métodos contêm as declarações de um programa executáveis.

```antlr
MethodMemberDeclaration
    : MethodDeclaration
    | ExternalMethodDeclaration
    ;

InterfaceMethodMemberDeclaration
    : InterfaceMethodDeclaration
    ;

MethodDeclaration
    : SubDeclaration
    | MustOverrideSubDeclaration
    | FunctionDeclaration
    | MustOverrideFunctionDeclaration
    ;

InterfaceMethodDeclaration
    : InterfaceSubDeclaration
    | InterfaceFunctionDeclaration
    ;

SubSignature
    : 'Sub' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
    ;

FunctionSignature
    : 'Function' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

SubDeclaration
    : Attributes? ProcedureModifier* SubSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Sub' StatementTerminator
    ;

MustOverrideSubDeclaration
    : Attributes? MustOverrideProcedureModifier+ SubSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceSubDeclaration
    : Attributes? InterfaceProcedureModifier* SubSignature StatementTerminator
    ;

FunctionDeclaration
    : Attributes? ProcedureModifier* FunctionSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Function' StatementTerminator
    ;

MustOverrideFunctionDeclaration
    : Attributes? MustOverrideProcedureModifier+ FunctionSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceFunctionDeclaration
    : Attributes? InterfaceProcedureModifier* FunctionSignature StatementTerminator
    ;

ProcedureModifier
    : AccessModifier | 'Shadows' | 'Shared' | 'Overridable' | 'NotOverridable' | 'Overrides'
    | 'Overloads' | 'Partial' | 'Iterator' | 'Async'
    ;

MustOverrideProcedureModifier
    : ProcedureModifier
    | 'MustOverride'
    ;

InterfaceProcedureModifier
    : 'Shadows' | 'Overloads'
    ;

HandlesOrImplements
    : HandlesClause
    | ImplementsClause
    ;
```

Métodos, que tem uma lista opcional de parâmetros e um valor de retorno opcional, são compartilhados ou não compartilhados. Métodos compartilhados são acessados por meio de instâncias da classe ou classe. Métodos não compartilhados, também chamados de métodos de instância, são acessados por meio de instâncias da classe. O exemplo a seguir mostra uma classe `Stack` que tem vários métodos compartilhados (`Clone` e `Flip`), e vários métodos de instância (`Push`, `Pop`, e `ToString`):

```vb
Public Class Stack
    Public Shared Function Clone(s As Stack) As Stack
        ...
    End Function

    Public Shared Function Flip(s As Stack) As Stack
        ...
    End Function

    Public Function Pop() As Object
        ...
    End Function

    Public Sub Push(o As Object)
        ...
    End Sub 

    Public Overrides Function ToString() As String
        ...
    End Function 
End Class 

Module Test
    Sub Main()
        Dim s As Stack = New Stack()
        Dim i As Integer

        While i < 10
            s.Push(i)
        End While

        Dim flipped As Stack = Stack.Flip(s)
        Dim cloned As Stack = Stack.Clone(s)

        Console.WriteLine("Original stack: " & s.ToString())
        Console.WriteLine("Flipped stack: " & flipped.ToString())
        Console.WriteLine("Cloned stack: " & cloned.ToString())
    End Sub
End Module
```

Métodos podem ser sobrecarregados, o que significa que vários métodos podem ter o mesmo nome, desde que eles tenham assinaturas exclusivas. A assinatura de um método consiste em número e tipos de seus parâmetros. A assinatura de um método especificamente não inclui o tipo de retorno ou modificadores de parâmetro como opcional, ByRef ou ParamArray. O exemplo a seguir mostra uma classe com um número de sobrecargas:

```vb
Module Test
    Sub F()
        Console.WriteLine("F()")
    End Sub 

    Sub F(o As Object)
        Console.WriteLine("F(Object)")
    End Sub

    Sub F(value As Integer)
        Console.WriteLine("F(Integer)")
    End Sub 

    Sub F(a As Integer, b As Integer)
        Console.WriteLine("F(Integer, Integer)")
    End Sub 

    Sub F(values() As Integer)
        Console.WriteLine("F(Integer())")
    End Sub 

    Sub G(s As String, Optional s2 As String = 5)
        Console.WriteLine("G(String, Optional String")
    End Sub

    Sub G(s As String)
        Console.WriteLine("G(String)")
    End Sub


    Sub Main()
        F()
        F(1)
        F(CType(1, Object))
        F(1, 2)
        F(New Integer() { 1, 2, 3 })
        G("hello")
        G("hello", "world")
    End Sub
End Module
```

A saída do programa é:

```vb
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

Sobrecargas que diferem apenas em parâmetros opcionais podem ser usadas para "controle de versão" das bibliotecas. Por exemplo, v1 de uma biblioteca pode incluir uma função com parâmetros opcionais:

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

Em seguida, v2 da biblioteca quer adicionar outro parâmetro opcional "senha", e deseja fazer isso sem interromper a compatibilidade de origem (de modo que os aplicativos usados para direcionar v1 podem ser recompilados) e sem interromper a compatibilidade binária (de modo que os aplicativos usados para referência v1 agora pode fazer referência v2 sem recompilação). Esta é a aparência v2:

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

Observe que os parâmetros opcionais em uma API pública não são compatíveis com CLS. No entanto, eles podem ser consumidos pelo menos por Visual Basic e C n º 4 e F #.



### <a name="regular-async-and-iterator-method-declarations"></a>Regular, assíncrono e iterador declarações de método

Há dois tipos de métodos: *sub-rotinas*, que não retornam valores, e *funções*, que fazer. O corpo e `End` construção de um método só pode ser omitida se o método é definido em uma interface ou se tiver o `MustOverride` modificador. Se nenhum tipo de retorno é especificado em uma função e a semântica estrita está sendo usada, ocorre um erro de tempo de compilação; Caso contrário, o tipo é implicitamente `Object` ou o tipo de caractere de tipo do método. O domínio de acessibilidade do tipo de retorno e tipos de parâmetro de um método deve ser igual ou um superconjunto do domínio de acessibilidade do método em si.

Um __método regular__ é aquela com nenhum dos dois `Async` nem `Iterator` modificadores. Pode ser uma sub-rotina ou uma função. Seção [métodos regulares](statements.md#regular-methods) fornece detalhes sobre o que acontece quando um método regular é invocado.

Uma __método iterador__ é aquele com o `Iterator` modificador e nenhum `Async` modificador. Ele deve ser uma função, e seu tipo de retorno deve ser `IEnumerator`, `IEnumerable`, ou `IEnumerator(Of T)` ou `IEnumerable(Of T)` para alguns `T`, e não deve ter `ByRef` parâmetros. Seção [métodos de iterador](statements.md#iterator-methods) fornece detalhes sobre o que acontece quando um método iterador é invocado.

Uma __método assíncrono__ é aquele com o `Async` modificador e nenhum `Iterator` modificador. Ele deve ser uma sub-rotina, ou uma função com o tipo de retorno `Task` ou `Task(Of T)` para alguns `T`e deve ter nenhum `ByRef` parâmetros. Seção [métodos assíncronos](statements.md#async-methods) fornece detalhes sobre o que acontece quando um método assíncrono é invocado.

Ele é um erro de tempo de compilação se um método não é um desses três tipos de método.

Declarações de função e de sub-rotina são especiais em que seu início e instruções end devem cada começar no início de uma linha lógica. Além disso, o corpo de um não -`MustOverride` declaração de função ou sub-rotina deve começar no início de uma linha lógica. Por exemplo:

```vb
Module Test
    ' Illegal: Subroutine doesn't start the line
    Public x As Integer : Sub F() : End Sub

    ' Illegal: First statement doesn't start the line
    Sub G() : Console.WriteLine("G")
    End Sub

    ' Illegal: End Sub doesn't start the line
    Sub H() : End Sub
End Module
```


### <a name="external-method-declarations"></a>Declarações de método externo

Uma declaração de método externo apresenta um novo método cuja implementação é fornecida para o programa externo.

```antlr
ExternalMethodDeclaration
    : ExternalSubDeclaration
    | ExternalFunctionDeclaration
    ;

ExternalSubDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Sub'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )? StatementTerminator
    ;

ExternalFunctionDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Function'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
      StatementTerminator
    ;

ExternalMethodModifier
    : AccessModifier
    | 'Shadows'
    | 'Overloads'
    ;

CharsetModifier
    : 'Ansi' | 'Unicode' | 'Auto'
    ;

LibraryClause
    : 'Lib' StringLiteral
    ;

AliasClause
    : 'Alias' StringLiteral
    ;
```

Como uma declaração de método externo não fornece nenhuma implementação real, ele não tem nenhum corpo de método ou `End` construir. Métodos externos são implicitamente compartilhados, pode não ter parâmetros de tipo e podem não manipular eventos ou implementar membros de interface. Se nenhum tipo de retorno é especificado em uma função e a semântica estrita está sendo usada, ocorrerá um erro de tempo de compilação. Caso contrário, o tipo é implicitamente `Object` ou o tipo de caractere de tipo do método. O domínio de acessibilidade do tipo de retorno e tipos de parâmetro de um método externo deve ser igual ou um superconjunto do domínio de acessibilidade do método externo em si.

A cláusula de biblioteca de uma declaração de método externa Especifica o nome do arquivo externo que implementa o método. A cláusula alias opcional é uma cadeia de caracteres que especifica o ordinal numérico (prefixado por um `#` caractere) ou o nome do método no arquivo externo. Um modificador de conjunto de caractere único também pode ser especificado, que controla o conjunto de caracteres usado para realizar marshaling de cadeias de caracteres durante uma chamada para o método externo. O `Unicode` modificador realiza marshaling de todas as cadeias de caracteres para valores Unicode, o `Ansi` modificador realiza marshaling de todas as cadeias de caracteres para valores ANSI e o `Auto` modificador realiza marshaling de cadeias de caracteres de acordo com as regras do .NET Framework com base no nome do método, ou o nome do alias se especificado. Se nenhum modificador é especificado, o padrão é `Ansi`.

Se `Ansi` ou `Unicode` for especificado, o nome do método é pesquisado no arquivo externo sem modificação. Se `Auto` for especificado, a pesquisa de nome de método depende da plataforma. Se a plataforma é considerada ANSI (por exemplo, o Windows 95, Windows 98, Windows ME), em seguida, o nome do método é pesquisado sem modificação. Se a pesquisa falhar, um `A` é acrescentado e repetir a pesquisa. Se a plataforma é considerada como Unicode (por exemplo, Windows NT, Windows 2000, Windows XP), em seguida, um `W` é acrescentado e o nome é pesquisado. Se a pesquisa falhar, a pesquisa será tentada novamente sem o `W`. Por exemplo:

```vb
Module Test
    ' All platforms bind to "ExternSub".
    Declare Ansi Sub ExternSub Lib "ExternDLL" ()

    ' All platforms bind to "ExternSub".
    Declare Unicode Sub ExternSub Lib "ExternDLL" ()

    ' ANSI platforms: bind to "ExternSub" then "ExternSubA".
    ' Unicode platforms: bind to "ExternSubW" then "ExternSub".
    Declare Auto Sub ExternSub Lib "ExternDLL" ()
End Module
```

Tipos de dados que está sendo passados para métodos externos são empacotados de acordo com as convenções de marshaling de dados do .NET Framework com uma exceção. As variáveis que são passadas por valor de cadeia de caracteres (ou seja, `ByVal x As String`) são empacotados para o tipo BSTR de automação OLE, e as alterações feitas para o BSTR no método externo são refletidas no argumento de cadeia de caracteres. Isso ocorre porque o tipo `String` na externa métodos é mutável, e esse marshalling especial imita esse comportamento. Parâmetros que são passados por referência da cadeia de caracteres (ou seja, `ByRef x As String`) são empacotados como um ponteiro para o tipo BSTR de automação OLE. É possível substituir esses comportamentos especiais, especificando o `System.Runtime.InteropServices.MarshalAsAttribute` atributo no parâmetro.

O exemplo demonstra o uso de métodos externos:

```vb
Class Path
    Declare Function CreateDirectory Lib "kernel32" ( _
        Name As String, sa As SecurityAttributes) As Boolean
    Declare Function RemoveDirectory Lib "kernel32" ( _
        Name As String) As Boolean
    Declare Function GetCurrentDirectory Lib "kernel32" ( _
        BufSize As Integer, Buf As String) As Integer
    Declare Function SetCurrentDirectory Lib "kernel32" ( _
        Name As String) As Boolean
End Class
```


### <a name="overridable-methods"></a>Métodos substituíveis

O `Overridable` modificador indica que um método é substituível. O `Overrides` modificador indica que um método substitui um método substituível de tipo base que tem a mesma assinatura. O `NotOverridable` modificador indica que um método substituível não pode ser substituído ainda mais. O `MustOverride` modificador indica que um método deve ser substituído em classes derivadas.

Determinadas combinações desses modificadores não são válidas:

* `Overridable` e `NotOverridable` são mutuamente exclusivos e não podem ser combinados.

* `MustOverride` implica `Overridable` (e, portanto, não é possível especificá-lo) e não pode ser combinada com `NotOverridable`.

* `NotOverridable` não pode ser combinado com `Overridable` ou `MustOverride` e deve ser combinada com `Overrides`.

* `Overrides` implica `Overridable` (e, portanto, não é possível especificá-lo) e não pode ser combinada com `MustOverride`.

Também há restrições adicionais em métodos substituíveis:

* Um `MustOverride` método não pode incluir um corpo de método ou uma `End` construir, não podem substituir outro método e só pode aparecer em `MustInherit` classes.

* Se um método especifica `Overrides` e não há nenhum método de base correspondente, substituir, ocorre um erro de tempo de compilação. Um método de substituição não pode especificar `Shadows`.

* Um método não pode substituir outro método, se o domínio de acessibilidade do método de substituição não é igual ao domínio de acessibilidade do método que está sendo substituído. A única exceção é que um método substituindo um `Protected Friend` método em outro assembly que não tenha `Friend` acesso deve especificar `Protected` (não `Protected Friend`).

* `Private` métodos podem não estar `Overridable`, `NotOverridable`, ou `MustOverride`, nem talvez elas substituem outros métodos.

* Métodos em `NotInheritable` classes não podem ser declaradas `Overridable` ou `MustOverride`.

O exemplo a seguir ilustra as diferenças entre métodos substituíveis e nonoverridable:

```vb
Class Base
    Public Sub F()
        Console.WriteLine("Base.F")
    End Sub

    Public Overridable Sub G()
        Console.WriteLine("Base.G")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Shadows Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Overrides Sub G()
        Console.WriteLine("Derived.G")
    End Sub
End Class

Module Test
    Sub Main()
        Dim d As Derived = New Derived()
        Dim b As Base = d

        b.F()
        d.F()
        b.G()
        d.G()
    End Sub
End Module
```

No exemplo, a classe `Base` apresenta um método `F` e uma `Overridable` método `G`. A classe `Derived` apresenta um novo método `F`, sombreamento, portanto, o herdado `F`e também substitui o método herdado `G`. O exemplo produz a seguinte saída:

```
Base.F
Derived.F
Derived.G
Derived.G
```

Observe que a instrução `b.G()` invoca `Derived.G`, e não `Base.G`. Isso ocorre porque o tempo de execução de tipo da instância (que é `Derived`) em vez do tipo de tempo de compilação da instância (que é `Base`) determina a implementação real do método para invocar.

### <a name="shared-methods"></a>Métodos compartilhados

O `Shared` modificador indica um método é um *compartilhado método*. Um método compartilhado não funciona em uma instância específica de um tipo e pode ser chamado diretamente de um tipo em vez de uma determinada instância de um tipo. No entanto, é válido, use uma instância para qualificar um método compartilhado. Não é válido para fazer referência a `Me`, `MyClass`, ou `MyBase` em um método compartilhado. Métodos compartilhados não podem ser `Overridable`, `NotOverridable`, ou `MustOverride`, e eles não podem substituir métodos. Métodos definidos em interfaces e módulos padrão não podem especificar `Shared`, pois eles são implicitamente `Shared` já.

Um método declarado em uma estrutura ou classe sem um `Shared` modificador é um *método de instância*. Um método de instância opera em uma determinada instância de um tipo. Métodos de instância só pode ser chamados por meio de uma instância de um tipo e pode referir-se à instância por meio de `Me` expressão.

O exemplo a seguir ilustra as regras de acesso compartilhado e os membros da instância:

```vb
Class Test
    Private x As Integer
    Private Shared y As Integer

    Sub F()
        x = 1 ' Ok, same as Me.x = 1.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub G()
        x = 1 ' Error, cannot access Me.x.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub Main()
        Dim t As Test = New Test()

        t.x = 1 ' Ok.
        t.y = 1 ' Ok.
        Test.x = 1 ' Error, cannot access instance member through type.
        Test.y = 1 ' Ok.
    End Sub
End Class
```

Método `F` mostra que um membro da função de instância, um identificador pode ser usado para acessar membros de instância e membros compartilhados. Método `G` mostra que, em um membro da função compartilhada, ele é um erro ao acessar um membro de instância por meio de um identificador. Método `Main` mostra que, em uma expressão de acesso de membro, membros de instância devem ser acessados por meio de instâncias, mas membros compartilhados podem ser acessados por meio de tipos ou instâncias.

### <a name="method-parameters"></a>Parâmetros de método

Um *parâmetro* é uma variável que pode ser usada para passar informações dentro e fora de um método. Parâmetros de um método são declarados por lista de parâmetros do método, que consiste em um ou mais parâmetros separados por vírgulas.

```antlr
ParameterList
    : Parameter ( Comma Parameter )*
    ;

Parameter
    : Attributes? ParameterModifier* ParameterIdentifier ( 'As' TypeName )?
      ( Equals ConstantExpression )?
    ;

ParameterModifier
    : 'ByVal' | 'ByRef' | 'Optional' | 'ParamArray'
    ;

ParameterIdentifier
    : Identifier IdentifierModifiers
    ;
```

Se nenhum tipo é especificado para um parâmetro e a semântica estrita é usada, ocorrerá um erro de tempo de compilação. Caso contrário, o tipo padrão é `Object` ou o tipo de caractere de tipo do parâmetro. Até mesmo em semântica permissiva, se um parâmetro inclui um `As` cláusula, todos os parâmetros devem especificar tipos.

Parâmetros são especificados como parâmetros de valor, referência, opcional ou paramarray pelos modificadores `ByVal`, `ByRef`, `Optional`, e `ParamArray`, respectivamente. Um parâmetro que não especifica `ByRef` ou `ByVal` assume como padrão `ByVal`.

Nomes de parâmetro têm o escopo para todo o corpo do método e sempre são publicamente acessíveis. Uma invocação de método cria uma cópia específica para essa invocação, os parâmetros, e a lista de argumentos da invocação atribui valores ou referências de variável aos parâmetros recém-criado. Como declarações de método externo e declarações de delegado não tem nenhum corpo, nomes de parâmetro duplicados são permitidos em listas de parâmetros, mas não recomendados.

O identificador pode ser seguido pelo modificador anulável nome `?` para indicar que permite valor nulo, e também por modificadores de nome de matriz para indicar que ele é uma matriz. Eles podem ser combinados, por exemplo, "`ByVal x?() As Integer`". Não é permitido usar limites de matriz explícita; Além disso, se o modificador anulável nome está presente, uma `As` cláusula deve estar presente.


#### <a name="value-parameters"></a>Parâmetros de valor

Um *parâmetro value* é declarado com uma explícita `ByVal` modificador. Se o `ByVal` modificador é usado, o `ByRef` modificador não pode ser especificado. Um parâmetro de valor entra em existência com a invocação do membro, o parâmetro pertence e é inicializado com o valor do argumento fornecido na invocação. Um parâmetro de valor deixa de existir após o retorno do membro.

Um método tem permissão para atribuir novos valores para um parâmetro de valor. Essas atribuições afetam apenas o local de armazenamento local representado pelo parâmetro de valor; eles não têm efeito sobre o argumento real fornecido na invocação do método.

Um parâmetro de valor é usado quando o valor de um argumento é passado para um método e as modificações do parâmetro não afetam o argumento original. Um parâmetro de valor se refere a sua própria variável, que é diferente da variável do argumento correspondente. Essa variável é inicializada copiando o valor do argumento correspondente. O exemplo a seguir mostra um método `F` que tem um parâmetro de valor denominado `p`:

```vb
Module Test
    Sub F(p As Integer)
        Console.WriteLine("p = " & p)
        p += 1
    End Sub 

    Sub Main()
        Dim a As Integer = 1

        Console.WriteLine("pre: a = " & a)
        F(a)
        Console.WriteLine("post: a = " & a)
    End Sub
End Module
```

O exemplo produz a seguinte saída, mesmo que o parâmetro de valor `p` é modificado:

```
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a>Parâmetros de referência

Um parâmetro de referência é um parâmetro declarado com um `ByRef` modificador. Se o `ByRef` modificador é especificado, o `ByVal` modificador não pode ser usado. Um parâmetro de referência não cria um novo local de armazenamento. Em vez disso, um parâmetro de referência representa a variável fornecida como o argumento na invocação do método ou construtor. Conceitualmente, o valor de um parâmetro de referência é sempre o mesmo que a variável subjacente.

Parâmetros de referência atuam em dois modos, como *aliases* ou por meio das *cópia na cópia-back.*

__Aliases.__ Um parâmetro de referência é usado quando o parâmetro atua como um alias para um argumento fornecido pelo chamador. Um parâmetro de referência não em si define uma variável, mas em vez disso, se refere à variável do argumento correspondente. As modificações de um parâmetro de referência diretamente e imediatamente um impacto sobre o argumento correspondente. O exemplo a seguir mostra um método `Swap` que tem dois parâmetros de referência:

```vb
Module Test
    Sub Swap(ByRef a As Integer, ByRef b As Integer)
        Dim t As Integer = a
        a = b
        b = t
    End Sub 

    Sub Main()
        Dim x As Integer = 1
        Dim y As Integer = 2

        Console.WriteLine("pre: x = " & x & ", y = " & y)
        Swap(x, y)
        Console.WriteLine("post: x = " & x & ", y = " & y)
    End Sub 
End Module
```

A saída do programa é:

```
pre: x = 1, y = 2
post: x = 2, y = 1
```

Para a invocação de método `Swap` na classe `Main`, `a` representa `x,` e `b` representa `y`. Portanto, a invocação tem o efeito de troca os valores das `x` e `y`.

Em um método que usa parâmetros de referência, é possível que vários nomes representar o mesmo local de armazenamento:

```vb
Module Test
    Private s As String

    Sub F(ByRef a As String, ByRef b As String)
        s = "One"
        a = "Two"
        b = "Three"
    End Sub

    Sub G()
        F(s, s)
    End Sub
End Module
```

No exemplo a invocação de método `F` na `G` passa uma referência a `s` para ambos `a` e `b`. Assim, para essa invocação, os nomes `s`, `a`, e `b` todas se referem ao mesmo local de armazenamento e as todas as atribuições de três modificar a variável de instância `s`.

__Cópia-em-back de cópia.__ Se o tipo da variável que está sendo passado para um parâmetro de referência não é compatível com o tipo do parâmetro de referência, ou se um não-variável (por exemplo, uma propriedade) é passada como um argumento para um parâmetro de referência, ou se a invocação é associação tardia e, em seguida, um temporário variável é alocada e passado para o parâmetro de referência. O valor que está sendo passado será copiado nessa variável temporária antes que o método é invocado e será copiado novamente para a variável original (se houver um e é gravável) quando o método retorna. Assim, um parâmetro de referência não necessariamente pode conter uma referência para o armazenamento exato da variável que está sendo passado, e todas as alterações para o parâmetro de referência podem não ser refletidas na variável até que o método sai. Por exemplo:

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(ByRef b As Base)
        b = New Base()
    End Sub

    Property G() As Base
        Get
        End Get
        Set
        End Set
    End Property

    Sub Main()
        Dim d As Derived

        F(G)   ' OK.
        F(d)   ' Throws System.InvalidCastException after F returns.
    End Sub
End Module
```

No caso da primeira invocação da `F`, uma variável temporária é criada e o valor da propriedade `G` é atribuído a ele e passado para `F`. Após o retorno de `F`, o valor na variável temporária é atribuído para a propriedade de `G`. No segundo caso, a outra variável temporária é criada e o valor de `d` é atribuído a ele e passado para `F`. Ao retornar de `F`, o valor na variável temporária é convertido de volta para o tipo da variável `Derived`e atribuídos a `d`. Desde o valor que está sendo passado de volta não pode ser convertido `Derived`, uma exceção será lançada em tempo de execução.

#### <a name="optional-parameters"></a>Parâmetros opcionais

Um parâmetro opcional é declarado com o `Optional` modificador. Parâmetros que seguem um parâmetro opcional na lista de parâmetros formais também devem ser opcionais; Falha ao especificar o `Optional` modificador nos seguintes parâmetros acionará um erro de tempo de compilação. Um parâmetro opcional de alguns digite tipo anulável `T?` ou tipo não anulável `T` deve especificar uma expressão constante `e` a ser usado como um valor padrão se nenhum argumento for especificado. Se `e` for avaliada como `Nothing` do tipo Object, em seguida, o valor padrão a *tipo de parâmetro* será usado como o padrão para o parâmetro. Caso contrário, `CType(e, T)` deve ser uma expressão constante e ele será interpretado como o padrão para o parâmetro.

Os parâmetros opcionais são a única situação em que um inicializador em um parâmetro é válido. A inicialização sempre é feita como parte da expressão de invocação, não no próprio corpo do método.

```vb
Module Test
    Sub F(x As Integer, Optional y As Integer = 20)
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub

    Sub Main()
        F(10)
        F(30,40)
    End Sub
End Module
```

A saída do programa é:

```
x = 10, y = 20
x = 30, y = 40
```

Parâmetros opcionais não podem ser especificados em declarações de delegado ou eventos, nem em expressões lambda.

#### <a name="paramarray-parameters"></a>Parâmetros ParamArray

`ParamArray` os parâmetros são declarados com o `ParamArray` modificador. Se o `ParamArray` modificador estiver presente, o `ByVal` modificador deve ser especificado e nenhum outro parâmetro pode usar o `ParamArray` modificador. O `ParamArray` tipo do parâmetro deve ser uma matriz unidimensional, e ele deve ser o último parâmetro na lista de parâmetros.

Um `ParamArray` parâmetro representa um número indefinido de parâmetros do tipo do `ParamArray`. Dentro do método em si, um `ParamArray` parâmetro é tratado como seu tipo declarado e não tem nenhuma semântica especial. Um `ParamArray` parâmetro é opcional implicitamente, com um valor padrão de uma matriz unidimensional vazia do tipo do `ParamArray`.

Um `ParamArray` permite argumentos a ser especificada em uma das duas maneiras de uma invocação de método:

* O argumento fornecido para um `ParamArray` pode ser uma única expressão de um tipo que é ampliado para o `ParamArray` tipo. Nesse caso, o `ParamArray` funciona exatamente como um parâmetro de valor.

* Como alternativa, a invocação pode especificar zero ou mais argumentos para o `ParamArray`, onde cada argumento é uma expressão de um tipo que é implicitamente conversível para o tipo de elemento de `ParamArray`. Nesse caso, a invocação cria uma instância do `ParamArray` tipo com um comprimento correspondente ao número de argumentos, inicializa os elementos da matriz de instância com os valores de argumento fornecido e usa a matriz criada recentemente a instância como o real argumento.

Exceto para permitir que um número variável de argumentos em uma invocação um `ParamArray` é precisamente equivalente a um parâmetro de valor do mesmo tipo, como mostra o exemplo a seguir.

```vb
Module Test
    Sub F(ParamArray args() As Integer)
        Dim i As Integer

        Console.Write("Array contains " & args.Length & " elements:")
        For Each i In args
            Console.Write(" " & i)
        Next i
        Console.WriteLine()
    End Sub

    Sub Main()
        Dim a As Integer() = { 1, 2, 3 }

        F(a)
        F(10, 20, 30, 40)
        F()
    End Sub
End Module
```

O exemplo produz a saída

```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

A primeira invocação da `F` simplesmente passa a matriz `a` como um parâmetro de valor. A segunda chamada de `F` automaticamente cria uma matriz de quatro elementos com os valores do elemento fornecido e passa essa instância de matriz como um parâmetro de valor. Da mesma forma, a terceira invocação de `F` cria uma matriz de elemento zero e passa essa instância como um parâmetro de valor. As invocações de segunda e terceira são precisamente equivalentes a gravação:

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

`ParamArray` parâmetros não podem ser especificados em declarações de delegado ou evento.

### <a name="event-handling"></a>Tratamento de Evento

Métodos declarativamente podem manipular eventos gerados por objetos na instância ou variáveis compartilhadas. Para manipular eventos, uma declaração de método Especifica o `Handles` palavra-chave e um ou mais eventos de lista.

```antlr
HandlesClause
    : ( 'Handles' EventHandlesList )?
    ;

EventHandlesList
    : EventMemberSpecifier ( Comma EventMemberSpecifier )*
    ;

EventMemberSpecifier
    : Identifier Period IdentifierOrKeyword
    | 'MyBase' Period IdentifierOrKeyword
    | 'MyClass' Period IdentifierOrKeyword
    | 'Me' Period IdentifierOrKeyword
    ;
```

Um evento no `Handles` lista é especificada por dois identificadores separados por um período:

* O identificador da primeira deve ser uma instância ou uma variável compartilhada no tipo recipiente que especifica o `WithEvents` modificador ou o `MyBase` ou `MyClass` ou `Me` palavra-chave; caso contrário, ocorrerá um erro de tempo de compilação. Essa variável contém o objeto que irá gerar os eventos manipulados por este método.

* O identificador do segundo deve especificar um membro do tipo do identificador do primeiro. O membro deve ser um evento e pode ser compartilhado. Se uma variável compartilhada for especificada para o identificador do primeiro, em seguida, o evento deve ser compartilhado ou ocorrerá um erro.

Um método de manipulador `M` é considerado um manipulador de eventos válidos para um evento `E` se a instrução `AddHandler E, AddressOf M` também seria válida. Ao contrário de um `AddHandler` instrução, no entanto, os manipuladores de eventos explícito permitem manipulando um evento com um método sem nenhum argumento, independentemente se a semântica estrita está sendo usada ou não:

```vb
Option Strict On

Class C1
    Event E(x As Integer)
End Class

Class C2
    withEvents C1 As New C1()

    ' Valid
    Sub M1() Handles C1.E
    End Sub

    Sub M2()
        ' Invalid
        AddHandler C1.E, AddressOf M1
    End Sub
End Class
```

Um único membro pode manipular vários eventos correspondentes e vários métodos podem lidar com um único evento. Acessibilidade de um método não tem nenhum efeito em sua capacidade de manipular eventos. O exemplo a seguir mostra como um método pode manipular eventos:

```vb
Class Raiser
    Event E1()

    Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    WithEvents x As Raiser

    Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
        x.Raise()
        x.Raise()
    End Sub
End Module
```

Isso imprimirá:

```
Raised
Raised
```

Um tipo herda todos os manipuladores de eventos fornecidos pelo seu tipo base. Um tipo derivado não é possível, de forma alguma, alterar os mapeamentos de evento herda de seus tipos base, mas pode adicionar manipuladores adicionais para o evento.


### <a name="extension-methods"></a>Métodos de extensão

Métodos podem ser adicionados aos tipos de fora a declaração de tipo usando *métodos de extensão*. Métodos de extensão são métodos com o `System.Runtime.CompilerServices.ExtensionAttribute` atributo aplicado a eles. Eles só podem ser declarados em módulos padrão e devem ter pelo menos um parâmetro, que especifica que o tipo o método estende. Por exemplo, o seguinte método de extensão estende o tipo `String`:

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

__Observação.__ Embora o Visual Basic exige os métodos de extensão deve ser declarado em um módulo padrão, outras linguagens como c# podem permitir que eles sejam declaradas em outros tipos de tipos. Desde que os métodos seguem as outras convenções descritas aqui e que o contém tipo não é um tipo genérico aberto e não pode ser instanciado, o Visual Basic reconhecerá os métodos de extensão.

Quando um método de extensão é chamado, ele está sendo invocado na instância é passada para o primeiro parâmetro. O primeiro parâmetro não pode ser declarado `Optional` ou `ParamArray`. Qualquer tipo, incluindo um parâmetro de tipo, pode aparecer como o primeiro parâmetro de um método de extensão. Por exemplo, os seguintes métodos estendem os tipos `Integer()`, qualquer tipo que implemente `System.Collections.Generic.IEnumerable(Of T)`, e qualquer tipo em todos os:

```vb
Imports System.Runtime.CompilerServices

Module Extensions
    <Extension> _
    Sub PrintArray(a() As Integer)
        ...
    End Sub

    <Extension> _
    Sub PrintList(Of T)(a As IEnumerable(Of T))
        ...
    End Sub

    <Extension> _
    Sub Print(Of T)(a As T)
        ...
    End Sub
End Module
```

Como mostra o exemplo anterior, as interfaces podem ser estendidos. Métodos de extensão de interface fornecem a implementação do método, para que os tipos que implementam uma interface que tem métodos de extensão definidos nele ainda apenas implementam os membros declarados originalmente pela interface. Por exemplo:

```vb
Imports System.Runtime.CompilerServices

Interface IAction
  Sub DoAction()
End Interface

Module IActionExtensions 
    <Extension> _
    Public Sub DoAnotherAction(i As IAction) 
        i.DoAction()
    End Sub
End Module

Class C
  Implements IAction

  Sub DoAction() Implements IAction.DoAction
    ...
  End Sub

  ' ERROR: Cannot implement extension method IAction.DoAnotherAction
  Sub DoAnotherAction() Implements IAction.DoAnotherAction
    ...
  End Sub
End Class
```

Métodos de extensão também podem ter restrições de tipo em seus parâmetros de tipo e, assim como com métodos de não-extensão genéricos, argumento de tipo pode ser inferido:

```vb
Imports System.Runtime.CompilerServices

Module IEnumerableComparableExtensions
    <Extension> _
    Public Function Sort(Of T As IComparable(Of T))(i As IEnumerable(Of T)) _
        As IEnumerable(Of T)
        ...
    End Function
End Module
```

Métodos de extensão também podem ser acessados por meio de expressões de instância implícita dentro do tipo que está sendo estendido:

```vb
Imports System.Runtime.CompilerServices

Class C1
    Sub M1()
        Me.M2()
        M2()
    End Sub
End Class

Module C1Extensions
    <Extension>
    Sub M2(c As C1)
        ...
    End Sub
End Module
```

Para fins de acessibilidade, os métodos de extensão também são tratados como membros do módulo padrão que são declarados, eles não têm mais acesso aos membros do tipo que estão estendendo além de acesso eles têm em virtude de seu contexto de declaração.

Métodos de extensão estão disponíveis somente quando o método de módulo padrão está no escopo. Caso contrário, o tipo original não será exibida para foram estendidos. Por exemplo:

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Namespace N1
    Module C1Extensions
        <Extension> _
        Sub M1(c As C1)
            ...
        End Sub
    End Module
End Namespace

Module Test
    Sub Main()
        Dim c As New C1()

        ' Error: c has no member named "M1"
        c.M1()
    End Sub
End Module
```

Referindo-se a um tipo quando apenas um método de extensão no tipo está disponível ainda produzirá um erro de tempo de compilação.

É importante observar que os métodos de extensão são considerados membros do tipo em todos os contextos em que os membros são associados, como fortemente tipado `For Each` padrão. Por exemplo:

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Class C1Enumerator
    ReadOnly Property Current() As C1
        Get
            ...
        End Get
    End Property

    Function MoveNext() As Boolean
        ...
    End Function
End Class

Module C1Extensions
    <Extension> _
    Function GetEnumerator(c As C1) As C1Enumerator
        ...
    End Function
End Module

Module Test
    Sub Main()
        Dim c As New C1()

        ' Valid
        For Each o As Object In c
            ...
        Next o
    End Sub
End Module
```

Delegados também podem ser criados que se referem aos métodos de extensão. Portanto, o código:

```vb
Delegate Sub D1()

Module Test
    Sub Main()
        Dim s As String = "Hello, World!"
        Dim d As D1

        d = AddressOf s.Print
        d()
    End Sub
End Module
```

É aproximadamente equivalente a:

```vb
Delegate Sub D1()

Module Test
    Sub Main()
      Dim s As String = "Hello, World!"
      Dim d As D1

      d = CType([Delegate].CreateDelegate(GetType(D1), s, _
                GetType(StringExtensions).GetMethod("Print")), D1)
      d()
    End Sub
End Module
```

__Observação.__ Normalmente, o Visual Basic insere uma verificação em uma chamada de método de instância que faz com que um `System.NullReferenceException` ocorra se a instância que o método é invocado em é `Nothing`. No caso de métodos de extensão, não há nenhuma maneira eficiente para inserir essa verificação, portanto, serão necessário verificar explicitamente os métodos de extensão `Nothing`. 

__Observação.__ Um tipo de valor será ser boxed quando está sendo passado como um `ByVal` argumento para um parâmetro digitado como uma interface.  Isso implica que os efeitos colaterais do método de extensão funcionará em uma cópia da estrutura, em vez do original. Embora a linguagem não coloca nenhuma restrição sobre o primeiro argumento de um método de extensão, é recomendável que os métodos de extensão não são usados para estender os tipos de valor ou que, ao estender tipos de valor, o primeiro parâmetro é passado `ByRef` para garantir que esse lado efeitos de operam no valor original.

### <a name="partial-methods"></a>Métodos parciais

Um *método parcial* é um método que especifica uma assinatura, mas não o corpo do método. O corpo do método pode ser fornecido por outra declaração de método com o mesmo nome e assinatura, muito provavelmente na outra declaração parcial do tipo. Por exemplo:

a.vb:

```vb
' Designer generated code
Public Partial Class MyForm
    Private Partial Sub ValidateControls()
    End Sub

    Public Sub New()
        ' Initialize controls
        ...

        ValidateControls()
    End Sub    
End Class
```

b.vb:

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

Neste exemplo, uma declaração parcial da classe `MyForm` declara um método parcial `ValidateControls` sem implementação. O construtor na declaração parcial chama o método parcial, mesmo que não há nenhum corpo fornecido no arquivo. A declaração parcial de `MyForm` , em seguida, fornece a implementação do método.

Métodos parciais podem ser chamados, independentemente se um corpo foi fornecido; não se for fornecido nenhum corpo de método, a chamada será ignorada. Por exemplo:

```vb
Public Class C1
    Private Partial Sub M1()
    End Sub

    Public Sub New()
        ' Since no implementation is supplied, this call will not be made.
        M1()
    End Sub
End Class
```

Ignorado também todas as expressões que são passadas como argumentos para uma chamada de método parcial que é ignorada e não avaliadas. (__Observação.__ Isso significa que os métodos parciais são uma maneira muito eficiente de fornecer o comportamento que é definido entre dois tipos parciais, como os métodos parciais têm sem custos se elas não são usadas.)

Declaração de método parcial deve ser declarada como `Private` e deve ser sempre uma sub-rotina sem declarações em seu corpo. Métodos parciais não podem se implementar métodos de interface, embora o método que fornece o corpo pode.

Somente um método pode fornecer um corpo de um método parcial. Um método fornecendo um corpo de um método parcial deve ter a mesma assinatura que o método parcial, as mesmas restrições sobre quaisquer parâmetros de tipo, mesmo modificadores de declaração e o mesmo parâmetro e os nomes de parâmetro de tipo. Atributos de método parcial e o método que fornece seu corpo são mesclados, assim como todos os atributos nos parâmetros dos métodos. Da mesma forma, a lista de eventos que lidam com os métodos é mesclada. Por exemplo:

```vb
Class C1
    Event E1()
    Event E2()

    Private Partial Sub S() Handles Me.E1
    End Sub

    ' Handles both E1 and E2
    Private Sub S() Handles Me.E2
        ...
    End Sub
End Class
```

## <a name="constructors"></a>Construtores

*Construtores* são métodos especiais que permitem o controle sobre a inicialização. Eles são executados depois que o programa é iniciado ou quando uma instância de um tipo é criada. Ao contrário de outros membros, construtores não são herdados e não introduzem um nome para o espaço de declaração de um tipo. Construtores podem ser invocados somente por expressões de criação do objeto ou do .NET Framework. eles podem nunca ser chamados diretamente.

__Observação.__ Construtores têm a mesma restrição no posicionamento de linha que tem as sub-rotinas. A instrução de início, a instrução end e o bloco devem aparecer no início de uma linha lógica.

```antlr
ConstructorMemberDeclaration
    : Attributes? ConstructorModifier* 'Sub' 'New'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Sub' StatementTerminator
    ;

ConstructorModifier
    : AccessModifier
    | 'Shared'
    ;
```

### <a name="instance-constructors"></a>Construtores de instâncias

*Construtores de instância* inicializar instâncias de um tipo e são executados pelo .NET Framework quando uma instância é criada. A lista de parâmetros de um construtor está sujeito às mesmas regras que a lista de parâmetros de um método. Construtores de instância podem estar sobrecarregados.

Todos os construtores em tipos de referência devem chamar outro construtor. Se a invocação explícita, ele deve ser a primeira instrução no corpo do método de construtor. A instrução pode invocar outro dos construtores de instância do tipo – por exemplo, `Me.New(...)` ou `MyClass.New(...)` – ou se ele não é uma estrutura, ela pode invocar um construtor de instância do tipo base do tipo – por exemplo, `MyBase.New(...)`. Não é válido para um construtor chamar a mesmo. Se um construtor omite uma chamada para outro construtor, `MyBase.New()` é implícito. Se não houver nenhum construtor sem parâmetros de tipo base, ocorrerá um erro de tempo de compilação. Porque `Me` não é considerado ser construído até após a chamada para um construtor de classe base, os parâmetros para uma instrução de invocação do construtor não podem referenciar `Me`, `MyClass`, ou `MyBase` implícita ou explicitamente.

Quando a primeira instrução de um construtor é da forma `MyBase.New(...)`, o construtor implicitamente executa as inicializações especificadas pelos inicializadores de variável das variáveis de instância declarados no tipo. Isso corresponde a uma sequência de atribuições que são executadas imediatamente depois de invocar o construtor do tipo base direta. Essa ordenação garante que todas as variáveis de instância de base são inicializadas por seus inicializadores de variável antes de quaisquer declarações que têm acesso à instância são executadas. Por exemplo:

```vb
Class A
    Protected x As Integer = 1
End Class

Class B
    Inherits A

    Private y As Integer = x

    Public Sub New()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Class
```

Quando `New B()` é usado para criar uma instância de `B`, a seguinte saída é produzida:

```
x = 1, y = 1
```

O valor de `y` é `1` porque o inicializador de variável é executado depois que o construtor de classe base será invocado. Inicializadores de variável são executados na ordem textual aparecem na declaração de tipo.

Quando um tipo declara apenas `Private` construtores, não é possível em geral para outros tipos derivam do tipo ou criar instâncias do tipo; a única exceção é tipos aninhados dentro do tipo. `Private` construtores são comumente usados em tipos que contêm apenas `Shared` membros.

Se um tipo não contém nenhuma declaração de construtor de instância, um construtor padrão é fornecido automaticamente. O construtor padrão simplesmente invoca o construtor sem parâmetros do tipo base direto. Se o tipo base direto não tem um construtor sem parâmetros acessível, ocorrerá um erro de tempo de compilação. O tipo de acesso declarado para o construtor padrão é `Public` , a menos que o tipo é `MustInherit`, caso em que o construtor padrão é `Protected`.

__Observação.__ O acesso padrão para um `MustInherit` é o construtor padrão do tipo `Protected` porque `MustInherit` classes não podem ser criadas diretamente. Portanto, não há nenhum ponto de tornar o construtor padrão `Public`.

No exemplo a seguir, um construtor padrão é fornecido porque a classe não contém nenhuma declaração de construtor:

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

Assim, o exemplo é precisamente equivalente ao seguinte:

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

Construtores padrão que são emitidos em um designer gerado classe marcada com o atributo `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` chamará o método `Sub InitializeComponent()`, se ele existir, após a chamada para o construtor base. (__Observação.__ Isso permite que os arquivos gerados designer, como aqueles criados pelo criador do WinForms, omita o construtor no arquivo do designer. Isso permite que o programador deve especificá-lo por conta própria, se desejarem.)

### <a name="shared-constructors"></a>Construtores de compartilhado

*Construtores compartilhados* inicializar um tipo compartilhado variáveis; eles são executados depois que o programa começa a ser executado, mas antes de quaisquer referências a um membro do tipo. Um construtor compartilhado Especifica o `Shared` modificador, a menos que ele está em um módulo padrão, caso em que o `Shared` modificador é implícita.

Ao contrário de construtores de instância, construtores compartilhados tem acesso implícito de público, não têm parâmetros e não podem chamar outros construtores. Antes da primeira instrução em um construtor compartilhado, o construtor compartilhado implicitamente executa as inicializações especificadas pelos inicializadores de variável das variáveis compartilhadas declarados no tipo. Isso corresponde a uma sequência de atribuições que são executados imediatamente após a entrada para o construtor. Os inicializadores de variável são executados na ordem textual aparecem na declaração de tipo.

A exemplo a seguir mostra um `Employee` classe com um construtor compartilhado que inicializa uma variável compartilhada:

```vb
Imports System.Data

Class Employee
    Private Shared ds As DataSet

    Shared Sub New()
        ds = New DataSet()
    End Sub

    Public Name As String
    Public Salary As Decimal
End Class
```

Existe um construtor compartilhado separado para cada tipo genérico fechado. Porque o construtor compartilhado é executado exatamente uma vez para cada tipo fechado, ele é um local conveniente para impor verificações de tempo de execução no parâmetro de tipo não podem ser verificados em tempo de compilação por meio de restrições. Por exemplo, o tipo a seguir usa um construtor compartilhado para impor que o parâmetro de tipo é `Integer` ou `Double`:

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

Exatamente quando os construtores compartilhados são executados é principalmente implementação depende, embora várias garantias são fornecidas, se um construtor compartilhado é definido explicitamente:

* Construtores compartilhados são executados antes do primeiro acesso a qualquer campo estático do tipo.

* Construtores compartilhados são executados antes da primeira invocação de qualquer método estático do tipo.

* Construtores compartilhados são executados antes da primeira invocação de qualquer construtor para o tipo.

As garantias acima não se aplicam a situação em que um construtor compartilhado é implicitamente criado para inicializadores compartilhados. A saída do exemplo a seguir é incerta, porque a ordem exata de carregamento e, portanto, do construtor compartilhado execução não foi definida:

```vb
Module Test
    Sub Main()
        A.F()
        B.F()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("A.F")
    End Sub
End Class

Class B
    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("B.F")
    End Sub
End Class
```

A saída pode ser um destes procedimentos:

```
Init A
A.F
Init B
B.F
```

ou

```
Init B
Init A
A.F
B.F
```

Por outro lado, o exemplo a seguir produz saída previsível. Observe que o `Shared` construtor da classe `A` nunca é executado, mesmo que classe `B` deriva dela:

```vb
Module Test
    Sub Main()
        B.G()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub
End Class

Class B
    Inherits A

    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub G()
        Console.WriteLine("B.G")
    End Sub
End Class
```

A saída é:

```
Init B
B.G
```

Também é possível criar dependências circulares que permitem `Shared` variáveis com inicializadores de variável a ser observada no seu padrão valor de estado, como no exemplo a seguir:

```vb
Class A
    Public Shared X As Integer = B.Y + 1
End Class

Class B
    Public Shared Y As Integer = A.X + 1

    Shared Sub Main()
        Console.WriteLine("X = " & A.X & ", Y = " & B.Y)
    End Sub
End Class
```

Isso produz a saída:

```
X = 1, Y = 2
```

Para executar o `Main` método, o sistema carrega primeiro classe `B`. O `Shared` construtor da classe `B` prossegue para calcular o valor inicial da `Y`, quais recursivamente faz com que a classe `A` a ser carregado porque o valor de `A.X` é referenciado. O `Shared` construtor da classe `A` por sua vez prossegue para calcular o valor inicial da `X`e fazer buscas caso o *padrão* valor `Y`, que é zero. `A.X` Assim, é inicializado para `1`. O processo de carregamento `A` , em seguida, for concluído, retornando para o cálculo do valor inicial de `Y`, cujo resultado se torna `2`.

Tinha a `Main` método em vez disso, foi localizado na classe `A`, o exemplo seria ter produziu a saída a seguir:

```
X = 2, Y = 1
```

Evitar referências circulares no `Shared` inicializadores de variável, pois ela é geralmente impossível determinar a ordem na quais as classes que contêm essas referências são carregados.

## <a name="events"></a>Eventos

Eventos são usados para notificar o código de uma ocorrência específica. Uma declaração de evento consiste em um identificador, um tipo de delegado ou uma lista de parâmetros e um opcional `Implements` cláusula.

```antlr
EventMemberDeclaration
    : RegularEventMemberDeclaration
    | CustomEventMemberDeclaration
    ;

RegularEventMemberDeclaration
    : Attributes? EventModifiers* 'Event'
      Identifier ParametersOrType ImplementsClause? StatementTerminator
    ;

InterfaceEventMemberDeclaration
    : Attributes? InterfaceEventModifiers* 'Event'
      Identifier ParametersOrType StatementTerminator
    ;

ParametersOrType
    : ( OpenParenthesis ParameterList? CloseParenthesis )?
    | 'As' NonArrayTypeName
    ;

EventModifiers
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    ;

InterfaceEventModifiers
    : 'Shadows'
    ;
```

Se um tipo de delegado for especificado, o tipo de delegado não pode ter um tipo de retorno. Se uma lista de parâmetro for especificada, ele não pode conter `Optional` ou `ParamArray` parâmetros. Domínio de acessibilidade dos tipos de parâmetro e/ou tipo de delegado deve ser igual ou um superconjunto do domínio de acessibilidade de evento em si. Eventos podem ser compartilhados, especificando o `Shared` modificador.

O nome do membro adicionado ao espaço de declaração do tipo, além de uma declaração de evento declara implicitamente vários outros membros. Dado um evento chamado `X`, os seguintes membros são adicionados ao espaço de declaração:

* Se a forma da declaração é uma declaração de método, uma classe delegate aninhado chamado `XEventHandler` é introduzido. A classe delegate aninhado corresponde à declaração de método e tem a mesma acessibilidade que o evento. Os atributos na lista de parâmetros se aplicam aos parâmetros da classe delegada.

* Um `Private` variável de instância é digitado como o delegado chamado `XEvent`.

* Dois métodos chamados `add_X` e `remove_X` que não pode ser invocado, substituído ou sobrecarregado.

Se um tipo de tentativas declarar um nome que corresponda a um dos nomes acima, ocorrerá um erro de tempo de compilação e o implícitas `add_X` e `remove_X` declarações são ignoradas para fins de associação de nome. Não é possível substituir ou sobrecarga de qualquer um dos membros introduzidos, embora seja possível sombreá-los em tipos derivados. Por exemplo, a declaração de classe

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

é equivalente à declaração a seguir

```vb
Class Raiser
    Public Delegate Sub ConstructedEventHandler(i As Integer)

    Protected ConstructedEvent As ConstructedEventHandler

    Public Sub add_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Combine(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub

    Public Sub remove_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Remove(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub
End Class
```

Declarar um evento sem especificar um tipo de delegado é a sintaxe mais simples e mais compacta, mas tem a desvantagem de declarar um novo tipo de delegado para cada evento. Por exemplo, no exemplo a seguir, três tipos de delegado ocultos são criados, mesmo que todos os três eventos tenham a mesma lista de parâmetros:

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

No exemplo a seguir, os eventos simplesmente usam o mesmo delegado, `EventHandler`:

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

Eventos podem ser manipulados em uma das duas maneiras: estática ou dinamicamente. Estaticamente a manipulação de eventos é mais simples e requer apenas um `WithEvents` variável e um `Handles` cláusula. No exemplo a seguir, a classe `Form1` estaticamente manipula o evento `Click` do objeto `Button`:

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

Dinamicamente a manipulação de eventos é mais complexo porque o evento deve ser explicitamente conectado e desconectado no código. A instrução `AddHandler` adiciona um manipulador para um evento e a instrução `RemoveHandler` remove um manipulador para um evento. O exemplo a seguir mostra uma classe `Form1` que adiciona `Button1_Click` como um manipulador de eventos `Button1`do `Click` evento:

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

No método `Disconnect`, o manipulador de eventos é removido.


### <a name="custom-events"></a>Eventos personalizados

Conforme discutido na seção anterior, declarações de evento definem implicitamente um campo, uma `add_` método e um `remove_` método que são usados para manter o controle de manipuladores de eventos. Em algumas situações, no entanto, pode ser desejável para fornecer o código personalizado para manipuladores de eventos de rastreamento. Por exemplo, se uma classe define quarenta eventos dos quais somente alguns serão sempre tratadas, usando uma tabela de hash em vez de quarenta campos para acompanhar os manipuladores para cada evento podem ser mais eficientes. *Eventos personalizados* permitir que o `add_X` e `remove_X` métodos seja definido explicitamente, o que permite que um armazenamento personalizado para manipuladores de eventos.

Eventos personalizados são declarados da mesma forma que eventos que especificam um tipo de delegado são declarados com a exceção de que a palavra-chave `Custom` deve preceder o `Event` palavra-chave. Uma declaração de evento personalizado contém três declarações: uma `AddHandler` declaração, um `RemoveHandler` declaração e um `RaiseEvent` declaração. Nenhuma das declarações podem ter qualquer modificador, embora possam ter atributos.

```antlr
CustomEventMemberDeclaration
    : Attributes? EventModifiers* 'Custom' 'Event'
      Identifier 'As' TypeName ImplementsClause? StatementTerminator
      EventAccessorDeclaration+
      'End' 'Event' StatementTerminator
    ;

EventAccessorDeclaration
    : AddHandlerDeclaration
    | RemoveHandlerDeclaration
    | RaiseEventDeclaration
    ;

AddHandlerDeclaration
    : Attributes? 'AddHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'AddHandler' StatementTerminator
    ;

RemoveHandlerDeclaration
    : Attributes? 'RemoveHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RemoveHandler' StatementTerminator
    ;

RaiseEventDeclaration
    : Attributes? 'RaiseEvent'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RaiseEvent' StatementTerminator
    ;
```

Por exemplo:

```vb
Class Test
    Private Handlers As EventHandler

    Public Custom Event TestEvent As EventHandler
        AddHandler(value As EventHandler)
            Handlers = CType([Delegate].Combine(Handlers, value), _
                EventHandler)
        End AddHandler

        RemoveHandler(value as EventHandler)
            Handlers = CType([Delegate].Remove(Handlers, value), _
                EventHandler)
        End RemoveHandler

        RaiseEvent(sender As Object, e As EventArgs)
            Dim TempHandlers As EventHandler = Handlers

            If TempHandlers IsNot Nothing Then
                TempHandlers(sender, e)
            End If
        End RaiseEvent
    End Event
End Class
```

O `AddHandler` e `RemoveHandler` declaração assumir um `ByVal` parâmetro, que deve ser do tipo de delegado do evento. Quando um `AddHandler` ou `RemoveHandler` instrução é executada (ou um `Handles` cláusula automaticamente manipula um evento), a declaração correspondente será chamada. O `RaiseEvent` declaração usa os mesmos parâmetros que o delegado do evento e será chamada quando um `RaiseEvent` instrução é executada. Todas as declarações de devem ser fornecidas e são consideradas sub-rotinas.

Observe que `AddHandler`, `RemoveHandler` e `RaiseEvent` declarações têm a mesma restrição no posicionamento de linha que tem as sub-rotinas. A instrução de início, a instrução end e o bloco devem aparecer no início de uma linha lógica.

O nome do membro adicionado ao espaço de declaração do tipo, além de uma declaração de evento personalizado declara implicitamente vários outros membros. Dado um evento chamado `X`, os seguintes membros são adicionados ao espaço de declaração:

* Um método chamado `add_X`, correspondente à `AddHandler` declaração.

* Um método chamado `remove_X`, correspondente à `RemoveHandler` declaração.

* Um método chamado `fire_X`, correspondente à `RaiseEvent` declaração.

Se um tipo de tentativas declarar um nome que corresponda a um dos nomes acima, ocorrerá um erro de tempo de compilação e as declarações implícitas são ignoradas para fins de associação de nome. Não é possível substituir ou sobrecarga de qualquer um dos membros introduzidos, embora seja possível sombreá-los em tipos derivados.

__Observação.__ `Custom` não é uma palavra reservada.

#### <a name="custom-events-in-winrt-assemblies"></a>Eventos personalizados em assemblies de WinRT

A partir do Microsoft Visual Basic 11.0, eventos declarados em um arquivo compilado com `/target:winmdobj`, declarado em uma interface em um arquivo desse tipo e, em seguida, implementado em outro lugar, são tratados de maneira um pouco diferente.

* Ferramentas externas usadas para criar o winmd normalmente permitir somente determinados tipos de delegado, como `System.EventHandler(Of T)` ou `System.TypedEventHandle(Of T, U)`e não permitirá a outras pessoas.

* O `XEvent` campo tem um tipo `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` onde `T` é o tipo de delegado.

* Retorna o acessador AddHandler um `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`, e o acessador RemoveHandler usa um único parâmetro do mesmo tipo.

Aqui está um exemplo de um evento personalizado.

```vb
Imports System.Runtime.InteropServices.WindowsRuntime

Public NotInheritable Class ClassInWinMD
    Private XEvent As EventRegistrationTokenTable(Of EventHandler(Of Integer))

    Public Custom Event X As EventHandler(Of Integer)
        AddHandler(handler As EventHandler(Of Integer))
            Return EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                   GetOrCreateEventRegistrationTokenTable(XEvent).
                   AddEventHandler(handler)
        End AddHandler

        RemoveHandler(token As EventRegistrationToken)
            EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                RemoveEventHandler(token)
        End RemoveHandler

        RaiseEvent(sender As Object, i As Integer)
            Dim table = EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                InvocationList
            If table IsNot Nothing Then table(sender, i)
        End RaiseEvent
    End Event
End Class
```


## <a name="constants"></a>Constantes

Um *constante* é um valor constante que é um membro de um tipo.

```antlr
ConstantMemberDeclaration
    : Attributes? ConstantModifier* 'Const' ConstantDeclarators StatementTerminator
    ;

ConstantModifier
    : AccessModifier
    | 'Shadows'
    ;

ConstantDeclarators
    : ConstantDeclarator ( Comma ConstantDeclarator )*
    ;

ConstantDeclarator
    : Identifier ( 'As' TypeName )? Equals ConstantExpression StatementTerminator
    ;
```

Constantes são implicitamente compartilhadas. Se a declaração contém um `As` cláusula, a cláusula Especifica o tipo do membro introduzido pela declaração. Se o tipo for omitido, o tipo da constante é inferido. O tipo de uma constante só pode ser um tipo primitivo ou `Object`. Se uma constante é digitada como `Object` e não há nenhum caractere de tipo, o tipo real da constante será o tipo da expressão constante. Caso contrário, o tipo da constante é o tipo de caractere de tipo de constante.

O exemplo a seguir mostra uma classe chamada `Constants` que tem duas constantes públicas:

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

Constantes podem ser acessadas por meio da classe, como no exemplo a seguir, que imprime os valores de `Constants.A` e `Constants.B`.

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

Uma declaração de constante que declara várias constantes é equivalente a várias declarações de constantes únicos. O exemplo a seguir declara três constantes em uma declaração de estrutura.

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

Essa declaração é equivalente ao seguinte:

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

O domínio de acessibilidade do tipo da constante deve ser igual ou um superconjunto do domínio de acessibilidade da própria constante. A expressão de constante deve produzir um valor de tipo de constante ou de um tipo que é implicitamente conversível no tipo de constante. A expressão de constante não pode ser circular; ou seja, uma constante não pode ser definida em termos de em si.

O compilador avalia automaticamente as declarações de constante na ordem apropriada. No exemplo a seguir, o compilador primeiro avalia `Y`, em seguida, `Z`e, finalmente, `X`, produzindo os valores 10, 11 e 12, respectivamente.

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

Quando um nome simbólico para um valor constante é desejado, mas o tipo do valor não é permitido em uma declaração de constante ou quando o valor não pode ser calculado em tempo de compilação por uma expressão constante, uma variável somente leitura pode ser usada em vez disso.


## <a name="instance-and-shared-variables"></a>Variáveis compartilhadas e instância

Uma instância ou uma variável compartilhada é um membro de um tipo que pode armazenar informações.

```antlr
VariableMemberDeclaration
    : Attributes? VariableModifier+ VariableDeclarators StatementTerminator
    ;

VariableModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'ReadOnly'
    | 'WithEvents'
    | 'Dim'
    ;

VariableDeclarators
    : VariableDeclarator ( Comma VariableDeclarator )*
    ;

VariableDeclarator
    : VariableIdentifiers 'As' ObjectCreationExpression
    | VariableIdentifiers ( 'As' TypeName )? ( Equals Expression )?
    ;

VariableIdentifiers
    : VariableIdentifier ( Comma VariableIdentifier )*
    ;

VariableIdentifier
    : Identifier IdentifierModifiers
    ;
```

O `Dim` modificador deve ser especificado se nenhum modificador forem especificados, mas poderá ser omitido, caso contrário. Uma única declaração de variável pode incluir múltiplos declaradores variáveis; cada variável declarador introduz uma nova instância ou um membro compartilhado.

Se um inicializador for especificado, apenas uma instância ou uma variável compartilhada pode ser declarado pela variável declarador:

```vb
Class Test
    Dim a, b, c, d As Integer = 10  ' Invalid: multiple initialization
End Class
```

Essa restrição não se aplica para inicializadores de objeto:

```vb
Class Test
    Dim a, b, c, d As New Collection() ' OK
End Class
```

Uma variável declarada com o `Shared` modificador é uma *variável compartilhada*. Uma variável compartilhada identifica exatamente um local de armazenamento, independentemente do número de instâncias do tipo que são criados. Uma variável compartilhada entra em existência quando um programa começa a ser executado e deixa de existir quando o programa é encerrado.

Uma variável compartilhada é compartilhada somente entre instâncias de um determinado tipo genérico fechado. Por exemplo, o programa:

```vb
Class C(Of V) 
    Shared InstanceCount As Integer = 0

    Public Sub New()  
        InstanceCount += 1 
    End Sub

    Public Shared ReadOnly Property Count() As Integer 
        Get
            Return InstanceCount
        End Get
    End Property
End Class

Class Application 
    Shared Sub Main() 
        Dim x1 As New C(Of Integer)()
        Console.WriteLine(C(Of Integer).Count)

        Dim x2 As New C(Of Double)() 
        Console.WriteLine(C(Of Integer).Count)

        Dim x3 As New C(Of Integer)() 
        Console.WriteLine(C(Of Integer).Count)
    End Sub
End Class
```

Imprime:

```
1
1
2
```

Uma variável declarada sem a `Shared` modificador é chamado um *variável de instância*. Cada instância de uma classe contém uma cópia separada de todas as variáveis de instância da classe. Uma variável de instância de um tipo de referência entra em existência quando uma nova instância do que o tipo é criado e deixa de existir quando não houver nenhuma referência a essa instância e o `Finalize` método foi executado. Uma variável de instância de um tipo de valor tem exatamente a mesma duração como a variável à qual ele pertence. Em outras palavras, quando uma variável de um tipo de valor entra em existência ou deixa de existir, então, faz a variável de instância do tipo de valor.

Se o declarador contiver um `As` cláusula, a cláusula Especifica o tipo dos membros apresentados pela declaração. Se o tipo for omitido e a semântica estrita está sendo usada, ocorrerá um erro de tempo de compilação. Caso contrário, o tipo dos membros é implicitamente `Object` ou o tipo de caractere de tipo dos membros.

__Observação.__ Não há nenhuma ambiguidade na sintaxe: se um declarador omite um tipo, ele sempre usará o tipo de um declarador a seguir.

O domínio de acessibilidade de uma instância ou o tipo da variável compartilhada ou tipo de elemento de matriz deve ser igual ou um superconjunto do domínio de acessibilidade da instância ou uma variável compartilhada em si.

A exemplo a seguir mostra uma `Color` classe que tem variáveis de instância interna denominadas `redPart`, `greenPart`, e `bluePart`:

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub
End Class
```


### <a name="read-only-variables"></a>Variáveis somente leitura

Quando uma instância ou declaração de variável compartilhada inclui um `ReadOnly` modificador, atribuições para as variáveis introduzidas pela declaração podem ocorrer somente como parte da declaração ou em um construtor na mesma classe. Especificamente, as atribuições para uma instância somente leitura ou uma variável compartilhada são permitidas somente nas seguintes situações:

* A declaração de variável que apresenta a instância ou uma variável compartilhada (com a inclusão de um inicializador de variável na declaração).

* Para uma variável de instância, nos construtores de instância da classe que contém a declaração de variável. A variável de instância só pode ser acessada de forma não qualificada ou por meio `Me` ou `MyClass`.

* Para uma variável compartilhada, no construtor da classe compartilhado que contém a declaração de variável compartilhada.

Uma variável compartilhada somente leitura é útil quando um nome simbólico para um valor constante for desejado, mas quando o tipo do valor não é permitido em uma declaração de constante ou quando o valor não pode ser calculado em tempo de compilação por uma expressão constante.

Um exemplo de como a primeira maneira tal aplicativo, na qual cor variáveis compartilhadas são declaradas `ReadOnly` para impedir que eles estão sendo alterados por outros programas:

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub 

    Public Shared ReadOnly Red As Color = New Color(&HFF, 0, 0)
    Public Shared ReadOnly Blue As Color = New Color(0, &HFF, 0)
    Public Shared ReadOnly Green As Color = New Color(0, 0, &HFF)
    Public Shared ReadOnly White As Color = New Color(&HFF, &HFF, &HFF)
End Class
```

Constantes e variáveis compartilhadas somente leitura têm semânticas diferentes. Quando uma expressão fizer referência a uma constante, o valor da constante é obtido em tempo de compilação, mas quando uma expressão fizer referência a uma variável compartilhada somente leitura, o valor da variável compartilhada não é obtido até o tempo de execução. Considere o aplicativo a seguir, que consiste em dois programas separados.

File1.vb:

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

File2.vb:

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

Os namespaces `Program1` e `Program2` denotam os dois programas que são compilados separadamente. Como variável `Program1.Utils.X` é declarado como `Shared ReadOnly`, a saída de valor a `Console.WriteLine` instrução não é conhecida em tempo de compilação, mas em vez disso, é obtida em tempo de execução. Portanto, se o valor de `X` é alterada e `Program1` é recompilado, o `Console.WriteLine` instrução produzirá o novo valor, mesmo se `Program2` não é recompilado. No entanto, se `X` tivesse sido uma constante, o valor da `X` seria obtido no momento `Program2` foi compilado e estaria afetado pelas alterações na `Program1` até `Program2` foi recompilado.

### <a name="withevents-variables"></a>Variáveis WithEvents

Um tipo pode declarar que manipula um conjunto de eventos gerados por um de sua instância ou a variáveis compartilhadas declarando a instância ou uma variável compartilhada que gera os eventos com o `WithEvents` modificador. Por exemplo:

```vb
Class Raiser
    Public Event E1()

    Public Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Public Sub Main()
        x = New Raiser()
    End Sub
End Module
```

Neste exemplo, o método `E1Handler` manipula o evento `E1` que é gerado pela instância do tipo `Raiser` armazenado na variável de instância `x`.

O `WithEvents` modificador faz com que a variável a ser renomeado com um sublinhado à esquerda e substituído por uma propriedade o mesmo nome que faz o vínculo de evento. Por exemplo, se for o nome da variável `F`, ele é renomeado para `_F` e uma propriedade `F` é declarado implicitamente. Se houver uma colisão entre o nome da variável nova e outra declaração, será relatado um erro de tempo de compilação. Todos os atributos aplicados à variável são transferidos para a variável renomeada.

A propriedade implícita criada por um `WithEvents` declaração se encarrega de interceptação e unhooking os manipuladores de eventos relevantes. Quando um valor é atribuído à variável, a propriedade primeiro chama o `remove` método para o evento na instância atualmente na variável (unhooking o manipulador de eventos existente, se houver). Em seguida, a atribuição é feita e a propriedade chama o `add` método para o evento na nova instância da variável (conectar o novo manipulador de eventos). O código a seguir é equivalente ao código acima para o módulo padrão `Test`:

```vb
Module Test
    Private _x As Raiser

    Public Property x() As Raiser
        Get
            Return _x
        End Get

        Set (Value As Raiser)
            ' Unhook any existing handlers.
            If _x IsNot Nothing Then
                RemoveHandler _x.E1, AddressOf E1Handler
            End If

            ' Change value.
            _x = Value

            ' Hook-up new handlers.
            If _x IsNot Nothing Then
                AddHandler _x.E1, AddressOf E1Handler
            End If
        End Set
    End Property

    Sub E1Handler()
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
    End Sub
End Module
```

Não é válido para declarar uma instância ou uma variável compartilhada como `WithEvents` se a variável é tipada como uma estrutura. Além disso, `WithEvents` não pode ser especificado em uma estrutura, e `WithEvents` e `ReadOnly` não podem ser combinados.

### <a name="variable-initializers"></a>Inicializadores de variável

Instância e declarações de variável compartilhadas em classes e instância declarações de variável (mas declarações de variável não compartilhadas) em estruturas podem incluir inicializadores de variável. Para `Shared` variáveis, inicializadores de variável correspondem a instruções de atribuição que são executadas depois que o programa é iniciado, mas antes de `Shared` variável é referenciada pela primeira vez. Por exemplo, variáveis de inicializadores de variável correspondem às instruções de atribuição que são executadas quando uma instância da classe é criada. Estruturas não podem ter inicializadores de variável de instância porque seus construtores sem parâmetros não podem ser modificados.

Considere o exemplo a seguir:

```vb
Class Test
    Public Shared x As Double = Math.Sqrt(2.0)
    Public i As Integer = 100
    Public s As String = "Hello"
End Class

Module TestModule
    Sub Main()
        Dim a As New Test()

        Console.WriteLine("x = " & Test.x & ", i = " & a.i & ", s = " & a.s)
    End Sub
End Module
```

O exemplo produz a seguinte saída:

```
x = 1.4142135623731, i = 100, s = Hello
```

Uma atribuição para `x` ocorre quando a classe é carregada e as atribuições a `i` e `s` ocorrem quando uma nova instância da classe é criada.

É útil pensar em inicializadores de variável como instruções de atribuição que são inseridas automaticamente no bloco de construtor do tipo. O exemplo a seguir contém várias inicializadores de variável de instância.

```vb
Class A
    Private x As Integer = 1
    Private y As Integer = -1
    Private count As Integer

    Public Sub New()
        count = 0
    End Sub

    Public Sub New(n As Integer)
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double = Math.Sqrt(2.0)
    Private items As ArrayList = New ArrayList(100)
    Private max As Integer

    Public Sub New()
        Me.New(100)
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1)
        max = n
    End Sub
End Class
```

O exemplo corresponde ao código mostrado abaixo, onde cada comentário indica que uma instrução inserida automaticamente.

```vb
Class A
    Private x, y, count As Integer

    Public Sub New()
        MyBase.New ' Invoke object() constructor.
        x = 1 ' This is a variable initializer.
        y = -1 ' This is a variable initializer.
        count = 0
    End Sub

    Public Sub New(n As Integer)
        MyBase.New ' Invoke object() constructor. 
        x = 1 ' This is a variable initializer.
        y = - 1 ' This is a variable initializer.
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double
    Private items As ArrayList
    Private max As Integer

    Public Sub New()
        Me.New(100) 
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1) 
        sqrt2 = Math.Sqrt(2.0) ' This is a variable initializer.
        items = New ArrayList(100) ' This is a variable initializer.
        max = n
    End Sub
End Class
```

Todas as variáveis são inicializadas como o valor padrão de seu tipo antes de qualquer variável inicializadores são executados. Por exemplo:

```vb
Class Test
    Public Shared b As Boolean
    Public i As Integer
End Class

Module TestModule
    Sub Main()
        Dim t As New Test()
        Console.WriteLine("b = " & Test.b & ", i = " & t.i)
    End Sub
End Module
```

Porque `b` é inicializado automaticamente para seu valor padrão quando a classe é carregada e `i` é automaticamente inicializado com seu valor padrão quando uma instância da classe é criada, o código anterior produz a saída a seguir:

```
b = False, i = 0
```

Cada inicializador de variável deve produzir um valor de tipo de variável ou de um tipo que é implicitamente conversível para o tipo da variável. Um inicializador de variável pode ser circular ou fazer referência a uma variável que será inicializada depois dele, nesse caso o valor da variável referenciada é seu valor padrão para os fins do inicializador. Tal um inicializador é de valor duvidosa.

Há três formas de inicializadores de variável: inicializadores regulares, inicializadores de tamanho da matriz e inicializadores de objeto. Os primeiros dois formulários aparecem após um sinal de igual que segue o nome de tipo, os dois últimos são parte da declaração em si. Somente um formulário de inicializador pode ser usado em qualquer declaração em particular.

#### <a name="regular-initializers"></a>Inicializadores regulares

Um *inicializador regular* é uma expressão que é implicitamente conversível para o tipo da variável. Aparece após um sinal de igual que segue o nome do tipo e deve ser classificado como um valor. Por exemplo:

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

Este programa produz a saída:

```vb
x = 10, y = 20
```

Se uma declaração de variável tiver um inicializador regular, apenas uma única variável pode ser declarada por vez. Por exemplo:

```vb
Module Test
    Sub Main()
        ' OK, only one variable declared at a time.
        Dim x As Integer = 10, y As Integer = 20

        ' Error: Can't initialize multiple variables at once.
        Dim a, b As Integer = 10
    End Sub
End Module
```

#### <a name="object-initializers"></a>Inicializadores de objeto

Uma *inicializador de objeto* é especificado usando uma expressão de criação de objeto no lugar do nome do tipo. Um inicializador de objeto é equivalente a um inicializador regular de atribuir o resultado da expressão de criação de objeto para a variável. So

```vb
Module TestModule
    Sub Main()
        Dim x As New Test(10)
    End Sub
End Module
```

equivale a

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test(10)
    End Sub
End Module
```

Os parênteses em um inicializador de objeto é sempre interpretado como a lista de argumentos para o construtor e nunca como modificadores de tipo de matriz. Um nome de variável com um inicializador de objeto não pode ter um modificador de tipo de matriz ou um modificador de tipo anulável.

#### <a name="array-size-initializers"></a>Inicializadores de tamanho da matriz

Uma *inicializador de tamanho da matriz* é um modificador no nome da variável que oferece um conjunto de limites superiores de dimensão indicada por expressões.

```antlr
ArraySizeInitializationModifier
    : OpenParenthesis BoundList CloseParenthesis ArrayTypeModifiers?
    ;

BoundList
    : Bound ( Comma Bound )*
    ;

Bound
    : Expression
    | '0' 'To' Expression
    ;
```

As expressões de limite superior deve ser classificadas como valores e deve ser implicitamente conversíveis para `Integer`. O conjunto de limites superiores é equivalente a um inicializador de variável de uma expressão de criação de matriz com os limites superiores determinados. O número de dimensões do tipo de matriz é inferido do inicializador de matriz de tamanho. So

```vb
Module Test
    Sub Main()
        Dim x(5, 10) As Integer
    End Sub
End Module
```

equivale a

```vb
Module Test
    Sub Main()
        Dim x As Integer(,) = New Integer(5, 10) {}
    End Sub
End Module
```

Todos os limites superiores do deve ser igual ou maior que -1 e todas as dimensões devem ter um limite superior especificado. Se o tipo de elemento da matriz que está sendo inicializado em si for um tipo de matriz, os modificadores de tipo de matriz ir à direita do inicializador de tamanho da matriz. Por exemplo

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

declara uma variável local `x` cujo tipo é uma matriz bidimensional de matrizes tridimensionais de `Integer`inicializado para uma matriz com os limites do `0..5` na primeira dimensão e `0..10` na segunda dimensão. Não é possível usar um inicializador de tamanho de matriz para inicializar os elementos de uma variável cujo tipo é uma matriz de matrizes.

Uma declaração de variável com um inicializador de tamanho da matriz não pode ter um modificador de tipo de matriz em seu tipo ou um inicializador regular.


### <a name="systemmarshalbyrefobject-classes"></a>Classes de System. MarshalByRefObject

As classes que derivam da classe `System.MarshalByRefObject` têm o marshaling realizado entre limites de contexto usando proxies (isto é, por referência) e não por meio de cópia (ou seja, por valor). Isso significa que uma instância dessa classe não pode ser uma instância de true, mas em vez disso, pode ser apenas um stub que realiza marshaling de variável acessa e chama o método de um limite de contexto.

Como resultado, não é possível criar uma referência para o local de armazenamento de variáveis definidas em tais classes. Isso significa que as variáveis digitadas como classes derivadas de `System.MarshalByRefObject` não pode ser passado para fazer referência a parâmetros e métodos e variáveis de variáveis, digitadas como tipos de valor não podem ser acessados. Em vez disso, o Visual Basic trata as variáveis definidas em tais classes como se fossem propriedades (já que as restrições são os mesmos em Propriedades).

Há uma exceção a essa regra: um membro implicitamente ou explicitamente qualificado com `Me` é isento das restrições acima, pois `Me` sempre é garantido para ser um objeto real, não um proxy.

## <a name="properties"></a>Propriedades

*Propriedades* são uma extensão natural das variáveis; elas são denominadas membros com tipos associados e a sintaxe para acessar variáveis e propriedades é o mesmo. Ao contrário de variáveis, no entanto, as propriedades não denotam locais de armazenamento. Em vez disso, as propriedades têm *acessadores*, que especifica as instruções a executar para ler ou gravar seus valores.

Propriedades são definidas com declarações de propriedade. A primeira parte de uma declaração de propriedade é semelhante a uma declaração de campo. A segunda parte inclui um `Get` acessador e/ou um `Set` acessador.

```antlr
PropertyMemberDeclaration
    : RegularPropertyMemberDeclaration
    | MustOverridePropertyMemberDeclaration
    | AutoPropertyMemberDeclaration
    ;

PropertySignature
    : 'Property'
      Identifier ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

RegularPropertyMemberDeclaration
    : Attributes? PropertyModifier* PropertySignature
      ImplementsClause? LineTerminator
      PropertyAccessorDeclaration+
      'End' 'Property' StatementTerminator
    ;

MustOverridePropertyMemberDeclaration
    : Attributes? MustOverridePropertyModifier+ PropertySignature
      ImplementsClause? StatementTerminator
    ;

AutoPropertyMemberDeclaration
    : Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )? ( Equals Expression )?
      ImplementsClause? LineTerminator
    | Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      'As' Attributes? 'New'
      ( NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )? )?
      ObjectCreationExpressionInitializer?
      ImplementsClause? LineTerminator
    ;

InterfacePropertyMemberDeclaration
    : Attributes? InterfacePropertyModifier* PropertySignature StatementTerminator
    ;

AutoPropertyModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'Overridable'
    | 'NotOverridable'
    | 'Overrides'
    | 'Overloads'
    ;

PropertyModifier
    : AutoPropertyModifier
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    | 'Iterator'
    ;

MustOverridePropertyModifier
    : PropertyModifier
    | 'MustOverride'
    ;

InterfacePropertyModifier
    : 'Shadows'
    | 'Overloads'
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    ;

PropertyAccessorDeclaration
    : PropertyGetDeclaration
    | PropertySetDeclaration
    ;
```

No exemplo a seguir, o `Button` classe define um `Caption` propriedade.

```vb
Public Class Button
    Private captionValue As String

    Public Property Caption() As String
        Get
            Return captionValue
        End Get

        Set (Value As String)
            captionValue = value
            Repaint()
        End Set
    End Property

    ...
End Class
```

Com base nas `Button` classe acima, o seguinte é um exemplo de uso do `Caption` propriedade:

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

Aqui, o `Set` acessador é invocado, atribuindo um valor para a propriedade e o `Get` acessador é invocado por fazer referência à propriedade em uma expressão.

Se nenhum tipo é especificado para uma propriedade e a semântica estrita está sendo usada, ocorre um erro de tempo de compilação; Caso contrário, o tipo da propriedade é implicitamente `Object` ou o tipo de caractere de tipo da propriedade. Uma declaração de propriedade pode conter um `Get` acessador, que recupera o valor da propriedade, um `Set` acessador, que armazena o valor da propriedade, ou ambos. Porque uma propriedade declara implicitamente métodos, uma propriedade pode ser declarada com os mesmos modificadores como um método. Se a propriedade é definida em uma interface ou definida com o `MustOverride` modificador, o corpo de propriedade e o `End` constructo deve ser omitido; caso contrário, ocorrerá um erro de tempo de compilação.

A lista de parâmetros de índice compõe a assinatura da propriedade, portanto, as propriedades podem ser sobrecarregadas em parâmetros de índice, mas não no tipo da propriedade. A lista de parâmetros de índice é da mesma maneira que um método regular. No entanto, nenhum dos parâmetros podem ser modificadas com o `ByRef` modificador e nenhum deles pode ser nomeado `Value` (que é reservado para o parâmetro de valor implícito no `Set` acessador).

Uma propriedade pode ser declarada da seguinte maneira:

* Se a propriedade não especifica nenhum modificador de tipo de propriedade, a propriedade deve ter uma `Get` acessador e um `Set` acessador. A propriedade deve ser uma propriedade de leitura / gravação.

* Se a propriedade especifica o `ReadOnly` modificador, a propriedade deve ter um `Get` acessador e não pode ter um `Set` acessador. A propriedade deve ser de propriedade somente leitura. Ele é um erro de tempo de compilação para uma propriedade somente leitura ser o destino de uma atribuição.

* Se a propriedade especifica o `WriteOnly` modificador, a propriedade deve ter um `Set` acessador e não pode ter um `Get` acessador. A propriedade deve ser de propriedade somente gravação. Ele é um erro de tempo de compilação para fazer referência a uma propriedade somente gravação em uma expressão, exceto como o destino de uma atribuição ou como um argumento para um método.

O `Get` e `Set` acessadores de uma propriedade não são membros diferentes e não é possível declarar os acessadores de uma propriedade separadamente. O exemplo a seguir não declara uma única propriedade de leitura / gravação. Em vez disso, ele declara duas propriedades com o mesmo nome, um somente leitura e somente gravação:

```vb
Class A
    Private nameValue As String

    ' Error, contains a duplicate member name.
    Public ReadOnly Property Name() As String 
        Get
            Return nameValue
        End Get
    End Property

    ' Error, contains a duplicate member name.
    Public WriteOnly Property Name() As String 
        Set (Value As String)
            nameValue = value
        End Set
    End Property
End Class
```

Como dois membros declarados na mesma classe não podem ter o mesmo nome, o exemplo causa um erro de tempo de compilação.

Por padrão, a acessibilidade de uma propriedade `Get` e `Set` acessadores é o mesmo que a acessibilidade da propriedade em si. No entanto, o `Get` e `Set` acessadores também podem especificar a acessibilidade separadamente da propriedade. Nesse caso, a acessibilidade de um acessador deve ser mais restritiva do que a acessibilidade da propriedade e apenas um acessador pode ter um nível de acessibilidade diferente da propriedade. Tipos de acesso são considerados mais ou menos restritivos da seguinte maneira:

* `Private` é mais restritivo do que `Public`, `Protected Friend`, `Protected`, ou `Friend`.

* `Friend` é mais restritivo do que `Protected Friend` ou `Public`.

* `Protected` é mais restritivo do que `Protected Friend` ou `Public`.

* `Protected Friend` é mais restritivo que `Public`.

Quando um dos acessadores da propriedade é acessível, mas o outro não é, a propriedade é tratada como se fosse somente leitura ou somente gravação. Por exemplo:

```vb
Class A
    Public Property P() As Integer
        Get
            ...
        End Get

        Private Set (Value As Integer)
            ...
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim a As A = New A()

        ' Error: A.P is read-only in this context.
        a.P = 10
    End Sub
End Module
```

Quando um tipo derivado é sombra de uma propriedade, a propriedade derivada oculta a propriedade sombreada em relação à leitura e gravação. No exemplo a seguir, o `P` propriedade na `B` oculta a `P` propriedade no `A` em relação à leitura e gravação:

```vb
Class A
    Public WriteOnly Property P() As Integer
        Set (Value As Integer)
        End Set
    End Property
End Class

Class B
    Inherits A

    Public Shadows ReadOnly Property P() As Integer
       Get
       End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As B = New B

        B.P = 10     ' Error, B.P is read-only.
    End Sub
End Module
```

O domínio de acessibilidade do tipo de retorno ou tipos de parâmetro deve ser igual ou um superconjunto do domínio de acessibilidade da propriedade em si. Uma propriedade só pode ter uma `Set` acessador e um `Get` acessador.

Exceto pelas diferenças na sintaxe de declaração e chamada `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, e `MustInherit` propriedades se comportam exatamente como `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, e `MustInherit` métodos. Quando uma propriedade é substituída, a propriedade de substituição deve ser do mesmo tipo (leitura-gravação, somente leitura, somente gravação). Uma `Overridable` propriedade não pode conter um `Private` acessador.

No exemplo a seguir `X` é um `Overridable` propriedade somente leitura, `Y` é uma `Overridable` leitura / gravação de propriedade, e `Z` é um `MustOverride` propriedade de leitura / gravação.

```vb
MustInherit Class A
    Private _y As Integer

    Public Overridable ReadOnly Property X() As Integer
        Get
            Return 0
        End Get
    End Property

    Public Overridable Property Y() As Integer
        Get
            Return _y
         End Get
        Set (Value As Integer)
            _y = value
        End Set
    End Property

    Public MustOverride Property Z() As Integer
End Class
```

Porque `Z` está `MustOverride`, o que contém a classe `A` deve ser declarado `MustInherit`.

Por outro lado, uma classe que deriva da classe `A` é mostrado abaixo:

```vb
Class B
    Inherits A

    Private _z As Integer

    Public Overrides ReadOnly Property X() As Integer
        Get
            Return MyBase.X + 1
        End Get
    End Property

    Public Overrides Property Y() As Integer
        Get
            Return MyBase.Y
        End Get
        Set (Value As Integer)
            If value < 0 Then
                MyBase.Y = 0
            Else
                MyBase.Y = Value
            End If
        End Set
    End Property

    Public Overrides Property Z() As Integer
        Get
            Return _z
        End Get
        Set (Value As Integer)
            _z = Value
        End Set
    End Property
End Class
```

Aqui, as declarações de propriedades `X`,`Y`, e `Z` substituir as propriedades de base. Cada declaração de propriedade corresponde exatamente os modificadores de acessibilidade, o tipo e o nome da propriedade herdada correspondente. O `Get` acessador de propriedade `X` e o `Set` acessador de propriedade `Y` usam o `MyBase` palavra-chave para acessar as propriedades herdadas. A declaração de propriedade `Z` substitui o `MustOverride` propriedade – assim, há não pendentes `MustOverride` membros na classe `B`, e `B` tem permissão para ser uma classe regular.

Propriedades podem ser usadas para atrasar a inicialização de um recurso até o momento em que ele é referenciado pela primeira vez. Por exemplo:

```vb
Imports System.IO

Public Class ConsoleStreams
    Private Shared reader As TextReader
    Private Shared writer As TextWriter
    Private Shared errors As TextWriter

    Public Shared ReadOnly Property [In]() As TextReader
        Get
            If reader Is Nothing Then
                reader = Console.In
            End If
            Return reader
        End Get
    End Property

    Public Shared ReadOnly Property Out() As TextWriter
        Get
            If writer Is Nothing Then
                writer = Console.Out
            End If
            Return writer
        End Get
    End Property

    Public Shared ReadOnly Property [Error]() As TextWriter
        Get
            If errors Is Nothing Then
                errors = Console.Error
            End If
            Return errors
        End Get
    End Property
End Class
```

O `ConsoleStreams` classe contém três propriedades: `In`, `Out`, e `Error`, que representam a entrada padrão, saída e dispositivos de erro, respectivamente. Ao expor esses membros como propriedades, o `ConsoleStreams` classe pode atrasar sua inicialização até que eles são realmente usados. Por exemplo, após a primeira referência a `Out` propriedade, como na `ConsoleStreams.Out.WriteLine("hello, world")`, subjacente `TextWriter` para o dispositivo de saída é inicializado. Porém, se o aplicativo não faz referência à `In` e `Error` propriedades, em seguida, não há objetos são criados para esses dispositivos.


### <a name="get-accessor-declarations"></a>Obter declarações de acessador

Um `Get` acessador (getter) é declarado por meio de uma propriedade `Get` declaração. Uma propriedade `Get` declaração consiste na palavra-chave `Get` seguido de um bloco de instruções. Dada uma propriedade chamada `P`, um `Get` declaração do acessador declara implicitamente um método com o nome `get_P` com o mesmos modificadores, o tipo e a lista de parâmetros como a propriedade. Se o tipo contém uma declaração com esse nome, um erro de tempo de compilação, mas a declaração implícita é ignorada para fins de associação de nome.

Uma variável local especial, que é declarada implicitamente no `Get` espaço de declaração do corpo do acessador com o mesmo nome que a propriedade representa o valor retornado da propriedade. A variável local tem semântica de resolução de nome especial quando usadas em expressões. Se a variável local é usada em um contexto que espera uma expressão que é classificada como um grupo de método, como uma expressão de invocação, em seguida, o nome é resolvido para a função em vez da variável local. Por exemplo:

```vb
ReadOnly Property F(i As Integer) As Integer
    Get
        If i = 0 Then
            F = 1    ' Sets the return value.
        Else
            F = F(i - 1) ' Recursive call.
        End If
    End Get
End Property
```

O uso de parênteses pode causar situações ambíguas (como `F(1)` onde `F` é uma propriedade cujo tipo é uma matriz unidimensional). Em todas as situações ambíguas, o nome é resolvido para a propriedade em vez da variável local. Por exemplo:

```vb
ReadOnly Property F(i As Integer) As Integer()
    Get
        If i = 0 Then
            F = new Integer(2) { 1, 2, 3 }
        Else
            F = F(i - 1) ' Recursive call, not index.
        End If
    End Get
End Property
```

Quando o controle deixa de fluxo de `Get` corpo do acessador, o valor da variável local é passado para a expressão de invocação. Como invocar um `Get` acessador é conceitualmente equivalente ao ler o valor de uma variável, ele é considerado um estilo ruim de programação para `Get` acessadores para ter efeito colateral observável, conforme ilustrado no exemplo a seguir:

```vb
Class Counter
    Private Value As Integer

    Public ReadOnly Property NextValue() As Integer
        Get
            Value += 1
            Return Value
        End Get
    End Property
End Class
```

O valor da `NextValue` propriedade depende do número de vezes que a propriedade foi acessada anteriormente. Assim, acessar a propriedade produz um efeito colateral observável, e a propriedade em vez disso, deve ser implementada como um método.

A convenção de "sem efeitos colaterais" para `Get` acessadores não significa que `Get` acessadores sempre devem ser escritos para simplesmente retornar valores armazenados em variáveis. Na verdade, `Get` acessadores geralmente calcular o valor de uma propriedade ao acessar diversas variáveis ou invocar métodos. No entanto, projetado corretamente `Get` acessador não executará nenhuma ação que causam alterações observáveis no estado do objeto.

__Observação.__ `Get` acessadores têm a mesma restrição no posicionamento de linha que tem as sub-rotinas. A instrução de início, a instrução end e o bloco devem aparecer no início de uma linha lógica.

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a>Declarações de acessador de conjunto

Um `Set` acessador (setter) é declarado usando uma declaração de propriedade de conjunto. Uma declaração de propriedade de conjunto consiste na palavra-chave `Set`, uma lista de parâmetros opcional e um bloco de instruções. Dada uma propriedade chamada `P`, uma declaração de setter declara implicitamente um método com o nome `set_P` com o mesmos modificadores e lista de parâmetros como a propriedade. Se o tipo contém uma declaração com esse nome, um erro de tempo de compilação, mas a declaração implícita é ignorada para fins de associação de nome.

Se uma lista de parâmetro for especificada, ele deve ter um membro, esse membro não deve ter nenhum modificador exceto `ByVal`, e seu tipo deve ser o mesmo que o tipo da propriedade. O parâmetro representa o valor da propriedade que está sendo definido. Se o parâmetro for omitido, um parâmetro chamado `Value` é declarado implicitamente.

__Observação.__ `Set` acessadores têm a mesma restrição no posicionamento de linha que tem as sub-rotinas. A instrução de início, a instrução end e o bloco devem aparecer no início de uma linha lógica.

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a>Propriedades padrão

Uma propriedade que especifica o modificador `Default` é chamado de um *propriedade padrão*. Qualquer tipo que permite que as propriedades pode ter uma propriedade padrão, incluindo interfaces. A propriedade padrão pode ser referenciada sem precisar qualificar a instância com o nome da propriedade. Assim, dada a classe

```vb
Class Test
    Public Default ReadOnly Property Item(i As Integer) As Integer
        Get
            Return i
        End Get
    End Property
End Class
```

O código

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x(10)
    End Sub
End Module
```

equivale a

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x.Item(10)
    End Sub
End Module
```

Depois que uma propriedade é declarada `Default`, todas as propriedades sobrecarregadas nesse nome na hierarquia de herança tornam-se a propriedade padrão, se eles foram declarados `Default` ou não. Declarar uma propriedade `Default` em uma classe derivada quando a classe base declarar uma propriedade padrão por outro nome não exige qualquer outros modificadores, como `Shadows` ou `Overrides`. Isso é porque a propriedade padrão não tem nenhuma identidade ou assinatura e, portanto, não pode ser sombreado ou sobrecarregado. Por exemplo:

```vb
Class Base
    Public ReadOnly Default Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Base = " & i)
        End Get
    End Property
End Class

Class Derived
    Inherits Base

    ' This hides Item, but does not change the default property.
    Public Shadows ReadOnly Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Derived = " & i)
        End Get
    End Property
End Class

Class MoreDerived
    Inherits Derived

    ' This declares a new default property, but not Item.
    ' This does not need to be declared Shadows
    Public ReadOnly Default Property Value(i As Integer) As Integer
        Get
            Console.WriteLine("MoreDerived = " & i)
        End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = New MoreDerived()
        Dim y As Integer
        Dim z As Derived = x

        y = x(10)        ' Calls MoreDerived.Value.
        y = x.Item(10)   ' Calls Derived.Item
        y = z(10)        ' Calls Base.Item
    End Sub
End Module
```

Este programa produzirá a saída:

```
MoreDerived = 10
Derived = 10
Base = 10
```

Todas as propriedades padrão declaradas dentro de um tipo deve ter o mesmo nome e, para maior clareza, deve especificar o `Default` modificador. Como uma propriedade padrão sem parâmetros de índice resultaria em uma situação ambígua durante a atribuição de instâncias da classe recipiente, as propriedades padrão devem ter parâmetros de índice. Além disso, se uma propriedade sobrecarregada em um determinado nome inclui o `Default` modificador, todas as propriedades sobrecarregadas nesse nome deverá especificá-lo. As propriedades padrão não podem ser `Shared`, e não deve ser pelo menos um acessador da propriedade `Private`.

### <a name="automatically-implemented-properties"></a>Propriedades implementadas automaticamente

Se uma propriedade omite a declaração de qualquer acessadores, uma implementação da propriedade será fornecida automaticamente, a menos que a propriedade é declarada em uma interface ou é declarada `MustOverride`. Somente as propriedades de leitura/gravação sem argumentos podem ser implementadas automaticamente; Caso contrário, ocorrerá um erro de tempo de compilação.

Uma propriedade implementada automaticamente `x`, até mesmo uma substituição de outra propriedade, que apresenta uma variável local privada `_x` com o mesmo tipo da propriedade. Se houver uma colisão entre o nome da variável local e outra declaração, será relatado um erro de tempo de compilação. A propriedade implementada automaticamente `Get` acessador retorna o valor de local e a propriedade `Set` acessador que define o valor do local. Por exemplo, a declaração:

```vb
Public Property x() As Integer
```

É aproximadamente equivalente a:

```vb
Private _x As Integer
Public Property x() As Integer
    Get
        Return _x
    End Get
    Set (value As Integer)
        _x = value
    End Set
End Property
```

Assim como acontece com declarações de variável, uma propriedade implementada automaticamente pode incluir um inicializador. Por exemplo:

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

__Observação.__ Quando uma propriedade implementada automaticamente é inicializada, ele é inicializado por meio da propriedade, não o campo subjacente. Isso é então substituindo propriedades pode interceptar a inicialização, se necessário.

Inicializadores de matriz são permitidos em propriedades implementadas automaticamente, exceto que não há nenhuma maneira de especificar os limites da matriz explicitamente.  Por exemplo:

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a>Propriedades de iterador

Uma *propriedade de iterador* é uma propriedade com o `Iterator` modificador. Ele é usado pelo mesmo motivo que um método iterador (seção [métodos de iterador](statements.md#iterator-methods)) é usado, como uma maneira conveniente para gerar uma sequência, que podem ser consumidos pelo `For Each` instrução. O `Get` acessador de propriedade de um iterador é interpretado da mesma forma como um método iterador.

A propriedade de um iterador deve ter um explícito `Get` acessador e seu tipo devem ser `IEnumerator`, ou `IEnumerable`, ou `IEnumerator(Of T)` ou `IEnumerable(Of T)` para alguns `T`.

Aqui está um exemplo de uma propriedade de iterador:

```vb
Class Family
    Property Daughters As New List(Of String) From {"Beth", "Diane"}
    Property Sons As New List(Of String) From {"Abe", "Carl"}

    ReadOnly Iterator Property Children As IEnumerable(Of String)
        Get
            For Each name In Daughters : Yield name : Next
            For Each name In Sons : Yield name : Next
        End Get
    End Property
End Class

Module Module1
    Sub Main()
        Dim x As New Family
        For Each c In x.Children
            Console.WriteLine(c) ' prints Beth, Diane, Abe, Carl
        Next
    End Sub
End Module
```

## <a name="operators"></a>Operadores

*Operadores* são métodos que definem o significado de um operador existente do Visual Basic para a classe continente. Quando o operador é aplicado à classe em uma expressão, o operador é compilado em uma chamada para o método de operador definido na classe. Definindo um operador para uma classe é também conhecido como *sobrecarga* o operador.

```antlr
OperatorDeclaration
    : Attributes? OperatorModifier* 'Operator' OverloadableOperator
      OpenParenthesis ParameterList CloseParenthesis
      ( 'As' Attributes? TypeName )? LineTerminator
      Block?
      'End' 'Operator' StatementTerminator
    ;

OperatorModifier
    : 'Public' | 'Shared' | 'Overloads' | 'Shadows' | 'Widening' | 'Narrowing'
    ;

OverloadableOperator
    : '+' | '-' | '*' | '/' | '\\' | '&' | 'Like' | 'Mod' | 'And' | 'Or' | 'Xor'
    | '^' | '<' '<' | '>' '>' | '=' | '<' '>' | '>' | '<' | '>' '=' | '<' '='
    | 'Not' | 'IsTrue' | 'IsFalse' | 'CType'
    ;
```

Não é possível sobrecarregar um operador que já existe; Na prática, isso principalmente se aplica aos operadores de conversão. Por exemplo, não é possível sobrecarregar a conversão de uma classe derivada em uma classe base:

```vb
Class Base
End Class

Class Derived
    ' Cannot redefine conversion from Derived to Base,
    ' conversion will be ignored.
    Public Shared Widening Operator CType(s As Derived) As Base
        ...
    End Operator
End Class
```

Operadores também podem ser sobrecarregados no sentido comum da palavra:

```vb
Class Base
    Public Shared Widening Operator CType(b As Base) As Integer
        ...
    End Operator

    Public Shared Narrowing Operator CType(i As Integer) As Base
        ...
    End Operator
End Class
```

Declarações de operador não adicione explicitamente nomes ao espaço de declaração do tipo recipiente. No entanto, eles declarar implicitamente um método correspondente, começando com os caracteres "op_". As seções a seguir listam os nomes de método correspondentes com cada operador.

Há três classes de operadores que podem ser definidos: unário operadores, operadores binários e operadores de conversão. Todas as declarações de operador compartilham algumas restrições:

* Declarações do operador devem ser sempre `Public` e `Shared`. O `Public` modificador pode ser omitido em contextos em que o modificador será assumido.

* Os parâmetros de um operador não podem ser declarados `ByRef`, `Optional` ou `ParamArray`.

* O tipo de pelo menos um dos operandos ou o valor de retorno deve ser o tipo que contém o operador.

* Não há nenhuma variável de retorno de função definida para operadores. Portanto, o `Return` declaração deve ser usada para retornar valores de um corpo de operador.

A única exceção a essas restrições se aplica a tipos de valor anuláveis. Como tipos de valor anuláveis não possuem uma definição de tipo real, um tipo de valor pode declarar operadores definidos pelo usuário para a versão que permite valor nula do tipo. Ao determinar se um tipo pode declarar um operador definido pelo usuário específico, o `?` modificadores são descartados pela primeira vez em todos os tipos envolvidos na declaração para fins de verificação de validade. Essa atenuação não se aplica ao tipo de retorno do `IsTrue` e `IsFalse` operadores; eles ainda devem retornar `Boolean`, e não `Boolean?`.

Precedência e associatividade do operador não podem ser modificados por uma declaração do operador.

__Observação.__ Os operadores têm a mesma restrição no posicionamento de linha que tem as sub-rotinas. A instrução de início, a instrução end e o bloco devem aparecer no início de uma linha lógica.


### <a name="unary-operators"></a>Operadores unários

Os seguintes operadores unários podem ser sobrecarregados:

* O operador unário de adição `+` (método correspondente: `op_UnaryPlus`)

* O operador de subtração unário `-` (método correspondente: `op_UnaryNegation`)

* A lógica `Not` operador (método correspondente: `op_OnesComplement`)

* O `IsTrue` e `IsFalse` operadores (métodos correspondentes: `op_True`, `op_False`)

Todos os operadores unários sobrecarregado deve receber um único parâmetro do tipo recipiente e pode retornar qualquer tipo, exceto para `IsTrue` e `IsFalse`, que deve retornar `Boolean`. Se o tipo recipiente for um tipo genérico, os parâmetros de tipo devem corresponder os parâmetros de tipo do tipo recipiente. Por exemplo,

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

Se um tipo de uma das sobrecargas `IsTrue` ou `IsFalse`, em seguida, ele deverá sobrecarregar o outro também. Se apenas um está sobrecarregado, um erro de tempo de compilação ocorre.

__Observação.__ `IsTrue` e `IsFalse` não são palavras reservadas.

### <a name="binary-operators"></a>Operadores binários

Os seguintes operadores binários podem ser sobrecarregados:

* A adição `+`, subtração `-`, multiplicação `*`, divisão `/`, divisão integral `\`, módulo `Mod` e a exponenciação `^` operadores (método correspondente: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)

* Os operadores relacionais `=`, `<>`, `<`, `>`, `<=`, `>=` (métodos correspondentes: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, `op_LessThanOrEqual` , `op_GreaterThanOrEqual`). __Observação.__ Enquanto o operador de igualdade pode ser sobrecarregado, o operador de atribuição (usado somente em instruções de atribuição) não pode ser sobrecarregado.

* O `Like` operador (método correspondente: `op_Like`)

* O operador de concatenação `&` (método correspondente: `op_Concatenate`)

* A lógica `And`, `Or` e `Xor` operadores (métodos correspondentes: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)

* Os operadores shift `<<` e `>>` (métodos correspondentes: `op_LeftShift`, `op_RightShift`)

Todos os operadores binários sobrecarregados devem levar o tipo recipiente como um dos parâmetros. Se o tipo recipiente for um tipo genérico, os parâmetros de tipo devem corresponder os parâmetros de tipo do tipo recipiente. Os operadores shift ainda mais restringem essa regra para exigir que o primeiro parâmetro seja do tipo recipiente; o segundo parâmetro sempre deve ser do tipo `Integer`.

Os seguintes operadores binários devem ser declarados em pares:

* Operador `=` e operador `<>`

* Operador `>` e operador `<`

* Operador `>=` e operador `<=`

Se um do par é declarado, em seguida, o outro também deve ser declarado com parâmetros e tipos de retorno correspondentes, ou ocorrerá um erro de tempo de compilação. (__Observação.__ A finalidade da exigência de emparelhado declarações de operadores relacionais é tentar garantir que pelo menos um nível mínimo de consistência lógica em operadores sobrecarregados.)

Em contraste com os operadores relacionais, a sobrecarga da divisão e operadores de divisão integral é recomendado, embora não um erro. (__Observação.__ Em geral, os dois tipos de divisão devem ser totalmente distintos: é um tipo que oferece suporte à divisão integral (nesse caso, ele deve dar suporte a `\`) ou não (nesse caso, ele deve dar suporte a `/`). Consideramos tornando um erro ao definir os dois operadores, mas porque suas linguagens em geral não fazem distinção entre dois tipos de divisão da forma faz do Visual Basic, nós achamos que era mais segura permitir que a prática, mas não recomendamos-lo.)

Operadores não podem ser sobrecarregados diretamente de atribuição composta. Em vez disso, quando o operador binário correspondente está sobrecarregado, o operador de atribuição composta usará o operador sobrecarregado. Por exemplo:

```vb
Structure Complex
    ...

    Public Shared Operator +(x As Complex, y As Complex) _
        As Complex
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim c1, c2 As Complex
        ' Calls the overloaded + operator
        c1 += c2
    End Sub
End Module
```

### <a name="conversion-operators"></a>Operadores de conversão

Operadores de conversão definem novos conversões entre tipos. Essas novas conversões são chamadas *conversões definidas pelo usuário*. Um operador de conversão converte de um tipo de fonte, indicado pelo tipo de parâmetro de operador de conversão, para um tipo de destino, indicado pelo tipo de retorno do operador de conversão. Conversões devem ser classificadas como ampliação ou redução. Uma declaração do operador de conversão que inclui o `Widening` palavra-chave introduz uma conversão de ampliação definida pelo usuário (método correspondente: `op_Implicit`). Uma declaração do operador de conversão que inclui o `Narrowing` palavra-chave introduz uma conversão redutora definidos pelo usuário (método correspondente: `op_Explicit`).

Em geral, conversões de ampliação definida pelo usuário devem ser criadas para nunca lançam exceções e perder informações. Se uma conversão definida pelo usuário pode causar exceções (por exemplo, porque o argumento de origem está fora do intervalo) ou perda de informações (por exemplo, descartando os bits de ordem superior) e, em seguida, essa conversão deve ser definida como uma conversão de estreitamento. No exemplo:

```vb
Structure Digit
    Dim value As Byte

    Public Sub New(value As Byte)
        if value < 0 OrElse value > 9 Then Throw New ArgumentException()
        Me.value = value
    End Sub

    Public Shared Widening Operator CType(d As Digit) As Byte
        Return d.value
    End Operator

    Public Shared Narrowing Operator CType(b As Byte) As Digit
        Return New Digit(b)
    End Operator
End Structure
```

a conversão de `Digit` à `Byte` é uma conversão de ampliação porque nunca gera exceções ou perde informações, mas a conversão de `Byte` ao `Digit` é uma conversão redutora desde `Digit` pode representar apenas um subconjunto de possíveis valores de um `Byte`.

Ao contrário de todos os outros membros de tipo que podem ser sobrecarregados, a assinatura de um operador de conversão inclui o tipo de destino da conversão. Isso é o único membro de tipo para o qual o tipo de retorno participa na assinatura. Ampliação ou redução de classificação de um operador de conversão, no entanto, não é parte da assinatura do operador. Portanto, uma classe ou estrutura não pode declarar um operador de conversão de ampliação e um operador de conversão de estreitamento com a mesma fonte e tipos de destino.

Um operador de conversão definida pelo usuário deve converter para ou do tipo recipiente – por exemplo, é possível que uma classe `C` para definir uma conversão de `C` para `Integer` bidirecionalmente `Integer` para `C`, mas não de `Integer` para `Boolean`. Se o tipo recipiente for um tipo genérico, os parâmetros de tipo devem corresponder os parâmetros de tipo do tipo recipiente. Além disso, não é possível redefinir uma conversão de intrínseca (ou seja, não-definido pelo usuário). Como resultado, um tipo não pode declarar uma conversão em que:

* O tipo de origem e o tipo de destino são os mesmos.

* O tipo de origem e o tipo de destino não são o tipo que define o operador de conversão.

* O tipo de origem ou o tipo de destino é um tipo de interface.

* O tipo de fonte e tipos de destino são relacionados por herança (incluindo `Object`).

A única exceção a essas regras se aplica a tipos de valor anuláveis. Como tipos de valor anuláveis não possuem uma definição de tipo real, um tipo de valor pode declarar conversões definidas pelo usuário para a versão que permite valor nula do tipo. Ao determinar se um tipo pode declarar uma conversão definida pelo usuário específica, o `?` modificadores são descartados pela primeira vez em todos os tipos envolvidos na declaração para fins de verificação de validade. Portanto, a declaração a seguir é válida porque `S` pode definir uma conversão de `S` para `T`:

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Widening Operator CType(ByVal v As S?) As T
    ...
    End Operator
End Structure
```

A declaração a seguir não é válida, no entanto, como estrutura `S` não é possível definir uma conversão de `S` para `S`:

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a>Mapeamento de operador

Porque o conjunto de operadores que dá suporte ao Visual Basic pode não coincidir com o conjunto de operadores que outras linguagens no .NET Framework, alguns operadores são mapeadas especialmente em outros operadores quando está sendo definido ou usado. Especificamente:

* Definindo um operador de divisão integral definirá automaticamente um operador de divisão regulares (utilizável apenas a partir de outras linguagens) que chamará o operador de divisão integral.

* Sobrecarregando o `Not`, `And`, e `Or` operadores serão sobrecarregar somente o operador bit a bit da perspectiva de outras linguagens que distinguir entre os operadores lógicos e bit a bit.

* Uma classe que sobrecarrega apenas os operadores lógicos em uma linguagem que faz distinção entre os operadores lógicos e bit a bit (ou seja, um que usa linguagens `op_LogicalNot`, `op_LogicalAnd`, e `op_LogicalOr` para `Not`, `And`e `Or`, respectivamente) terão seus operadores lógicos mapeados para os operadores lógicos do Visual Basic. Se os operadores lógicos e bit a bit são sobrecarregados, somente os operadores bit a bit serão usados.

* Sobrecarregando o `<<` e `>>` operadores serão sobrecarregar apenas os operadores com sinal da perspectiva de outras linguagens que distinguir entre operadores shift assinados e não assinados.

* Uma classe que sobrecarrega um operador de deslocamento não assinado somente terá o operador de deslocamento não assinado mapeado para o operador de deslocamento do Visual Basic correspondente. Se ambos os um operador shift sem sinal e assinado está sobrecarregado, somente o operador de deslocamento assinado será usado.
