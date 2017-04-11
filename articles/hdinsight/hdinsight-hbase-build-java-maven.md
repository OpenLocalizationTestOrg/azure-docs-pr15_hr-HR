<properties
pageTitle="Stvaranje aplikacije HBase korištenjem Maven i implementacija utemeljen na sustavu Windows HDInsight | Microsoft Azure"
description="Saznajte kako koristiti Apache Maven za sastavljanje utemeljena na HBase Apache aplikacije, a zatim implementacija klaster utemeljen na sustavu Windows Azure HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"
tags="azure-portal"/>

<tags
ms.service="hdinsight"
ms.workload="big-data"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Kako koristiti Maven za izradu Java aplikacije koje koriste HBase s utemeljen na sustavu Windows HDInsight (Hadoop)

Saznajte kako stvoriti i pomoću Apache Maven izraditi [Apache HBase](http://hbase.apache.org/) aplikacije u Java. Zatim koristite aplikaciju sa servisom Azure HDInsight (Hadoop).

[Maven](http://maven.apache.org/) je softver upravljanje projektima i razumijevanje alat koji omogućuje sastavljanje softver, dokumentaciju i izvješća za Java projekte. U ovom se članku saznat ćete kako se koristi za stvaranje osnovne Java aplikacije koje koji stvara upita, a briše tablice programa HBase na programa Azure HDInsight klaster.

> [AZURE.NOTE] U ovom dokumentu pretpostavlja da koristite utemeljen na sustavu Windows HDInsight klaster. Informacije o korištenju sustavom Linux HDInsight klaster, potražite u članku [Korištenje Maven da biste sastavili Java aplikacije koje koriste HBase sa sustavom Linux HDInsight](hdinsight-hbase-build-java-maven-linux.md)

##<a name="requirements"></a>Preduvjeti

* [Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 ili noviji

* [Maven](http://maven.apache.org/)


* [Klaster utemeljen na sustavu Windows HDInsight s HBase](hdinsight-hbase-tutorial-get-started.md#create-hbase-cluster)


    > [AZURE.NOTE] HDInsight klaster verzije 3,2 i 3,3 testirate korake u ovom dokumentu. Zadane vrijednosti navedene u primjerima su za HDInsight 3,3 klaster.

##<a name="create-the-project"></a>Stvaranje projekta

1. Iz naredbenog retka u okruženje za razvoj mijenjati mape na mjesto na kojem želite stvoriti projekt, primjerice, `cd code\hdinsight`.

2. Koristite naredbu __mvn__ , koji se instalira uz Maven, da biste generirali scaffolding za projekt.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Ta naredba stvara direktorij na trenutnom mjestu pod nazivom navedenu parametrom __artifactID__ (**hbaseapp** u ovom primjeru.) Taj direktorij sadrži sljedeće stavke:

    * __pom.XML__: U programu Project objektnom modelu ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) sadrži podatke i konfiguracije detalje koristi za stvaranje projekta.

    * __src__: direktorij koji sadrži __main\java\com\microsoft\examples__ direktorija, gdje će autor aplikacije.

3. Brisanje datoteka __src\test\java\com\microsoft\examples\apptest.java__ jer se koristi u ovom primjeru.

##<a name="update-the-project-object-model"></a>Ažuriranje objektni Model projekta

1. Uređivanje datoteke __pom.xml__ i dodati sljedeći kod unutar na `<dependencies>` sekciju:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    U ovom je odjeljku opisano Maven projekta potrebna je verzija __hbase klijent__ __1.1.2__. Kompiliranje vrijeme ovaj ovisnost se preuzima iz spremišta Maven zadani. Dodatne informacije o ovom ovisnost možete koristiti [Maven središnje spremište pretraživanja](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) .

    > [AZURE.IMPORTANT] Broj verzije moraju se podudarati verziju HBase koje ste dobili uz svoj klaster HDInsight. U sljedećoj su tablici koristite da biste pronašli broj ispravne verzije.

  	| HDInsight klaster verzija | Verzija HBase koristi |
  	| ----- | ----- |
  	| 3,2 | 0.98.4-hadoop2 |
  	| 3,3 | 1.1.2 |

    Dodatne informacije o verzijama HDInsight i komponenata potražite u članku [što su različite Hadoop komponente dostupno u sklopu HDInsight](hdinsight-component-versioning.md).

2. Ako koristite programa HDInsight 3,3 klaster, morate dodati na sljedeći način u `<dependencies>` odjeljak:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    U ovom ovisnost učitava komponente Phoenixu core koji koriste verziju Hbase 1.1.x.

2. Dodajte sljedeći kod __pom.xml__ datoteku. U ovom se odjeljku mora se nalaziti unutar na `<project>...</project>` oznake u datoteci, na primjer, između `</dependencies>` i `</project>`.

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
              <resource>
                <directory>${basedir}/conf</directory>
                <filtering>false</filtering>
                <includes>
                  <include>hbase-site.xml</include>
                </includes>
              </resource>
            </resources>
          <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                </configuration>
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

    Na `<resources>` odjeljak konfigurira resursa (__conf\hbase site.xml__) koji sadrži informacije o konfiguraciji za HBase.

    > [AZURE.NOTE] Možete postaviti i konfiguracija vrijednosti putem koda. Pogledajte komentare u __CreateTable__ primjer u kojem se nalazi iza kako to učiniti.

    To `<plugins>` odjeljak konfigurira [Maven Nijansa dodatak](http://maven.apache.org/plugins/maven-shade-plugin/)i [Dodatak za Maven prevoditelj (Compiler)](http://maven.apache.org/plugins/maven-compiler-plugin/) . Dodatak compiler koristi se za Kompiliranje topologije. Da biste spriječili dupliciranje licence u paketu POSUDU koja je ugrađena po Maven koristi se dodatak nijansu. Razlog koristi se datoteke duplicirane licence uzrokovati pogreške prilikom izvođenja na klasteru HDInsight. Korištenje maven-Nijansa – dodatak s na `ApacheLicenseResourceTransformer` implementaciju sprječava ta se pogreška.

    Maven-Nijansa – dodatak i daje uber posudu (ili fat posudu) koji sadrži sve ovisnosti potrebnih aplikacije.

3. Spremite datoteku __pom.xml__ .

4. Stvorite novi direktorij pod nazivom __konf__ u direktoriju __hbaseapp__ . U direktoriju __konf__ stvoriti datoteku pod nazivom __hbase site.xml__. Koristite sljedeće kao sadržaj datoteke:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
          * Licensed to the Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See the NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  The ASF licenses this file
          * to you under the Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with the License.  You may obtain a copy of the License at
          *
          *     http://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed to in writing, software
          * distributed under the License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See the License for the specific language governing permissions and
          * limitations under the License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Datoteka će se koristiti za učitavanje konfiguracija HBase za programa klaster HDInsight.

    > [AZURE.NOTE] Minimalna hbase site.xml datoteka, a sadrži Gola minimalne postavke za klaster HDInsight.

3. Spremite datoteku __hbase site.xml__ .

##<a name="create-the-application"></a>Stvaranje aplikacije

1. Idite u direktorij __hbaseapp\src\main\java\com\microsoft\examples__ i preimenujte datoteku app.java __CreateTable.java__.

2. Otvorite datoteku __CreateTable.java__ i zamijeniti postojeći sadržaj na sljedeći kod:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    Ovo je predmet __CreateTable__ koji će stvoriti tablice pod nazivom __osobe__ i popunjavanje s nekim korisnicima unaprijed definirane.

3. Spremite datoteku __CreateTable.java__ .

4. U direktoriju __hbaseapp\src\main\java\com\microsoft\examples__ stvorite novu datoteku pod nazivom __SearchByEmail.java__. Koristite sljedeći kod kao sadržaj ove datoteke:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    Klase __SearchByEmail__ može se koristiti upit za retke tako da adresu e-pošte. Budući da je koristi Regularni izraz filtra, možete unijeti niz ili uobičajenog izraza prilikom korištenja klasu.

5. Spremite datoteku __SearchByEmail.java__ .

6. U direktoriju __hbaseapp\src\main\hava\com\microsoft\examples__ stvorite novu datoteku pod nazivom __DeleteTable.java__. Koristite sljedeći kod kao sadržaj ove datoteke:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    Klase se čišćenje u ovom se primjeru onemogućivanjem i ispuštanje tablice stvorio __CreateTable__ predmete.

7. Spremite datoteku __DeleteTable.java__ .

##<a name="build-and-package-the-application"></a>Stvaranje i paket aplikacije

1. Otvorite naredbeni redak i promijenite direktorija u direktoriju __hbaseapp__ .

2. Da biste sastavili POSUDU datoteku koja sadrži aplikaciju, koristite sljedeću naredbu:

        mvn clean package

    To briše sve prethodne artefakte Sastavi, preuzimanja sve ovisnosti koji već je instaliran, zatim stvara i paketi aplikacije.

3. Kada se naredba dovrši, direktorija __hbaseapp\target__ sadrži datoteku pod nazivom __hbaseapp 1.0 SNAPSHOT.jar__.

    > [AZURE.NOTE] Datoteka __hbaseapp 1.0 SNAPSHOT.jar__ je programa uber posudu (ponekad se zove i fat posudu,) koji sadrži sve ovisnosti koji su potrebni za pokretanje aplikacije.

##<a name="upload-the-jar-file-and-start-a-job"></a>Prijenos datoteke POSUDU i pokretanje zadatka

Načina mnogo da biste prenijeli datoteke na svoj klaster HDInsight, kao što je opisano u [prijenos podataka za Hadoop poslove u HDInsight](hdinsight-upload-data.md). U sljedećim se koracima koristi Azure PowerShell.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


1. Nakon instaliranja i konfiguriranja Azure PowerShell, stvorite novu datoteku pod nazivom __hbase runner.psm1__. Sadržaj ove datoteke upotrijebili na sljedeći način:

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>
        
        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,
        
        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        
        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,
        
        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )
        
        Set-StrictMode -Version 3
        
        # Is the Azure module installed?
        FindAzure
        
        # Get the login for the HDInsight cluster
        $creds = Get-Credential
        
        # Get storage information
        $storage = GetStorage -clusterName $clusterName
        
        # The JAR
        $jarFile = "wasbs:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"
        
        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex
        
        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
        }
        
        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>
        
        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,
                
                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,
                
                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,
                
                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )
            
            Set-StrictMode -Version 3
            
            # Is the Azure module installed?
            FindAzure
            
            # Get authentication for the cluster
            $creds=Get-Credential
            
            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }
            
            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName
            
            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }
        
        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }
        
        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}
            
            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey
            
            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    Datoteka sadrži dvije modula:

    * __Dodavanje HDInsightFile__ - koristi za prijenos datoteka na HDInsight

    * __Početak HBaseExample__ - se pokreće klase ste ranije stvorili

2. Spremite datoteku __hbase runner.psm1__ .

3. Otvaranje novog prozora Azure PowerShell, promijenite direktorija u direktoriju __hbaseapp__ i pokrenite sljedeću naredbu.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Promijenite put do mjesta na kojem ste ranije stvorili datoteku __hbase runner.psm1__ . To registrira modula za ovu sesiju Azure PowerShell.

2. Koristite sljedeću naredbu da biste prenijeli __hbaseapp 1.0 SNAPSHOT.jar__ na svoj klaster HDInsight.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Zamijenite __hdinsightclustername__ naziv svoj klaster HDInsight. Naredba prenosi __hbaseapp 1.0 SNAPSHOT.jar__ __primjer/staklenke__ mjesto u primarni prostora za pohranu za svoj klaster HDInsight.

3. Nakon prijenosa datoteka omogućuju stvaranje tablice pomoću __hbaseapp__sljedeći kod:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Zamijenite __hdinsightclustername__ naziv svoj klaster HDInsight.

    Ta se naredba stvara novu tablicu pod nazivom __osoba__ u svoj klaster HDInsight. Ta se naredba Prikaži sve izlazne u prozoru konzole.

2. Da biste pronašli stavke u tablici, koristite sljedeću naredbu:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Zamijenite __hdinsightclustername__ naziv svoj klaster HDInsight.

    Ta se naredba koristi klase **SearchByEmail** da biste pronašli sve retke u kojima liniji __contactinformation__ stupca i stupca __e-pošte__ sadrži niz __contoso.com__. Trebali biste dobiti sljedeće rezultate:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Korištenje __fabrikam.com__ za na `-emailRegex` vrijednost vraća korisnici koji imaju __fabrikam.com__ u polje e-pošte. Budući da pretraživanje je implementirati pomoću običnog filtara temeljenih na izraz, možete unijeti regularne izraze, kao što su __^ r__, koje vraća stavke u kojima se e-pošte počinje slovom "r".

##<a name="delete-the-table"></a>Brisanje tablice

Kada završite s primjera, koristite sljedeću naredbu iz sesije Azure PowerShell da biste izbrisali tablicu __osobe__ koja se koristi u ovom primjeru:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Zamijenite __hdinsightclustername__ naziv svoj klaster HDInsight.

##<a name="troubleshooting"></a>Otklanjanje poteškoća

###<a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Bez rezultata ili neočekivane rezultate kada se koristi Start HBaseExample

Korištenje na `-showErr` parametar da biste pogledali standardnu pogrešku (STDERR) koji je sastavio prilikom pokretanja posla.
