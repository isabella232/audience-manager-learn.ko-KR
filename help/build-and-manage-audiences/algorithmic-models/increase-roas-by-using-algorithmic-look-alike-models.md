---
title: Audience Manager에서 알고리즘(유사) 모델을 사용하여 ROAS 증가
description: Audience Manager의 유사 모델링의 실제 장점은 기준 고객을 품질로 확대하려는 경우, 2차 및 3차 데이터 소스의 새로운 사용자 집합을 브랜드에 맞게 확장할 수 있다는 것입니다. 이 자습서에서는 이 데이터에서 모델을 만드는 단계를 알아봅니다.
feature: 알고리즘 모델
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
translation-type: tm+mt
source-git-commit: 6c81fd73d2c5abd646b0d38b6f4eebde837b09f2
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 0%

---


# Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}에서 알고리즘(유사) [!UICONTROL Models]을 사용하여 ROAS 증가

Audience Manager의 유사 모양 [!UICONTROL Modeling]의 진정한 기능은 기준 대상을 품질에 맞게 확장하려는 경우에 제공됩니다. [!UICONTROL second party] 및 [!UICONTROL third party] [!UICONTROL data sources]의 새 사용자 집합을 새로 만듭니다. 이 자습서에서는 이 데이터에서 [!UICONTROL model]을(를) 만드는 데 필요한 단계를 알아봅니다.

## Audience Marketplace {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}에서 [!UICONTROL Second Party] 또는 [!UICONTROL Third Party] 데이터 스트림 활성화

유사 [!UICONTROL model]에서 [!UICONTROL second party] 및 [!UICONTROL third party] 데이터를 사용하려면 먼저 Audience Manager 인터페이스에 이 데이터를 활성화해야 합니다. Adobe에는 선택할 수 있는 [!UICONTROL second party] 및 [!UICONTROL third party] 데이터 공급자가 많이 있습니다. Audience Marketplace을 통해 AAM의 셀프 서비스 인터페이스에서 사용할 수 있습니다. Audience Marketplace으로 이동하여 여러 가능성을 탐색합니다. 다음 비디오에서는 데이터 공급자의 가격을 결정하기 전에 조직에 가장 유용한 데이터를 잠글 수 있도록 무료 &quot;구매하기 전에 시도&quot; 스트림을 사용하는 방법을 비롯하여 이러한 방법을 보여 줍니다.

또한 사용할 데이터 제공업체를 조사하고 결정하는 데 도움이 되는 유용한 리소스는 [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)입니다.

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 이상적인 사용자(전환) [!UICONTROL trait] 또는 [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment} 식별/만들기

사이트에서 사람들에게 DO를 제공하도록 하는 것은 무엇입니까? 전환 이벤트는 무엇입니까? 물론 사이트 유형/수직 및 조직 목표에 따라 이 질문에 대한 다양한 답변이 있습니다. 어떤 경우든 AAM에서는 이러한 기준을 충족하는 방문자에 대해 [!UICONTROL trait]을 만드는 것이 일반적입니다.

아래 비디오에서는 이 튜토리얼을 계속 진행하고 유사한 [!UICONTROL model]을(를) 만들 때 사용할 전환 [!UICONTROL trait]을 만드는 방법을 보여줍니다.

또한 Adobe Analytics 이벤트를 사용하여 [!UICONTROL traits]을(를) 만들 때, [!UICONTROL trait]에 들어가야 하는 사용자 수보다 많은 사용자를 수집하지 않도록 주의해야 하는 주요 정보가 있습니다. 다음 비디오를 통해 많은 정보를 확인하십시오.:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**참고:** 위의 비디오에서 내가 보여 주는 예는 Adobe Analytics이 있다고 가정합니다. 분명히, 이것은 사실이 아닐 것이다. Google Analytics(GA)가 있는 경우 데이터를 AAM으로 보내는 데 사용할 수 있는 모듈이 있습니다([documentation](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html) 참조). 사이트의 전환 활동이 GA로 AAM으로 전송되는 경우 이 모듈에서 전환 [!UICONTROL trait]을 만들 수 있습니다. 다른 분석 솔루션이 있거나 분석 솔루션이 없는 경우에도 DIL 코드와 `submit` 함수 등을 통해 데이터를 AAM에 여전히 보낼 수 있습니다. ([설명서](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html) 참조). 그런 다음 전환 활동이 사이트에서 수행될 때 전송된 데이터를 기준으로 전환 [!UICONTROL trait]을 만듭니다.

## [!UICONTROL Second Party] 또는 [!UICONTROL Third Party] 데이터 {#create-a-look-alike-model-from-2nd-or-3rd-party-data}에서 유사 [!UICONTROL Model] 만들기

위의 단계를 완료한 후 이제 알고리즘(유사) [!UICONTROL Model]을 만들 준비가 되었습니다. [!UICONTROL model]을(를) 설정할 때 전환 [!UICONTROL trait]을 기본 [!UICONTROL trait](복제할 주요 방문자)으로 사용할 것이며, 활성화된 [!UICONTROL third party] 데이터 스트림을 가져올 사용자 풀로 사용할 것입니다.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 중요한 우수 사례 {#an-important-best-practice}

Audience Manager에서 알고리즘 [!UICONTROL model]을 만들 때 분명 [!UICONTROL model]이 가능한 한 효과적이어야 합니다. [!UICONTROL model]이(가) 모두 [!UICONTROL traits]에 대해 사용자의 기본 [!UICONTROL trait]/[!UICONTROL segment]의 구성원이 모두 포함됨을 고려하고 있으므로 모든 사용자가 [!UICONTROL trait]/[!UICONTROL segment]에 있는 경우에는 [!UICONTROL model]에 도움이 되지 않습니다. 따라서 사이트에 온 모든 사람 또는 사용자로부터 광고를 받은 모든 사람과 마찬가지로 매우 일반적인 [!UICONTROL traits](이)가 있는 경우, 속해 있는 [!UICONTROL data source]이 [!UICONTROL model]의 [!UICONTROL data sources]에 포함되지 않아야 합니다. 이 문서의 사용 사례에서는, Adobe가 새로운 Look-Alike를 위해 [!UICONTROL third party] 데이터를 보는 데 주력하고 있지만, 아무튼 언급할 가치가 있으며, ALL of your algorithmic [!UICONTROL models]에 적용됩니다.

## 알고리즘 [!UICONTROL Trait] {#creating-an-algorithmic-trait} 만들기

다음으로 [!UICONTROL model]의 결과를 사용할 수 있도록 알고리즘 [!UICONTROL Trait]을 만들어야 합니다. [!UICONTROL trait]을(를) 만들지 않으면 모델은 무용지물입니다. 따라서 [!UICONTROL model]이(가) 실행된 후 [!UICONTROL trait] 대화 상자로 이동하여 알고리즘 [!UICONTROL Trait]을 만들어야 합니다. 다음 비디오에서는 몇 가지 팁을 보여 줍니다.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## [!UICONTROL Model] 데이터에서 [!UICONTROL Segment]을(를) 만들고 DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}에 보내기

알고리즘 [!UICONTROL Trait]을(를) 만든 후에는 데이터를 활성화할 수 있도록 새 [!UICONTROL segment]을 만들어서 데이터를 활성화할 수 있습니다([!UICONTROL trait]을 활성화할 수 없지만 알고리즘 [!UICONTROL Trait]을 사용하여 새 one-[!UICONTROL trait] [!UICONTROL segment]을 만드는 대신 [!UICONTROL segment])을 활성화(사용)할 수 있습니다.

이 알고리즘 [!UICONTROL trait]에서 [!UICONTROL segment]을(를) 만들었으면 사이트에서 이미 전환된 사람처럼 보이는 잠재적인 클라이언트를 갖게 됩니다. 이제 Audience Manager의 DSP [!UICONTROL destinations]에 이 [!UICONTROL segment]을 매핑할 수 있습니다. 일반 대중뿐만 아니라 사이트에서 전환할 가능성이 높은 유사 마케팅으로 마케팅을 타깃팅하여 광고 지출에 대한 수익률을 높일 수 있습니다. 행운을 빌어요!
