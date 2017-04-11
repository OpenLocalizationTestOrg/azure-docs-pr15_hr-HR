<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Bonus.ly | Microsoft Azure" 
    description="Saznajte kako koristiti Bonus.ly s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Praktični vodič: Azure Active Directory Integracija s Bonus.ly

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Bonus.ly. Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Testiranje klijenta u Bonus.ly

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Bonus.ly
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-bonus-tutorial/IC773679.png "Scenarij")
##<a name="enabling-the-application-integration-for-bonusly"></a>Omogućivanje integraciju aplikacija za Bonus.ly

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Bonus.ly.

###<a name="to-enable-the-application-integration-for-bonusly-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Bonus.ly, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Omogućivanje jedinstvenu prijavu] (./media/active-directory-saas-bonus-tutorial/IC773680.png "Omogućivanje jedinstvenu prijavu")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-bonus-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-bonus-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-bonus-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Bonus.ly**.

    ![Galerija aplikacije] (./media/active-directory-saas-bonus-tutorial/IC773681.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Bonus.ly**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Bonus.ly sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za Bonus.ly potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Bonus.ly** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bonus-tutorial/IC749323.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Bonus.ly** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bonus-tutorial/IC773683.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Bonus.ly klijentu URL** unesite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. Bonus.LY*", a zatim kliknite **Dalje**: 

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-bonus-tutorial/IC773684.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Bonus.ly** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno kao **c:\\Bonusly.cer**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bonus-tutorial/IC773685.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru drugi preglednik, prijavite se u klijent **Bonus.ly** .

6.  Na alatnoj traci na vrhu kliknite **Postavke**, a zatim odaberite **integracije i aplikacije**.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.  U odjeljku **Jedinstvenu prijavu**odaberite **SAML**.

8.  Na stranici dijaloški **SAML** poduzeti sljedeće korake:

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Bonus.ly** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **IdP SSO odredišni URL** .
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Bonus.ly** kopirajte vrijednost **ID izdavača** i pa ih zalijepite u tekstni okvir **IdP izdavač** .
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Bonus.ly** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL IdP prijava** .
    4.  Kopirajte vrijednost **otisak prsta** iz izvezene potvrde i pa ih zalijepite u tekstni okvir **Otiska prsta certifikata** .

        >[AZURE.TIP] Dodatne informacije potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

9.  Kliknite **Spremi**.

10. Na portalu Microsoft Azure klasični odaberite konfiguracije potvrdu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bonus-tutorial/IC773689.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u Bonus.ly, oni mora biti dodijeljena u Bonus.ly.  
U slučaju Bonus.ly, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  U prozoru web-preglednika, prijavite se u klijentu za Bonus.ly.

2.  Kliknite **Postavke**

    ![Postavke] (./media/active-directory-saas-bonus-tutorial/IC781041.png "Postavke")

3.  Kliknite karticu **korisnika i bonusa** .

    ![Korisnici i bonusa] (./media/active-directory-saas-bonus-tutorial/IC781042.png "Korisnici i bonusa")

4.  Kliknite **Upravljanje korisnicima**.

    ![Upravljanje korisnicima] (./media/active-directory-saas-bonus-tutorial/IC781043.png "Upravljanje korisnicima")

5.  Kliknite **Dodaj korisnika**.

    ![Dodavanje korisnika] (./media/active-directory-saas-bonus-tutorial/IC781044.png "Dodavanje korisnika")

6.  U dijaloškom okviru **Dodavanje korisnika** , poduzmite sljedeće korake:

    ![Dodavanje korisnika] (./media/active-directory-saas-bonus-tutorial/IC781045.png "Dodavanje korisnika")

    1.  Upišite "**e-pošte**, **ime**, **Prezime**" valjani AAD račun koji želite dodjele resursa u povezani tekstni okviri.
    2.  Kliknite **Spremi**.

    >[AZURE.NOTE] Vlasnik računa AAD primit će poruku e-pošte koja sadrži vezu da biste potvrdili račun prije postaje aktivna.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Bonus.ly korisnički račun alate za stvaranje ili API-ji nudi Bonus.ly dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-bonusly-perform-the-following-steps"></a>Da biste korisnicima dodijelili Bonus.ly, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije Bonus.ly kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-bonus-tutorial/IC773690.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-bonus-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
