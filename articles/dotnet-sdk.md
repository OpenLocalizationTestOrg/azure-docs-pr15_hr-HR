<properties
    pageTitle="Što je Azure .NET SDK"
    description="Saznajte što je sve obuhvaćeno Azure .NET SDK."
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="what-is-the-azure-sdk-for-net"></a>Što je Azure SDK za .NET?

## <a name="overview"></a>Pregled

Azure SDK za .NET je skup alata za Visual Studio, alati naredbenog retka, izvođenja binarne datoteke i klijenta biblioteke koje olakšavaju razvoj, testiranje i implementacija aplikacije koji se izvode na Azure. U ovom se članku detaljno što se kada instalirate SDK-a. SDK možete preuzeti sa [stranice s preuzimanjima Azure](https://azure.microsoft.com/downloads/).

Azure SDK za .NET sadrži i [biblioteke klijenta za dosta Azure servise](http://go.microsoft.com/fwlink/?LinkId=510472). Te biblioteke instaliraju zasebno pomoću NuGet.

##<a id="included"></a>Što je sve obuhvaćeno Azure SDK za .NET

Azure SDK za .NET instalira sljedeće proizvode:

- [Visual Studio zajednice Edition 2015.](#vwd)
- [Emulator prostora za pohranu za Microsoft Azure](#stgemulator)
- [Alati za pohranu za Microsoft Azure](#stgtools)
- [Alata za stvaranje Microsoft Azure](#auth)
- [Emulator Microsoft Azure](#emulator)
- [Alati za HDInsight za Visual Studio i Microsoft grozd ODBC upravljački program](#hdinsight)
- [Microsoft Azure biblioteke za .NET](#libraries)
- [Microsoft Azure mobilne aplikacije SDK](#mobile)
- [PowerShell Microsoft Azure](#ps)
- [Alati Microsoft Azure za Microsoft Visual Studio](#tools)
- [Microsoft ASP.NET i web-Alati za Visual Studio](#wte)
- [Alati Microsoft Azure podataka Lake za Visual Studio](#datalake)

###<a id="vwd"></a>Visual Studio zajednice Edition 2015.

Ako na računalu nemate Visual Studio, SDK instalirat će [Visual Studio 2015 Edition zajednice](https://www.visualstudio.com/products/visual-studio-community-vs).

###<a id="stgemulator"></a>Emulator prostora za pohranu za Microsoft Azure

[Azure prostora za pohranu Emulator](http://msdn.microsoft.com/library/hh403989.aspx) koristi instancu komponente SQL Server i lokalnom sustavu datoteka u programu Publisher prostora za pohranu Azure (redovi, tablice, blob-ova), tako da možete testirati lokalno.

###<a id="stgtools"></a>Alati za pohranu za Microsoft Azure

Time se instalira [AzCopy](http://aka.ms/AzCopy), alat naredbenog retka možete koristiti za prijenos podataka u i Odjava iz njega račun za Azure prostora za pohranu.

###<a id="auth"></a>Alata za stvaranje Microsoft Azure

To obuhvaća sljedeće:

* [Alat za CSPack naredbenog retka](http://msdn.microsoft.com/library/gg432988.aspx) za stvaranje paketa za implementaciju.
* [CSEncrypt naredbenog retka alat](http://msdn.microsoft.com/library/hh404001.aspx) za šifriranje lozinki koje se koriste za pristup instanci uloge servisa za oblak putem udaljene radne površine.
* Izvođenje binarne datoteke koje projekata servis u oblaku potrebni za komunikaciju s svoje okruženje za izvođenje i dijagnostike sustava. Ove binarne datoteke nisu dostupne u NuGet paketa.

###<a id="emulator"></a>Emulator Microsoft Azure

[Azure Emulator](http://msdn.microsoft.com/library/dn339018.aspx) simulira servisa okruženje oblaka tako da možete testirati oblaka servisa projekata lokalno na vašem računalu prije implementacije za Azure.

###<a id="hdinsight"></a>HDInsight alate za Visual Studio i Microsoft grozd ODBC upravljački program

Alati za HDInsight u programu Explorer poslužitelja omogućuju kretati grozd baze podataka i povezane prostora za pohranu računa za HDInsight klastere, stvaranje tablice i stvaranje i slanje upita grozd. Dodatne informacije potražite u članku [Prvi koraci pri korištenju HDInsight Hadoop alate za Visual Studio](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

###<a id="libraries"></a>Microsoft Azure biblioteke za .NET

To obuhvaća sljedeće:

* NuGet paketi za pohranu Azure, Bus servisa i Međuspremanje pohranjene na računalu Visual Studio možete stvoriti novu oblaka servisa projekata tijekom izvanmrežnog.
* U Visual Studio dodatak koji omogućuje projekata [u ulozi predmemorije](http://msdn.microsoft.com/library/dn386103.aspx) da biste pokrenuli lokalno u Visual Studio.

###<a id="mobile"></a>Microsoft Azure mobilne aplikacije SDK

Alati za rad s [Azure servisa mobilna aplikacija](app-service-mobile/app-service-mobile-value-prop.md).

###<a id="ps"></a>PowerShell Microsoft Azure

Azure PowerShell omogućuje vam da biste [automatizirali Azure okruženje stvaranja i implementacija](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything).

###<a id="tools"></a>Alati Microsoft Azure za Microsoft Visual Studio

Omogućuje rad s Azure resursima prvenstveno servisa u Oblaku i virtualnih računala:

* [Stvaranje, otvorite, i objavljivanje oblaka servisa projekata](cloud-services/cloud-services-dotnet-get-started.md).
* [Stvaranje implementaciju paketa za projekte servisa oblaka](http://msdn.microsoft.com/library/ff683672.aspx).
* [Stvaranje virtualnim računalima sustava Azure prilikom stvaranja novih web projekata](virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md).
* [Stvaranje skripte komponente PowerShell prilikom stvaranja novog virtualnih računala](http://msdn.microsoft.com/library/dn642480.aspx).
* [Prikaz oblaka servisa project postavki u sustavu windows svojstva projekta za Visual Studio i upravljanje njima](http://msdn.microsoft.com/library/ee405486.aspx).
* Prikaz i upravljanje [servise u oblaku](http://msdn.microsoft.com/library/ff683675.aspx), [virtualnim strojevima](http://msdn.microsoft.com/library/jj131259.aspx)i [Bus servisa](http://msdn.microsoft.com/library/jj149828.aspx) u programu Explorer poslužitelja.
* [Pokreni u načinu rada za ispravljanje pogrešaka daljinski za servise u oblaku i virtualnih računala](http://msdn.microsoft.com/library/ff683670.aspx).
* [Automatiziranje resursa dodjele resursa pomoću Azure resursa grupne implementacije projekte](https://msdn.microsoft.com/library/dn872471.aspx)

###<a id="wte"></a>Alati Microsoft aplikacije servisa za Visual Studio

Omogućuje rad s web-mjesta Azure:

* [Objavljivanje web projekata Azure web-mjesta](app-service-web/web-sites-dotnet-get-started.md).
* [Objavljivanje konzole aplikacije projekata Azure WebJobs](app-service-web/websites-dotnet-deploy-webjobs.md).
* [Stvaranje Azure web-mjesta i SQL baze podataka resursa prilikom stvaranja novog projekta web ili tijekom objavljivanja web projekta](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
* [Stvaranje skripte komponente PowerShell implementacije prilikom stvaranja novog web-mjesta](http://msdn.microsoft.com/library/dn642480.aspx).
* [Upravljanje i otklanjanje poteškoća s Azure web-mjesta u programu Explorer poslužitelja](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement).
* [Pokreni u načinu rada za ispravljanje pogrešaka daljinski za web-mjesta i WebJobs](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).

>[AZURE.NOTE] Ne morate instalirati Azure SDK za .NET da biste koristili te značajke Također se uključuju u Visual Studio ažuriranja.

##<a id="datalake"></a>Alati Microsoft Azure podataka Lake za Visual Studio

Dodatne informacije potražite u članku [Praktični vodič: razvoj U SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

##<a id="notincluded"></a>Što je uključeno kada instalirate Azure SDK za .NET

Postoji nekoliko stvari koje biste mogli za Azure razvoj koji nisu uvršteni kada instalirate SDK-a. Najvažnije od ovih su sljedeće:

* [Biblioteka klijenta](http://go.microsoft.com/fwlink/?LinkId=510472).

    Azure SDK uključuje biblioteke klijenta za troše Azure services, ali ne sve instaliraju kada i SDK-a. Ako aplikacija treba klijentska biblioteka koje nije moguće instalirati SDK-a, možete je zatražite od [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Ako aplikacija koristi biblioteci klijenta koji instaliraju SDK, je dobro da biste ažurirali s trenutnom verzijom pri NuGet.org.

    **Lokalne kopije biblioteke klijenta.** Azure SDK za .NET kopira na vaše računalo NuGet paketi za neke biblioteke Azure klijenta, kao što je prostor za pohranu, Bus servisa i Međuspremanje. Te biblioteke klijenta automatski se uključiti u nove projekte servisa oblaka, pa lokalne paketa NuGet omogućivanje programa Visual Studio stvaranje projekata čak i ako niste povezani s Internetom. Biblioteka klijentski uglavnom se ažuriraju češće od objavljivanja nove verzije SDK tako bibliotekama klijenta na NuGet.org često su trenutno od se s SDK-a.

    **Project predloške koji sadrže biblioteke klijenta.** Samo [Azure u Oblaku](cloud-services/cloud-services-dotnet-get-started.md) i aplikacije servisa za Azure [Mobilne aplikacije](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) predlošcima projekata automatski uključiti neke biblioteke klijenta. Za druge biblioteke i ostale predloške, instalirajte [klijent biblioteke NuGet pakete](http://go.microsoft.com/fwlink/?LinkId=510472) koje su vam potrebne.

* [Mobilne aplikacije projektni predlošci](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

    Mobilne aplikacije Predlošci su dostupne samo u Visual Studio 2013 ažuriranje 2 ili novije. Nisu dostupne u Visual Studio 2012 i starijim verzijama, a ne u Visual Studio 2013 Update 1 ili starijim verzijama, čak i ako instalirate Azure SDK za .NET.

##<a id="faq"></a>Najčešća pitanja

- [Mnoge značajke Azure se već nalaze u Visual Studio. Morate instalirati Azure SDK za .NET?](#azinvs)
- [Želim da se klijentska biblioteka. Imati da biste instalirali Azure SDK za .NET da biste podesili?](#clientlib)
- [Gdje pronaći starije verzije programa Azure SDK za .NET?](#olderversions)
- [Što je Pravilnik za životni ciklus za verzije Azure SDK za .NET?](#lifecycle)
- [Koje verzije OS goste Azure SDK za .NET kompatibilan je s?](#guestos)
- [Kako deinstalirati Azure SDK za .NET?](#uninstall)

###<a id="azinvs"></a>Mnoge značajke Azure se već nalaze u Visual Studio. Morate instalirati Azure SDK za .NET?

Dobro da biste instalirali SDK ako želite razviti za Azure pomoću alata za najnovija je. Ako biste radije ne instalirate SDK, to možete učiniti ako su ispunjeni sljedeći uvjeti:

* Koje ste instalirali najnovija [Ažuriranja za Visual Studio](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5).
* Ako razvijate samo za Azure web-mjesta ili servise Mobile, ne i za servise u Oblaku ili virtualnim računalima.
* Aplikacija ne koriste prostor za pohranu ili koristi za pohranu, ali ne trebate Emulator prostora za pohranu ili alat za AzCopy.

###<a id="clientlib"></a>Želim da se klijentska biblioteka. Imati da biste instalirali Azure SDK za .NET da biste podesili?

SDK instalira klijent biblioteke samo tako da možete stvoriti oblaka servisa projekata čak i ako niste povezani s Internetom. Trenutni klijent biblioteke dostupne su u NuGet paketa pri [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Dodatne informacije potražite u članku [što je uključeno kada instalirate Azure SDK za .NET](#notincluded) ranije u ovom dokumentu.

###<a id="olderversions"></a>Gdje pronaći starije verzije programa Azure SDK za .NET?

Starije verzije potražite u članku [Azure SDK za .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) preuzimanja stranice.

###<a id="lifecycle"></a>Što je Pravilnik za životni ciklus za verzije Azure SDK za .NET?

Potražite u članku [Microsoft Azure servise u Oblaku podržava Pravilnik za životni ciklus](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq).

###<a id="guestos"></a>Koje verzije OS goste Azure SDK za .NET kompatibilan je s?

U odjeljku [Azure goste OS izdanja i SDK kompatibilnosti matrice](http://msdn.microsoft.com/library/ee924680.aspx).

###<a id="uninstall"></a>Kako deinstalirati Azure SDK za .NET?

Svaka stavka prikazana u ovom članku u odjeljku [što je sve obuhvaćeno Azure SDK za .NET](#included) je zasebnom program na popisu instaliranih programa na upravljačkoj ploči sustava Windows **Programi i značajke**.  Ne postoji način da biste deinstalirali ih kao grupu; morate deinstalirati svaki program pojedinačno.

Kada imate Azure SDK za .NET već instaliran i instalirati novu verziju, obično je riječ o nema potrebe da biste deinstalirali staru. U većini slučajeva, instalacija SDK ažurira postojećeg programa umjesto dodavanja nove i ostavite staru.

Međutim, ako želite ukloniti bez-dulje-potrebno remnants starije verzije deinstalirati samo programe koji određuju broj starije verzije, a samo ih deinstalirati ako se isti program novijom verzijom. Ako, na primjer, nakon postavljanja od 2,5 u 2,6 možda ćete vidjeti verzije 2.5 i 2.6 "Microsoft Azure alata za Microsoft Visual Studio 2013", a možete deinstalirati 2.5 verziju. No mogu vidjeti samo 2.5 verziju "Microsoft Azure alata za stvaranje", a ne može biti sigurni da biste deinstalirali koji.

Imajte na umu brojevi verzija u naslove programa prikazan u odjeljku **Programi i značajke** mogu biti netočni.  Na primjer, SDK verziju 2,6 uključuje "Microsoft Azure mobilne aplikacije SDK V1.0" Ako instalirate SDK za Visual Studio 2013 i "Microsoft Azure mobilne aplikacije SDK 2.0" za Visual Studio 2015; verzija u tom slučaju nije SDK verzija, ali pokazatelja koje verzije Visual Studio program primjenjuje se na.

U ovom se članku popis svaki program uvrštene svaki starijoj verziji programa Azure SDK; postoje drugi programi možete deinstalirati iz starijih verzija SDK jer starije verzije SDK ponekad uključena programe koji su izostavljena u novijim verzijama. Na primjer, verzija 2,5 instalira "Azure resursa Upravitelj Alati za Visual Studio", ali koji nije na popisu u ovom članku jer više ne prikazuje kao zasebna program u odjeljku **Programi i značajke**.  U ovom se članku se navodi samo programe koji su uključeni u Azure SDK za .NET verzije 2,6.

> **Bilješke:** Neki od programa koji uključuje SDK može biti instaliran i neovisno u drugim kontekstima i možda će biti potrebno čak i ako ne trebate cijelog SDK. Isti može biti zadovoljen programa koji su instalirani starije verzije SDK, ali su izostavljena u novijim verzijama SDK. Kada deinstalirate programe, pripazite da biste izbjegli uklonite nešto što se i dalje potreban je na računalu.



##<a id="versions"></a>Verzija

Da biste vidjeli koja je verzija trenutni ili da biste preuzeli starije verzije, posjetite stranicu [Azure SDK za .NET povijest verzija](https://azure.microsoft.com/downloads/archive-net-downloads/) .

##<a id="resources"></a>Resursi

Da biste preuzeli trenutnu Azure SDK za .NET ili biblioteci klijent, posjetite [stranice s preuzimanjima Azure](https://azure.microsoft.com/downloads/).

Azure SDK za .NET izvornog koda, uključujući biblioteke klijent, potražite u članku [GitHub.com/Azure](https://github.com/azure/).

Azure klijenta biblioteke referentnu dokumentaciju potražite u članku [Referenca za .NET Azure](https://azure.microsoft.com/documentation/api/).
