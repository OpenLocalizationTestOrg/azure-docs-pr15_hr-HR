<properties 
    pageTitle="Python web-aplikacije s Django na Linux | Microsoft Azure" 
    description="Saznajte kako se hostira na Django web-aplikaciju utemeljenu na Azure pomoću Linux virtualnog računala." 
    services="virtual-machines-linux" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="huvalo"/>
    
# <a name="django-hello-world-web-application-on-a-linux-vm"></a>Django pozdrav svijeta web-aplikaciju na Linux VM

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac i Linux](virtual-machines-linux-python-django-web-app.md)

<br>

Pomoću ovog praktičnog vodiča opisuje Glavno računalo web-mjesto Django utemeljen na Microsoft Azure pomoću Linux virtualnog računala. Pomoću ovog praktičnog vodiča pretpostavlja da imate bez rad Azure prethodnog sa. Nakon dovršetka ovaj vodič, imat ćete aplikacije utemeljene na Django prema gore i pokretanje u oblaku.

Saznat ćete kako:

* Postavljanje Azure virtualnog računala glavno računalo za Django. Tijekom ovog praktičnog vodiča objašnjava kako to postigli u odjeljku **Linux**, isti nije moguće i napraviti s VM poslužitelja za Windows smješten u Azure. 
* Stvaranje nove aplikacije Django iz Linux.

Slijedeći ovog praktičnog vodiča će izrađivati jednostavne pozdrav svijeta web-aplikacije. Aplikacija će se nalaziti u Azure virtualnog računala.

Snimka zaslona dovršene aplikacija je ispod:

![Prikaz stranice svijeta pozdrav na Azure prozoru preglednika](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Stvaranje i konfiguriranje Azure virtualnog računala glavno računalo za Django

1. Slijedite upute navedeni [u nastavku](virtual-machines-linux-quick-create-portal.md) da biste stvorili Azure virtualnog računala *Ubuntu Server 14.04 LTS* distribucije.  Ako želite, možete odabrati provjeru autentičnosti lozinke umjesto SSH javni ključ.

1. Uređivanje sigurnosne grupe mreže da dopušta promet za dolazne http priključak 80 prema uputama [u nastavku](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

1. Prema zadanim postavkama, novi virtualnog računala ne obuhvaća na potpuno kvalificirani naziv domene (FQDN).  Ne možete stvoriti slijedeći upute [u nastavku](virtual-machines-linux-portal-create-fqdn.md).  Ovaj korak nije obavezan za dovršetak ovog praktičnog vodiča.

## <a id="setup"> </a>Postavljanje razvojno okruženje

**Bilješke:** Ako je potrebno za instalaciju Python ili želite omogućiti korištenje biblioteka za klijenta, pročitajte članak u [Vodiču za instalaciju Python](../python-how-to-install.md).

Ubuntu Linux VM već u sklopu Python 2.7 unaprijed instaliran, ali ne sadrži Apache ili django instaliran.  Slijedite ove korake za povezivanje s vašeg VM i instalirati Apache i django.

1.  Pokrenite novi prozor **Terminal** .
    
1.  Unesite sljedeću naredbu za povezivanje s Azure VM.  Ako niste stvorili na FQDN, možete se povezati pomoću javnu IP adresu velikim sažetka na portalu za Azure klasični virtualnog računala.

        $ ssh yourusername@yourVmUrl

1.  Unesite sljedeće naredbe da biste instalirali django:

        $ sudo apt-get install python-setuptools python-pip
        $ sudo pip install django

1.  Unesite sljedeću naredbu da biste instalirali Apache s mod wsgi:

        $ sudo apt-get install apache2 libapache2-mod-wsgi


## <a name="creating-a-new-django-application"></a>Stvaranje nove aplikacije Django

1.  Otvorite prozor **Terminal** ste koristili u prethodnom odjeljku da biste ssh u vašem VM.
    
1.  Unesite sljedeće naredbe da biste stvorili novi projekt Django:

        $ cd /var/www
        $ sudo django-admin.py startproject helloworld

    Skripta **django admin.py** generira osnovna struktura utemeljen na Django web-mjesta:
    -   **helloworld/Manage.PY** pomaže vam pokretanje hostiranje i zaustavljanje hosting utemeljen na Django web-mjesta
    -   **helloworld/helloworld/Settings.PY** sadrži Django postavke za aplikaciju.
    -   **helloworld/helloworld/URLs.PY** sadrži kod mapiranja između sve URL-ove i njegov prikaz.

1.  Stvorite novu datoteku pod nazivom **views.py** u direktoriju **/var/www/helloworld/helloworld** . Sadržavat će prikaz koji prikazuje stranici "Zdravo svijete". Pokrenite uređivač i unesite sljedeće:
        
        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Sada zamijenite sadržaj datoteke **urls.py** sljedeće:

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )


## <a name="setting-up-apache"></a>Postavljanje Apache

1.  Stvaranje programa Apache virtualnog glavnog računala konfiguracije datoteke **/etc/apache2/sites-available/helloworld.conf**. Postavite sadržaja na sljedeće, a *yourVmName* zamijenite stvarni naziv računala koristite (na primjer *pyubuntu*).

        <VirtualHost *:80>
        ServerName yourVmName
        </VirtualHost>
        WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
        WSGIPythonPath /var/www/helloworld

1.  Omogućivanje web-mjesta pomoću sljedeće naredbe:

        $ sudo a2ensite helloworld

1.  Ponovno pokrenite Apache pomoću sljedeće naredbe:

        $ sudo service apache2 reload

1.  Na kraju, učitati web-stranicu u pregledniku:

    ![Prikaz stranica svijeta pozdrav na Azure prozoru preglednika](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)


## <a name="shutting-down-your-azure-virtual-machine"></a>Isključuje Azure virtualnog računala

Kada završite s ovaj Praktični vodič, zatvaranja i/ili uklanjanje vaše novostvorenu Azure virtualnog računala da biste oslobodili resurse za druge vodiče i izbjegli povećavanja naknade za korištenje Azure.
