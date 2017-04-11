<properties
    pageTitle="Stvaranje aplikacije HBase korištenjem Maven i Java, a zatim implementacija sustavom Linux HDInsight | Microsoft Azure"
    description="Saznajte kako koristiti Apache Maven za sastavljanje utemeljena na HBase Apache aplikacije, a zatim implementacija sustavom Linux HDInsight u Azure oblaka."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>Kako koristiti Maven za izradu Java aplikacije koje koriste HBase sa sustavom Linux HDInsight (Hadoop)

Saznajte kako stvoriti i pomoću Apache Maven izraditi [Apache HBase](http://hbase.apache.org/) aplikacije u Java. Sa sustavom Linux HDInsight klaster koristiti aplikaciju.

[Maven](http://maven.apache.org/) je softver upravljanje projektima i razumijevanje alat koji omogućuje sastavljanje softver, dokumentaciju i izvješća za Java projekte. U ovom se članku opisano kako ga koristiti za stvaranje osnovne Java aplikacije koje stvara, upita, i briše tablice programa HBase na sustavom Linux HDInsight klaster.

> [AZURE.NOTE] U ovom dokumentu pretpostavlja da koristite sustavom Linux HDInsight klaster. Informacije o korištenju utemeljen na sustavu Windows HDInsight klaster, potražite u članku [Korištenje Maven da biste sastavili Java aplikacije koje koriste HBase s HDInsight utemeljen na sustavu Windows](hdinsight-hbase-build-java-maven.md)

##<a name="requirements"></a>Preduvjeti

* [Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 ili noviji

* [Maven](http://maven.apache.org/)

* [U operacijskim sustavom Linux Azure HDInsight klaster s HBase](../hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

    > [AZURE.NOTE] Koraci u ovom dokumentu testirate HDInsight klaster verzije 3,2, 3,3 i 3.4. Zadane vrijednosti navedene u primjerima su za HDInsight 3.4 klaster.

* **Poznavanje s SSH i Pronađenim**. Dodatne informacije o korištenju SSH i Pronađenim sa servisa HDInsight potražite u sljedećim člancima:

    * **Linux, Unix ili OS X klijenata**: potražite u članku [Korištenje SSH s operacijskim sustavom Linux Hadoop na HDInsight Linux, OS X ili Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Klijenti za Windows**: potražite u članku [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

##<a name="create-the-project"></a>Stvaranje projekta

1. Iz naredbenog retka u razvojno okruženje, mijenjati mape na mjesto na kojem želite stvoriti projekt, primjerice, `cd code/hdinsight`.

2. Koristite naredbu __mvn__ , koji se instalira uz Maven, da biste generirali scaffolding za projekt.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Time ste stvorili novi imenik u trenutnom direktoriju s nazivom navedeno parametrom __artifactID__ (**hbaseapp** u ovom primjeru.) Taj imenik će sadržavati sljedeće stavke:

    * __pom.XML__: U programu Project objektni Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) sadrži podatke i konfiguracije detalje koristi za stvaranje projekta.

    * __src__: direktorij koji sadrži __glavne/java/com/microsoft/Primjeri__ direktorija, gdje će autor aplikacije.

3. Brisanje datoteka __src/test/java/com/microsoft/examples/apptest.java__ jer ga se ne koriste u ovom primjeru.

##<a name="update-the-project-object-model"></a>Ažuriranje objektni Model projekta

1. Uređivanje datoteke __pom.xml__ i dodati sljedeći kod unutar na `<dependencies>` odjeljak:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Ovo govori Maven projekta potrebna je verzija __hbase klijent__ __1.1.2__. Kompiliranje vrijeme, to će se preuzeti u spremištu Maven zadani. Dodatne informacije o ovom ovisnost možete koristiti [Maven središnje spremište pretraživanja](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) .

    > [AZURE.IMPORTANT] Broj verzije moraju se podudarati verziju HBase koje ste dobili uz svoj klaster HDInsight. Koristite tablici u nastavku da biste pronašli broj ispravne verzije.

  	| HDInsight klaster verzija | Verzija HBase koristi |
  	| ----- | ----- |
  	| 3,2 | 0.98.4-hadoop2 |
  	| 3,3 i 3.4 | 1.1.2 |

    Dodatne informacije o verzijama HDInsight i komponenata potražite u članku [što su različite Hadoop komponente dostupno u sklopu HDInsight](hdinsight-component-versioning.md).

2. Ako koristite programa 3,3 HDInsight ili 3.4 klaster, morate dodati na sljedeći način u `<dependencies>` odjeljak:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    To će učitavanje komponente Phoenixu core, koje su vam potrebne verzijom Hbase 1.1.x.

2. Dodajte sljedeći kod __pom.xml__ datoteku. Time se mora nalaziti unutar na `<project>...</project>` oznake u datoteci, na primjer, između `</dependencies>` i `</project>`.

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

    To konfigurira resursa (__konf site.xml/hbase__,) koji sadrži informacije o konfiguraciji za HBase.

    > [AZURE.NOTE] Možete postaviti i konfiguracija vrijednosti putem koda. Pogledajte komentare u __CreateTable__ primjer u kojem se nalazi iza kako to učiniti.

    To se i konfigurira [Maven Nijansa dodatak](http://maven.apache.org/plugins/maven-shade-plugin/)i [Dodatak za Maven prevoditelj (Compiler)](http://maven.apache.org/plugins/maven-compiler-plugin/) . Dodatak compiler koristi se za Kompiliranje topologije. Da biste spriječili dupliciranje licence u paketu POSUDU koja je ugrađena po Maven koristi se dodatak nijansu. Razlog koristi se datoteke duplicirane licence uzrokovati pogreške prilikom izvođenja na klasteru HDInsight. Korištenje maven-Nijansa – dodatak s na `ApacheLicenseResourceTransformer` implementaciju sprječava ta se pogreška.

    Maven-Nijansa – dodatak i daje uber posudu (ili fat posudu) koji sadrži sve ovisnosti potrebnih aplikacije.

3. Spremite datoteku __pom.xml__ .

4. Stvorite novi direktorij pod nazivom __konf__ u direktoriju __hbaseapp__ . To će se koristiti za držite konfiguracijske informacije o povezivanju sa servisom HBase.

5. Koristite sljedeću naredbu da biste kopirali HBase konfiguraciju poslužitelja HDInsight __konf__ direktorija. Zamjena **korisničko ime** na naziv svoje SSH prijava. Zamijenite **CLUSTERNAME** klaster naziva HDInsight:

        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml

    > [AZURE.NOTE] Ako ste koristili lozinku za račun SSH, zatražit će se da unesete lozinku. Ako ste koristili ključa SSH s računom, možda ćete morati koristiti u `-i` parametar da biste odredili put do ključa datoteke. U sljedećem primjeru učitava privatni ključ iz `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`

##<a name="create-the-application"></a>Stvaranje aplikacije

1. Idite na imenik __hbaseapp/src/glavne/java/com/microsoft/primjere__ i preimenujte datoteku app.java __CreateTable.java__.

2. Otvorite datoteku __CreateTable.java__ i zamijeniti postojeći sadržaj sljedeće:

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
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
            
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");

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

4. U direktoriju __hbaseapp/src/glavne/java/com/microsoft/Primjeri__ stvorite novu datoteku pod nazivom __SearchByEmail.java__. Sadržaj ove datoteke upotrijebili na sljedeći način:

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

    Klase __SearchByEmail__ može se koristiti za upit za redaka prema adresu e-pošte. Jer koristi Uobičajeni izraz filtra, možete unijeti niz ili Uobičajeni izraz prilikom korištenja klasu.

5. Spremite datoteku __SearchByEmail.java__ .

6. U direktoriju __hbaseapp/src/glavne/hava/com/microsoft/Primjeri__ stvorite novu datoteku pod nazivom __DeleteTable.java__. Sadržaj ove datoteke upotrijebili na sljedeći način:

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

2. Iz imenika __hbaseapp__ , koristite sljedeću naredbu da biste sastavili POSUDU datoteku koja sadrži aplikaciju:

        mvn clean package

    To briše sve prethodne artefakte Sastavi, preuzimanja sve ovisnosti koji već je instaliran, zatim stvara i paketi aplikacije.

3. Kada se naredba dovrši, direktorija __hbaseapp/ciljne__ će sadržavati datoteku pod nazivom __hbaseapp 1.0 SNAPSHOT.jar__.

    > [AZURE.NOTE] Datoteka __hbaseapp 1.0 SNAPSHOT.jar__ je programa uber posudu (ponekad se zove i fat posudu,) koji sadrži sve ovisnosti koji su potrebni za pokretanje aplikacije.

##<a name="upload-the-jar-file-and-run-jobs"></a>Prijenos datoteke POSUDU i pokretanje poslova

1. Koristite sljedeće da biste prenijeli posudu klaster HDInsight. Zamjena **korisničko ime** na naziv svoje SSH prijava. Zamijenite **CLUSTERNAME** klaster naziva HDInsight:

        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    To će prijenos datoteke u početnom direktorija za SSH korisnički račun.

    > [AZURE.NOTE] Ako ste koristili lozinku za račun SSH, zatražit će se da unesete lozinku. Ako ste koristili ključa SSH s računom, možda ćete morati koristiti u `-i` parametar da biste odredili put do ključa datoteke. U sljedećem primjeru učitava privatni ključ iz `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

2. Korištenje SSH za povezivanje s klaster HDInsight. Zamjena **korisničko ime** na naziv svoje SSH prijava. Zamijenite **CLUSTERNAME** klaster naziva HDInsight:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] Ako ste koristili lozinku za račun SSH, zatražit će se da unesete lozinku. Ako ste koristili ključa SSH s računom, možda ćete morati koristiti u `-i` parametar da biste odredili put do ključa datoteke. U sljedećem primjeru učitava privatni ključ iz `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Nakon uspostave, koristite sljedeće da biste stvorili novu tablicu HBase pomoću aplikacije Java:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable

    To će stvoriti novu tablicu HBase pod nazivom __osobe__i popunjavanje s podacima.

4. Nakon toga koristite sljedeće da biste potražili adrese e-pošte pohranjuju u tablicu:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com

    Trebali biste dobiti sljedeće rezultate:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

##<a name="delete-the-table"></a>Brisanje tablice

Kada završite s primjera, koristite sljedeću naredbu iz sesije Azure PowerShell da biste izbrisali tablicu __osobe__ koja se koristi u ovom primjeru:

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable

