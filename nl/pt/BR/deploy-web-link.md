---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-11"

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

# Integrando com um widget de bate-papo hospedado na web
{: #deploy-web-link}

Se você não desativar o link de visualização, então o assistente estará imediatamente disponível para teste em uma página da web.
{: shortdesc}

O assistente é implementado como um widget de bate-papo integrado a uma página da web simples com a marca da IBM automaticamente. É possível testar a qualificação de diálogo que você incluiu no assistente, inserindo texto no widget de bate-papo. Também é possível compartilhar a URL da página com outras pessoas para inscrever-se na ajuda para teste e obtenção de feedback sobre o assistente.

Ao contrário de quando você testa usando a área de janela "Experimente" na ferramenta, quaisquer chamadas da API que resultam de suas interações com o assistente hospedado pelo URL do Link de visualização incorrem em encargos.

## Usando a integração do Link de visualização para testar seu assistente
{: #deploy-web-link-try}

Para testar o assistente por meio de um widget de bate-papo hospedado pela web, conclua as etapas a seguir:

1.  Na guia Assistentes, clique para abrir o tile de assistente que você deseja testar.

1.  Na seção *Integrações*, clique no tile **Link de visualização**.

    Se você não ativou o link de visualização quando criou o assistente, clique em **Incluir integração**, clique no tile de integração **Link de visualização** e, em seguida, clique em **Criar**.

1.  **Opcional**: mude o nome e a descrição da página da web de visualização.

1.  Clique no link de URL que é exibido para abrir a página de teste.

    É aberta uma guia de navegador da web separada contendo uma implementação do widget de bate-papo de seu assistente.

    Se sua instância de serviço foi criada no data center do Reino Unido antes de 13 de dezembro de 2018 ou em Sydney antes de 7 de maio de 2018, deve-se editar a URL do link de visualização. A URL inclui um código do local para o data center no qual a instância está hospedada. Como as instâncias em Londres e Sydney foram organizadas em Dallas antes das datas especificadas, deve-se substituir a referência `eu-gb` ou `au-syd` na URL por `us-south` para que a página da web seja renderizada adequadamente.
    {: important}

1.  Envie as elocuções de teste para ver como o assistente responde.

    Nenhuma resposta é retornada até que você crie uma qualificação de diálogo e inclua-a no assistente.

    O fluxo de diálogo da sessão atual é reiniciado após 60 minutos de inatividade (5 minutos para os planos Lite e Standard). Isso significa que, se um usuário parar de interagir com o assistente, após 60 (ou 5) minutos, todos os valores da variável de contexto configurados durante a conversa anterior serão configurados como nulos ou de volta para seus valores padrão.

1.  Após o teste, é possível fechar a guia do navegador para sair da página da web pública.

1.  Na ferramenta, clique em **Salvar mudanças** para salvar quaisquer edições feitas na integração de link de visualização e feche a página ou clique em **X** para fechar a página sem salvar.

## Considerações sobre Diálogo
{: #deploy-web-link-dialog}

As respostas ricas que você inclui em um diálogo são exibidas no widget de bate-papo hospedado pela web, conforme esperado, com as exceções a seguir:

- **Conectar-se ao agente humano**: esse tipo de resposta é ignorado.

- **Pausar**: esse tipo de resposta pausa a atividade do assistente no widget de bate-papo. No entanto, a atividade não continua após a pausa até que outra resposta seja acionada. Sempre que você incluir um tipo de resposta `pause`, inclua outro tipo de resposta diferente, como `text`, após ele.

Consulte [Respostas avançadas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia) para obter mais informações sobre os tipos de resposta.

## Incluindo uma Integração de Link de Visualização
{: #deploy-web-link-add-more}

Se você excluiu acidentalmente a integração de link de visualização que é criada automaticamente ou deseja criar outra página da web pública separada, é possível incluir uma integração de link de visualização.

1.  Na guia Assistente, clique para abrir o tile para o assistente que você deseja implementar.

1.  Acesse a seção **Integrações** e, em seguida, clique em **Incluir integração**.

1.  Clique no quadro de integração  ** Link de visualização ** .

1.  Opcionalmente, edite o nome e a descrição da integração de link de visualização e, em seguida, clique em **Criar**.

    Uma URL é incluída na página. Clique nela para abrir a página da web de visualização.
