### <a name="prerequisites"></a>Preduvjeti
- Račun Azure; možete stvoriti [pomoću računa](https://azure.microsoft.com/free)
- Bazom [Baze podataka SQL Azure](../articles/sql-database/sql-database-get-started.md) s njegove podatke za povezivanje, uključujući naziv poslužitelja, naziv baze podataka i korisničkog imena i lozinke. Ove informacije uključen u nizu za povezivanje s bazom podataka SQL:
  
    Poslužitelj = tcp:*yoursqlservername*. database.windows.net,1433;Initial kataloga =*yourqldbname*; Informacije o sigurnosti i dalje pojavljuje = False; Korisnički ID = {your_username}; Lozinka = {your_password}; MultipleActiveResultSets = False; Šifriranje = True; Certifikatpouzdanogposlužitelja = False; Vremensko ograničenje veze = 30;

    Dodatne informacije o [Baze podataka SQL Azure](https://azure.microsoft.com/services/sql-database).

> [AZURE.NOTE] Kada stvorite baze podataka SQL Azure, možete stvoriti i dio sustava SQL ogledne baze podataka. 



Prije korištenja baze podataka SQL Azure u aplikaciji logike, povezivanje s bazom podataka sustava SQL. To možete jednostavno unutar aplikacije logike portala za Azure.  

Povezivanje s baze podataka SQL Azure na sljedeći način:  

1. Stvaranje logike aplikacije. U dizajneru logike aplikacije dodajte okidač, a zatim dodajte akciju. Odaberite **Prikaži Microsoft upravljani API-ji** padajućem popisu, a zatim u okvir za pretraživanje unesite "sql". Odaberite jednu od akcija:  

    ![Korak SQL Azure veze](./media/connectors-create-api-sqlazure/sql-actions.png)

2. Ako već niste stvorili sve veze s bazom podataka SQL, od vas će se traži pojedinosti veze:  

    ![Korak SQL Azure veze](./media/connectors-create-api-sqlazure/connection-details.png) 

3. Unesite detalje SQL baze podataka. Potrebni su svojstva zvjezdicom.

    | Svojstvo | Pojedinosti |
|---|---|
| Povezivanje putem pristupnika | Ostavite poništen. Koristi se prilikom povezivanja s poslužiteljem SQL lokalnog. |
| Naziv veze * | Unesite naziv neke za vezu. | 
| Naziv poslužitelja SQL * | Unesite naziv poslužitelja što je nešto poput *servername.database.windows.net*. Naziv poslužitelja je prikazana u svojstva baze podataka SQL Azure portal i također prikazati u nizu za povezivanje. | 
| Naziv baze podataka SQL * | Unesite naziv baze podataka sustava SQL. To je naveden u svojstvima SQL baze podataka u nizu za povezivanje: početnog kataloga =*yoursqldbname*. | 
| Korisničko ime * | Unesite korisničko ime koje ste stvorili prilikom stvaranja baze podataka SQL. Naveden je u svojstva baze podataka SQL Azure portalu. | 
| Lozinka * | Unesite lozinku koju ste stvorili prilikom stvaranja baze podataka SQL. | 

    Da biste autorizirali logike aplikacije za povezivanje i pristup vašim podacima SQL se koristi te vjerodajnice. Kada završi, pojedinosti veze izgleda otprilike ovako:  

    ![Korak SQL Azure veze](./media/connectors-create-api-sqlazure/sample-connection.png) 

4. Odaberite **Stvori**. 

5. Obratite pozornost na to je stvorena veza. Sada, nastavite s koracima u svojoj aplikaciji logika: 

    ![Korak SQL Azure veze](./media/connectors-create-api-sqlazure/table.png)