<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Citrix ShareFile | Microsoft Azure" 
    description="Saznajte kako koristiti Citrix ShareFile s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Praktični vodič: Azure Active Directory Integracija s Citrix ShareFile

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Citrix ShareFile.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Citrix ShareFile klijenta

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Citrix ShareFile će moći znak u aplikaciju na vašem Citrix ShareFile web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Citrix ShareFile
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Scenarij")
##<a name="enabling-the-application-integration-for-citrix-sharefile"></a>Omogućivanje integraciju aplikacija za Citrix ShareFile

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Citrix ShareFile.

###<a name="to-enable-the-application-integration-for-citrix-sharefile-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Citrix ShareFile, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Citrix ShareFile**.

    ![Galerija aplikacije] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Citrix ShareFile**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Citrix ShareFile] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Citrix ShareFile sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Citrix ShareFile** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Omogućivanje jedinstvenu prijavu] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Omogućivanje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Citrix ShareFile** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Citrix ShareFile na URL za prijavu** upišite URL pomoću sljedećeg uzorka `https://<tenant-name>.shareFile.com`, a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Citrix ShareFile** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![ConfigureSingle prijave] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle prijave")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke **Citrix ShareFile** kao administrator.

6.  Na alatnoj traci na vrhu kliknite **administrator**.

7.  U lijevom navigacijskom oknu odaberite **Konfigurirati jedinstvenu prijavu**.

    ![Administracija računa] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Administracija računa")

8.  Na na **jedinstvenu prijavu / SAML 2.0 konfiguracije** dijalošku stranicu u odjeljku **Osnovne postavke**poduzeti sljedeće korake:

    ![Jedinstvenu prijavu] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Jedinstvenu prijavu")

    1.  Kliknite **Omogući SAML**.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Citrix ShareFile** kopirajte vrijednost **ID entitet** , a zatim je zalijepite u na **vaše IDP izdavač / ID entitet** tekstni okvir.
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Citrix ShareFile** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu** .
    4.  Azure klasični portalu na **na Konfiguriraj jedinstvenu prijavu na Citrix ShareFile** dijalošku stranicu kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **URL odjavite** .
    5.  Kliknite **Promijeni** pored polja **X.509 certifikat** , a zatim prenese certifikat koji ste preuzeli s portala za Azure AD za klasični.
        ![Osnovne postavke] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Osnovne postavke")

9.  Kliknite **Spremi** na portal za upravljanje Citrix ShareFile.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u Citrix ShareFile, oni mora biti dodijeljena u Citrix ShareFile.  
U slučaju Citrix ShareFile dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **Citrix ShareFile** .

2.  Kliknite **Upravljanje korisnicima \> Upravljanje korisnicima Polazno \> + stvaranje zaposlenika**.

    ![Stvaranje zaposlenika] (./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Stvaranje zaposlenika")

3.  Unesite **e-pošte**, **ime** i **Prezime, ime** od valjani Azure AD računa koji želite dodjele resursa.

    ![Osnovne informacije] (./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Osnovne informacije")

4.  Kliknite **Dodaj korisnika**.

    >[AZURE.NOTE] Vlasnik računa AAD će primiti poruku e-pošte i slijedite vezu da biste potvrdili svoj račun prije postaje aktivna.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Citrix ShareFile korisnički račun alate za stvaranje ili API-ji nudi Citrix ShareFile dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-citrix-sharefile-perform-the-following-steps"></a>Da biste korisnicima dodijelili Citrix ShareFile, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Citrix ShareFile **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
