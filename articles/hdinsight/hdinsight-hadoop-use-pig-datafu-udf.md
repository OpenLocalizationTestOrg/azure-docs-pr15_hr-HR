<properties
pageTitle="Korištenje DataFu s Svinja na HDInsight"
description="Zbirka biblioteka za korištenje s Hadoop je DataFu. Saznajte kako možete koristiti DataFu s Svinja na svoj klaster HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="use-datafu-with-pig-on-hdinsight"></a>Korištenje DataFu s svinja na HDInsight

Zbirka Otvori izvor biblioteka za korištenje s Hadoop je DataFu. U ovom dokumentu ćete saznati kako koristiti DataFu na svoj klaster HDInsight te kako koristiti DataFu korisnički definirane funkcije (UDF) s Svinja.

##<a name="prerequisites"></a>Preduvjeti

* Azure pretplate.

* Azure HDInsight klaster (Linux ili Windows temelje)

* Osnovni poznavanje [pomoću Svinja na HDInsight](hdinsight-use-pig.md)

##<a name="install-datafu-on-linux-based-hdinsight"></a>Kliknite pločicu DataFu Linux temelje HDInsight

> [AZURE.NOTE] DataFu je instalirana verzija Linux temelje klastere 3.3 i noviji, a zatim na klastere utemeljen na sustavu Windows. To nije instaliran na Linux temelje klastere starija od 3,3.
>
> Ako koristite verziju Linux temelje klaster 3.3 ili noviji ili klaster utemeljen na sustavu Windows, možete preskočiti ovaj odjeljak.

DataFu možete preuzeti i instalirati Maven spremištu. Da biste dodali DataFu svoj klaster HDInsight, poduzmite sljedeće korake:

1. Povezivanje s svoj klaster sustavom Linux HDInsight pomoću SSH. Dodatne informacije o korištenju SSH s HDInsight potražite u nekom od sljedećih dokumenata:

    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight Linux, OS X i Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-unix.md)
    
2. Koristite sljedeću naredbu za preuzimanje datoteke posudu DataFu pomoću uslužni wget ili kopirajte i zalijepite vezu u pregledniku da biste započeli preuzimanje.

        wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar

3. Nakon toga Prenesite datoteku na zadani prostor za pohranu za svoj klaster HDInsight. To čini datoteku dostupnim sve čvorove u klasteru i ona će ostati u prostor za pohranu čak i ako izbrišete i ponovno stvoriti klaster.

        hdfs dfs -put datafu-1.2.0.jar /example/jars
    
    > [AZURE.NOTE] U primjeru iznad pohranjuje posudu u `wasbs:///example/jars` jer se taj imenik već postoji na klaster prostora za pohranu. Možete koristiti bilo koje mjesto na koje želite da se HDInsight klaster prostora za pohranu.

##<a name="use-datafu-with-pig"></a>Korištenje DataFu s Svinja

U ovom odjeljku pretpostavlja se upoznati s komponentom Svinja na HDInsight i pružaju samo latinica Svinja naredbe ne korake za njihovo korištenje s klaster. Dodatne informacije o korištenju Svinja s HDInsight potražite u članku [Korištenje Svinja s HDInsight](hdinsight-use-pig.md).

> [AZURE.IMPORTANT] Prilikom korištenja DataFu iz Svinja na sustavom Linux HDInsight klaster, prvo morate registrirati datoteku posudu pomoću sljedeće naredbe latinica Svinja:
>
> ```register wasbs:///example/jars/datafu-1.2.0.jar```
>
> DataFu je registrirana po zadanom na klastere HDInsight utemeljen na sustavu Windows.

Obično će definirati pseudonim za funkcije DataFu. Ako, na primjer:

    DEFINE SHA datafu.pig.hash.SHA();
    
Ovim se definira na pseudonim imenovani `SHA` za SHA raspršivanje (opis funkcije). Zatim koristite to latinica Svinja skripta za generiranje Raspršivanje za unos podataka. Na primjer, sljedeće zamjenjuje nazive ulaznih podataka s vrijednošću raspršivanje:

    raw = LOAD '/data/raw/' USING PigStorage(',') AS  
        (name:chararray, 
        int1:int, 
        int2:int,
        int3:int); 
    mask = FOREACH raw GENERATE SHA(name), int1, int2, int3; 
    DUMP mask;

Ako koristi se sa sljedećim podacima unosa:

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5
    
To će generirati sljedeći rezultat:

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

##<a name="next-steps"></a>Daljnji koraci

Dodatne informacije o DataFu ili Svinja potražite u članku sljedeće dokumente:

* [Vodič za DataFu Svinja Apache](http://datafu.incubator.apache.org/docs/datafu/guide.html).

* [Korištenje Svinja s HDInsight](hdinsight-use-pig.md)
