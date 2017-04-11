<properties
   pageTitle="Sastavljanje integrirani rješenja s SQL Data Warehouse | Microsoft Azure"
   description="Alati i partnerima s rješenjima integriranih s SQL Data Warehouse. "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="leverage-other-services-with-sql-data-warehouse"></a>Korištenje drugim servisima SQL Data Warehouse
Osim njegov osnovne funkcije SQL Data Warehouse omogućuje korisnicima odražava mnoge od drugih servisa u Azure duž ga.  Konkretno, ne možemo trenutno stupila na korake da biste Duboko integrirati s sljedeće:

+ Power BI
+ Tvorničke Azure podataka
+ Azure strojnog učenja
+ Azure strujanje Analytics

Radimo da biste se povezali s više servisima preko Azure zajednici.

##<a name="power-bi"></a>Power BI
Integracija s programom Power BI omogućuje vam korištenje računalnim moć SQL Data Warehouse dinamički izvještaja o i vizualizaciju dodatka Power BI. Integracija s programom Power BI trenutno obuhvaća:

+ **Izravni povezivanje**: na više napredne veze sa logičke Potiskivanje prema dolje prema SQL Data Warehouse.  To omogućuje brže analizu na veće mjerilo.
+ **Otvaranje u dodatku Power BI**: gumba Otvori u dodatku Power BI prosljeđuje informacije instanca servisa Power BI omogućuje jednostavnije veze.

Pročitajte članak [Integrate pomoću dodatka Power BI](./sql-data-warehouse-integrate-power-bi.md) ili [dokumentacija za Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) za dodatne informacije.

##<a name="azure-data-factory"></a>Tvorničke Azure podataka
Azure tvorničke podataka korisnicima daje upravljanih platformu da biste stvorili složene kanali izdvojiti Učitaj.  SQL Data Warehouse Integracija s Azure podataka tvorničke obuhvaća sljedeće:

+ **Pohranjene procedure**: orkestrirali izvršavanja pohranjene procedure na SQL Data Warehouse.
+ **Kopiranje**: korištenje ADF da biste premjestili podatke u SQL Data Warehouse.  Ovaj postupak možete koristiti ADF na standardni podaci premještanja mehanizam ili PolyBase u odjeljku s naslovnice. 

Pročitajte članak [Integrate s tvorničke Azure podataka](./sql-data-warehouse-integrate-azure-data-factory.md) ili [Azure podataka tvorničke dokumentaciju](https://azure.microsoft.com/documentation/services/data-factory/) za dodatne informacije.

##<a name="azure-machine-learning"></a>Azure strojnog učenja
Azure strojnog učenja je servis za potpuno upravljanih analize koji korisnicima omogućuje stvaranje složene modela korištenje velikih skup alata za predvidljivu.  SQL Data Warehouse podržana kao izvor i odredište te modela pomoću sljedeće funkcije:

+ **Čitanje podataka:** Pogon modela na razini pomoću T SQL protiv SQL Data Warehouse.
+ **Zapisivanje podataka:** Zapiši promjene iz bilo kojeg modela natrag u SQL Data Warehouse.

Pročitajte članak [Integrate s Azure strojnog učenja](./sql-data-warehouse-integrate-azure-machine-learning.md) ili [Azure strojnog učenja dokumentaciju](https://azure.microsoft.com/services/machine-learning/) za dodatne informacije.

##<a name="azure-stream-analytics"></a>Azure strujanje Analytics
Azure analize strujanje je složene, potpuno upravljanih infrastrukture za obradu i potrošnje stvorene iz koncentratora događaj Azure podaci o događaju.  Integracija sa SQL Data Warehouse omogućuje tok učinkovito obrađuju i pohranjene uz relacijskih podataka Omogućivanje dublju, napredne analize podataka.  

+ **Posla izlaz:** Slanje Izlaz iz strujanje Analitika poslova izravno SQL Data Warehouse.

Pročitajte članak [Integrate s Azure strujanje analize](./sql-data-warehouse-integrate-azure-stream-analytics.md) ili [Azure strujanje analize dokumentaciju](https://azure.microsoft.com/documentation/services/stream-analytics/) za dodatne informacije.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
