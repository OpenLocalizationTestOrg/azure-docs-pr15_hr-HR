<properties
    pageTitle="Čvorovi Linux u grupe za Azure grupe | Microsoft Azure"
    description="Saznajte kako da obradi paralelni računalni radnih opterećenja na grupe Linux virtualnim strojevima u seriji Azure."
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="marsma" />

# <a name="provision-linux-compute-nodes-in-azure-batch-pools"></a>Dodjela Linux računalnim čvorove u grupe za Azure grupe

Grupe za Azure možete koristiti da biste pokrenuli radnih opterećenja paralelni računalni na virtualnim strojevima Linux i Windows. U ovom se članku detaljno kako stvoriti grupe Linux računalnim čvorove u servis za obradu pomoću oba na [Obradu Python] [ py_batch_package] i [Grupe za .NET] [ api_net] biblioteke klijenta.

> [AZURE.NOTE] [Pakete](batch-application-packages.md) trenutno nisu podržana na čvorove računalnim Linux.

## <a name="virtual-machine-configuration"></a>Konfiguracija virtualnog računala

Kada stvarate skup računalnim čvorove u seriji, koje su vam dvije mogućnosti iz koje će se odaberite željenu veličinu čvor i operacijski sustav: Konfiguriranje usluge oblaka i konfiguracija virtualnog računala.

**Konfiguriranje usluge oblaka** nudi Windows čvorove *samo*izračunati. Dostupne računalnim čvor veličine navedene su u [veličinama za servise u Oblaku](../cloud-services/cloud-services-sizes-specs.md), a dostupna operacijski sustavi navedene su u [Azure goste OS izdanja i SDK kompatibilnosti matrice](../cloud-services/cloud-services-guestos-update-matrix.md). Kada stvorite grupu koja sadrži čvorove Azure servise u Oblaku, morate navesti samo veličinu čvor i njezinih "OS," koji se nalaze u prethodno spomenuta člancima. Za grupe sustava Windows za izračun čvorove, servise u Oblaku najčešće koriste.

**Konfiguracija virtualnog računala** sadrži slike Linux i Windows za čvorove računalnim. Dostupne računalnim čvor veličine navedene su u [veličine za virtualnim strojevima u Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) i [veličine za virtualnim strojevima u Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Kada stvorite grupu koja sadrži čvorove konfiguracija virtualnog računala, morate navesti veličinu čvorove, referenca slika virtualnog računala i čvor agent grupe za SKU-om za instalaciju na čvorove.

### <a name="virtual-machine-image-reference"></a>Referenca slika virtualnog računala

Servis za obradu koristi [Skaliranje virtualnog računala postavlja](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) kako čvorove računalnim Linux. Operacijski sustav slike za ove virtualnim strojevima nudi [Trgovine Windows Azure][vm_marketplace]. Kada konfigurirate reference slika virtualnog računala, navedite svojstva virtualnog računala slike trgovine. Kada stvarate referencu slika virtualnog računala, potrebni su sljedeća svojstva:

| **Svojstva referenca slike** | **Primjer** |
| ----------------- | ------------------------ |
| Publisher         | Kanonski                |
| Ponude             | UbuntuServer             |
| SKU               | 14.04.4-LTS              |
| Verzija           | najnovije                   |

> [AZURE.TIP] Dodatne informacije o tim svojstvima i kako popis trgovine slike u [Kretanje i odaberite slika Linux virtualnog računala u Azure s EŽA ili PowerShell](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md). Imajte na umu da su sve slike trgovine trenutno kompatibilan s grupe. Dodatne informacije potražite u članku [čvor agent SKU-om](#node-agent-sku).

### <a name="node-agent-sku"></a>Agent za čvor SKU

Agent za obradu čvor je program koji pokreće svaki čvor u i nudi sučelje naredba i kontrola između čvor i servis za obradu. Postoje različite implementacije agent čvor koji se nazivaju SKU-ove, za različite operacijske sustave. Zapravo, prilikom stvaranja virtualnog računala konfiguracije odredite referencu slika virtualnog računala, a zatim navedite agent čvor da biste instalirali na slici. Obično svaki pojedini agent čvor SKU kompatibilan s više slika virtualnog računala. Evo nekoliko primjera čvor agent SKU-ove:

* batch.Node.ubuntu 14.04
* batch.Node.centos 7
* batch.Node.Windows amd64

> [AZURE.IMPORTANT] Sve slike virtualnog računala koje su dostupne na tržištu su kompatibilni s trenutno dostupno agenata čvor grupe. Morate koristiti obradu SDK-ovi na popisu dostupna čvor agent SKU-ove i slike virtualnog računala s kojim su kompatibilne. [Slika popisa virtualnog računala](#list-of-virtual-machine-images) u nastavku ovog članka dodatne informacije potražite u članku.

## <a name="create-a-linux-pool-batch-python"></a>Stvaranje grupe aplikacija Linux: Python grupe

Sljedeći isječak koda prikazani Primjeri kako koristiti [Microsoft Azure obradu klijentska biblioteka za Python] [ py_batch_package] da biste stvorili skup Ubuntu poslužitelja računalnim čvorove. Referentnu dokumentaciju za modul Python grupe možete pronaći na [paket azure.batch] [py_batch_docs] na čitanje dokumenti.

U ovom isječak stvara [ImageReference] [ py_imagereference] izričito i određuje svih svojstava (publisher ponuda, SKU-om, verzija). U kodu proizvodnje, no preporučujemo da koristite [list_node_agent_skus] [ py_list_skus] način da biste odredili, a zatim odaberite dostupne slike i čvor agent SKU kombinacije tijekom rada.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Kao što je već rečeno, preporučujemo da umjesto stvaranja [ImageReference] [ py_imagereference] izričito, koristite [list_node_agent_skus] [ py_list_skus] način dinamički birati kombinacije slika trenutno podržanih čvor agent/trgovine. Sljedeći isječak Python prikazuje korištenje ove metode.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Stvaranje grupe aplikacija Linux: grupe za .NET

Sljedeće koda prikazani Primjeri kako koristiti [Grupe za .NET] [ nuget_batch_net] klijentska biblioteka za stvaranje skupa Ubuntu poslužitelja računalnim čvorove. Možete pronaći [referentnu dokumentaciju za obradu .NET] [ api_net] na MSDN-u.

Sljedeći isječak koda koristi [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] način da biste odabrali s popisa trenutno podržava trgovine slike i čvor agent SKU kombinacije. Ovu tehniku je poželjno jer na popisu podržanih kombinacije s vremena na vrijeme mogu se promijeniti. Najčešće, podržane kombinacije dodat će se.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

Iako prethodni isječak koristi [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] način dinamički popis, a zatim odaberite podržan slike i čvor agent SKU kombinacije (preporučeno), možete konfigurirati i [ImageReference] [ net_imagereference] izričito:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Popis slika virtualnog računala

U sljedećoj su tablici navedeni slike virtualnog računala trgovine koji su kompatibilni s dostupna agenata čvor grupe prilikom zadnjeg ažuriranja ovog članka. Nije važno Imajte na umu da ovaj popis nisu potpuni jer slike i čvor agenata može dodati ili ukloniti u bilo kojem trenutku. Preporučujemo da obradu aplikacije i servise uvijek koristite [list_node_agent_skus] [ py_list_skus] (Python) i [ListNodeAgentSkus] [ net_list_skus] (grupe za .NET) da biste odredili, a zatim odaberite trenutno dostupno SKU-ove.

> [AZURE.WARNING] Na sljedećem su popisu mogu se promijeniti u bilo kojem trenutku. Uvijek koristi na **popisu čvor agent SKU** načine u grupe API-ji za popis, a zatim odaberite iz kompatibilne virtualnog računala i čvor agent SKU-ove kada pokrenete vaše obrade.

| **Publisher** | **Ponude** | **Slika SKU** | **Verzija** | **Agent za čvor SKU ID** |
| ------- | ------- | ------- | ------- | ------- |
| Kanonski | UbuntuServer | 14.04.0-LTS | najnovije | batch.Node.ubuntu 14.04 |
| Kanonski | UbuntuServer | 14.04.1-LTS | najnovije | batch.Node.ubuntu 14.04 |
| Kanonski | UbuntuServer | 14.04.2-LTS | najnovije | batch.Node.ubuntu 14.04 |
| Kanonski | UbuntuServer | 14.04.3-LTS | najnovije | batch.Node.ubuntu 14.04 |
| Kanonski | UbuntuServer | 14.04.4-LTS | najnovije | batch.Node.ubuntu 14.04 |
| Kanonski | UbuntuServer | 14.04.5-LTS | najnovije | batch.Node.ubuntu 14.04 |
| Kanonski | UbuntuServer | 16.04.0-LTS | najnovije | batch.Node.ubuntu 16.04 |
| Credativ | Debian | 8 | najnovije | batch.Node.debian 8 |
| OpenLogic | CentOS | 7.0 | najnovije | batch.Node.centos 7 |
| OpenLogic | CentOS | 7.1 | najnovije | batch.Node.centos 7 |
| OpenLogic | CentOS HPC | 7.1 | najnovije | batch.Node.centos 7 |
| OpenLogic | CentOS | 7,2 | najnovije | batch.Node.centos 7 |
| Oracle | Oracle Linux | 7.0 | najnovije | batch.Node.centos 7 |
| SUSE | openSUSE | 13.2 | najnovije | batch.Node.opensuse 13.2 |
| SUSE | openSUSE Skok | 42.1 | najnovije | batch.Node.opensuse 42.1 |
| SUSE | SLES HPC | 12 | najnovije | batch.Node.opensuse 42.1 |
| SUSE | SLES | 12 SP1 | najnovije | batch.Node.opensuse 42.1 |
| Microsoft reklame | Standardna-podataka – znanstvenog-vm | Standardna-podataka – znanstvenog-vm | najnovije | batch.Node.Windows amd64 |
| Microsoft reklame | Linux-podataka – znanstvenog-vm | linuxdsvm | najnovije | batch.Node.centos 7 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 | najnovije | batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012 Standard | najnovije | batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012 R2 Standard | najnovije | batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Pretpregled sustava Windows – Server – Tehnički | najnovije | batch.Node.Windows amd64 |

## <a name="connect-to-linux-nodes"></a>Povezivanje s čvorove Linux

Tijekom razvoja ili tijekom otklanjanje poteškoća, možda će vam potrebne da biste se prijavili čvorove u svoje grupe aplikacija. Za razliku od čvorove računalnim Windows, nećete moći koristiti protokola udaljene radne površine (RDP) za povezivanje s čvorove Linux. Umjesto toga, servis za obradu omogućuje pristup SSH na svakom čvor za udaljenu.

Sljedeće Python koda stvara korisnika na svakom čvor u područje koje je potrebno za udaljene. Zatim se ispisuje podatke o vezi sigurne ljuske (SSH) za svaki čvor.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Evo ogledne izlaz za prethodni kod za grupu koja sadrži četiri čvorove Linux.

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Imajte na umu da umjesto lozinkom, možete odrediti SSH javni ključ prilikom stvaranja korisnika na čvor. U Python SDK to obavlja pomoću parametar **ssh_public_key** na [ComputeNodeUser][py_computenodeuser]. U .NET, to je izvršiti pomoću [ComputeNodeUser][net_computenodeuser]. [SshPublicKey] [net_ssh_key] svojstvo.

## <a name="pricing"></a>Cijene

Grupe za Azure se temelji na servise u Oblaku Azure i virtualnim računalima sustava Azure tehnologija. Servis za obradu sam je ponuđen pri bez trošak, što znači da vam se naplatiti samo za resurse računalnim zauzeti vašeg rješenja za obradu. Kad odaberete **Konfiguracija servisa oblaka**, koje naplatiti utemeljene na [servise u Oblaku cijene] [ cloud_services_pricing] strukturu. Kad odaberete **Konfiguracija virtualnog računala**, koje naplatiti utemeljene na [virtualnim strojevima cijene] [ vm_pricing] strukturu.

## <a name="next-steps"></a>Daljnji koraci

### <a name="batch-python-tutorial"></a>Praktični vodič Python grupe

Za detaljnije vodič o radu s grupe pomoću Python potražite u članku [Početak rada s klijentom Python obradu Azure](batch-python-tutorial.md). Suradnik [uzorak koda] [ github_samples_pyclient] obuhvaća funkciju preglednika, `get_vm_config_for_distro`, koji prikazuje Druga tehnika da biste dobili konfiguraciju virtualnog računala.

### <a name="batch-python-code-samples"></a>Obrada Python primjere koda

Pogledajte na druge [Python kod uzorka] [ github_samples_py] u [azure, grupe i uzorke] [ github_samples] spremište na GitHub nekoliko skripte koji vam pokazuju kako izvoditi uobičajene grupe operacija kao što su grupe, posla i stvaranje zadataka. [Datoteka pročitajme za] [ github_py_readme] koji se pridružuje u Python uzoraka sadrži upute za instaliranje potrebne paketa.

### <a name="batch-forum"></a>Forum za obradu

[Forum za obradu Azure] [ forum] na MSDN-sjajno su početno mjesto raspravljati grupe i postavljati pitanja o servisu. Čitanje objava korisno "stickied", i pitanja kao što su biste tijekom stvaranja vašeg rješenja za obradu.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
