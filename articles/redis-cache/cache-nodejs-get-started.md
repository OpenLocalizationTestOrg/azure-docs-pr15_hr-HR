<properties
    pageTitle="Kako koristiti predmemorije Redis Azure s Node.js | Microsoft Azure"
    description="Početak rada s Redis Azure predmemoriju pomoću Node.js i node_redis."
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Kako koristiti predmemorije Redis Azure s Node.js

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [PLATFORME ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis predmemorije omogućuje vam pristup sigurne, namjenski Redis predmemoriju, upravlja Microsoft. Predmemoriju se može pristupiti iz bilo koju aplikaciju unutar Microsoft Azure.

U ovoj se temi objašnjava za početak rada s predmemorije Redis Azure pomoću Node.js. Drugi primjer primjene predmemorije Redis Azure pomoću Node.js, potražite u članku [Stvaranje aplikacija za razgovor za Node.js s Socket.IO na web-mjestu sustava Azure](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## <a name="prerequisites"></a>Preduvjeti

Instalirajte [node_redis](https://github.com/mranney/node_redis):

    npm install redis

Pomoću ovog praktičnog vodiča koristi [node_redis](https://github.com/mranney/node_redis). Primjeri korištenja drugih Node.js klijenata, potražite u članku pojedinačne dokumentaciju za klijente Node.js navedeni na [Node.js Redis klijente](http://redis.io/clients#nodejs).

## <a name="create-a-redis-cache-on-azure"></a>Stvaranje Redis predmemorije na Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Dohvaćanje tipki glavno računalo ime i access

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Povezivanje s predmemoriju sigurno putem SSL-a

Najnovija izdanja sustava [node_redis](https://github.com/mranney/node_redis) podržavaju o povezivanju sa servisom Azure Redis predmemorije putem SSL-a. Sljedeći primjer pokazuje kako se povezati sa predmemorije Redis Azure pomoću SSL krajnjoj točci 6380. Zamjena `<name>` pod nazivom predmemorije i `<key>` pomoću ključa primarni ili sekundarni kao opisane u prethodnom odjeljku [dohvatiti tipki glavno računalo naziv i pristup](#retrieve-the-host-name-and-access-keys) .

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Dodajte nešto predmemorije i preuzeti

Sljedeći primjer pokazuje kako povezati instancu Azure Redis predmemorije i pohranu i dohvaćanje stavku iz predmemorije. Još primjera korištenja Redis [node_redis](https://github.com/mranney/node_redis) klijenta potražite u članku [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Izlaz:

    OK
    value


## <a name="next-steps"></a>Daljnji koraci

- [Omogućivanje Dijagnostika predmemoriju](cache-how-to-monitor.md#enable-cache-diagnostics) tako da možete [monitor](cache-how-to-monitor.md) stanju predmemoriju.
- Službeni [Redis dokumentaciju](http://redis.io/documentation)za čitanje.



