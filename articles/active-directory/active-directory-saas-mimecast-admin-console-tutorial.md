<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s konzole za administratore Mimecast | Microsoft Azure" 
    description="Saznajte kako koristiti Mimecast administratorske konzole s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Praktični vodič: Azure Active Directory Integracija s Mimecast konzole za administratore
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Mimecast administracijskoj konzoli sustava.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   S konzole za administratore Mimecast jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili administratorske konzole Mimecast će moći znak u aplikaciju na vašem Mimecast konzole za administratore web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Mimecast konzolu za administratore
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "Scenarij")
##<a name="enabling-the-application-integration-for-mimecast-admin-console"></a>Omogućivanje integraciju aplikacija za Mimecast konzole za administratore
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Mimecast administracijskoj konzoli sustava.

###<a name="to-enable-the-application-integration-for-mimecast-admin-console-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Mimecast administracijskoj konzoli sustava, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Mimecast administracijskoj konzoli sustava**.

    ![Galerija aplikacije] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Mimecast administracijskoj konzoli sustava**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Konzola za administratore Mimecast] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Konzola za administratore Mimecast")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Mimecast administratorske konzole sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Mimecast konzole za administratore** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da se prijavite se konzolu za administratore Mimecast** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Mimecast administrator konzole za prijavu na URL** unesite URL koji se koristi korisnika da biste se prijavili Mimecast administratorske konzole aplikacije (npr.: "https://webmail-uk.mimecast.com" ili "https://webmail-us.mimecast.com"), a zatim kliknite **Dalje**.

    >[AZURE.NOTE] URL za prijavu je određenu regiju.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Mimecast administracijskoj konzoli sustava** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**i spremanje datoteka certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u konzole za administratore Mimecast kao administrator.

6.  Idite na **Services \> aplikacije**.

    ![Usluge] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "Usluge")

7.  Kliknite **Provjera autentičnosti profile**.

    ![Provjera autentičnosti profila] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "Provjera autentičnosti profila")

8.  Kliknite **novi profil za provjeru autentičnosti**.

    ![Nove profile provjere autentičnosti] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "Nove profile provjere autentičnosti")

9.  U odjeljku **Provjera autentičnosti profila** poduzeti sljedeće korake:

    ![Provjera autentičnosti profila] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "Provjera autentičnosti profila")

    1.  U tekstni okvir **Opis** upišite naziv za konfiguraciju.
    2.  Odaberite **nametnuti provjeru autentičnosti SAML Mimecast administracijskoj konzoli sustava**.
    3.  Kao **davatelja usluge**, odaberite **Azure Active Directory**.
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na konzolu za administratore Mimecast** kopirajte vrijednost **Izdavač URL** i pa ih zalijepite u tekstni okvir **URL izdavač** .
    5.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na konzolu za administratore Mimecast** kopirajte vrijednost **URL daljinskog prijava** i pa ih zalijepite u tekstni okvir **URL za prijavu** .
    6.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na konzolu za administratore Mimecast** kopirajte vrijednost **URL daljinskog prijave** i pa ih zalijepite u tekstni okvir **URL odjavite** .  

        >[AZURE.NOTE]URL za prijavu vrijednosti i u odjavite URL jednaki su za Mimecast administracijskoj konzoli sustava.

    7.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.  

        >[AZURE.TIP]Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

    8.  Otvaranje certifikata kodirana osnovni 64 u Bloku za pisanje, uklonite u prvom retku ("*--*") i zadnji redak ("*--*"), kopirajte preostale sadržaj je u međuspremnik, a zatim je zalijepite u tekstni okvir za **Potvrdu davatelja identiteta (metapodaci)** .
    9.  Odaberite **Dopusti znak na**.
    10. Kliknite **Spremi**.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u administracijskoj konzoli Mimecast, oni mora biti dodijeljena u Mimecast administracijskoj konzoli sustava.  
U slučaju Mimecast administratorske konzole dodjeljivanje jest zadatak za ručno.
  
Morate registrirati domene možete stvarati korisnike.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se **Konzolu za administratore Mimecast** kao administrator.

2.  Idite na **direktorija \> interne**.

    ![Direktorija] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "Direktorija")

3.  Kliknite **registrirati novu domenu**.

    ![Registrirajte se nova domena] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "Registrirajte se nova domena")

4.  Nakon stvaranja nove domene kliknite **Novu adresu**.

    ![Nova adresa] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "Nova adresa")

5.  U dijaloškom okviru za novi adresu poduzeti sljedeće korake:

    ![Spremanje] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Spremanje")

    1.  Upišite **Adresu e-pošte**, **Globalni ime**, **lozinku** i **Potvrdite lozinku** atributa valjani AAD računa koji želite dodjele resursa u povezani tekstni okviri.
    2.  Kliknite **Spremi**.

>[AZURE.NOTE]Možete koristiti druge alate za stvaranje Mimecast administratorske konzole korisnički račun ili API-ji nudi Mimecast administratorske konzole za dodjeljivanje AAD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-mimecast-admin-console-perform-the-following-steps"></a>Da biste korisnicima dodijelili Mimecast administratorske konzole, učinite sljedeće:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Mimecast konzole za administratore **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).