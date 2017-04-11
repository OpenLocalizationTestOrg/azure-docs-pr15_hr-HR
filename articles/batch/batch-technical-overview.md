<properties
    pageTitle="Osnove servisa za Azure obradu | Microsoft Azure"
    description="Dodatne informacije o korištenju servisa Azure grupe za veliki paralelno i radnih opterećenja HPC"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/22/2016"
    ms.author="marsma"/>

# <a name="basics-of-azure-batch"></a>Osnove grupe za Azure

Azure grupe omogućuje učinkovito pokretanje aplikacije veliki paralelno i visokih performansi računalstvo (HPC) u oblaku. Je servis platformu koja Zakazuje računalnim ćete morati usko rada pokrenuti upravljani skup virtualnim strojevima i možete automatski skaliranje izračunati resursi potrebama svoje zadatke.

Sa servisom za obradu definirati Azure računalnim resursi za izvršavanje aplikacija paralelno i na razini. Možete pokrenuti na zahtjev ili Zakazani zadaci, a ne morate ručno stvaranje, konfiguriranje i programa klasteru HPC, pojedinačne virtualnim strojevima, virtualne mreže ili složeni zadatak i upravljanje zadatka infrastruktura za planiranje rasporeda.

## <a name="use-cases-for-batch"></a>Korištenje slučajeva za obradu

Obrada je servisa upravljanih Azure koji se koristi za *Skupna obrada* ili *grupe za računalstvo*– pokretanje velik broj slične zadatke da biste dobili neke željeni rezultat. Grupe za računalstvo se najčešće koristi tvrtkama ili ustanovama koje redovito postupka, pretvaranje i analizirati velike količine podataka.

Obradu dobro funkcionira s intrinsically paralelno aplikacije (poznat i kao "embarrassingly paralelno") i radnih opterećenja. Intrinsically paralelno radnih opterećenja jednostavno podijelite više zadataka koje istodobno obavljaju rad na više računala.

![Paralelni zadataka][1]<br/>

Primjeri radnih opterećenja koji se često obrađuju korištenja ove tehnike su:

* Modeliranje financijskog rizika
* Klimatske i hydrology analiza podataka
* Prikazivanje slika, analizu i obrada
* Kodiranje medijskih sadržaja i prekodiranje
* Analiza genetski niza
* Inženjering opterećenjem analizu
* Testiranje softver

Obradu paralelne izračunavati korakom smanjivanje na kraju, i izvesti složeniju HPC radnih opterećenja kao što je aplikacija [Sučelja prosljeđivanje poruke (MPI)](batch-mpi.md) .

Usporedba između grupe i druge mogućnosti HPC rješenja Azure, potražite u članku [grupe i HPC rješenja](batch-hpc-solutions.md).

## <a name="developing-with-batch"></a>Razvoj pomoću obrade

Obradu paralelne radnih opterećenja pomoću obrade obično obavlja programski pomoću neke od [API -ji grupe](#batch-development-apis). S API-ji obradu stvarate i upravljanje grupe računalnim čvorove (virtualnim strojevima) i zakazivanje zadataka i zadataka da biste pokrenuli na te čvorove. Klijentski program ili servis koji vi ste autor koristi obradu API-ji za komunikaciju sa servisom grupe.

Učinkovito možete obraditi veliki radnih opterećenja za tvrtku ili ustanovu ili pružanje sučelje servisa klijentima tako da ih mogu se izvoditi zadataka i zadataka – na zahtjev ili rasporedu – na jedan, stotine ili čak tisuće čvorove. Kao dio veći tijeka rada, upravlja alate kao što su [Tvorničke Azure podataka](../data-factory/data-factory-data-processing-using-batch.md)možete koristiti i grupe.

> [AZURE.TIP] Kada budete spremni za obradu API detaljnije razumijevanje značajki pruža istražujte, potražite u članku [Pregled značajki grupe za razvojne inženjere](batch-api-basics.md).

### <a name="azure-accounts-youll-need"></a>Azure račune, potreban vam je

Ako razvijate rješenja za obradu, koristit ćete sljedećih računa u Microsoft Azure.

- **Račun za azure i pretplatom** – Ako još nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika prednosti][msdn_benefits], ili prijave za [besplatan račun za Azure][free_account]. Kada stvorite račun, pretplatu zadani je stvoriti.

- **Račun za obradu** – kada aplikacija interakciju sa servisom obradu naziv računa, URL račun i tipkovni prečac se koriste kao vjerodajnice. Sve grupe za resurse kao što su grupe, izračunati čvorove, zadatke, i zadaci trebaju povezanih s računom grupe. Možete [stvoriti račun grupe](batch-account-create-portal.md) na portalu za Azure.

- **Račun za pohranu** – obradu uključuje ugrađenu podršku za rad s datotekama u [Spremište Azure][azure_storage]. Skoro sve grupe scenarij koristi pohranu Azure – pripremna programe koji se izvode zadatke i podatke koje obrade te za pohranu izlazne podatke koji stvaraju. Stvaranje računa za pohranu potražite u članku [o Azure prostora za pohranu računa](./../storage/storage-create-storage-account.md).

### <a name="batch-development-apis"></a>Grupe za razvoj API-ji

Aplikacija i servisa može problema Izravni pozive REST API-JA, koristite jednu ili više sljedeće biblioteke klijenta ili kombinacijom tih dvaju načina upravljanja izračunati resurse i izvođenja paralelno radnih opterećenja na razini putem servisa za grupu.

| API-JA    | Referenca API-JA | Preuzimanje | Primjere koda |
| ----------------- | ------------- | -------- | ------------ |
| **Obradu ostalih** | [MSDN][batch_rest] | N/D | [MSDN][batch_rest] |
| **Grupe za .NET**    | [MSDN][api_net] | [NuGet][api_net_nuget] | [GitHub][api_sample_net] |
| **Obradu Python**  | [readthedocs.IO][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Obrada Node.js** | [github.IO][api_nodejs] | [npm][api_nodejs_npm] | - |
| **Obradu Java** (pretpregled) | [github.IO][api_java] | [Maven][api_java_jar] | [GitHub][api_sample_java] |

### <a name="batch-resource-management"></a>Upravljanje resursima za obradu

Uz Klijentski API-ji na sljedeći način možete koristiti i za upravljanje resursima unutar grupe za račun.

- [Obrada cmdleta ljuske PowerShell][batch_ps]: cmdleti za grupe u Azure u modul [Azure PowerShell](../powershell-install-configure.md) omogućuju upravljanje resursima za obradu sa servisom PowerShell.

- [Azure EŽA](../xplat-cli-install.md): U sučelje Azure naredbenog retka (Azure EŽA) je skup alata za različite platforme, koja omogućuje naredbe ljuske za interakciju s mnogo Azure servisa, uključujući grupe.

- [Grupe za upravljanje .NET](batch-management-dotnet.md) klijentska biblioteka: dostupna i putem [NuGet][api_net_mgmt_nuget], klijentska biblioteka .NET upravljanje grupe možete koristiti za programski upravljanje računi grupe, kvota i paketa aplikacije. Referenca za biblioteke za upravljanje je na [MSDN-][api_net_mgmt].

### <a name="batch-tools"></a>Alati za obradu

Dok nije potrebno da biste sastavili rješenja koja koriste grupe, Slijede neki koristan Alati za korištenje prilikom Izrada i ispravljanje pogrešaka grupe aplikacija i usluga.

 - [Portal za Azure][portal]: Stvaranje, praćenje i brisanje grupe za grupe, zadataka i zadataka u blades grupe portalom Azure. Možete pogledati status informacije o tim i druge resurse dok pokrenuti poslova i čak i preuzimanje datoteka s računalnim čvorove u svoje grupe (Preuzimanje nije uspjelo zadatka `stderr.txt` prilikom rješavanja problema, na primjer). Možete preuzeti i udaljene radne površine (RDP) datoteke koje možete koristiti da biste se prijavili izračunati čvorove.

 - [Azure Explorer obradu][batch_explorer]: obradu Explorer nudi slične grupe resursa upravljanje funkcije kao portala za Azure, ali u samostalne klijentska aplikacija Windows Presentation Foundation (WPF). Jedna od aplikacija uzorka za obradu .NET na [GitHub][github_samples], možete izraditi s Visual Studio 2015 ili noviji i koristiti ga pregledavanje i upravljanje resursima na vašem računu obradu dok razvoj i na obradu rješenja za ispravljanje pogrešaka. Prikaz posla, skup i Detalji o zadatku preuzimati datoteke iz računalnim čvorove i povezati čvorove daljinski pomoću udaljene radne površine (RDP) datoteke možete preuzeti pomoću programa Explorer grupe.

 - [Microsoft Azure prostora za pohranu Explorer][storage_explorer]: dok ne isključivo za grupe za Azure alata za pohranu Explorer je drugi koristan alat da bi prilikom razvoja i ispravljanje pogrešaka na obradu rješenja.

## <a name="scenario-scale-out-a-parallel-workload"></a>Scenarij: Skaliranje out paralelno radno opterećenje

Uobičajena rješenja koja ih koriste obrade API-ji za interakciju sa servisom za obradu uključuje skaliranje izvan intrinsically paralelno rada – kao što su vizualizacije slike za 3D scene – na skup računalnim čvorove. Ovaj skup računalnim čvorove može biti "renderiranje farmi" koji sadrži tens, stotine ili čak tisuće jezgri za vaš posao vizualizacije, na primjer.

Na sljedećem su dijagramu prikazani uobičajeni obradu tijeka rada, s klijentski program ili servis pomoću grupe da biste pokrenuli paralelno radno opterećenje.

![Tijek rada za obradu rješenja][2]

U ovom scenariju uobičajenih aplikaciju ili servis obrađuje računalne radno opterećenje u seriji Azure pomoću sljedećih koraka:

1. Prenesite **ulaznih datoteka** i **aplikacije** obradit će te datoteke na račun servisa Azure prostora za pohranu. Unos datoteke mogu biti sve podatke koje obradit će se aplikacija, kao što su financijske modele podataka ili videodatoteke se prebacuju. Datoteka aplikacije može biti bilo koju aplikaciju koja se koristi za obradu podataka, kao što je 3D renderiranje aplikacije ili transcoder medijske sadržaje.

2. Stvaranje grupe za **skup** od računalnim čvorove na vašem računu grupe – su te čvorovi virtualnim strojevima koji će izvršavanje zadataka. Navedite svojstva kao što su [Veličina čvor](./../cloud-services/cloud-services-sizes-specs.md), njihove operacijski sustav i mjesto u odjeljku pohrana Azure aplikacije da biste instalirali kada čvorove uključite skup (aplikacija za koji ste prenijeli u koraku #1). Možete konfigurirati grupu da biste [automatski promijeniti omjer](batch-automatic-scaling.md)– dinamički podesiti broj računalnim čvorove u – u odgovoru radno opterećenje koje generirati zadataka.

3. Stvaranje obrade **posao** pokrenuti povećavaju skup računalnim čvorove. Kada stvorite zadatak, povezati ga sa grupe za skup.

4. Dodavanje **zadataka** na posao. Kada dodate zadatke na poslu, servis za obradu automatski rasporede zadatke za izvršavanje na čvorove računalnim u. Svaki zadatak koji koristi aplikacije koji ste prenijeli na obradu ulaznih datoteka.

    - 4a. Prije nego što se izvršava zadatak, možete preuzeti podatke (ulaznih datoteka) koje je u tijeku čvor računalnim što vam je dodijeljen. Ako aplikacija nije već instaliran na čvor (pročitajte članak korak #2), možete ga preuzeti ovdje umjesto toga. Nakon dovršetka preuzimanja Zadaci će se izvoditi na njihove dodijeljene čvorove.

5. Kao zadaci pokrenuli, upit možete poslati grupe za praćenje napretka posla i njegov zadatke. Klijentski program ili servis komunicira sa servisom za obradu putem HTTP, a jer je možda nadzor tisuće zadataka koji se izvode na tisuće računalnim čvorove, provjerite jeste li [servis za obradu upita učinkovito](batch-efficient-list-queries.md).

6. Kao dovršenih zadataka ih možete prenijeti svoje podatke za rezultat Azure prostora za pohranu. Datoteke možete dohvatiti i izravno s računalnim čvorove.

7. Kada vaš nadzor otkrije dovršite zadatke u svoj posao, klijentski program ili servis možete preuzeti za izlazne podatke za daljnje obrade i procjenu.

Imajte na umu to je samo jedan pokrenuta koristiti grupe i scenarij opisuje samo nekoliko njegove dostupne značajke. Na primjer, možete izvršiti [više zadataka paralelno](batch-parallel-node-tasks.md) na svakom računalnim čvor i omogućuju [zadataka Priprema i dovršetka posla](batch-job-prep-release.md) Priprema čvorove za svoje zadatke, a zatim Očisti kasnije.

## <a name="next-steps"></a>Daljnji koraci

Sad kad imate više razine pregled obrade servisa, vrijeme je da biste pristupili da biste saznali kako je možete koristiti za obradu paralelne radnih opterećenja sustava računalnim ćete morati usko.

- Pročitajte [Pregled značajki grupe za razvojne inženjere](batch-api-basics.md), ključne informacije za sve Priprema za korištenje grupe. Članak sadrži detaljne informacije o resursima servisa grupe kao što su grupe, čvorove, zadatke, i zadatke i mnoge značajke API koje možete koristiti tijekom sastavljanja grupe aplikacija.

- [Početak rada s bibliotekom Azure grupe za .NET](batch-dotnet-get-started.md) da biste saznali kako koristiti C# i grupe za .NET biblioteke za izvršavanje jednostavne radno opterećenje pomoću tijeka rada za uobičajene grupe. U ovom se članku mora biti jedan od vašeg prvi tabulatora dok učenjem kako koristiti grupe za servis. Također je [verzija Python](batch-python-tutorial.md) vodič.

- Preuzimanje [primjere koda na GitHub] [ github_samples] da biste vidjeli kako i C# i Python možete sučelja pomoću obrade za raspored i proces radnih opterećenja uzorka.

- Pogledajte [Tečaj za obradu] [ learning_path] da biste pristupili uvid dostupnih resursa koje dok učite da biste radili s grupe.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
