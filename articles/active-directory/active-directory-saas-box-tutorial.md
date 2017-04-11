<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s okvirom | Microsoft Azure" 
    description="Saznajte kako koristiti okvir s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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




#<a name="tutorial-azure-active-directory-integration-with-box"></a>Praktični vodič: Azure Active Directory Integracija s okvirom


  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i okvir.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Testiranje klijenta u okviru
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili okvir će moći znak u aplikaciju na vaše okvir web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za okvir
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje korisnika i grupa dodjele resursa
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-box-tutorial/IC769537.png "Scenarij")



##<a name="enabling-the-application-integration-for-box"></a>Omogućivanje integraciju aplikacija za okvir
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za okvir.

###<a name="to-enable-the-application-integration-for-box-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za okvir, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-box-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-box-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-box-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-box-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **okvir**.

    ![Galerija aplikacije] (./media/active-directory-saas-box-tutorial/IC701023.png "Galerija aplikacije")

7.  U oknu s rezultatima, potvrdite **okvir**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Okvir] (./media/active-directory-saas-box-tutorial/IC701024.png "Okvir")



##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti okvira sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol. Ovaj postupak, dio su potrebne da biste prenijeli metapodataka na Box.com.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **okvir** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-box-tutorial/IC769538.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili okviru** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-box-tutorial/IC769539.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni **Okvir klijentu URL** unesite URL okvir klijenta (npr.: https://<mydomainname>. box.com), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-box-tutorial/IC669826.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na okvir** da biste preuzeli metapodatka, kliknite **Preuzimanje metapodatke**, a zatim podatkovnu datoteku na lokalno računalo.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-box-tutorial/IC669824.png "Konfiguriranje jedinstvenu prijavu")

5.  Proslijedi te datoteke metapodataka za podršku. Tim potrebama podršku konfigurira jedinstvenu prijavu umjesto vas.

6.  Odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-box-tutorial/IC769540.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti dodjeljivanje korisničkih računa servisa Active Directory u okvir.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1. Azure klasični portalu na stranici za integraciju aplikacije **okvir** kliknite **Konfiguriraj dodjele resursa korisnika** da biste otvorili dijaloški okvir **Konfiguriranje dodjele resursa korisnika** . 

    ![Omogućivanje automatskog korisnika dodjele resursa] (./media/active-directory-saas-box-tutorial/IC769541.png "Omogućivanje automatskog korisnika dodjele resursa")

2. Na stranici za **Omogućivanje korisnika dodjele resursa za** dijaloški okvir kliknite **Omogući dodjeljivanje korisnika**. 

    ![Omogućivanje automatskog korisnika dodjele resursa] (./media/active-directory-saas-box-tutorial/IC769544.png "Omogućivanje automatskog korisnika dodjele resursa")

3. Na stranici **prijavite se da biste dodijelili pristup okvir** unesite potrebne vjerodajnice, a zatim **ovlasti**. 

    ![Omogućivanje automatskog korisnika dodjele resursa] (./media/active-directory-saas-box-tutorial/IC769546.png "Omogućivanje automatskog korisnika dodjele resursa")


4. Kliknite **Odobri pristup okvir** da biste autorizirali ovaj postupak i vratili se na portal Azure klasični. 

    ![Omogućivanje automatskog korisnika dodjele resursa] (./media/active-directory-saas-box-tutorial/IC769549.png "Omogućivanje automatskog korisnika dodjele resursa")


5. Na stranici s **Mogućnostima za dodjelu resursa** potvrdne okvire **vrste objekata za dodjelu resursa** omogućuju vam odabir hoće li Grupiraj objekte dodjeli okvir uz korisničke objekte.  U odjeljku "Dodjela korisnicima i grupama" ispod dodatne informacije.


6. Da biste dovršili konfiguraciju, kliknite gumb dovršeno. 

    ![Omogućivanje automatskog korisnika dodjele resursa] (./media/active-directory-saas-box-tutorial/IC769551.png "Omogućivanje automatskog korisnika dodjele resursa")



##<a name="assigning-a-test-user"></a>Dodjela korisnika test
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-box-perform-the-following-steps"></a>Da biste korisnicima dodijelili okvir, poduzmite sljedeće korake:

1. Azure klasični portalu stvorite testnom računu.

2. Na stranici za integraciju aplikacije **okvir **kliknite **dodijeliti korisnicima**. 

    ![Dodjela korisnika] (./media/active-directory-saas-box-tutorial/IC769552.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele. 

    ![Da] (./media/active-directory-saas-box-tutorial/IC767830.png "Da")
  
Sada treba pričekajte 10 minuta i provjerite da je sinkronizirana račun u okvir.

Kao prvi korak za potvrdu, možete provjeriti status dodjele resursa tako da kliknete nadzorne ploče u D na stranici za integraciju okvir aplikacije na portalu Azure klasični.

![Nadzorna ploča] (./media/active-directory-saas-box-tutorial/IC769553.png "Nadzorna ploča")

Korisnik uspješno dovršena dodjeljivanje ciklusa označena je povezane status:

![Stanje integracije] (./media/active-directory-saas-box-tutorial/IC769555.png "Stanje integracije")


U klijentu za okvir sinkronizirani korisnici navedene su u odjeljku **Upravljani korisnika** u **Administracijskoj konzoli sustava**.

![Stanje integracije] (./media/active-directory-saas-box-tutorial/IC769556.png "Stanje integracije")


##<a name="assigning-users-and-groups"></a>Dodjeljivanje korisnika i grupa

U **okvir > korisnici i grupe** kartica na portalu za Azure klasični omogućuje vam da odredite koji korisnici i grupe trebali biste dobiti pristup okvir. Dodjelu korisnika ili grupu uzrokuje dogoditi sljedeće:

* Azure AD omogućuje dodijeljenog korisnika (bilo izravno dodjele ili članstvo u grupi) za provjeru autentičnosti okvir. Ako korisnik je dodijeljen, zatim Azure AD ne dopuštaju ih da biste se prijavili okvir i će vratiti pogrešku na stranicu za prijavu u Azure AD.

* Pločicu aplikacije za okvir se dodaje na korisnikovu [pokretač aplikacija](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

* Ako je omogućeno automatsko dodjele resursa, dodijeljenih korisnicima i/ili grupe dodaju dodjele resursa red da biste automatski dodjeli.

    * Ako samo korisnički objekti su konfigurirana za biti dodijeljeni resursi, pa se sve korisnike izravno dodijeljenu smještaju u redu čekanja za dodjelu resursa, a svi korisnici koji su članovi grupe dodijeljene će smjestiti u redu čekanja za dodjelu resursa. 
    
    * Ako su Grupiraj objekte konfiguriran za biti dodijeljena, svi objekti dodijeljenog grupi su dodjeli okvir, kao i sve korisnike koji su članovi te grupe. Članstvo grupe i korisniku zadržavaju se nakon koje se zapisuju u okvir.
    
Možete koristiti u **atribute > jedinstvenu prijavu** kartice da biste konfigurirali koje korisničke atribute (ili zahtjevima) predstavljaju okvir tijekom provjere autentičnosti utemeljene na SAML i **atribute > Provisioning** kartica konfigurirati kako se korisnika i grupa atribute tijeka iz Azure AD okvir tijekom operacije za dodjelu resursa. Potražite u člancima ispod dodatne informacije.


## <a name="additional-resources"></a>Dodatni resursi

* [Prilagodba zahtjevima izdavanja u SAML tokena](active-directory-saml-claims-customization.md)
* [Dodjeljivanje: Prilagodba mapiranja atributa](active-directory-saas-customizing-attribute-mappings.md)
* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)
