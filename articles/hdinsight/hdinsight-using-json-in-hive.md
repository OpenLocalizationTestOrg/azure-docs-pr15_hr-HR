<properties
   pageTitle="Analizirati i proces JSON dokumente s grozd u HDInsight | Microsoft Azure"
   description="Saznajte kako koristiti dokumente JSON i analizirati ih pomoću grozd u HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/22/2015"
   ms.author="rashimg"/>


# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Postupak i analizirati JSON dokumenata pomoću grozd u HDInsight

Saznajte kako se postupak i analizirati datoteke JSON pomoću grozd u HDInsight. U ovom praktičnom vodiču koristit će sljedeći JSON dokument

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Datoteku možete pronaći na wasbs://processjson@hditutorialdata.blob.core.windows.net/. Dodatne informacije o korištenju spremište blobova platforme Azure s HDInsight potražite u članku [Korištenje HDFS kompatibilnog Azure blobova s Hadoop u HDInsight](hdinsight-hadoop-use-blob-storage.md). Ako želite, zadani spremnik svoj klaster možete kopirati datoteku.

U ovom ćete praktičnom vodiču će koristiti grozd konzolu.  Upute za otvaranje konzole grozd, potražite u članku [Korištenje vrste Hive s Hadoop na HDInsight putem udaljene radne površine](hdinsight-hadoop-use-hive-remote-desktop.md).

##<a name="flatten-json-documents"></a>Stopi JSON dokumenata

Načini naveden u sljedećem odjeljku zahtijevaju JSON dokument u jednom retku. Stoga morate stopi JSON dokumenta u niz. Ako već plošna je dokumentu JSON, možete preskočiti ovaj korak i Idite izravno u sljedećem odjeljku JSON analiza podataka.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Neobrađenog JSON datoteka se nalazi u **wasbs://processjson@hditutorialdata.blob.core.windows.net/**. Tablicu vrste Hive *StudentsRaw* upućuje na neobrađenog nepotvrđen plošnu JSON dokumenta.

Tablicu vrste Hive *StudentsOneLine* će spremiti podatke u HDInsight zadani datotečni sustav pod put */json/učenici /* .

Naredba za umetanje popunjavanje tablice StudentOneLine plošnu JSON podacima.

Naredba SELECT će vratiti samo 1 redak.

Evo izlaz iskaza SELECT.

![Stapanja JSON dokumenta.][image-hdi-hivejson-flatten]

##<a name="analyze-json-documents-in-hive"></a>Analiza JSON dokumenata u grozd

Grozd pruža tri različite mehanizme za izvođenje upita na JSON dokumenata:

- Korištenje DOHVATI\_JSON\_OBJEKT UDF (korisnički definirana funkcija)
- Korištenje JSON_TUPLE UDF
- korištenje prilagođenih SerDe
- pisanje vlasnik UDF pomoću Python ili druge jezike. Pročitajte [Ovaj članak] [ hdinsight-python] o pokretanju Python kod s grozd.

### <a name="use-the-getjsonobject-udf"></a>Korištenje DOHVATI\_JSON_OBJECT UDF
Grozd nudi ugrađene UDF nazivaju [se json objekt](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) koji izvršava JSON upita tijekom vremena izvođenja. Ovaj postupak vodi dva argumenta – naziva tablice i prema nazivu metode koja ima plošnu JSON dokument i JSON polja koje je potrebno mogu raščlaniti. Pogledajmo primjer da biste vidjeli kako funkcionira ova UDF.

Ime i prezime za svakog učenika

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Evo izlaz kada se pokrene ovaj upit u prozor konzole.

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

Postoji nekoliko ograničenja UDF get-json_object.

- Svako polje u upitu zahtijeva ponovno Raščlanjivanje upit, utječe na performanse.
- Početak\_JSON_OBJECT() vraća niz prikaz polja. Da biste pretvorili to grozd polja, morat ćete koristiti regularne izraze da biste zamijenili uglate zagrade ' [' i '] "i i nazovite Podijeli da biste dobili polja.


To je Zašto wiki grozd preporučuje korištenje json_tuple.  

### <a name="use-the-jsontuple-udf"></a>Korištenje JSON_TUPLE UDF

Drugi UDF nudi grozd zove [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple) koji se izvodi bolje [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Ovaj postupak vodi skup tipke i niz JSON i vraća n-torke vrijednosti pomoću funkcije jedan. Sljedeći upit vraća id učenika i kategorija iz dokumenta JSON:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

Izlaz ovu skriptu na konzoli grozd:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_n-TORKE koristi sintaksu [lateral prikaz](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) u grozd koji omogućuje json\_n-torke da biste stvorili virtualne tablicu tako da primijenite funkciju UDT za svaki redak izvorne tablice.  Složena JSONs postaju previše unwieldy zbog koji se ponavljaju korištenja LATERAL PRIKAZA. Osim toga, JSON_TUPLE ne prepoznaje ugniježđene JSONs.


###<a name="use-custom-serde"></a>Korištenje prilagođenih SerDe

SerDe je najbolji odabir za Raščlanjivanje dokumenata ugniježđene JSON, omogućuje definiranje shemi JSON i korištenje shemi raščlaniti dokumente. U ovom ćete praktičnom vodiču će koristiti jedan od češće SerDe koji sadrži [rcongiu](https://github.com/rcongiu)razvio.

**Da biste koristili prilagođeni SerDe:**

1. Instalacija [programskog jezika Java Jugoistočna Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Odaberite Windows X64 verziju na JDK ako namjeravate koristiti za implementaciju sustava Windows HDInsight

    >[AZURE.WARNING] JDK 1.8 ne funkcionira s ovom SerDe.

    Kada se instalacija završi, dodajte novi varijablu okruženja korisnika:

    1. Otvorite **Prikaz dodatne postavke sustava** na zaslonu sustava Windows.
    2. Kliknite **varijable okruženja**.  
    3. Dodajte novi varijabla okruženja **JAVA_HOME** pokazuje **C:\Program Files\Java\jdk1.7.0_55** ili mjestu na kojem je instaliran na JDK.

    ![Postavljanje vrijednosti ispravno config JDK][image-hdi-hivejson-jdk]

2. Instalacija [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)

    Smeće mapu dodali u vašem putu tako da odete na upravljačkoj ploči--> Uređivanje varijable sustava za vaš račun Environment varijabli. Snimka zaslona koja se nalazi ispod prikazuje kako to učiniti.

    ![Postavljanje Maven][image-hdi-hivejson-maven]

3. Kloniraj projekta s [Grozd JSON SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github web-mjesta. To možete učiniti tako da kliknete gumb "Preuzimanje Zip" kao što je prikazano u nastavku snimku zaslona.

    ![Kloniranje projekta][image-hdi-hivejson-serde]

4: Idi u mapu koju ste preuzeli ovaj paket i vrsta "package mvn". To potrebno stvoriti potrebne posudu datoteke koje možete kopirati iznad da biste klaster.

5: idite na ciljne mape u odjeljku korijensku mapu koju ste preuzeli paket. Prijenos datoteke json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar glave čvor svoj klaster-a. Se obično staviti u mapi binarni grozd: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin ili nešto slično.

6: u grozd naredbeni redak upišite "Dodavanje posudu /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar". Budući da u slučaju, posudu nalazi u mapi C:\apps\dist\hive-0.13.x\bin, možete izravno dodati posudu s nazivom kao što je prikazano u nastavku:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

    ![Adding JAR to your project][image-hdi-hivejson-addjar]

Sada ste spremni za korištenje na SerDe za izvoditi upite na dokument JSON.

Sljedeća naredba stvorite tablicu s definirani sheme

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

Da biste dobili popis imena i prezimena učenika

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Evo i rezultata s konzole grozd.

![Upit SerDe 1][image-hdi-hivejson-serde_query1]

Da biste izračunali zbroj brojčane pokazatelje JSON dokumenta

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

Upit iznad koristi [lateral prikaz razrezivanje](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF da biste proširili polja brojčane pokazatelje tako da ih možete obuhvaća.

Evo Izlaz iz konzole za grozd.

![Upit SerDe 2][image-hdi-hivejson-serde_query2]

Da biste pronašli odaberite koje predmeta navedeni učenika sadrži testu dobije više od 80  
      JT. StudentClassCollection.ClassId iz json_table jt lateral prikaz razrezivanje (jt. Zbirka StudentClassCollection.Score) kao rezultat gdje rezultatu > 80;

Iznad upit vraća grozd polja za razliku od get\_json\_objekt koji vraća niz.

![Upit SerDe 3][image-hdi-hivejson-serde_query3]

Ako želite skil oblikovan JSON, a zatim kao što je opisano u [wiki stranica](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) ova SerDe možete koji postići tako da upišete kod u nastavku:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




##<a name="summary"></a>Sažetak
Vrste operatora JSON u grozd koje ste odabrali da zaključimo, ovisno o scenariju. Ako imate jednostavne JSON dokument, a imate samo jedno polje za pretraživanje na – možete odabrati da biste koristili funkciju Dohvati vrste Hive UDF\_json\_objekt. Ako imate više od jedne tipke da biste potražili možete koristiti json_tuple. Ako imate ugniježđene dokumenta, trebali biste koristiti JSON SerDe.

Ostali povezani članci potražite u članku

- [Pomoću grozd i HiveQL Hadoop u HDInsight da biste analizirali oglednu datoteku za log4j Apache](hdinsight-use-hive.md)
- [Analizirajte podatke o kašnjenju leta pomoću grozd u HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Analiziranje podataka Twitter pomoću grozd u HDInsight](hdinsight-analyze-twitter-data.md)
- [Pokrenuti posao Hadoop pomoću DocumentDB i HDInsight](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
