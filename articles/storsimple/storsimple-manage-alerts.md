<properties 
   pageTitle="Prikaz i upravljanje upozorenjima StorSimple | Microsoft Azure"
   description="U članku se opisuje StorSimple upozorenja uvjete i težinu, upute za konfiguriranje upozorenja i kako koristiti StorSimple Upravitelj servisa za upravljanje upozorenjima."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="anbacker" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-alerts"></a>Prikaz i upravljanje upozorenjima StorSimple pomoću StorSimple Upravitelj servisa

## <a name="overview"></a>Pregled

Karticu **upozorenja** u servisu StorSimple Manager omogućuje pregled i poništite StorSimple uređaja vezanih uz upozorenja na temelju u stvarnom vremenu. Kartica, središnje je moguće nadzirati problemi stanja uređajima StorSimple i cjelokupan rješenja za Microsoft Azure StorSimple.

Pomoću ovog praktičnog vodiča opisuju uobičajenih uvjeta koji se upozorenja, Razina upozorenja težinu i upute za konfiguriranje upozorenja. Uz to, sadrži tablice upozorenja Brzi vodič koji vam omogućuju da brzo pronaći određene upozorenja i odgovaranje pravilno.

![Stranice upozorenja](./media/storsimple-manage-alerts/HCS_AlertsPage.png)

## <a name="common-alert-conditions"></a>Uobičajenih uvjeta koji se upozorenja

Vaš uređaj StorSimple stvara upozorenja u odgovoru raznih uvjeta. Ovo su najčešće upozorenja uvjeti:

- **Hardverske probleme** – te upozorenja obavijestili o stanju hardvera. Omogućuju znati ako firmver nadogradnje je potrebno, ako je mrežno sučelje problema ili ako postoji problem s nekim pogonima podataka.

- **Problema s povezivanjem** – te upozorenja pojaviti kada postoji težine za prijenos podataka. Problemi komunikacije može dogoditi tijekom prijenos podataka da biste i s računa za Azure prostora za pohranu zbog nedostatka veze između uređaja i StorSimple Upravitelj servisa. Problemi komunikacije su neke od Najteži da biste riješili problem jer je toliko točke nije uspjelo. Prvo uvijek treba provjeriti veza s mrežom i pristup Internetu su dostupni prije nastavka na više Napredno otklanjanje poteškoća. Pomoć za otklanjanje poteškoća, idite na [Otklanjanje poteškoća s cmdlet Testiraj vezu](storsimple-troubleshoot-deployment.md).

- **Problemi s performansama** – te upozorenja su zbog odjeljaka sustav nije optimalnog, kao što su kada je pod velikim opterećenjem.

Osim toga, možda će se prikazati upozorenja vezana uz sigurnost, ažuriranja i neuspjelim.

## <a name="alert-severity-levels"></a>Razina za upozorenje težinu

Upozorenja imaju različite težinu razine, ovisno o utjecaj koji će imati upozorenja situacija i potrebe za odgovor na upozorenje. Razine težinu su:

- **Od ključne važnosti** – upozorenje je kao odgovor na uvjet koji je utjecaja uspješno performanse sustava. Akcija potreban je kako bi StorSimple servisa ne prekine.

- **Upozorenje** – ovaj uvjet može postati ključnih ako nije riješen. Trebali biste istražili situaciji i ništa poduzimati potrebne da biste očistili problem.

- **Informacije o** – upozorenje sadrži informacije koje se mogu poslužiti za praćenje i upravljanje sustavom.

## <a name="configure-alert-settings"></a>Konfiguriranje postavki upozorenja

Možete odabrati želite li primati obavijesti putem e-pošte upozorenja uvjeti za svaku StorSimple uređaja. Osim toga, možete prepoznati drugim primateljima upozorenje tako da unesete njihove adrese e-pošte u okviru **drugim primateljima e-pošte** razdvojene točkom sa zarezom.

>[AZURE.NOTE] Možete unijeti najviše 20 adrese e-pošte po uređaja.

Kada omogućite obavijesti e-pošte na uređajima, članove popisa obavijesti primit će poruku e-pošte svaki put kada se dogodi Kritično upozorenje. Poruka će biti poslana s *storsimple-alerts-noreply@mail.windowsazure.com* i će opisuju upozorenja uvjet. Primatelji kliknite **Otkaži pretplatu** izuzimanje iz popisa obavijesti e-pošte.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>Da biste omogućili obavijesti e-pošte upozorenja za uređaj

1. Otvorite odjeljak **uređaji** > **Konfiguriraj** za uređaj.

2. U odjeljku **Postavke upozorenja**postaviti sljedeće:

    1. U polju **obavijesti e-pošte za slanje** , odaberite **da**.

    2. U polje **Administratori servisa za e-pošte** odaberite **da** ako želite da se obratite administratoru za servis, a sve dodatnih administratora primati upozorenja obavijesti.

    3. U polju **drugim primateljima e-pošte** unesite adresu e-pošte svih primatelja koje bi trebali primiti upozorenje obavijesti. Unesite imena u obliku *someone@somewhere.com*. Za razdvajanje adrese e-pošte, koristite točke sa zarezom. Možete konfigurirati najviše 20 adrese e-pošte po uređaja. 

        ![Konfiguriranje upozorenja obavijesti](./media/storsimple-manage-alerts/AlertNotify.png)

3. Da biste poslali test obavijest e-pošte, kliknite ikonu sa strelicom pokraj **test slanje e-pošte**. Servis za upravitelja StorSimple prikazat će se poruke o statusu kao prosljeđuje test obavijesti. 

4. Kada se prikaže sljedeća poruka, kliknite **u redu**. 

    ![Upozorenja testiranje obavijesti e-pošte poslane](./media/storsimple-manage-alerts/HCS_AlertNotificationConfig3.png)

    >[AZURE.NOTE] Ako se ne može poslati obavijest test, StorSimple Upravitelj servisa prikazat će odgovarajuću poruku. Kliknite **u redu**, pričekajte nekoliko minuta pa pokušajte ponovno poslati poruku test obavijesti. 

## <a name="view-and-track-alerts"></a>Prikaz i praćenje upozorenja

Nadzorna ploča službe za Upravitelj StorSimple nudi sažet pregled broj upozorenja na uređajima organizirani po razini težinu.

![Nadzorna ploča za upozorenja](./media/storsimple-manage-alerts/admin_alerts_dashboard.png)

Klikom na razinu težinu otvara karticu **upozorenja** . Rezultati obuhvatiti samo upozorenja koje odgovaraju te razine težinu.

![Upozorenja izvješća iz djelokruga vrstu upozorenja](./media/storsimple-manage-alerts/admin_alerts_scoped.png)

Klikom na upozorenje na popisu pruža dodatne detalje upozorenja, uključujući vrijeme zadnje prijavljena je upozorenje, broja pojavljivanja upozorenje na uređaj i preporučene akcije da biste riješili upozorenje. Ako je hardver upozorenja, komponenta hardvera i odredite.

![Primjer upozorenja na hardvera](./media/storsimple-manage-alerts/admin_alerts_hardware.png)

Ako morate poslati informacije tvrtki Microsoft Support, tekstne datoteke možete kopirati detalje upozorenja. Nakon što ste pratili na preporuke i riješiti na upozorenja uvjet lokalnog, treba očistiti upozorenje s uređaja odabirom upozorenja na kartici **upozorenja** i klikom **poništite**. Da biste očistili višestruka upozorenja, odaberite svaki upozorenja, kliknite sve stupce osim stupaca **upozorenja** , a zatim **poništite** nakon što ste odabrali sva upozorenja tabulatore. Imajte na umu da su neke upozorenja automatski poništeni kada se problem ne riješi ili sustav ažurira upozorenje s novim podacima.

Kada kliknete **Očisti**, imat ćete mogućnost davanja komentare o upozorenje i koraci koje ste snimili da biste riješili taj problem. Ako neki drugi događaj aktivira s novim podacima, sustav bit će uklonjena neke događaje. U tom slučaju prikazat će se sljedeća poruka.

![Poništite okvir upozorenjima](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Sortiranje i pregled upozorenja

Možda će vam biti učinkovitiji za pokretanje izvješća o upozorenjima tako da možete pregledavati i isključite ih u grupe. Uz to, karticu **upozorenja** možete prikazati do 250 upozorenja. Ako ste premašili taj broj upozorenja, ne sva upozorenja prikazat će se u zadanom prikazu. Možete kombinirati sljedeća polja da biste prilagodili koja upozorenja:

- **Status** – možete prikazati **aktivna** ni **očišćeno** upozorenja. Aktivni upozorenja i dalje se aktiviraju u vašem sustavu dok očišćenog upozorenja su ili ručno poništen administrator ili programski poništen jer sustav upozorenja uvjet ažurirati novim podacima.

- **Težinu** – možete prikazati upozorenja sve razine težinu (ključnih, upozorenje, informacije o) ili samo s određenim težinu, kao što su samo ključnih upozorenja.

- **Izvor** – prikaz upozorenja iz svih izvora ili ograničili upozorenja onima koji dolaze iz ili usluge ili jedne ili svih uređaja.

- **Vremenski raspon** – navođenjem **od** i **do** datuma i vremenske oznake, možete pogledati upozorenja tijekom vremenskog razdoblja koja vas zanima.

## <a name="alerts-quick-reference"></a>Brzi pregled upozorenja

U sljedećim tablicama navedeni neka upozorenja Microsoft Azure StorSimple koje biste mogli naići, kao i dodatnih informacija i preporuke tamo gdje je dostupno. Upozorenja o uređaju StorSimple nalaziti u jednu od sljedećih kategorija:

- [Oblak povezivanje upozorenja](#cloud-connectivity-alerts)

- [Klaster upozorenja](#cluster-alerts)

- [Izrada oporavak upozorenja](#disaster-recovery-alerts)

- [Hardverski upozorenja](#hardware-alerts)

- [Zadatak pogreška upozorenja](#job-failure-alerts)

- [Lokalno prikvačene glasnoću upozorenja](#locally-pinned-volume-alerts)

- [Povezivanje s mrežom upozorenja](#networking-alerts)

- [Performanse upozorenja](#performance-alerts)

- [Sigurnosna upozorenja](#security-alerts)

- [Podrška za paket upozorenja](#support-package-alerts)

- [Ažuriranje upozorenja](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Oblak povezivanje upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Povezivanje <*naziv vjerodajnica oblaka*> nije moguće uspostaviti.|Ne mogu se povezati s računom za pohranu.|Čini se da možda postoji problem s povezivanjem s uređajem. Ponovno pokrenite na `Test-HcsmConnection` cmdlet putem sučelja za Windows PowerShell za StorSimple na uređaju Prepoznajte i riješite problem. Ako postavke nisu točne, problem može biti s vjerodajnicama račun za pohranu za koje je pokrenuto upozorenje. U ovom slučaju, koristite na `Test-HcsStorageAccountCredential` cmdlet da biste utvrdili postoje li problemi koje možete riješiti.<ul><li>Provjerite postavke mreže.</li><li>Provjerite račun vjerodajnice za pohranu.</li></ul>|
|Ne možemo ne primili kontrolni signal s uređaja posljednje <*broj*> minuta.|Ne mogu se povezati s uređajem.|Čini se da postoji problem s povezivanjem s uređajem. Koristite na `Test-HcsmConnection` cmdlet putem sučelja za Windows PowerShell za StorSimple na uređaju za prepoznavanje i rješavanje problema ili obratite se administratoru.|

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>Ponašanje StorSimple kada ne uspije povezivanje oblaka

Što se događa ako ne uspije povezivanje oblaka za uređaj StorSimple izvodi u radni?

Ne uspijete oblaka povezivanje na uređaju radnog StorSimple, zatim ovisno o stanju uređaju, sljedeće se može pojaviti: 

- **Za lokalne podatke na uređaju**: neko vrijeme neće biti bez prekidu i čitanja će se i dalje posluživanje. Međutim, kao što je broj preostala IOs povećava i premašuje ograničenje, na čitanja se može pokrenuti uvoza. 

    Ovisno o količinu podataka na uređaju, u zapisivanja će i nastaviti pojaviti nekoliko sati nakon u prekidu u oblak povezivanje. Za zapisivanje će se zatim usporiti i naposljetku početi neće uspjeti ako povezivanje oblak je nadziranje prekinuto nekoliko sati. (Nema privremeno spremište na uređaju za podatke koji će se pomiču u oblak. U ovom području Isprazni prilikom slanja podataka. Povezivanje ne uspije, podatke u tom području prostora za pohranu će se pomiču u oblak i IO neće uspjeti.)   

 
- **Za podatke u oblaku**: Većina pogrešaka povezivanje oblaka, vraća se pogreška. Kada je vraćen spajanje, na IOs su nastaviti bez potrebe da bi se prikazala glasnoće online korisnik. U rijetko instance intervencije korisnika može biti potrebne da bi se prikazala natrag glasnoće na Internetu na portalu Azure klasični. 
 
- **Za oblak snimke u tijeku**: postupak je ponoviti nekoliko puta u roku od 4-5 sati i spajanje je vraćen, snimaka oblaka neće uspjeti.


### <a name="cluster-alerts"></a>Klaster upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Uređaj nije uspjelo putem <*naziv uređaja*>.|Uređaj je u načinu za održavanje.|Uređaj nije uspjelo putem zbog unosa ili održavanja izlaska iz njega. To je normalno i potreban je ne poduzmete. Nakon što ste prihvaćen upozorenje, poništite ga na stranici upozorenja.|
|Uređaj nije uspjelo putem <*naziv uređaja*>.|Samo ažurirati opremom uređaja ili softvera.|Došlo je prebacivanje klaster zbog ažuriranje. To je normalno i potreban je ne poduzmete. Nakon što ste prihvaćen upozorenje, poništite ga na stranici upozorenja.|
|Uređaj nije uspjelo putem <*naziv uređaja*>.|Kontroler je isključiti ili ponovno pokrenuti.|Uređaj nije uspjela putem jer aktivni kontroler je isključiti ili ponovno pokrenuti administrator. Potreban je ne poduzmete. Nakon što ste prihvaćen upozorenje, poništite ga na stranici upozorenja.|
|Uređaj nije uspjelo putem <*naziv uređaja*>.|Planirani prebacivanje.|Provjerite je li to je bio planiranog prebacivanje. Nakon što ste snimili odgovarajuće akcije, poništite upozorenje na stranici upozorenja.|
|Uređaj nije uspjelo putem <*naziv uređaja*>.|Neplanirano prebacivanje.|Automatski oporavak iz neplanirano failovers ugrađena je StorSimple. Ako vidite velikog broja te upozorenja, obratite se Microsoft Support.|
|Uređaj nije uspjelo putem <*naziv uređaja*>.|Uzrok druge/Nepoznato.|Ako vidite velikog broja te upozorenja, obratite se Microsoft Support. Nakon što se problem ne riješi, poništite upozorenje na stranici upozorenja.|
|Servisa ključnih uređaja izvješća stanja kao nije uspjelo.|Nije uspjelo DataPath servisa. |Pomoć zatražite od Microsoft Support.|
|Virtualna IP adresa za mrežno sučelje <*podataka #*> izvješća stanja kao nije uspjelo. |Uzrok druge/Nepoznato. |Ponekad privremene uvjeta mogu prouzročiti sljedeće upozorenja. Ako je to slučaj, upozorenje će se automatski očistiti nakon određenog vremena. Ako se problem nastavi pojavljivati, obratite se Microsoft Support.|
|Virtualna IP adresa za mrežno sučelje <*podataka #*> izvješća stanja kao nije uspjelo.|Naziv sučelja: <*podataka #*> IP adresa <IP address> ne može biti na mreži jer je otkriven je duplikat IP adresa na mreži. |Provjerite je li duplicirane IP adresa nestaje s mreže ili konfigurirajte s različitim IP adresa.|


### <a name="disaster-recovery-alerts"></a>Izrada oporavak upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Oporavak operacije nije moguće vratiti sve postavke za taj servis. Uređaj konfiguracije podaci u stanju koje nisu usklađene za neke uređaje.|Nedosljednosti podataka nakon oporavka Izrada prepoznat.|Šifrirane podatke na servis nije sinkroniziran s na uređaju. Autorizirajte uređaj <*naziv uređaja*> iz StorSimple upravitelja da biste pokrenuli postupak sinkronizacije. Pokretanje izvješća pomoću sučelja sustava Windows PowerShell za StorSimple u `Restore-HcsmEncryptedServiceData` na uređaju <*naziv uređaja*> cmdlet, koja omogućuje staru lozinku kao ulaz za ovaj cmdlet da biste vratili sigurnosni profil. Pokrenite na `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet da biste ažurirali ključa za šifriranje podataka servisa. Nakon što ste snimili odgovarajuće akcije, poništite upozorenje na stranici upozorenja.|


### <a name="hardware-alerts"></a>Hardverski upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Komponenta hardvera <*ID komponente*> izvješća stanja kao <*status*>.||Ponekad privremeni uvjete mogu prouzročiti sljedeće upozorenja. Ako je tako, upozorenje bit će automatski uklonjena nakon određenog vremena. Ako se problem nastavi pojavljivati, obratite se Microsoft Support.|
|Pasivne kontroler neispravna.|Ne radi kontrolerom pasivni (sekundarni).|Uređaj je radu, ali jedno od vašeg kontrolera je neispravna. Pokušajte ponovno pokrenuti taj kontroler. Ako se problem ne riješi, obratite se Microsoft Support.|

### <a name="job-failure-alerts"></a>Zadatak pogreška upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Sigurnosno kopiranje <*ID grupe jedinica izvora*> nije uspjelo.|Sigurnosno kopiranje nije uspjelo.|Problema s povezivanjem sprečavaju uspješno dovršavanje operacije sigurnosne kopije. Ako nema problema s povezivanjem, možda Dostigli ste maksimalni broj kopija. Izbrišite sve sigurnosnih kopija koje više nisu potrebne, a zatim ponovite postupak. Nakon što ste snimili odgovarajuće akcije, poništite upozorenje na stranici upozorenja.|
|Kloniraj <*izvorni sigurnosne kopije element ID-a*> <*odredište glasnoću serijske brojeve*> nije uspjelo.|Zadatak Kloniraj nije uspio.|Osvježavanje popisa sigurnosnu kopiju da biste provjerili je li još uvijek valjan sigurnosnu kopiju. Ako vrijedi sigurnosnog kopiranja, moguće je problema s povezivanjem oblaka sprječavaju uspješno dovršavanje operacije Kloniraj. Ako nema problema s povezivanjem, možda ste dosegnuli ograničenje prostora za pohranu. Izbrišite sve sigurnosnih kopija koje više nisu potrebne, a zatim ponovite postupak. Nakon što ste snimili odgovarajuće akcije da biste riješili taj problem, poništite upozorenje na stranici upozorenja.|
|Vraćanje <*izvorni sigurnosne kopije element ID-a*> nije uspjelo.|Vraćanje posla nije uspjelo.|Osvježavanje popisa sigurnosnu kopiju da biste provjerili je li još uvijek valjan sigurnosnu kopiju. Ako vrijedi sigurnosnog kopiranja, moguće je problema s povezivanjem oblaka sprječavaju uspješno dovršavanje operacije vraćanja. Ako nema problema s povezivanjem, možda ste dosegnuli ograničenje prostora za pohranu. Izbrišite sve sigurnosnih kopija koje više nisu potrebne, a zatim ponovite postupak. Nakon što ste snimili odgovarajuće akcije da biste riješili taj problem, poništite upozorenje na stranici upozorenja.|

### <a name="locally-pinned-volume-alerts"></a>Lokalno prikvačene glasnoću upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Stvaranje lokalne jedinice <*naziv glasnoću*> nije uspjelo.| Nije uspjelo stvaranje zadatka glasnoću. <*Pogreške poruku koja odgovara kod pogreške nije uspjelo*>.|Problema s povezivanjem sprečavaju uspješno dovršavanje operacije stvaranja prostora. Lokalno prikvačene količine su thickly dodjeli i postupak stvaranja prostora obuhvaća ne prelaze tiered količine u oblak. Ako nema problema s povezivanjem, možda ste iscrpljen lokalne prostora na uređaju. Određuje razmak postoji li na uređaju provesti ovaj postupak.|
|Proširenje lokalnu jedinicu <*naziv glasnoću*> nije uspjelo.|Izmjena posao glasnoće nije uspjela zbog <*pogreške poruku koja odgovara kod pogreške nije uspjelo*>.|Problema s povezivanjem sprečavaju uspješno dovršavanje operacije proširenja glasnoću. Lokalno prikvačene jedinicama su thickly dodjeli i proširivanje postojeće prostora postupak obuhvaća ne prelaze tiered jedinicama u oblak. Ako nema problema s povezivanjem, možda ste iscrpljen lokalne prostora na uređaju. Određuje razmak postoji li na uređaju provesti ovaj postupak.|
|Pretvorba jedinice <*naziv glasnoću*> nije uspjela.|Zadatak pretvorbe jedinice da biste pretvorili vrste jedinice iz lokalno prikvačena na tiered nije uspjelo.|Pretvorba jedinice iz vrste lokalno prikvačene tiered nije moguće izvršiti. Provjerite ima li problema s povezivanjem sprječava uspješno dovršavanje operacije. Otklanjanje problema s povezivanjem potražite za [Otklanjanje poteškoća s cmdlet Test HcsmConnection](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Izvornu lokalno prikvačene glasnoću sada označena kao tiered glasnoću jer neke podatke iz lokalno prikvačene glasnoću sadrži spilled u oblak tijekom pretvorbe. Konačni tiered glasnoću je još uvijek zauzima lokalne prostora na uređaju koji ne može se povratiti za buduće lokalne jedinice.<br>Rješavanje problema s povezivanjem, izbrišite upozorenje i ponovno pretvoriti jedinicu Izvorna vrsta lokalno prikvačene jedinice da biste bili sigurni sve podatke postane dostupna lokalno ponovno.|
|Pretvorba jedinice <*naziv glasnoću*> nije uspjela.|Zadatak pretvorbe jedinice da biste pretvorili vrste jedinice iz tiered za lokalno prikvačene nije uspjelo.|Pretvorba jedinice iz vrste tiered za lokalno prikvačene nije moguće dovršiti. Provjerite ima li problema s povezivanjem sprječava uspješno dovršavanje operacije. Otklanjanje problema s povezivanjem potražite za [Otklanjanje poteškoća s cmdlet Test HcsmConnection](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Izvorni tiered jedinica označene kao lokalno prikvačene jedinica kao dio pretvorbe i dalje ima podataka residing u oblaku, u thickly dodijeljenu prostora na uređaju za ovu jedinicu više ne možete vraćeno za buduće lokalne jedinice.<br>Rješavanje problema s povezivanjem, izbrišite upozorenje i ponovno pretvoriti jedinicu Izvorna vrsta tiered jedinice da biste bili sigurni mogu se povratiti lokalne prostor thickly dodjeli na uređaju.|
|Nearing prostora na lokalnom utrošak lokalne snimke <*naziv grupe jedinica*>|Lokalni snimke sigurnosne kopije pravila moglo bi uskoro vam ponestati prostora i biti nevažeći da biste izbjegli pogreške pri pisanju glavnog računala.|Česti lokalne snimke duž churn najviših podataka u količine vezanu uz ovu grupu sigurnosne kopije pravila uzrokuju lokalne prostora na uređaju da biste brzo potrošena. Brisanje snimka lokalne koje više nisu potrebne. Osim toga, ažurirajte lokalne snimku rasporede tog sigurnosne kopije pravila da biste snimili manje Česti lokalne snimke i provjerite je li se redovito uzimaju oblaka snimke. Ako ne uzimaju se ove akcije, lokalni prostora za ove snimke možda uskoro iscrpljena i sustav automatski će izbrisati ih da biste bili sigurni da nastaviti uspješno obrađen zapisivanja glavnog računala.|
|Lokalni snimke <*naziv grupe jedinica*> ste je nevažeći.|Lokalni snimke <*naziv grupe jedinica*> su nevažeći i zatim izbrisati jer oni su premašuju lokalne prostora na uređaju.|Da biste bili sigurni to ne ponavlja u budućnosti, pregledajte lokalne snimke rasporede za ovo pravilo za sigurnosno kopiranje i brisanje snimka lokalne koje više nisu potrebne. Česti lokalne snimke duž churn najviših podataka u količine vezanu uz ovu grupu sigurnosne kopije pravila može uzrokovati lokalne prostora na uređaju da biste brzo potrošena.|
|Vraćanje <*izvorni sigurnosne kopije element ID-a*> nije uspjelo.|Zadatak obnavljanja nije uspjelo.|Ako ste lokalno prikvačene ili kombinacije lokalno prikvačeni i tiered jedinice u ovo pravilo za sigurnosne kopije, osvježite sigurnosne kopije popis da biste potvrdili da sigurnosno kopiranje je još uvijek valjan. Ako vrijedi sigurnosnog kopiranja, moguće je problema s povezivanjem oblaka sprječavaju uspješno dovršavanje operacije vraćanja. Lokalno prikvačene količine koje su vraćaju kao dio grupe snimke nemaju sve svoje podatke preuzeti na uređaj, a ako imate kombinacije tiered i lokalno prikvačene jedinice u ovoj grupi snimke, oni neće sinkronizirati s drugom. Da biste uspješno završili postupak vraćanja, iskoristite jedinice u ovoj grupi izvanmrežno na glavnom računalu i ponovite postupak vraćanja. Imajte na umu da sve izmjene glasnoću podataka koji su izvesti tijekom postupka Vrati se izgubiti.|

### <a name="networking-alerts"></a>Povezivanje s mrežom upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Nije moguće pokrenuti StorSimple servise.|DataPath pogreške |Ako se problem nastavi pojavljivati, obratite se Microsoft Support.|
|Duplicirane IP adresa za 'Data0'.| |Sustav je otkrio sukob IP adresa "10.0.0.1". Mrežni resurs 'Data0' na uređaju *<device1>* je izvan mreže. Provjerite je li da ovu IP adresu ne koristi drugi entitet u ovu mrežu. Otklanjanje poteškoća s mrežom, idite na [Otklanjanje poteškoća s cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Obratite se administratoru za pomoć pri otklanjanju taj problem. Ako se problem nastavi pojavljivati, obratite se Microsoft Support. |
|IPv4 (ili IPv6) adresu 'Data0' je izvan mreže.| |Mrežni resurs 'Data0' s IP adresom '10.0.0.1'. i duljina '22' na uređaju prefiksa *<device1>* je izvan mreže. Provjerite jesu li priključke prijelaz s kojim je povezan ovo sučelje radu li. Otklanjanje poteškoća s mrežom, idite na [Otklanjanje poteškoća s cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
 

### <a name="performance-alerts"></a>Performanse upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Mogućnost Učitaj uređaj premašila <*prag*>.|Manja od očekivanim vremena.|Vaš uređaj izvješća Upotreba pod velikim opterećenjem ulaza i izlaza. To može uzrokovati uređaj tako da ne funkcioniraju kao i bi trebao. Pregledajte opterećenjem ste joj pridružili uređaj i određivanje ako postoje bilo koje nije moguće premjestiti na drugi uređaj ili koje više nisu potrebne.<br>Da biste shvatili trenutni status, idite na članak [korištenje servisa StorSimple Upravitelj praćenje uređaju](storsimple-monitor-device.md)|

### <a name="security-alerts"></a>Sigurnosna upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Microsoft Support sesije je počeo.|Treće strane pristupiti podršku sesiju.|Potvrdite taj pristup ovlašten. Nakon što ste snimili odgovarajuće akcije, poništite upozorenje na stranici upozorenja.|
|Lozinku za <*element*> istječe <*trajanja*>.|Istek lozinke približava.|Promjena lozinke prije no što istekne.|
|Sigurnost informacije o konfiguraciji nedostaje za <*ID elementa*>.||Količine povezan s ovom spremniku glasnoće nije moguće koristiti za replikaciju konfiguraciju StorSimple. Da bi vaši podaci sigurno spremljeni, preporučujemo da izbrišete spremnik glasnoću i jedinice pridružene spremnik glasnoću. Nakon što ste snimili odgovarajuće akcije, poništite upozorenje na stranici upozorenja.|
|<*broj*> za <*ID elementa*> nije uspjela pokušaja prijave.|Pokušava više nije uspjelo prijave.|Vaš uređaj možda napada ili ovlaštenog korisnika pokušava povezati s netočnu lozinku.<ul><li>Obratite se ovlašteni korisnici i provjerite da su te prilikom pokušaja valjane izvora. Ako i dalje da biste vidjeli velik broj pokušaja prijave nije uspio, razmislite o onemogućivanju daljinsko upravljanje i obratite administratoru. Nakon što ste snimili odgovarajuće akcije, poništite upozorenje na stranici upozorenja.</li><li>Potvrdite okvir vašeg upravitelja snimke instance konfigurirane s ispravnu lozinku. Nakon što ste snimili odgovarajuće akcije, poništite upozorenje na stranici upozorenja.</li></ul>Dodatne informacije, prijeđite na odjeljak [Promjena programa lozinku istekle uređaju](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password).|
|Promjena ključa za šifriranje podataka servisa došlo je do pogreške na jednu ili više.||Došlo je do pogreške pri promjeni ključa za šifriranje podataka servisa. Kada ste riješili stanja pogreške, pokrenite na `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet putem sučelja za Windows PowerShell za StorSimple na uređaju da biste ažurirali servis. Ako se problem nastavi pojavljivati, obratite se Microsoftovoj podršci. Nakon što riješite problem, poništite upozorenje na stranici upozorenja.|

### <a name="support-package-alerts"></a>Podrška za paket upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Nije uspjelo stvaranje paketa za podršku.|StorSimple ne može uzrokovati paketa.|Ponovno pokušajte ovaj postupak. Ako se problem nastavi pojavljivati, obratite se Microsoft Support. Nakon što ste riješiti problem, poništite upozorenje na stranici upozorenja.|

### <a name="update-alerts"></a>Ažuriranje upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Hitni popravak instaliran.|Ažuriranje softver i opremu dovršeno.|Hitni popravak uspješno je instaliran na vašem uređaju.|
|Ručno dostupna ažuriranja.|Obavijest o dostupna ažuriranja.|Pomoću sučelja sustava Windows PowerShell za StorSimple na uređaju instalirati sljedeća ažuriranja. <br>Dodatne informacije potražite [ažuriranja StorSimple 8000 niz uređaja](storsimple-update-device.md).|
|Nova dostupna ažuriranja.|Obavijest o dostupna ažuriranja.|Možete instalirati sljedeća ažuriranja sa stranice za **Održavanje** ili pomoću sučelja sustava Windows PowerShell za StorSimple na uređaju. <br>Dodatne informacije potražite [ažuriranja StorSimple 8000 niz uređaja](storsimple-update-device.md).|
|Instalacija ažuriranja nije uspjelo.|Ažuriranja su uspješno instaliran.|Sustav nije uspio instalirati ažuriranja. Možete instalirati sljedeća ažuriranja sa stranice za **Održavanje** ili pomoću sučelja sustava Windows PowerShell za StorSimple na uređaju. Ako se problem nastavi pojavljivati, obratite se Microsoft Support. <br>Dodatne informacije potražite [ažuriranja StorSimple 8000 niz uređaja](storsimple-update-device.md).|
|Nije moguće automatski provjeri ima li novih ažuriranja.|Automatska provjera uspjela.|Možete ručno provjeriti ima li novih ažuriranja sa stranice za **Održavanje** .|
|Novi WUA agent dostupna.|Obavijest o dostupna ažuriranja.|Preuzmite najnovije Agent za Windows Update i instalirajte putem sučelja komponente Windows PowerShell.|
|Verzija komponente firmver <*ID komponente*> ne odgovara pomoću hardvera.|Firmver update(s) uspješno nisu instalirane.|Obratite se Microsoftovoj podršci.|

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [StorSimple pogreške i otklanjanje poteškoća radu uređaja](storsimple-troubleshoot-operational-device.md).

