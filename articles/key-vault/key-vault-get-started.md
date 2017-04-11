<properties
    pageTitle="Početak rada s Azure ključ sigurnog | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča pomoći da započnete s Azure sigurnog ključ za stvaranje spremnika hardened u Azure za pohranu i upravljanje šifriranja tipke i tajne servisu Azure."
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
    ms.date="10/24/2016"
    ms.author="cabailey"/>

# <a name="get-started-with-azure-key-vault"></a>Početak rada s sigurnog ključ Azure #
Azure ključ sigurnog dostupan je u većini područja. Dodatne informacije potražite u članku [ključ sigurnog cijene stranice](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Uvod  
Pomoću ovog praktičnog vodiča pomoći da započnete s Azure sigurnog ključ za stvaranje kontejnera hardened (sigurnog) u Azure za pohranu i upravljanje šifriranja tipke i tajne u Azure. Ga vodit će vas kroz postupak korištenja Azure PowerShell da biste stvorili sigurnog koja sadrži ključ ili lozinke koje možete koristiti s Azure aplikacije. Zatim pokazuje kako koristiti aplikaciju taj ključ ili lozinku.

**Procijenjena vrijeme da biste dovršili:** 20 minuta

>[AZURE.NOTE]  Pomoću ovog praktičnog vodiča ne uključuje upute za pisanje Azure aplikacije da jedan od koraka obuhvaća, odnosno način da biste autorizirali aplikaciju pomoću tipke ili tajna u ključa zbirke ključeva.
>
>Pomoću ovog praktičnog vodiča koristi Azure PowerShell. Različite platforme sučelje naredbenog retka upute potražite u članku [ovom ćete praktičnom vodiču ekvivalentan](key-vault-manage-with-cli.md).

Pregled informacija o sigurnog ključ Azure, potražite u članku [što je sigurnog ključ Azure?](key-vault-whatis.md)

## <a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj Praktični vodič, morate imati sljedeće:

- Pretplate na Microsoft Azure. Ako ne postoji, možete se prijaviti [pomoću računa](https://azure.microsoft.com/pricing/free-trial/).
- Azure komponente PowerShell **Minimalna verzija 1.1.0**. Da biste instalirali Azure PowerShell i povezati s pretplatom Azure, Saznajte [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md). Ako ste već instalirali Azure PowerShell i ne znate na verziju iz konzole za Azure PowerShell, upišite `(Get-Module azure -ListAvailable).Version`. Kada imate Azure PowerShell verzije 0.9.1 kroz 0.9.8 instaliran, i dalje možete koristiti pomoću ovog praktičnog vodiča manji promjenama. Na primjer, morate koristiti u `Switch-AzureMode AzureResourceManager` naredbe i neke naredbe Azure ključ sigurnog promijenjeni. Popis cmdleta sigurnog ključ za verzije 0.9.1 putem 0.9.8, potražite u članku [Cmdleti za sigurnog ključ za Azure](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.98\).aspx). 
- Aplikacija koja će biti konfigurirana tako da pomoću tipke ili lozinke koje ste stvorili pomoću ovog praktičnog vodiča. Primjer aplikacije dostupna iz [Microsoftova centra za preuzimanje](http://www.microsoft.com/en-us/download/details.aspx?id=45343). Upute potražite u članku popratnu datoteke Readme.


Pomoću ovog praktičnog vodiča namijenjen za početnike Azure PowerShell, ali pretpostavlja se da razumijete osnovni koncepti, kao što su module, cmdleta i sesije. Dodatne informacije potražite u članku [Početak rada s komponentom Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx).

Da biste dobili detaljnu pomoć za sve cmdlet koja se prikazuju u ovom ćete praktičnom vodiču, koristite cmdlet **Get-pomoć** .

    Get-Help <cmdlet-name> -Detailed

Na primjer, da biste dobili pomoć za **Prijavu AzureRmAccount** cmdlet, upišite:

    Get-Help Login-AzureRmAccount -Detailed

Možete pročitati i sljedeći vodiči za upoznati s Azure Voditelj resursa u Azure PowerShell:

- [Kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md)
- [Korištenje Azure PowerShell s Voditelj resursa](../powershell-azure-resource-manager.md)


## <a id="connect"></a>Povezivanje s pretplate ##

Pokretanje sesije razmjene Azure PowerShell i prijavite se na račun za Azure pomoću sljedeće naredbe:  

    Login-AzureRmAccount 

Imajte na umu da ako koristite instancu Azure, na primjer, državne Azure, koristite parametar - okruženju pomoću ove naredbe. Ako, na primjer:`Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

U prozoru preglednika skočni unesite račun za Azure korisničko ime i lozinku. Azure PowerShell dohvaća sve pretplate koje su povezane s ovim računom i prema zadanim postavkama, koristi prvoga.

Ako imate više pretplata i želite da navedete jednu određenu za Azure ključ sigurnog, upišite sljedeće da biste vidjeli popis pretplata za vaš račun:

    Get-AzureRmSubscription

Zatim, da biste odredili pretplate da biste koristili, upišite:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Dodatne informacije o konfiguriranju Azure PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).


## <a id="resource"></a>Stvorite novu grupu resursa ##

Kada koristite upravitelj resursa Azure, sve povezane resurse stvaraju se u grupu resursa. Ćemo će stvoriti novu grupu resursa pod nazivom **ContosoResourceGroup** za ovaj Praktični vodič:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Stvaranje ključa zbirke ključeva ##

Pomoću cmdleta [New AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt603736\(v=azure.300\).aspx) da biste stvorili ključa zbirke ključeva. Ovaj cmdlet ima tri obavezno parametara: **naziv grupe resursa**, **Naziv ključa sigurnog**i **geografsku lokaciju**.

Na primjer, ako koristite naziv sigurnog **ContosoKeyVault**, naziv grupe resursa za **ContosoResourceGroup**i mjesto **istočnoazijski**, upišite:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Izlaz iz ovaj cmdlet prikazuje svojstva zbirke ključeva ključa koji ste upravo stvorili. Dva najvažnija svojstva su:

- **Naziv zbirke ključeva**: U ovom primjeru to je **ContosoKeyVault**. Koristit ćete naziv cmdleti za drugi ključ sigurnog.
- **URI sigurnog**: U ovom primjeru to je https://contosokeyvault.vault.azure.net/. Aplikacije koje koriste vaš sigurnog do njegova REST API-JA morate koristiti ovaj URI.

Račun za Azure sada ovlašteni za izvođenje operacije na ovom ključa sigurnog. Što još nitko drugi je.

>[AZURE.NOTE]  Ako se prikaže poruka **pretplate nije registriran da biste koristili polje naziva "Microsoft.KeyVault"** kada pokušate stvoriti novi ključ zbirke ključeva, pokrenite `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` i zatim ponovno pokrenite naredbu novo AzureRmKeyVault. Dodatne informacije potražite u članku [Register AzureRmResourceProvider](https://msdn.microsoft.com/library/azure/mt759831\(v=azure.300\).aspx).
>

## <a id="add"></a>Dodajte ključ ili tajna ključa zbirke ključeva ##

Ako želite Azure sigurnog ključ za stvaranje ključa zaštićenima softver za vas, koristite cmdlet za [Dodavanje AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) i upišite sljedeće:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Međutim, ako imate postojeći softver zaštićeni ključ u u. PFX datoteke spremljene na pogon C:\ u datoteku pod nazivom softkey.pfx koje želite prenijeti sigurnog ključ Azure, upišite sljedeće da biste postavili varijable **securepfxpwd** za lozinku **123** za na. PFX datoteka:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Zatim upišite sljedeću naredbu da biste uvezli ključ iz sustava. Datoteka PFX koji štiti tipku softverom servisa sigurnog ključ:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Sada možete referencirati ovaj ključ koji ste stvorili ili prenijeli sigurnog ključ Azure, pomoću njegov URI. Pomoću **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** uvijek preuzeli trenutnu verziju, a **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** da biste pristupili slijedite ovaj određenu verziju.  

Da biste prikazali URI za ovaj ključ, upišite:

    $Key.key.kid

Da biste dodali na tajna sigurnog koja je lozinka pod nazivom SQLPassword i ima vrijednost stranicu$ w0rd za Azure ključ sigurnog, najprije pretvorite vrijednost od stranicu$ $w0rd niz sigurne tako da upišete na sljedeći način:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Nakon toga upišite sljedeće:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Sada možete referencirati ovu lozinku koju ste dodali sigurnog ključ Azure, pomoću njegov URI. Pomoću **https://ContosoVault.vault.azure.net/secrets/SQLPassword** uvijek preuzeli trenutnu verziju, a **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** da biste pristupili slijedite ovaj određenu verziju.

Da biste prikazali URI za ovu tajna, upišite:

    $secret.Id

Pogledajmo prikaz ključ ili tajna koju ste upravo stvorili:

- Da biste vidjeli svoj ključ, upišite:`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
- Da biste pogledali svoje tajna, upišite:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Sada ključa sigurnog i ključ ili tajna spremna je za korištenje aplikacijama. Morate Autorizirajte aplikacije da biste ih koristiti.  

## <a id="register"></a>Registracija aplikacije pomoću servisa Azure Active Directory ##

Ovaj korak obično želite napraviti razvojni inženjer na odvojenom računalu. Nije specifična za Azure ključ sigurnog, ali se za dovršenosti.


>[AZURE.IMPORTANT] Da biste dovršili vodič, vaš račun, na sigurnog i aplikaciju koja će registrirati u ovom ćete koraku svi moraju biti u istoj Azure direktoriju.

Aplikacije koje koriste ključne sigurnog morate provjeriti autentičnost pomoću tokena iz servisa Azure Active Directory. Da biste to učinili, vlasnik aplikacije morate registrirati aplikacije u svoje Azure Active Directory. Na kraju Registracija aplikacije vlasnik dobiva sljedeće vrijednosti:


- **ID aplikacije** (poznat i kao ID klijenta) i **provjeru autentičnosti** (poznat i kao dijeljena tajna). Aplikacija morate prezentirati obje te vrijednosti Azure Active Directory, da biste dobili token. Kako će se aplikacija konfiguriran za to ovisi o aplikaciji. Vlasnik aplikacije za uzorak aplikaciju sigurnog ključ postavlja te vrijednosti u datoteci app.config.

Da biste registrirali aplikaciju servisa Azure Active Directory:

1. Prijavite se na portal Azure klasični.
2. Na lijevoj strani kliknite **Servisa Active Directory**, a zatim direktoriju u kojem će registrirati svoje aplikacije. <br> <br> **Bilješke:** Morate odabrati isti direktorij koji sadrži Azure pretplate koji ste stvorili vaše ključa sigurnog. Ako ne znate koje direktorija to je, kliknite **Postavke**, prepoznavanje pretplate koji ste stvorili vaše ključa sigurnog, a zatim zapišite naziv direktorija koji se prikazuje u zadnjem stupcu.

3. Kliknite **APLIKACIJE**. Ako nema aplikacije dodani direktorija, ova stranica pokazuje samo veza **Dodaj aplikaciju** . Kliknite vezu ili umjesto toga, možete kliknuti **DODAJ** na naredbenoj traci.
4.  U čarobnjaku za **Dodavanje APLIKACIJE** na na **što želite učiniti?** stranice, kliknite **Dodaj aplikaciju je moje tvrtke ili ustanove razvoju**.
5.  Na stranici **Recite nam o aplikaciji** Navedite naziv aplikacije, a zatim **API WEB AND/OR APLIKACIJU za WEB** (zadano). Kliknite ikonu **Dalje** .
6.  Na stranici **Svojstva aplikacije** navedite **URL za prijavu na** i **ID URI Aplikaciju** za web-aplikaciju. Ako aplikacija nije te vrijednosti, možete ih učiniti za ovaj korak (možete, primjerice, odrediti http://test1.contoso.com za oba okvira). Nije važno ako postoji tih web-mjesta. Što je važno je aplikaciju ID URI za svaku aplikaciju razlikuje za svaku aplikaciju u direktoriju. Imenik koristi taj niz za označavanje aplikacije.
7.  Kliknite ikonu **Dovršeno** da biste spremili promjene u čarobnjaku.
8.  Na stranici za **Brzo pokretanje** kliknite **KONFIGURIRAJ**.
9.  Pomaknite se do odjeljka **tipke** , odaberite trajanje i zatim kliknite **SPREMI**. Stranica osvježava i prikazuje vrijednost ključa. Pomoću ovog ključa i vrijednost **ID KLIJENTA** , morate konfigurirati aplikacije. (Upute za tu konfiguraciju su specifičnim aplikacijama).
10. Kopirajte vrijednost ID klijenta s ove stranice koje ćete koristiti u sljedećem koraku za postavljanje dozvola za vaše zbirke ključeva.

## <a id="authorize"></a>Autorizirajte aplikaciju koju želite koristiti ključ ili tajna ##

Da biste autorizirali aplikacije da biste pristupili ključ ili tajna u na sigurnog, koristite cmdlet  [Skup AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625\(v=azure.300\).aspx) .

Na primjer, ako je naziv sigurnog **ContosoKeyVault** i željenu aplikaciju da biste autorizirali sadrži ID klijenta sustava 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, a želite da biste autorizirali aplikacije za šifriranje i prijavite se pomoću tipki u vaše zbirke ključeva pokrenite sljedeću naredbu:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Ako želite da biste autorizirali tu istu aplikaciju da biste pročitali tajne u vaše zbirke ključeva pokrenite sljedeću naredbu:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Ako želite koristiti modul sigurnost hardver (HSM) ##

Za dodatnu jamstva, moguće je uvesti i generiranje ključeva u hardver sigurnost Module (HSMs) koji nikad ostavite HSM granicu. Na HSMs su FIPS 140-2 razine 2 provjeriti. Ako taj zahtjev ne odnose na vas, preskočite ovaj odjeljak i otvorite da biste [izbrisali ključa sigurnog i pridruženi ključeva i tajne](#delete).

Da biste stvorili ključeve HSM zaštićen, morate koristiti [sloju servisa Azure ključ sigurnog Premium za podršku zaštićenima HSM tipke](https://azure.microsoft.com/pricing/free-trial/). Osim toga, imajte na umu da ta je funkcija nije dostupna za Azure Kina.


Kada stvorite ključa sigurnog, dodajte parametar **- SKU** :


    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Možete dodati zaštićenima softver (kao što je prikazano ranije) i HSM zaštićene tipke ovog ključa zbirke ključeva. Da biste stvorili ključa HSM zaštićen, postavite na **-odredište** parametar "HSM":

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Koristite sljedeću naredbu da biste uvezli ključ iz programa. PFX datoteku na računalu. Ta se naredba uvozi tipku u HSMs u servisu sigurnog ključ:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


Uvozi sljedeće naredbe u "Premjesti vlastiti ključ" paketa (BYOK). Ovaj scenarij omogućuje generiranje ključa u vašem lokalnom HSM i prenijeti ga HSMs u servisu sigurnog ključ bez napuštanja granicu HSM ključa:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Detaljnije upute o tome da biste generirali paket BYOK potražite u članku [upute za stvaranje i prijenos zaštićenima HSM ključeva za Azure ključ sigurnog](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Izbrišite ključ sigurnog i pridruženi tipke i tajne ##

Ako više ne trebate ključa sigurnog i ključ ili tajna koje sadrži, možete izbrisati ključa sigurnog pomoću cmdleta [Ukloni AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt619485\(v=azure.300\).aspx) :

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Ili možete izbrisati grupu cijela Azure resursa koja sadrži ključne sigurnog i ostale resurse uvrštene u toj grupi:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Ostale Azure cmdleta ljuske PowerShell ##

Ostale naredbe koji vam mogu biti korisni za upravljanje sigurnog Azure ključ:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Ta se naredba dohvaća tablični prikaz svih tipke i odabrana svojstva.
- `$Keys[0]`: Ta naredba prikazuje cijeli popis svojstava za navedeni ključ
- `Get-AzureKeyVaultSecret`: Ovaj command dohvaća tablični prikaz svih tajnu imena i odabrana svojstva.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Primjer uklanjanje određenih tipka.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Primjer kako ukloniti određene tajna.


## <a id="next"></a>Daljnji koraci ##

Praćenje vodič koji koristi Azure ključ sigurnog u web-aplikaciji, potražite u članku [Korištenje Azure ključ sigurnog iz web-aplikacije](key-vault-use-from-web-application.md).

Da biste vidjeli kako se vaše ključa sigurnog koristi, potražite u članku [Azure ključ sigurnog bilježenje u zapisnik](key-vault-logging.md).

Popis najnovije Azure PowerShell cmdleti za Azure ključ sigurnog, potražite u članku [Cmdleti za sigurnog ključ za Azure](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.300\).aspx). 
 

Programiranje reference, potražite u članku [Vodič za Azure ključ sigurnog programiranje](key-vault-developers-guide.md).
