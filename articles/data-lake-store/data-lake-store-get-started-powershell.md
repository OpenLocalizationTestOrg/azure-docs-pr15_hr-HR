<properties
   pageTitle="Početak rada s spremišta podataka Lake | Azure"
   description="Pomoću komponente PowerShell Azure stvaranje spremišta podataka Lake račun i izvođenje osnovnih operacija"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/04/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Početak rada s spremišta Lake podataka za Azure pomoću komponente PowerShell Azure

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-JA](data-lake-store-get-started-rest-api.md)
- [Azure EŽA](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Saznajte kako pomoću Azure PowerShell možete stvarati Lake spremišta podataka za Azure račun i izvođenje osnovnih operacija kao što su prilikom stvaranja mape, prijenos i preuzimanje podatkovne datoteke, izbrišite račun, itd. Dodatne informacije o spremištu Lake podataka potražite u članku [Pregled od Lake spremišta podataka](data-lake-store-overview.md).

## <a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

* **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

* **Azure PowerShell 1.0 ili noviji**. Saznajte [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

## <a name="authentication"></a>Provjera autentičnosti

U ovom se članku koristi jednostavniji način provjere autentičnosti s trgovinom Lake podataka u kojoj se od vas zatraži unesite vjerodajnice za račun za Azure. Razinu pristupa podacima Lake spremište razinu pristupa prijavljenog korisnika pa uređena račun i datoteke sustava. No postoje drugi pristupa kao i za provjeru s spremišta Lake podataka, koje su **provjere autentičnosti za krajnjeg korisnika** ili **provjere autentičnosti servis za servis**. Upute i dodatne informacije o provjeri autentičnosti potražite u članku [provjere autentičnosti s trgovinom Lake podatke pomoću servisa Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Stvaranje računa spremišta Lake podataka za Azure

1. Na radnoj površini otvaranje novog prozora komponente Windows PowerShell pa unesite sljedeće isječak prijavite se na račun za Azure, postavite pretplate i registrirati davatelja spremišta Lake podataka. Kada se to od vas zatraži da biste se prijavili, provjerite je li se prijaviti kao jedan od admininistrators/vlasnik pretplate:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"


2. Račun za Azure podataka Lake spremište povezan je s grupu resursa Azure. Započnite tako da stvorite grupu resursa Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Stvaranje grupe sustava Azure resursa] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Stvaranje grupe sustava Azure resursa")

2. Stvorite račun za Azure podataka Lake trgovine. Naziv navedete mora sadržavati samo mala slova i brojeva.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Stvaranje računa spremišta Lake podataka za Azure] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Stvaranje računa spremišta Lake podataka za Azure")

3. Provjerite je li račun uspješno stvorili.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Izlaz za to mora biti **True**.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Stvaranje strukture direktorija u spremištu Lake Azure podataka

Direktorija možete stvoriti na vašem računu Lake spremišta podataka za Azure za upravljanje i spremanje podataka.

1. Navedite korijenski direktorij.

        $myrootdir = "/"

2. Stvorite novi pod nazivom **mynewdirectory** u odjeljku navedeni korijenski direktorij.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Provjerite je li novi direktorij uspješno stvorili.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Ga treba Prikaži na Izlaz kao što je sljedeća:

    ![Provjerite je li direktorija] (./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Provjerite je li direktorija")


## <a name="upload-data-to-your-azure-data-lake-store"></a>Prijenos podataka u spremište Lake Azure podataka

Podatke možete prenijeti spremišta podataka Lake izravno na korijenskoj razini ili direktorija koji ste stvorili u račun. Isječci u nastavku sadrži upute za prijenos oglednih podataka u imeniku (**mynewdirectory**) koju ste stvorili u prethodnom odjeljku.

Ako tražite oglednih podataka za prijenos **Podataka Ambulance** mape možete pristupiti iz [Azure podataka Lake brojka spremište](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Preuzmite datoteku i spremite ga na lokalni direktorij na vašem računalu, kao što su C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Preimenovanje, preuzimanje i brisanje podataka iz spremišta Lake podataka

Da biste preimenovali datoteku, koristite sljedeću naredbu:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Da biste preuzeli datoteke, koristite sljedeću naredbu:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Da biste izbrisali datoteku, koristite sljedeću naredbu:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Kada se to od vas zatraži, unesite **Y** da biste izbrisali stavke. Ako imate više od jedne datoteke da biste izbrisali, možete unijeti svi putovi odvojenih zarezom.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Brisanje računa spremišta Lake podataka za Azure

Koristite sljedeću naredbu da biste izbrisali računa spremišta Lake podataka.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Kada se to od vas zatraži, unesite **Y** da biste izbrisali račun.


## <a name="next-steps"></a>Daljnji koraci

- [Zaštite podataka u spremištu Lake podataka](data-lake-store-secure-data.md)
- [Korištenje analize Lake Azure podataka s spremišta Lake podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight pomoću spremišta Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)
