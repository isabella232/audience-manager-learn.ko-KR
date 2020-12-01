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


# 유사 항목 [!UICONTROL Models]을 사용하여 [!UICONTROL First Party] 데이터 {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}에서 매진 재고 확장

이 자습서에서는 유사한 신규 대상을 만들고 전환 [!UICONTROL segment]의 익스텐션으로 판매할 수 있도록 Look-Similar [!UICONTROL Models]을 설정하고 사용하는 데 필요한 단계를 살펴봅니다.

## 사용 사례 세부 정보 {#use-case-details}

콘텐츠 게시자입니다. 사이트의 변환기에 대한 재고가 이미 매진된 경우 기회가 종료될 수 있습니다. AAM 유사 항목 [!UICONTROL Models]을 입력합니다. 이 기능을 사용하면 판매 재고를 더 확대할 수 있으며, 아직 전환하지 않았지만 전환된 사람처럼 보이거나 행동하는 사람들을 판매할 수도 있습니다. 이 대상 [!UICONTROL segment]은 일반적으로 실제 변환기보다 적게 팔리지만, 사이트에 광고를 배치하려는 광고주에 대해 추가 대상 옵션을 제공하여 수익에 추가할 수 있습니다. 이 사용 사례의 추가적인 이점은 퍼스트 파티 데이터에서 이 모델을 실행하는 데 비용이 들지 않는다는 것입니다.

이 자습서의 단계는 다음과 같습니다.

1. 이상적인 사용자(전환) [!UICONTROL trait] 또는 [!UICONTROL segment] 식별/만들기
1. 이 전환 [!UICONTROL trait]/[!UICONTROL segment]을 기본 항목으로 사용하여 [!UICONTROL model]을 만듭니다.
1. [!UICONTROL model]에서 [!UICONTROL First party] 데이터 소스를 선택하고 [!UICONTROL model]를 실행합니다.
1. [!UICONTROL model] 결과에서 알고리즘 [!UICONTROL Trait]을 만들고 [!UICONTROL trait]를 [!UICONTROL segment]에 추가합니다.
1. 관심 있는 광고주에게 [!UICONTROL segment]을(를) 제공하여 전환 [!UICONTROL segment] 판매를 연장합니다.

## 이상적인 사용자(전환) [!UICONTROL trait] 또는 [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment} 식별/만들기

사이트에서 사람들이 DO를 하도록 하려는 이유는 무엇입니까? 전환 이벤트는 무엇입니까? 물론 사이트 유형/수직 및 조직 목표에 따라 이 질문에 대한 다양한 답변이 있습니다. 어떤 경우든 AAM에서는 이러한 기준을 충족하는 방문자에 대해 [!UICONTROL trait]을(를) 만드는 것이 일반적입니다.

이 사용 사례에서는 이미 가정된 건데, 전환자인 사용자에 대해 재고가 바닥났기 때문입니다. 그러나 이 튜토리얼에서는 나머지 사용 사례에 대한 참조용으로 이 자습서를 사용하는 것이 좋습니다.

또한 이벤트를 사용하여 [!UICONTROL traits]을(를) 만드는 경우, [!UICONTROL trait]에 비해 더 많은 사용자를 수집하지 않도록 주의해야 하는 주요 정보가 있습니다. 다음 비디오를 시청하십시오.:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**참고:** 위의 비디오에서 내가 보여 주는 예제는 사용자가 Adobe Analytics을 가지고 있다고 가정합니다. 물론 그럴 수는 없다. Google Analytics(GA)가 있는 경우 데이터를 AAM으로 보내는 데 사용할 수 있는 모듈이 있습니다([documentation](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html) 참조). 사이트의 전환 활동이 GA별로 AAM으로 전송되는 경우 이 모듈에서 전환 특성을 만들 수 있습니다. 다른 분석 솔루션(또는 분석 솔루션 없음)이 있는 경우에도 DIL 코드와 `submit` 함수 등을 통해 데이터를 AAM에 보낼 수 있습니다. ([documentation](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html) 참조). 그런 다음 사이트에서 전환 활동이 수행될 때 전송된 데이터를 기반으로 전환 트레이트를 만듭니다.

## [!UICONTROL First Party] 데이터 {#creating-a-look-alike-model-from-first-party-data}에서 유사 [!UICONTROL Model] 만들기

이 단계에서 [!UICONTROL First Party] 유사 항목 [!UICONTROL Model]을 만듭니다. 즉, 기본 [!UICONTROL trait]/[!UICONTROL segment]에 대해 [!UICONTROL first party]/[!UICONTROL segment]을(를) 사용할 수 있을 뿐만 아니라, 변환자와 같은 사용자를 위해 [!UICONTROL first party] 데이터 풀만 검토 중입니다. [!UICONTROL models][!UICONTROL trait] [!UICONTROL second party] 또는 [!UICONTROL third party] 데이터는 보지 않습니다.

이 사용 사례에서는 전환 업체와 비슷하지만 아직 변환되지 않은 사용자의 [!UICONTROL segment]을(를) 만들려고 하므로 관심 있는 광고업체에 이 유사 [!UICONTROL segment]을(를) 판매할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 알고리즘 만들기 [!UICONTROL Trait] {#creating-an-algorithmic-trait}

다음으로 [!UICONTROL model]의 결과를 사용할 수 있도록 알고리즘 [!UICONTROL Trait]을 만들어야 합니다. [!UICONTROL trait]을(를) 만들지 않으면 [!UICONTROL model]은 사용할 수 없습니다. 따라서 [!UICONTROL model]이(가) 실행된 후 [!UICONTROL trait] 대화 상자로 이동하여 알고리즘 [!UICONTROL Trait]을 만들어야 합니다. 다음 비디오에서는 몇 가지 팁을 보여 줍니다.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 광고주에게 알고리즘 [!UICONTROL Segment] 제공 {#offering-the-algorithmic-segment-to-advertisers}

알고리즘 [!UICONTROL Trait]을(를) 만들고 나면 데이터를 활성화할 수 있도록 새 [!UICONTROL segment]을 만들어 데이터를 활성화할 수 있습니다.([!UICONTROL trait]를 활성화할 수는 없지만 알고리즘 [!UICONTROL Trait]로 새 one-[!UICONTROL trait] [!UICONTROL segment]를 만드는 것이 아니라 [!UICONTROL segment]을 활성화(사용)할 수 있습니다.

유사 [!UICONTROL model]에서 높은 점수를 받은 [!UICONTROL first party] 방문자의 [!UICONTROL segment]을(를) 만든 후(즉, 변환기는 비슷하지만 아직 전환하지 않은) 사이트에 있는 실제 변환기의 모든 목록을 판매한 후라도 사이트의 광고주에게 이 [!UICONTROL segment]을(를) 제공할 수 있습니다. Audience Manager의 유사 &lt; a0/>을(를) 사용하여 이 고객을 확장하고 추가 매출을 계속 볼 수 있는 좋은 방법입니다.[!UICONTROL Models]
