---
title: Adobe Audience Manager DIL 버전 8.0 이상으로 업데이트
description: 이 문서에서는 AAM(Adobe Audience Manager) Data Integration Library(DIL) 코드를 버전 8.0 이상으로 업데이트하는 단계 및 권장 사항을 제공합니다. 이는 Adobe Analytics 데이터의 서버측 전달이 아니라 "클라이언트측" DIL 구현을 참조하며, Adobe 태그 관리자 솔루션 없이 DTM, Launch by Adobe 및 구현을 다룹니다.
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 1%

---

# Adobe Audience Manager의 DIL 버전 8.0(또는 이상)으로 업데이트 {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

이 문서에서는 Adobe Audience Manager (AAM) [!DNL Data Integration Library] (DIL) 코드를 버전 8.0 이상으로 업데이트하는 단계 및 권장 사항을 제공합니다. 이는 Adobe Analytics 데이터의 서버측 전달이 아니라 &quot;클라이언트측&quot; DIL 구현을 참조하며, Adobe 태그 관리자 솔루션 없이 DTM, Launch by Adobe 및 구현을 다룹니다.

## 개요 {#overview}

Audience Manager의 [!DNL Data Integration Library] (DIL) 코드를 사용하면 웹 사이트*에서 AAM을 구현할 수 있습니다. 이전 버전의 DIL을 구현할 때 Adobe의 ECID(Experience Cloud ID Service)도 구현하지 않아도 됩니다(매우 좋은 생각이었지만). DIL 버전 8.0부터 ECID 버전 3.3 이상에 대한 하드 종속성이 있습니다. ECID 3.3 없이 또는 이전 버전으로 DIL 8.0 이상을 구현하는 경우 오류가 발생하고 작동하지 않습니다. AAM을 구현할 수 있는 여러 가지 방법이 있으므로 몇 가지 단계와 몇 가지 권장 사항을 제공하기 위해 이 페이지를 만들었습니다. 아래에는 플랫폼/구현 방법별로 분류된 이러한 단계 및 권장 사항이 있습니다. DIL에 대한 자세한 내용은 [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en)에서 확인할 수 있습니다.

* 이 페이지 설명에 나와 있듯이 여기에는 Adobe Analytics이 없는 AAM 고객이 사용하는 &quot;클라이언트측&quot; DIL 구현만 다룹니다. Adobe Analytics이 있는 경우 AAM을 구현하는 서버측 전달 방법을 사용해야 합니다. 이 메서드는 [설명서](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html)에 설명되어 있습니다.

## 중복 및 더 이상 사용되지 않는 요소 및 메서드 {#duplicate-and-deprecated-elements-and-methods}

이전 버전의 DIL 및 ECID에는 중복 메서드(DIL과 ECID 모두에서 동일한 기능을 수행하는 메서드)가 있어서 사용할 메서드와 혼동했었습니다. 일반적으로 두 가지를 모두 사용하고 일치시켜야 하며 해당 메시지는 고객에게 잘 전달되지 않았습니다. DIL 8.0부터 이러한 중복 방법 및 요소는 DIL에서 더 이상 사용되지 않으며, ECID 버전을 사용하는 것이 좋습니다.

예:

* [!DNL DIL.create] 사용 시 일부 요소가 더 이상 사용되지 않으므로 대신 ECID 요소를 사용해야 합니다. 이러한 요소는 [[!DNL DIL.create] 설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)에서 호출됩니다.
* [!DNL idSync] 인스턴스 수준 메서드는 메서드의 [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html)에서 호출된 대로 더 이상 사용되지 않습니다.

## 고객 ID와 ID 동기화 {#id-syncing-with-a-customer-id}

AAM에서 시스템의 UUID(익명의 고유 사용자 ID)를 고객 ID와 동기화하여 해당 고객에 대한 오프라인 데이터를 업로드하고 해당 고객의 온라인 동작과 함께 연결하여 고객을 보다 잘 이해할 수 있습니다. 이전에는 이 작업이 다음 두 가지 방법 중 하나로 수행되었습니다.

* [!DNL idSync] 인스턴스 수준 메서드
* [!DNL DIL.create]의 [!DNL declaredId] 요소

이러한 이전 방법 중 하나를 사용하여 고객 ID와 동기화한 경우에는 ECID 서비스의 일부인 [!DNL setCustomerIDs] 메서드를 사용하여 로 업데이트하는 것이 좋습니다. [!DNL setCustomerIDs]에 대한 자세한 내용은 메서드의 [documentation](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)에서 확인할 수 있습니다.

**빠른 팁:** 이전에 위의 방법 중 하나를 사용할 때  [!UICONTROL Data Source]   [!UICONTROL Data Source] ID(DPID 라고도 함)로 AAM을 참조했습니다. [!DNL setCustomerIDs](으)로 업데이트할 때에는 대신 AAM [!UICONTROL Data Source]&#39;s &quot;[!UICONTROL Integration Code]&quot;를 사용해야 합니다. 여전히 동일한 [!UICONTROL Data Source]을 가리키지만 다른 식별자입니다. 아래 비디오에 나와 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

다음 섹션에는 구현 방법을 기반으로 DIL 8.0으로 업데이트하기 위한 단계 및 권장 사항이 나열되어 있습니다.

## Adobe Experience Platform Launch에서 DIL 8.0으로 업데이트 {#updating-to-dil-in-experience-platform-launch}

DIL 8.0으로 업데이트하는 기본 단계

1. 8.0 이전 DIL을 사용하는 경우 업그레이드하기 전에 AAM 확장의 DIL 구성으로 이동한 후 사용(다음 단계에서 사용)하는 모든 고급 옵션을 확인합니다
1. AAM 확장을 버전 8.0 이상으로 업데이트합니다
1. Experience Cloud ID 서비스 확장 버전이 3.3.0 이상인지 확인하십시오
1. 8.0 이전 AAM 확장 프로그램이나 DIL에 대한 사용자 지정 코드에 있었던 더 이상 사용되지 않는 메서드/요소(예: &quot;[!DNL disableIDSyncs]&quot;)에 대해 ECID 확장에서 ECID 메서드를 활성화합니다.

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs
   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs
   1. (DIL) iframeAkamaiHTTPS -> (ECID) dSyncSSLUseAkamai
   1. (DIL) 선언된 ID -> (ECID) setCustomerIDs

1. 변경 사항 게시

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Adobe DTM에서 DIL 8.0으로 업데이트 {#updating-to-dil-in-adobe-dtm}

1. AAM 도구를 버전 8.0 이상으로 업데이트합니다. 이 버전 설정은 AAM 도구의 &quot;일반&quot; 섹션 아래에 있습니다.
1. 8.0 이전 AAM 도구의 DIL 사용자 지정 코드에 있었던 더 이상 사용되지 않는 메서드/요소(예: &quot;disableIDSyncs&quot;)에 대해 기록해 두고(ECID 도구에 추가할 수 있도록) AAM 도구의 사용자 지정 [!DNL DIL code]에서 제거합니다.
1. Experience Cloud ID 서비스 확장을 버전 3.3.0 이상으로 업데이트합니다
1. AAM 도구의 사용자 지정 코드에서 제거한 ECID 도구에 고급 옵션을 추가합니다.
1. 변경 사항 게시

## Adobe 태그 관리 솔루션이 없는 DIL 8.0으로 업데이트 {#additional-resources}

페이지에서 직접 코드를 업데이트하는 경우 위에서 설명한 대로 메서드/요소를 DIL에서 ECID로 이동해야 하는 경우를 제외하고 오래된 항목을 최신 항목으로 바꿀 수 있습니다. 이 경우 DIL 위치의 이전 메서드/요소를 ECID 위치의 ECID 메서드/요소로 대체하면 됩니다.

비Adobe 태그 관리자도 마찬가지입니다. 해당 태그 관리 솔루션에 이전 버전이 있을 때마다 다음 단계에 설명된 대로 새 코드로 바꿉니다.

1. DIL 라이브러리를 최신 버전(8.0 이상)으로 업데이트 - 현재 공개 위치에서 사용할 수 없으므로 Adobe 컨설팅이나 Adobe 고객 지원 센터에서 최신 DIL 코드를 가져와야 합니다. 그런 다음 이전 DIL 라이브러리 코드를 새 DIL 라이브러리 코드로 바꾸고 다음 단계로 이동합니다(지금 중지하지 않으면 문제가 발생할 수 있음, ha).
1. [!DNL ECID Service] 을 설치하거나 기존 버전을 3.3.0 이상으로 업데이트합니다. GitHub 페이지](https://github.com/Adobe-Marketing-Cloud/id-service/releases)에서 최신 Experience Cloud ID 서비스 릴리스 [를 다운로드할 수 있습니다. 도움이 필요한 경우 [설명서](https://experienceleague.adobe.com/docs/id-service/using/home.html)를 참조하거나 Adobe 컨설턴트에게 문의하십시오.

1. DIL에 대한 사용자 지정 코드에 있는 더 이상 사용되지 않는 메서드나 요소가 ECID 메서드로 이동되는지 확인합니다.

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs

      [설명서](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs

      [설명서](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) iframeAkamaiHTTPS -> (ECID) idSyncSSLUseAkamai

      [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) 선언된 ID -> (ECID) setCustomerIDs

      [설명서](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
