<properties
   pageTitle="Servis tkanina klaster Voditelj resursa – afinitet | Microsoft Azure"
   description="Pregled konfiguriranja afinitet za servis tkanina servise"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Konfiguriranje i korištenje servisa afinitet u tkanina servisa

Afinitet je kontrola koja se nudi uglavnom da bi se olakšalo njihovo prijelaza veće monolithic aplikacije u oblak i microservices svijeta. Koji je rečeno i korištenja u određenim slučajevima kao valjane optimizirati za unaprjeđenje servise, iako to može imati popratne pojave.

Recimo da ste prebacivanja veće aplikacije ili onu koja je samo nije namijenjen s microservices na umu da biste tkanina servisa. Ovaj prijelaz je zapravo uobičajenih i zadnje vrijeme smo nekoliko kupaca (interne i vanjske) u tom slučaju. Najprije dizanja gore cijelu aplikacije u okruženju, a zatim ga pakirat početak i pokretanje. Zatim počnite ga raščlaniti u različitim servisima za manje sve objasniti međusobno.

Zatim postoji programa "Oops...". "Oops" obično pada u jednu od tih kategorija:

1. Neke komponente X u aplikaciji monolithic imali ovisnost nedokumentiranom u komponenti Y i smo one u zasebnom servisa samo uključiti. Budući da ih sada su pokrenuti na različitim čvorovi u klasteru, oni se prekida.
2.  Sljedeće komunikaciju putem (lokalni imenovani kanali | zajedničko korištenje memorije | datoteke na disku), ali zaista morate moći ažurirati neovisno o sadržaju brzinu gore malo. Tvrdi ovisnost ćete ukloniti kasnije.
3.  Sve je u redu, ali ga uključuje zapravo vrlo su ove dvije komponente chatty/performanse i mala slova. Kad su ih premjestite u zasebnom services ukupni performanse računala tanked ili Latencija povećava. Zbog toga cjelokupan aplikacija je sastanak očekivanja.

U tim slučajevima smo ne želite izgubiti naš restrukturiranje rad i ne želite da biste se vratili na monolith, no ipak Trebamo neki osjećaj kraj. To će zadržava sve dok ne možemo možete promijeniti dizajn komponente rad prirodan servisa ili dok se ne možemo očekivanja performanse neki drugi način možete riješiti ako je to moguće.

Što učiniti? Dobro se pokušajte uključiti afinitet.

## <a name="how-to-configure-affinity"></a>Kako konfigurirati afinitet
Da biste postavili afinitet, definirate odnos afinitet između dvije različite servise. Možete shvatiti afinitet "pokazivanje" jednog servisa na drugi i izgovorite "ovaj servis može pokrenuti samo gdje je li servis pokrenut." Ponekad nazivamo afinitet odnos nadređenosti (gdje pokažete dijete na nadređenog). Afinitet osigurava replike ili instance jedan servis smještaju na čvorove isti kao replike ili drugoj instanci.

``` csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

## <a name="different-affinity-options"></a>Mogućnosti za različite afinitet
Afinitet predstavlja putem jednu od nekoliko shema korelacije, a sadrži dvije različite načine. Najčešći način afinitet nazivamo NonAlignedAffinity. U NonAlignedAffinity replike ili instance u različitim servisima smještaju se na istom čvorove. Drugi način je AlignedAffinity. Poravnati afinitet koristan je samo s s praćenjem stanja servisa. Konfiguriranje dva s praćenjem stanja servisa da biste imati poravnat afinitet osigurava očuvat od tih servisa smještaju na istom čvorove međusobno. Također, uzrokuje svaki par secondaries za te servise smjestiti na isti čvorove. Moguće je i (Ako se nalazite manje čest scenarij) da biste konfigurirali NonAlignedAffinity za s praćenjem stanja servisa. Za NonAlignedAffinity različite replike dva s praćenjem stanja servisa bi collocated na istom čvorove, ali ne bi biti pokušaja Poravnajte njihove očuvat ili secondaries.

![Načini rada s afinitet i njihovi Učinci][Image1]

### <a name="best-effort-desired-state"></a>Najbolji trud želji stanja
Postoji nekoliko razlike između afinitet i monolithic arhitekturi. Mnoge od njih su jer je odnos afinitet najbolje truda. Servisi u odnosu afinitet su bitno različiti entiteti koji uspijeva neovisno premjestiti. Postoje i razloga za Zašto nije moguće prekinuti odnos afinitet. Na primjer, kapacitet ograničenja gdje samo nekih objekata servisa u odnosu afinitet stane na navedeni čvor. U tim slučajevima čak i ako postoji odnos afinitet na mjestu, ga nije moguće provesti zbog drugim ograničenjima. Ako je moguće da biste nametnuli sva ostala ograničenja i afinitet kasnije kršenja ograničenje afinitet će automatski ispravlja.  

### <a name="chains-vs-stars"></a>Nabavom nasuprot zvjezdica
Danas ne možemo modela nabavom afinitet odnosa. Što to znači da je to servis podređeni u jedan odnos afinitet ne mogu biti nadređeni objekt u nekom drugom afinitet odnosu. Ako želite model ove vrste odnosa, imate učinkovito modela kao zvjezdica, umjesto lanac. Da biste to učinili, najniže podređeni će biti parented "Srednji" podređeni nadređeno umjesto toga. Ovisno o raspored servisa, to može zahtijevati stvaranje usluge "rezerviranog mjesta" da bi služio kao nadređene više djece.

![Nabavom nasuprot zvjezdica u kontekstu afinitet odnosa][Image2]

Druge stvari koje treba Imajte na umu o odnosima afinitet danas je da su usmjereni. To znači da pravilo "afinitet" samo nameće dijete se gdje se nalazi nadređeno. Ako, primjerice nadređenog iznenada ne uspije putem drugi čvor zatim Voditelj resursa klaster ne zapravo mislite da postoji išta pogrešan dok je obavijesti koje dijete nije nalazi nadređeno; odnos odmah neće provesti.

### <a name="partitioning-support"></a>Podrška za stvaranje particija
Konačni što biste obavijest o afinitet je taj afinitet odnosi nisu podržani gdje je particije nadređenog. To je nešto što podržavamo možda naposljetku, ali danas nije dopušteno.

## <a name="next-steps"></a>Daljnji koraci
- Za dodatne informacije o drugim mogućnostima dostupnima za konfiguriranje usluge odjavu temu na druge konfiguracije Voditelj resursa klaster dostupne [Dodatne informacije o konfiguriranju Services](service-fabric-cluster-resource-manager-configure-services.md)
- Mnogo je razloga zbog koje koriste afinitet, kao što su ograničavanje servisa mali skup strojeva i pokušaja aggregate učitavanja zbirka servise, bolje podržani su putem grupe aplikacija. Pogledajte [Grupe aplikacija](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png
