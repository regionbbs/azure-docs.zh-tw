---
title: "將 RedHat Linux VM 加入 Azure Active Directory DS | Microsoft Docs"
description: "如何將現有 RedHat Enterprise Linux 7 VM 加入 Azure Active Directory 網域服務。"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/17/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 4e764e0d115c787f78593116f29c89e7c1e0e527
ms.openlocfilehash: 0c596b630c44ff68a01c7dbd3173c192fafac125


---

# <a name="join-a-redhat-linux-vm-to-an-azure-active-directory-domain-service"></a>將 RedHat Linux VM 加入 Azure Active Directory 網域服務

本文說明如何將 Red Hat Enterprise Linux (RHEL) 7 虛擬機器加入 Azure Active Directory Domain Services (AADDS) 管理的網域。  這些需求包括：

- [一個 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/)

- [SSH 公開金鑰和私密金鑰檔案](virtual-machines-linux-mac-create-ssh-keys.md)

- [Azure Active Directory Domain Services DC](../active-directory-domain-services/active-directory-ds-getting-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>快速命令

將任何範例換成您自己的設定。

### <a name="switch-the-azure-cli-to-classic-deployment-mode"></a>將 azure-cli 切換至傳統部署模式

```azurecli
azure config mode asm
```

### <a name="search-for-a-rhel-version-and-image"></a>搜尋 RHEL 版本和映像

```azurecli
azure vm image list | grep "Red Hat"
```

### <a name="create-a-redhat-linux-vm"></a>建立 Redhat Linux VM

```azurecli
azure vm create myVM \
-o a879bbefc56a43abb0ce65052aac09f3__RHEL_7_2_Standard_Azure_RHUI-20161026220742 \
-g ahmet \
-p P@ssw0rd! \
-z "Small" \
-e \
-l "West US"
```

### <a name="ssh-to-the-vm"></a>透過 SSH 連接到 VM

```bash
ssh -i ~/.ssh/id_rsa ahmet@
```

### <a name="update-yum-packages"></a>更新 YUM 套件

```bash
sudo yum update
```

### <a name="install-packages-needed"></a>安裝需要的套件

```bash
sudo yum -y install realmd sssd krb5-workstation krb5-libs
```

既然 Linux 虛擬機器上已安裝必要的封裝，下一個工作是將虛擬機器加入受管理的網域。

### <a name="discover-the-aad-domain-services-managed-domain"></a>探索 AAD Domain Services 管理的網域

```bash
sudo realm discover mydomain.com
```

### <a name="initialize-kerberos"></a>初始化 kerberos

請確定您是指定屬於 'AAD DC Administrators' 群組的使用者。 只有這些使用者可以將電腦加入受管理的網域。

```bash
kinit ahmet@mydomain.com
```

### <a name="join-the-machine-to-the-domain"></a>將電腦加入網域

```bash
sudo realm join --verbose mydomain.com -U 'ahmet@mydomain.com'
```

### <a name="verify-the-machine-is-joined-to-the-domain"></a>確認電腦已加入網域

```bash
ssh -l ahmet@mydomain.com mydomain.cloudapp.net
```

## <a name="next-steps"></a>後續步驟

* [適用於 Azure 中隨選 Red Hat Enterprise Linux VM 的 Red Hat Update Infrastructure (RHUI)](virtual-machines-linux-update-infrastructure-redhat?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [為 Azure Resource Manager 中的虛擬機器設定金鑰保存庫](virtual-machines-linux-key-vault-setup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [使用 Azure Resource Manager 範本和 Azure CLI 部署和管理虛擬機器](virtual-machines-linux-cli-deploy-templates?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)



<!--HONumber=Nov16_HO5-->


