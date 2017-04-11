<properties 
    pageTitle="Stvaranje web-aplikacijama PHP MySQL u aplikacije servisa za Azure i implementacija pomoću FTP" 
    description="Praktični vodič koji pokazuje kako stvoriti i web-aplikacije koje se sprema podatke u MySQL i korištenje implementacije FTP Azure." 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>


#<a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>Stvaranje web-aplikacijama PHP MySQL u aplikacije servisa za Azure i implementacija pomoću FTP

Pomoću ovog praktičnog vodiča pokazuje kako stvoriti i MySQL web-aplikacijama i kako implementirati pomoću FTP. Pomoću ovog praktičnog vodiča pretpostavlja da imate [i][install-php], [MySQL][install-mysql], web-poslužitelj i FTP klijent instaliran na vašem računalu. Na bilo kojem operacijskom sustavu, uključujući Windows, Mac i Linux mogu slijediti upute u ovom ćete praktičnom vodiču. Nakon dovršetka ovaj vodič, imat ćete web-aplikacijama PHP/MySQL izvodi u Azure.
 
Saznat ćete:

* Upute za stvaranje web-aplikacijama i bazom podataka MySQL pomoću portala za Azure. Budući da PHP po zadanom je omogućena u web-aplikacijama, ništa posebno je potreban za pokretanje koda PHP.
* Kako objaviti aplikaciju Azure pomoću FTP.
 
Slijedeći ovog praktičnog vodiča će Stvaranje jednostavne Registracija web app u PHP. Aplikacija će se nalaziti u web-aplikaciji. Snimka zaslona dovršene aplikacija je ispod:

![Azure i Web-mjesta][running-app]

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice obavezna, bez preuzete obveze. 


##<a name="create-a-web-app-and-set-up-ftp-publishing"></a>Stvaranje web-aplikacijama i postavljanje FTP objavljivanja

Slijedite ove korake da biste stvorili web-aplikacijama i bazom podataka MySQL:

1. Prijavite se na [Portal za Azure][management-portal].
2. Kliknite ikonu **+ Novo** u gornjem lijevom kutu Portal za Azure.

    ![Stvaranje novog Azure Web-mjesta][new-website]

3. U okvir pretražite upišite **web-aplikacije + MySQL** pa kliknite **web-aplikacije + MySQL**.

    ![Prilagođeni Stvori novo Web-mjesto][custom-create]

4. Kliknite **Stvori**. Unesite naziv za servis jedinstveni aplikacije, valjani naziv za grupu resursa i novoj tarifi za servis.

    ![Naziv grupe za skup resursa][resource-group]


6. Unesite vrijednosti za novu bazu podataka, uključujući pristanete pravne uvjete.

    ![Stvaranje nove baze podataka MySQL][new-mysql-db]
    
7. Prilikom stvaranja web-aplikaciji vidjet ćete novi plohu servisa aplikacija.


6. Kliknite **Postavke** > **implementacije vjerodajnice**. 

    ![Postavljanje vjerodajnica za implementaciju][set-deployment-credentials]

7. Da biste omogućili objavljivanje FTP, navedite korisničko ime i lozinku. Spremite vjerodajnice i zabilježite korisničko ime i lozinka koje ste stvorili.

    ![Stvaranje vjerodajnica za objavljivanje][portal-ftp-username-password]

##<a name="build-and-test-your-app-locally"></a>Stvaranje i testiranje lokalno aplikacije

Registracija aplikacije je jednostavno aplikacija i koja omogućuje vam da biste registrirali za događaj unosom adrese ime i e-pošte. Informacije o prethodno registrants prikazuju se u tablici. Informacije o registraciji pohranjuju u bazi podataka MySQL. Aplikaciju sastoji se od dvije datoteke:

* **index.php**: prikazuje obrazac za registraciju i tablice s podacima za registrant.
* **createtable.php**: Stvaranje tablice MySQL za aplikaciju. Datoteka će se koristiti samo jedanput.

Omogućuje stvaranje i pokretanje aplikacije lokalno, slijedite korake u nastavku. Imajte na umu da se korake pretpostavlja da imate i MySQL, a na web-poslužitelj postavljanje na lokalno računalo, a koji ste omogućili [Ako je PDO proširenja za MySQL][pdo-mysql].

1. Stvaranje baze podataka MySQL pod nazivom `registration`. To možete učiniti iz naredbenog retka MySQL s sljedeću naredbu:

        mysql> create database registration;

2. U korijenskom direktoriju web-poslužitelj, stvorite mapu naziva `registration` i stvaranje dvije datoteke u njemu – jedan pod nazivom `createtable.php` i pod nazivom `index.php`.

3. Otvaranje u `createtable.php` datoteku u uređivaču teksta ili IDE i dodavanje koda u nastavku. Kod će se koristiti za stvaranje na `registration_tbl` tablice u na `registration` baze podataka.

        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
                        PRIMARY KEY(id),
                        name VARCHAR(30),
                        email VARCHAR(30),
                        date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    > [AZURE.NOTE] 
    > Morat ćete ažuriranje vrijednosti za <code>$user</code> i <code>$pwd</code> s lokalnom MySQL korisničko ime i lozinku.

4. Otvorite web-preglednik i idite [http://localhost/registration/createtable.php][localhost-createtable]. Time ćete stvoriti na `registration_tbl` tablice u bazi podataka.

5. Otvorite **index.php** datoteku u uređivaču teksta ili IDE i dodajte osnovni HTML i CSS kod za stranicu (i kod dodat će se u koracima od noviji).

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

        ?>
        </body>
        </html>

6. Unutar oznaka i dodavanje koda za PHP za povezivanje s bazom podataka.

        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    > [AZURE.NOTE]
    > Ponovno će morati ažurirati vrijednosti za <code>$user</code> i <code>$pwd</code> s lokalnom MySQL korisničko ime i lozinku.

7. Sljedeći kod vezu baze podataka, dodavanje koda za umetanje informacije o registraciji u bazu podataka.

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

8. Na kraju, slijedeći gore navedeni kod dodavanje koda za dohvaćanje podataka iz baze podataka.

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

Sada možete pregledavati [http://localhost/registration/index.php] [ localhost-index] da biste testirali aplikaciju.

##<a name="get-mysql-and-ftp-connection-information"></a>Zatražite MySQL i FTP podatke za povezivanje

Povezivanje s bazom podataka MySQL koji se izvodi u web-aplikacijama, vaše će potrebne informacije o vezi. Da biste dobili informacije o povezivanju MySQL, slijedite ove korake:

1. Iz aplikacije servisa plohu web app kliknite na vezu za grupu resursa:

    ![Odaberite grupu resursa][select-resourcegroup]

1. Grupa resursa, kliknite bazu podataka:

    ![Odaberite bazu podataka][select-database]

2. Iz baze podataka sažetka, odaberite **Postavke** > **Svojstva**.

    ![Odaberite svojstva][select-properties]
    
2. Zabilježite vrijednosti za `Database`, `Host`, `User Id`, i `Password`.

    ![Napomena svojstva][note-properties]

3. Iz web-aplikacije, kliknite vezu za **Preuzimanje objavljivanje profila** pri donjem desnom kutu stranice:

    ![Preuzimanje objavljivanje profila][download-publish-profile]

4. Otvaranje u `.publishsettings` datoteke u XML uređivaču. 

3. Pronalaženje na `<publishProfile >` element s `publishMethod="FTP"` koji izgleda ovako:

        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>
    
Zabilježite na `publishUrl`, `userName`, a `userPWD` atribute.

##<a name="publish-your-app"></a>Objavljivanje aplikacije

Nakon testirate aplikacije lokalno, možete ga objaviti na web-aplikaciju pomoću FTP. Međutim, najprije morate ažurirati podatke o vezi baze podataka u aplikaciji. Korištenje baze podataka podatke o vezi ste nabavili ranije (u odjeljku **Početak MySQL i podatke o vezi FTP** ), ažurirajte sljedeće podatke u **i** na `createdatabase.php` i `index.php` datoteke s odgovarajuće vrijednosti:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Sada ste spremni za objavljivanje aplikacije pomoću FTP.

1. Otvorite FTP klijent po izboru.

2. Upišite *dio naziva glavnog računala* s na `publishUrl` atribut ste zabilježili iznad u FTP klijent.

3. Unesite u `userName` i `userPWD` atribute koje ste zabilježili iznad nepromijenjena u FTP klijent.

4. Uspostaviti vezu.

Nakon povezivanja moći za prijenos i preuzimanje datoteka prema potrebi. Provjerite je li se prijenos datoteka u korijenskom direktoriju, što je `/site/wwwroot`.

Nakon prenošenja i `index.php` i `createtable.php`, pronađite **http://[site name].azurewebsites.net/createtable.php** da biste stvorili tablicu MySQL za aplikaciju, a zatim pronađite **http://[site name].azurewebsites.net/index.php** da biste počeli koristiti aplikaciju.
 
## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u [Centru za razvojne inženjere PHP](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png
 
