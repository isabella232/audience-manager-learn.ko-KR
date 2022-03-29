---
title: 사이트의 Audience Manager 구현을 클라이언트측 DIL에서 서버측 전달로 마이그레이션
description: 사이트의 Audience Manager(AAM) 구현을 클라이언트측 DIL에서 서버측 전달로 마이그레이션하는 방법을 알아봅니다. 이 자습서는 AAM과 Adobe Analytics이 모두 있고 DIL(Data Integration Library) 코드를 사용하여 페이지에서 AAM으로 히트를 전송하며 페이지에서 Adobe Analytics으로 히트를 전송하는 경우에 적용됩니다.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '2333'
ht-degree: 0%

---

# 사이트의 Audience Manager 구현을 클라이언트측 DIL에서 서버측 전달로 마이그레이션 {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

이 자습서는 Adobe Audience Manager(AAM)과 Adobe Analytics이 모두 있고 현재 DIL( )를 사용하여 페이지에서 AAM으로 히트를 보내는 경우에 적용됩니다[!DNL Data Integration Library]) 코드를 채울 수도 있고 페이지에서 Adobe Analytics으로 히트를 보낼 수도 있습니다. 이 두 가지 솔루션을 모두 사용할 수 있으며, 둘 다 Adobe Experience Cloud의 일부이므로 서버측 전달을 활성화하는 우수 사례를 따를 수 있습니다. [!DNL Analytics] 데이터 수집 서버는 클라이언트측 코드가 Audience Manager에서 AAM으로 추가 히트를 전송하도록 하지 않고, 사이트 분석 데이터를 실시간으로 전달하도록 합니다. 이 자습서에서는 이전 클라이언트측 DIL 구현에서 최신 서버측 전달 방법으로 전환하는 단계를 안내합니다.

## 클라이언트측(DIL)과 서버측 {#client-side-dil-vs-server-side}

Adobe Analytics 데이터를 AAM에 가져오는 두 가지 방법을 비교하고 비교할 때, 먼저 다음 이미지에서 차이점을 시각화하는 것이 도움이 될 수 있습니다.

![클라이언트측 서버측](assets/client-side_vs_server-side_aam_implementation.png)

### 클라이언트측 DIL 구현 {#client-side-dil-implementation}

이 방법을 사용하여 Adobe Analytics 데이터를 AAM에 가져오는 경우 웹 페이지에서 두 개의 히트가 있습니다. 하나 이동 [!DNL Analytics]및 AAM으로 이동한 후 [!DNL Analytics] 데이터를 포함할 수 없습니다. [!UICONTROL Segments] AAM에서 페이지로 반환되며, 여기에서 개인화에 사용할 수 있습니다. 이는 이전 구현으로 간주되며 더 이상 권장되지 않습니다.

이 방법이 우수 사례를 따르지 않는다는 사실 외에 이 메서드를 사용할 때의 단점은 다음과 같습니다.

* 한 번의 히트가 아니라 페이지에서 오는 두 개의 히트입니다
* AAM 대상을 에 실시간 공유하려면 서버측 전달이 필요합니다 [!DNL Analytics]따라서 클라이언트측 구현에서는 이 기능(및 향후 다른 기능)을 허용하지 않습니다

AAM 구현의 서버측 전달 방법으로 이동하는 것이 좋습니다.

### 서버 측 전달 구현 {#server-side-forwarding-implementation}

위의 이미지에 표시된 대로, 히트는 웹 페이지에서 Adobe Analytics으로 가져옵니다. [!DNL Analytics] 그런 다음 데이터를 AAM에 실시간으로 전달하며 방문자는 AAM 트레이트로 평가되고 [!UICONTROL segments]히트가 페이지에서 바로 온 것처럼.

[!UICONTROL Segments] 는 동일한 실시간 히트에서 [!DNL Analytics]: 개인화를 위해 웹 페이지에 응답을 전달하는 등

서버측 전달로 이동하는 데 시간이 따르는 단점이 없습니다. Adobe은 Audience Manager과 [!DNL Analytics] 은 이 구현 방법을 사용합니다.

## 두 가지 주요 작업이 있습니다 {#you-have-two-main-tasks}

이 페이지에는 많은 정보가 있으며, 물론 이 모든 것이 중요합니다. 하지만, **요점은 당신이 해야 할 두 가지 주요 사항으로 요약됩니다**:

1. 코드를 클라이언트측 DIL 코드에서 서버측 전달 코드로 변경합니다
1. 에서 스위치 뒤집기 [!DNL Analytics] [!DNL Admin Console] 실제 데이터 전달을 시작하려면(에) [!UICONTROL report suite])

이러한 작업을 건너뛰면 서버 측 전달이 제대로 작동하지 않습니다. 설정에 대해 이러한 두 단계를 올바르게 수행하는 데 도움이 되는 단계 및 추가 데이터가 이 문서에 추가되었습니다.

## 구현 옵션 {#implementation-options}

클라이언트측에서 서버측 전달로 이동할 때 수행할 작업 중 하나는 코드를 새 서버측 전달 코드로 변경하는 것입니다. 이 작업은 다음 옵션 중 하나를 사용하여 수행됩니다.

* Adobe Experience Platform 태그 - Adobe의 웹 속성에 대한 권장 구현 옵션. 플랫폼 태그가 모든 힘든 작업을 수행했기 때문에 이 작업은 쉬운 작업임을 알 수 있습니다.
* 페이지에서 - 새 SSF 코드를 `doPlugins` 함수 내에 있어야 합니다 `appMeasurement.js` 파일(Adobe Launch를 아직 사용하지 않는 경우)
* 다른 태그 관리자 - SSF 코드를 여전히 에 넣듯이 이전(페이지에서) 옵션처럼 처리할 수 있습니다. `doPlugins`, 다른 태그 관리자가 저장하는 위치에 관계없이 [!DNL AppMeasurement] 코드

아래에 있는 각 항목을 살펴보겠습니다 _코드 업데이트_ 섹션을 참조하십시오.

## 구현 단계 {#implementation-steps}

다음 단계에서는 구현에 대해 설명합니다.

### 0단계: 전제 조건: Experience Cloud ID 서비스(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

서버측 전달로 이동하기 위한 기본 전제 조건은 Experience Cloud ID 서비스가 구현된 상태여야 합니다. 이 작업은 Experience Platform Launch을 사용하는 경우 가장 쉽게 수행합니다. 이 경우 ECID 확장을 설치하면 나머지 작업을 수행합니다.

Adobe이 아닌 TMS를 사용하거나 TMS가 없는 경우 ECID를 구현하여 실행하십시오 **이전** 기타 모든 Adobe 솔루션. 자세한 내용은 [ECID 설명서](https://experienceleague.adobe.com/docs/id-service/using/home.html) 자세한 내용 유일한 다른 전제 조건은 코드 버전에 대한 것이므로 다음 단계에서 최신 버전의 코드를 적용하면 됩니다.

>[!NOTE]
>
>구현하기 전에 이 전체 문서를 읽어 보십시오. 아래의 &quot;시간&quot; 섹션에는 *when* ecid(아직 구현되지 않은 경우)를 포함하여 각 조각을 구현해야 합니다.

### 1단계: 현재 사용된 DIL 코드의 옵션을 기록합니다 {#step-record-currently-used-options-from-dil-code}

클라이언트측 DIL 코드에서 서버측 전달로 이동할 준비가 되면 첫 번째 단계는 사용자 지정 설정 및 AAM으로 전송된 데이터를 포함하여 DIL 코드로 수행하는 모든 것을 식별하는 것입니다. 주목할 만한 사항은 다음과 같습니다.

* 일반 [!DNL Analytics] 변수, 사용 `siteCatalyst.init` DIL 모듈 - 이 모듈은 정상 전송을 위한 것이므로 이 모듈에 대해 걱정할 필요가 없습니다 [!DNL Analytics] 변수를 전달하면 서버 측 전달을 사용할 수 있기 때문에 이러한 작업이 수행됩니다.
* 파트너 하위 도메인 - 에서 `DIL.create` 함수 위에 있는 `partner` 매개 변수. 이를 &quot;파트너 하위 도메인&quot; 또는 때로 &quot;파트너 ID&quot;라고 하며, 새 서버측 전달 코드를 배치할 때 필요합니다.
* [!DNL Visitor Service Namespace] - &quot;이라고도 합니다.[!DNL Org ID]&quot; 또는 &quot;[!DNL IMS Org ID],&quot;새로운 서버측 전달 코드를 설정할 때도 이 기능이 필요합니다. 메모를 해 보세요.
* containerNSID, uuidCookie 및 기타 고급 옵션 - 서버측 전달 코드에서도 설정할 수 있도록 사용 중인 추가 고급 옵션을 주목하십시오.
* 추가 페이지 변수 - 일반 변수 외에 다른 변수가 페이지에서 AAM으로 전송되는 경우 [!DNL Analytics] siteCatalyst.init에서 처리하는 변수)에서는 서버측 전달(spoiler alert: via [!DNL contextData] 변수 참조).

### 2단계: 코드 업데이트 {#step-updating-the-code}

in [구현 옵션](#implementation-options) (위의 경우) 서버측 전달을 구현하는 방법과 위치에 대한 여러 옵션이 제공됩니다. 이 섹션이 유효하려면 이러한 섹션으로 분류해야 합니다(두 개가 결합되어 있음). 필요에 대해 가장 잘 설명하는 이 섹션의 메서드로 이동합니다.

#### Adobe Experience Platform 태그 {#launch-by-adobe}

클라이언트 측 DIL 코드에서 Experience Platform Launch의 서버측 전달로 구현 옵션을 이동하는 방법에 대해 알아보려면 아래 비디오를 시청하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### &quot;페이지에서&quot; 또는 Adobe이 아닌 태그 관리자 {#on-the-page-or-non-adobe-tag-manager}

클라이언트 측 DIL 코드에서 의 서버측 전달로 구현 옵션을 이동하는 방법에 대해 알아보려면 아래 비디오를 시청하십시오 [!DNL AppMeasurement] 코드가 있어야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 3단계: 전달 활성화(단위) [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

지금까지 이 자습서에서는 클라이언트측 DIL 코드에서 서버측 전달로 코드를 전환하는 데 모든 시간을 사용했습니다. 그게 더 어려운 부분이라 괜찮습니다 이 섹션은 매우 간단하지만 코드를 업데이트하는 것만큼 중요합니다. 이 비디오에서는 데이터를 Analytics에서 Audience Manager으로 실제로 전달할 수 있는 스위치를 뒤집는 방법을 확인할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**참고:** 비디오에 명시된 대로 Experience Cloud 백엔드에서 전달을 완전히 구현하려면 최대 4시간이 소요됩니다.

## 타이밍 {#timing}

미리 알림으로 클라이언트 측 DIL에서 서버 측 전달로 전환하는 두 가지 기본 작업이 있습니다.

1. 코드 업데이트
1. 스위치를 [!DNL Analytics] [!DNL Admin Console]

하지만 문제는, 어느 것을 먼저 하느냐입니다. 중요한 건가요? 네, 죄송합니다. 두 가지 질문이었습니다. 하지만 대답은... 상황에 따라 다르죠 *다음을 수행할 수 있습니다.* 문제. 막연한 게 어때요? 그것을 분류합시다. 그러나 먼저 수많은 사이트가 있는 대규모 조직일 경우 발생할 수 있는 추가 질문: 모든 일을 한꺼번에 해야 하나요? 그건 좀 쉬워요 아니 한 조각씩 할 수 있습니다.

### 좀 더 깊이 들어가 {#a-little-deeper-dive}

타이밍과 순서가 중요한 이유는 전달 방식 때문입니다 _정말로_ 저작물로서, 다음의 몇 가지 기술적 사실에 요약할 수 있습니다.

* ECID(Experience Cloud ID 서비스)가 구현되어 있고 [!DNL Analytics] [!DNL Admin Console] (&quot;스위치&quot;)가 켜져 있으면 데이터가 다음 위치에서 전달됩니다. [!DNL Analytics] AAM으로 전환하는 것이 좋습니다.
* ECID가 구현되지 않은 경우, 스위치가 켜져 있고 서버측 전달 코드가 있는 경우에도 데이터가 전달되지 않습니다.
* 서버 측 전달 코드(플랫폼 태그든 페이지든)는 실제로 응답을 처리하며 마이그레이션을 완료해야 합니다.
* 서버 측 전달 스위치는 [!UICONTROL report suite]하지만 코드가 플랫폼 태그의 속성 또는 [!DNL AppMeasurement] 플랫폼 태그를 사용하지 않는 경우 파일로 내보낼 수 있습니다.

### 우수 사례 {#best-practices}

다음은 이러한 기술 세부 사항에 따라 수행할 작업과 수행할 시기에 대한 권장 사항입니다.

#### ECID가 아직 구현되지 않은 경우 {#if-you-do-not-have-ecid-yet-implemented}

1. 스위치 뒤집기 [!DNL Analytics] 각 [!UICONTROL report suite] 이 옵션을 활성화하면 서버측 전달이 활성화됩니다.

   1. ECID가 없으므로 전달이 아직 시작되지 않습니다.

1. 사이트별로, 클라이언트측 DIL에서 서버측 전달로(플랫폼 태그에서 가능) 또는 페이지에서 위의 다른 섹션에 설명된 대로 코드를 업데이트합니다.

   1. 이제 전달은(ECID를 추가한 경우) 흐름이며, 또한 JSON 응답을 수신해야 합니다 [!DNL Analytics] 비콘(자세한 내용은 아래 유효성 검사 및 문제 해결 섹션 참조)을 참조하십시오.

#### ECID가 구현되어 있다면 {#if-you-do-have-ecid-implemented}

1. 코드를 DIL에서 서버 측 전달로 업데이트할 준비가 되도록 준비하고 계획하십시오 [!UICONTROL report suite] 서버 측 전달을 활성화합니다.

   1. 스위치 뒤집기 [!DNL Analytics] 를 눌러 서버 측 전달을 활성화합니다.

      1. ECID가 활성화되어 있으므로 전달이 시작됩니다.
   1. 가능한 한 빨리 클라이언트 측 DIL에서 단일 측 전달로 코드를 업데이트합니다(위의 다른 섹션에서 설명한 대로 플랫폼 태그 또는 페이지에 있을 수 있음).

      1. 에 대한 적절한 JSON 응답을 수신해야 합니다 [!DNL Analytics] 비콘( [유효성 검사 및 문제 해결](#validation-and-troubleshooting) 자세한 내용은 아래 섹션을 참조하십시오.


>[!NOTE]
>
>위의 1단계와 2단계 간에 AAM으로 데이터의 중복을 받게 되므로 이 두 단계를 가능한 한 서로 가깝게 수행하는 것이 중요합니다. 즉, 단일 측 전달에서 [!DNL Analytics] AAM에 DIL 코드가 아직 페이지에 있으므로 페이지에서 AAM으로 바로 이동하는 히트가 있으므로 데이터를 두 배로 늘립니다. DIL에서 서버측 전달로 코드를 업데이트하면 바로 이를 완화합니다.

>[!NOTE]
>
>데이터의 작은 중복이 아니라 데이터에 작은 불일치가 있는 것이 나을 경우 위의 1단계와 2단계의 순서를 전환할 수 있습니다. 코드를 DIL에서 서버측 전달로 이동하면 스위치를 전환하여 을 위한 서버측 전달을 활성화할 때까지 AAM으로 데이터 흐름이 중지됩니다 [!UICONTROL report suite]. 일반적으로 고객은 방문자를 트레이트 및 트레이트로 유입시키지 않고 데이터를 두 배 가량 늘릴 수 있습니다 [!UICONTROL segments].

#### 많은 사이트 및 [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

이 주제는 다음과 같이 주요 전략을 요약할 수 있다는 점에서 이전 섹션에서 간단히 언급되었습니다.

하나의 사이트 마이그레이션/[!UICONTROL report suite] (또는 sites/ 그룹)[!UICONTROL report suites])을 한 번에 하나씩 사용할 수 있습니다.

그러나 몇 가지 가능한 시나리오에 따라 이 작업이 약간 까다로울 수 있습니다.

* 에는 여러 개의 개별 항목이 포함된 사이트가 있습니다 [!UICONTROL report suites]
* 다음을 수행합니다. [!UICONTROL report suite] 여기에는 몇 개의 사이트(예: 전역)가 포함됩니다 [!UICONTROL report suite])
* 하나의 Platform 태그 속성을 사용하여 여러 사이트를 다룹니다
* 서로 다른 사이트마다 다른 개발 팀이 있습니다

이 품목들 때문에 좀 복잡해질 수 있어요 제가 제안할 수 있는 가장 좋은 것은 다음과 같습니다.

* 위에서 설명한 사항에 따라 서버측 전달로 마이그레이션하는 전략을 만드는 데 시간이 좀 걸립니다
* 플랫폼 태그(또는 [!DNL AppMeasurement] 일반적으로 1개 또는 2개의 개별 항목에 매핑됩니다 [!UICONTROL report suites]를 지정하는 경우, 엔터프라이즈를 서버측 전달로 업데이트하여 이러한 개별 그룹에서 하나씩 작동하는 계획을 세울 수 있습니다
* Adobe 컨설팅 팀과 작업하는 경우 마이그레이션 계획에 대해 해당 팀에 연락하여 필요한 경우 지원할 수 있습니다

## 유효성 검사 및 문제 해결 {#validation-and-troubleshooting}

서버 측 전달이 작동되고 실행 중인지 확인하는 기본 방법은 앱으로부터 수신되는 Adobe Analytics 히트에 대한 응답을 확인하는 것입니다.

에서 데이터의 서버측 전달을 수행하지 않는 경우 [!DNL Analytics] 에 대한 Audience Manager이 실제로 없으면 [!DNL Analytics] 비콘(2x2 픽셀 외). 그러나 서버측 전달을 수행하는 경우에서 확인할 수 있는 항목이 있습니다 [!DNL Analytics] 요청 및 응답을 통해 [!DNL Analytics] 가 Audience Manager과 올바르게 통신하고 히트를 전달하며 응답을 받습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>잘못된 &quot;Success&quot;를 확인합니다. 응답이 있고 모든 것이 작동하는 것처럼 보이는 경우 가 있는지 확인하십시오. `stuff` 개체의 이름을 지정합니다. 없는 경우 `"status":"SUCCESS"`. 이상하게 들리겠지만, 이것은 실제로는 제대로 작동하지 않는다는 증거입니다.
>
>이 경우 플랫폼 태그에서 코드 업데이트를 완료했거나 [!DNL AppMeasurement]그러나 전달을에서 [!DNL Analytics] [!DNL Admin Console] 아직 완료되지 않았습니다. 이 경우에서 서버측 전달을 활성화했는지 확인해야 합니다 [!DNL Analytics] [!DNL Admin Console] 에 대해 [!UICONTROL report suite]. 활성화했지만 아직 4시간이 되지 않았다면 백엔드에서 필요한 모든 변경 작업을 수행하는 데 시간이 오래 걸릴 수 있으므로 기다려 주십시오.


![거짓 성공](assets/falsesuccess.png)

서버측 전달에 대한 자세한 내용은 [설명서](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).
