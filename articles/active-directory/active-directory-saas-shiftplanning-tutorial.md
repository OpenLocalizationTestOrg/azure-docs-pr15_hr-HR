<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s ShiftPlanning | Microsoft Azure" 
    description="Saznajte kako koristiti ShiftPlanning s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-shiftplanning"></a>Praktični vodič: Azure Active Directory Integracija s ShiftPlanning
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i ShiftPlanning.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na ShiftPlanning jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili ShiftPlanning će moći znak u aplikaciju na vašem ShiftPlanning web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za ShiftPlanning
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-shiftplanning-tutorial/IC786612.png "Scenarij")
##<a name="enabling-the-application-integration-for-shiftplanning"></a>Omogućivanje integraciju aplikacija za ShiftPlanning
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za ShiftPlanning.

###<a name="to-enable-the-application-integration-for-shiftplanning-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za ShiftPlanning, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-shiftplanning-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-shiftplanning-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-shiftplanning-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-shiftplanning-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **ShiftPlanning**.

    ![Galerija aplikacije] (./media/active-directory-saas-shiftplanning-tutorial/IC786613.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **ShiftPlanning**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![ShiftPlanning] (./media/active-directory-saas-shiftplanning-tutorial/IC786614.png "ShiftPlanning")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti ShiftPlanning sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **ShiftPlanning** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-shiftplanning-tutorial/IC786615.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili ShiftPlanning** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-shiftplanning-tutorial/IC786616.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **ShiftPlanning na URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://company.shiftplanning.com/includes/saml/*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-shiftplanning-tutorial/IC786617.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na ShiftPlanning** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-shiftplanning-tutorial/IC786618.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke **ShiftPlanning** kao administrator.

6.  Na izborniku na vrhu kliknite **administrator**.

    ![Administrator] (./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Administrator")

7.  U odjeljku **Integracija**kliknite **Jedinstvenu prijavu**.

    ![Jedinstvenu prijavu] (./media/active-directory-saas-shiftplanning-tutorial/IC786620.png "Jedinstvenu prijavu")

8.  U odjeljku **Jedinstvenu prijavu** poduzeti sljedeće korake:

    ![Jedinstvenu prijavu] (./media/active-directory-saas-shiftplanning-tutorial/IC786905.png "Jedinstvenu prijavu")

    1.  Odaberite **SAML omogućena**.
    2.  Odaberite **Dopusti prijavu lozinke**
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na ShiftPlanning** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL SAML izdavač** .
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na ShiftPlanning** kopirajte **URL daljinskog odjavite** vrijednost, a zatim je zalijepite u tekstni okvir **URL daljinskog odjavite** .
    5.  Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata.  

        >[AZURE.TIP]Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **Certifikat X.509**
    7.  Kliknite **Spremi postavke**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-shiftplanning-tutorial/IC786621.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u ShiftPlanning, oni mora biti dodijeljena u ShiftPlanning.  
U slučaju ShiftPlanning, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **ShiftPlanning** kao administrator.

2.  Kliknite **administrator**.

    ![Administrator] (./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Administrator")

3.  Kliknite **osoblje**.

    ![Osoblje] (./media/active-directory-saas-shiftplanning-tutorial/IC786623.png "Osoblje")

4.  U odjeljku **Akcije**kliknite **Dodaj zaposlenika**.

    ![Dodavanje zaposlenika] (./media/active-directory-saas-shiftplanning-tutorial/IC786624.png "Dodavanje zaposlenika")

5.  U odjeljku **Dodavanje zaposlenici** poduzeti sljedeće korake:

    ![Spremanje zaposlenika] (./media/active-directory-saas-shiftplanning-tutorial/IC786625.png "Spremanje zaposlenika")

    1.  Upišite **ime**, **Prezime** i **e-pošte** valjan račun za AAD želite Dodjela resursa u povezani tekstni okviri.
    2.  Kliknite **Spremi zaposlenika**.

>[AZURE.NOTE]Možete koristiti bilo koji drugi ShiftPlanning korisnički račun alate za stvaranje ili API-ji nudi ShiftPlanning dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-shiftplanning-perform-the-following-steps"></a>Da biste korisnicima dodijelili ShiftPlanning, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **ShiftPlanning **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-shiftplanning-tutorial/IC786626.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-shiftplanning-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).