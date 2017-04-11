<properties 
   pageTitle="Pružanje podrške i njegovo povlačenje iz upotrebe pravila vodič za Azure goste OS | Microsoft Azure" 
   description="Pruža informacije o što Microsoft će podržavati kao regards za OS goste Azure koristi servise u Oblaku." 
   services="cloud-services" 
   documentationCenter="na" 
   authors="raiye" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="10/24/2016"
   ms.author="raiye"/>

# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure goste OS pružanje podrške i njegovo povlačenje iz upotrebe pravila
Informacije na ovoj stranici odnosi se na operacijski sustav Azure goste ([S operacijskim Sustavom goste](cloud-services-guestos-update-matrix.md)) za servise u Oblaku tempiranja i uloge web (PaaS). Ne odnose na virtualnim strojevima (IaaS). 

Microsoft je u objavljeni [podržavaju pravila za goste OS](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). Stranica čitate sada opisuje kako je primijenjeno pravilniku.

Pravila 

1. Microsoft će podržavati **barem najnovije dva linije od OS goste**. Kada je povučena obitelji, korisnici imaju 12 mjeseci od datuma službeni umirovljenje da biste ažurirali na noviju podržani goste OS liniju.
2. Microsoft će podržavati na **barem dvije najnovije verzije podržanih linije goste OS**. 
3. Microsoft će podržavati na pri **najmanje najnovije dvije verzije Azure SDK**. Kada povlačenja verziju SDK korisnici će imati 12 mjeseci od datuma službeni umirovljenje da biste ažurirali na noviju verziju. 

Možda je podržano ponekad više od dva linije ili izdanja. Službeni informacije o podršci goste OS pojavit će se na [Azure goste OS izdanja i SDK kompatibilnosti matrice](cloud-services-guestos-update-matrix.md).


## <a name="when-a-guest-os-family-or-version-is-retired"></a>Kada je povučena goste OS obitelj ili verziju 


Novi goste OS **obitelji** uvodi se tijekom nakon objavljivanja nova službeni verzija operacijskog sustava Windows Server. Kad god uvodi se novi goste OS obitelji, Microsoft će povući liniji najstarije goste OS. 

OS za goste nove **verzije** uvode se o svakog mjeseca ugraditi najnovija ažuriranja za MSRC. Zbog običnog mjesečnog ažuriranja verzija OS-a za goste je obično onemogućene 60 dana nakon njegova izdanje. To će zadržati barem dvije verzije goste OS za svaki obitelj dostupan za korištenje. 

### <a name="process-during-a-guest-os-family-retirement"></a>Postupak tijekom goste OS umirovljenje za grupe 


Kada je objaviti u umirovljenje, korisnici imaju 12 mjeseci "prijelaza" razdoblja prije liniji starije službeno uklonjeni iz servisa. Ovaj put prijelaza mogu se proširiti, prema želji programa Microsoft. Ažuriranja će biti objavljena na [Azure goste OS izdanja i SDK kompatibilnosti matrice](cloud-services-guestos-update-matrix.md).

Postupak postupne umirovljenje će početi 6 mjeseci u razdoblju prijelaz. Tijekom tog razdoblja:

1. Microsoft će obavijestiti kupce u umirovljenje. 
2. Novijom verzijom Azure SDK ne podržava otpisa obitelji goste OS.
3. Novi implementacije i redeployments servise u Oblaku nije dozvoljen na otpisa obitelj

Microsoft će i dalje se predstavlja novi goste verzija OS-a ugradnje najnovija ažuriranja za MSRC do zadnjeg dana razdoblja prijelaza, koji se nazivaju "datum isteka". Tada se sve oblaka servise i dalje će podržana u odjeljku Azure SLA. Microsoft je to da biste nametnuli nadogradnje, brisati i zaustavljanje tih servisa nakon datuma.



### <a name="process-during-a-guest-os-version-retirement"></a>Postupak tijekom verzija OS-a za goste umirovljenje 
Ako korisnici svoje OS goste da bi automatski ažurirao, nikad imaju razmišljati o verzija OS-a za goste. Oni će uvijek koristite najnoviju verziju goste OS.

Verzija OS-a za goste objavljivanja svakog mjeseca. Zbog rata običnog izdanja u svakoj verziji ima fixed vrijeme upotrebljivosti.

Na 60 dana u na vrijeme upotrebljivosti verzija je "*onemogućeno*". "Onemogućene" znači verzija nestaje s portala za Azure klasični. Ga možete više ne može postaviti iz konfiguracijske datoteke CSCFG. Postojeće implementacijama su pokrenuta, ali novi implementacije i kod i konfiguraciji ažuriranja postojećeg implementacijama nije dozvoljen. 

Na kasnije, na OS goste verziju "*istekne*", a sve instalacije i dalje koristi tu verziju su prisilno nadograditi i postavljanje da bi automatski ažurirao OS gosta u budućnosti. Istek obavlja skupna tako da se može razlikovati određenog vremena iz disablement isteka. 

Tih razdoblja može izvršiti dulje na Microsoftovu diskretni da biste olakšali prijelaza klijenta. Promjene će se prenosi na [Azure goste OS izdanja i SDK kompatibilnosti matrice](cloud-services-guestos-update-matrix.md).



### <a name="notifications-during-retirement"></a>Obavijesti o tijekom njegovo povlačenje iz upotrebe 

* **Obiteljski njegovo povlačenje iz upotrebe** <br>Microsoft će koristiti članaka za blog i Azure klasični portala obavijesti. Korisnici koji su još uvijek koriste otpisa obitelji goste OS ćete primati obavijesti putem izravne komunikacije (e-pošte, portala poruka, telefonski poziv) administratorima dodijeljena servisa. Sve promjene će biti objavljena na ovu stranicu i RSS sažetaka sadržaja na popisu na početku ovu stranicu. 


* **Verzija njegovo povlačenje iz upotrebe** <br>Sve promjene će biti objavljena na ovu stranicu i RSS sažetaka sadržaja na popisu na početku Ova stranica, uključujući izdanje, onemogućeno i datum isteka. Administratori servisa primit će poruke e-pošte ako imaju implementacijama sustavom onemogućene goste verzija OS-a i obitelji. Tempiranje te poruke e-pošte mogu se razlikovati. Obično su barem mjesec dana prije disablement, iako ova tempiranja nije službeni SLA. 


## <a name="frequently-asked-questions"></a>Najčešća pitanja

**Kako prevladavanje učinke Migracija?**

Trebali biste koristiti najnovije obitelji goste OS za dizajniranje servise u Oblaku. 

1. Počnite s planiranjem migraciju novije liniju ranije. 
2. Postavljanje implementacijama privremene test da biste testirali na servis u Oblaku radi na nova grupa. 
3. Da biste **automatski** postavili verziju goste OS (osVersion = * u datoteci [.cscfg](cloud-services-model-and-package.md#cscfg) ) tako da migracije na nove verzije s operacijskim Sustavom goste pojavljuje se automatski.

**Što ako moje web-aplikacije potreban je dublju Integracija s OS-a?**

Ako vaša arhitektura web aplikacije zahtijeva dublju ovisnost u podlozi operacijskom sustavu, korištenje platformu podržana kao što su [pokretanje zadataka](cloud-services-startup-tasks.md) ili druge proširenja mehanizme koje možda postoji u budućnosti. Osim toga, možete koristiti i [virtualnim računalima sustava Azure](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS – infrastrukture kao Service), gdje se nalazili odgovoran za održavanje Temeljni operacijski sustav.
 
## <a name="next-steps"></a>Daljnji koraci
Pregledajte najnovija [izdanja goste OS](cloud-services-guestos-update-matrix.md).
