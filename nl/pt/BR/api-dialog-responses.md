---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# Implementando Respostas
{: #dialog-api-responses}

Um nó de diálogo pode responder aos usuários com uma resposta que inclua texto, imagens ou elementos interativos, tais como opções clicáveis. Se você está construindo seu próprio aplicativo cliente, deve-se implementar a exibição correta de todos os tipos de resposta retornados por seu diálogo. (Para obter mais informações sobre respostas de diálogo, consulte [Respostas](/docs/services/assistant?topic=assistant-dialog-overview#responses)).

## Formato da saída de resposta
{: #dialog-api-responses-output}

Por padrão, as respostas de um nó de diálogo são especificadas no objeto `output.generic` no JSON de resposta retornado da API /message. O objeto `generic` contém uma matriz de até 5 elementos de resposta que são destinados a qualquer canal. O exemplo de JSON a seguir mostra uma resposta que inclui texto e uma imagem:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "OK, here's a picture of a dog."
      },
      {
        "response_type": "image",
        "source": "http://example.com/dog.jpg"
      }
    ],
    "text" : ["OK, here's a picture of a dog."]
  }
}
```

**Nota:** como esse exemplo mostra, a resposta de texto (`OK, here's a picture of a dog.`) também é retornada na matriz `output.text`. Isso é incluído para compatibilidade com versões anteriores para aplicativos que não suportam o formato `output.generic`.

É responsabilidade do seu aplicativo cliente manipular todos os tipos de resposta apropriadamente. Nesse caso, seu aplicativo precisaria exibir o texto e a imagem especificados para o usuário.

## Tipos de resposta
{: #dialog-api-responses-types}

Cada elemento de uma resposta é de um dos tipos de resposta suportados (atualmente `image`, `option`, `pause` e `text`). Cada tipo de resposta é especificado usando um conjunto diferente de propriedades JSON, portanto, as propriedades incluídas para cada resposta variarão dependendo do tipo de resposta. Para obter informações completas sobre o modelo de resposta da API /message, consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.

Esta seção descreve os tipos de resposta disponíveis e como eles são representados no JSON de resposta da API /message. (Se você estiver usando o Watson SDK, será possível usar as interfaces fornecidas para seu idioma para acessar os mesmos objetos.)

**Nota:** os exemplos nesta seção mostram o formato dos dados JSON retornados da API /message no tempo de execução. Tenha em mente que isso é diferente do formato JSON usado para definir respostas dentro de um nó de diálogo. Para obter mais informações, consulte [Definindo respostas usando o editor JSON](/docs/services/assistant?topic=assistant-dialog-responses-json).

### Imagem
{: #dialog-api-responses-image}

O tipo de resposta `image` instrui o aplicativo cliente a exibir uma imagem, acompanhada opcionalmente por um título e descrição:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Image example",
        "description": "This is an example image"
      }
    ]
  }
}
```

Seu aplicativo é responsável por recuperar a imagem especificada pela propriedade `source` e exibi-la para o usuário. Se o `title` e ` description` opcionais forem fornecidos, seu aplicativo poderá exibi-los de qualquer maneira que seja apropriada (por exemplo, renderizando o título abaixo da imagem e a descrição como texto de ajuda instantânea).

### Opção
{: #dialog-api-responses-option}

O tipo de resposta `option` instrui o aplicativo cliente a exibir um controle de interface com o usuário que permite que o usuário selecione dentre uma lista de opções e, em seguida, envie a entrada de volta para o serviço {{site.data.keyword.conversationshort}} com base na opção selecionada:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "Available options",
        "description": "Please select one of the following options:",
        "preference": "button",
        "options": [
          {
            "label": "Option 1",
            "value": {
              "input": {
                "text": "option 1"
              }
            }
          },
          {
            "label": "Option 2",
            "value": {
              "input": {
                "text": "option 2"
              }
            }
          }
        ]
      }
    ]
  }
}
```

Seu app pode exibir as opções especificadas usando qualquer controle apropriado da interface com o usuário (por exemplo, um conjunto de botões ou uma lista suspensa). A propriedade `preference` opcional indica o tipo preferencial de controle que seu app deve usar (`button` ou `dropdown`), se suportado.

Para cada opção, a propriedade `label` especifica o texto do rótulo que deve aparecer para a opção no controle de IU. A propriedade `value` especifica a entrada que deve ser enviada de volta para o serviço {{site.data.keyword.conversationshort}} (usando a API /message) quando o usuário seleciona a opção correspondente. Observe que além da entrada de texto, a propriedade `value` pode incluir também outros objetos de entrada, como intenções e entidades, todos os quais devem ser enviados para o serviço.

### Pausar
{: #dialog-api-responses-pause}

O tipo de resposta `pause` instrui o aplicativo a aguardar por um intervalo especificado antes de exibir a próxima resposta:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 500,
        "typing": false
      }
    ]
  }
}
```

Essa pausa pode ser solicitada pelo diálogo para permitir tempo para que uma solicitação seja concluída ou simplesmente para imitar a aparência de um agente humano que possa pausar entre as respostas. A pausa pode ser de qualquer duração até 10 segundos.

Uma resposta `pause` é geralmente enviada em combinação com outras respostas. Seu aplicativo deve pausar pelo intervalo especificado pela propriedade `time` (em milissegundos) antes de exibir a próxima resposta na matriz. A propriedade `typing` opcional solicita que o aplicativo cliente mostre um indicador "user is typing", se suportado, para simular um agente humano.

### Texto
{: #dialog-api-responses-text}

O tipo de resposta `text` é usado para respostas de texto ordinário por meio do diálogo:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "OK, you want to fly to Boston next Monday."
      }
    ]
  }
}
```

Observe que, para compatibilidade com versões anteriores, o mesmo texto também é incluído na matriz `output.text` na resposta do diálogo (conforme mostrado no exemplo no início desta página).
