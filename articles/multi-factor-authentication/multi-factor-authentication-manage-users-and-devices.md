<properties 
    pageTitle="Izvješća Azure višestruka provjera autentičnosti"
    description="Time se opisuje kako promijeniti korisničkih postavki, primjerice prisilno korisnicima da biste ponovno učinili postupka dokaz gore."
    documentationCenter=""
    services="multi-factor-authentication"
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Upravljanje postavkama korisnika s Azure višestruke provjere autentičnosti u oblaku

Kao administrator možete upravljati sljedeće postavke korisnicima i uređajima.  

- [Obavezan odabranim korisnicima možete ponovno unijeti kontakt metode](#require-selected-users-to-provide-contact-methods-again)
- [Brisanje korisnika postojeće lozinke aplikacije](#delete-users-existing-app-passwords)
- [Vraćanje MFA na svim uređajima obustavljenom za korisnika](#restore-mfa-on-all-suspended-devices-for-a-user)






To je korisno ako izgubite ili krađe računala ili uređaja ili morate ukloniti korisnicima pristup.


## <a name="require-selected-users-to-provide-contact-methods-again"></a>Obavezan odabranim korisnicima možete ponovno unijeti kontakt metode

To će postavku Prisiljava korisnika da biste dovršili postupak registracije ponovno kada on se prijavi. Imajte na umu da će se aplikacije koje nisu preglednika nastaviti raditi ako korisnik ima aplikacije lozinke za njih.  Odabirom i **izbrisati sve postojeće aplikacije lozinke generira za odabrane korisnike**možete izbrisati aplikacije lozinke korisnika.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Upute za korisnike Obvezali da ponovno unijeti kontakt metode




1. Prijava na portal Azure klasični.
2. Na lijevoj strani kliknite servisa Active Directory.
3. U odjeljku direktorija kliknite direktorija za korisnika kojem želite da se traži da biste ponovno unijeti svoje vrstu kontakta.
4. Pri vrhu kliknite korisnicima.
5. Pri dnu stranice kliknite Upravljanje višestruke provjere za izradu deklaracije Otvorit će se stranica višestruke provjere autentičnosti.
6. Pronađite korisnika kojeg želite upravljati i potvrdite okvir koji se nalazi pokraj naziva. Možda ćete morati promijeniti prikaz pri vrhu.
7. To će otvorili vezu **Upravljanje korisničkih postavki** na desnoj strani. Kliknite ga.
8. Potvrdite **zahtijevaju odabranim korisnicima omogućuje ponovno kontakta metode**.
![Navedite kontakta metode](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
10. Kliknite Spremi.
11. Kliknite Zatvori

## <a name="delete-users-existing-app-passwords"></a>Brisanje korisnika postojeće lozinke aplikacije

To će se briše sve aplikacije lozinki koje je stvorio korisnika. Aplikacije koje nisu preglednika povezani s ove aplikacije lozinke prestat će funkcionirati dok se ne stvara novu lozinku aplikacije.

### <a name="how-to-delete-users-existing-app-passwords"></a>Brisanje korisnika postojeće lozinke aplikacije

1. Prijava na portal Azure klasični.
2. Na lijevoj strani kliknite servisa Active Directory.
3. U odjeljku direktorija kliknite direktorija za korisnika koje želite izbrisati lozinki aplikacije za.
4. Pri vrhu kliknite korisnicima.
5. Pri dnu stranice kliknite Upravljanje višestruke provjere za izradu deklaracije Otvorit će se stranica višestruke provjere autentičnosti.
6. Pronađite korisnika kojeg želite upravljati i potvrdite okvir koji se nalazi pokraj naziva. Možda ćete morati promijeniti prikaz pri vrhu.
7. To će otvorili vezu **Upravljanje korisničkih postavki** na desnoj strani. Kliknite ga.
8. Potvrdite **Brisanje sve postojeće aplikacije lozinke generira za odabrane korisnike**.
![Brisanje aplikacije lozinki](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
10. Kliknite Spremi.
10. Kliknite Zatvori.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Vraćanje MFA na svim uređajima zapamćenih za korisnika

Administratori imaju mogućnost vraćanja višestruka provjera autentičnosti na korisnici uređaja i preglednika. Kada to učinite, uklonit će se Zapamti MFA iz svih korisnika uređaji i preglednici i korisnik će se tražiti da biste koristili MFA prilikom prijave sljedeći put.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Kako vratiti MFA na svim uređajima obustavljenom za korisnika

1. Prijava na portal Azure klasični.
2. Na lijevoj strani kliknite servisa Active Directory.
3. U odjeljku direktorija kliknite direktorija za korisnika koje želite vratiti mfa na.
4. Pri vrhu kliknite korisnicima.
5. Pri dnu stranice kliknite Upravljanje višestruke provjere za izradu deklaracije Otvorit će se stranica višestruke provjere autentičnosti.
6. Pronađite korisnika kojeg želite upravljati i potvrdite okvir koji se nalazi pokraj naziva. Možda ćete morati promijeniti prikaz pri vrhu.
7. To će otvorili vezu **Upravljanje korisničkih postavki** na desnoj strani. Kliknite ga.
8. Potvrdite **Vraćanje višestruke provjere autentičnosti na svim uređajima zapamćenih**
![Izbriši aplikaciju lozinke](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Kliknite Spremi.
10. Kliknite Zatvori.
