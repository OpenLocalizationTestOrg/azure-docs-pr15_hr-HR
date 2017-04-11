<properties
    pageTitle="Pomoću Docker VM proširenje za Linux na Azure"
    description="Opisuje Docker i proširenja virtualnim računalima sustava Azure i prikazuje kako programski stvoriti virtualnim strojevima na Azure koji su docker hosts iz naredbenog retka pomoću EŽA Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="08/29/2016"
    ms.author="rasquill"/>

# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>Korištenje nastavka VM Docker iz naredbenog retka Azure sučelja (Azure EŽA)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]



U ovoj se temi opisuje kako stvoriti na VM s nastavkom VM Docker iz servisa management (asm) načina rada u Azure EŽA na svim platformama. [Docker](https://www.docker.com/) je jedan od najpopularnijih virtualizacije pristupa koja koristi [Linux spremnika](http://en.wikipedia.org/wiki/LXC) umjesto virtualnim strojevima kao način izoliranja podataka i rad na zajedničkim resursima. Proširenje Docker VM i [Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) možete koristiti da biste stvorili Docker VM koji hostira bilo koji broj spremnici aplikacija na Azure. Da biste vidjeli više razine rasprave spremnika i njihove prednosti, potražite u članku [Docker visoke razine Zaslonska ploča](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).


##<a name="how-to-use-the-docker-vm-extension-with-azure"></a>Kako koristiti VM proširenje Docker s Azure
Da biste koristili proširenje Docker VM s Azure, morate instalirati verziju [Azure naredbenog retka sučelja](https://github.com/Azure/azure-sdk-tools-xplat) (Azure EŽA) veće od 0.8.6 (kao što je pisanja ovog trenutna verzija je 0.10.0). Azure EŽA možete instalirati na Mac, Linux i Windows.


Dovršavanje postupka za korištenje Docker na Azure je jednostavno:

+ Instalirajte EŽA Azure i njezine ovisnosti na računalu s kojeg želite nadzirati Azure (u sustavu Windows, to će biti raspodjele Linux izvodi kao virtualnog računala)
+ Koristite naredbe za Azure EŽA Docker da biste stvorili VM Docker glavno računalo u Azure
+ Lokalni Docker naredbama za upravljanje sustava Docker spremnika u vašem Docker VM servisu Azure.


### <a name="install-the-azure-command-line-interface-azure-cli"></a>Instalacija sustava Azure sučelje naredbenog retka (Azure EŽA)

Da biste instalirali i konfigurirali EŽA Azure, pročitajte [kako se instalira Azure sučelja naredbenog retka](../xplat-cli-install.md). Da biste potvrdili instalaciju, upišite `azure` u naredbenom retku i nakon kratki trenutak trebali biste vidjeti Azure EŽA ASCII crteža koji će se popisi osnovne dostupne naredbe. Ako instalacija uspjeli pravilno, trebali biste moći upišite `azure help vm` i je li jednu od navedenih naredbi "docker".

> [AZURE.NOTE] Docker sadrži alate za Windows, [Docker računalu](https://docs.docker.com/installation/windows/), koji vam može poslužiti da biste automatizirali stvaranje klijent docker koje možete koristiti da biste radili s Azure VMs kao docker domaćini.

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>Povezivanje Azure EŽA da biste na račun za Azure
Prije korištenja EŽA Azure vjerodajnice za račun za Azure morate pridružiti EŽA Azure na svoju platformu. U odjeljku [kako se povezati sa pretplate Azure](../xplat-cli-connect.md) objašnjava kako preuzeti i uvezli datoteku **.publishsettings** ili da biste svoje EŽA Azure pridružiti id tvrtke ili ustanove.

> [AZURE.NOTE] Postoje neke razlike ponašanje pri korištenju neke druge načine provjere autentičnosti, pa provjerite je li za čitanje dokumenta da biste razumjeli različite funkcije.

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>Instaliranje Docker i korištenje VM proširenje Docker za Azure
Slijedite [upute za instalaciju Docker](https://docs.docker.com/installation/#installation) da biste instalirali Docker lokalno na vašem računalu.

Da biste koristili Docker pomoću programa Azure virtualnog računala, slika Linux koristi za na VM mora imati [Azure Linux VM Agent](virtual-machines-linux-agent-user-guide.md) instaliran. Trenutno postoje samo dvije vrste slika koje omogućuju sljedeće:

+ Ubuntu sliku iz galerije slika Azure ili

+ Prilagođeni Linux sliku koju ste stvorili s Azure Linux VM agenta instalacije i konfiguracije. Dodatne informacije o tome da biste sastavili prilagođene VM Linux s Azure VM Agent potražite u članku [VM Agent za Azure Linux](virtual-machines-linux-agent-user-guide.md) .

### <a name="using-the-azure-image-gallery"></a>Pomoću galerije slika Azure

Iz tulumu ili Terminal sesiju, koristite sljedeću naredbu Azure EŽA da biste pronašli najnoviju Ubuntu sliku u galeriji VM koristiti tako da upišete

`azure vm image list | grep Ubuntu-14_04`

i odaberite jednu od nazivi slika kao što su `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, i koristite sljedeću naredbu da biste stvorili novi VM pomoću toj slici.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

pri čemu je:

+ * &lt;naziv vm cloudservice&gt; * je naziv VM koji će postati Docker spremnik glavno računalo u Azure

+  * &lt;korisničko ime&gt; * je korisničko ime korisnika zadani korijen od na VM

+ * &lt;lozinka&gt; * lozinka *korisničko ime* računa koji zadovoljava standarde složenosti za Azure

> [AZURE.NOTE] Trenutno lozinke mora biti najmanje 8 znakova, sadržavati jedan malim slovima i jedan pisani velikim slovima znakova, broj i posebnih znakova kao što je jedan od sljedećih znakova: `!@#$%^&+=`. Ne, razdoblje na kraju prethodne rečenice nije posebnog znaka.

Ako je naredba je uspjela, trebali biste vidjeti nešto poput sljedećeg, ovisno o točnih argumenata i mogućnosti koje ste koristili:

![](./media/virtual-machines-linux-classic-cli-use-docker/dockercreateresults.png)

> [AZURE.NOTE] Stvaranje virtualnog računala može potrajati nekoliko minuta, ali kada je stvorena (vrijednost stanje `ReadyRole`) započinje daemon Docker (servis Docker), a možete se povezati s Docker spremnik glavno računalo.

Da biste testirali VM Docker koji ste stvorili u Azure, upišite

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

gdje * &lt;vm-naziv--koristite&gt; * je naziv virtualnog računala koje ste koristili u poziva `azure vm docker create`. Vidjet ćete nešto slično kao na sljedeće koji upućuje na to da vaše VM glavno računalo Docker gore poslužitelju u Azure i čeka se naredbe. 

Sada možete pokušati povezati pomoću docker klijent za dohvaćanje podataka (u nekim Docker postavu klijenta, kao što je koji su na Mac, možda ćete morati koristiti `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Da biste bili sigurni da je sve radne možete provjeriti VM za proširenje Docker:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Provjera autentičnosti VM glavno računalo za docker

Osim stvaranja Docker VM na `azure vm docker create` naredba se automatski stvara potrebne potvrde da biste omogućili Docker klijentsko računalo povezati glavno računalo za Azure spremnik pomoću HTTPS i certifikate pohranjuju se na računalima klijenta i glavno računalo, po potrebi. Na prilikom narednih pokušaja postojeće certifikati su ponovno iskoristiti i zajednički koristiti pomoću novog glavnog računala.

Prema zadanim postavkama certifikati spremaju se u `~/.docker`, a Docker će se konfigurirati za rad na priključak **2376**. Ako želite koristiti drugi priključak ili direktorija, a zatim možete koristiti jedan od sljedećih `azure vm docker create` mogućnostima naredbenog retka za konfiguriranje vaše Docker spremnik hostira VM da biste koristili drugi priključak ili drugu potvrde o povezivanju sa servisom klijenti:

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

Daemon Docker na glavnom računalu je konfiguriran za slušanje za i provjere autentičnosti klijentske veze na navedeni priključak pomoću certifikata koji je generirao na `azure vm docker create` naredbe. Na klijentskom računalu morate imati ove potvrde da biste pristupili glavno računalo za Docker.

> [AZURE.NOTE] Glavno računalo umreženi radi bez ove potvrde bit će dostupnim svakome koji se mogu povezati s računalom. Prije no što promijenite zadanu konfiguraciju, provjerite je li razumijete rizika na računalima i aplikacije.

## <a name="next-steps"></a>Daljnji koraci

* Spremni ste da biste se [Docker korisničkom priručniku] i koristiti vaš VM Docker. Da biste stvorili VM za Docker omogućena na novom portalu, pogledajte [kako se koriste nastavak VM Docker s portala sustava].

* Proširenje Azure Docker VM podržava i Docker sastavite, koji koristi deklarativno datoteke YAML stupe aplikacije za razvojne inženjere katalog modeliran sve okruženje i generiranje dosljedan implementacije. Potražite u članku [Početak rada s web-mjesto Docker i u okvir za sastavljanje definiranje i izvođenje više spremniku na Azure virtualnog računala].  

<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Next steps]: #next-steps

[How to use the Docker VM Extension with Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]: #Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
[Kako koristiti VM proširenje Docker s portala]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker korisničkom priručniku]: https://docs.docker.com/userguide/
 
[Početak rada s Docker i sastavljanje definiranje i pokrenuti više spremniku Azure virtualnog računala]:virtual-machines-linux-docker-compose-quickstart.md