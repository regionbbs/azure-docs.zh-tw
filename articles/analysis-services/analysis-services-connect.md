---
title: "從 Azure Analysis Services 取得資料 | Microsoft Docs"
description: "了解如何在 Azure 連線至 Analysis Services 伺服器並從該伺服器中取得資料。"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: b37f70a0-9166-4173-932d-935d769539d1
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/28/2016
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 193c939065979dc48243d31e7f97cd87d96bf9a8
ms.openlocfilehash: 6b65689c75247af81e916730f125ff39bb49489f


---
# <a name="get-data-from-azure-analysis-services"></a>從 Azure Analysis Services 取得資料
您在 Azure 中建立一個伺服器，並將表格式模型部署至該伺服器後，您組織中的使用者便可以連線與開始瀏覽資料。

Azure Analysis Services 支援用戶端使用[更新後的物件模型](#client-libraries)；TOM、AMO、Adomd.Net 或 MSOLAP，透過 xmla 連線至伺服器。 例如，Power BI、Power BI Desktop、Excel 或任何支援物件模型的協力廠商用戶端應用程式。

## <a name="server-name"></a>伺服器名稱
當您在 Azure 中建立 Analysis Services 伺服器時，您可以指定唯一的名稱和要建立伺服器的區域。 在連線中指定伺服器名稱時，伺服器命名配置為：

```
<protocol>://<region>/<servername>
```
 其中 protocol 是字串 **asazure**，region 是伺服器建立所在區域的 URI (例如若為美國西部，則為 westus.asazure.windows.net)，servername 是在該區域中您唯一伺服器的名稱。

## <a name="get-the-server-name"></a>取得伺服器名稱
連線之前，您必須先取得伺服器名稱。 在 [Azure 入口網站] > 伺服器 > [概觀]  >  [伺服器名稱] 中，複製整個伺服器名稱。 如果您的組織中有其他使用者也連線到這部伺服器，請將此伺服器名稱告訴他們。 指定伺服器名稱時，必須使用完整路徑。

![在 Azure 中取得伺服器名稱](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

## <a name="connect-in-power-bi-desktop"></a>在 Power BI Desktop 中連線
> [!NOTE]
> 此功能為預覽。
> 
> 

1. 在 [Power BI Desktop](https://powerbi.microsoft.com/desktop/) 中，按一下 [取得資料]  >  [資料庫]  >  [Azure Analysis Services]。
2. 在 [伺服器] 中，貼上剪貼簿中的伺服器名稱。
3. 在 [資料庫] 中，如果您知道所要連線表格式模型資料庫或檢視方塊的名稱，請在此貼上。 否則，您可以將此欄位保留空白。 您可以在下一個畫面上選取資料庫或檢視方塊。
4. 讓預設的 [即時連接] 選項保持為已選取，然後按下 [連線]。 如果提示您輸入帳戶，請輸入您的組織帳戶。
5. 在 [導覽器] 中展開伺服器，然後選取您要連線的模型或檢視方塊，再按一下 [連線]。 在模型或檢視方塊上按一下，便會顯示該檢視的所有物件。

## <a name="connect-in-power-bi"></a>在 Power BI 中連線
1. 建立與您伺服器上模型即時連線的 Power BI Desktop 檔案。
2. 在 [Power BI](https://powerbi.microsoft.com) 中，按一下 [取得資料]  >  [檔案]。 找到並選取您的檔案。

## <a name="connect-in-excel"></a>在 Excel 中連線
支援使用 Excel 2016 中的 [取得資料] 或舊版中的 Power Query，在 Excel 中連線至 Azure Analysis Services 伺服器。 必須有 [MSOLAP.7 提供者](https://aka.ms/msolap)。 不支援在 Power Pivot 中使用 [匯入資料表精靈] 連線。

1. 在 Excel 2016 中的 [資料] 功能區上，按一下 [Get External Data] (取得外部資料)  >  **[從其他來源]**  >  [From Analysis Services] (從 Analysis Services)。
2. 在 [資料連線精靈] 的 [伺服器名稱] 中，貼上剪貼簿中的伺服器名稱。 然後在 [登入認證] 中，選取 [使用下列的使用者名稱和密碼]，接著再輸入組織使用者名稱 (例如 nancy@adventureworks.com,) 和密碼。
   
    ![在 Excel 登入時連線](./media/analysis-services-connect/aas-connect-excel-logon.png)
3. 在 [選取資料庫和資料表] 中，選取資料庫和模型或檢視方塊，然後按一下 [完成]。
   
    ![在 Excel 選取模型中連線](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Connection string
使用表格式物件模型連線至 Azure Analysis Services 時，請使用下列連接字串格式：

###### <a name="integrated-azure-active-directory-authentication"></a>整合型 Azure Active Directory 驗證
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
整合型驗證將挑選 Azure Active Directory 認證快取 (若有的話)。 如果沒有，則會顯示 Azure 登入視窗。

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>以使用者名稱與密碼進行的 Azure Active Directory 驗證
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>用戶端程式庫
從 Excel 或其他介面 (如 TOM、AsCmd、ADOMD.NET) 連線至 Azure Analysis Services 時，您可能需要安裝最新版的提供者用戶端程式庫。 取得最新版本︰  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>後續步驟
[管理您的伺服器](analysis-services-manage.md)




<!--HONumber=Nov16_HO3-->


