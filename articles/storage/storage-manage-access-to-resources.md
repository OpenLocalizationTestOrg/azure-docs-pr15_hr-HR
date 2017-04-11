<properties
    pageTitle="Upravljanje anonimnim pristupom čitanje spremnika i blob-ova | Microsoft Azure"
    description="Saznajte kako omogućiti spremnika i blob-ova za anonimni pristup te kako ih programatski pristup."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Upravljanje anonimnim pristupom čitanje spremnika i blob-ova

## <a name="overview"></a>Pregled

Prema zadanim postavkama, samo vlasnik računa za pohranu mogu pristupati resursa za pohranu u taj račun. Za spremište blobova platforme samo, možete postaviti dozvole u spremniku dopustiti anonimni pristup čitanje spremnik i njegov blob polja tako da možete dodijeliti pristup tim resursima bez omogućivanja zajedničkog korištenja ključ za račun.

Anonimni pristup je najbolje odgovara scenariji mjesto na koje želite da određene blob-ova uvijek biti dostupan za anonimni pristup za čitanje. Za kontrolu precizniji grained možete stvoriti potpis zajednički pristup, što vam omogućuje da biste pomoću drugačije dozvole prava pristupa delegiranju ograničena, a pokazivač u određenom vremenskom intervalu. Dodatne informacije o stvaranju potpisa zajednički pristup potražite u članku [Korištenje zajednički pristup potpisa (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Dodjela dozvola anonimnim korisnicima za spremnika i blob-ova

Po zadanom spremnika i sve blob-ova u njoj mogu pristupiti samo vlasnik računa za pohranu. Da bi se dobilo anonimnim korisnicima dozvole spremnika i njegova blob-ova za čitanje, možete postaviti dozvole spremnik dopustili pristup javno. Anonimni korisnici mogu čitati blob-ova u spremniku javno dostupnu bez provjere autentičnosti zahtjev.

Spremnika sadrže sljedeće mogućnosti za upravljanje pristupom spremnik:

- **Puni pristup javnim čitanje:** Spremnik i blob podataka mogu čitati putem anonimni zahtjev. Klijenti možete numerirati blob polja unutar kontejnera putem anonimni zahtjev, ali nije moguće numerirati spremnika subjekta prostora za pohranu.

- **Javno čitati pristup samo blob-ova:** Blob podataka u ovom spremniku je moguće čitati putem anonimni zahtjev, ali spremnik podataka nije dostupna. Klijenti nije moguće numerirati blob polja unutar kontejnera putem anonimni zahtjev.

- **Bez javno čitati pristupa:** Spremnik i blob podataka mogu čitati samo vlasnik računa.

Spremnik dozvole možete postaviti na sljedeće načine:

- S [portala za Azure](https://portal.azure.com).
- Programski pomoću klijentska biblioteka za pohranu ili REST API-JA.
- Pomoću komponente PowerShell. Dodatne informacije o postavljanju dozvola kontejner s Azure PowerShell, potražite u članku [Korištenje Azure PowerShell s Azure prostora za pohranu](storage-powershell-guide-full.md#how-to-manage-azure-blobs).

### <a name="setting-container-permissions-from-the-azure-portal"></a>Postavljanje dozvola za kontejner s portala za Azure

Da biste postavili dozvole kontejner s [Portala za Azure](https://portal.azure.com), slijedite ove korake:

1. Idite na nadzornoj ploči za vaš račun za pohranu.
2. Odaberite naziv kontejner s popisa. Kliknite naziv izlaže blob-ova u odabranom spremniku
3. Na alatnoj traci odaberite **pravilnik o pristupu** .
4. U polju **Vrsta pristupa** odaberite na željenu razinu dozvola, kao što je prikazano u nastavku snimku zaslona.

    ![Uređivanje metapodataka spremnik dijaloški okvir](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="setting-container-permissions-programmatically-using-net"></a>Postavljanje dozvola za spremnik programatski pomoću .NET

Da biste postavili dozvole za spremnik pomoću klijentska biblioteka .NET, najprije dohvatiti kontejner s postojeće dozvole tako da nazovete metodu **GetPermissions** . Postavite svojstvo **PublicAccess** **BlobContainerPermissions** objekt koji je vratio metodu **GetPermissions** . Na kraju, nazovite **SetPermissions** način s dozvolama za ažurirani.

Sljedeći primjer postavlja dozvole kontejner s puno javno pristup za čitanje. Postavljanje dozvola za javnu pristup za čitanje za blob-ova samo, postavite svojstvo **PublicAccess** na **BlobContainerPublicAccessType.Blob**. Da biste uklonili sve dozvole za anonimnim korisnicima, postavite svojstvo na **BlobContainerPublicAccessType.Off**.

    public static void SetPublicContainerPermissions(CloudBlobContainer container)
    {
        BlobContainerPermissions permissions = container.GetPermissions();
        permissions.PublicAccess = BlobContainerPublicAccessType.Container;
        container.SetPermissions(permissions);
    }

## <a name="access-containers-and-blobs-anonymously"></a>Anonimno pristupiti spremnika i blob-ova

Klijent koji pristupa spremnika i blob-ova anonimno možete koristiti constructors koja ne zahtijeva vjerodajnice. Sljedeći primjeri pokazuju na nekoliko različitih načina referentni resursi za servisnu Blob anonimno.

### <a name="create-an-anonymous-client-object"></a>Stvaranje objekta anonimni klijenta

Možete stvoriti novu klijentskog objekta servisa za anonimni pristup unosom krajnja točka servisa blobova platforme za račun. Međutim, morate znati i naziv spremnika u taj račun koji je dostupan za anonimni pristup.

    public static void CreateAnonymousBlobClient()
    {
        // Create the client object using the Blob service endpoint.
        CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

        // Read the container's properties. Note this is only possible when the container supports full public read access.
        container.FetchAttributes();
        Console.WriteLine(container.Properties.LastModified);
        Console.WriteLine(container.Properties.ETag);
    }

### <a name="reference-a-container-anonymously"></a>Anonimno referencu spremnik

Ako imate URL spremniku anonimno dostupan, možete je koristiti referentni spremnik izravno.

    public static void ListBlobsAnonymously()
    {
        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

        // List blobs in the container.
        foreach (IListBlobItem blobItem in container.ListBlobs())
        {
            Console.WriteLine(blobItem.Uri);
        }
    }


### <a name="reference-a-blob-anonymously"></a>Referenca blob anonimno

Ako imate URL blob koji je dostupan za anonimni pristup, možete se referencirati blob izravno pomoću tog URL-a:

    public static void DownloadBlobAnonymously()
    {
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
        blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
    }

## <a name="features-available-to-anonymous-users"></a>Značajke koje su dostupne anonimnim korisnicima

Sljedeća tablica pokazuje operacijama možda zove anonimni korisnici kada je spremnik ACL postavljen dopustili pristup javno.

| OSTATAK postupka                                         | Dozvola s puno javno pristup za čitanje | Dozvola s javnim pristupom čitanja za samo blob-ova |
|--------------------------------------------------------|-----------------------------------------|---------------------------------------------------|
| Popis spremnika                                        | Samo vlasnik                              | Samo vlasnik                                        |
| Stvaranje kontejnera                                       | Samo vlasnik                              | Samo vlasnik                                        |
| Dobiti spremnik svojstva                               | Sve                                     | Samo vlasnik                                        |
| Početak spremnik metapodataka                                 | Sve                                     | Samo vlasnik                                        |
| Postavljanje spremnika metapodataka                                 | Samo vlasnik                              | Samo vlasnik                                        |
| Početak spremnik ACL-a                                      | Samo vlasnik                              | Samo vlasnik                                        |
| Postavljanje spremnika ACL-a                                      | Samo vlasnik                              | Samo vlasnik                                        |
| Brisanje spremnik                                       | Samo vlasnik                              | Samo vlasnik                                        |
| Popis blob-ova                                             | Sve                                     | Samo vlasnik                                        |
| Stavite blobova platforme                                               | Samo vlasnik                              | Samo vlasnik                                        |
| Početak blobova platforme                                               | Sve                                     | Sve                                               |
| Dobiti Blob svojstva                                    | Sve                                     | Sve                                               |
| Postavljanje svojstava Blob                                    | Samo vlasnik                              | Samo vlasnik                                        |
| Početak Blob metapodataka                                      | Sve                                     | Sve                                               |
| Postavljanje Blob metapodataka                                      | Samo vlasnik                              | Samo vlasnik                                        |
| Stavite bloka                                              | Samo vlasnik                              | Samo vlasnik                                        |
| Dohvaćanje popisa blokova (samo za izvršenja blokova)                 | Sve                                     | Sve                                               |
| Dohvaćanje popisa blokova (samo nepotvrđenim blokova ili sve blokova) | Samo vlasnik                              | Samo vlasnik                                        |
| Postavljanje popisa blokova                                         | Samo vlasnik                              | Samo vlasnik                                        |
| Brisanje blobova platforme                                            | Samo vlasnik                              | Samo vlasnik                                        |
| Kopiranje blobova platforme                                              | Samo vlasnik                              | Samo vlasnik                                        |
| Snimka Blob                                          | Samo vlasnik                              | Samo vlasnik                                        |
| Zakup Blob                                             | Samo vlasnik                              | Samo vlasnik                                        |
| Postavljanje stranice                                               | Samo vlasnik                              | Samo vlasnik                                        |
| Dohvaćanje raspon stranica                                        | Sve                                     | Sve                                                  |
| Dodavanje blobova platforme                                            | Samo vlasnik                              | Samo vlasnik                                                  |


## <a name="see-also"></a>Vidi također

- [Provjera autentičnosti za servise za pohranu za Azure](https://msdn.microsoft.com/library/azure/dd179428.aspx)
- [Korištenje potpisa zajednički pristup (SAS)](storage-dotnet-shared-access-signature-part-1.md)
- [Delegiranje pristupa potpisom zajednički pristup](https://msdn.microsoft.com/library/azure/ee395415.aspx)
