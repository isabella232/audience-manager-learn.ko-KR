---
title: 알고리즘(유사) 모델을 사용하여 ROAS를 늘립니다
description: Audience Manager의 유사 모델링에서 제공하는 진정한 기능은 기준선 대상을 품질로 확장하려 할 때 제공되며, 제2자 데이터 소스와 타사 데이터 소스의 새로운 사용자 집합입니다. 이 자습서에서는 이 데이터에서 모델을 만드는 단계를 알아봅니다.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '909'
ht-degree: 0%

---

# Audience Manager에서 알고리즘(유사) 모델을 사용하여 ROAS를 늘립니다 {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Manager의 유사 잠재력 [!UICONTROL Modeling] 은 품질에 대해 기본 대상을 확장하려 할 때 제2자 데이터 소스 및 타사 데이터 소스의 새 사용자 집합을 브랜딩합니다. 이 자습서에서는 이 데이터에서 모델을 만드는 데 필요한 단계를 알아봅니다.

## Audience Marketplace에서 제2자 데이터 스트림 또는 타사 데이터 스트림 활성화 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

유사 모델에서 제2자 데이터와 타사 데이터를 사용하려면 먼저 이 데이터를 Audience Manager 인터페이스에 활성화해야 합니다. Adobe에는 선택할 수 있는 많은 수의 제2자 데이터 공급자와 타사 데이터 공급자가 있습니다. AAM의 셀프 서비스 인터페이스에서 Audience Marketplace을 통해 사용할 수 있습니다. Audience Marketplace으로 이동하고 가능성을 살펴봅니다. 다음 비디오에서는 데이터 공급자의 가격을 커밋하기 전에 조직에 가장 유용한 데이터를 잠글 수 있도록 무료 &quot;구매하기 전에 시도&quot; 스트림을 활성화하는 방법을 포함하여 이 방법을 보여 줍니다.

또한 사용할 데이터 공급자를 조사하고 결정하는 데 도움이 되는 유용한 리소스는 입니다. [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 이상적인 사용자(전환) 트레이트 또는 세그먼트를 식별하거나 만들 수 있습니다 {#identify-create-an-ideal-user-conversion-trait-or-segment}

사람들이 사이트에서 DO를 하도록 하려는 것은 무엇입니까? 전환 이벤트란? 물론 사이트 유형/세로 및 조직 목표에 따라 이 질문에 대한 다양한 답변이 있습니다. 어떤 경우든, AAM에서 이러한 기준을 충족한 방문자에 대한 트레이트를 만드는 것이 일반적입니다.

아래 비디오에서는 이 자습서를 계속 진행하고 유사 모델을 만들 때 적용할 전환 트레이트를 만드는 방법을 보여줍니다.

또한 Adobe Analytics 이벤트를 사용하여 트레이트를 만들 때 트레이트에 참여해야 하는 것보다 더 많은 사용자를 수집하지 않도록 기억해야 하는 주요 문제가 있습니다. 다음 비디오를 시청하여 큰 주목을 받으십시오. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**참고:** 위의 비디오에서 제가 보여주는 예는 Adobe Analytics이 있다고 가정합니다. 분명히, 이것은 사실이 아닐 수도 있습니다. GA(Google Analytics)이 있는 경우 AAM에 데이터를 전송하는 데 사용할 수 있는 모듈이 있습니다(자세한 내용은 [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html))이고, 사이트의 전환 활동이 GA에 의해 AAM으로 전송되는 경우, 그것에서 전환 트레이트를 만들 수 있습니다. 다른 Analytics 솔루션(또는 Analytics 솔루션이 없음)이 있는 경우에도 DIL 코드 및 를 통해 데이터를 AAM에 보낼 수 있습니다. `submit` 함수 등 (자세한 내용은 [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)). 그런 다음 사이트에서 전환 활동이 수행될 때 전송된 데이터를 기반으로 전환 트레이트를 만듭니다.

## 제2자 데이터 또는 타사 데이터에서 유사 모델 만들기 {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

위의 단계를 완료하고 이제 알고리즘(유사) 모델을 만들 준비가 되었습니다. 모델을 설정할 때 전환 트레이트를 기본 트레이트(복제하려는 주요 방문자)로 사용하고, 활성화된 타사 데이터 스트림을 가져올 사람 풀로 사용합니다.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 중요한 모범 사례 {#an-important-best-practice}

Audience Manager에서 알고리즘 모델을 만들 때, 모델이 가능한 한 효과적이어야 합니다. 기본 트레이트/세그먼트의 구성원이 속하는 모든 트레이트를 모델이 고려하고 있으므로 모든 사용자가 트레이트/세그먼트에 있는 경우 모델에 도움이 되지 않습니다. 따라서 사이트에 방문한 모든 사람 또는 사용자로부터 광고를 받은 모든 사람 등 일반적인 트레이트가 있는 경우, 해당 트레이트가 속한 데이터 소스가 모델의 데이터 소스에 포함되지 않아야 합니다. 이 문서의 사용 사례에서는 그럴 것 같지 않습니다. 왜냐하면 우리는 새로운 룩앤리를 위한 타사 데이터를 찾는 데 집중하고 있지만, 어쨌든 언급할 가치가 있고 모든 알고리즘 모델에 적용됩니다.

## [!UICONTROL Algorithmic Trait] {#creating-an-algorithmic-trait}

다음으로,  [!UICONTROL Algorithmic Trait]로 설정되면 해당 모델의 결과를 사용할 수 있습니다. 트레이트를 만들지 않으면 모델은 무용지물입니다. 따라서 모델이 실행된 후 트레이트 대화 상자로 이동하여 를 만들어야 합니다 [!UICONTROL Algorithmic Trait]. 다음 비디오에서는 몇 가지 팁을 살펴봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## 모델 데이터에서 세그먼트를 만들어 DSP으로 보냅니다 {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

만든 후에 [!UICONTROL Algorithmic Trait]로 지정하는 새 세그먼트를 만들어 데이터를 활성화할 수 있습니다(트레이트를 활성화할 수는 없지만, [!UICONTROL Algorithmic Trait] 세그먼트를 활성화(사용)할 수 있습니다.

이 알고리즘 트레이트에서 세그먼트를 만들면 사이트에서 이미 전환한 사람처럼 보이는 잠재 고객을 갖게 됩니다. 이제 이 세그먼트를 Audience Manager의 DSP 대상에 매핑할 수 있습니다. 마케팅을 일반적인 공개 사이트보다 사이트 전환을 더 많이 할 수 있는 그러한 마케팅 전략에 타겟팅하여 광고 투자 수익률을 높일 수 있습니다.
