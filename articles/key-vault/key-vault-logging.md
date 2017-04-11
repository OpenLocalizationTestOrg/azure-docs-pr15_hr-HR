<properties
    pageTitle="Zapisivanje Azure ključa sigurnog | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča pomoći da započnete s Azure ključ sigurnog bilježenje u zapisnik."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/31/2016"
    ms.author="cabailey"/>

# <a name="azure-key-vault-logging"></a>Azure ključa sigurnog zapisivanje #
Azure ključ sigurnog dostupan je u većini područja. Dodatne informacije potražite u članku [ključ sigurnog cijene stranice](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Uvod  
Kada stvorite jedan ili više tipki sefovi, vjerojatno ćete praćenje kako i kada se vaše ključa sefovi pristupa, a tko. To možete učiniti tako da omogućite zapisivanje za sigurnog ključ, čime se štedi informacije u račun Azure prostora za pohranu koji navedete. Novi spremnik pod nazivom **uvida zapisnika auditevent** automatski stvara za vaš račun navedeni prostora za pohranu, a taj isti prostor za pohranu račun možete koristiti za prikupljanje zapisnika za više tipki sefovi.

O zapisivanje mogu pristupiti najviše, 10 minuta nakon tipku sigurnog operacija. U većini slučajeva, bit će brže nego to.  Je prema gore da biste upravljanje vaše zapisnika u račun za pohranu:

- Pomoću standardnih Azure pristup kontrola načina radi zaštite vaše zapisnika ograničavanjem tko može pristupiti.
- Brisanje zapisnika koji više ne želite da ostanu na vašem računu za pohranu.

Pomoću ovog praktičnog vodiča pomoći da započnete s Azure ključ sigurnog prijave, stvorite račun za pohranu, omogućite zapisivanje i tumačenje Zapisnički podaci koji se prikupljaju.  


>[AZURE.NOTE]  Pomoću ovog praktičnog vodiča ne uključuje upute za stvaranje sefovi tipki, tipke ili tajne. Ove informacije potražite u članku [Početak rada s sigurnog ključ Azure](key-vault-get-started.md). Ili sučelje naredbenog retka za različite platforme upute potražite u članku [ovom ćete praktičnom vodiču ekvivalentan](key-vault-manage-with-cli.md).
>
>Trenutno ne možete konfigurirati Azure ključ sigurnog na portalu za Azure. Umjesto toga koristite ove upute za Azure PowerShell.

Zapisnike prikupite se mogu vizualizirati pomoću zapisnika analize iz paketa za upravljanje operacije. Dodatne informacije potražite u članku [rješenja Azure ključ sigurnog (pretpregled) u zapisnik analize](../log-analytics/log-analytics-azure-key-vault.md).

Pregled informacija o sigurnog ključ Azure, potražite u članku [što je sigurnog ključ Azure?](key-vault-whatis.md)

## <a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj Praktični vodič, morate imati sljedeće:

- Za postojeće ključne sigurnog koji koristite.  
- Azure komponente PowerShell **Minimalna verzija 1.0.1**. Da biste instalirali Azure PowerShell i povezati s pretplatom Azure, Saznajte [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md). Ako ste već instalirali Azure PowerShell i ne znate na verziju iz konzole za Azure PowerShell, upišite `(Get-Module azure -ListAvailable).Version`.  
- Dovoljno prostora za pohranu na Azure za vaše ključ sigurnog zapisnika.


## <a id="connect"></a>Povezivanje s pretplate ##

Pokretanje sesije razmjene Azure PowerShell i prijavite se na račun za Azure pomoću sljedeće naredbe:  

    Login-AzureRmAccount

U prozoru preglednika skočni unesite račun za Azure korisničko ime i lozinku. Azure PowerShell će se sve pretplate koje su povezane s ovim računom i prema zadanim postavkama, koristi prvoga.

Ako imate više pretplata, možda ćete morati odrediti onaj koji je korišten za stvaranje vaše sigurnog ključ Azure. Upišite sljedeće da biste vidjeli popis pretplata za vaš račun:

    Get-AzureRmSubscription

Zatim, da biste odredili pretplate pridružen vaše ključa sigurnog će zapisivanje, upišite:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Dodatne informacije o konfiguriranju Azure PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).


## <a id="storage"></a>Stvaranje novog računa za pohranu za vaše zapisnika ##

Iako postojeći račun za pohranu možete koristiti za svoje zapisnike, stvorit ćemo ćete na novi račun za pohranu koji će namjenski zapisnicima sigurnog ključ. Pogodnost za kada imamo navedite ga kasnije ćemo ćete spremiti pojedinosti u varijablu pod nazivom **Pacifička**.

Radi lakšeg upravljanja dodatne, ćemo i koristiti isti grupa resursa kao ona koja sadrži naš ključa sigurnog. Na [uvodni vodič](key-vault-get-started.md), ovu grupu resursa pod nazivom **ContosoResourceGroup** i nastavit koristiti istočnoazijski mjesto. Zamjena te vrijednosti za vlastitih, kao što je primjenjivo:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name ContosoKeyVaultLogs -Type Standard_LRS -Location 'East Asia'


>[AZURE.NOTE]  Ako odlučite da biste koristili postojeći račun za pohranu, morate koristiti istu pretplatu kao vaše ključa sigurnog i morate koristiti model implementacije Voditelj resursa, umjesto model implementacije klasični.

## <a id="identify"></a>Prepoznavanje ključa zbirke ključeva za vaše zapisnika ##

U našem dohvaćanje rada praktičnom vodiču naš ključa sigurnog naziv je **ContosoKeyVault**tako da se i dalje ćemo pomoću tog naziva i spremanje pojedinosti u varijablu pod nazivom **kv**:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>Omogućite zapisivanje ##

Da biste omogućili za ključ sigurnog zapisivanje, ćemo koristiti cmdlet skup AzureRmDiagnosticSetting zajedno s varijable koju smo stvorili za naš novi račun za pohranu i naše ključa sigurnog. Ne možemo ćete postaviti na **-omogućeno** zastavica za **$true** i postavljanje kategorije AuditEvent (samo kategorija za zapisivanje sigurnog ključa):


    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

Izlaz za to uključuje:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


To se potvrđuje da je sada omogućeno zapisivanje za ključne sigurnog, spremanje podataka o računu za pohranu.

Po želji možete i postaviti pravilnika o zadržavanju za vaše zapisnika tako da se stariji zapisnika automatski će se izbrisati. Ako, na primjer, pomoću zastavice **- RetentionEnabled** **$true** pravilnika o zadržavanju i upravljanje parametar **- RetentionInDays** **90** tako da se zapisnici starije od 90 dana automatski će se izbrisati.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Što se zapisuje:

- Prijavljeni ste sve zahtjeve za čija je autentičnost provjerena REST API-JA i koja sadrži neuspjelih zahtjeva uslijed dozvole pristupa, pogreške sustava ili neispravni zahtjeva.
- Operacije na tipku vault zasebno, što obuhvaća i stvaranje, brisanje, a zatim postavke ključa sigurnog pristupa pravila, a ažuriranje ključa sigurnog atribute kao što su oznake.
- Operacije na tipke i tajne u ključa sigurnog, što obuhvaća stvaranje, izmjena i brisanje ove tipke ili tajne; Operacija kao što su znak, provjerite je li, šifriranje, dešifrirati, prelamanje i Ukini prelamanje tipke, tajne, tipke popisa i tajne i njihove verzije.
- Neprovjereni zahtjeva koji kao rezultat 401 odgovor. Na primjer, zahtjevi za koje posjeduju nošenja token, ili oštećen ili istekle ili token koji nije valjan.  


## <a id="access"></a>Pristup vašem zapisnika ##

Zapisnici ključa sigurnog spremaju se u spremniku **uvida zapisnika auditevent** na računu za pohranu koje ste naveli. Da biste dobili popis svih blob-ova u ovom spremniku, upišite:

    Get-AzureStorageBlob -Container 'insights-logs-auditevent' -Context $sa.Context

Izlaz izgledat će otprilike ovako:

**Spremnik Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**


**Ime**

**----**

**resourceId = / PRETPLATE/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/davatelji/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**resourceId = / PRETPLATE/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/davatelji/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

** resourceId = / PRETPLATE/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/davatelji/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json***


Kao što možete vidjeti iz ovog izlaza, slijedite s blob-ova u konvencija imenovanja: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**

Vrijednosti datuma i vremena koristite UTC-a.

Jer isti prostor za pohranu račun može se koristiti za prikupljanje zapisnika za više resursa, ID cijelog resursa u nazivu blob vrlo je praktična za pristup ili preuzimanje samo s blob-ova koje su vam potrebne. No prije toga moramo, ćemo najprije objasniti kako preuzeti sve blob-ova.

Prvo, stvorite mapu u koju želite preuzeti s blob-ova. Ako, na primjer:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Zatim se popis svih blob-ova:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Pipe ovaj popis kroz 'Get-AzureStorageBlobContent"da biste preuzeli s blob-ova u našem odredišnu mapu:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

Kada pokrenete tu drugu naredbu na **/** graničnik u nazivima blob Stvori strukturu cijelog mapa u odjeljku odredišnu mapu, a ovu strukturu će se koristiti za preuzimanje i pohraniti s blob-ova kao datoteke.

Da biste selektivno preuzeli blob-ova, pomoću zamjenskih znakova. Ako, na primjer:

- Ako imate više tipki sefovi i želite preuzeti zapisnika za samo jednu ključa sigurnog, pod nazivom CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3

- Ako imate više grupa resursa i želite da biste preuzeli zapisnika za samo jednu grupu resursa, pomoću `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'

- Ako želite preuzeti sve zapisnike za mjesec siječanj 2016, koristite `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Sada ste spremni za početak pogled na što je u zapisnicima. No prije prelaska na koja dva više parametara za dohvaćanje AzureRmDiagnosticSetting koje morate znati:

- Da biste upit status dijagnostičkih postavki za vaš ključa sigurnog resursa:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`

- Da biste onemogućili zapisivanje za vaše ključa sigurnog resursa:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`


## <a id="interpret"></a>Tumačenje vaše ključ sigurnog zapisnika ##

Pojedinačne blob-ova pohranjene kao tekst oblikovan kao JSON blob. Ovo je primjer unos zapisnika pokretanje `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


U sljedećoj su tablici navedeni polje naziva i opisa.


| Naziv polja        | Opis |
| ------------- |-------------|
| vrijeme      | Datum i vrijeme (UTC).|
| resourceId      | ID Azure resursima resursa. Za zapisnike sigurnog ključ, to je uvijek ID sigurnog ključ resursa.|
| operationName      | Naziv operacije, kao što je navedenih u sljedeću tablicu.|
| operationVersion      | Ovo je verzija REST API-JA zatražio klijent.|
| Kategorija      | Zapisnici sigurnog ključ AuditEvent je vrijednost jedne, koja je dostupna.|
| resultType      | Rezultat zahtjeva za REST API-JA.|
| resultSignature      | HTTP stanje.|
| resultDescription     | Dodatni opis o rezultatu, kada je dostupna.|
| durationMs      | Vrijeme trajanja za servisni zahtjev za REST API-JA u milisekundama. Da bi se vrijeme mjerenje na klijentskoj strani ne odgovaraju ovaj put to ne uključuje latenciju mreže.|
| callerIpAddress      | IP adresa klijenta koji je izvršio zahtjev.|
| correlationId      | Neobavezni GUID koji klijent prenositi za povezivanje klijentski se zapisnici sa zapisnicima davatelj usluge (ključ sigurnog).|
| identitet      | Identitet iz token koji je unesen pri stvaranju zahtjev za REST API-JA. To je obično "korisnik", "servisa glavnica" ili kombinaciju "korisniku + ID programa" kao predmet zahtjeva dobivenima Azure PowerShell cmdlet.|
| Svojstva      | To polje sadrži različite informacije koje se temelje na operacija (operationName). U većini slučajeva sadrži informacije klijenta (useragent niz proslijeđena klijenta), točno REST API-JA URI zahtjeva i HTTP kod stanja. Osim toga, kada neki objekt, vraća se kao rezultat zahtjeva (na primjer, KeyCreate ili VaultGet) adresar će sadržavati ključ URI (kao što je "id"), sigurnog URI ili URI tajna.|




Vrijednosti polja **operationName** su u obliku ObjectVerb. Ako, na primjer:

- Sve operacije ključa sigurnog ste na ' sigurnog`<action>`' oblikovanje, kao što su `VaultGet` i `VaultCreate`.

- Sve operacije ključne su na ' ključ`<action>`' oblikovanje, kao što su `KeySign` i `KeyList`.

- Sve tajne operacije ste na ' tajna`<action>`' oblikovanje, kao što su `SecretGet` i `SecretListVersions`.

U sljedećoj su tablici navedeni operationName i odgovarajuću naredbu REST API-JA.

| operationName        | Naredba REST API-JA |
| ------------- |-------------|
| Provjera autentičnosti      | Putem krajnjoj točki Azure Active Directory|
| VaultGet      | [Informacije o ključa zbirke ključeva](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx)|
| VaultPut      | [Stvaranje ili ažuriranje ključa zbirke ključeva](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx)|
| VaultDelete      | [Brisanje ključa zbirke ključeva](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx)|
| VaultPatch      | [Ažuriranje ključa zbirke ključeva](https://msdn.microsoft.com/library/azure/mt620025.aspx)|
| VaultList      | [Popis svih ključa sefovi u grupu resursa](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx)|
| KeyCreate      | [Stvaranje ključa](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx)|
| KeyGet      | [Informacije o ključa](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx)|
| KeyImport      | [Ključ uvesti u zbirke ključeva](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx)|
| KeyBackup      | [Sigurnosno kopiranje ključa](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).|
| KeyDelete      | [Brisanje ključa](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx)|
| KeyRestore      | [Vraćanje ključa](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx)|
| KeySign      | [Prijavite se pomoću ključa](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx)|
| KeyVerify      | [Provjerite je li s ključem](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx)|
| KeyWrap      | [Prelamanje ključa](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx)|
| KeyUnwrap      | [Ukini prelamanje ključa](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx)|
| KeyEncrypt      | [Šifriraj pomoću ključa](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx)|
| KeyDecrypt      | [Dešifriranje s ključem](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx)|
| KeyUpdate      | [Ažuriranje ključa](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx)|
| KeyList      | [Popis tipki u na zbirke ključeva](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx)|
| KeyListVersions      | [Popis verzija ključa](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx)|
| SecretSet      | [Stvaranje na tajna](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx)|
| SecretGet      | [Početak tajnu](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx)|
| SecretUpdate      | [Ažuriranje programa tajna](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx)|
| SecretDelete      | [Brisanje na tajna](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx)|
| SecretList      | [Popis tajne u na zbirke ključeva](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx)|
| SecretListVersions      | [Verzija programa tajna popisa](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx)|




## <a id="next"></a>Daljnji koraci ##

Praktični vodič koji koristi Azure ključ sigurnog u web-aplikacije, potražite u članku [Korištenje Azure ključ sigurnog iz web-aplikacije](key-vault-use-from-web-application.md).

Programiranje reference, potražite u članku [Vodič za Azure ključ sigurnog programiranje](key-vault-developers-guide.md).

Popis Azure PowerShell 1.0 cmdleti za Azure ključ sigurnog, potražite u članku [Cmdleti za sigurnog ključ za Azure](https://msdn.microsoft.com/library/azure/dn868052.aspx).

Praktični vodič na ključa zakretanja i zapisnika nadzora s sigurnog Azure ključ, potražite [u](key-vault-key-rotation-log-monitoring.md)članku postavljanje sigurnog tipku end da biste završetka ključa zakretanje i nadzor.
