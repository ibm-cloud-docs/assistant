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
{:gif: data-image-type='gif'}

# Corrigindo a entrada do usuário
{: #beta-spell-check}

Esse recurso fica disponível para uso por participantes somente no programa beta. Para descobrir como solicitar acesso, consulte [Participar do programa beta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) A IBM libera serviços, recursos e suporte ao idioma classificados como beta para sua avaliação. Esses recursos podem ficar instáveis, podem mudar frequentemente e podem ser descontinuados com breve aviso. Os recursos beta podem também não fornecer o mesmo nível de desempenho ou compatibilidade que os recursos geralmente disponíveis fornecem e não são destinados ao uso em um ambiente de produção.

Ative o recurso *verificação ortográfica* para corrigir erros de ortografia que os usuários fazem nas elocuções que eles enviam como entrada do usuário. Quando a verificação ortográfica está ativada, as palavras com erro de ortografia são corrigidas automaticamente. E são as palavras corrigidas que são usadas para avaliar a entrada. Quando uma entrada mais precisa é fornecida, o serviço pode reconhecer mais frequentemente as menções de entidade e entender a intenção do usuário.

Atualmente, essa configuração pode ser ativada somente para qualificações de diálogo no idioma inglês.
{: note}

Com a verificação ortográfica ativada, a entrada do usuário é corrigida da maneira a seguir:

- Entrada original: `letme applt for a memberdhip`
- Entrada corrigida: `let me apply for a membership`

Quando o serviço avalia se deve corrigir a ortografia de uma palavra, ele não depende de um processo de consulta de dicionário simples. Em vez disso, ele usa uma combinação de Processamento de linguagem natural e modelos probabilísticos para avaliar se um termo está, de fato, com erro de ortografia e deve ser corrigido.

## Ativando a verificação ortográfica
{: #beta-spell-check-enable}

Para ativar o recurso de verificação ortográfica, conclua as etapas a seguir:

1.  Na página Qualificações, localize sua qualificação.
1.  Clique no ícone ![abrir e fechar a lista de opções](images/kabob-beta.png) e, em seguida, escolha **Configurações**.
1.  Na página de configurações *Verificação ortográfica*, ative **Autocorreção de verificação ortográfica**.

### Testando a correção de ortografia
{: #beta-spell-check-test}

1.  Na área de janela "Experimente", envie uma elocução que inclua algumas palavras com erro de ortografia.

    Se as palavras em sua entrada estiverem com erro de ortografia, elas serão corrigidas automaticamente e um ícone ![autocorreção](images/auto-correct.png) será exibido. A elocução corrigida é sublinhada.
1.  Passe o mouse sobre a elocução sublinhada para ver o texto original.

Se houver termos com erro de ortografia que você esperava que o serviço corrigisse, mas ele não fez isso, revise as regras que o serviço usa para decidir se deve corrigir uma palavra para ver se a palavra se encaixa na categoria de palavras que o serviço intencionalmente não muda.

Para evitar a correção excessiva, o serviço não corrige a ortografia dos tipos de entrada a seguir:

- Palavras capitalizadas
- Emojis
- Entidades de local, tais como estados e endereços de rua
- Números e unidades de medida ou tempo
- Nomes próprios, como nomes comuns ou nomes de empresas
- Texto entre aspas
- Palavras que contêm caracteres especiais, tais como hifens (-), asteriscos (*) e E comercial (símbolo &)
- Palavras que *pertencem* a essa qualificação, significando palavras que têm significado implícito porque elas ocorrem em valores de entidade, sinônimos de entidade ou exemplos do usuário de intenção.

Se a palavra que não estiver corrigida não for obviamente um desses tipos de entrada, talvez valha a pena verificar se a entidade possui uma correspondência difusa ativada para ela.

#### Como a correção de ortografia está relacionada à correspondência difusa?
{: #beta-spell-check-vs-fuzzy-matching}

A correspondência difusa ajuda o serviço a reconhecer as menções de entidade baseadas em dicionário na entrada do usuário. Ela usa uma abordagem de consulta de dicionário para corresponder a uma palavra da entrada do usuário a um valor ou sinônimo de entidade existente nos dados de treinamento da qualificação. Por exemplo, se o usuário inserir `books` e seus dados de treinamento contiverem o sinônimo de entidade `book`, a correspondência difusa reconhecerá que esses dois termos significam a mesma coisa.

Quando você ativa a verificação ortográfica e a correspondência difusa, a função de correspondência difusa é executada antes da verificação ortográfica ser acionada. Se localiza um termo que pode corresponder a um valor ou sinônimo de entidade de dicionário existente, ela inclui o termo na lista de palavras que *pertencem* à qualificação e, portanto, não devem ser corrigidas. Da mesma forma, se um usuário insere uma sentença como `I want to buy a boook`, a correspondência difusa reconhece que o termo `boook` significa a mesma coisa que seu sinônimo de entidade `book` e inclui-o na lista de palavras protegidas. Como resultado, o serviço *não* corrige a ortografia de `boook`.

#### Como ele Funciona
{: #beta-spell-check-how-it-works}

Normalmente, a entrada do usuário é salva no estado em que se encontra no campo `text` do objeto `input` da mensagem. Se, e somente se a entrada do usuário é corrigida de alguma maneira, um novo campo é criado no objeto `input`, chamado `original_text`. Esse campo armazena a entrada original do usuário que inclui quaisquer palavras com erro de ortografia nele. E o texto corrigido é incluído no campo `input.text`.
