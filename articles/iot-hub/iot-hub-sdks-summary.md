---
title: Azure IoT 中心 SDK 的清單 | Microsoft Docs
description: 各種 Azure IoT 中樞裝置和服務 SDK 的相關資訊和連結。
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: dobett

---
# IoT 中心 SDK
本文提供各種 [Microsoft Azure IoT SDK][Microsoft Azure IoT SDK] 的資訊，以及其他資源的連結。

## IoT 中心裝置 SDK
Microsoft Azure IoT 裝置 SDK 包含有助於建置裝置和應用程式的程式碼，並使裝置和應用程式連接到 Azure IoT 中心服務，且由其進行管理。

您可從 GitHub 下載下列 IoT 裝置 SDK：

* 為了可攜性和廣泛平台相容性，以 ANSI C (C99) 撰寫之 [Azure IoT 裝置 SDK for C][Azure IoT 裝置 SDK for C]。
* [Azure IoT 裝置 SDK for .NET][Azure IoT 裝置 SDK for .NET]
* [Azure IoT 裝置 SDK for Java][Azure IoT 裝置 SDK for Java]
* [Azure IoT 裝置 SDK for Node.js][Azure IoT 裝置 SDK for Node.js]
* [Microsoft Azure IoT 裝置 SDK for Python 2.7][Microsoft Azure IoT 裝置 SDK for Python 2.7]

### 作業系統平台與硬體相容性
如需特定硬體裝置相容性的詳細資訊，請參閱下列文章︰

* [作業系統平台與裝置 SDK 硬體相容性][OS Platforms and hardware compatibility]
* [Microsoft Azure IoT 認證方案][Microsoft Azure IoT 認證方案]。

## IoT 中心服務 SDK
Microsoft Azure IoT 服務 SDK 包含有助於建置應用程式的程式碼，並使該應用程式直接與 IoT 中樞互動，以便管理裝置和安全性。

您可從 GitHub 下載下列 IoT 服務 SDK：

* [Azure IoT 服務 SDK for Node.js][Azure IoT 服務 SDK for Node.js]
* [Azure IoT 服務 SDK for Java][Azure IoT 服務 SDK for Java]

## Azure IoT 閘道器 SDK
此 Azure IoT 閘道器 SDK 包含用來建立 IoT 閘道器方案的基礎結構和模組。您可以擴充 SDK 來建立適合任何端對端案例的閘道器。

您可以從 GitHub 下載 [Azure IoT 閘道器 SDK][Azure IoT 閘道器 SDK]。

## 線上 API 參考文件
以下是 Azure IoT 裝置、服務和閘道器程式庫的線上 API 參考文件連結清單：

* [物聯網 (IoT) .NET (英文)][物聯網 (IoT) .NET (英文)]
* [IoT 中樞 REST (英文)][IoT 中樞 REST (英文)]
* [Microsoft Azure IoT 裝置 SDK for C (英文)][Microsoft Azure IoT 裝置 SDK for C (英文)]
* [Microsoft Azure IoT 裝置 SDK for Java (英文)][Microsoft Azure IoT 裝置 SDK for Java (英文)]
* [Microsoft Azure IoT 服務 SDK for Java][Microsoft Azure IoT 服務 SDK for Java]
* [Microsoft Azure IoT 裝置 SDK for Node.js (英文)][Microsoft Azure IoT 裝置 SDK for Node.js (英文)]
* [Microsoft Azure IoT 服務 SDK for Node.js][Microsoft Azure IoT 服務 SDK for Node.js]
* [Microsoft Azure IoT 閘道器 SDK][Microsoft Azure IoT 閘道器 SDK]

## 後續步驟
若要進一步探索 IoT 中樞的功能，請參閱︰

* [設計您的解決方案][lnk-design]
* [使用範例 UI 探索裝置管理][lnk-dmui]
* [使用閘道 SDK 模擬裝置][lnk-gateway]
* [使用 Azure 入口網站管理 IoT 中樞][lnk-portal]

[Microsoft Azure IoT SDK]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[Azure IoT 裝置 SDK for C]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[Azure IoT 裝置 SDK for .NET]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[Azure IoT 裝置 SDK for Java]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[Azure IoT 服務 SDK for Java]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[Azure IoT 裝置 SDK for Node.js]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[Azure IoT 服務 SDK for Node.js]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[Microsoft Azure IoT 裝置 SDK for Python 2.7]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[OS Platforms and hardware compatibility]: iot-hub-tested-configurations.md
[Microsoft Azure IoT 認證方案]: iot-hub-tested-configurations.md#microsoft-azure-certified-for-iot
[Azure IoT 閘道器 SDK]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[物聯網 (IoT) .NET (英文)]: https://msdn.microsoft.com/library/mt488521.aspx
[Microsoft Azure IoT 裝置 SDK for C (英文)]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[Microsoft Azure IoT 裝置 SDK for Java (英文)]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[Microsoft Azure IoT 裝置 SDK for Node.js (英文)]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.14/index.html
[IoT 中樞 REST (英文)]: https://msdn.microsoft.com/library/mt548492.aspx
[Microsoft Azure IoT 服務 SDK for Java]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[Microsoft Azure IoT 服務 SDK for Node.js]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.16/index.html
[Microsoft Azure IoT 閘道器 SDK]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-design]: iot-hub-guidance.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0914_2016-->