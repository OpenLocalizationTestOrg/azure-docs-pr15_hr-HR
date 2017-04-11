<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Learningpool | Microsoft Azure" 
    description="Saznajte kako koristiti Learningpool s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-learningpool"></a>Praktični vodič: Azure Active Directory Integracija s Learningpool
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Learningpool.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Learningpool jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Learningpool će moći znak u aplikaciju na vašem Learningpool web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploče](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Learningpool
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-learningpool-tutorial/IC791166.png "Scenarij")
##<a name="enabling-the-application-integration-for-learningpool"></a>Omogućivanje integraciju aplikacija za Learningpool
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Learningpool.

###<a name="to-enable-the-application-integration-for-learningpool-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Learningpool, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-learningpool-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-learningpool-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-learningpool-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-learningpool-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Learningpool**.

    ![Galerija aplikacije] (./media/active-directory-saas-learningpool-tutorial/IC795073.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Learningpool**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Learningpool] (./media/active-directory-saas-learningpool-tutorial/IC809577.png "Learningpool")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Learningpool sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.
  
Aplikacija Learningpool očekuje SAML assertions u određenom obliku, koje je potrebno da biste dodali prilagođeni atribut mapiranja konfiguraciju **saml tokena atribute** .  
Sljedeća slika prikazuje primjer za to.

![SAML tokena atributa] (./media/active-directory-saas-learningpool-tutorial/IC795074.png "SAML tokena atributa")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici integraciju aplikacije **Learningpool** na izborniku na vrhu kliknite **atribute** da biste otvorili dijaloški okvir **SAML tokena atribute** .

    ![Atributi] (./media/active-directory-saas-learningpool-tutorial/IC795075.png "Atributi")

2.  Da biste dodali mapiranja obavezan atribut, poduzmite sljedeće korake:

    ###  

  	|Naziv atributa                |Vrijednost atributa            |
  	|------------------------------|---------------------------|

     urn: oid:1.2.840.113556.1.4.221 | User.userprincipalname
  	|-------------------------------|--------------------------|  
     urn: oid:2.5.4.42|User.givenname   
  	|urn: oid:0.9.2342.19200300.100.1.3|User.Mail
  	|urn: oid:2.5.4.4|User.surname

    1.  Za svaki redak podatke iz gornje tablice, kliknite **Dodaj korisnika atribut**.
    2.  U tekstni okvir **Naziv atributa** upišite naziv atributa koji se prikazuju za tom retku.
    3.  Na popisu **Atributa vrijednosti** odaberite vrijednost atributa koji se prikazuju za tom retku.
    4.  Kliknite **dovrši**.

3.  Kliknite **Primijeni promjene**.

4.  U pregledniku kliknite **natrag** da biste ponovno otvorite dijaloški okvir za **Brzo pokretanje** .

5.  Kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje Singel prijave] (./media/active-directory-saas-learningpool-tutorial/IC795076.png "Konfiguriranje Singel prijave")

6.  Na stranici **kako biste željeli korisnika da biste se prijavili Learningpool** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-learningpool-tutorial/IC795077.png "Konfiguriranje jedinstvenu prijavu")

7.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Learningpool na URL za prijavu** upišite URL koji se koristi korisnika da biste se prijavili aplikaciju Learningpool (npr.: https://parliament.preview.learningpool.com/auth/shibboleth/index.php), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-learningpool-tutorial/IC795078.png "Konfiguriranje URL adresa Web App")

8.  Na stranici **Konfiguracija jedinstvenu prijavu na Learningpool** da biste preuzeli metapodatka, kliknite **Preuzmi metapodataka**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-learningpool-tutorial/IC795079.png "Konfiguriranje jedinstvenu prijavu")

9.  Prosljeđivanje tu datoteku metapodataka službi za podršku Learningpool.

    >[AZURE.NOTE]Jedinstvenu prijavu mora biti omogućen od tima za podršku Learningpool.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-learningpool-tutorial/IC795080.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Learningpool, oni mora biti dodijeljena u Learningpool.
  
Postoji nijedna stavka akcija za konfiguriranje korisnika dodjeljivanje Learningpool.  
Korisnici moraju stvoriti tako da Learningpool za podršku.

>[AZURE.NOTE]Možete koristiti bilo koji drugi Learningpool korisnički račun alate za stvaranje ili API-ji nudi Learningpool dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-learningpool-perform-the-following-steps"></a>Da biste korisnicima dodijelili Learningpool, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Learningpool **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-learningpool-tutorial/IC795081.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-learningpool-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).