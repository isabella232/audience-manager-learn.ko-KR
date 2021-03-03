---
title: Adobe Audience Manager DIL 버전 8.0 이상으로 업데이트
description: 이 문서에서는 AAM(Adobe Audience Manager) Data Integration Library(DIL) 코드를 버전 8.0 이상으로 업데이트하는 단계 및 권장 사항을 제공합니다. 이것은 Adobe Analytics 데이터의 서버측 전달이 아니라 "클라이언트측" DIL 구현을 참조하며 Adobe 태그 관리자 솔루션이 없는 DTM, Launch by Adobe 및 구현을 다룹니다.
feature: DIL 구현
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: '"개발자, 데이터 엔지니어"'
level: 중간
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 1%

---


# Adobe Audience Manager의 DIL 버전 8.0(이상) {#updating-to-adobe-audience-manager-s-dil-version-or-greater}으로 업데이트

이 문서에서는 Adobe Audience Manager(AAM) [!DNL Data Integration Library](DIL) 코드를 버전 8.0 이상으로 업데이트하는 단계 및 권장 사항을 제공합니다. 이것은 Adobe Analytics 데이터의 서버측 전달이 아니라 &quot;클라이언트측&quot; DIL 구현을 참조하며 Adobe 태그 관리자 솔루션이 없는 DTM, Launch by Adobe 및 구현을 다룹니다.

## 개요 {#overview}

Audience Manager의 [!DNL Data Integration Library](DIL) 코드를 사용하면 웹 사이트*에서 AAM을 구현할 수 있습니다. 이전 버전의 DIL을 구현할 때 Adobe의 ECID(Experience Cloud ID Service)도 구현하지 않아도 됩니다(매우 좋은 생각이었지만). DIL 버전 8.0부터 시작하여 ECID 버전 3.3 이상에 대한 강한 의존성이 있습니다. ECID 3.3 없이 또는 이전 버전과 함께 DIL 8.0 이상을 구현하면 오류가 발생하고 작동하지 않습니다. AAM을 구현할 수 있는 여러 가지 방법이 있기 때문에, 몇 가지 추천뿐만 아니라 단계별로 진행할 수 있도록 이 페이지를 만들었습니다. 아래에서 플랫폼/구현 방법별로 분류된 이러한 단계 및 권장 사항을 확인할 수 있습니다. DIL에 대한 자세한 내용은 [설명서](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)에서 확인할 수 있습니다.

* 이 페이지의 설명에 명시된 대로, Adobe Analytics이 없는 AAM 고객이 사용하는 &quot;클라이언트측&quot; DIL 구현만 다룹니다. Adobe Analytics이 있는 경우 AAM 구현을 위한 서버측 전달 방법을 사용해야 합니다. 이 방법은 [설명서](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html)에 설명되어 있습니다.

## 복제 및 사용되지 않는 요소 및 메서드 {#duplicate-and-deprecated-elements-and-methods}

이전 버전의 DIL 및 ECID에는 중복 방법(DIL 및 ECID 모두에서 동일한 기능을 수행하는 방법)이 있어 사용할 방법과 혼동되는 문제가 있었습니다. 일반적으로 두 가지 메시지를 모두 사용하고 일치시켜야 했으며 이 메시지는 고객에게 잘 전달되지 않았습니다. DIL 8.0부터 이러한 복제 메서드 및 요소는 DIL에서 더 이상 사용되지 않으며, ECID 버전을 사용하는 것이 좋습니다.

예:

* [!DNL DIL.create]을(를) 사용할 때 몇 가지 요소가 더 이상 사용되지 않으므로 대신 ECID 요소를 사용해야 합니다. 이러한 요소는 [[!DNL DIL.create] 설명서](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)에서 호출됩니다.
* [!DNL idSync] 인스턴스 수준 메서드도 메서드의 [documentation](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_idsync.html)에서 호출되는 대로 사용되지 않습니다.

## 고객 ID {#id-syncing-with-a-customer-id}와 동기화 중

AAM에서는 컴퓨터의 UUID(익명의 고유 사용자 ID)를 고객 ID와 동기화할 수 있으므로 해당 고객에 대한 오프라인 데이터를 업로드하고 고객의 온라인 행동과 함께 연계하여 고객을 보다 명확하게 파악할 수 있습니다. 이전에는 이러한 작업을 두 가지 방법 중 하나로 수행했습니다.

* [!DNL idSync] 인스턴스 수준 메서드
* [!DNL DIL.create]의 [!DNL declaredId] 요소

이러한 이전 방법을 사용하여 고객 ID와 동기화한 경우 ECID 서비스의 일부인 [!DNL setCustomerIDs] 메서드를 사용하도록 업데이트하는 것이 좋습니다. [!DNL setCustomerIDs]에 대한 자세한 내용은 메서드의 [documentation](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)에서 확인할 수 있습니다.

**빠른 팁:** 이전에 위 방법 중 하나를 사용할 때  [!UICONTROL Data Source] ID(&quot;DPID&quot;라고도 함) [!UICONTROL Data Source] 로 AAM을 참조했습니다. [!DNL setCustomerIDs]으로 업데이트할 때 AAM [!UICONTROL Data Source]의 &quot;[!UICONTROL Integration Code]&quot;을 대신 사용해야 합니다. 여전히 동일한 [!UICONTROL Data Source]을 가리키지만 다른 식별자에 불과합니다. 이것은 아래 비디오에 나와 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

다음 섹션에서는 구현 방법에 따라 DIL 8.0으로 업데이트하기 위한 단계 및 권장 사항을 나열합니다.

## Adobe Experience Platform Launch {#updating-to-dil-in-experience-platform-launch}의 DIL 8.0으로 업데이트

DIL 8.0으로 업데이트하는 기본 단계

1. 8.0 이전 DIL을 사용하는 경우 업그레이드하기 전에 AAM 확장의 DIL 구성으로 이동한 후 사용 중인(다음 단계에서 사용) 고급 옵션을 확인합니다.
1. AAM 확장 프로그램을 버전 8.0 이상으로 업데이트합니다.
1. Experience Cloud ID 서비스 확장 버전이 3.3.0 이상인지 확인
1. 8.0 이전 AAM 확장 프로그램 또는 DIL에 대한 사용자 정의 코드에 있었던 더 이상 사용되지 않는 메서드/요소(예: &quot;[!DNL disableIDSyncs]&quot;)에 대해 ECID 확장 프로그램에서 ECID 메서드를 활성화합니다.

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs
   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs
   1. (DIL) iframeAkamaiHTTPS -> (ECID) dSyncSSLUseAkamai
   1. (DIL) declaredId -> (ECID) setCustomerIDs

1. 변경 내용 게시

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Adobe DTM {#updating-to-dil-in-adobe-dtm}의 DIL 8.0으로 업데이트

1. AAM 도구를 버전 8.0 이상으로 업데이트합니다. 이 버전 설정은 AAM 도구의 &quot;일반&quot; 섹션 아래에 있습니다.
1. 8.0 이전 AAM 도구의 DIL 사용자 정의 코드에 있었던 더 이상 사용되지 않는 메서드/요소(&quot;disableIDSyncs&quot; 등)에 대해 ECID 도구에 해당 메서드를 추가할 수 있도록 메모해 두었다가 AAM 도구의 사용자 정의 [!DNL DIL code]에서 제거합니다.
1. Experience Cloud ID 서비스 확장을 버전 3.3.0 이상으로 업데이트합니다.
1. AAM 도구의 사용자 지정 코드에서 제거한 고급 옵션을 ECID 도구에 추가합니다.
1. 변경 내용 게시

## Adobe 태그 관리 솔루션 없음 {#additional-resources}으로 DIL 8.0으로 업데이트

페이지에서 바로 코드를 업데이트하는 경우 위에서 설명한 바와 같이 메서드/요소를 DIL에서 ECID로 이동해야 하는 경우를 제외하고, 이전 항목을 새 항목으로 교체할 수 있습니다. 이 경우 DIL 위치의 이전 메서드/요소를 ECID 위치의 ECID 메서드/요소로 간단히 대체하게 됩니다.

Adobe이 아닌 태그 관리자도 마찬가지입니다. 태그 관리 솔루션에 이전 버전이 있는 경우 다음 단계에 설명된 대로 태그 관리 솔루션을 새 코드로 바꿉니다.

1. DIL 라이브러리를 최신 버전(8.0 이상)으로 업데이트 - 현재 공개 장소에 있지 않으므로 Adobe 컨설팅 또는 Adobe 고객 지원 센터에서 최신 DIL 코드를 받아야 합니다. 그런 다음 이전 DIL 라이브러리 코드를 새 DIL 라이브러리 코드로 교체하고 다음 단계로 이동합니다(지금 중지하지 않으면 문제가 발생할 수 있음, ha).
1. [!DNL ECID Service]을(를) 설치하거나 기존 버전을 3.3.0 이상으로 업데이트합니다. 최신 Experience Cloud ID 서비스 릴리스 [는 GitHub 페이지](https://github.com/Adobe-Marketing-Cloud/id-service/releases)에서 다운로드할 수 있습니다. 도움이 필요한 경우 [설명서](https://marketing.adobe.com/resources/help/ko_KR/mcvid/)를 참조하거나 Adobe 컨설턴트에게 문의하십시오.

1. DIL에 대해 사용자 정의 코드에 있는 더 이상 사용되지 않는 메서드나 요소가 ECID 메서드로 이동되는지 확인합니다.

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs

      [설명서](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs

      [설명서](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL) iframeAkamaiHTTPS -> (ECID) idSyncSSLUseAkamai

      [설명서](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)

   1. (DIL) declaredId -> (ECID) setCustomerIDs

      [설명서](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)