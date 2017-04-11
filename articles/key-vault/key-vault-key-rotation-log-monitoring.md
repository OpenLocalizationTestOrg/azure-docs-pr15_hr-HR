<properties
    pageTitle="Kako postaviti sigurnog tipku end da biste završetka ključa zakretanje i nadzor | Microsoft Azure"
    description="Koristite ovaj upute da biste dobili postavljanje ključa zakretanje i ključne sigurnog zapisnika nadzora"
    services="key-vault"
    documentationCenter=""
    authors="swgriffith"
    manager="mbaldwin"
    tags=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="jodehavi;stgriffi"/>
#<a name="how-to-setup-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Kako postaviti sigurnog tipku end da biste završetka ključa zakretanje i nadzor

##<a name="introduction"></a>Uvod

Nakon stvaranja vaše sigurnog ključ Azure moći pokrenuti korištenje tog zbirke ključeva za pohranu ključeva i tajne. Aplikacija više nije potrebna održati tipke ili tajne, ali umjesto će zatražiti ih s ključa sigurnog prema potrebi. Omogućuje ažuriranje ključeva i tajne bez koje utječu na ponašanje aplikacije koja otvara breadth s mogućnostima oko ključ i ponašanje tajnu upravljanje.

U ovom se članku vodi kroz primjera korištenje Azure sigurnog ključ za pohranu tajna, u ovom slučaju ključa račun za pohranu Azure kojoj se pristupa putem aplikacije. Će demonstrirati i implementacija zakazani zakretanje taj ključ računa za pohranu. Na kraju, ona će voditi kroz pokazni o i praćenje zapisnike nadzora ključa sigurnog Potenciranje upozorenja kada se neočekivana zahtjeva.

> \[AZURE. Napomena\] ovog praktičnog vodiča nije namijenjen objašnjenja detaljno početni skup prema gore od vašeg sigurnog ključ Azure. Ove informacije potražite u članku [Početak rada s sigurnog ključ Azure](key-vault-get-started.md). Ili sučelje naredbenog retka za različite platforme upute potražite u članku [ovom ćete praktičnom vodiču ekvivalentan](key-vault-manage-with-cli.md).

##<a name="setting-up-keyvault"></a>Postavljanje KeyVault

Da biste omogućili aplikacije za dohvaćanje na tajna iz zbirke ključeva Azure ključ, najprije morate stvoriti na tajna i prenese na vaše zbirke ključeva. To je moguće napraviti jednostavno PowerShell kao što je prikazano u nastavku.

Pokretanje sesije razmjene Azure PowerShell i prijavite se na račun za Azure pomoću sljedeće naredbe:

```powershell
Login-AzureRmAccount
```

U prozoru preglednika skočni unesite račun za Azure korisničko ime i lozinku. Azure PowerShell će se sve pretplate koje su povezane s ovim računom i prema zadanim postavkama, koristi prvoga.

Ako imate više pretplata, možda ćete morati odrediti onaj koji je korišten za stvaranje vaše sigurnog ključ Azure. Upišite sljedeće da biste vidjeli popis pretplata za vaš račun:

```powershell
Get-AzureRmSubscription
```

Zatim, da biste odredili pretplate pridružen vaše ključa sigurnog će zapisivanje, upišite:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID> 
```

Kao u ovom se članku objašnjava pohrani ključ računa za pohranu kao na tajna, morat ćete dobiti ključ za račun taj prostor za pohranu.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Nakon preuzimanja sustava tajna, u tom slučaju ključ za račun za pohranu morate pretvorite koji sigurne niz, a zatim stvorite na tajna s tom vrijednošću u vašem ključa sigurnog.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Sljedeće želite dobiti URI za tajna koji ste upravo stvorili. To će se koristiti u noviji koraku kada zovete sigurnog ključ dohvatiti vaše tajna. Pokrenite sljedeću naredbu komponente PowerShell i zabilježite vrijednost "Id", tajnu URI.

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

##<a name="setting-up-application"></a>Postavljanje aplikacije

Sad kad ste tajna pohranjene želite dohvatiti te tajna te ga koristiti iz koda. Postoji nekoliko koraka potrebne da biste postigli to ime i najvažnije koji je Registracija aplikacije pomoću servisa Azure Active Directory i zatim koja upozorava Azure ključ sigurnog podataka za aplikaciju tako da ga možete omogućiti zahtjeve iz aplikacije.

> \[AZURE. Napomena\] aplikacije moraju se stvoriti na istom klijentu Azure Active Directory kao vaše sigurnog ključ. 

Najprije otvorite karticu aplikacije Azure Active Directory

![Aplikacije koje su otvorene u Azure AD](./media/keyvault-keyrotation/AzureAD_Header.png)

Odaberite Dodaj da biste dodali novu aplikaciju za Azure AD

![Odaberite dodavanje aplikacije](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Ostavite vrsta aplikacije kao "Web aplikacije AND/OR API WEB", a zatim dajte naziv aplikacije.

![Naziv aplikacije](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Dajte aplikacije "Prijave URL-a" i u aplikaciji URI-ID-a. To može biti bilo što želite za demonstraciju, a možete kasnije promijeniti ako je potrebno.

![Navedite potrebne ji](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Kada dodate aplikaciju za Azure AD, će biti uvezeni u stranici aplikacije. Od tog trenutka na kartici "Konfiguriraj" kliknite, a zatim pronađite i kopirajte vrijednost "ID klijenta". Zabilježite ID klijenta za kasnije korake.

Morat ćete stvaranje ključa za svoju aplikaciju da biste mogli raditi s Azure AD dalje. To možete stvoriti u odjeljku "Tipke" na kartici "Konfiguracija". Zabilježite u novi generirani ključ iz aplikacije za Azure AD za korištenje u noviji korak.

![Prečaci za aplikaciju servisa Azure AD](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Prije uspostavljanja sve pozive iz aplikacije u ključ sigurnog morat ćete prepoznati sigurnog ključ o aplikaciji i njezinu ' dozvole. Sljedeća naredba uzima naziv sigurnog i ID klijenta iz aplikacije za Azure AD i dopušta 'dobiti pristup vašem ključa sigurnog za aplikaciju.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Sada ste spremni da biste počeli raditi pozive aplikacije. U aplikaciji će najprije morate instalirati paketa NuGet potrebne za interakciju s sigurnog ključ Azure i Azure Active Directory. S konzole Upravitelj paketa za Visual Studio unesite sljedeće naredbe. Imajte na umu da na tekst koji unosite u ovom se članku trenutnu verziju paketa konzolu ActiveDirectory 3.10.305231913, da bi se trebali biste potvrdili najnoviju verziju i ažuriranje sukladno tome.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

U kodu aplikacije stvorite razred tako da držite načina provjere autentičnosti na servisu Active Directory. U ovom se primjeru klase se zove "Uslužni". Zatim morat ćete dodati pomoću sljedeće.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Zatim dodajte sljedeće način za dohvaćanje JWT tokena iz Azure AD. Za maintainability preporučujemo vam da biste premjestili na tvrdi programiranja vrijednosti niza u konfiguraciji weba ili računala.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Na kraju, možete dodati potreban kod da biste pozvali sigurnog ključ ili dohvatiti tajnu vrijednosti. Najprije morate dodati na sljedeći način pomoću izjave.

```csharp
using Microsoft.Azure.KeyVault;
```

Sljedeće će dodati pozive način za pozivanje sigurnog ključ i dohvaćanje vaše tajna. U ovu metodu dat će tajna URI koji ste spremili u prethodnom koraku. Imajte na umu pomoću metode GetToken iz klase uslužni stvorena iznad.
    
```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Kada pokrenete aplikaciju, koje sada bi trebala biti provjeru autentičnosti Azure Active Directory, a zatim dohvaćanja tajnu vrijednosti iz vaše zbirke ključeva ključ Azure.

##<a name="key-rotation-using-azure-automation"></a>Zakretanje ključ korištenju automatizacije Azure

Dostupne su različite mogućnosti za implementaciju zakretanje Strategije za vrijednosti koji se spremaju kao tajne sigurnog ključ Azure. Tajne može zakretati kao dio ručnog procesa, oni mogu biti zakreće programatically za korištenje API poziva ili može se rotirati putem skripte automatizaciju. Za potrebe ovog članka možemo će biti korištenje Azure PowerShell u kombinaciji s Azure Automatizacija da biste promijenili tipkovnog Azure prostora za pohranu računa, a zatim ćemo ažurirati ključa sigurnog tajna s tog novog ključa. 

Da bi se omogućuje automatizaciju Azure da biste postavili tajnu vrijednosti u vašem ključa sigurnog morat ćete dobiti ID klijenta za vezu pod nazivom "AzureRunAsConnection", koji je stvoren dok pokrenut na instancu Azure automatizaciju. Odabirom 'Imovine' vašoj instanci Azure Automatizacija možete pristupiti tim ID-om. Iz nje odaberite "Veze", a zatim odaberite servis načelo 'AzureRunAsConnection'. Želite uzeti u obzir "ID aplikacije". 

![ID klijenta Azure automatizacije](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Dok ste u prozoru resursi će preporučujemo i da biste odabrali "Moduli". Iz modula će odaberite "Galerija" i potražite i 'Uvoz' ažurirati verzije svakog od sljedećih module.

    Azure
    Azure.Storage   
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage
    
> \[AZURE. Napomena\] na tekst koji unosite u ovom se članku samo iznad zabilježili moduli potreban ažurirati skripte zajednički se koristi ispod. Ako pronađete ne daje Automatizacija posla, trebali biste provjerite da imate sve potrebne module i njihove ovisnosti uvesti.

Nakon što ste dohvatili ID aplikacije za automatizaciju Azure vezu, morat ćete prepoznati vaš sigurnog Azure ključ ima li ovu aplikaciju programa access da biste ažurirali tajne u vaše zbirke ključeva. To je moguće napraviti pomoću sljedeće naredbe ljuske PowerShell.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Zatim će odaberite resurs 'Runbooks' u odjeljku vašoj instanci Automatizacija Azure i odaberite "Add na Runbook". Odaberite 'Brzo stvaranje'. Naziv vaše runbook i odaberite "PowerShell" kao vrstu runbook. Po želji dodajte opis. Na kraju, kliknite "Stvori".

![Stvaranje Runbook](./media/keyvault-keyrotation/Create_Runbook.png)

U oknu uređivača za svoje nove runbook će zalijepite sljedeću skriptu komponente PowerShell.

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -StorageAccountName $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

U oknu uređivača možete odabrati "Test okno" da biste testirali skriptu. Kada je pokrenut skriptu bez pogreške možete odabrati mogućnosti "Objavi", a zatim primijenite raspored za runbook na oknu runbook konfiguracije.

##<a name="key-vault-auditing-pipeline"></a>Nadzor za ključ sigurnog kanala

Prilikom postavljanja sustava sigurnog ključ Azure možete uključiti nadzor prikupljanje zapisnika na zahtjeve za pristup pokušaj sigurnog ključ. Ti zapisnici se pohranjuju u određenoj račun za Azure prostora za pohranu i možete zatim povlačenje, nadzire i analizirati. Ispod sferama kroz scenarij u kojem se upravlja Azure funkcije Azure logike aplikacije i sigurnog ključ nadzor zapisnika da biste stvorili kanala da biste poslali poruku e-pošte kad tajne iz na sigurnog dohvaća aplikaciju koja odgovara ID-u aplikaciji web-aplikacije.

Najprije morate omogućiti prijave na sigurnog ključ. To možete učiniti putem sljedeće naredbe ljuske PowerShell (sve detalje mogu vidjeti [ovdje](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>' 
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Kada je omogućen, zatim zapisnike nadzora počet će prikupljanje u obzir zaduženog za pohranu. Ti zapisnici će sadržavati događaja o tome kako i kada se pristupa vašem sefovi ključ i tko. 

> \[AZURE. Napomena\] podataka za zapisivanje mogu pristupiti najviše, 10 minuta nakon tipku sigurnog operacija. U većini slučajeva, bit će brže nego to.

Sljedeći je korak da biste [stvorili red čekanja za Bus servisa Azure](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Ta će se mjesto zapisnike nadzora ključa sigurnog pomiču se. Kad na u redu čekanja aplikaciju logike će ih obraditi i djelovanje na njima. Da biste stvorili servisa Bus je razmjerno klikom naprijed i u nastavku su navedeni koraci za više razine:

1. Stvorite prostor naziva Bus servis (Ako već imate onaj koji želite koristiti za ovu, a zatim prijeđite na drugi korak 2).
2. Pronađite tržišta servisa na portalu za pa odaberite mjesto za naziv koji želite stvoriti reda čekanja u.
3. Odaberite novo, a zatim servis Bus -> u redu, a zatim unesite potrebne podatke.
4. Privucite podatke za povezivanje servisa tržišta tako da odaberete prostora za naziv, a zatim kliknete _Podatke o vezi_. Trebat će ti podaci za sljedeće dio.

Nakon toga će [stvoriti funkciji Azure](../azure-functions/functions-create-first-azure-function.md) ankete zapisnike ključ sigurnog subjekta za pohranu i skupljanje nove događaje. To će biti funkcija koja se pokreće na raspored.

Stvaranje funkciji Azure (odaberite novo -> funkcija aplikacije na portalu). Tijekom stvaranja možete koristiti postojeće hostinga plan ili stvorite novi. Nije moguće odabrati i za dinamičku nalaze. Dodatne informacije o funkcija koje se nalaze mogućnosti možete pronaći [ovdje](../azure-functions/functions-scale.md).

Pri stvaranju Azure funkcija dođite do ga, a zatim je odaberite vremena – opis funkcije i C\# pa kliknite **Stvori** na početnom zaslonu.

![Azure funkcije pokretanje plohu](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Na kartici _razvoju_ zamijenite kod run.csx sljedeće:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging; 
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log) 
{ 
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob) 
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```
> \[AZURE. Napomena\] pripazite da biste zamijenili varijable kod iznad pokažite na račun za pohranu koje se pišu zapisnike sigurnog ključ, Bus servis koji ste ranije stvorili i određene put do zapisnike ključa sigurnog spremišta.

Funkcija uzima najnovije datoteka zapisnika s računa za pohranu koje se pišu zapisnike sigurnog ključ, privlače najnovije događaje iz datoteke i ih gura servisa Bus red. Budući da u jednoj datoteci može imati više događaje, npr. preko cijelog sat, pa ćemo stvoriti datoteku _sync.txt_ koju funkciju pregledava da biste odredili vremenska oznaka posljednje događaja izdvojeni prema gore. To će osigurati da mi ne automatske istog događaja više puta. Datoteka _sync.txt_ jednostavno sadrži vremenske oznake za posljednje je naišao na događaj. Zapisnike, prilikom učitavanja, morate se sortirati prema na vremensku oznaku da bi se ispravno naručili.

Za ovu funkciju reference smo nekoliko dodatnih biblioteke koje nisu dostupne iz okvira u funkcijama Azure. Da biste ih obuhvatili, moramo Azure funkcije da biste izvukli ih pomoću nuget. Odaberite mogućnost _Prikaz datoteka_ 

![Mogućnosti za prikaz datoteka](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

i dodajte novu datoteku pod nazivom _project.json_ sa sadržajem sljedeće:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Nakon _spremite_ to će pokrenuti Azure funkcije da biste preuzeli potrebne binarne datoteke. 

Prijeđite na karticu **Integrate** i parametar mjerača vremena dodijelite smisleni naziv da biste koristili unutar funkcije. U gore navedeni kod očekuje timer pozivanje _myTimer_. Odredite [CRON izraz](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) na sljedeći način: 0 \* \* \* \* \* za timer čega će funkcija koja se pokreće nakon nekoliko minuta. 

U istoj kartici **Integrate** dodajte ulazni koji će biti vrste _Spremište blobova platforme Azure_. Pokažite će _sync.txt_ datoteku koja sadrži vremensku oznaku zadnju gledali pomoću funkcije događaja. To će biti dostupne unutar funkcije naziv parametra. U gore navedeni kod unos spremište blobova platforme Azure očekuje naziv parametra _inputBlob_. Odaberite račun za pohranu gdje će se nalaziti _sync.txt_ datoteke (može biti isti ili računa za pohranu za različite) i u polju put Navedite put gdje datoteku nalazi, u obliku {container-name}/path/to/sync.txt.

Dodajte u izlaz koji će biti s vrstom _Spremište blobova platforme Azure_ izlaz. To će pokazati _sync.txt_ datoteke u unos definirani. To će koristiti funkciju pisati vremenske oznake događaja zadnju gledali. Gore navedeni kod očekuje ovom parametru pozivanje _outputBlob_.

U ovom trenutku funkciju spreman. Provjerite jeste li prebacite se natrag u **razvoju** kartica i _Spremanje_ kod. Provjerite u izlaznom prozoru za sastavljanje pogreške i ispravite one sukladno tome. Ako kompilira, zatim kod sad trebao bi biti pokrenut i svake minute će zapisnicima sigurnog ključ i automatske sve nove događaje na definirani red Bus servisa. Trebali biste vidjeti Zapisnički podaci za pisanje na zapisnika prozor svaki put kada se pokrene funkciju.

###<a name="azure-logic-app"></a>Azure logike aplikacije

Sljedeće moramo stvorite aplikaciju logike Azure koja će obraditi događaja koji je funkciju odaberite red Bus servisa, raščlaniti sadržaj i pošaljite poruku e-pošte na temelju uvjeta koji se podudaraju.

[Stvaranje aplikacije za logiku](../app-service-logic/app-service-logic-create-a-logic-app.md) tako da novo -> logike aplikacije. 

Kada je stvorena aplikacija logike, otvorite ga, a zatim odaberite _Uređivanje_. U uređivaču logike aplikacije odaberite _Red čekanja Bus servisu_ upravljani API za, a zatim unesite vjerodajnice Bus servisa za povezivanje s reda čekanja.

![Bus aplikacije servisa Azure logike](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Zatim odaberite _Dodaj uvjet_. U uvjetu, prebacite se na _Napredni uređivač_ i unesite sljedeće, na APP_ID zamjenjuje stvarni APP_ID web-aplikacije:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Ovaj izraz zapravo će vratiti **false** ako je svojstvo **ID programa** iz dolazne događaja (što je tijela poruke servisa Bus) ne **ID programa** aplikacije. 

Stvorite akciju u odjeljku mogućnost _Ako ne, nemojte učiniti ništa..._ .

![Azure logike aplikacije odaberite akciju](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Akcije, odaberite _Office 365 – slanje e-pošte_. Ispunite polja da biste stvorili poruku e-pošte da biste poslali kad definirani uvjet vraća false. Ako nemate Office 365 ne može pogledati alternative da biste postigli iste.

Sada imate programa završetka na kraj kanala koji će nakon nekoliko minuta, tražiti nove zapisnike nadzora zbirke ključeva ključ. Sve nove zapisnika pronađe ga će automatske ih Bus čekanja za servis. Aplikaciju logike će se pokrenuti čim novu poruku stane u redu čekanja i ako ID programa unutar događaja ne odgovara id aplikacije aplikacija, a zatim pošaljite poruku e-pošte. 
