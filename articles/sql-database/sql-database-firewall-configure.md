<properties
   pageTitle="Konfiguriranje s pregledom Vatrozid za SQL server | Microsoft Azure"
   description="Saznajte kako konfigurirati Vatrozid za baze podataka SQL poslužitelja baze podataka – razini i vatrozid pravila za upravljanje pristupom."
   keywords="Vatrozid za baze podataka"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor="cgronlun"
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/14/2016"
   ms.author="rickbyh"/>

# <a name="configure-azure-sql-database-firewall-rules---overview"></a>Konfigurirati pravila vatrozida baze podataka SQL Azure \- pregled


> [AZURE.SELECTOR]
- [Pregled](sql-database-firewall-configure.md)
- [Portal za Azure](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API-JA](sql-database-configure-firewall-settings-rest.md)


Baza podataka Microsoft Azure SQL omogućuje uslugu relacijske baze podataka za Azure i druge aplikacije utemeljene na Internetu. Da biste zaštitili podatke, vatrozida onemogućuju sve pristup s poslužiteljem baze podataka dok ne odredite na kojim računalima imati dozvolu. Vatrozid dopušta pristup baze podataka koji se temelje na izvornom IP adrese svaki zahtjev.

Da biste konfigurirali vatrozida, stvorite pravila vatrozida koji odredite raspon prihvatljiva IP adrese. Na razinama poslužitelj i bazu podataka možete stvoriti pravila vatrozida.

- **Pravila vatrozida razini poslužitelja:** Ta pravila omogućiti klijenti pristupili cijelu Azure SQL server, odnosno sve baze podataka unutar iste logičke poslužitelja. Ta pravila spremaju se u **glavnom** bazom podataka. Moguće je konfigurirati pravila vatrozida razini poslužitelja pomoću portala za ili pomoću Transact-SQL naredbe.
- **Pravila vatrozida razinom baze podataka:** Ta pravila omogućiti klijenti pristupili pojedinačne baze podataka unutar vaš poslužitelj baze podataka SQL Azure. Možete stvoriti ta pravila za svaku bazu podataka i one spremaju se u pojedinačne baze podataka. (Pravila vatrozida razinom baze podataka za **glavnom** bazom podataka možete stvoriti.) Ta pravila može biti korisno u ograničavanje pristupa određene (Sigurnost) baza podataka unutar iste logičke poslužitelja. Pravila vatrozida razinom baze podataka može se konfigurirati pomoću Transact-SQL naredbe.

**Preporuka:** Microsoft preporučuje korištenje pravila vatrozida baze podataka razinom kad god je moguće za poboljšanje sigurnosti i pripreme bazu podataka. Korištenje pravila vatrozida razini poslužitelja za administratore i kada imate mnogo baze podataka koje imaju isti zahtjeve za pristup i ne želite trošiti vrijeme pojedinačno konfiguriranje svaku bazu podataka.


## <a name="firewall-overview"></a>Pregled vatrozid

Početku Transact-SQL pristupa s poslužiteljem Azure SQL blokirao je vatrozida. Da biste počeli koristiti poslužitelj za Azure SQL, idite na portal za Azure i navedite jednu ili više pravila vatrozida razini poslužitelja koji omogućuju pristup s poslužiteljem Azure SQL. Da biste odredili koji rasponi IP adresa s Interneta dopušteno i li Azure aplikacije možete pokušati povezati s poslužiteljem Azure SQL pomoću pravila vatrozida.

Selektivno dopustiti pristup samo jedan od baza podataka u poslužiteljem Azure SQL, stvarate pravilo razinom baze podataka potreban baze podataka. Odredite rasponu IP adresa za pravila vatrozida baze podataka koji se nalazi izvan raspona adresa IP koji je naveden u pravilu razini poslužitelja vatrozid i bili sigurni da se IP adresa klijenta pada u rasponu navedenom u pravilu razinom baze podataka.

Prilikom pokušaja povezivanja s Internetom i Azure morate najprije proći kroz vatrozid prije nego što dođu Azure SQL server ili SQL baze podataka, kao što je prikazano na sljedećem su dijagramu.

   ![Dijagram s opisom konfiguraciju vatrozida.][1]

## <a name="connecting-from-the-internet"></a>Povezivanje s Internetom

Kada računalo pokuša povezati s poslužiteljem baze podataka s Interneta, vatrozid prvo provjerava izvorišne IP adresu na zahtjev na potpunog skupa pravila vatrozida:

- Ako je IP adresa zahtjeva unutar nekog raspona koji je naveden u pravila vatrozida razini poslužitelja, moguć je veza s poslužiteljem baze podataka SQL Azure.

- Ako je IP adresa zahtjeva nije unutar nekog raspona koji je naveden u pravilu vatrozid razini poslužitelja, provjeravaju se pravila vatrozida razinom baze podataka. Ako je IP adresa zahtjeva unutar nekog raspona koji je naveden u pravila vatrozida razinom baze podataka, veza moguć je samo na bazu podataka koja sadrži pravilo podudaranja razinom baze podataka.

- Ako je IP adresa zahtjeva nije unutar raspona koji je naveden u bilo kojem od pravila vatrozida razini poslužitelja ili razinom baze podataka, ne uspije zahtjev za povezivanje.

> [AZURE.NOTE] Da biste pristupili baze podataka SQL Azure s lokalnog računala, provjerite je li vatrozid na vašoj mreži i lokalnom računalu omogućuje izlaznu komunikaciju na TCP priključak 1433.


## <a name="connecting-from-azure"></a>Povezivanje s Azure

Kada aplikacije Azure pokuša povezati s poslužiteljem baze podataka, vatrozida provjerava je li dopušten Azure veze. Postavljanje s početnog i završnog adresu jednako 0.0.0.0 vatrozid označava dopušteno ove veze. Ako pokušaj povezivanja nije dopuštena, zahtjev dosegne poslužitelj baze podataka SQL Azure.

> [AZURE.IMPORTANT] Ta mogućnost konfigurira vatrozid da biste omogućili sve veze iz Azure obuhvaća veze iz pretplate ostali korisnici. Kada odaberete tu mogućnost, provjerite je li prijava i korisničke dozvole ograničiti pristup samo autoriziranih korisnika.

Možete omogućiti veze iz Azure na dva načina:

- [Portal Microsoft Azure](https://portal.azure.com/)potvrdite okvir **Dopusti azure servisa za pristup poslužitelju** prilikom stvaranja novog poslužitelja.

- [Klasični portal](http://go.microsoft.com/fwlink/p/?LinkID=161793)na kartici **Konfiguriraj** na poslužitelju, u odjeljku **Dopuštene usluge** za **Microsoftove servise Azure**kliknite **da** .

## <a name="creating-the-first-server-level-firewall-rule"></a>Stvaranje prvog pravila vatrozida razini poslužitelja

Prvi postavke vatrozida razini poslužitelja možete stvoriti pomoću [portala za Azure](https://portal.azure.com/) ili programski pomoću REST API-JA ili Azure PowerShell. Pravila koja slijedi vatrozida razini poslužitelja mogu stvoriti i upravlja pomoću ovih metoda, kao i kao kroz Transact-SQL. Da biste poboljšali performanse, pravila vatrozida razini poslužitelja predmemoriraju privremeno na razini baze podataka. Da biste osvježili predmemorije, potražite u članku [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). Dodatne informacije o pravila vatrozida razini poslužitelja potražite u članku [Kako: Konfiguriranje programa Vatrozid Azure SQL server pomoću portala za Azure](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Stvaranje pravila vatrozida razinom baze podataka

Nakon što ste konfigurirali prvi vatrozid razini poslužitelja, preporučujemo vam da biste ograničili pristup određenim bazama podataka. Ako navedete raspon IP adresa u pravilu vatrozid razinom baze podataka koji se nalazi izvan raspona koji je naveden u pravilu razini poslužitelja vatrozid, samo klijenti koji imaju IP adrese u rasponu razinom baze podataka možete pristupiti bazu podataka. Može sadržavati najviše od pravila vatrozida 128 razinom baze podataka za baze podataka. Pravila vatrozida razinom baze podataka za matrice i korisnik baze podataka možete stvoriti i upravlja Transact-SQL. Dodatne informacije o konfiguriranju pravila vatrozida razinom baze podataka potražite u članku [sp_set_database_firewall_rule (baze podataka SQL Azure)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Programsko upravljanje pravila vatrozida

Uz portal za Azure pravila vatrozida može upravljati programski pomoću Transact-SQL, REST API-JA i Azure PowerShell. Tablice u nastavku opisuju skupa naredbi koje su dostupne za svaki način.


### <a name="transact-sql"></a>SQL transakcija

| Prikaz katalog ili pohranjena procedura                                                           | Razina     | Opis                                          |
|--------------------------------------------------------------------------------------------|-----------|------------------------------------------------------|
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx)                   | Poslužitelj    | Prikazuje trenutni pravila vatrozida razini poslužitelja     |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx)             | Poslužitelj    | Stvori ili ažurira pravila vatrozida razini poslužitelja       |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx)          | Poslužitelj    | Uklanja pravila vatrozida razini poslužitelja                  |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx)        | Baze podataka  | Prikazuje trenutni pravila vatrozida razinom baze podataka   |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx)    | Baze podataka  | Stvori ili ažurira pravila vatrozida razinom baze podataka |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) | Baza podataka | Uklanja baze podataka razini pravila vatrozida                |

### <a name="rest-api"></a>REST API-JA

| API-JA                  | Razina  | Opis                                                      |
|----------------------|--------|------------------------------------------------------------------|
| [Popis pravila vatrozida](https://msdn.microsoft.com/library/azure/dn505715.aspx)  | Poslužitelj | Prikazuje trenutni pravila vatrozida razini poslužitelja                 |
| [Stvaranje pravila vatrozida](https://msdn.microsoft.com/library/azure/dn505712.aspx) | Poslužitelj | Stvori ili ažurira pravila vatrozida razini poslužitelja                   |
| [Postavljanje pravila vatrozida](https://msdn.microsoft.com/library/azure/dn505707.aspx)    | Poslužitelj | Ažuriranja svojstva postojećeg pravila vatrozida razini poslužitelja |
| [Brisanje pravila vatrozida](https://msdn.microsoft.com/library/azure/dn505706.aspx) | Poslužitelj | Uklanja pravila vatrozida razini poslužitelja                              |


### <a name="azure-powershell"></a>Azure PowerShell

| Cmdlet                                                                                              | Razina  | Opis                                                      |
|-----------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------------|
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx)    | Poslužitelj | Vraća trenutni pravila vatrozida razini poslužitelja                  |
| [Novi AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx)    | Poslužitelj | Stvaranje novog pravila vatrozida razini poslužitelja                         |
| [Postavljanje AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx)    | Poslužitelj | Ažuriranja svojstva postojećeg pravila vatrozida razini poslužitelja |
| [Uklanjanje AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) | Poslužitelj | Uklanja pravila vatrozida razini poslužitelja                              |

> [AZURE.NOTE] Može se kao pet minuta odgoditi za promjene postavki vatrozida stupile na snagu.

## <a name="troubleshooting-the-database-firewall"></a>Otklanjanje poteškoća s vatrozidom baze podataka

Kada pristup servisu baza podataka Microsoft Azure SQL ponašaju kao što ste očekivali Imajte na umu sljedeće:

- **Konfiguraciju lokalne vatrozida:** Prije računala možete pristupiti baze podataka SQL Azure, možda ćete morati iznimku vatrozida na svojem računalu stvorite za priključak TCP 1433. Ako želite napraviti veze unutar granicu Azure oblaka, možda ćete morati otvaraju dodatni priključci. Dodatne informacije potražite u **V12 podataka za SQL: izvan Dodavanje veze za vanjskih unutar** odjeljak [priključci izvan 1433 za ADO.NET 4,5 i V12 baze podataka SQL](sql-database-develop-direct-route-ports-adonet-v12.md).

- **Mrežnog prevođenja adresa (NAT):** Zbog NAT, IP adresa koristi vaše računalo da biste se povezali s bazom podataka SQL Azure mogu se razlikovati od IP adresu prikazanu u postavke za konfiguriranje IP računala. Da biste pogledali IP adresu vaše računalo koristi za povezivanje s Azure, prijavite se na portal i idite na karticu **Konfiguracija** poslužitelja koji hostira baze podataka. U odjeljku **Dopuštene IP adrese** prikazuje se **Trenutni IP adresu klijenta** . Kliknite **Dodaj** u **Dopuštene IP adrese** da biste omogućili računalu za pristup poslužitelju.

- **Promjene na popis dopuštenih ne stupila na snagu još:** Mogu postojati kao pet minuta odgoditi za promjene konfiguracije vatrozid baze podataka SQL Azure stupile na snagu.

- **Prijavu nemate ovlasti ili koristila netočnu lozinku:** Ako prijave imati dozvole na poslužitelju baze podataka SQL Azure ili netočna lozinka koristi, odbijen je veza s poslužiteljem baze podataka SQL Azure. Stvaranje vatrozid postavke omogućuje samo klijente s prilikom pokušaja povezivanja s poslužiteljem; svaki klijent morate unijeti vjerodajnice potrebne sigurnost. Dodatne informacije o pripremi prijave potražite u članku Upravljanje bazama podataka, prijave i korisnika u bazi podataka SQL Azure.

- **Dinamičke IP adresa:** Ako imate internetsku vezu s odabiru IP adresa za dinamičku i imate li problema s postavljanjem kroz vatrozid, nije moguće pokušajte nešto od sljedećeg:

 - Traži vašeg davatelja internetskih usluga (ISP) za dodijeljene na klijentskim računalima pristup poslužitelju baze podataka SQL Azure rasponu IP adresa, a zatim dodajte rasponu IP adresa u pravilu vatrozida.

 - Početak statičke IP adresiranja umjesto klijentskim računalima za, a zatim dodajte IP adrese kao pravila vatrozida.

## <a name="next-steps"></a>Daljnji koraci

Kako na članke o stvaranju pravila poslužitelja baze podataka – razini i vatrozida potražite u članku:

- [Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću portala za Azure](sql-database-configure-firewall-settings.md)
- [Konfiguriranje pravila poslužitelja baze podataka – razini i vatrozida za baze podataka SQL Azure pomoću T SQL](sql-database-configure-firewall-settings-tsql.md)
- [Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću komponente PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću REST API-JA](sql-database-configure-firewall-settings-rest.md)

Vodič za stvaranje baze podataka, potražite u članku [Stvaranje baze podataka SQL u minutama pomoću portala za Azure](sql-database-get-started.md).
Pomoć za povezivanje s bazom podataka Azure SQL s Otvori izvor ili aplikacije drugih proizvođača, potražite u članku [klijent brzi početak rada primjere koda s bazom podataka SQL](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Da biste razumjeli kako se kretati s bazama podataka potražite u članku [Upravljanje sigurnošću baze podataka programa access i prijavite se](https://msdn.microsoft.com/library/azure/ee336235.aspx).



## <a name="additional-resources"></a>Dodatni resursi

- [Zaštita baze podataka](sql-database-security.md)
- [Centar za sigurnost za modul za baze podataka SQL Server i baze podataka Azure SQL](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
