<properties
    pageTitle="B2C Azure Active Directory: Višestruke provjere autentičnosti | Microsoft Azure"
    description="Kako omogućiti višestruke provjere autentičnosti u aplikacijama potrošača dostupnog osigurava Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Omogućiti višestruke provjere autentičnosti u korisničke dostupnog aplikacija

Azure Active Directory (Azure AD) B2C integrira izravno [Azure višestruka provjera autentičnosti](../multi-factor-authentication/multi-factor-authentication.md) tako da možete dodati drugu razinu zaštite sučelja za prijavu i prijava u aplikacije potrošača dostupnog. I to možete učiniti bez pisanja s jednim retkom kod. Trenutačno podržavamo nazvati i tekst poruke provjere valjanosti. Ako ste već stvorili pravila registracije i prijave, i dalje možete omogućiti višestruke provjere autentičnosti.

> [AZURE.NOTE]
Višestruka provjera autentičnosti mogu omogućiti i kada stvoriti pravila za prijavu i prijavu, ne samo tako da uredite postojeće pravila.

Ta značajka pomaže aplikacije rukovati scenariji kao što je sljedeće:

- Nije potreban višestruke provjere autentičnosti da biste pristupili jedne aplikacije, ali potreban je pristup neku drugu. Na primjer, mogu se prijaviti u aplikaciju osiguranja automatski pomoću računa za društvene ili lokalni korisnik, ali morate provjeriti telefonski broj prije pristupa kućni osiguranja aplikacije registrirani u istoj mapi.
- Nije potreban višestruke provjere autentičnosti da biste pristupili aplikaciju Općenito, no potrebna vam je pristup osjetljive dijelova unutar njega. Ako, na primjer, korisnik mogu se prijaviti u aplikaciju bankovne s društvene ili lokalni račun i potvrdite saldo računa, ali morate provjeriti telefonski broj prije prijenosa žičani.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Izmjena registracije pravilo da biste omogućili višestruka provjera autentičnosti

1. [Slijedite ove korake da biste došli do plohu značajke B2C portala za Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknite **pravila za prijavu**.
3. Kliknite prijavi pravilnika (na primjer, "B2C_1_SiUp") da biste ga otvorili.
4. Kliknite **višestruka provjera autentičnosti** , a zatim uključite **Stanje** na **Uključeno**. Kliknite **u redu**.
5. Kliknite **Spremi** pri vrhu na plohu.

Koristite značajku "Izvedi sada" na pravila da biste provjerili korisnička sučelja. Provjerite sljedeće:

Potrošačkog računa dobiti stvorena u direktoriju prije pojavljuje se na višestruke provjere autentičnosti korak. Tijekom koraka, korisnik se od vas zatraži da navedite broj telefona i potvrdite je. Ako je provjera uspješna, telefonski broj možete priložiti potrošačkog računa za kasnije korištenje. Čak i ako korisnik otkazuje ili pada, on možete zatražiti da biste provjerili telefonskog broja ponovno tijekom prijavu sljedeće (s višestruka provjera autentičnosti omogućeno).

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Izmjena pravilnika prijave da biste omogućili višestruka provjera autentičnosti

1. [Slijedite ove korake da biste došli do plohu značajke B2C portala za Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknite **pravila za prijavu**.
3. Kliknite prijava pravilnika (na primjer, "B2C_1_SiIn") da biste ga otvorili. Kliknite **Uređivanje** pri vrhu na plohu.
4. Kliknite **višestruka provjera autentičnosti** , a zatim uključite **Stanje** na **Uključeno**. Kliknite **u redu**.
5. Kliknite **Spremi** pri vrhu na plohu.

Koristite značajku "Izvedi sada" na pravila da biste provjerili korisnička sučelja. Provjerite sljedeće:

Kada korisnik prijavi (pomoću društvenih ili lokalni račun), ako je provjereno telefonski broj povezan s potrošačkog računa, on se od vas zatraži da biste ga potvrdili. Ako nemate telefonski broj koji je pridružena, korisnik je zatražiti da navedite ga i potvrdite je. Na uspjela provjera telefonski broj možete priložiti potrošačkog računa za kasnije korištenje.

## <a name="multi-factor-authentication-on-other-policies"></a>Višestruka provjera autentičnosti na druge pravila

Kao što je opisano za prijavu i prijavu pravila o iznad i moguće je omogućiti višestruke provjere autentičnosti na registracije ili pravila za prijavu i lozinku ponovno postaviti pravila. Bit će dostupan uskoro na profilu uređivanje pravilnika.
