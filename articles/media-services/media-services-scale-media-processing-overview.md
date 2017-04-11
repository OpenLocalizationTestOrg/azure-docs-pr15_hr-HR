<properties
    pageTitle="Skaliranje pregled obrade Media | Microsoft Azure"
    description="U ovoj se temi je pregled skaliranja Media obrade pomoću servisa Azure Media Services."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="juliako"/>


# <a name="scaling-media-processing-overview"></a>Promjena veličine pregled obrade medijske sadržaje

Ova stranica omogućuje pregled i zašto za promjenu veličine obrada medijske sadržaje. 

## <a name="overview"></a>Pregled

Media Services račun povezan je s rezervirane jedinica vrsta koji određuje Brzina kojom se obrađuju medijskih sadržaja obrade zadataka. Možete odabrati od sljedećih vrsta rezervirane jedinica: **S1**, **S2**ili **S3**. Na primjer, isti kodiranja posao pokreće brže prilikom korištenja **S2** rezervirane jedinica vrsta Usporedi vrstu **S1** . Dodatne informacije potražite u članku [Vrste rezervirane jedinica](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Osim određivanja vrsta rezervirana jedinice, možete odrediti Dodjela računa s rezerviranim jedinice. Broj dodijeljenu rezervirane jedinica određuje broj media zadataka koji se mogu biti obuhvaćene istovremeno zadani račun. Ako, na primjer, ako vaš račun ima pet rezervirana jedinice, a zatim zadaci pet media će imati istovremeno toliko kao postoje zadataka za obradu. Preostali zadatke će čekati u redu čekanja, te će se izdvajati prema gore za obradu sekvencijalno kada se završi izvodi zadatak. Ako račun nije sve rezervirane jedinice dodjeli, zatim Zadaci će se izdvajati sekvencijalno. U ovom slučaju vrijeme čekanja između jedan zadatak dovršetka i dalje jedan početni ovise o dostupnosti resursa u sustavu.

## <a name="choosing-between-different-reserved-unit-types"></a>Odabir između različitih rezervirane jedinica vrsta

U sljedećoj su tablici olakšava vam određivanje odluke prilikom odabira između različitih kodiranja brzine. Također nudi nekoliko slučajeva usporednih te sadrži SAS URL-ovi koje možete koristiti da biste preuzeli videozapisa na kojem možete izvršiti vlastite testove:

Scenariji|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Namjena slova| Jednostruki kodiranje brzina prijenosa. <br/>Datoteke na SD ili ispod razlučivosti, put ne osjetljive, malo troškova.|Jedan brzina prijenosa i više kodiranja brzina prijenosa.<br/>Normalni korištenja za šifriranje SD i HD. |Jedan brzina prijenosa i više kodiranja brzina prijenosa.<br/>Potpuna HD i 4K razlučivost videozapisa. Vrijeme i mala slova, brže turnaround kodiranja. 
Usporednih|[Unos datoteke: pet minuta 640x360p pri 29.97 okvira/sekundi](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Kodiranje jedan brzina prijenosa datoteka MP4, na istu razlučivost traje približno 11 minuta.|[Ulazna datoteka: pet minuta 1280x720p pri 29.97 okvira/sekundi](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>Kodiranje s "H264 jedan brzina prijenosa 720p" unaprijed postavljeni potrajati više od pet minuta.<br/><br/>Kodiranje s "H264 više brzina prijenosa 720p" Zadana Postava traje približno 11.5 minuta.|[Unos datoteke: pet minuta 1920x1080p pri 29.97 okvira/sekundi](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>Kodiranje s "H264 jedan brzina prijenosa 1080p" unaprijed postavljeni traje približno 2.7 minuta.<br/><br/>Kodiranje s "H264 više brzina prijenosa 1080p" Zadana Postava traje približno 5.7 minuta.

##<a name="considerations"></a>Razmatranja

>[AZURE.IMPORTANT] Pregledajte pitanja vezana uz navedena u ovom odjeljku.  

- Rezervirane jedinice radi parallelizing sve obrada medijskih sadržaja, uključujući indeksiranje zadatke pomoću Azure Media indeksiranje.  Međutim, za razliku od kodiranje poslova indeksiranja ne se obrađuju brže s brže rezervirane jedinice.

- Ako koristite zajednički skup, odnosno bez sve rezervirane jedinice Kodiraj zadataka imati isti performanse s S1 RUs. No postoji imati bez gornja granica vrijeme zadataka potrošiti na u redu čekanja stanje pa u bilo kojem trenutku, od najviše jedan zadatak.

- Sljedeće podatke središta nude jedinica vrsta **S2** rezervirane: Brazil Jug, Indija Zapad, središnje Indija i Jug Indija.

- Sljedeće podatke središta nude jedinica vrsta **S3** rezervirane: Brazil Jug, zapadni Indija Indija središnje.

- Najveći broj jedinica navedene za razdoblje 24-satni služi za izračun trošak.


##<a name="quotas-and-limitations"></a>Kvota i ograničenja

Informacije o kvotama i ograničenja i kako da biste otvorili zahtjev za podršku možete potražite u članku [kvotama i ograničenja](media-services-quotas-and-limitations.md).

##<a name="next-step"></a>Sljedeći korak

Postizanje skaliranja zadatak obrada medijske sadržaje s jednim od tih tehnologija: 

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [Portal](media-services-portal-scale-media-processing.md)
- [OSTALE](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
