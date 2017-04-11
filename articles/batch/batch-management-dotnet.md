<properties
    pageTitle="Upravljanje resursima pomoću grupe za upravljanje .NET računa | Microsoft Azure"
    description="Stvaranje, brisanje i izmjena grupe za Azure resursi račun s bibliotekom grupe za upravljanje .NET."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/19/2016"
    ms.author="marsma"/>

# <a name="manage-azure-batch-accounts-and-quotas-with-batch-management-net"></a>Upravljanje grupe za Azure računi i kvote za obradu upravljanje .NET

> [AZURE.SELECTOR]
- [Portal za Azure](batch-account-create-portal.md)
- [Upravljanje grupe za .NET](batch-management-dotnet.md)

Možete smanjite održavanja indirektni u grupe za Azure aplikacija pomoću [Grupe za upravljanje .NET] [ api_mgmt_net] biblioteke da biste automatizirali stvaranje računa za obradu, brisanje, upravljanja ključem i otkrivanje kvote.

- **Stvaranje i brisanje grupe za račune** unutar bilo kojeg područja. Ako kao proizvođači softvera dobavljača (Neovisni), na primjer, unesete služba za klijentima u kojem svaki je dodijeljen zaseban obradu račun za naplatu svrhe, stvaranje i brisanje mogućnosti računa možete dodati portal za klijenta.
- **Dohvaćanje i regenerate računa tipke** programski za bilo koju od vaših računa grupe. To može vam pomoći u skladu s sigurnosna pravila kojih se provode periodičku dinamične ili isteka ključeva za račun. Ako imate nekoliko računa grupe u različitim područjima Azure, automatizacija prelaska postupak povećava učinkovitosti vaše rješenje.
- **Provjera kvote za račun** i preuzimanje nagađanja probnu verziju i pogreške iz određivanje koje račune za obradu imaju koje ograničenja. Potvrđivanjem kvote za račun prije pokretanja poslova stvaranja grupe ili dodavanje računalnim čvorove, pri možete prilagoditi mjesto ili te izračunati resursi se stvaraju. Možete odrediti koje račune zahtijevaju kvote povećava prije dodjeljivanje dodatne resurse u te račune.
- **Kombiniranje značajke drugih servisa za Azure** za sučelje za upravljanje punu – pomoću grupe za upravljanje .NET, [Azure Active Directory][aad_about], i [Upravljanja resursima Azure] [ resman_overview] zajedno u aplikaciji. Pomoću ove značajke i njihove API-ji pružaju doživljaj frictionless provjeru autentičnosti, mogućnost stvaranje i brisanje grupa resursa i mogućnosti koje su opisana iznad za upravljanje završetka do kraja rješenje.

> [AZURE.NOTE] Dok je ovaj članak usredotočuje se na programski upravljanje obradu račune, tipke i kvote, na raspolaganju vam brojne te aktivnosti pomoću [portala za Azure][azure_portal]. Dodatne informacije potražite u članku [Stvaranje računa grupe za Azure pomoću portala za Azure](batch-account-create-portal.md) i [kvotama i ograničenja servisa Azure grupe](batch-quota-limit.md).

## <a name="create-and-delete-batch-accounts"></a>Stvaranje i brisanje grupe za račune

Kao što je rečeno, jedna od primarni značajke upravljanja API obradu je za stvaranje i brisanje računa grupe u Azure regiji. Da biste to učinili, koristite [BatchManagementClient.Account.CreateAsync] [ net_create] i [DeleteAsync][net_delete], ili njihovim sinkrono verzija.

Sljedeći isječak koda stvara račun, dohvaća novostvorenu računa iz grupe servisa, a zatim briše. U ovom isječak, a drugi u ovom se članku `batchManagementClient` je potpuno inicijalizirana pojavljivanja [BatchManagementClient][net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] Aplikacije koje koriste biblioteku .NET upravljanje grupe i njegova klasa BatchManagementClient potreban je **administrator servisa** ili **coadministrator** pristup pretplatu koja je vlasnik grupe za račun koji želite upravljati. Dodatne informacije potražite u odjeljku [Azure Active Directory](#azure-active-directory) i [AccountManagement] [ acct_mgmt_sample] uzorak koda.

## <a name="retrieve-and-regenerate-account-keys"></a>Dohvaćanje i Obnovi tipke računa

Nabavite tipke primarnih i sekundarnih račun s bilo kojeg računa grupe unutar pretplate pomoću [ListKeysAsync][net_list_keys]. Možete Obnovi te ključeve pomoću [RegenerateKeyAsync][net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [AZURE.TIP] Možete stvoriti pojednostavnjeno veze tijeka rada za upravljanje aplikacije. Najprije nabaviti ključ za račun za grupe za račun koji želite upravljati s [ListKeysAsync][net_list_keys]. Zatim pomoću tog ključa kada pokretanje grupe za .NET biblioteke [BatchSharedKeyCredentials] [ net_sharedkeycred] klase koja se koristi Inicijalizacija [BatchClient][net_batch_client].

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Provjera Azure pretplate i obradu račun kvote

Azure pretplate i pojedinačne Azure servise kao što je sve skupna imati zadani kvote koje ograničavanje broja određene entiteti u njima. Zadani kvote za Azure pretplate, potražite u članku [Azure pretplate i ograničenja servisa, kvote, i ograničenja](./../azure-subscription-service-limits.md). Kvote za zadane grupe servisa potražite u članku [kvotama i ograničenja servisa Azure grupe](batch-quota-limit.md). Pomoću grupe za upravljanje .NET biblioteke možete provjeriti ove kvote u aplikacije. To vam omogućuje da prije Dodavanje računa ili resurse kao što su grupe za izračun i izračunati čvorove odluke dodijeljeni.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Provjera Azure pretplatu za obradu račun kvote

Prije stvaranja grupe za račun u regiji, možete provjeriti pretplate Azure da biste vidjeli jeste li moći dodati račun u tom području.

U koda ispod, najprije koristimo [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] da biste dobili zbirka svi računi grupe koje se nalaze u pretplatu. Nakon što smo ste nabavili zbirku, ne možemo odrediti kolikim se brojem računa u regiji cilj. Zatim ćemo pomoću [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] da biste dobili kvote za obradu računa i odrediti koliko računi (ako ih ima) mogu se kreirati to područje.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

U isječak gornjeg `creds` je instance komponente [TokenCloudCredentials][azure_tokencreds]. Primjer stvaranje taj objekt potražite [AccountManagement] [ acct_mgmt_sample] uzorak koda na GitHub.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Provjerite račun grupe za kvota računalnim resurse poslužitelja

Prije povećanje računalnim resursa u rješenje grupe, možete provjeriti da biste bili sigurni resursa koje želite dodijeliti neće biti dulji od kvote na račun. U koda ispod, ne možemo ispisati kvota za obradu račun pod nazivom `mybatchaccount`. U vlastiti računala možete koristiti takve informacije da biste odredili hoće li račun možete rukovati dodatne resurse će biti stvoren.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] Dok je zadani kvote za Azure pretplate i servise, mnoge od tih ograničenja može biti potenciju slanjem zahtjeva za [Azure portal][azure_portal]. Ako, na primjer, potražite u članku [kvotama i ograničenja servisa za obradu Azure](batch-quota-limit.md) upute za povećanje kvote za račun grupe.

## <a name="batch-management-net-azure-ad-and-resource-manager"></a>Obrada upravljanje .NET, Azure AD i resursima

Kada radite s bibliotekom grupe za upravljanje .NET, obično koristite i [Azure Active Directory] [ aad_about] (Azure AD) i [Upravljanja resursima Azure][resman_overview]. Uzorak projekta spominju ispod koristi Azure Active Directory i Voditelj resursa dok pokazuje .NET API-JA upravljanje grupe.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure sam koristi Azure AD za provjeru autentičnosti njene kupci, administratori servisa i korisnici tvrtke ili ustanove. U kontekstu grupe za upravljanje .NET, koristiti Azure AD za provjeru autentičnosti administratora za pretplatu ili zajednički administratora. Time se omogućuje biblioteke za upravljanje za slanje upita servis za obradu i izvođenje operacija opisane u ovom članku.

U programu project uzorka spominju ispod Azure [Provjera autentičnosti biblioteke imenika Active Directory] [ aad_adal] (ADAL) koristi se za Pitaj korisnika za svoja uvjerenja Microsoft. Kada se od administratora ili coadministrator vjerodajnice servisa, aplikacija možete upit Azure popis pretplata – i možete stvarati i brisati grupe resursa i obradu računi.

### <a name="resource-manager"></a>Voditelj resursa

Kada stvorite grupe za račune s bibliotekom grupe za upravljanje .NET, koji će obično stvoriti ih unutar [grupa resursa][resman_overview]. Grupa resursa možete stvoriti programski pomoću [ResourceManagementClient] [ resman_client] klase u [Resursima .NET] [ resman_api] biblioteke. Ili možete dodati račun u postojeću grupu resursa koji ste stvorili ranije korištenjem [Azure portal][azure_portal].

## <a name="sample-project-on-github"></a>Ogledna projekt na GitHub

Da biste vidjeli grupe za upravljanje .NET u praksi, pogledajte [AccountManagment] [ acct_mgmt_sample] uzorka projekt na GitHub. Prikazuje ovu aplikaciju konzole za stvaranje i korištenje funkcije [BatchManagementClient] [ net_mgmt_client] i [ResourceManagementClient][resman_client]. Pokazuje i korištenje Azure [Provjera autentičnosti biblioteke imenika Active Directory] [ aad_adal] (ADAL), koji je potreban i klijenti.

Da biste uspješno pokrenuti aplikaciju uzorka, prvo morate registrirati ga s Azure AD pomoću portala za Azure. Slijedite korake u odjeljku [Dodavanje aplikacije](../active-directory/active-directory-integrating-applications.md#adding-an-application) u [aplikacijama Integrating s Azure Active Directory] [ aad_integrate] da biste registrirali primjer aplikacije vlastite subjekta je zadani direktorija. Obavezno odaberite **Izvorni klijentska aplikacija** za vrstu aplikacije, a možete navesti bilo koji valjani URI (kao što su `http://myaccountmanagementsample`) za **Preusmjeravanje URI**– ga ne mora biti stvarni krajnjoj točki.

Kad dodate aplikaciju, delegiranje dozvola za **Upravljanje servisom Azure Access kao tvrtke ili ustanove** s aplikacijom sustava *Windows Azure usluga upravljanja API* u odjeljku postavke aplikacije na portalu:

![Dozvole za aplikaciju u portal za Azure][2]

> [AZURE.TIP] Ako **Windows Azure usluga upravljanja API** ne pojavi u odjeljku *dozvole drugim aplikacijama*, kliknite **Dodaj aplikaciju**, odaberite **Upravljanje API usluge za Windows Azure**, a zatim kliknite gumb kvačica. Nakon toga dodijeliti dozvole kao što je navedeno iznad.

Kada dodate aplikaciju prethodno opisan ažuriranje `Program.cs` u [AccountManagment] [ acct_mgmt_sample] uzorka projekta s preusmjeravanje URI vaše aplikacije i ID klijenta. Na kartici **Konfiguriranje** aplikacije možete pronaći ove vrijednosti:

![Konfiguracija aplikacije Azure portalu][3]

[AccountManagment] [ acct_mgmt_sample] probna aplikacija prikazuje sljedeće postupke:

1. Nabava sigurnosnog tokena iz Azure AD pomoću [ADAL][aad_adal]. Ako korisnik je već niste prijavljeni, oni se traži vjerodajnica Azure.
2. Pomoću sigurnosnog tokena dobivenog Azure AD stvoriti [SubscriptionClient] [ resman_subclient] upit Azure popis pretplata povezanu s računom. Time se omogućuje odabir pretplate ako je više nalaze.
3. Stvorite povezanog s pretplatom za odabrani objekt vjerodajnice.
4. Stvaranje [ResourceManagementClient] [ resman_client] pomoću vjerodajnica.
5. Korištenje [ResourceManagementClient] [ resman_client] da biste stvorili grupu resursa.
6. Korištenje [BatchManagementClient] [ net_mgmt_client] da biste izveli nekoliko postupaka obradu računa:
  - Stvaranje grupe za račun u novu grupu resursa.
  - Nabavljanje novostvorenu računa iz grupe servisa.
  - Ispis tipki račun za novi račun.
  - Obnovi novi primarni ključ za račun.
  - Ispis kvota za račun.
  - Ispis kvota za pretplatu.
  - Ispis svih računa u sklopu pretplate.
  - Brisanje novostvorenu računa.
7. Izbrisati grupu resursa.

Prije brisanja novostvorenu grupe za račun i resursa grupi, možete provjeriti i [Azure portal][azure_portal]:

![Azure portal prikazuje grupu resursa i grupe za račun][1]
<br />
*Azure portal prikazuje novu grupu resursa i grupe za račun*

[aad_about]: ../active-directory/active-directory-whatis.md "Što je Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Provjera autentičnosti scenariji za Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integriranje aplikacije s Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspxs
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
