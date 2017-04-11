<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Lynda.com | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory Lynda.com da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lyndacom"></a>Praktični vodič: Azure Active Directory Integracija s Lynda.com
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Lynda.com.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Lynda.com klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Lynda.com će moći znak u aplikaciju na vašem Lynda.com web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Lynda.com
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-lynda-tutorial/IC781046.png "Scenarij")
##<a name="enabling-the-application-integration-for-lyndacom"></a>Omogućivanje integraciju aplikacija za Lynda.com
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Lynda.com.

###<a name="to-enable-the-application-integration-for-lyndacom-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Lynda.com, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-lynda-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-lynda-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-lynda-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-lynda-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Lynda.com**.

    ![Galerija aplikacije] (./media/active-directory-saas-lynda-tutorial/IC777524.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Lynda.com**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Lynda.com] (./media/active-directory-saas-lynda-tutorial/IC777525.png "Lynda.com")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Lynda.com sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

>[AZURE.IMPORTANT]Da biste mogli konfigurirati jedinstvenu prijavu na klijentu Lynda.com, morate najprije obratite Lynda.com za tehničku podršku da biste dobili Ova funkcija nije omogućena.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Lynda.com** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-lynda-tutorial/IC777526.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Lynda.com** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-lynda-tutorial/IC777527.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Lynda.com URL za prijavu** upišite Lynda.com klijentu URL (npr.: *https://shib.lynda.com/Shibboleth.sso/InCommon?providerId=https://sts.windows-ppe.net/6247032d-9415-403c-b72b-277e3fb6f2c8/&target= https://shib.lynda.com/InCommon*), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-lynda-tutorial/IC781047.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Lynda.com** da biste preuzeli metapodatka, kliknite **Preuzmi metapodataka**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-lynda-tutorial/IC777529.png "Konfiguriranje jedinstvenu prijavu")

5.  Slanje datoteke preuzete metapodataka na Lynda.com službi za podršku. Podršku Lynda.com ne konfiguracije jedinstvenu prijavu umjesto vas.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-lynda-tutorial/IC777530.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Postoji nijedna stavka akcija za konfiguriranje korisnika dodjeljivanje Lynda.com.  
Kada se pokuša dodijeljenog korisnika da se prijavite u Lynda.com pomoću ploče programa access, Lynda.com provjerava postoji li korisnik.  
Ako je dostupno korisnički račun još, automatski stvorena je pomoću Lynda.com.

>[AZURE.NOTE]Možete koristiti bilo koji drugi Lynda.com korisnički račun alate za stvaranje ili API-ji nudi Lynda.com dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-lyndacom-perform-the-following-steps"></a>Da biste korisnicima dodijelili Lynda.com, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Lynda.com **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-lynda-tutorial/IC777531.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-lynda-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).