---
title: 유사 모델을 사용하여 자사 데이터에서 판매 재고 확장
description: 이 자습서에서는 유사 대상을 만들고 전환 세그먼트에 확장으로 판매할 수 있도록 유사 모델을 설정하고 사용하는 데 수행해야 하는 단계를 살펴봅니다.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '822'
ht-degree: 0%

---

# 유사 [!UICONTROL Models]을 사용하여 [!UICONTROL First Party] 데이터에서 매진 인벤토리를 확장합니다. {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

이 자습서에서는 새로운 유사 대상을 만들고 전환 [!UICONTROL segment]에 확장으로 판매할 수 있도록 유사 [!UICONTROL Models] 을 설정하고 사용하는 데 수행해야 하는 단계를 안내합니다.

## 사용 사례 세부 사항 {#use-case-details}

컨텐츠 게시자입니다. 사이트의 변환기에 대해 이미 인벤토리를 판매한 경우 해당 기회가 종료될 것으로 생각할 수 있습니다. AAM 유사 [!UICONTROL Models]을 입력합니다. 이 기능을 사용하면, 매진 재고량을 더 연장할 수 있고, 또한 아직 전환하지 않았을 수 있지만 전환한 사람들처럼 보이는, 행동하고 있는 사람들의 대상도 팔 수 있습니다. 이 대상 [!UICONTROL segment]은 일반적으로 실제 전환자보다 적게 판매되지만, 사이트에 광고를 배치하려는 광고주에게 추가 대상 옵션을 제공하여 하단에 추가할 수 있습니다. 이 사용 사례의 추가적인 이점은 자사 데이터에서 이 모델을 실행하는 데 비용이 들지 않는다는 것입니다.

이 자습서의 단계는 다음과 같습니다.

1. 이상적인 사용자(전환) [!UICONTROL trait] 또는 [!UICONTROL segment] 식별/만들기
1. 이 전환 [!UICONTROL trait]/[!UICONTROL segment]을 기본 항목으로 사용하여 [!UICONTROL model]을 만듭니다
1. [!UICONTROL model]에서 [!UICONTROL First party] 데이터 소스를 선택하고 [!UICONTROL model]를 실행합니다.
1. [!UICONTROL model] 결과에서 알고리즘 [!UICONTROL Trait]을(를) 만들고 [!UICONTROL trait]를 [!UICONTROL segment]에 추가합니다.
1. 관심 있는 광고주에게 [!UICONTROL segment] 매출 전환 [!UICONTROL segment] 을 제안합니다.

## 이상적인 사용자(전환) [!UICONTROL trait] 또는 [!UICONTROL segment] 식별/만들기 {#identify-create-an-ideal-user-conversion-trait-or-segment}

사람들이 사이트에서 DO를 하도록 하려는 것은 무엇입니까? 전환 이벤트란? 물론 사이트 유형/세로 및 조직 목표에 따라 이 질문에 대한 다양한 답변이 있습니다. 어떤 경우든, AAM에서 이러한 기준을 충족한 방문자에 대해 [!UICONTROL trait]을 만드는 것이 일반적입니다.

이 사용 사례에서는 변환자인 사용자를 위해 인벤토리를 판매했으므로 이 사용 사례에서 이미 가정합니다. 그러나 이 자습서를 위해 나머지 사용 사례에 대해 참조로 논의하는 것이 좋습니다.

또한 이벤트를 사용하여 [!UICONTROL traits]을(를) 만들 때 [!UICONTROL trait]에 참여해야 하는 것보다 더 많은 사용자를 수집하지 않도록 기억해야 하는 중요한 문제가 있습니다. 다음 비디오를 시청하여 큰 주목을 받으십시오. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**참고:**  위의 비디오에서 내가 보여주는 예는 Adobe Analytics이 있다고 가정합니다. 분명히, 이것은 사실이 아닐 수도 있습니다. GA(Google Analytics)이 있는 경우 AAM에 데이터를 전송하는 데 사용할 수 있는 모듈이 있습니다( [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html) 참조). 사이트의 전환 활동이 GA별로 AAM으로 전송되는 경우 이 모듈에서 전환 트레이트를 생성할 수 있습니다. 다른 Analytics 솔루션이 있거나 Analytics 솔루션이 없는 경우에도 DIL 코드 및 `submit` 함수 등을 통해 데이터를 AAM에 보낼 수 있습니다. ( [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html) 참조). 그런 다음 사이트에서 전환 활동이 수행될 때 전송된 데이터를 기반으로 전환 트레이트를 만듭니다.

## [!UICONTROL First Party] 데이터에서 유사 [!UICONTROL Model] 만들기 {#creating-a-look-alike-model-from-first-party-data}

이 단계에서는 [!UICONTROL First Party] 유사 [!UICONTROL Model]을 만듭니다. 즉, 기본 [!UICONTROL trait]/[!UICONTROL segment]에 [!UICONTROL first party]/[!UICONTROL trait] 변환을 사용할 뿐만 아니라, 대부분의 [!UICONTROL models]에 정상일 수도 있습니다. 변환기와 같은 사용자를 위해 [!UICONTROL first party] 데이터 풀만 볼 수 있습니다. [!UICONTROL segment] [!UICONTROL second party] 또는 [!UICONTROL third party] 데이터는 보지 않습니다.

이 사용 사례에서는 전환자처럼 보이지만 아직 전환되지 않은 사용자의 [!UICONTROL segment]을 만들려고 하므로 이 유사 [!UICONTROL segment]을 관심 있는 광고주에게 판매할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 알고리즘 만들기 [!UICONTROL Trait] {#creating-an-algorithmic-trait}

다음으로 [!UICONTROL model] 결과를 사용할 수 있도록 알고리즘 [!UICONTROL Trait]을 만들어야 합니다. [!UICONTROL trait]을 만들지 않으면 [!UICONTROL model]은(는) 사용할 수 없습니다. 따라서 [!UICONTROL model] 이 실행되면 [!UICONTROL trait] 대화 상자로 이동하여 알고리즘 [!UICONTROL Trait]을 만들어야 합니다. 다음 비디오는 이를 통해 몇 가지 팁을 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 광고주에게 알고리즘 [!UICONTROL Segment] 제공 {#offering-the-algorithmic-segment-to-advertisers}

알고리즘 [!UICONTROL Trait]을(를) 만든 후에는 새 [!UICONTROL segment]을(를) 만들어 데이터를 활성화할 수 있습니다([!UICONTROL trait]를 활성화할 수는 없지만, 알고리즘 [!UICONTROL Trait]을(를) 사용하여 새 one-[!UICONTROL trait] [!UICONTROL segment]를 만드는 대신, [!UICONTROL segment]를 활성화(사용)할 수 있습니다.

유사 [!UICONTROL model]에서 높은 점수를 받은 [!UICONTROL first party] 방문자의 [!UICONTROL segment]을(를) 만들었으면(예: 전환자인 것처럼 보이지만 아직 전환하지 않은 사람) 사이트의 광고주에게 이 [!UICONTROL segment]을 제공할 수 있습니다. 이는 사이트의 실제 전환자의 모든 인벤토리를 판매한 후에도 가능합니다. 이는 이 대상을 확장하고 Audience Manager에서 유사 [!UICONTROL Models]을 사용하여 추가 수입을 계속 볼 수 있는 좋은 방법입니다.
