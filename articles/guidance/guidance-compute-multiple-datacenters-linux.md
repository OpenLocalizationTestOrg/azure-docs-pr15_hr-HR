<properties
   pageTitle="Pokretanje Linux VMs u više područja za visoke dostupnosti | Referenca arhitektura | Microsoft Azure"
   description="Kako implementirati VMs na više područjima Azure visoke dostupnosti i otpornost."
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
   ms.date="10/21/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-in-multiple-regions-for-high-availability"></a>Pokretanje Linux VMs u više područja za visoke dostupnosti

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Pokretanje Linux VMs u više područja za visoke dostupnosti](guidance-compute-multiple-datacenters-linux.md)
- [Izvodi Windows VMs u više područja za visoke dostupnosti](guidance-compute-multiple-datacenters.md)

U ovom se članku preporučujemo skup prakse da biste pokrenuli Linux virtualnim strojevima (VMs) u više Azure regija, da biste postigli dostupnosti i infrastruktura za oporavak robusne Izrada.

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Resursima] [ resource groups] i classic. U ovom se članku koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

Više područja arhitektura unijeti veća dostupnost od implementacija jedno područje. Ako regionalne nedostupnosti utječe primarni regija, pomoću [Upravitelja promet] [ traffic-manager] uvoza putem sekundarne regiju. U ovom arhitektura mogu pomoći ne uspijete pojedinačne podsustav aplikacije.

Postoji nekoliko općenitih pristupa postizanja visoke dostupnosti svim centrima podataka:   
  
- Aktivno/pasivni s tipkovni čekanja. Promet odlazak određenu regiju tijekom Čekanje na čekanja. VMs u području sekundarne su dodijeljene i pokrenuta cijelo vrijeme.

- Aktivno/pasivni s Hladna čekanja. Isti, ali VMs u području sekundarne nije dodijeljeno dok potrebnog za prebacivanje. Takvog troškove manje da biste pokrenuli, ali obično imati više pritisnutu tijekom pogreške.

- Aktivno/aktivno. Obje regije aktivna, a zahtjevi za rasporediti između njih opterećenje. Ako jednog podatkovnog centra postane dostupan, koristi se iz rotacije.

U ovom arhitektura usredotočuje se na aktivno/pasivni s tipkovni stanja čekanja pomoću upravitelja promet za prebacivanje. Imajte na umu da nije moguće implementirati mali broj VMs za tipkovni čekanja i zatim Skaliraj izvan prema potrebi.

## <a name="architecture-diagram"></a>Arhitektura dijagrama

Na sljedećem su dijagramu sastavlja na arhitekturi prikazano [pouzdanosti dodavanje da biste je N sloja arhitektura na Azure](guidance-compute-n-tier-vm-linux.md). 

> Visio dokument koji sadrži ovaj dijagram arhitektura dostupna je za preuzimanje na [Microsoftov centar za preuzimanje][visio-download]. Ovaj dijagram uključen u "računalnim - više područja (Linux) stranice.

![[0]][0]

- **Primarnih i sekundarnih područja**. U ovom arhitektura koristi dva područja da biste postigli veća dostupnost. Jedna je primarni regija. Tijekom uobičajenih postupaka mrežni promet usmjeravanja primarni regiju. No ako koji postane dostupan, promet usmjeravanja sekundarne regiju.

- ** [Upravitelj promet azure] [ traffic-manager] ** usmjerava zahtjevi za primarni regiju. Ako to područje postane dostupan, Upravitelj promet neće uspjeti putem sekundarne regiju. Dodatne informacije potražite u odjeljku [Konfiguriranje Upravitelj promet](#configuring-traffic-manager).

- **Grupa resursa**. Stvaranje zasebnih [grupa resursa] [ resource groups] za primarni regiju, sekundarne regija i za Upravitelj promet. Tako ćete dobiti fleksibilnost za upravljanje svaku regiju kao jedan skup resursa. Ako, na primjer, nije moguće implementirati određenu regiju bez poduzimanja prema dolje u drugu. [Povezivanje grupe resursa][resource-group-links], tako da pokrenete upit za popis resursa za aplikaciju.

- **VNets**. Stvorite zaseban VNet za svako područje. Provjerite je li adresa razmake se preklapaju.

- **Apache Cassandra** implementiran u podatkovnim centrima preko Azure područja. Centri podataka Cassandra su implementiran u različitim područjima Azure za visoke dostupnosti. U svakom području čvorove konfigurirane u načinu za bicikle umu s kvara i nadogradite domenama za otpornost unutar područja.

## <a name="recommendations"></a>Preporuke

Azure nudi mnogo različitih resurse i vrste resursa, tako da ovu referenca arhitekture može dodjeli brojne načine. Ne možemo je navesti predložak Voditelj resursa Azure da biste instalirali arhitektura reference koja slijedi te preporuke. Ako se odlučite za stvaranje vlastite arhitektura referenca slijedite ove preporuke osim ako imate određene zahtjeva koji ne stane i preporuke.

### <a name="regional-pairing"></a>Regionalne uparivanja

Svako područje Azure uparen je s drugog područja unutar iste Zemljopis. Općenito govoreći, odaberite područja s istom regionalne par (na primjer, Istočni sad 2 i NAM središnje). Prednosti time obuhvaćaju sljedeće:

- Ako postoji širok prekida, oporavak barem jedan područja iz svaki par je prioritet.

- Ažuriranja planiranog Azure sustava su poslednjeg područja upareni sekvencijalno, da biste minimizirali moguće isključiti.

- Parove nalaziti unutar iste Zemljopis da bi odgovarao potrebama residency podataka.

No, provjerite je li da obje regije podržavaju sve potrebne za svoju aplikaciju servise za Azure (pogledajte [Services po regijama][services-by-region]). Dodatne informacije o regionalnim parove potražite u članku [tvrtke continuity i Izrada oporavak (BCDR): Uparena područja Azure][regional-pairs].

### <a name="traffic-manager-configuration"></a>Konfiguracija upravitelja promet

Imajte na umu sljedeće prilikom konfiguriranja promet upravitelja za scenariju:

- **Usmjeravanje.** Upravitelj promet podržava nekoliko [usmjeravanje algoritama][tm-routing]. Scenarija opisane u ovom članku, korištenje _Prioritet_ usmjeravanja (prije se zvao _Prebacivanje_ usmjeravanje). Uz tu postavku, promet Upravitelj šalje sve zahtjeve za primarni regiju, osim ako se primarni područje postane nedostupan. U tom trenutku ga automatski ne uspijeva putem sekundarne regiju. Potražite u članku [Konfiguriranje prebacivanje usmjeravanje način][tm-configure-failover].

- **Probni stanja.** Upravitelj promet koristi HTTP (ili HTTPS) [probni] [ tm-monitoring] Praćenje Dostupnost svako područje. Na probni provjerava je HTTP 200 odgovor za navedeni put URL-a. Preporučenim načinom rada stvaranje krajnje izvješća stanje aplikacije te koristite ovaj krajnja točka za probni stanja. U suprotnom se probni možda izvješće "dobar" krajnje točke kada ključnih dijelove aplikacija su zapravo ne uspijeva. Dodatne informacije potražite u članku [Nadzor uzorak krajnja točka za stanje][health-endpoint-monitoring-pattern].

Kada upravitelj promet ne uspije iznad, postoji neko vrijeme kada klijenti ne može pristupiti aplikaciji, što može biti nekoliko minuta. Dva čimbenika utječe na ukupno trajanje:

- Probni stanja morate otkriva primarni podatkovnog centra u je postao dostupan.

- DNS poslužitelji morate ažurirati predmemorirani DNS zapise za IP adresu, a to ovisi o na DNS vrijeme važenja (TTL). Zadani TTL je 300 sekundi (5 minuta), ali možete konfigurirati tu vrijednost pri stvaranju profila Upravitelja promet.

Detalje potražite u članku [O nadzor promet Upravitelj][tm-monitoring].

Upravitelj promet ne uspije iznad, preporučujemo da izvođenje ručno failback umjesto da automatski ne uspijeva natrag. Provjerite jesu li sve aplikacije podsustava dobar prvi put. U suprotnom, možete stvoriti situaciji gdje aplikacije natrag i naprijed Zrcali između centre za podatke.

Po zadanom upravitelju promet automatski neće uspjeti natrag. Da biste spriječili, ručno smanjite prioritet područja za primarni nakon prebacivanje događaj. Ako, na primjer, pretpostavimo da područje primarni je prioritet 1, a sekundarne prioritet 2. Nakon prebacivanje, postavite primarni regija prioritet 3, da biste spriječili automatsko failback. Kada ste spremni za povratak, ažurirajte prioritet 1.

Sljedeća naredba Azure EŽA obnavlja prioritet:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --priority 3
```    

Da biste izbjegli flip-flop tako da biste privremeno onemogućiti krajnja točka:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --status Disabled
```    

Ovisno o uzroku s pomoćnim ćete možda morati redploy resursi unutar područja. Prije neuspješnih ponovno izvođenje u radu spremnosti test. Test trebali biste provjeriti elemente kao što su:

- VMs pravilno konfigurirani. (Sve potrebne softver je instaliran, IIS je pokrenut itd.)

- Dobar su podsustava aplikacije.

- Funkcionalno testiranja. (Ako, na primjer, razina baze podataka nije dostupna iz web razina.)

### <a name="cassandra-deployment-across-multiple-regions"></a>Uvođenje Cassandra preko više područja

Centri podataka Cassandra su odjelima radnih opterećenja: grupe povezanih čvorove konfiguriranih zajedno unutar klaster za replikaciju i radno opterećenje segregation.

Preporučujemo da [DataStax Enterprise] [ datastax] za proizvodnju koristiti. Dodatne informacije o pokretanju DataStax u Azure potražite u članku [Vodič za implementaciju Enterprise DataStax za Azure][cassandra-in-azure]. Sljedeće knjige primjenjuju se na bilo koje Cassandra izdanje.

- Javnu IP adresu dodijeliti svakom čvor. Time se omogućuje klastere komunikaciju preko područja pomoću Infrastruktura Azure backbone pruža visoke propusnost pri niskoj trošak.

- Sigurne čvorove pomoću odgovarajuće vatrozid i konfiguracija NSG, što omogućuje promet samo i s poznatim domaćini, uključujući klijenti i ostale čvorove klaster. Imajte na umu da Cassandra koristi drugi priključke za komunikaciju, OpsCenter, Spark itd. Korištenje priključak u Cassandra, potražite u članku [Konfiguriranje vatrozida priključak pristup][cassandra-ports].

- Korištenje šifriranja SSL za sve [klijent čvor] [ ssl-client-node] i [čvor čvor] [ ssl-node-node] komunikacije.

- Unutar područja, slijedite upute navedene u [Cassandra preporuke](guidance-compute-n-tier-vm-linux.md#cassandra-recommendations).

## <a name="availability-considerations"></a>Razmatranja dostupnosti

Uz složene aplikaciju N sloja, ne morate za replikaciju cijelu aplikaciju u području sekundarne. Umjesto toga, možda samo replicirati ključnih podsustav koji je potreban za podršku poslovanje.

Voditelj promet je moguće pogreška točke u sustavu. Ako servis ne uspije, klijenti ne može pristupiti aplikaciji tijekom na nedostupnost. Pregled [Upravitelja promet SLA][tm-sla], i odrediti hoće li pomoću upravitelja promet samostalno zadovoljava vaše tvrtke preduvjete za visoke dostupnosti. Ako nije, razmislite o dodavanju drugo rješenje za upravljanje promet kao u failback. Ako servisa Azure promet Upravitelj ne uspije, promijenite CNAME zapisa u DNS-a na druge promet servis za upravljanje. (Ovaj korak moraju izvršiti ručno, i aplikacije više neće biti dostupna dok se ne prenose promjene DNS-a)

Za klaster Cassandra scenariji prebacivanje treba uzeti u obzir ovise o dosljednost razine koji se koristi aplikacija, kao i broj replike koristi. Razine dosljednost i korištenje u Cassandra potražite u članku [Konfiguriranje podataka dosljednost] [ cassandra-consistency] i [Cassandra: čvorove koliko se je bila riječ da biste s kvorum?][cassandra-consistency-usage] Dostupnost podataka u Cassandra određuju razinu dosljednost koristi aplikacija i mehanizam za replikaciju. Replikacija u Cassandra potražite u članku [Replikacije podataka u NoSQL baze podataka objašnjenje][cassandra-replication].

## <a name="manageability-considerations"></a>Pitanja vezana uz mogućnost upravljanja

Kada ažurirate implementaciju sustava, ažurirajte određenu regiju u trenutku da biste smanjili izgledi globalni pogreške netočan konfiguraciju ili pogreške u aplikaciji.

Testirajte otpornost sustav pogreške. Evo nekih uobičajenih scenarija pogreška da biste testirali:

- Isključivanje VM instance.

- Resursi tlaka kao što su procesora i memorije.

- Prekid veze/odgode mreže.

- Neočekivano zatvoriti procesa.

- Istječe certifikata.

- Kao zamjenu za hardver kvarove.

- Isključivanje DNS servis kontrolera domena.

Mjerenje vremena oporavak i provjerite je li oni ispunjavaju s poslovnim potrebama. Testirajte kombinacije neuspjeh načine, kao i.

## <a name="next-steps"></a>Daljnji koraci

Ovaj niz sadrži filtriran na čisto oblaka implementacije. Scenariji za Enterprise često potrebno hibridnog mreži, veze lokalne mreže s Azure virtualne mreže. Da biste saznali kako izraditi takve hibridnog mreže, potražite u članku [implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN][hybrid-vpn].

<!-- Links -->

[azure-sla]: https://azure.microsoft.com/support/legal/sla/
[cassandra-in-azure]: https://academy.datastax.com/resources/deployment-guide-azure
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[cassandra-consistency-usage]: https://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-ports]: http://docs.datastax.com/en/latest-dse/datastax_enterprise/sec/secConfFirePort.html
[datastax]: https://www.datastax.com/products/datastax-enterprise
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssl-client-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLClientToNode_t.html
[ssl-node-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLNodeToNode_t.html
[tablediff]: https://msdn.microsoft.com/en-us/library/ms162843.aspx
[tm-configure-failover]: ../traffic-manager/traffic-manager-configure-failover-routing-method.md
[tm-monitoring]: ../traffic-manager/traffic-manager-monitoring.md
[tm-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[tm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/
[traffic-manager]: https://azure.microsoft.com/en-us/services/traffic-manager/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[vnet-to-vnet]: ../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[wsfc]: https://msdn.microsoft.com/en-us/library/hh270278.aspx
[0]: ./media/blueprints/compute-multi-dc-linux.png "Iznimno dostupnih mrežnih arhitektura za Azure N sloja aplikacije"
