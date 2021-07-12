---
title: AAM에 데이터를 보낼 때 SPA 페이지에서 모범 사례 사용
description: 이 문서에서는 SPA(단일 페이지 애플리케이션)에서 Adobe Audience Manager(AAM)으로 데이터를 보낼 때 따라야 하는 몇 가지 우수 사례를 설명합니다. 이 설명서는 권장되는 구현 방법인 Launch by Adobe 사용에 중점을 둡니다.
feature: 구현 기본 사항
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: Developer, Data Engineer
level: Experienced
exl-id: 99ec723a-dd56-4355-a29f-bd6d2356b402
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---

# AAM에 데이터를 보낼 때 SPA 페이지에서 모범 사례 사용 {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

이 문서에서는 [!UICONTROL Single Page Applications](SPA)에서 Adobe Audience Manager(AAM)으로 데이터를 보낼 때 따라야 하는 몇 가지 우수 사례를 설명합니다. 이 설명서는 권장되는 구현 방법인 [!UICONTROL Experience Platform Launch] 사용에 중점을 둡니다.

## 초기 노트

* 아래 항목에서는 [!DNL Platform Launch]을 사용하여 사이트에서 를 구현한다고 가정합니다. [!DNL Platform Launch] 을 사용하지 않는 경우에도 고려 사항이 여전히 존재하지만 구현 방법을 조정해야 합니다.
* 모든 SPA이 다르므로 다음 항목 중 일부를 조정해야 사용자의 요구 사항을 가장 잘 충족할 수 있지만 일부 우수 사례를 여러분과 공유하려고 했습니다. SPA 페이지에서 Audience Manager으로 데이터를 보낼 때 고려해야 하는 사항입니다.

## Experience Platform Launch에서 SPA 및 AAM으로 작업하는 간단한 다이어그램 {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![aam용 spa  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>명시된 대로, [!DNL Platform Launch]을 사용하여 Adobe Audience Manager 구현(Adobe Analytics 없음)에서 SPA 페이지를 처리하는 방법을 간소화된 다이어그램입니다. 알 수 있듯이 이는 매우 직선적으로 진행되며, 큰 결정은 보기 변경(또는 작업)을 [!DNL Platform Launch]에 전달하는 방법입니다.

## SPA 페이지에서 [!DNL Launch] 트리거 {#triggering-launch-from-the-spa-page}

[!DNL Platform Launch]에서 규칙을 트리거하기 위한 보다 일반적인 방법 중 두 가지는 다음과 같습니다(따라서 데이터를 Audience Manager으로 전송).

* JavaScript 사용자 지정 이벤트 설정(예: [HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) with Adobe Analytics 참조)
* [!UICONTROL Direct Call Rule] 사용

이 Audience Manager 예에서는 [!DNL Launch]에 [!UICONTROL Direct Call rule]을 사용하여 히트가 Audience Manager으로 들어가는 것을 트리거합니다. 다음 부분에서 확인했듯이 이 기능은 [!UICONTROL Data Layer]을(를) 새 값으로 설정하여 [!DNL Platform Launch]에서 [!UICONTROL Data Element]에 의해 선택될 수 있도록 함으로써 매우 유용합니다.

## 데모 페이지 {#demo-page}

SPA 페이지에서 하듯이 [!DNL data layer]에서 값을 변경하고 AAM으로 전송하는 방법을 보여 주는 작은 데모 페이지를 만들었습니다. 이 기능은 필요한 보다 정교한 변경을 위해 모델링할 수 있습니다. 이 데모 페이지 [HERE](https://aam.enablementadobe.com/SPA-Launch.html)를 찾을 수 있습니다.

##  [!DNL data layer] {#setting-the-data-layer}

위에서 언급했듯이 새 컨텐츠가 페이지에 로드되거나 사이트에서 작업을 수행하는 경우 [!DNL data layer]을(를) 페이지 헤드의 [!DNL Launch] 호출에서 동적으로 설정하고 [!UICONTROL rules]를 실행하여 [!DNL Platform Launch]이(가) [!DNL data layer]에서 새 값을 선택하여 Audience Manager에 푸시할 수 있도록 설정해야 합니다.

위에 나열된 데모 사이트로 이동하여 페이지 소스를 확인하면 다음이 표시됩니다.

* [!DNL data layer]은 [!DNL Platform Launch] 호출 전 페이지 헤드에 있습니다.
* 시뮬레이션된 SPA 링크의 JavaScript가 [!UICONTROL Data Layer]를 변경한 다음 THEN이 [!DNL Platform Launch](_satellite.track() 호출)를 호출합니다. 이 [!UICONTROL Direct Call Rule] 대신 JavaScript 사용자 지정 이벤트를 사용하는 경우 단원은 동일합니다. 먼저 [!DNL data layer]을 변경한 다음 [!DNL Launch]을 호출합니다.

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 추가 리소스 {#additional-resources}

* [Adobe 포럼에 대한 SPA 토론](https://forums.adobe.com/thread/2451022)
* [Launch by Adobe에서 SPA을 구현하는 방법을 보여주는 참조 아키텍처 사이트](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Adobe Analytics에서 SPA을 추적할 때 모범 사례 사용](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [이 문서에 사용된 데모 사이트](https://aam.enablementadobe.com/SPA-Launch.html)
