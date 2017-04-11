<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s područja crtanja LMS | Microsoft Azure" 
    description="Saznajte kako koristiti područje crtanja LMS s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Praktični vodič: Azure Active Directory Integracija s područja crtanja LMS

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i područje crtanja.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Područje crtanja klijenta

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili područje crtanja će moći znak u aplikaciju na vaše područje crtanja web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za područje crtanja
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "Scenarij")
##<a name="enabling-the-application-integration-for-canvas"></a>Omogućivanje integraciju aplikacija za područje crtanja

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za područje crtanja.

###<a name="to-enable-the-application-integration-for-canvas-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za područje crtanja, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **područje crtanja**.

    ![Galerija aplikacije] (./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "Galerija aplikacije")

7.  U oknu s rezultatima, odaberite **područje crtanja**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Područje crtanja] (./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "Područje crtanja")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti područje crtanja sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za područje crtanja potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici integraciju aplikacije **područje crtanja** , kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili područje crtanja** , odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Crtanja URL za prijavu** upišite URL pomoću sljedećeg uzorka `https://<tenant-name>.instructure.com`, a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na područje crtanja** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke crtanja kao administrator.

6.  Idite na **tečajeve \> upravlja računima \> Microsoft**.

    ![Područje crtanja] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Područje crtanja")

7.  U navigacijskom oknu s lijeve strane odaberite **provjeru autentičnosti**, a zatim **Dodaj novu Config SAML**.

    ![Provjera autentičnosti] (./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Provjera autentičnosti")

8.  Na stranici trenutni Integracija poduzeti sljedeće korake:

    ![Trenutni Integracija] (./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Trenutni Integracija")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na područje crtanja** kopirajte vrijednost **ID entitet** , a pa ih zalijepite u tekstni okvir **ID entitet IdP** .
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na područje crtanja** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za na zapisnika** .
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na područje crtanja** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za odgovor zapisnika** .
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na područje crtanja** kopirajte vrijednost **URL Promijeni lozinku** i pa ih zalijepite u tekstni okvir **Vezom za promjenu lozinke** .
    5.  Kopirajte vrijednost **otisak prsta** iz izvezene potvrde i pa ih zalijepite u tekstni okvir za **Potvrdu otiska prsta** .  

        >[AZURE.TIP] Dodatne informacije potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

    6.  Na popisu **Prijava atribut** odaberite **NameID**.
    7.  Na popisu **Identifikator oblik** odaberite **Adresa**.
    8.  Kliknite **Spremi postavke provjere autentičnosti**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u području crtanja, oni mora biti dodijeljena u područje crtanja.  
U slučaju područje crtanja, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **područje crtanja** .

2.  Idite na **tečajeve \> upravlja računima \> Microsoft**.

    ![Područje crtanja] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Područje crtanja")

3.  Kliknite **korisnici**.

    ![Korisnici] (./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Korisnici")

4.  Kliknite **Dodaj novog korisnika**.

    ![Korisnici] (./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Korisnici")

5.  Na stranici Dodavanje novog korisnika dijaloški okvir, poduzmite sljedeće korake:

    ![Dodavanje korisnika] (./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Dodavanje korisnika")

    1.  U tekstni okvir **Ime i prezime** upišite ime korisnika.
    2.  U tekstni okvir **e-pošte** upišite adresu e-pošte za korisnika.
    3.  U tekstni okvir za **prijavu** upišite adresu e-pošte za Azure AD korisnika.
    4.  Odaberite **e-pošte korisnika o stvaranja ovog računa**.
    5.  Kliknite **Dodaj korisnika**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi područje crtanja korisnički račun alate za stvaranje ili API-ji nudi crtanja dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-canvas-perform-the-following-steps"></a>Da biste korisnicima dodijelili područje crtanja, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **crtanja **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
