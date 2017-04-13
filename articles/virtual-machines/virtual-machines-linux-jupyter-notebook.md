<properties
    pageTitle="Stvaranje bilježnice Jupyter/IPython | Microsoft Azure"
    description="Saznajte kako implementirati Jupyter/IPython bilježnica na računalu virtualne Linux stvorene pomoću model implementacije upravitelj resursa u Azure."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="crwilcox"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="python"
    ms.topic="article"
    ms.date="11/10/2015"
    ms.author="crwilcox"/>

# <a name="jupyter-notebook-on-azure"></a>Jupyter bilježnice na Azure

[Jupyter projekt](http://jupyter.org), prethodno [IPython projekta](http://ipython.org)pruža skup alata za računalstvo znanstveni pomoću naprednih interaktivne ljuske kojima se kombiniraju izvršavanje koda stvaranjem uživo računalne dokumenta. Ove bilježnice datoteke mogu sadržavati proizvoljne teksta, matematičke formule, unos koda, rezultata, grafika, videozapise i druge vrste medijskih sadržaja Moderna web-preglednik podržava prikazivanja. Poznajete apsolutno Python i želite im Saznajte u okruženju zabavna, interaktivne ili nemojte učiniti neke ozbiljne paralelno/Tehnički računalstvo, bilježnicu Jupyter li sjajno odabir.

![Snimka zaslona](./media/virtual-machines-linux-jupyter-notebook/ipy-notebook-spectral.png) pomoću SciPy i Matplotlib paketa da biste analizirali strukturu zvučnog snimanje.


## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter dva načina: Azure bilježnica ili prilagođeni uvođenje
Azure omogućuje uslugu koje možete koristiti da biste [brzo početi koristiti Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Pomoću servisa Azure bilježnice možete jednostavno ostvariti pristup Jupyter na web-pristupačnost sučelja skalabilni računalne resursi power Python i mnogo biblioteka.  Nakon instalacije rukuje servis, korisnici mogu pristupati ove resurse bez potrebe za administraciju i konfiguracija korisnika.

Ako servis bilježnice odgovara vašem scenariju nastavite ovaj članak koji će vidjet ćete kako implementirati Jupyter bilježnice na Microsoft Azure pomoću Linux virtualnim strojevima (VMs).

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Stvaranje i konfiguriranje na VM na Azure

U prvi je korak da biste stvorili virtualnog računala (VM) sustavom Azure.
U ovom VM je dovršena operacijski sustav u oblaku i će se koristiti za izvođenje Jupyter bilježnice. Se može pokrenuti virtualnim strojevima Linux i Windows Azure, a ne možemo obrađuje postavljanje Jupyter na obje vrste virtualnim računalima.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Stvaranje Linux VM i otvorite priključak za Jupyter

Slijedite upute [u nastavku] navedeni[ portal-vm-linux] da biste stvorili virtualnog računala *Ubuntu* distribucije. Pomoću ovog praktičnog vodiča koristi Ubuntu Server 14.04 LTS. Ne možemo ćete pretpostavlja ime korisnika *azureuser*.

Nakon virtualnog računala uvodi moramo otvorite pravilo sigurnosti na mreži sigurnosne grupe.  Na portalu Azure idite na **Mreži sigurnosne grupe** i otvorite karticu za sigurnosnu grupu koja odgovara vašem VM. Najprije morate dodati pravilo dolazni sigurnost s sljedeće postavke: **TCP** protokol, **\*** za priključak izvora (javnih) i **9999** za priključak odredište (privatna).

![Snimka zaslona](./media/virtual-machines-linux-jupyter-notebook/azure-add-endpoint.png)

U svoje mreže sigurnosne grupe kliknite **Sučelje mreže** i zabilježite **Javnu IP adresu** kao bit će potrebno povezati svoje VM u sljedećem koraku.

## <a name="install-required-software-on-the-vm"></a>Kliknite pločicu potrebni softver na VM

Da biste pokrenuli Jupyter bilježnice na našem VM, ne možemo morate instalirati Jupyter i njezine ovisnosti. Povežite se s vašeg linux vm pomoću ssh i uparivanje korisničkog imena i lozinke koje ste odabrali kada ste stvorili u vm. U ovom ćete praktičnom vodiču smo će koristiti PuTTY i povezivanje sa sustavom Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Instalacija Jupyter na Ubuntu
Instalirajte Anaconda, distribucija python znanstvenog popularne podataka, koristite neku od veza koje se nalaze iz [Kao analize](https://www.continuum.io/downloads).  Od pisanje taj dokument u ispod veze su najviše do datuma verzije.

#### <a name="anaconda-installs-for-linux"></a>Instalira anaconda za Linux
<table>
  <th>Python 3.4</th>
  <th>Python 2.7</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64-bitni</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64-bitni</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32-bitni</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32-bitni</href>
    </td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a>Instaliranje Anaconda3 2.3.0 64-bitne na Ubuntu
Na primjer, to je kako instalirati Anaconda na Ubuntu

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Snimka zaslona](./media/virtual-machines-linux-jupyter-notebook/anaconda-install.png)


### <a name="configuring-jupyter-and-using-ssl"></a>Konfiguriranje Jupyter i putem SSL-a
Nakon instalacije moramo potrajati s postavljanjem konfiguracijske datoteke za Jupyter. Ako naiđete na troubles s konfiguracijom Jupyter može biti korisno da biste pogledali svaku [Jupyter dokumentaciji pokrenut na poslužitelju bilježnice](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).

Sljedeći smo `cd` direktorij profila naš SSL certifikat za stvaranje i uređivanje profila konfiguracijskoj datoteci.

Na Linux koristite sljedeću naredbu.

    cd ~/.jupyter

Da biste stvorili SSL certifikat (Linux i Windows), koristite sljedeću naredbu.

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Imajte na umu da jer smo stvarate samopotpisani SSL certifikata prilikom povezivanja s bilježnicu preglednika steći ćete sigurnosno upozorenje.  Za korištenje Dugoročne proizvodnje, želite koristite pravilno traje pridružena vašoj tvrtki ili ustanovi.  Budući da je upravljanje certifikatom obuhvaćeno Ovaj pokazni, ne možemo će bi s samopotpisani certifikat za sada.

Osim pomoću certifikata, morate unijeti i lozinku radi zaštite vaše bilježnice iz neovlaštenu upotrebu.  Sigurnosnih vam razloga Jupyter koristi šifriranu lozinku u njegov konfiguracijskoj datoteci morat ćete najprije šifriranje lozinku.  IPython nudi utility da biste to učinili; u naredbenom retku pokrenite sljedeću naredbu.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

To će vas za lozinku i potvrdu, a zatim ispišite lozinke. Imajte na umu sljedeće za sljedeći korak.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

Nakon toga će Uređivanje profila konfiguracijska datoteka, što je na `jupyter_notebook_config.py` datoteka u direktoriju u kojem se nalazite.  Napomena da datoteku možda ne postoji – samo ga stvoriti ako je to slučaj.  Datoteka sadrži više polja, a po zadanom sve su komentar.  Možete otvoriti datoteku s bilo kojeg uređivač teksta po svom ukusu, a potrebno je provjeriti ima najmanje sljedeći sadržaj. **Ne zaboravite da biste zamijenili c.NotebookApp.password u config s sha1 u prethodnom koraku**.

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a>Pokretanje bilježnice Jupyter

Sada ćemo spremni za početak Jupyter bilježnice. Da biste to učinili, pomaknite se do imenik želite pohraniti bilježnica u i pokrenuti poslužitelj Jupyter bilježnica pomoću sljedeće naredbe.

    /anaconda3/bin/jupyter-notebook

Sada trebali biste moći pristupiti bilježnici Jupyter na adresi `https://[PUBLIC-IP-ADDRESS]:9999`.

Kada prvi put pristupiti bilježnici, na stranicu za prijavu traži lozinku. A kad se prijavite, vidjet ćete "Jupyter bilježnice nadzornu ploču", što je centar za ontrolne za sve operacije bilježnice.  S ove stranice možete stvoriti nove bilježnice i Otvori postojeće.

![Snimka zaslona](./media/virtual-machines-linux-jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a>Korištenje Jupyter bilježnice

Ako kliknete gumb **Novo** , vidjet ćete sljedeće početne stranice.

![Snimka zaslona](./media/virtual-machines-linux-jupyter-notebook/jupyter-untitled-notebook.png)

Područja označena je `In []:` upit se u području za unos i Ovdje možete unijeti bilo koji valjani kod Python te će izvršiti kada kliknete `Shift-Enter` ili kliknite ikonu za "Reprodukciju" (desnu trokuta na alatnoj traci).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>Napredna paradigm: live računalne dokumenata s obogaćenim medijskim sadržajima

Bilježnice sam treba čini vam se vrlo prirodnim svakome tko ovo koristio Python i obradu teksta, jer se neke načine kombinacije oba: možete izvršiti blokova Šifra Python, ali možete i zadržati bilješke i drugog teksta promjenom stila ćelija s "Kod" i "Markdown" pomoću padajući izbornik na alatnoj traci.

Jupyter je mnogo veća od programu za obradu teksta kao što je omogućuje Miješanje izračunavanje i obogaćenim medijskim sadržajima (tekst, grafiku, videozapisa i gotovo sve Moderna web-pregledniku možete prikazati). Ne možete miješati, tekst, kod, videozapisi i više!

![Snimka zaslona](./media/virtual-machines-linux-jupyter-notebook/jupyter-editing-experience.png)

I s power Python na mnogo izvrstan biblioteke za znanstvene i tehničke računalstvo s, u sljedeće snimka osnovni izračuni može izvoditi isti jednostavno kao složene mreže analiza, sve u jednom okruženju.

U ovom paradigm od Miješanje power Moderna web-mjesta s uživo izračuni nudi brojne mogućnosti, a najbolje najprikladniji za oblak; Bilježnicu možete koristiti:

* Kao računalne brzi međuspremnik da biste snimili exploratory rad na problem.

* Da biste zajednički koristili rezultate sa suradnicima u 'uživo' računalne obrazac ili u ispisanoj kopiji oblici (HTML, PDF-a).

* Da biste distribucija i izlaganje uživo Poduka materijale koje obuhvaćaju izračunavanje, pa učenici odmah možete isprobati realni kod ga izmijeniti i ponovno izvođenje interaktivno.

* Možete unijeti "izvršni radova" koji dovode rezultate istraživanja na način na koji se mogu odmah reproducirati, nastavlja i proširiti drugim korisnicima.

* Kao platformu za suradnju računalstvo: više korisnika možete se prijaviti u istom poslužitelju bilježnice da biste zajednički koristili računalne sesiju uživo.


Ako se kod IPython izvora [spremište][], tražit će se cijelom direktoriju primjerima bilježnicu koju možete preuzeti i Poigrajte se s na vlastitu VM Jupyter Azure.  Jednostavno preuzeti na `.ipynb` datoteke na web-mjestu i prenesite ih na nadzornoj ploči bilježnice Azure VM (ili ih preuzeti izravno u na VM).

## <a name="conclusion"></a>Zaključak

Bilježnice Jupyter nudi Napredna sučelje za pristup interaktivno potenciju zajednici Python na Azure.  Pokriva širokog raspona korištenje slučajeva uključujući jednostavno Istraživanje i učenjem Python, analiza podataka i vizualizaciju, simulacije i paralelno računalstvo. Rezultat dokumenti bilježnice sadrže potpunog zapisa computations koji se izvode možete zajednički koristiti s drugim korisnicima Jupyter.  Jupyter bilježnice koje se mogu koristiti kao lokalnog računala, ali najbolje najprikladniji za oblak implementacije na Azure

Osnovne funkcije Jupyter dostupne i u Visual Studio putem [Python alate za Visual Studio][] (PTVS). PTVS se slobodno i Otvori izvornih dodatak Microsoft koji postane Visual Studio napredne Python razvoj okruženju koji uključuje Napredni uređivač pomoću značajke IntelliSense ispravljanje pogrešaka, profiliranja i paralelnih računalstvo integracije.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u [Centru za razvojne inženjere Python](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[spremište]: https://github.com/ipython/ipython
[Python alate za Visual Studio]: http://aka.ms/ptvs
