---
title: "IoT 中樞作業監視"
description: "Azure IoT 中樞的作業監視概觀，可讓您即時監視其 IoT 中樞上的作業狀態。"
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a299f3a5-b14d-4586-9c3b-44aea14ed013
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: nberdy
translationtype: Human Translation
ms.sourcegitcommit: ce514e19370d2b42fb16b4e96b66f212d5fa999c
ms.openlocfilehash: 3c1ae13409c11ec49810209dd155e934b34c3a9b


---
# <a name="introduction-to-operations-monitoring"></a>作業監視簡介
IoT 中樞的作業監視可讓您即時監視其 IoT 中樞上的作業狀態。 IoT 中樞可追蹤橫跨數個作業類別的事件。 您可以選擇將一或多個類別的事件傳送至 IoT 中樞的端點進行處理。 您可以監視資料中是否有錯誤，或根據資料模式設定更複雜的處理行為。

IoT 中樞會監視五個類別的事件：

* 裝置身分識別作業
* 裝置遙測
* 雲端到裝置的命令
* 連線
* 檔案上傳

## <a name="how-to-enable-operations-monitoring"></a>如何啟用作業監視
1. 建立 IoT 中樞。 您可以在[開始使用][lnk-get-started]指南中找到如何建立 IoT 中樞的指示。
2. 開啟 IoT 中樞的刀鋒視窗。 按一下其中的 [作業監視] 。
   
    ![][1]
3. 選取您要監視的監視類別，然後按一下 [儲存] 。 您可以從 [監視設定] 中所列出的事件中樞相容端點讀取事件。 IoT 中樞端點稱為 `messages/operationsmonitoringevents`。
   
    ![][2]

## <a name="event-categories-and-how-to-use-them"></a>事件類別和其使用方式
每個作業監視類別會各自追蹤與 IoT 中樞所進行的不同類型的互動，而每一個監視類別都有結構描述來定義該類別的事件要如何構成。

### <a name="device-identity-operations"></a>裝置身分識別作業
裝置身分識別作業類別會追蹤您嘗試在其 IoT 中樞的身分識別登錄中建立、更新或刪除項目時所發生的錯誤。 佈建案例就很適合追蹤此類別。

    {
        "time": "UTC timestamp",
         "operationName": "create",
         "category": "DeviceIdentityOperations",
         "level": "Error",
         "statusCode": 4XX,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "durationMs": 1234,
         "userAgent": "userAgent",
         "sharedAccessPolicy": "accessPolicy"
    }

### <a name="device-telemetry"></a>裝置遙測
裝置遙測類別會追蹤在 IoT 中樞發生，且與遙測管線相關的錯誤。 此類別包括在傳送遙測事件 (例如節流) 和接收遙測事件 (例如未經授權的讀取器) 時所發生的錯誤。 請注意，這個類別無法捕捉裝置本身所執行之程式碼所造成的錯誤。

    {
         "messageSizeInBytes": 1234,
         "batching": 0,
         "protocol": "Amqp",
         "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "time": "UTC timestamp",
         "operationName": "ingress",
         "category": "DeviceTelemetry",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": "UTC timestamp"
    }

### <a name="cloud-to-device-commands"></a>雲端到裝置的命令
雲端到裝置的命令類別會追蹤在 IoT 中樞發生，且與裝置命令管線相關的錯誤。 這個類別包括在傳送命令 (例如未經授權的寄件者)、接收命令 (例如超過傳遞計數) 和接收命令回饋 (例如回饋已過期) 時所發生的錯誤。 此類別不會捕捉未正確處理命令但卻將其成功傳遞之裝置所發生的錯誤。

    {
         "messageSizeInBytes": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "deliveryAcknowledgement": 0,
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "C2DCommands",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": “UTC timestamp"
    }

### <a name="connections"></a>連線
連線類別會追蹤當裝置與 IoT 中樞連線或中斷連線時發生的錯誤。 若想識別未經授權的連線嘗試，以及在連線品質不佳之區域內的裝置中斷連線時進行追蹤，就很適合追蹤此類別。

    {
         "durationMs": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "deviceConnect",
         "category": "Connections",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID"
    }

### <a name="file-uploads"></a>檔案上傳

檔案上傳類別會追蹤在 IoT 中樞發生且與檔案上傳功能相關的錯誤。 此類別包括︰

- SAS URI 所發生的錯誤，例如當它在裝置通知中樞已完成上傳之前就到期時。
- 裝置所報告的失敗上傳。
- IoT 中樞通知訊息建立期間在儲存體中找不到檔案時所發生的錯誤。

請注意，此類別無法捕捉直接發生在裝置將檔案上傳到儲存體時的錯誤。


    {
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "HTTP",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "fileUpload",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "blobUri": "http//bloburi.com",
         "durationMs": 1234
    }

## <a name="next-steps"></a>後續步驟
若要進一步探索 IoT 中樞的功能，請參閱︰

* [開發人員指南][lnk-devguide]
* [使用 IoT 閘道 SDK 來模擬裝置][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md



<!--HONumber=Nov16_HO5-->


