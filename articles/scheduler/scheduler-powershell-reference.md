<properties
 pageTitle="Raspored PowerShell cmdleti za pregled"
 description="Raspored PowerShell cmdleti za pregled"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-powershell-cmdlets-reference"></a>Raspored PowerShell cmdleti za pregled

U sljedećoj su tablici opisuju i veze na stranici referenca svake od glavnih cmdleta u rasporedu Azure.

Da biste instalirali Azure PowerShell i povezati s pretplatom Azure, Saznajte [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md). 

Dodatne informacije o [cmdletima Azure upravljanja resursima](https://msdn.microsoft.com/library/mt125356\(v=azure.200\).aspx)potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md).

|Cmdlet|Cmdlet opis|
|---|---|
[Onemogući AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490133\(v=azure.200\).aspx) |Onemogućuje zbirku posao. 
[Omogući AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490135\(v=azure.200\).aspx) |Omogućuje zbirku posao.
[Get-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490125\(v=azure.200\).aspx) |Dohvaća raspored zadataka.
[Get-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490132\(v=azure.200\).aspx) |Uzima zadatka zbirke.
[Get-AzureRmSchedulerJobHistory](https://msdn.microsoft.com/library/mt490126\(v=azure.200\).aspx) |Povijest zadatka dobije.
[Novi AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490136\(v=azure.200\).aspx) |Stvara zadatak programa HTTP.
[Novi AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490141\(v=azure.200\).aspx) |Stvara zadatak zbirke.
[Novi AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490134\(v=azure.200\).aspx) |Stvara zadatak reda čekanja za bus servisa.
[Novi AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490142\(v=azure.200\).aspx) |Stvara zadatak temu za bus servisa.
[Novi AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490127\(v=azure.200\).aspx) |Stvara zadatak reda čekanja za pohranu. 
[Uklanjanje AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490140\(v=azure.200\).aspx) |Uklanja posao raspored.  
[Uklanjanje AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490131\(v=azure.200\).aspx) |Uklanja zbirku posao. 
[Postavljanje AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490130\(v=azure.200\).aspx) |Mijenja raspored HTTP posao.
[Postavljanje AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490129\(v=azure.200\).aspx) |Mijenja zbirku posao. 
[Postavljanje AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490143\(v=azure.200\).aspx) |Mijenja posao reda čekanja za bus servisa.  
[Postavljanje AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490137\(v=azure.200\).aspx) |Mijenja temu posao bus servisa. 
[Postavljanje AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490128\(v=azure.200\).aspx) |Mijenja posao reda čekanja za pohranu.   

Detaljnije informacije, možete pokrenuti bilo koji od sljedeće Cmdlete: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Vidi također


 [Što je raspored?](scheduler-intro.md)

 [Azure koncepata raspored, terminologija i entitet hijerarhije](scheduler-concepts-terms.md)

 [Početak rada s raspored na portalu za Azure](scheduler-get-started-portal.md)

 [Tarife i naplata u rasporedu Azure](scheduler-plans-billing.md)

 [Referenca za Azure raspored REST API-JA](https://msdn.microsoft.com/library/mt629143)

 [Azure visoku dostupnost raspored i pouzdanosti](scheduler-high-availability-reliability.md)

 [Azure ograničenja raspored, zadanih vrijednosti i šifre pogreške](scheduler-limits-defaults-errors.md)

 [Azure izlaznu provjeru autentičnosti rasporeda](scheduler-outbound-authentication.md)
