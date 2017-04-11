<properties 
   pageTitle="Azure mobilne radnje korisničko sučelje - razgovor" 
   description="Saznajte kako stupili korisnicima aplikacije s automatske obavijesti pomoću Azure Mobile radnje" 
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


# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Kako stupili korisnicima aplikacije s automatske obavijesti

U ovom se članku opisuju kartici **DOSEGNE** portala za **Mobilne radnje** . Pomoću portala za **Mobilne radnje** za nadzor i upravljanje mobilne aplikacije. Imajte na umu da da biste počeli koristiti portal najprije morate stvoriti račun za **Azure Mobile radnje** . Dodatne informacije potražite u članku [Stvaranje računa Azure Mobile radnje](mobile-engagement-create.md).

U odjeljku razgovor korisničkog sučelja je automatske kampanje alat za upravljanje kojem možete stvoriti/uređivanje/aktiviranje/Završi/monitor te pronađite Statistika kampanje automatske obavijesti i značajke koje se također može pristupiti putem API dosegne (i neke elemente nisku razinu automatske API-JA). Imajte na umu da bez obzira koristite li API-ji ili korisničkog Sučelja, morat ćete integraciju Azure Mobile radnje i razgovor u aplikaciji za svaki platformu s SDK prije korištenja dosegne kampanja.

>[AZURE.NOTE] Broj sekcija portala za **Mobilne radnje** korisničkog Sučelja sadrže gumb **PRIKAŽI pomoć** . Pritisnite taj gumb da biste saznali više kontekstne o sekcije.

## <a name="four-types-of-push-notifications"></a>Četiri vrste automatske obavijesti
1.    Objava - omogućuju vam da biste poslali poruke oglašavanje korisnicima koji ih preusmjeravanje na drugo mjesto unutar aplikacije ili im poslati u web-stranicu ili trgovinu izvan aplikacije. 
2.    Ankete - omogućuju vam da Prikupite informacije s krajnjim korisnicima tako da biste zatražili pitanja.
3.    Ih gura podataka – omogućuju slanje binarni ili base64 podatkovne datoteke. Informacije koje se nalaze u podataka automatske se šalje u aplikaciji za izmjenu trenutno sučelje vaših korisnika u svojoj aplikaciji. Aplikacija moraju imati mogućnost za obradu podataka u podataka pribadače.

## <a name="campaign-details"></a>Detalje kampanje

Možete uređivati, Kloniraj, brisanje ili aktiviranje kampanje koje nije aktiviran još po iznad njihova imena ili možete kliknuti da biste ih otvorili. Mogu Kloniraj kampanje koje već aktiviran po iznad njihova imena ili možete kliknuti da biste ih otvorili. Međutim, nije moguće promijeniti kampanje kada je aktiviran.
 
![Reach1][18]

## <a name="reach-feedback"></a>Dosegne povratnih informacija

Kliknite **Statistika** da biste vidjeli detalje o kampanje razgovor. **Jednostavan** prikaz omogućuje vizualni prikaz u obliku stupca trakasti grafikon o tome što se dogodilo Nakon aktiviranja s kampanjom. Prikaz za **Dodatno** omogućuje precizniji detalje o kampanje pribadače. Te detalje neće biti dostupne ako šaljete s kampanjom test odnosno automatske šalje test uređaj. Evo kako treba tumačenje te detalje:

1. **Pushed** - određuje se broj poruka pomiču uređaja. Taj broj ovisi o ciljne publike koju ste naveli prilikom stvaranja automatske kampanje. Ako ne navedete sve publiku, zatim ovaj automatske će se slati svih registrirani uređaja. Kao što je sve ostale servise automatske, ne možemo ne automatske obavijesti izravno na uređajima, ali umjesto automatske odgovarajući platformi određene automatske obavijesti usluge (PNS - GCM/APNS/WNS) tako da se isporučuje obavijesti uređaja. 

2.  **Delivered** - određuje se broj poruka koji su uspješno isporučuju drugi PNS na uređaju te označeni kao primljene putem mobilne radnje SDK. 
        
    *Razlozi za Delivered Brojanje manji od broja pomiču se:*
    
    1. Ako korisnik ima ste deinstalirali aplikaciju s uređaja, ali u PNS ne zna je trenutku mi šaljemo na automatske na PNS zatim poruku će biti ispušteni.
    2. Ako je uređaj ima aplikaciju, ali su uređaji same izvanmrežne za duljeg vremenskog razdoblja, na PNS se neće uspjeti isporučiti poruku na uređaju. 
    3. Ako je poruka isporučena na uređaju, ali SDK radnje Mobile u aplikaciji ne prepoznaje sadržaj poruke, zatim izostavlja poruke. To se može dogoditi ako prilagodbu obavijesti u aplikaciju generira iznimku koje ćemo Uhvatite u SDK i neposrednu poruku. To se može dogoditi i ako aplikaciju na uređaju se pomoću verzije programa SDK Mobile radnje koje ne mogu razumjeti novijom verzijom automatske poruke poslane s platforme, ali to je samo kada aplikaciju nadograđen kada je obavijesti poslanih iz servisa platforme. Na kartici **Dodatno** će se obavještava koliko poruke su prekida. 
    4. Na uređajima sa sustavom iOS poruke ponekad nije isporučena ako je neki uređaj na baterija ili ako aplikaciju koristi veliku količinu energije tijekom obrade udaljene obavijesti. To je ograničenje uređaje sa sustavom iOS.   

3.  **Prikazuje se** - određuje se broj poruka koje prikazuju se uspješno korisniku aplikacije na uređaju u obliku obavijest automatske/izlaz-zaslona – aplikacije sustava u centru za obavijesti ili obavijest o u aplikaciji iz aplikacije za mobilne uređaje.  Na kartici **Dodatno** će vas obavještava koliko su obavijesti sustava i koliko su obavijesti u aplikaciji. 
    
    *Brojanje razloge prikazuje se manji od broja Delivered (Čekanje da se prikazuje)*
    
    1. Ako kampanje obavijesti imali završni datum na njemu pa moguće je isporučena obavijesti, ali kada vrijeme isporučen da biste otvorili i prikazali korisniku aplikacije, ga već istekao je tako da nikada ne prikazuje.   
    2. Ako je obavijest obavijest u aplikaciji, a zatim obavijesti prikazuje se samo kada korisnik aplikacije otvoriti aplikaciju. U slučajevima gdje korisnika aplikacije nije otvorena aplikaciju SDK prijavljuje da obavijesti je isporučena, no još nije prikazan dok je otvorena aplikaciju. 
    2. Ako je obavijesti obavijest u aplikaciji i konfigurirali da bi se prikazao na određene aktivnosti/zaslonu, a zatim i prijavljena obavijesti kao isporučena, ali nije isporučena dok korisnik otvori aplikaciju na određeni zaslon. 
    
4.  **Interakcija korisnika** – određuje se broj poruka koja sadrži komunicirao korisnika aplikacije i neće sadržavati stavku poruke na koje su actioned ili napuste. 

    - *Korisnik aplikacije mogu akcija obavijest na jedan od sljedećih načina:*
            
        1. Ako je obavijesti obavijest sustava/izlaz-zaslona – aplikacije ili obavijest u aplikaciji poslane samo za obavijesti, a zatim aplikaciju klika na obavijesti.
        2. Ako je obavijesti obavijest u aplikaciji sa tekst ili prikaz za web ili anketa pa aplikacije klika na gumb Akcije u obavijesti.
        3. Ako je obavijesti obavijest u aplikacije s web-prikaz, a zatim aplikaciju klika na URL-u pogledu web-[Android samo]
    
    - *Korisnik aplikacije možete zatvoriti obavijest na jedan od sljedećih načina:*
    
        1. Klikom na gumb za zatvaranje obavijest o izravno. 
        2. Prstom prijeđete Odsutan ili brisanje obavijesti. 
        3. U aplikaciji obavijesti s tekst/web-sadržaja i ankete obično se prikazuju korisnika aplikacije u dva koraka. Oni vide obavijest, a kad kliknu na njemu, vide kasnije web/tekst/ankete sadržaja. Korisnik aplikacije možete izađite iz obavijest na jedan od ovih koraka i pojedinosti u prikazu napredne spremit će to. 

5.  **Actioned** - određuje se broj poruka koje su izričito actioned korisnik aplikacije. To je broj zanimljive kao što je to upućuje na to koliko korisnici aplikacije su zanimaju putem poruke pomiču u obavijesti. 
 
> [AZURE.NOTE] IOS & Windows platforme, ako korisnik ima aplikaciju otvorite i kampanje je kampanje "Bilo. kojem trenutku", zatim je moguće i iz aplikacije i obavijesti u aplikaciji se prikazuju u isto vrijeme. To može uzrokovati veće od na Delivered prikazuje zbroj. Ako korisnik stupi u interakciju ili akcije obavijesti, a zatim čak i broj interakcije Actioned korisnik ne može biti veći od Delivered. 


![Reach2][19]

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
