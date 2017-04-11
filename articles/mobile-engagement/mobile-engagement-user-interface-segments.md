<properties 
   pageTitle="Azure mobilne radnje korisničko sučelje - segmenata" 
   description="Saznajte kako stvarati i upravljati segmenata korisnika da biste odredili uzoraka korištenja pomoću Azure Mobile radnje" 
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

# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a>Kako stvoriti i upravljanje segmenata korisnika da biste odredili uzoraka korištenja

U ovom se članku opisuju kartici **SEGMENATA** portala za **Mobile radnje** . Pomoću portala za **Mobile radnje** za nadzor i upravljanje mobilne aplikacije.

U odjeljku segmenata korisničkog sučelja omogućuje rad na segmenting korisnika na temelju različito ponašanje i analitiku koji može pristupiti iz aplikacije i možete pristupiti i putem API segmenata. Segmenata se najprije izračunati 24 sata nakon stvaranja, a oni su recomputed svaka 24 sata koji se temelji na najnovije informacije analize. Kada se izračunava segment, "Day povijest dan" grafikon prikazuje svakog dana.


>[AZURE.NOTE] Broj sekcija portala za **Mobilne radnje** korisničkog Sučelja sadrže gumb **PRIKAŽI pomoć** . Pritisnite taj gumb da biste saznali više kontekstne o sekcije.

## <a name="create-segments"></a>Stvaranje segmenata
Možete stvoriti segmenta na temelju do 10 kriterija na određeno razdoblje gore do 60 dana u prošlosti iz odjeljka analize. Na primjer, možete stvoriti segmenta na temelju s osobama koje su prikazane određenim stranicama ili tražili određenog sadržaja unutar aplikacije u zadnjih 10 dana. Te su informacije dostupne u odjeljku analize. Tako, možete je koristite da biste stvorili segment i postavite automatske obavijesti ciljne ovaj podskup korisnika da biste ih vratite se u aplikaciju. 
 
> Napomena: Kada segment izračunat, ne može biti uređivati; možete samo ga klonirana (kopiranja) ili uništava (izbrisane). Segment može biti klonirana unutar iste aplikacije (s istom ID programa), a može biti klonirana i u druge aplikacije (s različitim ID programa). 
 
 ![segments1][35] 

## <a name="examples-segments"></a>Primjeri segmenata
 ![segments2][36]

Segmenata omogućuju fazi krajnji korisnici aplikacije.
Segmenting korisnike je važno marketinške strategije. Azure radnje Mobile omogućuje vam dohvaćanje povijesnim podataka i stvaranje prilagođenih segmenata. Alat za napredne omogućuje vam dodatne informacije o vašem klijentova u aplikaciji. Jednostavno možete analizirati segmente i koristiti segmente kao automatske ciljnih web-mjesta.
Uobičajena slučaja koristi je koju želite poslati na automatske obavijesti da biste potaknuli krajnji korisnici i ocjenjivanje aplikacija u trgovini. Umjesto slanja obavijesti sve svoje krajnjim korisnicima, možete stvoriti segment koji određuju samo korisnici koji ste koristili aplikacije svakodnevno za prošli mjesec i imao sjajno korisničko sučelje. Kada šaljete manje, Visoko ciljano automatske obavijesti, dobit ćete bolju povrat ulaganja.
 
 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a>Segmenata možete stvoriti na temelju glavne Azure Mobile radnje elemenata:
- Događaj: Stvaranje segment te ciljnih web-mjesta jedan određene događaja aplikacije u kojoj se dogodilo s više od dva tjedna. 
- Sesije: Stvaranje segmentu korisnika koje ste koristili aplikacije više od 5 puta prošli tjedan.
- Aktivnost: Stvaranje segmentu korisnika koje ste koristili jednu stranicu ili sadržaj više ili manje od 10 vremena prošli mjesec.
- Zadatka: Stvaranje segmentu korisnika koje ste dovršili zadatak dvaput više od jednog dana.
- Pad: Stvaranje segmentu svim korisnicima čiji je neočekivanog više od 10 puta prošli tjedan. (Ovog segmenta programa apology ili čak i kupon nije automatske!)
- Pogreška: Stvaranje segmentu svim korisnicima čiji je pogreška više od 100 puta zadnja tri dana.
- Informacije o aplikaciji: Stvaranje segmenta namijenjen prilagođene aplikacije informacije koje se događa tijekom posljednjeg dana 25.
 
 ![segments4][38]

Da biste koristili segmenta optimalnih način, morate nastale prilagođene Integracija SDK u aplikaciji s označavanju tarifom oznake "Aplikacije informacije".
Zatim, idite na početnu stranicu sučelja, odaberite aplikaciju i kliknite odjeljak "Segmenata".

1. Odaberite odjeljak "Segmenata".
2. Kliknite "novi segment" gumb da biste stvorili novi segment.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Primjer realni život: Stvaranje jednostavne segmenta na temelju informacija "Sesiju"
Stvaranje segmentu sve krajnjim korisnicima koji ste koristili aplikacije najmanje 50 vremena u prošli tjedan. Iz nje, pronađite samo s krajnjim korisnicima koji imaju potrošeno najmanje 30 sekundi u svojoj aplikaciji po sesiji. Prikazat će sve krajnjim korisnicima koji imaju sučelje za pozitivne u svojoj aplikaciji. Nakon toga segment koji je stvorio može se automatske obavijesti te krajnjim korisnicima da biste zatražite da ocijeniti vaše aplikacije u trgovini sustava.
 
 ![segments5][39]

1. Imenujte segment da biste ga brzo pronaći na popisu "Segmenta".
2. Kliknite gumb "Stvori".
 
 ![segments6][40]

Odaberite sesiju.
 
 ![segments7][41]

1. Odaberite razdoblje "Tjedan".
2. Kliknite Dalje.
 
 ![segments8][42]

1. Odaberite Operator koji određuje relevantnost između stavki na popisu: =; ≥, ≤.
2. Unesite broj koji želite.
3. Odaberite pojavljivanje koje želite. 
4. Kliknite Dalje.
U ovom se primjeru postavljen tako da koji putem prošli tjedan odgovaraju korisnika koje ste unijeli najmanje 50 sesije.
 
 ![segments9][43]

Za segmente sesiju možete odabrati duljine po sesiji kao kriterij.

1. Odabir operatora s popisa.
2. Unesite duljinu po sesiji.
3. Kliknite Dalje.
U ovom primjeru piše da putem sve sesije koje ste je segmentirajući odjeljak pojavljivanje, odaberite samo korisnici koji imaju potrošeno više od 30 sekundi po sesiji.
 
 ![segments10][44]

Naziv kriterij dohvaćanje u Dovršeno lijevak, a zatim kliknite Završi.
 
 ![segments11][45]

Kada ste završili s postavljanjem gore kriterij, pojavit će se u lijevak segmenta.
Budući da segment temelji se na analize podataka, segmenata izračunat će se jednom dnevno.
U ovom primjeru 47,7% od ukupne krajnjim korisnicima uparuje kriterij. Trebali bi korisnicima koji ste imali dobar sučelje i bit će vjerojatno omogućuju veću ocjena ako ih automatske obavijesti da biste zatražili da biste ocijenili aplikacije u trgovini sustava.


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
 
