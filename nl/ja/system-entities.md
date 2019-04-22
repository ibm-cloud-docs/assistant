---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# システム・エンティティーの詳細
{: #system-entities}

このリファレンス・セクションでは、使用可能なシステム・エンティティーの完全な情報を示しています。 システム・エンティティーの詳細情報と使用法については、[Defining entities](/docs/services/assistant?topic=assistant-entities#entities-enable-system-entities) を参照して、『Enabling system entities』を検索してください。
{: shortdesc}

システム・エンティティーは、[サポートされる言語](/docs/services/assistant?topic=assistant-language-support) のトピックに示されている言語で使用可能です。

## @sys-currency エンティティー
{: #system-entities-sys-currency}

@sys-currency システム・エンティティーでは、発話内で通貨記号や通貨固有の用語を伴って表現されている通貨値が検出されます。 数値が返されます。

### 認識される形式
{: #system-entities-sys-currency-formats}

- 20 cents
- Five dollars
- $10

### メタデータ
{: #system-entities-sys-currency-metadata}

- `.numeric_value`: 基本単位での整数またはダブルの標準数値
- `.unit`: 基本単位の通貨コード (例えば、「USD」や「EUR」)

### 戻り値
{: #system-entities-sys-currency-returns}

入力が `twenty dollars` または `$1,234.56` の場合、@sys-currency では以下の値が返されます。

| 属性                   | タイプ   | `twenty dollars` の戻り値 | `$1,234.56` の戻り値 |
|-----------------------------|--------|-------------------------------|-------------------------:|
| @sys-currency               | ストリング | 20                            |                  1234.56 |
| @sys-currency.literal       | ストリング | twenty dollars                |                $1,234.56 |
| @sys-currency.numeric_value | 数値 | 20                            |                  1234.56 |
| @sys-currency.location      | 配列  | [0,14]                        |                    [0,9] |
| @sys-currency.unit          | ストリング | USD*                          |                      USD |

*@sys-currency.unit では常に 3 文字の ISO 通貨コードが返されます。

入力がスペイン語で `veinte euro` または <code>&euro;1.234,56</code> の場合、@sys-currency では以下の値が返されます。

| 属性                   | タイプ   | `veinte euro` の戻り値 | <code>&euro;1.234,56</code> の戻り値 |
|-----------------------------|--------|-----------------------------|-------------------------:|
| @sys-currency               | ストリング | 20                          |                  1234.56 |
| @sys-currency.literal       | ストリング | veinte euro                 |                &euro;1.234,56 |
| @sys-currency.numeric_value | 数値 | 20                          |                  1234.56 |
| @sys-currency.location      | 配列  |[0,11]                       |                     [0,9]|
| @sys-currency.unit          | ストリング | EUR*                        |                     EUR  |
*@sys-currency.unit では常に 3 文字の ISO 通貨コードが返されます。

サポートされている他の言語や国の通貨でも同等の結果が得られます。

### @system-currency 使用のヒント
{: #system-entities-currencty-usage-tips}

- 通貨値は、@sys-number エンティティーのインスタンスとしても認識されます。 通貨値と数値の両方をチェックするために別個の条件を使用している場合は、通貨をチェックする条件を、数値をチェックする条件よりも上に置きます。

- ノード条件として @sys-currency エンティティーを使用している場合に、ユーザーが値として `$0` を指定すると、値は通貨として適切に認識されますが、条件は通貨のゼロではなく数値のゼロとして評価されます。 結果として、予期されている応答が返されません。 ゼロが適切に処理される方法で通貨値をチェックするには、ノード条件で代わりに完全な SpEL 式構文 `entities['sys-currency']?.value` を使用します。

## @sys-date と @sys-time エンティティー
{: #system-entities-sys-date-time}

`@sys-date` システム・エンティティーにより、`金曜`、`今日`、または `11 月 1 日`といった言及が抽出されます。このエンティティーの値には、推測される対応日付が「yyyy-MM-dd」という形式のストリング (例えば、「2016-11-21」) として保管されます。 日付の欠落した要素 (「11 月 21 日」に対する年など) は現在日付値を使用してシステムによって拡張されます。

**注:** - 英語ロケールの場合のみ、日付入力のデフォルト・システム動作は MM/DD/YYYY です。 最初の 2 つの数値が 12 より大きい場合にのみ、DD/MM/YYYY に変わります。 保管される値は、形式「yyyy-MM-dd」のままです。

`@sys-time` システム・エンティティーによって、`午後 2 時`、`4 時`、または `15:30` といった言及が抽出されます。 このエンティティーの値には、時刻が「HH:mm:ss」という形式のストリングとして保管されます。 例えば、「13:00:00」です。

### 日時の言及
{: #system-entities-date-time-mentions}

日時の言及 (`今`や`今から 2 時間後`など) は、`@sys-date` と `@sys-time` という 2 つの個別のエンティティー言及として抽出されます。 これら 2 つの言及は、完全な日時の言及にまたがる同じリテラル・ストリングを共有していることを除き、相互にリンクしていません。

`月曜の午後 4 時`のような複数語の日時の言及も、@sys-date と @sys-time の 2 つの言及として抽出されます。 連続して共に言及される場合、これらも完全な日時の言及にまたがる単一のリテラル・ストリングを共有します。

### 日付と時刻の範囲
{: #system-entities-date-time-ranges}

`週末`、`来週`、または`月曜から金曜まで`といった日付範囲の言及は、範囲の開始と終了を示す 1 ペアの `@sys-date` エンティティー言及として抽出されます。 同様に、`2 時から 3 時まで`のような時刻範囲の言及も、開始時刻と終了時刻を示す 2 つの `@sys-time` エンティティーとして抽出されます。 ペアの 2 つのエンティティーは、完全な日付または時刻の範囲言及に対応するリテラル・ストリングを共有します。

### `最後の`および`次の`日時
{: #system-entities-last-next}

一部のロケールでは、「最後の月曜」のような句は、前の週の月曜のみを指定するために使用されます。 対照的に、他のロケールでは、月曜だった最後の日を指定するために「最後の月曜」が使用され、この月曜は同じ週の場合も前の週の場合もあります。

例として、6 月 16 日金曜の場合、一部のロケールでの「最後の月曜」は 6 月 12 日または 6 月 5 日のいずれかを示し、他のロケールでは 6 月 5 日 (前の週) のみを示します。 「次の月曜」のような句にもこれと同じ論理が当てはまります。

{{site.data.keyword.conversationshort}} サービスでは、「最後の」日および「次の」日は、直近の最後および次の日を示すものとして扱われ、同じ週の場合も前の週の場合もあります。

「最後の 3 日間」や「これから 4 時間」のような時間の句の場合も、論理は同等です。 例えば、「これから 4 時間」の場合、結果は 2 つの `@sys-time` エンティティー、つまり、現在時刻と、現在時刻の 4 時間後の時刻になります。

### タイム・ゾーン
{: #system-entities-time-zones}

現在時刻に関連する日付や時刻の言及は、選択されているタイム・ゾーンを基準にして解決されます。 デフォルトでは、これは UTC (GMT) です。 つまり、デフォルトでは、UTC と異なるタイム・ゾーンにある REST API クライアントでは、現在の UTC 時刻に基づいて抽出される`今`の値が監視されます。

オプションで、REST API クライアントは、コンテキスト変数 `$timezone` としてローカル・タイム・ゾーンを追加できます。 このコンテキスト変数は、すべてのクライアント要求で送信する必要があります。 例えば、`$timezone` 値は、`America/Los_Angeles`、`EST`、`UTC` などにする必要があります。 サポートされるタイム・ゾーンの完全なリストについては、[サポートされるタイム・ゾーン](/docs/services/assistant?topic=assistant-time-zones)を参照してください。

`$timezone` 変数が指定されている場合、@sys-date と @sys-time 言及の相対値は、UTC の代わりにクライアント・タイム・ゾーンに基づいて計算されます。

#### タイム・ゾーンに関連する言及の例
{: #system-entities-time-zone-examples}

- 今
- 2 時間後
- 今日
- 明日
- 2 日後

### 認識される形式
{: #system-entities-sys-date-time-formats}

- November 21
- 10:30
- at 6 pm
- this weekend

### 戻り値
{: #system-entities-sys-date-time-returns}

入力が `November 21` の場合、@sys-date では以下の値が返されます。

| 属性               | タイプ   | `November 21` の戻り値 |
|-------------------------|--------|---------------------------:|
| @sys-date.literal       | ストリング |                November 21 |
| @sys-date               | ストリング |                20xx-11-21 *|
| @sys-date.location      | 配列 |                     [0,11]  |
| @sys-date.calendar_type | ストリング |                  GREGORIAN |

- @sys-date では、日付は常に yyyy-MM-dd 形式で返されます。
- \* 次に一致する日付が返されます。 今年のその日付が既に過ぎている場合、来年の日付が返されます。

入力が `at 6 pm` の場合、@sys-time では以下の値が返されます。

| 属性               | タイプ   | `at 6 pm` の戻り値 |
|-------------------------|--------|-----------------------:|
| @sys-time.literal       | ストリング |                at 6 pm |
| @sys-time               | ストリング |               18:00:00 |
| @sys-time.location      | 配列 |                   [0,7]|
| @sys-time.calendar_type | ストリング |              GREGORIAN |

- @sys-time では、時刻は常に HH:mm:ss 形式で返されます。

日付と時刻の値の処理については、[日付と時刻](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-time)のメソッドのリファレンスを参照してください。
{: tip}

## @sys-location エンティティー
{: #system-entities-sys-location}

**ベータ、[サポートされる言語](/docs/services/assistant?topic=assistant-language-support) のトピックに示されている言語のみ**: @sys-location システム・エンティティーでは、ユーザー入力から場所の名前 (国、州、県、市、町など) が抽出されます。 エンティティーの値は、場所のシステム標準値ではありません。

### 認識される形式
{: #system-entities-sys-location-formats}

- Boston
- U.S.A.
- New South Wales

ストリング値の処理については、[ストリング](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings)のメソッドのリファレンスを参照してください。
{: tip}

## @sys-number エンティティー
{: #system-entities-sys-number}

@sys-number システム・エンティティーでは、数字または単語のいずれかを使用して書き込まれた数値が検出されます。 どちらの場合も、数値が返されます。

### 認識される形式
{: #system-entities-sys-number-formats}

- 21
- twenty one
- 3.13

### メタデータ
{: #system-entities-sys-number-metadata}

- `.numeric_value` - 整数またはダブルの標準数値

### 戻り値
{: #system-entities-sys-number-returns}

入力が `twenty` または `1,234.56` の場合、@sys-number では以下の値が返されます。

| 属性                   | タイプ   | `twenty` の戻り値 | `1,234.56` の戻り値 |
|-----------------------------|--------|-------------------|------------------------:|
| @sys-number               | ストリング | 20                |                 1234.56 |
| @sys-number.literal       | ストリング | twenty            |                1,234.56 |
| @sys-number.location      | 配列 |  [0,6]             |                    [0,8]|
| @sys-number.numeric_value | 数値 | 20                |                 1234.56 |

入力がスペイン語で `veinte` または `1.234,56` の場合、@sys-number では以下の値が返されます。

| 属性                   | タイプ   | `veinte` の戻り値 | `1.234,56` の戻り値 |
|-----------------------------|--------|-----------------------|------------------------:|
| @sys-number               | ストリング | 20                    |                 1234.56 |
| @sys-number.literal       | ストリング | veinte                |                1.234,56 |
| @sys-number.location      | 配列  | [0,6]                 |                   [0,8] |
| @sys-number.numeric_value | 数値 | 20                    |                 1234.56 |

サポートされる他の言語でも同等の結果が得られます。

### @system-number 使用のヒント
{: #system-entities-sys-number-usage-tips}

- ノード条件として @sys-number エンティティーを使用している場合に、ユーザーが値としてゼロを指定すると、値 0 は数値として適切に認識されますが、条件は偽として評価され、関連する応答を適切に返すことができません。 ゼロが適切に処理される方法で数値をチェックするには、ノード条件で代わりに完全な SpEL 式構文 `entities['sys-number']?.value` を使用します。

- 条件で数値を比較するために @sys-number を使用する場合、必ず数値自体の存在のチェックを別に組み込んでください。 数値が検出されない場合、@sys-number はヌルと評価され、その結果、数値が存在しない場合でも比較では真と評価されることがあります。

  例えば、`@sys-number<4` を単独で使用しないでください。数値が検出されない場合に `@sys-number` はヌルと評価されるからです。 ヌルは 4 より小さいので、数値が存在しないにもかかわらず、条件は真と評価されます。

  代わりに、`@sys-number AND @sys-number<4` を使用してください。 数値が存在しない場合、最初の条件は偽と評価され、その結果、全体の条件も適切に偽と評価されます。

数値の処理については、[数値](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-numbers)のメソッドのリファレンスを参照してください。
{: tip}

## @sys-percentage エンティティー
{: #system-entities-sys-percentage}

@sys-percentage システム・エンティティーでは、発話内でパーセント記号や単語 `percent` を伴って表現されているパーセンテージが検出されます。 どちらの場合も、数値が返されます。

### 認識される形式
{: #system-entities-sys-percentage-formats}

- 15%
- 10 percent

### メタデータ
{: #system-entities-sys-percentage-metadata}

`.numeric_value`: 整数またはダブルの標準数値

### 戻り値
{: #system-entities-sys-percentage-returns}

入力が `1,234.56%` の場合、@sys-percentage では以下の値が返されます。

| 属性                     | タイプ   | `1,234.56%` の戻り値 |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | ストリング |                  1234.56 |
| @sys-percentage.literal       | ストリング |                1,234.56% |
| @sys-percentage.location      | 配列  |                    [0,9] |
| @sys-percentage.numeric_value | 数値 |                  1234.56 |

入力がスペイン語で `1.234,56%` の場合、@sys-currency では以下の値が返されます。

| 属性                     | タイプ   | `1.234,56%` の戻り値 |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | ストリング |                  1234.56 |
| @sys-percentage.literal       | ストリング |                1.234,56% |
| @sys-percentage.location      | 配列  |                    [0,9] |
| @sys-percentage.numeric_value | 数値 |                  1234.56 |

サポートされる他の言語でも同等の結果が得られます。

### @system-percentage 使用のヒント
{: #system-entities-sys-percentage-usage-tips}

- パーセンテージ値は、@sys-number エンティティーのインスタンスとしても認識されます。 パーセンテージ値と数値の両方をチェックするために別個の条件を使用している場合は、パーセンテージをチェックする条件を、数値をチェックする条件よりも上に置きます。

- ノード条件として @sys-percentage エンティティーを使用している場合に、ユーザーが値として `0%` を指定すると、値はパーセンテージとして適切に認識されますが、条件はパーセンテージの 0% ではなく数値のゼロとして評価されます。したがって、予期される応答が返されません。 ゼロ・パーセントが適切に処理される方法でパーセンテージをチェックするには、代わりに、ノード条件に完全な SpEL 式構文 `entities['sys-percentage']?.value` を使用します。

- `1-2%` のような値を入力すると、システム・エンティティーとして値 `1%` と `2%` が返されます。 索引は 1% と 2% の間の範囲全体で、両方のエンティティーが同じ索引を持ちます。

## @sys-person エンティティー
{: #system-entities-sys-person}

**ベータ、[サポートされる言語](/docs/services/assistant?topic=assistant-language-support) のトピックに示されている言語のみ**: @sys-person システム・エンティティーでは、ユーザー入力から名前が抽出されます。 名前は個別に認識されます。したがって「Joe」は「Joseph」としては扱われず、その逆も同様です。 エンティティーの値は、名前のシステム標準値ではありません。

### 認識される形式
{: #system-entities-sys-person-formats}

- Ronald
- Jane Doe
- Vijay

ストリング値の処理については、[ストリング](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings)のメソッドのリファレンスを参照してください。
{: tip}
