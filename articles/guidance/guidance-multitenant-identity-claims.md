<properties
   pageTitle="Rad sa sustavom zahtjeva identiteta u složene aplikacije | Microsoft Azure"
   description="Način na koji se na korištenje zahtjeve za provjeru valjanosti izdavača i autorizacije"
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
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="working-with-claims-based-identities-in-multitenant-applications"></a>Rad s identitete utemeljene na zahtjevima u složene aplikacije

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ovaj je članak [dio niza]. Također je dovršena [primjer aplikacije] koja se isporučuje se uz ovaj niz.

## <a name="claims-in-azure-ad"></a>Sporove u Azure AD

Prilikom prijave, Azure AD šalje token za ID-a koji sadrži skup zahtjevima o korisniku. Zahtjev je jednostavno neku informaciju, izražen u paru ključa vrijednosti. Na primjer, `email` = `bob@contoso.com`.  Sporove ste je izdavač &mdash; u ovom slučaju Azure AD &mdash; koji je entitet koji potvrđuje korisnika i stvara na zahtjevima. U zahtjevima pouzdanost jer pouzdan izdavač. (Obratno, ako ne pouzdan izdavač, ne Smatraj pouzdanim u zahtjevima!)

Visoke razine:

1.  Korisnik se autentificira servisa.
2.  Na IDP šalje skup zahtjevima.
3.  Aplikaciju normalizes ili augments zahtjevima (neobavezno).
4.  Aplikaciju koristi u zahtjevima za donošenje odluka autorizacije.

U OpenID povezati, skup zahtjevima koji se isporučuju upravlja se [parametar opseg] zahtjev za provjeru autentičnosti. Međutim, Azure AD problemi s ograničenim zahtjevima putem OpenID povezivanje; u odjeljku [podržani tokena i vrste zahtjeva]. Ako želite dodatne informacije o korisniku, morat ćete koristiti API Azure AD grafikonu.

Evo nekoliko zahtjevima iz AAD koje možda obično zanimaju aplikacije:

Vrsta zahtjeva u token ID-a |    Opis
-----------------------|--------------
aud | Tko token izdavanja za. To će biti ID-a aplikacije klijenta. Općenito govoreći, ne morate brinuti o ovom zahtjeva jer na proizvod automatski provjerava je. Primjer:`"91464657-d17a-4327-91f3-2ed99386406f"`
grupa   | Popis AAD grupe koje je korisnik član. Primjer:`["93e8f556-8661-4955-87b6-890bc043c30f", "fc781505-18ef-4a31-a7d5-7d931d7b857e"]`
ISS  | [Izdavač] tokena OIDC. Primjer:`https://sts.windows.net/b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4/`
ime    | Korisničko ime za prikaz. Primjer:`"Alice A."`
OID | Identifikator objekta za korisnika u AAD. Ova vrijednost je immutable i ponovno – korištenje identifikator korisnika. Pomoću ove vrijednost ne e-poštu, kao jedinstveni identifikator za korisnike; možete promijeniti adrese e-pošte. Ako koristite Azure AD grafikonu API-JA u aplikaciji, ID objekta je tu vrijednost koja se koristi za informacije o profilu upita. Primjer:`"59f9d2dc-995a-4ddf-915e-b3bb314a7fa4"`
uloge   | Popis aplikacija uloge za korisnika. Primjer:`["SurveyCreator"]`
tid | ID klijenta. Ta vrijednost je jedinstveni identifikator za klijenta u Azure AD. Primjer:`"b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4"`
unique_name | Ljudski čitljiv zaslonsko ime korisnika. Primjer:`"alice@contoso.com"`
UPN-a | Korisnikovo Glavno ime. Primjer:`"alice@contoso.com"`

U ovoj su tablici navedene vrste zahtjeva kako su prikazane u token ID-a. U ASP.NET osnovne 1.0, proizvod OpenID povezivanje pretvara neke vrste zahtjeva kada ga popunjava zbirke zahtjevima za upravitelja korisnika:

-   OID >`http://schemas.microsoft.com/identity/claims/objectidentifier`
-   tid >`http://schemas.microsoft.com/identity/claims/tenantid`
-   unique_name >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
-   UPN >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`

## <a name="claims-transformations"></a>Sporove transformacije

Tijekom tijek provjeru autentičnosti, možda ćete morati izmijeniti zahtjevima koji ste dobili od na IDP. U ASP.NET osnovne 1.0, možete izvršiti zahtjevima transformaciju unutar **AuthenticationValidated** događaj iz proizvod OpenID povezivanje. (Pogledajte [događaje provjere autentičnosti]).

Bilo koji zahtjevima koje dodate tijekom **AuthenticationValidated** spremaju se u provjere autentičnosti kolačića sesiju. Oni ne se završi natrag Azure AD.

Evo nekoliko primjera zahtjevima transformacije:

-   **Normalizacija zahtjevima**ili unosom zahtjevima dosljedan svim korisnicima. To je osobito Važno Ako dobivate zahtjevima iz većeg broja IDPs koje mogu koristiti zahtjeva za različite vrste slične podatke.
Ako, na primjer, Azure AD šalje zahtjev za "stupce upn" koji sadrži e-pošte korisnika. Drugi IDPs može poslati zahtjev u "e-pošte". Sljedeći kod pretvara zahtjeva za "stupce upn" u zahtjev za "slanje e-poštom":

    ```csharp
    var email = principal.FindFirst(ClaimTypes.Upn)?.Value;
    if (!string.IsNullOrWhiteSpace(email))
    {
      identity.AddClaim(new Claim(ClaimTypes.Email, email));
    }
    ```

- Dodavanje **zadane vrijednosti iz zahtjeva** za zahtjevima koje drugi ne postoji &mdash; korisnika, na primjer, dodijelite zadane uloge. U nekim slučajevima to može pojednostavniti logike za autorizaciju.
- Dodavanje **prilagođene zahtjeva vrste** sa specifičnim aplikacijama informacija o korisniku. Na primjer, mogu sadržavati neke informacije o korisniku u bazi podataka. Nije moguće dodati prilagođene zahtjeva tih informacija karata provjere autentičnosti. Zahtjev je pohranjena u kolačića, pa samo morate dobiti iz baze podataka jednom po sesiji prijava. S druge strane, preporučujemo i da bi se izbjeglo stvaranje prenapadno velike kolačići tako da je potrebno imati na umu trade-off između veličine kolačića nasuprot pretraživanja baze podataka.   

Po dovršetku tijeka provjere autentičnosti na zahtjevima dostupne u `HttpContext.User`. U tom trenutku treba tretirati ih kao zbirku koja je samo za čitanje &mdash; – primjerice, koristite autorizacije odluke.

## <a name="issuer-validation"></a>Provjera valjanosti izdavač
U OpenID povezati, izdavača zahtjeva ("iss") označava IDP koja je izdala token ID-a. Dio provjere autentičnosti tijek OIDC je da biste potvrdili da odgovara zahtjeva izdavač stvarni izdavač. Proizvod OIDC ovime rukuje umjesto vas.

U Azure AD vrijednost izdavač je jedinstveni po klijentu za AD (`https://sts.windows.net/<tenantID>`). Stoga aplikacija biste trebali učiniti potvrdite dodatne, da biste bili sigurni izdavač predstavlja klijentom koji je dopušteno da biste se prijavili aplikaciju.

Za aplikaciju jednog klijenta, možete jednostavno provjerite je li izdavač vlastite klijenta. Zapravo, proizvod OIDC to čini automatski prema zadanim postavkama. U aplikaciji više klijentu morate dopustiti za više kreditnih odgovara s različitim korisnicima. Slijede općenite pristup za korištenje:

-   U odjeljku mogućnosti proizvod OIDC postavite **ValidateIssuer** FALSE. Isključivanje automatske provjere.
-   Kad klijentom registrira, pohranu na klijentu i izdavač u korisnički DB.
-   Prilikom prijave, pogledajte članak izdavača u bazi podataka. Ako ne pronađete izdavač, to znači da nije registrirali taj klijent. Možete ih preusmjeriti na stranici Registracija.
-  Nije moguće blacklist i određene klijenata Ako, na primjer, za korisnike koji niste plaćanje pretplate.

Detaljnije rasprave, potražite u članku [prijave i smjernice za uhodavanje u složene aplikacije][signup].

## <a name="using-claims-for-authorization"></a>Korištenje zahtjevima za provjeru autentičnosti

Sa zahtjevima, identitet korisnika više nije monolithic entitet. Ako, na primjer, korisnik možda je adresa e-pošte, telefonski broj, rođendan, spol itd. Možda korisnika IDP pohranjuje sve ove informacije. No kada provjere autentičnosti korisnika obično dobit ćete podskup kao zahtjevima. U ovaj model identitet korisnika je jednostavno paket zahtjevima. Kada odaberete željene autorizacije odluke o korisniku, izgledat će za određene skupove zahtjevima. Drugim riječima, pitanje "Korisnik X akciju mogu izvršiti Y" konačni postaje "Ne X ste zahtjeva korisnika Z".

Evo nekih osnovnih uzoraka za provjeru zahtjevima.

-  Da biste provjerili ima li korisnik određeni zahtjeva s određenom vrijednosti:

    ```csharp
    if (User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
    ```
    Kod provjerava ima li korisnik uloga zahtjeva s vrijednošću "Administrator". Rukuje se ispravno kutije kojoj korisnik ima bez zahtjeva uloga ili više uloga zahtjevima.

    Klase **ClaimTypes** definira konstante za najčešće korištene zahtjeva vrste. Međutim, možete koristiti bilo koju vrijednost niza za vrstu zahtjeva.

-   Da biste dobili jednu vrijednost za vrstu zahtjeva kada očekujete tamo biti najviše jednu vrijednost:
    ```csharp
     string email = User.FindFirst(ClaimTypes.Email)?.Value;
    ```
-   Da biste dohvatili sve vrijednosti za tu vrstu zahtjeva:

    ```csharp
     IEnumerable<Claim> groups = User.FindAll("groups");
    ```

Dodatne informacije potražite u članku [autorizacija uloga i resursa u složene aplikacije][authorization].

## <a name="next-steps"></a>Daljnji koraci

- Pročitajte sljedeći članak u ovom nizu: [prijave i smjernice za uhodavanje u složene aplikacije][signup]
- Dodatne informacije o [autorizacije utemeljene na zahtjevima] u dokumentaciji 1.0 Core platforme ASP.NET

<!-- Links -->
[dio niza]: guidance-multitenant-identity.md
[opseg parametra]: http://nat.sakimura.org/2012/01/26/scopes-and-claims-in-openid-connect/
[Vrste zahtjeva i podržanim tokena]: ../active-directory/active-directory-token-and-claims.md
[Izdavač]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
[Provjera autentičnosti događaja]: guidance-multitenant-identity-authenticate.md#authentication-events
[signup]: guidance-multitenant-identity-signup.md
[Autorizacija utemeljene na zahtjevima]: https://docs.asp.net/en/latest/security/authorization/claims.html
[primjer aplikacije]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[authorization]: guidance-multitenant-identity-authorize.md
