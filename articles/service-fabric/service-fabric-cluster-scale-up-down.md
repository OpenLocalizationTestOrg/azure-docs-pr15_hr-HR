<properties
   pageTitle="Skaliranje servisa tkanina klaster ili smanjili | Microsoft Azure"
   description="Promjena veličine servisa tkanina klaster ili smanjili tako da odgovara zahtjev postavljanjem automatsko skaliranje pravila za svaki skup za čvor vrsta/VM mjerilo. Dodavanje i uklanjanje čvorove servisa tkanina klaster"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Promjena veličine servisa tkanina klaster ili smanjili pomoću pravila za automatsko skaliranje

Skupovi skaliranje virtualnog računala su programa Azure računalnim resursa koje možete koristiti i upravljanje skupovima virtualnim strojevima u skupu. Svaka vrsta čvora koji je definiran na servis tkanina postavljena kao zasebna skup skaliranje VM. Svaka vrsta čvor pa možete skalirana u ili izvan neovisno o imaju različite skupove otvorene priključke i može imati kapacitet različitih metriku. Saznajte više o tome u dokumentu [nodetypes tkanina servisa](service-fabric-cluster-nodetypes.md) . Budući da tkanina servisa vrste čvor u svoj Klaster se sastoji od VM skaliranje postavlja pri pozadinski, morate postavljanje pravila za automatsko skaliranje za svaki skup za čvor vrsta/VM mjerilo.

>[AZURE.NOTE] Vaša pretplata mora imati dovoljno jezgri da biste dodali novi VMs koji čine ovoj grupi. Trenutno nema Provjera valjanosti modela, da biste dobili vrijeme pogreške implementaciju, ako je bilo koji od ograničenja pronalaženja objekata.

## <a name="choose-the-node-typevm-scale-set-to-scale"></a>Odaberite čvor skale vrsta/VM postavljen na skaliranje

Trenutno se ne može odrediti pravila za automatsko skaliranje za VM skaliranje skupove pomoću portala, pa nam pomoću Azure PowerShell (1.0 +) čvor vrste popisa, a zatim ih dodati pravila za automatsko skaliranje.

Da biste dobili popis skupa VM skaliranje koji čine svoj klaster, pokrenite sljedeće Cmdlete:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <VM Scale Set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevm-scale-set"></a>Postavljanje pravila za automatsko skaliranje za skup čvor vrsta/VM skala

Ako svoj klaster ima više vrsta čvor, ponovite za svaku skaliranje vrste/VM čvor postavlja da biste skalirali (ili smanjili). Uzimaju u obzir broj čvorove da morate imati prije no što postavite automatsko skaliranje. Najmanji broj čvorove koje morate imati za vrstu primarni čvor pokreću razine pouzdanosti koju ste odabrali. Dodatne informacije o [pouzdanosti razine](service-fabric-cluster-capacity.md).

>[AZURE.NOTE]  Skaliranje dolje čvor primarni vrstu manja od minimalne broj stvaranjem nestabilan klaster ili Premjesti. To može dovesti do gubitka podataka za aplikacije i servise sustava.

Trenutno opterećenje koje aplikacija može biti izvješća servisa tkanina ne pokreću značajka Automatsko skaliranje. Tako da trenutno na performanse isključivo pokreću se automatsko-skaliranje mjerača koji su čuje svaki na VM skaliranje skup instance.  

Slijedite ove upute [Da biste postavili automatsko skaliranje za svaki skup VM mjerilo](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

>[AZURE.NOTE] Skalu dolje scenarij, osim ako vaša vrsta čvor ima razinu rok trajanja Zlatna ili Srebrno morate nazvati [Ukloni ServiceFabricNodeState cmdlet](https://msdn.microsoft.com/library/azure/mt125993.aspx) odgovarajuće čvor naziva.

## <a name="manually-add-vms-to-a-node-typevm-scale-set"></a>Ručno dodavanje VMs skup čvora vrsta/VM skala

Pomoću uputa uzorka/u [galeriji predložaka za brzi početak rada](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) da biste promijenili broj VMs u svakom Nodetype. 

>[AZURE.NOTE] Dodavanje od VMs traje, tako da ne očekujete dodaci se trenutačno. Stoga namjeravate dodati kapacitet dobro u vremenu, da biste omogućili radi veće od 10 minuta prije kapaciteta VM dostupna je za na replike / instance koja će se postaviti za servis.

## <a name="manually-remove-vms-from-the-primary-node-typevm-scale-set"></a>Ručno uklanjanje VMs iz skupa primarni čvor vrsta/VM skala

>[AZURE.NOTE] Servise sustava tkanina servisa pokrenite Vrsta primarnog čvora u svoj klaster. Tako da nikada ne treba isključiti ili skalirali broj instanci u tom čvor vrste manji od koje u sloju pouzdanosti preuzimanje. Pogledajte [detalje o pouzdanosti razine ovdje](service-fabric-cluster-capacity.md). 

Morate izvršiti sljedeće korake jedan VM instancu odjednom. Time se omogućuje za servise sustava (i s praćenjem stanja servisa) da biste isključiti bez poteškoća na instancu VM uklanjate i nove replike stvorene na ostale čvorove.

1. Pokretanje [Onemogući ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) s svrhu "RemoveNode" da biste onemogućili čvor namjeravate uklonite (najveće instancu u toj vrsti čvora).

2. Pokrenite [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) da biste bili sigurni da se čvor uistinu prebačena je onemogućen. Ako nije, pričekajte da se čvor je onemogućen. Nije moguće hurry ovaj korak.

2. Pomoću uputa uzorka/u [galeriji predložaka za brzi početak rada](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) da biste promijenili broj VMs jedan u tom Nodetype. Instancu uklonjena je najveće VM instance. 

3. Ponovite korake od 1 do 3 po potrebi, ali nikad ne skalirali broj instanci u primarni čvor vrstama manje od što sloju pouzdanosti preuzimanje. Pogledajte [detalje o pouzdanosti razine ovdje](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevm-scale-set"></a>Ručno uklanjanje VMs iz skupa skaliranje vrsta/VM-primarni čvor

>[AZURE.NOTE] S praćenjem stanja servisa, potrebno vam je određeni broj čvorove do uvijek biti održavanje dostupnosti i očuvanje stanje servisa. Pri na vrlo minimalne, potreban vam je broj čvorove jednako skup broj replike cilj particija/servis. 

Potreban vam je izvršavanjem sljedeće korake jedan VM instancu odjednom. Time se omogućuje za servise sustava (i s praćenjem stanja servisa) isključiti bez poteškoća na instancu VM uklanjate i nove replike stvoriti drugo mjesto.

1. Pokretanje [Onemogući ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) s svrhu "RemoveNode" da biste onemogućili čvor namjeravate uklonite (najveće instancu u toj vrsti čvora).

2. Pokrenite [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) da biste bili sigurni da se čvor uistinu prebačena je onemogućen. Ako ne želite Pričekajte dok je onemogućen čvor. Nije moguće hurry ovaj korak.

2. Pomoću uputa uzorka/u [galeriji predložaka za brzi početak rada](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) da biste promijenili broj VMs jedan u tom Nodetype. Uklonit će se sada najveće VM instance. 

3. Ponovite korake od 1 do 3 po potrebi, ali nikad ne skalirali broj instanci u primarni čvor vrstama manje od što sloju pouzdanosti preuzimanje. Pogledajte [detalje o pouzdanosti razine ovdje](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Ponašanja možda pridržavajte u programu Explorer tkanina servisa

Kada proširenja klaster tkanina Explorer servisa odražavaju broj čvorove (VM skaliranje postavljene instance) koje su dio klaster.  Međutim, kada promijenite li klaster dolje koje prikazat će instancu uklonjene čvor/VM prikazuju u stanju koje dobro, osim ako poziva [Ukloni ServiceFabricNodeState cmd](https://msdn.microsoft.com/library/mt125993.aspx) odgovarajuće čvor naziva.   

Slijedi objašnjenje za taj problem.

Čvorovi navedene u pregledniku tkanina servisa su odraz na servis tkanina servisa sustava (FM posebno) zna o broju čvorove klaster imali/sadrži. Kada skaliranje skale VM skup prema dolje, na VM izbrisan, ali FM sistemski servis je i dalje misli da čvor (koje je pridruženo VM izbrisanog) će vratiti. Pa Explorer tkanina servisa i dalje da biste prikazali taj čvor (iako stanja možda se pogreška "ili" Nepoznato).

Da biste bili sigurni da čvor nestaje kada je uklonjena s VM, imate dvije mogućnosti:

1) Odaberite razinu rok trajanja Zlatna ili Srebrno (dostupno uskoro) vrsta čvor svoj klaster, koji omogućuje integraciju sa servisom infrastrukture. Koje će zatim automatski uklanjanje čvorove naš stanje servisa (FM) sustava kada skaliranje prema dolje.
Pogledajte [detalje o ovdje razine rok trajanja](service-fabric-cluster-capacity.md)

2) Kada instancu VM sadrži je neproporcionalno prema dolje, koje morate nazvati [cmdlet Ukloni ServiceFabricNodeState](https://msdn.microsoft.com/library/mt125993.aspx).

>[AZURE.NOTE] Servis tkanina klastere zahtijevaju određenog broja čvorove se prema gore u vrijeme da biste zadržali dostupnosti i očuvanje stanje - se nazivaju "održavanje kvorum". Dakle, je obično sigurno isključiti svim računalima klaster osim u slučaju da najprije obavite [cjelovite sigurnosne kopije stanje](service-fabric-reliable-services-backup-restore.md).

## <a name="next-steps"></a>Daljnji koraci
Pročitajte sljedeće da biste Informirajte se o planiranja kapaciteta klaster nadogradnje klaster te particija servisi:

- [Planiranje kapaciteta vaše klaster](service-fabric-cluster-capacity.md)
- [Nadograđuje klaster](service-fabric-cluster-upgrade.md)
- [Particija s praćenjem stanja servisa za maksimalno skaliranje](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
