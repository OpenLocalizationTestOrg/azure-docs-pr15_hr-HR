<properties 
   pageTitle="Korištenje Apache Phoenixu i SQuirreL u HDInsight | Microsoft Azure" 
   description="Saznajte kako koristiti Apache Phoenixu u HDInsight i kako instalirati i konfigurirati SQuirreL na vaše radne stanice za povezivanje programa klaster HBase u HDInsight." 
   services="hdinsight" 
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
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a>Korištenje Phoenixu Apache i SQuirreL s klastere utemeljen na sustavu Windows HBase u HDinsight  

Saznajte kako koristiti [Apache Phoenixu](http://phoenix.apache.org/) u HDInsight i kako instalirati i konfigurirati SQuirreL na vaše radne stanice za povezivanje programa klaster HBase u HDInsight. Dodatne informacije o Phoenixu potražite u članku [Phoenixu 15 minuta ili manje](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Gramatiku Phoenixu potražite u članku [Phoenixu gramatiku](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Phoenixu informacije o verziji u HDInsight, potražite u članku [što je novo u verzijama klaster Hadoop nudi HDInsight?] [hdinsight-versions].
>
> Informacije u ovom dokumentu je za klastere HDInsight utemeljen na sustavu Windows. Informacije o korištenju Phoenixu na sustavom Linux HDInsight potražite u članku [Korištenje Apache Phoenixu sa sustavom Linux HBase klastere u HDinsight](hdinsight-hbase-phoenix-squirrel-linux.md).

##<a name="use-sqlline"></a>Korištenje SQLLine
[SQLLine](http://sqlline.sourceforge.net/) je uslužni program naredbenog retka za izvođenje SQL naredbe. 

###<a name="prerequisites"></a>Preduvjeti
Prije korištenja SQLLine mora imati sljedeće:

- **A HBase klaster u HDInsight**. Dodatne informacije o dodjele resursa HBase skupine, pročitajte članak [Početak rada s Apache HBase u HDInsight][hdinsight-hbase-get-started].
- **Povezivanje s klaster HBase putem protokola udaljene radne površine**. Upute potražite u članku [Upravljanje Hadoop klastere u HDInsight pomoću portala za klasični Azure][hdinsight-manage-portal].

**Da biste saznali naziv glavnog računala**

1. Otvaranje **Hadoop naredbenog retka** na radnoj površini.
2. Pokrenite sljedeću naredbu da biste dobili DNS nastavak:

        ipconfig

    Zapišite **Sufiks DNS specifične za vezu**. Na primjer, *myhbasecluster.f5.internal.cloudapp.net*. Kada se povežete s programa HBase klaster, morat ćete povezati s nekom od Zookeepers pomoću FQDN. Svaki klaster HDInsight ima 3 Zookeepers. Su *zookeeper0*, *zookeeper1*i *zookeeper2*. FQDN bit će otprilike ovako *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.

**Da biste koristili SQLLine**

1. Otvaranje **Hadoop naredbenog retka** na radnoj površini.
2. Pokrenite sljedeće naredbe da biste otvorili SQLLine:

        cd %phoenix_home%\bin
        sqlline.py [The FQDN of one of the Zookeepers]

    ![hdinsight hbase Phoenixu sqlline][hdinsight-hbase-phoenix-sqlline]

    Naredbi koje se koriste u uzorku:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
        
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;

Dodatne informacije potražite u članku [ručno SQLLine](http://sqlline.sourceforge.net/#manual) i [Phoenixu gramatiku](http://phoenix.apache.org/language/index.html).


















##<a name="use-squirrel"></a>Korištenje SQuirreL

[Klijent za SQL sQuirreL](http://squirrel-sql.sourceforge.net/) je grafički program Java koji omogućuje prikaz strukture baze podataka JDBC usklađen pregledavanje podataka u tablicama, problema SQL naredbe itd. Može se koristiti za povezivanje s Apache Phoenixu na HDInsight.

U ovom se odjeljku objašnjava instaliranja i konfiguriranja SQuirreL na vaše radne stanice za povezivanje programa HBase klaster u HDInsight putem VPN-a. 

###<a name="prerequisites"></a>Preduvjeti

Prije nego što slijedite postupke, morate imati sljedeće:

- HBase klaster koji se implementiran na Azure virtualne mreže s DNS virtualnog računala.  Upute potražite u članku [Dodjeljivanje HBase klastere na Azure virtualne mreže][hdinsight-hbase-provision-vnet]. 

    >[AZURE.IMPORTANT] Morate instalirati DNS poslužitelj virtualne mreže. Upute potražite u članku [Konfiguriranje DNS između dvije Azure virtualne mreže](hdinsight-hbase-geo-replication-configure-dns.md)

- Pronađite HBase klaster klaster DNS specifične nastavak. Da biste ga RDP u klaster, a zatim pokrenite IPConfig.  Slično je DNS nastavak:

        myhbase.b7.internal.cloudapp.net
- Preuzmite i instalirajte [Microsoft Visual Studio Express 2013 za radnu površinu sustava Windows](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) na vaše radne stanice. Trebat će vam makecert iz paketa za stvaranje certifikata.  
- Preuzmite i instalirajte [Okruženje za izvođenje Java](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) na vaše radne stanice.  SQuirreL SQL klijent verzije 3.0 i noviji zahtijeva JRE verziju 1,6 ili noviji.  


###<a name="configure-a-point-to-site-vpn-connection-to-the-azure-virtual-network"></a>Konfiguriranje mjesta točke VPN vezu Azure virtualne mreže

Postoje uvrštene Konfiguriranje VPN veza točke web 3 koraka:

1. [Konfiguracija virtualne mreže i dinamični usmjeravanje pristupnika](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [Stvaranje vlastitih certifikata](#Create-your-certificates)
3. [Konfiguriranje VPN klijent](#Configure-your-VPN-client)

Dodatne informacije potražite u članku [Konfiguriranje mjesta točke VPN vezu Azure virtualne mreže](../vpn-gateway/vpn-gateway-point-to-site-create.md) .

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a>Konfiguracija virtualne mreže i dinamični usmjeravanje pristupnika

Bili ste dodjeli programa HBase klaster u Azure virtualne mreže (pogledajte preduvjete za ovaj odjeljak). Sljedeći je korak Konfiguriranje veze točke na web.

**Konfiguriranje povezivanja točke web**

1. Prijava na [Portal za Azure klasični][azure-portal].
2. Na lijevoj strani kliknite **MREŽE**.
3. Kliknite virtualne mreže koje ste stvorili (potražite u članku [Dodjeljivanje HBase klastere na Azure virtualne mreže][hdinsight-hbase-provision-vnet]).
4. Kliknite **KONFIGURIRAJ** od vrha.
5. U odjeljku **Povezivanje točke web** odaberite **Povezivanje Konfiguriraj točke na web**. 
6. Konfiguriranje **POČETNI IP** i **CIDR** da biste odredili rasponu IP adresa iz kojeg klijentima VPN primit će IP adresu nakon uspostave veze. Raspon se ne može preklapati s bilo kojeg od raspona koji se nalaze na lokalnu mrežu i Azure virtualne mreže će se povezujete s. Ako, na primjer. Ako ste odabrali 10.0.0.0/20 za virtualne mreže, možete odabrati 10.1.0.0/24 za prostor adrese klijenta. Potražite u članku [Povezivanje točke web] [ vnet-point-to-site-connectivity] stranice da biste saznali više.
7. U odjeljku razmake adresa virtualne mreže kliknite **Dodaj podmreže pristupnika**.
7. Kliknite **SPREMI** na dno stranice.
8. Kliknite **da** da biste potvrdili promjene. Pričekajte dok sustav Završi promjenom prije nego što nastavite sa sljedećim postupkom.


**Da biste stvorili dinamičnu pristupnik za usmjeravanje**

1. Na portalu klasični Azure kliknite **nadzorna PLOČA** s vrha stranice.
2. Kliknite **Stvaranje PRISTUPNIKA** na dno stranice.
3. Kliknite **da** da biste potvrdili. Pričekajte da se stvara pristupnika.
4. Kliknite **nadzorna PLOČA** s vrha.  Prikazat će se vizualni dijagram virtualne mreže:

    ![Azure virtualne mreže točke web virtualne dijagrama][img-vnet-diagram] 

    Dijagram prikazuje 0 klijentske veze. Nakon što ste napravili vezu virtualne mreže, broj će se ažurirati na jedan. 

#### <a name="create-your-certificates"></a>Stvaranje vlastitih certifikata

Da biste stvorili X.509 certifikat je pomoću certifikata stvaranje alata (makecert.exe) koji se isporučuju s [Microsoft Visual Studio Express 2013 za radnu površinu sustava Windows](https://www.visualstudio.com/products/visual-studio-express-vs.aspx). 


**Da biste stvorili korijenski samopotpisani certifikat**

1. Iz vaše radne stanice, otvorite prozor naredbenog retka.
2. Dođite do mape alata Visual Studio. 
3. Sljedeće naredbe u primjeru u nastavku će stvoriti i instalirati s korijenskim certifikatom u spremištu osobnih certifikata na vaše radne stanice i stvoriti odgovarajuće .cer datoteka koje će vam kasnije prenijeti na portalu klasični Azure. 

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    Promijenite u direktoriju koji želite .cer datoteka smjestiti, pri čemu je HBaseVnetVPNRootCertificate naziv koji želite koristiti za potvrdu. 

    Ne zatvorite naredbeni redak.  Potrebno je na sljedeći postupak.

    >[AZURE.NOTE] Budući da ste stvorili s korijenskim certifikatom iz kojeg će se generira klijentskih potvrda, trebali biste izvesti certifikat i privatni ključ i spremite je na sigurnom mjestu gdje je može obnoviti. 

**Stvaranje certifikata za klijenta**

- U isti naredbeni redak (ona mora biti na istom računalu na kojem ste stvorili korijenski certifikat. Klijentski certifikat mora biti generirao korijenski certifikat), pokrenite sljedeću naredbu:

        makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    HBaseVnetVPNRootCertificate je naziv certifikata korijen.  On se podudara s nazivom korijenski certifikat.  

    Korijenski certifikat i klijentski certifikat spremaju se u spremištu osobnih certifikata na vašem računalu. Koristite certmgr.msc da biste potvrdili.

    ![Certifikat za vpn točke web Azure virtualne mreže][img-certificate]

    Klijentski certifikat mora biti instaliran na svim računalima na kojima se želite povezati virtualne mreže. Preporučujemo vam da stvorite jedinstvene klijent certifikati za svim računalima na kojima se želite povezati virtualne mreže. Da biste izvezli klijentskih potvrda, koristite certmgr.msc. 

**Da biste prenijeli korijenski certifikat na portalu klasični Azure**

1. Na portalu klasični Azure kliknite **MREŽA** s lijeve strane.
2. Kliknite virtualne mreže na kojima je svoj klaster HBase implementiran na.
3. Kliknite **CERTIFIKATI** na vrhu.
4. Kliknite **PRENESI** od dna i navedite datoteka certifikata korijenski koji ste stvorili u postupku prije posljednje. Pričekajte da imate uvesti certifikat.
5. Kliknite **nadzorna PLOČA** na vrhu.  Virtualna dijagram prikazuje status.


#### <a name="configure-your-vpn-client"></a>Konfiguriranje VPN klijent



**Da biste preuzeli i instalirali paket za VPN klijentskog**

1. Na nadzornoj ploči stranice virtualne mreže, u odjeljku brzi pogled kliknite neki **preuzmite paket za VPN klijentskog 64-bitne** ili **preuzmite paket za VPN klijentskog 32-bitne** na temelju vaše radne stanice verzija OS-a.
2. Kliknite **Pokreni** da biste instalirali paket.
3. Kada se zatraži sigurnost, kliknite **Dodatne informacije**, a zatim **pokrenite ipak**.
4. Dvaput kliknite **da** .

**Povezivanje s VPN-a**

1. Na radnoj površini vaše radne stanice, kliknite ikonu mreža na programskoj traci. Prikazat će VPN vezu s vašim imenom virtualne mreže.
2. Kliknite naziv VPN veza.
3. Kliknite **Poveži**.

**Da biste testirali VPN vezu te domene naziv razlučivosti**

- Iz radne stanice, otvorite naredbeni redak i jedan od sljedećih naziva dali HBase klaster DNS sufiks je myhbase.b7.internal.cloudapp.net:

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

###<a name="install-and-configure-squirrel-on-your-workstation"></a>Instaliranje i konfiguriranje SQuirreL na vaše radne stanice

**Da biste instalirali SQuirreL**

1. Preuzmite datoteku posudu SQuirreL SQL klijenta s [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).
2. Otvori/Izvedi posudu datoteku. Potrebna je [Java Runtime okruženje](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).
3. Kliknite **Dalje** dvaput.
4. Odredite put na kojem imate dozvolu za pisanje, a zatim kliknite **Dalje**.
    >[AZURE.NOTE] Zadana mapa za instalaciju je u mapu C:\Program Files\squirrel-sql-3,6.  Da biste napisali ovaj put, instalacijski program mora biti odobren administratorske ovlasti. Otvorite naredbeni redak kao administrator, idite na Java smeće mapu i pokrenite 
    >
    >     java.exe -jar [the path of the SQuirreL jar file] 
5. Kliknite **u redu** da biste potvrdili stvaranje direktorija cilj.
6. Zadana je postavka da biste instalirali osnovni i standardne paketa.  Kliknite **Dalje**.
7. Dvaput kliknite **Dalje** , a zatim kliknite **gotovo**.


**Da biste instalirali Phoenixu upravljački program**

Datoteka za posudu Phoenixu upravljački program nalazi se na HBase klaster. Put je slično sljedećem na temelju verzije:

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
Morate kopirati vaše radne stanice, u odjeljku [SQuirreL instalacijsku mapu sustava] / biblioteka put.  Najlakši način je RDP u klaster, a zatim pomoću datoteke kopirajte i zalijepite (CTRL + C i CTRL + V) da biste ga kopirali vaše radne stanice.

**Da biste dodali upravljački program za Phoenixu SQuirreL**

1. Otvorite klijent za SQL SQuirreL iz vaše radne stanice.
2. Kliknite karticu **upravljački program** na lijevoj strani.
2. Na izborniku **upravljačke programe** kliknite **Novi upravljački program**.
3. Unesite sljedeće podatke:

    - **Naziv**: Phoenixu
    - **Primjer URL**: jdbc:phoenix:zookeeper2.contoso hbase eu.f5.internal.cloudapp.net
    - **Naziv klase**: org.apache.phoenix.jdbc.PhoenixDriver

    >[AZURE.WARNING] Korisnik sve mala slova u primjeru URL-u. Možete koristiti one kvorum puno zookeeper u slučaju da jedan od njih je prema dolje.  Na hostnames su zookeeper0, zookeeper1 i zookeeper2.

    ![Upravljački program za HDInsight HBase Phoenixu SQuirreL][img-squirrel-driver]
4. Kliknite **u redu**.

**Da biste stvorili pseudonima HBase klaster**

1. SQuirreL, kliknite karticu **pseudonima** na lijevoj strani.
2. Na izborniku **pseudonima** kliknite **Novi pseudonim**.
3. Unesite sljedeće podatke:

    - **Naziv**: naziv klaster HBase ili bilo koji naziv koji želite.
    - **Upravljački program**: Phoenixu.  To mora odgovarati naziv upravljački program koji ste stvorili u zadnji postupak.
    - **URL**: URL se kopiraju iz konfiguraciju upravljački program. Provjerite je li korisniku sve malim slovima.
    - **Korisničko ime**: može biti bilo koji tekst.  Budući da ovdje koriste grupa podaci, korisničko ime ne obavlja uopće.
    - **Lozinka**: može biti bilo koji tekst.

    ![Upravljački program za HDInsight HBase Phoenixu SQuirreL][img-squirrel-alias]
4. Kliknite **Testiraj**. 
5. Kliknite **Poveži**. Kada će se veza, SQuirreL izgleda:

    ![HBase Phoenixu SQuirreL][img-squirrel]

**Da biste pokrenuli test**

1. Kliknite karticu **SQL** odmah pokraj kartica **objekata** .
2. Kopirajte i zalijepite sljedeći kod:

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. Kliknite gumb za pokretanje.

    ![HBase Phoenixu SQuirreL][img-squirrel-sql]
4. Prijeđite na karticu **objekata** .
5. Proširite drugo ime, a zatim proširite **tablicu**.  Prikazat će nove tablice u odjeljku.
 
##<a name="next-steps"></a>Daljnji koraci
U ovom se članku ste naučili kako koristiti Apache Phoenixu u HDInsight.  Da biste saznali više, pročitajte članak

- [Pregled HDInsight HBase][hdinsight-hbase-overview]: HBase je Apache, Otvori izvor NoSQL baza podataka utemeljena na Hadoop koja omogućuje izravnim pristupom i istaknuti dosljednost velikih količina podataka nestrukturirane i semistructured.
- [Dodjela resursa za klastere HBase na Azure virtualne mreže][hdinsight-hbase-provision-vnet]: uz integraciju virtualne mreže klastere HBase može uvesti u istom virtualne mreže kao aplikacija bi aplikacija možete komunicirati s HBase izravno.
- [Konfiguriranje HBase replikacije u HDInsight](hdinsight-hbase-geo-replication.md): Saznajte kako konfigurirati replikacije HBase preko dva Azure podatkovnim centrima. 
- [Analiza Twitter šalju s HBase u HDInsight][hbase-twitter-sentiment]: Saznajte kako u stvarnom vremenu [šalju analiza](http://en.wikipedia.org/wiki/Sentiment_analysis) velikih skupova podataka pomoću HBase klasteru Hadoop u HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 
