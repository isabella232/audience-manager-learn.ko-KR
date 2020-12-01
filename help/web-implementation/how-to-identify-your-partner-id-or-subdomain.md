---
title: Audience Manager 파트너 ID 또는 하위 도메인을 식별하는 방법
description: 일부 Experience Cloud 기능을 구현할 때는 Audience Manager "파트너 ID"가 무엇인지 알아야 합니다("클라이언트 ID" 또는 "하위 도메인"이라고도 함). 이 비디오에서는 Audience Manager UI에서 이 ID를 얻을 수 있는 두 가지 위치를 보여줍니다.
feature: implementation basics
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 0%

---


# Audience Manager 하위 도메인을 식별하는 방법 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

일부 Experience Cloud 기능을 구현할 때는 Audience Manager `Subdomain`이 무엇인지 알아야 합니다(경우에 따라 `client ID` 또는 `Partner ID`라고도 함). 이 비디오에서는 Audience Manager UI에서 이 정보를 얻을 수 있는 두 가지 방법을 보여 줍니다.

## 끝 부분을 포기하는 중.. {#giving-away-the-ending}

이 짧은 비디오를 보지 않고 바로 들어가서 찾을 수 있는 것이 더 나을 경우 UI의 두 위치에서 `Partner Subdomain`을(를) 찾을 수 있습니다.

1. [!UICONTROL rule-based] [!UICONTROL trait]을(를) 이미 만든 경우 **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] 가 해당 폴더 [!UICONTROL trait] 의 목록 옆에 있고 URL에 하위 도메인 [!UICONTROL traits] 이 포함됩니다.
1. **[!UICONTROL Tools]** **[!UICONTROL Tags]** 인터페이스로 이동하고 컨테이너에 대해 **[!UICONTROL Get code]**&#x200B;를 클릭하면 하위 도메인이 Akamai 라인 끝을 향해 있습니다

빠른 참조를 사용하여 빠르게 찾을 수 없는 경우 비디오는 짧은 시간 약정이 됩니다. :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>Adobe Experience Cloud의 각 고객에게 숫자 ID가 할당되어 있으며 이를 &quot;PID&quot; 또는 파트너 ID라고 합니다. 이 아티클 및 비디오에서 말하고 있는 ID가 아닙니다. 대신, 파트너 ID라고도 하는 &quot;파트너 하위 도메인&quot;은 일반적으로 클라이언트 이름의 버전이며 데이터가 전송되는 서버의 하위 도메인입니다. 예를 들어, 귀하의 회사가 &quot;Bob&#39;s Knobs&quot;(모든 것은 문지기, 하하)인 경우, 당신의 파트너 하위 도메인이 &quot;bobsknobs&quot;일 가능성이 높고, 반면 &quot;PID&quot;는 &quot;12345&quot;와 비슷한 것일 수 있습니다. 일반적으로 PID를 알 필요는 없지만, Audience Manager 구현을 구성할 수 있도록 하위 도메인을 알아야 합니다.

