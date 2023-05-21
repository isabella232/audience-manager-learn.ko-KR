---
title: 사이트의 Audience Manager 구현을 클라이언트측 DIL에서 서버측 전달로 마이그레이션
description: 사이트의 Audience Manager(AAM) 구현을 클라이언트측 DIL에서 서버측 전달로 마이그레이션하는 방법에 대해 알아봅니다. 이 자습서는 AAM과 Adobe Analytics이 모두 있고 DIL(Data Integration Library) 코드를 사용하여 페이지에서 AAM으로 히트를 보내고 페이지에서 Adobe Analytics으로 히트를 보내는 경우에 적용됩니다.
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

이 자습서는 Adobe Audience Manager(AAM)와 Adobe Analytics이 모두 있고 현재 DIL( )를 사용하여 페이지에서 AAM으로 히트를 전송하는 경우에 적용됩니다[!DNL Data Integration Library]) 코드를 실행하고 페이지에서 Adobe Analytics으로 히트를 전송하는 것입니다. 이러한 두 솔루션이 모두 있고 이 두 솔루션이 모두 Adobe Experience Cloud의 일부이므로 서버측 전달을 사용하는 모범 사례를 따를 수 있습니다. 이렇게 하면 [!DNL Analytics] 데이터 수집 서버가 클라이언트측 코드가 추가 히트를 페이지에서 AAM으로 보내도록 하는 대신 실시간으로 사이트 분석 데이터를 Audience Manager에 전달하는 역할을 합니다. 이 자습서에서는 이전 클라이언트측 DIL 구현에서 최신 서버측 전달 방법으로 전환하는 단계를 안내합니다.

## 클라이언트측(DIL)과 서버측 비교 {#client-side-dil-vs-server-side}

Adobe Analytics 데이터를 AAM에 가져오는 두 가지 방법을 비교하고 대조할 때 먼저 다음 이미지에서 차이를 시각화하는 것이 유용할 수 있습니다.

![클라이언트측에서 서버측](assets/client-side_vs_server-side_aam_implementation.png)

### 클라이언트측 DIL 구현 {#client-side-dil-implementation}

이 방법을 사용하여 Adobe Analytics 데이터를 AAM으로 가져오는 경우 웹 페이지에서 두 개의 히트가 발생합니다(한 개는 [!DNL Analytics]및 AAM으로 이동하는 항목(복사한 후) [!DNL Analytics] 웹 페이지의 데이터입니다. [!UICONTROL Segments] 은 AAM에서 개인화에 사용할 수 있는 페이지로 반환됩니다. 이는 레거시 구현으로 간주되며 더 이상 권장되지 않습니다.

모범 사례를 따르고 있지 않다는 사실을 넘어, 이 방법을 사용할 때의 단점은 다음과 같습니다.

* 페이지에서 전송되는 히트가 한 개가 아니라 두 개입니다
* AAM 대상을 실시간으로 공유하려면 서버측 전달이 필요합니다. [!DNL Analytics]따라서 클라이언트측 구현에서는 이 기능(및 향후 다른 기능 포함)을 사용할 수 없습니다

AAM 구현의 서버측 전달 방법으로 이동하는 것이 좋습니다.

### 서버측 전달 구현 {#server-side-forwarding-implementation}

위의 이미지에 표시된 대로 히트는 웹 페이지에서 Adobe Analytics으로 이동합니다. [!DNL Analytics] 그런 다음 해당 데이터를 AAM에 실시간으로 전달하고 방문자가 AAM 트레이트 및 [!UICONTROL segments]: 마치 히트가 페이지에서 바로 온 것처럼.

[!UICONTROL Segments] 은 동일한 실시간 히트에서 다시 로 반환됩니다. [!DNL Analytics]개인화를 위해 의 응답을 웹 페이지에 전달합니다.

서버측 전달로 이동하는 데 있어 시간 단축이 없습니다. Adobe은 Audience Manager과 을 모두 가진 모든 사용자에게 [!DNL Analytics] 는 이 구현 메서드를 사용합니다.

## 두 가지 주요 작업이 있습니다. {#you-have-two-main-tasks}

이 페이지에는 꽤 많은 정보가 있으며, 물론 중요한 정보입니다. 그러나, **이 모든 것이 당신이 해야 할 두 가지 주요 사항으로 요약됩니다**:

1. 클라이언트 측 DIL 코드에서 서버 측 전달 코드로 코드 변경
1. 에서 스위치를 대칭 이동합니다. [!DNL Analytics] [!DNL Admin Console] 데이터의 실제 전달을 시작하려면 다음을 수행합니다. [!UICONTROL report suite])

이러한 작업 중 하나를 건너뛸 경우 서버측 전달이 제대로 작동하지 않습니다. 설정에서 이 두 단계를 올바르게 수행하는 데 도움이 되도록 단계 및 추가 데이터가 이 문서에 추가되었습니다.

## 구현 옵션 {#implementation-options}

클라이언트측에서 서버측 전달로 이동할 때 수행할 작업 중 하나는 코드를 새로운 서버측 전달 코드로 변경하는 것입니다. 이 작업은 다음 옵션 중 하나를 사용하여 수행됩니다.

* Adobe Experience Platform 태그 - 웹 속성에 대한 Adobe의 권장 구현 옵션입니다. Platform 태그가 모든 힘든 작업을 수행했으므로 이 작업은 쉬운 작업이라는 것을 알 수 있습니다.
* 페이지에서 새 SSF 코드를 `doPlugins` 내 함수 `appMeasurement.js` 파일, 아직 Adobe Launch를 사용하지 않는 경우
* 기타 태그 관리자 - SSF 코드를 계속 넣을 수 있으므로 이전 (페이지에서) 옵션처럼 처리할 수 있습니다. `doPlugins`: 다른 태그 관리자가 [!DNL AppMeasurement] 코드

아래에서는 각각의 이러한 기능에 대해 살펴보겠습니다. _코드 업데이트_ 섹션.

## 구현 단계 {#implementation-steps}

다음 단계에서는 구현에 대해 설명합니다.

### 0단계: 전제 조건: Experience Cloud ID 서비스(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

서버측 전달로 이동하기 위한 주요 전제 조건은 Experience Cloud ID 서비스를 구현하는 것입니다. 이 작업은 Experience Platform Launch을 사용하는 경우 가장 쉽게 수행됩니다. 이 경우 ECID 확장을 설치하면 나머지가 수행됩니다.

Adobe이 아닌 TMS를 사용하거나 TMS가 전혀 없는 경우 ECID를 구현하여 실행하십시오 **다음 이전** 기타 모든 Adobe 솔루션. 다음을 참조하십시오. [ECID 설명서](https://experienceleague.adobe.com/docs/id-service/using/home.html) 을 참조하십시오. 다른 유일한 전제 조건은 코드 버전에 대한 것이므로 다음 단계에서 코드의 최신 버전을 간단하게 적용할 수 있으므로 문제가 없습니다.

>[!NOTE]
>
>구현하기 전에 이 전체 문서를 읽으십시오. 아래의 &quot;타이밍&quot; 섹션에는 다음에 대한 중요한 정보가 있습니다. *조건* ecid를 포함한 각 조각을 구현해야 합니다(아직 구현되지 않은 경우).

### 1단계: DIL 코드에서 현재 사용된 옵션 기록 {#step-record-currently-used-options-from-dil-code}

클라이언트측 DIL 코드에서 서버측 전달로 이동할 준비가 되면 첫 번째 단계는 사용자 지정 설정 및 AAM에 전송된 데이터를 포함하여 DIL 코드로 수행하는 모든 작업을 식별하는 것입니다. 주목하고 고려할 사항은 다음과 같습니다.

* 기본 [!DNL Analytics] 변수, 사용 `siteCatalyst.init` DIL 모듈 - 이 모듈은 일반적인 파일을 보내는 것이므로 걱정할 필요가 없습니다. [!DNL Analytics] 변수를 넘기면 서버측 전달이 활성화되어 있기 때문에 이러한 문제가 발생합니다.
* 파트너 하위 도메인 - `DIL.create` 함수, 다음을 기록합니다. `partner` 매개 변수. 이를 &quot;파트너 하위 도메인&quot; 또는 경우에 따라 &quot;파트너 ID&quot;라고 하며 새 서버측 전달 코드를 배치할 때 필요합니다.
* [!DNL Visitor Service Namespace] - &quot;[!DNL Org ID]&quot;&quot; 또는 &quot;[!DNL IMS Org ID],&quot; 새 서버측 전달 코드를 설정할 때도 이 옵션이 필요합니다. 기록해 두십시오.
* containerNSID, uuidCookie 및 기타 고급 옵션 - 사용 중인 추가 고급 옵션을 기록해 두면 서버측 전달 코드에서도 설정할 수 있습니다.
* 추가 페이지 변수 - 다른 변수가 페이지에서 AAM으로 전송되는 경우(기본과 함께) [!DNL Analytics] siteCatalyst.init에서 처리하는 변수인 경우 서버측 전달을 통해 전송할 수 있도록 변수를 메모해야 합니다(spoiler 경고: [!DNL contextData] 변수)를 참조하십시오.

### 2단계: 코드 업데이트 {#step-updating-the-code}

위치 [구현 옵션](#implementation-options) (위의 경우) 서버측 전달을 구현하는 방법과 위치와 관련하여 여러 옵션이 제공됩니다. 이 섹션이 효과적이기 위해서는 이 섹션(두 개의 섹션이 조합됨)으로 세분화해야 합니다. 요구 사항을 가장 잘 설명하는 이 섹션의 메서드로 이동합니다.

#### Adobe Experience Platform 태그 {#launch-by-adobe}

아래 비디오를 통해 클라이언트측 DIL 코드에서 Experience Platform Launch의 서버측 전달로 구현 옵션을 이동하는 방법에 대해 알아보십시오.

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### &quot;페이지에서&quot; 또는 Adobe이 아닌 태그 관리자 {#on-the-page-or-non-adobe-tag-manager}

아래 비디오를 통해 클라이언트측 구현 코드에서 의 서버측 전달로 DIL 옵션을 이동하는 방법에 대해 알아보십시오 [!DNL AppMeasurement] 파일 또는 비 Adobe 태그 관리 시스템에 있는 코드.

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 3단계: 전달 활성화(마다 [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

지금까지 이 자습서에서는 클라이언트 측 DIL 코드에서 서버 측 전달로 코드를 전환하는 데 모든 시간을 투자했습니다. 그게 더 어려운 부분이기 때문에 괜찮습니다. 이 섹션은 매우 간단하지만 코드를 업데이트하는 것만큼 중요합니다. 이 비디오에서는 Analytics에서 Audience Manager으로 데이터를 실제로 전달할 수 있도록 하는 스위치를 전환하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**참고:** 비디오에 설명된 대로 전달 활성화가 Experience Cloud 백엔드에서 완전히 구현되려면 최대 4시간이 소요됩니다.

## 시간 {#timing}

다시 말해서 클라이언트측 DIL에서 서버측 전달로 이동하는 데 필요한 두 가지 주요 작업이 있습니다.

1. 코드 업데이트
1. 에서 스위치 뒤집기 [!DNL Analytics] [!DNL Admin Console]

하지만 문제는, 어느 것을 먼저 할 것인가 하는 것입니다. 그게 중요한가요? 네, 죄송합니다. 두 가지 질문이었습니다. 하지만 답은... 상황에 따라 다르죠 *can* 문제. 어째서 모호한 거죠? 분류해 보겠습니다. 그러나 먼저 사이트가 많은 대규모 조직인 경우 제기될 수 있는 추가 질문이 있습니다. 모든 것을 한 번에 수행해야 합니까? 저것이 조금 더 쉬워요 아니. 그것을 하나씩 할 수 있다.

### 좀 더 깊이 파고들기 {#a-little-deeper-dive}

타이밍과 주문이 중요한 이유는 전달 방식 때문입니다 _정말로_ 다음 몇 가지 기술적 사실로 요약할 수 있는 작업입니다.

* ECID(Experience Cloud ID 서비스)가 구현되어 있고 [!DNL Analytics] [!DNL Admin Console] (&quot;스위치&quot;) 켜짐, 데이터가 다음 위치에서 전달됨 [!DNL Analytics] 아직 코드를 업데이트하지 않았더라도 AAM으로 보내십시오.
* ECID를 구현하지 않은 경우 스위치가 켜져 있고 서버측 전달 코드가 있더라도 데이터가 전달되지 않습니다.
* 서버측 전달 코드(플랫폼 태그든 페이지든)는 실제로 응답을 처리하며 마이그레이션을 완료하는 데 필요합니다.
* 서버 측 전달 스위치는 [!UICONTROL report suite]를 반환하지만 코드는 Platform 태그의 속성에 의해 처리되거나 [!DNL AppMeasurement] platform 태그를 사용하지 않는 경우 파일입니다.

### 우수 사례 {#best-practices}

이러한 기술적 세부 정보를 기반으로 할 때 수행할 작업과 그 시기를 권장하는 사항은 다음과 같습니다.

#### ECID가 아직 구현되지 않은 경우 {#if-you-do-not-have-ecid-yet-implemented}

1. 스위치 뒤집기 [!DNL Analytics] 각 [!UICONTROL report suite] 서버측 전달을 위해 를 활성화합니다.

   1. ECID가 없으므로 전달이 아직 시작되지 않습니다.

1. 사이트별로 위의 다른 섹션에서 설명한 대로 클라이언트측 DIL에서 서버측 전달(플랫폼 태그일 수 있음) 또는 페이지에서 코드를 업데이트합니다.

   1. 전달은 (ECID를 추가한 대로) 흐르며, 다음에 대한 적절한 JSON 응답도 받아야 합니다. [!DNL Analytics] 비콘(자세한 내용은 아래 유효성 검사 및 문제 해결 섹션 참조)

#### ECID가 구현된 경우 {#if-you-do-have-ecid-implemented}

1. DIL에서 서버측 전달 PER로 코드를 업데이트할 준비가 되도록 준비하고 계획합니다. [!UICONTROL report suite] 서버측 전달을 위해 를 활성화합니다.

   1. 스위치 뒤집기 [!DNL Analytics] 서버측 전달을 활성화합니다.

      1. ECID가 활성화되었으므로 전달이 시작됩니다.
   1. 최대한 빨리 클라이언트측 DIL에서 단일 측 전달로 코드를 업데이트합니다(위의 다른 섹션에서 설명한 대로 Platform 태그나 페이지에 있을 수 있음).

      1. 에 대한 적절한 JSON 응답을 수신해야 합니다. [!DNL Analytics] 비콘( 참조: [유효성 검사 및 문제 해결](#validation-and-troubleshooting) 아래 섹션 을 참조하십시오.


>[!NOTE]
>
>위의 1단계와 2단계 사이에는 AAM으로 이동하는 데이터 중복이 있으므로 이 두 단계를 가능한 한 가깝게 수행하는 것이 중요합니다. 즉, 단일 측 전달이에서 데이터 전송을 시작했습니다. [!DNL Analytics] AAM으로 전송되며 DIL 코드가 아직 페이지에 있으므로 페이지에서 AAM으로 직접 이동하는 히트가 발생하여 데이터가 두 배가 됩니다. DIL에서 서버측 전달로 코드를 업데이트하는 즉시 이 문제가 해결됩니다.

>[!NOTE]
>
>데이터의 작은 중복보다 작은 불일치가 더 나을 경우 위의 1단계와 2단계의 순서를 변경할 수 있습니다. DIL에서 서버측 전달로 코드를 이동하면에서 서버측 전달을 켜기 위해 스위치를 전환할 때까지 AAM으로의 데이터 흐름이 중지됩니다. [!UICONTROL report suite]. 일반적으로 고객은 방문자를 트레이트와 로 안내하지 않고 두 배로 적은 양의 데이터를 사용하는 것이 좋습니다 [!UICONTROL segments].

#### 사이트가 많을 때 마이그레이션 타이밍 및 [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

이 주제는 주요 전략을 다음과 같이 요약할 수 있다는 점에서 이전 섹션에서 간단히 설명합니다.

하나의 사이트 마이그레이션/[!UICONTROL report suite] (또는 사이트 그룹/[!UICONTROL report suites]) 한 번에.

그러나 몇 가지 가능한 시나리오에 따라 다소 까다로울 수 있습니다.

* 여러 개의 고유한 항목이 포함된 사이트가 있습니다. [!UICONTROL report suites]
* 다음 항목이 있습니다. [!UICONTROL report suite] 여러 사이트 포함(예: 글로벌 [!UICONTROL report suite])
* 하나의 Platform 태그 속성을 사용하여 여러 사이트를 다룹니다
* 사이트마다 다른 개발 팀이 있습니다.

이 물건들 때문에 조금 복잡해질 수 있습니다. 제가 제안할 수 있는 가장 좋은 사항은 다음과 같습니다.

* 위에서 설명한 사항을 기반으로 서버측 전달로 마이그레이션하기 위한 전략을 수립하는 데 약간의 시간을 투자하십시오
* Platform의 단일 속성 태그(또는 단일 속성 태그) [!DNL AppMeasurement] file)은 일반적으로 1개 또는 2개의 개별 파일에 매핑됩니다 [!UICONTROL report suites]을 사용하면 이러한 개별 그룹에서 작동하는 계획을 하나씩 수립하여 엔터프라이즈를 서버측 전달로 업데이트할 수 있습니다
* Adobe 컨설팅과 협력하고 있는 경우, 필요에 따라 도움을 받을 수 있도록 마이그레이션 계획에 대해 상담하십시오

## 유효성 검사 및 문제 해결 {#validation-and-troubleshooting}

서버 측 전달이 작동되고 실행 중인지 확인하는 기본 방법은 앱에서 수신되는 Adobe Analytics 히트에 대한 응답을 확인하는 것입니다.

에서 데이터의 서버측 전달을 수행하지 않을 경우 [!DNL Analytics] Audience Manager에 대한 응답 없음 [!DNL Analytics] 비콘(2x2 픽셀 외). 그러나 서버측 전달을 수행하는 경우에서 확인할 수 있는 항목이 있습니다 [!DNL Analytics] 다음을 알리는 요청 및 응답 [!DNL Analytics] 은 Audience Manager과 올바르게 통신하고 히트를 전달하며 응답을 받습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>잘못된 &quot;Success&quot;를 확인합니다. 응답이 있고 모든 것이 작동하는 것처럼 보이는 경우 `stuff` 응답에 있는 개체입니다. 그렇지 않으면 다음과 같은 메시지가 표시될 수 있습니다 `"status":"SUCCESS"`. 이상하게 들리겠지만, 이것은 실제로는 제대로 작동하지 않는다는 증거입니다.
>
>이 메시지가 표시된다면 Platform 태그에서 코드 업데이트를 완료했거나 [!DNL AppMeasurement], 하지만 의 전달은 [!DNL Analytics] [!DNL Admin Console] 이(가) 아직 완료되지 않았습니다. 이 경우에서 서버측 전달을 활성화했는지 확인해야 합니다 [!DNL Analytics] [!DNL Admin Console] 에 대한 [!UICONTROL report suite]. 백엔드에서 필요한 모든 사항을 변경하는 데 시간이 오래 걸릴 수 있으므로 4시간이 지나지 않은 경우 기다려 주십시오.


![거짓 성공](assets/falsesuccess.png)

서버측 전달에 대한 자세한 내용은 [설명서](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).
