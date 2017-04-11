<properties
    pageTitle="Početak rada Azure MFA u oblak | Microsoft Azure"
    description="To je stranica za provjeru autentičnosti Microsoft Azure multi-factor koji opisuje način za početak rada s Azure MFA u oblaku."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Uvod u Azure višestruke provjere autentičnosti u oblaku
U ovom se članku vodi kroz kako započeti rad sa sustavom Azure višestruke provjere autentičnosti u oblaku.

> [AZURE.NOTE]  Sljedeća dokumentacija pruža informacije o omogućivanju korisnika putem **Klasične Portal Azure**. Ako tražite informacije o tome kako postaviti Azure višestruke provjere autentičnosti za O365 korisnike, pročitajte članak [Postavljanje višestruke provjere autentičnosti za Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MFA u Oblaku](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisites"></a>Preduvjeti
Sljedeći preduvjeti potrebni su prije omogućivanja Azure višestruke provjere autentičnosti za korisnike.


1. [Registracija za pretplatu na Azure](https://azure.microsoft.com/pricing/free-trial/) – ako već nemate Azure pretplatu, morate registracije za jednu. Ako ste upravo počevši i pomoću Azure MFA možete koristiti probnu pretplatu
2. [Stvaranje multi-factor davatelja provjere autentičnosti](multi-factor-authentication-get-started-auth-provider.md) i njezino dodavanje direktorija ili [im dodijelili licence za korisnika](multi-factor-authentication-get-started-assign-licenses.md)

> [AZURE.NOTE]  Licenci dostupnih za korisnike koji imaju Azure MFA, Azure AD Premium ili Enterprise mobilnost paket (EMS).  MFA je sve obuhvaćeno Azure AD Premium i na EMS. Ako imate dovoljno licenci, nije potrebno da biste stvorili davatelja usluge provjere autentičnosti.


## <a name="turn-on-two-step-verification-for-users"></a>Uključivanje provjere dva koraka za korisnike
Da biste započeli koje je obavezna Provjera dva start na za korisnika, promijenite stanje korisnika iz onemogućeno za omogućena.  Dodatne informacije o stanja korisnika potražite u članku [Stanja korisnika u Azure višestruka provjera autentičnosti](multi-factor-authentication-get-started-user-states.md)

Koristite sljedeći postupak da biste omogućili MFA za korisnike.

### <a name="to-turn-on-multi-factor-authentication"></a>Da biste uključili višestruka provjera autentičnosti

1.  Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com) kao administrator.
2.  Na lijevoj strani kliknite **Servisa Active Directory**.
3.  U odjeljku direktorija, odaberite direktorija za korisnika koje želite omogućiti.
![Kliknite imenika](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Pri vrhu kliknite **korisnicima**.
5.  Pri dnu stranice kliknite **Upravljanje provjere autentičnosti višestruke provjere**. Otvorit će se na novoj kartici preglednika.
![Kliknite imenika](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Pronađite korisnika koje želite omogućiti za potvrdu dva koraka. Možda ćete morati promijeniti prikaz pri vrhu. Provjerite je li status **onemogućene.** 
 ![Omogući korisnika](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Potvrdite **potvrdite** okvir pokraj njihovog imena.
7.  Na desnoj strani kliknite **Omogući**.
![Omogućivanje korisnika](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Kliknite **Omogući provjeru višestruke provjere autentičnosti**.
![Omogućivanje korisnika](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Primijetit ćete da se stanje korisnika promijenila iz **onemogućeno** za **omogućena**.
![Omogućivanje korisnicima](./media/multi-factor-authentication-get-started-cloud/user.png)

Nakon što ste omogućili korisnika, trebali biste ih obavijestiti putem e-pošte. Na sljedećem su pokušaja prijave, oni će se zatražiti da biste registrirali svoj račun za potvrdu dva koraka. Kada su je počeli koristiti Provjera dva koraka, oni i potrebno da biste postavili lozinke aplikacije da biste izbjegli se zaključan iz aplikacije koje nisu preglednika.


## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>Da biste automatizirali uključivanjem potvrdu dva koraka pomoću komponente PowerShell

Da biste promijenili [Stanje](multi-factor-authentication-whats-next.md) pomoću [Komponente PowerShell Azure AD](../powershell-install-configure.md), možete koristiti sljedeće.  Možete promijeniti `$st.State` jednako jedno od sljedećih stanja:

- Omogućeno
- Provesti
- Onemogućeni  

> [AZURE.IMPORTANT]  Ne možemo radnju protiv premještanje korisnika izravno iz stanja Onemogući Enforced stanje. Osobe koje nisu-utemeljenima na pregledniku aplikacija će prestati raditi jer je korisnik je nestalo kroz MFA Registracija i nabavili [aplikaciju lozinku](multi-factor-authentication-whats-next.md#app-passwords). Ako imate aplikacije koje nisu-utemeljenima na pregledniku i tražila lozinka aplikacije, preporučujemo da otvorite iz kao onemogućena omogućeno. To korisnicima omogućuje registriranje i nabaviti aplikacije lozinke. Nakon toga, možete ih premjestiti na Enforced.

Pomoću komponente PowerShell bi mogućnost za skupno korisnicima. Trenutno nema bez masovnog omogućiti značajku na portalu za Azure i morate pojedinačno odaberite svakog korisnika. Ako imate mnogo korisnika to može biti vrlo zadatka. Stvaranjem programa PowerShell skripte pomoću sljedeće, možete prolazi kroz popis korisnika, a zatim ih omogućiti.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Evo jednog primjera:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Dodatne informacije potražite u članku [stanja korisnika u Azure višestruka provjera autentičnosti](multi-factor-authentication-get-started-user-states.md)

## <a name="next-steps"></a>Daljnji koraci
Sad kad ste postavili Azure višestruke provjere autentičnosti u oblak, možete konfigurirati i postavljanje implementaciju sustava. Dodatne informacije potražite u članku [Konfiguriranje Azure višestruka provjera autentičnosti](multi-factor-authentication-whats-next.md) .
