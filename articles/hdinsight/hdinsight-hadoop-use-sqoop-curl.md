---
title: 在 HDInsight 中搭配使用 Hadoop Sqoop 與 Curl | Microsoft Docs
description: 了解如何使用 Curl 從遠端提交 Sqoop 工作到 HDInsight。
services: hdinsight
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal

ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/25/2016
ms.author: jgao

---
# 使用 Curl 在 HDInsight 中以 Hadoop 執行 Sqoop 作業
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

在本文件中，您將學習如何使用 Curl 在 Azure HDInsight 叢集的 Hadoop 上執行 Sqoop 作業。

本文件使用 Curl 示範如何使用未經處理的 HTTP 要求來與 HDInsight 互動，以便執行、監視和擷取 Sqoop 作業的結果。要想執行這些作業，就要使用 HDInsight 叢集所提供的 WebHCat REST API (先前稱為 Templeton)。

> [!NOTE]
> 如果您已熟悉使用以 Linux 為基礎的 Hadoop 伺服器，但剛接觸 HDInsight，請參閱[在以 Linux 為基礎的 HDInsight 上安裝 Hadoop 的須知事項](hdinsight-hadoop-linux-information.md)。
> 
> 

## 必要條件
若要完成本文中的步驟，您需要下列項目。

* HDInsight 叢集上的 Hadoop (Linux 或 Windows 型)
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## 使用 Curl 提交 Sqoop 作業
> [!NOTE]
> 在使用 Curl 或與 WebHCat 進行任何其他 REST 通訊時，您必須提供 HDInsight 叢集系統管理員的使用者名稱和密碼來驗證要求。您也必須在用來將要求傳送至伺服器的統一資源識別項 (URI) 中使用叢集名稱。
> 
> 在本節的所有命令中，將 **USERNAME** 取代為用來驗證叢集的使用者，並將 **PASSWORD** 取代為使用者帳戶的密碼。將 **CLUSTERNAME** 取代為您叢集的名稱。
> 
> 透過[基本驗證](http://en.wikipedia.org/wiki/Basic_access_authentication)來保護 REST API 的安全。您應該一律使用安全 HTTP (HTTPS) 提出要求，確保認證安全地傳送至伺服器。
> 
> 

1. 從命令列中，使用下列命令來確認您可以連線到 HDInsight 叢集：
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    您應該會收到如下所示的回應：
   
        {"status":"ok","version":"v1"}
   
    此命令中使用的參數如下：
   
   * **-u** - 用來驗證要求的使用者名稱和密碼。
   * **-G** - 指出這是 GET 要求。
     
     所有要求的 URL 開頭 (**https://CLUSTERNAME.azurehdinsight.net/templeton/v1**) 都會相同。路徑 **/status** 指出要求是要傳回伺服器之 WebHCat (也稱為 Templeton) 的狀態。
2. 使用以下命令提交 Sqoop 作業：

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    此命令中使用的參數如下：

    * **-d** - 因為未使用 `-G`，要求會依預設使用 POST 方法。`-d` 可指定與要求一起傳送的資料值。

        * **user.name** - 執行命令的使用者。

        * **命令** - 要執行的 Sqoop 命令。

        * **statusdir** - 要寫入此工作狀態的目錄。

    此命令應該會傳回可用來檢查工作狀態的工作識別碼。

        {"id":"job_1415651640909_0026"}

1. 若要檢查工作的狀態，請使用下列命令。將 **JOBID** 取代為上一個步驟中所傳回的值。例如，如果傳回值為 `{"id":"job_1415651640909_0026"}`，則 **JOBID** 會是 `job_1415651640909_0026`。
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    如果工作已完成，則狀態會是 [**成功**]。
   
   > [!NOTE]
   > 此 Curl 要求會傳回含有工作資訊的 JavaScript Object Notation (JSON) 文件；jq 可用來僅擷取狀態值。
   > 
   > 
2. 工作狀態變更為 [**成功**] 之後，即可從 Azure Blob 儲存體擷取工作結果。隨查詢一起傳送的 `statusdir` 參數包含輸出檔案的位置；在此案例中為 **wasbs:///example/curl**。此位址會將工作輸出儲存至 HDInsight 叢集所使用之預設儲存體容器的 **example/curl** 目錄中。
   
    您可以使用 [Azure CLI](../xplat-cli-install.md) 列出並下載這些檔案：例如，若要列出 **example/curl** 中的檔案，請使用下列命令：
   
        azure storage blob list <container-name> example/curl
   
    若要下載檔案，請使用下列命令：
   
        azure storage blob download <container-name> <blob-name> <destination-file>
   
   > [!NOTE]
   > 您必須使用 `-a` 和 `-k` 參數指定包含 Blob 的儲存體帳戶名稱，或是設定 **AZURE\_STORAGE\_ACCOUNT** 和 **AZURE\_STORAGE\_ACCESS\_KEY** 環境變數。如需詳細資訊，請參閱 <a href="hdinsight-upload-data.md" target="\_blank"。
   > 
   > 

## 限制
* 大量匯出 - 使用 Linux 型 HDInsight，用來將資料匯出至 Microsoft SQL Server 或 Azure SQL Database 的 Sqoop 連接器目前不支援大量插入。
* 批次處理 - 使用 Linux 型 HDInsight，執行插入時若使用 `-batch` 參數，Sqoop 將會執行多個插入，而不是批次處理插入作業。

## 摘要
如這份文件所示，您可以使用原始 HTTP 要求來執行、監視和檢視 HDInsight 叢集上的 Sqoop 作業結果。

如需本文中使用的 REST 介面的詳細資訊，請參閱 <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API 指南</a>。

## 後續步驟
Hive 與 HDInsight 搭配使用的一般資訊：

* [在 HDInsight 上將 Sqoop 與 Hadoop 搭配使用](hdinsight-use-sqoop.md)

如需您可以在 HDInsight 上使用 Hadoop 之其他方式的詳細資訊：

* [搭配使用 Hive 與 HDInsight 上的 Hadoop](hdinsight-use-hive.md)
* [搭配 HDInsight 上的 Hadoop 使用 Pig](hdinsight-use-pig.md)
* [搭配使用 MapReduce 與 HDInsight 上的 Hadoop](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

<!---HONumber=AcomDC_0914_2016-->