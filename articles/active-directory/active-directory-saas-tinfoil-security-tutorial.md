<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Tinfoil sigurnost | Microsoft Azure"
    description="Saznajte kako koristiti Tinfoil sigurnost s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!." 
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

#<a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Praktični vodič: Azure Active Directory Integracija s Tinfoil sigurnošću
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Tinfoil sigurnost.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na sigurnost Tinfoil jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Tinfoil sigurnost će moći znak u aplikaciju na vašem Tinfoil sigurnost web-mjesto tvrtke (znak identiteta davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za sigurnost Tinfoil
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-tinfoil-security-tutorial/IC798965.png "Konfiguriranje jedinstvenu prijavu")

##<a name="enabling-the-application-integration-for-tinfoil-security"></a>Omogućivanje integraciju aplikacija za sigurnost Tinfoil
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za sigurnost Tinfoil.

###<a name="to-enable-the-application-integration-for-tinfoil-security-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za sigurnost Tinfoil, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-tinfoil-security-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-tinfoil-security-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-tinfoil-security-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-tinfoil-security-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Tinfoil sigurnost**.

    ![Galerija aplikacije] (./media/active-directory-saas-tinfoil-security-tutorial/IC798966.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Tinfoil sigurnost**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Sigurnost tinfoil] (./media/active-directory-saas-tinfoil-security-tutorial/IC802771.png "Sigurnost tinfoil")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti sigurnosti Tinfoil sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za sigurnost Tinfoil potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Tinfoil sigurnost** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-tinfoil-security-tutorial/IC798967.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Tinfoil sigurnost** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-tinfoil-security-tutorial/IC798968.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Tinfoil sigurnost odgovor URL** unesite URL Tinfoil sigurnost pridruživanju potrošača servisa (ACS) (npr.: "*https://www.tinfoilsecurity.com/saml/consume*", a zatim kliknite **Dalje**.

    >[AZURE.NOTE] Trebali biste moći dohvaćanje URL-a ACS iz Tinfoil sigurnost metapodataka (https://www.tinfoilsecurity.com/saml/metadata).

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-tinfoil-security-tutorial/IC798969.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Tinfoil sigurnost** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno kao **c:\\Tinfoil Security.cer**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-tinfoil-security-tutorial/IC798970.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Tinfoil sigurnost kao administrator.

6.  Na alatnoj traci na vrhu kliknite **Moj račun**.

    ![Nadzorna ploča] (./media/active-directory-saas-tinfoil-security-tutorial/IC798971.png "Nadzorna ploča")

7.  Kliknite **Sigurnost**.

    ![Sigurnost] (./media/active-directory-saas-tinfoil-security-tutorial/IC798972.png "Sigurnost")

8.  Konfiguriranje stranice **Jedinstvenu prijavu** poduzeti sljedeće korake:

    ![Jedinstvenu prijavu] (./media/active-directory-saas-tinfoil-security-tutorial/IC798973.png "Jedinstvenu prijavu")

    1.  Odaberite **Omogući SAML**.
    2.  Kliknite **Ručna konfiguracija**.
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na sigurnost Tinfoil** kopirajte vrijednost **SAML SSO URL** i pa ih zalijepite u tekstni okvir **URL SAML objavu** .
    4.  Kopirajte vrijednost **otisak prsta** iz izvezene potvrde i pa ih zalijepite u tekstni okvir **Otiska prsta SAML certifikata** .  

        >[AZURE.TIP] Dodatne informacije potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

    5.  Kopirajte **identifikacijskog Broja za račun**.
    6.  Kliknite **Spremi**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-tinfoil-security-tutorial/IC798974.png "Konfiguriranje jedinstvenu prijavu")

10. Na izborniku na vrhu kliknite **atribute** da biste otvorili dijaloški okvir **SAML tokena atribute** .

    ![Atributi] (./media/active-directory-saas-tinfoil-security-tutorial/IC795920.png "Atributi")

11. Da biste dodali mapiranja obavezan atribut, poduzmite sljedeće korake:

    ![Atributi] (./media/active-directory-saas-tinfoil-security-tutorial/IC798975.png "Atributi")

    1.  Kliknite **Dodavanje korisnika atribut**.
    2.  U tekstni okvir **Naziv atributa** upišite **accountid**.
    3.  U tekstni okvir **Vrijednost atributa** zalijepite vrijednost ID računa koji ste kopirali u prethodnom odjeljku.
    4.  Kliknite **dovrši**.

12. Kliknite **Primijeni promjene**.

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Tinfoil sigurnost, oni mora biti dodijeljena u Tinfoil sigurnost.  
U slučaju Tinfoil sigurnosti, dodjeljivanje jest zadatak za ručno.

###<a name="to-get-a-user-provisioned-perform-the-following-steps"></a>Da biste dobili dodjeli korisniku, učinite sljedeće:

1.  Ako je korisnik član račun Enterprise, morate je obratite se timu za podršku Tinfoil sigurnost da biste stvorili korisnički račun.

2.  Ako je korisnik redoviti korisnik SaaS Tinfoil sigurnost, zatim korisnika možete dodati na collaborator svih korisnika web-mjesta. Time se pokreće postupak da biste poslali pozivnicu na navedeni e-pošte za stvaranje novog korisničkog računa Tinfoil sigurnost.

>[AZURE.NOTE] Možete koristiti bilo koje druge Tinfoil sigurnost korisnički račun alate za stvaranje ili API-ji nudi Tinfoil sigurnost dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-tinfoil-security-perform-the-following-steps"></a>Da biste korisnicima dodijelili Tinfoil sigurnost, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Tinfoil sigurnost **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-tinfoil-security-tutorial/IC798976.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-tinfoil-security-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).