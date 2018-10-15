# <a name="statements"></a>Instruções

Instruções representam o código executável.

```antlr
Statement
    : LabelDeclarationStatement
    | LocalDeclarationStatement
    | WithStatement
    | SyncLockStatement
    | EventStatement
    | AssignmentStatement
    | InvocationStatement
    | ConditionalStatement
    | LoopStatement
    | ErrorHandlingStatement
    | BranchStatement
    | ArrayHandlingStatement
    | UsingStatement
    | AwaitStatement
    | YieldStatement
    ;
```

__Observação.__ O compilador do Microsoft Visual Basic permite apenas instruções que começam com uma palavra-chave ou um identificador. Assim, por exemplo, a instrução de chamada "`Call (Console).WriteLine`"é permitido, mas a instrução de chamada"`(Console).WriteLine`" não é.

## <a name="control-flow"></a>Fluxo de controle

*Fluxo de controle* é a sequência na qual são executadas instruções e expressões. A ordem de execução depende de determinada instrução ou expressão.

Por exemplo, ao avaliar um operador de adição (seção [operador de adição](expressions.md#addition-operator)), primeiro o operando esquerdo é avaliado, em seguida, o operando à direita e, em seguida, o operador em si. Blocos são executados (seção [blocos e rótulos](statements.md#blocks-and-labels)) primeiro executando sua subdeclaração primeiro e, em seguida, continuar individualmente através das instruções do bloco.

Ordenação implícita em que isso é o conceito de um *ponto de controle*, que é a próxima operação a ser executada. Quando um método é invocado (ou "chamado"), podemos dizer que ele cria uma *instância* do método. Uma instância de método consiste em sua própria cópia dos parâmetros do método e variáveis locais e seu próprio ponto de controle.

### <a name="regular-methods"></a>Métodos regulares

Aqui está um exemplo de um método regular

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

Quando um método regular é invocado,

1. Primeiro, uma instância do método é criada específica para essa invocação. Esta instância inclui uma cópia de todos os parâmetros e variáveis locais do método.
2. Em seguida, todos os seus parâmetros são inicializados para os valores fornecidos e todas as suas variáveis locais para os valores padrão de seus tipos.
3. No caso de uma `Function`, uma variável local implícita é inicializada também chamado de *variável de retorno de função* cujo nome é o nome da função, cujo tipo é o tipo de retorno da função e cujo valor inicial é o padrão de seu tipo.
4. Ponto de controle da instância de método, em seguida, é definido na primeira instrução do corpo do método e o corpo do método imediatamente começa a executar a partir daí (seção [blocos e rótulos](statements.md#blocks-and-labels)).

Quando o fluxo de controle é encerrada o corpo do método normalmente – por meio de atingir o `End Function` ou `End Sub` que marcar seu fim, ou por meio de um explícito `Return` ou `Exit` statement - fluxo de controle retorna ao chamador da instância do método. Se houver uma variável de retorno de função, o resultado da invocação é o valor final dessa variável.

Quando o fluxo de controle sai do corpo do método por meio de uma exceção sem tratamento, essa exceção é propagada para o chamador.

Depois que o fluxo de controle foi encerrado, não há quaisquer referências ao vivo para a instância de método. Se a instância de método mantida apenas referências à sua cópia de variáveis locais ou parâmetros, eles podem ser coletadas como lixo.

### <a name="iterator-methods"></a>Métodos de iterador

Métodos de iterador são usados como uma maneira conveniente para gerar uma sequência, que podem ser consumidos pelo `For Each` instrução. Métodos de iterador que usam o `Yield` instrução (seção [instrução Yield](statements.md#yield-statement)) para fornecer elementos da sequência. (Um método iterador sem nenhum `Yield` instruções produzirá uma sequência vazia). Aqui está um exemplo de um método iterador:

```vb
Iterator Function Test() As IEnumerable(Of Integer)
    Console.WriteLine("hello")
    Yield 1
    Yield 2
End Function

Dim en = Test()
For Each x In en          ' prints "hello" before the first x
    Console.WriteLine(x)  ' prints "1" and then "2"
Next
```

Quando um método iterador é invocado cujo tipo de retorno é `IEnumerator(Of T)`,

1. Primeiro, uma instância do método iterador é criada específica para essa invocação. Esta instância inclui uma cópia de todos os parâmetros e variáveis locais do método.
2. Em seguida, todos os seus parâmetros são inicializados para os valores fornecidos e todas as suas variáveis locais para os valores padrão de seus tipos.
3. Uma variável local implícita é inicializada também chamado de *variável do iterador atual*, cujo tipo é `T` e cujo valor inicial é o padrão de seu tipo.
4. Ponto de controle da instância de método, em seguida, é definido na primeira instrução do corpo do método.
5. Uma *objeto do iterador* , em seguida, é criado, associado a esta instância de método. O objeto de iterador implementa o tipo de retorno declarado e tem um comportamento conforme descrito abaixo.
6. Fluxo de controle, em seguida, é retomado *imediatamente* no chamador e o resultado da invocação é o objeto de iterador. Observe que essa transferência é feita sem sair a instância do método iterador e não faz com que finalmente manipuladores executar. A instância de método ainda é referenciada pelo objeto de iterador, e não será lixo coletado desde que exista uma referência ao vivo para o objeto de iterador.

Quando o objeto de iterador `Current` propriedade é acessada, o *variável atual* da invocação é retornada.

Quando o objeto de iterador `MoveNext` método é invocado, a invocação não cria uma nova instância de método. Em vez disso, a instância existente do método é usada (e seu ponto de controle e parâmetros e variáveis locais) - a instância que foi criada quando o método iterador foi invocado pela primeira vez. Fluxo de controle retoma a execução no ponto de controle dessa instância de método e passa para o corpo do método iterador como de costume.

Quando o objeto de iterador `Dispose` método é invocado, novamente a instância existente do método é usada. Controlar fluxo continua do ponto de controle dessa instância de método, mas, em seguida, imediatamente se comporta como se um `Exit Function` instrução foram a próxima operação.

As descrições acima do comportamento de invocação de `MoveNext` ou `Dispose` em um objeto de iterador só se aplicam se todos os últimos invocações de `MoveNext` ou `Dispose` nesse objeto de iterador já retornaram aos seus chamadores. Se eles ainda não, o comportamento será indefinido.

Quando o fluxo de controle é encerrada o corpo do método iterador normalmente – por meio de atingir o `End Function` que marcar seu fim, ou por meio de um explícito `Return` ou `Exit` instrução – ele deve ter feito isso no contexto de uma invocação de `MoveNext` ou `Dispose` função em um objeto de iterador para retomar a instância do método iterador e ele será usa a instância de método que foi criada quando o método iterador foi invocado pela primeira vez. O ponto de controle dessa instância é deixado como o `End Function` instrução e retoma de fluxo de controle no chamador; e se ele tinha sido retomado por uma chamada para `MoveNext` , em seguida, o valor `False` é retornado ao chamador.

Quando o fluxo de controle sai o corpo do método iterador por meio de uma exceção sem tratamento e, em seguida, a exceção é propagado para o chamador, que novamente será uma invocação de `MoveNext` ou de `Dispose`.

Assim como para os outros tipos de retorno possíveis de uma função de iterador

* Quando um método iterador é invocado é cujo tipo de retorno `IEnumerable(Of T)` para alguns `T`, uma instância é criada pela primeira vez – específica para essa invocação do método iterador – de todos os parâmetros no método, e eles são inicializados com os valores fornecidos. O resultado da invocação é uma um objeto que implementa o tipo de retorno. Deve este objeto `GetEnumerator` método ser chamado, ele cria uma instância – específica para essa invocação de `GetEnumerator` – de todos os parâmetros e variáveis locais no método. Ele inicializa os parâmetros para os valores já salvados e prossegue para o método iterador acima.
* Quando um método iterador é invocado cujo tipo de retorno é a interface não genérica `IEnumerator`, o comportamento é exatamente como `IEnumerator(Of Object)`.
* Quando um método iterador é invocado cujo tipo de retorno é a interface não genérica `IEnumerable`, o comportamento é exatamente como `IEnumerable(Of Object)`.

### <a name="async-methods"></a>Métodos assíncronos

Os métodos assíncronos são uma maneira conveniente de fazer o trabalho de longa execução sem por exemplo, bloquear a interface do usuário de um aplicativo. Async é a abreviação de *Asynchronous* -isso significa que o chamador do método assíncrono retomará sua execução imediatamente, mas a eventual conclusão da instância do método assíncrono pode acontecer em um momento posterior no futuro. Por convenção, os métodos assíncronos são nomeados com o sufixo "Async".

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

__Observação.__ Os métodos assíncronos são *não* executado em um thread em segundo plano. Em vez disso, eles permitem que um método para suspender a mesmo por meio de `Await` operador e o agendamento em si para ser retomada em resposta a um evento.

Quando um método assíncrono é invocado

1. Primeiro, uma instância do método assíncrono é criada específica para essa invocação. Esta instância inclui uma cópia de todos os parâmetros e variáveis locais do método.
2. Em seguida, todos os seus parâmetros são inicializados para os valores fornecidos e todas as suas variáveis locais para os valores padrão de seus tipos.
3. No caso de um método assíncrono com o tipo de retorno `Task(Of T)` para alguns `T`, uma variável local implícita é inicializada também chamada a *variável de retorno de tarefa*, cujo tipo é `T` e cujo valor inicial é o padrão de `T`.
4. Se o método assíncrono é uma `Function` com o tipo de retorno `Task` ou `Task(Of T)` para alguns `T`, em seguida, um objeto desse tipo criadas implicitamente, associado com a invocação da atual. Isso é chamado de um *objeto async* e representa o trabalho futuro que será feito com a execução da instância do método assíncrono. Quando o controle continua no chamador desta instância de método assíncrono, o chamador recebe esse objeto async como resultado de sua invocação.
5. Ponto de controle da instância, em seguida, é definido na primeira instrução do corpo do método assíncrono e imediatamente começa a executar o corpo do método a partir daí (seção [blocos e rótulos](statements.md#blocks-and-labels)).

__Delegado de continuação e o chamador atual__

Conforme detalhado na seção [operador Await](expressions.md#await-operator), a execução de um `Await` expressão tem a capacidade de suspender o ponto de controle da instância de método deixando o fluxo de controle para ir em outro lugar. Fluxo de controle pode retomar posteriormente no ponto de controle da mesma instância por meio de invocação de um *delegado de continuação*. Observe que dessa suspensão é feita sem sair do método assíncrono e não faz com que finalmente manipuladores executar. A instância de método ainda é referenciada pelo delegado de continuação e o `Task` ou `Task(Of T)` resultar (se houver), e não será lixo coletado desde que exista uma referência ao vivo para delegar ou resultar.

Vale a pena imaginar a instrução `Dim x = Await WorkAsync()` aproximadamente como um atalho sintático para o seguinte:

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

A seguir, o *chamador atual* do método de instância é definida como o chamador original ou o chamador do delegado de continuação, o que for mais recente.

Quando o fluxo de controle é encerrada o corpo do método async – por meio de atingir o `End Sub` ou `End Function` que marcar seu fim, ou por meio de um explícito `Return` ou `Exit` instrução, ou por meio de uma exceção sem tratamento, o ponto de controle da instância é definido como o final do método. Em seguida, comportamento depende do tipo de retorno do método assíncrono.

* No caso de um `Async Function` com o tipo de retorno `Task`:
  1. Se o fluxo de controle sai por meio de uma exceção sem tratamento, em seguida, status assíncrono do objeto é definido como `TaskStatus.Faulted` e sua `Exception.InnerException` estiver definida como a exceção (exceto: determinadas exceções definido pela implementação, como `OperationCanceledException` alterá-lo para `TaskStatus.Canceled`). Retoma de fluxo de controle em que o chamador atual.
  2. Caso contrário, o status assíncrono do objeto é definido como `TaskStatus.Completed`. Retoma de fluxo de controle em que o chamador atual.

     (__Observação.__ Todo ponto de tarefa e o que torna os métodos assíncronos interessante, é que, quando uma tarefa se torna concluído e em seguida, todos os métodos que estavam aguardando ele atualmente terão seus representantes de continuação executadas, ou seja, eles se tornarão desbloqueados.)

* No caso de um `Async Function` com o tipo de retorno `Task(Of T)` para alguns `T`: o comportamento é como acima, exceto que em exceção não casos, o objeto de async `Result` propriedade também é definida como o valor final da variável de retorno de tarefa.

* No caso de um `Async Sub`:
  1. Se o fluxo de controle sai por meio de uma exceção sem tratamento, em seguida, essa exceção é propagada para o ambiente de alguma maneira de específico da implementação. Retoma de fluxo de controle em que o chamador atual.
  2. Caso contrário, o fluxo de controle simplesmente retoma no chamador atual.

#### <a name="async-sub"></a>Sub Async

Há alguns comportamentos específicos da Microsoft de um `Async Sub`.

Se `SynchronizationContext.Current` é `Nothing` no início da invocação, em seguida, todas as exceções sem tratamento de uma Async Sub serão lançadas para o pool de threads.

Se `SynchronizationContext.Current` não é `Nothing` no início da chamada, em seguida, `OperationStarted()` é invocado no contexto de antes do início do método e `OperationCompleted()` após o término. Além disso, qualquer exceção sem tratamento será lançada para ser relançada no contexto de sincronização.

Isso significa que em aplicativos de interface do usuário, para um `Async Sub` que é invocado no thread da interface do usuário, todas as exceções que ele não consegue manipular serão ser relançadas o thread de interface do usuário.

#### <a name="mutable-structures-in-async-and-iterator-methods"></a>Estruturas mutáveis em métodos assíncrono e iterador

Estrutura mutável em geral é considerada uma prática inadequada e não têm suporte pelos métodos assíncrono ou iterador. Em particular, cada invocação de um método assíncrono ou iterador em uma estrutura implicitamente opera em um *cópia* dessa estrutura que é copiada em seu momento da invocação. Assim, por exemplo,

```vb
Structure S
       Dim x As Integer
       Async Sub Mutate()
           x = 2
       End Sub
End Structure

Dim s As New S With {.x = 1}
s.Mutate()
Console.WriteLine(s.x)   ' prints "1"
```

### <a name="blocks-and-labels"></a>Blocos e rótulos

Um grupo de instruções executáveis é chamado de um bloco de instruções. Execução de um bloco de instrução começa com a primeira instrução no bloco. Após a execução de uma instrução, a próxima instrução em ordem léxica é executada, a menos que uma instrução transfere a execução em outro lugar ou ocorrerá uma exceção.

Dentro de um bloco de instrução, a divisão de instruções em linhas lógicas não é significativa com exceção de instruções de declaração de rótulo. Um rótulo é um identificador que identifica uma posição específica dentro do bloco de instrução que pode ser usado como o destino de uma instrução de ramificação, como `GoTo`.

```antlr
Block
    : Statements*
    ;

LabelDeclarationStatement
    : LabelName ':'
    ;

LabelName
    : Identifier
    | IntLiteral
    ;

Statements
    : Statement? ( ':' Statement? )*
    ;
```


Instruções de declaração de rótulo devem aparecer no início de uma linha lógica e os rótulos podem ser um identificador ou um literal inteiro. Porque as instruções de declaração de rótulo e instruções de invocação podem consistir em um único identificador, um único identificador no início de uma linha local sempre é considerado uma instrução de declaração de rótulo. Instruções de declaração de rótulo sempre devem ser seguidas por dois-pontos, mesmo se nenhuma instrução execute na mesma linha lógica.

Rótulos tenham seu próprio espaço de declaração e não interfiram com outros identificadores. O exemplo a seguir é válido e usa a variável de nome `x` como um parâmetro e um rótulo.

```vb
Function F(x As Integer) As Integer
    If x >= 0 Then
        GoTo x
    End If
    x = -x
x: 
    Return x
End Function
```

O escopo de um rótulo é o corpo do método que o contém.

Para fins de legibilidade, produções de instrução que envolvem vários substatements são tratadas como uma única produção nesta especificação, mesmo que o substatements cada talvez por si mesmos em uma linha rotulada.


### <a name="local-variables-and-parameters"></a>Parâmetros e variáveis locais

Anterior seções detalhe como e quando são criadas instâncias de método e com elas, as cópias das variáveis locais e parâmetros de um método. Além disso, sempre que o corpo de um loop for inserido, uma nova cópia é feita de cada variável local declarada dentro desse loop, conforme descrito na seção [instruções de Loop](statements.md#loop-statements), e a instância de método agora contém esta cópia de sua variável local em vez disso que a cópia anterior.

Todos os locais são inicializados para seu valor padrão de tipo. Parâmetros e variáveis locais são sempre acessíveis publicamente. É um erro se referir a uma variável local em uma posição textual que precede a sua declaração, como mostra o exemplo a seguir:

```vb
Class A
    Private i As Integer = 0

    Sub F()
        i = 1
        Dim i As Integer       ' Error, use precedes declaration.
        i = 2
    End Sub

    Sub G()
        Dim a As Integer = 1
        Dim b As Integer = a   ' This is valid.
    End Sub
End Class
```

No `F` método acima, a primeira atribuição para `i` especificamente não se refere ao campo declarado no escopo externo. Em vez disso, ele se refere à variável local e ele é um erro porque ele textualmente precede a declaração da variável. No `G` método, uma declaração de variável subsequente se refere a uma variável local declarada em uma declaração de variável anterior na mesma declaração de variável local.

Cada bloco em um método cria um espaço de declaração para variáveis locais. Os nomes são introduzidos nesse espaço de declaração, por meio de declarações de variável local no corpo do método e lista de parâmetros do método, que apresenta os nomes no espaço de declaração do bloco externo. Blocos não permitem o sombreamento de nomes por meio de aninhamento: depois que um nome declarado em um bloco, o nome pode não ser declarado novamente todos os blocos aninhados.

Assim, no exemplo a seguir, o `F` e `G` métodos estão em erro porque o nome `i` é declarado em um bloco externo e não pode ser declarado novamente no bloco interno. No entanto, o `H` e `I` métodos são válidos porque os dois `i`do são declarados em blocos separados não aninhadas.

```vb
Class A
    Sub F()
        Dim i As Integer = 0
        If True Then
               Dim i As Integer = 1
        End If
    End Sub

    Sub G()
        If True Then
            Dim i As Integer = 0
        End If
        Dim i As Integer = 1
    End Sub 

    Sub H()
        If True Then
            Dim i As Integer = 0
        End If
        If True Then
            Dim i As Integer = 1
        End If
    End Sub

    Sub I() 
        For i As Integer = 0 To 9
            H()
        Next i

        For i As Integer = 0 To 9
            H()
        Next i
    End Sub 
End Class
```

Quando o método é uma função, uma variável local especial é declarada implicitamente em espaço de declaração do corpo do método com o mesmo nome que o método que representa o valor de retorno da função. A variável local tem semântica de resolução de nome especial quando usadas em expressões. Se a variável local é usada em um contexto que espera uma expressão classificada como um grupo de método, como uma expressão de invocação, em seguida, o nome é resolvido para a função em vez da variável local. Por exemplo:

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

O uso de parênteses pode causar situações ambíguas (como `F(1)`, onde `F` é uma função cujo tipo de retorno é uma matriz unidimensional); em todas as situações ambíguas, o nome é resolvido para a função em vez da variável local. Por exemplo:

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

Quando o fluxo de controle deixa o corpo do método, o valor da variável local é passado de volta para a expressão de invocação. Se o método for uma sub-rotina, não há nenhum tal variável local implícita e, simplesmente, o controle retorna para a expressão de invocação.

## <a name="local-declaration-statements"></a>Instruções de declaração de local

Uma instrução de declaração de local declara uma nova variável local, uma constante local ou uma variável estática. *Variáveis locais* e *constantes locais* são equivalentes a variáveis de instância e constantes no escopo para o método e são declarados da mesma maneira. *Variáveis estáticas* são semelhantes às `Shared` variáveis e são declarados usando a `Static` modificador.

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

Variáveis estáticas são locais que reter seu valor entre invocações do método. Variáveis estáticas declaradas dentro de métodos não compartilhados são por instância: cada instância do tipo que contém o método tem sua própria cópia da variável estática. Variáveis estáticas declaradas dentro de `Shared` métodos são por tipo, há apenas uma cópia da variável estática para todas as instâncias. Enquanto as variáveis locais são inicializadas para seu valor padrão de tipo após cada entrada no método, variáveis estáticas são inicializadas somente para seu valor padrão de tipo quando o tipo ou instância de tipo é inicializada. Variáveis estáticas não podem ser declaradas em estruturas ou métodos genéricos.

Variáveis locais, as constantes locais e as variáveis estáticas sempre tem acessibilidade pública e não é podem especificar modificadores de acessibilidade. Se nenhum tipo é especificado em uma instrução de declaração de local, as etapas a seguir determinam o tipo da declaração de local:

1. Se a declaração tem um caractere de tipo, o tipo de caractere de tipo é o tipo da declaração de local.

2. Se a declaração local é uma constante local, ou se a declaração local é uma variável local com um inicializador e Inferência de tipo de variável local está sendo usada, o tipo da declaração de local é inferido do tipo do inicializador. Se o inicializador refere-se a declaração de local, ocorrerá um erro de tempo de compilação. (As constantes locais devem ter inicializadores.)

3. Se a semântica estrita não está sendo usada, o tipo de instrução de declaração de local é implicitamente `Object`.

4. Caso contrário, ocorrerá um erro de tempo de compilação.

Se nenhum tipo é especificado em uma instrução de declaração de local que tem um tamanho da matriz ou um modificador de tipo de matriz, em seguida, o tipo da declaração de local é uma matriz com a classificação especificada e as etapas anteriores são usadas para determinar o tipo de elemento da matriz. Se a inferência de tipo de variável local é usada, o tipo do inicializador deve ser um tipo de matriz com a mesma forma de matriz (ou seja, modificadores de tipo de matriz) que a instrução de declaração de local. Observe que é possível que o tipo do elemento deduzido ainda pode ser um tipo de matriz. Por exemplo:

```vb
Option Infer On

Module Test
    Sub Main()
        ' Error: initializer is not an array type
        Dim x() = 1

        ' Type is Integer()
        Dim y() = New Integer() {}

        ' Type is Integer()()
        Dim z() = New Integer()() {}

        ' Type is Integer()()()

        Dim a()() = New Integer()()() {}

        ' Error: initializer does not have same array shape
        Dim b()() = New Integer(,)() {}
    End Sub
End Module
```

Se nenhum tipo é especificado em uma instrução de declaração de local que tem um modificador de tipo anulável, o tipo da declaração de local é a versão que permite valor nula do tipo inferido ou o tipo inferido em si se ele digitar um valor anulável já.  Se o tipo inferido não for um tipo de valor pode ser anulável, ocorrerá um erro de tempo de compilação. Se um modificador de tipo anulável e um modificador de tipo de tamanho ou uma matriz de matriz são colocados em uma instrução de declaração local sem tipo, em seguida, o modificador de tipo que permite valor nulo é considerado a ser aplicado ao tipo de elemento da matriz e as etapas anteriores são usadas para determinar o eleme tipo do NT.

Inicializadores de variável em instruções de declaração de local são equivalentes às instruções de atribuição colocadas no local textual da declaração. Portanto, se as ramificações de execução para a instrução de declaração de local, o inicializador de variável não é executado. Se a instrução de declaração de local é executada mais de uma vez, o inicializador de variável é executado um número igual de vezes. Variáveis estáticas apenas executar seu inicializador na primeira vez. Se ocorrer uma exceção ao inicializar uma variável estática, a variável estática é considerada inicializado com o valor padrão de tipo da variável estática.

O exemplo a seguir mostra o uso de inicializadores:

```vb
Module Test
    Sub F()
        Static x As Integer = 5

        Console.WriteLine("Static variable x = " & x)
        x += 1
    End Sub

    Sub Main()
        Dim i As Integer

        For i = 1 to 3
            F()
        Next i

        i = 3
label:
        Dim y As Integer = 8

        If i > 0 Then
            Console.WriteLine("Local variable y = " & y)
            y -= 1
            i -= 1
            GoTo label
        End If
    End Sub
End Module
```

Esse programa imprime:

```
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

Inicializadores em variáveis locais estáticas são thread-safe e protegidos contra exceções durante a inicialização. Se uma exceção ocorrer durante um inicializador de local estático, o local estático terá seu valor padrão e não ser inicializado. Um inicializador estático de local

```vb
Module Test
    Sub F()
        Static x As Integer = 5
    End Sub
End Module
```

equivale a

```vb
Imports System.Threading
Imports Microsoft.VisualBasic.CompilerServices

Module Test
    Class InitFlag
        Public State As Short
    End Class

    Private xInitFlag As InitFlag = New InitFlag()

    Sub F()
        Dim x As Integer

        If xInitFlag.State <> 1 Then
            Monitor.Enter(xInitFlag)
            Try
                If xInitFlag.State = 0 Then
                    xInitFlag.State = 2
                    x = 5
                Else If xInitFlag.State = 2 Then
                    Throw New IncompleteInitialization()
                End If
            Finally
                xInitFlag.State = 1
                Monitor.Exit(xInitFlag)
            End Try
        End If
    End Sub
End Module
```

Variáveis locais, as constantes locais e as variáveis estáticas têm o escopo para a instrução de bloco em que elas são declaradas. Variáveis estáticas são especiais em que seus nomes podem ser usados somente uma vez ao longo de todo o método. Por exemplo, não é válido para especificar duas declarações de variável estáticas com o mesmo nome, mesmo se eles estiverem em blocos diferentes.


### <a name="implicit-local-declarations"></a>Declarações locais implícitas

Além das instruções de declaração de local, as variáveis locais também podem ser declaradas implicitamente por meio do uso. Uma expressão de nome simples que usa um nome que não é resolvido para algo mais declara uma variável local com esse nome. Por exemplo:

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 10
        y = 20
        Console.WriteLine(x + y)
    End Sub
End Module
```

Declaração local implícita ocorre apenas em contextos de expressão que podem aceitar uma expressão classificada como uma variável. A exceção a essa regra é que uma variável local não pode ser implicitamente declarada quando ele é o destino de uma expressão de invocação de função, expressão de indexação ou uma expressão de acesso de membro.

Locais implícitas são tratados como se elas são declaradas no início do método que a contém. Assim, eles sempre têm escopo para o corpo do método inteira, mesmo se declarados dentro de uma expressão lambda. Por exemplo, o código a seguir:

```vb
Option Explicit Off 

Module Test
    Sub Main()
        Dim x = Sub()
                    a = 10
                End Sub
        Dim y = Sub()
                    Console.WriteLine(a)
                End Sub

        x()
        y()
    End Sub
End Module
```

imprimirá o valor `10`. Locais implícitas são digitadas como `Object` se nenhum caractere de tipo foi anexado ao nome da variável; caso contrário, o tipo da variável é o tipo de caractere de tipo. Inferência de tipo de variável local não é usada para locais implícitas.

Se a declaração explícita de local é especificada pelo ambiente de compilação ou por `Option Explicit`, todas as variáveis locais devem ser declaradas explicitamente e declaração de variável implícita não é permitida.

## <a name="with-statement"></a>Com instrução

Um `With` instrução permite várias referências aos membros de uma expressão sem especificar a expressão várias vezes.

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

A expressão deve ser classificada como um valor e é avaliada uma vez, após a entrada em um bloco. Dentro de `With` bloco de instrução, uma expressão de acesso de membro ou expressão de acesso de dicionário, começando com um ponto final ou um ponto de exclamação é avaliada como se o `With` expressão antecedem. Por exemplo:

```vb
Structure Test
    Public x As Integer

    Function F() As Integer
        Return 10
    End Sub
End Structure

Module TestModule
    Sub Main()
        Dim y As Test

        With y
            .x = 10
            Console.WriteLine(.x)
            .x = .F()
        End With
    End Sub
End Module
```

Não é válido se ramificar para um `With` bloco de instruções de fora do bloco.


## <a name="synclock-statement"></a>Instrução SyncLock

Um `SyncLock` instrução permite que as instruções a serem sincronizados em uma expressão, o que garante que vários threads de execução não executem as mesmas instruções ao mesmo tempo.

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

A expressão deve ser classificada como um valor e é avaliada uma vez, após a entrada para o bloco. Ao inserir o `SyncLock` bloco, o `Shared` método `System.Threading.Monitor.Enter` é chamado na expressão especificada, que bloqueia até que o thread de execução tem um bloqueio exclusivo no objeto retornado pela expressão. O tipo da expressão em um `SyncLock` instrução deve ser um tipo de referência. Por exemplo:

```vb
Class Test
    Private count As Integer = 0

    Public Function Add() As Integer
        SyncLock Me
            count += 1
            Add = count
        End SyncLock
    End Function

    Public Function Subtract() As Integer
        SyncLock Me
            count -= 1
            Subtract = count
        End SyncLock
    End Function
End Class
```

O exemplo acima sincroniza na instância específica da classe `Test` para garantir que não mais do que um thread de execução pode adicionar ou subtrair a partir da variável de contagem de uma vez para uma determinada instância.

O `SyncLock` bloco está contido implicitamente por um `Try` instrução cuja `Finally` bloquear chamadas a `Shared` método `System.Threading.Monitor.Exit` na expressão. Isso garante que o bloqueio seja liberado, mesmo quando uma exceção é lançada. Como resultado, não é válido ramificar em uma `SyncLock` bloquear de fora do bloco e uma `SyncLock` bloco é tratado como uma única instrução para os fins `Resume` e `Resume Next`. O exemplo acima é equivalente ao código a seguir:

```vb
Class Test
    Private count As Integer = 0

    Public Function Add() As Integer
        Try
            System.Threading.Monitor.Enter(Me)

            count += 1
            Add = count
        Finally
            System.Threading.Monitor.Exit(Me)
        End Try
    End Function

    Public Function Subtract() As Integer
        Try
            System.Threading.Monitor.Enter(Me)

            count -= 1
            Subtract = count
        Finally
            System.Threading.Monitor.Exit(Me)
        End Try
    End Function
End Class
```


## <a name="event-statements"></a>Instruções Event

O `RaiseEvent`, `AddHandler`, e `RemoveHandler` instruções geram eventos e manipular eventos dinamicamente.

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a>Instrução RaiseEvent

Um `RaiseEvent` instrução notifica os manipuladores de eventos que um determinado evento ocorreu.

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

A expressão de nome simples em uma `RaiseEvent` instrução é interpretada como uma pesquisa de membro em `Me`. Portanto, `RaiseEvent x` é interpretado como se fosse `RaiseEvent Me.x`. O resultado da expressão deve ser classificado como um evento de acesso para um evento definido na classe em si; eventos definidos em tipos de base não podem ser usados em um `RaiseEvent` instrução.

O `RaiseEvent` instrução é processada como uma chamada para o `Invoke` método do delegado do evento, usando os parâmetros fornecidos, se houver. Se o valor do delegado é `Nothing`, nenhuma exceção é lançada. Se não houver nenhum argumento, os parênteses podem ser omitidos. Por exemplo:

```vb
Class Raiser
    Public Event E1(Count As Integer)

    Public Sub Raise()
        Static RaiseCount As Integer = 0

        RaiseCount += 1
        RaiseEvent E1(RaiseCount)
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler(Count As Integer) Handles x.E1
        Console.WriteLine("Raise #" & Count)
    End Sub

    Public Sub Main()
        x = New Raiser
        x.Raise()        ' Prints "Raise #1".
        x.Raise()        ' Prints "Raise #2".
        x.Raise()        ' Prints "Raise #3".
    End Sub
End Module
```

A classe `Raiser` acima é equivalente a:

```vb
Class Raiser
    Public Event E1(Count As Integer)

    Public Sub Raise()
        Static RaiseCount As Integer = 0
        Dim TemporaryDelegate As E1EventHandler

        RaiseCount += 1

        ' Use a temporary to avoid a race condition.
        TemporaryDelegate = E1Event
        If Not TemporaryDelegate Is Nothing Then
            TemporaryDelegate.Invoke(RaiseCount)
        End If
    End Sub
End Class
```


### <a name="addhandler-and-removehandler-statements"></a>AddHandler e RemoveHandler instruções

Embora a maioria dos manipuladores de eventos são conectados automaticamente por meio de `WithEvents` variáveis, talvez seja necessário adicionar e remover manipuladores de eventos em tempo de execução dinamicamente. `AddHandler` e `RemoveHandler` instruções fazer isso.

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

Cada instrução leva dois argumentos: o primeiro argumento deve ser uma expressão que é classificada como um acesso de evento e o segundo argumento deve ser uma expressão que é classificada como um valor. Tipo do segundo argumento deve ser do tipo de delegado associado com o acesso ao evento. Por exemplo:

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

Dada a um evento `E,` a instrução chama o relevantes `add_E` ou `remove_E` método na instância para adicionar ou remover o delegado como um manipulador para o evento. Assim, o código acima é equivalente a:

```vb
Public Class Form1
    Public Sub New()
        Button1.add_Click(AddressOf Button1_Click)
    End Sub 

    Private Button1 As Button = New Button()

    Sub Button1_Click(sender As Object, e As EventArgs)
        Console.WriteLine("Button1 was clicked!")
    End Sub

    Public Sub Disconnect()
        Button1.remove_Click(AddressOf Button1_Click)
    End Sub
End Class
```


## <a name="assignment-statements"></a>Instruções de atribuição

Uma instrução de atribuição atribui o valor de uma expressão a uma variável. Há vários tipos de atribuição.

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a>Instruções de atribuição regular

Uma instrução de atribuição simples armazena o resultado de uma expressão em uma variável.

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

A expressão no lado esquerdo do operador de atribuição deve ser classificada como uma variável ou um acesso de propriedade, enquanto a expressão no lado direito do operador de atribuição deve ser classificada como um valor. O tipo da expressão deve ser implicitamente conversível para o tipo de acesso a variável ou propriedade.

Se a variável que está sendo atribuída em é um elemento de matriz do tipo de referência, uma verificação de tempo de execução será executada para garantir que a expressão é compatível com o tipo de elemento da matriz. No exemplo a seguir, a última atribuição faz com que um `System.ArrayTypeMismatchException` ser lançada, porque uma instância do `ArrayList` não podem ser armazenados em um elemento de um `String` matriz.

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

Se a expressão no lado esquerdo do operador de atribuição é classificada como uma variável, a instrução de atribuição armazena o valor na variável. Se a expressão é classificada como um acesso de propriedade e, em seguida, a instrução de atribuição transforma o acesso de propriedade em uma invocação do `Set` acessador da propriedade com o valor substituído para o parâmetro de valor. Por exemplo:

```vb
Module Test
    Private PValue As Integer

    Public Property P As Integer
        Get
            Return PValue
        End Get

        Set (Value As Integer)
            PValue = Value
        End Set
    End Property

    Sub Main()
        ' The following two lines are equivalent.
        P = 10
        set_P(10)
    End Sub
End Module
```

Se o destino do acesso de variável ou propriedade é digitado como um tipo de valor, mas não classificado como uma variável, ocorrerá um erro de tempo de compilação. Por exemplo:

```vb
Structure S
    Public F As Integer
End Structure

Class C
    Private PValue As S

    Public Property P As S
        Get
            Return PValue
        End Get

        Set (Value As S)
            PValue = Value
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim ct As C = New C()
        Dim rt As Object = new C()

        ' Compile-time error: ct.P not classified as variable.
        ct.P.F = 10

        ' Run-time exception.
        rt.P.F = 10
    End Sub
End Module
```

Observe que a semântica da atribuição depende do tipo da variável ou propriedade à qual ele está sendo atribuído. Se a variável à qual ele está sendo atribuído é um tipo de valor, a atribuição copia o valor da expressão para a variável. Se a variável à qual ele está sendo atribuído é um tipo de referência, a atribuição copia a referência, não o próprio valor, a variável. Se o tipo da variável for `Object`, a semântica de atribuição é determinada pelo tipo do valor seja um tipo de valor ou um tipo de referência em tempo de execução.


__Observação.__ Para tipos intrínsecos, como `Integer` e `Date`, semântica de atribuição de referência e valor é os mesmos, porque os tipos são imutáveis. Como resultado, o idioma é livre para usar a atribuição de referência no box tipos intrínsecos como uma otimização. De uma perspectiva de valor, o resultado é o mesmo.

Porque o caractere de igual (`=`) é usado para atribuição e de igualdade, haja uma ambiguidade entre uma atribuição simples e uma instrução de invocação em situações como `x = y.ToString()`. Nesses casos, a instrução de atribuição prevalece sobre o operador de igualdade. Isso significa que o exemplo de expressão é interpretado como `x = (y.ToString())` em vez de `(x = y).ToString()`.


### <a name="compound-assignment-statements"></a>Instruções de atribuição compostas

Um *instrução de atribuição composta* assume a forma `V op= E` (onde `op` é um operador binário válido).

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

A expressão no lado esquerdo do operador de atribuição deve ser classificada como um acesso de variável ou propriedade, enquanto a expressão no lado direito do operador de atribuição deve ser classificada como um valor. A instrução de atribuição composta é equivalente à instrução `V = V op E` com a diferença de que a variável no lado esquerdo do operador de atribuição composta só é avaliada uma vez. O exemplo a seguir demonstra essa diferença:

```vb
Module Test
    Function GetIndex() As Integer
        Console.WriteLine("Getting index")
        Return 1
    End Function

    Sub Main()
        Dim a(2) As Integer

        Console.WriteLine("Simple assignment")
        a(GetIndex()) = a(GetIndex()) + 1

        Console.WriteLine("Compound assignment")
        a(GetIndex()) += 1
    End Sub
End Module
```

A expressão `a(GetIndex())` é avaliada duas vezes para atribuição simples, mas somente depois de atribuição composta, portanto, o código imprime:

```
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a>Instrução MID de atribuição

Um `Mid` instrução de atribuição atribui uma cadeia de caracteres em outra cadeia de caracteres. O lado esquerdo da atribuição tem a mesma sintaxe que uma chamada para a função `Microsoft.VisualBasic.Strings.Mid`.

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

O primeiro argumento é o destino da atribuição e deve ser classificado como uma variável ou um acesso de propriedade cujo tipo é implicitamente conversível para e de `String`. O segundo parâmetro é a posição de início de base 1 que corresponde em que a atribuição deve começar na cadeia de caracteres de destino e deve ser classificada como um valor cujo tipo deve ser implicitamente conversível para `Integer`. O terceiro parâmetro opcional é o número de caracteres à direita o valor a atribuir para a cadeia de caracteres de destino e deve ser classificada como um valor cujo tipo é implicitamente conversível para `Integer`. O lado direito é a cadeia de caracteres de origem e deve ser classificado como um valor cujo tipo é implicitamente conversível para `String`. À direita será truncada para o parâmetro de comprimento, se especificado e substitui os caracteres na cadeia de caracteres esquerda, começando na posição inicial. Se a cadeia de caracteres do lado direito contiver menos caracteres que o terceiro parâmetro, somente os caracteres da cadeia de caracteres do lado direito serão copiados.

O exemplo a seguir exibe `ab123fg`:

```vb
Module Test
    Sub Main()
        Dim s1 As String = "abcdefg"
        Dim s2 As String = "1234567"

        Mid$(s1, 3, 3) = s2
        Console.WriteLine(s1)
    End Sub
End Module
```

__Observação.__ `Mid` não é uma palavra reservada.


## <a name="invocation-statements"></a>Instruções de invocação

Uma instrução de invocação invoca um método precedido pela palavra-chave a opcional `Call`. A instrução de invocação é processada da mesma forma que a expressão de invocação de função, com algumas diferenças, indicadas abaixo. A expressão de invocação deve ser classificada como um valor ou nulo. Qualquer valor resultante da avaliação da expressão de invocação é descartado.

Se o `Call` palavra-chave for omitida e, em seguida, a expressão de invocação deve começar com um identificador ou palavra-chave ou com `.` dentro de um `With` bloco. Assim, por exemplo, "`Call 1.ToString()`" é uma instrução válida, mas "`1.ToString()`" não é. (Observe que em um contexto de expressão, expressões de invocação também precisam não começar com um identificador. Por exemplo, "`Dim x = 1.ToString()`" é uma instrução válida).

Há outra diferença entre as instruções de invocação e expressões de invocação: se uma instrução de invocação inclui uma lista de argumentos, isso sempre será interpretado como a lista de argumentos da invocação. O exemplo a seguir ilustra a diferença:

```vb
Module Test
    Sub Main()
        Call {Function() 15}(0)
        ' error: (0) is taken as argument list, but array is not invokable

        Call ({Function() 15}(0))
        ' valid, since the invocation statement has no argument list

        Dim x = {Function() 15}(0)
        ' valid as an expression, since (0) is taken as an array-indexing

        Call f("a")
        ' error: ("a") is taken as argument list to the invocation of f

        Call f()("a")
        ' valid, since () is the argument list for the invocation of f

        Dim y = f("a")
        ' valid as an expression, since f("a") is interpreted as f()("a")
    End Sub

    Sub f() As Func(Of String,String)
        Return Function(x) x
    End Sub
End Module
```

```antlr
InvocationStatement
    : 'Call'? InvocationExpression StatementTerminator
    ;
```

## <a name="conditional-statements"></a>Instruções condicionais

Instruções condicionais permitem execução condicional de instruções com base em expressões avaliadas em tempo de execução.

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a>If... Then... Instruções Else

Um `If...Then...Else` instrução é a instrução condicional básica.

```antlr
IfStatement
    : BlockIfStatement
    | LineIfThenStatement
    ;

BlockIfStatement
    : 'If' BooleanExpression 'Then'? StatementTerminator
      Block?
      ElseIfStatement*
      ElseStatement?
      'End' 'If' StatementTerminator
    ;

ElseIfStatement
    : ElseIf BooleanExpression 'Then'? StatementTerminator
      Block?
    ;

ElseStatement
    : 'Else' StatementTerminator
      Block?
    ;

LineIfThenStatement
    : 'If' BooleanExpression 'Then' Statements ( 'Else' Statements )? StatementTerminator
    ;
    
ElseIf      
    : 'ElseIf'      
    | 'Else' 'If'   
   ;
```

Cada expressão em uma `If...Then...Else` instrução deve ser uma expressão booleana, de acordo com a seção [expressões Boolianas](expressions.md#boolean-expressions). (Observação: isso não exige que a expressão para ter um tipo booliano). Se a expressão na `If` instrução for true, as instruções estão entre o `If` bloco são executados. Se a expressão for false, cada um do `ElseIf` expressões será avaliada. Se um do `ElseIf` expressões for avaliada como true, o bloco correspondente é executado. Se nenhuma expressão for avaliada como true e houver um `Else` bloco, o `Else` bloco é executado. Depois que um bloco da execução, a execução passa para o final do `If...Then...Else` instrução.

A versão de linha do `If` instrução tem um único conjunto de instruções a serem executadas se a `If` expressão é `True` e um conjunto opcional de instruções a serem executadas se a expressão for `False`. Por exemplo:

```vb
Module Test
    Sub Main()
        Dim a As Integer = 10
        Dim b As Integer = 20

        ' Block If statement.
        If a < b Then
            a = b
        Else
            b = a
        End If

        ' Line If statement
        If a < b Then a = b Else b = a
    End Sub
End Module
```

A versão de linha da se instrução associa menor firmemente que ":" e seu `Else` associa a precedente mais próximo lexicalmente `If` que é permitido pela sintaxe. Por exemplo, as duas versões a seguir são equivalentes:

```vb
If True Then _
If True Then Console.WriteLine("a") Else Console.WriteLine("b") _
Else Console.WriteLine("c") : Console.WriteLine("d")

If True Then
    If True Then
        Console.WriteLine("a")
    Else
        Console.WriteLine("b")
    End If
    Console.WriteLine("c") : Console.WriteLine("d")
End If
```

Todas as instruções que não sejam instruções de declaração de rótulo são permitidas dentro de uma linha `If` instrução, incluindo instruções de blocos. No entanto, eles não podem usar LineTerminators como StatementTerminators exceto dentro de expressões lambda de várias linhas. Por exemplo:

```vb
' Allowed, since it uses : instead of LineTerminator to separate statements
If b Then With New String("a"(0),5) : Console.WriteLine(.Length) : End With

' Disallowed, since it uses a LineTerminator
If b then With New String("a"(0), 5)
              Console.WriteLine(.Length)
          End With

' Allowed, since it only uses LineTerminator inside a multi-line lambda
If b Then Call Sub()
                   Console.WriteLine("a")
               End Sub.Invoke()
```

### <a name="select-case-statements"></a>Instruções Select Case

Um `Select Case` instrução executa as instruções com base no valor de uma expressão.

```antlr
SelectStatement
    : 'Select' 'Case'? Expression StatementTerminator
      CaseStatement*
      CaseElseStatement?
      'End' 'Select' StatementTerminator
    ;

CaseStatement
    : 'Case' CaseClauses StatementTerminator
      Block?
    ;

CaseClauses
    : CaseClause ( Comma CaseClause )*
    ;

CaseClause
    : ( 'Is' LineTerminator? )? ComparisonOperator LineTerminator? Expression
    | Expression ( 'To' Expression )?
    ;

ComparisonOperator
    : '=' | '<' '>' | '<' | '>' | '>' '=' | '<' '='
    ;

CaseElseStatement
    : 'Case' 'Else' StatementTerminator
      Block?
    ;
```

A expressão deve ser classificada como um valor. Quando um `Select Case` instrução é executada, o `Select` expressão é avaliada em primeiro lugar e o `Case` instruções, em seguida, são avaliadas na ordem da declaração textual. A primeira `Case` instrução que é avaliada como `True` tem seu bloco executado. Se nenhum `Case` instrução é avaliada como `True` e não há um `Case Else` instrução, esse bloco é executado. Depois que um bloco tiver concluído a execução, a execução passa para o final do `Select` instrução.

Execução de um `Case` bloco não tem permissão para "passar" para a próxima seção switch. Isso impede que uma classe comum de erros que ocorrem em outros idiomas quando um `Case` encerrar instrução acidentalmente é omitido. O exemplo a seguir ilustra esse comportamento:

```vb
Module Test
    Sub Main()
        Dim x As Integer = 10

        Select Case x
            Case 5
                Console.WriteLine("x = 5")
            Case 10
                Console.WriteLine("x = 10")
            Case 20 - 10
                Console.WriteLine("x = 20 - 10")
            Case 30
                Console.WriteLine("x = 30")
        End Select
    End Sub
End Module
```

O código imprime:

```
x = 10
```

Embora `Case 10` e `Case 20 - 10` para o mesmo valor, selecione `Case 10` é executado porque ele precede `Case 20 - 10` textualmente. Quando a próxima `Case` é atingido, a execução continua após a `Select` instrução.

Um `Case` cláusula pode assumir duas formas. Uma forma é um recurso opcional `Is` palavra-chave, um operador de comparação e uma expressão. A expressão será convertida para o tipo dos `Select` expressão; se a expressão não é implicitamente conversível no tipo do `Select` expressão, um erro de tempo de compilação ocorre. Se o `Select` expressão é *eletrônico*, é o operador de comparação *Op*e o `Case` expressão é *E1*, o caso é avaliado como *E OP E1*. O operador deve ser válido para os tipos das duas expressões; Caso contrário, ocorrerá um erro de tempo de compilação.

A outra forma é uma expressão opcionalmente seguida da palavra-chave `To` e uma segunda expressão. As duas expressões são convertidas para o tipo dos `Select` expressão; se qualquer expressão não é implicitamente conversível no tipo do `Select` expressão, um erro de tempo de compilação ocorre. Se o `Select` expressão é `E`, a primeira `Case` expressão é `E1`e a segunda `Case` expressão é `E2`, o `Case` é avaliada como `E = E1` (se nenhum `E2`for especificado) ou `(E >= E1) And (E <= E2)`. Os operadores devem ser válidos para os tipos das duas expressões; Caso contrário, ocorrerá um erro de tempo de compilação.


## <a name="loop-statements"></a>Instruções de loop

Instruções de loop permitem que a execução repetida de instruções no corpo.

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

Cada vez que o corpo do loop for inserido, uma nova cópia é feita de todas as variáveis locais declaradas no corpo dessa, inicializado com os valores anteriores das variáveis. Qualquer referência a uma variável dentro do corpo do loop usará a cópia feita mais recentemente. Este código mostra um exemplo:

```vb
Module Test
    Sub Main()
        Dim lambdas As New List(Of Action)
        Dim x = 1

        For i = 1 To 3
            x = i
            Dim y = x
            lambdas.Add(Sub() Console.WriteLine(x & y))
        Next

        For Each lambda In lambdas
            lambda()
        Next
    End Sub
End Module
```

O código produz a saída:

```
31    32    33
```

Quando o corpo do loop é executado, ele usa qualquer cópia da variável é atual. Por exemplo, a instrução `Dim y = x` refere-se à cópia mais recente do `y` e a cópia original do `x`. E quando uma lambda é criada, ela lembra a qualquer cópia de uma variável era atual no momento em que ele foi criado. Portanto, cada lambda usa a mesma cópia compartilhada da `x`, mas uma outra cópia do `y`. No final do programa, quando ele executa os lambdas que cópia compartilhada do `x` que todos eles denotam agora está em seu valor final 3.

Observe que se não houver nenhum lambdas ou expressões de LINQ, em seguida, é impossível saber que uma nova cópia é feita na entrada do loop. Na verdade, as otimizações do compilador evitará fazendo cópias nesse caso. Observe também que é ilegal `GoTo` em um loop que contém expressões de LINQ ou lambdas.


### <a name="whileend-while-and-doloop-statements"></a>While... End ao mesmo tempo e não... Instruções de loop

Um `While` ou `Do` loop loop for baseado em uma expressão booliana.

```antlr
WhileStatement
    : 'While' BooleanExpression StatementTerminator
      Block?
      'End' 'While' StatementTerminator
    ;

DoLoopStatement
    : DoTopLoopStatement
    | DoBottomLoopStatement
    ;

DoTopLoopStatement
    : 'Do' ( WhileOrUntil BooleanExpression )? StatementTerminator
      Block?
      'Loop' StatementTerminator
    ;

DoBottomLoopStatement
    : 'Do' StatementTerminator
      Block?
      'Loop' WhileOrUntil BooleanExpression StatementTerminator
    ;

WhileOrUntil
    : 'While' | 'Until'
    ;
```

Um `While` instrução de loop executa um loop, desde a expressão booleana for avaliada como true; um `Do` instrução loop pode conter uma condição mais complexa. Uma expressão pode ser colocada após o `Do` palavra-chave ou após o `Loop` palavra-chave, mas não após os dois. A expressão booliana é avaliada de acordo com a seção [expressões Boolianas](expressions.md#boolean-expressions). (Observação: isso não exige que a expressão para ter um tipo booliano). Também é válido para não especificar nenhuma expressão Nesse caso, o loop nunca será encerrado. Se a expressão é posicionada após `Do`, ele será avaliado antes do bloco de loop é executado em cada iteração. Se a expressão é posicionada após `Loop`, ele será avaliado depois que o bloco de loop for executado em cada iteração. Colocar a expressão após `Loop` , portanto, irá gerar um loop mais que o posicionamento após `Do`. O exemplo a seguir demonstra esse comportamento:

```vb
Module Test
    Sub Main()
        Dim x As Integer

        x = 3
        Do While x = 1
            Console.WriteLine("First loop")
        Loop

        Do
            Console.WriteLine("Second loop")
        Loop While x = 1
    End Sub
End Module
```

O código produz a saída:

```
Second Loop
```

No caso do primeiro loop, a condição é avaliada antes que o loop é executado. No caso do segundo loop, a condição é executada depois que o loop é executado. A expressão condicional deve ser prefixada com um `While` palavra-chave ou um `Until` palavra-chave. O primeiro interrompe o loop se a condição for avaliada como false, o último quando a condição for avaliada como true.

__Observação.__ `Until` não é uma palavra reservada.


### <a name="fornext-statements"></a>Para... Próximas instruções

Um `For...Next` loop for baseado em um conjunto de limites. Um `For` declaração especifica uma variável de controle de loop, uma expressão de limite inferior, uma expressão de limite superior e uma expressão de valor da etapa opcional. A variável de controle de loop é especificada por meio de um identificador seguido por um recurso opcional `As` cláusula ou uma expressão.

```antlr
ForStatement
    : 'For' LoopControlVariable Equals Expression 'To' Expression
      ( 'Step' Expression )? StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;

LoopControlVariable
    : Identifier ( IdentifierModifiers 'As' TypeName )?
    | Expression
    ;

NextExpressionList
    : Expression ( Comma Expression )*
    ;
```

De acordo com as regras a seguir, a variável de controle de loop refere-se para uma nova variável local específica para isso `For...Next` instrução, ou a uma variável já existente, ou como uma expressão.

* Se a variável de controle de loop é um identificador com um `As` cláusula, o identificador define uma nova variável local do tipo especificado em de `As` cláusula, no escopo para todo o `For` loop.

* Se a variável de controle de loop é um identificador sem um `As` cláusula e, em seguida, o identificador do primeiro é resolvido usando as regras de resolução de nome simples (consulte a seção [expressões simples de nome](expressions.md#simple-name-expressions)), exceto que dessa ocorrência do o identificador não por si só causaria uma variável local implícita a ser criado (seção [implícita declarações locais](statements.md#implicit-local-declarations)).

  * Se essa resolução for bem-sucedida e o resultado é classificado como uma variável, a variável de controle de loop é essa variável já existente.

  * Se a resolução falhar ou se a resolução for bem-sucedida e o resultado é classificado como um tipo, em seguida:
    * Se a inferência de tipo de variável local está sendo usada, o identificador define uma nova variável local cujo tipo é inferido do que o limite e expressões, como todo o escopo de etapa `For` loop;
    * Se inferência de tipo de variável local não está sendo usada, mas a declaração local implícita é, então uma variável local implícita é criada, cujo escopo é o método inteiro (seção [implícita declarações locais](statements.md#implicit-local-declarations)) e a variável de controle de loop refere-se a essa variável pré-existente;
    * Se não inferência de tipo de variável local nem implícitas declarações locais são usadas, é um erro.

  * Se a resolução for bem-sucedida com algo classificado como um tipo nem uma variável, ele é um erro.

* Se a variável de controle de loop for uma expressão, a expressão deve ser classificada como uma variável.

Uma variável de controle de loop não pode ser usada por outro delimitador `For...Next` instrução. O tipo da variável de controle de loop de um `For` instrução determina o tipo da iteração e deve ser um destes:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`
* Um tipo enumerado
* `Object`
* Um tipo `T` que tem os seguintes operadores, onde `B` é um tipo que pode ser usado em uma expressão booliana:

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

O limite e expressões de etapa devem ser implicitamente conversíveis para o tipo da variável de controle de loop e devem ser classificadas como valores. Em tempo de compilação, o tipo da variável de controle de loop é inferido, escolhendo o tipo o maior entre o limite inferior, o limite superior e os tipos de expressão de etapa. Se não houver nenhuma conversão de ampliação entre dois dos tipos, ocorre um erro de tempo de compilação.

No tempo de execução, se o tipo da variável de controle de loop for `Object`, em seguida, o tipo da iteração é inferido a mesma que em tempo de compilação, com duas exceções. Primeiro, se o limite e expressões de etapa estão todos os tipos integrais, mas não ter nenhum tipo o maior e o tipo o maior que abrange todos os três tipos será inferido. E, segundo, se o tipo da variável de controle de loop é inferido para ser `String`, `Double` será inferido. Se, em tempo de execução, nenhum tipo de controle de loop pode ser determinado ou se qualquer uma das expressões não pode ser convertido no tipo de controle de loop, um `System.InvalidCastException` ocorrerá. Depois que um tipo de controle de loop for escolhido no início do loop, o mesmo tipo será usado ao longo da iteração, independentemente das alterações feitas ao valor na variável de controle de loop.

Um `For` instrução deve ser fechada por uma correspondência `Next` instrução. Um `Next` instrução sem uma variável corresponde ao abrir o mais interno `For` instrução, enquanto um `Next` instrução com uma ou mais variáveis de controle de loop, da esquerda para direita, corresponderá a `For` loops que correspondem a cada variável. Se corresponder a uma variável de um `For` resulta do loop for que não é o loop aninhado mais nesse ponto, um erro de tempo de compilação.

No início do loop, três expressões são avaliadas na ordem textual e a expressão de limite inferior é atribuída à variável de controle de loop. Se o valor da etapa for omitido, é implicitamente o literal `1`, convertido para o tipo da variável de controle de loop. As três expressões são avaliadas apenas no início do loop.

No início de cada loop, a variável de controle é comparada para ver se ele for maior que o ponto de extremidade se a expressão de etapa for positivo, ou menor que o ponto de extremidade, se a expressão de etapa for negativa. Se for, o `For` loop é encerrado; caso contrário, o bloco de loop é executado. Se a variável de controle de loop não é um tipo primitivo, o operador de comparação é determinado pelo se a expressão `step >= step - step` é true ou false. No `Next` instrução, o valor da etapa é adicionado à variável de controle e execução retorna para a parte superior do loop.

Observe que é uma nova cópia da variável de controle de loop *não* criada em cada iteração do bloco de loop. Nesse sentido, o `For` instrução difere `For Each` (seção [para cada um... Próximas instruções](statements.md#for-eachnext-statements)).

Não é válido ramificar em uma `For` executar um loop de fora do loop.


### <a name="for-eachnext-statements"></a>Para cada um... Próximas instruções

Um `For Each...Next` loops de instrução com base nos elementos em uma expressão. Um `For Each` declaração especifica uma variável de controle de loop e uma expressão do enumerador. A variável de controle de loop é especificada por meio de um identificador seguido por um recurso opcional `As` cláusula ou uma expressão.

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

Seguindo as mesmas regras `For...Next` instruções (seção [para... Próximas instruções](statements.md#fornext-statements)), a variável de controle de loop refere-se para uma nova variável local específica para isso para cada um... Próxima instrução, ou a uma variável já existente, ou como uma expressão.

A expressão do enumerador deve ser classificada como um valor e seu tipo deve ser um tipo de coleção ou `Object`. Se o tipo da expressão do enumerador é `Object`, em seguida, todo o processamento é adiada até o tempo de execução. Caso contrário, uma conversão deve existir do tipo de elemento da coleção para o tipo da variável de controle de loop.

A variável de controle de loop não pode ser usada por outro delimitador `For Each` instrução. Um `For Each` instrução deve ser fechada por uma correspondência `Next` instrução. Um `Next` instrução sem uma variável de controle de loop corresponde ao abrir o mais interno `For Each`. Um `Next` instrução com uma ou mais variáveis de controle de loop, da esquerda para direita, corresponderá a `For Each` loops que têm a mesma variável de controle de loop. Se corresponder a uma variável de um `For Each` ocorre do loop for que não é o loop aninhado mais nesse ponto, um erro de tempo de compilação.

Um tipo `C` deve ser um *tipo de coleção* se um de:

* Todos os itens a seguir forem verdadeiras:
  * `C` contém um método de extensão com a assinatura ou uma instância acessível, compartilhados `GetEnumerator()` que retorna um tipo `E`.
  * `E` contém um método de extensão com a assinatura ou uma instância acessível, compartilhados `MoveNext()` e o tipo de retorno `Boolean`.
  * `E` contém uma propriedade compartilhada chamada ou instância acessível `Current` que tem um getter. O tipo dessa propriedade é o tipo de elemento do tipo de coleção.

* Ele implementa a interface `System.Collections.Generic.IEnumerable(Of T)`, caso em que o tipo de elemento da coleção é considerado `T`.

* Ele implementa a interface `System.Collections.IEnumerable`, caso em que o tipo de elemento da coleção é considerado `Object`.

A seguir está um exemplo de uma classe que pode ser enumerado:

```vb
Public Class IntegerCollection
    Private integers(10) As Integer

    Public Class IntegerCollectionEnumerator
        Private collection As IntegerCollection
        Private index As Integer = -1

        Friend Sub New(c As IntegerCollection)
            collection = c
        End Sub

        Public Function MoveNext() As Boolean
            index += 1

            Return index <= 10
        End Function

        Public ReadOnly Property Current As Integer
            Get
                If index < 0 OrElse index > 10 Then
                    Throw New System.InvalidOperationException()
                End If

                Return collection.integers(index)
            End Get
        End Property
    End Class

    Public Sub New()
        Dim i As Integer

        For i = 0 To 10
            integers(i) = I
        Next i
    End Sub

    Public Function GetEnumerator() As IntegerCollectionEnumerator
        Return New IntegerCollectionEnumerator(Me)
    End Function
End Class
```

Antes de inicia o loop, a expressão do enumerador é avaliada. Se o tipo da expressão não satisfaz o padrão de design, a expressão é convertida para `System.Collections.IEnumerable` ou `System.Collections.Generic.IEnumerable(Of T)`. Se o tipo de expressão implementa a interface genérica, a interface genérica é preferencial em tempo de compilação, mas a interface não genérica é preferencial em tempo de execução. Se o tipo de expressão implementa a interface genérica várias vezes, a instrução será considerada ambígua e ocorre um erro de tempo de compilação.

__Observação.__ A interface não genérica é preferida no caso de tardia acoplado, porque a interface genérica de separação significa que todas as chamadas para os métodos de interface envolveria a parâmetros de tipo. Como não é possível saber a correspondência de argumentos de tipo em tempo de execução, todas as chamadas precisaria ser feita usando chamadas de associação tardia. Isso pode ser mais lento do que chamar a interface não genérica, porque a interface não genérica pode ser chamada usando chamadas de tempo de compilação.

`GetEnumerator` é chamado no valor resultante e o retorne o valor da função é armazenado em um temporário. Em seguida, no início de cada iteração, `MoveNext` é chamado em temporárias. Se ele retornar `False`, o loop é encerrado. Caso contrário, cada iteração do loop é executada da seguinte maneira:

1. Se a variável de controle de loop identificou uma nova variável local (em vez de uma já existente), uma cópia atualizada dessa variável local é criada. Para a iteração atual, todas as referências dentro do bloco de loop fará referência a essa cópia.
2. O `Current` propriedade é recuperada, forçado para o tipo da variável de controle de loop (independentemente se a conversão é implícita ou explícita) e atribuído à variável de controle de loop.
3. O bloco de loop é executado.

__Observação.__ Há uma pequena alteração no comportamento entre a versão 10.0 e 11.0 do idioma. Antes de 11.0, uma variável de iteração atualizada foi *não* criado para cada iteração do loop. Essa diferença é observável somente se a variável de iteração é capturada por um lambda ou uma expressão LINQ que, em seguida, é invocada após o loop:

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

Até o Visual Basic 10.0, isso produziu um aviso em tempo de compilação e impresso "3" três vezes. Isso foi porque não havia somente uma variável compartilhada por todas as iterações do loop, "x" e todas as três lambdas capturados no mesmo "x" e, em seguida, no momento em que os lambdas foram executados ele mantido, em seguida, o número 3. A partir do Visual Basic 11.0, ele imprime "1, 2, 3". Isso ocorre porque cada lambda captura uma variável diferente "x".


__Observação.__ O elemento atual da iteração é convertido para o tipo da variável de controle de loop, mesmo se a conversão é explícita, pois há um local conveniente para introduzir um operador de conversão na instrução. Isso tornou-se especialmente problemático ao trabalhar com o tipo agora obsoleta `System.Collections.ArrayList`, porque seu tipo de elemento é `Object`. Isso teria as conversões necessárias em uma ótima muitos loops, algo que achamos não era ideal. Ironicamente, genéricos habilitado a criação de uma coleção fortemente tipada, `System.Collections.Generic.List(Of T)`, que pode ter feito nos repensar a esse ponto de design, mas para meu Deus da compatibilidade, isso não pode ser alterado agora.


Quando o `Next` instrução for atingida, a execução retorna para a parte superior do loop. Se uma variável é especificada após o `Next` palavra-chave, ele deve ser o mesmo que a primeira variável após a `For Each`. Por exemplo, considere o seguinte código:

```vb
Module Test
    Sub Main()
        Dim i As Integer
        Dim c As IntegerCollection = New IntegerCollection()

        For Each i In c
            Console.WriteLine(i)
        Next i
    End Sub
End Module
```

É equivalente ao seguinte código:

```vb
Module Test
    Sub Main()
        Dim i As Integer
        Dim c As IntegerCollection = New IntegerCollection()

        Dim e As IntegerCollection.IntegerCollectionEnumerator

        e = c.GetEnumerator()
        While e.MoveNext()
            i = e.Current

            Console.WriteLine(i)
        End While
    End Sub
End Module
```

Se o tipo `E` do enumerador implementa `System.IDisposable`, em seguida, o enumerador é descartado ao sair do loop, chamando o `Dispose` método. Isso garante que os recursos mantidos pelo enumerador são liberados. Se o método que contém o `For Each` instrução não usa o tratamento de erro não estruturados, o `For Each` instrução é encapsulado em um `Try` instrução com o `Dispose` método chamado no `Finally` para garantir a limpeza.

__Observação.__ O `System.Array` é um tipo de coleção e, pois a matriz por todos os tipos derivam `System.Array`, qualquer expressão de tipo de matriz é permitida em um `For Each` instrução. Para matrizes unidimensionais, o `For Each` instrução enumera os elementos de matriz em ordem crescente de índice, começando com o índice 0 e terminando com o índice de comprimento - 1. Para matrizes multidimensionais, os índices da dimensão mais à direita são aumentados pela primeira vez.

Por exemplo, o código a seguir imprime `1 2 3 4`:

```vb
Module Test
    Sub Main()
        Dim x(,) As Integer = { { 1, 2 }, { 3, 4 } }
        Dim i As Integer

        For Each i In x
            Console.Write(i & " ")
        Next i
    End Sub
End Module
```

Não é válido ramificar em uma `For Each` bloco de instruções de fora do bloco.


## <a name="exception-handling-statements"></a>Instruções de manipulação de exceção

Visual Basic oferece suporte a tratamento de exceções estruturado e tratamento de exceções não estruturados. Apenas um estilo de tratamento de exceções pode ser usado em um método, mas o `Error` instrução pode ser usada na manipulação de exceção estruturada. Se um método usa os dois estilos de tratamento de exceções, um erro de tempo de compilação ocorre.

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a>Instruções de tratamento de exceções estruturadas

Tratamento de exceções estruturado é um método de tratamento de erros por meio da declaração explícitos blocos dentro do qual determinadas exceções serão tratadas. Tratamento de exceções estruturado é feito por meio de um `Try` instrução.

```antlr
StructuredErrorStatement
    : ThrowStatement
    | TryStatement
    ;

TryStatement
    : 'Try' StatementTerminator
      Block?
      CatchStatement*
      FinallyStatement?
      'End' 'Try' StatementTerminator
    ;
```


Por exemplo:

```vb
Module Test
    Sub ThrowException()
        Throw New Exception()
    End Sub

    Sub Main()
        Try
            ThrowException()
        Catch e As Exception
            Console.WriteLine("Caught exception!")
        Finally
            Console.WriteLine("Exiting try.")
        End Try
    End Sub
End Module
```

Um `Try` instrução é composta por três tipos de blocos: blocos try, catch blocos e finalmente blocos. Um *bloco try* é um bloco de instrução que contém as instruções a serem executadas. Um *bloco catch* é um bloco de instrução que trata uma exceção. Um *bloco finally* é um bloco de instrução que contém instruções para ser executado quando o `Try` instrução é finalizada, independentemente de uma exceção tenha ocorrido e foi tratada. Um `Try` instrução, que pode conter apenas uma tentativa de bloco e outro, por fim, bloquear, deve conter pelo menos um bloco catch ou bloco finally. Não é válido para transferir a execução em um bloco try, exceto de um bloco catch na mesma instrução explicitamente.


#### <a name="finally-blocks"></a>Blocos Finally

Um `Finally` bloco sempre será executado quando a execução deixar qualquer parte do `Try` instrução. Nenhuma ação explícita é necessária para executar o `Finally` bloco; quando a execução deixa a `Try` instrução, o sistema será executado automaticamente o `Finally` bloquear e, em seguida, transferir a execução para seu destino pretendido. O `Finally` bloco é executado, independentemente de como a execução deixa o `Try` instrução: até o final do `Try` bloco, até o final de um `Catch` bloquear, por meio um `Exit Try` instrução, por meio um `GoTo` instrução, ou por não manipular uma exceção gerada.

Observe que o `Await` expressão em um método assíncrono e o `Yield` instrução em um método iterador, pode fazer com que o fluxo de controle para a instância de método assíncrono ou iterador de suspensão e retomada em alguma outra instância de método. No entanto, isso é simplesmente uma suspensão de execução e não envolve sair a instância de método assíncrono respectivo método ou iterador e portanto, não faz com que `Finally` blocos a serem executadas.

Não é válido para transferir explicitamente a execução em um `Finally` bloquear; também é inválido para transferir a execução de um `Finally` bloquear, exceto por meio de uma exceção.

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a>Blocos catch

Se ocorrer uma exceção ao processar o `Try` bloquear, cada `Catch` instrução é examinada na ordem textual para determinar se ele trata a exceção.

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

O identificador especificado em um `Catch` cláusula representa a exceção foi lançada. Se o identificador contém um `As` cláusula e, em seguida, o identificador é considerado declarado dentro de `Catch` espaço de declaração de local do bloco. Caso contrário, o identificador deve ser uma variável local (não uma variável estática) que foi definida em um bloco recipiente.

Um `Catch` cláusula sem nenhum identificador irá capturar todas as exceções derivadas do `System.Exception`. Um `Catch` cláusula com um identificador só irá capturar exceções cujos tipos são iguais ou derivam do tipo do identificador. O tipo deve ser `System.Exception`, ou um tipo derivado de `System.Exception`. Quando uma exceção é detectada, o que é derivada de `System.Exception`, uma referência para o objeto de exceção é armazenada no objeto retornado pela função `Microsoft.VisualBasic.Information.Err`.

Um `Catch` cláusula com um `When` cláusula somente capturar exceções quando a expressão é avaliada como `True`; o tipo da expressão deve ser uma expressão booliana de acordo com a seção [expressões Boolianas](expressions.md#boolean-expressions). Um `When` cláusula só será aplicada depois de verificar se o tipo da exceção e a expressão pode se referir ao identificador que representa a exceção, como demonstra este exemplo:

```vb
Module Test
    Sub Main()
        Dim i As Integer = 5

        Try
            Throw New ArgumentException()
        Catch e As OverflowException When i = 5
            Console.WriteLine("First handler")
        Catch e As ArgumentException When i = 4
            Console.WriteLine("Second handler")
        Catch When i = 5
            Console.WriteLine("Third handler")
        End Try

    End Sub
End Module
```

Este exemplo imprime:

```
Third handler
```

Se um `Catch` cláusula manipula a exceção, a execução é transferida para o `Catch` bloco. No final o `Catch` bloquear transferências de execução para a primeira instrução após a `Try` instrução. O `Try` instrução não tratará as exceções geradas em um `Catch` bloco. Se nenhum `Catch` cláusula manipula a exceção, transferências de execução para um local determinado pelo sistema.

Não é válido para transferir explicitamente a execução em um `Catch` bloco.

Os filtros no quando as cláusulas normalmente são avaliadas antes da exceção sendo lançada. Por exemplo, o seguinte código imprimirá "Filtro, por fim, Catch".

```vb
Sub Main()
   Try
       Foo()
   Catch ex As Exception When F()
       Console.WriteLine("Catch")
   End Try
End Sub

Sub Foo()
    Try
        Throw New Exception
    Finally
        Console.WriteLine("Finally")
    End Try
End Sub

Function F() As Boolean
    Console.WriteLine("Filter")
    Return True
End Function
```

 No entanto, os métodos assíncrono e iterador causam, por fim, todos os blocos dentro deles para ser executado antes de qualquer filtro de fora. Por exemplo, se o código acima tivesse `Async Sub Foo()`, em seguida, a saída seria "Por fim, filtrar, Catch".


#### <a name="throw-statement"></a>Instrução Throw

O `Throw` instrução gera uma exceção, o que é representada por uma instância de um tipo derivado de `System.Exception`.

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

Se a expressão não é classificada como um valor ou não é um tipo derivado de `System.Exception`, em seguida, ocorre um erro de tempo de compilação. Se a expressão for avaliada como um valor nulo em tempo de execução, em seguida, um `System.NullReferenceException` exceção é acionada em vez disso.

Um `Throw` declaração pode omitir a expressão dentro de um bloco catch de uma `Try` instrução, desde que não há nenhuma intervenção bloco finally. Nesse caso, a instrução Relança a exceção sendo manipulada no momento dentro do bloco catch. Por exemplo:

```vb
Sub Test(x As Integer)
    Try
        Throw New Exception()
    Catch
        If x = 0 Then
            Throw    ' OK, rethrows exception from above.
        Else
            Try
                If x = 1 Then
                    Throw   ' OK, rethrows exception from above.
                End If
            Finally
                Throw    ' Invalid, inside of a Finally.
            End Try
        End If
    End Try
End Sub
```


### <a name="unstructured-exception-handling-statements"></a>Instruções de manipulação de exceção não estruturadas

Tratamento de exceção não estruturados é um método de tratamento de erros, indicando as instruções para ramificar para quando ocorre uma exceção. Tratamento de exceção não estruturados é implementado usando três instruções: o `Error` instrução, o `On Error` instrução e o `Resume` instrução.

```antlr
UnstructuredErrorStatement
    : ErrorStatement
    | OnErrorStatement
    | ResumeStatement
    ;
```

Por exemplo:

```vb
Module Test
    Sub ThrowException()
        Error 5
    End Sub

    Sub Main()
        On Error GoTo GotException

        ThrowException()
        Exit Sub

GotException:
        Console.WriteLine("Caught exception!")
        Resume Next
    End Sub
End Module
```

Quando um método usa o tratamento de exceções não estruturados, um manipulador de exceção estruturada único é estabelecido para todo o método que captura todas as exceções. (Observe que em construtores esse manipulador não se estende sobre a chamada para a chamada para `New` no início do construtor.) O método, em seguida, mantém o controle do local mais recente do manipulador de exceção e a exceção mais recente que foi lançada. Na entrada para o método, o local do manipulador de exceção e a exceção são definidas como `Nothing`. Quando uma exceção é gerada em um método que usa tratamento de exceções não estruturados, uma referência para o objeto de exceção é armazenada no objeto retornado pela função `Microsoft.VisualBasic.Information.Err`.

Instruções de tratamento de erros não estruturado não são permitidas em métodos de iterador ou assíncrono.


#### <a name="error-statement"></a>Instrução Error

Uma `Error` instrução gera uma `System.Exception` exceção contendo um número de exceção do Visual Basic 6. A expressão deve ser classificada como um valor e seu tipo deve ser implicitamente conversível para `Integer`.

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a>Instrução On Error

Um `On Error` instrução modifica o estado de manipulação de exceção mais recente.

```antlr
OnErrorStatement
    : 'On' 'Error' ErrorClause StatementTerminator
    ;

ErrorClause
    : 'GoTo' '-' '1'
    | 'GoTo' '0'
    | GoToStatement
    | 'Resume' 'Next'
    ;
```

Ele pode ser usado em uma das quatro maneiras:

* `On Error GoTo -1` Redefine a exceção mais recente para `Nothing`.

* `On Error GoTo 0` Redefine o local mais recente do manipulador de exceção para `Nothing`.

* `On Error GoTo LabelName` estabelece o rótulo como o local mais recente do manipulador de exceção. Essa instrução não pode ser usada em um método que contém uma expressão lambda ou de consulta.

* `On Error Resume Next` estabelece o `Resume Next` comportamento como o local mais recente do manipulador de exceção.


#### <a name="resume-statement"></a>Instrução Resume

Um `Resume` instrução retorna a execução para a instrução que causou a exceção mais recente.

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

Se o `Next` modificador é especificado, a execução retorna para a instrução que deveriam ter sido executada após a instrução que causou a exceção mais recente. Se um nome de rótulo for especificado, a execução retorna para o rótulo.

Porque o `SyncLock` instrução contém um bloco de tratamento de erros estruturado implícito, `Resume` e `Resume Next` ter comportamentos especiais para exceções que ocorrem no `SyncLock` instruções. `Resume` Retorna a execução para o início dos `SyncLock` instrução, enquanto `Resume Next` retorna a execução para a próxima instrução que segue o `SyncLock` instrução. Por exemplo, considere o seguinte código:

```vb
Class LockClass
End Class

Module Test
    Sub Main()
        Dim FirstTime As Boolean = True
        Dim Lock As LockClass = New LockClass()

        On Error GoTo Handler

        SyncLock Lock
            Console.WriteLine("Before exception")
            Throw New Exception()
            Console.WriteLine("After exception")
        End SyncLock

        Console.WriteLine("After SyncLock")
        Exit Sub

Handler:
        If FirstTime Then
            FirstTime = False
            Resume
        Else
            Resume Next
        End If
    End Sub
End Module
```

Ele imprime o resultado a seguir.

```
Before exception
Before exception
After SyncLock
```

A primeira vez que o `SyncLock` instrução, `Resume` retorna a execução para o início do `SyncLock` instrução. Na segunda vez por meio de `SyncLock` instrução, `Resume Next` retorna a execução até o final do `SyncLock` instrução. `Resume` e `Resume Next` não são permitidas dentro de um `SyncLock` instrução.

Em todos os casos, quando um `Resume` instrução é executada, a exceção mais recente é definida como `Nothing`. Se um `Resume` instrução for executada com nenhuma exceção mais recente, a instrução gera uma `System.Exception` exceção que contém o número de erro do Visual Basic `20` (retomar sem erro).


## <a name="branch-statements"></a>Instruções Branch

Instruções Branch modificam o fluxo de execução em um método. Há seis instruções de ramificação:

1. Um `GoTo` instrução faz com que a execução transferir para o rótulo especificado no método. Não é permitido `GoTo` em um `Try`, `Using`, `SyncLock`, `With`, `For` ou `For Each` bloco, nem em qualquer bloco de loop se uma variável local desse bloco é capturada em um lambda ou expressão LINQ.
2. Um `Exit` instrução transfere a execução para a próxima instrução após o término da instrução do bloco imediatamente contido do tipo especificado. Se o bloco é o bloco de método, o fluxo de controle sai do método, conforme descrito na seção [fluxo de controle](statements.md#control-flow). Se o `Exit` instrução não está contida dentro do tipo de bloco especificado na instrução, ocorre um erro de tempo de compilação.
3. Um `Continue` instrução transfere a execução até o final da instrução de loop de bloco imediatamente contido do tipo especificado. Se o `Continue` instrução não está contida dentro do tipo de bloco especificado na instrução, ocorre um erro de tempo de compilação.
4. Um `Stop` instrução faz com que uma exceção do depurador ocorra.
5. Um `End` instrução encerra o programa. Os finalizadores são executados antes do desligamento, mas os blocos finally de qualquer execução atualmente `Try` instruções não são executadas. Essa instrução não pode ser usada em programas que não são executáveis (por exemplo, DLLs).
6. Um `Return` instrução com nenhuma expressão é equivalente a um `Exit Sub` ou `Exit Function` instrução. Um `Return` instrução com uma expressão é permitida somente em um método regular que é uma função ou em um método assíncrono que é uma função com o tipo de retorno `Task(Of T)` para alguns `T`. A expressão deve ser classificada como um valor que é implicitamente conversível para o *variável de retorno da função* (no caso de métodos regulares) ou o *variável de retorno de tarefa* (no caso de métodos assíncronos) . Seu comportamento é avaliar sua expressão, em seguida, armazená-lo na variável de retorno, em seguida, execute implícito `Exit Function` instrução.

```antlr
BranchStatement
    : GoToStatement
    | ExitStatement
    | ContinueStatement
    | StopStatement
    | EndStatement
    | ReturnStatement
    ;

GoToStatement
    : 'GoTo' LabelName StatementTerminator
    ;

ExitStatement
    : 'Exit' ExitKind StatementTerminator
    ;

ExitKind
    : 'Do' | 'For' | 'While' | 'Select' | 'Sub' | 'Function' | 'Property' | 'Try'
    ;

ContinueStatement
    : 'Continue' ContinueKind StatementTerminator
    ;

ContinueKind
    : 'Do' | 'For' | 'While'
    ;

StopStatement
    : 'Stop' StatementTerminator
    ;

EndStatement
    : 'End' StatementTerminator
    ;

ReturnStatement
    : 'Return' Expression? StatementTerminator
    ;
```

## <a name="array-handling-statements"></a>Instruções de tratamento de matriz

Duas instruções simplificam o trabalho com matrizes: `ReDim` instruções e `Erase` instruções.

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a>Instrução ReDim

Um `ReDim` instrução instancia novas matrizes.

```antlr
RedimStatement
    : 'ReDim' 'Preserve'? RedimClauses StatementTerminator
    ;

RedimClauses
    : RedimClause ( Comma RedimClause )*
    ;

RedimClause
    : Expression ArraySizeInitializationModifier
    ;
```

Cada cláusula na instrução deve ser classificada como uma variável ou um acesso de propriedade cujo tipo é um tipo de matriz ou `Object`e ser seguido por uma lista de limites de matriz. O número de limites deve ser consistente com o tipo da variável; qualquer número de limites é permitido para `Object`. Em tempo de execução, uma matriz é instanciada para cada expressão da esquerda para a direita com os limites especificados e, em seguida, é atribuída à variável ou propriedade. Se for o tipo de variável `Object`, o número de dimensões é o número de dimensões especificadas e o tipo de elemento da matriz é `Object`. Se o número determinado de dimensões é incompatível com a variável ou propriedade em tempo de execução ocorrerá um erro de tempo de compilação. Por exemplo:

```vb
Module Test
    Sub Main()
        Dim o As Object
        Dim b() As Byte
        Dim i(,) As Integer

        ' The next two statements are equivalent.
        ReDim o(10,30)
        o = New Object(10, 30) {}

        ' The next two statements are equivalent.
        ReDim b(10)
        b = New Byte(10) {}

        ' Error: Incorrect number of dimensions.
        ReDim i(10, 30, 40)
    End Sub
End Module
```

Se o `Preserve` palavra-chave for especificado, em seguida, as expressões devem ser classificáveis como um valor e o novo tamanho para cada dimensão, exceto pelo mais à direita deve ser igual ao tamanho da matriz existente. Os valores na matriz existente são copiados para a nova matriz: se a nova matriz for menor, os valores existentes são descartados; Se a nova matriz é maior, os elementos adicionais serão inicializados com o valor padrão do tipo de elemento da matriz. Por exemplo, considere o seguinte código:

```vb
Module Test
    Sub Main()
        Dim x(5, 5) As Integer

        x(3, 3) = 3

        ReDim Preserve x(5, 6)
        Console.WriteLine(x(3, 3) & ", " & x(3, 6))
    End Sub
End Module
```

Imprime o seguinte resultado:

```
3, 0
```

Se a referência de matriz existente for um valor nulo em tempo de execução, nenhum erro será mostrado. Diferente da dimensão mais à direita, se o tamanho de uma dimensão for alterada, um `System.ArrayTypeMismatchException` será lançada.

__Observação.__ `Preserve` não é uma palavra reservada.


### <a name="erase-statement"></a>Instrução Erase

Uma `Erase` instrução define cada uma das variáveis de matriz ou propriedades especificadas na instrução para `Nothing`. Cada expressão na instrução deve ser classificada como um acesso de propriedade ou variável cujo tipo é um tipo de matriz ou `Object`. Por exemplo:

```vb
Module Test
    Sub Main()
        Dim x() As Integer = New Integer(5) {}

        ' The following two statements are equivalent.
        Erase x
        x = Nothing
    End Sub
End Module
```

```antlr
EraseStatement
    : 'Erase' EraseExpressions StatementTerminator
    ;

EraseExpressions
    : Expression ( Comma Expression )*
    ;
```

## <a name="using-statement"></a>instrução Using

Instâncias de tipos são liberadas automaticamente pelo coletor de lixo quando uma coleção é executada e nenhuma referência ao vivo para a instância é encontrada. Se um tipo for em um recurso particularmente valioso e escasso (como conexões de banco de dados ou identificadores de arquivos), ele não pode ser desejável esperar até a próxima coleta de lixo para limpar uma determinada instância do tipo que não está mais em uso. Para fornecer uma maneira simples de liberar recursos antes de uma coleção, um tipo pode implementar o `System.IDisposable` interface. Um tipo que exponha um `Dispose` método que pode ser chamado para forçar a recursos valiosos para ser liberado imediatamente, assim:

```vb
Module Test
    Sub Main()
        Dim x As DBConnection = New DBConnection("...")

        ' Do some work
        ...

        x.Dispose()        ' Free the connection
    End Sub
End Module
```

O `Using` instrução automatiza o processo de aquisição de um recurso, executando um conjunto de instruções e, em seguida, descarte do recurso. A instrução pode ocorrer de duas formas: em um, o recurso é uma variável local declarada como parte da instrução e tratado como uma instrução de declaração de variável local regular; no outro, o recurso é o resultado de uma expressão.

```antlr
UsingStatement
    : 'Using' UsingResources StatementTerminator
      Block?
      'End' 'Using' StatementTerminator
    ;

UsingResources
    : VariableDeclarators
    | Expression
    ;
```

Se o recurso é uma instrução de declaração de variável local, o tipo de declaração de variável local deve ser um tipo que pode ser convertido implicitamente em `System.IDisposable`. As variáveis locais declaradas são somente leitura, como escopo o `Using` instrução bloquear e deve incluir um inicializador. Se o recurso é o resultado de uma expressão, a expressão deve ser classificada como um valor e deve ser de um tipo que pode ser convertido implicitamente em `System.IDisposable`. A expressão é avaliada apenas uma vez, no início da instrução.

O `Using` bloco implicitamente contido por um `Try` instrução bloco finally chama o método `IDisposable.Dispose` no recurso. Isso garante que o recurso é descartado, mesmo quando uma exceção é lançada. Como resultado, não é válido ramificar em uma `Using` bloquear de fora do bloco e uma `Using` bloco é tratado como uma única instrução para os fins `Resume` e `Resume Next`. Se o recurso estiver `Nothing`, em seguida, nenhuma chamada para `Dispose` é feita. Portanto, o exemplo:

```vb
Using f As C = New C()
    ...
End Using
```

equivale a:

```vb
Dim f As C = New C()
Try
    ...
Finally
    If f IsNot Nothing Then
        f.Dispose()
    End If
End Try
```

Um `Using` instrução que tenha uma instrução de declaração de variável local possa adquirir vários recursos de cada vez, que é equivalente a aninhados `Using` instruções.  Por exemplo, um `Using` instrução do formulário:

```vb
Using r1 As R = New R(), r2 As R = New R()
    r1.F()
    r2.F()
End Using
```

equivale a:

```vb
Using r1 As R = New R()
    Using r2 As R = New R()
        r1.F()
        r2.F()
    End Using
End Using
```


## <a name="await-statement"></a>Instrução await

Uma instrução de espera tem a mesma sintaxe de uma expressão de operador de espera (seção [operador Await](expressions.md#await-operator)), só é permitido em métodos que também permitem que expressões await e tem o mesmo comportamento que uma expressão de operador de espera.

No entanto, ele poderá ser classificado como um valor ou nulo. Qualquer valor resultante da avaliação da expressão de operador de espera será descartada.

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a>Instrução Yield

Instruções yield estão relacionadas aos métodos de iterador, que são descritos na seção [métodos de iterador](statements.md#iterator-methods).

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

`Yield` é uma palavra reservada se a expressão de lambda ou método imediatamente delimitador no qual ele aparece tem um `Iterator` modificador e se o `Yield` aparece depois que `Iterator` modificador; é não reservado em outro lugar. Também é reservado em diretivas de pré-processador. A instrução yield só é permitida no corpo de uma expressão lambda ou método em que ele é uma palavra reservada. Dentro do método ou lambda imediatamente delimitador, a instrução yield pode não ocorrer dentro do corpo de uma `Catch` ou `Finally` bloquear nou dentro do corpo de um `SyncLock` instrução.

A instrução yield leva a uma única expressão que deve ser classificado como um valor e cujo tipo é implicitamente conversível para o tipo dos *variável do iterador atual* (seção [métodos de iterador](statements.md#iterator-methods)) do seu método delimitador do iterador.

Fluxo de controle só atinge uma `Yield` instrução quando o `MoveNext` método é invocado em um objeto de iterador. (Isso ocorre porque uma instância de método iterador só executa suas instruções devido à `MoveNext` ou `Dispose` métodos que está sendo chamados em um objeto de iterador; e o `Dispose` método só executará o código em `Finally` blocos, onde `Yield` não é permitido).

Quando um `Yield` instrução for executada, sua expressão é avaliada e armazenada na *variável do iterador atual* da instância do método iterador associada a esse objeto de iterador. O valor `True` é retornado ao chamador `MoveNext`, e o ponto de controle dessa instância é interrompida avançando até a próxima invocação de `MoveNext` no objeto de iterador.

