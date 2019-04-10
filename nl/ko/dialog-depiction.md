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

# 대화 설명
{: #dialog-depiction}

![예제 컨텐츠가 있는 샘플 대화 트리](images/dialog-depiction-full.png)

이 다이어그램은 그래픽 사용자 인터페이스 대화 편집기 도구로 빌드된 대화 트리의 모형을 표시합니다. 여기에는 두 개의 루트 대화 노드가 포함되어 있습니다. 일반적인 대화 트리에는 더 많은 노드가 있을 수 있지만 이 설명에서는 노드의 서브세트 모양을 한 눈에 살펴볼 수 있습니다.

- 첫 번째 루트 노드는 인텐트 값을 조건으로 합니다. 이 노드에는 각각 엔티티 값을 조건으로 하는 두 개의 하위 노드가 있습니다. 두 번째 하위 노드는 두 개의 응답을 정의합니다. 컨텍스트 변수의 값이 조건에 지정된 값과 일치하는 경우 첫 번째 응답이 사용자에게 리턴됩니다. 그렇지 않으면 두 번째 응답이 리턴됩니다.

  이 표준 유형의 노드는 특정 주제에 대한 질문을 캡처한 후 루트 응답에서 하위 노드가 처리하는 후속 질문을 묻는 데 유용합니다. 예를 들어, 할인에 대한 사용자 질문을 인식하고 사용자가 회사에 특수 할인 약정이 있는 단체의 구성원인지 여부에 대한 후속 질문을 할 수 있습니다. 또한 하위 노드가 단체 멤버십과 관련된 질문에 대한 사용자의 답변을 기반으로 다른 응답을 제공합니다.

- 두 번째 루트 노드는 슬롯이 있는 노드입니다. 이 노드도 인텐트 값을 조건으로 합니다. 사용자로부터 수집할 각 정보 부분마다 하나씩, 슬롯 세트를 정의합니다. 각 슬롯은 사용자로부터 답변을 도출하기 위해 질문합니다. 프롬프트에 대한 사용자의 응답에서 특정 엔티티 값을 찾은 후 해당 값을 슬롯 컨텍스트 변수에 저장합니다.

  이 유형의 노드는 사용자 대신 트랜잭션을 수행하는 데 필요할 수 있는 세부사항을 수집하는 데 유용합니다. 예를 들어, 사용자의 목적이 항공권을 예약하는 것이면 슬롯이 출발지 및 도착지 정보, 여행 날짜 등을 수집할 수 있습니다.
