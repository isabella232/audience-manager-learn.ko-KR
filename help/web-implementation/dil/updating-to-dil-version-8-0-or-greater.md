---
title: Adobe Audience Manager DIL 버전 8.0 이상으로 업데이트
description: 이 문서에서는 Adobe Audience Manager(AAM) Data Integration Library(DIL) 코드를 버전 8.0 이상으로 업데이트하는 방법에 대한 단계 및 권장 사항을 제공합니다. 이는 Adobe Analytics 데이터의 서버측 전달이 아닌 "클라이언트측" DIL 구현을 지칭하는 것으로, DTM, Launch by Adobe 및 Adobe 태그 관리자 솔루션이 없는 구현을 다룹니다.
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1122'
ht-degree: 1%

---

# Adobe Audience Manager의 DIL 버전 8.0 이상으로 업데이트 {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

이 문서에서는 Adobe Audience Manager(AAM) 업데이트에 대한 단계 및 권장 사항을 제공합니다 [!DNL Data Integration Library] (DIL) 코드를 버전 8.0 이상으로 변환. 이는 Adobe Analytics 데이터의 서버측 전달이 아닌 &quot;클라이언트측&quot; DIL 구현을 지칭하는 것으로, DTM, Launch by Adobe 및 Adobe 태그 관리자 솔루션이 없는 구현을 다룹니다.

## 개요 {#overview}

Audience Manager [!DNL Data Integration Library] (DIL) 코드를 사용하면 웹 사이트에서 AAM을 구현할 수 있습니다*. 이전 버전의 DIL을 구현할 때 Adobe의 ECID(Experience Cloud ID 서비스)도 구현할 필요가 없었습니다(매우 좋은 생각이었지만). DIL 버전 8.0부터 ECID 버전 3.3 이상에 대한 종속성이 강합니다. ECID 3.3 없이 또는 이전 버전으로 DIL 8.0 이상을 구현하는 경우 오류가 발생하여 작동하지 않습니다. AAM을 구현할 수 있는 방법에는 여러 가지가 있으므로, 몇 가지 권장 사항과 몇 가지 단계를 제공하기 위해 이 페이지를 만들었습니다. 아래에서는 이러한 단계 및 권장 사항이 플랫폼/구현 방법으로 분류되어 있습니다. DIL에 대한 자세한 내용은 [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en).

* 이 페이지의 설명에 나와 있는 것처럼, 이 섹션에서는 Adobe Analytics이 없는 AAM 고객이 사용하는 &quot;클라이언트측&quot; DIL 구현만 다룹니다. Adobe Analytics이 있는 경우 AAM을 구현하는 서버측 전달 방법을 사용해야 합니다. 이 방법은 다음에 설명되어 있습니다. [설명서](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).

## 중복 및 더 이상 사용되지 않는 요소 및 메서드 {#duplicate-and-deprecated-elements-and-methods}

이전 버전의 DIL 및 ECID에서는 중복 메서드(DIL 및 ECID 모두에서 동일한 기능을 수행하는 메서드)가 있어 사용할 메서드와 관련하여 혼동을 일으켰습니다. 일반적으로 두 가지를 모두 사용하고 일치시켜야 했으며 해당 메시지는 고객에게 잘 전달되지 않았습니다. DIL 8.0부터 이러한 중복 메서드 및 요소는 DIL에서 더 이상 사용되지 않으며 ECID 버전을 사용하는 것이 좋습니다.

예:

* 사용 시 [!DNL DIL.create], 일부 요소는 더 이상 사용되지 않으며, 대신 ECID 요소를 사용해야 합니다. 이러한 요소는에서 호출됩니다. [[!DNL DIL.create] 설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html).
* 다음 [!DNL idSync] 인스턴스 수준 메서드도 메서드의 [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html).

## 고객 ID와 ID 동기화 {#id-syncing-with-a-customer-id}

AAM에서는 시스템의 UUID(익명 고유 사용자 ID)를 고객 ID와 동기화하여 해당 고객에 대한 오프라인 데이터를 업로드하고 고객의 온라인 동작과 함께 연결하여 고객을 더 잘 이해할 수 있습니다. 과거에는 다음 두 가지 방법 중 하나로 이 작업을 수행했습니다.

* 다음 [!DNL idSync] 인스턴스 수준 메서드
* 다음 [!DNL declaredId] 의 요소 [!DNL DIL.create]

이러한 이전 방법 중 하나를 사용하여 고객 ID와 동기화한 경우 를 사용하도록 업데이트하는 것이 좋습니다. [!DNL setCustomerIDs] 메서드, ECID 서비스의 일부. 다음에 대한 추가 정보: [!DNL setCustomerIDs] 은 메서드에서 사용할 수 있습니다 [설명서](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html).

**빠른 팁:** 이전에 위의 방법 중 하나를 사용할 때 AAM을 참조했습니다 [!UICONTROL Data Source] (으)로 [!UICONTROL Data Source] ID(&quot;DPID&quot;라고도 함). 로 업데이트할 때 [!DNL setCustomerIDs], AAM을 사용해야 합니다. [!UICONTROL Data Source]의 &quot;[!UICONTROL Integration Code]&quot; 형식을 사용하십시오. 그것은 여전히 같은 것을 가리키고 있다 [!UICONTROL Data Source] 는 다른 식별자일 뿐입니다. 아래 비디오에 나와 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

다음 섹션에는 구현 방법에 따라 DIL 8.0으로 업데이트하는 단계 및 권장 사항이 나와 있습니다.

## Adobe Experience Platform 태그의 DIL 8.0으로 업데이트 {#updating-to-dil-in-experience-platform-launch}

DIL 8.0으로 업데이트하기 위한 기본 단계

1. 8.0 이전 DIL을 사용하는 경우 업그레이드하기 전에 AAM 확장에서 DIL 구성으로 이동하여 사용 중인 고급 옵션(후속 단계에서 사용)을 메모하십시오.
1. AAM 확장을 버전 8.0 이상으로 업데이트합니다.
1. Experience Cloud ID 서비스 확장이 버전 3.3.0 이상인지 확인하십시오.
1. 더 이상 사용되지 않는 모든 메서드/요소(예: `disableIDSyncs`8.0 이전 AAM 확장 기능 또는 DIL을 위한 사용자 지정 코드에 있었던 는 ECID 확장에서 ECID 메서드를 활성화합니다.

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

1. 변경 사항을 게시합니다.

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Adobe DTM에서 DIL 8.0으로 업데이트 {#updating-to-dil-in-adobe-dtm}

1. AAM 도구를 버전 8.0 이상으로 업데이트합니다. 이 버전 설정은 AAM 도구의 &quot;일반&quot; 섹션 아래에 있습니다.
1. 더 이상 사용되지 않는 모든 메서드/요소(예: `disableIDSyncs`) DIL을 위해 8.0 이전 AAM 도구의 사용자 지정 코드에 있는 사용자는 메모한 후(ECID 도구에 추가할 수 있도록) 사용자 지정에서 제거합니다 [!DNL DIL code] AAM 도구에서.
1. Experience Cloud ID 서비스 확장을 버전 3.3.0 이상으로 업데이트합니다.
1. AAM 도구의 사용자 지정 코드에서 제거한 ECID 도구에 고급 옵션을 추가합니다.
1. 변경 사항 게시

## Adobe 없는 Tag Management 솔루션으로 DIL 8.0으로 업데이트 {#additional-resources}

페이지에서 바로 코드를 업데이트하는 경우 위에서 설명한 대로 메서드/요소를 DIL에서 ECID로 이동해야 하는 경우를 제외하고 이전 항목을 최신 항목으로 바꿀 수 있습니다. 이 경우 DIL 위치의 이전 메서드/요소를 ECID 위치의 ECID 메서드/요소로 바꾸기만 하면 됩니다.

비 Adobe 태그 관리자도 마찬가지입니다. 해당 태그 관리 솔루션에 이전 버전이 있는 경우 다음 단계에 설명된 대로 새 코드로 바꿉니다.

1. DIL 라이브러리를 최신 버전(8.0 이상)으로 업데이트 - 현재 공개 위치에서 사용할 수 없으므로 Adobe 컨설팅 또는 Adobe 고객 지원 센터에서 최신 DIL 코드를 받아야 합니다. 그런 다음 이전 DIL 라이브러리 코드를 새 DIL 라이브러리 코드로 바꾸고 다음 단계로 이동하십시오(지금 중지하지 않으면 문제가 발생합니다.).
1. 설치 [!DNL ECID Service] 또는 기존 버전을 3.3.0 이상으로 업데이트하십시오. 최신 Experience Cloud ID 서비스 릴리스를 다운로드할 수 있습니다 [GitHub 페이지에서](https://github.com/Adobe-Marketing-Cloud/id-service/releases). 도움이 필요한 경우 다음을 참조하십시오. [설명서](https://experienceleague.adobe.com/docs/id-service/using/home.html) 또는 Adobe 컨설턴트에게 문의하십시오.

1. 사용자 지정 DIL 코드에 있는 더 이상 사용되지 않는 모든 메서드 또는 요소가 ECID 메서드로 이동되는지 확인합니다.

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`

      [설명서](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`

      [설명서](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `idSyncSSLUseAkamai`

      [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

      [설명서](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
