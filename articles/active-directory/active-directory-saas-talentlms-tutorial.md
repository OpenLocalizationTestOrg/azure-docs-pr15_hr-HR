<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s TalentLMS | Microsoft Azure" 
    description="Saznajte kako koristiti TalentLMS s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Praktični vodič: Azure Active Directory Integracija s TalentLMS
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i TalentLMS.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   TalentLMS klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili TalentLMS će moći znak u aplikaciju na vašem TalentLMS web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za TalentLMS
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-talentlms-tutorial/IC777289.png "Scenarij")

##<a name="enabling-the-application-integration-for-talentlms"></a>Omogućivanje integraciju aplikacija za TalentLMS
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za TalentLMS.

###<a name="to-enable-the-application-integration-for-talentlms-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za TalentLMS, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-talentlms-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-talentlms-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-talentlms-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-talentlms-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **TalentLMS**.

    ![Galerija aplikacije] (./media/active-directory-saas-talentlms-tutorial/IC777290.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **TalentLMS**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![TalentLMS] (./media/active-directory-saas-talentlms-tutorial/IC777291.png "TalentLMS")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti TalentLMS sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol. .  
Konfiguriranje jedinstvene prijave za TalentLMS potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **TalentLMS** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-talentlms-tutorial/IC777292.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili TalentLMS** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-talentlms-tutorial/IC777293.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **TalentLMS URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. TalentLMSapp.com*", a zatim kliknite **Dalje**.

    ![Prijavite se na URL-a] (./media/active-directory-saas-talentlms-tutorial/IC777294.png "Prijavite se na URL-a")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na TalentLMS** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno kao **c:\\TalentLMS.cer**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-talentlms-tutorial/IC777295.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke TalentLMS kao administrator.

6.  U odjeljku **računa i postavke** kliknite karticu **korisnika** .

    ![Računa i postavke] (./media/active-directory-saas-talentlms-tutorial/IC777296.png "Računa i postavke")

7.  Kliknite **Jedinstvenu prijavu (SSO)**

8.  U odjeljku jedinstvenu prijavu poduzeti sljedeće korake:

    ![Jedinstvenu prijavu] (./media/active-directory-saas-talentlms-tutorial/IC777297.png "Jedinstvenu prijavu")

    1.  Na popisu **Vrsta Integracija SSO** odaberite **SAML 2.0**.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na TalentLMS** kopirajte vrijednost **ID davatelja identiteta** i pa ih zalijepite u tekstni okvir **davatelja identiteta (IdP)** .
    3.  Kopirajte vrijednost **otisak prsta** iz izvezene potvrde i pa ih zalijepite u tekstni okvir za **Potvrdu otiska prsta** .

        >[AZURE.TIP] Dodatne informacije potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na TalentLMS** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL daljinskog prijavu** .
    5.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na TalentLMS** kopirajte **URL daljinskog odjavite** vrijednost, a zatim je zalijepite u tekstni okvir **URL daljinskog sign-out** .
    6.  U tekstni okvir **TargetedID** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**
    7.  U tekstni okvir **ime** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    8.  U tekstni okvir **Prezime** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    9.  U tekstni okvir **e-pošte** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**
    10. Kliknite **Spremi**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-talentlms-tutorial/IC777298.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u TalentLMS, oni mora biti dodijeljena u TalentLMS.  
U slučaju TalentLMS, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **TalentLMS** .

2.  Kliknite **korisnici**, a zatim kliknite **Dodaj korisnika**.

3.  Na stranici dijaloški okvir **Dodavanje korisnika** , poduzmite sljedeće korake:

    ![Dodavanje korisnika] (./media/active-directory-saas-talentlms-tutorial/IC777299.png "Dodavanje korisnika")

    1.  Upišite vrijednosti povezanog atributa Azure AD korisničkog računa u sljedeći tekstni okviri: **ime**, **Prezime**, **adresu e-pošte**.
    2.  Kliknite **Dodaj korisnika**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi TalentLMS korisnički račun alate za stvaranje ili API-ji nudi TalentLMS dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-talentlms-perform-the-following-steps"></a>Da biste korisnicima dodijelili TalentLMS, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **TalentLMS **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-talentlms-tutorial/IC777300.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-talentlms-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).