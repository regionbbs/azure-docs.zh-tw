---
title: "從 Linux、Unix 或 OS X 搭配使用 SSH 金鑰與 Linux 架構的 Hadoop | Microsoft Docs"
description: " 您可以使用安全殼層 (SSH) 存取 Linux 架構的 HDInsight。 本文件提供從 Linux、Unix 或 OS X 用戶端搭配使用 SSH 與 HDInsight 的資訊。"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/13/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 476d9ce8b64f3442031310bd9170c682a9940b2b


---
# <a name="use-ssh-with-linuxbased-hadoop-on-hdinsight-from-linux-unix-or-os-x"></a>從 Linux、Unix 或 OS X 在 HDInsight 上搭配使用 SSH 與以 Linux 為基礎的 Hadoop
> [!div class="op_single_selector"]
> * [Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
> * [Linux、Unix、OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
> 
> 

[安全殼層 (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) 可讓您使用命令列介面在以 Linux 為基礎的 HDInsight 叢集上遠端執行作業。 本文件提供從 Linux、Unix 或 OS X 用戶端搭配使用 SSH 與 HDInsight 的資訊。

> [!NOTE]
> 本文中的步驟假設您是使用 Linux、Unix 或 OS X 用戶端。 如果您安裝的套件可提供 `ssh` 和 `ssh-keygen`，例如 [Windows 上 Ubuntu 上的Bash](https://msdn.microsoft.com/commandline/wsl/about)，則可能會在以 Windows 為基礎的用戶端上執行這些步驟。
> 
> 如果您未在以 Windows 為基礎的用戶端上安裝 SSH，請使用 [從 Windows 搭配使用 SSH 與以 Linux 為基礎的 HDInsight (Hadoop)](hdinsight-hadoop-linux-use-ssh-windows.md) 中的步驟，以取得安裝和使用 PuTTY 的相關資訊。
> 
> 

## <a name="prerequisites"></a>必要條件
* 適用於 Linux、Unix 和 OS X 用戶端的 **ssh-keygen** 和 **ssh**。 作業系統通常會隨附提供此公用程式，或是可透過封裝管理系統來取得。
* 支援 HTML5 的新式網頁瀏覽器。

或

* [Azure CLI](../xplat-cli-install.md)。
  
    [!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

## <a name="what-is-ssh"></a>什麼是 SSH？
SSH 是用來登入遠端伺服器並在其中遠端執行命令的公用程式。 SSH 會用以 Linux 為基礎的 HDInsight 來與叢集前端節點建立加密的連線，並提供命令列供您輸入命令。 然後直接在該伺服器上執行命令。

### <a name="ssh-user-name"></a>SSH 使用者名稱
SSH 使用者名稱為您用來向 HDInsight 叢集驗證的名稱。 當您在叢集建立期間指定 SSH 使用者名稱時，會在叢集中所有節點上建立此使用者。 建立叢集之後，您可以使用此使用者名稱連線至 HDInsight 叢集前端節點。 然後您可以從前端節點連線至個別的背景工作節點。

### <a name="ssh-password-or-public-key"></a>SSH 密碼或公開金鑰
SSH 使用者可以使用密碼或公開金鑰來驗證。 密碼是您自己設定的字串，公開金鑰則是為唯一識別您而產生之密碼編譯金鑰組的一部份。

金鑰會比密碼更安全，但是需要額外步驟才能產生金鑰，且您必須將含有金鑰的檔案保存在安全位置。 如果任何人取得金鑰檔案存取權，他們就可以取得您帳戶的存取權。 或者，如果您遺失金鑰檔案，您將無法登入帳戶。

金鑰組包含公開金鑰 (會傳送至 HDInsight 伺服器) 和私密金鑰 (會保留在您的用戶端機器中)。當您使用 SSH 連線至 HDInsight 伺服器時，SSH 用戶端將會使用您電腦上的私密金鑰向伺服器驗證。

## <a name="create-an-ssh-key"></a>建立 SSH 金鑰
如果您計劃搭配使用 SSH 金鑰與叢集，請使用下列資訊。 如果您計畫使用密碼，可以略過此小節。

1. 開啟終端機工作階段，並使用下列命令來查看是否有任何現有 SSH 金鑰：
   
        ls -al ~/.ssh
   
    在目錄清單中尋找下列檔案。 以下是公用 SSH 金鑰的常見名稱。
   
   * id\_dsa.pub
   * id\_ecdsa.pub
   * id\_ed25519.pub
   * id\_rsa.pub
2. 如果不想使用現有檔案或沒有現有 SSH 金鑰，請用下列命令產生新的檔案：
   
        ssh-keygen -t rsa
   
    系統將會提示您輸入下列資訊：
   
   * 檔案位置 - 預設位置為 ~/.ssh/id\_rsa。
   * 複雜密碼 - 系統會提示您再次輸入此密碼。
     
     > [!NOTE]
     > 強烈建議您對此金鑰使用安全的複雜密碼。 不過如果您忘記此複雜密碼，將無法加以復原。
     > 
     > 
     
     命令完成後，您會擁有兩個新檔案，即私密金鑰 (例如 **id\_rsa**) 和公開金鑰 (例如 **id\_rsa.pub**)。

## <a name="create-a-linuxbased-hdinsight-cluster"></a>建立以 Linux 為基礎的 HDInsight 叢集
在建立以 Linux 為基礎的 HDInsight 叢集時，您必須提供先前建立的公開金鑰。 從 Linux、Unix 或 OS X 用戶端有兩種方式可供建立 HDInsight 叢集：

* **Azure 入口網站** - 使用網頁型入口網站來建立叢集。
* **適用於 Mac、Linux 和 Windows 的 Azure CLI** - 使用命令列命令建立叢集。

這兩種方法都需要密碼或公開金鑰。 如需建立以 Linux 為基礎的 HDInsight 叢集的完整資訊，請參閱 [佈建以 Linux 為基礎的 HDInsight 叢集](hdinsight-hadoop-provision-linux-clusters.md)。

### <a name="azure-portal"></a>Azure 入口網站
使用 [Azure 入口網站][preview-portal]來建立以 Linux 為基礎的 HDInsight 叢集時，您必須輸入 [SSH 使用者名稱]，然後選取以輸入 [密碼] 或 [SSH 公開金鑰]。

如果您選取 [SSH 公開金鑰]，則可以將公開金鑰 (位於副檔名為 **.pub** 的檔案中) 貼入 [SSH 公開金鑰] 欄位，或選取 [選取檔案] 以瀏覽和選取公開金鑰檔案。

![要求公開金鑰的表單映像](./media/hdinsight-hadoop-linux-use-ssh-unix/ssh-key.png)

> [!NOTE]
> 金鑰檔就只是文字檔案。 其內容類似下面這樣：
> 
> ```
> ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCelfkjrpYHYiks4TM+r1LVsTYQ4jAXXGeOAF9Vv/KGz90pgMk3VRJk4PEUSELfXKxP3NtsVwLVPN1l09utI/tKHQ6WL3qy89WVVVLiwzL7tfJ2B08Gmcw8mC/YoieT/YG+4I4oAgPEmim+6/F9S0lU2I2CuFBX9JzauX8n1Y9kWzTARST+ERx2hysyA5ObLv97Xe4C2CQvGE01LGAXkw2ffP9vI+emUM+VeYrf0q3w/b1o/COKbFVZ2IpEcJ8G2SLlNsHWXofWhOKQRi64TMxT7LLoohD61q2aWNKdaE4oQdiuo8TGnt4zWLEPjzjIYIEIZGk00HiQD+KCB5pxoVtp user@system
> ```
> 
> 

這會使用您提供的密碼或公用金鑰建立指定使用者的登入資訊。

### <a name="azure-commandline-interface-for-mac-linux-and-windows"></a>適用於 Mac、Linux 和 Windows 的 Azure 命令列介面
您可以使用[適用於 Mac、Linux 和 Windows 的 Azure CLI](../xplat-cli-install.md)，使用 `azure hdinsight cluster create` 來建立新叢集。

如需使用這個命令的詳細資訊，請參閱 [使用自訂選項在 HDInsight 中佈建 Hadoop Linux 叢集](hdinsight-hadoop-provision-linux-clusters.md)。

## <a name="connect-to-a-linuxbased-hdinsight-cluster"></a>連線至以 Linux 為基礎的 HDInsight 叢集
在終端機工作階段使用 SSH 命令，提供位址和使用者名稱以連線到叢集前端節點：

* **SSH 位址** - 有兩個使用 SSH 的位址可用來連接到叢集：
  
  * **連接到前端節點**：叢集名稱加上 **-ssh.azurehdinsight.net**。 例如， **mycluster-ssh.azurehdinsight.net**。
  * **連接到邊緣節點**：如果您的叢集是 HDInsight 上的 R Server，此叢集也會包含可使用 **RServer.CLUSTERNAME.ssh.azurehdinsight.net** (其中 **CLUSTERNAME** 是叢集名稱) 存取的邊緣節點。
* **使用者名稱** - 建立叢集時所提供的 SSH 使用者名稱。

下列範例以使用者身分 **me** 連接到 **mycluster** 的主要前端節點：

    ssh me@mycluster-ssh.azurehdinsight.net

如果您對使用者帳戶使用密碼，系統會提示您輸入密碼。

如果您使用以複雜密碼保護的 SSH 金鑰，系統會提示您輸入複雜密碼。 否則，SSH 會嘗試使用您的用戶端上的其中一個本機私密金鑰，進行自動驗證。

> [!NOTE]
> 如果 SSH 沒有以正確私密金鑰自動進行驗證，請使用 **-i** 參數並指定私密金鑰的路徑。 下列範例會從 `~/.ssh/id_rsa`載入私密金鑰：
> 
> `ssh -i ~/.ssh/id_rsa me@mycluster-ssh.azurehdinsight.net`
> 
> 

如果您使用前端節點的位址進行連接，則 SSH 預設為連接埠 22，這將會連接到 HDInsight 叢集上的主要前端節點。 如果您使用連接埠 23，您將會連接到次要前端節點。 如需前端節點的詳細資訊，請參閱 [HDInsight 上 Hadoop 叢集的可用性和可靠性](hdinsight-high-availability-linux.md)。

### <a name="connect-to-worker-nodes"></a>連接至背景工作節點
背景工作節點無法直接從 Azure 資料中心外部存取，但是可以透過 SSH 從叢集前端節點存取。

如果您使用 SSH 金鑰驗證您的使用者帳戶，您必須在您的用戶端上完成下列步驟：

1. 使用文字編輯器開啟 `~/.ssh/config`。 如果此檔案不存在，您可以藉由在終端機中輸入 `touch ~/.ssh/config` 加以建立。
2. 將下列項目新增至檔案。 將 *CLUSTERNAME* 取代為 HDInsight 叢集的名稱。
   
        Host CLUSTERNAME-ssh.azurehdinsight.net
          ForwardAgent yes
   
    這樣會為您的 HDInsight 叢集設定 SSH 代理程式轉送。
3. 從終端機使用下列命令，測試 SSH 代理程式轉送：
   
        echo "$SSH_AUTH_SOCK"
   
    應該會傳回類似以下的資訊：
   
        /tmp/ssh-rfSUL1ldCldQ/agent.1792
   
    如果未傳回任何項目，則表示 **ssh-agent** 未執行。 請參閱您的作業系統文件以取得安裝和設定 **ssh-agent**的特定步驟，或參閱 [透過 ssh 使用 ssh-agent](http://mah.everybody.org/docs/ssh)。
4. 一旦確認 **ssh-agent** 正在執行，請使用下列項目將您的 SSH 私密金鑰新增至代理程式：
   
        ssh-add ~/.ssh/id_rsa
   
    如果您的私密金鑰儲存在不同的檔案，請將 `~/.ssh/id_rsa` 取代為檔案的路徑。

請使用下列步驟連接到您的叢集的背景工作節點。

> [!IMPORTANT]
> 如果您使用 SSH 金鑰驗證您的使用者帳戶，您必須完成上述步驟以驗證代理程式轉送正在運作。
> 
> 

1. 使用 SSH 連接到 HDInsight 叢集，如先前所述。
2. 連接之後，請使用下列項目以擷取您的叢集中的節點清單。 將 *ADMINPASSWORD* 取代為您的叢集系統管理員帳戶的密碼。 將 *CLUSTERNAME* 取代為您叢集的名稱。
   
        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts
   
    這樣會以 JSON 格式傳回叢集中節點的資訊，包括 `host_name`，其中包含每個節點的完整網域名稱 (FQDN)。 以下是 **curl** 命令傳回之 `host_name` 項目的範例：
   
        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"
3. 當您具有想要連接之背景工作節點的清單之後，請從 SSH 工作階段對伺服器使用下列命令，以開啟與背景工作節點的連線：
   
        ssh USERNAME@FQDN
   
    將 *USERNAME* 取代為您的 SSH 使用者名稱，將 *FQDN* 取代為背景工作節點的 FQDN。 例如， `workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`。
   
   > [!NOTE]
   > 如果您使用密碼以驗證您的 SSH 工作階段，則系統會提示您再次輸入密碼。 如果您使用 SSH 金鑰，連線應該沒有任何提示即會完成。
   > 
   > 
4. 建立工作階段之後，終端機提示會從 `username@hn#-clustername` 變更為 `username@wk#-clustername`，以指出您已連接至背景工作節點。 目前您執行的任何命令會在背景工作節點上執行。
5. 完成在背景工作節點上執行動作之後，請使用 `exit` 命令以關閉背景工作節點的工作階段。 這樣會帶您返回 `username@hn#-clustername` 提示字元。

## <a name="connect-to-a-domainjoined-hdinsight-cluster"></a>連線至已加入網域的 HDInsight 叢集
[已加入網域的 HDInsight](hdinsight-domain-joined-introduction.md) 會整合 Kerberos 與 HDInsight 中的 Hadoop。 因為 SSH 使用者不是 Active Direcotry 網域使用者，所以此使用者帳戶無法直接在已加入網域的叢集上從 SSH Shell 執行 Hadoop 命令。 您必須先執行 *kinit*。 

**使用 SSH 在已加入網域的 HDInsight 叢集上執行 Hive 查詢**

1. 使用 SSH 連線至已加入網域的 HDInsight 叢集。  如需指示，請參閱[連線至以 Linux 為基礎的 HDInsight 叢集](#connect-to-a-linux-based-hdinsight-cluster)。
2. 執行 kinit。 它會要求您提供網域使用者名稱和網域使用者密碼。 如需有關設定已加入網域 HDInsight 叢集之網域使用者的詳細資訊，請參閱[設定已加入網域的 HDInisight 叢集](hdinsight-domain-joined-configure.md)。
   
    ![HDInsight Hadoop 已加入網域的 kinit](./media/hdinsight-hadoop-linux-use-ssh-unix/hdinsight-domain-joined-hadoop-kinit.png)
3. 輸入下列內容以開啟 Hive 主控台：
   
        hive
   
    然後即可執行 Hive 命令。

## <a name="add-more-accounts"></a>新增更多帳戶
1. 為新的使用者帳戶產生新的公開金鑰和私密金鑰，如 [建立 SSH 金鑰](#create-an-ssh-key-optional) 章節所述。
   
   > [!NOTE]
   > 私密金鑰應該在使用者用來連接到叢集的用戶端上產生，或在建立後安全地傳輸到這個用戶端。
   > 
   > 
2. 在連往叢集的 SSH 工作階段中，使用下列命令加入新使用者：
   
        sudo adduser --disabled-password <username>
   
    這會建立新的使用者帳戶，但會停用密碼驗證。
3. 使用下列命令建立用來保存金鑰的目錄和檔案：
   
        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys
4. 在 nano 編輯器開啟時，複製並貼上新使用者帳戶的公開金鑰內容。 最後，使用 **Ctrl-X** 來儲存檔案並結束編輯器。
   
    ![具有範例金鑰之 nano 編輯器的映像](./media/hdinsight-hadoop-linux-use-ssh-unix/nano.png)
5. 使用下列命令將 .ssh 資料夾和內容的擁有權變更為新的使用者帳戶：
   
        sudo chown -hR <username>:<username> /home/<username>/.ssh
6. 您現在應該就能使用新的使用者帳戶和私密金鑰驗證伺服器。

## <a name="a-idtunnelassh-tunneling"></a><a id="tunnel"></a>SSH 通道
SSH 可用來建立通道以將本機要求 (例如 Web 要求) 傳送到 HDInsight 叢集。 要求便會路由至要求的資源，彷彿要求是在 HDInsight 叢集前端節點上產生。

> [!IMPORTANT]
> SSH 通道是存取某些 Hadoop 服務之 Web UI 的必要項目。 例如，[作業記錄] UI 或 [資源管理員] UI 都只能使用 SSH 通道存取。
> 
> 

如需建立及使用 SSH 通道的詳細資訊，請參閱 [使用 SSH 通道來存取 Ambari Web UI、ResourceManager、JobHistory、NameNode、Oozie 及其他 Web UI](hdinsight-linux-ambari-ssh-tunnel.md)。

## <a name="next-steps"></a>後續步驟
您已經了解如何使用 SSH 金鑰進行驗證，接著請了解如何在 HDInsight 上搭配使用 MapReduce 和 Hadoop。

* [搭配 HDInsight 使用 Hivet](hdinsight-use-hive.md)
* [搭配 HDInsight 使用 Pig](hdinsight-use-pig.md)
* [搭配 HDInsight 使用 MapReduce 工作](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/



<!--HONumber=Nov16_HO2-->


