<properties
    pageTitle="Azure EŽA naredbi u načinu rada za upravljanje servisom | Microsoft Azure"
    description="Naredbe Azure naredbeni redak sučelja (EŽA) u načinu rada za upravljanje servisom za upravljanje implementacijama u modelu klasični implementacije"
    services="virtual-machines-linux,virtual-machines-windows,mobile-services, cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="danlep"/>

# <a name="azure-cli-commands-in-azure-service-management-asm-mode"></a>Azure EŽA naredbi u načinu rada za upravljanje servisom Azure (asm)

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-classic-include.md)]Možete i [Saznajte više o sve naredbe modela Voditelj resursa](virtual-machines/azure-cli-arm-commands.md), i koristiti u EŽA će se [migrirati resursa](virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md) od klasičnog model Voditelj resursa.

Ovaj članak sadrži sintaksa i mogućnosti za Azure EŽA naredbe koje se najčešće koriste stvaranje i upravljanje Azure resursa u model klasični implementacije. Te naredbe pristupiti tako da pokrenete u EŽA u načinu rada za upravljanje servisom Azure (asm). Nije dovršeno referenca, a vaša verzija EŽA mogu prikazati malo drugačije naredbe ili parametre. 

Da biste počeli, prvi [instalirati EŽA Azure](xplat-cli-install.md) i [Povezivanje s Azure pretplatu](xplat-cli-connect.md).

Za trenutni Sintaksa naredbe i mogućnosti u naredbenom retku upišite `azure help` ili prikaz pomoći za određenu naredbu `azure help [command]`. Pronaći i primjeri EŽA u dokumentaciji za stvaranje i upravljanje određene Azure usluge.

Neobavezni parametri prikazuju se u uglatim zagradama (na primjer, `[parameter]`). Potrebni su sve parametre.

Osim specifične za naredbu neobavezni parametri navedenih u nastavku, postoje tri neobavezne parametre koje je moguće koristiti za prikaz detaljnih izlaz kao što su mogućnosti zahtjev i šifre statusa. Na `-v` parametar omogućuje opširno izlazne i `-vv` parametar nudi čak i detaljnije opširno izlaz. Na `--json` mogućnost proizvodi rezultat u obliku neobrađenog json.

## <a name="setting-asm-mode"></a>Postavka asm načinu

Koristite sljedeću naredbu da biste omogućili upravljanje servisom za Azure EŽA način naredbe.

    azure config mode asm

>[AZURE.NOTE] Način za Azure Voditelj resursa i upravljanje servisom Azure način u EŽA isključuju. To je resursa stvorene u jednom načinu nije moguće upravljati iz drugih načina.

## <a name="manage-your-account-information-and-publish-settings"></a>Upravljanje informacijama računa i postavke objavljivanja
Jednosmjerna na EŽA možete povezati s računom se pomoću informacija o pretplati Azure. (Potražite u članku [Povezivanje s Azure pretplate s EŽA Azure](xplat-cli-connect.md) ostale mogućnosti) Ove informacije možete dobivenog Azure klasični portal u datoteci postavki Objavi kao što je opisano u nastavku. Datoteka postavki za objavljivanje možete uvesti kao što je stalni lokalnoj konfiguraciji postavka koja se EŽA koristi za sljedeće postupke. Što je potrebno da biste uvezli vaše postavke objavljivanja jedanput.

**Preuzimanje račun [mogućnosti]**

Ta se naredba pokreće preglednik da biste preuzeli .publishsettings datoteku s portala za Azure klasični.

    ~$ azure account download
    info:   Executing command account download
    info:   Launching browser to https://windows.azure.com/download/publishprofile.aspx
    help:   Save the downloaded file, then execute the command
    help:   account import <file>
    info:   account download command OK

**račun uvoz [mogućnosti] &lt;datoteka >**


Ta se naredba uvozi publishsettings datoteke ili certifikat tako da se može se koristiti alatom za ubuduće sesije.

    ~$ azure account import publishsettings.publishsettings
    info:   Importing publish settings file publishsettings.publishsettings
    info:   Found subscription: 3-Month Free Trial
    info:   Found subscription: Pay-As-You-Go
    info:   Setting default subscription to: 3-Month Free Trial
    warn:   The 'publishsettings.publishsettings' file contains sensitive information.
    warn:   Remember to delete it now that it has been imported.
    info:   Account publish settings imported successfully

> [AZURE.NOTE] Datoteka publishsettings može sadržavati pojedinosti (to jest, naziv pretplate i ID) više pretplata. Kada uvezete datoteku publishsettings, prvi pretplate koristi se kao zadani opis. Da biste koristili neku drugu pretplatu, pokrenite sljedeću naredbu:
<code>~$ azure config set subscription &lt;other-subscription-id&gt;</code>

**računa poništite [mogućnosti]**

Ta se naredba uklanja spremljene publishsettings koji su uvezeni. Ako završite pomoću alata na ovom računalu, koristite sljedeću naredbu i želite da biste dobili da alat ne može se koristiti s vašim računom sesije u budućnosti.

    ~$ azure account clear
    Clearing account info.
    info:   OK

**Popis poslovnih subjekata [mogućnosti]**

Popis uvezenih pretplate

    ~$ azure account list
    info:    Executing command account list
    data:    Name                                    Id
           Current
    data:    --------------------------------------  -------------------------------
    -----  -------
    data:    Forums Subscription                     8679c8be-3b05-49d9-b8fb  true
    data:    Evangelism Team Subscription            9e672699-1055-41ae-9c36  false
    data:    MSOpenTech-Prod                         c13e6a92-706e-4cf5-94b6  false

**Postavljanje računa [mogućnosti] &lt;pretplate&gt;**

Postavljanje trenutne pretplate

###<a name="commands-to-manage-your-affinity-groups"></a>Naredbe za upravljanje grupama za afinitet

**Popis afinitet grupe računa [mogućnosti]**

Ta se naredba popis Azure afinitet grupe.

Afinitet grupe mogu se postaviti kada grupe virtualnim strojevima obuhvaća više fizička računala. Grupa afinitet određuje fizičke strojeva treba kao blizu međusobno moguće, smanjite latenciju mreže.

    ~$ azure account affinity-group list
    + Fetching affinity groups
    data:   Name                                  Label   Location
    data:   ------------------------------------  ------  --------
    data:   535EBAED-BF8B-4B18-A2E9-8755FB9D733F  opentec  West US
    info:   account affinity-group list command OK

**[mogućnosti] stvaranja afinitet grupa računa &lt;naziv&gt;**

Ta naredba stvara grupu afinitet

    ~$ azure account affinity-group create opentec -l "West US"
    info:    Executing command account affinity-group create
    + Creating affinity group
    info:    account affinity-group create command OK

**Afinitet grupa računa Pokaži [mogućnosti] &lt;naziv&gt;**

Ta se naredba prikazuje detalje o grupi afinitet

    ~$ azure account affinity-group show opentec
    info:    Executing command account affinity-group show
    + Getting affinity groups
    data:    $ xmlns "http://schemas.microsoft.com/windowsazure"
    data:    $ xmlns:i "http://www.w3.org/2001/XMLSchema-instance"
    data:    Name "opentec"
    data:    Label "b3BlbnRlYw=="
    data:    Description $ i:nil "true"
    data:    Location "West US"
    data:    HostedServices ""
    data:    StorageServices ""
    data:    Capabilities Capability 0 "PersistentVMRole"
    data:    Capabilities Capability 1 "HighMemory"
    info:    account affinity-group show command OK

**račun afinitet grupe izbrišite [mogućnosti] &lt;naziv&gt;**

Ta se naredba briše navedene afinitet grupe

    ~$ azure account affinity-group delete opentec
    info:    Executing command account affinity-group delete
    Delete affinity group opentec? [y/n] y
    + Deleting affinity group
    info:    account affinity-group delete command OK

###<a name="commands-to-manage-your-account-environment"></a>Naredbe za upravljanje okruženje za račun

**Popis poslovnih subjekata env [mogućnosti]**

Popis okruženja računa

    C:\windows\system32>azure account env list
    info:    Executing command account env list
    data:    Name
    data:    ---------------
    data:    AzureCloud
    data:    AzureChinaCloud
    info:    account env list command OK

**račun env Pokaži [mogućnosti] [okruženje]**

Prikaz pojedinosti o računu okruženje

    ~$ azure account env show
    info:    Executing command account env show
    Environment name: AzureCloud
    data:    Environment publishingProfile  http://go.microsoft.com/fwlink/?LinkId=2544
    data:    Environment portal  http://go.microsoft.com/fwlink/?LinkId=2544
    info:    account env show command OK

**račun env dodavanje [odrednice] [okruženje]**

Ta se naredba dodaje okruženju na račun

**Postavljanje računa env [mogućnosti] [okruženje]**

Ta se naredba postavlja okruženje za račun

**račun env izbrišite [mogućnosti] [okruženje]**

Ta se naredba briše navedeni okruženje s računa

## <a name="commands-to-manage-your-classic-virtual-machines"></a>Naredbe za upravljanje klasični virtualnim strojevima
Sljedeći dijagram prikazuje kako klasični Azure virtualnim strojevima nalaze se u okruženju za implementaciju radnog od Azure oblaku.

![Azure tehničke dijagrama](./media/virtual-machines-command-line-tools/architecturediagram.jpg)

**Stvaranje – novi** stvara pogon u spremište blobova (to jest, e:\ u dijagramu); **prilaganje** pridružuje već stvoren, ali nepovezane na disku virtualnog računala.

**[mogućnosti] stvaranja VM &lt;naziv DNS-a > &lt;slika > &lt;korisničko ime > [lozinka]**

Ta naredba stvara Azure virtualnog računala. Prema zadanim postavkama, svaki virtualnog računala (vm) se stvara u vlastitom u oblaku. Možete odrediti da virtualnog računala treba dodati postojeći oblaka servis pomoću koristite mogućnost - c kao navedenih u nastavku.

Na vm stvorite naredbe, poput portala za Azure klasični, samo stvara virtualnim strojevima u okruženje za implementaciju sustava radnog. Ne postoji mogućnost da biste stvorili virtualnog računala u pripremna okruženje implementacije za servise u oblaku. Ako pretplatu na postojeći račun Azure prostora za pohranu, naredba stvara jednu.

Možete navesti mjesto kroz – mjesto parametar ili možete odrediti granu afinitet kroz parametar – afinitet grupe. Ako nijedan nije naveden, morat ćete unijeti jednu od popisa valjana mjesta.

Navedena lozinka mora biti 8 123 znakova i zadovoljavaju složenosti lozinki operacijskog sustava koji koristite za ovu virtualnog računala.

Ako namjeravate koristiti SSH da biste upravljali distribuiranih Linux virtualnog računala (kao što je obično slučaj), morate omogućiti SSH putem -e mogućnost prilikom stvaranja virtualnog računala. Nije moguće da biste omogućili SSH nakon stvaranja virtualnog računala.

Virtualnim strojevima sa sustavom Windows možete omogućiti RDP kasnije dodavanjem priključak 3389 kao krajnje točke.

Za tu naredbu podržane su sljedeće neobavezne parametre:

**- c, – povežite** stvaranje virtualnog računala unutar već stvoreni implementacije u usluge hostinga. Ako - vmname ne koriste tu mogućnost, automatski se generira naziv novog virtualnog računala.<br />
**-n, – vm naziv** Navedite naziv virtualnog računala. Ovaj parametar vodi hostinga naziv usluge prema zadanim postavkama. Ako - vmname nije naveden, naziv za novu virtualnog računala generira se kao &lt;naziv usluge >&lt;ID-a >, pri čemu &lt;ID-a > je broj postojeće virtualnim strojevima servis plus 1. Na primjer, ako koristite sljedeću naredbu da biste dodali virtualnog računala hostinga servisa MyService koja sadrži jedan postojeće virtualnog računala, novi virtualnog računala pod nazivom MyService2.<br />
**-u, – blob-URL-a** Navedite odredišni URL spremišta blobova platforme na kojem želite stvoriti disk sustava virtualnog računala. <br />
**+ z, veličina – vm** Odredite veličinu virtualnog računala. Valjane vrijednosti su: "ExtraSmall", "Mala", "Srednji", "Velika", "ExtraLarge", "A5", "A6", "A7", "A8", "A9", "A10", "A11", "Basic_A0", "Basic_A1", "Basic_A2", "Basic_A3", "Basic_A4", "Standard_D1", "Standard_D2", "Standard_D3", "Standard_D4", "Standard_D11", "Standard_D12", "Standard_D13", "Standard_D14", "Standard_DS1", "Standard_DS2", "Standard_DS3", "Standard_DS4", "Standard_DS11", "Standard_DS12", "Standard_DS13", "Standard_DS14", "Standard_G1", "Standard_G2" , "Standard_G3", "Standard_G4", "Standard_G55". Zadana je vrijednost "Mali". <br />
**-r** Dodaje RDP povezivanje virtualnog računala za Windows. <br />
**-e, – ssh** Dodaje SSH povezivanje virtualnog računala za Windows. <br />
**-t, – ssh – certifikata** Određuje SSH certifikata. <br />
**-s** Pretplate <br />
**-o, – zajednice** Navedeni slika je slika zajednice <br />
**-w** Naziv virtualne mreže <br/>
**-l, – mjesto** određuje mjesto (na primjer, "Sjeverna središnje mi"). <br />
**-a, – afinitet grupe** određuje afinitet grupe.<br />
**-w, – virtualne--naziv mreže** Odredite virtualna mreže na koju želite dodati novi virtualnog računala. Virtualne mreže možete postaviti i upravlja se putem portala za Azure klasični.<br />
**-b, – podmreže imena** Određuje podmreže imena koja želite dodijeliti virtualnog računala.

U ovom primjeru MSFT__Win2K8R2SP1-120514-1520-141205-01-en-us-30GB je slici prikazan nudi platforme. Dodatne informacije o slikama operacijski sustav potražite u članku vm popisa slika.

    ~$ azure vm create my-vm-name MSFT__Windows-Server-2008-R2-SP1.11-29-2011 username --location "West US" -r
    info:   Executing command vm create
    Enter VM 'my-vm-name' password: ************
    info:   vm create command OK

**VM stvaranje izvora &lt;naziv DNS-a > &lt;uloga datoteka >**

Ta naredba stvara Azure virtualnog računala iz datoteke JSON uloge.

    ~$ azure vm create-from my-vm example.json
    info:   OK

**Popis VM [mogućnosti]**

Ta se naredba popis Azure virtualnih računala. Mogućnost – json određuje u obliku neobrađenog JSON vraćaju rezultate.

    ~$ azure vm list
    info:   Executing command vm list
    data:   DNS Name                          VM Name      Status
    data:   --------------------------------  -----------  ---------
    data:   my-vm-name.cloudapp-preview.net        my-vm        ReadyRole
    info:   vm list command OK

**Popis lokacija VM [mogućnosti]**

U ovom command dohvaća sve dostupne račun za Azure lokacije.

    ~$ azure vm location list
    info:   Executing command vm location list
    data:   Name                   Display Name
    data:   ---------------------  ------------
    data:   Azure Preview  West US
    info:   account location list command OK

**VM Pokaži [mogućnosti] &lt;naziv >**

Ta se naredba prikazuje detalje o Azure virtualnog računala. Mogućnost – json određuje u obliku neobrađenog JSON vraćaju rezultate.

    ~$ azure vm show my-vm
    info:   Executing command vm show
    data:   {
    data:       InstanceSize: 'Small',
    data:       InstanceStatus: 'ReadyRole',
    data:       DataDisks: [],
    data:       IPAddress: '10.26.192.206',
    data:       DNSName: 'my-vm.cloudapp.net',
    data:       InstanceStateDetails: {},
    data:       VMName: 'my-vm',
    data:       Network: {
    data:           Endpoints: [
    data:               {
    data:                   Protocol: 'tcp',
    data:                   Vip: '65.52.250.250',
    data:                   Port: '63238' ,
    data:                   LocalPort: '3389',
    data:                   Name: 'RemoteDesktop'
    data:               }
    data:           ]
    data:       },
    data:       Image: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       OSVersion: 'WA-GUEST-OS-1.18_201203-01'
    data:   }
    info:   vm show command OK

**Izbrišite VM [mogućnosti] &lt;naziv >**

Ta se naredba briše Azure virtualnog računala. Prema zadanim postavkama ta se naredba Izbriši blobova platforme Azure iz kojeg se stvaraju na disku operacijski sustav i podatkovni disk. Da biste izbrisali blob-om i virtualnog računala na kojem se temelji, navedite željenu mogućnost -b.

    ~$ azure vm delete my-vm
    info:   Executing command vm delete
    info:   vm delete command OK

**[mogućnosti] pokretanja VM &lt;naziv >**

Ta se naredba pokreće Azure virtualnog računala.

    ~$ azure vm start my-vm
    info:   Executing command vm start
    info:   vm start command OK

**[mogućnosti] ponovno pokrenite VM &lt;naziv >**

Ta se naredba pokreće Azure virtualnog računala.

    ~$ azure vm restart my-vm
    info:   Executing command vm restart
    info:   vm restart command OK

**isključivanje VM [mogućnosti] &lt;naziv >**

Ta se naredba isključuje Azure virtualnog računala. Mogućnost -p možete koristiti da biste odredili da resursa računalnim ne oslobađa na zatvaranja.

```
~$ azure vm shutdown my-vm
info:   Executing command vm shutdown
info:   vm shutdown command OK  
```

**snimanje VM &lt;vm naziv > &lt;cilj, slika i ime >**

Ta se naredba snima se slika Azure virtualnog računala.

Sliku virtualnog računala možete zabilježene samo ako je stanje virtualnog računala **zaustavljanja**. Prije nastavka zatvorite virtualnog računala.

    ~$ azure.cmd vm capture my-vm mycaptureimagename --delete
    info:   Executing command vm capture
    + Fetching VMs
    + Capturing VM
    info:   vm capture command OK

**Izvoz VM [mogućnosti] &lt;vm naziv > &lt;put datoteke >**

Ta se naredba izvozi Azure virtualnog računala sliku u datoteku

    ~$ azure vm export "myvm" "C:\"
    info:    Executing command vm export
    + Getting virtual machines
    + Exporting the VM
    info:   vm export command OK

##  <a name="commands-to-manage-your-azure-virtual-machine-endpoints"></a>Naredbe za upravljanje sustava Azure virtualnog računala krajnje točke
Sljedeći dijagram prikazuje arhitektura uobičajeni implementacije više instanci klasični virtualnog računala. U ovom primjeru priključak 3389 je otvorena na svakom virtualnog računala (za pristup RDP). Postoji i interne IP adrese (na primjer, 168.55.11.1) na virtualnog računala koja se koristi opterećenja za usmjeravanje prometa virtualnog računala. Interna IP adresa može se koristiti za komunikaciju između virtualnih računala.

![azurenetworkdiagram](./media/virtual-machines-command-line-tools/networkdiagram.jpg)

Vanjski zahtjevi za virtualnim strojevima proći kroz raspoređivača opterećenja. Zbog toga zahtjeva nije moguće navesti odnosu na određeni virtualnog računala na implementacijama s više virtualnim računalima. U slučaju implementacije s više virtualnim strojevima preslikavanje priključka mora biti konfigurirana između virtualnim strojevima (vm priključak) i raspoređivača opterećenja (lb priključak).

**Stvaranje krajnje točke VM &lt;vm naziv > &lt;lb priključak > [vm priključak]**

Ta naredba stvara krajnjoj točki virtualnog računala. Da biste odredili hoće li da biste omogućili izravno poslužitelj vratiti na ovom krajnje točke, po zadanom onemogućeni možda koristi i -u ili – Omogući-izravno-server-return.

    ~$ azure vm endpoint create my-vm 8888 8888
    azure vm endpoint create my-vm 8888 8888
    info:   Executing command vm endpoint create
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint create command OK

**vm krajnjoj točki stvaranje-više [mogućnosti] &lt;vm naziv > &lt;lb priključak > [:&lt;vm priključak > [:&lt;protokol > [:&lt;Omogući-izravno-server-return > [:&lt;lb naziv-skupa > [:&lt;probni protokol > [:&lt;probni priključak > [:&lt;probni put > [:&lt;-lb-naziv internog >]]] {1 –*}**

Stvaranje više vm krajnje točke.

**Krajnja točka VM izbrišite [mogućnosti] &lt;vm naziv > &lt;naziv krajnje točke >**

Ta se naredba brisanje krajnje točke virtualnog računala.

    ~$ azure vm endpoint delete my-vm http
    azure vm endpoint delete my-vm http
    info:   Executing command vm endpoint delete
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint delete command OK

**Popis krajnjoj točki VM &lt;vm naziv >**

Ta se naredba navedene krajnje točke za sve virtualnog računala. Mogućnost – json određuje u obliku neobrađenog JSON vraćaju rezultate.

    ~$ azure vm endpoint list my-linux-vm
    data:   Name  External Port  Local Port
    data:   ----  -------------  ----------
    data:   ssh   22             22

**Ažuriranje krajnjoj točki VM [mogućnosti] &lt;vm naziv > &lt;naziv krajnje točke >**

Ta se naredba ažurira krajnjoj točki vm u nove vrijednosti pomoću ove mogućnosti.

    -n, --endpoint-name <name>          the new endpoint name
    -lo, --lb-port <port>                the new load balancer port
    -t, --vm-port <port>                the new local port
    -o, --endpoint-protocol <protocol>  the new transport layer protocol for port (tcp or udp)

**Krajnja točka VM Pokaži [mogućnosti] &lt;vm naziv >**

Ta se naredba prikazuje detalje o krajnjih točaka u vm

    ~$ azure vm endpoint show "mycouchvm"
    info:    Executing command vm endpoint show
    + Getting virtual machines
    data:    Network Endpoints 0 LoadBalancedEndpointSetName "CouchDB_EP-5984"
    data:    Network Endpoints 0 LocalPort "5984"
    data:    Network Endpoints 0 Name "CouchDB_EP"
    data:    Network Endpoints 0 Port "5984"
    data:    Network Endpoints 0 Protocol "tcp"
    data:    Network Endpoints 0 Vip "168.61.9.97"
    data:    Network Endpoints 1 LoadBalancedEndpointSetName "CouchEP_2-2020"
    data:    Network Endpoints 1 LocalPort "2020"
    data:    Network Endpoints 1 Name "CouchEP_2"
    data:    Network Endpoints 1 Port "2020"
    data:    Network Endpoints 1 Protocol "tcp"
    data:    Network Endpoints 1 Vip "168.61.9.97"
    data:    Network Endpoints 2 LocalPort "3389"
    data:    Network Endpoints 2 Name "RemoteDesktop"
    data:    Network Endpoints 2 Port "3389"
    data:    Network Endpoints 2 Protocol "tcp"
    data:    Network Endpoints 2 Vip "168.61.9.97"
    info:    vm endpoint show command OK

## <a name="commands-to-manage-your-azure-virtual-machine-images"></a>Naredbe za upravljanje slike Azure virtualnog računala

Slika virtualnog računala je snimljeni sadržaj već konfiguriran virtualnih računala koje je moguće je replicirati prema potrebi.

**popis slika VM [mogućnosti]**

Ta se naredba može vidjeti popis slika virtualnog računala. Postoje tri vrste slika: Stvaranje slike trećim stranama su mjestu pod nazivom dobavljača i slika koje je stvorio slike stvorio Microsoft, koje su mjestu s "MSFT". Da biste stvorili slike, možete snimiti postojeće virtualnog računala ili stvaranje sliku iz prilagođene .vhd prenijeti u spremište blobova platforme. Dodatne informacije o korištenju prilagođene .vhd, potražite u članku vm slika stvaranje.
Mogućnost – json određuje u obliku neobrađenog JSON vraćaju rezultate.

    ~$ azure vm image list
    data:   Name                                                                   Category   OS
    data:   ---------------------------------------------------------------------  ---------  -------
    data:   CANONICAL__Canonical-Ubuntu-12-04-20120519-2012-05-19-en-us-30GB.vhd   Canonical  Linux
    data:   MSFT__Windows-Server-2008-R2-SP1.11-29-2011                            Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1-with-SQL-Server-2012-Eval.11-29-2011  Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.en-us.30GB.2012-03-22                      Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.2-17-2012                                  Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1.en-us.30GB.2012-3-22                  Microsoft  Windows
    data:   OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd                 OpenLogic  Linux
    data:   SUSE__SUSE-Linux-Enterprise-Server-11SP2-20120521-en-us-30GB.vhd       SUSE       Linux
    data:   SUSE__OpenSUSE64121-03192012-en-us-15GB.vhd                            SUSE       Linux
    data:   WIN2K8-R2-WINRM                                                        User       Windows
    info:   vm image list command OK

**Prikaz slika VM [mogućnosti] &lt;naziv >**

Ta se naredba prikazuje detalje o sliku virtualnog računala.

    ~$ azure vm image show MSFT__Windows-Server-2008-R2-SP1.11-29-2011
    + Fetching VM image
    info:   Executing command vm image show
    data:   {
    data:       Label: 'Windows Server 2008 R2 SP1, Nov 2011',
    data:       Name: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       Description: 'Microsoft Windows Server 2008 R2 SP1',
    data:       @: { xmlns: 'http://schemas.microsoft.com/windowsazure', xmlns:i: 'http://www.w3.org/2001/XMLSchema-instance' },
    data:       Category: 'Microsoft',
    data:       OS: 'Windows',
    data:       Eula: 'http://www.microsoft.com',
    data:       LogicalSizeInGB: '30'
    data:   }
    info:   vm image show command OK

**Brisanje slika VM [mogućnosti] &lt;naziv >**

Ta se naredba briše sliku virtualnog računala.

    ~$ azure vm image delete my-vm-image
    info:   Executing command vm image delete
    info:   VM image deleted: my-vm-image
    info:   vm image delete command OK

**Stvaranje slike VM &lt;naziv > [izvorni put]**

Ta naredba stvara sliku virtualnog računala. Datoteka prilagođenih .vhd prenose bloba prostora za pohranu, a zatim će se slika virtualnog računala stvorene iz nje. Na ovoj se slici virtualnog računala pa koristite da biste stvorili virtualnog računala. Potrebni su parametri mjesto i OS.

>[AZURE.NOTE]Trenutno ta naredba podržava samo prijenos fixed .vhd datoteke. Da biste prenijeli datoteke dinamičke .vhd, koristite [Azure VHD uslužni programi za Idi](https://github.com/Microsoft/azure-vhd-utils-for-go).

Neki sustavi nametnuti ograničenja opisnika po postupak datoteka. Ako je to ograničenje premaši, alat prikazuje pogreška ograničenje opisnika datoteke. Možete pokrenuti naredbu ponovno korištenje -p &lt;broj > parametar da biste smanjili maksimalni broj paralelno prijenosa. Zadani maksimalni broj paralelno prijenosi je 96.

    ~$ azure vm image create mytestimage ./Sample.vhd -o windows -l "West US"
    info:   Executing command vm image create
    + Retrieving storage accounts
    info:   VHD size : 13 MB
    info:   Uploading 13312.5 KB
    Requested:100.0% Completed:100.0% Running: 105 Time:    8s Speed:  1721 KB/s
    info:   http://myaccount.blob.core.azure.com/vm-images/Sample.vhd is uploaded successfully
    info:   vm image create command OK

## <a name="commands-to-manage-your-azure-virtual-machine-data-disks"></a>Naredbe za upravljanje sustava diskova podataka Azure virtualnog računala

Diskova podaci su .vhd datoteke u spremište blobova platforme koje je moguće koristiti virtualnog računala. Dodatne informacije o korištenju podataka diskova uvode se bloba prostora za pohranu potražite Azure tehničke dijagram ranije prikazane.

Naredbe za pridruživanje diskova podataka (disk azure vm priložiti i azure vm diska priložite – nova) logičke jedinice broj (LUN) dodijeliti priložene podatkovni disk, po potrebi prema protokolu SCSI. Prvi podatkovni disk priložiti virtualnog računala vam je dodijeljen LUN 0, sljedeći je dodijeljena LUN 1 i tako dalje.

Kada odvajanje podatkovni disk disku azure vm odvajanje naredbu, koristite na &lt;lun&gt; parametara koji određuju koji disk odvajanje.

>[AZURE.NOTE] Trebali biste uvijek odvojite podataka diskova obrnutim redoslijedom, počevši od najveće numerirani LUN koji vam je dodijeljen. Ne podržava odvajanje na donjem brojevima LUN dok je i dalje povezan s većim brojem LUN sloj Linux SCSI. Ako, na primjer, ne treba odvajanje LUN 0 Ako i dalje pridružen LUN 1.

**Prikaži VM disk [mogućnosti] &lt;naziv >**

Ta se naredba prikazuje detalje o Azure disk.

    ~$ azure vm disk show anucentos-anucentos-0-20120524070008
    info:   Executing command vm disk show
    data:   AttachedTo DeploymentName "mycentos"
    data:   AttachedTo HostedServiceName "myanucentos"
    data:   AttachedTo RoleName "myanucentos"
    data:   OS "Linux"
    data:   Location "Azure Preview"
    data:   LogicalDiskSizeInGB "30"
    data:   MediaLink "http://mystorageaccount.blob.core.azure-preview.com/vhd-store/mycentos-cb39b8223b01f95c.vhd"
    data:   Name "mycentos-mycentos-0-20120524070008"
    data:   SourceImageName "OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd"
    info:   vm disk show command OK

**popis diskova VM [odrednice] [vm name]**

U ovom naredba popise Azure diskova ili diskova priložiti navedeni virtualnog računala. Ako se izvodi s naziv parametra virtualnog računala, vratit će sve diskova priložiti virtualnog računala. Lun 1 stvara se pomoću virtualnog računala i drugih popisa diskova su priložene zasebno.

    ~$ azure vm disk list mycentos
    info:   Executing command vm disk list
    data:   Lun  Size(GB)  Blob-Name
    data:   ---  --------  --------------------------------
    data:   1    30        mycentos-cb39b8223b01f95c.vhd
    data:   2    10        mycentos-e3f0d717950bb78d.vhd
    info:   vm disk list command OK

Izvođenje ove naredbe bez naziv parametra virtualnog računala vraća sve diskova.

    ~$ azure vm disk list
    data:   Name                                        OS
    data:   ------------------------------------------  -------
    data:   mycentos-mycentos-0-20120524070008          Linux
    data:   mycentos-mycentos-2-20120525055052
    data:   mywindows-winvm-20120522223119              Windows
    info:   vm disk list command OK

**Brisanje diska VM [mogućnosti] &lt;naziv >**

Ta se naredba briše Azure disk osobne spremištu. Na disku mora biti odvojena iz virtualnog računala prije nego što je izbrisana.

    ~$ azure vm disk delete mycentos-mycentos-2-20120525055052
    info:   Executing command vm disk delete
    info:   Disk deleted: mycentos-mycentos-2-20120525055052
    info:   vm disk delete command OK

**Stvaranje VM disk &lt;naziv > [izvorni put]**

Ta se naredba prenosi i registrira Azure disk. – blob-url, – mjesto ili – afinitet grupa mora biti naveden. Ako koristite tu naredbu s [izvora put], prijenosa datoteka .vhd navedena i stvara se slike. Možete zatim priložiti sliku virtualnog računala pomoću diska vm priložiti.

Neki sustavi nametnuti ograničenja opisnika po postupak datoteka. Ako je to ograničenje premaši, alat prikazuje pogreška ograničenje opisnika datoteke. Možete pokrenuti naredbu ponovno korištenje -p &lt;broj > parametar da biste smanjili maksimalni broj paralelno prijenosa. Zadani maksimalni broj paralelno prijenosi je 96.

    ~$ azure vm disk create my-data-disk ~/test.vhd --location "West US"
    info:   Executing command vm disk create
    info:   VHD size : 10 MB
    info:   Uploading 10240.5 KB
    Requested:100.0% Completed:100.0% Running:  81 Time:   11s Speed:   952 KB/s
    info:   http://account.blob.core.azure.com/disks/test.vhd is uploaded successfully
    info:   vm disk create command OK

**prijenos na disku VM [mogućnosti] &lt;izvorni put > &lt;blob-url > &lt;prostora za pohranu, račun i ključa >**

Ta naredba omogućuje prijenos vm disk

    ~$ azure vm disk upload "http://sourcestorage.blob.core.windows.net/vhds/sample.vhd" "http://destinationstorage.blob.core.windows.net/vhds/sample.vhd" "DESTINATIONSTORAGEACCOUNTKEY"
    info:   Executing command vm disk upload
    info:   Uploading 12351.5 KB
    info:   vm disk upload command OK

**Prilaganje VM disk &lt;vm naziv > &lt;disk, slika i ime >**

Ta se naredba pridružuje postojeći disk u spremište blobova platforme postojeće virtualnog računala implementiran u oblaku.

    ~$ azure vm disk attach my-vm my-vm-my-vm-2-201242418259
    info:   Executing command vm disk attach
    info:   vm disk attach command OK

**VM diska priložite – novi &lt;vm naziv > &lt;veličina u gb > [blob-url]**

Ta se naredba pridružuje podatkovni disk Azure virtualnog računala. U ovom primjeru 20 je veličina novi disk gigabajta priloženi. Po želji možete koristiti blob URL-a kao argument posljednje izričito navesti blob cilj da biste stvorili. Ako ne navedete blob URL-a, automatski se generira blob objekta.

    ~$ azure vm disk attach-new nick-test36 20 http://nghinazz.blob.core.azure-preview.com/vhds/vmdisk1.vhd
    info:   Executing command vm disk attach-new
    info:   vm disk attach-new command OK  

**odvajanje VM disk &lt;vm naziv > &lt;lun >**

Ta se naredba odvaja podatkovni disk priložiti Azure virtualnog računala. &lt;lun > prepoznaje disk biti odvojeni. Da biste popis diskova povezan s diskom prije nego ga odvojiti, koristite diska popisom vm &lt;vm naziv >.

    ~$ azure vm disk detach my-vm 2
    info:   Executing command vm disk detach
    info:   vm disk detach command OK

## <a name="commands-to-manage-your-azure-cloud-services"></a>Naredbe za upravljanje servisa Azure oblaka

Servisi za Azure oblaka su aplikacija i usluga koje se nalaze na webu uloge i uloge suradnika. Sljedeće naredbe se može koristiti za upravljanje uslugama za Azure oblaka.

**servis stvoriti [mogućnosti] &lt;naziv servisa >**

Ta naredba stvara neki servis u oblaku

    ~$ azure service create newservicemsopentech
    info:    Executing command service create
    + Getting locations
    help:    Location:
      1) East Asia
      2) Southeast Asia
      3) North Europe
      4) West Europe
      5) East US
      6) West US
      : 6
    + Creating cloud service
    data:    Cloud service name newservicemsopentech
    info:    service create command OK

**servis Pokaži [mogućnosti] &lt;naziv servisa >**

Ta se naredba prikazuje detalje o Azure oblaku

    ~$ azure service show newservicemsopentech
    info:    Executing command service show
    + Getting cloud service
    data:    Name newservicemsopentech
    data:    Url https://management.core.windows.net/9e672699-1055-41ae-9c36-e85152f2e352/services/hostedservices/newservicemsopentech
    data:    Properties location West US
    data:    Properties label newservicemsopentech
    data:    Properties status Created
    data:    Properties dateCreated
    data:    Properties dateLastModified
    info:    service show command OK

**popis za servis [mogućnosti]**

Ta se naredba popis servisa Azure oblaka.

    ~$ azure service list
    info:   Executing command service list
    data:   Name         Status
    data:   -----------  -------
    data:   service1     Created
    data:   service2     Created
    info:   service list command OK

**Servis za brisanje [mogućnosti] &lt;naziv >**

Ta se naredba briše Azure oblaku.

    ~$ azure service delete myservice
    info:   Executing command service delete myservice
    info:   cloud-service delete command OK

Da biste nametnuli brisanja, poslužite se `-q` parametar.


## <a name="commands-to-manage-your-azure-certificates"></a>Naredbe za upravljanje certifikatima Azure

Servis za Azure certifikati su SSL certifikata povezan s vašim računom Azure. Dodatne informacije o Azure certifikata potražite u članku [Upravljanje certifikata](http://msdn.microsoft.com/library/azure/gg981929.aspx).

**Popis certifikata za servis [mogućnosti]**

Ta se naredba popis Azure certifikata.

    ~$ azure service cert list
    info:   Executing command service cert list
    + Fetching cloud services
    + Fetching certificates
    data:   Service   Thumbprint                                Algorithm
    data:   --------  ----------------------------------------  ---------
    data:   myservice  262DBF95B5E61375FA27F1E74AC7D9EAE842916C  sha1
    info:   service cert list command OK

**Stvaranje certifikata za servis &lt;dns prefiks > &lt;datoteka > [lozinka]**

Ta se naredba prenosi certifikat. Upit za lozinku ostavite prazno za certifikate koji nisu zaštićeni lozinkom.

    ~$ azure service cert create nghinazz ~/publishSet.pfx
    info:   Executing command service cert create
    Cert password:
    + Creating certificate
    info:   service cert create command OK

**Brisanje certifikata servisa [mogućnosti] &lt;otisak prsta >**

Ta se naredba briše certifikat.

    ~$ azure service cert delete 262DBF95B5E61375FA27F1E74AC7D9EAE842916C
    info:   Executing command service cert delete
    + Deleting certificate
    info:   nghinazz : cert deleted
    info:   service cert delete command OK

## <a name="commands-to-manage-your-web-apps"></a>Naredbe za upravljanje aplikacijama web

Azure web-aplikaciju programa je na web-konfiguracija pristupiti tako da URI. Web-aplikacije koje se nalaze na virtualnim računalima sustava, ali ne morate razmislite o detalje o stvaranju i implementaciji virtualnog računala sami. Te detalje rješava za Azure.

**Popis web-mjesta [mogućnosti]**

Ta se naredba popis aplikacija web.

    ~$ azure site list
    info:   Executing command site list
    data:   Name            State    Host names
    data:   --------------  -------  --------------------------------------------------
    data:   mongosite       Running  mongosite.antdf0.antares.windows.net
    data:   myphpsite       Running  myphpsite.antdf0.antares.windows.net
    data:   mydrupalsite36  Running  mydrupalsite36.antdf0.antares.windows.net
    info:   site list command OK

**Postavljanje web-mjesta [mogućnosti] [Ime]**

Ta se naredba postavlja mogućnosti konfiguracije za web-aplikacije [Ime]

    ~$ azure site set
    info:    Executing command site set
    Web site name: mydemosite
    + Getting sites
    + Updating site config information
    info:    site set command OK

**web-mjesta deploymentscript [mogućnosti]**

Ta se naredba generira implementaciju prilagođene skripte

    ~$ azure site deploymentscript --node
    info:    Executing command site deploymentscript
    info:    Generating deployment script for node.js Web Site
    info:    Generated deployment script files
    info:    site deploymentscript command OK

**[mogućnosti] [Ime] stvaranja web-mjesta**

Ta naredba stvara web app i lokalnog imenika.

    ~$ azure site create mysite
    info:   Executing command site create
    info:   Using location northeuropewebspace
    info:   Creating a new web site
    info:   Created web site at  mysite.antdf0.antares.windows.net
    info:   Initializing repository
    info:   Repository initialized
    info:   site create command OK

> [AZURE.NOTE] Naziv web-mjesta mora biti jedinstvena. Ne možete stvoriti web-mjesta s istim nazivom u DNS-a kao postojećeg web-mjesta.

**pregledavanje web-mjesta [odrednice] [Ime]**

Ta se naredba otvara web-aplikaciju programa u pregledniku.

    ~$ azure site browse mysite
    info:   Executing command site browse
    info:   Launching browser to http://mysite.antdf0.antares-test.windows-int.net
    info:   site browse command OK

**Prikaz web-mjesta [odrednice] [Ime]**

Ta se naredba prikazuje detalje za web-aplikacije.

    ~$ azure site show mysite
    info:   Executing command site show
    info:   Showing details for site
    data:   Site AdminEnabled true
    data:   Site HostNames mysite.antdf0.antares-test.windows-int.net
    data:   Site Name mysite
    data:   Site Owner 00060000814EDDEE
    data:   Site RepositorySiteName mysite
    data:   Site SelfLink https://s1.api.antdf0.antares.windows.net:454/subscriptions/444e62ff-4c5f-4116-a695-5c803ed584a5/webspaces/northeuropewebspace/sites/mysite
    data:   Site State Running
    data:   Site UsageState Normal
    data:   Site WebSpace northeuropewebspace
    data:   Config AppSettings
    data:   Config ConnectionStrings
    data:   Config DefaultDocuments 0=Default.htm, 1=Default.asp, 2=index.htm, 3=index.html, 4=iisstart.htm, 5=default.aspx, 6=index.php, 7=hostingstart.aspx
    data:   Config DetailedErrorLoggingEnabled false
    data:   Config HttpLoggingEnabled false
    data:   Config Metadata
    data:   Config NetFrameworkVersion v4.0
    data:   Config NumberOfWorkers 1
    data:   Config PhpVersion 5.3
    data:   Config PublishingPassword rJ}[Er2v[Y]q16B6vTD]n$[C2z}Z.pvgLfRcLnAp%ax]xstiLny};o@vmMAote@d
    data:   Config RequestTracingEnabled false
    data:   Repository https://mysite.scm.antdf0.antares-test.windows-int.net/
    info:   site show command OK

**Brisanje web-mjesta [odrednice] [Ime]**

Ta se naredba briše web-aplikacijama.

    ~$ azure site delete mysite
    info:   Executing command site delete
    info:   Deleting site mysite
    info:   Site mysite has been deleted
    info:   site delete command OK

 **Zamjena web-mjesta [odrednice] [Ime]**

Ta se naredba swaps dva slobodnih web app.

Ta se naredba podržava sljedeće dodatne mogućnosti:

**- pitanja ili **– quiet **: pitaj za potvrdu. Tu mogućnost koristite u automatiziranog skripti.


**Početak web-mjesta [odrednice] [Ime]**

Ta se naredba pokreće web-aplikacijama.

    ~$ azure site start mysite
    info:   Executing command site start
    info:   Starting site mysite
    info:   Site mysite has been started
    info:   site start command OK

**Zaustavi web-mjesta [odrednice] [Ime]**

Ta se naredba zaustavlja web-aplikacijama.

    ~$ azure site stop mysite
    info:   Executing command site stop
    info:   Stopping site mysite
    info:   Site mysite has been stopped
    info:   site stop command OK

**ponovno pokretanje web-mjesta [odrednice] [Ime]**

Naredba Zaustavi i pokreće navedeni web-aplikacijama.

Ta se naredba podržava sljedeće dodatne mogućnosti:

**– vremensko razdoblje** &lt;vremensko razdoblje >: naziv vremensko razdoblje da biste ponovno pokrenuli.


**Popis lokacija web-mjesta [mogućnosti]**

Ta se naredba popis web-mjesta za aplikacije.

    ~$ azure site location list
    info:    Executing command site location list
    + Getting locations
    data:    Name
    data:    ----------------
    data:    West Europe
    data:    West US
    data:    North Central US
    data:    North Europe
    data:    East Asia
    data:    East US
    info:    site location list command OK

###<a name="commands-to-manage-your-web-app-application-settings"></a>Naredbe za upravljanje postavke web-aplikacije aplikacije

**Popis web-mjesta appsetting [odrednice] [Ime]**

Ta se naredba popis postavka app dodati web-aplikaciji.

    ~$ azure site appsetting list
    info:    Executing command site appsetting list
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Name  Value
    data:    ----  -----
    data:    test  value
    info:    site appsetting list command OK

**web-mjesta appsetting dodavanje [mogućnosti] &lt;keyvaluepair > [Ime]**

Ta se naredba dodaje se postavka aplikacije na web-aplikaciju u paru vrijednost ključa.

    ~$ azure site appsetting add test=value
    info:    Executing command site appsetting add
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    + Updating site config information
    info:    site appsetting add command OK

**web-mjesta appsetting izbrišite [mogućnosti] &lt;ključ > [Ime]**

Ta se naredba briše postavku navedeni aplikacije iz web-aplikacije.

    ~$ azure site appsetting delete test
    info:    Executing command site appsetting delete
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    Delete application setting test? [y/n] y
    + Updating site config information
    info:    site appsetting delete command OK

**web-mjesta appsetting Pokaži [mogućnosti] &lt;ključ > [Ime]**

Ta naredba prikazuje detalje o postavku navedeni aplikacije

    ~$ azure site appsetting show test
    info:    Executing command site appsetting show
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Value:  value
    info:    site appsetting show command OK

###<a name="commands-to-manage-your-web-app-certificates"></a>Naredbe za upravljanje certifikatima web app

**Popis certifikata na web-mjesta [odrednice] [Ime]**

Ta naredba prikazuje popis certifikati o aplikaciju za web.

    ~$ azure site cert list
    info:    Executing command site cert list
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Subject                       Expiration Date                    Thumbprint
    data:    ----------------------------  -----------------------------------------
    ----------------  ----------------------------------------
    data:    *.msopentech.com              Fri Nov 28 2014 09:49:57 GMT-0800 (Pacific Standard Time)  A40E82D3DC0286D1F58650E570ECF8224F69A148
    data:    msopentech.azurewebsites.net  Fri Jun 19 2015 11:57:32 GMT-0700 (Pacific Daylight Time)  CE1CD6538852BF7A5DC32001C2E26A29B541F0E8
    info:    site cert list command OK

**web-mjesta certifikata dodavanje [mogućnosti] &lt;certifikata put > [Ime]**

**Brisanje certifikata na web-mjesta [mogućnosti] &lt;otisak prsta > [Ime]**

**Prikaz certifikata [mogućnosti] web-mjesta &lt;otisak prsta > [Ime]**

Ta se naredba prikazuje detalje certifikata

    ~$ azure site cert show CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    Executing command site cert show
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Certificate hostNames 0=msopentech.azurewebsites.net
    data:    Certificate expirationDate
    data:    Certificate friendlyName msopentech.azurewebsites.net
    data:    Certificate issueDate
    data:    Certificate issuer CN=MSIT Machine Auth CA 2, DC=redmond, DC=corp, DC=microsoft, DC=com
    data:    Certificate subjectName msopentech.azurewebsites.net
    data:    Certificate thumbprint CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    site cert show command OK

###<a name="commands-to-manage-your-web-app-connection-strings"></a>Naredbe za upravljanje nizovima web app veze

**Popis web-mjesta connectionstring [odrednice] [Ime]**

**connectionstring web-mjesta dodajte [mogućnosti] &lt;connectionname > &lt;vrijednost > &lt;vrsta > [Ime]**

**web-mjesta connectionstring izbrišite [mogućnosti] &lt;connectionname > [Ime]**

**web-mjesta connectionstring Pokaži [mogućnosti] &lt;connectionname > [Ime]**

###<a name="commands-to-manage-your-web-app-default-documents"></a>Naredbe za upravljanje web-aplikacije zadane dokumente

**Popis web-mjesta defaultdocument [odrednice] [Ime]**

**web-mjesta defaultdocument dodavanje [mogućnosti] &lt;dokument > [Ime]**

**web-mjesta defaultdocument izbrišite [mogućnosti] &lt;dokument > [Ime]**

###<a name="commands-to-manage-your-web-app-deployments"></a>Naredbe za upravljanje implementacijama aplikacije na webu

**popis za implementaciju web-mjesta [odrednice] [Ime]**

**Implementacija web-mjesta prikazuje [mogućnosti] &lt;commitId > [Ime]**

**redeploy implementaciju web-mjesta [mogućnosti] &lt;commitId > [Ime]**

**github implementaciju web-mjesta [odrednice] [Ime]**

**Postavljanje korisnika za implementaciju web-mjesta [mogućnosti] [username] [lozinka]**

###<a name="commands-to-manage-your-web-app-domains"></a>Naredbe za upravljanje domenama web app

**popis domena web-mjesta [odrednice] [Ime]**

**[mogućnosti] dodavanja domene web-mjesta &lt;dn > [Ime]**

**Brisanje domene web-mjesta [mogućnosti] &lt;dn > [Ime]**

###<a name="commands-to-manage-your-web-app-handler-mappings"></a>Naredbe za upravljanje mapiranja rukovatelj web app

**Popis web-mjesta rukovatelj [odrednice] [Ime]**

**Rukovatelj web-mjesta dodajte [mogućnosti] &lt;proširenje > &lt;procesor > [Ime]**

**Brisanje web-mjesta rukovatelj [mogućnosti] &lt;proširenje > [Ime]**

###<a name="commands-to-manage-your-web-jobs"></a>Naredbe za upravljanje zadacima na webu

**Popis web-mjesta posla [odrednice] [Ime]**

Ta se naredba popis svih zadataka za web u odjeljku web-aplikacijama.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– Vrsta posla** &lt;vrsta posla >: nije obavezno. Vrsta na webjob. Valjana vrijednost je "okidačima" ili "neprekinuti". Prema zadanim postavkama vraćaju webjobs svih vrsta.
+ **– vremensko razdoblje** &lt;vremensko razdoblje >: naziv vremensko razdoblje da biste ponovno pokrenuli.

**web-mjesta posao Pokaži [mogućnosti] &lt;jobName > &lt;Vrsta zadatka > [Ime]**

Ta se naredba prikazuje detalje posla određene web.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– naziv zadatka** &lt;naziv zadatka >: potrebna. Naziv u webjob.
+ **– Vrsta posla** &lt;vrsta posla >: potrebna. Vrsta na webjob. Valjana vrijednost je "okidačima" ili "neprekinuti".
+ **– vremensko razdoblje** &lt;vremensko razdoblje >: naziv vremensko razdoblje da biste ponovno pokrenuli.

**Brisanje web-mjesta posao [mogućnosti] &lt;jobName > &lt;Vrsta zadatka > [Ime]**

Ta se naredba briše posao navedeno web-mjesto.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– naziv zadatka** &lt;naziv zadatka > potrebna. Naziv u webjob.
+ **– Vrsta posla** &lt;vrsta posla > potrebna. Vrsta na webjob. Valjana vrijednost je "okidačima" ili "neprekinuti".
+ **-q** ili **– Tihi**: pitaj za potvrdu. Tu mogućnost koristite u automatiziranog skripti.
+ **– vremensko razdoblje** &lt;vremensko razdoblje >: naziv vremensko razdoblje da biste ponovno pokrenuli.

**Prijenos posla web-mjesta [mogućnosti] &lt;jobName > &lt;Vrsta zadatka > <jobFile> [Ime]**

Ta se naredba briše posao navedeno web-mjesto.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– naziv zadatka** &lt;naziv zadatka >: potrebna. Naziv u webjob.
+ **– Vrsta posla** &lt;vrsta posla >: potrebna. Vrsta na webjob. Valjana vrijednost je "okidačima" ili "neprekinuti".
+ **– datoteka posla** &lt;datoteka posla >: potrebna. Datoteka posla.
+ **– vremensko razdoblje** &lt;vremensko razdoblje >: naziv vremensko razdoblje da biste ponovno pokrenuli.

**web-mjesta posao start [mogućnosti] &lt;jobName > &lt;Vrsta zadatka > [Ime]**

Ta se naredba pokreće posao navedeno web-mjesto.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– naziv zadatka** &lt;naziv zadatka >: potrebna. Naziv u webjob.
+ **– Vrsta posla** &lt;vrsta posla >: potrebna. Vrsta na webjob. Valjana vrijednost je "okidačima" ili "neprekinuti".
+ **– vremensko razdoblje** &lt;vremensko razdoblje >: naziv vremensko razdoblje da biste ponovno pokrenuli.

**web-mjesta posao Zaustavi [mogućnosti] &lt;jobName > &lt;Vrsta zadatka > [Ime]**

Ta se naredba zaustavlja posao navedeno web-mjesto. Samo neprekinuti poslove može se zaustaviti.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– naziv zadatka** &lt;naziv zadatka >: potrebna. Naziv u webjob.
+ **– vremensko razdoblje** &lt;vremensko razdoblje >: naziv vremensko razdoblje da biste ponovno pokrenuli.

###<a name="commands-to-manage-your-web-jobs-history"></a>Naredbe za upravljanje zadacima povijesti Web

**Popis povijesti zadatka web-mjesta [odrednice] [jobName] [Ime]**

Ta naredba prikazuje povijest pokreće posao navedeno web-mjesto.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– naziv zadatka** &lt;naziv zadatka >: potrebna. Naziv u webjob.
+ **– vremensko razdoblje** &lt;vremensko razdoblje >: naziv vremensko razdoblje da biste ponovno pokrenuli.

**Prikaži Povijest zadatka web-mjesta [mogućnosti] [jobName] [runId] [Ime]**

Ta se naredba može vidjeti detalje o zadatka za posao navedeno web-mjesto.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– naziv zadatka** &lt;naziv zadatka >: potrebna. Naziv u webjob.
+ **– pokretanje id** &lt;Pokreni id >: nije obavezno. Id izvođenja povijest. Ako nije naveden, prikažite najnoviji izvođenja.
+ **– vremensko razdoblje** &lt;vremensko razdoblje >: naziv vremensko razdoblje da biste ponovno pokrenuli.

###<a name="commands-to-manage-your-web-app-diagnostics"></a>Naredbe za upravljanje vaše Dijagnostika web app

**zapisnik web-mjestu preuzmite [odrednice] [Ime]**

Preuzmite .zip datoteku koja sadrži Dijagnostika web app.

    ~$ azure site log download
    info:    Executing command site log download
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    + Downloading diagnostic log to diagnostics.zip
    info:    site log download command OK

**web-mjesta zapisnika krakom [odrednice] [Ime]**

Ta se naredba povezuje vaše terminal servis za strujanje za zapisnika.

    ~$ azure site log tail
    info:    Executing command site log tail
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    2013-11-19T17:24:17  Welcome, you are now connected to log-streaming service.

**Postavljanje web-mjesta zapisnika [mogućnosti] [Ime]**

Ta se naredba konfigurira dijagnostičkih mogućnosti za web-aplikacije.

    ~$ azure site log set -a
    info:    Executing command site log set
    + Getting output options
    help:    Output:
      1) file
      2) storage
      : 1
    Web site name: mydemosite
    + Getting locations
    + Getting sites
    + Getting site information
    + Getting diagnostic settings
    + Updating diagnostic settings
    info:    site log set command OK

###<a name="commands-to-manage-your-web-app-repositories"></a>Naredbe za upravljanje sustava spremištima web app

**web-mjesta spremišta podružnice [mogućnosti] &lt;podružnice > [Ime]**

**Brisanje web-mjesta spremišta [odrednice] [Ime]**

**Sinkronizacija web-mjesta spremišta [odrednice] [Ime]**

###<a name="commands-to-manage-your-web-app-scaling"></a>Naredbe za upravljanje skaliranje web app

**način skaliranje web-mjesta [mogućnosti] &lt;način > [Ime]**

**Promjena veličine web-mjesta instance [mogućnosti] &lt;instance > [Ime]**


## <a name="commands-to-manage-azure-mobile-services"></a>Naredbe za upravljanje uslugama za mobilne Azure

Azure mobilne usluge objedinjuje skup Azure servise koji omogućuju pozadinskog mogućnosti za aplikacije. Mobilna usluga naredbe su podijeljeni sljedećih kategorija:

+ [Naredbe za upravljanje instance servis za mobilne uređaje](#Mobile_Services)
+ [Naredbe za upravljanje konfiguracijom servis za mobilne uređaje](#Mobile_Configuration)
+ [Naredbe za upravljanje tablice servis za mobilne uređaje](#Mobile_Tables)
+ [Naredbe za upravljanje skripte servis za mobilne uređaje](#Mobile_Scripts)
+ [Naredbe za upravljanje Zakazani zadaci](#Mobile_Jobs)
+ [Naredbe za promjenu veličine servis za mobilne uređaje](#Mobile_Scale)

Većina naredbi mobilne usluge primjenjuju se sljedeće mogućnosti:

+ **-h** ili **– Pomoć**: prikaz izlaz informacije o korištenju.
+ **-s `<id>` ** ili **– pretplate `<id>` **: korištenje određene pretplatu, naveden kao `<id>`.
+ **-v** ili **– opširno**: pisanje opširno izlaz.
+ **– json**: pisanje JSON izlaz.

### <a name="Mobile_Services"></a>Naredbe za upravljanje instance servis za mobilne uređaje

**Mobilni mjesta [mogućnosti]**

Ta se naredba popis zemljopisna mjesta podržava mobilne usluge.

    ~$ azure mobile locations
    info:    Executing command mobile locations
    info:    East US (default)
    info:    West US
    info:    North Europe

**Mobilni stvaranje [odrednice] [naziv servisa] [sqlAdminUsername] [sqlAdminPassword]**

Ta naredba stvara servis za mobilne uređaje zajedno s bazom podataka SQL i poslužitelja.

    ~$ azure mobile create todolist your_login_name Secure$Password
    info:    Executing command mobile create
    + Creating mobile service
    info:    Overall application state: Healthy
    info:    Mobile service (todolist) state: ProvisionConfigured
    info:    SQL database (todolist_db) state: Provisioned
    info:    SQL server (e96ean1c6v) state: ProvisionConfigured
    info:    mobile create command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **- r `<sqlServer>` ** ili **– sqlServer `<sqlServer>` **: korištenje postojećeg baze podataka SQL poslužitelja, naveden kao `<sqlServer>`.
+ **-d `<sqlDb>` ** ili **– sqlDb `<sqlDb>` **: korištenje postojeće baze podataka SQL, naveden kao `<sqlDb>`.
+ **-l `<location>` ** ili **– mjesto `<location>` **: Stvaranje usluge na određenom mjestu, naveden kao `<location>`. Pokrenite azure mobilne mjesta da biste dobili dostupna mjesta.
+ **– sqlLocation `<location>` **: Stvaranje SQL server u određenom `<location>`; po zadanom je mjesto servis za mobilne uređaje.

**Brisanje mobilnog [odrednice] [naziv servisa]**

Ta se naredba briše servis za mobilne uređaje i njegova SQL baze podataka i poslužitelja.

    ~$ azure mobile delete todolist -a -q
    info:    Executing command mobile delete
    data:    Mobile service todolist
    data:    SQL database todolistAwrhcL60azo1C401
    data:    SQL server fh1kvbc7la
    + Deleting mobile service
    info:    Deleted mobile service
    + Deleting SQL server
    info:    Deleted SQL server
    + Deleting mobile application
    info:    Deleted mobile application
    info:    mobile delete command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **d** ili **– deleteData**: brisanje svih podataka s ovaj servis za mobilne uređaje iz baze podataka.
+ **-a** ili **– deleteAll**: brisanje SQL baze podataka i poslužitelja.
+ **-q** ili **– Tihi**: pitaj za potvrdu. Tu mogućnost koristite u automatiziranog skripti.

**mobilnog popisa [mogućnosti]**

Ta se naredba popis mobilnih usluga.

    ~$ azure mobile list
    info:    Executing command mobile list
    data:    Name          State  URL
    data:    ------------  -----  --------------------------------------
    data:    todolist      Ready  https://todolist.azure-mobile.net/
    data:    mymobileapp   Ready  https://mymobileapp.azure-mobile.net/
    info:    mobile list command OK

**Mobilni prikaz [odrednice] [naziv servisa]**

Ta se naredba prikazuje detalje o servis za mobilne uređaje.

    ~$ azure mobile show todolist
    info:    Executing command mobile show
    + Getting information
    info:    Mobile application
    data:    status Healthy
    data:    Mobile service name todolist
    data:    Mobile service status ProvisionConfigured
    data:    SQL database name todolistAwrhcL60azo1C401
    data:    SQL database status Linked
    data:    SQL server name fh1kvbc7la
    data:    SQL server status Linked
    info:    Mobile service
    data:    name todolist
    data:    state Ready
    data:    applicationUrl https://todolist.azure-mobile.net/
    data:    applicationKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    masterKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    webspace WESTUSWEBSPACE
    data:    region West US
    data:    tables TodoItem
    info:    mobile show command OK

**Mobilni ponovno pokretanje [odrednice] [naziv servisa]**

Ta se naredba pokreće servis za mobilne uređaje instance.

    ~$ azure mobile restart todolist
    info:    Executing command mobile restart
    + Restarting mobile service
    info:    Service was restarted.
    info:    mobile restart command OK

**Mobilni zapisnika [odrednice] [naziv servisa]**

Ta naredba Vrati zapisnike mobilnu uslugu, filtrira sve vrste zapisnik, ali `error`.

    ~$ azure mobile log todolist -t error
    info:    Executing command mobile log
    data:
    data:    timeCreated 2013-01-07T16:04:43.351Z
    data:    type error
    data:    source /scheduler/TestingLogs.js
    data:    message This is an error.
    data:
    info:    mobile log command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **- r `<query>` ** ili **– upita `<query>` **: izvršava navedeni zapisnika upita.
+ **-t `<type>` ** ili **– Vrsta `<type>` **: filtrirati zapisnike vraćene stavke `<type>`, koja može biti `information`, `warning`, ili `error`.
+ **– k `<skip>` ** ili **– preskakati `<skip>` **: preskače broj redaka određen `<skip>`.
+ **-p `<top>` ** ili **– vrha `<top>` **: vraća određeni broj redaka, određen `<top>`.

> [AZURE.NOTE] Na **– upita** parametar ima prednost pred **– Vrsta**, **– preskočiti**, i **– vrha**.

**Mobilni oporavak [odrednice] [unhealthyservicename] [healthyservicename]**

Ta se naredba oporavlja dobro mobilnu uslugu tako da je premjestite dobar servis za mobilne uređaje u nekoj drugoj regiji.

Ta se naredba podržava sljedeće dodatne mogućnosti:

**-q** ili **– Tihi**: izostavi okvir Pitaj za potvrdu oporavak.

**Mobilni ključ Obnovi [odrednice] [naziv servisa] [Vrsta]**

Ta se naredba obnavlja tipku za aplikacije servis za mobilne uređaje.

    ~$ azure mobile key regenerate todolist application
    info:    Executing command mobile key regenerate
    info:    New application key is SmLorAWVfslMcOKWSsuJvuzdJkfUpt40
    info:    mobile key regenerate command OK

Ključni vrste su `master` i `application`.

> [AZURE.NOTE] Kada Obnovi tipke, klijenti koje koriste stari ključ možda neće moći pristupiti servis za mobilne uređaje. Kada Obnovi tipku za aplikacije, trebali biste ažurirati aplikaciju s novom ključa vrijednosti.

**Postavljanje mobilnih ključ [mogućnosti] [naziv servisa] [Vrsta] [vrijednost]**

Ta se naredba postavlja tipku servis za mobilne uređaje na određenu vrijednost.


### <a name="Mobile_Configuration"></a>Naredbe za upravljanje konfiguracijom servis za mobilne uređaje

**Popis mobilne konfiguracije [odrednice] [naziv servisa]**

Ta se naredba popis mogućnosti konfiguracije za servis za mobilne uređaje.

    ~$ azure mobile config list todolist
    info:    Executing command mobile config list
    + Getting mobile service configuration
    data:    dynamicSchemaEnabled true
    data:    microsoftAccountClientSecret Not configured
    data:    microsoftAccountClientId Not configured
    data:    microsoftAccountPackageSID Not configured
    data:    facebookClientId Not configured
    data:    facebookClientSecret Not configured
    data:    twitterClientId Not configured
    data:    twitterClientSecret Not configured
    data:    googleClientId Not configured
    data:    googleClientSecret Not configured
    data:    apnsMode none
    data:    apnsPassword Not configured
    data:    apnsCertifcate Not configured
    info:    mobile config list command OK

**Mobilni config dobiti [odrednice] [naziv servisa] [tipka]**

Ta se naredba može vidjeti određenu konfiguraciju mogućnost za mobilne usluge u tom slučaju dinamički sheme.

    ~$ azure mobile config get todolist dynamicSchemaEnabled
    info:    Executing command mobile config get
    data:    dynamicSchemaEnabled true
    info:    mobile config get command OK

**Postavljanje mobilnih config [mogućnosti] [naziv servisa] [tipka] [vrijednost]**

Ta se naredba postavlja mogućnost određeni konfiguracije za mobilne usluge u tom slučaju dinamički shemu.

    ~$ azure mobile config set todolist dynamicSchemaEnabled false
    info:    Executing command mobile config set
    info:    mobile config set command OK


### <a name="Mobile_Tables"></a>Naredbe za upravljanje tablice servis za mobilne uređaje

**Popis mobilne tablica [odrednice] [naziv servisa]**

U ovom command dohvaća sve tablice u servis za mobilne uređaje.

    ~$azure mobile table list todolist
    info:    Executing command mobile table list
    data:    Name      Indexes  Rows
    data:    --------  -------  ----
    data:    Channel   1        0
    data:    TodoItem  1        0
    info:    mobile table list command OK

**Mobilni tablica prikazuje [mogućnosti] [naziv servisa] [tablename]**

Ta se naredba prikazuje vraća detalje o određenim tablice.

    ~$azure mobile table show todolist
    info:    Executing command mobile table show
    + Getting table information
    info:    Table statistics:
    data:    Number of records 5
    info:    Table operations:
    data:    Operation  Script       Permissions
    data:    ---------  -----------  -----------
    data:    insert     1900 bytes   user
    data:    read       Not defined  user
    data:    update     Not defined  user
    data:    delete     Not defined  user
    info:    Table columns:
    data:    Name  Type           Indexed
    data:    ----  -------------  -------
    data:    id    bigint(MSSQL)  Yes
    data:    text      string
    data:    complete  boolean
    info:    mobile table show command OK

**Mobilni tablice stvaranje [odrednice] [naziv servisa] [tablename]**

Ta se naredba stvara tablicu.

    ~$azure mobile table create todolist Channels
    info:    Executing command mobile table create
    + Creating table
    info:    mobile table create command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **-p `&lt;permissions>` ** ili **– dozvole `&lt;permissions>` **: razdvojen zarezom popis `<operation>` = `<permission>` parove, gdje `<operation>` je `insert`, `read`, `update`, ili `delete` i `&lt;permissions>` je `public`, `application` (zadano), `user`, ili `admin`.

**Mobilni podaci pročitati [odrednice] [naziv servisa] [tablename] [query]**

Ta se naredba čita podatke iz tablice.

    ~$azure mobile data read todolist TodoItem
    info:    Executing command mobile data read
    data:    id  text     complete
    data:    --  -------  --------
    data:    1   item #1  false
    data:    2   item #2  true
    data:    3   item #3  false
    data:    4   item #4  true
    info:    mobile data read command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– k `<skip>` ** ili **– preskočiti `<skip>` **: preskače broj redaka određen `<skip>`.
+ **-t `<top>` ** ili **– vrha `<top>` **: vraća određeni broj redaka, određen `<top>`.
+ **-l** ili **– popis**: vraća podatke u obliku popisa.

**Ažuriranje mobilne tablice [odrednice] [naziv servisa] [tablename]**

Ta se naredba Izbriši dozvole za tablicu mijenja se u samo za administratore.

    ~$azure mobile table update todolist Channels -p delete=admin
    info:    Executing command mobile table update
    + Updating permissions
    info:    Updated permissions
    info:    mobile table update command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **-p `&lt;permissions>` ** ili **– dozvole `&lt;permissions>` **: razdvojen zarezom popis `<operation>` = `<permission>` parove, gdje `<operation>` je `insert`, `read`, `update`, ili `delete` i `&lt;permissions>` je `public`, `application` (zadano), `user`, ili `admin`.
+ **– deleteColumn `<columns>` **: razdvojen zarezom popis stupaca da biste izbrisali, kao `<columns>`.
+ **-q** ili **– Tihi**: brisanje stupaca bez postavljanja upita za potvrdu.
+ **– addIndex `<columns>` **: razdvojen zarezom popis stupce koje želite uključiti u indeks.
+ **– deleteIndex `<columns>` **: razdvojen zarezom popis stupce koje želite izuzeti iz indeksa.

**Mobilni tablice izbrišite [mogućnosti] [naziv servisa] [tablename]**

Ta se naredba briše tablice.

    ~$azure mobile table delete todolist Channels
    info:    Executing command mobile table delete
    Do you really want to delete the table (yes/no): yes
    + Deleting table
    info:    mobile table delete command OK

Navedite parametar - pitanja da biste izbrisali tablicu bez potvrde. Učinite sljedeće da biste spriječili blokiranje Automatizacija skripti.

**Mobilni podaci skratiti [odrednice] [naziv servisa] [tablename]**

Ta se naredba uklanja sve retke podataka iz tablice.

    ~$azure mobile data truncate todolist TodoItem
    info:    Executing command mobile data truncate
    info:    There are 7 data rows in the table.
    Do you really want to delete all data from the table? (y/n): y
    info:    Deleted 7 rows.
    info:    mobile data truncate command OK


### <a name="Mobile_Scripts"></a>Naredbe za upravljanje skripti

Naredbe u ovom odjeljku se koriste za upravljanje poslužitelja skripte koje pripadaju servis za mobilne uređaje. Dodatne informacije potražite u članku [Rad s poslužiteljem skripte u mobilne usluge](https://github.com/Azure/azure-mobile-services/blob/master/docs/mobile-services-how-to-use-server-scripts.md).

**Popis mobilne skripte [odrednice] [naziv servisa]**

Ta se naredba Popis registriranih skripte, uključujući tablice i raspored skripte.

    ~$azure mobile script list todolist
    info:    Executing command mobile script list
    + Getting script information
    info:    Table scripts
    data:    Name                   Size
    data:    ---------------------  ----
    data:    table/TodoItem.delete  256
    data:    table/Devices.insert   1660
    error:   Unable to get shared scripts
    info:    Scheduler scripts
    data:    Name                 Status     Interval   Last run   Next run
    data:    -------------------  ---------  ---------  ---------  ---------
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    info:    mobile script list command OK

**Preuzimanje mobilne skripte [odrednice] [naziv servisa] [scriptname]**

Ta se naredba preuzima skripte Umetanje iz tablice TodoItem datoteku pod nazivom `todoitem.insert.js` u na `table` podmape.

    ~$azure mobile script download todolist table/todoitem.insert.js
    info:    Executing command mobile script download
    info:    Saved script to ./table/todoitem.insert.js
    info:    mobile script download command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **-p `<path>` ** ili **– put `<path>` **: mjesto u tekstu u koju želite spremiti skriptu, gdje se nalazi na trenutnom radnom direktoriju zadani.
+ **-f `<file>` ** ili **– datoteka `<file>` **: naziv datoteke u koju želite spremiti skriptu.
+ **-o** ili **– nadjačati**: Prebriši postojeće datoteke.
+ **-c** ili **– konzole**: napisati skriptu konzole umjesto u datoteku.

**prijenos mobilne skripte [odrednice] [naziv servisa] [scriptname]**

Ta se naredba prenosi skriptu pod nazivom `todoitem.insert.js` iz na `table` podmape.

    ~$azure mobile script upload todolist table/todoitem.insert.js
    info:    Executing command mobile script upload
    info:    mobile script upload command OK

Naziv datoteke mora se sastoji od naziva tablica i postupak. Mora biti smještena u podmapu tablice u odnosu na mjesto na kojem se izvršava naredbu. Možete koristiti u **-f `<file>` ** ili **– datoteka `<file>` ** parametar da biste naveli drugi naziv datoteke i put do datoteke koja sadrži skriptu za registraciju.


**Brisanje mobilnog skripte [odrednice] [naziv servisa] [scriptname]**

Ta se naredba uklanja postojeće skripte Umetanje iz tablice TodoItem.

    ~$azure mobile script delete todolist table/todoitem.insert.js
    info:    Executing command mobile script delete
    info:    mobile script delete command OK

### <a name="Mobile_Jobs"></a>Naredbe za upravljanje Zakazani zadaci

Naredbe u ovom odjeljku se koriste za upravljanje Zakazani zadaci koji pripadaju servis za mobilne uređaje. Dodatne informacije potražite u članku [zakazivanje zadataka](http://msdn.microsoft.com/library/windowsazure/jj860528.aspx).

**Popis mobilne posla [odrednice] [naziv servisa]**

Ta se naredba popis Zakazani zadaci.

    ~$azure mobile job list todolist
    info:    Executing command mobile job list
    info:    Scheduled jobs
    data:    Job name    Script name           Status    Interval     Last run              Next run
    data:    ----------  --------------------  --------  -----------  --------------------  --------------------
    data:    getUpdates  scheduler/getUpdates  enabled   15 [minute]  2013-01-14T16:15:00Z  2013-01-14T16:30:00Z
    info:    You can manipulate scheduled job scripts using the 'azure mobile script' command.
    info:    mobile job list command OK

**Stvaranje mobilnog zadatka [odrednice] [naziv servisa] [jobname]**

Ta naredba stvara zadatak pod nazivom `getUpdates` koji je zakazano izvođenje zaračunava.

    ~$azure mobile job create -i 1 -u hour todolist getUpdates
    info:    Executing command mobile job create
    info:    Job was created in disabled state. You can enable the job using the 'azure mobile job update' command.
    info:    You can manipulate the scheduled job script using the 'azure mobile script' command.
    info:    mobile job create command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **-i `<number>` ** ili **– interval `<number>` **: intervala posla, kao i cijeli broj. Zadana vrijednost je `15`.
+ **-u `<unit>` ** ili **– intervalUnit `<unit>` **: jedinica za _interval_, što može biti jedna od sljedećih vrijednosti:
    + **minute** (zadano)
    + **sat**
    + **dan**
    + **mjesec**
    + **ništa** (osvježavati zadataka)
+ **-t `<time>`** **– startTime `<time>` ** Početno vrijeme prvog pokretanja za skripte u obliku ISO. Zadana vrijednost je `now`.

> [AZURE.NOTE] Novi zadaci stvaraju se u stanje onemogućenosti jer skriptu morate i dalje mogu prenijeti. Pomoću naredbe **Prijenos mobilne skriptu** da biste prenijeli skripte i naredbu **mobilnih poslova ažuriranje** da biste omogućili posao.

**Ažuriranje mobilne posao [odrednice] [naziv servisa] [jobname]**

Sljedeća naredba omogućuje na onemogućeno `getUpdates` posao.

    ~$azure mobile job update -a enabled todolist getUpdates
    info:    Executing command mobile job update
    info:    mobile job update command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **-i `<number>` ** ili **– interval `<number>` **: intervala posla, kao i cijeli broj. Zadana vrijednost je `15`.
+ **-u `<unit>` ** ili **– intervalUnit `<unit>` **: jedinica za _interval_, što može biti jedna od sljedećih vrijednosti:
    + **minute** (zadano)
    + **sat**
    + **dan**
    + **mjesec**
    + **ništa** (osvježavati poslova)
+ **-t `<time>`** **– startTime `<time>` ** Početno vrijeme prvog pokretanja za skripte u obliku ISO. Zadana vrijednost je `now`.
+ **- `<status>` ** ili **– status `<status>` **: statusa posla, što može biti nešto od sljedećeg `enabled` ili `disabled`.

**Brisanje mobilnog posao [odrednice] [naziv servisa] [jobname]**

Ta se naredba uklanja zakazani posao getUpdates s poslužitelja TodoList.

    ~$azure mobile job delete todolist getUpdates
    info:    Executing command mobile job delete
    info:    mobile job delete command OK

> [AZURE.NOTE] Brisanje posla briše i prenesene skripte.

### <a name="Mobile_Scale"></a>Naredbe za promjenu veličine servis za mobilne uređaje

Naredbe u ovom odjeljku koriste se za promjenu veličine servis za mobilne uređaje. Dodatne informacije potražite u članku [Skaliranje servis za mobilne uređaje](http://msdn.microsoft.com/library/windowsazure/jj193178.aspx).

**Mobilni skaliranje Pokaži [mogućnosti] [naziv servisa]**

Ta naredba prikazuje informacije o mjerilo, uključujući trenutni način računalnim i broj instanci.

    ~$azure mobile scale show todolist
    info:    Executing command mobile scale show
    data:    webspace WESTUSWEBSPACE
    data:    computeMode Free
    data:    numberOfInstances 1
    info:    mobile scale show command OK

**Mobilni skaliranje promijenite [mogućnosti] [naziv servisa]**

Tu naredbu Promjena skale servis za mobilne uređaje s besplatno premium način za.

    ~$azure mobile scale change -c Reserved -i 1 todolist
    info:    Executing command mobile scale change
    + Rescaling the mobile service
    info:    mobile scale change command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **- c `<mode>` ** ili **– computeMode `<mode>` **: način računalnim mora biti `Free` ili `Reserved`.
+ **-i `<count>` ** ili **– numberOfInstances `<count>` **: broj instanci koristiti kada se pokrene u načinu rada za rezervirana.

> [AZURE.NOTE] Kada postavite računalnim način `Reserved`, sve mobilne usluge na istom području u premium način rada.


###<a name="commands-to-enable-preview-features-for-your-mobile-service"></a>Naredbe za omogućivanje značajki pregleda za mobilnu uslugu

**Popis mobilne pretpregled [odrednice] [naziv servisa]**

Ta naredba prikazuje pregled značajki dostupnih na navedeni servis i je li su te značajke omogućene.

    ~$ azure mobile preview list mysite
    info:    Executing command mobile preview list
    + Getting preview features
    data:    Preview feature  Enabled
    data:    ---------------  -------
    data:    SourceControl    No
    data:    Users            No
    info:    You can enable preview features using the 'azure mobile preview enable' command.
    info:    mobile preview list command OK

**Mobilni pretpregled Omogućivanje [mogućnosti] [naziv servisa] [featurename]**

Ta se naredba omogućuje značajku navedeni pretpregled za servis za mobilne uređaje. Kada je omogućen, ne može se onemogućiti značajke za servis za mobilne uređaje.

###<a name="commands-to-manage-your-mobile-service-apis"></a>Naredbe za upravljanje mobilne usluge API-ji

**api mobilnog popisa [odrednice] [naziv servisa]**

Ta naredba prikazuje popis mobilnih usluga prilagođene API-ji koji ste stvorili za servis za mobilne uređaje.

    ~$ azure mobile api list mysite
    info:    Executing command mobile api list
    + Retrieving list of APIs
    info:    APIs
    data:    Name                  Get          Put          Post         Patch        Delete
    data:    --------------------  -----------  -----------  -----------  -----------  -----------
    data:    myCustomRetrieveAPI   application  application  application  application  application
    info:    You can manipulate API scripts using the 'azure mobile script' command.
    info:    mobile api list command OK

**Mobilni api stvaranje [odrednice] [naziv servisa] [apiname]**

Stvara prilagođeni API-JA servis za mobilne uređaje

    ~$ azure mobile api create mysite myCustomRetrieveAPI
    info:    Executing command mobile api create
    + Creating custom API: 'myCustomRetrieveAPI'
    info:    API was created successfully. You can modify the API using the 'azure mobile script' command.
    info:    mobile api create command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

**-p** ili **– dozvole** &lt;dozvole >: popis razdvojen zarezom &lt;način > =&lt;dozvole > parove.

**Mobilni api ažuriranje [odrednice] [naziv servisa] [apiname]**

Ta se naredba ažurira API prilagođene navedeni servis za mobilne uređaje.

Ta se naredba podržava sljedeće dodatne mogućnosti:

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **-p** ili **– dozvole** &lt;dozvole >: popis razdvojen zarezom &lt;način > =&lt;dozvole > parove.
+ **-f** ili **– prisilno**: nadjačava sve prilagođene promjene metapodataka datoteka dozvole.

**Brisanje mobilnog api [odrednice] [naziv servisa] [apiname]**

    ~$ azure mobile api delete mysite myCustomRetrieveAPI
    info:    Executing command mobile api delete
    + Deleting API: 'myCustomRetrieveAPI'
    info:    mobile api delete command OK

Ta se naredba briše API prilagođene navedeni servis za mobilne uređaje.

###<a name="commands-to-manage-your-mobile-application-app-settings"></a>Naredbe za upravljanje postavkama aplikacije za mobilne aplikacije

**Mobilni appsetting popis [odrednice] [naziv servisa]**

Ta naredba prikazuje postavki aplikacije za mobilne aplikacije servisa za navedeni.

    ~$ azure mobile appsetting list mysite
    info:    Executing command mobile appsetting list
    + Retrieving app settings
    data:    Name               Value
    data:    -----------------  -----
    data:    enablebetacontent  true
    info:    mobile appsetting list command OK

**Mobilni appsetting dodavanje [odrednice] [naziv servisa] [Ime] [vrijednost]**

Ta se naredba dodaje postavku prilagođene aplikacije za servis za mobilne uređaje.

    ~$ azure mobile appsetting add mysite enablebetacontent true
    info:    Executing command mobile appsetting add
    + Retrieving app settings
    + Adding app setting
    info:    mobile appsetting add command OK

**Brisanje mobilnog appsetting [odrednice] [naziv servisa] [Ime]**

Ta se naredba uklanja postavku određenu aplikaciju za servis za mobilne uređaje.

    ~$ azure mobile appsetting delete mysite enablebetacontent
    info:    Executing command mobile appsetting delete
    + Retrieving app settings
    + Removing app setting 'enablebetacontent'
    info:    mobile appsetting delete command OK

**Mobilni appsetting Pokaži [mogućnosti] [naziv servisa] [Ime]**

Ta se naredba uklanja postavku određenu aplikaciju za servis za mobilne uređaje.

    ~$ azure mobile appsetting show mysite enablebetacontent
    info:    Executing command mobile appsetting show
    + Retrieving app settings
    info:    enablebetacontent: true
    info:    mobile appsetting show command OK

## <a name="manage-tool-local-settings"></a>Upravljanje postavkama za lokalni alata

Lokalne postavke su ID pretplate i zadani naziv računa za pohranu.

**Popis konfiguracije [mogućnosti]**

Ta se naredba prikazuje konfiguracijske postavke.

    ~$ azure config list
    info:   Displaying config settings
    data:   Setting                Value
    data:   ---------------------  ------------------------------------
    data:   subscription           32-digit-subscription-key
    data:   defaultStorageAccount  name

**Postavljanje config [mogućnosti] &lt;naziv&gt;,&lt;vrijednost&gt;**

Ta se naredba mijenja konfiguracijske postavke.

    ~$ azure config set defaultStorageAccount myname
    info:   Setting 'defaultStorageAccount' to value 'myname'
    info:   Changes saved.

## <a name="commands-to-manage-service-bus"></a>Naredbe za upravljanje Bus servisa

Pomoću ove naredbe upravljati računom Bus servisa

**Potvrdite polja naziva SB [mogućnosti] &lt;naziv >**

Provjera je li servis bus prostor naziva pravne i dostupna.

**Stvaranje prostora za naziv SB &lt;naziv > &lt;mjesto >**

Stvara prostor naziva Bus servisa.

    ~$ azure sb namespace create mysbnamespacea-test "West US"
    info:    Executing command sb namespace create
    + Creating namespace mysbnamespacea-test in region West US
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Activating
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    _: [object Object]
    info:    sb namespace create command OK


**Brisanje polja naziva SB &lt;naziv >**

Uklanjanje prostora za naziv.

    ~$ azure sb namespace delete mysbnamespacea-test
    info:    Executing command sb namespace delete
    Delete namespace mysbnamespacea-test? [y/n] y
    + Deleting namespace mysbnamespacea-test
    info:    sb namespace delete command OK

**Popis SB prostor naziva**

Popis svi se prostori naziva stvorene za vaš račun.

    ~$ azure sb namespace list
    info:    Executing command sb namespace list
    + Getting namespaces
    data:    Name                 Region   Status
    data:    -------------------  -------  ------
    data:    mysbnamespacea-test  West US  Active
    info:    sb namespace list command OK


**popis za mjesto SB prostor naziva**

Prikaz popisa sva mjesta dostupan prostor za naziv.

    ~$ azure sb namespace location list
    info:    Executing command sb namespace location list
    + Getting locations
    data:    Name              Code
    data:    ----------------  ----------------
    data:    East Asia         East Asia
    data:    West Europe       West Europe
    data:    North Europe      North Europe
    data:    East US           East US
    data:    Southeast Asia    Southeast Asia
    data:    North Central US  North Central US
    data:    West US           West US
    data:    South Central US  South Central US
    info:    sb namespace location list command OK

**Pokaži polje naziva SB &lt;naziv >**

Prikaz detalja o određenim prostora za naziv.

    ~$ azure sb namespace show mysbnamespacea-test
    info:    Executing command sb namespace show
    + Getting namespace
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Active
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    UpdatedAt: 2013-11-14T16:25:37.85Z
    info:    sb namespace show command OK

**Provjerite je li polje naziva SB &lt;naziv >**

Provjerite je li dostupan prostor za naziv.

## <a name="commands-to-manage-your-storage-objects"></a>Naredbe za upravljanje objekte za pohranu

###<a name="commands-to-manage-your-storage-accounts"></a>Naredbe za upravljanje računima za pohranu

**popis za pohranu poslovnih subjekata [mogućnosti]**

Ta naredba prikazuje račune za pohranu na vašu pretplatu.

    ~$ azure storage account list
    info:    Executing command storage account list
    + Getting storage accounts
    data:    Name             Label  Location
    data:    ---------------  -----  --------
    data:    mybasestorage           West US
    info:    storage account list command OK

**Prikaz računa za pohranu [mogućnosti]<name>**

Ta naredba prikazuje podatke o računu za navedeni prostor za pohranu uključujući svojstva URI i račun.

**račun za pohranu stvaranje [mogućnosti]<name>**

Ta se naredba stvara račun za pohranu koji se temelji na navedenom mogućnosti.

    ~$ azure storage account create mybasestorage --label PrimaryStorage --location "West US"
    info:    Executing command storage account create
    + Creating storage account
    info:    storage account create command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **-e** ili **– natpis** &lt;oznaka >: natpis za račun za pohranu.
+ **-d** ili **– Opis** &lt;opis >: račun za pohranu opis.
+ **-l** ili **– mjesto** &lt;naziv >: regiji u kojoj želite stvoriti račun za pohranu.
+ **-a** ili **– afinitet grupe** &lt;naziv >: grupa afinitet s kojim će se povezati s računom za pohranu. 
+ **– Vrsta**: označava vrstu računa koju želite stvoriti: ili standardni pohranu zalihosti mogućnost (LRS/ZRS/GRS/RAGRS) ili Premium prostora za pohranu (PLRS).

**Postavljanje računa za pohranu [mogućnosti]<name>**

Ta se naredba ažurira računa navedena prostora za pohranu.

    ~$ azure storage account set mybasestorage --kind Storage --sku-name GRS
    info:    Executing command storage account set
    + Updating storage account
    info:    storage account set command OK

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **-e** ili **– natpis** &lt;oznaka >: natpis za račun za pohranu.
+ **-d** ili **– Opis** &lt;opis >: račun za pohranu opis.
+ **-l** ili **– mjesto** &lt;naziv >: regiji u kojoj želite stvoriti račun za pohranu.
+ **– Vrsta**: označava novu vrstu računa: ili standardni pohranu zalihosti mogućnost (LRS/ZRS/GRS/RAGRS) ili Premium prostora za pohranu (PLRS).

**Brisanje računa za pohranu [mogućnosti]<name>**

Ta se naredba briše računa navedena prostora za pohranu.

Ta se naredba podržava sljedeće dodatne mogućnosti:

**-q** ili **– Tihi**: pitaj za potvrdu. Tu mogućnost koristite u automatiziranog skripti.

###<a name="commands-to-manage-your-storage-account-keys"></a>Naredbe za upravljanje računom ključeva za pohranu

**prostor za pohranu računa tipke [mogućnosti] popisa<name>**

Ta se naredba popis primarnih i sekundarnih ključeva za račun navedeni prostora za pohranu.

**prostor za pohranu računa tipke obnoviti [mogućnosti]<name>**

###<a name="commands-to-manage-your-storage-container"></a>Naredbe za upravljanje sustava spremnik za pohranu

**Popis spremnik za pohranu [odrednice] [prefiks]**

Ta se naredba prikazat će se popis prostora za pohranu spremnik za račun navedeni prostora za pohranu. Prostor za pohranu računa navedena je niz za povezivanje ili prostora za pohranu imenom i računom ključ računa.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **-p** ili **-prefiksa** &lt;prefiksa >: prefiks imena pohranu kontejner.
+ **-a** ili **– naziv računa** &lt;accountName >: naziv računa za pohranu.
+ **-k** ili **– ključ za račun** &lt;accountKey >: ključ za račun za pohranu.
+ **-c** ili **– niz za povezivanje** &lt;connectionString >: niz za povezivanje za pohranu.
+ **– ispravljanje pogrešaka**: izvodi naredbu za pohranu u načinu rada za ispravljanje pogrešaka.

**prostor za pohranu spremnika Pokaži [odrednice] [spremnika]**
**spremnik za pohranu stvaranje [odrednice] [spremnika]**

Ta naredba stvara spremnik za pohranu za račun navedeni prostora za pohranu. Prostor za pohranu računa navedena je niz za povezivanje ili prostora za pohranu imenom i računom ključ računa.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– spremnika** &lt;spremnik >: naziv spremnika za pohranu da biste stvorili.
+ **-p** ili **-prefiksa** &lt;prefiksa >: prefiks imena pohranu kontejner.
+ **-a** ili **– naziv računa** &lt;accountName >: naziv računa spremišta
+ **-k** ili **– ključ za račun** &lt;accountKey >: ključ za račun za pohranu
+ **-c** ili **– niz za povezivanje** &lt;connectionString >: niz za povezivanje za pohranu
+ **– ispravljanje pogrešaka**: izvodi naredbu za pohranu u načinu rada za ispravljanje pogrešaka.

**prostor za pohranu spremnika Izbriši [odrednice] [spremnika]**

Ta se naredba briše spremnik navedeni prostora za pohranu. Prostor za pohranu računa navedena je niz za povezivanje ili prostora za pohranu imenom i računom ključ računa.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– spremnika** &lt;spremnik >: naziv spremnika za pohranu da biste stvorili.
+ **-p** ili **-prefiksa** &lt;prefiksa >: prefiks imena pohranu kontejner.
+ **-a** ili **– naziv računa** &lt;accountName >: naziv računa za pohranu.
+ **-k** ili **– ključ za račun** &lt;accountKey >: ključ za račun za pohranu.
+ **-c** ili **– niz za povezivanje** &lt;connectionString >: niz za povezivanje za pohranu.
+ **– ispravljanje pogrešaka**: izvodi naredbu za pohranu u načinu rada za ispravljanje pogrešaka.

**spremnik za pohranu postavljanje [mogućnosti] [spremnik]**

Ta se naredba postavlja popis za kontrolu pristupa za spremnik za pohranu. Prostor za pohranu računa navedena je niz za povezivanje ili prostora za pohranu imenom i računom ključ računa.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– spremnika** &lt;spremnik >: naziv spremnika za pohranu da biste stvorili.
+ **-p** ili **-prefiksa** &lt;prefiksa >: prefiks imena pohranu kontejner.
+ **-a** ili **– naziv računa** &lt;accountName >: naziv računa za pohranu.
+ **-k** ili **– ključ za račun** &lt;accountKey >: ključ za račun za pohranu.
+ **-c** ili **– niz za povezivanje** &lt;connectionString >: niz za povezivanje za pohranu.
+ **– ispravljanje pogrešaka**: izvodi naredbu za pohranu u načinu rada za ispravljanje pogrešaka.

###<a name="commands-to-manage-your-storage-blob"></a>Naredbe za upravljanje sustava spremište blobova platforme

**popis za pohranu blob [odrednice] [spremnika] [prefiks]**

Ta naredba Vrati popis blob polja za pohranu u spremniku navedeni prostora za pohranu.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– spremnika** &lt;spremnik >: naziv spremnika za pohranu da biste stvorili.
+ **-p** ili **-prefiksa** &lt;prefiksa >: prefiks imena pohranu kontejner.
+ **-a** ili **– naziv računa** &lt;accountName >: naziv računa za pohranu.
+ **-k** ili **– ključ za račun** &lt;accountKey >: ključ za račun za pohranu.
+ **-c** ili **– niz za povezivanje** &lt;connectionString >: niz za povezivanje za pohranu.
+ **– ispravljanje pogrešaka**: izvodi naredbu za pohranu u načinu rada za ispravljanje pogrešaka.

**spremište blobova Pokaži [mogućnosti] [spremnik] [blob]**

Ta naredba prikazuje detalje o blob navedeni prostora za pohranu.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– spremnika** &lt;spremnik >: naziv spremnika za pohranu da biste stvorili.
+ **-p** ili **-prefiksa** &lt;prefiksa >: prefiks imena pohranu kontejner.
+ **-a** ili **– naziv računa** &lt;accountName >: naziv računa za pohranu.
+ **-k** ili **– ključ za račun** &lt;accountKey >: ključ za račun za pohranu.
+ **-c** ili **– niz za povezivanje** &lt;connectionString >: niz za povezivanje za pohranu.
+ **– ispravljanje pogrešaka**: izvodi naredbu za pohranu u ispravljanje pogrešaka.

**spremište blobova izbrišite [mogućnosti] [spremnik] [blob]**

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– spremnika** &lt;spremnik >: naziv spremnika za pohranu da biste stvorili.
+ **-b** ili **– blob** &lt;blobName >: naziv spremišta blobova da biste izbrisali.
+ **-q** ili **– Tihi**: uklanjanje spremišta blobova platforme navedeni bez potvrdu.
+ **-a** ili **– naziv računa** &lt;accountName >: naziv računa za pohranu.
+ **-k** ili **– ključ za račun** &lt;accountKey >: ključ za račun za pohranu.
+ **-c** ili **– niz za povezivanje** &lt;connectionString >: niz za povezivanje za pohranu.
+ **– ispravljanje pogrešaka**: izvodi naredbu za pohranu u ispravljanje pogrešaka.

**spremište blobova platforme prijenos [odrednice] [datoteka] [spremnika] [blob]**

Ta se naredba prenesite navedenu datoteku spremišta blobova specified\.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– spremnika** &lt;spremnik >: naziv spremnika za pohranu da biste stvorili.
+ **-b** ili **– blob** &lt;blobName >: naziv spremišta blobova da biste prenijeli.
+ **-t** ili **– blobtype** &lt;blobtype >: vrstu spremišta blobova platforme: stranice ili bloka.
+ **-p** ili **– Svojstva** &lt;svojstva >: svojstava blob prostora za pohranu prenesene datoteke. Svojstva su ključ = s par vrijednost i razdvojene točkom. Svojstva dostupna su contentType, contentEncoding, contentLanguage i cacheControl.
+ **m –** ili **– metapodataka** &lt;metapodataka >: blob metapodataka za pohranu za prenesenih datoteka. Metapodaci su vam ključne = parove vrijednosti naredbe d razdvojene zarezom (;).
+ **– concurrenttaskcount** &lt;concurrenttaskcount >: maksimalan broj zahtjeva Istodobni prijenos.
+ **-q** ili **– Tihi**: prebrisati spremišta blobova platforme navedenu bez potvrdu.
+ **-a** ili **– naziv računa** &lt;accountName >: naziv računa za pohranu.
+ **-k** ili **– ključ za račun** &lt;accountKey >: ključ za račun za pohranu.
+ **-c** ili **– niz za povezivanje** &lt;connectionString >: niz za povezivanje za pohranu.
+ **– ispravljanje pogrešaka**: izvodi naredbu za pohranu u ispravljanje pogrešaka.

**spremište blobova platforme preuzimanje [odrednice] [spremnik] [blob] [odredište]**

Ta se naredba preuzima navedeni spremišta blobova.

Ta se naredba podržava sljedeće dodatne mogućnosti:

+ **– spremnika** &lt;spremnik >: naziv spremnika za pohranu da biste stvorili.
+ **-b** ili **– blob** &lt;blobName >: naziv blob prostora za pohranu.
+ **-d** ili **– odredište** [odredište]: preuzimanje odredišnu datoteku ili direktorij put.
+ **-m** ili **– checkmd5**: md5sum potvrdite preuzete datoteke.
+ **– concurrenttaskcount** &lt;concurrenttaskcount > maksimalan broj Istodobni prijenos zahtjeva
+ **-q** ili **– Tihi**: prebrisati odredišnu datoteku bez potvrde.
+ **-a** ili **– naziv računa** &lt;accountName >: naziv računa za pohranu.
+ **-k** ili **– ključ za račun** &lt;accountKey >: ključ za račun za pohranu.
+ **-c** ili **– niz za povezivanje** &lt;connectionString >: niz za povezivanje za pohranu.
+ **– ispravljanje pogrešaka**: izvodi naredbu za pohranu u ispravljanje pogrešaka.

## <a name="commands-to-manage-sql-databases"></a>Naredbe za upravljanje bazama podataka za SQL

Upravljanje baze podataka SQL Azure pomoću ove naredbe

###<a name="commands-to-manage-sql-servers"></a>Naredbe za upravljanje SQL poslužitelja.

Upravljanje SQL Server pomoću ove naredbe

**Stvaranje sustava SQL server &lt;administratorLogin > &lt;administratorPassword > &lt;mjesto >**

Stvaranje poslužitelj baze podataka

    ~$ azure sql server create test T3stte$t "West US"
    info:    Executing command sql server create
    + Creating SQL Server
    data:    Server Name i1qwc540ts
    info:    sql server create command OK

**prikaz SQL server &lt;naziv >**

Prikaz pojedinosti o poslužitelju.

    ~$ azure sql server show xclfgcndfg
    info:    Executing command sql server show
    + Getting SQL server
    data:    SQL Server Name xclfgcndfg
    data:    SQL Server AdministratorLogin msopentechforums
    data:    SQL Server Location West US
    data:    SQL Server FullyQualifiedDomainName xclfgcndfg.database.windows.net
    info:    sql server show command OK

**popis za SQL server**

Pronađite popis poslužitelja.

    ~$ azure sql server list
    info:    Executing command sql server list
    + Getting SQL server
    data:    Name        Location
    data:    ----------  --------
    data:    xclfgcndfg  West US
    info:    sql server list command OK

**SQL server Izbriši &lt;naziv >**

Briše poslužitelju

    ~$ azure sql server delete i1qwc540ts
    info:    Executing command sql server delete
    Delete server i1qwc540ts? [y/n] y
    + Removing SQL Server
    info:    sql server delete command OK

###<a name="commands-to-manage-sql-databases"></a>Naredbe za upravljanje bazama podataka za SQL

Pomoću ove naredbe za upravljanje bazama podataka za SQL.

**[mogućnosti] stvaranja baze podataka SQL &lt;naziv poslužitelja > &lt;ImeBazePodataka > &lt;administratorPassword >**

Stvara instanca baze podataka

    ~$ azure sql db create fr8aelne00 newdb test
    info:    Executing command sql db create
    Administrator password: ********
    + Creating SQL Server Database
    info:    sql db create command OK

**SQL db prikaz [mogućnosti] &lt;naziv poslužitelja > &lt;ImeBazePodataka > &lt;administratorPassword >**

Prikaz detalja baze podataka.

    C:\windows\system32>azure sql db show fr8aelne00 newdb test
    info:    Executing command sql db show
    Administrator password: ********
    + Getting SQL server databases
    data:    Database _ ContentRootElement=m:properties, id=https://fr8aelne00.datab
    ase.windows.net/v1/ManagementService.svc/Server2('fr8aelne00')/Databases(4), ter
    m=Microsoft.SqlServer.Management.Server.Domain.Database, scheme=http://schemas.m
    icrosoft.com/ado/2007/08/dataservices/scheme, link=[rel=edit, title=Database, hr
    ef=Databases(4), rel=http://schemas.microsoft.com/ado/2007/08/dataservices/relat
    ed/Server, type=application/atom+xml;type=entry, title=Server, href=Databases(4)
    /Server, rel=http://schemas.microsoft.com/ado/2007/08/dataservices/related/Servi
    ceObjective, type=application/atom+xml;type=entry, title=ServiceObjective, href=
    Databases(4)/ServiceObjective, rel=http://schemas.microsoft.com/ado/2007/08/data
    services/related/DatabaseMetrics, type=application/atom+xml;type=entry, title=Da
    tabaseMetrics, href=Databases(4)/DatabaseMetrics, rel=http://schemas.microsoft.c
    om/ado/2007/08/dataservices/related/DatabaseCopies, type=application/atom+xml;ty
    pe=feed, title=DatabaseCopies, href=Databases(4)/DatabaseCopies], title=, update
    d=2013-11-18T19:48:27Z, name=
    data:    Database Id 4
    data:    Database Name newdb
    data:    Database ServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database AssignedServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database ServiceObjectiveAssignmentState 1
    data:    Database ServiceObjectiveAssignmentStateDescription Complete
    data:    Database ServiceObjectiveAssignmentErrorCode
    data:    Database ServiceObjectiveAssignmentErrorDescription
    data:    Database ServiceObjectiveAssignmentSuccessDate
    data:    Database Edition Web
    data:    Database MaxSizeGB 1
    data:    Database MaxSizeBytes 1073741824
    data:    Database CollationName SQL_Latin1_General_CP1_CI_AS
    data:    Database CreationDate
    data:    Database RecoveryPeriodStartDate
    data:    Database IsSystemObject
    data:    Database Status 1
    data:    Database IsFederationRoot
    data:    Database SizeMB -1
    data:    Database IsRecursiveTriggersOn
    data:    Database IsReadOnly
    data:    Database IsFederationMember
    data:    Database IsQueryStoreOn
    data:    Database IsQueryStoreReadOnly
    data:    Database QueryStoreMaxSizeMB
    data:    Database QueryStoreFlushPeriodSeconds
    data:    Database QueryStoreIntervalLengthMinutes
    data:    Database QueryStoreClearAll
    data:    Database QueryStoreStaleQueryThresholdDays
    info:    sql db show command OK

**Popis db SQL [mogućnosti] &lt;naziv poslužitelja > &lt;administratorPassword >**

Prikaži popis baza podataka.

    ~$ azure sql db list fr8aelne00 test
    info:    Executing command sql db list
    Administrator password: ********
    + Getting SQL server databases
    data:    Name    Edition  Collation                     MaxSizeInGB
    data:    ------  -------  ----------------------------  -----------
    data:    master  Web      SQL_Latin1_General_CP1_CI_AS  5
    info:    sql db list command OK

**SQL db izbrišite [mogućnosti] &lt;naziv poslužitelja > &lt;ImeBazePodataka > &lt;administratorPassword >**

Brisanje baze podataka.

    ~$ azure sql db delete fr8aelne00 newdb test
    info:    Executing command sql db delete
    Administrator password: ********
    Delete database newdb? [y/n] y
    + Getting SQL server databases
    + Removing database
    info:    sql db delete command OK

###<a name="commands-to-manage-your-sql-server-firewall-rules"></a>Naredbe za upravljanje pravilima Vatrozid za SQL Server

Upravljanje pravilima Vatrozid za SQL Server pomoću te naredbe

**SQL firewallrule stvaranje [mogućnosti] &lt;naziv poslužitelja > &lt;ruleName > &lt;startIPAddress > &lt;endIPAddress >**

Stvaranje pravila vatrozida za SQL Server.

    ~$ azure sql firewallrule create fr8aelne00 allowed 131.107.0.0 131.107.255.255
    info:    Executing command sql firewallrule create
    + Creating Firewall Rule
    info:    sql firewallrule create command OK

**SQL firewallrule Pokaži [mogućnosti] &lt;naziv poslužitelja > &lt;ruleName >**

Prikaži vatrozid detalje pravila.

    ~$ azure sql firewallrule show fr8aelne00 allowed
    info:    Executing command sql firewallrule show
    + Getting firewall rule
    data:    Firewall rule Name allowed
    data:    Firewall rule Type Microsoft.SqlAzure.FirewallRule
    data:    Firewall rule State Normal
    data:    Firewall rule SelfLink https://management.core.windows.net/9e672699-105
    5-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00/firewallrules/allowed
    data:    Firewall rule ParentLink https://management.core.windows.net/9e672699-1
    055-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00
    data:    Firewall rule StartIPAddress 131.107.0.0
    data:    Firewall rule EndIPAddress 131.107.255.255
    info:    sql firewallrule show command OK

**Popis firewallrule SQL [mogućnosti] &lt;naziv poslužitelja >**

Popis pravila vatrozida.

    ~$ azure sql firewallrule list fr8aelne00
    info:    Executing command sql firewallrule list
    \data:    Name     Start IP address  End IP address
    data:    -------  ----------------  ---------------
    data:    allowed  131.107.0.0       131.107.255.255
    +
    info:    sql firewallrule list command OK

**SQL firewallrule izbrišite [mogućnosti] &lt;naziv poslužitelja > &lt;ruleName >**

Ta se naredba briše pravila vatrozida.

    ~$ azure sql firewallrule delete fr8aelne00 allowed
    info:    Executing command sql firewallrule delete
    Delete rule allowed? [y/n] y
    + Removing firewall rule
    info:    sql firewallrule delete command OK

## <a name="commands-to-manage-your-virtual-networks"></a>Naredbe za upravljanje virtualne mreže

Upravljanje virtualne mreže pomoću ove naredbe

**mrežni vnet stvaranje [mogućnosti] &lt;mjesto >**

Stvaranje virtualne mreže.

    ~$ azure network vnet create vnet1 --location "West US" -v
    info:    Executing command network vnet create
    info:    Using default address space start IP: 10.0.0.0
    info:    Using default address space cidr: 8
    info:    Using default subnet start IP: 10.0.0.0
    info:    Using default subnet cidr: 11
    verbose: Address Space [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/8 (16777216)
    verbose: Subnet [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/11 (2097152)
    verbose: Fetching Network Configuration
    verbose: Fetching or creating affinity group
    verbose: Fetching Affinity Groups
    verbose: Fetching Locations
    verbose: Creating new affinity group AG1
    info:    Using affinity group AG1
    verbose: Updating Network Configuration
    info:    network vnet create command OK

**mrežni vnet Prikaži &lt;naziv >**

Prikaz detalja o virtualne mreže.

    ~$ azure network vnet show vnet1
    info:    Executing command network vnet show
    + Fetching Virtual Networks
    data:    Name "vnet1"
    data:    Id "25786fbe-08e8-4e7e-b1de-b98b7e586c7a"
    data:    AffinityGroup "AG1"
    data:    State "Created"
    data:    AddressSpace AddressPrefixes 0 "10.0.0.0/8"
    data:    Subnets 0 Name "subnet-1"
    data:    Subnets 0 AddressPrefix "10.0.0.0/11"
    info:    network vnet show command OK

**Popis vnet mreža**

Popis postojeće virtualne mreže.

    ~$ azure network vnet list
    info:    Executing command network vnet list
    + Fetching Virtual Networks
    data:    Name        Status   AffinityGroup
    data:    ----------  -------  -------------
    data:    vnet1      Created  AG1
    data:    vnet2      Created  AG1
    data:    vnet3      Created  AG1
    data:    vnet4      Created  AG1
    info:    network vnet list command OK


**Brisanje mreže vnet &lt;naziv >**

Briše navedeni virtualne mreže.

    ~$ azure network vnet delete opentechvn1
    info:    Executing command network vnet delete
    + Fetching Network Configuration
    Delete the virtual network opentechvn1 ?  (y/n) y
    + Deleting the virtual network opentechvn1
    info:    network vnet delete command OK

**Izvoz mreže [put datoteke]**

Za napredno mrežna konfiguracija možete izvesti mrežna konfiguracija lokalno. Konfiguracija izvezene mreže sadrži postavke DNS poslužitelja, virtualne mrežne postavke, postavke lokalne mreže web-mjesta i druge postavke.

**Uvoz mreže [put datoteke]**

Uvoz konfiguracije za lokalnu mrežu.

**mrežni dnsserver registrirati [mogućnosti] &lt;dnsIP >**

Registrirajte se DNS poslužitelja koje namjeravate koristiti za razrješavanje imena u mrežnoj konfiguraciji.

    ~$ azure network dnsserver register 98.138.253.109 --dns-id FrontEndDnsServer
    info:    Executing command network dnsserver register
    + Fetching Network Configuration
    + Updating Network Configuration
    info:    network dnsserver register command OK

**Popis dnsserver mreža**

Popis svih DNS poslužitelji registrira u mrežnoj konfiguraciji.

    ~$ azure network dnsserver list
    info:    Executing command network dnsserver list
    + Fetching Network Configuration
    data:    DNS Server ID         DNS Server IP
    data:    --------------------  --------------
    data:    DNS-bb39b4ac34d66a86  44.55.22.11
    data:    FrontEndDnsServer     98.138.253.109
    info:    network dnsserver list command OK

**mrežni dnsserver unregister [mogućnosti] &lt;dnsIP >**

Uklanja stavke na DNS poslužitelj iz mrežna konfiguracija.

    ~$ azure network dnsserver unregister 77.88.99.11
    info:    Executing command network dnsserver unregister
    + Fetching Network Configuration
    Delete the DNS server entry dns-4 ( 77.88.99.11 ) %s ? (y/n) y
    + Deleting the DNS server entry dns-4 ( 77.88.99.11 )
    info:    network dnsserver unregister command OK

