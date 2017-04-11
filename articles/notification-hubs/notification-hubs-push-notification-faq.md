<properties
    pageTitle="Azure obavijesti koncentratora – najčešća pitanja"
    description="Najčešća pitanja na dizajniranje/implementacije rješenja na obavijesti koncentratora"
    services="notification-hubs"
    documentationCenter="mobile"
    authors="ysxu"
    manager="erikre"
    keywords="automatske obavijesti, slanje obavijesti, iOS automatske obavijesti, android automatske obavijesti, automatske ios, android automatske"
    editor="" />

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu" />

#<a name="push-notifications-with-azure-notification-hubs---frequently-asked-questions"></a>Automatske obavijesti s koncentratorima Azure obavijesti – najčešća pitanja

##<a name="general"></a>Općenito
###<a name="1---what-is-the-price-model-for-notification-hubs"></a>1. što je cijena modela za obavijesti koncentratora?
Obavijest o koncentratora je ponuđen u tri razine:

* **Slobodno** - dobiti do 1 milijun ih gura po pretplati mjesečno.
* **Osnovni** - ih gura get 10 milijuna po pretplati mjesečno kao referentne vrijednosti, s mogućnostima kvote rasta.
* **Standardni** - se 10 milijuna ih gura po pretplati mjesečno kao referentne vrijednosti, s kvote povećati mogućnosti plus obogaćenog telemetrijskih capabilties.

Na stranici [Cijene za obavijesti koncentratora] možete pronaći najnovije detalje. Cijene je pokrenut na razini pretplate i temelji se na broj initiations automatske obavijesti da nije važno koliko prostora naziva ili obavijest koncentratora koji ste stvorili u pretplatu za Azure.

**Slobodno** sloju je ponuđen svrhu razvoja s jamstva SLA. Iako ovaj sloju može biti dobru početnu točku za one koje želite proučiti mogućnosti automatske obavijesti putem koncentratora Azure obavijesti, možda neće biti najbolji odabir za srednje velike skaliranje aplikacijama.

**Osnovni** & **standardne** razine ponuđen za korištenje radnog sljedeće ključne značajke omogućene *samo za standardu sloju*:

- *Obogaćeni telemetrijskih* - ponuda obavijesti koncentratora broj mogućnosti za izvoz telemetrijskih podataka kao i automatske obavijesti informacije o registraciji radi izvanmrežnog pregledavanja i analize.
- *Više tenancy* - Ideal ako stvarate mobilne aplikacije pomoću obavijesti koncentratora za podršku više klijenata. Poravnanje omogućuje postavljanje vjerodajnica automatske obavijesti Services (PNS) na razini prostor naziva obavijesti koncentrator za aplikaciju, a zatim segregate klijenata za pružanje pojedinačne koncentratora u odjeljku imenski uobičajenih. Omogućuje olakšani održavanja dok čuvanja tipke SAS za slanje i primanje automatske obavijesti iz koncentratora obavijesti segregated za svaki klijent osiguravanje preklapanja nije više klijenta.
- *Zakazano automatske* - omogućuje zakazivanje automatske obavijesti koje će se naknadno u redu i slati.
- *Skupno uvoz* - omogućuje vam da biste uvezli registracije masovno.

###<a name="2---what-is-the-notification-hubs-sla"></a>2. što je koncentratora SLA obavijesti?
Za **osnovne** i **standardne** obavijesti koncentratora razine, ne možemo jamči da barem 99.9% vrijeme pravilno konfigurirani aplikacija će moći slati automatske obavijesti i izvođenje operacija upravljanja Registracija vezana uz obavijest koncentrator implementiran unutar podržani sloju. Da biste saznali više o naš SLA, posjetite stranicu [SLA koncentratora obavijesti] .

> [AZURE.NOTE] Postoje bez SLA jamstva za leg između servisa platforme obavijesti i uređaja jer obavijesti koncentratora ovise o vanjskim platformu davatelji izlaganje automatske obavijesti na uređaju.

###<a name="3---which-customers-are-using-notification-hubs"></a>3. koji korisnici koriste koncentratora obavijesti?
Imamo velikog broja kupci obavijesti koncentratora pomoću nekoliko najvažnije ovima u nastavku:

* Sochi 2014. – 100s kamata grupe, 3 + milijuna uređaja, 150 + milijuna obavijesti poslanih u dva tjedna. [CaseStudy - Sochi]
* Skanska - [CaseStudy - Skanska]
* Vrijeme Seattle - [CaseStudy - Seattle puta]
* Mural.LY - [CaseStudy - Mural.ly]
* 7Digital - [CaseStudy - 7Digital]
* Bing aplikacije – 10s milijune uređaja, slanje obavijesti dan milijuna 3.

###<a name="4-how-do-i-upgrade-or-downgrade-my-notification-hubs-to-change-my-service-tier"></a>4. kako nadograditi ili prijeći na nižu Moje koncentratora obavijesti da biste promijenili moj servis sloju?
Idite na [Klasični Portal Azure], kliknite Knjižna servisa pa kliknite na svoje mjesto za naziv pa koncentratora za obavijesti. Na kartici skaliranje moći promijeniti svoje sloju servisa koncentratora obavijesti.

![](./media/notification-hubs-faq/notification-hubs-classic-portal-scale.png)

##<a name="design--development"></a>Dizajn i razvoj
###<a name="1---which-server-side-platforms-do-you-support"></a>1. koje poslužiteljsko platforme koje podržava?
Dajemo SDK-ovi i [dovršavanje uzorka] za .NET, Java, PHP, Python, Node.js tako da je pozadinska aplikacija može biti instalacijski program za komunikaciju s koncentratorima obavijesti na bilo koji od tih platformama. Obavijest o koncentratora API-ji temelje se na OSTALE sučelja da biste odabrali izravno razgovarati s onima umjesto ako ne želite da biste dodali dodatni ovisnosti. Dodatne informacije možete pronaći na stranici [NH - REST API -ji] .

###<a name="2---which-client-platforms-do-you-support"></a>2. koju klijentskih platformi koje podržavaju?
Podržavamo slanje automatske obavijesti da biste [operacijskim sustavom Apple iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Univerzalni sustava Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android Kina (putem Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) & [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Aplikacija za Chrome](notification-hubs-chrome-push-notifications-get-started.md) i [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari) platformama. Za popis svih upute za prve korake koje tackling slanje automatskih obavijesti na te platforme, posjetite naš [NH – vodiči za početak rada] .

###<a name="3---do-you-support-smsemailweb-notifications"></a>3. koje podržavaju obavijesti SMS/e-pošte i web-stranice?
Obavijest o koncentratora prvenstveno je namijenjen za slanje obavijesti za mobilne aplikacije pomoću platforme naveden. Nije još dajemo mogućnost slanja e-pošte ili SMS upozorenja; No platforme treće strane koja sadrže te mogućnosti možete integrirana zajedno s koncentratorima obavijesti da biste poslali izvorni automatske obavijesti pomoću [Azure mobilne aplikacije].

Obavijesti koncentratora također pružaju sustava u pregledniku automatske obavijesti isporuke servisa Izlaz u-tvorničke. Korisnici možete implementirati to pomoću SignalR pri vrhu podržane platforme poslužiteljsko. Ako tražite za slanje obavijesti o pregledniku aplikacije u memoriji za testiranje Chrome, pogledajte [Vodič za Chrome aplikacije].

###<a name="4---what-is-the-relation-between-azure-mobile-apps-and-azure-notification-hubs-and-when-do-i-use-what"></a>4. što je odnos između Azure mobilne aplikacije i koncentratora Azure obavijesti i kada koristiti što?
Ako imate postojeću mobilne aplikacije pozadinskog sustava i samo želite dodati mogućnost Pošalji automatske obavijesti možete koristiti koncentratora Azure obavijesti. Upute za postavljanje mobilne aplikacije pozadinskog sustava ispočetka zatim razmotrite korištenje Azure mobilna aplikacija. Mobilne aplikacije Azure automatski dodjeljuje koncentrator obavijest koju biste mogli jednostavno slati automatske obavijesti iz mobilne aplikacije pozadine. Cijene za Azure mobilne aplikacije sadrži osnovni naknade za obavijesti koncentratora i samo plaćate kada odete izvan uključene ih gura. Dodatne informacije o troškova dostupne su na stranici [Aplikacije servisa cijene] .

###<a name="5---how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>5. koliko uređaje podržavaju ako pošaljem slanje obavijesti putem koncentratora obavijesti?
Pogledajte na stranicu [Obavijesti koncentratora cijene] detalje o broju podržanih uređaja.

Za određene scenarije, ako vam je potrebna podrška za uređaje s više od 10,000,000 registrirani, provjerite izravno [obratite nam se](https://azure.microsoft.com/overview/contact-us/) i ne možemo pomoći će vam skaliranje rješenje.

###<a name="6---how-many-push-notifications-can-i-send-out"></a>6. koliko automatske obavijesti možete poslati?
Ovisno o odabrane sloju Azure će automatski promijeniti omjer ovisno o broju obavijesti slijedi kroz sustav.

>[AZURE.NOTE] Trošak cjelokupan korištenje možete prijeći ovisno o broju automatske obavijesti koja se poslužena. Provjerite jeste li vi svjesni postojeće sloju ograničenja navedene na stranici [Obavijesti koncentratora cijene] .

Naš postojeće korisnike koriste koncentratora obavijesti da biste poslali milijune automatske obavijesti svaki dan. Ne morate ništa posebno za promjenu veličine na automatske obavijesti dosegne pod uvjetom da koristite koncentratora Azure obavijesti.

###<a name="7---how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>7. koliko dugo traje za šalju automatske obavijesti dosegne uređaju?
Azure koncentratora obavijesti je moguće obraditi najmanje **1 milijun automatske obavijesti šalje minute** u u običnom scenarij koristi se gdje dolazne opterećenje vrlo dosljedan, a ne spikey u prirode. Ovaj tečaj mogu se razlikovati ovisno o broju oznake, priroda dolazne šalje i ostali čimbenici vanjski.

Vrijeme isporuke Procijenjena servis je mogućnost izračun ciljeve po platforme i usmjeravanje poruka na odgovarajući automatske obavijesti isporukom koji se temelje na izrazima registrirani oznake/oznaka. Na tom mjestu na je odgovornosti automatske obavijesti services (PNS) za slanje obavijesti na uređaju.

Na PNS ne jamči sve SLA za obavijesti; Međutim, obično je većina automatske obavijesti isporučuju ciljnih za nekoliko minuta (obično unutar ograničenja od 10 minuta) iz vrijeme slanja naš platforme. Mogu postojati nekoliko outliers koji može trajati dulje.

>[AZURE.NOTE] Azure obavijesti koncentratora sadrži pravilo na mjestu za prekid automatske obavijesti koji se ne mogu se isporučuju s PNS 30 minuta. Odgode se može dogoditi nekoliko razloga, većina najčešće jer u PNS je ograničavanje aplikacije.

###<a name="8---is-there-any-latency-guarantee"></a>8. postoji neki jamstva Latencija?
Zbog prirode automatske obavijesti (isporuke tako da na vanjskim, ovisne automatske obavijesti servis), nema jamstva Latencija. Obično se većina automatske obavijesti isporučena unutar nekoliko minuta.

###<a name="9---what-are-the-considerations-i-need-to-take-into-account-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>9. koji su pitanja vezana uz morate poduzeti u obzir prilikom dizajniranja rješenje s prostora naziva i koncentratora obavijesti?

####<a name="mobile-appenvironment"></a>Mobilne aplikacije/okruženje

* Treba jedan koncentrator obavijesti po mobilne aplikacije po okruženju.
* U scenariju s više klijentu svaki klijent mora imati zasebne koncentratora.
* Nikad ne morate dijeliti isti koncentrator obavijesti između test i radnog okruženja kao što je to može uzrokovati probleme u retku prilikom slanja obavijesti. Npr Apple nudi izdvojeni i automatske radnog krajnje točke sa svakom pojavljuju zasebnom vjerodajnice.
* Prema zadanim postavkama, možete poslati obavijesti test uređajima registrirani putem portala za Azure ili komponentu Azure integrirani u Visual Studio. Prag postavljen je na 10 uređajima koji su odabrani nasumično skup registracije.

>[AZURE.NOTE] Ako središtu je izvorno konfigurirana potvrdom izdvojeni Apple, a zatim rekonfigurirati da biste koristili potvrdu radnog Apple, stari tokeni uređaj će biti valjane novim certifikatom zbog čega ih gura uvoza. Preporučuje se odvojite proizvodnje i testiranje okruženja za i koristiti različite koncentratora različitim okruženjima.

####<a name="pns-credentials"></a>PNS vjerodajnice

Kad mobilne aplikacije registriran s portala za razvojne inženjere na platformi (npr. Apple ili Google itd) pa se aplikacije identifikator i sigurnost tokeni koje je pozadinska aplikacija treba pružiti servisima na platformi automatske obavijesti da biste mogli slati slanje obavijesti za uređaje. Ove sigurnosnih tokena koje mogu biti u obliku certifikati (npr. za Apple iOS ili Windows Phone) ili sigurnosnih ključeva (Google Android, Windows itd) potrebno je konfigurirati u koncentratora obavijesti. To obavlja obično na razini koncentrator obavijesti, ali možete napraviti na razini prostor naziva u scenariju s više klijenta.

####<a name="namespaces"></a>Prostori naziva

Prostori naziva može se koristiti za implementaciju grupiranje.  Može se koristiti i koja predstavlja sve obavijesti koncentratora za sve klijenata aplikacije isti u scenariju više klijenta.

####<a name="geo-distribution"></a>Zemlj.-distribuciju.

Zemlj raspodjele nije uvijek ključnim scenariji automatske obavijesti. Preporučuje se da biste zabilježili različite automatske obavijesti usluge (primjerice APN-ove, GCM itd), koje konačni isporuči slanje obavijesti za uređaje, nisu ravnomjerno distribuirati.

Ako imate aplikaciju koja se koristi širom svijeta, možete stvoriti nekoliko koncentratora u različitim prostorima naziva poduzimanja prednost dostupnost servisa koncentratora obavijesti u različitim područjima Azure oko globusa.

>[AZURE.NOTE] To će se povećati trošak upravljanje – osobito oko registracije, tako da to nije zaista preporučuje se i mora poduzeti samo ako postoji eksplicitnih potrebe.

###<a name="10--should-i-do-registrations-from-the-app-backend-or-directly-through-client-devices"></a>10. učiniti registracije s pozadinskom aplikacije ili izravno putem klijentskog uređaja?
Registracija iz aplikacije pozadine su korisne kada to morate učiniti provjera autentičnosti klijenta prije stvaranja Registracija ili imate oznake koje morate stvorene ili izmijenjene po pozadinskog aplikacije na temelju logike neke aplikacije. Dodatne informacije, možete potražite dodatne informacije na stranicama [pozadinskog Registracija upute] i [upute za registraciju pozadinskog - 2] .

###<a name="11--what-is-the-push-notification-delivery-security-model"></a>11. što je model sigurnosti za isporuku automatske obavijesti?
Azure obavijesti koncentratora koristiti [Zajednički pristup potpis (SAS)](../storage/storage-dotnet-shared-access-signature-part-1.md)– temelji model sigurnosti. Tokeni SAS možete koristiti na korijenskoj razini prostor naziva ili na razini zrnastog koncentratora obavijesti. Tokena SAS možete postaviti s različitim autorizacije pravila – primjerice dozvole za slanje poruka, Slušajte obavijesti dozvole itd. Dodatne informacije dostupne su u [model sigurnosti NH] dokumenta.

###<a name="12--how-should-i-handle-sensitive-payload-in-the-push-notifications"></a>12. kako rukovati osjetljive tereta u automatske obavijesti?
Sve obavijesti isporučuju ciljnih tako da na platformi automatske obavijesti Services (PNS). Kada pošiljatelja će poslati primatelju obavijest s koncentratorima Azure obavijesti pa ćemo obraditi i prosljeđivanje obavijesti odgovarajući PNS.

Sve veze pošiljatelja obavijesti koncentratora Azure, a zatim na PNS pomoću HTTPS.

>[AZURE.NOTE] Azure obavijesti koncentratora prijavite opseg poruke na bilo koji način.

Za slanje osjetljive payloads no preporučujemo da uzorak sigurne automatske gdje pošiljatelj nudi obavijest "ping s identifikatora poruka na uređaju bez osjetljive opseg i kada aplikacija na uređaju primi ovaj tereta, je moći poziva sigurne API izravno za dohvaćanje detalja poruke. Vodič za implementaciju uzorak prethodno opisan je dostupna na stranici [NH - sigurne automatske vodič] .

##<a name="operations"></a>Operacije
###<a name="1---what-is-the-disaster-recovery-dr-story"></a>1. što je priče Izrada oporavak (DR)?
Dajemo opseg Izrada oporavak metapodataka na našem završetka (ime koncentrator obavijesti, niz za povezivanje i druge ključnih informacija). Kada se pokrene scenarij DR, registracije podaci **samo segmenta** infrastrukture koncentratora obavijesti koji će biti izgubljeni. Morat ćete implementacija rješenja za ponovno popunjavanje ove podatke u novu oporavak nakon koncentratora.

- *Korak 1* – stvaranje sekundarni koncentrator obavijesti u različitim podatkovnog centra. To možete stvoriti u hodu trenutku DR događaj ili možete stvoriti iz Dohvati Idi. Ga ne provjerite velik dio razlika koju mogućnost odaberete jer dodjele resursa obavijesti koncentrator je relativno brzo postupak obliku nekoliko sekundi. Imate od početka, koje će se shield iz DR događaja koje utječu na vašem mogućnosti upravljanja tako da je mogućnost se preporučuje.

- *Step2* - Hydrate sekundarni koncentrator obavijesti s registracije u središtu primarni obavijesti. Ne preporučuje se da biste zadržali registracije na oba koncentratora i pokušate sinkroniziranosti ih u hodu prilikom registracije uskoro u – obično koje niste uspjeli i zbog postojećih srednju vrijednost od registracije istječe na strani PNS. Obavijest o koncentratora čišćenja ih redovito PNS povratne informacije o registracije istekle ili nije valjana.  

Preporuke je za korištenje programa pozadinska aplikacija koje nešto od sljedećeg:

- Zadržava zadani skup registracije pri kraja tako da ga možete učiniti masovno Umetanje u središtu sekundarne obavijesti u slučaju DR

**OR**

- Uzima običnu ispis od registracije u središtu primarni kao sigurnosnu kopiju, a zatim skupnog umetnite u sekundarne NH.

>[AZURE.NOTE] Funkcija izvoza/uvoza registracije dostupni u standardnom sloju opisan je u dokumentu [Registracije izvoza/uvoza] .

Ako nemate pozadinskog, prilikom aplikaciju pokretanja na bilo kojoj od ciljne uređaje, će izvođenje novi registracija u središtu sekundarne obavijesti i naposljetku sekundarne koncentrator obavijesti će imati registrirani aktivnih uređaja.

Negativna strana je da će biti vremensko razdoblje kada uređaja koje još niste otvorili aplikacije gore nećete primati obavijesti.

###<a name="2---is-there-any-audit-log-capability"></a>2. postoji neki mogućnost zapisnika nadzora?
Sve operacije upravljanja koncentratora obavijesti idite na zapisnik postupka koji su na raspolaganju [Azure klasični Portal].

##<a name="monitoring--troubleshooting"></a>Praćenje i rješavanje problema
###<a name="1---what-troubleshooting-capabilities-are-available"></a>1. što otklanjanje poteškoća mogućnosti dostupne su?
Azure obavijesti koncentratora nude nekoliko značajki za uobičajene otklanjanje poteškoća, posebice u Najčešći scenarij oko izostavljanje obavijesti. Detalje potražite u našem [NH – otklanjanje poteškoća s] koje.

###<a name="2---what-telemetry-features-are-available"></a>2. koje telemetrijskih su značajke dostupne?
Prikaz podataka za telemetriju [Azure klasični Portal]Azure omogućuje koncentratora obavijesti. Detalje o dostupna metriku dostupne su na stranici [NH - metriku] .

>[AZURE.NOTE] Uspješan obavijesti samo znači da ste isporučena automatske obavijesti vanjskih automatske obavijesti o servisu (npr. APN-ove za Apple, GCM za Google itd). Preporučuje se najviše PNS za isporuku obavijesti na ciljnom uređaje. Obično u PNS izložiti isporuke metriku trećim stranama.  

Nudimo vam i mogućnost da biste izvezli telemetrijskih podataka programski (u **standardni** sloju). Potražite u članku [NH - uzorka metrika] detalje.

[Azure klasični Portal]: https://manage.windowsazure.com
[Obavijest o koncentratora cijene]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Obavijest o koncentratora SLA]: http://azure.microsoft.com/support/legal/sla/
[CaseStudy - Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[CaseStudy - Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[CaseStudy - Seattle vremena]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[CaseStudy - Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[CaseStudy - 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[NH - REST API-ji]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[NH – vodiči za početak rada]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Praktični vodič aplikacija za chrome]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Upute za registraciju pozadinskog]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Upute za registraciju pozadinskog - 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[Model sigurnosti NH]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[NH – vodič za sigurnu automatske]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[NH – otklanjanje poteškoća]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[NH - mjerenja]: https://msdn.microsoft.com/library/dn458822.aspx
[NH - uzorka mjerenja]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[Registracija izvoza/uvoza]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure Portal]: https://portal.azure.com
[Dovršavanje uzorka]: https://github.com/Azure/azure-notificationhubs-samples
[Azure mobilne aplikacije]: https://azure.microsoft.com/en-us/services/app-service/mobile/
[Aplikacije servisa za cijene]: https://azure.microsoft.com/en-us/pricing/details/app-service/
