<properties
   pageTitle="Početak rada s spremišta Lake podataka pomoću sučelja za različite platforme naredbeni redak | Microsoft Azure"
   description="Stvaranje spremišta podataka Lake račun i izvođenje osnovnih operacija pomoću Azure različite platforme naredbenog retka"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Početak rada s spremišta Lake podataka za Azure pomoću Azure naredbenog retka

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-JA](data-lake-store-get-started-rest-api.md)
- [Azure EŽA](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Saznajte kako koristiti Azure sučelje naredbenog retka spremno za stvaranje Lake spremišta podataka za Azure račun i izvođenje osnovnih operacija kao što su prilikom stvaranja mape, prijenos i preuzimanje podatkovne datoteke, izbrišite račun, itd. Dodatne informacije o spremištu Lake podataka potražite u članku [Pregled od Lake spremišta podataka](data-lake-store-overview.md).

Azure EŽA je implementirana u Node.js. Može se koristiti na bilo kojoj platformi koji podržava Node.js, uključujući Windows, Mac i Linux. Azure EŽA je Otvori izvor. Izvorni kod upravlja se GitHub na <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>. U ovom se članku opisuje samo EŽA Azure pomoću spremišta Lake podataka. Opće smjernice o korištenju Azure EŽA potražite [u]članku korištenje EŽA Azure [azure-command-line-tools].


##<a name="prerequisites"></a>Preduvjeti

Prije nego počnete u ovom se članku, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

- **Azure EŽA** - potražite u članku [instalirati i konfigurirati EŽA Azure](../xplat-cli-install.md) instalacije i konfiguracije informacije. Provjerite je li ponovno pokrenite računalo nakon što instalirate na EŽA.

## <a name="authentication"></a>Provjera autentičnosti

U ovom se članku koristi jednostavniji način provjere autentičnosti pomoću podataka iz trgovine Lake gdje se prijaviti kao korisnik krajnjeg korisnika. Razinu pristupa podacima Lake spremište razinu pristupa prijavljenog korisnika pa uređena račun i datoteke sustava. No postoje drugi pristupa kao i za provjeru s spremišta Lake podataka, koje su **provjere autentičnosti za krajnjeg korisnika** ili **provjere autentičnosti servis za servis**. Upute i dodatne informacije o provjeri autentičnosti potražite u članku [provjere autentičnosti s trgovinom Lake podatke pomoću servisa Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

##<a name="login-to-your-azure-subscription"></a>Prijava u pretplatu za Azure

1. Slijedite korake u [pretplatu na Azure s Azure sučelja naredbenog retka (Azure EŽA) za povezivanje](../xplat-cli-connect.md) i povezivanje s pretplate pomoću na `azure login` način.

2. Popis pretplata na koje su vezane uz račun pomoću na `azure account list` naredbe.

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    Iz izlaza iznad **Azure sub 1** trenutno aktivna i druge pretplate je **Azure sub 2**. 

3. Odaberite pretplatu u kojima želite raditi u odjeljku. Ako želite raditi u odjeljku pretplate Azure sub 2, koristite na `azure account set` naredbe.

        azure account set Azure-sub-2


## <a name="create-an-azure-data-lake-store-account"></a>Stvaranje računa spremišta Lake podataka za Azure

Otvorite naredbeni redak, ljuske ili sesije terminal i pokrenite sljedeće naredbe.

2. Prebacite se u način upravljanja resursima Azure pomoću sljedeće naredbe:

        azure config mode arm


5. Stvorite novu grupu resursa. U sljedeću naredbu, navedite vrijednosti parametara koji želite koristiti.

        azure group create <resourceGroup> <location>

    Ako naziv lokacije sadrži razmake, stavite ga unutar navodnika. Na primjer "Istočni sad 2".

5. Stvaranje računa spremišta Lake podataka.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Stvaranje mapa u spremištu Lake podataka

U odjeljku računa spremišta Lake za Azure podataka za upravljanje i spremanje podataka možete stvoriti mape. Koristite sljedeću naredbu da biste stvorili mapu pod nazivom "mynewfolder" u korijenu Lake pohrane podataka.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Ako, na primjer:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Prijenos podataka Lake spremište podataka

Podatke možete prenijeti spremišta podataka Lake izravno na korijenskoj razini ili u mapu koju ste stvorili u račun. Ispod isječci Demonstracija prenesite ogledne podatke u mapu (**mynewfolder**) koju ste stvorili u prethodnom odjeljku.

Ako tražite oglednih podataka za prijenos **Podataka Ambulance** mape možete pristupiti iz [Azure podataka Lake brojka spremište](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Preuzmite datoteku i spremite ga na lokalni direktorij na vašem računalu, kao što su C:\sampledata\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Ako, na primjer:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Popis datoteka u spremištu Lake podataka

Koristite sljedeću naredbu na popisu datoteka u spremištu Lake podataka računa.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Ako, na primjer:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

Izlaz iz to mora biti sličnu ovoj:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Preimenovanje, preuzimanje i brisanje podataka iz spremišta Lake podataka

* **Da biste preimenovali datoteku**, koristite sljedeću naredbu:

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    Ako, na primjer:

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **Da biste preuzeli datoteke**, koristite sljedeću naredbu. Provjerite je li odredište put navedete već postoji.

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    Ako, na primjer:

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **Da biste izbrisali datoteku**, koristite sljedeću naredbu:

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    Ako, na primjer:

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    Kada se to od vas zatraži, unesite **Y** da biste izbrisali stavke.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>Prikaz popis za kontrolu pristupa za mapu u spremištu Lake podataka

Koristite sljedeću naredbu da biste pogledali ACL-a za pohranu podataka Lake mapu. U trenutnom se izdanju ACL-a možete postaviti samo na korijenu Lake pohrane podataka. Tako, parametar put uvijek biti korijenski (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Ako, na primjer:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Brisanje računa spremišta Lake podataka

Koristite sljedeću naredbu da biste izbrisali spremišta podataka Lake račun.

    azure datalake store account delete <dataLakeStoreAccountName>

Ako, na primjer:

    azure datalake store account delete mynewdatalakestore

Kada se to od vas zatraži, unesite **Y** da biste izbrisali račun.


## <a name="next-steps"></a>Daljnji koraci

- [Zaštite podataka u spremištu Lake podataka](data-lake-store-secure-data.md)
- [Korištenje analize Lake Azure podataka s spremišta Lake podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight pomoću spremišta Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md
