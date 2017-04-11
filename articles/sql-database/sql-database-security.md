<properties
   pageTitle="Pregled sigurnosti baze podataka za SQL"
   description="Saznajte više o sigurnosti baze podataka SQL Azure i SQL Server, uključujući razlike u oblak i SQL Server lokalnog kada je riječ o autentičnosti, autorizacije, sigurnost veze, šifriranje i usklađenost."
   services="sql-database"
   documentationCenter=""
   authors="tmullaney"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="06/09/2016"
   ms.author="thmullan;jackr"/>


# <a name="securing-your-sql-database"></a>Zaštita baze podataka sustava SQL

## <a name="overview"></a>Pregled

U ovom se članku vodi kroz osnove zaštita sloju podataka aplikacije pomoću baze podataka SQL Azure. Točnije, u ovom se članku će početak rada s resursima za ograničavanje pristupa, zaštita podataka, i nadzor aktivnosti u bazi podataka stvorene u programu [Početak rada s bazom podataka SQL vodič](sql-database-get-started.md). Pregled sigurnosnih značajki dostupne na svim flavors SQL potražite u članku [Centar za sigurnost za modul za baze podataka SQL Server i baze podataka SQL Azure](https://msdn.microsoft.com/library/bb510589). Dodatne informacije i dostupna je u [Sigurnost i baze podataka SQL Azure tehničke studiju](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="connection-security"></a>Sigurnost veze

Sigurnost veze se odnosi na kako ograničiti i sigurnu vezu u bazu podataka pomoću pravila vatrozida i šifriranje veze.

Pravila vatrozida koriste poslužitelj i bazu podataka da biste odbacili pokušaja povezivanja s IP adrese nisu izričito whitelisted. Da biste omogućili aplikacije ili javnu IP adresu klijentskom računalu pokušaja povezivanja s novom bazom podataka, najprije morate stvoriti pravila vatrozida razini poslužitelja pomoću klasične Portal Azure, REST API-JA ili PowerShell. Kao preporučenim načinom rada, trebali biste ograničiti dopušteno kroz vatrozid za poslužitelj najveću moguću rasponi IP adresa. Dodatne informacije potražite u članku [Vatrozid za baze podataka SQL Azure](https://msdn.microsoft.com/library/ee621782).

Sve veze s bazom podataka SQL Azure zahtijevaju šifriranja (SSL/TLS) na cijelo vrijeme dok je podataka "na putu" i iz baze podataka. U nizu za povezivanje s vašeg računala, morate navesti parametara za šifriranje veze i *ne* pouzdanosti certifikata poslužitelja (to je obavljen Ako kopirate niz za povezivanje Classic portala za Azure), u suprotnom veze će provjeri identitet poslužitelja i bit će susceptible od napada "Muškarac-u-u – Srednje". Za upravljački program ADO.NET, na primjer, parametri niza te veze su **Šifriraj = True** i **Certifikatpouzdanogposlužitelja = False**. Dodatne informacije potražite u članku [web-mjesto šifriranje za veze baze podataka SQL Azure i u okvir za provjeru valjanosti certifikata](https://msdn.microsoft.com/library/azure/ff394108#encryption).


## <a name="authentication"></a>Provjera autentičnosti

Provjera autentičnosti se odnosi na kako dokazivanje vašeg identiteta prilikom povezivanja s bazom podataka. Baze podataka SQL podržava dvije vrste provjere autentičnosti:

 - **SQL provjeru autentičnosti**, koji koristi korisničko ime i lozinku
 - **Provjera autentičnosti azure Active Directory**, koji koristi identiteta upravlja Azure Active Directory i nije podržana za upravlja i integriran domene

Pri stvaranju logičke poslužitelj za bazu podataka koju ste naveli "administrator poslužitelja" Prijava pomoću korisničkog imena i lozinke. Koristi te vjerodajnice, koje možete provjeriti autentičnost za sve baze podataka na tom poslužitelju kao vlasnik baze podataka ili "vlasnika baze podataka." Ako želite koristiti provjeru autentičnosti Azure Active Directory, morate stvoriti drugi poslužitelj za administratore pod nazivom "Azure AD administrator," što je dopušteno da biste saznali više o administraciji Azure AD korisnici i grupe. U ovom administrator možete izvesti i sve operacije običnog server administrator može. Vodič kroz stvaranje administrator Azure AD da biste omogućili provjere autentičnosti Azure Active Directory potražite u članku [Povezivanje sa SQL tako da pomoću Azure Active Directory provjera autentičnosti baze podataka](sql-database-aad-authentication.md) .

Kao preporučenim načinom rada aplikacije trebali biste koristiti neki drugi račun za provjeru autentičnosti – na taj način možete ograničiti dozvole dodijeljene aplikacije i smanjiti količinu rizika zlonamjerni aktivnosti u slučaju da je podložno napada za unos SQL kodu aplikacije. Preporučeni način je da biste stvorili na [nalazi korisnik baze podataka](https://msdn.microsoft.com/library/ff929188), koji omogućuje uređivanje aplikacije za provjeru autentičnosti izravno u jednu bazu podataka. Možete stvoriti korisnik sadržavao baze podataka koja koristi provjeru autentičnosti SQL izvršavanjem sljedeće T SQL naredbe dok ste povezani s korisnik bazu podataka kao prijava administrator poslužitelja:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Ako ste stvorili administrator Azure AD, možete stvoriti korisnik sadržavao baze podataka koja koristi provjeru autentičnosti Azure Active Directory izvršavanjem sljedeće T SQL naredbe dok ste povezani s korisnik bazu podataka kao administrator Azure AD:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

U svakom slučaju, niz za povezivanje vaše aplikacije navedite sljedeće korisničke vjerodajnice, umjesto administrator prijava server, da biste se povezali s bazom podataka.

Dodatne informacije o provjeru autentičnosti baze podataka za SQL potražite u odjeljku [Upravljanje bazama podataka i prijave u bazi podataka SQL Azure](sql-database-manage-logins.md).


## <a name="authorization"></a>Autorizacija
Autorizacija se odnosi na što možete učiniti unutar baze podataka SQL Azure, a to upravlja članstva korisnički račun uloga i dozvola. Kao preporučenim načinom rada treba dodijeliti korisnicima najmanje ovlasti potrebne. Baze podataka SQL Azure taj postupak olakšava da biste upravljali s ulogama u T-SQL:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

Računa administratora poslužitelja s kojim se povezujete je član db_owner koja ima izdavanje ništa unutar baze podataka. Spremite ovaj račun za implementaciju nadogradnje sheme i ostalih operacija upravljanja. Koristite račun "ApplicationUser" s više ograničene dozvole za povezivanje iz aplikacije u bazu podataka s najmanje ovlasti potrebne za svoju aplikaciju.

Načina da biste dodatno ograničiti korisnika pruža baze podataka SQL Azure:

* [Uloge baze podataka](https://msdn.microsoft.com/library/ms189121) koji nisu db_datareader i db_datawriter se može koristiti za stvaranje naprednijih aplikacije korisničkih računa ili manje Napredna upravljanje računima.
* Zrnastog [dozvole](https://msdn.microsoft.com/library/ms191291) omogućuju kontrolu operacijama možete učiniti na pojedinačne stupci, tablice, prikaza, postupke, i drugih objekata u bazi podataka.
* [Oponašanje](https://msdn.microsoft.com/library/vstudio/bb669087) i [Modul potpisivanje](https://msdn.microsoft.com/library/bb669102) mogu se sigurno privremeno pretvoriti dozvole.
* [Sigurnost na razini retka](https://msdn.microsoft.com/library/dn765131) mogu se ograničenje koje redaka korisnik može pristupiti.
* [Masking podataka](sql-database-dynamic-data-masking-get-started.md) može se koristiti za ograničavanje izlaganje povjerljive podatke.
* [Spremljene procedure](https://msdn.microsoft.com/library/ms190782) može se koristiti za ograničavanje akcije koje možete poduzeti u bazu podataka.

Upravljanje bazama podataka i logički poslužitelje na portalu klasični Azure ili pomoću upravitelja resursa API Azure upravlja dodjele uloga portala korisnički račun. Dodatne informacije o ovoj temi potražite u članku [Upravljanje pristupom utemeljeno na ulogama Azure portalu](../active-directory./role-based-access-control-configure.md).


## <a name="encryption"></a>Šifriranje

Azure SQL baze podataka pridonosi zaštiti podataka šifrira podataka kada je "na ostale" ili spremljene u datoteke baze podataka i sigurnosne kopije, pomoću [Prozirne šifriranje podataka](http://go.microsoft.com/fwlink/?LinkId=526242). Da biste šifrirali bazu podataka, povežite kao vlasnik baze podataka i izvršiti:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Za druge načine za šifriranje tajne vaše podatke, imajte na umu:

* [Šifriranje ćeliju razinom](https://msdn.microsoft.com/library/ms179331.aspx) određene stupce ili čak i ćelija s podacima s tipkama različite šifriranje.
* Ako vam je potrebna modulu sigurnost hardver ili središnje upravljanje hijerarhijom ključa šifriranja, razmislite o korištenju [Azure ključ sigurnog sa sustavom SQL Server u VM programa Azure](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).
* [Uvijek šifrirane](https://msdn.microsoft.com/library/mt163865.aspx) (u pretpregledu) čini šifriranje prozirne aplikacijama i klijentima omogućuje šifriranje povjerljive podatke u klijentskim aplikacijama bez zajedničko korištenje ključeva za šifriranje s bazom podataka SQL.

## <a name="auditing"></a>Nadzor

Nadzor i praćenje događaja baze podataka može pojednostavniti održavanje usklađenosti s propisima i prepoznavanje sumnjivoj aktivnosti. Nadzor baze podataka SQL omogućuje na događaje zapisa u bazi podataka za reviziju prijave u račun za Azure prostora za pohranu. Nadzor baze podataka SQL i integrira u Microsoft Power BI da biste olakšali izvješća dubinske analize i analize. Dodatne informacije potražite u članku [Uvod u SQL baze podataka nadzor](sql-database-auditing-get-started.md).

Otkrivanje prijetnju baze podataka za SQL pruža dodatne slojeva zaštite pri vrhu nadzor. Omogućuje odgovoriti prijetnji kako nastaju unosom sigurnosnih upozorenja na anomalous aktivnosti. Dodatne informacije potražite u članku [Početak rada s otkrivanje prijetnju baze podataka za SQL](sql-database-threat-detection-get-started.md).  

## <a name="compliance"></a>Usklađenost

Osim gornje značajki i funkcija koje omogućuju aplikacije i zadovoljavaju različite sigurnost zahtjevima za usklađenosti, baze podataka SQL Azure sudjeluje u pravilnim revizije i certificiran protiv broj standarde usklađenosti. Dodatne informacije potražite u [Centru za pouzdanost Microsoft Azure](https://azure.microsoft.com/support/trust-center/), gdje možete pronaći najnovije popis [certifications usklađenost SQL baze podataka](https://azure.microsoft.com/support/trust-center/services/).
