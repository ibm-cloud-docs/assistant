---

copyright:
  years: 2015, 2019
lastupdated: "2018-02-05"

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

# 式言語のメソッド
{: #dialog-methods}

ユーザー発話から抽出した値を処理して、応答の中のコンテキスト変数や条件などで参照することができます。
{: shortdesc}

## 評価構文

他の変数内の変数値を拡張したり、メソッドを出力テキストやコンテキスト変数に適用したりするには、 `<? expression ?>` 式構文を使用します。例:

- **数値プロパティーの増分**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **オブジェクトでのメソッドの呼び出し**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

以下の各セクションで、値を処理するために使用できるメソッドについて説明します。メソッドはデータ・タイプ別にまとめています。

- [配列](dialog-methods.html#arrays)
- [日付と時刻](dialog-methods.html#date-time)
- [数値](dialog-methods.html#numbers)
- [オブジェクト](dialog-methods.html#objects)
- [ストリング](dialog-methods.html#strings)

## 配列
{: #arrays}

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
    "text": {
      "values": [
        "The first item in the array is <?$nested.array.get(0)?>"
      ],
      "selection_policy": "sequential"
    }
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
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

結果: `"ham is a great choice!"` または `"onion is a great choice!"` または `"olives is a great choice!"`

**注:** 結果の出力テキストはランダムに選択されます。

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
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

結果:

```json
This is the array: onion;olives;ham;
```
{: codeblock}

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
{: #com.google.gson.JsonArray}

組み込みメソッドに加えて、`com.google.gson.JsonArray` クラスの標準メソッドを使用できます。

#### 新規配列

new JsonArray().append('value')

ユーザーから提供された値を取り込む新規配列を定義するためには、配列をインスタンス化します。また、インスタンス化するときにはプレースホルダー値を配列に追加する必要もあります。そのためには、以下の構文を使用します。

```json
{
  "context": {
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

## 日付と時刻
{: #date-time}

いくつかのメソッドは、日時の処理に使用できます。

ユーザー入力から日時情報を認識して抽出する方法については、[@sys-date および @sys-time エンティティー](system-entities.html#sys-datetime)を参照してください。

### .after(日付/時刻ストリング)

- 日付/時刻値が日付/時刻引数より後かどうかを判別します。
- `.before()` に似ています。

### .before(日付または時刻ストリング)

- 例:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- 日付/時刻値が日付/時刻引数より前かどうかを判別します。
- `時刻と日付`、`日付と時刻`、`時刻と日時`など、異なる項目を比較した場合、メソッドは false を返し、応答 JSON ログの `output.log_messages` に例外が出力されます。
    - 例えば、`@sys-date.before(@sys-time)` などです。
- `日時と時刻`を比較した場合、メソッドは日付を無視して時刻だけ比較します。

### now()

- 静的関数。
- `yyyy-MM-dd HH:mm:ss` 形式の現在の日付と時刻を含むストリングを返します。
- この関数から返される日時値で他の日付/時刻メソッドを呼び出して、それらのメソッドの引数として渡すことができます。
- コンテキスト変数 `$timezone` が設定されている場合、この関数はクライアントのタイム・ゾーンでの日付と時刻を返します。

出力フィールドで `now()` が使用されるダイアログ・ノードの例:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

(まだ午前かどうかを決定するための) ノードの条件における `now()` の例:

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
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

### java.util.Date のサポート
{: #java.util.Date}

組み込みメソッドに加えて、`java.util.Date` クラスの標準メソッドを使用できます。

#### 日付の計算

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

この式は、最初に現在日付を 1970 年 1 月 1 日 00:00:00 GMT からのミリ秒として取得します。また、7 日間のミリ秒数も計算します(`(24*60*60*1000L)` は、1 日をミリ秒で表しています)。次に、現在日付に 7 日を加えます。結果は、今日から 1 週間後の日を示す完全な日付です。例えば、`Fri Jan 26 16:30:37 UTC 2018` のようになります。この時間は UTC タイム・ゾーンであることに気を付けてください。いつでも、この 7 を、値を渡せる変数 (`$number_of_days` など) に変更できます。この式が評価される前に、その値が設定されていることを確認してください。

後で、サービスで生成された別の日付と比較できるようにするには、日付の形式を再設定する必要があります。システム・エンティティー (`@sys-date`) および別の組み込みメソッド (`now()`) は、日付を `yyyy-MM-dd` 形式に変換します。

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))).format('yyyy-MM-dd') ?>"
  }
}
```
{: codeblock}

日付の形式を再設定すると、結果は `2018-01-26` になります。これで、応答条件で `@sys-date.after($week_from_today)` のような式を使用して、ユーザー入力で指定された日付をコンテキスト変数に保存された日付と比較できます。

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

`(60*60*1000L)` の値は、1 時間をミリ秒で表しています。この式は、現在時刻に 3 時間を加算しています。そして、その時間から 5 時間を減算して、UTC タイム・ゾーンの時間を EST タイム・ゾーンで再計算します。また、時間と分、および AM/PM を含むように日付値の形式を再設定します。

## 数値
{: #numbers}

これらのメソッドを使用して、数値を取得し、形式を再設定できます。

ユーザー入力から数値を認識して抽出するシステム・エンティティーについては、[@sys-number エンティティー](system-entities.html#sys-number)を参照してください。

ユーザー入力内の特定の数値形式 (注文番号参照など) をサービスで認識したい場合は、それをキャプチャーするためのパターン・エンティティーを作成することを検討してください。詳しくは、[エンティティーの作成](entities.html#creating-entities)を参照してください。

### toDouble()

  オブジェクトまたはフィールドを Double 数値型に変換します。 任意のオブジェクトまたはフィールドに対してこのメソッドを呼び出すことができます。 変換に失敗した場合は、*null* が返されます。

### toInt()

  オブジェクトまたはフィールドを Integer 数値型に変換します。 任意のオブジェクトまたはフィールドに対してこのメソッドを呼び出すことができます。 変換に失敗した場合は、*null* が返されます。

### toLong()

  オブジェクトまたはフィールドを Long 数値型に変換します。 任意のオブジェクトまたはフィールドに対してこのメソッドを呼び出すことができます。 変換に失敗した場合は、*null* が返されます。

  SpEL 式で Long 数値型を指定する場合は、Long 数値型であることを示す `L` を数値の末尾に付加する必要があります (例: `5000000000L`)。32 ビットの整数型に収まらない数値には、この構文が必須です。例えば、2^31 (2,147,483,648) より大きい数値や -2^31 (-2,147,483,648) より小さい数値は Long 型と見なされます。Long 数値型の最小値は -2^63、最大値は 2^63-1 です。

### Java 数値のサポート
{: #java.lang.Number}

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
    "text": {
      "values": [
        "The bigger number is $bigger_number."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "The smaller number is $smaller_number."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "Your number $base to the second power is $power_of_two."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "Here's a random number between 0 and @sys-number.literal: $answer."
      ],
      "selection_policy": "sequential"
    }
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
{: #objects}

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
{: #com.google.gson.JsonObject}

組み込みメソッドに加えて、`com.google.gson.JsonObject` クラスの標準メソッドを使用できます。

## ストリング
{: #strings}

これらのメソッドは、テキストの操作に使用できます。

ユーザー入力から特定のタイプの文字列 (人名や場所など) を認識して抽出する方法については、[システム・エンティティー](system-entities.html)を参照してください。

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

### String.substring(beginIndex 整数, endIndex 整数)

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

標準の Java String `format()` メソッドをテキストに適用できます。形式の詳細を指定するために使用する構文については、[java.util.formatter のリファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window} を参照してください。

例えば、次の式は 3 つの 10 進整数 (1、1、および 2) を受け取り、それらを文に追加します。

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

結果: `1 + 1 equals 2`

## 間接的なデータ型変換

例えば、ノード応答の一部として式をテキスト内に組み込むと、その値は文字列としてレンダリングされます。式を元のデータ・タイプでレンダリングする場合は、式をテキストで囲まないでください。

例えば、次の式をダイアログ・ノードの応答に追加すると、ユーザー入力で認識されたエンティティーを文字列形式で返すことができます。

```json
  The entities are <? entities ?>.
```
{: codeblock}

ユーザーが入力として *Hello now* を指定した場合、`now` 参照によって @sys-date エンティティーと @sys-time エンティティーがトリガーされます。entities オブジェクトは配列ですが、式がテキスト内に組み込まれているため、エンティティーは次のように文字列形式で返されます。

```json
  The entities are 2018-02-02, 14:34:56.
```
{: codeblock}

応答にテキストを指定しない場合は、代わりに配列が返されます。例えば、テキストで囲まずに式だけを応答として指定します。

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
