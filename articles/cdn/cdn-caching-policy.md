<properties
    pageTitle="CDN predmemoriranje pravila Media Services proširenja"
    description="Ova tema sadrži pregled ustvari CDN predmemoriranje pravila Media Services nastavak."
    services="media-services,cdn"
    documentationCenter=".NET"
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>
 
#<a name="cdn-caching-policy-in-media-services-extension"></a>CDN predmemoriranje pravila Media Services proširenja

Azure Media Services nudi HTTP na temelju prilagodljivu Streaming i progresivno preuzimanje. HTTP temelji strujanje je vrlo skalabilni s pogodnostima predmemoriranje proxy i slojeve CDN kao i predmemoriranja na strani klijenta. Strujanje krajnje točke nudi Općenite mogućnosti strujanje i konfiguracija za HTTP predmemorije zaglavlja. Strujanje krajnje točke postavlja predmemorijom HTTP: max dobi i istječe zaglavlja. Dodatne informacije o HTTP predmemorije zaglavlja možete doći s [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

##<a name="default-caching-headers"></a>Zadani Međuspremanje zaglavlja

Prema zadanim postavkama strujanje krajnje točke primijeniti 3 dana predmemorije zaglavlja na zahtjev strujanje (stvarne medijske fragmentirane/blokova) ili manifest(playlist). Uživo strujanje strujanje krajnje točke Primjena zaglavlja predmemorije 3 dana za podatke (stvarne medijske fragmentirane/blokova) i dvije sekunde predmemoriju zaglavlja manifest(playlist) zahtjeva za. Kada program uživo Pretvori da biste (uživo arhiva) na zahtjev na zahtjev strujanje predmemorije zaglavlja Primijeni.

##<a name="azure-cdn-integration"></a>Azure CDN Integracija

Azure Media Services nudi [integrirani CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) za strujanje krajnje točke. Predmemorijom zaglavlja se odnosi na isti način kao strujanje krajnje točke za CDN omogućeni strujanje krajnje točke. Azure CDN koristi strujanje krajnjoj točki konfigurirana predmemorije vrijednosti da biste definirali vijek trajanja interno predmemorirani objekata, a i koristi tu vrijednost da biste postavili isporuku predmemorije zaglavlja. Kada pomoću CDN omogućen strujanje krajnje točke ne preporučuje se da biste postavili small predmemorije vrijednosti. Postavljanje small vrijednosti Smanji performanse i smanjivanje prednost CDN. Nije dopušteno postavljanje zaglavlja predmemorije manje od 600 sekundi CDN omogućeno strujanje krajnje točke.

>[AZURE.IMPORTANT] Azure Media Services Integracija s Azure CDN je primijenjeno na **CDN Azure s Verizon**.  Ako želite koristiti **CDN Azure s Akamai** servisa Azure Media Services, morate [ručno konfiguriranje krajnju točku](cdn-create-new-endpoint.md).  Dodatne informacije o značajkama Azure CDN potražite u članku [Pregled CDN](cdn-overview.md).

##<a name="configuring-cache-headers-with-azure-media-services"></a>Konfiguriranje predmemorije zaglavlja pomoću servisa Azure Media Services

Portal za upravljanje Azure ili Azure Media Services API-ji možete koristiti da biste konfigurirali vrijednosti predmemorije zaglavlja.

1. Da biste konfigurirali predmemorije zaglavlja značajkom upravljanja portal pogledajte odjeljak [kako upravljati strujanje krajnje točke](../media-services/media-services-portal-manage-streaming-endpoints.md) konfiguriranje krajnja točka za strujanje.
2. Azure Media Services REST API-JA, [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK [StreamingEndpointCacheControl svojstva](http://go.microsoft.com/fwlink/?LinkId=615302).

##<a name="cache-configuration-precedence-order"></a>Redoslijed prednosti Konfiguracija predmemorije

1. Azure Media Services konfigurirana predmemorije vrijednost nadjačava zadane vrijednosti.
2. Ako nema ručna konfiguracija, primjenjuje se zadane vrijednosti.
3. Po zadanom dvije sekunde predmemorije zaglavlja odnosi se na live strujanje manifest(playlist) bez obzira na to konfiguracija Azure Media ili Azure prostora za pohranu i nadjačavanje te vrijednosti nije dostupna.
