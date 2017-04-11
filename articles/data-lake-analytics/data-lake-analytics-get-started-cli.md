<properties 
   pageTitle="Početak rada s Azure podataka Lake analize korištenja sučelja naredbenog retka za Azure | Microsoft Azure" 
   description="Saznajte kako koristiti Azure sučelja naredbenog retka za stvaranje računa spremišta Lake podataka, stvorite posao analize Lake podataka koji se koristi U SQL i slanje posao. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Praktični vodič: početak rada s Azure podataka Lake analize pomoću Azure sučelje naredbenog retka (EŽA)

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Saznajte kako koristiti Azure EŽA za stvaranje računa za Azure podataka Lake analize, definirati analize podataka Lake zadacima [U SQL](data-lake-analytics-u-sql-get-started.md)pa pošaljite poslove analize podataka Lake računima. Dodatne informacije o analize Lake podataka potražite u članku [Pregled Azure podataka Lake analize](data-lake-analytics-overview.md).

U ovom ćete praktičnom vodiču će razviti zadatak koji se čita karticu datoteku (TSV) vrijednosti odvojenih i pretvara ga u datoteku razdvojene zarezom (CSV). Da biste došli do isti praktičnom vodiču pomoću drugih alata za podržane, klikom na kartice pri vrhu stranice u ovom se odjeljku.

##<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
- **Azure EŽA**. U odjeljku [instalirati i konfigurirati Azure EŽA](../xplat-cli-install.md).
    - Preuzmite i instalirajte **predizdanje** [Alati za Azure EŽA](https://github.com/MicrosoftBigData/AzureDataLake/releases) kako bi obavili ovaj pokazni videozapis.
- **Provjera autentičnosti**, pomoću naredbe za sljedeće:

        azure login
    Dodatne informacije o provjera autentičnosti pomoću računa tvrtke ili obrazovne ustanove, potražite u članku [Povezivanje Azure pretplatu s EŽA Azure](../xplat-cli-connect.md).
- **Da biste prešli na način upravljanja resursima Azure**pomoću sljedeće naredbe:

        azure config mode arm
        
## <a name="create-data-lake-analytics-account"></a>Stvaranje računa analize podataka Lake

Morate imati račun analize podataka Lake prije pokretanja sve zadatke. Da biste stvorili analize podataka Lake račun, morate navesti sljedeće:

- **Grupa resursa Azure**: A podataka Lake analize računa moraju se stvoriti u skupini Azure resursa. [Voditelj resursa Azure](../azure-resource-manager/resource-group-overview.md) omogućuje rad s resursima u aplikaciji kao grupu. Možete implementirati, ažuriranje i brisanje svi resursi aplikacije u jednom, usklađenih postupak.  

    Da biste numeriranje grupa resursa u svoju pretplatu:
    
        azure group list 
    
    Da biste stvorili novu grupu resursa:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Naziv računa analize podataka Lake**
- **Lokacija**: jedan od središta Azure podataka koje podržava analize podataka Lake.
- **Zadani podataka Lake računa**: svaki račun analize podataka Lake ima zadani račun Lake podataka.

    Da biste dobili popis postojeći račun Lake podataka:
    
        azure datalake store account list

    Da biste stvorili novi račun Lake podataka:

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] Naziv računa Lake podataka mora sadržavati samo mala slova i brojeva.



**Da biste stvorili analize podataka Lake račun**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Analize podataka Lake prikaz računa](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] Naziv računa analize podataka Lake mora sadržavati samo mala slova i brojeva.


## <a name="upload-data-to-data-lake-store"></a>Prijenos podataka u trgovinu Lake podataka

U ovom ćete praktičnom vodiču obradit će nekim zapisnicima pretraživanja.  U zapisniku pretraživanja može se spremiti u spremište podataka Lake ili spremište blobova platforme Azure. 

Portal za Azure predstavlja korisničko sučelje za kopiranje neke ogledne podatkovne datoteke na zadani račun Lake podataka, što obuhvaća datoteku zapisnika pretraživanja. Potražite u članku [Priprema izvora podataka](data-lake-analytics-get-started-portal.md#prepare-source-data) da biste prenijeli podatke na zadani račun za spremište Lake podataka.

Da biste prenijeli datoteke pomoću eža, koristite sljedeću naredbu:

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Spremište blobova platforme Azure možete pristupiti i analize podataka Lake.  Za prijenos podataka u spremište blobova platforme Azure, pogledajte odjeljak [Korištenje EŽA Azure s Azure prostora za pohranu](../storage/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Slanje podataka Lake Analitika poslova

Poslovi analize podataka Lake zapisuju U SQL jeziku. Dodatne informacije o U SQL potražite u članku [Uvod U SQL jezika](data-lake-analytics-u-sql-get-started.md) i [U SQL jezične preporuke](http://go.microsoft.com/fwlink/?LinkId=691348).

**Da biste stvorili skriptu analize podataka Lake posla**

- Stvaranje tekstne datoteke pomoću sljedeće skripte U SQL i spremite tekstnu datoteku vaše radne stanice:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Ova skripta U SQL čita izvornu datoteku podataka pomoću **Extractors.Tsv()**i stvara u csv datoteku pomoću **Outputters.Csv()**. 
    
    Nemojte mijenjati dva puta osim ako kopirate izvornu datoteku na drugo mjesto.  Ako ne postoji analize podataka Lake će stvoriti Izlazna datoteka.
    
    To je jednostavnije koristiti relativni putovi za datoteke spremljene u zadanom podataka Lake računi. Možete koristiti i apsolutni putovi.  Na primjer 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Koristite apsolutne putova da biste pristupili datotekama u povezane poslovne subjekte prostora za pohranu.  Vidjet ćete da sintaksa za datoteke spremljene u povezani poslovni subjekt za pohranu Azure je:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob kontejner s javnim blob-ova ili dozvole za pristup javnim spremnika trenutno nisu podržani.      

    
**Da biste poslali posao**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
Popis zadataka, dohvaćanje detalja za posao i otkazati poslove mogu se sljedeće naredbe:

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

Nakon završetka posla, koristite sljedeće Cmdlete da biste datoteke i preuzimanje datoteke:
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## <a name="see-also"></a>Vidi također

- Da biste vidjeli iste praktičnom vodiču pomoću drugih alata, kliknite karticu Birači pri vrhu stranice.
- Da biste vidjeli složeniji upit, potražite u članku [zapisnika analiza web-mjesta pomoću Azure podataka Lake analize](data-lake-analytics-analyze-weblogs.md).
- Prvi koraci u razvoju aplikacija U SQL, potražite u članku [razviti U – SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Da biste saznali U SQL, potražite u članku [Početak rada s jezikom Azure podataka Lake analize U-SQL](data-lake-analytics-u-sql-get-started.md).
- Upravljanje zadacima, potražite u članku [Upravljanje Azure podataka Lake analize pomoću portala za Azure](data-lake-analytics-manage-use-portal.md).
- Da biste dobili pregled analize podataka Lake, potražite u članku [Pregled Azure podataka Lake analize](data-lake-analytics-overview.md).

