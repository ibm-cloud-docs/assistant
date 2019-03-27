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

# Processo de desenvolvimento
{: #dev-process}

Use a ferramenta {{site.data.keyword.conversationshort}} para alavancar a IA à medida que você constrói, implementa e melhora incrementalmente um assistente conversacional.
{: shortdesc}

![Mostra o fluxo de etapas de desenvolvimento, iniciando com o desenvolvimento de dados de treinamento e terminando com a implementação para produção](images/dev-process.png)

## Workflow
{: #dev-process-workflow}

O fluxo de trabalho típico para um projeto de assistente inclui as etapas a seguir:

1.  Defina um conjunto limitado de necessidades chave do cliente que você deseja que o assistente direcione em seu nome, incluindo todos os processos de negócios que ele possa iniciar ou concluir para os clientes.
1.  Crie intenções que representem as necessidades do cliente que você identificou na etapa anterior. Por exemplo, intenções como `About_company` ou `#Place_order`.

    Use o recurso de recomendação de exemplo de intenção para extrair os logs existentes da central de atendimento para localizar exemplos do usuário de intenção ideal.
    {: tip}

1.  Construa um diálogo que detecte as intenções definidas e direcione-as, seja com respostas simples ou com um fluxo de diálogo que colete mais informações primeiro.
1.  Defina qualquer entidade necessária para uma compreensão mais clara daquilo que o usuário quer dizer.

    Extraia os exemplos do usuário de intenção existentes para menções de valores de entidade comuns. O uso de anotações para definir entidades captura não somente o texto do valor de entidade, mas o contexto no qual o valor de entidade é geralmente usado em uma sentença.

    Para entidades baseadas em dicionário, use recomendações de sinônimos para expandir suas definições de entidade.
    {: tip}

1.  Teste cada função que for incluída no assistente na área de janela "Experimente", incrementalmente, conforme você avançar.
1.  Quando você tiver um assistente de trabalho que possa manipular as tarefas chave com êxito, inclua uma integração que implemente o assistente em um ambiente de desenvolvimento. Teste o assistente implementado e faça os refinamentos.

1.  Depois de construir um assistente efetivo, obtenha uma captura instantânea da qualificação de diálogo e salve-a como uma versão.

    O salvamento de uma versão quando você atinge um marco de desenvolvimento fornece algo para o qual será possível voltar se as mudanças subsequentes feitas na qualificação diminuírem sua efetividade. Consulte  [ Criando versões de qualificação ](/docs/services/assistant?topic=assistant-versions).
1.  Implemente a versão do assistente em um ambiente de teste e teste-a.

    Se você usar o widget de bate-papo hospedado pela web, será possível compartilhar a URL com outras pessoas para obter ajuda com o teste.
1.  Use as métricas da guia Analítica para localizar áreas para melhoria e fazer ajustes.

    Se for necessário testar abordagens alternativas para resolver um problema, crie uma versão para cada solução, para que seja possível implementar e testar cada uma independentemente e comparar os resultados.
1.  Quando estiver satisfeito com o desempenho do seu assistente, implemente a melhor versão dele em um ambiente de produção.
1.  Monitore os logs de conversas que os usuários têm com o assistente implementado.

    É possível visualizar os logs para uma versão de uma qualificação que está em execução na produção por meio da guia Analítica de uma versão de desenvolvimento da qualificação. Conforme você localiza classificações erradas ou outros problemas, é possível corrigi-los na versão de desenvolvimento da qualificação e, em seguida, implementar a versão melhorada para produção após o teste. Consulte [Melhoria entre os assistentes](/docs/services/assistant?topic=assistant-logs#logs-deploy-id) para obter mais detalhes.

O processo de análise dos logs e de melhoria da qualificação de diálogo está em andamento. Com o tempo, você pode desejar expandir as tarefas que o assistente pode manipular para você. As necessidades do cliente também mudam. À medida que novas necessidades surgem, as métricas geradas por seus assistentes implementados podem ajudá-lo a identificar e direcioná-las em iterações subsequentes.
