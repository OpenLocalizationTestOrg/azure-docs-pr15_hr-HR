<properties
    pageTitle="Kako koristiti dodatak Azure podređeni uz Jenkins neprekinuti integraciju sa | Microsoft Azure"
    description="U članku se opisuje kako koristiti dodatak Azure podređeni uz Jenkins neprekinuti integraciju sa."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="how-to-use-the-azure-slave-plug-in-with-jenkins-continuous-integration"></a>Kako pomoću dodatka Azure podređeni pomoću Jenkins neprekinuti Integracija

Azure podređeni dodatak možete koristiti za Jenkins za dodjeljivanje podređeni čvorovi na Azure prilikom pokretanja raspodijeljeno sastavlja.

## <a name="install-the-azure-slave-plug-in"></a>Instalacija dodatka Azure podređeni

1. Na nadzornoj ploči Jenkins kliknite **Upravljanje Jenkins**.

1. Na stranici **Upravljanje Jenkins** kliknite **Upravljanje dodacima**.

1. Kliknite karticu **dostupno** .

1. U polje za filtar na popisu Dostupni dodaci, upišite **Azure** da biste ograničili na popisu da biste odgovarajuće dodatke.

    Ako odlučite da biste se pomicali po popisu Dostupni dodaci, pronaći ćete Azure podređeni dodatak u odjeljku **Upravljanje klaster i distribuirati izraditi** .

1. Potvrdite okvir **Dodatak podređeni Azure** .

1. Kliknite **Instaliraj bez ponovnog pokretanja** ili **Odmah preuzmite i instalirajte nakon ponovnog pokretanja**.

Dodatak je instaliran na sljedeće korake su da biste konfigurirali dodatka profila Azure pretplate, a da biste stvorili predložak koji će se koristiti u stvaranju virtualnog računala za podređeni čvor.


## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Konfiguriranje Azure dodatak podređeni uz pretplatu profila

Profil pretplatu, koja se naziva i kako objaviti postavke je XML datoteku koja sadrži vjerodajnica za sigurno i nekih dodatnih informacija morate raditi s Azure u razvojno okruženje. Da biste konfigurirali Azure podređeni dodatak, potrebno je:

* Identifikacijskog broja za pretplatu
* Upravljanje certifikat za vašu pretplatu

Te se može pronaći u svom [profilu pretplate]. U nastavku je prikazan primjer profila pretplate.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID value>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Nakon što ste pretplatu profila, slijedite ove korake da biste konfigurirali Azure podređeni dodatak:

1. Na nadzornoj ploči Jenkins kliknite **Upravljanje Jenkins**.

1. Kliknite **Konfiguriranje sustava**.

1. Pomaknite se prema dolje po stranici upute za pronalaženje odjeljka **oblaka** .

1. Kliknite **Dodavanje novog oblaka > Microsoft Azure**.

    ![Oblak sekcije][cloud section]

    To će prikazati polja kojem trebate unijeti pojedinosti pretplate.

    ![Pretplata konfiguraciji][subscription configuration]

1. Kopirajte id pretplate i upravljanje certifikatom vrijednosti iz pretplate profila, pa ih zalijepite u odgovarajuća polja.

    Prilikom kopiranja pretplate certifikat id i upravljanje njima, nemojte uvrstiti ponude koje stavite vrijednosti.

1. Kliknite **Provjera konfiguracije**.

1. Kada konfiguraciju provjerava se čini ispravnom, kliknite **Spremi**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Postavljanje predloška virtualnog računala za Azure podređeni dodatka

Predložak virtualnog računala definira parametre dodatka pomoću kojega će stvoriti podređeni čvor na Azure. U sljedećim koracima ne možemo stvorit ćete predložak za programa Ubuntu virtualnog računala.

1. Na nadzornoj ploči Jenkins kliknite **Upravljanje Jenkins**.

1. Kliknite **Konfiguriranje sustava**.

1. Pomaknite se prema dolje po stranici upute za pronalaženje odjeljka **oblaka** .

1. U odjeljku **oblaka** traženje **Dodavanje predloška Azure virtualnog računala**, a zatim **Dodaj**.

    ![Dodavanje vm predloška][add vm template]

    Prikazat će polja koji morate unijeti detalje o predlošku koji stvarate.

    ![prazno Općenito konfiguracija][blank general configuration]

1. U okvir **naziv** unesite naziv za servis programa Azure oblaka. Ako ime koje ste unijeli upućuje na postojeće oblaku, virtualnog računala dodijelit će se Resursi na tom servisu. U suprotnom Azure će stvoriti novi.

1. U okvir **Opis** unesite tekst koji opisuje predlošku koji stvarate. Ovo je samo za zapise koja se koristi u dodjeljivanje virtualnog računala.

1. Okvir **oznake** koristi se za identificiranje predlošku koji stvarate i naknadno koristi referentni predloška prilikom stvaranja Jenkins posao. Za naše svrhu u taj okvir unesite **linux** .

1. Na popisu **regija** kliknite područje u kojem će se stvoriti virtualnog računala.

1. Na popisu **Veličina virtualnog računala** kliknite odgovarajuću veličinu.

1. U okvir **Naziv računa spremišta** Navedite račun za pohranu u kojem će se stvoriti virtualnog računala. Provjerite je li u području isti kao servisa u oblaku hoćete li koristiti. Ako želite da se novi prostor za pohranu će biti stvoren, taj okvir možete ostavite prazno.

1. Koliko dugo određuje broj minuta prije Jenkins briše neaktivnosti podređeni. Ostavite na zadanu vrijednost od 60. Možete odabrati i da biste isključili podređeni umjesto brisanja kada je neaktivno. Da biste to učinili, odaberite potvrdni okvir **Samo za isključivanje (nemojte brisati) nakon zadržavanja vrijeme** .

1. Na popisu **Korištenje** kliknite odgovarajući uvjet kada koristit će se podređeni čvor. Zasad, kliknite **Utilize čvor najveću moguću**.

    U ovom trenutku obrazac trebao bi izgledati Pomalo slično sljedećemu:

    ![Kontrolna točka Općenito predložak config][checkpoint general template config]

    Sljedeći je korak možete unijeti detalje o operacijskom sustavu sliku koju želite da se u vašem podređeni će biti stvoren u.

1. U okviru **obitelji slike ili ID-a** morate odrediti koje slika sustava instalirat će se na virtualnog računala. Možete popisu obitelji slika odaberite ili navedite prilagođenu sliku.

    Ako želite da biste odabrali s popisa slika linije, unesite prvog znaka (velika i mala slova) Prezime slike. Ako, primjerice, pisati **U** će se srušiti popis linije Ubuntu poslužitelja. Kada odaberete s popisa, Jenkins će koristiti najnoviju verziju te slika sustava iz tog obitelji prilikom dodjele resursa virtualnog računala.

    ![Ogledni popis OS slike][OS Image list sample]

    Ako imate prilagođenu sliku koju želite koristiti umjesto toga, unesite naziv te prilagođenu sliku. Prilagođenu sliku nazivi ne prikazuju se na popis, stoga ćete morati provjerite je li ispravno unijeli naziv.

    Za ovaj vodič upišite **U** da biste otvorili popis Ubuntu slike, a zatim **Ubuntu Server 14.04 LTS**.

1. Na popisu **Pokretanje način** kliknite **SSH**.

1. Kopirajte skripte u nastavku i zalijepite ga u okvir **Init skripta** .

        # Install Java

        sudo apt-get -y update

        sudo apt-get install -y openjdk-7-jdk

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y openjdk-7-jdk

        # Install git

        sudo apt-get install -y git

        #Install ant

        sudo apt-get install -y ant

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y ant

    Skripta init će se izvršavati nakon stvaranja virtualnog računala. U ovom primjeru skriptu instalira Java i brojka ant.

1. U okvire **korisničko ime** i **lozinku** , unesite željene vrijednosti za administratorski račun koji će se stvoriti na virtualnog računala.

1. Kliknite **Provjera predložak** da biste provjerili parametre koje ste naveli vrijede.

1. Kliknite **Spremi**.


## <a name="create-a-jenkins-job-that-runs-on-a-slave-node-on-azure"></a>Stvaranje zadatka Jenkins koja se izvršava na podređeni čvor na Azure

U ovom ćete odjeljku stvorit ćete Jenkins zadatak koji će se izvoditi na podređeni čvor na Azure. Morate imati vlastitu projekta prema gore na GitHub za praćenje.

1. Na nadzornoj ploči Jenkins kliknite **Nova stavka**.

1. Unesite naziv za zadatke koje stvarate.

1. Vrsta projekta kliknite **slobodna improvizacija projekta**.

1. Kliknite **u redu**.

1. Na stranici Konfiguracija zadatka, odaberite **Ograničavanje gdje se može pokrenuti taj projekt**.

1. U okvir **Natpis izraza** unesite **linux**. U prethodnom odjeljku koju smo stvorili podređeni predložak da ne možemo pod nazivom **linux**koji je što smo se određuje ovdje.

1. U odjeljku **Sastavljanje** kliknite **Dodaj sastavljanje korak** i odaberite **ljuske za izvršavanje**.

1. Uređivanje sljedeću skriptu **(GitHub naziv računa)**, **(vaše ime projekt)**i **(projekta direktorija)** zamjenjuje odgovarajuće vrijednosti i zalijepite uređivati skripte u područje teksta koji će se prikazati.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your GitHub account name)/(your project name).git

        fi

        # change directory to project

        cd $currentDir/(your project directory)

        #Execute build task

        ant

1. Kliknite **Spremi**.

1. Na nadzornoj ploči Jenkins pokazivač miša postavite iznad zadatka koji ste upravo stvorili, a zatim kliknite strelicu prema dolje da biste prikazali mogućnosti zadatka.

1. Kliknite **Stvaranje sada**.

Jenkins će čvor podređeni pomoću predloška stvorili u prethodnom odjeljku i izvoditi skripte koje ste naveli u koraku Sastavi za taj zadatak.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java].

<!-- URL List -->

[Razvojni centar za Azure Java]: https://azure.microsoft.com/develop/java/
[profil pretplate]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[cloud section]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-cloud-section.png
[subscription configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png
[blank general configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png
[checkpoint general template config]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png
[OS Image list sample]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png