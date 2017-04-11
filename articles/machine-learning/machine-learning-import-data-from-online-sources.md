<properties
    pageTitle="Uvoz podataka u strojnog učenja Studio iz izvora podataka online | Microsoft Azure"
    description="Upute za uvoz podataka obuka Azure strojnog učenja Studio iz različitih izvora na Internetu."
    keywords="Uvoz podataka, oblik podataka, vrste podataka, izvora podataka, obuka podataka"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;garye" />


# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Uvoz podataka u Azure strojnog učenja Studio iz različitih izvora podataka za online pomoću modula za uvoz podataka

U ovom se članku opisuju podrška za uvoz online podataka iz različitih izvora i podatke koji su potrebni za premještanje podataka iz tih izvora u pokusa Azure strojnog učenja.

> [AZURE.NOTE] U ovom članku navedene opće informacije o [Uvoz podataka] [ import-data] modul. Detaljnije informacije o vrstama podataka možete pristupiti, oblike, Parametri i odgovore na najčešća pitanja potražite u modulu referenca temi za [Uvoz podataka] [ import-data] modul.

<!-- -->

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Uvod

Možete pristupiti podacima iz Studio za učenje Azure strojno iz jednog od nekoliko izvora podataka online pokrenutom vaše eksperiment pomoću [Uvoz podataka] [ import-data] modula:

- Web-URL putem HTTP
- Hadoop pomoću HiveQL
- Spremište blobova platforme Azure
- Tablica platforme Azure
- Azure SQL baze podataka ili SQL Server na Azure VM
- Baze podataka SQL Server lokalnog
- Davatelj usluga, trenutno OData sažetka sadržaja podataka
 
Tijek rada za vođenje eksperimenata u Azure strojnog učenja Studio sastoji se od povlačenjem – i – ispuštanje komponente na područje crtanja. Da biste pristupili izvore podataka online, dodavanje [Uvoz podataka] [ import-data] modul eksperiment, odaberite **izvor podataka**i navedite parametre potrebne za pristup podacima. Izvori podataka online koje su podržane su detaljni popis u tablici u nastavku. U ovoj su tablici navedene su i oblici datoteka podržani i parametrima koji se koriste za pristup podacima.

Imajte na umu da jer ovaj tečaj pristupa podataka dok se izvodi na eksperiment, je dostupna je samo u tom eksperiment. Za usporedbu, podataka koji je spremljeno u modulu skup podataka dostupni su za sve eksperiment u radnom prostoru.

> [AZURE.IMPORTANT] Trenutno, [Uvoz podataka] [ import-data] i [Izvoz podataka] [ export-data] moduli lakše čitali i pisali podataka samo iz stvoren pomoću model implementacije klasični Azure prostora za pohranu. Drugim riječima, nova vrsta poslovnog subjekta spremište blobova platforme Azure koja nudi razina pristupa tipkovni prostora za pohranu ili razina pristupa odlične za pohranu još nije podržana. 

> Općenito govoreći, sve poslovne kontakte Azure prostora za pohranu koji ste stvorili prije tu mogućnost servisa postala dostupna treba neizmijenjen. Ako morate stvoriti novi račun, odaberite **Klasični** za implementaciju model ili pomoću upravitelja resursa i za **vrstu računa**odaberite **opće namjene** umjesto **spremište blobova platforme**. 

> Dodatne informacije potražite u članku [spremište blobova platforme Azure: tipkovni i zanimljivih prostora za pohranu razine](../storage/storage-blob-storage-tiers.md).



## <a name="supported-online-data-sources"></a>Podržani izvori podataka online
Modul Azure strojnog učenja **Uvoz podataka** podržava sa sljedećim izvorima podataka:

Izvor podataka | Opis | Parametri |
---|---|---|
Web-URL putem HTTP-a |Čita podatke u vrijednosti odvojenih zarezom (CSV), vrijednosti odvojene zarezom (TSV), atribut odnosa oblik datoteke (ARFF) i oblika podršku vektorski strojeva (SVM – Svijetli), iz web-URL koji koristi HTTP | <b>URL</b>: određuje puni naziv datoteke, uključujući URL web-mjesto i naziv datoteke, s bilo kojeg nastavkom. <br/><br/><b>Oblikovanje podataka</b>: određuje jedan s podacima podržane oblike: CSV, TSV, ARFF ili SVM light. Ako podaci imaju zaglavlje retka, koristi se za dodjeljivanje naziva stupaca.|
Hdps HADOOP|Čita podatke iz raspodijeljeno prostor za pohranu u Hadoop. Odredite podatke koje želite pomoću HiveQL, jezika za upite SQL sviđa. HiveQL može se koristiti i prikupljati podatke i izvršiti filtriranje prije nego što dodate podatke strojnog učenja Studio podataka.|<b>Vrste Hive upita baze podataka</b>: određuje grozd upit koji se koristi za generiranje podataka.<br/><br/><b>Poslužitelj HCatalog URI</b> : naveden naziv svoj klaster koristeći oblik * &lt;naziva klaster&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop korisničko ime</b>: određuje Hadoop korisničko ime za dodjeljivanje klaster.<br/><br/><b>Lozinka Hadoop korisničkog računa</b> : određuje vjerodajnica koje se koriste prilikom dodjele resursa klaster. Dodatne informacije potražite u članku [Stvaranje Hadoop klastere u HDInsight](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Mjesto za izlazne podatke</b>: određuje hoće li se podaci spremaju u datotečni sustav Hadoop distributed (HDFS) ili Azure. <br/><ul>Ako spremate podatke u HDFS, navedite poslužitelj HDFS URI. (Obavezno upišite naziv klaster HDInsight bez prefiksom HTTPS://). <br/><br/>Izlazne podatke koje ste pohranili na Azure, navedite naziv računa Azure prostora za pohranu, tipkovni prečac za pohranu i naziv spremnika za pohranu.</ul>|
SQL baze podataka |Čitanje podataka koja je spremljena u bazi podataka Azure SQL ili u bazi podataka sustava SQL Server na Azure virtualnog računala. | <b>Naziv poslužitelja baze podataka</b>: određuje naziv poslužitelja na kojem se izvodi u bazi podataka.<br/><ul>U slučaju baze podataka SQL Azure unesite naziv poslužitelja koje generira. Obično je obrazac * &lt;generated_identifier&gt;. database.windows.net.* <br/><br/>U slučaju sustava SQL server hostirane na Azure virtualnog računala unesite *tcp:&lt;virtualnog računala DNS naziva&gt;, 1433*</ul><br/><b>Naziv baze podataka </b>: određuje naziv baze podataka na poslužitelju. <br/><br/><b>Naziv poslužitelja korisničkog računa</b>: određuje korisničko ime za račun s dozvolama za bazu podataka programa access. <br/><br/><b>Lozinka korisničkog računa poslužitelja</b>: određuje lozinku za korisnički račun.<br/><br/><b>Prihvati sve certifikat poslužitelja</b>: koristite tu mogućnost (manje siguran) ako želite preskočiti pregledavanje certifikatom web-mjesta prije čitanja podataka.<br/><br/><b>Upit za bazu podataka</b>: Unesite SQL naredbe kojim opisujete podatke koje želite pročitati.|
Lokalni SQL baze podataka |Čita podatke koji su pohranjenu u lokalni SQL baze podataka. | <b>Pristupnik podacima</b>: određuje naziv pristupnika za upravljanje podacima instaliran na računalu na kojem možete pristupati bazom podataka sustava SQL Server. Informacije o postavljanju pristupnika, potražite u članku [Izvedi napredne analize s Azure strojnog učenja pomoću podataka iz programa lokalnog sustava SQL server](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Naziv poslužitelja baze podataka</b>: određuje naziv poslužitelja na kojem se izvodi u bazi podataka.<br/><br/><b>Naziv baze podataka </b>: određuje naziv baze podataka na poslužitelju. <br/><br/><b>Naziv poslužitelja korisničkog računa</b>: određuje korisničko ime za račun s dozvolama za bazu podataka programa access. <br/><br/><b>Korisničko ime i lozinku</b>: kliknite <b>vrijednosti Enter</b> da biste unijeli vjerodajnice za bazu podataka. Možete koristiti integriranu provjeru autentičnosti sustava Windows ili SQL Server ovisno o tome kako je konfigurirano vaše lokalnog sustava SQL Server.<br/><br/><b>Upit za bazu podataka</b>: Unesite SQL naredbe kojim opisujete podatke koje želite pročitati.|
Tablica platforme Azure|Čita podatke iz tablice servisa Azure pohrane u.<br/><br/>Ako diskovni pročitati velikih količina podataka pomoću servisa Azure tablice. Omogućuje fleksibilna, koji nisu relacijski (NoSQL), massively skalabilni, jeftini i vrlo dostupan prostor za pohranu rješenja.| Mogućnosti **Uvoza podataka** mijenja se ovisno o tome pristupate javne podatke ili račun privatnih prostora za pohranu koji zahtijeva vjerodajnice za prijavu. To se određuje <b>Vrsta provjere autentičnosti</b> koja može imati vrijednost "PublicOrSAS" ili "Račun", od kojih svaka ima vlastiti skup parametara. <br/><br/><b>Javno ili zajednički pristup potpis (SAS) URI</b>: parametre su:<br/><br/><ul><b>Tablica URI</b>: određuje javno ili SAS URL za tablicu.<br/><br/><b>Određuje redaka radi pronalaženja naziva svojstava</b>: vrijednosti su <i>TopN</i> skeniranje navedeni broj redaka ili <i>ScanAll</i> da biste sve retke u tablici. <br/><br/>Ako su podaci istovrstan i predvidljivi, preporučuje se da odaberete *TopN* i unesite broj N. Za velike tablice, to može uzrokovati brže vremena za čitanje.<br/><br/>Ako podatke strukturirano je s skupa svojstva razlikuju se ovisno o dubine i položaj tablice, odaberite mogućnost *ScanAll* da biste pregledali sve retke. Na taj način integritet dobivene svojstvo i Pretvorba metapodataka.<br/><br/></ul><b>Račun za pohranu privatno</b>: parametre su: <br/><br/><ul><b>Naziv računa</b>: određuje naziv računa koji sadrži tablicu za čitanje.<br/><br/><b>Ključ računa</b>: određuje ključa za pohranu povezanu s računom.<br/><br/><b>Naziv tablice</b> : određuje naziv tablice koja sadrži podatke koje želite pročitati.<br/><br/><b>Reci koje treba tražiti nazivi svojstava</b>: vrijednosti su <i>TopN</i> skeniranje navedeni broj redaka ili <i>ScanAll</i> da biste sve retke u tablici.<br/><br/>Ako su podaci istovrstan i predvidljivi, preporučujemo da odaberete *TopN* i unesite broj N. Za velike tablice, to može uzrokovati brže vremena za čitanje.<br/><br/>Ako podatke strukturirano je s skupa svojstva razlikuju se ovisno o dubine i položaj tablice, odaberite mogućnost *ScanAll* da biste pregledali sve retke. Na taj način integritet dobivene svojstvo i Pretvorba metapodataka.<br/><br/>|
Spremište blobova platforme Azure | Čitanje podataka pohranjenih u servisu blobova platforme Azure pohrane, uključujući slike, nestrukturirane tekst ili binarnim podacima u.<br/><br/>Možete koristiti servis Blob javno ponudili podatke ili privatno pohrane podataka aplikacije. Podatke možete pristupiti s bilo kojeg mjesta putem HTTP ili HTTPS veze. | Mogućnosti u modulu za **Uvoz podataka** mijenja se ovisno o tome pristupate javne podatke ili račun privatnih prostora za pohranu koji zahtijeva vjerodajnice za prijavu. Time se određuje <b>Vrsta provjere autentičnosti</b> koja može imati vrijednost od "PublicOrSAS" ili "Računa".<br/><br/><b>Javno ili zajednički pristup potpis (SAS) URI</b>: parametre su:<br/><br/><ul><b>URI</b>: određuje javno ili SAS URL za spremište blobova.<br/><br/><b>Oblik datoteke</b>: određuje oblik podataka u servisu Blob. Podržani oblici su CSV, TSV i ARFF.<br/><br/></ul><b>Račun za pohranu privatno</b>: parametre su: <br/><br/><ul><b>Naziv računa</b>: određuje naziv računa koji sadrži blob koju želite pročitati.<br/><br/><b>Ključ računa</b>: određuje ključa za pohranu povezanu s računom.<br/><br/><b>Put do spremnik, direktorija, ili blob</b> : određuje naziv blob koji sadrži podatke koje želite pročitati.<br/><br/><b>Oblik datoteke blob</b>: određuje oblik podataka u servisu blob. Oblici podržanih su CSV TSV, ARFF, CSV s navedenom kodiranje i Excel. <br/><br/><ul>Ako je oblik CSV ili TSV, obavezno označava je li datoteka sadrži redak zaglavlja.<br/><br/>Mogućnosti programa Excel možete koristiti za čitanje podataka iz radnih knjiga programa Excel. Mogućnost <i>oblik podataka u Excel</i> označava je li podaci u rasponu radnog lista programa Excel ili u tablici programa Excel. U mogućnosti <i>lista programa Excel ili ugrađene tablice </i>Navedite naziv lista ili tablice koje želite pročitati iz.</ul><br/>|
Davatelj sažetaka sadržaja | Čita podatke iz podržani davatelj sažetaka sadržaja. Trenutno je podržano samo oblikovanje protokola Open Data (OData). | <b>Vrsta sadržaja podataka</b>: određuje oblik OData.<br/><br/><b>Izvorišni URL</b>: određuje cijeli URL sažetka sadržaja podataka. <br/>Na primjer, sljedeći URL čita iz oglednu bazu podataka Northwind: http://services.odata.org/northwind/northwind.svc/|


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
