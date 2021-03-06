<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Panorama9 | Microsoft Azure" 
    description="Saznajte kako koristiti Panorama9 s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Praktični vodič: Azure Active Directory Integracija s Panorama9
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Panorama9.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Panorama9 jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Panorama9 će moći znak u aplikaciju na vašem Panorama9 web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Panorama9
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-panorama9-tutorial/IC790016.png "Scenarij")
##<a name="enabling-the-application-integration-for-panorama9"></a>Omogućivanje integraciju aplikacija za Panorama9
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Panorama9.

###<a name="to-enable-the-application-integration-for-panorama9-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Panorama9, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-panorama9-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-panorama9-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-panorama9-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-panorama9-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Panorama9**.

    ![Galerija aplikacije] (./media/active-directory-saas-panorama9-tutorial/IC790017.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Panorama9**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Panorama9] (./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Panorama9 sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za Panorama9 potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Panorama9** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-panorama9-tutorial/IC790019.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Panorama9** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-panorama9-tutorial/IC790020.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Panorama9 na URL za prijavu** upišite URL koriste vaši korisnici da biste se prijavili Panorama9 (npr.: "*https://dashboard.panorama9.com/saml/access/3262*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-panorama9-tutorial/IC790021.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Panorama9** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**, a zatim ga spremiti lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-panorama9-tutorial/IC790022.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Panorama9 kao administrator.

6.  Na alatnoj traci na vrhu kliknite **Upravljanje**, a zatim **proširenja**.

    ![Proširenja] (./media/active-directory-saas-panorama9-tutorial/IC790023.png "Proširenja")

7.  U dijaloškom okviru **proširenja** kliknite **Jedinstvenu prijavu**.

    ![Jedinstvenu prijavu] (./media/active-directory-saas-panorama9-tutorial/IC790024.png "Jedinstvenu prijavu")

8.  U odjeljku **Postavke** učinite sljedeće:

    ![Postavke] (./media/active-directory-saas-panorama9-tutorial/IC790025.png "Postavke")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Panorama9** kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir **URL davatelja identiteta** .
    2.  Kopirajte vrijednost **otisak prsta** iz izvezene potvrde i pa ih zalijepite u tekstni okvir za **potvrdu otiska prsta** .  

        >[AZURE.TIP]Dodatne informacije potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

    3.  Kliknite **Spremi**.

9.  Na portalu klasični Azure AD odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-panorama9-tutorial/IC790026.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Panorama9, oni mora biti dodijeljena u Panorama9.  
U slučaju Panorama9, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke **Panorama9** kao administrator.

2.  Na izborniku na vrhu kliknite **Upravljanje**, a zatim **korisnika**.

    ![Korisnici] (./media/active-directory-saas-panorama9-tutorial/IC790027.png "Korisnici")

3.  Kliknite **+**.

4.  U odjeljku podataka korisnik poduzeti sljedeće korake:

    ![Korisnici] (./media/active-directory-saas-panorama9-tutorial/IC790028.png "Korisnici")

    1.  U tekstni okvir **e-pošte** upišite adresu e-pošte valjani Azure Active Directory korisnika kojem želite da dodjele resursa.
    2.  Kliknite **Spremi**.

>[AZURE.NOTE]Možete koristiti bilo koji drugi Panorama9 korisnički račun alate za stvaranje ili API-ji nudi Panorama9 dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-panorama9-perform-the-following-steps"></a>Da biste korisnicima dodijelili Panorama9, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Panorama9** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-panorama9-tutorial/IC790029.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-panorama9-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).