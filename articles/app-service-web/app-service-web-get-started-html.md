<properties 
    pageTitle="Implementacija web-aplikaciju programa prvog Azure pet minuta | Microsoft Azure" 
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
    
# <a name="deploy-your-first-web-app-to-azure-in-five-minutes"></a>Implementacija web-aplikaciju programa prvog Azure pet minuta

Pomoću ovog praktičnog vodiča pomaže vam implementacija jednostavne HTML + CSS-a web-aplikacijama [Aplikacije servisa za Azure](../app-service/app-service-value-prop-what-is.md).
Možete koristiti aplikacije servisa za stvaranje web-aplikacije, [mobilne aplikacije sigurnosno završava](/documentation/learning-paths/appservice-mobileapps/)i [aplikacija za API-JA](../app-service-api/app-service-api-apps-why-best-platform.md).

Hoćeš: 

- Stvaranje web-aplikacijama u aplikacije servisa za Azure.
- Implementacija HTML i CSS-a na njega.
- Pogledajte na stranice radi uživo u radni.
- Ažurirajte svoj sadržaj na isti način kao što to činite [Gurni brojka pamti](https://git-scm.com/docs/git-push).

## <a name="prerequisites"></a>Preduvjeti

- [Brojka](http://www.git-scm.com/downloads).
- [Azure EŽA](../xplat-cli-install.md).
- Račun za Microsoft Azure. Ako nemate račun, možete ga [Registrirajte se za besplatnu probnu verziju](/pricing/free-trial/?WT.mc_id=A261C142F) ili [aktivirali svoje prednosti pretplatnika Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Možete [Pokušati aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751) bez računa za Azure. Stvaranje starter aplikacije, a zatim reproducirajte s njim za jedan sat – bez kreditne kartice potrebna, bez preuzete obveze.

## <a name="deploy-a-simple-html-site"></a>Implementacija jednostavne HTML web-mjesta

1. Otvorite novi Windows naredbeni redak, u prozoru ljuske PowerShell, Linux ljuske ili terminal OS X. Pokrenite `git --version` i `azure --version` da biste potvrdili da su brojka i Azure EŽA instaliran na vašem računalu.

    ![Testiranje instalacije EŽA alata za web-aplikaciju programa prvog servisu Azure](./media/app-service-web-get-started/1-test-tools.png)

    Ako još niste instalirali alate, potražite u članku [preduvjeti](#Prerequisites) za preuzimanje veze.

3. Prijavite se na Azure ovako:

        azure login

    Praćenje poruka pomoći da biste nastavili postupak prijave.

    ![Prijavite se u sustav Azure da stvorite prvu web-aplikaciju](./media/app-service-web-get-started/3-azure-login.png)

4. Promjena Azure EŽA ASM u način rada, a zatim postavite implementaciju korisnika servisa za aplikaciju. Će implementirati kod kasnije pomoću vjerodajnica.

        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

1. Promjena radni direktorij (`CD`) i Kloniraj aplikaciju uzorka ovako:

        git clone https://github.com/Azure-Samples/app-service-web-html-get-started.git

2. Promijenite u spremištu aplikacije uzorka. 

        cd app-service-web-html-get-started

4. Stvaranje aplikacije resursa aplikacije servisa u Azure s naziv jedinstveni aplikacije i implementaciju korisnika ste prethodno konfigurirali. Kada se od vas zatraži, navedite broj željeno područje.

        azure site create <app_name> --git --gitusername <username>

    ![Stvaranje Azure resursa za prvu web-aplikacije u Azure](./media/app-service-web-get-started/4-create-site.png)

    Vaša je stvorena aplikacija u Azure sada. Osim toga, trenutnog direktorija je brojka pokrenuti i kontaktu nove aplikacije servisa za aplikacije kao na udaljenom brojka.
    Možete pregledavati na URL aplikacije (http://&lt;app_name >. azurewebsites.net) da biste vidjeli lijepe zadane stranice HTML, ali recimo zapravo dohvatite kod li sada.

4. Ogledni kod implementirati Azure aplikacije, kao što činite Gurni kod s brojka. Kada se to od vas zatraži, pomoću lozinke koje ste prethodno konfigurirali.

        git push azure master

    ![Automatske kod na prvi web-aplikaciju u Azure](./media/app-service-web-get-started/5-push-code.png)

    Ako ste koristili neku od okviri jezik, vidjet ćete različite izlaz. Razlog je tome što `git push` ne samo stavlja kod u Azure, ali i pokreće zadataka implementacije u modul implementacije. Ako imate neki package.json (Node.js) ili requirements.txt (Python) datoteke u korijensko projekta (spremište) ili ako imate packages.config datoteku u ASP.NET projektu, implementacijsku skriptu vraća potrebna paketa za vas. Možete i [omogućiti Skladatelj proširenje](web-sites-php-mysql-deploy-use-git.md#composer) za automatsku obradu composer.json datoteke u aplikaciji i.

Čestitamo, ste implementirati aplikaciju u aplikacije servisa za Azure.

## <a name="see-your-app-running-live"></a>U odjeljku aplikacije radi uživo

Da biste vidjeli aplikaciju radi uživo u Azure, pokrenite sljedeću naredbu iz bilo kojeg direktorija u vašem spremištu:

    azure site browse

## <a name="make-updates-to-your-app"></a>Ništa ažurirati aplikaciju

Sada možete koristiti i brojka da biste u bilo kojem trenutku korijensko projekta (spremište) da biste ažuriranje uživo web-mjesta. To učiniti na isti način kao i kada implementiran kod prvi put. Ako, na primjer, svaki put kada želite lokalno automatske novi promjena koje ste testirati samo izvršite sljedeće naredbe iz korijensko projekta (spremište):

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Daljnji koraci

Pronađite Preferirani razvoja i implementaciju korake za vaše framework jezik:

> [AZURE.SELECTOR]
- [.NET](web-sites-dotnet-get-started.md)
- [PHP](app-service-web-php-get-started.md)
- [Node.js](app-service-web-nodejs-get-started.md)
- [Python](web-sites-python-ptvs-django-mysql.md)
- [Java](web-sites-java-get-started.md)

Ili bolje iskorištavanje web-aplikaciju programa prvi. Ako, na primjer:

- [Drugi načini implementacije kod da biste Azure](../app-service-web/web-sites-deploy.md), isprobajte. Ako, na primjer, da biste implementirali neku od vašeg spremištima GitHub, samo odaberite **GitHub** umjesto **Lokalnom spremištu brojka** u **Mogućnosti implementacije**.
- Mogućnošću Azure aplikacije sljedeću razinu. Provjere autentičnosti korisnika. Promjena veličine koji se temelji na zahtjev. Postavljanje upozorenja neke performanse. Sve uz samo nekoliko klikova. Potražite u članku [Dodavanje funkcija u prvom web app](app-service-web-get-started-2.md).

