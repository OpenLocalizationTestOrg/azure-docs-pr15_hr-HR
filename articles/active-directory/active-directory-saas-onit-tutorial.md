<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Onit | Microsoft Azure" 
    description="Saznajte kako koristiti Onit s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-onit"></a>Praktični vodič: Azure Active Directory Integracija s Onit
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Onit.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Onit jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Onit će moći znak u aplikaciju na vašem Onit web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Onit
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-onit-tutorial/IC791166.png "Scenarij")
##<a name="enabling-the-application-integration-for-onit"></a>Omogućivanje integraciju aplikacija za Onit
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Onit.

###<a name="to-enable-the-application-integration-for-onit-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Onit, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-onit-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-onit-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-onit-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-onit-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Onit**.

    ![Galerija aplikacije] (./media/active-directory-saas-onit-tutorial/IC791167.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Onit**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Onit] (./media/active-directory-saas-onit-tutorial/IC795325.png "Onit")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Onit sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za Onit potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).
  
Aplikacija Onit očekuje SAML assertions u određenom obliku, koje je potrebno da biste dodali prilagođeni atribut mapiranja konfiguraciju **saml tokena atribute** .  
Sljedeća slika prikazuje primjer za to.

![Jedinstvenu prijavu] (./media/active-directory-saas-onit-tutorial/IC791168.png "Jedinstvenu prijavu")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici integraciju aplikacije **Onit** na izborniku na vrhu kliknite **atribute** da biste otvorili dijaloški okvir **SAML tokena atribute** .

    ![Atributi] (./media/active-directory-saas-onit-tutorial/IC791169.png "Atributi")

2.  Da biste dodali mapiranja obavezan atribut, poduzmite sljedeće korake:

    
  	|Naziv atributa|Vrijednost atributa|
  	|---|---|
  	|ime|User.userprincipalname|
  	|e-pošte|User.Mail|

    1.  Za svaki redak podatke iz gornje tablice, kliknite **Dodaj korisnika atribut**.
    2.  U tekstni okvir **Naziv atributa** upišite naziv atributa koji se prikazuju za tom retku.
    3.  Na popisu **Atributa vrijednosti** odaberite vrijednost atributa koji se prikazuju za tom retku.
    4.  Kliknite **dovrši**.

3.  Kliknite **Primijeni promjene**.

4.  U pregledniku kliknite **natrag** da biste ponovno otvorite dijaloški okvir za **Brzo pokretanje** .

5.  Kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-onit-tutorial/IC791170.png "Konfiguriranje jedinstvenu prijavu")

6.  Na stranici **kako biste željeli korisnika da biste se prijavili Onit** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-onit-tutorial/IC791171.png "Konfiguriranje jedinstvenu prijavu")

7.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Onit na URL za prijavu** upišite URL koji se koristi korisnika da biste se prijavili aplikaciju Onit (npr.: "*https://ms-sso-test.onit.com*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-onit-tutorial/IC791172.png "Konfiguriranje URL adresa Web App")

8.  Na stranici **Konfiguracija jedinstvenu prijavu na Onit** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-onit-tutorial/IC791173.png "Konfiguriranje jedinstvenu prijavu")

9.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Onit kao administrator.

10. Na izborniku na vrhu kliknite **Administracija**.

    ![Administracija] (./media/active-directory-saas-onit-tutorial/IC791174.png "Administracija")

11. Kliknite **Uređivanje Corporation**.

    ![Uređivanje Corporation] (./media/active-directory-saas-onit-tutorial/IC791175.png "Uređivanje Corporation")

12. Kliknite karticu **Sigurnost** .

    ![Informacije o tvrtki za uređivanje] (./media/active-directory-saas-onit-tutorial/IC791176.png "Informacije o tvrtki za uređivanje")

13. Na kartici **Sigurnost** poduzeti sljedeće korake:

    ![Jedinstvenu prijavu] (./media/active-directory-saas-onit-tutorial/IC791177.png "Jedinstvenu prijavu")

    1.  Kao **Strategije za provjeru autentičnosti**, odaberite **jedinstvenu prijavu i lozinku**.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Onit** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **Idp odredišni URL** .
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Onit** kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **URL odjavite Idp** .
    4.  Kopirajte vrijednost **otisak prsta** iz izvezene potvrde, a zatim je zalijepite u tekstni okvir **Certifikata Idp otiska prsta (SHA1)** .  

        >[AZURE.TIP] Dodatne informacije potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

    5.  Kao **SSO vrsta**, odaberite **SAML**.
    6.  U tekstnom okviru **tekst gumba prijava SSO** upišite tekst gumb koji vam se sviđa.
    7.  Odaberite **Prijava uz SSO: obavezno za sljedeće domene/korisnike**, unesite adresu e-pošte testnih korisnika u povezani tekstni okvir, a zatim kliknite **Ažuriraj**.
        ![Uređivanje Corporation] (./media/active-directory-saas-onit-tutorial/IC791178.png "Uređivanje Corporation")

14. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-onit-tutorial/IC791179.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Onit, oni mora biti dodijeljena u Onit.  
U slučaju Onit, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se web-mjesto tvrtke **Onit** kao administrator.

2.  Kliknite **Dodaj korisnika**.

    ![Administracija] (./media/active-directory-saas-onit-tutorial/IC791180.png "Administracija")

3.  Na stranici dijaloški okvir **Dodavanje korisnika** , poduzmite sljedeće korake:

    ![Dodavanje korisnika] (./media/active-directory-saas-onit-tutorial/IC791181.png "Dodavanje korisnika")

    1.  Upišite **ime** i **Adresu e-pošte** valjan račun za AAD želite Dodjela resursa u povezani tekstni okviri.
    2.  Kliknite **Stvori**.  

        >[AZURE.NOTE] Vlasnik računa će primiti poruku e-pošte uključujući vezu da biste potvrdili račun prije postaje aktivna.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Onit korisnički račun alate za stvaranje ili API-ji nudi Onit dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-onit-perform-the-following-steps"></a>Da biste korisnicima dodijelili Onit, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Onit **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-onit-tutorial/IC791182.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-onit-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).