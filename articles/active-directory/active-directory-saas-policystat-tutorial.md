<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s PolicyStat | Microsoft Azure" 
    description="Saznajte kako koristiti PolicyStat s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-policystat"></a>Praktični vodič: Azure Active Directory Integracija s PolicyStat
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i PolicyStat.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   PolicyStat klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili PolicyStat će moći znak u aplikaciju na vašem PolicyStat web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za PolicyStat
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-policystat-tutorial/IC808662.png "Scenarij")
##<a name="enabling-the-application-integration-for-policystat"></a>Omogućivanje integraciju aplikacija za PolicyStat
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za PolicyStat.

###<a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za PolicyStat, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-policystat-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-policystat-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-policystat-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **PolicyStat**.

    ![Galerija aplikacije] (./media/active-directory-saas-policystat-tutorial/IC808627.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **PolicyStat**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![PolicyStat] (./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti PolicyStat sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Aplikacija PolicyStat očekuje SAML assertions u određenom obliku, koje je potrebno da biste dodali prilagođeni atribut mapiranja konfiguraciju **saml tokena atribute** .  
Sljedeća slika prikazuje primjer za to.

![Atributi] (./media/active-directory-saas-policystat-tutorial/IC808628.png "Atributi")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **PolicyStat** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-policystat-tutorial/IC808629.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili PolicyStat** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-policystat-tutorial/IC808630.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje postavki aplikacije** u tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju URL PolicyStat (npr.: *"https://demo-azure.policystat.com"*), a zatim kliknite **Dalje**.

    ![Konfiguriranje postavki aplikacije] (./media/active-directory-saas-policystat-tutorial/IC808631.png "Konfiguriranje postavki aplikacije")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na PolicyStat** kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-policystat-tutorial/IC808632.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke PolicyStat kao administrator.

6.  Kliknite karticu **Administracija** , a zatim **Jednu konfiguracije za prijavu** u lijevom navigacijskom oknu.

    ![Izbornik administratora] (./media/active-directory-saas-policystat-tutorial/IC808633.png "Izbornik administratora")

7.  U odjeljku **Postavljanje** odaberite **Omogućivanje jedan Integracija za prijavu**.

    ![Jedan konfiguracije za prijavu] (./media/active-directory-saas-policystat-tutorial/IC808634.png "Jedan konfiguracije za prijavu")

8.  Kliknite **Konfiguriranje atribute**, a zatim u odjeljku **Konfiguriranje atribute** poduzeti sljedeće korake:

    ![Jedan konfiguracije za prijavu] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Jedan konfiguracije za prijavu")

    1.  U tekstni okvir **Atribut korisničko ime** upišite **uid**.
    2.  U tekstni okvir **Atribut ime** upišite **ime**.
    3.  U tekstni okvir **Posljednje atribut naziv** upišite **Prezime**.
    4.  U tekstni okvir **Atribut e-pošte** upišite **Adresa**.
    5.  Kliknite **Spremi promjene**.

9.  Kliknite **Vaše IDP metapodataka**, a zatim u odjeljku **Vaše IDP metapodataka** poduzeti sljedeće korake:

    ![Jedan konfiguracije za prijavu] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Jedan konfiguracije za prijavu")

    1.  Otvorite metapodataka preuzetu datoteku, kopirajte sadržaj i pa ih zalijepite u tekstni okvir **Vašeg metapodataka davatelja identiteta**
    2.  Kliknite **Spremi promjene**.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-policystat-tutorial/IC771723.png "Konfiguriranje jedinstvenu prijavu")

11. 12. Na izborniku na vrhu kliknite **atribute** da biste otvorili dijaloški okvir **SAML tokena atribute** .

    ![Atributi] (./media/active-directory-saas-policystat-tutorial/IC795920.png "Atributi")

13. Da biste dodali mapiranja obavezan atribut, poduzmite sljedeće korake:

    ![Atributi] (./media/active-directory-saas-policystat-tutorial/IC804823.png "Atributi")

    1.  Kliknite **Dodavanje korisnika atribut**.
    2.  U tekstni okvir **Naziv atributa** upišite **uid**.
    3.  U tekstni okvir **Vrijednost atributa** odaberite **ExtractMailPrefix()**.
    4.  Na popisu **pošta** odaberite **User.mail**.
    5.  Kliknite **dovrši**.
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u PolicyStat, oni mora biti dodijeljena u PolicyStat.  
PolicyStat ne podržava samo u vrijeme korisnika dodjele resursa. To znači da ne morate ručno dodati korisnike PolicyStat.  
Korisnici će se automatski dodaju na njihove prvi Prijava putem znak na.

>[AZURE.NOTE]Možete koristiti bilo koji drugi PolicyStat korisnički račun alate za stvaranje ili API-ji nudi PolicyStat dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-policystat-perform-the-following-steps"></a>Da biste korisnicima dodijelili PolicyStat, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **PolicyStat **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-policystat-tutorial/IC808636.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-policystat-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).