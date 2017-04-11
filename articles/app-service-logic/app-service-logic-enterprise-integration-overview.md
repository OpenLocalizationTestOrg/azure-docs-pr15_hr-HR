<properties 
    pageTitle="Pregled integracije Enterprise | Aplikacije servisa za Microsoft Azure | Microsoft Azure" 
    description="Korištenje značajki integracije Enterprise da biste omogućili postupak i integracija scenariji za tvrtke pomoću aplikacije logike" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-the-enterprise-integration-pack"></a>Pregled integracije paket Enterprise

## <a name="what-is-the-enterprise-integration-pack"></a>Što je integracija paket Enterprise?
Integracija paket Enterprise je rješenje oblaku tvrtke Microsoft za jednostavno Omogućivanje komunikacije tvrtke za tvrtke (B2B). Paket koristi standardnih industrijskih protokola uključujući [AS2](./app-service-logic-enterprise-integration-as2.md), [X12](./app-service-logic-enterprise-integration-x12.md)i [EDIFACT](./app-service-logic-enterprise-integration-edifact.md) radi razmjene poruka između poslovnih partnera. Poruke mogu po želji zaštiti pomoću šifriranja i digitalne potpise. 

Paket omogućuje tvrtkama ili ustanovama koje koriste različite protokole i oblici za razmjenu poruka elektronički po Pretvorba različite oblike u oblik koji protumačiti i akcijama sustavi obje tvrtke ili ustanove. 

Ako ste upoznati s BizTalk Server ili Microsoft Azure BizTalk Services, pronaći ćete ga jednostavno je za korištenje značajki integracije Enterprise jer Većina konceptima slični su. Jedna od glavnih razlika je integracija Enterprise koristi Integracija račune da biste pojednostavnili prostora za pohranu i upravljanje artefakte koji se koriste u B2B komunikacije. 

Architecturally, Integracija paket Enterprise temelji se na **račune za integraciju** pohraniti sve artefakte koje je moguće koristiti za dizajniranje, uvođenje i održavanje B2B aplikacija. Račun za integraciju je spremniku oblaku gdje spremati artefakte kao što su sheme, partnerima, certifikata, karte i ugovore. Te elemente pa se poslužite u aplikacijama logike da biste sastavili B2B tijekova rada. U artefakte mogli koristiti u aplikaciji logike, morate povezati s računom za integraciju u aplikaciju za logiku. Nakon što ih povežete logike aplikacije će imati pristup artefakte Integracija računa.  

## <a name="why-should-you-use-enterprise-integration"></a>Zašto morate koristiti enterprise Integracija?
- Uz integraciju enterprise vam se za spremanje svih artefakte na jednom mjestu, što je račun za integraciju. 
- Možete koristiti modul logike aplikacije i njegov poveznici za sastavljanje B2B tijekovi rada i integracija s aplikacijama SaaS 3 proizvođača, lokalni aplikacije kao i prilagođene aplikacije
- Možete koristiti i Azure funkcije

## <a name="how-to-get-started-with-enterprise-integration"></a>Upute za početak rada s enterprise Integracija?
Možete izraditi i upravljanje aplikacijama B2B pomoću Integracija paket Enterprise putem aplikacije dizajner logike za **Azure portal**.  

Da biste upravljali aplikacijama logike možete koristiti i [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "logike aplikacije komponente PowerShell teme") . 

Slijedi pregled korake koje treba poduzeti da biste mogli stvarati aplikacije na portalu za Azure: ![pregled slika](./media/app-service-logic-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Što su neke uobičajeni scenariji?

Integracija Enterprise podržava te standardima:   

- Uređivanje - razmjenu elektroničke podataka  
- EAI - integraciju aplikacija Enterprise  

## <a name="heres-what-you-need-to-get-started"></a>Evo što vam je potrebno za početak rada
- Azure pretplate s računom Integracija
- Visual Studio 2015 da biste stvorili karte i sheme
- [Integracija programa Microsoft Azure logike Enterprise aplikacije Tools za Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it"></a>Isprobajte
[Isprobajte odmah](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) za implementaciju potpuno radu uzorka AS2 slanje i primanje logike aplikacije koja koristi značajke B2B logike aplikacija.

## <a name="learn-more-about"></a>Dodatne informacije:
- [Ugovori] (./app-service-logic-enterprise-integration-agreements.md "Informirajte se o ugovore Integracija enterprise")
- [Scenariji za tvrtke za tvrtke (B2B)] (./app-service-logic-enterprise-integration-b2b.md "Saznajte kako stvoriti logike aplikacije sa značajkama B2B")  
- [Certifikati] (./app-service-logic-enterprise-integration-certificates.md "Informirajte se o enterprise Integracija certifikata")
- [Nehijerarhijskom datotekom kodiranje/dekodiranje] (./app-service-logic-enterprise-integration-flatfile.md "Saznajte kako kodiranje i dekodiranje sadržaja s nehijerarhijskom datotekom")  
- [Integracija računi] (./app-service-logic-enterprise-integration-accounts.md "Dodatne informacije o računima Integracija")
- [Karte] (./app-service-logic-enterprise-integration-maps.md "Informirajte se o enterprise Integracija karte")
- [Partneri] (./app-service-logic-enterprise-integration-partners.md "Informirajte se o partnera za integraciju enterprise")
- [Shema] (./app-service-logic-enterprise-integration-schemas.md "Informirajte se o sheme Integracija enterprise")
- [Provjera valjanosti XML poruke] (./app-service-logic-enterprise-integration-xml.md "Saznajte kako provjeriti valjanost XML poruka s aplikacijama logike")
- [Transformacija XML-a] (./app-service-logic-enterprise-integration-transform.md "Informirajte se o enterprise Integracija karte")
- [Enterprise Integracija poveznika] (../connectors/apis-list.md "Informirajte se o enterprise Integracija s programom paketa poveznika")



