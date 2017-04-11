<properties
   pageTitle="Analiza podataka pomoću Azure strojnog učenja | Microsoft Azure"
   description="Da biste sastavili predvidljivu strojnog učenja modela na temelju podataka pohranjenih u skladištu podataka za SQL Azure pomoću Azure strojnog učenja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/14/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="analyze-data-with-azure-machine-learning"></a>Analiza podataka pomoću Azure strojnog učenja

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure strojnog učenja](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Pomoću ovog praktičnog vodiča koristi Azure strojnog učenja radi stvaranja predvidljivu strojnog učenja modela na temelju podataka pohranjenih u skladištu podataka za SQL Azure. Konkretno, to sastavlja ciljano marketinške kampanje za Adventure Works, pogon bicikle po procjenjivanje ako je klijent vjerojatno kupiti u bicikle ili ne.

> [AZURE.VIDEO integrating-azure-machine-learning-with-azure-sql-data-warehouse]


## <a name="prerequisites"></a>Preduvjeti
Da biste se pomicali kroz ovaj Praktični vodič, potrebno je:

- SQL Data Warehouse unaprijed učitani s oglednim podacima AdventureWorksDW. Dodjela to, potražite u članku [Stvaranje SQL Data Warehouse][] i odaberite da biste učitali ogledne podatke. Ako već imate podatke skladištu, no ne sadrži ogledne podatke, možete ga [ručno učitavanje ogledne podatke][].

## <a name="1-get-data"></a>1. dohvaćanje podataka
Podaci u prikazu dbo.vTargetMail u AdventureWorksDW bazi podataka. Da biste pročitali ove podatke:

1. Prijavite se u [studio Azure strojnog učenja][] i kliknite Moje eksperimenata.
2. Kliknite **+ NOVO** i odaberite **Prazan eksperiment**.
3. Unesite naziv svoje eksperiment: ciljani Marketing.
4. Modul **čitač** iz modula okna povucite u područje crtanja.
5. U oknu svojstva navedite detalje baze podataka SQL Data Warehouse.
6. Odredite bazu podataka **upita** da biste pročitali relevantne podatke.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Izvođenje pokusa klikom na **Pokreni** u odjeljku eksperiment područje crtanja.
![Pokretanje pokusa][1]


Kada pokusa Završi uspješno pokretanje, kliknite izlazni priključak pri dnu modul čitač i odaberite **Vizualiziraj** da biste vidjeli uvezenih podataka.
![Prikaz uvezenih podataka][3]


## <a name="2-clean-the-data"></a>2. čišćenje podataka
Da biste očistili podatke, ispustite neke stupce koji nisu bitni za model. Da biste to učinili:

1. Povucite modul **Projekta stupci** u područje crtanja.
2. U oknu svojstva da biste naveli koje stupce želite ispustite kliknite **birač stupca za pokretanje** .
![Stupci projekta][4]

3. Isključivanje dva stupca: CustomerAlternateKey i ključ Zemljopisa.
![Uklanjanje nepotrebnih stupaca][5]


## <a name="3-build-the-model"></a>3. sastavljanje modela
Ne možemo će podijeliti podataka 80 do 20: 80% obuke za rad s strojnog učenja model i 20% da biste testirali modela. Ćemo pomoću algoritama "Dva klasa" Binarni klasifikacija problema.

1. Modul za **podjelu** povucite u područje crtanja.
2. Unesite 0,8 razlomak retke u prvi skup podataka Izlaz u oknu svojstva.
![Podijelite podataka u skup obuka i test][6]
3. Modul **Stabla odlučivanja Boosted dva predmete** povucite u područje crtanja.
4. Modul **Vlaku modela** odvučete područje crtanja i navedite ulaza. Zatim kliknite **Pokreni birač stupca** u oknu svojstva.
      - Najprije unos: ML algoritam.
      - Drugi unos: obuka algoritam na podatke.
![Povezivanje modul vlaku modela][7]
5. Odaberite stupac **BikeBuyer** kao stupac za predviđanje.
![Odaberite stupac za predviđanje][8]


## <a name="4-score-the-model"></a>4. rezultatu modela
Sada će ispitivanja kako model provodi testiranje podataka. Ne možemo će usporediti algoritam naš izbora s različitim algoritam da biste vidjeli koji izvršava bolje.

1. Modul **Rezultat modela** povucite u područje crtanja.
    Najprije unos: put uvježbavan modela drugi unos: Provjera podataka ![rezultatu modela][9]
2. **Dva klase Bayes točke strojno** odvučete eksperiment područje crtanja. Ne možemo će usporedite kako ovaj algoritam izvodi usporedbi stabla odlučivanja Boosted dva predmete.
3. Kopiranje i lijepljenje module vlaku modela i rezultat modela u područje crtanja.
4. Modul za **Procjenu modela** povucite u područje crtanja da biste usporedili dvije algoritama.
5. **Pokretanje** pokusa.
![Pokretanje pokusa][10]
6. Kliknite izlazni priključak pri dnu modul vrednovati Model, a zatim kliknite Vizualiziraj.
![Vizualni prikaz rezultata za procjenu][11]

Metriku navedene su ROC krivulje, dijagrame i Dizalica krivulje preciznosti opoziv. Pogled na te metriku, Vidimo bolje nego drugu izvršiti prvi će se model. Da biste vidjeli što prvi će se model predviđene, kliknite na izlazni priključak rezultat modela, a zatim Vizualiziraj.
![Vizualizacija rezultat rezultata][12]

Prikazat će se dva stupca dodane na vaše test skup podataka.

- Testu dobije vjerojatnosti: vjerojatnost da je kupac bicikle kupac.
- Natpisi testu dobije: klasifikaciju gotovo model – bicikle kupac (1) ili ne (0). Prag vjerojatnosti za označavanje postavljeno na 50%, a može se prilagoditi.

Usporedba stupca BikeBuyer (Stvarno) s natpisima testu dobije (predviđanje), možete vidjeti i kako je izvršila modela. Kao sljedeće korake, koristite ovaj model provjerite predviđanja za nove korisnike i objaviti ovaj model kao web-servisa ili odgovorili rezultate SQL Data Warehouse.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o izradi predvidljivu strojnog učenja modelima pogledajte [Uvod u strojnog učenja na Azure][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure strojnog učenja studio]:https://studio.azureml.net/
[Uvod u učenje na Azure na računalu]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[ručno učitavanje oglednih podataka]: sql-data-warehouse-load-sample-databases.md
[Stvaranje SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
