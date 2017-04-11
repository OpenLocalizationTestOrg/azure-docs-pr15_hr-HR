<properties
   pageTitle="Migracija: Podataka skladištu Utility za migraciju | Microsoft Azure"
   description="Migrirati u SQL Data Warehouse."
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
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="data-warehouse-migration-utility-preview"></a>Za migraciju podataka skladištu (pretpregled)

> [AZURE.SELECTOR]
- [Preuzimanje Utility za migraciju][]

Podaci skladištu migracije uslužni alat je namijenjenu migrirati shemu i podatke iz sustava SQL Server i baze podataka SQL Azure Azure SQL Data Warehouse. Tijekom migracije sheme alat automatski mapira odgovarajuće sheme iz izvora odredište. Nakon što u shemi sadrži je migrirati, alate pruža mogućnost premještanje podataka s automatski generirani skripti.

Osim shemu i podatke migracije ovaj alat omogućuje vam da biste generirali kompatibilnost izvješća koja sažetak nekompatibilnosti između ciljna i izvorna instanci koje onemogućuju pojednostavnjeno migracije.

## <a name="get-started"></a>Početak rada
Kao preduvjeta za instalaciju je potrebno, trebat će vam uslužni BCP naredbenog retka za pokretanje migracije skripte i sustava Office da biste pogledali izvješće o kompatibilnosti. Nakon pokretanja izvršnu datoteku koja se preuzima zatražit će se da biste prihvatili standardnu licencnog ugovora prije nego što će se instalirati alat.

Osim toga, da biste pokrenuli Utiliy migracije, potrebno je onaj pratiti dozvole na bazu podataka koju želite migrirati: Stvaranje baze podataka, PROMIJENITI bilo koje baze podataka ili PRIKAZU SVAKA DEFINICIJA.

### <a name="launching-the-tool-and-connecting"></a>Pokretanje alata i povezivanje
Pokretanje alata klikom na ikonu na radnoj površini koji se pojavljuje nakon instalacije. Prilikom otvaranja alata, zatražit će se stranicom prvotne veze odaberite izvor i odredište alata za migraciju. Trenutačno podržavamo SQL Server i baze podataka SQL Azure kao izvora i SQL Data Warehouse kao odredište. Nakon odabira to će se tražiti da se povezati s izvorišnim poslužiteljem ispunjavanje naziv poslužitelja i provjera autentičnosti, a potom 'Poveži'.

Nakon provjere autentičnosti, alat za prikazat će se popis bazama podataka koje se nalaze na poslužitelju koji su povezani s. Migracija možete započeti tako da odaberete bazu podataka koju želite migrirati, a zatim klikom na "Migriranje odabrana".

## <a name="migration-report"></a>Izvještaja o migraciji
Odabir 'Provjera baze podataka kompatibilnosti' u alatu generirat će izvješća sa sažetkom sve nekompatibilnosti objekata u bazi podataka koju želite migrirati. Šira popis nekih funkcionalnost sustava SQL Server koja ne postoji u SQL Data Warehouse pronaći ćete u našem [dokumentaciju za migraciju][]. Kada se generira izvješće moći za spremanje i otvaranje izvješća u programu Excel.

Napomena pri stvaranju migracije shemu, većina problema koja je prepoznata kao 'Objekt' će se prilagoditi kako biste omogućili odmah migracije tih podataka. Pregledajte promjene da biste bili sigurni da želite napraviti dodatne prilagodbe prije primjene u shemi.

## <a name="migrate-schema"></a>Migriranje sheme

Nakon povezivanja odabir 'Migrirati sheme' generirat će skripti za migraciju sheme za odabrane tablice. U ovom priključci skripte strukturu tablice, karte kompatibilan podataka vrste više kompatibilne obrazaca i stvara sigurnosne vjerodajnice i sheme ako je to označena je korisnik u postavkama za migraciju. Kod mogu se izvoditi prema ciljnim instancu sustava SQL Data Warehouse, spremiti u datoteku, kopiraju u međuspremnik ili čak i uređivati u retku prije poduzimanja dodatne akcije.  

Kao navedena iznad, prilikom migriranja sheme pregled migracije promjene koje alat učinio da bi se Pobrinite se da u potpunosti razumjeti ih.  

## <a name="migrate-data"></a>Migracija podataka

Klikom na mogućnost "Migrirati podatke" možete generirati BCP skripte koje će premještanje podataka najprije paušalni datotekama na poslužitelju, a zatim izravno u SQL Data Warehouse. Preporučujemo da se taj postupak za premještanje manjih količina podataka i kao ponovne pokušaje nisu ugrađene te pogreške može doći ako postoji gubitak mrežne veze. Da biste pokrenuli, morat ćete imati instaliran BCP naredbenog retka za, a zatim u shemi podataka mora već stvorili.

Nakon ispunjavanja gornje parametre samo morate kliknuti izvođenja migracije i skup dva paketa bit će načinjena navedeno mjesto. Pokrenite izvezene datoteke da bi se izvoz podataka iz izvora migracije u plošnu datoteke, a da biste se uvesti podatke u SQL Data Warehouse pokrenuti datoteke za uvoz.

## <a name="next-steps"></a>Daljnji koraci
Sad kad ste migrirati neke podatke, pogledajte upute za [razvoj][].

<!--Image references-->

<!--Article references-->
[Dokumentacija za migraciju]: sql-data-warehouse-overview-migrate.md
[razvoj]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Preuzimanje Utility za migraciju]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
