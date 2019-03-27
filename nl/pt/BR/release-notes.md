---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Notas sobre a liberação
{: #release-notes}

## Versão da API de Serviço
{: #release-notes-api-version}

As solicitações de API requerem um parâmetro version que use uma data no formato `version=YYYY-MM-DD`. Sempre que mudamos a API de uma maneira incompatível com versões anteriores, lançamos uma nova versão secundária da API.

Envie o parâmetro version com cada solicitação de API. O serviço usa a versão da API para a data que você especificar ou a versão mais recente antes dessa data. Não padronize com a data atual. Em vez disso, especifique uma data que corresponda a uma versão que seja compatível com seu aplicativo e não a mude até que o aplicativo esteja pronto para uma versão mais recente.

- A versão atual para V1 é `2019-02-28`.
- A versão atual para V2 é `2019-02-28`.
- A área de janela "Experimente" no conjunto de ferramentas do {{site.data.keyword.conversationshort}} está usando a versão `2018-07-10`.

## Recursos beta
{: #release-notes-beta}

A IBM libera os serviços, recursos e suporte ao idioma para sua avaliação que são classificados como beta. Esses recursos podem ficar instáveis, podem mudar frequentemente e podem ser descontinuados com breve aviso. Os recursos beta podem também não fornecer o mesmo nível de desempenho ou compatibilidade que os recursos geralmente disponíveis fornecem e não são destinados ao uso em um ambiente de produção. Os recursos Beta são suportados somente no [IBM Developer Answers ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window}.

## Modelos atualizados
{: #release-notes-updated-models}

Os algoritmos do {{site.data.keyword.conversationshort}} podem ser refinados e atualizados periodicamente com base no feedback, em aprimoramentos científicos e em fatores adicionais, a fim de aprimorar continuamente o desempenho. Atualizações para o modelo serão comunicadas nessas notas sobre a liberação.

Os modelos existentes que você treinou não serão afetados imediatamente, mas os modelos expirados serão atualizados para o modelo atual, se você ainda não tiver feito isso, após 60 dias de um novo modelo se tornar disponível.

**Nota:** esta instrução de atualização se aplica apenas aos idiomas e recursos Disponíveis de Modo Geral (GA).

Os novos recursos e mudanças a seguir para o serviço estão disponíveis. Verifique nosso [blog ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://medium.com/ibm-watson/assistant/home) para localizar informações detalhadas sobre como os recursos mais recentes podem beneficiar seus negócios.

## 1 de março de 2019
{: #1March2019}

- **Navegação simplificada**: a navegação da barra lateral com as guias *Construir*, *Melhorar* e *Implementar* separadas foi removida. Agora, é possível obter todas as ferramentas necessárias para construir uma qualificação de diálogo por meio da página de qualificação principal.

- **A página Melhorar agora é chamada Analítica**: as métricas informativas que o serviço gera de conversas entre seus usuários e seu assistente foram movidas da guia *Melhorar* da barra lateral para uma nova guia na página de qualificação principal chamada **Analítica**.

## 28 de fevereiro de 2019
{: #28February2019}

- **Nova versão da API**: a versão da API atual é agora `2019-02-28`. As mudanças a seguir foram feitas com essa versão:

    - A ordem na qual as condições são avaliadas em nós com intervalos foi mudada. Anteriormente, se você tivesse um nó com intervalos que permitissem digressões fora, o nó raiz `anything_else` era acionado antes que qualquer uma das condições Não localizado no nível de intervalo pudesse ser avaliada. A ordem das operações mudou para direcionar esse comportamento. Agora, quando um usuário digressiona de um nó com intervalos, todos os nós raiz, exceto o nó `anything_else`, são processados. Em seguida, as condições Não localizado de nível de intervalo são avaliadas. E, finalmente, o nó de nível raiz `anything_else` é processado. Para entender melhor a ordem integral de operações para um nó com intervalos, veja [Dicas de uso de intervalo](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-node-level-handler).

    - As sequências que iniciam com um sinal de número (#) nos objetos `context` ou `output` de uma mensagem não são mais tratadas como referências de intenção.
  
      Anteriormente, essas sequências eram tratadas como intenções automaticamente. Por exemplo, se você especificasse uma variável de contexto, como `"color":"#FFFFFF"`, o código de cor hexadecimal (#FFFFFF) seria tratado como uma intenção. O serviço verificaria se uma intenção denominada #FFFFFF foi detectada na entrada do usuário e, se não, substituiria #FFFFFF por `false`. Esta substituição não ocorre mais.
  
      Da mesma forma, se incluísse um sinal de número (#) na sequência de texto em uma resposta do nó, você precisaria escapá-lo precedendo-o com uma barra invertida (`\`). Por exemplo, `We are the \#1 seller of lobster rolls in Maine.` Você não precisa mais escapar o símbolo `#` em uma resposta de texto.

      Essa mudança não se aplica a condições de resposta de nó ou condicional. Quaisquer sequências que iniciam com um sinal de número (#) que são especificadas em condições continuam a ser tratadas como referências de intenção. Além disso, é possível usar a sintaxe de expressão SpEL para forçar o sistema a tratar uma sequência nos objetos `context` ou `output` de uma mensagem como uma intenção. Por exemplo, especifique a intenção como  `<? #intent-name ?>`.

## 25 de fevereiro de 2019
{: #25February2019}

**Aprimoramento de integração do Slack**: agora é possível escolher o tipo de evento que aciona seu assistente em um canal Slack. Anteriormente, quando você integrava seu assistente ao Slack, o assistente interagia com os usuários por meio de um canal de mensagens diretas. Agora, é possível configurar o assistente para atender a menções e responder quando ele é mencionado em outros canais. É possível escolher usar um ou ambos os tipos de eventos como o mecanismo por meio do qual seu assistente interage com os usuários.

## 11 de fevereiro de 2019
{: #11February2019}

**Integrar à Intercom**: a Intercom, uma plataforma de sistema de mensagens de atendimento ao cliente líder, tem parceria com a IBM para incluir um novo agente na equipe, um Watson Assistant virtual. É possível integrar seu assistente a um aplicativo Intercom para permitir que o app passe, sem interrupções, em conversas do usuário entre seu assistente e os agentes de suporte humano. Essa integração está disponível somente para os usuários do plano Plus e Premium. Consulte [Integrando à Intercom](/docs/services/assistant?topic=assistant-deploy-intercom) para obter mais detalhes.

## 8 de fevereiro de 2019
{: #8February2019}

- **Versão de suas qualificações**: agora é possível fazer uma captura instantânea de intenções, entidades, diálogo e definições de configuração para uma qualificação em pontos-chave durante o processo de desenvolvimento. Com versões, é seguro ser criativo. É possível implementar novas abordagens de design em um ambiente de teste para validá-las antes de aplicar quaisquer atualizações a uma implementação de produção de seu assistente. Consulte [Criando versões de qualificação](/docs/services/assistant?topic=assistant-versions) para obter mais detalhes.

- **Catálogo de conteúdo em árabe**: os usuários de qualificações no idioma árabe podem agora incluir intenções pré-construídas em seus diálogos. Consulte [Usando catálogos de conteúdo](/docs/services/assistant?topic=assistant-catalog) para obter mais informações.

## 17 de janeiro de 2019
{: #17January2019}

- **O suporte ao idioma tcheco está geralmente disponível**: o suporte para o idioma tcheco não é mais classificado como beta; ele está agora geralmente disponível. Consulte  [ Idiomas Suportados ](/docs/services/assistant?topic=assistant-language-support)  para obter mais informações.

- **Melhorias de suporte ao idioma**: os componentes de entendimento de idioma foram atualizados para melhorar os recursos a seguir:

  - Entidades do sistema alemão e coreano
  - Tokenização de classificação de intenção para árabe, holandês, francês, italiano, japonês, português e espanhol

## 4 de janeiro de 2019
{: #4January2019}

- **IBM Cloud Functions em locais de DC e de Londres**: agora é possível fazer chamadas programáticas para o IBM Cloud Functions por meio do diálogo de um assistente em uma instância de serviço que está hospedada nos data centers de Londres e Washington, DC. Consulte [Fazendo chamadas programáticas por meio de um nó de diálogo](/docs/services/assistant?topic=assistant-dialog-actions).

- **Novos métodos para trabalhar com matrizes**: os métodos de expressão SpEL a seguir estão disponíveis, que tornam mais fácil trabalhar com valores de matriz em seu diálogo:

  - **JSONArray.filter**: filtra uma matriz comparando cada valor na matriz com um valor que pode variar com base na entrada do usuário.
  - **JSONArray.includesIntent**: verifica se uma matriz `intents` contém uma intenção específica.
  - **JSONArray.indexOf**: obtém o número do índice de um valor específico em uma matriz.
  - **JSONArray.joinToArray**: aplica a formatação aos valores que são retornados de uma matriz.

   Consulte a [documentação do método de matriz](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-arrays) para obter mais detalhes.

## 13 de dezembro de 2018
{: #13December2018}

- **Data center de Londres**: agora é possível criar instâncias de serviço do {{site.data.keyword.conversationshort}} que estão hospedadas no data center de Londres sem organização. Consulte  [ Data centers ](/docs/services/assistant?topic=assistant-services-information#services-information-regions)  para obter mais detalhes.

- **Mudanças de limite do nó de diálogo**: o limite do nó de diálogo foi mudado temporariamente de 100.000 para 500 para novas instâncias de plano Padrão. Esta mudança de limite foi posteriormente revertida. Se você criou uma instância do plano Padrão durante o prazo no qual o limite estava em vigor, seus diálogos poderão ser afetados. O limite ficou em vigor para as qualificações criadas entre 10 de dezembro e 12 de dezembro de 2018. Os limites inferiores serão removidos de todas as instâncias afetadas em janeiro. Se for necessário elevar o limite inferior antes disso, abra um chamado de suporte.

## 1 de dezembro de 2018
{: #1December2018}

   Para determinar o número de nós de diálogo em uma qualificação de diálogo, execute um dos procedimentos a seguir:

   - Na ferramenta, se ela ainda não estiver associada a um assistente, inclua a qualificação de diálogo em um assistente e, em seguida, visualize o quadro de qualificações na página principal do assistente. A seção *dados treinados* lista o número de nós de diálogo.

   - Envie uma solicitação GET para o terminal de API /dialog_nodes e inclua o parâmetro `include_count=true`. Por exemplo:

     ```curl
     curl -u "apikey: {1}/workspaces/ {2}{0}{1}{8}-09-20 & include_count & include_count = true"
     ```

     Na resposta, o atributo `total` no objeto `pagination` contém o número de nós de diálogo.

     Consulte [Resolução de problemas de importação de qualificação](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors) para obter informações sobre como editar qualificações que você deseja continuar usando.

## 27 de novembro de 2018
{: #27November2018}

- **Um novo plano de serviço, o plano Plus, está disponível**: o novo plano oferece recursos de nível premium em um ponto de preço inferior. Ao contrário dos planos anteriores, o plano Plus é um plano de faturamento baseado em usuário. Ele mede o uso pelo número de usuários exclusivos que interagem com seu assistente durante um determinado período. Para obter o máximo do plano, se você construir seu próprio aplicativo cliente, projete seu app de forma que ele defina um ID exclusivo para cada usuário e passe o ID do usuário com cada chamada da API /message. Para as integrações integradas, o ID de sessão é usado para identificar interações do usuário com o assistente. Consulte [Planos baseados em usuário](/docs/services/assistant?topic=assistant-services-information#services-information-user-based-plans) para obter mais informações.

  <table>
  <caption>Mais limites de plano</caption>
    <tr>
      <th>Artefato</th>
      <th>Limit</th>
    </tr>
    <tr>
      <td>Assistentes</td>
      <td>100</td>
    </tr>
    <tr>
       <td>Entidades Contextuais</td>
       <td>20</td>
    </tr>
    <tr>
       <td>Anotações de Entidade Contextual</td>
       <td>2.000</td>
    </tr>
    <tr>
       <td>Nós de diálogo</td>
       <td>100.000</td>
    </tr>
    <tr>
       <td>Entidades</td>
       <td>1.000</td>
    </tr>
    <tr>
       <td>Sinônimos de entidade</td>
       <td>100.000</td>
    </tr>
    <tr>
       <td>Valores de entidade</td>
       <td>100.000</td>
    </tr>
    <tr>
       <td>Intents</td>
       <td>2.000</td>
    </tr>
    <tr>
       <td>Exemplos de usuário de Intenção</td>
       <td>25.000</td>
    </tr>
    <tr>
       <td>Integrações</td>
       <td>100</td>
    </tr>
    <tr>
       <td>Logs</td>
       <td>30 dias</td>
    </tr>
    <tr>
       <td>Habilidades</td>
       <td>50</td>
    </tr>
  </table>

- **Plano Premium baseado em usuário**: o plano Premium agora baseia seu faturamento no número de usuários exclusivos ativos. Se escolher usar esse plano, projete quaisquer aplicativos customizados que você construir para identificar corretamente os usuários que geram chamadas da API /message. Consulte [Planos baseados em usuário](services-information#user-based-plans) para obter mais informações.

  As instâncias de serviço do plano Premium existentes não são afetadas por essa mudança; elas continuam a usar os métodos de faturamento baseados em API. Somente os usuários do plano Premium existentes verão o plano baseado em API listado como a opção de plano *Premium (API)*.
  {: note}

  Consulte [Opções de plano de serviço ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} do {{site.data.keyword.conversationshort}} para obter mais informações sobre todos os planos de serviços disponíveis.

- **Recomendações de exemplo do usuário de intenção ![Somente plano Plus ou Premium](images/premium.png)**: é possível fazer upload de um arquivo que contém entradas brutas do usuário, como consultas do usuário de um log da central de atendimento, que o serviço pode analisar e extrair para candidatos de exemplo do usuário de intenção. Consulte  [ Incluindo exemplos de arquivos de log ](intent-recommendations#intent-recommendations-get-example-recommendations).

## 20 de novembro de 2018
{: #20November2018}

**Recomendações estão descontinuadas**: a seção Recomendações na guia Melhorar foi removida. As Recomendações eram um recurso beta disponível somente para os usuários do plano Premium. Elas recomendavam ações que os usuários poderiam tomar para melhorar seus dados de treinamento. Em vez de consolidar as recomendações em um local, as recomendações estão agora sendo disponibilizadas por meio das partes da ferramenta em que você faz mudanças de dados de treinamento reais. Por exemplo, ao incluir sinônimos de entidade, é possível agora optar por ver uma lista de termos sinônimos que são recomendados pelo serviço. Se você estiver procurando outras maneiras de analisar os logs de conversa do usuário em mais detalhes, considere usar os blocos de notas do Jupyter. Consulte  [ Tarefas avançadas ](/docs/services/assistant?topic=assistant-logs-resources)  para obter mais detalhes.

## 9 de novembro de 2018
{: #9November2018}

- **Revisão da interface com o usuário principal**: o serviço {{site.data.keyword.conversationshort}} tem uma nova aparência e recursos incluídos.

  Essa versão da ferramenta foi avaliada por participantes do programa beta durante os últimos meses.

  - **Qualificações**: o que você acha de uma *área de trabalho* ser agora chamada de *qualificação*. Uma *qualificação de diálogo* é um contêiner para os dados de treinamento e artefatos do processamento de linguagem natural que permitem que o assistente entenda perguntas do usuário e as responda.

    **Onde estão minhas áreas de trabalho?** Quaisquer áreas de trabalho criadas anteriormente estão agora listadas em sua instância de serviço como qualificações. Clique na guia **Qualificações** para vê-las. Para obter mais informações, consulte [Qualificações](/docs/services/assistant?topic=assistant-skills).

  - **Assistentes**: agora é possível publicar sua qualificação em apenas duas etapas. Inclua sua qualificação em um assistente e, em seguida, configure uma ou mais integrações com as quais implementar sua qualificação. O assistente inclui uma camada de função na parte superior de sua qualificação que permite que o serviço orquestre e gerencie o fluxo de informações para você. Consulte  [ Assistentes ](/docs/services/assistant?topic=assistant-assistants).

  - **Integrações integradas**: em vez de acessar a guia **Implementar** para implementar sua área de trabalho, você inclui sua qualificação de diálogo em um assistente e inclui integrações no assistente por meio do qual a qualificação é disponibilizada para seus usuários. Não é necessário construir um aplicativo front-end customizado e gerenciar o estado de conversa a partir de uma chamada para a próxima. No entanto, ainda será possível fazer isso se você desejar. Consulte  [ Incluindo Integrações ](/docs/services/assistant?topic=assistant-deploy-integration-add)  para obter mais informações.

  - **Nova versão de API principal**: uma versão V2 da API está disponível. Essa versão fornece acesso a métodos que podem ser usados para interagir com um assistente no tempo de execução. Não há mais contexto de passagem com cada chamada da API; o estado de sessão é gerenciado para você como parte da camada do assistente.
  
    O que é apresentado no conjunto de ferramentas como uma qualificação de diálogo é efetivamente um wrapper para uma área de trabalho V1. Não há atualmente nenhum método de API para criar qualificações e assistentes com a API V2. No entanto, é possível continuar a usar a API V1 para criar áreas de trabalho. Consulte  [ Visão Geral da API ](/docs/services/assistant?topic=assistant-api-overview)  para obter mais detalhes.
    {: note}

  - **Alternando origens de dados**: agora é mais fácil melhorar o modelo em uma qualificação com os logs de conversa do usuário de uma qualificação diferente. Você não precisa depender dos IDs de implementação, mas pode simplesmente selecionar o nome do assistente para o qual uma qualificação foi incluída e implementada para usar seus dados. Consulte  [ Melhorando entre os assistentes ](/docs/services/assistant?topic=assistant-logs#logs-deploy-id).

  O vídeo a seguir fornece uma visão geral de 2 minutos da ferramenta {{site.data.keyword.conversationshort}} atualizada.

  <iframe class="embed-responsive-item" id="youtubeplayer" title="Visão geral do produto" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/OkW7gnHrw30?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

  - **Links de visualização de instâncias de Londres**: se a sua instância de serviço está hospedada em Londres, deve-se editar a URL do link de visualização. A URL inclui um código de região para a região na qual a instância está hospedada. Como as instâncias em Londres são organizadas em Dallas, deve-se substituir a referência `eu-gb` na URL por `us-south` para que a página da web de visualização seja renderizada adequadamente.

## 8 de novembro de 2018
{: #8November2018}

- **Data center em japonês**: agora é possível criar as instâncias de serviço do {{site.data.keyword.conversationshort}} que estão hospedadas no data center de Tóquio. Consulte  [ Data centers ](/docs/services/assistant?topic=assistant-services-information#services-information-regions)  para obter mais detalhes.

## 30 de outubro de 2018
{: #30October2018}

- **Novo processo de autenticação de API**: o serviço {{site.data.keyword.conversationshort}} executou a transição do uso do Cloud Foundry para o uso da autenticação do Identity and Access Management (IAM) baseada em token nas regiões a seguir:

  - Dallas (us-sul)
  - Frankfurt (eu-de)

  Para novas instâncias de serviço, você usa o IAM para autenticação. É possível passar um token de acesso ou uma chave de API. Tokens suportam solicitações autenticadas sem a integração de credenciais de serviço em cada chamada. As chaves API usam autenticação básica.

  Para todas as instâncias de serviço existentes, você continua a usar credenciais de serviço (`{username}:{password}`) para autenticação.

  Consulte [Autenticando chamadas da API](/docs/services/assistant?topic=assistant-services-information#services-information-authenticate-api-calls) para obter mais informações.

## 25 de outubro de 2018
{: #25October2018}

- **As recomendações de sinônimo de entidade estão disponíveis em mais idiomas**: o suporte de recomendação de sinônimo foi incluído para os idiomas francês, japonês e espanhol.

## 26 de setembro de 2018
{: #26September2018}

- O **{{site.data.keyword.conversationfull}} está disponível no {{site.data.keyword.icpfull}}**: consulte a [documentação do {{site.data.keyword.icpfull}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.3/featured_applications/watson_assistant.html) para obter mais informações.

## 21 de setembro de 2018
{: #21September2018}

- **Nova versão da API**: a versão da API atual é agora `2018-09-20`. Nessa versão, o atributo `errors[].path ` do objeto de erro que é retornado pela API é expresso como um [Ponteiro JSON ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://tools.ietf.org/html/rfc6901) em vez de em formato de notação de ponto.
- **Suporte a ações da web**: agora é possível chamar ações da web do {{site.data.keyword.openwhisk_short}} por meio de um nó de diálogo. Veja [Fazendo chamadas programáticas de um nó de diálogo](/docs/services/assistant?topic=assistant-dialog-actions) para obter mais detalhes.

## 15 de agosto de 2018
{: #15August2018}

- **Melhorias de suporte de correspondência difusa da entidade**: a correspondência difusa é totalmente suportada para as entidades em inglês e o recurso de erro de ortografia não é mais um recurso somente Beta para muitos outros idiomas. Veja [Linguagens suportadas](/docs/services/assistant?topic=assistant-language-support) para obter detalhes.

## 6 de agosto de 2018
{: #6August2018}

- **Resolução de conflito de intenção ![Somente plano Plus ou Premium](images/premium.png)**: a ferramenta pode agora ajudá-lo a resolver conflitos quando dois ou mais exemplos de usuários em intenções separadas são semelhantes entre si. Exemplos do usuário não distintos podem enfraquecer os dados de treinamento e tornar mais difícil para o serviço mapear a entrada do usuário para a intenção apropriada no tempo de execução. Consulte  [ Resolvendo conflitos de intenção ](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)  para obter detalhes.

- **Desambiguação** ![Somente plano Plus ou Premium](images/premium.png): ative a desambiguação para permitir que seu assistente solicite ao usuário ajuda quando ele precisar decidir entre dois ou mais nós de diálogo viáveis a serem processados para uma resposta. Consulte  [ Desambiguação ](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)  para obter mais detalhes.

- **Correção de salto**: corrigido um erro na ferramenta Dialogs que impedia que você configurasse um salto que direcionava a resposta de um nó com a condição especial `anything_else`.

- **Mensagem de retorno de digressão**: agora é possível especificar texto para ser exibido quando o usuário retorna a um nó após uma digressão. O usuário já terá visto o prompt para o nó. É possível mudar a mensagem um pouco para permitir que os usuários saibam que estão retornando para onde pararam. Por exemplo, especificar uma resposta como `Where were we? Oh, yes...`, consulte [Digressões](dialog-runtime#digressions) para obter mais detalhes.

## 12 de julho de 2018
{: #12July2018}

- **Tipos de resposta rica**: agora é possível incluir respostas ricas que incluem elementos como imagens ou botões, além do texto, em seu diálogo. Consulte  [ Respostas rich ](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)  para obter mais informações.

- **Entidades contextuais (beta)**: as entidades contextuais são entidades que você define rotulando menções do tipo de entidade que ocorrem em exemplos do usuário de intenção. Esses tipos de entidade ensinam ao serviço não somente os termos de interesse, mas também o contexto no qual os termos de interesse geralmente aparecem em elocuções do usuário, permitindo que o serviço reconheça menções de entidade nunca vistas antes com base apenas em como elas são referenciadas na entrada do usuário. Por exemplo, se você anota o exemplo do usuário de intenção, "I want a flight to Boston", rotulando "Boston" como uma entidade @destination, o serviço pode reconhecer "Chicago" como um @destination em uma entrada do usuário que diz "I want a flight to Chicago". Esse recurso está atualmente disponível somente para inglês. Consulte [Incluindo entidades contextuais](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based) para obter mais informações.

  Quando você acessa a ferramenta com um navegador da web Internet Explorer, não é possível rotular menções de entidade em exemplos do usuário de intenção nem editar texto de exemplo do usuário.
  {: note}

- **Recomendações de entidade**: o serviço pode agora recomendar sinônimos para seus valores de entidade. O recomendador localiza sinônimos relacionados com base na similaridade contextual extraída de um vasto corpo de informações existentes, incluindo grandes fontes de texto escrito, além de usar técnicas de processamento de linguagem natural para identificar palavras semelhantes aos sinônimos existentes em seu valor de entidade. Para obter mais informações, consulte  [ Sinônimos ](/docs/services/assistant?topic=assistant-entities#entities-synonyms).

- **Nova versão da API**: a versão da API atual é agora `2018-07-10`. Esta versão introduz as mudanças a seguir:

  - O conteúdo do objeto `output` de /message mudou de um objeto JSON `text` para uma matriz `generic` que suporta múltiplos tipos de resposta rica, incluindo `image`, `option`, `pause` e `text`.
  - O suporte para entidades contextuais foi incluído.

- **Filtro de data da página de visão geral**: use os novos filtros de data para escolher o período para o qual os dados são exibidos. Esses filtros afetam todos os dados mostrados na página: não apenas o número de conversas exibidas no gráfico, mas também as estatísticas exibidas junto com o gráfico e as listas de principais intenções e entidades. Consulte  [ Controles ](logs-overview#controls)  para obter mais informações.

- **Limite de padrão expandido**: ao usar o campo **Padrões** para [definir padrões específicos para um valor de entidade](/docs/services/assistant?topic=assistant-entities#entities-patterns), o padrão (expressão regular) está agora limitado a 512 caracteres.

## 2 de julho de 2018
{: #2July2018}

- **Saltos de respostas condicionais**: agora é possível configurar uma resposta condicional para ir diretamente para outro nó. Consulte [Respostas condicionais](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple) para obter mais detalhes.

## 21 de junho de 2018
{: #21June2018}

- **Atualizações de idioma para entidades do sistema**: o suporte ao idioma holandês e chinês simplificado está agora geralmente disponível. O suporte ao idioma holandês inclui a correspondência difusa para erros de ortografia. O suporte ao idioma chinês tradicional inclui a disponibilidade de [entidades do sistema](/docs/services/assistant?topic=assistant-system-entities) na liberação beta. Veja [Linguagens suportadas](/docs/services/assistant?topic=assistant-language-support) para obter detalhes.

## 14 de junho de 2018
{: #14June2018}

- **O data center de Washington, DC é aberto**: agora é possível criar instâncias de serviço do {{site.data.keyword.conversationshort}} que estão hospedadas no data center de Washington, DC. Consulte  [ Data centers ](/docs/services/assistant?topic=assistant-services-information#services-information-regions)  para obter mais detalhes.

- **Novo processo de autenticação de API**: o serviço {{site.data.keyword.conversationshort}} tem um novo processo de autenticação de API para instâncias de serviço que são hospedadas nas regiões a seguir:

  - Washington, DC (us-east) a partir de 14 de junho de 2018
  - Sydney, Austrália (au-syd) a partir de 7 de maio de 2018

  O {{site.data.keyword.cloud_notm}} está migrando para a autenticação Identity and Access Management (IAM) baseada em token.

  Para novas instâncias de serviço nas regiões listadas acima, você usa o IAM para autenticação. É possível passar um token de acesso ou uma chave de API. Tokens suportam solicitações autenticadas sem a integração de credenciais de serviço em cada chamada. As chaves API usam autenticação básica.

  Para todas as instâncias de serviço novas e existentes em outras regiões, você continua a usar credenciais de serviço (`{username}:{password}`) para autenticação.

  Ao usar qualquer um dos SDKs do Watson, é possível passar a chave de API e deixar que o SDK gerencie o ciclo de vida dos tokens. Para obter mais informações e exemplos, consulte [Autenticação ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")]https://{DomainName}/apidocs/assistant-v2#authentication){: new_window} na referência de API.

  Se você não tiver certeza de qual tipo de autenticação deve ser usado, visualize as credenciais de serviço clicando na instância de serviço na [Lista de recursos do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/resources){: new_window}.

## 25 de maio de 2018
{: #25May2018}

- **Nova área de trabalho de amostra**: foi mudada a área de trabalho de amostra que é fornecida para você explorar ou para usar como um ponto de início para sua própria área de trabalho. A amostra **Painel de carro** foi substituída por uma amostra **Atendimento ao cliente**. A nova amostra mostra como usar as intenções do catálogo de conteúdo e outros recursos mais novos para construir um robô. Ela pode responder perguntas comuns, como consultas sobre horas e locais da loja, e ilustra como usar um nó com intervalos para planejar compromissos na loja.

- **A renderização de HTML foi incluída em Experimente**: a área de janela "Experimente" renderiza agora a formatação de HTML que é incluída no texto de resposta. Anteriormente, se incluísse um link de hipertexto como uma tag âncora HTML em uma resposta de texto, você veria a origem HTML na área de janela "Experimente" durante o teste. Ele era semelhante a este:

  ` Entre em contato conosco em  <a href="https://www.ibm.com"> ibm.com </a>. `

  Agora, o link de hipertexto é renderizado como se estivesse em uma página da web. Ela é exibida como esta:

  ` Entre em contato conosco em `  [ ibm.com ](https://www.ibm.com){: new_window}.

    Lembre-se, deve-se usar o tipo apropriado de sintaxe em suas respostas para o aplicativo cliente no qual você implementará a conversa. Somente use a sintaxe HTML se o aplicativo cliente puder interpretá-la adequadamente. Outros canais de integração podem esperar outros formatos.

- **Mudanças na implementação**: a opção **Testar no Slack** foi removida.

## 11 de maio de 2018
{: #11May2018}

- **Segurança de informações**: a documentação inclui alguns novos detalhes sobre privacidade de dados. Leia mais em  [ Segurança de informações ](/docs/services/assistant?topic=assistant-information-security).

## 7 de maio de 2018
{: #7May2018}

- **O data center de Sydney, Austrália é aberto**: agora é possível criar as instâncias de serviço do {{site.data.keyword.conversationshort}} que estão hospedadas no data center de Sydney, Austrália. Consulte [Data centers globais do IBM Cloud![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"](https://www.ibm.com/cloud/data-centers/) para obter mais detalhes.

## 4 de abril de 2018
{: #4April2018}

- **Diálogos de procura**: é possível agora [procurar nós de diálogo](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-search) para uma determinada palavra ou frase.

## 15 de março de 2018
{: #15March2018}

- ** Introduzindo o  {{site.data.keyword.conversationfull}} **:  {{site.data.keyword.ibmwatson}}  Conversação foi renomeada. Agora ele é chamado  {{site.data.keyword.conversationfull}}. A mudança de nome reflete o fato de que o serviço está se expandindo para fornecer conteúdo pré-construído e ferramentas que ajudam você a compartilhar mais facilmente os assistentes virtuais construídos. Leia [esta postagem do blog ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/watson/2018/03/the-future-of-watson-conversation-watson-assistant/) para obter mais detalhes.

- **Novas APIs de REST e SDKs estão disponíveis para o Watson Assistant**: as novas APIs são funcionalmente idênticas às APIs do Conversation existentes, que continuam a ser suportadas. Para obter mais informações sobre as APIs do Watson Assistant, consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant){: new_window}.

- **Aprimoramentos de diálogo **: os recursos a seguir foram incluídos na ferramenta de diálogo:

  - Os campos de nome e valor de variável simples estão agora disponíveis, os quais podem ser usados para incluir variáveis de contexto ou atualizar valores de variáveis de contexto. Não é necessário abrir o editor JSON, a menos que você deseje. Consulte [Definindo uma variável de contexto](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-define) para obter mais detalhes.
  - Organize seu diálogo usando as pastas para agrupar os nós de diálogo relacionados. Consulte [Organizando o diálogo com pastas](dialog-build#folders) para obter mais detalhes.
  - O suporte foi incluído para a customização de como cada nó de diálogo participa de digressões iniciadas pelo usuário fora do fluxo de diálogo designado. Consulte [Digressões](dialog-runtime#digressions) para obter mais detalhes.

- **Procurar intenções e entidades**: foi incluído um novo recurso de procura que permite [procurar intenções](intents#searching-intents) para exemplos do usuário, nomes de intenções ou descrições ou [procurar valores e sinônimos de entidade](/docs/services/assistant?topic=assistant-entities#entities-search).

- **Catálogos de conteúdo**: os novos [catálogos de conteúdo](/docs/services/assistant?topic=assistant-catalog#catalog-add) contêm uma única categoria de intenções e entidades comuns pré-construídas que podem ser incluídas em seu aplicativo. Por exemplo, a maioria dos aplicativos requer uma intenção geral #greeting-type que inicia um diálogo com o usuário. É possível incluí-la por meio do catálogo de conteúdo em vez de construir a sua própria.

- **Métricas aprimoradas de usuário**: O componente Melhorar foi aprimorado com métricas de usuário e estatísticas de criação de log adicionais. Por exemplo, a página Visão geral inclui vários gráficos novos e detalhados que resumem interações entre usuários e seu aplicativo, a quantia de tráfego para um determinado período e as intenções e entidades que foram reconhecidas com mais frequência em conversas do usuário.

## 12 de março de 2018
{: #12March2018}

- **Novos métodos de data e hora**: foram incluídos métodos que tornam mais fácil executar cálculos de data por meio do diálogo. Consulte [Cálculos de data e hora](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-and-time-calculations) para obter mais detalhes.

## 16 de fevereiro de 2018
{: #16February2018}

- **Rastreio do nó de diálogo**: quando você usa a área de janela "Experimente" para testar um diálogo, um ícone local é exibido próximo a cada resposta. É possível clicar no ícone para destacar o caminho que o serviço atravessou na árvore de diálogo para chegar à resposta. Veja [Construindo um diálogo](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-test) para obter detalhes.

- **Nova versão de API**: a versão de API atual agora é `2018-02-16`. Esta versão introduz as mudanças a seguir:

  - Um novo parâmetro `include_audit` é agora suportado na maioria das solicitações GET. Esse é um parâmetro booleano opcional que especifica se a resposta deve incluir as propriedades de auditoria (registros de hora `created` e `updated`). O valor padrão é `false`. (Se você estiver usando uma versão de API antes de `2018-02-16`, o valor padrão será `true`.) Para obter mais informações, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant){: new_window}.

  - As respostas de chamadas API que usam a nova versão incluem somente propriedades com valores não `null`.

  - As propriedades `output.nodes_visited` e `output.nodes_visited_details` de respostas de mensagens agora incluem os nós com os tipos a seguir, que eram omitidos anteriormente:

    - Nós com `type`=`response_condition`
    - Nós com `type`=`event_handler` e `event_name`=`input`

## 9 de fevereiro de 2018
{: #9February2018}

- **Entidades do sistema holandês (beta)**: o suporte ao idioma holandês foi aprimorado para incluir a disponibilidade do [Entidades do sistema](/docs/services/assistant?topic=assistant-system-entities) na liberação beta. Veja [Linguagens suportadas](/docs/services/assistant?topic=assistant-language-support) para obter detalhes.

## 29 de janeiro de 2018
{: #29January2018}

- A API de REST do {{site.data.keyword.conversationshort}} agora suporta novos parâmetros de solicitação:
  - Use o parâmetro `append` ao atualizar uma área de trabalho para indicar se os novos dados da área de trabalho devem ser incluídos nos dados existentes, em vez de substituí-los. Para obter mais informações, veja [Atualizar área de trabalho ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant?curl=#update-workspace){: new_window}.
  - Use o parâmetro `nodes_visited_details` ao enviar uma mensagem para indicar se a resposta deve incluir informações de diagnóstico adicionais sobre os nós que foram visitados durante o processamento da mensagem. Para obter mais informações, veja [Enviar mensagem ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.

## 23 de janeiro de 2018
{: #23January2018}

- **Não é possível recuperar a lista de áreas de trabalho**: se você vê esta ou mensagens de erro semelhantes ao trabalhar no conjunto de ferramentas, isso pode significar que sua sessão expirou. Efetue logout escolhendo **Efetuar logout** no ícone **Informações sobre o usuário** ![Menu do ícone Informações sobre o usuário](images/user-icon.png) e, em seguida, efetue login novamente.

## 8 de dezembro de 2017
{: #8December2017}

- **Acesso a dados do log entre instâncias (somente usuários Premium)**: se você é um usuário Premium do {{site.data.keyword.conversationshort}}, suas instâncias premium podem ser opcionalmente configuradas para permitir o acesso a dados do log de áreas de trabalho em suas diferentes instâncias premium.

- **Copiar nós**: agora é possível duplicar um nó para fazer uma cópia dele e seus filhos. Esse recurso é útil ao construir um nó com lógica útil que você deseja reutilizar em outro lugar no diálogo. Veja [Copiando um nó de diálogo](dialog-build#copy-node) para obter mais informações.

- **Capturar grupos em entidades padrão**: é possível identificar grupos no padrão de expressão regular que você define para uma entidade. A identificação de grupos é útil se você deseja ser capaz de referir-se a uma subseção do padrão mais tarde. Por exemplo, a entidade pode ter um padrão de expressão regular que captura os números de telefone dos EUA. Se você identifica o segmento de código de área do padrão de número como um grupo, é possível referir-se subsequentemente a esse grupo para acessar apenas o segmento de código de área de um número de telefone. Veja [Definindo entidades](/docs/services/assistant?topic=assistant-entities#entities-creating-task) para obter mais informações.

## 6 de dezembro de 2017
{: #6December2017}

- **Integração do {{site.data.keyword.openwhisk}} (beta)**: chame ações do {{site.data.keyword.openwhisk}} (anteriormente IBM OpenWhisk) diretamente de um nó de diálogo. Esse recurso permite, por exemplo, chamar uma ação para recuperar informações meteorológicas de dentro de um nó de diálogo e, em seguida, condicionar as informações retornadas na resposta do diálogo. Atualmente, é possível chamar uma ação de uma instância do {{site.data.keyword.openwhisk_short}} que está hospedada na região sul dos EUA por meio de instâncias do {{site.data.keyword.conversationshort}} que estão hospedadas na região sul dos EUA. Veja [Fazendo chamadas programáticas de um nó de diálogo](/doc/services/assistant?topic=assistant-dialog-actions) para obter mais detalhes.

## 5 de dezembro de 2017
{: #5December2017}

- **UI reprojetada para intenções e entidades**: as guias `Intents` e `Entities` foram reprojetadas para fornecer um fluxo de trabalho mais fácil e eficiente ao criar e editar entidades e intenções. Veja [Definindo intenções](intents#creating-intents) e [Definindo entidades](/docs/services/assistant?topic=assistant-entities#entities-creating-task) para obter informações sobre como trabalhar com essas guias.

## 30 de novembro de 2017
{: #30November2017}

- **Suporte a numerais arábicos do leste**: os numerais arábicos do leste são agora suportados em entidades do sistema árabe.

## 29 de novembro de 2017
{: #29November2017}

- **Melhorando o entendimento de entrada do usuário em áreas de trabalho**: agora é possível melhorar uma área de trabalho com elocuções que foram enviadas para outras áreas de trabalho dentro de sua instância. Por exemplo, você pode ter múltiplas versões de áreas de trabalho de produção e áreas de trabalho de desenvolvimento; é possível usar os mesmos dados de elocução para melhorar qualquer uma dessas áreas de trabalho. Consulte  [ Improvando entre áreas de trabalho ](/docs/services/assistant?topic=assistant-logs#logs-deploy-id).

## 20 de novembro de 2017
{: #20November2017}

- **Conformidade GB18030**: GB18030 é um padrão chinês que especifica uma página de códigos estendida para uso no mercado chinês. Esse padrão de página de códigos é importante para a indústria de software porque o Comitê técnico de normatização de tecnologia de informações da China tem exigido que qualquer aplicativo de software que for liberado para o mercado chinês após 1 de setembro de 2001 seja ativado para GB18030. O serviço {{site.data.keyword.conversationshort}} suporta essa codificação e é compatível com o GB18030 certificado.

## 9 de novembro de 2017
{: #9November2017}

- **Os exemplos de intenção podem referenciar entidades diretamente**: agora é possível especificar uma referência de entidade diretamente em um exemplo de intenção. Essa referência de entidade, juntamente com todos os seus valores ou sinônimos, é usada pelo classificador do serviço {{site.data.keyword.conversationshort}} para treinar a intenção. Para obter mais informações, veja [*Entidade como exemplo*](/docs/services/assistant?topic=assistant-intents#intents-entity-as-example) no tópico [Intenções](/docs/services/assistant?topic=assistant-intents).

  Atualmente, é possível referenciar diretamente somente as entidades encerradas que você define. Não é possível referenciar diretamente [entidades padrão](/docs/services/assistant?topic=assistant-entities#entities-pattern) ou [entidades do sistema](/docs/services/assistant?topic=assistant-system-entities).
  {: note}

## 8 de novembro de 2017
{: #8November2017}

- **Conector do {{site.data.keyword.conversationshort}}**: é possível usar a nova ferramenta de conector do {{site.data.keyword.conversationshort}} para conectar sua área de trabalho a um app Slack ou Facebook Messenger que você possui, tornando-a disponível como um robô de bate-papo com o qual os usuários do Slack ou do Facebook Messenger podem interagir. Essa ferramenta está disponível apenas para a região Sul dos EUA do {{site.data.keyword.Bluemix_notm}}.

## 3 de novembro de 2017
{: #3November2017}

- **Atualizações do diálogo**: as atualizações a seguir facilitam a construção de um diálogo. (Veja [Construindo um diálogo](/docs/services/assistant?topic=assistant-dialog-build) para obter detalhes.)

    - É possível incluir uma condição em um intervalo para torná-lo necessário somente se determinadas condições forem atendidas. Por exemplo, é possível tornar um intervalo que pergunta o nome de um cônjuge necessário somente se um intervalo anterior (necessário) que pergunta o status civil indica que o usuário é casado.

    - Agora é possível escolher **Ignorar entrada do usuário** como a próxima etapa para um nó. Ao escolher essa opção, após o processamento do nó atual, o serviço vai diretamente para o primeiro nó-filho do nó atual. Esta opção é semelhante à opção de próxima etapa *Ir para* existente, exceto que ela permite mais flexibilidade. Você não precisa especificar o nó exato para o qual ir. No tempo de execução, o serviço sempre vai para qualquer nó que é o primeiro nó-filho, mesmo se os nós-filhos são reordenados ou novos nós são incluídos após o comportamento de próxima etapa ser definido.

    - É possível incluir respostas condicionais para intervalos. Para respostas Localizado e Não localizado, é possível customizar como o serviço responde com base em se determinadas condições são atendidas. Esse recurso permite verificar possíveis interpretações errôneas e corrigi-las antes de salvar o valor fornecido pelo usuário na variável de contexto do intervalo. Por exemplo, se o intervalo salva a idade do usuário e usa `@sys-number` no campo *Verificar* campo para capturar isso, é possível incluir uma condição que verifica os números acima de 100 e responde com algo como *Forneça uma idade válida em anos.* Veja [Incluindo condições em respostas Localizado e Não localizado](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-handler-next-steps) para obter mais detalhes.

    - A interface que você usa para incluir respostas condicionais em um nó foi reprojetada para tornar mais fácil listar cada condição e sua resposta. Para incluir respostas condicionais no nível do nó, clique em **Customizar** e, em seguida, ative a opção **Múltiplas respostas**.

     A alternância **Múltiplas respostas** configura o recurso como ativado ou desativado somente para a resposta no nível do nó. Ela não controla a capacidade de definir respostas condicionais para um intervalo. A configuração de múltiplas respostas de intervalo é controlada separadamente.
     {: note}

    - Para manter simples a página na qual edita um intervalo, agora você seleciona opções de menu para a.) incluir uma condição que deve ser atendida para o intervalo ser processado e b.) incluir respostas condicionais para as condições Localizado e Não localizado para um intervalo. A menos que você escolha incluir essa funcionalidade extra, a condição do intervalo e os campos de múltiplas respostas não são exibidos, que simplifica a página e a torna mais fácil de usar.

## 25 de outubro de 2017
{: #25October2017}

- **Atualizações para chinês simplificado**: o suporte ao idioma foi aprimorado para chinês simplificado. Isso inclui melhorias na classificação de intenção usando incorporações de palavras no nível de caractere e a disponibilidade de entidades do sistema. Observe que os modelos de aprendizado de serviço do {{site.data.keyword.conversationshort}} podem ter sido atualizados como parte desse aprimoramento, e quando você treinar o seu modelo, todas as mudanças serão aplicadas; veja [Modelos atualizados](#release-notes-updated-models) para obter mais informações.
- **Atualizações para Espanhol** - foram feitas melhorias na classificação da intenção em espanhol, para conjuntos de dados muito grandes.

## 11 de outubro de 2017
{: #11October2017}

- **Atualizações para coreano**: o suporte ao idioma foi aprimorado para coreano. Observe que os modelos de aprendizado de serviço do {{site.data.keyword.conversationshort}} podem ter sido atualizados como parte desse aprimoramento, e quando você treinar o seu modelo, todas as mudanças serão aplicadas; veja [Modelos atualizados](#release-notes-updated-models) para obter mais informações.

## 3 de outubro de 2017
{: #3October2017}

- **Entidades definidas por padrão (Beta)**: agora é possível definir padrões específicos para uma entidade, usando expressões regulares. Isso pode ajudá-lo a identificar entidades que seguem um padrão definido, por exemplo de SKU ou números de peça, números de telefone ou endereços de e-mail. Consulte [Entidades definidas por padrão](/docs/services/assistant?topic=assistant-entities#entities-pattern) para obter detalhes adicionais.
    - É possível incluir sinônimos ou padrões para um único valor de entidade; você não pode incluir ambos.
    - Para cada valor de entidade, pode haver um máximo de até 5 padrões.
    - Cada padrão (expressão regular) é limitado a 128 caracteres.
    - A importação ou exportação por meio de um arquivo CSV não suporta padrões atualmente.
    - A API de REST não suporta acesso direto aos padrões, mas é possível recuperar ou modificar padrões usando o terminal `/values`.

- **Correspondência difusa filtrada por dicionário (somente inglês)**: uma versão melhorada da correspondência difusa para entidades está agora disponível, para o inglês. Esta melhoria evita a captura de algumas palavras comuns válidas em inglês como correspondências difusas para uma determinada entidade. Por exemplo, a correspondência difusa não corresponderá o valor da entidade `like` a `hike` ou `bike`, que são palavras válidas em inglês, mas continuará correspondendo exemplos como `lkie` ou `oike`.

## 27 de setembro de 2017
{: #27September2017}

- **Atualizações do construtor de condição**: o controle que é exibido para ajudá-lo a definir uma condição em um nó de diálogo foi atualizado. Os aprimoramentos incluem suporte para listar nomes de variável de contexto disponíveis após você inserir o $ para começar a incluir uma variável de contexto.

## 31 de agosto de 2017
{: #31August2017}

- **Retrocesso da seção Melhorar**: a métrica de tempo médio de conversa e os filtros correspondentes estão sendo removidos temporariamente da página Visão geral da seção Melhorar. Essa remoção evitará que o cálculo de determinadas métricas faça com que a métrica de tempo médio de conversa e o gráfico de conversas ao longo do tempo, exibam informações imprecisas. A IBM lamenta a remoção da funcionalidade da ferramenta, mas está comprometida em assegurar que informações precisas sejam comunicadas aos usuários.
- **Nomes de nós de diálogo**: agora é possível designar qualquer nome a um nó de diálogo; ele não precisa ser exclusivo. E será possível mudar posteriormente o nome do nó sem impactar a forma como o nó será referenciado internamente. O nome especificado é salvo como um atributo de título do nó no arquivo JSON da área de trabalho e o sistema usa um ID exclusivo que é armazenado no atributo de nome para referenciar o nó.

## 23 de agosto de 2017
{: #23August2017}

- **Atualizações para coreano, japonês e italiano**: o suporte ao idioma foi aprimorado para coreano, japonês e italiano. Observe que os modelos de aprendizado de serviço do {{site.data.keyword.conversationshort}} podem ter sido atualizados como parte desse aprimoramento, e quando você treinar o seu modelo, todas as mudanças serão aplicadas; veja [Modelos atualizados](#release-notes-updated-models) para obter mais informações.

## 10 de agosto de 2017
{: #10August2017}

- **Normalização do acento**: em uma configuração de conversação, os usuários podem ou não usar acentos enquanto interagem com o serviço {{site.data.keyword.conversationshort}}. Dessa forma, foi feita uma atualização no algoritmo para que as versões acentuadas e não acentuadas de palavras sejam tratadas da mesma forma para detecção de intenção e reconhecimento de entidade.

  No entanto, para alguns idiomas, como o espanhol, alguns acentos podem alterar o significado da entidade. Dessa forma, para a detecção de entidade, embora a entidade original possa ter implicitamente um acento, o serviço também poderá corresponder à versão não acentuada da mesma entidade, mas com uma pontuação de confiança ligeiramente mais baixa.

  Por exemplo, para a palavra `barrió`, que tem um acento e corresponde ao passado do verbo `barrer` (varrer), o serviço também pode corresponder à palavra `barrio` (vizinhança), mas com uma confiança ligeiramente mais baixa.

  O sistema fornecerá a maior pontuação de confiança em entidades com correspondências exatas. Por exemplo, `barrio` não será detectado se `barrió` estiver no conjunto de treinamento, e `barrió` não será detectado se `barrio` estiver no conjunto de treinamento.

  Você deverá treinar o sistema com os caracteres e acentos apropriados. Por exemplo, se você estiver esperando `barrió` como uma resposta, coloque `barrió` no conjunto de treinamento.

  Embora não seja um acento, o mesmo se aplica às palavras que usam, por exemplo, a letra espanhola `ñ` versus a letra `n`, como `uña` versus `una`. Nesse caso, a letra `ñ` não é apenas um `n` com um acento; ela é uma letra exclusiva, específica do espanhol.

  Se você achar que os seus clientes não usarão os acentos apropriados ou usarão palavras erradas (incluindo, por exemplo, a colocação de um `n` em vez de um `ñ`), ative a correspondência difusa ou inclua-os explicitamente nos exemplos de treinamento.

  **Nota:** A normalização de acento está ativada para português, espanhol, francês e tcheco.

- **Sinalização de opção por não participar da área de trabalho**: a API de REST do {{site.data.keyword.conversationshort}} agora suporta uma sinalização de opção por não participar para áreas de trabalho. Essa sinalização indica que os dados de treinamento da área de trabalho como intenções e entidades não devem ser usados pela IBM para melhorias gerais do serviço. Para obter mais informações, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant?curl=#data-collection){: new_window}

## 7 de agosto de 2017
{: #7August2017}

- **Interpretação de datas `next` e `last`**: o serviço {{site.data.keyword.conversationshort}} trata as datas `last` e `next` como referência ao último ou próximo dia mais imediato referenciado, que pode estar na mesma semana ou em uma semana anterior. Consulte o tópico [entidades do sistema](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-datetime) para obter informações adicionais.

## 3 de agosto de 2017
{: #3August2017}

- **Correspondência difusa para idiomas adicionais (Beta)**: a correspondência difusa para entidades agora está disponível para idiomas adicionais, conforme observado no tópico [Idiomas suportados](/docs/services/assistant?topic=assistant-language-support).
- **Correspondência parcial (beta - somente inglês)**: a correspondência difusa agora sugerirá automaticamente sinônimos baseados em subsequências presentes em entidades definidas pelo usuário e designará uma pontuação de confiança inferior quando comparada à correspondência de entidade exata. Veja [Correspondência difusa](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching) para obter detalhes.

## 28 de julho de 2017
{: #28July2017}

- Ao configurar as preferências bidirecionais para o conjunto de ferramentas, agora é possível especificar a direção da interface gráfica com o usuário.
- O esquema de cores do conjunto de ferramentas foi atualizado para ficar consistente com outros serviços e produtos do Watson.

## 19 de julho de 2017
{: #19July2017}

- A API de REST do {{site.data.keyword.conversationshort}} agora suporta o acesso a nós de diálogo. Para obter mais informações, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant?curl=#list-dialog-nodes){: new_window}.

## 14 de julho de 2017
{: #14July2017}

- A funcionalidade de intervalos de diálogos foi aprimorada. Por exemplo, foi incluída uma propriedade *slot_in_focus* que pode ser usada para definir uma condição que se aplica somente a um único intervalo. Veja [Reunindo informações com intervalos](/docs/services/assistant?topic=assistant-dialog-slots) para obter detalhes.

## 12 de julho de 2017
{: #12July2017}

- **Suporte para tcheco**: o suporte ao idioma tcheco foi introduzido; veja o tópico [Linguagens suportadas](/docs/services/assistant?topic=assistant-language-support) para obter detalhes adicionais.

## 11 de julho de 2017
{: #11July2017}

- **Testar no Slack**: é possível usar a nova ferramenta **Testar no Slack** para implementar rapidamente sua área de trabalho como um usuário robô do Slack para propósitos de teste. Essa ferramenta está disponível apenas para a região Sul dos EUA do {{site.data.keyword.Bluemix_notm}}.
- **Atualizações para árabe**: o suporte ao idioma árabe foi aprimorado para incluir a pontuação absoluta por intenção e a capacidade de marcar intenções como irrelevantes; veja o tópico [Linguagens suportadas](/docs/services/assistant?topic=assistant-language-support) para obter detalhes adicionais. Observe que os modelos de aprendizado de serviço do {{site.data.keyword.conversationshort}} podem ter sido atualizados como parte desse aprimoramento, e quando você treinar o seu modelo, todas as mudanças serão aplicadas; veja [Modelos atualizados](#release-notes-updated-models) para obter mais informações.

## 23 de junho de 2017
{: #23June2017}

- **Atualizações para coreano**: o suporte ao idioma coreano foi aprimorado; veja o tópico [Linguagens suportadas](/docs/services/assistant?topic=assistant-language-support) para obter detalhes adicionais. Observe que os modelos de aprendizado de serviço do {{site.data.keyword.conversationshort}} podem ter sido atualizados como parte desse aprimoramento, e quando você treinar o seu modelo, todas as mudanças serão aplicadas; veja [Modelos atualizados](#release-notes-updated-models) para obter mais informações.

## 22 de junho de 2017
{: #22June2017}

- **Apresentando os intervalos**: agora é mais fácil coletar múltiplas informações de um usuário em um único nó incluindo intervalos. Anteriormente, você precisava criar vários nós de diálogo para cobrir todas as combinações possíveis de maneiras que os usuários poderiam fornecer as informações. Com os intervalos, é possível configurar um único nó que salva quaisquer informações que o usuário forneça e solicita quaisquer detalhes necessários que o usuário não forneça. Veja [Reunindo informações com intervalos](/docs/services/assistant?topic=assistant-dialog-slots) para obter mais detalhes.
- **Árvore de diálogo simplificada** - a árvore de diálogo foi reprojetada para melhorar sua usabilidade. A visualização em árvore está mais compacta, portanto, é mais fácil ver onde você está nela. E os links entre nós são representados de uma maneira que torna mais fácil entender os relacionamentos entre os nós.

## 21 de junho de 2017
{: #21June2017}

- **Suporte ao árabe**: o suporte ao idioma para árabe agora está geralmente disponível. Para obter detalhes, consulte [Configurando idiomas bidirecionais](/docs/services/assistant?topic=assistant-language-support#language-support-configuring-bi-directional).
- **Atualizações de linguagem**: os algoritmos do serviço {{site.data.keyword.conversationshort}} foram atualizados para melhorar o suporte ao idioma geral. Consulte o tópico [Idiomas Suportados](/docs/services/assistant?topic=assistant-language-support) para obter detalhes.

## 16 de junho de 2017
{: #16June2017}

- **Recomendações (beta - somente usuários Premium)**: o painel Melhorar também inclui uma página **Recomendações** que recomenda maneiras de melhorar seu sistema analisando as conversas que os usuários têm com seu robô de bate-papo e levando em conta os dados de treinamento de seu sistema e a certeza de resposta.

## 14 de junho de 2017
{: #14June2017}

- **Correspondência difusa para idiomas adicionais (Beta)**: a correspondência difusa para entidades agora está disponível para idiomas adicionais, conforme observado no tópico [Idiomas suportados](/docs/services/assistant?topic=assistant-language-support). Será possível ativar a correspondência difusa por entidade para melhorar a capacidade do serviço para reconhecer termos na entrada do usuário com sintaxe que é semelhante à entidade, sem exigir uma correspondência exata. O recurso é capaz de mapear a entrada do usuário para a entidade correspondente, apesar da presença de erros de ortografia ou de pequenas diferenças sintáticas. Por exemplo, se você definir girafa como um sinônimo para uma entidade animal e a entrada do usuário contiver os termos girafas ou girafa, a correspondência difusa será capaz de mapear o termo para a entidade animal corretamente. Veja [Correspondência difusa](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching) para obter detalhes.

## 13 de junho de 2017
{: #13June2017}

- **Conversas do usuário**: o painel Melhorar agora inclui uma página **Conversas do usuário**, que fornece uma lista de interações com o usuário com seu robô de bate-papo que podem ser filtradas por palavra-chave, intenção, entidade ou número de dias. É possível abrir conversas individuais para corrigir intenções ou para incluir valores de entidade ou sinônimos.
- **Mudança de expressão regular**: as expressões regulares suportadas por funções SpEL como find, matches, extract, replaceFirst, replaceAll e split mudaram. Um grupo de construções de expressões regulares não é mais permitido, incluindo as construções look-ahead, look-behind, de repetição possessiva e backreference. Essa mudança foi necessária para evitar uma exposição de segurança na biblioteca de expressões comuns original.

## 12 de junho de 2017
{: #12June2017}

- O número máximo de áreas de trabalho que você pode criar com o plano **Lite** (anteriormente chamado de plano Grátis) foi mudado de 3 para 5.
- Agora é possível designar qualquer nome para um nó de diálogo; ele não precisa ser exclusivo. E será possível mudar posteriormente o nome do nó sem impactar a forma como o nó será referenciado internamente. O nome que você especifica é tratado como um alias e o sistema usa seu próprio identificador interno para referenciar o nó.
- Não é mais possível mudar o idioma de uma área de trabalho depois de criá-lo editando os detalhes da área de trabalho. Se você precisar mudar o idioma, poderá exportar a área de trabalho como um arquivo JSON, atualizar a propriedade de idioma e, em seguida, importar o arquivo JSON como uma nova área de trabalho.

## 6 de junho de 2017
{: #6June2017}

- **Aprender**: uma nova página *Aprenda sobre o {{site.data.keyword.conversationfull}}* está disponível que fornece as informações de introdução e links para a documentação de serviço e outros recursos úteis. Para abrir a página, clique no ícone ![i para informações.](images/info.png) no cabeçalho da página.
- **Exportação e exclusão em massa**: agora é possível exportar simultaneamente inúmeras intenções ou entidades para um arquivo CSV, para que então seja possível importar e reutilizá-las para outro aplicativo {{site.data.keyword.conversationshort}}. Também é possível selecionar simultaneamente inúmeras entidades ou intenções para exclusão em massa.
- **Atualizações para coreano**: os tokenizers coreanos foram atualizados para direcionar o suporte ao idioma informal. A IBM continua trabalhando em melhorias para o reconhecimento e a classificação de entidade.
- **Suporte ao emoji**: os emojis incluídos em exemplos de intenção ou como valores de entidade agora serão classificados/extraídos corretamente.

  Somente emojis que estão incluídos em seus dados de treinamento serão identificados de forma correta e consistente; o suporte de emoji pode não classificar corretamente emojis semelhantes com diferentes tons de cores ou outras variações.
  {: note}

- **Stemming de entidade (beta - somente inglês)**: o recurso beta de correspondência difusa reconhece entidades e as corresponde com base no formato raiz do valor da entidade. Por exemplo, esse recurso reconhece corretamente 'bananas' como sendo semelhante a 'banana' e 'executar' sendo semelhante a 'executando' pois eles compartilham um formato raiz comum. Para obter mais informações, consulte [Correspondência Difusa](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching).
- **Progresso de importação da área de trabalho**: ao importar uma área de trabalho de um arquivo JSON, um tile para a área de trabalho é exibido imediatamente, no qual as informações sobre o progresso da importação são exibidas.
- **Tempo de treinamento reduzido**: múltiplos modelos agora são treinados em paralelo, o que reduz significativamente o tempo de treinamento para áreas de trabalho grandes.

## 26 de maio de 2017
{: #26May2017}

- A versão de API atual agora é `2017-05-26`. Esta versão introduz as mudanças a seguir:
    - O esquema de objetos ErrorResponse mudou. Essa mudança afeta todos os terminais e métodos. Para obter mais informações, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant){: new_window}.
    - O esquema interno usado para representar os nós de diálogo no JSON da área de trabalho exportado mudou. Se você usar a API `2017-05-26` para importar uma área de trabalho que foi exportada usando uma versão anterior, alguns nós de diálogo poderão não ser importados corretamente. Para obter melhores resultados, sempre importe uma área de trabalho usando a mesma versão que foi usada para exportá-la.

## 25 de maio de 2017
{: #25May2017}

- Agora é possível gerenciar variáveis de contexto na área de janela "Experimente". Clique no link **Gerenciar contexto** para abrir uma nova área de janela na qual é possível configurar e verificar os valores de variáveis de contexto à medida que você testa o diálogo. Consulte [Testando seu diálogo](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-test) para obter informações adicionais.

## 16 de maio de 2017
{: #16May2017}

- Uma área de trabalho de amostra **Painel do Carro** agora está disponível quando você abre a ferramenta. Para usar a amostra como um ponto de início para sua própria área de trabalho, edite a área de trabalho. Se você desejar usá-lo para múltiplas áreas de trabalho, duplique-o como alternativa. A área de trabalho de amostra não considera o total de sua área de trabalho de assinatura, a menos que você a utilize.
- Agora é mais fácil navegar pela ferramenta. As opções do menu de navegação estão disponíveis no lado da página principal em vez de na parte superior. Na parte superior da página, há links de Trilha de Navegação que mostram onde você está. Agora é possível alternar entre as instâncias de serviço da página Áreas de Trabalho. Para chegar lá rapidamente, clique em **Voltar para áreas de trabalho** no menu de navegação. Se você tiver várias instâncias de serviço, o nome da instância atual será exibido. É possível clicar no link **Mudar** ao lado dele para escolher outra instância.
- Ao criar um diálogo, dois nós agora são incluídos nele para você: 1) um nó **Bem-vindo** na parte superior da árvore de diálogo que contém a saudação a ser exibida para o usuário e 2) um nó **Qualquer outra coisa** na parte inferior da árvore que captura quaisquer consultas do usuário que não sejam reconhecidas por outros nós no diálogo e responde a elas. Consulte [Criando um diálogo](/docs/services/assistant?topic=assistant-dialog-build) para obter mais detalhes.
- Quando você está testando um diálogo na área de janela "Experimente", agora é possível localizar e reenviar uma elocução de teste recente pressionando a tecla Para Cima para percorrer suas entradas anteriores.
- Agora está disponível o suporte ao idioma coreano experimental para 5 entidades do sistema (`@sys-data`, `@sys-time`, `@sys-currency`, `@sys-number`, `@sys-percentage`). Existem problemas conhecidos para algumas das entidades numéricas e suporte limitado para a entrada de linguagem informal.
- Uma página Visão Geral está disponível na guia Melhorar. A página fornece um resumo de interações com seu robô. Será possível ver a quantia de tráfego para um determinado período de tempo, bem como as intenções e entidades que foram reconhecidas mais frequentemente nas conversas do usuário. Para obter informações adicionais, consulte [Usando a página Visão Geral](/docs/services/assistant?topic=assistant-logs-overview).

## 27 de abril de 2017
{: #27April2017}

- As entidades do sistema a seguir agora estão disponíveis como recursos beta apenas em inglês:
    - sys-location: reconhece referências a locais como vilas, cidades e países em elocuções do usuário.
    - sys-person: reconhece referências a nomes de pessoas, primeiro e último, em elocuções do usuário.

    Para obter mais informações, consulte a [Referência de entidades do sistema](/docs/services/assistant?topic=assistant-system-entities).
- A correspondência difusa para entidades é um recurso beta que agora está disponível em inglês. Será possível ativar a correspondência difusa por entidade para melhorar a capacidade do serviço para reconhecer termos na entrada do usuário com sintaxe que é semelhante à entidade, sem exigir uma correspondência exata. O recurso é capaz de mapear a entrada do usuário para a entidade correspondente, apesar da presença de erros de ortografia ou de pequenas diferenças sintáticas. Por exemplo, se você definir **girafa** como um sinônimo para uma entidade animal e a entrada do usuário contiver os termos *girafas* ou *girafa*, a correspondência difusa será capaz de mapear o termo para a entidade animal corretamente. Veja [Definindo entidades](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching) e procure `Correspondência difusa` para obter detalhes.

## 18 de abril de 2017
{: #18April2017}

- A API de REST do {{site.data.keyword.conversationshort}} agora suporta o acesso aos recursos a seguir:
    - entidades
    - valores de entidade
    - sinônimos de valor da entidade
    - Logs

    Para obter mais informações, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant){: new_window}.
- O comportamento do método `POST` /messages mudou a manipulação de entidades e intenções especificadas como parte da entrada da mensagem:
    - Se você especificar intenções na entrada, o serviço usará as intenções especificadas, mas usará processamento de língua natural para detectar entidades na entrada do usuário.
    - Se você especificar entidades na entrada, o serviço usará as entidades especificadas, mas usará processamento de língua natural para detectar intenções na entrada do usuário.

        O comportamento não foi mudado para mensagens que especificam intenções e entidades, ou para mensagens que não especificam nenhuma das duas.
- A opção para marcar a entrada do usuário como irrelevante agora está disponível para todos os idiomas suportados. Este é um recurso beta.
- Uma nova guia Credenciais fornece um único local no qual você pode localizar todas as informações de que precisa para conectar seu aplicativo a uma área de trabalho (como as credenciais de serviço e o ID da área de trabalho), bem como outras opções de implementação. Para acessar a guia Credenciais para sua área de trabalho, clique no ícone ![Menu](images/Menu_16.png) e selecione **Credenciais**.

## 9 de março de 2017
{: #9March2017}

A API de REST do {{site.data.keyword.conversationshort}} agora suporta o acesso aos recursos a seguir:

- áreas de trabalho
- intenções
- exemplos
- contra-exemplos

Para obter mais informações, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant){: new_window}.

## 7 de março de 2017
{: #7March2017}

- O uso de `.` ou `..` como um nome de intenção causa problemas e não é mais suportado.

    Não é possível renomear ou excluir uma intenção com este nome; para mudar o nome, exporte suas intenções para um arquivo, renomeie a intenção no arquivo e importe o arquivo atualizado em sua área de trabalho.

    Os clientes pagantes podem entrar em contato com o suporte para uma mudança do banco de dados.

## 1 de março de 2017
{: #1March2017}

- Entidades do sistema agora são ativadas em alemão.

## 22 de fevereiro de 2017
{: #22February2017}

- As mensagens agora estão limitadas a 2.048 caracteres.

## 3 de fevereiro de 2017
{: #3February2017}

- Nós mudamos como as intenções são pontuadas e incluímos a capacidade de marcar a entrada como irrelevante para seu aplicativo. Para obter detalhes, veja [Definindo intenções](/docs/services/assistant?topic=assistant-intents) e procure `Mark as irrelevant`.

- Esta liberação introduziu uma grande mudança na área de trabalho. Para se beneficiar das mudanças, deve-se fazer upgrade manualmente de sua área de trabalho.

- O processamento de ações **Ir para** mudou para evitar loops que possam ocorrer sob determinadas condições. Anteriormente, se você pulasse para a condição de um nó e nem o nó nem qualquer um de seus nós de mesmo nível tivessem uma condição que foi avaliada como verdadeira, o sistema pularia para o nó de nível raiz e procuraria por um nó cuja condição correspondesse à entrada. Em algumas situações, esse processamento criou um loop, o que impediu o progresso do diálogo.

  Sob o novo processo, se nem o nó de destino nem seus peers forem avaliados como verdadeiros, a rodada do diálogo será finalizada. Para reimplementar o modelo antigo, inclua um nó de mesmo nível final com uma condição de `true`. Na resposta, use uma ação **Ir para** destinada à condição do primeiro nó no nível raiz de sua árvore de diálogos.

## 11 de janeiro de 2017
{: #11January2017}

- Nesta liberação, é possível customizar títulos de nós no diálogo.

## 22 de dezembro de 2016
{: #22December2016}

- Nesta liberação, nós de diálogo exibem uma nova seção para `node title`. A capacidade de customizar o `node title` não está disponível. Quando reduzido, o `node title` exibe a `node condition` do nó de diálogo. Se não houver uma `node condition`, "Nó Sem Título" será exibido como o título.

## 19 de dezembro de 2016
{: #19December2016}

Várias mudanças tornam o editor de diálogo mais fácil e mais intuitivo para usar:

- Uma visualização de edição maior torna mais fácil visualizar todos os detalhes de um nó à medida que você trabalha nele.
- Um nó pode conter várias respostas, cada uma acionada por uma condição separada. Para obter mais informações, veja [Múltiplas respostas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses).

## 5 de dezembro de 2016
{: #5December2016}

- Novos idiomas são suportados, todos no modo Experimental: alemão, chinês tradicional, chinês simplificado e holandês.
- Duas novas entidades do sistema estão disponíveis: @sys-data e @sys-time. Para obter detalhes, consulte [Entidades do Sistema](/docs/services/assistant?topic=assistant-system-entities).

## 21 de outubro de 2016
{: #21October2016}

- O serviço do {{site.data.keyword.conversationshort}} agora fornece entidades do sistema, que são entidades comuns que podem ser usadas em qualquer caso de uso. Para obter detalhes, veja [Definindo entidades](/docs/services/assistant?topic=assistant-entities) e procure `Ativando entidades do sistema`.
- Agora é possível visualizar um histórico de conversas com usuários na página Melhorar. É possível usar isso para entender o comportamento de seu robô. Para obter detalhes, consulte  [ Melhorando sua habilidade ](/docs/services/assistant?topic=assistant-logs-intro).
- Agora é possível importar entidades de um arquivo CSV (Valor Separado por Vírgula), que ajuda quando você tem um grande número de entidades. Para obter detalhes, veja [Definindo entidades](/docs/services/assistant?topic=assistant-entities) e procure `Importando entidades`.

## 20 de setembro de 2016
{: #20September2016}

**Nova versão**: 2016-09-20

Para aproveitar as mudanças em uma nova versão, mude o valor do parâmetro `version` para a nova data. Se você não estiver pronto para atualizar para esta versão, não mude sua data da versão.

- Versão **2016-09-20**: `dialog_stack` foi mudado de uma matriz de sequências para uma matriz de objetos JSON.

## 29 de agosto de 2016
{: #29August2016}

- É possível mover os nós de diálogo de uma ramificação para outra, como irmãos ou de mesmo nível. Para obter detalhes, veja [Movendo um nó de diálogo](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-move-node).
- É possível expandir a janela do editor JSON.
- É possível visualizar os logs de bate-papo de conversas de seu robô para ajudá-lo a entender seu comportamento. É possível filtrar por intenções, entidades, data e horário. Para obter detalhes, consulte  [ Melhorando sua habilidade ](/docs/services/assistant?topic=assistant-logs-intro)

## 11 de julho de 2016
{: #21July2016}

- Esta liberação de Disponibilidade Geral permite trabalhar com entidades e diálogos para criar um robô totalmente funcional.

## 18 de maio de 2016
{: #18May2016}

- Esta liberação Experimental do {{site.data.keyword.conversationshort}} apresenta a interface com o usuário e permite trabalhar com áreas de trabalho, intenções e exemplos.
