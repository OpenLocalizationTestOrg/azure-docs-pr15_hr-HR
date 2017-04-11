<properties 
   pageTitle="Otklanjanje poteškoća s StorSimple implementaciju | Microsoft Azure"
   description="U članku se opisuje kako dijagnosticirajte i Otklonite pogreške koje se javljaju prilikom implementacije StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="troubleshoot-storsimple-device-deployment-issues"></a>Otklanjanje poteškoća s StorSimple uređaj implementacije

## <a name="overview"></a>Pregled

Ovaj članak sadrži korisne upute za otklanjanje poteškoća za implementaciju sustava Microsoft Azure StorSimple. Opisuje uobičajene probleme, mogući uzroci i preporučena koraka za rješavanje problema koji koje se mogu pojaviti prilikom konfiguriranja StorSimple. Ove se informacije odnose fizički uređaj za lokalni StorSimple i StorSimple virtualnog uređaja.

> [AZURE.NOTE] Uređaj konfiguracije probleme vezane uz koje se može biti namijenjeno se može dogoditi kada implementacija uređaj prvi put ili ih se može pojaviti kasnije, kada je uređaj radu. U ovom se članku usredotočuje se na otklanjanje problema s prvo implementacije. Da biste otklonili radu uređaj, otvorite [Otklanjanje poteškoća radu uređaja](storsimple-troubleshoot-operational-device.md).

U ovom se članku opisuju alata za otklanjanje poteškoća s implementacijama StorSimple i sadrži detaljne primjer za otklanjanje poteškoća.

## <a name="first-time-deployment-issues"></a>Problemi sa prvo implementacije

Ako naiđete na problem prilikom implementacije uređaju prvi put, imajte na umu sljedeće:

- Ako pokušavate otkloniti fizički uređaj, provjerite je li da hardver instalacije i konfiguracije kao što je opisano u [Instaliranje StorSimple 8100 uređaja](storsimple-8100-hardware-installation.md) ili ponovno [instalirati StorSimple 8600 uređaj](storsimple-8600-hardware-installation.md).
- Pogledajte preduvjete za implementaciju. Provjerite imate li sve podatke koje su opisane u [kontrolni popis za konfiguraciju implementacije](storsimple-deployment-walkthrough.md#deployment-configuration-checklist).
- Pregledajte napomene StorSimple da biste vidjeli ako opisana je problem. Napomene u obuhvaćaju zaobilazna rješenja za instalaciju poznate probleme. 

Tijekom implementacije uređaj najčešće problemi koje korisnici nominalne pojaviti kada pokrenite čarobnjak za postavljanje i kada su registrirali uređaj putem komponente Windows PowerShell za StorSimple. (Pomoću komponente Windows PowerShell za StorSimple registrirati i konfiguriranje StorSimple uređaj. Dodatne informacije o Registracija uređaja potražite u članku [korak 3: konfigurirati i registrirati uređaju putem komponente Windows PowerShell za StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

U sljedećim se odjeljcima može vam pomoći riješiti probleme koji se pojaviti prilikom konfiguriranja uređaj StorSimple prvi put.

## <a name="first-time-setup-wizard-process"></a>Čarobnjak za prvo postavljanja

Sljedeći koraci sažetak postupka čarobnjaka za postavljanje. Postavljanje detaljne informacije potražite u članku [uvođenja StorSimple uređaju lokalnog](storsimple-deployment-walkthrough.md).

1. Pokrenite cmdlet [Pozovite HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) da biste pokrenuli čarobnjak za postavljanje vodit će vas kroz preostale korake. 
2. Konfiguriranje mreže: Čarobnjak za postavljanje možete konfigurirati postavke mreže za 0 mrežno sučelje podataka na uređaju StorSimple. Te postavke obuhvaćaju sljedeće:
  - Virtualna IP (VIP), masku i pristupnik – [Postavljanje HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) cmdlet se izvršava u pozadini. Konfigurira IP adresu, masku i pristupnik za 0 mrežno sučelje podataka na uređaju StorSimple.
  - Primarni DNS poslužitelj – [Postavljanje HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) cmdlet se izvršava u pozadini. Ga konfigurira postavke DNS-a za rješenje StorSimple.
  - NTP poslužitelja – [Postavljanje HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet se izvršava u pozadini. Ga konfigurira NTP postavke poslužitelja za rješenje StorSimple.
  - Neobavezni web proxy – [Postavljanje HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) cmdlet se izvršava u pozadini. Postavlja i omogućuje web-konfiguracija proxy poslužitelja za rješenje StorSimple.
3. Postavljanje lozinke: sljedeći je korak da biste postavili administratora uređaja i StorSimple snimke Upravitelj lozinki. Ako su pokrenuti Update 1, pa ćete će neće morati postavljanje lozinke Upravitelj StorSimple snimke.
  - Administratorsku lozinku uređaj koristi se za prijavu na uređaju. Lozinka zadani uređaj nije **Password1**.
  - Kada konfigurirate uređaj da biste pomoću upravitelja StorSimple snimke, potrebna je lozinka Upravitelj StorSimple snimke. Morate najprije postaviti lozinku u čarobnjaku za postavljanje, a zatim možete postaviti i promijeniti iz StorSimple Upravitelj servisa. Ovu lozinku potvrđuje s StorSimple snimke Upravitelj uređaja.
 
    > [AZURE.IMPORTANT] Lozinke koji se prikupljaju prije registracije, ali se primjenjuje samo kada je uspješno registrirali uređaj. Ako postoji pogreška da biste primijenili lozinke, morat ćete navesti lozinku ponovno dok se prikupe potrebna lozinke (koje ispunjavaju uvjete složenosti).

4. Registrirajte uređaj: završnom koraku je da biste registrirali uređaj sa servisom StorSimple Upravitelj koji se izvodi u Microsoft Azure. Registracija potrebno da biste [dobili ključa za registraciju servisa](storsimple-manage-service.md#get-the-service-registration-key) na portalu Azure klasični i ponuditi u čarobnjaku za instalaciju. Kada uređaj uspješno registriran, ključa za šifriranje podataka servisa navedeni su vam. Provjerite da biste zadržali ključa za šifriranje na sigurnom mjestu jer će se zatražiti da biste registrirali sve sljedeće uređaje sa servisom.

## <a name="common-errors-during-device-deployment"></a>Uobičajene pogreške tijekom implementacije uređaja

U sljedećim tablicama navedeni uobičajenih pogrešaka koje možete naići kada ste:

- Konfiguriranje postavki za potrebnu mrežu.
- Konfiguriranje postavki proxyja neobavezna web.
- Postavljanje administratora uređaja i StorSimple snimke Upravitelj lozinki. 
- Registrirajte uređaj. 

## <a name="errors-during-the-required-network-settings"></a>Pogreške tijekom potrebne mrežne postavke

| ne.| Poruka o pogrešci | Mogući uzroci | Preporučene akcije |
| ---| ------------- | --------------- | ------------------ |
| 1  | Pozivanje-HcsSetupWizard: Ta se naredba može se pokrenuti samo na aktivni kontroleru. | Konfiguriranje je se izvodi na pasivni kontroleru.| Pokrenite sljedeću naredbu s kontrolerom aktivna. Dodatne informacije potražite u članku [otkrivanje aktivni kontroler na uređaju](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).|
| 2 | Pozivanje-HcsSetupWizard: Uređaj nije spreman. | Postoje problemi s veza s mrežom na PODACIMA 0.| Provjerite je li fizičke mrežne na PODACIMA 0.|
| 3 | Pozivanje-HcsSetupWizard: Nema sukoba IP adresa s nekim drugim sustavom na mreži (iznimka HRESULT: 0x80070263). | Koristi neki drugi sustav nije IP naveden za podataka 0. | Navedite novi IP koji se ne koristi.|
| 4 | Pozivanje-HcsSetupWizard: Klaster resursa nije uspjelo. (Iznimka HRESULT: 0x800713AE). | Dupliciranje VIP. Navedeni IP već se koristi.| Navedite novi IP koji se ne koristi.|
| 5 | Pozivanje-HcsSetupWizard: Koji nisu valjani IPv4 adresa. | IP adresa navedeni su u ispravnom obliku.| Provjerite oblik i ponovno navesti IP adresa. Dodatne informacije potražite u članku [Ipv4 adresiranja][1]. |
| 6 | Pozivanje-HcsSetupWizard: Koji nisu valjani IPv6 adrese. | IP adresa navedeni su u ispravnom obliku.| Provjerite oblik i ponovno navesti IP adresa. Dodatne informacije potražite u članku [Ipv6 adresiranja][2].|
| 7 | Pozivanje-HcsSetupWizard: Dostupno više krajnje točke iz mapiranje krajnjoj točki. (Iznimka HRESULT: 0x800706D9) | Funkcija klaster ne funkcionira. | [Kontakt Microsoftovoj podršci](storsimple-contact-microsoft-support.md) za sljedeće korake.

## <a name="errors-during-the-optional-web-proxy-settings"></a>Pogreške tijekom postavke proxyja neobavezno web

| ne.| Poruka o pogrešci | Mogući uzroci | Preporučene akcije |
| ---| ------------- | --------------- | ------------------ |
| 1  | Pozivanje-HcsSetupWizard: Parametar koji nisu valjani (iznimka HRESULT: 0x80070057) | Jedan od parametara za postavke proxyja nije valjan.| URI nije naveden u ispravnom obliku. Pomoću sljedećih oblika: http://*<IP address or FQDN of the web proxy server>*:*<TCP port number>* |
| 2 | Pozivanje-HcsSetupWizard: RPC poslužitelj nije dostupan (iznimka HRESULT: 0x800706ba) | Uzrok je nešto od sljedećeg:<ol><li>Skupine nije prema gore.</li><li>Pasivni kontroler ne možete komunicirati s kontrolerom aktivni i pokretanja naredbe iz pasivni kontroler.</li></ol> | Ovisno o uzrok:<ol><li>[Obratite se podršci Microsoft](storsimple-contact-microsoft-support.md) provjerite nalazi li se skupine prema gore.</li><li>Pokrenite naredbu s kontrolerom aktivna. Ako želite da se izvodi naredbu s kontrolerom pasivni, morat ćete biti sigurni da kontrolerom pasivni mogli komunicirati s kontrolerom active. Morat ćete [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) ako je ovo veza prekinuta.</li></ol> |
| 3 | Pozivanje-HcsSetupWizard: RPC poziv nije uspio (iznimka HRESULT: 0x800706be) | Klaster nije dostupno. | [Obratite se podršci Microsoft](storsimple-contact-microsoft-support.md) provjerite nalazi li se skupine prema gore.|
| 4 | Pozivanje-HcsSetupWizard: Skupine resurs nije pronađen (iznimka HRESULT: 0x8007138f) | Klaster resursa nije pronađen. To se može dogoditi kada se instalacija nije bio ispravan. | Možda ćete morati ponovno postavljanje uređaja na tvorničke zadane postavke. [Obratite se podršci Microsoft](storsimple-contact-microsoft-support.md) da biste stvorili klaster resursa.|
| 5 | Pozivanje-HcsSetupWizard: Skupine nije mrežni resurs (iznimka HRESULT: 0x8007138c)| Resursi za klaster nisu u mreži. | [Kontakt Microsoftovoj podršci](storsimple-contact-microsoft-support.md) za sljedeće korake.|

## <a name="errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords"></a>Pogreške vezane uz administratora uređaja i StorSimple snimke Upravitelj lozinki

Zadani uređaj administratorsku lozinku je **Password1**. Ovo je lozinka istekla nakon prve prijave; Dakle, morat ćete promijeniti pomoću čarobnjaka za postavljanje. Kada uređaj registrirate prvi put, morate unijeti novi uređaj administratorsku lozinku. 

Ako koristite Upravitelj snimku StorSimple softver koji se izvodi na glavnom računalu Windows Server da biste upravljali uređaj, zatim morate navesti lozinku StorSimple snimku Upravitelj tijekom prvog registracije. 

Provjerite je li da vaše lozinke zadovoljava sljedeće uvjete:

- Lozinku administratora uređaja mora biti između 8 i 15 znakova.
- Upravitelj snimka StorSimple lozinku mora biti 14 ili 15 znakova.
- Lozinke mora sadržavati 3 od sljedećih vrsta 4 znaka: mala slova, velika slova, numeričke i posebno. 
- Lozinku ne mogu biti jednaki zadnje 24 lozinke.

Osim toga, imajte na umu da se lozinke istječe svake godine, a mogu se promijeniti samo kada je uspješno registrirali uređaj. Ako iz bilo kojeg razloga ne uspije Registracija, promijenit će se ne lozinke. Dodatne informacije o administratora uređaja i StorSimple snimke Upravitelj lozinki, idite na članak [korištenje servisa Upravitelj StorSimple da biste promijenili svoje StorSimple lozinke](storsimple-change-passwords.md).

Možda ćete naići jedan ili više sljedećih pogrešaka prilikom postavljanja administratora uređaja i StorSimple snimke Upravitelj lozinki.

| ne.| Poruka o pogrešci | Preporučene akcije |
| ---| ------------- | ------------------ | 
| 1  | Lozinka premašuje maksimalnu duljinu. |Koristite lozinku koja zadovoljava te preduvjete:<ul><li>Lozinku administratora uređaja mora biti između 8 i 15 znakova.</li><li>Upravitelj snimka StorSimple lozinku mora biti 14 ili 15 znakova.</li></ul> | 
| 2 | Lozinka ne zadovoljava potrebna duljina. | Koristite lozinku koja zadovoljava te preduvjete:<ul><li>Vaš uređaj administratorsku lozinku mora biti između 8 i 15 znakova.</li><li>Upravitelj snimka StorSimple lozinku mora biti 14 ili 15 znakova.</lu></ul> |
| 3 | Lozinka mora sadržavati malim slovima. | Lozinke mora sadržavati 3 od sljedećih vrsta 4 znaka: mala slova, velika slova, numeričke i posebno. Provjerite ispunjava li lozinku te preduvjete. |
| 4 | Lozinka mora sadržavati znamenke. | Lozinke mora sadržavati 3 od sljedećih vrsta 4 znaka: mala slova, velika slova, numeričke i posebno. Provjerite ispunjava li lozinku te preduvjete. |
| 5 | Lozinka mora sadržavati posebne znakove. | Lozinke mora sadržavati 3 od sljedećih vrsta 4 znaka: mala slova, velika slova, numeričke i posebno. Provjerite ispunjava li lozinku te preduvjete. |
| 6 | Lozinka mora sadržavati 3 od sljedećih vrsta 4 znaka: velika slova, mala slova, numeričke i posebno. | Lozinka sadržavati potrebne vrste znakova. Provjerite ispunjava li lozinku te preduvjete. |
| 7 | Parametar ne odgovara potvrdu. | Provjerite ispunjava li lozinke za sve zahtjeve i da ste je unijeli pravilno. |
| 8 | Lozinku ne odgovara zadani. | Lozinka zadani je *Password1*. Morate promijeniti tu lozinku kada se prvi put prijavite. |
| 9 | Lozinka koju ste upisali odgovara lozinki uređaja. Ponovno upišite lozinku. | Potvrdite lozinku, a zatim je ponovno unesite. |

Lozinke se prikupe prije uređaj je registrirana, ali primjenjuju se tek nakon uspješne registracije. Tijek rada za oporavak lozinke zahtijeva uređaj registriranje. 

> [AZURE.IMPORTANT] Općenito govoreći, pokušaj primijeniti lozinku ne uspije, zatim softvera više puta pokušava prikupljanje lozinka sve dok ne bude uspješan. U rijetko slučajevima, nije moguće primijeniti lozinku. U tom slučaju možete registrirali uređaj i nastavite, no lozinke neće se promijeniti. Primit ćete bez oznaka kao koji se lozinka je promijenjena – administratorsku lozinku uređaja ili upravitelj snimka StorSimple lozinku. Ako se pojavljuje u ovom slučaju, preporučujemo da promijenite obje lozinke.

Možete ponovno postaviti lozinke na portalu Azure klasični putem StorSimple Upravitelj servisa. Dodatne informacije potražite u odjeljku da biste: 

- [Promjena lozinke administratora uređaja](storsimple-change-passwords.md#change-the-device-administrator-password).
- [Promjena lozinke Upravitelj StorSimple snimke](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Pogreške tijekom Registracija uređaja

Servis upravitelja StorSimple koji se izvodi u Microsoft Azure koristite da biste registrirali uređaj. Tijekom Registracija uređaja nije naići jedan ili više od sljedećih problema.

| ne.| Poruka o pogrešci | Mogući uzroci | Preporučene akcije |
| ---| ------------- | --------------- | ------------------ |
| 1  | 350027 pogreška: Registracija uređaja s StorSimple Manager nije uspjelo. |  | Pričekajte nekoliko minuta, a zatim pokušajte ponovno. Ako se problem nastavi pojavljivati, [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md).|
| 2  | Pogreška 350013: Je došlo u Registracija uređaja. To može biti netočan servisa ključa za registraciju. | | Registrirajte uređaj ponovno pomoću ključa za registraciju točan servisa. Dodatne informacije potražite u članku [Registracija ključ servis.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 | Pogreška 350063: Provjera autentičnosti za servis upravitelja StorSimple proslijeđena, ali Registracija nije uspjelo. Ponovite postupak nakon određenog vremena. | Ta pogreška znači da je provjera autentičnosti s ACS prošao, ali poziv register servis nije uspjela. To može biti rezultat povremeni gubitak mreže glitch. | Ako se problem ne riješi, imajte [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md). |
| 4 | Pogreška 350049: Servis nije moguće pristupiti tijekom registracije. | Po poziv na servis primio je iznimka za web. U nekim slučajevima možda to dobiti popravi s ponovni operaciju kasnije. | Provjerite IP adresa i naziv DNS-a i pokušajte ponovno. Ako se problem ne riješi, [obratite Microsoft Support.](storsimple-contact-microsoft-support.md) | 
| 5 | Pogreška 350031: Uređaj već je registriran. | | Nije potrebno poduzimati. |
| 6 | Pogreška 350016: Registracija uređaja nije uspjela. | |Provjerite je li ključa za registraciju točni. |
| 7 | Pozivanje-HcsSetupWizard: Pojavila se pogreška prilikom registracije uređaja; To može biti zbog ispravna IP adresa ili naziv DNS-a. Provjerite postavke mreže i pokušajte ponovno. Ako se problem nastavi pojavljivati, [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md). (Pogreška 350050) | Provjerite je li uređaj možete ping izvan mreže. Ako ne postoji veza s mjesta izvan mreže, Registracija možda neće funkcionirati uz tu pogrešku. Ta se pogreška može biti kombinacija jednog ili više od sljedećeg:<ul><li>Ispravna IP</li><li>Netočan podmreže</li><li>Netočan pristupnika</li><li>Netočni DNS postavki</li></ul> | Pogledajte korake u [detaljnim primjer za otklanjanje poteškoća](#step-by-step-storsimple-troubleshooting-example). |
| 8 | Pozivanje-HcsSetupWizard: Trenutna operacija nije uspjela zbog pogreške interne servisa [0x1FBE2]. Ponovite postupak nakon tijekom. Ako se problem nastavi pojavljivati, obratite se Microsoft Support. | Ovo je generička pogreška izbačena sve pogreške nevidljivi korisnika iz servisa i agent. Najčešćih razloga možda nije uspjela provjera autentičnosti ACS. Mogući uzrok pogreške je da postoje problemi s konfiguracijom NTP poslužitelja i vrijeme na uređaju nije ispravno postavljen. | Ispraviti vrijeme (ako postoje problemi), a zatim ponoviti postupak registracije. Ako koristite naredbu Postavi HcsSystem - vremenska zona da biste prilagodili vremensku zonu, veliko početno slovo svake riječi u vremenske zone (na primjer "po pacifičkom standardnom vremenu").  Ako taj se problem ne riješi, [kontakt Microsoftove službe za podršku](storsimple-contact-microsoft-support.md) za sljedeće korake. |
| 9 | Upozorenje: Nije moguće aktivirati uređaj. Administratora uređaja i StorSimple snimku Upravitelj lozinki nisu promijenjene. | Ako registracija ne uspije, administratora uređaja i StorSimple snimke Upravitelj lozinki neće se promijeniti. |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>Alati za otklanjanje poteškoća s StorSimple implementacije

StorSimple sadrži nekoliko alate koje možete koristiti za otklanjanje poteškoća s StorSimple rješenje. To obuhvaća:

- Podrška za pakete i uređaja zapisnici 
- Cmdleti za posebno dizajnirane za otklanjanje poteškoća 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Podržava pakete i zapisnika uređaja dostupnih za otklanjanje poteškoća

Paket za podršku sadrži sve relevantne zapisnika koje vam mogu pomoći u tim Microsoft Support s otklanjanje problema s uređajem. Windows PowerShell za StorSimple možete koristiti za stvaranje paketa za šifriranu podršku zatim moguće zajednički koristiti s osoblje službe za podršku.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>Da biste pogledali zapisnike ili sadržaj paketa za podršku

1. Korištenje komponente Windows PowerShell za StorSimple da biste generirali paket za podršku, kao što je opisano u [Stvaranje i upravljanje paket podršku](storsimple-create-manage-support-package.md).

2. Preuzmite [dešifriranje skripte](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokalno na klijentskom računalu.

3. Koristite ovaj [detaljni opis postupka](storsimple-create-manage-support-package.md#edit-a-support-package) za otvaranje i dešifriranje paketa za podršku.

4. Paket zapisnike dešifriranu podršku su u obliku etw/etvx. Možete poduzeti sljedeće korake da biste vidjeli te datoteke u preglednik događaja sustava Windows:
  1. Pokrenite naredbu **eventvwr** na vaš klijent za Windows. Započet će preglednik događaja.
  2. U oknu **Akcije** kliknite **Otvori zapisnika sprema** i pokažite na zapisničke datoteke u obliku etvx/etw (paket podrška). Sada možete gledati datoteku. Kada otvorite datoteku, možete desnom tipkom miša kliknite i spremite datoteku u obliku teksta.
   
    > [AZURE.IMPORTANT] Cmdlet **Get-WinEvent** možete koristiti i da biste otvorili te datoteke u ljusci Windows PowerShell. Dodatne informacije potražite u članku [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) u dokumentaciji referenca cmdlet komponente Windows PowerShell.

5. Kada otvorite zapisnike u preglednik događaja, potražite sljedeće zapisnici koje sadrže problema vezanih uz konfiguraciju uređaja:

  - hcs_pfconfig/Operational zapisnika
  - hcs_pfconfig/Config

6. U datoteke zapisnika potražite nizova vezanih uz cmdleta pozove Čarobnjak za postavljanje. Popis tih cmdleta potražite u članku [prvo postupka čarobnjaka za postavljanje](#first-time-setup-wizard-process) . 

7. Ako niste moći Utvrđivanje uzroka problema, možete ga [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) za sljedeće korake. Kada Microsoft Support zatražite pomoć od sadržaja, slijedite korake u [Stvori zahtjev za podršku](storsimple-contact-microsoft-support.md#create-a-support-request) .

## <a name="cmdlets-available-for-troubleshooting"></a>Cmdleti za otklanjanje poteškoća

Koristite sljedeće komponente Windows PowerShell Cmdlete da biste otkrili pogreške za povezivanje.

- `Get-NetAdapter`: Pomoću tog cmdleta za otkrivanje stanja sučelje mreže. 

- `Test-Connection`: Pomoću tog cmdleta da biste provjerili veza s mrežom unutar i izvan mreže.

- `Test-HcsmConnection`: Pomoću tog cmdleta za provjerite je li uspješno registrirani uređaja.

Ako koristite Update 1 na uređaju StorSimple sljedeće Cmdlete dijagnostičkih dostupne su i.

- `Sync-HcsTime`: Pomoću tog cmdleta za prikaz uređaja vrijeme i prisilno sinkronizacija vremena s poslužiteljem NTP.

- `Enable-HcsPing`i `Disable-HcsPing`: korištenje tih cmdleta dopustili domaćini da biste pomoću naredbe ping sučelja mrežom na uređaju StorSimple. Prema zadanim postavkama sučelje mreže StorSimple ne reagirate ping zahtjevi.

- `Trace-HcsRoute`: Pomoću tog cmdleta kao alat za praćenje rute. Šalje pakete svaki usmjerivača na putu konačno odredište tijekom vremenskog razdoblja, a zatim formula izračunava rezultate na temelju pakete vratio svaki put. Budući da `Trace-HcsRoute` prikazuje stupanj gubitak pri navedeni usmjerivač ili vezu, možete pinpoint koje usmjerivača ili veze možda uzrokuje probleme s mrežom. 

- `Get-HcsRoutingTable`: Pomoću tog cmdleta da biste prikazali na lokalni IP tablice usmjeravanja.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Rješavanje problema s cmdlet Get-NetAdapter

Kada konfigurirate sučelje mreže za implementaciju prvo uređaj, status hardver nije dostupna u servis upravitelja StorSimple korisničkog Sučelja jer uređaj još nije registriran s uslugom. Uz to, na stranici sa stanjem hardver možda neće ispravno uvijek odražavati stanje uređaja, osobito ako postoje problemi koji utječe na sinkronizaciju servisa. U ovih situacija možete koristiti u `Get-NetAdapter` cmdlet radi određivanja stanja i status sučelja za mrežu.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Da biste vidjeli popis svih mrežnih prilagodnika na uređaju

1. Pokrenite Windows PowerShell za StorSimple, a zatim upišite `Get-NetAdapter`. 

2. Korištenje Izlaz na `Get-NetAdapter` cmdlet i smjernice u nastavku da biste shvatili status mrežno sučelje.
  - Ako je sučelje dobar i omogućena, **ifIndex** status se prikazuje kao **prema gore**.
  - Ako je dobar sučelje, ali nije fizički povezan s (tako da je mrežni kabel), **ifIndex** se prikazuje kao **onemogućene**.
  - Ako je sučelje dobar, ali nije omogućeno **ifIndex** status prikazuje se kao **NotPresent**.
  - Ako je sučelje ne postoji, ne prikazuje na popisu. Servis upravitelja StorSimple korisničkog Sučelja i dalje prikazuju ovog sučelja u stanju nije uspjelo.

Dodatne informacije o korištenju ovaj cmdlet potražite [GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) u referenci cmdlet komponente Windows PowerShell. 

U sljedećim se odjeljcima prikaz uzoraka izlaza iz na `Get-NetAdapter` cmdlet. 

 U ta uzorka kontroler 0 je kontrolerom pasivni pa je konfiguriran na sljedeći način:

- PODATAKA 0, podataka 1, 2 podataka i podataka 3 mreže sučelja postojao na uređaju.
- Podaci 4 i 5 podataka mrežne kartice nisu izlaganje; Dakle, on nije naveden u izlaz.
- Omogućen je podataka 0.

Kontroler 1 je aktivna kontroler pa je konfiguriran na sljedeći način:

- PODATAKA 0, podataka 1, 2 podataka, podataka 3, 4 podataka i podataka 5 mrežom sučelja postojao na uređaju.
- Omogućen je podataka 0.

**Uzorak izlaz – kontroler 0**

Slijedi Izlaz iz kontroler 0 (pasivni kontrolerom). Niste povezani podaci 1, podataka 2 i 3 podataka. PODATAKA 4 i 5 podataka ne navode se jer se ne nalaze na uređaj. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Uzorak izlaz – kontroler 1**

Slijedi Izlaz iz kontroler 1 (active kontrolerom). Samo podaci 0 mrežno sučelje na uređaju je konfiguriran i rad.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent

 
## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>Rješavanje problema s cmdlet Testiranje veze

Možete koristiti u `Test-Connection` cmdlet da biste odredili hoće li uređaj StorSimple možete povezati s mjesta izvan mreže. Ako se svi umrežavanje parametre, uključujući DNS-a, ispravno konfigurirane u čarobnjaku za postavljanje, možete koristiti u `Test-Connection` cmdlet da biste pomoću naredbe ping poznati adresu izvan mreže, kao što je outlook.com. 

Trebate omogućiti ping za otklanjanje poteškoća s povezivanjem s ovaj cmdlet ako ping je onemogućen.

Pogledajte sljedeće primjere izlaza iz na `Test-Connection` cmdlet. 

> [AZURE.NOTE] U prvi primjer uređaj konfiguriran netočni DNS-a. U drugom uzorku točna se DNS-om.
 
**Uzorak izlaz – netočni DNS-a**

U sljedeći primjer postoji bez izlaza IPV4 i IPV6 adrese, koja pokazuje da je DNS-a nije riješen. To znači da je bez veze s mjesta izvan mreže i mora biti navedena ispravna DNS. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Poslušajte izlaz – ispravljanje DNS-a**

U sljedeći primjer DNS-om vraća IPV4 adresa koji označava ispravno konfigurirano DNS-om. To se potvrđuje da je veza s mjesta izvan mreže. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Rješavanje problema s cmdlet Test HcsmConnection

Korištenje na `Test-HcsmConnection` cmdlet za uređaj koji je već povezani i registrirana na servisu StorSimple Manager. Ovaj cmdlet pomaže vam provjerite je li veze između registrirani uređaja i odgovarajuće StorSimple Upravitelj servisa. Pokrenite sljedeću naredbu na Windows PowerShell za StorSimple. 

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>Da biste pokrenuli Test HcsmConnection cmdleta

1. Provjerite je li uređaj registriran.

2. Provjerite status uređaja. Ako je uređaj nije aktiviran, u načinu za održavanje ili izvan mreže, može se prikazati sljedeće pogreške: 

   - ErrorCode.CiSDeviceDecommissioned – to znači da je deaktiviran uređaj.
   - ErrorCode.DeviceNotReady – to označava je li u načinu održavanja uređaj.
   - ErrorCode.DeviceNotReady – to znači da nije online na uređaju.

3. Provjerite je li pokrenut servis za StorSimple upravitelja (pomoću cmdleta [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) ). Ako servis nije pokrenut, može se prikazati sljedeće pogreške:

   - ErrorCode.CiSApplianceAgentNotOnline
   - ErrorCode.CisPowershellScriptHcsError – ukazuje da došlo je do iznimke kada ste pokrenuli Get-ClusterResource.

4. Potvrdite okvir token pristup kontrola servisa (ACS). Ako je throws web iznimku, možda je rezultat pristupnika problem, nedostaju provjeru autentičnosti proxyja, netočni DNS-a ili neuspješne provjere autentičnosti. Možda će se prikazati sljedeće pogreške:

   - ErrorCode.CiSApplianceGateway – ukazuje da iznimke HttpStatusCode.BadGateway: servis za razrješavanje naziva može razriješiti naziv glavnog računala. 
   - ErrorCode.CiSApplianceProxy – ukazuje da iznimke HttpStatusCode.ProxyAuthenticationRequired (HTTP Šifra stanja 407): klijent nije moguće provjeriti autentičnost s proxy poslužitelj. 
   - ErrorCode.CiSApplianceDNSError – ukazuje da WebExceptionStatus.NameResolutionFailure iznimke: servis za razrješavanje naziva može razriješiti naziv glavnog računala.
   - ErrorCode.CiSApplianceACSError – to znači da je servis vraća pogreške provjere autentičnosti, ali postoji povezivanje.
   
    Ako je vratiti iznimku za web, provjerite ErrorCode.CiSApplianceFailure. To znači da nije uspjela na uređaj.

5. Provjerite je li oblaka servisa. Ako servis throws web iznimku, može se prikazati sljedeće pogreške:

  - ErrorCode.CiSApplianceGateway – ukazuje da iznimke HttpStatusCode.BadGateway: programa Srednja proxy poslužitelj primili neispravan zahtjev iz drugog proxy ili izvornom poslužitelju.
  - ErrorCode.CiSApplianceProxy – ukazuje da iznimke HttpStatusCode.ProxyAuthenticationRequired (HTTP Šifra stanja 407): klijent nije moguće provjeriti autentičnost s proxy poslužitelj. 
  - ErrorCode.CiSApplianceDNSError – ukazuje da WebExceptionStatus.NameResolutionFailure iznimke: servis za razrješavanje naziva može razriješiti naziv glavnog računala.
  - ErrorCode.CiSApplianceACSError – to znači da je servis vraća pogreške provjere autentičnosti, ali postoji povezivanje.
  
    Ako je vratiti iznimku za web, provjerite ErrorCode.CiSApplianceSaasServiceError. Ukazuje na problem sa servisom StorSimple Manager.
 
6. Provjerite Bus servisa Azure povezivanje. ErrorCode.CiSApplianceServiceBusError ukazuje da uređaj ne može povezati s Bus servisa.
 
Prijava datoteke CiSCommandletLog0Curr.errlog i CiSAgentsvc0Curr.errlog će imati više informacija, kao što su pojedinosti iznimke. 

Dodatne informacije o tome kako pomoću cmdleta potražite [Test HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) u ljusci Windows PowerShell reference dokumentacije.

> [AZURE.IMPORTANT] Pokrenite ovaj cmdlet za aktivni i pasivni kontroler. 
 
Pogledajte sljedeće primjere izlaza iz na `Test-HcsmConnection` cmdlet. 

**Poslušajte izlaz – uspješno registrirane uređaju pokrenut izdanje StorSimple (srpanj 2014.)**

Prvi primjer je putem uređaja koji je uspješno registrirana sa servisom StorSimple upravitelj i ima bez problema s povezivanjem. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes to complete....
     Checking device authentication.  ... Success.
     Checking connectivity from device to StorSimple Manager service.  ... This operation will take few minutes to complete....
     Checking connectivity from device to StorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service to StorSimple device. .... Success.
     Controller1>

**Ogledna izlaz – uspješno registrirane uređaju pokrenut StorSimple Update 1**

Ako su pokrenuti Update 1 na uređaju StorSimple, ne morate pokrenuti s parametrom opširno.

      Controller1>Test-HcsmConnection
       
      Checking device registration state  ... Success
      Device registered successfully
       
      Checking primary NTP server [time.windows.com] ... Success
       
      Checking web proxy  ... NOT SET
       
      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET
       
      Checking device online  ... Success
 
      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success
       
      Checking connectivity from device to service  ... This will take a few minutes.
       
      Checking connectivity from device to service  ... Success
       
      Checking connectivity from service to device  ... Success
       
      Checking connectivity to Microsoft Update servers  ... Success
      Controller1>

**Poslušajte izlaz – izvanmrežne uređaju pokrenut izdanje StorSimple (srpanj 2014.)**

Ovaj primjer je u uređaj koji ima status **izvanmrežno** Azure klasični portalu.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device to SaaS.. Failure

Uređaj nije mogao povezati pomoću trenutnog web-konfiguracija proxy poslužitelja. To može biti problem s web-konfiguracija proxy poslužitelja ili problema s povezivanjem s mrežom. U ovom slučaju, provjerite je li web-postavke proxyja ispravne, a proxy poslužitelja web na mreži ste i dostupna. 

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Rješavanje problema sa sinkronizacijom HcsTime cmdlet

Pomoću tog cmdleta za prikaz vrijeme uređaja. Ako je uređaj vrijeme na pomak s poslužiteljem NTP, pa možete koristiti ovaj cmdlet da biste prisilno-vremenski sinkronizirati s NTP poslužitelja. Ako pomak između uređaja i poslužitelja NTP veća od pet minuta, prikazat će se upozorenje. Ako pomak premašuje 15 minuta, uređaj će raditi izvanmrežno. Ovaj cmdlet i dalje možete koristiti da biste nametnuli sinkronizacija vremena. Međutim, ako pomak premašuje 15 sati, pa neće moći prisilno sinkronizacije će biti prikazani vrijeme i poruka o pogrešci.

**Ogledna izlaz – sinkronizacija prisilne vremena pomoću sinkronizacije HcsTime**
 
     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.
 
     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>Rješavanje problema s HcsPing za omogućivanje i onemogućivanje HcsPing cmdleta

Pomoću tih cmdleta da biste bili sigurni sučelja mrežom na uređaju odgovoriti ICMP ping zahtjevi. Prema zadanim postavkama sučelje mreže StorSimple ne reagirate ping zahtjevi. Ovaj cmdlet je najlakše ćete znati je li uređaj na mreži i dostupna.  

**Ogledna izlaz – HcsPing za omogućivanje i onemogućivanje HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>Rješavanje problema s cmdlet praćenja HcsRoute

Pomoću tog cmdleta kao alat za praćenje rute. Šalje pakete svaki usmjerivača na putu konačno odredište tijekom vremenskog razdoblja, a zatim formula izračunava rezultate na temelju pakete vratio svaki put. Jer cmdlet prikazuje stupanj gubitak na sve navedene usmjerivač ili vezu, možete pinpoint koje usmjerivača ili veze možda uzrokuje probleme s mrežom.

**Izlaz uzorka s prikazom načina za praćenje rute paket s praćenja HcsRoute**

     Controller0>Trace-HcsRoute -Target 10.126.174.25
     
     Tracing route to contoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]
      
     Computing statistics for 25 seconds...
                 Source to Here   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]
      
     Trace complete.

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>Rješavanje problema s cmdlet Get-HcsRoutingTable

Koristite ovaj cmdlet da biste prikazali tablicu usmjeravanja za svoj uređaj StorSimple. Usmjeravanje tablica je skup pravila koji možete utvrditi gdje će biti preusmjereni podatkovne pakete putovanje putem mreže za Internet Protocol (IP). 

Usmjeravanje tablica prikazuje sučelja i pristupnika koji usmjerava podaci navedeni mrežama. Pruža i metriku usmjeravanja koja je maker odluka za put snimanja dosegne određenu odredište. U donjem metriku usmjeravanje, viša preference. 

Na primjer, ako imate 2 sučelje mreže, podataka 2 i 3 podataka, povezani s Internetom. Ako su usmjeravanje metriku podataka 2 i 3 podataka 15 i 261 odnosno, 2 podataka s niže usmjeravanje metrika je sučelje Preferirani dosegne s Internetom.

Ako su pokrenuti Update 1 na uređaju StorSimple, vaše podatke 0 mrežno sučelje ima najveći preferenci za promet oblaka. To podrazumijeva da čak i ako postoje drugi sučelja oblaka s omogućenim, promet oblaka će biti usmjerena kroz podataka 0. 

Ako naiđete na `Get-HcsRoutingTable` cmdlet bez navođenja parametre (kao što je prikazano u sljedećem primjeru), cmdlet izlaz IPv4 i IPv6 tablice usmjeravanja. Osim toga, možete odrediti `Get-HcsRoutingTable -IPv4` ili `Get-HcsRoutingTable -IPv6` da biste dobili odgovarajuću tablicu usmjeravanja.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================
       
      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================
       
      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None
       
      Controller0>
 
## <a name="step-by-step-storsimple-troubleshooting-example"></a>Detaljne StorSimple primjer otklanjanje poteškoća

Sljedeći primjer prikazuje detaljne otklanjanje poteškoća s StorSimple implementacije. U slučaju primjer Registracija uređaja ne uspijeva uz poruku o pogrešci koja navodi postavke mreže ili naziv DNS-a nije valjana.

Poruka o pogrešci koja je vraćena je:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

Pogrešku može uzrokovati nešto od sljedećeg:

- Instalacije pogrešne hardvera
- Pogreška u mrežnom interface(s)
- Ispravna IP adresa, masku, pristupnika, primarni DNS poslužitelj ili proxy poslužitelja za web
- Netočan registraciji ključ
- Postavke vatrozida nisu ispravne

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Da biste pronašli i riješili problem Registracija uređaja

1. Konfiguraciju uređaj: kontrolerom aktivni pokrenut `Invoke-HcsSetupWizard`.

     > [AZURE.NOTE] Na aktivni kontroleru morate pokrenuti čarobnjak za postavljanje. Da biste potvrdili da ste povezani s kontrolerom aktivna, pogledajte natpis prezentirati serijskih konzolu. Na natpisu označava tome jeste li povezani kontroler 0 ili kontroler 1 i je li kontrolerom aktivni ili pasivni. Dodatne informacije potražite na [otkrivanje aktivni kontroler na uređaju](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).
 
2. Provjerite je li uređaj ispravno cabled: Provjera mrežni kabeli na uređaju ponovno ravnini. Na kabeli je za model uređaja. Dodatne informacije potražite [Instaliranje StorSimple 8100 uređaja](storsimple-8100-hardware-installation.md) ili ponovno [instalirati StorSimple 8600 uređaj](storsimple-8600-hardware-installation.md).

     > [AZURE.NOTE] Ako koristite 10 GbE mrežne priključke, morat ćete koristiti navedeni QSFP SFP prilagodnika i SFP kabela. Dodatne informacije potražite u članku [popis kabeli, parametri, i transceivers preporučuje dobavljača OEM Mellanox priključke](http://www.mellanox.com/page/cables?mtag=cable_overview).
 
3. Provjera stanja mrežnog sučelja:

   - Pomoću cmdleta Get-NetAdapter za otkrivanje stanja sučelje mreže za podatke 0. 
   - Ako vezu ne funkcionira, **ifindex** status pokazati sučelje nije prema dolje. Zatim morat ćete mrežnu vezu priključka na uređaj i promjenu. Također ćete pravilo out neispravni kabeli. 
   - Ako sumnjate da se podaci 0 priključak na aktivni kontroler nije uspjela, to možete potvrditi putem veze s podacima 0 priključak na kontroleru 1. Da biste to potvrdili, prekid mrežni kabel na stražnjoj strani uređaj s kontroler 0, kabelom povežite s kontrolerom 1, a zatim ponovno pokrenite cmdlet Get-NetAdapter. 
   Ako se podaci 0 priključak na kontroler ne uspije, [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) za sljedeće korake. Možda ćete morati zamijeniti kontrolerom na računalu.
 
4. Provjerite vezu s parametar:
   - Provjerite jesu li podaci sučelje 0 mreže na kontrolera 0 i kontrolera 1 u vaš primarni s prilozima na istoj podmreži. 
   - Provjerite koncentrator ili usmjerivač. Obično oba kontrolera treba povezati s istim koncentrator ili usmjerivač. 
   - Provjerite imaju li parametri koji koristite za povezivanje podataka 0 za oba kontrolera iste vLAN.
   
5. Uklonite sve pogreške korisnika:

  - Pokrenite čarobnjak za postavljanje ponovno (izvođenja **Pozovite HcsSetupWizard**), a zatim unesite vrijednosti ponovno da biste bili sigurni da postoje pogreške. 
  - Provjerite je li registracija ključ. Isti ključa Registracija mogu se povezati više uređaja StorSimple Upravitelj servisa. Slijedite postupak u [Registracija ključ servisa](storsimple-manage-service.md#get-the-service-registration-key) da biste bili sigurni koristite li ispravnu Registracija ključ.

    > [AZURE.IMPORTANT] Ako imate više servise, morat ćete osigurati da registraciji ključ za odgovarajući servis koristi da biste registrirali uređaj. Ako ste registrirali uređaj sa servisom pogrešan StorSimple Manager, morat ćete se [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) za sljedeće korake. Možda ćete morati izvršiti tvorničke Vrati izvorne postavke uređaja (što može dovesti do gubitka podataka) da biste zatim povezivanje sa servisom predviđeno.

6. Pomoću cmdleta Testiraj vezu Provjerite jeste li povezani s mjesta izvan mreže. Dodatne informacije potražite za [Otklanjanje poteškoća s cmdlet Testiraj vezu](#troubleshoot-with-the-test-connection-cmdlet).

7. Provjerite ima li vatrozid smetnje. Ako ste potvrdili koji su sve točno, virtualne IP (VIP), podmreže, pristupnika i DNS postavke i i dalje gledali problema s povezivanjem, a zatim je moguće vatrozid blokira komunikaciju između uređaja i izvan mreže. Koje je potrebno provjeriti priključke 80 i 443 dostupnih na uređaju StorSimple za izlazni komunikaciju. Dodatne informacije potražite u članku [preduvjeti za StorSimple uređaj za povezivanje s mrežom](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

8. Pogledajte zapisnike. Idite na [web-mjesto paketi za podršku i u okvir za uređaj zapisnici dostupne za otklanjanje poteškoća](#support-packages-and-device-logs-available-for-troubleshooting).

9. Ako prethodnih koraka ne uspijete riješiti problem, [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) za pomoć.

## <a name="next-steps"></a>Daljnji koraci
[Saznajte kako otkloniti poteškoće na uređaju sa sustavom radu](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
