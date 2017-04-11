<properties
   pageTitle="Implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN | Microsoft Azure"
   description="Kako implementirati arhitekturu sigurna mreža web-mjesto koji obuhvaća Azure virtualne mreže i lokalne mreže povezani putem virtualne privatne MREŽE."
   services=""
   documentationCenter="na"
   authors="RohitSharma-pnp"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="roshar"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-and-on-premises-vpn"></a>Implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN-a

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

U ovom se članku navode skup Savjeti za proširivanje lokalne mreže na Azure pomoću na web-mjesto virtualne privatne mreže (VPN-a). Promet se odvija između lokalne mreže i programa Azure virtualne mreže (VNet) putem programa tunelom IPSec VPN-a. U ovom arhitektura je prikladna za hibridno aplikacije sa sljedećim karakteristikama:

- Dijelovi aplikacije pokrenuti lokalnog drugima izlaganja preko Azure.

- Promet između lokalnog hardver i oblaka je vjerojatno će biti svijetlo, ili dobro je trgovanje malo prošireni Latencija fleksibilnost i obrada power oblaka.

- Prošireni mreže čine zatvoreni sustava. Postoji nema Izravni puta s Interneta da biste Azure VNet.

- Korisnici povezuju sa lokalnu mrežu za korištenje usluga smješten u Azure. Most između lokalne mreže i servise u Azure jednostavan je za korisnika.

Primjeri scenariji koje odgovaraju ovaj profil obuhvaćaju sljedeće:

- Redak tvrtke aplikacije koriste unutar tvrtke ili ustanove, gdje je u oblak migrirati dio funkciju.

- Test/razvoj/Laboratorija radnih opterećenja.

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Voditelj resursa Azure] [ resource-manager-overview] i classic. U ovom nacrt koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

## <a name="architecture-diagram"></a>Arhitektura dijagrama

Na sljedećem su dijagramu ističe komponente u toj arhitekturi:

> Visio dokument koji sadrži ovaj dijagram arhitektura dostupna je za preuzimanje na [Microsoftov centar za preuzimanje][visio-download]. Ovaj dijagram je na "hibridnog mreži - VPN" stranice.

![[0]][0]

- **Lokalne mreže.** Ovo je mreža računala i uređaja povezanih putem lokalnog područje privatnom radi unutar tvrtke ili ustanove.

- **Potražite VPN-a.** Ovo je uređaj ili servis koji omogućuje vanjske povezivost za lokalnu mrežu. Potražite VPN-a može biti hardverski uređaj ili može biti rješenje softver kao što su usmjeravanje i daljinski pristup servisa (RRAS) u sustavu Windows Server 2012.

> [AZURE.NOTE] Popis podržanih aparata VPN-a i informacije o konfiguriranju odabrane aparata VPN-a za povezivanje programa Azure VPN pristupnika, potražite u članku upute za odgovarajući uređaj na [popisu uređaja VPN podržava Azure][vpn-appliance].

- **Oblak N sloja aplikacija.** Ovo je aplikacija smješten u Azure. On može sadržavati više razine, s više podmreže povezanih putem balancers Azure Učitaj. Promet u svakom podmreže mogu se primjenjivati pravila definirane pomoću [Azure mreže sigurnosnih grupa (NSGs)][azure-network-security-group]. Dodatne informacije potražite u članku [Uvod u Microsoft Azure sigurnost][getting-started-with-azure-security].

> [AZURE.NOTE] U ovom se članku opisuju aplikacije oblaka kao jedan entitet. Izvodi [se arhitektura N sloja na Azure] [ implementing-a-multi-tier-architecture-on-Azure] detaljne informacije.

- **[Virtualne mreže (VNet)][azure-virtual-network].** Oblak aplikacije i komponente za pristupnik za Azure VPN-a koji se nalaze u istom VNet.

- **[Pristupnik za Azure VPN][azure-vpn-gateway].** Servis za VPN pristupnik omogućuje kojom ćete se povezati s VNet lokalne mreže putem VPN-a potražite. Dodatne informacije potražite u članku [Povezivanje lokalne mreže za Microsoft Azure virtualne mreže][connect-to-an-Azure-vnet]. Pristupnik za VPN sastoji se od sljedećih elemenata:

  - **Pristupnik virtualne mreže.** Ovo je resurs koji nudi virtualne VPN-a potražite na VNet. Nije odgovoran za usmjeravanje prometa iz lokalne mreže da biste na VNet.

  - **Pristupnik za lokalnu mrežu.** Ovo je programa apstrakcije VPN-a potražite lokalnog. Mrežni promet iz aplikacije oblaka za lokalne mreže je usmjerena kroz ovaj pristupnika.

  - **Veza.** Veza ima svojstva koja odredite vrstu veze (IPSec) i ključ koji se zajednički koriste s VPN-a potražite na lokalni šifriranje promet.

  - **Podmreže pristupnika.** Pristupnik za virtualne mreže čuva se vlastitu podmreže koje se mogu preduvjetima za različite, kao što je opisano u odjeljku preporuke.

- **Interna opterećenja.** Mrežni promet s pristupnika VPN usmjeravanja oblaka aplikaciju putem interne opterećenja. Raspoređivača opterećenja nalazi se u sučelja podmreži aplikacije.

## <a name="recommendations"></a>Preporuke

Azure nudi mnogo različitih resurse i vrste resursa, tako da ovu referenca arhitekture može dodjeli brojne načine. Ne možemo je navesti predložak Voditelj resursa Azure da biste instalirali arhitektura reference koja slijedi te preporuke. Ako se odlučite za stvaranje vlastite arhitektura referenca slijedite ove preporuke u osim ako imate određene zahtjeva koji ne stane i preporuke.

### <a name="vnet-and-gateway-subnet"></a>VNet i pristupnik podmreže

Stvaranje programa Azure VNet za čuvanje komponente u oblaku. Adresa prostor Azure VNet mora biti dovoljna kako bi odgovarao adrese koje koristi VMs i podmreže u na VNet. Prostor za adrese VNet provjerite ima li dovoljno prostora za rast ako dodatne VMs vjerojatno će biti potrebno u budućnosti. Adresa prostora na VNet se mora preklapati s lokalne mreže. Ako, na primjer, dijagramu iznad koristi prostor 10.20.0.0/16 adresa u VNet.

Stvaranje podmreže pod nazivom _GatewaySubnet_s adresom raspon /27. Ovaj podmreže potreban je pristupnik virtualne mreže i dodjeljivanje 32 adrese za ovaj podmreže će pomoći da radi s programima ograničenja veličine moguće pristupnika u budućnosti. Izbjegavajte stavljanje ovaj podmreže sredini prostor adrese. Preporučuje se da biste postavili prostor adrese za podmreže pristupnika na kraju gornje VNet adresni prostor. U primjeru prikazano u dijagramu pomoću 10.20.255.224/27.  Brzim postupkom da biste izračunali u CIDR je na sljedeći način:

1. Postavljanje varijable bitova u prostor adresu VNet 1 do bitova koji se koriste podmreže pristupnika, a zatim postavite preostalih bitova na 0.

2. Pretvaranje dobivene bitova u decimalni i express kao u adresu prostor sa skupom prefiks trajanje na željenu veličinu podmreže pristupnika.

Na primjer, za VNet s rasponu IP adresa od 10.20.0.0/16, Primjena korak #1 postaje 10.20.0b11111111.0b11100000.  Pretvaranje koji decimalni i kojem se iznosi ono je kao adresni prostor rezultira 10.20.255.224/27

> [AZURE.WARNING] Ne implementacija druge virtualnim strojevima ili uloga instance pristupnika podmreže. Osim toga, ne dodijelite programa NSG u ovom podmreži kao uzrokovat će pristupnika mogli prestati funkcionirati.

### <a name="virtual-network-gateway"></a>Pristupnik virtualne mreže

Dodijeliti javnu IP adresa za pristupnik za virtualne mreže.

Stvaranje pristupnika virtualne mreže u podmreže pristupnika i njezino dodavanje upravo dodijeljene javnu IP adresu. Koristite vrstu pristupnika koja najviše odgovara potrebama i koje je omogućena tako da na uređaj VPN:

Stvaranje [pravila utemeljen pristupnika] [ policy-based-routing] Ako morate usko odrediti kako se usmjeriti zahtjeva. na temelju pravila kriterijima kao što su adresa prefiksi. Pravila utemeljen pristupnika pomoću statičke usmjeravanja, a funkcioniraju samo s vezama web-mjesto.

Stvaranje [pristupnik za usmjeravanje sustavom] [ route-based-routing] povezati lokalne mreže pomoću RRAS podržava više web-mjesta ili regije-veze ili implementirati VNet VNet veze (uključujući usmjerava koji prolaziti više VNets). Usmjeravanje sustavom pristupnika pomoću dinamičke usmjeravanje Izravni promet između mreže. Oni tolerate pogreške u mrežni put bolje nego Statički smjerovi jer pokušajte zamjenski usmjerava. Usmjeravanje sustavom pristupnika možete smanjiti pomoćni proces upravljanja, jer usmjerava možda ne moraju se ručno ažurirati promjenama mrežne adrese.

Popis podržanih aparata VPN, potražite u članku [o VPN uređaji za veze na web-mjesto VPN pristupnika][vpn-appliances].

> [AZURE.NOTE] Nakon stvaranja pristupnik nije moguće promijeniti između pristupnik vrste bez brisanja i ponovno stvoriti pristupnik.

Odaberite SKU pristupnika Azure VPN koja najviše odgovara potrebama propusnost. Azure VPN pristupnika dostupan je u tri SKU-ove prikazano u sljedećoj tablici. Svaki SKU nudi različiti propusnost [su naplatila naknada] [ azure-gateway-charges] ovisno o vremenskog razdoblja koja vam je dodijeljena pristupnika i dostupan.

| SKU | Propusnost VPN-a | Max IPSec Tunnels|
|-----|----------------|------------------|
| Osnovni | 100 MB/s | 10 |
| Standardna | 100 MB/s | 10 |
| Visoke performanse | 200 MB/s | 30 |

> [AZURE.NOTE] Osnovni SKU nije kompatibilno s Azure ExpressRoute. Možete [promijeniti na SKU] [ changing-SKUs] nakon stvaranja pristupnika.

Stvaranje usmjeravanje pravila za podmreže pristupnik te Izravni aplikacije promet iz pristupnik Interna opterećenja umjesto dopuštanja zahtjeve za prosljeđivanje izravno VMs koji implementirati aplikaciju.

### <a name="on-premises-network-connection"></a>Lokalni mrežne veze

Stvaranje pristupnika za lokalnu mrežu. Navedite javnu IP adresu VPN-a potražite lokalnog i adresu prostor lokalne mreže. Imajte na umu VPN-a potražite lokalnog mora imati javno IP adresu koja se pristupa putem lokalne mreže pristupnika u Azure VPN pristupnika. Uređaj VPN-a ne nalazi iza NAT uređaja.

Povezivanje web-mjesto za pristupnik za virtualne mreže i pristupnika lokalnoj mreži. Odaberite željenu vrstu veze web-mjesta web-na-mjesta (IPSec), a zatim odredite zajednički ključ. Web-mjesto šifriranje s VPN pristupnika Azure temelji se na protokol IPSec, pomoću tipki zajednički za provjeru autentičnosti. Navedite tipku prilikom stvaranja pristupnika Azure VPN-a. Morate konfigurirati potražite VPN-a koji se izvodi lokalno s istim ključem. Drugi načini provjere autentičnosti trenutno nisu podržani.

Pobrinite se da na lokalni infrastrukturu konfiguriran za prosljeđivanje zahtjeva za namijenjen adresa u VNet Azure uređaj VPN-a.

Otvorite bilo koju priključke potrebnih aplikaciju oblaka lokalne mreže.

Testiranje veze da biste potvrdili da:

- VPN-a potražite lokalnog pravilno usmjerava promet na aplikaciju oblaka putem VPN pristupnika Azure.

- Na VNet pravilno usmjerava promet natrag na lokalnu mrežu.

- Zabrane promet u oba smjera blokirana je ispravno.

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

Ograničeno okomiti skalabilnost možete postići pomicanjem Basic ili standardni VPN pristupnika SKU-ove visoke performanse SKU za VPN-a.

VNets očekivana velik broj promet VPN, razmislite o distribuciji različitih radnih opterećenja u zasebnom manje VNets i konfiguriranje pristupnik VPN-a za svaku od njih.

Možete odrediti particije na VNet okomito ili vodoravno. Da biste particija vodoravno, premjestite pojedinim VM iz svakog sloju u podmreže novi VNet. Rezultat je ima li svaki VNet strukture i funkcionalnosti. Da biste okomito particija promijeniti dizajn svaki sloju podjele funkciju u različitim područjima logički (kao što su obrada narudžbe, fakturiranja, upravljanje klijentima računa i tako dalje). Svaki funkcionalno područje pa možete staviti u vlastitom VNet.

Replikaciju kontroler domene servisa Active Directory lokalnog u na VNet i implementaciju DNS-a u VNet, može pomoći da biste smanjili neke vezane uz sigurnost i administrativnih promet slijedi lokalnim u oblak.

## <a name="availability-considerations"></a>Razmatranja dostupnosti

Ako je potrebno provjeriti lokalne mreže ostati u pristupnik za VPN Azure, implementirati prebacivanje klaster za pristupnik za lokalni VPN-a.

Ako vaša tvrtka ili ustanova ima više lokalnog web-mjesta, stvaranje [veza s više web-mjesta] [ vpn-gateway-multi-site] na jednu ili više VNets Azure. Takvog zahtijeva dinamički (usmjeravanje sustavom) usmjeravanja, pa pristupnika lokalnog VPN-a provjerite podržava li značajka.

Potražite u članku [SLA za pristupnik za VPN] [ sla-for-vpn-gateway] detalje o pristupnik SLA VPN-a.

## <a name="manageability-considerations"></a>Pitanja vezana uz mogućnost upravljanja

Praćenje dijagnostičke informacije iz lokalnog VPN aparata. Ovaj postupak ovisi o značajke koje nudi potražite VPN-a. Na primjer, ako koristite usmjeravanja i servis za daljinski pristup na Windows Server 2012, omogućite [zapisivanje RRAS][rras-logging].

Pomoću [pristupnika VPN Azure Dijagnostika] [ gateway-diagnostic-logs] za snimanje informacija o problema s povezivanjem. Ti zapisnici se može koristiti za praćenje informacija kao što su izvora i odredišta veze zahtjeve, protokol koristila, i kako je uspostaviti vezu (ili zašto pokušaj nije uspio).

Praćenje zapisnike radu sa servisom pristupnika VPN Azure pomoću zapisnika nadzora na portalu za Azure. Zasebne zapisnike dostupni su za pristupnik za lokalnu mrežu, pristupnika Azure mreže i veze. Ove informacije možete koristiti da biste pratili sve promjene pristupnika, a može biti korisno ako prethodno funkcionira pristupnik prestane funkcionirati zbog nekog razloga.

![[2]][2]

Praćenje povezivanje i praćenje događaja nije uspjelo povezivanje. Možete koristiti nadzora paket kao što su [Nagios] [ nagios] da biste zabilježili i izvješća te podatke.

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

Stvaranje različitih zajedničke ključa za svaki pristupnik za VPN-a. Pomoću istaknuti zajednički ključ resist napada grube sile.

> [AZURE.NOTE] Trenutno ne možete koristiti sigurnog Azure ključ za zajednički tipke Azure VPN pristupnika.

Provjerite koristi li VPN-a potražite lokalnog je način šifriranja kompatibilan [s VPN pristupnika Azure][vpn-appliance-ipsec]. Za usmjeravanje pravila utemeljen VPN pristupnika Azure podržava šifriranje algoritama AES256, AES128 i 3DES. Usmjeravanje sustavom pristupnika podržava AES256 i 3DES.

Ako je vaše lokalne VPN-a potražite na mreži opseg koja ima vatrozid između opseg mreža i Internet, možda ćete morati konfigurirati [pravila vatrozida dodatne] [ additional-firewall-rules] da biste omogućili VPN veza web-mjesto.

Ako aplikaciju na VNet šalje podatke s Internetom, razmislite o [implementacijom prisilne tuneliranje] [ forced-tunneling] da biste usmjerili sav promet povezanih s internetom putem lokalne mreže. Taj se način omogućuje nadzora odlazne zahtjeve načinio aplikacije iz lokalnog infrastrukture.

> [AZURE.NOTE] Prisilne tuneliranje može utjecati veza s Azure services (prostora za pohranu servisa, na primjer) i upravitelja licenci sustava Windows.

## <a name="troubleshooting-considerations"></a>Napomene za otklanjanje poteškoća

> [AZURE.NOTE] Posjetite usmjeravanje i daljinski pristup Blog opće informacije o [otklanjanju poteškoća s uobičajenih pogrešaka vezanih uz VPN][troubleshooting-vpn-errors].

Promet se ne može dogoditi VPN veza, provjerite sljedeće:

### <a name="on-premises-vpn-appliance"></a>Lokalni VPN-a potražite:

**VPN-a potražite lokalnog ispravno funkcionira?**

Provjera bilo prijaviti datoteke generira VPN-a potražite pogreške i pogreške. Mjesto ti podaci razlikuju se prema vaš uređaj. Na primjer, ako koristite RRAS na Windows Server 2012, koristite sljedeću naredbu komponente PowerShell da biste prikazali podatke o pogreškama događaja servisa za RRAS:

```
Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
```

Svojstvo _poruke_ svake stavke sadrži opis pogreške. Primjeri uobičajenih su:

- Nemogućnost povezivanja, vjerojatno zbog netočan IP adresu za VPN pristupnika Azure u RRAS VPN mrežna konfiguracija sučelja:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {41, 3, 0, 0}
Index              : 14231
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: The network connection between your computer and
                     the VPN server could not be established because the remote server is not responding. This could
                     be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer
                     and the remote server is not configured to allow VPN connections. Please contact your
                     Administrator or your service provider to determine which device may be causing the problem.
Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                     your computer and the VPN server could not be established because the remote server is not
                     responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc)
                     between your computer and the remote server is not configured to allow VPN connections. Please
                     contact your Administrator or your service provider to determine which device may be causing the
                     problem.}
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:26:02 PM
TimeWritten        : 3/18/2016 1:26:02 PM
UserName           :
Site               :
Container          :
```

- Pogrešan zajedničke ključ koji se navedenom u konfiguraciji sučelja RRAS VPN mreže:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {233, 53, 0, 0}
Index              : 14245
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: IKE authentication credentials are unacceptable

Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                     unacceptable
                     }
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:34:22 PM
TimeWritten        : 3/18/2016 1:34:22 PM
UserName           :
Site               :
Container          :
```

Možete dobiti i evidenciji o pokušaji povezivanja putem servisa RRAS pomoću sljedeće naredbe ljuske PowerShell:

```
Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
```

U slučaju pogreške za povezivanje, ovaj zapisnik će sadrže pogreške koja izgleda otprilike ovako:

```
EventID            : 20227
MachineName        : on-prem-vm
Data               : {}
Index              : 4203
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                     AzureGateway which has failed. The error code returned on failure is 809.
Source             : RasClient
ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
InstanceId         : 20227
TimeGenerated      : 3/18/2016 1:29:21 PM
TimeWritten        : 3/18/2016 1:29:21 PM
UserName           :
Site               :
Container          :
```

**Je ispravno usmjeravanje prometa potražite VPN putem VPN pristupnika Azure?**

Upotreba alata kao što je [PsPing] [ psping] da biste provjerili povezivanje i usmjeravanje preko pristupnika VPN-a. Na primjer, da biste testirali povezivanje s računala na lokalni na web-poslužitelju koji se nalazi na VNet, pokrenite sljedeću naredbu (zamjena `<<web-server-address>>` s adresom web-poslužitelj):

```
PsPing -t <<web-server-address>>:80
```

Ako je na lokalno računalo možete usmjeriti promet na web-poslužitelj, trebali biste vidjeti izlaz sličnu ovoj:

```
D:\PSTools>psping -t 10.20.0.5:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.0.5:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.0.5:80 (warmup): 6.21ms
Connecting to 10.20.0.5:80: 3.79ms
Connecting to 10.20.0.5:80: 3.44ms
Connecting to 10.20.0.5:80: 4.81ms

  Sent = 3, Received = 3, Lost = 0 (0% loss),
  Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
```

Ako je na lokalno računalo ne možete komunicirati s navedeno odredište, prikazat će se poruka ovako:

```
D:\PSTools>psping -t 10.20.1.6:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.1.6:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80:
  Sent = 3, Received = 0, Lost = 3 (100% loss),
  Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
```

**Dopušta li lokalni vatrozid VPN promet na proći kroz? Odgovarajući priključci otvoren?**

Provjerite koristi li VPN-a potražite lokalnog je način šifriranja kompatibilan [s VPN pristupnika Azure][vpn-appliance].

Za usmjeravanje pravila utemeljen VPN pristupnika Azure podržava šifriranje algoritama AES256, AES128 i 3DES. Usmjeravanje sustavom pristupnika podržava AES256 i 3DES.

### <a name="azure-vnet-gateway"></a>Azure pristupnik VNet:

Pregledajte [Azure VPN pristupnika dijagnostičke zapisnike] [ gateway-diagnostic-logs] potencijalne probleme.

**Jesu li pristupnik VPN-a za Azure i lokalnih VPN-a uređaj konfiguriran s istim ključem zajednički za provjeru autentičnosti?**

Možete vidjeti zajedničke ključ pohranjen pristupnik za VPN Azure pomoću sljedeće naredbe Azure EŽA:

```
azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
```

Pomoću naredbe prikladna za vaše lokalne VPN-a potražite da bi se prikazala zajednički ključ konfigurirana za taj uređaj.

Provjerite _GatewaySubnet_ podmreže držanjem Azure VPN pristupnika nije povezan s programa NSG.

Možete vidjeti detalje podmreže pomoću naredbe Azure EŽA sljedeće:

```
azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
```

Provjerite je li još nema podatkovno polje pod nazivom _id mreže sigurnosne grupe_. Sljedeći primjer prikazuje rezultate na primjer _GatewaySubnet_ je dodijeljena NSG (_VPN-a, pristupnika i grupe_). To može uzrokovati spriječiti pristupnika pravilno funkcioniranje ako postoje pravila definirano za ovo NSG:

```
C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
    info:    Executing command network vnet subnet show
    + Looking up virtual network "profx-vnet"
    + Looking up the subnet "GatewaySubnet"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
    data:    Name                            : GatewaySubnet
    data:    Provisioning state              : Succeeded
    data:    Address prefix                  : 10.20.3.0/27
    data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
    info:    network vnet subnet show command OK
```

**Virtualnim strojevima u Azure VNet konfigurirani tako da dopušta promet dolaze iz izvan na VNet? Provjera pravila NSG pridružene podmreže koja sadrži te virtualnim računalima.**

Sva pravila NSG možete prikazati pomoću naredbe Azure EŽA sljedeće:

```
azure network nsg show -g <<resource-group>> -n <<nsg-name>>
```

**Prekinuta je veza VPN pristupnika Azure?**

Koristite sljedeću naredbu Azure PowerShell da biste provjerili trenutni status Azure VPN veza. Na `<<connection-name>>` parametar je naziv Azure VPN vezu koja povezuje pristupnika virtualne mreže i lokalne pristupnika.

```
Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
```

Sljedeće isječci istaknuli izlaz ako pristupnika povezani (prvi primjer), a nije povezan generira (drugi primjer):

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : Connected
EgressBytesTransferred     : 55254803
IngressBytesTransferred    : 32227221
ProvisioningState          : Succeeded
...
```

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : NotConnected
EgressBytesTransferred     : 0
IngressBytesTransferred    : 0
ProvisioningState          : Succeeded
...
```

### <a name="host-vm-configuration-network-bandwidth-utilization-and-application-performance"></a>Konfiguracija hosta VM, Upotreba propusnost mreže i performanse aplikacije:

Provjerite je li vatrozid u operacijskom sustavu goste na VMs Azure u podmreži su konfiguriran pravilno da dopušta promet dopušteno iz lokalnog IP raspone.

**Količinu promet blizu ograničenja propusnosti dostupna je u pristupnik za VPN Azure?**

Tooling ovisi o dostupna za vaš uređaj VPN-a radi lokalnog funkcije. Ako, na primjer, ako koristite RRAS na Windows Server 2012, možete koristiti nadzor performansi moraju pratiti količinu podataka primljene i prenose putem veze VPN-a; pomoću _RAS ukupni_ objekta, odaberite mjerača _Bajtova Primljeno/Sec_ i _Prenose Sec bajtova_ :

![[3]][3]

Trebali biste uspoređuju rezultate s propusnosti za pristupnik za VPN-a (100 MB/s za osnovne i standardne SKU-ove i 200 MB/s visoke performanse SKU-om):

![[4]][4]

**Su neki od virtualnim strojevima u Azure VNet spora? Su oni preopterećene, su dovoljno ih učiniti opterećenje postoje sve učitavanja-balancers ispravno konfigurirano?**

Za tu radnju [hvatanje i analiza dijagnostičke informacije][azure-vm-diagnostics]. Možete provjeriti rezultata pomoću portala za Azure, ali su mnoge alata drugih proizvođača i dostupne koja pružaju detaljne uvida u podataka o performansama.

**Je li aplikacije upućivanje učinkovito korištenje resursa oblaka?**

Instrument aplikacije kod koji se izvodi na svakom VM da biste odredili hoće li aplikacija činite najbolje korištenje resursa. Možete koristiti alate kao što je [Aplikacija uvida] [ application-insights] da biste lakše izvršiti sljedeće zadatke.

## <a name="solution-deployment"></a>Uvođenje rješenja

Ako imate postojeću lokalnog infrastrukturu konfigurirane s VPN-a potražite, možete implementirati arhitektura referenca na sljedeći način:

1. Desnom tipkom miša kliknite donji gumb i odaberite neki "Otvori vezu na novoj kartici" ili "Otvori vezu u novom prozoru":  
[![Implementacija Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn%2Fazuredeploy.json)

2. Pričekajte vezu da biste otvorili na portalu za Azure, a zatim slijedite ove korake: 
    - Naziv **grupe resursa** već definiran u datoteci parametar, pa odaberite **Stvori novo** i unesite `ra-hybrid-vpn-rg` u tekstni okvir.
    - **Mjesto** padajućem okviru odaberite regiju.
    - Uređivati **Predložak korijenski Uri** ili tekstne okvire **Parametar korijenski Uri** .
    - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
    - Kliknite gumb **za kupnju** .

3. Pričekajte implementaciju da biste dovršili.

## <a name="next-steps"></a>Daljnji koraci

- [Instaliranje kontrolera dodatnu domenu za korištenje VMs u programa Azure VNet lokalne domene servisa Active Directory][installing-ad].

- [Smjernice za implementaciju sustava Windows Server Active Directory na Azure VMs][deploying-ad].

- [Stvaranje DNS poslužitelja u na VNet][creating-dns].

- [Konfiguriranje i upravljanje DNS-u s VNet][configuring-dns].

- [Korištenje lokalnog Stormshield mreže sigurnost aparata putem virtualne privatne MREŽE][stormshield].

- [Implementacijom arhitekturu hibridnog mreže s Azure ExpressRoute][expressroute].

<!-- links -->

[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[arm-templates]: ../resource-group-authoring-templates.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-portal]: ../azure-portal/resource-group-portal.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[azure-virtual-network]: ../virtual-network/virtual-networks-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-network-security-group]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: ../vpn-gateway/vpn-gateway-multi-site.md
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_0/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: http://blogs.technet.com/b/keithmayer/archive/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell.aspx
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: http://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: http://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: http://blogs.technet.com/b/keithmayer/archive/2015/12/07/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs.aspx
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[create-azure-vnet]: ../virtual-network/virtual-networks-create-vnet-classic-cli.md
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: ../application-insights/app-insights-overview-usage.md
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[vpn-appliances]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[installing-ad]: ../active-directory/active-directory-install-replica-active-directory-domain-controller.md
[deploying-ad]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[creating-dns]: https://blogs.msdn.microsoft.com/mcsuksoldev/2014/03/04/creating-a-dns-server-in-azure-iaas/
[configuring-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[stormshield]: https://azure.microsoft.com/marketplace/partners/stormshield/stormshield-network-security-for-cloud/
[vpn-appliance-ipsec]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec-parameters
[expressroute]: ./guidance-hybrid-network-expressroute.md
[naming conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/deploy-reference-architecture.sh
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetwork.parameters.json
[virtualNetworkGateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetworkGateway.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/blueprints/hybrid-network-vpn.png "Struktura hibridnoj mreže koje se protežu na lokalni i infrastructures u oblaku"
[1]: ./media/guidance-hybrid-network-vpn/partitioned-vpn.png "Particija VNet da biste poboljšali skalabilnost"
[2]: ./media/guidance-hybrid-network-vpn/audit-logs.png "Zapisnike nadzora na portalu za Azure"
[3]: ./media/guidance-hybrid-network-vpn/RRAS-perf-counters.png "Mjerača performansi za praćenje mrežnog prometa VPN-a"
[4]: ./media/guidance-hybrid-network-vpn/RRAS-perf-graph.png "Primjer VPN mreže performanse grafikonu"