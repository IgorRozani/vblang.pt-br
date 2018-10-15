# <a name="introduction"></a>Introdução

O Microsoft&reg; Visual Basic&reg; linguagem de programação é uma linguagem de programação de alto nível para o Microsoft .NET Framework. Embora tenha sido criado para ser uma linguagem acessível e fácil de aprender, também é poderoso o suficiente para atender às necessidades dos programadores experientes. A linguagem de programação Visual Basic tem uma sintaxe semelhante para o inglês, que promove a clareza e legibilidade do código do Visual Basic. Sempre que possível, significativas palavras ou frases são usadas em vez de abreviações, acrônimos ou caracteres especiais. Sintaxe de estranhos ou desnecessário é geralmente permitido, mas não obrigatório.

A linguagem de programação Visual Basic pode ser fortemente tipados ou uma linguagem fracamente tipada. Tipagem flexível adia grande parte da carga de tipo de verificação até que um programa já está em execução. Isso inclui não apenas verificação de tipo de conversões, mas também das chamadas de método, que significa que a associação de uma chamada de método pode ser adiada até o tempo de execução. Isso é útil ao criar protótipos ou outros programas na qual a velocidade de desenvolvimento é mais importante do que a velocidade de execução. O Visual Basic, a linguagem de programação também fornece fortemente tipados semântica que executa todas as verificação de tipo em tempo de compilação e não permite associação de tempo de execução de chamadas de método. Isso garante o desempenho máximo e ajuda a garantir que as conversões de tipo estão corretas. Isso é útil ao criar aplicativos de produção no qual velocidade de execução e execução de correção é importante.

Este documento descreve a linguagem Visual Basic. Ele é projetado para ser uma descrição completa da linguagem em vez de um tutorial da linguagem ou manual de referência do usuário.

## <a name="grammar-notation"></a>Notação de gramática

Esta especificação descreve duas gramáticas: uma gramática lexical e uma gramática sintática. A gramática lexical define como os caracteres podem ser combinados para tokens de formulário; a gramática sintática define como os tokens podem ser combinados para programas do Visual Basic de formulário. Também há várias gramáticas secundárias usadas para operações como compilação condicional de pré-processamento.

As gramáticas nesta especificação são gravadas no formato ANTLR – consulte http://www.antlr.org/.

Caso não é importante nos programas em Visual Basic. Para manter a simplicidade, todos os terminais serão dado em maiusculas e minúsculas padrão, mas qualquer uso de maiusculas e minúsculas corresponderá a eles. Os terminais são imprimíveis elementos do conjunto de caracteres ASCII são representados por seus caracteres ASCII correspondentes. Visual Basic também é largura kana ao fazer a correspondência de terminais, permitindo que os caracteres Unicode de largura inteira coincidir com seus equivalentes Unicode de meia largura, mas apenas em uma base de token de todo. Um token não corresponderão se ele contiver caracteres mistos de meia largura e de largura inteira.

Quebras de linha e recuo podem ser adicionadas para facilitar a leitura e não fazem parte da produção.

## <a name="compatibility"></a>Compatibilidade

Um recurso importante de uma linguagem de programação é a compatibilidade entre versões diferentes do idioma. Se uma versão mais recente de uma linguagem não aceita o mesmo código que uma versão anterior da linguagem ou interpreta isso de forma diferente do que a versão anterior, em seguida, uma carga pode ser colocada em um programador ao atualizar o seu código de uma versão do idioma para outro. Como tal, compatibilidade entre versões deve ser preservada, exceto quando o benefício para os consumidores de idioma é de natureza clara e esmagadora.

A seguinte política controla alterações para a linguagem Visual Basic entre versões. O idioma do termo, quando usada nesse contexto, refere-se apenas os aspectos de sintáticos e semânticos da linguagem Visual Basic em si e não inclui quaisquer classes do .NET Framework incluídas como parte do `Microsoft.VisualBasic` namespace (e subnamespaces). Todas as classes no .NET Framework são cobertas por uma política de controle de versão e compatibilidade separada fora do escopo deste documento.

### <a name="kinds-of-compatibility-breaks"></a>Tipos de interrupções de compatibilidade

Em um mundo ideal, a compatibilidade seria 100% entre a versão existente do Visual Basic e todas as versões futuras do Visual Basic. No entanto, pode haver situações em que a necessidade de uma quebra de compatibilidade pode superar o custo ele pode impor de programadores. Essas situações são:

* *Novos avisos.* Apresentando um novo aviso não é, por si só, uma quebra de compatibilidade. No entanto, porque muitos desenvolvedores compilar com "tratar avisos como erros" ativados, extra deve ter cuidado ao introduzir avisos.

* *Novas palavras-chave.* Talvez seja necessário introduzir novas palavras-chave ao introduzir novos recursos de linguagem. Os esforços razoáveis serão feitos para escolher palavras-chave que minimizar a possibilidade de colisão com identificadores de usuários e usar as palavras-chave existentes em que faz sentido. A Ajuda será fornecida para atualizar projetos de versões anteriores e quaisquer novas palavras-chave de escape.

* *Bugs de compilador.* Quando o comportamento do compilador é conflitante com um comportamento documentada na especificação da linguagem, talvez seja necessário corrigir o comportamento do compilador para corresponder ao comportamento documentado.

* *Bug de especificação.* Quando o compilador é consistente com a especificação da linguagem, a especificação da linguagem, mas é claramente errado, alterando a especificação da linguagem e o comportamento do compilador talvez seja necessário. A frase "claramente incorreta" significa que o comportamento documentado contador é executado para a maioria clara e inequívoca dos usuários que esperava e gera um comportamento muito ruim para os usuários.

* *Ambiguidade da especificação.* Quando a especificação da linguagem deve esclarecer o que acontece em uma situação específica, mas não, e o compilador trata a situação de maneira inconsistente ou claramente incorreto (usando a mesma definição do ponto anterior), esclarecimento do especificação e corrigindo o comportamento do compilador podem ser necessários. Em outras palavras, quando a especificação aborda casos a, b, d e e, mas omite qualquer menção do que acontece no caso c e o compilador se comportar incorretamente no caso de c, talvez seja necessário documentar o que acontece no caso c e alterar o comportamento do compilador para corresponder. (Observe que, se a especificação foi ambígua sobre o que acontece em uma situação e o compilador se comporta de forma que não está claramente incorreta, que o comportamento do compilador torna-se a especificação de fato.)

* *Tornando os erros de tempo de execução em erros de tempo de compilação.* Em uma situação em que o código é 100% garantido para falhar em tempo de execução (ou seja, o código do usuário tem um bug não ambíguo nele), ele pode ser desejável para adicionar um erro de tempo de compilação que captura a situação.

* *Omissão da especificação.* Quando a especificação da linguagem não especificamente permitir ou impedir que uma determinada situação e o compilador trata a situação de uma maneira que é indesejável (se o comportamento do compilador foi claramente errado, ele seria um bug de especificação, não uma especificação a omissão), pode ser necessário esclarecer a especificação e alterar o comportamento do compilador. Além de análise de impacto usual, as alterações desse tipo são mais restrito para casos em que o impacto da alteração é considerado extremamente mínimo e o benefício para os desenvolvedores é muito alto.

* *Novos recursos.* Em geral, a introdução de novos recursos não deve alterar partes existentes da especificação da linguagem ou o comportamento existente do compilador. Em situações onde apresentando um novo recurso exige a alteração a especificação da linguagem existente, tal uma quebra de compatibilidade é razoável somente se o impacto seria extremamente mínimo e o benefício do recurso é alta.

* *Segurança.* Em situações extraordinários, questões de segurança podem necessitar de uma quebra de compatibilidade, como remover ou modificar um recurso que é inerentemente inseguro e representa um risco de segurança claro para os usuários.

As situações a seguir não são aceitáveis motivos para introduzir as quebras de compatibilidade:

* *Comportamento encaramos ou indesejável.* Comportamento de design ou o compilador de linguagem que é razoável, mas considerados indesejáveis ou encaramos em retrospectiva não é uma justificativa para interromper a compatibilidade com versões anteriores. O processo de substituição de linguagem, abordado a seguir, deve ser usado em vez disso.

* *Mais alguma coisa.* Caso contrário, o comportamento do compilador permanece compatível com versões anteriores.

### <a name="impact-criteria"></a>Critérios de impacto

Ao considerar se uma quebra de compatibilidade pode ser aceitável, vários critérios são usados para determinar o que é o impacto da alteração. Quanto maior o impacto, quanto maior a barra para aceitar a compatibilidade de quebra.

Os critérios são:

* O que é o escopo da alteração? Em outras palavras, como muitos programas têm probabilidade de que serão afetadas? Quantos usuários estão provavelmente serão afetados? Quão comum será escrever código que é afetado pela alteração?

* Nenhuma solução alternativa existe para obter o mesmo comportamento antes da alteração?

* Como óbvia é a alteração? Os usuários receberão comentários imediatos que algo foi alterado ou seus programas apenas executará diferente?

* Pode a alteração ser razoavelmente resolvida durante a atualização? É possível escrever uma ferramenta que pode encontrar a situação em que a alteração ocorre com precisão perfeito e altere o código para contornar a alteração?

* O que é o comentários da comunidade sobre a alteração?

### <a name="language-deprecation"></a>Substituição de idioma

Ao longo do tempo, partes da linguagem ou o compilador podem se tornar preteridos. Conforme discutido anteriormente, não é aceitável para interromper a compatibilidade para remover tais recursos preteridos. Em vez disso, as etapas a seguir devem ser seguidas:

1. Dado um recurso que existe na versão *um* do Visual Studio, comentários devem ser solicitados a substituição do recurso e aviso completo fornecido antes de qualquer substituição final decisão é tomada da comunidade de usuário. O processo de substituição pode ser revertido ou abandonado a qualquer momento com base nos comentários da comunidade de usuário.

2. a versão completa (ou seja, não uma versão de ponto) *B* do Visual Studio deve ser liberado com avisos do compilador avisando sobre uso preterido. Os avisos devem estar ativado por padrão e podem ser desativados. As reprovações devem ser claramente documentadas na documentação do produto e na web.

3. Uma versão completa *C* do Visual Studio deve ser liberado com avisos do compilador não podem ser desativados.

4. Uma versão completa *1!d* do Visual Studio, subsequentemente, devem ser liberados com os avisos de compilador preteridas convertidos em erros de compilador. A versão do *1!d* deve ocorrer após o término da fase de suporte base (5 anos a partir da redação deste artigo) da versão *um*.

5. Por fim, uma versão *eletrônico* do Visual Studio pode ser liberado que remove os erros do compilador.

Alterações que não podem ser tratadas dentro dessa estrutura de substituição não serão permitidas.
