<properties
    pageTitle="Azure AD Connect: Početak rada s korištenju eksplicitnih postavki | Microsoft Azure"
    description="Upute za preuzimanje, instaliranje i pokretanje čarobnjaka za postavljanje za Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Uvod u korištenju eksplicitnih postavki Azure AD Connect
Azure AD Connect **Ekspresne postavke** koristi se kada se jednom-skupa stabala topologije i [sinkronizaciju lozinke](../active-directory-aadconnectsync-implement-password-synchronization.md) za provjeru autentičnosti. **Ekspresne postavke** je zadana mogućnost i koristi se za najčešće distribuiranih scenarij. Su samo nekoliko kratkih klikova odmah da biste proširili lokalnog imenika u oblak.

Prije pokretanja instalacije Azure AD Connect, provjerite je li da biste [preuzeli Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) i dovršite stara obavezna koraka u [Azure AD Connect: hardver i preduvjeti](../active-directory-aadconnect-prerequisites.md).

Ako eksplicitnih postavke odgovaraju topologiji, potražite u članku [povezani dokumentaciju](#related-documentation) za drugim situacijama.

## <a name="express-installation-of-azure-ad-connect"></a>Express instalacije Azure AD Connect
Vidjet ćete sljedeće korake u akcija u odjeljku [videozapisi](#videos) .

1. Prijavite se kao administrator lokalni poslužitelj koji želite instalirati Azure AD Connect na. Trebali biste to na poslužitelju želite biti poslužitelj za sinkronizaciju.
2. Pronađite i dvokliknite **AzureADConnect.msi**.
3. Na zaslonu dobrodošlice potvrdite okvir pristanete uvjete licenciranja, a zatim kliknite **Nastavi**.  
4. Na zaslonu postavke Express kliknite **koristi eksplicitnih postavke**.  
![Dobro došli u Azure AD povezivanje](./media/active-directory-aadconnect-get-started-express/express.png)
5. Na povezivanje Azure AD zaslon, unesite korisničko ime i lozinku za globalni administrator za Azure AD. Kliknite **Dalje**.  
![Povezivanje s Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) ako obavijest o pogrešci i imate problema s povezivanjem, a zatim pročitajte [Otklanjanje poteškoća s povezivanjem](../active-directory-aadconnect-troubleshoot-connectivity.md).
6. Na za povezivanje s AD DS zaslona, unesite korisničko ime i lozinku za račun administrator tvrtke. Možete unijeti dio domene u obliku koji je NetBios ili FQDN, odnosno FABRIKAM\administrator ili fabrikam.com\administrator. Kliknite **Dalje**.  
![Povezivanje s AD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. Stranici [**Azure AD prijavu konfiguracija**](../active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) prikazuje samo ako niste dovršili [preduvjeti](../active-directory-aadconnect-prerequisites.md) [Provjerite svoje domene](../active-directory-add-domain.md) .
![Neprovjereni domene](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
Ako vidite ovu stranicu, zatim pregledajte svake domene označeni **Niste dodali** i **Nije provjereno**. Provjerite je li tih domena koristite provjereno Azure AD. Kliknite simbol osvježavanja kada ste potvrdili domenu.
8. Na jeste li spremni za konfiguriranje zaslona, kliknite **Instaliraj**.
    - Na jeste li spremni za konfiguriranje stranice, po želji možete poništiti potvrdni okvir **pokretanje postupka za sinkronizaciju čim dovršava konfiguracije** . Trebali biste poništiti ovaj okvir ako želite napraviti dodatne konfiguracijske, kao što su [filtriranja](../active-directory-aadconnectsync-configure-filtering.md). Ako poništite odabir te mogućnosti čarobnjaka konfigurira sinkronizaciju, ali ne i raspored onemogućene. To se izvoditi dok ga omogućite ručno tako da [rerunning Čarobnjak za instalaciju](../active-directory-aadconnectsync-installation-wizard.md).
    - Ako imate sustava Exchange u lokalni Active Directory, zatim imate mogućnost da biste omogućili [**Exchange Hibridne implementacije**](https://technet.microsoft.com/library/jj200581.aspx). Omogućite tu mogućnost ako planirate poštanski sandučići sustava Exchange u oblak i lokalne u isto vrijeme.
![Jeste li spremni za konfiguriranje Azure AD Connect](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Kada se instalacija završi, kliknite **Izlaz**.
10. Po dovršetku instalacije odjaviti i ponovno se prijavite u prije korištenja Upravitelj sinkronizacije servisa ili uređivač pravila za sinkronizaciju.

## <a name="videos"></a>Videozapisi

Videozapis o korištenju eksplicitnih instalaciju, potražite u članku:

>[AZURE.VIDEO azure-active-directory-connect-express-settings]

## <a name="next-steps"></a>Daljnji koraci
Sad kad ste instalirali Azure AD Connect možete [Potvrda instalacije i dodjeljivati licence](../active-directory-aadconnect-whats-next.md).

Dodatne informacije o tim značajkama koje su omogućena za instalaciju: [Automatska Nadogradnja](../active-directory-aadconnect-feature-automatic-upgrade.md), [Onemogućivanje slučajne briše](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)i [Povežite zdravlje Azure AD](../active-directory-aadconnect-health-sync.md).

Dodatne informacije o ovim temama uobičajenih: [Raspored i kako pokrenuti sinkronizaciju](../active-directory-aadconnectsync-feature-scheduler.md).

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Srodni dokumentacija

Tema |  
--------- | ---------
Pregled Azure AD Connect | [Integriranje sustava lokalnih identiteta sa Azure Active Directory](../active-directory-aadconnect.md)
Instalacija pomoću prilagođenih postavki | [Prilagođene instalacije Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Nadogradnja s DirSync | [Nadogradnja s alat za sinkronizaciju Azure AD (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Račune koji se koriste za instalaciju | [Dodatne informacije o Azure AD Connect računa i dozvola](active-directory-aadconnect-accounts-permissions.md)
