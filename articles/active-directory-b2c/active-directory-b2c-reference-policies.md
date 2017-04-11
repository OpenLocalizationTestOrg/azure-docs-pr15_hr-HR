<properties
    pageTitle="Azure Active Directory B2C: Okvir Extensible pravila | Microsoft Azure"
    description="Tema na okvir extensible pravila programa Azure Active Directory B2C i kako stvoriti različite vrste pravila"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-extensible-policy-framework"></a>Azure Active Directory B2C: Okvir Extensible pravila

## <a name="the-basics"></a>Osnove

Okvir za extensible pravila od B2C Azure Active Directory (Azure AD) je core jačinu servis. Pravila potpuno opisuju korisnička sučelja identitet kao što su prijave, prijavu ili profila za uređivanje. Ako, primjerice, prijavu pravila omogućuje kontrolu ponašanja konfiguriranjem sljedeće postavke:

- Vrste računa (računi društvenih kao što su Facebook ili lokalni računi kao što je adresa e-pošte) koji korisnici mogu koristiti da biste se registrirali za aplikaciju.
- Atributi (na primjer, imena, poštanski i shoe veličina) prikupljeni s korisnik prilikom registracije.
- Korištenje višestruke provjere autentičnosti.
- Izgled – i – dojam svih stranica za prijavu.
- Informacije o (koje manifesti kao zahtjevima u token) da aplikacije prima kada pravila pokretanje Završi.

Možete stvoriti pravila za više različitih vrsta na klijentu i po potrebi ih koristiti u aplikacije. Pravila se može ponovno koristiti putem aplikacije. To razvojnim inženjerima omogućuje definiranje i izmjena korisnička sučelja identiteta s minimalnim ili bez promjene njihove kod.

Pravila su dostupni za upotrebu putem sučelja za jednostavne za razvojne inženjere. Aplikacija pokreće pravila pomoću standardne zahtjeva provjeru autentičnosti HTTP (prosljeđivanje parametar pravila u zahtjevu za) i prima prilagođene token kao odgovor. Ako, na primjer, jedina razlika između zahtjeve pozivanje Pravilnik za prijavu i one pozivanje pravila za prijavu je naziv pravila koji se koristi u parametra niza upita za "p":

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Dodatne pojedinosti o okvir pravila pročitajte sljedeću [objavu na blogu](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-policy"></a>Stvaranje pravila za prijavu

Da biste omogućili prijave na aplikaciju, morat ćete stvoriti pravilo za prijavu. Ovo pravilo opisuje sadržaje koje će korisnici prolaze kroz prilikom registracije i sadržaj tokena koje aplikacija će primiti uspješno znak kopije.

1. [Slijedite ove korake da biste došli do plohu značajke B2C portala za Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknite **pravila za prijavu**.
3. Kliknite **Dodaj +** pri vrhu na plohu.
4. **Naziv** određuje naziv registracije pravila koriste aplikacije. Na primjer, unesite "SiUp".
5. Kliknite **davatelji identiteta** i odaberite "E-pošte prijava". Po želji možete odabrati i davatelji identiteta društvene, ako već konfigurirana. Kliknite **u redu**.
6. Kliknite **Prijavi atribute**. Ovdje možete odabrati atribute koje želite prikupiti iz korisnik prilikom registracije. Na primjer, odaberite "Države/regije", "Zaslonsko ime" i "Poštanski broj". Kliknite **u redu**.
7. Kliknite **zahtjevima aplikacije**. Ovdje možete odabrati zahtjevima koji će se prikazati u tokeni šalje natrag u aplikaciji nakon uspješne sučelje za prijavu. Na primjer, odaberite "Zaslonsko ime", "Davatelja identiteta", "Poštanski broj", "Korisnik je novi" i "Objekt ID korisnika".
8. Kliknite **Stvori**. Imajte na umu da će se pojaviti pravila netom stvorili kao "**B2C_1_SiUp**" (u **B2C\_1\_ ** djelić automatski se dodaju) u plohu **pravilnicima za prijavu** .
9. Otvorite pravilnik tako da kliknete "**B2C_1_SiUp**".
10. Odaberite "Contoso B2C aplikacije" u **aplikacijama** padajući popis i `https://localhost:44321/` u na **odgovor URL / preusmjeravanje URI** padajućeg popisa.
11. Kliknite **Izvedi sada**. Otvorit će se na novoj kartici preglednika, a možete pokrenuti putem potrošača pisanju registracije za svoju aplikaciju.

    > [AZURE.NOTE]
    Potrebno prema gore za nekoliko minuta za stvaranje pravila i ažuriranja stupile na snagu.

## <a name="create-a-sign-in-policy"></a>Stvaranje pravila za prijavu

Da biste omogućili prijavu na aplikaciju, morat ćete stvoriti pravila za prijavu. Ovo pravilo opisuje sadržaje koje korisnici će prolaze kroz tijekom prijave i sadržaj tokena koje će primiti aplikacija na uspješno znak dodataka.

1. [Slijedite ove korake da biste došli do plohu značajke B2C portala za Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknite **pravila za prijavu**.
3. Kliknite **Dodaj +** pri vrhu na plohu.
4. **Naziv** određuje naziv pravila za prijavu u koji se koristio aplikacije. Na primjer, unesite "SiIn".
5. Kliknite **davatelji identiteta** i odaberite "Prijava lokalni račun". Po želji možete odabrati i davatelji identiteta društvene, ako već konfigurirana. Kliknite **u redu**.
6. Kliknite **zahtjevima aplikacije**. Ovdje možete odabrati zahtjevima koji će se prikazati u tokeni šalje natrag u aplikaciji nakon je uspješno sučelje za prijavu. Na primjer, odaberite "Zaslonsko ime", "Davatelja identiteta", "Poštanski broj" i "Objekt ID korisnika". Kliknite **u redu**.
7. Kliknite **Stvori**. Imajte na umu da će se pojaviti pravila netom stvorili kao "**B2C_1_SiIn**" (u **B2C\_1\_ ** fragment automatski se dodaju) u plohu **pravila za prijavu** .
8. Otvorite pravilnik tako da kliknete "**B2C_1_SiIn**".
9. Odaberite "Contoso B2C aplikacije" u **aplikacijama** padajući popis i `https://localhost:44321/` u na **odgovor URL / preusmjeravanje URI** padajućeg popisa.
10. Kliknite **Izvedi sada**. Otvorit će se na novoj kartici preglednika, a možete pokrenuti putem potrošača pisanju prijava u aplikaciju.

    > [AZURE.NOTE]
    Potrebno prema gore za nekoliko minuta za stvaranje pravila i ažuriranja stupile na snagu.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Stvaranje pravila registracije ili prijave

Ovo pravilo rukuje oba korisničke prijave i prijavu iskustva s jednom konfiguracijom. Korisnici su vodstvom dolje desno put (registracije ili prijave) ovisno o kontekstu. Opisuje i sadržaj tokena koje aplikacije primit će nakon uspješne kopije znak ili znak dodataka.  Uzorak koda za pravila registracije ili prijave je [dostupan ovdje](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

1. [Slijedite ove korake da biste došli do plohu značajke B2C portala za Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknite **registracije ili prijave pravila**.
3. Kliknite **Dodaj +** pri vrhu na plohu.
4. **Naziv** određuje naziv registracije pravila koriste aplikacije. Na primjer, unesite "SiUpIn".
5. Kliknite **davatelji identiteta** i odaberite "E-pošte prijava". Po želji možete odabrati i davatelji identiteta društvene, ako već konfigurirana. Kliknite **u redu**.
6. Kliknite **Prijavi atribute**. Ovdje možete odabrati atribute koje želite prikupiti iz korisnik prilikom registracije. Na primjer, odaberite "Države/regije", "Zaslonsko ime" i "Poštanski broj". Kliknite **u redu**.
7. Kliknite **zahtjevima aplikacije**. Ovdje možete odabrati zahtjevima koji će se prikazati u tokeni šalje natrag u aplikaciji nakon uspješne sučelje za prijavu ili prijavu. Na primjer, odaberite "Zaslonsko ime", "Davatelja identiteta", "Poštanski broj", "Korisnik je novi" i "Objekt ID korisnika".
8. Kliknite **Stvori**. Imajte na umu da će se pojaviti pravila netom stvorili kao "**B2C_1_SiUpIn**" (u **B2C\_1\_ ** fragment automatski se dodaju) u plohu **registracije ili prijave pravila** .
9. Otvorite pravilo tako da kliknete "**B2C_1_SiUpIn**".
10. Odaberite "Contoso B2C aplikacije" u **aplikacijama** padajući popis i `https://localhost:44321/` u na **odgovor URL / preusmjeravanje URI** padajućeg popisa.
11. Kliknite **Izvedi sada**. Otvorit će se na novoj kartici preglednika, a možete pokrenuti putem registracije ili prijave korisnička sučelja kao što je konfiguriran.

    > [AZURE.NOTE]
    Potrebno prema gore za nekoliko minuta za stvaranje pravila i ažuriranja stupile na snagu.

## <a name="create-a-profile-editing-policy"></a>Stvaranje pravila za uređivanje profila

Da biste omogućili profila na svoju aplikaciju za uređivanje, morat ćete stvoriti pravila za uređivanje profila. Ovo pravilo opisuje sadržaje koje će korisnici prolaze kroz tijekom Uređivanje profila i sadržaj tokena koje aplikacija će primiti uspješan dovršetak.

1. [Slijedite ove korake da biste došli do plohu značajke B2C portala za Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknite **pravila za uređivanje profila**.
3. Kliknite **Dodaj +** pri vrhu na plohu.
4. **Naziv** određuje naziv pravila koja koristi značajka aplikacije za uređivanje profila. Na primjer, unesite "SiPe".
5. Kliknite **davatelji identiteta** i odaberite "Adresa e-pošte". Po želji možete odabrati i davatelji identiteta društvene, ako već konfigurirana. Kliknite **u redu**.
6. Kliknite **atribute profila**. Ovdje možete odabrati atribute koje korisnik može prikazivati i uređivati. Na primjer, odaberite "Države/regije", "Zaslonsko ime" i "Poštanski broj". Kliknite **u redu**.
7. Kliknite **zahtjevima aplikacije**. Ovdje možete odabrati zahtjevima koji će se prikazati u tokeni šalje natrag u aplikaciji nakon uspješne profila sučelje za uređivanje. Na primjer, odaberite "Zaslonsko ime" i "Poštanski broj".
8. Kliknite **Stvori**. Imajte na umu da se pravilo upravo stvorili pojavit će se kao "**B2C_1_SiPe**" (u **B2C\_1\_ ** fragment automatski se dodaju) u plohu **pravila za uređivanje profila** .
9. Otvorite pravilnik tako da kliknete "**B2C_1_SiPe**".
10. Odaberite "Contoso B2C aplikacije" u **aplikacijama** padajući popis i `https://localhost:44321/` u na **odgovor URL / preusmjeravanje URI** padajućeg popisa.
11. Kliknite **Izvedi sada**. Otvorit će se na novoj kartici preglednika, a možete pokrenuti putem korisnička sučelja u aplikaciji za uređivanje profila.

    > [AZURE.NOTE]
    Potrebno prema gore za nekoliko minuta za stvaranje pravila i ažuriranja stupile na snagu.
    
## <a name="create-a-password-reset-policy"></a>Stvaranje pravila za ponovno postavljanje lozinke

Da biste omogućili preciznije za vraćanje izvorne lozinke na aplikaciju, morat ćete stvoriti pravila za ponovno postavljanje lozinke. Napomena da na razini klijenta za ponovno postavljanje lozinke mogućnost naveden [ovdje](active-directory-b2c-reference-sspr.md) je i dalje primjenjiva pravila za prijavu. Ovo pravilo opisuje sadržaje koje će se korisnici prolaze kroz tijekom ponovno postavljanje lozinke i sadržaj tokena koje aplikacija će primiti uspješan dovršetak.

1. [Slijedite ove korake da biste došli do plohu značajke B2C portala za Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknite **pravila za ponovno postavljanje lozinke**.
3. Kliknite **Dodaj +** pri vrhu na plohu.
4. **Naziv** određuje naziv pravila koja koristi značajka aplikacije za vraćanje izvorne lozinke. Na primjer, unesite "SSPR".
5. Kliknite **davatelji identiteta** i odaberite "Ponovno postavljanje lozinke pomoću adrese e-pošte". Kliknite **u redu**.
6. Kliknite **zahtjevima aplikacije**. Ovdje možete odabrati zahtjevima koji će se prikazati u tokeni šalje natrag u aplikaciji nakon sučelje za ponovno postavljanje lozinke za uspješan. Na primjer, odaberite "Objekt ID korisnika".
7. Kliknite **Stvori**. Imajte na umu da se pravilo upravo stvorili pojavit će se kao "**B2C_1_SSPR**" (u **B2C\_1\_ ** fragment automatski se dodaju) u plohu **pravila za ponovno postavljanje lozinke** .
8. Otvorite pravilo tako da kliknete "**B2C_1_SSPR**".
9. Odaberite "Contoso B2C aplikacije" u **aplikacijama** padajući popis i `https://localhost:44321/` u na **odgovor URL / preusmjeravanje URI** padajućeg popisa.
10. Kliknite **Izvedi sada**. Otvorit će se na novoj kartici preglednika, a možete pokrenuti putem potrošača sučelje za ponovno postavljanje lozinke u aplikaciji.

    > [AZURE.NOTE]
    Potrebno prema gore za nekoliko minuta za stvaranje pravila i ažuriranja stupile na snagu.

## <a name="additional-resources"></a>Dodatni resursi

- [Tokena, sesije i jedan konfiguracije za prijavu](active-directory-b2c-token-session-sso.md).
