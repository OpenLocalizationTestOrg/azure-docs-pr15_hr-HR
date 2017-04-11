<properties
   pageTitle="Ograničavanje u upravitelju resursa za servis tkanina klaster | Microsoft Azure"
   description="Saznajte kako konfigurirati Reguliranje navedene po tkanina klaster resursa Upravitelj servisa."
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


# <a name="throttling-the-behavior-of-the-service-fabric-cluster-resource-manager"></a>Ograničavanje ponašanje od servisa tkanina klaster upravitelj resursa
Čak i ako ste konfigurirali Voditelj resursa klaster pravilno, klaster možete dobiti nadziranje prekinuto. Na primjer tomu može biti istodobno čvor ili kvara domene pogreške – što će se dogoditi ako do kojih je došlo tijekom nadogradnje? Voditelj resursa će pokušati najbolje riješiti sve, ali u puta ovako trebali uzeti u obzir u backstop tako da klaster sam ima izgledi Stabiliziranje (čvorove koji će vratiti učinite mrežni uvjeti heal sami, implementiraju ispravnog bitova). Da biste lakše s takve situacijama, Voditelj resursa za servis tkanina klaster obuhvaćaju nekoliko Reguliranje. Napomena jesu li te Reguliranje prilično disruptive i obično ne bi trebalo koristiti osim ako je došlo neki oprezni matematičke gotovo oko količina paralelno rada koje je moguće izvršiti zapravo u klaster, kao što je često korištenih morate odgovoriti na te sortira od (ahem) neplanirano macroscopic ponovno konfiguriranje događaja (AKA: "Vrlo neispravni dana").

Općenito govoreći, preporučujemo da izbjegavanje vrlo neispravni dana do druge mogućnosti (kao što su uobičajeni kod ažuriranja i izbjegavanje počinje sa overscheduling klaster) umjesto ograničavanje svoj klaster da biste spriječili koristi resurse je prilikom da biste riješili problem sam). Na Reguliranje sadrže zadane vrijednosti koje ste naišli kroz sučelje bude dovoljne zadane vrijednosti, ali trebali biste vjerojatno pogledajte i namjestite na Očekivani stvarni Učitaj. Dok ne pretjerano constraining ili učitavanje klaster je najbolji način može odrediti da postoje slučajeva koje (dok ih riješiti) koju morate koristiti nekoliko Reguliranje na mjestu, čak i ako se to znači klaster će trajati dulje Stabiliziranje.

##<a name="configuring-the-throttles"></a>Konfiguriranje na Reguliranje
Reguliranje uvrštene po zadanom su:

-   GlobalMovementThrottleThreshold – to kontrole ukupan broj kretanja u skupine putem neko vrijeme (koje se definira kao GlobalMovementThrottleCountingInterval, vrijednost u sekundama)
-   MovementPerPartitionThrottleThreshold – to kontrole ukupan broj premještanja sve particije servisa putem neko vrijeme (u MovementPerPartitionThrottleCountingInterval, vrijednost u sekundama)

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Imajte na umu da u većini slučajeva smo vidjeli klijentima pomoću ove Reguliranje prošla jer su već u okruženju resursa ograničeno (kao što su propusnost ograničena mreže u pojedinačne čvorove ili diskova koji nisu najviše preduvjeti paralelno replike sastavlja koji su smještaju na njih) koje Predviđeni da je takav operacija ne uspije ili bio ipak sporo.  U tim situacijama korisnici su upoznati znati koje oni su potencijalno proširivanje vrijeme osvojio klaster dosegne stabilan stanju, uključujući znati koje su nije moguće završiti pokrenut na donjem cjelokupan pouzdanosti dok se oni su ograničio vrijeme.

## <a name="next-steps"></a>Daljnji koraci
- Da biste saznali kako Voditelj resursa klaster upravlja te salda Učitaj u klasteru, pogledajte članak na [za ujednačavanje opterećenja](service-fabric-cluster-resource-manager-balancing.md)
- Voditelj resursa klaster sadrži mnogo mogućnosti za s opisom klaster. Da biste saznali više o njima pogledajte ovaj članak na [s opisom servisa tkanina klaster](service-fabric-cluster-resource-manager-cluster-description.md)
