<properties 
    pageTitle="Praktični vodič: Azure Active Directory integracije sa softverom Iglu | Microsoft Azure" 
    description="Saznajte kako koristiti softver Iglu s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="10/20/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Praktični vodič: Azure Active Directory integracije sa softverom Iglu
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Iglu softver.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Jedan znak [Iglu softver](http://www.igloosoftware.com/) omogućeno pretplate na
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Iglu softver će moći znak u aplikaciju na vašem Iglu softver tvrtke (znak servisa davatelja pokrenut na), ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Iglu softver
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Scenarij")
##<a name="enabling-the-application-integration-for-igloo-software"></a>Omogućivanje integraciju aplikacija za Iglu softver
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Iglu softvera.

###<a name="to-enable-the-application-integration-for-igloo-software-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Iglu softver, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-igloo-software-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Iglu softvera**.

    ![Galerija aplikacije] (./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Iglu softver**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Iglu] (./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Iglu")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Iglu softver sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za prijenos certifikata kodirana osnovni 64 u klijentu za središnju radnu površinu.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Softver Iglu** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Iglu softver** , odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Microsoft Azure AD jedinstvene prijave] (./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure AD jedinstvene prijave")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Iglu softver URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://company.igloocommunities.com/?signin*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-igloo-software-tutorial/IC773625.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Iglu softver** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**i spremanje datoteka certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Iglu softver kao administrator.

6.  Otvorite **upravljačku ploču**.

    ![Upravljačka ploča] (./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Upravljačka ploča")

7.  Na kartici **članstvo** kliknite **Prijavite se u postavke**.

    ![Prijavite se u odjeljku postavke] (./media/active-directory-saas-igloo-software-tutorial/IC783968.png "Prijavite se u odjeljku postavke")

8.  U odjeljku Konfiguriranje SAML kliknite **Konfiguriranje provjere autentičnosti SAML**.

    ![Konfiguriranje SAML] (./media/active-directory-saas-igloo-software-tutorial/IC783969.png "Konfiguriranje SAML")

9.  U odjeljku **Općenito konfiguracije** poduzeti sljedeće korake:

    ![Konfiguriranje općenitih] (./media/active-directory-saas-igloo-software-tutorial/IC783970.png "Konfiguriranje općenitih")

    1.  U tekstni okvir **Naziv veze** upišite prilagođeni naziv za konfiguraciju.
    2.  Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na Iglu softver** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL IdP prijava** .
    3.  Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na softver Iglu** kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **URL odjavite IdP** .
    4.  Kao **odgovor odjavite i zahtjev HTTP vrsta**, odaberite **OBJAVI**.
    5.  Stvaranje tekstne datoteke iz preuzete certifikata.
        
        >[AZURE.TIP]Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    6.  Uklanjanje u prvom retku i zadnji redak iz verzije datoteke teksta za potvrdu, kopirajte preostali tekst certifikat i pa ih zalijepite u tekstni okvir **Javni certifikat** .

10. U okvir **odgovor i konfiguriranje provjere autentičnosti**, učinite sljedeće:

    ![Odgovor i konfiguriranje provjere autentičnosti] (./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Odgovor i konfiguriranje provjere autentičnosti")

    1.  Kao **Davatelja identiteta**, odaberite **Microsoft ADFS**.
    2.  Kao **Identifikator vrsta**, odaberite **Adresu e-pošte**.
    3.  U tekstni okvir **Atribut e-pošte** upišite **Adresa**.
    4.  U tekstni okvir **Atribut ime** upišite **givenname**.
    5.  U tekstni okvir **Posljednje atribut naziv** upišite **Prezime**.

11. Preform sljedeće korake da biste dovršili konfiguraciju:

    ![Stvaranje korisničkih na prijava] (./media/active-directory-saas-igloo-software-tutorial/IC783972.png "Stvaranje korisničkih na prijava")

    1.  Kao **Stvaranje korisničkih na prijava**, odaberite **Stvori novog korisnika s web-mjesta kada se prijaviti**.
    2.  Kao **prijavite se u odjeljku postavke**, odaberite **gumb koristi SAML na zaslonu "Prijavite se u"**.
    3.  Kliknite **Spremi**.

12. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Postoji nijedna stavka akcija za konfiguriranje korisnika dodjele resursa Iglu softvera.  
Kada se pokuša dodijeljenog korisnika da se prijavite u Iglu softver pomoću ploče programa access, softver Iglu provjerava postoji li korisnik.  
Ako je dostupno korisnički račun još, automatski stvara Iglu softvera.
##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-igloo-software-perform-the-following-steps"></a>Da biste korisnicima dodijelili Iglu softver, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Softver Iglu **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).