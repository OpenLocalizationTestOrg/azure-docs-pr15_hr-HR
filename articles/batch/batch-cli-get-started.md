<properties
   pageTitle="Početak rada s Azure obradu EŽA | Microsoft Azure"
   description="Dobivanje kratkog uvoda naredbe grupe u Azure EŽA za upravljanje resursima servisa Azure grupe"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/30/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-cli"></a>Početak rada s EŽA grupe za Azure

Različite platforme Azure naredbenog retka sučelja (Azure EŽA) omogućuje upravljanje računima grupe i resursima kao što su grupe, zadatke i zadatke u naredbe ljuske Linux, Mac i Windows. EŽA Azure možete izvesti i skriptiraj mnoge zadatke izvršavanje s API-ji grupe, Azure portal i cmdleta ljuske PowerShell za obradu.

U ovom se članku temelji se na Azure EŽA verzija 0.10.5.

## <a name="prerequisites"></a>Preduvjeti

* [Instalacija Azure EŽA](../xplat-cli-install.md)

* [Povezivanje EŽA Azure u pretplatu za Azure](../xplat-cli-connect.md)

* Prebacite se u **način rada Voditelj resursa**:`azure config mode arm`

>[AZURE.TIP] Preporučujemo da ažurirate instalaciju sustava Azure EŽA često da biste iskoristili servisnih ažuriranja i poboljšanja.

## <a name="command-help"></a>Pomoć za naredbe

Prikaz teksta pomoći za svaku naredbu u EŽA Azure dodavanjem `-h` kao mogućnost za samo nakon naredbe. Ako, na primjer:

* Pomoć za na `azure` naredbu, unesite:`azure -h`
* Da biste dobili popis svih naredbi za obradu u na EŽA, koristite:`azure batch -h`
* Da biste dobili pomoć za stvaranje računa grupe, unesite:`azure batch account create -h`

Kada se nalazite u niste sigurni, koristite na `-h` mogućnost naredbenog retka za pomoć u bilo kojoj naredbi Azure EŽA.

## <a name="create-a-batch-account"></a>Stvaranje grupe za račun

Korištenje:

    azure batch account create [options] <name>

Primjer:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Stvara novi račun za obradu s navedenim parametrima. Morate navesti najmanje mjesto, grupa resursa i naziv računa. Ako još nemate grupu resursa, stvorite ga tako da pokrenete `azure group create`, i navedite jednu Azure područja (kao što su "Zapad NAM") za na `--location` mogućnost. Ako, na primjer:

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] Naziv računa grupe mora biti jedinstvena unutar područja za Azure stvara se na račun. Može sadržavati samo malo alfanumeričke znakove i mora biti 3 24 znakova. Ne možete koristiti posebne znakove kao što su `-` ili `_` u nazive računa grupe.

### <a name="linked-storage-account-autostorage"></a>Povezane prostora za pohranu računa (autostorage)

(Nije obavezno) možete povezati s računom za pohranu **opće namjene** na račun za obradu kad ga stvorite. Značajka [paketa aplikacije](batch-application-packages.md) serije koristi spremište blobova platforme u povezane opće namjene račun za pohranu, kao i [Obradu datoteka konvencije .NET](batch-task-output.md) biblioteke. Ove značajke će vam pomoći u implementacija aplikacije pokretanje grupe za zadatke i persisting podataka mogu proizvesti.

Da biste se povezali postojećeg računa za Azure prostora za pohranu na novi račun za obradu kad ga stvorite, navedite u `--autostorage-account-id` mogućnost. Ova mogućnost zahtijeva ID potpuno kvalificiran resursa za pohranu računa.

Najprije prikaz pojedinosti o računu za pohranu:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Pomoću **URL-a** vrijednost u `--autostorage-account-id` mogućnost. Vrijednost Url započinje rečenicom "/ pretplate /" i sadrži svoje pretplate ID i resursa put na račun za pohranu:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>Brisanje grupe za račun

Korištenje:

    azure batch account delete [options] <name>

Primjer:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Briše navedeni grupe za račun. Kada se to od vas zatraži, potvrdili da biste uklonili račun (uklanjanje računa može potrajati neko vrijeme da biste dovršili).

## <a name="manage-account-access-keys"></a>Upravljanje računom pristupnih tipki

Tipkovni prečac za [Stvaranje i mijenjanje resursi](#create-and-modify-batch-resources) morate na vašem računu grupe.

### <a name="list-access-keys"></a>Popis pristupnih tipki

Korištenje:

    azure batch account keys list [options] <name>

Primjer:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Popis tipki račun za određene grupe za račun.

### <a name="generate-a-new-access-key"></a>Stvaranje novog ključa za access

Korištenje:

    azure batch account keys renew [options] --<primary|secondary> <name>

Primjer:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Obnavlja tipku navedeni račun za određene grupe za račun.

## <a name="create-and-modify-batch-resources"></a>Stvaranje i izmjena grupe za resurse

Azure EŽA omogućuje stvaranje, čitanje, ažuriranje, i brisanje (CRUD) obradu resurse kao što su grupe, izračunati čvorove, zadatke i zadatke. Te operacije CRUD potreban je vaš naziv računa grupe, tipkovni prečac, a krajnje točke. Možete odrediti na njih u `-a`, `-k`, i `-u` mogućnosti ili postavljanje [varijable okruženja](#credential-environment-variables) koje koristi u EŽA automatski (Ako je popunjen).

### <a name="credential-environment-variables"></a>Varijable okruženja vjerodajnica

Možete postaviti `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY`, a `AZURE_BATCH_ENDPOINT` varijable okruženja umjesto navodeći `-a`, `-k`, i `-u` mogućnosti u naredbenom retku za svaku naredbu izvršavanja. Obradu EŽA koristi ove varijable (Ako postavite) tako da možete izostaviti na `-a`, `-k`, i `-u` mogućnosti. Ostatak u ovom se članku pretpostavlja da koristite ove varijable okruženja.

>[AZURE.TIP] Popis ključeva s `azure batch account keys list`, te prikazati krajnje točke na račun s `azure batch account show`.

### <a name="json-files"></a>JSON datoteke

Kada stvorite grupe za resurse kao što su grupe i zadatke, možete odrediti JSON datoteku koja sadrži novi resurs konfiguracije umjesto Prosljeđivanje parametara mogućnosti kao naredbenog retka. Ako, na primjer:

`azure batch pool create my_batch_pool.json`

Dok je na raspolaganju vam mnogo resursa stvaranje operacije pomoću samo mogućnostima naredbenog retka, neke značajke potreban je JSON oblikovani datoteka koja sadrži detalje o resursu. Ako, na primjer, morate koristiti JSON datoteke ako želite navesti datoteke resursa za početak zadatak.

Da biste pronašli JSON potrebne za stvaranje resursa, pogledajte [Pregled obrade REST API -JA] [ rest_api] dokumentaciju na MSDN-u. Svaka tema "Dodavanje *Vrsta resursa"*sadrži primjer JSON za stvaranje resursa koje možete koristiti kao predlošci JSON datoteke. Na primjer, JSON radi stvaranja grupe aplikacija pronaći ćete u članku [Dodavanje grupe aplikacija s poslovnim subjektom][rest_add_pool].

>[AZURE.NOTE] Ako navedete JSON datoteke prilikom stvaranja resursa, zanemaruju se sve parametre koje ste odredili u naredbenom retku za taj resurs.

## <a name="create-a-pool"></a>Stvaranje zajedničko područje

Korištenje:

    azure batch pool create [options] [json-file]

Primjer (Konfiguriranje virtualnog računala):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Primjer (oblaka Services konfiguracije):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Stvara skup računalnim čvorove u servisu grupe.

Kao što je rečeno [Pregled značajki za obradu](batch-api-basics.md#pool), imate dvije mogućnosti kad odaberete operacijski sustav za čvorove u vašem: **Konfiguriranje virtualnog računala** i **Konfiguracija servisa oblaka**. Korištenje na `--image-*` mogućnost stvaranja grupe konfiguracija virtualnog računala i `--os-family` da biste stvorili konfiguraciju servisa oblaka grupe. Ne možete navesti oba `--os-family` i `--image-*` mogućnosti.

Možete navesti skup [paketa aplikacije](batch-application-packages.md) i naredbeni redak za [pokretanje zadatka](batch-api-basics.md#start-task). Da biste odredili datoteke resursa za pokretanje zadatka, međutim, umjesto toga koristite [JSON datoteke](#json-files).

Brisanje grupe aplikacija sa:

    azure batch pool delete [pool-id]

>[AZURE.TIP] Provjerite [popis slika virtualnog računala](batch-linux-nodes.md#list-of-virtual-machine-images) za odgovarajuće vrijednosti za na `--image-*` mogućnosti.

## <a name="create-a-job"></a>Stvaranje zadatka

Korištenje:

    azure batch job create [options] [json-file]

Primjer:

    azure batch job create --id "job001" --pool-id "pool001"

Dodaje posao račun grupe i određuje skup njegov zadatke izvršiti.

Brisanje posla uz:

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>Popis grupe, zadatke, zadatke i ostale resurse

Podržava za svaku vrstu grupe resursa u `list` naredbu koju upiti računa grupe i popisi resurse te vrste. Na primjer, navedete u grupe u svoj račun i zadatke u posao:

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>Učinkovito popis resursa

Za brže upita možete navesti **Odaberite**, **Filtriranje**i **Proširivanje** uvjet mogućnosti za `list` operacije. Pomoću ove mogućnosti za ograničavanje vraćenih servis za obradu podataka. Budući da se sva filtriranja pojavljuje poslužiteljsko, samo oni podaci koje vas zanimaju presjek na žičani. Pomoću ove uvjete za spremanje propusnosti (i zato vremena) prilikom obavljanja operacije popisa.

Na primjer, to će vratiti samo grupe čiji ID-a počinju s "renderTask":

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

Obradu EŽA podržava sve tri uvjeta: podržava grupe servisa:

* `--select-clause [select-clause]`Vraćanje podskup svojstva za svaki entitet
* `--filter-clause [filter-clause]`Vraćanje samo entiteti koje odgovaraju navedeni izraz OData
* `--expand-clause [expand-clause]`Nabavite entitet informacije u jednom podlozi OSTALE poziv. Proširivanje uvjet podržava samo na `stats` svojstvo trenutno.

Detalje o tri uvjeta: i predstavljanja upite s njima, potražite u članku [učinkovito upita servisa Azure grupe](batch-efficient-list-queries.md).

## <a name="application-package-management"></a>Upravljanje aplikacijama paketa

Pakete omogućuje pojednostavljeni implementacija aplikacije da biste čvorove računalnim u svoje grupe. S EŽA Azure možete prenijeti paketa aplikacije, upravljanje verzijama paketa i brisanje paketa.

Da biste stvorili novu aplikaciju i dodavanje verzije paketa:

**Stvaranje** aplikacije:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**Dodavanje** paketa aplikacije:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**Aktiviranje** značajke pakiranja:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

Postavili **zadanu verziju** aplikacije:

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>Implementacija Aplikacijski paket

Jedan ili više paketa aplikacije za implementaciju možete odrediti kada stvorite novu grupu. Kada odredite paket vrijeme stvaranja grupe aplikacija, kao skup spojeva čvor je implementirana svaki čvor. Paketi i uvode se kada se čvor sustava ili reimaged.

Odredite na `--app-package-ref` mogućnosti prilikom stvaranja grupe aplikacija za implementaciju paketa aplikacije za čvorove na resurse kao što su uključiti u grupu. Na `--app-package-ref` mogućnost prihvaća popis ID-ove za implementaciju čvorove računalnim zarezom sa zarezom.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

Kada stvorite zajedničko područje pomoću mogućnosti naredbenog retka, trenutno ne možete navesti *koju* verziju paketa aplikacije za implementaciju čvorove računalnim, primjerice "1.10-beta3". Dakle, najprije morate odrediti zadanu verziju aplikacije s `azure batch application set [options] --default-version <version-id>` prije stvaranja na resurse (pogledajte prethodnom sekcijom). Međutim, možete odrediti verziju paketa za na resurse ako koristite [JSON datoteke](#json-files) umjesto mogućnosti naredbenog retka kada stvarate na resurse.

Dodatne informacije o paketa aplikacije možete pronaći u [Implementacija aplikacije s paketa aplikacije Azure grupe](batch-application-packages.md).

>[AZURE.IMPORTANT] Na račun za obradu da biste koristili aplikaciju paketa morate [vezu račun za Azure prostora za pohranu](#linked-storage-account-autostorage) .

### <a name="update-a-pools-application-packages"></a>Ažuriranje paketa aplikacije u grupu

Da biste ažurirali aplikacije dodijeljena postojeću grupu, problema s `azure batch pool set` naredbe s na `--app-package-ref` mogućnost:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Da biste implementirali novog paketa aplikacije za izračunavanje čvorove već u postojeću grupu, ponovno pokrenite ili reimage te čvorove:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

>[AZURE.TIP] Možete dobiti popis čvorove u grupu, zajedno s identifikacijskim čvor s `azure batch node list`.

Imajte na umu da morate već ste konfigurirali aplikacije uz zadanu verziju prije implementacije (`azure batch application set [options] --default-version <version-id>`).

## <a name="troubleshooting-tips"></a>Savjeti za otklanjanje poteškoća

U ovom se odjeljku namijenjen da vam pruži resursa za korištenje pri otklanjanju poteškoća Azure EŽA. Ne moraju nužno biti riješiti sve probleme, ali je može vam pomoći u suzili uzrok i pokažite pomoći resursi.

* Korištenje `-h` da biste dobili **tekst pomoći** za bilo koju naredbu EŽA

* Korištenje `-v` i `-vv` da biste prikazali **opširno** izlaz naredba; `-vv` je "vrlo" opširno i prikazuje stvarni REST zahtjeve i odgovore. Te parametre su za prikaz cijelog pogreške izlaz.

* Možete vidjeti **naredbe izlaz kao JSON** pomoću na `--json` mogućnost. Na primjer, `azure batch pool show "pool001" --json` prikazuje svojstva pool001 korisnika u JSON OSNOVNI oblik. Možete i zatim kopiranje i izmjena ovaj izlaz za korištenje u na `--json-file` (pogledajte [JSON datoteke](#json-files) ranije u ovom članku).

* [Forum za obradu na MSDN-] [ batch_forum] je resurs sjajno pomoći i usko nadzire članovi grupe. Provjerite postoji objavljivati pitanja Ako naiđete na probleme ili želite pomoći s određenom operacijom.

* Azure EŽA trenutno podržava se svi skupne operacije resursa. Ako, na primjer, trenutno ne možete navesti programa paketa aplikacije *verzija* za skup samo na paket ID-a. U tim slučajevima morat ćete navesti na `--json-file` za naredbu umjesto korištenja mogućnosti naredbenog retka. Ne zaboravite Budite u tijeku s najnovijom verzijom EŽA mogao preuzeti buduće poboljšanja.

## <a name="next-steps"></a>Daljnji koraci

*  Potražite u članku [Implementacija aplikacije s paketa aplikacije Azure grupe](batch-application-packages.md) da biste saznali kako pomoću ove značajke možete upravljati i implementacija aplikacije izvršiti na obradu računalnim čvorove.

* Pročitajte [učinkovito upita servisa grupe](batch-efficient-list-queries.md) za više o smanjite broj stavki i vrstu podataka koje se vraćaju za upite grupe.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx