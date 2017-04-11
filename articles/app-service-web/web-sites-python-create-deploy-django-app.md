<properties
    pageTitle="Stvaranje web-aplikacije pomoću Django servisu Azure"
    description="Praktični vodič koji predstavljaju izvodi web-aplikacijama Python u Azure aplikacije servisa web-aplikacijama."
    services="app-service\web"
    documentationCenter="python"
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article" 
    ms.date="02/19/2016"
    ms.author="huvalo"/>


# <a name="creating-web-apps-with-django-in-azure"></a>Stvaranje web-aplikacije pomoću Django servisu Azure

Pomoću ovog praktičnog vodiča opisuje način za početak rada pokrenut Python [Azure servisa Web](http://go.microsoft.com/fwlink/?LinkId=529714)aplikacija. Web Apps nudi ograničeni besplatan hosting i brzog uvođenja i koristite Python! Kao što je rastom aplikacije koje možete prijeći u plaćenu hosting, a možete integrirati i sa svim ostalim servisima Azure.

Stvorite aplikacije korištenjem Django web framework (pogledajte zamjenske verzije ovog praktičnog vodiča za [Flask](web-sites-python-create-deploy-flask-app.md) i [boca](web-sites-python-create-deploy-bottle-app.md)). Će stvoriti web-aplikaciju iz trgovine Windows Azure, postavite brojka implementacije i Kloniraj lokalno spremište. Zatim će pokrenuti aplikaciju lokalno, unesite promjene, izvršavanje i automatske ih Azure. Vodič prikazuje kako to učiniti sa sustavom Windows ili Mac i Linux.

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.


## <a name="prerequisites"></a>Preduvjeti

- Windows, Mac i Linux
- Python 2.7 ili 3.4
- setuptools točaka, virtualenv (samo za Python 2.7)
- Brojka
- [Python alate za Visual Studio][] (PTVS) - Imajte na umu: Ovo nije obavezno

**Napomena**: objavljivanje TFS trenutno nije podržano za projekte Python.

### <a name="windows"></a>Windows

Ako već nemate Python 2.7 ili 3.4 instaliranih (32-bitni), preporučujemo da instalirate [Azure SDK Python 2.7] ili [Azure SDK Python 3.4] pomoću Installer platformu za Web. Time se instalira 32-bitnu verziju sustava Python, setuptools, točaka, virtualenv itd (32-bitni Python je što je instalirano na Azure glavnog računala). Osim toga, možete dobiti Python od [python.org].

Brojka, preporučujemo [Brojka za Windows] ili [GitHub za Windows]. Ako koristite Visual Studio, možete koristiti integrirani brojka podrška.

Preporučujemo i instalacije [Python 2.2 Alati za Visual Studio]. Ovo nije obavezno, ali ako imate [Visual Studio], uključujući besplatne Visual Studio zajednice 2013 ili Visual Studio Express 2013 za web-mjesto, zatim imat ćete sjajno Python IDE.

### <a name="maclinux"></a>Mac i Linux

Trebali biste imati Python i brojka već instaliran, no pripazite da imate Python 2.7 ili 3.4.


## <a name="web-app-creation-on-portal"></a>Web App stvaranje portala

Prvi korak pri stvaranju aplikacije je da biste stvorili web-aplikaciju putem [Portala za Azure](https://portal.azure.com).

1. Prijavite se na Azure Portal, a zatim kliknite gumb **NOVO** u donjem lijevom kutu.
3. U okvir za pretraživanje upišite "python".
4. U rezultatima pretraživanja odaberite **Django** (objavljuje PTVS), a zatim kliknite **Stvori**.
5. Konfiguriranje nove aplikacije Django, kao što su stvaranje nove aplikacije servisa za planiranje i novu grupu resursa za njega. Nakon toga kliknite **Stvori**.
6. Konfiguriranje brojka objavljivanjem za novostvorenom web-aplikaciju programa slijedeći upute u [Lokalnoj implementaciji brojka za aplikacije servisa za Azure](app-service-deploy-local-git.md).

## <a name="application-overview"></a>Pregled aplikacija

### <a name="git-repository-contents"></a>Brojka spremište sadržaja

Slijedi pregled datoteka pronaći ćete u spremištu za početne brojka koje ćemo ćete Kloniraj u sljedećem odjeljku.

    \app\__init__.py
    \app\forms.py
    \app\models.py
    \app\tests.py
    \app\views.py
    \app\static\content\
    \app\static\fonts\
    \app\static\scripts\
    \app\templates\about.html
    \app\templates\contact.html
    \app\templates\index.html
    \app\templates\layout.html
    \app\templates\login.html
    \app\templates\loginpartial.html
    \DjangoWebProject\__init__.py
    \DjangoWebProject\settings.py
    \DjangoWebProject\urls.py
    \DjangoWebProject\wsgi.py

Glavni izvori za aplikaciju. Sastoji se od 3 stranice (indeks o kontaktu) s izgled matrice. Statički sadržaj i skripte obuhvaćaju samopokretanja programa, jquery, modernizr i odgovor.

    \manage.py

Lokalni upravljanje i podrška za razvoj poslužitelja. Ta postavka omogućuje lokalno pokrenuti aplikaciju, sinkronizirati bazu podataka, itd.

    \db.sqlite3

Zadanu bazu podataka. Sadrži potrebne tablice za aplikaciju da biste pokrenuli, ali sadrže sve korisnike (sinkronizirati bazu podataka da biste stvorili korisnika).

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

Datoteke projekta za korištenje alata za [Python za Visual Studio].

    \ptvs_virtualenv_proxy.py

Proxy IIS virtualne okruženjima i PTVS udaljene podrška za ispravljanje pogrešaka.

    \requirements.txt

Vanjski paketa potrebne za ovu aplikaciju. Implementacijsku skriptu će pip instalacija pakete na popisu u ovoj datoteci.

    \web.2.7.config
    \web.3.4.config

IIS konfiguracijske datoteke. Implementacijsku skriptu će koristiti odgovarajući web.x.y.config i kopirati kao web.config.

### <a name="optional-files---customizing-deployment"></a>Neobavezni datoteke - prilagodbu implementacije

[AZURE.INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Neobavezni datoteke - Python izvođenja

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>Dodatne datoteke na poslužitelju

Neke datoteke postoji na poslužitelju, ali se dodaju u spremište brojka. Ovo je stvorio implementacijsku skriptu.

    \web.config

Konfiguracijska datoteka IIS. Stvorene iz web.x.y.config u svakoj implementaciji.

    \env\

Python okruženje virtualne. Stvara se tijekom implementacije ako kompatibilne okruženje virtualne još ne postoji na web-aplikaciji. Paketi naveden u requirements.txt su točaka instaliran, ali točaka preskočiti instalacije ako već su instalirane pakete.

Sljedeća 3 odjeljcima objašnjeno kako nastaviti s web-aplikacije razvoju pod različitim okruženjima 3:

- Windows, pomoću alata za Python za Visual Studio
- Windows i naredbenog retka
- Mac i Linux s naredbenog retka


## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Razvoj aplikacija za web - Windows – Python Tools za Visual Studio

### <a name="clone-the-repository"></a>Kloniraj spremište

Najprije Kloniraj spremište pomoću portala za Azure navedeni URL. Dodatne informacije potražite u članku [Lokalne implementacije brojka aplikacije servisa za Azure](app-service-deploy-local-git.md).

Otvorite datoteku rješenja (.sln) koji se isporučuje u korijenskom direktoriju spremište.

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a>Stvaranje virtualne okruženje

Sada ćemo stvoriti okruženje virtualne za lokalni razvoj. Desnom tipkom miša kliknite **Python okruženja** odaberite **Dodavanje virtualne okruženje...**.

- Provjerite je li naziv okruženje `env`.

- Odaberite osnovni tumačenja. Provjerite je li koristiti istu verziju Python koji je odabran za web-aplikacije (u runtime.txt ili plohu **Postavke aplikacije** web-aplikacije na portalu za Azure).

- Provjerite je li uključena mogućnost da biste preuzeli i instalirali paketa.

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

Kliknite **Stvori**. To će stvoriti okruženje virtualne i instalirajte ovisnosti requirements.txt na popisu.

### <a name="create-a-superuser"></a>Stvaranje na superkorisnik

Baze podataka u sklopu aplikacije ne sadrži sve superkorisnik definirani. Da biste koristili funkciju prijava u aplikaciju ili administrator sučelja Django (Ako odlučite da ga omogućite), morat ćete stvoriti na superkorisnik.

Pokrenite to iz naredbenog retka iz mape projekta:

    env\scripts\python manage.py createsuperuser

Slijedite upute da biste postavili korisničko ime, lozinku, itd.

### <a name="run-using-development-server"></a>Pokretanje pomoću poslužitelja za razvoj

Pritisnite F5 da biste pokrenuli ispravljanje pogrešaka i web-pregledniku otvorit će automatski stranice koji se izvodi lokalno.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

Možete postaviti prekidne točke u izvora, koristite nadzorni windows itd. [Python alate za Visual Studio dokumentaciju] za dodatne informacije potražite na razne značajke.

### <a name="make-changes"></a>Unesite promjene

Sada možete isprobati promjenom aplikacije izvora i/ili predlošci.

Nakon što ste testirali promjene, izvršavanje ih u spremište brojka:

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a>Instaliranje više paketa

Program možda ovisnosti izvan Python i Django.

Možete instalirati dodatne paketa pomoću točaka. Da biste instalirali paket, desnom tipkom miša kliknite okruženje virtualne i odaberite **Instalacija Python paketa**.

Na primjer, da biste instalirali Azure SDK za Python, koji omogućuje pristup Azure prostora za pohranu, bus servisa i drugih servisa za Azure, unesite `azure`:

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

Desnom tipkom miša kliknite virtualne okruženja, a zatim odaberite **Generiraj requirements.txt** da biste ažurirali requirements.txt.

Nakon toga pohranite promjene requirements.txt brojka spremištu.

### <a name="deploy-to-azure"></a>Implementacija Azure

Da biste pokrenuli implementacije, kliknite **Sinkroniziraj** ili **Proslijedi**. Sinkroniziranje ne na automatske i na povlačite.

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

Prve implementacije će potrajati neko vrijeme, kao što je stvorit će okruženje virtualne, instalaciju paketa itd.

Visual Studio ne prikazuje napredak implementacije. Ako želite da pregledate rezultat, u odjeljku [Otklanjanje poteškoća – implementacije](#troubleshooting-deployment).

Pronađite Azure URL-a da biste vidjeli promjene.


## <a name="web-app-development---windows---command-line"></a>Web-aplikacije razvoj – Windows – naredbenog retka

### <a name="clone-the-repository"></a>Kloniraj spremište

Najprije Kloniraj spremište pomoću URL-a na portalu Azure i dodajte Azure spremište kao u alat za analizu daljinske. Dodatne informacije potražite u članku [Lokalne implementacije brojka aplikacije servisa za Azure](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Stvaranje virtualne okruženje

Ne možemo ćete stvoriti novo okruženje virtualne svrhe razvoj (ne ga dodati u spremištu). Virtualna okruženja u Python nisu relocatable, tako da svaki za razvojne inženjere radi na aplikaciju će stvoriti vlastite lokalno.

Provjerite je li koristiti istu verziju Python koji je odabran za web-aplikacije (u runtime.txt ili plohu postavke aplikacije web-aplikacije na portalu za Azure).

Za Python 2.7:

    c:\python27\python.exe -m virtualenv env

Za Python 3.4:

    c:\python34\python.exe -m venv env

Instalacija svih vanjskih paketa potrebnih aplikacije. Datoteka requirements.txt korijenu spremište možete koristiti da biste instalirali pakete u okruženju sustava virtualne:

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a>Stvaranje na superkorisnik

Baze podataka u sklopu aplikacije ne sadrži sve superkorisnik definirani. Da biste koristili funkciju prijava u aplikaciju ili administrator sučelja Django (Ako odlučite da ga omogućite), morat ćete stvoriti na superkorisnik.

Pokrenite to iz naredbenog retka iz mape projekta:

    env\scripts\python manage.py createsuperuser

Slijedite upute da biste postavili korisničko ime, lozinku, itd.

### <a name="run-using-development-server"></a>Pokretanje pomoću poslužitelja za razvoj

Možete pokrenuti aplikaciju u odjeljku poslužitelj za razvoj pomoću sljedeće naredbe:

    env\scripts\python manage.py runserver

Na konzoli prikazat će se URL-a i priključak poslužitelja očekuje podatke za:

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

Zatim otvorite web-preglednik te URL-a.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a>Unesite promjene

Sada možete isprobati promjenom aplikacije izvora i/ili predlošci.

Nakon što ste testirali promjene, izvršavanje ih u spremište brojka:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Instaliranje više paketa

Program možda ovisnosti izvan Python i Django.

Možete instalirati dodatne paketa pomoću točaka. Na primjer, da biste instalirali Azure SDK za Python, koji omogućuje pristup Azure prostora za pohranu, bus servisa i drugih servisa za Azure, upišite:

    env\scripts\pip install azure

Provjerite je li ažurirati requirements.txt:

    env\scripts\pip freeze > requirements.txt

Zapiši promjene:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Implementacija Azure

Da biste pokrenuli implementacije, automatske promjene za Azure:

    git push azure master

Prikazat će se rezultat implementacijsku skriptu, uključujući okruženje virtualne stvaranja, instalaciju paketa, stvaranje web.config.

Pronađite Azure URL-a da biste vidjeli promjene.


## <a name="web-app-development---maclinux---command-line"></a>Web-aplikacije razvoj – Mac i Linux - naredbenog retka

### <a name="clone-the-repository"></a>Kloniraj spremište

Najprije Kloniraj spremište pomoću URL-a na portalu Azure i dodajte Azure spremište kao u alat za analizu daljinske. Dodatne informacije potražite u članku [Lokalne implementacije brojka aplikacije servisa za Azure](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Stvaranje virtualne okruženje

Ne možemo ćete stvoriti novo okruženje virtualne svrhe razvoj (ne ga dodati u spremištu). Virtualna okruženja u Python nisu relocatable, tako da svaki za razvojne inženjere radi na aplikaciju će stvoriti vlastite lokalno.

Provjerite je li koristiti istu verziju Python koji je odabran za web-aplikacije (u runtime.txt ili plohu postavke aplikacije web-aplikacije na portalu za Azure).

Za Python 2.7:

    python -m virtualenv env

Za Python 3.4:

    python -m venv env

ili

    pyvenv env

Instalacija svih vanjskih paketa potrebnih aplikacije. Datoteka requirements.txt korijenu spremište možete koristiti da biste instalirali pakete u okruženju sustava virtualne:

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a>Stvaranje na superkorisnik

Baze podataka u sklopu aplikacije ne sadrži sve superkorisnik definirani. Da biste koristili funkciju prijava u aplikaciju ili administrator sučelja Django (Ako odlučite da ga omogućite), morat ćete stvoriti na superkorisnik.

Pokrenite to iz naredbenog retka iz mape projekta:

    env/bin/python manage.py createsuperuser

Slijedite upute da biste postavili korisničko ime, lozinku, itd.

### <a name="run-using-development-server"></a>Pokretanje pomoću poslužitelja za razvoj

Možete pokrenuti aplikaciju u odjeljku poslužitelj za razvoj pomoću sljedeće naredbe:

    env/bin/python manage.py runserver

Na konzoli prikazat će se URL-a i priključak poslužitelja očekuje podatke za:

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

Zatim otvorite web-preglednik te URL-a.

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a>Unesite promjene

Sada možete isprobati promjenom aplikacije izvora i/ili predlošci.

Nakon što ste testirali promjene, izvršavanje ih u spremište brojka:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Instaliranje više paketa

Program možda ovisnosti izvan Python i Django.

Možete instalirati dodatne paketa pomoću točaka. Na primjer, da biste instalirali Azure SDK za Python, koji omogućuje pristup Azure prostora za pohranu, bus servisa i drugih servisa za Azure, upišite:

    env/bin/pip install azure

Provjerite je li ažurirati requirements.txt:

    env/bin/pip freeze > requirements.txt

Zapiši promjene:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Implementacija Azure

Da biste pokrenuli implementacije, automatske promjene za Azure:

    git push azure master

Prikazat će se rezultat implementacijsku skriptu, uključujući okruženje virtualne stvaranja, instalaciju paketa, stvaranje web.config.

Pronađite Azure URL-a da biste vidjeli promjene.


## <a name="troubleshooting---package-installation"></a>Otklanjanje poteškoća – instalaciju paketa

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Otklanjanje poteškoća – virtualne okruženje

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]


## <a name="troubleshooting---static-files"></a>Otklanjanje poteškoća – statičke datoteke

Django ima pojam prikupljanje statičke datoteke. Otvara sve u statični datoteke na izvorno mjesto i kopira ih u jednu mapu. Za ovu aplikaciju se kopiraju u `/static`.

To možete učiniti jer statičke datoteke mogu potjecati iz različitih Django "aplikacije". Ako, na primjer, statičke datoteke iz Django sučelja za administratore nalaze se u podmapu biblioteke Django okruženje virtualne. Statičke datoteke definira ovu aplikaciju nalaze se u `/app/static`. Prilikom korištenja više Django 'aplikacije', imat ćete statične datoteke koje se nalaze na više mjesta.

Kada je pokrenut program u načinu rada za ispravljanje pogrešaka, aplikacija služi statičke datoteke na izvorno mjesto.

Kada je pokrenut program u načinu izdanje, aplikacija ne **ne** služi statičke datoteke. To je odgovornosti web-poslužitelj da bi služio datoteke. Za ovu aplikaciju IIS će vam poslužiti statičke datoteke iz `/static`.

Skup statičke datoteke obavlja automatski kao dio implementacijsku skriptu poništavanjem prethodno prikupljene datoteke. To znači da zbirke pojavljuje se u svakoj implementaciji malo, usporava implementaciju, ali osigurava suvišne datoteke neće biti dostupan izbjegavanje potencijalne sigurnošću.

Ako želite preskočiti skup statičke datoteke Django aplikacije:

    \.skipDjango

Zatim morat ćete učiniti zbirke ručno na lokalnom računalu:

    env\scripts\python manage.py collectstatic

Zatim uklonite na `\static` mapu iz `.gitignore` i dodajte ga u spremištu brojka.


## <a name="troubleshooting---settings"></a>Otklanjanje poteškoća – postavke

Moguće je promijeniti različite postavke za aplikaciju u `DjangoWebProject/settings.py`.

Pogodnost za razvojne inženjere omogućena je način za ispravljanje pogrešaka. Jedan gumb strani efekt koji je ćete moći vidjeti slike i drugi statički sadržaj kada se izvodi lokalno, bez potrebe za prikupljanje statičke datoteke.

Da biste onemogućili način rada za ispravljanje pogrešaka:

    DEBUG = False

Kada je onemogućen ispravljanje pogrešaka, vrijednost za `ALLOWED_HOSTS` mora biti ažurirani u Azure naziv glavnog računala. Ako, na primjer:

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

ili da biste omogućili neki:

    ALLOWED_HOSTS = (
        '*',
    )

U praksi, trebali biste učiniti nešto složenije baviti prebacivanje između ispravljanje pogrešaka i pustite načinu i početak naziv glavnog računala.

Varijable okruženja putem portala sustava Azure **KONFIGURIRAJ** stranice, možete postaviti u odjeljku **Postavke aplikacije** .  To može biti korisno za postavljanje vrijednosti koje možda želite prikazati u izvora (nizove veze, lozinku itd.), ili za koje želite postaviti drugačije između Azure i na lokalno računalo. U `settings.py`, za upite varijable okruženje pomoću `os.getenv`.


## <a name="using-a-database"></a>Korištenje baze podataka

Baza podataka koju ste dobili s aplikacijom je sqlite baze podataka. Ovo je praktičan i korisni zadanu bazu podataka za razvoj kao što je potrebna je gotovo bez postavljanja. Baza podataka pohranjena u db.sqlite3 datoteku u mapu projekta.

Azure pruža uslugu baze podataka koji su jednostavno je za korištenje iz aplikacije Django. Vodiči za korištenje [Baze podataka SQL] i [MySQL] iz aplikacije Django prikaz korake potrebne da biste stvorili servis baze podataka, promjena postavki baze podataka u `DjangoWebProject/settings.py`, i u bibliotekama potrebne za instalaciju.

Naravno, ako biste radije da biste upravljali poslužitelja baze podataka, možete učiniti pomoću sustava Windows ili Linux virtualnim strojevima sustavom Azure.


## <a name="django-admin-interface"></a>Django sučelja za administratore

Kada pokrenete stvaranje vaše modela, ćete htjeti popunjavanje baze podataka pomoću neke podatke. Jednostavan način za dodavanje i uređivanje sadržaja interaktivno je koristiti administratorskom sučelju Django.

Kod za administratore sučelje je komentar izvorima aplikacije, ali je jasno označena tako da možete lako je omogućiti (traženje "administrator").

Kada je omogućen, sinkronizirati bazu podataka, pokrenite aplikaciju i idite na `/admin`.


## <a name="next-steps"></a>Daljnji koraci

Slijedite ove veze da biste saznali više o Django i Python Alati za Visual Studio:

- [Dokumentacija Django]
- [Alati za Python dokumentacije Visual Studio]

Informacije o korištenju baze podataka SQL i MySQL:

- [Django i MySQL na Azure pomoću alata za Python za Visual Studio]
- [Django i baze podataka SQL Azure pomoću alata za Python za Visual Studio]

Dodatne informacije potražite u [Centru za razvojne inženjere Python](/develop/python/).


## <a name="whats-changed"></a>Što se promijenilo
* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)


<!--Link references-->
[Django i MySQL na Azure pomoću alata za Python za Visual Studio]: web-sites-python-ptvs-django-mysql.md
[Django i baze podataka SQL Azure pomoću alata za Python za Visual Studio]: web-sites-python-ptvs-django-sql.md
[SQL baze podataka]: web-sites-python-ptvs-django-sql.md
[MySQL]: web-sites-python-ptvs-django-mysql.md

<!--External Link references-->
[Azure SDK Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[Python.org]: http://www.python.org/
[Brojka za Windows]: http://msysgit.github.io/
[GitHub za Windows]: https://windows.github.com/
[Python alate za Visual Studio]: http://aka.ms/ptvs
[Python 2.2 Tools za Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Alati za Python dokumentacije Visual Studio]: http://aka.ms/ptvsdocs
[Dokumentacija Django]: https://www.djangoproject.com/
