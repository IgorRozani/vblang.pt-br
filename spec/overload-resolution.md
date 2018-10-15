### <a name="overloaded-method-resolution"></a>Resolução do método sobrecarregado

Na prática, as regras para determinar a resolução de sobrecarga devem encontrar a sobrecarga que é "mais próxima" para os argumentos reais fornecidos. Se houver um método cujos tipos de parâmetro correspondem aos tipos de argumento, em seguida, esse método é obviamente o mais próximo. Bloqueando que, um método é mais próximo do que outro se todos os seus tipos de parâmetro são mais estreitos do que (ou igual a) os tipos de parâmetro de outro método. Se os parâmetros do método, nem são mais estreitos do que o outro, em seguida, não é possível para determinar qual método é mais próximo para os argumentos.

__Observação.__ Resolução de sobrecarga não leva em conta o tipo de retorno esperado do método. 

Observe também que, devido à sintaxe do parâmetro nomeado, a ordem dos parâmetros formais e reais pode não ser o mesmo.

Dado um grupo de método, o método mais aplicável no grupo para uma lista de argumentos é determinado usando as etapas a seguir. Se, após a aplicação de uma determinada etapa, nenhum membro permanecem no conjunto, ocorrerá um erro de tempo de compilação. Se apenas um membro permanece no conjunto, esse membro é o membro mais aplicável. As etapas são:

1.  Primeiro, não se tiver sido fornecido nenhum argumento de tipo, aplique inferência de tipo sobre quaisquer métodos que têm parâmetros de tipo. Se a inferência de tipo for bem-sucedida para um método, os argumentos de tipo inferidos são usados para esse método específico. Se a inferência de tipo falhar para um método, esse método é eliminado do conjunto.

2.  Em seguida, eliminar todos os membros do conjunto que estão inacessível ou não aplicável (seção [aplicabilidade à lista de argumentos](overload-resolution.md#applicability-to-argument-list)) à lista de argumentos

3.  Em seguida, se um ou mais argumentos forem `AddressOf` ou expressões lambda, em seguida, calcule a *níveis relaxamento de delegado* para cada argumento tal como mostrado abaixo. Se o pior (menor) delegar nível relaxamento `N` é pior do que o nível mais baixo de relaxamento de delegado no `M`, eliminar, em seguida, `N` do conjunto. Os níveis de relaxamento de delegado são da seguinte maneira:

    31. *Nível de relaxamento de delegado de erro* - - se a `AddressOf` ou lambda não pode ser convertido para o tipo de delegado.
  
    32. *Relaxamento de delegado do tipo de retorno ou parâmetros de estreitamento* – se o argumento for `AddressOf` ou um lambda com um tipo de declaração e a conversão de seu tipo de retorno para o delegado de retorno tipo é restritiva; ou se o argumento for um lambda regular e a conversão de qualquer uma das suas expressões de retorno para o delegado de retorno é de tipo de restrição ou se o argumento for um lambda async e o delegado de retorno tipo é `Task(Of T)` e a conversão de qualquer uma das suas expressões de retorno para `T` é restritiva; ou se o argumento é um lambda de iterador e tipo de retorno do delegado `IEnumerator(Of T)` ou `IEnumerable(Of T)` e a conversão de qualquer um dos seus operandos yield para `T` é restritiva.

    33. *Relaxamento de delegado para delegar sem assinatura de ampliação* - - que é do tipo de delegado `System.Delegate` ou `System.MultiCastDelegate` ou `System.Object`.

    34. *Descartar o relaxamento de delegado de retorno ou argumentos* – se o argumento for `AddressOf` ou um lambda com um tipo de retorno declarado e o tipo de delegado não tem um tipo de retorno; ou se o argumento for um lambda com uma ou mais expressões e o tipo de delegado de retorno não tem um tipo de retorno; ou se o argumento for `AddressOf` ou lambda sem parâmetros e o tipo de delegado tem parâmetros.

    35. *Relaxamento de delegado do tipo de retorno de ampliação* – se o argumento for `AddressOf` ou lambda com um tipo de retorno declarado, e há uma conversão de ampliação do seu tipo de retorno do delegado; ou se o argumento for um lambda regular em que o conversão de todas as expressões de retornadas para o tipo de retorno do delegado é de ampliação ou a identidade com pelo menos uma ampliação; ou se o argumento for um lambda async e o delegado é `Task(Of T)` ou `Task` e a conversão de todas as expressões de retornadas para `T` / `Object` respectivamente é de ampliação ou a identidade pelo menos uma ampliação; ou se o argumento é um lambda de iterador e é o delegado `IEnumerator(Of T)` ou `IEnumerable(Of T)` ou `IEnumerator` ou `IEnumerable` e a conversão de todas as expressões de retornadas para `T` / `Object` está ampliando ou identidade com pelo menos um de ampliação.

    36. *Relaxamento de delegado de identidade* – se o argumento for um `AddressOf` ou um lambda que corresponde ao delegado exatamente, sem nenhum ampliação ou redução ou descarte de parâmetros ou retorna ou produz. Em seguida, se alguns membros do conjunto de conversões de estreitamento não exigindo para ser aplicáveis a qualquer um dos argumentos, em seguida, elimine todos os membros que fazem. Por exemplo:

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

4.  Em seguida, a eliminação é feita com base na redução da seguinte maneira. (Observe que, se Option Strict for On, em seguida, todos os membros que exigem a restrição já foi considerados inaplicáveis (seção [aplicabilidade à lista de argumentos](overload-resolution.md#applicability-to-argument-list)) e removido pela etapa 2.)

    41. Se alguns membros de instância do conjunto de exigem apenas onde o tipo de expressão de argumento é de conversões de estreitamento `Object`, em seguida, eliminar todos os outros membros.
    42. Se o conjunto contém mais de um membro que exige a restringir apenas do `Object`, em seguida, a expressão de destino de invocação é reclassificada como um método de associação tardia de acesso (e recebe um erro se o tipo que contém o grupo de método é uma interface, ou se qualquer um dos os membros aplicáveis foram membros de extensão).
    43. Se houver qualquer candidatos que exigem apenas o estreitamento de literais numéricos, em seguida, escolheu mais específica entre todos os candidatos restantes pelas etapas a seguir. Se o vencedor requer apenas estreitamento de literais numéricos, em seguida, ele é escolhido como o resultado da resolução de sobrecarga; Caso contrário, ele é um erro.

    __Observação.__ A justificativa para essa regra é que, se um programa é fracamente tipado (ou seja, a maioria ou todas as variáveis são declaradas como `Object`), a resolução de sobrecarga pode ser difícil quando várias conversões de `Object` são reduzidas. Em vez de ter a resolução de sobrecarga falha em muitas situações (exigir a rigidez de tipos dos argumentos para a chamada de método), a resolução sobrecarregado apropriado método a ser chamado é adiado até o tempo de execução. Isso permite que a chamada tipagem seja bem-sucedida sem conversões adicionais. Um efeito colateral indesejado-isso, no entanto, é executar a chamada de associação tardia requer conversão para o destino de chamada `Object`. No caso de um valor de estrutura, isso significa que o valor deve ser boxed a um temporário. Se o método chamado eventualmente tenta alterar um campo da estrutura, essa alteração serão perdida quando o método retorna. Interfaces são excluídos dessa regra especial porque a associação tardia sempre é resolvido em relação aos membros do tempo de execução classe ou estrutura de tipo, que pode ter nomes diferentes que os membros das interfaces que elas implementam.

5.  Em seguida, se nenhum método de instância permanecem no conjunto que não necessitam de redução, elimine todos os métodos de extensão do conjunto. Por exemplo:

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

    __Observação.__ Métodos de extensão são ignorados, se houver métodos de instância aplicável para garantir que a adição de uma importação (que pode trazer novos métodos de extensão para o escopo) não causará uma chamada em um método de instância existente para associar novamente para um método de extensão. Dado o escopo amplo de alguns métodos de extensão (ou seja, aquelas definidas em interfaces e/ou parâmetros de tipo), isso é uma abordagem mais segura para associação aos métodos de extensão.

6.  Em seguida, if, considerando qualquer dois membros do conjunto de `M` e `N`, `M` é mais *específico* (seção [especificidade dos membros/tipos dada uma lista de argumentos](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) que `N`dada a lista de argumentos, eliminar `N` do conjunto. Se mais de um membro permanece no conjunto e os membros restantes não são igualmente específicos recebe a lista de argumentos, um erro de tempo de compilação ocorre.

7.  Caso contrário, considerando qualquer dois membros do conjunto de `M` e `N`, aplique as seguintes regras de desempate em ordem:

    71. Se `M` não tem um parâmetro ParamArray mas `N` faz, ou se ambos fazem mas `M` passa menos argumentos para o parâmetro ParamArray que `N` , em seguida, eliminar `N` do conjunto. Por exemplo:

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

        O exemplo acima produz a saída a seguir:

        ```
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        __Observação.__ Quando uma classe declara um método com um parâmetro paramarray, não é incomum para incluir também algumas das formas expandidas como métodos regulares. Ao fazer isso, é possível evitar a alocação de uma matriz que ocorre quando uma forma expandida de um método com um parâmetro paramarray instância é invocada.

    72. Se `M` é definido em um tipo mais derivado `N`, eliminar `N` do conjunto. Por exemplo:

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

        Essa regra também se aplica aos tipos de métodos de extensão são definidos no. Por exemplo:

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

    73. Se `M` e `N` são os métodos de extensão e o tipo de destino `M` é uma classe ou estrutura e o tipo de destino `N` é uma interface, eliminar `N` do conjunto. Por exemplo:

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

    74. Se `M` e `N` são os métodos de extensão e o tipo de destino `M` e `N` sejam idênticos após a substituição de parâmetro de tipo e o tipo de destino `M` antes do tipo de substituição de parâmetro não contém Digite os parâmetros, mas o tipo de destino `N` , tem menos parâmetros de tipo que o tipo de destino `N`, eliminar `N` do conjunto. Por exemplo:

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

    75. Antes do tipo de argumentos tiverem sido substituídos, se `M` está *menos genérica* (seção [generalidade](overload-resolution.md#genericity)) que `N`, eliminar `N` do conjunto.

    76. Se `M` não é um método de extensão e `N` é, eliminar `N` do conjunto.

    77. Se `M` e `N` são métodos de extensão e `M` foi encontrado antes `N` (seção [coleção de método de extensão](overload-resolution.md#extension-method-collection)), eliminar `N` do conjunto. Por exemplo:

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

        Se os métodos de extensão foram encontrados na mesma etapa, esses métodos de extensão são ambíguos. A chamada pode sempre ser a ambiguidade removida usando o nome do módulo padrão que contém o método de extensão e chamando o método de extensão como se fosse um membro regular. Por exemplo:

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

    78. Se `M` e `N` ambos necessários inferência de tipo para produzir os argumentos de tipo, e `M` não exigia determinando o tipo dominante para qualquer um dos argumentos de tipo (ou seja, cada os argumentos de tipo inferidos para um único tipo), mas `N`fez, eliminar `N` do conjunto.

        __Observação.__ Essa regra garante que resolução de sobrecarga que teve êxito em versões anteriores (onde inferir vários tipos para um argumento de tipo causaria um erro), continue produzir os mesmos resultados.

    79. Se a resolução de sobrecarga está sendo feita para resolver o destino de uma expressão de criação de delegado de um `AddressOf` expressão e tanto o delegado e `M` são funções enquanto `N` é uma sub-rotina, eliminar `N` do conjunto. Da mesma forma, se tanto o delegado e `M` são as sub-rotinas, enquanto `N` é uma função, eliminar `N` do conjunto.

    710. Se `M` não tiver usado os padrões de parâmetro opcional no lugar de argumentos explícitos, mas `N` , em seguida, fez, eliminar `N` do conjunto.

    711. Antes do tipo de argumentos tiverem sido substituídos, se `M` tem *maior profundidade de generalidade* (seção [generalidade](overload-resolution.md#genericity)) que `N`, em seguida, eliminar `N` do conjunto.

8. Caso contrário, a chamada é ambígua e ocorrer um erro de tempo de compilação.

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a>Especificidade dos membros/tipos dada uma lista de argumentos

Um membro `M` é considerado *igualmente específicos* como `N`, dada uma lista de argumentos `A`, se suas assinaturas são as mesmas ou se cada parâmetro de tipo no `M` é o mesmo que o correspondente tipo de parâmetro em `N`.

__Observação.__ Dois membros podem terminar em um grupo de método com a mesma assinatura devido a métodos de extensão. Dois membros podem também ser igualmente específicos, mas não têm a mesma assinatura devido a parâmetros de tipo ou expansão paramarray.

Um membro `M` é considerado *mais específicos* que `N` se suas assinaturas são diferentes e pelo menos um parâmetro de tipo na `M` é mais específica que um tipo de parâmetro no `N`e nenhum tipo de parâmetro na `N` é mais específica que um tipo de parâmetro no `M`. Dado um par de parâmetros `Mj` e `Nj` que corresponde ao argumento `Aj`, o tipo de `Mj` é considerado *mais específicos* que o tipo do `Nj` se um dos seguintes condições for verdadeira:

* Existe uma conversão de ampliação do tipo de `Mj` para o tipo `Nj`. (__Observação.__ Porque os tipos de parâmetros estão sendo comparados sem levar em consideração o argumento real nesse caso, o conversão de ampliação de expressões de constante para um tipo numérico que do valor se encaixa não é considerado neste caso.)

* `Aj` é o literal `0`, `Mj` é um tipo numérico e `Nj` é um tipo enumerado. (__Observação.__ Essa regra é necessária porque o literal `0` é ampliado para qualquer tipo enumerado. Uma vez que um tipo enumerado é ampliado para seu tipo subjacente, isso significa que resolução de sobrecarga em `0` será, por padrão, prefira tipos enumerados tipos numéricos. Recebemos muitos comentários que esse comportamento foi contra-intuitivo.)

* `Mj` e `Nj` são ambos os tipos numéricos, e `Mj` vem anteriores ao `Nj` na lista `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`. (__Observação.__ A regra sobre os tipos numéricos é útil porque os tipos numéricos assinados e não assinados de um tamanho específico têm apenas reduzir conversões entre eles. A regra acima quebra o vínculo entre os dois tipos em favor do tipo numérico mais "natural". Isso é particularmente importante quando fazendo a resolução de sobrecarga em um tipo que é ampliado para os dois os assinados e tipos numéricos de um tamanho específico, por exemplo, um literal numérico que se encaixa em ambos.)

* `Mj` e `Nj` é delegado tipos de função e o tipo de retorno `Mj` é mais específico do que o tipo de retorno `Nj` se `Aj` é classificado como um método lambda, e `Mj` ou `Nj` é `System.Linq.Expressions.Expression(Of T)`, em seguida, o argumento de tipo do tipo (supondo que ele é um tipo delegado) é substituído para o tipo que estão sendo comparado.

* `Mj` é idêntico ao tipo de `Aj`, e `Nj` não é. (__Observação.__ É interessante observar que a regra anterior é ligeiramente diferente do c#, em que c# requer que os tipos de função de delegado têm listas de parâmetro idênticos antes de comparar tipos de retorno, enquanto o Visual Basic não.)

#### <a name="genericity"></a>Generalidade

Um membro `M` é determinado como *menos genérica* de um membro `N` da seguinte maneira:

1. Se, para cada par de parâmetros de correspondência `Mj` e `Nj`, `Mj` é menor que ou igualmente genérico que `Nj` em relação aos parâmetros de tipo no método e pelo menos um `Mj` é menos genérica com respeito ao tipo parâmetros do método.
2. Caso contrário, se para cada par de parâmetros de correspondência `Mj` e `Nj`, `Mj` é menor que ou igualmente genérico que `Nj` em relação aos parâmetros de tipo no tipo e pelo menos um `Mj` é menos genérica com respeito ao tipo parâmetros do tipo, em seguida `M` é menos genérica que `N`.

Um parâmetro `M` é considerada igualmente genérico a um parâmetro `N` se seus tipos `Mt` e `Nt` fazem referência aos parâmetros de tipo ou ambos não faz referência aos parâmetros de tipo. `M` é considerado a ser menos genérica que `N` se `Mt` não faz referência a um parâmetro de tipo e `Nt` faz.

Por exemplo:

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

Parâmetros de tipo de método de extensão que foram corrigidos durante currying são considerados parâmetros de tipo no tipo, não os parâmetros de tipo no método. Por exemplo:

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

#### <a name="depth-of-genericity"></a>Profundidade de forma geral

Um membro `M` é determinado como ter *maior profundidade de generalidade* de um membro `N` se, para cada par de parâmetros correspondentes `Mj` e `Nj`, `Mj` tem maior ou igual a *profundidade de generalidade* que `Nj`e pelo menos um `Mj` é tem maior profundidade de forma geral. Profundidade de generalidade é definida da seguinte maneira:

* Algo diferente de um parâmetro de tipo tem maior profundidade de generalidade de um parâmetro de tipo;

* Recursivamente, um tipo construído tem maior profundidade de generalidade que outro tipo construído (com o mesmo número de argumentos de tipo) se pelo menos um argumento de tipo tem uma profundidade maior de generalidade e nenhum argumento de tipo menor profundidade que o tipo correspondente argumento na outra.

* Um tipo de matriz tem maior profundidade de generalidade que outro tipo de matriz (com o mesmo número de dimensões), se o tipo de elemento da primeira tem maior profundidade de forma geral que o tipo de elemento da segunda.

Por exemplo:

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

### <a name="applicability-to-argument-list"></a>Aplicabilidade à lista de argumentos

É um método *aplicável* a um conjunto de argumentos nomeados, se o método pode ser chamado usando as listas de argumento, argumentos posicionais e argumentos de tipo. As listas são comparadas com o parâmetro de listas de argumentos da seguinte maneira:

1. Primeiro, corresponde cada argumento posicional na ordem à lista de parâmetros de método. Se houver mais argumentos posicionais que os parâmetros e o último parâmetro não é um paramarray, o método não é aplicável. Caso contrário, o parâmetro paramarray é expandido com parâmetros do tipo de elemento paramarray para corresponder ao número de argumentos posicionais. Se um argumento posicional for omitido, que deveria ir para um paramarray, o método não é aplicável.
2. Em seguida, corresponde cada argumento nomeado para um parâmetro com o nome fornecido. Se um dos argumentos nomeados não corresponde, corresponde a um parâmetro paramarray ou corresponde a um argumento que já foi correspondido com o outro argumento posicional ou nomeado, o método não é aplicável.
3. Em seguida, se tiverem sido especificados argumentos de tipo, eles são correspondidos em relação à lista de parâmetro de tipo. Se as duas listas não tiverem o mesmo número de elementos, o método não é aplicável, a menos que a lista de argumentos de tipo está vazia. Se a lista de argumentos de tipo estiver vazia, inferência de tipo é usada para tentar inferir a lista de argumentos de tipo. Se a inferência de tipo falhar, o método não é aplicável. Caso contrário, os argumentos de tipo são preenchidos no lugar de parâmetros de tipo na assinatura. Se os parâmetros que não foram correspondidos não são opcionais, o método não é aplicável.
4. Se as expressões do argumento não são implicitamente conversíveis para os tipos dos parâmetros forem iguais, então o método não é aplicável.
5. Se um parâmetro é ByRef, e não há uma conversão implícita do tipo do parâmetro para o tipo do argumento, o método não é aplicável.
6. Se os argumentos de tipo violam as restrições do método (incluindo os argumentos de tipo inferido da etapa 3), o método não é aplicável. Por exemplo:

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

Se uma expressão de argumento único corresponde a um parâmetro paramarray e o tipo da expressão do argumento é conversível para o tipo do parâmetro paramarray e o tipo de elemento paramarray, o método é aplicável em ambas as suas expandidas e não expandidas formas, com duas exceções. Se a conversão do tipo da expressão do argumento para o tipo de paramarray é restritiva, em seguida, o método só é aplicável em sua forma expandida. Se a expressão de argumento é o literal `Nothing`, em seguida, o método só é aplicável em sua forma não expandida. Por exemplo:

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

O exemplo acima produz a saída a seguir:

```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

Nas primeiros e últimas invocações do `F`, a forma normal de `F` é aplicável porque uma conversão de ampliação existe do tipo de argumento para o tipo de parâmetro (ambos são do tipo `Object()`), e o argumento é passado como um valor comum parâmetro. Em que as invocações de segunda e terceira, a forma normal de `F` não é aplicável porque nenhuma conversão de ampliação existe do tipo de argumento para o tipo de parâmetro (conversões de `Object` para `Object()` são reduzidas). No entanto, a forma expandida do `F` é aplicável e um elemento de um `Object()` é criado pela invocação. O único elemento da matriz é inicializado com o valor do argumento fornecido (que por si só é uma referência a um `Object()`).

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a>Passando argumentos e argumentos para parâmetros opcionais de separação

Se um parâmetro é um parâmetro de valor, a expressão do argumento correspondente deve ser classificada como um valor. O valor é convertido para o tipo do parâmetro e passado como o parâmetro em tempo de execução. Se o parâmetro é um parâmetro de referência e a expressão do argumento correspondente é classificada como uma variável cujo tipo é o mesmo que o parâmetro, em seguida, uma referência à variável é passada como o parâmetro em tempo de execução.

Caso contrário, se a expressão do argumento correspondente é classificada como uma variável, o valor ou o acesso de propriedade, uma variável temporária do tipo do parâmetro é alocada. Antes da chamada do método em tempo de execução, a expressão de argumento é reclassificada como um valor, convertida para o tipo do parâmetro e atribuída à variável temporária. Em seguida, uma referência à variável temporária é passada como o parâmetro. Depois que a chamada do método é avaliada, se a expressão de argumento é classificada como um acesso de variável ou propriedade, variável temporária é atribuído para a expressão de variável ou expressão de acesso de propriedade. Se a expressão de acesso de propriedade não tiver nenhum `Set` acessador e, em seguida, a atribuição não é executada.

Para parâmetros opcionais em que um argumento não foi fornecido, o compilador escolhe argumentos, conforme descrito abaixo. Em todos os casos ele testa o tipo de parâmetro após a substituição de tipo genérico.

* Se o parâmetro opcional tem o atributo `System.Runtime.CompilerServices.CallerLineNumber`e a invocação é de um local no código-fonte e um literal numérico que representa o número da linha do local tem uma conversão intrínseco para o tipo de parâmetro, então o literal numérico é usado. Se a invocação abrange várias linhas, a opção de linha a ser usada é dependente da implementação.

* Se o parâmetro opcional tem o atributo `System.Runtime.CompilerServices.CallerFilePath`e a invocação é de um local no código-fonte e um literal de cadeia de caracteres que representa o caminho do arquivo do local tem uma conversão intrínseco para o tipo de parâmetro, o literal de cadeia de caracteres é usado. O formato do caminho do arquivo é dependente da implementação.

* Se o parâmetro opcional tem o atributo `System.Runtime.CompilerServices.CallerMemberName`e a invocação é dentro do corpo de um membro de tipo ou em um atributo aplicado a qualquer parte desse membro de tipo e um literal de cadeia de caracteres que representa o nome do membro que tem uma conversão intrínseco para o parâmetro Digite, em seguida, o literal de cadeia de caracteres é usado. Para invocações que fazem parte de acessadores de propriedade ou manipuladores de eventos personalizados, em seguida, o nome do membro usado é da propriedade ou evento em si. Para invocações que fazem parte de um operador ou construtor, em seguida, é usado um nome específico da implementação.

Se nenhum dos acima para aplicar, em seguida, valor padrão do parâmetro opcional é usado (ou `Nothing` se nenhum valor padrão é fornecido). E se mais de um dos acima para aplicar, em seguida, a escolha de qual deles usar depende da implementação.

O `CallerLineNumber` e `CallerFilePath` atributos são úteis para registro em log. O `CallerMemberName` é útil para implementar `INotifyPropertyChanged`. Estes são exemplos.

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

Além dos parâmetros opcionais acima, o Microsoft Visual Basic também reconhece alguns parâmetros opcionais adicionais se eles são importados dos metadados (ou seja, de uma referência DLL):

* Após a importação dos metadados, o Visual Basic também trata o parâmetro `<Optional>` como uma indicação que o parâmetro é opcional: dessa forma, é possível importar uma declaração que tem um parâmetro opcional, mas nenhum valor padrão, mesmo que isso não pode ser expressa usando o `Optional` palavra-chave.

* Se o parâmetro opcional tem o atributo `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`e o valor numérico literal 1 ou 0 tem uma conversão para o tipo de parâmetro e, em seguida, o compilador usa como argumento o literal 1 se `Option Compare Text` está em vigor ou os 0 se o literal `Optional Compare Binary` está em vigor.

* Se o parâmetro opcional tem o atributo `System.Runtime.CompilerServices.IDispatchConstantAttribute`, e ele tem o tipo `Object`e ele não especifica um valor padrão e, em seguida, o compilador usa o argumento `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.

* Se o parâmetro opcional tem o atributo `System.Runtime.CompilerServices.IUnknownConstantAttribute`, e ele tem o tipo `Object`e ele não especifica um valor padrão e, em seguida, o compilador usa o argumento `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.

* Se o parâmetro opcional tem um tipo `Object`e ele não especifica um valor padrão e, em seguida, o compilador usa o argumento `System.Reflection.Missing.Value`.

### <a name="conditional-methods"></a>Métodos condicionais

Se o método de destino ao qual se refere a uma expressão de invocação é uma sub-rotina que não é um membro de uma interface e se o método tem um ou mais `System.Diagnostics.ConditionalAttribute` atributos, avaliação da expressão depende de constantes de compilação condicional definidas no Esse ponto no arquivo de origem. Cada instância do atributo que especifica uma cadeia de caracteres que nomeia uma constante de compilação condicional. Cada constante de compilação condicional é avaliada como se fosse parte de uma instrução de compilação condicional. Se a constante é avaliada como `True`, a expressão é avaliada normalmente em tempo de execução. Se a constante é avaliada como `False`, a expressão não será avaliada.

Quando você está procurando o atributo, a declaração mais derivada de um método substituível é verificada.

__Observação.__ O atributo não é válido em funções ou métodos de interface e será ignorado se especificado em um desses tipos de método. Assim, os métodos condicionais só serão exibidas nas instruções de invocação.

### <a name="type-argument-inference"></a>Inferência de argumento de tipo

Quando um método com parâmetros de tipo é chamado sem especificar argumentos de tipo *inferência de argumento* é usado para tentar inferir argumentos de tipo para a chamada. Isso permite uma sintaxe mais natural a ser usado para chamar um método com parâmetros de tipo quando os argumentos de tipo podem ser inferidos superficialmente. Por exemplo, dada a seguinte declaração de método:

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

é possível invocar o `Choose` método sem especificar explicitamente um argumento de tipo:

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

Por meio de inferência de argumento de tipo, os argumentos de tipo `Integer` e `String` são determinados dos argumentos para o método.

Ocorre a inferência de argumento de tipo *antes de* reclassificação da expressão é executada em métodos de lambda ou ponteiros de método na lista de argumentos, uma vez que a reclassificação desses dois tipos de expressões pode exigir o tipo dos parâmetro a ser conhecido.  Dado um conjunto de argumentos `A1,...,An`, um conjunto de parâmetros de correspondência `P1,...,Pn` e um conjunto de método parâmetros de tipo `T1,...,Tn`, as dependências entre os argumentos e os parâmetros de tipo de método são coletadas primeiro da seguinte maneira:

* Se `An` é o `Nothing` literal, sem dependências são geradas.

* Se `An` é um método lambda e o tipo de `Pn` é um tipo delegado construído ou `System.Linq.Expressions.Expression(Of T)`, onde `T` é um tipo de delegado construído,

* Se o tipo de um parâmetro de método lambda será inferido do tipo do parâmetro correspondente `Pn`, e o tipo do parâmetro depende de um parâmetro de tipo do método `Tn`, em seguida, `An` tem uma dependência no `Tn`.

* Se o tipo de um parâmetro de método lambda é especificado e o tipo do parâmetro correspondente `Pn` depende de um parâmetro de tipo do método `Tn`, em seguida, `Tn` tem uma dependência no `An`.

* Se o tipo de retorno de `Pn` depende de um parâmetro de tipo do método `Tn`, em seguida, `Tn` tem uma dependência no `An`.

* Se `An` é um ponteiro de método e o tipo de `Pn` é um tipo de delegado construído,

* Se o tipo de retorno de `Pn` depende de um parâmetro de tipo do método `Tn`, em seguida, `Tn` tem uma dependência no `An`.

* Se `Pn` é um tipo construído e o tipo de `Pn` depende de um parâmetro de tipo de método `Tn`, em seguida, `Tn` tem uma dependência no `An`.

* Caso contrário, nenhuma dependência é gerada.

Depois de coletar dependências, os argumentos que não têm dependências são eliminados. Se quaisquer parâmetros de tipo de método não tem nenhuma dependência de saída (ou seja, o parâmetro de tipo de método não depende de um argumento), em seguida, inferência de tipo falhará. Caso contrário, os argumentos restantes e os parâmetros de tipo de método são agrupados em componentes fortemente conectados. Um componente fortemente conectado é um conjunto de argumentos e parâmetros de tipo de método, em que qualquer elemento no componente está acessível por meio de dependências em outros elementos.

Os componentes fortemente conectados são, em seguida, topologicamente classificados e processados em ordem topológica:

* Se o componente com rigidez de tipos contiver apenas um elemento,

  * Se o elemento já foi marcado completo, ignorá-la.

  * Se o elemento é um argumento, adicione dicas de tipo do argumento para os parâmetros de tipo de método que dependem dele e marcam o elemento como concluído. Se o argumento for um método lambda com parâmetros que ainda precisam inferido tipos, inferir `Object` para os tipos desses parâmetros.

  * Se o elemento é um parâmetro de tipo de método, em seguida, inferir o parâmetro de tipo de método para ser o tipo dominante entre as dicas de tipo de argumento e marcar o elemento como concluído. Se uma dica de tipo tiver uma restrição de elemento de matriz, apenas as conversões que são válidas entre matrizes do tipo especificado são consideradas (conversões de matriz ou seja, covariante e intrínseco). Se uma dica de tipo tiver uma restrição de argumento genérico, somente conversões de identidade são consideradas. Se nenhum tipo dominante pode ser escolhido, a inferência de tipos falha. Se quaisquer tipos de argumento do método lambda dependem este parâmetro de tipo de método, o tipo é propagado para o método lambda.

* Se o componente com rigidez de tipos contiver mais de um elemento, o componente contém um ciclo.

  * Para cada parâmetro de tipo de método que é um elemento no componente, se o parâmetro de tipo de método depende de um argumento que não é marcada como concluído, converter essa dependência em uma declaração que será verificada no final do processo de inferência de tipos.

  * Reinicie o processo de inferência de tipos no ponto em que os componentes com rigidez de tipos foram determinados.

Se a inferência de tipo for bem-sucedida para todos os parâmetros de tipo de método, todas as dependências que foram alteradas em asserções são verificadas. Uma asserção terá êxito se o tipo do argumento é implicitamente conversível para o tipo inferido do parâmetro de tipo de método. Se uma asserção falha, inferência de argumento de tipo falhará.

Dado um tipo de argumento `Ta` para um argumento `A` e um tipo de parâmetro `Tp` para um parâmetro `P`, dicas de tipo são geradas da seguinte maneira:

* Se `Tp` não envolve quaisquer parâmetros de tipo de método e nenhuma dica é gerada.

* Se `Tp` e `Ta` são tipos de matriz da mesma classificação e, em seguida, substitua `Ta` e `Tp` com os tipos de elemento de `Ta` e `Tp` e reiniciar o processo com uma restrição de elemento de matriz.

* Se `Tp` é um parâmetro de tipo de método, em seguida, `Ta` é adicionado como uma dica de tipo com a restrição atual, se houver.

* Se `A` é um método lambda e `Tp` é um tipo delegado construído ou `System.Linq.Expressions.Expression(Of T)`, onde `T` é um tipo de delegado construído, para cada tipo de parâmetro do método lambda `TL` e correspondente do tipo de parâmetro delegado`TD`, substitua `Ta` com `TL` e `Tp` com `TD` e reinicie o processo sem restrição. Em seguida, substitua `Ta` com o tipo de retorno do método lambda e:

  * Se `A` é um método lambda regular, substitua `Tp` com o tipo de retorno do tipo de delegado;
  * Se `A` é um método de lambda assíncrono e o tipo de retorno do tipo de delegado tem o formato `Task(Of T)` para alguns `T`, substitua `Tp` com que `T`;
  * Se `A` é um método de lambda de iterador e o tipo de retorno do tipo de delegado tem o formato `IEnumerator(Of T)` ou `IEnumerable(Of T)` para alguns `T`, substitua `Tp` com que `T`.
  * Em seguida, reinicie o processo sem restrição.

* Se `A` é um ponteiro de método e `Tp` é um construído tipo delegate, use os tipos de parâmetro de `Tp` determinar qual método apontado é mais aplicável às `Tp`. Se houver um método que é mais aplicável, substitua `Ta` com o tipo de retorno do método e `Tp` com o tipo de retorno do tipo de delegado e reiniciar o processo sem restrição.

* Caso contrário, `Tp` deve ser um tipo construído. Considerando `TG`, o tipo genérico de `Tp`,

  * Se `Ta` é `TG`, herda `TG`, ou implementa o tipo `TG` exatamente uma vez, em seguida, para cada argumento de tipo correspondente `Tax` do `Ta` e `Tpx` de `Tp`, substitua `Ta` com `Tax` e `Tp` com `Tpx` e reinicie o processo com uma restrição de argumento genérico.

  * Caso contrário, a inferência de tipo falhará para o método genérico.

O sucesso da inferência de tipo, em e de si mesmo, garante que o método é aplicável.
