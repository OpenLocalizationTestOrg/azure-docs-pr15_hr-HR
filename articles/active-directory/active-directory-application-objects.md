<properties
pageTitle="Aplikacija za Azure Active Directory i usluge objekata | Microsoft Azure"
description="Rasprave odnosa između aplikacija i servisa glavni objekata u Azure Active Directory"
documentationCenter="dev-center-name"
authors="bryanla"
manager="mbaldwin"
services="active-directory"
editor=""/>

<tags
ms.service="active-directory"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="identity"
ms.date="08/10/2016"
ms.author="bryanla;mbaldwin"/>

# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Aplikacija i servisa glavni objekata u Azure Active Directory
Kada čitate o programa Azure Active Directory (AD) "aplikacija", nije uvijek Očisti točno što je koji se poziva autor. Cilj ovog članka je da biste ga jasniji prikaz, definiranjem konceptualni i konkretni aspekte Azure AD aplikacije Integracija s primjera Registracija i dozvole za [više klijentske aplikacije](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Pregled
Zatvaranje Azure AD je šira od samo piece softvera. Je konceptualni termina koje upućuju ne samo na softver aplikacije, ali i njegov Registracija (ili: Konfiguriranje identiteta) s Azure AD koji omogućuje sudjelovanje u provjere autentičnosti i autorizacije "razgovorima" prilikom izvođenja. Po definiciji, aplikaciju funkcionirati u ulogu [klijenta](active-directory-dev-glossary.md#client-application) (troše resursa), uloge [poslužitelja resursa](active-directory-dev-glossary.md#resource-server) (će se API-ji klijentima) ili čak i oba. U [tijek OAuth 2.0 autorizacije Grant](active-directory-dev-glossary.md#authorization-grant)s ciljem dopuštanja klijentskog/resursa za pristup/zaštitu resursa podataka odnosno definira protokol za razgovor. Sada prođimo razinu obuhvaćaju i potražite u članku kako interno u modelu Azure AD predstavlja aplikaciju. 

## <a name="application-registration"></a>Registracija aplikacije
Kada registrirate programa [Azure klasični portal][AZURE-Classic-Portal], dva objekta stvaraju se u klijentu za Azure AD: objekt aplikacija i objektima upravitelja za servis.

#### <a name="application-object"></a>Objekt aplikacije
Aplikaciju Azure AD je *definiran* njegov jedan, a samo objekt aplikacije, koji se nalazi u klijentu za Azure AD gdje je registrirana aplikaciju, se nazivaju "kod kuće" klijentske aplikacije. Objekt aplikacije pruža informacije vezane uz identiteta za aplikaciju, a predložak iz kojeg njegov odgovarajući objekata glavni servis su *izvedena* za vrijeme izvođenja. 

Možete shvatiti aplikacije kao *globalni* predstavljanje aplikacije (za korištenje preko svih drugih korisnika) i servis glavnicu kao *lokalne* predstavljanje (za korištenje u određenim klijentu). Azure AD grafikonu [aplikacije entitet] [ AAD-Graph-App-Entity] definira shemu za objekt aplikacija. Objekt aplikacija stoga ima odnos 1:1 s računala softver i na 1:*n* odnos s njegova odgovarajuće *n* servisa glavni objekata.

#### <a name="service-principal-object"></a>Glavni objekt servisa
Glavni objekt servisa definira pravila i dozvole za aplikaciju, pruža temelj za sigurnosni upravitelj za predstavljanje aplikacije prilikom pristupanja resursa pri izvođenju. Azure AD grafikonu [entitet ServicePrincipal] [ AAD-Graph-Sp-Entity] definira shemu glavni objekta servisa. 

U svakom klijentu za koju instance komponente korištenje aplikacije mora biti predstavljeni, omogućivanje sigurnog pristupa resursima vlasništvu korisnički računi iz taj klijent, potreban je objekt za glavni servisa. Jedne klijentske aplikacije neće imati samo jedan glavnicu servisa (u početnom klijent). Više klijentu [web-aplikacije](active-directory-dev-glossary.md#web-client) dodijelit će se glavni servisa u svakom klijentu gdje administrator ili korisnika na klijentu date pristanak, dopustite da biste pristupili njihove resursima. Sljedeći pristanak, će konzultirali glavni objekt servisa zahtjeva za buduće autorizacije. 

> [AZURE.NOTE] Sve promjene koje napravite u objekt aplikacije, odražavaju i njegov objekt servisa glavni aplikacije kućni klijentu samo (klijentsko gdje je registrirana). Za aplikacije više klijentu promjene objekt aplikacije ne odražavaju klijenata sve korisničke servisa glavni objekata, dok klijent potrošača uklanja pristup i ponovno dopušta pristup.

## <a name="example"></a>Primjer
Na sljedećem su dijagramu ilustrira odnos između objekt aplikacije za aplikacije i odgovarajuća glavni objekte, u kontekstu aplikacije više klijentu uzorka naziva **HR aplikacije**za servis. Postoje tri klijenata za Azure AD u ovom scenariju: 

- **Adatum** - klijentu koristi tvrtku koja je razvio **HR aplikacije**
- **Contoso** - klijentu koristi organizacija Contoso, potrošača **HR aplikacije**
- **Fabrikam** - klijentu koristi organizacija Fabrikam koji i troši **HR aplikacije**

![Odnos između objekt aplikacija i objektima upravitelja za servis](./media/active-directory-application-objects/application-objects-relationship.png)

U dijagramu prethodni korak 1 je postupak stvaranja aplikacija i servisa glavni objekata u klijentu za kućne aplikacije.

U koraku 2 kada provoditi Contoso i Fabrikam administratori pristanak, objektima upravitelja za servis je stvorena u klijentu za Azure AD svoje tvrtke i dodjeljuju dozvole koje vam dodijelio administrator. Imajte na umu da aplikaciju HR može biti konfiguriran/dizajnirali dopustili pristanak za korištenje za pojedinačne korisnike.

U koraku 3 klijenata potrošača HR aplikacije (Contoso i Fabrikam) svaki imati vlastite glavni objekt servisa. Svaki predstavlja njihova upotreba instance programa tijekom izvođenja uređena dozvole pristanak vezanom administrator.

## <a name="next-steps"></a>Daljnji koraci
Objekt aplikacije aplikacije moguće pristupiti putem API Azure AD grafikon kao predstavljen brojem OData [entitet aplikacije][AAD-Graph-App-Entity]

Glavni objekt servisa za aplikacije sustava moguće pristupiti putem API Azure AD grafikon kao što je prikazano po OData [entitet ServicePrincipal][AAD-Graph-Sp-Entity]



<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Classic-Portal]: https://manage.windowsazure.com