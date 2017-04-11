<properties
   pageTitle="Servis tkanina klaster Voditelj resursa: premještanje cijena | Microsoft Azure"
   description="Pregled premještanja trošak za servis tkanina servise"
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

# <a name="service-movement-cost-for-influencing-cluster-resource-manager-choices"></a>Premještanje trošak za određujući mogućnosti upravljanja resursima klaster
Važno faktor treba uzeti u obzir kada pokušavate određuju što promjene da biste klaster, a rezultat rješenje je ukupni trošak postizanje tog rješenja.

Premještanje servisa instanci ili replike troškove procesora vrijeme i mrežne propusnosti najmanje. Za s praćenjem stanja servisa i ga troškove količinu prostora na disku koje su vam potrebne da biste stvorili kopiju stanje prije isključivanja stare replike. Jasno bi želite minimizirati trošak rješenjem sastavu Voditelj resursa za Azure servisa tkanina klaster prema gore. Ali i ne želite da biste zanemarili rješenja koja će znatno poboljšati Dodjela resursa u klasteru.

Voditelj resursa klaster ima dva načina računanja troškove i ograničavanje, čak i kada je pokušava upravljanje klaster prema postavljenim ciljevima. Prvi je da prilikom upravljanja resursima klaster je planiranja novi raspored za klaster, broji svaki Premjesti koji bi. U slučaju jednostavne Ako nailazite na dva rješenja s o isti ukupni saldo (rezultat) na kraju pa snimite onaj s najmanje trošak (ukupan broj poteza).

Ovo vrlo dobro funkcionira. No kao i u slučaju zadane ili statične opterećenje je vjerojatno u sistemske složene sve premješta jednake. Neke su vjerojatno će biti puno više skupi.

## <a name="changing-a-replicas-move-cost-and-factors-to-consider"></a>Promjena na replike Premjesti trošak i čimbenici koje treba uzeti u obzir
Kao izvještaja o učitavanja (drugi značajku klaster Voditelj resursa), dajete servis način koja se sama izvješćivanja kako skup servis je da biste premjestili u određenom trenutku.

Kod:

```csharp
this.ServicePartition.ReportMoveCost(MoveCost.Medium);
```

MoveCost ima četiri razine: nula, najniža, Srednje i Visoko. To su u međusobno povezani, osim nule. Nula znači premještanje kopiju je besplatna i trebali biste smanjuju raspoloživu rezultat rješenja. Postavljanje vaš trošak Premjesti na visoku je *ne* jamči da se neće pomaknuti replike uz samo da ga neće se premjestiti osim ako postoji dobar razlog da.

![Premještanje trošak kao u odabir replike za premještanje][Image1]

MoveCost olakšava pronalaženje rješenja koja uzrokuju najmanje prekidu cjelokupan i najkorisnije da biste postigli dok još uvijek stizati u ekvivalentne saldo. Na servis notion troška može biti odnosu mnogo toga. Najčešći faktori za izračunavanje vaš trošak Premjesti su:

- Iznos savezne države ili podataka s servis za premještanje.
- Trošak prekidanje veze sa klijenata. Premještanje primarni replike trošak je obično veća od trošak premještanje sekundarnog replike.
- Trošak prekida in-flight postupak. Neki postupci podataka pohraniti razinu ili operacije obavljene kao odgovor na poziv klijent su skup. Nakon što u određenom trenutku ne želite da ako ne morate ih isključiti. Pa trajanja operacije koje prijeđe gore trošak da biste smanjili vjerojatnost koja replike servisa ili instancu će se premjestiti. Kada završi postupak, postavite ga natrag na obični.

## <a name="next-steps"></a>Daljnji koraci
- Upravitelj resursa za servis tkanina klaster koristi metriku za upravljanje potrošnje i kapacitet u klasteru. Da biste saznali više o metriku i konfiguriranju ih, pogledajte [potrošnje resursa za upravljanje i opterećenje u servis tkanina s metriku](service-fabric-cluster-resource-manager-metrics.md).
- Da biste saznali kako Voditelj resursa klaster upravlja i salda Učitaj u klasteru, pogledajte [opterećenja svoj klaster tkanina servisa](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
