<properties
   pageTitle="Preporučeno konvencije imenovanja za Azure resurse | Smjernice | Microsoft Azure"
   description="Preporučuje se konvencije imenovanja za Azure resurse. Kako imenovati virtualnim strojevima, račune za pohranu, mrežama, virtualne mreže, podmreže i ostali Azure entiteti"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/27/2016"
   ms.author="christb"/>
   
# <a name="recommended-naming-conventions-for-azure-resources"></a>Preporučena konvencije imenovanja za Azure resursi

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Odabir naziv bilo kojeg resursa u Microsoft Azure važan jer:

- Ovo je teško naknadno promijeniti naziv.
- Nazivi mora zadovoljavati uvjete njihove vrste određeni resurs.

Dosljedne konvencije imenovanja načinite resurse lakše pronašli. Također možete označiti ulogu resursa u rješenje.

Ovaj se članak odnosi sažetak pravila imenovanja i ograničenja za Azure resurse i referentne vrijednosti skupa preporuke za konvencije imenovanja.  Te preporuke i dalje možete koristiti kao početnu točku za vlastitu konvencije određene vašim potrebama.  

Ključ uspješnog s konvencije imenovanja je uspostavljanje te ih pratiti preko aplikacija i tvrtke ili ustanove. 

## <a name="naming-subscriptions"></a>Dodjela naziva pretplate

Prilikom imenovanja Azure pretplate opširno imena provjerite objašnjenje kontekst i svrha Očisti pretplate.  Kada radite u okruženju s mnogo pretplate, pratiti zajedničkog konvencija imenovanja možete poboljšati preglednosti.

Preporučena uzorak imenovanja pretplate je:

`<Company> <Department (optional)> <Product Line (optional)> <Environment>`

- Tvrtka obično će biti iste za svaku pretplatu. Međutim, neke tvrtke može imati podređene tvrtke unutar tvrtke ili ustanove strukture. Te tvrtke možda upravlja središnje IT grupe. U tim slučajevima ih nije moguće razlikuju nemaju naziv nadređene tvrtke (*Contoso*) i naziv tvrtke podređeni (*Sjeverna vjetar*).

- Odjel je naziv unutar tvrtke ili ustanove u kojem raditi grupa pojedinaca. Stavka unutar prostora za naziv kao neobavezno.

- Redak proizvoda je određeni naziv proizvoda ili funkcija koja se izvršava iz unutar odjel.
To je obično obavezna za interne services i web-aplikacije. Međutim, preporučujemo da biste koristili za dostupnog javnosti servise koje je potrebno jednostavno odvojenosti i identifikaciju (kao što je čišćenje odvojenosti naplate zapisa).

- Okruženje je naziv koji opisuje životnim ciklusom implementaciju aplikacije ili servisa, kao što je razvojni, značajke pitanja i odgovora ili grupa.

| Tvrtke | Odjelu | Redak proizvoda ili usluga | Okruženje | Ime i prezime  |
----------| ---------- | ----------------------- | ----------- | ---------- |
| Contoso | SocialGaming | AwesomeService | Proizvodne | Proizvodne tvrtke Contoso SocialGaming AwesomeService |
| Contoso | SocialGaming | AwesomeService | Razvojni | Contoso SocialGaming AwesomeService razvojni |
| Contoso | GA | InternalApps | Proizvodne | Contoso IT radnog InternalApps |
| Contoso | GA | InternalApps | Razvojni | Contoso IT razvojni InternalApps |

<!-- TODO; include more information about organizing subscriptions for application deployment, pods, etc. -->

## <a name="use-affixes-to-avoid-ambiguity"></a>Korištenje Affixes da biste izbjegli Dvosmislenosti

Pri dodjeli resursa u Azure, preporučuje se da biste koristili uobičajene Prefiksi ili nastavke da biste odredili vrstu i kontekst resursa.  Dok sve informacije o vrsti, metapodaci, kontekstu programski je dostupna, Primjena uobičajenih affixes pojednostavljuje vizualne identifikacijski.  Kada uključite affixes u vašem konvencija imenovanja, važno je da biste jasno odredite hoće li se na affix na početku naziv (prefiks) ili na kraju (sufiks).  

Ako, primjerice, Evo dva moguća imena za usluge hostinga izračuna modul:

- SvcCalculationEngine (prefiks)
- CalculationEngineSvc (sufiks)

Affixes može se odnositi na različitih aspekata koje opisuju određene resurse. Sljedeća tablica prikazuje primjere obično koristi.

| Razmjer | Primjer | Bilješke |
| ------ | ------- | ----- |
| Okruženje | Razvojni, katalog, značajke pitanja i odgovora | Služi za identifikaciju okruženja za resurs |
| Mjesto | uw (NAM Zapad), vrijednost (NAM istok) | Služi za identifikaciju regija u koji je implementiran resursa |
| Instance | 01, 02 | Za resurse koji imaju više od jedne imenovani instance (web-poslužiteljima, itd.). |
| Proizvod ili uslugu | servis | Služi za identifikaciju proizvoda, aplikacije ili servisa koji podržava resursa |
| Uloga | SQL, web-, razmjenu poruka | Služi za identifikaciju ulogu pridružene resursa |

Prilikom razvoja određene konvencija imenovanja za projekte ili tvrtke je važno je napomenuti da biste odabrali zajednički skup affixes i njihov položaj (sufiks "ili" prefiks ").

## <a name="naming-rules-and-restrictions"></a>Pravila imenovanja i ograničenja

Vrsti resursa ili servisa Azure nameće skup imenovanja ograničenja i opseg; konvencije imenovanja ni uzorak moraju biti potrebni pravila imenovanja i opseg.  Na primjer, dok je naziv na VM karte DNS naziv (i zato potreban je moraju biti jedinstvene preko svih Azure), naziv je VNET implementaciju ograničen je na grupu resursa koji je stvoren u.

Općenito govoreći, izbjegnete posebne znakove (`-` ili `_`) kao znak prve ili zadnje u bilo koji naziv. Te znakove uzrokovat će većina pravila za provjeru valjanosti uvoza.

| Kategorija | Usluga ili entitet | Opseg | Duljina | Kućište | Valjani znakovi | Predloženi uzorak | Primjer |
| ------------- | ----------------- | ----- | ------ | ------ | ---------------- | ----------------- | ------- |
| Grupa resursa | Grupa resursa | Globalni | 1-64 | Slova | Alphanumeric, podvlake i spojnica | `<service short name>-<environment>-rg` | `profx-prod-rg` |
| Grupa resursa | Postavljanje dostupnosti | Grupa resursa | 1 80 | Slova | Alphanumeric, podvlake i spojnica | `<service-short-name>-<context>-as` | `profx-sql-as` |
| Općenito | Oznaka | Pridruženi entitet | 512 (ime), 256 (vrijednost) | Slova | Alfanumerički | `"key" : "value"` | `"department" : "Central IT"` |
| Izračun | Virtualnog računala | Grupa resursa | 1-15 | Slova | Alphanumeric, podvlake i spojnica | `<name>-<role>-<instance>` | `profx-sql-001` |
| Prostor za pohranu | Naziv računa za pohranu (podaci) | Globalni | 3 24 | Mala slova | Alfanumerički | `<service short name><type><number>` | `profxdata001` |
| Prostor za pohranu | Naziv računa za pohranu (diskova) | Globalni | 3 24 | Mala slova | Alfanumerički | `<vm name without dashes>st<number>` | `profxsql001st0` |
| Prostor za pohranu | Naziv spremnika | Račun za pohranu | 3 63 |   Mala slova | Alphanumeric i crtica | `<context>` | `logs` |
| Prostor za pohranu | Naziv blobova platforme | Spremnik | 1 1024 | Velika i mala slova | Bilo koji znak URL-a | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Prostor za pohranu | Naziv reda čekanja | Račun za pohranu | 3 63 | Mala slova | Alphanumeric i crtica | `<service short name>-<context>-<num>` | `awesomeservice-messages-001` |
| Prostor za pohranu | Naziv tablice | Račun za pohranu | 3 63 |Slova | Alfanumerički | `<service short name>-<context>` | `awesomeservice-logs` |
| Prostor za pohranu | Naziv datoteke | Račun za pohranu | 3 63 | Mala slova | Alfanumerički | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Povezivanje s mrežom | Virtualne mreže (VNet) | Grupa resursa | 2-64 | Velika i mala slova | Alphanumeric, crtice, podvlake i razdoblje. | `<service short name>-[section]-vnet` | `profx-vnet` |
| Povezivanje s mrežom | Podmreže | Nadređeni VNet | 2-80 | Velika i mala slova | Alphanumeric, podvlake, crtice i razdoblje. | `<role>-subnet` | `gateway-subnet` |
| Povezivanje s mrežom | Mrežno sučelje | Grupa resursa | 1 80 | Velika i mala slova | Alphanumeric, crtice, podvlake i razdoblje. | `<vmname>-<num>nic` | `profx-sql1-1nic` |
| Povezivanje s mrežom | Mrežni sigurnosne grupe | Grupa resursa | 1 80 | Velika i mala slova | Alphanumeric, crtice, podvlake i razdoblje. | `<service short name>-<context>-nsg` | `profx-app-nsg` |
| Povezivanje s mrežom | Pravilo mreže sigurnosne grupe | Grupa resursa | 1 80 | Velika i mala slova | Alphanumeric, crtice, podvlake i razdoblje. | `<descriptive context>` | `sql-allow` |
| Povezivanje s mrežom | Javnu IP adresa | Grupa resursa | 1 80 | Velika i mala slova | Alphanumeric, crtice, podvlake i razdoblje. | `<vm or service name>-pip` | `profx-sql1-pip` |
| Povezivanje s mrežom | Opterećenja | Grupa resursa | 1 80 | Velika i mala slova | Alphanumeric, crtice, podvlake i razdoblje. | `<service or role>-lb` | `profx-lb` |
| Povezivanje s mrežom | Učitavanje Config Balansirani pravila | Opterećenja | 1 80 | Velika i mala slova | Alphanumeric, crtice, podvlake i razdoblje. | `descriptive context` | `http` |

<!-- TODO fill in the rest of these resources
| Networking | Azure Application Gateway | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `<service or role>-aag` | `profx-aag`
| Networking | Azure Application Gateway Connection | Azure Application Gateway | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `` | TODO
| Networking | Traffic Manager Profile | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | TODO | TODO
-->

## <a name="organizing-resources-with-tags"></a>Organiziranje resursi s oznakama

Voditelj resursa Azure podržava označavanju entiteti s proizvoljne tekstnih nizova za prepoznavanje kontekst i pojednostaviti automatizaciju.  Na primjer, oznake `"sqlVersion: "sql2014ee"` mogu identificirati VMs implementacije izvodi SQL Server 2014 Enterprise Edition za pokretanje automatskog skripte od njih.  Oznaka moraju se proširiti i poboljšati kontekst strane konvencije imenovanja odabrali.

> [AZURE.TIP]Jedna prednosti oznaka je oznake obuhvaćaju je grupe resursa, što omogućuje povezivanje i povezivanje entiteti preko raznovrsne implementacije.

Svaki resurs ili grupa resursa može sadržavati najviše **15** oznake. Naziv oznake ograničeno je na 512 znakova, a vrijednost oznake ograničeno je na dulje od 256 znakova.

Dodatne informacije o resursu za označavanje pogledajte [Korištenje oznake da biste organizirali Azure resurse](../resource-group-using-tags.md).

Neke od uobičajena slučaja koristi za označavanje su:

- **Naplata**; Resursi za grupiranje i pridruže naplata ili trošak sigurnosno šifre.
- **Servis kontekst identifikacijski**; Prepoznavanje grupa resursa preko grupe resursa za uobičajene operacije i grupiranje
- **Kontrola pristupa i sigurnosni kontekst**; Administratorska uloga identifikacijski na temelju portfelja sustava, usluge, aplikacije, instancu, itd.

> [AZURE.TIP]Označavanje ranijeg - često oznaka.  Bolje da bi osnovne označavanje shemu na mjestu i prilagodite tijekom vremena, a ne morate otvarati retrofit nakon toga.  

Primjer nekoliko uobičajenih označavanju pristupa:

| Naziv oznake | Ključ | Primjer | Komentar |
| -------- | --- | ------- | ------- |
| Računa za / Interna Chargeback ID-a | billTo  | `IT-Chargeback-1234` | Interna/i ili Šifra za naplatu |
| Operator ili izravno odgovorne pojedinac (DRI) | managedBy | `joe@contoso.com`  | Pseudonim ili e-pošte adresa |
| Naziv projekta | Naziv projekta | `myproject`  | Naziv retka projekta ili proizvoda |
| Verzija projekta | verzija projekta | `3.4`  | Verzija retka projekta ili proizvoda |
| Okruženje | okruženje | `<Production, Staging, QA >` | Okolini identifikator | 
| Razina | Razina | `Front End, Back End, Data` | Identifikacijski sloju ili uloga/kontekst |
| Podataka profila | dataProfile | `Public, Confidential, Restricted, Internal` | Osjetljivost podataka pohranjenih u resursu |
 
## <a name="tips-and-tricks"></a>Savjeti i upute

Neke vrste resursa biti potrebna dodatna pažnju na i konvencija imenovanja.

### <a name="virtual-machines"></a>Virtualnim strojevima

Osobito u većim topologija pažljivo imenovanja virtualnim strojevima pojednostavnjuje prepoznavanja radno mjesto i svrha svakom računalu i omogućavanja predvidljivi skriptiranje.

> [AZURE.WARNING]Svaki virtualnog računala u Azure ima i naziva programa Azure resursa i operacijski sustav naziv glavnog računala.  
> Ako se naziv resursa i naziv glavnog računala razlikuju, upravljanje u VMs možda zahtjevne i moraju se izbjegavati.
> Ako, na primjer, virtualnog računala stvara se iz programa .vhd koje se već sadrži konfigurirani operacijski sustav uz naziv glavnog računala.

- [Konvencije imenovanja za Windows Server VMs](https://support.microsoft.com/en-us/kb/188997)

<!-- TODO - recommendations on naming VMs. -->

### <a name="storage-accounts-and-storage-entities"></a>Računi za pohranu i spremište entiteti

Postoje dva slučaja prvenstveno se koristi za pohranu računi – sigurnosno diskova za VMs i spremanje podataka u BLOB-ova, redovima i tablice.  Prostor za pohranu račune koji se koriste za VM diskova trebali biste slijedite konvencija imenovanja od Pridruživanjem nadređenog VM naziv (i s potencijalne potrebe za više računa za pohranu za visokokvalitetni VM SKU-ove, primijeniti broj sufiks).

> [AZURE.TIP]Računi za pohranu – li za podatke ili diskova - morate slijediti konvencija imenovanja koja omogućuje više prostora za pohranu računa da biste leveraged (odnosno uvijek se pomoću numeričke sufiks).

Ga moguće konfigurirati prilagođenog naziva domene za pristup podacima blob na vašem računu Azure prostor za pohranu.
Zadana krajnja točka za servis Blob ima `https://mystorage.blob.core.windows.net`.

No ako mapirali prilagođenu domenu (kao što je www.contoso.com) krajnjoj blobova platforme za vaš račun za pohranu, možete pristupiti i putem te domene bloba podatke na računu za pohranu. Na primjer, s prilagođenog naziva domene, `http://mystorage.blob.core.windows.net/mycontainer/myblob` se može pristupiti kao `http://www.contoso.com/mycontainer/myblob`.

Dodatne informacije o konfiguriranju značajka priručniku [Konfiguriraj prilagođenog naziva domene za vaše krajnju točku spremište blobova platforme](../storage/storage-custom-domain-name.md).

Dodatne informacije o imenovanju blob-ova, spremnika i tablice:

- [Dodjela naziva i pozivate spremnika, blob-ova i metapodaci](https://msdn.microsoft.com/library/dd135715.aspx)
- [Imenovanje redovima i metapodacima](https://msdn.microsoft.com/library/dd179349.aspx)
- [Dodjela naziva tablice](https://msdn.microsoft.com/library/azure/dd179338.aspx)

Naziv blob može sadržavati bilo koju kombinaciju znakova, ali rezerviranih znakova URL-a mora biti ispravno unijeti prespojni znak. Izbjegavanje blob nazive koji završavati točkom (.), kosu crtu (/) ili niz ili kombinaciju jednog i drugog. Konvencije, kosa crta je **virtualne** razdjelnik direktorija. Korištenje obrnutom kosom crtom (\) u nazivu blob. Klijent API-ji možda dopustiti, ali neće ispravno raspršivanja, a ne odgovara potpisi.

Nije moguće izmijeniti naziv računa za pohranu ili spremnik kada je stvorena.
Ako želite koristiti novi naziv, morate izbrisati i stvorite novi.

> [AZURE.TIP] Preporučujemo da prije inicijativu razvoja novog servisa ili aplikacija uspostaviti imenovanja za sve račune za pohranu i vrste.

## <a name="example---deploying-an-n-tier-service"></a>Primjer – implementacija na servis n sloja

U ovom primjeru smo definiranje Konfiguracija programa n sloja servis na koji se sastoji od IIS sučelja (smještena u sustavu Windows Server VMs), sa sustavom SQL Server (smješten u dva VMs Windows Server), u ElasticSearch klaster (smješten u 6 Linux VMs) i pridruženi prostora za pohranu korisničke račune, virtualne mreže resursa grupirati i raspoređivača učitavanja.

Ne možemo ćete pokrenuti definiranjem kontekstne konvencije za ovu aplikaciju:

| Entitet | Konvencije | Opis  |
| ------ | ---------- | ------------ |  
| Naziv usluge | `profx` | Kratki naziv računala ili usluge uvodi |
| Okruženje | `prod` | To vrijedi za implementaciju proizvodnje (umjesto značajke pitanja i odgovora, testiranje i itd.) |

Iz tog osnovne smo zatim možete mapirati odgovor u obliku za sve vrste resursa:

| Vrsta resursa | Osnovni konvencije | Primjer |
| ------------- | --------------- | ------- |
| Pretplate | `<Company> <Department (optional)> <Product Line (optional)> <Environment>` | `Contoso IT InternalApps Profx Production` |
| Grupa resursa | `servicename-rg` | `profx-rg` |
| Virtualne mreže | `servicename-vnet` | `profx-vnet` |
| Podmreže | `role-subnet` | `sql-vnet` |
| Opterećenja | `servicename-lb` | `profx-lb` |
| Virtualnog računala | `servicename-role[number]` | `profx-sql0` |
| Račun za pohranu | `<vmnamenodashes>st<num>` | `profxsql0st0` |

Kako se vidi u dijagramu sljedeće:

![Dijagram Topologija aplikacije] (media/guidance-naming-conventions/guidance-naming-convention-example.png "Topologija aplikacije za uzorak")

## <a name="sample---azure-cli-script-for-deploying-the-sample-above"></a>Primjer – Azure EŽA skripte za implementaciju iznad uzorka

```bash
#!/bin/sh

#####################################################################
# Sample script using the Azure CLI to build out an application 
# demonstrating naming conventions.  
#
# Note; this script is not intended for production deployment, as it does 
# not create availability sets, configure network security rules, etc.
#####################################################################

# Set up variables to build out the naming conventions for deploying
# the cluster  
LOCATION=eastus2
APP_NAME=profx
ENVIRONMENT=prod
USERNAME=testuser
PASSWORD="testpass"

# Set up the tags to associate with items in the application
TAG_BILLTO="InternalApp-ProFX-12345"
TAGS="billTo=${TAG_BILLTO}"

# Explicitly set the subscription to avoid confusion as to which subscription
# is active/default
SUBSCRIPTION=3e9c25fc-55b3-4837-9bba-02b6eb204331

# Set up the names of things using recommended conventions
RESOURCE_GROUP="${APP_NAME}-${ENVIRONMENT}-rg"
VNET_NAME="${APP_NAME}-vnet"

# Set up the postfix variables attached to most CLI commands
POSTFIX="--resource-group ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}"

##########################################################################################
# Set up the VM conventions for Linux and Windows images

# For Windows, get the list of URN's via 
# azure vm image list ${LOCATION} MicrosoftWindowsServer WindowsServer 2012-R2-Datacenter
WINDOWS_BASE_IMAGE=MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20160126

# For Linux, get the list or URN's via 
# azure vm image list ${LOCATION} canonical ubuntuserver
LINUX_BASE_IMAGE=canonical:ubuntuserver:16.04.0-DAILY-LTS:16.04.201602130

#########################################################################################
## Define functions 
create_vm ()
{
    vm_name=$1
    vnet_name=$2
    subnet_name=$3
    os_type=$4
    vhd_path=$5
    vm_size=$6
    diagnostics_storage=$7

    # Create the network interface card for this VM
    azure network nic create --name "${vm_name}-0nic" --subnet-name ${subnet_name} --subnet-vnet-name ${vnet_name} \
        --tags="${TAGS}" ${POSTFIX}

    # Create the storage account for this vm's disks (premium locally redundant storage -> PLRS)
    # Note the ${var//-/} syntax to remove dashes from the vm name
    storage_account_name=${vm_name//-/}st01
    azure storage account create --type=PLRS --tags "${TAGS}" ${POSTFIX} "${storage_account_name}"

    # Map the name of the diagnostics storage account to a blob URI for boot diagnostics
    # This is (currently) required when deploying with a named premium storage account 
    diag_blob="https://${diagnostics_storage}.blob.core.windows.net/"

    # Create the VM
    azure vm create --name ${vm_name} --nic-name "${vm_name}-0nic" --os-type ${os_type} \
        --image-urn ${vhd_path} --vm-size ${vm_size} --vnet-name ${vnet_name} --vnet-subnet-name ${subnet_name} \
        --storage-account-name "${storage_account_name}" --storage-account-container-name vhds --os-disk-vhd "${vm_name}-osdisk.vhd" \
        --admin-username "${USERNAME}" --admin-password "${PASSWORD}" \
        --boot-diagnostics-storage-uri "${diag_blob}" \
        --tags="${TAGS}" ${POSTFIX} 
}

###################################################################################################
# Create resources

# Step 1 - create the enclosing resource group
azure group create --name "${RESOURCE_GROUP}" --location "${LOCATION}" --tags "${TAGS}" --subscription "${SUBSCRIPTION}"

# Step 2 - create the network security groups

# Step 3 - create the networks (VNet and subnets)
azure network vnet create --name "${VNET_NAME}" --address-prefixes="10.0.0.0/8" --tags "${TAGS}" ${POSTFIX}
# TODO - does subnet support tagging?
azure network vnet subnet create --name gateway-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.1.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-master-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.2.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-data-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.3.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name sql-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.4.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}

# Step 4 - define the load balancer and network security rules
azure network lb create --name "${APP_NAME}-lb" ${POSTFIX}
# In a production deployment script, we'd create load balancer rules and 
# network security groups here

# Step 5 - create a diagnostics storage account
diagnostics_storage_account=${APP_NAME//-/}diag
azure storage account create --type=LRS --tags "${TAGS}" ${POSTFIX} "${diagnostics_storage_account}"

# Step 6.1 - Create the gateway VMs
for i in `seq 1 4`;
do
    create_vm "${APP_NAME}-gw${i}" "${APP_NAME}-vnet" "gateway-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}" 
done    

# Step 6.2 - Create the ElasticSearch master and data VMs
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-master${i}" "${APP_NAME}-vnet" "es-master-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-data${i}" "${APP_NAME}-vnet" "es-data-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done

# Step 6.3 - Create the SQL VMs
create_vm "${APP_NAME}-sql0" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
create_vm "${APP_NAME}-sql1" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
```
