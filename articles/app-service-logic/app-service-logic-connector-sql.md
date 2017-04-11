<properties
   pageTitle="Pomoću poveznika SQL u aplikacijama logike | Aplikacije servisa za Microsoft Azure"
   description="Kako stvoriti i konfigurirati aplikaciju SQL poveznika ili API-JA i koristiti u aplikaciji logike u aplikacije servisa za Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# <a name="get-started-with-the-microsoft-sql-connector-and-add-it-to-your-logic-app"></a>Upoznavanje s programom Microsoft SQL Connector i njezino dodavanje logike aplikacije
>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2014. – 12-01 – pregled shema verziju. Verzija sheme Azure SQL 2015-08-01 – pretpregled kliknite [SQL Azure API -JA](../connectors/connectors-create-api-sqlazure.md).

Povežite se s lokalnog sustava SQL Server ili baze podataka SQL Azure stvarati i mijenjati podatke ili podatke. Poveznika može se koristiti u aplikacijama logike dohvatiti procesa ili slanje podataka u sklopu "tijek rada". Kada koristite SQL poveznika u tijeku rada, možete postići različite scenarije. Na primjer, možete:

- Izlaganje područja podataka residing u SQL baze podataka pomoću weba ili mobilnu aplikaciju.
- Umetnite podatke u tablici baze podataka SQL za pohranu. Ako, na primjer, možete unijeti zapise zaposlenika, ažuriranje prodajne naloge i tako dalje.
- Dohvaćanje podataka iz SQL te ga koristiti u poslovni proces. Na primjer, možete dobiti zapise kupaca i staviti zapise kupaca u SalesForce.

Poveznik za SQL možete dodati tijek rada i proces podataka tvrtke kao dio ovaj tijek rada u aplikaciji za logiku. 

## <a name="triggers-and-actions"></a>Okidača i akcija
*Okidača* su događajima koji se odvijaju. Na primjer, kada se ažurira reda ili novog klijenta dodaje. *Akcija* je rezultat okidača. Na primjer, kada se ažurira reda, slanje upozorenja prodavača. Ili prilikom dodavanja novog klijenta, slanje dobrodošlice e-pošte novog klijenta.

Poveznik za SQL može se koristiti kao okidač ili akcija u logiku aplikacije i podržava podataka u JSON i XML formatima. Za svaku tablicu uključeni u vašem paketu postavke (više o tome u nastavku ovog članka) postoji skup akcija JSON i skup akcija putem XML.

Poveznik za SQL sadrži sljedeće okidača i dostupne akcije:

Okidača | Akcija
--- | ---
Ankete podataka | <ul><li>Umetanje tablice</li><li>Ažuriranje tablice</li><li>Odaberite iz tablice</li><li>Brisanje iz tablice</li><li>Pozivanje pohranjena procedura</li></ul>

## <a name="create-the-sql-connector"></a>Stvaranje poveznik za SQL

Poveznik mogu stvoriti iz logike aplikacije ili stvoriti izravno iz trgovine Azure Marketplace. Da biste stvorili poveznik iz trgovine:  

1. Azure startboard odaberite **trgovine**.
2. Traženje "SQL poveznik", odaberite ga i odaberite **Stvori**.
3. Unesite ime, Plan za aplikaciju servisa i druga svojstva.
4. Unesite sljedeće postavke paketa:

    Ime | Obavezno |  Opis
--- | --- | ---
Naziv poslužitelja | Da | Unesite naziv za SQL Server. Na primjer, unesite *SQLserver/sqlexpress* ili *SQLserver.mydomain.com*.
Priključak | ne | Zadana vrijednost je 1433.
Korisničko ime | Da | Unesite korisničko ime koje se možete prijaviti u SQL Server. Ako se povezujete s poslužiteljem SQL lokalnog, unesite vjerodajnice za provjeru autentičnosti SQL.
Lozinke | Da | Unesite korisničko ime i lozinku.
Naziv baze podataka | Da | Unesite baze podataka se povezujete. Ako, na primjer, možete unijeti *korisnike* ili *vlasnika baze podataka i narudžbe*.
Lokalni | Da | Zadana vrijednost je False. Unesite False ako povezivanje s bazom podataka Azure SQL. Unesite vrijednost True ako se povezujete s poslužiteljem SQL lokalnog.
Niz za povezivanje servisa Bus | ne | Ako se povezujete s lokalnim, unesite niz za povezivanje Bus usluge preusmjeravanja.<br/><br/>[Pomoću Upravitelja veze hibridnog](app-service-logic-hybrid-connection-manager.md)<br/>[Bus cijene za servis](https://azure.microsoft.com/pricing/details/service-bus/)
Naziv poslužitelja partnera | ne | Ako se primarni poslužitelj nije dostupan, možete unijeti poslužitelju partnera kao zamjenski ili sigurnosne kopije poslužitelja.
Tablica | ne | Popis tablica baze podataka koja se može ažurirati poveznik. Na primjer, unesite *OrdersTable* ili *EmployeeTable*. Ako nema tablica se unose, može se koristiti sve tablice. Valjani tablica i/ili pohranjene procedure moraju koristiti ovaj poveznik kao akciju.
Pohranjene procedure | ne | Unesite postojeću pohranjena procedura koje možete pozvati tako da poveznik. Na primjer, unesite *sp_IsEmployeeEligible* ili *sp_CalculateOrderDiscount*. Valjani tablica i/ili pohranjene procedure moraju koristiti ovaj poveznik kao akciju.
Dostupne upit podataka | Za podršku za pokretanje | SQL naredbu da biste utvrdili je li sve podatke za provjere bazu podataka sustava SQL Server. To mora vratiti numeričku vrijednost koja predstavlja broj redaka podataka dostupna. Primjer: Odaberite COUNT(*) table_name.
Ankete podatkovni upit | Za podršku za pokretanje | SQL naredbe za tablicu baze podataka sustava SQL Server. Možete unijeti bilo koji broj SQL naredbe razdvojene točkom sa zarezom. Izjavu o zaštiti se izvršava putem transakcije i samo izvršenom kada u svojoj aplikaciji logike siguran način pohrane podataka. Primjer: Odaberite *table_name; Brisanje iz table_name. <br/>**Note**<br/>Morate navesti ankete izjava kojim se izbjegava beskonačno Ponavljaj brisanjem, premještanje ili ažuriranje odabranih podataka da biste bili sigurni nije ponovno polled iste podatke.

5. Po dovršetku postavke paketa izgleda otprilike ovako:  
![][1]  

6. Odaberite **Stvori**. 


## <a name="use-the-connector-as-a-trigger"></a>Poveznik upotrijebili okidača
Pogledajmo jednostavne logike aplikacija na ankete podataka iz tablice SQL dodaje podatke u drugoj tablici i ažuriranja podataka.

Da biste koristili poveznik za SQL okidač, unesite vrijednosti **Podataka dostupna upita** i **Ankete podatkovni upit** . **Podatkovni dostupna upit** se izvršava u rasporedu unesite i određuje podataka dostupna. Budući da se ovaj upit vraća samo skalarna broj, možete se počeo gledati i optimizirana Česti izvođenja.

**Ankete podatkovni upit** izvršava se samo kada upit podataka dostupna označava podataka dostupna. Izjavu o zaštiti izvršava unutar transakcije i samo izvršenom kada izdvojene podataka durably pohranjene u tijeku rada. Nije važno da biste izbjegli neizmjerno ponovno izdvajanje iste podatke. Da biste izbrisali ili ažurirajte podatke da biste bili sigurni da je nije koji se prikupljaju se sljedeći put je mu podataka mogu se transakcijskih prirode ovaj izvršavanja.

> [AZURE.NOTE] Shema vratio izjavu o zaštiti označava dostupnih svojstava u vašem poveznik. Naziv mora biti sve stupce.

#### <a name="data-available-query-example"></a>Primjer podataka dostupna upita

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### <a name="poll-data-query-example"></a>Primjer upita za podatke ankete

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### <a name="add-the-trigger"></a>Dodavanje okidača
1. Pri stvaranju ili uređivanju logike aplikacije, odaberite SQL poveznik koji ste stvorili kao okidača. Taj popis dostupnih okidača: **Ankete podataka (JSON)** i **Ankete podatke (XML)**:  
![][5]

2. Odaberite okidača **Ankete podataka (JSON)** , unesite željenu učestalost pa kliknite u ✓:  
![][6]

3. Okidača knjiga prikazuje se kao što je konfiguriran u aplikaciji logike. Output(s) okidača prikazani su, a mogu se koristiti kao unose u sljedeće radnje:  
![][7]

## <a name="use-the-connector-as-an-action"></a>Poveznik upotrijebili akcije
Korištenje naše jednostavne logike aplikacije scenarij u kojem se ankete podatke iz tablice SQL dodaje podatke u drugoj tablici, a ažurira podatke.

Da biste koristili poveznik za SQL akciju, unesite naziv tablice i/ili pohranjene procedure koje ste unijeli prilikom stvaranja poveznik za SQL:

1. Nakon vaše okidača (ili odaberite "ručno pokrenite ovaj logike"), dodajte SQL poveznik koji ste stvorili u galeriji. Odaberite neku od akcija Umetanje, kao što je *Umetnuli u TempEmployeeDetails (JSON)*:  
![][8]

2. Unesite ulazne vrijednosti zapis koji želite umetnuti, a zatim kliknite na u ✓:  
![][9]

3. U galeriji odaberite isti SQL poveznik koji ste stvorili. Kao akciju, odaberite akcije ažuriranja na istoj tablici, kao što su *EmployeeDetails ažuriranja*:  
![][11]

4. Unesite ulazne vrijednosti za akcije ažuriranja, a na na ✓ kliknite:  
![][12]

Možete testirati aplikaciju logike dodavanjem novi zapis u tablici koja se prečesto.

## <a name="what-you-can-and-cannot-do"></a>Što možete, odnosno ne možete raditi

SQL upit | Podržana | Nije podržano
--- | --- | ---
Gdje uvjet | <ul><li>Operatori: I, ili, = <> <, < =, >, > = i sviđa mi se</li><li>Moguće kombinirati više uvjeta sub "(" i")"</li><li>Niz literala, datuma i vremena (u jednostrukim navodnicima), brojeva (mora sadržavati samo znamenke)</li><li>Isključivo treba u obliku binarne izraz, poput ((operand operator operand) i/ili (operand operator za operand)) *</li></ul> | <ul><li>Operatori: Između IN</li><li>Sve ugrađene funkcije kao što su ADD(), MAX() NOW(), POWER() i tako dalje</li><li>Kao što su matematičke operatore *, -, + i tako dalje</li><li>Niz concatenations pomoću +.</li><li>Sve spojevi</li><li>JE NULL i nije Null</li><li>Svi brojevi s numerička znakove, poput heksadecimalni brojeva</li></ul>
Polja (u upit s izdvajanjem) | <ul><li>Nazivi valjani stupac odvojenih točkama sa zarezom. Nema prefiksi naziv tablice dopušteno (poveznik funkcionira na jednu tablicu odjednom).</li><li>Nazivi možete unijeti prespojni znak s ' ["i"] "</li></ul> | <ul><li>Ključne riječi kao što su VRH, DISTINCT i tako dalje</li><li>Pseudonimu, kao što su ulica + grad + poštanski broj kao adresu</li><li>Sve ugrađene funkcije, kao što su ADD(), MAX() NOW(), POWER() i tako dalje</li><li>Kao što su matematičke operatore *, -, + i tako dalje</li><li>Niz concatenations pomoću +</li></ul>

#### <a name="tips"></a>Savjeti

- Za naprednih upita, predlažemo da stvaranje pohranjena procedura i izvršavanje pomoću izvršavanje pohranjene procedure API-JA.
- Kada se koristi unutarnji upita pomoću njih unutar pohranjene procedure.
- Za uključivanje više uvjeta, poslužite se "I" i "Ili" operatore.

## <a name="hybrid-configuration-optional"></a>Hibridna konfiguracija (nije obavezno)

> [AZURE.NOTE] Ovaj korak potreban je samo ako koristite SQL Server lokalnog iza vatrozida.

Pomoću upravitelja konfiguracije hibridnog aplikacije servisa za sigurno povezivanje s lokalnim sustavom. Ako ste poveznik koristi se lokalnog sustava SQL Server, potreban je Upravitelja veze hibridnog.

> [AZURE.NOTE] Ako želite započeti s Azure logike aplikacije prije registracije za račun za Azure, idite na [Pokušajte logike aplikacije](https://tryappservice.azure.com/?appservice=logic), gdje možete odmah stvorite short-lived starter logike aplikaciju u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

Pogledajte odjeljak [Korištenje upravitelja veze hibridnog](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Bolje iskorištavanje vaše poveznika
Sad kad je poveznik stvorili, možete ga dodati u tijeku rada poslovne logike aplikacija. U odjeljku [koje su aplikacije logike?](app-service-logic-what-are-logic-apps.md).

Prikaz reference Swagger REST API-JA na [poveznika i API aplikacije Reference](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Možete pregledati i performanse Statistika i kontrola sigurnost da biste poveznik. Potražite u članku [Upravljanje i nadzirati ugrađene aplikacije API -JA i poveznike](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png
