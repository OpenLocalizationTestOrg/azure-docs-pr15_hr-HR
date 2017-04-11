<properties
   pageTitle="Pokretanje više VMs | Referenca arhitektura | Microsoft Azure"
   description="Kako pokrenuti više instanci VM Azure za skalabilnost, otpornost, mogućnost upravljanja i sigurnost."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/19/2016"
   ms.author="mwasson"/>

# <a name="running-multiple-vms-on-azure-for-scalability-and-availability"></a>Skalabilnost i dostupnost više VMs sustavom Azure 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

U ovom se članku navode skup dokazana prakse za pokretanje više instanci virtualnim strojevima (VM) da biste poboljšali skalabilnost, dostupnost, mogućnost upravljanja i sigurnost.   

U toj arhitekturi povećavaju distribuira kroz sve instance VM. Postoji jedan javnu IP adresu, a internetski promet distribuira u VMs pomoću raspoređivača opterećenja. U ovom arhitektura može se koristiti za aplikaciju jednoslojnih, kao što su bez praćenja stanja web app ili prostora za pohranu klaster. Preporučuje se i sastavnog bloka za aplikacije N sloja. 

U ovom se članku sastavlja o [pokretanju jedan VM na Azure][single vm]. Preporuke u tom članku primijeniti tu arhitekturu.

## <a name="architecture-diagram"></a>Arhitektura dijagrama

VMs u Azure potreban je mreže i pohranu resurse za podršku.

> Visio dokument koji sadrži ovaj dijagram arhitektura dostupna je za preuzimanje na [Microsoftov centar za preuzimanje][visio-download]. Ovaj dijagram uključen u "računalnim – kartica VM višestruko." 

![[0]][0]

Arhitektura sadrži sljedeće komponente: 

- **Postavljanje dostupnosti.** [Postavljanje dostupnosti] [ availability set] sadrži na VMs te je potrebno za podršku [dostupnost SLA za Azure VMs][vm-sla].

- **VNet**. Svaki VM u Azure je uveden u virtualne mreže (VNet) koji je dodatno podijeljen **podmreže**.

- **Azure opterećenja.** [Učitavanje opterećenja] distribuira Internet zahtjevi za razgovore u instance VM u skupu dostupnost. Raspoređivača opterećenja sadrži nekoliko povezanih resursa:

  - **Javna IP adresa.** Javna IP adresa je potrebno za opterećenja prima internetski promet.

  - **Konfiguracija sučelja.** Na javnu IP adresu pridružuje raspoređivača opterećenja.

  - **Skupna pozadinsku adresa.** Sadrži sučelje mreže (NIC-ovi) za VMs koja će primiti dolazni promet.

- **Učitavanje opterećenja pravila.** Koristi se za distribuciju mrežni promet među VMs u pozadinskoj adresu. 

- **Pravila za NAT.** Služi za usmjeravanje prometa određene VM. Ako, na primjer, da biste omogućili udaljene radne površine protocol (RDP) da biste na VMs, stvorite pravilo prevođenje (NAT) zasebnu mrežu adresa za svaki VM. 

- **Mrežni sučelja (NIC-ovi)**. Svaki VM ima NIC za povezivanje s mrežom.

- **Prostor za pohranu.** Računi za pohranu držite VM slika i drugih vezane uz datoteke resursa, kao što su zabilježene po Azure VM dijagnostičkih podataka.

## <a name="recommendations"></a>Preporuke

Azure nudi mnogo različitih resurse i vrste resursa, tako da ovu referenca arhitekture može dodjeli brojne načine. Ne možemo je navesti predložak Voditelj resursa Azure da biste instalirali arhitektura reference koji se nalazi iza preporuke navedene u nastavku. Ako se odlučite za stvaranje vlastite arhitektura referenca slijedite ove preporuke u osim ako imate određene zahtjeva koji ne podržava i preporuke. 

### <a name="availability-set-recommendations"></a>Preporuke za postavljanje dostupnosti

Morate stvoriti barem dva VMs dostupnost postavljena za podršku [dostupnost SLA za Azure VMs][vm-sla]. Imajte na umu da Azure opterećenja i zahtijeva uravnoteženja VMs pripadate isti skup dostupnost.

### <a name="network-recommendations"></a>Preporuke o mreže

VMs iza raspoređivača opterećenja sve smjestiti u istoj podmreži. Prikazivanje VMs izravno s Internetom, ali umjesto toga dodijelite svaki VM privatne IP adrese. Klijenti povezuju pomoću na javnu IP adresu raspoređivača opterećenja.

### <a name="load-balancer-recommendations"></a>Preporuke za raspoređivača učitavanja

Dodajte sve VMs dostupnosti postavljena na skupna pozadinsku adresa od raspoređivača opterećenja.

Definirajte pravila raspoređivača opterećenja na izravni mrežni promet na VMs. Na primjer, da biste omogućili HTTP promet, stvoriti pravilo koje karata priključak 80 iz sučelja konfiguracije priključak 80 na na skupna pozadinsku adresa. Kada raspoređivača opterećenja primi zahtjev na priključak 80 na javnu IP adresu, ga će usmjeravanje zahtjev za priključak 80 na jedan od mrežne kartice u pozadinskoj adresu.

Definiranje NAT pravila za usmjeravanje prometa određene VM. Ako, na primjer, da biste omogućili RDP da biste na VMs stvorite pravilo zasebnom NAT za svaki VM. Svako pravilo mora mapirati broj priključka distinct priključak 3389, zadani je priključak za RDP. (Na primjer, koristi priključak 50001 za "VM1," priključak 50002 "VM2" itd.) Pravila za NAT dodijeliti mrežne kartice na na VMs. 

### <a name="storage-account-recommendations"></a>Preporuke za pohranu računa

Stvaranje računa zasebnom Azure prostora za pohranu za svaku VM držati virtualne tvrdom disku (VHDs) da biste izbjegli odlazak operacija ulaza i izlaza po drugi [ograničenja (IOPS)] [ vm-disk-limits] za račune za pohranu. 

Stvaranje jednog računa za pohranu za dijagnostičke zapisnike. Taj račun za pohranu možete zajednički koristiti tako da sva VMs.

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

Za skaliranje izvan VMs u Azure na dva načina: 

- Pomoću raspoređivača opterećenja mrežni promet raspodijelite skup VMs. Vremensko mjerilo, Dodjela resursa za dodatna VMs i njihovo iza raspoređivača opterećenja. 

- Korištenje [skupova skaliranje virtualnog računala][vmss]. Skaliranje skup sadrži broj speficied identične VMs iza raspoređivača opterećenja. Skaliranje VM postavlja podršku autoscaling na temelju metrike performansi. Kao što je opterećenje na VMs povećava, dodatne VMs automatski se dodaju na raspoređivača opterećenja. 

U sljedećim odjeljcima usporedbu ove dvije mogućnosti.

### <a name="load-balancer-without-vm-scale-sets"></a>Opterećenja bez VM skaliranje skupova

Raspoređivača opterećenja uzima zahtjevi za mreže i distribuira ih preko mrežne kartice u pozadinskoj adresu. Da biste skalirali vodoravno, dodati više instanci VM skup dostupnosti (ili deallocate VMs da biste skalirali prema dolje). 

Na primjer, pretpostavimo da koristite web-poslužitelj. Dodajte pravilo raspoređivača opterećenja za priključak 80 i/ili priključak 443 (za SSL). Kada klijent pošalje HTTP zahtjev, raspoređivača opterećenja izdvajanja u pozadinskoj IP adresu pomoću programa [raspršivanje algoritam] [ load balancer hashing] koja sadrži IP adresu izvora. Na taj način, zahtjevi klijenta raspodijeliti su sve VMs. 

> [AZURE.TIP] Kada dodate nove VM raspoloživost postavite, provjerite jeste li stvorili na NIC za na VM, a dodavanje NIC-a u pozadinskoj skupna adresa na raspoređivača opterećenja. U suprotnom, internetski promet neće biti usmjeren novi VM.

Azure pretplate ima zadani ograničenja na mjestu, uključujući maksimalni broj VMs po regijama. Ograničenje možete povećati tako da arhiviranja zahtjev za podršku. Dodatne informacije potražite u članku [Azure pretplate i ograničenja servisa, kvote, i ograničenjima][subscription-limits].  

### <a name="vm-scale-sets"></a>Skupovi VM skala 

Skupove skaliranje VM olakšavaju upravljanje skup identične VMs i. Sa svim VM skaliranje skupove podržavao je true automatsko skaliranje bez unaprijed dodjele resursa VMs, što olakšava stvaranje veliki services ciljanja veliki računalnim, velikih skupova podataka i containerized radnih opterećenja, VMs konfiguriran isti. 

Dodatne informacije o skupovima skaliranje VM potražite u članku [Pregled postavlja za virtualnog računala skaliranje][vmss].

Zahtjevi za korištenje VM skaliranje skupova:

- Ako morate brzo skaliranja izvan VMs i morate automatsko skaliranje, preporučuje se skaliranje skupove. 

- Trenutno skaliranje skupove ne podržava diskova podataka. Mogućnosti za spremanje podataka su Azure pohrani, pogon OS, pogon Temp ili vanjski pohrana, kao što su Azure prostora za pohranu. 

- Sve instance VM u rasponu koji se postavljaju automatski pripada isti skup dostupnosti s 5 kvara domene i 5 ažuriranje domene.

- Prema zadanim postavkama, skaliranje skupove koristite "overprovisioning", što znači da se skup skaliranje prethodno dodjeljuje više VMs nego što tražite, a zatim briše dodatni VMs. To poboljšava cjelokupan uspješnosti prilikom dodjele resursa u VMs. 

- Preporučujemo da više, a zatim od 20 VMs po prostora za pohranu račun s overprovisioning omogućeno ili više od 40 VMs s overprovisioning onemogućen.  

- Možete pronaći predloške resursima za implementacija skaliranje postavlja u [Azure predložaka za brzi početak rada][vmss-quickstart].

- Postoje dva načina osnovni da biste konfigurirali VMs implementiran u skupu Skaliranje: stvoriti prilagođenu sliku ili koristite proširenja za konfiguriranje na VM nakon što su mu dodijeljeni resursi.

    - Skup skaliranje utemeljena na prilagođenu sliku morate stvoriti sve VHDs disk OS unutar jednog računa za pohranu. 

    - Prilagođene slikama, morate ažurirati sliku.

    - S datotečnim nastavcima, može trajati dulje za nove dodijeljenu VM za Okretni prema gore.

Dodatna pitanja vezana uz potražite u članku [Dizajniranje VM skaliranje skupovi za skaliranje][vmss-design].

> [AZURE.TIP]  Prilikom korištenja rješenjem automatsko skaliranje ga testirati s opterećenje radnog razinom rad i unaprijed. 

## <a name="availability-considerations"></a>Razmatranja dostupnosti

Dostupnost skup čini aplikacije više prebacuju planiranog i Neplanirana održavanja događaja.

- _Planirano održavanje_ pojavljuje kada Microsoft ažurira podlozi platformu ponekad uzrokuje VMs ponovno pokrenuti. Azure čini li VMs u skupu dostupnost nisu sve ponovno pokrenuti u isto vrijeme, barem jedno se drži pokrenut dok drugi ponovnog pokretanja.

- _Održavanje neplanirano_ se događa ako postoji kvara hardvera. Azure jamči da se VMs u skupu dostupnost dodjeli preko više poslužitelja za bicikle. To omogućuje smanjivanje utjecaj hardverske pogreške, mreže kvarove, power prekide i tako dalje.

Dodatne informacije potražite u članku [Upravljanje dostupnost virtualnim strojevima][availability set]. Sljedeći videozapis ima dobar pregled skupa dostupnost: [Kako ću konfigurirati dostupnost postavljen na skaliranje VMs][availability set ch9]. 

> [AZURE.WARNING]  Provjerite je li za konfiguriranje dostupnosti postaviti prilikom dodjele resursa u VM. Trenutno ne postoji način da biste dodali resursima VM raspoloživost postaviti nakon na VM je dodijeljena.

Raspoređivača opterećenja koristi [stanja probes] Praćenje Dostupnost VM instanci. Ako je probni ne može pristupiti instance unutar vremenskog ograničenja razdoblja, raspoređivača opterećenja prestaje pošaljete promet na tom VM. Međutim, raspoređivača opterećenja će se i dalje isprobati i ako na VM ponovno postane dostupan, raspoređivača opterećenja nastavlja pošaljete promet na tom VM.

Evo nekih preporuke o probes za stanje raspoređivača opterećenja:

- Probes možete testirati HTTP-a ili TCP. Ako VMs pokretanja HTTP poslužitelj (IIS, nginx, Node.js aplikacije i tako dalje), stvorite probni programa HTTP-a. U suprotnom stvorite TCP probni.

- Probni programa HTTP Navedite put do krajnju točku HTTP-a. Na probni provjerava HTTP 200 odgovor ovaj put. To se može biti korijenski put ("/") i nadzor stanja krajnje koji implementira neke prilagođene logike da biste provjerili stanje aplikacije. Krajnja točka morate omogućiti anonimni HTTP zahtjeva.

- Na probni poslali na [poznati] [ health-probe-ip] 168.63.129.16 IP adresa. Provjerite ne blokiraj promet ili s ovom IP u bilo kojem pravila vatrozida ili mreže sigurnosnih grupa (NSG) pravila.

- Pomoću [zapisnika probni stanja] [ health probe log] da biste vidjeli stanje probes stanje. Omogući bilježenje Azure portalu za svaki opterećenja. Spremište blobova platforme Azure zapisuju se zapisnika. U zapisnicima navedene koliko VMs na pozadinska stiže mrežni promet zbog neuspjelog probni odgovore.

## <a name="manageability-considerations"></a>Pitanja vezana uz mogućnost upravljanja

S više VMs, on postaje važno Automatizacija procesa, tako da budu pouzdani i kontrolirani. Možete koristiti [Automatizaciju Azure] [ azure-automation] da biste automatizirali implementacije OS zakrpa te druge zadatke. Azure Automatizacija je automation Services koje pokreće Azure i temelji se na Windows PowerShell. Primjer Automatizacija skripte su dostupni u [Galeriji Runbook] na mreži TechNet.

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

Virtualne mreže su granicu odvajanja promet u Azure. VMs u jednom VNet komunikacija izravno u VMs u različitim VNet. VMs unutar iste VNet možete komunicirati dok ne stvorite [mreže sigurnosne grupe] [ nsg] (NSGs) da biste ograničili promet. Dodatne informacije potražite u članku [Microsoftovim servisima u oblaku i sigurnost mrežne][network-security].

Za dolazne internetski promet pravila raspoređivača opterećenja definirati koje promet možete doći do pozadinska. Međutim, pravila raspoređivača opterećenja ne podržavaju stvaranja popisa IP dopuštenih stavki, pa ako želite da se NSG whitelist određene javnu IP adrese, dodati podmreži.

## <a name="solution-deployment"></a>Uvođenje rješenja

Uvođenje za referencu arhitektura koji implementira te preporuke i dalje dostupna je na GitHub. Arhitektura ovu referenca sadrži virtualne mreže (VNet), mreže sigurnosne grupe (NSG), opterećenja i dva virtualnim strojevima (VMs).

Arhitektura referenca može uvesti pomoću sustava Windows ili Linux VMs slijedeći upute u nastavku: 

1. Desnom tipkom miša kliknite donji gumb, a zatim odaberite neki "Otvori vezu na novoj kartici" ili "Otvori vezu u novom prozoru":  
[![Implementacija Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-multi-vm%2Fazuredeploy.json)

2. Kada na portalu za Azure otvorio vezu, morate unijeti vrijednosti za neke postavke: 
    - Naziv **grupe resursa** već definiran u datoteci parametar, pa odaberite **Koristi postojeće** i unesite `ra-multi-vm-rg` u tekstni okvir.
    - **Mjesto** padajućem okviru odaberite regiju.
    - Uređivati **Predložak korijenski Uri** ili tekstne okvire **Parametar korijenski Uri** .
    - Odaberite **Vrstu Os** na padajućem okviru, **windows** ili **linux**. 
    - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
    - Kliknite gumb **za kupnju** .

3. Pričekajte implementaciju da biste dovršili.

4. Parametar datoteke uključuje programiranih administrator korisničko ime i lozinku, a preporučuje da odmah promijeniti oba. Kliknite na VM pod nazivom `ra-multi-vm1` na portalu za Azure. Zatim na **ponovno postavljanje lozinke** u plohu **podršku + otklanjanje poteškoća** . U okviru **način** padajućeg izbornika odaberite **ponovno postavljanje lozinke** , a zatim odaberite novo **korisničko ime** i **lozinku**. Kliknite gumb **Ažuriraj** da biste spremili novi korisničko ime i lozinku. Ponovite to za VM pod nazivom `ra-multi-vm2`.

Informacije o dodatnim načinima za implementaciju ovu referenca arhitektura, potražite u članku datoteke readme u [upute, jednom i vm] [ github-folder] GitHub mapu. 

## <a name="next-steps"></a>Daljnji koraci

Stavljanje nekoliko VMs iza raspoređivača opterećenja je sastavni blok za stvaranje više razina arhitekturi. Dodatne informacije potražite u članku [Izvodi Windows VMs za Arhitektura programa N sloja na Azure] [ n-tier-windows] i [Pokretanje Linux VMs za Arhitektura programa N sloja na Azure][n-tier-linux]

<!-- Links -->
[availability set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[availability set ch9]: https://channel9.msdn.com/Series/Microsoft-Azure-Fundamentals-Virtual-Machines/08
[azure-automation]: https://azure.microsoft.com/en-us/documentation/services/automation/
[azure-cli]: ../virtual-machines-command-line-tools.md
[bastion host]: https://en.wikipedia.org/wiki/Bastion_host
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-multi-vm
[health probe log]: ../load-balancer/load-balancer-monitor-log.md
[Stanje probes]: ../load-balancer/load-balancer-overview.md#service-monitoring
[health-probe-ip]: ../virtual-network/virtual-networks-nsg.md#special-rules
[opterećenja]: ../load-balancer/load-balancer-get-started-internet-arm-cli.md
[load balancer hashing]: ../load-balancer/load-balancer-overview.md#hash-based-distribution
[n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[n-tier-windows]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[network-security]: ../best-practices-network-security.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md 
[Galerija Runbook]: ../automation/automation-runbook-gallery.md#runbooks-in-runbook-gallery
[single vm]: guidance-compute-single-vm.md
[subscription-limits]: ../azure-subscription-service-limits.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_2/
[vmss]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md
[vmss-design]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md
[vmss-quickstart]: https://azure.microsoft.com/documentation/templates/?term=scale+set
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[0]: ./media/blueprints/compute-multi-vm.png "Arhitekturu rješenja za višestruki VM na Azure comprising raspoloživost postavite s dva VMs i raspoređivača opterećenja"
