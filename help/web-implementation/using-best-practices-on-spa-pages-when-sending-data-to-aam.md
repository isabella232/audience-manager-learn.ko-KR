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


# AAM으로 데이터를 전송할 때 SPA 페이지에서 모범 사례 사용 {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

본 문서에서는 (SPA)에서 Adobe Audience Manager(AAM)으로 데이터를 전송할 때 따라야 하는 몇 가지 모범 사례 [!UICONTROL Single Page Applications] 에 대해 설명합니다. 이 문서는 권장 구현 방법인 사용 [!UICONTROL Experience Platform Launch]에 중점을 둡니다.

## 초기 메모

* 아래 항목은 사이트에서 구현하는 데 사용 [!DNL Platform Launch] 을 한다고 가정합니다. 고려 사항은 사용하지 않는 경우에도 존재하지만 구현 방법 [!DNL Platform Launch]에 적용해야 합니다.
* 모든 SPA은 서로 다르기 때문에 다음 항목 중 일부를 수정해야 하는 경우가 있지만, 귀하와 모범 사례를 공유하고 싶습니다.spa 페이지에서 Audience Manager으로 데이터를 전송할 때 고려해야 할 사항입니다.

## Experience Platform Launch에서 SPA 및 AAM을 사용한 간단한 작업 다이어그램 {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![aam을 위한 spa [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>앞에서 설명한 바와 같이, 이 다이어그램은 Adobe Analytics 없이 Adobe Audience Manager 구현에서 SPA 페이지를 처리하는 방법을 나타낸 것입니다 [!DNL Platform Launch]. 여러분이 볼 수 있듯이, 큰 결정은 보기 변경(또는 조치)을 어떻게 전달할지 하는 것입니다. [!DNL Platform Launch]

## SPA 페이지 [!DNL Launch] 에서 트리거 {#triggering-launch-from-the-spa-page}

규칙을 트리거하는 일반적인 두 가지 방법 [!DNL Platform Launch] (따라서 데이터를 Audience Manager으로 보내기)은 다음과 같습니다.

* JavaScript 사용자 지정 이벤트 설정(Adobe Analytics [와](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) 함께 예제 참조)
* 사용 [!UICONTROL Direct Call Rule]

이 Audience Manager 예에서, 히트가 Audience Manager으로 들어가는 것을 트리거하 [!UICONTROL Direct Call rule] 는 in [!DNL Launch] 을 사용할 것입니다. 이 기능은 다음 섹션에서 볼 수 있듯이 새 값 [!UICONTROL Data Layer] 으로 설정하여 In [!UICONTROL Data Element] 에서 선택할 수 있도록 함으로써 매우 유용합니다 [!DNL Platform Launch].

## 데모 페이지 {#demo-page}

SPA 페이지에서와 같이 Adobe에서 [!DNL data layer] 의 값을 변경하여 AAM으로 전송하는 방법을 설명하는 작은 데모 페이지를 만들었습니다. 이 기능은 보다 정교한 변경 사항을 위해 모델링할 수 있습니다. 데모 페이지는 여기에서 찾을 수 [있습니다](https://aam.enablementadobe.com/SPA-Launch.html).

##  [!DNL data layer] {#setting-the-data-layer}

앞에서 언급했듯이, 새 컨텐츠가 페이지에 로드되거나 사이트에서 작업을 수행할 때, 페이지 BEFORE [!DNL data layer] 가 호출되고 실행되는 페이지 맨 앞부분에서 동적으로 컨텐츠를 설정해야 [!DNL Launch] 합니다. 그러면 페이지에서 새 값을 선택하여 Audience Manager으로 푸시할 [!UICONTROL rules]수 [!DNL Platform Launch] [!DNL data layer] 있습니다.

위에 나열된 데모 사이트로 이동하여 페이지 소스를 보면 다음을 확인할 수 있습니다.

* 페이지 [!DNL data layer] 의 맨 앞에 있는 [!DNL Platform Launch]
* 시뮬레이트된 SPA 링크의 JavaScript가 코드를 변경하고 THEN [!UICONTROL Data Layer][!DNL Platform Launch] (_satellite.track() 호출)을 호출합니다. 이 대신 JavaScript 사용자 지정 이벤트를 사용하는 경우 [!UICONTROL Direct Call Rule]이 단원도 동일합니다. 먼저 전화 [!DNL data layer]를 변경한 다음 [!DNL Launch]전화하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 추가 리소스 {#additional-resources}

* [ADOBE 포럼 SPA 토론](https://forums.adobe.com/thread/2451022)
* [Launch by Adobe에서 SPA을 구현하는 방법을 보여주는 참조 아키텍처 사이트](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Adobe Analytics에서 SPA 추적 시 모범 사례 사용](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [이 아티클에 사용된 데모 사이트](https://aam.enablementadobe.com/SPA-Launch.html)
