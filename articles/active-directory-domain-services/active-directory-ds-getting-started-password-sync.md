<properties
    pageTitle="Azure servisa Active Directory Domain Services: Omogući sinkronizaciju lozinke | Microsoft Azure"
    description="Uvod u Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Omogući sinkronizaciju lozinke za Azure servisa Active Directory Domain Services
U prethodnom zadaci omogućiti Azure AD domenske servise za vaš klijent Azure AD. Sljedeći zadatak jest omogućivanje vjerodajnica raspršivanja potreban za provjeru autentičnosti NTLM i Kerberos za sinkronizaciju servisa Azure AD domene. Kada postavite vjerodajnica sinkronizacije, korisnici mogli prijaviti upravljanih domenu pomoću svoje tvrtke vjerodajnice.

Koraka razlikuju se ovisno o tome je li tvrtka ili ustanova koristi samo oblak Azure AD smjernice ili je postavljena za sinkronizaciju s lokalnog imenika pomoću Azure AD Connect.

<br>

> [AZURE.SELECTOR]
- [Smjernice za samo oblak Azure AD](active-directory-ds-getting-started-password-sync.md)
- [Sinkronizirane Azure AD klijentu](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Zadatak 5: Omogući sinkronizaciju lozinke za AAD domenske servise za samo oblak Azure AD klijenta
Azure servisa Active Directory Domain Services mora vjerodajnica raspršivanja u obliku prikladna za NTLM i Kerberos provjeru autentičnosti, za provjeru autentičnosti korisnika na upravljanih domeni. Osim u slučaju da omogućite AAD domenske servise za vaš klijent, Azure AD ne generiranje ili spremanje vjerodajnica raspršivanja u oblik koji je potreban za provjeru autentičnosti NTLM ili Kerberos. Očite sigurnosnih vam razloga Azure AD i ne sprema vjerodajnice u Očisti tekstnom obliku. Stoga Azure AD nema način da biste generirali te NTLM ili Kerberos vjerodajnica raspršivanja na temelju postojećih vjerodajnica korisnika.

> [AZURE.NOTE] Ako vaša tvrtka ili ustanova s samo oblak Azure AD klijent, korisnici koji su potrebni za korištenje Azure servisa Active Directory Domain Services morate promijeniti svoje lozinke.

Ovaj postupak za promjenu lozinke uzrokuje vjerodajnicu raspršivanja potrebnih Azure servisa Active Directory Domain Services za provjeru autentičnosti Kerberos i NTLM generiranje u Azure AD. Možete ili isteka lozinke za sve korisnike u klijentu za koje je potrebno koristiti Azure servisa Active Directory Domain Services ili uputite ti korisnici za promjenu lozinke.


### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Omogućivanje NTLM i Kerberos generacije vjerodajnica Raspršivanje za samo oblak Azure AD klijenta
Slijede upute, morate upisati krajnjim korisnicima da mijenjaju svoje lozinke:

1. Idite na stranicu Azure AD teksta za tvrtku ili ustanovu pri [http://myapps.microsoft.com](http://myapps.microsoft.com).

2. Odaberite karticu **profila** na ovoj stranici.

3. Kliknite pločicu **Promjena lozinke** na ovoj stranici.

    ![Stvaranje virtualne mreže za Azure servisa Active Directory Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password.png)

    > [AZURE.NOTE] Ako ne vidite mogućnost **Promijeni lozinku** na stranici ploče programa Access, provjerite je li vaša organizacija konfigurirala [Upravljanje lozinkama u Azure AD](../active-directory/active-directory-passwords-getting-started.md).

4. Na stranici **Promjena lozinke** upišite svoju lozinku postojeći (stari) i unesite novu lozinku i potvrdite je. Kliknite **Pošalji**.

    ![Stvaranje virtualne mreže za Azure servisa Active Directory Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Nakon što ste promijenili lozinku, s novom lozinkom će moći koristiti u Azure AD domenske servise uskoro. Nakon nekoliko minuta (obično otprilike 20 minuta), možete se prijaviti na računalima pridruženima upravljanih domene pomoću nedavno promijenjenim lozinke.

<br>

## <a name="related-content"></a>Srodni sadržaji

- [Kako ažurirati vlastitu lozinku](../active-directory/active-directory-passwords-update-your-own-password.md)

- [Uvod u upravljanje lozinkama u Azure AD](../active-directory/active-directory-passwords-getting-started.md).

- [Omogući sinkronizaciju lozinke za AAD domenske servise za sinkronizirane Azure AD klijenta](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Administriranje domene upravljanih programa Azure servisa Active Directory Domain Services](active-directory-ds-admin-guide-administer-domain.md)

- [Uključivanje virtualnog računala za Windows s domenom upravljanih programa Azure servisa Active Directory Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)

- [Uključivanje crveno je vaša Enterprise Linux virtualnog računala s domenom upravljanih programa Azure servisa Active Directory Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
