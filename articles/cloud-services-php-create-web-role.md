<properties
    pageTitle="Uloge i web- a tempiranja | Microsoft Azure"
    description="Vodič za stvaranje web- a tempiranja uloge i u Azure oblaku i konfiguriranje izvođenja i."
    services=""
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

#<a name="how-to-create-php-web-and-worker-roles"></a>Kako stvoriti i web- a tempiranja uloge

## <a name="overview"></a>Pregled

Ovaj vodič vidjet ćete kako stvoriti i weba ili tempiranja uloge u okruženje za razvoj sustava Windows, određenu verziju programa i birati "ugrađene" verzija, promijeniti konfiguraciju i, Omogući proširenja i na kraju, implementacija Azure. Također se objašnjava kako konfigurirati weba ili radnih ulogu želite koristiti i runtime (s prilagođena konfiguracija i proširenja) koje ste omogućili.

## <a name="what-are-php-web-and-worker-roles"></a>Što su i web- a tempiranja uloge?

Azure pruža tri izračunati modele za pokretanje aplikacija: aplikacije servisa za Azure, virtualnim računalima sustava Azure i Azure servise u Oblaku. Sve tri modeli podržavaju i. Servisi u oblaku, što obuhvaća uloge web i tempiranja, nudi *platforme kao service (PaaS)*. U oblaku, uloge web nudi namjenski web-poslužitelj Internet Information Services (IIS) glavnog računala sučelja web-aplikacije. Uloga suradnika možete pokrenuti asinkronog, dugoročnih ili trajna zadataka ovisi o interakcije s korisnikom ili unos.

Dodatne informacije o tim mogućnostima potražite u članku [izračunati hostinga mogućnosti nudi Azure](./cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>Preuzmite Azure SDK za PHP

[Azure SDK i] sastoji se od nekoliko komponenti. U ovom se članku će koristiti dvije od njih: Azure PowerShell i Azure Emulatora. Ove dvije komponente mogu instalirati putem instalacijskog programa Microsoft Web platforme. Dodatne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](powershell-install-configure.md).

## <a name="create-a-cloud-services-project"></a>Stvaranje projekta servise u Oblaku

Prvi korak pri stvaranju ulogu i weba ili radnih je da biste stvorili projekta programa servisa Azure. projekt programa servisa Azure služi kao logička spremnik za web, a tempiranja uloge, a sadrži datoteke u projekt [definicija servisa (.csdef)] i [Konfiguracija servisa (.cscfg)] .

Da biste stvorili novi projekt servisa Azure, Azure PowerShell pokrenite kao administrator, a zatim pokrenite sljedeću naredbu:

    PS C:\>New-AzureServiceProject myProject

Ta se naredba će stvoriti novi imenik (`myProject`) na koje možete dodati web- a tempiranja uloge.

## <a name="add-php-web-or-worker-roles"></a>Dodavanje i weba ili tempiranja uloge

Da biste dodali web uloge i projekta, pokrenite sljedeću naredbu iz u korijenskom direktoriju projekta:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Za ulogu suradnika, koristite sljedeću naredbu:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [AZURE.NOTE] Na `roleName` parametar nije obavezno. Ako je izostavljen, naziv uloge će automatski generira. Bit će prvi uloga web stvorili `WebRole1`, bit će drugi `WebRole2`i tako dalje. Bit će ulogu suradnika prvi stvorili `WorkerRole1`, bit će drugi `WorkerRole2`i tako dalje.

## <a name="specify-the-built-in-php-version"></a>Odredite ugrađene i verzije

Kada dodate ulogu i weba ili tempiranja s projektom, tako da se i instalirat će se na svaku instancu weba ili tempiranja aplikacije kada je u upotrebi mijenjaju se konfiguracijske datoteke u projekt. Da biste vidjeli verziju i koji će se instalirati prema zadanim postavkama, pokrenite sljedeću naredbu:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

Izlaz iz naredbu iznad izgledat će slično kao što je prikazano u nastavku. U ovom primjeru u `IsDefault` zastavice postavljen na `true` za PHP 5.3.17, koja označava da će biti instalirana i verzija za zadani.

    Runtime Version     PackageUri                      IsDefault
    ------- -------     ----------                      ---------
    Node 0.6.17         http://nodertncu.blob.core...   False
    Node 0.6.20         http://nodertncu.blob.core...   True
    Node 0.8.4          http://nodertncu.blob.core...   False
    IISNode 0.1.21      http://nodertncu.blob.core...   True
    Cache 1.8.0         http://nodertncu.blob.core...   True
    PHP 5.3.17          http://nodertncu.blob.core...   True
    PHP 5.4.0           http://nodertncu.blob.core...   False

Runtime verzija i možete postaviti u bilo koji od verzije i koji su navedeni. Na primjer, da biste postavili i verziju (uloge s nazivom `roleName`) da biste 5.4.0, koristite sljedeću naredbu:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [AZURE.NOTE] Verzija i u budućnosti može promijeniti.

## <a name="customize-the-built-in-php-runtime"></a>Prilagodba ugrađene i izvođenja

Omogućuje potpunu kontrolu nad konfiguriranje izvođenja i koje se instaliraju kada i slijedite korake iznad, uključujući promjenu `php.ini` postavki i omogućivanje proširenja.

Da biste prilagodili ugrađeni runtime PHP, slijedite ove korake:

1. Dodajte novu mapu pod nazivom `php`, prima u `bin` imeničkog svoje uloge web. Za ulogu suradnika ga dodati u ulogu korijenski direktorij.
2. U na `php` mapu, stvorite drugu mapu pod nazivom `ext`. Smještati `.dll` nastavak datoteke (npr., `php_mongo.dll`) koji želite omogućiti u ovu mapu.
3. Dodavanje u `php.ini` datoteke na `php` mapu. Omogući sve prilagođene proširenja i postavite sve upute i poslužitelja u ovoj datoteci. Na primjer, ako ste željeli da biste uključili `display_errors` na i omogućite na `php_mongo.dll` nastavak, sadržaj vaše `php.ini` datoteka će se na sljedeći način:

        display_errors=On
        extension=php_mongo.dll

> [AZURE.NOTE] Postavke koje se ne eksplicitno postavljene u na `php.ini` datoteke automatski unijeti će biti postavljena na njihove zadane vrijednosti. Međutim, imajte na umu koje možete dodati potpuni `php.ini` datoteku.

## <a name="use-your-own-php-runtime"></a>Koristite vlastitu i izvođenja
U nekim slučajevima, umjesto da odaberete ugrađene i izvođenja i konfiguriranju je opisan, preporučujemo vam pružaju vlastite izvođenja i. Na primjer, možete koristiti isti izvođenja i u web- ili radnih uloge koje koristite u razvojno okruženje. To olakšava da biste bili sigurni aplikacija ne mijenja ponašanje u vašem radnom okruženju.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Konfiguriranje web uloga da biste koristili vlastiti i izvođenja

Da biste konfigurirali web uloga da biste koristili izvođenja i koje ste omogućili, slijedite ove korake:

1. Stvaranje projekta programa servisa Azure i dodavanje uloge web i kao što je prethodno opisano u ovoj temi.
2. Stvaranje na `php` mape u na `bin` mapu koja u korijenskom direktoriju uloga web, a zatim dodajte i runtime (sve binarne datoteke konfiguracijske datoteke, podmape, itd.) na `php` mapu.
3. (NEOBAVEZNO) Ako vaš izvođenja i koristi [Microsoft upravljačke programe za i za SQL Server][sqlsrv drivers], morat ćete konfigurirati vaša uloga web instalirati [SQL Server nativni klijent 2012] [ sql native client] pri dodjeli. Da biste to učinili, dodajte [sqlncli.msi x64 instalacijski program] za na `bin` mape u korijenskom direktoriju uloga web. Što je opisano u sljedećem koraku skriptu za pokretanje pokrenut će se tihu instalacijski program kad je dodijeljena uloga. Ako vaš izvođenja i koristite Microsoft Drivers za PHP za SQL Server, možete ukloniti sljedeći redak iz skripte prikazano u sljedećem koraku:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Definiranje pokretanje zadatak koji se konfigurira [Internet Information Services (IIS)] [ iis.net] da biste koristili vaše izvođenja i rukovanja zahtjeva za `.php` stranice. Da biste to učinili, otvorite na `setup_web.cmd` datoteke (u na `bin` datoteka web uloga korijenski direktorij) u uređivaču teksta i njezin sadržaj zamijeniti sljedeću skriptu:

        @ECHO ON
        cd "%~dp0"

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
        SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"

5. Dodavanje datoteka aplikacije uloga web korijenski direktorij. To će biti korijenski direktorij web-poslužitelj.

6. Objavljivanje aplikacija kao što je opisano u odjeljku [Objavljivanje aplikacija](#how-to-publish-your-application) .

> [AZURE.NOTE] Na `download.ps1` skripte (u na `bin` mapu uloga web korijenski direktorij) moguće je izbrisati nakon što napravite korake gore opisanih za korištenje vlastitog izvođenja i.

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Konfiguriranje ulogu suradnika da biste koristili vlastiti izvođenja i

Da biste konfigurirali tempiranja uloga da biste koristili izvođenja i koje ste omogućili, slijedite ove korake:

1. Stvaranje projekta programa servisa Azure i dodavanje ulogu suradnika i kao što je prethodno opisano u ovoj temi.
2. Stvaranje na `php` mape u korijenskom direktoriju ulogu suradnika, pa dodajte i runtime (sve binarne datoteke konfiguracijske datoteke, podmape, itd.) na `php` mapu.
3. (NEOBAVEZNO) Ako vaš izvođenja i koristi [Microsoft upravljačke programe za i za SQL Server][sqlsrv drivers], morat ćete konfigurirati vaša uloga suradnika instalirati [SQL Server nativni klijent 2012] [ sql native client] pri dodjeli. Da biste to učinili, dodajte [sqlncli.msi x64 installer] ulogu suradnika korijenski direktorij. Što je opisano u sljedećem koraku skriptu za pokretanje pokrenut će se tihu instalacijski program kad je dodijeljena uloga. Ako vaš izvođenja i koristite Microsoft Drivers za PHP za SQL Server, možete ukloniti sljedeći redak iz skripte prikazano u sljedećem koraku:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Definiranje pokretanje zadatka koji dodaje vaš `php.exe` izvršna ulogu suradnika put okruženje varijabli kada je dodijeljena uloga. Da biste to učinili, otvorite na `setup_worker.cmd` datoteka (u korijenskom direktoriju ulogu suradnika) u uređivaču teksta i njezin sadržaj zamijeniti sljedeću skriptu:

        @echo on

        cd "%~dp0"

        echo Granting permissions for Network Service to the web root directory...
        icacls ..\ /grant "Network Service":(OI)(CI)W
        if %ERRORLEVEL% neq 0 goto error
        echo OK

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        setx Path "%PATH%;%~dp0php" /M

        if %ERRORLEVEL% neq 0 goto error

        echo SUCCESS
        exit /b 0

        :error

        echo FAILED
        exit /b -1

5. Dodavanje datoteka aplikacije ulogu suradnika korijenski direktorij.

6. Objavljivanje aplikacija kao što je opisano u odjeljku [Objavljivanje aplikacija](#how-to-publish-your-application) .

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Pokretanje aplikacije u Emulatora računalnim i prostora za pohranu

Azure Emulatora pružaju lokalnog okruženja u kojima možete testirati Azure aplikacije prije njegove implementacije kod u oblak. Postoje neke razlike između web-mjesto u Emulatora i u okvir za Azure okruženja. Da biste bolje razumjeli to potražite u članku [Korištenje emulator Azure prostora za pohranu za razvoj i testiranje](./storage/storage-use-emulator.md).

Imajte na umu da morate imati i instalirali lokalno da biste koristili emulator računalnim. Emulator računalnim će koristiti lokalne instalacije i da biste pokrenuli aplikaciju.

Da biste pokrenuli projekta u na Emulatora, pokrenite sljedeću naredbu iz korijenski direktorij projekta:

    PS C:\MyProject> Start-AzureEmulator

Prikazat će se izlazni sličnu ovoj:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Možete vidjeti vaše aplikacije koje se izvode u na emulator otvaranjem web-preglednik i pregledavanje lokalne adresom koja je prikazana u rezultatu (`http://127.0.0.1:81` u primjeru iznad).

Da biste prestali s Emulatora izvršavanje sljedeću naredbu:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Objavljivanje aplikacija

Da biste objavili aplikacije, morate najprije uvesti vaše postavke objavljivanja pomoću cmdleta [Uvoz AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) . Zatim možete objaviti svoju aplikaciju pomoću cmdleta [Objavi AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) . Informacije o prijavi u potražite u članku [kako instalirati i konfigurirati Azure PowerShell](powershell-install-configure.md).

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u [Centru za razvojne inženjere i](/develop/php/).

[Azure SDK PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[Definicija servisa (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[Konfiguracija servisa (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[Instalacijski program SQLNCli.msi x64]: http://go.microsoft.com/fwlink/?LinkID=239648
