---
title: 추적 서버에서 보고서 세트 수준의 서버측 전달로 마이그레이션
description: Adobe Analytics 데이터의 서버측 전달을 Audience Manager 서버 수준이 아닌 보고서 세트 수준에서 추적하는 방법에 대해 알아봅니다.
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

# 추적 서버에서 보고서 세트 수준의 서버측 전달로 마이그레이션 {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

이 문서와 비디오에서는 의 서버측 전달을 활성화하는 방법을 보여 줍니다 [!DNL Analytics] Audience Manager 대상 데이터 [!UICONTROL report suite] 대신 레벨 [!UICONTROL tracking server] 레벨.

## 소개 {#introduction}

Adobe Audience Manager 및 Adobe Analytics이 있는 경우 의 서버측 전달을 구현할 수 있습니다. [!DNL Analytics] Audience Manager 대상 데이터. 즉, 페이지 대신 2개의 히트가 전송됩니다(1개에서 [!DNL Analytics] 및 1개를 Audience Manager으로), 히트를 [!DNL Analytics], 및 [!DNL Analytics] 는 해당 데이터를 Audience Manager에 전달합니다.

이 기능을 이미 실행 중이고 2017년 10월 이전에 활성화/구현한 경우 서버측 전달은 [!UICONTROL Tracking Server]Adobe 고객 지원 센터 또는 Adobe 컨설팅에서 활성화해야 했습니다. 2017년 10월부터 직접 서버측 전달을 구성하고 보고서 세트 수준에서 수행할 수 있습니다(보고서 세트당 전달). 이에 대한 상당한 이점이 있는데, 이에 대해서는 아래에서 살펴본다.

## [!UICONTROL Tracking server] 전달 {#tracking-server-forwarding}

사용자 [!UICONTROL tracking server] 는 을(를) 보내는 위치입니다. [!DNL Analytics] 데이터 및 이미지 요청 및 쿠키가 작성된 도메인입니다. DTM에서 또는 [!DNL Experience Platform Launch]또는 [!DNL AppMeasurement.js] 파일 및 는 일반적으로 다음과 같이 표시되며, 사이트 또는 비즈니스 이름은 &quot;mysite&quot;를 대체합니다.

`s.trackingServer = "mysite.sc.omtrdc.net";`

서버측 전달이 다음에서 전달하도록 설정되어 있는 경우 [!UICONTROL tracking server] 레벨, 여기에 전송되는 모든 히트 [!UICONTROL tracking server] (Experience Cloud ID 서비스도 활성화된 경우)가 Audience Manager으로 전달됩니다. 이는 Adobe 고객 지원 센터 또는 Adobe 컨설팅 팀이 활성화해야 했습니다. 또한 로 전환한 후 비활성화할 수 있는 대상이기도 합니다. [!UICONTROL report suite] 전달, 아래 설명.

확실하지 않은 경우 [!DNL tracking server forwarding] 이 활성화되어 있으면 Adobe 고객 지원 센터 또는 Adobe 컨설팅에 연락하여 알려 줄 수 있습니다.

## [!UICONTROL Report-suite]-레벨 서버측 전달 {#report-suite-level-server-side-forwarding}

로 이동하여 얻을 수 있는 가장 큰 이점 중 하나 [!UICONTROL report suite] 다음에서 전달: [!UICONTROL tracking server] 전달은 이제 Audience Manager 전달 기능인 &quot;Audience Analytics&quot;를 사용할 수 있다는 의미입니다 [!UICONTROL segments] 자세한 세그먼트 분석을 위해 Adobe Analytics으로 돌아갑니다. 이 기능이 을(를) 계속 사용하는 경우에는 지원되지 않습니다. [!UICONTROL tracking server] 전달 및 아님 [!UICONTROL report suite] 전달 중. 에서 Audience Analytics에 대한 자세한 내용 보기 [설명서](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 중요한 팁 {#additional-resources}

위 비디오에 명시된 대로, 모든 항목을 보유하고 나면 [!UICONTROL report suites] Audience Manager에 전달하려면 Adobe 고객 지원 또는 Adobe 컨설팅에 연락하여 [!UICONTROL tracking server] 전달 중. 둘 다 가지고 있기 때문에, 당신이 이것을 하는 것은 비상하지 않습니다 [!UICONTROL tracking server] 전달 및 [!UICONTROL report suite] 전달해도 히트가 중복되지 않습니다. 단, 다음을 보유하는 것이 좋습니다. [!UICONTROL report suite] 전달 중입니다.

떠나시면 [!UICONTROL tracking server] 전달 시,에서 데이터를 전달할 수 있을 뿐만 아니라 [!UICONTROL report suites] 전달하지 않으려 하지만, 나중에는 사용자(및 회사의 모든 사용자)가 이 사실을 잊고 [!UICONTROL tracking server] 전달이 켜져 있으면 데이터가 특정 [!UICONTROL report suite]. 이는 보고서 세트 수준에서 설정되어 있지 않지만 [!UICONTROL tracking server]. 그러면 왜 전달인지 파악하고 예상하지 못한 AAM 서버 호출에 대해서도 요금을 지불하면서 시간과 비용을 낭비하게 됩니다. 따라서 을 비활성화하는 것이 좋습니다 [!UICONTROL tracking server] 모든 을(를) 보유한 즉시 전달 [!UICONTROL report suites] 비즈니스 요구 사항에 맞는 것을 제공하도록 설정하십시오.
