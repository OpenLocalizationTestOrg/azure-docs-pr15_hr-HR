<properties
    pageTitle="Popis dostupnih poveznika i aplikacije API | Aplikacije servisa za Microsoft Azure"
    description="Saznajte više o poveznika i API aplikacije u aplikacije servisa za Azure"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>Popis poveznika i aplikacije API-JA za korištenje u aplikacijama za logike
>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2014. – 12-01 – pregled shema verziju. Logika aplikacije opće dostupnih (GA) verzija potražite u članku [Novi popis poveznike](../connectors/apis-list.md).

Informirajte se o dostupna poveznika i aplikacije API stvorio Microsoft za korištenje unutar logike aplikacije.

Informacije o cijenama i popis što je isporučuje se uz svaki servis sloju, potražite u članku [Cijene za Azure aplikacije servisa](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Da biste započeli s aplikacijama logiku prije registracije za račun za Azure, idite na [Pokušajte logike aplikacije](https://tryappservice.azure.com/?appservice=logic). Odmah možete stvoriti aplikaciju za logiku short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="core-connectors"></a>Temeljni poveznika
U sljedećoj su tablici navedeni sve dostupne poveznika i API aplikacijama koje su dostupni kao Jezgra poveznika stvorio Microsoft:

Ime | Opis
--- | ---
[Bing Translator](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Pomoću servisa Bing Prevedite tekst na drugom jeziku.
[HTTP](app-service-logic-connector-http.md) | HTTP ga Slušatelj otvara krajnje koji funkcionira kao HTTP poslužitelj i očekuje podatke za HTTP ili HTTPS zahtjevi za razgovore. HTTP akciju ne zahtijeva aplikaciju API podržana je i nativno unutar logike aplikacije.
[Prazan hod](app-service-logic-connector-slack.md) | Povezivanje s Prazan hod i objavu poruka u zalihe kanala.


## <a name="enterprise-integration-connectors"></a>Enterprise Integracija poveznika
Sljedeća tablica prikazuje sve dostupne poveznika i aplikacije API stvorio Microsoft dostupni kao poveznici za integraciju Enterprise:

Ime  | Opis
------------- | -------------
[BizTalk pravila](app-service-logic-use-biztalk-rules.md) | Pomoću pravila BizTalk možete definirati i kontrolirati poslovne logike unutar tvrtke ili ustanove. Pravila tvrtke može se ažurirati bez recompiling ili bez redeploying povezane aplikacije.
[Program za izdvajanje BizTalk XPath](app-service-logic-xpath-extract.md) | Traži riječ i izvlači podatke iz XML sadržaja na temelju XPath koje odaberete.
[Poveznik za DB2](app-service-logic-connector-db2.md) | Povezuje za IBM DB2 baze podataka na lokalno i na Azure virtualnog računala sustavom operacijskog sustava Windows. Možete mapirati Web API i OData API operacija Informix Structured Query Language naredbe. <br/><br/>Nema okidača. Akcije uključuju odabir tablice, umetanje, ažuriranje, brisanje i prilagođene naredbe<br/><br/>Ovaj poveznik obuhvaća i Microsoft Client za DRDA da biste se povezali s poslužiteljem Informix putem mreže TCP/IP.
[Datoteka](app-service-logic-connector-file.md) | Koristite ovaj poveznik, možete se povezati lokalnog datotečnom sustavu ili mreži i dovršavanje datotečne zadataka, uključujući prijenos, brisanje, unos datoteka i drugo.
[Informix](app-service-logic-connector-informix.md) | Povezuje se s bazom podataka IBM Informix, lokalni i Azure virtualnog računala operacijskog sustava Windows. Možete mapirati Web API i OData API operacija Informix Structured Query Language naredbe.<br/><br/>Nema okidača. Akcije uključuju odabir tablice, umetanje, ažuriranje, brisanje i prilagođene naredbe.<br/><br/>Prilikom korištenja lokalnog VPN-a ili Azure ExpressRoute može se koristiti. Ovaj poveznik obuhvaća i Microsoft Client za DRDA da biste se povezali s poslužiteljem Informix putem mreže TCP/IP.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | Povezuje se s lokalnog sustava SQL Server ili baze podataka Azure SQL. Možete stvaranje, ažuriranje, dobiti i brisanje stavki na tablici baze podataka SQL.
MQ | Povezuje IBM WebSphere MQ poslužitelj verzije 8 lokalnog i Azure virtualnog računala operacijskog sustava Windows. Prilikom korištenja lokalnog VPN-a ili Azure ExpressRoute može se koristiti. Poveznik obuhvaća i Microsoft Client za MQ.<br/><br/>Nema okidača. Nema akcija.<br/><br/>**Napomena** Trenutno nije moguće koristiti s aplikacijama logike.

## <a name="connectors-as-triggers"></a>Poveznika kao okidača
Nekoliko poveznika okidača omogućavaju logike aplikacije. Ove okidača su dvije vrste:

1. Ankete okidača: Ove okidača ankete usluge na navedeni učestalost da biste potražili nove podatke. Kada je dostupno nove podatke, novu instancu logike aplikacije se pokreće s podacima kao ulaz. Da biste spriječili iste podatke u tijeku potrošena više puta okidača možda čišćenja podataka koji čitati i proslijeđena aplikaciju logike. Primjeri takvih poveznika su datoteke te SQL Azure prostora za pohranu.
2. Automatske okidača: Ove okidača poslušali za podatke na krajnje točke ili događaj da se pokreće. Nakon toga pokretanje nove instance programa logike aplikacije. Primjeri takvih poveznika su HTTP ga Slušatelj i Twitter.

## <a name="connectors-as-actions"></a>Poveznika kao akcije
Poveznika može se koristiti kao akcija unutar logike aplikacije. Akcije korisne su za traženje podataka logike aplikacije koje možete koristi se u izvršavanja. Na primjer, možda ćete morati traženje podataka iz baze podataka SQL dodatne informacije o klijentu tijekom obrade reda. Ili, možda ćete morati pisanje, ažuriranje i brisanje podataka u odredište. To možete učiniti pomoću akcije koje ste dobili od poveznika. Akcije mapirajte operacije u aplikacijama za API-JA (kako je definirano njihovim metapodacima Swagger).

## <a name="create-your-own-connectors-and-api-apps"></a>Stvaranje vlastitih poveznika i aplikacije API-JA
[Referenca za aplikacije API-JA i poveznika](http://aka.ms/appservicesconnectorreference)  
[Azure okidača API aplikacije servisa za aplikaciju](../app-service-api/app-service-api-dotnet-triggers.md)  
[Referenca za aplikaciju logike](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>Dodatne informacije o poveznika i aplikacije API-JA
[Što su poveznika i aplikacije BizTalk API-JA](app-service-logic-what-are-biztalk-api-apps.md)  
[Korištenje upravitelja veze hibridnog u aplikacije servisa za Azure](app-service-logic-hybrid-connection-manager.md)  
[Upravljanje i nadzirati ugrađene aplikacije API-JA i poveznika](app-service-logic-monitor-your-connectors.md)
