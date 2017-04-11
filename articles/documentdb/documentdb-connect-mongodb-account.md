<properties 
    pageTitle="Povezivanje s računom DocumentDB s podrškom za protokol za MongoDB | Microsoft Azure" 
    description="Saznajte kako se povezati s računom DocumentDB s podrškom za protokol za MongoDB, sada dostupni za pretpregled. Povežite se pomoću niza za povezivanje MongoDB." 
    keywords="niz za povezivanje mongodb"
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
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="how-to-connect-to-a-documentdb-account-with-protocol-support-for-mongodb"></a>Kako se povezati s računom DocumentDB s podrškom za protokol za MongoDB

Saznajte kako se povezati s računom za Azure DocumentDB s podrškom za protokol za MongoDB pomoću standardnih oblika URI MongoDB veze niz.  

## <a name="get-the-accounts-connection-string-information"></a>Dohvati podatke niz za povezivanje s računom

1. U novom prozoru, prijavite se na [Portal za Azure](https://portal.azure.com).
2. U **Lijevom navigacijskom oknu** traku plohu računa kliknite **Niz za povezivanje**. Da biste došli do **Plohu račun**, na na Jumpbar kliknite **Više usluge**, kliknite **DocumentDB (NoSQL)** , a zatim račun DocumentDB s podrškom za protokol za MongoDB.

    ![Snimka zaslona s plohu sve postavke](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. **Informacija niza veze** plohu otvara i sadrži sve podatke koje su potrebne za povezivanje s računom pomoću upravljački program za MongoDB, uključujući unaprijed izgrađene veza_niz.

    ![Snimka zaslona s plohu niz za povezivanje](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Preduvjeti za niz veze

Važno je da Imajte na umu da DocumentDB podržava standardnu MongoDB veze niz URI oblik, u nekoliko preduvjete: DocumentDB računi zahtijeva provjeru autentičnosti i sigurnu komunikaciju putem SSL.  Stoga je format niza za povezivanje:

    mongodb://username:password@host:port/[database]?ssl=true

Mjesto vrijednosti niz dostupne su u niz za povezivanje plohu gore navedenoj sintaksi.

- Korisničko ime (obavezno)
    - Naziv računa DocumentDB
- Lozinka (obavezno)
    - Lozinka računa DocumentDB
- Glavno računalo (obavezno)
    - FQDN DocumentDB računa
- Port (obavezno)
    - 10250
- Baza podataka (neobavezno)
    - Zadanu bazu podataka koristi vezu
- SSL = true (obavezno)

Na primjer, razmislite o računu prikazani u iznad informacija niza veze.  Valjani niz za povezivanje je:
    
    mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## <a name="connecting-with-the-c-driver-for-mongodb"></a>Povezivanje s C# upravljački program za MongoDB
Spomenuti već, svi računi DocumentDB zahtijeva provjeru autentičnosti i sigurnu komunikaciju putem SSL. Dok je ssl za podržava format URI za MongoDB niza za povezivanje = parametra niza upita true, rad s na MongoDB C# upravljački program zahtijeva korištenje objekta MongoClientSettings prilikom pisanja na MongoClient.  Uz gore navedene podatke o računu, sljedeće koda pokazuje kako se povezati s računom i rad s bazom podataka "Zadaci".

            MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
    

## <a name="next-steps"></a>Daljnji koraci


- Saznajte kako [koristiti MongoChef](documentdb-mongodb-mongochef.md) s računom DocumentDB s protokolom podrške za MongoDB.
- Istražite DocumentDB s podrškom za protokol za MongoDB [uzorka](documentdb-mongodb-samples.md).

 
