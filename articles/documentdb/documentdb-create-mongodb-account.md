<properties 
    pageTitle="Stvaranje računa DocumentDB s podrškom za protokol za MongoDB | Microsoft Azure" 
    description="Saznajte kako stvoriti DocumentDB račun s podrškom za protokol za MongoDB, sada dostupni za pretpregled." 
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2016" 
    ms.author="anhoh"/>

# <a name="how-to-create-a-documentdb-account-with-protocol-support-for-mongodb-using-the-azure-portal"></a>Kako stvoriti račun DocumentDB s podrškom za protokol za MongoDB pomoću portala za Azure

Da biste stvorili račun za Azure DocumentDB protokol podrška za MongoDB, morate:

- Imate račun za Azure. [Besplatni račun za Azure](https://azure.microsoft.com/free/) možete dobiti ako još nemate jedno mjesto.

## <a name="create-the-account"></a>Stvaranje računa  

Da biste stvorili DocumentDB račun s podrškom za protokol za MongoDB, poduzeti sljedeće korake.

1. U novom prozoru, prijavite se na [Portal za Azure](https://portal.azure.com).
2. Kliknite **NOVO**, kliknite **podataka + prostor za pohranu**, kliknite **Pogledajte sve**, a pretraživanje kategorija **podataka + prostor za pohranu** za "DocumentDB protokol". Kliknite **DocumentDB - protokol za podršku za MongoDB**.

    ![Snimka zaslona trgovine i podataka + prostor za pohranu pretraživanje blades, isticanje DocumentDB - protokol za podršku za MongoDB, Mongo baze podataka](./media/documentdb-create-mongodb-account/marketplacegallery2.png)

3. Umjesto toga, u kategoriji **podataka + prostor za pohranu** , u odjeljku **Spremanje**, kliknite **više**, a zatim **učitati dodatne** jedan ili više puta da biste prikazali **DocumentDB - protokol za podršku za MongoDB**. Kliknite **DocumentDB - protokol za podršku za MongoDB**.

    ![Snimka zaslona s trgovine i podataka + prostor za pohranu blades, isticanje DocumentDB - protokol za podršku za MongoDB, Mongo baze podataka](./media/documentdb-create-mongodb-account/marketplacegallery1.png)

4. U plohu **DocumentDB - protokol za podršku za MongoDB (pretpregled)** kliknite **Stvori** da biste pokrenuli proces prijave za pretpregled.

    ![DocumentDB - protokol za podršku za MongoDB plohu na portalu za Azure](./media/documentdb-create-mongodb-account/marketplacegallery3.png)

5. U plohu **DocumentDB računa** kliknite **prijavite se da biste pretpregledali**. Pročitajte informacije, a zatim kliknite **u redu**.

    ![Znak najviše pretpregled danas plohu za DocumentDB - protokol za podršku za MongoDB na portalu za Azure](./media/documentdb-create-mongodb-account/registerforpreview.png)

6.  Nakon prihvaćanja uvjete pretpregled, vratit ćete se na plohu Stvori.  U plohu **DocumentDB računa** navedite željena konfiguracija računa.

    ![Snimka zaslona novog DocumentDB s podrškom za protokol za plohu MongoDB](./media/documentdb-create-mongodb-account/create-documentdb-mongodb-account.png)


    - U okviru **ID-a** unesite naziv za prepoznavanje računa.  Kada se provjerava **ID-a** , u okviru **ID** pojavit će se Zelena kvačica. Vrijednost za **ID-a** postaje naziv glavnog računala unutar URI. **ID-a** mogu sadržavati samo mala slova, brojeve i "-" znakova i mora biti između 3 i 50 znakova. Imajte na umu *documents.azure.com* dodaju se na krajnjoj točki naziv odaberete, rezultat koji će postati krajnjoj točki vaš račun.

    - Za **pretplate**odaberite pretplatu za Azure koju želite koristiti za račun. Ako račun sadrži samo jedan pretplatu, taj račun nije odabrano po zadanom.

    - U **Grupi resursa**, odaberite ili stvorite grupu resursa za račun.  Po zadanom će biti odabran postojeću grupu resursa u odjeljku Azure pretplate.  Međutim, možete odabrati da biste stvorili novu grupu resursa na koji želite da biste dodali račun. Dodatne informacije potražite u članku [pomoću portala za Azure upravljanja Azure resurse](resource-group-portal.md).

    - Da biste odredili zemljopisnu lokaciju u kojem će biti račun koristiti **mjesto** .
    
    - Neobavezno: Potvrdite **Prikvači na nadzornoj ploči**. Ako je fiksiran nadzorne ploče, slijedite **korak 8** dolje da biste pogledali navigaciji na novi račun.

7.  Kada niste konfigurirali nove mogućnosti računa, kliknite **Stvori**.  Može potrajati nekoliko minuta da biste stvorili račun.  Ako prikvačena na nadzornu ploču, možete pratiti napredak dodjele resursa u Startboard.  
    ![Snimka zaslona pločice stvaranje Startboard – alat za stvaranje internetske baze podataka](./media/documentdb-create-mongodb-account/create-nosql-db-databases-json-tutorial-3.png)  

    Ako nije prikvačena na nadzornu ploču, možete pratiti tijek rada u središtu obavijesti.  

    ![Brzo stvaranje baze podataka – snimka zaslona s središtu obavijesti koji se prikazuje račun DocumentDB je stvoren](./media/documentdb-create-mongodb-account/create-nosql-db-databases-json-tutorial-4.png)  

    ![Snimka zaslona s središtu obavijesti koji se prikazuje da DocumentDB računa uspješno stvorena i implementiran u grupu resursa – obavijesti autor internetske baze podataka](./media/documentdb-create-mongodb-account/create-nosql-db-databases-json-tutorial-5.png)

8.  Da biste pristupili na novi račun, na izborniku s lijeve strane kliknite **DocumentDB (NoSQL)** . Na popisu običnog DocumentDB i DocumentDB s računima za podršku protokol Mongo kliknite naziv novog računa.

9.  Sada je spremna za korištenje sa zadanim postavkama. 

    ![Snimka zaslona s računa plohu zadani](./media/documentdb-create-mongodb-account/defaultaccountblades.png)
    

## <a name="next-steps"></a>Daljnji koraci


- Saznajte kako da biste se [povezali](documentdb-connect-mongodb-account.md) s računom DocumentDB s protokolom podrške za MongoDB.

 
