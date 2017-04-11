<properties
   pageTitle="Azure obradu Zapisivanje dijagnostičkih podataka | Microsoft Azure"
   description="Snimanje i analizirati dijagnostičkog zapisnika događaja za obradu Azure račun resurse kao što su grupe i zadatke."
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="10/12/2016"
   ms.author="marsma"/>

# <a name="azure-batch-diagnostic-logging"></a>Azure obradu Zapisivanje dijagnostičkih podataka

Kao i kod mnoge Azure servise, servis za obradu emits događaje zapisnika za određene resurse tijekom života resursa. Omogućavanje obrade Azure dijagnostičke zapisnike zapisa događajima za resurse kao što su grupe i zadaci i zatim upotrijebite u zapisnicima dijagnostičkih procjenu te praćenje. Stvaranje događaje kao što je skup, Izbriši grupu, Početak zadatka, zadatak dovršen i drugima uključeni u obradu dijagnostičke zapisnike.

>[AZURE.NOTE] U ovom se članku govori o zapisivanje događaja za obradu račun resurse sami, ne posla i zadataka izlazne podatke. Informacije o pohrani za izlazne podatke zadataka i zadataka, potražite u članku [Izlaz zadržava grupe za Azure posao i zadatak](batch-task-output.md).

## <a name="prerequisites"></a>Preduvjeti

* [Račun za Azure grupe](batch-account-create-portal.md)

* [Račun za Azure prostora za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account)

  Za obradu dijagnostičke zapisnike i dalje pojavljuje, morate stvoriti račun za pohranu Azure gdje Azure će sadržavati zapisnike. Odredite taj račun za pohranu kada [uključite Zapisivanje dijagnostičkih podataka](#enable-diagnostic-logging) za vaš račun grupe. Račun za pohranu navedete Kada omogućite prikupljanje zapisnika nije isti kao račun povezane prostora za pohranu naziva u člancima [paketa aplikacije](batch-application-packages.md) i [postojanost izlaz zadatka](batch-task-output.md) .

  >[AZURE.WARNING] Prikazuje vam se **naplatiti** za podatke pohranjene u račun za Azure prostora za pohranu. To obuhvaća dijagnostičke zapisnike koji se spominju u ovom članku. Imajte na umu pri dizajniranju [pravila zadržavanja zapisnika](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).

## <a name="enable-diagnostic-logging"></a>Uključite Zapisivanje dijagnostičkih podataka

Zapisivanje dijagnostičkih podataka nije omogućen po zadanom za vaš račun grupe. Eksplicitno omogućite Zapisivanje dijagnostičkih podataka za svaki račun grupe kojima želite nadzirati:

[Kako omogućiti skup dijagnostičkih zapisnika](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

Preporučujemo da pročitate cijeli članak [Pregled Azure dijagnostičke zapisnike](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) da razumijete ne samo kako omogućiti zapisivanje, ali kategorije zapisnika podržava različite servise za Azure. Na primjer, grupe za Azure trenutno podržava jednoj kategoriji zapisnika: **Zapisnici servisa**.

## <a name="service-logs"></a>Servis zapisnika

Azure zapisnika servisa za obradu sadrže događaje čuje servis grupe za Azure tijekom života resursa grupe kao što je skup ili zadatka. Svaki događaj čuje obrade tako da se pohranjuju u navedeni račun za pohranu u JSON OSNOVNI oblik. Ako, na primjer, to je tijelo na ogledne **skup stvoriti događaj**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Svaki događaj tijelo nalazi se u datoteku .json u navedeni račun za Azure prostora za pohranu. Ako želite da biste izravno pristupili zapisnike, možda želite pregledajte [sheme dijagnostičke zapisnike na računu za pohranu](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

## <a name="service-log-events"></a>Servis zapisnika događaja

Servis za obradu trenutno emits sljedeće servis zapisnika događaja. Ovaj popis možda neće biti iscrpan, jer dodatne događaji su dodane nakon zadnjeg ažuriranja u ovom se članku.

| **Servis zapisnika događaja** |
| ------------------ |
| [Stvaranje grupe aplikacija][pool_create] |
| [Početak Izbriši grupu][pool_delete_start] |
| [Brisanje grupe aplikacija dovršeno][pool_delete_complete] |
| [Početak skup promjenu veličine][pool_resize_start] |
| [Promjena veličine skup dovršeno][pool_resize_complete] |
| [Početak zadatka][task_start] |
| [Dovršavanje zadatka][task_complete] |
| [Nije uspjelo zadatka][task_fail] |

## <a name="next-steps"></a>Daljnji koraci

Osim pohrana dijagnostičkog zapisnika događaja u račun za Azure prostora za pohranu, možete strujanje grupe servisa zapisnika događaja [Azure događaj koncentrator](../event-hubs/event-hubs-what-is-event-hubs.md), i pošaljite im [Azure prijava analitiku](../log-analytics/log-analytics-overview.md).

* [Strujanje Azure dijagnostičke zapisnike s koncentratorima događaja](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)

  Strujanje obradu dijagnostičkih događaja iznimno skalabilni podatkovnog servisa ingress koncentratora za događaj. Događaj koncentratora možete ingest milijune događaje u sekundi koje možete, a zatim Pretvorba i spremanje pomoću bilo kojeg davatelja usluga analize u stvarnom vremenu.

* [Analiza Azure dijagnostičke zapisnike pomoću zapisnika Analytics](../log-analytics/log-analytics-azure-storage-json.md)

  Slanje dijagnostičkih zapisnika zapisnika analize gdje možete analizirati ih na portalu operacije upravljanja paket (OMS) ili ih izvesti za analizu u dodatku Power BI ili Excel.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
