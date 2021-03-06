<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Kudos | Microsoft Azure" 
    description="Saznajte kako koristiti Kudos s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kudos"></a>Praktični vodič: Azure Active Directory Integracija s Kudos
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Kudos.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Kudos klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Kudos će moći znak u aplikaciju na vašem Kudos web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Kudos
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-kudos-tutorial/IC787799.png "Scenarij")
##<a name="enabling-the-application-integration-for-kudos"></a>Omogućivanje integraciju aplikacija za Kudos
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Kudos.

###<a name="to-enable-the-application-integration-for-kudos-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Kudos, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kudos-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-kudos-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-kudos-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-kudos-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Kudos**.

    ![Galerija aplikacije] (./media/active-directory-saas-kudos-tutorial/IC787800.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Kudos**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Kudos] (./media/active-directory-saas-kudos-tutorial/IC787801.png "Kudos")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Kudos sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Kudos** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-kudos-tutorial/IC787802.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Kudos** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-kudos-tutorial/IC787803.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Kudos na URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://company.kudosnow.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-kudos-tutorial/IC787804.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Kudos** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-kudos-tutorial/IC787805.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Kudos kao administrator.

6.  Na izborniku na vrhu kliknite **Postavke**.

    ![Postavke] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Postavke")

7.  Kliknite **integracije \> SSO**.

8.  U odjeljku **SSO** poduzeti sljedeće korake:

    ![Jedinstvene Prijave] (./media/active-directory-saas-kudos-tutorial/IC787807.png "Jedinstvene Prijave")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Kudos** kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir **prijavite se na URL-a** .
    2.  Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata.  

        >[AZURE.TIP]
        Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    3.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **certifikat x.509**
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Kudos** kopirajte **URL servisa za jednu Sign-Out** vrijednost i pa ih zalijepite u tekstni okvir **Odjavite ili URL** .
    5.  U tekstni okvir **Vašeg Kudos URL-a** upišite naziv svoje tvrtke.
    6.  Kliknite **Spremi**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-kudos-tutorial/IC787808.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Kudos, oni mora biti dodijeljena u Kudos.  
U slučaju Kudos, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **Kudos** kao administrator.

2.  Na izborniku na vrhu kliknite **Postavke**.

    ![Postavke] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Postavke")

3.  Kliknite **administrator korisnicima**.

4.  Kliknite karticu **korisnika** , a zatim **Dodaj korisnika**.

    ![Administrator korisnicima] (./media/active-directory-saas-kudos-tutorial/IC787809.png "Administrator korisnicima")

5.  U odjeljku **Dodavanje korisnika** , učinite sljedeće:

    ![Dodavanje korisnika] (./media/active-directory-saas-kudos-tutorial/IC787810.png "Dodavanje korisnika")

    1.  Upišite **ime**, **Prezime**, **e-pošte** i druge detalje valjani Azure Active Directory račun koji želite dodjele resursa u povezani tekstni okviri.
    2.  Kliknite **Stvori korisnika**.

>[AZURE.NOTE]Možete koristiti bilo koji drugi Kudos korisnički račun alate za stvaranje ili API-ji nudi Kudos dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-kudos-perform-the-following-steps"></a>Da biste korisnicima dodijelili Kudos, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Kudos **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-kudos-tutorial/IC787811.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-kudos-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).