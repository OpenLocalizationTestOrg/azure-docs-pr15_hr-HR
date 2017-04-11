<properties
    pageTitle="Stvaranje i povezivanje s bazom podataka MySQL u Azure"
    description="Upute za stvaranje baze podataka MySQL i povežite se s njom iz web-aplikacije i u Azure pomoću portala za Azure."
    documentationCenter="php"
    services="app-service\web"
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm;cephalin"/>

# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Stvaranje i povezivanje s bazom podataka MySQL u Azure

Pomoću ovog praktičnog vodiča prikazuje kako stvoriti bazom podataka MySQL [Azure portal](https://portal.azure.com) (Davatelj je [ClearDB](http://www.cleardb.com/)) te kako se povezati s web-aplikacijama i izvodi u [Aplikacije servisa za Azure](./app-service/app-service-value-prop-what-is.md). 

> [AZURE.NOTE] Možete stvoriti i bazom podataka MySQL kao dio [trgovine predložak za aplikaciju](./app-service-web/app-service-web-create-web-app-from-marketplace.md).

## <a name="create-a-mysql-database-in-azure-portal"></a>Stvaranje baze podataka MySQL Azure portalu

Da biste stvorili bazom podataka MySQL na portalu za Azure, učinite sljedeće:

1. Prijavite se na [portal za Azure](https://portal.azure.com).

2. Na lijevom izborniku kliknite **Novo** > **podataka + prostor za pohranu** > **Baze podataka MySQL**.

    ![Stvaranje baze podataka MySQL u Azure - početka](./media/store-php-create-mysql-database/create-db-1-start.png)

2. U novoj bazi podataka MySQL [plohu](azure-portal-overview.md)konfigurirati nove baze podataka MySQL na sljedeći način (*plohu*: stranici portala pripadnom vodoravno):

    - **Naziv baze podataka**: upišite naziv služi za identifikaciju osobe
    - **Pretplate**: Odaberite pretplatu za korištenje
    - **Vrsta baze podataka**: odaberite **zajednički se koristi** za najniža trošak ili besplatne razine ili **Dedicated** da biste dobili namjenski resursi. 
    - **Grupa resursa**: Dodavanje baze podataka MySQL u postojeću [grupu resursa](../azure-resource-manager/resource-group-overview.md) ili staviti u novi. Resursa u istoj grupi jednostavno upravlja zajedno.
    - **Lokacija**: Odaberite mjesto blizu vam. Prilikom dodavanja u postojeću grupu resursa, koristite zaključana mjesto grupu resursa.
    - **Cijene sloju**: kliknite **Sloju cijene**, a zatim odaberite mogućnost određivanja (**žive** sloju je besplatni), a zatim **Odaberite**. 
    - **Pravne uvjeta**: kliknite **Pravne uvjete**, pregledajte detalje za kupnju pa kliknite **kupite**.
    - **Prikvači na nadzornoj ploči**: odaberite ako želite da biste pristupili izravno s nadzorne ploče. Ovo je posebno korisno ako niste upoznati s portala navigacije još.
    
    Snimka zaslona za sljedeće je samo primjera kako možete konfigurirati baze podataka MySQL.  
    ![Stvaranje baze podataka MySQL u Azure - konfiguriranje](./media/store-php-create-mysql-database/create-db-2-configure.png)

3. Kada završite konfiguriranje, kliknite **Stvori**.

    ![Stvaranje baze podataka MySQL u Azure – stvaranje](./media/store-php-create-mysql-database/create-db-3-create.png)

    Prikazat će se skočna obavijest obavještava znate da je počeo implementacije.

    ![Stvaranje baze podataka MySQL u Azure – u tijeku](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Dobit druge skočni kada je uspio implementacije. Na portalu će otvoriti i vaše plohu baze podataka MySQL automatski.

<a name="connect"></a>
## <a name="connect-to-your-mysql-database"></a>Povezivanje baze podataka MySQL

Da biste vidjeli informacije o vezi za novu bazu podataka MySQL, samo kliknite **Svojstva** u plohu web app.
    
![Stvaranje baze podataka MySQL u Azure - plohu baze podataka MySQL](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Sada možete koristiti te informacije o vezi u bilo koju aplikaciju za web. Uzorak koji pokazuje kako koristiti podatke za povezivanje iz jednostavne aplikacije i dostupna je [u nastavku](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>Povezivanje web-aplikacijama Laravel (iz i početak rada vodič)

Pretpostavimo da samo završite vodiča [Stvaranje, konfiguriranje i implementacija web-aplikacijama i za Azure](./app-service-web/app-service-web-php-get-started.md) , a imate web-aplikacijama [Laravel](https://www.laravel.com/) izvodi u Azure. Mogućnosti baze podataka možete jednostavno dodati Laravel aplikacije. Slijedite korake u nastavku:

>[AZURE.NOTE] Na sljedeći način pretpostavlja da ste završili vodiča [Stvaranje, konfiguriranje i implementacija web-aplikacijama i za Azure](./app-service-web/app-service-web-php-get-started.md).

1. Konfiguriranje Laravel aplikacije u okruženju sustava lokalne razvoj tako da pokazuje na baze podataka MySQL. Da biste to učinili, otvorite `.env` iz aplikacije Laravel korijenski direktorij i konfiguriranje mogućnosti baze podataka MySQL.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

    >[AZURE.NOTE] Naziv baze podataka MySQL u plohu **Svojstva** možda ili možda neće biti ono koje je prikazano u polje **Naziv baze podataka** . Je bolje da biste provjerili parametar baze podataka u polje **NIZA za POVEZIVANJE** . 
    >
    >![Stvaranje baze podataka MySQL u Azure – u tijeku](./media/store-php-create-mysql-database/connect-db-1-database-name.png)

2. Najbrži način da biste provjerili imate li MySQL pristup sada je za korištenje [Laravel na zadani provjere autentičnosti scaffolding](https://laravel.com/docs/5.2/authentication#authentication-quickstart). U naredbeni redak terminal iz aplikacije Laravel korijenski direktorij izvršite sljedeće naredbe:

         php artisan migrate
         php artisan make:auth

    Prva naredba stvara tablice u Azure utemeljenog na unaprijed definirane migracije u na `database/migrations` direktorija i drugi naredba scaffolds osnovni prikazi i usmjerava za registraciju za korisnika i provjeru autentičnosti.

3. Odmah pokrenite razvoj poslužitelja:

        php artisan serve

4. U pregledniku idite na http://localhost:8000 i registrirali novog korisnika, kao što je prikazano:

    ![Povezivanje s bazom podataka MySQL u Azure - Registracija korisnika](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Slijedite dovršeno upit za korisničko Sučelje registracije. Nakon dovršetka registraciju koju zapisat će se u.
    
    ![Povezivanje s bazom podataka MySQL u Azure - Registracija korisnika](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Pokrenite aplikaciju za bazu podataka MySQL u Azure sada razvijate.

5. Sada, morate samo za replikaciju vaše `.env` postavke na Azure web-aplikaciju. Pokrenite sljedeće naredbe Azure EŽA:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

    Saznajte kako to funkcionira u članku [Konfiguriranje Azure web-aplikaciji](./app-service-web/app-service-web-php-get-started.md#configure).

6. Nakon toga izvršavanje i automatske Azure lokalne promjene ranije tijekom rada `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master

7. Pronađite Azure web-aplikaciji.

        azure site browse

8. Prijavite se pomoću vjerodajnica korisnika koji ste prethodno stvorili.

    ![Povezivanje s bazom podataka MySQL u Azure – pregled Azure web App](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Kad se prijavite, trebali biste vidjeti neslužbeni nakon zaslon za prijavu u.
    
    ![Povezivanje s bazom podataka MySQL u Azure - prijavljeni](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Čestitamo, web-aplikaciju programa i servisu Azure je pristup podacima iz baze podataka MySQL. 

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u [Centru za razvojne inženjere i](/develop/php/).
