<properties
   pageTitle="Upravljanje identitetom za složene aplikacije | Microsoft Azure"
   description="Najbolje prakse za provjeru autentičnosti, autorizacije i identitet upravljanje u složene aplikacije."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="identity-management-for-multitenant-applications-in-microsoft-azure"></a>Upravljanje identitetom za složene aplikacije Microsoft Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ovaj niz opisuje najbolje prakse za multitenancy, prilikom korištenja Azure AD za provjeru autentičnosti i identitet upravljanje.

Kada se stvara složene aplikacije, jednu od prvog izazove je Upravljanje identitetima korisnika jer je sada svaki korisnik pripada klijentom. Ako, na primjer:

- Korisnici prijavite se pomoću vjerodajnica za tvrtke ili ustanove.
- Korisnici moraju imati pristup svoje tvrtke ili ustanove podataka, ali ne podatke koji pripada s drugim korisnicima.
- Tvrtki ili ustanovi možete registrirati za aplikaciju, a zatim dodijelite uloge aplikacije članovima.

Azure Active Directory (Azure AD) sadrži neke odlične značajke koje podržavaju sve scenarija.

Da bi se podudarala ovaj niz članke, ne možemo i stvorili dovrši, [Završi do kraja implementaciju] [ tailspin] složene aplikacije. U člancima odražavaju ono što smo naučili u tijeku stvaranje aplikacija. Početak rada s aplikacijom, potražite u članku [pokretanje aplikacije ankete](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md).

Slijedi popis članaka u ovoj seriji:

- [Uvod u upravljanje identitetom za složene aplikacije](guidance-multitenant-identity-intro.md)
- [O aplikaciji Tailspin ankete](guidance-multitenant-identity-tailspin.md)
- [Provjera autentičnosti pomoću Azure AD i povezivanje OpenID](guidance-multitenant-identity-authenticate.md)
- [Rad s identitete utemeljene na zahtjev](guidance-multitenant-identity-claims.md)
- [Prijava i smjernice za uhodavanje](guidance-multitenant-identity-signup.md)
- [Uloge aplikacije](guidance-multitenant-identity-app-roles.md)
- [Autorizacija sustavom resursa ili na temelju uloga](guidance-multitenant-identity-authorize.md)
- [Zaštita pozadinskog web API-JA](guidance-multitenant-identity-web-api.md)
- [Predmemoriranje OAuth2 pristupna tokena](guidance-multitenant-identity-token-cache.md)
- [Združivanju s klijenta AD fs složene aplikacije u Azure](guidance-multitenant-identity-adfs.md)
- [Da biste dobili pristup tokena iz Azure AD pomoću pridruživanju klijenta](guidance-multitenant-identity-client-assertion.md)
- [Pomoću tipke sigurnog da biste zaštitili tajne aplikacije](guidance-multitenant-identity-keyvault.md)

[tailspin]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
