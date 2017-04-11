<properties 
   pageTitle="Azure mobilne radnje korisničko sučelje - kriterija za razgovor" 
   description="Saznajte kako koristiti ciljnih kriterija da biste poslali automatske kampanja odaberite podskup korisnika pomoću Azure Mobile radnje" 
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


# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Upute za korištenje ciljnih kriterija da biste poslali automatske kampanja odaberite podskup korisnika

Birate prema određenim kriterijima s gumbom "Nove kriterije" jedan je od najnaprednijih koncepta u Azure Mobile radnje koji olakšava šaljete odgovarajući automatske obavijesti da kupci će odgovoriti umjesto Spamming svima. Možete ograničiti publici na temelju kriterija standardne i zamjenu ih gura da biste odredili koliko osoba će primiti obavijest.

**Vidi također:**

- [Korisničko Sučelje dokumentaciju - razgovor - nove automatske kampanje][Link 27]

## <a name="audience-criteria-can-include"></a>Možete uključiti publike kriterija:
- **Technicals:** Usmjeriti možete koji se temelji na istom tehničke informacije možete vidjeti u odjeljcima analize i Monitor. **Vidi također:** [Korisničko Sučelje dokumentaciju - Analitika] [ Link 15], [Dokumentaciju UI - monitora][Link 16]
- **Lokacije:** Aplikacije koje koriste "izvješća o stvarnom vremenu mjesto" s zemlj Fencing pomoću zemlj mjesto kao kriterij ciljnu skupinu od mjesta GPS. "Drži područje mjesto izvješćivanja" poziva se koristiti za ciljnu skupinu s mjesta na mobilnom telefonu ("stvarnom vremenu mjesto izvješćivanje o pogreškama" i "Drži područje mjesto izvješćivanje" morate aktivirati iz SDK-a). **Vidi također:** [Dokumentacija SDK - iOS – Integracija] [ Link 5], [SDK dokumentacija – Android – Integracija][Link 5]
- **Dosegne povratnih informacija:** Usmjeriti možete publici povratne informacije iz prethodne razgovor obavijesti putem razgovor povratnih informacija objave, ankete i ih gura podataka na temelju. To vam omogućuje bolji ciljno publici nakon dvije ili tri dosegne kampanje nego kada prvi put. Može se koristiti i za filtriranje korisnika koji već primili obavijest o s sličan sadržaj postavljanjem kampanje neće slati korisnicima koji su već primili određene prethodni kampanje. Možete čak i isključivanje korisnika koji su uključeni određene kampanje koja je još uvijek aktivan prima novi ih gura. **Vidi također:** [Dokumentacija UI - dosegne – automatske sadržaja][Link 29]
- **Instalacija praćenje:** Možete pratiti informacije koje ovise o korisnicima instaliranim aplikacije. **Vidi također:** [Dokumentacija UI - postavke][Link 20]
- **Korisničkog profila:** Usmjeriti na temelju standardni podaci o korisniku, a možete cilj na temelju informacije prilagođene aplikacije koje ste stvorili. To obuhvaća tko trenutno prijavljeni i korisnika koje ste ime odgovorili odgovore na pitanja vas ste ih da biste postavili u samoj aplikaciji umjesto samo kako ih reagiraju na prethodni kampanja. Sve informacije o aplikaciji definirane aplikacije prikazuju se na ovom popisu.
- Segmenata: Možete i cilj koji se temelji na koji ste stvorili na temelju određenog korisnika ponašanje koja sadrži više kriterija. Sve segmente definirali za aplikacije prikazuju se na ovaj popis. **Vidi također:** [Dokumentacija UI - segmenata][Link 18]
- **Informacije aplikacije:** Prilagođene oznake informacije aplikacije mogu se kreirati "Postavke" da biste pratili ponašanje korisnika. **Vidi također:** [Dokumentacija UI - postavke][Link 20]

## <a name="example"></a>Primjer: 
Ako želite da biste najava samo na podređene skup korisnika koji su izvesti akcije u aplikaciji za kupnju.

1. Idite na stranicu postavke aplikacije, odaberite izbornik "Aplikacije informacije" i odaberite "Informacije novu aplikaciju"
2. Registrirajte se nove informacije Booleova aplikacije pod nazivom "inAppPurchase"
3. Provjerite aplikacije postavljanje aplikacije informacije "TRUE" kada korisnik uspješno izvodi se u aplikaciji za kupnju (pomoću sendAppInfo ("inAppPurchase",...) – opis funkcije)
4. Ako ne želite da biste to učinili iz aplikacije, možete je učiniti iz svoje pozadine pomoću uređaja za API-JA)
5. Nakon toga samo potrebnih za stvaranje objava, s kriterijem ograničavanje publici korisnicima koji se pojavljuju "inAppPurchase" postavljeno na "true")
 
> Napomena: Ciljanja na temelju kriterija osim oznake informacije aplikacije potreban je Azure Mobile radnje prikupiti podatke s uređaja vaših korisnika prije slanja na automatske i tako može uzrokovati odgodu. Konfiguracija složene automatske mogućnosti (kao što je ažuriranje tipki) i može se usporiti ih gura. Pomoću "jedan snimka" kampanje iz API automatske je apsolutna najbrži način automatske u Azure Mobile radnje. Korištenje oznaka informacije samo aplikacije kao kriterij automatske kampanje razgovor (ili iz API dosegne ili korisničko Sučelje) je sa sljedećim korakom najbrže jer aplikacije informacije oznake pohranjuju se na strani poslužitelja. Pomoću drugih ciljnih kriterija za automatske kampanje je automatske metoda fleksibilna ali najmanju jer Azure Mobile radnje je poslati upit uređaje da biste poslali kampanje.
 
![Razgovor Criterion1][29] 

## <a name="criterion-options-apply-to"></a>Primjena kriterija mogućnosti:
- **Technicals**     
- Firmver naziv: naziv opreme
- Verzija opreme: verzija opreme
- Model uređaja: model uređaja
- Proizvođača uređaja: proizvođača uređaja
- Verzija aplikacije: verzija aplikacije
- Prijenosni naziv: naziv prijenosni Nedefinirana
- Prijenosni država: država prijenosni Nedefinirana
- Vrsta mreže: mreže vrsta
- Regionalne postavke: regionalne postavke
- Zaslon veličina: zaslona veličina
- **Mjesto**      
- Zadnja poznata područje: država, regija kraj
- Zemlj fencing stvarnom vremenu: popis od POIs (naziv, akcije), kružne POI (ime, zemljopisnu širinu, dužinu, polumjer u metar)
- **Dosegne povratnih informacija**     
- Objava povratnih informacija: objava, povratnih informacija
- Ankete povratnih informacija: ankete, povratnih informacija
- Ankete povratne informacije za odgovor: ankete odgovora povratne informacije, pitanje, a zatim odabir
- Povratne informacije za slanje podataka: podaci automatske, povratne informacije
- **Instalacija praćenja**     
- Spremište: Pohrana, Nedefinirana
- Izvor: Izvor Nedefinirana
- **Korisnički profil**     
- Spol: muško ili žena, Nedefinirana
- Datum rođenja: operatora, datum, Nedefinirana
- Prijava: true ili false, definirana
- **Informacije o aplikaciji**      
- Niz: Niz Nedefinirana
- Datum: operator, datum, Nedefinirana
- Cijelog broja: operator, broj, a zatim Nedefinirana
- Booleove vrijednosti: true ili false, Nedefinirana
- **Segmenta**    
- Naziv segmenata (s padajućeg popisa), izuzetaka (cilj korisnici koji nisu dio ovog segmenta).

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
 
