<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s ArcGIS | Microsoft Azure" 
    description="Saznajte kako koristiti ArcGIS s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-arcgis"></a>Praktični vodič: Azure Active Directory Integracija s ArcGIS

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i ArcGIS. Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na ArcGIS jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili ArcGIS će moći znak u aplikaciju na vašem ArcGIS web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za ArcGIS
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-arcgis-tutorial/IC784735.png "Scenarij")
##<a name="enabling-the-application-integration-for-arcgis"></a>Omogućivanje integraciju aplikacija za ArcGIS

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za ArcGIS.

###<a name="to-enable-the-application-integration-for-arcgis-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za ArcGIS, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-arcgis-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-arcgis-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-arcgis-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-arcgis-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **ArcGIS**.

    ![Galerija Applcation] (./media/active-directory-saas-arcgis-tutorial/IC784736.png "Galerija Applcation")

7.  U oknu s rezultatima odaberite **ArcGIS**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![ArcGIS] (./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti ArcGIS sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **ArcGIS** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-arcgis-tutorial/IC784738.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili ArcGIS** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-arcgis-tutorial/IC784739.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **ArcGIS URL za prijavu** unesite URL ADRESU koriste vaši korisnici za prijavu pomoću sljedećeg uzorka "*https://company.maps.arcgis.com*", a zatim **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-arcgis-tutorial/IC784740.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na ArcGIS** kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-arcgis-tutorial/IC784741.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke ArcGIS kao administrator.

6.  Kliknite **Uređivanje postavki**.

    ![Uređivanje postavki] (./media/active-directory-saas-arcgis-tutorial/IC784742.png "Uređivanje postavki")

7.  Kliknite **Sigurnost**.

    ![Sigurnost] (./media/active-directory-saas-arcgis-tutorial/IC784743.png "Sigurnost")

8.  U odjeljku **Enterprise prijave**, kliknite **Postavljanje davatelja identiteta**.

    ![Enterprise prijave] (./media/active-directory-saas-arcgis-tutorial/IC784744.png "Enterprise prijave")

9.  Na stranici Konfiguracija **Postavljanje davatelja identiteta** poduzeti sljedeće korake:

    ![Postavljanje davatelja identiteta] (./media/active-directory-saas-arcgis-tutorial/IC784745.png "Postavljanje davatelja identiteta")

    1.  U tekstni okvir Naziv upišite naziv svoje tvrtke ili ustanove.
    2.  **Metapodaci za davatelja identiteta Enterprise će biti navedena pomoću**, odaberite **Datoteku**.
    3.  Da biste prenijeli metapodataka preuzetu datoteku, kliknite **Odabir datoteke**.
    4.  Kliknite **Postavi davatelja identiteta**.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-arcgis-tutorial/IC784746.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u ArcGIS, oni mora biti dodijeljena u ArcGIS.  
U slučaju ArcGIS, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se u klijent **ArcGIS** .

2.  Kliknite **Pozovi članove**.

    ![Pozivanje članova] (./media/active-directory-saas-arcgis-tutorial/IC784747.png "Pozivanje članova")

3.  Odaberite **Dodaj članove automatski bez slanja poruke e-pošte**, a zatim kliknite **Dalje**.

    ![Automatsko dodavanje članova] (./media/active-directory-saas-arcgis-tutorial/IC784748.png "Automatsko dodavanje članova")

4.  Na stranici dijaloški **članova** poduzeti sljedeće korake:

    ![Dodavanje i pregledavanje] (./media/active-directory-saas-arcgis-tutorial/IC784749.png "Dodavanje i pregledavanje")

    1.  Unesite **ime**, **Prezime** i **e-pošte** valjani AAD računa koji želite za dodjelu resursa.
    2.  Kliknite **Dodaj, a zatim pregledajte**.

5.  Pregledajte podatke koje ste unijeli, a zatim kliknite **Dodaj članove**.

    ![Dodavanje člana] (./media/active-directory-saas-arcgis-tutorial/IC784750.png "Dodavanje člana")

>[AZURE.NOTE] Možete koristiti bilo koji drugi ArcGIS korisnički račun alate za stvaranje ili API-ji nudi ArcGIS dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-arcgis-perform-the-following-steps"></a>Da biste korisnicima dodijelili ArcGIS, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **ArcGIS **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-arcgis-tutorial/IC784751.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-arcgis-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
