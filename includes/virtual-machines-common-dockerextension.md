

[Docker](https://www.docker.com/) je jedan od najpopularnijih virtualizacije pristupa koja koristi [Linux spremnika](http://en.wikipedia.org/wiki/LXC) umjesto virtualnim strojevima kao način izoliranja podataka aplikacije i računalstvo zajedničkog resursa. [Proširenje Azure Docker VM](https://github.com/Azure/azure-docker-extension/blob/master/README.md) [Azure Linux Agent](../articles/virtual-machines/virtual-machines-linux-agent-user-guide.md) možete koristiti da biste stvorili Docker VM koji hostira bilo koji broj spremnici aplikacija na Azure.

U ovoj se temi opisuju:

+ [Docker i Linux spremnika]
+ [Kako koristiti VM proširenje Docker s Azure]
+ [Virtualnog računala datotečne nastavke za Linux i Windows]

Da biste stvorili Docker omogućeno VMs odmah, pogledajte:

+ [Kako koristiti VM proširenje Docker iz Azure sučelja naredbenog retka (Azure EŽA)]
+ [Kako koristiti VM proširenje Docker s portala za Azure klasični]
+ [Upute za početak brzo Docker u Azure Marketplace]

Da biste saznali više o proširenje i kako funkcionira, potražite u [Vodiču Docker nastavak](https://github.com/Azure/azure-docker-extension/blob/master/README.md).

## <a name="docker-and-linux-containers"></a>Docker i Linux spremnika
[Docker](https://www.docker.com/) je jedan od najpopularnijih pristupa virtualizacije koji koristi [Linux spremnika](http://en.wikipedia.org/wiki/LXC) umjesto virtualnim strojevima kao način izoliranja podataka i rad na zajedničkim resursima i omogućuje druge servise koji omogućuju vam Sastavljanje i brzo prikupiti aplikacije i distribucija ih između druge spremnike Docker.

Docker i Linux spremnika nisu [Hypervisors](http://en.wikipedia.org/wiki/Hypervisor) kao što su Windows Hyper-V i [KVM](http://www.linux-kvm.org/page/Main_Page) na Linux (su mnoge još primjera). Hypervisors virtualizacije Temeljni operacijski sustav da biste omogućili dovršeno operacijskim sustavima (pod nazivom *virtualnim strojevima*) da biste pokrenuli unutar na hypervisor kao da su aplikacije.

Docker drugi *spremnik* pristupa imaju radically smanjiti vrijeme pokretanja potrošena i obrada i spremište indirektni potrebno pomoću postupka i datoteka sustava odvajanja značajke otklanjanje Linux da bi se prikazale samo za otklanjanje značajke u suprotnom Izolirani kontejner.

U sljedećoj tablici opisane vrlo visoke razine vrste razlike u značajkama koji postoji između hypervisors i spremnika Linux. Imajte na umu da neke značajke možda više ili manje poželjno ovisno o aplikaciji potrebama.

|   Značajka      | Hypervisors | Spremnika  |
| :------------- |-------------| ----------- |
| Postupak odvajanja | Više ili manje dovršeno | Ako je korijenski nabavili, nije moguće ugrožena spremnik glavnog računala |
| Memorije na disku potrebna | Dovršavanje OS plus aplikacije | Preduvjeti za aplikaciju samo |
| Vrijeme za pokretanje | Značajno dulje: Pokretanje sustava OS plus učitavanja aplikacije | Značajno kraći: samo aplikacije morate pokrenuti jer otklanjanje već pokrenut  |
| Automatizacija spremnik | Široko ovisi o OS i aplikacija | [Galerija slika docker](https://registry.hub.docker.com/); drugim korisnicima

Da biste vidjeli više razine rasprave spremnika i njihove prednosti, potražite u članku [Docker visoke razine Zaslonska ploča](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

Dodatne informacije o što je Docker i zaista kako potražite u članku [što je Docker?](https://www.docker.com/whatisdocker/)

#### <a name="docker-and-linux-container-security-best-practices"></a>Preporučeni Načini rada docker i Linux spremnik sigurnosti

Jer spremnika imaju pristup otklanjanje glavno računalo ako je mogućnost da bi se dobio korijenski kodu i možda moći pristupiti ne samo na glavnom računalu, ali i druge spremnike. Dodatne informacije svakako od zadanu konfiguraciju [Docker preporučuje](https://docs.docker.com/articles/security/) korištenje zbrajanja-pravilnik grupe ili [uloga temeljenu na](http://en.wikipedia.org/wiki/Role-based_access_control) isto tako, kao što su [SELinux](http://selinuxproject.org/page/Main_Page) ili [AppArmor](http://wiki.apparmor.net/index.php/Main_Page), na primjer, kao i smanjivanje najveću moguću otklanjanje mogućnosti koje odobravaju spremnike sigurnost sustava spremnik. Osim toga, postoje mnogo drugih dokumenata na Internetu koji opisuje postupke za sigurnost pomoću spremnika kao što je Docker.

## <a name="how-to-use-the-docker-vm-extension-with-azure"></a>Kako koristiti VM proširenje Docker s Azure

Proširenje VM Docker je komponenta korisničkog sučelja koji je instaliran u VM instanci koje ste stvorili koju sam instalira modul Docker i upravlja daljinsku komunikaciju s na VM. Da biste instalirali proširenje VM dva načina: možete stvoriti na VM pomoću portala za upravljanje ili možete stvoriti iz Azure sučelja naredbenog retka (Azure EŽA).

Portal sustava možete koristiti da biste dodali VM proširenje Docker sve kompatibilne VM Linux (trenutno samo slike koja ga podržava je slika Ubuntu 14.04 LTS novije od srpanj). Pomoću naredbenog retka EŽA Azure, međutim, možete instalirati VM proširenje Docker i stvaranje i prijenos certifikatima komunikacije Docker sve u isto vrijeme prilikom stvaranja VM instance.

Da biste stvorili Docker omogućeno VMs odmah, pogledajte:

+ [Kako koristiti VM proširenje Docker iz Azure sučelja naredbenog retka (Azure EŽA)]
+ [Kako koristiti VM proširenje Docker s portala za Azure klasični]

## <a name="virtual-machine-extensions-for-linux-and-windows"></a>Virtualnog računala datotečne nastavke za Linux i Windows
[Docker VM proširenja za Azure](https://github.com/Azure/azure-docker-extension/blob/master/README.md) je samo jedan od nekoliko VM nastavaka koje će vam pružiti posebno ponašanje pa su u razvoju. Ako, na primjer, nekoliko značajki [Linux VM Agent proširenje](../articles/virtual-machines/virtual-machines-linux-agent-user-guide.md) omogućuju izmijeniti i upravljanje virtualnog računala, uključujući sigurnosnim značajkama, Jezgra i povezivanje s mrežom značajke i tako dalje. Proširenje VMAccess, primjerice možete ponovno postaviti administratorsku lozinku ili SSH ključ.

Cjelovit popis potražite u članku [Azure VM proširenja](../articles/virtual-machines/virtual-machines-windows-extensions-features.md).

<!--Anchors-->
[Kako koristiti VM proširenje Docker iz Azure sučelja naredbenog retka (Azure EŽA)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Kako koristiti VM proširenje Docker s portala za Azure klasični]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/
[Upute za početak brzo Docker u Azure Marketplace]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-ubuntu-quickstart/
[Docker i Linux spremnika]: #Docker-and-Linux-Containers
[Kako koristiti VM proširenje Docker s Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Virtualnog računala datotečne nastavke za Linux i Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows
