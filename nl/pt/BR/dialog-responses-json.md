---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# Definindo respostas usando o editor JSON
{: #dialog-responses-json}

Em algumas situações, pode ser necessário definir respostas usando o editor JSON. (Para obter mais informações sobre respostas de diálogo, consulte [Respostas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)). A edição do JSON de resposta fornece acesso direto aos dados que serão retornados para o canal de comunicação ou o aplicativo customizado.

## Formato JSON genérico
{: #dialog-responses-json-generic}

O formato JSON genérico para respostas é usado para especificar respostas destinadas a qualquer canal. Esse formato pode acomodar vários tipos de resposta que são suportados por integrações do Slack e do Facebook e também pode ser implementado por um aplicativo cliente customizado. (Esse é o formato usado por padrão para respostas de diálogo definidas usando a ferramenta {{site.data.keyword.conversationshort}}.)

Para obter informações sobre como abrir o editor JSON para uma resposta do nó de diálogo por meio da ferramenta, consulte [Variáveis de contexto no editor JSON](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-json).

Para especificar uma resposta interativa no formato JSON genérico, insira os objetos JSON apropriados no campo `output.generic` da resposta do nó de diálogo. O exemplo a seguir mostra como você pode enviar uma resposta contendo múltiplos tipos de resposta (texto, uma imagem e opções clicáveis):

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          {
            "text": "Here are your nearest stores."
          }
        ]
      },
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Example image",
        "description": "Some description for the image."
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value": {
              "input": {
                "text": "Location 1"
              }
            }
          },
          {
            "label": "Location 2",
            "value": {
              "input": {
                "text": "Location 2"
              }
            }
          },
          {
            "label": "Location 3",
            "value": {
              "input": {
                "text": "Location 3"
              }
            }
          }
        ]
      }
    ]
  }
}
```

Para obter mais informações sobre como especificar cada tipo de resposta suportado usando objetos JSON, consulte [Tipos de resposta](#dialog-responses-json-response-types).

Se você estiver usando o conector do {{site.data.keyword.conversationshort}}, a resposta será convertida no tempo de execução para o formato esperado pelo canal (Slack ou Facebook Messenger). Se a resposta contiver múltiplos tipos de mídia ou anexos, a resposta genérica será convertida em uma série de cargas úteis de mensagem separadas, conforme necessário. O conector então envia cada carga útil da mensagem para o canal em uma mensagem separada.

**Nota:** quando uma resposta é dividida em múltiplas mensagens, o conector do {{site.data.keyword.conversationshort}} envia essas mensagens para o canal em sequência. É responsabilidade do canal entregar essas mensagens para o usuário final; isso pode ser afetado por problemas de rede ou servidor.

Se você estiver construindo seu próprio aplicativo cliente, seu app deverá implementar cada tipo de resposta conforme apropriado. Para obter mais informações, consulte  [ Implementando respostas ](/docs/services/assistant?topic=assistant-api-dialog-responses).

## Formato JSON nativo
{: #dialog-responses-json-native}

Além do formato JSON genérico, o JSON do nó de diálogo também suporta respostas específicas do canal gravadas usando os formatos nativos do Slack e do Facebook Messenger. Esses formatos também são suportados pelo conector do {{site.data.keyword.conversationshort}}. Você poderá desejar usar os formatos JSON nativos se souber que sua área de trabalho será integrada a somente um tipo de canal e precisar especificar um tipo de resposta que não é suportado atualmente pelo formato JSON genérico.

É possível especificar o JSON nativo para Slack ou Facebook usando o campo apropriado na resposta do nó de diálogo:

- `output.slack`: insira qualquer JSON que você deseja que seja incluído no campo `attachment` da resposta do Slack. Para obter mais informações sobre o formato JSON do Slack, veja a [documentação ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://api.slack.com/docs/message-attachments){: new_window} do Slack.

- `output.facebook`: insira qualquer JSON que você deseja incluir no campo `message.attachment.payload` da resposta do Facebook. Para obter mais informações sobre o formato JSON do Facebook, veja a [documentação ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window} do Facebook.

## Tipos de resposta
{: #dialog-responses-json-response-types}

Os tipos de resposta a seguir são suportados pelo formato JSON genérico.

### Imagem
{: #dialog-responses-json-image}

Exibe uma imagem especificada por uma URL.

#### Campos
{: #{: #dialog-responses-json-image-fields}

| Nome          | Tipo   | Descrição                        | Necessário? |
|---------------|--------|------------------------------------|-----------|
| response_type | enumeração   | `image`                            | S         |
| source        | sequência | A URL da imagem. A imagem especificada deve estar no formato .jpg, .gif ou .png. | S |
| title         | sequência | O título a ser mostrado antes da imagem.| N         |
| description   | sequência | O texto da descrição que acompanha a imagem. | N |

#### Exemplo
{: #dialog-responses-json-image-example}

Este exemplo exibe uma imagem com um título e texto descritivo.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Example image",
        "description": "An example image returned as part of a multimedia response."
      }
    ]
  }
}
```

### Opção
{: #dialog-responses-json-option}

Exibe um conjunto de botões ou uma lista suspensa que os usuários podem usar para escolher uma opção. O valor especificado é então enviado para a área de trabalho como entrada do usuário.

#### Campos
{: #dialog-responses-json-option-fields}

| Nome          | Tipo   | Descrição                         | Necessário? |
|---------------|--------|-------------------------------------|-----------|
| response_type | enumeração   | `option`                            | S         |
| title         | sequência | O título a ser mostrado antes das opções. | S       |
| description   | sequência | O texto da descrição que acompanha as opções. | N |
| preferência    | enumeração   | O tipo preferencial de controle a ser exibido, se suportado pelo canal (`dropdown` ou `button`). O conector do  {{site.data.keyword.conversationshort}}  suporta atualmente apenas  ` botão `.| N |
| options       | lista   | Uma lista de pares de chave/valor que especificam as opções dentre as quais o usuário pode escolher. | S |
| options[].label | sequência | O rótulo voltado ao usuário para a opção. | S     |
| options[].value | objeto | Um objeto que define a resposta que será enviada para o serviço {{site.data.keyword.conversationshort}} se o usuário selecionar a opção. | S |
| options [ ].value.input | objeto | Um objeto de entrada que inclui o texto de entrada correspondente à opção. | N |
| options[].value.input.text | sequência | O texto que será enviado ao seu assistente para a opção. | N |

#### Exemplo
{: #dialog-responses-json-option-example}

Este exemplo exibe duas opções:

- A opção 1 (rotulada `Buy something`) envia uma mensagem de sequência simples (`Place order`), que é enviada para a área de trabalho como o texto de entrada.
- A opção 2 (rotulada `Exit`) envia uma mensagem complexa que inclui o texto de entrada e uma matriz de intenções. A resposta pode incluir qualquer campo que é uma parte válida de uma mensagem do {{site.data.keyword.conversationshort}}. (Para obter mais informações sobre a estrutura de entrada de mensagem, veja a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.)

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "Choose from the following options:",
        "preference": "button",
        "options": [
          {
            "label": "Buy something",
            "value": {
              "input": {
                "text": "Place order"
              }
            }
          },
          {
            "label": "Exit",
      "value": {
              "input": {
                "text": "Exit"
              },
              "intents": [
                {
                  "intent": "exit_app",
            "confidence": 1.0
          }
              ]
            }
          }
        ]
      }
    ]
  }
}
```

### Pausar
{: #dialog-responses-json-pause}

Pausa antes de enviar a próxima mensagem para o canal e, opcionalmente, envia um evento "user is typing" (para canais que o suportam).

#### Campos
{: #dialog-responses-json-pause-fields}

| Nome          | Tipo   | Descrição        | Necessário? |
|---------------|--------|--------------------|-----------|
| response_type | enumeração   | `pause`            | S         |
| horário          | int    | Quanto tempo para pausar, em milissegundos. | S |
| digitando        | booleano | Se o evento "user is typing" deve ser enviado durante a pausa. Ignorado se o canal não suportar esse evento. | N |

#### Exemplo
{: #dialog-responses-json-pause-example}

Este exemplo envia o evento "user is typing" ao pausar por 5 segundos.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 5000,
        "typing": true
      }
    ]
  }
}
```

### Texto
{: #dialog-responses-json-text}

Exibe texto. Para incluir variedade, é possível especificar múltiplas respostas de texto alternativo. Se você especificar múltiplas respostas, será possível escolher girar sequencialmente na lista, escolher uma resposta aleatoriamente ou gerar todas as respostas especificadas.

#### Campos
{: #dialog-responses-json-text-fields}

| Nome          | Tipo   | Descrição        | Necessário? |
|---------------|--------|--------------------|-----------|
| response_type | enumeração   | `text`             | S         |
| valores        | lista   | Uma lista de um ou mais objetos que definem a resposta de texto. | S |
| .[_ n _ ] .text   | sequência | O texto de uma resposta. Isso pode incluir caracteres de nova linha (`\n`) e identificação de Redução de preço, se suportado pelo canal. (Qualquer formatação não suportada pelo canal é ignorada.) | N |
| selection_policy | sequência | Como uma resposta será selecionada na lista, se mais de uma resposta for especificada. Os valores possíveis são `sequential`, `random` e `multiline`. | N |
| delimitador     | sequência | O delimitador para saída como um separador entre as respostas. Usado somente quando  ` selection_policy ` = ` multiline `. O delimitador padrão é nova linha (`\n`). | N |

#### Exemplo
{: #dialog-responses-json-text-example}

Este exemplo exibe uma mensagem de saudação para o usuário.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          { },
          { "text": "Hi lá." }
        ],
        "selection_policy": "random"
      }
    ]
  }
}  
```
