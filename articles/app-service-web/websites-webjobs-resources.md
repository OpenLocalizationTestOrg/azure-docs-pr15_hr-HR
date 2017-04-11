<properties 
    pageTitle="Azure WebJobs dokumentaciju resursi" 
    description="Preporučuje se Resursi za učenje kako koristiti Azure WebJobs i Azure WebJobs SDK." 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="tdykstra"/>

# <a name="azure-webjobs-documentation-resources"></a>Azure WebJobs dokumentaciju resursi

## <a name="overview"></a>Pregled

Ovom temi navode veze na resurse dokumentaciju o načinu korištenja Azure WebJobs i Azure WebJobs SDK. Azure WebJobs pružaju jednostavan način da biste pokrenuli skripte ili programa kao pozadinski procesi u kontekstu [aplikacije servisa za web-aplikacije, aplikacija API-JA ili mobilnoj aplikaciji](../app-service/app-service-value-prop-what-is.md). Možete prenijeti i pokrenuti izvršne datoteke kao što su kao cmd, aplikacije za Outlook, exe (.NET), ps1, p, i Kopiraj, js i posudu. Ti programi pokrenuti kao WebJobs rasporedu (cron) ili kontinuirano.

Svrha [WebJobs SDK](websites-webjobs-resources.md) je da biste pojednostavnili kod u pišete za uobičajene zadatke u WebJob možete izvršiti, kao što su obrada slike, red čekanja obrade, RSS zbrajanja, datoteka održavanja i slanje poruke e-pošte. WebJobs SDK ima ugrađene značajke za rad s Azure i prostor za pohranu servisa Bus, za zakazivanje zadataka i rukovanja pogreškama i mnoge druge uobičajeni scenariji. Osim toga, vam omogućuje da vas se extensible, a postoji programa [otvorite spremištu izvora za proširenja](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Funkcija Azure](../azure-functions/functions-overview.md) (trenutno u pretpregledu) temelji se na verziju SDK WebJobs kojima radi C# skripte, Node.js i druge jezike. 

Stvaranje, uvođenje i upravljanje WebJobs je objedinjenog s Integriranom tooling u Visual Studio. Stvaranje WebJobs iz predložaka, objavljivanje i upravljanje (Pokreni/zaustavi/monitor/Provjera pogrešaka) ih. 

Na nadzornoj ploči WebJobs na portalu za Azure pruža mogućnosti Napredna upravljanja koji će vam potpunu kontrolu nad izvođenja WebJobs, uključujući mogućnost za pozivanje pojedinačne funkcija unutar WebJobs. Na nadzornoj ploči prikazuje i funkcija runtimes i zapisivanje izlaz. 

##<a name="getstarted"></a>Prvi koraci s WebJobs i WebJobs SDK

* [Uvod u Azure WebJobs](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs su fenomenalna i trebali biste ih mogli početi koristiti odmah!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Objava na blogu Troy Hunt.)
* [Azure WebJobs značajke](/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Što je WebJobs SDK](websites-dotnet-webjobs-sdk.md)
* [Upute za zadatke pozadine Microsoft uzoraka i postupci](/documentation/articles/best-practices-background-jobs/)
* [Objavljivanje na 1.1.0 RTM SDK WebJobs Microsoft Azure](/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Početak rada s Azure WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md)
* [Kako koristiti za pohranu Azure reda čekanja s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Kako koristiti blobova platforme Azure s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Kako koristiti spremište tablica platforme Azure s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Kako koristiti Bus servisa Azure s WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)
* [Kako koristiti WebHooks s SDK WebJobs primjerima za GitHub, IFTTT i HTTP](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/WebHooks-Walkthrough)
* [Azure WebJobs SDK brzi pregled (Preuzimanje PDF-a)](http://go.microsoft.com/fwlink/?LinkID=524028&clcid=0x409)
* [WebJobs postavke dokumentacije GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Videozapisi
    * [WebJobs i WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
    * [Azure WebJobs serija videozapisa s na kanal 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
    * [Uvod u WebJobs Tooling za Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs Tooling i daljinsko uklanjanje programskih pogrešaka](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
    * [Azure ažuriranje WebJobs s Pranav Rastogi - proširenja verzije 1.1](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Pogledajte i u sljedećim se odjeljcima [Implementacija WebJobs](#deploy) i [testiranje i ispravljanje pogrešaka WebJobs](#debug).

##<a name="deploy"></a>Implementacija WebJobs

* [Kako implementirati pomoću Visual Studio WebJobs Azure](websites-dotnet-deploy-webjobs.md)
* [Kako implementirati WebJobs pomoću portala za Azure](web-sites-create-web-jobs.md)
* [Omogućivanje naredbenog retka ili neprekinuti isporuku Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Brojka implementacije aplikaciju za .NET konzole za Azure pomoću WebJobs](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Implementacija programa F # WebJob za Azure](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Implementacija prilagođene servise kao Azure Webjobs](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Videozapisi
    * [Uvod u WebJobs Tooling za Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs Tooling i daljinsko uklanjanje programskih pogrešaka](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="schedule"></a>Planiranje WebJobs

* [Za dodavanje Azure WebJob dijaloški okvir](websites-dotnet-deploy-webjobs.md#configure)
* [Stvaranje zakazani WebJob na portalu za Azure](web-sites-create-web-jobs.md#CreateScheduled)
* [Hooking tako da na WebJob posao planer](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Planiranje Azure WebJobs cron izraza](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Planiranje pojedinačne WebJob funkcije pomoću WebJobs SDK TimerTrigger](websites-dotnet-webjobs-sdk.md#schedule)

##<a name="debug"></a>Testiranje i ispravljanje pogrešaka WebJobs

* [Novi za razvojne inženjere i ispravljanje pogrešaka značajke za Azure WebJobs u Visual Studio](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [Prikaz nadzorne ploče WebJobs](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Kako napisati zapisnika pomoću WebJobs SDK i njihov prikaz u nadzorne ploče](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Udaljena WebJobs za ispravljanje pogrešaka](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Koji je napisao te blob?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Hostiranje interaktivne kod u Oblaku](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Dodavanje praćenja Azure WebJobs](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Praćenje, dijagnosticiranje i otklanjanje poteškoća s spremište na platformi Microsoft Azure](../storage/storage-monitoring-diagnosing-troubleshooting.md)
* Videozapisi
    * [WebJobs Tooling i daljinsko uklanjanje programskih pogrešaka](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="scale"></a>Promjena veličine WebJobs

* [Promjena veličine web-aplikacije s web-mjesta za Azure](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Aplikacije servisa za azure: Architecting pretraživanje velikog skaliranje jeste li spremni za tvrtke Web Apps](https://channel9.msdn.com/Events/Build/2014/3-626). Naslovnice skaliranje web-aplikacije s WebJobs, uključujući WebJobs SDK.
* Videozapisi
    * [Promjena veličine izgleda WebJobs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

##<a name="additional"></a>Dodatni resursi WebJobs

* [Azure WebJobs GA objava na blogu Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Izvođenje zadataka Powershell Web na aplikacije servisa za Azure](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Početak obavijest kada vaš Azure pokrene WebJobs dovršava](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Jednostavan pravila zadržavanja sigurnosnu kopiju Web App s WebJobs](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure web-aplikacije i servise u Oblaku sporo na prvi zahtjev](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). U članku se objašnjava korištenje WebJobs za zamjenu za značajku AlwaysOn koja je dostupna samo za standardne cijene sloju.
* [WebJobs Graceful zatvaranja](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). Za SDK WebJobs graceful isključivanja, potražite u članku [Graceful zatvaranja](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Automatske sigurnosnog kopiranja Azure WebJobs & AzCopy](http://markjbrown.com/azure-webjobs-azcopy/)
* Videozapisi
    * [Azure WebJobs videozapise prema Magnus Mårtensson](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
    * [Azure WebJobs serija videozapisa s na kanal 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="additionalsdk"></a>Dodatni resursi WebJobs SDK

* [Napomene za SDK WebJobs](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [WebJobs SDK izvornog koda](https://github.com/Azure/azure-webjobs-sdk)
* [WebJobs SDK proširenja izvorni kod](https://github.com/Azure/azure-webjobs-sdk-extensions), s [detaljni vodič za proširivanje modela](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [Skripta SDK WebJobs izvornog koda](https://github.com/Azure/azure-webjobs-sdk-script/) (koristi se za [Azure funkcije](../azure-functions/functions-overview.md))
* [WebJob da biste prenijeli datoteke FREB pohranu Azure pomoću WebJobs SDK-a](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Hostiranje Azure webjobs izvan Azure s pogodnostima zapisivanje iz programa Azure hostira webjob](http://bypassion.dk/?p=510)
* [Stvaranje alat za uvoz podataka s Azure WebJobs](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Pregled Azure funkcija](../azure-functions/functions-overview.md)
* Videozapisi
    * [Azure WebJobs serija videozapisa s na kanal 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="samples"></a>Poslušajte WebJob aplikacije

* [Probne aplikacije dodijelio tim za WebJobs na GitHub](https://github.com/azure/azure-webjobs-sdk-samples)
* [Jednostavan Azure Web App s pozadinskom WebJobs pomoću WebJobs SDK-a](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Pokazuje korištenje WebJobs zakazano i utemeljenih na događaj. Pročitajte članak na blogu [novog SiteMonitR pomoću Azure WebJobs SDK](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs).

##<a name="blogs"></a>Blogovi

* [Azure bloga](/blog)
* [Amit Apple bloga](http://blog.amitapple.com/). Usredotoči na WebJobs (ne SDK).
* [Blog Magnus Mårtensson](http://magnusmartensson.com/)

##<a name="gethelp"></a>Pomoć za WebJobs

* [StackOverflow za WebJobs](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [StackOverflow za WebJobs SDK](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [StackOverflow za Azure funkcije](http://stackoverflow.com/questions/tagged/azure-functions)
* [Azure i platforme ASP.NET](http://forums.asp.net/1247.aspx)
* [Forum za Azure aplikacije servisa web-aplikacije](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure Web Apps korisnika govorne web-mjesta](https://feedback.azure.com/forums/169385-websites/)
* [Na twitteru](http://twitter.com/). Koristite oznaku #AzureWebJobs.
* [Izvješće WebJobs pogrešku ili problem](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

