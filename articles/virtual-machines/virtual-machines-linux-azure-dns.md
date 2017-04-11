<properties 
   pageTitle="Mogućnosti za razlučivanje imena DNS-a za Linux VMs servisu Azure"
   description="Naziv razlučivost scenariji za Linux VMs u IaaS Azure, uključujući navedene DNS servise, hibridnog vanjskog DNS-a i unijeti na vaše vlastite DNS poslužitelja."
   services="virtual-machines"
   documentationCenter="na"
   authors="RicksterCDN"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="rclaus" />

# <a name="dns-name-resolution-options-for-linux-vms-in-azure"></a>Mogućnosti za razlučivanje imena DNS-a za Linux VMs servisu Azure

Azure nudi Razrješavanje DNS naziva prema zadanim postavkama za sve VMs sadržana u jednom virtualne mreže. Vi ste moći implementacija rješenja za razlučivost naziv vlastite DNS konfiguriranjem DNS servise na vašem Azure VMs glavnom računalu. Sljedećim scenarijima bi vam pomoći da odaberete onu radi bolje određeni situaciji.

- [Razlučivanje naziva pod uvjetom Azure](#azure-provided-name-resolution)

- [Razlučivanje naziva korištenjem vlastite DNS poslužitelja](#name-resolution-using-your-own-dns-server) 

Vrsta razlučivanje naziva koristite ovisi o tome kako VMs i uloga instance morate međusobno komunicirati.

**Sljedeća tablica prikazuje scenariji i odgovarajuće ime razlučivost rješenja:**

| **Scenarij** | **Rješenja** | **Nastavak** |
|--------------|--------------|----------|
| Razlučivanje naziva između uloga instance ili VMs koja se nalazi u istom virtualne mreže | [Razlučivanje naziva pod uvjetom Azure](#azure-provided-name-resolution)| Naziv glavnog računala ili FQDN-a |
| Razlučivanje naziva između uloga instance ili VMs koja se nalazi u različitim virtualne mreže | Korisnička upravlja DNS poslužitelji prosljeđivanje upita između vnets za razlučivanje po Azure (proxy DNS-a).  u odjeljku [naziv razlučivost korištenjem vlastite DNS poslužitelja](#name-resolution-using-your-own-dns-server)| Samo FQDN |
| Razrješavanje naziva lokalnog računala i servisa iz uloge instanci ili VMs u Azure | Korisnička upravlja DNS poslužitelji (Ako, na primjer, kontroler domene na lokaciji, kontroler lokalne domene samo za čitanje ili DNS sekundarne sinkronizirali pomoću prijenosi zone).  U odjeljku [naziv razlučivost korištenjem vlastite DNS poslužitelja](#name-resolution-using-your-own-dns-server)|Samo FQDN |
| Rješenja Azure hostnames s lokalnog računala | Prosljeđivanje upita klijenta upravlja DNS-a proxy poslužitelja u odgovarajuće vnet proxy poslužitelj prosljeđuje upita Azure za rješavanje. U odjeljku [naziv razlučivost korištenjem vlastite DNS poslužitelja](#name-resolution-using-your-own-dns-server)| Samo FQDN |
| Obrnutom DNS-om za interne IP-ovi | [Korištenje vlastite DNS poslužitelja razlučivanje naziva](#name-resolution-using-your-own-dns-server) | n/d |

## <a name="azure-provided-name-resolution"></a>Razlučivanje naziva pod uvjetom Azure

Uz razlučivosti javno DNS naziva, Azure nudi rješenje naziv internog VMs i instance uloge koje se nalaze u istom virtualne mreže.  U na ARM-virtualne mreže DNS-a sufiks je dosljedan putem virtualne mreže (pa nije potrebna FQDN) i DNS naziva se mogu dodijeliti NIC-ovi i VMs. Iako razlučivanje naziva pod uvjetom Azure ne zahtijeva sve konfiguracije, nije odgovarajući izbor za sve scenariji za implementaciju, kao što se vidi u prethodnoj tablici.

### <a name="features-and-considerations"></a>Značajke i napomene

**Značajke:**

- Lakoća korištenja: nema konfiguracije potreban je za korištenje razlučivanje naziva pod uvjetom Azure.

- Servis za rješenja navedene za Azure naziva dostupna iznimno, spremanje potrebe za stvaranje i upravljanje klastere DNS poslužitelja.

- Može se koristiti uz DNS poslužitelja za rješavanje na lokaciji i Azure hostnames.

- Razlučivanje naziva navedeni su između VMs u virtualne mreže bez potrebe za FQDN. 

- Možete koristiti hostnames koji najbolje opisuje implementacije, umjesto rad s nazivima automatski generirani.

**Pitanja vezana uz:**

- Azure stvara DNS sufiks nije moguće mijenjati.

- Ručno ne može registrirati vlastite zapise.

- Ima PREDNOST i NetBIOS nisu podržani.

- Hostnames mora biti kompatibilan s preglednikom DNS (ih morate koristiti samo 0 do 9, a z i "-", i ne možete pokretati ni završavati na "-". U odjeljku RFC 3696 2.)

- Promet upita DNS ograničio je vrijeme za svaki VM. To ne utječe na većini aplikacija.  Ako je zahtjev za ograničavanje opaženih, provjerite je li klijentsko predmemoriranje omogućen.  Dodatne informacije potražite u članku [u potpunosti iskoristite razlučivanje naziva pod uvjetom Azure](#Getting-the-most-from-Azure-provided-name-resolution).


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Početak iz navedene za Azure naziv razlučivost
**Klijentsko predmemoriranje:**

Nije svaka DNS upit se šalje putem mreže.  Predmemoriranje klijentsko omogućuje smanjivanje Latencija i poboljšati resilience na mreži blips rješavanje ponavljajuća DNS upite iz lokalne predmemorije.  DNS zapisi sadrže na vrijeme važenja (TTL) koji omogućuje predmemoriju za pohranu zapis za dugo moguće bez utjecaja svježina zapisa.  Zbog toga klijentsko predmemoriranje je odgovarajuću u većini slučajeva.

Neke distros Linux obuhvaćaju predmemoriranje prema zadanim postavkama.  Preporučuje se dodavanje jedan za svaki VM Linux (nakon provjere da nema lokalne predmemorije već).

Postoji nekoliko različitih DNS predmemoriranja paketa dostupna, primjerice dnsmasq, Evo nekoliko koraka da biste instalirali dnsmasq na najčešće distros:

- **Ubuntu (koristi resolvconf)**:
    - instalirajte paket dnsmasq ("sudo Zemaljska get Instaliraj dnsmasq").
- **SUSE (koristi netconf)**:
    - instalirajte paket dnsmasq ("sudo zypper Instaliraj dnsmasq") 
    - omogućite servis za dnsmasq ("systemctl Omogući dnsmasq.service") 
    - pokretanje servisa dnsmasq ("systemctl start dnsmasq.service") 
    - Uređivanje "/ itd/sysconfig/mreže/config" i promijenite NETCONFIG_DNS_FORWARDER = "" u "dnsmasq"
    - Ažuriranje resolv.conf ("netconfig ažuriranje") da biste postavili predmemoriju kao lokalne Razrješavanje DNS-a
- **OpenLogic (koristi NetworkManager)**:
    - instalirajte paket dnsmasq ("sudo yum Instaliraj dnsmasq")
    - omogućite servis za dnsmasq ("systemctl Omogući dnsmasq.service")
    - pokretanje servisa dnsmasq ("systemctl start dnsmasq.service")
    - Dodavanje "prepend poslužitelje naziva domene-127.0.0.1;" u "/etc/dhclient-eth0.conf"
    - ponovno pokrenite servis za mreže ("servisa mreže ponovno pokretanje") da biste postavili predmemoriju kao lokalne Razrješavanje DNS-a

> [AZURE.NOTE]: Paket za 'dnsmasq' samo jedan je od mnogo DNS predmemorije za Linux.  Prije korištenja, provjerite njezinu prikladnosti za konkretne potrebe i koji je instaliran bez predmemorije.

**Ponovne pokušaje klijentsko:**

DNS je prvenstveno UDP protokola.  Kao što je UDP protokola ne jamči Isporuka poruke, pokušaj logike je riješiti protokol DNS sam.  Svaki klijent DNS (operacijski sustav) možete imati različite pokušaj logike ovisno o preferenca kreatora:

 - Operacijski sustavi Windows ponovno nakon nešto drugo, a zatim ponovno nakon drugog dvije, četiri i drugi četiri sekunde. 
 - U zadanom Linux postavljanje ponovne pokušaje nakon pet sekundi.  Trebali biste promijeniti tako da ponovite pet puta intervalima jedne sekunde.  

Da biste provjerili trenutne postavke na Linux VM, "mačka /etc/resolv.conf" i pogledajte u retku "Mogućnosti", na primjer:

    options timeout:1 attempts:5

Datoteka resolv.conf automatski se generira i nije moguće uređivati.  Određene korake za dodavanje u retku "Mogućnosti" razlikuju se po distro:

- **Ubuntu** (koristi resolvconf):
    - Dodavanje retka mogućnosti "/ etc/resolveconf/resolv.conf.d/head" 
    - Pokrenite "resolvconf -u" da biste ažurirali
- **SUSE** (koristi netconf):
    - dodati "prilikom pokušaja timeout:1: 5" u NETCONFIG_DNS_RESOLVER_OPTIONS = "" parametar u '/ itd/sysconfig/mreže/config' 
    - Pokrenite "netconfig ažuriranje" za ažuriranje
- **OpenLogic** (koristi NetworkManager):
    - Dodavanje "jeka"Mogućnosti timeout:1 pokušaja: 5"" "/ etc/NetworkManager/dispatcher.d/11-dhclient" 
    - Pokrenite "servisa ponovno pokretanje mrežom" da biste ažurirali

## <a name="name-resolution-using-your-own-dns-server"></a>Razlučivanje naziva korištenjem vlastite DNS poslužitelja
Postoji nekoliko situacija u kojima vašim potrebama razlučivost naziv možda okružje značajke pod uvjetom da po Azure, primjerice kada tražite Razrješavanje DNS između virtualne mreže (vnets).  Da prekrije scenarij Azure pruža mogućnost za korištenje DNS poslužitelja.  

DNS poslužitelji unutar virtualne mreže možete proslijediti DNS upite Azure je rekurzivna resolvers da biste riješili hostnames unutar tog virtualne mreže.  DNS poslužitelju koji se izvodi u Azure, na primjer, možete odgovoriti DNS upite za vlastitu datoteka zone DNS-a i druge upite proslijediti Azure.  Time se omogućuje VMs da biste vidjeli oba svoje stavke u vašim datotekama zone i pod uvjetom Azure hostnames (putem forwarder).  Pristup resolvers rekurzivne Azure na navedeni su putem virtualne IP 168.63.129.16.

DNS prosljeđivanja omogućuje Razrješavanje DNS suč vnet i omogućuje na računalima na lokaciji da biste riješili navedene za Azure hostnames.  Da biste riješili naziv glavnog računala na VM, DNS poslužitelj VM moraju se nalaziti u istoj virtualne mreže i konfigurirati za prosljeđivanje naziv glavnog računala upite za Azure.  Kao što je DNS sufiks razlikuje se u svakom vnet, možete koristiti pravila za uvjetno prosljeđivanje da biste poslali DNS upite ispravne vnet za rješavanje.  Sljedeća slika prikazuje dva vnets i na mreži na lokaciji način Razrješavanje DNS suč vnet koristite taj način:

![Suč vnet DNS-a](./media/virtual-machines-linux-azure-dns/inter-vnet-dns.png)

Kada se koristi naziv pod uvjetom Azure razlučivost interne DNS nastavak dostupni su svaki VM koristeći DHCP.  Kada koristite vlastiti naziv razlučivost rješenja, ovaj sufiks nije naveden na VMs jer ometa druge arhitekturi DNS-a.  Da biste se pozvali na računalima po FQDN ili da biste konfigurirali sufiks vaše VMs sufiks možete odrediti pomoću programa PowerShell ili na API-JA:

-  Za upravljanje resursima Azure upravlja vnets, sufiks je dostupan putem resursa za [mrežna kartica](https://msdn.microsoft.com/library/azure/mt163668.aspx) ili pokrenete naredbu `azure network public-ip show <resource group> <pip name>` da biste prikazali detalje o javnu IP, uključujući FQDN na nic.    


Prosljeđivanje upita Azure ne odgovara vašim potrebama, morate unijeti rješenje DNS-a.  Mora DNS rješenje:

-  Navedite razlučivost odgovarajući naziv glavnog računala, primjerice putem [DDNS](../virtual-network/virtual-networks-name-resolution-ddns.md).  Napomena, ako koristite DDNS možda ćete morati onemogućiti DNS zapis scavenging kao zakupa Azure, DHCP poslužitelja vrlo dugo i scavenging prerano mogu ukloniti DNS zapisa. 
-  Navedite odgovarajuće rekurzivne razlučivost omogućuje razrješenje naziva vanjskih domena.
-  Biti dostupno (TCP i UDP na priključak 53) klijenata ga služi i moći pristupiti s Internetom.
-  Zaštiti od programa access s Interneta da biste smanjili prijetnji posed tako da vanjski agenata.

> [AZURE.NOTE] Ostvarili najbolje performanse, prilikom korištenja Azure VMs kao DNS poslužitelji, biti onemogućena IPv6 i [Instancu razinom javnu IP](../virtual-network/virtual-networks-instance-level-public-ip.md) trebaju biti dodijeljeni svaki DNS poslužitelju VM.  

