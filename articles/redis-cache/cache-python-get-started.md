<properties
    pageTitle="Kako koristiti predmemorije Redis Azure s Python | Microsoft Azure"
    description="Početak rada s Redis Azure predmemoriju pomoću Python"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-python"></a>Kako koristiti predmemorije Redis Azure s Python

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [PLATFORME ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

U ovoj se temi objašnjava za početak rada s predmemorije Redis Azure pomoću Python.


## <a name="prerequisites"></a>Preduvjeti

Instalirajte [redis Kopiraj](https://github.com/andymccurdy/redis-py).


## <a name="create-a-redis-cache-on-azure"></a>Stvaranje Redis predmemorije na Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Dohvaćanje tipki glavno računalo ime i access

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Omogućivanje krajnju točku koja nije SSL

Neki klijenti Redis ne podržava SSL i prema zadanim se postavkama [koje nisu SSL priključak nije omogućen za nove instance predmemorije Redis Azure](cache-configure.md#access-ports). Prilikom pisanja ovog [redis Kopiraj](https://github.com/andymccurdy/redis-py) klijent ne podržava SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Dodajte nešto predmemorije i preuzeti


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Zamjena `<name>` uz naziv predmemorije i `key` pomoću svojeg ključa programa access.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
