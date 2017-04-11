<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Zendesk | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory Zendesk da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!." 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Praktični vodič: Azure Active Directory Integracija s Zendesk
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Zendesk.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Zendesk klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Zendesk će moći znak u aplikaciju na vašem Zendesk web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Zendesk
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-zendesk-tutorial/IC773083.png "Scenarij")

##<a name="enabling-the-application-integration-for-zendesk"></a>Omogućivanje integraciju aplikacija za Zendesk
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Zendesk.

###<a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Zendesk, poduzmite sljedeće korake:

1.  Na portalu za upravljanje Azure, u lijevom navigacijskom oknu kliknite **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zendesk-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-zendesk-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-zendesk-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-zendesk-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Zendesk**.

    ![Galerija aplikacije] (./media/active-directory-saas-zendesk-tutorial/IC773084.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Zendesk**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Zendesk] (./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Zendesk sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za Zendesk potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Na portalu Azure AD na stranici za integraciju aplikacije **Zendesk** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Jedinstvenu prijavu] (./media/active-directory-saas-zendesk-tutorial/IC773086.png "Jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Zendesk** , odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zendesk-tutorial/IC773087.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , učinite sljedeće:

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-zendesk-tutorial/IC773088.png "Konfiguriranje URL adresa web app")
  
    na. U tekstni okvir **Zendesk URL za prijavu** upišite URL pomoću sljedećeg uzorka:`https://<tenant-name>.zendesk.com`

    b. Kliknite **Dalje**.



4.  Na stranici **Konfiguracija jedinstvenu prijavu na Zendesk** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem compiter.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zendesk-tutorial/IC777534.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Zendesk kao administrator.

6.  Kliknite **administrator**.

7.  U lijevom navigacijskom oknu kliknite **Postavke**, a zatim kliknite **Sigurnost**.

    ![Sigurnost] (./media/active-directory-saas-zendesk-tutorial/IC773089.png "Sigurnost")

8.  Na stranici **Sigurnost** kliknite karticu **Administracija i agenata** .

9.  Odaberite **jedan prijavu (SSO) i SAML**, a zatim odaberite **SAML**.

10. Na portalu Azure AD na stranici **Konfiguracija jedinstvenu prijavu na Zendesk** , kopirajte **SAML SSO URL** vrijednost i pa ih zalijepite u tekstni okvir **SAML SSO URL** .

11. Na portalu Azure AD na stranici **Konfiguracija jedinstvenu prijavu na Zendesk** , kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **URL daljinskog odjavite** .

    ![Jedinstvenu prijavu] (./media/active-directory-saas-zendesk-tutorial/IC773090.png "Jedinstvenu prijavu")

12. Kopirajte vrijednost **otisak prsta** iz izvezene potvrde i pa ih zalijepite u tekstni okvir za **Potvrdu otiska prsta** .

    >[AZURE.TIP] Dodatne informacije potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

13. Kliknite **Spremi**.

14. Na portalu za Azure AD odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zendesk-tutorial/IC773093.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u **Zendesk**, oni mora biti dodijeljena u **Zendesk**.  
U slučaju **Zendesk**dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>Dodjela korisničkog računa radi Zendesk, učinite sljedeće:

1.  Prijavite se u klijent **Zendesk** .

2.  Odaberite karticu **Popis korisnika** .

3.  Odaberite karticu **korisnika** , a zatim kliknite **Dodaj**.

    ![Dodavanje korisnika] (./media/active-directory-saas-zendesk-tutorial/IC773632.png "Dodavanje korisnika")

4.  Upišite adresu e-pošte postojeći račun Azure AD želite dodjele resursa, a zatim kliknite **Spremi**.

    ![Novi korisnik] (./media/active-directory-saas-zendesk-tutorial/IC773633.png "Novi korisnik")

>[AZURE.NOTE] Možete koristiti bilo koji drugi Zendesk korisnički račun alate za stvaranje ili API-ji nudi Zendesk dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>Da biste korisnicima dodijelili Zendesk, poduzmite sljedeće korake:

1.  Na portalu za Azure AD stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Zendesk **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-zendesk-tutorial/IC773094.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-zendesk-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
