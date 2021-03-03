---
title: Audience Manager에서 IAB TCF 2.0 지원
description: Adobe은 옵트인 기능 및 IAB 투명도 및 동의 프레임워크 2.0(TCF 2.0) 지원에 대한 Audience Manager 플러그인을 통해 사용자의 개인정보 보호 선택 사항을 관리하고 소통할 수 있는 방법을 제공합니다. 이 문서는 IAB TCF에 대한 Audience Manager 플러그인을 이해하고 Adobe의 옵트인 개체 및 CMP(Consent Management Provider)와 함께 사용하는 방법을 이해하는 데 도움이 되는 문서와 함께 사용할 수 있습니다.
feature: '"데이터 거버넌스 및 개인 정보"'
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
role: '"개발자, 데이터 엔지니어, 건축가"'
level: 경험
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '1131'
ht-degree: 0%

---


# Audience Manager {#iab-tcf-support-in-audience-manager}에서 IAB TCF 2.0 지원

Adobe은 옵트인 기능 및 IAB 투명도 및 동의 프레임워크 2.0(TCF 2.0) 지원에 대한 Audience Manager 플러그인을 통해 사용자의 개인정보 보호 선택 사항을 관리하고 소통할 수 있는 방법을 제공합니다. 이 문서는 IAB TCF에 대한 Audience Manager 플러그인을 이해하고 Adobe의 옵트인 개체 및 CMP(Consent Management Provider)와 함께 사용하는 방법을 이해하는 데 도움이 되는 문서와 함께 사용할 수 있습니다. IAB에 대한 자세한 내용은 해당 웹 사이트([https://www.iabeurope.eu/](https://www.iabeurope.eu/))를 참조하십시오.

## 첫 단계:ECID의 옵트인 {#first-step-understand-ecid-s-opt-in} 이해

IAB TCF를 사용하여 작업하는 방법을 이해하려면 먼저 ECID(Experience Cloud ID Service) 라이브러리의 일부인 [!DNL Opt-in] 기능을 이해해야 합니다. 옵트인 방법에 익숙하지 않은 경우 [이 유용한 아티클](https://docs.adobe.com/content/help/en/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html)을 먼저 참조하십시오. 옵트인 [설명서](https://docs.adobe.com/content/help/ko-KR/id-service/using/implementation/opt-in-service/optin-overview.html)도 검토해야 합니다. 이러한 리소스를 살펴본 후 이 페이지로 돌아가서 계속합니다.

## IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}용 Audience Manager 플러그인

이제 Opt-in 서비스의 작동 방식에 대한 기본적인 지식이 최소한 있기 때문에, Audience Manager은 Opt-in 개체에 대한 플러그인을 통해 수행하는 [!DNL IAB Transparency and Consent Framework (TCF)] 지원에 레이어를 지정할 수 있습니다.

IAB TCF용 Audience Manager 플러그인은 Opt-in의 기능을 확장하며 AAM 고객이 IAB TCF에 따라 다운스트림 파트너에게 사용자 개인 정보 선택을 평가, 인증 및 전달할 수 있도록 합니다. 데이터 컨트롤러(Adobe 고객인 경우) 및 공급업체(DMP, DSP, SSP, 광고 서버 등)에 대한 표준을 제공합니다. 동의 지형의 동의를 이해하는 데 사용할 수 있습니다.

## IAB TCF {#enabling-iab-tcf} 활성화

아래 짧은 비디오에서와 같이 IAB TCF용 Audience Manager 플러그인 활성화는 간단한 확인란이므로 Adobe Experience Platform Launch을 사용하는 경우 매우 쉽습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

또는 시작을 사용하지 않는 경우 Experience Cloud 방문자를 인스턴스화할 때 `isIabContext=true`을 사용하여 활성화할 수 있습니다. 이 작업은 IAB TCF 흐름을 시작합니다. 즉, IAB TCF를 사용하여 IAB TC 문자열을 쿼리하고 다시 Opt-in에 제공하여 Experience Cloud 솔루션과 통신합니다.

## IAB TC 문자열 {#iab-tcf-consent-string}

IAB가 제공하는 표준 중 하나는 &quot;동의 문자열&quot;(즉 &quot;데이지 비트&quot;라고도 함)로, 두 개의 목록이 하나로 결합된 것이다:

1. 목적:**동의**&#x200B;는 어떻게 해야 합니까?
1. 공급업체:**누가**&#x200B;의 동의를 받습니까?

### 목적 {#purposes}

IAB TCF 2.0에는 동의를 수집하는 10가지 &quot;목적&quot;이 있습니다(방문자 데이터로 공급업체의 작업). Adobe Audience Manager은 10가지 모두를 요구하지 않으며, 오히려 다음 목적을 위해 공급업체 동의와 함께 동의를 요구합니다.

* **목적 1: 장치에서** 정보 저장 및/또는 액세스
* **목적 10:** 제품 개발 및 개선
* **특별 목적 1:** 보안, 사기 방지 및 디버그를 보장합니다.

이것은 IAB TC 문자열의 첫 번째 부분이며, 해당 목적/활동이 승인되었는지 여부를 결정하는 1과 0으로 기록됩니다.

>[!NOTE]
>
>IAB 규정에 따라, 특별 목적 1(보안 보장, 사기 방지 및 디버그)은 항상 이에 동의하며, 사용자는 이에 반대할 수 없습니다.

### 공급업체 {#vendors}

IAB TC 문자열의 또 다른 부분은 수백 개의 벤더 목록으로, 방문자에게 사이트에 태그를 가지고 있고 사용할 벤더를 선택할 수 있는 해당 벤더 목록을 제공할 수 있습니다. 벤더는 목록에서 자신의 위치를 유지합니다. 예를 들어 이 목록의 Adobe Audience Manager 공급업체 번호는 565입니다. 목록에 있는 숫자에 &quot;1&quot;이 있는 경우 Audience Manager은 목록 맨 앞에 있는 승인된 목적을 수행할 수 있습니다. AAM 스팟에 &quot;0&quot;이 있으면 데이터로 아무 작업도 할 수 없습니다.

**고객이 IAB TCF를 사용하여 이러한 목적과 공급업체를 선택하도록 Audience Manager을 제공하거나, 모든 활동을 승인/거부하려면, IAB TCF에 등록된 CMP 파트너를 사용하거나, IAB TCF를 지원하며 IAB TCF에 등록된 CMP 파트너를 구축해야 합니다.**

## 옵트인:IAB와 Adobe 솔루션 간 번역 {#opt-in-translating-between-iab-and-adobe-solutions}

IAB TCF를 사용할 때 얻을 수 있는 이점 중 하나는 위에 나열된 표준 목적이 최종 사용자에게 Adobe 솔루션 목록보다 자신이 승인하는 사항에 대한 아이디어를 더 많이 제시한다는 것입니다. 최종 사용자는 Audience Manager 또는 [!DNL Target]을 &quot;승인&quot;하는 의미를 알지 못할 수도 있지만, &quot;장치에 정보 저장 및/또는 액세스&quot; 또는 &quot;제품 개발 및 개선&quot;을 이해하는 것이 더 쉽습니다.

Audience Manager이 승인되기 위해서는(즉, 옵트인에 대한 IAB 목적 번역을 위해 AAM에 &quot;예&quot; 투표를 실시하려면), 상기에 나열된 바와 같이, 1 및 10번째 목적을 최종 사용자로부터 승인받아야 합니다. 이러한 항목 중 하나가 승인되지 않았거나 베너가 승인되지 않은 경우에는 AAM에서 픽셀 실행을 실행하거나 쿠키를 설정하지 않습니다. 또한 많은 고객이 Audience Manager(및 기타 Experience Cloud 솔루션)의 사용을 허용 또는 거부할 수 있는 &quot;전체 또는 없음&quot; UI를 최종 사용자에게 제공하기로 선택했다는 사실을 알고 있어야 합니다.

[설명서](https://marketing.adobe.com/resources/help/en_US/aam/aam-iab-plugin.html)에는 IAB TCF용 Audience Manager 플러그인이 게시자와 광고주 사용 사례 모두에 적용되는 방법에 대한 좋은 정보가 있습니다.

## IAB:동의 다운스트림 전송 {#iab-sending-consent-downstream}

IAB TCF용 Audience Manager 플러그인을 사용하는 경우, 사용자의 동의 선택 사항도 글로벌 벤더 목록에 있는 파트너의 플랫폼 수준(제3자) ID 동기화로 전송되어 파트너가 사용자 동의 정보를 가지고 있고 이에 따라 행동할 수도 있습니다. 이 정보는 다음 두 가지 변수로 전송됩니다.

* gdpr = 1
* gdpr_concepts = [인코딩된 동의 문자열]

주의 사항은 사용자가 IAB 컨텍스트에 있고 동의를 제공하지 않는(또는 부정적인 동의 제공) 경우, Audience Manager은 IAB TC 문자열을 전혀 수집하지 않으며, 따라서 호출은 삭제됩니다. 그렇다면... 동의 하류에는 통과되지 않는 거야

## 데모 {#demo}

아래 비디오에서는 ECID 및 솔루션의 쿠키 및 비콘이 IAB 사용자 선택 사항의 영향을 받는 방식을 참조하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

구현 및 테스트 방법, 사용 사례, 작업 흐름 등 IAB TCF 2.0용 Audience Manager 플러그인에 대한 자세한 내용은 [설명서](https://docs.adobe.com/content/help/en/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)를 참조하십시오.
