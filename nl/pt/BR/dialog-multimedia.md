---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Respostas de multimídia

Se a sua área de trabalho é integrada ao Slack ou Facebook Messenger usando o [conector do {{site.data.keyword.conversationshort}}](conversation-connector.html), é possível especificar respostas do nó de diálogo que incluem multimídia ou elementos interativos, como botões clicáveis. Para especificar uma resposta interativa, você insere um bloco de dados JSON na saída de um nó de diálogo.

**Nota:** se você deseja usar mensagens interativas com o Slack, certifique-se de ter ativado o suporte a mensagens interativas. Para obter mais informações, veja o [LEIA-ME ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/slack/README.md#interactive-messages){: new_window} da implementação do Slack.

## Formato JSON genérico

É possível especificar respostas interativas usando um formato JSON genérico que suporta as implementações do Slack ou Facebook Messenger. Para especificar uma resposta interativa no formato JSON genérico, insira o JSON no campo `output.generic` da resposta do nó de diálogo. O exemplo a seguir mostra como você pode enviar uma resposta contendo múltiplos tipos de resposta (texto, uma imagem e opções clicáveis):

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "Here are your nearest stores."
      },
      {
        "response_type": "image",
        "source": "http://...",
        "title": "Image title",
        "description": "Some description for the image"
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value:" "Location 1"
          },
          {
            "label": "Location 2",
            "value:" "Location 2"
          },
          {
            "label": "Location 3",
            "value:" "Location 3"
          }
        ]
      }
    ]
  }
}
```

Para obter mais informações sobre os tipos de resposta suportados e como especificá-los, veja [Tipos de resposta](#response-types).

No tempo de execução, o conector do {{site.data.keyword.conversationshort}} converte essa resposta para o formato esperado pelo canal (Slack ou Facebook Messenger). Se a resposta contém múltiplos tipos de mídia ou anexos, a resposta genérica é convertida em uma série de cargas úteis de mensagem separadas. O conector então envia cada carga útil da mensagem para o canal em uma mensagem separada.

**Nota:** quando uma resposta é dividida em múltiplas mensagens, o conector do {{site.data.keyword.conversationshort}} envia essas mensagens para o canal em sequência. É responsabilidade do canal entregar essas mensagens para o usuário final; isso pode ser afetado por problemas de rede ou servidor.

## Formato JSON nativo

Além do formato JSON genérico, o conector do {{site.data.keyword.conversationshort}} também suporta as respostas do canal específico escritas usando os formatos nativos do Slack e Facebook Messenger. Você pode desejar usar os formatos JSON nativos se precisar especificar um tipo de resposta que não é suportado atualmente pelo formato JSON genérico.

É possível especificar o JSON nativo para Slack ou Facebook usando o campo apropriado na resposta do nó de diálogo:

- `output.slack`: insira qualquer JSON que você deseja incluir no campo `attachment` da resposta do Slack. Para obter mais informações sobre o formato JSON do Slack, veja a [documentação ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://api.slack.com/docs/message-attachments){: new_window} do Slack.

- `output.facebook`: insira qualquer JSON que você deseja incluir no campo `message.attachment.payload` da resposta do Facebook. Para obter mais informações sobre o formato JSON do Facebook, veja a [documentação ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window} do Facebook.

## Tipos de resposta

Os tipos de resposta a seguir são suportados pelo formato JSON genérico.

### Imagem

Exibe uma imagem especificada por uma URL.

#### Campos

| Nome          | Tipo   | Descrição                        | Necessário? |
|---------------|--------|------------------------------------|-----------|
| response_type | enumeração   | `image`                            | S         |
| source        | sequência | A URL da imagem. A imagem especificada deve estar no formato .jpg, .gif ou .png. | S |
| title         | sequência | O título a ser mostrado antes da imagem.| N         |
| description   | sequência | O texto da descrição que acompanha a imagem. | N |

#### Exemplo

Este exemplo exibe uma imagem com um título e texto descritivo.

```json
{
  "response_type": "image",
  "source": "http://example.com/image.jpg",
  "title": "Example image",
  "description": "An example image returned as part of a multimedia response."
}
```

### Opção

Exibe um conjunto de botões nos quais os usuários podem clicar para escolher uma opção. O valor especificado é então enviado para a área de trabalho como entrada do usuário.

#### Campos

| Nome          | Tipo   | Descrição                         | Necessário? |
|---------------|--------|-------------------------------------|-----------|
| response_type | enumeração   | `option`                            | S         |
| title         | sequência | O título a ser mostrado antes das opções. | S       |
| description   | sequência | O texto da descrição que acompanha as opções. | N |
| options       | lista  | Uma lista de pares de chave/valor que especificam as opções dentre as quais o usuário pode escolher. | S |
| options[].label | sequência | O rótulo voltado ao usuário para a opção. | S     |
| options[].value | sequência<br/>objeto | O valor que será enviado ao serviço {{site.data.keyword.conversationshort}} se o usuário selecionar a opção. Isso pode ser uma sequência (para respostas simples) ou um objeto que contém múltiplos campos. Veja abaixo para obter exemplos. | S |

#### Exemplo

Este exemplo exibe duas opções:

- Opção 1 (rotulada 'Buy something') envia uma mensagem de sequência simples (`option_1`), que é enviada para a área de trabalho usando o campo `input.text` da mensagem.
- Opção 2 (rotulada 'Exit') envia uma mensagem complexa que inclui o texto de entrada e uma matriz de intenções. A resposta pode incluir qualquer campo que é uma parte válida de uma mensagem do {{site.data.keyword.conversationshort}}. (Para obter mais informações sobre a estrutura de entrada de mensagem, veja a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/?curl#send_message){: new_window}.)

```json
{
  "response_type": "option",
  "title": "Choose from the following options:",
  "options": [
    {
      "label": "Buy something",
      "value": "Place order"
    },
    {
      "label": "Exit",
      "value": {
        "input": {
          "text": "Exit"
        },
        "intent": [
          {
            "intent": "exit_app",
            "confidence": 1.0
          }
        ]
      }
    }
  ]
}
```

### Texto

Exibe texto.

#### Campos

| Nome          | Tipo   | Descrição        | Necessário? |
|---------------|--------|--------------------|-----------|
| response_type | enumeração   | `text`             | S         |
| text          | sequência | O texto a ser exibido. Isso pode incluir caracteres de nova linha (`\n`) e identificação de Redução de preço, se suportado pelo canal. (Qualquer formatação não suportada pelo canal é ignorada.) | S |

#### Exemplo

Este exemplo exibe uma mensagem de texto para o usuário.

```json
{
  "response_type": "text",
  "text": "This is a text response."
}
```
