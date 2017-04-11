<properties 
   pageTitle="Azure mobilne radnje korisničko sučelje - razgovor kako"
   description="Korisničko sučelje pregled za Azure mobilne radnje" 
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

# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a>Kako započeti korištenje i upravljanje njima ih gura da biste stupili krajnjim korisnicima

Kada u SDK potpuno je integriran u aplikacije, što možete počnete koristiti u odjeljku razgovor korisničkog Sučelja za slanje obavijesti korisnicima aplikacije.  

## <a name="do-your-first-push-notification-campaign"></a>Učinite prva kampanja automatske obavijesti
-    Provjerite je li vaš razgovor integriran u aplikacije s SDK-a. 
-    Odabir aplikacije
 
![First1][1]

-    Idite na odjeljak "Razgovor" i kliknite "Nova objava"
 
![First2][2]

-    Stvaranje nove kampanje i nazovite ih
 
 ![First3][3]

-    Odaberite kako obavijesti moraju biti isporučeni, kao u samoj aplikaciji samo
 
![First4][4]

-    Stvorite poruku koju želite automatske
 
![First5][5]

-    Možda pišete naslov na obavijesti (neobavezno).
-    Pisanje automatske sadržaj poruke.
-    Možete prenijeti sliku. Imajte na umu da veličina datoteke ne može prelaziti 32.768 bajtova.
-    Imate mogućnost da biste odabrali dodatne mogućnosti, no fokus je ovog praktičnog vodiča, ne možemo će potražite u članku koji kasnije.

-    Odaberite vrstu sadržaja samo kao obavijest
 
![First6][6]

-    Stvaranje automatske kampanje i pojavit će se na popisu kampanje.
 
![First7][7]

## <a name="test-your-push-notification-campaign"></a>Testiranje kampanje automatske obavijesti
![Ispit1][8]

-    Registrirajte uređaj.
-    Kliknite okvir uređaj koji želite automatske.
-    Kliknite gumb za "Testiranje" da biste poslali na automatske na uređaju.
 
![Ispit2][9]

-    Aktiviranje kampanje
 
![Test3][10]

-    Sad kad ste stvorili kampanje samo želite aktivirati za obavijesti da bi se pomiču svojim korisnicima.
 
## <a name="send-personalized-pushes"></a>Slanje personalizirane ih gura
-    U ovom se primjeru stvara automatske gdje popusta prilagođeni kod unesu automatske obavijesti.
 
![Personalize1][11]

Personalizacija funkcionira jer zamjenjuju oznake tako da iz oznaku informacije aplikacije pa, morat ćete provjerite ima li korisnik odgovarajuće aplikacije-informacije definirani najprije. U ovom primjeru ciljano korisnici će imati oznaku informacije aplikacije koja se naziva rebate_code definirane.
Kada ugledate iznad sadržaj automatske obavijesti sadrži oznake ${rebate_code} koji će vas upozoriti da je će biti zamijenjen njegov sadržaj oznake informacije aplikacije.

> Upozorenje: Ako informacije oznaka aplikacija nije definiran za korisnika, korisnik neće primiti u automatske.

-    Rezultat
 
![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a>Možete dodatno prilagoditi tekst na obavijesti
![Personalize3][13]

-    Naslov obavijesti, uključujući
-    I sadržaj poruke.
-    Odaberite vrstu objava (prikazu teksta ili web-prikaz)
 
![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a>U tijelu obavijest također mogu prilagoditi s:
-    URL akcije treba koji želite prilagoditi odredišna stranica
-    Naslov
-    Tijelo poruke.
 
 
## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>Razlikovanje vaše automatske obavijesti (u ili iz aplikacije)
-    Odaberite vrstu obavijesti ćete automatske, odaberite svoju aplikaciju, prijeđite na odjeljak "Do", odaberite ili stvaranje automatske kampanje i prijeđite na odjeljak "Obavijest".
 
-    Kliknite na "Način isporuke".
-    Kliknite potvrdni okvir "Ograničiti aktivnosti" kada želite da se obavijest pojavljuje se na određene aktivnosti (zaslonima).

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>"Iz aplikacije samo" Način isporuke
![Differentiate2][16]

"Iz aplikacije samo" Način isporuke omogućuje automatsko prosljeđivanje obavijesti prilikom zatvaranja. To je standardni automatske obavijesti.
Kad odaberete "iz aplikacije samo", potrebno je već navesti certifikata iz platformu za svoju aplikaciju sastavlja na (APN-ove ili GCM).

### <a name="see-also"></a>Vidi također
-  [Apple automatske obavijesti servisa – certifikata](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), Google Cloud porukama – certifikat] (http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"u samoj aplikaciji samo" Način isporuke
![Differentiate3][17]

Način isporuke "U samoj aplikaciji samo" omogućuje automatsko prosljeđivanje obavijesti kada je pokrenut aplikacije.
Za ovu obavijest, ne morate proći kroz APN-ove i GCM sustava.
U aplikaciji sustava isporuke možete koristiti dosegne vaše krajnjim korisnicima.
Potpuno možete prilagoditi obavijesti i odlučiti u kojem će se pojaviti aktivnosti (zaslon) obavijesti.

### <a name="anytime-delivery-mode"></a>"Bilo. kojem trenutku" Način isporuke
Možete odabrati do "Bilo. kojem trenutku" Način isporuke, osigurava da dođete do vaše krajnjeg korisnika li aplikacija instalirana ili nije.
Kad odaberete "Bilo. kojem trenutku", morate već niste certifikata iz platformu za svoju aplikaciju sastavlja nakon (APN-ove ili GCM). 
 
## <a name="schedule-a-push-campaign"></a>Raspored s kampanjom pribadače
### <a name="plan-to-start-a-campaign"></a>Planiranje da biste započeli s kampanjom
![Shedule1][18]

Je ožujak na 21st od i imate obavijest da bi i planirane za u 22nd od ožujka od ponoći. Nemate Ostanite ispred sučelja učiniti s automatske! Planiranje unaprijed točno minute poslat će se obavijesti.
-    Poništite potvrdni okvir "Nema" potvrdni okvir, a zatim odaberite vrijeme početka 
-    Odaberite datum i vrijeme želite započeti s kampanjom automatske.

### <a name="plan-to-end-a-campaign"></a>Planiranje da biste završili s kampanjom
![Shedule2][19]

Želite da vaše kampanje da biste prestali na na 25 od ožujka na 3.00 pm, ali znate da nećete li to učiniti.
Nemate Ostanite ispred sučelje za automatske! Planiranje unaprijed točno minute kampanje će se zaustaviti.
-    Kliknite na "Nema" potvrdni okvir ili odaberite vrijeme završetka
-    Odaberite datum i vrijeme želite završiti kampanje automatske.

### <a name="end-a-campaign-manually"></a>Ručno završili s kampanjom
![Shedule3][20]

Prema zadanim postavkama, "Nema" potvrdne okvire su potvrđeni.
To znači da će početi kampanje čim ga aktivirati u odjeljku razgovor i dogodit će se ako će prestanete odjeljak razgovor.
 
> Napomena: Kampanje stvorena bez završni datum lokalnu pohranu poruka s automatske na uređaju i prikaz rezultata prilikom sljedećeg otvaranja aplikacije čak i ako kampanje ručno je završen.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Poboljšanje automatske obavijesti s prikazom teksta
### <a name="what-is-a-text-view"></a>Što je prikaz teksta?
![TextView1][21]

Prikaz teksta je skočni prozor s tekstni sadržaj. Nakon krajnji korisnik ima nakon klika na automatske obavijesti, pojavljuje se ova skočni prozor.
Prikaz teksta omogućuje prikaz sadržaja na vašem krajnjeg korisnika. Ujedno prilike kako bi se prikazao poziva na akciju kao što su skoka na stranicu aplikacije, preusmjeravanja u trgovini, otvaranje web-stranice, slanje e-pošte, počevši pretraživanja zemlj lokalizirani itd...

### <a name="example-text-view"></a>Primjer: Prikaz teksta
-    U odjeljku "Razgovor" Stvaranje kampanje automatske obavijesti i kampanje dodijelite naziv
 
![TextView2][22]

-    Napišite poruku koja će se pojaviti na obavijesti.
-    Odaberite vrstu sadržaja objava "tekst"
 
![TextView3][23]

> Napomena: kada automatske prikaz teksta, uvijek dolazi s obavijesti najprije. 

- Definiranje tekst (nakon pojavljuju odabrana objava sadržaja tekst podređenu sekcije pojavit će se što vam omogućuje definiranje tekst koji želite da se prikazuje.)
 
![TextView4][24]

-    Upišite naslov koji će se pojaviti pri vrhu poruke.
-    Glavni sadržaj prikaza teksta za pisanje.
-    Pisanje sadržaja koji će se pojaviti na akcijski gumb (akcijskog gumba omogućuje aplikacija za određenu akciju kao što su otvorite stranicu aplikacije za preusmjeravanje App store ili bilo koju vrstu izvora možete unijeti).
-    Pisanje sadržaja koji će se pojaviti na gumbu izlaz (pritiskom na gumb Izlaz iz prikaza teksta će nestati.)
 
-    Stvaranje kampanje automatske obavijesti i pojavit će se na popis kampanja.
 
![TextView5][25]

-    Aktiviranje kampanje automatske obavijesti da biste poslali prikazu teksta svojim korisnicima.
 
![TextView6][26]

-    Rezultat
 
![TextView7][27]

-    Korisnik prima obavijest, a zatim kliknite ga.
-    Prikaz teksta se prikazuje kao skočni prozor dopuštanja korisnika za interakciju s njim.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Poboljšanje automatske obavijesti s web-tablice
### <a name="what-is-a-web-view"></a>Što je web-prikaz?
![WebView1][28]

Web-prikaz je skočni prozor s web-sadržaja. U ovom se skočni prozor se pojavljuje kada krajnji korisnik ima nakon klika na automatske obavijesti.
Web-prikaz omogućuje vam više interakciju s krajnji korisnik.
Ujedno prilika za predstavljanje poziva na akciju kao što je preusmjeravanje App Store, otvorite web-stranicu, slanje poruke e-pošte, pokretanje pretraživanja zemlj lokalizirani, itd...

### <a name="example-web-view"></a>Primjer: Web-tablice
-    Stvaranje automatske kampanje u odjeljku "Razgovor" i kampanje dodijelite naziv.
 
![WebView2][29]

-    Napišite poruku koja će se pojaviti na obavijesti.
-    Odaberite vrstu sadržaja na objavu kao "web"
 
![WebView3][30]

### <a name="about-announcement-types"></a>O vrstama objave:
- Samo obavijest: je jednostavno standardna obavijest. Što znači da ako korisnik klikne, pojavit će se bez dodatnog prikaza, ali će se izvršiti samo akcija koja je povezana s njom.
- Objava teksta: je obavijest o engages da korisnik ima susret prikaza teksta.
- Objava na webu: je obavijest o engages da korisnik ima potražite na web-prikaz.
Odabir sadržaja za "Objava Web".

> Napomena: Kada automatske web-prikaz, uvijek dolazi s obavijest najprije.

- Definiranje web-sadržaja (nakon pojavljuju odabrana objava sadržaja web na Podsekcija pojavit će se što vam omogućuje definiranje web-prikaz sadržaja koji želite prikazati.)

 
![WebView4][31]

-    Upišite naslov koji će se pojaviti pri vrhu poruke (neobavezno).
-    Ovdje unesite svoj HTML kod.
-    Kliknite gumb način prebacivanje edition i vidjeli kako izgleda za uređivanje izvora.
-    Pisanje sadržaja koji će se pojaviti na akcijski gumb (akcijskog gumba omogućuje aplikacija za određenu akciju kao što su otvorite stranicu aplikacije za preusmjeravanje u trgovini ili bilo koju vrstu izvora možete unijeti).
-    Pisanje sadržaja koji će se pojaviti na gumbu izlaz (pritiskom na gumb Izlaz iz web-tablice nestat će).
 
-    Rezultat
 
![WebView5][32]

-    Korisnik primanje obavijesti i kliknite je.
-    Prikaz teksta se prikazuje kao skočni prozor dopuštanja korisnika za interakciju s njim.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
