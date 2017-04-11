<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s SAP HANA oblaka platformu | Microsoft Azure" 
    description="Saznajte kako pomoću SAP HANA oblaka platforme Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Praktični vodič: Azure Active Directory Integracija s SAP HANA oblaka platforme
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i SAP HANA oblaka platforme.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   SAP HANA oblaka platformu računa
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili SAP HANA oblaka platforma moći jednom prijava u aplikacije pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

>[AZURE.IMPORTANT]Morate uvesti vlastitu aplikacije ili pretplata na aplikaciju na svom računu SAP HANA oblaka platformu da biste testirali jednokratne prijave na. U ovom ćete praktičnom vodiču aplikacija je uveden u računa.
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za SAP HANA oblaka platforme
2.  Konfiguriranje jedinstvenu prijavu
3.  Dodjeljivanje uloge korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenarij")
##<a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>Omogućivanje integraciju aplikacija za SAP HANA oblaka platforme
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za SAP HANA oblaka platformu.

###<a name="to-enable-the-application-integration-for-sap-hana-cloud-platform-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za SAP HANA oblaka platformu, učinite sljedeće:

1.  Na portalu za upravljanje Azure, u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **SAP HANA oblaka platforme**.

    ![Galerija aplikacije] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Platformu za oblak HANA SAP**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![SAP Hana] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti SAP HANA oblaka platformu sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za prijenos certifikata kodirana osnovni 64 klijent sustava SAP HANA oblaka platforme.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **SAP HANA oblaka platformu** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili SAP HANA oblaka platformu** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Konfiguriranje jedinstvenu prijavu")

3.  U prozoru preglednika drugoj web, prijavite se platformi Cockpit SAP oblaka HANA pri https://account. \<vodoravno host\>.ondemand.com/cockpit (npr.: *https://account.hanatrial.ondemand.com/cockpit*).

4.  Kliknite karticu **pouzdanost** .

    ![Pouzdanost] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Pouzdanost")

5.  U odjeljku Upravljanje pouzdanost, učinite sljedeće:

    ![Dohvaćanje metapodataka] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Dohvaćanje metapodataka")

    1.  Kliknite karticu **Lokalnog davatelja usluga** .
    2.  Da biste preuzeli datoteku metapodataka SAP HANA oblaka platforme, kliknite **Dohvati metapodataka**.

6.  Azure Active klasični portalu na stranici **Konfiguriranje URL adresa Web App** poduzeti sljedeće korake, a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Konfiguriranje URL adresa Web App")

    1.  U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika da biste se prijavili u aplikaciju sustava **SAP HANA oblaka platforme** . To je URL specifične za račun zaštićeni resursa u aplikaciji za SAP HANA oblaka platforme. URL temelji se na sljedeći uzorak: *https://\<applicationName\>\<accountName\>.\< glavno računalo za vodoravno\>.ondemand.com/\<put\_da biste\_zaštićena\_resursa\> * (npr.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

        >[AZURE.NOTE]To je URL-a u aplikaciji SAP HANA oblaka platformu koji zahtijeva provjeru autentičnosti korisnika.

    2.  Otvorili preuzetu datoteku metapodataka SAP HANA oblaka platforme, a zatim pronađite **ns3:AssertionConsumerService** oznaka.
    3.  Kopirajte vrijednost atributa **mjesto** i pa ih zalijepite u tekstni okvir **SAP HANA oblaka platformu odgovor URL-a** .

7.  Na stranici **Konfiguracija jedinstvenu prijavu na SAP HANA oblaka platformu** da biste preuzeli metapodatka, kliknite **Preuzmi metapodatke**, a zatim spremite datoteku na računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Konfiguriranje jedinstvenu prijavu")

8.  U odjeljku **Lokalni davatelj usluga** na platformi Cockpit SAP HANA oblaka poduzeti sljedeće korake:

    ![Upravljanje pouzdanim] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Upravljanje pouzdanim")

    1.  Kliknite **Uređivanje**.
    2.  Kao **Vrsta konfiguracijske**, odaberite **Prilagođeno**.
    3.  **Lokalni naziv davatelja usluge**, ostavite zadanu vrijednost.
    4.  Da biste generirali **Potpisivanja ključ** i ključne par **Potpisni certifikat** , kliknite **Generiraj par ključ**.
    5.  Kao **Glavni prijenos**, odaberite **onemogućene**.
    6.  Kao **Prisilno provjeru autentičnosti**, odaberite **onemogućene**.
    7.  Kliknite **Spremi**.

9.  Kliknite karticu **Pouzdanih davatelja identiteta** , a zatim kliknite **Dodavanje pouzdanih davatelja identiteta**.

    ![Upravljanje pouzdanim] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Upravljanje pouzdanim")

    >[AZURE.NOTE]Da biste upravljali popisom davatelj pouzdanog identiteta, morate da biste odabrali vrstu prilagođene konfiguracije u odjeljku lokalnog davatelja usluga. Za zadanu vrstu konfiguracije, imate ne može uređivati i implicitno pouzdanost na servis za ID SAP-a. Za ništa, ne morate sve postavke sigurnosti.

10. Kliknite karticu **Općenito** , a zatim **Pregledaj** da biste prenijeli datoteku preuzete metapodataka.

    ![Upravljanje pouzdanim] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Upravljanje pouzdanim")

    >[AZURE.NOTE] Nakon prenošenja datoteku metapodataka vrijednosti za **jednu URL za prijavu**, **Jedan odjavite URL-a** i **Potpisni certifikat** se unose automatski.

11. Kliknite karticu **atribute** .

12. Na kartici **atribute** poduzeti sljedeće korake:

    ![Atributi] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Atributi")

    1.  Klikom na **Add Assertion-Based atribut**dodajte sljedećim atributima utemeljen na pridruživanju:

        |Atribut pridruživanju| Glavnica atributa|
        |-------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/givenname|   ime|--------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/surname|        Prezime|-----------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/emailAddress|e-pošte|

    >[AZURE.NOTE]Konfiguracija atributa ovisi o tome kako aplikacijama na HCP su razvijaju, odnosno koje attribute(s) kakvu očekuju u odgovoru SAML i pod nazivom (glavnica atribut) pristupi taj atribut kod.
    >  
    >na.  **Zadani atributa** u snimku zaslona je samo na slici. Nije obavezno da bi scenarij rad.  
    >
    >b.  Nazive i vrijednosti za **Atribut glavnicu** prikazano snimku zaslona ovise o tome kako je razvio aplikacije. Nije moguće aplikacije potreban je drugi mapiranja.

13. Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na SAP HANA oblaka platformu** odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Konfiguriranje jedinstvenu prijavu")
  
Kao neobavezan korak možete konfigurirati utemeljen na pridruživanju grupe za Azure Active Directory identiteta davatelja usluga

>[AZURE.NOTE]Korištenje grupe na platformi oblaka za SAP HANA omogućuje dinamički jednu ili više uloga u aplikacijama SAP HANA oblaka platforme, određen vrijednosti atributa u pridruživanju SAML 2.0 dodijeliti jedan ili više korisnika. Na primjer, ako u pridruživanju sadrži atribut "*ugovora = privremene*", možda ćete sve zahvaćene korisnike potrebno dodati u grupu "*PRIVREMENE*". Grupi "*PRIVREMENE*" možda sadrži jednu ili više uloga iz jednog ili više aplikacija implementiran na vašem računu SAP HANA oblaka platforme.
>  
>Koristite utemeljen na pridruživanju grupe ako želite mase dodjeljivanje mnogi korisnici jedan ili više ulogama aplikacija na vašem računu SAP HANA oblaka platforme. Ako samo želite dodijeliti jednostruke ili mali broj korisnika (a) određene uloge preporučujemo da joj dodijelite izravno na kartici "**autorizacijama**" cockpit SAP HANA oblaka platforme.

##<a name="assigning-a-role-to-a-user"></a>Dodjeljivanje uloge korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u SAP HANA oblaka platformu, morate dodijeliti ulogama u oblak platforme HANA SAP ih.

###<a name="to-assign-a-role-to-a-user-perform-the-following-steps"></a>Da biste korisniku dodijelite uloge, poduzmite sljedeće korake:

1.  Prijavite se vaše cockpit **SAP HANA oblaka platforme** .

2.  Poduzmite sljedeće korake:

    ![Autorizacijama] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Autorizacijama")

    1.  Kliknite **autorizacije**.
    2.  Kliknite karticu **korisnika** .
    3.  U tekstni okvir **korisnika** upišite adresu e-pošte za korisnika.
    4.  Kliknite **Dodijeli** korisniku dodijeliti ulogu.
    5.  Kliknite **Spremi**.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-sap-hana-cloud-platform-perform-the-following-steps"></a>Da biste korisnicima dodijelite SAP HANA oblaka platforme, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **SAP HANA oblaka platformu** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).