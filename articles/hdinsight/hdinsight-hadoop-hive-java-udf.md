<properties
pageTitle="Korištenje Java korisnički definirane funkcije (UDF) s grozd u HDInsight | Microsoft Azure"
description="Saznajte kako stvoriti i koristiti Java korisnički definirane funkcije (UDF) iz grozd u HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/27/2016"
ms.author="larryfr"/>

#<a name="use-a-java-udf-with-hive-in-hdinsight"></a>Korištenje programa Java UDF s grozd u HDInsight

Grozd sjajan je za rad s podacima u HDInsight, ali ponekad morate dodatne Općenito svrhu jezik. Grozd omogućuje stvaranje korisnički definirane funkcije (UDF) pomoću raznih programskog jezika. U ovom dokumentu će Saznajte kako koristiti Java UDF grozd.

## <a name="requirements"></a>Preduvjeti

* Azure pretplate

* Za HDInsight klaster (Windows sustavom Linux ili)

    > [AZURE.NOTE] Većina korake u ovom dokumentu će raditi na obje vrste klaster; No korake koji se koristi za prijenos kompilirane UDF klaster, a zatim ga pokrenuti su specifične za klastere sustavom Linux. Veza se daju informacije koje je moguće koristiti s klastere utemeljen na sustavu Windows.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 ili noviji (ili ekvivalent, kao što je OpenJDK)

* [Apache Maven](http://maven.apache.org/)

* Uređivač teksta ili Java IDE

    > [AZURE.IMPORTANT] Ako koristite sustavom Linux HDInsight poslužitelj, ali stvaranje datoteka Python na klijentskom računalu Windows, morate koristiti uređivač koji koristi LF kao završetak crte. Ako niste sigurni je li uređivač koristi LF ili riječ o CRLF, u odjeljku [Otklanjanje poteškoća](#troubleshooting) upute o uklanjanju znak CR pomoću temeljenih na klaster HDInsight.

## <a name="create-an-example-udf"></a>Stvaranje primjera UDF

1. Iz naredbenog retka, koristite sljedeće da biste stvorili novi projekt Maven:

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] Ako koristite PowerShell, morate staviti ponuda oko parametre. Na primjer, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    To će stvoriti novi direktorij pod nazivom __exampleudf__, koje će sadržavati Maven projekta.

2. Nakon što projekt, izbrisati imenik __exampleudf i src/testiranje__ koja je stvorena u sklopu projekta; ne će se koristiti u ovom primjeru.

3. Otvorite __exampleudf/pom.xml__i zamijeniti postojeći `<dependencies>` stavku s na sljedeći način:

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    Te stavke navedite verziju Hadoop i grozd uključene 3,3 HDInsight i 3.4 klastere. Možete pronaći informacije o verzijama Hadoop i grozd dao HDInsight iz [HDInsight komponente verzijama](hdinsight-component-versioning.md) dokumenta.

    Dodavanje u `<build>` sekcije prije na `</project>` redak na kraj datoteke. U ovom se odjeljku mora sadržavati sljedeće:

        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                            </transformer>
                        </transformers>
                        <!-- Keep us from getting a bad signature error -->
                        <filters>
                            <filter>
                                <artifact>*:*</artifact>
                                <excludes>
                                    <exclude>META-INF/*.SF</exclude>
                                    <exclude>META-INF/*.DSA</exclude>
                                    <exclude>META-INF/*.RSA</exclude>
                                </excludes>
                            </filter>
                        </filters>
                    </configuration>
                    <executions>
                        <execution>
                            <phase>package</phase>
                            <goals>
                                <goal>shade</goal>
                            </goals>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
    
    Te stavke definirati sastavljanje projekta. Konkretno, verzija Java koji koristi projekta i sastavljanje programa uberjar za implementaciju sustava klaster.

    Spremite datoteku nakon što se učinjene promjene.

4. Preimenovanje __exampleudf/src/main/java/com/microsoft/examples/App.java__ __ExampleUDF.java__, a zatim otvorite datoteku u uređivaču.

5. Sadržaj datoteke __ExampleUDF.java__ zamijenite sljedećeg, a zatim spremite datoteku.

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    To implementira UDF koja prihvaća vrijednosti niza i vraća mala verzija niza.

## <a name="build-and-install-the-udf"></a>Stvaranje i instalirati na UDF

1. Kompiliranje i paket na UDF, koristite sljedeću naredbu:

        mvn compile package

    To će stvoriti, a zatim pakiranje na UDF u __exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__.

2. Korištenje na `scp` naredbu da biste kopirali datoteku klaster HDInsight.

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    Zamijenite __myuser__ SSH korisnički račun za svoj klaster. __Mycluster__ zamijenite nazivom klaster. Ako ste koristili lozinku radi zaštite računa SSH, zatražit će se da unesete lozinku. Ako ste koristili certifikat, možda ćete morati koristiti u `-i` parametar da biste odredili datoteka privatnog ključa.

3. Povezivanje s klaster pomoću SSH. 

        ssh myuser@mycluster-ssh.azurehdinsight.net

    Dodatne informacije o korištenju SSH sa servisa HDInsight potražite u članku sljedećim dokumentima.

    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight Linux, Unix ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

4. Iz sesije SSH kopirajte datoteku posudu HDInsight prostora za pohranu.

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>Korištenje UDF iz grozd

1. Koristite sljedeće da biste pokrenuli Beeline klijenta s SSH sesiju.

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Ta se naredba pretpostavlja da ste koristili zadani __administrator__ računa za prijavu za svoj klaster.

2. Kada dođete na na `jdbc:hive2://localhost:10001/>` upit, unesite sljedeće da biste dodali na UDF grozd i izlaganje kao funkciju.

        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. Koristite na UDF za pretvaranje vrijednosti dohvaća iz tablice u nizove malim slovima.

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    To će odaberite platforme uređaja (Android Windows, iOS, itd.) iz tablice, pretvoriti u nizu u mala slova, a zatim ih prikazati. Izlaz izgledat će otprilike ovako.

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Daljnji koraci

Drugi načini rada s grozd, potražite u članku [Korištenje vrste Hive s HDInsight](hdinsight-use-hive.md).

Dodatne informacije o funkcijama Hive User-Defined, potražite u članku [vrste Hive operatori i korisnički definirane funkcije](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) dio wiki grozd pri apache.org.
