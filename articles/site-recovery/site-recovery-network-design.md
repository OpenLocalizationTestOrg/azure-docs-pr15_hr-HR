<properties
    pageTitle="Dizajniranje mrežne infrastrukture za oporavak Izrada | Microsoft Azure"
    description="U ovom se članku govori o mreži dizajn zahtjevi za oporavak Azure web-mjesta"
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="pratshar"/>

#  <a name="designing-your-network-infrastructure-for-disaster-recovery"></a>Dizajniranje mrežne infrastrukture za oporavak Izrada

U ovom se članku je preusmjereni na Informatičari koji su odgovorni za architecting, implementacijom, i podršku poslovanje i Izrada infrastruktura za oporavak (BCDR) i koji žele iskoristiti prednost Microsoft oporavak Azure web-mjesta (ASR) da biste podržavaju i poboljšati svoje BCDR usluge. U ovom papira govori o praktično zahtjevi za implementaciju poslužitelja upravitelja virtualnog računala centar sustava, argumente za i protiv od prošireni podmreže nasuprot podmreže prebacivanje te kako strukturirati Izrada oporavak virtualne web-mjesta u programu Microsoft Azure.

## <a name="overview"></a>Pregled

[Oporavak Azure web-mjesta (ASR)](https://azure.microsoft.com/services/site-recovery/) je servis Microsoft Azure orchestrates zaštitu i oporavak virtualiziranom aplikacija za tvrtke continuity Izrada oporavak (BCDR). Ovaj dokument je namijenjen vodič čitač kroz postupak dizajniranja mrežama, usredotočite se na architecting IP rasponi i podmreže oporavak mjestu Izrada kada replikaciju virtualnim strojevima (VMs) pomoću oporavak web-mjesta.

Osim toga, u ovom se članku objašnjava kako oporavak web-mjesta omogućuje architecting i implementaciju dodjeli resursa virtualne podatkovnog centra za BCDR usluge podrške u trenutku test ili Izrada.

U svijetu gdje svi očekuje povezivanje 24-7, nije važno nego ikad održavanje infrastrukturu i aplikacije i pokrenut. Svrha poslovanje i Izrada oporavak (BCDR) je vratiti nije uspjelo komponente tako tvrtke ili ustanove brzo možete nastaviti normalno operacije. Razvoja Strategije oporavka Izrada baviti vjerojatno, devastating događaja je vrlo zahtjevne. To je zbog postojećih poteškoće s procjenjivanje u budućnosti osobito kao što je to povezano s improbable događaja i visoke trošak možete unijeti odgovarajuće mjere zaštite od far-reaching catastrophes. 

Presudne za BCDR planiranje oporavak vrijeme cilj (RTO) i oporavak točke cilj (RPO) potrebno je definirati kao dio plan za oporavak Izrada. Kada na Izrada teži kupca podatkovnog centra, pomoću Azure oporavak web-mjesta, korisnici mogu brzo (malo RTO) putem Interneta premjestiti njihov repliciranu virtualnim strojevima nalazi se u Centar za sekundarne podatkovne ili Microsoft Azure bez gubitka podataka minimalne (malo RPO). 

Prebacivanje je načinio moguće ASR koji su prethodno kopira određenu virtualnim strojevima iz centra za primarni podataka u Centar za sekundarne podatkovne ili Azure (ovisno o scenariju) te povremeno osvježava u replike. Prilikom planiranja infrastrukture, dizajn mreže trebalo bi smatrati potencijalne usko grlo zbog kojih ste iz tvrtke sastanka RTO i RPO ciljeva.  

Prilikom planiranja su administratori implementacija rješenja za oporavak Izrada, jedna od ključne pitanja u svoje mladenačkom je kako virtualnog računala bila dostupna nakon dovršetka na prebacivanje. ASR omogućuje administrator da biste odabrali mrežu na kojoj virtualnog računala će biti povezani s nakon prebacivanje. Ako je primarni web-mjesta upravlja VMM poslužitelja, a zatim to je postići pomoću mapiranja mreže. Dodatne informacije potražite u odjeljku [Priprema za preslikavanje](site-recovery-network-mapping.md) .

Prilikom dizajniranja mreža za oporavak web-mjesta, administrator ima dvije mogućnosti:

- Koristite neki drugi raspon IP adresa za mrežu na web-mjestu za oporavak. U ovom scenariju virtualnog računala nakon prebacivanje će se nova IP adresa i administrator bi potrebno ažuriranje DNS-a. Pročitajte više o tome kako se DNS-om ažuriranje [ovdje](site-recovery-vmm-to-vmm.md#test-your-deployment) 
- Koristite isti rasponu IP adresa za mrežu na web-mjestu za oporavak. U određenim slučajevima administratori radije da biste zadržali IP adresa koji imaju na web-mjestu primarni čak i nakon na prebacivanje. U normalnom scenariju administrator bit će potrebno ažuriranje usmjerava da biste naznačili na novo mjesto IP adresa. No u scenariju gdje je implementiran prošireni VLAN između primarnih i vraćanje web-mjesta, zadržavanje IP adresa za virtualnim strojevima postaje privlačne mogućnost. Zadržavanje isti IP adrese pojednostavljuje postupka oporavka prihvaćanjem Odsutan mreže povezane objavu prebacivanje korake.


Prilikom planiranja su administratori implementacija rješenja za oporavak Izrada, jedna od ključne pitanja u svoje mišljenje je kako aplikacija će biti dostupno nakon dovršetka na prebacivanje. Moderna aplikacije ovise o gotovo uvijek mreže mogu pridonijeti donekle fizički pa premještanje servisa s jednog web-mjesta na drugo predstavlja umrežavanje test. Postoje dva glavna načina koji je problem riješiti za Izrada oporavak rješenja. Prvi pristup je da biste zadržali fiksnim IP adrese. Bez obzira servisima premještanje i hostinga poslužitelja koji se na drugim lokacijama fizičke, aplikacija potrebno konfiguraciju IP adrese s njima na novo mjesto. Drugi način uključuje potpuno promjena IP adresu prilikom prelaska u oporavljenim web-mjesta. Svaki pristup ima nekoliko varijacije implementacije koje su navedene u nastavku.

Prilikom dizajniranja mreža za oporavak web-mjesta, administrator ima dvije mogućnosti:

## <a name="option-1-retain-ip-addresses"></a>Mogućnost 1: Zadržavanje IP adresa 

Iz perspektive Izrada oporavak procesa, pomoću fiksnim IP adrese čini se da je najjednostavniji način za implementaciju, ali ima broj potencijalne izazove koji su u praksi je barem popularne pristup. Oporavak Azure web-mjesta pruža mogućnost da biste zadržali IP adresa u svim situacijama. Prije nego što nešto odluči da biste zadržali IP, odgovarajući misli treba dodijeljen ograničenja nameće na mogućnosti prebacivanje. Javite nam pogledajte čimbenici koji omogućuju donošenje odluka da biste zadržali IP adresa ili ne. To se može postići na dva načina pomoću prošireni podmreže ili tako da učinite cijelog podmreže prebacivanje.

### <a name="stretched-subnet"></a>Prošireni podmreže

Ovdje podmreži postane dostupna u primarni i DR mjesta istovremeno. Jednostavno rečeno tomu poslužitelj i njegov konfiguraciju IP (Layer 3) možete premjestiti na drugo mjesto, a zatim Mreža će na promet usmjerili na novo mjesto automatski. Ovo je trivial baviti iz perspektive server, ali ima broj izazove:

- Iz perspektive sloja 2 (podatkovne veze layer), potrebno umrežavanje opremu koju možete upravljati prošireni VLAN, ali to je postao manji od problema kao što je sada široko dostupan. Drugi i teže problem koji je tako da razmaknete VLAN potencijalne domene kvara proširen je na oba web-mjesta, zapravo postanete jednu točku nije uspjelo. Dok je vjerojatno pojavljivanje, sadrži dogodilo emitiranje oluja pokrene, ali nije moguće Izolirani. Smo pronašli ste se koriste različite verzije mišljenje o problemu posljednje i kao i "smo će nikad implementirati te tehnologije" vidjeti mnoge uspješno implementacije.
- Prošireni podmreže nije moguće ako koristite Microsoft Azure s DR web-mjesta.


### <a name="subnet-failover"></a>Prebacivanje podmreže

Moguće je implementirati podmreže prebacivanje da biste dobili prednosti rješenje prošireni podmreže prethodno opisan bez razmaknete podmreži na više web-mjesta. Ovdje sve navedene podmreže bi na web-mjestu 1 ili 2 web-mjesta, ali ne na oba web-mjesta istodobno. Da biste zadržali prostor IP adrese slučaju na prebacivanje, moguće je programski Rasporedi za infrastrukture usmjerivača da biste premjestili na podmreže iz jednog web-mjesta na drugo. U scenariju prebacivanje na podmreže bi premjestit pridruženog zaštićen VMs. Glavni drawback tog pristupa je u slučaju pogreške ćete morati premjestiti cijelu podmreži, koja može biti u redu, ali to može utjecati na pitanja vezana uz preciznosti prebacivanje. 

Provjerimo po čemu se moći replicirati njegov VMs mjesto oporavak tijekom neuspjeh preko cijele podmreže fictional enterprise pod nazivom Contoso. Ne možemo najprije izgledat će pri čemu se moći upravljati svojim podmreže dok replikaciju VMs između dvaju lokalnog mjesta, a zatim ćemo obrađuje podmreže prebacivanje funkcioniranje kada Contoso [Azure se koristi kao Izrada oporavak web-mjesta](#failover-to-azure).

#### <a name="failover-to-a-secondary-on-premises-site"></a>Prebacivanje na sekundarnom lokalnog web-mjesto

Javite nam pogledajte scenarij gdje želimo zadržati IP svakog na VMs i nije uspjelo pokazivač dovršeno podmreže zajedno. Primarni web-mjesta sadrži aplikacije koje rade u 192.168.1.0/24 podmreže. Kada se dogodi u prebacivanje, sve virtualnim strojevima koje su dio ovog podmreže će se nije uspjela tijekom oporavak web-mjesta i zadržati njihove IP adrese. Usmjerava morat će se pravilno izmijeniti tako da odražava činjenica da sve virtualnim strojevima pripadaju 192.168.1.0/24 podmreže sada prijeđete oporavak web-mjesta. 

Na sljedećoj slici usmjerava između primarni web-mjesta i oporavak web-mjesta, treći web-mjesta i primarni web-mjesta i treći web-mjesta i web-mjesta za oporavak morat će se pravilno mijenjati. 

Sljedeća slika prikazuje podmreže prije na prebacivanje. 192.168.0.1/24 podmreže aktivan primarni web-mjesta prije za prebacivanje, a postaje aktivan oporavak web-mjesta nakon za prebacivanje 

![Prije prebacivanja u slučaju pogreške](./media/site-recovery-network-design/network-design2.png)

Prije prebacivanja u slučaju pogreške


Na slici u nastavku prikazuje mreže i podmreže nakon prebacivanje.
    
![Nakon prebacivanje](./media/site-recovery-network-design/network-design3.png)

Nakon prebacivanje

Sekundarni web-mjestu je lokalno i koristite VMM poslužitelju za upravljanje ga, a zatim prilikom omogućivanja zaštiti za određene virtualnog računala ASR dodijeliti mrežni resursi prema sljedećim tijeka rada:

- ASR dodjeljuje IP adresa za svaki mrežno sučelje na virtualnog računala s statične Skupna IP adresa definiranih odgovarajući mreže za svaku instancu VMM centar sustava.
- Ako administrator određuje isti Skupna IP adresa za mrežu na web-mjestu oporavak kao Skupna IP adresa mreže na primarni web-mjesta prilikom Dodjela IP adresa za virtualnog računala replike ASR želite dodijeliti istu IP adresu kao primarni virtualnog računala.  Na IP je rezervirana u VMM, ali nije postavljena kao IP prebacivanje. Prebacivanje IP postavljen neposredno prije na prebacivanje.

![Zadržavanje IP adresa](./media/site-recovery-network-design/network-design4.png)
    
Slika 5

5 slika prikazuje prebacivanje TCP/IP postavki virtualnog računala replike (konzole Hyper-V). Te postavke će biti ispunjena prije virtualnog računala pokrene nakon na prebacivanje

Ako se isti IP nije dostupna, ASR želite dodijeliti neke druge dostupna IP adresa iz definirani Skupna IP adresa. 

Nakon na VM omogućena za zaštitu možete koristiti sljedeći primjer skripte da biste provjerili IP koji će se dodijeliti virtualnog računala. Isti IP želite postaviti kao prebacivanje IP i dodijeljene u VM trenutku prebacivanje:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

>[AZURE.NOTE] Scenarij u kojemu virtualnim strojevima koristiti DHCP upravljanje IP adrese je potpuno izvan kontrole ASR. Administrator je da biste bili sigurni da DHCP poslužitelj posluživanje IP adrese na web-mjestu oporavak vam može poslužiti iz na isti raspon kao primarni web-mjesta.

#### <a name="failover-to-azure"></a>Prebacivanje na Azure

Oporavak Azure web-mjesta (ASR) omogućuje Microsoft Azure će se koristiti kao Izrada oporavak web-mjesto za virtualnih računala. U ovom slučaju morate baviti nešto više ograničenja. 

Provjerimo scenarij u kojem fictional tvrtka pod nazivom Woodgrove banke ima lokalnog infrastruktura za hostiranje njihove crte poslovnih aplikacija, a oni hostira svojim mobilnim aplikacijama na Azure. Povezivanje između Woodgrove banke VMs na poslužiteljima Azure i lokalnih navedeni su tako da na web-mjesta web-na-mjesta (S2S) virtualne privatne mreže (VPN). S2S VPN omogućuje Woodgrove banke virtualne mreže u Azure da biste se prikazivati kao datotečni nastavak Woodgrove banke lokalne mreže. U ovom komunikacije omogućena je prema S2S VPN između bankovni Woodgrove rub i Azure virtualne mreže. Sada Woodgrove želi koristiti ASR za replikaciju njegov radnih opterećenja izvodi u njegov podatkovnog centra za Azure. Tu mogućnost odgovara potrebama Woodgrove koji želi najekonomičniji mogućnost DR te je moći spremiti podatke u okruženju javno oblaka. Woodgrove ima baviti aplikacije i konfiguracija koje ovise o programiranih IP adrese, dakle imaju zahtjev da biste zadržali IP adresa za svoje aplikacije nakon što putem za Azure.

Woodgrove odlučio da biste dodijelili IP adrese rasponu IP adresa (172.16.1.0/24, 172.16.2.0/24) njegovih resursa koji se izvodi u Azure.

Za Woodgrove moći replicirati virtualnim računalima sustava za Azure, ali zadržava IP adrese Azure virtualne mreže treba stvoriti. Datotečni nastavak lokalne mreže mora biti tako da se aplikacije možete jednostavno prebacivanje iz lokalnog web-mjesta za Azure. Azure omogućuje dodavanje povezivanje VPN-a web-mjesto, kao i zareza web virtualne mrežama koje su stvorene u Azure. Kada postavljate vezu s web-mjesto, Azure mreže omogućuje vam da biste usmjerili promet na lokalne lokacije (Azure poziva je lokalnoj mreži) samo ako rasponu IP adresa razlikuje se od rasponu lokalnog IP adresa, jer Azure ne podržava razmaknete podmreže.  To znači da imate podmreže 192.168.1.0/24 lokalnog, ne možete dodati lokalnoj mreži 192.168.1.0/24 Azure mreže. Time se očekuje jer Azure ne zna da postoje nema aktivne VMs u podmreži i koji je stvoren podmreži samo za potrebe DR. Da biste mogli pravilno mrežni promet usmjerili iz Azure mreže podmreže u mreži i lokalnoj mreži mora nisu u sukobu. 

![Prije nego što prebacivanje podmreže](./media/site-recovery-network-design/network-design7.png)

Prije prebacivanja u slučaju pogreške

Da biste lakše Woodgrove ispunjavanje preduvjeti svoje tvrtke, potrebna je implementirati tijekovima rada za sljedeće:

- Stvaranje dodatnih mreže, Javite nam nazovite ga oporavak mreži, gdje virtualnim strojevima nije uspjela nad stvorit će se.
- Da biste bili sigurni da IP za na VM zadržava se nakon na prebacivanje, idite na karticu konfiguracija u odjeljku Svojstva VM navedite isti IP ima li na VM lokalnog, a zatim kliknite Spremi. Kada u VM nije uspio iznad, oporavak web-mjesta Azure će dodijeliti navedeni IP virtualnog računala. 

![Svojstva mreže](./media/site-recovery-network-design/network-design8.png)

Kada se pokrene u prebacivanje, a virtualnih računala stvaraju se u oporavak mreža uz željene IP, povezivanje s mrežom možete uspostaviti pomoću [Vnet Vnet vezu](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Ako je potrebno ovu akciju mogu biti postavljanje upita skriptiranih.  Kao što smo opisan u prethodnom odjeljku o podmreže prebacivanje, čak i ako prebacivanje na Azure, usmjerava bit će potrebno je komponenta izmijeniti tako da odražava tu 192.168.1.0/24 sada se premješta u Azure. 

![Nakon prebacivanje podmreže](./media/site-recovery-network-design/network-design9.png)

Nakon prebacivanje

Ako još nemate 'Azure mreže' kao što je prikazano na gornjoj slici. Možete stvoriti web-mjesta za web-mjesta vpn vezu između "Primarni web-mjesta" i 'Oporavak mreže' nakon za prebacivanje.  


## <a name="option-2-changing-ip-addresses"></a>Mogućnost 2: Promjena IP adrese

Takvog čini se da se najčešće sustavi koji se temelje na što ste možemo vidjeti. To se sastoji od promjena IP adrese svaki VM koji je uključen u za prebacivanje. Drawback od takvog zahtijeva dolazne mreže "informacije" sada je aplikaciju koja je pri nakon pri IPy. Čak i ako se nakon i IPy su logički imena, DNS stavke obično moraju biti ili isprazniti cijeloj na mreži i predmemorirane stavke u tablicama mreže morate ažurirati ili isprazniti, stoga na nedostupnost može vidjeti ovisno o tome kako infrastrukture DNS je instaliran. Te probleme može biti mitigated pomoću niske vrijednosti TTL slučaju intranetu aplikacije i pomoću [Upravitelj promet Azure s ASR](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) za internetske temelji aplikacije

### <a name="changing-the-ip-addresses---illustration"></a>Promjena IP adrese - slika

Javite nam pogledajte scenarij koju namjeravate koristiti različite IP-ovi preko primarnih osnovnih stranica i web-mjesta za oporavak. U sljedećem primjeru smo također imaju nekog drugog web-mjesta iz gdje će se aplikacija hostirane na primarnog ili oporavak može pristupiti web-mjesta.

![Drugi IP - prije prebacivanja u slučaju pogreške](./media/site-recovery-network-design/network-design10.png)

Slika 11

Na slici 11 postoje neke aplikacije smješten u podmreže 192.168.1.0/24 podmreže primarni web-mjesta, a oni konfiguriran za pojavljuju na web-mjestu oporavak u 172.16.1.0/24 podmreže nakon na prebacivanje. VPN veza/mrežni putovi komponenta ste nije konfiguriran tako da sva tri web-mjesta mogu pristupiti međusobno povezani.
 
Kao što je slika 12 prikazuje nakon što iznad neke programe, oni vratit će se u podmreže oporavak. U ovom slučaju ne možemo su nije ograničeno na prebacivanje cijelu podmreže u isto vrijeme. Bez promjena moraju konfigurirajte VPN-a ili mrežni putovi. Na prebacivanje i neka ažuriranja DNS će pripazite da ostanu dostupna aplikacija. Ako se DNS-om nije konfigurirano tako da omogućuje dinamička ažuriranja pa virtualnim strojevima će registrirati same pomoću novog IP kada počnu nakon s pomoćnim. 

![Drugi IP - nakon prebacivanje](./media/site-recovery-network-design/network-design11.png)

Slika 12

Nakon neuspjeh postavite pokazivač virtualnog računala replike možda IP adresu koja nije isto što i IP adrese primarni virtualnog računala. Virtualnim strojevima će se ažurirati DNS poslužitelju na kojem se koriste kada počnu. DNS stavke obično moraju biti ili isprazniti cijeloj mreži, a predmemorirane stavke u tablicama mreže morate ažurirati ili isprazniti, tako da ih se ne može se nađu nedostupnost tijekom promjene stanja odvija. Taj se problem može biti mitigated tako da:

- Korištenje niske vrijednosti TTL za intranetsku aplikacije.
- Pomoću ASR Azure promet Upravitelj za internet temelji aplikacije.
- Pomoću sljedeće skripte unutar plan oporavak o ažuriranju DNS poslužitelja da biste bili sigurni pravovremeno ažuriranja (skripta nije potreban ako je konfiguriran za registraciju za dinamičku DNS-a)

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


### <a name="changing-the-ip-addresses--dr-to-azure"></a>Promjena IP adresa – DR za Azure

[Umrežavanje infrastrukture instalacijski program za Microsoft Azure kao Izrada oporavak web-mjesto](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) bloga objašnjava Kada zadržavanje IP adresa nije preduvjet za postavljanje obavezno Azure mrežne infrastrukture. Ga započinje s opisom aplikacije, a zatim pogledajte kako postaviti umrežavanje lokalnog i na Azure, a zatim concluding s kako obavljati test prebacivanje i planiranog prebacivanje.



## <a name="next-steps"></a>Daljnji koraci

[Saznajte](site-recovery-network-mapping.md) kako koristi oporavak web-mjesta karte izvorišno i odredišno mreža kada VMM poslužitelj za upravljanje primarni web-mjesta. 
