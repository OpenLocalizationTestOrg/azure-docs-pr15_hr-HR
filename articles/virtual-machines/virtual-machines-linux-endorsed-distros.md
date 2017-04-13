<properties
    pageTitle="Licencira distribucija Linux | Microsoft Azure"
    description="Informirajte se o Linux na Azure licencira distribucija, uključujući smjernice za Ubuntu, OpenLogic, Oracle i SUSE."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager"
    />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="szark"/>



#<a name="linux-on-azure-endorsed-distributions"></a>Linux na Azure licencira distribucije

> [AZURE.NOTE] Ako imate nekoliko sekundi dok, Pridonesite poboljšanju dokumentaciju Azure Linux VM prihvaćanjem ovaj [Brzi upitnik](https://aka.ms/linuxdocsurvey) rad. Svaki odgovor pridonose pomoć zatražite posla.

Linux slike iz galerije Azure ili trgovine nudi brojne partnere i radimo s različitim zajednicama Linux da biste dodali još više flavors popis za raspodjelu licencira. U aplikacijom distribucija nisu dostupni u galeriji možete uvijek Premjesti-vaše – vlasnik – Linux slijedeći upute na [toj stranici](virtual-machines-linux-classic-create-upload-vhd.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="supported-distributions--versions"></a>Podržani distribucija & verzije ##

U sljedećoj su tablici navedeni verzije koje su podržane u Azure i distribucija Linux. Također pogledajte [podrška za Linux slike u programu Microsoft Azure](https://support.microsoft.com/en-us/kb/2941892) detaljnije informacije.

Linux servisima za integraciju (LIS) upravljačke programe za Hyper-V i Azure su moduli otklanjanje doprinosa Microsoft izravno upstream otklanjanje Linux.  Upravljačke programe za LIS ili su ugrađeni u je distribucija otklanjanje po zadanom ili za starije RHEL/CentOS-poštu su distribucija zasebnom preuzeti [ovdje](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409).  Pročitajte [Ovaj članak](virtual-machines-linux-create-upload-generic.md#linux-kernel-requirements) dodatne informacije o upravljačke programe za LIS.

Agent za Azure Linux unaprijed već instaliran na slikama Galerija Azure i obično su dostupni za raspodjelu paketa spremištu.  Izvorni kod možete pronaći na [GitHub](https://github.com/azure/walinuxagent).

Distribuciju.|Verzija|Upravljačke programe|Agent
---|---|---|---
CentOS po OpenLogic | CentOS 6,3 +, 7.0 + | CentOS 6,3: [Preuzimanje LIS](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>Otklanjanje centOS: 6,4 + u | Paket: U [OpenLogic repo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) u odjeljku "WALinuxAgent" <br/>Izvorni kod: [GitHub](https://github.com/Azure/WALinuxAgent)
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) | 494.4.0+ | U otklanjanje | Izvorni kod: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Debian | Debian 7,9 +, 8,2 + | U otklanjanje | Paket: U repo u odjeljku "waagent" <br/>Izvorni kod: [GitHub](https://github.com/Azure/WALinuxAgent)
Oracle Linux | 6,4 +, 7.0 + | U otklanjanje | Paket: U repo u odjeljku "WALinuxAgent" <br/>Izvorni kod: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Crvena je vaša Enterprise Linux | RHEL 6,7 +, 7.1 + | U otklanjanje|Paket: U repo u odjeljku "WALinuxAgent" <br/>Izvorni kod: [GitHub](https://github.com/Azure/WALinuxAgent)
SUSE Linux Enterprise | SLES 11 SP4, SLES 12 SP1 + i <p> SLES za SAP 11 SP3 + | U otklanjanje | Paket: U [Oblaku: Alati](https://build.opensuse.org/project/show/Cloud:Tools) repo u odjeljku "python azure-agenta" <br/>Izvorni kod: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE | openSUSE 13.2 + | U otklanjanje | Paket: U [Oblaku: Alati](https://build.opensuse.org/project/show/Cloud:Tools) repo u odjeljku "python azure-agenta" <br/>Izvorni kod: [GitHub](https://github.com/Azure/WALinuxAgent)
Ubuntu|Ubuntu 12.04 14.04, 16.04, 16.10 | U otklanjanje | Paket: U repo u odjeljku "walinuxagent" <br/>Izvorni kod: [GitHub](https://github.com/Azure/WALinuxAgent)


## <a name="partners"></a>Partneri

### <a name="openlogic"></a>OpenLogic
[http://www.openlogic.com/Azure](http://www.openlogic.com/azure)

OpenLogic je početne davatelja enterprise Otvori izvor rješenja u oblak i podatkovnog centra. OpenLogic olakšava stotine početnih enterprise preko širok raspon delatnosti sigurno Nabava, podržava i kontrolirati softver Otvori izvor. OpenLogic nudi komercijalne klase tehničke podrške i indemnification za 600 Otvori izvor pakete sigurnosno stručnjaka zajednica OpenLogic, uključujući enterprise razine podrške za CentOS te se pokretanje partnera za dohvat utemeljen na CentOS slike na Azure.

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/Running-coreos/Cloud-Providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

S web-mjesta CoreOS:

*CoreOS namijenjen je sigurnost, dosljednosti i pouzdanosti. Umjesto instalacije paketa putem yum ili Zemaljska CoreOS za upravljanje servisa više razine apstrakcije koristi spremnika Linux. Kod jedne usluge i sve ovisnosti pakirat je u spremniku koji se mogu se izvoditi na jedan ili više CoreOS.*


### <a name="credativ"></a>Credativ
[http://www.credativ.co.uk/credativ-blog/debian-Images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ je nezavisne Savjetodavne usluge tvrtke ili ustanove specializing u razvoju i implementacija rješenja profesionalni pomoću besplatan softver i. Kao početne specijalistima Otvori izvor, Credative ima međunarodni prepoznavanja s mnogo odjelima pomoću njihovih podrška. U kombinaciji s Microsoftom Credativ trenutno Priprema odgovarajuće Debian slike za Debian 8 (Jessie) i Debian prije 7 (Wheezy), koje su osmišljena pokrenuti Azure i možete jednostavno upravlja putem platforme. Credativ će podržavati i održavanje dugoročnu i za Azure do njegova Otvori izvor centri za podršku za ažuriranje Debian slika.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/Topics/Cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle, strategije je nudi širok portfelja rješenja za javne i privatne oblaka tijekom održavanja kupci odabir i fleksibilnosti u način na koji implementacija softver tvrtke Oracle Oracle oblaka, kao i druge oblaka.  Oracle, partnerstvo s Microsoftom omogućuje klijentima implementacije softver tvrtke Oracle u Microsoft javne i privatne oblaka pouzdanosti certifikata i podršku iz Oracle.  Oracle, izvršenja i ulaganja u rješenja Oracle javne i privatne oblaka se ne mijenja.

### <a name="red-hat"></a>Crvena razgovor
[http://www.redhat.com/en/partners/strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Na svijetu početne davatelja rješenja Otvori izvor crvena je vaša olakšava više od 90% Fortune 500 tvrtki riješiti izazove tvrtke, poravnanje njihove IT i strategije tvrtke i Priprema za buduće tehnologije. Crvena je vaša to unosom sigurnih rješenja putem modelu Otvori tvrtke i modelu jeftin, predvidljivi pretplate.

### <a name="suse"></a>SUSE
[http://www.suse.com/suse-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server na Azure je dokazana platformu koja omogućuje nadređenog pouzdanosti i sigurnost za cloud računalstvo. SUSE-svestrane Linux platforme jednostavno integrira pomoću servisa Azure oblaka izlaganje okruženju jednostavno moguće upravljati oblaka. I s više od 9,200 potvrđenog aplikacije iz 1.800 nezavisne proizvođači za SUSE Linux Enterprise Server, SUSE osigurava da radnih opterećenja radi podržane u podatkovnom centru može Povjerljivo uvesti na Azure.

### <a name="canonical"></a>Kanonski
[http://www.ubuntu.com/Cloud/Azure](http://www.ubuntu.com/cloud/azure)

Kanonski inženjerska i uspjeh otvorena zajednica korisnika upravljanja pogon Ubuntu korisnika u klijent, poslužitelj i oblaka računalstvo, uključujući servise u oblaku osobne kupcima. Kanonski na vidom sjedinjenu besplatne platformu u Ubuntu, s telefona na cloud s obitelji koherentnost sučelja za telefona, tableta, TV i radna površina, čini Ubuntu prvi izbor za raznih institucije davatelja javno oblaka proizvođači potrošačkog elektroničkom i u favorite među pojedinačne technologists.

S inženjerima i inženjerskih centri diljem svijeta Canonical jedinstveno smještena partnera proizvođači hardvera, davatelje usluga i softverski razvojni inženjeri da bi se prikazala Ubuntu rješenja koja oglašavanje, s PC-ja na poslužiteljima i ručnih uređaja.

