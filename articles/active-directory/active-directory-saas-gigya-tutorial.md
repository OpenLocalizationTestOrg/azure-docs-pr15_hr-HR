<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Gigya | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory Gigya da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-gigya"></a>Praktični vodič: Azure Active Directory Integracija s Gigya
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Gigya.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Gigya znak na omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Gigya će moći znak u aplikaciju na vašem Gigya web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Gigya
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-gigya-tutorial/IC789512.png "Konfiguriranje jedinstvenu prijavu")
##<a name="enabling-the-application-integration-for-gigya"></a>Omogućivanje integraciju aplikacija za Gigya
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Gigya.

###<a name="to-enable-the-application-integration-for-gigya-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Gigya, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-gigya-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-gigya-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-gigya-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-gigya-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Gigya**.

    ![Galerija aplikacije] (./media/active-directory-saas-gigya-tutorial/IC789513.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Gigya**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Gigya] (./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Gigya sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Gigya** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-gigya-tutorial/IC789528.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Gigya** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-gigya-tutorial/IC789529.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Gigya na URL za prijavu** upišite URL pomoću sljedećeg uzorka "*http://company.gigya.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-gigya-tutorial/IC789530.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Gigya** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-gigya-tutorial/IC789531.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Gigya kao administrator.

6.  Idite na **Postavke \> SAML prijava**, a zatim kliknite gumb **Dodaj** .

    ![Prijava SAML] (./media/active-directory-saas-gigya-tutorial/IC789532.png "Prijava SAML")

7.  U odjeljku **Prijava SAML** poduzeti sljedeće korake:

    ![Konfiguriranje SAML] (./media/active-directory-saas-gigya-tutorial/IC789533.png "Konfiguriranje SAML")

    1.  U tekstni okvir **naziv** upišite naziv za konfiguraciju.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Gigya** kopirajte vrijednost **Izdavač URL** i pa ih zalijepite u tekstni okvir **izdavač** .
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Gigya** kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir **Jedan prijave URL servisa** .
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Gigya** kopirajte vrijednost **Oblik naziva identifikator** i pa ih zalijepite u tekstni okvir **Oblik naziva ID -a** .
    5.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.
        
        >[AZURE.TIP]Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **Certifikat X.509** .
    7.  Kliknite **Spremi postavke**.

8.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-gigya-tutorial/IC789534.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Gigya, oni mora biti dodijeljena u Gigya.  
U slučaju Gigya, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **Gigya** kao administrator.

2.  Idite na **administrator \> Upravljanje korisnicima**, a zatim kliknite **Pozovi korisnike**.

    ![Upravljanje korisnicima] (./media/active-directory-saas-gigya-tutorial/IC789535.png "Upravljanje korisnicima")

3.  U dijaloškom okviru Pozivanje korisnika poduzeti sljedeće korake:

    ![Pozivanje korisnika] (./media/active-directory-saas-gigya-tutorial/IC789536.png "Pozivanje korisnika")

    1.  U tekstni okvir **e-pošte** upišite pseudonim za e-pošte valjani Azure Active Directory račun koji želite dodjele resursa.
    2.  Kliknite **Pozovi korisnika**.
    
        >[AZURE.NOTE] Vlasnik računa Azure Active Directory primit će poruku e-pošte koja sadrži vezu da biste potvrdili račun prije postaje aktivna.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Gigya korisnički račun alate za stvaranje ili API-ji nudi Gigya dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-gigya-perform-the-following-steps"></a>Da biste korisnicima dodijelili Gigya, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Gigya **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-gigya-tutorial/IC789537.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-gigya-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).