<properties
    pageTitle="Azure PowerShell pomoću Azure prostora za pohranu | Microsoft Azure"
    description="Saznajte kako koristiti Azure PowerShell cmdleti za Azure prostora za pohranu za stvaranje i upravljanje računa za pohranu. Rad s blob-ova, tablice, redovima i datotekama. Konfiguriranje upita analytics za pohranu i stvaranje zajedničkog pristupa potpisa."
    services="storage"
    documentationCenter="na"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="using-azure-powershell-with-azure-storage"></a>Azure PowerShell pomoću Azure prostora za pohranu

## <a name="overview"></a>Pregled

Azure PowerShell je modul koji sadrži cmdleti za upravljanje Azure putem komponente Windows PowerShell. Je utemeljenoj na zadacima naredbenog retka ljuske i skriptnog jezika dizajniran posebno za administraciju sustava. Pomoću komponente PowerShell jednostavno možete kontrolirati i automatizirati Administracija servisa Azure i aplikacije. Ako, na primjer, možete koristiti s cmdletima za izvođenje iste zadatke koje možete izvršiti pomoću [Portala za Azure](https://portal.azure.com).

U ovom vodiču smo ćete Istražite kako pomoću [Cmdleta Azure prostora za pohranu](https://msdn.microsoft.com/library/azure/mt269418.aspx) za izvođenje različitih razvoj i administracije zadataka s Azure prostora za pohranu.

Ovaj vodič podrazumijeva prethodnog sučelje pomoću [Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/) i [Komponente Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx). Vodič sadrži broj skripte da bismo pokazali korištenje ljuske PowerShell s Azure prostora za pohranu. Trebali biste ažurirati varijable skripte ovisno o vašoj konfiguraciji prije pokretanja svako pismo.

Prvi se odjeljak u ovom vodiču nudi sažet pregled Azure i prostor za pohranu PowerShell. Detaljne upute za i pokretanje [preduvjeti za korištenje Azure PowerShell s Azure prostora za pohranu](#prerequisites-for-using-azure-powershell-with-azure-storage).


## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Početak rada s Azure prostora za pohranu i PowerShell u pet minuta

U ovom se odjeljku objašnjava da biste pristupili Azure prostora za pohranu PowerShell pet minuta.

**Novi korisnik Azure:** Pronađite web-mjesto Microsoft Azure pretplata i u okvir za Microsoftov račun pridružen tom pretplate. Informacije o mogućnostima Azure za kupnju, potražite u članku [Besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/), [Mogućnosti za kupnju](https://azure.microsoft.com/pricing/purchase-options/)i [Nudi člana](https://azure.microsoft.com/pricing/member-offers/) (za članove MSDN, Microsoftovoj partnerskoj mreži i BizSpark i druge Microsoftove programe).

Dodatne informacije o Azure pretplate potražite u članku [Dodjela administratorskih uloga u Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Nakon stvaranja pretplate na Microsoft Azure i račun:**

1.  Preuzmite i instalirajte [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409).
2.  Start sustava Windows PowerShell skriptiranje okruženje (filtar): U lokalnom računalu, idite na izbornik **Start** . Upišite **Administrativni alati** i kliknite ga pokrenuti. U prozoru **Administrativni alati** , desnom tipkom miša kliknite **Windows Očisti filtar**, kliknite **Pokreni kao Administrator**.
3.  U **Windows Očisti filtar**, kliknite **datoteka** > **Novo** da biste stvorili novu datoteku skripte.
4.  Sada ćemo će vam dati jednostavnu skriptu koja prikazuje osnovne naredbe ljuske PowerShell za pristup Azure prostora za pohranu. Skripta najprije zatražite od vašeg Azure vjerodajnice računa da biste dodali svoje Azure račun u lokalnom okruženju PowerShell. Skripta pa će postavljanje zadanog Azure pretplate i stvaranje novog računa za pohranu u Azure. Nakon toga skripta će stvoriti novi spremnik na novi račun za pohranu i Prenesi postojeću datoteku slike (blob) za njih. Nakon popise skripta sve blob-ova u njih stvorite novi odredište direktorij na lokalnom računalu će se i preuzmite slikovne datoteke.
5.  U sljedećem odjeljku kod odaberite skripte između s primjedbama **#begin** i **#end**. Pritisnite CTRL + C da biste ga kopirali u međuspremnik.

        #begin
        # Update with the name of your subscription.
        $SubscriptionName = "YourSubscriptionName"

        # Give a name to your new storage account. It must be lowercase!
        $StorageAccountName = "yourstorageaccountname"

        # Choose "West US" as an example.
        $Location = "West US"

        # Give a name to your new container.
        $ContainerName = "imagecontainer"

        # Have an image file and a source directory in your local computer.
        $ImageToUpload = "C:\Images\HelloWorld.png"

        # A destination directory in your local computer.
        $DestinationFolder = "C:\DownloadImages"

        # Add your Azure account to the local PowerShell environment.
        Add-AzureAccount

        # Set a default Azure subscription.
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

        # Create a new storage account.
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location

        # Set a default storage account.
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

        # Create a new container.
        New-AzureStorageContainer -Name $ContainerName -Permission Off

        # Upload a blob into a container.
        Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

        # List all blobs in a container.
        Get-AzureStorageBlob -Container $ContainerName

        # Download blobs from the container:
        # Get a reference to a list of all blobs in a container.
        $blobs = Get-AzureStorageBlob -Container $ContainerName

        # Create the destination directory.
        New-Item -Path $DestinationFolder -ItemType Directory -Force  

        # Download blobs into the local destination directory.
        $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
        #end

5.  U **Windows Očisti filtar**, pritisnite CTRL + V da biste kopirali skriptu. Kliknite **datoteka** > **Spremi**. U dijaloškom okviru **Spremi kao** upišite naziv datoteke skripte, kao što su "mystoragescript". Kliknite **Spremi**.

6.  Sada ćete morati ažurirati varijable skripte na temelju konfiguracijske postavke. Varijabla **$SubscriptionName** morate ažurirati vlastiti pretplate. Možete zadržati varijable kao što je navedeno u skripti ili ih ažurirati po želji.

    - **$SubscriptionName:** Ova varijabla morate ažurirati uz ime pretplate. Poduzmite jedan od sljedeća tri načina da biste pronašli naziv pretplate:

        na. U **Windows Očisti filtar**, kliknite **datoteka** > **Novo** da biste stvorili novu datoteku skripte. Kopirajte sljedeću skriptu novu datoteku skripte, a zatim kliknite **ispravljanje pogrešaka** > **Pokreni**. Sljedeću skriptu najprije zatražite od vašeg Azure vjerodajnice računa da biste dodali svoje Azure račun u lokalnom okruženju PowerShell i prikaz svih pretplata koji su povezani s lokalnom sesiju ljuske PowerShell. Uzeti u obzir naziv pretplatu u koju želite koristiti tijekom slijedite ovaj Praktični vodič:

            Add-AzureAccount
                Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName

        b. Da biste pronašli i kopirajte naziv svoje pretplate [Azure Portal](https://portal.azure.com)na izborniku koncentrator s lijeve strane kliknite **pretplate**. Kopirajte naziv pretplatu koju želite koristiti prilikom pokretanja skripte u ovom vodiču.

        ![Portal za Azure][Image2]

        c. Da biste pronašli i kopirajte naziv svoje pretplate na [Klasični Portal Azure](https://manage.windowsazure.com/), pomaknite se prema dolje, a zatim **Postavke** na lijevoj strani portal. Kliknite **pretplate** da biste vidjeli popis pretplate. Kopirajte naziv pretplatu koju želite koristiti prilikom pokretanja skripte navedene u ovom vodiču.

        ![Azure klasični Portal][Image1]

    - **$StorageAccountName:** Nazovite navedenog u skripti ili unesite novi naziv za vaš račun za pohranu. **Važne:** Naziv računa spremišta mora biti jedinstvena u Azure. Mora biti mala slova, previše!

    - **$Location:** Koristite zadani "Zapad Sjedinjenih DRŽAVA" u skripti ili odaberite drugim mjestima Azure, kao što su Istočni sad, Sjeverna Europa i tako dalje.

    - **$ContainerName:** Nazovite navedenog u skripti ili unesite novi naziv za svoje kontejner.

    - **$ImageToUpload:** Unesite put do slike na lokalnom računalu, kao što su: "C:\Images\HelloWorld.png".

    - **$DestinationFolder:** Unesite put do lokalni direktorij za spremanje datoteke preuzete iz Azure prostor za pohranu, kao što su: "C:\DownloadImages".

7.  Nakon ažuriranja varijable skripte u datoteci "mystoragescript.ps1", kliknite **datoteka** > **Spremi**. Zatim **ispravljanje pogrešaka** > **pokretanje** ili pritisnite **F5** da biste pokrenuli skriptu.  

Nakon izvođenja skripta, imat ćete lokalne odredišnu mapu koja obuhvaća preuzetih slikovne datoteke. Sljedeće snimka zaslona prikazuje se primjer izlaz:

![Preuzimanje blob-ova][Image3]


> [AZURE.NOTE] U odjeljku "Prvi koraci s Azure prostora za pohranu i PowerShell u pet minuta" kratko Upoznavanje navedeni su na kako pomoću ljuske PowerShell za Azure s Azure prostora za pohranu. Detaljne upute za i Pozivamo vas da biste pročitali u sljedećim odjeljcima.

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Preduvjeti za korištenje Azure PowerShell za pohranu za Azure
Potreban je Azure pretplate i račun za pokretanje cmdleta ljuske PowerShell navedene u ovom vodiču opisane.

Azure PowerShell je modul koji sadrži cmdleti za upravljanje Azure putem komponente Windows PowerShell. Informacije o instalaciji i postavljanju Azure PowerShell, potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md). Preporučujemo da preuzimanje i instaliranje ili nadogradnja najnovije modul Azure PowerShell prije nego počnete koristiti ovaj vodič.

Možete pokrenuti s cmdletima u standardni konzole za Windows PowerShell ili u Windows PowerShell integrirani skriptiranje okruženje (filtar). Ako, na primjer, da biste otvorili **Windows Očisti filtar**, idite na izbornik Start, upišite Administrativni alati i kliknite da biste ga pokrenuti. U prozoru Administrativni alati, desnom tipkom miša kliknite Windows Očisti filtar, kliknite Pokreni kao Administrator.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Kako upravljati računima za pohranu u Azure

### <a name="how-to-set-a-default-azure-subscription"></a>Postavljanje zadanih Azure pretplate
Da biste upravljali pohranu Azure pomoću komponente PowerShell Azure, morate klijent okruženje pomoću Azure putem provjere autentičnosti Azure Active Directory ili provjere autentičnosti utemeljene na certifikat za provjeru autentičnosti. Detaljne informacije potražite u članku vodič [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) . Ovaj vodič koristi provjeru autentičnosti Azure Active Directory.

1.  U sustavu Windows Očisti filtar upišite sljedeću naredbu da biste dodali svoje Azure račun u lokalnom okruženju PowerShell:

    `Add-AzureAccount`

2.  U prozoru "Znak u za Microsoft Azure" upišite adresu e-pošte i lozinku povezanu s računom. Azure potvrđuje sprema vjerodajnicu informacije i zatvara prozor.

3.  Nakon toga pokrenite sljedeću naredbu da biste pogledali Azure računi u lokalnom okruženju PowerShell i provjerite je li naveden vaš račun:

    `Get-AzureAccount`

4.  Zatim pokrenite sljedeći cmdlet da biste pogledali sve pretplate koji su povezani s lokalnom sesiju ljuske PowerShell i provjerite je li navedena je pretplate:

    `Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`

5.  Da biste postavili zadani Azure pretplatu, pokrenite cmdlet odaberite AzureSubscription:

        $SubscriptionName = 'Your subscription Name'
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

6.  Provjerite naziv pretplate zadani tako da pokrenete cmdlet Get-AzureSubscription:

    `Get-AzureSubscription -Default`

7.  Da biste vidjeli sve dostupne PowerShell cmdleti za pohranu Azure, pokrenite:

    `Get-Command -Module Azure -Noun *Storage*`

### <a name="how-to-create-a-new-azure-storage-account"></a>Kako stvoriti novi račun za Azure prostora za pohranu
Da biste koristili Azure prostora za pohranu, morate je račun za pohranu. Nakon što ste konfigurirali računalo povezati s pretplate možete stvoriti na novi račun za Azure prostora za pohranu.

1.  Pokrenite cmdlet Get-AzureLocation da biste pronašli sve dostupne podatkovnog centra lokacije:

    `Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes`

2.  Nakon toga pokrenite cmdlet novo AzureStorageAccount da biste stvorili novi račun za pohranu. Sljedeći primjer stvara novi račun za pohranu u podatkovnim centrom "Zapad sad".

        $location = "West US"
        $StorageAccountName = "yourstorageaccount"
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location

> [AZURE.IMPORTANT] Naziv računa spremišta mora biti jedinstvena u Azure i mora biti mala slova. Konvencije imenovanja i ograničenja, potražite u članku [O računi servisa Azure prostora za pohranu](storage-create-storage-account.md) i [imenovanje i pozivate spremnika, blob-ova, te metapodataka](http://msdn.microsoft.com/library/azure/dd135715.aspx).

### <a name="how-to-set-a-default-azure-storage-account"></a>Kako postaviti zadani račun za Azure prostora za pohranu
Imate više računa za pohranu za pretplatu. Možete odabrati jednu od njih i postavite ga kao zadani račun za pohranu za sve naredbe za pohranu u istu sesiju ljuske PowerShell. Omogućuje pokretanje naredbe za pohranu Azure PowerShell bez navođenja izričito kontekst za pohranu.

1.  Da biste postavili zadani račun za pohranu za vašu pretplatu, možete pokrenuti cmdlet skup AzureSubscription.

        $SubscriptionName = "Your subscription name"
        $StorageAccountName = "yourstorageaccount"  
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

2.  Nakon toga pokrenite cmdlet Get-AzureSubscription da biste provjerili je li račun za pohranu povezane s vašim računom zadani pretplate. Ta naredba Vrati svojstva pretplate na trenutne pretplate uključujući njegove trenutni račun za pohranu.

        Get-AzureSubscription –Current

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Kako na popisu svi računi Azure prostora za pohranu u pretplatu
Azure pretplate može sadržavati do 100 račune za pohranu. Najnovije informacije na ograničenja, potražite u članku [Azure pretplate i ograničenja servisa, kvote, i ograničenja](../azure-subscription-service-limits.md).

Pokrenite sljedeći cmdlet da biste saznali naziv i status račune za pohranu u trenutne pretplate:

    Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus

### <a name="how-to-create-an-azure-storage-context"></a>Kako stvoriti u kontekstu Azure prostora za pohranu
Kontekst Azure prostora za pohranu je objekta u ljuske PowerShell za Enkapsulacija vjerodajnice za pohranu. Korištenje kontekst za pohranu dok se izvodi sve sljedeći cmdlet omogućuje provjeru autentičnosti zahtjev bez računa za pohranu i njegov tipkovnog izričito. Kontekst za pohranu možete stvoriti na brojne načine, kao što je pomoću računa naziv i pristup ključa za pohranu, token zajednički pristup potpis (SAS), a zatim niz za povezivanje ili anonimnih. Dodatne informacije potražite u članku [Novo AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx).  

Koristite jedan od sljedeća tri načina za stvaranje kontekst za pohranu:

- Pokrenite cmdlet [Get-AzureStorageKey](http://msdn.microsoft.com/library/azure/dn495235.aspx) da biste saznali tipkovnog primarni prostora za pohranu za vaš račun za Azure prostora za pohranu. Nakon toga poziv cmdleta [New AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx) da biste stvorili kontekst za pohranu:

        $StorageAccountName = "yourstorageaccount"
        $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
        $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary


- Generiranje token potpis zajednički pristup za u kontejner Azure prostora za pohranu i koristiti za stvaranje kontekst za pohranu:

        $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
        $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken

    Dodatne informacije potražite u članku [Novo AzureStorageContainerSASToken](http://msdn.microsoft.com/library/azure/dn806416.aspx) i [Korištenje zajednički pristup potpisa (SAS)](storage-dotnet-shared-access-signature-part-1.md).

- U nekim slučajevima, preporučujemo vam da biste odredili krajnje točke servisa kada stvorite novi prostor za pohranu kontekst. To može biti potrebno kada registriranih prilagođenog naziva domene za vaš račun za pohranu sa servisom Blob ili ako želite potpis zajednički pristup koristite za pristup resursa za pohranu. Postavljanje krajnje točke servisa u nizu za povezivanje i koristite da biste stvorili novi kontekst za pohranu, kao što je prikazano u nastavku:

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
        $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString

Dodatne informacije o konfiguriranju niza za povezivanje za pohranu potražite u članku [Konfiguriranje nizu za povezivanje](storage-configure-connection-string.md).

Sad kad ste postavili računalo i naučili kako upravljanje pretplatama i račune za pohranu pomoću komponente PowerShell Azure, idite na sljedeći odjeljak da biste saznali kako upravljati Azure blob-ova i bloba snimke.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Kako dohvatiti i Obnovi tipke Azure prostora za pohranu

Račun za Azure pohranu isporučuje se s dvije tipke računa. Koristite sljedeći cmdlet za dohvaćanje ključeva.

    Get-AzureStorageKey -StorageAccountName "yourstorageaccount"

Koristite sljedeći cmdlet za dohvaćanje određenih tipka. Valjane vrijednosti su primarni i sekundarni.  

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary

Ako želite Obnovi ključeva, koristite sljedeći cmdlet. Valjane vrijednosti za - KeyType su "Primarne" i "Sekundarne"

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Primary”

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Secondary”

## <a name="how-to-manage-azure-blobs"></a>Kako upravljati Azure blob-ova
Azure blobova je servis za pohranu velike količine nestrukturirane podatke, poput teksta ili binarni podataka koji se može pristupiti iz bilo kojeg mjesta na svijetu putem HTTP ili HTTPS. U ovom se odjeljku pretpostavlja da ste već upoznati s usluga spremišta blobova platforme Azure koncepata. Detaljne informacije potražite u članku [Početak rada s blobova pomoću .NET](storage-dotnet-how-to-use-blobs.md) i [Blob servisa koncepata](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-to-create-a-container"></a>Upute za stvaranje spremnika
Svaki blobova platforme Azure pohrane u mora biti u spremniku. Možete stvoriti spremnik za privatni pomoću cmdleta New-AzureStorageContainer:

    $StorageContainerName = "yourcontainername"
    New-AzureStorageContainer -Name $StorageContainerName -Permission Off

> [AZURE.NOTE] Postoje tri razine anonimni pristup za čitanje: **isključivanje** **Blob**i **kontejner**. Da biste spriječili anonimni pristup blob-ova, postavite parametar dozvola na **Isključeno**. Prema zadanim postavkama, novi spremnik je privatna te im možete pristupiti samo vlasnik računa. Da biste omogućili anonimni javno čitanje blob resursa, ali ne spremnik metapodataka ili popis blob-ova u spremniku, postavite parametar dozvola na **Blob**. Dopustili potpuni javno čitanje pristup resursima blob spremnik metapodataka i popis blob-ova u željeni spremnik dozvola parametar postaviti **spremniku**. Dodatne informacije potražite u članku [Upravljanje anonimni pristup za čitanje spremnika i blob-ova](storage-manage-access-to-resources.md).

### <a name="how-to-upload-a-blob-into-a-container"></a>Kako prenijeti blob u spremniku
Azure blobova podržava bloka blob-ova i blob-Ova stranica. Dodatne informacije potražite u članku [objašnjenje bloka blob-ova, dodavanje blob-ova, i blob-Ova stranica](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Da biste prenijeli blob-ova u spremniku, koristite cmdlet [Skup AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) . Prema zadanim postavkama ta se naredba prenosi lokalne datoteke blob blok. Da biste odredili vrstu za blob-om, možete koristiti parametar - BlobType.

U sljedećem primjeru izvršava cmdlet [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) da biste sve datoteke u određenu mapu, a zatim prosljeđuje ih sljedeći cmdlet pomoću operatora kanalima. [Postavljanje AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) cmdlet prenosi lokalne datoteke na spremnik:

    Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"

### <a name="how-to-download-blobs-from-a-container"></a>Kako preuzeti blob-ova iz spremnik
Sljedeći primjer pokazuje kako preuzeti blob-ova iz spremnika. U primjeru najprije uspostavlja vezu za pohranu Azure pomoću računa kontekst za pohranu koji sadrži naziv računa za pohranu i pristup primarni ključ. Nakon toga u primjeru dohvaća reference blob pomoću cmdleta [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) . Nakon toga u primjeru pomoću cmdleta [Get-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806418.aspx) da biste preuzeli blob-ova u lokalnom odredišnu mapu.

    #Define the variables.
    $ContainerName = "yourcontainername"
    $DestinationFolder = "C:\DownloadImages"

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #List all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Download blobs from a container.
    New-Item -Path $DestinationFolder -ItemType Directory -Force
    $blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>Kako kopirati blob-ova iz jednog spremnik za pohranu u drugi
Možete kopirati blob-ova preko račune za pohranu i područja asinkrono. Sljedeći primjer pokazuje kako kopirati blob-ova iz jednog spremnik za pohranu na drugu u dva računa različite prostora za pohranu. U primjeru najprije postavlja varijable za izvorišne i odredišne račune za pohranu i stvara kontekst za pohranu za svaki račun. Nakon toga u primjeru kopira blob-ova iz spremnik izvor spremniku odredište pomoću cmdleta [Start AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) . U primjeru pretpostavlja da izvorišni i odredišni račune za pohranu i spremnika već postoji.

    #Define the source storage account and context.
    $SourceStorageAccountName = "yoursourcestorageaccount"
    $SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
    $SrcContainerName = "yoursrccontainername"
    $SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

    #Define the destination storage account and context.
    $DestStorageAccountName = "yourdeststorageaccount"
    $DestStorageAccountKey = "Storage key for yourdeststorageaccount"
    $DestContainerName = "destcontainername"
    $DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

    #Get a reference to blobs in the source container.
    $blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

    #Copy blobs from one container to another.
    $blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext

Imajte na umu da u ovom primjeru izvodi asinkronog Kopiraj. Možete nadzirati status svaku kopiju tako da pokrenete cmdlet [Get-AzureStorageBlobCopyState](http://msdn.microsoft.com/library/azure/dn806406.aspx) .

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>Kako kopirati blob-ova sa sekundarnom mjesta
Blob-ova možete kopirati iz sekundarne mjesto RA GRS-omogućenim računa.

    #define secondary storage context using a connection string constructed from secondary endpoints.
    $SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
    Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext

### <a name="how-to-delete-a-blob"></a>Kako izbrisati blob
Da biste izbrisali blob, najprije se referenca blob, a zatim poziva cmdlet Ukloni AzureStorageBlob na njemu. U sljedećem primjeru briše sve blob-ova u spremniku za dani. U primjeru najprije postavlja varijable za račun za pohranu i stvara kontekst za pohranu. Nakon toga u primjeru dohvaća reference blob pomoću cmdleta [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) i izvršava cmdlet [Ukloni AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806399.aspx) da biste uklonili blob-ova iz spremnika u Azure prostora za pohranu.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "containername"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to all the blobs in the container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Delete blobs in a specified container.
    $blobs| Remove-AzureStorageBlob

## <a name="how-to-manage-azure-blob-snapshots"></a>Kako upravljati snimke blobova platforme Azure
Azure omogućuje stvaranje snimke blob. Snimka je verzija blob koji koristi se u trenutku u vremenu koja je samo za čitanje. Nakon što snimku, ga možete biti čitati, kopirati, ili izbrisane, ali ne i mijenjati. Brze snimke omogućuje sigurnosno kopiranje blob kao što je prikazano u trenutku vremena. Dodatne informacije potražite u članku [Stvaranje snimke Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-to-create-a-blob-snapshot"></a>Kako stvoriti blob snimke
Da biste stvorili Brza snimka od blob, najprije se blob referencu, a zatim poziva na `ICloudBlob.CreateSnapshot` metoda na njemu. U sljedećem primjeru najprije postavlja varijable za račun za pohranu i stvara kontekst za pohranu. Nakon toga u primjeru dohvaća reference blob pomoću cmdleta [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) i pokreće metodu [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) da biste stvorili brzu snimku.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "yourcontainername"
    $BlobName = "yourblobname"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

    #Create a snapshot of the blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

### <a name="how-to-list-a-blobs-snapshots"></a>Kako se popis s blob snimke
Možete stvoriti proizvoljan broj snimke u obliku u kojem želite za blob. Možete navesti snimke povezan s vašeg blob da biste pratili svoje trenutne snimke. Sljedeći primjer koristi unaprijed definirane blob i poziva cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) popis snimaka te blob.  

    #Define the blob name.
    $BlobName = "yourblobname"

    #List the snapshots of a blob.
    Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Kako kopirati snimke blob
Možete kopirati snimke blob da biste vratili snimke. Detaljne informacije i ograničenja, potražite u članku [Stvaranje snimke Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx). U sljedećem primjeru najprije postavlja varijable za račun za pohranu i stvara kontekst za pohranu. Nakon toga u primjeru definira naziv varijable kontejner i blob. U primjeru dohvaća reference blob pomoću cmdleta [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) i pokreće metodu [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) da biste stvorili brzu snimku. Nakon toga u primjeru pokreće cmdlet [Start AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) da biste kopirali snimke blob pomoću objekta ICloudBlob za blob izvora. Ne zaboravite ažurirati varijable ovisno o vašoj konfiguraciji prije pokretanja u primjeru. Imajte na umu u sljedećem primjeru pretpostavlja da izvorišnog web-mjesta i odredište spremnika i blob izvora već postoji.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Define the variables.
    $SrcContainerName = "yoursourcecontainername"
    $DestContainerName = "yourdestcontainername"
    $SrcBlobName = "yourblobname"
    $DestBlobName = "CopyBlobName"

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

    #Create a snapshot of a blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

    #Copy the snapshot to another container.
    Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName

Sad kad ste naučili kako upravljati Azure blob-ova i bloba snimke sa servisom PowerShell Azure, idite na sljedeći odjeljak da biste saznali kako upravljati tablica, redovima i datoteke.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Kako upravljati Azure tablice i entiteti tablice
Servis za pohranu za Azure tablice je NoSQL datastore koji koristite za pohranu i upit velikog skupa strukturirane, koji nisu relacijskih podataka. Glavne komponente servisa su tablicama, entiteti i svojstva. Tablica je zbirka entiteti. Entitet je skup svojstava. Svaki entitet može sadržavati do 252 svojstva koja su svi parovi vrijednost za naziv. U ovom se odjeljku pretpostavlja da ste već upoznati s koncepata servis za pohranu za Azure tablice. Detaljne informacije potražite u članku [Osnove podatkovnog modela servisa u tablicu](http://msdn.microsoft.com/library/azure/dd179338.aspx) i [Početak rada sa spremištem tablica Azure pomoću .NET](storage-dotnet-how-to-use-tables.md).

U Pododlomci koji slijede ćete saznati kako upravljati servis za pohranu tablice Azure pomoću komponente PowerShell Azure. Scenariji u kojima je moguće uvrstiti **Stvaranje**, **Brisanje**i **Dohvaćanje** **tablice**, kao i **Dodavanje**, **upita**i **Brisanje entiteti tablice**.

### <a name="how-to-create-a-table"></a>Upute za stvaranje tablice
Svakom tablicom moraju se nalaziti u račun za Azure prostora za pohranu. Sljedeći primjer pokazuje kako stvoriti tablice u Azure prostora za pohranu. U primjeru najprije uspostavlja vezu za pohranu Azure pomoću računa kontekst za pohranu koji sadrži naziv računa za pohranu i njegov tipkovni prečac. Nakon toga koristi cmdleta [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) za stvaranje tablice u Azure prostora za pohranu.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Create a new table.
    $tabName = "yourtablename"
    New-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-retrieve-a-table"></a>Kako dohvatiti tablice
Možete upita i dohvaćanje jedne ili svih tablica u račun za pohranu. Sljedeći primjer pokazuje kako dohvatiti navedene tablice pomoću cmdleta [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) .

    #Retrieve a table.
    $tabName = "yourtablename"
    Get-AzureStorageTable –Name $tabName –Context $Ctx

Ako poziv cmdlet Get-AzureStorageTable bez parametre, dohvaća sve tablice za pohranu za račun za pohranu.

### <a name="how-to-delete-a-table"></a>Brisanje tablice
Brisanje tablice s računa za pohranu pomoću cmdleta [Ukloni AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806393.aspx) .  

    #Delete a table.
    $tabName = "yourtablename"
    Remove-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-manage-table-entities"></a>Kako upravljati entiteti tablice
Azure PowerShell trenutno ne nudi Cmdlete da biste izravno upravljanje entiteti tablice. Za izvođenje operacije na entiteti tablice, možete koristiti klase u [Azure prostora za pohranu klijenta biblioteke za .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-to-add-table-entities"></a>Kako dodati tablicu entiteti
Da biste tablicu dodali entitet, stvorite objekt koji definira entitet svojstva. Entitet mogu imati do 255 svojstva, uključujući 3 svojstva sustava: **PartitionKey**, **RowKey**i **vremenske oznake**. Vi ste odgovorni za umetanje i ažuriranje vrijednosti **PartitionKey** i **RowKey**. Poslužitelj upravlja vrijednost **vremenske oznake**, koje nije moguće mijenjati. Zajednički **PartitionKey** i **RowKey** identificirati samo svaki entitet unutar tablice.

-   **PartitionKey**: određuje particija entitet pohranjena u.
-   **RowKey**: služi kao jedinstvena identifikacija entitet unutar particije.

Možete definirati do 252 prilagođena svojstva za entitet. Dodatne informacije potražite u članku [Osnove podatkovnog modela servisa u tablici](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Sljedeći primjer pokazuje kako dodati entiteti u tablicu. U primjeru prikazano kako dohvatiti tablice zaposlenika i dodavanje nekoliko entiteti u nju. Najprije uspostavlja vezu za pohranu Azure pomoću računa kontekst za pohranu koji sadrži naziv računa za pohranu i njegov tipkovni prečac. Nakon toga dohvaća danoj tablici pomoću cmdleta [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) . Ako ne postoji u tablici, cmdleta [New AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) se koristi za stvaranje tablice u Azure prostora za pohranu. Nakon toga u primjeru definira prilagođenu funkciju Dodaj entitet da biste dodali entiteti tablici navođenjem svaki entitet particija i ključ za redak. Funkcija Dodaj entitet poziva cmdleta [New objekt](http://technet.microsoft.com/library/hh849885.aspx) na klase [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) da biste stvorili objekt entitet. Noviju verziju, na primjer poziva metodu [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) na taj objekt entitet da biste ga dodali u tablicu.

    #Function Add-Entity: Adds an employee entity to a table.
    function Add-Entity() {
        [CmdletBinding()]
        param(
           $table,
           [String]$partitionKey,
           [String]$rowKey,
           [String]$name,
           [Int]$id
        )

      $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
      $entity.Properties.Add("Name", $name)
      $entity.Properties.Add("ID", $id)

      $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
    }

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Retrieve the table if it already exists.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

    #Create a new table if it does not exist.
    if ($table -eq $null)
    {
       $table = New-AzureStorageTable –Name $TableName -Context $Ctx
    }

    #Add multiple entities to a table.
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4

#### <a name="how-to-query-table-entities"></a>Kako entiteti tablice upita
Da biste upit tablice, koristite klasa [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) . U sljedećem primjeru pretpostavlja da ste već pokrenuli skripte navedene u članku dodavanje entiteti odjeljku ovog vodiča. U primjeru najprije uspostavlja vezu za pohranu Azure pomoću kontekst za pohranu koji sadrži naziv računa za pohranu i njegov tipkovni prečac. Ga pokušava dohvatiti tablici "Zaposlenici" prethodno stvorena pomoću cmdleta [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) . Pozivanje cmdleta [New objekt](http://technet.microsoft.com/library/hh849885.aspx) na Microsoft.WindowsAzure.Storage.Table.TableQuery predmete stvara novi upit objekt. Primjer izgleda sinkronizacije koje imaju stupac 'ID' čija je vrijednost 1 kao što je navedeno u filtru niz. Detaljne informacije potražite u članku [Postavljanje upita tablice i entiteti](http://msdn.microsoft.com/library/azure/dd894031.aspx). Kada pokrenete upit, vraća se svi entiteti koji zadovoljavaju kriterije filtriranja.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Get a reference to a table.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx

    #Create a table query.
    $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

    #Define columns to select.
    $list = New-Object System.Collections.Generic.List[string]
    $list.Add("RowKey")
    $list.Add("ID")
    $list.Add("Name")

    #Set query details.
    $query.FilterString = "ID gt 0"
    $query.SelectColumns = $list
    $query.TakeCount = 20

    #Execute the query.
    $entities = $table.CloudTable.ExecuteQuery($query)

    #Display entity properties with the table format.
    $entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize

#### <a name="how-to-delete-table-entities"></a>Kako izbrisati tablicu entiteti
Možete izbrisati entitet pomoću tipki njegov particija i redaka. U sljedećem primjeru pretpostavlja da ste već pokrenuli skripte navedene u članku dodavanje entiteti odjeljku ovog vodiča. U primjeru najprije uspostavlja vezu za pohranu Azure pomoću kontekst za pohranu koji sadrži naziv računa za pohranu i njegov tipkovni prečac. Ga pokušava dohvatiti tablici "Zaposlenici" prethodno stvorena pomoću cmdleta [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) . Ako postoji u tablici, na primjer poziva metodu [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) za dohvaćanje entitet particija i redak ključa vrijednosti na temelju. Nakon toga prenesite entitet [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) način da biste izbrisali.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the table.
    $TableName = "Employees"
    $table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

    #If the table exists, start deleting its entities.
    if ($table -ne $null) {
       #Together the PartitionKey and RowKey uniquely identify every  
       #entity within a table.
       $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
       $entity = $tableResult.Result
    if ($entity -ne $null)
    {
       #Delete the entity.
       $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
    }

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Kako upravljati Azure redovima i reda čekanja poruke
Azure reda čekanja za pohranu je servis za pohranu velikog broja poruka koje je moguće pristupiti s bilo kojeg mjesta na svijetu putem čija je autentičnost provjerena poziva pomoću HTTP ili HTTPS. U ovom se odjeljku pretpostavlja da ste već upoznati s servis za pohranu reda čekanja Azure koncepata. Detaljne informacije potražite u članku [Početak rada s pohranu reda čekanja Azure pomoću .NET](storage-dotnet-how-to-use-queues.md).

U ovom se odjeljku vidjet ćete kako upravljati servis za pohranu red Azure pomoću komponente PowerShell Azure. Scenariji u kojima je moguće uvrstiti **umetanja** i **brisanja** reda čekanja poruke kao i **Stvaranje**, **Brisanje**i **Dohvaćanje redova**.

### <a name="how-to-create-a-queue"></a>Stvaranje reda čekanja
U sljedećem primjeru najprije uspostavlja vezu za pohranu Azure pomoću računa kontekst za pohranu koji sadrži naziv računa za pohranu i njegov tipkovni prečac. Nakon toga poziva cmdleta [New-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806382.aspx) da biste stvorili red pod nazivom "queuename".

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $QueueName = "queuename"
    $Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx

Informacije o konvencije imenovanja za servis reda čekanja Azure potražite u članku [imenovanja redovima i metapodacima](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Kako dohvatiti reda čekanja
Možete upit i dohvatili određeni red ili popis redova na računu za pohranu. Sljedeći primjer pokazuje kako dohvatiti navedeni reda čekanja pomoću cmdleta [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) .

    #Retrieve a queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx

Ako poziv cmdlet [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) bez parametre, dohvaća se popis svih redova.

### <a name="how-to-delete-a-queue"></a>Kako izbrisati reda čekanja
Da biste izbrisali red i sve poruke koje se nalaze u njoj, nazovite cmdlet Ukloni AzureStorageQueue. Sljedeći primjer prikazuje način da biste izbrisali navedeni reda čekanja pomoću cmdleta Ukloni AzureStorageQueue.

    #Delete a queue.
    $QueueName = "yourqueuename"
    Remove-AzureStorageQueue –Name $QueueName –Context $Ctx

#### <a name="how-to-insert-a-message-into-a-queue"></a>Kako umetnuti poruku u redu čekanja
Da biste umetnuli postojeću reda čekanja poruke, stvorite novu instancu klase [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) . Nakon toga pozovite metodu [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) . Na CloudQueueMessage mogu se kreirati niza (u obliku UTF-8) ili raspon bajtova.

Sljedeći primjer pokazuje kako dodati poruku u redu. U primjeru najprije uspostavlja vezu za pohranu Azure pomoću računa kontekst za pohranu koji sadrži naziv računa za pohranu i njegov tipkovni prečac. Nakon toga dohvaća navedeni red čekanja pomoću cmdleta [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) . Ako postoji reda, cmdleta [New objekt](http://technet.microsoft.com/library/hh849885.aspx) se koristi za stvaranje instanca klase [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) . Noviju verziju, na primjer poziva metodu [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) na taj objekt poruku da biste ga dodali u red. Evo kod koji dohvaća podatke u redu čekanja i umeće poruka "MessageInfo":

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    #If the queue exists, add a new message.
    if ($Queue -ne $null) {
       # Create a new message using a constructor of the CloudQueueMessage class.
       $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

       #Add a new message to the queue.
       $Queue.CloudQueue.AddMessage($QueueMessage)
    }


#### <a name="how-to-de-queue-at-the-next-message"></a>Upute za uklanjanje red čekanja na sljedeću poruku
Kod deaktivirali redovi poruke iz reda čekanja u dva koraka. Kada pozovete metodu [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) dobiti sljedeću poruku u redu. Ostale kodove čitanju poruka iz ovog reda čekanja nevidljivim vratio **GetMessage** poruke. Da biste dovršili uklanjanje poruka iz reda čekanja, morate pozvati i metodu [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) . Dva koraka na sljedeći uklanjanja poruke pokazuje da ne uspijete koda za obradu poruku zbog kvara hardver ili softver, drugoj instanci koda možete se ista poruka i pokušajte ponovno. Kod poziva **DeleteMessage** desno kada poruka bude obrađen.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    $InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

    #Get the message object from the queue.
    $QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
    #Delete the message.
    $Queue.CloudQueue.DeleteMessage($QueueMessage)

## <a name="how-to-manage-azure-file-shares-and-files"></a>Kako upravljati zajedničke Azure datoteke i datoteke
Azure pohrani nudi zajedničkog prostora za pohranu za aplikacije pomoću standardnih SMB protokola. Virtualnim računalima sustava Microsoft Azure i servise u oblaku možete omogućiti zajedničko korištenje podataka iz datoteke putem aplikacije komponente putem postavljenu dionice, a lokalni aplikacije može pristupiti podacima datoteka u zajedničko korištenje putem API za pohranu datoteka ili Azure PowerShell.

Dodatne informacije o pohrani Azure potražite u članku [Početak rada s Azure pohrani u sustavu Windows](storage-dotnet-how-to-use-files.md) , a [Datoteka servisa REST API -JA](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>Kako postaviti i analize upita za pohranu
[Azure prostora za pohranu analize](storage-analytics.md) možete koristiti za prikupljanje metriku za Azure prostora za pohranu računi i podaci iz zapisnika o zahtjeva za poslanu na račun za pohranu. Prostor za pohranu metrika možete koristiti za praćenje stanja na račun za pohranu i zapisivanje prostora za pohranu za dijagnosticiranje i otklanjanje poteškoća s računa za pohranu.
Prema zadanim postavkama metrike pohrane nije omogućen za servise za pohranu. Možete omogućiti praćenje pomoću portala za Azure ili komponente Windows PowerShell ili programski pomoću biblioteke za pohranu klijenta. Spremanje zapisnika se dogodi poslužiteljsko i omogućuje pojedinosti zapisa uspješnih i neuspješnih zahtjeva na vašem računu za pohranu za. Ti zapisnici omogućuju vam da biste vidjeli detalje o pročitajte temu, napisati i brisanje operacije odnosu tablica, redove, i blob polja kao i razloge neuspjelih zahtjeva.

Da biste saznali kako omogućiti i prikaz metrike pohrane podataka pomoću komponente PowerShell potražite u [članku Omogućivanje metrike pohrane pomoću komponente PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Da biste saznali kako omogućiti i dohvaćanje prostora za pohranu zapisivanje podataka pomoću komponente PowerShell potražite u članku [kako omogućiti za pohranu zapisivanje pomoću komponente PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) i [Pronalaženje podatke iz zapisnika prijave za pohranu](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Detaljnije informacije o korištenju metrike pohrane i prijava za pohranu za otklanjanje poteškoća za pohranu potražite u članku [nadzor, dijagnosticiranje, i otklanjanje poteškoća s Microsoft Azure prostora za pohranu](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Kako upravljati zajednički pristup potpis (SAS) i spremljene pravilnik o pristupu
Zajednički pristup potpisi su važan dio model sigurnosti za sve aplikacije koje koriste Azure prostora za pohranu. Su potrebne za pružanje ograničene dozvole na račun servisa za pohranu za klijente koje smiju ključ za račun. Samo vlasnik računa za pohranu po zadanom mogu pristupati blob-ova, tablice i redova u taj račun. Ako usluga ili aplikacija treba koristiti ove resurse u drugim klijentima bez omogućivanja zajedničkog korištenja tipka za pristup, postoje tri mogućnosti:

- Postavljanje dozvola u spremniku za anonimni pristup čitanje spremnik i njegov blob-ova. To nije dopuštena za tablice ili redovi.
- Koristite zajednički pristup potpis daje ograničena prava pristupa za spremnika, blob-ova, redovima i tablice za određeni vremenski interval.
- Pomoću pravila za spremljene pristup da biste dobili dodatnu razinu kontrole nad zajednički pristup potpisa za spremnik ili njegov blob-ova, reda ili tablicu. Pravila pohranjene access omogućuje promjenu vrijeme početka, vrijeme isteka ili dozvole za potpis ili da biste opozvali nakon njega sredstava.

Zajednički pristup potpis može se na jedan od dva oblika:

- **Ad-hoc SAS**: kada stvarate ad-hoc SAS, vrijeme početka, vrijeme isteka i dozvole za na SAS su sve navedene SAS URI. Ovu vrstu SAS mogu stvarati klijente na spremnik, blob, tablice ili reda čekanja, i to je koji nisu revokable.
- **SAS s pravilima pohranjene pristupa**: pravilnik pohranjene pristupa je definiran na resursa spremnik spremniku blob, tablicu ili red - i koristite da biste upravljali ograničenja za jedan ili više potpisa zajednički pristup. Kada SAS pridružiti pohranjene pristup pravila, u SAS nasljeđuje ograničenja - vrijeme početka, vrijeme isteka i - definiranih dozvola za pravilnik pohranjene pristupa. Ta vrsta SAS je revokable.

Dodatne informacije potražite u članku [Korištenje zajednički pristup potpisa (SAS)](storage-dotnet-shared-access-signature-part-1.md) i [Upravljanje anonimni pristup za čitanje spremnika i blob-ova](storage-manage-access-to-resources.md).

U sljedećim odjeljcima će Saznajte kako stvoriti pravilo tokena i spremljene pristup zajednički pristup potpis za Azure tablice. Azure PowerShell sadrži slične cmdleta za spremnika, blob-ova i redova kao i. Da biste pokrenuli skripte u ovom odjeljku, preuzmite [Azure PowerShell verzije 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) ili noviji.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>Kako stvoriti zajednički pristup potpis token utemeljen na pravila
Pomoću cmdleta New AzureStorageTableStoredAccessPolicy stvaranje novog pravila pohranjene programa access. Nakon toga nazovite cmdleta [New AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) da biste stvorili novi potpis token zajednički pristup na temelju pravila za pohranu Azure tablicu.

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
    New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx

### <a name="how-to-create-an-ad-hoc-non-revokable-shared-access-signature-token"></a>Kako stvoriti ad-hoc (koji nisu-revokable) token za potpis za zajedničko korištenje programa Access
Pomoću cmdleta [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) da biste stvorili novi ad-hoc (koji nisu-revokable) zajednički pristup potpis token za pohranu Azure tablicu:

    New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx

### <a name="how-to-create-a-stored-access-policy"></a>Kako stvoriti pravilo pohranjene programa access
Pomoću cmdleta New-AzureStorageTableStoredAccessPolicy stvaranje novog pravila pohranjene pristup za pohranu Azure tablicu:

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx

### <a name="how-to-update-a-stored-access-policy"></a>Kako ažurirati pravilnik pohranjene pristupa
Pomoću cmdleta skup AzureStorageTableStoredAccessPolicy da biste ažurirali postojećeg pohranjene pristup pravila za pohranu Azure tablicu:

    Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx

### <a name="how-to-delete-a-stored-access-policy"></a>Kako izbrisati pohranjena pristup pravila
Brisanje pravila pristup pohranjene na tablici prostora za pohranu Azure pomoću cmdleta Ukloni AzureStorageTableStoredAccessPolicy:

    Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx


## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Kako koristiti Azure prostora za pohranu za vlada SAD-a i Kine Azure
Azure okruženje je nezavisne implementaciju sustava Microsoft Azure, kao što su [Državne Azure za vlada SAD-a](https://azure.microsoft.com/features/gov/), [AzureCloud za globalnu Azure](https://portal.azure.com)i [AzureChinaCloud za Azure kojim upravlja 21vianet u Kini](http://www.windowsazure.cn/). Možete implementirati novi Azure okruženja za državne ustanove Američka i Kine Azure.

Da biste koristili za pohranu Azure s AzureChinaCloud, potrebnih za stvaranje kontekstu prostora za pohranu koji je pridružen AzureChinaCloud. Slijedite ove korake za početak:

1.  Pokrenite cmdlet [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) da biste vidjeli dostupne Azure okruženja:

    `Get-AzureEnvironment`

2.  Dodavanje računa Kine Azure Windows PowerShell:

    `Add-AzureAccount –Environment AzureChinaCloud`

3.  Stvaranje kontekst za pohranu za račun AzureChinaCloud:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud

Da biste koristili za pohranu Azure s [Vlade sad Azure](https://azure.microsoft.com/features/gov/), definiranje novo okruženje i zatim stvorite novi prostor za pohranu kontekst okruženje za ovo:

1.  Pokrenite cmdlet [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) da biste vidjeli dostupne Azure okruženja:

    `Get-AzureEnvironment`

2.  Dodavanje računa Azure NAM državne Windows PowerShell:

    `Add-AzureAccount –Environment AzureUSGovernment`

3.  Stvaranje kontekst za pohranu za račun AzureUSGovernment:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment

Dodatne informacije potražite u članku:

- [Vodič za razvojne inženjere za državne Microsoft Azure](../azure-government-developer-guide.md).
- [Pregled razlike prilikom stvaranja aplikacije na servis Kina](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Daljnji koraci
U ovom vodiču ste naučili kako upravljanje pohranom Azure s Azure PowerShell. Evo nekoliko srodnih članaka i resursi za učenje više o njima.

- [Dokumentacija Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/)
- [Cmdleta ljuske PowerShell Azure prostora za pohranu](http://msdn.microsoft.com/library/azure/dn806401.aspx)
- [Referenca za Windows PowerShell](https://msdn.microsoft.com/library/ms714469.aspx)

[Image1]: ./media/storage-powershell-guide-full/Subscription_currentportal.png
[Image2]: ./media/storage-powershell-guide-full/Subscription_Previewportal.png
[Image3]: ./media/storage-powershell-guide-full/Blobdownload.png


[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
