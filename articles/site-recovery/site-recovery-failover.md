<properties
    pageTitle="Prebacivanje u web-mjesta oporavak | Microsoft Azure" 
    description="Oporavak Azure web-mjesta koordinate replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužiteljima. Saznajte više o prebacivanje Azure ili sekundarni podatkovnog centra." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="failover-in-site-recovery"></a>Prebacivanje u oporavak web-mjesta

Oporavak Azure web-mjesta servisa doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužiteljima. Moguće je replicirati strojeva Azure ili sekundarni lokalnog podatkovnog centra. Kratak pregled pročitajte [oporavak web-mjesta Azure?](site-recovery-overview.md)

## <a name="overview"></a>Pregled

U ovom se članku navode informacije i upute za oporavak (ne uspijeva iznad i failing natrag) virtualnim strojevima i fizičke poslužitelje koji su zaštićeni oporavak web-mjesta. 

Pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)objavljuju komentare ni pitanja.


## <a name="types-of-failover"></a>Vrste prebacivanje

Nakon zaštite omogućen za virtualnim strojevima i fizičke poslužitelja, a mogu se replikaciju failovers možete izvesti kao potreba. Oporavak web-mjesta podržava više vrsta prebacivanje.

**Prebacivanje** | **Kada za pokretanje** | **Pojedinosti** | **Postupak**
---|---|---|---
**Prebacivanje test** | Pokrenite provjeru strategije replikacije ili izvršiti analize za oporavak Izrada | Bez gubitka podataka ili isključiti.<br/><br/>Bez utjecaja na replikacije<br/><br/>Bez utjecaja na radnom okruženju | Počnite s pomoćnim<br/><br/>Odredite kako strojeva test će se povezati s mreže nakon prebacivanje<br/><br/>Praćenje napretka na kartici **Zadaci** . Testiranje računala stvaraju i pokrenuti na sekundarnom mjestu<br/><br/>Azure - povezati s računalom na portalu za Azure<br/><br/>Sekundarni web-mjesta – pristup na računalu na glavno računalo i oblaka<br/><br/>Dovršite testiranje i automatski čišćenja testiranje postavki prebacivanje.
**Planirani prebacivanje** | Pokreni da biste zadovoljava preduvjete za usklađenost<br/><br/>Pokrenite planiranog održavanja<br/><br/>Pokretanje uvoza iznad podataka da biste zadržali radnih opterećenja pokrenut za poznati kvarove – kao što su očekivani struje ili izvješća loši vremenske prognoze<br/><br/>Pokretanje da biste failback nakon prebacivanje s primarnog na sekundarnom | Bez gubitka podataka<br/><br/>Nedostupnost nastali je vrijeme potrebno da biste isključili virtualnog računala na primarnih i bi se na sekundarnom mjesto.<br/><br/>Virtualnim strojevima su utjecaj kao cilj strojeva postaje izvor strojeva nakon prebacivanje. | Počnite s pomoćnim<br/><br/>Praćenje napretka na kartici **Zadaci** . Izvor strojeva su isključena<br/><br/>Početak strojeva replike sekundarne mjestu<br/><br/>Azure - povezati s računalom replike na portalu za Azure<br/><br/>Sekundarni web-mjesta – pristup na računalu na istom računalu koje hostira i u istom oblaka<br/><br/>Zapisivanje na prebacivanje
**Prebacivanje neplanirano** | Pokrenuti tu vrstu prebacivanje ponovno aktiviranje način kad postane primarni web-mjesta može pristupiti zbog incident neočekivane, kao što je power napada prekida ili virusa <br/><br/> Možete pokrenuti na neplanirano prebacivanje možete učiniti čak i ako se primarni web-mjesta nije dostupna. | Ovisi o postavkama učestalost ponavljanja gubitka podataka<br/><br/>Podaci će biti ažurni skladu zadnji put je bila sinkronizirana | Počnite s pomoćnim<br/><br/>Praćenje napretka na kartici **Zadaci** . Po želji pokušaja isključivanja virtualnim strojevima i sinkronizacija najnovije podatke<br/><br/>Početak strojeva replike sekundarne mjestu<br/><br/>Azure - povezati s računalom replike na portalu za Azure<br/><br/>Pristup sekundarne web-mjesta na računalu na istom računalu koje hostira i u istom oblaka<br/><br/>Zapisivanje na prebacivanje


Vrste failovers podržani ovise o scenariju implementacije.

**Prebacivanje smjeru** | **Prebacivanje test** | **Planirani prebacivanje** | **Prebacivanje neplanirano**
---|---|---|---
Primarni VMM web-mjesta za sekundarne VMM web-mjesta | Podržana | Podržana | Podržana 
Sekundarni VMM web-mjesta za primarni VMM web-mjesta | Podržana | Podržana | Podržana
Oblak u oblak (jedan VMM server) |  Podržana | Podržana | Podržana
VMM web-mjesta za Azure | Podržana | Podržana | Podržana 
Azure VMM web-mjesto | Nije podržana | Podržana | Nije podržana 
Hyper-V web-mjesta za Azure | Podržana | Podržana | Podržana
Azure Hyper-V web-mjesto | Nije podržana | Podržana | Nije podržana
VMware web-mjesta za Azure | Podržani (poboljšana scenarij)<br/><br/> Nepodržane (naslijeđene scenarij) |Nije podržana | Podržana
Fizička poslužitelj za Azure | Podržani (poboljšana scenarij)<br/><br/> Nepodržane (naslijeđene scenarij) | Nije podržana | Podržana

## <a name="failover-and-failback"></a>Prebacivanje i failback

Uspjeti putem virtualnim strojevima sekundarne lokalnog web-mjesta ili Azure, ovisno o implementaciju sustava. Računalu koje ne uspije putem Azure stvara kao Azure virtualnog računala. Možete se neće putem jednog virtualnog računala ili fizičke poslužitelja ili tarifu za oporavak. Tarife za oporavak sastoji se od jedan ili više naručili grupe koje sadrže zaštićeni virtualnim strojevima ili fizičke poslužiteljima. Koriste da biste orkestrirali prebacivanje od više strojeva koje neće uspjeti putem prema grupi oni pripadaju. [Dodatne informacije potražite u](site-recovery-create-recovery-plans.md) o tarifama za oporavak. 

Nakon dovršetka prebacivanje i na računalima s radom u su sekundarne mjesto Imajte na umu da:

- Ako vam nije uspjelo putem Azure, nakon strojeva prebacivanje nisu zaštićeni ili replicira u primarni ili sekundarni mjesto. U Azure virtualnim strojevima spremaju se u zemlj replicirati prostora za pohranu koji omogućuje otpornost, ali ne replikacije.
- Ako niste neplanirano prebacivanje na sekundarnom web-mjestu, nakon nisu zaštićeni strojeva prebacivanje na sekundarnom mjesto ili replikaciju.
- Ako niste planiranog prebacivanje na sekundarnom web-mjesto, nakon što su zaštićeni strojeva prebacivanje na sekundarnom mjestu.
 

### <a name="failback-from-secondary-site"></a>Failback iz sekundarnog web-mjesta

Failback iz sekundarnog web-mjesta za primarni koristi na isti način kao prebacivanje iz primarnih za sekundarne. Kada je prebacivanje iz primarnog, sekundarnog podatkovnim centrom izvršenja i potpune, pokretanje obrnutim replikacije kad postane dostupan primarni web-mjesta. Obrnuti replikacije pokreće replikacije između sekundarnog web-mjesta i primarni sinkroniziranjem delta podataka. Obrnuti replikacije objedinjuje virtualnim strojevima u zaštićeno stanje, ali sekundarne podatkovnog centra i dalje je aktivna mjesto. Da biste omogućili primarni web-mjesta u aktivni mjesto pokrenete planiranog prebacivanje iz sekundarne za primarni, nakon čega slijedi drugi obrnutim replikacije.

### <a name="failback-from-azure"></a>Failback iz Azure

Ako ste uspio putem za Azure virtualnim strojevima su zaštićeni Azure otpornost značajke za virtualnih računala. Da biste izvornog primarni web-mjesta u aktivni mjesto pokrećete planiranog prebacivanje. Ako izvorno web-mjesto nije dostupno, uspjeti popis natrag na izvorno mjesto ili na drugo mjesto. Da biste pokrenuli replikaciju ponovnog nakon failback na primarno mjesto pokretanje obrnutim replikacije.

### <a name="failover-considerations"></a>Prebacivanje pitanja

- **IP adresa nakon prebacivanje**– prema zadanim se postavkama neuspjelog putem strojno će imati drugu IP adresu od izvornog računala. Ako želite da biste zadržali na istom IP adresu potražite u članku: 
    - **Sekundarni web-mjesta**– ako vam se ne uspijeva putem sekundarnog web-mjesta i želite zadržati je IP adresa [pročitajte](http://blogs.technet.com/b/scvmm/archive/2014/04/04/retaining-ip-address-after-failover-using-hyper-v-recovery-manager.aspx) ovaj članak. Imajte na umu da možete zadržati javnu IP adresu ako to podržava vaš davatelj internetskih usluga.
    - **Azure**– ako vam se ne uspijeva putem Azure možete odrediti IP adresa na koju želite dodijeliti na kartici **Konfiguriraj** svojstva virtualnog računala. Ne možete zadržati javnu IP adresu nakon prebacivanje na Azure. Možete zadržati razmake adresa koji nisu iz programa RFC 1918 koji se koriste kao interne adrese.
- **Djelomično prebacivanje**– ako želite putem dio web-mjesta koje se neće umjesto cijelog web-mjesta Imajte na umu da: 
    - **Sekundarni web-mjesta**– ako uspjeti putem dio primarnog web-mjesta da biste sekundarnog web-mjesta i koje se želite povezati primarni web-mjesta, koristite VPN veza web-mjesto nije uspjelo povezivanje putem aplikacije na web-mjestu sekundarne infrastrukture komponente pokrenuti na primarni web-mjesta. Ne uspijete cijelu podmreže putem možete zadržati IP adresa virtualnog računala. Ako putem djelomične podmreže ne možete zadržati IP adresa virtualnog računala jer podmreže ne može se podijeliti između web-mjesta.
    - **Azure**– ako neće uspjeti putem web-mjesta djelomično Azure, a želite povezati primarni web-mjesta, možete koristiti web-mjesto VPN-a povezivanje neuspjelog putem aplikacije Azure infrastrukture komponente pokrenuti na primarni web-mjesta. Imajte na umu da ako ne uspije cijelu podmreže putem koje možete zadržati IP adresa virtualnog računala. Ako putem djelomične podmreže ne možete zadržati IP adresa virtualnog računala jer podmreže ne može se podijeliti između web-mjesta.
 
- **Slovo**– ako želite zadržati slovo na virtualnim strojevima nakon prebacivanje SAN pravila za virtualnog računala možete postaviti da biste **na**. Diskova virtualnog računala automatski potjecati online. [Dodatne informacije potražite u](https://technet.microsoft.com/library/gg252636.aspx).
- **Zahtjevi za usmjeravanje klijenta**– oporavak web-mjesta radi s Azure promet Upravitelj na zahtjeve za usmjeravanje klijenta u aplikaciji nakon prebacivanje.




## <a name="run-a-test-failover"></a>Pokrenite test prebacivanje

Kada pokrenete test prebacivanje će se zatražiti da biste odabrali mrežne postavke za testiranje replike strojeva. Imate nekoliko mogućnosti.  

**Testiranje mogućnost prebacivanja u slučaju pogreške** | **Opis** | **Prebacivanje potvrdite** | **Pojedinosti**
---|---|---|---
**Nije uspjelo putem Azure – bez mreže** | Nemojte odabrati ciljnu Azure mreže | Prebacivanje provjere testiranje virtualnog računala pokreće očekivani Azure | Sve test virtualnim strojevima u planu za oporavak dodaju se u jednom u oblaku, a možete se povezati s drugom<br/><br/>Strojeva niste povezani s mrežom Azure nakon prebacivanje.<br/><br/>Korisnicima možete se povezati s računala test s javnu IP adresu
**Nije uspjelo putem Azure – s mrežom** | Odaberite cilj Azure mreže | Prebacivanje provjerava test strojeva povezani s mrežom | Stvaranje programa Azure mrežu s kojom je Izolirani u mreži Azure radnog i postavljanje infrastrukture za repliciranu virtualnog računala funkcionirati prema očekivanjima.<br/><br/>Podmreže virtualnog računala test temelji se na podmreži na kojima nije uspio putem virtualnog računala očekuje se povezati ako dođe do planiranog ili neplanirano prebacivanje.
**Nije uspjelo putem sekundarnog VMM web-mjesta – bez mreže** | Nemojte odabrati VM mreže | Prebacivanje provjerava se stvaraju strojeva test.<br/><br/>Na glavnom računalu isti kao glavno računalo replike virtualnog računala postoji stvorit će se test virtualnog računala. Nije se dodaju u oblak u kojoj se nalazi replike virtualnog računala. | <p>Nije uspio putem strojno neće biti povezani s mreže.<br/><br/>Na računalu se mogu povezati s mrežom VM kada je stvorena
**Nije uspjelo putem sekundarnog VMM web-mjesta – s mrežom** | Odaberite postojeću VM mrežu na | Prebacivanje provjerava se stvaraju virtualnim strojevima | Na glavnom računalu isti kao glavno računalo replike virtualnog računala postoji stvorit će se test virtualnog računala. Nije se dodaju u oblak u kojoj se nalazi replike virtualnog računala.<br/><br/>Stvaranje mreže VM koja sadrži Izolirani u mreži radnog<br/><br/>Ako koristite mreže utemeljen na VLAN preporučujemo stvorite zaseban logičke mrežu (ne koriste se u proizvodnje) u VMM u tu svrhu. Logička mreže se koristi za stvaranje VM mreža radi testiranja prebacivanje.<br/><br/>Logička mreže mora biti pridružen barem jedan od mrežnih prilagodnika svih Hyper-V poslužitelja hostira virtualnih računala.<br/><br/>VLAN mreža za logičke mreže web-mjesta dodavanja logičke mrežu mora biti Izolirani.<br/><br/>Ako koristite koji se temelji na Windows mreže virtualizacije – logičke mreže, oporavak Azure web-mjesta automatski stvara Izolirani VM mrežama.
**Nije uspjelo putem sekundarnog VMM web-mjesta – stvaranje mreže** | Mreža privremene test stvorit će se automatski ovisno o postavci koji ste naveli u **Logičke mreže** i i povezane mreže web-mjesta | Prebacivanje provjerava se stvaraju virtualnim strojevima | Tu mogućnost koristite ako tarifu za oporavak koristi više mreža VM. Ako koristite Windows mreže virtualizacije mrežama, tu mogućnost možete automatski stvoriti VM mreža s istim postavkama (podmreže i IP adrese grupe) u mreži replike virtualnog računala. Njima VM očistiti automatski nakon prebacivanje test.</p><p>Na glavnom računalu isti kao glavno računalo replike virtualnog računala postoji stvorit će se test virtualnog računala. Nije se dodaju u oblak u kojoj se nalazi replike virtualnog računala.

>[AZURE.NOTE] IP adresa dodijeljen virtualnog računala tijekom testiranja prebacivanje jednaki IP adresa je kada želite primati radi planiranog ili neplanirano prebacivanje (presuming IP adresa je dostupna u mreži prebacivanje test. Ako se isti IP adresa nije dostupna u mreži prebacivanje test virtualnog računala će primiti drugi IP adresa dostupne u mreži prebacivanje test.



### <a name="run-a-test-failover-from-on-premises-to-azure"></a>Pokrenite test prebacivanje iz lokalnog Azure

Ovaj postupak opisuje kako pokrenuti prebacivanje test za plan za oporavak. Umjesto toga možete pokrenuti prebacivanje na jednom računalu na kartici **virtualnih računala** .

1. Odaberite **Oporavak tarife** > *recoveryplan_name*. Kliknite **Prebacivanje** > **Prebacivanje Test**.
2. Na stranici **Potvrda Test prebacivanje** odredite kako replike strojeva će se povezati s Azure mreže nakon prebacivanje.
3. Ako vam se ne uspijeva putem Azure i šifriranje podataka omogućen za oblak, **Ključa za šifriranje** odaberite certifikat izdan kada je omogućeno šifriranje podataka tijekom instalacije davatelja usluga. 
4. Praćenje napretka prebacivanje na kartici **Zadaci** . Trebali biste moći vidjeti strojno replike test na portalu za Azure.
5. Možete pristupiti računalima replike Azure s lokalno mjesto započeti RDP veza za virtualnog računala. priključak 3389 će moraju biti otvoreni na krajnjoj točki za virtualnog računala.
5. Kada završite, kada na prebacivanje dosegne faza **testiranje dovrši** , kliknite **Dovrši Test** da biste završili.
5. U **bilješkama** snimiti i spremiti sve opažanja povezan s pomoćnim test.
8. Kliknite **test prebacivanje dovršetka** automatski očistiti okruženje za testiranje. Po dovršetku to prebacivanje test prikazat će se C**omplete** status.

> [AZURE.NOTE] Ako testiranje prebacivanje nastavljaju se za više od dva tjedna ćete ga dovršiti prisilno. Elementi ni virtualnim strojevima automatski stvara tijekom prebacivanje test će se izbrisati.
  

### <a name="run-a-test-failover-from-a-primary-on-premises-site-to-a-secondary-on-premises-site"></a>Pokrenite test prebacivanje s web-mjesta primarni lokalnog sekundarne lokalnog web-mjesta

Morat ćete učiniti nekoliko stvari koje možete pokrenuti test prebacivanje, uključujući kopiranje kontroler domene i postavljanje test DHCP i DNS poslužitelji u testnom okruženju. To možete učiniti na nekoliko načina:

- Da biste pokrenuli test prebacivanje pomoću postojeće mreže pripremite servisa Active Directory, DHCP i DNS-a u toj mreži.
- Ako želite da biste pokrenuli test prebacivanje pomoću mogućnosti da biste automatski stvorili VM mrežama, dodajte ručni korak prije grupe 1 u planu oporavak namjeravate koristiti za testiranje prebacivanje, a zatim dodajte resursi infrastrukture automatski stvoreni mrežu prije pokretanja prebacivanje test.

#### <a name="things-to-note"></a>Napomena

- Kada replikaciju sekundarnu web-mjesta, vrsti mreže koristi računalo replike nije potrebno odgovara vrsti logičke mreža koja se koristi za prebacivanje test, ali neke kombinacije možda neće funkcionirati. Ako replike koristi DHCP i sustavom VLAN odvajanja, na mreži VM replike ne mora statične Skupna IP adresa. Da bi se pomoću Windows mreže virtualizacije za prebacivanje test ne funkcioniraju jer nema adresu grupe su dostupne. Uz to testiranje prebacivanje neće funkcionirati ako mreža replike bez odvajanja te je mreža test virtualizacije mreže za Windows. To je zato mreže bez odvajanja nema podmreže potrebne za stvaranje Windows mreže virtualizacije mreže.
- Način na koji replike virtualnim strojevima spojeni mapirani VM mreža nakon prebacivanje ovisi o tome kako se VM mreža bude konfigurirana na konzoli VMM:
    - **VM mreža konfigurirana bez odvajanja ili odvajanja VLAN**– ako DHCP definiranju VM mreže, virtualnog računala replike će se povezati s VLAN ID pomoću postavki koje su navedene za web-mjesto mreže povezane logičke mreže. Virtualnog računala primit će IP adrese s dostupna DHCP poslužitelja. Ne morate u statični Skupna IP adresa definirani cilj VM mreže. Ako se koristi statički Skupna IP adresa za mrežu VM replike virtualnog računala će se povezati s VLAN ID pomoću postavki koje su navedene na web-mjestu mreže povezane logičke mreže. Virtualnog računala će primiti IP adrese iz skup definiran VM mreže. Statički Skupna IP adresa nije definirano na mreži VM cilj, IP adresa dodijeljeni neće uspjeti. Skupna IP adresa moraju se stvoriti na izvorišno i odredišno poslužiteljima VMM koje namjeravate koristiti za zaštitu i oporavak.
    - **Mrežni VM sa sustavom Windows mreže virtualizacije**– ako mreža VM konfiguriran tu postavku statične grupe aplikacija potrebno je definirati za mrežu VM cilj, bez obzira na to je li konfiguriran VM mreže izvora da biste koristili DHCP ili statičke IP adrese grupe aplikacija. Ako definirate DHCP VMM poslužitelju ciljni će poslužiti kao DHCP poslužitelj i navedite IP adresu iz skupa koji je definiran za mrežu VM cilj. Ako je definiran korištenje statične Skupna IP adresa za izvorni poslužitelj, VMM poslužitelju ciljni dodijeliti IP adresu iz na resurse. U oba slučaja IP adresa dodijeljeni neće uspjeti ako statične Skupna IP adresa nije definiran.

#### <a name="run-test"></a>Pokrenite test

Ovaj postupak opisuje kako pokrenuti prebacivanje test za plan za oporavak. Umjesto toga možete pokrenuti prebacivanje za jednu virtualnog računala ili poslužitelj za fizičke na kartici **virtualnim strojevima** .

1. Odaberite **Oporavak tarife** > *recoveryplan_name*. Kliknite **Prebacivanje** > **Prebacivanje Test**.
2. Na stranici **Potvrda testiranje prebacivanje** odredite kako virtualnim strojevima mora biti povezan s mrežama nakon prebacivanje test.
3. Praćenje napretka prebacivanje na kartici **Zadaci** . Kada u prebacivanje dosegne faza** testiranje dovrši** , kliknite **Dovrši testirati** da biste dovršili postupak prebacivanje test.
4. Kliknite **bilješke** da biste snimili i spremili sve opažanja povezan s pomoćnim test.
4. Nakon završetka provjerite je li uspješno pokretanje virtualnih računala.
5. Nakon potvrđivanja uspješno pokretanje virtualnim strojevima, dovršite prebacivanje test za čišćenje Izolirani okruženja. Ako odaberete da biste automatski stvorili VM mrežama, čišćenje briše sve u test virtualnim strojevima i testiranje mreže.

> [AZURE.NOTE] Ako testiranje prebacivanje nastavljaju se za više od dva tjedna ćete ga dovršiti prisilno. Elementi ni virtualnim strojevima automatski stvara tijekom prebacivanje test će se izbrisati.


#### <a name="prepare-dhcp"></a>Priprema DHCP

Ako virtualnim strojevima uključen u prebacivanje Probno korištenje DHCP, DHCP poslužitelj za testiranje treba stvoriti unutar Izolirani mreže koja je stvorena u svrhu prebacivanje test.


### <a name="prepare-active-directory"></a>Priprema servisa Active Directory
Da biste pokrenuli prebacivanje test za aplikaciju testiranje, morate kopiju radnog okruženja servisa Active Directory u testnom okruženju. Idite do [testiranje prebacivanje pitanja vezana uz za active directory](site-recovery-active-directory.md#considerations-for-test-failover) odjeljak dodatne informacije. 


### <a name="prepare-dns"></a>Priprema DNS-a

Priprema DNS poslužitelj za testiranje prebacivanje na sljedeći način:

- **DHCP**– ako virtualnim strojevima koristite DHCP, IP adresu DNS testa trebaju biti ažurirane na poslužitelju DHCP test. Ako koristite vrste mrežnog virtualizacije mreže Windows VMM server djeluje kao DHCP poslužitelj. Zbog toga IP adresu DNS mora se ažurirati u mreži prebacivanje test. U ovom slučaju virtualnim strojevima će registrirati same odgovarajući DNS poslužitelju.
- **Statičke adrese**– ako virtualnim strojevima pomoću statičke IP adrese, trebaju biti u mreži prebacivanje test ažurirane IP adresu DNS poslužitelja za testiranje. Možda ćete morati ažuriranje DNS-a s IP adresom virtualnih računala test. Sljedeći primjer skripte možete koristiti u tu svrhu: 

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-planned-failover-primary-to-secondary"></a>Pokretanje planiranog prebacivanje (primarni da biste sekundarni)

 Ovaj postupak opisuje kako pokrenuti planiranog prebacivanje za plan za oporavak. Umjesto toga možete pokrenuti prebacivanje za jednu virtualnog računala na kartici **virtualnim računalima** .

1. Prije početka provjerite je li sve virtualnim strojevima želite neće uspjeti putem dovršili početne replikacije.
2. Odaberite **Oporavak tarife** > *recoveryplan_name*. Kliknite **Prebacivanje** > **Planirano prebacivanje**. 
3. Na stranici **Potvrda planirano prebacivanje **odaberite izvorišno i odredišno mjesta. Imajte na umu prebacivanje smjer.

    - Ako prethodni failovers radili na očekivani način i poslužiteljima virtualnog računala nalaze se na mjesto izvorišni ili ciljni, smjer pojedinosti prebacivanje bit će se samo informacije. 
    - Ako su virtualnim računalima sustava active na izvorišno i odredišno mjesta, prikazuje se gumb **Promijeni smjer** . Koristite ovaj gumb da biste promijenili i navedite smjer u kojem se događa s pomoćnim.

5. Ako vam se ne uspijeva putem Azure i šifriranje podataka omogućen za oblak, **Ključa za šifriranje** odaberite certifikat izdan kada je omogućeno šifriranje podataka tijekom instalacije davatelja usluge na poslužitelju VMM. 
6. Kada započne planiranog prebacivanje na prvi je korak da biste isključili virtualnih računala da biste bili sigurni bez gubitka podataka. Na kartici **Zadaci** možete pratiti napredak prebacivanje. Ako javlja se pogreška u prebacivanje (ili na virtualnog računala ili skripta uvrštene u planu oporavak), zaustavlja planiranog prebacivanje tarifa za oporavak. Za prebacivanje možete ponovno pokrenuti.
8. Nakon stvaranja replike virtualnim strojevima oni pripadaju potvrdi na čekanju stanje. Kliknite **Potvrdi** za primjenu na prebacivanje. 
9. Po završetku replikacije dovršiti na virtualnim strojevima pokretanja sekundarne lokaciji. 

## <a name="run-an-unplanned-failover"></a>Pokretanje neplanirano prebacivanje

Ovaj postupak opisuje kako pokrenuti neplanirano prebacivanje za plan za oporavak. Umjesto toga možete pokrenuti prebacivanje za jednu virtualnog računala ili poslužitelj za fizičke na kartici **virtualnim strojevima** .

1. Odaberite **Oporavak tarife** > *recoveryplan_name*. Kliknite **Prebacivanje** > **neplanirano prebacivanje**. 
3. Na stranici **Potvrda neplanirano prebacivanje **odaberite izvorišno i odredišno mjesta. Imajte na umu prebacivanje smjer.

    - Ako prethodni failovers radili na očekivani način i poslužiteljima virtualnog računala nalaze se na mjesto izvorišni ili ciljni, smjer pojedinosti prebacivanje bit će se samo informacije. 
    - Ako su virtualnim računalima sustava active na izvorišno i odredišno mjesta, prikazuje se gumb **Promijeni smjer** . Koristite ovaj gumb da biste promijenili i navedite smjer u kojem se događa s pomoćnim.

4. Ako vam se ne uspijeva putem Azure i šifriranje podataka omogućen za oblak, **Ključa za šifriranje** odaberite certifikat izdan kada je omogućeno šifriranje podataka tijekom instalacije davatelja usluge na poslužitelju VMM. 
5. Odaberite **Isključi virtualnim strojevima i sinkronizacija najnovije podatke** da biste odredili da biste isključili zaštićeni virtualnim strojevima i sinkronizirati podatke da bi se najnoviju verziju podataka će se nije uspjela putem trebao oporavak web-mjesta. Ako ne odaberete tu mogućnost, ili pokušaj ne uspije u prebacivanje bit će od točke najnovije dostupne oporavak za virtualnog računala.
6. Na kartici **Zadaci** možete pratiti napredak prebacivanje. Imajte na umu da čak i ako se tijekom neplanirano prebacivanje dođe do pogreške, plan za oporavak izvodi sve dok ne bude dovršena.
7. Nakon prebacivanje, virtualnim strojevima su u stanju za **primjenu na čekanju** . Kliknite **Potvrdi** za primjenu na prebacivanje.
8. Ako postavljate replikacije da biste koristili više točaka oporavak, u točki oporavak promjena možete odabrati da biste koristili točka vraćanja koja nije najnoviji (najnoviji služi po zadanom). Nakon što primijenite dodatne uklonit će se točaka za oporavak.
9. Nakon dovršetka replikacije virtualnim strojevima pokretanja i su pokrenuti na sekundarnom mjestu. No nisu zaštićeni ili replicira. Kada primarni web-mjesta dostupan je ponovno sa iste osnovne infrastrukture, kliknite da biste započeli obrnutim replikacije **Obrnutim replicirati** . Time se osigurava da se svi podaci replicirati natrag u primarni web-mjesta, a da virtualnog računala spreman za prebacivanje ponovno. Poništavanje replikacije nakon neplanirano prebacivanje uključuje se indirektni u prijenos podataka. Prijenos će koristiti isti način na koji je konfiguriran za početne replikacije postavki u oblak.

## <a name="failback-from-secondary-to-primary"></a>Failback iz sekundarne za primarni

 Nakon prebacivanje primarni sekundarne mjesto, repliciranu virtualnim strojevima nisu zaštićeni oporavak web-mjesta i primarni sada ulozi sekundarne mjesto. Slijedite ove postupke uvoza natrag na izvorno primarni web-mjesto. Ovaj postupak opisuje kako pokrenuti planiranog prebacivanje za plan za oporavak. Umjesto toga možete pokrenuti prebacivanje za jednu virtualnog računala na kartici **virtualnim računalima** .

1. Odaberite **Oporavak tarife** > *recoveryplan_name*. Kliknite **Prebacivanje** > **Planirano prebacivanje**.
2. Na stranici **Potvrda planirano prebacivanje **odaberite izvorišno i odredišno mjesta. Imajte na umu prebacivanje smjer. Ako ste radili prebacivanje iz primarnog kao očekujete, a sve virtualnim strojevima su sekundarne mjestu to je samo za informaciju.
3. Ako se ne uspijeva natrag od Azure odaberite postavke **Sinkronizacije podataka**:

    - **Sinkronizacija podataka prije prebacivanje (samo Synchonize delta promjene)**– ta mogućnost minimizira nedostupnost za virtualnim strojevima kao sinkronizira prije nego što isključite ih. Čini sljedeće:
        - Faza 1: Stvara snimku virtualnog računala u Azure i kopira glavno računalo za lokalni Hyper-V. Na računalu i dalje izvodi u Azure.
        - Faza 2: Zatvara virtualnog računala u Azure tako da nema novih promjena pojaviti ima. Konačni skup promjena prenose se na lokalni poslužitelj, a zatim je pokrenuti lokalnog virtualnog računala.
    

    - **Sinkronizacija podataka tijekom prebacivanje samo (Potpuna preuzimanje)**– tu mogućnost koristite ako ste je pokrenut na Azure na dulje vrijeme. Ta je mogućnost brže jer smo očekivali promijenio se najčešće diska, a ne želite trošiti vrijeme u izračunu kontrolni zbroj. Izvodi preuzimanje diska. To je korisno kada je izbrisan lokalno virtualnog računala.
    
    > [AZURE.NOTE] Preporučujemo da koristite ovu mogućnost ako ste je pokrenut Azure neko vrijeme (mjesečno ili više) ili VM lokalno izbrisan. Ta mogućnost ne izračunavati sve kontrolni zbroj.
    
5. Ako vam se ne uspijeva putem Azure i šifriranje podataka omogućen za oblak, **Ključa za šifriranje** odaberite certifikat izdan kada je omogućeno šifriranje podataka tijekom instalacije davatelja usluge na poslužitelju VMM. 
5. Po zadanom se koristi posljednju točku oporavak, ali u **Točka vraćanja promjena** možete odrediti različite oporavak zareza. 
6. Kliknite kvačicu da biste započeli s failback.  Na kartici **Zadaci** možete pratiti napredak prebacivanje. 
7. f odabrana mogućnost da biste sinkronizirali podatke prije prebacivanje, nakon dovršetka sinkronizacije početne podatke i spremni ste za isključivanje virtualnim strojevima u Azure, kliknite **Poslovi**  >  <planned failover job name> **Dovršeno prebacivanja u slučaju pogreške**. To zatvara Azure strojno prenosi najnovije promjene u lokalni virtualnog računala te ga pokreće.
8. Sada možete zapisnika premjestite virtualnog računala da biste provjerili valjanost je dostupna prema očekivanjima. 
9. Virtualnog računala se potvrdi na čekanju stanje. Kliknite **Potvrdi** za primjenu na prebacivanje.
10. Sada kako bi obavili na failback kliknite **Obrnuti replicirati** da biste pokrenuli zaštita virtualnog računala u primarni web-mjesta.



## <a name="failback-to-an-alternate-location"></a>Failback na drugo mjesto

Ako ste implementiran zaštitu između [Hyper-V web-mjesta i Azure](site-recovery-hyper-v-site-to-azure.md) imate mogućnost failback iz Azure za zamjenske na lokalno mjesto. To je korisno ako vam je potrebna da biste postavili novi hardver lokalnog. Evo kako to učiniti.

1. Ako postavljate novi hardver instalirajte Windows Server 2012 R2 i Hyper-V uloga na poslužitelju.
2. Stvaranje skretnice virtualne mreže s istim nazivom koji ste imali na izvornom poslužitelju.
3. Odaberite **Stavke zaštićena** -> **CORBIS**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> želite neće vratiti, a zatim odaberite **Planirano prebacivanje**.
4. **Potvrda planirano prebacivanje** odaberite **Stvori lokalnog virtualnog računala ako ne postoji**. 
5. U **Naziv glavnog računala** odaberite novi poslužitelj Hyper-V glavnog računala na kojem želite smjestiti virtualnog računala.
6. U sinkronizacije podataka preporučujemo da odaberete mogućnost **Sinkroniziraj podatke prije na prebacivanje**. Time se smanjuje nedostupnost za virtualnim strojevima kao sinkronizira prije nego što isključite ih. Čini sljedeće:

    - Faza 1: Stvara snimku virtualnog računala u Azure i kopira glavno računalo za lokalni Hyper-V. Na računalu i dalje izvodi u Azure.
    - Faza 2: Zatvara virtualnog računala u Azure tako da nema novih promjena pojaviti ima. Konačni skup promjena prenose se na lokalni poslužitelj, a zatim je pokrenuti lokalnog virtualnog računala.
    
7. Kliknite kvačicu da biste započeli prebacivanje (failback).
8. Kada završi početne sinkronizacije i spremni ste za isključivanje virtualnog računala u Azure, kliknite **Poslovi** > <planned failover job> > **Dovršeno prebacivanja u slučaju pogreške**. To će se isključuje Azure računalu, prenosi najnovije promjene u lokalni virtualnog računala i ga pokreće.
9. Možete se prijavite na lokalni virtualnog računala da biste provjerili funkcionira sve prema očekivanjima. Kliknite da biste završili s pomoćnim **izvršavanje** .
10. Kliknite **Obrnuti replicirati** da biste pokrenuli zaštita lokalnih virtualnog računala.

    >[AZURE.NOTE] Ako odustanete posao failback dok je u koraku sinkronizacije podataka, VM lokalnog će se u stanju su oštećeni. To je zato kopije podataka odvija najnovije podatke iz Azure VM diskova za podataka diskova lokalno i untill sinkronizacija dovrši, disk podataka možda neće biti u dosljedan stanju. Ako je uključeno prem VM nije pokrenuto kada je otkazan odvija podataka, je možda ne pokretanje. Ponovno pokretanje prebacivanje da biste dovršili odvija podataka.
 
