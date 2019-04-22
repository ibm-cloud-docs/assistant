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

# 式言語のメソッド
{: #dialog-methods}

ユーザー発話から抽出した値を処理して、応答の中のコンテキスト変数や条件などで参照することができます。
{: shortdesc}

## 評価構文
{: #dialog-methods-evaluation-syntax}

他の変数内の変数値を拡張したり、メソッドを出力テキストやコンテキスト変数に適用したりするには、 `<? expression ?>` 式構文を使用します。 例えば、次のようにします。

- **数値プロパティーの増分**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **オブジェクトでのメソッドの呼び出し**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

以下の各セクションで、値を処理するために使用できるメソッドについて説明します。 メソッドはデータ・タイプ別にまとめています。

- [配列](#dialog-methods-arrays)
- [日付と時刻](#dialog-methods-date-time)
- [数値](#dialog-methods-numbers)
- [オブジェクト](#dialog-methods-objects)
- [ストリング](#dialog-methods-strings)

## 配列
{: #dialog-methods-arrays}

これらのメソッドを使用して、配列値を設定するその同じノード内のノード条件または応答条件に含まれる配列の値を調べることはできません。

### JSONArray.append(オブジェクト)

このメソッドは、JSONArray に新しい値を付加し、変更後の JSONArray を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

適用する更新:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

結果:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: codeblock}

### JSONArray.clear()

このメソッドは、配列のすべての値をクリアしてヌルを返します。

出力で以下の式を使用して、値のコンテキスト変数 ($toppings_array) に保存した配列をクリアするフィールドを定義します。

```json
{
  "output": {
    "array_eraser": "<? $toppings_array.clear() ?>"
  }
}
```
{: codeblock}

この後で $toppings_array コンテキスト変数を参照すると、'[]' のみが戻されます。

### JSONArray.contains(オブジェクト値)

このメソッドは、入力 JSONArray に入力値が含まれていれば true を返します。

直前のノードまたは外部アプリケーションによって設定されたダイアログ・ランタイム・コンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

ダイアログのノード条件または応答条件:

```json
$toppings_array.contains('ham')
```
{: codeblock}

結果: `True`。配列にエレメント「ham」が含まれています。

### JSONArray.containsIntent(String intent_name, Double min_score, [Integer top_n])
{: #dialog-methods-array-containsIntent}

このメソッドは、指定のインテントが `intents` JSONArray に特に含まれており、そのインテントの信頼度スコアが指定の最小スコア以上である場合に、`true` を戻します。オプションで数字を指定して、インテントが配列内のその数字の位までの最上位要素に含まれていなければならないことを指示することができます。

指定済みのインテントが配列内にないか、信頼度スコアが最小スコア以上でないか、配列におけるインテントの位置が指定された添字位置より低い場合には、`false` を戻します。

このサービスは、ユーザー入力が送信されるたびに、入力内で検出したインテントをリストする `intents` 配列を自動生成します。この配列は、サービスが検出したすべてのインテントを、信頼度の高い順にリストします。

ノード条件内でこのメソッドを使用することにより、インテントの存在を検査することに加えて、ノードを処理してその応答を返す前に満たしているべき信頼度スコアしきい値を設定することができます。

例えば、次の条件が満たされる場合のみダイアログ・ノードをトリガーするには、ノード条件内で以下の式を使用します。

- `#General_Ending` インテントが存在する。
- `#General_Ending` インテントの信頼度スコアが 80% を超える。
- `#General_Ending` インテントが、インテント配列内の最上位の 2 つのインテントの 1 つである。

```bash
intents.containsIntent("General_Ending", 0.8, 2)
```
{: codeblock}

### JSONArray.filter(temp, "temp.property operator comparison_value")
{: #dialog-methods-array-filter}

各配列要素値と指定した値を比較して、配列をフィルタリングします。このメソッドは、[コレクション・プロジェクション](#collection-projection)と似ています。コレクション・プロジェクションは、配列要素の名前と値のペアの名前に基づいて、フィルタリングされた配列を返します。このフィルター・メソッドは、配列要素の名前と値のペアの値に基づいて、フィルタリングされた配列を戻します。

フィルター式の値は、以下のとおりです。

- `temp`: 各配列要素が評価される際に一時的に使用される変数の名前。例: `city`。
- `property`: `comparison_value` と比較する要素プロパティー。このプロパティーは、1 つ目のパラメーターで指定した一時変数のプロパティーとして指定します。`temp.property` という構文を使用します。例えば、`latitude` が配列内の名前と値のペアのに対して有効な要素名であれば、`city.latitude` としてこのプロパティーを指定します。
- `operator`: プロパティー値と `comparison_value` の比較に使用する演算子。

    サポートされている演算子は、以下のとおりです。

    <table>
    <caption>サポートされているフィルター演算子</caption>
    <tr>
      <th>演算子</th>
      <th>説明</th>
    </tr>
    <tr>
      <td>`==`</td>
      <td>等しい</td>
    </tr>
    <tr>
      <td>`>`</td>
      <td>次より大きい</td>
    </tr>
    <tr>
      <td>`<`</td>
      <td>次より小さい</td>
    </tr>
    <tr>
      <td>`>=`</td>
      <td>次以上</td>
    </tr>
    <tr>
      <td>`<=`</td>
      <td>次以下</td>
    </tr>
    <tr>
      <td>`!=`</td>
      <td>次と等しくない</td>
    </tr>
    </table>

- `comparison_value`: 各配列要素プロパティー値の比較対象にする値。ユーザー入力に応じて変化する可能性のある値を指定するには、コンテキスト変数かエンティティーを値として使用します。可変値を指定する場合には、評価時に `comparison_value` 値が有効であることを保証する論理を追加します。そうしないと、エラーが発生します。

#### フィルター例 1

例えば、市区町村の名前と人口数のセットが含まれる配列を評価し、人口が 500 万人を超える市区町村のみが含まれる小配列を戻すために、フィルター・メソッドを使用できます。

以下の `$cities` コンテキスト変数にはオブジェクトの配列が含まれています。各オブジェクトには `name` と `population` のプロパティーが含まれています。

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Rome",
      "population":2868104
   },
   {
      "name":"Beijing",
      "population":20693000
   },
   {
      "name":"Paris",
      "population":2241346
   }
]
```
{: codeblock}

以下の例では、任意の一時変数名は `city` です。SpEL 式は `$cities` 配列をフィルタリングして、人口が 500 万人を超える市区町村のみを組み込みます。

```bash
$cities.filter("city", "city.population > 5000000")
```
{: codeblock}

この式は、以下のフィルタリング済みの配列を戻します。

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

コレクション・プロジェクションを使用して、フィルター・メソッドで戻された配列の市区町村名のみを組み込んだ新しい配列を作成できます。続いて `join` メソッドを使用して、配列の 2 つの名前要素値をストリングとして表示し、コンマとスペースで値を区切ることができます。

```bash
The cities with more than 5 million people include <?  T(String).join(", ",($cities.filter("city", "city.population > 5000000")).![name]) ?>.
```
{: codeblock}

結果の応答: `The cities with more than 5 million people include Tokyo, Beijing.`

#### フィルター例 2

このフィルター・メソッドには、`comparison_value` 値をハードコーディングする必要がないという利点があります。この例では、ハードコーディングされた値 5000000 の代わりに、コンテキスト変数を使用しています。

この例では、`$population_min` コンテキスト変数に、数値 `5000000` が含まれます。任意の一時変数名は `city` です。SpEL 式は `$cities` 配列をフィルタリングして、人口が 500 万人を超える市区町村のみを組み込みます。

```bash
$cities.filter("city", "city.population > $population_min")
```
{: codeblock}

この式は、以下のフィルタリング済みの配列を戻します。

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

数値を比較する際には、フィルター・メソッドをトリガーする前に、比較に関係するコンテキスト変数を有効な値に設定していることを確認してください。比較対象にする配列要素にヌルが含まれる可能性がある場合は、`null` も有効な値になり得ることに注意してください。例えば、Tokyo の人口の名前と値のペアが `"population":null` で、比較式が `"city.population == $population_min"` の場合、`null` は `$population_min` コンテキスト変数の有効な値になります。
{: tip}

以下のようなダイアログ・ノード応答式を使用できます。

```bash
The cities with more than $population_min people include <?  T(String).join(", ",($cities.filter("city", "city.population > $population_min")).![name]) ?>.
```
{: codeblock}

結果の応答: `The cities with more than 5000000 people include Tokyo, Beijing.`

#### フィルター例 3

この例では、エンティティー名を `comparison_value` として使用しています。ユーザー入力は `What is the population of Tokyo?` です。任意の一時変数名は `y` です。`Tokyo` を含む市区町村名を認識する、`@city` という名前のエンティティーを作成してあります。

```bash
$cities.filter("y", "y.name == @city")
```

この式は、以下の配列を戻します。

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   }
]
```
{: codeblock}

コレクション・プロジェクションを使用して、元の配列の人口要素のみを含んだ配列を取得してから、`get` メソッドを使用して人口要素の値を戻すことができます。

```bash
The population of @city is: <? ($cities.filter("y", "y.name == @city").![population]).get(0) ?>.
```
{: codeblock}

式が戻す結果: `The population of Tokyo is 9273000.`

### JSONArray.get(整数)

このメソッドは、JSONArray から入力添字を返します。

直前のノードまたは外部アプリケーションによって設定されたダイアログ・ランタイム・コンテキスト:

```json
{
  "context": {
    "name": "John",
    "nested": {
      "array": [ "one", "two" ]
    }
  }
}
```
{: codeblock}

ダイアログのノード条件または応答条件:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

結果:
`True`。ネストされた配列に値として `one` が含まれています。

応答:

```json
"output": {
  "generic":[
      {
      "values": [
        {
        "text" : "The first item in the array is <?$nested.array.get(0)?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
```
{: codeblock}

### JSONArray.getRandomItem()

このメソッドは、入力 JSONArray からランダム項目を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

ダイアログ・ノード出力:

```json
{
  "output": {
  "generic":[
      {
      "values": [
        {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

結果: `"ham is a great choice!"` または `"onion is a great choice!"` または `"olives is a great choice!"`

**注:** 結果の出力テキストはランダムに選択されます。

### JSONArray.indexOf(value)
{: #dialog-methods-array-indexOf}

このメソッドは、パラメーターとして指定した値と一致する配列内要素の添字番号を返します。配列内にその値が見つからない場合は `-1` を返します。値はストリング (`"School"`)、整数 (`8`)、または倍精度 (`9.1`) のいずれかにすることができます。値は完全一致する必要があり、大/小文字が区別されます。

例として、以下のコンテキスト変数には配列が含まれています。

```json
{
  "context": {
    "array1": ["Mary","Lamb","School"],
    "array2": [8,9,10],
    "array3": [8.1,9.1,10.1]
  }
}
```

以下の式を使用して、指定されている値がある位置の配列添字を判別できます。

```bash
<? $array1.indexOf("Mary") ?> returns `0`
<? $array2.indexOf(9) ?> returns `1`
<? $array3.indexOf(10.1) ?> returns `2`
```

このメソッドは、例えばインテント配列内の要素の添字を取得する際などに役立つ場合があります。ユーザー入力が評価されるたびに生成されるインテントの配列に `indexOf` メソッドを適用して、特定のインテントの配列添字番号を判別できます。

```bash
intents.indexOf("General_Greetings")
```
{: codeblock}

特定のインテントの信頼度スコアを知る必要がある場合、上記の式を、  `intents[`*`index`*`].confidence`
 という構文の式に *`index`* 値として渡すことができます。例えば、次のようにします。

```bash
intents[intents.indexOf("General_Greetings")].confidence
```
{: codeblock}

### JSONArray.join(ストリング区切り文字)

このメソッドは、この配列内のすべての値をストリングに結合します。 値はストリングに変換され、入力区切り文字で区切られます。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

ダイアログ・ノード出力:

```json
{
  "output": {
  "generic":[
      {
      "values": [
        {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

結果:

```json
This is the array: onion;olives;ham;
```
{: codeblock}

JSON 配列内で複数の値を保管する変数を定義している場合は、その配列から値のサブセットを戻してから、join() メソッドを使用して適切にフォーマットできます。

#### コレクション・プロジェクション
{: #dialog-methods-collection-projection}

SpEL の `collection projection` 式は、オブジェクトが含まれている配列からサブコレクションを抽出します。コレクション・プロジェクションの構文は `array_that_contains_value_sets.![value_of_interest]` です。

例として、以下のコンテキスト変数は、フライト情報を保管する JSON 配列を定義しています。フライトごとに、時刻とフライト・コードという 2 つのデータ・ポイントがあります。

```json
"flights_found": [
  {
    "time": "10:00",
    "flight_code": "OK123"
  },
  {
    "time": "12:30",
    "flight_code": "LH421"
  },
  {
    "time": "16:15",
    "flight_code": "TS4156"
  }
]
```
{: codeblock}

フライト・コードのみを戻すには、以下の構文を使用してコレクション・プロジェクション式を作成できます。

```
<? $flights_found.![flight_code] ?>
```

この式は、`flight_code` 値の配列を `["OK123","LH421","TS4156"]` として戻します。詳しくは、[SpEL のコレクション・プロジェクションの資料 ](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html)を参照してください。

返された配列内の値に `join()` メソッドを適用すると、コンマ区切りリストでフライト・コードが表示されます。例えば、応答内で次の構文を使用できます。

```
The flights that fit your criteria are:
  <? T(String).join(",", $flights_found.![flight_code]) ?>.
```
{: codeblock}

結果: `The flights that match your criteria are: OK123,LH421,TS4156.`

### JSONArray.joinToArray(template)
{: #dialog-methods-joinToArray}

このメソッドは、テンプレート内で定義したフォーマットを配列に適用し、仕様に従ってフォーマットされた配列を戻します。このメソッドは、例えばダイアログ応答で返す配列値にフォーマットを適用する場合などに便利です。

テンプレートはストリング、JSON オブジェクト、または JSON 配列として指定できます。テンプレートで編集中の配列の値を参照するには、以下の構文規則に従います。

- `%`: 編集中の配列から返す要素または要素プロパティーの先頭または末尾を表します。
- `e`: フォーマットを適用する配列要素を一時的に表します。この一時変数名を `e` 以外に変更することはできません。

例えば、以下のコンテキスト変数には、3 つのフライトに関するフライト詳細情報のリストを含んだ配列が含まれています。

```json
"flights": [
      {
        "flight": "DL1040",
        "origin": "JFK",
        "carrier": "Alitalia",
        "duration": 485,
        "destination": "FCO",
        "arrival_date": "2019-02-03",
        "arrival_time": "07:00",
        "departure_date": "2019-02-02",
        "departure_time": "16:45"
      },
      {
        "flight": "DL1710",
        "origin": "JFK",
        "carrier": "Delta",
        "duration": 379,
        "destination": "LAX",
        "arrival_date": "2019-02-02",
        "arrival_time": "10:19",
        "departure_date": "2019-02-02",
        "departure_time": "07:00"
      },
      {
        "flight": "DL4379",
        "origin": "BOS",
        "carrier": "Virgin Atlantic",
        "duration": 385,
        "destination": "LHR",
        "arrival_date": "2019-02-03",
        "arrival_time": "09:05",
        "departure_date": "2019-02-02",
        "departure_time": "21:40"
      }
    ]
```
{: codeblock}

フライト・コードのリストのみを返そうとしているとします。各配列から `flight` 要素の値のみを抽出してリストで返すには、以下の式を使用できます。

```
The available flights are <? $flights.joinToArray("%e.flight%"). ?>
```
{: codeblock}

ダイアログ・ノード応答は `The available flights are ["DL1040","DL1710","DL4379"].` です。

配列をテキストとして表示するには、以下のように式で `join` メソッドを使用します。

```
The available flights are <? $flights.joinToArray("%e.flight%").join(", "). ?>
```
{: codeblock}

応答は `The available flights are DL1040, DL1710, DL4379.` になります。

#### 複雑なテンプレート
{: #dialog-methods-complex-template}

さらに複雑なテンプレートを作成するには、メソッド・パラメーター内にテンプレートの詳細情報を直接指定する代わりに、コンテキスト変数を作成できます。

以下のように、このテンプレート・コンテキスト変数には、配列要素のサブセットが含まれ、その前にラベルが追加されているので、応答で情報は見やすいリストの形式で表示されます。

```json
"template": "<br/>Flight number: %e.flight% <br/> Airline: %e.carrier% <br/> Departure date: %e.departure_date% <br/> Departure time: %e.departure_time% <br/> Arrival time: %e.arrival_time% <br/>"
```
{: codeblock}

Facebook や Slack などの一部の統合チャネルでは、改行用の `<br/>` HTML タグはレンダリングされ*ません*。
{: note}

`$template` 内で定義されているテンプレートを `$flights` 内の配列に適用するには、ダイアログ・ノード応答内で以下の式を使用します。

```
The flight info is <? $flights.joinToArray($template).join(" ") ?>
```
{: codeblock}

応答は以下のようになります。

```
The flight info is
Flight number: DL1040
Airline: Alitalia
Departure date: 2019-02-02
Departure time: 16:45
Arrival time: 07:00

Flight number: DL1710
Airline: Delta
Departure date: 2019-02-02
Departure time: 07:00
Arrival time: 10:19

Flight number: DL4379
Airline: Virgin Atlantic
Departure date: 2019-02-02
Departure time: 21:40
Arrival time: 09:05
```
{: screen}

このメソッドを使用すると、配列内の値が変更される頻度や配列内の要素の数が増えるかどうかは問題にならないという利点があります。各配列要素に、テンプレートで参照されているプロパティーのサブセットが少なくとも含まれていれば、式は機能します。

#### JSON オブジェクト・テンプレートの例
{: #dialog-methods-object-template}

この例では、テンプレート・コンテキスト変数が、`$flights` コンテキスト変数内の配列内で指定されている各フライト要素からフライト番号および到着と出発の日時を抽出する JSON オブジェクトとして定義されています。この方法を使用すれば、2 社の異なる航空会社 (それぞれの Web サービスでフライト情報に異なるフォーマットを採用している) が管理するフライトの詳細情報に、標準のフォーマットを適用することなども可能です。

```json
"template": {
      "departure": "Flight %e.flight% departs on %e.departure_date% at %e.departure_time%.",
      "arrival": "Flight %e.flight% arrives on %e.arrival_date% at %e.arrival_time%."
    }
```
{: codeblock}

返された配列からオブジェクトを読み取り、チャットボットの応答に適した形式に値をフォーマットする、カスタムのクライアント・アプリケーションを設計することもできます。以下の式を使用すると、ダイアログ・ノード応答で、フライトの到着の詳細情報オブジェクトを配列として返すことができます。

```
<? $flights.joinToArray($template) ?>
```
{: screen}

ダイアログ・ノード応答は、次のようになります。

```json
[
  {
    "arrival":"Flight DL1040 arrives on 2019-02-03 at 07:00.",
    "departure":"Flight DL1040 departs on 2019-02-02 at 16:45."
    },
  {
    "arrival":"Flight DL1710 arrives on 2019-02-02 at 10:19.",
    "departure":"Flight DL1710 departs on 2019-02-02 at 07:00."
    },
  {
    "arrival":"Flight DL4379 arrives on 2019-02-03 at 09:05.",
    "departure":"Flight DL4379 departs on 2019-02-02 at 21:40."
    }
  ]
  ```

応答内で `arrival` 要素と `departure` 要素の順序が入れ替わっている点に注目してください。通常このサービスは JSON オブジェクト内の要素を再配列します。特定の順序で要素が返されるようにするには、代わりに JSON 配列かストリングの値を使用してテンプレートを定義します。

### JSONArray.remove(整数)

このメソッドは、JSONArray から添字位置のエレメントを削除し、更新後の JSONArray を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

適用する更新:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

結果:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.removeValue(オブジェクト)

このメソッドは、JSONArray に最初に現れる値を削除し、更新後の JSONArray を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

適用する更新:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

結果:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.set(添字整数, オブジェクト値)

このメソッドは、JSONArray の入力添字を入力値に設定し、変更後の JSONArray を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

ダイアログ・ノード出力:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

結果:

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: codeblock}

### JSONArray.size()

このメソッドは、JSONArray 配列のサイズを整数として返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

適用する更新:

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

結果:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: codeblock}

### JSONArray split(正規表現ストリング)

このメソッドは、入力正規表現を使用して、入力ストリングを分割します。 結果はストリングの JSONArray です。

入力:

```
"bananas;apples;pears"
```
{: codeblock}

構文:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: codeblock}

### com.google.gson.JsonArray のサポート
{: #dialog-methods-com.google.gson.JsonArray}

組み込みメソッドに加えて、`com.google.gson.JsonArray` クラスの標準メソッドを使用できます。

#### 新規配列

new JsonArray().append('value')

ユーザーから提供された値を取り込む新規配列を定義するためには、配列をインスタンス化します。 また、インスタンス化するときにはプレースホルダー値を配列に追加する必要もあります。 そのためには、以下の構文を使用します。

```json
{
  "context": {
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

## 日付と時刻
{: #dialog-methods-date-time}

いくつかのメソッドは、日時の処理に使用できます。

ユーザー入力から日時情報を認識して抽出する方法については、[@sys-date および @sys-time エンティティー](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time)を参照してください。

### .after(日付または時刻ストリング)

日付/時刻値が日付/時刻引数より後かどうかを判別します。

### .before(日付または時刻ストリング)
日付/時刻値が日付/時刻引数より前かどうかを判別します。

例えば、次のようにします。
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')

- `時刻と日付`、`日付と時刻`、`時刻と日時`など、異なる項目を比較した場合、メソッドは false を返し、応答 JSON ログの `output.log_messages` に例外が出力されます。

  例えば、`@sys-date.before(@sys-time)` などです。
- `日時と時刻`を比較した場合、メソッドは日付を無視して時刻だけ比較します。

### now()

`yyyy-MM-dd HH:mm:ss` 形式の現在の日付と時刻を含むストリングを返します。

- 静的関数。
- この関数から返される日時値で他の日付/時刻メソッドを呼び出して、それらのメソッドの引数として渡すことができます。
- コンテキスト変数 `$timezone` が設定されている場合、この関数はクライアントのタイム・ゾーンでの日付と時刻を返します。

出力フィールドで `now()` が使用されるダイアログ・ノードの例:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "<? now() ?>"
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ]
  }
}
```
{: codeblock}

(まだ午前かどうかを決定するための) ノードの条件における `now()` の例:

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
      "generic":[
      {
        "values": [
        {
          "text": "Good morning!"
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
      ]
  }
}
```
{: codeblock}

### .reformatDateTime(形式ストリング)

日時ストリングをユーザー出力に必要な形式にフォーマット設定します。

指定された次の形式に従ってフォーマット設定されたストリングを返します。

- `MM/dd/yyyy` (12/31/2016 とする場合)
- `h a` (10pm とする場合)

曜日を返すには次を指定します。

- `E` (火曜日とする場合)
- `u` (曜日指標とする場合) (1 = 月曜日、...、7 = 日曜日)

例えば、次のコンテキスト変数定義の場合は、値 17:30:00 を *5:30 PM* として保存する $time 変数が作成されます。

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

形式は Java の [SimpleDateFormat ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html){: new_window} ルールに従います。

**注**: 時刻のみをフォーマット設定しようとすると、日付は `1970-01-01` として扱われます。

### .sameMoment(日付/時刻ストリング)

- 日付/時刻値が日付/時刻引数と同じかどうかを判別します。

### .sameOrAfter(日付/時刻ストリング)

- 日付/時刻値が日付/時刻引数以降かどうかを判別します。
- `.after()` に似ています。

### .sameOrBefore(日付/時刻ストリング)

- 日付/時刻値が日付/時刻引数以前かどうかを判別します。

### today()

`yyyy-MM-dd` 形式の現在の日付を含むストリングを返します。

- 静的関数。
- この関数から返される日付値に対して他の日付メソッドを呼び出して、それらのメソッドの引数として渡すことができます。
- コンテキスト変数 `$timezone` が設定されている場合、この関数はクライアントのタイム・ゾーンでの日付を返します。 設定されていない場合は、`GMT` のタイム・ゾーンを使用します。

出力フィールドで `today()` が使用されるダイアログ・ノードの例:

```json
{
  "conditions": "#what_day_is_it",
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "Today's date is <? today() ?>."
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ]
  }
}
```
{: codeblock}

結果: `Today's date is 2018-03-09.`

## 日時の計算
{: #dialog-methods-calculations}

日付を計算するには、以下のメソッドを使用します。

| 方法                  | 説明 |
|-------------------------|-------------|
| `<date>.minusDays(n)`   | 指定した日付より n 日前の日付を戻します。 |
| `<date>.minusMonths(n)` | 指定した日付より n カ月前の日付を戻します。 |
| `<date>.minusYears(n)`  | 指定した日付より n 年前の日付を戻します。 |
| `<date>.plusDays(n)`   | 指定した日付より n 日後の日付を戻します。 |
| `<date>.plusMonths(n)` | 指定した日付より n カ月後の日付を戻します。 |
| `<date>.plusYears(n)`  | 指定した日付より n 年後の日付を戻します。 |

`<date>` は `yyyy-MM-dd` または `yyyy-MM-dd HH:mm:ss` のフォーマットで指定します。

明日の日付を取得するには、以下の式を指定します。

```json
{
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "Tomorrow's date is <? today().plusDays(1) ?>."
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ]
  }
}
```
{: codeblock}

今日が 2018 年 3 月 9 日である場合の結果: `Tomorrow's date is 2018-03-10.`

今日から 1 週間後の日付を取得するには、以下の式を指定します。

```json
{
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "Next week's date is <? @sys-date.plusDays(7) ?>."
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ]
  }
}
```
{: codeblock}

@sys-date エンティティーに取り込まれた日付が今日の日付 (2018 年 3 月 9 日) である場合の結果: `Next week's date is 2018-03-16.`

1 カ月前の日付を取得するには、以下の式を指定します。

```json
{
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "Last month the date was <? today().minusMonths(1) ?>."
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ]
  }
}
```
{: codeblock}

今日が 2018 年 3 月 9 日である場合の結果: `Last month the date was 2018-02-9.`

時刻を計算するには、以下のメソッドを使用します。

| 方法                  | 説明 |
|-------------------------|-------------|
| `<time>.minusHours(n)`   | 指定した時刻より n 時間前の時刻を戻します。 |
| `<time>.minusMinutes(n)` | 指定した時刻より n 分前の時刻を戻します。 |
| `<time>.minusSeconds(n)`  | 指定した時刻より n 秒前の時刻を戻します。 |
| `<time>.plusHours(n)`   | 指定した時刻より n 時間後の時刻を戻します。 |
| `<time>.plusMinutes(n)` | 指定した時刻より n 分後の時刻を戻します。 |
| `<time>.plusSeconds(n)`  | 指定した時刻より n 秒後の時刻を戻します。 |

`<time>` は `HH:mm:ss` のフォーマットで指定します。

現時点から 1 時間後の時刻を取得するには、以下の式を指定します。

```json
{
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "One hour from now is <? now().plusHours(1) ?>."
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ]
  }
}
```
{: codeblock}

現在が午前 8 時である場合の結果: `One hour from now is 09:00:00.`

30 分前の時刻を取得するには、以下の式を指定します。

```json
{
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "A half hour before @sys-time is <? @sys-time.minusMinutes(30) ?>."
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ]
  }
}
```
{: codeblock}

@sys-time エンティティーに取り込まれた時刻が午前 8 時である場合の結果: `A half hour before 08:00:00 is 07:30:00.`

返された時刻を再フォーマットするには、以下の式を使用できます。

```json
{
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "6 hours ago was <? now().minusHours(6).reformatDateTime('h:mm a') ?>."
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ]
  }
}
```
{: codeblock}

現在が午後 2 時 19 分である場合の結果: `6 hours ago was 8:19 AM.`

### 期間の処理
{: #dialog-methods-time-spans}

今日の日付が特定の時間枠内に入っているかどうかに基づいて応答を表示するには、時刻関連のメソッドを組み合わせて使用できます。例えば、毎年休暇シーズン中に特価提供を実施する場合は、今日の日付が今年の 11 月 25 日から 12 月 24 日の間に入っているかどうか調べることができます。最初に、対象の日付をコンテキスト変数として定義します。

以下の開始日と終了日のコンテキスト変数式で、動的に導出された現在の年の値にハードコーディングされた月と日の値を連結して日付が構成されます。

```json
"context": {
   "end_date": "<? now().reformatDateTime('Y') + '-12-24' ?>",
   "start_date": "<? now().reformatDateTime('Y') + '-11-25' ?>"
 }
```

応答条件で、コンテキスト変数として定義した開始日と終了日の間に現在の日付が入っている場合のみ応答が表示されるように指示できます。

`now().after($start_date) && now().before($end_date)`

### java.util.Date のサポート
{: #dialog-methods-java.util.Date}

組み込みメソッドに加えて、`java.util.Date` クラスの標準メソッドを使用できます。

今日から 1 週間後の日付を取得するには、次の構文を使用します。

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))) ?>"
  }
}
```
{: codeblock}

この式は、最初に現在日付を 1970 年 1 月 1 日 00:00:00 GMT からのミリ秒として取得します。 また、7 日間のミリ秒数も計算します (`(24*60*60*1000L)` は、1 日をミリ秒で表しています)。 次に、現在日付に 7 日を加えます。 結果は、今日から 1 週間後の日を示す完全な日付です。 例えば、`Fri Jan 26 16:30:37 UTC 2018` のようになります。 この時間は UTC タイム・ゾーンであることに気を付けてください。 いつでも、この 7 を、値を渡せる変数 (`$number_of_days` など) に変更できます。 この式が評価される前に、その値が設定されていることを確認してください。

後で、サービスで生成された別の日付と比較できるようにするには、日付の形式を再設定する必要があります。 システム・エンティティー (`@sys-date`) および別の組み込みメソッド (`now()`) は、日付を `yyyy-MM-dd` 形式に変換します。

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))).format('yyyy-MM-dd') ?>"
  }
}
```
{: codeblock}

日付の形式を再設定すると、結果は `2018-01-26` になります。 これで、応答条件で `@sys-date.after($week_from_today)` のような式を使用して、ユーザー入力で指定された日付をコンテキスト変数に保存された日付と比較できます。

次の式は、今から 3 時間後の時間を計算します。

```json
{
  "context": {
    "future_time": "<? new Date(new Date().getTime() + (3 * (60*60*1000L)) -
      (5 * (60*60*1000L))).format('h:mm a') ?>"
  }
}
```
{: codeblock}

`(60*60*1000L)` の値は、1 時間をミリ秒で表しています。 この式は、現在時刻に 3 時間を加算しています。 そして、その時間から 5 時間を減算して、UTC タイム・ゾーンの時間を EST タイム・ゾーンで再計算します。 また、時間と分、および AM/PM を含むように日付値の形式を再設定します。

## 数値
{: #dialog-methods-numbers}

これらのメソッドを使用して、数値を取得し、形式を再設定できます。

ユーザー入力から数値を認識して抽出するシステム・エンティティーについては、[@sys-number エンティティー](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number)を参照してください。

ユーザー入力内の特定の数値形式 (注文番号参照など) をサービスで認識したい場合は、それをキャプチャーするためのパターン・エンティティーを作成することを検討してください。 詳しくは、[エンティティーの作成](/docs/services/assistant?topic=assistant-entities)を参照してください。

数字の小数部の桁数を変更する場合 (例えば、数字を通貨値として再フォーマットする場合) は、[String format() メソッド](#dialog-methods-java.lang.String)を参照してください。

### toDouble()

  オブジェクトまたはフィールドを Double 数値型に変換します。 任意のオブジェクトまたはフィールドに対してこのメソッドを呼び出すことができます。 変換に失敗した場合は、*null* が返されます。

### toInt()

  オブジェクトまたはフィールドを Integer 数値型に変換します。 任意のオブジェクトまたはフィールドに対してこのメソッドを呼び出すことができます。 変換に失敗した場合は、*null* が返されます。

### toLong()

  オブジェクトまたはフィールドを Long 数値型に変換します。 任意のオブジェクトまたはフィールドに対してこのメソッドを呼び出すことができます。 変換に失敗した場合は、*null* が返されます。

  SpEL 式で Long 数値型を指定する場合は、Long 数値型であることを示す `L` を数値の末尾に付加する必要があります (例: `5000000000L`)。 32 ビットの整数型に収まらない数値には、この構文が必須です。 例えば、2^31 (2,147,483,648) より大きい数値や -2^31 (-2,147,483,648) より小さい数値は Long 型と見なされます。 Long 数値型の最小値は -2^63、最大値は 2^63-1 です。

### Java 数値のサポート
{: #dialog-methods-java.lang.Number}

### java.lang.Math()

基本的な数値演算を実行します。

次のようなクラス・メソッドを使用できます。

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "The bigger number is $bigger_number."
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ]
  }
}
```
{: codeblock}

- min()

```json
{
  "context": {
    "smaller_number": "<? T(Math).min($number1,$number2) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "The smaller number is $smaller_number."
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ]
  }
}
```
{: codeblock}

- pow()

```json
{
  "context": {
    "power_of_two": "<? T(Math).pow($base.toDouble(),2.toDouble()) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "Your number $base to the second power is $power_of_two."
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ]
  }
}
```
{: codeblock}

その他のメソッドについては、[java.lang.Math 参照資料](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html)を参照してください。

### java.util.Random()

乱数を返します。 以下のいずれかの構文オプションを使用できます。

- ランダム・ブール値 (true または false) を返すには、`<?new Random().nextBoolean()?>` を使用します。
- 0 (含まれる) から 1 (含まれない) までのランダム倍精度浮動小数点数を返すには、` を使用します。<?new Random().nextDouble()?>`
- 0 (含まれる) から指定した数値までのランダム整数を返すには、`<?new Random().nextInt(n)?>` を使用します。ここで、n は目的の数値範囲の最高値に 1 を加えた数値です。
  例えば、0 から 10 までの乱数を返す場合は、`<?new Random().nextInt(11)?>` を指定します。
- 整数値の全範囲 (-2147483648 から 2147483648) のランダム整数を返すには、`<?new Random().nextInt()?>` を使用します。

例えば、#random_number インテントでトリガーされるダイアログ・ノードを作成することもできます。 この場合、最初の応答条件は、例えば次のようになります。

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "Here's a random number between 0 and @sys-number.literal: $answer."
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ]
  }
}
```
{: codeblock}

その他のメソッドについては、[java.util.Random 参照資料](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html)を参照してください。

以下のクラスの標準メソッドも使用できます。

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## オブジェクト
{: #dialog-methods-objects}

### JSONObject.clear()

このメソッドは、JSON オブジェクトのすべての値をクリアしてヌルを返します。

例えば、次の $user コンテキスト変数の現行値をクリアしたいとします。

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

出力で以下の式を使用して、このオブジェクトの値をクリアするフィールドを定義します。

```json
{
  "output": {
    "object_eraser": "<? $user.clear() ?>"
  }
}
```
{: codeblock}

この後で $user コンテキスト変数を参照すると、`{}` のみが戻されます。

`clear()` メソッドは、API `/message` 呼び出しの本文の `context` JSON オブジェクトか `output` JSON オブジェクトで使用できます。

#### コンテキストのクリア
{: #dialog-methods-clearing-context}

`clear()` メソッドを使用して `context` オブジェクトをクリアする際には、以下を除く**すべての**変数がクリアされます。

 - `context.conversation_id`
 - `context.timezone`
 - `context.system`

**警告**: すべてのコンテキスト変数値とは、以下のものを指します。

  - 現行セッション中にトリガーされたノード内の変数に対して設定されたすべてのデフォルト値。
  - 現行セッション中にユーザーまたは外部サービスにより提供された情報を使用してデフォルト値に加えられた更新。

このメソッドを使用する場合は、出力オブジェクトで定義する変数内の式の中でこのメソッドを指定できます。例えば、次のようにします。

```json
{
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "Response for this node."
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ],
    "context_eraser": "<? context.clear() ?>"
  }
}

```

#### 出力のクリア
{: #dialog-methods-clearing-output}

`clear()` メソッドを使用して `output` オブジェクトをクリアする際には、出力オブジェクトをクリアするために使用する変数と現行ノード内で定義しているテキスト応答を除き、すべての変数がクリアされます。以下の変数もクリアされません。

- `output.nodes_visited`
- `output.nodes_visited_details`

このメソッドを使用する場合は、出力オブジェクトで定義する変数内の式の中でこのメソッドを指定できます。例えば、次のようにします。

```json
{
  "output": {
    "generic":[
      {
        "values": [
        {
          "text": "Have a great day!"
          }
        ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
    ],
    "output_eraser": "<? output.clear() ?>"
  }
}
```

ツリー内の前のノードが `I'm happy to help.` というテキスト応答を定義している場合、これは上記で定義した JSON 出力オブジェクトが含まれるノードにジャンプし、`Have a great day.` のみが応答として表示されるようになります。`I'm happy to help.` という出力は表示されません。この出力はクリアされ、`clear()` メソッドを呼び出しているノードのテキスト応答に置き換えられるからです。

### JSONObject.has(ストリング)

このメソッドは、複合 JSONObject に入力名のプロパティーがあれば true を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

ダイアログ・ノード出力:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

結果: 条件は true です。user オブジェクトにプロパティー `first_name` が含まれています。

### JSONObject.remove(ストリング)

このメソッドは、入力 `JSONObject` から名前のプロパティーを削除します。 このメソッドから返される `JSONElement` が、削除される `JSONElement` です。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

ダイアログ・ノード出力:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

結果:

```json
{
  "context": {
    "user": {
      "last_name": "Snow"
    },
    "attribute_removed": {
      "first_name": "John"
    }
  }
}
```
{: codeblock}

### com.google.gson.JsonObject のサポート
{: #dialog-methods-com.google.gson.JsonObject}

組み込みメソッドに加えて、`com.google.gson.JsonObject` クラスの標準メソッドを使用できます。

## ストリング
{: #dialog-methods-strings}

これらのメソッドは、テキストの操作に使用できます。

ユーザー入力から特定のタイプの文字列 (人名や場所など) を認識して抽出する方法については、[システム・エンティティー](/docs/services/assistant?topic=assistant-system-entities)を参照してください。

**注:** 正規表現を使用するメソッドで、正規表現を指定するときに使用する構文について詳しくは、[RE2 構文のリファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/google/re2/wiki/Syntax){: new_window} を参照してください。

### String.append(オブジェクト)

このメソッドは、ストリングに入力オブジェクトをストリングとして付加し、変更後のストリングを返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

構文:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: codeblock}

### String.contains(ストリング)

このメソッドは、ストリングに入力サブストリングが含まれていれば true を返します。

入力: "Yes, I'd like to go."

構文:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

結果: 条件は `true` です。

### String.endsWith(ストリング)

このメソッドは、ストリングが入力サブストリングで終わっていれば true を返します。

入力:

```
"What is your name?".
```
{: codeblock}

構文:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

結果: 条件は `true` です。

### String.extract(正規表現ストリング, グループ添字整数)

このメソッドは、入力正規表現の指定したグループ添字によって取り出されたストリングを返します。

入力:

```
"Hello 123456".
```
{: codeblock}

構文:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **重要:** `\\d` を正規表現として処理するには、別の `\\` を追加して両方の円記号 (\) をエスケープする必要があります。つまり、`\\\\d` にします。

結果:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(正規表現ストリング)

このメソッドは、ストリングのいずれかのセグメントが入力正規表現と一致すれば true を返します。  JSONArray または JSONObject エレメントに対してこのメソッドを呼び出すことができます。このメソッドは、配列またはオブジェクトをストリングに変換してから比較を行います。

入力:

```
"Hello 123456".
```
{: codeblock}

構文:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

結果: 条件は true です。入力テキストの数値部分が正規表現 `^[^\d]*[\d]{6}[^\d]*$` と一致します。

### String.isEmpty()

このメソッドは、ストリングが空ストリングであるがヌル以外の場合に true を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

構文:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

結果: 条件は `true` です。

### String.length()

このメソッドは、ストリングの文字長を返します。

入力:

```
"Hello"
```
{: codeblock}

構文:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: codeblock}

### String.matches(正規表現ストリング)

このメソッドは、ストリングが入力正規表現と一致すれば true を返します。

入力:

```
"Hello".
```
{: codeblock}

構文:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

結果: 条件は true です。入力テキストが正規表現 `\^Hello\$` と一致します。

### String.startsWith(ストリング)

このメソッドは、ストリングが入力サブストリングで始まっていれば true を返します。

入力:

```
"What is your name?".
```
{: codeblock}

構文:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

結果: 条件は `true` です。

### String.substring(Integer beginIndex, Integer endIndex)

このメソッドは、`beginIndex` の位置にある文字と `endIndex` の前の添字に設定された最後の文字を含むサブストリングを取得します。
endIndex の位置の文字は含まれません。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

構文:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: codeblock}

### String.toLowerCase()

このメソッドは、小文字に変換された元のストリングを返します。

入力:

```
"This is A DOG!"
```
{: codeblock}

構文:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: codeblock}

### String.toUpperCase()

このメソッドは、大文字に変換された元のストリングを返します。

入力:

```
"hi there".
```
{: codeblock}

構文:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: codeblock}

### String.trim()

このメソッドは、ストリングの先頭と末尾にスペースがあればそれを切り取り、変更後のストリングを返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

構文:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: codeblock}

### java.lang.String のサポート
{: #java.lang.String}

組み込みメソッドに加えて、`java.lang.String` クラスの標準メソッドを使用できます。

#### java.lang.String.format()

標準の Java String `format()` メソッドをテキストに適用できます。 形式の詳細を指定するために使用する構文については、[java.util.formatter のリファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter#syntax){: new_window} を参照してください。

例えば、次の式は 3 つの 10 進整数 (1、1、および 2) を受け取り、それらを文に追加します。

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

結果: `1 + 1 equals 2`

数字の小数部の桁数を変更するには、以下の構文を使用します。

```
{
  <? T(String).format('%.2f',<number to format>) ?>
}
```
{: codeblock}

例えば、米ドルにフォーマットする必要のある $number 変数が `4.5` の場合は、`Your total is $<? T(String).format('%.2f',$number) ?>` などの応答で `Your total is $4.50` が返されます。

## 間接的なデータ型変換
{: #dialog-methods-indirect-type-conversion}

例えば、ノード応答の一部として式をテキスト内に組み込むと、その値は文字列としてレンダリングされます。 式を元のデータ・タイプでレンダリングする場合は、式をテキストで囲まないでください。

例えば、次の式をダイアログ・ノードの応答に追加すると、ユーザー入力で認識されたエンティティーを文字列形式で返すことができます。

```json
  The entities are <? entities ?>.
```
{: codeblock}

ユーザーが入力として *Hello now* を指定した場合、`now` 参照によって @sys-date エンティティーと @sys-time エンティティーがトリガーされます。 entities オブジェクトは配列ですが、式がテキスト内に組み込まれているため、エンティティーは次のように文字列形式で返されます。

```json
  The entities are 2018-02-02, 14:34:56.
```
{: codeblock}

応答にテキストを指定しない場合は、代わりに配列が返されます。 例えば、テキストで囲まずに式だけを応答として指定します。

```
  <? entities ?>
```
{: codeblock}

エンティティー情報がネイティブのデータ・タイプ、配列として返されます。

```json
[
  {
    "entity":"sys-date","location":[6,9],"value":"2018-02-02","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  },
  {
    "entity":"sys-time","location":[6,9],"value":"14:33:22","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  }
  ]
```
{: codeblock}

別の例として、次の $array コンテキスト変数は配列ですが、$string_array コンテキスト変数は文字列です。

```json
{
  "context": {
    "array": [
      "one",
      "two"
    ],
    "array_in_string": "this is my array: $array"
  }
}
```
{: codeblock}

「Try it out」ペインでこれらのコンテキスト変数の値をチェックすると、次のように値が指定されていることを確認できます。

**$array** : `["one","two"]`

**$array_in_string** : `"this is my array: [\"one\",\"two\"]"`

この後、$array 変数には `<? $array.removeValue('two') ?>` などの配列メソッドを実行できますが、$array_in_string 変数には実行できません。
