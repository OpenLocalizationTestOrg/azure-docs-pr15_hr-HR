<properties
   pageTitle="Povezivanje programa Excel s Hadoop s grozd ODBC upravljački program | Microsoft Azure"
   description="Upute za postavljanje i korištenje Microsoft vrste Hive ODBC upravljački program za Excel podatke upita programa klasteru HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

#<a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>Povezivanje programa Excel s Hadoop pomoću Microsoft vrste Hive ODBC upravljački program

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoftovi velikih skupova podataka rješenja komponente Microsoft poslovne inteligencije (BI) integrira Apache Hadoop klastere koje ste uveden Azure HDInsight. Primjer Ta integracija je mogućnost povezivanje Excel skladištu grozd podataka od programa Hadoop klaster u HDInsight pomoću upravljački program za Microsoft vrste Hive Open Database Connectivity (ODBC).

Također moguće je povezati podataka vezanih uz programa HDInsight klaster i drugih izvora podataka, uključujući druge klastere Hadoop (koji nisu HDInsight), iz programa Excel pomoću dodatka Microsoft Power Query za Excel. Informacije za instalaciju i korištenje dodatka Power Query potražite u članku [Povezivanje programa Excel sa servisa HDInsight pomoću značajke Power Query][hdinsight-power-query].

> [AZURE.NOTE] Dok se koraci u ovom članku može se koristiti s Linux ili utemeljen na sustavu Windows HDInsight klaster, za klijent radne stanice potreban je Windows.

**Preduvjeti**:

Prije nego počnete u ovom se članku, morate imati sljedeće:

- **Klaster programa HDInsight**. Da biste je stvorili, potražite u članku [Početak rada sa servisom Azure HDInsight][hdinsight-get-started].
- **Radne stanice A** za Office 2013 Professional Plus, Office 365 Pro Plus, samostalno izdanje programa Excel 2013 ili Office 2010 Professional Plus.


##<a name="install-microsoft-hive-odbc-driver"></a>Instalirajte Microsoft vrste Hive ODBC upravljački program

Preuzmite i instalirajte Microsoft vrste Hive ODBC upravljački program iz [Centra za preuzimanje][hive-odbc-driver-download].

Upravljački program moguće je instalirati na 32-bitnu ili 64-bitne verzije sustava Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 i Windows Server 2012 i jednostavan način veze sa servisom Azure HDInsight (verzija 1,6 i noviji) i Azure HDInsight Emulator (v.1.0.0.0 i novijim verzijama). Instalirajte na verziju koja odgovara verziji aplikacije gdje će pomoću ODBC upravljački program. Za ovaj vodič upravljački program koristit će se iz programa Office Excel.

##<a name="create-hive-odbc-data-source"></a>Stvaranje vrste Hive ODBC izvora podataka

Sljedeći koraci pokazati kako stvoriti vrste Hive ODBC izvor podataka.

1. Na sustavu Windows 8 ili Windows 10, pritisnite tipku s logotipom sustava Windows da biste otvorili na početni zaslon i upišite **izvora podataka**.
2. Kliknite **Postavljanje podataka ODBC izvora (32-bitni)** ili **Postavljanje ODBC izvori podataka (64-bitni)** ovisno o verziji sustava Office. Ako koristite Windows 7, odaberite **ODBC izvori podataka (32-bitni)** ili **ODBC izvori podataka (64-bitni)** s **Administrativni alati**. To će se pokrenuti i dijaloški okvir **ODBC izvora podataka** .

    ![Administrator za izvor podataka ODBC][img-hdi-simbahiveodbc-datasource-admin]

3. DNS-a korisnika, kliknite **Dodaj** da biste otvorili čarobnjak za **Stvaranje novog izvora podataka** .
4. Odaberite **Microsoft vrste Hive ODBC upravljački program**, a zatim kliknite **Završi**. To će se pokrenuti Microsoft vrste Hive ODBC upravljački program DNS dijaloški okvir **Postavljanje** .

5. Upišite ili odaberite sljedeće vrijednosti:

    Svojstvo|Opis
    ---|---
    Naziv izvora podataka|Dodijelite naziv s izvorom podataka
    Glavno računalo|Unesite &lt;HDInsightClusterName >. azurehdinsight.net. Na primjer, myHDICluster.azurehdinsight.net
    Priključak|Koristite <strong>443</strong>. (Priključak promijenjen iz 563 443.)
    Baze podataka|Koristite <strong>zadani</strong>.
    Vrsta poslužitelja grozd|Odabir <strong>vrste Hive poslužitelja 2</strong>
    Mehanizam|Odabir <strong>Servisa Azure HDInsight</strong>
    HTTP put|Ostavite prazno.
    Korisničko ime|Unesite korisničko ime korisnika za HDInsight klaster. To je korisničko ime koje je stvorio tijekom postupka dodjele klaster. Ako ste koristili mogućnost brzo stvaranje, korisničko ime zadani je <strong>administrator</strong>.
    Lozinke|Unesite lozinku za korisnika klaster HDInsight.
    </table>

    Postoje neke važne parametre morate imati na umu kada kliknete **Dodatne mogućnosti**:

    Parametar|Opis
    ---|---
    Pomoću Nativnog upita|Kada je odabrana, ODBC upravljački program će pokušati pretvoriti TSQL HiveQL. Moraju koristiti samo ako ste sigurni šalju čisto HiveQL izjave 100%. Prilikom povezivanja sa SQL Server ili baze podataka SQL Azure, ostavite je isključen.
    Dobivenih po blok redaka|Pri dohvaćanju velik broj zapisa usklađivanje taj parametar možda morati osigurati optimalnih izvedbe.
    Zadana duljina niza stupca, Duljina binarni stupac, a zatim skaliranje decimalni stupca|Vrste podataka duljine i precisions može utjecati na način na koji će podaci se vraćaju. Uzrokuju pogrešne podatke se vraćaju zbog gubitak točnost i/ili obrezivanje.


    ![Dodatne mogućnosti][img-HiveOdbc-DataSource-AdvancedOptions]

6. Kliknite **testiranje** da biste testirali izvora podataka. Kada je izvor podataka ispravno konfigurirano, prikazuje *TESTOVA USPJEŠNO DOVRŠENA!*.
7. Kliknite **u redu** da biste zatvorili dijaloški okvir testiranje. Novi izvor podataka mora biti naveden odmah **ODBC izvora podataka**.
8. Kliknite **u redu** da biste zatvorili čarobnjak.

##<a name="import-data-into-excel-from-hdinsight"></a>Uvoz podataka u programu Excel iz servisa HDInsight

Koraci u nastavku opisuju način za uvoz podataka iz tablicu vrste hive u radnu knjigu programa Excel pomoću ODBC izvor podataka koji ste stvorili u gore navedene korake.

1. Otvorite novu ili postojeću radnu knjigu u programu Excel.
2. Na kartici **Podaci** kliknite **Iz drugih izvora podataka**, a zatim **Iz čarobnjaka za povezivanje podataka** da biste pokrenuli **Čarobnjak za povezivanje podataka**.

    ![Čarobnjak za povezivanje podataka otvaranja][img-hdi-simbahiveodbc.excel.dataconnection]

3. Odaberite **ODBC DSN** kao izvor podataka, a zatim kliknite **Dalje**.
4. Iz ODBC izvora podataka, odaberite naziv izvora podataka koji ste stvorili u prethodnom koraku, a zatim kliknite **Dalje**.
5. Ponovno unesite lozinku za klaster u čarobnjaku, a zatim **Test** za konfiguraciju provjerite opet po potrebi.
6. Kliknite **u redu** da biste zatvorili dijaloški okvir testiranje.
7. Kliknite **u redu**. Pričekajte dijaloškom okviru **Odaberite bazu podataka i tablicu** da biste otvorili. To može potrajati nekoliko sekundi.
8. Odaberite tablicu koju želite uvesti, a zatim kliknite **Dalje**. *Hivesampletable* je tablicu vrste hive uzorka koji se isporučuju s klastere HDInsight.  Možete odabrati ako još niste stvorili. Dodatne informacije o Pokreni vrste Hive upita i stvaranje tablica grozd, pročitajte članak [Korištenje vrste Hive s HDInsight][hdinsight-use-hive].
8. Kliknite **Završi**.
9. U dijaloškom okviru **Uvoz podataka** možete promijeniti i naveli upit. Da biste to učinili, kliknite **Svojstva**. To može potrajati nekoliko sekundi.
10. Kliknite karticu **definicija** i dodati **200 ograničenje** u tekstnom okviru **tekst naredbe** grozd iskaza select. Izmjena će ograničiti skup vraćenih zapisa 200.

    ![Svojstva veze][img-hdi-simbahiveodbc-excel-connectionproperties]

11. Kliknite **u redu** da biste zatvorili dijaloški okvir svojstva veze.
12. Kliknite **u redu** da biste zatvorili dijaloški okvir **Uvoz podataka** .  
13. Ponovno unesite lozinku, a zatim kliknite **u redu**. U svega nekoliko sekundi prije nego što se podaci uvoze u Excel.

##<a name="next-steps"></a>Daljnji koraci

U ovom članku naučili kako koristiti Microsoft vrste Hive ODBC upravljački program za dohvaćanje podataka iz servisa HDInsight u programu Excel. Isto tako, možete dohvatiti podatke iz servisa HDInsight u SQL baze podataka. Također je moguće prenijeti podatke u servis za HDInsight. Da biste saznali više, pogledajte:

- [Analizirati podatke o kašnjenju leta pomoću HDInsight][hdinsight-analyze-flight-data]
- [Prijenos podataka HDInsight][hdinsight-upload-data]
- [Korištenje Sqoop s HDInsight] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png
