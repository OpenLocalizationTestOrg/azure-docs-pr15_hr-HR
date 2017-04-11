<properties 
    pageTitle="Upravljanje strujanje krajnje točke s portala za Azure | Microsoft Azure" 
    description="U ovoj se temi objašnjava upravljanje strujanje krajnje točke s portala za Azure." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    writer="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016"
    ms.author="juliako"/>


#<a name="manage-streaming-endpoints-with-the-azure-portal"></a>Upravljanje strujanje krajnje točke s portala za Azure

## <a name="overview"></a>Pregled

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). 

U programu Microsoft Azure Media Services **Strujanje krajnjoj točki** predstavlja servis za strujanje koja možete ispuniti sadržajem izravno u aplikaciju za reprodukciju klijenta ili da biste na sadržaj isporuke mreže (CDN) za daljnje distribuciju. Media Services i omogućuje objedinjenog integraciju Azure CDN. Izlazni toka iz servisa StreamingEndpoint može biti aktivno strujanje ili videozapis na zahtjev resursa na vašem računu Media Services.

Osim toga, možete kontrolirati kapacitet servis za strujanje krajnja točka za rukovanje sve veći propusnosti potrebama prilagodbom strujanje jedinice. Preporučuje se dodijeliti jednu ili više jedinica mjerilo za aplikacije u radnom okruženju. Strujanje jedinice vam ponuditi namjenski izlazne kapacitet koji je moguće kupiti u koracima od 200 MB/s i dodatne funkcije, što obuhvaća: [dinamički pakiranje](media-services-dynamic-packaging-overview.md), Integracija CDN i Napredna konfiguracija.

>[AZURE.NOTE]Samo se naplaćuju kada je na krajnjoj točki strujanje u izvodi stanje.

Ova tema sadrži pregled glavni radovi koje nudi strujanje krajnje točke. Teme i pokazuje kako koristiti Azure portal za upravljanje strujanje krajnje točke. Informacije o skaliranje strujanje krajnje točke, pogledajte [ovu](media-services-portal-scale-streaming-endpoints.md) temu.

## <a name="start-managing-streaming-endpoints"></a>Započnite upravljati strujanje krajnje točke

Da biste započeli upravljanje strujanje krajnje točke za vaš račun, učinite sljedeće:

1. [Portal za Azure](https://portal.azure.com/)odaberite svoj račun servisa Azure Media Services.
2. U prozoru **Postavke** odaberite **strujeće krajnje točke**.

    ![Strujanje krajnje točke](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

##<a name="adddelete-a-streaming-endpoint"></a>Dodavanje i brisanje strujanje krajnje točke

Za dodavanje i brisanje strujanje krajnjoj točki pomoću portala za Azure, učinite sljedeće:

1. Da biste dodali strujanje krajnju točku, kliknite **+ krajnjoj točki** pri vrhu stranice. 
2. Da biste izbrisali strujanje krajnje točke, pritisnite gumb **Izbriši** . 

    Nije moguće izbrisati zadane strujanje krajnjoj točki.
2. Kliknite gumb **Start** da biste pokrenuli strujanje krajnjoj točki.

    ![Strujanje krajnje točke](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)

Po zadanom može imati do dva strujanje krajnje točke. Ako vam je potrebna da biste zatražili informacije, potražite u članku [kvotama i ograničenja](media-services-quotas-and-limitations.md).
    
##<a id="configure_streaming_endpoints"></a>Konfiguriranje strujanje krajnje točke

Strujanje krajnje točke omogućuje vam da biste konfigurirali sljedeća svojstva kad imate barem 1 skaliranje jedinica: 

- Kontrola pristupa
- Kontrola predmemorije
- Unakrsni pravila za pristup web-mjesta

Detaljne informacije o tim svojstvima potražite u članku [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx).

Strujanje krajnjoj točki možete konfigurirati tako da učinite sljedeće:

1. Odaberite krajnju točku strujanje koju želite konfigurirati.
1. Kliknite **Postavke**.
  
Slijedi kratak opis polja.

![Strujanje krajnje točke](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)
  
1. Pravila Maksimalna predmemorije: služi za konfiguriranje predmemorije vijek imovine poslužena kroz ovaj strujanje krajnjoj točki. Ako nije postavljena vrijednost, koristit će se zadani. Zadane vrijednosti može se definirati i izravno u Azure prostora za pohranu. Ako je Azure CDN omogućen za strujanje krajnju točku, ne postavite vrijednost pravila predmemoriju za manje od 600 sekundi.  

2. Dopuštene IP adrese: koristi da biste odredili IP adrese koje želite omogućiti povezati objavljene strujanje krajnjoj točki. Ako nije naveden IP adrese, bilo koje IP adrese će se moći povezati. IP adrese možete navesti kao jedan IP adresa (na primjer, "10.0.0.1"), raspon IP pomoću IP adresa i masku CIDR (na primjer, "10.0.0.1/22") ili raspona IP pomoću IP adresa i istočkanom decimalni masku (na primjer, "10.0.0.1(255.255.255.0)').

3. Konfiguracija za provjeru autentičnosti sustava Akamai potpis zaglavlja: koristi se za određivanje konfiguraciji potpis zahtjev za provjeru autentičnosti zaglavlja s poslužitelja Akamai. Istek je u UTC-u.



##<a id="enable_cdn"></a>Omogući Integracija Azure CDN

Možete odrediti da biste omogućili integraciju sa servisom Azure CDN za strujanje krajnje (to je po zadanom onemogućene.)

Da biste postavili integraciju sa servisom Azure CDN na true:

- Strujanje krajnjoj točki mora imati najmanje jednu strujanje jedinicu. Ako kasnije želite postaviti skaliranje jedinice 0, najprije morate onemogućiti integraciju sa servisom CDN. Prema zadanim postavkama prilikom stvaranja novog strujanje krajnjoj točki strujanje jedinice automatski postavlja.

- Strujanje krajnje točke mora biti u prestao stanju. Kada je omogućen dobiti na CDN, možete pokrenuti strujanje krajnjoj točki. 

Može potrajati i do 90 min za integraciju sa servisom Azure CDN da biste dobili omogućena.  Potrebno do dva sata da se aktivni preko svih pojavljuje CDN promjene.

Integracija CDN omogućen u svim centrima Azure podataka: NAM Zapad, NAM Istok, Sjeverna Europa, Zapad Europa, Japan Zapad, Istok Japan, Južna istočnoazijski i istočnoazijski.

Kada je omogućen, onemogućeno dobiva konfiguracije **Kontrola pristupa** .

![Strujanje krajnje točke](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints5.png)

>[AZURE.IMPORTANT] Azure Media Services Integracija s Azure CDN je primijenjeno na **CDN Azure s Verizon**.  Ako želite koristiti **CDN Azure s Akamai** servisa Azure Media Services, morate [ručno konfiguriranje krajnju točku](../cdn/cdn-create-new-endpoint.md).  Dodatne informacije o značajkama Azure CDN potražite u članku [Pregled CDN](../cdn/cdn-overview.md).

###<a name="additional-considerations"></a>Dodatne napomene

- Kada CDN je omogućen za strujanje krajnju točku, klijenti ne možete zatražiti sadržaj izravno iz izvor. Ako vam je potrebna mogućnost da biste testirali sadržaja sa ili bez CDN, možete stvoriti drugi strujanje krajnju točku koja nije CDN omogućena.
- Strujanje krajnjoj točki naziv glavnog računala ostaje nakon omogućavanja CDN. Ne morate mijenjati media services tijeka rada kada je omogućen CDN. Ako, na primjer, ako strujanje krajnjoj točki naziv glavnog računala strasbourg.streaming.mediaservices.windows.net, nakon omogućavanja CDN, koristi se točno isti naziv glavnog računala.
- Za novi strujanje krajnje točke, možete omogućiti CDN jednostavno stvaranjem krajnju točku; za postojeće strujanje krajnje točke, morate najprije zaustaviti krajnju točku, a zatim omogućiti na CDN.
 

Dodatne informacije potražite u člancima, [Objavljivanje servisa Azure Media Services Integracija s Azure CDN (sadržaja mreže za isporuku)](http://azure.microsoft.com/blog/2015/03/17/announcing-azure-media-services-integration-with-azure-cdn-content-delivery-network/).


##<a name="next-steps"></a>Daljnji koraci

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
