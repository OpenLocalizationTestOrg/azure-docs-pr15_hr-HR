<properties 
   pageTitle="Azure SQL baza podataka Savjetnik pomoću portala za Azure | Microsoft Azure" 
   description="Koristite Savjetnik za baze podataka SQL Azure na portalu za Azure da pregledavaju i implementirati preporuke za postojećih SQL baza podataka koje možete poboljšati performanse trenutni upit." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="sql-database-advisor-using-the-azure-portal"></a>SQL Savjetnik za baze podataka pomoću portala za Azure

> [AZURE.SELECTOR]
- [Pregled Savjetnik za baze podataka za SQL](sql-database-advisor.md)
- [Portal](sql-database-advisor-portal.md)

Koristite Savjetnik za baze podataka SQL Azure na portalu za Azure da pregledavaju i implementirati preporuke za postojećih SQL baza podataka koje možete poboljšati performanse trenutni upit.

## <a name="viewing-recommendations"></a>Preporuke za prikaz

Preporuke stranica je koju pregledavate gornji preporuke njihov utjecaj na temelju da biste poboljšali performanse. Možete pogledati i status povijesne operacije. Odaberite preporuke ili status da biste vidjeli dodatne detalje.

Da biste pogledali i Primjena preporuke, potrebne su vam dozvole točan [Kontrola pristupa na temelju uloga](../active-directory/role-based-access-control-configure.md) u Azure. **Čitač** **SQL DB suradničke** dozvole su potrebne za preporuke za prikaz i **vlasnika**, **SQL DB suradničke** dozvole potrebne za izvršavanje akcije; Stvaranje i ispustite indekse i poništavanje stvaranje indeksa.

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Kliknite **Dodatne usluge** > **baze podataka SQL**, a zatim odaberite bazu podataka.
5. Kliknite **performanse preporuke** da biste vidjeli dostupne preporuke za odabrane baze podataka.

> [AZURE.NOTE] Da biste dobili preporuke baze podataka mora imati o dan korištenje, a mora postojati nekoliko aktivnosti. Također mora postojati nekoliko dosljedan aktivnosti. Savjetnik za baze podataka SQL jednostavnije možete optimizirati za uzoraka dosljedan upit ne može se za slučajni spotty bursts aktivnosti. Ako se preporuke nisu dostupne, stranici **performanse preporuke** mora sadržavati poruci na kojemu se objašnjava zašto.

![Preporuke](./media/sql-database-advisor-portal/recommendations.png)

Evo primjera "Riješiti problem sheme" preporuke na portalu za Azure.

![Rješavanje problema sheme](./media/sql-database-advisor-portal/sql-database-advisor-schema-issue.png)

Preporuke su sortirani prema njihov utjecaj na performanse u sljedeće četiri kategorije:

| Utjecaj | Opis |
| :--- | :--- |
| Visoke | Preporuke visoke utjecaj mora sadržavati najčešće značajan učinak na performanse. |
| Srednje | Srednje utjecaj preporuke trebali biste poboljšali performanse, ali ne značajno. |
| Niska | Niska utjecaj preporuke mora sadržavati bolje performanse od bez, ali poboljšanja možda neće biti vrlo. 


### <a name="removing-recommendations-from-the-list"></a>Uklanjanje preporuke s popisa

Ako na popisu preporuke sadrži stavku koju želite ukloniti s popisa, možete odbaciti na preporuka:

1. Odaberite preporuku na popisu **preporuke**.
2. Kliknite **Odbaci** plohu **pojedinosti** .


Po želji možete dodati odbačene stavke popis za **preporuke** :

1. Na plohu **preporuke** kliknite **Prikaz odbacuju**.
1. Odabir odbačene stavke s popisa da biste vidjeli njegove detalje.
1. Ako želite, kliknite **Poništi Odbaci** da biste dodali indeks vratili na glavni popis **preporuke**.



## <a name="applying-recommendations"></a>Primjena preporuke

Savjetnik za baze podataka SQL omogućuje potpunu kontrolu nad po čemu se preporuke omogućena na bilo koji od tri sljedeće mogućnosti: 

- Primjena pojedinačnih preporuke jedan po jedan.
- Omogućivanje Savjetnik za Automatska Primjena preporuke (trenutno primjenjuje se na samo preporuke indeks).
- Da biste ručno implementirati preporuku pokrenuti preporučene T SQL skripta cjelokupnom bazu podataka.

Odaberite preporuke za prikaz detalja, a zatim kliknite **Prikaz skripte** za pregledajte pojedinosti točno kako se stvara u preporuke.

Baza podataka ostane online dok se primjenjuje se na Savjetnik za preporuke – korištenju savjetnika za baze podataka SQL nikad ne vodi baze podataka izvan mreže.

### <a name="apply-an-individual-recommendation"></a>Primjena pojedinačnih preporuka

Možete pregledavati i prihvatiti preporuke jedan po jedan.

1. Na plohu **preporuke** kliknite preporuku.
2. **Detalje** o plohu kliknite **Primijeni**.

    ![Primjena preporuka](./media/sql-database-advisor-portal/apply.png)

### <a name="enable-automatic-index-management"></a>Omogući upravljanje automatsko indeksa

Možete postaviti Savjetnik za baze podataka SQL automatski implementaciju preporuke. Kao što je preporuke postaju dostupne su automatski primijenit će se. Kao sve operacije u indeks upravlja servis ako je negativan učinak na performanse u preporuke vratit će se stanje.

1. Na plohu **preporuke** kliknite **Automate**:

    ![Postavke savjetnika](./media/sql-database-advisor-portal/settings.png)

2. Postavite program advisor automatski **Stvori** ili **neposredne** indekse:

    ![Preporučuje se indeksi](./media/sql-database-advisor-portal/automation.png)


### <a name="manually-run-the-recommended-t-sql-script"></a>Ručno pokretanje preporučene T SQL skripta

Odaberite preporuke, a zatim kliknite **Prikaz skripte**. Pokrenuli ovu skriptu bazu podataka da biste ručno primijenili na preporuke.

*Indeksi koji se izvode ručno ne nadzirati i provjeriti za utjecaj na performanse servis* da bi se predlaže praćenje tih indeksa nakon stvaranja da biste provjerili oni omogućuju performansi i prilagoditi ili ih izbrisati ako je potrebno. Detalje o stvaranju indeksi potražite u članku [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).


### <a name="canceling-recommendations"></a>Otkazivanje preporuke

Preporuke koji su na **čekanju**, **Provjera valjanosti**i **uspjeh** stanja mogu se otkazati. Preporuke čiji je status **Executing** nije moguće poništiti.

1. Odaberite i preporuke u području **Usklađivanje povijest** da biste otvorili plohu **preporuke pojedinosti** .
2. Kliknite **Odustani** da biste prekinuli postupak primjene na preporuke.



## <a name="monitoring-operations"></a>Nadzor operacije

Primjena preporuku možda po dogoditi. Na portalu sadrži detalje o stanju operacije preporuke. Slijede mogućih stanja indeksa može biti u:

| Status | Opis |
| :--- | :--- |
| Na čekanju | Primjena preporuke naredba zaprimljeni i zakazana za izvršavanje. |
| Izvođenje | Primjenjuje se na preporuke. |
| Uspjeh | Preporuka uspješno je primijenjena. |
| Pogreška | Pojavila se pogreška tijekom primjene na preporuke. To može biti tranzitne problem ili vjerojatno shemu pretvorite tablicu i skripte više nije valjana. |
| Vraćanje | Za preporuke je primijenjena, ali je smatra koje nisu performant a je koji se automatski vratit stanje. |
| Vratit stanje | Vratit je stanje na preporuke. |

Kliknite je u tijeku preporuke s popisa da biste vidjeli dodatne detalje:

![Preporučuje se indeksi](./media/sql-database-advisor-portal/operations.png)


### <a name="reverting-a-recommendation"></a>Vraćanje i preporuke

Ako ste koristili u Savjetnik za primjenu na preporuke (što znači da ne uspijete ručno pokrenuti skriptu T SQL) je automatski vratit će ga ako pronađe učinak na performanse će negativno. Ako iz bilo kojeg razloga jednostavno želite vratiti preporuku možete učiniti sljedeće:


1. Odaberite uspješno primijenjena preporuke u području **Tuning povijest** .
2. Kliknite **Vrati** plohu **preporuke pojedinosti** .

![Preporučuje se indeksi](./media/sql-database-advisor-portal/details.png)


## <a name="monitoring-performance-impact-of-index-recommendations"></a>Praćenje performansi posljedice preporuke indeksa

Kada uspješno primjenjuju preporuke (trenutno indeksirati operacije i parameterize samo preporuke za upite) možete kliknuti **Uvida upita** na plohu pojedinosti preporuke za otvaranje [Uvida performanse upita](sql-database-query-performance.md) i prikaz posljedice performanse Najčešći upiti.

![Utjecaj na performanse monitora](./media/sql-database-advisor-portal/query-insights.png)



## <a name="summary"></a>Sažetak

Savjetnik za baze podataka SQL nudi preporuke za poboljšanje performansi SQL baze podataka. Unosom T SQL skripte te osobe i potpuno – automatsko (trenutno indeks samo) na Savjetnik omogućuje korisne pomoć u optimiziranje bazu podataka i konačni unaprjeđenje upita.



## <a name="next-steps"></a>Daljnji koraci

Praćenje vaše preporuke i nastaviti da biste primijenili ih suzite performansi. Baza podataka radnih opterećenja dinamične i promijenite kontinuirano. Savjetnik za baze podataka SQL će i dalje i praćenje pružaju preporuke kojom ćete unaprijediti performanse sustava baze podataka. 

 - Pregled Savjetnik baze podataka za SQL potražite u članku [Savjetnik za baze podataka SQL](sql-database-advisor.md) .
 - U odjeljku [Uvida performanse upita](sql-database-query-performance.md) da biste saznali više o performansama utjecaj Najčešći upiti.

## <a name="additional-resources"></a>Dodatni resursi

- [Spremište upita](https://msdn.microsoft.com/library/dn817826.aspx)
- [STVARANJE INDEKSA](https://msdn.microsoft.com/library/ms188783.aspx)
- [Kontrola pristupa na temelju uloga](../active-directory/role-based-access-control-configure.md)






