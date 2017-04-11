<properties
    pageTitle="Upravljanje u seriji Azure instalacijom jednostavno aplikacije i | Microsoft Azure"
    description="Korištenje značajke pakete aplikacija Azure serije jednostavno upravljanje više aplikacija i verzija za instalaciju na obradu izračunati čvorove."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="application-deployment-with-azure-batch-application-packages"></a>Implementacija aplikacije s paketa aplikacije za grupe za Azure

Značajka paketa aplikacije Azure serije omogućuje jednostavno Upravljanje aplikacijama zadataka i njihovih implementacije čvorove računalnim u svoje grupe aplikacija. Paketa aplikacije, možete prenijeti i upravljanje više verzija aplikacije pokrenuli zadataka, uključujući svoje datoteke podrške. Možete zatim automatski implementirati jedan ili više od tih aplikacija za čvorove računalnim u svoje grupe aplikacija.

U ovom se članku će Saznajte kako prenijeti i upravljanje paketa aplikacije na portalu za Azure. Pa ćete informacije o da biste ih instalirali na skup računalnim čvorove s [.NET obradu] [ api_net] biblioteke.

> [AZURE.NOTE] Značajka paketa aplikacije ovdje opisane zamjenjuje značajku "Obradu aplikacije" u prethodnim verzijama servisa.

## <a name="application-package-requirements"></a>Preduvjeti za aplikaciju paketa

Na račun za obradu da biste koristili aplikaciju paketa morate [vezu račun za Azure prostora za pohranu](#link-a-storage-account) .

Značajka paketa aplikacije spominju u ovom članku je kompatibilan *samo* s obradu grupe koje su stvorene nakon 10 ožujak 2016. Pakete aplikacija će biti implementirano za izračunavanje čvorove u grupe stvorene prije tog datuma.

Značajka je uvedena u [Grupe REST API -JA] [ api_rest] verzija 2015 12 01.2.2 i odgovarajuće [Grupe za .NET] [ api_net] verzija za biblioteku 3.1.0. Preporučujemo da uvijek koristite najnoviju verziju API prilikom rada s grupe.

> [AZURE.IMPORTANT] Trenutno samo *CloudServiceConfiguration* grupe podržava paketa aplikacije. Pakete aplikacija ne možete koristiti u grupe stvoren pomoću VirtualMachineConfiguration slike. U odjeljku [Konfiguriranje virtualnog računala](batch-linux-nodes.md#virtual-machine-configuration) od [dodjele Linux izračun čvorove u grupe za Azure grupe](batch-linux-nodes.md) dodatne informacije o dvije različite konfiguracije.

## <a name="about-applications-and-application-packages"></a>O aplikacijama i paketa aplikacije

Unutar grupe za Azure *aplikacije* se odnosi na skup određuju binarne datoteke koja se automatski preuzeti na računalnim čvorove u svoje grupe aplikacija. *Aplikacijski paket* odnosi na *određeni skup* te binarne datoteke te predstavlja navedeni *verzije* aplikacije.

![Više razine dijagram aplikacija i paketa aplikacije][1]

### <a name="applications"></a>Aplikacija

Aplikacija u seriji sadrži jednu ili više aplikacija paketi i određuje mogućnosti konfiguracije za aplikaciju. Aplikacije, na primjer, možete odrediti zadanu verziju paketa aplikacije da biste instalirali na računalnim čvorove i li mogu ažurirati ili izbrisati njegov paketa.

### <a name="application-packages"></a>Pakete aplikacija

Aplikacijski paket je .zip datoteku koja sadrži binarne datoteke iz aplikacije i datoteke podrške koje su potrebne za izvršavanje po zadataka. Svaki Aplikacijski paket predstavlja određene verziju aplikacije.

Možete navesti paketa aplikacije na razini grupe aplikacija i zadatke. Prilikom stvaranja grupe aplikacija ili zadatka možete navesti jednu ili više ovih paketa i (nije obavezno) verzije.

* **Skup pakete** uvode se *svaki* čvor u. Aplikacije su uveden kada čvor spaja zajedničko područje, a kada je sustava ili reimaged.

    Skup pakete su odgovarajuće kada sve čvorove u zajedničko područje izvršavanje zadataka s posla. Jedan ili više paketa aplikacije možete odrediti kada stvarate zajedničko područje, a možete dodati ili ažurirati postojeće skup pakete. Ako ažurirate paketa aplikacije za postojeću grupu, morate ponovno pokrenuti svoje čvorove da biste instalirali paket.

* **Zadatak pakete** uvode se samo na računalnim čvor zakazano izvođenje zadatka, neposredno prije pokretanja zadatka naredbenog retka. Ako navedeni Aplikacijski paket i verzija je već čvor, ona se neće ponovno implementirati i koristi postojećeg paketa.

    Zadatak pakete korisne su u zajednički skup okruženja, pri čemu se izvode drugi poslovi na jedan skup, a na resurse neće se izbrisati nakon dovršetka posla. Ako vaš posao ima manje zadaci od čvorove u, pakete zadatak možete minimizirati prijenos podataka jer je aplikacija implementiran samo na čvorove koji se izvode zadatke.

    Ostale su prednosti paketa aplikacije zadatka scenarija poslove koje koriste osobito velike aplikacije, ali samo mali broj zadataka. Na primjer, unaprijed obradi faza ili zadatak pisma, pri čemu je stara obrada i cirkularnih pisama aplikacije heavyweight.

> [AZURE.IMPORTANT] Postoje ograničenja broja aplikacija i paketa aplikacije unutar grupe računa, kao i veličinu paketa Maksimalna aplikacije. Potražite u članku [kvotama i ograničenja servisa za obradu Azure](batch-quota-limit.md) detalje o ta ograničenja.

### <a name="benefits-of-application-packages"></a>Pogodnosti paketa aplikacije

Pakete aplikacija možete pojednostaviti kod u rješenje grupe i donje indirektni potrebne za upravljanje aplikacijama koji se izvode zadataka.

Vaš skup početka zadatka nema da biste odredili dugačak popis datoteka pojedinačnog resursa za instalaciju na čvorove. Ne morate ručno upravljanje više verzija sustava Azure prostora za pohranu ili na vašem čvorove datoteka aplikacije. Možete i ne morate brinuti o generiranje [URL-ovi SAS](../storage/storage-dotnet-shared-access-signature-part-1.md) omogućuje pristup datotekama na vašem računu za pohranu. Obradu radi u pozadini s Azure prostor za pohranu za pohranu paketa aplikacije i uvesti ih za izračunavanje čvorove.

## <a name="upload-and-manage-applications"></a>Prijenos i upravljanje aplikacijama

Možete koristiti [portal za Azure] [ portal] ili u biblioteci [.NET upravljanje grupe](batch-management-dotnet.md) da biste upravljali pakete aplikacija na vašem računu grupe. U sljedećim odjeljcima nekoliko smo najprije povezati s računom za pohranu, a zatim raspravljati dodavanjem aplikacija i pakete i upravljanje njima pomoću portala za.

### <a name="link-a-storage-account"></a>Povezivanje poslovnog subjekta prostora za pohranu

Da biste koristili aplikaciju paketa, najprije morate povezivanje poslovnog subjekta Azure prostora za pohranu na račun za obradu. Ako još niste konfigurirali račun za pohranu za vaš račun za obradu Azure portal prikazat će se upozorenje kada prvi put kliknete pločicu **aplikacije** u plohu **grupe za račun** .

> [AZURE.IMPORTANT] Obrada trenutno podržava *samo* vrstu računa za pohranu za **opće namjene** kao što je opisano u koraku 5, [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account), u [račune za pohranu o Azure](../storage/storage-create-storage-account.md). Kad povežete račun za Azure prostora za pohranu na obradu račun, veze *samo* na račun za pohranu **opće namjene** .

![Bez upozorenja za račun koji je konfiguriran za pohranu Azure portalu][9]

Servis za obradu koristi pridruženog računa za pohranu za pohranu i dohvaćanje paketa aplikacije. Kada ste povezani dva računa, grupe možete implementirati automatski paketa pohranjene u povezani poslovni subjekt za pohranu u vašem računalnim čvorove. Kliknite **Postavke računa za pohranu** na plohu **upozorenja** , a zatim **Račun za pohranu** na **Račun za pohranu** plohu da biste se povezali s računom za pohranu na račun za obradu.

![Odaberite plohu račun za pohranu na portalu za Azure][10]

Preporučujemo stvaranje je prostora za pohranu računa *Posebno* za korištenje s računom za obradu i odaberite ovdje. Detalje o stvaranju s računom za pohranu potražite u članku "Stvaranje računa za pohranu" u [Računi za pohranu o Azure](../storage/storage-create-storage-account.md). Nakon stvaranja računa za pohranu koji ga možete povezati s računom za obradu pomoću **Računa za pohranu** plohu.

> [AZURE.WARNING] Budući da obrade koristi Azure prostora za pohranu za pohranu paketi za aplikaciju, se [naplaćuje kao običnu] [ storage_pricing] za blokiranje blob podatke. Svakako razmislite o veličini i broju paketa aplikacije i povremeno uklonite zastarjeli pakete da biste minimizirali trošak.

### <a name="view-current-applications"></a>Prikaz trenutnog aplikacija

Da biste pregledali aplikacije na vašem računu grupe, kliknite stavku izbornika **aplikacije** na lijevom izborniku prilikom pregleda plohu **grupe za račun** .

![Pločica aplikacije][2]

Otvorit će se plohu **aplikacije** :

![Popis aplikacija][3]

Plohu **aplikacija** prikazuje ID svaku aplikaciju vaš račun i sljedeća svojstva:

* **Paketi**– broj verzija koje su povezane s ovog računala.
* **Zadana verzija**– na verziju koja će se instalirati ako ne navedete verziju prilikom postavljanja aplikacije za zajedničko područje. Ovaj korak nije obavezan.
* **Dopusti ažuriranja**– vrijednost koja određuje hoće li se paketa ažuriranja, brisanja i dodatke dopušteno. Ako je to je postavljeno na **ne**, paketa ažuriranja i brisanja onemogućene su za aplikaciju. Moguće je dodavati samo nove aplikacije paketa verzije. Zadano je **da**.

### <a name="view-application-details"></a>Prikaz detalja iz aplikacije

Kliknite aplikaciju u **aplikacije** plohu da biste otvorili plohu koji sadrži detalje za tu aplikaciju.

![Detalji o aplikaciji][4]

U pojedinosti plohu aplikacija možete konfigurirati sljedeće postavke za aplikaciju.

* **Dopusti ažuriranja**– odredite hoće li njegov paketa aplikacije mogu ažurirati ili izbrisati. Potražite u članku "Ažuriranje i brisanje Aplikacijski paket" u nastavku ovog članka.
* **Zadanu verziju**– odredite zadani Aplikacijski paket za implementaciju za izračunavanje čvorove.
* **Zaslonsko ime**– navedite "prikladnu" naziv koji seriju rješenje možete koristiti kad se prikazuje podatke o aplikaciji, u korisničkom Sučelju servisa koji omogućuju klijentima putem grupe.

### <a name="add-a-new-application"></a>Dodavanje nove aplikacije

Da biste stvorili novu aplikaciju, dodajte paketa aplikacije i navedite ID-a za novo, jedinstveno aplikacije Prvi Aplikacijski paket koji dodajete novi ID aplikacije će stvoriti novu aplikaciju.

Kliknite **Dodaj** na plohu **aplikacije** da biste otvorili plohu **nove aplikacije** .

![Nova aplikacija plohu Azure portalu][5]

Plohu **nove aplikacije** sadrži sljedeća polja da biste odredili postavke nove aplikacije i Aplikacijski paket.

**Id aplikacije**

U ovom polju navodi ID novu aplikaciju, koje se mogu standardne Azure obradu ID pravila za provjeru valjanosti:

* Mogu sadržavati bilo koju kombinaciju alfanumeričkih znakova, uključujući rastavnice i podvlake.
* Ne smije sadržavati više od 64.
* Mora biti jedinstvena u grupe za račun.
* Je slučaja čuvanje i predmet razlikuje naglaske.

**Verzija**

Određuje verziju Aplikacijski paket koji prenosite. Verzija nizovi podliježe sljedeća pravila za provjeru valjanosti:

* Mogu sadržavati bilo koju kombinaciju alfanumeričkih znakova, uključujući crtice, podvlake i razdoblja.
* Ne smije sadržavati više od 64.
* Mora biti jedinstvena u aplikaciji.
* Slova očuvanje i slova.

**Aplikacijski paket**

To polje određuje .zip datoteku koja sadrži binarne datoteke iz aplikacije i datoteke podrške koje su potrebne za izvršavanje aplikacije. Kliknite okvir za **Odabir datoteke** ili ikonu mape da biste pronađite i odaberite .zip datoteku koja sadrži vaše aplikacije datoteke.

Nakon što ste odabrali datoteku, kliknite **u redu** da biste započeli prijenos Azure prostora za pohranu. Nakon dovršetka postupka prijenos, bit ćete obaviješteni i na plohu će se zatvoriti. Ovisno o veličini datoteke koju prenosite i brzinu mrežnu vezu, ovaj postupak može potrajati neko vrijeme.

> [AZURE.WARNING] Prije dovršetka prijenosa operacije, zatvorite plohu **nove aplikacije** . Time će se prestati postupka prijenos.

### <a name="add-a-new-application-package"></a>Dodavanje novog paketa aplikacije

Da biste dodali novu verziju paketa aplikacije za postojeće aplikacije, odaberite aplikaciju u **aplikacije** plohu, kliknite **paketa**, a zatim kliknite **Dodaj** da biste otvorili plohu **Dodaj paketa** .

![Dodavanje aplikacije paketa plohu Azure portalu][8]

Kao što možete vidjeti polja podudaraju s dimenzijama plohu **nove aplikacije** , ali je onemogućen okvir **id aplikacije** . Kao što je niste za novu aplikaciju u određivanje **verzije** za novi paket, dođite do datoteke .zip **Aplikacijski paket** , a zatim kliknite **u redu** da biste prenijeli paketa.

### <a name="update-or-delete-an-application-package"></a>Ažuriranje i brisanje Aplikacijski paket

Ažuriranje ili brisanje postojećeg Aplikacijski paket, otvorite plohu detalje za aplikaciju, kliknite **paketa** da biste otvorili plohu **paketa** , kliknite **tri točke** u retku Aplikacijski paket koji želite izmijeniti i odaberite akciju koju želite izvesti.

![Ažuriranje i brisanje paketa Azure portalu][7]

**Ažuriranje**

Kada kliknete **ažuriranja**, prikazuje se plohu *paketa ažuriranja* . Ovaj plohu slično je plohu *Novi Aplikacijski paket* no samo paketa odabrano polje omogućena, što vam omogućuje da odredite ZIP datoteke za prijenos.

![Ažuriranje plohu paket na portalu za Azure][11]

**Brisanje**

Kada kliknete **Izbriši**, se od vas zatraži da potvrdite brisanje verzije paketa, a obradu briše paketa iz spremišta Azure. Ako izbrišete na zadanu verziju aplikacije, postavka **zadanu verziju** se uklanja za aplikaciju.

![Brisanje aplikacije][12]

## <a name="install-applications-on-compute-nodes"></a>Instaliranje aplikacija na računalnim čvorove

Sad kad ste vidjeti kako upravljati paketa aplikacije s portala za Azure, ne možemo navode kako uvesti ih izračun čvorove i pokretanje ih sa zadacima grupe.

### <a name="install-pool-application-packages"></a>Instaliranje paketa aplikacije za grupu

Da biste instalirali paket aplikacije na sve čvorove računalnim u zajedničko područje, navedite jednu ili više računala paketa *reference* za na resurse. Paketa aplikacije koje navedete za zajedničko područje su instalirani na svakom čvor računalnim kada se taj čvor spaja na resurse i kada je čvor sustava ili reimaged.

U .NET grupe, navedite jednu ili više [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref] prilikom stvaranja novog skupa ili za postojeće grupe aplikacija. [ApplicationPackageReference] [ net_pkgref] predmete određuje poruku ID aplikacije i verziju da biste instalirali na na skup izračunati čvorove.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicated: "1",
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package will be installed on each.
await myCloudPool.CommitAsync();
```

>[AZURE.IMPORTANT] Označava ne uspijete implementacije sustava aplikacije paketa iz bilo kojeg razloga, servis za obradu u čvor [neupotrebljiv][net_nodestate], a nema zadataka koji će se planirati za izvršenje na tom čvor. U ovom slučaju, trebali biste **ponovno pokrenite** čvor da biste li ponovo inicijalizirati preseljenje implementaciju paketa. Ponovno pokretanje čvor će omogućiti raspoređivanje zadatka ponovno na čvor.

### <a name="install-task-application-packages"></a>Instaliranje paketa aplikacije zadatka

Slično kao u grupu, navedite aplikacije paketa *reference* za zadatak. Ako je zadatak zakazan pokrenuti čvor, paket je preuzeti i izdvojene neposredno prije izvođenja naredbeni redak zadatka. Ako navedeni paketa i verzija već instaliran na čvor, paket se preuzima i koristi postojećeg paketa.

Da biste instalirali paket aplikacije zadatka, konfiguriranje zadatka [CloudTask][net_cloudtask]. [ApplicationPackageReferences] [net_cloudtask_pkgref] svojstvo:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>Izvršavanje instalirane aplikacije

Paketi koje ste naveli za skup ili zadatak preuzimanja i izdvojene imenovani imeniku unutar na `AZ_BATCH_ROOT_DIR` čvora. Obradu stvara i varijabla okruženja koja sadrži put do imenovani direktorija. Vaš reci naredba zadataka pomoću ove varijable okruženja kada se pozivate na aplikaciju na čvor. Varijabla je u sljedećem obliku:

`AZ_BATCH_APP_PACKAGE_APPLICATIONID#version`

`APPLICATIONID`i `version` su vrijednosti koje odgovaraju verziji aplikacije i paket ste naveli za implementaciju. Na primjer, ako ste naveden tu verziju 2.7 aplikacije *stapanje* mora biti instaliran, vaše reci naredba zadatka koristila ove varijable okruženja za pristup njezine datoteke:

`AZ_BATCH_APP_PACKAGE_BLENDER#2.7`

Ako navedete zadanu verziju programa, možete izostaviti sufiks verziju. Na primjer, ako postavite "2.7" kao zadanu verziju za aplikaciju *stapanje*, zadataka možete referencirati sljedeće varijablu okruženja i oni će izvršiti verzija 2.7:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Sljedeći isječak koda prikazuje se primjer zadatka naredbenog retka koja se pokreće na zadanu verziju aplikacije za *stapanje* :

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [AZURE.TIP] Potražite u članku [postavki okruženja za zadatke](batch-api-basics.md#environment-settings-for-tasks) u [Pregled značajki grupe za](batch-api-basics.md) dodatne informacije o postavki okruženja čvor računalnim.

## <a name="update-a-pools-application-packages"></a>Ažuriranje paketa aplikacije u grupu

Ako postojeće grupe aplikacija već je konfiguriran pomoću Aplikacijski paket, možete odrediti novi paket za na resurse. Ako navedete novu referencu paket za skup Primijeni sljedeće:

* Sve nove čvorove koji uključivanje na resurse i postojeći čvor sustava ili reimaged instalirat će upravo određeni paket.
* Izračun čvorove koji se već nalaze u nakon što ažurirate reference paket automatski instalirati paket aplikacije. Te se računati čvorove mora biti sustava ili reimaged prima novog paketa.
* Kada je novi paket implementiran, varijable stvoreni okruženja odražavaju nove reference paketa aplikacije.

U ovom primjeru postojeće grupe aplikacija instalirana verzija 2.7 aplikacije *stapanje* konfiguriran u nekom od [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref]. Da biste ažurirali na resurse čvorove verzijom 2.76b, navedite novi [ApplicationPackageReference] [ net_pkgref] pomoću nove verzije i potvrđivanje promjena.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Sad kad je nova verzija konfiguriran, *Novi* čvor koji spaja na resurse dodijelit će verzija 2.76b implementiran na njega. Da biste instalirali 2.76b na čvorove koji su *već* u, ponovno pokretanje ili ih reimage. Imajte na umu sustava čvorove će zadržati datoteke iz prethodnog implementacijama paketa.

## <a name="list-the-applications-in-a-batch-account"></a>Popis aplikacija na računu za obradu

Popis aplikacija i njihovih paketa u račun za obradu pomoću [ApplicationOperations][net_appops]. [ListApplicationSummaries] [net_appops_listappsummaries] način.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Prelamanje prema gore

S pakete aplikacija može pridonijeti klijentima odaberite aplikacije za svoje zadatke i navedite točne verziju će se koristiti tijekom obrade poslove s omogućenim za obradu usluge. Može omogućiti i mogućnost za klijente da biste prenijeli i pratiti vlastite aplikacije na servisu.

## <a name="next-steps"></a>Daljnji koraci

* [Obradu REST API -JA] [ api_rest] nudi podršku za rad s paketa aplikacije. Ako, na primjer, potražite u članku [applicationPackageReferences] [ rest_add_pool_with_packages] element u članku [Dodavanje grupe aplikacija s poslovnim subjektom] [ rest_add_pool] o tome da biste odredili paketa da biste instalirali pomoću REST API-JA. U odjeljku [aplikacije] [ rest_applications] detalje o načinu da biste dobili informacije o aplikaciji pomoću obrade REST API-JA.

* Naučite programatski [Upravljanje grupe za Azure računi i kvote za obradu upravljanje .NET](batch-management-dotnet.md). [Grupe za upravljanje .NET] [ api_net_mgmt] biblioteke možete omogućiti značajke stvaranje i brisanje računa za obradu aplikaciju ili servis.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Program paketa više razine dijagrama"
[2]: ./media/batch-application-packages/app_pkg_02.png "Pločica aplikacije Azure portalu"
[3]: ./media/batch-application-packages/app_pkg_03.png "Aplikacija plohu Azure portalu"
[4]: ./media/batch-application-packages/app_pkg_04.png "Aplikacija plohu Detalji o portalu za Azure"
[5]: ./media/batch-application-packages/app_pkg_05.png "Nova aplikacija plohu Azure portalu"
[7]: ./media/batch-application-packages/app_pkg_07.png "Ažuriranje i brisanje paketa padajućeg Azure portalu"
[8]: ./media/batch-application-packages/app_pkg_08.png "Novi plohu paket aplikacije na portalu za Azure"
[9]: ./media/batch-application-packages/app_pkg_09.png "Nema povezane upozorenja za pohranu računa"
[10]: ./media/batch-application-packages/app_pkg_10.png "Odaberite plohu račun za pohranu na portalu za Azure"
[11]: ./media/batch-application-packages/app_pkg_11.png "Ažuriranje plohu paket na portalu za Azure"
[12]: ./media/batch-application-packages/app_pkg_12.png "Brisanje paket Potvrdi dijaloški Azure portalu"
