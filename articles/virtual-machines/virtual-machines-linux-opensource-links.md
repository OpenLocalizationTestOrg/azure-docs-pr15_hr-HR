<properties
    pageTitle="Linux i Otvori izvor računalstvo na Azure | Microsoft Azure"
    description="Popis Linux i računalstvo Otvori izvor članaka na Azure, uključujući korištenje osnovni Linux, neke osnovne koncepte s pokretanjem ili prijenosom slika Linux Azure i druge sadržaje o određenim tehnologije i optimizacije."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="06/27/2016"
    ms.author="rasquill"/>



# <a name="linux-and-open-source-computing-on-azure"></a>Linux i Otvori izvor računalstvo na Azure

Pronaći sve dokumentaciju morate stvoriti i upravljanje sustavom Linux virtualnim strojevima u modelu klasični implementacije.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="get-started"></a>Početak rada
- [Uvod za Linux na Azure](virtual-machines-linux-intro-on-azure.md)
- [Najčešća pitanja o virtualnim računalima sustava Azure stvorene pomoću model klasični implementacije](virtual-machines-linux-classic-faq.md)
- [O slike za virtualnim strojevima](virtual-machines-linux-classic-about-images.md)
- [Prijenos Distro sliku](virtual-machines-linux-classic-create-upload-vhd.md) (i upute koristi [Azure-Endorsed raspodjele](virtual-machines-linux-endorsed-distros.md))
- [Prijavite se na pomoću Linux VM Azure klasični portal](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="set-up"></a>Postavljanje

- [Instalacija Azure sučelje naredbenog retka (Azure EŽA)](../xplat-cli-install.md)


## <a name="tutorials"></a>Vodiči za

- [Instalacija stog ŽARULJE na Linux virtualnog računala u Azure](virtual-machines-linux-create-lamp-stack.md)
- [Ruby tračnicama web-aplikacije na VM programa Azure](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
- [Kako: instalacija Apache Qpid Proton-C za AMQP i Bus servisa](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Baza podataka
- [Optimiziranja performansi MySQL na Azure](virtual-machines-linux-classic-optimize-mysql.md)
- [Klastere MySQL](virtual-machines-linux-classic-mysql-cluster.md)
- [Radi Cassandra s Linux Azure i pristup s Node.js](virtual-machines-linux-classic-cassandra-nodejs.md)
- [Stvaranje više glavni klaster od MariaDbs](virtual-machines-linux-classic-mariadb-mysql-cluster.md)

### <a name="hpc"></a>HPC
- [Početak rada s Linux računalnim čvorove programa klasteru HPC paketa u Azure](virtual-machines-linux-classic-hpcpack-cluster.md)
- [Mada NAMD s paket Microsoft HPC Linux računalnim čvorove u Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
- [Postavljanje Linux RDMA klaster za pokretanje aplikacije MPI](virtual-machines-linux-classic-rdma-cluster.md)

### <a name="docker"></a>Docker
- [Korištenje nastavka VM Docker iz naredbenog retka Azure sučelja (Azure EŽA)](virtual-machines-linux-classic-cli-use-docker.md)
- [Korištenje nastavka VM Docker s portala za Azure](virtual-machines-linux-classic-portal-use-docker.md)
- [Upute za korištenje docker računala na Azure](virtual-machines-linux-docker-machine.md)

### <a name="ubuntu"></a>Ubuntu
- [Kako: klastere MySQL](virtual-machines-linux-classic-mysql-cluster.md)
- [Kako: Node.js i Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md)

### <a name="opensuse"></a>OpenSUSE
- [Kako: instalirajte i pokrenite MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md)

### <a name="coreos"></a>CoreOS
- [Kako: korištenje CoreOS na Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="planning"></a>Planiranje
- [Smjernice za implementaciju servisa Azure infrastrukture](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md)
- [Odabir Linux korisnička imena](virtual-machines-linux-usernames.md)
- [Kako konfigurirati raspoloživost za virtualnim strojevima u modelu klasični implementacije](virtual-machines-linux-classic-configure-availability.md)
- [Kako zakazati planiranog održavanja na Azure VMs](virtual-machines-linux-planned-maintenance-schedule.md)
- [Upravljanje dostupnost virtualnim strojevima](virtual-machines-linux-manage-availability.md)
- [Planirano održavanje Linux virtualnim strojevima servisu Azure](virtual-machines-linux-planned-maintenance.md)


## <a name="deployment"></a>Uvođenje
- [Stvaranje prilagođene virtualnog računala radi Linux](virtual-machines-linux-classic-createportal.md)
- [Osnove: dohvaćanje Linux VM da biste stvorili predložak](virtual-machines-linux-classic-capture-image.md)
- [Informacije za koje nisu licencira distribucije](virtual-machines-linux-create-upload-generic.md)


## <a name="management"></a>Upravljanje

- [SSH](virtual-machines-linux-mac-create-ssh-keys.md)
- [Kako ponovno postavljanje lozinke ili SSH svojstva za Linux](virtual-machines-linux-classic-reset-access.md)
- [Korištenje korijenske](virtual-machines-linux-use-root-privileges.md)


## <a name="azure-resources"></a>Resursi za Azure

- [Agent za Azure Linux](virtual-machines-linux-agent-user-guide.md)
- [Azure VM proširenja i značajke](virtual-machines-windows-extensions-features.md)
- [Injecting podatke Prilagođeno VM za uporabu oblaka pokretanja](virtual-machines-windows-classic-inject-custom-data.md)


## <a name="storage"></a>Prostor za pohranu

- [Na disku podataka priložite Linux VM](virtual-machines-linux-classic-attach-disk.md)
- [Odvajanje Disk podataka s Linux VM](virtual-machines-linux-classic-detach-disk.md)
- [RAID](virtual-machines-linux-configure-raid.md)


## <a name="networking"></a>Povezivanje s mrežom
- [Upute za postavljanje krajnje točke na klasični virtualnog računala u Azure](virtual-machines-linux-classic-setup-endpoints.md)


## <a name="troubleshooting"></a>Otklanjanje poteškoća
- [Otklanjanje poteškoća s vezama sigurne ljuske (SSH) da biste s operacijskim sustavom Linux Azure virtualnog računala](virtual-machines-linux-troubleshoot-ssh-connection.md)
- [Otklanjanje poteškoća s klasični implementaciju sa stvaranjem novog Linux virtualnog računala u Azure](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md)  
- [Otklanjanje poteškoća klasični implementaciju s ponovnim ili promjenu veličine na postojeće Linux virtualnog računala u Azure](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md) 


## <a name="reference"></a>Referenca

- [Azure EŽA naredbi u načinu rada za upravljanje servisom Azure (asm)](../virtual-machines-command-line-tools.md)
- [Upravljanje servisom za Azure REST API-JA](https://msdn.microsoft.com/library/azure/ee460799.aspx)




## <a name="general-links"></a>Općenito veze
Na sljedećim vezama su za Microsoft blogova, stranica Technet i vanjskih web-mjesta umjesto Azure.com dokumentacije kao gore. Kao i Azure i Otvori izvor potrebnom svijeta brzo premještanje ciljnih web-mjesta, je gotovo određene na sljedećim vezama jesu iz datuma, *despite* fact smo moraju učinite sve najbolje za neprestano dodajte noviju teme i uklonite zastarjele one. Ako ne možemo ja propustio jednu, provjerite Javite nam informacije u komentarima ili pošaljite zahtjev istaknuti naše [GitHub repo](https://github.com/Azure/azure-content/).

- [ASP.NET 5 sustavom Linux pomoću Docker spremnika](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
- [Kako implementirati sliku VM CentOS iz OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
- [Infrastruktura za ažuriranje SUSE](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
- [Server Linux Enterprise SUSE za SAP oblaka potražite biblioteku](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
- [Stvaranje iznimno dostupna Linux na Azure u koracima od 12](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
- [Automatiziranje dodjeljivanje Linux na Azure s Azure EŽA, node.js, jhawk](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
- [GlusterFS na Azure](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
- [Pokretanje FreeBSD u Azure](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
- [Jednostavno implementacija FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
- [Implementacija prilagođene FreeBSD slika](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
- [Kaspersky AV za poslužitelj Linux datoteka](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL

- [8 NoSql Otvori izvor baze podataka za Azure](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
- [Slideshare (MSOpenTech): Iskustvo CouchDb na Azure](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
- [Pokretanje CouchDB kao-na-usluga s node.js, CORS i Grunt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)

- [Redis u sustavu Windows na servisu Azure Redis predmemorije](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
- [Objavljivanje stanje ASP.NET sesije davatelj usluga za Redis pretpregled izdanja](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)

- [Blog: RavenHQ sada dostupna u Azure Marketplace](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Velikih skupova podataka
- [Instalacija Hadoop na Azure Linux VMs](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
- [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relacijske baze podataka
- [Arhitektura dostupnost visoke MySQL u Microsoft Azure](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
- [Instaliranje Postgres s corosync, pg_bouncer pomoću ILB](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Visoke performanse Linux računalstvo (HPC)

- [Predložak za brzi početak rada: Okretni gore SLURM klaster](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (i [bloga](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
- [Predložak za brzi početak rada: Stvaranje programa klasteru HPC s Linux računalnim čvorove](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>Devops, upravljanje i optimizaciju

Kao svijetu devops, upravljanje i optimizirati je prilično rastuća i vrlo brzo mijenjanje, razmislite o popisu ispod početne točke.

- [Videozapis: Azure virtualnim strojevima: korištenje Chef, Puppet i Docker za upravljanje Linux VMs](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)

- [Čitav vodič s automatiziranog implementacijom klaster Kubernetes s CoreOS i vodoravna](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
- [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)

- [Podređeni Jenkins dodatak za Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
- [GitHub repo: Jenkins pohranu dodatak za Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)

- [Trećih strana: Dodatak za Azure podređeni Hudson](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
- [Trećih strana: Dodatak za Azure pohranu Hudson](https://github.com/hudson3-plugins/windows-azure-storage-plugin)

- [Videozapis: Što je Chef i kako to funkcionira?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

- [Videozapis: Kako koristiti Azure Automatizacija s Linux VMs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)

- [Blog: Kako se DSC ljuske Powershell za Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
- [GitHub: Klijent Docker DSC](https://github.com/anweiss/DockerClientDSC)

- [Dodatak packer za Azure](https://github.com/msopentech/packer-azure)
