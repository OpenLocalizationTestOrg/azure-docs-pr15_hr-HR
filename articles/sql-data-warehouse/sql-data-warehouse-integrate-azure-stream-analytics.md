<properties
   pageTitle="Azure strujanje analize pomoću SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za korištenje Azure strujanje analize pomoću Azure SQL Data Warehouse za razvoj rješenja."
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

# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Azure strujanje analize pomoću SQL Data Warehouse

Azure strujanje analize je potpuno Upravljani servis nudi obrada najniža Latencija iznimno dostupna, skalabilni složene događaja putem strujanja podataka u oblaku. Osnove možete saznati tako da pročitate [Uvod u Azure toka analize][]. Zatim možete Saznajte kako stvaranje rješenja završetka do kraja s strujanje analize slijedeći vodič [Prvi koraci pri korištenju Azure strujanje analize][] .

U ovom se članku ćete naučiti da biste koristili Azure SQL Data Warehouse bazu podataka kao u izlaz primatelj poslova pare analize.

## <a name="prerequisites"></a>Preduvjeti

Prvo pokretanje kroz sljedeće korake u ovom praktičnom vodiču za [Prvi koraci pri korištenju Azure strujanje analize][] .  

1. Stvaranje ulazni koncentratora za događaj
2. Konfiguraciju i pokretanje aplikacija generator događaja
3. Dodjela resursa za posao strujanje Analytics
4. Navedite unos za posao i upita

Zatim stvorite bazu podataka Azure SQL Data Warehouse

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Odredite izlazni posao: Azure SQL Data Warehouse baze podataka

### <a name="step-1"></a>Korak 1

U svoj posao strujanje analize kliknite **IZLAZ** iz vrha stranice, a zatim **Dodajte IZLAZ**.

### <a name="step-2"></a>Korak 2

Odaberite SQL baze podataka, a zatim kliknite Dalje.

![][add-output]

### <a name="step-3"></a>Korak 3
Na sljedećoj stranici unesite sljedeće vrijednosti:

- *Pseudonim za izlaz*: Upišite neslužbeni naziv za ovaj zadatak izlaz.
- *Pretplate*:
    - Ako je baze podataka SQL Data Warehouse u okviru iste pretplate kao posao strujanje analize, odaberite SQL baza podataka iz trenutne pretplate.
    - Ako bazu podataka je u neku drugu pretplatu, odaberite koristi SQL baze podataka na drugu pretplatu.
- *Baza podataka*: Navedite naziv odredišnu bazu podataka.
- *Naziv poslužitelja*: Navedite naziv poslužitelja za bazu podataka koju ste upravo naveli. Da biste pronašli to možete koristiti Portal klasični Azure.

![][server-name]

- *Korisničko ime*: navedite korisničko ime računa koji ima dozvolu za zapisivanje za bazu podataka.
- *Lozinka*: Navedite lozinku za određeni korisnički račun.
- *Tablica*: Navedite naziv ciljne tablice u bazi podataka.

![][add-database]

### <a name="step-4"></a>Korak 4

Kliknite gumb Provjeri da biste dodali ovaj zadatak izlazne i da biste provjerili možete li strujanje Analytics uspješno povezati s bazom podataka.

![][test-connection]

Nakon uspješnog povezivanja s bazom podataka, vidjet ćete obavijest pri dnu portalu. Kliknite Testiraj vezu pri dnu da biste testirali vezu s bazom podataka.

## <a name="next-steps"></a>Daljnji koraci

Pregled integracije potražite u članku [Pregled integracije SQL Data Warehouse][].

Dodatne savjete za razvoj potražite u članku [Pregled SQL Data Warehouse razvoj][].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Uvod u Azure strujanje Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Prvi koraci pri korištenju Azure strujanje Analytics]: ../stream-analytics/stream-analytics-get-started.md
[Pregled SQL Data Warehouse razvoj]:  ./sql-data-warehouse-overview-develop.md
[Pregled integracije SQL Data Warehouse]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
