<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Bime | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory Bime da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bime"></a>Praktični vodič: Azure Active Directory Integracija s Bime

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Bime.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Bime klijenta

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Bime će moći znak u aplikaciju na vašem Bime web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Bime
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-bime-tutorial/IC775552.png "Scenarij")
##<a name="enabling-the-application-integration-for-bime"></a>Omogućivanje integraciju aplikacija za Bime

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Bime.

###<a name="to-enable-the-application-integration-for-bime-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Bime, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bime-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-bime-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-bime-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-bime-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Bime**.

    ![Galerija aplikacije] (./media/active-directory-saas-bime-tutorial/IC775553.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Bime**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Bime] (./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Bime sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za Bime potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Bime** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bime-tutorial/IC771709.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Bime** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bime-tutorial/IC775555.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Bime URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. Bimeapp.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-bime-tutorial/IC775556.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Bime** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno kao **c:\\Bime.cer**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bime-tutorial/IC775557.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Bime kao administrator.

6.  Na alatnoj traci kliknite **administrator**, a zatim **račun**.

    ![Administrator] (./media/active-directory-saas-bime-tutorial/IC775558.png "Administrator")

7.  Na stranici Konfiguracija računa, poduzmite sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bime-tutorial/IC775559.png "Konfiguriranje jedinstvenu prijavu")

    1.  Odaberite **Omogući SAML provjeru autentičnosti**.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Bime** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL daljinskog prijava** .
    3.  Kopirajte vrijednost **otisak prsta** iz izvezene potvrde i pa ih zalijepite u tekstni okvir za **Potvrdu otiska prsta** .  

        >[AZURE.TIP] Dodatne informacije potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

    4.  Kliknite **Spremi**.

8.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bime-tutorial/IC775560.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u Bime, oni mora biti dodijeljena u Bime.  
U slučaju Bime, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se u klijent **Bime** .

2.  Na alatnoj traci kliknite **administrator**, a zatim **korisnika**.

    ![Administrator] (./media/active-directory-saas-bime-tutorial/IC775561.png "Administrator")

3.  **Popis korisnika**, kliknite **Dodaj novog korisnika** ("+").

    ![Korisnici] (./media/active-directory-saas-bime-tutorial/IC775562.png "Korisnici")

4.  Na stranici dijaloški okvir **Detalji o korisniku** , učinite sljedeće:

    ![Detalji o korisniku] (./media/active-directory-saas-bime-tutorial/IC775563.png "Detalji o korisniku")

    1.  Unesite ime, prezime, prijavu, e-pošte valjani AAD računa koji želite dodjele resursa.
    2.  Kliknite Spremi.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Bime korisnički račun alate za stvaranje ili API-ji nudi Bime dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-bime-perform-the-following-steps"></a>Da biste korisnicima dodijelili Bime, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Bime **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-bime-tutorial/IC775564.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-bime-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
