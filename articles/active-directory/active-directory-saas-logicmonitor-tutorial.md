<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s LogicMonitor | Microsoft Azure" 
    description="Saznajte kako koristiti LogicMonitor s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Praktični vodič: Azure Active Directory Integracija s LogicMonitor
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i LogicMonitor.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   LogicMonitor klijenta
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za LogicMonitor
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Scenarij")
##<a name="enabling-the-application-integration-for-logicmonitor"></a>Omogućivanje integraciju aplikacija za LogicMonitor
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za LogicMonitor.

###<a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za LogicMonitor, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **logicmonitor**.

    ![Galerija aplikacije] (./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **LogicMonitor**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![LogicMonitor] (./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti LogicMonitor sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **LogicMonitor **kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili LogicMonitor** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Prijavite se na URL** unesite URL koriste vaši korisnici da biste se prijavili LogicMonitor \(e, g,: "*http://company.logicmonitor.com*"\), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na LogicMonitor** kliknite **Preuzmi metapodataka**i spremite je na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Konfiguriranje jedinstvenu prijavu")

5.  Prijavite se na web-mjesto tvrtke **LogicMonitor** kao administrator.

6.  Na izborniku na vrhu kliknite **Postavke**.

    ![Postavke] (./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Postavke")

7.  U u navigaciju aplikacije za Outlook na lijevoj strani, kliknite **Jedinstvenu prijavu**

    ![Jedinstvenu prijavu] (./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Jedinstvenu prijavu")

8.  U odjeljku **postavke za jedinstvenu prijavu (SSO)** , učinite sljedeće:

    ![Jedan postavke za prijavu] (./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Jedan postavke za prijavu")

    1.  Odaberite **Omogući jedinstvenu prijavu**.
    2.  Kao **Zadani Dodjela uloge**, odaberite **samo za čitanje**.
    3.  Otvaranje datoteke preuzete metapodataka u blok za pisanje, a zatim zalijepite sadržaj datoteke u tekstni okvir **Metapodataka davatelja identiteta** .
    4.  Kliknite **Spremi promjene**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Za AAD da korisnici mogu se prijaviti, oni mora biti dodijeljena da aplikacije LogicMonitor pomoću njihova korisničkog imena Azure Active Directory.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke LogicMonitor kao administrator.

2.  Na izborniku na vrhu, kliknite **Postavke**, a zatim kliknite **uloge i korisnicima**.

    ![Uloge i korisnika] (./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Uloge i korisnika")

3.  Kliknite **Dodaj**.

4.  U odjeljku **Dodavanje računa** učinite sljedeće:

    ![Dodavanje računa] (./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Dodavanje računa")

    1.  Unesite **korisničko ime**, **e-pošte**, **lozinku** i **ponovno upišite lozinku** vrijednosti Azure Active Directory korisnika koje želite dodjele resursa u povezane tekstne okvire.
    2.  Odaberite **uloge**, **Prikaz dozvola** i **Status**.
    3.  Kliknite **Pošalji**.

>[AZURE.NOTE]Možete koristiti bilo koji drugi LogicMonitor korisnički račun alate za stvaranje ili API-ji nudi LogicMonitor Dodjela resursa za Azure Active Directory korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>Da biste korisnicima dodijelili LogicMonitor, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **LogicMonitor** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).




