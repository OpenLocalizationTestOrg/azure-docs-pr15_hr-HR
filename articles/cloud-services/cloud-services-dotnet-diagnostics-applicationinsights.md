<properties
   pageTitle="Rješavanje problema pomoću aplikacije uvida servise u Oblaku | Microsoft Azure"
   description="Saznajte kako otkloniti poteškoće problemi sa servisom cloud pomoću aplikacije uvida postupak podatke iz dijagnostike Azure."
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />


# <a name="troubleshoot-cloud-services-using-application-insights"></a>Rješavanje problema pomoću aplikacije uvida servise u Oblaku

S nastavkom Dijagnostika [2,8 SDK Azure](https://azure.microsoft.com/downloads/) i Azure 1,5 možete sada poslati podatke Azure Dijagnostika za servis u Oblaku izravno aplikacije uvid u. Zapisnika za razne vrste zapisnika prikupljene putem aplikacije zapisnike, zapisnika događaja sustava windows, uključujući dijagnostiku za Azure ETW i mjerača performansi mogu sada šalje aplikacije uvid u i vizualizirati u portal za aplikacije uvida korisničkog Sučelja. Kada se koristi uz uvida SDK aplikacije sada možete dobiti uvida u metriku i zapisnika koji dolaze iz aplikacije, kao i sustav i infrastrukture razine podatke koji dolaze iz dijagnostike Azure.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Konfiguriranje Azure Dijagnostika slanje podataka do uvida aplikacije

Slijedite ove korake za postavljanje servisa projekta oblaka slanje Azure Dijagnostika podataka do uvida aplikacije.

1) U pregledniku rješenja za Visual Studio desnom tipkom miša na ulozi pa odaberite **Svojstva** da biste otvorili dizajner uloga

![Svojstva uloga Explorer rješenja][1]

2) U dizajneru uloga u odjeljku Dijagnostika odaberite potvrdni okvir da biste **poslali Dijagnostika podataka do uvida aplikacije**

![Dizajner uloga poslati podatke Dijagnostika aplikacije uvid u][2]

3) U dijaloškom okviru koji se pojavljuje odaberite resurs uvida aplikacije koje želite poslati podatke Azure Dijagnostika. Dijaloški okvir omogućuje da biste odabrali postojeći resurs za uvid aplikacije iz pretplate ili ručno navesti ključa instrumentation resursa uvida aplikacije. Ako nemate postojeći resurs za uvid aplikacije pa možete stvoriti na klikom na vezu za **Stvaranje novog resursa** koji će se otvoriti prozor preglednika Azure klasični portalu kojem ćete stvoriti resursa za uvid u aplikacije. Dodatne informacije o stvaranju do uvida aplikacije resursa potražite u članku [Stvaranje nove uvide aplikacije resursa](../application-insights/app-insights-create-new-resource.md)

![Odaberite aplikaciju uvida resurs][3]

4) Kada dodate aplikacije uvida resursa, tipku instrumentation resursa pohranjuju se kao postavke za konfiguriranje usluge pod nazivom **APPINSIGHTS_INSTRUMENTATIONKEY**. Tu postavku konfiguracije za svaku konfiguracija servisa ili okruženje možete promijeniti tako da odaberete drugu konfiguraciju na padajućem izborniku konfiguracija servisa dolje i navođenjem novog ključa instrumentation za tu konfiguraciju.

![Konfiguracija servisa za odabir][4]

Postavke konfiguracije **APPINSIGHTS_INSTRUMENTATIONKEY** upotrebljava Visual Studio da biste konfigurirali proširenje Dijagnostika odgovarajuće podatke resursa uvida aplikacije tijekom objavljivanja. Postavke konfiguracije prikladan je način definiranja različite instrumentation tipke za različite servisne konfiguracije. Visual Studio će prijevod tu postavku i umetnite je u javnim konfiguraciju za nastavak Dijagnostika pri objavljivanju. Da biste pojednostavili postupak konfiguriranja proširenje Dijagnostika sa servisom PowerShell sadrži paket i izlaz iz Visual Studio javno konfiguracija XML pomoću odgovarajuće aplikacije uvida instrumentation dobili i ključ. Datoteka javno config stvaraju se u mapi Extensions i slijedite uzorak PaaSDiagnostics. <RoleName>. PubConfig.xml. Sve implementacije PowerShell temelji možete koristiti ovaj uzorak da biste mapirali svaki konfiguraciju u ulogu.

5) Omogućivanje **Šalji podatke o Dijagnostika aplikacije uvid u** automatski konfigurirao Azure dijagnostiku da biste poslali sve mjerača performansi i razine evidencije pogrešaka koje se prikuplja agent za Azure Dijagnostika aplikacije uvid u. Ako želite dodatno konfigurirati podacima koje se šalju do uvida aplikacije, a zatim ćete morati ručno urediti datoteku *diagnostics.wadcfgx* za svaku ulogu. Potražite u članku [Konfiguriranje Dijagnostika Azure slanje podataka do uvida aplikacije](../azure-diagnostics-configure-applicationinsights.md) da biste saznali više o ručno ažurirati konfiguraciju.

Provjerite nastavak Azure Dijagnostika omogućena kada servis u Oblaku je konfiguriran za slanje Azure Dijagnostika podataka do uvida aplikacije ga možete implementirati Azure kao što to obično činite. U odjeljku [servis u Oblaku pomoću Visual Studio za objavljivanje](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Prikaz Azure Dijagnostika podataka u aplikaciji uvida
Azure dijagnostičkih telemetrijskih prikazat će se u aplikaciju uvida resursa konfigurirana za servis u oblaku.

Ovo je kako na razne Azure zapisnika dijagnostike vrste karte koncepte uvida aplikacije:  

-  Mjerača performansi prikazuju se kao prilagođeni metriku u aplikaciji uvida
-  Zapisnike događaja sustava Windows prikazana su kao kašnjenja i prilagođena događaja u aplikaciji uvida
-  Aplikacije zapisnike, ETW zapisnika i sve infrastruktura za dijagnostiku zapisnika prikazuju se kao kašnjenja u aplikaciji uvida.

Da biste pogledali Azure Dijagnostika podataka u aplikaciji uvida:

- Pomoću [programa explorer metriku](../application-insights/app-insights-metrics-explorer.md) vizualizacija mjerača prilagođene performansi ni broji različite vrste događaje zapisnika događaja sustava windows.

![Prilagođeni metriku u programu Explorer mjerenja][5]

- Da biste pretražili različitim zapisnika praćenja poslao Dijagnostika Azure pomoću [pretraživanja](../application-insights/app-insights-diagnostic-search.md) . Za ako ste neobrađenu iznimku u ulozi koji je uzrok uloga ruši i koša za te podatke, primjerice bi prikazivati u *aplikaciji* kanal zapisnika *događaja sustava Windows*. Funkcije pretraživanja možete koristiti za pregled pogreške zapisnik događaja sustava Windows i praćenje cijelog stoga za iznimku omogućujući vam da biste pronašli uzrok problema.

![Pretraživanje kašnjenja][6]

## <a name="next-steps"></a>Daljnji koraci

- [Dodavanje uvida SDK aplikacije na servis u oblaku](../application-insights/app-insights-cloudservices.md) slanje podataka o zahtjeve iznimke, ovisnosti te sve prilagođene telemetrijskih iz aplikacije. U kombinaciji s Dijagnostika Azure podataka možete dobiti potpuni pregled aplikacije i sustav na isti resurs uvid aplikacije.  


<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
