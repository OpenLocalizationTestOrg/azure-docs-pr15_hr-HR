<properties
    pageTitle="Praktični vodič HBase: početak rada s HBase u Hadoop | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič HBase da biste mogli početi koristiti Apache HBase s Hadoop u HDInsight. Stvaranje tablice iz ljuske HBase i upit ih pomoću grozd."
    keywords="Apache hbase, hbase, hbase ljuske, hbase vodiča"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-windows-based-hadoop-in-hdinsight"></a>Praktični vodič HBase: početak rada s Apache HBase s utemeljen na sustavu Windows Hadoop u HDInsight

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Upute za stvaranje klastere HBase u HDInsight, stvorite HBase tablice i upite tablice pomoću vrste Hive Apache. Opće informacije HBase potražite u članku [Pregled HDInsight HBase][hdinsight-hbase-overview].

Informacije u ovom dokumentu je za klastere HDInsight utemeljen na sustavu Windows. Informacije o klastere utemeljen na sustavu Windows koristite birač tabulatora pri vrhu stranice da biste se prebacili.

> [AZURE.NOTE] HBase (verzija 0.98.0) na utemeljen na sustavu Windows HDInsight dostupna je samo za korištenje sa servisa HDInsight 3.1 klastere (na temelju Apache Hadoop i YARN 2.4.0). Informacije o verziji, potražite u članku [što je novo u verzijama klaster Hadoop nudi HDInsight?][hdinsight-versions]

## <a name="before-you-begin"></a>Prije početka

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Prije početka ovog praktičnog vodiča HBase, morate imati sljedeće:

- **A Microsoft Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Na radne stanice** s Visual Studio 2013 ili noviji: upute potražite u članku [Instalacija Visual Studio](http://msdn.microsoft.com/library/e2h7fzkw.aspx).

### <a name="access-control-requirements"></a>Preduvjeti za kontrolu pristupa

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Stvaranje HBase klaster

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**Da biste stvorili programa HBase klaster pomoću portala za Azure**

1. Prijava na [portal za Azure][azure-management-portal].
2. Kliknite **Novo** ili **+** u gornjem lijevom kutu uglu, a zatim kliknite **podataka + analize**, **HDInsight**.
3. Unesite sljedeće vrijednosti:

    - **Naziv klaster** - unesite naziv za identifikaciju ovoj grupi.
    - **Vrsta klaster** – odabir **HBase**.
    - **Operacijski sustav klaster** – odaberite **Windows**.  Za stvaranje sustavom Linux HBase skupine potražite u članku [HBase Praktični vodič: početak rada s Apache HBase s Hadoop u HDInsight (Linux)](hdinsight-hbase-tutorial-get-started-linux.md).
    - **Verzija** – odaberite neku od verzija HBase.
    - **Pretplata** - odaberite pretplate Azure koristiti za stvaranje ovoj grupi.
    - **Grupa resursa** – stvaranje nove grupe Azure resursa ili odaberite postojeći. Dodatne informacije potražite u članku [Pregled upravljanja resursima za Azure](azure-resource-manager/resource-group-overview.md)
    - **Vjerodajnice** – za Windows temelji klaster, možete stvoriti klaster korisnika (a.k.a HTTP korisniku, korisnik usluge web HTTP) i korisnik udaljene radne površine. Kliknite **Omogući udaljene radne površine** da biste dodali vjerodajnice udaljenog računala korisnika. U sljedećem odjeljku zahtijeva RDP.
    - **Izvor podataka** – stvaranje novog računa Azure prostora za pohranu ili odaberite postojeći račun za Azure pohranu će se koristiti kao zadani datotečni sustav za klaster. Zadano mjesto za pohranu računa određuje mjesto klaster mjesto. Zadani račun za pohranu i klaster Suradnja morate pronaći u centru za iste podatke.
    - **Razine cijena čvor** – odaberite broj područja poslužitelja za HBase klaster

        > [AZURE.WARNING] Dostupnost visoke HBase usluga, morate stvoriti klaster koja sadrži barem **tri** čvorove. Na taj način, ako funkcionira jedan čvor, područja podataka HBase su dostupni na ostale čvorove.

        > Ako učenju HBase, uvijek odaberite 1 za klaster i izbrišite skupine nakon svakog korištenja da biste smanjili trošak.

    - **Neobavezni konfiguracija** – konfiguriranje Azure virtualne mreže, konfiguriranje akcije skripte i dodati račune za dodatni prostor za pohranu.

4. Kliknite **Stvori**.

>[AZURE.NOTE] Nakon brisanja programa HBase klaster drugi HBase klaster možete stvoriti pomoću isti zadani račun za pohranu i blob spremnik zadani. Novi klaster će obraditi HBase tablice koje ste stvorili u izvornom klaster. Da biste izbjegli nedosljednosti, preporučujemo da prije brisanja klaster onemogućite HBase tablice.

## <a name="create-tables-and-insert-data"></a>Stvaranje tablice i umetanje radnog lista

Trenutno postoje dva načina da biste pristupili HBase. U ovom se odjeljku opisuje pomoću ljuske HBase. U sljedećem odjeljku pokriva pomoću .NET SDK-a.

Za većinu korisnika, prikaza podataka u tabličnom obliku:

![hdinsight hbase tablične podatke][img-hbase-sample-data-tabular]

U HBase koja je implementacija BigTable iste podatke izgleda:

![hdinsight hbase bigtable podataka][img-hbase-sample-data-bigtable]

To ćete smisla više nakon što dovršite sljedeći postupak.  

**Da biste koristili HBase shell**

1. Korištenje RDP za povezivanje s svoj klaster HBase u HDInsight. Upute za RDP potražite u članku [Upravljanje Hadoop klastere u HDInsight pomoću portala za Azure][hdinsight-manage-portal].
2. Unutar sesiju RDP kliknite prečac **Hadoop naredbenog retka** koji se nalazi na radnoj površini.
3. Otvorite ljusku HBase:

        cd %HBASE_HOME%\bin
        hbase shell

4. Stvaranje programa HBase s dva stupca linije:

        create 'Contacts', 'Personal', 'Office'
        list
5. Umetanje nekih podataka:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsight hadoop hbase shell][img-hbase-shell]

6. Početak jedan redak

        get 'Contacts', '1000'

    Vidjet ćete iste rezultate kao pomoću naredbe za pregled jer postoji samo jedan redak.

    Dodatne informacije o shemi Hbase tablice potražite u članku [Uvod u HBase sheme dizajn][hbase-schema]. Dodatne naredbe HBase, potražite u članku [vodič Apache HBase][hbase-quick-start].


6. Izlaz iz ljuske

        exit

**Da biste skupno učitavanja podataka u tablicu HBase kontakata**

HBase sadrži nekoliko metoda učitavanja podataka u tablice. Dodatne informacije potražite u članku [Učitavanje skupno](http://hbase.apache.org/book.html#arch.bulk.load).


Datoteku s oglednim podacima prenesen spremniku javno blob wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt. Sadržaj podatkovne datoteke je:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Možete stvoriti tekstnu datoteku i prijenos datoteke u račun za pohranu po želji. Upute potražite u odjeljku [prijenos podataka za Hadoop poslove u HDInsight][hdinsight-upload-data].

> [AZURE.NOTE] Ovaj postupak koristi tablici HBase kontakata koje ste stvorili u zadnji postupak.

1. Unutar sesiju RDP kliknite prečac **Hadoop naredbenog retka** koji se nalazi na radnoj površini.
2. Promjena direktorija:

        cd %HBASE_HOME%\bin

3. Pokrenite sljedeću naredbu za pretvorbu podatkovna datoteka za web-mjesto StoreFiles i u okvir za spremište na relativni put određen Dimporttsv.bulk.output:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Pokrenite sljedeću naredbu da biste prenijeli podatke iz /example/data/storeDataFileOutput HBase tablicu:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Otvorite ljusku HBase i upotrijebite naredbu pregled da biste sadržaj tablice popisa.



## <a name="use-hive-to-query-hbase-tables"></a>Pomoću grozd tablicama HBase upita

Upit možete poslati podatke pohranjene u HBase pomoću grozd. U ovom se odjeljku stvara tablicu vrste Hive karte HBase tablice i koristi za dohvaćanje podataka u tablici HBase.

**Da biste otvorili na nadzornoj ploči klaster**

1. Pronađite **https://<HDInsight Cluster Name>.azurehdinsight.net/**.
5. Unesite Hadoop računa korisničko ime i lozinku. Zadano korisničko ime je **administrator** i lozinka je unijeli tijekom procesa stvaranja. Otvorit će se na novoj kartici preglednika.
6. Kliknite **Vrste Hive uređivač** pri vrhu stranice. Uređivač vrste Hive izgleda ovako:

    ![Nadzorna ploča klaster HDInsight.][img-hdinsight-hbase-hive-editor]

**Da biste pokrenuli grozd upita**

1. Unesite sljedeću skriptu HiveQL u vrste Hive uređivač i kliknite **Pošalji** da biste stvorili tablicu vrste Hive koja mapira u tablici HBase. Provjerite je li koji ste stvorili ogledne tablice koje se pozivaju na nju ranije u ovom praktičnom vodiču pomoću ljuske HBase prije pokretanja izjavu o zaštiti.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    Pričekajte dok ažuriranjima **statusa** **dovršiti**.

2. Unesite sljedeću skriptu HiveQL u vrste Hive uređivač pa kliknite **Pošalji**. Upit grozd upiti podatke u tablici HBase:

        SELECT count(*) FROM hbasecontacts;

4. Da biste dohvatili rezultate upita grozd, kliknite vezu **Prikaz detalja** u prozoru **Sesiju posao** kada posao završi s radom. Pojavit će se samo jedan zadatak Izlazna datoteka jer staviti jedan zapis u tablici HBase.




**Da biste pregledali Izlazna datoteka**

1. Na konzoli za upit kliknite **Preglednik datoteka**.
2. Kliknite račun za Azure prostora za pohranu koji se koristi kao zadani datotečni sustav za klaster HBase.
3. Kliknite naziv HBase klaster. Spremnik za račun Azure pohranu zadani koristi naziv klaster.
4. Kliknite **korisnika**, a zatim **administrator**. (To je korisničko ime Hadoop.)
6. Kliknite naziv zadatka s vremenom **Zadnje promjene** koja odgovara put kada pokrenete upit za odabir vrste Hive.
4. Kliknite **stdout**. Spremite datoteku i otvorite datoteku s Blok za pisanje. Pojavit će se jedan Izlazna datoteka.

    ![HDInsight HBase grozd uređivač datoteke preglednika][img-hdinsight-hbase-file-browser]

## <a name="use-the-net-hbase-rest-api-client-library"></a>Korištenje biblioteka klijentski .NET HBase REST API-JA

Morate preuzeti klijentska biblioteka HBase REST API-JA za .NET iz GitHub i sastavljanje projekt da bi mogli koristiti HBase .NET SDK. U nastavku sadrži upute za taj zadatak.

1. Stvaranje nove aplikacije C# Visual Studio radnu površinu konzole za Windows.
2. Otvorite Upravitelj paketa NuGet konzole tako da kliknete **Alati** > **Upravitelj paketa NuGet** > **Konzole za Upravitelj paketa**.
3. Pokrenite sljedeću naredbu NuGet na konzoli:

        Install-Package Microsoft.HBase.Client

5. Dodajte sljedeće **pomoću** naredbe pri vrhu datoteke:

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. Zamijenite funkciju **glavne** sljedeće:

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

            //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
            Scanner scanSettings = new Scanner()
            {
                batch = 10,
                startRow = BitConverter.GetBytes(25),
                endRow = BitConverter.GetBytes(35)
            };

            ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
            CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
            {
                foreach (CellSet.Row row in next.rows)
                {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
                }
            }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. Postavite prva tri varijable u funkciji **glavne** .
8. Pritisnite **F5** da biste pokrenuli aplikaciju.

## <a name="check-cluster-status"></a>Provjera statusa klaster

HBase u HDInsight dolazi s korisničkim Sučeljem Web za klastere za nadzor. Pomoću korisničkog Sučelja Web, možete zatražiti Statistika ili informacije o područja.

Da biste otvorili korisničkog Sučelja Web, morate RDP u klaster, i zatim kliknite HMaster informacije Web UI prečac na radnoj površini, ili koristiti sljedeći URL web-preglednika:

    http://zookeeper[0-2]:60010/master-status

Klasteru visoke dostupnosti, vidjet ćete vezu na trenutni aktivni HBase osnovne čvor koji se nalaze korisničkog Sučelja Web.

##<a name="delete-the-cluster"></a>Brisanje klaster
Da biste izbjegli nedosljednosti, preporučujemo da prije brisanja klaster onemogućite HBase tablice.
[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="whats-next"></a>Što je sljedeće?
U ovom vodiču HBase za HDInsight naučili kako stvoriti programa HBase klaster i kako stvoriti tablice i prikaz podataka u tim tablicama iz ljuske HBase. Koje interaktivnima te kako koristiti upit grozd podataka u tablicama HBase i upute za korištenje HBase C# REST API-ji za stvaranje tablice programa HBase i dohvaćanje podataka iz tablice.

Dodatne informacije potražite u članku:

- [Pregled HDInsight HBase][hdinsight-hbase-overview].
HBase je Apache, Otvori izvor NoSQL baza podataka utemeljena na Hadoop koja omogućuje izravnim pristupom i istaknuti dosljednost velikih količina podataka nestrukturirane i semistructured.
- [Stvaranje HBase klastere na Azure virtualne mreže][hdinsight-hbase-provision-vnet].
Uz integraciju virtualne mreže klastere HBase može uvesti u istom virtualne mreže kao aplikacija bi aplikacija možete komunicirati s HBase izravno.
- [Konfiguriranje HBase replikacije u HDInsight](hdinsight-hbase-geo-replication.md). Saznajte kako konfigurirati replikacije HBase preko dva Azure podatkovnim centrima.
- [Analiza Twitter šalju s HBase u HDInsight][hbase-twitter-sentiment].
Saznajte kako u stvarnom vremenu [šalju analiza](http://en.wikipedia.org/wiki/Sentiment_analysis) velikih skupova podataka pomoću HBase klasteru Hadoop u HDInsight.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png
