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

# 권장사항
이 페이지는 시스템 개선 방법에 대한 권장사항을 제공합니다.
{: shortdesc}

![권장사항 탭](images/RecommendTop.png)

이 기능은 베타 전용입니다.
{: tip}

이 기능은 프리미엄 사용자만 사용할 수 있습니다.
{: tip}

사용자가 작업공간에서 나눈 대화를 분석하고 시스템의 현재 훈련 데이터와 응답의 확실성을 고려하여 작업공간을 쉽고 효율적으로 개선하기 위해 수행할 수 있는 조치가 제공됩니다.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/scMu66AvZtY" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

권장사항은 야간에 생성되며 많은 양의 사용자 메시지(예: 50개가 넘는)를 필요로 합니다.
{: tip}

## 기존 인텐트 개선
이 권장사항에는 사용자가 입력했으나 시스템이 인식하지 못하는 개별 구를 가져와 각 구에 대한 인텐트를 선택할 수 있도록 제공하는 일이 포함됩니다. 이렇게 하면 사용자가 말한 내용을 이해하는 데 도움이 됩니다.

**시작**을 클릭하여 인텐트 식별을 시작하십시오.
![기존 인텐트 개선 페이지](images/rec_improve_intent.png)

**기존 인텐트 개선**을 입력하거나 종료할 때 진행 표시줄에 하루 동안의 총 구의 수 중 현재 세션에서 조치를 취한 구의 수가 표시됩니다. 종료하고 다시 입력하는 경우, 진행 표시줄이 다시 `empty` 상태에서 시작되지만, 이전 작업이 유실되었다는 것을 의미하지는 않습니다. 단지 현재 세션의 진행상태에 포함되지 않습니다.

제공된 목록에서 구에 대한 최상의 인텐트를 선택하거나 *관련 없음으로 표시*를 선택하십시오. **저장**을 클릭하자마자 구는 인텐트에 예제로 추가됩니다(훈련 데이터로 추가됨).

*다음으로 건너뛰기* 단추를 사용하면 현재 구를 건너뛰고 다음 구로 이동할 수 있습니다. 같은 날에 **기존 인텐트 개선**을 종료하고 다시 입력하면 건너뛴 구가 다시 표시되지 않지만 나중에 다시 나타날 수 있습니다.

![기존 인텐트 편집 개선 페이지](images/rec_improve_intent2.png)
