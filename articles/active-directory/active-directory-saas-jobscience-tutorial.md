<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Jobscience | Microsoft Azure" 
    description="Saznajte kako koristiti Jobscience s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Praktični vodič: Azure Active Directory Integracija s Jobscience
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Jobscience.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Pretplatu Jobscience jedinstvenu prijavu omogućeno
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Jobscience će moći znak u aplikaciju na vašem Jobscience web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Jobscience
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-jobscience-tutorial/IC784341.png "Scenarij")
##<a name="enabling-the-application-integration-for-jobscience"></a>Omogućivanje integraciju aplikacija za Jobscience
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Jobscience.

###<a name="to-enable-the-application-integration-for-jobscience-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Jobscience, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-jobscience-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-jobscience-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-jobscience-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-jobscience-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **jobscience**.

    ![Galerija aplikacije] (./media/active-directory-saas-jobscience-tutorial/IC784342.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Jobscience**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Jobscience] (./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Jobscience sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za Jobscience potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke Jobscience kao administrator.

2.  Idite na **Postavljanje**.

    ![Postavljanje] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Postavljanje")

3.  U lijevom navigacijskom oknu u odjeljku **administriranje** kliknite **Upravljanje domenom** proširite odjeljak povezani, a zatim **Moje domene** da biste otvorili stranicu **Moje domene** . 

    ![Domene] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Domene")

4.  Da biste potvrdili da vaše domene je ispravno, provjerite je li u "**Korak 4 implementiran korisnicima**" i pregled "**Postavke za Moje domene**".

    ![Doman implementiran korisniku] (./media/active-directory-saas-jobscience-tutorial/IC784377.png "Doman implementiran korisniku")

5.  U prozoru preglednika drugoj web prijaviti na portal Azure klasični.

6.  Na stranici za integraciju aplikacije **Jobscience** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-jobscience-tutorial/IC784360.png "Konfiguriranje jedinstvenu prijavu")

7.  Na stranici **kako biste željeli korisnika da biste se prijavili Jobscience** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-jobscience-tutorial/IC784361.png "Konfiguriranje jedinstvenu prijavu")

8.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Jobscience URL za prijavu** upišite URL pomoću sljedećeg uzorka "*http://company.my.salesforce.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-jobscience-tutorial/IC784362.png "Konfiguriranje URL adresa Web App")

9.  Na stranici **Konfiguracija jedinstvenu prijavu na Jobscience** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-jobscience-tutorial/IC784363.png "Konfiguriranje jedinstvenu prijavu")

10. Na Jobscience tvrtke web-mjesta, kliknite **Sigurnosne kontrole**, a zatim **Jednu postavke za prijavu**.

    ![Sigurnosne kontrole] (./media/active-directory-saas-jobscience-tutorial/IC784364.png "Sigurnosne kontrole")

11. U odjeljku **Postavke za jednu prijave** poduzeti sljedeće korake:

    ![Jedan postavke za prijavu] (./media/active-directory-saas-jobscience-tutorial/IC781026.png "Jedan postavke za prijavu")

    1.  Odaberite **SAML omogućena**.
    2.  Kliknite **Novo**.

12. U dijaloškom okviru **SAML jedan prijave postavka uređivanje** poduzeti sljedeće korake:

    ![SAML jedan prijave postavka] (./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML jedan prijave postavka")

    1.  U tekstni okvir **naziv** upišite naziv za konfiguraciju.
    2.  Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na Jobscience** kopirajte **URL izdavač** vrijednost, a pa ih zalijepite u tekstni okvir **izdavač**
    3.  U tekstni okvir **Id entitet** , upišite **https://salesforce-jobscience.com**
    4.  Kliknite **Pregledaj** da biste prenijeli Azure AD certifikata.
    5.  Kao **Vrsta identiteta SAML**, odaberite **pridruživanju sadrži ID vanjski pristup iz korisničkom objektu**.
    6.  **Mjesto identiteta SAML**, odaberite **identitet se element NameIdentfier izjave predmet**.
    7.  Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na Jobscience** kopirajte **URL daljinskog prijava** vrijednost, a zatim je zalijepite u tekstni okvir **URL za prijavu davatelja identiteta**
    8.  Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na Jobscience** kopirajte **URL daljinskog odjavite** vrijednost, a zatim je zalijepite u tekstni okvir **URL za odjavite davatelja identiteta**
    9.  Kliknite **Spremi**.

13. U lijevom navigacijskom oknu u odjeljku **administriranje** kliknite **Upravljanje domenom** proširite odjeljak povezani, a zatim **Moje domene** da biste otvorili stranicu **Moje domene** . 

    ![Domene] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Domene")

14. Na stranici **Moje domene** u odjeljku **Brendiranje stranicu za prijavu** kliknite **Uređivanje**.

    ![Brendiranje stranicu za prijavu] (./media/active-directory-saas-jobscience-tutorial/IC767826.png "Brendiranje stranicu za prijavu")

15. Na stranici **Brendiranje stranicu za prijavu** u odjeljku **Servis za provjeru autentičnosti** , prikazuje se naziv **SAML SSO postavke** . Odaberite ga, a zatim kliknite **Spremi**.

    ![Brendiranje stranicu za prijavu] (./media/active-directory-saas-jobscience-tutorial/IC784366.png "Brendiranje stranicu za prijavu")

16. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-jobscience-tutorial/IC784367.png "Konfiguriranje jedinstvenu prijavu")
  
Da biste u SP pokrenuti jedine prijave URL za prijavu kliknite na stranici **postavke za jedinstvenu prijavu** u odjeljku izbornik **Sigurnosne kontrole** .

![Sigurnosne kontrole] (./media/active-directory-saas-jobscience-tutorial/IC784368.png "Sigurnosne kontrole")
  
Kliknite SSO profil koji ste stvorili u koraku gore.  
Ova stranica prikazuje jedan znak URL-a za svoju tvrtku (npr. *https://companyname.my.salesforce.com?so=companyid*).
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Jobscience, oni mora biti dodijeljena u Jobscience.  
U slučaju Jobscience, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke **Jobscience** kao administrator.

2.  Idite na postavljanje

    ![Postavljanje] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Postavljanje")

3.  Idite na **Upravljanje korisnicima \> korisnika**.

    ![Korisnici] (./media/active-directory-saas-jobscience-tutorial/IC784369.png "Korisnici")

4.  Kliknite **novi korisnik**.

    ![Svi korisnici] (./media/active-directory-saas-jobscience-tutorial/IC784370.png "Svi korisnici")

5.  U dijaloškom okviru **Uređivanje korisnika** poduzeti sljedeće korake:

    ![Uređivanje korisnika] (./media/active-directory-saas-jobscience-tutorial/IC784371.png "Uređivanje korisnika")

    1.  Upišite ime, prezime, pseudonim, e-pošte, svojstva naziv i Nadimak korisničkih Azure AD korisnika kojem želite Dodjela resursa u povezani tekstni okviri.
    2.  Kliknite **Spremi**.

    >[AZURE.NOTE] Vlasnik računa Azure AD će primiti poruku e-pošte koja sadrži vezu da biste potvrdili račun prije nego što je aktivirana.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Jobscience korisnički račun alate za stvaranje ili API-ji nudi Jobscience dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-jobscience-perform-the-following-steps"></a>Da biste korisnicima dodijelili Jobscience, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Jobscience **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-jobscience-tutorial/IC784372.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-jobscience-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).