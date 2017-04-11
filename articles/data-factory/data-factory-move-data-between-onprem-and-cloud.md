<properties 
    pageTitle="Premještanje podataka – pristupnik za upravljanje podacima | Microsoft Azure"
    description="Premještanje podataka s lokalnog poslužitelja u oblak i postavljanje pristupnika podacima. Premještanje podataka pomoću pristupnika za upravljanje podacima u tvorničke Azure podataka." 
    keywords="pristupnik za podacima, Integracija podataka, premještanje podataka, vjerodajnice za pristupnik"
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="jingwang"/>

# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Premještanje podataka s lokalnim izvorima poslužitelja u oblak i s pristupnik za upravljanje podacima
Ovaj članak sadrži pregled Integracija podataka između lokalne podatke trgovine i oblaka podataka trgovine pomoću tvorničke podataka. Sastavlja na članak [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md) i ostali članci podataka tvorničke osnovni koncepti: [skupova podataka](data-factory-create-datasets.md) i [kanali](data-factory-create-pipelines.md). 

## <a name="data-management-gateway"></a>Pristupnik za upravljanje podacima
Morate instalirati pristupnik za upravljanje podacima na vašem računalu lokalnog da biste omogućili premještanje podataka iz spremišta podataka na lokalni. Pristupnik može instalirati na istom računalu kao spremišta podataka ili na drugo računalo dok god pristupnika možete se povezati s spremišta podataka. 

> [AZURE.IMPORTANT] Pogledajte članak [Pristupnik za upravljanje podacima](data-factory-data-management-gateway.md) detalje o pristupnik za upravljanje podacima.   

Sljedeći prikaz prikazuje kako stvoriti podataka tvorničke s kanala koje premješta podatke iz lokalne baze podataka **SQL Server** Azure blobova. U prikazu, dio instalirate i konfigurirate pristupnik za upravljanje podacima na vašem računalu. 

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>Vodič: kopiranje lokalnih podataka na cloud
  
## <a name="create-data-factory"></a>Stvaranje tvorničke podataka
U ovom ćete koraku pomoću portala za Azure stvoriti instancu Azure podataka tvorničke pod nazivom **ADFTutorialOnPremDF**. 

1.  Prijavite se na [portal za Azure](https://portal.azure.com). 
2.  Kliknite **+ NOVO**, kliknite **Obavještavanje + analize**i kliknite **Tvorničke podataka**.

    ![Novi -> DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
2. U plohu **Novi tvorničke podataka** unesite **ADFTutorialOnPremDF** naziv.

    ![Dodavanje Startboard](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

    > [AZURE.IMPORTANT] 
    > Naziv tvorničke Azure podataka mora biti globalno jedinstveni. Ako vam se prikaže pogreška: **nije dostupna je naziv tvorničke podataka "ADFTutorialOnPremDF"**, promijenite naziv tvorničke podataka (primjerice, yournameADFTutorialOnPremDF) i pokušajte ponovno stvoriti. Pomoću tog naziva umjesto ADFTutorialOnPremDF prilikom izvođenja preostale korake ovog praktičnog vodiča.
    > 
    > Naziv tvorničke podataka možda registrirana kao naziv **DNS-a** u budućnosti i zato postaju javno vidljivi.
3. Odaberite **Azure pretplate** na mjesto na kojem želite da se podaci tvorničke će biti stvoren. 
4.  Odaberite postojeću **grupu resursa** ili stvorite grupu resursa. Praktični vodič, stvorite grupu resursa pod nazivom: **ADFTutorialResourceGroup**. 
5.  Kliknite **Stvori** **Novi podataka tvorničke** plohu.

    > [AZURE.IMPORTANT] Da biste stvorili instance tvorničke podataka, morate biti član uloge [Suradnika tvorničke podataka](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) na razini grupe pretplate/resursa. 
11. Po dovršetku stvaranja vidite plohu **Tvorničke podataka** kao što je prikazano na sljedećoj slici:

    ![Podaci tvorničke Početna stranica](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Stvaranje pristupnika
5. U plohu **Tvorničke podataka** kliknite **autora i implementacija** pločicu za pokretanje **uređivača** za tvorničke podataka.

    ![Stvaranje i implementacija pločica](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
6.  U podataka tvorničke Editor, kliknite **... Dodatne** na alatnoj traci kliknite **Novi pristupnik podacima**. Osim toga, možete desnom tipkom miša kliknite **Pristupnici podataka** u prikazu stabla i kliknite **Novi pristupnik podacima**. 

    ![Novi pristupnik podacima na alatnoj traci](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2. U plohu **Stvaranje** **adftutorialgateway** unesite **naziv**pa kliknite **u redu**.    

    ![Stvaranje pristupnika plohu](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)
3. U plohu **Konfiguriraj** kliknite **Instaliraj izravno na ovom računalu**. Ova akcija preuzimanje instalacijskog paketa pristupnika, instalira, konfigurira i registrira pristupnika na računalu.  

    > [AZURE.NOTE] 
    > Koristite Internet Explorer ili kompatibilne web-preglednika Microsoft ClickOnce.
    > 
    > Ako koristite Chrome, otvorite [spremište web vizualnog okvira](https://chrome.google.com/webstore/)pretraživanje pomoću ključne riječi "ClickOnce", odaberite neku od proširenja ClickOnce i instalirajte ga. 
    >  
    > Isto učinite i za Firefox (Instaliraj dodatak). Kliknite gumb **Otvori izbornik** na alatnoj traci (**tri vodoravne crte** u gornjem desnom kutu), kliknite **Dodaci**, pretraživanje pomoću ključne riječi "ClickOnce", odaberite neku od proširenja ClickOnce i instalirajte ga.    

    ![Pristupnik - konfiguriranje plohu](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Na taj način je najjednostavnije (jedan klik) za preuzimanje, instaliranje, konfiguriranje i registrirati pristupnik u jedan korak. Vidjet ćete aplikaciju **Microsoft upravitelja konfiguracije pristupnika za upravljanje podacima** nije instaliran na vašem računalu. Izvršna **ConfigManager.exe** možete pronaći i u mapi: **C:\Programske datoteke\Microsoft podataka upravljanja Gateway\2.0\Shared**.

    Preuzimanje i ručno instalirati pristupnika pomoću veze u ovom plohu i registrirati pomoću ključa u tekstni okvir **Novi KLJUČ** .
    
    Članak [Pristupnik za upravljanje podacima](data-factory-data-management-gateway.md) potražite u članku dodatne informacije o pristupnika.

    >[AZURE.NOTE] Morate biti administrator na lokalnom računalu da biste mogli instalirati i konfigurirati pristupnik za upravljanje podacima uspješno. Ostale korisnike možete dodati lokalne grupe sustava Windows **Korisnicima pristupnika za upravljanje podataka** . Članovi ove grupe možete koristiti alat za Upravitelj konfiguracijama pristupnika za upravljanje podacima da biste konfigurirali pristupnika. 

5. Pričekajte nekoliko minuta ili pričekajte da se prikaže sljedeća obavijest:

    ![Uspješna instalacija pristupnika](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png) 
6. Pokrenite aplikaciju **Upravitelja konfiguracije pristupnika za upravljanje podacima** na vašem računalu. U prozoru za **pretraživanje** upišite **Pristupnik za upravljanje podacima** da biste pristupili uslužni. Izvršna **ConfigManager.exe** možete pronaći i u mapi: **C:\Programske datoteke\Microsoft podataka upravljanja Gateway\2.0\Shared** 

    ![Upravitelj konfiguracijama pristupnika](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
6. Provjerite nalaze li se `adftutorialgateway is connected to the cloud service` poruku. Na traka stanja na dnu prikazuje **povezan sa servisa u oblaku** uz **Zelena kvačica**.

    Na kartici **Polazno** možete učiniti i sljedeće postupke: 
    - **Registrirajte** pristupnik s ključem s portala za Azure pomoću gumba Register. 
    - **Zaustavljanje** servisa za glavnog računala pristupnika za upravljanje podataka radi na pristupnom računalu. 
    - **Raspored ažuriranja** za instalaciju u određenom trenutku dana. 
    - Prikaz kada je pristupnika **zadnje ažuriranje**.
    - Pritisak na kojoj može instalirati ažuriranje pristupnika. 

8. Prijelaz na karticu **Postavke** . Potvrda navedena u odjeljku **certifikata** za šifriranje/dešifriranje vjerodajnica za lokalni spremišta podataka koje ste odredili na portalu. (neobavezno) Kliknite **Promijeni** namijenjenu vlastiti certifikat. Prema zadanim postavkama pristupnik koristi certifikat koji se automatski se generira servis tvorničke podataka.

    ![Konfiguracija potvrde pristupnika](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    Možete učiniti i sljedeće radnje na kartici **Postavke** : 
    - Prikaz ili izvoz certifikata koristi pristupnik.
    - Promijenite krajnju točku HTTPS pristupnik koristi.    
    - Postavite HTTP proxy će se koristiti pristupnik.   
9. (neobavezno) Prijelaz na karticu **Dijagnostika** , potvrdite mogućnost **Omogući opširno zapisivanje** ako želite omogućiti opširno zapisivanje koje možete koristiti za otklanjanje poteškoća s pristupnika. Zapisivanje informacija pronaći ćete u **Preglednik događaja** u odjeljku **Zapisnici programa i servisa** -> čvor**Pristupnik za upravljanje podacima** . 

    ![Kartica za dijagnostiku](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    Na kartici **Dijagnostika** možete izvršiti i sljedeće radnje: 
    
    - Pomoću sekcija **Testiraj vezu** pomoću pristupnika na lokalni izvor podataka.
    - Kliknite **Prikaz zapisnika** da biste vidjeli zapisnika pristupnik za upravljanje podacima u prozoru preglednik događaja. 
    - Kliknite **Pošalji zapisnike** zip datoteku sa zapisnicima zadnjih sedam dana želite Microsoftu poslati olakšati otklanjanje poteškoća s vašeg problema. 
10. Na kartici **Dijagnostika** , u odjeljku **Testiraj vezu** odaberite **SqlServer** za vrstu spremišta podataka, unesite naziv poslužitelja baze podataka, naziv baze podataka, navedite vrsta provjere autentičnosti, unesite korisničko ime i lozinku pa kliknite **testiranje** da biste testirali li pristupnika možete povezati s bazom podataka. 
11. Prebacivanje na web-preglednik i **Azure portal**, kliknite **u redu** na plohu **Konfiguracija** , a zatim na plohu **novog pristupnika podataka** .
6. Trebali biste vidjeti **adftutorialgateway** u odjeljku **Pristupnika podataka** u prikazu stabla na lijevoj strani.  Ako kliknete, trebali biste vidjeti pridružene JSON. 
    

## <a name="create-linked-services"></a>Stvorite povezani servisi 
U ovom ćete koraku stvoriti dvije povezane usluge: **AzureStorageLinkedService** i **SqlServerLinkedService**. **SqlServerLinkedService** veze baze podataka SQL Server lokalnog i veze povezane servisa **AzureStorageLinkedService** blobova platforme Azure pohraniti na tvorničke podataka. Stvorite na kanal u nastavku ovog vodiča koja se kopira podatke iz baze podataka SQL Server na lokalno spremište blobova platforme Azure. 

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Dodavanje servisa povezanih s bazom podataka sustava SQL Server lokalnog
1.  U **Uređivaču tvorničke podataka**, na alatnoj traci kliknite **Spremanje nove podatke** i odaberite **SQL Server**. 

    ![Novi servis sustava SQL Server povezana](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png) 
3.  U **uređivaču JSON** na desnoj strani, učinite sljedeće: 
    1. **GatewayName**navedite **adftutorialgateway**. 
    2. U **connectionString**, učinite sljedeće: 
        1. Za **naziv poslužitelja**unesite naziv poslužitelja koji hostira baze podataka SQL Server.
        2. Za **ImeBazePodataka**, unesite naziv baze podataka.
        3. Kliknite gumb na alatnoj traci za **šifriranje** . Preuzimanje i pokreće aplikacije upravitelj vjerodajnica.
        
            ![Programa upravitelj vjerodajnica](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
        5. U dijaloškom okviru **Postavljanje vjerodajnica** navedite vrsta provjere autentičnosti, korisničko ime i lozinku, a zatim kliknite **u redu**. Ako veze ne uspije, šifrirane vjerodajnice pohranjene su u na JSON i zatvara dijaloški okvir. 
        6. Zatvorite karticu prazan preglednika koji se pokrene dijaloški okvir ako ne automatski zatvara i vratili se na karticu s portala za Azure. 
  
            Na pristupnom računalu te vjerodajnice su **šifrirane** pomoću certifikata vlasništvu servis tvorničke podataka. Ako želite koristiti certifikat koji je povezan s pristupnik za upravljanje podacima umjesto toga, u odjeljku [Postavljanje vjerodajnica za sigurno](#set-credentials-and-security).    
    1.  Kliknite **uvođenja** trake s naredbama za implementaciju servis sustava SQL Server povezani. Trebali biste vidjeti povezane servisa u prikazu stabla. 
        
        ![Servis SQL Server koji su povezani u prikazu stabla](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)  

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Dodavanje povezane servisa za račun za Azure prostora za pohranu
 
1. U **Uređivaču tvorničke podataka**, na traci izbornika kliknite **Spremanje nove podatke** , a zatim kliknite **Azure prostora za pohranu**.
2. Unesite naziv računa za Azure prostora za pohranu za **naziv računa**.
3. Unesite ključ za račun Azure prostora za pohranu za **ključ za račun**.
4. Zatim **Implementiraj** za implementaciju **AzureStorageLinkedService**.
   
 
## <a name="create-datasets"></a>Stvaranje skupova podataka
U ovom ćete koraku stvoriti unos i izlaz skupove podataka koji predstavljaju ulazni i izlazni podataka za operaciju kopiranja (baze podataka SQL Server lokalnog = > blobova platforme Azure). Prije stvaranja skupova podataka, učinite sljedeće korake (detaljne upute slijedi popis):

- Stvaranje tablice pod nazivom **emp** u bazi podataka SQL Server koji ste dodali kao povezane usluge na tvorničke podataka i umetanje nekoliko Primjeri unosa u tablicu.
- Stvaranje spremnika blob pod nazivom **adftutorial** na računu spremišta blobova platforme Azure dodati kao povezane servis tvorničke podataka.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>Priprema lokalnog sustava SQL Server za vodič

1. U bazi podataka koje ste naveli za SQL Server lokalnog povezane usluge (**SqlServerLinkedService**), koristite sljedeću skriptu SQL da biste stvorili tablicu **emp** u bazi podataka.

        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL, 
            FirstName varchar(50),
            LastName varchar(50),
            CONSTRAINT PK_emp PRIMARY KEY (ID)
        )
        GO 
2. Umetnite primjerima u tablici: 

        INSERT INTO emp VALUES ('John', 'Doe')
        INSERT INTO emp VALUES ('Jane', 'Doe')

### <a name="create-input-dataset"></a>Stvaranje unosa skup podataka

1. **Uređivač tvorničke podataka**, kliknite **... Dodatne**kliknite **novi skup podataka** na naredbenoj traci, a zatim kliknite **Tablica sustava SQL Server**. 
2.  Zamijenite JSON u desnom oknu sljedeći tekst:
        
            {       
                "name": "EmpOnPremSQLTable",
                "properties": {
                    "type": "SqlServerTable",
                    "linkedServiceName": "SqlServerLinkedService",
                    "typeProperties": {
                        "tableName": "emp"
                    },
                    "external": true,
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "policy": {
                        "externalData": {
                            "retryInterval": "00:01:00",
                            "retryTimeout": "00:10:00",
                            "maximumRetry": 3
                        }
                    }
                }
            }    

    Imajte na umu sljedeće točke: 

    - **Vrsta** postavljena na **SqlServerTable**.
    - **tableName** postavljen je na **emp**.
    - **linkedServiceName** postavljen je na **SqlServerLinkedService** (ste stvorili povezane servis ranije u ovom vodiču.).
    - Za unos dataset koji je generirao drugi kanal na tvorničke Azure podataka, morate postaviti **vanjskih** **True**. Označava ulaznih podataka je sastavio vanjskih sa servisom Azure podataka tvorničke. Po želji možete navesti sva pravila vanjskih podataka pomoću **externalData** element u odjeljku **pravila** .    

    [Premještanje podataka iz sustava SQL Server](data-factory-sqlserver-connector.md) potražite dodatne informacije o svojstvima JSON.
2. Kliknite **uvođenja** trake s naredbama za implementaciju skupu podataka.  


### <a name="create-output-dataset"></a>Stvaranje skupa podataka za izlaz

1.  U **Uređivaču tvorničke podataka**kliknite **novi skup podataka** na naredbenoj traci pa kliknite **spremište blobova platforme Azure**.
2.  Zamijenite JSON u desnom oknu sljedeći tekst: 

            {
                "name": "OutputBlobTable",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "AzureStorageLinkedService",
                    "typeProperties": {
                        "folderPath": "adftutorial/outfromonpremdf",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            }
  
    Imajte na umu sljedeće točke: 
    
    - **Vrsta** postavljena na **AzureBlob**.
    - **linkedServiceName** postavljen je na **AzureStorageLinkedService** (ste stvorili povezane servis u korak 2).
    - **folderPath** postavljen je na **adftutorial/outfromonpremdf** gdje outfromonpremdf je mapa u spremniku adftutorial. Ako se još ne postoji, stvorite spremnik **adftutorial** . 
    - **Dostupnost** postavljen je na **zaračunava** (**Učestalost** postavite na **sat** i **interval** postavite na **1**).  Servis podataka tvorničke generira u isječak podataka izlaz svaki sat **emp** tablice u bazi podataka SQL Azure. 

    Ako ne odredite **naziv datoteke** za **izlaznu tablicu**, generirani datoteke u **folderPath** se nazivaju u sljedećem obliku: podataka. <Guid>.txt (na primjer:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    Da biste postavili **folderPath** i **naziv datoteke** dinamički na temelju vremena **SliceStart** , koristite svojstvo partitionedBy. U sljedećem primjeru folderPath koristi godinu, mjesec i dan iz SliceStart (vrijeme početka odsječak obrade), a naziv datoteke koristi sat iz na SliceStart. Na primjer, ako se u isječak Proizvodi za 2014.-10-20T08:00:00, na nazivmape postavljen na wikidatagateway/wikisampledataout/2014. / 10/20 i naziv datoteke je postavljeno na 08.csv. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 
    Potražite u članku [Premještanje podataka iz spremišta blobova Azure](data-factory-azure-blob-connector.md) detalje o JSON svojstva.
2.  Kliknite **uvođenja** trake s naredbama za implementaciju skupu podataka. Provjerite nalaze li se i u skupove podataka u prikazu stabla.  

## <a name="create-pipeline"></a>Stvaranje kanala
U ovom ćete koraku stvoriti **kanal** s jednog **Kopiraj aktivnosti** koja koristi **EmpOnPremSQLTable** kao unos i **OutputBlobTable** kao rezultat.

1.  U uređivaču podataka tvorničke, kliknite **... Dodatne**, a zatim kliknite **Novi kanal**. 
2.  Zamijenite JSON u desnom oknu sljedeći tekst: 
    
            {
                "name": "ADFTutorialPipelineOnPrem",
                "properties": {
                "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
                "activities": [
                {
                    "name": "CopyFromSQLtoBlob",
                    "description": "Copy data from on-prem SQL server to blob",
                    "type": "Copy",
                    "inputs": [
                    {
                        "name": "EmpOnPremSQLTable"
                    }
                    ],
                    "outputs": [
                    {
                        "name": "OutputBlobTable"
                      }
                    ],
                    "typeProperties": {
                      "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from emp"
                      },
                      "sink": {
                        "type": "BlobSink"
                      }
                    },
                    "Policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "NewestFirst",
                      "style": "StartOfInterval",
                      "retry": 0,
                      "timeout": "01:00:00"
                    }
                  }
                ],
                "start": "2016-07-05T00:00:00Z",
                "end": "2016-07-06T00:00:00Z",
                "isPaused": false
              }
            }

    > [AZURE.IMPORTANT]
    > Vrijednost svojstva **pokretanje** zamijenite trenutnu vrijednost dan i **Kraj** s sljedeći dan.

    Imajte na umu sljedeće točke:
 
    - U odjeljku aktivnosti postoji samo aktivnosti čija je **Vrsta** postavljen na **kopiji**.
    - **Unos** aktivnosti je **EmpOnPremSQLTable** i **Izlazni** aktivnosti postavljen na **OutputBlobTable**.
    - U odjeljku **typeProperties** **SqlSource** je navedeno kao **Vrsta izvora** i **BlobSink **je navedeno kao **Vrsta primatelj**.
    - SQL upit `select * from emp` navedena za svojstvo **sqlReaderQuery** **SqlSource**.

     Oba pokrenite i vrijeme završetka mora biti u [obliku ISO](http://en.wikipedia.org/wiki/ISO_8601). Na primjer: 2014.-10-14T16:32:41Z. Vrijeme **završetka** nije obavezno, ali koristimo ovog praktičnog vodiča. 
    
    Ako ne navedete vrijednost za svojstvo **Završi** , izračunava se kao "**Početak + 48 sati**". Da biste pokrenuli beskonačno kanal, navedite **9/9/9999** vrijednosti za svojstvo **Završi** . 
    
    Definirate trajanje u kojem isječke podataka obrađuju utemeljenog na svojstvima **dostupnost** definirana za svaki skup podataka tvorničke Azure podataka.
    
    U ovom primjeru postoje 24 isječke podataka kao svaki isječak podataka zaračunava stvorio.     
2. Zatim **Implementiraj** na naredbenoj traci uvesti skup podataka (tablica je pravokutni skup podataka). Provjerite da kanal se prikazuje u prikazu stabla u odjeljku čvor **kanali** .  
5. Sada kliknite **X** dvaput da biste zatvorili blades da biste se vratili plohu **Tvorničke podataka** za **ADFTutorialOnPremDF**.

**Čestitamo!** Uspješno stvorili na tvorničke Azure podataka, povezani servisi, skupova podataka i na kanal i zakazano kanal.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>Prikaz tvorničke podataka u prikazu dijagrama 
1. **Portal za Azure**kliknite pločicu **dijagrama** na početnoj stranici za tvorničke podataka **ADFTutorialOnPremDF** . :

    ![Dijagram veze](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. Trebali biste vidjeti dijagram slično kao na sljedećoj slici:

    ![Prikaz dijagrama](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Povećavanje, Smanjivanje, Zumiranje na 100%, zumiranje Prilagodi, automatski smjestite kanali i skupova podataka i prikaz informacija o podrijetlu (ističe upstream i do stavke odabrane stavke).  Dvokliknite objekt (ulazni i izlazni skup podataka ili kanal) da biste pogledali svojstva za njega. 

## <a name="monitor-pipeline"></a>Monitor kanal
U ovom ćete koraku praćenje što se događa u na tvorničke Azure podataka pomoću portala za Azure. Praćenje skupova podataka i kanali možete koristiti i cmdleta ljuske PowerShell. Detalje o nadzoru potražite u članku [nadzor i upravljanje kanali](data-factory-monitor-manage-pipelines.md).

5. Dijagram, dvokliknite **EmpOnPremSQLTable**.  

    ![EmpOnPremSQLTable isječaka](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. Imajte na umu da sve podatke presijeca gore su u stanju **spremno** jer kanal (vrijeme početka vremenu završetka) traje u prošlosti. Preporučuje se i jer ste umetnuli podatke u bazi podataka sustava SQL Server i postoji cijelo vrijeme. Provjerite da nema isječaka prikazuju se u odjeljku **isječke Problem** pri dnu. Da biste pogledali sve isječke, kliknite **Više potražite u članku** pri dnu popisa isječke. 
7. Sada u plohu **skupove podataka** kliknite **OutputBlobTable**.

    ![OputputBlobTable isječaka](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
9. Kliknite bilo koji isječkom podataka s popisa, a trebali biste vidjeti plohu **Isječak podataka** . Vidjeti aktivnost traje isječak. Vidjet ćete samo jedan aktivnosti pokretanje obično.  

    ![Plohu isječak podataka](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Ako isječak nije **spreman** stanje, vidjet ćete upstream isječci koji su spremni onemogućuju trenutni isječak izvršavaju na popisu **Upstream isječke koje su nije spreman** .

10. Kliknite **pokretanje aktivnosti** s popisa pri dnu da biste vidjeli **aktivnosti pokrenuti pojedinosti**.

    ![Plohu pokrenuti Detalji o aktivnosti](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

    Želite vidjeti informacije kao što je propusnost, trajanje i pristupnik koristi za prijenos podataka. 
11. Kliknite **X** da biste zatvorili sve blades dok ne 
12. vratiti se kućni plohu za **ADFTutorialOnPremDF**.
14. (neobavezno) Kliknite **kanali**, kliknite **ADFTutorialOnPremDF**i Brazdanje ulaznih tablica (**Consumed**) ili izlazna skupove podataka (**Produced**).
15. Da biste potvrdili da se za svaki sat stvorili blob/datoteka pomoću alata kao što su Alati za korištenje kao što je [Microsoft Explorer prostora za pohranu](http://storageexplorer.com/) .

    ![Explorer Azure prostora za pohranu](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Daljnji koraci

- Članak [Pristupnik za upravljanje podacima](data-factory-data-management-gateway.md) potražite u članku dodatne informacije o pristupnik za upravljanje podacima.
- Potražite u članku [Kopiranje podataka iz blobova platforme Azure na Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) da biste saznali kako koristiti kopiju aktivnosti da biste premjestili sadržaj iz spremišta izvora podataka primatelj izvor podataka. 
