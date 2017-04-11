<properties
   pageTitle="Kako koristiti predmemorije Redis Azure s Java | Microsoft Azure"
    description="Početak rada s Redis Azure predmemoriju pomoću jezika Java"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-java"></a>Kako koristiti predmemorije Redis Azure s Java

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [PLATFORME ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis predmemorije omogućuje pristup s namjenski Redis predmemorije, upravlja Microsoft. Predmemoriju se može pristupiti iz bilo koju aplikaciju unutar Microsoft Azure.

U ovoj se temi objašnjava za početak rada s predmemorije Redis Azure pomoću Java.

## <a name="prerequisites"></a>Preduvjeti

[Jedis](https://github.com/xetorthio/jedis) - Java klijent za Redis

Pomoću ovog praktičnog vodiča koristi Jedis, ali možete koristiti bilo koji klijent Java navedeni na [http://redis.io/clients](http://redis.io/clients).

## <a name="create-a-redis-cache-on-azure"></a>Stvaranje Redis predmemorije na Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Dohvaćanje tipki glavno računalo ime i access

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Omogućivanje krajnju točku koja nije SSL

Neki klijenti Redis ne podržava SSL i prema zadanim se postavkama [koje nisu SSL priključak nije omogućen za nove instance predmemorije Redis Azure](cache-configure.md#access-ports). Prilikom pisanja ovog [Jedis](https://github.com/xetorthio/jedis) klijent ne podržava SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## <a name="add-something-to-the-cache-and-retrieve-it"></a>Dodajte nešto predmemorije i preuzeti

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


## <a name="next-steps"></a>Daljnji koraci

- [Omogućivanje Dijagnostika predmemoriju](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) tako da možete [monitor](https://msdn.microsoft.com/library/azure/dn763945.aspx) stanja predmemoriju.
- Službeni [Redis dokumentaciju](http://redis.io/documentation)za čitanje.

