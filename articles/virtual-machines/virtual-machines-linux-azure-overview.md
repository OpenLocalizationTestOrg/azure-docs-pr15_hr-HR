 <properties
   pageTitle="Azure i Linux | Microsoft Azure"
   description="U članku se opisuje izračunati Azure, prostora za pohranu i mrežne usluge s ugovorima o virtualnim strojevima Linux."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines-linux"
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/14/2016"
   ms.author="v-livech"/>

# <a name="azure-and-linux"></a>Azure i Linux

Microsoft Azure je sve veći skup integrirani javnog servisa uključujući analize, virtualnim strojevima baze podataka, mobilne umrežavanje prostor za pohranu u oblaku i web-– prikladno za hostiranje vaše rješenja.  Microsoft Azure nudi skalabilni računalno platformu koja omogućuje vam da samo platiti koje koristite, kada to želite - bez potrebe za ulažete u lokalni hardvera.  Azure spremna je kada se skaliranja vašeg rješenja i odgovor na bilo kakve skaliranje potrebni za potrebe klijentima za servis.

Ako ste upoznati s različite značajke Amazon na AWS, možete provjeriti AWS Dodavanje veze za Azure vanjskih [definicija mapiranja dokumenta](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).


## <a name="regions"></a>Područja
Resursi za Microsoft Azure su raspodijeliti više zemljopisnim područjima diljem svijeta.  "Regija" predstavlja centre za više podataka u jednom zemljopisnom području.  Od siječnja 1, 2016, to uključuje: 8 u Americi, 2 u Europi, 6 u države Pacifičke 2 u Kini i 3 u Indija.  Ako želite cjelovit popis svih Azure područja, ne možemo imaju popis postojeće i nove objaviti područja.

- [Azure regije](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Dostupnost
Redoslijedom za implementaciju sustava da biste potvrdili za naše 99.95 VM razinu ugovor o usluzi, morate uvesti dva ili više VMs izvodi svoje radno opterećenje unutar raspoloživost postavljanje. To će osigurati svoje VMs su raspodijeliti više domena kvara u našem podataka kao i implementiran na glavno računalo sa sustavom windows različite održavanja. Potpuna [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) objašnjava sigurno dostupnost Azure kao cjelinu.

## <a name="azure-virtual-machines--instances"></a>Azure virtualnim strojevima i instanci
Microsoft Azure podržava izvodi brojne popularne distribucija Linux omogućuje i održava broj partnera.  Tražit će distribucija kao što je Enterprise je vaša crveno, CentOS, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD i drugih značajki u trgovine Windows Azure. Aktivno radimo s različitim Linux zajednice da biste dodali još više flavors popis [Azure licencira Linux Distros](virtual-machines-linux-endorsed-distros.md) .

Ako je Preferirani Linux distro odabranih trenutno su prisutne u galeriji, možete "prenijeti vlastite Linux" VM [stvaranjem](virtual-machines-linux-create-upload-generic.md)i prijenos Linux VHD u Azure.

Azure virtualnim strojevima omogućuju implementaciju širok raspon računalstvo rješenja agilno način. Možete implementirati gotovo bilo kojeg radno opterećenje i bilo koji jezik na gotovo bilo kojeg operacijski sustav – Windows, Linux, ili prilagođeni stvorili s bilo koje od popisu sve veći od partnera. I dalje ne možete pronaći ono što tražite?  Ne brinite – iz lokalnog možete prenijeti vlastite slike.

## <a name="vm-sizes"></a>Veličina VM
Ako pokrenete VM u Azure, prilikom prelaska da biste odabrali veličina VM unutar nešto niza naš veličina prikladan za svoje radno opterećenje. Veličina utječe i obrada power, memorije i prostora za pohranu kapacitet virtualnog računala. Se naplaćuju koji se temelji na vrijeme na VM je operacijski sustav, a troše svojih dodijeljene resursa. Potpuni popis veličina [virtualnih računala](virtual-machines-linux-sizes.md).

Evo nekoliko osnovnih smjernica za odabir veličina VM neku od naše niza (A, D, DS, G i Oznaka).

* A niz VMs su naš vrijednost cjenovnom entry-level VMs za osnovnu radnih opterećenja i scenariji razvojni i testiranje. Široko su dostupni u svim regijama i mogu povezati i koristiti sve standardne dostupnih resursa na virtualnim računalima.
* Veličina odgovora niza (A8 - A11) su posebno računalnim intenzivno konfiguracije odgovarajuću visokih performansi računalno klaster aplikacijama.
* D niz VMs dizajnirani za pokretanje aplikacije koje potražnje veći računalnim power i performanse privremene diska. D niz VMs pružaju procesora koji se brže, veća omjer memorije core i solid-state pogon (SSD) za privremene disk.
* Dv2 niz je najnovija verzija naš D niza, značajke jače procesora. CPU Dv2 niz je otprilike 35% brže nego procesora D niz. Se temelji na najnovijih 2,4 GHz Intel Xeon® E5-2673 v3 procesor (Haskell) i s tehnologije 2.0 Intel Turbo isticanja možete prijeći do 3.2 GHz. Dv2 niz ima isti konfiguracije memorije i disk kao D niz.
* G niz VMs nude najviše memorije i pokrenuti hosts koji imaju obiteljski procesori Intel Xeon E5 V3.

Napomena: DS niza i Oznaka niz VMs imati pristup Premium prostora za pohranu – naš SSD sigurnosno visokih performansi, najniža Latencija prostor za pohranu/i intenzivno radnih opterećenja. Prostor za pohranu Premium dostupna je u određenim regijama. Detalje potražite u članku:

- [Prostor za pohranu Premium: Visokih performansi prostor za pohranu radnih opterećenja Azure virtualnog računala](../storage/storage-premium-storage.md)

## <a name="automation"></a>Automatizacija
Da biste postigli proper kulture DevOps, infrastruktura za sve mora biti kod.  Kada sve Infrastruktura nalazi se u kodu jednostavno može ponovno stvoriti (Phoenixu poslužitelji).  Azure funkcionira s tooling kao što su Ansible, Chef, SaltStack i Puppet glavne automatizaciju.  Azure sadrži i vlastitu tooling za automatizaciju:

- [Predlošci za Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

- [Azure VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

Azure vodoravnim je izvan podrška za [oblak pokretanja](http://cloud-init.io/) preko Većina Linux Distros koji podržavaju ga.  Trenutno Canonical na Ubuntu VMs uvode se sa oblaka pokretanja po zadanom omogućena.  RedHats RHEL, CentOS i Fedora podržava oblaka pokretanja, no Azure slike održava RedHat imaju oblaka pokretanja instalacije.  Da biste koristili oblaka pokretanja na RedHat obitelj OS, morate stvoriti prilagođenu sliku s oblaka pokretanja instalacije.

- [Korištenje oblaka pokretanja na Azure Linux VMs](virtual-machines-linux-using-cloud-init.md)

## <a name="quotas"></a>Kvota
Azure pretplate ima ograničenja zadano mjesto na kojem se može utjecati na implementaciju velik broj VMs projekta. Trenutno ograničenje na temelju po pretplate je 20 VMs po regijama.  Ograničenja možete prijavio arhiviranja podršku ulaznice traži ograničenje povećava.  Dodatne informacije o ograničenja:

- [Ograničenja servisa Azure pretplate](../azure-subscription-service-limits.md)


## <a name="partners"></a>Partneri

Microsoft usko surađuje s našim partnerima da bi se slike dostupne su ažuriranja i optimiziran za Azure runtime.  Dodatne informacije o našim partnerima Provjerite svoje trgovine stranice.

- [Linux na Azure licencira distribucije](virtual-machines-linux-endorsed-distros.md)

Redhat - [Azure Marketplace - RedHat Enterprise Linux 7,2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Kanonski - [Azure Marketplace - poslužitelja Ubuntu 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian - [Azure Marketplace - Debian 8 "Jessie"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

CoreOS - [Azure Marketplace - CoreOS (stabilan)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami - [Bitnami biblioteka za Azure](https://azure.bitnami.com/)

Mesosphere - [Azure Marketplace - Mesosphere Kontroler/OS na Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Docker - [Azure Marketplace - servisa Azure kontejner s Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins - [Azure Marketplace - CloudBees Jenkins platforme](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)


## <a name="getting-setup-on-azure"></a>Dovršavanje postavljanja na Azure
Da biste počeli koristiti Azure potreban je račun za Azure, EŽA Azure instaliran, a par SSH javni i privatni ključevi.

## <a name="sign-up-for-an-account"></a>Registracija za račun
Prvi korak u oblak Azure pomoću se registrirati za račun za Azure.  Otvorite stranicu [Azure Prijava na račun](https://azure.microsoft.com/pricing/free-trial/) za početak rada.

## <a name="install-the-cli"></a>Instalacija sustava EŽA
S na novi račun Azure vam mogli početi s radom odmah pomoću portala Azure koji je na ploči koji se temelji na web administrator.  Da biste upravljali oblaka Azure putem naredbenog retka, instalirajte na `azure-cli`.  Instalirajte [Azure EŽA ](../xplat-cli-install.md)na vaše radne stanice Mac ili Linux.

## <a name="create-an-ssh-key-pair"></a>Stvaranje programa SSH par ključa
Sada imate račun za Azure, Azure web-portala i EŽA Azure.  Sljedeći je korak da biste stvorili par ključa programa SSH koji se koristi za SSH u Linux bez lozinke.  [Stvaranje SSH tipke na Linux i Mac](virtual-machines-linux-mac-create-ssh-keys.md) da biste omogućili lozinku manje prijave i bolje sigurnosti.

## <a name="getting-started-with-linux-on-microsoft-azure"></a>Uvod u Linux na Microsoft Azure
S postavu račun za Azure, Azure EŽA instalirana i SSH tipke stvorili sada ste spremni da biste počeli raditi izvan programa infrastrukture u Oblaku Azure.  Prvi zadatak je da biste stvorili nekoliko VMs.

## <a name="create-a-vm-using-the-cli"></a>Stvaranje VM pomoću na EŽA
Stvaranje Linux VM pomoću na EŽA je brz način implementacije u VM bez pomicanja na terminal u kojem radite.  Sve možete odrediti na web-portalu dostupan je putem naredbenog retka zastavice ili promjenu.  

- [Stvaranje Linux VM pomoću na EŽA](virtual-machines-linux-quick-create-cli.md)

## <a name="create-a-vm-in-the-portal"></a>Stvaranje na VM na portalu
Stvaranje Linux VM na portalu za Azure web je način pokažite i kliknite kroz različite mogućnosti da biste pristupili implementacije.  Umjesto naredbenog retka zastavice ili Parametri se mogu vidjeti na bolje web-izgled različite mogućnosti i postavki.  Sve dostupne putem sučelja naredbenog retka i dostupna je na portalu.

- [Stvaranje Linux VM pomoću portala](virtual-machines-linux-quick-create-portal.md)

## <a name="login-using-ssh-without-a-password"></a>Prijavite se pomoću SSH bez lozinke
Na VM sada radi Azure i spremni ste za prijavu.  Prijavite se putem SSH pomoću lozinke je nepouzdanog i dosta vremena.  Pomoću tipki SSH je najsigurnija način i najbrži način za prijavu.  Kada stvorite vam Linux VM putem portala sustava ili u EŽA, imate dvije mogućnosti za provjeru autentičnosti.  Ako odaberete lozinke za SSH, Azure konfigurira VM dopustili prijave putem lozinke.  Ako odlučite koristiti SSH javni ključ, Azure konfigurira VM dopustiti pristup samo prijave putem SSH tipke i onemogućuje prijave lozinku. Radi zaštite vaše VM Linux samo dopuštanjem SSH ključa prijave, koristite SSH javnog ključa mogućnost tijekom stvaranja VM portal ili EŽA.

- [Onemogućivanje SSH lozinke na vašem VM Linux konfiguriranjem SSHD](virtual-machines-linux-mac-disable-ssh-password-usage.md)

## <a name="related-azure-components"></a>Povezane Azure komponente

## <a name="storage"></a>Prostor za pohranu

- [Uvod u Microsoft Azure prostora za pohranu](../storage/storage-introduction.md)

- [Dodavanje na disku Linux VM pomoću eža azure](virtual-machines-linux-add-disk.md)

- [Kako priložiti podatkovni disk Linux VM na portalu za Azure](virtual-machines-linux-attach-disk-portal.md)

## <a name="networking"></a>Povezivanje s mrežom

- [Pregled virtualne mreže](../virtual-network/virtual-networks-overview.md)

- [IP adrese u Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md)

- [Otvaranje priključaka na Linux VM servisu Azure](virtual-machines-linux-nsg-quickstart.md)

- [Stvaranje potpuno kvalificirani naziv domene na portalu za Azure](virtual-machines-linux-portal-create-fqdn.md)


## <a name="containers"></a>Spremnika

- [Virtualnim strojevima i spremnika u Azure](virtual-machines-linux-containers.md)

- [Azure spremnik servisa Uvod](../container-service/container-service-intro.md)

- [Implementacija programa servisa Azure spremnik klaster](../container-service/container-service-deployment.md)

## <a name="next-steps"></a>Daljnji koraci

Sada imate pregled Linux na Azure.  Sljedeći je korak da biste prikazali i stvoriti nekoliko VMs!

- [Stvaranje Linux VM na Azure pomoću portala](virtual-machines-linux-quick-create-portal.md)

- [Stvaranje Linux VM na Azure pomoću na EŽA](virtual-machines-linux-quick-create-cli.md)
