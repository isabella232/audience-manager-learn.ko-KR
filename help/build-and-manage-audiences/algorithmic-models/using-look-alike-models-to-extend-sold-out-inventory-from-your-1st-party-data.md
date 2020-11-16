---
title: 유사 모델을 사용하여 자사 데이터에서 판매 재고 확장
description: 이 자습서에서는 유사한 신규 고객을 만들고 전환 세그먼트에 대한 익스텐션으로 판매할 수 있도록 유사 모델을 설정하고 사용하는 데 필요한 단계를 살펴봅니다.
feature: algorithmic models
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
kt: 1688
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '825'
ht-degree: 0%

---


# 유사 기능을 사용하여 데이터 [!UICONTROL Models] 에서 판매 재고 [!UICONTROL First Party] 확장 {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

이 튜토리얼에서는 유사 대상을 설정하고 사용하기 위해 수행해야 하는 단계 [!UICONTROL Models]를 단계별로 살펴봅니다. 그러면 유사 대상을 만들고 전환 확장으로 판매할 수 있습니다 [!UICONTROL segment].

## 사용 사례 세부 정보 {#use-case-details}

콘텐츠 게시자입니다. 사이트의 변환기에 대한 재고가 이미 매진된 경우 기회가 종료될 수 있습니다. AAM 유사 항목 입력 [!UICONTROL Models]. 이 기능을 사용하면 판매 재고를 더 확대할 수 있으며, 아직 전환하지 않았지만 전환된 사람처럼 보이거나 행동하는 사람들을 판매할 수도 있습니다. 이 대상은 [!UICONTROL segment] 일반적으로 실제 전환자보다 적게 판매되지만 사이트에 광고를 배치하려는 광고주에게 추가 대상 옵션을 제공하여 수익에 추가할 수 있습니다. 이 사용 사례의 추가적인 이점은 퍼스트 파티 데이터에서 이 모델을 실행하는 데 비용이 들지 않는다는 것입니다.

이 자습서의 단계는 다음과 같습니다.

1. 이상적인 사용자 식별/만들기(전환) [!UICONTROL trait] 또는 [!UICONTROL segment]
1. 이 전환 [!UICONTROL model] / [!UICONTROL trait][!UICONTROL segment] 를 기본 항목으로 만들기
1. 에서 [!UICONTROL First party] 데이터 소스 [!UICONTROL model] 를 선택하고 [!UICONTROL model]
1. 결과에서 알고리즘 만들기 [!UICONTROL Trait] 를 [!UICONTROL model] [!UICONTROL trait] 통해 [!UICONTROL segment]
1. 관심 있는 광고주 [!UICONTROL segment] 에게 전환 [!UICONTROL segment] 판매 연장

## 이상적인 사용자 식별/만들기(전환) [!UICONTROL trait] 또는 [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

사이트에서 사람들이 DO를 하도록 하려는 이유는 무엇입니까? 전환 이벤트는 무엇입니까? 물론 사이트 유형/수직 및 조직 목표에 따라 이 질문에 대한 다양한 답변이 있습니다. 어떤 경우든 AAM에서는 이러한 기준을 충족한 방문자를 [!UICONTROL trait] 위한 방문자를 만드는 것이 일반적입니다.

이 사용 사례에서는 이미 가정된 건데, 전환자인 사용자에 대해 재고가 바닥났기 때문입니다. 그러나 이 튜토리얼에서는 나머지 사용 사례에 대한 참조용으로 이 자습서를 사용하는 것이 좋습니다.

또한 이벤트를 사용하여 만들 때 [!UICONTROL traits]는 사용자가 더 많은 사용자를 수집하지 않도록 주의해야 하는 주요 사항이 있습니다. [!UICONTROL trait] 다음 비디오를 시청하십시오.:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**참고:** 위의 비디오에서 제가 보여주는 예제는 여러분이 Adobe Analytics을 가지고 있다고 가정합니다. 물론 그럴 수는 없다. Google Analytics(GA)가 있는 경우 데이터를 AAM으로 보내는 데 사용할 수 있는 모듈이 있습니다( [설명서](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)참조). 사이트의 전환 활동이 AAM으로 GA로 전송되는 경우 이 모듈에서 전환 특성을 만들 수 있습니다. 다른 분석 솔루션(또는 분석 솔루션 없음)이 있는 경우에도 DIL 코드 및 함수 등을 통해 데이터를 AAM으로 보낼 수 `submit` 있습니다. ( [설명서](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)참조). 그런 다음 사이트에서 전환 활동이 수행될 때 전송된 데이터를 기반으로 전환 트레이트를 만듭니다.

## 데이터에서 유사 [!UICONTROL Model] 만들기 [!UICONTROL First Party] {#creating-a-look-alike-model-from-first-party-data}

이 단계에서 유사한 [!UICONTROL First Party] 모양을 만들 것입니다 [!UICONTROL Model]. 즉, Adobe는 기본 [!UICONTROL first party] / [!UICONTROL trait]에 대한 전환[!UICONTROL segment] / [!UICONTROL trait]을 사용할 수 있을 뿐만 아니라[!UICONTROL segment] ( [!UICONTROL models] 어쨌든 대부분의 [!UICONTROL first party] 경우 일반일 수 있음), Adobe는 변환자와 같은 사용자를 위한데이터 풀만 조사할 것입니다. 우리는 [!UICONTROL second party] 데이터나 데이터를 보지 않을 [!UICONTROL third party] 것입니다.

이 사용 사례에서는, Adobe가 사이트에 변환기 [!UICONTROL segment] 와 비슷하지만 아직 변환되지 않은 사용자를 만들기 때문에 관심 있는 광고주에게 유사한 룩을 판매할 수 [!UICONTROL segment] 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 알고리즘 만들기 [!UICONTROL Trait] {#creating-an-algorithmic-trait}

그 다음 알고리즘 [!UICONTROL Trait]을 만들어야 결과를 사용할 수 [!UICONTROL model] 있습니다. 창조 [!UICONTROL trait]가 없으면 [!UICONTROL model] 쓸모가 없다. 실행 후 [!UICONTROL model] 대화 상자 [!UICONTROL trait] 로 이동하여 알고리즘 코드를 만들어야 합니다 [!UICONTROL Trait]. 다음 비디오에서는 몇 가지 팁을 보여 줍니다.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 광고주에 알고리즘 [!UICONTROL Segment] 제공 {#offering-the-algorithmic-segment-to-advertisers}

알고리즘 [!UICONTROL Trait]을 만들고 나면 새로운 알고리즘 [!UICONTROL segment] 을 만들어 데이터를 활성화할 수 있습니다. 즉, 데이터 [!UICONTROL trait]를 활성화할 수 없고 알고리즘[!UICONTROL trait] 을 사용하여 새 알고리즘 [!UICONTROL segment] 을 만들면 해당 알고리즘을 활성화(사용)할 수 [!UICONTROL Trait] [!UICONTROL segment]있습니다.

유사 항목 [!UICONTROL segment] 에서 높은 점수를 받은 [!UICONTROL first party] 방문자(즉, 변환자 같지만 아직 전환하지 못한 방문자)를 만든 후에는 사이트에서 실제 변환기의 모든 목록을 판매한 후라도 사이트의 광고주에게 이 [!UICONTROL model] [!UICONTROL segment] 를 제공할 수 있습니다. Audience Manager의 유사 기능을 사용하여 고객을 확장하고 추가 매출을 계속 확인할 수 [!UICONTROL Models] 있는 좋은 방법입니다.
