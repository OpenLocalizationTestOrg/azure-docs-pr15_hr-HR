<properties
   pageTitle="Implementacija virtualne aparata u visoke dostupnosti | Microsoft Azure"
   description="Kako implementirati mreže virtualne aparata u visoke dostupnosti."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="telmos"/>

# <a name="deploying-virtual-appliances-in-high-availability"></a>Implementacija virtualne aparata u visoke dostupnosti

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

U ovom se članku navode skup postupci za uvođenje mreže virtualne aparata (NVAs) vrlo dostupna način. Prije nastavka, provjerite je li znate kako [korisnički definirane usmjerava (UDR)] [ udr-overview] i [Učitavanje opterećenja] [ lb-overview] rad u Azure.

Možete koristiti različite NVAs dostupne u Azure marketplace da biste proširili funkcionalnost Azure na isti način kao i koristite aparata u vaše lokalne podatkovnog centra. Sljedeća slika prikazuje uzorak implementacije [jedan NVA] [ nva-scenario] kao vatrozida potražite. 

![[0]][0]

Iako prethodne implementacije radi, predstavlja jednu točku nije uspjelo. Virtualna uređaj ne uspije, bez promet teče. Da biste riješili taj problem, morate koristiti više NVAs. Međutim, koji zahtijeva druge postavke i resursima koja će se koristiti ovisno o svojim potrebama.

Možete koristiti jedan od sljedeća rješenja za implementaciju iznimno dostupne NVA okruženje.

|Rješenja|Prednosti|Razmatranja|
|---|---|---|
|Ingress s virtualne aparata sloja 7|Sve čvorove su aktivne|Potreban je NVA koju mogu prekinuti veze i koristiti SNAT<br/>Potreban je zaseban skup NVAs za prometa koji dolaze iz s Internetom i Azure<br/>Može se koristiti samo za promet potječu izvan Azure|
|Ingress izlazne s virtualne aparata sloja 7|Sve čvorove su aktivne<br/>Možete učiniti promet potječe servisu Azure |Potreban je NVA koju mogu prekinuti veze i koristiti SNAT<br/>Potreban je zaseban skup NVAs za prometa koji dolaze iz s Internetom i Azure|
|Promjena TOČAKA UDR|Jedan skup NVAs za sve promet<br/>Možete rukovati sav promet (bez ograničenja pravila priključka)|Aktivni pasivni<br/>Potreban je postupak za prebacivanje|

## <a name="ingress-with-layer-7-virtual-appliances"></a>Ingress s virtualne aparata sloja 7
Možete koristiti skup NVAs iza Azure opterećenja omogućuje povezivanje Azure radnih opterećenja iza malom poslužiteljsko priključaka (kao što su HTTP i HTTPS). Na sljedećoj slici ističe kako unijeti visoke dostupnosti u ovom scenariju na razini NVA.

![[1]][1]

U ovom scenariju potražite virtualne mreže koristi morate prekinuti sve veze i prenesite podmreže radno opterećenje. Radno opterećenje virtualnim strojevima (VMs) odgovoriti na NVA primio zahtjev iz te promet tokova bez problema. 

## <a name="ingress-egress-with-layer-7-virtual-appliances"></a>Ingress izlazne s virtualne aparata sloja 7
Možete proširiti prethodni arhitektura i dodajte drugi skup NVAs učiniti promet potječu Azure za rukovanje NVAs, kao što je prikazano na sljedećoj slici:

![[2]][2]

U ovom scenariju sav promet potječu u Azure usmjeravanja za interne raspoređivača opterećenja koji distribuira učitavanja između drugačiji skup NVAs. Ove NVAs usmjeriti promet na Internetu pomoću njihove pojedinačne javne IP adrese. 

## <a name="pip-udr-switch"></a>Promjena TOČAKA UDR
Možete izbjegavajte stvaranje više NVA snop pomoću dva NVAs u aktivni pasivni način rada. U ovom scenariju, možete se prebacivati javnu IP adresa (TOČAKA) i korisnički definirana usmjerava (UDRs) ako čvor aktivni prekine.  

![[3]][3]

Ovaj scenarij je slična jedan scenarij NVA. Jedina razlika je da TOČAKA i UDRs moraju promijeniti da biste se prebacili promet između na NVAs. Te promjene možete ručno riješiti ili ih također možete automatizirati. Da biste automatizirali, možete implementirati aplikaciju za oba NVAs kojom se traže stanja čvor active. Kada je aktivna čvor isključen, aplikacija možete promijeniti TOČAKA i UDRs da biste se povezali pasivni čvor.

Mogući implementacija rješenja jest korištenje [ZooKeeper] [ zookeeper] daemon na NVAs učiniti izborno Vodeća crta (pri odlučivanju koji čvor čvor aktivni). Kada se taj prostor Voditelj poziva za Azure REST API-JA uklanjanje na TOČAKA za čvor nije uspjelo i je priložite vodeću crtu. Trebali biste promijeniti i UDRs tako da pokazuje na novu Voditelj privatne IP adrese.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [implementirati DMZ između Azure i podatkovnim centrom vaše lokalne] [ dmz-on-prem] pomoću NVAs sloja 7.
- Saznajte kako [implementirati DMZ između Azure i Interneta] [ dmz-internet] pomoću NVAs sloja 7.

<!-- links -->
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[lb-overview]: ../load-balancer/load-balancer-overview.md
[zookeeper]: https://zookeeper.apache.org/
[nva-scenario]: ../virtual-network/virtual-network-scenario-udr-gw-nva.md
[dmz-on-prem]: guidance-iaas-ra-secure-vnet-hybrid.md
[dmz-internet]: guidance-iaas-ra-secure-vnet-dmz.md

<!-- images -->
[0]: ./media/guidance-nva-ha/single-nva.png "Jedan NVA arhitekture"
[1]: ./media/guidance-nva-ha/l7-ingress.png "Ingress sloja 7"
[2]: ./media/guidance-nva-ha/l7-ingress-egress.png "Slojeva 7 ingress i izlazne"
[3]: ./media/guidance-nva-ha/active-passive.png "Aktivni pasivni klaster"