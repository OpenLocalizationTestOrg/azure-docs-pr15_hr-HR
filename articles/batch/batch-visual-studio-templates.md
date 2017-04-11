<properties
    pageTitle="Visual Studio predloške za obradu Azure | Microsoft Azure"
    description="Saznajte kako te predloške projekta za Visual Studio mogli implementirati i pokrenuti radnih opterećenja sustava računalnim ćete morati usko grupe za Azure"
    services="batch"
    documentationCenter=".net"
    authors="fayora"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="visual-studio-project-templates-for-azure-batch"></a>Visual Studio predlošcima projekata za obradu Azure

**Upravitelj posla** i **Visual Studio zadatka procesor predložaka** za obradu sadrže kod da biste lakše implementirati i pokrenuti radnih opterećenja sustava računalnim ćete morati usko obradu uz najmanju truda. U ovom dokumentu opisuje te predloške i sadrži upute kako ih koristiti.

>[AZURE.IMPORTANT] U ovom se članku govori o samo podatke primijeniti na te dvije predloške i pretpostavlja da ste upoznati s grupe servisa i koncepata povezana s njim: grupe, izračunati čvorove, zadataka i zadataka, posao upravitelja zadataka, varijable okruženja i relevantne informacije. Dodatne informacije možete pronaći u [Osnove grupe za Azure](batch-technical-overview.md), [grupe za pregled značajke za razvojne inženjere](batch-api-basics.md)i [Početak rada s bibliotekom Azure grupe za .NET](batch-dotnet-get-started.md).

## <a name="high-level-overview"></a>Pregled visoke razine

Upravitelj za posao i procesor zadatka Predlošci se mogu koristiti za stvaranje dvije korisne komponente:

* Upravitelj zadatak koji implementira razdjelnika posla koji možete raščlanjuju posao u više zadataka koji se može pokrenuti neovisno o paralelno.

* Procesor zadataka koji se mogu koristiti za izvođenje predobradu i nakon obrade oko naredbenog retka za aplikacije.

Na primjer, u scenariju s prikazom film razdjelnika posao želite pretvoriti film jedan zadatak u stotine ili tisuće odvojene zadatke koji želite obrađivati pojedinačne okvira zasebno. Način, procesor zadatak želite pozvati aplikacije vizualizacije i sve zavisne procesa koji su potrebni za prikazivanje svaki okvir, kao i sve dodatne akcije (na primjer kopiranje prikazanih okvir mjesto spremišta).

>[AZURE.NOTE] Upravitelj za posao i procesor zadatka Predlošci su neovisno o drugoga, pa možete koristiti oba ili samo jedan od njih, ovisno o preduvjetima računalnim posla i preference.

Kao što je prikazano u dijagramu u nastavku, računalnim posla koji koristi te predloške će proći kroz tri stanja:

1. Kod klijenta (npr., aplikacija web-servisa, itd.) šalje zadatak za obradu servis Azure, pri određivanju kao njegov posao upravitelja zadataka program manager posao.

2. Servis za obradu pokreće Upravitelj zadatak na računalnim čvor i razdjelnika posao pokreće na određeni broj zadatak procesor zadaci na kao mnoge izračunati čvorove prema potrebi, na temelju parametara i specifikacije posla kod razdjelnika.

3. Zadatak procesor zadaci zasebno, pokrenite paralelno, za obradu ulaznih podataka i generiranje za izlazne podatke.

![Dijagram s prikazom interakciju kod klijent sa servisom za obradu][diagram01]

## <a name="prerequisites"></a>Preduvjeti

Da biste koristili predloške za obradu, će vam sljedeće:

* Računalo s Visual Studio 2015, ili novija verzija, već instaliran na njemu.

* Predlošci za obradu koji su dostupni u [Galeriji Visual Studio] [ vs_gallery] kao proširenja za Visual Studio. Da biste dobili predložaka na dva načina:

  * Instalacija predložaka pomoću dijaloškog okvira **Extensions i ažuriranja** u Visual Studio (Dodatne informacije potražite u članku [pronalaženje i korištenje datotečne nastavke za Visual Studio][vs_find_use_ext]). U dijaloškom okviru **Extensions i ažuriranja** pretraživanje i preuzmite sljedeća dva extensions:

    * Azure upravitelja grupe za posao s posla razdjelnika
    * Procesor zadatka grupe za Azure

  * Preuzimanje predložaka iz galerije online za Visual Studio: [Predlošci projekta za Microsoft Azure grupe][vs_gallery_templates]

* Ako namjeravate koristiti značajku [Paketa aplikacije](batch-application-packages.md) za implementaciju upravitelja za posao i procesor zadatka s grupom za izračunavanje čvorove, morate se povezati s računom za pohranu na račun za obradu.

## <a name="preparation"></a>Priprema

Preporučujemo stvaranje rješenja koja sadrži vaš rukovoditelj posla, kao i vaš procesor zadatka jer to može olakšati isti broj između upravitelja za posao i programa za procesor zadatka. Da biste stvorili rješenja, slijedite ove korake:

1. Otvorite Visual Studio 2015 i odaberite **datoteku** > **Novo** > **projekta**.

2. U odjeljku **Predlošci**proširite **Drugih vrsta projekta**, kliknite **Visual Studio rješenja**, a zatim odaberite **Prazno rješenja**.

3. Upišite naziv koji opisuje vaše aplikacije i svrha rješenje (npr., "LitwareBatchTaskPrograms").

4. Da biste stvorili novo rješenje, kliknite **u redu**.

## <a name="job-manager-template"></a>Predložak Upravitelj posla

Predložak posao Upravitelj pomaže vam implementaciju zadatak Upravitelj koje možete izvršiti sljedeće radnje:

* Podijelite posao više zadataka.
* Slanje izvođenje na obradu tih zadataka.

>[AZURE.NOTE] Dodatne informacije o posao upravitelja zadataka potražite u članku [Pregled značajki grupe za razvojne inženjere](batch-api-basics.md#job-manager-task).

### <a name="create-a-job-manager-using-the-template"></a>Stvaranje zadatka Manager pomoću predloška

Da biste dodali posao Upravitelj rješenje koje ste prethodno stvorili, slijedite ove korake:

1. Otvaranje postojeće rješenje u Visual Studio 2015.

2. U pregledniku rješenja, desnom tipkom miša kliknite rješenje, kliknite **Dodaj** > **Novi projekt**.

3. U odjeljku **Visual C#**, kliknite **oblačić**, a zatim **Azure grupe za posao Manager s posla razdjelnika**.

4. Upišite naziv koji opisuje aplikacije i označava taj projekt kao upravitelj posla (npr. "LitwareJobManager").

5. Da biste stvorili projekta, kliknite **u redu**.

6. Na kraju, međuverziju projekta za Visual Studio učitavanje sve pakete referencirani NuGet i provjerite je li projekt valjan prije nego što počnete izmjenom prisilno.

### <a name="job-manager-template-files-and-their-purpose"></a>Datoteke predložaka upravitelja za posao i njihove svrhe

Kada stvorite pomoću predloška za posao Upravitelj projekta, generirat će se tri skupine kod datoteke:

* Glavni programske datoteke (Program.cs). Sadrži program točku unosa i rukovanje najviše razine iznimke. Obično nećete morati izmijeniti.

* Imenik Framework. To se nalaze datoteke koje su dužni 'predloženog' posla Upravitelj program posao – Raspakiravanje parametre, dodjeljivati zadatke na obradu itd. Obično bi trebalo morati izmijeniti te datoteke.

* Zadatak razdjelnika datoteka (JobSplitter.cs). Ovo je gdje će staviti logiku specifičnim aplikacijama za razdvajanje posao u zadatke.

Naravno možete dodati dodatne datoteke prema potrebi podržava posao kod razdjelnika, ovisno o složenosti posao Podjela logike.

Predložak stvara i standardne .NET projektnih datoteka kao što su datoteke .csproj, app.config, packages.config, itd.

Ostatak u ovom se odjeljku opisuju različite datoteke i strukturu kod te se objašnjava što čini svaki predmet.

![Visual Studio rješenje Explorer prikazuje upravitelju posao predložak rješenja][solution_explorer01]

**Framework datoteke**

* `Configuration.cs`: Encapsulates učitavanje posao konfiguracijske podatke kao što su pojedinosti o računu za obradu, povezane prostora za pohranu vjerodajnica računa, posla i informacije o zadatku i zadatka Parametri. Također, omogućuje pristup varijable okruženja definirane grupe (pogledajte postavki okruženja za zadatke u dokumentaciji za obradu) putem Configuration.EnvironmentVariable predmete.

* `IConfiguration.cs`: Abstracts implementaciju klase konfiguracije kako biste mogli test jedinica vašeg posla razdjelnika pomoću konfiguracije lažne ili mock objekta.

* `JobManager.cs`: Orchestrates komponente programa Upravitelj posao. Nije odgovoran za inicijalizaciju razdjelnika posla, pozivanje razdjelnika posla, a i isporuka zadaci vratila razdjelnika posao slanja zadatka.

* `JobManagerException.cs`: Predstavlja pogreške koje je potrebno Upravitelj zadatak da biste prekinuli. Da biste prelomili 'očekivani pogrešaka gdje se određeni dijagnostičke informacije može pružati kao dio prekid koristi se JobManagerException.

* `TaskSubmitter.cs`: Klase odgovorna dodjeljivati zadatke vratila razdjelnika posao obradom. Klase zbrajanja JobManager niz zadataka u grupama za učinkovito, ali pravovremeno dodavanjem posla, zatim poziva TaskSubmitter.SubmitTasks na pozadini niti za svaku seriju.

**Zadatak razdjelnika**

`JobSplitter.cs`: Klase sadrži logiku specifičnim aplikacijama za razdvajanje posao u zadatke. Okvir poziva JobSplitter.Split načina dobivanja niza zadataka, koji se dodaje se na posao kao metodu vraća ih. Ovo je klasa gdje ubacivanje logičku vrijednost posla. Implementacija način podjele da biste se vratili nizu CloudTask instanci koje označavaju zadatke u koju želite particija posao.

**Standardni .NET naredbenog retka projektnih datoteka**

* `App.config`: Standardne .NET aplikacije konfiguracijska datoteka.

* `Packages.config`: Standardne datoteke ovisnost paketa NuGet.

* `Program.cs`: Sadrži program točku unosa i rukovanje najviše razine iznimke.

### <a name="implementing-the-job-splitter"></a>Implementacijom razdjelnika posla

Kada otvorite projekta predloška Upravitelj posla, projekt dodijelit će datoteku JobSplitter.cs prema zadanim postavkama. Možete implementirati logike podjele zadataka u svoje radno opterećenje pomoću ispod Split() Prikaži način:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

>[AZURE.NOTE] Odjeljak označenim u na `Split()` način je samo dio šifru predloška Upravitelj zadatak koji je namijenjen za izmjenu dodavanjem logičku da biste podijelili poslova različitih zadataka. Ako želite da biste izmijenili različite sekcije predloška, provjerite je li se familiarized s obradu funkcioniranja, i isprobajte nekoliko [primjere koda za obradu][github_samples].

Split() implementaciju ima pristup:

* Parametri zadatak putem na `_parameters` polja.
* CloudJob objekt koji predstavlja posla, putem na `_job` polja.
* CloudTask objekt koji predstavlja Upravitelj zadatak putem na `_jobManagerTask` polja.

Vaše `Split()` implementaciju ne treba izravno dodavanje zadataka na posao. Umjesto toga kod mora vratiti niz CloudTask objekte i te automatski se dodaju na posao po klase framework koji pozivanje razdjelnika posao. Se zajednički koristi C#-iterator (`yield return`) značajku implementirati razdjelnike posla, kao što je time zadataka da biste pokrenuli izvodi čim umjesto čekanja za sve zadatke za izračun.

**Pogreška razdjelnika posla**

Ako vaš zadatak razdjelnika naiđe na pogrešku, što bi trebala jedan od sljedećih načina:

* Prekid slijed pomoću C# `yield break` naredbe, u kojem slučaju upravitelju posla tretirat će se kao uspjelo; ili

* Vratiti iznimku, u kojem se slučaju upravitelju posao će se smatrati nije uspjela i mogu ponoviti ovisno o tome kako mu klijent je konfiguriran).

U oba slučaja, svi zadaci već vratio razdjelnika posla i dodati obradu bit će uvjete za pokretanje. Ako ne želite da se to dogodi, zatim koje nije moguće:

* Prekid posla prije vraćanja iz razdjelnika posla

* Formulate zadatak za cijelu zbirku prije vraćanje (to jest, vratite se `ICollection<CloudTask>` ili `IList<CloudTask>` umjesto implementacije sustava razdjelnika posla pomoću C# iterator)

* Korištenje međuzavisnosti zadataka da biste sve zadatke koje ovise o uspješan dovršetak upravitelju posla

**Ponovne pokušaje Upravitelj posla**

Upravitelj posla ne uspije, možda pokušati servis obrade ovisno o postavkama pokušaj klijenta. Općenito govoreći, to je sigurnim, jer kada framework dodaje zadatke na poslu, zanemaruje sve zadatke koje već postoje. Međutim, ako izračun zadaci pozivnim, možda ne želite plaćati trošak ponovni izračun zadatke koji su već dodani na posao Nasuprot tome, ako ponovno pokrenite se zajamčeno da biste generirali isti zadatak ID-a zatim ponašanje "Zanemari duplikate" će ne pokretanje. U tim slučajevima treba dizajnirati svoje razdjelnika zadatak da biste otkrili posla koji već obavljeno, a ne Ponavljaj, primjerice izvođenjem na CloudJob.ListTasks prije nego što započnete s yield zadataka.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Izlaz iz šifre i iznimke u predlošku Upravitelj posla

Kodovi Izlaz i iznimke nude mehanizam da biste odredili rezultat pokretanju programa, a oni omogućuju da biste odredili problemi s izvođenja programa. Predložak posao Upravitelj implementira izlaz kodova i iznimke navedena u ovom odjeljku.

Upravitelj zadatak koji je primijenjeno pomoću predloška za posao Upravitelj možete se vratiti na tri moguće izlaz šifre:

| Kod | Opis |
|------|-------------|
| 0    | Upravitelj zadatak uspješno dovršena. Kod razdjelnika posao pokrenuli do kraja, a svi zadaci koje su dodane na posao. |
| 1    | Upravitelj zadatak nije uspjela uz iznimku u 'očekivani dio programa. Iznimka je preveden JobManagerException s Dijagnostičkim informacijama i, gdje je to moguće, prijedloge za ispravljanje pogreške. |
| 2    | Upravitelj zadatak nije uspjela uz iznimku "neočekivane". Iznimka je bio prijavljen u standardni Izlaz, ali Upravitelj posla nije moguće dodati dodatne informacije Dijagnostika ili olakšava. |

Slučaju posao upravitelja zadataka pogreške, neke zadatke možda i dalje su dodani na servis prije pogreške. Zadaci će se pokrenuti uobičajen. Potražite "Posao razdjelnika pogreška" iznad rasprave ovog puta kod.

Sve informacije o vratio iznimke zapisuje u stdout.txt i stderr.txt datoteke. Dodatne informacije potražite u članku [Rukovanje pogreškama](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>Razmatranja klijenta

U ovom se odjeljku opisuje neki preduvjeti za implementaciju klijenta kada se pozivanje upravitelja zadatak koji se temelji na predlošku. Saznajte [kako prosljeđivanja parametara i varijable okruženja iz klijenta kod](#pass-environment-settings) pojedinosti o Prosljeđivanje parametara i postavki okruženja.

**Obavezno vjerodajnice**

Da bi se Dodavanje zadataka Azure obradu Upravitelj zadatak potreban URL računa grupe za Azure i ključa. Prenesite ih u varijable okruženja pod nazivom YOUR_BATCH_URL i YOUR_BATCH_KEY. Ih možete postaviti u upravitelju posao postavki okruženja zadatka. Na primjer, u C# klijent:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Vjerodajnice za pohranu**

Obično klijent nisu potrebne vjerodajnice računa povezane prostora za pohranu za Upravitelj zadatak jer (a) najčešće posao upravitelji ne morate izričito pristup računu povezane prostora za pohranu i (b) račun za povezane prostora za pohranu često dostupni su svi zadaci kao uobičajena postavka okruženje za posao. Ako ne pruža računa povezane prostora za pohranu putem uobičajenih postavki okruženja, a posao manager potreban pristup povezane za pohranu, zatim treba unesete vjerodajnice povezane prostora za pohranu na sljedeći način:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Postavke za posao upravitelja zadataka**

Klijent postavite *killJobOnCompletion* zastavicu za zadatak Upravitelj **False**.

Obično se preporučuje za klijentski program da biste postavili *runExclusive* **False**.

Klijent trebali biste koristiti zbirke *resourceFiles* ili *applicationPackageReferences* da bi se zadatak Upravitelj izvršna (i njezin potrebna DLL bibliotekama) implementiran na čvor računalnim.

Po zadanom upravitelju posla će ne može pokušati ako ne uspijete. Ovisno o upravitelju logiku posao klijent možda htjeti omogućiti ponovne pokušaje putem *ograničenja*/*maxTaskRetryCount*.

**Postavke zadatka**

Ako razdjelnika posao emits zadataka s ovisnosti, klijent morate postaviti posla usesTaskDependencies na true.

U tom modelu razdjelnika posla je neobično za klijente za želite dodati zadataka u zadacima nastali što stvara zadatak razdjelnika. Klijent treba stoga obično postavite posla *onAllTasksComplete* **terminatejob**.

## <a name="task-processor-template"></a>Predložak procesor zadatka

Predložak procesor zadatka pomaže vam implementaciju procesor zadataka koje možete izvršiti sljedeće radnje:

* Postavljanje informacija potrebnih za svaki zadatak grupe da biste pokrenuli.
* Pokreni sve akcije potrebnih svaki zadatak grupe.
* Spremite zadatak izlaze stalnih prostor za pohranu.

Iako zadatka procesor nisu potrebni za izvođenje zadataka na obradu, ključne prednost korištenja zadatka procesor je pruža omot za implementaciju sve akcije za izvršavanje zadataka na jednom mjestu. Na primjer, ako morate pokrenuti nekoliko aplikacija u kontekstu svakog zadatka ili ako vam je potrebna da biste kopirali podatke stalnih prostor za pohranu nakon dovršetka svakog zadatka.

Akcije obavlja procesor zadatka može biti kao jednostavnih ili kompleksnih, a koliko ili mali, po potrebi po svoje radno opterećenje. Uz to, implementacijom sve akcije zadatka u procesor od najmanje jedan zadatak možete lako ažuriranje ili dodajte akcije na temelju promjena aplikacije ili radno opterećenje preduvjeti. No u nekim slučajevima zadatka procesor možda neće biti optimalnih rješenja za implementaciju možete dodati nepotrebne složenosti, primjerice kada izvođenje zadataka koji se brzo započeli putem jednostavne naredbenog retka.

### <a name="create-a-task-processor-using-the-template"></a>Stvaranje zadatka procesor pomoću predloška

Da biste dodali zadatka procesor rješenje koje ste prethodno stvorili, slijedite ove korake:

1. Otvaranje postojeće rješenje u Visual Studio 2015.

2. U pregledniku rješenja, desnom tipkom miša kliknite rješenje, kliknite **Dodaj**, a zatim kliknite **Novi projekt**.

3. U odjeljku **Visual C#**, kliknite **oblačić**, a zatim **Azure obradu zadatka procesor**.

4. Upišite naziv koji opisuje aplikacije i označava taj projekt kao procesor zadatka (npr. "LitwareTaskProcessor").

5. Da biste stvorili projekta, kliknite **u redu**.

6. Na kraju, međuverziju projekta za Visual Studio učitavanje sve pakete referencirani NuGet i provjerite je li projekt valjan prije nego što počnete izmjenom prisilno.

### <a name="task-processor-template-files-and-their-purpose"></a>Datoteke predložaka procesor zadatka i njihove svrhe

Kada stvorite projektu pomoću predloška za procesor zadataka, generirat će se tri skupine kod datoteke:

* Glavni programske datoteke (Program.cs). Sadrži program točku unosa i rukovanje najviše razine iznimke. Obično nećete morati izmijeniti.

* Imenik Framework. To se nalaze datoteke koje su dužni 'predloženog' posla Upravitelj program posao – Raspakiravanje parametre, dodjeljivati zadatke na obradu itd. Obično bi trebalo morati izmijeniti te datoteke.

* Zadatak procesor datoteka (TaskProcessor.cs). Ovo je gdje će staviti logiku aplikacije specifičnih za izvođenje zadatka (obično po pozivanje na postojeće izvršne datoteke). Prije i poslije obradi kod, kao što su preuzimanje dodatnih podataka i prijenos datoteka rezultat, također Ovdje dolazi.

Naravno možete dodati dodatne datoteke prema potrebi podržava zadatka kod procesor, ovisno o složenosti posao Podjela logike.

Predložak stvara i standardne .NET projektnih datoteka kao što su datoteke .csproj, app.config, packages.config, itd.

Ostatak u ovom se odjeljku opisuju različite datoteke i strukturu kod te se objašnjava što čini svaki predmet.

![Visual Studio rješenje Explorer prikazuje predložak rješenje procesor zadatka][solution_explorer02]

**Framework datoteke**

* `Configuration.cs`: Encapsulates učitavanje posao konfiguracijske podatke kao što su pojedinosti o računu za obradu, povezane prostora za pohranu vjerodajnica računa, posla i informacije o zadatku i zadatka Parametri. Također, omogućuje pristup varijable okruženja definirane grupe (pogledajte postavki okruženja za zadatke u dokumentaciji za obradu) putem Configuration.EnvironmentVariable predmete.

* `IConfiguration.cs`: Abstracts implementaciju klase konfiguracije kako biste mogli test jedinica vašeg posla razdjelnika pomoću konfiguracije lažne ili mock objekta.

* `TaskProcessorException.cs`: Predstavlja pogreške koje je potrebno Upravitelj zadatak da biste prekinuli. Da biste prelomili 'očekivani pogrešaka gdje se određeni dijagnostičke informacije može pružati kao dio prekid koristi se TaskProcessorException.

**Procesor zadatka**

* `TaskProcessor.cs`: Pokreće zadatak. Okvir poziva metodu TaskProcessor.Run. Ovo je klasa gdje ubacivanje specifičnim aplikacijama logičku vrijednost zadatka. Implementacija u način pokretanja:
  * Raščlanjivanje i provjeriti parametre zadatka
  * Sastavite naredbenog retka za bilo koje vanjski program koji želite pozvati
  * Prijavite se sve dijagnostičke informacije biti potrebna za potrebe za ispravljanje pogrešaka
  * Pokretanje procesa pomoću tog naredbenog retka
  * Pričekajte da postupak da biste izašli iz
  * Snimite izlazni kod postupka da biste odredili ako je uspjelo ili nije uspjela
  * Spremite sve izlazne datoteke koje želite zadržati stalnih prostor za pohranu

**Standardni .NET naredbenog retka projektnih datoteka**

* `App.config`: Standardne .NET aplikacije konfiguracijska datoteka.
* `Packages.config`: Standardne datoteke ovisnost paketa NuGet.
* `Program.cs`: Sadrži program točku unosa i rukovanje najviše razine iznimke.

## <a name="implementing-the-task-processor"></a>Procesor zadatka implementacije

Kada otvorite predložak projekta procesor zadatak projekta dodijelit će datoteku TaskProcessor.cs prema zadanim postavkama. Možete implementirati izvođenja logike za zadatke u svoje radno opterećenje metodom Run() prikazano u nastavku:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
>[AZURE.NOTE] Odjeljak označenim u način Run() je samo dio šifru predloška procesor zadatak koji je namijenjen za izmjenu dodavanjem izvođenja logike za zadatke u svoje radno opterećenje. Ako želite da biste izmijenili različite sekcije predloška, najprije Upoznajte se s funkcioniranje obrade tako da pročitate u dokumentaciji za obradu i isprobavate nekoliko primjere koda za obradu.

Metoda Run() je zadužen za pokretanje u naredbenom retku, počevši jednom ili više procesa, Čeka se sve postupak da biste dovršili, spremanje rezultata i na kraju povratkom s izlazni kod. Način Run() je gdje implementirati logiku obrade za svoje zadatke. Procesor framework zadatka poziva metodu Run() umjesto vas; ne morate pozvati sami.

Run() implementaciju ima pristup:

* Parametri zadatak putem na `_parameters` polja.
* ID posao i zadatak putem na `_jobId` i `_taskId` polja.
* Konfiguriranje zadatka putem na `_configuration` polja.

**Pogreška zadatka**

U slučaju Run() način možete zatvoriti prijavi iznimka, ali to ostavlja rukovatelj gornje razine iznimku potpunu kontrolu nad izlazni kod zadatka. Ako je potrebno kontrolirati izlazni kod tako da se različite vrste pogreške, možete, primjerice razlikovanje dijagnostičkih svrhe ili jer nekoliko načina pogreške treba prekid posla, a drugi ne bi trebala trebali biste napustili način Run(), vraćanje kod izlaz od nule. To će biti izlazni kod zadatka.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Izlaz iz šifre i iznimke u predlošku procesor zadatka

Kodovi Izlaz i iznimke nude mehanizam da biste odredili ishodu pokretanju programa, a može pomoći identificirati probleme s izvođenja programa. Predložak zadatka procesor implementira izlaz kodova i iznimke navedena u ovom odjeljku.

Procesor zadatkom koji je primijenjeno s predloškom procesor zadatak možete se vratiti na tri moguće izlaz šifre:

| Kod | Opis |
|------|-------------|
|  [Process.ExitCode][process_exitcode] | Procesor zadatka pokrenuli do kraja. Imajte na umu da to ne znači da program koji poziva nije uspjelo – samo procesor zadatak uspješno je pozvati te izvršiti sve nakon obrade bez iznimke. Značenje izlazni kod ovisi o programu pozvana – obično se izlazni kod 0 znači da je uspjelo program i izlazni kod znači da program nije uspio. |
| 1    | Procesor zadatka nije uspjela uz iznimku u 'očekivani dio programa. Iznimka je preveden na `TaskProcessorException` s Dijagnostičkim informacijama i, gdje je to moguće, prijedloge za ispravljanje pogreške. |
| 2    | Procesor zadatka nije uspjela uz iznimku "neočekivane". Iznimka je bio prijavljen u standardni Izlaz, ali procesor zadatka nije uspio dodajte dodatne Dijagnostika ili olakšava informacije. |

>[AZURE.NOTE] Ako program koji poziva koristi izlaz kodovi 1 i 2 da biste naznačili načini određene pogreške, korištenje izlaz kodovi 1 i 2 pogrešaka procesor zadatak je prevelika. Možete promijeniti te šifre pogrešaka za zadatak procesor u isticanjem izlaz kodovi uređivanjem slučajeva iznimke u datoteci Program.cs.

Sve informacije o vratio iznimke zapisuje u stdout.txt i stderr.txt datoteke. Dodatne informacije potražite u članku Rukovanje pogreškama u dokumentaciji grupe.

### <a name="client-considerations"></a>Razmatranja klijenta

**Vjerodajnice za pohranu**

Ako vaš zadatak procesor koristi spremište blobova platforme Azure održati izlaze, na primjer pomoću pomoćnih biblioteka konvencije datoteke pa ga treba pristup *ili* oblaka za pohranu poslovni subjekt vjerodajnice *ili* blob spremnik URL-a koji uključuje zajednički pristup potpis (SAS). Predložak uključuje podrška za pružanje vjerodajnice putem uobičajenih varijable okruženja. Klijent prenositi vjerodajnice za pohranu na sljedeći način:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Račun za pohranu zatim dostupna je u TaskProcessor predmete putem na `_configuration.StorageAccount` svojstvo.

Ako biste radije da biste koristili spremnik URL-a s SAS, što to prođete i putem programa posao uobičajena okruženje postavka, ali procesor predložak zadatka trenutno ne uključuje ugrađenu podršku za to.

**Postavljanje prostora za pohranu**

Preporučuje se da upravitelj zadataka klijenta ili posao stvoriti sve spremnika potrebnih zadataka prije nego što dodate zadatke na posao. To je obavezan ako koristite spremnik URL-a s SAS, kao što su URL-a ne obuhvaća dozvolu za stvaranje spremnika. Preporučuje se čak i ako vjerodajnice za pohranu računa, kao što je sprema svakog zadatka potrebe za pozivanje CloudBlobContainer.CreateIfNotExistsAsync na spremnik.

## <a name="pass-parameters-and-environment-variables"></a>Prosljeđivanje parametara i varijable okruženja

### <a name="pass-environment-settings"></a>Prenesite postavki okruženja

Klijent prenositi informacije da biste zadatak Upravitelj u obliku postavki okruženja. Ove informacije možete zatim koristit Upravitelj zadatak pri stvaranju zadatak procesor zadaci koji će se izvoditi kao dio posla računalnim. Primjeri informacije koje možete proslijediti kao postavki okruženja su:

* Tipke ime i račun za pohranu računa
* URL za obradu računa
* Ključ za račun grupe

Servis za obradu sadrži jednostavne mehanizam za prosljeđivanje postavki okruženja pomoću upravitelja zadatak na `EnvironmentSettings` svojstvo u [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Na primjer, da biste na `BatchClient` instancu za obradu račun, možete proslijediti kao varijable okruženja iz klijentskog programa kod URL i zajedničke ključne vjerodajnice za grupe za račun. Isto tako, za pristup računu za pohranu koji je povezan serije računa, naziv računa za pohranu i spremište ključ za račun možete proslijediti kao varijable okruženja.

### <a name="pass-parameters-to-the-job-manager-template"></a>Prosljeđivanje parametara predložak Upravitelj posla

U mnogim slučajevima je korisno za prosljeđivanje parametara po posao upravitelja zadataka posla da biste odredili posao Podjela postupak ili da biste konfigurirali zadataka za posao. To možete učiniti tako da prenesete JSON datoteku pod nazivom parameters.json kao datoteka resursa za Upravitelj zadatak. Parametri zatim može postati dostupna u na `JobSplitter._parameters` u predlošku Upravitelj posao.

>[AZURE.NOTE] Rukovatelj ugrađene parametar podržava samo rječnici niza u nizu. Ako želite proslijediti složene JSON vrijednosti kao vrijednosti parametara, morat ćete prenesite ih u nizove i analizirati ih u razdjelnika posao ili izmjena na framework `Configuration.GetJobParameters` način.

### <a name="pass-parameters-to-the-task-processor-template"></a>Prosljeđivanje parametara predložak procesor zadatka

Prenositi parametara za pojedine zadatke implementirati pomoću predloška procesor zadatka. Kao što s predloškom Upravitelj posao procesor predložak zadatka traži resursa datoteku pod nazivom

PARAMETERS.JSON, a ako je pronađeno učitava kao rječnik parametara. Postoji nekoliko mogućnosti za prosljeđivanje parametara zadatak procesor zadaci:

* Ponovno korištenje JSON zadatka Parametri. To funkcionira i ako su parametre samo one razini posla (na primjer, s renderiranje visina i širina). Da biste to učinili, prilikom stvaranja na CloudTask u razdjelnika posla, dodajte referenca na objekt datoteka resursa parameters.json iz ResourceFiles Upravitelj zadatak (`JobSplitter._jobManagerTask.ResourceFiles`) u CloudTask ResourceFiles zbirku.

* Generiranje prijenos dokumenta parameters.json specifične za zadatak kao dio posla razdjelnika izvođenja i upućuju na tom blob u zbirci datoteka resursa za zadatka. To je potrebno ako različitih zadataka druge parametre. Primjer može biti scenarij 3D renderiranje gdje će se okvir indeks proslijeđena zadatak kao parametar.

>[AZURE.NOTE] Rukovatelj ugrađene parametar podržava samo rječnici niza u nizu. Ako želite proslijediti složene JSON vrijednosti kao vrijednosti parametara, morat ćete prenesite ih u nizove i analizirati ih u procesor zadatak ili izmjena na framework `Configuration.GetTaskParameters` način.

## <a name="next-steps"></a>Daljnji koraci

### <a name="persist-job-and-task-output-to-azure-storage"></a>Zadržava posao i zadatak izlaz za pohranu za Azure

Neki drugi alat korisno u razvoju rješenja za obradu je [Azure obradu datoteka konvencije][nuget_package]. Koristite ovu .NET biblioteku klase (trenutno u pretpregledu), u .NET grupe aplikacija za pohranu i dohvaćanje izlaze zadatka i iz Azure prostora za pohranu. [Zadržava Azure obrada i zadatka izlaz](batch-task-output.md) sadrži cjelokupnu raspravu biblioteke i njegova korištenja.

### <a name="batch-forum"></a>Forum za obradu

[Forum za obradu Azure] [ forum] na MSDN-sjajno su početno mjesto raspravljati grupe i postavljati pitanja o servisu. Glavni na putem za korisno "ljepljive" objave, i pitanja kao što su biste tijekom stvaranja vašeg rješenja za obradu.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
