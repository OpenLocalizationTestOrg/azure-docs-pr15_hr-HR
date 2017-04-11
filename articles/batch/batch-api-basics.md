<properties
    pageTitle="Azure pregled obrade značajke za razvojne inženjere | Microsoft Azure"
    description="Dodatne značajke servisa grupe i njegov API-ji iz razvoj aspekta."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/29/2016"
    ms.author="marsma"/>

# <a name="batch-feature-overview-for-developers"></a>Pregled značajki grupe za razvojne inženjere

U ovom pregled središnje komponente servisa Azure grupe ćemo razmotriti značajke primarni usluge i resursa koje razvojni inženjeri grupe možete koristiti da biste sastavili veliki paralelno izračunati rješenja.

Hoće li razvoja raspodijeljeno računalne aplikacije ili servisa koji problemi izravnu [REST API -JA] [ batch_rest_api] pozive ili pak koristite jedan od [Obradu SDK-ovi](batch-technical-overview.md#batch-development-apis), koristit ćete mnoge značajke koje se spominju u ovom članku i resursa.

> [AZURE.TIP] Više razine Uvod u usluge grupe potražite u članku [Osnove grupe za Azure](batch-technical-overview.md).

## <a name="batch-service-workflow"></a>Tijek rada za obradu servisa

Sljedeće više razine tijek rada je uobičajeni gotovo sve aplikacije i servise pomoću servisa grupe za obradu paralelne radnih opterećenja:

1. Prijenos **podatkovne datoteke** koje želite obrađivati [Prostora za pohranu Azure] [ azure_storage] računa. Obradu uključuje ugrađenu podršku za pristup spremište blobova platforme Azure i zadataka možete preuzeti te datoteke za [izračunavanje čvorove](#compute-node) izvršavanja zadataka.

2. Prijenos **Datoteka aplikacije** koji će se pokrenuti zadataka. Te datoteke mogu biti binarne datoteke ili skripte i njihove ovisnosti pa su izvršio zadatke u svoje zadatke. Zadataka te datoteke možete preuzeti s računa za pohranu ili koristite značajku [pakete](#application-packages) grupe za upravljanje aplikacijama i implementacija.

3. Stvaranje [grupe aplikacija](#pool) računalnim čvorove. Kada stvorite zajedničko područje, navedite broj računalnim čvorove na resurse, veličinu i operacijski sustav. Kada se pokrene svaki zadatak u svoj posao, je dodijeljen izvršiti na jedan od čvorove u svoje grupe aplikacija.

4. Stvaranje [zadatka](#job). Posao upravlja zbirku zadataka. Povežite svaki zadatak na određeni skup gdje će pokrenuti zadatke za taj zadatak.

5. Dodavanje [zadataka](#task) na posao. Svaki zadatak koji se izvodi aplikacije ili skriptu koju ste prenijeli za obradu podatkovnih datoteka preuzimanja s računa za pohranu. Prilikom svakog zadatka dovrši, to možete prenijeti rezultat Azure prostora za pohranu.

6. Praćenje napretka posla i dohvaćanje zadataka Izlaz iz spremišta Azure.

U sljedećim se odjeljcima navode ih i drugi resursi grupe koje omogućuju raspodijeljeno računalne scenariju.

> [AZURE.NOTE] Potreban [račun za obradu](batch-account-create-portal.md) korištenja servisa za grupu. Osim toga, koristiti gotovo sve rješenja [Azure prostora za pohranu] [ azure_storage] računa za pohranu datoteka i dohvaćanja. Obrada trenutno podržava samo **opće namjene** prostora za pohranu vrstu računa, kao što je opisano u koraku 5 [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account) u [računi o Azure prostora za pohranu](../storage/storage-create-storage-account.md).

## <a name="batch-service-resources"></a>Resursi za servisnu grupe

Neki od navedenih resursa – računi za izračun čvorove, grupe, zadataka i zadataka – su potrebni za sve rješenja koja koriste servis za obradu. Drugim korisnicima, kao što su rasporedima zadataka i paketa aplikacije su korisne, ali nije obavezno, značajke.

- [Računa](#account)
- [Izračunavanje čvor](#compute-node)
- [Skup](#pool)
- [Zadatak](#job)

  - [Raspored posla](#scheduled-jobs)

- [Zadatak](#task)

  - [Početak zadatka](#start-task)
  - [Upravitelj zadatak](#job-manager-task)
  - [Priprema i izdanje zadataka posla](#job-preparation-and-release-tasks)
  - [Zadatak s više instanci (MPI)](#multi-instance-tasks)
  - [Međuzavisnosti zadataka](#task-dependencies)

- [Paketa aplikacije](#application-packages)

## <a name="account"></a>Računa

Račun za obradu je jedinstveno identificirani entitet unutar grupe za servis. Obrada svih povezan je s računom grupe. Prilikom izvođenja operacije sa servisom grupe, morate i naziv računa i jedan od svoje ključeve za račun. Možete [stvoriti račun grupe za Azure pomoću portala za Azure](batch-account-create-portal.md).

## <a name="compute-node"></a>Izračunavanje čvor

Računalnim čvor je programa Azure virtualnog računala (VM) posvećenu obrade dio radno opterećenje vaše aplikacije. Veličina čvor određuje broj procesora jezgri, kapaciteta memorije i lokalni sustav veličinu dodijeljen čvor. Grupe sustava Windows ili Linux čvorove možete stvoriti pomoću servisa u Oblaku Azure ili virtualnim strojevima trgovine slike. U odjeljku sljedeće [grupe aplikacija](#pool) za dodatne informacije o tim mogućnostima.

Čvorovi mogu se izvoditi izvršne datoteke ni skriptu koja podržava okruženje za operacijski sustav čvor. To obuhvaća \*.exe, \*.cmd, \*ljuske .bat i skripti ljuske PowerShell za Windows – i binarne datoteke, a Python skripte za Linux.

Sve izračun čvorove u seriji obuhvaćaju:

- Na standardni [Struktura mapa](#files-and-directories) i pridruženi [varijable okruženja](#environment-settings-for-tasks) koje su dostupne za referencu koriste zadaci.
- Postavke **vatrozida** podešena za kontrolu pristupa.
- [Daljinski pristup](#connecting-to-compute-nodes) sustava Windows (Remote Desktop Protocol (RDP)) i čvorove Linux (Secure ljuske (SSH)).

## <a name="pool"></a>Skup

Zbirka čvorove pokrenute aplikacije na je u grupu. Na resurse možete stvoriti ručno vi ili automatski putem usluge obradu kada odredite da se izvršiti. Možete stvoriti i upravljati skup koje ispunjavaju uvjete resursa aplikacije. Zajedničko područje mogu koristiti samo grupe za račun u kojem je stvorena. Grupe za račun može imati više od jedne grupe aplikacija.

Azure grupe grupe za sastavljanje pri vrhu platforme Azure računalnim core. Pružaju veliki dodijeljeni, instalacija aplikacije, distribucija podataka, nadzor stanja i prilagodljivo prilagođavanje broja računalnim čvorove unutar skup ([Skaliranje](#scaling-compute-resources)).

Svaki čvor koji se dodaje u grupu se dodjeljuje jedinstveni naziv i IP adrese. Kada se čvor Ukloni iz skupa, gube sve promjene načinjene operacijski sustav ili datoteke, a naziv i IP adrese objavio za buduću upotrebu. Kada čvor napusti zajedničko područje, njegov vijek je premašila dopuštenu.

Kada stvorite zajedničko područje, možete odrediti sljedećim atributima:

- Izračunavanje čvor **operacijski sustav** i **verzija**

    Kada odaberete operacijski sustav za čvorove u svoje grupe aplikacija koje su vam dvije mogućnosti: **Konfiguriranje virtualnog računala** i **Konfiguracija servisa oblaka**.

    **Konfiguracija virtualnog računala** omogućuje Linux i Windows slike za izračunavanje čvorove iz [Trgovine Azure virtualnim strojevima][vm_marketplace].
    Kada stvorite grupu koja sadrži čvorove konfiguracija virtualnog računala, morate navesti ne samo veličine čvorove, ali i **reference slika virtualnog računala** i obradu **čvor agent SKU-om za** instalaciju na čvorove. Dodatne informacije o navođenju tih svojstava za skup potražite u članku [Dodjeljivanje Linux izračunati čvorove u grupe za Azure grupe](batch-linux-nodes.md).

    **Konfiguriranje usluge oblak** nudi Windows čvorove *samo*izračunati. Dostupna operacijski sustavi za konfiguraciju servisa oblak grupe navedene su u [izdanjima OS goste Azure i SDK kompatibilnosti matrice](../cloud-services/cloud-services-guestos-update-matrix.md). Kada stvorite grupu koja sadrži čvorove servise u Oblaku, morate odrediti samo veličina čvor i *Obitelji OS*. Prilikom stvaranja grupe sustava Windows izračunati čvorove, najčešće koristiti servise u Oblaku.

    - *OS obitelji* određuje i koje verzije .NET koje se instaliraju pomoću OS-a.
    - Uloga suradnika u servise u Oblaku, možete navesti *Verzija OS-a* (Dodatne informacije o ulogama tempiranja [obavijesti me na servise u oblaku](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) u vidjeli odjeljak [Pregled servise u Oblaku](../cloud-services/cloud-services-choose-me.md)).
    - Kao što je s ulogama tempiranja, preporučujemo da navedete `*` za *Verzija OS-a* tako da su čvorove automatski nadograditi, a još ne postoji zaobilazno morati upravo cater da biste objavio verzije. Slučaj prvenstveno se koristi za odabir određenih verzija OS-a je kompatibilnost aplikacija, koji omogućuje kompatibilnost s prijašnjim verzijama testiranja koje je potrebno izvršiti prije nego što verziju ažurirati. Nakon provjere valjanosti, može se ažurirati *Verzija OS-a* za na resurse i novu sliku s operacijskim Sustavom može biti instaliran – sve pokrenute zadatke su prekinuta i premjestio.

- **Veličina čvorove**

    **Konfiguriranje usluge oblaka** računalnim čvor veličine navedene su u [veličine za servise u Oblaku](../cloud-services/cloud-services-sizes-specs.md). Obradu podržava sve servise u Oblaku veličine osim `ExtraSmall`.

    **Konfiguracija virtualnog računala** izračunati čvor veličine navedene su u [veličine za virtualnim strojevima u Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) i [veličine za virtualnim strojevima u Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Obradu podržava svih veličina Azure VM osim `STANDARD_A0` i one s premium prostora za pohranu (`STANDARD_GS`, `STANDARD_DS`, i `STANDARD_DSV2` niz).

    Kada odaberete veličinu čvor računalnim, razmislite o značajke i preduvjeti pokrenut ćete za čvorove aplikacija sustava. Aspekti kao što je li aplikacija s usporednim nitima i koliko memorije je troši može pomoći odrediti čvor veličinu prikladne i učinkovit. Karakteristično za odabir u čvor veličina uz pretpostavku da jedan zadatak će se izvoditi na čvor istovremeno je. Međutim, moguće je da bi više zadataka (i zbog toga više instanci aplikacije) [izvode paralelno](batch-parallel-node-tasks.md) izračunati čvorove tijekom izvođenja posla. U ovom slučaju uobičajeno je da biste odabrali veći čvor kako bi odgovarao povećana zahtjev izvršavanja paralelno zadatka. Dodatne informacije potražite [pravila za zakazivanje zadataka](#task-scheduling-policy) .

    Sve čvorove u zajedničko područje su iste veličine. Ako namjeravate pokrenuti aplikacije s različiti sistemski preduvjeti i/ili učitavanje razine, preporučujemo da koristite odvojene grupe.

- **Broj čvorove cilj**

    To je broj računalnim čvorove koje želite uvesti u. To se naziva kao *cilj* jer u nekim slučajevima vaše skup možda dosegne željeni broj čvorove. Zajedničko područje mogu pristupiti željeni broj čvorove ako dođe do [core kvote](batch-quota-limit.md#batch-account-quotas) za vaš račun za obradu – ili ako formula za automatsko skaliranje koji ste primijenili na resurse koji ograničava maksimalni broj čvorove (u odjeljku "Skaliranje pravila").

- **Promjena veličine pravila**

    Osim određivanja statične broj čvorove, umjesto toga možete pisati i Primjena [Automatsko skaliranje formulu](#scaling-compute-resources) na resurse. Servis za obradu povremeno procjenjuje formulu i podešava broj čvorove unutar skupa na temelju različite skup, posla i parametre zadatka možete navesti.

- **Pravila za zakazivanje zadataka**

    Mogućnost konfiguracija [Maks zadataka po čvor](batch-parallel-node-tasks.md) određuje najveći je broj zadataka koji se može pokrenuti paralelno na svakom čvor računalnim unutar na resurse.

    Konfiguracija zadano je da jedan zadatak po pokreće čvor, no postoje scenarije u kojima je korisno imati više od jedne zadatak koji se izvodi na čvor istodobno. Pogledajte [primjer scenarija](batch-parallel-node-tasks.md#example-scenario) u članku [Istodobni čvor zadataka](batch-parallel-node-tasks.md) da biste vidjeli koje su prednosti iz više zadataka po čvor.

    Možete odrediti i *vrstu ispune* koja određuje hoće li se grupe ravnomjerno širi zadaci preko sve čvorove u zajedničko područje ili paketi svaki čvora s maksimalni broj zadataka prije dodjele zadataka na drugi čvor.

- **Stanje komunikacije s** računalnim čvorove

    U većini slučajeva, zadaci samostalno rade i ne morate međusobno komuniciraju. No postoje neke aplikacije u kojem zadaci moraju komunikaciju, kao što su [scenariji MPI](batch-mpi.md).

    Možete konfigurirati grupu dopustiti komunikaciju između čvorove unutar it –**internode komunikacije**. Kada je omogućen internode komunikacije, čvorove u oblak Services konfiguracije grupe mogli komunicirati sa međusobno na veći od 1100 priključke i konfiguracija virtualnog računala grupe ograničili promet na bilo koji priključak.

    Imajte na umu da Omogućivanje komunikacije internode utječe položaj čvorove unutar klastere i može ograničiti maksimalni broj čvorove u zajedničko područje zbog ograničenja implementacije. Ako aplikacija ne zahtijeva komunikaciju između čvorove, servis za obradu može dodijeliti potencijalno velik broj čvorove na resurse s mnogo različitih klastere i podatkovnim centrima da biste omogućili povećan power paralelno obrada.

- Čvorovi računalnim **Početak zadatka**

    Neobavezno *pokrenuti zadatak* izvršava na svakom čvor taj čvor spaja na resurse i svaki put čvor je ponovno pokrenuti ili reimaged. Pokretanje zadatka osobito je korisna za pripremu računalnim čvorove za izvršavanje zadataka, kao što su instalacije aplikacije koje zadataka koji se izvoditi na čvorove računalnim.

- **Pakete aplikacija**

    Možete navesti [paketa aplikacije](#application-packages) za implementaciju čvorove računalnim u. Paketa aplikacije sadrže pojednostavljeni implementacije i upravljanje verzijama programa koji se pokreću zadataka. Paketa aplikacije koje navedete za zajedničko područje instaliranih na svakoj čvor koji spaja tu grupu, a svaki put kada čvor sustava ili reimaged. Pakete aplikacija trenutno nisu podržana na čvorove računalnim Linux.

- **Mrežna konfiguracija**

    Možete odrediti treba stvoriti ID Azure [virtualne mreže (VNet)](../virtual-network/virtual-networks-overview.md) u kojem na resurse za izračun čvorove. Preduvjeti za određivanje u VNet za svoje područje pronaći ćete u [Dodaj grupu s poslovnim subjektom] [ vnet] referenca za obradu REST API-JA.

> [AZURE.IMPORTANT] Svi računi obradu imaju zadani **kvote** koja se ograničava broj **jezgri** (i zato računalnim čvorove) grupe za račun. Zadani kvotama i upute da biste [povećali ograničenja](batch-quota-limit.md#increase-a-quota) (kao što su maksimalni broj jezgri na vašem računu grupe) možete pronaći u [kvotama i ograničenja servisa Azure grupe](batch-quota-limit.md). Ako pronađete sami pitanjem "Zašto neće Moje skup doći do više od X čvorove?" kvota za temeljni se nalaze.

## <a name="job"></a>Zadatak

Zadatak je zbirka zadataka. Upravlja kako izračunavanje obavlja njegov zadaci na čvorove računalnim u zbirci web.

- Posao određuje **područje** u kojem se rad bit će se pokrenuti. Stvaranje nove grupe aplikacija za svaki zadatak ili koristiti jedan skup za mnoge zadatke. Možete stvoriti grupu za svaki zadatak koji je povezan s posla rasporeda ili za sve zadatke koje su vezane uz raspored posla.

- Možete odrediti neobavezno **Prioritet posao**. Kada posao šalje se s veći prioritet od poslova koji su trenutno u tijeku, zadatke za posao veći prioritet umeću se u redu čekanja na zadatke za poslove donjem prioritet. Zadaci u donjem prioriteta zadataka koji su već pokrenuti su preempted.

- Zadatak **ograničenja** možete koristiti da biste odredili određene ograničenja za svoje zadatke:

    **Vrijeme maksimalni wallclock**možete postaviti tako da ako posao traje dulje od maksimalno wallclock vrijeme navedeno, su prekinuti posla i sve njegove zadatke.

    Grupe možete otkriti i ponovno pokušajte nije uspjelo zadatke. **Maksimalni broj ponovne pokušaje zadatka** možete navesti kao ograničenja, uključujući li zadatak je *uvijek* ili *nikada* ponoviti. Ponovni zadatak, to znači da se zadatak premjestio da biste ponovno pokrenuli.

- Klijentska aplikacija možete dodati zadatke za posao ili možete odrediti [Upravitelj zadatak](#job-manager-task). Upravitelj zadatak sadrži informacije koje su potrebne za stvaranje obavezne zadatke za posao, zadatak Upravitelj pokrenut na jedan od čvorove računalnim u. Upravitelj zadatak obrađuje posebno po grupe – ga je u redu čekanja čim posao se stvara i ponovnog pokretanja Ako ne uspijete. Upravitelj zadatak je *potreban* za zadatke koje je stvorio [raspored posla](#scheduled-jobs) jer je jedini način da biste definirali zadataka prije nego što je posao instancirati.

- Prema zadanim postavkama, zadacima ostaju u stanju aktivan nakon dovršetka svih zadataka unutar posla. Takvo ponašanje možete promijeniti tako da se posao automatski prekinuti kada dovršite sve zadatke u sklopu posla. Postavite svojstvo **onAllTasksComplete** posla ([OnAllTasksComplete] [ net_onalltaskscomplete] u grupe za .NET) da biste *terminatejob* automatski prekinuti posao kada sve njegove zadataka u stanju dovršenosti.

    Imajte na umu da servis za obradu smatra zadatak s *nema* zadataka da bi svi dovršenih zadataka. Zbog toga tu mogućnost najčešće koriste sa [Upravitelj zadatak](#job-manager-task). Ako želite koristiti automatsko kopiranje prekid bez upravitelja posla koji treba prethodno postavite svojstvo **onAllTasksComplete** novi zadatak na *noaction*, a zatim postavite *terminatejob* tek nakon što završite s dodavanjem zadataka na posao.

### <a name="job-priority"></a>Prioritet za posao

Prioritet možete dodijeliti zadatke koje stvorite u grupe. Servis za obradu koristi vrijednost prioritet zadatka da biste odredili redoslijed posao zakazivanja poslovnog subjekta (to je li isto [zakazani posao](#scheduled-jobs)). Prioritet vrijednosti u rasponu od -1000 do 1000-1000 se najniže prioritet i 1000 se najviše. Možete ažurirati prioritet zadatka pomoću mogućnosti [za ažuriranje svojstava posao] [ rest_update_job] operacija (obradu REST) ili izmjenom [CloudJob.Priority] [ net_cloudjob_priority] svojstvo (grupe za .NET).

Isti subjekta viši prioritet postoje zakazivanje prednost putem poslove donjem prioritet. Zadatak s vrijednošću višeg prioriteta u jedan račun nema zakazivanje prednost putem drugog zadatka s vrijednošću donjem prioriteta u neki drugi račun.

Planiranje preko grupe nalazi nezavisne. Između različitih grupe ga se ne zajamčiti da viši prioritet zadatka je zakazano najprije ako je njegova pridruženi skup nedostaje neaktivnosti čvorove. U isti skup poslove s istu razinu prioriteta imati jednak izgledi koji se zakazano.

### <a name="scheduled-jobs"></a>Zakazani zadaci

[Zadatak rasporede] [ rest_job_schedules] omogućuju vam da biste stvorili Ponavljajući poslovi unutar grupe za servis. Raspored posla određuje kada za pokretanje zadatke i uključuje postavke vezane uz poslove koji će se pokrenuti. Možete odrediti trajanje rasporeda – koliko i kada je raspored na snazi – te koliko često tijekom tog vremenskog razdoblja koje treba kreirati zadatke.

## <a name="task"></a>Zadatak

Zadatak je jedinica izračuni koji je povezan s posla. Izvodi se čvor. Zadaci dodjeljuju čvor za izvršavanje ili u redu čekanja dok čvor postaje besplatne. Stavite jednostavno, zadatak pokreće jedan ili više programa ili skripte računalnim čvor da biste izvršili obavljen vam je potrebna.

Kada stvorite zadatak, možete odrediti:

- U **naredbeni redak** zadatka. Ovo je naredbenog retka koji se izvodi aplikacije ili skripte na čvor računalnim.

    Važno je da Imajte na umu da u naredbenom retku ne zapravo izvode u ljusci. Stoga je nije moguće nativno iskoristite prednost ljuske značajke kao što su proširenja [varijablu okruženja](#environment-settings-for-tasks) (To uključuje u `PATH`). Da biste iskoristili značajke kao što su, morate pozovete ljuske u naredbenom retku – na primjer, tako da pokrenete `cmd.exe` na Windows čvorove ili `/bin/sh` na Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Ako je potrebno za pokretanje aplikacije ili skriptu koja se ne nalazi na čvor zadataka `PATH` ili referencu varijable okruženja, pozovite ljuske izričito u naredbenom retku zadatka.

- **Datoteka resursa** koje sadrže podatke koje želite da se obrađuju. Datoteke se automatski se kopiraju u čvor iz spremišta blobova na računu za pohranu Azure **opće namjene** prije izvođenja zadatka naredbenog retka. Dodatne informacije potražite u odjeljcima [pokretanje zadatka](#start-task) i [datoteke i mape](#files-and-directories).

- **Varijable okruženja** na koji su potrebni za aplikacije. Dodatne informacije potražite u odjeljku [Postavke okruženja za zadatke](#environment-settings-for-tasks) .

- **Ograničenja** u odjeljku koji moraju izvršiti zadatak. Zadržavaju se za maksimalno vrijeme u kojem je zadatak je dopušteno da biste pokrenuli, maksimalni broj puta nije uspjelo zadatka mora se ponoviti, primjeru i maksimalno vrijeme te datoteke u direktoriju rad na zadatku.

- **Program paketa** za implementaciju čvor računalnim na kojem zakazano izvođenje zadatka. [Paketa aplikacije](#application-packages) sadrže pojednostavljeni implementaciju i upravljanje verzijama programa koji se pokreću zadataka. Paketa aplikacije na razini zadatka osobito korisne su u zajednički skup okruženja, pri čemu se izvode drugi poslovi na jedan skup, a na resurse neće se izbrisati kada je zadatak dovršen. Ako je vaš posao zadaci manje od čvorove u, pakete zadatak možete minimizirati prijenos podataka jer je aplikacija implementiran samo na čvorove koji se izvode zadatke.

Zadaci koje ste definirali za izvođenje izračuna na čvor, sljedeće zadatke posebnih također nudi grupe servisa:

- [Početak zadatka](#start-task)
- [Upravitelj zadatak](#job-manager-task)
- [Priprema i izdanje zadataka posla](#job-preparation-and-release-tasks)
- [Više instanci zadataka (MPI)](#multi-instance-tasks)
- [Međuzavisnosti zadataka](#task-dependencies)

### <a name="start-task"></a>Početak zadatka

Pridruživanjem **pokrenuti zadatak** zajedničko područje, možete pripremiti radnoj okolini svoje čvorove. Na primjer, možete izvoditi akcije kao što su instalacije programa koji se pokreću zadataka i pokretanje pozadinski procesi. Pokretanje zadatka pokreće prilikom svakog pokretanja čvor za sve dok se ona ostaje u – uključujući pri prvom čvor dodane na resurse i kada je ponovno pokrenuti ili reimaged.

Glavna prednost zadatka pokreni jest da mogu sadržavati sve podatke koji su potrebni za konfiguriranje računalnim čvor i instalirati programe koji su potrebni za izvršavanje zadataka. Stoga povećava broj čvorove u zajedničko područje jednostavan je određivanje novi cilj čvor broj – obradu već sadrži podatke koje je potrebno za konfiguriranje novi čvorovi i pripremite ih za prihvaćanje zadataka.

Kao što je bilo koji zadatak Azure grupe omogućuju da odredite popis **datoteka resursa** u [Spremište Azure][azure_storage], osim **naredbenog retka** želite izvršiti. Obradu najprije kopira datoteke resursa čvor iz spremišta Azure, a zatim pokreće naredbenog retka. Za početak zadatku skup popisa datoteka obično sadrži aplikacije zadatka i njezine ovisnosti.

Međutim, ga nije moguće uvrštavati referentnih podataka koja će se koristiti tako da sve zadatke koji su pokrenuti na čvor računalnim. Na primjer, nije moguće izvesti zadatka pokreni naredbenog retka na `robocopy` postupak da biste kopirali datoteka aplikacije (koje su navedene kao datoteka resursa i preuzeti čvor) za zadatak početka [rada direktorija](#files-and-directories) u [zajedničku mapu](#files-and-directories), a zatim pokrenite msi-JA ili `setup.exe`.

> [AZURE.IMPORTANT] Obrada trenutno podržava *samo* vrstu računa **opće namjene** prostora za pohranu, kao što je opisano u koraku 5 [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account) u [računi o Azure prostora za pohranu](../storage/storage-create-storage-account.md). Obradu zadataka (uključujući standardne zadatke, zadaci start, Zadaci pripreme za posao i zadatke posla izdanje) morate navesti datoteke resursa koji se nalaze *samo* u računi **opće namjene** prostora za pohranu.

To je obično poželjno servisa za obradu ćete morati pričekati zadatka pokreni da biste mogli odlučuje čvor jeste li spremni za dodjelu zadataka, no to možete konfigurirati.

Ne uspijete početka zadatka na računalnim čvor, stanje čvor ažurirat će se u skladu s vizualnim pogreške, a čvor je dodijeljen sve zadatke. Pokretanje zadatka uspijeva ako je došlo do problema kopiranje njezine datoteke resursa iz spremišta ili postupka izvršio njegov naredbenog retka vraća kod koja nije nula izlaz.

Ako dodate ili ažuriranje zadatka pokreni za *postojeću* grupu, morate ponovno pokrenuti svoje čvorove računalnim početka zadatka koji želite primijeniti čvorove.

### <a name="job-manager-task"></a>Upravitelj zadatak

Koje obično pomoću **upravitelja zadatak** da biste upravljali i/ili praćenje poslova izvođenja – na primjer, za stvaranje i slanje zadataka za posao, odredite dodatne zadatke da biste pokrenuli i određivanje nakon dovršetka rada. Međutim, zadatak manager nije ograničena te aktivnosti. Nije potpuno fledged zadaci koje možete izvršiti nijednu akciju koji su potrebni za posao. Upravitelj zadatak možda, na primjer, preuzimanje datoteke koja je navedena kao parametar, analizirali sadržaj te datoteke i slanje dodatnih zadataka na temelju tih sadržaja.

Upravitelj zadatak će se pokrenuti prije druge zadatke. Pruža sljedeće značajke:

- Automatski šalje se zadatak servis za obradu stvaranja posao.

- Zakazano je izvršiti prije zadatke u posao.

- Svoj pridružene čvor je zadnji koje će biti uklonjene iz skupa kada je u tijeku downsized na resurse.

- Njegov prekid može biti uz prekid sve zadatke u sklopu posla.

- Kada je potrebno je ponovno pokrenuti upravitelj zadatak je zadan najveći prioritet. Ako neaktivnosti čvor nije dostupna, servis za obradu mogu prekinuti jedan od drugih izvodi zadataka u da biste oslobodili prostor za Upravitelj zadatak da biste pokrenuli.

- Upravitelj zadatak u jedan zadatak imaju prednost pred zadataka druge zadatke. Preko poslovi opaženih će se samo Prioriteti posao razinu.

### <a name="job-preparation-and-release-tasks"></a>Priprema i izdanje zadataka posla

Obradu nudi Zadaci pripreme posla za posao prije izvođenja postavljanje. Zadaci izdanje posla su za održavanje nakon posao ili čišćenje.

- **Priprema zadatak**: Priprema zadatak pokreće se na sve čvorove računalnim koji su zakazani da biste pokrenuli zadataka prije izvođenja sve druge zadatke posla su. Možete koristiti zadatak pripreme za kopiranje podataka koje zajednički koriste svi zadaci, ali je jedinstvena posla, na primjer.
- **Zadatak izdanja**: kada je zadatak dovršen, izdanje zadatak pokreće svaki čvor u grupu koja izvršava najmanje jedan zadatak. Da biste izbrisali podatke koji se kopiraju prema Priprema zadatak ili biste sažimanja i prijenos dijagnostičkih zapisnika podataka, na primjer, poslužite se zadatak izdanje.

Oba zadatka pripreme i zadaci izdanje omogućuju vam da biste odredili naredbenog retka da biste pokrenuli kada se zadatak poziva. Oni nude značajke kao što su preuzimanje datoteke, dodatnim izvođenja, varijable prilagođene okruženja, trajanje najveći izvršavanja, pokušajte ponovno count i vrijeme zadržavanja datoteke.

Dodatne informacije o Priprema i izdanje zadataka posla, potražite u članku [Pokreni posao Priprema i dovršetka zadaci na obradu Azure izračunati čvorove](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Zadatak s više instanci

[Zadatak s više instanci](batch-mpi.md) je zadatak koji je konfiguriran za rad na više od jedne računalnim čvor istodobno. S više instanci zadatke, možete omogućiti visokih performansi računalno scenarija koji zahtijevaju grupe čvorove računalnim koji su zajedno dodijeliti za obradu jedan radno opterećenje (kao što su poruke prosljeđivanje sučelja (MPI)).

Za detaljne rasprave na izvođenje zadataka MPI u grupe pomoću grupe za .NET biblioteke, pogledajte [Korištenje više instanci zadatke za pokretanje aplikacije sučelja prosljeđivanje poruke (MPI) u grupe za Azure](batch-mpi.md).

### <a name="task-dependencies"></a>Međuzavisnosti zadataka

[Međuzavisnosti zadataka](batch-task-dependencies.md), kao što to naziv kaže, omogućuju vam da biste odredili da zadatka ovisi o obavljanjem druge zadatke prije njegova izvođenja. Ta značajka omogućuje podršku za situacije u kojima "do" zadatka troši Izlaz "upstream" zadatka – ili upstream zadataka izvodi neke Inicijalizacija koje je potrebno do zadatka. Da biste koristili ovu značajku, najprije morate omogućiti međuzavisnosti zadataka na skupna obrada. Zatim, za svaki zadatak koji ovisi o drugi (ili mnoge druge), odrediti zadatke koje ovise o tom zadatku.

Pomoću međuzavisnosti zadataka možete konfigurirati scenariji kao što je sljedeća:

* *taskB* ovisi o *taskA* (*taskB* će ne započne izvršavanja dok se ne dovrši *taskA* ).
* *taskC* ovisi o *taskA* i *taskB*.
* *taskD* ovisi o rasponu zadatke, na primjer zadaci od *1* do *10*, prije nego što je izvršava.

Pogledajte [međuzavisnosti zadataka u seriji Azure](batch-task-dependencies.md) i [TaskDependencies] [ github_sample_taskdeps] uzorak koda u [azure, grupe i uzorke] [ github_samples] GitHub spremište dodatne detaljnije informacije o toj značajci.

## <a name="environment-settings-for-tasks"></a>Postavki okruženja za zadatke

Svaki zadatak koji se izvodi servis za obradu ima pristup varijable okruženja, koje se postavlja na računalnim čvorove. To uključuje varijabli okruženja definira servis za obradu ([servis definirane][msdn_env_vars]) i varijable prilagođene okruženja, koje možete definirati zadataka. Aplikacije i skripte izvršavanje zadataka tijekom izvođenja imaju pristup te varijable okruženja.

Varijable prilagođene okruženja na razini zadatak ili posao možete postaviti tako da popunite svojstvo *postavki okruženja* za ove entiteti. Na primjer, potražite u članku [Dodavanje zadataka na posao] [ rest_add_task] operacija (obradu REST API-JA) ili [CloudTask.EnvironmentSettings] [ net_cloudtask_env] i [CloudJob.CommonEnvironmentSettings] [ net_job_env] svojstava u .NET grupe.

Klijentski program ili servis možete dobiti na zadatak okruženje varijable servisa definirani i prilagođeni, pomoću [Dohvaćanje informacija o zadatku] [ rest_get_task_info] operacija (obradu REST) ili pristupom [CloudTask.EnvironmentSettings] [ net_cloudtask_env] svojstvo (grupe za .NET). Procesa koji se izvršavaju na računalnim čvor mogu pristupati te i druge varijable okruženja na čvor, na primjer, pomoću u poznate `%VARIABLE_NAME%` (Windows) ili `$VARIABLE_NAME` sintaksa (Linux).

Potpuni popis svih okruženje za servis definirane varijable možete pronaći u [čvor varijable okruženja za izračun][msdn_env_vars].

## <a name="files-and-directories"></a>Datoteke i mape

Svaki zadatak koji je u *radu direktorija* pod kojim stvara nula ili više datoteke i mape. Ovaj radni direktorij može se koristiti za pohranu programa koji se izvodi zadatak, podatke koje obrađuje i izlaz iz obrada izvodi. Sve datoteke i mape zadatka čiji je korisnik zadatka.

Servis za obradu izlaže dio datotečni sustav na čvor kao *korijenski direktorij*. Zadaci možete pristupiti korijenski direktorij tako da upućuju na `AZ_BATCH_NODE_ROOT_DIR` varijablu okruženja. Dodatne informacije o korištenju varijable okruženja, potražite u članku [postavki okruženja za zadatke](#environment-settings-for-tasks).

U korijenskom direktoriju sadrži sljedeću strukturu direktorija:

![Izračunavanje strukturu čvor direktorija][1]

- **zajedničke**: taj imenik omogućuje pristup čitanje/pisanje *svih* zadataka koji se izvode na čvor. Svaki zadatak koji se izvodi na čvor možete stvoriti, čitanje, ažuriranje i brisanje datoteka u taj imenik. Zadaci možete pristupiti taj imenik tako da upućuju na `AZ_BATCH_NODE_SHARED_DIR` varijablu okruženja.

- **pokretanje**: taj imenik koristi zadatka Pokreni kao njegov radni imenik. Sve datoteke koje se preuzimaju u čvor po zadatka pokreni spremaju se ovdje. Zadatak start možete stvoriti, čitanje, ažuriranje i brisanje datoteka u odjeljku taj imenik. Zadaci možete pristupiti taj imenik tako da upućuju na `AZ_BATCH_NODE_STARTUP_DIR` varijablu okruženja.

- **Zadaci**: direktorij stvara se za svaki zadatak koji se izvodi na čvor. Pristupa tako da upućuju na `AZ_BATCH_TASK_DIR` varijablu okruženja.

    U direktoriju svaki zadatak servis za obradu stvara radni direktorij (`wd`) čiji jedinstveni put određen u `AZ_BATCH_TASK_WORKING_DIR` varijablu okruženja. Taj imenik omogućuje čitanje/pisanje pristup zadatak. Zadatak možete stvaranje, čitanje, ažuriranje i brisanje datoteka u odjeljku taj imenik. Taj imenik zadržava se temelji na *RetentionTime* ograničenje koje je navedeno za zadatak.

    `stdout.txt`i `stderr.txt`: te datoteke zapisuju u mapu zadatka tijekom izvođenja zadatka.

>[AZURE.IMPORTANT] Kada čvor je uklonjena iz na resurse, *sve* datoteke koje se pohranjuju se na čvor se uklanjaju.

## <a name="application-packages"></a>Paketa aplikacije

Značajka za [pakete](batch-application-packages.md) omogućuje jednostavno upravljanje i implementaciju aplikacija za čvorove računalnim u svoje grupe. Možete prenijeti i upravljati više verzija aplikacije pokrenuti zadataka, uključujući njihove binarne datoteke i datoteke za podršku. Zatim možete automatski implementirati jedan ili više od tih aplikacija za čvorove računalnim u svoje grupe aplikacija.

Možete navesti paketa aplikacije na razini grupe aplikacija i zadatke. Kada odredite skup pakete aplikacija je implementiran na svaki čvor u. Kada odredite zadatka pakete aplikacija je implementiran samo na čvorove koji su zakazani da biste pokrenuli barem jedan od zadataka posla, neposredno prije pokretanja zadatka naredbenog retka.

Obradu rukuje detalje o radu s Azure prostora za pohranu za pohranu paketi za aplikacije i uvesti ih za izračunavanje čvorove, tako da vaše kod i upravljanje indirektni možete pojednostavnjeni.

Da biste saznali više o značajki paketa aplikacije, potražite u članku [Implementacija aplikacije s paketa aplikacije Azure grupe](batch-application-packages.md).

>[AZURE.NOTE] Ako dodate grupu pakete *postojeće* grupe aplikacija, morate ponovno pokrenuti svoje čvorove računalnim za pakete aplikacija uvesti čvorove.

## <a name="pool-and-compute-node-lifetime"></a>Skup i računalnim čvor vijek trajanja

Prilikom dizajniranja rješenja Azure grupe, sami morate dizajna odluku o i kada grupe stvaraju i koliko izračunati čvorove unutar te grupe čuvaju dostupna.

Na jedan kraj spektra, možete stvoriti grupe aplikacija za svaki zadatak koji pošaljete i brisanje na resurse čim njegov zadaci Završi izvršavanja. To Maksimizira Upotreba jer čvorove samo dodijeliti prilikom potrebno, a zatim isključi čim postanu neaktivan. Dok je to znači da posao morate pričekati čvorove će se dodijeliti, važno je Imajte na umu da se zadaci zakazuju za izvršavanje čim čvorove dostupnih pojedinačno, dodijeliti, a start zadatak dovršen. Ne grupe *ne* Pričekajte sve čvorove unutar na skup dostupnih prije dodjeljivanje zadataka čvorove. Na taj način Maksimalna Upotreba sve dostupne čvorove.

Na drugoj strani suprotan pristup jest, ako imate zadatke koji su se odmah pokrenuti najveći prioritet možete stvoriti grupu na vrijeme i dostupnim svoje čvorove prije poslane poslova. U ovom scenariju zadaci možete započeti odmah, ali čvorove možda sjesti neaktivnosti dok ih želite dodijeliti.

Kombinirani pristup obično koristi za rukovanje varijable, ali tijeku, Učitaj. Imate skup koji se šalje na više zadataka, ali mogu mijenjati veličinu broj čvorove gore ili dolje prema posao učitavanje (pogledajte u sljedećem odjeljku [Skaliranje izračunati resursa](#scaling-compute-resources) ). To možete učiniti reactively, koji se temelje na trenutno opterećenje ili doći, ako učitavanja možete predviđene.

## <a name="scaling-compute-resources"></a>Promjena veličine računalnim resursi

Uz [Automatsko skaliranje](batch-automatic-scaling.md)možete imati servis za obradu dinamički podesiti broj računalnim čvorove u skup prema trenutni korištenje radno opterećenje i resursa računalnim scenariju. Omogućuje donji ukupni trošak pokretanje aplikacije pomoću samo resursa potrebno, a one ne morate otpustite.

Omogući automatsko skaliranje pisanjem [automatskog skaliranja formulu](batch-automatic-scaling.md#automatic-scaling-formulas) i pridruživanje formulu u grupu. Servis za obradu koristi formulu za određivanje ciljnog broj čvorove u sljedeći skaliranja razdoblju (interval koja možete konfigurirati). Možete odrediti postavke automatskog skaliranja za zajedničko područje prilikom stvaranja ili omogućivanje skaliranje na zajedničko područje kasnije. Možete ažurirati i postavke grupe aplikacija skaliranja omogućenim u skaliranja.

Na primjer, možda posao zahtijeva pošaljete velikog broja zadaci koje ćete morati izvršiti. Skaliranja formulu možete dodijeliti skup koja prilagođava broj čvorove u skup koji se temelji na trenutni broj u redu čekanja zadaci i stopa dovršetka zadataka u posao. Servis za obradu povremeno procjenjuje formulu i mijenja veličinu na resurse na temelju radno opterećenje (čvorove za mnoge zadatke u redu čekanja za dodavanje i uklanjanje čvorove za zadatke koji nije u redu čekanja ili ne radi) i druge formule postavke.

Skaliranja formule mogu se temeljiti na metriku sljedeće:

- **Mjerenja vremena** temelje se na Statistika prikupljaju svakih pet minuta na navedeni broj sati.

- **Resurs metriku** temelje se na korištenje procesora, korištenja propusnosti, memorije i broj čvorove.

- **Zadatka metriku** temelje se na stanje zadatka, kao što je *Active* (u redu čekanja), *pokretanje*ili *Dovršeno*.

Kada automatsko skaliranje se smanjuje broj računalnim čvorove u zajedničko područje, morate uzeti u obzir kako rukovati zadataka koji se izvode u trenutku smanjenje operacije. Da biste to omogućili, obradu nudi *mogućnost deallocation čvor* koje možete uvrstiti u formulama. Na primjer, možete odrediti da izvodi zadatke su zaustavio odmah, odmah zaustavio i zatim premjestio za izvršavanje na drugi čvor ili dopušteno da biste dovršili prije čvor je uklonjena iz na resurse.

Dodatne informacije o automatski skaliranje aplikacija potražite u članku [automatski skaliranje izračunati čvorove u programa Azure grupe](batch-automatic-scaling.md).

> [AZURE.TIP] Da biste maksimizirali računalnim Upotreba resursa, postavite ciljne broj čvorove nulu na kraju posla, ali dopustiti pokretanje zadataka da biste završili.

## <a name="security-with-certificates"></a>Sigurnost s potvrdama

Obično, morate koristiti potvrde kada šifriranje ili dešifriranje povjerljive podatke za zadatke, kao što je ključ za [račun za Azure pohranu][azure_storage]. Da biste to podržava, možete instalirati certifikata na čvorove. Šifrirane tajne su zadacima poslao parametara naredbenog retka ili uložiti u neki od resursa zadatka i instaliranih certifikati može se koristiti za dešifriranje ih.

Korištenje [certifikata Dodaj] [ rest_add_cert] operacija (obradu REST) ili [CertificateOperations.CreateCertificate] [ net_create_cert] način (grupe za .NET) da biste dodali certifikat grupe za račun. Zatim možete povezati certifikata radi novu ili postojeću grupu. Kada je certifikat povezan s zajedničko područje, servis za obradu instalira certifikata na svakom čvor u. Servis za obradu instalira odgovarajuće potvrde prilikom čvor pokretanja, prije pokretanja sve zadatke (uključujući zadatka Pokreni i Upravitelj zadatak).

Ako dodate potvrde u *postojeću* grupu, morate ponovno pokrenuti svoje čvorove računalnim za certifikate zatvoriti čvorove.

## <a name="error-handling"></a>Rukovanje pogreškama

Ponekad je potrebno učiniti zadataka i aplikacije pogreške unutar grupe rješenje.

### <a name="task-failure-handling"></a>Rukovanje neuspjeh zadatka
Do neuspjele zadatka svrstati u jednu od tih kategorija:

- **Planiranje pogreške**

    Ako prijenos datoteke koje su navedene za zadatak neuspješan iz bilo kojeg razloga, "Planiranje pogreška" postavljen za zadatak.

    Planiranje pogreške se može dogoditi ako datoteka resursa zadatka premještena, više nije dostupan prostor za pohranu računa ili neki drugi problem došlo je do koji spriječio uspješno kopiranje datoteke u čvor.

- **Do neuspjele aplikacije**

    Postupak naveden u zadatka naredbenog retka možete i neće uspjeti. Proces se smatra da nije uspjela prilikom osim nule izlaz kod je vratio postupak koji je izvršio zadatak (pogledajte *zadatka Izlazni kodovi* u sljedećem odjeljku).

    Za aplikacije pogreške, možete konfigurirati grupe automatski pokušati zadatka na određeni broj puta.

- **Ograničenja neuspjeha**

    Možete postaviti ograničenja koja određuje trajanje Maksimalna izvođenja za posao ili zadatak, a zatim *maxWallClockTime*. To može biti korisno za prekidanje "ne reagira" Zadaci.

    Kada je premašena maksimalnu količinu vremena, zadatak je označen kao *dovršen*, ali izlazni kod postavljen na `0xC000013A` i polje *schedulingError* označen kao `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Ispravljanje pogrešaka aplikacije neuspjeha

- `stderr`i`stdout`

    Tijekom izvođenja, aplikacija može proizvesti dijagnostičkih izlaza koje možete koristiti za otklanjanje poteškoća. Kao što je rečeno u prethodnom odjeljku [datoteke i mape](#files-and-directories), servis za obradu piše standardni izlazni i izlaz standardnu pogrešku u `stdout.txt` i `stderr.txt` datoteka u direktoriju zadatka na čvor računalnim. Portal za Azure ili jednu od obrade SDK-ovi možete koristiti da biste preuzeli te datoteke. Na primjer, možete dohvatiti te i druge datoteke za rješavanje problema pomoću [ComputeNode.GetNodeFile] [ net_getfile_node] i [CloudTask.GetNodeFile] [ net_getfile_task] u biblioteci .NET grupe.

- **Šifre izlaz zadatka**

    Kao što je rečeno ranije, zadatak je označen kao nije uspjela servis za obradu ako postupak koji je izvršio zadatak vraća kod osim nule izlaz. Kada se zadatak izvršava postupak, obradu popunjava zadatka izlaz kod svojstvo s *kod postupka*. Važno Imajte na umu da je zadatak izlazni kod **nije** određen servis za obradu – to ovisi o postupka sam ili operacijski sustav na kojem se izvršava postupak je.

### <a name="accounting-for-task-failures-or-interruptions"></a>Računovodstveni neuspjeha zadatak ili prekidi

Zadaci povremeno možda se neće niti se prekida. Same aplikacije zadatak možda neće uspjeti, čvor na kojemu je pokrenut zadatak možda ponovno pokrenuti ili čvor možda će se ukloniti iz na resurse tijekom operacije za promjenu veličine ako je na resurse deallocation pravilo postavljeno da biste uklonili čvorove odmah bez čekanja za zadatke da biste završili. U svakom slučaju, zadatak možete biti automatski premjestio po grupe za izvršavanje na drugi čvor.

Moguće je i Povremeni problem uzrokuju zadatak "smrzavanje" ili izveli predugačak izvršiti. Možete postaviti maksimalno vrijeme izvođenja zadatka. Ako je premašena obradu interrupts aplikacije zadatka.

### <a name="connecting-to-compute-nodes"></a>Povezivanje s čvorove za izračun

Možete izvršiti dodatne ispravljanje pogrešaka i otklanjanje poteškoća s prijavom na računalnim čvor daljinski. Portal za Azure možete koristiti da biste preuzeli datoteke protokola udaljene radne površine (RDP) za Windows čvorove i dobiti informacije o vezi sigurne ljuske (SSH) za čvorove Linux. Možete to učiniti i pomoću obrade API-ji – na primjer, [Grupe za .NET] [ net_rdpfile] ili [Python grupe](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] Da biste povezali čvor putem RDP ili SSH, prvo morate stvoriti korisnika na čvor. Da biste to učinili, možete koristiti Azure portala, [Dodavanje korisničkog računa radi čvor] [ rest_create_user] pomoću obrade REST API-JA poziva [ComputeNode.CreateComputeNodeUser] [ net_create_user] način obrade .NET i poziva [add_user] [ py_add_user] način u modulu Python grupe.

### <a name="troubleshooting-bad-compute-nodes"></a>Otklanjanje poteškoća "bad" izračunati čvorove

U slučajevima gdje su neki zadaci neuspješnih grupe za klijentski program ili servis možete provjeriti metapodatke neuspjelih zadataka da biste odredili misbehaving čvor. Svaki čvor u zajedničko područje dobiva jedinstveni ID, a čvor na kojem se izvodi zadatak je sve obuhvaćeno metapodataka zadatka. Nakon što ste označena čvor problem, možete poduzeti nekoliko je:

- **Ponovno čvor** ([REST][rest_reboot] | [.NET][net_reboot])

    Ponovno pokretanje čvor ponekad možete očistiti gore latent probleme kao što su zamrzne ili ruši procesa. Imajte na umu da ako vaše skup koristi zadatak start ili posla koristi Priprema zadatak, oni se izvode prilikom ponovnog pokretanja čvor.

- **Reimage čvor** ([REST][rest_reimage] | [.NET][net_reimage])

    To ponovno instalira sustav operacijski sustav na čvor. Kao i kod ponovnog pokretanja čvor, pokrenite zadaci, a zatim ponovno Zadaci pripreme posao pokrenite kada je omogućeno reimaged čvor.

- **Uklanjanje čvor iz na resurse** ([REST][rest_remove] | [.NET][net_remove])

    Ponekad je potrebno da biste potpuno uklonili čvor iz na resurse.

- **Onemogućivanje na čvor za zakazivanje zadataka** ([REST][rest_offline] | [.NET][net_offline])

    To učinkovito traje čvor "izvanmrežno" tako da nema daljnje se zadaci dodjeljuju na nju, no omogućuje čvor bude pokrenut i u. To vam omogućuje izvođenje daljnje istrage u uzroka na neuspjelih bez gubitka podataka nije uspjelo zadataka – i bez čvor uzrokuju pogreške dodatne zadatka. Ako, na primjer, možete onemogućiti Pregledajte zapisnike događaja u čvor ili izvršiti druge otklanjanje poteškoća za zakazivanje na čvor, [prijavite se u daljinski](#connecting-to-compute-nodes) zadataka. Nakon što završite vaše istrage, čvor mrežni način možete prenijeti pa omogućivanjem zakazivanje zadataka ([REST][rest_online] | [.NET][net_online]), ili neke druge akcije spomenuli izvršiti.

> [AZURE.IMPORTANT] Uz svaku akciju opisanu u ovoj sekciji – ponovno, reimage, uklanjanje i Onemogući zakazivanje zadataka – prikazuje vam se da biste odredili kako se upravlja zadataka koji se trenutno izvode na čvor prilikom izvođenja akcije. Ako, na primjer, kada onemogućite zadatak raspoređivanje na čvor pomoću grupe za .NET klijentska biblioteka, možete odrediti [DisableComputeNodeSchedulingOption] [ net_offline_option] redni broj da biste odredili hoće li za **Prekinite** zadataka koji se izvode, **Requeue** ih za zakazivanje na ostale čvorove ili dopustiti pokretanje zadataka da biste mogli izvršiti akciju (**TaskCompletion**).

## <a name="next-steps"></a>Daljnji koraci

- Prođite kroz u uzorka obradu aplikaciju detaljne na [Početak rada s bibliotekom Azure grupe za .NET](batch-dotnet-get-started.md). Postoji [Python verziju](batch-python-tutorial.md) vodič koji se izvodi radno opterećenje na čvorove računalnim Linux.

- Preuzimanje i stvaranje [Grupe za Explorer] [ github_batchexplorer] uzorka projekta za upotrebu tijekom razvijate rješenja za obradu. Pomoću programa Explorer grupe, možete izvršiti sljedeće i drugo:
  - Praćenje i upravljali grupe, zadataka i zadataka unutar grupe za račun
  - Preuzmite `stdout.txt`, `stderr.txt`, i druge datoteke iz čvorove
  - Stvaranje korisnika na čvorove i preuzimanje datoteka RDP za daljinsko prijavljivanje

- Saznajte kako [stvoriti grupe čvorove računalnim Linux](batch-linux-nodes.md).

- Posjetite [forum grupe za Azure] [ batch_forum] na MSDN-u. Forum dobro je mjesto za postavljanje pitanja, hoće li samo učenju ili su stručnjak pomoću grupe.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
