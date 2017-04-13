<properties
   pageTitle="Sastavljanje naredbenog retka za Azure | Microsoft Azure"
   description="Sastavljanje naredbenog retka za Azure"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="command-line-build-for-azure"></a>Sastavljanje naredbenog retka za Azure

## <a name="overview"></a>Pregled

Paket za implementaciju Azure možete stvoriti tako da pokrenete MSBuild naredbeni redak. Možete konfigurirati i definirati izgradi za ispravljanje pogrešaka, pripremna i radni osim Automatizacija neke od postupka Sastavi.


## <a name="microsoft-build-engine-msbuild"></a>Modul Microsoft Sastavi (MSBuild)

Pomoću Microsoft sastavljanje modul (MSBuild) možete izraditi proizvode Sastavi Laboratorija okruženja kojem nije instaliran Visual Studio. MSBuild koristi XML oblik datoteke programa project koje extensible i potpuno podržane Microsoft. U ovom obliku datoteke možete opisati što artikli moraju biti u komponenti za platforme i konfiguracije.

Možete i pokrenuti MSBuild naredbeni redak, a u ovoj se temi opisuju taj pristup. Postavljanjem svojstava u naredbenom retku možete sastaviti određene konfiguracije projekta. Isto tako, možete definirati i ciljeve koje će izraditi naredbu MSBuild. Dodatne informacije o parametara naredbenog retka i MSBuild potražite u članku [Referenca MSBuild naredbenog retka](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="installation"></a>Instalacija

Kao u sljedećem je postupku opisano, morate instalirati softver i alati na poslužitelju Sastavi da biste mogli stvarati paketa za Azure pomoću MSBuild:

1. Instalacija 4 za .NET Framework ili noviju verziju, koja sadrži MSBuild.

1. Instalacija [Alata za stvaranje Azure](http://go.microsoft.com/fwlink/?LinkId=394615) (potražite MicrosoftAzureAuthoringTools x64.msi ili MicrosoftAzureAuthoringTools x86.msi.

1. Instalacija [Azure biblioteke za .NET](http://go.microsoft.com/fwlink/?LinkId=394616) (potražite MicrosoftAzureLibsForNet x64.msi ili MicrosoftAzureLibs x86.msi.

1. Kopirajte datoteku Microsoft.WebApplication.targets iz instalacije za Visual Studio na drugom računalu.

    Datoteka se nalazi u c:\Programske datoteke (x86) \MSBuild\Microsoft\Visual Studio\v12.0\WebApplications direktorija (v11.0 za Visual Studio 2012), a trebali biste kopirate u istu direktorij na poslužitelju Sastavi.

1. Instalirajte [Azure alate za Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616).

    Potražite WindowsAzureTools.vs120.exe da biste sastavili Visual Studio 2013 projekata.

## <a name="msbuild-parameters"></a>Parametri MSBuild

Najjednostavniji način za stvaranje paketa jest da biste pokrenuli MSBuild s na `/t:Publish` mogućnost. Prema zadanim postavkama ta se naredba stvara direktorij odnosu korijenske mape za projekt, kao što su ProjectDir\bin\Configuration\app.publish\. prilikom stvaranja Azure projekta, generiranje dvije datoteke, samu datoteku paketa i popratnu konfiguracijska datoteka:

- Project.cspkg

- ServiceConfiguration.TargetProfile.cscfg

Prema zadanom svaki Azure projekt uključuje sastavlja datoteke konfiguracija servisa za lokalne (ispravljanje pogrešaka), a drugi izgradi oblaka (Održavanje ili proizvodnju), ali možete dodati ili ukloniti konfiguracija servisa datoteke prema potrebi. Prilikom stvaranja paketa programa Visual Studio, zatražit će se datoteka koje konfiguracija servisa da biste uključili uz paket. Prilikom stvaranja paket pomoću MSBuild, po zadanom uključena je lokalna datoteka konfiguraciju servisa. Da biste dodali drugu datoteku konfiguracija servisa, postavite na `TargetProfile` svojstvo naredbe MSBuild (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Ako želite koristiti alternativni imenik pohranjene paketa i datoteka konfiguracije postavite pomoću na `/p:PublishDir=Directory\` mogućnost, uključujući završne obrnute kose crte razdjelnika.

## <a name="deployment"></a>Uvođenje

Nakon što je ugrađena paketa, možete je uvesti za Azure. Praktični vodič koji pokazuje taj postupak, potražite u članku Azure web-mjesta. Informacije o tome da biste automatizirali taj proces potražite u članku [Neprekinuti isporuke za servise u Oblaku u Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).
