<properties
   pageTitle="Sustavom Linux VMs za Arhitektura programa N sloja Azure | Microsoft Azure"
   description="Kako pokrenuti Linux VMs Arhitektura programa N sloja u Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-for-an-n-tier-architecture-on-azure"></a>Sustavom Linux VMs za Arhitektura programa N sloja Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]


> [AZURE.SELECTOR]
- [Sustavom Linux VMs za Arhitektura programa N sloja Azure](guidance-compute-n-tier-vm-linux.md)
- [Sustavom Windows VMs za Arhitektura programa N sloja Azure](guidance-compute-n-tier-vm.md)

U ovom se članku navode skup dokazana prakse za pokretanje Linux virtualnim strojevima (VMs) za aplikaciju s Arhitektura programa N sloja. Sastavlja članka na vrpci [radi više VMs na Azure][multi-vm].

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Resursima] [ resource-manager-overview] i classic. U ovom se članku koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

## <a name="architecture-diagram"></a>Arhitektura dijagrama

Postoje varijacije arhitekturi N sloja. U najvećem, bi trebalo bitan razlike u svrhu te preporuke. U ovom se članku pretpostavlja uobičajeni 3 sloja web-aplikacijama:

- **Web razina.** Rukuje HTTP zahtjevi za razgovore. Odgovori se vraćaju kroz ovaj sloju.

- **Razina tvrtke.** Primjenjuje poslovnih procesa i druge funkcionalni logike za sustav.

- **Razina baze podataka.** Omogućuje spremanje stalni podataka pomoću Apache Cassandra za visoke dostupnosti.

> Visio dokument koji sadrži ovaj dijagram arhitektura dostupna je za preuzimanje na [Microsoftov centar za preuzimanje][visio-download]. Ovaj dijagram uključen u "računalnim - više razina (Linux) stranice.

![[0]][0]

- **Skupovi dostupnost.** Stvaranje programa [Postavljanje dostupnosti] [ azure-availability-sets] za svaku sloju i dodjela najmanje dva VMs u svakom sloju. Potreban je takvog dosegne dostupnost [SLA] [ vm-sla] za VMs.

- **Podmreže.** Stvorite zaseban podmreže za svaki sloju. Navedite adresu raspon i podmreže maska pomoću [CIDR] notacije. 

- **Učitavanje balancers.** Korištenje [mjesto na Internetu opterećenja] [ load-balancer-external] distribuirati dolazne internetski promet web razina i [Interna opterećenja] [ load-balancer-internal] distribuirati mrežni promet iz sloju web razina tvrtke.

- **Jumpbox**. Na _jumpbox_, naziva se i [bastion glavno računalo]je VM na mreži koje administratori koristi za povezivanje s drugim VMs. Na jumpbox ima programa NSG koji omogućuje udaljeni promet samo iz whitelisted javnu IP adrese. Na NSG treba dopušta promet sigurne ljuske (SSH).

- **Nadzor**. Nadzor softver kao što su [Nagios], [Zabbix]ili [Icinga] mogu vam dati uvid u reakcija, vrijeme VM aktivnosti i stanje sustava. Instalirajte nadzora softver na VM koji se smješta u zasebnom upravljanje podmreže.

- **NSGs**. Koristite [mrežni sigurnosne grupe] [ nsg] (NSGs) da biste ograničili mrežni promet unutar na VNet. Ako, na primjer, 3 sloja arhitekture što je prikazano ovdje, sloju baze podataka ne prihvaća promet s web-sučelju, samo iz sloju tvrtke i upravljanje podmreži.

- **Apache Cassandra baze podataka**. Nudi visoke dostupnosti u sloju podataka putem omogućivanja replikacije i prebacivanje.

## <a name="recommendations"></a>Preporuke

Azure nudi mnogo različitih resurse i vrste resursa, tako da ovu referenca arhitekture može dodjeli brojne načine. Ne možemo je navesti predložak Voditelj resursa Azure da biste instalirali arhitektura reference koja slijedi te preporuke. Ako se odlučite za stvaranje vlastite arhitektura referenca slijedite ove preporuke osim ako imate određene zahtjeva koji ne stane i preporuke.

### <a name="vnet--subnets"></a>VNet / podmreže

Kada stvorite na VNet, za podmreže ćete dodijeliti dovoljno prostora na adresu. Navedite VNet adresa raspon i podmreže maska pomoću [CIDR] notacije. Koristiti prostor za adresu koja se nalazi unutar standardne [privatne blokovi IP adresa][private-ip-space], koji su 10.0.0.0/8, 172.16.0.0/12 i 192.168.0.0/16.

Odaberite raspon adresa koji se ne preklapa s mrežom na lokaciji u slučaju da trebate kasnije postavljanje pristupnika između na VNet i mreže na lokaciji. Kada stvorite na VNet, ne možete promijeniti adresu raspona.

Dizajnirajte podmreže potrebe funkcionalnost i sigurnost na umu. Sve VMs unutar istog sloju ili uloga prosljeđuje u istoj podmreži, što može biti granicu sigurnost. Dodatne informacije o dizajniranju VNets i podmreže potražite u članku [Planiranje i dizajniranje Azure virtualne mreže][plan-network].

Za svaki podmreže Navedite adresu prostora za podmreži CIDR notacijom. Na primjer, "10.0.0.0/24" stvara raspon 256 IP adrese. (VMs možete koristiti 251 od ovih; pet rezerviranih. Pročitajte članak [virtualne mreže najčešća pitanja vezana uz][vnet faq].) Provjerite je li rasponi adresa ne preklapa preko podmreže.

### <a name="load-balancers"></a>Učitavanje balancers

Vanjski opterećenja distribuira internetski promet za web razina. Stvaranje javne IP adresa za ovaj opterećenja. U odjeljku [Stvaranje na mjesto na Internetu opterećenja][lb-external-create].

Interna opterećenja distribuira mrežni promet iz sloju web razina tvrtke. Da biste dodijelili ovom opterećenja privatne IP adrese, stvorite sučelju IP konfiguracija i pridružiti podmreže za sloju tvrtke. Potražite u članku [Početak rada prilikom stvaranja interne opterećenja][lb-internal-create].

### <a name="cassandra"></a>Cassandra

Preporučujemo da [DataStax Enterprise] [ datastax] za proizvodnju korištenje, ali te preporuke primijeniti bilo koje Cassandra izdanje. Dodatne informacije o pokretanju DataStax u Azure potražite u članku [Vodič za implementaciju Enterprise DataStax za Azure][cassandra-in-azure]. 

Smjestite VMs za Cassandra klaster skupa dostupnost da biste bili sigurni da replike Cassandra su raspodijeliti više domena kvara i nadogradnja domene. Dodatne informacije o domenama kvara i nadogradnje domena potražite u članku [Upravljanje dostupnost virtualnim strojevima][availability-sets-manage]. 

Konfiguriranje 3 kvara domena (maksimalno) po skupu dostupnost. 

Konfiguriranje 18 nadogradnje domena po skupu dostupnost. To vam maksimalni broj nadogradnje domene ne možete i dalje biti raspodijeliti ravnomjerno kvara domene.   

Konfiguriranje čvorove u načinu rada za bicikle umu. Mapiranje kvara domene nosači za u na `cassandra-rackdc.properties` datoteku.

Ne morate opterećenja ispred klaster. Klijent se povezuje izravno čvor u klasteru.

### <a name="jumpbox"></a>Jumpbox

Postavite na jumpbox u istu VNet kao drugi VMs, ali u zasebnom upravljanje podmreže.

Stvorite [javnu IP adresa] za na jumpbox.

Korištenje malu veličinu VM jumpbox, primjerice standardni A1.

Konfiguriranje NSGs web razina, razina tvrtke i podmreže sloju baze podataka da dopušta promet za administratora (SSH) tako da prolazi kroz iz podmreže upravljanje.

Sigurnost na jumpbox stvaranje programa NSG i primijenite ga na podmreži jumpbox. Dodajte pravilo NSG koji omogućuje SSH veze samo iz skupa whitelisted javnu IP adrese.

U NSG mogu priložiti podmreži ili jumpbox NIC. U ovom slučaju, preporučujemo da prilaganja NIC-a, pa SSH promet je dopušteno samo jumpbox, čak i ako se u istoj podmreži dodate druge VMs.

Dopusti pristup SSH iz javnog Interneta da biste VMs koji se izvode radno opterećenje aplikacije. Umjesto toga SSH pristupa te VMs morate primitak na jumpbox. Administrator prijavi u u jumpbox, a zatim prijavi u VM iz na jumpbox. Na jumpbox dopušta promet SSH s Interneta, ali samo s poznatim, whitelisted IP adrese.

## <a name="availability-considerations"></a>Razmatranja dostupnosti

Postavite svaki sloju ili VM uloga u zasebnom dostupnosti skupa. Nemojte smještati VMs iz različite razine u isti skup dostupnost. 

U sloju baze podataka pojavljuju više VMs ne automatski Prevedite iznimno dostupnih baza podataka. Za relacijske baze podataka, obično ćete koristiti replikaciju i prebacivanje da biste postigli visoke dostupnosti.  

Ako vam je potrebna veća dostupnost od [Azure SLA za VMs] [ vm-sla] nudi, replicirati aplikacije preko dviju regija i pomoću upravitelja promet Azure za prebacivanje. Dodatne informacije potražite u članku [Pokretanje Linux VMs u više područja za visoke dostupnosti][multi-dc].  

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

Pomoću pravila NSG da biste ograničili promet između razine. Ako, na primjer, u gore navedenoj sintaksi arhitekturi 3 sloja, web razina komunicirati izravno s sloju baze podataka. Da biste nametnuli, razina baze podataka mora Blokiraj dolazne promet od podmreži web razina.  

  1. Stvaranje programa NSG i povezati je u sloju podmreži baze podataka.

  2. Dodavanje pravila koja blokira sav dolazni promet iz na VNet. (Koristi se `VIRTUAL_NETWORK` oznaka u pravilu.) 

  3. Dodavanje pravila s većim prioritetom koji omogućuje dolazni promet s sloju podmreže tvrtke. Ovo pravilo nadjačava prethodni pravilo, a omogućuje sloju tvrtke razgovarati s sloju baze podataka.

  4. Dodavanje pravila koja omogućuje dolazni promet s podmreže sloju baze podataka sam. Tim se pravilom omogućuje komunikaciju između VMs u sloju baze podataka, što je potrebno za replikacije baze podataka i prebacivanje.

  5. Dodajte pravilo koje dopušta promet SSH s jumpbox podmreže. Tim se pravilom omogućuje povezivanje s sloju baze podataka u jumpbox za administratore.

  > [AZURE.NOTE] U NSG je [Zadana pravila] [ nsg-rules] koje omogućuju sav dolazni promet od unutar na VNet. Ta pravila nije moguće izbrisati, ali ih možete nadjačati stvaranjem viši prioritet pravila.

Razmislite o dodavanju potražite virtualne mreže (NVA) da biste stvorili DMZ između javnog Interneta i Azure virtualne mreže. NVA je generički izraz za virtualne uređaj koji izvršava zadatke povezane s mrežom kao što je vatrozid, paketa provjeru, nadzor, prilagođene usmjeravanje ili raznih ostalih operacija. Dodatne informacije potražite u članku [implementacijom DMZ između Azure i Interneta][dmz].

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

Učitavanje balancers distribucija mrežni promet na web- a tvrtke razine. Promjena veličine vodoravno tako da dodate nove instance VM. Imajte na umu da vam mogu mijenjati veličinu razine web i business zasebno, na temelju učitavanja. Da biste smanjili moguće komplikacije uzrokovanih potrebno da biste zadržali afinitet klijent, VMs u sloju web mora biti bez praćenja stanja. VMs hosting poslovne logike smatraju bez praćenja stanja.

## <a name="manageability-considerations"></a>Pitanja vezana uz mogućnost upravljanja

Pojednostavnite upravljanje cijelog sustava pomoću alata za središnje administracije kao što su [Automatizacija Azure][azure-administration], [Microsoft operacije upravljanja paket][operations-management-suite], [Chef][chef], ili [Puppet][puppet]. Ove alate možete konsolidirati informacije Dijagnostika i stanje zabilježene iz većeg broja VMs omogućuje Opći pregled sustava.

## <a name="solution-deployment"></a>Uvođenje rješenja

Uvođenje za referencu arhitektura koji implementira te preporuke i dalje dostupna je na [Github][github-folder]. Ovu referenca arhitektura obuhvaća web razina, razina tvrtke i sloj podataka.

1. Kliknite donji gumb.  
[! ["Implementacija Azure"] [1]][2]

2. Kada na portalu za Azure otvorio vezu, unesite vrijednosti za praćenje: 
  - Naziv **grupe resursa** već definiran u datoteci parametar, pa odaberite **Stvori novo** i unesite `ra-ntier-sql-network-rg` u tekstni okvir.
  - **Mjesto** padajućem okviru odaberite regiju.
  - Uređivati **Predložak korijenski Uri** ili tekstne okvire **Parametar korijenski Uri** .
  - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
  - Kliknite gumb **za kupnju** .

3. Provjerite je li Azure portala obavijesti za implementaciju poruku dovršeno.

4. Parametar datoteke uključuju na programiranih administrator korisnička imena i lozinke te ga preporučuje se da odmah promijeniti i na sve VMs. Kliknite svaki VM na portalu za Azure, a zatim kliknite **ponovno postavljanje lozinke** u plohu **podršku + otklanjanje poteškoća** . U okviru **način** padajućeg izbornika odaberite **ponovno postavljanje lozinke** , a zatim odaberite novo **korisničko ime** i **lozinku**. Kliknite gumb **Ažuriraj** održati novo korisničko ime i lozinku.

## <a name="next-steps"></a>Daljnji koraci

Da biste postigli visoke dostupnosti za ovu referenca arhitektura, preporučujemo da [uvođenje na više područja][multi-dc].

<!-- links -->

[azure-administration]: ../automation/automation-intro.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[availability-sets-manage]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[glavno računalo bastion]: https://en.wikipedia.org/wiki/Bastion_host
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-consistency-usage]: http://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-in-azure]: https://docs.datastax.com/en/datastax_enterprise/4.5/datastax_enterprise/install/installAzure.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[cidr]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[datastax]: http://www.datastax.com/products/datastax-enterprise
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters-linux.md
[multi-vm]: guidance-compute-multi-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[Javna IP adresa]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[0]: ./media/blueprints/compute-n-tier-linux.png "N sloja arhitektura koristeći Microsoft Azure"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier%2Fazuredeploy.json


