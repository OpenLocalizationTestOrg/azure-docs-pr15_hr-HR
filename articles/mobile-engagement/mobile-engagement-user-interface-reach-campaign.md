<properties 
   pageTitle="Azure mobilne radnje korisničko sučelje - kampanje razgovor" 
   description="Laern način za stvaranje i upravljanje automatske obavijesti kampanje pomoću Azure Mobile radnje" 
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


# <a name="how-to-create-and-manage-push-notification-campaigns"></a>Kako stvoriti i upravljanje kampanje automatske obavijesti
U odjeljku razgovor korisničkog sučelja možete koristiti za stvaranje nove kampanje automatske složene formule unosom sve podatke koje ćete morati poslati obavijest automatske. Mogućnosti automatskih kampanje malo razlikovati ovisno o vrsti četiri kampanje: objave, ankete, ih gura podataka i pločice (samo za Windows Phone).

### <a name="option-applies-to"></a>Mogućnost odnosi se na:
- Jezici: Sve (objave, ankete, podataka ih gura, pločice)
- Kampanja: Sve (objave, ankete, podataka ih gura, pločice)
- Obavijest: Objave, ankete
- Sadržaj: Jedinstvena za svaku vrstu kampanje
- Ciljna publika: Sve (objave, ankete, podataka ih gura, pločice)
- Vremensko razdoblje: objave, ankete, pločica
- Test: Sve (objave, ankete, podataka ih gura, pločice)
 
![Razgovor Campaign1][20]

## <a name="languages"></a>Jezici
Padajući izbornik jezika možete koristiti da biste poslali drugu verziju sustava automatske uređajima koji su postavljeni za korištenje različitih jezika. Po zadanom sve uređaje primit će isti automatske bez obzira na nekom od sljedećih jezika postavili za korištenje. Korisnici s svoj uređaj postavljen na nekom drugom jeziku primit će verziju zadani jezik za slanje. Mnoge od kampanje mogućnosti automatske omogućuju da biste odredili zamjenski sadržaja za svaku dodatne jezike koje odaberete. 
 
![Razgovor Campaign2][21]

### <a name="language-differences-apply-to"></a>Primjena jezik razlike:
- Jezici: Jedinstveni jezike mogu odabrane uz zadani jezik
- Kampanja: Ista za sve jezike
- Obavijest: Jedinstvena za svaki jezik uz zadani jezik
- Sadržaj: Jedinstvena za svaki jezik uz zadani jezik
- Ciljna publika: Koje se mogu filtrirati prema odvojene jezične kriterij
- Vremensko razdoblje: isti za sve jezike
- Test: Može biti poslan svaki jezik odjednom
 
### <a name="supported-languages"></a>Podržani jezici:
- Arapski (Očisti) 
- Bugarske abecede (bg) 
- Katalonski (ca) 
- Kineski (zh) 
- Hrvatski (hr) 
- Czech (CS) 
- Danski (da) 
- Nizozemski (servisu) 
- Engleski (en) 
- Finski (fi) 
- Francuski (fr) 
- Njemački (de) 
- Grčki (el) 
- Hebrejski (on) 
- Hindski (najviša) 
- Mađarskom (hu) 
- Indonezijski (id) 
- Talijanski (ga) 
- Japanski (ja) 
- Korejski (ko) 
- Latvijski (lv) 
- Litvanski (lt) 
- Malajski (macrolanguage) (ms) 
- Norveški (bokmal) (nb) 
- Poljski (pl) 
- Portugalski (pt) 
- Rumunjski (retke) 
- Ruski (pravi) 
- Srpski (sr) 
- Slovačkom (sk) 
- Slovenskog (sl) 
- Španjolski (es) 
- Švedski (sv) 
- Tagalog (TT) 
- Tajlandski (ti) 
- Turski (tr) 
- Ukrajinskom (Velika Britanija) 
- Vijetnamski (smjer) 
 
## <a name="campaign"></a>Kampanje
U odjeljku kampanje možete koristiti da biste postavili naziva i kategorije vaše kampanje kao Ako planirate da biste zanemarili odjeljku publike automatske kampanje i pošaljite kampanje putem API dosegne (i neke elemente s nisku razinu automatske API-JA). Kategorije može se koristiti s predloškom prilagođenog obavijesti na kontrolu u aplikaciji obavijesti koji se temelji na unaprijed definirane postavke. Možete dobiti popis vaše postojeće "Kategorija" putem API dosegne.

> Upozorenje: Ako koristite mogućnost "Publike Zanemari automatske poslat će korisnicima putem na API-JA" u odjeljku "Kampanje" s kampanjom razgovor kampanje će automatski poslati, morat ćete ručno slanje putem API dosegne.
 
![Razgovor Campaign3][22]
 
### <a name="option-applies-to"></a>Mogućnost odnosi se na:
- Naziv: sve
- Kategorija: Objave, ankete
- Zanemarivanje ciljnu skupinu automatske poslat će korisnicima putem API Sučelja: sve
 
## <a name="notification"></a>Obavijesti
U odjeljku obavijesti možete koristiti za postavljanje osnovne postavke za automatske uključujući: naslov za slanje, poruke, a zatim sliku u aplikaciji ili ako je dismissible. Mnoge postavke obavijesti su specifične za platformu uređaj. Možete odabrati je li vaša automatske poslat će se "u aplikaciji" ili "iz aplikacije" ili i jedno i drugo. (Imajte na umu da korisnici mogu "slaganja" ili "onemogućivanju" od "iz aplikacije" ih gura na operacijski sustav razine uređajima i Azure Mobile radnje nećete moći nadjačati tu postavku. Imajte na umu i da API dosegne rukuje "u aplikaciji" i "iz aplikacije" ih gura. Automatske API može se koristiti to učiniti "iz aplikacije" ih gura.) Ih gura moguće je prilagoditi sa slikama ili HTML sadržaj, uključujući precizno veze za povezivanje izvan aplikacije ili na drugo mjesto u aplikaciji (Android SDK 2.1.0 ili noviji svrhe kategorije obavezno). Možete promijeniti ikonu ili iOS iskaznice i slanje teksta ili web-sadržaja (izbornik s html sadržaja, URL vezu na drugo mjesto unutar ili izvan aplikacije). Možete učiniti Nazovi uređaje sa sustavom Android ili vibracija s na automatske. (Imajte na umu da ćete ispravno datoteku preusmjeravaju ili vibracija uređaj manifesta SDK dozvole u uređaj sa sustavom Android.) Trenutno nema industrijskih standardne za Android "širu sliku", jer veličine zaslona razlikuju se na svim uređajima, ali 400 x 100 slike raditi na gotovo bilo koje veličine zaslona.

### <a name="delivery-types"></a>Vrste isporuke:
-    Iz aplikacije samo: isporuku će se obavijest kada korisnik koristiti aplikaciju.
- Izlaz samo obavijesti aplikacije potreban je certifikat stablo jabuke ili Google (APN-ove ili GCM certifikat).
- U samoj aplikaciji samo: obavijesti pojavit će se samo kada je pokrenut aplikacije.
- Obavijesti koristi sustav isporuke Capptain dosegne korisnika. Potpuno možete prilagoditi vizualni izgled/prikaz vaše automatske.
- Bilo kojem trenutku: Tu mogućnost osigurava pošaljete obavijest neki program radi ili ne.

 
![Razgovor Campaign4][23]

### <a name="option-applies-to"></a>Mogućnost odnosi se na:
- Obavijest: Objave, ankete
 
## <a name="content"></a>Sadržaj
U odjeljku sadržaja možete koristiti da biste izmijenili sadržaj vaše objave, ankete, ih gura podataka i pločice (samo za Windows Phone). Postavku sadržaja automatske kampanja odgovara vrsti kampanje. 

### <a name="see-also"></a>Vidi također
- [Dokumentacija UI - dosegne – automatske sadržaja][Link 29]
 
![Razgovor Campaign5][24]

## <a name="audience"></a>Ciljne skupine
U odjeljku publici možete koristiti da biste definirali standardni popis stavki da biste ograničili kampanje ili ograničenja kampanje na temelju kriterija prilagođene. Standardni skup mogućnosti da biste ograničili publici omogućuje slanje nove i stare ili samo korisnicima nativni automatske. Možete postaviti i ograničenja da biste ograničili broj korisnici koji dobiju na automatske. Možete ručno urediti izraz za filtriran kampanje da biste dodali jednu ili više kriterija ciljne korisnike. Možete ručno upišite izraz publici. Takve izraz morate izričito definirati odnos između kriterija. Kriterij je opisan identifikatoru koji moraju započeti znakom veliko slovo i ne smiju sadržavati razmake. Odnos između kriterij se može opisati pomoću "i", "ili", "ne" operatora kao i "(",")". Primjer: "Criterion1 ili (Criterion1 i ne Criterion2)".

> Napomena: S veću publiku obuhvaćeno kampanje, na strani poslužitelja ciljanja pregleda može biti spor, osobito ako pokušate pokrenuti više kampanje u isto vrijeme.

- Ako je to moguće, samo započnite kampanje.
- Od najviše samo započnite četiri kampanja.
- Samo na aktivni korisnici (potvrdni okvir "sudjelovati samo korisnici koji mogu pristupiti pomoću Nativnog automatske" i "Sudjelovati samo aktivni korisnici") tako da samo korisnici koji i dalje imati instaliranu aplikaciju i koristiti ga trebaju koje treba pretraživati.
Kada je definiran publici, gumb simulate možete koristiti da biste saznali koliko korisnicima prikazat će se ovaj pribadače. To će izračunati broj poznati korisnika potencijalno ciljani po te ciljne skupine (to je procjena na temelju uzorka slučajni korisnika). Imajte na umu da korisnici koji ste deinstalirali aplikaciju su dio te ciljne skupine, ali ne može otvoriti.

### <a name="see-also"></a>Vidi također
- [Korisničko Sučelje dokumentaciju - razgovor – novi automatske kriterij][Link 28]

![Razgovor Campaign6][25]

### <a name="edit-expression"></a>Uređivanje izraza
![Razgovor Campaign7][26]
 
### <a name="limit-your-audience-option-applies-to"></a>Ograničenje mogućnosti publici odnosi se na:
- Sudjelovati samo podskup korisnika: sve (objave, ankete, ih gura podataka, pločice)
- Sudjelovati samo stari korisnici: sve (objave, ankete, ih gura podataka, pločice)
- Sudjelovati samo nove korisnike: sve (objave, ankete, ih gura podataka, pločice)
- Sudjelovati samo neaktivne korisnike: objave, ankete, pločica
- Sudjelovati samo aktivni korisnici: sve (objave, ankete, ih gura podataka, pločice)
- Sudjelovati samo korisnici koji mogu pristupiti pomoću Nativnog automatske: objave, ankete
 
## <a name="time-frame"></a>Vremensko razdoblje
U odjeljku vremenski okvir možete koristiti da biste postavili kada poslat će se na automatske ili vremenskog okvira možete ostaviti praznim da biste pokrenuli kampanje odmah. Imajte na umu da pomoću krajnjih korisnika vremensku zonu možda pokretanja kampanje starije od očekivati vaše krajnjim korisnicima u Azije i slanje manjim grupama od ih gura u trenutku dok sve vremenske zone u svijetu odgovaraju vremenski okvir Postavljanje kampanje dan. Pomoću vremensku zonu krajnji korisnici mogu i zbog kašnjenja u kampanje jer je da bi se zatražila vrijeme s telefona prije pokretanja na automatsko prosljeđivanje.

> Napomena: Kampanje bez završni datum možete predmemoriju ih gura lokalno i još uvijek ih prikazati nakon što ručno dovršeno kampanja. Da biste izbjegli taj problem, određene vrijeme završetka kampanje.

### <a name="see-also"></a>Vidi također
- [Dosegne – kako Tos – planiranje][Link 3] 
 
![Razgovor Campaign8][27]

### <a name="settings-apply-to"></a>Primjena postavki:
- Vremensko razdoblje: objave, ankete, pločica
 
## <a name="test"></a>Test
U odjeljku Test možete koristiti da biste poslali ovaj automatske test uređaj prije spremanja kampanje. Ako ste konfigurirali sve prilagođene jezike za kampanje, možete testirati u automatske u svaki jezik. Možete postaviti test uređaj onemogućili "Moj račun".
> Napomena: Nema strani poslužitelja podataka prijavljen je kada koristite gumb za "testiranje" ih gura, podataka prijavljen je samo za kampanje realni automatske.

### <a name="see-also"></a>Vidi također
- [Dokumentacija UI - moj račun][Link 14]
 
![Razgovor Campaign9][28]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
