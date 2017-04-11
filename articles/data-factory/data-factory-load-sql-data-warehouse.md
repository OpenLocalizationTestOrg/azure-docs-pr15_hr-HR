<properties 
    pageTitle="Učitavanje terabajta podataka u SQL Data Warehouse | Microsoft Azure" 
    description="Pokazuje kako 1 TB podataka mogu se učitati u Azure SQL Data Warehouse u odjeljku 15 minuta s tvorničke Azure podataka" 
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
    ms.date="10/28/2016" 
    ms.author="jingwang"/>

# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-azure-data-factory-copy-wizard"></a>Učitavanje 1 TB u Azure SQL Data Warehouse u odjeljku 15 minuta s Azure podataka tvorničke [Čarobnjak za kopiranje]
[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) je u oblaku, skala iz baze podataka može obrade pretraživanje velikog količine podataka, relacijski i koje nisu relacijske.  Ugrađena na arhitekturi massively paralelno obrada (MPP), SQL Data Warehouse optimiziran je za radnih opterećenja skladištu podataka za enterprise.  Oblak elasticity nudi s fleksibilnost skaliranja prostora za pohranu i izračunati neovisno.

Uvod u Azure SQL Data Warehouse sada je lakše nego ikada korištenje **Tvorničke Azure podataka**.  Azure tvorničke podataka je servis za integraciju potpuno upravljanih podataka u oblaku, koji se može koristiti za popunjavanje SQL Data Warehouse s podacima iz postojećeg sustava i spremanje koristan kada ocjenjujete SQL Data Warehouse i izrade rješenja sustava analytics nudi.  Slijede ključne pogodnosti učitavanja podataka u skladištu podataka SQL Azure koje pomoću tvorničke Azure podataka:

- **Easy da biste postavili**: 5 korak intuitivno čarobnjaka bez skriptiranje obavezno.
- **Podrška spremišta obogaćenog podataka**: ugrađenu podršku za bogatog skupa lokalnog i služi za pohranu podataka u oblaku.
- **Sigurne i usklađen**: podaci se prenose putem HTTP ili ExpressRoute i prisutnosti globalni servisa osigurava podataka nikad ne ostavlja zemljopisnim granicu
- **Unparalleled performanse pomoću PolyBase** – pomoću Polybase je najučinkovitiji način da biste premjestili podatke u Azure SQL Data Warehouse. Pomoću značajke pripremna blob možete postići visoke učitavanja brzine iz svih vrsta podataka trgovine osim blobova platforme Azure prostor za pohranu na Polybase podržava prema zadanim postavkama.

U ovom se članku objašnjava za korištenje čarobnjaka za kopiranje podataka tvorničke da biste učitali 1 TB podataka iz spremišta blobova platforme Azure u Azure SQL Data Warehouse u odjeljku 15 minuta, preko 1.2 GBps propusnost.

Ovaj članak sadrži detaljne upute za premještanje podataka u skladištu podataka za SQL Azure pomoću čarobnjaka za kopiranje. 

> [AZURE.NOTE] Članak [Premještanje podataka i iz Azure SQL Data Warehouse pomoću tvorničke Azure podataka](data-factory-azure-sql-data-warehouse-connector.md) potražite u članku opće informacije o mogućnostima tvorničke podataka u premještanju podataka do/od Azure SQL Data Warehouse. 
> 
> Možete sastaviti i kanali pomoću Azure portala za Visual Studio komponente PowerShell itd. U odjeljku [Praktični vodič: kopiranje podataka iz blobova platforme Azure s bazom podataka SQL Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) Brzi vodič s detaljnim uputama za korištenje aktivnosti Kopiraj u tvorničke Azure podataka.  

## <a name="prerequisites"></a>Preduvjeti
- Spremište blobova platforme Azure: ovaj eksperiment koristi spremište blobova na Azure (GRS) za pohranu TPC H testiranja skup podataka.  Ako nemate račun za Azure prostora za pohranu, Saznajte [kako stvoriti račun za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account).
- Podaci [TPC H](http://www.tpc.org/tpch/) : ne možemo namjeravate koristiti TPC H kao testiranja skupu podataka.  Da biste to učinili, morate koristiti `dbgen` iz komplet alata za TPC H, koji olakšava generiranje skupu podataka.  Ili možete preuzeti izvornog koda za `dbgen` iz [Alata za TPC](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) i objedinjavanje sami ili preuzeti kompilirane binarni s [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Vođenje dbgen.exe uz sljedeće naredbe za generiranje nehijerarhijskom datotekom 1 TB za `lineitem` tablice Podjela preko 10 datoteke:
    - `Dbgen -s 1000 -S **1** -C 10 -T L -v`
    - `Dbgen -s 1000 -S **2** -C 10 -T L -v`
    - …
    - `Dbgen -s 1000 -S **10** -C 10 -T L -v` 

    Sada kopirajte generirani datoteke blobova platforme Azure.  Pogledajte [Premjesti podatke iz lokalnog datotečnog sustava pomoću tvorničke Azure podataka](data-factory-onprem-file-system-connector.md) za kako to učiniti pomoću ADF Kopiraj.   
- Azure SQL Data Warehouse: ovaj eksperiment učita podatke u Azure SQL Data Warehouse stvorena pomoću 6,000 DWUs

    Pogledajte članak [Stvaranje programa Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision/) za detaljne upute za stvaranje baze podataka SQL Data Warehouse.  Da biste najbolje performanse moguće učitavanja SQL Data Warehouse pomoću Polybase, ne možemo odaberite najveći broj dopuštena za postavku performanse, 6,000 DWUs podataka skladištu jedinica (DWUs).

    > [AZURE.NOTE] 
    > Prilikom učitavanja iz blobova platforme Azure, podaci učitavanja performanse izravno je proporcionalna broj DWUs konfigurirati na SQL Data Warehouse:
    > 
    > Učitavanje 1 TB u 1000 DWU SQL Data Warehouse vodi 87min (~ 200MBps propusnost) učitavanje 1 TB u 2000 DWU SQL Data Warehouse vodi 46min (~ 380MBps propusnost) učitavanje 1 TB u 6000 DWU SQL Data Warehouse vodi 14min (~1.2GBps propusnost) 

    Da biste stvorili SQL Data Warehouse 6,000 DWUs, pomaknite klizač performansi do kraja desno:

    ![Klizač performansi](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    Za postojeću bazu podataka koji je konfiguriran pomoću 6,000 DWUs, promijenite joj pomoću portala za Azure.  Dođite do baze podataka na portalu Azure, a nalazi se gumb **Skaliranje** na ploči **Pregled** prikazano na sljedećoj slici:

    ![Gumb Promjena veličine](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Kliknite gumb **mjerilo** da biste otvorili ploču za sljedeće pomaknite klizač do maksimalne vrijednosti, a zatim kliknite gumb **Spremi** .

    ![Dijaloški okvir mjerilo](media/data-factory-load-sql-data-warehouse/scale-dialog.png)
    
    Ovaj eksperiment učita podatke u skladištu podataka za SQL Azure pomoću `xlargerc` predmete resursa.

    Da biste postigli najbolje moguće propusnost Kopiraj potrebno izvršiti pomoću SQL Data Warehouse korisnik pripadaju `xlargerc` predmete resursa.  Saznajte kako to učiniti sljedeće [promjene klase primjer resursa za korisnika](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example).  

- Stvaranje shema odredišne tablice u bazi podataka za Azure SQL Data Warehouse, pokretanjem sljedeće naredbe DDL:

        CREATE TABLE [dbo].[lineitem]
        (
            [L_ORDERKEY] [bigint] NOT NULL,
            [L_PARTKEY] [bigint] NOT NULL,
            [L_SUPPKEY] [bigint] NOT NULL,
            [L_LINENUMBER] [int] NOT NULL,
            [L_QUANTITY] [decimal](15, 2) NULL,
            [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
            [L_DISCOUNT] [decimal](15, 2) NULL,
            [L_TAX] [decimal](15, 2) NULL,
            [L_RETURNFLAG] [char](1) NULL,
            [L_LINESTATUS] [char](1) NULL,
            [L_SHIPDATE] [date] NULL,
            [L_COMMITDATE] [date] NULL,
            [L_RECEIPTDATE] [date] NULL,
            [L_SHIPINSTRUCT] [char](25) NULL,
            [L_SHIPMODE] [char](10) NULL,
            [L_COMMENT] [varchar](44) NULL
        )
        WITH
        (
            DISTRIBUTION = ROUND_ROBIN,
            CLUSTERED COLUMNSTORE INDEX
        )

U koracima pripremni dovršen, ne možemo spremni za konfiguriranje aktivnosti Kopiraj pomoću čarobnjaka za kopiranje.

## <a name="launch-copy-wizard"></a>Pokrenite čarobnjak za kopiranje

1.  Prijavite se na [portal za Azure](https://portal.azure.com).
2.  Kliknite **+ NOVO** iz gornji lijevi kut, kliknite **Obavještavanje + analize**pa kliknite **Tvorničke podataka**. 
6. U plohu **Novi tvorničke podataka** :
    1. Unesite **LoadIntoSQLDWDataFactory** **naziv**.
        Naziv tvorničke Azure podataka mora biti globalno jedinstveni. Ako vam se prikaže pogreška: **nije dostupna je naziv tvorničke podataka "LoadIntoSQLDWDataFactory"**, promijenite naziv tvorničke podataka (primjerice, yournameLoadIntoSQLDWDataFactory) i pokušajte ponovno stvoriti. Pogledajte temu [Tvorničke podataka – pravila imenovanja](data-factory-naming-rules.md) za imenovanja pravila za artefakte tvorničke podataka.  
     
    2. Odaberite Azure **pretplate**.
    3. Za grupu resursa, učinite nešto od sljedećeg postupka: 
        1. Odaberite **Koristi postojeću** da biste odabrali postojeću grupu resursa.
        2. Odaberite **Stvori novi** da biste unesite naziv za grupu resursa.
    3. Odaberite **mjesto** za tvorničke podataka.
    4. Potvrdite okvir **Prikvači na nadzornoj ploči** pri dnu zaslona u plohu.  
    5. Kliknite **Stvori**.
10. Po dovršetku stvaranje vidite plohu **Tvorničke podataka** kao što je prikazano na sljedećoj slici:

    ![Podaci tvorničke Početna stranica](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
11. Na početnoj stranici tvorničke podataka, kliknite pločicu **Kopiranje podataka** da biste pokrenuli **Čarobnjak za kopiranje**. 

    > [AZURE.NOTE] Ako vam se prikazuje pri "Dopuštanja..." je se zamrzne u web-preglednik, Onemogući/poništite postavka **Blokiranje trećih strana i podataka za web-mjesta** (ili) Zadrži omogućena i stvoriti iznimku **login.microsoftonline.com** i pokušajte ponovno pokretanje čarobnjaka.


## <a name="step-1-configure-data-loading-schedule"></a>Korak 1: Konfiguriranje podataka učitavanja rasporeda
U prvi je korak konfiguriranje podataka učitavanja raspored.  

Na stranici **Svojstva** :
1. Unesite **CopyFromBlobToAzureSqlDataWarehouse** za **naziv zadatka**
2. Odaberite mogućnost **Izvedi sada jedanput** .   
3. Kliknite **Dalje**.  

![Čarobnjak za kopiranje - svojstava stranice](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Korak 2: Konfiguriranje izvora
U ovom je odjeljku objašnjeno korake da biste konfigurirali izvorišnog web-mjesta: blobova platforme Azure koja sadrži redak 1 TB TPC-H stavke datoteke.

Odaberite **Spremište blobova platforme Azure** kao podatke pohranu i kliknite **Dalje**.

![Čarobnjak za kopiranje – Odabir izvora stranice](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

Ispunite podatke o vezi za račun spremište blobova platforme Azure, a zatim kliknite **Dalje**.

![Čarobnjak za kopiranje - izvora podataka za veze](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

Odaberite **mapu** koja sadrži stavku datoteke TPC H, a zatim kliknite **Dalje**.

![Kopiranje čarobnjaka - odaberite ulazne mape](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

Nakon kliknete **Dalje**, postavke oblik datoteke se otkrivaju automatski.  Provjerite da je taj stupac graničnik ' | 'umjesto zarez zadani','.  Nakon što ste pregledati podatke, kliknite **Dalje** .

![Čarobnjak za kopiranje – postavke oblik datoteke](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Korak 3: Konfiguriranje odredište
U ovom se odjeljku objašnjava kako konfigurirati odredište: `lineitem` tablicu u bazi podataka za Azure SQL Data Warehouse.

Odaberite **Azure SQL Data Warehouse** kao Odredišno spremište, a zatim kliknite **Dalje**.

![Čarobnjak za kopiranje - odaberite odredišno spremište podataka](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

Unesite podatke o vezi za Azure SQL Data Warehouse.  Provjerite jesu li korisnik koji je član uloge `xlargerc` (pogledajte odjeljak **preduvjeti** za detaljne upute), a zatim kliknite **Dalje**. 

![Čarobnjak za kopiranje - odredište veze informacije](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

Odaberite odredišnu tablicu, a zatim kliknite **Dalje**.

![Kopiranje čarobnjaka - stranice mapiranja tablica](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

Prihvatite zadane postavke za mapiranje stupca, a zatim kliknite **Dalje**.

![Kopiranje čarobnjaka - sheme mapiranje stranice](media/data-factory-load-sql-data-warehouse/schema-mapping.png)


## <a name="step-4-performance-settings"></a>Korak 4: Postavke performansi

**Dopusti polybase** označeno je prema zadanim postavkama.  Kliknite **Dalje**.

![Kopiranje čarobnjaka - sheme mapiranje stranice](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Korak 5: Implementacija i praćenje opterećenja rezultata
Kliknite gumb **Završi** za implementaciju. 

![Čarobnjak za kopiranje – stranica sa sažetkom](media/data-factory-load-sql-data-warehouse/summary-page.png)

Po dovršetku implementacijskih kliknite `Click here to monitor copy pipeline` praćenje Kopiraj pokrenuti tijek.

Odaberite Kopiraj kanal koji ste stvorili na popisu **Windows aktivnosti** .

![Čarobnjak za kopiranje – stranica sa sažetkom](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

Možete pogledati Kopiraj pokrenuti pojedinosti u programu **Explorer prozor aktivnosti** u desnom oknu, uključujući glasnoće podataka čitanje iz izvora i napisali u odredišnoj, trajanje i average propusnost za izvođenje.

Kao što možete vidjeti iz sljedećih snimku zaslona, kopirate 1 TB iz spremišta blobova platforme Azure u SQL Data Warehouse snimljene 14 minuta učinkovito postizanje 1.22 GBps propusnost!

![Čarobnjak za kopiranje - je uspjela dijaloški okvir](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Najbolje prakse
Slijedi popis nekoliko najboljih postupaka za pokretanje Azure SQL Data Warehouse baze podataka:

- Veće klasu resursa koristiti prilikom učitavanja u INDEKS COLUMNSTORE za GRUPIRANI.
- Učinkovitiji spojevi, razmislite o razdiobom raspršivanje po odaberite stupac umjesto zadanog okrugle robin raspodjele.
- Za brže učitavanje brzine, preporučujemo da koristite skupova za tranzitne podatke.
- Kada završite s učitavanja Azure SQL Data Warehouse, stvorite Statistika.

Detalje potražite u članku [najbolje prakse za Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) . 

## <a name="next-steps"></a>Daljnji koraci
- [Čarobnjak za kopiranje podataka tvorničke](data-factory-copy-wizard.md) – ovaj članak sadrži detalje o čarobnjaku za kopiranje. 
- [Kopiraj aktivnosti performanse i ugađanje vodič](data-factory-copy-activity-performance.md) – u ovom se članku sadrži referencu performanse mjere i ugađanje vodič.

