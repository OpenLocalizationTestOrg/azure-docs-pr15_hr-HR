<properties
    pageTitle="Azure obavijesti koncentratora"
    description="Saznajte kako koristiti automatske obavijesti u Azure. Primjere koda pisane C# pomoću .NET API-JA."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="yuaxu"/>


#<a name="azure-notification-hubs"></a>Azure obavijesti koncentratora

##<a name="overview"></a>Pregled

Azure koncentratora obavijesti pružaju infrastruktura za jednostavan upotrebu, multiplatform, skalirana odgovaranjem automatske koja omogućuje da biste poslali mobilne automatske obavijesti iz bilo kojeg pozadine (u oblaku ili na lokalnim) bilo kojeg mobilnog platforme.

S koncentratorima obavijesti, možete jednostavno poslati različite platforme, personalizirane automatske obavijesti, abstracting pojedinosti u sustavima različite platforme obavijesti (PNS). S jednom poziv API usmjeriti možete pojedinačnih korisnika ili cijeloj publici segmenti koje sadrže milijune korisnika, svim svojim uređajima.

Obavijesti koncentratora možete koristiti za enterprise i potrošača scenarijima. Ako, na primjer:

- Slanje obavijesti o novostima prijelom milijune s niske latencije (obavijesti koncentratora potencije aplikacija Bing unaprijed instalirana na svim uređajima Windows i Windows Phone).
- Slanje temeljena kupone korisnika segmenata.
- Slanje obavijesti o događajima korisnicima ili grupama za financije/Sportovi/igre aplikacije.
- Obavještavanje korisnika o enterprise događaje kao što su nove poruke/poruke e-pošte i potencijalne klijente prodaje.
- Pošaljite one-time-lozinke potreban za višestruke provjere autentičnosti.



##<a name="what-are-push-notifications"></a>Što su automatske obavijesti?

Pametni telefoni i tableti možete "obavijesti" korisnici kada došlo je do događaja. Ove obavijesti može potrajati nekoliko oblika.

Obavijesti u aplikacijama iz Windows trgovine i Windows Phone, može biti u obliku _skočnoj_: pojavit će se dijaloški prozor, sa zvukom, da biste signala obavijest o novoj. Druge vrste obavijesti koje su podržane uključiti obavijesti _pločica_, _neobrađenog_i _značke_ . Dodatne informacije o vrstama obavijesti podržane na uređaje sa sustavom Windows potražite u članku [pločice, tipki, i obavijestima](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx).

Na uređajima Apple iOS na automatske slično obavještava korisnika u dijaloškom okviru Traženje korisnika da biste pogledali ili zatvorite obavijesti. Kliknite **Prikaz** otvara u aplikaciji koja prima poruke. Dodatne informacije o iOS obavijesti potražite u članku [iOS obavijesti](http://go.microsoft.com/fwlink/?LinkId=615245).

Automatske obavijesti pomoći za mobilne uređaje na prikaz Osvježi podatke prilikom preostalo energiju učinkovitog. Obavijesti mogu se poslati pozadinskog sustava za mobilne uređaje čak i kad odgovarajuće aplikacije na uređaju nisu aktivna. Automatske obavijesti su tehnika komponenta za aplikacije za korisničke kojima se koriste da biste povećali radnje aplikacije i korištenje. Obavijesti su korisne za korporacije i kada najnovije informacije povećava odziv zaposlenika tvrtke događajima.

Primjeri određene mobilne radnje Scenariji su:

1.  Ažuriranje pločica u sustavu Windows 8 ili Windows Phone s trenutnom financijske podatke.
2.  Upozorenjem korisnik s skočnoj koji neke radnu stavku vam je dodijeljen za tog korisnika, u aplikaciji enterprise utemeljen na tijeka rada.
3.  Prikaz iskaznice s brojem trenutnu prodaju potencijalnih klijenata u aplikaciji CRM (kao što je Microsoft Dynamics CRM).

##<a name="how-push-notifications-work"></a>Kako automatske obavijesti rad

Automatske obavijesti isporučuju se kroz ovisne infrastructures naziva _Sustavi obavijesti platformu_ (PNS). Na PNS nudi funkcije barebones (to jest, nema podrške za emitiranje, personalizacija) i imaju nema uobičajenih sučelja. Na primjer, da biste poslali obavijest aplikacija iz Windows trgovine, razvojni inženjer morate se obratiti WNS (servis obavijesti za Windows). Da biste poslali obavijest na uređaju sa sustavom iOS, iste programiranje ima kontakt APN-ove (Apple automatske obavijesti servis), a drugi vrijeme slanja poruke. Azure obavijesti koncentratora pomoći unosom uobičajenih sučelje, zajedno s drugih značajki za podršku automatske obavijesti preko svake platforme.

Visoke razine, no svi sustavi obavijesti platformu slijede isti uzorak:

1.  Aplikacije klijenta kontakta PNS dohvatiti njegov _rukovati_. Vrsta ručicu ovisi o sustavu. WNS, to je URI ili "obavijest o kanal." Za APN-ove, je token.
2.  Aplikacije klijenta pohranjuje tom pokazivaču aplikacije _pozadinskih_ za kasnije korištenje. Za WNS, pozadinske obično je servis u oblaku. Za Apple, sustav zove _davatelja usluga_.
3.  Da biste poslali automatske obavijesti, aplikacija pozadinske kontakta PNS Upotreba ručice za ciljnu aplikaciju instancu komponente određenog klijenta.
4.  Na PNS prosljeđuje obavijesti na uređaju naveo ručicu.

![][0]

##<a name="the-challenges-of-push-notifications"></a>Izazove automatske obavijesti

Dok su te sustavima vrlo moćne, oni i dalje ostavite baš radili na proizvođač da biste implementirali čak uobičajeni scenariji automatske obavijesti, kao što je emitiranje ili slanje automatskih obavijesti segmentiranog korisnicima.

Automatske obavijesti se jedna od najčešće tražene značajki na servise u oblaku za mobilne aplikacije. Razlog za to da je potrebne da bi funkcionirali infrastrukture prilično složene i uglavnom nepovezanih glavni poslovne logike aplikacije. Neke od izazove za izgradnju infrastruktura za automatske osvježavati su:

- **Ovisnost platforme.** Da biste slali obavijesti uređaji za različite platforme, više sučelja mora biti zadan u pozadinske. Ne samo razlikuju najniže razine detalja, ali prezentacije obavijesti (pločice, skočnoj ili iskaznice) ujedno platformu zavisne. Te razlike može dovesti do složenih i teško održava pozadinske koda.

- **Promjena veličine.** Skaliranje ovaj infrastrukture sastoji se od dva aspekata:
    + Po smjernice PNS tokeni uređaj mora osvježiti prilikom svakog pokretanja aplikacije. To potencijalnih klijenata za veliku količinu promet (i pristupa consequent baze podataka) da biste jednostavno ažurirati tokeni uređaja. Kada je broj uređaja prijeđe (vjerojatno milijune), nonnegligible je trošak stvaranja i održavanje ovaj infrastrukture.

    + Većina PNSs ne podržavaju emitiranje na više uređaja. Slijedi emitiranje milijune uređaji rezultira milijune pozive u PNSs. Mogućnosti za promjenu veličine te zahtjeve je nontrivial, jer obično razvojnim inženjerima aplikaciju želite zadržati Ukupna Latencija prema dolje. Na primjer, zadnji uređaj poruka trebale primiti obavijest 30 minuta nakon poslao obavijesti, kao mnogim slučajevima bi defeat namjenu da bi automatske obavijesti.
- **Usmjeravanje.** PNSs omogućuje slanje poruke s uređajem. Obavijesti u aplikacijama za većinu no su ciljani na korisnika i/ili kamata grupe (na primjer, sve zaposlenike dodijeljene određenih klijentova računa). Kao takve, da bi se usmjeravanje obavijesti u odgovarajuće uređaja, aplikacija pozadinske moraju podržavati registra koji se pridružuje kamata grupe tokeni uređaja. U ovom indirektni dodaje Ukupno vrijeme tržištu i održavanje troškovima aplikacije.

##<a name="why-use-notification-hubs"></a>Zašto koristiti koncentratora obavijesti?

Obavijest o koncentratora uklonili složenosti: nemate da biste upravljali izazove automatske obavijesti. Umjesto toga možete koristiti koncentratora za obavijesti. Obavijesti koncentratora pomoću obavijesti infrastruktura za potpuni multiplatform, skalirana izlaz automatske i znatno smanjiti specifične za automatske kod koji se izvodi u pozadinska aplikacija. Obavijest o koncentratora implementirati funkcionalnost automatske infrastrukture. Uređaji odgovorni samo za registriranje PNS ručice i pozadinski je zadužen za slanje poruka platformu neovisno korisnicima ili grupama kamatu, kao što je prikazano na sljedećoj slici:

![][1]


Obavijest o koncentratora pružaju infrastruktura za obavijesti automatske spremni za korištenje s sljedeće prednosti:

- **Više platformama.**
    +  Podrška za glavne mobilnog platformama. Obavijest o koncentratora možete poslati automatske obavijesti aplikacije iz Windows trgovine, iOS, Android i Windows Phone.

    +  Obavijest o koncentratora navedite uobičajenih sučelje za slanje obavijesti za sve podržane platforme. Ovisne protokoli nisu potrebni. Aplikacija pozadinske možete poslati obavijesti u oblicima ovisne ili platformu neovisno. Aplikacija samo komunicira s koncentratorima obavijesti.

    +  Upravljanje uređajima ručicu. Obavijest o koncentratora održava ručicu registra i povratne informacije iz PNSs.

- **Radi s bilo kojeg pozadinske**: Oblaku ili na lokalnim poslužiteljima, .NET, PHP, Java, čvor, itd.

- **Promjena veličine.** Prilagodi koncentratora obavijesti milijune uređaja bez potrebe za mijenjanje arhitekture ili shard.


- **Obogaćeni skup isporuke uzorke**:

    - *Emitiranje*: omogućuje blizu-istodobno emitiranje milijune uređaja s jednom API poziva.

    - *Jednosmjernog/višesmjernog*: automatske oznake koji predstavlja pojedinačnih korisnika, uključujući sve svoje uređaje ili širem grupu; na primjer, zasebnom obrascu čimbenika (tablet nasuprot telefona).

    - *Segmente*: automatske složene segment definira oznaka izraza (na primjer, uređaji u New Yorku praćenja u Yankees).

    Svakom uređaju, prilikom slanja njen držač koncentratoru obavijesti, možete odrediti jednu ili više _oznaka_. Dodatne informacije o [oznake]. Oznaka ne morate unaprijed dodjeli ili odbačen. Oznake omogućuju jednostavan način za slanje obavijesti o korisnicima ili grupama kamata. Budući da oznake mogu sadržavati nijedan identifikator specifične za aplikacije (kao što je korisnik ili grupa ID-a), njihova upotreba oslobađa aplikacije pozadinske iz teret pamtiti za pohranu i upravljanje ručice za uređaj.

- **Personalizacija**: svakom uređaju može imati jedan ili više predložaka, da biste postigli lokalizaciju – uređaja i personalizacija bez utjecaja pozadinske kod.

- **Sigurnost**: zajednički pristup tajna (SAS) ili pridruženim provjere autentičnosti.

- **Obogaćeni telemetrijskih**: dostupna na portalu i programski.


##<a name="integration-with-app-service-mobile-apps"></a>Integracija s programom aplikacije servisa za mobilne aplikacije

Da biste olakšali objedinjenog i objedinjujućih iskustvo putem servisa Azure, [Aplikacija mobilne usluge] ima ugrađenu podršku za slanje obavijesti pomoću koncentratora obavijesti. [Mobilna aplikacija servisa] nudi platforme za razvoj iznimno prilagodljivi, globalno dostupne mobilne aplikacije za Enterprise razvojnim inženjerima i Integrators sustav koji objedinjuje bogatog skupa mogućnosti mobilne programerima.

Razvojni inženjeri mobilne aplikacije mogu koristiti koncentratora obavijesti s tijeka rada za sljedeće:

1. Dohvaćanje rukovatelj PNS uređaja
2. Registracija uređaja i [predloške] s koncentratorima obavijesti putem praktičan mobilne aplikacije klijenta SDK register API-JA
    + Imajte na umu da mobilne aplikacije pruge Odsutan sve oznake na registracije sigurnosnih razloga. Rad s koncentratorima obavijesti iz svoje pozadine izravno želite pridružiti oznake na uređajima.
3. Slanje obavijesti iz aplikacije pozadinskog sustava s obavijesti koncentratora

Evo nekih conveniences unese programerima Ta integracija:

- **Mobilne aplikacije klijenta SDK-ovi.** Ove više platform SDK-ovi navedite jednostavne API-ji za registraciju i razgovarati s središtu obavijesti pomoću aplikacije za mobilne uređaje automatski povezana. Razvojni inženjeri ne trebate istražujte putem koncentratora obavijesti vjerodajnice i rad s dodatnim servisa.
    + U SDK-ovi automatsko označavanje zadani uređaj s mobilne aplikacije čija je autentičnost provjerena korisnički ID da biste omogućili slanje da biste Korisnički scenarij.
    + U SDK-ovi automatski pomoću ID instalacije mobilne aplikacije kao GUID morate registrirati koncentratora obavijesti, spremanje razvojnim inženjerima imate li problema prilikom održavanja više GUID servisa.
    
- **Instalacija model.** Mobilne aplikacije funkcionira s obavijesti koncentratora najnovije automatske model za predstavljanje sve automatske svojstva povezana s uređaja u instalaciji JSON poravnava sa servisima automatske obavijesti i jednostavan.

- **Fleksibilnost.** Razvojni inženjeri uvijek možete raditi izravno koncentratora obavijesti čak i uz integraciju sa servisom na mjestu.

- **Integrirano sučelje [Azure]portalu.** Automatske mogućnost predstavlja vizualno u mobilne aplikacije i razvojni inženjeri mogli lako raditi središtu pridružene obavijesti putem mobilne aplikacije.



##<a name="next-steps"></a>Daljnji koraci

Možete saznati više o koncentratora obavijesti u ovim temama:

+ **[Kako korisnici koriste koncentratora obavijesti]**

+ **[Vodiči za koncentratora obavijesti i vodilice]**

+ **Vodiči za obavijesti koncentratora početak rada** ([iOS], [Android], [univerzalno Windows], [u sustavu Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

Odgovarajući .NET upravljani API reference za slanje obavijesti nalaze se ovdje:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [Kako korisnici koriste koncentratora obavijesti]: http://azure.microsoft.com/services/notification-hubs
  [Vodiči za koncentratora obavijesti i vodilice]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [Univerzalni za Windows]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [Aplikacije servisa za mobilne aplikacije]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [Predlošci]: notification-hubs-templates-cross-platform-push-messages.md
  [Portal za Azure]: https://portal.azure.com
  [oznaka]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
