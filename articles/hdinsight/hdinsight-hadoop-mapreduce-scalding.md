<properties
 pageTitle="Razvoj Scalding MapReduce poslove s Maven | Microsoft Azure"
 description="Saznajte kako koristiti Maven stvaranje Scalding MapReduce posao, a zatim implementacije i Pokreni na Hadoop na HDInsight klaster."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
    tags="azure-portal"/>
<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="10/18/2016"
 ms.author="larryfr"/>

# <a name="develop-scalding-mapreduce-jobs-with-apache-hadoop-on-hdinsight"></a>Razvoj Scalding MapReduce poslove s Apache Hadoop na HDInsight

Scalding je Scala biblioteke koja olakšava stvaranje Hadoop MapReduce zadatke. Nudi kratka sintaksu, kao i čvrsto Integracija s Scala.

U ovom dokumentu, Saznajte kako koristiti Maven da biste stvorili posao MapReduce count osnovni riječi pisane Scalding. Zatim ćete saznati kako uvesti i pokrenuti ovaj zadatak programa klaster HDInsight.

## <a name="prerequisites"></a>Preduvjeti

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Windows odgovora i Linux temelji Hadoop na klasteru HDInsight**. Dodatne informacije potražite [sustavom Linux dodjele resursa Hadoop na HDInsight](hdinsight-hadoop-provision-linux-clusters.md) ili [utemeljen na sustavu Windows dodjele resursa Hadoop na HDInsight](hdinsight-provision-clusters.md) .

* **[Maven](http://maven.apache.org/)**

* **[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 ili noviji**

## <a name="create-and-build-the-project"></a>Stvaranje i stvaranje projekta

1. Da biste stvorili novi projekt Maven, koristite sljedeću naredbu:

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    Ta se naredba će stvoriti novi direktorij pod nazivom **scaldingwordcount**i stvaranje scaffolding programa Scala.

2. U direktoriju **scaldingwordcount** , otvorite datoteku **pom.xml** i zamijeniti sadržaj sljedeće:

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
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
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    Tu datoteku opisuju projekt, ovisnosti te dodataka. Ovo su važne stavke:

    * **maven.Compiler.Source** i **maven.compiler.target**: postavlja verziju jezika Java za taj projekt

    * **spremištima**: spremištima koje sadrže ovisnost datoteke koje se koristi za taj projekt

    * **scalding core_2.11** i **hadoop core**: taj projekt ovisi o Scalding i Hadoop core paketa

    * **maven scala dodatak**: dodatak za Kompiliranje scala aplikacije

    * **maven Nijansa dodatak**: dodatak da biste stvorili osjenčan staklenke (fat). Ovaj dodatak primjenjuje se filtri i transformacije; specificially:

        * **Filtri**: primijenjenim filtrima izmjene informacija meta uključene u datoteci posudu. Da biste spriječili potpisivanja iznimke prilikom izvođenja, to isključuje različitih datoteka potpis koji se isporučuje se uz ovisnosti.

        * **izvršavanja**: Konfiguriranje izvođenja za faza paketa određuje klasu **com.twitter.scalding.Tool** kao glavni predmete paketa. Bez, trebat ćete odrediti com.twitter.scalding.Tool, kao i klasa koja sadrži logiku aplikacije prilikom pokretanja posla pomoću naredbe hadoop.

3. Brisanje direktorija **src i testiranje** kao koji će neće stvoriti testira s ovom primjeru.

4. Otvorite datoteku **src/main/scala/com/microsoft/example/App.scala** i zamijeniti sadržaj sljedeće:

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    To implementira posao count osnovni word.

5. Spremite i zatvorite datoteke.

6. Koristite sljedeću naredbu iz imenika **scaldingwordcount** omogućuje stvaranje i paket aplikacije:

        mvn package

    Kada se dovrši posao, paket koji sadrži aplikacije WordCount možete pronaći na **target/scaldingwordcount-1.0-SNAPSHOT.jar**.

## <a name="run-the-job-on-a-linux-based-cluster"></a>Pokreni na klasteru sa sustavom Linux

> [AZURE.NOTE] Sljedeći koraci pomoću SSH i naredbu Hadoop. Druge načine izvođenje zadataka MapReduce potražite u članku [Korištenje MapReduce u Hadoop na HDInsight](hdinsight-use-mapreduce.md).

1. Da biste prenijeli paket na svoj klaster HDInsight, koristite sljedeću naredbu:

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    To čvor glavni kopira datoteka s lokalnog sustava.

    > [AZURE.NOTE] Ako ste koristili lozinku radi zaštite računa SSH, zatražit će se za lozinku. Ako ste koristili ključa SSH, možda ćete morati koristiti u `-i` parametar i put do privatni ključ. Na primjer,`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. Povezivanje s čvor glavni klaster, koristite sljedeću naredbu:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Ako ste koristili lozinku radi zaštite računa SSH, zatražit će se za lozinku. Ako ste koristili ključa SSH, možda ćete morati koristiti u `-i` parametar i put do privatni ključ. Na primjer,`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. Nakon uspostave čvor glavni, koristite sljedeću naredbu da biste pokrenuli posao Brojanje riječi

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    To će pokrenuti klase WordCount ste implementirali neke starije verzije. `--hdfs`upućuje zadatak da biste koristili HDFS. `--input`Određuje unos tekstne datoteke dok `--output` određuje mjesto za izlazne podatke.

4. Nakon dovršetka posla, koristite sljedeće da biste prikazali izlaz.

        hdfs dfs -text wasbs:///example/wordcountout/*

    Time će se prikazati informacije sličnu ovoj:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="run-the-job-on-a-windows-based-cluster"></a>Pokreni na klaster utemeljen na sustavu Windows

Sljedeći koraci pomoću komponente Windows PowerShell. Druge načine izvođenje zadataka MapReduce potražite u članku [Korištenje MapReduce u Hadoop na HDInsight](hdinsight-use-mapreduce.md).

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. Pokrenite Azure PowerShell i Prijava na račun za Azure. Nakon unošenja vjerodajnice, naredba vraća podatke o vašem računu.

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. Ako imate više pretplata, unesite id pretplate koju želite koristiti za implementaciju.

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] Možete koristiti `Get-AzureRMSubscription` da biste dobili popis svih pretplata povezanu s računom koji sadrži Id pretplate za svaku grupu.

4. Koristite sljedeću skriptu da biste prenijeli i Pokreni WordCount. Zamjena `CLUSTERNAME` s nazivom svoje HDInsight skupine i provjerite je li `$fileToUpload` je točan put do datoteke __scaldingwordcount 1.0 SNAPSHOT.jar__ .

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = Read-Host -Prompt "Enter the HDInsight cluster name"
        $fileToUpload = Read-Host -Prompt "Enter the path to the scaldingwordcount-1.0-SNAPSHOT.jar file"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential -Message "Enter the login credentials for the cluster"
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                        "--input", `
                        "wasbs:///example/data/gutenberg/davinci.txt", `
                        "--output", `
                        "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     Kada pokrenete skriptu, zatražit će se da biste unijeli administrator korisničko ime i lozinku za svoj klaster HDInsight. Sve pogreške koje se pojavljuju dok se izvodi posao zapisat će se na konzolu.
     
6. Nakon dovršetka posla izlaz će se preuzeti datoteku __output.txt__ u trenutnom direktoriju. Koristite sljedeću naredbu za prikaz rezultata.

        cat output.txt

    Datoteka mora sadržavati vrijednosti sličnu ovoj:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili kako koristiti Scalding da biste stvorili MapReduce zadacima za HDInsight, koristite sljedeće veze da biste istražili druge načine za rad sa servisom Azure HDInsight.

* [Korištenje grozd s HDInsight](hdinsight-use-hive.md)

* [Korištenje Svinja s HDInsight](hdinsight-use-pig.md)

* [Korištenje MapReduce poslove s HDInsight](hdinsight-use-mapreduce.md)
