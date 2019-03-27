---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-12"

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

# Tutorial: Entendendo as digressões
{: #tutorial-digressions}

Neste tutorial, você verá em primeira mão como as digressões funcionam.
{: shortdesc}

## Objetivos do aprendizado
{: #tutorial-digressions-objectives}

Quando concluir o tutorial, você entenderá como:

- as digressões são projetadas para funcionar
- as configurações de digressão afetam o fluxo do diálogo
- testar as configurações de digressão para um diálogo

### Duração
{: #tutorial-digressions-duration}

Este tutorial levará aproximadamente 20 minutos para ser concluído.

### Pré-requisito
{: #tutorial-digressions-prereqs}

Se você não tiver uma instância do {{site.data.keyword.conversationshort}}, conclua a etapa **Antes de iniciar** do [Tutorial de Introdução](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites) para criar uma.

## Etapa 1: importar a qualificação de diálogo do showcase de Digressões
{: #tutorial-digressions-import-json}

Primeiro, você precisará importar a qualificação de diálogo *Showcase de digressão* em sua instância do {{site.data.keyword.conversationshort}}.

1.  Faça download do arquivo  [ digression-showcase.json ](https://github.com/watson-developer-cloud/community/raw/master/watson-assistant/digression-showcase.json) .
1.  Em sua instância do {{site.data.keyword.conversationshort}}, clique no ícone ![Importar](images/workspace_import.png).
1.  Clique em **Escolher um arquivo** e, em seguida, selecione o arquivo **digression-showcase.json** transferido por download anteriormente.
1.  Clique em **Importar** para concluir a importação da qualificação de diálogo.

## Etapa 2: digressionando temporariamente do diálogo
{: #tutorial-digressions-temporarily-digress-away}

As digressões permitem que o usuário se afaste de uma ramificação de diálogo para mudar temporariamente o tópico antes de retornar ao fluxo de diálogo original. Nesta etapa, você começará a fazer uma reserva de restaurante e, em seguida, digressionará para perguntar as horas do restaurante. Depois de fornecer as informações de horas de abertura, o serviço retornará para o fluxo de diálogo de reserva do restaurante.

1.  Clique em **Diálogo** para alternar da página com intenções para uma visualização da árvore de diálogo.

1.  Clique no ícone ![Tente](images/ask_watson.png) para abrir a área de janela "Experimente".
1.  Digite `Book me a restaurant` no campo de texto.

    O serviço responde com um prompt para o dia a ser reservado, `When do you want to go?`

1.  Clique no ícone **Local** ![Local](images/location.png) próximo da resposta para destacar o nó que acionou a resposta, o nó **Reserva de restaurante**, na árvore de diálogo.

    ![Mostra o nó Reserva de restaurante destacado e o diálogo em andamento na área de janela Experimente.](images/tut-dig-location.png)
1.  Digite `Tomorrow`.

    O serviço responde com um prompt para o horário a ser reservado, `What time do you want to go?`

1.  Você não sabe quando o restaurante fecha, portanto, pergunta `What time do you close?`

    O robô digressiona do nó de reserva do restaurante para processar o nó **Horas de abertura do restaurante**. Ele responde com `The restaurant is open from 8:00 AM to 10:00 PM.` Em seguida, o serviço retorna ao nó de reserva de restaurante e solicita novamente o horário de reserva.

    ![Mostra a digressão acontecendo na área de janela Experimente.](images/tut-dig-digression.png)
1.  **Opcional**: para concluir o fluxo de diálogo, digite `8pm` para o horário de reserva e `2` para o número de convidados.

Parabéns! Você digressionou com êxito e retornou para um fluxo de diálogo.

## Etapa 3: desativando as digressões do intervalo
{: #tutorial-digressions-disable-slot}

Nesta etapa, você editará a configuração de digressão para o nó de reserva de restaurante para evitar que os usuários digressionem dele e verá como a mudança de configuração afeta o fluxo de diálogo.

1.  Vamos ver as configurações de digressão atuais para o nó **Reserva de restaurante**. Clique no nó para abri-lo na visualização de edição.

1.  Clique em **Customizar** e, em seguida, clique na guia **Digressões**.

    ![Mostra as configurações de digressão para o nó Reserva de restaurante.](images/tut-dig-resto-settings.png)

1.  Mude a alternância **Permitir digressões** de ativado para **desativado** e, em seguida, clique em **Aplicar**.

1.  Clique em ![Fechar](images/close.png) para fechar a visualização de edição de nó.

1.  Clique em **Limpar** na área de janela "Experimente" para reconfigurar o diálogo.

1.  Digite  ` Book me a restaurante `.

    O serviço responde com um prompt para o dia a ser reservado, `When do you want to go?`

1.  Digite `Tomorrow`.

    O serviço responde com um prompt para o horário a ser reservado, `What time do you want to go?`

1.  Perguntar,  ` Que horas você fecha? `

    O serviço reconhece que a pergunta aciona a intenção #restaurant_opening_hours, mas a ignora e exibe o prompt associado ao intervalo @sys-time novamente.

Você impediu com êxito o usuário de digressionar do processo de reserva de restaurante.

## Etapa 4: digressionando para um nó que não retorna
{: #tutorial-digressions-digress-without-return}

É possível configurar um nó de diálogo para não voltar para o nó do qual o serviço digressionou para que o nó atual seja processado. Para demonstrar isso, você mudará a configuração de digressão para o nó de horas do restaurante. Na Etapa 2, você viu que, depois de digressionar do nó de reserva do restaurante para acessar o nó de horas de abertura do restaurante, o serviço retornou ao nó de reserva do restaurante para continuar com o processo de reserva. Nesse exercício, depois de mudar a configuração, você digressionará do diálogo **Oportunidades de trabalho** para perguntar sobre as horas de abertura do restaurante e ver que o serviço não retorna para onde ele parou.

1.  Clique para abrir o nó **Horas de abertura do restaurante** .

1.  Clique em **Customizar** e, em seguida, clique na guia **Digressões**.

1.  Expanda a seção **As digressões podem vir para este nó** e desmarque a caixa de seleção **Retornar após a digressão**. Clique em **Aplicar** e, em seguida, clique em ![Fechar](images/close.png) para fechar a visualização de edição do nó.

1.  Clique em **Limpar** na área de janela "Experimente" para reconfigurar o diálogo.

1.  Para encaixar o nó de diálogo **Oportunidades de trabalho**, digite `I'm looking for a job`.

    O serviço responde dizendo `We are always looking for talented people to add to our team. What type of job are you interested in?`

1.  Em vez de responder essa pergunta, faça ao robô uma pergunta não relacionada. Digite  ` Que horas você abre? `

    O serviço digressiona do nó de Oportunidades de trabalho para o nó Horas de abertura do restaurante para responder à sua pergunta. O serviço responde com `The restaurant is open from 8:00 AM to 10:00 PM.`

    Diferentemente do teste anterior, desta vez o diálogo não assimila onde ele parou no nó **Oportunidades de trabalho**. O serviço não retorna ao diálogo que estava em andamento porque você mudou a configuração no nó **Horas de abertura do restaurante** para não retornar.

    ![Mostra uma conversa que não retorna após uma digressão](images/tut-dig-noreturn.png)

Parabéns! Você digressionou com êxito de um diálogo sem retornar.

## Resumo
{: #tutorial-digressions-summary}

Neste tutorial, você experimentou como as digressões funcionam e viu como as configurações individuais do nó de diálogo podem afetar o comportamento de digressões.

## Próximas etapas
{: #tutorial-digressions-next-steps}

Para obter ajuda quando você configura digressões para seu próprio diálogo, consulte [Digressões](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions).
