<properties
    pageTitle="Python web-aplikacije s Django | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča ručica za glavno računalo web-mjesto Django utemeljen na Azure pomoću programa Windows Server 2012 R2 Standard virtualnog računala pomoću klasične implementacije modela."
    services="virtual-machines-windows"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>


<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="huvalo"/>


# <a name="django-hello-world-web-application-on-a-windows-server-vm"></a>Django pozdrav svijeta web-aplikaciju na VM za poslužitelj sustava Windows

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac i Linux](virtual-machines-linux-python-django-web-app.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Pomoću ovog praktičnog vodiča opisuje Glavno računalo web-mjesto Django utemeljen na Microsoft Azure pomoću Windows Server virtualnog računala. Pomoću ovog praktičnog vodiča pretpostavlja da imate bez rad Azure prethodnog sa. Po dovršetku ovog praktičnog vodiča, imat ćete aplikacije utemeljene na Django prema gore i pokretanje u oblaku.

Saznat ćete kako:

* Postavljanje Azure virtualnog računala glavno računalo za Django. Tijekom ovog praktičnog vodiča objašnjava kako to postigli u odjeljku Windows Server, isti nije moguće i napraviti s Linux VM smješten u Azure.
* Stvaranje nove Django aplikacije iz sustava Windows.

Slijedeći ovog praktičnog vodiča će izrađivati jednostavne pozdrav svijeta web-aplikacije. Aplikacija će se nalaziti u Azure virtualnog računala.

Snimka zaslona dovršene aplikacija prikazuje se sljedeći.

![Prikaz stranice svijeta pozdrav na Azure prozoru preglednika][1]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Stvaranje i konfiguriranje Azure virtualnog računala glavno računalo za Django

1. Slijedite upute navedeni [u nastavku](virtual-machines-windows-classic-tutorial.md) da biste stvorili Azure virtualnog računala distribucije Windows Server 2012 R2 podatkovnog centra.

1. Uputite Azure da biste usmjerili priključak 80 promet s weba priključak 80 virtualnog računala:
 - Dođite do novostvorenu virtualnog računala na portalu za Azure klasični, a zatim kliknite karticu **krajnje točke** .
 - Kliknite gumb **DODAJ** pri dnu zaslona.
    ![Dodavanje krajnje točke](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-addendpoint.png)

 - Otvaranje kopije protokolom **TCP** je **JAVNO PRIKLJUČAK 80** kao **PRIVATNE PRIKLJUČAK 80**.
![][port80]
1. Kartica **nadzorna PLOČA** kliknite **POVEŽI se** s pomoću **Udaljene radne površine** daljinski prijaviti novostvorenu Azure virtualnog računala.  

**Važnih Napomena:** Sve upute u nastavku pretpostavlja da ste se prijavili u virtualnog računala pravilno i izdavanja nema naredbi, a ne na lokalno računalo.

## <a id="setup"> </a>Instalacije Python, Django, WFastCGI

**Bilješke:** Da biste preuzeli pomoću preglednika Internet Explorer možda ćete morati Konfiguriranje postavki preglednika Internet Explorer ESC (Pokreni/Administrativni alati/poslužitelj Upravitelj/lokalni poslužitelj, zatim **Poboljšana konfiguracija sigurnosti preglednika Internet Explorer**, postavite na isključeno).

1. Instalirajte najnovije 2.7 Python ili 3.4 iz [python.org][].
1. Instaliranje paketa wfastcgi i django pomoću točaka.

    Za Python 2.7, koristite sljedeću naredbu.

        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django

    Za Python 3.4, koristite sljedeću naredbu.

        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="installing-iis-with-fastcgi"></a>Instaliranje servisa IIS s FastCGI

1. Instalirajte IIS s podrškom za FastCGI.  To može potrajati nekoliko minuta za izvršavanje.

        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="creating-a-new-django-application"></a>Stvaranje nove aplikacije Django

1.  Iz *C:\inetpub\wwwroot*, unesite sljedeću naredbu da biste stvorili novi projekt Django:

    Za Python 2.7, koristite sljedeću naredbu.

        C:\Python27\Scripts\django-admin.exe startproject helloworld

    Za Python 3.4, koristite sljedeću naredbu.

        C:\Python34\Scripts\django-admin.exe startproject helloworld

    ![Rezultat naredbe nova AzureService](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-cmd-new-azure-service.png)

1.  Naredba **django administrator** stvara osnovna struktura utemeljen na Django web-mjesta:

  -   **helloworld\manage.PY** pomaže vam pokretanje hostiranje i zaustavljanje hosting utemeljen na Django web-mjesta
  -   **helloworld\helloworld\settings.PY** sadrži Django postavke za aplikaciju.
  -   **helloworld\helloworld\urls.PY** sadrži kod mapiranja između sve URL-ove i njegov prikaz.

1.  Stvorite novu datoteku pod nazivom **views.py** u direktoriju *C:\inetpub\wwwroot\helloworld\helloworld* . Sadržavat će prikaz koji prikazuje stranici "Zdravo svijete". Pokrenite uređivač i unesite sljedeće:

        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Zamijenite sadržaj datoteke urls.py sljedeće.

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )

## <a name="configuring-iis"></a>Konfiguriranje programa IIS

1. Otključavanje odjeljak rukovatelja u globalni applicationhost.config.  To će omogućiti korištenje python događajima u vašem web.config.

        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers

1. Omogućivanje WFastCGI.  Ovo će dodati aplikaciju globalni applicationhost.config koje se odnosi na Python tumačenja izvršna i skripte wfastcgi.py.

    Python 2.7:

        c:\python27\scripts\wfastcgi-enable

    Python 3.4:

        c:\python34\scripts\wfastcgi-enable

1. Stvaranje datoteke web.config u *C:\inetpub\wwwroot\helloworld*.  Vrijednost na `scriptProcessor` atributa mora podudarati Izlaz u prethodnom koraku.  Posjetite stranicu za [wfastcgi][] na pypi za dodatne postavke na wfastcgi.

    Python 2.7:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

    Python 3.4:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

1. Ažurirajte mjesto od IIS zadani Web-mjesta pokažite na mapu django projekta.

        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"

1. Na kraju, učitati web-stranicu u pregledniku.

![Prikaz stranice svijeta pozdrav na Azure prozoru preglednika][1]


## <a name="shutting-down-your-azure-virtual-machine"></a>Isključuje Azure virtualnog računala

Kada završite s ovom ćete praktičnom vodiču, isključite i/ili uklanjanje vaše novostvorenu Azure virtualnog računala da biste oslobodili resurse za druge vodiče i izbjegli povećavanja Azure ispravnim.

[1]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
