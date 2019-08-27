---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# Usando catálogos de conteúdo
{: #catalog}

Os ***Catálogos de conteúdo*** fornecem uma maneira fácil de incluir intenções comuns em sua qualificação de diálogo do {{site.data.keyword.conversationshort}}.
{: shortdesc}

As intenções que você inclui do catálogo são destinadas a fornecer um ponto de início. Inclua ou edite as intenções do catálogo para customizá-las para seu caso de uso.

## Incluindo um catálogo de conteúdo em sua qualificação de diálogo
{: #catalog-add}

1.  Abra sua qualificação de diálogo e, em seguida, clique na guia **Catálogo de conteúdo**.

1.  Selecione um catálogo de conteúdo, como *Financeiro*, para ver as intenções que são fornecidas com ele.

    ![Captura de tela mostrando os catálogos disponíveis](images/catalog_overview.png)

    Você verá informações sobre as intenções que estão incluídas no catálogo.

    ![Screen capture showing Banking category intents](images/catalog_open.png)

    As intenções que são incluídas de um catálogo de conteúdo são distinguíveis de outras intenções por seus nomes. Cada nome de intenção é pré-anexado com o nome do catálogo de conteúdo.

1.  Selecione ![Seta de fechamento](images/close_arrow.png) para retornar à guia **Catálogo de conteúdo**.

1.  Em seguida, inclua um catálogo de conteúdo em sua qualificação de diálogo, clicando no botão `Add to skill`.

1.  Agora, selecione a guia **Intenções** e verifique se as intenções do catálogo foram incluídas e estão disponíveis.

    ![Captura de tela mostrando as intenções de Financeiro listadas na guia Intenções](images/catalog_intents.png)

O sistema começa a treinar-se sobre os novos dados.

Depois de incluir um catálogo em sua qualificação, as intenções tornam-se parte de seus dados de treinamento. Se a IBM faz atualizações subsequentes em um catálogo de conteúdo, as mudanças não são aplicadas automaticamente a nenhuma intenção que você incluiu de um catálogo.
{: note}

## Editando exemplos do catálogo de conteúdo
{: #catalog-edit-content}

Como qualquer outra intenção, depois de incluir as intenções do catálogo de conteúdo em sua qualificação, é possível fazer as mudanças a seguir nelas:

- Renomear a intenção.
- Excluir a intenção.
- Incluir, editar ou excluir exemplos.
- Mover um exemplo para uma intenção diferente.
