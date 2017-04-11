<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija sa središnje radnu površinu | Microsoft Azure" 
    description="Saznajte kako koristiti središnje radnu površinu s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Praktični vodič: Azure Active Directory Integracija sa središnje radne površine

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i središnje radnu površinu. Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Središnje radne površine znak omogućeno pretplate na / smjernice za središnju radne površine

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za središnju radnu površinu
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenarij")
##<a name="enabling-the-application-integration-for-central-desktop"></a>Omogućivanje integraciju aplikacija za središnju radnu površinu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za središnju radnu površinu.

###<a name="to-enable-the-application-integration-for-central-desktop-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za središnju radnu površinu, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Središnje radnu površinu**.

    ![Galerija aplikacije] (./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Središnje radnu površinu**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Središnje radne površine] (./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Središnje radne površine")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti središnje radnu površinu sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za prijenos certifikata kodirana osnovni 64 u klijentu za središnju radnu površinu.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).



###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Središnja radne površine** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili središnje radne površine** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** poduzeti sljedeće korake, a zatim kliknite **Dalje**: 

    -   U tekstni okvir **Središnje radnu površinu URL za prijavu** upišite URL klijentu za središnju radne površine (npr.: *http://contoso.centraldesktop.com*).
    -   U tekstni okvir središnje radnu površinu odgovor URL-a upišite središnje radnu površinu AssertionConsumerService URL (npr.: https://contoso.centraldesktop.com/saml2-assertion.php).

    >[AZURE.NOTE] Vrijednost možete pristupiti iz središnje metapodataka za stolna računala (npr.: *http://contoso.centraldesktop.com*).

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na središnje radna površina** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**i spremanje datoteka certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Konfiguriranje jedinstvenu prijavu")

5.  Prijavite se u klijent **Središnje radnu površinu** .

6.  Idite na **Postavke**, kliknite **Dodatno**, a zatim kliknite **Jedinstvenu prijavu**.

    ![Postavljanje – Napredno] (./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Postavljanje – Napredno")

7.  Na stranici za **Jedan znak na postavke** poduzeti sljedeće korake:

    ![Jedine prijave postavke] (./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Jedine prijave postavke")

    1.  Odaberite **Omogući SAML v2 jedine prijave**.
    2.  Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na središnje radnu površinu** , kopirajte **URL izdavač** vrijednost, a pa ih zalijepite u tekstni okvir **SSO URL** .
    3.  Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na središnje radnu površinu** kopirajte vrijednost **URL daljinskog prijava** i pa ih zalijepite u tekstni okvir **URL SSO za prijavu** .
    4.  Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na središnje radnu površinu** , kopirajte **URL servisa za jednu Sign-Out** vrijednost, a pa ih zalijepite u tekstni okvir **URL odjavite SSO** .

8.  U odjeljku **Način potvrde potpis za poruku** , učinite sljedeće:

    ![Način provjere potpisa za poruke] (./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Način provjere potpisa za poruke")

    1.  Odaberite **certifikat**.
    2.  Na popisu **SSO za potvrdu** odaberite **RSH SHA256**.
    3.  Stvaranje tekstne datoteke iz preuzete certifikata, kopirajte sadržaj tekstnu datoteku i pa ih zalijepite u polje **SSO certifikata** .  

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    4.  Odaberite **Prikaz veza na stranicu za prijavu u SAMLv2**.

9.  Kliknite **Ažuriraj**.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Za AAD da korisnici mogu se prijaviti, oni mora biti dodijeljena aplikaciju središnje radnu površinu. U ovom se odjeljku opisuje kako stvoriti AAD korisničkih računa u središnjoj za stolna računala.

###<a name="to-provision-user-accounts-to-central-desktop"></a>Dodjela korisničkih računa za središnju radne površine:

1.  Prijavite se u klijent središnje radnu površinu.

2.  Idite na **osobe \> interne članove**.

3.  Kliknite **Dodavanje interne članove**.

    ![Osobe] (./media/active-directory-saas-central-desktop-tutorial/IC781051.png "Osobe")

4.  U tekstni okvir **E-pošte adresa od nove članove** unesite račun AAD želite dodjele resursa, a zatim kliknite **Dalje**.

    ![Adrese e-pošte članova] (./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Adrese e-pošte članova")

5.  Kliknite **Dodavanje internog članova**.

    ![Dodavanje internog člana] (./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Dodavanje internog člana")

    >[AZURE.NOTE] Korisnici koji ste dodali primit će poruku e-pošte koja sadrži vezu za potvrdu moraju kliknuti da biste aktivirali račun.

>[AZURE.NOTE] Možete koristiti bilo koji drugi središnje radne površine korisnički račun alate za stvaranje ili API-ji nudi središnje radnu površinu nakon dodjele resursa AAD korisničkih računa

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-central-desktop-perform-the-following-steps"></a>Da biste korisnicima dodijelili središnje radnu površinu, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Središnja radne površine** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
