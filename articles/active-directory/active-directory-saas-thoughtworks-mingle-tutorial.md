<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Thoughtworks Mingle | Microsoft Azure" 
    description="Saznajte kako koristiti Thoughtworks Mingle s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Praktični vodič: Azure Active Directory Integracija s Thoughtworks Mingle
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Thoughtworks Mingle.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Klijent Thoughtworks Mingle
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Thoughtworks Mingle
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Scenarij")

##<a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>Omogućivanje integraciju aplikacija za Thoughtworks Mingle
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Thoughtworks Mingle.

###<a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Thoughtworks Mingle, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **thoughtworks mingle**.

    ![Galerija aplikacije] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Thoughtworks Mingle**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Thoughtworks Mingle] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks Mingle")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Thoughtworks Mingle sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Ovaj postupak, dio su potrebne da biste prenijeli certifikat na Thoughtworks Mingle.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za **Thoughtworks Mingle **aplikacije Integracija kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Thoughtworks Mingle** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **URL klijentu Mingle Thoughtworks** upišite URL pomoću sljedećeg uzorka "*http://company.mingle.thoughtworks.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Thoughtworks Mingle** kliknite preuzimanje metapodataka i spremite je na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Konfiguriranje jedinstvenu prijavu")

5.  Prijavite se na web-mjesto tvrtke **Thoughtworks Mingle** kao administrator.

6.  Kliknite karticu **Administracija** , a zatim **SSO konfiguracije**.

    ![Konfiguracija SSO] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "Konfiguracija SSO")

7.  U odjeljku **SSO Config** poduzeti sljedeće korake:

    ![Konfiguracija SSO] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "Konfiguracija SSO")

    1.  Da biste prenijeli datoteku metapodataka, kliknite **Odabir datoteke**.
    2.  Kliknite **Spremi promjene**.

8.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Za AAD da korisnici mogu se prijaviti, oni mora biti dodijeljena da Thoughtworks Mingle aplikacije pomoću njihova korisničkog imena Azure Active Directory.  
U slučaju Thoughtworks Mingle, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke Thoughtworks Mingle kao administrator.

2.  Kliknite **profil**.

    ![Prvi projekta] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Prvi projekta")

3.  Kliknite karticu **Administracija** , a zatim kliknite **korisnici**.

    ![Korisnici] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Korisnici")

4.  Kliknite **novi korisnik**.

    ![Novi korisnik] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Novi korisnik")

5.  Na stranici dijaloški okvir **Novi korisnik** , učinite sljedeće:

    ![Novi korisnik] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Novi korisnik")

    1.  Upišite **ime za prijavu**, **zaslonsko ime**, **lozinku za odabir**, **Potvrda lozinke** valjani AAD računa želite dodjele resursa u povezani tekstni okviri.
    2.  Kao **Vrsta korisnika**, odaberite **cijeli korisnika**.
    3.  Kliknite **Stvori ovaj profil**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Thoughtworks Mingle korisnički račun alate za stvaranje ili API-ji nudi Thoughtworks Mingle dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>Da biste dodijelili Thoughtworks Mingle korisnika, učinite sljedeće:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za **Thoughtworks Mingle** aplikacije Integracija kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
