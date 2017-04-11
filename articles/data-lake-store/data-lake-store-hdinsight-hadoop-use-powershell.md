<properties
   pageTitle="Stvaranje HDInsight klastere s trgovinom Lake podataka za Azure pomoću komponente PowerShell | Azure"
   description="Pomoću komponente PowerShell Azure stvaranje i korištenje klastere HDInsight s Lake Azure podataka"
   services="data-lake-store,hdinsight" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="nitinme"/>

# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-powershell"></a>Stvaranje programa HDInsight klaster s spremišta Lake podataka pomoću komponente PowerShell Azure

> [AZURE.SELECTOR]
- [Pomoću portala](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Pomoću komponente PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)
- [Pomoću upravitelja resursa](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Saznajte kako pomoću ljuske PowerShell za Azure da biste konfigurirali klaster HDInsight (Hadoop, HBase ili oluja) s pristupom Lake spremišta podataka za Azure. U ovom izdanju neke važne stavke:

* **Spark za klastere (Linux) i Hadoop/oluja klastere (Windows i Linux)**, Lake spremišta podataka mogu se koristiti samo kao račun dodatnog prostora za pohranu. Zadani račun za pohranu za pretraživanje kao skupina će i dalje biti Azure blob polja za pohranu (WASB).

* **HBase za klastere (Windows i Linux)**, Lake spremišta podataka može se koristiti kao zadani prostor za pohranu ili dodatni prostor za pohranu.

> [AZURE.NOTE] Napomena neke važne točke. 
> 
> * Mogućnost stvaranja HDInsight klastere s pristupom spremišta podataka Lake je dostupan samo za HDInsight verzije 3,2 3.4 (za klastere Hadoop, HBase i oluja sustava Windows, kao i Linux). Za klastere Spark na Linux, ova mogućnost dostupna je samo na klastere HDInsight 3.4.
>
> * Kao što je rečeno iznad spremišta Lake podataka dostupna je kao zadani prostor za pohranu za neke vrste klaster (HBase) i dodatni prostor za pohranu za ostale vrste klaster (Hadoop, Spark, oluja). Korištenje spremišta podataka Lake kao račun dodatnog prostora za pohranu ne utječu na performanse ili omogućuje čitanje/pisanje za pohranu iz skupine. U scenariju kao dodatni prostor za pohranu na kojoj se koristi spremište Lake podataka, datoteke vezane uz klaster (kao što su zapisnike, itd.) zapisuju se zadani prostor za pohranu (Azure blob-ova), dok je s podacima koje želite obrađivati može se spremiti u spremištu Lake podataka računa.


U ovom se članku smo Dodjela Hadoop klaster s trgovinom Lake podataka kao dodatni prostor za pohranu.

Konfiguriranje HDInsight da biste radili s spremišta Lake podataka pomoću komponente PowerShell obuhvaća sljedeće korake:

* Stvaranje je iz trgovine Azure podataka Lake
* Postavljanje provjere autentičnosti za pristup spremišta Lake podataka na temelju uloga
* Stvaranje HDInsight klaster s provjerom autentičnosti spremište Lake podataka
* Pokreni test na klaster

## <a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

- **Azure PowerShell 1.0 ili noviji**. Saznajte [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

- **Windows SDK**. Možete ga instalirati s [ovdje](https://dev.windows.com/en-us/downloads). Koristi se za stvaranje sigurnosni certifikat.

- **Azure Active Directory servisa glavnicu**. Koraci u ovom ćete praktičnom vodiču sadrže upute za stvaranje glavni servisa u Azure AD. Međutim, morate biti administrator Azure AD da biste mogli stvoriti glavni servisa. Ako ste administrator Azure AD, preskočite ovaj preduvjeta i vodič za nastavak.
    
    **Ako niste administrator Azure AD**, nećete moći korake potrebne za stvaranje glavni servisa. U tom slučaju administratoru Azure AD najprije stvorite glavni servisa da biste mogli stvarati programa HDInsight klaster s trgovinom Lake podataka. Osim toga, Upravitelj servisa moraju se stvoriti pomoću certifikata, kao što je opisano na [stvorite glavni certifikatom za uslugu](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate). 


## <a name="create-an-azure-data-lake-store"></a>Stvaranje je iz trgovine Azure podataka Lake

Slijedite ove korake da biste stvorili Lake izvor podataka.

1. Na radnoj površini otvaranje novog prozora Azure PowerShell pa unesite sljedeće isječka. Kada se to od vas zatraži da biste se prijavili, provjerite je li se prijaviti kao jedan od admininistrators/vlasnik pretplate:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    >[AZURE.NOTE] Ako primite poruku o pogrešci slična `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` prilikom registriranja davatelja resursa za pohranu podataka Lake, moguće je na subsrcription nije whitelisted Azure podataka Lake trgovine. Provjerite je li omogućiti Azure pretplatu za pohranu podataka Lake javno pretpregled tako da slijedite ove [upute](data-lake-store-get-started-portal.md#signup).

3. Račun za Azure podataka Lake spremište povezan je s grupu resursa Azure. Započnite tako da stvorite grupu resursa Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Stvaranje grupe sustava Azure resursa] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Stvaranje grupe sustava Azure resursa")

2. Stvorite račun za Azure podataka Lake trgovine. Naziv računa koji navedete mora sadržavati samo mala slova i brojeva.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Stvaranje računa Lake Azure podataka] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Stvaranje računa Lake Azure podataka")

3. Provjerite je li račun uspješno stvorili.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Izlaz za to mora biti **True**.

4. Prenesite oglednih podataka Lake Azure podataka. Ćemo koristiti ovo u nastavku ovog članka da biste provjerili je li podataka dostupni iz programa klaster HDInsight. Ako tražite ogledne podatke da biste prenijeli, možete dobiti mapu **Podataka Ambulance** [Azure podataka Lake brojka spremište](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).


        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Postavljanje provjere autentičnosti za pristup spremišta Lake podataka na temelju uloga

Svaki Azure pretplate povezan je s Azure Active Directory. Korisnici i servise koje pristup resursima pretplate pomoću klasične Portal Azure ili Azure resursima API najprije morate provjeru autentičnosti s tom Azure Active Directory. Pristup moguć je Azure pretplate i servisima tako da im dodijelite odgovarajuću ulogu na Azure resursa.  Za usluge glavni servis označava servisa u na Azure Active Directory (AAD). U ovom se odjeljku ilustrira odobriti aplikacije servisa, kao što su HDInsight, pristup Azure resursa (Lake spremišta podataka za Azure račun koji ste prethodno stvorili) stvaranjem glavni servis za aplikaciju i korisnicima dodijelite uloge koje PowerShell Azure.

Da biste postavili provjeru autentičnosti servisa Active Directory za Azure podataka Lake, morate izvršiti sljedeće zadatke.

* Stvaranje samopotpisane potvrde
* Stvaranje aplikacije komponente u Azure Active Directory i glavni servisa

### <a name="create-a-self-signed-certificate"></a>Stvaranje samopotpisane potvrde

Provjerite je li [Windows SDK](https://dev.windows.com/en-us/downloads) instaliran prije nego što nastavite s koracima u ovom odjeljku. Morate i stvoriti imenika, kao što je **C:\mycertdir**, gdje će se stvoriti certifikata.

1. U prozoru PowerShell pomaknite se do mjesta na kojem je instaliran Windows SDK (obično `C:\Program Files (x86)\Windows Kits\10\bin\x86` i korištenje [MakeCert] [ makecert] utility da biste stvorili samopotpisani certifikat i privatni ključ. Koristite sljedeće naredbe.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        $startDate = (Get-Date).ToString('MM/dd/yyyy')
        $endDate = (Get-Date).AddDays(365).ToString('MM/dd/yyyy')

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -b $startDate -e $endDate -r -len 2048

    Će zatražiti da unesete lozinku privatni ključ. Kada se naredba uspješno izvrši, trebali biste vidjeti **CertFile.cer** i **mykey.pvk** u imeniku certifikat koje ste naveli.

4. Korištenje [Pvk2Pfx] [ pvk2pfx] utility za pretvaranje .pvk i .cer datoteke koje stvaraju MakeCert .pfx datoteka. Pokrenite sljedeću naredbu.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Kada se to od vas zatraži unos privatni ključ lozinku koju ste prethodno naveli. Vrijednost koju navedete parametar **– po** je lozinka koji je pridružen .pfx datoteka. Kada se naredba uspješno završi, trebali biste vidjeti i CertFile.pfx u imeniku certifikat koje ste naveli.

###  <a name="create-an-azure-active-directory-and-a-service-principal"></a>Stvaranje Azure Active Directory i glavni servisa

U ovom ćete odjeljku poduzeti korake za stvaranje servisa glavni programa Azure Active Directory, dodijelite uloge servisa glavni, i provjeri kao glavni servisa unosom certifikat. Pokrenite sljedeće naredbe za stvaranje aplikacije servisa Azure Active Directory.

1. Zalijepite sljedeće Cmdlete prozora konzole PowerShell. Provjerite je li vrijednost koju navedete za svojstvo **- Riješiti problem** jedinstveni. Osim toga, vrijednosti za **– Početna stranica** i **- IdentiferUris** su vrijednosti rezerviranog mjesta i se provjeriti.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
                    -DisplayName "HDIADL" `
                    -HomePage "https://contoso.com" `
                    -IdentifierUris "https://mycontoso.com" `
                    -KeyValue $credential  `
                    -KeyType "AsymmetricX509Cert"  `
                    -KeyUsage "Verify"  `
                    -StartDate $startDate  `
                    -EndDate $endDate

        $applicationId = $application.ApplicationId

2. Stvorite glavni servis pomoću aplikacije ID.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id

3. Dopustite pristup glavni servis spremišta podataka Lake datoteka/mapu u kojoj će pristupiti iz skupine HDInsight. Isječak ispod omogućuje pristup korijenu računa spremišta Lake podataka.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All

    Kada se zatraži, unesite **Y** da biste je potvrdili.

## <a name="create-an-hdinsight-cluster-with-authentication-to-data-lake-store"></a>Stvaranje programa HDInsight klaster s provjerom autentičnosti spremište Lake podataka

U ovom ćete odjeljku ćemo stvoriti programa klaster HDInsight Hadoop. U ovom izdanju klaster HDInsight i Lake spremišta podataka moraju biti na istom mjestu (Istočni sad 2).

1. Započnite dohvaćanje ID klijenta za pretplatu Trebat će vam koji kasnije.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. U ovom izdanju za na Hadoop klaster spremišta podataka Lake se koristiti samo kao dodatan prostor za pohranu za klaster. Zadani prostor za pohranu će i dalje biti Azure prostora za pohranu blob-ova (WASB). Tako, ne možemo ćete prvo stvorite račun za pohranu i potpisu potrebne za klaster.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName | %{ $_.Key1 }
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext

3. Stvaranje klaster HDInsight. Koristite sljedeće Cmdlete.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $rdpCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.2" -RdpCredential $rdpCredentials -RdpAccessExpiry (Get-Date).AddDays(14) -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Nakon što cmdlet uspješno završi, trebali biste vidjeti na Izlaz ovako:

        Name                      : hdiadlcluster
        Id                        : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/hdiadlgroup/providers/Mi
                                    crosoft.HDInsight/clusters/hdiadlcluster
        Location                  : East US 2
        ClusterVersion            : 3.2.7.707
        OperatingSystemType       : Windows
        ClusterState              : Running
        ClusterType               : Hadoop
        CoresUsed                 : 16
        HttpEndpoint              : hdiadlcluster.azurehdinsight.net
        Error                     :
        DefaultStorageAccount     :
        DefaultStorageContainer   :
        ResourceGroup             : hdiadlgroup
        AdditionalStorageAccounts :

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Pokrenite test zadatke na klaster HDInsight da koristi spremište Lake podataka

Nakon što ste konfigurirali za HDInsight klaster, možete pokrenuti testiranje zadatke na klaster da biste testirali klaster HDInsight pristupite spremištu Lake podataka. Da biste to učinili, ne možemo će se pokrenuti posao grozd uzorka koji stvara tablicu korištenje oglednih podataka koji ste prenijeli ranije Lake spremište podataka.

### <a name="for-a-linux-cluster"></a>Za Linux klaster

U ovom odjeljku će SSH u klaster i pokrenite na primjer grozd upita. Windows ne nudi ugrađene SSH klijenta. Preporučujemo korištenje **PuTTY**, koji možete preuzeti s [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Dodatne informacije o korištenju PuTTY potražite u članku [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Nakon uspostave pokrenuti EŽA vrste Hive u sljedeću naredbu:

        hive

2. Koristi se EŽA, unesite sljedeće naredbe za stvaranje nove tablice pod nazivom **postizanja** pomoću ogledne podatke u spremištu Lake podataka:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Trebali biste vidjeti na Izlaz sličnu ovoj:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


### <a name="for-a-windows-cluster"></a>Za Windows klaster

Koristite sljedeće Cmdlete da biste pokrenuli upit grozd. U ovom upitu smo stvaranje tablice iz podataka u spremištu Lake podataka i pokrenuti upit s izdvajanjem stvorili tablicu.

    $queryString = "DROP TABLE vehicles;" + "CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://$dataLakeStoreName.azuredatalakestore.net:443/';" + "SELECT * FROM vehicles LIMIT 10;"

    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString

    $hiveJob = Start-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $httpCredentials

    Wait-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $httpCredentials

Imat će sljedeći rezultat. **ExitValue** 0 u izlaz predlaže uspješno dovršena posao.

    Cluster         : hdiadlcluster.
    HttpEndpoint    : hdiadlcluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1445386885331_0012
    ParentId        :
    PercentComplete :
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done

Dohvaćanje Izlaz iz posla pomoću sljedeći cmdlet:

    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $hiveJob.JobId -DefaultContainer $containerName -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -ClusterCredential $httpCredentials

Izlaz posao izgleda otprilike ovako:

    1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
    1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
    1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
    1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
    1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
    1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
    1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
    1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
    1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
    1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


## <a name="access-data-lake-store-using-hdfs-commands"></a>Pristup podataka iz trgovine Lake pomoću naredbi HDFS

Nakon što ste konfigurirali klaster HDInsight da koristi spremište Lake podataka, koristite naredbe ljuske HDFS za pristup spremište.

### <a name="for-a-linux-cluster"></a>Za Linux klaster

U ovoj sekciji koju će SSH u klaster i pokretanje naredbe HDFS. Windows ne nudi ugrađene SSH klijenta. Preporučujemo korištenje **PuTTY**, koji možete preuzeti s [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Dodatne informacije o korištenju PuTTY potražite u članku [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Nakon uspostave koristite sljedeću naredbu datotečnom sustavu HDFS na popisu datoteka u spremištu Lake podataka.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

To treba popis datoteka koje ste prethodno prenijeli Lake spremište podataka.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Možete koristiti u `hdfs dfs -put` naredbu za prijenos neke datoteke Lake spremište podataka, a zatim pomoću `hdfs dfs -ls` da biste provjerili je li datoteka uspješno prenijeli.


### <a name="for-a-windows-cluster"></a>Za Windows klaster

1. Prijavite se novom [Portalu Azure](https://portal.azure.com).

2. Kliknite **Pregledaj**, kliknite **HDInsight klastere**, a zatim klaster HDInsight koji ste stvorili.

3. U plohu klaster kliknite **Udaljene radne površine**, a zatim plohu **Udaljene radne površine** kliknite **Poveži**.

    ![Udaljena u HDI klaster] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.HDI.PS.Remote.Desktop.png "Stvaranje grupe sustava Azure resursa")

    Kada se to od vas zatraži, unesite vjerodajnice navedene za udaljene radne površine korisnika.

4. U udaljenu sesiju pokrenite Windows PowerShell i naredbama HDFS datotečnom sustavu popis datoteka u spremištu Lake Azure podataka.

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    To treba popis datoteka koje ste prethodno prenijeli Lake spremište podataka.

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/vehicle1_09142014.csv

    Možete koristiti u `hdfs dfs -put` naredbu za prijenos neke datoteke Lake spremište podataka, a zatim pomoću `hdfs dfs -ls` da biste provjerili je li datoteka uspješno prenijeli.

## <a name="see-also"></a>Vidi također

* [Portal: Stvaranje programa HDInsight klaster da koristi spremište Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
