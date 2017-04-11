<properties
    pageTitle="Upravljanje Hadoop klastere pomoću Azure EŽA | Microsoft Azure"
    description="Kako koristiti EŽA Azure upravljanja klastere Hadoop u HDIsight"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Upravljanje Hadoop klastere u HDInsight pomoću EŽA Azure

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Saznajte kako koristiti [Azure sučelje naredbenog retka](../xplat-cli-install.md) za upravljanje Hadoop klastere u Azure HDInsight. Azure EŽA je implementirana u Node.js. Može se koristiti na bilo kojoj platformi koji podržava Node.js, uključujući Windows, Mac i Linux.

U ovom se članku opisuje samo pomoću EŽA Azure HDInsight. Opće smjernice o korištenju Azure EŽA potražite u članku [instalirati i konfigurirati Azure EŽA][azure-command-line-tools].

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="prerequisites"></a>Preduvjeti

Prije nego počnete u ovom se članku, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure EŽA** - potražite u članku [instalirati i konfigurirati EŽA Azure](../xplat-cli-install.md) instalacije i konfiguracije informacije.
- **Povezivanje s Azure**, pomoću naredbe za sljedeće:

        azure login

    Dodatne informacije o provjera autentičnosti pomoću računa tvrtke ili obrazovne ustanove, potražite u članku [Povezivanje Azure pretplatu s EŽA Azure](xplat-cli-connect.md).
    
- **Da biste prešli na način upravljanja resursima Azure**pomoću sljedeće naredbe:

        azure config mode arm

Da biste dobili pomoć, koristite parametar **-h** .  Ako, na primjer:

    azure hdinsight cluster create -h
    
##<a name="create-clusters"></a>Stvaranje klastere

Potražite u članku [Stvaranje Linux sustavom klastere u HDInsight pomoću EŽA Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

##<a name="list-and-show-cluster-details"></a>Popis i prikaz pojedinosti klaster
Na popisu, a zatim Prikaži detalje klaster, koristite sljedeće naredbe:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI. CLIListCluster][image-cli-clusterlisting]


##<a name="delete-clusters"></a>Brisanje klastere

Da biste izbrisali klaster, koristite sljedeću naredbu:

    azure hdinsight cluster delete <Cluster Name>

Klaster možete izbrisati i tako da izbrišete grupu resursa koji sadrži klaster. Uzmite u obzir, izbrisat će se svi resursi u grupi, uključujući zadani račun za pohranu.

    azure group delete <Resource Group Name>

##<a name="scale-clusters"></a>Promjena veličine klastere

Da biste promijenili veličinu klaster Hadoop:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>Omogućiti ili onemogućiti pristup HTTP klaster

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Omogućiti ili onemogućiti pristup RDP klaster

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##<a name="next-steps"></a>Daljnji koraci
U ovom se članku ste naučili kako obaviti različite HDInsight klaster administrativne zadatke. Dodatne informacije potražite u sljedećim člancima:

* [Administriranje HDInsight pomoću portala za Azure] [hdinsight-admin-portal]
* [Administriranje HDInsight pomoću komponente PowerShell Azure] [hdinsight-admin-powershell]
* [Početak rada sa servisom Azure HDInsight] [hdinsight-get-started]
* [Kako koristiti EŽA Azure] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "Popis i prikaz klastere"
