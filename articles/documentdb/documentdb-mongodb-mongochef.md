<properties 
    pageTitle="Korištenje MongoChef s računom DocumentDB s podrškom za protokol za MongoDB | Microsoft Azure" 
    description="Saznajte kako koristiti MongoChef s računom DocumentDB s podrškom za protokol za MongoDB, sada dostupni za pretpregled." 
    keywords="mongochef"
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
    ms.date="08/25/2016" 
    ms.author="anhoh"/>

# <a name="use-mongochef-with-a-documentdb-account-with-protocol-support-for-mongodb"></a>Korištenje MongoChef s računom DocumentDB s podrškom za protokol za MongoDB

Da biste se povezali s računom za Azure DocumentDB s podrškom za protokol za MongoDB pomoću MongoChef, morate:

- Preuzmite i instalirajte [MongoChef](http://3t.io/mongochef)
- Svoj račun DocumentDB s podrškom za protokol za MongoDB [niz za povezivanje](documentdb-connect-mongodb-account.md) podataka

## <a name="create-the-connection-in-mongochef"></a>Stvaranje veza u MongoChef  

Da biste dodali račun DocumentDB s podrškom za protokol za MongoDB Upravitelja veze MongoChef, poduzeti sljedeće korake.

1. Dohvatiti vaše DocumentDB s podrškom za protokol za podatke o vezi MongoDB prema uputama [u nastavku](documentdb-connect-mongodb-account.md).

    ![Snimka zaslona s plohu niz za povezivanje](./media/documentdb-mongodb-mongochef/ConnectionStringBlade.png)

2. Kliknite **Poveži** da biste otvorili Upravitelj veze, a zatim kliknite **Nova veza**

    ![Snimka zaslona s upraviteljem veze MongoChef](./media/documentdb-mongodb-mongochef/ConnectionManager.png)
    
2. U prozoru **Nove veze** na kartici **poslužitelj** unesite glavnog računala (FQDN) DocumentDB računa s podrškom za protokol za web-mjesto MongoDB i u okvir za PRIKLJUČAK.
    
    ![Snimka zaslona kartice MongoChef veza Upravitelj poslužitelja](./media/documentdb-mongodb-mongochef/ConnectionManagerServerTab.png)

3. U prozoru **Nove veze** na kartici **provjere autentičnosti** odaberite način provjere autentičnosti **Standardno (MONGODB CR ili SCARM-SHA-1)** , a zatim unesite korisničko ime i lozinku.  Prihvatite zadani provjera autentičnosti baze podataka (administratora) ili vlastite vrijednost.

    ![Snimka zaslona s karticom za provjeru autentičnosti Upravitelj MongoChef veze](./media/documentdb-mongodb-mongochef/ConnectionManagerAuthenticationTab.png)

4. U prozoru **Nove veze** na kartici **SSL** potvrdite potvrdni okvir **Koristi SSL protokol za povezivanje** i izborni gumb **Prihvati samopotpisane potvrde SSL** .

    ![Snimka zaslona s karticom SSL za Upravitelj MongoChef veze](./media/documentdb-mongodb-mongochef/ConnectionManagerSSLTab.png)

5. Kliknite gumb **Testiraj vezu** da biste podatke o vezi za provjeru valjanosti, kliknite **u redu** da biste se vratili u prozor novu vezu, a zatim **Spremi**.

    ![Snimka zaslona s prozorom za povezivanje MongoChef test](./media/documentdb-mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-to-create-a-database-collection-and-documents"></a>Stvaranje baze podataka, zbirke i dokumenata pomoću MongoChef  

Prikupljanje i dokumenata pomoću MongoChef, da biste stvorili bazu podataka, izvršite sljedeće korake.

1. U **Upravitelja veze**, istaknite vezu, a zatim kliknite **Poveži**.

    ![Snimka zaslona s upraviteljem veze MongoChef](./media/documentdb-mongodb-mongochef/ConnectToAccount.png)

2. Desnom tipkom miša kliknite glavno računalo, a zatim odaberite **Dodaj baze podataka**.  Navedite naziv baze podataka, a zatim kliknite **u redu**.
    
    ![Snimka zaslona mogućnosti za dodavanje MongoChef baze podataka](./media/documentdb-mongodb-mongochef/AddDatabase1.png)

3. Desnom tipkom miša kliknite bazu podataka, a zatim odaberite **Dodaj zbirku**.  Navedite naziv zbirke, a zatim kliknite **Stvori**.

    ![Snimka zaslona s mogućnosti Dodaj zbirku MongoChef](./media/documentdb-mongodb-mongochef/AddCollection.png)

4. Kliknite stavku izbornika **zbirke** , a zatim kliknite **Dodaj dokument**.

    ![Snimka zaslona s stavku izbornika MongoChef Dodaj dokument](./media/documentdb-mongodb-mongochef/AddDocument1.png)

5. U dijaloškom okviru Dodavanje dokumenta zalijepite sljedeće, a zatim kliknite **Dodaj dokument**.

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
            { "firstName": "Thomas" },
            { "firstName": "Mary Kay"}
        ],
        "children": [
        {
            "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
            "pets": [{ "givenName": "Fluffy" }]
        }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }

    
6. Dodavanje drugog dokumenta, ovaj put na sljedeće sadržaju.

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                "givenName": "Lisa", 
                "gender": "female", 
                "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }

7. Izvršavanje upita uzorka. Ako, na primjer, potražite linije ime i Prezime "Andersen" i povratna roditeljima i stanje polja.

    ![Snimka zaslona s Mongo Chef rezultata upita](./media/documentdb-mongodb-mongochef/QueryDocument1.png)
    

## <a name="next-steps"></a>Daljnji koraci

- Istražite DocumentDB s podrškom za protokol za MongoDB [uzorka](documentdb-mongodb-samples.md).

 
