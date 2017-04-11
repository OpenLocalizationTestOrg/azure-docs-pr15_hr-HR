<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s NetDocuments | Microsoft Azure" 
    description="Saznajte kako koristiti NetDocuments s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Praktični vodič: Azure Active Directory Integracija s NetDocuments
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i NetDocuments.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   NetDocuments klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili NetDocuments će moći znak u aplikaciju na vašem NetDocuments web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za NetDocuments
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Scenarij")
##<a name="enabling-the-application-integration-for-netdocuments"></a>Omogućivanje integraciju aplikacija za NetDocuments
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za NetDocuments.

###<a name="to-enable-the-application-integration-for-netdocuments-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za NetDocuments, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **NetDocuments**.

    ![Galerija aplikacije] (./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **NetDocuments**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![NetDocuments] (./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti NetDocuments sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za NetDocuments potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **NetDocuments** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili NetDocuments** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , učinite sljedeće:

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-netdocuments-tutorial/IC795045.png "Konfiguriranje URL adresa Web App")

    1.  U tekstni okvir **Prijavite se na URL** unesite URL koriste vaši korisnici da biste se prijavili aplikaciju NetDocuments (npr.: "*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*").
    2.  U tekstni okvir **URL odgovor NetDocuments** upišite ste upisali u jednaku vrijednost u tekstni okvir za **Prijavu na URL-a** .  

        >[AZURE.NOTE]Možete pronaći ispravnu vrijednost na kraju dijaloški okvir **Omogućila vanjski pristup identitetu** (pogledajte snimka koraka 9).

    3.  Kliknite **Dalje**

4.  Na stranici **Konfiguracija jedinstvenu prijavu na NetDocuments** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke NetDocuments kao administrator.

6.  Idite na **administrator**.

7.  Kliknite **Dodaj i Ukloni korisnike i grupe**.

    ![Spremište] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Spremište")

8.  Kliknite **Konfiguriraj Napredne mogućnosti provjere autentičnosti**.

    ![Konfiguriranje Napredne mogućnosti provjere autentičnosti] (./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Konfiguriranje Napredne mogućnosti provjere autentičnosti")

9.  U dijaloškom okviru **Omogućila vanjski pristup identitetu** , učinite sljedeće:

    ![Pridružene Identitty] (./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Pridružene Identitty")

    1.  Kao **Vrsta poslužitelja za vanjski identiteta**, odaberite **Active Directory Federation Services**.
    2.  Kliknite **Odabir datoteke**za prijenos datoteke preuzete metapodataka.
    3.  Kliknite **u redu**.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u NetDocuments, oni mora biti dodijeljena u NetDocuments.  
U slučaju NetDocuments, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Cvijeća na web-mjesto tvrtke **NetDocuments** kao administrator.

2.  Na izborniku na vrhu kliknite **administrator**.

    ![Administrator] (./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Administrator")

3.  Kliknite **Dodaj i Ukloni korisnike i grupe**.

    ![Spremište] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Spremište")

4.  U tekstni okvir **Adresa e-pošte** upišite adresu e-pošte valjan račun Azure Active Directory za dodjelu resursa, a zatim kliknite **Dodaj korisnika**.

    ![Adresa e-pošte] (./media/active-directory-saas-netdocuments-tutorial/IC795053.png "Adresa e-pošte")

    >[AZURE.NOTE]Vlasnik računa Azure Active Directory će primiti poruku e-pošte koja sadrži vezu da biste potvrdili račun prije postaje aktivna.

>[AZURE.NOTE]Možete koristiti bilo koji drugi NetDocuments korisnički račun alate za stvaranje ili API-ji nudi NetDocuments dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-netdocuments-perform-the-following-steps"></a>Da biste korisnicima dodijelili NetDocuments, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **NetDocuments **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).