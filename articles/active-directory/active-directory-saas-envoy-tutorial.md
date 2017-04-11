<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Envoy | Microsoft Azure" 
    description="Saznajte kako koristiti Envoy s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-envoy"></a>Praktični vodič: Azure Active Directory Integracija s Envoy
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Envoy.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Envoy klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Envoy će moći znak u aplikaciju na vašem Envoy web-mjesto tvrtke (znak servis davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Envoy
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-envoy-tutorial/IC776759.png "Scenarij")
##<a name="enabling-the-application-integration-for-envoy"></a>Omogućivanje integraciju aplikacija za Envoy
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Envoy.

###<a name="to-enable-the-application-integration-for-envoy-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Envoy, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-envoy-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-envoy-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-envoy-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-envoy-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Envoy**.

    ![Galerija aplikacije] (./media/active-directory-saas-envoy-tutorial/IC776760.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Envoy**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Envoy] (./media/active-directory-saas-envoy-tutorial/IC776777.png "Envoy")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Envoy sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za Envoy potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Envoy** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Omogućivanje jedinstvenu prijavu] (./media/active-directory-saas-envoy-tutorial/IC776778.png "Omogućivanje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Envoy** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-envoy-tutorial/IC776779.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Envoy URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. Envoy.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-envoy-tutorial/IC776780.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Envoy** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno kao **c:\\Envoy.cer**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-envoy-tutorial/IC776781.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Envoy kao administrator.

6.  Na alatnoj traci na vrhu kliknite **Postavke**.

    ![Envoy] (./media/active-directory-saas-envoy-tutorial/IC776782.png "Envoy")

7.  Kliknite **tvrtke**.

    ![Tvrtke] (./media/active-directory-saas-envoy-tutorial/IC776783.png "Tvrtke")

8.  Kliknite **SAML**.

    ![SAML] (./media/active-directory-saas-envoy-tutorial/IC776784.png "SAML")

9.  U odjeljku Konfiguriranje **Provjere autentičnosti SAML** poduzeti sljedeće korake:

    ![Provjera autentičnosti SAML] (./media/active-directory-saas-envoy-tutorial/IC776785.png "Provjera autentičnosti SAML")

    >[AZURE.NOTE] Vrijednost za ID mjesto HQ je automatski generira aplikacije.

    1.  Kopirajte vrijednost **otisak prsta** iz izvezene potvrde, a zatim je zalijepite u tekstni okvir **otiska prsta** .  

        >[AZURE.TIP] Dodatne informacije potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Envoy** kopirajte vrijednost **SAML SSO URL** i pa ih zalijepite u tekstni okvir **URL Adresom identiteta SAML HTTP davatelja usluga** .
    3.  Kliknite **Spremi promjene**.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-envoy-tutorial/IC776786.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Postoji nijedna stavka akcija za konfiguriranje korisnika dodjeljivanje Envoy.  
Kada se pokuša dodijeljenog korisnika da se prijavite u Envoy pomoću ploče programa access, Envoy provjerava postoji li korisnik.  
Ako je dostupno korisnički račun još, automatski stvorena je pomoću Envoy.
##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-envoy-perform-the-following-steps"></a>Da biste korisnicima dodijelili Envoy, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Envoy **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-envoy-tutorial/IC776787.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-envoy-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).