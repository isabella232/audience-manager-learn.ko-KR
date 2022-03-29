---
title: 글로벌 장치 ID 유효성 검사
description: 적절한 형식을 위해 글로벌 데이터 소스로 전송되는 장치 ID의 유효성 검사와 ID 형식이 잘못된 경우 오류 메시지에 대해 알아봅니다.
feature: Data Governance & Privacy
topics: mobile
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 0ff3f123-efb3-4124-bdf9-deac523ef8c9
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '718'
ht-degree: 1%

---

# 글로벌 장치 ID 유효성 검사 {#global-device-id-validation}

장치 광고 식별자(즉, iDFA, GAID, Roku ID)에는 디지털 광고 에코시스템에서 사용할 수 있도록 하기 위해 충족해야 하는 형식 지정 기준이 있습니다. 현재, 고객 및 파트너는 ID의 형식이 제대로 지정되었는지 여부에 대한 알림을 받지 않고 어떤 형식의 글로벌 데이터 소스에 ID를 업로드할 수 있습니다. 이 기능을 사용하면 적절한 형식을 위해 글로벌 데이터 소스로 전송되는 장치 ID의 유효성 검사가 도입되며 ID의 형식이 잘못된 경우 오류 메시지가 표시됩니다. 에 대한 유효성 검사를 지원할 예정입니다. [!DNL iDFA], [!DNL Google Advertising] 및 [!DNL Roku IDs] launch에 로그인할 수 있습니다.

## 형식 표준 개요 {#overview-of-format-standards}

다음은 AAM에서 현재 인식하여 지원하는 글로벌 장치 광고 ID 풀입니다. 공유로 구현됩니다 [!UICONTROL Data Sources] 이 값은 이러한 플랫폼의 사용자에게 연결된 데이터로 작동하는 모든 고객 또는 데이터 파트너가 사용할 수 있습니다.

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
   <td>일반적으로 8-4-4-4-12으로 표시된 32개의 16진수<em>예: 97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352년</td>
   <td>이 ID는 원시/해시되지 않은/변경되지 않은 양식 참조 - <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple iOS(IDFA)</td>
   <td>20915</td>
   <td>일반적으로 8-4-4-4-12으로 표시된 32개의 16진수 <em>예, 6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560년</td>
   <td>이 ID는 원시/해시되지 않은/변경되지 않은 양식 참조 - <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>Roku(RIDA)</td>
   <td>121963</td>
   <td>일반적으로 8-4-4-4-12으로 표시된 32개의 16진수 <em>예</em> <em>fcb2a29c-315a-5e6b-bcfd-d889ba19aada</em></td>
   <td>11536</td>
   <td>이 ID는 원시/해시되지 않은/변경되지 않은 양식 참조 - <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>Microsoft 광고 ID(WIDER)</td>
   <td>389146</td>
   <td>영숫자 문자열</td>
   <td>14593</td>
   <td>이 ID는 원시/해시되지 않은/변경되지 않은 양식 참조 - <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a></td>
  </tr>
  <tr>
   <td>삼성 DUID</td>
   <td>404660</td>
   <td>영숫자 문자열 예, 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>이 ID는 원시/해시되지 않은/변경되지 않은 양식 참조 - <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## 앱에서 광고 식별자 설정 {#setting-an-advertising-identifier-in-the-app}

앱에서 광고주 ID를 설정하는 것은 실제로 두 단계로, 먼저 광고주 ID를 검색한 다음 Experience Cloud에게 보냅니다. 링크는 이러한 단계를 수행하기 위해 아래에 있습니다.

1. ID 검색
   1. [!DNL Apple] 에 대한 정보 [!DNL advertising ID] 찾을 수 있음 [여기](https://developer.apple.com/documentation/adsupport/asidentifiermanager).
   1. 설정 관련 정보 [!DNL advertiser ID] 대상 [!DNL Android] 개발자는 [여기](http://android.cn-mirrors.com/google/play-services/id.html).
1. 를 사용하여 Experience Cloud에 보냅니다. [!DNL setAdvertisingIdentifier] sdk의 메서드
   1. 사용 정보 `setAdvertisingIdentifier` 다음 중 [설명서](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier) 둘 다 [!DNL iOS] 및 [!DNL Android].

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## 잘못된 ID에 대한 DCS 오류 메시지  {#dcs-error-messaging-for-incorrect-ids}

잘못된 글로벌 장치 ID(IDFA, GAID 등)가 실시간으로 Audience Manager에 제출되면 히트 시 오류 코드가 반환됩니다. 다음은 ID가 [!DNL Apple IDFA]: 대문자만 포함해야 하고 ID에는 하위 사례 &#39;x&#39;가 있습니다.

![오류 이미지](assets/image_4_.png)

자세한 내용은 [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code) 를 입력합니다.

## 글로벌 장치 ID 온보딩 {#onboarding-global-device-ids}

글로벌 장치 ID를 실시간으로 제출하는 것 외에도 &quot;[!DNL onboard]또한 ID에 대한 &quot;(업로드) 데이터를 생성할 수 있습니다. 이 프로세스는 고객 ID에 대해 데이터를 온보딩하는 경우와 동일합니다(일반적으로 키/값 쌍을 통해). 하지만 데이터가 글로벌 장치 ID에 할당되도록 적절한 데이터 소스 ID를 사용하면 됩니다. 온보딩 프로세스에 대한 설명서는 [설명서](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides). 사용 중인 플랫폼에 따라 글로벌 데이터 소스 ID를 사용해야 합니다.

온보딩 프로세스를 통해 잘못된 글로벌 장치 ID를 제출하면 에러가 [[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting).

다음은 해당 보고서를 통해 발생할 수 있는 오류의 샘플입니다.

![오류 이미지](assets/image_5_.png)
