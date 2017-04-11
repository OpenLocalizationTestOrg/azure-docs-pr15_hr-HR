<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Huddle | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory Huddle da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-huddle"></a>Praktični vodič: Azure Active Directory Integracija s Huddle
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Huddle.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Huddle jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Huddle će moći znak u aplikaciju na vašem Huddle web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Huddle
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-huddle-tutorial/IC787830.png "Konfiguriranje jedinstvenu prijavu")
##<a name="enabling-the-application-integration-for-huddle"></a>Omogućivanje integraciju aplikacija za Huddle
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Huddle.

###<a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Huddle, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-huddle-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-huddle-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-huddle-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-huddle-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Huddle**.

    ![Galerija aplikacije] (./media/active-directory-saas-huddle-tutorial/IC787831.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Huddle**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Huddle] (./media/active-directory-saas-huddle-tutorial/IC787832.png "Huddle")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Huddle sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za **Huddle** aplikacije Integracija kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-huddle-tutorial/IC787833.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako želite korisnicima da biste se prijavili Huddle** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-huddle-tutorial/IC787834.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Huddle na URL za prijavu** upišite URL vaš klijent Huddle pomoću sljedećeg uzorka "*http://company.huddle.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-huddle-tutorial/IC787835.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Huddle** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-huddle-tutorial/IC787836.png "Konfiguriranje jedinstvenu prijavu")

    1.  Kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.
    2.  Kopirajte vrijednost **Izdavač URL** , **SAML SSO URL** vrijednost i preuzeti certifikat i poslati ih Huddle timu za podršku.

    >[AZURE.NOTE] Jedinstvenu prijavu mora biti omogućen od tima za podršku Huddle.
Dobit ćete obavijest dovršetku konfiguracije.

5.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-huddle-tutorial/IC787837.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Huddle, oni mora biti dodijeljena u Huddle.  
U slučaju Huddle, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke **Huddle** kao administrator.

2.  Kliknite **radni prostor**.

3.  Kliknite **osobe \> pozivanje osoba**.

    ![Osobe] (./media/active-directory-saas-huddle-tutorial/IC787838.png "Osobe")

4.  U odjeljku **Stvori novu pozivnicu** poduzeti sljedeće korake:

    ![Novi poziv] (./media/active-directory-saas-huddle-tutorial/IC787839.png "Novi poziv")

    1.  Na popisu **Odaberite tima da biste pozvali osobe da biste se pridružili** , odaberite **tim**.
    2.  Unesite **Adresu e-pošte** valjani AAD račun koji želite dodjele resursa u povezani tekstni okvir.
    3.  Kliknite **Pozovi**.

    >[AZURE.NOTE] Vlasnik računa Azure AD primit će poruku e-pošte uključujući vezu da biste potvrdili račun prije postaje aktivna.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Huddle korisnički račun alate za stvaranje ili API-ji koje ste dobili od Huddle Dodjela resursa za AAD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Da biste korisnicima dodijelili Huddle, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za **Huddle **aplikacije Integracija kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-huddle-tutorial/IC787840.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-huddle-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).