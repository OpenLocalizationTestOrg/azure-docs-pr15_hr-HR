<properties 
    pageTitle="Praktični vodič: Integracija Azure Active Directory pomoću portala za osobno Mimecast | Microsoft Azure" 
    description="Saznajte kako koristiti Mimecast osobne Portal s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Praktični vodič: Azure Active Directory Integracija s Mimecast osobno Portal
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Mimecast osobne Portal.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na osobni Portal Mimecast jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Mimecast osobne Portal će moći znak u aplikaciju na vaše osobne Portal Mimecast web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija Mimecast osobne portala
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794991.png "Scenarij")
##<a name="enabling-the-application-integration-for-mimecast-personal-portal"></a>Omogućivanje integraciju aplikacija Mimecast osobne portala
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Mimecast osobne Portal.

###<a name="to-enable-the-application-integration-for-mimecast-personal-portal-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Portal za osobne Mimecast, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Mimecast osobne Portal**.

    ![Galerija aplikacije] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794992.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Mimecast osobne Portal**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Portal za osobno Mimecast] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794993.png "Portal za osobno Mimecast")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Mimecast osobne Portal sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Mimecast osobne Portal** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794994.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da se prijavite se Portal za osobne Mimecast** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794995.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Mimecast osobne Portal na URL za prijavu** upišite URL koji se koristi korisnika za prijavu na Portal za osobne Mimecast aplikacije (npr.: "https://webmail-uk.mimecast.com" ili "https://webmail-us.mimecast.com"), a zatim kliknite **Dalje**.

    >[AZURE.NOTE] URL za prijavu je određenu regiju.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794996.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Portal za osobne Mimecast** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794997.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u portalom osobne Mimecast kao administrator.

6.  Idite na **Services \> aplikacije**.

    ![Aplikacija] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794998.png "Aplikacija")

7.  Kliknite **Provjera autentičnosti profile**.

    ![Provjera autentičnosti profila] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794999.png "Provjera autentičnosti profila")

8.  Kliknite **novi profil za provjeru autentičnosti**.

    ![Novi Profil za provjeru autentičnosti] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795000.png "Novi Profil za provjeru autentičnosti")

9.  U odjeljku **Provjera autentičnosti profila** poduzeti sljedeće korake:

    ![Provjera autentičnosti Profil] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795001.png "Provjera autentičnosti Profil")

    1.  U tekstni okvir **Opis** upišite naziv za konfiguraciju.
    2.  Odaberite **Nametni SAML provjere autentičnosti za Portal Mimecast osobno**.
    3.  Kao **davatelja usluge**, odaberite **Azure Active Directory**.
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Portal za osobne Mimecast** kopirajte vrijednost **Izdavač URL** i pa ih zalijepite u tekstni okvir **URL izdavač** .
    5.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Portal za osobne Mimecast** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu** .
    6.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Portal za osobne Mimecast** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL odjavite** .  

        >[AZURE.NOTE] Vrijednosti URL za prijavu i na URL odjavite se za-na pri Mimecast osobne Portal isti.

    7.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.  

        >[AZURE.TIP]Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

    8.  Otvaranje certifikata kodirana osnovni 64 u Bloku za pisanje, uklonite u prvom retku ("*--*") i zadnji redak ("*--*"), kopirajte preostale sadržaj je u međuspremnik, a zatim je zalijepite u tekstni okvir za **Potvrdu davatelja identiteta (metapodaci)** .
    9.  Odaberite **Dopusti znak na**.
    10. Kliknite **Spremi**.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795002.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Mimecast osobne Portal, oni mora biti dodijeljena u Mimecast osobne Portal.  
U slučaju Mimecast Portal za osobne dodjeljivanje jest zadatak za ručno.
  
Morate registrirati domene možete stvarati korisnike.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se **Portal za osobne Mimecast** kao administrator.

2.  Idite na **direktorija \> interne**.

    ![Direktorija] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795003.png "Direktorija")

3.  Kliknite **registrirati novu domenu**.

    ![Registrirajte se nova domena] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795004.png "Registrirajte se nova domena")

4.  Nakon stvaranja nove domene kliknite **Novu adresu**.

    ![Nova adresa] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795005.png "Nova adresa")

5.  U dijaloškom okviru za novi adresu poduzeti sljedeće korake:

    ![Spremanje] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795006.png "Spremanje")

    1.  Upišite **Adresu e-pošte**, **Globalni ime**, **lozinku** i **Potvrdite lozinku** atributa valjani AAD računa koji želite dodjele resursa u povezani tekstni okviri.
    2.  Kliknite **Spremi**.

>[AZURE.NOTE]Možete koristiti druge alate za stvaranje osobnih Portal Mimecast korisnički račun ili API-ji nudi Mimecast osobne Portal za dodjeljivanje AAD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-mimecast-personal-portal-perform-the-following-steps"></a>Da biste korisnicima dodijelite Mimecast osobne Portal, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Mimecast osobne Portal **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795007.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).