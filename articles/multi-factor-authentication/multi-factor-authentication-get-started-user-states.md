<properties 
    pageTitle="Microsoft Azure višestruke provjere autentičnosti korisnika stanja"
    description="Saznajte više o stanja korisnika u Azure MFA."
    services="multi-factor-authentication"
    documentationCenter=""
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

# <a name="user-states-in-azure-multi-factor-authentication"></a>Korisnik stanja u Azure višestruka provjera autentičnosti

Korisnički računi u Azure višestruka provjera autentičnosti imaju sljedeća tri različita stanja:

Stanje | Opis |Aplikacije koje nisu preglednika utječe| Bilješke
:-------------: | :-------------: |:-------------: |:-------------: |
Onemogućeni | Zadano stanje novog korisnika nije uključen u višestruke provjere autentičnosti.|ne|Korisnik je koristiti višestruke provjere autentičnosti.
Omogućeno |Korisnik uključena u višestruke provjere autentičnosti.|ne.  Oni nastaviti s radom dok se ne postupka registracije.|Korisnik je omogućen, ali nije dovršena postupak registracije. Će se zatražiti da biste dovršili postupak na sljedeći prijava.
Provesti|Korisnik uključena, pa je dovršena postupak registracije za korištenje višestruke provjere autentičnosti.|Da.  Aplikacija tražila lozinka aplikacije. | Korisnik može ili bila registracije. Ako su ste dovršili postupak registracije, zatim koriste višestruke provjere autentičnosti. U suprotnom korisnika će se zatražiti da biste dovršili postupak na sljedeći prijava.

## <a name="changing-a-user-state"></a>Promjena korisnikova stanja
Korisnici stanje mijenja se ovisno o tome hoće li prošla instalacijski program za MFA i je li korisnik je dovršena postupak.  Kada uključite MFA za korisnika, korisnici stanje promijenit će se iz će onemogućiti omogućena.  Nakon što korisnika čije stanje promijenjen omogućena, prijavi i Završi proces, njihovo stanje promijenit će nametnuti.  

### <a name="to-view-a-users-state"></a>Da biste pogledali korisničke stanja
--------------------------------------------------------------------------------
1.  Prijavite se na **portal za Azure klasični** kao Administrator.
2.  Na lijevoj strani kliknite **Servisa Active Directory**.
3.  U odjeljku **direktorija** kliknite direktorija za korisnika koje želite omogućiti.
![Kliknite imenika](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Pri vrhu kliknite **korisnicima**.
5.  Pri dnu stranice kliknite **Upravljanje provjere autentičnosti višestruke provjere**.
![Kliknite imenika](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Otvorit će se na novoj kartici preglednika.  Ćete moći vidjeti stanje korisnika.
![Kliknite imenika](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

###<a name="to-change-the-state-from-disabled-to-enabled"></a>Da biste promijenili status s onemogućeno za omogućeno
1.  Prijavite se na **portal za Azure klasični** kao Administrator.
2.  Na lijevoj strani kliknite **Servisa Active Directory**.
3.  U odjeljku **direktorija** kliknite direktorija za korisnika koje želite omogućiti.
![Kliknite imenika](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Pri vrhu kliknite **korisnicima**.
5.  Pri dnu stranice kliknite **Upravljanje provjere autentičnosti višestruke provjere**.
![Kliknite imenika](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Otvorit će se na novoj kartici preglednika.  Pronađite korisnika kojeg želite omogućiti višestruke provjere autentičnosti. Možda ćete morati promijeniti prikaz pri vrhu. Provjerite je li status **onemogućene.** 
 ![Omogući korisnika](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Potvrdite **potvrdite** okvir pokraj njihovog imena.
7.  Na desnoj strani kliknite **Omogući**.
![Omogućivanje korisnika](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Kliknite **Omogući provjeru višestruke provjere autentičnosti**.
![Omogućivanje korisnika](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Trebali biste Primijetit ćete korisnika stanje promijenila iz **onemogućene** u **omogućena**.
![Omogućivanje korisnicima](./media/multi-factor-authentication-get-started-cloud/user.png)
10.  Nakon što ste omogućili korisnika, preporučuje se da vam obavijestite ih putem e-pošte.  Ga treba obavještava ih načina na koje možete koristiti svojih aplikacija koje nisu preglednika da biste izbjegli se zaključan.

### <a name="to-change-the-state-from-enabledenforced-to-disabled"></a>Da biste promijenili stanje iz omogućeno/nametnuo onemogućeno
1.  Prijavite se na **portal za Azure klasični** kao Administrator.
2.  Na lijevoj strani kliknite **Servisa Active Directory**.
3.  U odjeljku **direktorija** kliknite direktorija za korisnika koje želite omogućiti.
![Kliknite imenika](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Pri vrhu kliknite **korisnicima**.
5.  Pri dnu stranice kliknite **Upravljanje provjere autentičnosti višestruke provjere**.
![Kliknite imenika](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Otvorit će se na novoj kartici preglednika.  Pronađite korisnika kojeg želite onemogućiti. Možda ćete morati promijeniti prikaz pri vrhu. Provjerite je li status ili **omogućeno** ili **provesti.**
7.  Potvrdite **potvrdite** okvir pokraj njihovog imena.
7.  Na desnoj strani kliknite **Onemogući**.
![Onemogući korisnika](./media/multi-factor-authentication-get-started-user-states/userstate2.png)
8.  Od vas će se zatražiti da biste to potvrdili.  Kliknite **da**.
![Onemogući korisnika](./media/multi-factor-authentication-get-started-user-states/userstate3.png)
9.  Trebali biste vidjeti da je uspjelo.  Kliknite **zatvorite.** 
 ![Onemogući korisnika](./media/multi-factor-authentication-get-started-user-states/userstate4.png)
