<properties
    pageTitle="Instalirajte Python i SDK - Azure"
    description="Saznajte kako instalirati Python i SDK za uporabu Azure."
    services=""
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="installing-python-and-the-sdk"></a>Prilikom instalacije Python i SDK-a

Python je lako postavljanje u sustavu Windows, a dolazi unaprijed instalirana na Mac i Linux [Tulum za Windows](https://msdn.microsoft.com/commandline/wsl/about). Ovaj vodič vodit će vas kroz instalaciju i Priprema računala za korištenje s Azure.

## <a name="whats-in-the-python-azure-sdk"></a>Što je na Python Azure SDK?

Azure SDK Python sadrži komponente koje vam omogućuju da razviti, uvođenje i upravljanje aplikacijama Python za Azure. Konkretno, Azure SDK Python obuhvaća sljedeće:

* **Upravljanje bibliotekama**. Ove biblioteke klasa navedite sučelje za upravljanje Azure resurse, kao što su virtualnim strojevima prostora za pohranu računi.

* **Izvođenje biblioteke**. Ove biblioteke klasa pružaju sučelja za pristup Azure značajke, kao što su bus prostora za pohranu i usluga.

## <a name="which-python-and-which-version-to-use"></a>Koje Python i koju verziju koristiti

Dostupno nekoliko flavors Python interpreters – Primjeri:

* CPython - Python tumačenja standardnih ili najčešće korištenih
* PyPy - brza, usklađen zamjenski implementacije CPython
* IronPython - tumačenja Python koja se izvršava na .net/CLR
* Jython - tumačenja Python koja se izvršava na Java virtualnog računala

**CPython** v2.7 ili v3.3 + PyPy 5.4.0 se testira i podržana za Python Azure SDK.

## <a name="where-to-get-python"></a>Gdje se Python?

Da biste dobili CPython na nekoliko načina:

* Izravno iz [www.python.org][]
* Iz ugledna distro kao što su [www.continuum.io][], [www.enthought.com][] ili [www.activestate.com][]
* Stvaranje izvora!

Osim ako imate specifičnim potrebama, preporučujemo da prve dvije mogućnosti.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>Instalacija SDK na Windows, Linux i Mac OS (samo za klijent biblioteke)

Ako već imate Python instaliran točaka možete koristiti da biste instalirali paket svih biblioteka za klijenta u postojeće Python 2.7 ili Python 3,3 + okruženje. To će preuzeti pakete iz [Python paketa indeksa][] (PyPI).

Morat ćete administratorskim ovlastima:

- Linux i Mac OS, poslužite se `sudo` naredba: `sudo pip install azure-mgmt-compute`.
- Windows: otvorite svoje PowerShell/naredbenog retka kao administrator

Možete instalirati pojedinačno Svaka biblioteka za svaki servis Azure:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Pregled paketa možete instalirati i pomoću na `--pre` zastavicom:

```console
   $ pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

Skup Azure biblioteke možete instalirati i pomoću jedan redak u `azure` meta paketa. Budući da sve pakete u meta-paket se objavljuju kao stabilan još, u `azure` meta paketa je i dalje u pretpregledu. Međutim, pakete core iz perspektive kvalitete/dovršenosti kod možete smatrati "stabilan" trenutno
- To će biti službeno označene kao sinkronizirana s drugim jezicima čim. Ne možemo ne planiranja na Dodatno glavne promjene tek onda.

Budući da je pretpregled izdanja, morate koristiti u `--pre` zastavicom:

```console
   $ pip install --pre azure
```
   
ili izravno

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Početak više paketa

[Indeks paketa Python][] (PyPI) ima obogaćenim odabira Python biblioteke.  Ako odaberete da biste instalirali na Distro, imat ćete već Većina zanimljive bita za različite scenarija iz web-razvoju tehničke računalstvo.


## <a name="python-tools-for-visual-studio"></a>Python alate za Visual Studio

[Python alate za Visual Studio][] (PTVS) je dodatak slobodnom/OSS od Microsofta, dodavanje veze za VANJSKIH postane full-fledged Python IDE:

![How-to-Instaliraj-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Pomoću PTVS nije obavezno, ali preporučuje se kao što je vam daje službe za podršku Python i Project rješenje Web ispravljanje pogrešaka, Profiliranje, interaktivne prozor, predloška i uređivanje bilježaka te Intellisense.

PTVS pojednostavljuje implementacija sustava Microsoft Azure s podrškom za implementaciju za [Servise u Oblaku][] i [web-mjesta][].

PTVS funkcionira s postojećeg Visual Studio 2013 ili instalacije 2015.  Dokumentacija, preuzimanja i rasprave, potražite u članku [Python alate za Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure scenariji za Linux i Mac OS

Za Linux ili Mac OS, glavni Azure scenariji podržani:

1. Dosta servisa Azure pomoću biblioteka klijenta za Python

2. Pokretanje aplikacije u Linux VM

3. Razvoj i objavljivanje web-mjesta Azure pomoću brojka

Prvi scenarij omogućuje stvaranje obogaćenih web aplikacije koje iskoristite prednosti upravljanja Azure PaaS kao što su [blobova][], [reda čekanja za pohranu][] [spremište tablica][] itd putem Pythonic wrappers za Azure REST API-ji. Ove radi jednak način u sustavu Windows, Mac i Linux.  Te biblioteke klijenta možete koristiti i na računalu lokalne razvoj ili Linux VM sustavom Azure.

Za scenarij VM jednostavno počnite Linux VM po izboru (Ubuntu, CentOS, Suse) i Pokreni/upravljanje što vam se sviđa.  Na primjer, možete pokrenuti ZAMIJENI bilježnice [IPython][] na računalu Windows i Mac i Linux i pokažite pregledniku Linux ili Windows višestruki procedura VM sustavom IPython modul Azure. Praktični vodič [IPython bilježnice na Azure][] dodatne informacije potražite u članku.

Informacije o načinu postavljanja Linux VM potražite vodič za [Stvaranje virtualnog računala radi Linux][] .

Pomoću brojka implementaciju, možete razviti Python web-aplikacije i objavljivanje web-mjesto programa Azure s operacijske sustave.  Prilikom automatske vaše spremište za Azure, automatski stvoriti okruženje virtualne i točaka instalirajte paketi za potrebne.

Dodatne informacije o razvoju i Azure web-mjesta za objavljivanje, potražite u članku vodiči za [Stvaranje web-mjesta s Django][], [Stvaranje web-mjesta s boca][]i [Stvaranje web-mjesta s Flask][]. Općenite informacije o korištenju bilo koje podržavaju WSGI framework potražite u članku [Konfiguriranje Python s Azure web-mjesta][].


## <a name="additional-software-and-resources"></a>Dodatni softver i resursima:

* [Azure SDK Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK Python Github](https://github.com/Azure/azure-sdk-for-python)
* [Službeni Azure uzorka za Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Kao analize Python raspodjele][]
* [Enthought Python raspodjele][]
* [ActiveState Python raspodjele][]
* [SciPy - paketa znanstvenom Python biblioteka][]
* [NumPy - biblioteci numeričke vrijednosti za Python][]
* [Django Project – na starijih web framework/a CMS][]
* [IPython - programa napredne ZAMIJENI/bilježnicu za Python][]
* [IPython bilježnice na Azure][]
* [Python alate za Visual Studio na GitHub][]
* [Razvojni centar za Python](/develop/python/)

[Kao analize Python raspodjele]: http://continuum.io
[Enthought Python raspodjele]: http://www.enthought.com
[ActiveState Python raspodjele]: http://www.activestate.com
[www.Python.org]: http://www.python.org
[www.continuum.IO]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.activestate.com]: http://www.activestate.com
[SciPy - paketa znanstvenom Python biblioteka]: http://www.scipy.org
[NumPy - biblioteci numeričke vrijednosti za Python]: http://www.numpy.org
[Django Project – na starijih web framework/a CMS]: http://www.djangoproject.com
[IPython - programa napredne ZAMIJENI/bilježnicu za Python]: http://ipython.org
[IPython]: http://ipython.org
[IPython bilježnice na Azure]: virtual-machines-linux-jupyter-notebook.md
[Servisi u oblaku]: cloud-services-python-ptvs.md
[Web-mjesta]: web-sites-python-ptvs-django-mysql.md
[Python alate za Visual Studio]: http://aka.ms/ptvs
[Python alate za Visual Studio na GitHub]: https://github.com/microsoft/ptvs
[Indeks Python paketa]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Stvaranje virtualnog računala koja se izvodi Linux]: virtual-machines-linux-quick-create-cli.md
[Stvaranje web-mjesta s Django]: web-sites-python-create-deploy-django-app.md
[Stvaranje web-mjesta s boce]: web-sites-python-create-deploy-bottle-app.md
[Stvaranje web-mjesta s Flask]: web-sites-python-create-deploy-flask-app.md
[Konfiguriranje Python s Azure web-mjesta]: web-sites-python-configure.md
[spremište tablica]: storage-python-how-to-use-table-storage.md
[red čekanja za pohranu]: storage-python-how-to-use-queue-storage.md
[spremište blobova platforme]: storage-python-how-to-use-blob-storage.md
