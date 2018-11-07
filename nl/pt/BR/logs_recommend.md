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

# Recomendações
Esta página apresenta recomendações para maneiras de melhorar seu sistema.
{: shortdesc}

![Guia recomendações](images/RecommendTop.png)

Esse recurso é apenas Beta.
{: tip}

Esse recurso está disponível apenas para usuários Premium.
{: tip}

Ao analisar as conversas que os usuários tenham tido com sua área de trabalho e levar em conta seus dados de treinamento do sistema atual e de segurança de resposta, você será apresentado com ações que você pode tomar para melhorar de forma mais fácil e eficiente sua área de trabalho.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/scMu66AvZtY" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

As recomendações são geradas todas as noites e requerem um grande volume de mensagens do usuário, por exemplo, mais de 50.
{: tip}

## Melhorar intenções existentes
Essa recomendação envolve levar frases individuais inseridas por usuários, que o sistema não reconhece, e então apresentá-las a você para selecionar uma intenção para cada frase. Isso ajudará sua área de trabalho a entender melhor o que seus usuários estão dizendo.

Clique em **Iniciar** para iniciar a identificação de intenções.
![Página Melhorar intenções existentes](images/rec_improve_intent.png)

Ao entrar ou sair de **Melhorar intenções existentes**, a barra de progresso mostra em quantas frases você agiu na sessão atual, fora do total de frases deixadas para o dia. Observe que se você sair e entrar novamente, a barra de progresso será iniciada em `empty` novamente, mas isso não significa que seu trabalho anterior foi perdido, ela apenas não considerará o progresso da sessão atual.

Selecione a melhor intenção para uma frase da lista fornecida ou selecione *Marcar como irrelevante*. As frases são incluídas nas intenções como exemplos (incluídas como dados de treinamento) assim que você clica em **Salvar**.

O botão *Ir para Próximo* permite ignorar a frase atual e mover para a próxima. A frase ignorada não será mostrada novamente se você sair e entrar de novo em **Melhorar intenções existentes** durante o mesmo dia, mas pode aparecer novamente em dias subsequentes.

![Página de edição melhorar intenções existentes](images/rec_improve_intent2.png)
