<properties 
   pageTitle="Vodiči za otklanjanje poteškoća Azure mobilne radnje" 
   description="Vodič za Azure mobilne radnje za otklanjanje poteškoća" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure mobilne radnje – vodič za otklanjanje poteškoća

## <a name="introduction"></a>Uvod
Sljedeći vodič za otklanjanje poteškoća će vam olakšati razumijevanje korijenskog uzroka nekih često vide problema i će omogućuju vam da biste otklonili vlastite. 

## <a name="general"></a>Općenito

Općenito govoreći, trebali biste uvijek provjeriti sljedeće:

1. Provjerite imaju li se nestaje kroz sve korake potrebne za integraciju kao što je opisano u našem [vodiči za početak rada](mobile-engagement-windows-store-dotnet-get-started.md)
2. Koristite najnoviju verziju platforme SDK-ovi. 
3. Testirajte na uređaju sa sustavom stvarni i programa emulator jer su specifične za emulator samo neke od problema. 
4. Koje su odlazak neki ograničenja/Reguliranje iz mobilne radnje koje se [ovdje](../azure-subscription-service-limits.md)
5. Ako se ne možete povezati s pozadinskog usluga mobilne radnje ili prikazuju podataka neće se učitati neprestano zatim provjeriti postoje li bez tijeku incidentima tako da [ovdje](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>"Praćenje" problema

### <a name="i-am-not-seeing-my-device-showing-up-on-the-monitor-tab"></a>Ne vidi se prikazuju na kartici Monitor uređaj
Kartica monitor prikazuje uređaja povezanih s svoju platformu Mobile radnje u stvarnom vremenu. Ako su pogrešaka na emulator i uređaja, trebali biste vidjeti ovdje najmanje jednu sesiju. Ako je omogućeno distribuirao aplikaciju, će biti navedeno mjerača aktivnih sesija odražavaju uređaja koji su povezani s platformu u stvarnom vremenu. 

Ako ne vidite uređaj na kartici Monitor je vjerojatno problem Integracija SDK. Nekoliko uobičajenih koraka da otkloniti poteškoće s su na sljedeći način:

1. Provjerite koristite niz odgovarajuće veze u mobilnoj aplikaciji te je u SDK tipke i ne API sekciji tipke. Niz za povezivanje povezuje aplikacije za mobilne uređaje na instancu radnje mobilne aplikacije u kojima će vidjeti vaš uređaj na kartici Monitor. 
2. Za platformu sustava Windows – ako nadjačava svoju stranicu na `OnNavigatedTo` način, provjerite jeste li poziva `base.OnNavigatedTo(e)`.
3. Ako su integriranje Mobile radnje u postojeće mobilne aplikacije, a može osigurati i da nije nedostaju sve korake tako da pogledate napredne integracije korake [u nastavku](mobile-engagement-windows-store-integrate-engagement.md)
4. Provjerite je li šaljete barem jedan zaslon/aktivnosti po nadjačavanje na stranicu s EngagementActivity ovisno o platformu radite kao što je opisano u [vodiči za početak rada](mobile-engagement-windows-store-dotnet-get-started.md).

### <a name="i-am-seeing-the-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Prikazuju se kartice monitora s prikazanim sesije čak i kada imam prekinuta ili zatvaranja aplikacije / emulator. 
Ako su samo jedan sada povezana platformi i koristite je emulator da biste otvorili aplikaciju pa to je vjerojatno zbog emulator quirks. Općenito govoreći, morate biti sigurni da vam vratite se na početnom zaslonu na emulator za sesiju aplikacije da biste prekinuli vezu uspješno. Uz to, na platformi Windows tijekom ispravljanje pogrešaka s Visual Studio, morate da biste bili sigurni da u Visual Studio otvorite traku izbornika **Životni ciklus događaje** te kliknite **Suspend** zaista zatvoriti sesiju. Detalje potražite u članku [Vodič za Windows](mobile-engagement-windows-store-dotnet-get-started.md) . 

## <a name="analytics-issues"></a>Problemi s "Analize"

### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>Se ne prikazuju se svi podaci / osvježavanja podataka na kartici Analytics 
Redovito ponovnog izračuna analize podataka, a može potrajati i do 24 sata za osvježavanje. U ovom se podaci ne Realno vrijeme i vidjet ćete osvježiti u ovom 24 sata vremensko razdoblje.
Provjerite je li Međutim da šaljete barem jedan zaslon ili aktivnost pozadinskog platformu za neki overriding barem jednu stranicu s `EngagementActivity` ili pozivanje `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-the-analytics-tab"></a>Prikazuju se ispravno snimljenu datuma i vremena za uređaj na kartici Analytics
Razdoblje za analizu temelji se na datum iz korisničku postavke uređaja. Pa provjerite imaju li uređaj datum ispravno postavljena. 

## <a name="segment-issues"></a>Problemi s "Segmenta"

### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Sam stvorio segment i prikazuju zasivljen ili ne prikazuju svi podaci
Stvaranje segmenta nije u stvarnom vremenu u trenutku. To se izračunava istovremeno se pridružuje analize podataka i tako može potrajati i do 24 sata. Provjerite ponovno kasnije, ali u međuvremenu koje biste trebali provjeriti šaljete uistinu mobilne aplikacije podataka na temelju koji su i na segmenata. Npr. Ako događaj izgovorite "odnožje" ne šalje po bilo kojem mobilnom uređaju, a zatim ne može biti segmenta podatke za segment stvorena pomoću EventName = odnožje kao kriterija. Trebali biste provjeriti i SDK Integracija s da biste bili sigurni mobilne aplikacije pravilno šalje podatke. 

## <a name="reach-or-push-notifications-issues"></a>Problemi s "Do" ili automatske obavijesti

### <a name="my-push-messages-are-not-being-delivered"></a>Automatsko prosljeđivanje poruka se isporučuje 

1. Pokušajte slanja obavijesti test uređaj da biste sve komponente - mobilne aplikacije, SDK i servis provjerite jesu li pravilno povezani i moći će izlaganje automatske obavijesti. 
2. Uvijek pošalji najjednostavniji 'iz aplikacije obavijest"najprije putem kampanje koja je predviđena i niti je bilo koji kriterij publike naveden. Ovo je ponovno da biste dokazali obavijesti povezivanje ispravno funkcionira. 
3. Ako imate problema u izlaganja obavijesti u aplikaciji također je dobro prvi korak da biste isprobali najprije šalje obavijest o iz aplikacije. 
4. Provjerite je li na 'nativni automatske"je li pravilno konfiguriran za aplikacije za mobilne uređaje. Ovisno o platformi ga će ili obuhvaćaju tipke (Android, Windows) ili certifikata (iOS). U odjeljku [korisničko sučelje – postavke](mobile-engagement-user-interface-settings.md)
5. Iz aplikacije obavijesti nije moguće i blokira korisnika putem mobilne OS pa provjerite je li to nije slučaj. 
6. Provjerite da ne postavljate mogućnost *Zanemari publike automatske poslat će korisnicima putem na API-JA* u odjeljku **kampanje** razgovor kampanje jer je to će osigurati da automatske obavijesti samo se može poslati putem API-ji. 
7. Provjerite je li testirate kampanje automatske s obje uređaj koji je povezan putem Wi-Fi veze i telefonski operator mreže da biste uklonili vezu s mrežom kao izvor mogućih problema.
8. Provjerite je li sustav datuma/vremena na uređaju/emulator točan jer bilo kojeg uređaja usklađen će i ometati automatske obavijesti servisa za isporuku obavijesti. 

Dodatne platformu određene upute za otklanjanje poteškoća:

1. **iOS** 

    - Potvrda provjerite jesu li valjan i koji za iOS automatske obavijesti. 
    - Provjerite je li da pravilno konfigurirate *radnog* certifikat u svojoj aplikaciji Mobile radnje. 
    - Provjerite da testirate na na *realni fizički uređaj.* IOS simulator nije moguće obraditi slanje poruke.
    - Provjerite je li paket identifikator pravilno konfiguriran u mobilnoj aplikaciji. Pročitajte upute [u nastavku](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
    - Kada testiranje, koristite "Ad Hoc" razdiobu u profilu mobilne dodjele resursa. Nećete moći primanje obavijesti u slučaju aplikacije prikupljaju pomoću "Ispravljanje pogrešaka"

2. **Android**

    - Provjerite jeste li odredili točan broj projekta u datoteci AndroidManifest.xml mobilne aplikacije koji slijedi znak \n. 
    
            <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
        
    - Provjerite nisu nedostaju ili pogrešno konfigurirana sve dozvole u datoteci manifesta sa sustavom Android 
    - Provjerite je li broj projekta dodajete aplikacije klijenta s istim računom kojima imate tipku GCM poslužitelja. Bilo koji nepodudaranje između dviju onemogućuje vaše ih gura prelaska. 
    - Ako primate obavijesti sustava, ali ne u aplikaciji, a zatim pregled [Navedite ikona za odjeljak obavijesti](mobile-engagement-android-get-started.md) kao vjerojatno ne navodite ikonu točan u datoteci manifesta sa sustavom Android. 
    - Ako šaljete BigPicture obavijest, zatim bili sigurni da, ako imate vanjske slike poslužitelja, a koje su im potrebne da biste mogli podršku HTTP "DOHVATI" i "GLAVE".

3. **Windows**
    
    - Provjerite je li imate pridružene aplikaciju valjani aplikacije iz Windows trgovine. U Visual Studio – morat ćete desnom tipkom miša kliknite projekt odaberite mogućnost "Povezivanje s trgovine" i odaberite aplikaciju koju ste stvorili u trgovini u sustavu Windows. Aplikacija iz Windows trgovine mora biti isto nešto s kojima imate nativni automatske vjerodajnice da biste konfigurirali na portalu Mobile radnje.
    - Ako primate iz aplikacije automatske obavijesti, ali ne u samoj aplikaciji obavijesti s `EngagementOverlay` Integracija pa provjerite postoji korijenski element rešetke na stranici. Koristi EngagementOverlay prvi element "Rešetke" pronađe xaml datoteke da biste dodali dva web prikaza na stranici. Ako želite da biste pronašli gdje će biti postavljena web prikaza, možete definirati rešetke pod nazivom "EngagementGrid" kao što je to no morat ćete Provjerite postoji dovoljne visine i širine dva kasnije web-prikaza koji će se prikazati obavijest i sljedeće objava kao obavijest u aplikaciji:
        
            <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-the-notification-it-is-showing-as-active-what-does-it-mean"></a>Sam stvorio automatske obavijesti i objava/kampanje i čak i nakon što ga me šalje obavijest, se prikazuje kao "Aktivna". Što znači? 
**Kampanje** koju ste stvorili u Mobile radnje naziva tako da jer je dugo izvodi značenje obavijesti automatske kao nove uređaje povezati svoju platformu mobilne radnje, oni poslat će se automatski obavijesti ovdje, konfiguriranje pod uvjetom da se zadovoljavaju kriterij u kampanje postavite. Ovo je ne jedan Postava snimka jedan obavijesti. Morat ćete ručno kliknite gumb **Završi** da biste prekinuli kampanje tako da ga ne šalje daljnje obavijesti. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-the-app-i-get-the-same-notification-even-when-i-had-actioned-it-before"></a>Sam stvorio automatske kampanjom i primam obavijesti uspješno no kad god otvoriti gore aplikaciju, se isti obavijesti čak i kad je li imali actioned ga prije? 
To je vjerojatno da se dogodi tijekom testiranja i ako koristite Emulatora ili neke testirajte framework kao što je TestFlight. Što se događa Evo da pri svakoj aplikaciji pokrenite instancu je pri dohvaćanju novi DeviceID i pošaljite u našem pozadinskog koji uzrokuje platformu Mobile radnje tretirati kao novi uređaj i slanje obavijesti. 

## <a name="getting-support"></a>Podrška za početak

Ako ne uspijete riješiti problem sami, a zatim ste učiniti sljedeće:

1. Potražite problem postojeće niti na forumu StackOverflow i [forum na MSDN](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) i ako nije pa postavite pitanje postoji. 
2. Ako ne pronađete značajku nedostaje zatim Dodaj/glasovanja zahtjev na našem [UserVoice forum](https://feedback.azure.com/forums/285737-mobile-engagement/)
3. Ako imate Microsoft podržava Otvori slučajem za podršku tako da omogućuje sljedeće: 
    - ID pretplate Azure
    - Platformu (npr. iOS, Android itd)
    - ID aplikacije
    - ID kampanje (za automatsko prosljeđivanje obavijesti probleme)
    - ID uređaja
    - SDK radnje mobilne verzije (npr. v2.1.0 Android SDK)
    - Detalje o pogrešci s tekst poruke i scenarija
