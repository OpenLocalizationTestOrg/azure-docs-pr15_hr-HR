<properties
    pageTitle="Azure AD Java Uvod | Microsoft Azure"
    description="Kako stvoriti aplikaciju za Java naredbenog retka koji se korisnici prijavi da biste pristupili API."
    services="active-directory"
    documentationCenter="java"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Korištenje aplikacije naredbenog retka Java da biste pristupili API-JA s Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD olakšava jednostavne i jasan spoljni upravljanje identitetom web app, koja omogućuje jedan prijave i sign-out sa samo nekoliko redaka koda.  U web-aplikacijama Java, koje možete izvršiti to pomoću implementacija tvrtke Microsoft ADAL4J utemeljenih na zajednice.

  U nastavku ćemo pomoću ADAL4J da biste:
- Korisnik se prijaviti u aplikaciju pomoću Azure AD kao davatelja identiteta.
- Prikaz neke informacije o korisniku.
- Prijava korisnika iz aplikacije.

Da biste to učinili, morat ćete:

1. Registracija aplikacije s Azure AD
2. Postavljanje aplikacije da biste koristili biblioteku ADAL4J.
3. Pomoću biblioteke ADAL4J problema prijave i sign-out zahtjeve za Azure AD.
4. Ispis podataka o korisniku.

Da biste počeli, [Preuzimanje skeleton aplikacije](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) ili [Preuzimanje dovršene uzorka](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\/archive/complete.zip).  Trebat ćete klijent za Azure AD u kojoj se registrirati aplikacije.  Ako ga već imam [upute da biste ga nabavili](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1. Registracija aplikacije s Azure AD
Da biste omogućili aplikacije za provjeru autentičnosti korisnika, najprije morate registrirati novu aplikaciju u klijentu.

- Prijavite se na Portal za Azure upravljanje.
- U navigacijskom oknu na lijevoj strani kliknite **Servisa Active Directory**.
- Odaberite klijenta na kojem želite da biste registrirali aplikaciju.
- Kliknite karticu **aplikacije** pa kliknite Dodaj u donjem ladica.
- Slijedite upite i stvaranje je nove **web-aplikacije i/ili WebAPI**.
    - **Naziv** aplikacije opisane su aplikacije krajnjim korisnicima
    - **URL za prijavu** nije osnovni URL aplikacije.  Zadana vrijednost u skeleton je `http://localhost:8080/adal4jsample/`.
    - **Aplikacija ID URI** je jedinstveni identifikator za svoju aplikaciju.  Konvencije jest korištenje `https://<tenant-domain>/<app-name>`, npr.`http://localhost:8080/adal4jsample/`
- Nakon što ste dovršili registraciju, AAD će dodijeliti aplikacije klijent Jedinstveni identifikator.  Potreban vam je tu vrijednost u sljedećim odjeljcima tako da ga kopirali na kartici Konfiguriraj.

Jednom na portalu za aplikaciju **Aplikaciji tajna** za svoju aplikaciju za stvaranje i kopiranje prema dolje.  Jer će vam uskoro.


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. postavljanje aplikacije da biste koristili biblioteku ADAL4J i preduvjeti pomoću Maven
Ovdje, ne možemo konfigurirati ADAL4J koristiti protokol za povezivanje OpenID provjere autentičnosti.  ADAL4J će se koristiti za izdavanje zahtjeva za prijavu i sign-out, upravljanje sesija i informacije o korisniku, između ostalog.

-   U korijenskom direktoriju projekta, otvaranje/stvaranje `pom.xml` i pronađite u `// TODO: provide dependencies for Maven` i zamijeniti na sljedeći način:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. stvaranje PublicClient datoteka jezika java

Kao što je naznačeno iznad, ne možemo koristit će grafikonu API-JA za dohvaćanje podataka o prijavljenog korisnika. Za to bude jednostavno nam smo potrebno stvoriti datoteku za predstavljanje **Objekt imenika** i pojedinačne datoteke za predstavljanje **korisnika** tako da se uzorak OO Java može se koristiti.

1. Stvoriti datoteku pod nazivom `DirectoryObject.java` koji koristimo za pohranu osnovni podaci o sve DirectoryObject (koje možete slobodno to kasnije koristili za sve druge grafikonu upite možete učiniti). Koje možete Izreži i Zalijepi to dolje:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


##<a name="compile-and-run-the-sample"></a>Kompiliranje i pokretanje uzorka

Promjena vratili na prethodnu veličinu u korijenskom direktoriju i pokrenite sljedeću naredbu da biste sastavili uzorka samo stavite Suradnja pomoću `maven`. To će se koristiti u `pom.xml` datoteke koje ste sami napisali ovisnost.

`$ mvn package`

Trebala bi se pojaviti na `adal4jsample.war` datoteka u vašem `/targets` direktorija. Možda implementacije koje u vašem Tomcat spremnik i posjetite URL-a 

`http://localhost:8080/adal4jsample/`


> [AZURE.NOTE] 
Vrlo je jednostavno za implementaciju WAR s najnovijim Tomcat poslužiteljima. Jednostavno dođite do `http://localhost:8080/manager/` i slijedite upute na prijenos na "adal4jsample.war" datoteku. Autodeploy umjesto vas je će s točne krajnjoj točki.

##<a name="next-steps"></a>Daljnji koraci

Čestitamo! Sada imate radu Java aplikaciju koja je mogućnost za provjeru autentičnosti korisnika, sigurno poziva Web API-ji pomoću OAuth 2.0 i osnovne informacije o korisniku.  Ako to već niste učinili, sada je vrijeme za popunjavanje vaš klijent s nekim korisnicima.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) [prikazuje se kao .zip ovdje](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip)ili pak mogu Kloniraj ga iz GitHub:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

