<properties
   pageTitle="Prikupljanje zapisnika korištenjem dijagnostike Azure Linux | Microsoft Azure"
   description="U ovom se članku opisuje kako postaviti Azure Dijagnostika prikupljanje zapisnika iz servisa tkanina Linux klaster izvodi u Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="subramar"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Prikupljanje zapisnika korištenjem dijagnostike Azure

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Kada koristite programa tkanina servisa Azure klaster, je dobro prikupiti zapisnike iz sve čvorove na središnjem mjestu. Imate zapisnike središnjem mjestu olakšava za analizu i otklanjanje poteškoća, hoće li se ona nalazi u servisa, aplikacije ili klaster sam. Jedan da biste prenijeli i prikupljanje zapisnika tako da koriste nastavak Azure dijagnostiku koja prenosi zapisnika Azure prostora za pohranu. Možete pročitati događaje iz prostora za pohranu i potvrdite proizvoda kao što su [Elastic pretraživanje](service-fabric-diagnostic-how-to-use-elasticsearch.md) ili drugo rješenje zapisnika analize.

## <a name="log-sources-that-you-might-want-to-collect"></a>Zapisnik izvori koje želite prikupiti
- **Servis tkanina zapisnika**: čuje iz platformu putem [LTTng](http://lttng.org) i prenijeti na račun servisa za pohranu. Zapisnici mogu biti radu događaji ili runtime događaji koje emits platforme. Ti zapisnici pohranjuju se na mjesto koje određuje manifest klaster. (Da biste dobili pojedinosti o računu za pohranu, traženje oznaka **AzureTableWinFabETWQueryable** i potražite **StoreConnectionString**).
- **Događaji aplikacije**: čuje iz koda na servisu. Možete koristiti zapisivanje rješenja koja se ispisuje temeljenom na tekstu zapisničke datoteke – na primjer, LTTng. Dodatne informacije potražite u dokumentaciji LTTng na svoju aplikaciju za praćenje.  


## <a name="deploy-the-diagnostics-extension"></a>Implementacija proširenje dijagnostiku
Prvi korak u prikupljanje zapisnika je za implementaciju proširenje Dijagnostika za svaku od VMs u skupini tkanina servisa. Proširenje Dijagnostika prikuplja zapisnika na svakom VM i prenosi ih na račun za pohranu koji navedete. Koraci razlikuju se ovisno o tome koristite li Azure portala i upravljanja resursima Azure.

Da biste implementirati za nastavak Dijagnostika VMs u klasteru kao dio stvaranja klaster, postavite **Dijagnostika** **na**. Kada stvorite klaster, nije moguće promijeniti ovu postavku pomoću portala za.

Nakon toga konfigurirati Dijagnostika Azure na Linux (LAD) za prikupljanje datoteka i njihovo stavljanje na račun za pohranu. Ovaj postupak je objašnjeno kao scenarij 3 ("prijenos datoteka zapisnika") u članku [Korištenje LAD i praćenje dijagnosticiranje Linux VMs](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md). Slijedite ovaj postupak dobiti pristup da biste na kašnjenja. Visualizer po izboru možete prenijeti na kašnjenja.

Proširenje Dijagnostika može uvesti i pomoću upravitelja resursa Azure. Postupak je sličan za Windows i Linux i navedenih za klastere Windows u [uputama za prikupljanje zapisnika s Dijagnostika Azure](service-fabric-diagnostics-how-to-setup-wad.md).

Paket za upravljanje operacije, možete koristiti i kako je opisano u [Operacije upravljanja paket zapisnika analize s Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

Nakon što završite tu konfiguraciju, LAD agent nadzire navedeni zapisničke datoteke. Kad god se novi redak dodaju se u datoteku, stvara unos syslog koji se šalju prostora za pohranu koji ste naveli.


## <a name="next-steps"></a>Daljnji koraci
Da biste shvatili detaljno koji su se događaji treba provjeriti pri otklanjanju poteškoća, potražite u [dokumentaciji LTTng](http://lttng.org/docs) i [Pomoću LAD](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md).
