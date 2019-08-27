---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# Integrando à Intercom ![Somente plano Plus ou Premium](images/plus.png)
{: #deploy-intercom}

Intercom é uma plataforma de sistema de mensagens do cliente que ajuda a impulsionar o crescimento de negócios por meio de relacionamentos melhores em todo o ciclo de vida do cliente.
{: shortdesc}

A Intercom fez parceria com a IBM para incluir um novo agente na equipe de suporte ao cliente, um Watson Assistent virtual. É possível integrar seu assistente a um aplicativo Intercom para permitir que o app passe, sem interrupções, as conversas do usuário entre seu assistente e os agentes humanos. Leia a [postagem do blog do Watson ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://medium.com/@blakemcgregor/contact-center-post-394dff427c8) para saber mais sobre a integração.

Essa integração está disponível somente para usuários do plano Plus ou Premium. Para experimentar, é possível se inscrever para obter um plano grátis do Plus Trial. [Obtenha o Plus Trial](https://cloud.ibm.com/registration?target=%2Fdeveloper%2Fwatson%2Flaunch-tool%2Fconversation%3Fplan%3Dplus-trial&cm_mmc=OSocial_Voicestorm-_-Watson+AI_Watson+Core+-+Conversation-_-WW_WW-_-Intercom+Trial+Registration+Link&cm_mmca1=000027BD&cm_mmca2=10004432).
{: note}

Se você integrar o assistente à Intercom, o aplicativo Intercom se tornará o aplicativo voltado para o cliente para sua qualificação. Todas as interações com usuários serão iniciadas por meio da Intercom e gerenciadas por ela.

Atualmente, não há como passar uma conversa em andamento de um canal de integração para outro.

## Criação de agente único
{: #deploy-intercom-account-prereq}

Você ou alguém em sua organização deve concluir estas etapas de pré-requisito único antes de incluir a integração da Intercom em seu assistente.

1.  Crie uma conta de e-mail funcional para seu assistente.

    Cada assistente deve ter um endereço de e-mail válido e exclusivo antes que ele possa ser incluído em uma equipe na Intercom.
1.  Em sua área de trabalho Intercom, inclua o assistente em sua equipe como um novo agente.

    Acesse a página de configurações do colega de equipe em sua área de trabalho Intercom, convide o assistente como um novo agente incluindo o e-mail que você criou na etapa anterior no campo de convite.

    Se você não tiver uma área de trabalho Intercom configurada ainda, crie uma em [www.intercom.com ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.intercom.com). No mínimo, você precisa de uma assinatura para o produto *Inbox* da Intercom para ser capaz de criar uma área de trabalho.

1.  Na conta de e-mail do assistente que você criou anteriormente, localize o convite da Intercom. Clique no link no e-mail para se associar à equipe. Inscreva-se usando o endereço de e-mail funcional do assistente e, em seguida, associe-se à equipe.

1.  **Opcional**: atualize o perfil para seu assistente.

    É possível editar o nome e a foto do perfil para seu assistente. Esse perfil representa o assistente nas comunicações do agente privado dentro de sua área de trabalho e em interações públicas com clientes por meio de seus apps Intercom. Crie um perfil que reflita sua marca.

    Clique no ícone do perfil da Intercom na barra de navegação para acessar as configurações de perfil e área de trabalho.

    ![Captura de tela da página Configuração de Intercom.](images/intercom-settings.png)

## Preparando o diálogo
{: #deploy-intercom-dialog-prereq}

Se não tiver uma qualificação de diálogo associada ao seu assistente, crie ou inclua uma agora. Consulte [Construindo um diálogo](/docs/services/assistant?topic=assistant-dialog-build) para obter mais detalhes.

O acionamento de uma procura por meio de uma qualificação de procura não é suportado atualmente em uma integração do Intercom.
{: note}

Conclua estas etapas em sua qualificação de diálogo para que o assistente possa manipular solicitações do usuário e possa passar a conversa para um agente humano quando um cliente solicitar uma.

1.  Inclua uma intenção em sua qualificação que possa reconhecer a solicitação de um usuário para falar com um humano.

    É possível criar sua própria intenção ou incluir a intenção pré-construída denominada `#General_Connect_to_Agent` que é fornecida com o catálogo de conteúdo **Geral** desenvolvido pela IBM.

1.  Inclua um nó raiz em seu diálogo que condicione a intenção criada na etapa anterior. Escolha **Conectar-se ao agente humano** como o tipo de resposta.

1.  Prepare cada ramificação de diálogo para ser acionada pelo assistente por meio do app Intercom.

    Cada nó de diálogo raiz no diálogo pode ser processado pelo assistente enquanto ele está funcionando como um colega de equipe da Intercom, incluindo nós raiz em pastas. Você especificará a ação que deseja que o assistente tome para cada ramificação de diálogo posteriormente quando configurar a integração da Intercom. Portanto, embora não seja possível ocultar um nó da Intercom, será possível configurar o assistente para não fazer nada quando o nó for acionado.

    Preencha os campos a seguir do nó raiz de cada ramificação de diálogo:

    - **Nome do nó**: forneça um nome para o nó. Esse nome é como você identificará o nó quando configurar interações para ele posteriormente. Se não incluir um nome, você terá que escolher o nó com base em seu ID do nó.
    - **Nome do nó externo**: inclua um resumo do propósito da ramificação de diálogo. Por exemplo,  * Localize uma loja *.

      Essas informações são mostradas a outros agentes na equipe do assistente quando o assistente se oferece para responder a uma consulta do usuário. Se houver mais de um nó de diálogo que possa direcionar a consulta, o assistente compartilhará uma lista de opções de resposta com os colegas de equipe do agente humano para obter seus conselhos sobre qual resposta usar.

      ![Captura de tela do campo na visualização de edição do nó na qual você inclui o resumo do propósito do nó.](images/disambig-node-purpose.png)

      **Não** inclua um nome de nó externo no nó raiz criado na Etapa 2. Quando uma intensificação ocorre, seu assistente consulta o nome de nó externo do último nó processado para saber qual objetivo do usuário não foi atendido com sucesso. Se você incluir um nome de nó externo no nó com a conexão com a intenção do agente humano, você impedirá que seu assistente aprenda o último nó real e orientado por objetivo com o qual o usuário interagiu antes da intensificação do problema.
      {: tip}

1.  Se um nó-filho nas condições de ramificação em uma solicitação de acompanhamento ou pergunta que você não deseja que o assistente manipule, inclua um tipo de resposta **Conectar-se ao agente humano** no nó.

    Por exemplo, você pode desejar incluir esse tipo de resposta em nós que cobrem somente problemas sensíveis que um humano deve manipular ou que rastreiam quando um assistente falha repetidamente em entender um usuário.

    No tempo de execução, se a conversa atingir esse nó-filho, o diálogo será passado para um agente humano nesse ponto. Posteriormente, quando você configurar a integração do Intercom, será possível escolher um agente humano como um backup para cada ramificação.

Seu diálogo está agora pronto para suportar seu assistente na Intercom.

### Considerações sobre Diálogo
{: #deploy-intercom-dialog}

Algumas respostas ricas que você inclui em um diálogo são exibidas dentro da área de janela "Experimente" de forma diferente de como elas são exibidas para os usuários da Intercom. A tabela a seguir descreve como os tipos de resposta são tratados pela Intercom.

| Tipo de resposta | Como é exibido para os usuários da Intercom  |
|---------------|---------------------------|
| **Opção**    | As opções são exibidas como uma lista numerada. No campo **título** ou **descrição**, forneça instruções que explicam ao usuário como escolher uma opção na lista. |
| **Imagem**     | O **título** da imagem, a **descrição** e a imagem em si são renderizados. |
| **Pausar**     | Independentemente de você ativá-lo ou não, um indicador de digitação não será exibido durante a pausa. |

Consulte [Respostas avançadas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia) para obter mais informações sobre os tipos de resposta.

## Incluindo uma Integração de Intercom
{: #deploy-intercom-add-intercom}

1.  Na guia Assistentes, clique para abrir o quadro do assistente que você deseja implementar.

1.  Na seção Integrações, clique em **Incluir integração**.

1.  Clique em  ** Intercom **.

    Siga as instruções que são fornecidas na tela. As seções a seguir ajudam você com as etapas.

O vídeo de 4 minutos a seguir ilustra as etapas.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Configuração rápida" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/SkbFWNScueU" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Conectando o assistente à Intercom
{: #deploy-intercom-connect}

Assim que você fornece à Intercom a permissão para usar o assistente, o assistente se torna um membro viável da equipe da Intercom.

Os agentes humanos podem designar mensagens ao assistente usando as regras de designação do Intercom. As mensagens podem ser designadas ao assistente das maneiras a seguir:

- Designação automática de conversas de entrada para um membro da equipe ou uma caixa de entrada da equipe com base em alguns critérios

  O vídeo de 1,5 minuto a seguir ilustra as etapas.

  <iframe class="embed-responsive-item" id="youtubeplayer2" title="Designação automática" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/4M9wu8NHxcY" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

- Redesignação manual feita por um agente humano no tempo de execução.

  O vídeo a seguir de menos de 3 minutos ilustra as etapas.

  <iframe class="embed-responsive-item" id="youtubeplayer3" title="Designação manual" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/jAnolyUJAIA" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Consulte a [Documentação da Intercom ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.intercom.com/help/support-and-retain-customers/work-as-a-team/assign-conversations-to-teammates-and-teams) para obter mais detalhes.

1.  Quando seu diálogo estiver pronto, clique em **Conectar agora**.
1.  Clique em **Acessar Intercom** para ser redirecionado para o site da Intercom.

     Efetue login na Intercom usando o endereço de e-mail funcional do assistente e a senha, não os seus próprios. Você deseja estabelecer a conexão com um ID funcional que é compartilhado por mais de uma pessoa em sua organização.
     {: important}

1.  Clique em **Autorizar acesso**.
1.  Clique em  ** Voltar para Visão Geral **.

Agora o assistente está disponível para receber designações dos colegas de equipe da Intercom. Se você ainda não tiver configurado, [configure seu diálogo](#deploy-intercom-dialog-prereq).
{: important}

## Configurando o roteamento de
{: #deploy-intercom-config-backup}

Designe colegas de equipe humanos como backups para o assistente no caso de o assistente precisar transferir uma conversa em andamento para um humano. É possível escolher uma equipe ou um colega de equipe diferente para ser o contato de backup para cada ramificação de diálogo.

Para configurar designações de roteamento para escaladas do assistente para um humano, conclua as etapas a seguir:

1.  Na página de integração da Intercom, clique em **Minha qualificação de diálogo está pronta** para confirmar que você preparou seu diálogo.

    Somente clique nesse botão se você tiver concluído o procedimento [Preparando o diálogo](#deploy-intercom-dialog-prereq).
    {: important}

1.  Na seção *Configurações*, clique em **Gerenciar regras**.

    Se você não fizer nenhuma mudança, o contato de backup para todos os nós permanecerá não designado.

1.  Clique em  ** Nova regra **.

1.  Na lista suspensa *Escolher nó*, escolha o nó para a ramificação de diálogo que você deseja configurar.

    Lembre-se, as ramificações são identificadas por seu nome de nó. Se você não especificou um nome de nó, o ID do nó será exibido em seu lugar.

1.  Escolha a equipe ou o colega de equipe do agente humano para ser o contato de backup para essa ramificação de diálogo. A consulta do usuário será escalada para essa pessoa se o assistente não puder responder à consulta ou atingir um nó-filho com um tipo de resposta *Conectar-se ao agente humano*, indicando que ela deve ser respondida somente por um humano.

1.  Para definir regras de roteamento para outras ramificações de diálogo, clique em **Nova regra** novamente e repita as etapas anteriores.

    Não se esqueça de configurar uma designação para quaisquer nós raiz que tenham um tipo de resposta *Conectar-se ao agente humano* em um nó-filho em sua ramificação. Se você não transferir o nó raiz associado para uma pessoa ou uma equipe específica, um assunto sensível poderá ser transferido para a caixa de entrada *Não designado*.

1.  Depois de incluir regras, clique em **Retornar para a visão geral** para sair da página.

O vídeo de 3 minutos a seguir ilustra as etapas.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="Roteamento de intensificação baseado em tópico" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/dTwJZOqdzII" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Forneça ao assistente a permissão para monitorar e responder consultas do usuário
{: #deploy-intercom-config-action}

Quando você desejar que o assistente comece a monitorar uma caixa de entrada da Intercom e responda às mensagens por si só, ative o monitoramento.

Seu assistente observa as consultas do usuário conforme elas são registradas na Intercom. Quando o assistente está confiante de que sabe como responder a uma consulta do usuário, ele responde diretamente ao usuário (o assistente está confiante quando a intenção superior identificada tem uma pontuação de confiança de 0,75 ou superior).

Se você não desejar que o assistente responda a determinados tipos de consultas do usuário, será possível incluir regras para especificar outras ações a serem tomadas pelo assistente por ramificação de diálogo. Por exemplo, você pode desejar começar a incorporar o assistente à equipe da Intercom de forma mais conservadora, permitindo que o assistente somente sugira respostas, pois ele transfere mensagens do usuário para outros colegas de equipe para que eles respondam. Com o tempo, depois que o assistente prova a si mesmo, é possível dar a ele mais responsabilidade.

Para configurar como você deseja que o assistente manipule as ramificações de diálogo específicas, defina regras.

1.  Na página de integração da Intercom, na seção *Ativar seu assistente para monitorar uma caixa de entrada*, alterne o monitoramento para **Ativado**.

1.  Em  * Configurações *, clique em  ** Gerenciar regras **.

    Se você não definir regras, o assistente será configurado para monitorar a caixa de entrada *Não designado* e responderá automaticamente às consultas do usuário que ele estiver confiante de que pode direcionar.

1.  No campo *Monitorar caixa de entrada da Intercom*, escolha a caixa de entrada da Intercom que você deseja que o assistente monitore.

1.  Clique em **Nova regra** para definir um padrão de interação exclusivo para uma ramificação de diálogo específica.

1.  Na lista suspensa *Escolher nó*, escolha o nó para a ramificação que você deseja configurar.

    Lembre-se, as ramificações são identificadas por seu nome de nó. Se você não especificou um nome de nó, o ID do nó será exibido em seu lugar.

1.  Escolha o tipo de ação que você deseja que o assistente execute quando esse nó de diálogo for acionado. As opções de tipo de ação são estas:

    - **Não fazer nada**: o assistente não está envolvido na resposta; a mensagem do usuário permanece na caixa de entrada para ser direcionada por outra pessoa.
    - **Enviar para a equipe ou o colega de equipe**: o assistente avalia a entrada do usuário para determinar seu objetivo e, em seguida, a transfere para o colega de equipe apropriado.
    - **Sugerir para a equipe ou o colega de equipe**: o assistente fornece ao colega de equipe sugestões de como responder compartilhando notas com o agente humano por meio do aplicativo Intercom interno.

      - Se a entrada do usuário aciona uma ramificação de diálogo, significando um nó de diálogo raiz com nós-filhos que representam uma interação abrangente, o assistente indica que ele é capaz de direcionar a solicitação e se oferece para fazer isso.
      - Se a entrada do usuário aciona um nó raiz sem filhos, o assistente simplesmente compartilha a resposta programada do nó com o agente humano, mas não responde diretamente ao usuário.
      - Se a entrada aciona mais de um nó de diálogo com alta confiança, o assistente mostra ao colega de equipe humano uma lista de possíveis respostas e pede que o colega de equipe escolha a melhor resposta.

      Em todos os casos, o agente humano decide se deve deixar o assistente assumir o controle.

    - **Resposta**: o assistente responde diretamente ao usuário, sem consultar nenhum outro colega de equipe.

    O envolvimento do colega de equipe é necessário para todos os nós aos quais você designa os tipos de ação *Enviar para equipe ou colega de equipe* ou *Sugerir para equipe ou colega de equipe*. Certifique-se de voltar e incluir uma regra que designe a pessoa ou a equipe certa como o backup para esse nó de diálogo em particular.

1.  Para definir configurações de interação exclusiva para outras ramificações de diálogo, clique em **Nova regra** novamente e repita as etapas anteriores.

1.  Depois de incluir regras, clique em **Retornar para a visão geral** para sair da página.

Conforme seu diálogo mudar, você provavelmente retornará para a página de integração da Intercom para fazer mudanças incrementais nessas regras.

O vídeo de 3 minutos a seguir ilustra as etapas.

<iframe class="embed-responsive-item" id="youtubeplayer1" title="Monitoramento de caixa de entrada" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/fFKjWUfIftw" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Testando a integração
{: #deploy-intercom-try}

Para testar efetivamente a integração da Intercom de ponta a ponta, deve-se ter acesso a um aplicativo de usuário final Intercom. Você já criou ou editou uma área de trabalho da Intercom. A área de trabalho deve ter um cliente da interface com o usuário associado. Caso contrário, consulte [Apps na Intercom ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.intercom.com/help/apps-in-intercom){: new_window} para obter ajuda com a configuração de um.

Envie as consultas do usuário de teste por meio de um aplicativo cliente que esteja associado à sua área de trabalho da Intercom para ver como as mensagens são manipuladas pela Intercom. Verifique se as mensagens que devem ser respondidas pelo assistente estão gerando as respostas apropriadas e se o assistente não está respondendo a mensagens que ele não está configurado para responder.
