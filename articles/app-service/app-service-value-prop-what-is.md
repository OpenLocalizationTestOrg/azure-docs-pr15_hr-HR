<properties
    pageTitle="Azure aplikacije servisa za web, mobile i aplikacije API | Microsoft Azure"
    description="Saznajte kako aplikacije servisa za Azure pojednostavnjuje razviti, uvođenje i upravljanje web- a mobilne aplikacije."
    keywords="aplikacije servisa za, azure aplikacije servisa za, aplikacije servisa za cijena; skalabilni mjerilo, implementaciju aplikacije, azure aplikacije implementaciju, paas, platforme kao-na-usluga, web-mjesta, web-mjesta, web-mjesto azure mobilne"
    services="app-service"
    documentationCenter=""
    authors="omarkmsft"
    manager="erikre"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="omark"/>

# <a name="what-is-azure-app-service"></a>Što je aplikacije servisa za Azure?

*Aplikacija* je [platforme kao-na-usluga](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) koja se nudi sustava Microsoft Azure. Stvaranje web- a mobilne aplikacije za bilo koju platformu ili uređaj. Integriranje aplikacije s SaaS rješenja, povezivanje s lokalnim aplikacijama i automatizacija poslovnih procesa. Azure pokreće aplikacija na potpuno upravljanih virtualnim strojevima (VMs), sa zajedničkim resursima VM ili namjenski VMs.

Aplikacije servisa za obuhvaća web i mobilne mogućnosti koje ćemo prethodno isporučena zasebno kao Azure web-mjesta i servisa Azure Mobile. Obuhvaća i nove mogućnosti za hostiranje oblaka API-ji i automatizacija poslovnih procesa. Kao jedan servis integrirane aplikacije servisa omogućuje vam da sastavite razne komponente – web-mjesta, mobilne aplikacije natrag završava, RESTful API-ji i poslovnim procesima u sklopu – u jednom rješenja.

U sljedećem videozapisu 4-minutni naći kratko objašnjenje aplikacije servisa za odnosa starijim ponuda za Azure i što je novo u njoj.

+[AZURE.VIDEO app-service-history-lesson]

## <a name="why-use-app-service"></a>Zašto koristiti aplikacije servisa?

Slijede ključne značajke i mogućnosti aplikacije servisa:

- **Više jezika i okviri** – aplikacije servisa za ima jednostavno prva liga podršku za ASP.NET Node.js, jezika Java, i te Python. Možete pokrenuti i [komponente Windows PowerShell i druge skripte ili izvršne datoteke](../app-service-web/web-sites-create-web-jobs.md) na aplikaciju servisa VMs.

- **Optimizacija DevOps** – postavljanje [Neprekinuti integracije i implementacija](../app-service-web/app-service-continuous-deployment.md) za Visual Studio Team Services, GitHub ili BitBucket. Promicanje ažuriranja putem [test i pripremna okruženja](../app-service-web/web-sites-staged-publishing.md). Izvođenje [A / B testiranje](../app-service-web/app-service-web-test-in-production-get-start.md). Upravljanje aplikacijama u aplikacije servisa pomoću [Azure PowerShell](../powershell-install-configure.md) ili [više platformi sučelje naredbenog retka (EŽA)](../xplat-cli-install.md).

- **Globalnih Skaliranje s visoke dostupnosti** - skaliranje [prema gore](../app-service-web/web-sites-scale.md) ili prema [van](../monitoring-and-diagnostics/insights-how-to-scale.md) automatski ili ručno. Hostira aplikacija bilo gdje u infrastrukture globalni podatkovnog centra tvrtke Microsoft i aplikacije servisa [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) promises visoke dostupnosti.

- **Veze na platformama SaaS i lokalnih podataka** – odaberite s više od 50 [Poveznici](../connectors/apis-list.md) za sustave enterprise (kao što su SAP, Siebel i Oracle), SaaS (kao što su Salesforce i Office 365) i internet services (kao što su Facebook i Twitter). Pristup lokalnih podataka pomoću [Hibridnog veze](../biztalk-services/integration-hybrid-connection-overview.md) i [Azure virtualne mreže](../app-service-web/web-sites-integrate-with-vnet.md).

- **Sigurnost i usklađenost** – aplikacije servisa za je [ISO, PnP, i PCI usklađen](https://www.microsoft.com/TrustCenter/).

- **Predlošci aplikacija** – odaberite s popisa proširenom predlošci aplikacije [Servisa Azure Marketplace](https://azure.microsoft.com/marketplace/) omogućuju korištenje čarobnjaka za instaliranje popularne Otvori izvor softvera kao što su WordPress, Joomla i Drupal.

- **Integracija s programom visual Studio** - namjenski Alati u Visual Studio pojednostavniti rad stvaranja, uvođenje i ispravljanje pogrešaka.

## <a name="app-types-in-app-service"></a>Vrste aplikaciju u aplikacije servisa

Aplikacije servisa za nude nekoliko *Vrsta aplikaciju*, od kojih svaka namijenjen za hostiranje određene vrste radno opterećenje:

- [**Web-aplikacije**](../app-service-web/app-service-web-overview.md) - za hostiranje web-mjesta i web-aplikacije.

- [**Mobilne aplikacije**](../app-service-mobile/app-service-mobile-value-prop.md) Za hostiranje mobilne aplikacije sigurnosno završava.

- [**Aplikacija API**](../app-service-api/app-service-api-apps-why-best-platform.md) - za hostiranje RESTful API-ji.

- [**Logika aplikacije**](../app-service-logic/app-service-logic-what-are-logic-apps.md) - za automatizacija poslovnih procesa i integracija sustavi i podataka putem oblaka bez pisanja koda.

Word *aplikacije* Ovdje se odnosi na hostinga resursi za pokretanje radno opterećenje. Snimanja "web app", primjerice, ste vjerojatno navikli misle web-aplikacijama kao računalnim resurse i aplikacije kod koji zajedno isporuka funkcionalnost u pregledniku. Dok je u aplikacije servisa za *web-aplikacije* računalnim resursi koji Azure služi za hostiranje kodu aplikacije. Sastoji se aplikacija weba sučelje i RESTful API sigurnosno završi, oba web App nije moguće uvesti ili nije moguće uvesti sučelja kod web-aplikacijama i pozadinske kod da biste aplikaciju API-JA. Aplikacija se mogu sastojati od više aplikacije servisa za aplikacije od različitih vrsta.

## <a name="app-service-plans"></a>Aplikacije servisa za tarife

[Tarife za aplikaciju servisa](azure-web-sites-web-hosting-plans-in-depth-overview.md) navedite vrste računalnim resursi koji se pokrenuti aplikacije. Ako ste i očekivali opterećenje osnovnu promet, možete koristiti zajednički VMs (**slobodno** i **zajednički se koristi** cijene razine). Za veće opterećenje, možete odabrati s nekoliko veličine namjenski VMs (**Osnovni**, **standardne**i **Premium** razine). Više aplikacije servisa za aplikacije možete zajednički koristiti istu tarifu, a oni skaliranje gore i dolje cijene razine zajedno s planom.

Ako trebate još skalabilnost i odvajanja mrežu, možete pokrenuti aplikacije u [Okruženju aplikacije servisa](../app-service-web/app-service-app-service-environment-intro.md).

## <a name="pricing"></a>Cijene

Informacije o tome koliko aplikacije servisa za potražite u članku [Aplikacije servisa cijene](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Testiranje aplikacije servisa

[Stvaranje web-aplikacijama uzorka, aplikacije za mobilne uređaje ili logike aplikacije](http://go.microsoft.com/fwlink/?LinkId=523751) i reprodukciju s njim 1 h, s bez kreditne kartice obavezna, bez p nema gnjavaže.

Ili otvorite [besplatan račun za Azure](https://azure.microsoft.com/pricing/free-trial/)i isprobajte jedan od naše vodiči za početak rada:

* [Praktični vodič: Stvaranje web-aplikacijama](../app-service-web/app-service-web-get-started.md)
* [Praktični vodič: Stvaranje aplikacije za mobilne uređaje](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Praktični vodič: Stvaranje aplikacije API-JA](../app-service-api/app-service-api-dotnet-get-started.md)
* [Praktični vodič: Stvaranje aplikacije logike](../app-service-logic/app-service-logic-create-a-logic-app.md)
