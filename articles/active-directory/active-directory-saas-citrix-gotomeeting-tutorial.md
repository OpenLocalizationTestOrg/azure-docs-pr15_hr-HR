<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Citrix GoToMeeting | Microsoft Azure" 
    description="Saznajte kako koristiti Citrix GoToMeeting s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Praktični vodič: Azure Active Directory Integracija s Citrix GoToMeeting  
Odnosi se na: Azure

>[AZURE.TIP]Povratne informacije, kliknite [ovdje](http://go.microsoft.com/fwlink/?LinkId=522412).

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Citrix GoToMeeting. Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Klijenta u Citrix GoToMeeting

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Citrix GoToMeeting
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Konfiguracija] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "Konfiguracija")



##<a name="enabling-the-application-integration-for-citrix-gotomeeting"></a>Omogućivanje integraciju aplikacija za Citrix GoToMeeting

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Citrix GoToMeeting.

###<a name="to-enable-the-application-integration-for-citrix-gotomeeting-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Citrix GoToMeeting, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz galerije] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "Dodavanje aplikacije iz galerije")

6.  U **okvir za pretraživanje**upišite **Citrix GoToMeeting**.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  U oknu s rezultatima odaberite **Citrix GoToMeeting**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Citrix GoToMeeting sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za prijenos certifikata kodirana osnovni 64 Citrix GoToMeeting klijentu.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Na stranici za integraciju aplikacije **Citrix GoToMeeting** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje JEDAN znak Uključeno** .

    ![Omogućivanje jedinstvenu prijavu] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "Omogućivanje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Citrix GoToMeeting** odaberite **Microsoft Azure AD jedinstvenu prijavu**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "Konfiguriranje jedinstvenu prijavu")


3. Na stranici **Konfiguracija postavki aplikacije** kliknite **Dalje**. 

    ![Omogućivanje jedinstvenu prijavu] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689981.png "Omogućivanje jedinstvenu prijavu")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Citrix GoToMeeting** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru drugi preglednik, prijavite se u tvrtki ili ustanovi centru Citrix ([https://account.citrixonline.com/organization/administration/](https://account.citrixonline.com/organization/administration/)).

6. Kliknite karticu **Davatelja identiteta** , a zatim izvršite sljedeće korake:  

    ![Postavljanje SAML] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "Postavljanje SAML")

    na. Odaberite **ručno**

    
    b. Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Citrix GoToMeeting** kopirajte vrijednost **URL stranice za prijavu** i pa ih zalijepite u tekstni okvir **URL stranice za prijavu** . 

    
    c. Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Citrix GoToMeeting** kopirajte **URL stranice Sign-Out** vrijednost i pa ih zalijepite u tekstni okvir **URL Sign-out stranice** .

    
    d. Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Citrix GoToMeeting** kopirajte vrijednost **ID entitet** , a pa ih zalijepite u tekstni okvir **ID za entitet davatelja identiteta** .

   
    e. Da biste prenijeli preuzete certifikata, kliknite **Prenesi certifikata**.

    
    f. Kliknite **Spremi**.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "Konfiguriranje jedinstvenu prijavu")


7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.

    ![Postavljanje SAML] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689982.png "Postavljanje SAML")





##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Cilj ovaj odjeljak je Strukturiranje kako omogućiti dodjeljivanje korisničkih računa servisa Active Directory za Citrix GoToMeeting.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Citrix GoToMeeting** kliknite **Konfiguriraj dodjele resursa korisnika** da biste otvorili dijaloški okvir **Konfiguriranje dodjele resursa korisnika** .

    ![Dodjela resursa Konfiguracija korisnika] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "Dodjela resursa Konfiguracija korisnika")

2.  Na stranici **Postavke i administratorske vjerodajnice** , poduzmite sljedeće korake:

    ![Dodjela resursa Konfiguracija korisnika] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "Dodjela resursa Konfiguracija korisnika")

    na. U tekstni okvir **Citrix GoToMeeting administrator korisničko ime** upišite korisničko ime administratora.

    
    b. U **Citrix GoToMeeting administratorsku lozinku** tekstni okvir na administratorsku lozinku.

    
    c. Kliknite **Dalje**.

3.  Na stranici za **potvrdu** kliknite kvačicu da biste spremili konfiguraciju.

4.  Kliknite gumb **Provjeri valjanost** da biste provjerili konfiguraciju.


##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-citrix-gotomeeting-perform-the-following-steps"></a>Da biste korisnicima dodijelili Citrix GoToMeeting, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Citrix GoToMeeting** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "Da")

Sada treba pričekajte 10 minuta i provjerite da je sinkronizirana računa za Dropbox za tvrtke.

Kao prvi korak za potvrdu, možete provjeriti status dodjele resursa tako da kliknete nadzorne ploče u D na stranici za integraciju aplikacije **Citrix GoToMeeting** Azure portala za klasični.

![Nadzorna ploča] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "Nadzorna ploča")

Korisnik uspješno dovršena dodjeljivanje ciklusa označena je povezane status:

![Stanje integracije] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "Stanje integracije")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access.

Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](https://msdn.microsoft.com/library/dn308586).
