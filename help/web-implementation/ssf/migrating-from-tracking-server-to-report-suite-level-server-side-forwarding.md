---
title: 추적 서버에서 보고서 세트 수준 서버 측 전달로 마이그레이션
description: 추적 서버 수준이 아닌 보고서 세트 수준에서 Adobe Analytics 데이터를 Audience Manager에 서버측 전달을 활성화하는 방법을 알아봅니다.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer, Data Engineer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
source-git-commit: 4adaade180545bcf5f911b7453a7e9939e2ed178
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# 추적 서버에서 보고서 세트 수준 서버 측 전달로 마이그레이션 {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

이 문서와 비디오에서는 의 서버측 전달을 활성화하는 방법을 보여줍니다 [!DNL Analytics] 데이터 Audience Manager [!UICONTROL report suite] at이 아닌 level [!UICONTROL tracking server] 수준.

## 소개 {#introduction}

Adobe Audience Manager AND Adobe Analytics이 있는 경우 의 서버측 전달을 구현할 수 있습니다 [!DNL Analytics] Audience Manager 데이터. 즉, 페이지에서 두 개의 히트(하나로에)를 전송하는 대신 [!DNL Analytics] 및 하나로에 Audience Manager) 히트를 보낼 수 있습니다. [!DNL Analytics], 및 [!DNL Analytics] 은 해당 데이터를 Audience Manager에 전달합니다.

이미 이 기능을 실행 중이고 2017년 10월 이전에 활성화/구현한 경우 서버 측 전달은 을 기반으로 할 수 있습니다 [!UICONTROL Tracking Server]: 고객 지원 센터 또는 Adobe 컨설팅 센터에서 활성화해야 합니다. 이제 2017년 10월 현재, 서버측 전달을 직접 구성하고 보고서 세트 수준(보고서 세트당 전달)에서 수행할 수 있습니다. 이에 대해 아래 논의된 상당한 이점이 있습니다.

## [!UICONTROL Tracking server] 전달 {#tracking-server-forwarding}

사용자 [!UICONTROL tracking server] 는 전송하는 위치입니다 [!DNL Analytics] 또한, 이미지 요청 및 쿠키가 작성된 도메인이기도 합니다. DTM에서 또는 [!DNL Experience Platform Launch]또는 [!DNL AppMeasurement.js] 파일이며 은 일반적으로 이렇게 표시되고 사이트 또는 비즈니스 이름은 &quot;mysite&quot;를 대체합니다.

`s.trackingServer = "mysite.sc.omtrdc.net";`

에서 서버 측 전달을 전달하도록 설정된 경우 [!UICONTROL tracking server] 레벨, 이 페이지로 전송되고 있는 모든 히트 [!UICONTROL tracking server] (Experience Cloud ID 서비스가 또한 활성화되어 있으면) Audience Manager에게 전달됩니다. 이 기능은 Adobe 고객 지원 센터 또는 Adobe 컨설팅 팀이 활성화해야 합니다. 또한 사용자가 전환한 후 비활성화할 수 있는 사용자입니다 [!UICONTROL report suite] 아래 설명된 대로 전달.

확실하지 않은 경우 [!DNL tracking server forwarding] 이 활성화되어 있으면 Adobe 고객 지원 센터 또는 Adobe 컨설팅 팀에 문의하며, 문의할 수 있습니다.

## [!UICONTROL Report-suite]-수준 서버 측 전달 {#report-suite-level-server-side-forwarding}

로 이동하는 데 가장 큰 이점 중 하나 [!UICONTROL report suite] 전달 [!UICONTROL tracking server] 전달을 통해 이제 Audience Manager 전달 기능인 &quot;Audience Analytics&quot;을 사용할 수 있습니다 [!UICONTROL segments] 자세한 세그먼트 분석을 위해 Adobe Analytics으로 다시 돌아갑니다. 이 멋진 기능은 아직 켜져 있지 않으면 지원되지 않습니다 [!UICONTROL tracking server] 전달 및 아님 [!UICONTROL report suite] 전달. 에서 Audience Analytics에 대한 자세한 내용은 [설명서](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 중요 팁 {#additional-resources}

위의 비디오에 명시된 대로, 모든 항목이 [!UICONTROL report suites] Audience Manager으로 전달하려는 경우 Adobe 고객 지원 센터 또는 Adobe 컨설팅 팀에 연락하여를 비활성화하도록 해야 합니다 [!UICONTROL tracking server] 전달. 두 가지를 모두 가지고 있기 때문에 당신이 이것을 하는 것은 응급상황이 아닙니다 [!UICONTROL tracking server] 전달 및 [!UICONTROL report suite] 전달해도 중복 히트가 발생하지 않습니다. 그러나 다음과 같은 경우에만 사용하는 것이 좋습니다 [!UICONTROL report suite] 전달 설정.

나가시면 [!UICONTROL tracking server] 전달을 통해 데이터를 [!UICONTROL report suites] 전달을 원하지 않지만, 나중에 사용자(및 회사의 모든 사용자)가 이 사실을 잊어버립니다 [!UICONTROL tracking server] 전달이 켜진 경우 특정 데이터에 대해 데이터가 전달되지 않는다고 생각할 수 있습니다 [!UICONTROL report suite]. 이것은 보고서 세트 수준에서 켜져 있지 않지만, [!UICONTROL tracking server]. 그런 다음 왜 전달되는지 파악하는 데 시간과 비용을 낭비하고 예상치 못한 AAM 서버 호출에 대한 비용도 지불하게 됩니다. 그러니까, [!UICONTROL tracking server] 모든 것이 있는 즉시 전달 [!UICONTROL report suites] 비즈니스 요구 사항에 적합한 솔루션을 제공하도록 설정
