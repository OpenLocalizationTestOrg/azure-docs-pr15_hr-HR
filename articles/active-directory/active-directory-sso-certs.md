<properties
    pageTitle="Kako upravljati certifikati vanjski pristup za Azure AD | Microsoft Azure"
    description="Saznajte kako prilagoditi datum isteka za vanjski pristup certifikate te kako obnoviti certifikate koji će uskoro isteći."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="managing-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Upravljanje certifikata za vanjsko jedinstvenu prijavu servisa Azure Active Directory

U ovom se članku opisuje česta pitanja vezane uz certifikata koji stvara Azure Active Directory da biste uspostavili pridruženim jedinstvenu prijavu (SSO) za SaaS aplikacija.

U ovom se članku vrijedi samo za aplikacije koje su konfigurirana za korištenje **Azure AD jedinstvenu prijavu**, kao što je prikazano u primjeru u nastavku:

![Azure AD jedinstvenu prijavu](./media/active-directory-sso-certs/fed-sso.PNG)

##<a name="how-to-customize-the-expiration-date-for-your-federation-certificate"></a>Kako prilagoditi datum isteka za potvrdu vanjski pristup

Prema zadanim postavkama certifikati su postavljeni na istječe nakon dvije godine. Možete odabrati različite rok trajanja certifikata slijedeći korake u nastavku. Uključeni snimke zaslona koristiti Salesforce radi primjer, ali ove korake možete primijeniti na bilo koju aplikaciju za vanjsko SaaS.

1. U Azure Active Directory, na stranici za brzo pokretanje aplikacije, kliknite **Konfiguriraj jedinstvenu prijavu**.

    ![Otvorite Čarobnjak za konfiguraciju jedinstvene Prijave.](./media/active-directory-sso-certs/config-sso.png)

2. Odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

3. Unesite **URL za prijavu** aplikacija, a zatim potvrdite okvir za **Konfiguriranje certifikata za vanjsko jedinstvenu prijavu**. Zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave](./media/active-directory-sso-certs/new-app-config-sso.PNG)

4. Na sljedećoj stranici odaberite **generiraj novi certifikat**, a zatim odaberite koliko želite certifikat vrijedi za. Zatim kliknite **Dalje**.

    ![Stvaranje novog certifikata](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Nakon toga kliknite **Preuzmite certifikat**. Da biste saznali kako prenijeti certifikata na određenom SaaS aplikacije, kliknite **Prikaži upute o konfiguraciji**.

    ![Preuzimanje, a zatim prijenos certifikata](./media/active-directory-sso-certs/new-app-config-app.PNG)

6. Potvrda neće biti omogućen dok potvrdite okvir za potvrdu pri dnu dijaloškog okvira, a zatim pritisnite Pošalji.

##<a name="how-to-renew-a-certificate-that-will-soon-expire"></a>Kako obnoviti certifikat koji će uskoro isteći

Obnavljanje korake prikazano u nastavku najbolje treba rezultirati bez značajan nedostupnost za korisnike. Snimke zaslona koriste u značajka sekcije Salesforce kao primjer, ali ove korake možete primijeniti na bilo koju aplikaciju za vanjsko SaaS.

1. U Azure Active Directory, na stranici za brzo pokretanje aplikacije, kliknite **Konfiguriraj jedinstvenu prijavu**.

    ![Otvorite Čarobnjak za konfiguraciju jedinstvene Prijave](./media/active-directory-sso-certs/renew-sso-button.PNG)

2. Na prvoj stranici dijaloškog okvira **Azure AD jedinstvenu prijavu** mora već biti odabran, pa kliknite **Dalje**.

3. Na drugoj stranici, potvrdite okvir za **Konfiguriranje certifikat koji je korišten za vanjsko jedinstvenu prijavu**. Zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave](./media/active-directory-sso-certs/renew-config-sso.PNG)

4. Na sljedećoj stranici odaberite **generiraj novi certifikat**, a zatim odaberite koliko želite novi certifikat vrijedi za. Zatim kliknite **Dalje**.

    ![Stvaranje novog certifikata](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Kliknite **Preuzmite certifikat**. Da biste uspješno rewnew certifikat, morate izvršiti sljedeća dva koraka:

    - Prenesite novu potvrdu zaslon aplikaciju SaaS jedan konfiguracije za prijavu. Da biste saznali kako to učiniti za određeni SaaS aplikaciju, kliknite **Prikaži upute o konfiguraciji**.

    - U Azure AD potvrdite okvir za potvrdu pri dnu dijaloškog okvira da biste omogućili novi certifikat, a zatim kliknite **Dalje** da biste poslali.

    > [AZURE.IMPORTANT] Jedinstvena prijava u aplikaciju će biti onemogućena momenta ili jedan od ta dva koraka ne završi, ali on će biti omogućene ponovno kada se dovrši drugi korak. Stoga, da biste minimizirali prekid u radu Molimo pripremite za obavljanje oba koraka u kratki vremenskog razdoblja od drugih.

    ![Preuzimanje, a zatim prijenos certifikata](./media/active-directory-sso-certs/renew-config-app.PNG)

## <a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Program access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Otklanjanje poteškoća utemeljene na SAML jedinstvenu prijavu](active-directory-saml-debugging.md)
