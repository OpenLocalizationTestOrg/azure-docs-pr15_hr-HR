<properties
   pageTitle="Sistemski preduvjeti za StorSimple virtualne polja"
   description="Dodatne informacije o softver i umrežavanje preduvjeti za vaše StorSimple virtualne polja"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/17/2016"
   ms.author="alkohli"/>

# <a name="storsimple-virtual-array-system-requirements"></a>Sistemski preduvjeti za StorSimple virtualne polja

## <a name="overview"></a>Pregled

U ovom se članku opisuje važna sistemski preduvjeti za klijente za pohranu pristupa polja i sustava Microsoft Azure StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualnog uređaja ili StorSimple virtualnog uređaja). Preporučujemo da pregledate informacije pažljivo prije implementacije sustava StorSimple, a zatim upućivanje ga prema potrebi tijekom uvođenja i sljedeći postupak.

Sistemski preduvjeti obuhvaćaju sljedeće:

-   **Softverski preduvjeti za klijente za pohranu** - opisuje virtualizacije podržane platforme, web-preglednici, iSCSI Pokretači, SMB klijenata, preduvjeti minimalne virtualnog uređaja i sve dodatni preduvjeti za te operacijske sustave.

-   **Preduvjeti za StorSimple uređaj za povezivanje s mrežom** - navedene su informacije o priključke koji moraju biti otvoreni u vatrozid da biste dopustili iSCSI "," oblaka "ili" Upravljanje promet.

Informacije preduvjeti sustava StorSimple objaviti u ovom članku odnose se samo StorSimple virtualne polja.

- Za uređaje 8000 niz, idite na [sistemski preduvjeti za svoj uređaj StorSimple 8000 niz](storsimple-system-requirements.md).
 
- Za uređaje 7000 niz, idite na [sistemski preduvjeti za svoj uređaj za StorSimple 5000 7000 niz](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).


## <a name="software-requirements"></a>Softverski preduvjeti

Softverski preduvjeti obuhvatiti informacije na podržani web-preglednici, SMB verzijama, virtualizacije platforme i preduvjeti minimalne virtualnog uređaja.

### <a name="supported-virtualization-platforms"></a>Virtualizacija podržane platforme


| **Hypervisor** | **Verzija**                          |
|----------------|--------------------------------------|
| Hyper-V        | Windows Server 2008 R2 SP1 ili noviji |
| VMware ESXi    | 5,5 i noviji                        |

### <a name="virtual-device-requirements"></a>Preduvjeti za virtualnog uređaja

| **Komponenta**                                | **Preduvjet**            |
|----------------------------------------------|----------------------------|
| Najmanji broj virtualne procesora (jezgri) | 4                          |
| Minimalna RAM memorije                         | 8 GB                       |
| Prostor na disku<sup>1</sup>                       | Na disku OS - 80 GB <br></br>Podatkovni disk - 500 GB 8 TB|
| Najmanji broj interface(s) mreže       | 1                          |
| Minimalna Internet propusnosti<sup>2</sup>       | 5 MB/s                     |

<sup>1</sup> – thin dodjeli

<sup>2</sup> – mreže razlikuje se ovisno o dnevnih stopa promjene podataka. Ako, na primjer, ako uređaj nije potrebno sigurnosno kopiranje 10 GB ili dodatne promjene tijekom dana, zatim dnevna sigurnosna kopija putem 5 MB/s veze može potrajati do 4,25 sata (Ako podataka nije moguće sažeti ili deduplicated).

### <a name="supported-web-browsers"></a>Podržani web-preglednici

| **Komponenta**     | **Verzija** | **Dodatni preduvjeti/bilješke** |
|-------------------|-----------------|-----------------------------------|
| Microsoft Edge    | Najnovija verzija  |                                   |
| Internet Explorer | Najnovija verzija  | Testira s preglednikom Internet Explorer 11  |
| Google Chrome     | Najnovija verzija  | S Chrome 46             |

### <a name="supported-storage-clients"></a>Klijenti za podržane prostora za pohranu 

Softverski preduvjeti su Pokretači iSCSI kojoj pristup vaše StorSimple polja virtualne (konfigurirati kao iSCSSI poslužiteljem).

| **Podržani operacijski sustavi** | **Verzije koja je potrebna** | **Dodatni preduvjeti/bilješke** |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008R2 SP1, 2012, 2012R2 |StorSimple možete stvoriti thinly dodijeljenu i potpuno dodijeljenu količine. Ne može stvoriti djelomično dodijeljenu količine. StorSimple iSCSI jedinicama za podržane su samo: <ul><li>Jednostavne jedinice na Windows osnovnih diskova.</li><li>Windows NTFS za oblikovanje jedinicu.</li>|


Su softverski preduvjeti za klijente SMB kojoj pristup vaše StorSimple polja virtualne (konfigurirati kao poslužitelj datoteka).

| **Verzija SMB** |
|-------------|
| SMB 2.x     |
| SMB 3.0     |
| SMB 3.02    |

> [AZURE.IMPORTANT] Kopiranje i spremanje datoteka zaštićena po Windows šifriranje datotečni sustav (EFS) na poslužitelj datoteka StorSimple virtualne polja; To će rezultirati nepodržane konfiguracije. 

## <a name="networking-requirements"></a>Preduvjeti za povezivanje s mrežom 

U sljedećoj su tablici navedeni priključci koje morate otvoriti u vatrozid da biste dopustili iSCSI, SMB, oblaka ili upravljanje promet. U ovoj tablici *ili *ulaznog* * odnosi se na smjer iz kojeg zahtjevi za klijentski pristup uređaju. *Izgleda* ili *Izlazni* odnosi se na smjer u kojem uređaju StorSimple šalje podatke vanjsko, osim implementacijskih: na primjer, izlazni s Internetom.

| **Broj priključka<sup>1</sup>** | **I smanjivanje** | **Opseg priključak** | **Obavezno**              | **Bilješke**                                                                                                            |
|--------------------------|---------------|----------------|---------------------------|----------------------------------------------------------------------------------------------------------------------|
| TCP 80 (HTTP)            | Izlaz           | NASTAVKU            | ne                        | Da biste dohvatili ažuriranja koristi se izlazni priključak za pristup Internetu. <br></br>Proxy poslužitelj odlazne web je korisnik može konfigurirati. |
| TCP 443 (HTTPS)          | Izlaz           | NASTAVKU            | Da                       | Koristi se izlazni priključak za pristup podacima u oblaku. <br></br>Proxy poslužitelj odlazne web je korisnik može konfigurirati. |
| UDP 53 (DNS)             | Izlaz           | NASTAVKU            | U nekim slučajevima Pogledajte bilješke. | Ovaj priključak potreban je samo ako koristite poslužitelj za DNS utemeljen na Internet. <br></br> **Napomena**: Ako implementirate datotečnom poslužitelju, preporučujemo korištenje lokalnog poslužitelja DNS-a.|
| UDP 123 (NTP)            | Izlaz           | NASTAVKU            | U nekim slučajevima Pogledajte bilješke. | Ovaj priključak potreban je samo ako koristite poslužitelj za NTP utemeljene na Internetu.<br></br> **Bilješke:** Ako implementirate datotečnom poslužitelju, preporučujemo da sinkronizirate vrijeme s vašeg kontrolera domene servisa Active Directory.  |
| TCP 80 (HTTP)           | U            | LAN-A            | Da                       | Ovo je ulazni priključak za lokalni korisničko Sučelje na uređaju StorSimple lokalnog upravljanja. <br></br> **Napomena**: pristup lokalnog korisničkog Sučelja putem HTTP-a HTTPS će automatski preusmjeravanje.|
| TCP 443 (HTTPS)          | U            | LAN-A            | Da                       | Ovo je ulazni priključak za lokalni korisničko Sučelje na uređaju StorSimple lokalnog upravljanja.|
| TCP 3260 (iSCSI)         | U            | LAN-A            | ne                        | Pristup podacima putem iSCSI koristi se ova priključak.|

<sup>1</sup> ulaznog priključci morate otvoriti u javni Internet.

> [AZURE.IMPORTANT] Provjerite je li da Vatrozid ne Izmijeni ili dešifriranje sav promet SSL između StorSimple uređaja i Azure.


### <a name="url-patterns-for-firewall-rules"></a>Uzorci URL-a za pravila vatrozida 

Mrežni administratori često možete konfigurirati pravila vatrozida za napredne na temelju uzoraka URL-a da biste filtrirali na ulazni i izlazni promet. Vaš virtualne polja i servis za upravitelja StorSimple ovise o drugim aplikacijama Microsoft kao što su Bus servisa Azure, Azure Active Directory kontrola pristupa, račune za pohranu i poslužitelji Microsoft Update. Uzorci URL vezane uz ove aplikacije mogu se konfigurirati pravila vatrozida. Važno je da razumijete možete promijeniti URL uzorke vezane uz ove aplikacije. To shodno je potrebno mrežni administrator za praćenje i ažuriranje pravila vatrozida za vaše StorSimple kao i po potrebi. 

Preporučujemo da postavite pravila vatrozida za izlazni promet na temelju StorSimple liberally Fiksna IP adresa u većini slučajeva. Međutim, možete koristiti informacija u nastavku da biste postavili pravila napredni vatrozid koji su potrebni za stvaranje sigurnog okruženja.

> [AZURE.NOTE] 
> 
> - Uređaj (izvor) IP-ovi mora uvijek biti postavljena na sva sučelja oblaka omogućeno mreže. 
> - Odredišni IP-ovi mora biti postavljena na [Azure podatkovnog centra IP raspone](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


| Uzorak URL-a                                                      | Komponenta/funkcija                                           |
|------------------------------------------------------------------|---------------------------------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | Servis upravitelja StorSimple<br>Servis kontrole programa Access<br>Azure Service Bus|
|`http://*.backup.windowsazure.com`|Registracija uređaja|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Opoziva certifikata |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Računi za Azure prostora za pohranu i nadzor |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Poslužitelji Microsoft Update<br>                        |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*`                    | Paket za podršku                                                  |
| `http://*.data.microsoft.com `                   | Servis za telemetriju u sustavu Windows potražite u članku [Ažuriranje za korisnička sučelja i dijagnostičkih telemetrijskih](https://support.microsoft.com/en-us/kb/3068708)                                                  |

## <a name="next-step"></a>Sljedeći korak

-   [Priprema portal za implementaciju sustava StorSimple virtualne polja](storsimple-ova-deploy1-portal-prep.md)
