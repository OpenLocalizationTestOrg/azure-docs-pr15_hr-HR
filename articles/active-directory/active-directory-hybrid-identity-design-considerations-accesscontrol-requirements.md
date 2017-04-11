
<properties
    pageTitle="Preduvjeti za kontrolu pristupa za određivanje Azure Active Directory hibridnog identiteta zahtjevi dizajna - | Microsoft Azure"
    description="Obuhvaća stupovima identiteta i označavanje zahtjeve za pristup za resurse za korisnike u hibridnom okruženju."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Određivanje pristup kontrola preduvjeti za hibridno rješenje identiteta
Kad tvrtkom ili ustanovom dizajnira njihova rješenja identiteta hibridnog također mogu koristiti prilika da biste pregledali pristup preduvjeti za resurse koji planiranja ga učiniti dostupnim za korisnike. Pristup podacima Unakrsna sve četiri stupovima identiteta, a to su:

- Administracija
- Provjera autentičnosti
- Autorizacija
- Nadzor

U odjeljcima koji se nalazi iza obrađuje provjere autentičnosti i autorizacije u više detalja, Administracija i nadzor dio životnom ciklusu hibridnog identitet. Dodatne informacije o ove mogućnosti pročitajte [zadatke upravljanja za određivanje hibridnog identitet](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) .

>[AZURE.NOTE]
Dodatne informacije o svakom od tih stupovima pročitajte [U četiri stupovima od identiteta – upravljanje identitetom u IT za dob hibridnog](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) .

## <a name="authentication-and-authorization"></a>Provjera autentičnosti i autorizacije
Postoje različitim scenarijima za provjeru autentičnosti i ovlaštenja, scenarija će imati preduvjete koje mora ispuniti rješenje hibridnog identitet koji će se primijene tvrtke. Scenariji koje obuhvaćaju tvrtke za tvrtke (B2B) komunikacije možete dodati dodatni test za IT administratore jer će potrebne da biste bili sigurni da načina provjere autentičnosti i autorizacije koji se koristi u tvrtki ili ustanovi mogli komunicirati sa svojim poslovni partneri. Tijekom postupka dizajniranje za provjeru autentičnosti i ovlaštenja preduvjeti bili sigurni da se odgovori na sljedeća pitanja:

- Će vašoj tvrtki ili ustanovi provjeru autentičnosti i autorizirali samo korisnike smještene u sustavu za upravljanje njihov identitet?
 - Postoje li sve tarife za scenarije B2B?
 - Ako je da, već znate koje protokole (SAML, OAuth, Kerberos, tokeni ili certifikati) će se koristiti za povezivanje obje tvrtke?
- Označava rješenje identiteta hibridnog koje namjeravate prihvaćaju podršku te protokole?

Drugi važne točka treba uzeti u obzir je mjesto spremišta provjere autentičnosti koje će koristiti korisnika i partnera nalazit će se i administratora modela koja će se koristiti. Imajte na umu sljedeće core dvije mogućnosti:
- Centralizirano: u ovaj model korisničke vjerodajnice, pravila i administracije može biti središnje lokalnog u oblaku.
- Hibridno: u ovaj model korisničke vjerodajnice, pravila i administriranje bit će središnje lokalnog poslužitelja i na repliciranoj u oblaku.

Model koji će vašoj tvrtki ili ustanovi prihvaćaju razlikovat će se prema svojim poslovnim zahtjevima želite odgovaraju na sljedeća pitanja da biste odredili gdje će se nalaziti sustav upravljanja identiteta i administratora način rada koristite:

- Ne vašoj tvrtki ili ustanovi trenutno imate na upravljanje identitetom lokalnog?
 - Ako je da, ne namjeravaju da biste ga zadržali?
 - Postoje li sve regulacije ili usklađenost zahtjevima da vaše tvrtke ili ustanove moraju pratiti tu određuje gdje treba nalaziti sustav upravljanja identiteta?
- Ne vaša tvrtka ili ustanova koristi jedinstvenu prijavu aplikacije koje se nalaze informacije o lokalnom ili u oblaku?
 - Ako da, prihvaćanja modela identiteta hibridnog utječe na tom se postupku?

## <a name="access-control"></a>Kontrola pristupa
Tijekom provjere autentičnosti i autorizacije su osnovni elementi da biste omogućili pristup korporativnih podataka pomoću provjere valjanosti korisnika, također važno je da biste odredili razinu pristupa koji ti će korisnici imati razinu pristupa administratorima će imati putem resurse koji su upravljate. Identitet rješenje hibridnog moraju imati mogućnost zrnastog pristup resursima, delegiranje i kontrola osnovni pristupa uloge. Provjerite odgovara na su sljedeće pitanje o kontrola pristupa:

- Vaša tvrtka ima više korisnika s dodatnim ovlasti za upravljanje sustavom identiteta?
 - Ako je da, svaki korisnik mora istu razinu pristupa?
- Vaša tvrtka potrebni delegiranje pristupa korisnicima omogućuje upravljanje određene resursa?
 - Ako je da, koliko se često to događa?
- Vaša tvrtka potrebni za integraciju mogućnosti upravljanja pristup između lokalnog i oblaka resursa?
- Vaša tvrtka potrebni za ograničavanje pristupa resursima prema nekih uvjeta?
- Želite li vaša tvrtka imati bilo koju aplikaciju koja je potreban pristup prilagođene kontrole za neke resurse
 - Ako je da, gdje tim aplikacijama nalaze (lokalni u oblaku)?
 - Ako je da, gdje se one ciljani resursa koji se nalazi (lokalni u oblaku)?

>[AZURE.NOTE]
Provjerite je li bilješke svaki odgovor i razumijevanje rationale iza odgovor. [Definiranje Strategije za zaštitu podataka](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) će ići preko mogućnosti dostupne i prednosti i nedostatke svake mogućnosti.  Odgovorima na ta pitanja koju ćete odabrati koju mogućnost najbolje odgovara vašim potrebama tvrtke.

## <a name="next-steps"></a>Daljnji koraci

[Određivanje incidenta odgovor preduvjeti](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Vidi također
[Pregled pitanja vezana uz dizajn](active-directory-hybrid-identity-design-considerations-overview.md)
