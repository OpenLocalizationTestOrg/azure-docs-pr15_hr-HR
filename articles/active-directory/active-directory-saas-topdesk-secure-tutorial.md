<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s TOPdesk - sigurne | Microsoft Azure"
    description="Saznajte kako koristiti TOPdesk - sigurne s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!." 
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

#<a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Praktični vodič: Azure Active Directory Integracija s TOPdesk - sigurne
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i TOPdesk - sigurne.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   A TOPdesk - sigurne jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili TOPdesk - sigurno će moći znak u aplikaciju na TOPdesk – web-mjesto tvrtke sigurne (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za TOPdesk - sigurne
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Scenarij")

##<a name="enabling-the-application-integration-for-topdesk---secure"></a>Omogućivanje integraciju aplikacija za TOPdesk - sigurne
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za TOPdesk - sigurne.

###<a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za TOPdesk - sigurne, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **TOPdesk - sigurne**.

    ![Galerija aplikacije] (./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **TOPdesk - sigurne**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![TOPdesk - sigurne] (./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - sigurne")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti TOPdesk - sigurne sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za TOPdesk - sigurne potrebno da biste prenijeli datoteke ikona logotip. Da biste datoteku ikone, obratite se timu za podršku TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Prijavite se web-mjesto tvrtke **TOPdesk - sigurne** kao administrator.

2.  Na izborniku **TOPdesk** kliknite **Postavke**.

    ![Postavke] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Postavke")

3.  Kliknite **postavke za prijavu**.

    ![Postavke za prijavu] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Postavke za prijavu")

4.  Proširite izbornik **Postavke za prijavu** , a zatim kliknite **Općenito**.

    ![Općenito] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Općenito")

5.  U odjeljku **sigurne** sekciji konfiguracija **SAML prijava** poduzeti sljedeće korake:

    ![Tehničke postavke] (./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Tehničke postavke")

    1.  Kliknite **Preuzmi** da biste preuzeli datoteku javno metapodataka, a zatim ga spremiti lokalno na vašem računalu.
    2.  Otvorite datoteku metapodataka, a zatim pronađite čvor **AssertionConsumerService** .
        ![Servis potrošača pridruživanju] (./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Servis potrošača pridruživanju")
    3.  Kopirajte vrijednost **AssertionConsumerService** .  

        >[AZURE.NOTE] Trebat će vrijednost u odjeljku **Konfiguriranje URL aplikacije** u nastavku ovog praktičnog vodiča.

6.  U prozoru preglednika drugoj web, prijavite se u **Azure klasični portal** kao administrator.

7.  Na stranici za integraciju aplikacije **TOPdesk - sigurne** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Konfiguriranje jedinstvenu prijavu")

8.  Na stranici **kako biste željeli korisnika da biste se prijavili TOPdesk - sigurne** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Konfiguriranje jedinstvenu prijavu")

9.  Na stranici **Konfiguriranje URL adresa Web App** , učinite sljedeće:

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Konfiguriranje URL adresa Web App")

    1.  U tekstni okvir **TOPdesk - sigurne na URL za prijavu** upišite URL koji se koristi korisnika da biste se prijavili u TOPdesk - sigurne aplikacije (npr.: "*https://qssolutions.topdesk.net*").
    2.  U tekstni okvir **TOPdesk – javnog URL-a odgovori** zalijepite **TOPdesk - sigurne AssertionConsumerService URL** (npr.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Kliknite **Dalje**.

10. Na stranici **Konfiguracija jedinstvenu prijavu na TOPdesk - sigurne** da biste preuzeli metapodataka datoteku, kliknite **Preuzmi metapodatke**, a zatim spremite datoteku na lokalno računalo.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Konfiguriranje jedinstvenu prijavu")

11. Da biste stvorili datoteka certifikata, učinite sljedeće:

    ![Certifikat] (./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Certifikat")

    1.  Otvorite datoteku preuzete metapodataka.
    2.  Proširite čvor **RoleDescriptor** koja ima **xsi:type** od **umeće: ApplicationServiceType**.
    3.  Kopirajte vrijednost čvor **X509Certificate** .
    4.  Lokalno spremanje kopirane **X509Certificate** vrijednost na vašem računalu u datoteci.

12. Na TOPdesk - sigurno web-mjesto tvrtke, na izborniku **TOPdesk** kliknite **Postavke**.

    ![Postavke] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Postavke")

13. Kliknite **postavke za prijavu**.

    ![Postavke za prijavu] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Postavke za prijavu")

14. Proširite izbornik **Postavke za prijavu** , a zatim kliknite **Općenito**.

    ![Općenito] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Općenito")

15. U odjeljku **javne** kliknite **Dodaj**.

    ![Dodavanje] (./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Dodavanje")

16. Na stranici dijaloški okvir **Pomoćnik za konfiguriranje SAML** poduzeti sljedeće korake:

    ![Pomoćnik za konfiguriranje SAML] (./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "Pomoćnik za konfiguriranje SAML")

    1.  Da biste prenijeli datoteke preuzete metapodataka, u odjeljku **Vanjski pristup metapodataka**kliknite **Pregledaj**.
    2.  Prijenos datoteka certifikata, u odjeljku **Certifikat (RSA)**, kliknite **Pregledaj**.
    3.  Da biste prenijeli datoteke logotipa imate TOPdesk za podršku, u odjeljku **ikona logotip**, kliknite **Pregledaj**.
    4.  U tekstni okvir **korisničko ime atribut** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  U tekstni okvir **zaslonski naziv** upišite naziv za konfiguraciju.
    6.  Kliknite **Spremi**.

17. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u TOPdesk - sigurne, oni mora biti dodijeljena u TOPdesk - sigurne.  
U slučaju TOPdesk - sigurne, dodjeljivanje je ručno zadatak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se web-mjesto tvrtke **TOPdesk - sigurne** kao administrator.

2.  Na izborniku na vrhu kliknite **TOPdesk \> novo \> datoteke za podršku \> Operator**.

    ![Operator] (./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")

3.  U dijaloškom okviru **Novi Operator** poduzeti sljedeće korake:

    ![Novi Operator] (./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "Novi Operator")

    1.  Kliknite karticu Općenito.
    2.  U tekstni okvir **Prezime** odjeljka **Općenito** upišite Prezime valjani Azure Active Directory račun koji želite dodjele resursa.
    3.  Odaberite **web-mjesta** za račun u odjeljku **mjesto** .
    4.  U tekstni okvir **Korisničko ime za** **Prijavu TOPdesk** sekcije upišite ime za prijavu za korisniku.
    5.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi TOPdesk - alate za stvaranje sigurnog korisnički račun ili API-ji nudi TOPdesk - sigurne Dodjela AAD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>Da biste korisnicima dodijelili TOPdesk - sigurne, učinite sljedeće:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **TOPdesk - sigurne **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).