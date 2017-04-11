<properties
    pageTitle="Pregled Azure CDN | Microsoft Azure"
    description="Saznajte što je Azure sadržaj isporuke mreže (CDN) i kako je koristiti za isporuku sadržaja velikom propusnošću po predmemoriranje blob-ova i statični sadržaj."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/30/2016"
    ms.author="casoper"/>

# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Pregled mreže za Azure isporuku sadržaja (CDN)

> [AZURE.NOTE] U ovom dokumentu opisuje što je Azure mreže sadržaja isporuke (CDN), kako funkcionira i značajke za svaki proizvod Azure CDN.  Ako želite preskočiti te podatke i Idite izravno na vodič o tome da biste stvorili krajnju točku CDN potražite u članku [Korištenje CDN Azure](cdn-create-new-endpoint.md).  Ako želite da biste vidjeli popis od trenutnog mjesta čvor CDN, potražite u članku [Azure CDN POP mjesta](cdn-pop-locations.md).

Azure sadržaj isporuke mreže (CDN) predmemorira statičke web-sadržaja na strateški postavljen lokacijama nudi Maksimalna propusnost za pružanje sadržaja korisnicima.  Na CDN nudi razvojnim inženjerima globalni rješenje za sadržaj velikom propusnošću po predmemoriranje sadržaj na fizičke čvorove širom svijeta. 

Prednosti korištenja CDN predmemoriju web-mjesta imovini obuhvaćaju sljedeće:

- Bolje performanse i korisničko sučelje za krajnje korisnike, osobito kada korištenje aplikacija u kojima su potrebne više spajanja da biste učitali sadržaja.
- LARGE skaliranje bolje ručicu za trenutačno opterećenjem visoka, poput na početku događaj za pokretanje proizvoda.
- Distribucija zahtjevima korisnika i posluživanje sadržaj s edge poslužitelji, manje promet se šalje izvor.


## <a name="how-it-works"></a>Kako funkcionira

![Pregled CDN](./media/cdn-overview/cdn-overview.png)

1. Korisnik (Alice) zatraži datoteke (naziva se i sredstvo) pomoću URL-a s nazivom domene posebno, kao što su `<endpointname>.azureedge.net`.  DNS preusmjerava zahtjev najbolje izvođenje mjesto točke-od-prisutnosti (POP).  To je obično POP koji je geografski najbliži korisnika.

2. Ako edge poslužitelji u na POP datoteka u predmemoriji za svoje, rubni poslužitelj zahtijeva datoteku iz izvor.  Izvor može biti web-aplikaciju programa Azure, Azure u Oblaku, račun za Azure prostora za pohranu ili javno dostupnu web-poslužitelj.

3. Izvor vraća datoteku rubni poslužitelj, uključujući neobavezno HTTP zaglavlja s opisom datoteke vrijeme važenja (TTL).

4. Rubni poslužitelj predmemorira datoteku i vraća datoteke u izvornom podnositelja zahtjeva (Alice).  Datoteka ostaje predmemorirane na rubnom poslužitelju dok u TTL istekne.  Ako izvor niste naveli TTL, zadani TTL je sedam dana.

5. Dodatni korisnici možda zatražiti istu datoteku pomoću tog iste URL-a, a možda također biti preusmjereni na toj istoj POP.

6. Ako nije istekao TTL datoteke, rubni poslužitelj vraća datoteke iz predmemorije.  Rezultira brže odzivom korisničkog sučelja.


## <a name="azure-cdn-features"></a>Azure CDN značajke

Postoje tri Azure CDN proizvodi: **Azure CDN standardne iz Akamai**, **Azure CDN standardne iz Verizon**i **Azure CDN Premium s Verizon**.  U sljedećoj su tablici navedene značajke koje su dostupne uz svaki proizvod.

|       | Standardni Akamai | Standardni Verizon | Premium Verizon |
|-------|-----------------|------------------|-----------------|
| Jednostavna Integracija s Azure servise kao što je [prostor za pohranu](cdn-create-a-storage-account-with-cdn.md), [Servise u Oblaku](cdn-cloud-service-with-cdn.md), [Web-aplikacije](../app-service-web/cdn-websites-with-cdn.md)i [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;**|
| Upravljanje putem [REST API-JA](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](./cdn-app-dev-net.md), [Node.js](./cdn-app-dev-node.md)ili [PowerShell](./cdn-manage-powershell.md). | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| HTTPS za podršku | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Za ujednačavanje opterećenja | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Zaštita [DDOS](https://www.us-cert.gov/ncas/tips/ST04-015) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Lišće stogu IPv4 i IPv6 | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Podrška za naziv prilagođene domene](cdn-map-content-to-custom-domain.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Predmemoriranje niza upita](cdn-query-string.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Filtriranje zemlj.](cdn-restrict-access-by-country.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Brzo čišćenje](cdn-purge-endpoint.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Prije učitavanja resursa](cdn-preload-endpoint.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Temeljni analytics](cdn-analyze-usage-patterns.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Podrška za HTTP/2](https://msdn.microsoft.com/library/mt762901.aspx) | **& #x 2713;**  |  |  |
| [Dodatna izvješća HTTP](cdn-advanced-http-reports.md) | | | **& #x 2713;** |
| [U stvarnom vremenu stat](cdn-real-time-stats.md) | | | **& #x 2713;** |
| [Upozorenja u stvarnom vremenu](cdn-real-time-alerts.md) | | | **& #x 2713;** |
| [Modul za isporuku sadržaja moguće prilagoditi, pravila](cdn-rules-engine.md) | | | **& #x 2713;** |
| Postavke predmemorije/zaglavlje (pomoću [pravila modul](cdn-rules-engine.md))  | | | **& #x 2713;** |
| URL preusmjeravanja/novog teksta (pomoću [pravila modul](cdn-rules-engine.md)) | | | **& #x 2713;** |
| Pravila za mobilni uređaj (pomoću [pravila modul](cdn-rules-engine.md))  | | | **& #x 2713;** |

>[AZURE.TIP] Je li značajka želite vidjeti u Azure CDN?  [Pošaljite nam povratne informacije](https://feedback.azure.com/forums/169397-cdn)! 

## <a name="next-steps"></a>Daljnji koraci

Početak rada s CDN, potražite u članku [Korištenje CDN Azure](./cdn-create-new-endpoint.md).

Ako ste postojeći CDN klijent, sada mogu upravljati CDN točke putem [portala sustava Microsoft Azure](https://portal.azure.com) ili sa [servisom PowerShell](cdn-manage-powershell.md).

Da biste vidjeli CDN u praksi, pogledajte u [video naš sesije Sastavi 2016](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Saznajte kako automatizirati CDN Azure s [.NET](./cdn-app-dev-net.md) ili [Node.js](./cdn-app-dev-node.md).

Informacije o cijenama, potražite u članku [CDN cijene](https://azure.microsoft.com/pricing/details/cdn/).
