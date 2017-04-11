<properties
    pageTitle="Kopiranje aktivnosti performanse i vodič za ugađanje | Microsoft Azure"
    description="Saznajte više o ključa čimbenici koji utječu na performanse premještanje podataka na tvorničke Azure podataka kada koristite aktivnost Kopiraj."
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
    ms.date="10/25/2016"
    ms.author="jingwang"/>


# <a name="copy-activity-performance-and-tuning-guide"></a>Kopiranje aktivnosti performanse i ugađanje vodiča
Azure podataka tvorničke Kopiraj aktivnosti nudi jednostavno prva liga sigurne pouzdan te visokih performansi podataka učitavanja rješenja. Ga omogućuje kopiju tens terabajta podataka svakodnevno na brojnim obogaćenog oblaka i lokalnih podataka trgovine. Blazing brzo učitavanje performanse podaci ključ da biste bili sigurni usredotočite se na problem "velikih skupova podataka" core: izrade rješenja naprednom analitikom i ne uspijevate precizno uvida iz sve podatke.

Azure pruža skup poslovne klase podataka i prostor za pohranu podataka skladištu rješenja i Kopiraj aktivnosti nudi iznimno optimizirana podataka učitavanja sučelje, koje je jednostavno konfigurirati i postavljanje. S samo jednu kopiju aktivnosti, možete postići:

- Učitavanje podataka u **Azure SQL Data Warehouse** na **1.2 GBps**
- Učitavanje podataka u **spremište blobova platforme Azure** pri **1.0 GBps**
- Učitavanje podataka u **Spremištu Lake podataka za Azure** pri **1.0 GBps**


U ovom se članku opisuje:

- [Performanse Referentni brojevi](#performance-reference) za podržanih izvora i primatelj podataka pohranjuje da biste lakše isplanirali projekta;
- Značajke koje se povećati propusnost Kopiraj u različitim scenarijima, uključujući [paralelno Kopiraj](#parallel-copy), [jedinica za premještanje podataka oblaka](#cloud-data-movement-units)i [kopirana bez postavljanja Kopiraj](#staged-copy);
- [Upute za ugađanje performansi](#performance-tuning-steps) za precizno podesite performanse i ključne čimbenici koji mogu utjecati na performanse Kopiraj.

> [AZURE.NOTE] Ako niste upoznati s Kopiraj aktivnosti Općenito, potražite u članku [Premještanje podataka pomoću Kopiraj aktivnosti](data-factory-data-movement-activities.md) prije pročitajte ovaj članak.

## <a name="performance-reference"></a>Referenca performansi

![Performanse matrice](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

> [AZURE.NOTE] Veću propusnost možete postići tako da korištenje više podataka premještanja jedinice (DMUs) umjesto zadanog Maksimalna DMUs koja je 8 za aktivnost kopiju oblaka oblaka pokrenuti. Ako, na primjer, s 100 DMUs možete kopirati podatke iz blobova platforme Azure Azure podataka Lake spremište brzinom od 1 gigabajta sekundi. Detalje o toj značajci potražite u odjeljku [jedinica za premještanje podataka oblaka](#cloud-data-movement-units) . Zatražite da biste zatražili dodatne DMUs [podržava Azure](https://azure.microsoft.com/support/) .

Upućuje na Imajte na umu:

- Propusnost izračunava se pomoću sljedeće formule: [veličina podataka čitanje iz izvora] / [Kopiraj aktivnosti pokrenuti trajanje].
- Performanse Referentni brojevi u tablici su mjeriti [TPC H](http://www.tpc.org/tpch/) zadani skup podataka u jednu kopiju aktivnosti pokrenuti.
- Da biste kopirali između služi za pohranu podataka oblaka, postavite **cloudDataMovementUnits** 1 i 4 (ili 8) za usporedbu. **parallelCopies** nije naveden. Pojedinosti o tim značajkama potražite u odjeljku [paralelno Kopiraj](#parallel-copy) .
- U trgovine Azure podataka izvor i primatelj su se na istom području Azure.
- Za hibridno (lokalni oblaka ili oblaka za lokalne) premještanje podataka instancu pristupnika nije pokrenut na računalo koje je osim onih iz lokalnog spremišta podataka. Konfiguracija nalazi se u sljedeću tablicu. Kada je pristupnika radi jedne aktivnosti, kopiranja potrošena samo mali dio na računalu test procesora, memorije ili propusnost mreže.
    <table>
    <tr>
        <td>PROCESOR</td>
        <td>32 jezgri 2.20 v2 GHz Intel Xeon E5-2660</td>
    </tr>
    <tr>
        <td>Memorije</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Mreža</td>
        <td>Internet sučelja: 10 Gbps; intranet sučelja: 40 Gbps</td>
    </tr>
    </table>

## <a name="parallel-copy"></a>Paralelni Kopiraj
Možete pročitati podatke iz izvorišnog web-mjesta ili zapisivanje podataka na odredište **paralelno unutar Kopiraj aktivnosti pokrenuti**. Ova značajka poboljšava propusnost postupak kopiranja i smanjuje vrijeme potrebno da biste premjestili sadržaj.

Ta postavka razlikuje se od svojstvo **istodobnosti** u definiciji aktivnosti. Svojstvo **istodobnosti** određuje broj **pokreće Istodobni Kopiraj aktivnosti** obrade podataka iz različitih aktivnosti sustava windows (1 AM za 2 AM, 2 AM 3 Prijepodne, 3 Prijepodne do 4 AM i tako dalje). Ova mogućnost je korisno prilikom izvršavanja povijesne Učitaj. Paralelni Kopiraj mogućnost odnosi se na **jednu aktivnost pokrenuti**.

Pogledajmo uzorak scenarija. U sljedećem primjeru potrebno obraditi više isječaka s u prošlosti. Tvorničke podataka pokreće instance komponente Kopiraj aktivnosti (aktivnosti cilja) za svaki isječak:

- Odsječak podataka u prozoru prvi aktivnosti (1 AM za AM 2) == > aktivnosti pokrenuti 1
- Odsječak podataka iz druge aktivnosti prozora (2 AM za AM 3) == > aktivnosti pokrenuti 2
- Odsječak podataka iz druge aktivnosti prozora (3 AM za AM 4) == > aktivnosti pokrenuti 3

I tako dalje.

U ovom primjeru kada **istodobnosti** vrijednosti je postavljeno na 2, **aktivnosti pokrenuti 1** i **2 za pokretanje aktivnosti** kopirajte podatke iz dva aktivnosti windows **istovremeno** radi poboljšanja performansi za premještanje podataka. Međutim, ako više datoteka su povezane s aktivnosti pokretanje 1, servisa za premještanje podataka kopira datoteke s izvorišnog web-mjesta na je odredišna datoteka istovremeno.

### <a name="parallelcopies"></a>parallelCopies
Svojstvo **parallelCopies** možete koristiti da biste naznačili parallelism koje želite kopiju aktivnosti da biste koristili. Ovo svojstvo možete smatrati maksimalni broj niti unutar Kopiraj aktivnosti koje se mogu čitati iz izvora ili primatelj trgovine za podatke u paralelno za pisanje.

Aktivnosti kopija svake pokrenuti, tvorničke podataka određuje broj kopija paralelno koristiti da biste kopirali podatke iz izvora podataka pohranu i odredište podacima pohraniti. Zadani broj paralelno kopije koja se koristi ovisi o vrsti izvora i primatelj koji trenutno koristite.  

Izvor i primatelj |   Zadani paralelno Kopiraj count određen servisa
------------- | -------------------------------------------------
Kopiranje podataka između utemeljenih na datotekama trgovine (blobova; Pohrana podataka Lake; Amazon S3; lokalni datotečni sustav; lokalnim sustavom HDFS) | Između 1 i 32. Ovisi o veličini datoteka i broj jedinica oblaka premještanje podataka (DMUs) koristi za kopiranje podataka između dva oblaka podataka služi za pohranu ili fizičke konfiguraciji pristupnom računalu koristiti radi hibridnog Kopiraj (za kopiranje podataka ili iz trgovine lokalnih podataka).
Kopirajte podatke iz **izvora podataka pohraniti sa spremištem tablica Azure** | 4
Svi drugi izvor i primatelj parovi | 1

Obično zadano ponašanje mora vam dati preporučeni propusnost. Međutim, da biste odredili opterećenje na računalima na kojima su hostirani podataka pohranjuje ili podesite performanse Kopiraj, možete promijeniti zadanu vrijednost i odrediti vrijednost za svojstvo **parallelCopies** . Vrijednost mora biti između 1 i 32 (obje uključivo). Prilikom izvođenja radi ostvarivanja najboljih performansi Kopiraj aktivnost koristi vrijednost koja je manja od ili jednaka vrijednosti koje ste postavili.

    "activities":[  
        {
            "name": "Sample copy activity",
            "description": "",
            "type": "Copy",
            "inputs": [{ "name": "InputDataset" }],
            "outputs": [{ "name": "OutputDataset" }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                },
                "parallelCopies": 8
            }
        }
    ]

Upućuje na Imajte na umu:

- Kopiranje podataka između utemeljenih na datotekama trgovine, parallelism se ne događa kada na razini datoteke. Postoji bez chunking u jednu datoteku. Stvarni broj paralelno kopije servisa za premještanje podataka koristi se za kopiranje vrijeme izvođenja nije veći od broja datoteka koje imate. Ako je ponašanje Kopiraj **mergeFile**, Kopiraj aktivnosti ne možete iskoristiti parallelism.
- Kada odredite vrijednosti za svojstvo **parallelCopies** , razmislite o povećava Učitaj na izvor i primatelj spremišta podataka i pristupnika ako je hibridno kopiju. To se događa osobito kada imate više aktivnosti ili Istodobni pokreće iste aktivnosti koje su pokrenute na istom spremištu podataka. Ako primijetite spremišta podataka i pristupnika overwhelmed s opterećenja, smanjite vrijednost **parallelCopies** relieve opterećenja.
- Kada kopirate podatke iz trgovine koji nisu utemeljenih na datotekama u spremišta koje su utemeljene na datoteku, servis za premještanje podataka zanemaruje svojstvo **parallelCopies** . Čak i ako je naveden parallelism, ona se ne primjenjuje u tom slučaju.

> [AZURE.NOTE] Da biste koristili značajku **parallelCopies** kada to učinite kopiju hibridnog morate koristiti pristupnik za upravljanje podacima 1.11 ili novija verzija.

### <a name="cloud-data-movement-units"></a>Jedinice za premještanje podataka oblaka
**Oblak podataka premještanja jedinica (DMU)** je mjera koja predstavlja power (kombinacija procesora i memorije Dodjela resursa za mrežni) jedna jedinica u tvorničke podataka. Na DMU možda navikli u oblak u oblak kopiranja, ali ne u hibridnog kopiju.

Prema zadanim postavkama tvorničke podataka koristi jedan oblaka DMU za izvođenje jednu kopiju aktivnost pokrenuti. Da biste nadjačali to je zadana vrijednost, odrediti vrijednost za svojstvo **cloudDataMovementUnits** na sljedeći način. Informacije o razini performanse mogla bi vam se prilikom konfiguriranja više jedinica za određenu kopiju izvornog i primatelj potražite u članku [Referenca performansi](#performance-reference).

    "activities":[  
        {
            "name": "Sample copy activity",
            "description": "",
            "type": "Copy",
            "inputs": [{ "name": "InputDataset" }],
            "outputs": [{ "name": "OutputDataset" }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                },
                "cloudDataMovementUnits": 4
            }
        }
    ]

U **dopuštene vrijednosti** za svojstvo **cloudDataMovementUnits** su 1 (zadano), 2, 4 i 8. **Stvarni broj oblaka DMUs** kopiranja koristi prilikom izvođenja je jednaka nuli ili manja od vrijednosti konfiguriran, ovisno o vašem uzorak podataka. 

> [AZURE.NOTE] Ako trebate više oblaka DMUs veću propusnost, obratite se [Azure podržava](https://azure.microsoft.com/support/). Postavljanje broja 8 i iznad trenutno funkcionira samo kada kopirate više datoteka iz spremišta blobova spremište blobova platforme spremišta Lake podataka ili baze podataka SQL Azure, a veličina datoteke je veće od ili jednako 16 MB pojedinačno.

Da biste bolje pomoću tih dvaju svojstava i za poboljšanje vaše propusnost premještanje podataka potražite u članku [uzorka pomoću slučajeva](#case-study-use-parallel-copy). Ne morate konfigurirati **parallelCopies** da iskoristite prednost zadano ponašanje. Ako morate konfigurirati i **parallelCopies** je premalen, više oblaka DMUs možda neće se u potpunosti iskoristiti.  

Je **Važno** Imajte na umu da vam se naplatiti na temelju Ukupno vrijeme kopiranja. Ako Kopiranje posla koristi se za jedan sat ponijeti jedinica jedan oblaka, a sada traje 15 minuta s četiri jedinice oblaka, cjelokupan računa ostaje gotovo. Ako, na primjer, koristite četiri jedinice oblaka. Prvu jedinicu oblaka provodi 10 minuta, a zatim na drugu, 10 minuta, treći jedan pet minuta i četvrti jedan pet minuta sve u jednu kopiju aktivnosti pokreću. Vam se naplatiti za vrijeme ukupni Kopiraj (kretanje podataka), 10 + 10 + 5 + 5 = 30 minuta. Korištenje **parallelCopies** utjecati na naplatu.

## <a name="staged-copy"></a>Postupne Kopiraj
Kada kopirate podatke iz spremišta izvora podataka primatelj izvor podataka, možete da biste upotrijebili blobova privremeni pripremna trgovine. Pripremna osobito je korisna u sljedećim slučajevima:

1.  **Želite li ingest podatke iz različitih sprema podatke u SQL Data Warehouse putem PolyBase**. SQL Data Warehouse koristi PolyBase kao visokom propusnošću mehanizam da biste učitali veliku količinu podataka u SQL Data Warehouse. Međutim, izvorišnih podataka mora biti u spremište blobova platforme i on mora zadovoljiti dodatne kriterije. Prilikom učitavanja podataka iz spremišta podataka koji nisu blobova, možete aktivirati podataka kopiranje putem privremene pripremna blobova. U tom slučaju podataka tvorničke izvodi transformacije potrebnih podataka da biste bili sigurni da zadovoljava preduvjete PolyBase. Zatim koristi PolyBase da biste učitali podatke u SQL Data Warehouse. Dodatne informacije potražite u članku [Korištenje PolyBase da biste učitali podatke Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
2.  **Ponekad potrebno neko vrijeme da biste izvršili hibridnog premještanje podataka (to jest, da biste kopirali između lokalne podatke trgovine i oblaka podataka pohraniti) putem sporu mrežnu vezu**. Da biste poboljšali performanse, možete sažeti na podatke na lokalni tako da je potrebno manje vremena da biste premjestili sadržaj pripremna spremišta podataka u oblaku. Zatim Dekomprimiranje podataka u trgovini pripremna prije učitali u Odredišno spremište podataka.
3.  **Ne želite da biste otvorili priključke osim priključak 80 i 443 u vatrozidu, zbog korporativnih pravila IT**. Na primjer, kada kopirate podatke iz lokalnih podataka trgovine programa baze podataka SQL Azure primatelj ili je primatelj Azure SQL Data Warehouse, morate aktivirati izlaznog TCP komunikacije na priključak 1433 za vatrozid za Windows i vatrozida za tvrtke. U ovom scenariju iskoristite prednost pristupnik prvi kopiranih podataka Blob pripremna instanci za pohranu putem HTTP ili HTTPS na priključak 443. Zatim učitati podatke u SQL baze podataka ili SQL Data Warehouse iz pripremna spremište blobova platforme. U ovom tijek ne morate omogućiti priključak 1433.

### <a name="how-staged-copy-works"></a>Kako postupnu radi kopiranja
Kada aktivirate značajku pripremna, najprije podatke se kopiraju iz spremišta izvora podataka u pripremna spremišta podataka (Premjesti vlastite). Nakon toga se podaci kopiraju iz spremišta podataka pripremna primatelj spremište podataka. Tvorničke podataka automatski upravlja dvije paralelne faze tijeka umjesto vas. Nakon premještanja podataka tvorničke podataka i čisti privremene podatke iz pripremna prostora za pohranu.

Pristupnik ne koristi u oblak Kopiraj scenariju (izvor i primatelj podaci su služi za pohranu u oblaku). Servis podataka tvorničke izvodi postupke za kopiranje.

![Kopirana bez postavljanja Kopiraj: oblaku](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

U kopiji scenarija hibridnog (je lokalni izvor i primatelj je u oblaku), pristupnika premješta podatke iz izvora podataka trgovine pripremna spremišta podataka. Podatkovnog servisa tvorničke premješta podatke iz spremišta podataka pripremna spremišta podataka primatelj. Kopiranje podataka iz spremišta podataka oblaka programa lokalnih podataka iz trgovine putem pripremna je podržano i obrnuti tokove.

![Kopirana bez postavljanja Kopiraj: scenarija hibridnog](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Kada aktivirate premještanje podataka pomoću pripremna trgovine, možete odrediti želite li da se podaci sažeti prije prelaska podataka iz spremišta izvora podataka u spremištu privremeni ili pripremna podataka, a zatim dekomprimira prije premještanju podataka iz programa privremeni ili pripremna spremišta podataka primatelj spremište podataka.

Trenutno ne možete kopirati podataka između dva trgovine lokalnih podataka pomoću pripremna trgovine. Očekivanog tu mogućnost da biste uskoro dostupno.

### <a name="configuration"></a>Konfiguracija
Konfiguriranje postavke **enableStaging** u kopiju aktivnosti odredite želite li da se podaci za biti kopirana bez postavljanja u spremište blobova platforme prije učitali u Odredišno spremište podataka. Kada postavite **enableStaging** TRUE, navedite dodatna svojstva naveden u sljedeću tablicu. Ako ga nemate, morate stvoriti programa Azure prostora za pohranu ili prostora za pohranu zajednički pristup potpis povezana služba za pripremna.

Svojstvo | Opis | Zadana vrijednost | Obavezno
--------- | ----------- | ------------ | --------
**enableStaging** | Odredite želite li da biste kopirali podatke putem programa privremeni pripremna trgovine. | FALSE | ne
**linkedServiceName** | Navedite naziv [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) ili [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) povezane servisa koja se odnosi na instancu prostora za pohranu na koje možete koristiti kao privremeni pripremna trgovine. <br/><br/> Da biste učitali podatke u SQL Data Warehouse putem PolyBase nećete moći koristiti za pohranu potpisom zajednički pristup. Možete ga koristiti u svim drugim situacijama. | N/D | Da, kada **enableStaging** postavljen na TRUE.
**put** | Navedite put Blob prostora za pohranu koji želite sadrže postupnu podatke. Ako ne navedete put, servis stvara spremnik za pohranu privremene podataka. <br/><br/> Navedite put samo ako koristite za pohranu potpisom zajednički pristup ili zahtijevaju privremene podataka na određenom mjestu. | N/D | ne
**enableCompression** | Određuje hoće li se podaci moraju sažeti prije nego što je kopirana na odredište. Ta postavka smanjuje količinu podataka koji se ne prenose. | FALSE | ne

Evo definiciju uzorka Kopiraj aktivnosti sa svojstvima koji su opisani u prethodnoj tablici:

    "activities":[  
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [{ "name": "OnpremisesSQLServerInput" }],
        "outputs": [{ "name": "AzureSQLDBOutput" }],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": "MyStagingBlob",
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
    ]


### <a name="billing-impact"></a>Utjecaj naplata
Vam se naplatiti ovisno o dva koraka: kopiranje trajanje i kopirati vrstu. 

- Kada koristite pripremna tijekom kopiju oblaka (kopiranje podataka iz spremišta podataka u oblaku na drugom spremište podataka u oblaku) se naplaćuju [zbroj Kopiraj trajanje korak 1 i 2] x [oblaka Kopiraj Jedinična cijena].
- Kada koristite pripremna tijekom kopiju hibridnog (kopiranje podataka iz trgovine lokalnih podataka na izvor podataka oblaka) vam se naplatiti trajanja [hibridnog Kopiraj] x [hibridnog Kopiraj Jedinična cijena] + [cloud Kopiraj trajanje] x [oblaka Kopiraj Jedinična cijena].


## <a name="performance-tuning-steps"></a>Koraci za ugađanje performansi
Predlažemo da se snimljene ove korake da biste precizno podesite performanse servisa tvorničke podataka s Kopiraj aktivnosti:

1.  **Uspostavljanje referentne vrijednosti**. Tijekom faze razvoja testirajte svoje kanal pomoću aktivnosti Kopiraj na temelju uzorka predstavniku podataka. Tvorničke podataka [istjecanju model](data-factory-scheduling-and-execution.md#time-series-datasets-and-data-slices) možete koristiti da biste ograničili količinu podataka kojima surađujete.

    Prikupljanje vrijeme izvođenja i karakteristike performansi pomoću **nadzor i upravljanje aplikacija**. Na početnoj stranici tvorničke podataka odaberite **nadzor i upravljanje** . U prikazu stabla odaberite **Izlazni skup podataka**. Na popisu **Aktivnosti Windows** odaberite Kopiraj aktivnosti pokrenuti. **Windows aktivnost** navodi trajanje Kopiraj aktivnosti i veličina podataka koja se kopira. Propusnost nalazi se u **Programu Explorer prozora aktivnosti**. Dodatne informacije o aplikaciji potražite u članku [nadzor i upravljanje kanali tvorničke Azure podataka pomoću nadzor i upravljanje aplikacija](data-factory-monitor-manage-app.md).

    ![Aktivnosti pokrenuti pojedinosti](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

    U nastavku članka, možete usporediti performanse i konfiguraciji scenariju Kopiraj aktivnosti [performanse referencu](#performance-reference) iz naših testira.
2. **Dijagnoza i optimiziranja performansi**. Ako se performanse ne zadovoljava vaše očekivanja, morate prepoznavanje grla performanse. Nakon toga optimiziranja performansi da biste uklonili ili smanjili efekt grla. Puni opis Dijagnostika performansi izlazi iz ovog članka, ali Evo nekih uobičajenih pitanja vezana uz:
    - Performanse značajke:
        - [Paralelni Kopiraj](#parallel-copy)
        - [Jedinice za premještanje podataka oblaka](#cloud-data-movement-units)
        - [Postupne Kopiraj](#staged-copy)   
    - [Izvor](#considerations-for-the-source)
    - [Primatelj](#considerations-for-the-sink)
    - [Serijalizacije i deserijalizacija](#considerations-for-serialization-and-deserialization)
    - [Sažimanje](#considerations-for-compression)
    - [Mapiranje stupca](#considerations-for-column-mapping)
    - [Pristupnik za upravljanje podacima](#considerations-for-data-management-gateway)
    - [Druge napomene](#other-considerations)

3. **Proširivanje konfiguracija da cijeli skup podataka**. Kada budete zadovoljni performanse i izvršavanja rezultata, možete proširiti definicija i kanal aktivni razdoblje da prekrije cijeli skup podataka.

## <a name="considerations-for-the-source"></a>Zahtjevi za izvorišnog web-mjesta
### <a name="general"></a>Općenito
Ne zaboravite da podlozi spremišta podataka nije Zapljusnuti drugih radnih opterećenja koji su pokrenuti na ili ga. 

Microsoft podatke trgovine, potražite u članku [za nadzor i podešavanje tema](#performance-reference) koje su specifične za služi za pohranu podataka te pomoć za razumijevanje podataka pohrana karakteristike performansi, minimiziranje vrijeme odaziva i Maksimiziraj propusnost.

Ako kopirate podatke iz spremišta blobova SQL Data Warehouse, preporučujemo da koristite **PolyBase** da biste uvećali performansi. Detalje potražite u članku [Korištenje PolyBase da biste učitali podatke Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse) .


### <a name="file-based-data-stores"></a>Služi za pohranu datoteka na temelju podataka
*(Uključuje Blob prostora za pohranu, spremišta podataka Lake, Amazon S3, lokalnog datotečnih i lokalne HDFS)*

- **Prosječna veličina datoteke i broj datoteka**: Kopiraj aktivnosti prenosi podatkovnu datoteku za jedan po jedan. Cjelokupan propusnost je jednakih razmaka podataka će se premjestiti niže ako podatke sastoji se od mnogo datoteka small umjesto nekoliko velikih datoteka zbog Samopokretanje faza za svaku datoteku. Dakle, ako je to moguće kombinirati male datoteke u veće datoteke da bi se dobio veću propusnost.
- **Oblik datoteke i sažimanje**: više načina da biste poboljšali performanse u potražite u odjeljcima [pitanja za serijalizacije i deserijalizacija](#considerations-for-serialization-and-deserialization) i [Zahtjevi za spajanje](#considerations-for-compression) .
- **Lokalni datotečni sustav** scenarij, u kojoj je potrebno, **Pristupnik za upravljanje podacima** u odjeljku [Zahtjevi za pristupnik za upravljanje podacima](#considerations-for-data-management-gateway) .

### <a name="relational-data-stores"></a>Služi za pohranu relacijskih podataka
*(Uključujući baze podataka SQL; SQL Data Warehouse; Amazon Redshift; Baza podataka sustava SQL Server; i Oracle, baza podataka MySQL, DB2, Teradata, Sybase i PostgreSQL, itd.)*

- **Uzorak podataka**: sheme tablice utječe na propusnost Kopiraj. Veliki retka veličina daje bolje performanse od veličine malih retka, da biste kopirali istu količinu podataka. Razlog je dohvaćate učinkovitije bazu podataka manje serijama od podataka koje sadrže manje redaka.
- **Upit ili pohranjena procedura**: Optimizacija logičku vrijednost upita ili pohranjena procedura navedete u izvoru Kopiraj aktivnosti učinkovitije za dohvaćanje podataka.
- **Lokalni relacijske baze podataka**, kao što je SQL Server i Oracle, koje zahtijevaju korištenje **Pristupnik za upravljanje podacima**, u odjeljku [Zahtjevi za pristupnik za upravljanje podacima](#considerations-on-data-management-gateway) .

## <a name="considerations-for-the-sink"></a>Zahtjevi za primatelj

### <a name="general"></a>Općenito
Ne zaboravite da podlozi spremišta podataka nije Zapljusnuti drugih radnih opterećenja koji su pokrenuti na ili ga. 

Microsoft podatke trgovine, priručniku [za nadzor i podešavanje tema](#performance-reference) koje su specifične za služi za pohranu podataka. Ove teme mogu olakšati razumijevanje karakteristike performansi pohrane podataka te o tome kako smanjiti vrijeme odaziva i Maksimiziraj propusnost.

Ako kopirate podatke iz **spremišta blobova** **SQL Data Warehouse**, preporučujemo da koristite **PolyBase** da biste uvećali performansi. Detalje potražite u članku [Korištenje PolyBase da biste učitali podatke Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse) .


### <a name="file-based-data-stores"></a>Služi za pohranu datoteka na temelju podataka
*(Uključuje Blob prostora za pohranu, spremišta podataka Lake, Amazon S3, lokalnog datotečnih i lokalne HDFS)*

- **Kopiraj ponašanje**: Ako kopirate podatke iz različitih datoteku na temelju podataka trgovine, Kopiraj aktivnosti ima tri mogućnosti putem svojstvo **copyBehavior** . Zadržava hijerarhije, poravnava hijerarhije ili spajanja datoteka. Očuvanje ili stapanja hijerarhije ima malo ili nimalo indirektni performanse, ali Spajanje datoteka uzrokuje indirektni performanse da biste povećali.
- **Oblik datoteke i sažimanje**: potražite u odjeljcima [pitanja za serijalizacije i deserijalizacija](#considerations-for-serialization-and-deserialization) i [Zahtjevi za sažimanje](#considerations-for-compression) radi dodatnih načina da biste poboljšali performanse.
- **Spremište blobova platforme**: trenutno podržava spremište blobova platforme samo blokirati blob-ova za prijenos optimizirana podataka i propusnost.
- **Lokalni datotečnih sustava** scenariji koje je potrebno koristiti **Pristupnik za upravljanje podacima**, u odjeljku [Zahtjevi za pristupnik za upravljanje podacima](#considerations-for-data-management-gateway) .

### <a name="relational-data-stores"></a>Služi za pohranu relacijskih podataka
*(Obuhvaća SQL baze podataka SQL Data Warehouse, baza podataka sustava SQL Server i baze podataka tvrtke Oracle)*

- **Kopiranje ponašanje**: ovisno o svojstva koje ste postavili za **sqlSink**, Kopiraj aktivnosti piše odredišnu bazu podataka na različite načine.
    - Po zadanom koristi servisa premještanje podataka API Kopiraj masovno da biste umetnuli podatke u dodavanje način koji omogućuje najbolje performanse.
    - Ako pohranjena procedura konfigurirati u primatelj, bazu podataka odnosi jedan redak podataka po umjesto skupno Učitaj. Performanse znatno izostavlja. Ako je vaš zadani skup podataka velik, kada je potrebno, razmislite o prijelaz na korištenje svojstvo **sqlWriterCleanupScript** .
    - Ako konfigurirate svojstva **sqlWriterCleanupScript** aktivnosti kopija svake pokrenuti, servis pokreće skriptu, a zatim Kopiraj API skupno koristite da biste umetnuli podatke. Na primjer, da biste prebrisali cijelu tablicu s najnovijim podacima, možete odrediti skriptu da biste prvi put izbrišete sve zapise prije skupno učitavanje nove podatke iz izvora.
- **Veličina uzorka i obradu podataka**:
    - Tablica sheme utječe na propusnost Kopiraj. Da biste kopirali istu količinu podataka, velike retka veličina daje bolje performanse od veličine malih retka jer bazu podataka možete učinkovitije provođenja manje serijama od podataka.
    - Kopiraj aktivnosti Umeće podatke u nizu serije. Broj redaka u grupu možete postaviti pomoću svojstva **writeBatchSize** . Ako podaci sadrže small redaka, možete postaviti svojstvo **writeBatchSize** dodjeljuje veću vrijednost prednosti za manje grupe dodataka i veću propusnost. Ako je veličine retka podataka velik, pripazite da kada povećate **writeBatchSize**. Visoke vrijednosti mogu dovesti do pogreške Kopiraj uzrokovanih preopterećenje bazu podataka.
- **Lokalni relacijske baze podataka** kao što je SQL Server i Oracle, koje zahtijevaju korištenje **Pristupnik za upravljanje podacima**, u odjeljku [Zahtjevi za pristupnik za upravljanje podacima](#considerations-for-data-management-gateway) .


### <a name="nosql-stores"></a>Služi za pohranu NoSQL
*(Uključuje spremište tablica i Azure DocumentDB)*

- Za **spremište tablica**:
    - **Particija**: zapisivanje podataka interleaved particije znatno smanjuje performanse. Sortirati izvorišnih podataka particija ključ tako da se podaci je umetnut učinkovito particija drugim ili prilagodili logike za zapisivanje podataka jedna particija.
- Za **DocumentDB**:
    - **Veličina grupe**: svojstvo **writeBatchSize** postavlja broj paralelno zahtjeva za uslugu DocumentDB za stvaranje dokumenata. Možete očekivati od boljih performansi povećavate **writeBatchSize** jer više paralelnih zahtjeva za koju se šalju DocumentDB. Međutim, pogledajte za ograničavanje prilikom pisanja DocumentDB (poruka o pogrešci je "Zahtjev je velika brzina"). Različitim čimbenicima mogu prouzročiti ograničavanje, uključujući veličinu dokumenta, broj uvjeta u dokumente i odredišne zbirke indeksiranja pravila. Da biste postigli veću propusnost Kopiraj, razmislite o korištenju bolje zbirke, na primjer, S3.

## <a name="considerations-for-serialization-and-deserialization"></a>Zahtjevi za serijalizacije i deserijalizacija
Serijalizacije i deserijalizacija se može dogoditi kada unos skup podataka ili skup podataka za izlaz datoteke. Trenutno podržava aktivnosti Kopiraj tekst (na primjer, CSV i TSV) i Avro oblika podataka.

**Kopiranje ponašanje**:

-   Kopiranje datoteke između služi za pohranu datoteka na temelju podataka:
    - Kada se ulazni i izlazni skupova podataka i imaju isti ili bez postavke oblikovanja datoteke, servis za premještanje podataka izvršava binarni kopiju bez serijalizacije ili deserijalizacija. Vidjet ćete veću propusnost u usporedbi s scenarij u kojem se razlikuju od međusobno izvor i primatelj postavke oblik datoteke.
    - Prilikom unosa i izlaz skupova podataka i su u obliku teksta i samo kodiranje je različitih vrsta podatkovnog servisa za premještanje samo ne kodiranja pretvorbe. Ne li sve serijalizacije i deserijalizacija, čime će se neke performanse indirektni uspoređuje binarne kopiji.
    - Kada ste ulazni i izlazni skupova podataka i drugim oblicima datoteka ili različitih konfiguracija, kao što su graničnike, servis za premještanje podataka deserializes izvorišnih podataka za strujanje, pretvaranje i zatim serijalizirati u izlazni oblik koji je označen. Ovaj postupak rezultira znatno značajan performanse indirektni u usporedbi s drugim situacijama.
- Prilikom kopiranja datoteke iz spremišta podataka koji nije temelje (na primjer, iz spremišta temelje relacijskih spremište), potreban je korak serijalizacije ili deserijalizacija. Ovaj korak rezultira indirektni značajan performansi.

**Oblik datoteke**: odabrani oblik datoteke može utjecati na performanse Kopiraj. Na primjer, Avro je sažetom binarnom obliku koji pohranjuje metapodataka s podacima. Podrška za široke ima u zajednici Hadoop za obradu i postavljanje upita. Međutim, Avro je više pozivnim serijalizacije i deserijalizacija zbog čega se u donjem Kopiraj propusnost u usporedbi s tekstnom obliku. Odaberite datotečnog oblika tijekom obrade tijek holistically. Počinju što obrasca podataka pohranjena u izvoru podataka trgovine ili se izdvajati iz vanjskih sustava Najbolji format za pohranu, analitičke obrade i postavljanje upita; i u kojem obliku treba, podaci se izvesti u marts podataka za alata za izvješćivanje i vizualizaciju. Katkad se oblik datoteke koji je ili lošije za čitanje, a pisanje performanse mogu biti dobar izbor kada razmislite o ukupni analytical postupak.

## <a name="considerations-for-compression"></a>Zahtjevi za spajanje
Kada je ulazni i izlazni skup podataka u datoteku, možete postaviti Kopiraj aktivnosti da biste izvršili sažimanje ili dekompresiju zapisuje podatke na odredište. Kad odaberete spajanja, provjerite tradeoff između ulazni i izlazni / (i) i procesora. Sažimanje podataka troškova dodatni računalnim resursa. No u povrat smanjuje mrežni/i i pohranu. Ovisno o vašim podacima, možda ćete vidjeti isticanja u ukupnu propusnost Kopiraj.

**Kodek**: Kopiraj aktivnosti podržava gzip, bzip2 i vrste Deflate sažimanja. Azure HDInsight može raditi sve tri vrste za obradu. Svaki kodek ima sljedeće prednosti. Na primjer, bzip2 ima najmanje propusnost Kopiraj, ali se najbolje performanse upita grozd s bzip2 jer ga podijelite za obradu. Gzip je mogućnost najčešće Balansirani pa se najčešće koristi. Odaberite kodek koji najbolje odgovara scenariju završetka do kraja.

**Razina**: možete odabrati dvije mogućnosti za svaku kodek: fastest komprimiran i optimalnog komprimirane. Fastest Komprimirana mogućnost podatke komprimira što prije, čak i ako rezultat ne optimalnog komprimiranja datoteke. Mogućnost optimalnog Komprimirana provodi više vremena na sažimanja i rezultira minimalnog količinu podataka. Možete testirati obje mogućnosti da biste vidjeli koji omogućuje bolje performanse cjelokupnog u vašem slučaju.

**Razmatranje odgovora**: da biste kopirali veliku količinu podataka između programa lokalne pohrane i oblaka, preporučuje se korištenje privremeni blobova s sažimanja. Pomoću privremeni prostor za pohranu je korisno kada propusnosti s korporacijskom mrežom i servisa Azure je ograničavanje faktor, a želite skup podataka ulazni i izlazni skup podataka oba biti u obliku koje nisu komprimirane. Preciznije, prelomite jednu kopiju aktivnosti u dva Kopiraj aktivnosti. Prvi aktivnosti Kopiraj kopira iz izvorišnog web-mjesta da biste privremeno ili pripremna blob Komprimirana obrascu. Drugi aktivnosti Kopiraj kopira sažete podatke iz pripremna, a zatim dekomprimira dok je zapisuje primatelj.

## <a name="considerations-for-column-mapping"></a>Zahtjevi za mapiranje stupca
Svojstvo **columnMappings** možete postaviti u aktivnosti Kopiraj sve mape ili podskup unos stupce koje želite izlaznih stupaca. Nakon premještanja podatkovnog servisa čita podatke iz izvora, on mora poduzeti mapiranja stupca na podacima prije zapisuje podatke primatelj. U ovom dodatnu obradu smanjuje propusnost Kopiraj.

Ako spremanje izvorišnih podataka je možete, na primjer, ako je relacijski spremište kao što su baze podataka SQL ili SQL Server, ili ako je u trgovini NoSQL kao što su spremište tablica ili DocumentDB, razmislite o margina filtriranje stupca, a Promjenom redoslijeda logike svojstvo **upita** umjesto korištenja mapiranja stupca. Na taj način projekciju pojavljuje dok podatkovnog servisa za premještanje čita podatke iz spremišta izvora podataka, pri čemu je mnogo učinkovitiji.

## <a name="considerations-for-data-management-gateway"></a>Zahtjevi za pristupnik za upravljanje podacima
Preporuke za postavljanje pristupnika, potražite u članku [pitanja o korištenju pristupnik za upravljanje podacima](data-factory-move-data-between-onprem-and-cloud.md#Considerations-for-using-Data-Management-Gateway).

**Okruženje za strojno pristupnika**: preporučujemo korištenje namjenski strojno glavno računalo za pristupnik za upravljanje podacima. Alati za korištenje kao što su programom PerfMon da biste pregledali korištenje procesora i memorije propusnosti tijekom postupka kopiranja na pristupnom računalu. Prijeđite na jače računalo ako procesora, memorije ili propusnost mreže postane usko grlo.

**Istodobni Kopiraj aktivnosti pokreće**: instancu pristupnik za upravljanje podacima vam može poslužiti više kopija aktivnosti se pokreće u isto vrijeme ili istodobno. Maksimalni broj Istodobni poslovi se izračunava na temelju na pristupnom računalu hardverske konfiguracije. Dodatne kopije zadataka u redu čekanja dok izdvajanja pristupnika ili dok se ne promijeni posao ističe. Da biste izbjegli Nadmetanje resursa na pristupnom računalu, faze raspored Kopiraj aktivnosti da biste smanjili broj kopija zadataka u redu čekanja po ili razmislite o Podjela opterećenje na više računala pristupnika.


## <a name="other-considerations"></a>Druge napomene
Ako je veličina podataka koje želite kopirati velik, možete je prilagoditi poslovne logike za daljnje particija podacima pomoću slicing mehanizam u tvorničke podataka. Nakon toga zakazivanje Kopiraj aktivnosti da biste pokrenuli češće da biste smanjili veličinu podatke za svaku aktivnost Kopiraj pokrenuti.

Budite oprezni broj skupova podataka i Kopiraj aktivnosti koje je obavezna podataka tvorničke poveznik isti spremište podataka u isto vrijeme. Mnoge zadatke Istodobni Kopiraj možda throttle spremišta podataka i potencijalnim da su smanjene performanse, a zatim Kopiraj posao Interna ponovne pokušaje, a u nekim slučajevima, Neuspjelo izvršavanje.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Uzorak scenarija: Kopiraj iz programa lokalnog sustava SQL Server u spremište blobova platforme
**Scenarij**: da biste kopirali podatke iz programa sustava SQL Server na lokalno spremište blobova platforme u obliku CSV koji se temelji na kanal. Da biste brže Kopiranje posla, CSV datoteke mora se spojiti u bzip2 oblik.

**Testiranje i analize**: propusnost Kopiraj aktivnosti je MB/s manje od 2, koja je mnogo manja od usporednih performansi.

**Analiza performansi i ugađanje**: da biste otklonili poteškoće s performansama, Pogledajmo kako obrađuju i premjestiti podatke.

1.  **Čitanje podataka**: pristupnik otvara vezu sa sustavom SQL Server i šalje upit. SQL Server odgovori slanjem toka podataka pristupnik putem intraneta.
2.  **Serialize i sažimanje podataka**: pristupnik serializes toka podataka u obliku CSV i komprimira podataka bzip2 strujanje.
3.  **Zapisivanje podataka**: pristupnik prenosi strujanje bzip2 sa spremištem blobova putem Interneta.

Kao što možete vidjeti, podaci se obrađuju i premjestiti strujanje uzastopnih način: SQL Server > LAN-a > pristupnika > WAN > bloba prostora za pohranu. **Performanse cjelokupnog je gated po minimalne propusnost preko kanal**.

![Toka podataka](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Usko grlo performanse mogu uzrokovati jedan ili više od sljedećih čimbenika:

-   **Izvor**: SQL Server sam ima malo propusnost zbog podebljanom opterećenje.
-   **Pristupnik za upravljanje podacima**:
    -   **LAN-a**: pristupnik nalazi far from strojno SQL Server i sadrži vezu niske propusnosti.
    -   **Pristupnik**: pristupnik je Pristigla učitavanja ograničenja za izvesti sljedeće radnje:
        -   **Serijalizacije**: Serijalizacija toka podataka u obliku CSV je spora propusnost.
        -   **Sažimanje**: odaberete sporo sažimanja kodek (na primjer, bzip2, što je 2,8 MB/s s osnovnim i7).
    -   **WAN**: nema dovoljno propusnosti između poslovnoj mreži i servisa Azure (na primjer, T1 = 1,544 kb/s; T2 = 6,312 kb/s).
-   **Sita**: blobova ima malo propusnost. (Ovaj scenarij je vjerojatno jer je njegova SLA jamčiti najmanje 60 MB/s).

U ovom slučaju bzip2 spajanja podataka možda usporava cijelu kanal. Prijelaz na kodek za sažimanje gzip možda olakšalo njihovo ovaj usko grlo.


## <a name="sample-scenarios-use-parallel-copy"></a>Ogledni scenariji: paralelno kopiranje  

**Scenarij I:** Kopirajte 1000 1 MB datoteke s lokalnim sustavom za datoteke sa spremištem blobova.

**Analiza i ugađanje performansi**: na primjer, ako ste instalirali pristupnika na računalu s četverosmjernom core tvorničke podataka koristi 16 paralelno kopije da biste premjestili datoteke iz sustava datoteka blobova istovremeno. Ovaj paralelno izvođenja moraju rezultirati visoke propusnost. Možete izričito navesti broj paralelno kopije. Kada kopirate više male datoteke, paralelnog kopije znatno pomoći propusnost učinkovitije korištenje resursa.

![Scenarij 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Scenarij II**: kopirajte 20 blob-ova 500 MB iz spremišta blobova podataka Lake spremište analize, a zatim precizno podesite performanse.

**Analiza i ugađanje performansi**: U ovom scenariju podataka tvorničke kopira podatke iz spremišta blobova spremište Lake podataka pomoću jedne Kopiraj (**parallelCopies** postavite na 1) i jedinice premještanje podataka jednom oblaka. Propusnost se bit će blizu koji opisane u [sekciji pregled performanse](#performance-reference).   

![Scenarij 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Scenarij III**: veličina datoteke veća od desetke MB prostora i ukupni volumen prevelik.

**Analiza i performanse uključivanjem**: povećanje **parallelCopies** neće rezultirati bolje performanse Kopiraj zbog ograničenja resursa jedne oblaka DMU. Umjesto toga, trebali biste navesti dodatne oblaka DMUs da biste dobili dodatne resurse za izvođenje premještanje podataka. Određivanje vrijednosti za svojstvo **parallelCopies** . Tvorničke podataka obrađuje na parallelism umjesto vas. U ovom slučaju ako **cloudDataMovementUnits** postavljena na 4, propusnost od otprilike četiri puta pojavljuje se.

![Scenarij 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Referenca
Ovo su performanse nadzor i ugađanje reference za neke od podržanih trgovine:

- Azure prostora za pohranu (uključujući blobova i spremište tablica): [ciljevi skalabilnost Azure prostora za pohranu](../storage/storage-scalability-targets.md) i [pohranu Azure performanse i skalabilnost kontrolni popis](../storage//storage-performance-checklist.md)
- Azure SQL baze podataka: Možete [monitor performanse](../sql-database/sql-database-service-tiers.md#monitoring-performance) i provjerite postotka jedinica (DTU) transakcije baze podataka
- Azure SQL Data Warehouse: Njegov mogućnost mjeri se u jedinice skladištu podataka (DWUs) potražite u članku [Upravljanje izračunati power u skladištu podataka SQL Azure (pregled)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
- Azure DocumentDB: [performanse razina u DocumentDB](../documentdb/documentdb-performance-levels.md)
- Lokalnog sustava SQL Server: [monitoru i podešavanje performanse](https://msdn.microsoft.com/library/ms189081.aspx)
- Lokalni datoteke poslužitelja: [ugađanje performansi za datoteke poslužitelja](https://msdn.microsoft.com/library/dn567661.aspx)
