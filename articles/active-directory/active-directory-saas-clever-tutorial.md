<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Clever | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory Clever da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clever"></a>Praktični vodič: Azure Active Directory Integracija s Clever

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Clever. Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Dobra klijenta

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Clever će moći znak u aplikaciju na vašem dobra web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Clever
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-clever-tutorial/IC798977.png "Scenarij")
##<a name="enabling-the-application-integration-for-clever"></a>Omogućivanje integraciju aplikacija za Clever

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Clever.

###<a name="to-enable-the-application-integration-for-clever-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Clever, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clever-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-clever-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-clever-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-clever-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Clever**.

    ![Galerija aplikacije] (./media/active-directory-saas-clever-tutorial/IC798978.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Clever**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Dobra] (./media/active-directory-saas-clever-tutorial/IC798979.png "Dobra")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Clever sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Dobra aplikacija očekuje SAML assertions u određenom obliku, koje je potrebno da biste dodali prilagođeni atribut mapiranja konfiguraciju **saml tokena atribute** .  
Sljedeća slika prikazuje primjer za to.

![Atributi] (./media/active-directory-saas-clever-tutorial/IC798980.png "Atributi")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Clever** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-clever-tutorial/IC784682.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Clever** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-clever-tutorial/IC798981.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Dobra na URL za prijavu** upišite URL koji se koristi korisnika za prijavu u aplikaciju dobra (npr.: *https://clever.com/in/azsandbox*), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-clever-tutorial/IC798982.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Clever** da biste preuzeli metapodatka, kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-clever-tutorial/IC798983.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke dobra kao administrator.

6.  Na alatnoj traci kliknite **Trenutno prijava**.

    ![Trenutno prijava] (./media/active-directory-saas-clever-tutorial/IC798984.png "Trenutno prijava")

7.  Na stranici **Prijava izravne** poduzeti sljedeće korake:

    ![Trenutno prijava] (./media/active-directory-saas-clever-tutorial/IC798985.png "Trenutno prijava")

    1.  Unesite **URL za prijavu**.  

        >[AZURE.NOTE] **URL za prijavu** nije prilagođene vrijednosti.
Stvarna vrijednost možete doći s dobra podršku.

    2.  Kao **Identitet sustava**odaberite **ADFS**.
    3.  Kliknite **Spremi**.

8.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-clever-tutorial/IC798986.png "Konfiguriranje jedinstvenu prijavu")

9.  Na izborniku na vrhu kliknite **atribute** da biste otvorili dijaloški okvir **SAML tokena atribute** .

    ![Atributi] (./media/active-directory-saas-clever-tutorial/IC795920.png "Atributi")

10. Da biste dodali mapiranja obavezan atribut, poduzmite sljedeće korake:

    ![saml tokena atributa] (./media/active-directory-saas-clever-tutorial/IC795921.png "saml tokena atributa")

  	|Naziv atributa|Vrijednost atributa|
  	|---|---|
  	|clever.student.credentials.district\_korisničko ime|User.userprincipalname|

    1.  Za svaki redak podatke iz gornje tablice, kliknite **Dodaj korisnika atribut**.
    2.  U tekstni okvir **Naziv atributa** upišite naziv atributa koji se prikazuju za tom retku.
    3.  U tekstni okvir **Atributa vrijednosti** odaberite vrijednost atributa koji se prikazuju za tom retku.
    4.  Kliknite **dovrši**.

11. Kliknite **Primijeni promjene**.

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u Clever, oni mora biti dodijeljena u Clever.  
U slučaju Clever, dodjeljivanje je ručno zadatak koji je potrebno izvršiti dobra podršku.

>[AZURE.NOTE] Možete koristiti bilo koji drugi dobra korisnički račun alate za stvaranje ili API-ji nudi Clever dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-clever-perform-the-following-steps"></a>Da biste korisnicima dodijelili Clever, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Clever **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-clever-tutorial/IC798987.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-clever-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
