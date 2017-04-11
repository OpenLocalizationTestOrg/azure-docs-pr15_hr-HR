<properties
    pageTitle="Praktični vodič – početak rada s klijentskog programa Azure obradu Python | Microsoft Azure"
    description="Naučite osnovni koncepti obradu Azure i razvoju obradu servis pomoću jednostavni scenarij"
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/27/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-python-client"></a>Početak rada s klijentom Python obradu Azure

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Informirajte se o osnovama [Grupe za Azure] [ azure_batch] i [Obradu Python] [ py_azure_sdk] klijent kao ćemo razmotriti mala aplikacija za obradu pisane Python. Ne možemo pogledajte kako dva koristi ogledne skripte grupe servisa za obradu paralelne radno opterećenje na virtualnim računalima sustava Linux u oblak i način interakcije s [Azure prostora za pohranu](./../storage/storage-introduction.md) za pripremna datoteka i dohvaćanja. Ćete dodatne uobičajenih obradu aplikacije tijeka rada i dobiti osnovni razumijevanja glavne komponente obrade kao što su zadacima, zadatke, grupe i izračunati čvorove.

![Obradu rješenja tijeka rada (basic)][11]<br/>

## <a name="prerequisites"></a>Preduvjeti

U ovom se članku pretpostavlja da imate rad znanja Python i poznavanje programa Linux. Također pretpostavlja da ste moći zadovoljava preduvjete za stvaranje računa koje su navedene u nastavku za Azure i u grupe i servisi za pohranu.

### <a name="accounts"></a>Poslovni subjekti

- **Račun za Azure**: Ako još nemate Azure pretplatu, [stvorite račun za Azure besplatne][azure_free_account].
- **Račun za obradu**: kada imate Azure pretplatu, [stvorite račun za Azure grupe](batch-account-create-portal.md).
- **Račun za pohranu**: potražite u članku [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account) u [računi o Azure prostora za pohranu](../storage/storage-create-storage-account.md).

### <a name="code-sample"></a>Uzorak koda

Python vodiča [uzorka kod] [ github_article_samples] jednu od mnogo primjere koda za obradu nalazi se u [azure, grupe i uzorke] [ github_samples] spremište na GitHub. Svi uzorci možete preuzeti klikom **Kloniraj ili preuzimanje > preuzimanje POŠTANSKI** na početnoj stranici spremište ili tako da kliknete [azure-obradu-uzorka-master.zip] [ github_samples_zip] vezu za preuzimanje izravno. Nakon što ste dobivenih sadržaj ZIP datoteke, dva skripte za ovog praktičnog vodiča nalaze se u na `article_samples` direktorija:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Python okruženje

Da biste pokrenuli ogledne skripte *python_tutorial_client.py* na vašem lokalnom radne stanice, morate **Python tumačenja** kompatibilan s verzije **2.7** ili **3.3 +**. Skripta testirano Linux i Windows.

### <a name="cryptography-dependencies"></a>zavisnosti šifriranja

Potrebno je instalirati ovisnosti [šifriranja] [ crypto] biblioteke, potrebnih u `azure-batch` i `azure-storage` Python paketa. Izvršite jednu od sljedećih operacija prikladna za svoju platformu ili se odnositi na [šifriranja instalacije] [ crypto_install] pojedinosti dodatne informacije:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`

* CentOS

    `yum update && yum install -y gcc openssl-dev libffi-devel python-devel`

* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`

* Windows

    `pip install cryptography`

>[AZURE.NOTE] Ako instalirate za Python 3,3 + na Linux, koristite na ekvivalentima python3 ovisnost Python. Na primjer, na Ubuntu:`apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`

### <a name="azure-packages"></a>Azure paketa

Nakon toga instalirati paketa **Grupe za Azure** i Python **Azure prostora za pohranu** . To možete učiniti pomoću **točaka** i *requirements.txt* ovdje:

`/azure-batch-samples/Python/Batch/requirements.txt`

Naredbe **točaka** za instalaciju paketa obradu i pohranu problem:

`pip install -r requirements.txt`

Ili, možete instalirati [grupe za azure] [ pypi_batch] i [azure pohranu] [ pypi_storage] Python paketi ručno:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [AZURE.TIP] Možda ćete morati prefiks se naredbe s `sudo` ako koristite račun unprivileged. Na primjer, `sudo pip install -r requirements.txt`. Dodatne informacije o instaliranju Python paketa potražite u članku [Instaliranje paketa] [ pypi_install] na readthedocs.io.

## <a name="batch-python-tutorial-code-sample"></a>Obradu Python kod vodiča uzorka

Kod vodiča uzorka Python grupe sastoji se od dva Python skripte i nekoliko podatkovne datoteke.

- **python_tutorial_client.PY**: stupi u interakciju sa servisima za obradu i pohranu izvršiti paralelno radno opterećenje na računalnim čvorove (virtualnim strojevima). Skripta *python_tutorial_client.py* se izvodi na vašem lokalnom radne stanice.

- **python_tutorial_task.PY**: skriptu koja se izvršava na izračunati čvorove u Azure za izvođenje stvarni posao. U uzorku, *python_tutorial_task.py* raščlanjuje tekst iz datoteke preuzete iz Azure prostora za pohranu (ulazni datoteka). Zatim daje tekstnu datoteku (Izlazna datoteka) koja sadrži popis gornje tri riječi koje se prikazuju u ulaznoj datoteci. Nakon što ga Stvori Izlazna datoteka, *python_tutorial_task.py* prenese datoteka Azure prostora za pohranu. To čini dostupnim za preuzimanje skriptu klijenta koji se izvode na vašem radne stanice. Skripta *python_tutorial_task.py* pokreće paralelno više računalnim čvorove u servisu grupe.

- **./data/taskdata\*.txt**: ove tri tekstne datoteke omogućuju unos za zadatke koji su pokrenuti na čvorove računalnim.

Sljedeći dijagram prikazuje primarni postupaka koji se izvode po skripte klijenta i zadatak. Uobičajeni mnogo računalnim rješenja koje su stvorene pomoću obrade je osnovni tijek rada. Dok ne sadrži sve značajke dostupne u servisu obradu, gotovo svaki obradu scenarij sadrži dijelove ovaj tijek rada.

![Grupe za tijek rada][8]<br/>

[**Korak 1.**](#step-1-create-storage-containers) Stvaranje **spremnika** u spremište blobova platforme Azure.<br/>
[**Korak 2.**](#step-2-upload-task-script-and-data-files) Prijenos datoteka skripte i unos zadatka spremnika.<br/>
[**Korak 3.**](#step-3-create-batch-pool) Stvaranje grupe za **skup**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Skup **StartTask** preuzima skripte zadatka (python_tutorial_task.py) da biste čvorove kako se uključiti u grupu.<br/>
[**Korak 4.**](#step-4-create-batch-job) Stvaranje grupe za **posao**.<br/>
[**Korak 5.**](#step-5-add-tasks-to-job) Dodavanje **zadataka** na posao.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Zadaci koji su zakazane izvršiti na čvorove.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Svaki zadatak preuzimanja ulazne podatke iz spremišta Azure, a zatim počinje izvršavanja.<br/>
[**Korak 6.**](#step-6-monitor-tasks) Praćenje zadataka.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Zadaci se dovršenim prijenosa svoje podatke za pohranu Azure.<br/>
[**Korak 7.**](#step-7-download-task-output) Preuzmite zadatka Izlaz iz prostora za pohranu.

Spomenuti, ne svake grupe rješenja izvodi te točno korake, a mogu sadržavati mnogo više, ali ovaj primjer pokazuje uobičajenih procesa koji se nalaze u rješenje grupe.

## <a name="prepare-client-script"></a>Priprema klijenta skripte

Prije nego što pokrenete uzorka, dodati *python_tutorial_client.py*obradu i pohranu vjerodajnice za račun. Ako vam još niste učinili, otvorite datoteku u uređivaču za omiljene i ažurirajte sljedeće retke vjerodajnice.

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
batch_account_name = "";
batch_account_key  = "";
batch_account_url  = "";

# Storage account credentials
storage_account_name = "";
storage_account_key  = "";
```

Vjerodajnice račun grupe i pohranu unutar plohu računa za svaki servis možete pronaći na [portal za Azure][azure_portal]:

![Obrada vjerodajnice na portalu][9]
![vjerodajnice za pohranu na portalu][10]<br/>

U sljedećim se odjeljcima ćemo analizu koraci koriste skripte za obradu radno opterećenje u servisu grupe. Pozivamo vas da potražite redovito skripte u uređivaču dok radite na način kroz sve ostale članka.

Idite na sljedeći redak u **python_tutorial_client.py** da biste započeli korak 1:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Korak 1: Stvaranje potpisu

![Stvaranje spremnika u prostor za pohranu za Azure][1]
<br/>

Obradu uključuje ugrađenu podršku za interakciju s Azure prostora za pohranu. Spremnika u vašem računu za pohranu dat će datoteke potrebne za zadatke koji se izvode na vašem računu grupe. Spremnike omogućuje i mjesto za pohranu za izlazne podatke koji daju zadaci. Najprije ne skripte *python_tutorial_client.py* je stvorite tri spremnika u [Spremište blobova platforme Azure](../storage/storage-introduction.md#blob-storage):

- **aplikacija**: ovaj spremnik će sadržavati skripte Python izvesti tako da se zadaci *python_tutorial_task.py*.
- **unos**: zadaci će preuzimati podatkovnih datoteka za obradu iz *ulaznog* spremnik.
- **Izlaz**: kada zadaci dovršiti obrada ulazne datoteke, će prijenosa rezultate spremniku *Izlaz* .

Da biste mogli raditi s računom za pohranu i stvaranje spremnika, koristimo [spremištem azure] [ pypi_storage] paketa da biste stvorili [BlockBlobService] [ py_blockblobservice] objekta – "blob klijenta." Ne možemo stvoriti tri spremnika u račun za pohranu pomoću klijentskog programa blob.

```python
 # Create the blob client, for use in obtaining references to
 # blob storage containers and uploading files to containers.
 blob_client = azureblob.BlockBlobService(
     account_name=_STORAGE_ACCOUNT_NAME,
     account_key=_STORAGE_ACCOUNT_KEY)

 # Use the blob client to create the containers in Azure Storage if they
 # don't yet exist.
 app_container_name = 'application'
 input_container_name = 'input'
 output_container_name = 'output'
 blob_client.create_container(app_container_name, fail_on_exist=False)
 blob_client.create_container(input_container_name, fail_on_exist=False)
 blob_client.create_container(output_container_name, fail_on_exist=False)
```

Kada stvorite spremnike aplikacije sada možete prenijeti datoteke koje će koristiti zadaci.

> [AZURE.TIP] [Kako koristiti Azure blobova iz Python](../storage/storage-python-how-to-use-blob-storage.md) omogućuje dobar pregled rad s Azure potpisu i blob-ova. Pri vrhu popisa čitanje mora biti dok radite s grupe.

## <a name="step-2-upload-task-script-and-data-files"></a>Korak 2: Prijenos zadatka skripte i podatkovne datoteke

![Prijenos aplikacije zadatka i unos (podaci) datoteke spremnika][2]
<br/>

Prijenos operacija datoteke *python_tutorial_client.py* najprije definira zbirke putova datoteka **aplikacije** i **unos** kao postoje na lokalnom računalu. Zatim ga prenosite te datoteke spremnika koji ste stvorili u prethodnom koraku.

```python
 # Paths to the task script. This script will be executed by the tasks that
 # run on the compute nodes.
 application_file_paths = [os.path.realpath('python_tutorial_task.py')]

 # The collection of data files that are to be processed by the tasks.
 input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                     os.path.realpath('./data/taskdata2.txt'),
                     os.path.realpath('./data/taskdata3.txt')]

 # Upload the application script to Azure Storage. This is the script that
 # will process the data files, and is executed by each of the tasks on the
 # compute nodes.
 application_files = [
     upload_file_to_container(blob_client, app_container_name, file_path)
     for file_path in application_file_paths]

 # Upload the data files. This is the data that will be processed by each of
 # the tasks executed on the compute nodes in the pool.
 input_files = [
     upload_file_to_container(blob_client, input_container_name, file_path)
     for file_path in input_file_paths]
```

Korištenje razumijevanje popisa u `upload_file_to_container` funkcija zove za svaku datoteku u zbirke i dva [ResourceFile] [ py_resource_file] bili popunjeni zbirke. Na `upload_file_to_container` funkcija pojavljuje se ispod:

```
def upload_file_to_container(block_blob_client, container_name, file_path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """
    blob_name = os.path.basename(file_path)

    print('Uploading file {} to container [{}]...'.format(file_path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles

[ResourceFile] [ py_resource_file] sadrži zadatke u seriji URL-om s datotekom u Azure prostora za pohranu koji se preuzima računalnim čvor prije pokretanja tom zadatku. [ResourceFile][py_resource_file]. Svojstvo **blob_source** određuje cijeli URL datoteke kao ona nalazi Azure prostora za pohranu. URL-a može obuhvaćati i zajednički pristup potpis (SAS) koji omogućuje siguran pristup datoteci. Većinu zadataka u seriji uključiti svojstvo *ResourceFiles* , uključujući:

- [CloudTask][py_task]
- [StartTask][py_starttask]
- [JobPreparationTask][py_jobpreptask]
- [JobReleaseTask][py_jobreltask]

Ovaj primjer koristi JobPreparationTask ili JobReleaseTask vrsta zadataka, no potražite dodatne informacije o njima u [Pokreni posao Priprema i dovršetka zadaci na obradu Azure izračunati čvorove](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Zajednički pristup potpis (SAS)

Zajednički pristup potpisi su nizovi koji Omogućivanje sigurnog pristupa spremnika i blob-ova u spremište Azure. Skripta *python_tutorial_client.py* koristi oba blob i kontejner zajednički pristup potpisa i pokazuje kako nabaviti Ti nizovi potpis zajednički pristup servisu za pohranu.

- **Blob zajednički pristup potpisa**: koristi StartTask na resurse bloba zajednički pristup potpisa kada preuzme zadatka skripte i unos podataka datoteke sa servisa za pohranu (pročitajte članak [korak #3](#step-3-create-batch-pool) ispod). Na `upload_file_to_container` funkcija u *python_tutorial_client.py* sadrži kod koji dohvaća svaki blob zajednički pristup potpis. To čini tako da nazovete [BlockBlobService.make_blob_url] [ py_make_blob_url] u modulu za pohranu.

- **Spremnik zajednički pristup potpis**: kao svaki zadatak završi rad na čvor računalnim, prenosi njegov Izlazna datoteka spremniku *Izlaz* u Azure prostora za pohranu. Da biste to učinili, *python_tutorial_task.py* koristi potpis zajednički pristup spremnik koji omogućuje pristup zapisivanju spremnik. Na `get_container_sas_token` funkcija u *python_tutorial_client.py* dohvaća kontejner s zajednički pristup potpis, koji se zatim prosljeđuje kao argumenta naredbenog retka zadaci. Korak #5 [Dodavanje zadataka posla](#step-5-add-tasks-to-job), objašnjava korištenje spremnik SAS.

> [AZURE.TIP] Pogledajte Dvodijelno niza na zajednički pristup potpise, [dio 1: osnove modela SAS](../storage/storage-dotnet-shared-access-signature-part-1.md) i [dio 2: Stvaranje i korištenje SAS sa servisom Blob](../storage/storage-dotnet-shared-access-signature-part-2.md), da biste saznali više o slanju siguran pristup podacima na vašem računu za pohranu.

## <a name="step-3-create-batch-pool"></a>Korak 3: Stvaranje grupe za grupu

![Stvaranje grupe za grupu][3]
<br/>

Obradu **skup** je zbirka računalnim čvorove (virtualnim strojevima) na kojem obradu izvršava zadataka s posla.

Nakon što ga prenosi zadatka skripte i podatkovne datoteke na račun za pohranu, *python_tutorial_client.py* pokreće njegov interakcija sa servisom za obradu pomoću modula Python grupe. Da biste to učinili, [BatchServiceClient] [ py_batchserviceclient] stvara se:

```python
 # Create a Batch service client. We'll now be interacting with the Batch
 # service in addition to Storage.
 credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                              _BATCH_ACCOUNT_KEY)

 batch_client = batch.BatchServiceClient(
     credentials,
     base_url=_BATCH_ACCOUNT_URL)
```

Nakon toga će se skup računalnim čvorove stvorene u račun za obradu s pozivom `create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Kada stvorite zajedničko područje, koje ste definirali [PoolAddParameter] [ py_pooladdparam] koji određuje nekoliko svojstava na resurse:

- **ID** grupe (*id* - obavezno)<p/>Kao i u većini entiteti u seriji nove grupe za aplikacija mora imati jedinstveni ID unutar grupe za račun. Kod koji se odnosi na ovaj skup pomoću njegov ID-a, a nije kako prepoznati skup Azure [portal][azure_portal].

- **Broj računalnim čvorove** (*target_dedicated* - obavezno)<p/>Ovo svojstvo određuje koliko VMs mora biti implementirano u. Važno Imajte na umu da svi računi obradu imaju zadani **kvote** koja se ograničava broj **jezgri** (i zato računalnim čvorove) na računu za obradu je. Zadani kvotama i upute da biste [povećali ograničenja](batch-quota-limit.md#increase-a-quota) (kao što su maksimalni broj jezgri na vašem računu grupe) možete pronaći u [kvotama i ograničenja servisa Azure grupe](batch-quota-limit.md). Ako ste s pitanjem "Zašto neće Moje skup dosegne više od X čvorove?" Uzrok može biti kvota za core.

- **Operacijski sustav** za čvorove (*virtual_machine_configuration* **ili** *cloud_service_configuration* - obavezno)<p/>U *python_tutorial_client.py*ćemo stvoriti skup Linux čvorove pomoću [VirtualMachineConfiguration][py_vm_config]. Na `select_latest_verified_vm_image_with_node_agent_sku` funkcionirati u `common.helpers` pojednostavljuje rad s [Trgovine virtualnim računalima sustava Windows Azure] [ vm_marketplace] slike. Dodatne informacije o korištenju slika trgovine potražite u članku [Dodjeljivanje Linux izračunati čvorove u grupe za Azure grupe](batch-linux-nodes.md) .

- **Veličina računalnim čvorove** (*vm_size* - obavezno)<p/>Jer smo si određivanje Linux čvorove za naše [VirtualMachineConfiguration][py_vm_config], ne možemo odredite veličinu VM (`STANDARD_A1` u ovom primjeru) s [veličine za virtualnim strojevima u Azure](../virtual-machines/virtual-machines-linux-sizes.md). Ponovno potražite u članku [Dodjeljivanje Linux izračunati čvorove u grupe za Azure grupe](batch-linux-nodes.md) da biste saznali više.

- **Početak zadatka** (*start_task* - nije obavezno)<p/>S na iznad svojstva fizičke čvor, možete odrediti [StartTask] [ py_starttask] za skup (nije obavezno). U StartTask se izvršava na svakom čvor taj čvor spaja na resurse i svaki put čvor ponovnog pokretanja. U StartTask je posebno korisno za pripremu računalnim čvorove za izvršavanje zadataka, kao što su instalacije programa koji se pokreću zadataka.<p/>U ovoj aplikaciji uzorak u StartTask kopira datoteke koje se preuzima iz spremišta (koji su navedeni pomoću svojstva u StartTask **resource_files** ) StartTask *rad direktorija* u imeniku za *zajedničko korištenje* koji mogu pristupiti svim zadataka koji se izvode na čvor. Zapravo, to kopira `python_tutorial_task.py` u imeniku za zajedničko korištenje na svakom čvor kao čvor spaja na resurse tako da se svi zadaci koji se pokreću na čvor mogu pristupati.

Mogli biste primijetiti poziv na `wrap_commands_in_shell` preglednika (opis funkcije). Ova funkcija uzima zbirka zasebnom naredbe i stvara jednu naredbenog retka prikladna za svojstvo naredbenog retka na zadatak.

Osim toga primjećuje u isječak koda iznad je korištenje dvije varijable okruženja u svojstvu **command_line** na StartTask: `AZ_BATCH_TASK_WORKING_DIR` i `AZ_BATCH_NODE_SHARED_DIR`. Svaki čvor računalnim unutar grupe aplikacija za obradu automatski se konfigurira pomoću nekoliko varijable okruženja specifične za obradu. Izvođenja procesa koje je izvršio zadatak ima pristup te varijable okruženja.

> [AZURE.TIP] Da biste saznali više o varijable okruženja koje su dostupne na računalnim čvorove u skup grupe, kao i informacije o zadatka rad direktorija, potražite u članku **postavki okruženja za zadatke** i **datoteke i mape** u [Pregled značajki Azure grupe](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Korak 4: Stvaranje obrade

![Stvaranje obrade][4]<br/>

Obrade **posao** zbirku zadataka, a pridruženi skup računalnim čvorove. Zadatke u posao izvršiti na pridruženi skup računalnim čvorove.

Možete koristiti posao ne samo za organiziranje i praćenje zadataka u povezanih radnih opterećenja, nego i za nametanja ograničenja – kao što je maksimalna runtime za posao (i nastavak, njegov zadaci) i posla prioritet u odnosu na druge zadatke na računu za obradu. U ovom primjeru no posao pridruženo samo grupu koja je stvorena u koraku #3. Konfigurirana bez dodatna svojstva.

Sve obrade su vezane uz određenu grupu. To pridruživanje upućuje na to koji se čvorovi izvršavanje zadataka s posla na. Odredite na resurse pomoću [PoolInformation] [ py_poolinfo] svojstvo, kao što je prikazano u nastavku isječak koda.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Sad kad je stvoren posao, zadaci dodaju se za obavljanje posla.

## <a name="step-5-add-tasks-to-job"></a>Korak 5: Dodavanje zadataka posla

![Dodavanje zadataka posla][5]<br/>
*(1) zadaci dodat će se na poslu, (2) zadaci zakazuju pokrenuti čvorove i (3) zadaci preuzimanje podatkovne datoteke za obradu*

Obradu **Zadaci** su pojedinačne jedinice rada izvršiti na čvorove računalnim. Zadatak je naredbenog retka i pokreće skripte ili izvršne datoteke koje ste odredili u tom naredbenog retka.

Da biste zapravo izvršiti, zadaci mora biti dodan na posao. Svaki [CloudTask] [ py_task] je konfiguriran za korištenje svojstvo naredbenog retka i [ResourceFiles] [ py_resource_file] (kao i na resurse StartTask) koje zadatak preuzima čvor prije njegove naredbenog retka automatski izvršava. U uzorku, svaki zadatak obrađuje samo jednu datoteku. Dakle, njegov zbirke ResourceFiles sadrži jedan element.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [AZURE.IMPORTANT] Kada pristupi varijable okruženja kao što su `$AZ_BATCH_NODE_SHARED_DIR` ili izvoditi aplikacija nije pronađen u na čvor `PATH`, reci naredba zadatka morate pozvati ljuske izričito, kao što su s `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Taj zahtjev je nepotreban zadataka izvršavanje aplikacije u na čvor `PATH` i pozvati sve varijable okruženja.

Unutar na `for` petlje u isječak koda iznad, vidjet ćete da se naredbenog retka za zadatak konstruirana s pet Argumenti naredbenog retka koji se prosljeđuju *python_tutorial_task.py*:

1. **FilePath**: to je lokalni put do datoteke kao ona se nalazi na čvor. Kada se u ResourceFile objekt u `upload_file_to_container` stvorena u koraku 2 iznad naziva datoteke je korišten za to svojstvo (u `file_path` parametar u Graditelj ResourceFile). To znači da datoteku moguće je pronaći u istom direktorij na čvor kao *python_tutorial_task.py*.

2. **NumWords**: gornju *N* riječi koje valja staviti na izlazna datoteka.

3. **storageaccount**: naziv računa spremišta vlasništvu spremnika u koji želite prenijeti izlaz zadatka.

4. **storagecontainer**: naziv spremnika za pohranu na koje želite prenijeti datoteke izlaz.

5. **sastoken**: potpis zajednički pristup (SAS) koji omogućuje pristup zapisivanju spremnik **Izlaz** iz spremišta Azure. Skripta *python_tutorial_task.py* koristi zajednički pristup potpis kada stvara referencu BlockBlobService. To omogućuje pristup pisanje spremnik bez tipkovni prečac za račun za pohranu.

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Korak 6: Praćenje zadataka

![Praćenje zadataka][6]<br/>
*Skripta (1) nadzire zadaci za stanje dovršenja i (2) zadaci prenijeti podatke za rezultat Azure prostora za pohranu*

Kada se zadaci dodaju na poslu, automatski u redu čekanja i im zakazano za izvršavanje na računalnim čvorove unutar skup povezan s posla. Na temelju postavke koje navedete obradu rukuje sve stavljanje zadatka, Planiranje, implementacijom i drugih zadataka administracije obavezama koje imate umjesto vas.

Postoji mnogo postupke za nadzor izvršavanje zadataka. Na `wait_for_tasks_to_complete` funkcija u *python_tutorial_client.py* nudi jednostavnog primjera praćenje zadataka s određenim stanja u tom slučaju [dovršiti] [ py_taskstate] stanje.

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>Korak 7: Preuzimanje izlaz zadatka

![Preuzimanje zadatka Izlaz iz spremišta][7]<br/>

Sad kad dovršetka posla Izlaz iz zadaci možete preuzeti s Azure prostora za pohranu. To možete učiniti s pozivom `download_blobs_from_container` u *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [AZURE.NOTE] Poziv na `download_blobs_from_container` *python_tutorial_client.py* određuje se da se datoteka preuzeti na početnom direktorija. Slobodno izmjena ovo mjesto za izlazne podatke.

## <a name="step-8-delete-containers"></a>Korak 8: Spremnici Izbriši

Budući da vam se naplatiti za podatke koji se nalazi u odjeljku pohrana Azure, preporučuje se uvijek u da biste uklonili sve blob-ova koje više nisu potrebne za vaše obrade. U *python_tutorial_client.py*, to možete učiniti s tri poziva [BlockBlobService.delete_container][py_delete_container]:

```
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Korak 9: Brisanje posla i na resurse

U posljednjem koraku se od vas zatraži da biste izbrisali zadatak i grupe aplikacija koje su stvorene pomoću skripte *python_tutorial_client.py* . Iako vam se naplatiti za zadatke i zadacima, *su* naplatiti računalnim čvorove. Stoga preporučujemo da dodijeliti čvorove samo po potrebi. Brisanje nekorištenih grupe može biti dio održavanja procesa.

U BatchServiceClient [JobOperations] [ py_job] i [PoolOperations] [ py_pool] imat odgovarajuće metode brisanja koje se nazivaju ako potvrda brisanja:

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [AZURE.IMPORTANT] Imajte na umu da vam se naplatiti za resurse računalnim – brisanje nekorištenih grupe minimiziranje trošak. Osim toga, imajte na umu da brisanje zajedničko područje briše sve čvorove računalnim u tu grupu, a da sve podatke na čvorove bit će nepopravljiva nakon brisanja na resurse.

## <a name="run-the-sample-script"></a>Pokrenuti skriptu uzorka

Kada pokrenete skriptu *python_tutorial_client.py* iz vodiča [uzorka kod][github_article_samples], izlazna konzole je slično sljedećem. Postoji Pauziraj pri `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` dok se stvaraju na resurse računalnim čvorove uz rada, a zatim se provode naredbi u zadatku start na resurse. Pomoću [portala za Azure] [ azure_portal] da biste pratili vaše skup, izračunati čvorove, posla i zadatke tijekom i nakon izvršavanja. Pomoću [portala za Azure] [ azure_portal] ili [Microsoft Azure prostora za pohranu Explorer] [ storage_explorer] da biste pogledali resursa za pohranu (spremnika i blob-ova) koje su stvorene u aplikaciji.

>[AZURE.TIP] Pokrenuti skriptu *python_tutorial_client.py* unutar na `azure-batch-samples/Python/Batch/article_samples` direktorija. Koristi relativni put za na `common.helpers` uvoz modula, pa biste mogli vidjeti `ImportError: No module named 'common'` ako ne pokrenete u skriptu iz imenika.

Uobičajeni vrijeme izvođenja je **otprilike 5-7 minuta** kada pokrenete uzorka njegov zadanoj konfiguraciji.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a>Daljnji koraci

Slobodno promjene *python_tutorial_client.py* i *python_tutorial_task.py* eksperimentiranje s računalnim različitim scenarijima. Na primjer, pokušajte dodati programa vrijeme izvođenja *python_tutorial_task.py* za zamjenu dugoročnih zadaci i nadzor ih na portalu. Pokušajte dodavanjem više zadataka ili Prilagodba broj računalnim čvorove. Dodavanje logike provjeru i omogućuje korištenje postojeće grupe aplikacija za vrijeme izvođenja brzinu.

Sad kad ste upoznati s osnovni tijek rada rješenja za obradu, vrijeme je za istražujte dodatnim značajkama servisa za grupu.

- Pročitajte članak [Pregled grupe za Azure značajke](batch-api-basics.md) koje preporučujemo ako ste novi korisnik usluge.
- Pokretanje na druge grupe za razvoj članaka u **razvoju detaljnije** u [grupe tečaj][batch_learning_path].
- Pogledajte drugoj implementaciji obrade radno opterećenje "N najgornjih riječi" pomoću obrade u [TopNWords] [ github_topnwords] uzorka.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage

[pypi_install]: http://python-packaging-user-guide.readthedocs.io/en/latest/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Stvaranje spremnika u prostor za pohranu za Azure"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Prijenos aplikacije zadatka i unos (podaci) datoteke spremnika"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Stvaranje grupe za grupu"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Stvaranje obrade"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Dodavanje zadataka posla"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Praćenje zadataka"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Preuzimanje zadatka Izlaz iz spremišta"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Rješenje obradu tijeka rada (cijeli dijagram)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Vjerodajnice za obradu portalu"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Vjerodajnice za pohranu na portalu"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Rješenje obradu tijeka rada (minimalan dijagram)"
