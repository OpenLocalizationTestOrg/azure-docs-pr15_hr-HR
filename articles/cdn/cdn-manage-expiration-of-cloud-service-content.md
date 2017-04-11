<properties
 pageTitle="Kako upravljati isteka Azure web-aplikacije/oblaka servisi, ASP.NET i IIS sadržaja u Azure CDN | Microsoft Azure"
 description="U članku se opisuje kako upravljati isteka oblaka sadržaj servisa Azure CDN"
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="09/19/2016"
 ms.author="casoper"/>

# <a name="how-to-manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Kako upravljati isteka Azure web-aplikacije/oblaka servisi, ASP.NET ili IIS sadržaja u Azure CDN

> [AZURE.SELECTOR]
- [Azure web-aplikacije/oblaka servisi, ASP.NET ili IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Servis blobova platforme Azure za pohranu](cdn-manage-expiration-of-blob-content.md)

Datoteka s bilo kojeg web-poslužitelj javno dostupnu polazište može se spremiti u Azure CDN dok minutama njegov vrijeme važenja (TTL).  Za TTL određen [ *Predmemorijom* zaglavlja](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) u HTTP odgovor polazište poslužitelja.  U ovom se članku opisuje kako postaviti `Cache-Control` zaglavlja za Azure web-aplikacije, Azure servise u Oblaku, ASP.NET aplikacija i servisa Internet Information Services web-mjesta, koja su konfigurirana na sličan način.

>[AZURE.TIP] Možete odabrati da biste postavili bez TTL na datoteci.  U ovom slučaju Azure CDN automatski primjenjuje zadani TTL od sedam dana.
>
>Dodatne informacije o funkcioniranje Azure CDN brzinu pristup datotekama i ostalim resursima potražite u članku [Pregled CDN Azure](./cdn-overview.md).

## <a name="setting-cache-control-headers-in-configuration"></a>Postavljanje predmemorijom zaglavlja u konfiguraciji

Za statički sadržaj, kao što su slike i listove stila, možete odrediti učestalost ažuriranja izmjenom **applicationHost.config** ili **web.config** datoteke za web-aplikaciju.  Postavit će **system.webServer\staticContent\clientCache** element u konfiguracijskoj datoteci na `Cache-Control` zaglavlja za sadržaj. Za **web.config**konfiguracijske postavke utjecat će na sve u mapi i sve podmape, osim ako nadjačati na razini podmape.  Ako, na primjer, postaviti zadano vrijeme važenja u korijenu da bi se u statični sadržaj predmemorirane za tri dana, ali ste podmapu koja ima više varijabli sadržaj postavku predmemorije od 6 sati.  **ApplicationHost.config**sve aplikacije na web-mjestu utjecat će, ali možete nadjačati u **web.config** datoteke u aplikacijama.

Sljedeći XML prikazuje i primjer postavka **clientCache** da biste odredili maksimalno 3 dana:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Određivanje **UseMaxAge** dodaje u `Cache-Control: max-age=<nnn>` zaglavlja u odgovoru na temelju vrijednosti navedene u atribut **CacheControlMaxAge** . Oblikovanje na vremenski raspon je za atribut **cacheControlMaxAge** <days>. <hours>:<min>:<sec>. Dodatne informacije o čvor **clientCache** potražite u članku [klijent predmemorije <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Postavljanje predmemorijom zaglavlja u kodu

Za aplikacije ASP.NET, možete postaviti CDN programski predmemoriranje ponašanje tako da svojstvo **HttpResponse.Cache** . Dodatne informacije o svojstvo **HttpResponse.Cache** potražite u članku [HttpResponse.Cache svojstvo](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) i [HttpCachePolicy predmete](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Želite li programatski predmemorije aplikacije sadržaja u ASP.NET, provjerite je li sadržaj označen kao predmemorirati postavljanjem HttpCacheability *javno*. Osim toga, provjerite je li alat za provjeru valjanosti predmemorije postavljen. U alat za provjeru valjanosti predmemorije može biti Zadnja izmjena vremenska oznaka postavljena tako da nazovete SetLastModified ili jedna vrijednost postaviti tako da nazovete SetETag. Po želji možete navesti vrijeme isteka predmemoriju tako da nazovete SetExpires ili možete oslanjate heuristike predmemorije zadani opisane ranije u ovom dokumentu.  

Na primjer, predmemoriju sadržaja na jedan sat, dodajte sljedeće:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Daljnji koraci

- [Pročitajte detalje o elementu **clientCache**](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
- [Pročitajte dokumentaciju za svojstvo **HttpResponse.Cache**](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
- [Čitanje potražite u dokumentaciji **HttpCachePolicy predmete**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  
