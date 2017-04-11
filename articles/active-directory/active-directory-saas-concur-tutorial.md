<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Concur | Microsoft Azure" 
    description="Saznajte kako koristiti Concur s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-concur"></a>Praktični vodič: Azure Active Directory Integracija s Concur  


Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Concur.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Klijenta u Concur

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Concur
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-concur-tutorial/IC769766.png "Scenarij")

>[AZURE.NOTE] Konfiguracija pretplate Concur za vanjsko SSO putem SAML je zasebnom zadataka koje morate se obratiti Concur za izvođenje.

##<a name="enabling-the-application-integration-for-concur"></a>Omogućivanje integraciju aplikacija za Concur

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Concur.

###<a name="to-enable-the-application-integration-for-concur-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Concur, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-concur-tutorial/IC700993.png "Active Directory")

2.  Iz **imenika** popisa, odaberite direktorija za koju želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-concur-tutorial/IC700994.png "Aplikacija")

4.  Da biste otvorili **Galeriju aplikaciju**, kliknite **Dodaj programa aplikacija**, a zatim **Dodavanje aplikacije za tvrtke ili ustanove da biste koristili**.

    ![Što želite učiniti?] (./media/active-directory-saas-concur-tutorial/IC700995.png "Što želite učiniti?")

5.  U **okvir za pretraživanje**upišite **Concur**.

    ![Galerija aplikacije] (./media/active-directory-saas-concur-tutorial/IC721727.png "Galerija aplikacije")

6.  U oknu s rezultatima odaberite **Concur**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Concur] (./media/active-directory-saas-concur-tutorial/IC721728.png "Concur")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Concur sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

>[AZURE.NOTE] Konfiguracija pretplate Concur za vanjsko SSO putem SAML je zasebnom zadataka koje morate se obratiti Concur za izvođenje.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Concur **kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-concur-tutorial/IC769767.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Concur** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-concur-tutorial/IC769768.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Concur URL za prijavu** upišite concur klijentu prijavu URL, a zatim kliknite **Dalje**: 

    ![Konfiguriranje prijavite se u URL-a] (./media/active-directory-saas-concur-tutorial/IC769769.png "Konfiguriranje prijavite se u URL-a")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Concur** poduzeti sljedeće korake.

    ![Konfiguriranje prijavite se u URL-a] (./media/active-directory-saas-concur-tutorial/IC769770.png "Konfiguriranje prijavite se u URL-a")

    1.  Kliknite Preuzimanje metapodatke, a zatim sigurnih podatkovnu datoteku na računalo.
    2.  Obratite se timu za podršku Concur da biste konfigurirali SSO za vaš klijent.
    3.  Odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .  

    >[AZURE.NOTE] Konfiguracija pretplate Concur za vanjsko SSO putem SAML je zasebnom zadataka koje morate se obratiti Concur za izvođenje.

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Cilj ovaj odjeljak je Strukturiranje kako omogućiti dodjeljivanje korisničkih računa servisa Active Directory za Concur.

Da biste omogućili aplikacije u troškovima uslugu, postoje mora biti proper postavljanje i korištenje profila administrator servisa Web. Ne jednostavno dodajte WS administratorske uloge u postojeći profil administrator koju koristite za po & T funkcija administratora.

Concur savjetnika ili administrator klijenta mora stvoriti distinct administratora web-usluge profila i administrator klijenta mora koristi ovaj profil za administratora Web Services funkcije (primjerice aplikacija za omogućivanje). Ove profile mora biti zadržane neovisan administrator klijenta dnevnih po & T administrator profila (administrator profila po & T treba imati ulogu WSAdmin koja je dodijeljena).

Kada stvorite profil koji će se koristiti za omogućivanje aplikacije, unesite ime administratora klijenta u korisničkih profila polja. To će dodijeliti vlasništvo profil. Nakon stvaranja na profila klijenta morate se prijaviti pomoću ovaj profil kliknite gumb "*Omogući*" za aplikaciju Partner u izborniku web-servisa.

Iz sljedećih razloga tu akciju nije računajući s profilom koji se koriste za normalni po & T administraciju.

1.  Klijent mora biti onaj koji se klikne "*da*" u prozoru dijaloški okvir koji se prikazuje kada je omogućen aplikacije. U ovom kliknite priznaje klijent će naznačiti pristupati svojim podacima tako da možete ili partnera ne kliknite taj gumb da bi aplikacija partnera.
2.  Ako administrator klijenta koji je omogućen aplikacije pomoću administrator po & T profila napusti tvrtku (rezultira profila koji se neaktivno), aplikacija omogućena pomoću da profila neće funkcionirati aplikaciju dok se ne Omogući s drugom aktivnom profilu WS administrator. To je Zašto koje morate stvoriti distinct WS administrator profile.
3.  Ako administrator Ostavi tvrtke, naziv povezane profil WS administrator mogu mijenjati administratoru zamjenski po želji bez utjecaja na aplikaciju omogućeno jer je taj profil potreban neaktivno

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se u klijent **Concur** .

2.  Na izborniku **Administracija** odaberite **Web-servisa**.

    ![Concur klijenta] (./media/active-directory-saas-concur-tutorial/IC721729.png "Concur klijenta")

3.  Na lijevoj strani, u oknu **Web-servisi** odaberite **Omogući aplikaciju partnera**.

    ![Omogućivanje aplikacije Partner] (./media/active-directory-saas-concur-tutorial/IC721730.png "Omogućivanje aplikacije Partner")

4.  Omogućivanje popisa u **Aplikaciji,** odaberite **Azure Active Directory**, a zatim **Omogući**.

    ![Microsoft Azure Active Directory] (./media/active-directory-saas-concur-tutorial/IC721731.png "Microsoft Azure Active Directory")

5.  Kliknite **da** da biste zatvorili dijaloški okvir **Potvrdite akciju** .

    ![Potvrdite akciju] (./media/active-directory-saas-concur-tutorial/IC721732.png "Potvrdite akciju")

6.  Azure klasični portalu odaberite **Concur** s popisa aplikacija da biste otvorili dijaloški okvir stranicu **Concur** .

7.  Da biste otvorili dijaloški okvir stranicu **Konfiguriranje dodjele resursa korisnika** , kliknite **Konfiguriraj korisnika dodjele resursa**.

8.  Unesite korisničko ime i lozinku administratora Concur, a zatim kliknite **Dalje**.

9.  Da biste dovršili konfiguraciju, na stranici za **potvrdu** kliknite gumb **Dovršeno** .

Možete sada stvoriti testnom računu, pričekajte 10 minuta i provjerite je li sinkronizirani da račun Concur.
##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-concur-perform-the-following-steps"></a>Da biste korisnicima dodijelili Concur, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Concur **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-concur-tutorial/IC769771.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-concur-tutorial/IC767830.png "Da")

Sada treba pričekajte 10 minuta i provjerite je li sinkronizirani da račun Concur.

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
