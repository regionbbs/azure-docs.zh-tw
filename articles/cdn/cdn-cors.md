---
title: "搭配 CORS 使用 Azure CDN | Microsoft Docs"
description: "了解如何使用 Azure 內容傳遞網路 (CDN) 搭配跨來源資源共用 (CORS)。"
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: casoper
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e4f7e947ab3e9ee67224edc9cad07d82524af2b7


---
# <a name="using-azure-cdn-with-cors"></a>搭配 CORS 使用 Azure CDN
## <a name="what-is-cors"></a>CORS 是什麼？
CORS (Cross Origin Resource Sharing；跨來源資源共用) 是一項 HTTP 功能，可讓在某個網域下執行的 Web 應用程式存取其他網域中的資源。 為了減少跨網站指令碼攻擊的可能性，所有現代網頁瀏覽器都實作稱為 [Same-Origin Policy (同源原則)](http://www.w3.org/Security/wiki/Same_Origin_Policy)的安全性限制。  這樣可以防止網頁呼叫其他網域中的 API。  CORS 提供安全的方式來允許從一個網域 (來源網域) 呼叫其他網域中的 API。

## <a name="how-it-works"></a>運作方式
1. 瀏覽器傳送包含 **Origin** HTTP 標頭的 OPTIONS 要求。 這個標頭的值是提供父頁面的網域。 當來自 https://www.contoso.com 的頁面嘗試存取位於 fabrikam.com 網域的使用者資料時，會將以下要求標頭傳送到 fabrikam.com： 
   
   `Origin: https://www.contoso.com`
2. 該伺服器可能的回應如下：
   
   * 回應中的 **Access-Control-Allow-Origin** 標頭指出所允許的來源網站。 例如：
     
     `Access-Control-Allow-Origin: https://www.contoso.com`
   * 如果伺服器不允許跨來源的要求，會顯示錯誤頁面。
   * **Access-Control-Allow-Origin** 標頭包含允許所有網域的萬用字元：
     
     `Access-Control-Allow-Origin: *`

以 HTTP 要求為例，傳送完整的要求之前，會先傳送「預檢」要求以判斷是否有權限。

## <a name="wildcard-or-single-origin-scenarios"></a>萬用字元或單一來源的狀況
當 **Access-Control-Allow-Origin** 標頭設為萬用字元 (\*) 或單一來源時，Azure CDN 上的 CORS 不需要額外設定就會自動生效。  CDN 會快取第一個回應，且後續的要求將使用相同的標頭。

如果在您的來源上設定 CORS 之前已經向 CDN 發出要求，您必須清除您端點內容上的內容，以重新載入包含 **Access-Control-Allow-Origin** 標頭的內容。

## <a name="multiple-origin-scenarios"></a>多重來源的狀況
如果您要針對 CORS 允許使用特定清單中的來源，則會稍微複雜一點。 問題會在 CDN 快取第一個 CORS 來源的 **Access-Control-Allow-Origin** 標頭時發生。  當其他 CORS 來源發出後續要求時，CDN 會提供已快取的 **Access-Control-Allow-Origin** 標頭，而此標頭會不相符。  有幾個方法可以修正此問題。

### <a name="azure-cdn-premium-from-verizon"></a>來自 Verizon 的 Azure CDN 進階
完成此作業的最佳方法是使用「來自 Verizon 的 Azure CDN 進階」 ，它會公開一些進階功能。 

您必須 [建立規則](cdn-rules-engine.md) 來檢查要求的 **Origin** 標頭。  如果是有效的來源，您的規則將使用要求中提供的來源設定 **Access-Control-Allow-Origin** 標頭。  如果 **Origin** 中指定的來源是不允許的，您的規則應會忽略 **Access-Control-Allow-Origin** 標頭，這將導致瀏覽器拒絕要求。 

使用規則引擎來這樣做的方法有兩種。  在這兩種情況下，會完全忽略來自檔案原始伺服器的 **Access-Control-Allow-Origin** 標頭，而允許的 CORS 來源會完全由 CDN 的規則引擎來管理。

#### <a name="one-regular-expression-with-all-valid-origins"></a>包含所有有效來源的規則運算式
在此情況下，您會建立包含您要允許使用之所有來源的規則運算式。 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> **來自 Verizon 的 Azure CDN** 會使用 [Perl 相容的規則運算式](http://pcre.org/)做為其規則運算式的引擎。  您可以使用像 [Regular Expressions 101](https://regex101.com/) 這樣的工具來驗證您的規則運算式。  請注意，規則運算式中的「/」字元是有效的，不需要逸出，不過將該字元逸出被視為是最佳做法，且某些規則運算式驗證程式也預期將它逸出。
> 
> 

如果規則運算式相符，您的規則會將來自來源的 **Access-Control-Allow-Origin** 標頭 (如果有) 取代為傳送該要求的來源。  您也可以加入額外的 CORS 標頭，例如 **Access-Control-Allow-Methods**。

![包含規則運算式的規則範例](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a>針對每個來源要求標頭規則
除了規則運算式之外，您也可以使用**要求標頭萬用字元**[比對條件](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1)，針對每個要允許的來源建立個別規則。 如同規則運算式方法一樣，也是僅由規則引擎設定 CORS 標頭。 

![不含規則運算式的規則範例](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> 在上述範例中，萬用字元 (*) 的作用是告訴規則引擎 HTTP 和 HTTPS 兩者都要比對。
> 
> 

### <a name="azure-cdn-standard"></a>Azure CDN 標準
在 Azure CDN 標準設定檔上，在不使用萬用字元來源情況下允許多重來源的唯一機制是使用 [查詢字串快取](cdn-query-string.md)。  您需要啟用 CDN 端點的查詢字串設定，然後針對來自每個允許之網域的查詢使用唯一的查詢字串。 這樣做將使 CDN 針對每個唯一的查詢字串快取個別物件。 但是這不是最理想的方法，因為它會導致在 CDN 上快取同一個檔案的多個複本。  




<!--HONumber=Nov16_HO3-->


