---
title: Audience Manager에서 알고리즘(유사) 모델을 사용하여 ROAS를 늘립니다
description: Audience Manager의 유사 모델링에서 제공하는 진정한 기능은 기준선 대상을 품질로 확장하려 할 때 제공되며, 제2자 데이터 소스와 타사 데이터 소스의 새로운 사용자 집합입니다. 이 자습서에서는 이 데이터에서 모델을 만드는 단계를 알아봅니다.
feature: 알고리즘 모델
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 0%

---

# Audience Manager에서 알고리즘(유사) [!UICONTROL Models]을 사용하여 ROAS를 늘립니다. {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Manager의 유사 [!UICONTROL Modeling]의 진정한 기능은 품질에 따라 기본 대상을 확장하려 할 때 나옵니다. [!UICONTROL second party] 및 [!UICONTROL third party] [!UICONTROL data sources]의 새 사용자 집합을 새로 만드십시오. 이 자습서에서는 이 데이터에서 [!UICONTROL model]을(를) 만드는 데 필요한 단계를 알아봅니다.

## Audience Marketplace에서 [!UICONTROL Second Party] 또는 [!UICONTROL Third Party] 데이터 스트림 활성화 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

유사 [!UICONTROL model]에서 [!UICONTROL second party] 및 [!UICONTROL third party] 데이터를 사용하려면 먼저 이 데이터를 Audience Manager 인터페이스에 활성화해야 합니다. Adobe에는 선택할 수 있는 [!UICONTROL second party] 및 [!UICONTROL third party] 데이터 공급자가 많습니다. AAM의 셀프 서비스 인터페이스에서 Audience Marketplace을 통해 사용할 수 있습니다. Audience Marketplace으로 이동하고 가능성을 살펴봅니다. 다음 비디오에서는 데이터 공급자의 가격을 커밋하기 전에 조직에 가장 유용한 데이터를 잠글 수 있도록 무료 &quot;구매하기 전에 시도&quot; 스트림을 활성화하는 방법을 포함하여 이 방법을 보여 줍니다.

또한 사용할 데이터 공급자를 조사하고 결정하는 데 도움이 되는 유용한 리소스는 [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)입니다.

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 이상적인 사용자(전환) [!UICONTROL trait] 또는 [!UICONTROL segment] 식별/만들기 {#identify-create-an-ideal-user-conversion-trait-or-segment}

사람들이 사이트에서 DO를 하도록 하려는 것은 무엇입니까? 전환 이벤트란? 물론 사이트 유형/세로 및 조직 목표에 따라 이 질문에 대한 다양한 답변이 있습니다. 어떤 경우든, AAM에서 이러한 기준을 충족한 방문자에 대해 [!UICONTROL trait]을 만드는 것이 일반적입니다.

아래 비디오에서는 이 자습서를 계속 진행하고 유사 [!UICONTROL model]을(를) 만들 때 사용할 전환 [!UICONTROL trait]을 만드는 방법을 보여줍니다.

또한 Adobe Analytics 이벤트를 사용하여 [!UICONTROL traits]을(를) 만들 때 [!UICONTROL trait]에 참여해야 하는 것보다 더 많은 사용자를 수집하지 않도록 기억해야 하는 중요한 문제가 있습니다. 다음 비디오를 시청하여 큰 주목을 받으십시오. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**참고:**  위의 비디오에서 내가 보여주는 예는 Adobe Analytics이 있다고 가정합니다. 분명히, 이것은 사실이 아닐 수도 있습니다. Google Analytics(GA)가 있는 경우 AAM에 데이터를 전송하는 데 사용할 수 있는 모듈이 있습니다( [설명서](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html) 참조). 사이트의 전환 활동이 GA에 의해 AAM으로 전송되는 경우 이 모듈에서 전환 [!UICONTROL trait]을 생성할 수 있습니다. 다른 Analytics 솔루션이 있거나 Analytics 솔루션이 없는 경우에도 DIL 코드 및 `submit` 함수 등을 통해 데이터를 AAM에 보낼 수 있습니다. ( [설명서](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html) 참조). 그런 다음 사이트에서 전환 활동이 수행될 때 전송된 데이터를 기반으로 전환 [!UICONTROL trait]을 만듭니다.

## [!UICONTROL Second Party] 또는 [!UICONTROL Third Party] 데이터에서 유사 [!UICONTROL Model] 만들기 {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

위의 단계를 완료하고 이제 알고리즘(유사) [!UICONTROL Model]을 만들 준비가 되었습니다. [!UICONTROL model]을(를) 설정할 때 변환 [!UICONTROL trait]을 기본 [!UICONTROL trait](복제하려는 주요 방문자)로 사용하고, 활성화된 [!UICONTROL third party] 데이터 스트림을 가져올 사용자 풀로 사용합니다.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 중요한 우수 사례 {#an-important-best-practice}

Audience Manager에서 알고리즘 [!UICONTROL model]을(를) 만들 때 분명히 [!UICONTROL model]이(가) 가능한 한 유효했으면 합니다. [!UICONTROL model]이(가) 모두 [!UICONTROL traits][!UICONTROL trait]/[!UICONTROL segment]의 멤버가 속해 있다는 것을 고려하고 있으므로, 모든 사용자가 [!UICONTROL trait]/[!UICONTROL segment]에 있는 경우 [!UICONTROL model]에 도움이 되지 않습니다. 따라서 슈퍼 일반 [!UICONTROL traits](사이트를 방문한 모든 사용자 또는 귀하를 통해 광고를 받은 모든 사용자 등)가 있는 경우, 해당 ID가 속한 [!UICONTROL data source]이 [!UICONTROL model]의 [!UICONTROL data sources]에 포함되어 있지 않은지 확인하십시오. 이 문서의 사용 사례에서는 그렇게 할 것 같지 않습니다. 왜냐하면 Adobe에서는 새로운 영업 기회에 대해 [!UICONTROL third party] 데이터를 찾는 데 집중하고 있지만, 어쨌든 언급할 가치가 있으며, 모든 알고리즘 [!UICONTROL models]에 적용됩니다.

## 알고리즘 만들기 [!UICONTROL Trait] {#creating-an-algorithmic-trait}

다음으로, [!UICONTROL model] 결과를 사용할 수 있도록 알고리즘 [!UICONTROL Trait]을 만들어야 합니다. [!UICONTROL trait]을 만들지 않으면 모델은 사용할 수 없습니다. 따라서 [!UICONTROL model] 이 실행되면 [!UICONTROL trait] 대화 상자로 이동하여 알고리즘 [!UICONTROL Trait]을 만들어야 합니다. 다음 비디오에서는 몇 가지 팁을 살펴봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## [!UICONTROL Model] 데이터에서 [!UICONTROL Segment]을 만들고 DSP에 보내기 {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

알고리즘 [!UICONTROL Trait]을(를) 만든 후에는 새 [!UICONTROL segment]을(를) 만들어 데이터를 활성화할 수 있습니다([!UICONTROL trait]를 활성화할 수는 없지만, 알고리즘 [!UICONTROL Trait]을(를) 사용하여 새 one-[!UICONTROL trait] [!UICONTROL segment]를 만들 수 있으므로 [!UICONTROL segment])를 활성화(사용)할 수 있습니다.

이 알고리즘 [!UICONTROL trait]에서 [!UICONTROL segment]을(를) 만들면 사이트에서 이미 전환한 사람처럼 보이는 잠재 고객을 갖게 됩니다. 이제 이 [!UICONTROL segment]을 Audience Manager의 DSP [!UICONTROL destinations]에 매핑할 수 있습니다. 마케팅을 일반적인 공개 사이트보다 사이트 전환을 더 많이 할 수 있는 그러한 마케팅 전략에 타겟팅하여 광고 투자 수익률을 높일 수 있습니다. 행운을 빌어요!
