---
title: AAM으로 데이터를 보낼 때 SPA 페이지에 대한 우수 사례 사용
description: 이 문서에서는 SPA(단일 페이지 애플리케이션)에서 Adobe Audience Manager(AAM)으로 데이터를 전송할 때 따라야 하는 몇 가지 우수 사례에 대해 설명합니다. 이 문서에서는 권장 구현 방법인 Launch by Adobe 사용에 중점을 둡니다.
feature: 구현 기본 사항
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: '"개발자, 데이터 엔지니어"'
level: 경험
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 0%

---


# AAM {#using-best-practices-on-spa-pages-when-sending-data-to-aam}에 데이터를 보낼 때 SPA 페이지에 대한 우수 사례 사용

이 문서에서는 [!UICONTROL Single Page Applications](SPA)에서 Adobe Audience Manager(AAM)으로 데이터를 전송할 때 따라야 하는 몇 가지 우수 사례에 대해 설명합니다. 이 문서는 권장 구현 방법인 [!UICONTROL Experience Platform Launch]을 사용하는 것에 중점을 둡니다.

## 초기 메모

* 아래 항목은 [!DNL Platform Launch]을(를) 사용하여 사이트에서 구현한다고 가정합니다. [!DNL Platform Launch]을(를) 사용하지 않을 경우에도 고려 사항이 여전히 존재하지만 구현 방법에 적용되어야 합니다.
* 모든 SPA은 다르므로 귀하의 요구에 가장 잘 맞도록 다음 항목 중 일부를 수정해야 할 수도 있지만, 귀하와 모범 사례를 공유하고자 합니다.SPA 페이지에서 Audience Manager으로 데이터를 전송할 때 고려해야 할 사항입니다.

## Experience Platform Launch {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}에서 SPA 및 AAM을 사용하여 작업하는 간단한 다이어그램

![aam을 위한 스파  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>앞에서 설명한 바와 같이, 이 다이어그램은 [!DNL Platform Launch]을(를) 사용하여 Adobe Audience Manager 구현(Adobe Analytics 제외)에서 SPA 페이지를 처리하는 방법을 간략히 나타낸 것입니다. 볼 수 있듯이, 보기 변경(또는 작업)을 [!DNL Platform Launch]으로 전달하는 방법을 결정하는 것은 매우 간단합니다.

## SPA 페이지 {#triggering-launch-from-the-spa-page}에서 [!DNL Launch] 트리거

[!DNL Platform Launch]에서 규칙을 트리거하는 일반적인 방법 중 두 가지는 다음과 같습니다(따라서 데이터를 Audience Manager으로 보내기).

* JavaScript 사용자 지정 이벤트 설정(Adobe Analytics에서 [HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) 참조)
* [!UICONTROL Direct Call Rule] 사용

이 Audience Manager 예제에서는 [!DNL Launch]의 [!UICONTROL Direct Call rule]을 사용하여 Audience Manager으로 가는 히트를 트리거합니다. 다음 섹션에서 확인하듯이 이 기능은 [!UICONTROL Data Layer]을(를) 새 값으로 설정하여 [!DNL Platform Launch]의 [!UICONTROL Data Element]에서 선택할 수 있도록 함으로써 매우 유용합니다.

## 데모 페이지 {#demo-page}

SPA 페이지에서 하는 것처럼 [!DNL data layer]의 값을 변경하고 AAM으로 전송하는 방법을 보여 주는 작은 데모 페이지를 만들었습니다. 이 기능은 필요한 보다 정교한 변경 사항을 위해 모델링할 수 있습니다. 이 데모 페이지 [HERE](https://aam.enablementadobe.com/SPA-Launch.html)을 찾을 수 있습니다.

##  [!DNL data layer] {#setting-the-data-layer}

앞에서 언급했듯이 새 컨텐츠가 페이지에 로드되거나 다른 사람이 사이트에서 작업을 수행할 때 [!DNL data layer]은(는) 페이지 헤드에서 동적으로 설정되어야 합니다. [!DNL Launch]이 호출되고 [!UICONTROL rules]가 실행되기 전에 [!DNL Platform Launch]이(가) [!DNL data layer]에서 새 값을 선택하여 Audience Manager으로 푸시할 수 있습니다.

위에 나열된 데모 사이트로 이동하여 페이지 소스를 보면 다음과 같은 내용이 표시됩니다.

* [!DNL data layer]은(는) [!DNL Platform Launch]에 대한 호출 전에 페이지의 헤드에 있습니다.
* 시뮬레이션된 SPA 링크의 JavaScript는 [!UICONTROL Data Layer]을 변경하고 THEN은 [!DNL Platform Launch](_satellite.track() 호출)을 호출합니다. 이 [!UICONTROL Direct Call Rule] 대신 JavaScript 사용자 지정 이벤트를 사용하는 경우 이 단원은 동일합니다. 먼저 [!DNL data layer]을 변경한 다음 [!DNL Launch]을(를) 호출합니다.

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 추가 리소스 {#additional-resources}

* [Adobe 포럼에 대한 SPA 토론](https://forums.adobe.com/thread/2451022)
* [Launch by Adobe에서 SPA을 구현하는 방법을 보여주는 참조 아키텍처 사이트](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Adobe Analytics에서 SPA을 추적할 때 모범 사례 사용](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [이 아티클에 사용된 데모 사이트](https://aam.enablementadobe.com/SPA-Launch.html)
