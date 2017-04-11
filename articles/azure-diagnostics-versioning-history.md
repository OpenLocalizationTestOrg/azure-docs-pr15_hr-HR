<properties
    pageTitle="Povijest verzija Azure dijagnostiku"
    description="Objašnjenje promjene u različitim verzijama Azure Dijagnostika kao otpremljene s različitim verzijama Microsoft Azure SDK-ovi."
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="robb"/>


# <a name="microsoft-azure-diagnostics-version-history"></a>Povijest verzija dijagnostike sustava Microsoft Azure

Novi korisnik Azure Dijagnostika? Potražite u članku [Pregled Azure Dijagnostika](azure-diagnostics.md).

U svakoj verziji Azure SDK obično dolazi s novu verziju Dijagnostika Azure. U tablici u nastavku opisuju Azure SDK i Dijagnostika Azure verzije pridružene SDK izdanje.



Azure SDK verzija | Azure Dijagnostika verzija | Model
--- | --- | ---
1.x      | 1.0 | dodatak
2.0 2.4| 1.0 | "
2,5      | 1.2 | Proširenje
2.6      | 1.3 | "
2.7      | 1.4 | "
2,8      | 1,5 | "


Najnoviju verziju je 1,5 koji se isporučuju uz 2,8 SDK Azure. Verziju Azure Dijagnostika kućni broj koji se isporučuje uz SDK-a koristi se samo za lokalni emulator scenarije. Kada se pokrene u Azure, bez obzira koja je verzija aplikacije načinjene pomoću bilo koju aplikaciju distribuiranih automatski koristi najnoviju verziju. Međutim, osim ako ne instalirate najnovija SDK Azure, možda nemate sve alatima koji olakšavaju korištenje novih značajki.

Različite značajke i promjene koje su opisane u nastavku.

## <a name="azure-sdk-28"></a>Azure SDK 2,8
Azure SDK 2,8 dodali mogućnost slanje Dijagnostika podataka do [Uvida aplikacije](./application-insights/app-insights-cloudservices.md) olakšano dijagnosticiranje problema preko aplikacija, kao i razinu sustav i infrastrukture.

## <a name="azure-26-diagnostics-changes"></a>Azure 2,6 Dijagnostika promjene

Za projekte Azure SDK 2,6 u Oblaku u Visual Studio, sljedeće su izvršene. (Promjene se primijeniti i novijim verzijama Azure SDK.)

- Lokalni emulator sada podržava Dijagnostika. To znači da možete prikupiti podatke za dijagnostiku i provjerite je li aplikacija stvara desnom kašnjenja dok ste razvoj i testiranje u Visual Studio. Niz za povezivanje `UseDevelopmentStorage=true` omogućuje prikupljanje podataka Dijagnostika dok se izvodi oblaka servisa projekta u Visual Studio pomoću emulator Azure prostora za pohranu. Svi podaci za dijagnostiku prikupljaju se u račun za pohranu (razvoj spremište).

- Dijagnostika za pohranu računa niz veze (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) opet se pohranjuje u datoteci konfiguracije (.cscfg) servisa. U Azure SDK 2,5 nije naveden račun za dijagnostiku prostora za pohranu u datoteci diagnostics.wadcfgx.

Postoje neke najvažnije razlike između kako niz za povezivanje radili u Azure SDK 2.4 i noviji i kako funkcionira u 2,6 SDK Azure i novijim verzijama.

- U Azure SDK 2.4 i starijim verzijama, niz za povezivanje je koristio kao u vrijeme izvođenja dodatak za dijagnostiku da biste dobili informacije o računu za pohranu za prijenos dijagnostičkog zapisnika.

- Azure SDK 2,6 i novijim verzijama niz za povezivanje Dijagnostika koristi Visual Studio da biste konfigurirali proširenje Dijagnostika odgovarajuće prostora za pohranu podataka o računu na tijekom objavljivanja. Niz za povezivanje omogućuje određivanje prostora za pohranu za različite račune za različite servisne konfiguracija koji Visual Studio će se koristiti za objavljivanje. Međutim, jer dodatak za dijagnostiku više nije dostupan (nakon Azure 2,5 SDK), datoteka .cscfg sam ne možete omogućiti nastavak Dijagnostika. Morate omogućiti nastavak zasebno kroz alate kao što su Visual Studio ili PowerShell.

- Da biste pojednostavili postupak konfiguriranja proširenje Dijagnostika sa servisom PowerShell paketa Izlaz iz Visual Studio sadrži javno konfiguracija XML za dijagnostiku proširenja za svaku ulogu. Visual Studio koristi niz za povezivanje Dijagnostika za popunjavanje prostora za pohranu podataka o računu na koje su prisutne u javnom konfiguracije. Datoteka javno config stvaraju se u mapi Extensions i slijedite uzorak PaaSDiagnostics. <RoleName>. PubConfig.xml. Sve implementacije PowerShell temelji možete koristiti ovaj uzorak da biste mapirali svaki konfiguraciju u ulogu.

- Niz za povezivanje u datoteci .cscfg i koristi Azure portal za pristup podacima Dijagnostika tako da ga mogu se prikazivati na kartici **praćenja** . Konfiguriranje servisa za prikazivanje opširno nadzora podataka na portalu potrebno je niz za povezivanje.

### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migriranje projekata 2,6 SDK Azure i novijim verzijama

Prilikom migriranja iz Azure SDK 2,5 Azure SDK 2,6 ili noviji, ako imate račun za pohranu Dijagnostika naveden u datoteci .wadcfgx, zatim ostat će postoji. Da biste iskoristili fleksibilnost korištenja prostora za pohranu za različite račune za pohranu za različite konfiguracije, morat ćete niz za povezivanje ručno dodati u projekt. Ako projekt migrirate od Azure SDK 2.4 ili neke starije verzije za Azure SDK 2,6, sačuvati su Dijagnostika nizu za povezivanje. Međutim, uzmite u obzir promjene u kako nizove veze tretiraju u Azure SDK 2,6 kao što je navedeno u prethodnom odjeljku.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Kako Visual Studio određuje račun za pohranu dijagnostiku

- Ako niz za povezivanje dijagnostike nije naveden u datoteci .cscfg, Visual Studio je koristi za konfiguriranje proširenje Dijagnostika za objavljivanje i pri stvaranju xml datoteke javno konfiguracije tijekom pakiranje.

- Ako nema Dijagnostika niz za povezivanje nije naveden u datoteci .cscfg, zatim Visual Studio pada natrag da biste pomoću računa za pohranu naveden u datoteci .wadcfgx da biste konfigurirali proširenje Dijagnostika kada objavljivanje i generiranje xml datoteke javno konfiguracije kada pakiranje.

- Niz za povezivanje Dijagnostika u datoteci .cscfg ima prednost pred račun za pohranu u datoteci .wadcfgx. Ako niz za povezivanje dijagnostike nije naveden u datoteci .cscfg, zatim Visual Studio koji koristi i zanemaruje račun za pohranu u .wadcfgx.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Što znači "ažuriranje razvoj prostora za pohranu veze nizovi..." potvrdni okvir učiniti?

Potvrdni okvir **nizove veze ažuriranje razvoj prostora za pohranu za dijagnostiku i Međuspremanje s Microsoft Azure prostora za pohranu vjerodajnica računa pri objavljivanju Microsoft Azure** omogućuje praktičan način da biste ažurirali sve nizove razvoj prostora za pohranu računa povezivanja s računom Azure prostora za pohranu određenih tijekom objavljivanja.

Na primjer, pretpostavimo potvrdite ovaj okvir, a zatim niz za povezivanje Dijagnostika određuje `UseDevelopmentStorage=true`. Kad objavite projekt da biste Azure, Visual Studio ažurirat će se automatski Dijagnostika niz za povezivanje s računom za pohranu koje ste naveli u čarobnjaku za objavljivanje. Međutim, ako realni prostora za pohranu računa navedeno je kao niz za povezivanje Dijagnostika, zatim taj račun koristit će se.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Dijagnostika funkcionalnost razlike između Azure SDK 2.4 i starijim i Azure SDK 2.5 i noviji

Ako nadograđujete projekta s Azure SDK 2.4 Azure SDK 2,5 ili noviji, koje treba nose na umu sljedeće razlike funkcionalnost Dijagnostika.

- **Konfiguracija API -ji su ukinuta** – programski konfiguracije dijagnostike sustava je dostupna u Azure SDK 2.4 ili starijim verzijama, ali ukinuta je u Azure SDK 2,5 i novijim verzijama. Ako vašoj konfiguraciji Dijagnostika trenutno definirane u kodu, morat ćete ponovno konfigurira te postavke ispočetka u programu project migriranim redoslijedom dijagnostike sustava da biste nastavili raditi. Konfiguracijska datoteka Dijagnostika za Azure SDK 2.4 je diagnostics.wadcfg i diagnostics.wadcfgx Azure SDK 2,5 i novijim verzijama.

- **Dijagnostika za oblak servisne aplikacije mogu se konfigurirati na razini uloge, ne na razini instance.**

- **Svaki put kada implementacija aplikacije, konfiguracije Dijagnostika ažurira** – to može izazvati probleme sa slične značajke ako promijenite konfiguraciju Dijagnostika iz programa Explorer poslužitelja i implementirati aplikaciju.

- **U Azure SDK 2,5 i noviji, rušenje ispisi konfiguriran u konfiguracijskoj datoteci dijagnostike nije u kodu** – ako imate rušenje ispisi konfiguriran u kodu, morat ćete ručno prenijeti konfiguraciju iz koda konfiguracijska datoteka jer ispisi rušenje ne prenose tijekom migracije 2,6 SDK Azure.
