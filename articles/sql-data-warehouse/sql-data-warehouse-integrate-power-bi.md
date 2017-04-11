<properties
   pageTitle="Korištenje servisa Power BI s SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za korištenje servisa Power BI pomoću Azure SQL Data Warehouse za razvoj rješenja."
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

# <a name="use-power-bi-with-sql-data-warehouse"></a>Korištenje servisa Power BI s SQL Data Warehouse
Kao s bazom podataka SQL Azure, SQL podataka skladištu izravno povezivanje omogućuje korisniku odražava Napredna logičke Potiskivanje prema dolje uz analitičkih mogućnosti dodatka Power BI.  S izravno povezivanje upiti šalju natrag na skladištu podataka SQL Azure u stvarnom vremenu kao Istraživanje podataka.  Ovaj se kombinirati s skale korisnicima omogućuje da biste stvorili dinamičnu izvješća u minutama protiv terabajta podataka SQL Data Warehouse.  Osim toga, Uvod gumba Otvori u Power BI korisnicima omogućuje izravno povezati Power BI SQL Data Warehouse bez prikupljanje informacija iz drugih dijelova Azure.

Prilikom korištenja izravno povezivanje provjerite bilješke:

+ Navedite naziv poslužitelja za potpuno kvalificiran prilikom povezivanja (potražite u nastavku više detalja)
+ Provjerite je li pravila vatrozida za bazu podataka konfigurirani tako da "Dopusti pristup servisima Azure".
+ Svaku akciju kao što su odabirom stupca ili dodavanje filtra izravno upit skladištu podataka
+ Pločice se osvježavaju otprilike svakih 15 minuta (osvježavanja ne morate zakazivati)
+ Značajka pitanja i odgovora nije dostupna za izravno povezivanje skupova podataka
+ Promjena sheme ne izdvajaju automatski
+ Svi upiti izravno povezivanje će istek vremena nakon dvije minute

Ove ograničenja i bilješke može promijeniti kako ćemo da biste poboljšali rad s obrascem. Ispod detaljno opisane korake za povezivanje.  

## <a name="using-the-open-in-power-bi-button"></a>Pomoću gumba Otvori u dodatku Power BI
Da biste se kretali SQL Data Warehouse i Power BI najjednostavnije pomoću gumba Otvori u Power BI. Ovaj gumb omogućuje jednostavno početi stvarati nove nadzornih ploča u dodatku Power BI.  

1.  Da biste započeli idite na instancu sustava SQL Data Warehouse na portalu klasični Azure.
2.  Kliknite gumb Otvori u dodatku Power BI.
3.  Ako ne možemo prijavu izravno ili ako nemate račun za Power BI, morat ćete prijavu.  
4.  Bit ćete preusmjereni na stranici SQL Data Warehouse veze s podacima sustava SQL Data Warehouse je unaprijed popunjeno.
5.  Kada unesete vjerodajnice koje će biti potpuno povezani SQL Data Warehouse.

## <a name="connecting-through-the-power-bi-portal"></a>Povezivanje putem portala za Power BI
Osim pomoću gumba Otvori u Power BI, korisnici možete povezati na njihove SQL Data Warehouse putem portala za Power BI.

1.  Kliknite "Dohvati podatke" pri dnu navigacijskog okna.
2.  Odaberite "Baza podataka".
3.  Kada na stranici baze podataka odaberite "Azure SQL Data Warehouse", a zatim kliknite 'Poveži'.
4.  Unesite podatke o vezi potrebne.  Na portalu za Azure moguće je pronaći naziv poslužitelja i naziv baze podataka.
5.  Bit će preusmjereni natrag na glavnoj stranici Power BI, a nakon veza postane novu stavku u odjeljku 'Skupove podataka' pojavit će se s nazivom svoje instance.  
6.   Možete kliknuti novi skup podataka da biste istražili sve tablice i prikaze u bazi podataka. Odabir stupca slati upit izvora, dinamičko stvaranje vizualizaciju. Ove signale mogu spremiti u novo izvješće i prikvačiti nadzorne ploče.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
