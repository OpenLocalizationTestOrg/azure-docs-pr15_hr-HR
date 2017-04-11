<properties
   pageTitle="Stvaranje spremnika servisa oblaka sa servisom PowerShell | Microsoft Azure"
   description="U ovom se članku objašnjava kako stvaranje spremnika servisa oblaka sa servisom PowerShell. Spremnik glavno računalo web- a tempiranja uloge."
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="cawa"/>

# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Korištenje naredbe programa Azure PowerShell stvaranje kontejnera za servis je prazan oblaka
U ovom se članku objašnjava kako brzo stvaranje spremnika servise u Oblaku pomoću cmdleta ljuske PowerShell Azure. Slijedite korake u nastavku:

1. Instalirajte Microsoft Azure PowerShell cmdlet sa stranice za [Preuzimanje Azure PowerShell](http://aka.ms/webpi-azps) .
2. Otvorite naredbeni redak PowerShell.
3. [Dodavanje AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) koristite za prijavu.

    > [AZURE.NOTE] Dodatne upute za instalaciju cmdlet Azure PowerShell i povezati pretplate Azure, potražite [upute za instalaciju i konfiguriranje Azure PowerShell](../powershell-install-configure.md).

4. Pomoću cmdleta **New-AzureService** stvaranje kontejnera za servis je prazan Azure oblaka.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

5. Slijedite ovaj primjer pozvati cmdlet:
```
New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
```

Dodatne informacije o stvaranju Azure oblaku pokrenite:
```
Get-help New-AzureService
```

### <a name="next-steps"></a>Daljnji koraci

 * Da biste upravljali uvođenje servisa oblaka, pogledajte [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Ukloni AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)i [Postavljanje AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) naredbe. Može uputiti [kako konfigurirati servise u oblaku](cloud-services-how-to-configure.md) za dodatne informacije.

 * Da biste objavite projekt oblaka servisa Azure, pogledajte uzorak koda **PublishCloudService.ps1** s [neprekinutim isporuke za servis u oblaku u Azure](cloud-services-dotnet-continuous-delivery.md).
