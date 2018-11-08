---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"

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

# Usando catálogos

Os ***Catálogos*** fornecem uma maneira fácil de incluir intenções comuns em sua área de trabalho do serviço {{site.data.keyword.conversationshort}}.
{: shortdesc}

## Incluindo um catálogo em sua área de trabalho
{: #add-catalog}

Use a ferramenta {{site.data.keyword.conversationshort}} para incluir catálogos.

1.  Na ferramenta {{site.data.keyword.conversationshort}}, abra a sua área de trabalho e, em seguida, selecione a guia **Catálogo** na barra de navegação. Se **Catálogo** não estiver visível, use o menu ![Menu](images/Menu_16.png) para abrir a página.

1.  Selecione um catálogo, como *Faturamento*, para ver as intenções que são fornecidas com ele.

    ![Captura de tela mostrando os catálogos disponíveis](images/catalog_overview.png)

    Você verá informações sobre as intenções que estão definidas na categoria *Faturamento*.

    ![Captura de tela mostrando as intenções da categoria Faturamento](images/catalog_open.png)

    As intenções que são incluídas de um catálogo são distinguíveis de outras intenções pela convenção de nomenclatura; nesse caso, `#Billing_. . .`

1.  Selecione ![Seta de fechamento](images/close_arrow.png) para retornar à guia **Catálogo**.

1.  Em seguida, inclua o catálogo *Faturamento* em sua área de trabalho clicando no botão `Add to Bot`. Você verá uma mensagem indicando que as intenções de *Faturamento* foram incluídas em sua área de trabalho.

    ![Captura de tela mostrando o botão Add to Bot](images/catalog_addtobot.png)

1.  Agora, selecione a guia **Intenções** e verifique se as intenções de *Faturamento* foram incluídas em sua área de trabalho.

    ![Captura de tela mostrando as intenções de Faturamento listadas na guia Intenções](images/catalog_intents.png)

### Resultados

Os intentos do catálogo *Faturamento* foram incluídas na guia **Intenções** de sua área de trabalho e o sistema inicia o próprio treinamento com os novos dados.

## Editando exemplos do catálogo

Como qualquer outra intenção, uma vez que as intenções do catálogo *Faturamento* foram incluídas em sua área de trabalho, é possível fazer as mudanças a seguir:

- Renomear a intenção.
- Excluir a intenção.
- Incluir, editar ou excluir exemplos.
- Mover um exemplo para uma intenção diferente.

É possível usar a tecla tab do nome da intenção para cada exemplo, editando os exemplos, se você escolher.

Para mover ou excluir um exemplo, selecione o exemplo marcando a caixa de seleção e, em seguida, selecione **Mover** ou **Excluir**.

  ![Captura de tela mostrando como mover ou excluir um exemplo](images/catalog_edit.png)
