<properties
    pageTitle="Provjere autentičnosti i autorizacije u aplikacije servisa za Azure | Microsoft Azure"
    description="Konceptualni reference i pregled provjeru autentičnosti / autorizacije značajki za aplikacije servisa za Azure"
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-app-service"></a>Provjere autentičnosti i autorizacije u aplikacije servisa za Azure

## <a name="what-is-app-service-authentication--authorization"></a>Što je provjera autentičnosti za aplikaciju servisa / autorizacije?

Provjera autentičnosti za aplikaciju servisa / autorizacije je značajka koja omogućuje za svoju aplikaciju za prijavu korisnika tako da ne morate promijeniti kod na pozadinska aplikacija. Pruža jednostavan način da biste zaštitili svoju aplikaciju i rad s podacima po korisniku.

Aplikacije servisa za koristi pridruženim identiteta, u kojem se davatelja identiteta treće strane pohranjuje računi i potvrđuje korisnika. Aplikacija ovisi davatelja identiteta informacije tako da ne moraju pohranjuju same aplikacije. Aplikacije servisa za podržava pet davatelji identiteta iz okvir: Azure Active Directory, Facebook, Google, Microsoftov Account i Twitter. Pokrenite aplikaciju možete koristiti bilo koji broj od tih davatelja usluga identiteta da bi korisnici s mogućnostima kako se prijaviti. Da biste proširili ugrađenu podršku, možete integrirati drugog davatelja identiteta ili [prilagođenog identiteta rješenje][custom-auth].

Ako želite započeti odmah potražite u nekom od sljedećih vodiči za:

- [Dodavanje provjere autentičnosti za aplikaciju za iOS] [ iOS] (ili [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms]ili [Cordova])
- [Provjera autentičnosti korisnika API aplikacije u aplikacije servisa za Azure][apia-user]
- [Početak rada sa servisom Azure aplikacije – dio 2][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>Kako funkcionira provjera autentičnosti u servisu aplikacije

Da bi se autentičnost pomoću neke od davatelja identiteta, najprije morate konfigurirati davatelja identiteta informacije o aplikaciji. Davatelja identiteta pronaći ćete ID-ove i tajne koje nude usluge aplikacije. Time se dovršava odnos pouzdanosti tako da možete aplikacije servisa za provjeru valjanosti assertions korisnika, kao što su tokeni za provjeru autentičnosti davatelja identiteta.

Da biste se prijavili u korisnika pomoću neke od tih davatelja usluga, korisnik mora biti preusmjereni na krajnje se prijavi korisnika za tu davatelja usluga. Ako korisnici koriste web-preglednik, imate aplikacije servisa za automatski Izravni svim korisnicima Neprovjereni krajnju točku se prijavi korisnika. U suprotnom, morat ćete izravno klijente `{your App Service base URL}/.auth/login/<provider>`, pri čemu `<provider>` je jedan od sljedećih vrijednosti: aad, facebook, google, microsoft ili twitter. Scenariji Mobile i API objašnjeni su u odjeljcima u nastavku ovog članka.

Korisnici koji interakciju s aplikacijom putem web-pregledniku će imaju kolačić postaviti tako da ih može ostati čija je autentičnost provjerena kao što su pronađite aplikaciju. Za ostale vrste klijenta, kao što su mobile, JSON web token (JWT), koji moraju biti prikazana u `X-ZUMO-AUTH` zaglavlju će izdati klijent. Mobilne aplikacije klijenta SDK-ovi će riješiti umjesto vas. Osim toga, Azure Active Directory identiteta token ili token pristup mogu biti izravno obuhvaćene na `Authorization` zaglavlja kao [nošenja token](https://tools.ietf.org/html/rfc6750).

Aplikacije servisa za će provjeriti kolačića ni tokena koje izdaje svoju aplikaciju za provjeru autentičnosti korisnika. Da biste ograničili tko može pristupiti aplikaciji, u odjeljku [autorizacije](#authorization) u nastavku ovog članka.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobilni provjere autentičnosti kod davatelja usluga SDK

Nakon što je konfiguriran na pozadinski, možete izmijeniti mobilnih klijenata da se prijavi pomoću aplikacije servisa. Postoje dva načina prikaza ovdje:

- Korištenje programa SDK koje objavljuje određenom identitetu davatelja da biste ostvarili identiteta i ostvariti pristup aplikacije servisa.
- Koristite s jednim retkom kod tako da se klijent mobilne aplikacije SDK mogu se prijaviti u korisnika.

>[AZURE.TIP] Većina aplikacija trebali biste koristiti davatelja SDK da biste dobili dosljedan zadovoljstva korisnika za prijavu, da biste koristili podršku osvježavanja, a da biste dobili ostale pogodnosti koje određuje davatelj usluge.

Kada koristite davatelja SDK, korisnici mogli prijaviti u okruženje za koji se više čvrsto integrira s operacijskim sustavom aplikaciju na kojoj se izvršava. Time se davatelja token i neke korisničke informacije na klijentskom računalu, što olakšava mnogo zauzeti grafikonu API-ji i prilagodbu korisničkog sučelja. Povremeno na blogove i forume, vidjet ćete ovo se nazivaju "tijek klijent" ili "klijent usmjereni tijek" jer kod na klijentskom računalu se prijavi korisnika i kod klijenta ima pristup token za davatelja usluga.

Nakon dobiva se davatelja tokena, mora se šalje aplikacije servisa za provjeru valjanosti. Kada aplikacije servisa za Provjeri valjanost token, aplikacije servisa za stvara novi token aplikacije servisa za koje se vraćaju klijent. Mobilne aplikacije klijenta SDK je metoda Pomoćnik za upravljanje ovom exchange i automatski token priložiti sve zahtjeve za pozadinska aplikacija. Razvojni inženjeri možete i zadržati referenca token davatelja ako ih tako da odaberete.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobilni provjera autentičnosti bez davatelja SDK

Ako ne želite postaviti davatelja SDK, možete omogućiti značajku mobilne aplikacije Azure aplikacije servisa za prijavu za vas. Klijent mobilne aplikacije SDK će otvoriti web-prikaz s davateljem vašem odabiru i prijavite se u korisnika. Povremeno na blogove i forume, vidjet ćete ovo se nazivaju "poslužitelj tijek" ili "poslužitelj usmjereni tijek" jer je poslužitelj upravlja postupka prijave korisnika i klijent SDK nikad ne prima token za davatelja usluga.

Kod da biste pokrenuli ovaj tijek uvrštava u ovom praktičnom vodiču provjere autentičnosti za svaki platformu. Na kraju tijeka klijent SDK ima token za aplikacije servisa, a token automatski priložiti sve zahtjeve za pozadinska aplikacija.

### <a name="service-to-service-authentication"></a>Provjera autentičnosti servisa servisa

Iako korisnicima možete dati pristup u aplikaciji, možete vjerovati i neku drugu aplikaciju da biste nazvali vlastite API-JA. Na primjer, može imati jednu web-aplikacije poziva API u drugom web-aplikaciji. U ovom scenariju, koristite vjerodajnice za račun servisa umjesto korisničke vjerodajnice da biste dobili token. Račun servisa poznat i kao *glavni servisa* u žargonu Azure Active Directory, a provjere autentičnosti koja koristi takve račun poznat i kao scenarij servis za servis.

>[AZURE.IMPORTANT] Jer se pokreću mobilnih aplikacija na uređajima klijenta, mobilne aplikacije učinite _ne_ count kao pouzdane aplikacije i trebali biste koristiti tijek za glavni servisa. Umjesto toga, trebali biste koristiti tijek korisnika koji je ranije detaljne.

Za scenarije servisa za servis aplikacije servisa možete zaštititi aplikacije pomoću servisa Azure Active Directory. Aplikacija samo treba navesti oznaku Azure Active Directory servisa glavni ovlaštenja koji je dobiven unosom klijent klijent tajnu iz servisa Azure Active Directory i ID-a. Primjer scenarij koji koristi ASP.NET API aplikacija je objašnjeno po praktičnom vodiču, [servis glavni provjere autentičnosti za aplikacije API][apia-service].

Ako želite koristiti aplikacije servisa za provjeru autentičnosti za rukovanje scenarij servis za servis, možete koristiti klijentskih potvrda ili osnovnu provjeru autentičnosti. Informacije o klijentskih potvrda u Azure potražite u članku [Kako da biste konfigurirali TLS međusobna provjere autentičnosti za web-aplikacije](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Informacije o u ASP.NET osnovnu provjeru autentičnosti, potražite u članku [Filtri za provjeru autentičnosti u ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Servis provjera autentičnosti pomoću računa iz aplikacije logike aplikacije servisa za aplikaciju API poseban je slučaj opisanu u [korištenje prilagođenih API-JA hostirane na aplikacije servisa s aplikacijama logike](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="authorization"></a>Funkcioniranje autorizacije u aplikacije servisa

Imaju potpunu kontrolu nad zahtjeve za koji mogu pristupiti aplikaciji. Provjera autentičnosti za aplikaciju servisa / autorizacije može konfigurirati bilo koji od sljedećih ponašanja:

- Dopusti samo čija je autentičnost provjerena zahtjevi za doći do aplikacija.

    Ako preglednik primi anonimni zahtjev, aplikacije servisa za bit ćete preusmjereni na stranicu za davatelja identiteta koji ste odabrali da bi korisnici mogli prijaviti. Ako je zahtjev dolazi s mobilnog uređaja, vraćeni odgovor je je HTTP _401 Unauthorized_ odgovor.

    Uz tu se mogućnost ne morate pisati kod provjere autentičnosti u svojoj aplikaciji. Ako vam je potrebna precizniji autorizacije informacija o korisniku dostupna kod.

- Dopusti sve zahtjeve za dosegne aplikacije, ali čija je autentičnost provjerena zahtjeve za provjeru valjanosti i prenesite duž podatke za provjeru autentičnosti u zaglavljima HTTP.

    Ta mogućnost preusmjerava autorizacije odluka u kodu aplikacije. Pruža veću fleksibilnost obrade zahtjeva za anonimni, ali ćete se morati kod.

- Dopusti sve zahtjeve za dosegne aplikacija, a ne akcijama podatke za provjeru autentičnosti u zahtjeve.

    U ovom slučaju provjeru autentičnosti / autorizacije značajka isključena. Zadatke za provjeru autentičnosti i ovlaštenja su potpuno kodu aplikacije.

Prethodni ponašanja kontrolira mogućnost **poduzeti kada zahtjeva nije autentičnost** na portalu za Azure. Ako se odlučite * *prijavite se u *naziv davatelja* **, sve zahtjeve za morati proći.** Dopusti zahtjev (ne poduzmete) ** preusmjerava odluka autorizacije kodu, ali i dalje sadrži podatke za provjeru autentičnosti. Ako želite imati kod rukovati sve, možete onemogućiti provjeru autentičnosti i značajke za autorizaciju.

## <a name="working-with-user-identities-in-your-application"></a>Rad s identitetima korisnika u aplikaciji

Aplikacije servisa za prosljeđuje neke korisničke informacije u aplikaciji pomoću posebnih zaglavlja. Vanjski zahtjeve sprječavaju ta zaglavlja i samo ako prezentacija postavljaju se tako da provjera autentičnosti za aplikaciju servisa / autorizacije. Nekoliko primjera zaglavlja obuhvaćaju sljedeće:

* X-MS-KLIJENT-GLAVNICU-NAME
* X-MS--GLAVNICU-ID KLIJENTA
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Kod napisan bilo koji jezik ili framework možete dobiti informacije potrebne iz ove zaglavlja. Za aplikacije ASP.NET 4.6 **ClaimsPrincipal** automatski postavlja s odgovarajućim vrijednostima.

Aplikaciju možete dobiti dodatne korisničke pojedinosti putem HTTP GET na na `/.auth/me` krajnja točka aplikacije. Valjani token koji je uključen u zahtjevu za će vratiti JSON tereta s pojedinosti o davatelju usluga koje se koristi, temeljni token davatelja i neke korisničke informacije. Poslužitelj za mobilne aplikacije SDK-ovi sadrže metode preglednika da biste radili s tim podacima. Dodatne informacije potražite u članku [upute za korištenje u Node.js SDK Azure mobilne aplikacije](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)i [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Dokumentacija i dodatni resursi

### <a name="identity-providers"></a>Davatelji identiteta
Sljedeći vodiči za prikaz upute za konfiguriranje aplikacije servisa za korištenje davatelja usluge provjere autentičnosti za različite:

- [Upute za konfiguriranje aplikacije da biste koristili prijava Azure Active Directory][AAD]
- [Upute za konfiguriranje aplikacije da biste koristili prijava za Facebook][Facebook]
- [Upute za konfiguriranje aplikacije da biste koristili prijavu u Google][Google]
- [Upute za konfiguriranje aplikacije da biste koristili Microsoftov Account prijava][MSA]
- [Upute za konfiguriranje aplikacije da biste koristili Twitter prijava][Twitter]

Ako želite koristiti u sustavu identiteta osim one navedene ovdje vam može poslužiti [Pretpregled podrška za prilagođene provjere autentičnosti na poslužitelju za mobilne aplikacije .NET SDK][custom-auth], koji se može koristiti u web-aplikacijama, mobilne aplikacije ili aplikacije API-JA.

### <a name="web-applications"></a>Web-aplikacije
Sljedeći vodiči za prikaz kako dodati provjere autentičnosti web-aplikacije:

- [Početak rada sa servisom Azure aplikacije – dio 2][web-getstarted]

### <a name="mobile-applications"></a>Mobilne aplikacije
Sljedeći vodiči za pokazati kako dodati provjere autentičnosti za mobilne klijente pomoću tijek usmjereni na poslužitelju:

- [Dodavanje provjere autentičnosti za aplikaciju za iOS][iOS]
- [Dodavanje provjere autentičnosti u aplikaciju za Android] [Sa sustavom android]
- [Dodavanje provjere autentičnosti u aplikaciju za Windows] [Windows]
- [Dodavanje provjere autentičnosti za aplikaciju programa Xamarin.iOS] [Xamarin.iOS]
- [Dodavanje provjere autentičnosti za aplikaciju programa Xamarin.Android] [Xamarin.Android]
- [Dodavanje provjere autentičnosti za aplikaciju programa Xamarin.Forms] [Xamarin.Forms]
- [Dodavanje provjere autentičnosti za aplikaciju programa Cordova] [Cordova]

Ako želite koristiti tijek usmjereni na klijent za Azure Active Directory, koristite u sljedećim resursima:

- [Koristite za provjeru autentičnosti biblioteke imenika Active Directory za iOS][ADAL-iOS]
- [Korištenje na provjera autentičnosti biblioteke imenika Active Directory za Android][ADAL-Android]
- [Pomoću provjere autentičnosti biblioteke servisa Active Directory za Windows i Xamarin][ADAL-dotnet]

Ako želite koristiti tijek usmjereni na klijent za Facebook, koristite u sljedećim resursima:

- [Korištenje servisa Facebook SDK za iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Ako želite koristiti tijek usmjereni na klijent za Twitter, koristite u sljedećim resursima:

- [Korištenje servisa Twitter tkanina za iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Ako želite koristiti tijek usmjereni na klijent za Google, koristite u sljedećim resursima:

- [Korištenje SDK za prijavu u Google za iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>API aplikacije
Sljedeći vodiči za pokazalo kako se zaštititi API aplikacija:

- [Provjera autentičnosti korisnika API aplikacije u aplikacije servisa za Azure][apia-user]
- [Glavni provjera autentičnosti servisa za API aplikacije u aplikacije servisa za Azure][apia-service]









[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
