<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija sa servisom Dropbox za tvrtke | Microsoft Azure" 
    description="Saznajte kako koristiti Dropbox za tvrtke s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Praktični vodič: Azure Active Directory Integracija sa servisom Dropbox za tvrtke
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Dropbox za tvrtke.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Testiranje klijenta u Dropbox za tvrtke
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Dropbox za tvrtke će moći znak u aplikaciju na sa servisa Dropbox za tvrtke tvrtke web-mjesta (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Dropbox za tvrtke
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Scenarij")



##<a name="enabling-the-application-integration-for-dropbox-for-business"></a>Omogućivanje integraciju aplikacija za Dropbox za tvrtke
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Dropbox za tvrtke.

###<a name="to-enable-the-application-integration-for-dropbox-for-business-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Dropbox za tvrtke, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Dropbox za tvrtke**.

    ![Galerija aplikacije] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Dropbox za tvrtke**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Dropbox za tvrtke] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropbox za tvrtke")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Dropbox za tvrtke sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

Kao dio ovog postupka, koje su potrebne za prijenos certifikata kodirana osnovni 64 zajedničke mrežne mape za klijent Business. Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Dropbox za tvrtke** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Dropbox za tvrtke** , odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , učinite sljedeće:

    na. Prijava na vašem Dropbox za klijent business. 

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Konfiguriranje jedinstvenu prijavu")

    b. U navigacijskom oknu s lijeve strane kliknite **Administracijskoj konzoli sustava**. 

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Konfiguriranje jedinstvenu prijavu")

    c. Na **Konzoli za administratore**kliknite **Provjera autentičnosti** u lijevom navigacijskom oknu. 

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Konfiguriranje jedinstvenu prijavu")

    d. U odjeljku **jedinstvenu prijavu** odaberite **Omogući jedinstvenu prijavu**, a zatim kliknite **više** da biste proširili odjeljak.  

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Konfiguriranje jedinstvenu prijavu")

    e. Kopirajte URL pokraj **korisnici mogli prijaviti tako da unesete njihove adrese e-pošte ili ih možete posjetiti izravno**. 

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Konfiguriranje jedinstvenu prijavu")

    f. Azure portala za klasični **DropBox za tvrtke prijavite se u** URL-a tekstni okvir zalijepite URL. 

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Konfiguriranje jedinstvenu prijavu")  



4. Na stranici **Konfiguracija jedinstvenu prijavu na Dropbox za tvrtke** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.  

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Konfiguriranje jedinstvenu prijavu")


5. Na zajedničke mrežne mape za klijent Business, u odjeljku **jedinstvenu prijavu** na stranici **Provjera autentičnosti** poduzeti sljedeće korake: 

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Konfiguriranje jedinstvenu prijavu")

    na. Kliknite **obavezno**.

    b. Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Dropbox za tvrtke** kopirajte vrijednost **URL stranice za prijavu** i pa ih zalijepite u tekstni okvir **prijavite se u URL-a** .


    c. Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata. 

    > [AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).


    d. Pritisnite gumb **"Odaberite certifikat"** , a zatim idite na **Osnovni 64 kodirana datoteka certifikata**.


    e. Kliknite gumb **"Spremi promjene"** da biste dovršili konfiguraciju na zajedničke mrežne mape za klijent Business.


6. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** . 

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Konfiguriranje jedinstvenu prijavu")



##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisniku dodjele resursa od servisa Active Directory korisničkih računa za Dropbox za tvrtke.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1. Azure klasični portalu na stranici za integraciju aplikacije **Dropbox za tvrtke** kliknite **Konfiguriraj dodjele resursa korisnika** da biste otvorili dijaloški okvir **Konfiguriranje dodjele resursa korisnika** .

2. Na na Omogući korisnika dodjele resursa za DropBox za tvrtke, kliknite Omogući korisnika dodjeljivanje da biste otvorili prijavite se da biste zajedničke mrežne mape da biste se povezali pomoću dijaloškog okvira Azure AD.  

    ![Dodjela resursa za korisnika] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "Dodjela resursa za korisnika")

3. U dijaloškom okviru **Prijava na zajedničke mrežne mape da biste se povezali s Azure AD** prijavite zajedničke mrežne mape za klijent Business. 

    ![Dodjela resursa za korisnika] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "Dodjela resursa za korisnika")



4. Kliknite **Dopusti** da biste dodijelili Azure AD za pristup zajedničke mrežne mape. 

    ![Dodjela resursa za korisnika] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "Dodjela resursa za korisnika")



5. Da biste dovršili konfiguraciju, kliknite gumb **Dovršeno** .  

    ![Dodjela resursa za korisnika] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "Dodjela resursa za korisnika")




##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-dropbox-for-business-perform-the-following-steps"></a>Da biste korisnicima dodijelili Dropbox za tvrtke, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Dropbox za tvrtke **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Da")
  


Sada treba pričekajte 10 minuta i provjerite da je sinkronizirana računa za Dropbox za tvrtke.

Kao prvi korak za potvrdu, možete provjeriti status dodjele resursa tako da kliknete **nadzorne ploče** na stranici integraciju aplikacije za **Dropbox za tvrtke** na portalu Azure klasični.

![Dodjela korisnika] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Dodjela korisnika")


Srodni status označena je korisnik uspješno dovršena dodjeljivanje ciklus.

![Dodjela korisnika] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Dodjela korisnika")


Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access.
Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).




## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)