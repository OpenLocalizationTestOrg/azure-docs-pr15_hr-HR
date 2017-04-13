<properties 
   pageTitle="Razlučivost VMs i instance uloga"
   description="Naziv razlučivost scenariji za Azure IaaS, hibridnog rješenja, između servisa u oblaku za različite, servisa Active Directory i korištenje vlastitim DNS poslužitelj "
   services="virtual-network"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="telmos" />

# <a name="name-resolution-for-vms-and-role-instances"></a>Naziv razlučivost VMs i instance uloga

Ovisno o tome kako koristiti Azure za hostiranje IaaS, PaaS i hibridnog rješenja, morate dopustiti VMs i instance uloge koje ste stvorili za komunikaciju s drugom. Iako ovaj komunikacije možete učiniti i pomoću IP adrese, je mnogo jednostavnijim koristiti nazive koji možete jednostavno pamtiti, a zatim promijenite. 

Kada instance uloga i VMs smješten u Azure morate riješiti nazivi domena za interne IP adrese, mogu koristiti jedan od dva načina:

- [Razlučivanje naziva pod uvjetom Azure](#azure-provided-name-resolution)

- [Korištenje vlastite DNS poslužitelja razlučivanje naziva](#name-resolution-using-your-own-dns-server) (koje možda uzlazna upite za navedene za Azure DNS poslužitelji) 

Vrsta razlučivanje naziva koristite ovisi o tome kako VMs i uloga instance morate međusobno komunicirati.

**Sljedeća tablica prikazuje scenariji i odgovarajuće ime razlučivost rješenja:**

| **Scenarij** | **Rješenja** | **Nastavak** |
|--------------|--------------|----------|
| Razlučivanje naziva između uloga instance ili VMs koja se nalazi u istom servis u oblaku ili virtualne mreže | [Razlučivanje naziva pod uvjetom Azure](#azure-provided-name-resolution)| Naziv glavnog računala ili FQDN-a |
| Razlučivanje naziva između uloga instance ili VMs koja se nalazi u različitim virtualne mreže | Korisnička upravlja DNS poslužitelji prosljeđivanje upita između vnets za razlučivanje po Azure (proxy DNS-a).  u odjeljku [naziv razlučivost korištenjem vlastite DNS poslužitelja](#name-resolution-using-your-own-dns-server)| Samo FQDN |
| Razrješavanje naziva lokalnog računala i servisa iz uloge instanci ili VMs u Azure | Korisnička upravlja DNS poslužitelji (kontroler domene npr na lokaciji, kontroler lokalne domene samo za čitanje ili DNS sekundarne sinkronizirali pomoću prijenosi zone).  U odjeljku [naziv razlučivost korištenjem vlastite DNS poslužitelja](#name-resolution-using-your-own-dns-server)|Samo FQDN |
| Rješenja Azure hostnames s lokalnog računala | Prosljeđivanje upita klijenta upravlja DNS-a proxy poslužitelja u odgovarajuće vnet proxy poslužitelj prosljeđuje upita Azure za rješavanje. U odjeljku [naziv razlučivost korištenjem vlastite DNS poslužitelja](#name-resolution-using-your-own-dns-server)| Samo FQDN |
| Obrnutom DNS-om za interne IP-ovi | [Razlučivanje naziva korištenjem vlastite DNS poslužitelja](#name-resolution-using-your-own-dns-server) | n/d |
| Razlučivanje naziva između VMs ili uloga instance koja se nalazi u oblak različite servise, a ne u virtualne mreže| Nije primjenjivo. Povezivanje između VMs i uloga instance servise u oblaku drugi nije podržano izvan virtualne mreže.| n/d |



## <a name="azure-provided-name-resolution"></a>Razlučivanje naziva pod uvjetom Azure

Uz razlučivosti javno DNS naziva, Azure nudi rješenje naziv internog VMs i instance uloge koje se nalaze u istom virtualne mreže ili servis u oblaku.  VMs/instance u oblaku dijeliti iste kratice DNS (tako da je dovoljno samostalno naziv glavnog računala), ali u različitim oblaka klasični virtualne mreže servisa imaju različite DNS nastavke pa FQDN je potreban za razrješavanje imena između različitih oblaka servisa.  U virtualne mreže u modelu implementaciju upravljanja resursima DNS sufiks je dosljedan putem virtualne mreže (pa nije potrebna FQDN) i DNS naziva se mogu dodijeliti NIC-ovi i VMs. Iako razlučivanje naziva pod uvjetom Azure ne zahtijeva sve konfiguracije, nije odgovarajući izbor za sve scenariji za implementaciju, kao što se vidi na prethodno navedenoj tablici.

> [AZURE.NOTE] U slučaju uloge web i tempiranja, možete pristupiti i interne IP adrese instanci uloga na temelju uloga naziva te instance broj koristeći Azure usluga upravljanja REST API-JA. Dodatne informacije potražite u članku [Servis za upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/ee460799.aspx).

### <a name="features-and-considerations"></a>Značajke i napomene

**Značajke:**

- Lakoća korištenja: nema konfiguracije je potrebna lozinka koristi naziv pod uvjetom Azure razlučivost.

- Servis za rješenja navedene za Azure naziva dostupna iznimno, spremanje potrebe za stvaranje i upravljanje klastere DNS poslužitelja.

- Može se koristiti u kombinaciji s vlastitim DNS poslužitelji za rješavanje na lokaciji i Azure hostnames.

- Razlučivanje naziva navedeni su između uloga instance/VMs unutar iste servis u oblaku bez potrebe za na FQDN.

- Razlučivanje naziva navedeni su između VMs u virtualne mreže koje koriste model implementacije Voditelj resursa, bez potrebe za FQDN. Virtualne mreže u modelu uvođenje klasičnog zahtijevaju FQDN kada raščlanjuju imena u oblak različite servise. 

- Možete koristiti hostnames koji najbolje opisuje implementacije, umjesto rad s nazivima automatski generirani.

**Pitanja vezana uz:**

- Azure stvara DNS sufiks nije moguće mijenjati.

- Ručno ne može registrirati vlastite zapise.

- Ima PREDNOST i NetBIOS nisu podržani. (Ne vidite svoje VMs u programu Windows Explorer.)

- Hostnames mora biti kompatibilan s preglednikom DNS (ih morate koristiti samo 0 do 9, a z i "-", i ne možete pokretati ni završavati na "-". U odjeljku RFC 3696 2.)

- Promet upita DNS ograničio je vrijeme za svaki VM. To ne utječe na većini aplikacija.  Ako je zahtjev za ograničavanje opaženih, provjerite je li klijentsko predmemoriranje omogućen.  Dodatne informacije potražite u članku [u potpunosti iskoristite razlučivanje naziva pod uvjetom Azure](#Getting-the-most-from-Azure-provided-name-resolution).

- VMs u prvom servise u oblaku 180 registrirane za svaki virtualne mreže u modelu klasični implementacije. To se odnosi na virtualne mreže u modelima implementacije Voditelj resursa.


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Početak iz navedene za Azure naziv razlučivost
**Klijentsko predmemoriranje:**

Ne svaki upit DNS-a mora biti poslana putem mreže.  Predmemoriranje klijentsko omogućuje smanjivanje Latencija i poboljšati resilience na mreži blips rješavanje ponavljajuća DNS upite iz lokalne predmemorije.  DNS zapisi sadrže na vrijeme važenja (TTL) koji omogućuje predmemoriju za pohranu zapis za dugo moguće bez utjecaja zapisa svježina pa predmemoriranje klijentsko nije prikladna za većinu.

Zadani klijent DNS-a za Windows sadrži ugrađeni DNS predmemoriju.  Neke Linux distros obuhvaćaju predmemoriranje prema zadanim postavkama, preporučuje se da nešto moguće dodati u svakom VM Linux (nakon provjere da nema lokalne predmemorije već).

Postoji nekoliko različitih DNS predmemoriranja paketa dostupna, primjerice dnsmasq, Evo nekoliko koraka da biste instalirali dnsmasq na najčešće distros:

- **Ubuntu (koristi resolvconf)**:
    - samo instalirajte paket dnsmasq ("sudo Zemaljska get Instaliraj dnsmasq").
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

> [AZURE.NOTE] Paket 'dnsmasq' samo jedan je od mnogo DNS predmemorije za Linux.  Prije korištenja, provjerite je li njegov prikladnosti za konkretne potrebe i koji je instaliran bez predmemorije.

**Ponovne pokušaje klijentsko:**

DNS je prvenstveno UDP protokola.  Kao što je UDP protokola ne jamči Isporuka poruke, pokušaj logike je riješiti protokol DNS sam.  Svaki klijent DNS (operacijski sustav) možete imati različite pokušaj logike ovisno o preferenca kreatora:

 - Operacijski sustavi Windows ponovno nakon 1 drugi, a zatim ponovno nakon drugog 2, 4 i drugi 4 sekunde. 
 - U zadanom Linux postavljanje ponovne pokušaje nakon 5 sekundi.  Preporučuje se da biste promijenili to pokušati 5 puta intervalima 1 drugog.  

Da biste provjerili trenutne postavke na Linux VM, "mačka /etc/resolv.conf" i pogledajte u retku "Mogućnosti", npr.:

    options timeout:1 attempts:5

Datoteka resolv.conf je obično automatski generirani i nije moguće uređivati.  Određene korake za dodavanje u retku "Mogućnosti" razlikuju se po distro:

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
Postoji nekoliko situacijama gdje je razlučivost potrebama naziv možda otvorite izvan značajke nudi Azure, primjerice kada pomoću Active Directory domains ili kada tražite Razrješavanje DNS između virtualne mreže (vnets).  Da biste Popratno scenarija Azure pruža mogućnost za korištenje DNS poslužitelja.  

DNS poslužitelji unutar virtualne mreže možete proslijediti DNS upite Azure je rekurzivna resolvers da biste riješili hostnames unutar tog virtualne mreže.  Na domeni kontroleru (Kontroler) radi u Azure, na primjer, možete odgovoriti DNS upite za njegov domene i druge upite proslijediti Azure.  Time se omogućuje VMs da biste vidjeli i resursima na lokaciji (putem Kontroler) i pod uvjetom Azure hostnames (putem forwarder).  Pristup resolvers rekurzivne Azure na navedeni su putem virtualne IP 168.63.129.16.

DNS prosljeđivanja omogućuje Razrješavanje DNS suč vnet i omogućuje na računalima na lokaciji da biste riješili navedene za Azure hostnames.  Da biste riješili naziv glavnog računala na VM, DNS poslužitelj VM moraju se nalaziti u istoj virtualne mreže i konfigurirati za prosljeđivanje naziv glavnog računala upite za Azure.  Kao što je DNS sufiks razlikuje se u svakom vnet, možete koristiti pravila za uvjetno prosljeđivanje da biste poslali DNS upite ispravne vnet za rješavanje.  Sljedeća slika prikazuje dvije vnets i na mreži na lokaciji način Razrješavanje DNS suč vnet koristite taj način.  Na primjer DNS forwarder dostupna je u [galeriji predložaka za brzi početak rada Azure](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) i [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![Suč vnet DNS-a](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Kada se koristi naziv pod uvjetom Azure razlučivosti, na interni DNS sufiks (*. internal.cloudapp.net) nalazi se svaki VM koristeći DHCP.  Naziv glavnog računala razlučivost omogućuje kao što su zapisi glavnog računala u zoni internal.cloudapp.net.  Kada koristite vlastiti naziv razlučivost rješenja, sufiks IDN-ovi nije naveden na VMs jer ometa druge arhitekturi DNS (kao što je domena pridruženo scenariji).  Umjesto toga dajemo rezerviranog mjesta koje nisu funkcionira (reddog.microsoft.com).  

Ako je potrebno, interne DNS nastavak možete odrediti pomoću programa PowerShell ili na API-JA:

-  Za virtualne mreže u modelima implementaciju upravljanja resursima sufiks je dostupan putem cmdlet [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) ili putem resursa za [mrežna kartica](https://msdn.microsoft.com/library/azure/mt163668.aspx) .    
-  U modelima uvođenje klasičnog je dostupan putem poziv [Dobiti API za implementaciju](https://msdn.microsoft.com/library/azure/ee460804.aspx) ili putem sufiks na [Get-AzureVM-ispravljanje pogrešaka](https://msdn.microsoft.com/library/azure/dn495236.aspx) cmdlet.


Ako je prosljeđivanje upita Azure ne odgovara vašim potrebama, morat ćete unijeti rješenje DNS-a.  Morat ćete je rješenje DNS:

-  Navedite razlučivost odgovarajući naziv glavnog računala, primjerice putem [DDNS](virtual-networks-name-resolution-ddns.md).  Napomena, ako koristite DDNS možda ćete morati onemogućiti DNS zapis scavenging kao zakupa Azure, DHCP poslužitelja vrlo dugo i scavenging prerano mogu ukloniti DNS zapisa. 
-  Navedite odgovarajuće rekurzivne razlučivost omogućuje razrješenje naziva vanjskih domena.
-  Biti dostupno (TCP i UDP na priključak 53) klijenata ga služi i moći pristupiti s Internetom.
-  Zaštiti od programa access s Interneta da biste smanjili prijetnji posed tako da vanjski agenata.

> [AZURE.NOTE] Ostvarili najbolje performanse, prilikom korištenja Azure VMs kao DNS poslužitelji, biti onemogućena IPv6 i [Instancu razinom javnu IP](virtual-networks-instance-level-public-ip.md) trebaju biti dodijeljeni svaki DNS poslužitelju VM.  Ako odaberete da biste koristili Windows Server kao DNS poslužitelj, [Ovaj članak](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) sadrži dodatne performanse analizu i optimizacije.


### <a name="specifying-dns-servers"></a>Određivanje DNS poslužitelja

Koristite li sami DNS poslužitelji, Azure omogućuje određivanje više DNS poslužitelji po virtualne mreže ili po mrežnog sučelja (Voditelj resursa) / (klasični) servis u oblaku.  DNS poslužitelji za sučelje za usluge/mreže oblaka preuzimanje prednost onih koji su navedeni za virtualne mreže.

> [AZURE.NOTE] Mreža svojstva veze, kao što je DNS poslužitelj IP-ovi, se ne smije uređivati izravno u sustavu Windows VMs kao što je možda se brišu tijekom servisa heal kada se zamjenjuje prilagodnik virtualne mreže. 


Prilikom korištenja modela implementacije Voditelj resursa, DNS poslužitelji u moguće navesti portala za API/predloške ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [nic](https://msdn.microsoft.com/library/azure/mt163668.aspx)) ili PowerShell ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [nic](https://msdn.microsoft.com/library/mt619370.aspx)).

Prilikom korištenja modela klasični implementaciju, DNS poslužitelji za virtualne mreže mogu biti naveden u Portal ili [ *Mrežna konfiguracija* datoteku](https://msdn.microsoft.com/library/azure/jj157100).  Za servise u oblaku, DNS poslužitelji određeni su klauzulom putem [ *Servisa konfiguracijska* datoteka](https://msdn.microsoft.com/library/azure/ee758710) ili u ljusci PowerShell ([Novo AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [AZURE.NOTE] Ako promijenite postavke DNS-a za virtualne mreže/virtualne stroju koji je već ste implementirali ćete morati ponovno pokrenuti svaki problematične VM da bi promjene stupile na snagu.


## <a name="next-steps"></a>Daljnji koraci

Model implementacije Voditelj resursa:

- [Stvaranje ili ažuriranje virtualne mreže](https://msdn.microsoft.com/library/azure/mt163661.aspx)
- [Stvaranje ili ažuriranje mrežna kartica](https://msdn.microsoft.com/library/azure/mt163668.aspx)
- [Novi AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
- [Novi AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

 
Uvođenje klasičnog modela:

- [Shema konfiguracija servisa Azure](https://msdn.microsoft.com/library/azure/ee758710)
- [Shema Konfiguracija virtualne mreže](https://msdn.microsoft.com/library/azure/jj157100)
- [Konfiguriranje virtualne mreže pomoću mreže konfiguracijska datoteka](virtual-networks-using-network-configuration-file.md) 

