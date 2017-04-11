<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Coupa | Microsoft Azure" 
    description="Saznajte kako koristiti Coupa s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-coupa"></a>Praktični vodič: Azure Active Directory Integracija s Coupa

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Coupa.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Coupa jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Coupa će moći jednom prijava u aplikacije pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Coupa
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenarij")
##<a name="enabling-the-application-integration-for-coupa"></a>Omogućivanje integraciju aplikacija za Coupa

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Coupa.

###<a name="to-enable-the-application-integration-for-coupa-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Coupa, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-coupa-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-coupa-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-coupa-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Coupa**.

    ![Galerija aplikacije] (./media/active-directory-saas-coupa-tutorial/IC791898.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Coupa**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Coupa] (./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Coupa sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za Coupa potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Prijavite se web-mjesto tvrtke Coupa kao administrator.

2.  Idite na **Postavljanje \> sigurnosna kontrola**.

    ![Sigurnosne kontrole] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Sigurnosne kontrole")

3.  Da biste preuzeli datoteku metapodataka Coupa na vašem računalu, kliknite **Preuzimanje i uvoz SP metapodataka**.

    ![Coupa SP metapodataka] (./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metapodataka")

4.  U prozoru drugi preglednik, prijavite se portalu Azure klasični.

5.  Na stranici za integraciju aplikacije **Coupa** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-coupa-tutorial/IC791902.png "Konfiguriranje jedinstvenu prijavu")

6.  Na stranici **kako biste željeli korisnika da biste se prijavili Coupa** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-coupa-tutorial/IC791903.png "Konfiguriranje jedinstvenu prijavu")

7.  Na stranici **Konfiguriranje URL adresa Web App** , učinite sljedeće:

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-coupa-tutorial/IC791904.png "Konfiguriranje URL adresa Web App")

    1.  U tekstni okvir **Prijavite se na URL** unesite URL ADRESU koriste vaši korisnici da biste se prijavili aplikaciju Coupa (npr.: "*http://company.Coupa.com*").
    2.  Otvorite preuzetu datoteku metapodataka Coupa, a zatim kopirajte **Indeks URL AssertionConsumerService**.
    3.  U tekstni okvir **URL odgovor Coupa** zalijepite vrijednost **Indeksa URL AssertionConsumerService** .
    4.  Kliknite **Dalje**.

8.  Na stranici **Konfiguracija jedinstvenu prijavu na Coupa** da biste preuzeli metapodataka datoteku, kliknite **Preuzmi metapodatke**, a zatim spremite datoteku na lokalno računalo.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-coupa-tutorial/IC791905.png "Konfiguriranje jedinstvenu prijavu")

9.  Na Coupa tvrtke web-mjesta, idite na **Postavljanje \> sigurnosna kontrola**.

    ![Sigurnosne kontrole] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Sigurnosne kontrole")

10. U odjeljku **prijavite se pomoću vjerodajnica Coupa** poduzeti sljedeće korake:

    ![Prijavite se pomoću vjerodajnica Coupa] (./media/active-directory-saas-coupa-tutorial/IC791906.png "Prijavite se pomoću vjerodajnica Coupa")

    1.  Odaberite **prijavite se pomoću SAML**.
    2.  Kliknite **Pregledaj** da biste prenijeli preuzetu datoteku Azure Active metapodataka.
    3.  Kliknite **Spremi**.

11. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-coupa-tutorial/IC791907.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u Coupa, oni mora biti dodijeljena u Coupa.  
U slučaju Coupa, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke **Coupa** kao administrator.

2.  Na izborniku na vrhu kliknite **Postavljanje**, a zatim **korisnika**.

    ![Korisnici] (./media/active-directory-saas-coupa-tutorial/IC791908.png "Korisnici")

3.  Kliknite **Stvori**.

    ![Stvaranje korisnika] (./media/active-directory-saas-coupa-tutorial/IC791909.png "Stvaranje korisnika")

4.  U odjeljku **Stvaranje korisnika** poduzeti sljedeće korake:

    ![Detalji o korisniku] (./media/active-directory-saas-coupa-tutorial/IC791910.png "Detalji o korisniku")

    1.  Upišite **prijave**, **ime**, **Prezime**, **Jedan ID za prijavu** **e-pošte** atribute valjani Azure Active Directory račun koji želite dodjele resursa u povezani tekstni okviri.
    2.  Kliknite **Stvori**.

    >[AZURE.NOTE] Vlasnik računa Azure Active Directory će primiti poruku e-pošte s vezom na postaje aktivan potvrditi račun.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Coupa korisnički račun alate za stvaranje ili API-ji nudi Coupa dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-coupa-perform-the-following-steps"></a>Da biste korisnicima dodijelili Coupa, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Coupa **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-coupa-tutorial/IC791911.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-coupa-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
