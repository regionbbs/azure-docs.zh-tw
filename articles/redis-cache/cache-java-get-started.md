---
title: "如何搭配使用 Azure Redis 快取與 Java | Microsoft Docs"
description: "開始搭配使用 Azure Redis 快取與 Java"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 29275a5e-2e39-4ef2-804f-7ecc5161eab9
ms.service: cache
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 08/24/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 907f75dc02bff7e25712a564410c1974e22f0d99


---
# <a name="how-to-use-azure-redis-cache-with-java"></a>如何搭配使用 Azure Redis 快取與 Java
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis 快取可讓您存取 Microsoft 所管理的專用 Redis 快取。 從 Microsoft Azure 內的任何應用程式都可以存取您的快取。

本主題說明如何搭配使用 Azure Redis 快取與 Java。

## <a name="prerequisites"></a>必要條件
[Jedis](https://github.com/xetorthio/jedis) - Redis 的 Java 用戶端

本教學課程使用 Jedis，但是您可以使用列在 [http://redis.io/clients](http://redis.io/clients)的任何 Java 用戶端。

## <a name="create-a-redis-cache-on-azure"></a>在 Azure 上建立 Redis 快取
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>擷取主機名稱和存取金鑰
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="enable-the-nonssl-endpoint"></a>啟用非 SSL 端點
有些 Redis 用戶端不支援 SSL，且預設會 [停用新的 Azure Redis 快取執行個體的非 SSL 連接埠](cache-configure.md#access-ports)。 在本文撰寫當下， [Jedis](https://github.com/xetorthio/jedis) 用戶端不支援 SSL。 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-to-the-cache-and-retrieve-it"></a>在快取中加入項目並擷取該項目
    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
         jedis.set("foo", "bar");
         String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>後續步驟
* [啟用快取診斷](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics)，以[監視](https://msdn.microsoft.com/library/azure/dn763945.aspx)您快取的健全狀況。
* 閱讀官方 [Redis 文件](http://redis.io/documentation)。




<!--HONumber=Nov16_HO2-->


