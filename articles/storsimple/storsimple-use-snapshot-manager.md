<properties 
   pageTitle="Upravitelj snimka StorSimple korisničko sučelje | Microsoft Azure"
   description="U članku se opisuje korisničko sučelje StorSimple snimke upravitelj i u članku se objašnjava kako ga koristiti za upravljanje zadacima sigurnosne kopije i katalog sigurnosne kopije."
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
   ms.workload="TBD"
   ms.date="04/25/2016"
   ms.author="v-sharos" />

# <a name="storsimple-snapshot-manager-user-interface"></a>Upravitelj snimku StorSimple korisničkog sučelja

## <a name="overview"></a>Pregled

Snimka Upravitelj StorSimple ima intuitivno korisničko sučelje koje možete koristiti da biste snimili i upravljanje sigurnosne kopije. Pomoću ovog praktičnog vodiča nudi Uvod u korisničkom sučelju i objašnjava kako koristiti svih komponenti. Detaljan opis programa StorSimple snimke Manager potražite u članku [što je upravitelj StorSimple snimke?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Opis konzole

Da biste pogledali korisničko sučelje, kliknite ikonu StorSimple snimku Upravitelj na radnoj površini. Prozor konzole prikazuje se kao što je prikazano na sljedećoj slici.

![Upravitelj snimka StorSimple okna](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

Prozor konzole ima pet glavnih elemenata. Kliknite odgovarajuću vezu za potpuni opis svaki element.

- [Traka izbornika](#menu-bar) 
- [Alatnoj traci](#tool-bar) 
- [Okno dosega](#scope-pane) 
- [Okno s rezultatima](#results-pane) 
- [Okno akcije](#actions-pane) 

Uz to, snimka Upravitelj StorSimple podržava [Navigacija pomoću tipkovnice i broj prečaca](#keyboard-navigation-and-shortcuts).

### <a name="console-accessibility"></a>Konzola za pristupačnost

Korisničko sučelje StorSimple snimke Upravitelj podržava značajke pristupačnosti koje ste dobili od operacijskog sustava Windows i Microsoft Management Console (BLOG), kao i neke StorSimple snimke Upravitelj – tipkovne prečace. 

- Opis značajke pristupačnosti sustava Windows, idite na [Tipkovni prečaci za Windows](https://support.microsoft.com/kb/126449). 

- Opis značajke pristupačnosti za BLOG, potražite u odjeljku [Pristupačnost za BLOG 3.0](https://technet.microsoft.com/library/cc766075.aspx)

- Opis značajke pristupačnosti StorSimple snimke upravitelja, potražite [i navigacije za tipkovni prečaci](#keyboard-navigation-and-shortcuts).

## <a name="menu-bar"></a>Traka izbornika

Na traku izbornika na vrhu prozora konzole sadrži [datoteku](#file-menu), [Akcije](#action-menu), [Prikaz](#view-menu), [Favoriti](#favorites-menu), [prozor](#window-menu)i izbornika [Pomoć](#help-menu) .

Kliknite bilo koju stavku na traci izbornika da biste vidjeli popis dostupnih naredbi na njemu. Sljedeći primjer prikazuje izbornik **Prikaz** koji je odabran na traku izbornika.

![Odaberete izbornik Prikaz](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Izbornik datoteka

Izbornik **datoteka** sadrži standardne naredbe za Microsoft Management Console (BLOG).

#### <a name="menu-access"></a>Pristup izborniku

Da biste prikazali izbornik **datoteka** , na traci izbornika kliknite **datoteka** . Pojavit će se sljedeći izbornik.

![Izbornik datoteka Upravitelj StorSimple snimku](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Izbornik opis

U sljedećoj tablici opisane stavke koje se pojavljuju na izborniku **datoteka** .

| Stavka izbornika | Opis |
|:----------|:-------------|
| Novi       | Kliknite **Novo** da biste stvorili novi konzole koji se temelji na StorSimple snimke upravitelju. |
| Otvaranje      | Kliknite **Otvori** da biste otvorili postojeću konzolu. |
| Spremi      | Kliknite **Spremi** da biste spremili trenutnu konzolu. |
| Spremanje u obliku   | Kliknite **Spremi kao** da biste stvorili novu, preimenovane pojavljivanja trenutnu konzolu. Pomoću mogućnosti za **Spremanje u obliku** Prilagodba prikaza i spremiti ga za dohvaćanje kasnije. Ako, na primjer, mogli biste stvoriti StorSimple snimke Upravitelj dodaci koji upućuju na poslužitelje određene. |
| Dodavanje i uklanjanje dodatka | Kliknite **Dodaj/ukloni dodatak** za dodavanje ili uklanjanje dodataka i organizirati čvorove u oknu **opseg** . Dodatne informacije potražite u članku [Dodavanje, uklanjanje, i dodataka za organiziranje](https://technet.microsoft.com/library/cc722035.aspx)i proširenja za BLOG 3.0. |
| Mogućnosti   | Kliknite **Mogućnosti** da biste promijenili ikonu konzole, navedite načini pristupa korisnika i dozvola, ili brisanje datoteka konzole da biste povećali slobodnog prostora na disku. |
| Popis datoteka | Kliknite put u numerirani popis da biste ponovno otvorite datoteku koju ste nedavno otvorili. |
| Izlaz      | Klikom na **Izlaz** da biste zatvorili izbornik **datoteka** . |
 
### <a name="action-menu"></a>Izbornik akcija

Korištenje izbornika za **Akcije** da biste odabrali s dostupne akcije. Stavke koje su dostupne na ovise o odabiru koje ste napravili u oknu **opseg** ili okno s **rezultatima** .

#### <a name="menu-access"></a>Pristup izborniku

Da biste prikazali izbornik **Akcija** , učinite nešto od sljedećeg:

- Desnom tipkom miša kliknite stavku u oknu **opseg** ili okno s **rezultatima** .

- Odaberite stavku u okno **opseg** ili okno s **rezultatima** , a zatim **Akcije** na traci izbornika. 

Na primjer, ako odaberete čvor najviše razine u oknu **opseg** , a zatim desnom tipkom miša kliknite ili na traci izbornika kliknite **Akcija** , pojavit će se sljedeći izbornik.
 
![Izbornik akcija Upravitelj StorSimple snimke](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

Okno **Akcije** (na desnoj strani konzole) sadrži isti popis akcije kao na izborniku **Akcije** . Uz to, okno **Akcije** sadrži izbornik mogućnosti **prikaza** koji omogućuju stvaranje prilagođenog prikaza okna s **rezultatima** .

![Okno akcije s prikazom otvorenim izbornikom](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Izbornik opis

Sljedeća tablica sadrži abecedni popis StorSimple snimku Upravitelj akcija. 

- U stupcu **Akcija** navedene akcije koje možete izvršiti na čvorovi i rezultata. 

- Stupac za **navigaciju** objašnjava da biste prikazali odgovarajući izbornik **Akcije** tako da odaberete željenu akciju. Neke akcije pojavljuju se u višestruke izbornike **Akcija** . Ove se radnje odaberite jednu mogućnost **navigacije** s popisa s grafičkim oznakama. 

- Stupac **opisa** u članku se opisuje kako koristiti svaku akciju na izborniku **Akcije** ili okno akcije i objašnjava što označava.

>[AZURE.NOTE] Okno **Akcije** i **Akcija** izbornici sadrže dodatne mogućnosti, kao što su **Prikaz**, **Novi prozor odavde**, **Osvježavanje**, **Izvoz popisa**i **Pomoć**. Te mogućnosti dostupne u sklopu na BLOG, a nisu određene StorSimple snimke upravitelju. Tablica sadrži opise od tih mogućnosti.
 
| Akcija  | Navigacija  | Opis  |
|:--------|:------------|:-------------|
| Autentičnost | Kliknite čvor **uređaji** pa desnom tipkom miša kliknite uređaj u oknu s **rezultatima** . | Kliknite **provjere autentičnosti** da biste unijeli lozinku koju ste konfigurirali za uređaj. |
| Kloniraj  | Proširite **Katalog**, proširite **Oblaka snimke**, kliknite datumskim sigurnosnu kopiju, a zatim jedinice u oknu **rezultata** . | Kliknite **Kloniraj** da biste stvorili kopiju snimku oblaka i spremite ga na mjesto koje odredite. |
| Konfiguriranje uređaja | Desnom tipkom miša kliknite čvor **uređaji** . | Kliknite **Konfiguriranje uređaja** da biste konfigurirali uređaju jedan ili više uređaja da biste se povezali glavno računalo za Windows. |
| Stvaranje sigurnosne kopije pravila | Učinite nešto od sljedećeg:<ul><li>Desnom tipkom miša kliknite **sigurnosne kopije pravila**.</li><li>Kliknite ili proširivanje **Grupa jedinica**i desnom tipkom miša kliknite grupu jedinica.</li><li>Kliknite ili proširivanje **Kataloga sigurnosnu kopiju**, a desnom tipkom miša kliknite grupu jedinica.</li></ul> | Kliknite **Stvaranje sigurnosne kopije pravila** da biste konfigurirali zakazano sigurnosno kopiranje za grupu jedinica. |
| Stvaranje grupe za glasnoću | Učinite nešto od sljedećeg:<ul><li>Kliknite čvor **količine** , a zatim desnom tipkom miša kliknite jedinicu u oknu **rezultata** .</li><li>Desnom tipkom miša kliknite čvor **Grupe jedinica** .</li></ul> | Kliknite **Stvori grupu jedinice** da biste dodijelili količine grupu jedinica. |
| Brisanje | Kliknite čvor ili rezultat (pojavljuje se na raznim izbornika **Akcije** i okna **Akcije** .) | Kliknite **Izbriši** da biste izbrisali čvor ili rezultat koji ste odabrali. Kada se pojavi dijaloški okvir za potvrdu, potvrda ili poništavanje brisanja. |
| Pojedinosti | Kliknite čvor **uređaji** , a zatim desnom tipkom miša kliknite uređaj u oknu s **rezultatima** . | Kliknite **Detalji** da biste vidjeli detalje konfiguracije za uređaj. |
| Uređivanje | Kliknite **Sigurnosne kopije pravila**, a zatim desnom tipkom miša kliknite pravila u oknu **rezultata** . | Kliknite **Uređivanje** da biste promijenili raspored sigurnosnog kopiranja za grupu jedinica. |
| Izvoz popisa | Kliknite bilo koju čvor ili rezultat (pojavljuje se na sve **Akcije** izbornici i **Akcije** okana.) | Kliknite **Izvoz popisa** da biste spremili popis u datoteku vrijednosti odvojenih zarezom (CSV). Zatim možete uvesti tu datoteku u aplikaciju za proračunske tablice radi analize. |
| Pomoć | Kliknite bilo koju čvor ili rezultat. (Pojavljuje se na sve **Akcije** izbornici i **Akcije** okana.) | Kliknite **Pomoć** da biste otvorili Mrežna pomoć u zasebnom prozoru preglednika. |
| Novi prozor odavde | Kliknite bilo koju čvor ili rezultat (pojavljuje se na sve **Akcije** izbornici i **Akcije** okana.) | Kliknite **Novi prozor odavde** da biste otvorili novi prozor Upravitelj StorSimple snimke.|
| Osvježavanje | Kliknite bilo koju čvor ili rezultat (pojavljuje se na sve **Akcije** izbornici i **Akcije** okana.) | Kliknite **Osvježi** da biste ažurirali trenutno prikazane prozora upravitelja StorSimple snimke. |
| Osvježi uređaja | Kliknite čvor **uređaji** pa desnom tipkom miša kliknite uređaj u oknu s **rezultatima** . | Kliknite **Osvježi uređaja** za sinkronizaciju uređaja povezanih s StorSimple snimke Upravitelj. |
| Osvježi uređaje | Desnom tipkom miša kliknite čvor **uređaji** . | Kliknite **Osvježi uređaja** za sinkronizaciju popis povezanih uređaja s StorSimple snimke Upravitelj. |
| Ponovni pregled količine | Desnom tipkom miša kliknite čvor **količine** . | Kliknite da biste ažurirali popis jedinicama koji se prikazuje u oknu s **rezultatima** **Ponovni pregled količine** . |
| Vraćanje | Proširite **Katalog**, proširivanje grupe glasnoće, proširite **Lokalne snimke** ili **Oblaka snimke**, a desnom tipkom miša kliknite sigurnosnu kopiju. | Kliknite **Vrati** da biste zamijenili trenutno glasnoću grupiranje podataka s podacima iz odabranog sigurnosne kopije. |
| Iskoristite sigurnosnog kopiranja | Učinite nešto od sljedećeg:<ul><li>Proširivanje **Grupa jedinica**pa desnom tipkom miša kliknite grupu jedinica.</li><li>Proširite **Kataloga sigurnosnu kopiju**, a desnom tipkom miša kliknite grupu jedinica.</li></ul> | Kliknite **Preuzimanje sigurnosne kopije** odmah pokrenuti sigurnosno kopiranje. |
| Uključivanje i isključivanje uvozi prikaza | Desnom tipkom miša kliknite čvor najviše razine u oknu **opseg** ( **StorSimple snimku Upravitelj** čvor u primjerima). | Kliknite **Uključi/Isključi uvozi prikaz** da biste prikazali ili sakrili glasnoću grupe i pridruženi sigurnosne kopije koji su uvezeni iz nadzorna ploča za StorSimple Upravitelj servisa. |

### <a name="view-menu"></a>Izbornik Prikaz

Korištenje izbornika za **Prikaz** da biste stvorili prilagođeni prikaz sadržaja okno **Rezultati** . **Prikaz** izbornika sadrži mogućnosti **Dodaj/Ukloni stupce** i **Prilagodba** .

#### <a name="menu-access"></a>Pristup izborniku

Možete pristupiti na izborniku **Prikaz** na traci izbornika ili u oknu **Akcije** .

![Izbornik Prikaz Upravitelj StorSimple snimke](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Izbornik opis

U sljedećoj tablici opisane stavke koje se pojavljuju na izborniku **Prikaz** .

| Stavka izbornika  | Opis |
|:-----------|:-------------|
| Dodavanje i uklanjanje stupaca | Kliknite **Dodaj/Ukloni stupce** da biste dodali ili uklonili stupce u oknu **rezultata** . |
| Prilagodba | Kliknite **Prilagodi** za prikaz ili skrivanje stavke u prozoru upravitelja snimka StorSimple konzolu. |

### <a name="favorites-menu"></a>Izbornik Favoriti

Izbornik **Favoriti** omogućuje dodavanje, uklanjanje i organiziranje prikaza stranice i zadacima koje često koristite. 

#### <a name="menu-access"></a>Pristup izborniku

Možete pristupiti na izborniku **Favoriti** na traci izbornika.

![Izbornik StorSimple snimke Upravitelj Favoriti](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Izbornik opis

U sljedećoj tablici opisane stavke koje se pojavljuju na izborniku **Favoriti** .

| Stavka izbornika |  Opis |
|:----------|:-------------|
| Dodaj u favorite | Kliknite **Dodaj u favorite** da biste dodali trenutni prikaz na popis favorita. |
| Organiziranje favorita | Kliknite **Organiziraj favorite** da biste organizirali sadržaj mapi Favoriti. |

### <a name="window-menu"></a>Izbornik prozor

Korištenje izbornika za **prozor** za dodavanje i slaganje StorSimple snimke Upravitelj konzole za windows.

#### <a name="menu-access"></a>Pristup izborniku

Možete pristupiti na izborniku **prozora** na traci izbornika.

![Izbornik prozora upravitelja StorSimple snimke](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

Numerirani popis pri dnu izbornika prikazuje windows koji su trenutno otvoriti. Kliknite bilo koji prozora na popisu da biste otvorili prozor u prvom planu. 

#### <a name="menu-description"></a>Izbornik opis

U sljedećoj tablici opisane stavke koje se pojavljuju na izborniku prozor.

| Stavka izbornika  | Opis |
|:-----------|:-------------|
| Novi prozor | Kliknite **Novi prozor** da biste otvorili novi prozor konzole (osim postojeći prozor). |
| Kaskadno   | Kliknite **Kaskadno** da biste prikazali windows otvorene konzole oblikovanih stilom. |
| Popločaj vodoravno | Kliknite **Poploči vodoravno** da biste prikazali windows konzole za otvaranje u obliku pločica (ili rešetke). |
| Rasporedi ikone | Ako imate više konzole za windows otvorite i raspršeni na radnoj površini minimiziranje ih, a zatim kliknite **Rasporedi ikone** rasporedite u vodoravni retka na dnu zaslona. |

### <a name="help-menu"></a>Izbornik Pomoć

Korištenje izbornika **Pomoć** da biste vidjeli dostupne Mrežna pomoć za Upravitelj snimka StorSimple i na BLOG. Možete pogledati i informacije o verzijama softver BLOG i StorSimple snimke Upravitelj koji su trenutno instalirani na računalu. 

Možete pristupiti na izborniku **Pomoć** na traci izbornika. Teme pomoći za upravitelja snimka StorSimple možete pristupiti i iz okna **Akcije** .

![Pomoć za Upravitelj StorSimple snimka izbornika](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Izbornik opis

U sljedećoj tablici opisane stavke koje se pojavljuju na izborniku Pomoć.

| Stavka izbornika  | Opis  |
|:-----------|:-------------|
| Pomoć za Upravitelj StorSimple snimke | Kliknite **Pomoć na StorSimple snimku Upravitelj** da biste otvorili Upravitelj snimku StorSimple pomoć u zasebnom prozoru. |
| Teme pomoći |Kliknite **Pomoć** da biste otvorili BLOG Mrežna pomoć u zasebnom prozoru. |
| TechCenter Web-mjesto | Pritisnite **TechCenter Web-mjesto** da biste otvorili početnu stranicu Tech Center za Microsoft TechNet u zasebnom prozoru. |
| O Microsoft Management Console | Kliknite **O Microsoft Management Console** da biste vidjeli koju verziju sustava Microsoft Management Console je instaliran na računalu. |
| O upravitelju za StorSimple snimke | Kliknite **O StorSimple snimke Upravitelj** da biste vidjeli koju verziju dodatak je instaliran na računalu. |

## <a name="tool-bar"></a>Alatnoj traci

Na alatnoj traci koja se nalazi ispod trake izbornika sadrži ikone navigacije i zadatak. Svakog ikona je prečac do određenog zadatka.

### <a name="icon-descriptions"></a>Opisi ikona

U sljedećoj su tablici opisani ikone koje se pojavljuju na alatnoj traci. 

| Ikona  | Opis  |
|:------|:-------------| 
| ![Strelica lijevo](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) | Kliknite ikonu strelica lijevo da biste se vratili na prethodnu stranicu. |
| ![Strelica desno](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) | Kliknite strelicu desno da biste prešli na sljedeću stranicu (Ako je strelicu sivo, akcije nije dostupna). |
| ![Ikona gore](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) | Kliknite ikonu prema gore da biste se jednu razinu u stablu konzole (okno **opseg** ). |
| ![Prikaži/Sakrij stablu konzole](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) | Kliknite ikonu stabla konzole za prikaz/skrivanje da biste prikazali ili sakrili okno **opseg** . |
| ![Izvoz popisa](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) | Kliknite ikonu za izvoz popisa da biste izvezli popis u CSV datoteku koju navedete. |
| ![Ikona pomoći](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png)  |Kliknite ikonu pomoć da biste otvorili online teme pomoći BLOG. |
| ![Prikaži/sakrij okno akcije](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) | Kliknite Pokaži/sakrij okno ikona za **Akcije** da biste prikazali ili sakrili okno **Akcije** . 
 
## <a name="scope-pane"></a>Okno dosega

Okno **opseg** je krajnjem lijevom oknu StorSimple snimke Upravitelj korisničkog Sučelja. Ono sadrži stablu konzole (ili čvor) te je mehanizam primarnu navigaciju za StorSimple snimke Manager. 
 
### <a name="scope-pane-structure"></a>Opseg okno strukture

Okno **opseg** sadrži niz kliknuti objekte (čvorove) organizirane u strukturi stabla. 

![Okno dosega](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

- Da biste proširili ili saželi čvor, kliknite ikonu sa strelicom pokraj naziva čvor.

- Da biste vidjeli status ili sadržaj čvor, kliknite čvor naziva. Pojavit će se podaci u oknu **rezultata** . 

U oknu **opseg** sadrži čvorove sljedeće: 

- [Čvor uređaji](#devices-node) 
- [Količine čvor](#volumes-node) 
- [Čvor grupe jedinica](#volume-groups-node) 
- [Čvor sigurnosne kopije pravila](#backup-policies-node) 
- [Sigurnosno kopiranje čvor kataloga](#backup-catalog-node) 
- [Čvor poslove](#jobs-node) 

### <a name="scope-pane-tasks"></a>Opseg okna zadataka

Da biste dovršili akcije na određeni čvor možete koristiti okno **opseg** . Da biste odabrali zadatka, učinite nešto od sljedećeg:

- Desnom tipkom miša kliknite čvor, a zatim na izborniku koji će se prikazati odaberite zadatak.

- Kliknite čvor, a zatim na traci izbornika kliknite **Akcija** . Na izborniku koji će se prikazati odaberite zadatak.

- Kliknite čvor, a zatim u oknu **Akcije** odaberite akciju.

Kada odaberete čvor i koristite neku od ovih metoda da biste vidjeli popis zadataka, prikazuju se samo akcije koje možete izvršiti na tom čvor.

### <a name="devices-node"></a>Čvor uređaji

Čvor **uređaji** predstavlja StorSimple uređaji i StorSimple virtualne uređaje koji su povezani StorSimple snimke upravitelju. Odaberite čvor da bi povezao i konfiguriranje uređaja i uvezite njegove pridružene količine, grupe količine i postojeće sigurnosne kopije. Glavno računalo za jednu moguće je povezati više uređaja.

- Da biste proširili čvor, kliknite ikonu sa strelicom pokraj **uređaja**.

- Da biste vidjeli izbornika dostupnih akcija, desnom tipkom miša kliknite čvor **uređaja** ili desnom tipkom miša kliknite bilo koji od čvorove koje se prikazuju u prikazu prošireno.

- Da biste vidjeli popis konfiguriran uređaja, kliknite **uređaji** u oknu **opseg** . Na popisu uređaja, zajedno s informacijama o svakom uređaju se prikazuje u oknu **rezultata** .

### <a name="volumes-node"></a>Količine čvor

Čvor **količine** predstavlja pogona koji odgovaraju jedinicama postavljen tako da glavno računalo, uključujući one otkrio putem iSCSI i one otkrio pomoću uređaja. Koristite ovaj čvor da biste pogledali popis raspoložive jedinice i grupama jedinica dodijeliti pojedinačne količine.

- Da biste proširili čvor, kliknite ikonu sa strelicom uz **jedinicama**.

- Da biste vidjeli izbornika dostupnih akcija, desnom tipkom miša kliknite čvor **količine** ili desnom tipkom miša kliknite bilo koji od čvorove koje se prikazuju u prikazu prošireno.

- Da biste vidjeli popis jedinica, kliknite **količine** u oknu **opseg** . Pojavit će se popis količine, zajedno s informacije o svakoj jedinici u oknu **rezultata** .

### <a name="volume-groups-node"></a>Čvor grupe jedinica

Grupa jedinica se nazivaju i dosljednost grupe. Svaki glasnoću grupa je skup vezane uz aplikacije količine koji olakšava osigurati dosljednost aplikacije tijekom operacije sigurnosnog kopiranja. Pomoću čvor **Glasnoću grupe** da biste konfigurirali te grupe i ponijeti interaktivne sigurnosno kopiranje ili stvorite sigurnosnu kopiju rasporeda. 

- Da biste proširili čvor, kliknite ikonu strelicu pokraj **Grupe jedinica**.

- Da biste vidjeli izbornika dostupnih akcija, desnom tipkom miša kliknite čvor **Glasnoću grupe** ili desnom tipkom miša kliknite bilo koji od čvorove koje se prikazuju u prikazu prošireno.

- Da biste vidjeli popis grupa glasnoće, kliknite **Grupe jedinica** u oknu **opseg** . Pojavit će se popis grupa glasnoće, zajedno s informacije o svakoj grupi jedinica u oknu **rezultata** .

### <a name="backup-policies-node"></a>Čvor sigurnosne kopije pravila

Sigurnosne kopije pravila su rasporedima zadataka za lokalno i brze snimke u oblaku. Korištenje čvor **Sigurnosne kopije pravila** da biste odredili koliko često se stvara sigurnosnu kopiju i koliko sigurnosnu kopiju mora biti zadržani. 

- Da biste proširili čvor, kliknite ikonu sa strelicom pokraj **Sigurnosne kopije pravila**.

- Da biste vidjeli izbornika dostupnih akcija, desnom tipkom miša kliknite čvor **Sigurnosne kopije pravila** ili desnom tipkom miša kliknite bilo koji od čvorove koje se prikazuju u prikazu prošireno.

- Da biste vidjeli popis sigurnosne kopije pravila, kliknite **Sigurnosne kopije pravila** u oknu **opseg** . Pojavit će se popis sigurnosne kopije pravila, zajedno s informacijama o svakom pravila u oknu **rezultata** .

>[AZURE.NOTE] Možete zadržati najviše 64 sigurnosne kopije.


### <a name="backup-catalog-node"></a>Sigurnosno kopiranje čvor kataloga

Čvor **Sigurnosne kopije kataloga** sadrži popis on-site i službenoj sigurnosne kopije Azure StorSimple količine. Čvor organizirani po jedinici grupi, a svakog spremnika grupe jedinica sadrži zasebne strukture za lokalni snimke (s čvor **Lokalne snimka**) i oblaka snimke (čvor **Oblaka snimke** ). Kada se Proširi, svakog spremnika grupe jedinica popis svih uspješno kopija poduzete interaktivno ili konfigurirano pravilima.

- Da biste proširili čvor, kliknite ikonu sa strelicom uz **Katalog sigurnosne kopije**.

- Da biste vidjeli izbornika dostupnih akcija, desnom tipkom miša kliknite čvor **Kataloga sigurnosno kopiranje** ili desnom tipkom miša kliknite bilo koji od čvorove koje se prikazuju u prikazu prošireno.

- Da biste vidjeli popis sigurnosne kopije snimke, kliknite **Katalog** u oknu **opseg** . Pojavit će se popis snimke, zajedno s informacijama o svakom snimke u oknu **rezultata** .

### <a name="local-snapshots-node"></a>Lokalni čvor snimke

Čvor **Lokalne snimke** popis lokalne snimaka određene glasnoću grupe. Čvor nalazi se u odjeljku čvor **Sigurnosne kopije kataloga** u oknu **opseg** . Lokalni snimke su točke u vrijeme kopije glasnoću podataka pohranjenih na uređaju Azure StorSimple. Obično ovu vrstu sigurnosne kopije možete biti stvoren sustavu i brzo vratiti. Lokalni snimke stanja možete koristiti kao lokalnu sigurnosnu kopiju.

- Da biste proširili čvor, kliknite ikonu sa strelicom uz **Lokalni snimke**.

- Da biste vidjeli izbornika dostupnih akcija, desnom tipkom miša kliknite čvor **Lokalne snimke** ili desnom tipkom miša kliknite bilo koji od čvorove koje se prikazuju u prikazu prošireno.

- Da biste vidjeli popis lokalne snimke, kliknite **Lokalne snimke** u oknu **opseg** . Pojavit će se popis snimke, zajedno s informacijama o svakom snimke u oknu **rezultata** .

### <a name="cloud-snapshots-node"></a>Oblak snimke čvor

Čvor **Oblaka snimke** popis snimke oblaka za grupu određene glasnoću. Čvor nalazi se u odjeljku čvor **Sigurnosne kopije kataloga** u oknu **opseg** . Brze snimke oblaka su točke u vrijeme kopije glasnoću podatke pohranjene u oblaku. Oblak snimku jednako snimku replicirati na drugu, službenoj prostora za pohranu. Brze snimke oblaka su osobito je korisna u slučajevima Izrada oporavak.

- Da biste proširili čvor, kliknite ikonu sa strelicom pokraj **Oblaka snimke**.

- Da biste vidjeli izbornika dostupnih akcija, desnom tipkom miša kliknite čvor **Oblaka snimke** ili desnom tipkom miša kliknite bilo koji od čvorove koje se prikazuju u prikazu prošireno.

- Da biste vidjeli popis oblaka snimke, kliknite **Oblaka snimke** u oknu **opseg** . Pojavit će se popis snimke, zajedno s informacijama o svakom snimke u oknu **rezultata** .

### <a name="jobs-node"></a>Čvor poslove

Čvor **poslove** sadrži informacije o zakazani, pokrenuti i nedavno dovršene zadatke sigurnosne kopije. 

- Da biste proširili čvor, kliknite ikonu sa strelicom pokraj **zadatke**.

- Da biste vidjeli izbornika dostupnih akcija, desnom tipkom miša kliknite čvor **zadataka** ili desnom tipkom miša kliknite bilo koji od čvorove koje se prikazuju u prikazu prošireno.

- Da biste vidjeli popis Zakazani zadaci, proširite čvor **zadatke** , a zatim **zakazano**. Pojavit će se popis prethodno konfigurirane zadaci i informacije o svaki zadatak u oknu **rezultata** . 

- Da biste vidjeli popis nedavno dovršene zadatke, proširite čvor **zadatke** , a zatim **posljednje 24 sata**. U oknu s **rezultatima** pojavit će se popis zadataka koji su dovršeni u zadnja 24 sata. Okno s **rezultatima** sadrži i podatke o svakom dovršeni zadatak.

- Da biste vidjeli popis zadataka koji su trenutno pokrenuti, proširite čvor **zadatke** , a zatim **pokretanje**. Pojavit će se trenutno izvode zadaci i informacije o svaki zadatak na popisu u oknu **rezultata** .

## <a name="results-pane"></a>Okno s rezultatima

Okno s **rezultatima** je središnjeg okna u korisničkom Sučelju Upravitelj StorSimple snimke. Sadrži popise i detaljno stanje informacije za odabrani u oknu **opseg** čvor.

### <a name="example"></a>Primjer

Da biste vidjeli u sljedećem primjeru, kliknite čvor **Glasnoću grupe** u oknu **opseg** . Okno s **rezultatima** prikazuje popis grupa jedinica s pojedinostima o svake grupe.

![Okno s rezultatima](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

Možete konfigurirati pojedinosti prikazane u oknu s **rezultatima** : desnom tipkom miša kliknite čvor u oknu **opseg** , kliknite **Prikaz**, a zatim kliknite **Dodaj/Ukloni stupce**.

## <a name="actions-pane"></a>Okno akcije

Okno **Akcije** je u desnom oknu u korisničkom Sučelju StorSimple snimke Upravitelj. Sadrži izbornik operacije koje možete izvršiti na čvor, prikaza ili podatke koji ste odabrali u oknu **opseg** ili okno s **rezultatima** . Okno **Akcije** sadrži naredbe isti kao izbornika **Akcije** koje su dostupne za stavke u oknu **opsega** i okno s **rezultatima** . Opis svaku akciju, potražite u tablici u odjeljku izbornik **Akcija** .

### <a name="examples"></a>Primjeri

Da biste vidjeli sljedećem primjeru, u oknu **opseg** Proširite čvor **zadatke** , a zatim kliknite **zakazano**. Okno **Akcije** prikazuje dostupne akcije za **zakazano** čvor.

![Primjer za planirane poslove okno akcije](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

Da biste vidjeli dodatne mogućnosti, u oknu **opseg** Proširite čvor **poslova** , kliknite **zakazano**, a zatim zakazani zadatak u oknu **rezultata** . Okno **Akcije** prikazuje dostupne akcije zakazani posla, kao što je prikazano u sljedećem primjeru.

![Akcije okno zadatka akcije primjer](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Navigacija pomoću tipkovnice i prečaca

Upravitelj snimka StorSimple omogućuje značajke pristupačnosti operacijskog sustava Windows i Microsoft Management Console (BLOG). Uključuje i neke značajke navigacije tipkovnice i prečaci specifične za StorSimple snimke upravitelja, kao što je opisano u sljedećim odjeljcima.
 
- [Tipke za navigaciju tipkovnice](#keyboard-navigation-keys) 
- [Izbornik traka tipke prečaca](#menu-bar-shortcut-keys) 
- [Tipkovni prečaci za okno dosega](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Tipke za navigaciju tipkovnice

U sljedećoj tablici opisane su tipke koje možete koristiti za navigaciju StorSimple snimke Upravitelj korisničkog sučelja. 

| Tipke za navigaciju  | Akcija  |
|:----------------|:--------| 
| Pritisnutu tipku sa strelicom | Pomoću strelice dolje da biste premjestili okomito na sljedeću stavku na izborniku ili u oknu. |
| Unesite | Pritisnite tipku Enter da biste dovršili akciju, a zatim prijeđite na sljedeći korak. Ako, na primjer, možete pritisnuti Enter **sljedeće**, **u redu**ili **Stvori**, a zatim prijeđite na sljedeći korak u čarobnjaku za web.|
| ESC | Pritisnite tipku Esc da biste zatvorili izbornik ili da biste odustali od promjene i zatvorite stranicu.|
| F1 | Da biste pogledali teme pomoći za trenutno aktivni prozor, pritisnite tipku F1.|
| F5 | Da biste osvježili čvor, pritisnite tipku F5. |
| F6 | Pritisnite tipku F6 da biste premjestili iz okna **opseg** okno s **rezultatima** .|
| F10 | Da biste prešli na traku izbornika, pritisnite tipku F10. |
| Tipke strelica lijevo | Pomoću tipke strelica lijevo da biste premjestili vodoravno iz mogućnost traku izbornika na prethodnu mogućnost. Kada se premjestili na prethodnu stavku na traci izbornika, pojavit će se izbornik Akcije (ili kontekst) za prethodne stavke. |
| Tipke strelica desno | Pomoću tipke strelica desno da biste premjestili vodoravno iz jednog izbornika traka mogućnost sljedeće. Kada se premjestili na sljedeću stavku na traci izbornika, pojavit će se na izborniku Akcije (ili kontekst) za novu stavku.
| Tipka TAB | Da biste premjestili u sljedeće okno na konzoli sustava ili sljedeću odabira ili tekstni okvir na stranici pomoću tipke tabulatora. |
| Strelica gore | Koristite tipke sa strelicom gore da biste premjestili okomito na prethodnu stavku na izborniku ili okna. |

### <a name="menu-bar-shortcut-keys"></a>Izbornik traka tipke prečaca

U sljedećoj tablici opisane kombinacije tipkovnih prečaca za traku izbornika. Nakon pritisnite tipke prečaca, a zatim na izborniku otvori, možete koristiti izbornik tipkovni prečaci (podcrtano tipke na izborniku). Dodatne informacije o traku izbornika, idite na [traku izbornika](#menu-bar).

| Prečac | Rezultat                    | Izbornik prečac | Rezultat          |
|:---------|:--------------------------|:------------------|:----------------|
| ALT + F    | Otvorit će se izbornik **datoteka** .  | N | Otvorit će se nove instance konzolu.   |
|          |                           | O | Otvorit će se stranica **Administrativni alati** . |
|          |                           | S | Sprema konzole za Upravitelj StorSimple snimke.|
|          |                           | ODGOVORA | Otvorit će se stranica **Spremanje u obliku** . |
|          |                           | M | Otvorit će se stranica **Dodavanje/uklanjanje dodatka** .|
|          |                           | P | Otvorit će se stranica **Mogućnosti** . |
|          |                           | H | Otvorit će se Mrežna pomoć.|
| ALT + A    | Otvorit će se izbornik **Akcija** .| Li | Uključivanje i isključivanje uključit će mogućnost prikaza za uvoz.|
|          |                           | W | Otvorit će se novi Upravitelj snimka StorSimple konzole.|
|          |                           | F | Ažurira konzole za Upravitelj StorSimple snimku.|
|          |                           | L | Otvorit će se na stranicu **Izvoz popisa** . 
|          |                           | H | Otvorit će se Mrežna pomoć.|
| ALT + V    | Otvorit će se izbornik **Prikaz** .  | ODGOVORA | Otvorit će se stranica **Dodaj/Ukloni stupce** . |
|          |                           | U | Otvorit će se stranica **Prilagodba prikaza** . |
| ALT + O    | Otvorit će se izbornik **Favoriti** . | ODGOVORA | Otvorit će se stranica **Dodaj u favorite** . |
|          |                           | O | Otvorit će se stranica za **Organiziranje favorita** .|
| ALT + W    | Otvorit će se na izborniku **prozor** .| N | Otvara drugi prozor Upravitelj StorSimple snimke.|
|          |                           | C | Prikazuje sve otvorene konzole windows oblikovanih stilom.|
|          |                           | T | Prikazuje sve otvorene konzole windows uzorak rešetke. |
|          |                           | Li | Slaže ikone u vodoravnom redu pri dnu zaslona.|
| ALT + H    | Otvorit će se izbornik **Pomoć** .  | H | Otvorit će se Mrežna pomoć.|
|          |                           | T | Otvorit će se web-stranicu Tech Center za Microsoft TechNet.|
|          |                           | ODGOVORA | Otvorit će se na stranicu **O Microsoftovu konzolu za upravljanje** . |
 
### <a name="scope-pane-shortcut-keys"></a>Tipkovni prečaci za okno dosega

U sljedećoj tablici prikazane prečaca kombinacije tipki za svaki čvor u oknu **opseg** . 

- [Tipkovni prečaci za uređaje čvor](#devices-node-shortcut-keys)
- [Tipkovni prečaci za jedinicama čvor](#volumes-node-shortcut-keys)
- [Tipkovni prečaci za količinsko grupe čvor](#volume-groups-node-shortcut-keys)
- [Sigurnosne kopije pravila čvor tipkovni prečaci](#backup-policies-node-shortcut-keys)
- [Sigurnosno kopiranje kataloga čvor tipkovni prečaci](#backup-catalog-node-shortcut-keys)
- [Tipkovni prečaci za zadatke čvor](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Tipkovni prečaci za uređaje čvor

| Izbornik prečac | Rezultat                               |
|:--------------|:-------------------------------------|
| C             | Otvorit će se stranica **Konfiguracija uređaja** . |
| D             | Osvježava popis uređaja i Detalji o uređaju.|
| V             | Otvorit će se izbornik **Prikaz** . |
| W             | Otvorit će se nove StorSimple snimku Upravitelj konzole filtriran na čvor **pojedinosti** . |
| F             | Ažurira konzole za Upravitelj StorSimple snimku. |
| L             | Otvorit će se na stranicu **Izvoz popisa** . 
| H             | Otvorit će se Mrežna pomoć.|
 

#### <a name="volumes-node-shortcut-keys"></a>Tipkovni prečaci za jedinicama čvor

| Izbornik prečac   | Rezultat                              |
|:----------------|:------------------------------------|
| V               | Ažurira popis količine.        |
| V (pritisnite dvaput) | Otvorit će se izbornik **Prikaz** .            |
| W               | Otvorit će se novi Upravitelj snimku StorSimple konzole filtriran na čvor **količine** .|
| F               | Ažurira konzole za Upravitelj StorSimple snimke.|
| L               | Otvorit će se na stranicu **Izvoz popisa** . 
| H               | Otvorit će se Mrežna pomoć.|
 
#### <a name="volume-groups-node-shortcut-keys"></a>Tipkovni prečaci za količinsko grupe čvor

| Izbornik prečac   | Rezultat                              |
|:----------------|:------------------------------------|
| G               | Otvorit će se stranici **Stvaranje grupe jedinica** . |
| V               | Otvorit će se izbornik **Prikaz** . |
| W               | Otvorit će se nove StorSimple snimke Upravitelj konzole filtriran na čvor **Grupe jedinica** .|
| F               | Ažurira konzole za Upravitelj StorSimple snimke. |
| L               | Otvorit će se na stranicu **Izvoz popisa** . |
| H               | Otvorit će se Mrežna pomoć.|

#### <a name="backup-policies-node-shortcut-keys"></a>Sigurnosne kopije pravila čvor tipkovni prečaci

| Izbornik prečac   | Rezultat                              |
|:----------------|:------------------------------------|
| B               | Otvorit će se stranica **Stvaranje pravila** . |
| V               | Otvorit će se izbornik **Prikaz** .            |
| W               | Otvorit će se nove StorSimple snimke Upravitelj konzole filtriran na čvor **Grupe jedinica** .|
| F               | Ažurira konzole za Upravitelj StorSimple snimke.|
| L               | Otvorit će se na stranicu **Izvoz popisa **. 
| H               | Otvorit će se Mrežna pomoć.|
 
#### <a name="backup-catalog-node-shortcut-keys"></a>Sigurnosno kopiranje kataloga čvor tipkovni prečaci

| Izbornik prečac   | Rezultat                              |
|:----------------|:------------------------------------|
| W               | Otvorit će se nove StorSimple snimke Upravitelj konzole filtriran na čvor **Grupe jedinica** . |
| F               | Ažurira konzole za Upravitelj StorSimple snimke. |
| H               | Otvorit će se Mrežna pomoć.|
 
#### <a name="jobs-node-shortcut-keys"></a>Tipkovni prečaci za zadatke čvor

| Izbornik prečac   | Rezultat                              |
|:----------------|:------------------------------------|
| V               | Otvorit će se izbornik **Prikaz** .            |
| W               | Otvorit će se novi Upravitelj snimka StorSimple konzole filtriran na čvor **poslove** .|
| F               | Ažurira konzole za Upravitelj StorSimple snimke.|
| L               | Otvorit će se na stranicu **Izvoz popisa** .     |
| H               | Otvorit će se internetska pomoć                   |
 
## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [pomoću upravitelja snimka StorSimple za administriranje StorSimple rješenje](storsimple-snapshot-manager-admin.md).
- Saznajte kako [pomoću upravitelja snimka StorSimple da bi povezao i upravljanje uređajima](storsimple-snapshot-manager-manage-devices.md).
