<properties
   pageTitle="Korištenje podataka Lake spremište Java SDK za razvoj aplikacija | Microsoft Azure"
   description="Korištenje Azure podataka Lake spremište Java SDK za razvoj aplikacija"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-java"></a>Početak rada s spremišta Lake podataka za Azure pomoću jezika Java

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-JA](data-lake-store-get-started-rest-api.md)
- [Azure EŽA](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Saznajte kako koristiti Java SDK Azure Lake pohrane podataka za izvođenje osnovnih operacija kao što su prilikom stvaranja mape, prijenos i preuzimanje datoteke s podacima, itd. Dodatne informacije o Lake podataka potražite u članku [Lake spremišta podataka za Azure](data-lake-store-overview.md).

Možete pristupiti Java SDK API dokumenti za Azure podataka Lake spremište na [dokumente Azure podataka Lake spremište Java API -JA](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Preduvjeti

* Java Development Kit (JDK 7 ili noviji, pomoću Java verzija 1.7 ili novija verzija)
* Račun za Azure podataka Lake trgovine. Slijedite upute na [Početak rada s spremišta Lake podataka za Azure pomoću portala za Azure](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). Pomoću ovog praktičnog vodiča koristi Maven ovisnost Sastavi i project. Iako je moguće da biste sastavili bez korištenja Sastavi sustava kao što su Maven ili Gradle, te provjerite sustavi je mnogo lakše upravljanje ovisnosti.
* (Neobavezno) I IDE kao što su [IntelliJ IDEJA](https://www.jetbrains.com/idea/download/) ili [Eclipse](https://www.eclipse.org/downloads/) ili slične.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Kako autentičnost pomoću servisa Azure Active Directory?

Pomoću ovog praktičnog vodiča koristimo klijent tajna aplikacije za Azure AD za dohvaćanje token Azure Active Directory (servis za servis provjera autentičnosti). Da biste stvorili objekt spremišta podataka Lake klijenta za izvođenje operacije datoteke i imenik operacija koristimo ovaj token. Upute za provjeru autentičnosti s trgovinom Lake podataka za Azure pomoću tajna klijent, ne možemo poduzeti sljedeće korake više razine:

1. Stvaranje Azure AD web-aplikacije
2. Dohvaćanje ID klijenta, tajna klijenta i tokena krajnja točka za Azure AD web-aplikaciju.
3. Konfiguriranje programa access za web-aplikaciju Azure AD na spremišta podataka Lake datoteka/mapu u koju želite pristupiti iz aplikacije Java stvarate.

Upute za izvođenje ovih koraka, potražite u članku [Stvaranje aplikacije servisa Active Directory](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application).

Azure Active Directory sadrži druge mogućnosti kao i za dohvaćanje token. Možete odabrati jedan od nekoliko različitih provjere autentičnosti mehanizme kako bi odgovarao scenariju, na primjer, aplikacije koje se izvode u pregledniku, aplikacija podijeliti kao aplikaciji za stolna računala ili poslužitelja aplikacija radi lokalnog ili Azure virtualnog računala. Možete odabrati i iz različitih vrsta vjerodajnice kao što su lozinke, certifikata, 2 provjere autentičnosti itd. Osim toga, Azure Active Directory omogućuje sinkronizirajte lokalne korisnike za Active Directory s oblaka. Detalje potražite u članku [Provjera autentičnosti scenariji za Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Stvaranje aplikacije Java

Kod ogledne dostupna [na GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) sferama vas kroz postupak stvaranja datoteka u trgovini sustava Ulančavanje datoteke, preuzimanja i brisanje neke datoteke u spremištu. U ovom odjeljku članka vodi vas kroz glavna dijela kod.

1. Stvaranje projekta Maven korištenje [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) iz naredbenog retka ili korištenje programa IDE. Upute za stvaranje Java projektu pomoću IntelliJ potražite [ovdje](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Upute za stvaranje projektu pomoću Eclipse potražite [ovdje](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 

2. Dodajte sljedeće ovisnosti Maven **pom.xml** datoteku. Dodajte sljedeće isječak teksta između na ** \</version >** oznaka i ** \</project >** oznaka:

        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.0.4-SNAPSHOT</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>

    Prvi ovisnost jest korištenje SDK za spremište podataka Lake (`azure-datalake-store`) u spremištu maven. Drugi ovisnosti (`slf4j-nop`) je da biste odredili koji zapisivanje framework za ovu aplikaciju. SDK za spremište podataka Lake koristi façade [slf4j](http://www.slf4j.org/) zapisivanje, koji omogućuje vam da odaberete od brojnih popularnih zapisivanje okviri, kao što su log4j, jezika Java zapisivanje, logback, itd., ili bez prijave. U ovom primjeru, onemogućit ćemo zapisivanje, dakle koristimo **slf4j nop** povezivanja. Da biste koristili druge mogućnosti zapisivanja u aplikaciji, potražite u članku [ovdje](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>Dodavanje koda za aplikacije

Postoje tri glavna dijela kod.

1. Nabavite token Azure Active Directory

2. Koristite token za stvaranje spremišta podataka Lake klijenta.

3. Klijent spremišta Lake podataka možete koristiti za izvođenje operacija.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Korak 1: Dobivanje token za Azure Active Directory.

SDK za spremište podataka Lake pruža praktičan metode kojih možete nabaviti sigurnosnih tokena potrebno obratiti računa spremišta Lake podataka. Međutim, SDK ne prema može koristiti samo ove metode. Možete koristiti bilo koji način dobivanja token kao i slično [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java)ili prilagođeni kod.

Da biste koristili SDK za spremište Lake podataka da biste dobili tokena za Active Directory web-aplikaciju koju ste ranije stvorili, koristite statične načine u `AzureADAuthenticator` predmete. **POPUNITE-u – ovdje** zamijenite stvarnih vrijednosti za Azure Active Directory web-aplikaciju.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AzureADToken token = AzureADAuthenticator.getTokenUsingClientCreds(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Korak 2: Stvaranje objekta Lake spremišta podataka za Azure klijenta (ADLStoreClient)

Stvaranje objekta [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) zahtijeva Navedite naziv računa spremišta Lake podataka i Azure Active Directory token generira u posljednjem koraku. Imajte na umu da naziv računa spremišta Lake podataka mora biti na potpuno kvalificirani naziv domene. Na primjer, zamijenite **POPUNITE-u – ovdje** nešto poput **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, token);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Korak 3: Korištenje ADLStoreClient za izvođenje operacije datoteke i imenik

Kod u nastavku sadrži primjer isječci nekoliko uobičajenih operacija. Možete pogledati cijeli [podataka Lake spremište Java SDK API dokumenti](https://azure.github.io/azure-data-lake-store-java/javadoc/) **ADLStoreClient** objekta da biste vidjeli ostalih operacija.
 
Imajte na umu datoteke su čitanja i napisali u pomoću standardnih strujanja Java. To znači da možete slojeva bilo koji od strujanja Java pri vrhu strujanja spremišta podataka Lake prednosti iz standardnog Java funkcija (npr., ispis strujanja oblikovani Izlaz ili bilo koji od strujanja sažimanje ili šifriranje za dodatne funkcije na vrh, itd.).

    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Korak 4: Stvaranje i pokretanje aplikacija

1. Da biste pokrenuli iz unutar programa IDE, pronađite, a zatim pritisnite gumb za **pokretanje** . Da biste pokrenuli iz Maven, koristite [izvršavanja prve: izvršavanja prve](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).

2. Čime se dobiva samostalne posudu koje možete pokrenuti iz naredbenog retka sastavljanje posudu s sve ovisnosti uključen, pomoću [Maven skupa dodatak](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). Pom.xml u [primjeru izvornog koda na github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) ima primjera kako to učiniti.


## <a name="next-steps"></a>Daljnji koraci

- [Zaštite podataka u spremištu Lake podataka](data-lake-store-secure-data.md)
- [Korištenje analize Lake Azure podataka s spremišta Lake podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight pomoću spremišta Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)
