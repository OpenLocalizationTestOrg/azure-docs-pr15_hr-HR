<properties
    pageTitle="Azure SQL baza podataka JSON značajke | Microsoft Azure"
    description="Baze podataka SQL Azure omogućuje rastavljanju, upita i oblikovanje podataka JavaScript objekt notacije (JSON) notacijom."
    services="sql-database"
    documentationCenter=""
    authors="jovanpop-msft"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/17/2016"
    ms.author="jovanpop"
   ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Uvod u značajke JSON u bazi podataka SQL Azure

Azure SQL baze podataka možete analizirati upit podatke prikazane u oblik notacije objekt JavaScript [(JSON)](http://www.json.org/) i izvoz relacijskih podataka kao JSON tekst.

JSON je oblik popularne podataka koji se koristi za razmjenu podataka u Moderna web i mobilne aplikacije. JSON koristi se za pohranu djelomično strukturirane podatke u datoteke zapisnika ili u bazama podataka NoSQL kao što su [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Mnoge REST web-servisi vraćenim rezultatima oblikovane kao tekst JSON ili prihvaća podatke oblikovan kao JSON. Većina Azure servise kao što je [Azure pretraživanja](https://azure.microsoft.com/services/search/), [Azure prostora za pohranu](https://azure.microsoft.com/services/storage/)i [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) imati krajnje točke OSTALE koji vraćaju ili zauzeti JSON.

Baze podataka SQL Azure omogućuje rad s podacima JSON jednostavno i integracija bazu podataka s modernim services.

## <a name="overview"></a>Pregled

Baze podataka SQL Azure nudi sljedeće funkcije za rad s podacima JSON:

![Funkcija JSON](./media/sql-database-json-features/image_1.png)

Ako imate JSON teksta, možete izdvajanje podataka iz JSON ili provjerite je li JSON ispravno oblikovana pomoću ugrađene funkcije [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx)i [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx). Funkcija [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) možete ažurirati vrijednost unutar JSON tekst. Za dodatne mogućnosti upita i analiza, funkcija [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) možete transformirati polja JSON objekata u skup redaka. Bilo koji SQL upit možete izvršiti na skupu vraćenih rezultata. Na kraju, postoji uvjet [JSON za](https://msdn.microsoft.com/library/dn921882.aspx) koji vam omogućuje oblikovanje podataka pohranjenih u tablicama relacijski kao JSON tekst.

## <a name="formatting-relational-data-in-json-format"></a>Oblikovanje relacijskih podataka u JSON OSNOVNI oblik
Ako imate web-servisa koji traje podataka iz baze podataka slojeva i daje odgovor u JSON oblikovanje ili klijentsko okviri JavaScript ili biblioteka koje prihvaća podatke koji su oblikovani kao JSON, sadržaj baze podataka možete oblikovati kao JSON izravno u SQL upita. Više imate pisati aplikacije kod koji se oblikuje rezultate iz baze podataka SQL Azure kao JSON ili uključiti neke JSON serijalizacije biblioteke pretvorite rezultate tablični upita, a serijalizirati objekata JSON OSNOVNI oblik. Umjesto toga možete koristiti za JSON uvjet oblikovanje SQL rezultata upita kao JSON u bazi podataka SQL Azure i koristiti ga izravno u aplikaciji.

U sljedećem primjeru redaka iz tablice Sales.Customer su oblikovani kao JSON korištenjem uvjeta za JSON:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

Uvjet JSON put oblikuje rezultate upita kao JSON tekst. Nazivi stupaca se koriste kao tipke, dok je vrijednosti u ćelijama generiraju kao JSON vrijednosti:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

Skup rezultata je oblikovan kao JSON polja u kojima se svaki redak oblikovan kao zasebni JSON objekt.

PUT označava da izlazni oblik rezultata JSON možete prilagoditi pomoću notacija u stupcu pseudonima. Sljedeći upit promijeniti naziv ključ "ImeKlijenta" u izlaznom obliku JSON te stavlja telefona i faksa u podređeni objekt "Kontakt":

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

Izlaz iz tog upita izgleda ovako:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

U ovom primjeru smo vraća jedan objekt JSON umjesto polja navođenjem mogućnost [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) . Koristite ovu mogućnost ako znate vraćate jedan objekt kao rezultat upita.

Glavni vrijednost uvjeta za JSON je da ga možete vratiti složene hijerarhijske podatke iz baze podataka oblikovan kao ugniježđene JSON objekata ili polja. Sljedeći primjer prikazuje način da biste uključili narudžbe koji pripadaju kupca kao ugniježđene raspon narudžbe:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Umjesto slanja razdvojiti upita da biste dobili korisničkih podataka, a zatim za dohvaćanje popisa povezanih narudžbe, možete dobiti svih potrebnih podataka s jednog upita, kao što je prikazano u sljedeće ogledne izlaz:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>Rad s podacima JSON

Ako nemate isključivo strukturiranih podataka ako imate složene objekte podmjesta, polja ili hijerarhijske podatke ili poslovanja vaše strukture podataka tijekom vremena, JSON OSNOVNI oblik omogućuje vam da se odnositi na bilo koju strukturu složenih podataka.

JSON tekstnih oblik koji je stvorio može poslužiti kao neku drugu vrstu niza u bazi podataka SQL Azure. Možete poslati ili spremiti podatke JSON kao standardni NVARCHAR:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

JSON podaci koji se koriste u ovom primjeru predstavlja vrstu NVARCHAR(MAX). JSON možete umetnuti u ovoj su tablici ili prikazuje se kao argument pohranjena procedura pomoću standardnih Transact-SQL sintaksa kao što je prikazano u sljedećem primjeru:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Bilo koji jezik klijentsko ili biblioteku koja funkcionira s nizom podataka u bazi podataka SQL Azure funkcionirat će i s JSON podataka. JSON može se spremiti u svakoj tablici koji podržava vrsti NVARCHAR, kao što su tablice optimiziran za memorije ili sustava određuju tablice. JSON predstavljanje sve ograničenja kodom na strani klijenta ili u sloju baze podataka.

## <a name="querying-json-data"></a>Slanje upita za podatke JSON

Ako imate podatke koji su oblikovani kao JSON spremljeni u tablicama Azure SQL, JSON funkcije omogućuju korištenje tih podataka u bilo kojem SQL upita.

Funkcija JSON koje su dostupne u omogućuju baze podataka Azure SQL tretirati podatke koji su oblikovani kao JSON kao drugom vrstom podataka SQL. Jednostavno možete izdvojiti vrijednosti iz teksta JSON i korištenje JSON podataka u bilo koji upit:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

Funkcija JSON_VALUE izdvaja vrijednost iz teksta JSON pohranjene u stupcu podataka. Ova funkcija koristi puta JavaScript nalik referentni vrijednost u tekst JSON za izdvajanje. Vrijednost izdvojene može se koristiti u bilo kojem dijelu SQL upita.

JSON_QUERY funkcija je slična JSON_VALUE. Za razliku od JSON_VALUE, ova funkcija izdvaja složene podređeni objekt kao što su polja ili objekte koji su smještene u JSON tekst.

Funkcija JSON_MODIFY omogućuje vam da odredite put vrijednost u tekst JSON koji treba ažurirati, kao i novu vrijednost koja će prebrisati staru. Na taj način možete jednostavno ažurirati JSON tekst bez reparsing cijelu strukturu.

Budući da je JSON pohranjena u standardni tekst, postoje bez jamstva su pravilno oblikovani vrijednostima spremljenima u stupaca teksta. Možete provjeriti je li tekst pohranjen u stupcu JSON ispravno oblikovana pomoću standardnih baze podataka SQL Azure check ograničenja i funkciju ISJSON:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Ako unos teksta ispravno oblikovana JSON, funkcija ISJSON vraća vrijednost 1. Na svaku umetanje ili ažuriranje stupca JSON ovo ograničenje će provjerite je li novu vrijednost tekst nije oblikovan JSON.

## <a name="transforming-json-into-tabular-format"></a>Pretvorba JSON u tabličnom obliku

Baze podataka SQL Azure omogućuje vam pretvaranje JSON zbirke u tabličnom obliku i učitavanje ili upit JSON podatke.

OPENJSON je funkcija vrijednost tablice koja se raščlanjuje JSON tekst, pronalazi polja JSON objekata, zadanu ponovno izračunava po elementima polja i vraća jedan redak u rezultatu izlaz za svaki element polja.

![Tablični JSON](./media/sql-database-json-features/image_2.png)

U gornjem primjeru smo možete odrediti gdje da biste pronašli JSON polja koje se otvarati (u $. Put narudžbe), koji stupci bi trebala biti vraćena kao rezultat te gdje možete pronaći JSON vrijednosti koje će se vratiti kao ćelije.

Ne možemo mogu transformirati JSON polja u na @orders varijabla u skup redaka, analizirati skup rezultata ili umetanje redaka u standardni tablicu:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
Skup narudžbe oblikovan kao JSON polja i navedene kao parametar pohranjena procedura mogu raščlaniti i umetnuta u tablici Narudžbe.

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali kako integrirati JSON u aplikaciji, pogledajte sljedeće resurse:

- [Članak na blogu](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
- [Dokumentacija MSDN](https://msdn.microsoft.com/library/dn921897.aspx)
- [Kanal 9 videozapisa](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

Da biste saznali više o raznim scenarija za integraciju JSON u aplikaciji, pogledajte pokazni programi u [kanalu 9 videozapisa](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) ili pronaći scenarij koji odgovara slučaj koristi u [JSON bloga](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).
