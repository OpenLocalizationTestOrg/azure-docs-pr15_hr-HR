<properties 
   pageTitle="Stvaranje DNS zone i snimanje skupova u DNS Azure pomoću .NET SDK | Microsoft Azure" 
   description="Upute za stvaranje DNS zone i skupove zapisa u Azure DNS pomoću .NET SDK." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/19/2016"
   ms.author="jtuliani"/>


# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>Stvaranje DNS zone i skupove zapisa pomoću .NET SDK

Možete automatizirati postupke za stvaranje, brisanje ili ažuriranje DNS zone, skupove zapisa i zapisa pomoću SDK DNS-a biblioteke za upravljanje DNS-a za .NET. Dostupna je cijelog projekta za Visual Studio [ovdje.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Stvorite glavni račun servisa

Obično programatski pristup Azure resursi moguć je putem namjenski računa umjesto korisničke vjerodajnice. Namjenski poslovne subjekte nazivaju se računa "glavni servisa". Da biste koristili uzorak projekta Azure DNS SDK, najprije morate stvoriti glavni račun servisa i dodijelili odgovarajuće dozvole.

1. Slijedite [ove upute](../resource-group-authenticate-service-principal.md) za stvaranje glavni računa servisa (uzorka projekta Azure DNS SDK pretpostavlja provjere autentičnosti utemeljene na lozinku).

2. Stvaranje grupe resursa ([Evo kako](../azure-portal/resource-group-portal.md)).

3. Korištenje Azure RBAC da biste dodijelili glavni račun servisa 'DNS Zone suradničke' dozvole grupi resursa ([Evo kako](../active-directory/role-based-access-control-configure.md).)

4. Ako koristite uzorka projekta Azure DNS SDK, uređivati datoteku, 'program.cs' na sljedeći način:
    * Umetnite odgovarajuće vrijednosti za tenantId, clientId (poznat i kao ID računa), tajna (servis glavnog računa lozinka) i subscriptionId kako je koristiti u koraku 1.
    * Unesite naziv grupe resursa odabrali u koraku 2.
    * Unesite naziv DNS zone po izboru.

## <a name="nuget-packages-and-namespace-declarations"></a>Paketi NuGet i polja naziva deklaracija

Da biste koristili SDK za .NET Azure DNS-a, morate instalirati paket NuGet **Azure biblioteke za upravljanje DNS-a** i druge potrebne Azure pakete.
 
1. U **Visual Studio**, otvorite projekt ili novi projekt. 

2. Kliknite **Alati** **>** **Upravitelj paketa NuGet** **>** **Upravljanje NuGet paketa rješenja...**. 

3. Kliknite **Pregledaj**, omogućivanje potvrdni okvir **Uključi predizdanja** i upišite **Microsoft.Azure.Management.Dns** u okvir za pretraživanje.

4. Odaberite paket, a zatim kliknite **instalirajte** da biste ga dodali u Visual Studio projekt.
 
5. Ponovite postupak da biste i instalirali sljedeće paketa: **Microsoft.Rest.ClientRuntime.Azure.Authentication** i **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Dodavanje prostora za naziv deklaracija

Dodajte sljedeće polje naziva deklaracija

    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.Dns;
    using Microsoft.Azure.Management.Dns.Models;

## <a name="initialize-the-dns-management-client"></a>Pokretanje klijenta za upravljanje DNS-a

*DnsManagementClient* sadrži metode i svojstva koje su potrebne za upravljanje DNS zone i skupovi zapisa.  Sljedeći kod zapisnike u glavni račun za usluge i stvara DnsManagementClient objekt.

    // Build the service credentials and DNS management client
    var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
    var dnsClient = new DnsManagementClient(serviceCreds);
    dnsClient.SubscriptionId = subscriptionId;

## <a name="create-or-update-a-dns-zone"></a>Stvaranje ili ažuriranje DNS zone

Da biste stvorili DNS zone, "Zone" objekt je stvoreno tako da sadrže parametara zone DNS-a. Jer DNS zone nisu povezani određenu regiju, mjesto je postavljeno na "globalni. U ovom primjeru [Voditelj resursa Azure "oznake"](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) i dodaje se u zonu.

Da biste zapravo stvaranje ili ažuriranje zone u Azure DNS, zone objekta koji sadrži parametre zone prosljeđuje metodu *DnsManagementClient.Zones.CreateOrUpdateAsyc* .

>[AZURE.NOTE] DnsManagementClient podržava tri Načini rada: sinkrono ("CreateOrUpdate"), asinkronog ("CreateOrUpdateAsync"), ili asinkronog s pristupom HTTP odgovor ('CreateOrUpdateWithHttpMessagesAsync').  Možete odabrati neku od ovih načina rada, ovisno o vašim potrebama aplikacije.

Azure DNS podržava optimistična Procjena istodobnosti pod nazivom [Etags](dns-getstarted-create-dnszone.md). U ovom primjeru navodeći "*" za 'If-ništa – podudaranje' zaglavlje pokazuje Azure DNS-a da biste stvorili DNS zone ako nešto još ne postoji.  Poziv neće uspjeti ako zonu s navedenim nazivom već postoji u grupi navedeni resursa.

    // Create zone parameters
    var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"
    
    // Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
    dnsZoneParams.Tags = new Dictionary<string, string>();
    dnsZoneParams.Tags.Add("dept", "finance");
    
    // Create the actual zone.
    // Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
    // Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    // Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");

## <a name="create-dns-record-sets-and-records"></a>Stvaranje DNS zapisa skupova i zapisa

DNS zapisima upravlja u skupu zapisa. Skup zapisa je skup zapisa s istim nazivom i vrsta zapisa unutar zone.  Naziv skup zapisa je odnosu naziv zone nije potpuno kvalificiran naziv DNS-a.

Stvaranje ili ažuriranje zapisa postaviti "Skup zapisa" parametara objekt se stvara i prosljeđuje *DnsManagementClient.RecordSets.CreateOrUpdateAsync*. Kao što je s DNS zone postoje tri načina operacije: sinkrono ("CreateOrUpdate"), asinkronog ("CreateOrUpdateAsync"), ili asinkronog s pristupom HTTP odgovor ('CreateOrUpdateWithHttpMessagesAsync').

Kao i kod DNS zone na skupove zapisa obuhvaćaju podršku za optimistična Procjena istodobnosti.  U ovom primjeru jer su navedeni "If podudaranje" niti 'If-ništa – Match', skup zapisa uvijek stvara se.  Ovaj poziv prebrisati neki postojeći zapis postavite s istim nazivom i vrsta zapisa u ovoj zoni DNS-a.

    // Create record set parameters
    var recordSetParams = new RecordSet();
    recordSetParams.TTL = 3600;

    // Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
    recordSetParams.ARecords = new List<ARecord>();
    recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

    // Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
    recordSetParams.Metadata = new Dictionary<string, string>();
    recordSetParams.Metadata.Add("user", "Mary");

    // Create the actual record set in Azure DNS
    // Note: no ETAG checks specified, will overwrite existing record set if one exists
    var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);

## <a name="get-zones-and-record-sets"></a>Zone i skupove zapisa

Metode *DnsManagementClient.Zones.Get* i *DnsManagementClient.RecordSets.Get* dohvaćanje pojedinačne zone i skupove zapisa, odnosno. Skupovi zapisa prepoznaju se po vrsti, naziv i postoje u grupi zonu i resursa. Zone prepoznaju se njegovo ime i postoje u grupu resursa.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
    
## <a name="update-an-existing-record-set"></a>Ažuriranje postojeći skup zapisa

Da biste ažurirali postojeći skup zapisa DNS-a, najprije dohvatiti skup zapisa, a zatim ažuriranje sadržaja za skup zapisa pa pošaljite promjene.  U ovom primjeru ćemo navesti na e-"oznake" iz skupa dohvaćeni zapis u parametru "If podudaranje". Poziv neće uspjeti ako Istodobni postupak je izmjene zapisa u aplikacijom postavljanje.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

    // Add a new record to the local object.  Note that records in a record set must be unique/distinct
    recordSet.ARecords.Add(new ARecord("5.6.7.8"));

    // Update the record set in Azure DNS
    // Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
    recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);

## <a name="list-zones-and-record-sets"></a>Popis zone i skupove zapisa

Na popisu zone načina *DnsManagementClient.Zones.List...* koje podržavaju stavku ili svih zona u grupu resursa navedeni ili svih zona u određenom Azure pretplate (preko grupe resursa.) Na popisu skupove zapisa pomoću metode *DnsManagementClient.RecordSets.List...* koje podržavaju ili popis svih zapisa skupovima u određenoj zoni ili samo one skupove zapisa određene vrste.

Imajte na umu prilikom unosa zone, a može biti paginated skupova zapisa koja je rezultat.  Sljedeći primjer prikazuje način iteracija do stranice s rezultatima. (Na veličinu umjetno male stranice "2" koristi da biste nametnuli broja stranica; u praksi taj parametar mora ispustiti i koristiti zadane veličine stranice)

    // Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
    // In practice, to improve performance you would use a large page size or just use the system default
    int recordSets = 0;
    var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
    recordSets += page.Count();

    while (page.NextPageLink != null)
    {
        page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
        recordSets += page.Count();
    }

## <a name="next-steps"></a>Daljnji koraci

Preuzmite [Azure DNS .NET SDK uzorka projekta](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), koji uključuje dodatne Primjeri kako koristiti Azure DNS .NET SDK-a, uključujući i primjeri za ostale vrste DNS zapisa.
