<properties
    pageTitle="Praktični vodič HBase: početak rada sa sustavom Linux HBase klastere u Hadoop | Microsoft Azure"
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
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-linux-based-hadoop-in-hdinsight"></a>Praktični vodič HBase: početak rada s Apache HBase s operacijskim sustavom Linux Hadoop u HDInsight 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Upute za stvaranje programa klaster HBase u HDInsight, stvorite HBase tablice i upite tablice pomoću grozd. Opće informacije HBase potražite u članku [Pregled HDInsight HBase][hdinsight-hbase-overview].

Informacije u ovom dokumentu je za klastere sustavom Linux HDInsight. Informacije o klastere utemeljen na sustavu Windows koristite birač tabulatora pri vrhu stranice da biste se prebacili.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča HBase, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- [Sigurne Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
- [curl](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Preduvjeti za kontrolu pristupa

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Stvaranje HBase klaster

U nastavku koristi predložak Azure Voditelj resursa za stvaranje klaster sustavom Linux HBase verziju 3.4 i o njima ovisne zadani račun za Azure prostora za pohranu. Da biste shvatili parametri koji se koriste u postupku i drugih načina stvaranja klaster, potražite u članku [Stvaranje Linux sustavom Hadoop klastere u HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknite na sljedećoj slici da biste otvorili predložak na portalu za Azure. Predložak nalazi se u spremniku javno blob. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Iz plohu **implementacije Prilagođeno** unesite sljedeće:

    - **Pretplate**: Odaberite Azure pretplatu koja će se koristiti za stvaranje klaster.
    - **Grupa resursa**: Stvaranje nove grupe Upravljanje resursima Azure ili koristite postojeću.
    - **Lokacija**: Navedite mjesto na kojem se u grupu resursa. 
    - **ClusterName**: Unesite naziv klaster HBase koje ćete stvoriti.
    - **Klaster korisničko ime i lozinku**: ime za prijavu zadani je **administrator**.
    - **SSH korisničko ime i lozinku**: korisničko ime zadani je **sshuser**.  Možete je preimenovati.
     
    Drugi parametri nisu obavezni.  
    
    Svaki klaster ima ovisnost za računa spremišta blobova platforme Azure. Nakon što izbrišete klaster, podaci se zadržava u račun za pohranu. Naziv računa za pohranu klaster zadani je naziv klaster s "store" dodan. To je koji nisu u odjeljku predložak varijabli.
        
3. Odaberite **se slažete uvjete i odredbe naveden iznad**, a zatim **za kupnju**. Da biste stvorili klaster traje otprilike 20 minuta.


>[AZURE.NOTE] Nakon brisanja programa HBase klaster možete stvoriti drugi HBase klaster koristeći istu spremnik blob zadani. Novi klaster će obraditi HBase tablice koje ste stvorili u izvornom klaster. Da biste izbjegli nedosljednosti, preporučujemo da prije brisanja klaster onemogućite HBase tablice.

## <a name="create-tables-and-insert-data"></a>Stvaranje tablice i umetanje radnog lista

Koristite SSH za povezivanje s HBase klastere, a zatim pomoću ljuske HBase da biste stvorili HBase tablice, umetanje podataka i upita za podatke. Informacije o korištenju SSH, potražite u članku [Korištenje SSH s operacijskim sustavom Linux Hadoop na HDInsight Linux, Unix, ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md) i [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
 

Za većinu korisnika, prikaza podataka u tabličnom obliku:

![HDInsight HBase tablične podatke][img-hbase-sample-data-tabular]

U HBase koja je implementacija BigTable iste podatke izgleda:

![HDInsight HBase bigtable podataka][img-hbase-sample-data-bigtable]

Kada dovršite sljedeći postupak će vam tako biti više veza.  


**Da biste koristili HBase shell**

1. Iz SSH, pokrenite sljedeću naredbu:

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

    Dodatne informacije o shemi HBase tablice potražite u članku [Uvod u HBase sheme dizajn][hbase-schema]. Dodatne naredbe HBase, potražite u članku [vodič Apache HBase][hbase-quick-start].

6. Izlaz iz ljuske

        exit



**Da biste skupno učitavanja podataka u tablicu HBase kontakata**

HBase sadrži nekoliko metoda učitavanja podataka u tablice.  Dodatne informacije potražite u članku [Učitavanje skupno](http://hbase.apache.org/book.html#arch.bulk.load).


Datoteku s oglednim podacima prenesen spremniku javno blob *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  Sadržaj podatkovne datoteke je:

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

> [AZURE.NOTE] Ovaj postupak koristi tablici HBase kontakata koji ste stvorili u zadnji postupak.

1. Iz SSH, pokrenite sljedeću naredbu za pretvorbu podatkovna datoteka za web-mjesto StoreFiles i u okvir za spremište na relativni put određen Dimporttsv.bulk.output:.  Ako ste u ljusci HBase, upotrijebite naredbu izlaz za izlaz.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Pokrenite sljedeću naredbu da biste prenijeli podatke iz /example/data/storeDataFileOutput HBase tablicu:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Otvorite ljusku HBase i upotrijebite naredbu pregled da biste sadržaj tablice popisa.



## <a name="use-hive-to-query-hbase"></a>Korištenje grozd upit HBase

Pomoću grozd može poslati podatke u tablicama HBase. U ovom se odjeljku stvara tablicu vrste Hive karte HBase tablice i koristi za dohvaćanje podataka u tablici HBase.

1. Otvorite **PuTTY**i povezati klaster.  Slijedite upute u odjeljku prethodnog postupka.
2. Otvorite ljusku grozd.

       hive
3. Pokrenite sljedeću skriptu HiveQL da biste stvorili tablicu za vrste Hive mapira HBase tablice. Pripazite da ste stvorili ogledne tablice koje se pozivaju na nju ranije u ovom praktičnom vodiču pomoću ljuske HBase prije pokretanja izjavu o zaštiti.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. Pokrenite sljedeću skriptu HiveQL upit podatke u tablici HBase:

        SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>Korištenje HBase REST API-ji pomoću zakretanja

> [AZURE.NOTE] Kada koristite zakretanja ili OSTALE komunikacije s WebHCat, morate autentičnost zahtjeve unosom korisničko ime i lozinku za administratora klaster HDInsight. Naziv klaster morate koristiti i kao dio sustava na Uniform Resource Identifier (URI) koristiti za slanje zahtjeve na poslužitelj.
>
> Da biste naredbe u ovom odjeljku, zamijenite **korisničko ime** korisnika za provjeru autentičnosti klaster, i zamijenite **lozinku** lozinke za korisnički račun. Zamijenite **CLUSTERNAME** naziv svoj klaster.
>
> REST API-JA je osigurani putem [osnovnu provjeru autentičnosti](http://en.wikipedia.org/wiki/Basic_access_authentication). Uvijek trebali napraviti zahtjeva putem sigurne HTTP (HTTPS) da biste bili sigurni da vjerodajnica za sigurno šalju na poslužitelj.

1. Iz naredbenog retka, koristite sljedeću naredbu da biste provjerili možete li se povezati s svoj klaster HDInsight:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    Trebali biste dobiti odgovor sličnu ovoj:

        {"status":"ok","version":"v1"}

    Su parametri koji se koristi u tu naredbu na sljedeći način:

    * **-u** – korisničko ime i lozinku koje se koriste za provjeru autentičnosti zahtjev.
    * **-G** - označava da je zahtjevom GET.

2. Da biste dobili popis postojeće HBase tablice, koristite sljedeću naredbu:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. Da biste stvorili novu tablicu HBase s dva stupca linije, koristite sljedeću naredbu:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    U obliku JSon navedeni su u shemi.

4. Da biste umetnuli neke podatke, koristite sljedeću naredbu:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    Morate base64 kodiranje vrijednosti navedene u parametar -d.  U exmaple:

    - MTAwMA ==: 1000
    - UGVyc29uYWw6TmFtZQ ==: osobno: naziv
    - Sm9obiBEb2xl: Dole Nevena

    [FALSE, redak i ključa](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) omogućuje vam da biste umetnuli više vrijednosti (odbacivanja).

5. Da biste dobili redak, koristite sljedeću naredbu:

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

Dodatne informacije o ostalim HBase potražite u članku [Vodič za HBase Apache](https://hbase.apache.org/book.html#_rest).

## <a name="check-cluster-status"></a>Provjera statusa klaster

HBase u HDInsight dolazi s korisničkim Sučeljem Web za klastere za nadzor. Pomoću korisničkog Sučelja Web, možete zatražiti Statistika ili informacije o područja.

SSH može se koristiti i za Tunel lokalne zahtjeve, kao što je web-zahtjeva klaster HDInsight. Traženom resursu zatim usmjerena zahtjev kao da je prodao potječe na glavni čvor klaster HDInsight. Dodatne informacije potražite u članku [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Da biste uspostavili programa SSH tuneliranje sesiju**

1. Otvorite **PuTTY**.  
2. Ako ste unijeli ključa SSH prilikom stvaranja korisnički račun tijekom procesa stvaranja, morate izvršiti sljedeće korake da biste odabrali privatni ključ za provjeru autentičnosti skupine:

    U **kategoriji**, proširite **veza**, proširite **SSH**pa odaberite **provjere autentičnosti**. Na kraju, kliknite **Pregledaj** i odaberite .ppk datoteku koja sadrži privatni ključ.

3. U odjeljku **kategorije**kliknite **sesiju**.
4. Osnovni mogućnosti za vaš zaslon PuTTY sesiju unesite sljedeće vrijednosti:

    - **Naziv glavnog računala**: adresu SSH polja HDInsight server na naziv glavnog računala (ili IP adresa). Adresa SSH pa je naziv vaše klaster **-ssh.azurehdinsight.net**. Na primjer, *mycluster ssh.azurehdinsight.net*.
    - **Priključak**: 22. Na ssh priključak na primarni headnode je 22.  
5. U odjeljku **kategorija** na lijevoj strani dijaloškog okvira proširite **vezu**, proširite **SSH**, a zatim kliknite **Tunnels**.
6. Na izborniku Mogućnosti kontrola SSH priključak prosljeđivanje obrazac navedite sljedeće podatke:

    - **Izvorni Priključak** - priključak na klijentskom računalu koju želite proslijediti. Na primjer, 9876.
    - **Dinamični** - omogućuje dinamički SOCKS proxy usmjeravanja.
7. Kliknite **Dodaj** da biste dodali postavke.
8. Kliknite **Otvori** pri dnu dijaloškog okvira da biste otvorili vezu s SSH.
9. Kada se to od vas zatraži, prijavite se na poslužitelj pomoću računa sustava SSH. To će uspostaviti sesiju SSH i omogućite na tunelom.

**Da biste pronašli FQDN zookeepers pomoću Ambari**

1. Pronađite https://<ClusterName>.azurehdinsight.net/.
2. Dvaput unesite vjerodajnice klaster korisnički račun.
3. Na lijevom izborniku kliknite **zookeeper**.
4. Kliknite neku od tri **ZooKeeper poslužitelja** veze na popisu sažetka.
5. Kopirajte **naziv glavnog računala**. Na primjer, zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**Konfiguriranje klijentski program (Firefox) i provjera stanja klaster**

1. Otvorite Firefox.
2. Kliknite gumb **Otvori izbornik** .
3. Kliknite **Mogućnosti**.
4. Kliknite **Napredno**, kliknite na **mrežu**, a zatim **Postavke**.
5. Odaberite **ručno proxy konfiguracija**.
6. Unesite sljedeće vrijednosti:

    - **Socks glavnog računala**: localhost
    - **Priključak**: koristiti isti priključak ste konfigurirali u na Putty SSH tuneliranje.  Na primjer, 9876.
    - **SOCKS v5**: (odabrano)
    - **Udaljena DNS**: (odabrano)
7. Kliknite **u redu** da biste spremili promjene.
8. Pronađite http://&lt;u FQDN na ZooKeeper >: Matrica/60010 – statusa.

Klasteru visoke dostupnosti, tražit će vezu na trenutni aktivni HBase osnovne čvor koji se nalaze korisničkog Sučelja Web.

##<a name="delete-the-cluster"></a>Brisanje klaster

Da biste izbjegli nedosljednosti, preporučujemo da prije brisanja klaster onemogućite HBase tablice.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Daljnji koraci

U ovom vodiču HBase za HDInsight naučili kako stvoriti programa HBase klaster i kako stvoriti tablice i prikaz podataka u tim tablicama iz ljuske HBase. I saznali kako koristiti upit grozd na podatke u tablicama HBase i upute za korištenje HBase C# REST API-ji za stvaranje tablice programa HBase i dohvaćanje podataka iz tablice.

Da biste saznali više, pogledajte:

- [Pregled HDInsight HBase][hdinsight-hbase-overview]: HBase je Apache, Otvori izvor NoSQL baza podataka utemeljena na Hadoop koja omogućuje izravnim pristupom i istaknuti dosljednost velikih količina podataka nestrukturirane i semistructured.


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
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
