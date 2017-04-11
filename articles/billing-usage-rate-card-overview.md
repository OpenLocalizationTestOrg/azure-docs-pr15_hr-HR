<properties
   pageTitle="Rast uvida u sustava Microsoft Azure resursa potrošnje | Microsoft Azure"
   description="Pruža konceptualni pregled korištenja naplata Azure i RateCard API-ji koji se koriste za uvida u potrošnje Azure resursa i trendova."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="gain-insights-into-your-microsoft-azure-resource-consumption"></a>Rast uvida u svoje potrošnje resursa za Microsoft Azure

Klijentima i partnerima potrebna mogućnost točno predviđanje i upravljanje njihove Azure troškove.  Kao što su premjestili iz programa Capex s Opex modelom, također moraju mogućnost showback nasuprot chargeback analizu, zaduženja Navedite način vjernosti u procjeni i naplatu, posebice za velike oblaka implementacije.

Korištenje resursa Azure i stopa kartica API-ji spominju u tu adresu članak te potrebama omogućivanjem nove uvide u svoje potrošnje Azure resursa.  

## <a name="introducing-the-azure-resource-usage-and-ratecard-apis"></a>Uvod u korištenje Azure resursa i RateCard API-ji

Korištenje resursa Azure i RateCard API-ji primjenjuju se kao davatelja resursa, kao dio obitelji API-ji izložen Azure resursa Upravitelj.  

### <a name="azure-resource-usage-api-preview"></a>Korištenje Azure resursa API-JA (pretpregled)
Klijentima i partnerima možete koristiti API za korištenje resursa Azure da biste svoje podatke Procijenjena Azure potrošnje. Značajke obuhvaćaju sljedeće:

- **Utemeljeno na ulogama azure kontrola pristupa** - klijentima i partnerima možete konfigurirati svoje pravilnike za pristup za [Azure portal](https://portal.azure.com) ili putem [Azure PowerShell cmdleti](powershell-install-configure.md) za određivanje koji korisnici ili aplikacije možete dobiti pristup podataka o korištenju svoje pretplate. Pozivatelji morate koristiti standardni tokeni Azure Active Directory za provjeru autentičnosti. Pozivatelj mora biti dodan ili Reader, vlasnik ili suradničke ulogu da biste pristupili podatke o korištenju za određenu pretplatu Azure.

- **Hourly ili dnevni zbrajanja** - pozivatelji možete odrediti želite li svoje podatke za Azure korištenje svaki sat memorijski blokovi ili dnevni memorijski blokovi. Zadana vrijednost je dnevno.

- **Instancu metapodatke navedene (obuhvaća resursa oznake)** – instancu razina detalja kao što su potpuno kvalificiran resursa uri (/subscriptions/ {id pretplate} /..), zajedno s resursa grupe oznake podataka i resursa objavit ćemo zajedno u odgovoru. To će korisnicima olakšava deterministically i programski dodijeliti korištenje po oznake za korištenje slučajeva sviđa unakrsno puni.

- **Resurs metapodatke navedene** - detalje o resursu kao što su metar naziv, kategorija metar, metar potkategorije, jedinica i regija će se proslijediti u odgovoru da bi se dobilo na pozivatelji bolje razumijevanje koje je potrošena. Također Radimo na poravnanje terminologija metapodataka resursa na portalu Azure Azure korištenje CSV, EA naplata CSV i druge sadržaje dostupnog javnosti da biste omogućili klijente povezivanje podataka putem sučelja.

- **Korištenje za sve vrste ponudu** – podataka o korištenju bit će dostupne za sve nude vrste uključujući Pay-as-you-go, MSDN, novčani izvršenja, novčani odobrenja i EA između ostalog.

### <a name="azure-resource-ratecard-api-preview"></a>Azure resursa RateCard API-JA (pretpregled)
Klijentima i partnerima možete koristiti RateCard API-JA resursa Azure da biste dobili popis dostupnih resursa Azure s očekivanim informacije o cijenama za svaki. Značajke obuhvaćaju sljedeće:

- **Utemeljeno na ulogama azure kontrola pristupa** - klijentima i partnerima možete konfigurirati svoje pravilnike za pristup za [Azure portal](https://portal.azure.com) ili putem [Azure PowerShell cmdleti](powershell-install-configure.md) za određivanje koji korisnici ili aplikacije možete dobiti pristup podacima RateCard. Pozivatelji morate koristiti standardni tokeni Azure Active Directory za provjeru autentičnosti. Pozivatelj mora biti dodan ili Reader, vlasnik ili suradničke ulogu da biste pristupili podatke o korištenju za određenu pretplatu Azure.

- **Podrška za Pay-as-you-go, MSDN, novčani izvršenja i novčane kreditnoj nudi (EA nije podržan)** – ovaj API sadrži Azure ponudu razinom stopa informacije, nasuprot razinom pretplate.  Pozivatelja taj API mora proći u podatke o ponudi detalje o resursu i stope.  Kao što je ponuda za EA ste prilagodili stope po registraciju, nije se moguće omogućuju stope EA trenutno.

## <a name="scenarios"></a>Scenariji

Evo nekoliko scenariji unesenima moguće s kombinacijom korištenje i RateCard API-ji:

- **Azure provedu tijekom mjeseca** – korisnici mogli koristiti korištenje i RateCard API-ji u kombinaciji da biste dobili bolji uvid u svoje oblaka provedu tijekom mjeseca, tako da svaki sat i dnevnih polja od korištenja i trošak procjene analizu.

- **Postavljanje upozorenja** – klijentima i partnerima možete postaviti tako resursa – novčani sustavom ili upozorenja na njihove potrošnje oblaka tako da Procijenjena potrošnje i procjenu trošak pomoću korištenje i RateCard API-JA.

- **Predict računa** – klijentima i partnerima može pristupiti svojim Procijenjena potrošnje i oblaka provedu i Primjena strojnog učenja algoritama za predviđanje što njihove računa bio na kraju ciklusu naplate.

- **Analize stara potrošnje troškova** – korisnici mogu i korištenje RateCard API-JA za predviđanje koliko njihove računa bio su da biste premjestili njihove radnih opterećenja Azure, uz pružanje želji korištenje brojeva. Ako korisnici imaju postojećih radnih opterećenja u drugim oblaka ili privatne oblaka, oni možete i mapirati njihove upotrebe s na Azure provedu tečajeve da biste dobili bolji procjenu njihove Procijenjena Azure. To sadrži poboljšani prikaz što je moguće dohvatiti putem [Azure određivanje cijena Kalkulator](https://azure.microsoft.com/pricing/calculator/)Osigurajte (na primjer) našim partnerima naplata mogućnost pivot na ponudu i Usporedba/kontrast između ponude za različite vrste izvan Pay-As-You-Go, uključujući novčani izvršenja i novčane kreditne kartice. API-ji pružaju mogućnost cijena procjeni promjene regiju, omogućivanje vrste što-ako analiza potrebne za implementaciju odluke, kao što je implementacija resursa u različitim prevođenja diljem svijeta mogu imati Izravni utjecaj na ukupni trošak.

- **Analiza "što ako"** -

    - Klijentima i partnerima možete odrediti hoće li bio bi više učinkovit da biste pokrenuli njihove radnih opterećenja u drugoj regiji ili na drugu konfiguraciju Azure resursa. Azure resursa troškove mogu se razlikovati ovisno o Azure područje u kojem su pokrenuti, a time klijentima i partnerima da biste dobili optimizacije trošak.

    - Klijentima i partnerima možete odrediti Ako neku drugu vrstu ponuda za Azure omogućuje bolju stopa na Azure resursa.

## <a name="partner-solutions"></a>Partnerskih rješenja

[Korištenja sustava Microsoft Azure i RateCard API -ji omogućiti Cloudyn da biste pružaju ITFM za kupce](billing-usage-rate-card-partner-solution-cloudyn.md) opisuje sučelje za integraciju nudi partnera API naplata Azure [Cloudyn](https://www.cloudyn.com/microsoft-azure/).  Ovaj članak sadrži detaljne opseg svoja iskustva, uključujući kratki videozapis koji prikazuje kako Azure klijenta možete koristiti Cloudyn i naplata Azure API-pozitivne uvid u iz svoje podatke za Azure potrošnje.

[Oblak Cruiser i naplata Integracija API programa Microsoft Azure](billing-usage-rate-card-partner-solution-cloudcruiser.md) opisuju [Express Cruiser oblaka za Azure paket](http://www.cloudcruiser.com/partners/microsoft/) funkcioniranje izravno na portalu WAP Omogućivanje korisnici jednostavno upravlja obje u radu i financijske aspektima njihovim Microsoft Azure privatni ili glavnom računalu javno oblaka iz jednog korisničkog sučelja.   

## <a name="next-steps"></a>Daljnji koraci
+ Pogledajte [Azure naplata REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) više pojedinosti o oba API koje su dio skupa API-ji dobili Azure resursa Upravitelj.
+ Ako želite da biste prikazali desno uzorak koda, pogledajte naše Microsoft Azure naplata API primjere koda na [Primjere koda Azure](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>uči više
+ Potražite u članku [Pregled upravljanja resursima Azure](azure-resource-manager/resource-group-overview.md) da biste saznali više o Voditelj resursa Azure.
+ Dodatne informacije o paket alata potrebne za pomoć u pokušavaju poznavati oblaka provedu, pogledajte članak Gartner [Tržištu vodič za IT financijske Management (ITFM) alate](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).
