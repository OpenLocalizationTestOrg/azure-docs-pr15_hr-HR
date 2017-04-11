<properties
    pageTitle="Azure stogu web-aplikacije pregled | Microsoft Azure"
    description="Pregled Web Apps u stogu Azure"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="azure-stack-web-apps-overview"></a>Azure stogu Web Apps pregled
    
> [AZURE.NOTE] Sljedeće informacije odnosi se samo na Azure stogu TP1 implementacije.

Azure stogu web-aplikacije je prvi element nuditi aplikacije servisa za Azure ne unese snop Azure. Instalacijski program Azure stogu web-aplikacije će stvoriti instancu svake od pet potreban uloga vrsta i datotečnom poslužitelju također će se stvoriti. Iako možete dodati više instanci za svaku vrstu uloge, imajte na umu da nema dostupnog prostora za VMs u Tehnički pretpregled 1. Trenutni mogućnosti Azure stogu web-aplikacijama su prvenstveno foundation mogućnosti koje su vam potrebne za upravljanje sustava i glavno računalo web-aplikacije.

![Azure stogu aplikacije servisa za Web Apps u na Azure posložiti Portal][1]

## <a name="limitations-of-the-technical-preview"></a>Ograničenja Tehnički pretpregled

Ne postoji podrška za pretpregled izdanja aplikacije servisa za Azure stogu. Nemojte smještati radnih opterećenja proizvodnje na ovom izdanju pretpregled. Postoji bez nadogradnje između aplikacije servisa za Azure stogu pretpregled izdanja. Osnovnih svrha ove pretpregled izdanja su da biste prikazali što smo vam i da biste dobili povratne informacije. 

## <a name="what-is-an-app-service-plan"></a>Što je Plan za aplikaciju servisa?

Davatelja resursa Azure stogu web-aplikacije koristi istu šifru koja koristi značajka Azure web-aplikacije u aplikaciju servisa Azure. Zbog toga neke uobičajene koncepte su s opisom. U web-aplikacijama cijene spremnik za web-aplikacije naziva aplikacije servisa za planiranje. Predstavlja skup namjenski virtualnim strojevima za držite aplikacija. Dani pretplate, imate više tarifa aplikacije servisa. To vrijedi i u web-aplikacijama za Azure stogu. 

Azure, razmjenjuju zaposlenici zaduženi za zajedničkih i namjenski. Zajedničkih radnih podržava high-density složene web app hostiranje i postoji samo jedan skup zaposlenici zaduženi za zajedničke. Namjenski poslužitelji su samo koristi jednog klijenta i dolaze u tri veličine: mala, Srednje i velike. Tarifa za korisnike lokalnog opisane ne može se uvijek pomoću tih uvjeta. U Azure stogu web-aplikacijama administratori davatelja resursa možete definirati razine tempiranja ih želite učiniti dostupnima tako da imaju više skupova zaposlenici zaduženi za zajedničke ili različite skupove namjenski zaposlenici zaduženi za koje vam na temelju njihove jedinstveni web-mjesta potrebama. Pomoću tih definicije sloju tempiranja, oni možete zatim definirati vlastite cijene SKU-ove.

## <a name="portal-features"></a>Značajke portala

Kao što je vrijedi i s pozadinska, Azure stogu web-aplikacije koristi iste korisničkog Sučelja koja koristi Azure web-aplikacije. Neke značajke onemogućuju a nisu još funkcionirati u stogu Azure. To je zato očekivanja specifične za Azure ili servise koje te značajke potreban je još nisu dostupne u stogu Azure. 

## <a name="next-steps"></a>Daljnji koraci

- [Prije nego što počnete s Azure stogu web-aplikacije](azure-stack-webapps-before-you-get-started.md)
- [Instalacija aplikacije resursa davatelja Web](azure-stack-webapps-deploy.md)

Možete isprobajte i druge [platforme kao servisi za servis (PaaS)](azure-stack-tools-paas-services.md), kao što je [davatelj usluga za SQL Server resursa](azure-stack-sql-rp-deploy-short.md) i [davatelja MySQL resursa](azure-stack-mysql-rp-deploy-short.md).

<!--Image references-->
[1]: ./media/azure-stack-webapps-overview/AppService_Portal.png
