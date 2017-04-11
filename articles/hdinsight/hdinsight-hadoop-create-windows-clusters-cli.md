<properties
   pageTitle="Stvaranje klastere utemeljen na sustavu Windows Hadoop u HDInsight pomoću EŽA Azure"
    description="Saznajte kako stvoriti klastere za Azure HDInsight pomoću Azure EŽA."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>Stvaranje klastere utemeljen na sustavu Windows Hadoop u HDInsight pomoću EŽA Azure

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Saznajte kako stvoriti klastere HDInsight pomoću Azure EŽA. Radi stvaranja druge klaster alata i značajki kliknite Odaberi kartica pri vrhu stranice ovu stranicu ili pogledajte [načina stvaranja klaster](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Preduvjeti:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Prije nego počnete upute u ovom članku, morate imati sljedeće:

- **Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure EŽA**.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>Preduvjeti za kontrolu pristupa

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="connect-to-azure"></a>Povezivanje s Azure

Povezivanje s Azure, koristite sljedeću naredbu:

    azure login

Dodatne informacije o provjera autentičnosti pomoću računa tvrtke ili obrazovne ustanove, potražite u članku [Povezivanje Azure pretplatu s EŽA Azure](../xplat-cli-connect.md).

Da biste prešli u način rada OBLAK, koristite sljedeću naredbu:

    azure config mode arm

Da biste dobili pomoć, koristite parametar **-h** .  Ako, na primjer:

    azure hdinsight cluster create -h

##<a name="create-clusters"></a>Stvaranje klastere

Da biste mogli stvarati programa HDInsight klaster morate imati Azure upravljanja resursa (ARM) i račun za spremište blobova platforme Azure. Da biste stvorili programa HDInsight klaster, morate navesti sljedeće:

- **Grupa resursa Azure**: A podataka Lake analize računa moraju se stvoriti u skupini Azure resursa. Azure Voditelj resursa omogućuje rad s resursima u aplikaciji kao grupu. Možete implementirati, ažuriranje i brisanje svi resursi aplikacije u jednom, usklađenih postupak.

    Da biste dobili popis grupa resursa u svoju pretplatu:

        azure group list

    Da biste stvorili novu grupu resursa:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Naziv HDInsight klaster**

- **Lokacija**: jedan od središta Azure podataka koje podržava klastere HDInsight. Popis podržanih mjesta, potražite u članku [HDInsight cijene](https://azure.microsoft.com/pricing/details/hdinsight/).

- **Zadani prostor za pohranu račun**: HDInsight koristi programa spremnik spremište blobova platforme Azure kao zadani datotečni sustav. Da biste mogli stvarati programa HDInsight klaster, potreban je račun za Azure prostora za pohranu.

    Da biste stvorili novi račun za Azure prostora za pohranu:

        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE]Račun za pohranu mora biti collocated s HDInsight u centru za podatke.
    > Vrsta računa za pohranu nije moguće ZRS, jer ZRS ne podržava tablice.

    Informacije o stvaranju račun za pohranu Azure pomoću portala za Azure potražite u članku [stvaranje, upravljanje i brisanje računa za pohranu] [azure – stvaranje-storageaccount].

    Ako već imate račun za pohranu, ali ne znate naziv računa i ključ računa, dohvatiti podatke možete koristiti sljedeće naredbe:

        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    Pojedinosti o dohvaćanje podataka pomoću portala za Azure, potražite u odjeljku "Upravljati računom za pohranu" [o Azure prostora za pohranu](../storage/storage-create-storage-account#manage-your-storage-account)računa.

- **Spremnik Blob zadani (neobavezno)**: naredba **Stvori azure hdinsight klaster** stvara spremnik ako ne postoji. Ako odaberete da prije toga stvaranje spremnika, koristite sljedeću naredbu:

    Stvaranje spremnika Azure prostora za pohranu – naziv računa "<Storage Account Name>" – ključ za račun <Storage Account Key> [naziv spremnika]

Nakon što dodate račun za pohranu pripremljeni, spremni ste za stvaranje klaster:


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##<a name="create-clusters-using-configuration-files"></a>Stvaranje klastere pomoću konfiguracijske datoteke
Obično vam stvaranje programa HDInsight klaster, pokrenuti zadatke, a zatim izbrišite klaster da biste izrezali dolje trošak. Sučelja naredbenog retka nudi mogućnost spremanja konfiguracije u datoteku, tako da ga možete koristiti prilikom svakog stvaranja klaster.  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

Primjer: Stvaranje konfiguracijska datoteka koja sadrži akciju skriptu da biste pokrenuli prilikom stvaranja klaster.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##<a name="create-clusters-with-script-action"></a>Stvaranje klastere s akcijom skripte

Stvaranje konfiguracijska datoteka koja sadrži akciju skriptu da biste pokrenuli prilikom stvaranja klaster.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Stvaranje klaster skripte akcija

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Općenito skripte akcija informacije potražite u članku [Prilagodba HDInsight klastere pomoću skripte Akcije (Linux)](hdinsight-hadoop-customize-cluster.md).


## <a name="create-clusters-using-arm-templates"></a>Stvaranje klastere pomoću OKVIRA predložaka

EŽA možete koristiti da biste stvorili klastere tako da nazovete ARM predložaka. Potražite u članku [Implementacija s Azure EŽA](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## <a name="see-also"></a>Vidi također

- [Početak rada sa servisom Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) – upute za početak rada s svoj klaster HDInsight
- [Slanje Hadoop poslovi programski](hdinsight-submit-hadoop-jobs-programmatically.md) – Saznajte kako programski slanje zadataka HDInsight
- [Upravljanje Hadoop klastere u HDInsight pomoću EŽA Azure](hdinsight-administer-use-command-line.md)
- [Pomoću Azure EŽA za Mac i Linux Windows i upravljanje servisom Azure](../virtual-machines-command-line-tools.md)
