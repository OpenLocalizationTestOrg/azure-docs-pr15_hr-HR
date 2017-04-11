<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Upute za sastavljanje aplikacije pomoću protokole koje podržava Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD B2C: Provjera autentičnosti protokola

Azure Active Directory (Azure AD) B2C omogućuje identitet kao servis za aplikacije po dva standardnih industrijskih protokola za podršku: OpenID povezati i OAuth 2.0. Servis je usklađene sa standardima, no sve dva implementacije te protokola imaju neznatne razlike.  Informacije u ovom vodiču bit će koristan ako putem izravno slanje i obrade zahtjeva za HTTP umjesto korištenja Otvori zamjena unesite svoj kod. Preporučujemo da prije nego što ste prikazali detalje o svakom određeni protokol pročitajte ovu stranicu. No ako ste već upoznati s Azure AD B2C, možete posjetiti izravno [vodiči protokol](#protocols).

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>Osnove
Svaku aplikaciju koja koristi Azure AD B2C mora biti registriran u direktoriju B2C [Azure portal](https://portal.azure.com). Postupak registracije aplikacije prikuplja i nekoliko vrijednosti dodjeljuje aplikacije:

- **ID aplikacije** koje služi kao jedinstvena identifikacija aplikacije.
- **Preusmjeravanje URI** ili **identifikator paket** koji se mogu koristiti za usmjeravanje odgovore ponovno pokrenite aplikaciju.
- Nekoliko drugih vrijednosti specifične za scenarij. Dodatne informacije potražite upute [Da biste registrirali aplikacije](active-directory-b2c-app-registration.md).

Kada registrirate aplikacije, komunikaciju s Azure AD slanjem zahtjeva za krajnju točku 2.0:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Četiri strane su gotovo sve OAuth i povezivanje OpenID tokova, koji je uključen u sustavu exchange:

![Uloge OAuth 2.0](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

- **Provjeru autentičnosti poslužitelja** je krajnja točka za Azure AD 2.0. Sigurno obrađuje sve povezano sa korisničke informacije i pristup. Bavi i pouzdanost odnose između stranama u tijeku. Nije odgovoran za potvrđivanja identitet korisnika, dodjelu i Opoziv pristupa resursima i izdavanja tokena. Nije poznat i kao davatelja identiteta.
- **Vlasnik resursa** obično je krajnjeg korisnika. To je zabava vlasništvu podatke, a sadrži power dopustili trećim stranama da biste pristupili te podatke ili resursa.
- **Klijent OAuth** je aplikacije. Prepoznaju se po njegov ID aplikacije. To je obično strana koju krajnji korisnici upotrebljavaju. Tokeni ga zahtjeve i provjeru autentičnosti poslužitelja. Vlasnik resursa mora odobriti klijentskog dozvolu za pristup resursu.
- **Resursa poslužitelja** je gdje se nalazi resursa ili podatke. To smatra pouzdanima provjeru autentičnosti poslužitelja sigurno provjeru autentičnosti i OAuth klijenta. Također koristi nošenja pristupna tokena da biste bili sigurni da se mogu dodijeliti pristup resursu.

## <a name="policies"></a>Pravila
Arguably, pravila za Azure AD B2C su najvažnije značajke servisa. Azure AD B2C proširuje standardne protokole OAuth 2.0 i povezivanje OpenID uvođenjem pravila. Oni dopušta Azure AD B2C za izvođenje puno više od jednostavne provjere autentičnosti i autorizacije. Pravila potpuno opisuju potrošača identiteta sučelja, uključujući prijave, prijavu i uređivanje profila. Pravila može se definirati u administratora korisničkog Sučelja. Mogu se izvršiti pomoću parametra posebno upita u HTTP zahtjeva provjeru autentičnosti. Pravila nisu standardne značajke OAuth 2.0 i povezati se OpenID, pa biste trebali uzeti vrijeme ih. Dodatne informacije potražite u članku [Referentni vodič za Azure AD B2C pravila](active-directory-b2c-reference-policies.md).

## <a name="tokens"></a>Tokeni
Implementacija Azure AD B2C OAuth 2.0 i povezivanje OpenID čini proširenom korištenje nošenja tokena, uključujući nošenja tokena koje su prikazane kao tokeni JSON web (JWTs). Nošenja token je laganih sigurnosni token koja omogućuje pristup "nošenja" na zaštićenom resurs. Na nošenja je bilo koji proizvođača koje je moguće izlagati token. Azure AD morate najprije provjere autentičnosti na tulum možete primati nošenja token. No ako potrebne korake ne uzimaju se sigurnost token u prijenosa i prostora za pohranu, može biti intercepted i koristi neželjeni proizvođača.

Neke sigurnosnih tokena su ugrađene mehanizam koji sprječava neovlaštenog stranama njihova korištenja, ali nošenja tokeni nemaju ovaj mehanizam. Morate se prenijeti u sigurnog kanala, kao što je sigurnost sloja prijenosa (HTTPS). Ako nošenja token prenosi izvan sigurnog kanala, zlonamjerni strana pomoću Muškarac-u-u – srednje napada Nabava token te ga koristiti da bi se dobio neovlašteni pristup resursu zaštićeni. Na isti način sigurnost primijeniti kada nošenja tokeni i spremanja predmemoriranog za kasnije korištenje. Uvijek provjerite je li aplikacije prenosi i pohranjuje nošenja tokeni siguran način.

Dodatni nošenja tokena sigurnosna pitanja vezana uz, potražite u članku [RFC 6750 odjeljak 5](http://tools.ietf.org/html/rfc6750).

Dodatne detalje o različitim vrstama tokeni koje se koriste u Azure AD B2C dostupne su u [Azure AD tokena referencu](active-directory-b2c-reference-tokens.md).

## <a name="protocols"></a>Protokola

Kada budete spremni za pregled nekih primjera zahtjeve, možete početi s jednim od sljedećih vodiče. Svaki od njih odgovara scenarij određeni provjere autentičnosti. Ako vam je potrebna pomoć prilikom određivanja koje tijek vam najviše odgovara, pogledajte [vrste aplikacija možete stvoriti pomoću Azure AD B2C](active-directory-b2c-apps.md).

- [Stvaranje aplikacije za mobilne uređaje i nativne pomoću OAuth 2.0](active-directory-b2c-reference-oauth-code.md)
- [Stvaranje web-aplikacije pomoću OpenID povezivanje](active-directory-b2c-reference-oidc.md)
