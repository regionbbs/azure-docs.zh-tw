---
title: "Azure RemoteApp 範本映像中有什麼內容？ | Microsoft Docs"
description: "了解 Azure RemoteApp 隨附的範本映像。"
services: remoteapp
documentationcenter: 
author: lizap
manager: mbaldwin
ms.assetid: 7f8442b2-81da-421e-a453-aa53ba2066b7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2016
ms.author: elizapo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b589fb3b1cdbf1f14ece6adf43e1eb0313ff09df


---
# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Azure RemoteApp 範本映像中有什麼內容？
> [!IMPORTANT]
> Azure RemoteApp 即將中止。 如需詳細資訊，請參閱 [公告](https://go.microsoft.com/fwlink/?linkid=821148) 。
> 
> 

Azure RemoteApp 訂用帳戶包含三個範本映像：

* Windows Server 2012
* Microsoft Office 365 ProPlus (需有 Office 365 訂用帳戶)
* Microsoft Office 2013 Professional Plus (僅限試用版)

> [!IMPORTANT]
> 您的 Azure RemoteApp 訂用帳戶讓您可以存取映像中的軟體，除了需要個別訂用帳戶的 Office 365 ProPlus，與不能用在生產環境中的 Office 2013 外。 這表示您可以與使用者共用範本映像上的程式或應用程式。 例如，如果您建立使用 Windows Server 2012 R2 映像的集合，則可以發佈 System Center Endpoint Protection，讓使用者透過 RemoteApp 存取。
> 
> 如需詳細資訊，請查看 [RemoteApp 授權詳細資訊](remoteapp-licensing.md) 。 此外，如需 Office 授權資訊，請參閱 [使用 Office 與 Azure RemoteApp 搭配](remoteapp-o365.md) 。
> 
> 

閱讀每個映像包含之內容的詳細資訊。

## <a name="windows-server-2012-r2-the-vanilla-image"></a>Windows Server 2012 R2 (「Vanilla 映像」)
此映像以 Microsoft Windows Server 2012 R2 Datacenter 作業系統為基礎，並已安裝下列角色和功能，以符合 Azure RemoteApp 範本映像的需求：

* .NET Framework 4.5、3.5.1、3.5
* 桌面體驗
* 筆跡和手寫服務
* 媒體基礎
* 遠端桌面工作階段主機
* Windows PowerShell 4.0
* Windows PowerShell ISE
* WoW64 支援

此映像也安裝了下列應用程式：

* Adobe Flash Player
* Microsoft Silverlight
* Microsoft System Center 2012 Endpoint Protection
* Microsoft Windows Media Player

## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (需有訂用帳戶)
Office 365 是最常要求的應用程式，因此我們建立「 自訂 」映像供您使用。

此映像是 Vanilla 映像的延伸，而且除了 Windows Server 2012 R2 映像中描述的元件外，還安裝了下列 Microsoft Office 365 ProPlus 元件：

* Access
* Excel
* Lync
* OneNote
* 商務用 OneDrive (請注意，不支援同步代理程式使用於 Azure RemoteApp)
* Outlook
* PowerPoint
* Word
* Microsoft Office 校訂工具

映像也包含 Visio Pro 及 Project Pro。

以及下列應用程式：

* SQL 原生用戶端
* ODBC 驅動程式
* SQL Server 資料採礦用戶端
* MasterDataServices 用戶端
* Microsoft Publisher
* PowerQuery
* PowerMap

Office 365 ProPlus 應用程式的完整功能只適用於擁有 Office 365 ProPlus 方案的使用者。 如需 Office 365 訂用計畫的詳細資訊，請參閱 [Office 365 服務方案](http://technet.microsoft.com/library/office-365-plan-options.aspx)。 還有疑問嗎？ 請查看 [Office 365 + RemoteApp](remoteapp-o365.md) 資訊。 同時請查看新的文章： [如何搭配 Azure RemoteApp 使用 Office 365 訂用帳戶](remoteapp-officesubscription.md)。

請注意您必須個別授權 Office 365 ProPlus、Visio Pro 和 Project Pro，他們都有自己的授權。

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (僅限試用版)
在免費試用期間，您可以使用 Office 2013 映像測試服務。

此映像是 Vanilla 映像的延伸，而且除了 Windows Server 2012 R2 映像中描述的元件外，還安裝了下列 Microsoft Office 2013 Professional Plus 元件：

* Access
* Excel
* Lync
* OneNote
* 商務用 OneDrive (請注意，不支援同步代理程式使用於 Azure RemoteApp)
* Outlook
* PowerPoint
* 隨附此逐步解說的專案
* Visio
* Word
* Microsoft Office 校訂工具

> [!IMPORTANT]
> **法律資訊：**此映像不包含 Microsoft Office 授權，且「無法用於生產環境」。 Office 2013 Professional Plus 映像僅作為試用之用。 如果您想要在 Azure RemoteApp 中使用 Office 應用程式作為生產之用，您必須使用 Office 365 ProPlus 映像。 如需授權 Office 的詳細資訊，請參閱 [使用 Office 365 與 Azure RemoteApp 搭配](remoteapp-o365.md)
> 
> 




<!--HONumber=Nov16_HO2-->


