<properties
    pageTitle="Provjere autentičnosti i autorizacije API aplikacije u aplikacije servisa za Azure | Microsoft Azure"
    description="Saznajte više o provjere autentičnosti i autorizacije servise koje nudi Azure aplikacije servisa za API aplikacije."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="rachelap"/>

# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Provjere autentičnosti i autorizacije API aplikacije u aplikacije servisa za Azure

## <a name="overview"></a>Pregled 

> [AZURE.NOTE] Će se premjestiti u ovoj se temi da biste na Konsolidirani [Provjera autentičnosti za aplikaciju servisa / autorizacije](../app-service/app-service-authentication-overview.md) tema, kojemu su opisane Web, Mobile i aplikacije API-JA.

Aplikacije servisa za Azure nudi ugrađene provjere autentičnosti i autorizacije servise koje implementirati [OAuth 2.0](#oauth) i [Povezivanje OpenID](#oauth). U ovom se članku opisuju mogućnosti koje su dostupne za aplikacije API-JA u servisu Azure aplikacije i servise.

Na sljedećem su dijagramu ilustrira neke ključne značajke provjere autentičnosti za aplikaciju servisa:

* Ga preprocesses API zahtjevi za razgovore, što znači da radi s bilo kojim jezik ili framework podržava aplikacije servisa.
* Pruža nekoliko mogućnosti za rad koliko provjere autentičnosti koje želite u vlastiti kod.
* Funkcionira za krajnjeg korisnika i provjera autentičnosti pomoću računa za servis. 
* Podržava pet davatelji identiteta: Azure Active Directory, Facebook, Google, Twitter i Microsoftov Account.
* To funkcionira na isti API aplikacije, web-aplikacije i mobilne aplikacije.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Jezik agnostic

Obrada aplikacije servisa za provjeru autentičnosti događa prije zahtjeve dosegne API aplikacije, što znači da se značajke provjere autentičnosti funkcionirati za API aplikacije pisane bilo koji jezik ili framework.  Vaš API mogu se temeljiti na ASP.NET, Java, Node.js ili bilo koju framework koji podržava aplikacije servisa.

Aplikacije servisa za prosljeđuje token JSON web (JWT) u zaglavlju autorizacije HTTP zahtjev, a kod napisan u bilo koji jezik ili framework možete dobiti potrebne podatke iz token. Osim toga, aplikacije servisa omogućuje vam jednostavnije pristup najčešće korištenih zahtjevima postavljanjem neke posebne zaglavlja, kao što je sljedeće:

* X-MS-KLIJENT-GLAVNICU-NAME
* X-MS--GLAVNICU-ID KLIJENTA
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON
 
U .NET API-JA možete koristiti u `Authorize` atribut i za preciznije autorizacije jednostavno možete napisati kod utemeljene na zahtjevima jer o unose umjesto vas u .NET klase.

## <a name="multiple-protection-options"></a>Više mogućnosti zaštite

Aplikacije servisa možete spriječiti anonimni HTTP zahtjeva Dostizanje API aplikacije, možete prenesite na sve zahtjeve za i provjeriti tokena zahtjevi za koje ih uključiti ili možete pustiti kroz sve zahtjeve za bez poduzimanja akcija na njima:

1. Dopusti samo čija je autentičnost provjerena zahtjeve dosegne aplikacije API-JA.

    Ako je zahtjev za anonimni primili iz preglednika, bit ćete aplikacije servisa za preusmjereni na stranicu za prijavu za davatelja usluge provjere autentičnosti (Azure AD, Google, Twitter, itd.) koje ste odabrali. 

    Uz tu se mogućnost ne morate pisati kod provjere autentičnosti u aplikaciji, a autorizacije kod je pojednostavnjenom jer najvažnije zahtjevima isporučuju HTTP zaglavlja.

2. Dopusti sve zahtjeve za dosegne API aplikacije, ali čija je autentičnost provjerena zahtjeve za provjeru valjanosti i prenesite duž podatke za provjeru autentičnosti u zaglavljima HTTP.

    Tu mogućnost daje veću fleksibilnost obrade zahtjeva za anonimni, ali ćete se morati pisanje koda ako želite anonimnim korisnicima onemogućiti korištenje vaše API-JA. Budući da se najčešće korištene zahtjevima prosljeđuju se u zaglavljima HTTP zahtjeva, autorizacije kod je relativno jednostavan.
    
3. Dopusti sve zahtjeve za do vaše API-JA, bez akcijama podatke za provjeru autentičnosti u zahtjeve.

    Ta mogućnost ostavlja zadatke provjere autentičnosti i autorizacije potpuno najviše kodu aplikacije.

[Portal za Azure](https://portal.azure.com/)odaberite željenu mogućnost na na **provjere autentičnosti / autorizacije** plohu.

![](./media/app-service-api-authentication/authblade.png)

Mogućnosti 1 i 2, uključite **Provjera autentičnosti za aplikaciju servisa**, a u **poduzeti kada zahtjeva nije autentičnost** padajućeg popisa odaberite **prijavite** ili **zahtjev za Dopusti (ne poduzmete)**.  Ako se odlučite **prijaviti**, morate odabrati davatelja provjere autentičnosti i konfiguriranje taj davatelj.

![](./media/app-service-api-authentication/actiontotake.png)

Detaljne informacije o konfiguriranju provjere autentičnosti potražite u članku [upute za konfiguriranje aplikacije servisnoj aplikaciji za korištenje prijava Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). U članku odnosi se na API aplikacija, kao i mobilne aplikacije pa ga veze na druge članke ostali davatelji provjere.
 
## <a id="internal"></a>Provjera autentičnosti pomoću računa za servis

Aplikacije servisa za provjeru autentičnosti radi interna scenariji kao što su zovete iz jedne aplikacije API-JA u nekoj drugoj aplikaciji API-JA. U ovom scenariju dobiti token pomoću vjerodajnica za račun servisa umjesto vjerodajnice za krajnjeg korisnika. Račun servisa poznat i kao *glavni servisa* u Azure Active Directory, a provjere autentičnosti putem takvih računa poznat i kao scenarij servis za servis. 

Za scenarije servisa za servis, zaštita pod nazivom API aplikacije pomoću servisa Azure Active Directory i ponuditi oznaku AAD servisa glavni ovlaštenja prilikom pozivanja aplikaciju API-JA. Dobit token unosom klijent klijent tajnu iz aplikacije AAD i ID-a. Ne samo za Azure Šifra posebne je obavezna, kao što su za vrijediti za rukovanje token Zumo mobilne usluge. Primjer scenarij korištenje ASP.NET API aplikacija je prekriven vodič [servisa glavni provjere autentičnosti za API aplikacije](app-service-api-dotnet-service-principal-auth.md).

Ako želite rukovati scenarij servis za servis bez korištenja aplikacije servisa za provjeru autentičnosti, možete koristiti klijentskih potvrda ili osnovnu provjeru autentičnosti. Informacije o klijentskih potvrda u Azure potražite u članku [Kako da biste konfigurirali TLS međusobna provjere autentičnosti za web-aplikacije](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Informacije o u ASP.NET osnovnu provjeru autentičnosti, potražite u članku [Filtri za provjeru autentičnosti u ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Servis provjera autentičnosti pomoću računa iz aplikacije logike aplikacije servisa za aplikaciju API je posebno slučaj u koji je objašnjeno u [korištenje prilagođenih API-JA hostirane na aplikacije servisa s aplikacijama logike](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="mobile-client-authentication"></a>Provjera autentičnosti mobilnog klijenta

Informacije o tome kako rukovati provjeru autentičnosti iz mobilnih klijenata potražite u [dokumentaciji na provjere autentičnosti za mobilne aplikacije](../app-service-mobile/app-service-mobile-ios-get-started-users.md). Provjera autentičnosti aplikacije servisa za funkcionira na isti način za mobilne aplikacije i aplikacije API-JA.
  
## <a name="more-information"></a>Dodatne informacije

Dodatne informacije o provjere autentičnosti i autorizacije u aplikacije servisa za Azure potražite u sljedećim resursima:

* [Provjera autentičnosti aplikacije servisa za širi / autorizacije](/blog/announcing-app-service-authentication-authorization/)
* [Upute za konfiguriranje aplikacije servisnoj aplikaciji za korištenje prijava Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (Obuhvaća veze za ostali davatelji provjere pri vrhu stranice) 

Dodatne informacije o OAuth 2.0 OpenID povezivanje i JSON Web tokeni (JWT), potražite u sljedećim resursima.

* [Uvod u OAuth 2.0] (http://shop.oreilly.com/product/0636920021810.do "Uvod u OAuth 2.0") 
* [Uvod u OAuth2, OpenID povezivanje i JSON Web tokena (JWT) – PluralSight tečaja](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Stvaranje i zaštita RESTful API za druge klijente u ASP.NET - PluralSight tečaja](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Dodatne informacije o servisu Azure Active Directory potražite u sljedećim resursima.

* [Scenariji za Azure AD](http://aka.ms/aadscenarios)
* [Vodič za Azure AD razvojnim inženjerima'](http://aka.ms/aaddev)
* [Uzorci Azure AD](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku je objašnjeno provjere autentičnosti i autorizacije značajke servisa aplikacije koje možete koristiti za API aplikacije. Na sljedeći Praktični vodič u nizu dohvaćanje rada pokazuje kako implementirati [provjere autentičnosti korisnika u API servisa aplikacija](app-service-api-dotnet-user-principal-auth.md).
