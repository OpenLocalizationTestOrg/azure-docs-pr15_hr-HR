<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Salesforce izdvojeni | Microsoft Azure"
    description="Saznajte kako pomoću servisa Salesforce izdvojeni s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!." 
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
    ms.date="10/28/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Praktični vodič: Azure Active Directory Integracija s izdvojeni Salesforce
>[AZURE.TIP]Povratne informacije, kliknite [ovdje](http://go.microsoft.com/fwlink/?LinkId=521878).
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i izdvojeni Salesforce.  
Sandboxes nude vam omogućuje stvaranje većeg broja primjeraka vaše tvrtke ili ustanove u zasebnom okruženja za različite potrebe, kao što su razvoj, testiranje i obuku ne ugrozi podataka i aplikacije u vašoj tvrtki ili ustanovi Salesforce radnog.  
Dodatne informacije potražite u članku [Pregled s memorijom za testiranje](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)
  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Izdvojeni u Salesforce.com
  
Ako nemate valjani izdvojeni Salesforce.com, obratite se Salesforce.
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za testiranje Salesforce
2.  Konfiguriranje jedinstvenu prijavu
3.  Omogućivanje domene
4.  Konfiguriranje dodjeljivanje korisnika
5.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenarij")
##<a name="enabling-the-application-integration-for-salesforce-sandbox"></a>Omogućivanje integraciju aplikacija za testiranje Salesforce
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za testiranje Salesforce.

###<a name="to-enable-the-application-integration-for-salesforce-sandbox-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za testiranje Salesforce, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Aplikacija")

4.  Da biste otvorili **Galeriju aplikaciju**, kliknite **Dodaj programa aplikacija**, a zatim **Dodavanje aplikacije za tvrtke ili ustanove da biste koristili**.

    ![Što želite učiniti?] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Što želite učiniti?")

5.  U **okvir za pretraživanje**upišite **Salesforce izdvojeni**.

    ![Galerija aplikacije] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Galerija aplikacije")

6.  U oknu s rezultatima odaberite **Izdvojeni Salesforce**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Izdvojeni Salesforce] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Izdvojeni Salesforce")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Salesforce sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Salesforce izdvojeni** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili memoriji za testiranje Salesforce** , odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Izdvojeni Salesforce] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Izdvojeni Salesforce")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Prijavite se na URL** unesite URL pomoću sljedećeg uzorka `http://company.my.salesforce.com`, a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Konfiguriranje URL adresa Web App")

4. Ako ste već konfigurirali jedinstvene prijave za drugu instancu Salesforce izdvojeni u direktoriju, zatim morate konfigurirati **identifikator** da imaju iste vrijednosti kao **prijavite se na URL-a**. **Identifikator** polja možete pronaći tako da poništite potvrdni okvir **Pokaži napredne postavke** na stranici **Konfiguriranje URL adresa Web App** dijaloškog okvira.

4.  Na stranici **Konfiguracija jedinstvenu prijavu u memoriji za testiranje Salesforce** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u memoriji za testiranje Salesforce kao administrator.

6.  Na izborniku na vrhu kliknite **Postavljanje**.

    ![Postavljanje] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Postavljanje")

7.  U navigacijskom oknu s lijeve strane kliknite **Sigurnosne kontrole**, a zatim **Jednu postavke za prijavu**.

    ![Jedan postavke za prijavu] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Jedan postavke za prijavu")

8.  U odjeljku postavke za jednu prijave poduzeti sljedeće korake:

    ![Jedan postavke za prijavu] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Jedan postavke za prijavu")

    na.  Odaberite **SAML omogućena**.
    
    b.  Kliknite **Novo**.

9.  Na SAML jedan prijavite se u odjeljku postavke učinite sljedeće:

    ![SAML jedan prijave postavke] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML jedan prijave postavke")

    na.  U tekstni okvir Naziv upišite naziv konfiguracije (npr.: *SPSSOWAAD\_Test*).
    
    b.  Azure klasični portalu na dijaloški okvir stranici **Konfiguracija jedinstvenu prijavu u memoriji za testiranje Salesforce** , kopirajte vrijednost **Izdavač URL** i pa ih zalijepite u tekstni okvir **izdavač** .
    
    c.  U tekstni okvir **Id entitet** , upišite **https://test.salesforce.com** ako je prvu instancu Salesforce izdvojeni koji dodajete u direktoriju. Ako ste već dodali instance programa izdvojeni Salesforce, a zatim za tu vrstu **ID entitet** u **Prijavite se na URL-a**, što bi trebalo biti u ovom obliku:`http://company.my.salesforce.com`
    
    d.  Kliknite **Pregledaj** da biste prenijeli preuzete certifikata.
    
    e.  Kao **Vrsta identiteta SAML**, odaberite **pridruživanju sadrži ID vanjski pristup iz korisničkom objektu**.
    
    f.  **Mjesto identiteta SAML**, odaberite **identitet se element NameIdentifier izjave predmet**.
    
    g.  Azure klasični portalu na dijaloški okvir stranici **Konfiguracija jedinstvenu prijavu u memoriji za testiranje Salesforce** , kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu davatelja identiteta** .
    
    h.  SFDC ne podržava SAML odjavite.  Kao zaobilazno rješenje, zalijepite 'https://login.windows.net/common/wsfederation?wa=wsignout1.0' u tekstni okvir **URL za odjavite davatelja identiteta** .
    
    li.  Kao **Servisa davatelja pokrenut zahtjev za povezivanje**, odaberite **HTTP POST**.
    
    j. Kliknite **Spremi**.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Konfiguriranje jedinstvenu prijavu")

##<a name="enabling-your-domain"></a>Omogućivanje domene
  
U ovom se odjeljku pretpostavlja da sve već stvorili domene.  
Dodatne informacije potražite u članku [Definiranje naziv vaše domene](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

###<a name="to-enable-your-domain-perform-the-following-steps"></a>Da biste omogućili vašu domenu, učinite sljedeće:

1.  U lijevom navigacijskom oknu kliknite **Upravljanje domenom**, a zatim kliknite **Moje domene.**

    ![Domene] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Domene")

    >[AZURE.NOTE]Provjerite da domenu pravilno konfigurirana.

2.  U odjeljku **Postavke stranice za prijavu** kliknite **Uredi**, zatim kao **Servis za provjeru autentičnosti**, odaberite naziv SAML jedan prijave postavke iz prethodnog odjeljka i na kraju kliknite **Spremi**.

    ![Domene] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Domene")
  
Čim imate domenu konfigurirana, korisnici trebali biste koristiti URL domene da biste se prijavili u memoriji za testiranje Salesforce.  
Da biste dobili vrijednost od URL-a, kliknite profil SSO koji ste stvorili u prethodnom odjeljku.
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisniku dodjeljivanje korisničkih računa servisa Active Directory u memoriji za testiranje Salesforce.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Na portalu servisa Salesforce, na gornjoj navigacijskoj traci odaberite svoje ime da biste proširili na izborniku korisnika:

    ![Moje postavke] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Moje postavke")

2.  Na izborniku korisnika odaberite **Postavke za Moje** otvoriti stranicu za **Moje postavke** .

3.  U lijevom oknu kliknite **osobne** proširite odjeljak osobnih, a zatim **Ponovno postavi moj sigurnosni Token**:

    ![Moje postavke] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Moje postavke")

4.  Na stranici **Ponovno postavi moj sigurnosni Token** kliknite **Vrati izvorno sigurnosni Token** da biste zatražili poruke e-pošte koja sadrži vaše Salesforce.com sigurnosni token.

    ![Novi Token] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "Novi Token")

5.  Provjerite mapu ulazne pošte za poruku e-pošte sa servisa Salesforce.com s "**salesforce.com.com sigurnost potvrdu**" kao predmet.

6.  Pregledajte ovaj e-pošte i kopirajte sigurnosni token vrijednost.

7.  Azure klasični portalu na stranici za integraciju aplikacije **salesforce izdvojeni** kliknite **Konfiguriraj dodjele resursa korisnika** da biste otvorili dijaloški okvir **Konfiguriranje dodjele resursa korisnika** .

    ![Dodjela resursa Konfiguracija korisnika] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Dodjela resursa Konfiguracija korisnika")

8.  Na stranici **Unesite vjerodajnice za testiranje Salesforce da biste omogućili automatsko korisnika dodjele resursa** , navedite sljedeće postavke konfiguracije:

    ![Izdvojeni Salesforce] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Izdvojeni Salesforce")

    na.  U tekstni okvir **Salesforce izdvojeni administrator korisničko ime** upišite naziv računa Salesforce izdvojeni s koja ima profila **Administrator sustava** u Salesforce.com dodijeljeni.

    b.  U tekstni okvir **Salesforce izdvojeni administratorsku lozinku** upišite lozinku za taj račun.

    c.  U tekstni okvir **Korisnika sigurnosni Token** zalijepite sigurnosni token vrijednost.

    d.  Kliknite **Provjeri valjanost** da biste provjerili konfiguraciju.

    e.  Kliknite gumb **Dalje** da biste otvorili stranicu **potvrde** .

9.  Na stranici za **potvrdu** kliknite **dovrši** da biste spremili konfiguraciju.
##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-salesforce-sandbox-perform-the-following-steps"></a>Da biste korisnicima dodijelili izdvojeni Salesforce, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Salesforce izdvojeni **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Da")
  
Sada treba pričekajte 10 minuta i provjerite je li sinkronizirani da na račun servisa Salesforce izdvojeni.
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](https://msdn.microsoft.com/library/dn308586).
