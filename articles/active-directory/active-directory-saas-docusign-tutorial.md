<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s DocuSign | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i DocuSign."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Praktični vodič: Azure Active Directory Integracija s DocuSign

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i DocuSign.
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

- Valjani Azure pretplate
- Klijenta u DocuSign



Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1. [Omogućivanje integraciju aplikacija za DocuSign](#enabling-the-application-integration-for-docusign) 


2. [Konfiguriranje jedinstvenu prijavu](#configuring-single-sign-on) 


3. [Konfiguriranje računa dodjele resursa](#configuring-account-provisioning) 


4. [Dodjela korisnika](#assigning-users) 

    ![Konfiguriranje jedinstvenu prijavu][0]
 

## <a name="enabling-the-application-integration-for-docusign"></a>Omogućivanje integraciju aplikacija za DocuSign

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za DocuSign.

### <a name="to-enable-the-application-integration-for-docusign-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za DocuSign, poduzmite sljedeće korake:

1. Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Konfiguriranje jedinstvenu prijavu][1]

2. Popis direktorija odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Konfiguriranje jedinstvenu prijavu][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. Na što želite učiniti dijaloški okvir, kliknite **Dodaj aplikaciju iz galerije**.

    ![Konfiguriranje jedinstvenu prijavu][4]


6. U okvir za pretraživanje upišite **DocuSign**.

    ![Konfiguriranje jedinstvenu prijavu][5]

7. U oknu s rezultatima odaberite **DocuSign**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Konfiguriranje jedinstvenu prijavu][6]


## <a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti DocuSign sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.


### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1. Azure klasični portalu na stranici **DocuSign integraciju aplikacija** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir konfiguriranje jedinstvenu prijavu.

    ![Konfiguriranje jedinstvenu prijavu][7]

2. Na stranici **kako biste željeli korisnika da biste se prijavili DocuSign** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite Dalje.

    ![Konfiguriranje jedinstvenu prijavu][8]

3. Na stranici **Konfiguracija postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu][61]

    na. U tekstni okvir **prijavite se na URL-a** upišite `https://account.docusign.com/*`.  

    b. U tekstni okvir **identifikator** upišite `https://account.docusign.com/*`.  
   
    c. Kliknite **Dalje**. 


    > [AZURE.TIP] Prijavite se na URL-a i vrijednosti identifikatora su samo rezervirana mjesta. Upute za dohvaćanje stvarnih vrijednosti u okruženju sustava možete pronaći u u nastavku ovog članka.
 

4. Na stranici **Konfiguracija jedinstvenu prijavu na DocuSign** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu][10]


5. U prozoru preglednika drugoj web, prijavite se u **portala za administratore DocuSign** kao administrator.


6. U navigacijskom izborniku na lijevoj strani kliknite **domene**.

    ![Konfiguriranje jedinstvenu prijavu][51]

7. U desnom oknu kliknite **Preuzmi domene**.

    ![Konfiguriranje jedinstvenu prijavu][52]

8. U dijaloškom okviru **zahtjeva domenu** , u tekstni okvir **Naziv domene** unesite domenu tvrtke, a zatim **Preuzmi**. Provjerite je li potvrda domene i status je aktivna.

    ![Konfiguriranje jedinstvenu prijavu][53]

9. Na izborniku na lijevoj strani kliknite **Davatelji identiteta**  

    ![Konfiguriranje jedinstvenu prijavu][54]

10. U desnom oknu kliknite **Dodavanje davatelja identiteta**. 
    
    ![Konfiguriranje jedinstvenu prijavu][55]

11. Na stranici **Postavke davatelja identiteta** , poduzmite sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu][56]


    na. U tekstni okvir **naziv** upišite jedinstveni naziv za konfiguraciju. Koristite razmake.

    b. Azure klasični portalu kopirajte URL izdavača i pa ih zalijepite u tekstni okvir **Izdavač davatelja identiteta** .

    c. Azure klasični portalu kopirajte **URL daljinskog prijava**, a pa ih zalijepite u tekstni okvir **URL za prijavu davatelja identiteta** .

    d. Azure klasični portalu kopirajte **URL daljinskog odjavite**, a zatim je zalijepite u tekstni okvir **URL za odjavite davatelja identiteta** .

    e. Odaberite **Znak AuthN zahtjev**.

    f. Kao **AuthN poslati zahtjev**, odaberite **OBJAVI**.

    g. Kao **poslati zahtjev odjavite**, odaberite **OBJAVI**. 


12. U odjeljku **Mapiranja atributa Prilagođeno** odaberite polje koje želite mapirati sa Azure AD zahtjeva. U ovom primjeru zahtjeva **Adresa** je pridružena s vrijednošću **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. To je zadani naziv zahtjeva iz Azure AD za zahtjev za e-pošte. 

    > [AZURE.NOTE] Koristite odgovarajuću **identifikator korisnika** da biste mapirali korisniku Azure AD Docusign korisničkog mapiranja. Odaberite odgovarajuće polje i unesite odgovarajuću vrijednost koja se temelji na postavkama tvrtke ili ustanove.

    ![Konfiguriranje jedinstvenu prijavu][57]

13. U odjeljku **Potvrda davatelja identiteta** kliknite **Dodavanje certifikat**, a zatim prijenos certifikat koji ste preuzeli s portala za klasični Azure AD.   

    ![Konfiguriranje jedinstvenu prijavu][58]

14. Kliknite **Spremi**.

15. U odjeljku **Davatelji identiteta** kliknite **Akcije**, a zatim **krajnje točke**.   

    ![Konfiguriranje jedinstvenu prijavu][59]



10. Azure portala za klasični, vratite se na stranici **Konfiguracija postavki aplikacije** . 

16. Na **portalu za administratore DocuSign**, u odjeljku **Krajnje točke za prikaz SAML 2.0** poduzeti, sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu][60]

    na. Kopirajte **URL izdavač davatelja usluge**i pa ih zalijepite u tekstni okvir **identifikator** Azure portala za klasični.

    b. Kopirajte **URL za prijavu davatelja usluge**, a zatim ga zalijepite u tekstni okvir **URL na za prijavu** na portalu Azure klasični.

    c.  Kliknite **Zatvori**  


10. Azure portala za klasični, kliknite **Dalje**. 


15. Azure klasični portala odaberite **potvrdu jedan konfiguracije za prijavu**pa zatim kliknite **Dalje**.

    ![Aplikacija][14]

10. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.

    ![Aplikacija][15]
 

## <a name="configuring-account-provisioning"></a>Konfiguriranje računa dodjele resursa

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisniku dodjeljivanje korisničkih računa servisa Active Directory za DocuSign.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1. **Azure klasični portal**na stranici **DocuSign integraciju aplikacija** kliknite **Dodjeljivanje za konfiguriranje računa** da biste otvorili dijaloški okvir konfiguriranje dodjele resursa korisnika.

    ![Konfiguriranje računa dodjele resursa][30]

2. Na stranici **Postavke i administratorske vjerodajnice** da biste omogućili automatsko korisnika dodjele resursa, unesite vjerodajnice računa DocuSign s dovoljno prava, a zatim **Dalje**. 

    ![Konfiguriranje računa dodjele resursa][31]

3. U dijaloškom okviru **Testiranje veze** kliknite **pokrenite test**, a nakon uspješne test, kliknite **Dalje**.

    ![Konfiguriranje računa dodjele resursa][32]

3. Na stranici za **potvrdu** kliknite **dovrši**.

    ![Konfiguriranje računa dodjele resursa][33]
 

## <a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

### <a name="to-assign-users-to-docusign-perform-the-following-steps"></a>Da biste korisnicima dodijelili DocuSign, poduzmite sljedeće korake:

1. **Azure klasični portal**stvorite testnom računu.

2. Na stranici **DocuSign integraciju aplikacija** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika][40]
 

3. Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Dodjela korisnika][41]


Sada treba pričekajte 10 minuta i provjerite je li sinkronizirani da račun DocuSign.

Kao prvi korak za potvrdu, možete provjeriti status dodjele resursa tako da kliknete nadzorne ploče u D na stranici za integraciju DocuSign aplikacije na portalu Azure klasični.

![Dodjela korisnika][42]

Korisnik uspješno dovršena dodjeljivanje ciklusa označena je povezane status:

![Dodjela korisnika][43]


Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access.

Dodatne pojedinosti o ploča programa Access potražite u članku Uvod u oknu programa Access.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png