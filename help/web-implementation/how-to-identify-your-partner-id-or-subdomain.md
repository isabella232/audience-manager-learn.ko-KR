---
title: 파트너 ID 또는 하위 도메인을 식별하는 방법
description: 일부 Experience Cloud 기능을 구현할 때 파트너 ID 또는 하위 도메인을 식별하는 방법과 Audience Manager UI에서 이 ID를 가져올 수 있는 두 위치에 대해 알아봅니다.
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer, Data Engineer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# Audience Manager 하위 도메인을 식별하는 방법 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

일부 Experience Cloud 기능을 구현할 때는 Audience Manager이 무엇인지 알아야 합니다 `Subdomain` 은(때로는 `client ID` 또는 `Partner ID`). 이 비디오에서는 Audience Manager UI에서 이 정보를 가져올 수 있는 두 개의 위치를 보여줍니다.

## 끝을 포기하는 중... {#giving-away-the-ending}

이 짧은 비디오를 시청하지 않고 그냥 들어가 찾으면, `Partner Subdomain` ui의 두 위치에서:

1. 을(를) 이미 만든 경우 [!UICONTROL rule-based] 트레이트, 클릭 **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] 는 해당 폴더의 트레이트 목록에 있는 트레이트 옆에 있으며 URL에 하위 도메인이 포함됩니다.
1. 에 로그인하면 **[!UICONTROL Tools]** > **[!UICONTROL Tags]** 인터페이스 및 클릭 **[!UICONTROL Get code]** 컨테이너의 경우 하위 도메인은 Akamai 라인의 끝을 향해 있습니다

이러한 빠른 참조를 통해 신속하게 찾을 수 없다면 비디오가 짧은 시간 약정이 됩니다. :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>Adobe Experience Cloud의 각 고객에게 지정된 숫자 ID가 있으며, 이를 &quot;PID&quot; 또는 파트너 ID라고 합니다. 이 문서와 비디오에서 현재 말하고 있는 ID가 아닙니다. 대신, 파트너 ID라고도 하는 &quot;파트너 하위 도메인&quot;은 일반적으로 클라이언트 이름의 버전이며 데이터가 전송되는 서버의 하위 도메인입니다. 예를 들어, 귀하가 속한 회사가 &quot;Bob&#39;s Knobs&quot;(물론, hha)라면, 파트너 하위 도메인이 &quot;bobsknobs&quot;일 가능성이 높고, &quot;PID&quot;는 &quot;12345&quot;과 비슷합니다. 일반적으로 PID를 알 필요는 없지만 Audience Manager 구현을 구성할 수 있도록 하위 도메인을 알고 있어야 합니다.
