---
title: 추적 서버에서 보고서 세트 수준 서버측 전달으로 마이그레이션
description: 이 문서 및 비디오에서는 추적 서버 수준이 아닌 보고서 세트 수준에서 Audience Manager으로 Analytics 데이터를 서버측 전달으로 설정하는 방법을 보여 줍니다.
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---


# 수준 [!UICONTROL Tracking Server] 으로 [!UICONTROL Report Suite]마이그레이션 [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

이 아티클 및 비디오에서는 수준 [!UICONTROL server-side forwarding] 이 아닌 수준 [!DNL Analytics] 에서 Audience Manager에 데이터를 활성화하는 방법을 [!UICONTROL report suite] [!UICONTROL tracking server] 보여 줍니다.

## 소개 {#introduction}

Adobe Audience Manager AND Adobe Analytics이 있는 경우 Audience Manager에[!UICONTROL Server-side Forwarding]데이터의 &quot; [!DNL Analytics] &quot;를 구현할 수 있습니다. 즉, 페이지가 2개의 히트(1개 대 [!DNL Analytics] 및 1개 [!DNL Analytics][!DNL Analytics] 로 히트)를 전송하는 대신 단순히 히트를 전송하고 해당 데이터를 Audience Manager으로 전달합니다. 이미 이 프로그램을 실행하고 있고 2017년 10월 이전에 활성화/구현한 경우, Adobe 고객 지원 센터 또는 Adobe 컨설팅을 통해 활성화되어야 하는 &quot; [!UICONTROL server-side forwarding][!UICONTROL Tracking Server]&quot;를 기반으로 할 수 있습니다. 2017년 10월 현재 [!UICONTROL server-side forwarding] 자신을 구성하고 [!UICONTROL Report Suite] 수준(PER 전달)으로 처리할 수 [!UICONTROL Report Suite]있습니다. 이에 대한 상당한 혜택은 아래에서 논의될 것입니다.

## [!UICONTROL Tracking Server] 전달 {#tracking-server-forwarding}

사용자 [!UICONTROL tracking server] 는 [!DNL Analytics] 데이터를 보내는 위치와 이미지 요청 및 쿠키를 쓰는 도메인입니다. DTM 또는 파일 [!DNL Experience Platform Launch]에서 설정되어야 하며, 일반적으로 이렇게 [!DNL AppMeasurement.js] 표시되고 귀하의 사이트 또는 회사 이름이 &quot;mysite&quot;를 대신합니다.

`s.trackingServer = "mysite.sc.omtrdc.net";`

레벨 [!UICONTROL server-side forwarding] 에서 전달되도록 설정된 경우, 이 [!UICONTROL tracking server] 로 전송되는 모든 히트 [!UICONTROL tracking server] (Experience Cloud ID 서비스도 활성화된 경우)가 Audience Manager으로 전달됩니다. 이 기능은 Adobe 고객 지원 센터 또는 Adobe 컨설팅을 통해 활성화되어야 했습니다. 또한, 아래에 설명된 것처럼, 귀하가 전달로 전환한 후 이것을 비활성화할 수 있는 [!UICONTROL report suite] 사람들입니다.

이 기능이 활성화되어 있는지 확실하지 않은 경우 Adobe 고객 지원 센터 또는 Adobe 컨설팅 [!DNL tracking server forwarding] 에 문의하면 해당 고객에게 알려드릴 수 있습니다.

## [!UICONTROL Report Suite]-레벨 [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

포워딩에서 [!UICONTROL report suite] 포워딩으로 전환하기 위한 가장 큰 [!UICONTROL tracking server] 이점 중 하나는 세밀한 [!UICONTROL segments] [!UICONTROL segment] 분석을 위해 Audience Manager을 다시 Adobe Analytics으로 전달하는 &quot;Audience Analytics&quot;을 사용할 수 있다는 것입니다. 이 뛰어난 기능은 전달을 계속하고 [!UICONTROL tracking server] 전달하지 않는 경우 지원되지 [!UICONTROL report suite] 않습니다. Audience Analytics에 대한 자세한 내용은 [설명서를 참조하십시오](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 중요 팁 {#additional-resources}

위의 비디오에 명시된 대로 Audience Manager으로 전달하려는 모든 [!UICONTROL report suites] 세트가 준비되면 Adobe 고객 지원 센터 또는 Adobe 컨설팅 팀에 연락하여 전달을 비활성화하도록 해야 [!UICONTROL tracking server] 합니다. 전달과 전달을 모두 [!UICONTROL tracking server] 갖춘다고 인해 중복된 히트가 발생하지 않으므로 이렇게 [!UICONTROL report suite] 하는 것은 응급상황이 아닙니다. 그러나 [!UICONTROL report suite] 전달만 하는 것이 좋습니다. 포워딩을 [!UICONTROL tracking server] 켜둔 상태에서 전달하면 전달하지 않으려는 데이터 [!UICONTROL report suites] 를 전달할 수 있을 뿐만 아니라 나중에 귀하(및 회사의 모든 사용자)가 포워딩이 켜져 있음을 확인한 후, 데이터를 특정 데이터 [!UICONTROL tracking server] (보고서 세트 수준에서 켜지지 않았기 때문)로 전달하지 않고 데이터 때문에 여전히 포워딩되고 있다고 생각할 수 [!UICONTROL report suite] [!UICONTROL tracking server]있습니다. 그렇게 하면 왜 전달되는지를 파악하는 데 시간과 비용을 낭비하게 되고 예상치 못한 AAM 서버 통화도 지불할 수 있습니다. 따라서 비즈니스 요구 사항에 맞는 전달을 [!UICONTROL tracking server] 모두 설정하는 즉시 전달 [!UICONTROL report suites] 을 비활성화하는 것이 좋습니다.
