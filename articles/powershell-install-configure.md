<properties
    pageTitle="Kako instalirati i konfigurirati Azure PowerShell"
    description="Saznajte kako instalirati i konfigurirati Azure PowerShell."
    editor="tysonn"
    manager="dongill"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="coreyp"/>

# <a name="how-to-install-and-configure-azure-powershell"></a>Kako instalirati i konfigurirati Azure PowerShell

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">PowerShell</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">Azure EŽA</a></div>

##<a name="what-is-azure-powershell"></a>Što je Azure PowerShell?
Azure PowerShell je skup moduli koji omogućuju cmdleti za upravljanje Azure s komponentom Windows PowerShell. Možete koristiti u Cmdlete da biste stvorili, testirajte, uvođenje i upravljanje rješenja i usluga koji se isporučuje putem platforme Azure. U većini slučajeva, može koristiti u cmdleta iste zadatke kao što su Portal Azure, kao što su stvaranje i konfiguriranje servisa u oblaku, virtualnim strojevima, virtualne mreže i web-aplikacije.

## <a name="how-versioning-works"></a>Kako se radi s verzijama

Azure PowerShell koristi Semantičkog rad s verzijama, što znači da, ako verzija odgovora > verzije B, a zatim verzija ima najnovije API-ji. Osim toga, to znači da se promjene u sredinu glavna verzija na važne promijeniti u jednu ili više cmdleta.  Tako, na primjer, verzija 1.7.0 je hitni popravak da biste riješili otpornosti promjene u verzijama 1.x Azure PowerShell.

Dodatne informacije o verzijama Semantičkog Savjeti za rad s Azure PowerShell potražite u članku Semantičkog specifikacija rad s verzijama na: http://semver.org
 
Da biste dobili najnovije API-ji, koristite verziju 2.x. Ali ako imate skripte sastavljene od verzije 1.x i ne želite apsorbira prijelom promjene u verziji 2.x opisana u 2.x [Napomene](https://github.com/Azure/azure-powershell/blob/dev/documentation/release-notes/migration-guide.2.0.0.md), a zatim instalirajte 1.7.0.

Nepodudarnost verzija može uzrokovati ako je instalirana najnovija verzija modula profila, a stariju verziju modula ovisi o njemu naknadno učitati. Najjednostavniji način da biste riješili taj problem jest da biste instalirali s najnovijim .msi. Na .msi automatski čisti starijih verzija module.
 
###<a name="installing-module-versions-side-by-side"></a>Instaliranje modula verzije tako da usporednu

2.1.0 (i verzije 1.2.6 za AzureStack) su prvi verzije modula namijenjena za instaliranje i koristi jedno uz drugo. Budući da Azure PowerShell koristi binarni module, morate otvaranje novog prozora PowerShell i koristiti **Modul za uvoz** da biste uvezli određenu verziju programa AzureRM Cmdlete:

    Import-Module AzureRM -RequiredVersion 2.1.0**

Verzije prije 2.1.0 (osim 1.2.6) ne funkcioniraju i-usporedno s drugim verzijama modul Azure PowerShell. Prilikom učitavanja stariju verziju modula Azure PowerShell pomoću naredbe kao iznad kompatibilna verzija modula **AzureRM.Profile** će biti učitan, rezultira cmdleta koja vas pita da biste se prijavili svaki put kada izvršavanje cmdlet, čak i nakon prijave.

Najjednostavniji način za rješavanje to je da biste instalirali najnoviju Azure PowerShell iz na WebPI sažetak sadržaja ili .msi – Time se uklanjaju svi starijim verzijama programa moduli instalirali iz galerije. 

Imajte na umu da imaju modula Azure i AzureRM ovisnosti zajednički, tako da ako koristite i moduli kada se ažurira jednu, trebali biste i ažurirati. Starije verzije programa modul Azure imaju isti problem s jedno uz drugo modul učitavanje tog starijim verzijama programa modul AzureRM ste.

<a id="Install"></a>
## <a name="step-1-install"></a>Korak 1: instalacija

Slijede dva načina instalacije Azure PowerShell. Sustav možete instalirati ili iz galerije PowerShell ili na WebPI.

###<a name="installing-azure-powershell-from-the-powershell-gallery"></a>Instaliranje komponente PowerShell Azure iz galerije PowerShell

Na željeni način je pomoću galerije PowerShell. Potreban vam je modul PowerShellGet pomoću galerije PowerShell. Ta je mogućnost dostupna ovdje: [PowerShellGallery.com](https://www.powershellgallery.com/)

Instaliranje modula Azure PowerShell 1.3.0 ili noviji iz galerije PowerShell pomoću povećane komponente Windows PowerShell ili Očisti integrirani skriptiranje okruženje (filtar) odzivnik pomoću sljedeće naredbe:

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

####<a name="more-about-these-commands"></a>Dodatne informacije o tim naredbama

- **Instaliranje modula AzureRM** instalira modula skupne vrijednosti za cmdleti za upravljanje resursima Azure. Modul za AzureRM ovisi o 
- raspon određenu verziju za svaki od modula Azure Voditelj resursa. Raspon uključene verziju pokazuje da nema promjena modul prijelom mogu uključiti prilikom instalacije AzureRM moduli s istom glavnu verziju. Kada instalirate modul AzureRM, bilo koje modul Azure Voditelj resursa koji prethodno nije instaliran će preuzeti i instalirati iz galerije PowerShell. Dodatne informacije o semantičkog verzijama koristi modula Azure PowerShell potražite u članku [semver.org](http://semver.org). 
- **Instaliranje modula Azure** instalira modul Azure. Ovaj modul je modul za upravljanje servisom iz Azure PowerShell 0.9.x. To mora imati bez glavnih promjene, a zatim biti resursa za prethodnu verziju modula Azure.

###<a name="installing-azure-powershell-from-webpi"></a>Instaliranje Azure PowerShell s WebPI

Instaliranje Azure PowerShell 1.0 i veće od WebPI ostaje isto kakvo je za 0.9.x. Preuzmite [Azure PowerShell](http://aka.ms/webpi-azps) i pokrenite instalaciju sustava. Ako imate Azure PowerShell 0.9.x instaliran, verzija 0.9.x će se deinstalirati kao dio nadogradnje. Ako ste instalirali modula Azure PowerShell iz galerije PowerShell, instalacijski program automatski uklanja module prije instalacije da biste bili sigurni dosljedan okruženje za Azure PowerShell.

> [AZURE.NOTE] Ako ste već instalirali modula Azure iz galerije PowerShell, instalacijski program automatski uklonit će ih. To sprječava zbrku o verzijama modul ste instalirali i gdje se nalaze. Galerija PowerShell moduli instalirat će obično u **%ProgramFiles%\WindowsPowerShell\Modules**. Nasuprot tome, WebPI instalacijski program instalirat će Azure module u * *% ProgramFiles (x86) %\Microsoft SDKs\Azure\PowerShell\**. Ako se pojavi pogreška tijekom instalacije, možete ukloniti ručno na Azure* mape u mapu **%ProgramFiles%\WindowsPowerShell\Modules** i pokušajte instalaciju.

Nakon dovršetka instalacije sustava ```$env:PSModulePath``` postavka uključuje direktorija koji sadrži cmdleta Azure PowerShell.

> [AZURE.NOTE] Postoji poznat problem sa servisom PowerShell **$env: PSModulePath** koji se mogu pojaviti prilikom instalacije s WebPI. Ako na računalu je potrebno ponovno pokrenuti zbog ažuriranja sustava ili druge instalacije, može uzrokovati ažuriranja **$env: PSModulePath** da ne uvrstite put na kojem je instaliran Azure PowerShell. U tom slučaju vidjet ćete poruku "nije prepoznata cmdlet" prilikom pokušaja pomoću cmdleta ljuske PowerShell za Azure nakon instalacije ili nadogradnje. Ako se to dogodi, računalo mora riješi ponovnim pokretanjem računala.

Ako vam se prikaže poruka ovako kada pokušate učitati ili izvoditi Cmdlete:

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

To se može ispraviti tako da ponovno pokrenuti računalo ili uvoz s cmdletima iz C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\ kao sljedeće (pri čemu je XXXX verziju PowerShell instaliran:

```
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

## <a name="step-2-start"></a>Korak 2: pokretanje
S cmdletima možete pokrenuti iz standardnog konzole za Windows PowerShell, ili iz Očisti integrirani skriptiranje okruženje (filtar).
Način koristite da biste otvorili ili konzole ovisi o verziji sustava Windows koju koristite:

- Na računalu sa sustavom najmanje Windows 8 ili Windows Server 2012, možete koristiti ugrađeno pretraživanje. Na **početnom** zaslonu počnite pisati power. To će vratiti opsegu popis aplikacija koja sadrži komponente Windows PowerShell. Da biste otvorili konzolu, kliknite ili aplikacije. (Da biste prikvačili aplikaciju na **Početni** zaslon, desnom tipkom miša kliknite ikonu.)

- Na računalu sa sustavom verziju stariju od sustava Windows 8 ili Windows Server 2012, možete koristiti **izbornik Start**. S izbornika **Start** , zatim **Svi programi**, pa **Pomagala**, kliknite mapu **Komponente Windows PowerShell** , a zatim **Komponente Windows PowerShell**.

Možete i pokrenuti **Windows Očisti filtar** da biste koristili stavke izbornika i tipkovni prečaci za izvođenje mnoge zadatke koje obavljate na konzoli za Windows PowerShell. Da biste u filtar na konzoli komponente Windows PowerShell Cmd.exe, ili u okvir **Pokreni** , upišite, **powershell_ise.exe**.

###<a name="commands-to-help-you-get-started"></a>Naredbe za početak rada

    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 
    
    # To log in to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster log in experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## <a name="step-3-connect"></a>Korak 3: povezivanje
Na cmdleta morate pretplatu da bi se mogao upravljati servisa. Možete kupiti pretplatu na Azure ako ga već nemate. Upute potražite u članku [kako kupiti Azure](http://go.microsoft.com/fwlink/p/?LinkId=320795).

1. Vrsta **Prijava AzureRmAccount**

2. Upišite adresu e-pošte i lozinku povezanu s računom. Azure potvrđuje sprema vjerodajnicu informacije i zatvara prozor.

– ILI –

Prijavite se u tvrtke ili obrazovne ustanove računa:

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [AZURE.NOTE] Ako imate više od jednog klijenta povezan s računa tvrtke ili ustanove, navedite parametar TenantId:

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [AZURE.NOTE] Koji nije interaktivan prijava metoda funkcionira samo s računom tvrtke ili obrazovne ustanove. Račun tvrtke ili obrazovne ustanove je korisnik koja se upravlja administrator tvrtke ili obrazovne ustanove i definirano u instancu Azure Active Directory za tvrtke ili obrazovne ustanove. Ako trenutno ne imati račun tvrtke ili obrazovne ustanove, a se pomoću Microsoftova računa za prijavu u pretplatu za Azure, pomoću sljedećih koraka možete jednostavno stvoriti.

> 1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com)pa kliknite na **Servisu Active Directory**.

> 2. Ako nema direktorijem, odaberite **Stvaranje direktorija** i pružili potrebne informacije.

> 3. Odaberite direktorija i dodavanje novog korisnika. Novi korisnik mogu se prijaviti pomoću računa tvrtke ili obrazovne ustanove. Tijekom stvaranja korisnika će biti navedena s obje adresu e-pošte za korisnika i privremenu lozinku. Spremite te informacije, kao što se koristi u nastavku 5.

> 4. Azure klasični, na portalu odaberite **Postavke** , a zatim odaberite **Administratori**. Odaberite **Dodaj**pa Dodaj novog korisnika kao suradnika administrator. Time se omogućuje račun tvrtke ili obrazovne ustanove da biste upravljali pretplatom Azure.

> 5. Na kraju, odjavite se iz aplikacije portala za Azure klasični, a zatim se prijavite u radu pomoću ili Školski račun. Ako je ovo prvi put prijave pomoću tog računa, morat ćete promijeniti lozinku.

> Dodatne informacije o registracije za Microsoft Azure pomoću računa tvrtke ili obrazovne ustanove, potražite u članku [registrirati za Microsoft Azure kao tvrtkom ili ustanovom](./active-directory/sign-up-organization.md).

> Dodatne informacije o upravljanju provjeru autentičnosti i pretplate u Azure potražite u članku [Upravljanje korisničkim računima, pretplate i administratorske uloge](http://go.microsoft.com/fwlink/?LinkId=324796).

### <a name="view-account-and-subscription-details"></a>Prikaz detalja račun i pretplate

Možete imati više računa i dostupni za korištenje Azure PowerShell pretplate. Možete dodati više računa tako da pokrenete **Dodaj AzureRmAccount** više puta.

Da biste prikazali dostupne Azure račune, upišite **Get-AzureAccount**.

Da biste prikazali pretplate Azure, upišite **Get-AzureRmSubscription**.

##<a id="Help"></a>Pristup pomoći##

Ove resurse pruža pomoć za određeni Cmdlete:


-   Iz unutar konzoli sustava možete koristiti ugrađeno sustava pomoći. Cmdlet **Pristup pomoći** omogućuje pristup sustavu. 

- Zatražite pomoć od zajednice, pokušajte sljedeće popularne forumima:

 - [Azure forum na MSDN-u]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
 - [Stackoverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

##<a name="learn-more"></a>uči više


Potražite u sljedećim resursima da biste saznali više o korištenju s cmdletima:

Osnovne upute o pomoću komponente Windows PowerShell potražite u članku [Pomoću komponente Windows PowerShell](http://go.microsoft.com/fwlink/p/?LinkId=321939).

Referentne informacije o na cmdleta potražite u članku [Referenca Cmdlet Azure](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx).

Ogledne skripte i upute da biste lakše saznali da biste upravljali Azure pomoću skriptiranje potražite u članku [Centar za skripte](http://go.microsoft.com/fwlink/p/?LinkId=321940).

