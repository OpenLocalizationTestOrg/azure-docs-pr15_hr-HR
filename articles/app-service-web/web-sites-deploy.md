<properties
    pageTitle="Implementacija aplikacije aplikacije servisa za Azure"
    description="Saznajte kako implementirati aplikaciju za aplikacije servisa za Azure."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cephalin;dariac"/>
    
# <a name="deploy-your-app-to-azure-app-service"></a>Implementacija aplikacije aplikacije servisa za Azure

U ovom se članku pomaže vam da odredite najbolja mogućnost za implementaciju datoteke za svoje web-aplikacijama, pozadinskog mobilne aplikacije ili API aplikacije za [Aplikacije servisa za Azure](http://go.microsoft.com/fwlink/?LinkId=529714)i vodi vas na odgovarajuće resurse s uputama za željenu mogućnost.

## <a name="overview"></a>Azure pregled aplikacije servisa za implementaciju

Aplikacije servisa za Azure održava framework aplikacije za vas (ASP.NET, PHP, Node.js itd). Neke okviri omogućena je prema zadanim postavkama tijekom drugima, kao što su Java i Python, možda će biti potrebno konfiguracije jednostavne kvačicu da biste ga omogućili. Osim toga, možete prilagoditi framework aplikacija, kao što su i verzija ili bitness vaše izvođenju. Dodatne informacije potražite u članku [Konfiguriranje aplikaciju u aplikacije servisa za Azure](web-sites-configure.md).

Budući da ne morate brinuti o web framework poslužitelja ili aplikacije, za implementaciju aplikacije u aplikaciju servisa pitanje je implementacija kodu, binarne datoteke, datoteke sa sadržajem i strukturu odgovarajući direktorija, [direktorija **/site/wwwroot** ](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) u Azure (ili **/web-mjesta/wwwroot/App_Data/poslove/** imeničkog za WebJobs). Aplikacije servisa za podržava sljedeće mogućnosti implementacije: 

- [FTP ili FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): korištenje omiljene FTP ili FTPS omogućeno alat za premještanje datoteka Azure, iz [FileZilla](https://filezilla-project.org) da biste punu IDEs kao što je [NetBeans](https://netbeans.org). Ovo je isključivo postupak za prijenos datoteka. Nema dodatna servisa nudi aplikacije servisa, kao što su kontrolu verzije, upravljanje datotekama strukturu, itd. 

- [Kudu (brojka/Mercurial ili OneDrive/Dropbox)](https://github.com/projectkudu/kudu/wiki/Deployment): koristite [Modul implementacije](https://github.com/projectkudu/kudu/wiki) u aplikacije servisa za. Automatske kod da biste Kudu izravno iz bilo kojeg spremište. Kudu omogućuje dodatnu usluge kad god kod je pomiču, uključujući kontrolu verzije, vraćanje paketa, MSBuild i [spojnica web](https://github.com/projectkudu/kudu/wiki/Web-hooks) za implementaciju neprekinuti i drugih automatizaciju zadataka. Modul za implementaciju Kudu podržava 3 različite vrste izvora implementacije:   
    * Sinkronizacija sadržaja sa OneDrive ili Dropbox   
    * Spremište sustavom neprekinuti implementaciju s automatsko sinkroniziranje sa GitHub, Bitbucket ili Visual Studio Team Services  
    * Spremište sustavom implementacija s ručnu sinkronizaciju s lokalnom brojka  

- [Implementacija web](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): uvođenja koda za aplikacije servisa za izravno iz programa Microsoft omiljene Alati kao što su Visual Studio koriste isti tooling koji automatizira implementacije IIS poslužiteljima. Ovaj alat podržava samo za razlika implementaciju, stvaranje baze podataka, a zatim pretvaranja nizove veze, itd. Uvođenje web razlikuje se od Kudu iz tog binarne datoteke iz aplikacije ugrađenih prije implementacije za Azure. Slično FTP, bez dodatne usluge nudi aplikacije servisa.

Alati za razvoj popularnih web podržava jedan ili više ovih procesa implementacije. Dok alat odaberete određuje procesa implementacije na raspolaganju, stvarni DevOps funkcionalnost na raspolaganju ovisi o kombinacije postupak implementacije i Alati za određene odaberete. Ako, na primjer, ako započnete implementacija Web iz [Visual Studio sa Azure SDK](#vspros), čak i ako ne primite Automatizacija od Kudu, dobit vraćanja paketa i automatizaciju MSBuild u Visual Studio. 

>[AZURE.NOTE] Ove postupke za implementaciju ne zapravo [Dodjela resursa za Azure](../resource-group-template-deploy-portal.md) koje možda morate aplikacije. Međutim, većina povezane članke vam pokazati kako Dodjela resursa za aplikaciju i njegova implementacija kod završetka do kraja. Možete pronaći i dodatne mogućnosti za dodjelu resursa Azure resursa u odjeljku [Automate implementaciju pomoću alata za naredbenog retka](#automate) .
     
## <a name="ftp"></a>Implementacija putem FTP kopiranjem datoteka za Azure ručno
Ako ste koristili za ručno kopiranje web-sadržaja na web-poslužitelj, možete koristiti uslužni program za [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) da biste kopirali datoteke, kao što su Windows Explorer ili [FileZilla](https://filezilla-project.org/).

Stručnjaka ručno kopiranja datoteke su:

- Dobro poznate značajke i minimalne složenosti za FTP tooling. 
- Kada zna točno gdje se premještaju datotekama.
- Zaštite s FTPS.

Protiv ručno kopiranja datoteke su:

- Imate li saznati kako uvođenje datoteka ispravna imenika aplikacije servisa. 
- Nema kontrole verzija za vraćanje kad se pojave pogreške.
- Nema povijesti ugrađene uvođenja za otklanjanje problema s implementacije.
- Potencijalne implementacije dugo vrijeme jer mnogi FTP Alati ne omogućuje samo za razlika kopiranja i jednostavno kopirajte sve datoteke.  

### <a name="howtoftp"></a>Kako implementirati kopiranjem datoteka za Azure ručno
Kopiranje datoteka Azure uključuje nekoliko jednostavnih koraka:

1. Ako već uspostavili vjerodajnice za implementaciju nabavite podatke o vezi FTP tako da otvorite **Postavke** > **Svojstva**, a zatim kopirajte vrijednosti za **FTP/implementaciju korisnika**, **Naziv glavnog računala FTP**i **FTPS naziv glavnog računala**. Kopirajte vrijednost korisnika **FTP/implementaciju korisnika** kao što je prikazano na portalu Azure uključujući naziv aplikacije da bi se pružaju kontekst proper FTP poslužitelja.
2. Iz FTP klijent koristiti podatke o vezi prikupili možete povezati s aplikacijom.
3. Kopirajte datoteke i njihovih strukturu odgovarajući direktorija [ **/site/wwwroot** direktorija](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) u Azure (ili **/web-mjesta/wwwroot/App_Data/poslove/** imeničkog za WebJobs).
4. Otvorite aplikaciju programa URL-a aplikacije provjerite je li pokrenut pravilno. 

Dodatne informacije potražite u sljedećim resursima:

* [Stvaranje web-aplikacijama i MySQL i implementacija pomoću FTP](web-sites-php-mysql-deploy-use-ftp.md).

## <a name="dropbox"></a>Implementacija po sinkronizirati s mapom oblaka
Dobar [Ručno kopiranje datoteka](#ftp) se sinkronizira datoteke i mape aplikacije servisa iz servis za pohranu u oblaku kao što su OneDrive i zajedničke mrežne mape. Sinkroniziranje mape oblaka koristi Kudu postupak implementacije (potražite u članku [Pregled implementacije procesa](#overview)).

Stručnjaci za sinkronizaciju s mapom oblaka su:

- Jednostavnosti implementacije. Servise kao što su OneDrive i zajedničke mrežne mape omogućuju klijenata za sinkronizaciju za stolna računala, pa je lokalni radni direktorij i implementaciju direktorija.
- Implementacija jednim klikom.
- Sve funkcije u implementaciju modul Kudu dostupna (npr. vraćanja paketa, automatizacija).

Protiv od sinkronizirati s mapom oblaka su:

- Nema kontrole verzija za vraćanje kad se pojave pogreške.
- Implementacija automatiziranog Ručna sinkronizacija nije potrebna.

### <a name="howtodropbox"></a>Kako implementirati po sinkronizirati s mapom oblaka
[Portal za Azure](https://portal.azure.com)možete odrediti mapu za sinkronizaciju sadržaja u oblak pohranom servisa OneDrive ili Dropbox, rad s kodom aplikacije i sadržaj te mape i sinkronizirati aplikacije servisa za klikom na gumb.

* [Sinkronizacija sadržaja iz mape oblaka za aplikacije servisa za Azure](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Implementacija neprestano iz servisa u oblaku izvora kontrole
Ako vaš tim za razvoj koristi servis management (IO) kod oblaku izvora kao što je [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com)ili [BitBucket](https://bitbucket.org/), možete konfigurirati aplikacije servisa za integrirati s vašeg spremište i implementacija kontinuirano. 

Profesionalce implementacije iz servisa u oblaku izvor kontrole su:

- Kontrola verzija da biste omogućili vraćanje.
- Mogućnost da biste konfigurirali neprekinuti implementacije za brojka (i Mercurial gdje je to primjenjivo) spremišta. 
- Specifične za podružnice implementaciju, možete implementirati različitim podružnicama za drugi [slobodnih](web-sites-staged-publishing.md).
- Sve funkcije u implementaciju modul Kudu dostupna (npr. implementacije rad s verzijama, vraćanje, vraćanje paketa, automatizacija).

Con implementacije iz servisa u oblaku izvora kontrole je:

- Neke znanja odgovarajući IO servis potrebna.

###<a name="vsts"></a>Kako implementirati neprestano iz servisa u oblaku izvora kontrole
[Portal za Azure](https://portal.azure.com)možete konfigurirati neprekinuti implementaciju sa GitHub, Bitbucket ili Visual Studio Team Services.

* [Continous implementacije aplikacije servisa za Azure](app-service-continuous-deployment.md). 

## <a name="localgitdeployment"></a>Implementacija iz lokalne brojka
Ako vaš tim za razvoj koristi za lokalni lokalni izvor kod (IO) servisa za upravljanje brojka na temelju, možete konfigurirati kao izvor implementaciju aplikacije servisa. 

Profesionalce implementacije iz lokalne brojka su:

- Kontrola verzija da biste omogućili vraćanje.
- Uvođenje granu specifične možete implementirati različitim podružnicama za različite [slobodnih](web-sites-staged-publishing.md).
- Sve funkcije u implementaciju modul Kudu dostupna (npr. implementacije rad s verzijama, vraćanje, vraćanje paketa, automatizacija).

Con implementacije iz lokalne brojka je:

- Neke znanja sustava IO odgovarajuća obavezno.
- Nema rješenja Uključi ključ neprekinuti implementacije. 

###<a name="vsts"></a>Kako implementirati iz lokalne brojka
[Portal za Azure](https://portal.azure.com)možete konfigurirati lokalnu implementaciju brojka.

* [Lokalni brojka implementacije aplikacije servisa za Azure](app-service-deploy-local-git.md). 
* [Objavljivanje na web-aplikacije s bilo kojeg repo brojka/hg](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Implementacija pomoću programa IDE
Ako već koristite [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) [Azure SDK](https://azure.microsoft.com/downloads/)ili druge pakete IDE kao što su [Xcode](https://developer.apple.com/xcode/), [Eclipse](https://www.eclipse.org)i [IntelliJ IDEJA](https://www.jetbrains.com/idea/), možete implementirati za Azure izravno iz unutar vaše IDE. Ova mogućnost idealna je za pojedinačne programiranje.

Visual Studio podržava sve tri implementaciju procese (FTP, brojka i implementacija Web), ovisno o postavkama, dok drugi IDEs možete implementirati aplikacije servisa ako imaju FTP ili brojka integracije (potražite u članku [Pregled implementaciju procesi](#overview)).

Stručnjaka implementacije pomoću programa IDE su:

- Potencijalno minimiziranje tooling za vaše aplikacije završetka do kraja životnog ciklusa. Razvoj, ispravljanje pogrešaka, praćenje i implementacija aplikacije Azure sve bez premještanja izvan vaše IDE. 

Protiv implementacije pomoću programa IDE su:

- Dodane složenosti u tooling.
- I dalje potreban izvorišnog sustava kontrole za projekt tima.

<a name="vspros"></a>Dodatni profesionalce implementacije Visual Studio pomoću Azure SDK su:

- Azure SDK čini Azure resursi jednostavno prva liga građana u Visual Studio. Stvaranje, brisanje, uređivanje, započeli, i prekid aplikacije, upit pozadinskom bazom podataka za SQL, live-ispravljanje pogrešaka aplikaciju Azure i još mnogo toga. 
- Live uređivanja datoteka kod na Azure.
- Live pogrešaka aplikacija na Azure.
- Integrirano Azure explorer.
- Samo za razlika implementacija. 

###<a name="vs"></a>Kako implementirati izravno iz Visual Studio

* [Početak rada s Azure i ASP.NET](web-sites-dotnet-get-started.md). Kako stvoriti i implementirati jednostavne projekta web ASP.NET MVC pomoću programa Visual Studio i implementacija Web.
* [Kako implementirati pomoću Visual Studio WebJobs Azure](websites-dotnet-deploy-webjobs.md). Upute za konfiguriranje aplikacije konzole za projekte tako da ih uvesti kao WebJobs.  
* [Implementacija aplikacije sigurne platforme ASP.NET MVC 5 članstvo, OAuth, i SQL baze podataka na web-aplikacije](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Kako stvoriti i implementirati programa project web ASP.NET MVC s bazom podataka sustava SQL pomoću Visual Studio, implementacija Web i entitet Framework kod prvog migracije.
* [Implementacija Web ASP.NET pomoću Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). Nizu 12-praktičnih vodiča koji prekriva potpuniji raspona zadataka uvođenja od ostalih na ovom popisu. Neke značajke Azure implementacije dodane su jer je zapisan vodič, ali bilješke dodane kasnije u članku se objašnjava što je nedostaju.
* [Implementacija ASP.NET web-mjesto za Azure u Visual Studio 2012 iz spremišta brojka izravno](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). U članku se objašnjava kako uvesti projekt web ASP.NET programa Visual Studio, pomoću brojka dodatak za primjenu kod brojka i Azure povezivanje sa spremištem brojka. Pokretanje u Visual Studio 2013, brojka podršku je ugrađene i ne zahtijeva instalacija dodatka.

###<a name="aztk"></a>Kako implementirati pomoću Kompleti alata Azure Eclipse i IntelliJ IDEJA

Microsoft omogućuje implementacija web-aplikacije Azure izravno iz Eclipse i IntelliJ putem [Azure komplet alata za Eclipse](../azure-toolkit-for-eclipse.md) i [Azure komplet alata za IntelliJ](../azure-toolkit-for-intellij.md). Sljedeći vodiči za ilustriraju koraci koje su povezane s implementacije jednostavne u "Pozdrav" svijeta Web App za Azure pomoću ili IDE:

*  [Stvorite pozdrav svijeta web-aplikaciju za Azure u Eclipse](./app-service-web-eclipse-create-hello-world-web-app.md). Pomoću ovog praktičnog vodiča pokazuje kako pomoću alata za Azure za Eclipse možete stvarati i implementirati pozdrav svijeta web-aplikacijama za Azure.
*  [Stvorite Zdravo svijeta web-aplikaciju za Azure u IntelliJ](./app-service-web-intellij-create-hello-world-web-app.md). Pomoću ovog praktičnog vodiča pokazuje kako pomoću alata za Azure za IntelliJ možete stvarati i implementirati pozdrav svijeta web-aplikacijama za Azure.


## <a name="automate"></a>Automatiziranje implementacije pomoću alata za naredbenog retka

* [Automatiziranje implementacija s MSBuild](#msbuild)
* [Kopiranje datoteke s FTP Alati i skripti](#ftp)
* [Automatiziranje implementaciju s komponentom Windows PowerShell](#powershell)
* [Automatiziranje implementaciju s upravljanjem .NET API-JA](#api)
* [Implementacija iz Azure sučelje naredbenog retka (Azure EŽA)](#cli)
* [Uvođenje iz implementacija Web naredbenog retka](#webdeploy)
* [Korištenje skupni FTP skripti](http://support.microsoft.com/kb/96269).
 
Druga implementacije je mogućnost korištenja servisa u oblaku kao što su [Octopus implementacija](http://en.wikipedia.org/wiki/Octopus_Deploy). Dodatne informacije potražite u članku [Implementacija ASP.NET aplikacija za Azure web-mjesta](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

###<a name="msbuild"></a>Automatiziranje implementacija s MSBuild

Ako koristite [Visual Studio IDE](#vs) za razvoj, [MSBuild](http://msbuildbook.com/) možete koristiti da biste automatizirali bilo što sve možete raditi u vašem IDE. Možete konfigurirati MSBuild da biste koristili [Implementaciju Web](#webdeploy) ili [FTP/FTPS](#ftp) da biste kopirali datoteke. Uvođenje web možete automatizirati i mnoge druge vezane uz implementaciju zadatke, na primjer implementacija baze podataka.

Dodatne informacije o naredbenog retka uvođenjem pomoću MSBuild potražite u sljedećim resursima:

* [ASP.NET Web implementacije pomoću Visual Studio: implementacija naredbenog retka](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Milimetar niza vodiče o implementaciji za Azure pomoću Visual Studio. U članku se objašnjava korištenje naredbenog retka za implementaciju nakon postavljanja objavite profila u Visual Studio.
* [Unutar modul Microsoft Sastavi: korištenje MSBuild i tima Foundation Sastavi](http://msbuildbook.com/). Teško Kopiranje knjige koja sadrži poglavlja o korištenju MSBuild radi implementacije.

###<a name="powershell"></a>Automatiziranje implementaciju s komponentom Windows PowerShell

Na raspolaganju vam MSBuild ili FTP implementacije funkcije iz [Komponente Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx). Ako to učinite, možete koristiti i skup cmdleta ljuske Windows PowerShell koji olakšavaju upravljanje Azure REST API-JA da biste uputili poziv.

Dodatne informacije potražite u sljedećim resursima:

* [Implementacija web-aplikacijama povezane s GitHub spremište](app-service-web-arm-from-github-provision.md)
* [Dodjela resursa web app s bazom podataka SQL](app-service-web-arm-with-sql-database-provision.md)
* [Dodjela resursa i implementacija microservices predvidljivije u Azure](app-service-deploy-complex-application-predictably.md)
* [Sastavni stvarnog života oblaka aplikacije s Azure - automatizirati sve](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-knjige poglavlja u kojem se objašnjava kako primjer aplikacije prikazana u adresar e koristi skripte komponente Windows PowerShell za stvaranje okruženju Azure test i implementacija u nju. Veze na dodatne dokumentaciju Azure PowerShell potražite u odjeljku [Resursi](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) .
* [Korištenje Windows PowerShell skripti za objavljivanje na web-mjesto razvojni i u okvir za testiranje okruženju](../vs-azure-tools-publishing-using-powershell-scripts.md). Kako da biste koristili Windows PowerShell implementaciju skripte koje generira Visual Studio.

###<a name="api"></a>Automatiziranje implementaciju s upravljanjem .NET API-JA

Možete napisati C# kod za izvođenje funkcije MSBuild ili FTP radi implementacije. Ako to učinite, možete pristupiti Azure upravljanja REST API-JA za izvršavanje funkcija upravljanja web-mjesta.

Dodatne informacije potražite u sljedećim resursima:

* [Automatizacija sve biblioteke upravljanja Azure i .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Uvod u upravljanje .NET API-JA i veze na dodatne dokumentacije.

###<a name="cli"></a>Implementacija iz Azure sučelje naredbenog retka (Azure EŽA)

U naredbenom retku na računalima sustava Windows, Mac i Linux možete koristiti za implementaciju pomoću FTP. Ako to učinite, možete pristupiti i upravljanje Azure REST API-JA pomoću EŽA Azure.

Dodatne informacije potražite u sljedećim resursima:

* [Alati za linijski azure naredba](/downloads/#cmd-line-tools). Stranica portala u Azure.com informacije alat naredbenog retka.

###<a name="webdeploy"></a>Uvođenje iz implementacija Web naredbenog retka

[Implementacija web](http://www.iis.net/downloads/microsoft/web-deploy) je Microsoftov softver za implementaciju u IIS koje se ne samo nudi značajke sinkronizacije Inteligentna datoteka, ali također mogu izvršiti ili koordiniranje mnoge druge implementaciju Zadaci vezani uz koje se ne može automatski kada koristite FTP. Na primjer, implementacija Web možete implementirati novu bazu podataka ili baze podataka ažuriranja zajedno s web-aplikaciju programa. Uvođenje web možete minimizirati i vrijeme potrebno da biste ažurirali postojeće web-mjesto jer energija možete kopirati samo promijenjene datoteke. Microsoft Visual Studio i poslužitelju Foundation timu imate podrška za implementaciju Web ugrađene, ali vam može poslužiti Web uvesti izravno iz naredbenog retka da biste automatizirali implementacije. Naredbe web uvođenja su vrlo moćne, ali krivulje učenje može biti steep.

Dodatne informacije potražite u sljedećim resursima:

* [Jednostavne web-aplikacije: implementaciju](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog tako da Nevena Ebbo o alatu on napisao radi lakšeg korištenja implementacija Web.
* [Alat za implementaciju web](http://technet.microsoft.com/library/dd568996). Službeni dokumentaciju na web-mjestu Microsoft TechNet. Datirana ali i dalje dobro mjesto za početak.
* [Pomoću Web implementacija](http://www.iis.net/learn/publish/using-web-deploy). Službeni dokumentaciju na web-mjestu Microsoft IIS.NET. I datirana ali dobro mjesto za početak.
* [ASP.NET Web implementacije pomoću Visual Studio: implementacija naredbenog retka](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild je modul Sastavi koristi Visual Studio, a može se koristiti iz naredbenog retka za implementaciju web-aplikacije na web-aplikacije. Pomoću ovog praktičnog vodiča je dio niza koji je uglavnom o implementaciji Visual Studio.

##<a name="nextsteps"></a>Daljnji koraci

U nekim slučajevima možda ćete moći ćete jednostavno prijeći natrag i naprijed na pripremna i verzija aplikacije. Dodatne informacije potražite u članku [Kopirana bez postavljanja implementacije na web-aplikacije](web-sites-staged-publishing.md).

Imate tarifu sigurnosnog kopiranja i vraćanja na mjestu je važan dio svaki implementacije tijek rada. Informacije o servisu aplikacije sigurnosno kopiranje i vraćanje značajku, pročitajte članak [Sigurnosne kopije Web Apps](web-sites-backup.md).  

Informacije o načinu korištenja kontrola pristupa na temelju uloga Azure korisnika za upravljanje pristupom za implementaciju aplikacije servisa za potražite u članku [RBAC i web-aplikacije objavu](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).


 
