<properties
 pageTitle="Upravljanje isteka sadržaja blobova platforme Azure prostora za pohranu u Azure CDN | Microsoft Azure"
 description="Dodatne informacije o mogućnostima za kontrolu vrijeme važenja za blob-ova u predmemoriju Azure CDN."
 services="cdn"
 documentationCenter=""
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="multiple"
 ms.topic="article"
 ms.date="09/15/2016"
 ms.author="casoper"/>


# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>Upravljanje isteka Azure spremište blobova platforme sadržaja u Azure CDN

> [AZURE.SELECTOR]
- [Azure web-aplikacije/oblaka servisi, ASP.NET ili IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Servis blobova platforme Azure za pohranu](cdn-manage-expiration-of-blob-content.md)

[Servis blobova platforme](../storage/storage-introduction.md#blob-storage) [Azure pohrane](../storage/storage-introduction.md) u jedan je od nekoliko utemeljen na Azure drugačijeg izvora integriran s Azure CDN.  Bilo koji sadržaj javno dostupnu blob može se spremiti u Azure CDN dok minutama njegov vrijeme važenja (TTL).  Za TTL određen [ *Predmemorijom* zaglavlja](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) u HTTP odgovor Azure prostora za pohranu.

>[AZURE.TIP] Možete odabrati da biste postavili bez TTL na blob.  U ovom slučaju Azure CDN automatski primjenjuje zadani TTL od sedam dana.
>
>Dodatne informacije o funkcioniranje Azure CDN brzinu pristup blob-ova i drugih datoteka potražite u članku [Pregled CDN Azure](./cdn-overview.md).
>
>Dodatne informacije o servisima za blobova platforme Azure pohranu potražite u članku [Blob servisa koncepata](https://msdn.microsoft.com/library/dd179376.aspx). 

Pomoću ovog praktičnog vodiča pokazuje nekoliko načina na koje možete postaviti na TTL blobova platforme Azure pohrane u.  

## <a name="azure-powershell"></a>Azure PowerShell

[Azure PowerShell](../powershell-install-configure.md) je najbrže, najnaprednijih načina za administraciju servisa Azure.  Korištenje na `Get-AzureStorageBlob` cmdlet da biste dobili referenca blob-om, postavite na `.ICloudBlob.Properties.CacheControl` svojstvo. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

>[AZURE.TIP] Možete koristiti i ljuske PowerShell za [Upravljanje profilima CDN i krajnje točke](./cdn-manage-powershell.md).

## <a name="azure-storage-client-library-for-net"></a>Azure prostora za pohranu klijenta biblioteka za .NET

Da biste postavili s blob TTL pomoću .NET, [Azure prostora za pohranu klijenta biblioteke za .NET](../storage/storage-dotnet-how-to-use-blobs.md) koristiti da biste postavili svojstvo [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) .

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
        
        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

>[AZURE.TIP] Dostupni u [Uzorka za spremište blobova platforme Azure za .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)su mnoge dodatne primjere koda .NET.

## <a name="other-methods"></a>Drugi načini

- [Azure sučelje naredbenog retka](../xplat-cli-install.md)

    Kada prenesete blob-om, postavite pomoću *cacheControl* svojstvo na `-p` prijelaz.  U ovom se primjeru postavlja na TTL jedan sat (3600 sekundi).

    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

- [Servise za pohranu Azure REST API-JA](https://msdn.microsoft.com/library/azure/dd179355.aspx)

    Izričito postavite svojstvo *x-ms-blob-predmemorijom* na zahtjev za [Staviti Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Postavite popis blokiranih](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)ili [Postaviti svojstva Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx) .

- Alati za upravljanje trećih strana prostora za pohranu

    Alati za Upravljanje spremištem Azure neke trećih strana omogućuju postavite svojstvo *CacheControl* na blob-ova. 

## <a name="testing-the-cache-control-header"></a>Testiranje *Predmemorijom* zaglavlja

Jednostavno možete provjeriti TTL vaše blob-ova.  Pomoću preglednika [alate za razvojne inženjere](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), testirajte svoje blob je uključujući zaglavlje *Predmemorijom* odgovor.  Alat za kao što su **wget**, [Postman](https://www.getpostman.com/)ili [Fiddler](http://www.telerik.com/fiddler) možete koristiti i da biste pregledali zaglavlja odgovor.

## <a name="next-steps"></a>Daljnji koraci

- [Saznajte više o zaglavlju *Predmemorijom*](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
- [Informirajte se o upravljanju isteka sadržaja u Oblaku u Azure CDN](./cdn-manage-expiration-of-cloud-service-content.md)

