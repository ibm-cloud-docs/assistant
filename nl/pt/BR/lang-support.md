---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-07"

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

# Idiomas suportados
O serviço {{site.data.keyword.conversationshort}} suporta os idiomas listados. Recursos individuais do serviço são suportados em uma extensão maior ou menor para cada idioma.
{: shortdesc}

Na tabela a seguir, o nível de suporte a recurso e idioma é indicado por estes códigos:

- **GA** - O recurso é geralmente disponibilizado e suportado para este idioma. Observe que os recursos podem continuar sendo atualizados mesmo após a disponibilização.
- **Beta** - O recurso é suportado apenas como uma liberação Beta e ainda está passando por testes antes que seja disponibilizado nesse idioma.
- **Em Branco** - Ausência de qualquer código indica que um recurso não está disponível nesse idioma.

|                  | **[Definindo intenções](intents.html)**, **[entidades](entities.html)** e **[diálogo](dialog-build.html)** | **[Pontuação absoluta e 'Marcar como irrelevante'](intents.html#mark-irrelevant)** | **Entidades do sistema([número](system-entities.html#sys-number), [moeda](system-entities.html#sys-currency), [porcentagem](system-entities.html#sys-percentage), [data, hora](system-entities.html#sys-datetime))** | **[Correspondência difusa da entidade](entities.html#fuzzy-matching)** |
|:---|:---:|:---:|:---:|:---:|
| **Inglês (en)**                   | disponibilidade geral | disponibilidade geral | disponibilidade geral </br> Beta ([local](system-entities.html#sys-location), [pessoa](system-entities.html#sys-person)) | Beta (Stemming, erro de ortografia e correspondência parcial) |
| **Árabe (ar)**                    | disponibilidade geral | Beta | Beta | Beta (Apenas erro de ortografia) |
| **Chinês (Simplificado) (zh-cn)**   | Beta | Beta | Beta |  |
| **Chinês (Tradicional) (zh-tw)**  | Beta | Beta |  |  |
| **Tcheco (cs)**                     | Beta | Beta | Beta | Beta (Apenas erro de ortografia) |
| **Holandês (nl)**                     | Beta | Beta | Beta |  |
| **Francês (fr)**                    | disponibilidade geral | disponibilidade geral | disponibilidade geral | Beta (Apenas erro de ortografia) |
| **Alemão (de)**                    | disponibilidade geral | disponibilidade geral | disponibilidade geral | Beta (Apenas erro de ortografia) |
| **Italiano (it)**                   | disponibilidade geral | disponibilidade geral | disponibilidade geral | Beta (Apenas erro de ortografia) |
| **Japonês (ja)**                  | disponibilidade geral | disponibilidade geral | disponibilidade geral | Beta (Apenas erro de ortografia) |
| **Coreano (ko)**                    | disponibilidade geral | disponibilidade geral | disponibilidade geral | Beta (Apenas erro de ortografia) |
| **Português (do Brasil) (pt-br)** | disponibilidade geral | disponibilidade geral | disponibilidade geral | Beta (Apenas erro de ortografia) |
| **Espanhol (es)**                   | disponibilidade geral | disponibilidade geral | disponibilidade geral | Beta (Apenas erro de ortografia) ||

**Nota:** O serviço do {{site.data.keyword.conversationshort}} suporta vários idiomas, conforme observado, mas a interface do conjunto de ferramentas em si (descrições, rótulos, etc.) está em inglês. Todos os idiomas suportados podem ser inseridos e treinados por meio da interface em inglês.

**Conformidade GB18030**: GB18030 é um padrão chinês que especifica uma página de códigos estendida para uso no mercado chinês. Esse padrão de página de códigos é importante para a indústria de software porque o Comitê técnico de normatização de tecnologia de informações da China tem exigido que qualquer aplicativo de software que for liberado para o mercado chinês após 1 de setembro de 2001 seja ativado para GB18030. O serviço {{site.data.keyword.conversationshort}} suporta essa codificação e é compatível com GB18030 certificado

## Mudando um idioma da área de trabalho

Quando uma área de trabalho for criada, seu idioma não poderá ser modificado. Se for necessário mudar o idioma suportado de uma área de trabalho, o usuário deverá fazer download da área de trabalho. Em seguida, editar o arquivo JSON resultante em um editor de texto, procurando uma propriedade JSON chamada `language`.

A propriedade `language` deve ser configurada com o idioma original da área de trabalho; por exemplo, inglês seria `en`. Modifique o valor dessa propriedade, mudando-a para o idioma desejado (`fr` para francês, `de` para alemão, etc.). Salve as mudanças no arquivo JSON e importe o arquivo modificado para sua instância de serviço do {{site.data.keyword.conversationshort}}.

## Configurando idiomas bidirecionais
{: #configuring-bi-directional}

Para idiomas bidirecionais, por exemplo, árabe, é possível mudar as preferências da área de trabalho adequadamente. Na guia da área de trabalho, selecione o menu suspenso *Ações* e selecione **Preferências Bidirecionais** (essa opção só está disponível para áreas de trabalho configuradas para um idioma bidirecional):

![Preferências bidirecionais](images/bidi_prefs.png)

Selecione dentre as opções a seguir para sua área de trabalho:

- **Direção da GUI**: especifica a direção do layout de elementos, como botões ou menus, na interface gráfica com o usuário. Escolha `LTR` (esquerda para a direita) ou `RTL` (direita para a esquerda). Se não especificado, a ferramenta seguirá a configuração da direção da GUI do navegador da web.
- **Direção do Texto**: especifica a direção do texto digitado. Escolha `LTR` (esquerda para a direita) ou `RTL` (direita para a esquerda) ou selecione `Auto`, que escolherá automaticamente a direção do texto com base nas configurações de seu sistema. A opção `None` exibirá o texto da esquerda para a direita.
- **Formato Numérico**: especifica qual formato de números usar ao apresentar dígitos regulares. Escolha entre `Nominal`, `Arabic-Indic` ou `Arabic-European`. A opção `None` exibirá números ocidentais.
- **Tipo de Calendário**: especifica como você escolhe as datas de filtragem na IU da área de trabalho. Escolha `Islamic-Civil`, `Islamic-Tabular`, `Islamic-Umm al-Qura` ou `Gregorian`. **Nota**: essa configuração não se aplica ao painel "Experimente".

![Opções bidirecionais](images/bidi_opts.png)

Ao terminar de fazer suas seleções, clique em **Atualizar** para salvar e retornar para a guia da área de trabalho.

## Trabalhando com caracteres acentuados
{: #working-with-accents}

Em uma configuração de conversa, os usuários podem ou não usar acentos enquanto interagem com o serviço {{site.data.keyword.conversationshort}}. Dessa forma, ambas as versões, acentuadas e não acentuadas, de palavras podem ser tratadas da mesma forma para detecção de intenção e reconhecimento de entidade.

No entanto, para alguns idiomas, como o espanhol, alguns acentos podem alterar o significado da entidade. Dessa forma, para a detecção de entidade, embora a entidade original possa ter implicitamente um acento, o serviço também poderá corresponder à versão não acentuada da mesma entidade, mas com uma pontuação de confiança ligeiramente mais baixa.

Por exemplo, para a palavra "barrió", que tem um acento e corresponde ao passado do verbo "barrer" (varrer), o serviço também pode corresponder à palavra "barrio" (vizinhança), mas com uma confiança ligeiramente mais baixa.

O sistema fornecerá a maior pontuação de confiança em entidades com correspondências exatas. Por exemplo, `barrio` não será detectado se `barrió` estiver no conjunto de treinamento, e `barrió` não será detectado se `barrio` estiver no conjunto de treinamento.

Você deverá treinar o sistema com os caracteres e acentos apropriados. Por exemplo, se você estiver esperando `barrió` como uma resposta, coloque `barrió` no conjunto de treinamento.

Embora não seja um acento, o mesmo se aplica às palavras que usam, por exemplo, a letra espanhola `ñ` vs. a letra `n`, como "uña" vs. "una". Nesse caso, a letra `ñ` não é apenas um `n` com um acento; ela é uma letra exclusiva, específica do espanhol.

Se você achar que os seus clientes não usarão os acentos apropriados ou usarão palavras erradas (incluindo, por exemplo, a colocação de um `n` em vez de um `ñ`), ative a correspondência difusa ou inclua-os explicitamente nos exemplos de treinamento.
