<properties
    pageTitle="Kako koristiti dodatak Azure podređeni uz Hudson neprekinuti integraciju sa | Microsoft Azure"
    description="U članku se opisuje kako koristiti dodatak Azure podređeni uz Hudson neprekinuti integraciju sa."
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

# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Kako pomoću dodatka Azure podređeni pomoću Hudson neprekinuti Integracija

Dodatak za Hudson Azure podređeni omogućuje Dodjela podređeni čvorovi na Azure prilikom pokretanja raspodijeljeno sastavlja.

## <a name="install-the-azure-slave-plug-in"></a>Instalacija dodatka podređeni Azure

1. Na nadzornoj ploči Hudson kliknite **Upravljanje Hudson**.

1. Na stranici **Upravljanje Hudson** kliknite **Upravljanje dodacima**.

1. Kliknite karticu **dostupno** .

1. Kliknite **pretraživanje** i upišite **Azure** da biste ograničili na popisu da biste odgovarajuće dodatke.

    Ako odlučite da biste se pomicali po popisu Dostupni dodaci, pronaći ćete Azure podređeni dodatak u odjeljku **Upravljanje klaster i distribuirati izraditi** na kartici **drugim korisnicima** .

1. Potvrdite okvir za **Azure podređeni dodatak**.

1. Kliknite **Instaliraj**.

1. Ponovno pokrenite Hudson.

Sad kad dodatak je instaliran na sljedeće korake bio bi da biste konfigurirali dodatka profila Azure pretplate, a da biste stvorili predložak koji će se koristiti u stvaranju VM za podređeni čvor.

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Konfiguriranje podređeni Azure dodatka uz pretplatu profila

Profil pretplatu, koja se naziva i kako objaviti postavke je XML datoteku koja sadrži vjerodajnica za sigurno i nekih dodatnih informacija morate raditi s Azure u razvojno okruženje. Da biste konfigurirali Azure podređeni dodatak, potrebno je:

* Identifikacijskog broja za pretplatu
* Upravljanje certifikat za vašu pretplatu

Te se može pronaći u svom [profilu pretplate]. U nastavku je prikazan primjer profila pretplate.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Nakon što dodate pretplate profila, slijedite ove korake da biste konfigurirali Azure podređeni dodatka.

1. Na nadzornoj ploči Hudson kliknite **Upravljanje Hudson**.

1. Kliknite **Konfiguriranje sustava**.

1. Pomaknite se prema dolje po stranici upute za pronalaženje odjeljka **oblaka** .

1. Kliknite **Dodavanje novog oblaka > Microsoft Azure**.

    ![Dodavanje novog oblaka][add new cloud]

    To će prikazati polja kojem trebate unijeti pojedinosti pretplate.

    ![Konfiguriranje profila][configure profile]

1. Kopirajte pretplate id i upravljanje certifikat iz profila pretplate, pa ih zalijepite u odgovarajuća polja.

    Prilikom kopiranja pretplate certifikat id i upravljanje njima, **ne** obuhvaćaju ponude koje stavite vrijednosti.

1. Kliknite **Provjera konfiguracije**.

1. Kad se konfiguraciju potvrdi uspješno, kliknite **Spremi**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Postavljanje predloška virtualnog računala za Azure podređeni dodatka

Predložak virtualnog računala definira parametre dodatka će koristiti da biste stvorili podređeni čvor na Azure. U sljedećim koracima ne možemo ćete stvoriti predložak za programa VM Ubuntu.

1. Na nadzornoj ploči Hudson kliknite **Upravljanje Hudson**.

1. Kliknite **Konfiguriranje sustava**.

1. Pomaknite se prema dolje po stranici upute za pronalaženje odjeljka **oblaka** .

1. U odjeljku **oblaka** pronađite **Dodavanje predloška Azure virtualnog računala** i kliknite gumb **Dodaj** .

    ![Dodavanje vm predloška][add vm template]

1. Navedite naziv oblaka servisa u polje **naziv** . Ako naziv navedete upućuje na postojeće oblaku, u VM dodijelit će se Resursi na tom servisu. U suprotnom Azure će stvoriti novi.

1. U polje **Opis** unesite tekst koji opisuje predlošku koji stvarate. Ove informacije vrijedi samo za documentary svrhama, i ne koristi u dodjele resursa u VM.

1. U polju **naljepnice** , unesite **linux**. Ova etiketa služi za identifikaciju predloška stvarate i naknadno koristi referentni predloška prilikom stvaranja Hudson posao.

1. Odaberite područje u kojem će se stvoriti u VM.

1. Odaberite odgovarajuću veličinu VM.

1. Odredite gdje se VM stvorit će se račun za pohranu. Provjerite je li u području isti kao servisa u oblaku hoćete li koristiti. Ako želite da se novi prostor za pohranu će biti stvoren, to polje možete ostavite prazno.

1. Koliko dugo određuje broj minuta prije Hudson briše neaktivno podređeni. Ostavite na zadanu vrijednost od 60.

1. U odjeljku **Korištenje**, odaberite odgovarajući uvjet kada koristit će se podređeni čvor. Zasad, odaberite **Utilize čvor najveću moguću**.

    U ovom trenutku obrazac izgledati Pomalo slično sljedećemu:

    ![Konfiguracija predloška][template config]

1. U **obitelji slike ili ID-ja** imate odredite koje slika sustava će biti instalirana na vaš VM. Možete popisu obitelji slika odaberite ili navedite prilagođenu sliku.

    Ako želite da biste odabrali s popisa slika linije, unesite prvog znaka (velika i mala slova) Prezime slike. Ako, primjerice, pisati **U** će se srušiti popis linije Ubuntu poslužitelja. Kada odaberete s popisa, Jenkins će koristiti najnoviju verziju te slika sustava iz tog obitelji prilikom dodjele resursa sustava VM.

    ![Obiteljski popis OS][OS family list]

    Ako imate prilagođenu sliku koju želite koristiti umjesto toga, unesite naziv te prilagođenu sliku. Prilagođenu sliku imena ne prikazuje na popisu da biste imali provjerite je li ispravno unijeli naziv.    

    Za ovaj vodič upišite **U** da biste otvorili popis Ubuntu slike, a zatim odaberite **Ubuntu Server 14.04 LTS**.

1. **Pokretanje načina**, odaberite **SSH**.

1. Kopirajte skripte u nastavku i zalijepite u polje **Init skripte** .

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

    **Skripta Init** će se izvršavati nakon stvaranja na VM. U ovom primjeru skriptu instalira Java i brojka ant.

1. U poljima **korisničko ime** i **lozinku** za administratorski račun koji će se stvoriti na vašem VM unesite željene vrijednosti.

1. Valjani su klikom na **Provjera predložak** da biste provjerili ako parametre koje ste naveli.

1. Kliknite **Spremi**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Stvaranje zadatka Hudson koja se izvršava na podređeni čvor na Azure

U ovom ćete odjeljku stvorit ćete Hudson zadatak koji će se izvoditi na podređeni čvor na Azure.

1. Na nadzornoj ploči Hudson kliknite **Novi zadatak**.

1. Unesite naziv za posao stvarate.

1. Za vrstu posla, odaberite **Sastavljanje posao softver slobodno stil**.

1. Kliknite **u redu**.

1. Na stranici Konfiguracija posla, odaberite **Ograniči gdje se može pokrenuti taj projekt**.

1. Odaberite **čvor i oznaka izbornika** i odaberite **linux** (smo navedeni ove oznake pri stvaranju predložaka virtualnog računala u prethodnom odjeljku).

1. U odjeljku **Sastavljanje** kliknite **Dodaj sastavljanje korak** i odaberite **ljuske za izvršavanje**.

1. Uređivanje sljedeću skriptu **{github naziv svog računa}**, **{naziva projekta}**i **{projekta direktorija}** zamjenjuje odgovarajuće vrijednosti i zalijepite uređivati skripte u područje teksta koji će se prikazati.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e {your project directory} ]; then

            cd {your project directory}

            git pull origin master

        else

            git clone https://github.com/{your github account name}/{your project name}.git

        fi

        # change directory to project

        cd $currentDir/{your project directory}

        #Execute build task

        ant

1. Kliknite **Spremi**.

1. Na nadzornoj ploči Hudson pronađite posao samo stvorili i kliknite ikonu **raspored na Sastavi** .

Hudson će stvoriti podređeni čvor pomoću predloška stvorili u prethodnom odjeljku i izvoditi skripte koje ste naveli u koraku Sastavi za taj zadatak.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java].

<!-- URL List -->

[Razvojni centar za Azure Java]: https://azure.microsoft.com/develop/java/
[profil pretplate]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

