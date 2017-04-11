<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija Integracija s Druva | Microsoft Azure" 
    description="Saznajte kako koristiti Druva s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Praktični vodič: Azure Active Directory Integracija Integracija s Druva

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Druva.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Druva jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Druva će moći znak u aplikaciju na vašem Druva web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Druva
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-druva-tutorial/IC795084.png "Scenarij")
##<a name="enabling-the-application-integration-for-druva"></a>Omogućivanje integraciju aplikacija za Druva

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Druva.

###<a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Druva, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-druva-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-druva-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-druva-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-druva-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Druva**.

    ![Galerija aplikacije] (./media/active-directory-saas-druva-tutorial/IC795085.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Druva**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Druva] (./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Druva sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

Aplikacija Druva očekuje SAML assertions u određenom obliku, koje je potrebno da biste dodali prilagođeni atribut mapiranja konfiguraciju **saml tokena atribute** .  
Sljedeća slika prikazuje primjer za to.

![SAML tokena atributa] (./media/active-directory-saas-druva-tutorial/IC795087.png "SAML tokena atributa")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Druva** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-druva-tutorial/IC795027.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Druva** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-druva-tutorial/IC795088.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Druva na URL za prijavu** upišite URL koji se koristi korisnika da biste se prijavili aplikaciju Druva (npr.: "*https://cloud.druva.com/home/*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-druva-tutorial/IC795089.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Druva** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-druva-tutorial/IC795090.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Druva kao administrator.

6.  Idite na **Upravljanje \> postavke**.

    ![Postavke] (./media/active-directory-saas-druva-tutorial/IC795091.png "Postavke")

7.  U dijaloškom okviru postavke za jednu prijave poduzeti sljedeće korake:

    ![Postavke za Singl prijave] (./media/active-directory-saas-druva-tutorial/IC795092.png "Postavke za Singl prijave")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Druva** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu davatelja ID -a** .
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Druva** kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **ID davatelja odjavite URL-a** .
    3.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.  

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **ID davatelja certifikata**
    5.  Da biste otvorili stranicu s **postavkama** , kliknite **Spremi**.

8.  Na stranici **Postavke** kliknite **Generiraj tokena SSO**.

    ![Postavke] (./media/active-directory-saas-druva-tutorial/IC795093.png "Postavke")

9.  U dijaloškom okviru **Jedan prijave provjere autentičnosti tokena** poduzeti sljedeće korake:

    ![SSO tokena] (./media/active-directory-saas-druva-tutorial/IC795094.png "SSO tokena")

    1.  Kliknite **Kopiraj**.
    2.  Kliknite **Zatvori**.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-druva-tutorial/IC795095.png "Konfiguriranje jedinstvenu prijavu")

11. Na izborniku na vrhu kliknite **atribute** da biste otvorili dijaloški okvir **SAML tokena atribute** .

    ![Atributi] (./media/active-directory-saas-druva-tutorial/IC795096.png "Atributi")

12. Da biste dodali mapiranja obavezan atribut, poduzmite sljedeće korake:

  	|Naziv atributa|Vrijednost atributa|
  	|---|---|
  	|insync\_auth\_tokena|<*vrijednost u međuspremnik*>|

    1.  Za svaki redak podatke iz gornje tablice, kliknite **Dodaj korisnika atribut**.
    2.  U tekstni okvir **Naziv atributa** upišite naziv atributa koji se prikazuju za tom retku.
    3.  U tekstni okvir **Vrijednost atributa** upišite vrijednost atributa koji se prikazuju za tom retku.
    4.  Kliknite **dovrši**.

13. Kliknite **Primijeni promjene**.
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u Druva, oni mora biti dodijeljena u Druva.  
U slučaju Druva, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke **Druva** kao administrator.

2.  Idite na **Upravljanje \> korisnika**.

    ![Upravljanje korisnicima] (./media/active-directory-saas-druva-tutorial/IC795097.png "Upravljanje korisnicima")

3.  Kliknite **Stvori novi**.

    ![Upravljanje korisnicima] (./media/active-directory-saas-druva-tutorial/IC795098.png "Upravljanje korisnicima")

4.  U dijaloškom okviru Stvaranje novog korisnika poduzeti sljedeće korake:

    ![Stvaranje NewUser] (./media/active-directory-saas-druva-tutorial/IC795099.png "Stvaranje NewUser")

    1.  Upišite adresu e-pošte, a naziv Azure Active Directory korisnički račun koji želite Dodjela resursa u povezane tekstne okvire.
    2.  Kliknite **Stvori korisnika**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Druva korisnički račun alate za stvaranje ili API-ji nudi Druva dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-druva-perform-the-following-steps"></a>Da biste korisnicima dodijelili Druva, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Druva **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-druva-tutorial/IC795100.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-druva-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
