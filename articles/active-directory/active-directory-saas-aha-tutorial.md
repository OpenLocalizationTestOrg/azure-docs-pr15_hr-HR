<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s programom Aha! | Microsoft Azure" 
    description="Saznajte kako koristiti Aha! s Azure Active Directory da biste omogućili jedinstvenu prijavu automatski dodjele resursa i još mnogo!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-aha"></a>Praktični vodič: Azure Active Directory Integracija s programom Aha!

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Aha!  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Programa Evo! jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Aha! moći ćete znak u aplikaciju na vašem Aha! web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Aha!
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-aha-tutorial/IC798944.png "Scenarij")
##<a name="enabling-the-application-integration-for-aha"></a>Omogućivanje integraciju aplikacija za Aha!

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Aha!.

###<a name="to-enable-the-application-integration-for-aha-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Aha!, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-aha-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-aha-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-aha-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-aha-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Aha!**.

    ![Galerija aplikacije] (./media/active-directory-saas-aha-tutorial/IC798945.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Aha!**, a zatim kliknite **dovrši** da biste dodali aplikaciju.

    ![Evo!] (./media/active-directory-saas-aha-tutorial/IC802746.png "Evo!")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Aha! s računom njihove Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na na **Aha!** Integracija aplikacije kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-aha-tutorial/IC798946.png "Konfiguriranje jedinstvenu prijavu")

2.  Na na **kako biste željeli korisnika da biste se prijavili Aha!** stranice, odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-aha-tutorial/IC798947.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u **Aha! Prijavite se na URL** tekstni okvir, upišite URL koriste vaši korisnici za prijave za vaše Aha! Aplikacije (npr.: "*https://company.aha.io/session/new*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-aha-tutorial/IC798948.png "Konfiguriranje URL adresa Web App")

4.  Na na **Konfiguriranje jedinstvenu prijavu na Aha!** stranica za preuzimanje metapodataka datoteku, kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-aha-tutorial/IC798949.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u vašem Aha! web-mjesto tvrtke kao administrator.

6.  Na izborniku na vrhu kliknite **Postavke**.

    ![Postavke] (./media/active-directory-saas-aha-tutorial/IC798950.png "Postavke")

7.  Kliknite **račun**.

    ![Profil] (./media/active-directory-saas-aha-tutorial/IC798951.png "Profil")

8.  Kliknite **Sigurnost i jedinstvenu prijavu**.

    ![Sigurnost i jedinstvenu prijavu] (./media/active-directory-saas-aha-tutorial/IC798952.png "Sigurnost i jedinstvenu prijavu")

9.  U odjeljku za **Jedinstvenu prijavu** , kao **Davatelja identiteta**, odaberite **SAML2.0**.

    ![Sigurnost i jedinstvenu prijavu] (./media/active-directory-saas-aha-tutorial/IC798953.png "Sigurnost i jedinstvenu prijavu")

10. Konfiguriranje stranice **Jedinstvenu prijavu** poduzeti sljedeće korake:

    ![Jedinstvenu prijavu] (./media/active-directory-saas-aha-tutorial/IC798954.png "Jedinstvenu prijavu")

    1.  U tekstni okvir **naziv** upišite naziv za konfiguraciju.
    2.  **Konfiguriranje pomoću**, odaberite **Datoteku metapodataka**.
    3.  Da biste prenijeli metapodataka preuzetu datoteku, kliknite **Pregledaj**.
    4.  Kliknite **Ažuriraj**.

11. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-aha-tutorial/IC798955.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u Aha!, mora biti dodijeljena u Aha!.  
U slučaju Aha!, dodjeljivanje je automatizirana zadatak.  
Postoji nijedna stavka akcija za vas.
  
Korisnicima se automatski stvaraju po potrebi tijekom prvog jedan prijave pokušaj.

>[AZURE.NOTE] Možete koristiti druge Aha! alate za stvaranje korisnički račun ili API-ji nudi Aha! Dodjela AAD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-aha-perform-the-following-steps"></a>Da biste korisnicima dodijelili Aha!, učinite sljedeće:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na na **Evo!** integraciju aplikacija kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-aha-tutorial/IC798956.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-aha-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
