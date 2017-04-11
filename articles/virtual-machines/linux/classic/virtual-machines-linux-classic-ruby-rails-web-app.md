<properties
    pageTitle="Glavno računalo Ruby na web-mjestu tračnicama na Linux VM | Microsoft Azure"
    description="Postavljanje i hostira Ruby utemeljen na tračnicama web-mjesta na Azure pomoću Linux virtualnog računala."
    services="virtual-machines-linux"
    documentationCenter="ruby"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="web"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Ruby tračnicama web-aplikacije na VM programa Azure

Pomoću ovog praktičnog vodiča pokazuje kako hostira Ruby na web-mjestu tračnicama na Azure pomoću Linux virtualnog računala.  

Pomoću ovog praktičnog vodiča je provjeriti pomoću Ubuntu Server 14.04 LTS. Ako koristite drugi Linux raspodjele, možda ćete morati izmijeniti korake da biste instalirali tračnicama.

> [AZURE.IMPORTANT] Azure sadrži dva različitoj implementaciji modela za stvaranje i rad s resursima: [Voditelj resursa i classic](../../../resource-manager-deployment-model.md).  U ovom se članku opisuje pomoću klasične implementacije modela. Microsoft preporučuje da najčešće novi implementacijama korištenje modela Voditelj resursa.

## <a name="create-an-azure-vm"></a>Stvaranje Azure VM

Započnite stvaranjem programa Azure VM slikom Linux.

Da biste stvorili na VM, možete koristiti web-mjesto portala za Azure klasični ili u okvir za Azure sučelje naredbenog retka (EŽA).

### <a name="azure-management-portal"></a>Portal za upravljanje Azure

1. Prijavite se na [portal za Azure klasični](http://manage.windowsazure.com)
2. Kliknite **Novi** > **izračunati** > **virtualnog računala** > **brzo stvaranje**. Odaberite sliku Linux.
3. Unesite lozinku.

Kada je na VM dodjeli, kliknite naziv VM pa kliknite **nadzorna ploča**. Pronađite krajnja točka za SSH, u odjeljku **SSH pojedinosti**.

### <a name="azure-cli"></a>Azure EŽA

Slijedite korake u odjeljku [Stvaranje virtualnog računala radi Linux][vm-instructions].

Kada je na VM dodjeli, prikazat će vam krajnju točku SSH ponovnim pokretanjem sljedeće naredbe:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Kliknite pločicu Ruby tračnicama

1. Da biste se povezali s VM pomoću SSH.

2. Iz sesije SSH da biste instalirali Ruby na na VM koristiti sljedeće naredbe:

        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install ruby ruby-dev build-essential libsqlite3-dev zlib1g-dev nodejs -y

    Instalacija može potrajati nekoliko minuta. Kada se dovrši, koristite sljedeću naredbu da biste provjerili je li instaliran Ruby:

        ruby -v

    Ovo prikazuje verziju Ruby koji je instaliran.

3. Da biste instalirali tračnicama, koristite sljedeću naredbu:

        sudo gem install rails --no-rdoc --no-ri -V

    Korištenje – ne rdoc i – bez ri zastavice da biste preskočili instalacije dokumentaciju, što je brže.
    Ta se naredba vjerojatno zahtijeva puno vremena za izvršavanje, tako da dodate -V prikazat će se informacije o tijeku instalacije.

## <a name="create-and-run-an-app"></a>Stvaranje i pokretanje aplikacije

Dok je i dalje prijavljeni putem SSH, pokrenite sljedeće naredbe:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

Naredba [Nova](http://guides.rubyonrails.org/command_line.html#rails-new) stvara novu aplikaciju tračnicama. Naredba [poslužitelja](http://guides.rubyonrails.org/command_line.html#rails-server) pokreće WEBrick web-poslužitelju koji se isporučuju s tračnicama. (Za korištenje radnog bi vjerojatno želite koristiti drugi poslužitelj, kao što su Unicorn ili putnika.)

Trebali biste vidjeti izlaz otprilike ovako.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Dodavanje krajnje točke

1. Idite na [portal za Azure klasični] [ management-portal] , a zatim odaberite vaše VM.

    ![Popis virtualnog računala][vmlist]

2. Odaberite **krajnje točke** pri vrhu stranice, a zatim **+ dodati krajnju TOČKU** pri dnu stranice.

    ![stranica za krajnje točke][endpoints]

3. U dijaloškom okviru **Dodavanje krajnje točke** odaberite "Dodaj samostalni krajnje", a zatim kliknite strelicu za **sljedeće** .

    ![Novi dijaloški okvir krajnje točke][new-endpoint1]

3. Na sljedećoj stranici dijaloški okvir unesite sljedeće podatke:

    * **Naziv**: HTTP

    * **Protokol**: TCP

    * **JAVNO PRIKLJUČAK**: 80

    * **PRIVATNA PRIKLJUČKA**: 3000

    To će stvoriti javne priključak od 80 koji će se usmjeriti promet na privatne priključak za 3000, gdje priključuje tračnicama poslužitelja.

    ![Novi dijaloški okvir krajnje točke][new-endpoint]

4. Kliknite kvačicu da biste spremili na krajnjoj točki.

5. Moraju se pojaviti poruka koja navodi **Ažuriranja u tijeku**. Kada nestane ovu poruku, krajnju točku je aktivna. Sada mogu testirati aplikacija tako da odete DNS naziva virtualnog računala. Na web-mjestu trebao izgledati otprilike ovako:

    ![zadane stranice tračnicama][default-rails-cloud]

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču niste većinu korake ručno. U okruženju radnog želite pisati aplikacije na računalo razvoj i implementacija Azure VM. Osim toga, većina radnog okruženja u kojima se hostira tračnicama aplikacije u kombinaciji s drugim procesom poslužitelja kao što su Apache ili NginX, koje ručice zahtjev za usmjeravanje više instanci programa tračnicama i posluživanje statične resursi. Dodatne informacije potražite u članku http://rubyonrails.org/deploy/.

Da biste saznali više o Ruby na tračnicama, posjetite [Ruby na tračnicama vodilice][rails-guides].

Da biste koristili Azure servisi Ruby aplikacije, pogledajte:

* [Spremanje nestrukturirane podatke pomoću blob-ova][blobs]

* [Spremište ključa vrijednosti parove korištenje tablica][tables]

* [Posluživanje visoke propusnosti sadržaja s mreže za isporuku sadržaja][cdn-howto]

<!-- WA.com links -->
[blobs]: ../../../storage/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]: https://azure.microsoft.com/develop/ruby/app-services/
[management-portal]: https://manage.windowsazure.com/
[tables]: ../../../storage/storage-ruby-how-to-use-table-storage.md
[vm-instructions]: ../../virtual-machines-linux-classic-createportal.md

<!-- External Links -->
[rails-guides]: http://guides.rubyonrails.org/
[sqlite3]: http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
