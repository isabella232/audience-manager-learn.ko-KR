---
title: 전역 장치 ID 유효성 검사
description: '장치 광고 식별자(예: iDFA, GAID, Roku ID)에는 디지털 광고 에코시스템에서 사용할 수 있도록 하기 위해 충족되어야 하는 형식 지정 기준이 있습니다. 오늘날 고객 및 파트너는 ID의 올바른 형식 지정 여부에 대한 알림을 받지 않고 어떤 형식의 글로벌 데이터 소스에 ID를 업로드할 수 있습니다. 이 기능을 사용하면 적절한 형식을 위해 전역 데이터 소스에 보낸 장치 ID의 유효성 검사가 도입되고 ID의 형식이 잘못된 경우 오류 메시지가 표시됩니다. 실행 시 iDFA, Google Advertising 및 Roku ID에 대한 유효성 검사를 지원할 예정입니다.'
feature: data governance & privacy
topics: mobile
audience: implementer, developer, architect
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 1%

---


# 전역 장치 ID 유효성 검사 {#global-device-id-validation}

장치 광고 식별자(예: iDFA, GAID, Roku ID)에는 디지털 광고 에코시스템에서 사용할 수 있도록 하기 위해 충족되어야 하는 형식 지정 기준이 있습니다. 오늘날 고객 및 파트너는 ID의 올바른 형식 지정 여부에 대한 통보 없이 모든 형식 [!UICONTROL data sources] 으로 ID를 Adobe Global에 업로드할 수 있습니다. 이 기능을 사용하면 적절한 형식을 위해 Global으로 보내진 장치 ID의 유효성 검사 [!UICONTROL data sources] 를 도입하고 ID의 형식이 잘못된 경우 오류 메시지를 제공합니다. Adobe는 인증 [!DNL iDFA]을 지원하고 [!DNL Google Advertising] 실행 시 [!DNL Roku IDs] 실행할 것입니다.

## 형식 표준 개요 {#overview-of-format-standards}

다음은 AAM에서 현재 인식하여 지원하는 전역 장치 광고 ID 풀입니다. 이러한 데이터 [!UICONTROL Data Sources] 는 이러한 플랫폼의 사용자와 연결된 데이터로 작동하는 모든 고객 또는 데이터 파트너가 사용할 수 있는 공유로 구현됩니다.

<table>
  <tr>
   <td>플랫폼 </td>
   <td>AAM 데이터 소스 ID </td>
   <td>ID 형식 </td>
   <td>AAM PID </td>
   <td>참고 </td>
  </tr>
  <tr>
   <td>Google Android(GAID)</td>
   <td>20914</td>
   <td>32개의 16진수 숫자, 일반적으로 8-4-4-4-12<em>예, 97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352</td>
   <td>이 ID는 원시/해시되지 않음/변경되지 않은 양식 참조 - <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple iOS(IDFA)</td>
   <td>20915</td>
   <td>32개의 16진수 숫자(일반적으로 8-4-4-4-12 <em>예)로 표시됨, 6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>이 ID는 원시/해시되지 않음/변경되지 않은 양식 참조 - <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>로쿠(RIDA)</td>
   <td>121963</td>
   <td>32개의 16진수 숫자(일반적으로 8-4-4-4-12 <em>예,</em> <em>fcb2a29c-315a-5e6b-bcfd-d889ba19aada로 표시됨)</em></td>
   <td>11536</td>
   <td>이 ID는 원시/해시되지 않음/변경되지 않은 양식 참조 - <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>Microsoft 광고 ID(가정부)</td>
   <td>389146</td>
   <td>영숫자 문자열</td>
   <td>14593</td>
   <td>이 ID는 원시/해시됨/변경되지 않은 양식 참조 - <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">advertisingidhttps://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a></td>
  </tr>
  <tr>
   <td>삼성 DUID</td>
   <td>404660</td>
   <td>영숫자 문자열 예, 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>이 ID는 원시/해시되지 않음/변경되지 않은 양식 참조 - <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## 앱에서 광고 식별자 설정 {#setting-an-advertising-identifier-in-the-app}

앱에서 광고주 ID를 설정하는 과정은 두 단계로, 먼저 광고주 ID를 검색한 다음 Experience Cloud으로 보냅니다. 이러한 단계를 수행하기 위한 링크는 아래에 나와 있습니다.

1. ID 검색
   1. [!DNL Apple] 여기에 관련 정보가 [!DNL advertising ID] 있습니다 [](https://developer.apple.com/documentation/adsupport/asidentifiermanager).
   1. 개발자를 위한 설정 [!DNL advertiser ID] 에 대한 일부 정보는 [!DNL Android] 여기를 참조하십시오 [](http://www.androiddocs.com/google/play-services/id.html).
1. SDK의 방법을 사용하여 Experience Cloud으로 [!DNL setAdvertisingIdentifier] 보내기
   1. 사용 `setAdvertisingIdentifier` 에 대한 정보는 [및](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier) 모두를 위한 [!DNL iOS] 설명서에서 [!DNL Android]확인할 수있습니다.

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## 잘못된 ID에 대한 DCS 오류 메시지  {#dcs-error-messaging-for-incorrect-ids}

잘못된 글로벌 장치 ID(IDFA, GAID 등)가 실시간으로 Audience Manager에 제출되면 히트에 대해 오류 코드가 반환됩니다. 다음은 ID가 대문자만 포함해야 [!DNL Apple IDFA]하며 ID에 소문자 &#39;x&#39;가 더 작기 때문에 반환되는 오류의 예입니다.

![오류 이미지](assets/image_4_.png)

오류 코드 목록은 [설명서를](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code) 참조하십시오.

## 온보딩 글로벌 장치 ID {#onboarding-global-device-ids}

글로벌 디바이스 ID를 실시간으로 제출하는 것 외에도 ID에 대해 &quot;[!DNL onboard]&quot;(업로드) 데이터를 적용할 수도 있습니다. 이 프로세스는 고객 ID(일반적으로 키/값 쌍을 통해)에 대해 온보딩 데이터를 사용하는 경우와 동일하지만, 데이터가 글로벌 장치 ID에 할당되도록 적절한 데이터 소스 ID만 사용하면 됩니다. 온보딩 프로세스에 대한 설명서는 [설명서에 나와 있습니다](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides). 사용 중인 플랫폼에 따라 글로벌 [!UICONTROL data source] ID를 사용해야 합니다.

온보딩 프로세스를 통해 잘못된 글로벌 장치 ID를 제출하는 경우 오류가 표시됩니다 [[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting).

다음은 해당 보고서를 통해 발생할 수 있는 오류의 예입니다.

![오류 이미지](assets/image_5_.png)