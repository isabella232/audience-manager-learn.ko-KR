---
title: 추적 서버에서 보고서 세트 수준 서버측 전달으로 마이그레이션
description: 이 문서 및 비디오에서는 추적 서버 수준이 아니라 보고서 세트 수준에서 Audience Manager으로 Analytics 데이터를 서버측에서 전달하는 방법을 보여 줍니다.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: "Developer, Data Engineer"
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
translation-type: tm+mt
source-git-commit: 256edb05f68221550cae2ef7edaa70953513e1d4
workflow-type: tm+mt
source-wordcount: '580'
ht-degree: 0%

---

# [!UICONTROL Tracking Server]에서 [!UICONTROL Report Suite]-Level [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}(으)로 마이그레이션

이 문서 및 비디오에서는 [!UICONTROL tracking server] 수준이 아닌 [!UICONTROL report suite] 수준에서 Audience Manager에 대한 데이터의 [!UICONTROL server-side forwarding]을(를) 활성화하는 방법을 보여 줍니다.[!DNL Analytics]

## 소개 {#introduction}

Adobe Audience Manager AND Adobe Analytics이 있는 경우 [!DNL Analytics] 데이터의 &quot;[!UICONTROL Server-side Forwarding]&quot;을(를) Audience Manager에 구현할 수 있습니다. 즉, 2개의 히트(한 개의 히트를 [!DNL Analytics] 및 한 개의 히트를 Audience Manager으로 전송하는 페이지 대신 단순히 [!DNL Analytics]에 히트를 보낼 수 있으며, [!DNL Analytics]는 해당 데이터를 Audience Manager으로 전달합니다. 이미 이 기능을 실행 중이며 2017년 10월 이전에 활성화/구현한 경우, [!UICONTROL server-side forwarding]은(는) Adobe 고객 지원 센터 또는 Adobe 컨설팅을 통해 활성화되어야 했던 &quot;[!UICONTROL Tracking Server]&quot;을 기반으로 할 수 있습니다. 2017년 10월 현재 [!UICONTROL server-side forwarding]을(를) 직접 구성하고 [!UICONTROL Report Suite] 수준(PER [!UICONTROL Report Suite] 전달)에서 수행할 수 있습니다. 이에 대한 상당한 이점들이 있으며, 그것은 아래에 논의될 것입니다.

## [!UICONTROL Tracking Server] 전달  {#tracking-server-forwarding}

[!UICONTROL tracking server]은 [!DNL Analytics] 데이터를 전송하는 위치와 이미지 요청 및 쿠키를 쓰는 도메인이기도 합니다. DTM 또는 [!DNL Experience Platform Launch] 또는 [!DNL AppMeasurement.js] 파일에서 설정해야 하며 일반적으로 이렇게 표시됩니다. 이 이름은 사이트 또는 비즈니스 이름으로 &quot;mysite&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

[!UICONTROL server-side forwarding]이(가) [!UICONTROL tracking server] 수준에서 전달되도록 설정된 경우 이 [!UICONTROL tracking server](Experience Cloud ID 서비스도 활성화된 경우)로 전송되는 모든 히트가 Audience Manager으로 전달됩니다. 이 기능은 Adobe 고객 지원 센터 또는 Adobe 컨설팅을 통해 활성화되어야 했습니다. 또한, 아래 설명된 대로 [!UICONTROL report suite] 전달로 전환한 후 비활성화할 수 있는 사람도 해당됩니다.

[!DNL tracking server forwarding]이(가) 활성화되어 있는지 확실하지 않은 경우 Adobe 고객 지원 센터 또는 Adobe 컨설팅 팀에 문의하면 언제든지 문의할 수 있습니다.

## [!UICONTROL Report Suite]-레벨  [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

[!UICONTROL tracking server] 전달에서 [!UICONTROL report suite] 전달으로 전환하는 것이 가장 큰 이점 중 하나는 &quot;Audience Analytics&quot;을 사용할 수 있다는 것입니다. 이 기능은 Audience Manager [!UICONTROL segments]을 다시 Adobe Analytics으로 전달하여 자세한 [!UICONTROL segment] 분석을 수행할 수 있습니다. [!UICONTROL report suite] 전달이 아닌 [!UICONTROL tracking server] 전달을 진행하는 경우에는 이 뛰어난 기능이 지원되지 않습니다. [설명서](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/)에서 Audience Analytics에 대한 자세한 내용을 참조하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 중요 팁 {#additional-resources}

위의 비디오에 명시된 대로, Audience Manager으로 전달하려는 [!UICONTROL report suites]을 모두 설정한 후에는 Adobe 고객 지원 센터 또는 Adobe 컨설팅 팀에 연락하여 [!UICONTROL tracking server] 전달을 비활성화하도록 해야 합니다. [!UICONTROL tracking server] 전달과 [!UICONTROL report suite] 전달을 모두 사용할 경우 중복 히트가 발생하지 않으므로 이 작업을 수행할 필요는 없습니다. 그러나 [!UICONTROL report suite] 전달만 하는 것이 좋습니다. [!UICONTROL tracking server]을(를) 계속 전달하면 [!UICONTROL report suites]의 데이터를 전달할 수 있을 뿐만 아니라, 나중에 [!UICONTROL tracking server] 전달이 설정되어 있다는 것을 (및 회사의 모든 사람)이 잊어버린 후, 데이터가 특정 [!UICONTROL report suite](보고서 세트 수준에서 켜져 있지 않으므로)에 대해 전달되지 않는다고 생각할 수 있지만 데이터는 &lt;a> 때문에 여전히 전달되고 있습니다. 4/>. [!UICONTROL tracking server] 그렇게 하면 왜 전달되고 있는지 알아내고 예상치 못한 AAM 서버 호출을 지불하면서 시간과 돈을 낭비하게 됩니다. 따라서 [!UICONTROL report suites]을(를) 모두 전달하여 비즈니스 요구 사항에 맞는 [!UICONTROL tracking server] 전달을 비활성화하는 것이 좋습니다.
