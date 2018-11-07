---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-16"

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

# オブジェクトにアクセスするための式

Spring Expression (SpEL) 言語を使用して、オブジェクトやオブジェクトのプロパティーにアクセスする式を作成できます。詳しくは、[Spring Expression Language (SpEL) ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window} を参照してください。
{: shortdesc}

## 評価構文

変数値を他の変数内で展開するか、またはプロパティーやグローバル・オブジェクトに対するメソッドを呼び出すには、`<? expression ?>` 式構文を使用します。例えば次のようにします。

- **プロパティーの展開**
    - `"output":{"text":"Your name is <? context.userName ?>"}`

- **グローバル・オブジェクトのプロパティーに対するメソッドの呼び出し**
    - `"context":{"email": "<? @email.literal ?>"}`

## 省略表現構文
{: #shorthand-syntax}

SpEL 省略表現構文を使用して以下のオブジェクトを素早く参照する方法について説明します。

- [コンテキスト変数](expression-language.html#shorthand-context)
- [エンティティー](expression-language.html#shorthand-entities)
- [インテント](expression-language.html#shorthand-intents)

### コンテキスト変数の省略表現構文
{: #shorthand-context}

条件式でコンテキスト変数を記述するために使用できる、省略表現構文の例を次の表に示します。

| 省略表現構文           | SpEL での完全構文                     |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

ハイフンやピリオドなどの特殊文字をコンテキスト変数名に含めることができます。 ただし、そうすると、SpEL 式が評価されるときに問題が発生する可能性があります。 例えば、ハイフンが負符号と解釈されることがあります。 そうした問題を回避するには、完全式の構文または省略表現構文 `$(variable-name)` のいずれかを使用して、変数を参照してください。また、以下の特殊文字を名前に使用しないでください。

- 括弧 ()
- 複数のアポストロフィ ''
- 引用符 "

### エンティティーの省略表現構文
{: #shorthand-entities}

エンティティーを参照するときに使用できる、省略表現構文の例を次の表に示します。

| 省略表現構文    | SpEL での完全構文                      |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

SpEL では、疑問符 `(?)` を使用することで、エンティティー・オブジェクトが NULL の場合に NULL ポインター例外がトリガーされないようにします。

検査するエンティティー値に `)` 文字が含まれる場合、比較のための `:` 演算子を使用できません。  例えば、city エンティティーが `Dublin (Ohio)` かどうかを検査する場合、`@city:(Dublin (Ohio))` ではなく、`@city == 'Dublin (Ohio)'` を使用する必要があります。

### インテントの省略表現構文
{: #shorthand-intents}

インテントを参照するときに使用できる、省略表現構文の例を次の表に示します。

| 省略表現構文        | SpEL での完全構文 |
|-------------------------|---------------------|
| `#help`                 | `intent == 'help'`  |
| `! #help`               | `intent != 'help'`  |
| `NOT #help`             | `intent != 'help'`  |
| `#help` または `#i_am_lost` | <code>(intent == 'help' \|\| intent == 'I_am_lost')</code> |

## 組み込みグローバル変数
{: #builtin-vars}

式言語を使用して、以下のグローバル変数に関するプロパティー情報を抽出できます。

| グローバル変数      | 定義 |
|----------------------|------------|
| *context*            | 処理される会話メッセージの JSON オブジェクト部分。 |
| *entities[ ]*        | 最初の要素へのデフォルト・アクセスをサポートするエンティティーのリスト。 |
| *input*              | 処理される会話メッセージの JSON オブジェクト部分。 |
| *intents[ ]*         | 最初の要素へのデフォルト・アクセスをサポートするインテントのリスト。 |
| *output*             | 処理される会話メッセージの JSON オブジェクト部分。 |

## エンティティーへのアクセス
{: #access-entity}

エンティティー配列には、ユーザー入力内で認識された 1 つ以上のエンティティーが含まれます。

対話のテスト中に、対話ノードの応答で以下の式を指定すると、ユーザー入力で認識されたエンティティーの詳細を参照できます。

```json
<? entities ?>
```
{: codeblock}

*こんにちは* というユーザー入力の場合、サービスは @sys-date システム・エンティティーと @sys-time システム・エンティティーを認識するため、応答には以下のエンティティー・オブジェクトが含まれます。

```json
[
{"entity":"sys-date","location":[6,9],"value":"2017-08-07",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}},
{"entity":"sys-time","location":[6,9],"value":"15:01:00",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}}
]
```
{: codeblock}

### 入力でエンティティーの配置が重要な場合

入力でエンティティーの配置が重要な場合は、完全な SpEL 式を使用してください。 `entities['city']?.contains('Boston')` という条件では、すべての @city エンティティーで、配置にかかわらず、少なくとも 1 つの「Boston」city エンティティーが検出されると true が戻されます。

例えば、ユーザーが`「Toronto から Boston に行きたい」`を送信するとします。 `@city:Toronto` エンティティーと `@city:Boston` エンティティーが検出され、以下のエンティティーで表されます。

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

対話ノードに `@city.contains('Boston')` 条件があると、Boston が 2 番目に検出されたエンティティーであっても true が戻されます。

### エンティティー・プロパティー

各エンティティーには、一連のプロパティーが関連付けられています。 エンティティーに関する情報は、そのプロパティーから取得できます。

| プロパティー              | 定義 | 使用法のヒント |
|-----------------------|------------|------------|
| *confidence*          | 認識されたエンティティーの、サービスの信頼度を表す小数で示す割合。 エンティティーのファジー・マッチングがアクティブでない場合、エンティティーの信頼度は 0 または 1 です。 ファジー・マッチングが有効な場合、信頼度レベルのデフォルトしきい値は 0.3 です。 ファジー・マッチングが有効かどうかにかかわらず、システム・エンティティーの信頼度レベルは常に 1.0 です。 | このプロパティーを条件に使用すると、信頼度レベルが指定したパーセント以下の場合に false が戻されるようにすることができます。 |
| *location*            | 入力テキストで検出されたエンティティー値の先頭と末尾を示す、ゼロを基準にした文字オフセット。 | `.literal` を使用すると、location プロパティーに格納された開始インデックス値と終了インデックス値の間の、テキストのスパンを抽出できます。 |
| *value*               | 入力で識別されたエンティティー値。 | このプロパティーによって、トレーニング・データで定義されているようにエンティティー値が戻されます。関連付けられた同義語のいずれかとマッチングした場合もそうなります。 `.values` を使用すると、ユーザー入力に存在するエンティティーの、複数のオカレンスを収集できます。 |

### エンティティー・プロパティーの使用例
以下の例では、ワークスペースに airport エンティティーが存在し、その値として JFK と、同義語「Kennedy Airport」が含まれます。 ユーザー入力は *Kennedy Aiport に行きたい*です。

- ユーザー入力で「JFK」エンティティーが認識された場合に特定の応答を返すには、次の式を応答条件に追加することができます。
  `entities.airport[0].value == 'JFK'`
  または
  `@airport = "JFK"`
- 対話の応答で、ユーザーによって指定されたエンティティー名を返すには、次のように .literal プロパティーを使用します。
  `行き先は <?entities.airport[0].literal?> ですね...`
  または
  `行き先は @airport.literal ですね...`

  どちらの形式も、応答で「行き先は Kennedy Airport ですね...」に評価されます。

- `@airport:(JFK)` または `@airport.contains('JFK')` といった式は、エンティティーの **value** (この例では `JFK`) を常に参照します。
- ファジー・マッチングが有効な場合に、入力で airport として識別する語句を制限するには、例えば次の式をノード条件に指定します。`@airport && @airport.confidence > 0.7`。 入力テキストに airport の参照が含まれるかどうかについて、サービスの信頼度が 70% の場合にのみ、ノードが実行されます。

この例では、ユーザー入力は *JFK、Logan、O'Hare に両替所はありますか?* です。

- ユーザー入力に含まれる、あるエンティティー・タイプの複数のオカレンスを収集するには、次のような構文を使用します。

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  収集されたリストを後で対話の応答で参照するには、次の構文を使用します。
  `<? $airports.join(', ') ?> の空港についてお尋ねですね。`
  表示内容は次のようになります。
  `JFK、Logan、O'Hare の空港についてお尋ねですね。`

## インテントへのアクセス
{: #access-intent}

インテント配列には、ユーザー入力で認識された 1 つ以上のインテントが、信頼度の降順でソートされて入っています。 

各インテントにはプロパティーが 1 つだけあります。それは `confidence` プロパティーです。 confidence プロパティーは、認識されたインテントの、サービスの信頼度を表す、小数で示す割合です。

対話のテスト中に、対話ノードの応答で以下の式を指定すると、ユーザー入力で認識されたインテントの詳細を参照できます。

```json
<? intents ?>
```
{: codeblock}

「*こんにちは (Hello now)*」というユーザー入力の場合、サービスは #greeting インテントとの完全一致を検索します。したがって、#greeting インテント・オブジェクトの詳細を最初にリストします。また信頼度にかかわらず、スキルに定義されているその他のインテントの上位 10 件も応答に含められます。(この例では、最初のインテントが完全一致なので、他のインテントの信頼度は 0 に設定されます。) 上位 10 件のインテントが返されるのは、「試行する (Try it out)」ペインで要求と共に `alternate_intents:true` パラメーターが送信されているためです。直接 API を使用している場合に上位 10 件の結果を表示するには、呼び出し内でこのパラメーターを指定していることを確認してください。`alternate_intents` が false (デフォルト値) の場合、信頼度が 0.2 を超えているインテントのみが配列として返されます。

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

以下の例では、インテント値の検査方法を示します。

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help'` は `intents[0] == 'help'` と異なります。なぜなら、`intent == 'help'` は、インテントが検出されない場合でも例外をスローしないためです。 インテントの信頼度がしきい値を上回る場合にのみ、true に評価されます。  必要に応じて、カスタム信頼度レベルを条件に指定できます。例えば、次のようにします。`intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`

## 入力へのアクセス
{: #access-input}

入力 JSON オブジェクトに含まれるプロパティーは 1 つだけで、それは text プロパティーです。 text プロパティーはユーザー入力のテキストを表します。

### 入力プロパティーの使用例

次の例は、入力へのアクセス方法を示しています。

- ユーザー入力が「Yes」の場合にノードを実行するには、次の式をノード条件に追加します。
  `input.text == 'Yes'`

いずれかの[ストリング・メソッド](/docs/services/conversation/dialog-methods.html#strings)を使用して、ユーザー入力のテキストを評価または操作することができます。 例えば次のようにします。

- ユーザー入力に「Yes」が含まれるかどうかを検査するには、次を使用します。`input.text.contains( 'Yes' )`。
- 次を使用すると、ユーザー入力が数値の場合に true を返します。`input.text.matches( '[0-9]+' )`。
- 入力ストリングに 10 文字含まれるかどうかを検査するには、次を使用します。`input.text.length() == 10`。
