<properties
    pageTitle="Početak rada davatelja usluge provjere autentičnosti za Azure multi-factor | Microsoft Azure"
    description="Saznajte kako stvoriti davateljem Azure višestruku provjeru autentičnosti."
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
    ms.date="10/14/2016"
    ms.author="kgremban"/>



# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Početak rada s davateljem Azure višestruku provjeru autentičnosti
Provjera dva koraka dostupna je prema zadanim postavkama za globalne administratore koji imaju Azure Active Directory i korisnike sustava Office 365. Međutim, ako želite da biste iskoristili [Napredne značajke](multi-factor-authentication-whats-next.md) pa treba kupite punu verziju programa Azure višestruke provjere autentičnosti (MFA).

> [AZURE.NOTE]  Davatelja usluge provjere autentičnosti višestruku Azure se koristi da biste iskoristili značajke koje nudi punu verziju Azure MFA. Je korisnicima koji **nemaju licence kroz Azure MFA, Azure AD Premium ili EMS**.  Azure MFA, Azure AD Premium i EMS obuhvaćaju punu verziju Azure MFA prema zadanim postavkama.  Ako imate licence, pa ne morate davatelja usluge provjere autentičnosti višestruku Azure.

Da biste preuzeli SDK-a potreban je davatelja provjere Azure višestruke provjere autentičnosti.

> [AZURE.IMPORTANT]  Da biste preuzeli SDK, stvorite davatelja usluge provjere autentičnosti višestruku Azure čak i ako imate Azure MFA, AAD Premium ili EMS licence.  Ako stvorite davateljem Azure multi-factor provjere autentičnosti u tu svrhu, a već imate licence, provjerite da biste stvorili davatelja s modelom **Po korisničkih** . Nakon toga povezati davatelj direktorij koji sadrži Azure MFA, Azure AD Premium ili EMS licence.  Time se osigurava da samo naplatu ako imate više jedinstvenih korisnika pomoću SDK od broj licenci koje ste vlasnik.


## <a name="to-create-a-multi-factor-auth-provider"></a>Da biste stvorili davatelja za provjeru autentičnosti višestruke provjere

Poduzmite sljedeće korake da biste stvorili davatelja usluge provjere autentičnosti višestruku Azure.

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com) kao administrator.
2. Na lijevoj strani odaberite **Servisa Active Directory**.
3. Na stranici servisa Active Directory, na vrhu, odaberite **Davatelja usluge provjere autentičnosti višestruke provjere**.
![Stvaranje davateljem MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Pri dnu, kliknite **Novo**.
![Stvaranje davateljem MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. U odjeljku aplikacije servisa, odaberite **Davatelja usluge provjere autentičnosti multi-factor**
![stvaranje davateljem MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Odaberite **brzo stvaranje**.
![Stvaranje davateljem MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Popunite sljedeća polja, a zatim odaberite **Stvori**.
    1. **Naziv** – naziv davatelja provjere autentičnosti višestruke provjere.
    2. **Korištenje modela** – želite li pojedinačnim korisnicima omogućiti ili platite po provjere valjanosti. Odaberite jednu od dvije mogućnosti:
        - Po provjera autentičnosti – nabavljanja model koji naplaćuje po provjere autentičnosti. Obično se koristi za scenarija koji koriste Azure višestruke provjere autentičnosti u aplikaciji potrošača dostupnog.
        - Po korisniku omogućeno – nabavljanja model koji naplaćuje po omogućen korisnika. Obično se koristi za pristup zaposlenika aplikacija kao što je Office 365. Tu mogućnost odaberite ako imate neki korisnici koji su već licencu za Azure MFA.
    2. **Direktorija** – koji je pridružen davatelja usluge provjere autentičnosti za višestruku klijent u Azure Active Directory. Imajte na umu sljedeće:
        - Ne morate u imeniku Azure AD da biste stvorili davatelja za provjeru autentičnosti višestruke provjere. Polje ostavite prazno samo namjeravate koristiti Azure višestruke provjere autentičnosti poslužitelja ili SDK.
        - Davatelj usluga za provjeru autentičnosti višestruku moraju biti pridruženi u imeniku Azure AD da biste iskoristili napredne značajke.
        - Azure AD Connect, AAD Sync ili DirSync su samo zahtjeva ako sinkronizirate lokalnog okruženja za Active Directory sa u imeniku Azure AD.  Ako koristite samo u imeniku Azure AD koja se sinkronizira, a zatim to nije obavezno.
![Stvaranje davateljem MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
5. Kada kliknete stvaranje, davatelja usluge provjere autentičnosti za višestruku se stvara i trebali biste vidjeti poruku koja govori: **uspješno stvorili višestruke provjere autentičnosti davatelja**. Kliknite **u redu**.
![Stvaranje davateljem MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)
