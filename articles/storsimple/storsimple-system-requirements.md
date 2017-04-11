<properties
   pageTitle="Sistemski preduvjeti za StorSimple | Microsoft Azure"
   description="U članku se opisuje softver, povezivanje s mrežom, i preduvjeti visoke dostupnosti i najbolje prakse za Microsoft Azure StorSimple rješenja."
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
   ms.workload="TBD"
   ms.date="08/31/2016"
   ms.author="alkohli"/>

# <a name="storsimple-software-high-availability-and-networking-requirements"></a>Softver StorSimple, visoke dostupnosti i preduvjeti za umrežavanje

## <a name="overview"></a>Pregled

Dobro došli u Microsoft Azure StorSimple. U ovom se članku opisuje važna sistemski preduvjeti i najbolje prakse za svoj uređaj StorSimple i za klijente za pohranu pristup uređaju. Preporučujemo da pregledate informacije pažljivo prije implementacije sustava StorSimple, a zatim možete se vratiti na ga prema potrebi tijekom uvođenja i sljedeći postupak.

Sistemski preduvjeti obuhvaćaju sljedeće:

- **Softverski preduvjeti za klijente za pohranu** – opisuju podržani operacijski sustavi i sve dodatni preduvjeti za te operacijske sustave.
- **Preduvjeti za StorSimple uređaj za povezivanje s mrežom** - navedene su informacije o priključke koji moraju biti otvoreni u vatrozid da biste dopustili iSCSI "," oblaka "ili" Upravljanje promet.
- **Visoke dostupnosti preduvjeti za StorSimple** - opisuju preduvjeti visoke dostupnosti i najbolje prakse za StorSimple uređaja i glavnog računala. 


## <a name="software-requirements-for-storage-clients"></a>Softverski preduvjeti za klijente za pohranu

Su softverski preduvjeti za klijente za pohranu koji pristup StorSimple uređaj.

| Podržani operacijski sustavi | Verzije koja je potrebna | Dodatni preduvjeti/bilješke |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008R2 SP1, 2012, 2012R2 |Količine iSCSI StorSimple podržani su za korištenje na samo Windows disk sljedeće:<ul><li>Jednostavna jedinica na osnovni disk</li><li>Jednostavan i zrcaljenje glasnoću na dinamički disk</li></ul>Windows Server 2012 Tanki dodjele resursa ODX značajke podržane su i ako koristite StorSimple iSCSI jedinicu.<br><br>StorSimple možete stvoriti thinly dodijeljenu i potpuno dodijeljenu količine. Ne može stvoriti djelomično dodijeljenu količine.<br><br>Ponovnog formatiranja thinly dodijeljenu glasnoću traje predugo. Preporučujemo da brisanje glasnoću i stvaranje novog umjesto ponovnog formatiranja. Međutim, ako se i dalje želite preoblikovati jedinice:<ul><li>Pokrenite sljedeću naredbu prije na Preoblikuj da biste izbjegli kašnjenja reclamation prostor: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Po dovršetku oblikovanje da biste omogućili reclamation prostora koristite sljedeću naredbu:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>Primjena hitni popravak sustava Windows Server 2012 kao što je opisano u [KB 2878635](https://support.microsoft.com/kb/2870270) s računalom Windows Server.</li></ul></li></ul></ul> Ako konfigurirate Upravitelj snimka StorSimple ili StorSimple prilagodnik za SharePoint, idite na [softverski preduvjeti za neobavezne komponente](#software-requirements-for-optional-components).|
| VMWare ESX | 5,5 i 6.0 | Podržava VMWare vSphere kao iSCSI klijent. Značajka VAAI blok podržan je VMware vSphere na StorSimple uređajima.
| Linux RHEL CentOS | 5, 6 i 7 | Podrška za klijente iSCSI Linux Otvori iSCSI pokretač verzije 5, 6 i 7. |
| Linux | SUSE Linux 11 | |
 > [AZURE.NOTE] IBM AIX trenutno nije podržano s StorSimple.

## <a name="software-requirements-for-optional-components"></a>Softverski preduvjeti za neobavezne komponente

Softverski preduvjeti su neobavezne komponente StorSimple (Upravitelj snimka StorSimple i StorSimple prilagodnik za SharePoint).

| Komponenta | Glavno računalo platforme | Dodatni preduvjeti/bilješke |
| --------------------------- | ---------------- | ------------- |
| Upravitelj StorSimple snimke | Windows Server 2008R2 SP1, 2012, 2012R2 | Korištenje programa StorSimple snimke Manager na poslužitelju Windows nije potreban za sigurnosno kopiranje i vraćanje zrcaljene dinamičkih diskova i za bilo koju aplikaciju dosljedan sigurnosne kopije.<br> Upravitelj snimku StorSimple podržana je samo na Windows Server 2008 R2 SP1 (64-bitni), Windows 2012 R2 i Windows Server 2012.<ul><li>Ako koristite prozor Server 2012, morate instalirati .NET 3.5 – 4,5 prije instalacije Upravitelj StorSimple snimke.</li><li>Ako koristite Windows Server 2008 R2 SP1, morate instalirati Windows Management Framework 3.0 prije instalacije Upravitelj StorSimple snimke.</li></ul> |
| StorSimple prilagodnik za SharePoint | Windows Server 2008R2 SP1, 2012, 2012R2 |<ul><li>StorSimple prilagodnik za SharePoint podržano je samo na web-mjesto sustava SharePoint 2010 i u okvir za SharePoint 2013.</li><li>RBS potreban je SQL Server Enterprise Edition, verzije 2008 R2 ili 2012.</li></ul>|

## <a name="networking-requirements-for-your-storsimple-device"></a>Preduvjeti za StorSimple uređaj za povezivanje s mrežom

Uređaj StorSimple je uređaj zaključan prema dolje. Međutim, priključke morati otvoriti u vatrozid da biste dopustili iSCSI, oblaka i upravljanje promet. U sljedećoj su tablici navedeni priključci koje morate otvoriti u vatrozidu. U ovoj tablici *ili *ulaznog* * odnosi se na smjer iz kojeg zahtjevi za klijentski pristup uređaju. *Izgleda* ili *Izlazni* odnosi se na smjer u kojem uređaju StorSimple šalje podatke vanjsko, osim implementacijskih: na primjer, izlazni s Internetom.

| Broj priključka<sup>1,2</sup> | I smanjivanje | Opseg priključak | Obavezno | Bilješke |
|------------------------|-----------|------------|----------|-------|
|TCP 80 (HTTP)<sup>3</sup>|  Izlaz |  NASTAVKU | ne |<ul><li>Da biste dohvatili ažuriranja koristi se izlazni priključak za pristup Internetu.</li><li>Proxy poslužitelj odlazne web je korisnik može konfigurirati.</li><li>Da biste omogućili ažuriranja sustava, priključak mora biti otvoren za kontroler fiksno IP-ovi.</li></ul> |
|TCP 443 (HTTPS)<sup>3</sup>| Izlaz | NASTAVKU | Da |<ul><li>Koristi se izlazni priključak za pristup podacima u oblaku.</li><li>Proxy poslužitelj odlazne web je korisnik može konfigurirati.</li><li>Da biste omogućili ažuriranja sustava, priključak mora biti otvoren za kontroler fiksno IP-ovi.</li><li>Ovaj priključak koristi se na oba kontrolera za smeća.</li></ul>|
|UDP 53 (DNS) | Izlaz | NASTAVKU | U nekim slučajevima Pogledajte bilješke. |Ovaj priključak potreban je samo ako koristite poslužitelj za DNS utemeljen na Internet. |
| UDP 123 (NTP) | Izlaz | NASTAVKU | U nekim slučajevima Pogledajte bilješke. |Ovaj priključak potreban je samo ako koristite poslužitelj za NTP utemeljene na Internetu. |
| TCP 9354 | Izlaz | NASTAVKU | Da |Izlazni priključak koristi StorSimple uređaja možete komunicirati s StorSimple Upravitelj servisa. |
| 3260 (iSCSI) | U | LAN-A | ne | Pristup podacima putem iSCSI koristi se ova priključak.|
| 5985 | U | LAN-A | ne | Ulazni priključak za komunikaciju s uređajem StorSimple StorSimple snimke Upravitelj.<br>Ovaj priključak koristi se kada daljinski povežete Windows PowerShell StorSimple putem HTTP-a. |
| 5986 | U | LAN-A | ne | Ovaj priključak koristi se kada daljinski povežete Windows PowerShell StorSimple putem HTTP. |

<sup>1</sup> ulaznog priključci morate otvoriti u javni Internet.

<sup>2</sup> ako više priključaka Izvedi konfiguraciju pristupnika, redoslijed odlazni promet usmjerena odredit će se temelji se na redoslijedu usmjeravanje priključak opisane u [priključak usmjeravanja](#routing-metric), ispod.

<sup>3</sup> kontrolerom fiksno IP-ovi na uređaju StorSimple mora biti usmjeravati i može se povezati s Internetom. Fiksni IP adrese se koriste za održavanje ažuriranja na uređaju. Ako kontrolera uređaj ne možete povezati s internetom putem na fiksnim IP-ovi, nećete moći da biste ažurirali StorSimple uređaj.

> [AZURE.IMPORTANT] Provjerite je li da Vatrozid ne Izmijeni ili dešifriranje sav promet SSL između StorSimple uređaja i Azure.

### <a name="url-patterns-for-firewall-rules"></a>Uzorci URL-a za pravila vatrozida

Mrežni administratori često možete konfigurirati pravila vatrozida za napredne na temelju uzoraka URL-a da biste filtrirali na ulazni i izlazni promet. Uređaj StorSimple i servis za StorSimple upravitelja ovise o drugim aplikacijama Microsoft kao što su Bus servisa Azure, Azure Active Directory kontrola pristupa, račune za pohranu i poslužitelji Microsoft Update. Uzorci URL vezane uz ove aplikacije mogu se konfigurirati pravila vatrozida. Važno je da razumijete možete promijeniti URL uzorke vezane uz ove aplikacije. To shodno je potrebno mrežni administrator za praćenje i ažuriranje pravila vatrozida za vaše StorSimple kao i po potrebi.

Preporučujemo da postavite pravila vatrozida za izlazni promet na temelju StorSimple liberally Fiksna IP adresa u većini slučajeva. Međutim, možete koristiti informacija u nastavku da biste postavili pravila napredni vatrozid koji su potrebni za stvaranje sigurnog okruženja.

> [AZURE.NOTE] Uređaj (izvor) IP-ovi moraju uvijek biti postavljena na sva omogućeni mrežni sučelja. Odredišni IP-ovi mora biti postavljena na [Azure podatkovnog centra IP raspone](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).

#### <a name="url-patterns-for-azure-portal"></a>Uzorci URL-a za portal za Azure
| Uzorak URL-a                                                      | Komponenta/funkcija                                           | IP-ovi uređaja                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | Servis upravitelja StorSimple<br>Servis kontrole programa Access<br>Azure Service Bus| Oblačić s omogućenim mreže sučelja        |
|`https://*.backup.windowsazure.com`|Registracija uređaja| 0 samo podatke|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Opoziva certifikata |Oblačić s omogućenim mrežom sučelja |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Računi za Azure prostora za pohranu i nadzor | Oblačić s omogućenim mreže sučelja        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Poslužitelji Microsoft Update<br>                             | Kontroler samo fiksno IP-ovi               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Kontroler samo fiksno IP-ovi   |
| `https://*.partners.extranet.microsoft.com/*`                    | Paket za podršku                                                  | Oblačić s omogućenim mreže sučelja        |

#### <a name="url-patterns-for-azure-government-portal"></a>Uzorci URL portala državne Azure
| Uzorak URL-a                                                      | Komponenta/funkcija                                           | IP-ovi uređaja                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`   | Servis upravitelja StorSimple<br>Servis kontrole programa Access<br>Azure Service Bus| Oblačić s omogućenim mreže sučelja        |
|`https://*.backup.windowsazure.us`|Registracija uređaja| 0 samo podatke|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Opoziva certifikata |Oblačić s omogućenim mreže sučelja |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Računi za Azure prostora za pohranu i nadzor | Oblačić s omogućenim mreže sučelja        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Poslužitelji Microsoft Update<br>                             | Kontroler samo fiksno IP-ovi               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Kontroler samo fiksno IP-ovi   |
| `https://*.partners.extranet.microsoft.com/*`                    | Paket za podršku                                                  | Oblačić s omogućenim mreže sučelja        |

### <a name="routing-metric"></a>Usmjeravanje mjerenja

Usmjeravanje metriku povezan je s sučelja i pristupnik za usmjeravanje podataka navedena mreža. Usmjeravanje metriku koristi protokol usmjeravanja da biste izračunali najbolje put do navedeni odredište, ako je Pratit će postoji više putova na isto odredište. U donjem metriku usmjeravanje, viša preference.

U kontekstu StorSimple, ako je više mreža sučelja i pristupnika konfigurirani tako da promet kanala usmjeravanje metriku će isporučuju u Reproduciraj da biste utvrdili relativni redoslijed kojim će se koristiti sučelja. Usmjeravanje metriku nije moguće promijeniti korisnik. No možete koristiti u `Get-HcsRoutingTable` cmdlet za ispis u smjeru tablice (i metriku) na uređaju StorSimple. Dodatne informacije o cmdlet Get-HcsRoutingTable implementacije [StorSimple za otklanjanje poteškoća](storsimple-troubleshoot-deployment.md).

Usmjeravanje metričkim algoritama razlikuju se ovisno o verziji softver koji se izvodi na uređaju StorSimple.

**Izdanja prije Update 1**

To obuhvaća verzija softvera prije no što Update 1, kao što su GA, 0,1, 0.2 ili 0,3 izdanje. Na temelju usmjeravanje metrike je na sljedeći način:

   *Zadnje konfiguriran 10 GbE mrežno sučelje > drugih GbE 10 mrežno sučelje > zadnje konfiguriran 1 GbE mrežno sučelje > drugih GbE 1 mrežno sučelje*


**Izdanja počevši od 1 ažuriranja i prije no što ažuriranju 2**

To obuhvaća verzija softvera kao što je 1, 1.1 ili 1,2. Redoslijed temelju usmjeravanje metrike navedena je na sljedeći način:

   *PODATAKA 0 > zadnje konfiguriran 10 GbE mrežno sučelje > drugih GbE 10 mrežno sučelje > zadnje konfiguriran 1 GbE mrežno sučelje > drugih GbE 1 mrežno sučelje*

   U Update 1 postala je metriku usmjeravanje podataka 0 najniže; Dakle, sve u oblaku – promet je usmjerena kroz podataka 0. Zabilježite to ako postoji više od jedne oblaka omogućeno mrežno sučelje na uređaju StorSimple.


**Izdanja počevši od ažuriranju 2**

Ažuriranje 2 ima nekoliko poboljšanja vezane uz mrežni rad i usmjeravanje metriku promijenio. Ponašanje moguće je objašnjeno na sljedeći način.

- Iz skupa unaprijed određenim vrijednosti su dodijeljene sučelje mreže.   

- Razmislite o tablice programa primjeru prikazano u nastavku s vrijednostima dodijeliti različite sučelje mreže kada su oblak – te značajke omogućene oblaka onemogućeno, ali s konfiguriran pristupnik. Imajte na umu vrijednosti Dodijeljeno ovdje primjer samo vrijednosti.


  	| Mrežno sučelje | Oblak omogućeno | Oblak onemogućeno s pristupnika |
  	|-----|---------------|---------------------------|
  	| Podataka 0  | 1            | -                        |
  	| Podaci 1  | 2            | 20                       |
  	| Podataka 2  | 3            | 30                       |
  	| Podaci 3  | 4            | 40                       |
  	| Podaci 4  | 5            | 50                       |
  	| Podataka 5  | 6            | 60                       |


- Redoslijed kojim promet oblaka će usmjerena kroz sučelje mreže je:

    *Podataka 0 > podataka 1 > datum 2 > podataka 3 > podataka 4 > podataka 5*

    To može biti objašnjena u sljedećem primjeru.

    Razmislite o uređaju StorSimple s dva oblaka omogućeno mreže sučelja podataka 0 i 5 podataka. Podataka od 1 do 4 podataka su oblaka-onemogućeno, ali ste konfiguriran pristupnik. Bit će redoslijedom u kojem će se usmjeriti promet za ovaj uređaj:

    *Podataka 0 (1) > podataka 5 (6) > podataka 1 (20) > podataka 2 (30) > podataka 3 (40) > podataka 4 (50)*

    *gdje brojevi u zagradama pokazuju odgovarajući usmjeravanje metriku.*

    Ne uspijete podataka 0, do 5 podataka će se usmjeriti promet oblaka. Given da pristupnik nije konfiguriran na sve ostale mreži podataka 0 i 5 podaci su uvoza, promet oblaka će proći kroz podaci 1.


- Oblačić s omogućenim mrežno sučelje ne uspije, su 3 ponovne pokušaje s 30 drugi odgode povezati sučelja. Ako se svi ponovne pokušaje neće funkcionirati, promet usmjeravanja sljedeći dostupan oblaka omogućeno sučelja kao što je određeno u tablicu usmjeravanja. Ako sve oblaka omogućeno mreže sučelja nije uspjelo, uređaj neće uspjeti iznad druge kontrolerom (bez ponovnog pokretanja u ovom slučaju).

- Ako postoji VIP pogreške za sučelja iSCSI omogućeno mreže, bit će 3 ponovne pokušaje s odgodu dvije sekunde. Takvo ponašanje stayed ima isti iz prethodnog izdanja. Ako se sva sučelja mreže iSCSI neće funkcionirati, kontroler prebacivanje pojavit će se (accompanied tako da ponovno pokretanje).


- Upozorenja i podignut na uređaju StorSimple kada VIP pogreške. Dodatne informacije, idite na [upozorenja brzi pregled](storsimple-manage-alerts.md).

- Pomoću ponovne pokušaje, iSCSI će nadjačavaju oblaka.

    Razmislite o sljedećem primjeru: A StorSimple uređaj ima dvije sučelje mreže omogućena, podataka 0 i 1 podataka. 0 podaci oblaka omogućenim dok je podaci 1 i oblaka i iSCSI omogućena. Nema drugih sučelje mreže na ovom uređaju omogućena za oblak ili iSCSI.

    Ako je 1 podataka ne uspije dali je zadnji iSCSI mrežnog sučelja, to će rezultirati kontroler prebacivanje 1 podatke na drugim kontroleru.


### <a name="networking-best-practices"></a>Najbolje prakse za povezivanje s mrežom

Osim gornje umrežavanje preduvjeta, postigli optimalne performanse rješenje StorSimple, provjerite drži sljedeće najbolje prakse:

- Provjerite imaju li uređaj StorSimple namjenski 40 MB/s propusnosti (ili više njih) dostupne cijelo vrijeme. Ovaj propusnosti treba nije moguće zajednički koristiti (ili dodijeljeni moraju se zajamčiti pomoću pravila QoS) s drugim aplikacijama.

- Provjerite je li mrežna veza s Internetom dostupne cijelo vrijeme. Povremeni gubitak ili nepouzdanih internetske veze s uređaja, uključujući nema mogućnost povezivanja s Internetom neće, rezultirat će nepodržane konfiguracije.

- Da biste izdvojili promet iSCSI i oblaka tako da imate namjenski sučelja mrežom na uređaju za pristup iSCSI i oblaka. Dodatne informacije potražite u članku [Izmjena sučelje mreže](storsimple-modify-device-config.md#modify-network-interfaces) na uređaju StorSimple.

- Korištenje konfiguraciju vezu zbrajanja kontrola Protocol (LACP) za sučelja za mrežu. Ovo je nepodržane konfiguracije.


## <a name="high-availability-requirements-for-storsimple"></a>Visoke dostupnosti preduvjeti za StorSimple

Hardverska platforma koju ste dobili uz rješenje StorSimple ima dostupnosti i pouzdanosti značajke koje omogućuju temelj za infrastrukturu iznimno dostupan, pogreške prostora za pohranu u vašem podatkovnog centra. No postoje preduvjeti i najbolje prakse koji moraju biti usklađene sa da biste lakše osiguraj StorSimple rješenje. Prije implementacije StorSimple pažljivo pregledajte sljedeće preduvjete i najbolje prakse za StorSimple uređaja i računala povezano glavno računalo.

Dodatne informacije o nadzora i održavanja hardverske komponente StorSimple uređaja potražite [pomoću servisa StorSimple Upravitelj praćenje hardverske komponente i status](storsimple-monitor-hardware-status.md) i [StorSimple hardver komponente zamjena](storsimple-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Preduvjeti za visoke dostupnosti i postupke za svoj uređaj StorSimple

Pregledajte sljedeće informacije pažljivo izraditi kako bi visoke dostupnosti StorSimple uređaj.

#### <a name="pcms"></a>PCMs

StorSimple uređaje obuhvaćaju suvišnih, tipkovne-swappable power i hlađenje Module (PCMs). Svaki uo ima dovoljno kapacitet za pružanje usluge cijeli nosačima. Da biste bili sigurni visoke dostupnosti, mora biti instaliran i PCMs.

- Povezivanje vaše PCMs izvori za različite power možete unijeti dostupnost ne uspijete izvor napajanja.
- Ako je uo ne uspije, zatražite zamjenu odmah.
- Uklanjanje nije uspjelo uo samo kada su zamjena i spremni ste da biste ga instalirali.
- Istovremeno ukloniti i PCMs. Modul uo sadrži sigurnosne kopije baterija modul. Uklanjanje obaju na PCMs rezultirat će zatvaranja bez baterija zaštite i stanje uređaj neće se spremiti. Dodatne informacije o baterija otvorite [Zadrži sigurnosne kopije baterija modul](storsimple-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Moduli kontroler

Uređaji StorSimple obuhvaćaju suvišnih, tipkovne-swappable kontroler module. Moduli kontroler raditi na aktivno/pasivni način. U bilo kojem trenutku, jednog modula kontroler je aktivna i je pružanje usluge, dok je u modulu kontroler pasivni. Modul pasivni kontroler uključen i postaje funkcionalna ako modul aktivni kontroler ne uspije ili ukloniti. Svakom modulu kontroler ima dovoljno kapacitet za pružanje usluge cijeli nosačima. Da biste bili sigurni visoke dostupnosti mora biti instaliran i moduli kontroler.

- Provjerite je li oba kontroler modula instaliranih cijelo vrijeme.

- Ako modula kontroler ne uspije, odmah zatražiti zamjenski.

- Uklanjanje modula nije uspjelo kontroler samo kada su zamjena i spremni ste da biste ga instalirali. Uklanjanje modula za prošireni razdoblja utječu na sustava zraka i zato hlađenje sustav.

- Provjerite da mrežne veze i moduli kontroler jednaki, a sučelja povezanom mrežnom ste je identične mrežna konfiguracija.

- Ako modula kontroler ne uspije ili mora zamjenski, provjerite je nalazi li se u modulu kontroler u aktivni stanje prije zamjene nije uspjelo kontroler modul. Da biste provjerili je li kontroler aktivna, idite na [otkrivanje kontrolerom aktivni na uređaju](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Uklanjanje i moduli kontroler u isto vrijeme. Ako je kontroler prebacivanje u tijeku, isključite modul čekanja kontroler ili ga uklonite s na nosačima.

- Kad prebacivanje na kontroler pričekajte najmanje pet minuta prije uklanjanja ili kontroler modul.

#### <a name="network-interfaces"></a>Sučelje mreže

StorSimple uređaj kontroler moduli svaki imaju četiri 1 gigabita i dva 10 sučelje gigabitna Ethernet mreže.

- Provjerite da mrežne veze i moduli kontroler jednaki, a mrežom sučelja modul sučelja kontroler povezani da bi se identične mrežnoj konfiguraciji.

- Kada je to moguće, implementirati mrežne veze preko različite skretnice da biste bili sigurni dostupnost usluge u slučaju pogreške uređaj mreže.

- Kada isključujete jedini ili posljednje preostale iSCSI omogućeno sučelja (s IP-ovi dodijeljeni), najprije onemogućuje sučelje i isključite na kabeli. Ako je sučelje nije priključen prvi put, a zatim uzrokovat će aktivne kontroler uvoza putem s kontrolerom pasivni. Ako kontrolerom pasivni ima i njegove odgovarajuće sučelja isključiti, zatim oba kontrolera će ponovno pokrenuti više puta prije podmirenje na jedan kontroler.

- Povezivanje najmanje dva sučelja podataka s mrežom u svakom modulu kontroler.

- Ako ste omogućili dviju 10 GbE sučelja, uvođenje one svim različite skretnice.

- Kada je to moguće, koristite MPIO na poslužiteljima da bi možete tolerate poslužitelji vezu, mreži ili sučelje nije uspjelo.

Dodatne informacije o umrežavanje uređaja radi visoke dostupnosti i performanse, otvorite [Instaliranje StorSimple 8100 uređaja](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) ili ponovno [instalirati StorSimple 8600 uređaj](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSDs i HDDs

Uređaji StorSimple obuhvaćaju pune stanje diskova (SSDs), a tvrdom disku pogona (HDDs) koji su zaštićeni pomoću zrcaljene razmake. Korištenje zrcaljene razmake osigurava uređaj moći tolerate neuspjeh SSDs ili HDDs.

- Provjerite jesu li instalirani svi moduli SSD i podizanje tvrdog diska.

- Ako SSD ili podizanje tvrdog diska ne uspije, zatražite zamjenu odmah.

- Ako SSD ili podizanje tvrdog diska ne uspije ili zahtijeva zamjenski, provjerite je li uklonili samo SSD ili podizanje tvrdog diska koji zahtijeva zamjena.

- Uklanja više SSD ili podizanje tvrdog diska iz sustava u bilo kojem trenutku u vremenu.
Nije uspjelo 2 ili više diskova određene vrste (podizanje tvrdog diska, SSD) ili uzastopnih pogreške unutar okvira kratko vrijeme može uzrokovati sustava im i potencijalni gubitak podataka. Ako se to dogodi, [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) za pomoć.

- Tijekom zamjena praćenje **Stanja hardvera** u stranice za **Održavanje** za pogone u SSDs i HDDs. Zelena kvačica status upućuje na to da se diskova dobar ili u redu, dok je crveni uskličnik pokažite pokazuje nije uspjelo SSD ili podizanje tvrdog diska.

- Preporučujemo da konfigurirate snimki oblaka za sve količine koje su vam potrebne da biste zaštitili u slučaju sustava.

#### <a name="ebod-enclosure"></a>S EBOD prilozima

Model uređaja StorSimple 8600 sadrži prošireni prešla od diskova (EBOD) s prilozima osim primarnog s prilozima. Na EBOD sadrži EBOD kontrolera i tvrdom disku pogona (HDDs) koji su zaštićeni pomoću zrcaljene razmake. Korištenje zrcaljene razmake osigurava uređaj moći tolerate pogreška jedan ili više HDDs. Primarni s prilozima kroz suvišnih SAS kabela povezan je s prilozima EBOD.

- Provjerite jesu li oba module za kontroler s prilozima EBOD i SAS kabeli, a svi diskovnih pogona tvrdom instaliraju se cijelo vrijeme.

- Ako je modula za EBOD s prilozima kontroler ne uspije, zatražite zamjenu odmah.

- Ako je modula za EBOD s prilozima kontroler ne uspije, provjerite nalazi li se u modulu kontroler aktivni prije nego što zamijenite modul nije uspjelo. Da biste provjerili je li kontroler aktivna, idite na [otkrivanje kontrolerom aktivni na uređaju](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Tijekom zamjenski modul kontroler programa EBOD neprestano praćenje status komponente u servisu Upravitelj StorSimple tako da pristupite **održavanja** > **hardver status**.

- Ako kabel SAS ne uspije ili zahtijeva zamjenski (Microsoft Support mora biti uključen da biste takve određivanja), provjerite je li uklonite samo SAS kabela koji zahtijeva zamjena.

- Istovremeno ukloniti oba kabeli SAS iz sustava u bilo kojem trenutku u vremenu.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Visoke dostupnosti preporuke za računala glavnog računala

Pažljivo pregledajte najbolje prakse da biste bili sigurni visoke dostupnosti glavno računalo povezano s uređajem StorSimple.

- Konfiguriranje StorSimple s [dva čvor datoteke poslužitelja klaster konfiguracijama][1]. Uklanjanjem jedan točke pogreške i izrade u zalihosti na strani glavno računalo cijelu rješenje postane iznimno dostupan.

- Korištenje neprestano dostupna (CA) dionice dostupna uz Windows Server 2012 (SMB 3.0) za visoke dostupnosti tijekom prebacivanje kontrolera prostora za pohranu. Za dodatne informacije o konfiguriranju klastere poslužitelj datoteka i neprestano dostupno zajedničko korištenje sa sustavom Windows Server 2012, pogledajte ovaj [pokazni videozapis](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Daljnji koraci

- [Dodatne informacije o ograničenjima StorSimple sustava](storsimple-limits.md).
- [Upute za implementaciju StorSimple rješenja](storsimple-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
