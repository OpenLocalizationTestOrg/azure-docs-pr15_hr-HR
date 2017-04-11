<properties
   pageTitle="Referenca Azure arhitektura - IaaS: Implementacijom DMZ između Azure i na Internetu | Microsoft Azure"
   description="Kako implementirati arhitekturu sigurne hibridnog mreže s Internetom u Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-the-internet"></a>Implementacijom DMZ između Azure i Interneta

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

U ovom se članku opisuje najbolje prakse za implementacije sigurno hibridnog mreže koji se proteže lokalne mreže koju prihvaća promet s mreže internet Azure. Ovu referenca arhitektura proširuje arhitektura opisane u članku [DMZ između Azure i podatkovnim centrom vaše lokalne implementacije][implementing-a-secure-hybrid-network-architecture]. Preporučuje se čitanje dokumenta i razumijevanje referenci tog arhitektura prije ovog dokumenta za čitanje.

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Resursima] [ resource-manager-overview] i classic. Ovu referenca arhitektura koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije. 

Tipična Upotreba slučajeva za tu arhitekturu obuhvaćaju sljedeće:

- Hibridno aplikacije mjesto cilja radnih opterećenja djelomično lokalnog i djelomično u Azure.

- Azure infrastrukture koji usmjerava promet s lokalnog poslužitelja i s Internetom.

## <a name="architecture-diagram"></a>Arhitektura dijagrama

Na sljedećem su dijagramu ističu važne komponente u toj arhitekturi:

> Visio dokument koji sadrži ovaj dijagram arhitektura dostupna je za preuzimanje na [Microsoftov centar za preuzimanje][visio-download]. Ovaj dijagram nalazi se na stranici "DMZ – javno".

[! [0]][0]

- **Javnu IP adresu (TOČAKA).** To je IP adresa javnog krajnje točke. Vanjski korisnici koji su povezani s Internetom možete pristupiti sustavu kroz tu adresu.

- **Mrežni virtualne potražite (NVA).**  NVA je generički izraz koji opisuje VM obavljaju zadatke kao što je dopuštanje ili odbijanje pristupa kao vatrozid, optimiziranje (uključujući spajanje mreža) je WAN operacije, prilagođene usmjeravanje ili druge funkcije mreže.

- **Azure opterećenja.** Svi zahtjevi za razgovore putem Interneta proći kroz ovaj opterećenja i su distribuirati da bi NVAs u javnim DMZ ulaznog podmreže.

- **Javni DMZ dolazni podmreže.** U ovom podmreže prihvaća zahtjeve iz Azure opterećenja. Zahtjevi za prosljeđuju se na neki od NVAs u na DMZ.

- **Javni podmreže izlaznog DMZ.** Zahtjevi za koje su odobrene tako da na NVA prenesite kroz ovaj podmreže Interna raspoređivača opterećenja za web razina.

## <a name="recommendations"></a>Preporuke

Azure nudi mnogo različitih resurse i vrste resursa, tako da ovu referenca arhitekture može dodjeli brojne načine. Ne možemo je navesti predložak Voditelj resursa Azure da biste instalirali arhitektura reference koja slijedi te preporuke. Ako se odlučite za stvaranje vlastite arhitektura referenca slijedite ove preporuke u osim ako imate određene zahtjeva koji ne stane i preporuke.

### <a name="nva-recommendations"></a>Preporuke za NVA

Implementacija jedan skup NVAs za promet potječu na Internetu i drugi za promet potječu lokalnog. To je sigurnosni rizik koristiti samo jedan skup NVAs za oba jer ovaj dizajn nudi bez opseg sigurnost između dva skupa mrežni promet. Je prednosti korištenja ovaj dizajn jer smanjuje složenost sigurnosna pravila za provjeru i olakšava jasniji prikaz pravila koja odgovaraju svaki dolazni zahtjev mreže. Na primjer, jedan skup NVAs implementira pravila za internetski promet samo dok drugi skup pravila implementacija NVAs samo promet na lokalni.

Uključite sloj 7 NVA za prekid veze aplikacije na razini NVA i održavanje afinitet s pozadinskom razine. Time će se jamčiti simetričnu povezivanje u kojoj promet odgovor s pozadinskom razine vraća kroz na NVA.  

### <a name="public-load-balancer-recommendations"></a>Preporuke za raspoređivača opterećenja javno ###

Da biste zadržali skalabilnost i dostupnost, implementacije javne DMZ dolazni NVAs u programa [Postavljanje dostupnosti] [ availability-set] i koristiti na [Internetu nasuprotne opterećenja] [ load-balancer] da biste zahtjevi raspodijelite NVAs u skupu dostupnost.  

Konfiguriranje opterećenja za prihvaćanje zahtjeva samo na priključke koje su potrebne za internetski promet. Na primjer, ograničiti ulaznog HTTP zahtjevima za priključak 80 i ulaznog HTTPS zahtjevi za priključak 443.

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

Dizajnirajte svoje infrastrukture pomoću internetsku nasuprotne opterećenja ispred ulaznog javno DMZ podmreže iz na outset. Čak i ako se vaš initally arhitektura zahtijeva jedan NVA, bit će lakše za promjenu veličine da biste više NVAs u budućnosti ako internet nasuprotne opterećenja već implementiran.

## <a name="availability-considerations"></a>Razmatranja dostupnosti

Internet nasuprotne opterećenja zahtijeva svaki NVA u javnim DMZ ulaznog podmreže implementaciju [probni stanja][lb-probe]. Stanje probni koji ne reagira na ovom krajnju točku smatra se da nije dostupan, a raspoređivača opterećenja će Izravni zahtjevi za druge NVAs unutar istog dostupnosti skupa. Imajte na umu da sve NVAs nije uspjelo odgovoriti, aplikacija neće uspjeti stoga je važno da bi nadzor konfiguriran za upozorenja DevOps kada broj instanci dobar NVA pada ispod praga.

## <a name="manageability-considerations"></a>Pitanja vezana uz mogućnost upravljanja

Ograničiti funkcionalnost nadzor i upravljanje za na dolazni javno DMZ NVA na odgovora na zahtjeve iz okvira Skok u samo podmreži upravljanje. Kako je opisano u [DMZ između Azure i podatkovnim centrom vaše lokalne implementacije] [ implementing-a-secure-hybrid-network-architecture] dokumenata, definiranje jedne mreže usmjeravanje iz lokalne mreže putem pristupnika skočni okvir u podmreže upravljanje ograničavanja pristupa.

Ako je pristupnika iz lokalne mreže veza sa sustavom Azure prema dolje, i dalje možete dobiti skočni okvir uvođenjem TOČAKA, dodavanjem skočni okvir, a remoting u s Interneta.

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

Ovu referenca arhitektura implementira više razine sigurnosti:

- Internet nasuprotne opterećenja usmjerava zahtjevi za na NVAs u ulaznog javno DMZ podmreži samo i samo na priključke koje su potrebne za aplikaciju.

- NSG pravila za ulazni i izlazni javno DMZ podmreži onemogućuju se ugrožena tako da blokira zahtjeva za koje se nalaze izvan pravila NSG na NVAs.

- Konfiguracija usmjeravanje NAT za na NVAs usmjerava zahtjevi za razgovore na priključak 80 i 443 za web Razina opterećenja, ali zanemaruje zahtjeve na druge priključke.

Imajte na umu zapisujte svi zahtjevi za razgovore na sve priključke. Redovito nadzor zapisnike plaćanja pažnju na zahtjeve za koje se nalaze izvan očekivani parametara kao što je to može značiti pokušaja podataka.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Korištenje NSGs za blokiranje/računanja promet između aplikacija razine

Svaki od razine web, tvrtke i podataka ograničiti promet između njih pomoću NSGs. To jest, sloju tvrtke koristi se NSG da biste blokirali sve promet koja ne potiče u sloju web i razina podataka koristi se NSG da biste blokirali sve promet koja ne potiče u sloju tvrtke. Ako imate zahtjev da biste proširili pravila NSG dopustili šira pristup te razine, obzir uzeli i te preduvjete protiv sigurnosni rizik. Svaki novi ulaznog pathway predstavlja priliku oštećenja ili aplikacija oštećenja slučajne ili purposeful podataka.

## <a name="solution-deployment"></a>Uvođenje rješenja

Uvođenje za referencu arhitektura koji implementira te preporuke i dalje dostupna je na Github. Arhitektura ovu referenca sadrži virtualne mreže (VNet), mreže sigurnosne grupe (NSG), opterećenja i dva virtualnim strojevima (VMs).

Arhitektura referenca može uvesti pomoću sustava Windows ili Linux VMs slijedeći upute u nastavku: 

1. Desnom tipkom miša kliknite donji gumb i odaberite neki "Otvori vezu na novoj kartici" ili "Otvori vezu u novom prozoru":  
[![Implementacija Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2FvirtualNetwork.azuredeploy.json)

2. Kada na portalu za Azure otvorio vezu, morate unijeti vrijednosti za neke postavke: 
    - Naziv **grupe resursa** već definiran u datoteci parametar, pa odaberite **Stvori novo** i unesite `ra-public-dmz-network-rg` u tekstni okvir.
    - **Mjesto** padajućem okviru odaberite regiju.
    - Uređivati **Predložak korijenski Uri** ili tekstne okvire **Parametar korijenski Uri** .
    - Odaberite **Vrstu Os** na padajućem okviru, **windows** ili **linux**.
    - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
    - Kliknite gumb **za kupnju** .

3. Pričekajte implementaciju da biste dovršili.

4. Desnom tipkom miša kliknite donji gumb i odaberite neki "Otvori vezu na novoj kartici" ili "Otvori vezu u novom prozoru":  
[![Implementacija Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fworkload.azuredeploy.json)

5. Kada na portalu za Azure otvorio vezu, morate unijeti vrijednosti za neke postavke: 
    - Naziv **grupe resursa** već definiran u datoteci parametar, pa odaberite **Stvori novo** i unesite `ra-public-dmz-wl-rg` u tekstni okvir.
    - **Mjesto** padajućem okviru odaberite regiju.
    - Uređivati **Predložak korijenski Uri** ili tekstne okvire **Parametar korijenski Uri** .
    - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
    - Kliknite gumb **za kupnju** .

6. Pričekajte implementaciju da biste dovršili.

7. Desnom tipkom miša kliknite donji gumb i odaberite neki "Otvori vezu na novoj kartici" ili "Otvori vezu u novom prozoru":  
[![Implementacija Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fsecurity.azuredeploy.json)

8. Kada na portalu za Azure otvorio vezu, morate unijeti vrijednosti za neke postavke: 
    - Naziv **grupe resursa** već definiran u datoteci parametar, pa odaberite **Koristi postojeće** i unesite `ra-public-dmz-network-rg` u tekstni okvir.
    - **Mjesto** padajućem okviru odaberite regiju.
    - Uređivati **Predložak korijenski Uri** ili tekstne okvire **Parametar korijenski Uri** .
    - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
    - Kliknite gumb **za kupnju** .

9. Pričekajte implementaciju da biste dovršili.

10. Datoteka parametar obuhvaćaju programiranih administrator korisničko ime i lozinku za sve VMs i preporučuje da odmah promijeniti oboje. Za svaki VM implementacije odaberite na portalu za Azure, a zatim kliknite na **ponovno postavljanje lozinke** u plohu **podršku + otklanjanje poteškoća** . U okviru **način** padajućeg izbornika odaberite **ponovno postavljanje lozinke** , a zatim odaberite novo **korisničko ime** i **lozinku**. Kliknite gumb **Ažuriraj** održati.


<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[iptables]: https://help.ubuntu.com/community/IptablesHowTo
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md
[load-balancer]: ../load-balancer/load-balancer-internet-overview.md
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[0]: ./media/blueprints/hybrid-network-secure-vnet-dmz.png "Sigurne arhitektura hibridnog mreže"
