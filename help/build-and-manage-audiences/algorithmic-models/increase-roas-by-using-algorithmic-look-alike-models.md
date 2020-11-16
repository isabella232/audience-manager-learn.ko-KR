---
title: Audience Manager에서 알고리즘(유사) 모델을 사용하여 ROAS 증가
description: Audience Manager의 유사 모델링의 실제 장점은 기준 고객을 품질, 2차 및 3차 데이터 소스의 새로운 사용자 세트로 확대하여 보려는 경우 발생합니다. 이 자습서에서는 이 데이터에서 모델을 만드는 단계를 알아봅니다.
feature: algorithmic models
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
kt: 1849
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---


# Audience Manager에서 알고리즘(유사)을 사용하여 ROAS [!UICONTROL Models] 증가 {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Manager의 유사 기능 [!UICONTROL Modeling] 을 사용하면 기준선 고객을 품질과 새로운 사용자 집합으로 확장할 수 있으므로 실제 효과를 낼 수 [!UICONTROL second party] [!UICONTROL third party] [!UICONTROL data sources]있습니다. 이 자습서에서는 이 데이터를 만드는 데 필요한 단계를 [!UICONTROL model] 알아봅니다.

## Audience Marketplace [!UICONTROL Second Party] 에서 데이터 스트림 활성화 또는 [!UICONTROL Third Party] 활성화 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

유사한 [!UICONTROL second party] 데이터와 [!UICONTROL third party] 데이터를 사용하기 위해 먼저 Audience Manager 인터페이스에 이 데이터를 활성화해야 [!UICONTROL model]합니다. Adobe에는 사용자가 선택할 수 있는 많은 수의 [!UICONTROL second party] 데이터 [!UICONTROL third party] 공급자가 있습니다. Audience Marketplace을 통해 AAM의 셀프 서비스 인터페이스에서 사용할 수 있습니다. Audience Marketplace으로 이동하여 여러 가능성을 탐색합니다. 다음 비디오에서는 데이터 제공업체의 가격을 커밋하기 전에 조직에서 가장 유용할 데이터를 잠글 수 있도록 무료 &quot;구매하기 전에 시험버전&quot; 스트림을 사용하는 방법을 비롯하여 이러한 방법을 보여 줍니다.

또한 사용할 데이터 제공업체를 조사하고 결정하는 데 도움이 되는 유용한 리소스가 바로 여기에 있습니다 [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 이상적인 사용자 식별/만들기(전환) [!UICONTROL trait] 또는 [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

사이트에서 사람들이 DO를 하도록 하려는 이유는 무엇입니까? 전환 이벤트는 무엇입니까? 물론 사이트 유형/수직 및 조직 목표에 따라 이 질문에 대한 다양한 답변이 있습니다. 어떤 경우든 AAM에서는 이러한 기준을 충족하는 방문자를 [!UICONTROL trait] 위한 방문자를 만드는 것이 일반적입니다.

아래 비디오에서는 전환 [!UICONTROL trait]을 만드는 방법을 살펴봅니다. 이 전환 방법은 이 튜토리얼을 계속 진행하고 유사한 모양을 만드는 과정에서 원하는 것입니다 [!UICONTROL model].

또한 Adobe Analytics 이벤트를 사용하여 만들 때 [!UICONTROL traits]는 사용자가 더 많은 사용자를 수집하지 않도록 주의해야 하는 주요 사항이 있습니다. [!UICONTROL trait] 다음 비디오를 시청하십시오.:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**참고:** 위의 비디오에서 제가 보여주는 예제는 여러분이 Adobe Analytics을 가지고 있다고 가정합니다. 물론 그럴 수는 없다. Google Analytics(GA)가 있는 경우 데이터를 AAM으로 보내는 데 사용할 수 있는 모듈이 있습니다( [설명서](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)참조). 사이트의 전환 활동이 AAM으로 GA로 전송되는 경우, 이 모듈로부터 전환을 만들 수 [!UICONTROL trait] 있습니다. 다른 분석 솔루션(또는 분석 솔루션 없음)이 있는 경우에도 DIL 코드 및 함수 등을 통해 데이터를 AAM으로 보낼 수 `submit` 있습니다. ( [설명서](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)참조). 그런 다음 전환 활동이 사이트에서 수행될 때 전송된 데이터를 [!UICONTROL trait] 기반으로 전환을 만듭니다.

## 데이터 [!UICONTROL Model] 또는 [!UICONTROL Second Party] 데이터에서 유사 항목 [!UICONTROL Third Party] 만들기 {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

위의 단계를 완료한 후에는 알고리즘(유사)을 만들 준비가 되었습니다 [!UICONTROL Model]. 이 설정을 [!UICONTROL model]설정할 때 전환 [!UICONTROL trait] 을 기본 [!UICONTROL trait] (복제하려는 주요 방문자)으로 사용할 것이며 활성화된 [!UICONTROL third party] 데이터 스트림을 가져올 사용자 풀로 사용할 것입니다.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 중요한 우수 사례 {#an-important-best-practice}

Audience Manager에서 알고리즘 [!UICONTROL model] 을 만들 때, 우리는 분명히 가능한 효과적인 방법 [!UICONTROL model] 을 원합니다. 기본 [!UICONTROL model] / [!UICONTROL traits] 의 구성원들이 모두 [!UICONTROL trait]속해 있다는 모든 것을 고려하고[!UICONTROL segment] 있으므로, 모든 사람들이 [!UICONTROL model] a/S에 속해 [!UICONTROL trait]있는 경우에는 도움이 되지[!UICONTROL segment]않습니다. 따라서 사이트에 온 모든 사람 또는 귀하 [!UICONTROL traits] 로부터 광고를 받은 모든 사람과 같은 슈퍼 일반 [!UICONTROL data source] 이 있는 경우 소유권이 속한 광고가 사용자 [!UICONTROL data sources] 의 제품 내에 포함되지 않아야 [!UICONTROL model]합니다. 이 글의 사용 사례에서는 그럴 것 같지 않습니다. 왜냐하면 우리는 새로운 룩알리스를 위한 [!UICONTROL third party] 데이터를 찾는 것에 집중하고 있지만, 어쨌든 언급할 가치가 있고, 알고리즘 모두를 적용할 수 있기 때문입니다 [!UICONTROL models].

## 알고리즘 만들기 [!UICONTROL Trait] {#creating-an-algorithmic-trait}

그 다음, 알고리즘 [!UICONTROL Trait]을 만들어 그 결과를 [!UICONTROL model] 사용할 수 있어야 합니다. Without creating a [!UICONTROL trait], the model is housed. 따라서 실행 후 [!UICONTROL model] 대화 상자로 이동하여 알고리즘 [!UICONTROL trait] 을 만들어야 합니다 [!UICONTROL Trait]. 다음 비디오에서는 몇 가지 팁을 보여 줍니다.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## 데이터 [!UICONTROL Segment] 에서 [!UICONTROL Model] 데이터 만들기 및 DSP으로 보내기 {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

알고리즘 [!UICONTROL Trait]을 만들고 나면 새로운 알고리즘 [!UICONTROL segment] 을 만들어 데이터를 활성화할 수 있습니다. 즉, 데이터 [!UICONTROL trait]를 활성화할 수 없고 알고리즘[!UICONTROL trait] 을 사용하여 새 [!UICONTROL segment] 알고리즘 [!UICONTROL Trait] 을 만들면 해당 알고리즘을 활성화(사용)할 수 [!UICONTROL segment]있습니다.

이 알고리즘 [!UICONTROL segment] 에서 만든 사용자 [!UICONTROL trait]는 사이트에 이미 전환된 사용자와 같은 잠재 고객을 확보하고 있습니다. Audience Manager의 모든 DSP [!UICONTROL segment] 에 [!UICONTROL destinations] 매핑할 수 있습니다. 일반적인 공개 사이트보다 사이트 전환 가능성이 높은 유사 마케팅을 타겟팅하여 광고 비용에 대한 수익률을 높일 수 있습니다. 행운을 빌어요
