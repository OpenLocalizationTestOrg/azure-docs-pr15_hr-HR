<properties
    pageTitle="Stvaranje web-aplikacijama i MySQL u aplikacije servisa za Azure i implementacija pomoću brojka"
    description="Praktični vodič koji pokazuje kako stvoriti i web-aplikacije koje se sprema podatke u MySQL i koristiti brojka implementacije Azure."
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Stvaranje web-aplikacijama i MySQL u aplikacije servisa za Azure i implementacija pomoću brojka

Pomoću ovog praktičnog vodiča prikazuje kako da biste stvorili web-aplikacijama i MySQL i implementacija [Aplikacije servisa za](http://go.microsoft.com/fwlink/?LinkId=529714) korištenje brojka. Koristite [i][install-php], alat za MySQL naredbenog retka (dio [MySQL][install-mysql]), i [brojka] [ install-git] instaliran na vašem računalu. Na bilo kojem operacijskom sustavu, uključujući Windows, Mac i Linux mogu slijediti upute u ovom ćete praktičnom vodiču. Nakon dovršetka ovaj vodič, imat ćete web-aplikacijama i/MySQL izvodi u Azure.

Saznat ćete:

* Upute za stvaranje web-aplikacijama i bazom podataka MySQL pomoću [Portala za Azure][management-portal]. Budući da i se po zadanom je omogućena u [Aplikaciju servisa web-aplikacijama](http://go.microsoft.com/fwlink/?LinkId=529714) , ništa posebno je potreban za pokretanje i kod.
* Upute za objavljivanje i ponovno objaviti aplikaciju Azure pomoću brojka.
* Kako omogućiti nastavak Skladatelj da biste automatizirali Skladatelj zadaci na svaku `git push`.

Slijedeći ovog praktičnog vodiča će Stvaranje jednostavne Registracija web app u PHP. Aplikacija će se nalaziti u web-aplikacijama. Snimka zaslona dovršene aplikacija je ispod:

![Azure i web-mjesta][running-app]

## <a name="set-up-the-development-environment"></a>Postavljanje okruženja za razvoj

Pomoću ovog praktičnog vodiča pretpostavlja da imate [i][install-php], alat za MySQL naredbenog retka (dio [MySQL][install-mysql]), i [brojka] [ install-git] instaliran na vašem računalu.

<a id="create-web-site-and-set-up-git"></a>
## <a name="create-a-web-app-and-set-up-git-publishing"></a>Stvaranje web-aplikacijama i postavljanje brojka objavljivanja

Slijedite ove korake da biste stvorili web-aplikacijama i bazom podataka MySQL:

1. Prijavite se na [Portal za Azure][management-portal].
2. Kliknite ikonu **Novo** .

3. Kliknite **Pogledajte sve** pokraj **trgovine**. 

4. Kliknite **Web + Mobile**, a zatim **web-aplikacije + MySQL**. Nakon toga kliknite **Stvori**.

4. Unesite valjani naziv za grupu resursa.

    ![Naziv grupe za skup resursa][resource-group]

5. Unesite vrijednosti za novu web-aplikaciju.

    ![Stvaranje web-aplikacije][new-web-app]

6. Unesite vrijednosti za novu bazu podataka, uključujući pristanete pravne uvjete.

    ![Stvaranje nove baze podataka MySQL][new-mysql-db]

7. Prilikom stvaranja web-aplikaciji vidjet ćete novi plohu aplikaciju za web.

7. U odjeljku **Postavke** kliknite **Neprekinuto implementaciju**, a zatim kliknite _Konfiguriraj potrebne postavke_.

    ![Postavljanje brojka objavljivanja][setup-publishing]

8. Odaberite **Lokalnom spremištu brojka** za izvor.

    ![Postavljanje brojka spremište][setup-repository]


9. Da biste omogućili objavljivanje brojka, navedite korisničko ime i lozinku. Zabilježite korisničko ime i lozinka koje ste stvorili. (Ako ste postavili spremište brojka prije, ovaj korak će se preskočiti.)

    ![Stvaranje vjerodajnica za objavljivanje][credentials]


## <a name="get-remote-mysql-connection-information"></a>Dohvati podatke o vezi udaljene MySQL

Povezivanje s bazom podataka MySQL koji se izvodi u web-aplikacijama, vaše će potrebne informacije o vezi. Da biste dobili informacije o povezivanju MySQL, slijedite ove korake:

1. Grupa resursa, kliknite bazu podataka:

    ![Odaberite bazu podataka][select-database]

2. Iz baze podataka **Postavke**odaberite **Svojstva**.

    ![Odaberite svojstva][select-properties]

2. Zabilježite vrijednosti za `Database`, `Host`, `User Id`, i `Password`.

    ![Napomena svojstva][note-properties]

## <a name="build-and-test-your-app-locally"></a>Stvaranje i testiranje lokalno aplikacije

Sad kad ste stvorili web-aplikacijama, možete razvoj aplikacija na lokalno, a zatim implementacija nakon testiranja.

Registracija aplikacije je jednostavno aplikacija i koja omogućuje vam da biste registrirali za događaj unosom adrese ime i e-pošte. Informacije o prethodno registrants prikazuju se u tablici. Informacije o registraciji pohranjuju u bazi podataka MySQL. Aplikacija se sastoji od jedne datoteke (kopirajte i zalijepite kod dostupna ispod):

* **index.php**: prikazuje obrazac za registraciju i tablice s podacima za registrant.

Omogućuje stvaranje i lokalno pokrenuti aplikaciju, slijedite korake u nastavku. Imajte na umu sljedeće korake pretpostavlja PHP i postavljanje na lokalno računalo MySQL naredbenog retka alat (dio MySQL), a koje ste omogućili [Ako je PDO proširenja za MySQL][pdo-mysql].

1. Povezivanje s udaljeni poslužitelj MySQL pomoću vrijednosti za `Data Source`, `User Id`, `Password`, a `Database` ranije dohvaćene:

        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. Pojavit će se MySQL naredbeni redak:

        mysql>

3. Lijepljenje u nastavku `CREATE TABLE` naredbu da biste stvorili na `registration_tbl` tablice u bazi podataka:

        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. U korijenskoj mapi lokalnog računala stvorite **index.php** datoteke.

5. Otvorite **index.php** datoteku u uređivaču teksta ili IDE dodati sljedeći kod i dovršili potrebne promjene koji je označen `//TODO:` komentare.


        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
            if(!empty($_POST)) {
            try {
                $name = $_POST['name'];
                $email = $_POST['email'];
                $date = date("Y-m-d");
                // Insert data
                $sql_insert = "INSERT INTO registration_tbl (name, email, date)
                           VALUES (?,?,?)";
                $stmt = $conn->prepare($sql_insert);
                $stmt->bindValue(1, $name);
                $stmt->bindValue(2, $email);
                $stmt->bindValue(3, $date);
                $stmt->execute();
            }
            catch(Exception $e) {
                die(var_dump($e));
            }
            echo "<h3>Your're registered!</h3>";
            }
            // Retrieve data
            $sql_select = "SELECT * FROM registration_tbl";
            $stmt = $conn->query($sql_select);
            $registrants = $stmt->fetchAll();
            if(count($registrants) > 0) {
                echo "<h2>People who are registered:</h2>";
                echo "<table>";
                echo "<tr><th>Name</th>";
                echo "<th>Email</th>";
                echo "<th>Date</th></tr>";
                foreach($registrants as $registrant) {
                    echo "<tr><td>".$registrant['name']."</td>";
                    echo "<td>".$registrant['email']."</td>";
                    echo "<td>".$registrant['date']."</td></tr>";
                }
                echo "</table>";
            } else {
                echo "<h3>No one is currently registered.</h3>";
            }
        ?>
        </body>
        </html>

4.  U je terminal Idi u mapu aplikacije, a zatim upišite sljedeću naredbu:

        php -S localhost:8000

Sada možete se prebacivati na **http://localhost:8000 /** da biste testirali aplikacije.


## <a name="publish-your-app"></a>Objavljivanje aplikacije

Nakon testirate aplikacije lokalno, možete ga objaviti na web-aplikacije pomoću brojka. Bit će pokrenuti na lokalnom spremištu brojka i objavljivanje aplikacija.

> [AZURE.NOTE]
> To su isti se koraci prikazani na portalu Azure na kraju Stvori web-aplikacijama i postavljanje gore brojka objavljivanje sekciji iznad.

1. (Neobavezno)  Ako ste zaboravili ili izgubili brojka udaljene repostitory URL-a, idite na web-aplikacije svojstva portala za Azure.

1. Otvorite GitBash (ili terminal, ako je brojka u vašem `PATH`), promijenite direktorija u korijenskom direktoriju aplikacije i izvršite sljedeće naredbe:

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    Će se tražiti lozinku koju ste ranije stvorili.

    ![Početna automatske Azure putem brojka][git-initial-push]

2. Idite na **http://[site name].azurewebsites.net/index.php** da biste počeli koristiti aplikaciju (ti podaci će se spremiti na nadzornu ploču računa):

    ![Azure i web-mjesta][running-app]

Nakon što ste objavili aplikacije, možete započeti s promjenama i brojka koristite da biste objavili ih.

## <a name="publish-changes-to-your-app"></a>Objavite promjene u aplikaciju

Da biste objavili promjene na aplikaciju, slijedite ove korake:

1. Unesite promjene u aplikaciju za lokalno.
2. Otvorite GitBash (ili je terminal it brojka je u vašem `PATH`), promijenite direktorija u korijenskom direktoriju aplikacije i izvršite sljedeće naredbe:

        git add .
        git commit -m "comment describing changes"
        git push azure master

    Će se tražiti lozinku koju ste ranije stvorili.

    ![Odaberite promjena web-mjesta za Azure putem brojka][git-change-push]

3. Idite na **http://[site name].azurewebsites.net/index.php** da biste vidjeli aplikaciju i sve promjene koje ste napravili:

    ![Azure i web-mjesta][running-app]

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

<a name="composer"></a>
## <a name="enable-composer-automation-with-the-composer-extension"></a>Omogućivanje Skladatelj automatizacije s datotečnim nastavkom Skladatelj

Prema zadanim postavkama, postupak implementacije brojka aplikacije servisa za nema ugrađenu bilo čega što sadrži composer.json, ako postoji u projektu i. Možete omogućiti composer.json tijekom obrade `git push` omogućivanjem Skladatelj datotečni nastavak.

1. U vašem i web-aplikacije plohu [Azure portal][management-portal], kliknite **Alati** > **proširenja**.

    ![Postavke za nastavak Skladatelj][composer-extension-settings]

2. Kliknite **Dodaj**, a zatim kliknite **Skladatelj**.

    ![Dodavanje Skladatelj proširenja][composer-extension-add]
    
3. Kliknite **u redu** da biste prihvatili uvjete za pravne. Kliknite **u redu** da biste dodali datotečni nastavak.

    **Instalirano proširenja** plohu prikazivat će se sada Skladatelj nastavak.  
    ![Prikaz Skladatelj proširenja][composer-extension-view]
    
4. Sada, izvršavati `git add`, `git commit`, a `git push` kao u prethodnom odjeljku. Sada vidjet ćete da Skladatelj instalira ovisnosti koji su definirani u composer.json.

    ![Skladatelj kućni broj uspjeha][composer-extension-success]

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u [Centru za razvojne inženjere i](/develop/php/).

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
