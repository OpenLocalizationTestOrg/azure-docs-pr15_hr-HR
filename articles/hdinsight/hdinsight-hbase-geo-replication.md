<properties 
   pageTitle="Konfiguriranje HBase replikacijom između dva podatkovnim centrima | Microsoft Azure" 
   description="Saznajte kako konfigurirati HBase replikacije svim centrima dva podataka i o slučajevima koristi za replikaciju klaster." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="07/25/2016"
   ms.author="jgao"/>

# <a name="configure-hbase-geo-replication-in-hdinsight"></a>Konfiguriranje zemlj replikacije HBase u HDInsight

> [AZURE.SELECTOR]
- [Konfiguriranje povezivanja s VPN-a](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfiguriranje DNS-a](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfiguriranje HBase replikacije](hdinsight-hbase-geo-replication.md) 
 
Saznajte kako konfigurirati HBase replikacije svim centrima dva podataka. Neki koriste slučajeva klaster replikacije biti:

- Oporavak i Izrada sigurnosne kopije
- Prikupljanje podataka
- Geografske podatke raspodjele
- Ingestion Online podataka u kombinaciji s analize podataka izvan mreže

Replikacija klaster koristi izvor automatske methodology. Na HBase klaster može biti izvor, odredište, ili odjednom ispuniti i uloge. Replikacija asinkronog, a cilj replikacije usmjerenog dosljednost. Kada izvorišnog web-mjesta dobije uređivanjem liniju za stupac s replikacijom omogućena, taj Uredi prenose se na sve klastere odredište. Kada podataka je replicirati iz jednog klaster na drugu, klaster izvora i sve klastere koje ste već potrošena podatke evidentiraju se da biste spriječili replikacijom petlje. Dodatne informacije potražite u ovom ćete praktičnom vodiču će konfigurirati izvor odredište replikacijom.  Topologija druge klaster potražite u članku [Vodič za HBase Apache](http://hbase.apache.org/book.html#_cluster_replication).

Ovo je treći dio niza:

- [Konfiguriranje VPN veze između dvije virtualne mreže][hdinsight-hbase-replication-vnet]
- [Konfiguriranje DNS-a za virtualne mreže][hdinsight-hbase-replication-dns]
- Konfiguriranje replikacije HBase zemlj. (ovog praktičnog vodiča)

Sljedeći dijagram prikazuje dvije virtualne mreže i veza s mrežom koju ste stvorili u članku [Konfiguriranje VPN veze između dvije virtualne mreže] [ hdinsight-hbase-geo-replication-vnet] i [Konfiguriranje DNS-om za virtualne mreže][hdinsight-hbase-replication-dns]: 

![HDInsight HBase replikacije virtualne mrežni dijagram][img-vnet-diagram]

## <a id="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Radne stanice s Azure PowerShell**.

    Za izvođenje skripte komponente PowerShell, morate Azure PowerShell Pokreni kao administrator i postavite pravila izvršavanja *RemoteSigned*. Pogledajte odjeljak korištenje cmdlet skup-pravilnik izvođenja.
    
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Dva Azure virtualne mreže s VPN povezivanja i konfigurirati DNS-a**.  Upute potražite u članku [Konfiguriranje VPN veza između dvije mreže Azure virtualne][hdinsight-hbase-replication-vnet], i [Konfiguriranje DNS između dvije mreže Azure virtualne][hdinsight-hbase-replication-dns].


    Prije pokretanja skripte komponente PowerShell, provjerite jeste li povezani u pretplatu Azure pomoću sljedeći cmdlet:

        Add-AzureAccount

    Ako imate više pretplata Azure, koristite sljedeći cmdlet da biste postavili trenutne pretplate:

        Select-AzureSubscription <AzureSubscriptionName>



## <a name="provision-hbase-clusters-in-hdinsight"></a>Dodjela resursa za klastere HBase u HDInsight

U članku [Konfiguriranje VPN veza između dvije mreže Azure virtualne][hdinsight-hbase-replication-vnet], stvorite virtualne mreže u u Europi podatkovnog centra, a virtualne mreže u podatkovnom centru SAD-a. Dva virtualne mreže povezani putem VPN-a. U ovoj sesiji će Dodjela programa HBase klaster u svakoj od virtualne mreže. U nastavku ovog praktičnog vodiča će postati nešto klastere HBase za replikaciju u HBase klaster.

Klasični Portal za Azure ne podržava dodjele resursa klastere HDInsight pomoću mogućnosti prilagođene konfiguracije. Na primjer, postavite *hbase.replication* na *true*. Ako postavite vrijednost u datoteci konfiguracije kada je dodjeli klaster, izgubit ćete postavku kada je u tijeku reimaged klaster. Dodatne informacije potražite u članku [Dodjeljivanje Hadoop klastere u HDInsight][hdinsight-provision]. Koristi neku od mogućnosti Dodjela HDInsight klaster s prilagođene mogućnosti Azure PowerShell.


**Dodjela resursa za HBase klaster u Contoso VNet EU** 

1. Iz vaše radne stanice, otvorite Windows Očisti filtar.
2. Postavljanje varijable na početku skriptu, a zatim pokrenuti skriptu.

        # create hbase cluster with replication enabled
        
        $azureSubscriptionName = "[AzureSubscriptionName]"
        
        $hbaseClusterName = "Contoso-HBase-EU" # This is the HBase cluster name to be used.
        $hbaseClusterSize = 1   # You must provision a cluster that contains at least 3 nodes for high availability of the HBase services.
        $hadoopUserLogin = "[HadoopUserName]"
        $hadoopUserpw = "[HadoopUserPassword]"
        
        $vNetName = "Contoso-VNet-EU"  # This name must match your Europe virtual network name.
        $subNetName = 'Subnet-1' # This name must match your Europe virtual network subnet name.  The default name is "Subnet-1".
        
        $storageAccountName = 'ContosoStoreEU'  # The script will create this storage account for you.  The storage account name doesn't support hyphens. 
        $storageAccountName = $storageAccountName.ToLower() #Storage account name must be in lower case.
        $blobContainerName = $hbaseClusterName.ToLower()  #Use the cluster name as the default container name.
        
        #connect to your Azure subscription
        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName
        
        # Create a storage account used by the HBase cluster
        $location = Get-AzureVNetSite -VNetName $vNetName | %{$_.Location} # use the virtual network location
        New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location
        
        # Create a blob container used by the HBase cluster
        $storageAccountKey = Get-AzureStorageKey -StorageAccountName $storageAccountName | %{$_.Primary}
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $blobContainerName -Context $storageContext
        
        # Create provision configuration object
        $hbaseConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHBaseConfiguration'
            
        $hbaseConfigValues.Configuration = @{ "hbase.replication"="true" } #this modifies the hbase-site.xml file
        
        # retrieve vnet id based on vnetname
        $vNetID = Get-AzureVNetSite -VNetName $vNetName | %{$_.id}
        
        $config = New-AzureHDInsightClusterConfig `
                        -ClusterSizeInNodes $hbaseClusterSize `
                        -ClusterType HBase `
                        -VirtualNetworkId $vNetID `
                        -SubnetName $subNetName `
                    | Set-AzureHDInsightDefaultStorage `
                        -StorageAccountName $storageAccountName `
                        -StorageAccountKey $storageAccountKey `
                        -StorageContainerName $blobContainerName `
                    | Add-AzureHDInsightConfigValues `
                        -HBase $hbaseConfigValues
        
        # provision HDInsight cluster
        $hadoopUserPassword = ConvertTo-SecureString -String $hadoopUserpw -AsPlainText -Force     
        $credential = New-Object System.Management.Automation.PSCredential($hadoopUserLogin,$hadoopUserPassword)
        
        $config | New-AzureHDInsightCluster -Name $hbaseClusterName -Location $location -Credential $credential



**Dodjela resursa za HBase klaster u Contoso-VNet-hr** 

- Koristite isti skripte za sljedeće vrijednosti:

        $hbaseClusterName = "Contoso-HBase-US" # This is the HBase cluster name to be used.
        $vNetName = "Contoso-VNet-US"  # This name must match your Europe virtual network name.
        $storageAccountName = 'ContosoStoreUS'  

    Budući da ste već povezali račun za Azure, ne morate pokrenuti sljedeći comlets više:

        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName




## <a name="configure-dns-conditional-forwarder"></a>Konfiguriranje DNS uvjetno forwarder

U [Konfiguriranje DNS-om za virtualne mreže][hdinsight-hbase-replication-dns], ste konfigurirali DNS poslužitelji za dvije mreže. Skupina HBase imaju nastavke drugu domenu. Stoga morate konfigurirati dodatne DNS prosljeđivanja uvjetno.

Da biste konfigurirali uvjetno forwarder, morate znati nastavke domene dva HBase skupina. 

**Da biste pronašli nastavke domene dva klastere HBase**

1. RDP u **Contoso HBase EU**.  Upute potražite u članku [Upravljanje Hadoop klastere u HDInsight pomoću portala za klasični Azure][hdinsight-manage-portal]. To je zapravo headnode0 klaster.
2. Otvorite konzole za Windows PowerShell ili naredbeni redak.
3. Pokrenite **ipconfig**i zapišite **sufiks DNS specifične za vezu**.
4. Ne zatvorite RDP sesiju.  Jer će vam kasnije za provjeru razrješenje naziva domene.
5. Ponovite iste korake da biste saznali **DNS specifične sufiks** **Contoso-HBase-hr**.


**Konfiguriranje DNS prosljeđivanja**
 
1.  RDP u **EU Contoso-DNS-a**. 
2.  Kliknite s logotipom sustava Windows u donjem lijevom.
2.  Kliknite **stavku Administrativni alati**.
3.  Kliknite **DNS**.
4.  U lijevom oknu proširite **DSN**, **EU Contoso-DNS-a**.
5.  Desnom tipkom miša kliknite **Uvjetno prosljeđivanja**, a zatim kliknite **Novi uvjetno Forwarder**. 
5.  Unesite sljedeće podatke:
    - **DNS domena**: Unesite DNS sufiks Contoso-HBase-hr. Na primjer: Contoso HBase US.f5.internal.cloudapp.net.
    - **IP adrese glavnog poslužitelja**: Unesite 10.2.0.4 koja je u Contoso-DNS-hr-IP adresa. Provjerite je li na IP. DNS poslužitelj može imati drugu IP adresu.
6.  Pritisnite **ENTER**, a zatim kliknite **u redu**.  Sada će biti možete riješiti na Contoso-DNS-hr-IP adrese iz EU Contoso-DNS-a.
7.  Ponovite korake da biste dodali DNS forwarder za uvjetno DNS servis virtualnog računala sad Contoso-DNS-a pomoću sljedeće vrijednosti:
    - **DNS domena**: Unesite DNS sufiks Contoso-HBase-EU-a. 
    - **IP adrese glavnog poslužitelja**: Unesite 10.2.0.4 koji je Contoso-DNS-EU-a, IP adresa.

**Da biste testirali razlučivanje naziva domene**

1. Prijelaz na prozor RDP Contoso HBase EU.
2. Otvorite naredbeni redak.
3. Pokretanje naredbe ping:

        ping headnode0.[DNS suffix of Contoso-HBase-US]

    Protokol ICM uključen čvorove tempiranja klastere HBase

4. Nemojte zatvoriti RDP sesiju. I dalje ćete ga kasnije u ovom praktičnom vodiču.
5. Ponovite iste korake da biste pomoću naredbe ping headnode0 Contoso-HBase-EU-a iz Contoso-HBase-hr.

>[AZURE.IMPORTANT] DNS-a morate prije nastavka na sljedeće korake.

## <a name="enable-replication-between-hbase-tables"></a>Omogućivanje replikacije između tablica HBase

Sada možete stvoriti tablicu HBase uzorka, omogućiti replikaciju i još testirajte sadrži podatke. Ogledna tablica će koristiti ima dva stupca linije: Personal i Office. 

U ovom ćete praktičnom vodiču će postati klaster Europe HBase kao izvor klaster, a sad HBase klaster kao klaster odredište.

Stvorite HBase tablice s istim nazivima i linije stupac na izvorišne i odredišne klastere tako da se klaster odredište zna gdje želite spremiti podatke primit će. Dodatne informacije o korištenju ljuske HBase potražite u članku [Početak rada s Apache HBase u HDInsight][hdinsight-hbase-get-started].

**Da biste stvorili tablicu programa HBase na Contoso HBase EU**

1. Prijelaz na prozor RDP **Contoso HBase EU** .
2. Na radnoj površini, kliknite **Hadoop naredbenog retka**.
2. Promijenite mapu u direktoriju kućni HBase:

        cd %HBASE_HOME%\bin
3. Otvorite ljusku HBase:

        hbase shell
4. Stvaranje tablice programa HBase:

        create 'Contacts', 'Personal', 'Office'
5. Nemojte zatvoriti sesiju RDP ni prozor naredbenog retka Hadoop. I dalje ćete ih kasnije u ovom praktičnom vodiču.
    
**Da biste stvorili tablicu programa HBase na Contoso-HBase-hr**

- Ponovite iste korake da biste stvorili istoj tablici na Contoso-HBase-hr.


**Da biste dodali Contoso-HBase-hr kao ravnopravnih članova replikacijom**

1. Prijelaz na prozor RDP **Contso HBase_EU** .
2. U prozoru ljuske HBase klaster odredište (Contoso-HBase-HR) kao ravnopravnih članova, na primjer, dodati:

        add_peer '1', 'zookeeper0.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper1.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper2.contoso-hbase-us.d4.internal.cloudapp.net:2181:/hbase'

    U uzorku, nastavak domene je *contoso hbase us.d4.internal.cloudapp.net*. Trebate ažurirati tako da odgovara sufiks klaster NAM HBase vaše domene. Provjerite je li bez razmaka između na hostnames.

**Da biste konfigurirali obitelj svakog stupca da biste je replicirati na klasteru izvora**

1. U prozoru ljuske HBase sesije RDP **Contso HBase EU** konfigurirati obitelj svakog stupca da biste je replicirati:

        disable 'Contacts'
        alter 'Contacts', {NAME => 'Personal', REPLICATION_SCOPE => '1'}
        alter 'Contacts', {NAME => 'Office', REPLICATION_SCOPE => '1'}
        enable 'Contacts'

**Da biste skupno prijenos podataka u tablici HBase**

Datoteku s oglednim podacima prenesene javno blobova platforme Azure kontejner s sljedeći URL:

        wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

Sadržaj datoteke:

        8396    Calvin Raji 230-555-0191    5415 San Gabriel Dr.
        16600   Karen Wu    646-555-0113    9265 La Paz
        4324    Karl Xie    508-555-0163    4912 La Vuelta
        16891   Jonathan Jackson    674-555-0110    40 Ellis St.
        3273    Miguel Miller   397-555-0155    6696 Anchor Drive
        3588    Osarumwense Agbonile    592-555-0152    1873 Lion Circle
        10272   Julia Lee   870-555-0110    3148 Rose Street
        4868    Jose Hayes  599-555-0171    793 Crawford Street
        4761    Caleb Alexander 670-555-0141    4775 Kentucky Dr.
        16443   Terry Chander   998-555-0171    771 Northridge Drive

Možete prenijeti isti podatkovne datoteke u svoj klaster HBase i uvoz podataka iz nje.

1. Prijelaz na prozor RDP **Contoso HBase EU** .
2. Na radnoj površini, kliknite **Hadoop naredbenog retka**.
3. Promijenite mapu u direktoriju kućni HBase:

        cd %HBASE_HOME%\bin

4. prijenos podataka:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:HomePhone, Office:Address" -Dimporttsv.bulk.output=/tmpOutput Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmpOutput Contacts


## <a name="verify-that-data-replication-is-taking-place"></a>Provjerite da replikacija podataka se izvršava u zemlji

Možete provjeriti da replikacija se izvršava u zemlji pregledom tablice iz obje klastere pomoću sljedeće naredbe ljuske HBase:

        Scan 'Contacts'


## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču ste naučili kako konfigurirati replikacije HBase preko dva podatkovnim centrima. Da biste saznali više o HDInsight i HBase, pogledajte:

- [Stranice servisa HDInsight](https://azure.microsoft.com/services/hdinsight/)
- [Dokumentacija HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)
- [Početak rada s Apache HBase u HDInsight][hdinsight-hbase-get-started]
- [Pregled HDInsight HBase][hdinsight-hbase-overview]
- [Dodjela resursa za klastere HBase na Azure virtualne mreže][hdinsight-hbase-provision-vnet]
- [Analiza u stvarnom vremenu Twitter šalju s HBase][hdinsight-hbase-twitter-sentiment]
- [Analiza podataka senzor s oluja i HBase u HDInsight (Hadoop)][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: powershell-install-configure.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-hbase-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
