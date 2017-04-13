<properties
   pageTitle="Pomoću skripte za Windows PowerShell za objavljivanje na web-mjesto razvojni i u okvir za testiranje okruženju | Microsoft Azure"
   description="Saznajte kako pomoću skripte komponente Windows PowerShell iz Visual Studio objavite razvoj i testiranje okruženja."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>Pomoću skripte komponente Windows PowerShell za objavljivanje razvojni i testiranje okruženja

Kada stvorite web-aplikaciju u Visual Studio, možete generirati skripte komponente Windows PowerShell koje možete koristiti kasnije da biste automatizirali objavljivanje web-mjesta za Azure kao web-aplikacijama u aplikacije servisa za Azure ili virtualnog računala. Možete urediti i proširivanje skripte komponente Windows PowerShell u uređivaču za Visual Studio u skladu s potrebama ili skriptu integrirati postojeće Sastavi, testiranje i skripte za objavljivanje.

Pomoću sljedeće skripte, možete je Dodjela prilagođene verzije (poznat i kao razvojni i testiranje okruženja) web-mjesta za privremeno korištenje. Na primjer, možete postaviti određenu verziju web-mjesta na Azure virtualnog računala ili pripremna vremensko razdoblje na web-mjestu pokrenite test paketa, ponovite pogrešku, testirajte otklanjanje problema, probnu verziju predloženu promjenu ili postavljanje prilagođene okruženja za pokazni videozapis ili prezentaciju. Nakon stvaranja skriptu koja objavljuje projekt, ponovno stvorite identične okruženja tako da ponovno pokrenete skriptu prema potrebi ili pokrenuti skriptu s vlastitim Sastavi web-aplikacije radi stvaranja prilagođenih okruženja za testiranje.

## <a name="what-you-need"></a>Što vam je potrebno

- Azure SDK 2.3 ili novijim. Dodatne informacije potražite [Visual Studio preuzimanja](http://go.microsoft.com/fwlink/?LinkID=624384) .

Ne morate Azure SDK za generiranje skripte za projekte web. Ta je značajka za projekte web, a ne web ulogama u servise u oblaku.

- Azure PowerShell 0.7.4 ili noviji. Dodatne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](powershell-install-configure.md) .

- [Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) ili noviji.

## <a name="additional-tools"></a>Dodatne alate

Dostupne su dodatne alate i resurse za rad sa servisom PowerShell u Visual Studio za Azure razvoj. U odjeljku [Alati ljuske PowerShell za Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-the-publish-scripts"></a>Generiranje skripte za objavljivanje

Možete generirati Objavi skripte za virtualnog računala na kojem je smještena web-mjesta prilikom stvaranja novog projekta sljedeće [ove upute](./virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md). Možete i [Generiranje objavljivanje skripte za web-aplikacije u aplikacije servisa za Azure](./app-service-web/web-sites-dotnet-get-started.md).

## <a name="scripts-that-visual-studio-generates"></a>Skripte koje generira Visual Studio

Visual Studio generira rješenje razinom mapu pod nazivom **PublishScripts** koja sadrži dvije datoteke komponente Windows PowerShell, Objavi skripte za virtualnog računala ili web-mjesta i modul koji sadrži funkcije koje možete koristiti u skripte. Visual Studio generira i datoteke u obliku JSON koji određuje detalja o projektu implementacije.

### <a name="windows-powershell-publish-script"></a>Komponente Windows PowerShell objavljivanje skripte

Skripta za objavljivanje sadrži određene Objavi korake za implementaciju web-mjesto ili virtualnog računala. Visual Studio nudi sintaksu Bojanje za Windows PowerShell razvoj. Pomoć za funkcije je dostupan i slobodno možete uređivati funkcije u skripti u skladu s potrebama promjena.

### <a name="windows-powershell-module"></a>Modul Windows PowerShell

Modul Windows PowerShell koje generira Visual Studio sadrži funkcije koje se koristi za skripte Objavi. Te su funkcije Azure PowerShell i nije namijenjen za mijenjanje. Dodatne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](powershell-install-configure.md) .

### <a name="json-configuration-file"></a>JSON konfiguracijska datoteka

Datoteka JSON je stvoriti u mapi **konfiguracije** i konfiguracija podacima koje određuje točno koje resurse za implementaciju Azure. Naziv datoteke koje generira Visual Studio je projekt-naziv-WAWS-dev.json ako ste stvorili web-mjesto ili projekt ime-VM-dev.json ako ste stvorili virtualnog računala. Evo primjera JSON konfiguracijska datoteka koje se generiraju prilikom stvaranja web-mjesta. Većina vrijednosti su self-explanatory. Naziv web-mjesta generira Azure, tako da bi možda bile podudarne naziva projekta.

```
{
"environmentSettings": {
"webSite": {
"name": "WebApplication26632",
"location": "West US"
},
"databases": [
{
"connectionStringName": "DefaultConnection",
"databaseName": "WebApplication26632_db",
"serverName": "YourDatabaseServerName",
"user": "sqluser2",
"password": "",
"edition": "",
"size": "",
"collation": "",
"location": "West US"
}
]
}
}
```
Kada stvorite virtualnog računala, konfiguracijska datoteka JSON izgleda otprilike ovako. Imajte na umu da su neki servis u oblaku stvorili kao spremnik za virtualnog računala. Virtualnog računala sadrži uobičajene krajnje točke za web access putem HTTP i HTTPS, kao i krajnje točke za Web implementacije, koju možete objaviti na web-mjesto s lokalnog računala, udaljene radne površine i programa Windows PowerShell.

```
{
"environmentSettings": {
"cloudService": {
"name": "myusernamevm1",
"affinityGroup": "",
"location": "West US",
"virtualNetwork": "",
"subnet": "",
"availabilitySet": "",
"virtualMachine": {
"name": "myusernamevm1",
"vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
"size": "Small",
"user": "vmuser1",
"password": "",
"enableWebDeployExtension": true,
"endpoints": [
{
"name": "Http",
"protocol": "TCP",
"publicPort": "80",
"privatePort": "80"
},
{
"name": "Https",
"protocol": "TCP",
"publicPort": "443",
"privatePort": "443"
},
{
"name": "WebDeploy",
"protocol": "TCP",
"publicPort": "8172",
"privatePort": "8172"
},
{
"name": "Remote Desktop",
"protocol": "TCP",
"publicPort": "3389",
"privatePort": "3389"
},
{
"name": "Powershell",
"protocol": "TCP",
"publicPort": "5986",
"privatePort": "5986"
}
]
}
},
"databases": [
{
"connectionStringName": "",
"databaseName": "",
"serverName": "",
"user": "",
"password": ""
}
],
"webDeployParameters": {
"iisWebApplicationName": "Default Web Site"
}
}
}
```

Možete urediti konfiguracije JSON da biste promijenili što se događa kada pokrenete skripte Objavi. Na `cloudService` i `virtualMachine` potrebni su sekcije, ali možete je izbrisati s `databases` sekcije ako vam nisu potrebni. Svojstva koja su prazna u konfiguracijskoj datoteci zadani koje generira Visual Studio nisu obavezni; one koje ne sadrže vrijednosti u konfiguracijskoj datoteci zadane su potrebni.

Ako imate web-mjesto koje sadrži više implementacije okruženja (poznatom kao slobodnih) umjesto jednog radnog web-mjesta u Azure, možete uključiti naziv vremensko razdoblje naziv web-mjesta u konfiguracijskoj datoteci JSON. Na primjer, ako imate web-mjesto koje sadrži pod nazivom **Moje web-mjesto** i vremensko razdoblje za nju pod nazivom **testiranje** zatim URI je Moje web-mjesto test.cloudapp.net, ali točan naziv da biste koristili u konfiguracijskoj datoteci je mysite(test). Možete obaviti samo ako web-mjesta i slobodnih već postoje u svoju pretplatu. Ako ne postoji, stvorite web-mjesta tako da pokrenete skriptu bez navođenja vremensko razdoblje, a zatim stvorite na vremensko razdoblje [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885)i nakon toga pokrenuti skriptu pod nazivom izmijenjene web-mjesta. Dodatne informacije o implementaciji slobodnih web-aplikacijama potražite u članku [Postavljanje pripremna okruženja za web-aplikacije u servisu Azure aplikacije](./app-service-web/web-sites-staged-publishing.md).

## <a name="how-to-run-the-publish-scripts"></a>Pokretanje skripte za objavljivanje

Ako nikada ne pokrenete skripte komponente Windows PowerShell prije, najprije morate postaviti pravila izvršavanja da biste omogućili pokretanje skripti. To je sigurnosna značajka da biste korisnicima onemogućili izvodi skripte komponente Windows PowerShell ako takvi stupci opasnosti od zlonamjernog softvera ili virusa koje obuhvaćaju izvršavanja skripti.

### <a name="run-the-script"></a>Pokrenuti skriptu

1. Stvaranje paketa implementacija Web projekta. Paket za implementaciju Web je sažete arhive (.zip datoteka) koja sadrži datoteke koje želite kopirati na web-mjesto ili virtualnog računala. Implementacija Web paketa možete stvoriti u Visual Studio za sve web-aplikaciju.

![Stvaranje Web uvesti paket](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Dodatne informacije potražite u članku [Kako: Stvaranje paketa za implementaciju Web u Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). Možete automatizirati i stvaranje paketa implementacija Web, kao što je opisano u odjeljku **Prilagodba i proširivanje skripte Objavi** u nastavku ovog članka.

1. U **Pregledniku rješenja**, otvara se Kontekstni izbornik za skripte, a zatim odaberite **Otvaranje s Očisti filtar**.

1. Ako je ovo prvi put pokrenete ste skripte komponente Windows PowerShell na ovom računalu, otvorite prozor naredbenog retka s administratorskim ovlastima i upišite sljedeću naredbu:

`Set-ExecutionPolicy RemoteSigned`

1. Prijavite se u Azure pomoću sljedeće naredbe.

`Add-AzureAccount`

Kada se to od vas zatraži, upišite svoje korisničko ime i lozinku.

Imajte na umu da kada automatizirati skriptu, ovaj način prikazivanja Azure vjerodajnice neće funkcionirati. Umjesto toga koristite .publishsettings datoteku da biste unijeli vjerodajnice. Jedna jedinica vremena samo, možete preuzeti datoteku s Azure pomoću naredbe **Get-AzurePublishSettingsFile** i nakon toga koristite **Uvoz AzurePublishSettingsFile** da biste uvezli datoteku. Detaljne upute potražite u članku [kako instalirati i konfigurirati Azure PowerShell](powershell-install-configure.md).

1. (Neobavezno) Ako želite stvoriti Azure resurse kao što su virtualnog računala, baze podataka i web-mjesto bez web-aplikacije za objavljivanje, upotrijebite naredbu **Objavi WebApplication.ps1** s na **-Konfiguracija** argument postavljen na JSON konfiguracijskoj datoteci. U ovom naredbenog retka koristi JSON konfiguracijska datoteka za određivanje resursa da biste stvorili. Jer koristi zadane postavke za ostale Argumenti naredbenog retka, on stvara resursa, ali ne objavite web-aplikacije. Dio – tekstni mogućnost pruža dodatne informacije o što se događa.

`Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json`

1. Pomoću naredbe **Objavi WebApplication.ps1** kao što je prikazano na jedan od sljedećih primjera pozivanje skripte i objavljivanje web-aplikacije. Ako vam je potrebna da biste nadjačali zadane postavke za sve druge argumente, kao što su naziv pretplate, objavljivanje naziv paketa, vjerodajnice virtualnog računala i vjerodajnice za poslužitelj baze podataka, možete odrediti te parametre. Korištenje na **– tekstni** mogućnost da biste pogledali dodatne informacije o tijeku simbol.

```
Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
–SubscriptionName Contoso `
-WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
-DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
-Verbose
```

Ako stvarate virtualnog računala, naredba izgleda ovako. U ovom se primjeru prikazuje i određivanje vjerodajnice za više baza podataka. Za virtualnim strojevima koji te skripte stvorili SSL certifikat nije od pouzdani korijenski za izdavanje certifikata. Stoga ćete morati koristiti mogućnost **– AllowUntrusted** .

```
Publish-WebApplication.ps1 `
-Configuration C:\Path\ADVM-VM-test.json `
-SubscriptionName Contoso `
-WebDeployPackage C:\Path\ADVM.zip `
-AllowUntrusted `
-VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
-DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
-Verbose
```

Skripta možete stvoriti baze podataka, ali se neće stvoriti bazu podataka poslužitelja. Ako želite stvoriti poslužitelj baze podataka, koristite funkciju **Novo AzureSqlDatabaseServer** u modul Azure.

## <a name="customizing-and-extending-the-publish-scripts"></a>Prilagođavanje i proširivanje skripte za objavljivanje

Možete prilagoditi Objavi skripte i JSON konfiguracijskoj datoteci. Funkcije u modul Windows PowerShell **AzureWebAppPublishModule.psm1** nije namijenjen mijenjati. Ako samo želite odrediti drugu bazu podataka ili promijeniti neka svojstva virtualnog računala, uredite JSON konfiguracijskoj datoteci. Ako želite proširiti mogućnosti funkcije skriptu da biste automatizirali stvaranje i testiranje projekta možete implementirati čvrstih funkcija u **Objavi WebApplication.ps1**.

Da biste automatizirali Stvaranje projekta, dodati kod koji poziva MSBuild za `New-WebDeployPackage` kao što je prikazano u ovom primjeru kod. Put do naredbe MSBuild razlikuje se ovisno o verziji programa Visual Studio ste instalirali. Da biste dobili ispravan put, koristite funkciju **Get-MSBuildCmd**, kao što je prikazano u ovom primjeru.

### <a name="to-automate-building-your-project"></a>Da biste automatizirali Stvaranje projekta

1. Dodavanje u `$ProjectFile` parametar u odjeljku Globalna parametarskog.

```
[Parameter(Mandatory = $false)]
  [ValidateScript({Test-Path $_ -PathType Leaf})]
  [String]
  $ProjectFile,
```

1. Kopiranje funkciju `Get-MSBuildCmd` u skriptna datoteka.

```
function Get-MSBuildCmd
{
        process
{

             $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                   Sort-Object {[double]$_.PSChildName} -Descending |
                                   Select-Object -First 1 |
                                   Get-ItemProperty -Name MSBuildToolsPath |
                                   Select -ExpandProperty MSBuildToolsPath
       
            $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

        return Get-Item $path
    }
}
```

1. Zamjena `New-WebDeployPackage` s sljedeću kod i zamijenite rezerviranih mjesta u retku izgradnje `$msbuildCmd`. Kod je za Visual Studio 2015. Ako koristite Visual Studio 2013, promijenite **VisualStudioVersion** svojstvo ispod `12.0`.

```
function New-WebDeployPackage
{
    #Write a function to build and package your web application
      
#To build your web application, use MsBuild.exe. For help, see MSBuild Command-Line Reference at: http://go.microsoft.com/fwlink/?LinkId=391339
      
Write-VerboseWithTime 'Build-WebDeployPackage: Start'
      
$msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory
      
Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
      
#Start execution of the build command
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru
      
if ($job.ExitCode -ne 0)
{
throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName
      
#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName
      
      
#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir
      
Write-VerboseWithTime 'Build-WebDeployPackage: End'
      
return $WebDeployPackage
}
```

1. Poziv u `New-WebDeployPackage` funkcija prije retkom: `$Config = Read-ConfigFile $Configuration` web-aplikacijama ili `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` za virtualnih računala.

```
if($ProjectFile)
{
$WebDeployPackage = New-WebDeployPackage
}
```

1. Pozivanje prilagođene skripte iz naredbenog retka pomoću Prenos na `$Project` argument kao primjer naredbeni redak.

```
.\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
-ProjectFile ..\WebApplication5\WebApplication5.csproj `
-VMPassword @{Name="VMUser";Password="Test.123"} `
-AllowUntrusted `
-Verbose
```

Da biste automatizirali testiranje aplikacije, dodavanje koda za `Test-WebApplication`. Pripazite da uklonite retke u **Objavi WebApplication.ps1** gdje se nazivaju te funkcije. Ako ne unesete implementacija, možete ručno stvaranje projekta s Visual Studio i pokrenuti skriptu Objavi da biste objavili Azure.

## <a name="publishing-function-summary"></a>Objavljivanje funkcija sažetka

Da biste dobili pomoć za funkcije možete koristiti u naredbenom retku komponente Windows PowerShell, upotrijebite naredbu `Get-Help function-name`. Pomoć sadrži parametar pomoć i primjeri. Isti tekst pomoći nije na izvorne datoteke skripte **AzureWebAppPublishModule.psm1** i **Objavljivanje WebApplication.ps1**. Na jeziku Visual Studio lokalizirane skripte i pomoć.

**AzureWebAppPublishModule**

|Naziv funkcije|Opis|
|---|---|
|Dodavanje AzureSQLDatabase|Stvara novu bazu podataka Azure SQL.|
|Dodavanje AzureSQLDatabases|Stvara baze podataka Azure SQL od vrijednosti u datoteci configuation JSON koje generira Visual Studio.|
|Dodavanje AzureVM|Stvara Azure virtualnog računala i vraća URL distribuiranih VM. Funkcija postavlja preduvjete i poziva funkciju **Novo AzureVM** (modul Azure) da biste stvorili novi virtualnog računala.|
|Dodavanje AzureVMEndpoints|Dodaje novi unos krajnje točke virtualnog računala i vraća virtualnog računala s novi krajnjoj točki.|
|Dodavanje AzureVMStorage|Stvara novi račun za Azure prostora za pohranu u trenutne pretplate. Naziv računa počinje s "devtest" slijedi jedinstveni alfanumerički niz. Funkcija vraća naziv novog računa za pohranu. Navedite mjesto ili grupu afinitet za novi račun za pohranu.|
|Dodavanje AzureWebsite|Stvara web-mjesto s navedenim nazivom i mjestom. Ova funkcija poziva funkciju **Novo AzureWebsite** u modul Azure. Ako pretplatu ne već sadrži web-mjesto s navedenim nazivom, ova funkcija stvara web-mjesta i vraća objekt web-mjesta. U suprotnom vraća `$null`.|
|Sigurnosno kopiranje pretplate|Spremanje trenutne pretplate Azure u na `$Script:originalSubscription` varijable u opsegu skripte. Ova funkcija sprema trenutne pretplate Azure (kao što je dobiti putem `Get-AzureSubscription -Current`) i njegov računa za pohranu i pretplatu za koju je promijenio ovu skriptu (spremljena u varijabli `$UserSpecifiedSubscription`) i njegov račun za pohranu u opsegu skripte. Spremanjem vrijednosti možete koristiti funkciju, kao što su `Restore-Subscription`, da biste vratili izvorne trenutne pretplate i pohranu račun trenutni status ako trenutni status promijenio.|
|Pronalaženje AzureVM|Dohvaća navedeni Azure virtualnog računala.|
|Oblikovanje DevTestMessageWithTime|Označava datum i vrijeme na poruku. Ova funkcija je namijenjen poruka napisanih strujanja pogreške i tekstni.|
|Get-AzureSQLDatabaseConnectionString|Spaja niz za povezivanje za povezivanje s bazom podataka Azure SQL.|
|Get-AzureVMStorage|Vraća naziv prvi račun za pohranu s uzorkom naziv "devtest*" (slova) u navedeno mjesto ili afinitet grupu. Ako na "devtest*" za pohranu računa ne ispunjavaju mjesta ili grupe afinitet, funkcija zanemaruje. Navedite mjesto ili grupe sustava afinitet.|
|Get-MSDeployCmd|Vraća naredbu da biste pokrenuli alat za MsDeploy.exe.|
|Novi AzureVMEnvironment|Pronalazi ili stvara virtualnog računala u pretplatu koja povezuje vrijednosti u konfiguracijskoj datoteci JSON.|
|Objavljivanje WebPaket|Koristi MsDeploy.exe i web-mjesto objavljivanje paketa. Zip datoteka za implementaciju resurse na web-mjesto. Ova funkcija ne generira bilo kakav izlaz. Ako poziv MSDeploy.exe ne uspije, funkcija throws iznimku. Da biste se detaljnije Izlaz, koristite na **-opširno** mogućnost.|
|Objavljivanje WebPackageToVM|Provjerava vrijednosti parametara, a zatim poziva funkciju **Objavi WebPaket** .|
|Čitanje ConfigFile|Provjeri valjanost datoteke za konfiguraciju JSON i vraća tablicu raspršivanje odabrane vrijednosti.|
|Vraćanje pretplate|Vraća trenutne pretplate na izvorne pretplate.|
|Test AzureModule|Vraća `$true` ako je verzija instaliranog modul Azure 0.7.4 ili noviji. Vraća `$false` ako modula nije instaliran ili starije verzije. Ova funkcija ima bez parametara.|
|Test AzureModuleVersion|Vraća `$true` ako je verzija modula Azure 0.7.4 ili noviji. Vraća `$false` ako modul nije instaliran ili starije verzije. Ova funkcija ima bez parametara.|
|Test HttpsUrl|Pretvara polazni URL System.Uri objekt. Vraća `$True` ako je apsolutna URL, a njegova shemu https. Vraća `$false` ako je relativna URL-a, njegov shemu nije HTTPS ili niz nije moguće pretvoriti u URL-a.|
|Testiranje člana|Vraća `$true` ako je svojstvo ili metodu član objekta. U suprotnom vraća `$false`.|
|Pisanje ErrorWithTime|Ispisuje se poruka o pogrešci mjestu s trenutnog vremena. Ova funkcija poziva funkciju **Oblik DevTestMessageWithTime** za prepend vrijeme prije pisanje poruke o pogrešci strujanje.|
|Pisanje HostWithTime|Zapisuje poruke na mjestu s trenutnog vremena program glavnog računala (**Pisanje-glavno računalo**). Mijenja se učinak zapisivanje u programu glavnog računala. Većina programa za koja hostiraju komponente Windows PowerShell za pisanje te poruke standardnog izlaza.|
|Pisanje VerboseWithTime|Ispisuje opširno poruku na mjestu s trenutnog vremena. Jer on poziva **Pisanje tekstni**, poruka se prikazuje samo pri pokretanju skripte s parametrom **tekstni** ili kada preferenca **VerbosePreference** postavljen na **Nastavi**.|

**Objavljivanje WebApplication**

|Naziv funkcije|Opis|
|---|---|
|Novi AzureWebApplicationEnvironment|Stvara Azure resurse, kao što je web-mjesto ili virtualnog računala.|
|Novi WebDeployPackage|Ova funkcija nije implementirana. Naredbi možete dodati u ovu funkciju da biste sastavili projekta.|
|Objavljivanje AzureWebApplication|Objavljuje web-aplikaciju za Azure.|
|Objavljivanje WebApplication|Stvara i implementira web-aplikacije, virtualnim strojevima, baze podataka SQL i račune za pohranu za projekta za Visual Studio web.|
|Testiranje WebApplication|Ova funkcija nije implementirana. Naredbi možete dodati u ovu funkciju da biste testirali aplikacije.|

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o PowerShell skriptiranje tako da pročitate [skriptiranje pomoću komponente Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) i pogledajte druge Azure PowerShell skripte u [Centar za skripte](https://azure.microsoft.com/documentation/scripts/).
