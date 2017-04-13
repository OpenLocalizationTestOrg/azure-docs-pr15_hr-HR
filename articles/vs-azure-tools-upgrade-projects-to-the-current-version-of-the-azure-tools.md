<properties
   pageTitle="Kako nadograditi projekata na trenutnu verziju alata za Azure | Microsoft Azure"
   description="Saznajte kako nadograditi Azure projekta u Visual Studio trenutnu verziju alata za Azure"
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

# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Kako nadograditi projekata na trenutnu verziju alata za Azure za Visual Studio

## <a name="overview"></a>Pregled

Nakon što instalirate trenutnom izdanju sustava Alati za Azure (ili prethodnog izdanja koja je novija od 1.6), sve projekata koje su stvorene pomoću alata za Azure otpustite prije 1.6 (studeni 2011) nadogradit će se automatski čim ih otvoriti. Ako ste stvorili projekte pomoću 1,6 izdanje (studeni 2011) od tih alata, a i dalje imate tu izdanje instaliran, možete otvoriti tih projekata starije verzije i kasnije odlučite želite li nadograditi ih.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Način mijenja projekta je nadogradnja

Ako projekt automatski nadograditi ili odredite želite li nadograditi, projektu Izmijenjeno za rad s trenutnim verzijama određene skupine, a neka svojstva mijenjaju se i kao u ovom se odjeljku opisuju. Ako projekt zahtijeva druge promjene nisu kompatibilne s novijom verzijom alata, te promjene morate napraviti ručno.

- Referentni novijom verzijom Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll se ažuriraju web.config datoteke za web uloge i app.config datoteke za uloge suradnika.

- Skupovi Microsoft.WindowsAzure.StorageClient.dll, Microsoft.WindowsAzure.Diagnostics.dll i Microsoft.WindowsAzure.ServiceRuntime.dll su nadograditi nove verzije.

- Objavljivanje profila pohranjenima u datoteku Azure projekta (.ccproj) premještaju se u zasebnu datoteku s .azurePubXml kućni broj, u podimeniku **Objavi** .

- Neka svojstva profila Objavi ažuriraju podržava nove i promijenjene značajke. **AllowUpgrade** zamijenjen je **DeploymentReplacementMethod** jer možete ažurirati distribuiranih oblaku istodobno ili postupno.

- Svojstvo **UseIISExpressByDefault** dodaje i postavite na false tako da se web-poslužitelju koji se koristi za ispravljanje pogrešaka neće se automatski promijeniti iz Internet Information Services (IIS) da biste izrazili IIS. IIS Express je zadani web-poslužitelj za projekte koje su stvorene pomoću noviju izdanja alata.

- Ako Azure predmemoriranje nalazi se u jednu ili više uloga u projekt, neka svojstva u Konfiguracija servisa (.cscfg datoteka) i servis definicija (.csdef datoteka) mijenjaju prilikom projekta. Ako projekt koristi paket NuGet predmemoriranje Azure, projekt će se nadograditi na najnoviju verziju paketa. Trebali biste otvoriti datoteku web.config i provjerite da konfiguracije klijenta je održava pravilno tijekom postupka nadogradnje. Ako ste dodali reference za predmemoriranje Azure sklopova klijenta bez korištenja paketa NuGet, neće se ažurirati te sklopova; morate ručno ažurirati te reference na nove verzije.

>[AZURE.IMPORTANT] Za F # projekte, morate ručno ažurirati reference na Azure sklopova tako da se koje se odnose na noviju verziju te sklopova.

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>Kako nadograditi Azure projekta na trenutnom izdanju

1. Instalirajte trenutnu verziju alata za Azure u instalaciju programa Visual Studio koji želite koristiti za nadograđena projekt, a zatim otvorite projekta kojim se želite li nadograditi. Ako projekt stvorena pomoću alata za Azure otpustite prije 1.6 (studeni 2011) projekt automatski nadograditi na najnoviju verziju. Projekt stvoren s studeni 2011 pustite i taj izdanje i dalje instaliran, projekta koji se otvara u tom izdanje.

1. U pregledniku rješenja otvaranje izbornika prečaca za čvor projekta, odaberite **Svojstva**i odaberite karticu **aplikacije** u dijaloškom okviru koji će se prikazati.

    Kartica **aplikacija** prikazuje verziju alata koji je pridružen projekta. Ako se pojavi na trenutnu verziju programa Azure Alati, već je nadograđen projekta. Ako ste instalirali novijom verzijom alata nego što se kartici prikazuje, prikazuje se gumb za **nadogradnju** .

1. Odaberite gumb **nadogradnje** za nadogradnju projekta na trenutnu verziju alata.

1. Stvaranje projekta, a adresu sve pogreške koje su rezultat promjena API-JA. Informacije o načinu izmjene kod za novu verziju potražite u dokumentaciji za određene API-JA.
