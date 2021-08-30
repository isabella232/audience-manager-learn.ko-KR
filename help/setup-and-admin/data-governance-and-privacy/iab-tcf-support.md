---
title: Audience Manager에서 IAB TCF 2.0 지원
description: Adobe은 옵트인 기능 및 Audience Manager 플러그인을 통해 IAB 투명성 및 동의 프레임워크 2.0(TCF 2.0) 지원을 통해 사용자의 개인 정보 보호 선택을 관리 및 소통할 수 있는 수단을 제공합니다. 이 문서는 IAB TCF에 대한 Audience Manager 플러그인을 이해하고 Adobe의 옵트인 개체 및 CMP(동의 관리 공급자)와 함께 작동하는 방법을 이해하는 데 도움이 되는 설명서와 함께 작동합니다.
feature: Data Governance & Privacy
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '1120'
ht-degree: 0%

---

# Audience Manager에서 IAB TCF 2.0 지원 {#iab-tcf-support-in-audience-manager}

Adobe은 옵트인 기능 및 Audience Manager 플러그인을 통해 IAB 투명성 및 동의 프레임워크 2.0(TCF 2.0) 지원을 통해 사용자의 개인 정보 보호 선택을 관리 및 소통할 수 있는 수단을 제공합니다. 이 문서는 IAB TCF에 대한 Audience Manager 플러그인을 이해하고 Adobe의 옵트인 개체 및 CMP(동의 관리 공급자)와 함께 작동하는 방법을 이해하는 데 도움이 되는 설명서와 함께 작동합니다. IAB에 대한 자세한 내용은 웹 사이트([https://www.iabeurope.eu/](https://www.iabeurope.eu/))를 참조하십시오.

## 첫 번째 단계: ECID의 옵트인 이해 {#first-step-understand-ecid-s-opt-in}

IAB TCF를 사용하는 방법을 이해하려면 먼저 ECID(Experience Cloud ID 서비스) 라이브러리의 일부인 [!DNL Opt-in] 기능을 이해해야 합니다. 옵트인 작동 방식을 잘 모를 경우 먼저 [이 유용한 문서](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html)를 참조하십시오. 또한 옵트인 [설명서](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html)도 검토해야 합니다. 해당 리소스를 살펴본 후 이 페이지로 돌아가서 계속합니다.

## IAB TCF용 Audience Manager 플러그인 {#the-audience-manager-plug-in-for-iab-tcf}

이제 옵트인 서비스 작동 방식에 대한 기본적인 지식이 적어도 있으므로 Audience Manager은 해당 옵트인 개체에 플러그인을 통해 수행되는 [!DNL IAB Transparency and Consent Framework (TCF)] 지원을 계층화할 수 있습니다.

IAB TCF용 Audience Manager 플러그인은 옵트인의 기능을 확장하며, AAM 고객이 IAB TCF에 따라 다운스트림 파트너에게 사용자 개인 정보 보호 선택 사항을 평가, 준수 및 전달할 수 있도록 해줍니다. 데이터 컨트롤러(Adobe 고객인 경우)와 공급업체(DMP, DSP, SSP, 광고 서버 등)에 대한 표준을 제공합니다. 를 사용하여 동의 구간에 대한 동의를 이해할 수 있습니다.

## IAB TCF 활성화 {#enabling-iab-tcf}

아래 짧은 비디오에 표시된 것처럼 간단한 확인란이므로 IAB TCF용 Audience Manager 플러그인 활성화는 Adobe Experience Platform Launch을 사용하는 경우 쉽게 수행할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

또는 Launch를 사용하지 않는 경우 Experience Cloud 방문자를 인스턴스화할 때 `isIabContext=true` 을 사용하여 활성화할 수 있습니다. 이렇게 하면 IAB TCF 플로우가 시작됩니다. 즉, IAB TCF를 사용하여 IAB TCF 문자열을 쿼리하고 다시 옵트인에 제공하고 Experience Cloud 솔루션과 통신합니다.

## IAB TC 문자열 {#iab-tcf-consent-string}

IAB가 제공하는 표준 중 하나는 &quot;동의 문자열&quot;(즉, &quot;DaisyBit&quot;이라고도 함)이며, 두 목록은 실제로 함께 연결되어 있습니다.

1. 목적: **어떤**&#x200B;에 동의해야 합니까?
1. 공급업체: **누가**&#x200B;에 동의합니까?

### 목적 {#purposes}

IAB TCF 2.0을 사용하는 경우 (공급업체가 방문자의 데이터로 수행할 수 있는 작업)에 동의를 수집하는 10가지 &quot;목적&quot;이 있습니다. Adobe Audience Manager은 10개의 모든 동의를 필요로 하지 않으며, 대신 공급업체 동의 외에 다음 목적을 위한 동의만 필요합니다.

* **목적 1:** 장치에 정보를 저장 및/또는 액세스하는 방법
* **목적 10:** 제품 개발 및 개선
* **특수 목적 1:** 보안 확보, 사기 방지 및 디버그

이 부분은 IAB TC 문자열의 첫 번째 부분이며, 해당 목적/활동이 승인되었는지 여부를 나타내는 1과 0으로 방금 기록됩니다.

>[!NOTE]
>
>IAB 규정에 따라 특수 목적 1(보안 확인, 사기 방지 및 디버그)이 항상 동의되며 사용자는 이에 대해 이의를 제기할 수 없습니다.

### 공급업체 {#vendors}

IAB TC 문자열의 또 다른 부분은 수백 개의 공급업체 긴 목록이므로 방문자에게 사이트에 태그가 있고 사용할 벤더를 선택할 수 있는 적용 가능한 공급업체 목록을 제공할 수 있습니다. 노점상들은 목록에 자신의 위치를 유지한다. 예를 들어 이 목록에 있는 Adobe Audience Manager의 공급업체 번호는 565입니다. 목록의 해당 숫자에 &quot;1&quot;이 있다면, Audience Manager은 목록의 맨 앞부분에서 승인된 목적을 수행할 수 있습니다. AAM spot에 &quot;0&quot;이 있는 경우 데이터로 작업을 수행할 수 없습니다.

**고객이 IAB TCF를 사용하여 이러한 목적과 공급업체의 선택을 하거나, 모든 활동을 승인/거부하기 위한 UI를 제공하는 Audience Manager은 IAB TCF에 등록된 CMP 파트너를 사용하거나, IAB TCF를 지원하고 IAB TCF에 등록된 CMP 파트너를 작성해야 합니다.**

## 옵트인: IAB와 Adobe 솔루션 간 번역 {#opt-in-translating-between-iab-and-adobe-solutions}

IAB TCF를 사용할 경우 얻을 수 있는 이점 중 하나는 위에 나열된 표준 목적이 최종 사용자에게 Adobe 솔루션 목록보다 자신이 승인하는 것에 대한 생각을 더 많이 줄 수 있다는 것입니다. 최종 사용자는 &quot;승인&quot; Audience Manager 또는 [!DNL Target]의 의미를 모를 수 있지만, &quot;장치에 대한 정보 저장 및/또는 액세스&quot; 또는 &quot;제품 개발 및 개선&quot;은 이해하기 쉽고 동의를 얻을 수 있습니다.

Audience Manager을 승인하려면(즉, 옵트인에 대한 IAB 목적 번역을 위해 AAM에 &quot;예&quot; 투표를 하려면), 위에 나열된 대로 목적 1 및 10을 최종 사용자의 동의를 받아야 합니다. 이러한 항목이 승인되지 않았거나 또는 벤더가 승인되지 않은 경우에는 AAM에서 픽셀을 실행하거나 쿠키를 설정하지 않습니다. 또한 많은 고객이 최종 사용자에게 Audience Manager(및 기타 Experience Cloud 솔루션)의 사용을 허용 또는 허용하지 않는 &quot;모두 또는 없음&quot; UI를 제공하도록 선택한다는 것을 알고 있는 것이 좋습니다.

[설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=en)에 IAB TCF 플로우용 Audience Manager 플러그인이 게시자와 광고주 사용 사례 모두에 적용되는 방식에 대한 몇 가지 중요한 정보가 있습니다.

## IAB: 다운스트림으로 동의 보내기 {#iab-sending-consent-downstream}

IAB TCF용 Audience Manager 플러그인을 사용하면 사용자의 동의 선택 사항이 글로벌 공급업체 목록에 있는 파트너에 대한 플랫폼 수준(타사) ID 동기화에도 전송되어 파트너가 사용자 동의 정보를 가지고 있으며 이에 대해서도 조치를 취할 수 있습니다. 이 정보는 다음 두 가지 변수로 전송됩니다.

* gdpr = 1
* gdpr_consent = [인코딩된 동의 문자열]

주의 사항은 사용자가 IAB 컨텍스트에 있고 동의를 제공하지 않는(또는 음의 동의 제공) 경우, Audience Manager이 IAB TC 문자열을 전혀 수집하지 않으며 이러한 호출이 삭제된다는 것입니다. 그래서, 그 경우에는... 동의 하류의 통과가 없습니다.

## 데모 {#demo}

아래 비디오에서는 ECID 및 솔루션의 쿠키 및 비콘이 IAB 사용자 선택 사항의 영향을 받는 방식을 확인할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

구현 및 테스트, 사용 사례, 워크플로우 등 IAB TCF 2.0용 Audience Manager 플러그인에 대한 자세한 내용은 [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)를 참조하십시오.
