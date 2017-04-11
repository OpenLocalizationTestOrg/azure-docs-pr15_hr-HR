<properties
   pageTitle="Azure strojnog učenja pomoću SQL Data Warehouse | Microsoft Azure"
   description="Praktični vodič za korištenje Azure strojnog učenja za Azure SQL Data Warehouse za razvoj rješenja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>Azure strojnog učenja pomoću SQL Data Warehouse

Azure strojnog učenja je potpuno upravljanih predvidljivu analize servis koji koriste da biste stvorili predvidljivu modela na temelju podataka u SQL Data Warehouse, a zatim Objavi kao spremna-na-zauzeti web-servisi. Možete Informirajte se o osnovama predvidljivu analitikom i strojnog učenja tako da pročitate [Uvod u strojnog učenja na Azure][].  Zatim možete Saznajte kako stvoriti, obuka, rezultat i testiranje strojnog učenja modela pomoću [Stvaranje Poigrajte se Praktični vodič][].

U ovom se članku će saznat ćete kako se na sljedeći način pomoću [Azure strojnog učenja Studio][]:

- Čitanje podataka iz baze podataka da biste stvorili, obuka i rezultatu predvidljivu modela
- Zapisivanje podataka u bazu podataka


## <a name="read-data-from-sql-data-warehouse"></a>Čitanje podataka iz SQL Data Warehouse

Ne možemo će pročitati podatke iz tablice proizvoda u bazi podataka AdventureWorksDW.

### <a name="step-1"></a>Korak 1

Najprije kliknite novi eksperiment + NOVO pri dnu prozora strojnog učenja Studio odaberite EKSPERIMENT, a zatim odaberite prazno Poigrajte. Odaberite zadani naziv eksperiment pri vrhu područja crtanja i preimenujte je nešto smisleni, predviđanje, na primjer, cijenu za bicikl.

### <a name="step-2"></a>Korak 2

Potražite čitač modul u paleti skupova podataka i moduli na lijevoj strani eksperiment područje crtanja. Povucite modul eksperiment područje crtanja.
![][drag_reader]

### <a name="step-3"></a>Korak 3

Odaberite modul za čitanje i ispunjavanje okna sa svojstvima.

1. Odaberite baze podataka Azure SQL kao izvor podataka.
2. Naziv poslužitelja baze podataka: Unesite naziv poslužitelja. [Portal za Azure][] možete koristiti da biste pronašli to.

![][server_name]

3. Naziv baze podataka: upišite naziv baze podataka na poslužitelju koji ste upravo naveli.
4. Naziv poslužitelja korisničkog računa: Unesite korisničko ime računa s dozvolama za bazu podataka programa access.
5. Lozinka korisničkog računa poslužitelja: Navedite lozinku za određeni korisnički račun.
6. Prihvati sve certifikat poslužitelja: koristite tu mogućnost (manje siguran) ako želite preskočiti pregledavanje certifikatom web-mjesta prije čitanja podataka.
7. Upit za bazu podataka: Unesite SQL naredbe kojim opisujete podatke koje želite pročitati. U ovom slučaju ne možemo će pročitati podatke iz tablice proizvoda pomoću sljedeći upit.


```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Korak 4

1. Pokrenite pokusa tako da kliknete Pokreni u odjeljku eksperiment područje crtanja.
2. Kada se dovrši pokusa, modul čitač dodijelit će zelenu kvačicu da biste naznačili da je uspješno dovršeno. Imajte na umu i dovršeno radi status u gornjem desnom kutu.

![][run]

3. Da biste vidjeli uvezene podatke, kliknite izlazni priključak pri dnu područja skup podataka i odaberite Vizualiziraj.


## <a name="create-train-and-score-a-model"></a>Stvaranje, obuka i rezultatu modela

Sada možete koristiti ovaj skup podataka da biste:

- Stvaranje modela: Obrada podataka i definirali značajke
- Obuka modela: odabir i njegova Primjena algoritam za učenje
- Rezultat i testiranje modela: predviđanje novu cijenu za bicikl


![][model]

Da biste saznali više o stvaranju, obuka, rezultat i testiranje strojnog učenja model koristi [Stvaranje Poigrajte vodič][].

## <a name="write-data-to-azure-sql-data-warehouse"></a>Zapisivanje podataka Azure SQL Data Warehouse

Ne možemo će pisati skup ProductPriceForecast tablicu u bazi podataka za AdventureWorksDW rezultata.

### <a name="step-1"></a>Korak 1

Potražite Writer modul u paleti skupova podataka i moduli na lijevoj strani eksperiment područje crtanja. Povucite modul eksperiment područje crtanja.

![][drag_writer]

### <a name="step-2"></a>Korak 2

Odaberite modul za pisanje, a zatim ispunite okna sa svojstvima.

1. Odaberite baze podataka Azure SQL kao odredišta za podatke.
2. Naziv poslužitelja baze podataka: Unesite naziv poslužitelja. [Portal za Azure][] možete koristiti da biste pronašli to.
3. Naziv baze podataka: upišite naziv baze podataka na poslužitelju koji ste upravo naveli.
4. Naziv poslužitelja korisničkog računa: Unesite korisničko ime računa koji ima dozvolu za zapisivanje za bazu podataka.
5. Lozinka korisničkog računa poslužitelja: Navedite lozinku za određeni korisnički račun.
6. Prihvati sve certifikat poslužitelja (nepouzdanog): Odaberite ovu mogućnost ako ne želite da biste pogledali certifikat.
7. Popis stupaca spremiti odvojenih zarezom: Navedite popis skup podataka ili rezultat stupce koje želite poslati.
8. Naziv tablice podataka: Navedite naziv podatkovne tablice.
9. Popis stupaca tablica podataka odvojenih zarezom: odredite naziva stupaca u novu tablicu. Nazivi stupaca može se razlikovati od onih u skupu izvora podataka, ali vam mora biti navedena isti broj stupaca ovdje koju ste definirali za izlaznu tablicu.
10. Broj redaka napisali po operaciji SQL Azure: možete konfigurirati broj redaka koji se pišu s bazom podataka SQL odjednom.

![][writer_properties]

### <a name="step-3"></a>Korak 3

1. Pokrenite pokusa tako da kliknete Pokreni u odjeljku eksperiment područje crtanja.
2. Kada se dovrši pokusa, sve module dodijelit će zelenu kvačicu da biste naznačili da ih je uspješno dovršena.

## <a name="next-steps"></a>Daljnji koraci

Dodatne savjete za razvoj potražite u članku [Pregled SQL Data Warehouse razvoj][].

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[Pregled SQL Data Warehouse razvoj]: ./sql-data-warehouse-overview-develop.md
[Stvaranje eksperiment vodiča]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Uvod u učenje na Azure na računalu]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure strojnog učenja Studio]: https://studio.azureml.net/Home
[Portal za Azure]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
