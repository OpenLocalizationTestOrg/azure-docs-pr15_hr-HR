<properties
   pageTitle="Kopirajte podatke iz blob polja za pohranu Azure u spremište Lake podataka | Microsoft Azure"
   description="Kopirajte podatke iz blob polja za pohranu Azure u spremištu Lake podataka pomoću alata za AdlCopy"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Kopirajte podatke iz blob polja za pohranu Azure u spremištu Lake podataka

> [AZURE.SELECTOR]
- [Korištenje DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [Korištenje AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)

Spremište Lake podataka za Azure omogućuje alata za naredbeni redak, [AdlCopy](http://aka.ms/downloadadlcopy), da biste kopirali podatke iz sljedećih izvora:

* Iz spremišta Azure blob-ova u spremištu Lake podataka. AdlCopy ne možete koristiti da biste kopirali podatke iz spremišta podataka Lake blob-ova Azure prostora za pohranu.

* Između dva računa spremišta Lake za Azure podataka. 

Osim toga, možete koristiti alat za AdlCopy u dva različita načina rada:

* **Samostalni**, gdje alat koristi spremište podataka Lake resursi za izvođenje zadatka.

* **Pomoću računa sustava analize podataka Lake**, gdje jedinice dodijeljena računu analize podataka Lake koriste se za izvođenje postupka kopiranja. Možda ćete morati tu mogućnost koristite kada tražite izvršavanje zadataka Kopiraj predvidljivi način.

##<a name="prerequisites"></a>Preduvjeti

Prije nego počnete u ovom se članku, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

- Spremnik **Blob polja za pohranu Azure** sadrži podatke.

- **Račun za azure podataka Lake analize (neobavezno)** – potražite u članku [Početak rada s Azure podataka Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) za upute za stvaranje računa spremišta Lake podataka.

- **Alat za AdlCopy**. Instalirajte alat za AdlCopy iz [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>Sintaksa alata za AdlCopy

Upotrijebite sljedeću sintaksu za rad s alatom za AdlCopy

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

Slijedi opis parametara u sintaksi:

| Mogućnost    | Opis                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| Izvor    | Određuje mjesto izvorišnih podataka u blobova platforme Azure prostora za pohranu. Izvor može biti spremniku blob, blob ili neki drugi račun za spremište Lake podataka.                                                                                                                                                                                                                                                                                                 |
| Odredišne      | Određuje odredište spremišta Lake podataka da biste kopirali.                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | Određuje tipkovni prečac za pohranu za izvor blobova platforme Azure prostora za pohranu. To je potrebna samo ako je izvor spremniku blob ili blob.                                                                                                                                                                                                                                                                                                                                                 |
| Računa   | **Nije obavezno**. Tom se mogućnošću poslužite ako želite da se pomoću računa za Azure podataka Lake analize Pokreni Kopiraj. Ako koristite mogućnost /Account u sintaksi, ali navedite analize podataka Lake račun, AdlCopy koristi zadani račun za izvođenje zadatka. Osim toga, ako koristite tu mogućnost, morate dodati (Azure spremište blobova platforme) izvorišne i odredišne (Azure spremišta Lake podataka) kao izvora podataka za vaš račun analize podataka Lake.  |
| Jedinice     |  Određuje broj jedinica analize Lake podataka koji će se koristiti za kopiranje posla. Ta je mogućnost obavezno ako koristite mogućnost **/Account** da biste odredili analize podataka Lake računa.
| Uzorak   | Određuje uzorak regex koji pokazuje koja blob-ova ili datoteka za kopiranje. AdlCopy koristi velika i mala slova koji se podudaraju. Zadani uzorak koristi kada nema uzorak nije naveden je da biste kopirali sve stavke. Određivanje uzoraka više datoteka nisu podržani.                                                                                                                                                                                                                                                                                                                                               



## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>Korištenje AdlCopy (kao što je samostalne) da biste kopirali podatke iz programa Azure spremište blobova platforme

1. Otvorite naredbeni redak, a zatim otvorite direktorija AdlCopy instaliranim obično `%HOMEPATH%\Documents\adlcopy`.

2. Pokrenite sljedeću naredbu da biste kopirali određene blob iz izvora spremnik Lake izvor podataka:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Ako, na primjer:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    
    >[AZURE.NOTE] Vidjet ćete da sintaksa iznad određuje datoteku da biste kopirali mapu računa spremišta Lake podataka. Alat za AdlCopy stvara mapu ako navedeni naziv mape ne postoji.

    Zatražit će se da biste unijeli vjerodajnice za Azure pretplatu na kojem imate računa spremišta Lake podataka. Prikazat će se na Izlaz sličnu ovoj:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Možete kopirati i sve blob-ova iz jedan spremnik računom spremišta Lake podataka pomoću sljedeće naredbe:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Ako, na primjer:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>Korištenje AdlCopy (kao što je samostalne) da biste kopirali podatke s drugog računa spremišta Lake podataka

Da biste kopirali podatke između dva računa spremišta Lake podataka možete koristiti i AdlCopy.

1. Otvorite naredbeni redak, a zatim otvorite direktorija AdlCopy instaliranim obično `%HOMEPATH%\Documents\adlcopy`.

2. Pokrenite sljedeću naredbu da biste kopirali određenom datotekom s jednog računa spremišta Lake podataka u drugu.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Ako, na primjer:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

    >[AZURE.NOTE] Vidjet ćete da sintaksa iznad određuje datoteku želite kopirati u mapu na odredištu računa spremišta Lake podataka. Alat za AdlCopy stvara mapu ako navedeni naziv mape ne postoji.

    Zatražit će se da biste unijeli vjerodajnice za Azure pretplatu na kojem imate računa spremišta Lake podataka. Prikazat će se na Izlaz sličnu ovoj:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Sljedeću naredbu kopira sve datoteke iz određene mape u izvoru spremišta podataka Lake računa u mapu na odredištu računa spremišta Lake podataka.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>Korištenje AdlCopy (s računom analize podataka Lake) da biste kopirali podatke

Računa analize Lake podataka možete koristiti i da biste pokrenuli posao AdlCopy da biste kopirali podatke s blob-ova Azure prostora za pohranu podataka Lake Store. To najčešće koristite tu mogućnost kada su podaci će se premjestiti u raspon gigabajta i terabajta, a želite propusnost performanse bolje i predvidljivi.

Da biste koristili račun analize podataka Lake s AdlCopy možete kopirati iz programa Azure spremišta blobova, kao izvor podataka za vaš račun analize podataka Lake mora biti dodan izvorišnog web-mjesta (Azure spremište blobova platforme). Upute o dodavanju dodatne izvore podataka na račun servisa analize Lake podataka potražite u članku [Upravljanje analize podataka Lake računa izvora podataka](..//data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

>[AZURE.NOTE] Ako kopirate s računa za spremište Lake za Azure podataka kao izvor pomoću analize podataka Lake računa, ne morate pridružiti računa spremišta podataka Lake analize podataka Lake računa. Zahtjev za pridruživanje izvorišnog spremišta računu analize podataka Lake je samo kada je izvor račun za Azure prostora za pohranu.

Pokrenite sljedeću naredbu da biste kopirali programa Azure spremište blobova platforme spremišta podataka Lake račun pomoću analize podataka Lake računa:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Ako, na primjer:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2


Isto tako, pokrenite sljedeću naredbu da biste kopirali programa Azure spremište blobova platforme spremišta podataka Lake račun pomoću analize podataka Lake računa:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Korištenje AdlCopy da biste kopirali podatke pomoću uzorka

U ovom se odjeljku saznat ćete kako koristiti AdlCopy da biste kopirali podatke iz izvora (u našem primjeru u nastavku koristimo Azure spremišta blobova) s računom spremišta podataka Lake odredište pomoću uzorka. Na primjer, možete koristiti korake u nastavku da biste kopirali sve datoteke s nastavkom .csv s blob izvora na odredište.

1. Otvorite naredbeni redak, a zatim otvorite direktorija AdlCopy instaliranim obično `%HOMEPATH%\Documents\adlcopy`.

2. Pokrenite sljedeću naredbu da biste kopirali sve datoteke s nastavkom *.csv određene blob iz izvora spremnik za Lake izvor podataka:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Ako, na primjer:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

    
## <a name="billing"></a>Naplata

* Ako koristite alat za AdlCopy kao samostalan proizvod vam naplaćivati za izlazne troškove za premještanje podataka, ako izvorišnog web-mjesta za pohranu Azure račun ne nalazi na istom području Lake pohrane podataka.

* Ako koristite alat za AdlCopy sa svojim računom analize podataka Lake, primjenjuju se standardni [analize podataka Lake tečajeve za naplatu](https://azure.microsoft.com/pricing/details/data-lake-analytics/) .

## <a name="considerations-for-using-adlcopy"></a>Okolnosti pri korištenju AdlCopy

* AdlCopy (za verziju 1.0.5), podržava kopiranje podataka iz izvora koji skupno imati više od tisuću datoteka i mapa. Međutim, Ako naiđete na probleme kopiranje velike skup podataka, možete distribucija datoteke i mape u različitim podmape i upotrijebite put za te podređene mape kao izvor.

## <a name="next-steps"></a>Daljnji koraci

- [Zaštite podataka u spremištu Lake podataka](data-lake-store-secure-data.md)
- [Korištenje analize Lake Azure podataka s spremišta Lake podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight pomoću spremišta Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)
