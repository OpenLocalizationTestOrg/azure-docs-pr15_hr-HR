<properties 
    pageTitle="Praktični vodič: Konfiguriranje Workday ulaznog sinkronizacije | Microsoft Azure" 
    description="Saznajte kako koristiti Workday kao izvor podataka identiteta za Azure Active Directory." 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />


#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Praktični vodič: Konfiguriranje Workday ulaznog sinkronizacije


Cilj ovog praktičnog vodiča je da bi se prikazala korake morate izvesti u Workday i Azure AD da biste uvezli osobe iz Workday Azure AD. 

Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure AD Premium pretplate
-   Klijenta u Workday
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1. Omogućivanje integraciju aplikacija za Workday 


2. Stvaranje korisnik sustava integraciju sustava 


3. Stvaranje sigurnosne grupe 


4. Dodjeljivanje korisnika za integraciju sustava sigurnosne grupe 


5. Konfiguriranje mogućnosti za sigurnosne grupe 


6. Aktiviranje promjena sigurnosnih pravilnika 


7. Konfiguriranje korisnički uvoz u Azure AD 



##<a name="enabling-the-application-integration-for-workday"></a>Omogućivanje integraciju aplikacija za Workday
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za dana.

### <a name="steps"></a>Koraci:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-workday-inbound-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-workday-inbound-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-workday-inbound-tutorial/IC749321.png "Dodavanje aplikacije")

  
5. U okvir za pretraživanje upišite **dana**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-workday-inbound-tutorial/IC701021.png "Dodavanje aplikacije iz gallerry")

6. U oknu s rezultatima odaberite dana, a zatim dovršeno da biste dodali aplikaciju.

    ![Galerija aplikacije] (./media/active-directory-saas-workday-inbound-tutorial/IC701022.png "Galerija aplikacije")





## <a name="creating-an-integration-system-user"></a>Stvaranje korisnik sustava integraciju sustava

### <a name="steps"></a>Koraci:


1. **Workday Workbench**unesite Stvaranje korisnika u okvir za pretraživanje, a zatim kliknite **Create User integraciju sustava**. 

    ![Stvaranje korisnika] (./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Stvaranje korisnika")



2. Dovršavanje zadatka **Create User integraciju sustava** po unošenju podataka korisničko ime i lozinku za novi korisnik sustava integracije.  Ostavite zahtijevaju novu lozinku na sljedeći znak u mogućnost poništen, jer se taj korisnik će se prijave programski. Ostavite u minutama sesije s zadanu vrijednost od 0, koja će spriječiti korisnika sesije prekoračenja prerano. 

    ![Stvaranje korisnika sustava Integracija] (./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Stvaranje korisnika sustava Integracija")
 




## <a name="creating-a-security-group"></a>Stvaranje sigurnosne grupe

Scenarija navedene u ovom ćete praktičnom vodiču, morate stvoriti sigurnosnu grupu sustava sustava ograničenja integracije i dodjela korisnika.

### <a name="steps"></a>Koraci:

1. Unesite stvaranje sigurnosne grupe u okvir za pretraživanje, a zatim kliknite **Stvori sigurnosnu grupu**. 

    ![Grupa CreateSecurity] (./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "Grupa CreateSecurity")
 

2. Dovršavanje zadatka stvaranje sigurnosne grupe.  Odaberite sigurnosne grupe za integraciju sustava – ograničenja na padajućem izborniku Vrsta unajmljenim sigurnosne grupe da biste stvorili sigurnosnu grupu kojoj članovi izričito dodaje se. 

    ![Grupa CreateSecurity] (./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "Grupa CreateSecurity")
 



## <a name="assigning-the-integration-system-user-to-the-security-group"></a>Dodjeljivanje korisnika za integraciju sustava sigurnosne grupe

### <a name="steps"></a>Koraci:


1. Unesite uređivanje sigurnosne grupe u okvir za pretraživanje, a zatim kliknite **Uređivanje sigurnosne grupe**. 

    ![Uređivanje sigurnosne grupe] (./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Uređivanje sigurnosne grupe")
 
 

2. Pronađite i odaberite Nova grupa za sigurnost Integracija po nazivu. 

    ![Uređivanje sigurnosne grupe] (./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Uređivanje sigurnosne grupe")

 

3. Dodavanje novog korisnika sustava za integraciju u novu grupu sigurnost. 

    ![Sustav sigurnosne grupe] (./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Sustav sigurnosne grupe")  




## <a name="configuring-security-group-options"></a>Konfiguriranje mogućnosti za sigurnosne grupe

U ovom ćete koraku dodijeliti nove sigurnosne grupe dozvole za **Početak** i **postavili** operacije na objekte osigurava sljedeće sigurnosnih pravilnika za domenu:

- Dodjeljivanje vanjskih računa

- Tempiranja podataka: Izvješća javno tempiranja

- Tempiranja podataka: Sve položaje

- Tempiranja podataka: Trenutni organiziraju informacije

- Tempiranja podataka: Naslov tvrtke na profilu tempiranja

 
### <a name="steps"></a>Koraci:

1. U okvir za pretraživanje unesite domenu sigurnosna pravila, a zatim na vezu, domene sigurnosne pravilnike za funkcionalno područje.  

    ![Domene sigurnosna pravila] (./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Domene sigurnosna pravila")  
 

2. Potražite sustava, a zatim odaberite **sustav** funkcionalno područje.  Kliknite **u redu**.  

    ![Domene sigurnosna pravila] (./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Domene sigurnosna pravila")  


3. Na popisu sigurnosne pravilnike za sustav funkcionalno područje proširite administriranje sigurnosti i odaberite sigurnosna pravila domene u vanjskim dodjele resursa računa.  

    ![Domene sigurnosna pravila] (./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Domene sigurnosna pravila")  


4. Kliknite **Uredi dozvole**, a zatim na stranici dijaloški okvir **Uređivanje dozvola**Dodaj novu grupu sigurnosti na popis sigurnosne grupe s dozvolama za integraciju **dobiti** i **postavili** . 

    ![Uređivanje dozvola] (./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Uređivanje dozvola")  

 

5. Ponovite korak 1, iznad da biste se vratili na zaslon za odabir područja funkcionalnosti, a ovaj put traženje organiziraju, odaberite područje funkcionalni Staffing pa kliknite **u redu**.

    ![Domene sigurnosna pravila] (./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Domene sigurnosna pravila")  
 

6. Na popisu sigurnosne pravilnike za Staffing funkcionalno područje proširite tempiranja podataka: Staffing, a zatim ponovite korak 4 iznad za svaki od ovih preostalo sigurnosna pravila:

     - Tempiranja podataka: Izvješća javno tempiranja

     - Tempiranja podataka: Sve položaje

     - Tempiranja podataka: Trenutni organiziraju informacije

     - Tempiranja podataka: Naslov tvrtke na profilu tempiranja


    ![Domene sigurnosna pravila] (./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Domene sigurnosna pravila")  







## <a name="activating-security-policy-changes"></a>Aktiviranje promjena sigurnosnih pravilnika

### <a name="steps"></a>Koraci:

1. Unesite aktivirati u okvir za pretraživanje, a zatim kliknite na vezu, aktivirati sigurnosna pravila promjene na čekanju. 

    ![Aktiviranje] (./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Aktiviranje") 
 

2. Početak zadatka aktivirati sigurnosna pravila promjene na čekanju tako da unesete komentar nadzora svrhe, a zatim **u redu**. 

    ![Aktiviranje na čekanju sigurnosti] (./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Aktiviranje na čekanju sigurnosti")   
 

3. Dovršavanje zadatka na sljedećem zaslonu tako da poništite potvrdni okvir s natpisom potvrdi, a zatim kliknite **u redu**. 

    ![Aktiviranje na čekanju sigurnosti] (./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Aktiviranje na čekanju sigurnosti")  





## <a name="configuring-user-import-in-azure-ad"></a>Konfiguriranje korisnički uvoz u Azure AD

Cilj ovaj odjeljak je Strukturiranje kako konfigurirati Azure AD za uvoz osobe iz dana.


### <a name="steps"></a>Koraci:


1. Na stranici za integraciju aplikacije **Workday** kliknite **Uvoz Konfiguracija korisnika** da biste otvorili dijaloški okvir **Konfiguriranje dodjele resursa** .


2. Na stranici **Postavke i administratorske vjerodajnice** poduzeti sljedeće korake, a zatim kliknite **Dalje**: 

    ![Postavke i administratorske vjerodajnice] (./media/active-directory-saas-workday-inbound-tutorial/IC750995.png "Postavke i administratorske vjerodajnice")  
 
    na. U Workday administrator korisnika tekstni okvir Naziv upišite naziv korisnika koji ste stvorili u na stvaranje odjeljak integraciju sustava korisnika.

    b. U Workday administratorske lozinke tekstni okvir, unesite lozinku za korisnika koji ste stvorili u na stvaranje odjeljak integraciju sustava korisnika.

    c. U Workday klijentu URL tekstni okvir, unesite URL-a ili Workday klijentu.


3. Na stranici **Testiranje veze** kliknite da biste potvrdili povezivanje **pokrenite test** , a zatim kliknite **Dalje**. 

    ![Testiranje veze] (./media/active-directory-saas-workday-inbound-tutorial/IC750996.png "Testiranje veze")  
 

4. Na stranici s mogućnostima **Provisioning** kliknite **Dalje**. 

    ![Mogućnosti za dodjelu resursa] (./media/active-directory-saas-workday-inbound-tutorial/IC750997.png "Mogućnosti za dodjelu resursa")


5. U dijaloškom okviru **pokretanje dodjele resursa** kliknite **dovrši**. 

    ![Pokretanje dodjele resursa] (./media/active-directory-saas-workday-inbound-tutorial/IC750998.png "Pokretanje dodjele resursa")
 

Sada možete prijeđite na odjeljak **korisnici** i provjerite je li korisnički Workday uvezena.



## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)
