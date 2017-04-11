<properties
    pageTitle="Upravljanje pristupom prijava analitiku | Microsoft Azure"
    description="Upravljanje pristupom zapisnika analize pomoću raznih administrativne zadatke na korisnike, račune, OMS radnih prostora i Azure računi."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="banders"/>

# <a name="manage-access-to-log-analytics"></a>Upravljanje pristupom zapisnika Analytics

Da biste upravljali pristup prijava analitiku, koristit ćete raznih administrativne zadatke na korisnike, račune, OMS radnih prostora i Azure računi. Za stvaranje novog radnog prostora u na operacije upravljanja paket (OMS) odaberite naziv radnog prostora povezati s vašim računom, a zatim odaberite geografske mjesto. Radni prostor je zapravo spremnik koja sadrži podatke o računu i podatke o konfiguraciji jednostavne za račun. Vi ili drugi članovi vaše tvrtke ili ustanove mogu pomoću više OMS radnih prostora za upravljanje različite skupove podataka koji se prikupljaju iz svih ili dijelove IT infrastrukturu.

U članku [Početak rada s prijava analitiku](log-analytics-get-started.md) pokazuje kako brzo počnite i radi li i ostale u ovom se članku opisuje detaljno neke od akcija morat ćete Upravljanje pristupom OMS.

Iako vam možda ne trebaju za obavljanje svakog zadatka upravljanja isprva, ćemo objasniti sve najčešće zadatke koje koristite u sljedećim odjeljcima:

- Određivanje broja radnih prostora koje je potrebno
- Upravljanje računima i korisnika
- Dodajte grupu na postojeći radni prostor
- Povezivanje postojećeg radnog prostora Azure pretplate
- Nadogradnja radnog prostora na tarifu plaćenu podataka
- Promijenite vrstu podataka plan
- Dodavanje Azure Active Directory tvrtke ili ustanove na postojeći radni prostor
- Zatvorite OMS radnog prostora

## <a name="determine-the-number-of-workspaces-you-need"></a>Određivanje broja radnih prostora koje je potrebno

Radni prostor nije Azure resursa i spremniku gdje koji se prikupljaju, pridružuje, analizirati i prezentirati na portalu OMS podataka.

Za stvaranje radnih prostora za više OMS prijava analitiku moguće je i korisnici mogu pristupiti jedan ili više radnih prostora. Općenito govoreći želite smanjiti broj radnih prostora kao što je to omogućuje upita, a zatim povezivanje preko Većina podataka. U ovom se odjeljku opisuju kada može biti korisno da biste stvorili više prostora.

Danas, radni prostor analize zapisnika omogućuje:

- Zemljopisnu lokaciju pohrana podataka
- Preciznosti za naplatu
- Odvajanja podataka

Na temelju iznad karakteristike, trebali biste stvoriti više radnih prostora ako:

- Su globalni tvrtke i potrebne podatke pohranjene u određena područja podataka samostalnosti ili usklađenost razloga.
- Koristite Azure i želite radi izbjegavanja naknada za prijenos izlaznog podataka tako da vas radni prostor prijava analitiku u području isti kao Azure resurse ga upravlja.
- Želite dodijeliti naknade za različite odjele ili poslovne grupe na temelju njihove upotrebe. Prilikom stvaranja radnog prostora za svakog odjela ili grupu tvrtke Azure izjava računa i korištenje zasebno prikazuje naknade za svaki radni prostor.
- Su upravljani usluga i potrebno da se podaci zapisnika analytics za svakog korisnika kojima upravljate Izolirani iz drugih korisničkih podataka.
- Upravljanje većem broju klijenata i želite da se svaki klijenta ili odjel ili poslovne grupe da biste vidjeli vlastite podatke, ali ne i podatke za druge korisnike ili odjela ili poslovne grupe.

Prilikom korištenja agenata da biste prikupili podatke, možete konfigurirati svaki pojedini agent za potreban prostor.

Ako koristite sustav centar Operations manager svake grupe upravljanje Operations Manager moguće je povezati s samo jedan radni prostor. Možete instalirati Microsoft Agent za nadzor na računala upravlja Operations Manager, a izvješće agent za komponentu Operations Manager i drugi prijava analitiku radni prostor.

### <a name="workspace-information"></a>Informacije o radnom prostoru

Na portalu OMS prikaz informacija o radnog prostora i odaberite želite li primati informacije od Microsofta.

#### <a name="view-workspace-information"></a>Prikaz radnog prostora informacija

1. U OMS, kliknite pločicu **Postavke** .
2. Kliknite karticu **Poslovni subjekti** .
3. Kliknite karticu **Informacija o radnom prostoru** .  
  ![Informacije o radnom prostoru](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Upravljanje računima i korisnika

Pojedinom radnom prostoru možete imati više korisničkih računa koji su povezani i svaki korisnički račun (Microsoftov račun ili račun tvrtke ili ustanove) možete pristupiti više OMS radne prostore.

Prema zadanim postavkama, Microsoftov račun ili račun tvrtke ili ustanove koji se koriste za stvaranje radnog prostora postaje Administrator radnog prostora. Administrator možete pozvati dodatne račune Microsoft ili odaberite korisnika iz servisa Azure Active Directory.

Što ljudi omogućuje pristup s radnim prostorom OMS se kontrolira 2 mjesta:

- U Azure, kontrola pristupa na temelju uloga možete koristiti za pristup Azure pretplate i pridružene resurse za Azure. Ovo je koristiti za pristup PowerShell i REST API-JA.
- Na portalu OMS pristupiti samo OMS portalu - niste povezani Azure pretplatu.

Ako korisnicima dopuštate pristup portalu OMS, ali ne i Azure pretplatu koja je povezana s, zatim rješenje pločice Automatizacija, sigurnosno kopiranje i vraćanje web-mjesta neće prikazivati podatke korisnicima kada ih prijavu na portal OMS.

Da biste svim korisnicima da biste vidjeli podatke u ova rješenja, provjerite je li imaju barem **čitač** pristup za sigurnog račun Automatizacija, sigurnog sigurnosne kopije i oporavak web-mjesta koja je povezana s radnim prostorom OMS.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Upravljanje pristupom prijava analitiku pomoću portala za Azure

Ako korisnicima dopuštate pristup s radnim prostorom prijava analitiku korištenja Azure dozvola, na portalu za Azure, na primjer, zatim na isti korisnici mogu pristupati portalu zapisnika analize. Li korisnici na portalu za Azure, oni možete pristupiti OMS portal klikom na zadatak **OMS Portal** prilikom pregledavanja resursa prijava analitiku radnog prostora.

Neke upućuje treba imati na umu o portalu Azure:

- Ovo nije *na temelju uloga kontrola pristupa*. Ako imate dozvole za pristup *čitač* na portalu za Azure prijava analitiku radnog prostora, možete raditi promjene pomoću portala za OMS. Portal za OMS ima pojam administratora, suradnika i korisnik samo za čitanje. Ako je račun za koji su prijavljeni pomoću servisa Azure Active Directory povezan s radnim prostorom će biti Administrator na portalu OMS, u suprotnom će biti suradnik.

- Kada se prijavite u OMS portal pomoću http://mms.microsoft.com, zatim prema zadanim postavkama, vidjet ćete popis **Odabir radnog prostora** . Sadrži samo radne prostore koji su dodani pomoću portala za OMS. Da biste vidjeli radnim prostorima kojima imate pristup, a uz Azure pretplate, a zatim navedite klijentom kao dio URL-a. Ako, na primjer:

  `mms.microsoft.com/?tenant=contoso.com`Identifikator klijenta često je zadnji dio adresu e-pošte koja se prijavite u s.

- Ako je račun koji prijavite se pomoću računa u klijentu Azure Active Directory, koji je obično slučaj osim ako ste prijava u obliku CSP-u, tada će biti *Administrator* na portalu OMS. Ako je vaš račun nije u klijentu Azure Active Directory, bit će *korisnik* na portalu OMS.

- Ako želite da biste prešli izravno na portalu imate pristup koristi Azure dozvole, morate navesti resursa kao dio URL-a. Nije moguće dobiti u ovom URL-a pomoću komponente PowerShell.

  Na primjer, `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  URL će izgledati:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`


### <a name="managing-users-in-the-oms-portal"></a>Upravljanje korisnicima na portalu OMS

Vi upravljate korisnici i grupe na kartici **Upravljanje korisnicima** na kartici **računi** na stranici Postavke. Postoji, možete izvršiti zadatke u sljedećim odjeljcima.  

![Upravljanje korisnicima](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Dodavanje korisnika u grupu postojeće radnog prostora

Poduzmite sljedeće korake da biste dodali korisnika ili grupu s radnim prostorom OMS. Korisnik ili grupa moći pregledavati i djelovanje na sva upozorenja koja se povezuju s ovom radnom prostoru.

>[AZURE.NOTE] Ako želite da biste dodali korisnika ili grupu s računa tvrtke ili ustanove Azure Active Directory, morate provjeriti da ste pridružili računa OMS domene servisa Active Directory. Pročitajte članak [Dodavanje Azure Active Directory tvrtki ili ustanovi postojeće radnog prostora](#add-an-azure-active-directory-organization-to-an-existing-workspace).

1. U OMS, kliknite pločicu **Postavke** .
2. Kliknite karticu **Poslovni subjekti** , a zatim kliknite karticu **Upravljanje korisnicima** .
3. U odjeljku **Upravljanje korisnicima** odaberite vrstu računa da biste dodali: **Račun tvrtke ili ustanove**, **Microsoftov račun**, **Microsoftovoj službi za podršku**.
    - Ako odaberete Microsoft Account, upišite adresu e-pošte korisnika vezan uz Microsoft Account.
    - Ako odaberete račun tvrtke ili ustanove, možete unijeti dio korisnika ili naziv ili e-pošte pseudonim grupe i prikazat će se popis korisnika i grupa. Odaberite korisnika ili grupu.
    - Koristite Microsoft Support da bi se dobilo Microsoftove Support inženjeringom privremeni pristup radnog prostora za pomoć za otklanjanje poteškoća.

    >[AZURE.NOTE] Da biste postigli najbolje rezultate performanse ograničavanje broja servisa Active Directory grupe koji su povezani s jednog računa OMS na tri – jedan za administratore, jedan za suradnika i jedan za korisnike koji su samo za čitanje. Koristite dodatne grupe može utjecati na performanse zapisnika analize.

5. Odaberite željenu vrstu korisnika ili grupu da biste dodali: **Administrator**, **Suradnik**ili **Korisnik samo za čitanje** .  
6. Kliknite **Dodaj**.

  Ako dodajete Microsoftov račun, na pozivnicu za radni prostor se šalje e-pošte koje ste naveli. Kada korisnik slijedi upute u pozivnicu za OMS, korisnik može pregledavati upozorenja i podataka o računu za taj račun OMS i moći ćete vidjeti podatke o korisniku na kartici **računi** stranice **Postavke** .
  Ako dodajete račun tvrtke ili ustanove, korisnik će moći pristupiti prijava analitiku odmah.  
  ![pozivnicu e-pošte](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Uređivanje postojeće vrste korisnika

Uloga poslovnog kontakta za korisnika koji je povezan s vašim računom OMS možete promijeniti. Imate sljedeće mogućnosti uloga:

 - *Administrator*: možete Upravljanje korisnicima, prikaz i djelovanje na sva upozorenja te dodavanje i uklanjanje poslužitelja

 - *Suradnik*: možete pogledati i djelovanje na sva upozorenja te dodavanje i uklanjanje poslužitelja

 - *Samo za čitanje korisnika*: korisnici označena je samo za čitanje neće moći:
   1. Dodavanje i uklanjanje rješenja. Galerija rješenja je skriven.
   2. Dodavanje i izmjena/uklanjanje pločica na **Moje nadzorne ploče**.
   3. Prikaz stranice s **postavkama** . Stranica je skriven.
   4. Zadaci su skriveni u pretraživanje prikaz, PowerBI konfiguracije, spremiti pretraživanja i upozorenja.


#### <a name="to-edit-an-account"></a>Da biste uredili računa

1. U OMS, kliknite pločicu **Postavke** .
2. Kliknite karticu **Poslovni subjekti** , a zatim kliknite karticu **Upravljanje korisnicima** .
3. Odaberite ulogu za korisnika kojeg želite promijeniti.
2. U dijaloškom okviru za potvrdu kliknite **da**.

### <a name="remove-a-user-from-a-oms-workspace"></a>Uklanjanje korisnika iz radnog prostora OMS

Poduzmite sljedeće korake da biste korisnika uklonili iz radnog prostora programa OMS. Imajte na umu da se to zatvorite radni prostor za korisnika. Umjesto toga, uklanja veza između tog korisnika i radnom prostoru. Ako je korisnik povezan s više radnih prostora, korisnik će i dalje moći prijavite se u OMS i radnim prostorima potražite u članku.

1. U OMS, kliknite pločicu **Postavke** .
2. Kliknite karticu **Poslovni subjekti** , a zatim kliknite karticu **Upravljanje korisnicima** .
3. Kliknite **Ukloni** uz korisničko ime koje želite ukloniti.
4. U dijaloškom okviru za potvrdu kliknite **da**.


### <a name="add-a-group-to-an-existing-workspace"></a>Dodajte grupu na postojeći radni prostor

1.  Slijedite korake od 1 -4 u "Za dodavanje korisnika u postojeći radni prostor", iznad.
2.  U odjeljku **Odaberite korisnika/grupe**odaberite **grupu**.
    ![Dodajte grupu na postojeći radni prostor](./media/log-analytics-manage-access/add-group.png)
3.  Unesite adresu zaslonsko ime ili e-pošte za grupu koju želite dodati.
4.  Odaberite grupu u rezultatima popisa, a zatim kliknite **Dodaj**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Povezivanje postojećeg radnog prostora Azure pretplate

Nije moguće stvoriti radni prostor s [microsoft.com/oms](https://microsoft.com/oms) web-mjesta.  Međutim, određene postoje ograničenja za ove radne prostore najviše najvažnije ako koristite pomoću računa koji se ograničenje od 500MB/dan prijenosa podataka. Da biste promijenili prostora morat ćete *povezati postojeće radnog prostora Azure pretplatu*.

>[AZURE.IMPORTANT] Da biste povezali radnog prostora programa Groove, račun za Azure morate već imati pristup u radni prostor koji se želite povezati.  Drugim riječima, račun koji koristite za pristup portalu Azure mora biti **isti** kao račun koji koristite za pristup OMS radnog prostora. Ako to nije tako, pročitajte članak [Dodavanje korisnika u postojeći radni prostor](#add-a-user-to-an-existing-workspace).

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Da biste se povezali radnog prostora programa Groove Azure pretplate na portalu OMS

Da biste povezali radnog prostora programa Groove Azure pretplate na portalu OMS, prijavljeni korisnik mora već plaćenu račun za Azure. Radni prostor koji aktivno koristite dobiva povezana s račun za Azure.

1. U OMS, kliknite pločicu **Postavke** .
2. Kliknite karticu **Poslovni subjekti** , a zatim kliknite karticu **Azure pretplate i podatkovne tarife** .
3. Kliknite podatkovne tarife na koje želite koristiti.
4. Kliknite **Spremi**.  
  ![tarife za pretplatu i podataka](./media/log-analytics-manage-access/subscription-tab.png)

Nove podatkovne tarife prikazuje se na vrpci OMS portala pri vrhu web-stranice.

![OMS vrpce](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Da biste se povezali radnog prostora programa Groove Azure pretplate na portalu za Azure

1.  Prijavite se na [portal za Azure](http://portal.azure.com).
2.  Traženje **Prijava analitiku (OMS)** , a zatim je odaberite.
3.  Vidjet ćete popis postojećih radnih prostora. Kliknite **Dodaj**.  
    ![Popis radnih prostora](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4.  U odjeljku **OMS radnog prostora**, kliknite **ili povezivanje postojeće**.  
    ![Povezivanje postojećeg](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5.  Kliknite **Konfiguriraj potrebne postavke**.  
    ![Konfiguriranje postavki potrebna](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6.  Vidjet ćete popis radnog prostora koji još nisu povezani s računom za Azure. Odabir radnog prostora.  
    ![Odaberite radne prostore](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7.  Ako je potrebno, možete promijeniti vrijednosti za sljedeće stavke:
    - Pretplate
    - Grupa resursa
    - Mjesto
    - Cijene sloju  
        ![Promijenite vrijednosti](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8.  Kliknite **Stvori**. Radni prostor sada povezana s račun za Azure.

>[AZURE.NOTE] Ako ne vidite mogućnost radni prostor koji se želite povezati, zatim Azure pretplate nemate pristup s radnim prostorom OMS koji ste stvorili pomoću OMS web-mjesta.  Morat ćete dopustiti pristup s ovim računom unutar radnog prostora OMS korištenje OMS web-mjesta. Da biste to učinili, potražite u članku [Dodavanje korisnika u postojeći radni prostor](#add-a-user-to-an-existing-workspace).



## <a name="upgrade-a-workspace-to-a-paid-data-plan"></a>Nadogradnja radnog prostora na tarifu plaćenu podataka

Postoje tri radnog prostora Osmišljavanje vrste OMS: **slobodno**, **standardne**i **Premium**.  Ako ste tarifu *slobodno* , možda ste pritisnite vaše podatke kapaciteta 500 MB.  Morat ćete nadograditi radnog prostora na ***pay-as-you-go plan*** da biste prikupili podatke iznad te granice. U bilo kojem trenutku možete pretvoriti vrsti vašega plan.  Dodatne informacije o OMS cijene, potražite u članku [Cijene pojedinosti](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

>[AZURE.IMPORTANT] Tarife za radni prostor možete promijeniti samo ako su *povezane* Azure pretplatu.  Ako ste stvorili radni prostor u Azure ili ako ste *već* povezani radnog prostora, možete zanemariti ovu poruku.  Ako ste stvorili radni prostor s sustava [OMS web-mjesta](http://www.microsoft.com/oms), morat ćete slijedite korake na [vezu postojeće radnog prostora Azure pretplatu](#link-an-existing-workspace-to-an-azure-subscription).

### <a name="using-entitlements-from-the-oms-add-on-for-system-center"></a>Pomoću prava iz OMS dodatak za centar za sustav

Dodatak za OMS za centar za sustav sadrži do prava na plan Premium analize zapisnika OMS, što je opisano na [OMS cijene](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

Kada kupite OMS dodatak za centar za sustav, dodatak OMS dodaje se kao do prava na ugovor za centar za sustav. Sve Azure pretplate koja je stvorena odredbama ovog ugovora možete izvršiti pomoću na prava. Time, na primjer, da bi više radnih prostora OMS koje koriste prava iz OMS dodatak.

Da biste bili sigurni da korištenje radnog prostora programa OMS primjenjuje se na vaše prava iz OMS dodatak, morat ćete:

1. Veza na radni prostor OMS Azure pretplatu koja je dio Enterprise Agreement koja sadrži OMS dodatak za kupnju i korištenja Azure pretplate
2. Odaberite plan Premium radnog prostora

Nakon što ste pregledali Upotreba na portalu za Azure ili OMS, nećete vidjeti pravima OMS dodatak. Međutim, vidjet ćete prava na portalu za Enterprise.  

Ako morate promijeniti Azure pretplate povezan OMS radnog prostora, koristite cmdlet Azure PowerShell [Premjesti AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) .

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Korištenje Azure izvršenja iz Enterprise Agreement

Ako odlučite koristiti samostalne cijene za OMS komponente, plaćate za svaku komponentu OMS zasebno i korištenje pojavit će se na računu za Azure.

Ako imate Azure novčani potvrdi na registraciju enterprise koji su povezani pretplate Azure, sve korištenje zapisnika analize se automatski teretiti protiv sve preostale novčani potvrdi.

Ako morate promijeniti Azure pretplate da su radni prostor OMS povezan s koje možete koristiti cmdlet Azure PowerShell [Premjesti AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) .  



### <a name="to-change-a-workspace-to-a-paid-data-plan"></a>Da biste promijenili radnog prostora na tarifu plaćenu podataka

1.  Prijavite se na [portal za Azure](http://portal.azure.com).
2.  Traženje **Prijava analitiku (OMS)** , a zatim je odaberite.
3.  Vidjet ćete popis postojećih radnih prostora. Odabir radnog prostora.  
    ![Popis radnih prostora](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4.  U odjeljku **Postavke**kliknite **određivanje cijena sloju**.  
    ![cijene sloju](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5.  U odjeljku **sloju određivanje cijena**, odaberite plan podataka, a zatim **Odaberite**.  
    ![Odaberite plan](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6.  Kada osvježite prikaz na portalu za Azure, vidjet ćete **određivanje cijena sloju** ažuriranja za tarifu koju ste odabrali.  
    ![Ažuriranje cijene sloju](./media/log-analytics-manage-access/manage-access-change-plan04.png)

Sada možete prikupiti podatke izvan kapaciteta "besplatni" podaci.


## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Dodavanje Azure Active Directory tvrtke ili ustanove na postojeći radni prostor

Radni prostor zapisnika analize (OMS) možete pridružiti domenom sustava Azure Active Directory. Omogućuje dodavanje korisnika iz servisa Active Directory izravno u radni prostor OMS ne zahtijeva zasebne Microsoftova računa.

Kada stvaranje radnog prostora na portalu Azure ili radni prostor povezati Azure pretplate bit će povezan Azure Active Directory kao računa tvrtke ili ustanove.

Prilikom stvaranja radnog prostora na portalu OMS zatražit će se povezati s web-mjesto Azure pretplata i u okvir za račun tvrtke ili ustanove.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Da biste dodali Azure Active Directory tvrtke ili ustanove na postojeći radni prostor

1. Na stranici Postavke OMS kliknite **računi** , a zatim kliknite karticu **Informacija o radnom prostoru** .  
2. Pregledajte podatke o računi tvrtke ili ustanove, a zatim kliknite **Dodaj tvrtke ili ustanove**.  
    ![Dodavanje tvrtke ili ustanove](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Unesite podatke identiteta za administratora domene Azure Active Directory. Nakon toga, vidjet ćete potvrdu koja kaže da su radni prostor povezan s vaše domene Azure Active Directory.
    ![Potvrda povezane radnog prostora](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

>[AZURE.NOTE] Kada vaš račun povezan s računa tvrtke ili ustanove, povezivanje ne može ukloniti ili promijeniti.

## <a name="close-your-oms-workspace"></a>Zatvorite OMS radnog prostora

Prilikom zatvaranja programa OMS radnog prostora, sve podatke koji se odnose na radni prostor izbriše s OMS usluga u 30 dana zatvaranja radni prostor.

Ako ste administrator, a postoje više korisnika koji su povezani s radnim prostorom, veza između tim korisnicima i radnog prostora se prekida. Ako korisnici su povezane s drugim radnim prostorima, pa ih možete nastaviti OMS pomoću te druge radne prostore. Međutim, ako nisu pridružene druge radne prostore pa ih morat ćete stvaranje novog radnog prostora da biste koristili OMS.

### <a name="to-close-an-oms-workspace"></a>Da biste zatvorili programa OMS radnog prostora

1. U OMS, kliknite pločicu **Postavke** .
2. Kliknite karticu **Poslovni subjekti** , a zatim kliknite karticu **Informacija o radnom prostoru** .
3. Kliknite **Zatvori radni prostor**.
4. Odaberite jednu od razloga za zatvaranje radni prostor ili unesite drugu razlog u tekstni okvir.
5. Kliknite **Zatvori radni prostor**.

## <a name="next-steps"></a>Daljnji koraci

- Potražite u članku [Povezivanje Windows računalima prijava analitiku](log-analytics-windows-agents.md) da biste dodali agenata i prikupljanje podataka.
- [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md) za dodavanje funkcionalnosti i njihovo prikupljanje podataka.
- [Konfiguracija postavki proxy poslužitelja i vatrozida u zapisnik analize](log-analytics-proxy-firewall.md) ako vaša tvrtka koristi proxy poslužitelj ili vatrozid tako da se agenata mogli komunicirati sa servisom zapisnika analize.
