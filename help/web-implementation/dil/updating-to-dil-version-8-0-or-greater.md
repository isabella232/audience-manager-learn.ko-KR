---
title: Adobe Audience Manager DIL 버전 8.0(이상)으로 업데이트
description: 이 문서에서는 Adobe Audience Manager(AAM) Data Integration Library(DIL) 코드를 버전 8.0 이상으로 업데이트하는 단계 및 권장 사항을 제공합니다. 이는 Adobe Analytics 데이터의 서버측 전달이 아니라 "클라이언트측" DIL 구현을 의미하며 Adobe 태그 관리자 솔루션이 없는 DTM, Launch by Adobe 및 구현을 다룹니다.
feature: dil implementation
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '1149'
ht-degree: 1%

---


# Adobe Audience Manager DIL 버전 8.0(이상)으로 업데이트 {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

이 문서에서는 Adobe Audience Manager(AAM) [!DNL Data Integration Library] (DIL) 코드를 버전 8.0 이상으로 업데이트하는 단계 및 권장 사항을 제공합니다. 이는 Adobe Analytics 데이터의 서버측 전달이 아니라 &quot;클라이언트측&quot; DIL 구현을 의미하며 Adobe 태그 관리자 솔루션이 없는 DTM, Launch by Adobe 및 구현을 다룹니다.

## 개요 {#overview}

Audience Manager [!DNL Data Integration Library] (DIL) 코드를 사용하면 웹 사이트에서 AAM을 구현할 수 있습니다.* 이전 버전의 DIL을 구현할 때 Adobe의 ECID(Experience Cloud ID 서비스)도 구현하지 않아도 됩니다(매우 좋은 생각이긴 하지만). DIL 버전 8.0부터 시작하여 ECID 버전 3.3 이상에 대한 강한 의존성이 있습니다. ECID 3.3 없이 또는 이전 버전에서 DIL 8.0 이상을 구현하면 오류가 발생하고 작동하지 않습니다. AAM을 구현할 수 있는 여러 가지 방법이 있으므로 몇 가지 추천과 함께 통과하기 위한 몇 가지 단계를 제공하는 이 페이지를 만들었습니다. 아래에서 플랫폼/구현 방법별로 분류된 이러한 단계 및 권장 사항을 확인할 수 있습니다. DIL에 대한 자세한 내용은 [설명서를 참조하십시오](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html).

* 이 페이지에 명시된 바와 같이, Adobe Analytics이 없는 AAM 고객이 사용하는 &quot;클라이언트측&quot; DIL 구현만 다룹니다. Adobe Analytics이 있는 경우 AAM 구현의 서버측 전달 방법을 사용해야 합니다. 이 방법은 [설명서에 설명되어 있습니다](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html).

## 복제 및 더 이상 사용되지 않는 요소 및 메서드 {#duplicate-and-deprecated-elements-and-methods}

이전 버전의 DIL 및 ECID에는 중복 방법(DIL과 ECID 모두에서 동일한 기능을 수행하는 방법)이 있어 어느 방법을 사용할지 혼동되었습니다. 일반적으로 두 가지 모두를 사용하고 일치시켜야 했고 이 메시지는 고객에게 잘 전달되지 않았습니다. DIL 8.0부터 이러한 중복 방법과 요소는 DIL에서 더 이상 사용되지 않으며, ECID 버전을 사용하는 것이 좋습니다.

예:

* 사용 시 일부 요소 [!DNL DIL.create]는 더 이상 사용되지 않으며 대신 ECID 요소를 사용해야 합니다. 이러한 요소는 [[!DNL DIL.create] 설명서에서 명시됩니다](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html).
* 또한 [!DNL idSync] 인스턴스 수준 메서드는 메서드의 설명서에서 호출되는 대로 더 이상 사용되지 [않습니다](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_idsync.html).

## 고객 ID와 동기화 {#id-syncing-with-a-customer-id}

AAM에서는 시스템의 UUID(익명 고유 사용자 ID)를 고객 ID와 동기화하여 해당 고객에 대한 오프라인 데이터를 업로드하고 해당 고객의 온라인 행동과 함께 연계하여 고객을 보다 명확하게 이해할 수 있습니다. 이전에는 이러한 작업이 두 가지 방법 중 하나로 수행되었습니다.

* 인스턴스 [!DNL idSync] 수준 메서드
* The [!DNL declaredId] element in [!DNL DIL.create]

고객 ID와 동기화하는 데 이러한 이전 방법을 사용하고 있는 경우 ECID 서비스의 일부인 메서드를 사용하도록 업데이트하는 것이 좋습니다. [!DNL setCustomerIDs] 자세한 내용 [!DNL setCustomerIDs] 은 메서드 [설명서에서 확인할 수 있습니다](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html).

**빠른 팁:** 이전에 위 방법 중 하나를 사용할 때 ID [!UICONTROL Data Source] 가 있는 AAM을 [!UICONTROL Data Source] 참조했습니다(&quot;DPID&quot;). 로 업데이트할 [!DNL setCustomerIDs]때 AAM의 &quot; [!UICONTROL Data Source]&quot;를 대신 사용해야[!UICONTROL Integration Code]합니다. 여전히 같은 점을 가리키지만 [!UICONTROL Data Source] 는 다른 식별자에 불과합니다. 아래 비디오에 나와 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

다음 섹션에서는 구현 방법에 따라 DIL 8.0으로 업데이트하기 위한 단계 및 권장 사항을 나열합니다.

## Adobe Experience Platform Launch의 DIL 8.0으로 업데이트 {#updating-to-dil-in-experience-platform-launch}

DIL 8.0으로 업데이트하는 기본 단계

1. 8.0 이전 DIL을 사용하는 경우 업그레이드하기 전에 AAM 확장 프로그램의 DIL 구성으로 이동한 후 사용 중인 고급 옵션(다음 단계에서 사용)을 참고합니다.
1. AAM 익스텐션을 버전 8.0 이상으로 업데이트합니다.
1. Experience Cloud ID 서비스 확장 버전이 3.3.0 이상인지 확인합니다.
1. 8.0 이전 AAM 익스텐션 또는 DIL에 대한 사용자 정의 코드에 있었던 더 이상 사용되지 않는 메서드/요소(&quot;[!DNL disableIDSyncs]&quot; 등)의 경우 ECID 익스텐션에서 ECID 메서드를 활성화합니다.

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs
   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs
   1. (DIL) iframeAkamaiHTTPS -> (ECID) dSyncSSLUseAkamai
   1. (DIL) decabledId -> (ECID) setCustomerIDs

1. 변경 내용 게시

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Adobe DTM의 DIL 8.0으로 업데이트 {#updating-to-dil-in-adobe-dtm}

1. AAM 도구를 버전 8.0 이상으로 업데이트합니다. 이 버전 설정은 AAM 도구의 &quot;일반&quot; 섹션 아래에 있습니다.
1. DIL에 대한 8.0 이전 AAM 도구의 사용자 정의 코드에 있었던 더 이상 사용되지 않는 메서드/요소(&quot;disableIDSyncs&quot; 등)에 대해서는 메모해 두고(ECID 도구에 이들을 추가할 수 있도록) AAM 도구의 사용자 정의 [!DNL DIL code] 에서 제거합니다.
1. Experience Cloud ID 서비스 익스텐션을 버전 3.3.0 이상으로 업데이트합니다.
1. AAM 도구의 사용자 지정 코드에서 제거한 고급 옵션을 ECID 도구에 추가합니다.
1. 변경 내용 게시

## Adobe 태그 관리 솔루션이 없는 DIL 8.0으로 업데이트 {#additional-resources}

페이지에서 바로 코드를 업데이트하는 경우 위에서 설명한 바와 같이 메서드/요소를 DIL에서 ECID로 이동해야 하는 경우를 제외하고 이전 항목을 새 항목으로 바꿀 수 있습니다. 이 경우 DIL 위치의 이전 방법/요소를 ECID 위치의 ECID 방법/요소로 대체하면 됩니다.

비Adobe 태그 관리자도 마찬가지입니다. 태그 관리 솔루션에 이전 버전이 있는 경우 다음 단계에 설명된 대로 새 코드로 대체합니다.

1. DIL 라이브러리를 최신 버전(8.0 이상)으로 업데이트 - 현재 공개 장소에서 제공되지 않으므로 Adobe 컨설팅 또는 Adobe 고객 지원 센터에서 최신 DIL 코드를 받아야 합니다. 그런 다음 이전 DIL 라이브러리 코드를 새 DIL 라이브러리 코드로 교체하고 다음 단계로 이동합니다(지금 중지하지 않으면 문제가 발생할 수 있음, 하).
1. 기존 버전 [!DNL ECID Service] 을 3.3.0 이상으로 설치하거나 업데이트합니다. GitHub 페이지에서 최신 Experience Cloud ID 서비스 릴리스 [를 다운로드할 수 있습니다](https://github.com/Adobe-Marketing-Cloud/id-service/releases). 도움이 필요한 경우 [설명서를](https://marketing.adobe.com/resources/help/ko_KR/mcvid/) 참조하거나 Adobe 컨설턴트에게 문의하십시오.

1. DIL에 대한 사용자 지정 코드에 있는 더 이상 사용되지 않는 메서드나 요소가 ECID 메서드로 이동되는지 확인합니다.

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs

      [설명서](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs

      [설명서](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL) iframeAkamaiHTTPS -> (ECID) idSyncSSLUseAkamai

      [설명서](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)

   1. (DIL) decabledId -> (ECID) setCustomerIDs

      [설명서](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)