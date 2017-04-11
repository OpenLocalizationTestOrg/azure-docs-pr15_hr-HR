<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Work.com | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory Work.com da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!." 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-workcom"></a>Praktični vodič: Azure Active Directory Integracija s Work.com
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Work.com.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Work.com jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča AAD korisnika kojemu ste dodijelite Work.com pristup bit će moći jedan znak u aplikaciju na vašem Work.com web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Work.com
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-work-com-tutorial/IC794105.png "Scenarij")

##<a name="enabling-the-application-integration-for-workcom"></a>Omogućivanje integraciju aplikacija za Work.com
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Work.com.

###<a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Work.com, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-work-com-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-work-com-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-work-com-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-work-com-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Work.com**.

    ![Galerija aplikacije] (./media/active-directory-saas-work-com-tutorial/IC794106.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Work.com**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Work.com] (./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Work.com sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Ovaj postupak, dio su potrebne da biste prenijeli certifikat na Work.com.com.

>[AZURE.NOTE] Da biste konfigurirali jedinstvenu prijavu, morate postaviti prilagođeni naziv domene Work.com još. Morate definirati barem naziv domene, testiranje naziva domene i implementirati za cijelu tvrtku ili ustanovu.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Prijavite se u klijent Work.com kao administrator.

2.  Idite na **Postavljanje**.

    ![Postavljanje] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Postavljanje")

3.  U lijevom navigacijskom oknu u odjeljku **administriranje** kliknite **Upravljanje domenom** proširite odjeljak povezani, a zatim **Moje domene** da biste otvorili stranicu **Moje domene** . 

    ![Domene] (./media/active-directory-saas-work-com-tutorial/IC767825.png "Domene")

4.  Da biste potvrdili da vaše domene je ispravno, provjerite je li u "**Korak 4 implementiran korisnicima**" i pregled "**Postavke za Moje domene**".

    ![Doman implementiran korisniku] (./media/active-directory-saas-work-com-tutorial/IC784377.png "Doman implementiran korisniku")

5.  U prozoru preglednika drugoj web prijaviti na portal Azure klasični.

6.  Na stranici za integraciju aplikacije **Work.com **kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-work-com-tutorial/IC794109.png "Konfiguriranje jedinstvenu prijavu")

7.  Na stranici **kako biste željeli korisnika da biste se prijavili Work.com** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-work-com-tutorial/IC794110.png "Konfiguriranje jedinstvenu prijavu")

8.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Work.com na URL za prijavu** upišite URL koji se koristi korisnika da biste se prijavili aplikaciju Work.com (npr.: " *http://company.my.salesforce.com*"), a zatim kliknite **Dalje**: 

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-work-com-tutorial/IC794111.png "Konfiguriranje URL adresa Web App")

9.  Na stranici **Konfiguracija jedinstvenu prijavu na Work.com** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-work-com-tutorial/IC794112.png "Konfiguriranje jedinstvenu prijavu")

10. Prijavite se u klijent Work.com.

11. Idite na **Postavljanje**.

    ![Postavljanje] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Postavljanje")

12. Proširite izbornik **Sigurnosne kontrole** , a zatim kliknite **Jedan postavke za prijavu**.

    ![Jedan postavke za prijavu] (./media/active-directory-saas-work-com-tutorial/IC794113.png "Jedan postavke za prijavu")

13. Na stranici za dijaloški okvir s **Postavkama za jednu prijave** poduzeti sljedeće korake:

    ![Omogućeno SAML] (./media/active-directory-saas-work-com-tutorial/IC781026.png "Omogućeno SAML")

    1.  Odaberite **SAML omogućena**.
    2.  Kliknite **Novo**.

14. U odjeljku **SAML jedan prijave postavke** poduzeti sljedeće korake:

    ![SAML jedan prijave postavka] (./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML jedan prijave postavka")

    1.  U tekstni okvir **naziv** upišite naziv za konfiguraciju.  

        >[AZURE.NOTE] Pružanje vrijednost za **naziv** automatski se popunjava tekstni okvir **Naziv API -JA** .

    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Work.com** kopirajte vrijednost **Izdavač URL** i pa ih zalijepite u tekstni okvir **izdavač** .
    3.  Da biste prenijeli preuzete certifikata, kliknite **Pregledaj**.
    4.  U tekstni okvir **Id entitet** , upišite **https://salesforce-work.com**.
    5.  Kao **Vrsta identiteta SAML**, odaberite **pridruživanju sadrži ID vanjski pristup iz korisničkom objektu**.
    6.  **Mjesto identiteta SAML**, odaberite **identitet se element NameIdentfier izjave predmet**.
    7.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Work.com** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu davatelja identiteta** .
    8.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Work.com** kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **URL za odjavite davatelja identiteta** .
    9.  Kao **Servisa davatelja pokrenut zahtjev za povezivanje**, odaberite **HTTP Post**.
    10. Kliknite **Spremi**.

15. U Work.com klasični portalom, u lijevom navigacijskom oknu kliknite **Upravljanje domenom** proširite odjeljak povezani, a zatim **Moje domene** da biste otvorili stranicu **Moje domene** . 

    ![Domene] (./media/active-directory-saas-work-com-tutorial/IC794115.png "Domene")

16. Na stranici **Moje domene** u odjeljku **Brendiranje stranicu za prijavu** kliknite **Uređivanje**.

    ![Brendiranje stranicu za prijavu] (./media/active-directory-saas-work-com-tutorial/IC767826.png "Brendiranje stranicu za prijavu")

17. Na stranici **Brendiranje stranicu za prijavu** u odjeljku **Servis za provjeru autentičnosti** , prikazuje se naziv **SAML SSO postavke** . Odaberite ga, a zatim kliknite **Spremi**.

    ![Brendiranje stranicu za prijavu] (./media/active-directory-saas-work-com-tutorial/IC784366.png "Brendiranje stranicu za prijavu")

18. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-work-com-tutorial/IC794116.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Za Azure Active Directory da korisnici mogu se prijaviti, oni mora biti dodijeljena da Work.com.  
U slučaju Work.com, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se web-mjesto tvrtke Work.com kao administrator.

2.  Idite na **Postavljanje**.

    ![Postavljanje] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Postavljanje")

3.  Idite na **Upravljanje korisnicima \> korisnika**.

    ![Upravljanje korisnicima] (./media/active-directory-saas-work-com-tutorial/IC784369.png "Upravljanje korisnicima")

4.  Kliknite **novi korisnik**.

    ![Svi korisnici] (./media/active-directory-saas-work-com-tutorial/IC794117.png "Svi korisnici")

5.  U odjeljku uređivanje korisnika poduzeti sljedeće korake:

    ![Uređivanje korisnika] (./media/active-directory-saas-work-com-tutorial/IC794118.png "Uređivanje korisnika")

    1.  Unesite **Prezime**, **Pseudonim**, **e-pošte**, **korisničko ime** i atribute **nadimaka** valjani Azure Active Directory račun koji želite dodjele resursa u povezani tekstni okviri.
    2.  Odaberite **ulogu**, **korisničke licence** i **profila**.
    3.  Kliknite **Spremi**.  

        >[AZURE.NOTE] Vlasnik računa Azure Active Directory će primiti poruku e-pošte uključujući vezu da biste potvrdili račun prije postaje aktivna.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Work.com korisnički račun alate za stvaranje ili API-ji nudi Work.com dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-workcom-perform-the-following-steps"></a>Da biste korisnicima dodijelili Work.com, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije Work.com kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-work-com-tutorial/IC794119.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-work-com-tutorial/IC767830.png "Da")
  
Sada treba pričekajte 10 minuta i provjerite je li sinkronizirani da račun Work.com.com.
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).