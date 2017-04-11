<properties 
   pageTitle="Što je StorSimple? | Microsoft Azure" 
   description="U članku se opisuje StorSimple tiering, uređaj, virtualnog uređaja, servise i upravljanje pohranom, a predstavlja ključa izraze koji se koriste u StorSimple." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="10/05/2016"
   ms.author="v-sharos@microsoft.com"/>

# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>Niz StorSimple 8000: rješenje za pohranu hibridnog oblaka

## <a name="overview"></a>Pregled

Dobro došli u Microsoft Azure StorSimple, rješenje za integriranu prostora za pohranu koji upravlja prostora za pohranu zadataka između lokalnog uređaja i sustava Microsoft Azure za pohranu u oblaku. StorSimple je učinkovitog učinkovit i jednostavno moguće upravljati prostora za pohranu područje mreže (SAN) rješenje koje se uklanja mnoge problema i troškove pridružene zaštita prostora za pohranu i podataka tvrtke. IT koristi vlasničkih uređaj niz StorSimple 8000 integrira servise u oblaku, a pruža skup alata za upravljanje za objedinjenog prikaz svih enterprise prostor za pohranu, uključujući za pohranu u oblaku. (Informacije o implementaciji StorSimple objaviti na web-mjestu Microsoft Azure odnosi se na StorSimple 8000 samo niz uređaja. Ako koristite uređaj StorSimple 5000 7000 niz, otvorite [Pomoć StorSimple](http://onlinehelp.storsimple.com/).)

StorSimple koristi [tiering prostora za pohranu](#automatic-storage-tiering) za upravljanje pohranjene podatke preko različitih medij za pohranu. Trenutni radni skup pohranjuju lokalno na pogonima pune stanje (SSDs) te podatke koji se koriste rjeđe pohranjen na tvrdom disku pogonima (HDDs) i arhiviranje podataka pomiču se s oblakom. Nadalje, StorSimple koristi Poništavanje duplikacije i sažimanje da biste smanjili količinu prostora za pohranu koji koristi podatke. Dodatne informacije potražite [Poništavanje duplikacije i sažimanja](#deduplication-and-compression). Definicija drugih ključa uvjete i koncepata koji se koriste u dokumentaciji niz StorSimple 8000 potražite [StorSimple terminologiju](#storsimple-terminology) na kraju ovog članka.

S StorSimple ažuriranju 2 možete prepoznati odgovarajuće jedinice kao *lokalno prikvačene* primarni podataka ostaje lokalne na uređaju i ne sloju u oblak. Omogućuje vam pokretanje radnih opterećenja koji su osjetljivi na Latencija oblaka, kao što je SQL i virtualnog računala radnih opterećenja, lokalno prikvačene količine korištenje oblaka za sigurnosno kopiranje. Dodatne informacije o lokalno prikvačene količine potražite u članku [Korištenje upravitelja StorSimple servisa za upravljanje količine](storsimple-manage-volumes-u2.md). 

Ažuriranje 2 omogućuje vam stvaranje uređaja virtualne StorSimple iskoristiti sve prednosti malo latencies i visoke performanse koje ste dobili od Azure premium prostora za pohranu. Dodatne informacije o StorSimple premium virtualne uređajima potražite u članku [uvođenje i upravljanje StorSimple virtualnog uređaja u Azure](storsimple-virtual-device-u2.md). Dodatne informacije o Azure premium prostora za pohranu, idite na [prostora za pohranu Premium: visokih performansi prostor za pohranu radnih opterećenja virtualnog računala Azure](../storage/storage-premium-storage.md).

Uz upravljanje pohranom značajke zaštite podataka StorSimple omogućuju stvaranje osvježavati zakazano sigurnosne kopije i sprema ih lokalno i u oblaku. Sigurnosno kopiranje uzimaju se u obliku rastuće snimke, što znači da ih je moguće stvoriti i brzo vratiti. Oblak snimke može biti kritično važne u scenarijima za oporavak Izrada jer zamjena sustavi sekundarne prostora za pohranu (kao što su sigurnosnu kopiju na traci), a omogućuju vam da biste vratili podatke na podatkovnim centrom ili na svaki drugi web-mjesta ako je potrebno.

![Ikona videoprikaz:](./media/storsimple-overview/video_icon.png) Pogledajte videozapis pogledajte Uvod u Microsoft Azure StorSimple.

> [AZURE.VIDEO storsimple-hybrid-cloud-storage-solution]

## <a name="why-use-storsimple"></a>Zašto koristiti StorSimple?

U sljedećoj su tablici opisani su neki od ključne prednosti koji nudi Microsoft Azure StorSimple.

| Značajka | Pogodnost |
|---------|---------|
|Prozirna Integracija | Microsoft Azure StorSimple koristi protokol iSCSI rade nevidljivo povezati funkcije pohrane podataka. Provjerava podatke pohranjene u oblaku, s podatkovnim centrom ili na udaljenim poslužiteljima pojavit će se spremiti na jednom mjestu.|
|Troškovi smanjene spremanja|Microsoft Azure StorSimple dodjeljuje potrebne lokalne ili za pohranu u oblaku da bi odgovarao trenutni zahtjeve i proširuje za pohranu u oblaku samo kada je to potrebno. Dodatno smanjuje preduvjeti za pohranu i troškovima uklanjanjem suvišnih verzije s istim podacima (Poništavanje duplikacije) i pomoću spajanja.|
|Upravljanje pohranom pojednostavljeni|Microsoft Azure StorSimple donosi sustava alate za administraciju koje možete koristiti za konfiguriranje i upravljanje podatke pohranjene na lokalnom na udaljeni poslužitelj i u oblaku. Osim toga, možete upravljati sigurnosno kopiranje i vraćanje funkcije iz dodatku Microsoft Management Console (BLOG). StorSimple nudi zaseban, neobavezno sučelje koje možete koristiti da biste proširili StorSimple upravljanje i podataka zaštitu servisa sadržaju pohranjenom na poslužiteljima sustava SharePoint. |
|Poboljšana Izrada oporavak i usklađenost|Microsoft Azure StorSimple zahtijevaju prošireni oporavku. Umjesto toga vraća podatke kao što je to potrebno. To znači da normalno operacije možete nastaviti s minimalnim prekidima. Osim toga, možete konfigurirati pravila da biste odredili sigurnosne kopije rasporede i zadržavanje podataka.
|Mobilnost podataka|Podaci prenijeli Microsoft Azure servisima u oblaku možete pristupiti iz drugih web-mjesta u svrhu oporavak i migracije. Osim toga, možete koristiti StorSimple da biste konfigurirali StorSimple virtualne uređajima na virtualnim strojevima (VMs) radi u Microsoft Azure. Na VMs možete koristiti virtualne uređaje da biste pristupili pohranjene podatke za testiranje ili oporavak svrhe.|
|Podrška za drugim davateljima usluga oblaka |Niz StorSimple 8000 sa softverom za ažuriranje 1 ili noviji podržava Amazon S3 s RRS, HP i OpenStack cloud servisa, kao i Microsoft Azure. (I dalje ćete račun sustava Microsoft Azure prostora za pohranu radi upravljanja uređaj.) Dodatne informacije potražite za [što je novo u Update 1.2](storsimple-update1-release-notes.md#whats-new-in-update-12).|
|Neprekidno poslovanje | Ažuriranje 1 ili noviji omogućuje je nova značajka za migraciju koja omogućuje StorSimple 5000 7000 niz korisnika će se migrirati svoje podatke na uređaj StorSimple 8000 niz.|
|Dostupnost na portalu za Azure državne ustanove | Nije dostupno na portalu za državne ustanove Azure StorSimple ažuriranje 1 ili noviji. Dodatne informacije potražite u članku [uvođenja uređaju StorSimple lokalnog na portalu za državne ustanove](storsimple-deployment-walkthrough-gov.md).|
|Zaštita podataka i dostupnosti | Niz StorSimple 8000 ažuriranje 1 ili noviji podržava Zone suvišnih prostora za pohranu (ZRS), osim lokalno suvišnih prostora za pohranu (LRS) i zemlj suvišnih pohranu (GRS). Pogledajte [Ovaj članak na mogućnosti zalihosti Azure pohrane](https://azure.microsoft.com/documentation/articles/storage-redundancy/) detalje ZRS.|
|Podrška za ključne aplikacije | S StorSimple ažuriranju 2 možete prepoznati odgovarajuće jedinice lokalno prikvačene. Tu mogućnost provjerava je li podatke potrebne ključnih aplikacija ne tiered u oblak. Lokalno prikvačene količine ne podložne su oblaka latencies ili problema s povezivanjem. Dodatne informacije o lokalno prikvačene količine potražite u članku [Korištenje upravitelja StorSimple servisa za upravljanje količine](storsimple-manage-volumes-u2.md).|
|Niske latencije i visoke performanse | Ažuriranju 2 za StorSimple omogućuje vam stvaranje virtualne uređaje koji se iskoristiti visoke performanse niske latencije značajke Azure premium prostora za pohranu. Dodatne informacije o StorSimple premium virtualne uređajima potražite u članku [uvođenje i upravljanje StorSimple virtualnog uređaja u Azure](storsimple-virtual-device-u2.md).|

![Ikona videoprikaz:](./media/storsimple-overview/video_icon.png) pogledajte [Ovaj videozapis](https://www.youtube.com/watch?v=4MhJT5xrvQw&feature=youtu.be) da biste saznali StorSimple 8000 niz značajke i pogodnosti paketa.

## <a name="storsimple-components"></a>StorSimple komponente

Rješenja za Microsoft Azure StorSimple obuhvaća sljedeće komponente:

- **Microsoft Azure StorSimple uređaja** – polja za pohranu lokalnog hibridnog koja sadrži SSDs i HDDs, zajedno s suvišnih kontrolera i automatsko prebacivanje mogućnosti. Na kontrolera Upravljanje prostorom za pohranu tiering, smještanje trenutno korištenih (ili tipkovni) podataka na lokalno spremište (u na uređaj ili na lokalnim poslužiteljima), tijekom premještanja manje često korištenih podataka s oblakom.
- **StorSimple virtualnog uređaja** – poznat i kao potražite virtualne StorSimple je verzija softvera StorSimple uređaja na kojem replicira arhitektura i većina mogućnosti fizičke hibridnog uređaj za pohranu. Pokreće se StorSimple virtualnog uređaja na jedan čvor u Azure virtualnog računala. Premium virtualne uređaji, koji iskoristili Azure premium prostora za pohranu, nisu dostupne u ažuriranju 2 ili novije.
- **Servis upravitelja StorSimple** – datotečni nastavak Azure klasični portal omogućuje upravljanje StorSimple uređaja ili StorSimple virtualnog uređaja putem sučelja Jednostruko web-mjesto. Servis StorSimple Manager možete koristiti za stvaranje i upravljanje uslugama, prikaz i upravljanje uređajima, pregled upozorenja, upravljanje količine, i prikaz i upravljanje sigurnosne pravilnike i katalog sigurnosne kopije.
- **Windows PowerShell za StorSimple** – sučelje naredbenog retka koje možete koristiti da biste upravljali StorSimple uređaja. Windows PowerShell za StorSimple sadrži značajke koje omogućuju registrirali uređaj StorSimple, konfiguriranje mrežnog sučelja na uređaju, instalirajte određene vrste ažuriranja, rješavanje problema s uređajem tako da pristupite sesiju podršku te promijenite stanje uređaja. Možete pristupiti komponente Windows PowerShell za StorSimple povezivanjem s konzole za serijski ili pomoću komponente Windows PowerShell remoting.
- **Cmdleti za azure PowerShell StorSimple** – zbirka cmdleta ljuske Windows PowerShell koji omogućuju da biste automatizirali zadatke razini usluge i migriranja iz naredbenog retka. Dodatne informacije o Azure PowerShell cmdleti za StorSimple, idite na [cmdlet referencu](https://msdn.microsoft.com/library/dn920427.aspx).
- **Upravitelj snimka StorSimple** – na BLOG dodatak koji koristi glasnoću grupe i servis za kopiranje Windows glasnoću sjene za generiranje aplikacije dosljedan sigurnosne kopije. Osim toga, možete koristiti StorSimple snimku upravitelja za stvaranje sigurnosne kopije rasporede i Kloniraj ili vraćanje količine. 
- **StorSimple prilagodnik za SharePoint** – alat koji proziran proširuje Microsoft Azure StorSimple prostora za pohranu podataka zaštitu i farmama sustava SharePoint Server pri čemu StorSimple prostora za pohranu može pregledavati i kojima se upravlja sa središnje administracije sustava SharePoint portal.

Dijagramu u nastavku nudi više razine prikaz Microsoft Azure StorSimple arhitektura i komponenata.

![StorSimple arhitekture](./media/storsimple-overview/overview-big-picture.png)

U sljedećim se odjeljcima opisuju svaki od tih komponenti detaljniji i objašnjavaju kako rješenje slaže podataka dodjeljuje prostora za pohranu i olakšava zaštita podataka i upravljanje pohranom. Zadnji se odjeljku nalaze definicije za neke važne uvjete i koncepata vezane uz StorSimple komponente i njihovih upravljanje.

## <a name="storsimple-device"></a>StorSimple uređaj

Microsoft Azure StorSimple uređaj je polja za pohranu lokalnog hibridnog koje nudi primarni prostora za pohranu i iSCSI pristup podacima koji su pohranjeni na njoj. Upravlja komunikaciju s za pohranu u oblaku, a omogućuje da biste bili sigurni sigurnost i povjerljivost sve podatke koji je pohranjen na Microsoft Azure StorSimple rješenja.

Uređaj StorSimple obuhvaća SSDs i HDDs tvrdom disku pogona, kao i podrška za Klasteriranje i automatsko prebacivanje. Sadrži zajedničke procesor, zajedničkog prostora za pohranu i dva zrcaljene kontrolera. Svaki kontroler nudi sljedeće:

- Povezivanje s glavnim računalom
- Do šest mrežne priključke za povezivanje s lokalne mreže (LAN-a)
- Nadzor hardvera
- Osobe koje nisu uklonjiv radnom memorijom (NVRAM) zadržati podatke, čak i ako se prekine power
- Klaster – umu ažuriranje da biste upravljali softverska ažuriranja na poslužiteljima klasteru prebacivanje tako da se ažuriranja ne minimalnog ili ne utječe na dostupnost usluge
- Skupine usluga, koji funkcionira kao pozadinske klaster, pruža visoke dostupnosti i minimiziranje sve štetnih do kojih može doći ako podizanje tvrdog diska ili SSD ne uspije ili je izvanmrežno

Samo jedan kontroler je aktivan u bilo kojem trenutku u vremenu. Aktivni kontroler ne uspije, drugi kontroler postaje aktivan automatski. 

Dodatne informacije potražite [StorSimple hardverske komponente i status](storsimple-monitor-hardware-status.md).

## <a name="storsimple-virtual-device"></a>StorSimple virtualnog uređaja

StorSimple možete koristiti da biste stvorili virtualnog uređaja koji umnaža arhitektura i mogućnosti fizičke hibridnog uređaj za pohranu. Pokreće virtualnog uređaja StorSimple (poznat i kao u StorSimple virtualne potražite) na jedan čvor u Azure virtualnog računala. (Virtualnog uređaja moguće je stvoriti samo na Azure virtualnog računala. Ne možete stvoriti na StorSimple uređaja ili putem lokalnog poslužitelja.) 

Virtualnog uređaja sadrži sljedeće značajke:

- Ponaša se kao fizičke potražite i ponuditi sučelja iSCSI na virtualnim računalima sustava u oblaku. 
- Stvaranje neograničen broj virtualne uređaja u oblak i uključivanje ih uključiti i isključiti prema potrebi. 
- Pridonosi lokalnim okruženjima Izrada oporavak, razvoj i testiranje scenariji kao zamjenu za, a omogućuju dohvaćanje razini stavke iz sigurnosne kopije. 

Ažuriranje 2 ili noviji StorSimple virtualnog uređaja dostupna je u dva modela: 8010 uređaj (prijašnji naziv modela 1100) i 8020 uređaja. Uređaj 8010 ima najveći kapacitet 30 TB. 8020 uređaj, koji traje prednost Azure premium prostora za pohranu, ima najveći kapacitet 64 TB. (U lokalnom razine Azure premium prostora za pohranu sprema podatke na SSDs dok standardne prostora za pohranu sprema podatke na HDDs.) Imajte na umu da moraju imati račun za pohranu za Azure premium da biste koristili premium prostora za pohranu. Dodatne informacije o premium prostora za pohranu, idite na [prostora za pohranu Premium: visokih performansi prostor za pohranu radnih opterećenja virtualnog računala Azure](../storage/storage-premium-storage.md).

Dodatne informacije o StorSimple virtualnog uređaja, idite na [uvođenje i upravljanje StorSimple virtualnog uređaja u Azure](storsimple-virtual-device-u2.md).

## <a name="storsimple-manager-service"></a>Servis upravitelja StorSimple

Microsoft Azure StorSimple nudi utemeljen na webu korisničko sučelje (servis upravitelja StorSimple) koji vam omogućuje središnje Upravljanje podatkovnim centrom i prostora za pohranu u oblaku. Servis StorSimple Manager možete koristiti za izvođenje sljedećih zadataka:

- Konfiguriranje postavki sustava za uređaje StorSimple.
- Konfiguriranje i upravljanje sigurnosnim postavkama za uređaje StorSimple.
- Konfiguriranje vjerodajnica u oblaku i svojstva.
- Konfiguriranje i upravljanje količine na poslužitelju.
- Konfiguriranje glasnoću grupe.
- Sigurnosno kopiranje i vraćanje podataka.
- Praćenje performansi.
- Pregledajte postavke sustava i prepoznavanje mogućih problema.

Servis StorSimple Manager možete koristiti za izvođenje svih administrativnih zadataka osim onih koje je potrebno sustava put, kao što su početnog postavljanja i instalaciju ažuriranja.

Dodatne informacije, idite na članak [Korištenje StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>Windows PowerShell za StorSimple

Windows PowerShell za StorSimple sadrži sučelje naredbenog retka koje možete koristiti za stvaranje i upravljanje servisom Microsoft Azure StorSimple i postavljanje i praćenje StorSimple uređaja. To je sučeljem naredbenog retka komponente Windows PowerShell – temelji, koji obuhvaća namjenski cmdleti za upravljanje StorSimple uređajem. Windows PowerShell za StorSimple sadrži značajke koje omogućuju vam:

- Registrirajte uređaj.
- Konfiguriranje mrežnog sučelja na uređaju.
- Instalirajte određene vrste ažuriranja.
- Rješavanje problema s uređajem tako da pristupite sesiju podrška.
- Promjena stanja uređaja.

Možete pristupiti komponente Windows PowerShell za StorSimple serijski konzoli (na glavnom računalu izravno povezani uređaj) ili daljinski pomoću komponente Windows PowerShell remoting. Imajte na umu da neke komponente Windows PowerShell za StorSimple zadatke, kao što je registracija početne uređaja, mogu izvršiti samo na konzoli za serijski. 

Dodatne informacije potražite na [Korištenje komponente Windows PowerShell za StorSimple za administriranje uređaj](storsimple-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure cmdleta ljuske PowerShell StorSimple

Cmdleti za Azure PowerShell StorSimple su zbirka cmdleta ljuske Windows PowerShell koji omogućuju da biste automatizirali zadatke razini usluge i migriranja iz naredbenog retka. Dodatne informacije o Azure PowerShell cmdleti za StorSimple, idite na [cmdlet referencu](https://msdn.microsoft.com/library/dn920427.aspx).

## <a name="storsimple-snapshot-manager"></a>Upravitelj StorSimple snimke

Upravitelj snimku StorSimple je Microsoft Management Console (BLOG) dodatku koje možete koristiti da biste stvorili dosljedan, točke u vrijeme sigurnosne kopije lokalno i podataka iz oblaka. Dodatak pokreće na koji se temelji na sustavu Windows Server – glavnog računala. Možete koristiti brze snimke Upravitelj StorSimple:

- Konfiguriranje, sigurnosno kopiranje i brisanje količine.
- Konfiguriranje glasnoću grupe da biste bili sigurni sigurnosne kopije podataka je aplikacija dosljedni.
- Upravljanje pravilnicima za sigurnosne kopije tako da je unaprijed određenim rasporedu sigurnosne kopije i spremanje podataka u navedeno mjesto (lokalno ili u oblaku).
- Vraćanje količine i pojedinačne datoteke.

Sigurnosno kopiranje snimaju kao snimke stanja koji snimanje samo promjene nakon zadnje snimke snimljena i zahtijevaju znatno manje prostora za pohranu od puno sigurnosne kopije. Možete stvoriti sigurnosnu kopiju rasporeda ili poduzeti odmah sigurnosne kopije prema potrebi. Osim toga, možete koristiti StorSimple snimke Upravitelj uspostaviti pravilnika o zadržavanju kontrole snimke koliko će se spremiti. Ako naknadno morate vratiti podatke iz sigurnosne kopije, Upravitelj snimka StorSimple omogućuje birate iz kataloga lokalne ili oblaka snimke. 

Ako se pojavi na Izrada ili ako morate vratiti podatke iz nekog drugog razloga, StorSimple snimke Manager vraća ga postupno kao što je to potrebno. Vraćanje podataka ne zahtijeva da isključite cijelog sustava dok vratiti datoteku, zamijenite opreme ili premještanje operacije na drugo web-mjesto.

Dodatne informacije potražite na [što je upravitelj StorSimple snimke?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>StorSimple prilagodnik za SharePoint

Microsoft Azure StorSimple obuhvaća StorSimple prilagodnik za SharePoint, neobavezna komponenta koja prozirna proširuje StorSimple prostora za pohranu i podataka značajke zaštite na farmama sustava SharePoint server. Prilagodnik funkcionira s davatelja udaljene spremišta blobova platforme (RBS) i značajku SQL Server RBS omogućujući vam da biste premjestili blob polja na poslužitelju sigurnosnu kopiju sustava Microsoft Azure StorSimple. Microsoft Azure StorSimple zatim sprema BLOB podatke lokalno i u oblaku, ovisno o korištenju.

StorSimple prilagodnik za SharePoint upravlja se putem unutar portala središnje administracije sustava SharePoint. Zbog toga ostaje središnje upravljanje SharePoint, a sve prostora za pohranu čini se da je u farmi sustava SharePoint.

Dodatne informacije potražite u članku [StorSimple](storsimple-adapter-for-sharepoint.md)prilagodnik za SharePoint. 

## <a name="storage-management-technologies"></a>Prostor za pohranu upravljanje tehnologije
 
Osim namjenski StorSimple uređaj, virtualnog uređaja i druge komponente, Microsoft Azure StorSimple koristi sljedeće tehnologije softver za brz pristup podacima i smanjiti potrošnju prostora za pohranu:

- [Automatsko spremanje tiering](#automatic-storage-tiering) 
- [Tanki dodjele resursa](#thin-provisioning) 
- [Poništavanje duplikacije i sažimanje](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Automatsko spremanje tiering

Microsoft Azure StorSimple automatski slaže podataka u logičke razine na temelju trenutnog korištenja, dobi i odnosa na druge podatke. Podaci koji je najaktivnije se pohranjuju lokalno, dok manje aktivne i neaktivne podataka automatski migrirati u oblak. Sljedeći dijagram prikazuje takvog prostora za pohranu.
 
![Razine StorSimple prostora za pohranu](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Da biste omogućili brzi pristup, StorSimple sprema vrlo aktivni podatke (tipkovni) na SSDs StorSimple uređaja. Sprema podatke koji se koriste povremeno (Toplo podataka) na HDDs u uređaj ili na poslužiteljima na s podatkovnim centrom. Pomicanje Neaktivni podatke, sigurnosno kopiranje podataka, a podaci se zadržavaju za arhiviranje ili svrhe usklađenost s oblakom. 

>[AZURE.NOTE] U ažuriranju 2 ili noviju verziju, možete odrediti glasnoću lokalno prikvačene, u tom slučaju podaci ostaju na lokalnim uređajem i nije tiered u oblak. 

StorSimple prilagođava i preslaguju podataka, a dodjele kao uzoraka korištenja i spremanje promjena. Na primjer, neke informacije može postati manje aktivni tijekom vremena. Kao ona postaje progresivno manje aktivna, on je migrirati iz SSDs HDDs, a zatim u oblak. Ako taj istih podataka ponovno postaje aktivna, migrirati natrag na uređaj za pohranu.

Postupak tiering prostora za pohranu dogodit će se na sljedeći način:

1. Administrator sustava postavlja na račun za Microsoft Azure oblaka za pohranu.
2. Administrator koristi serijski konzole i StorSimple Upravitelj servisa (koja se izvodi na portalu za Azure klasični) za konfiguraciju poslužitelja uređaja i datoteke stvaranje pravila zaštite količine i podatke. Lokalnog računala (kao što su datoteke poslužitelji) koristiti Internet Small računala sustava sučelje (iSCSI) da biste pristupili StorSimple uređaja.
3. Na početku, StorSimple sprema podatke na brz sloju SSD uređaja.
4. Kao što je sloju SSD izvršenja kapaciteta, StorSimple deduplicates komprimira najstarije podatkovni blokovi i premješta je u sloju podizanje tvrdog diska.
5. Kapacitet pristupa sloju podizanje tvrdog diska, StorSimple šifrira najstarije podatkovni blokovi i šalje ih sigurno prostora za pohranu računa sustava Microsoft Azure putem HTTP.
6. Microsoft Azure stvara više replike podataka u njegov podatkovnog centra i Udaljena podatkovnog centra jamči da podataka može oporaviti ako se pojavi na Izrada. 
7. Kada datotečni poslužitelj zahtijeva podatke pohranjene u oblaku, StorSimple vraća je jednostavno i sprema kopiju na sloju SSD StorSimple uređaja.

### <a name="thin-provisioning"></a>Tanki dodjele resursa

Tanki dodjeljivanje je tehnologija virtualizacije u kojem se dostupan prostor za pohranu pojavljuje premašuje fizičke resurse. Umjesto unaprijed rezervirati dovoljno prostora za pohranu, StorSimple koristi tanke dodjele resursa želite dodijeliti samo dovoljno prostora za zadovoljava preduvjete za trenutni. Elastic prirode za pohranu u oblaku olakšava takvog jer StorSimple možete povećati ili smanjiti za pohranu u oblaku da bi odgovarao zahtjeva za promjenom. 

>[AZURE.NOTE] Lokalno prikvačene količine nisu thinly resursi. Prostor za pohranu dodijeliti samo za lokalne glasnoću dodjeli je u potpunosti stvaranja glasnoću.

### <a name="deduplication-and-compression"></a>Poništavanje duplikacije i sažimanje

Microsoft Azure StorSimple koristi sažimanje Poništavanje duplikacije i podataka da biste dodatno smanjili preduvjeti za pohranu.

Poništavanje duplikacije smanjuje Ukupna količina podataka pohranjenih uklanjanjem zalihosti pohranjene skupa podataka. Informacije o promijeni, StorSimple zanemaruje nepromijenjen podataka i spremit će samo promjene. Osim toga, StorSimple smanjuje količinu podataka pohranjenih otkrivanje i uklanjanje nepotrebnih podataka. 

>[AZURE.NOTE] Podaci na lokalno prikvačene količine je deduplicated ili komprimirane. Međutim, deduplicated i sažeti sigurnosne kopije lokalno prikvačene količine.

## <a name="storsimple-workload-summary"></a>Radno opterećenje StorSimple sažetka

Sažetak podržani opterećenjem StorSimple je pozivu ispod.

| Scenarij                  | Radno opterećenje                | Podržana |  Ograničenja                                  | Verzija              |
|---------------------------|-------------------------|-----------|------------------------------------------------|----------------------|
| Suradnja             | Zajedničko korištenje datoteka            | Da       |                                                | Sve verzije         |
| Suradnja             | Zajedničko korištenje raspodijeljeno datoteka| Da       |                                                | Sve verzije         |
| Suradnja             | SharePoint              | Da *      |Podržano samo s lokalno prikvačene količine      | Ažuriranje 2 i noviji   |
| Arhiviranje                  | Datoteka vrste Simple arhiviranje   | Da       |                                                | Sve verzije         |
| Virtualizacija            | Virtualnim strojevima        | Da *      |Podržano samo s lokalno prikvačene jedinicama      | Ažuriranje 2 i noviji   |
| Baze podataka                  | SQL                     | Da *      |Podržano samo s lokalno prikvačene količine      | Ažuriranje 2 i noviji   |
| Video surveillance        | Video surveillance      | Da *       |Podržane kada uređaj StorSimple predano samo na ovom radno opterećenje| Ažuriranje 2 i noviji   |
| Sigurnosno kopiranje                    | Sigurnosno kopiranje primarni cilj | Da *      |Podržane kada uređaj StorSimple predano samo na ovom radno opterećenje| Ažuriranje 3 i noviji |
| Sigurnosno kopiranje                    | Sekundarni cilj sigurnosnog kopiranja | Da *      |Podržane kada uređaj StorSimple predano samo na ovom radno opterećenje| Ažuriranje 3 i noviji |

*Da & #42; -Treba primijeniti rješenje smjernice i ograničenja.*

Uređaji StorSimple 8000 niz ne podržava sljedeće radnih opterećenja. Ako implementiran na StorSimple, te radnih opterećenja rezultirat će nepodržane konfiguracije.

-  Liječnička obradu slika
-  Exchange
-  VDI-JA
-  Oracle
-  SAP-A
-  Velikih skupova podataka
-  Podjela sadržaja
-  Pokretanje iz SCSI

Slijedi popis komponenata infrastrukture StorSimple podržane. 

| Scenarij | Radno opterećenje      | Podržana |  Ograničenja                                 | Verzija      |
|----------|---------------|-----------|-----------------------------------------------|--------------|
| Općenito  | Usmjeravanje Express | Da       |                                               |Sve verzije |
| Općenito  | DataCore FC   | Da *       |Podržani s DataCore SANsymphony            | Sve verzije |
| Općenito  | DFSR          | Da *      |Podržano samo s lokalno prikvačene količine     | Sve verzije |
| Općenito  | Indeksiranje      | Da *       |Za tiered količine samo metapodatke indeksiranja podržana (nema podataka).<br>Dovršavanje indeksiranja je podržana za lokalno prikvačene količine.| Sve verzije |
| Općenito  | Antivirusni    | Da *       |Za tiered količine podržani su samo pregledavanje prilikom otvaranja i zatvori.<br> Lokalno prikvačene jedinicama potpuni pregled nije podržan.| Sve verzije |

*Da & #42; -Treba primijeniti rješenje smjernice i ograničenja.*

## <a name="storsimple-terminology"></a>StorSimple terminologija 

Prije implementacije rješenja sustava Microsoft Azure StorSimple, preporučujemo da pregledate sljedeći uvjeti i definicije.

### <a name="key-terms-and-definitions"></a>Ključni Uvjeti i definicije

| Izraz (akronim ili skraćenica) | Opis |
| ------------------------------ | ---------------- |
| zapis za kontrolu pristupa (ACR)    | Zapis povezan s glasnoću na uređaju Microsoft Azure StorSimple koji određuje koje domaćini povezivati s njom. Na određivanja temelji se na iSCSI kvalificirani naziv (IQN) na glavnih računala (sadržani u na ACR) koje se povezujete s uređajem StorSimple.|
| AES 256                        | Algoritam za napredno šifriranje standardne (AES) 256-bitni za šifriranje podataka kao što je pomiče se za i iz oblaka. |
| Veličina jedinice za dodjelu (Istočna Australija)     | Najmanji količinu prostora koji se može dodijeliti za spremanje datoteke u sustava Windows datotečnih sustava. Ako veličina datoteke nije višekratnik od klaster, dodatni prostor mora se koristiti za spremanje datoteke (gore na sljedeći višekratnik broja klaster) rezultira izgubiti prostora i fragmentacije tvrdog diska. <br>Preporučena Istočna Australija za Azure StorSimple jedinicama je 64 KB jer je dobro funkcionira s algoritama Poništavanje duplikacije.|
| Automatsko spremanje tiering      | Automatski premještanju manje aktivni podataka iz SSDs HDDs, a zatim sloju u oblaku i omogućivanje upravljanja sve prostora za pohranu putem središnje korisničkog sučelja.|
| katalog | Zbirka sigurnosnih kopija, obično odnose prema vrsti aplikacije koja se koristila. Zbirku prikazuje se u stranica kataloga sigurnosne kopije StorSimple Upravitelj servisa korisničkog Sučelja.|
| datoteka kataloga sigurnosnog kopiranja             | Datoteka koja sadrži popis dostupnih snimki pohranjena u sigurnosnu kopiju baze podataka programa StorSimple snimku Manager. |
| sigurnosne kopije pravila                   | Odabir količine, Vrsta sigurnosne kopije i vremenski raspored koji omogućuje stvaranje sigurnosne kopije unaprijed definirane rasporedu.|
| velike binarne (BLOB polja)    | Zbirka binarne podatke koji su pohranjeni kao jedan entitet u sustavu za upravljanje bazom podataka. Blob-ova su obično slike, zvuk ili druge multimedijske objekte, iako ponekad binarni izvršni kod pohranjen kao BLOB.|
| Protokol za ispitivanje rukovanja provjere autentičnosti (CHAP) | Protokol koji se koristi za provjeru autentičnosti ravnopravnih članova veze na temelju ravnopravnih članova zajedničko korištenje lozinke ili tajna. CHAP može biti jednosmjerna ili međusobna. S jednosmjerna CHAP cilj potvrđuje u pokretaču. Međusobna CHAP zahtijeva cilj provjeru autentičnosti pokretač i pokretač potvrđuje je cilj. | 
| Kloniraj                          | Kopiju jedinice. |
|Oblak kao sloju (CaaT)          | Prostor za pohranu integriran kao sloju unutar arhitektura za pohranu tako da se sve prostora za pohranu čini se da je dio jedan poslovne mreže za pohranu u oblaku.|
| Oblak usluga (CSP)   | Davatelj oblaka računalstvo services.|
| Oblak snimke                 | Točka u vrijeme kopiju glasnoću podataka koji je pohranjen u oblaku. Oblak snimku jednako snimku replicirati na drugu, službenoj prostora za pohranu. Brze snimke oblaka su osobito je korisna u slučajevima Izrada oporavak.|
| Oblak ključa za šifriranje prostora za pohranu   | Lozinke ili ključ koji koriste StorSimple uređaj da biste pristupili šifriranih podataka koji se šalju putem uređaja s oblakom.|
| Klaster umu ažuriranje         | Upravljanje softverska ažuriranja na poslužiteljima klasteru prebacivanje tako da se ažuriranja ne minimalnog ili ne utječe na dostupnost usluge.|
| DataPath                       | Zbirka funkcijske jedinice izvođenje operacija inter-povezanog obradu podataka.|
| Deaktiviranje                     | Trajno akciju koju prekida veze između StorSimple uređaja i povezane oblaku. Oblak snimke uređaja ostaju nakon ovog postupka, a može biti klonirana ili koristi za oporavak Izrada.|
| Zrcaljenje disk                 | Replikacije količine logičkog diska na zasebne teško da biste bili sigurni neprekinuti dostupnost pogoni u stvarnom vremenu.|
| Zrcaljenje dinamički disk         | Replikacija količine logičkog diska na dinamičkih diskova.|
| dinamičkih diskova                  | Oblik glasnoću disk koji koristi logičke Disk Manager (LDM) za pohranu i upravljanje podacima na više fizičkih diskova. Dinamičkih diskova moguće je povećati omogućuje više slobodnog prostora.|
| Prošireni s prilozima prešla od diskova (EBOD) | Sekundarni s prilozima Microsoft Azure StorSimple uređaja koji sadrži vrlo tvrdi disk diskova za dodatni prostor za pohranu.|
| FAT dodjele resursa               | U običan prostora za pohranu dodjele resursa koji pohrane u prostora dodijeljeno na temelju očekivanu potrebama (i obično se nalazi izvan trenutačno nepotrebno). Vidi također *Tanki dodjele resursa*.|
| tvrdi disk (podizanje tvrdog diska)          | Pogonu koji koristi zakreću više naslaganih pločica površinom radi pohrane podataka.|
| hibridno za pohranu u oblaku           | Prostor za pohranu arhitektura koja koristi lokalni i službenoj resursa, uključujući za pohranu u oblaku.|
| Mali računala sustava (iSCSI Internet Interface) | Temeljeno na internetskog protokola IP pohranu umrežavanje standard za povezivanje podataka za pohranu oprema ili objekti.|
| Pokretač iSCSI                 | Softverska komponenta koja omogućuje glavno računalo sa sustavom Windows da biste se povezali s vanjskim sustavom iSCSI prostora za pohranu mrežom.|
| iSCSI kvalificirani naziv (IQN)      | Jedinstveni naziv koji označava odredište iSCSI ili pokretača.|
| odredište iSCSI                    | Softverska komponenta koja nudi središnje iSCSI podsustava disk u mrežama područja za pohranu.|
| Live arhiviranja                  | Prostor za pohranu pristup koji se u kojem arhiviranje podataka se može pristupiti stalno (nije pohranjena zvuka na vrpci, na primjer). Microsoft Azure StorSimple koristi uživo arhiviranje.|
|lokalno prikvačene glasnoće | jedinica na kojoj se nalazi na uređaju, a nikada tiered u oblak. |
| Lokalni snimku                  | Točka u vrijeme kopiju glasnoću podataka koji je pohranjen na uređaju Microsoft Azure StorSimple.|
| StorSimple Microsoft Azure      | Napredna rješenja sastoji se od podatkovnog centra za pohranu potražite i softver koji omogućuje IT tvrtkama ili ustanovama odražava za pohranu u oblaku kao da je prostor za pohranu podatkovnog centra. Tijekom smanjenju troškova StorSimple olakšava zaštitu podataka i upravljanje podacima. Rješenje objedinjuje primarni prostora za pohranu, arhiviranje, sigurnosno kopiranje i Izrada oporavak (DR) putem objedinjenog integracije s oblaka. Kombiniranjem SAN prostora za pohranu i oblaka upravljanje podacima na platforme poslovne klase StorSimple uređaji omogućiti brzinu, jednostavnosti i pouzdanost za potrebe za sve vezane uz prostora za pohranu.|
| Power i hlađenje modul (uo)  | Hardverske komponente StorSimple uređaja koji se sastoji od power zalihama i na nikakvo Lepeza, dakle naziv Power i hlađenje modul. Primarni s prilozima uređaja ima dvije 764W PCMs dok s prilozima EBOD ima dvije 580W PCMs.|
| Primarni s prilozima               | Da biste s glavne prilozima StorSimple uređaja koji sadrži platformu kontrolera aplikacije.|
| oporavak vrijeme cilj (RTO)   | Najveće vrijeme koje treba expended prije poslovni proces ili sustava potpuno vratit će se nakon na Izrada.| 
|serijski priložene SCSI (SAS)       | Vrsta tvrdom disku (podizanje tvrdog diska).|
| ključ za šifriranje podataka usluge     | Ključ dostupne na novi uređaj StorSimple koja registrira sa servisom StorSimple Manager. Konfiguracija podataka koji se prenose između servisa StorSimple upravitelj i uređaja šifriran pomoću javni ključ, a zatim možete dešifrirati samo na uređaja koji koristi privatni ključ. Ključ za šifriranje podataka servisa omogućuje servisa da biste dobili ovo privatni ključ za dešifriranje.|
| ključa za registraciju        | Ključ koji olakšava registrirajte uređaj StorSimple sa servisom StorSimple Upravitelj tako da se pojavljuje na portalu Azure klasični za daljnje upravljanje akcije.|
| Sučelja sustava Small računala (SCSI) | Skup standarda za fizički povezivanje računala i slanjem podataka.|
| pune stanje pogon (SSD)         | Disk koji sadrži nema premještanje dijelova; na primjer, izbrisivi memorijski pogon.|
| račun za pohranu                 | Postavljanje vjerodajnica za pristup povezani s vašim računom za pohranu za davatelja usluga za dani oblaka.| 
| StorSimple prilagodnik za SharePoint| Komponenta Microsoft Azure StorSimple proziran proširuje zaštitu i prostor za pohranu podataka StorSimple farme poslužitelja sustava SharePoint.|
| Servis upravitelja StorSimple      | Datotečni nastavak Azure klasični portala za koji vam omogućuje upravljanje Azure StorSimple lokalnog i virtualne uređaja.|
| Upravitelj StorSimple snimke     | Microsoft Management Console (BLOG) dodatku za upravljanje sigurnosnog kopiranja i vraćanja operacije u programu Microsoft Azure StorSimple.|
| iskoristite sigurnosne kopije                     | Značajka koja omogućuje korisniku da biste preuzeli interaktivnu sigurnosnu kopiju jedinice. To je svaki drugi način ponijeti ručno sigurnosno kopiranje jedinice umjesto poduzimanja automatske sigurnosnog kopiranja putem definirani pravila.|
| Tanki dodjele resursa               | Način optimizacije učinkovitosti slobodnog prostora koji se koristi u sustavi za pohranu. U Tanki Dodjeljivanje prostora za pohranu dodijeljeno između više korisnika na temelju Minimalna prostora potrebno za svakog korisnika u bilo kojem trenutku. Vidi također *fat dodjele resursa*.|
| tiering | Razmještaj podataka u logička grupiranja na temelju trenutnog korištenja, dobi i odnosa na druge podatke. StorSimple automatski slaže podataka u razine.   |
| jedinica                          | Logička prostora za pohranu područja prikazuju u obliku pogona. StorSimple količine odgovaraju količine postavljen tako da glavno računalo, uključujući one otkrio pomoću iSCSI i StorSimple uređaja.|
 | spremnik za glasnoću                | Grupiranje količine i postavke koje se odnose na njih. Sve jedinice StorSimple uređaja u grupiraju se u spremnika glasnoću. Postavke glasnoće spremnik obuhvaćaju račune za pohranu, postavke šifriranja podataka koji se šalju na cloud s tipkama pridružene šifriranja i propusnosti koju za operacije koje obuhvaćaju oblaka.|
| grupa jedinica                    | U upravitelju StorSimple snimke, grupa jedinica je zbirka količine konfigurirano tako da biste olakšali sigurnosne kopije obrada.|
| Količinsko sjene Copy Service (VSS)| Servis operacijskog sustava Windows Server koji olakšava dosljednost aplikacije po komunikaciju s VSS umu aplikacije koordiniranje stvaranja rastuće snimke. VSS osigurava da su aplikacije privremeno Neaktivni kada uzimaju se snimke.|
| Windows PowerShell za StorSimple | Koji se temelji na sustavu Windows PowerShell – sučeljem naredbenog retka za funkcioniranje i upravljati uređajem StorSimple. Zadržavanje neke od osnovnih mogućnosti komponente Windows PowerShell, ovo sučelje ima dodatne namjenski cmdletima koji se usmjerena prema upravljanje StorSimple uređaja.|


## <a name="next-steps"></a>Daljnji koraci

Saznajte više o [sigurnosti StorSimple](storsimple-security.md).




 

 
