---
title: AAM으로 데이터를 전송할 때 SPA 페이지에서 모범 사례 사용
description: 본 문서에서는 단일 페이지 애플리케이션(SPA)에서 Adobe Audience Manager(AAM)으로 데이터를 전송할 때 따라야 하는 몇 가지 모범 사례를 설명합니다. 이 문서는 권장 구현 방법인 Launch by Adobe 사용에 중점을 둡니다.
feature: implementation basics
topics: spa
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---


# AAM {#using-best-practices-on-spa-pages-when-sending-data-to-aam}으로 데이터를 전송할 때 SPA 페이지에서 모범 사례 사용

이 문서에서는 [!UICONTROL Single Page Applications](SPA)에서 Adobe Audience Manager(AAM)으로 데이터를 전송하는 경우 알아야 하는 몇 가지 우수 사례에 대해 설명합니다. 이 문서는 권장 구현 방법인 [!UICONTROL Experience Platform Launch]을 사용하는 것에 중점을 둡니다.

## 초기 메모

* 아래 항목은 사이트에서 구현하기 위해 [!DNL Platform Launch]을(를) 사용하고 있다고 가정합니다. [!DNL Platform Launch]을(를) 사용하지 않을 경우에도 고려 사항이 여전히 존재하지만, 구현 방법에 부합되어야 합니다.
* 모든 SPA은 서로 다르기 때문에 다음 항목 중 일부를 수정해야 하는 경우가 있지만, 귀하와 모범 사례를 공유하고 싶습니다.spa 페이지에서 Audience Manager으로 데이터를 전송할 때 고려해야 할 사항입니다.

## Experience Platform Launch {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}에서 SPA 및 AAM으로 작업하는 간단한 다이어그램

![aam을 위한 spa  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>앞에서 설명한 바와 같이, 이 다이어그램은 SPA 페이지가 Adobe Analytics 없이 Adobe Audience Manager 구현에서 처리되는 방식을 나타낸 것입니다([!DNL Platform Launch] 사용). 여러분이 볼 수 있듯이, 이것은 매우 직설적인 결정이며, 여러분이 보기 변경(또는 작업)을 [!DNL Platform Launch]에 어떻게 전달할 것인가가 중요한 결정입니다.

## SPA 페이지 {#triggering-launch-from-the-spa-page}에서 [!DNL Launch] 트리거

[!DNL Platform Launch]에서 규칙을 트리거하고 데이터를 Audience Manager으로 전송하는 가장 일반적인 방법 중 두 가지는 다음과 같습니다.

* JavaScript 사용자 지정 이벤트 설정(예: [HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) with Adobe Analytics)
* [!UICONTROL Direct Call Rule] 사용

이 Audience Manager 예에서 [!DNL Launch]에 [!UICONTROL Direct Call rule]을 사용하여 히트가 Audience Manager으로 진입하도록 트리거합니다. 다음 섹션에서 확인하듯이 이 기능은 [!UICONTROL Data Layer]을(를) 새 값으로 설정하여 [!DNL Platform Launch]의 [!UICONTROL Data Element]에서 선택할 수 있도록 함으로써 매우 유용합니다.

## 데모 페이지 {#demo-page}

SPA 페이지에서 할 수 있듯이 [!DNL data layer]의 값을 변경하고 AAM으로 전송하는 방법을 보여주는 작은 데모 페이지를 만들었습니다. 이 기능은 보다 정교한 변경 사항을 위해 모델링할 수 있습니다. 이 데모 페이지 [HERE](https://aam.enablementadobe.com/SPA-Launch.html)를 찾을 수 있습니다.

##  [!DNL data layer] {#setting-the-data-layer}

앞에서 언급했듯이 새 컨텐츠가 페이지에 로드되거나 사이트에서 작업을 수행할 때 [!DNL data layer]이 페이지 헤드에서 동적으로 설정되어야 BEFORE [!DNL Launch]이 호출되고 [!UICONTROL rules]가 실행되므로 [!DNL Platform Launch]이(가) [!DNL data layer]에서 새 값을 선택하여 Audience Manager으로 푸시할 수 있습니다.

위에 나열된 데모 사이트로 이동하여 페이지 소스를 보면 다음을 확인할 수 있습니다.

* [!DNL data layer]은 [!DNL Platform Launch]에 대한 호출 전에 페이지의 헤드에 있습니다.
* 시뮬레이션된 SPA 링크의 JavaScript는 [!UICONTROL Data Layer]을 변경하고 THEN은 [!DNL Platform Launch](_satellite.track() 호출)을 호출합니다. 이 [!UICONTROL Direct Call Rule] 대신 JavaScript 사용자 지정 이벤트를 사용하는 경우 단원이 동일합니다. 먼저 [!DNL data layer]을 변경한 다음 [!DNL Launch]을(를) 호출합니다.

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 추가 리소스 {#additional-resources}

* [ADOBE 포럼 SPA 토론](https://forums.adobe.com/thread/2451022)
* [Launch by Adobe에서 SPA을 구현하는 방법을 보여주는 참조 아키텍처 사이트](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Adobe Analytics에서 SPA 추적 시 모범 사례 사용](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [이 아티클에 사용된 데모 사이트](https://aam.enablementadobe.com/SPA-Launch.html)
