<properties
    pageTitle="Planiranje nadogradnje okruženja sustava SQL baza podataka V12 | Microsoft Azure"
    description="U članku se opisuje pripreme i ograničenja prilikom nadogradnje na verziju V12 od Azure SQL baze podataka."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genemi"/>


# <a name="plan-and-prepare-to-upgrade-to-sql-database-v12"></a>Planiranje i Priprema za nadogradnju na V12 baze podataka SQL


U ovoj se temi opisuju planiranja i pripreme morate izvršiti nadogradnju baze podataka Azure SQL iz verzije V11 V12.


Novi [Azure Portal](https://portal.azure.com/) dostupna podrška nadogradnju V12.


U sljedećoj su tablici navedeni druge teme pomoći za V12.


| Naslov i veza | Opis sadržaja |
| :--- | :--- |
| [Što je novo u V12 baze podataka SQL](sql-database-v12-whats-new.md) | U članku se opisuje kako V12 Azure SQL baze podataka bliže u unosi cijelog slične značajke s programom Microsoft SQL Server detalje. |
| [Stvaranje baze podataka u V12 baze podataka SQL](sql-database-get-started.md) | U članku se opisuje kako stvoriti novu bazu podataka Azure SQL na verziju V12. Opisuje različite mogućnosti osim samo praznu bazu podataka. |


## <a name="plan-ahead"></a>Planiranje


Pododlomci koji slijede sadrže učenje i donošenje odluka morate adresa prije nego akcije prema nadogradnje baze podataka Azure SQL na V12.

### <a name="version-clarification"></a>Upit za verziju


Ovaj dokument odnosi nadogradnje baze podataka sustava Microsoft Azure SQL iz verzije V11 na V12. Više formally su brojevi verzija blizu sljedeće dvije vrijednosti, kao što je dojavi Transact-SQL naredbe **Odaberite @@version; ** :


- 12.0.2000.8 *(ili bitnom veće, V12)*
- 11.0.9228.18 *(V11)*
 - Ponekad V11 je također se nazivaju SAWA v2.

### <a name="service-tier-planning"></a>Planiranje sloju za servis


Počevši s V12, baze podataka SQL Azure će podržavati samo razine servisa pod nazivom Basic, standardna i Premium. Usluge sloju Web i Business na V12 nije podržano. Dakle, ako planirate nadogradite vašu bazu podataka Azure SQL koji se trenutno nalazi na webu ili tvrtke sloja u sustavu, morate odlučiti što mora biti njegova novog servisnog sloja sustava.


Detaljne informacije o servisa razine Basic, standardna i Premium potražite u članku:

- [Razine SQL baze podataka usluge](sql-database-service-tiers.md)
- [Nadogradnja Web-tvrtke baze podataka za SQL baze podataka na nove razine servisa](sql-database-upgrade-server-portal.md)



### <a name="review-the-geo-replication-configuration"></a>Konfiguriranje pregled replikacije zemlj.


Ako bazu podataka Azure SQL je konfiguriran za zemlj replikacije, trebali biste dokumenta svoju trenutnu konfiguraciju i Zaustavi zemlj replikacije prije nego što počnete postupak nadogradnje Priprema. Nakon dovršetka nadogradnje za zemlj replikaciju morate ponovo konfigurirati bazu podataka.


Na strategije je ostavite izvor ostaje netaknuta i testiranje na kopiju baze podataka.


## <a name="preparation-actions"></a>Priprema akcije


Nakon što dovršite planiranja, možete izvršiti akciju korake opisane u Pododlomci koji slijede Priprema za konačni nadogradnje faza.


Detaljne opise konačni nadogradnje faza dostupnih u temama pomoći koji su povezani s pri vrhu ove teme pomoći.


### <a name="service-tier-actions"></a>Servis sloju akcije


Cijene sloju Web i Business servisa nije podržana na V12.


Ako je baze podataka V11 Azure SQL weba ili tvrtke baze podataka, postupak nadogradnje nudi da biste se prebacili bazu podataka podržani sloju. Nadogradnja preporučuje sloj koji odgovara povijest radno opterećenje baze podataka. Međutim, možete odabrati bilo koji podržani sloju vam se sviđa.


Koraci potrebni tijekom nadogradnje možete smanjiti prebacivanjem V11 baze podataka od sloju Web i tvrtke prije nadogradnje. To možete učiniti pomoću novi [Azure Portal](https://portal.azure.com/).


Ako niste sigurni koji sloju servisa da biste prešli na, možda je razinu S2 standardne sloju prvo početni odabir. Bilo koji manje sloju će imati manje resursa od sloju Web i Business imali.


### <a name="suspend-geo-replication-during-upgrade"></a>Privremeno obustavljanje zemlj replikacije tijekom nadogradnje


Nadogradnja V12 ne može pokrenuti ako zemlj replikacijom aktivan na bazu podataka. Najprije morate ponovo konfigurirati da biste prestali koristiti zemlj replikacije baze podataka.


Kada Nadogradnja Završi možete konfigurirati da biste ponovno koristili zemlj replikacije baze podataka.


### <a name="open-ports-on-an-azure-vm-for-client-connectivity"></a>Otvorene priključke na programa Azure VM za povezivanje s klijentom


Ako klijentski program povezuje V12 baze podataka SQL tijekom klijent na Azure virtualnog računala (VM), morate otvoriti sljedeće priključak raspone na na VM:

- 11000 11999
- 14000 14999


Kliknite [ovdje](sql-database-develop-direct-route-ports-adonet-v12.md) detalje o priključke za V12 SQL baze podataka. Priključci potrebni su po poboljšane performanse u SQL baze podataka V12.


##<a id="limitations"></a>Ograničenja tijekom i nakon nadogradnje na V12


### <a name="portals-for-v12"></a>Portalima za V12


Postoje tri portalima za Azure, a svaka ima različite mogućnosti vezane uz V12 SQL baze podataka.


- [http://portal.Azure.com/](https://portal.azure.com/)<br/>Portal za Azure je novo, a nalazi se i dalje na pregled stanja. Ovaj portal prilično još nije na punu opće dostupnih (GA). Portal:
 - Možete upravljati V12 poslužitelj i bazu podataka.
 - Možete nadograditi bazu podataka V11 V12.


- [http://Manage.windowsazure.com/](http://manage.windowsazure.com/)<br/>Ovaj klasični Portal Azure se može osigurati naposljetku. Portal:
 - Možete upravljati V12 poslužitelj i bazu podataka.
 - Može *ne* nadogradnju vašeg V11 baze podataka za V12.


- (http://*yourservername*. database.windows.net)<br/>Portal klasične baze podataka Azure SQL:
 - Možete*ne* upravljanje V12 poslužiteljima.


Pozivamo vas da biste se povezali s bazama podataka sustava Azure SQL s Visual Studio 2015 (VS2015). VS2015 moguće je koristiti za zadatke kao što su sljedeće:


- Da biste pokrenuli Transact-SQL naredbu.
- Da biste dizajnirali shemu.
- Za razvoj baze podataka, mrežnog i izvanmrežnog.


Ili umjesto toga besplatno možete preuzeti [2015 zajednice za Visual Studio](https://www.visualstudio.com/vs/community/), što je punu verziju VS2015.


Stariji Azure klasični portalu na stranici baze podataka, možete kliknuti **otvorena u Visual Studio** da biste pokrenuli VS2015 na vašem računalu za povezivanje s bazom podataka Azure SQL.


Za drugi zamjenski možete koristiti SQL Server Management Studio (SSMS) 2014 s [CU6](http://support.microsoft.com/kb/3031047/) da biste se povezali s bazom podataka SQL Azure. Dodatne pojedinosti nalaze se na blogu:<br/>[Klijent tooling ažuriranja za baze podataka SQL Azure](https://azure.microsoft.com/blog/2014/12/22/client-tooling-updates-for-azure-sql-database/).


> [AZURE.IMPORTANT] Preporučuje se da uvijek koristite najnoviju verziju Management Studio ostati sinkronizirani s ažuriranjima za Microsoft Azure i SQL baze podataka. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="limitation-during-upgrade-to-v12"></a>Ograničenje *tijekom* nadogradnje na V12


Tijekom nadogradnje na V12 postaje dostupan za pristup podacima V11 bazu podataka. Još nema nekoliko ograničenja treba uzeti u obzir.


| Ograničenje | Opis |
| :--- | :--- |
| Trajanje nadogradnje | Trajanje nadogradnje ovisi o veličini, edition i broj baza podataka na poslužitelju. Postupak nadogradnje možete pokrenuti sata dana za poslužitelje osobito za poslužitelje koji je baza podataka:<br/><br/>*Veće od 50 GB ili<br/> * Pri sloj koji nisu premium servis<br/><br/>Stvaranje nove baze podataka na poslužitelju tijekom nadogradnje također možete povećati nadogradnje trajanje. |
| Nema zemlj replikacije | Zemlj replikacija nije podržana na poslužitelju V12 koji je trenutno uključen za nadogradnju s V11. |
| Baza podataka nije ukratko dostupna u konačni faza Nadogradnja V12 | Baza podataka pripadaju poslužitelja V11 ostaju dostupne tijekom postupka nadogradnje. Veza s poslužiteljem i baza podataka je ukratko nisu dostupne u konačne fazu, kad se parametar putem počinje s V11 za spreman V12.<br/><br/>Parametar tijekom razdoblja može biti u rasponu od 40 sekundi do pet minuta. Za većinu poslužitelje promjenu putem očekuje da biste dovršili 90 sekundi. Prijelaz s vremenom povećava se za poslužiteljima koji imaju velik broj baze podataka ili kada baze podataka imaju radnih opterećenja podebljanom pisanje. |


### <a name="limitation-after-upgrade-to-v12"></a>Ograničenje *nakon* nadogradnje na V12


| Ograničenje | Opis |
| :--- | :--- |
| Nije moguće vratiti V11 | Nakon nadogradnje mjestu, rezultat ne može biti vratit stanje ili poništiti. |
| Razina weba ili tvrtke | Nakon nadogradnje pokreće poslužitelj za novu bazu podataka V12 možete više ne prepoznaje ili prihvatiti servisa sloju weba ili tvrtke. |



### <a name="export-and-import-after-upgrade-to-v12"></a>Izvoz i uvoz *nakon* nadogradnje na V12


Možete izvoz i uvoz V12 baze podataka pomoću [Portala za Azure](https://portal.azure.com/). Ili možete izvoz i uvoz pomoću nekog od sljedećih alata:


- SQL Server Management Studio (SSMS)
- Visual Studio 2015.
- Framework sloja podataka aplikacije (DacFx)


Međutim, da biste koristili alate, prvo morate imati ili instalacija njihove najnovija ažuriranja da bi oni podržavaju nove značajke V12:


- [Kumulativno ažuriranje 6 za SQL Server Management Studio 2014.](http://support2.microsoft.com/kb/3031047)
- [Ažuriranje veljača 2015 za SQL Server baza podataka Tooling u Visual Studio 2013](https://msdn.microsoft.com/data/hh297027)
- [Veljača 2015 sloja podataka aplikacije Framework (DacFx) za V12 baze podataka Azure SQL](http://www.microsoft.com/download/details.aspx?id=45886)


> [AZURE.NOTE] Prethodni veze na alate su ažurirani na ili nakon ožujak 2, 2015. Preporučujemo da koristite noviju ažuriranja od tih alata.


#### <a name="automated-export"></a>Automatsko izvoza


I dalje biti dostupni kao pretpregled automatiziranog izvoz.  Kada koristite automatski izvoz potrebni su ažuriranja alata za klijenta.  Ako odaberete da biste snimili rezultirajuća datoteka i uvoz s poslužiteljem na lokaciji, ažuriranja tooling navedene su vam potrebne da biste uspješno uvezli.


### <a name="restore-to-v12-of-a-deleted-v11-database"></a>Vraćanje V12 izbrisane V11 baze podataka

Sljedeći scenarij objašnjava da se izbrisane baze podataka V11 Azure SQL može vratiti na poslužitelj baze podataka za SQL Azure V12.

1. Recimo da imate poslužitelj baze podataka za SQL Azure V11. <br/> Na poslužitelju koji su vam baze podataka na zastarjeli sloju servisa Web i tvrtke.
2. Brisanje baze podataka.
3. Nadogradite poslužitelj V12.
4. Sljedeće Vraćanje baze podataka na poslužitelj. <br/> Baza podataka će se time nadograditi V12 na razini S0 sloju standardnog servisa.
5. Bazu podataka možete prijeći u sloju bilo kojem podržanom servisu ako S0 nije vašim postavkama.


### <a name="powershell-cmdlets"></a>Cmdleta ljuske PowerShell


Da biste pokrenuli, zaustavili ili monitor nadogradnje V12 za baze podataka SQL Azure s V11 ili neku drugu verziju pre V12 dostupnih cmdleta ljuske PowerShell.

- [Nadogradnja V12 SQL baze podataka pomoću komponente PowerShell](sql-database-upgrade-server-powershell.md)

Referentnu dokumentaciju o tih cmdleta ljuske PowerShell potražite u članku:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Početak AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Zaustavi AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Cmdlet Zaustavi znači otkazati, bez pauze. Ne postoji način da biste nastavili nadogradnju, osim ponovno počevši od početka. Cmdlet Zaustavi čisti i izdavanje sve odgovarajuće resurse.


## <a name="failure-resolution"></a>Razrješavanje pogreške


Nadogradnja ne uspije iz bilo kojeg razloga neparan, bazu podataka V11 ostaje aktivna i raspoloživa normalno.


## <a name="related-links"></a>Srodne veze


- Microsoft Azure [značajke za pregled](https://azure.microsoft.com/services/preview/)


<!--Anchors-->
[Subheading 1]: #subheading-1
