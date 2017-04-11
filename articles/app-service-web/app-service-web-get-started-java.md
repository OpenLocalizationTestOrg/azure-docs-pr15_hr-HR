<properties 
    pageTitle="Implementacija prvi Java web-aplikaciju programa Azure pet minuta | Microsoft Azure" 
    description="Saznajte kako je lako da biste pokrenuli web-aplikacije u aplikaciju servisa uvođenjem uzorka aplikacije. Pokrenite brzo način realni razvoj i odmah vidjeli rezultate." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-java-web-app-to-azure-in-five-minutes"></a>Implementacija prvi Java web-aplikaciju programa Azure pet minuta

Pomoću ovog praktičnog vodiča pridonosi implementacija jednostavne Java web-aplikacijama [Aplikacije servisa za Azure](../app-service/app-service-value-prop-what-is.md).
Možete koristiti aplikacije servisa za stvaranje web-aplikacije, [mobilne aplikacije sigurnosno završava](/documentation/learning-paths/appservice-mobileapps/)i [aplikacija za API-JA](../app-service-api/app-service-api-apps-why-best-platform.md).

Hoćeš: 

- Stvaranje web-aplikacijama u aplikacije servisa za Azure.
- Implementacija aplikacije Java uzorka.
- Pogledajte na kod koji se izvodi uživo u radnog.

## <a name="prerequisites"></a>Preduvjeti

- Pronađite FTP/FTPS klijent, kao što su [FileZilla](https://filezilla-project.org/).
- Dobivanje korisničkog računa za Microsoft Azure. Ako nemate račun, možete ga [Registrirajte se za besplatnu probnu verziju](/pricing/free-trial/?WT.mc_id=A261C142F) ili [aktivirali svoje prednosti pretplatnika Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Možete [Pokušati aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751) bez računa za Azure. Stvaranje starter aplikacije, a zatim reproducirajte s njim za jedan sat – bez kreditne kartice potrebna, bez preuzete obveze.

<a name="create"></a>
## <a name="create-a-web-app"></a>Stvaranje web-aplikacijama

1. Prijavite se na [portal za Azure](https://portal.azure.com) s računom za Azure.

2. Na lijevom izborniku kliknite **Novo** > **Web + Mobile** > **Web App**.

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. U stvaranje plohu aplikacija za novu aplikaciju pomoću sljedećih postavki:

    - **Naziv aplikacije**: upišite jedinstveni naziv.
    - **Grupa resursa**: odaberite **Stvaranje nove** i grupu resursa dodijelite naziv.
    - **Aplikacije servisa za planiranje/mjesto**: kliknite je da biste konfigurirali, a zatim kliknite **Stvori novo** da biste postavili naziv, mjesto i cijene sloju aplikacije servisa za planiranje. Slobodno koristiti **slobodno** cijene sloju.

    Kada završite, na plohu stvaranja aplikacije trebao bi izgledati ovako:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. Kliknite **Stvori** pri dnu. Možete kliknuti ikonu **obavijesti** pri vrhu da biste vidjeli napredak.

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Po završetku implementacije trebali biste vidjeti ovu poruku. Kliknite poruku da biste otvorili plohu za implementaciju sustava.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. Plohu **uspješno uvođenje** kliknite vezu **resursa** da biste otvorili plohu novog web-aplikaciju programa korisnika.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## <a name="deploy-a-java-app-to-your-web-app"></a>Implementacija aplikacije Java na web-aplikaciju

Sada ćemo implementacija aplikacije Java za Azure pomoću FTPS.

5. U aplikaciji plohu web pomaknite se do **postavki aplikacije** ili pretraživanja za njega, a zatim kliknite ga. 

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

6. U **verziji jezika Java**, odaberite **Java 8** , a zatim kliknite **Spremi**.

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

    Kada primite obavijest o **uspješno ažuriranje postavki web aplikacije**, dođite do http://*&lt;appname >*. azurewebsites.net da biste vidjeli servlet JSP zadani u akciji.

7. Vratite se u aplikaciju plohu web pomaknite se do web-mjesto **vjerodajnice za implementaciju** ili u okvir za pretraživanje za njega, a zatim kliknite ga.

8. Postavljanje vjerodajnica za implementaciju, a zatim kliknite **Spremi**.

7. Vratite se u aplikaciju plohu web kliknite **Pregled**. Uz **FTP/implementacije korisničko ime** i **naziv glavnog računala FTPS**, kliknite gumb **Kopiraj** da biste kopirali te vrijednosti.

    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)

    Sada ste spremni za implementaciju aplikacije Java s FTPS.

8. U klijentu za FTP/FTPS prijaviti na poslužitelj za FTP Azure web app pomoću vrijednosti koju ste kopirali u posljednjem koraku. Koristite implementacije lozinku koju ste prethodno stvorili.

    Sljedeća slika prikazuje prijaviti pomoću FileZilla.

    ![](./media/app-service-web-get-started-languages/filezilla-login.png)

    Možda ćete vidjeti sigurnosnih upozorenja za nepoznatim SSL certifikat od Azure. Nastaviti i nastaviti.

9. Kliknite [ovu vezu](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war) da biste preuzeli datoteke WAR na lokalno računalo.

9. U klijentu za FTP/FTPS dođite do **/site/wwwroot/webapps** na udaljenom web-mjestu, a preuzetu datoteku WAR na lokalnom računalu odvučete tom udaljene direktoriju.

    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)

    Kliknite **u redu** da biste nadjačali datoteke u Azure.

    >[AZURE.NOTE] Skladu Tomcat na zadano ponašanje filename **ROOT.war** u /site/wwwroot/webapps omogućuje korijenskog web-aplikaciju (http://*&lt;appname >*. azurewebsites.net), i naziv datoteke ** * &lt;anyname >*.war** daje imenovani web-aplikacijama (http://*&lt;appname >*.azurewebsites.net/*&lt;anyname >*).

To je to! Pokrenite aplikaciju Java sada je pokrenut uživo u Azure. U pregledniku dođite do http://*&lt;appname >*. azurewebsites.net da biste vidjeli na djelu. 

## <a name="make-updates-to-your-app"></a>Ništa ažurirati aplikaciju

Kad god želite sastaviti ažuriranje, samo Prenesite novu datoteku WAR u istom udaljene imenik s FTP/FTPS klijent.

## <a name="next-steps"></a>Daljnji koraci

[Stvaranje web-aplikacijama Java iz predloška u Azure Marketplace](web-sites-java-get-started.md#marketplace). Možete dobiti vlastiti potpunosti prilagoditi Tomcat spremnik i dobiti poznatih Upravitelj korisničkog Sučelja. 

Azure web app, izravno u [IntelliJ](app-service-web-debug-java-web-app-in-intellij.md) ili [Eclipse](app-service-web-debug-java-web-app-in-eclipse.md)za ispravljanje pogrešaka.

Ili bolje iskorištavanje web-aplikaciju programa prvi. Ako, na primjer:

- [Drugi načini implementacije kod da biste Azure](../app-service-web/web-sites-deploy.md), isprobajte. 
- Mogućnošću Azure aplikacije sljedeću razinu. Provjere autentičnosti korisnika. Promjena veličine koji se temelji na zahtjev. Postavljanje upozorenja neke performanse. Sve uz samo nekoliko klikova. Potražite u članku [Dodavanje funkcija u prvom web app](app-service-web-get-started-2.md).

