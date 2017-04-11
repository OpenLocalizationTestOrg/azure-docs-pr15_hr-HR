<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Veracode | Microsoft Azure" 
    description="Saznajte kako koristiti Veracode s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-veracode"></a>Praktični vodič: Azure Active Directory Integracija s Veracode
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Veracode. Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Veracode jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Veracode će moći jednom prijava u aplikacije pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Veracode
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-veracode-tutorial/IC802903.png "Scenarij")

##<a name="enabling-the-application-integration-for-veracode"></a>Omogućivanje integraciju aplikacija za Veracode
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Veracode.

###<a name="to-enable-the-application-integration-for-veracode-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Veracode, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-veracode-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-veracode-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-veracode-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-veracode-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Veracode**.

    ![Galerija aplikacije] (./media/active-directory-saas-veracode-tutorial/IC802904.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Veracode**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Veracode] (./media/active-directory-saas-veracode-tutorial/IC802905.png "Veracode")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Veracode sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Aplikacija Veracode očekuje SAML assertions u određenom obliku, koje je potrebno da biste dodali prilagođeni atribut mapiranja konfiguraciju **saml tokena atribute** .  
Sljedeća slika prikazuje primjer za to.

![Atributi] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Atributi")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Veracode** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-veracode-tutorial/IC802907.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Veracode** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-veracode-tutorial/IC802908.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguracija postavki aplikacije** kliknite **Dalje**.

    ![Konfiguriranje postavki aplikacije] (./media/active-directory-saas-veracode-tutorial/IC802909.png "Konfiguriranje postavki aplikacije")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Veracode** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-veracode-tutorial/IC802910.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Veracode kao administrator.

6.  Na izborniku na vrhu kliknite **Postavke**, a zatim kliknite **administrator**.

    ![Administracija] (./media/active-directory-saas-veracode-tutorial/IC802911.png "Administracija")

7.  Kliknite karticu **SAML** .

8.  U odjeljku **Postavke SAML tvrtke ili ustanove** poduzeti sljedeće korake:

    ![Administracija] (./media/active-directory-saas-veracode-tutorial/IC802912.png "Administracija")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Veracode** kopirajte **URL izdavač** vrijednost, a pa ih zalijepite u tekstni okvir **izdavač**
    2.  Da biste prenijeli preuzete certifikata, kliknite **Odabir datoteke**.
    3.  Odaberite **Omogući prošao na samostalnom registracije**.

9.  U odjeljku **Postavke registracije samopotpisani** poduzeti sljedeće korake, a zatim kliknite **Spremi**:

    ![Administracija] (./media/active-directory-saas-veracode-tutorial/IC802913.png "Administracija")

    1.  Kao **Novi korisnik aktivaciju**odaberite **Bez aktivacija obavezna**.
    2.  Kao **Korisnička ažuriranja podataka**, odaberite **Preferenca Veracode korisničkih podataka**.
    3.  **SAML atribut detalje**, odaberite sljedeće:
        -   **Uloge korisnika**
        -   **Administrator pravila**
        -   **Pregledavatelj**
        -   **Sigurnost potencijalnog klijenta**
        -   **Izvršni**
        -   **Slanja**
        -   **Alat za stvaranje**
        -   **Sve vrste skeniranja**
        -   **Članstva u timu**
        -   **Zadani tim**

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-veracode-tutorial/IC802914.png "Konfiguriranje jedinstvenu prijavu")

11. Na izborniku na vrhu kliknite **atribute** da biste otvorili dijaloški okvir **SAML tokena atribute** .

    ![Atributi] (./media/active-directory-saas-veracode-tutorial/IC795920.png "Atributi")

12. Da biste dodali mapiranja obavezan atribut, poduzmite sljedeće korake:

    ![Atributi] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Atributi")

  	| Naziv atributa | Vrijednost atributa |
  	|:---------------|:----------------|
  	| ime      | User.givenname  |
  	| Prezime       | User.surname    |
  	| e-pošte          | User.Mail       |

    1.  Za svaki redak podatke iz gornje tablice, kliknite **Dodaj korisnika atribut**.
    
    2.  U tekstni okvir **Naziv atributa** upišite naziv atributa koji se prikazuju za tom retku.

    3.  U tekstni okvir **Atributa vrijednosti** odaberite vrijednost atributa koji se prikazuju za tom retku.

    4.  Kliknite **dovrši**.

13. Kliknite **Primijeni promjene**.

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Veracode, oni mora biti dodijeljena u Veracode.  
U slučaju Veracode, dodjeljivanje je automatizirana zadatak.  
Postoji nijedna stavka akcija za...
  
Korisnicima se automatski stvaraju po potrebi tijekom prvog jedan prijave pokušaj.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Veracode korisnički račun alate za stvaranje ili API-ji nudi Veracode dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-veracode-perform-the-following-steps"></a>Da biste korisnicima dodijelili Veracode, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Veracode **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-veracode-tutorial/IC802915.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-veracode-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).