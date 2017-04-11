<properties
    pageTitle="Web-aplikacije pregled | Microsoft Azure"
    description="Saznajte kako aplikacije servisa za Azure pomaže vam pri razvoju i glavno računalo web-aplikacije"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="cephalin"/>

# <a name="web-apps-overview"></a>Web-aplikacije pregled

*Web-aplikacije servisa za aplikaciju* je potpuno upravljanih računalnim koja je optimizirana za hostiranje web-mjesta i web-aplikacije. Ovu ponudu [platforme kao-na-usluga](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) sustava Microsoft Azure omogućuje vam fokus na poslovne logike dok Azure vodi brigu o infrastrukture za pokretanje i skaliranje aplikacija.

Sljedeći videozapis 5 minuta predstavlja Azure aplikacije servisa web-aplikacije.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="what-is-a-web-app-in-app-service"></a>Što je web-aplikacijama u servisu aplikacije?

*Web-aplikacije* je računalnim resursi koji Azure služi za hostiranje web-mjesta ili web-aplikaciju u aplikacije servisa.  

Resursi računalnim možda na zajedničkih ili namjenski virtualnim strojevima (VMs), ovisno o cijenama sloju koji odaberete. Pokreće se kodu aplikacije u upravljanih VM koji odvajanja od drugih korisnika.

Kod može biti bilo koji jezik ili framework koji podržavaju [Azure aplikacije servisa](../app-service/app-service-value-prop-what-is.md), kao što su ASP.NET, Node.js, jezika Java, PHP ili Python. Možete pokrenuti i skripte koje koriste [PowerShell i drugih skriptnog jezika](web-sites-create-web-jobs.md#acceptablefiles) u web-aplikaciji.

Primjeri scenarijima uobičajenu aplikacije koje možete koristiti web-aplikacije za potražite u članku [scenariji Web app](https://azure.microsoft.com/documentation/scenarios/web-app/) i **scenariji i preporuke** dio [aplikacije servisa za Azure, usporedbe virtualnim strojevima, tkanina servisa i servise u Oblaku](choose-web-site-cloud-service-vm.md#scenarios).

## <a name="why-use-web-apps"></a>Zašto koristiti web-aplikacije?

Evo nekih ključne značajke aplikacije servisa koji se odnose na web-aplikacije:

- **Više jezika i okviri** – aplikacije servisa za ima jednostavno prva liga podršku za ASP.NET Node.js, jezika Java, i te Python. Možete pokrenuti i [PowerShell i druge skripte ili izvršne datoteke](../app-service-web/web-sites-create-web-jobs.md) na aplikaciju servisa VMs.

- **Optimizacija DevOps** – postavljanje [Neprekinuti integracije i implementacija](../app-service-web/app-service-continuous-deployment.md) za Visual Studio Team Services, GitHub ili BitBucket. Promicanje ažuriranja putem [test i pripremna okruženja](../app-service-web/web-sites-staged-publishing.md). Izvođenje [A / B testiranje](../app-service-web/app-service-web-test-in-production-get-start.md). Upravljanje aplikacijama u aplikacije servisa pomoću [Azure PowerShell](../powershell-install-configure.md) ili [više platformi sučelje naredbenog retka (EŽA)](../xplat-cli-install.md).

- **Globalnih Skaliranje s visoke dostupnosti** - skaliranje [prema gore](../app-service-web/web-sites-scale.md) ili prema [van](../monitoring-and-diagnostics/insights-how-to-scale.md) automatski ili ručno. Hostira aplikacija bilo gdje u infrastrukture globalni podatkovnog centra tvrtke Microsoft i aplikacije servisa [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) promises visoke dostupnosti.

- **Veze na platformama SaaS i lokalnih podataka** – odaberite s više od 50 [Poveznici](../connectors/apis-list.md) za sustave enterprise (kao što su SAP, Siebel i Oracle), SaaS (kao što su Salesforce i Office 365) i internet services (kao što su Facebook i Twitter). Pristup lokalnih podataka pomoću [Hibridnog veze](../biztalk-services/integration-hybrid-connection-overview.md) i [Azure virtualne mreže](../app-service-web/web-sites-integrate-with-vnet.md).

- **Sigurnost i usklađenost** – aplikacije servisa za je [ISO, PnP, i PCI usklađen](https://www.microsoft.com/TrustCenter/).

- **Predlošci aplikacija** – odaberite s popisa proširenom predlošci aplikacije [Servisa Azure Marketplace](https://azure.microsoft.com/marketplace/) omogućuju korištenje čarobnjaka za instaliranje popularne Otvori izvor softvera kao što su WordPress, Joomla i Drupal.

- **Integracija s programom visual Studio** - namjenski Alati u Visual Studio pojednostavniti rad stvaranja, uvođenje i ispravljanje pogrešaka.

Uz to, web-aplikacijama možete iskoristiti značajke koje nudi [API aplikacije](../app-service-api/app-service-api-apps-why-best-platform.md) (kao što su CORS podržava) i [Mobilne aplikacije](../app-service-mobile/app-service-mobile-value-prop.md) (kao što je slanje obavijesti). Dodatne informacije o vrstama aplikaciju u aplikacije servisa za potražite u članku [Pregled aplikacije servisa za Azure](../app-service/app-service-value-prop-what-is.md).

Osim web-aplikacijama u aplikacije servisa Azure nudi druge servise koje je moguće koristiti za hostiranje web-mjesta i web-aplikacije. Većina scenarijima za web-aplikacije je najbolji odabir.  Za microservice arhitektura, razmislite o [Tkanina servisa](https://azure.microsoft.com/documentation/services/service-fabric), a ako trebate više kontrole nad VMs koja se izvršava kod na, razmislite o [virtualnim računalima sustava Azure](https://azure.microsoft.com/documentation/services/virtual-machines/). Dodatne informacije o tome da biste odabrali jednoliko tih servisa Azure potražite [aplikacije servisa za Azure, virtualnim strojevima, tkanina servisa i Usporedba servise u Oblaku](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Početak rada

Za početak uvođenjem ogledni kod web-aplikaciju u aplikacije servisa za praćenje vodič [uvođenja prvi web-aplikaciju programa za Azure 5 minuta](app-service-web-get-started.md) . Potreban vam je besplatan račun za Azure.

Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.
