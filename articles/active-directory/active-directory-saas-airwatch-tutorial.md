<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s AirWatch | Microsoft Azure" 
    description="Saznajte kako koristiti AirWatch s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Praktični vodič: Azure Active Directory Integracija s AirWatch

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i AirWatch.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na AirWatch jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili AirWatch će moći znak u aplikaciju na vašem AirWatch web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za AirWatch
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")
##<a name="enabling-the-application-integration-for-airwatch"></a>Omogućivanje integraciju aplikacija za AirWatch

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za AirWatch.

###<a name="to-enable-the-application-integration-for-airwatch-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za AirWatch, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-airwatch-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-airwatch-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-airwatch-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-airwatch-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **AirWatch**.

    ![Galerija aplikacije] (./media/active-directory-saas-airwatch-tutorial/IC791914.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **AirWatch**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti AirWatch sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **AirWatch** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-airwatch-tutorial/IC791916.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili AirWatch** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-airwatch-tutorial/IC791917.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **AirWatch na URL za prijavu** upišite URL koriste vaši korisnici da biste se prijavili aplikaciju AirWatch (npr.: "*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-airwatch-tutorial/IC791918.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na AirWatch** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-airwatch-tutorial/IC791919.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke AirWatch kao administrator.

6.  U lijevom navigacijskom oknu kliknite **računi**, a zatim **Administratori**.

    ![Administratori] (./media/active-directory-saas-airwatch-tutorial/IC791920.png "Administratori")

7.  Proširite izbornik **Postavke** , a zatim kliknite **Imeničkim servisima**.

    ![Postavke] (./media/active-directory-saas-airwatch-tutorial/IC791921.png "Postavke")

8.  Kliknite karticu **korisnika** u tekstualno polje **Osnovna DN** , upišite naziv domene, a zatim **Spremi**.

    ![Korisnik] (./media/active-directory-saas-airwatch-tutorial/IC791922.png "Korisnik")

9.  Kliknite karticu **poslužitelja** .

    ![Poslužitelj] (./media/active-directory-saas-airwatch-tutorial/IC791923.png "Poslužitelj")

10. Poduzmite sljedeće korake:

    ![Prijenos] (./media/active-directory-saas-airwatch-tutorial/IC791924.png "Prijenos")

    1.  Kao **Vrstu direktorija**, odaberite **ništa**.
    2.  Odaberite **koristi SAML za provjeru autentičnosti**.
    3.  Da biste prenijeli preuzete certifikata, kliknite **Prenesi**.

11. U odjeljku **Zatraži** poduzeti sljedeće korake:

    ![Zahtjev] (./media/active-directory-saas-airwatch-tutorial/IC791925.png "Zahtjev")

    1.  Kao **Zahtjev za povezivanje vrsta**, odaberite **OBJAVI**.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Airwatch** kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir **Identiteta davatelja jedan na URL za prijavu** .
    3.  Kao **NameID oblik**, odaberite **Adresu e-pošte**.
    4.  Kliknite **Spremi**.

12. Ponovno kliknite karticu **korisnika** .

    ![Korisnik] (./media/active-directory-saas-airwatch-tutorial/IC791926.png "Korisnik")

13. U odjeljku **atribut** poduzeti sljedeće korake:

    ![Atribut] (./media/active-directory-saas-airwatch-tutorial/IC791927.png "Atribut")

    1.  U tekstni okvir **Identifikator objekta** upišite **http://schemas.microsoft.com/identity/claims/objectidentifier**.
    2.  U tekstni okvir **korisničko ime** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    3.  U tekstni okvir **Zaslonski naziv** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    4.  U tekstni okvir **ime** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    5.  U tekstni okvir **Prezime** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    6.  U tekstni okvir **e-pošte** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Kliknite **Spremi**.

14. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-airwatch-tutorial/IC791928.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u AirWatch, oni mora biti dodijeljena u AirWatch.  
U slučaju AirWatch, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **AirWatch** kao administrator.

2.  U navigacijskom oknu s lijeve strane kliknite **računi**, a zatim **korisnika**.

    ![Korisnici] (./media/active-directory-saas-airwatch-tutorial/IC791929.png "Korisnici")

3.  Na izborniku **korisnika** kliknite **Prikaza popisa**, a zatim kliknite **Dodaj \> Add User**.

    ![Dodavanje korisnika] (./media/active-directory-saas-airwatch-tutorial/IC791930.png "Dodavanje korisnika")

4.  U dijaloškom okviru **Dodavanje / uređivanje korisnika** poduzeti sljedeće korake:

    ![Dodavanje korisnika] (./media/active-directory-saas-airwatch-tutorial/IC791931.png "Dodavanje korisnika")

    1.  Unesite **korisničko ime**, **lozinku**, **Potvrdite lozinku**, **ime**, **Prezime**, **Adresu e-pošte** valjani Azure Active Directory računa koji želite dodjele resursa u povezane tekstne okvire.
    2.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi AirWatch korisnički račun alate za stvaranje ili API-ji nudi AirWatch dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-airwatch-perform-the-following-steps"></a>Da biste korisnicima dodijelili AirWatch, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **AirWatch **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-airwatch-tutorial/IC791932.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-airwatch-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
