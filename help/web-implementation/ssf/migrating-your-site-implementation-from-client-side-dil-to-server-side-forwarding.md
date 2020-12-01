---
title: 사이트의 AAM 구현을 클라이언트측 DIL에서 서버측 포워딩으로 마이그레이션
description: 이 자습서는 Adobe Audience Manager(AAM)과 Adobe Analytics을 모두 보유하고 있고, 현재 "DIL"(Data Integration Library) 코드를 사용하여 페이지에서 AAM으로 히트를 보내고 있으며, 또한 페이지에서 Adobe Analytics으로 히트를 전송하는 경우에 적용됩니다. 이 두 솔루션을 모두 가지고 있고 이 두 솔루션은 모두 Adobe Experience Cloud의 일부이므로, Analytics 데이터 수집 서버가 사이트측 코드를 페이지에서 AAM으로 추가 히트를 전송하는 대신 실시간으로 사이트 분석 데이터를 Audience Manager으로 전달하는 "SSF(Server-Side Forwarding)"를 설정하는 최상의 방법을 따를 수 있습니다. 이 자습서에서는 이전 "클라이언트측 DIL" 구현에서 최신 "서버측 전달" 방법으로 전환하는 단계를 안내합니다.
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
translation-type: tm+mt
source-git-commit: 133279f589bd58aef36a980c2b7248ae00fd9496
workflow-type: tm+mt
source-wordcount: '2319'
ht-degree: 0%

---


# 사이트의 AAM 구현을 [!DNL Client-Side] DIL에서 [!DNL Server-Side Forwarding] {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}로 마이그레이션

이 자습서는 Adobe Audience Manager(AAM)과 Adobe Analytics을 모두 보유하고 있고, 현재 &quot;DIL&quot;([!DNL Data Integration Library]) 코드를 사용하여 페이지에서 AAM으로 히트를 보내고 있으며, 페이지에서 Adobe Analytics으로 히트를 전송하는 경우에 적용됩니다. 이 두 솔루션을 모두 가지고 있고 이 두 솔루션은 모두 Adobe Experience Cloud의 일부이므로, [!DNL client-side] 코드가 페이지에서 AAM으로 추가 히트를 보내지 않고, 실시간으로 사이트 분석 데이터를 Audience Manager으로 전달할 수 있는 &quot;[!DNL Server-Side Forwarding](SSF)을 설정하는 최상의 방법을 따를 수 있습니다. [!DNL Analytics] 이 자습서에서는 이전 &quot;[!DNL Client-Side DIL]&quot; 구현에서 최신 &quot;[!DNL Server-Side forwarding]&quot; 메서드로 전환하는 단계를 안내합니다.

## [!DNL Client-Side] (DIL) vs.  [!DNL Server-Side] {#client-side-dil-vs-server-side}

Adobe Analytics 데이터를 AAM으로 가져오는 두 가지 방법을 비교하고 대조할 때 먼저 다음 이미지에서 차이점을 시각화하는 데 도움이 될 수 있습니다.

![클라이언트측과 서버측](assets/client-side_vs_server-side_aam_implementation.png)

### [!DNL Client-side] DIL 구현  {#client-side-dil-implementation}

이 방법을 사용하여 Adobe Analytics 데이터를 AAM으로 가져오는 경우 웹 페이지에서 오는 히트가 두 개 있음을 의미합니다.[!DNL Analytics]으로 이동하고, AAM으로 가는 경우(웹 페이지에서 [!DNL Analytics] 데이터를 복사한 후) [!UICONTROL Segments] 는 AAM에서 페이지로 돌아가며, 여기에서 개인화 등에 사용할 수 있습니다. 이것은 &quot;기존&quot; 구현으로 간주되며 더 이상 권장되지 않습니다.

이 방법이 우수 사례를 따르지 않는다는 점을 제외하면 이 방법을 사용할 때의 단점은 다음과 같습니다.

* 한 개의 히트가 아닌 페이지에서 오는 두 개의 히트
* [!UICONTROL Server-Side Forwarding] aam 대상을 실시간으로 공유하는 데  [!DNL Analytics]필요하므로  [!DNL Client-side] 구현에서는 이 기능(및 향후 다른 기능)을 허용하지 않습니다.

AAM 구현의 [!UICONTROL Server-Side Forwarding] 메서드로 이동하는 것이 좋습니다.

### [!UICONTROL Server-Side Forwarding] 구현 {#server-side-forwarding-implementation}

위의 이미지에 표시된 것처럼, 히트는 웹 페이지에서 Adobe Analytics으로 옵니다. [!DNL Analytics] 그런 다음 해당 데이터를 AAM에 실시간으로 전달하면 방문자는 AAM [!UICONTROL traits] 와  [!UICONTROL segments]로 평가됩니다. 히트가 페이지에서 바로 온 것처럼 표시됩니다.

[!UICONTROL Segments] 는 동일한 실시간 히트에서 다시  [!DNL Analytics]반환되며, 이 히트는 개인화 등을 위해 웹 페이지에 응답을 전달합니다.

서버 측 전달으로 이동하는 데 시간이 지장을 주지 않습니다. Audience Manager과 [!DNL Analytics]이 모두 있는 사람은 이 구현 방법을 사용하는 것이 좋습니다.

## 두 개의 기본 작업 {#you-have-two-main-tasks}

이 페이지에는 약간의 정보가 있으며, 물론 그것은 모두 중요합니다. 하지만 이 모든 항목은 두 가지 주요 사항으로 요약됩니다.****

1. 코드를 [!DNL Client-Side] DIL 코드에서 [!UICONTROL Server-Side Forwarding] 코드로 변경
1. [!DNL Analytics] [!DNL Admin Console]에서 스위치를 뒤집어서 데이터의 실제 포워드를 시작합니다([!UICONTROL report suite] 단위).

이 두 가지 중 하나를 건너뛰면 SSF가 제대로 작동하지 않습니다. 설정에 대해 이 두 단계를 올바르게 수행할 수 있도록 이 문서에 단계 및 추가 데이터가 추가되었습니다.

## 구현 옵션 {#implementation-options}

[!DNL client-side]에서 [!DNL server-side](으)로 이동할 때 사용할 작업 중 하나가 코드를 새 [!UICONTROL Server-Side Forwarding] 코드로 변경하는 것입니다. 이 작업은 다음 옵션 중 하나를 사용하여 수행됩니다.

* Adobe Experience Platform Launch - 웹 속성에 대한 권장 구현 옵션입니다. [!DNL Launch]이(가) 사용자를 위해 모든 어려운 작업을 수행했기 때문에 이 작업은 매우 쉬운 작업임을 알 수 있습니다.
* 페이지의 경우 - Adobe 시작을 사용하지 않는 경우 새 SSF 코드를 [!DNL appMeasurement.js] 파일 내부의 `doPlugins` 함수에 직접 가져올 수도 있습니다
* 기타 태그 관리자 - 다른 태그 관리자가 [!DNL AppMeasurement] 코드를 저장하는 곳마다 `doPlugins`에 SSF 코드를 계속 저장하므로 이전(페이지에서) 옵션과 동일하게 처리할 수 있습니다

아래의 코드 업데이트 섹션에서 각 항목을 살펴봅니다.

## 구현 절차 {#implementation-steps}

### 0단계:필수 조건:Experience Cloud ID 서비스(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

[!UICONTROL Server-Side Forwarding](으)로 이동하는 기본 전제 조건은 Experience Cloud ID 서비스를 구현한 것입니다. Experience Platform Launch을 사용하는 경우 가장 쉽게 수행할 수 있습니다. 이 경우 ECID 익스텐션을 설치하면 나머지는 그대로 됩니다.

Adobe이 아닌 TMS를 사용하고 있거나 TMS가 전혀 없는 경우 다른 Adobe 솔루션 앞에서 **을(를) 실행하려면 ECID를 구현하십시오.** 자세한 내용은 [ECID 설명서](https://marketing.adobe.com/resources/help/ko_KR/mcvid/)를 참조하십시오. 단, 코드 버전과 관련된 다른 사전 요구 사항만 있으면 됩니다. 다음 단계에서 최신 버전의 코드를 간단히 적용할 수 있으므로 괜찮을 수 있습니다.

>[!NOTE]
>
>구현하기 전에 이 전체 문서를 읽어 보십시오. 아래의 &quot;시간 설정&quot; 섹션에는 *when*&#x200B;에 대해 ECID를 포함하여 각 조각을 구현해야 하는 중요한 정보가 있습니다(아직 구현되지 않은 경우).

### 1단계:DIL 코드 {#step-record-currently-used-options-from-dil-code}에서 현재 사용된 옵션 기록

[!DNL Client-Side] DIL 코드에서 [!UICONTROL Server-Side Forwarding](으)로 이동할 준비가 되면 첫 번째 단계는 사용자 지정 설정 및 AAM으로 전송된 데이터를 포함하여 DIL 코드로 수행하는 모든 것을 식별하는 것입니다. 주목해야 할 사항은 다음과 같습니다.

* [!DNL siteCatalyst.init] DIL 모듈을 사용하는 일반 [!DNL Analytics] 변수 - 이 변수를 걱정할 필요가 없습니다. 이 변수는 일반 [!DNL Analytics] 변수를 전송하기만 하면 되며 SSF가 활성화되면 됩니다.
* 파트너 하위 도메인 - DIL.create 함수에서 `partner` 매개 변수를 기록해 두십시오. 이를 &quot;파트너 하위 도메인&quot; 또는 &quot;파트너 ID&quot;라고도 하며 새 SSF 코드를 배치할 때 필요합니다.
* [!DNL Visitor Service Namespace] - &quot;[!DNL Org ID]&quot; 또는 &quot;[!DNL IMS Org ID]&quot;라고도 하는 경우 새로운 SSF 코드를 설정할 때도 이 기능이 필요합니다. 참고하세요.
* containerNSID, uuidCookie 및 기타 고급 옵션 - SSF 코드에서도 설정할 수 있도록 사용 중인 추가 고급 옵션을 메모해 두십시오.
* 추가 페이지 변수 - 다른 변수가 페이지에서 AAM으로 전송되는 경우(siteCatalyst.init에서 처리하는 일반 [!DNL Analytics] 변수 외에도) SSF(스포일러 경고:을(를) 참조하십시오.[!DNL contextData]

### 2단계:{#step-updating-the-code} 코드 업데이트

위의 &quot;구현 옵션&quot; 섹션에서 [!UICONTROL Server-Side Forwarding]을(를) 구현하는 방법/위치에 대해 여러 옵션이 제공됩니다. 이 섹션이 유효하려면 이 섹션을 두 개의 섹션과 함께 구분해야 합니다. 필요에 대해 가장 잘 설명하는 이 섹션의 방법으로 이동합니다.

#### Adobe Experience Platform Launch {#launch-by-adobe}

Experience Platform Launch의 [!DNL Client-Side] DIL 코드에서 [!UICONTROL Server-Side Forwarding](으)로 구현 옵션을 이동하는 방법에 대한 자세한 내용은 아래 비디오를 시청하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### &quot;On the Page&quot; 또는 Adobe Tag Manager 이외 {#on-the-page-or-non-adobe-tag-manager}

파일 또는 Adobe이 아닌 태그 관리 시스템에 있는 [!DNL Client-Side] DIL 코드에서 [!DNL AppMeasurement] 코드의 [!UICONTROL Server-Side Forwarding]으로 구현 옵션을 이동하는 방법에 대한 자세한 내용은 아래 비디오를 시청하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 3단계:전달 활성화([!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

지금까지 이 자습서에서는 코드를 [!DNL Client-Side DIL] 코드에서 [!UICONTROL Server-Side Forwarding]로 전환하는 데 많은 시간을 보냈습니다. 그게 더 어려운 부분이라 괜찮아요 이 섹션은 매우 간단하지만 코드를 업데이트하는 것만큼 중요합니다. 이 비디오에서는 데이터를 Analytics에서 Audience Manager으로 실제 전달하는 방식을 사용하는 스위치를 뒤집는 방법을 확인할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**참고:** 비디오에 명시된 대로 Experience Cloud 백엔드에 전달 기능이 완전히 구현되려면 최대 4시간이 소요됩니다.

## 시간 설정 {#timing}

다시 말해, [!DNL Client-Side DIL]에서 [!UICONTROL Server-Side Forwarding](으)로 이동하는 두 가지 주요 작업이 있습니다.

1. 코드 업데이트
1. [!DNL Analytics] [!DNL Admin Console]에서 스위치 뒤집기

하지만 문제는, 어떤 것을 먼저 해야 하는가 입니다. 그게 중요한가요? 네, 죄송합니다. 두 가지 질문이었습니다. 하지만 대답은... 상황에 따라 다르며, 그렇다. *can*&#x200B;의 문제이다. 막연한 게 어때요? 그것을 분류합시다. 그러나 먼저 많은 사이트를 보유한 대규모 조직의 경우 발생할 수 있는 추가 질문한번에 다 해야 하나요? 그건 좀 쉬워요 아니 조각마다 할 수 있습니다.:)

### 약간 더 깊이 있는 {#a-little-deeper-dive}

타이밍 및 주문 관련 이유는 다음 몇 가지 기술 사실에서 요약할 수 있는 전달 *정말 효과적인 방식 때문입니다.

* Experience Cloud ID 서비스(ECID)가 구현되어 있고 [!DNL Analytics] [!DNL Admin Console](&quot;스위치&quot;)의 스위치가 켜져 있는 경우, 아직 코드를 업데이트하지 않았더라도 데이터 WILL은 [!DNL Analytics]에서 AAM으로 전달됩니다.
* ECID가 구현되지 않은 경우, 스위치가 켜져 있고 SSF 코드가 있는 경우에도 데이터가 전달되지 않습니다.
* SSF 코드([!DNL Launch]에 있든 페이지에 있든)는 실제로 응답을 처리하며, 물론 마이그레이션을 완료하는 데 필요합니다.
* SSF 스위치가 [!UICONTROL Report Suite]에 의해 활성화되지만 코드가 [!DNL Launch]의 속성에 의해 처리되거나 [!DNL Launch]을(를) 사용하지 않는 경우 [!DNL AppMeasurement] 파일에 의해 처리된다는 점을 기억하십시오

### 우수 사례 {#best-practices}

이러한 기술 세부 사항에 따라 &quot;작업 시기&quot;에 대한 권장 사항이 다음과 같습니다.

#### ECID가 아직 구현되지 않은 경우 {#if-you-do-not-have-ecid-yet-implemented}

1. SSF에 대해 활성화할 각 [!UICONTROL report suite]에 대해 [!DNL Analytics]의 스위치를 뒤집습니다.

   1. ECID가 없기 때문에 포워딩이 아직 시작되지 않습니다.

1. 사이트별로 [!DNL Client-Side DIL]에서 SSF로 코드를 업데이트합니다(위의 다른 섹션에 설명된 대로 페이지 또는 [!DNL Launch]에 있을 수 있음)

   1. 이제 전달(ECID를 추가한 후)이 흐름되며, [!DNL Analytics] 비콘에 대한 적절한 JSON 응답도 수신해야 합니다(자세한 내용은 아래의 유효성 검사 및 문제 해결 섹션 참조).

#### ECID가 구현된 경우 {#if-you-do-have-ecid-implemented}

1. SSF에 대해 활성화할 코드를 DIL에서 SSF [!UICONTROL report suite]로 업데이트할 준비가 되도록 준비하고 계획하십시오.

   1. [!DNL Analytics]에서 스위치를 뒤집어서 SSF를 활성화합니다.

      1. ECID가 활성화되어 있으므로 전달 시작
   1. 가능한 한 빨리 코드를 [!DNL Client-Side DIL]에서 SSF로 업데이트하십시오(위의 다른 섹션에 설명된 대로 [!DNL Launch] 또는 페이지에 있을 수 있음).

      1. [!DNL Analytics] 비콘에 대한 적절한 JSON 응답을 수신해야 합니다(자세한 내용은 아래 유효성 검사 및 문제 해결 섹션 참조).


**참고 1:** 위의 1단계와 2단계 사이에 AAM으로 들어가는 데이터의 중복을 갖게 되므로 이러한 두 단계를 가능한 한 서로 가깝게 하는 것이 중요합니다. 즉, SSF는 [!DNL Analytics]에서 AAM으로 데이터를 전송하기 시작합니다. DIL 코드가 아직 페이지에 있기 때문에 페이지에서 AAM으로 바로 가는 히트도 있기 때문에 데이터가 두 배로 증가합니다. 코드를 DIL에서 SSF로 업데이트하면 그대로 유지됩니다.

**참고 2:** 데이터의 작은 복제보다는 데이터에 약간 차이가 있는 경우는 위의 1단계와 2단계의 순서를 전환할 수 있습니다. 코드를 DIL에서 SSF로 이동하면 스위치를 뒤집어서 [!UICONTROL report suite]에 대한 SSF를 활성화할 때까지 데이터 흐름이 AAM으로 중지됩니다. 일반적으로 고객은 방문자를 [!UICONTROL traits] 및 [!UICONTROL segments]으로 초대하는 데 허비하지 않고 데이터가 두 배 정도 증가하는 것을 선호합니다.

#### 여러 사이트와 [!UICONTROL Report Suites] {#migration-timing-when-you-have-many-sites-and-report-suites}이(가) 있는 마이그레이션 타이밍

이 항목은 다음 내용으로 주요 전략을 요약할 수 있다는 점에서 이전 섹션에 간단히 언급되어 있습니다.

한 번에 하나의 사이트/[!UICONTROL report suite](또는 사이트 그룹/[!UICONTROL report suites])을 마이그레이션합니다.

그러나 몇 가지 가능한 시나리오에 따라 이 문제가 약간 복잡해질 수 있습니다.

* 여러 개의 서로 다른 [!UICONTROL report suites]이 포함된 사이트가 있습니다.
* 일부 사이트(예: 전역 [!UICONTROL report suite])를 포함하는 [!UICONTROL report suite]이 있습니다.
* 하나의 [!DNL Launch] 속성을 사용하여 여러 사이트를 덮습니다.
* 다른 사이트마다 다른 개발 팀이 있습니다

이 물건들 때문에 좀 복잡해질 수 있어요. 가장 좋은 것은 다음과 같습니다.

* 위에서 설명한 내용을 바탕으로 SSF로 마이그레이션하는 전략을 세우는 데 시간을 좀 더 할애하십시오
* [!DNL Launch](또는 단일 [!DNL AppMeasurement] 파일)의 단일 속성이 일반적으로 1 또는 2개의 서로 다른 [!UICONTROL report suites]에 매핑된다는 점을 기준으로 이러한 개별 그룹에서 하나씩 작동하는 계획을 세울 수 있습니다. 이렇게 하면 엔터프라이즈를 SSF로 업데이트할 수 있습니다
* Adobe 컨설팅 팀과 일하는 경우 마이그레이션 계획에 관해 상담해 주십시오. 그러면 필요한 경우 도움을 드릴 수 있습니다

## 유효성 검사 및 문제 해결 {#validation-and-troubleshooting}

[!UICONTROL Server-Side Forwarding]이(가) 작동 중이고 실행되는지 확인하는 주요 방법은 앱에서 오는 Adobe Analytics 히트에 대한 응답을 보는 것입니다.

[!DNL Analytics]에서 Audience Manager으로 데이터의 [!UICONTROL server-side forwarding]을(를) 수행하지 않으면 [!DNL Analytics] 비콘(2x2 픽셀 제외)에 대한 응답이 실제로 없습니다. 그러나 SSF를 수행하는 경우, [!DNL Analytics] 요청과 응답에서 확인할 수 있는 항목이 있는데, 이를 통해 [!DNL Analytics]이(가) Audience Manager과 올바르게 통신하고 있고 히트를 전달하며 응답을 얻을 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

**경고:** 거짓 &quot;성공&quot; - 반응이 있고 모든 것이 작동하는 것처럼 보이는 경우 응답에 &quot;물건&quot; 개체가 있는지 확인합니다. 그렇지 않은 경우 [!DNL "status":"SUCCESS"]이라는 메시지가 표시될 수 있습니다. 이상하게 들릴지 모르지만, 이것은 실제로 그것이 제대로 작동하지 않았다는 증거이다. 이것은 [!DNL Launch] 또는 [!DNL AppMeasurement]에서 코드 업데이트를 완료했지만 [!DNL Analytics] [!DNL Admin Console]의 전달이 아직 완료되지 않았음을 의미합니다. 이 경우 [!UICONTROL report suite]에 대해 [!DNL Analytics] [!DNL Admin Console]에서 SSF를 활성화했는지 확인해야 합니다. 아직 4시간이 지나지 않았다면, 백 엔드에서 필요한 모든 변화를 만드는 데 시간이 오래 걸릴 수 있기 때문에 인내심을 가지세요.

![거짓 성공](assets/falsesuccess.png)

[!UICONTROL Server-Side Forwarding]에 대한 자세한 내용은 [documentation](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html)을 참조하십시오.
