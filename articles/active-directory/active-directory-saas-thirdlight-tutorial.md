<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Thirdlight | Microsoft Azure" 
    description="Saznajte kako koristiti Thirdlight s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Praktični vodič: Azure Active Directory Integracija s Thirdlight
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Thirdlight.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Thirdlight jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Thirdlight će moći znak u aplikaciju na vašem Thirdlight web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Thirdlight
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-thirdlight-tutorial/IC805836.png "Scenarij")

##<a name="enabling-the-application-integration-for-thirdlight"></a>Omogućivanje integraciju aplikacija za Thirdlight
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Thirdlight.

###<a name="to-enable-the-application-integration-for-thirdlight-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Thirdlight, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thirdlight-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-thirdlight-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-thirdlight-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-thirdlight-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Thirdlight**.

    ![Galerija aplikacije] (./media/active-directory-saas-thirdlight-tutorial/IC805837.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Thirdlight**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![ThirdLight] (./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Thirdlight sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za Thirdlight potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Thirdlight** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-thirdlight-tutorial/IC805839.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Thirdlight** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-thirdlight-tutorial/IC805840.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Thirdlight URL za prijavu** upišite URL koriste vaši korisnici da biste se prijavili aplikaciju Thirdlight (npr.: "*http://azuresso2.thirdlight.com/*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-thirdlight-tutorial/IC805841.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Thirdlight** da biste preuzeli metapodatka, kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-thirdlight-tutorial/IC805842.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Thirdlight kao administrator.

6.  Idite na **konfiguracije \> administracijom sustava**, a zatim kliknite **SAML2**.

    ![Administriranje sustava] (./media/active-directory-saas-thirdlight-tutorial/IC805843.png "Administriranje sustava")

7.  U odjeljku Konfiguriranje SAML2 poduzeti sljedeće korake:

    ![SAML jedinstvenu prijavu] (./media/active-directory-saas-thirdlight-tutorial/IC805844.png "SAML jedinstvenu prijavu")

    1.  Odaberite **Omogući SAML2 jedinstvenu prijavu**.
    2.  Kao **izvor za IdP metapodatke**, odaberite **Učitavanja IdP metapodataka iz XML-a**.
    3.  Otvorite datoteku preuzete metapodataka, kopirajte sadržaj, a pa ih zalijepite u tekstni okvir **XML metapodatke IdP** .
    4.  Kliknite **Spremi SAML2 postavke**.

8.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-thirdlight-tutorial/IC805845.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Thirdlight, oni mora biti dodijeljena u Thirdlight.  
U slučaju Thirdlight, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke **Thirdlight** kao administrator.

2.  Idite na karticu **korisnika** .

3.  Odaberite **korisnici i grupe**.

4.  Kliknite gumb **Dodaj novog korisnika** .

5.  Unesite **korisničko ime, naziv ili opis, e-pošte, odaberite gotovu ili grupe nove članove** valjani AAD računa koji želite za dodjelu resursa.

6.  Kliknite **Stvori**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Thirdlight korisnički račun alate za stvaranje ili API-ji nudi Thirdlight dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-thirdlight-perform-the-following-steps"></a>Da biste korisnicima dodijelili Thirdlight, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Thirdlight **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-thirdlight-tutorial/IC805846.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-thirdlight-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).