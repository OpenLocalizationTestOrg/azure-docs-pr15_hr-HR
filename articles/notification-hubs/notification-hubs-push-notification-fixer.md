<properties 
    pageTitle="Azure obavijesti koncentratora – smjernice Dijagnostika" 
    description="Upute za dijagnosticiranje uobičajene probleme s koncentratorima Azure obavijesti." 
    services="notification-hubs" 
    documentationCenter="Mobile" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure koncentratora obavijesti – smjernice Dijagnostika

##<a name="overview"></a>Pregled

Jedan od najčešćih pitanja smo čuti klijenata Azure obavijesti koncentratora je kako otkriti zašto ne vide obavijest šalje njihove pozadinska aplikacija pojavljuju u klijentskog uređaja - prekine gdje i zašto su obavijesti i kako to ispraviti. U ovom članku smo će proći kroz različite razloga zašto obavijesti možda se prekine ili ne završavaju na uređajima. Ne možemo će i pregledajte načina na koje možete analizirati i utvrđivanje uzrok. 

Najprije, je važnosti da biste razumjeli kako Azure obavijesti koncentratora ih gura out obavijesti za uređaje.
![][0]

U tijeku uobičajeni slanje obavijesti, poruka poslana s **pozadinskom aplikacije** da biste **Središte Azure obavijesti (NH)** koje u nizu ne neke obrada na sve registracije uzimajući u obzir konfigurirani oznake i izrazi oznake da biste odredili "ciljnih web-mjesta" odnosno sve registracije koje je potrebno da biste primili obavijest automatske. Ove registracije mogu obuhvaćati preko nešto ili sve od naše podržane platforme – iOS, Google, Windows, Windows Phone uređaju Kindle i Baidu za Android u Kini. Podjela kada se na ciljeve uspostavili, preko više skupine registracije uređaja platformi NH, a zatim ih gura out obavijesti, određene **Automatske obavijesti servisa (PNS)** – npr APN-ove za Apple, GCM za Google itd. NH potvrđuje s odgovarajući PNS na temelju vjerodajnica postavite na portalu Azure klasični na stranici Konfiguriranje obavijesti koncentratora. Na PNS zatim prosljeđuje obavijesti na odgovarajući **o klijentskim uređajima**. Ovo je platformu preporučeni način za održavanje automatske obavijesti i Imajte na umu da konačna leg isporuke obavijesti odvija između platformu PNS i uređaja. Stoga imamo četiri glavna komponente - *klijent*, *pozadinska aplikacija*, može uzrokovati obavijesti početak prekine *Azure obavijesti koncentratora (NH)* i *Automatske obavijesti Services (PNS)* i nešto od sljedećeg. Dodatne informacije o toj arhitekturi dostupna je na [Pregled koncentratora obavijesti].

Pogreška za isporuku obavijesti se može dogoditi tijekom početne test/pripremna faza koji mogu upućivati na problem s konfiguracijom ili može se dogoditi u radnog gdje sve ili neke obavijesti o može biti početak izostavljanje kojoj stoji da se neka aplikacija za dublju ili messaging uzorak problem. U odjeljku ispod smo će izgledati na razne izostavljanje obavijesti scenariji rasponu od uobičajenih rijetkim vrstu neki od njih ne može pronaći očite i druge nisu toliko. 

##<a name="azure-notifications-hub-mis-configuration"></a>Azure mis-konfiguracije koncentrator obavijesti 

Potrebno je provjere autentičnosti u kontekstu aplikacije za razvojne inženjere omogućiti uspješno slanje obavijesti odgovarajući PNS Azure koncentratora obavijesti. To je načinio moguće programiranje stvaranje računa za razvojne inženjere s odgovarajući platformu (Google Apple, Windows itd) i registracija svoje aplikacije gdje mogu dobiti vjerodajnice koje je potrebno je konfigurirati na portalu odjeljku Konfiguriranje obavijesti koncentratora. Ako su bez obavijesti želite putem, da biste provjerili jesi li ispravne vjerodajnice konfigurirani u središtu obavijesti koje odgovaraju s aplikacijom stvorene u odjeljku svoj račun za razvojne inženjere određene platformu mora biti prvi korak. Naš [Vodiči za početak rada] ćete pronaći korisne ići preko postupak korak po korak način. Evo nekih uobičajenih mis-konfiguracije:

1. **Općenito**
 
    a) provjerite nalazi li se naziv koncentrator obavijesti (bez pravopisnih pogrešaka) isti:

    - Gdje su Registracija iz klijentskog programa 
    - Gdje su slanja obavijesti iz pozadine,  
    - Gdje ste konfigurirali PNS vjerodajnice i 
    - Čije vjerodajnice SAS ste konfigurirali na klijentu i pozadinski. 
        
    b) provjerite jesu li koristite li ispravnu nizove konfiguracije SAS na klijentu i pozadinska aplikacija. Kao palca pravilo, morate koristiti **DefaultListenSharedAccessSignature** na klijenta i **DefaultFullSharedAccessSignature** na pozadinska aplikacija (koji daje dozvolu omogućiti slanje obavijesti na NH)

2. **Konfiguriranje Apple automatske obavijesti servisa (APN-ove)**
 
    Morate zadržati dvije različite koncentratora – jedan za proizvodnju i drugi za testiranje svrhu. To znači da prijenos certifikat namjeravate koristiti u memoriji za testiranje okruženju zasebnom koncentrator i certifikat namjeravate koristiti u radnog zasebne koncentrator. Pokušajte da biste prenijeli različitih vrsta certifikata na isti koncentrator kao što je može uzrokovati pogreške obavijesti u redak prema dolje. Ako pronađete sami na mjestu gdje ste slučajno prenijeli različitih vrsta certifikata isti koncentrator, preporučuje se da biste izbrisali središtu i pokretanje Osvježi. Ako zbog nekog razloga ne može izbrisati središtu pa pri vrlo najmanje, morate izbrisati sve postojeće registracije iz koncentratora. 

3. **Konfiguriranje poruka Google Cloud (GCM)** 

    a) provjerite sigurni da ste omogućili korištenje "Google Cloud poruka za Android" u odjeljku oblaka projekta. 
    
    ![][2]
    
    b) provjerite jesu li stvoriti "Ključ poslužitelja" dok dobivanja vjerodajnica koje NH će koristiti za provjeru autentičnosti s GCM. 
    
    ![][3]
    
    c) provjerite jesu li da ste konfigurirali "Projekta ID" na klijentskom računalu koji je entitet, numeričke koji možete dobiti na nadzornoj ploči:
    
    ![][1]

##<a name="application-issues"></a>Problemi s računala

1) **Oznake / označavanje izraza**

Ako koristite oznake ili izraza oznaka fazi publici, uvijek je moguće kada šaljete obavijesti, ima li nema cilj pronađen na temelju oznaka/oznaka izraza navodite u slanje poziva. Preporučuje se da biste pregledali vaše registracije da biste bili sigurni da su prisutne oznake koje su podudarne prilikom slanja obavijesti i provjerite isporuci samo klijenata s tim registracije. Npr. Ako sve svoje registracije s NH dovršetka s izgovorite oznake "Politika", a koju šaljete obavijest s oznakom "Sportovi", ne proslijedit će se na bilo kojem uređaju. Složena slučaja nije obuhvaćaju izraza oznaku koju ste samo registrirali s "Oznaka odgovora" ili "Oznaka B", ali prilikom slanja obavijesti, birate "Oznaka odgovora & & oznaka B". U odjeljku Savjeti za samostalno dijagnosticiranje ispod načina u kojem možete pregledati svoje registracije uz oznake koje imate. 

2) **Problemi s predloška**

Ako koristite predloške, a zatim Pobrinite se da su slijedite upute navedene što je opisano na [smjernice za predložak]. 

3) **Registracija koji nisu valjani**

Pod pretpostavkom da ispravno konfigurirano središtu obavijesti i sve oznake/oznaka izraze su se koristile pravilno rezultira traži valjani ciljnih web-mjesta na koje obavijesti treba poslati, NH aktivira isključivanje nekoliko serija obrada paralelno - svake grupe za slanje poruka skup registracije. 

> [AZURE.NOTE] Budući da moramo obrada paralelno, smo ne jamči redoslijed kojim će isporuku obavijesti. 

Sada Azure obavijesti koncentrator optimiziran je za model isporuke poruke programa "na većini jedanput". To znači da ne možemo pokušaj de-dupliciranje tako da nema obavijesti više puta se isporučuju s uređajem. Da bi to ćemo pregledajte registracije i provjerite je li samo jedne poruke poslane po identifikator uređaja prije no što zapravo pošaljete poruku u PNS. Kao što je svaku seriju šalje se PNS shodno prihvaćanja i provjera valjanosti registracije, moguće je na PNS otkrije pogreška s jednim ili više registracije u grupu, vraća se pogreška Azure NH, a Zaustavi obradu time potpuno ispuštanje te grupe. To vrijedi osobito s APN-ove koji koristi protokol TCP strujanje. Iako ne možemo su optimizirani za na većini nakon isporuke u ovom slučaju ne možemo ukloniti faulting Registracija naš baze podataka i ponovno pokušajte isporuku obavijesti za ostale uređaja u tu grupu.

Možete dobiti informacije o pogrešci za pokušaj isporuke Neuspjelo protiv Registracija pomoću Azure obavijesti koncentratora REST API-ji: [po Telemetrijskih poruka: početak Telemetrijskih poruke obavijesti](https://msdn.microsoft.com/library/azure/mt608135.aspx) i [PNS povratne informacije](https://msdn.microsoft.com/library/azure/mt705560.aspx). Potražite u članku [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) , primjerice kod.

##<a name="pns-issues"></a>Problemi s PNS

Kada se poruke s obavijesti primila odgovarajući PNS tada je njegova odgovornost za isporuku obavijesti na uređaju. Azure koncentratora obavijesti je izvan slike ovdje i ima nema kontrole kada ili ako obavijesti će se isporučivati u uređaj. Budući da su obavijesti servisa platforme vrlo robusne, obavijesti obično dosegne uređaje u nekoliko sekundi iz na PNS. Ako na PNS no je ograničavanje Azure obavijesti koncentratora Primijeni eksponencijalne natrag isključiti strategiju i ako na PNS ostaje dostupan za 30 min zatim imamo pravilo na mjestu istječe i ispustite te poruke trajno. 

Ako je PNS pokušava izlaganje obavijest, ali uređaj je izvan mreže, obavijesti se pohranjuju tako da na PNS za određeno vremensko razdoblje, a isporučena na uređaju kada postane dostupna. Samo jedan nedavne obavijesti za određenu aplikaciju pohranjen. Ako više se šalju obavijesti o dok je uređaj izvan mreže, svaki obavijest o novoj uzrokuje prethodne obavijesti da bi se odbacuju. Takvo ponašanje sinkroniziranja najnovije obavijesti se nazivaju coalescing obavijesti u APN-ove i sažimanje u GCM (koji koriti sažimanja ključa). Ako je uređaj izvanmrežne dulje vrijeme, odbacuju se obavijesti pohranjenima u tijeku za njega. Izvor – [upute za APN-ove] & [GCM upute]

S Azure koncentratora obavijesti – možete proslijediti coalescing ključ putem HTTP zaglavlje pomoću na generic `SendNotification` API-JA (npr. za .NET SDK – `SendNotificationAsync`) koji vodi i HTTP zaglavlja, a koje prosljeđuju se kao je odgovarajući PNS. 

##<a name="self-diagnose-tips"></a>Savjeti za samostalno dijagnosticiranje

U nastavku ćemo će provjeriti razne avenues dijagnosticiranje i korijenski uzrokuju probleme koncentrator obavijesti:

###<a name="verify-credentials"></a>Provjerite je li vjerodajnice

1. **PNS portala za razvojne inženjere**

    Provjerite je li ih na odgovarajući PNS za razvojne inženjere portal (APN-ove, GCM, WNS itd) pomoću naš [Vodiči za početak rada].

2. **Azure klasični portal**

    Idite na karticu Konfiguriraj da pregledavaju i odgovaraju vjerodajnice s onima dobivenog od PNS portala za razvojne inženjere. 

    ![][4]

###<a name="verify-registrations"></a>Provjerite je li registracije

1. **Visual Studio**

    Ako koristite Visual Studio za razvoj možete povezati s web-mjesto Microsoft Azure i u okvir za prikaz i upravljati skup Azure servisa uključujući koncentrator obavijesti iz "Poslužitelj programa Explorer". To je obično korisno okruženju sustava razvojni i testiranje. 

    ![][9]

    Možete pregledavati i upravljanje sve registracije u vašem središte koji su kategorizirani radi boljeg za izvorni platformu ili Registracija predložak, sve oznake, PNS identifikator, id za registraciju i datum isteka. Možete urediti i registracija u hodu – što je korisno da ako želite urediti sve oznake. 

    ![][8]
 
    > [AZURE.NOTE] Visual Studio funkcije da biste uredili registracije mogu koristiti samo tijekom razvojni i testiranje s ograničen broj registracije. Ako postoji potreba treba popraviti vaše registracije masovno, razmislite o pomoću funkcije Registracija izvoza/uvoza koje se ovdje opisuju - [Registracije izvoza/uvoza] (https://msdn.microsoft.com/library/dn790624.aspx)

2. **Servis Bus explorer**

    Više klijenata pomoću ServiceBus explorer koje se ovdje opisuju - [ServiceBus Explorer] radi prikaza i upravljanje njihove koncentrator obavijesti. Je li za Otvori izvor projekt iz code.microsoft.com - [ServiceBus Explorer kod]

###<a name="verify-message-notifications"></a>Provjerite je li poruka obavijesti

1. **Azure klasični Portal**

    Možete prijeći na karticu "Ispravljanje pogrešaka" za slanje obavijesti test klijentima bez potrebe za bilo koji servis pozadinskog prema gore i pokrenut. 

    ![][7]

2. **Visual Studio**

    Testiranje obavijesti možete poslati i iz comforts Visual Studio:

    ![][10]

    Dodatne informacije na funkcionalnost Visual Studio obavijesti koncentrator Azure explorer ovdje - 
    
    - [Dodavanje veze za VANJSKIH poslužitelja Explorer pregled]
    - [Dodavanje veze za VANJSKIH poslužitelja Explorer blogu - 1]
    - [Dodavanje veze za VANJSKIH poslužitelja Explorer blogu - 2]

###<a name="debug-failed-notifications-review-notification-outcome"></a>Nije uspjelo obavijesti za ispravljanje pogrešaka / pregledajte rezultat obavijesti

**Svojstvo EnableTestSend**

Kada pošaljete obavijest putem koncentratora obavijesti, na početku ga samo dobiti u redu za NH za obradu da biste utvrdili sve njegove ciljnih web-mjesta i zatim naposljetku NH šalje se na PNS. To znači da prilikom korištenja REST API-JA ili bilo koji od klijenta SDK, uspješno povrat poziva Pošalji samo znači da poruka je uspješno u redu s obavijesti koncentratora. Ne nudi uvida u što se dogodilo kada NH naposljetku imate da biste poslali poruku PNS. Ako vaš obavijesti ne stizati u klijentskog uređaja, postoji mogućnost da kada NH pokušao isporučiti poruku PNS, došlo je do pogreške – primjerice veličina tereta premašuje najveći dopušteni tako da na PNS ili vjerodajnice konfiguriran u NH nisu valjana itd. Da biste dobili uvida u PNS pogreške, ne možemo ste uvedena svojstvo [EnableTestSend značajku]. Svojstvo je automatski omogućena kad šaljete poruke test s portala i Visual Studio klijenta i stoga omogućuje vam da biste vidjeli detaljne informacije za ispravljanje pogrešaka. To možete koristiti putem API-ji poduzimanja primjer SDK .NET gdje je trenutno dostupan i dodat će se u svim klijentskim SDK-ovi koju koristite. Da biste koristili to s OSTATKOM poziva, jednostavno dodavanje parametra querystring pod nazivom "testiranje" na kraju slanje poziva npr. 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Primjer (.NET SDK)*
 
Pretpostavimo da koristite .NET SDK za slanje nativni skočnoj obavijesti:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
 
`result.State`će jednostavno stanja `Enqueued` na kraju izvođenja bez sve uvid u što se dogodilo s vašeg pribadače. Sada možete koristiti u `EnableTestSend` logičko svojstvo vrijeme pokretanje u `NotificationHubClient` te možete nabaviti detaljno stanje o pogreškama PNS tijekom slanja obavijesti. Pošalji poziv Ovdje će potrajati neko vrijeme da biste se vratili jer je samo vraćanje nakon NH je isporučena obavijesti PNS da biste odredili rezultat. 
 
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);
    
    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);
    
    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Ogledna Izlaz*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong
 
Ovu poruku označava bilo koji nisu valjani vjerodajnice nije konfiguriran u središtu obavijesti ili problem s registracije u središtu i preporučena tečaja bio da biste izbrisali ovaj Registracija i omogućuju klijent ponovno stvoriti prije slanja poruke. 
 
> [AZURE.NOTE] Imajte na umu da koristi to svojstvo intenzivnog ograničio vrijeme i tako da morate koristiti samo to u okruženju razvojni i testiranje s ograničenim registracije. Ne možemo samo slanje obavijesti za ispravljanje pogrešaka 10 uređaje. Imamo i ograničenje od obradu pogrešaka šalje da je 10 u minuti. 

###<a name="review-telemetry"></a>Pregled telemetrijskih 

1. **Korištenje Azure klasični Portal**

    Na portalu omogućuje vam da biste dobili Kratak pregled svih aktivnosti koncentratora za obavijesti. 
    
    a) na kartici "nadzorne ploče" možete pogledati Zbrojeno prikaza na registracije, obavijesti kao i pogreškama po platforme. 
    
    ![][5]
    
    b) možete dodati mnoge druge platforme određene metrike na kartici "Praćenje" da biste se detaljnije razmotrite osobito vraćaju kada NH pokušava poslati obavijest u PNS PNS određene pogreške. 
    
    ![][6]
    
    c) trebali biste započinju pregledom **Dolazne poruke**, **Registracija operacija** **Uspješno obavijesti** i zatim prijeđite u odjeljak po platformu tab da biste pregledali PNS konkretnim pogreškama. 
    
    d) ako imate središtu obavijesti pogrešno s postavke provjere autentičnosti, a zatim prikazat će se pogreška prilikom provjere autentičnosti PNS. To je dobro oznaka da biste provjerili PNS vjerodajnice. 

2) **Programski pristup**

Ovdje – dodatne informacije 

- [Pristup programski Telemetrijskih]
- [Pristup telemetrijskih putem API-ji uzorka] 

> [AZURE.NOTE] Nekoliko telemetrijskih povezane značajke kao što su **Izvoza/uvoza registracije** **Telemetrijskih pristup putem API-ji** itd dostupne su samo standardni sloju. Ako pokušate koristiti te značajke nalazite li se u slobodan ili osnovni sloju će se poruka iznimke ovaj efekt tijekom korištenja SDK i HTTP 403 (zabranjeno) dok ih koristite izravno iz REST API-ji. Pripazite da ne prijeđete do standardna sloju putem klasične Portal Azure.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png
 
<!-- LINKS -->
[Pregled koncentratora obavijesti]: notification-hubs-push-notification-overview.md
[Vodiči za početak rada]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Upute za predložak]: https://msdn.microsoft.com/library/dn530748.aspx 
[Upute za APN-ove]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[Upute za GCM]: http://developer.android.com/google/gcm/adv.html
[Registracija izvoza/uvoza]: http://msdn.microsoft.com/library/dn790624.aspx
[ServiceBus Explorer]: http://msdn.microsoft.com/library/dn530751.aspx
[Kôd ServiceBus]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Dodavanje veze za VANJSKIH poslužitelja Explorer pregled]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[Dodavanje veze za VANJSKIH poslužitelja Explorer blogu - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[Dodavanje veze za VANJSKIH poslužitelja Explorer blogu - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[Značajka EnableTestSend]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Pristup programski Telemetrijskih]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Pristup telemetrijskih putem API-ji uzorka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

 