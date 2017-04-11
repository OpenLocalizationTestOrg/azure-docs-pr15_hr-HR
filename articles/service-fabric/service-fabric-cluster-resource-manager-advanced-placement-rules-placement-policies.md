<properties
   pageTitle="Servis tkanina klaster Voditelj resursa – položaj pravila | Microsoft Azure"
   description="Pregled dodatnih položaj te pravila za servis tkanina servise"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="placement-policies-for-service-fabric-services"></a>Položaj pravilima za usluge tkanina servisa
Postoji mnogo različitih dodatna pravila koja se može završiti caring o ako je rastezati svoj klaster tkanina servisa preko geografske udaljenosti, izgovorite više podatkovnim centrima ili Azure regije, ili ako je vaše okruženje obuhvaća više područja Geopolitički kontrole (ili neke druge slova koji imate pravna ili pravila ograničenja koje vas zanimaju ili udaljenost uključene imati utjecaj stvarni performanse/Latencija). Većina tih nije moguće konfigurirati putem čvor svojstva i položaj ograničenja, ali neke su složenije. Da biste stvari jednostavniji dajemo te dodatne commmands. Baš kao i s drugim ograničenjima položaj položaj pravila možete konfigurirati na temelju instanci po – pod nazivom servisa.

## <a name="specifying-invalid-domains"></a>Određivanje koji nisu valjani domene
Položaj pravila InvalidDomain omogućuje vam da navedete da određene domene kvara nije valjana za ovog posla. Ovo pravilo osigurava da servis za određeni nikad ne pokreće se u određeno područje, na primjer razloga Geopolitički ili korporativnih pravila. Više domena koji nisu valjani moguće je navesti putem zasebna pravila.

![Primjer koji nisu valjani domene][Image1]

Kod:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Određivanje potrebna domene
Položaj pravila potrebna domene zahtijeva da se sve instance bez praćenja stanja servisa za servis ili s praćenjem stanja replike koje su prisutne u navedenu domenu. Više domena potrebna možete navesti putem zasebna pravila.

![Primjer potrebna domene][Image2]

Kod:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas"></a>Određivanje željene domene za primarni replike
Preferirani primarne domene je zanimljiv kontrola, jer dopušta odabir domene kvara u kojem primarni valja smjestiti ako je moguće da biste to učinili. Kada je sve dobar primarni će završe u ovoj domeni. Trebali biste domene ili nije uspjelo primarni replike ili isključite iz nekog razloga primarni će se premjestiti na nekom drugom mjestu. Ako to mjesto nije na preferiranom domene, zatim kada je to moguće Voditelj resursa klaster će ga odvojiti Preferirani domenu. Prirodno tu postavku samo sukladno s praćenjem stanja servisa. Ovo pravilo je najkorisnije u klastere koje su rastezati na Azure područja ili više podatkovnim centrima. U tim situacijama koristite sva mjesta za zalihosti, ali biste radije da primarni replike smjestiti na određene mjestu omogućili donjem Latencija za operacije koje otvorite primarni (piše i i po zadanom sve čitanja su poslužena tako da primarni).

![Preferirani primarne domene i prebacivanje][Image3]

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replicas-to-be-distributed-among-all-domains-and-disallowing-packing"></a>Zahtijevanje replike koje će biti podijeljene između svih domena i onemogućavanjem spremanje
Drugi pravila možete odrediti je obavezan replike koje uvijek treba raspodijeliti među domene dostupan kvara. To će se dogoditi po zadanom u većini slučajeva u kojima je dobar klaster, no postoje degenerate slučajevima gdje replike za dani particija može završiti privremeno zapakirane u jednom kvara ili nadogradnje domene. Na primjer, recimo da koji iako klaster ima 9 čvorove 3 kvara domene (0, 1 i 2) i na servisu sadrži 3 replike, čvorove koje su koristi za te replike u kvara domene 1 i 2 nije prema dolje i zbog problema s kapaciteta ništa ostale čvorove te domene nisu valjani. Ako su tkanina servisa da biste sastavili zamjena za te replike, Voditelj resursa klaster promijenile za njihovo kvara domene 0, ali koji stvara situaciji gdje je u tijeku bi ograničenje kvara domene. Također se povećava vjerojatnost da će se skup cijeli replike mogu biti izgubljeni (Ako se FD 0 se permananently prekinula). (Dodatne informacije o ograničenjima i ograničenja Prioriteti Općenito govoreći, pogledajte [članak](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities) )

Ako ste ikad vidjeti upozorenje o stanju kao što su "raspoređivača opterećenja otkrio povrede ograničenja za replike: tkanina: /<some service name> sekundarne particija <some partition ID> je Narušavanje ograničenje: FaultDomain" što ste pritisnete ovaj uvjet ili nešto sviđa mi se. Obično su tranzitne ovih situacija (čvorove ne ostaju dolje dugo, ili ako služe i moramo sastavljanje postoji zamjena ostale čvorove u odgovarajuće kvara domenama valjanih), no postoje neke radnih opterećenja koji bi radije trgovanje dostupnost rizik od gubitka njihove replike. Ne možemo to možete učiniti tako da navedete pravilnik "RequireDomainDistribution", koji će jamči da nema dvije replike s iste particije ikad dopušteno u istu domenu kvara ili nadogradnja.

Neke radnih opterećenja radije imali cilj broj replike (kopije stanje) uopće vremena (betting protiv neuspjeha ukupni domene i zna da ih možete obično oporaviti lokalne stanje), dok drugi radije osvojio na nedostupnost starije od rizika opasnosti ispravnost i dataloss. Budući da se većina radnog pokreću s više od 3 replike, zadana je mogućnost ne zahtijevaju raspodjele domene i omogućuju opterećenja i prebacivanje obično rukovati slučajeva čak i ako se to znači da privremeno domene ima više replike zapakirane u nju.

Kod:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Sada je bio moguće koristiti ove konfiguracije za servise u skupini koji je ne rastezati geografski? Sigurni nije! Ali nema sjajno razlog previše – osobito konfiguracije potrebna, koji nisu valjani i željene domene moraju se izbjegavati osim ako ne koristite zapravo geografski Rastegnutu klaster – nema smisla sve da biste prisilno navedeni radno opterećenje da biste pokrenuli u jednom bicikle ili da biste radije neki dio vaše lokalne klaster preko drugog, osim ako postoji različite vrste hardver ili radno opterećenje segmente događa , i tim slučajevima može riješiti putem normalni položaj ograničenja.

## <a name="next-steps"></a>Daljnji koraci
- Za dodatne informacije o drugim mogućnostima dostupnima za konfiguriranje usluge odjavu temu na druge konfiguracije Voditelj resursa klaster dostupne [Dodatne informacije o konfiguriranju Services](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
