---
title: 유사 모델을 사용하여 자사 데이터에서 판매 재고량을 확장합니다.
description: 이 자습서에서는 유사 대상을 만들고 전환 세그먼트에 확장으로 판매할 수 있도록 유사 모델을 설정하고 사용하는 데 수행해야 하는 단계를 안내합니다.
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
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 0%

---

# 유사 모델을 사용하여 자사 데이터에서 판매 재고량을 확장합니다. {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

이 자습서에서는 유사 항목을 설정하고 사용하는 데 필요한 단계를 살펴봅니다 [!UICONTROL Models]와 같은 규칙을 사용하여 새 유사 대상을 만들고 전환 세그먼트에 대한 확장으로 판매할 수 있습니다.

## 사용 사례 세부 사항 {#use-case-details}

컨텐츠 게시자입니다. 사이트의 변환기에 대해 이미 인벤토리를 판매한 경우 해당 기회가 종료될 것으로 생각할 수 있습니다. AAM 유사 항목 입력 [!UICONTROL Models]. 이 기능을 사용하면, 매진 재고량을 더 늘릴 수 있고, 또한 아직 전환하지 않았을 수 있지만 전환한 사람처럼 보이는 사람들을 판매할 수도 있습니다. 이 대상 세그먼트는 일반적으로 실제 전환자보다 적게 판매되지만, 사이트에 광고를 배치하려는 광고주에게 추가 대상 옵션을 제공하여 하단에 추가할 수 있습니다. 이 사용 사례의 추가적인 이점은 자사 데이터에서 이 모델을 실행하는 데 비용이 들지 않는다는 것입니다.

이 자습서의 단계는 다음과 같습니다.

1. 이상적인 사용자(전환) 트레이트 또는 세그먼트 식별/만들기
1. 이 전환 트레이트/세그먼트를 기본 항목으로 사용하여 모델을 만듭니다
1. 선택 [!UICONTROL First party] 모델의 데이터 소스 및 모델 실행
1. 만들기 [!UICONTROL Algorithmic Trait] 모델 결과에서 트레이트를 세그먼트에 추가합니다.
1. 관심 있는 광고주에게 세그먼트를 제공하여 전환 세그먼트 매출을 확장합니다

## 이상적인 사용자(전환) 트레이트 또는 세그먼트를 식별하거나 만들 수 있습니다 {#identify-create-an-ideal-user-conversion-trait-or-segment}

사람들이 사이트에서 DO를 하도록 하려는 것은 무엇입니까? 전환 이벤트란? 물론 사이트 유형/세로 및 조직 목표에 따라 이 질문에 대한 다양한 답변이 있습니다. 어떤 경우든, AAM에서 이러한 기준을 충족한 방문자에 대한 트레이트를 만드는 것이 일반적입니다.

이 사용 사례에서는 변환자인 사용자를 위해 인벤토리를 판매했으므로 이 사용 사례에서 이미 가정합니다. 그러나 이 자습서를 위해 나머지 사용 사례에 대해 참조로 논의하는 것이 좋습니다.

또한 이벤트를 사용하여 트레이트를 만들 때 트레이트에 참여해야 하는 것보다 더 많은 사용자를 수집하지 않도록 기억해야 하는 주요 문제가 있습니다. 다음 비디오를 시청하여 큰 주목을 받으십시오. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**참고:** 위의 비디오에서 제가 보여주는 예는 Adobe Analytics이 있다고 가정합니다. 분명히, 이것은 사실이 아닐 수도 있습니다. GA(Google Analytics)이 있는 경우 AAM에 데이터를 전송하는 데 사용할 수 있는 모듈이 있습니다(자세한 내용은 [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html))이고, 사이트의 전환 활동이 GA에 의해 AAM으로 전송되는 경우, 그것에서 전환 트레이트를 만들 수 있습니다. 다른 Analytics 솔루션(또는 Analytics 솔루션이 없음)이 있는 경우에도 DIL 코드 및 를 통해 데이터를 AAM에 보낼 수 있습니다. `submit` 함수 등 (자세한 내용은 [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)). 그런 다음 사이트에서 전환 활동이 수행될 때 전송된 데이터를 기반으로 전환 트레이트를 만듭니다.

## 자사 데이터에서 유사 모델 만들기 {#creating-a-look-alike-model-from-first-party-data}

이 단계에서는 [!UICONTROL First Party] 유사 모델. 즉, 기본 트레이트/세그먼트에 자사 전환 트레이트/세그먼트를 사용할 수 있을 뿐만 아니라(대부분의 모델에 대해 일반적일 수도 있음), 변환기와 같은 더 많은 사용자를 위한 자사 데이터 풀을 살펴볼 것입니다. 제2자 데이터 또는 타사 데이터는 확인하지 않습니다.

이 사용 사례에서는 전환자인 것처럼 보이지만 아직 전환되지 않은 사용자의 세그먼트를 사이트에 만들어 관심 있는 광고주에게 이 유사 세그먼트를 판매할 수 있도록 하기 때문에 중요합니다.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 알고리즘 트레이트 만들기 {#creating-an-algorithmic-trait}

다음으로, [!UICONTROL Algorithmic Trait]로 설정되면 해당 모델의 결과를 사용할 수 있습니다. 트레이트를 만들지 않으면 모델은 무용지물입니다. 따라서 모델이 실행된 후 트레이트 대화 상자로 이동하여 를 만들어야 합니다 [!UICONTROL Algorithmic Trait]. 다음 비디오는 이를 통해 몇 가지 팁을 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 오퍼 [!UICONTROL Algorithmic Segment] 광고주에게 {#offering-the-algorithmic-segment-to-advertisers}

만든 후에 [!UICONTROL Algorithmic Trait]로 지정하는 새 세그먼트를 만들어 데이터를 활성화할 수 있습니다(트레이트를 활성화할 수는 없지만, [!UICONTROL Algorithmic Trait] 세그먼트를 활성화(사용)할 수 있습니다.

유사 모델에서 높은 점수를 받은 자사 방문자 세그먼트를 만들었다면(예: 전환자처럼 보이지만 아직 전환하지 않은 사람), 사이트에서 실제 전환자의 인벤토리를 모두 판매한 후에도 사이트의 광고주에게 이 세그먼트를 제공할 수 있습니다. 이는 대상을 확장하고 유사 항목을 사용하여 추가 수입을 계속 볼 수 있는 좋은 방법입니다 [!UICONTROL Models] Audience Manager에서 확인하십시오.
