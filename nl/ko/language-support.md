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

# 지원되는 언어
{: #language-support}

{{site.data.keyword.conversationshort}} 서비스는 나열된 언어를 지원합니다. 서비스의 개별 기능은 다소 차이가 있지만 각 언어에 지원됩니다.
{: shortdesc}

다음 표에서 언어 및 기능 지원 레벨은 다음 코드로 표시됩니다.

- **GA** - 기능이 일반적으로 사용 가능하며 이 언어에 지원됩니다. 일반적으로 사용 가능하더라도 계속 업데이트될 수 있습니다.
- **베타** - 기능이 베타 릴리스로만 지원되며 이 언어로 일반적으로 사용 가능하게 되기 전까지 테스트가 진행됩니다.
- **N/A** - 기능을 이 언어로 사용할 수 없음을 표시합니다.

첫 번째 표에는 두 번째 및 세 번째 표에 표시된 인텐트 및 엔티티와 관련된 기능을 제외한 모든 기능의 지원 레벨이 표시되어 있습니다.

**표 1. 기능 지원 세부사항**

| 언어 | **[인텐트](/docs/services/assistant?topic=assistant-intents)**, **[엔티티](/docs/services/assistant?topic=assistant-entities)** 및 **[대화](/docs/services/assistant?topic=assistant-dialog-build) 정의** | **검색** |
|:---:|:---:|:---:|
| **영어(en)**                   | GA | GA |
| **아랍어(ar)**                    | GA | NA |
| **중국어(간체)(zh-cn)**   | GA | 베타 |
| **중국어(간체)(zh-tw)**  | 베타 | 베타 |
| **체코어(cs)**                     | GA | 베타 |
| **네덜란드어(nl)**                     | GA | 베타 |
| **프랑스어(fr)**                    | GA | 베타 |
| **독일어(de)**                    | GA | 베타 |
| **이탈리아어(it)**                   | GA | 베타 |
|**일본어(ja)**                  | GA | 베타 |
|**한국어(ko)**                    | GA | 베타 |
|**포르투갈어(브라질)(pt-br)** | GA | 베타 |
|**스페인어(es)**                   | GA | 베타 |
{: caption="기능 지원 세부사항" caption-side="top"}

**표 2. 인텐트 기능 지원 세부사항**

| 언어 | **[절대 스코어링 및 '관련 없음'으로 표시'](/docs/services/assistant?topic=assistant-intents#intents-mark-irrelevant)** | **[컨텐츠 카탈로그](/docs/services/assistant?topic=assistant-catalog)** |**[사용자 예제 권장사항](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations)** |
|:---:|:---:|:---:|:---:|
| **영어(en)**                   | GA | GA | 베타 |
| **아랍어(ar)**                    | 베타 | GA | N/A |
| **중국어(간체)(zh-cn)**   | GA | N/A | N/A |
| **중국어(간체)(zh-tw)**  | 베타 | N/A | N/A |
| **체코어(cs)**                     | GA | N/A | N/A |
| **네덜란드어(nl)**                     | GA | N/A | N/A |
| **프랑스어(fr)**                    | GA | GA | N/A |
| **독일어(de)**                    | GA | GA | N/A |
| **이탈리아어(it)**                   | GA | GA | N/A |
| **일본어(ja)**                  | GA | GA | N/A |
| **한국어(ko)**                    | GA | N/A | N/A |
| **포르투갈어(브라질)(pt-br)** | GA | GA | N/A |
| **스페인어(es)**                   | GA | GA | N/A |
{: caption="인텐트 기능 지원 세부사항" caption-side="top"}

**표 3. 엔티티 기능 지원 세부사항**

| 언어 | **시스템 엔티티([숫자](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number), [통화](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-currency), [백분율](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-percentage), [날짜, 시간](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time))** | **[엔티티 유사 일치](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching)** | **[컨텍스트 엔티티](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based)** | **[동의어 추천](/docs/services/assistant?topic=assistant-entities#entities-synonyms)**
|:---|:---:|:---:|:---:|:---:|
| **영어(en)**                   | GA, 베타([위치](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-location), [개인](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-person)) | GA | 베타 | GA |
| **아랍어(ar)**                    | 베타 |GA(오타만) | N/A | N/A |
| **중국어(간체)(zh-cn)**   | GA | N/A | N/A | N/A |
| **중국어(간체)(zh-tw)**  | 베타 | N/A | N/A | N/A |
| **체코어(cs)**                     | GA | GA(오타만) | N/A | N/A |
| **네덜란드어(nl)**                     | GA | GA(오타만) | N/A | N/A |
| **프랑스어(fr)**                    | GA | GA(오타만) | N/A | GA |
| **독일어(de)**                    | GA | GA(오타만) | N/A | N/A |
| **이탈리아어(it)**                   | GA | GA(오타만) | N/A | N/A |
| **일본어(ja)**                  |GA | GA(오타만) | N/A | GA |
| **한국어(ko)**                    | GA | GA(오타만) | N/A | N/A |
| **포르투갈어(브라질)(pt-br)** | GA | GA(오타만) | N/A | N/A |
| **스페인어(es)**                   | GA | GA(오타만) | N/A | GA |
{: caption="엔티티 기능 지원 세부사항" caption-side="top"}

**참고:** {{site.data.keyword.conversationshort}} 서비스는 언급된 대로 여러 언어를 지원하지만 도구 인터페이스 자체(설명, 레이블 등)는 영어로 되어 있습니다. 지원되는 모든 언어는 입력될 수 있으며 영어 인터페이스를 통해 훈련될 수 있습니다.

**GB18030 준수**: GB18030은 중국 시장에서 사용할 확장 코드 페이지를 지정하는 중국 표준입니다. 이 코드 페이지 표준은 중국 국가 정보 기술 표준 기술위원회가 2001년 9월 1일 이후 중국 시장에 출시되는 모든 소프트웨어 애플리케이션에 대해 GB18030을 사용하도록 정했기 때문에 소프트웨어 업계에 중요합니다. {{site.data.keyword.conversationshort}} 서비스는 이 인코딩을 지원하고 GB18030-준수 인증을 받았습니다.

## 스킬 언어 변경
{: #language-support-change-language}

스킬이 작성된 후에는 해당 언어를 수정할 수 없습니다. 스킬간의 지원되는 언어를 변경해야 하는 경우 사용자가 스킬을 다운로드해야 합니다. 그런 다음 JSON 특성 `language`를 검색하고 결과 JSON 파일을 텍스트 편집기에서 편집하십시오.

`language` 특성은 스킬의 원래 언어로 설정되어야 합니다. 예를 들어, 영어는 `en`입니다. 원하는 언어(프랑스어의 경우 `fr`, 독일어의 경우 `de` 등)로 이 특성 값을 변경하여 수정하십시오. JSON 파일의 변경사항을 저장하고 수정된 파일을 {{site.data.keyword.conversationshort}} 서비스 인스턴스로 가져오십시오.

## 양방향 언어 구성
{: #language-support-configure-bi-directional}

양방향 언어(예: 아랍어)의 경우 스킬 환경 설정을 적절하게 변경할 수 있습니다. 스킬 타일에서 *조치* 드롭 다운 메뉴를 선택하고 **양방향 환경 설정**을 선택하십시오(이 옵션은 양방향 언어로 설정된 스킬에만 사용 가능).

![양방향 환경 설정](images/bidi_prefs.png)

스킬에 대한 다음 옵션 중에서 선택하십시오.

- **GUI 방향**: 그래픽 사용자 인터페이스에서 단추 또는 메뉴와 같은 요소의 레이아웃 방향을 지정합니다. `LTR`(왼쪽에서 오른쪽) 또는 `RTL`(오른쪽에서 왼쪽)을 선택하십시오. 지정되지 않은 경우 도구가 웹 브라우저 GUI 방향 설정을 따릅니다.
- **텍스트 방향**: 입력된 텍스트의 방향을 지정합니다. `LTR`(왼쪽에서 오른쪽) 또는 `RTL`(오른쪽에서 왼쪽)을 선택하거나, 시스템 설정에 따라 텍스트 방향을 자동으로 선택하는 `Auto`을 선택하십시오. `None` 옵션은 왼쪽에서 오른쪽으로 텍스트를 표시합니다.
- **숫자 양식 지정**: 일반적인 숫자를 제공할 때 사용할 숫자 양식을 지정합니다. `Nominal`, `Arabic-Indic` 또는 `Arabic-European`에서 선택하십시오. `None` 옵션은 서양 숫자를 표시합니다.
- **달력 유형**: 스킬 UI에서 필터링 날짜를 선택하는 방법을 지정합니다. `Islamic-Civil`, `Islamic-Tabular`, `Islamic-Umm al-Qura` 또는 `Gregorian`을 선택하십시오.

  이 설정은 "시험 사용" 패널에 적용되지 않습니다.
  {: note}

![양방향 옵션](images/bidi_opts.png)

선택을 완료한 후 **업데이트**를 클릭하여 저장하고 스킬 타일로 돌아가십시오.

## 악센트 문자에 대한 작업
{: #language-support-accents}

대화식 설정에서 사용자는 {{site.data.keyword.conversationshort}} 서비스와 상호작용하는 동안 악센트를 사용하거나 사용하지 않을 수 있습니다. 이와 같이 악센트 버전의 단어 및 비악센트 버전의 단어는 인텐트 발견 및 엔티티 인식에 대해 동일하게 처리될 수 있습니다.

하지만 스페인어와 같은 일부 언어의 경우 몇 가지 악센트가 엔티티의 의미를 바꿀 수 있습니다. 따라서 엔티티 발견에 대해 원래 엔티티에 내재적으로 악센트가 있을 수 있지만 서비스는 비악센트 버전의 동일한 엔티티를 일치시킬 수 있습니다. 단, 신뢰도 점수가 약간 낮아집니다.

예를 들어, 악센트가 있고 "barrer"(쓸다)라는 동사의 과거 시제에 해당하는 단어 "barrió"에 대해 서비스는 단어 "barrio"(이웃)를 일치시킬 수도 있습니다. 단, 신뢰도가 약간 낮아집니다.

시스템은 정확한 일치의 엔티티에 가장 높은 신뢰도 점수를 제공합니다. 예를 들어, `barrió`가 훈련 세트에 있는 경우 `barrio`는 발견되지 않고, `barrio`가 훈련 세트에 있는 경우에는 `barrió`가 발견되지 않습니다.

적절한 문자와 악센트를 사용하여 시스템을 훈련시킬 수 있습니다. 예를 들어, `barrió`를 응답으로 예상하는 경우 훈련 세트에 `barrió`를 넣어야 합니다.

악세트 표시가 아니지만 예를 들어, 스페인 문자 `ñ` 대 문자 `n`(예: "uña" 대 "una")를 사용하는 단어에 동일한 내용이 적용됩니다. 이 경우 문자 `ñ`은 단순히 악센트가 있는 `n`이 아닙니다.

고객이 적절한 악센트를 사용하지 않거나 단어 철자를 잘못 쓸 것으로 생각되는 경우(예: `ñ` 대신 `n` 입력) 유사 일치를 사용할 수 있습니다. 또는 훈련 예제에 명시적으로 이를 포함할 수 있습니다.
