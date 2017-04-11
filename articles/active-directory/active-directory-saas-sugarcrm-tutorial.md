<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija Integracija s SugarCRM | Microsoft Azure" 
    description="Saznajte kako koristiti SugarCRM s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-sugarcrm"></a>Praktični vodič: Azure Active Directory Integracija Integracija s SugarCRM
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Šećer CRM.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   U CRM Šećer jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Šećer CRM će moći znak u aplikaciju na vašem Šećer CRM web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Šećer CRM
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-sugarcrm-tutorial/IC795881.png "Scenarij")

##<a name="enabling-the-application-integration-for-sugar-crm"></a>Omogućivanje integraciju aplikacija za Šećer CRM
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Šećer CRM.

###<a name="to-enable-the-application-integration-for-sugar-crm-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Šećer CRM, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sugarcrm-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-sugarcrm-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-sugarcrm-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-sugarcrm-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Šećer CRM**.

    ![Galerija aplikacije] (./media/active-directory-saas-sugarcrm-tutorial/IC795882.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Šećer CRM**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Šećer CRM] (./media/active-directory-saas-sugarcrm-tutorial/IC795883.png "Šećer CRM")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Šećer CRM sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za prijenos certifikata kodirana osnovni 64 Šećer CRM klijentu.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **CRM Šećer** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sugarcrm-tutorial/IC795884.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Šećer CRM** , odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sugarcrm-tutorial/IC795885.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Šećer CRM na URL za prijavu** upišite URL koji se koristi korisnika za prijavu u aplikaciju Šećer CRM (npr.: "*http://company.sugarondemand.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-sugarcrm-tutorial/IC795886.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na CRM Šećer** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**i spremanje datoteka certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sugarcrm-tutorial/IC796918.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Šećer CRM kao administrator.

6.  Idite na **administrator**.

    ![Administrator] (./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Administrator")

7.  U odjeljku **Administracija** kliknite **Upravljanje lozinkom**.

    ![Administracija] (./media/active-directory-saas-sugarcrm-tutorial/IC795889.png "Administracija")

8.  Odaberite **Omogući provjeru autentičnosti SAML**.

    ![Administracija] (./media/active-directory-saas-sugarcrm-tutorial/IC795890.png "Administracija")

9.  U odjeljku **Provjera autentičnosti SAML** poduzeti sljedeće korake:

    ![Provjera autentičnosti SAML] (./media/active-directory-saas-sugarcrm-tutorial/IC795891.png "Provjera autentičnosti SAML")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na CRM Šećer** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu** .
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na CRM Šećer** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL a SPORIM** .
    3.  Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata.

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otvorite certifikata kodirana base-64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim zalijepite cijelu certifikata u **Certifikat X.509** tekstni okvir.
    5.  Kliknite **Spremi**.

10. Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na CRM Šećer** odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sugarcrm-tutorial/IC796919.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Šećer CRM, oni mora biti dodijeljena Šećer CRM.  
U slučaju CRM Šećer dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **Šećer CRM** kao administrator.

2.  Idite na **administrator**.

    ![Administrator] (./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Administrator")

3.  U odjeljku **Administracija** kliknite **Upravljanje korisnicima**.

    ![Administracija] (./media/active-directory-saas-sugarcrm-tutorial/IC795893.png "Administracija")

4.  Idite na **korisnika \> stvaranje novog korisnika**.

    ![Stvaranje novog korisnika] (./media/active-directory-saas-sugarcrm-tutorial/IC795894.png "Stvaranje novog korisnika")

5.  Na kartici **Korisničkog profila** , poduzmite sljedeće korake:

    ![Novi korisnik] (./media/active-directory-saas-sugarcrm-tutorial/IC795895.png "Novi korisnik")

    1.  Upišite korisničko ime, posljednje ime i e-pošte adresa valjanog korisnika Azure Active Directory u povezane tekstne okvire.

6.  Kao **Status**odaberite **aktivni**.

7.  Na kartici lozinke, učinite sljedeće:

    ![Novi korisnik] (./media/active-directory-saas-sugarcrm-tutorial/IC795896.png "Novi korisnik")

    1.  Unesite lozinku u povezani tekstni okvir.
    2.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi CRM Šećer korisnički račun alate za stvaranje ili API-ji nudi Šećer CRM dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-sugar-crm-perform-the-following-steps"></a>Da biste korisnicima dodijelili Šećer CRM, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **CRM Šećer** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-sugarcrm-tutorial/IC795897.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-sugarcrm-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).