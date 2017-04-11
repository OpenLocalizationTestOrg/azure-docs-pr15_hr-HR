<properties
    pageTitle="Prijenos podataka za Hadoop poslove u HDInsight | Microsoft Azure"
    description="Saznajte kako prenijeti i pristup podacima za Hadoop poslove u HDInsight pomoću EŽA Azure, Explorer Azure prostora za pohranu, Azure PowerShell, Hadoop naredbenog retka ili Sqoop."
    services="hdinsight,storage"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>



#<a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Prijenos podataka za Hadoop poslove u HDInsight

Azure HDInsight omogućuje punu Hadoop distributed datotečnog sustava (HDFS) putem spremište blobova platforme Azure. Služi kao datotečni nastavak HDFS radi olakšavanja objedinjenog zadovoljstva korisnika. Omogućuje potpunog skupa komponente u zajednici Hadoop raditi izravno na podacima je upravlja. Azure blobova i HDFS su distinct datotečnih koji su optimizirani za pohranu podataka i computations na tih podataka. Informacije o prednostima korištenja spremište blobova platforme Azure potražite u članku [Korištenje Azure blobova s HDInsight][hdinsight-storage].

**Preduvjeti**

Prije nego što počnete Imajte na umu sljedeće zahtjev:

* Klaster programa Azure HDInsight. Upute potražite u članku [Početak rada sa servisom Azure HDInsight] [ hdinsight-get-started] ili [HDInsight Dodjela resursa za klastere][hdinsight-provision].

##<a name="why-blob-storage"></a>Zašto bloba prostora za pohranu?

Azure HDInsight klastere uvode obično da biste pokrenuli MapReduce zadacima i skupina ispuštaju se nakon što dovršite ove zadatke. Čuvanja podataka u na HDFS klastere nakon što završite computations bi skupi način da biste pohranili podatke. Azure blobova je vrlo dostupna, Visoko prilagodljivi, Visoko kapaciteta, mogućnost niskog troška i djeljiv pohrane podataka koji će se obrađuju pomoću HDInsight. Spremanje podataka u blob omogućuje HDInsight klastere koje se koriste za izračunavanje za sigurno izdati bez gubitka podataka.

###<a name="directories"></a>Direktorija

Azure Blob potpisu pohrane podataka kao parove ključa vrijednosti, a postoji bez hijerarhija direktorija. No znaka "/" može se koristiti u naziv ključa da bi se prikazala kao da je datoteka pohranjena unutar strukturu direktorija. HDInsight vidi ih kao da su stvarne direktorija.

Na primjer, s blob ključ možda *input/log1.txt*. Nema stvarni "unos" direktorijem, ali zbog značajke prisutnosti na "/" znaka u nazivu ključa ima izgled put datoteke.

Zbog toga, ako pomoću alata za Azure Explorer možda primijetiti neke datoteke 0 bajtova. Te datoteke imaju dvije namjene:

- Ako postoje prazne mape, oni označite od postojanje mape. Azure blobova je dovoljno dobra znati ako postoji blob naziva odnožje/traka, ima li u mapu pod nazivom **odnožje**. Dok je jedini način na koji označavaju praznu mapu pod nazivom **odnožje** tako da datoteku posebno 0 bajtova na mjestu.

- Oni držite posebno metapodatke koje je potrebno datotečnom sustavu Hadoop čest dozvole i vlasnike za mape.

##<a name="command-line-utilities"></a>Naredbenog retka uslužni programi

Microsoft pruža sljedeće uslužni programi za rad s spremište blobova platforme Azure:

| Alat za | Linux | OS X | Windows |
| ---- |:-----:|:----:|:-------:|
| [Azure sučelje naredbenog retka][azurecli] | ✔ | ✔ | ✔ |
| [Azure PowerShell][azure-powershell] | | | ✔ |
| [AzCopy][azure-azcopy] | | | ✔ |
| [Naredba Hadoop](#commandline) | ✔ | ✔ | ✔ |

> [AZURE.NOTE] Dok EŽA Azure, Azure PowerShell i AzCopy možete sve će se koristiti s mjesta izvan Azure, Hadoop naredba dostupna samo na klasteru HDInsight i samo omogućuje učitavanja podataka s lokalnom sustavu datoteka u spremište blobova platforme Azure.

###<a id="xplatcli"></a>Azure EŽA

Azure EŽA je alat za različite platforme koja omogućuje upravljanje uslugama za Azure. Da biste prenijeli podatke na spremište blobova platforme Azure, poduzmite sljedeće korake:

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Instalirati i konfigurirati EŽA Azure za Mac i Linux Windows](../xplat-cli-install.md).

2. Otvorite naredbeni redak, tulumu ili druge ljuske i koristiti sljedeće za provjeru autentičnosti u pretplatu za Azure.

        azure login

    Kada se to od vas zatraži, unesite korisničko ime i lozinku za vašu pretplatu.

3. Unesite sljedeću naredbu da biste dobili popis račune za pohranu za vašu pretplatu:

        azure storage account list

4. Odaberite račun za pohranu koji sadrži blob želite raditi, a zatim koristite sljedeću naredbu za dohvaćanje ključ za račun:

        azure storage account keys list <storage-account-name>

    To će se vratiti **primarnih** i **sekundarnih** ključeva. Kopirajte vrijednost **primarnog** ključa jer će se koristiti u sljedećim koracima.

5. Da biste dohvatili popis blob spremnika subjekta za pohranu, koristite sljedeću naredbu:

        azure storage container list -a <storage-account-name> -k <primary-key>

6. Prijenos i preuzimanje datoteka blob-om pomoću sljedeće naredbe:

    * Da biste prenijeli datoteke:

            azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>

    * Da biste preuzeli datoteke:

            azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Ako će uvijek rad s istim računom za pohranu, možete postaviti sljedeće varijable okruženja umjesto navodeći račun i tipku za svaku naredbu:
>
> * **AZURE\_prostora za POHRANU\_RAČUN**: naziv računa spremišta
>
> * **AZURE\_prostora za POHRANU\_PRISTUP\_KLJUČ**: ključ za račun za pohranu

###<a id="powershell"></a>Azure PowerShell

Azure PowerShell je skriptiranje okruženje koje možete koristiti za upravljanje i automatizirati implementacije i upravljanja vaše radnih opterećenja servisu Azure. Informacije o konfiguraciji vašeg radne stanice da biste pokrenuli Azure PowerShell potražite u članku [instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Da biste prenijeli lokalne datoteke na spremište blobova platforme Azure**

1. Otvorite konzolu Azure PowerShell prema odredbama u [instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).
2. Postavljanje vrijednosti prvih pet varijabli u sljedeću skriptu:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext

3. Skripta zalijepiti konzole za Azure PowerShell pokrenite je da biste kopirali datoteku.

Na primjer skripte komponente PowerShell stvorene za rad s HDInsight, potražite u članku [Alati za HDInsight](https://github.com/blackmist/hdinsight-tools).

###<a id="azcopy"></a>AzCopy

AzCopy je alat naredbenog retka koji je dizajniran da biste pojednostavnili zadatak prijenos podataka u i Odjava iz njega račun za Azure prostora za pohranu. Možete koristiti kao alat za samostalno ili uključivanje ovaj alat u postojeću aplikaciju. [Preuzmite AzCopy][azure-azcopy-download].

Sintaksa AzCopy je:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Dodatne informacije potražite u članku [AzCopy - prijenos i Downloading datoteka za Azure blob-ova][azure-azcopy].


###<a id="commandline"></a>Hadoop naredbenog retka

U naredbenom retku Hadoop koristan je samo za spremanje podataka u spremište blobova platforme kada se podaci već se nalazi na čvor glavni klaster.

Da biste koristili naredbu Hadoop, najprije morate povezati s headnode na jedan od sljedećih načina:

* **HDInsight utemeljen na sustavu Windows**: [Povezivanje putem udaljene radne površine](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)

* **Sustavom Linux HDInsight**: povezati pomoću SSH ([naredbu SSH](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster) ili [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster))

Nakon uspostave sljedeću sintaksu možete koristiti da biste prenijeli datoteke na prostora za pohranu.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Na primjer,`hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Budući da je zadani datotečni sustav za HDInsight u spremište blobova platforme Azure, /example/data.txt zapravo je u spremište blobova platforme Azure. Možete se referirati na datoteku kao:

    wasbs:///example/data/data.txt

ili

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Popis ostale Hadoop naredbe koje funkcioniraju s datotekama, potražite u članku [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [AZURE.WARNING] Na klastere HBase zadani blokirati veličinu kada je zapisivanje podataka 256KB. Dok se to radi precizno prilikom korištenja API-ji HBase ili REST API-ji, pomoću na `hadoop` ili `hdfs dfs` naredbi za zapisivanje podataka veća od ~ 12GB rezultira pogreškom. U odjeljku [iznimke za pohranu za pisanje na blob](#storageexception) ispod dodatne informacije.

##<a name="graphical-clients"></a>Grafički klijenti

Postoji nekoliko aplikacije koje sadrže grafičkog sučelja za rad s Azure prostora za pohranu. Slijedi popis nekoliko te aplikacije:

| Klijent | Linux | OS X | Windows |
| ------ |:-----:|:----:|:-------:|
| [Microsoft Visual Studio Tools za HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) | ✔ | ✔ | ✔ |
| [Explorer Azure prostora za pohranu](http://storageexplorer.com/) | ✔ | ✔ | ✔ |
| [Oblak prostora za pohranu Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | | ✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | | ✔ |
| [Azure Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | | ✔ |
| [Cyberduck](https://cyberduck.io/) |  | ✔ | ✔ |

###<a name="visual-studio-tools-for-hdinsight"></a>Visual Studio Tools for HDInsight

Dodatne informacije potražite u članku [Pronađite povezane resurse](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

###<a id="storageexplorer"></a>Explorer Azure prostora za pohranu

*Azure prostora za pohranu* je koristan alat za pregled i mijenjaju podatke u BLOB-ova. Je alat za slobodni, Otvori izvor koji se može preuzeti iz [http://storageexplorer.com/](http://storageexplorer.com/). Izvorni kod dostupna tu vezu.

Prije korištenja alata, morate znati Azure prostora za pohranu imenom i računom ključ za račun. Upute o tome kako izvući te informacije potražite u članku na "Kako: prikaz, Kopiraj i regenerate pohranu pristupne ključeve" odjeljak [Stvaranje, upravljanje, i brisanje računa za pohranu][azure-create-storage-account].  

1. Pokrenite Explorer Azure prostora za pohranu. Ako je ovo prvi put ste pokrenuli Explorer prostora za pohranu, zatražit će se ___naziv računa za pohranu__ i __ključ za račun za pohranu__. Ako imate pokrenuli ga prije korištenja gumba __Dodaj__ da biste dodali novi naziv računa za pohranu i ključa.

    Unesite naziv, a ključ za račun za pohranu koristi svoj klaster HDinsight, a zatim odaberite __SPREMANJE i otvaranje__.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]

5. Na popisu spremnika s lijeve strane sučelje kliknite naziv spremnika koji je pridružen svoj klaster HDInsight. Prema zadanim postavkama, to je naziv HDInsight klaster, ali mogu se razlikovati ako unesete naziv prilikom stvaranja klaster.

6. Na alatnoj traci odaberite ikona prijenos.

    ![Traka s istaknutom ikonom za prijenos](./media/hdinsight-upload-data/toolbar.png)

7. Određivanje datoteke za prijenos, a zatim kliknite **Otvori**. Kada se to od vas zatraži, odaberite __Prenesi__ na prijenos datoteke u korijenskom direktoriju spremnik za pohranu. Ako želite da biste prenijeli datoteke na određenom putu, unesite put u __odredišno__ polje, a zatim odaberite __Prenesi__.

    ![Dijaloški okvir prijenos datoteke](./media/hdinsight-upload-data/fileupload.png)
    
    Kada datoteku dovršetka prijenosa, možete ga koristiti iz poslova na klasteru HDInsight.

##<a name="mount-azure-blob-storage-as-local-drive"></a>Spremište blobova platforme Azure postavljanja kao lokalni pogon

[Spremište blobova platforme Azure postavljanja kao lokalni pogon](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx)potražite u članku.

##<a name="services"></a>Usluge

###<a name="azure-data-factory"></a>Tvorničke Azure podataka

Servisa Azure podataka tvorničke je potpuno upravljanih servis za sastavljanje prostora za pohranu, obradu podataka i podataka premještanja usluge podataka u pojednostavnjeno, skalabilni i pouzdan podataka radnog kanali.

Azure tvorničke podataka mogu se da biste premjestili podatke u spremište blobova platforme Azure ili da biste stvorili kanali podataka koje izravno koristiti HDInsight značajkama kao što su grozd Svinja.

Dodatne informacije potražite u [dokumentaciji tvorničke Azure podataka](https://azure.microsoft.com/documentation/services/data-factory/).

###<a id="sqoop"></a>Apache Sqoop

Sqoop je alat koji je namijenjen za prijenos podataka između Hadoop i relacijske baze podataka. Možete je koristiti da biste uvezli podatke iz relacijske baze podataka sustava upravljanja (RDBMS), kao što su SQL Server, MySQL ili Oracle u datotečnom sustavu Hadoop distributed (HDFS), pretvaranje podataka u Hadoop MapReduce ili grozd i zatim izvezite podatke natrag na RDBMS.

Dodatne informacije potražite u članku [Korištenje Sqoop s HDInsight][hdinsight-use-sqoop].

##<a name="development-sdks"></a>Razvoj SDK-ovi

Azure blobova može se pristupiti i pomoću Azure SDK-a iz sljedećih programskog jezika:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Dodatne informacije o instaliranju Azure SDK-ovi potražite u članku [Preuzimanje Azure](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Otklanjanje poteškoća

### <a id="storageexception"></a>Iznimke prostora za pohranu za pisanje na blobova platforme

__Simptomi__: kada se koristi u `hadoop` ili `hdfs dfs` naredbi za pisanje datoteke koje su GB ~ 12 ili veća na klasteru programa HBase koji se mogu pojaviti sljedeća pogreška: 

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

__Uzrok__: HBase na HDInsight klaster zadanu veličinu bloka 256 KB prilikom pisanja Azure prostora za pohranu. Dok se to radi HBase API-ji ili REST API-ji, ona će rezultiraju pogreškom prilikom korištenja u `hadoop` ili `hdfs dfs` naredbenog retka uslužni programi.

__Razlučivost__: korištenje `fs.azure.write.request.size` da biste odredili veću veličinu bloka. To možete učiniti na temelju po korištenju pomoću na `-D` parametar. Slijedi primjer pomoću taj parametar s na `hadoop` naredba:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Možete povećati i vrijednost `fs.azure.write.request.size` globalno pomoću Ambari. Da biste promijenili vrijednost u korisničkom Sučelju Ambari Web mogu se na sljedeći način:

1. U pregledniku idite na korisničko Sučelje Web Ambari za svoj klaster. Ovo je https://CLUSTERNAME.azurehdinsight.net, pri čemu je __CLUSTERNAME__ svoj klaster.

    Kada se to od vas zatraži, unesite administratore ime i lozinku za klaster.

2. Na lijevoj strani zaslona odaberite __HDFS__, a zatim odaberite karticu __Configs__ .

3. U polje __filtra...__ unesite `fs.azure.write.request.size`. Time će se prikazati polje i trenutnu vrijednost sredini stranice.

4. Nove vrijednosti promijenite vrijednost iz 262144 (256KB). Na primjer, 4194304 (4MB).

![Promijenite vrijednost putem korisničkog Sučelja Web Ambari slika](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Dodatne informacije o korištenju Ambari potražite u članku [Upravljanje HDInsight klastere pomoću korisničkog Sučelja Web Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Daljnji koraci

Sada kada razumijete upute za preuzimanje podataka u HDInsight, pročitajte u sljedećim člancima da biste saznali kako tema:

* [Početak rada sa servisom Azure HDInsight][hdinsight-get-started]
* [Programski slanje Hadoop poslove][hdinsight-submit-jobs]
* [Korištenje grozd s HDInsight][hdinsight-use-hive]
* [Korištenje Svinja s HDInsight][hdinsight-use-pig]




[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: ../powershell-install-configure.md

[azurecli]: ../xplat-cli-install.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
