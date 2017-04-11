<properties 
   pageTitle="Prikaz i upravljanje upozorenjima StorSimple virtualne polja | Microsoft Azure"
   description="U članku se opisuje upozorenja uvjeta StorSimple virtualne polja i težinu te kako koristiti StorSimple Upravitelj servisa za upravljanje upozorenjima."
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
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-alerts-for-the-storsimple-virtual-array"></a>Prikaz i upravljanje upozorenjima za polja virtualne StorSimple pomoću StorSimple Upravitelj servisa

## <a name="overview"></a>Pregled

Karticu **upozorenja** u servisu StorSimple Manager omogućuje pregled i poništite StorSimple virtualne polja vezanih uz upozorenja na temelju u stvarnom vremenu. Kartica, središnje je moguće nadzirati stanje problemi vaše StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualne uređaji) i ukupnog rješenja za Microsoft Azure StorSimple.

Pomoću ovog praktičnog vodiča opisuje kako konfigurirati upozorenja, uobičajenih uvjeta koji se upozorenja, a zatim upozorenja težinu razine te kako pregledati i praćenje upozorenja. Uz to, sadrži tablice upozorenja Brzi vodič koji vam omogućuju da brzo pronaći određene upozorenja i odgovaranje pravilno.

![Stranice upozorenja](./media/storsimple-ova-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Konfiguriranje postavki upozorenja

Možete odabrati želite li primati obavijesti putem e-pošte upozorenja uvjeti za svaku StorSimple virtualne uređaja. Osim toga, možete prepoznati drugim primateljima upozorenje tako da unesete njihove adrese e-pošte u okviru **drugim primateljima e-pošte** razdvojene točkom sa zarezom.

>[AZURE.NOTE] Možete unijeti najviše 20 adrese e-pošte po virtualnog uređaja.

Kada omogućite obavijesti e-pošte za virtualnog uređaja, članove popisa obavijesti primit će poruku e-pošte svaki put kada se dogodi Kritično upozorenje. Poruka će biti poslana s *storsimple-alerts-noreply@mail.windowsazure.com* i će opisuju upozorenja uvjet. Primatelji kliknite **Otkaži pretplatu** izuzimanje iz popisa obavijesti e-pošte.

#### <a name="to-enable-email-notification-of-alerts-for-a-virtual-device"></a>Da biste omogućili obavijesti e-pošte upozorenja za virtualnog uređaja

1. Otvorite odjeljak **uređaji** > **Konfiguracija** virtualnog uređaja. Prijeđite na odjeljak **Postavke upozorenja** .

    ![Postavke upozorenja](./media/storsimple-ova-manage-alerts/alerts2.png)

2. U odjeljku **Postavke upozorenja**postaviti sljedeće:

    1. U polju **obavijesti e-pošte za slanje** , odaberite **da**.

    2. U polje **Administratori servisa za e-pošte** odaberite **da** ako želite da se obratite administratoru za servis, a sve dodatnih administratora primati upozorenja obavijesti.

    3. U polju **drugim primateljima e-pošte** unesite adresu e-pošte svih primatelja koje bi trebali primiti upozorenje obavijesti. Unesite imena u obliku *someone@somewhere.com*. Za razdvajanje adrese e-pošte, koristite točke sa zarezom. Možete konfigurirati najviše 20 adrese e-pošte po virtualnog uređaja. 

        ![Konfiguriranje upozorenja obavijesti](./media/storsimple-ova-manage-alerts/alerts3.png)

3. Pri dnu stranice, kliknite **Spremi** da biste spremili konfiguraciju.

4. Da biste poslali test obavijest e-pošte, kliknite ikonu sa strelicom pokraj **test slanje e-pošte**. Servis za upravitelja StorSimple prikazat će se poruke o statusu kao prosljeđuje test obavijesti. 

5. Kada se prikaže sljedeća poruka, kliknite **u redu**. 

    ![Upozorenja testiranje obavijesti e-pošte poslane](./media/storsimple-ova-manage-alerts/alerts4.png)

    >[AZURE.NOTE] Ako se ne može poslati obavijest test, StorSimple Upravitelj servisa prikazat će odgovarajuću poruku. Kliknite **u redu**, pričekajte nekoliko minuta pa pokušajte ponovno poslati poruku obavijesti test. 

    Poruke s obavijesti test bit će otprilike ovako.

    ![Primjer e-pošte za testiranje upozorenja](./media/storsimple-ova-manage-alerts/alerts5.png)

## <a name="common-alert-conditions"></a>Uobičajenih uvjeta koji se upozorenja

Vaše StorSimple virtualne polja stvara upozorenja u odgovoru na različite uvjete. Ovo su najčešće upozorenja uvjeti:

- **Problema s povezivanjem** – te upozorenja pojaviti kada postoji težine za prijenos podataka. Problemi komunikacije može dogoditi tijekom prijenos podataka da biste i s računa za Azure prostora za pohranu zbog nedostatka veze između virtualne uređaje i StorSimple Upravitelj servisa. Problemi komunikacije su neke od Najteži da biste riješili problem jer je toliko točke nije uspjelo. Prvo uvijek treba provjeriti veza s mrežom i pristup Internetu su dostupni prije nastavka na više Napredno otklanjanje poteškoća. Informacije o priključci i postavke vatrozida, idite na [sistemski preduvjeti za StorSimple virtualne polja](storsimple-ova-system-requirements.md). Pomoć za otklanjanje poteškoća, idite na [Otklanjanje poteškoća s cmdlet Testiraj vezu](storsimple-troubleshoot-deployment.md).

- **Problemi s performansama** – te upozorenja su zbog odjeljaka sustav nije optimalnog, kao što su kada je pod velikim opterećenjem.

Osim toga, možda će se prikazati upozorenja vezana uz sigurnost, ažuriranja i neuspjelim.

## <a name="alert-severity-levels"></a>Razina za upozorenje težinu

Upozorenja imaju različite težinu razine, ovisno o utjecaj koji će imati upozorenja situacija i potrebe za odgovor na upozorenje. Razine težinu su:

- **Od ključne važnosti** – upozorenje je kao odgovor na uvjet koji je utjecaja uspješno performanse sustava. Akcija potreban je kako bi StorSimple servisa ne prekine.

- **Upozorenje** – ovaj uvjet može postati ključnih ako nije riješen. Trebali biste istražili situaciji i ništa poduzimati potrebne da biste očistili problem.

- **Informacije o** – upozorenje sadrži informacije koje se mogu poslužiti za praćenje i upravljanje sustavom.

## <a name="view-and-track-alerts"></a>Prikaz i praćenje upozorenja

Nadzorna ploča službe za Upravitelj StorSimple nudi sažet pregled broj upozorenja na virtualne uređaja, organizirani po razini težinu.

![Nadzorna ploča za upozorenja](./media/storsimple-ova-manage-alerts/alerts6.png)

Klikom na razinu težinu otvara karticu **upozorenja** . Rezultati obuhvatiti samo upozorenja koje odgovaraju te razine težinu.

![Upozorenja izvješća iz djelokruga vrstu upozorenja](./media/storsimple-ova-manage-alerts/alerts7.png)

Klikom na upozorenje na popisu pruža dodatne detalje upozorenja, uključujući vrijeme zadnje prijavljena je upozorenje, broja pojavljivanja upozorenje na uređaj i preporučene akcije da biste riješili upozorenje.

Ako morate poslati informacije tvrtki Microsoft Support, tekstne datoteke možete kopirati detalje upozorenja. Nakon što ste pratili na preporuke i riješiti na upozorenja uvjet lokalnog, treba očistiti upozorenje odabirom upozorenja na kartici **upozorenja** i klikom **poništite**. Da biste očistili višestruka upozorenja, odaberite svaki upozorenja, kliknite sve stupce osim stupaca **upozorenja** , a zatim **poništite** nakon što ste odabrali sva upozorenja tabulatore. Imajte na umu da su neke upozorenja automatski poništeni kada se problem ne riješi ili sustav ažurira upozorenje s novim podacima.

Kada kliknete **Očisti**, imat ćete mogućnost davanja komentare o upozorenje i koraci koje ste snimili da biste riješili taj problem. 

![upozorenja komentara](./media/storsimple-ova-manage-alerts/clear-alert.png)

Kliknite ikonu provjeri ![ikonu Provjera](./media/storsimple-ova-manage-alerts/check-icon.png) Da biste spremili svoje komentare.

Ako neki drugi događaj aktivira s novim podacima, sustav bit će uklonjena neke događaje. U tom slučaju prikazat će se sljedeća poruka.

![Poništite okvir upozorenjima](./media/storsimple-ova-manage-alerts/alerts8.png)

## <a name="sort-and-review-alerts"></a>Sortiranje i pregled upozorenja

Karticu **upozorenja** možete prikazati do 250 upozorenja. Ako ste premašili taj broj upozorenja, ne sva upozorenja prikazat će se u zadanom prikazu. Možete kombinirati sljedeća polja da biste prilagodili koja upozorenja:

- **Status** – možete prikazati **aktivna** ni **očišćeno** upozorenja. Aktivni upozorenja i dalje se aktiviraju u vašem sustavu dok očišćenog upozorenja su ili ručno poništen administrator ili programski poništen jer sustav upozorenja uvjet ažurirati novim podacima.

- **Težinu** – možete prikazati upozorenja sve razine težinu (ključnih, upozorenje, informacije o) ili samo s određenim težinu, kao što su samo ključnih upozorenja.

- **Izvor** – prikaz upozorenja iz svih izvora ili ograničili upozorenja onima koje se isporučuju sa servisom ili jedne ili svih virtualne uređaja.

- **Vremenski raspon** – navođenjem **od** i **do** datuma i vremenske oznake, možete pogledati upozorenja tijekom vremenskog razdoblja koja vas zanima.

## <a name="alerts-quick-reference"></a>Brzi pregled upozorenja

U sljedećim tablicama navedeni neka upozorenja Microsoft Azure StorSimple koje biste mogli naići, kao i dodatnih informacija i preporuke tamo gdje je dostupno. Upozorenja virtualnog uređaja StorSimple nalaziti u jednu od sljedećih kategorija:

- [Oblak povezivanje upozorenja](#cloud-connectivity-alerts)

- [Konfiguriranje upozorenja](#configuration-alerts)

- [Zadatak pogreška upozorenja](#job-failure-alerts)

- [Performanse upozorenja](#performance-alerts)

- [Sigurnosna upozorenja](#security-alerts)

- [Ažuriranje upozorenja](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Oblak povezivanje upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Uređaj *<device name>* nije povezan s oblakom.|Imenovani uređaj ne može povezati s oblakom. |Nije moguće povezati s oblakom. Razlog može biti nešto od sljedećeg:<ul><li>Možda postoji problem s postavkama mrežom na uređaju.</li><li>Možda postoji problem s računa vjerodajnice za pohranu.</li></ul>Dodatne informacije o otklanjanju poteškoća s povezivanjem, idite na [lokalnom web korisničkog Sučelja](storsimple-ova-web-ui-admin.md) uređaja.|


### <a name="configuration-alerts"></a>Konfiguriranje upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Konfiguracija lokalnog virtualnog uređaja nije podržana.|Slabe performanse.|Trenutna konfiguracija može uzrokovati smanjene performanse performanse. Provjerite ispunjava li vaš poslužitelj preduvjete minimalne konfiguracije. Dodatne informacije potražite na [StorSimple virtualne preduvjeti za polja](storsimple-ova-system-requirements.md). 
|Su dovoljno prostora na disku dodijeljenu <*naziv uređaja*>.|Upozorenje prostora na disku.|Vam ponestaje prostora na disku Dodjela resursa. Da biste oslobodili prostor, razmislite o premještanju radnih opterećenja drugi glasnoću ili zajednički mrežni resurs ili brisanje podataka.

### <a name="job-failure-alerts"></a>Zadatak pogreška upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Nije moguće završiti sigurnosnu kopiju <*naziv uređaja*>.|Sigurnosno kopiranje nije uspjelo.|Nije moguće stvoriti sigurnosnu kopiju. Imajte na umu nešto od sljedećeg:<ul><li>Problema s povezivanjem sprečavaju uspješno dovršavanje operacije sigurnosne kopije. Provjeriti postoje li bez problema s povezivanjem. Dodatne informacije o otklanjanju poteškoća s povezivanjem, idite na [lokalnom web korisničkog Sučelja](storsimple-ova-web-ui-admin.md) za virtualnog uređaja.</li><li>Dosegnuli ste ograničenje dostupan prostor za pohranu. Da biste oslobodili prostor, razmislite o brisanju sigurnosno kopiranje s koje više nisu potrebne.</li></ul> Riješiti probleme, poništite upozorenja, a zatim ponovite postupak.|
|Vraćanje <*naziv uređaja*> nije moguće završiti.|Vraćanje posla nije uspjelo.|Nije moguće vratiti iz sigurnosne kopije. Imajte na umu nešto od sljedećeg:<ul><li>Popis sigurnosne kopije ne moraju biti valjane. Osvježavanje popisa da biste provjerili je još uvijek valjan.</li><li>Problema s povezivanjem sprečavaju uspješno dovršavanje operacije vraćanja. Provjeriti postoje li bez problema s povezivanjem.</li><li>Dosegnuli ste ograničenje dostupan prostor za pohranu. Da biste oslobodili prostor, razmislite o brisanju sigurnosno kopiranje s koje više nisu potrebne.</li></ul>Rješavanje problema, izbrišite upozorenje i ponovite postupak vraćanja.|
|Nije moguće završiti Kloniraj <*naziv uređaja*>.|Kloniraj posla nije uspjelo.|Nije moguće stvoriti u Kloniraj. Imajte na umu nešto od sljedećeg:<ul><li>Popis sigurnosne kopije ne moraju biti valjane. Osvježavanje popisa da biste provjerili je još uvijek valjan.</li><li>Problema s povezivanjem sprečavaju uspješno dovršavanje operacije Kloniraj. Provjeriti postoje li bez problema s povezivanjem.</li><li>Dosegnuli ste ograničenje dostupan prostor za pohranu. Da biste oslobodili prostor, razmislite o brisanju sigurnosno kopiranje s koje više nisu potrebne.</li></ul>Rješavanje problema, izbrišite upozorenje i ponovite postupak.|

### <a name="performance-alerts"></a>Performanse upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Nailazite na neočekivano kašnjenja u prijenos podataka.|Prijenos sporo podataka.|Regulacije pogreške pojaviti ako premašite odredišta skalabilnost servis za pohranu. Servis za pohranu ne da biste bili sigurni da nema jednog klijenta ili klijent može koristiti servis štetu drugim korisnicima. Dodatne informacije o otklanjanju poteškoća s računa za Azure prostora za pohranu, idite na [Monitor, dijagnosticiranje, i otklanjanje poteškoća s spremište na platformi Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).
|Vam ponestaje lokalne rezervacija prostora na disku <*naziv uređaja*>.|Reakcija sporo.|10% od ukupnu veličinu Dodjela resursa za <*naziv uređaja*> rezervirana na lokalnim uređajem i vam sada ponestaje rezervirane prostora. Radno opterećenje <*naziv uređaja*> generira veći rata churn ili možda ste nedavno prenijeli ste veliku količinu podataka. To može uzrokovati smanjene performanse. Imajte na umu nešto od sljedećeg da biste riješili taj problem:<ul><li>Povećanje propusnosti oblaka za ovaj uređaj.</li><li>Smanjivanje ili premjestite radnih opterećenja drugi glasnoću ili zajednički mrežni resurs.</li></ul>

### <a name="security-alerts"></a>Sigurnosna upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Lozinku za <*naziv uređaja*> istječe <*broj*> dana.|Upozorenje lozinku.| Lozinku istječe < broj < dana. Razmislite o promjeni lozinke. Dodatne informacije, prijeđite na odjeljak [Promjena administratorsku lozinku za uređaj StorSimple virtualne polja](storsimple-ova-change-device-admin-password.md).

### <a name="update-alerts"></a>Ažuriranje upozorenja

|Tekst upozorenja|Događaja|Dodatne informacije o / preporučene akcije|
|:---|:---|:---|
|Nova ažuriranja su dostupne za svoj uređaj.|Dostupni su ažuriranja polja virtualne StorSimple.|Nova ažuriranja možete instalirati sa stranice za **Održavanje** .|
|Pregledajte ima li novih ažuriranja <*naziv uređaja*> nije moguće.|Ažuriranje nije uspjelo. |Pojavila se pogreška prilikom instalacije novih ažuriranja. Možete ručno instaliranje ažuriranja. Ako se problem nastavi pojavljivati, obratite se [Microsoftovoj službi za podršku](storsimple-contact-microsoft-support.md).|


## <a name="next-steps"></a>Daljnji koraci

- [Dodatne informacije o StorSimple virtualne polja](storsimple-ova-overview.md).


