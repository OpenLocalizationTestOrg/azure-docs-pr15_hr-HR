<properties
   pageTitle="Azure mobilne radnje korisničko sučelje - Analitika"
   description="Saznajte kako analizirati povijesne podatke o aplikaciji pomoću Azure Mobile radnje"
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

# <a name="how-to-analyze-historical-data-about-your-application"></a>Analizi povijesne podatke o aplikaciji

U ovom se članku opisuju kartica **ANALIZE** portala za **Mobilne radnje** . Pomoću portala za **Mobilne radnje** za nadzor i upravljanje mobilne aplikacije. Imajte na umu da da biste počeli koristiti portal najprije morate stvoriti račun za **Azure Mobile radnje** .


Odjeljak analize korisničkog sučelja pruža Zbrojeno informacije o aplikaciji koji se temelji na povijesnim podacima koji se ažuriraju svaka 24 sata. Podaci se prikazuju na različitim nadzornih ploča sastoji od traka/redak/tortni grafikoni, rešetke i karte. Podaci se može preuzeti i kao .csv datoteke. Većinu tih isti podataka dostupna je u stvarnom vremenu u odjeljku Monitor korisničkog sučelja i također može pristupiti i iz API analize.

>[AZURE.NOTE] Broj sekcija portala za **Mobilne radnje** korisničkog Sučelja sadrže gumb **PRIKAŽI pomoć** . Pritisnite taj gumb da biste saznali više kontekstne o sekcije.

## <a name="standard-and-custom-analytics"></a>Standardnih ili prilagođenih Analytics

Azure Mobile radnje nudi skup osnovni, standardne analitičkih podataka o vaše aplikacije koje mogu biti prikazan na grafikonu čim aplikacije integrirati SDK-a. Azure radnje Mobile i omogućuje prikupljanje dodatnih prilagođenih analize željenih informacija o ponašanju krajnjih korisnika. To možete učiniti tako da stvorite plan oznaka prilagođena "oznaka (aplikacija informacije)", stvorene iz **Postavke** tako da se Azure Mobile radnje možete prikupiti ove dodatne podatke umjesto vas.



## <a name="analytics"></a>Analytics
- Nadzorna ploča: Prikazuje opće informacije o novim i aktiviraju korisnika i njihove trendova.
- Korisnici: Korisnici su otkrije njihove identifikator uređaja: ovaj identifikator je jedinstvena za svaki uređaj (jedan novi korisnik je zapravo jedan novi uređaj). Korisnik se smatra kao nove na određenom vremenskom intervalu ako on je izvršila svoj prvi sesiju ovaj vremenski interval. Korisnik se smatra kao zadržani ako on je izvršila najmanje jednu sesiju tijekom zadnjih sedam dana. Aktivni korisnici su korisnici koja je proizvela najmanje jednu sesiju tijekom dano razdoblje. Možete sortirati, tjedno, mjesečno dnevnu ili zaračunava vremenskih razdoblja. Svi grafikoni slične, no omogućuju Filtriraj po različite značajke, kao što su verziju aplikacije, a zatim Sortiraj po vremensko razdoblje. Standardne podatke prikupljene putem integracije SDK obuhvaća sljedeće: aktivni korisnici, novog korisnika, broj sesija, duljinu svaki sesiju, tehničke informacije o u državi, varijabli, mjesto, prijenosni jezik, uređaja, firmver, mreža (Wi-Fi veze), verzije aplikacije i SDK koristi klijentima. Ove informacije mogu se pregledati u stvarnom vremenu iz odjeljka monitor.

> Napomena: Vremensko razdoblje temelji se na datum iz postavke uređaja korisnika, tako da korisnika čije telefona sadrži datum i pogrešno prikazuju nije u redu vremensko razdoblje.

- Zadržavanja: Korisnik smatra kao što je zadržati na određenom vremenskom intervalu ako on je izvršila svoj prvi sesiju ovaj vremenski interval. Možete promijeniti vremenske intervale tijekom kojeg zadržava se (i korisnika novi) broje se u sate, dana, tjedana ili mjeseci. Pri vrhu cohorts ugrađena je zadržavanja analize korisnika. U cohort je skup nove korisnike za dano razdoblje (odnosno skup korisnika koji se izvodi njihove prvi sesije tijekom tog razdoblja). Koristimo cohorts dana 1, 2 dana, 4 dana, 7 dana ili 1 mjesec. Zadana je cohort, 1-svakodnevno, 2 dana, 4 dana, 7 dana ili 1 mjesec, Azure mobilne radnje formula izračunava skup svih korisnika koji pripadaju cohort i su i dalje aktivno (odnosno skup korisnika koji se izvode najmanje jednu sesiju tijekom razdoblja). Taj skup korisnika zove cohort verziju. (Azure radnje Mobile može vam prikazati koliko korisnika još uvijek koriste aplikacije, ali samo platformu određene spremište može vam reći koliko korisnika ste deinstalirali aplikaciju – na primjer, GooglePlay iTunes, iz Windows trgovine itd.).
- Sesije: Korištenje jedne aplikacije korisnik. Sesije generiraju slijed aktivnosti korisnicima (aktivnost obično je povezana za korištenje jedan zaslon aplikacije, ali to može se razlikovati ovisno o način SDK sadrži je integriran u aplikaciji). Korisnik možete izvršiti samo aktivnosti jedan po jedan: sesije započinje čim korisnik pokrene svoje prve aktivnosti i prestaje kada se dovrši on svoje zadnje aktivnosti. Ako korisnik ostaje više od nekoliko sekundi bez provedbe aktivnost, njegov nizu aktivnosti je podijeljen u dva različita sesije.
- Aktivnosti: Imena zaslonu u aplikaciji i duljinu put kada korisnici potrošiti na zaslonu. Aktivnosti su prilagođene analitički mogućnost koja će odgovarati oznake "aplikacije informacije" ste postavili za vlastite aplikacije:
- Put korisnika: Prikazuje kako će se korisnici kretati po aktivnosti vaše aplikacije (zaslonima). Možete premjestiti klizača prilagodite razinu detalja. Plavi čvorove predstavljaju aktivnosti vaše aplikacije. Veličinu proporcionalna vrijeme utrošeno korisnici u njoj. Bijeli čvorove predstavljaju sesiju početka i završetka. Crvena čvorove predstavljaju ruši. Veza predstavljaju prijelaze između aktivnosti vaše aplikacije (ili između aktivnosti i ruši). Kliknite na čvor ili vezu da biste prikazali zaslonski opis s dodatnim informacijama o vašim podacima: u vrijeme utrošeno na pojedinom zaslonu count prijelaza i postotak prijelaze iz izvora aktivnosti aktivnosti odredište. (S---60%---> B znači da korisnici koji se na aktivnosti odgovora vodi do aktivnosti B 60% od vremena.) Grafikonu možete reorganiziranje koliko ih želite pojašnjavaju položaj sprema se svaki put kada se promjene. Možete prikazati ili sakriti ruši da biste posvijetlili na grafikonu.
- Događaji: Određenih akcija za korisnika u aplikaciji. Distribucija događaja prikazuje se kao broj događaja po korisničku sesiju. Događaj predstavlja trenutno akciju, na primjer, klikom na gumb ili primanje obavijesti. (Značenje događaje ovisi kako je u aplikaciji integriran SDK.) Događaj se pojavljuju tijekom sesije ili posao ili može biti će se izdvajati samostalno.
- Zadaci: Slično kao na događaje osim istakli duljinu akciju. Ako, na primjer, poslovi nije vam reći tehničke informacije o koliko dugo traje sadržaja učitavanja ili poziv na web-servisa. Također može prikazati koliko dugo traje korisniku ispunjavanje obrasca, stvorite račun ili izvršiti. Posao predstavlja trajanja zadatka, na primjer, trajanje zadatka preuzimanja ili vrijeme na natpisu je prikazan na zaslonu. (Značenje zadataka ovisi kako je u aplikaciji integriran SDK.) Poslovi pridružuju obično pozadinske zadatke koji se izvode izvan opsega sesije (odnosno bez sve aktivnosti korisnika).
- Technicals: Tehničke informacije o uređaje korisnika aplikacije koje možete pratiti, kao što su regionalnu shemu, prijenosni, mreža, uređaj, opremu, i zaslona uređaja za korisnika i verzije aplikacije i verziju SDK koristiti u aplikaciji.
- Pogreške: Informacije o pogreškama u aplikaciji koje tehničke koji uzrokuju aplikacije da ne funkcionira. Pogreška predstavlja izravne problem, na primjer, mrežni problem ili loša rukovanje. (Značenje događaje ovisi kako je u aplikaciji integriran SDK.) Pojavila se pogreška može dogoditi tijekom sesije ili posao ili može biti će se izdvajati samostalno.
- Prekida: Informacije o pogreškama koje uzrokuju aplikacije da ne funkcionira. Neočekivanog je neočekivan Uvjet gdje aplikacija zaustavlja izvođenje njegove očekivanih funkcije i mora se zaustaviti. Neočekivanog obično se zbog pogreške u aplikaciji.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Pristup pregled zadržavanja
![Analytics3][12]

Pregled zadržavanja dolazi do oštećenja prema dolje u sredini u nekoliko kartice, svaka se prikazuje na pregled za određeno razdoblje zadržavanja. Razdoblje zadržavanja 2 dana je vidjeti u primjeru. S kartice prikaz razdoblja zadržavanja 4 dana i 7 dana.

## <a name="understanding-the-retention-overview-cards"></a>Objašnjenje kartice pregled zadržavanja
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Svaka se kartica sastoji se od 3 glavna dijela:
1. 1: u cohort i razdoblja koje se smatraju
2. 2 – 4: zadržavanja za trenutno razdoblje
3. 5: minigrafikone povijesti

### <a name="here-is-detailed-information-about-each-element"></a>Evo detaljne informacije o svaki element:
1.    Cohort i razdoblje: Ovo je naslov daje vrstu cohort. Ovdje "2 dana razdoblje", to znači da ne možemo će izgledati na ponašanje korisnika veće od 2 dana, korisnici koji stigli tijekom razdoblja od 2 dana i hoće li se povezuju u sljedeće blokovima 2 dana. Gornji primjer smatra aktivnosti korisnika između 21st i 22nd studenom.
2.    Tako ćete dobiti Stopa zadržavanja nad na 21 i 22 studenom za korisnike stiže u 19 i 20 studenom. U nastavku ćemo je prodao 1 aktivnih korisnika između 21st i 22nd, putem 3 koje su nove korisnike između 19th i 20th.
3.    U ovom vizualni indikator daje iste podatke kao prethodna grafički. (Treći kruga je od broja 33%) Boje pruža dodatne informacije: zeleni označava taj broj je rast iz prethodnog izračuna. Žuta znači stabilan i crvene boje znači silazno.
4.    To znači da se vrijednosti koje se koriste za izračun.
5.    Ovo je minigrafikone povijesti zadržavanja vrijednosti. Omogućuje vam da vidite vrijednosti u prošlosti da bi općenite prikaz kako razvile.


## <a name="see-also"></a>Vidi također

- [Koncepti][Link 6]
- [Vodič za servis za otklanjanje poteškoća][Link 24]

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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
