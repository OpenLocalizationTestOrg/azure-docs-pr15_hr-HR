<properties
    pageTitle="Prije implementacije Azure stogu PNA | Microsoft Azure"
    description="Prikaz okruženje i hardver preduvjeti za Azure stogu PNA (administrator servisa)."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="erikje"/>

# <a name="azure-stack-deployment-prerequisites"></a>Preduvjeti za implementaciju Azure stogu

Prije implementacije Azure stogu PNA ([Dokaz pojam](azure-stack-poc.md)), provjerite je li vaše računalo zadovoljava sljedeće preduvjete.
Preduvjeti za implementaciju Tehnički pretpregled 2 za na PNA su jednaki onima potrebne za Tehnički pretpregled 1. Dakle, možete koristiti isti hardver koji ste koristili za prethodni jednog okvira Pretpregled.

## <a name="hardware"></a>Hardvera

| Komponenta | Minimum  | Preporučeno |
|---|---|---|
| Diskovnih pogona: operacijski sustav | 1 OS disk s najmanje 200 GB za sistemske particije (SSD ili podizanje tvrdog diska) | 1 OS disk s najmanje 200 GB za sistemske particije (SSD ili podizanje tvrdog diska) |
| Diskovnih pogona: općih Azure stogu PNA podataka | 4 diskova. Svakom disku sadrži najmanje 140 GB kapaciteta (SSD ili podizanje tvrdog diska). Koristit će se sve dostupne diskova. | 4 diskova. Svakom disku sadrži najmanje 250 GB kapaciteta (SSD ili podizanje tvrdog diska). Koristit će se sve dostupne diskova.|
| Izračun: CPU-a | Lišće Socket: 12 fizičke jezgri (zbroj)  | Lišće Socket: 16 fizičke jezgri (zbroj) |
| Izračun: memorije | 96 GB RAM-A  | 128 GB RAM-A |
| Izračun: BIOS | Omogućeno Hyper-V (s podrškom za SLAT)  | Omogućeno Hyper-V (s podrškom za SLAT) |
| Mrežni: NIC | Windows Server 2012 R2 ustanove potrebne za NIC; Nema specijalizirane značajke potrebni | Windows Server 2012 R2 ustanove potrebne za NIC; Nema specijalizirane značajke potrebni |
| Hardverski logotip certifikata | [Certificirano za Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Certificirano za Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

**Konfiguraciji diskovni pogon podataka:** Svi podaci pogoni moraju biti iste vrste (SAS sve ili sve SATA) i kapacitet. Ako koriste SAS diskovnih pogona, diskovnih pogona moraju biti priloženi putem jednog puta (bez MPIO podrške za više puta navedeni su).

**Mogućnosti konfiguracije HBA**
 
- (Preferirani) Jednostavni HBA
- RAID HBA – prilagodnik mora biti konfigurirano u načinu "prolaz"
- RAID HBA – diskova trebali biste konfigurirati kao jedan Disk, RAID 0

**Podržani bus i medijskih sadržaja upišite kombinacije**

-   PODIZANJE TVRDOG DISKA SATA

-   PODIZANJE TVRDOG DISKA SAS

-   PODIZANJE TVRDOG DISKA RAID

-   RAID SSD (Ako je vrsta medija neodređeni/Nepoznato\*)

-   SATA SSD + SATA PODIZANJE TVRDOG DISKA

-   SAS SSD + SAS PODIZANJE TVRDOG DISKA

\*RAID kontrolera bez prolazni mogućnost ne može prepoznati vrsti medija. Takve kontrolera će označiti podizanje tvrdog diska i SSD neodređenim. U tom slučaju u SSD bit će upotrijebljen kao stalnih prostor za pohranu umjesto predmemoriranje uređaja. Zbog toga možete implementirati PNA za stog Microsoft Azure na te SSDs.

**Primjer HBAs**: LSI 9207 8i, LSI 9300 8i ili LSI-9265-8i u prolazni načinu rada

Dostupni su konfiguracije OEM uzorka.

## <a name="operating-system"></a>Operacijski sustav

| | **Preduvjeti**  |
|---|---|
| **Verzija OS-a** | Windows Server 2012 R2 ili novijim. Verzija operacijskog sustava nije od ključne važnosti prije implementacijskih pokrene, kao što je glavno računalo će se pokrenuti u VHD uvrštene u stogu Azure instalacije zip. OS-a i sve potrebne zakrpa već integrirani u slike. Nemojte koristiti sve tražene za aktiviranje sve instance Windows Server koristi u na PNA.|

## <a name="deployment-requirements-check-tool"></a>Preduvjeti za implementaciju provjerite alata

Nakon što instalirate operacijski sustav, možete koristiti alat za [Provjeru implementacije za Azure stogu Tehnički pretpregled 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) da biste potvrdili da hardver zadovoljava sve preduvjete.



## <a name="microsoft-azure-active-directory-accounts"></a>Računi za Microsoft Azure Active Directory

Microsoft Azure stogu PNA implementacije mora biti povezano s Azure. Stoga račun za Microsoft Azure Active Directory morate pripremiti prije pokretanja implementacije skriptu PowerShell. Taj račun postaje globalni administrator za klijent Azure Active Directory. Će se koristiti za resurse i delegiranje aplikacija i servisa upravitelji za sve servise Azure stogu interakciju s Azure Active Directory i grafika API-JA. Bit će upotrijebljen i kao vlasnik pretplate zadanog davatelja (koju možete kasnije promijeniti). Centar za administratore sustava Azure snop možete se prijaviti pomoću ovog računa.

1. Stvorite račun za Azure AD koji je administrator direktorija barem jedan Azure Active Directory. Ako već postoji, možete koristiti koje. U suprotnom, možete ga stvoriti besplatno pri [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (u Kini, umjesto toga posjetite <http://go.microsoft.com/fwlink/?LinkID=717821> ).

    Spremite te vjerodajnice za korištenje u koraku 6 [pokrenuti implementacijsku skriptu PowerShell](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script). Ovaj *servis administratorski* račun možete konfigurirati i upravljati oblaka resursa, korisničke račune, tarife za klijenta, kvota i cijene. Na portalu mogu stvoriti oblaka web-mjesta, privatni oblaka virtualnog računala, stvaranje tarife, i upravljanje pretplatama korisnika.

2. [Stvaranje](azure-stack-add-new-user-aad.md) barem jedan račun tako da se možete prijaviti u stogu PNA Azure kao klijentom.

  	| **Račun za Azure Active Directory**  | **Podržani?** |
  	|---|---| 
  	| Račun tvrtke ili škole valjani javno Azure pretplate  | Da |
  	| Microsoftov Account valjani javno Azure pretplate  | ne |
  	| Račun tvrtke ili škole valjanu pretplatu Kina Azure  | Da |
  	| Račun tvrtke ili škole valjani NAM državne Azure pretplate  | Da |


## <a name="network"></a>Mreža

### <a name="switch"></a>Promjena

Jedan priključak dostupna na parametru za računalo PNA.  

Strojno Azure stogu PNA podržava povezivanje s priključak pristup za promjenu ili skup linija priključak. Nema specijalizirane značajke potrebni su parametar. Ako koristite priključak skup veza ili ako morate konfigurirati VLAN ID-a, morate unijeti ID VLAN kao parametar implementacije. Vidjet ćete Primjeri [popis parametara implementacije](azure-stack-run-powershell-script.md).

### <a name="subnet"></a>Podmreže

Povezati se s računala PNA sljedeće podmreže:
- 192.168.200.0/24
- 192.168.100.0/27
- 192.168.101.0/26
- 192.168.102.0/24
- 192.168.103.0/25
- 192.168.104.0/25

Ove podmreže su rezervirana za interne mreže Microsoft Azure stogu PNA okruženja.

### <a name="ipv4ipv6"></a>IPv4 i IPv6

Podržana je samo IPv4. Ne možete stvoriti mreža IPv6.

### <a name="dhcp"></a>DHCP

Provjerite je li DHCP poslužitelj dostupna na mreži koja povezuje NIC-a. Ako DHCP nije dostupna, morate pripremiti za dodatne statičnu IPv4 mrežnu osim onog koristi glavnog računala. Morate navesti te IP adresa i pristupnik kao parametar implementacije. Vidjet ćete Primjeri [popis parametara implementacije](azure-stack-run-powershell-script.md).

### <a name="internet-access"></a>Pristup Internetu

Azure stogu potreban je pristup Internetu, izravno ili putem prozirne proxy poslužitelja. Azure stoga ne podržava konfiguracije web proxy da biste omogućili pristup Internetu. IP glavno računalo i novi IP dodijelila MAS BGPNAT01 (DHCP ili statičke IP) moraju imati mogućnost za pristup Internetu. Priključke 80 i 443 koriste se u odjeljku domene graph.windows.net i login.windows.net.

### <a name="telemetry"></a>Telemetrijskih

Da biste podržali toka telemetrijskih podataka, priključak 443 (HTTPS) moraju biti otvoreni u vašoj mreži. Krajnja točka klijent je https://vortex-win.data.microsoft.com.


## <a name="next-steps"></a>Daljnji koraci

[Preuzimanje paketa za implementaciju PNA snop Azure](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Implementacija Azure stogu PNA](azure-stack-run-powershell-script.md)
