<properties
    pageTitle="Što je novo u SQL baze podataka V12 | Microsoft Azure"
    description="U članku se opisuje zašto sustavi tvrtke koje koriste baze podataka SQL Azure u oblaku će koje prednosti prilikom nadogradnje na verziju V12 sada."
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
    ms.date="08/15/2016"
    ms.author="genemi"/>


# <a name="whats-new-in-sql-database-v12"></a>Što je novo u V12 baze podataka SQL


U ovoj se temi opisuju brojne prednosti novu verziju V12 baze podataka SQL Azure s putem verzija V11.


Nastavit za dodavanje značajki na V12. Stoga preporučujemo posjetite naš web-stranici servisa ažuriranja za Azure i koristiti njezinim filtrima:


- Filtrirani [servisa SQL baze podataka](https://azure.microsoft.com/updates/?service=sql-database).
- Filtrirane tako da se Općenito dostupan [Najave (GA)](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) za značajke SQL baze podataka.


Najnovije informacije o ograničenjima resursa za bazu podataka sustava SQL navedenih na:<br/>[Ograničenja resursa za baze podataka azure SQL](sql-database-resource-limits.md).


## <a name="increased-application-compatibility-with-sql-server"></a>Kompatibilnost povećana aplikacija sa sustavom SQL Server


Ključni cilja V12 baze podataka SQL nije poboljšanje kompatibilnosti za 2014 sustava Microsoft SQL Server, a da biste zadržali kompatibilnost kao nove verzije sustava SQL Server objavljivanja. Među druga područja V12 postiže slične značajke sa sustavom SQL Server u području važne programibilnost. Ako, na primjer:

- [Ugrađeni JSON podrška](https://msdn.microsoft.com/library/dn921897.aspx)

- [Prozor funkcije](http://msdn.microsoft.com/library/ms189798.aspx), s [iznad](http://msdn.microsoft.com/library/ms189461.aspx)

- [XML indekse](http://msdn.microsoft.com/library/bb934097.aspx) i [selektivno XML indeksa](http://msdn.microsoft.com/library/jj670104.aspx)

- [Evidentiranje promjena](http://msdn.microsoft.com/library/bb933875.aspx)

- [ODABIR... U](http://msdn.microsoft.com/library/ms188029.aspx)

- [Pretraživanje po cijelom tekstu](http://msdn.microsoft.com/library/ms142571.aspx)

- [PROMIJENITI KONFIGURACIJU baze podataka iz DJELOKRUGA (-SQL transakcija)](http://msdn.microsoft.com/library/mt629158.aspx)

U odjeljku [ovdje](sql-database-transact-sql-information.md) za mali skup značajke još nisu podržane u SQL baze podataka.


### <a name="compatibility-level-130"></a>Razinu kompatibilnosti 130


> [AZURE.IMPORTANT] Pokretanje u **lipnja 2016**, *nove* baze podataka stvorene na V12 za baze podataka SQL Azure imaju svoje razinu kompatibilnosti Počni od 130, koji odgovara Microsoft SQL Server 2016 ZŽ
> 
> Možete koristiti `ALTER DATABASE YourDatabase SET COMPATIBILITY_LEVEL = 120` po želji.
> 
> Baze podataka stvorene prije lipnja 2016 imaju svoje razinu kompatibilnosti promijenio ta promjena zadanog. Niti je razina promijenio nadogradnje sa V11 na V12 baze podataka.



Objašnjenje kako možete usporediti upitima najvažnije između najnoviji nasuprot prethodne razine kompatibilnosti potražite u članku:

- [Poboljšani upita performansi uz razinu kompatibilnosti 130 u bazi podataka Azure SQL](sql-database-compatibility-level-query-performance-130.md)



## <a name="more-premium-performance-new-performance-levels"></a>Bolje performanse premium, nove razine performansi


U V12, ne možemo povećati jedinice baze podataka transakcije (DTUs) dodijeljen sve razine performanse Premium po bez dodatnih troškova 25%. Čak i veće pozitivne performanse mogu se postići pomoću nove značajke kao što su:


- Podrška za u memoriji [columnstore indeksi](http://msdn.microsoft.com/library/gg492153.aspx).
- [Tablica particija po recima](http://msdn.microsoft.com/library/ms187802.aspx) s povezanim poboljšanja [SKRATITI](http://msdn.microsoft.com/library/ms177570.aspx)tablicu.
- Dostupnost dinamički Upravljanje prikazima [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx) olakšavaju praćenje i ugađanje performansi.


### <a name="reliable-performance"></a>Pouzdan performansi


Ako klijentski program povezuje V12 baze podataka SQL tijekom klijent na Azure virtualnog računala (VM), morate otvoriti sljedeće priključak raspone na na VM:

- 11000 11999
- 14000 14999


Kliknite [ovdje](sql-database-develop-direct-route-ports-adonet-v12.md) detalje o priključke za V12 SQL baze podataka. Priključci potrebni su po poboljšane performanse u SQL baze podataka V12.


## <a name="better-support-for-cloud-saas-vendors"></a>Bolju podršku za oblak SaaS dobavljače


Samo u V12, ne možemo objavio novu razinu standardne performanse S3 i javne pretpregled [grupe elastic baze podataka](sql-database-elastic-pool.md). Grupe elastic baze podataka je rješenje namijenjen za oblak SaaS dobavljačima.  Pomoću grupe elastic baze podataka, možete učiniti sljedeće:


- Zajedničko korištenje DTUs među baze podataka da biste smanjili troškove za velikog broja baze podataka.
- Izvršavanje [zadataka elastic baze podataka](sql-database-elastic-jobs-overview.md) za upravljanje bazama podataka na razini.


## <a name="security-enhancements"></a>Poboljšanja sigurnosti


Sigurnost je primarni složen svakome tko pokreće svoje tvrtke u oblaku. Najnovije značajke sigurnosti u V12 obuhvaćaju sljedeće:


- [Sigurnost na razini retka](http://msdn.microsoft.com/library/dn765131.aspx) (RLS)
- [Masking dinamičke podatke](sql-database-dynamic-data-masking-get-started.md)
- [Sadrži baze podataka](http://msdn.microsoft.com/library/ff929188.aspx)
- [Uloge aplikacije](http://msdn.microsoft.com/library/ms190998.aspx) upravljati GRANT, Nemoj DOPUSTITI, OPOZIV
- [Šifriranje prozirne podataka](http://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) (TDE)
- [Povezivanje s bazom podataka SQL pomoću provjere autentičnosti Azure Active Directory](sql-database-aad-authentication.md)
 - Baze podataka SQL sada podržava provjeru autentičnosti Azure Active Directory, mehanizam povezivanja s bazom podataka SQL pomoću identiteta servisa Azure Active Directory (Azure AD). S provjerom autentičnosti Azure Active Directory možete središnjeg upravljanja identitetima korisnika baze podataka i druge Microsoftove servise na jednom središnjem mjestu.
- [Uvijek šifrirane](https://msdn.microsoft.com/library/mt163865.aspx) (u pretpregledu) čini šifriranje prozirne aplikacijama i klijentima omogućuje šifriranje povjerljive podatke u klijentskim aplikacijama bez zajedničko korištenje ključeva za šifriranje s bazom podataka SQL.


## <a name="increased-business-continuity-when-recovery-is-needed"></a>Povećana poslovanje kad je potreban oporavak


V12 nudi poboljšani oporavak točke ciljevi (RPOs) i Procijenjena oporavak vremena (ERTs):


| Značajka continuity tvrtke | Starije verzije | V12 |
| :-- | :-- | :-- |
| Vraćanje za zemlj. | • RPO < 24 sata.<br/>• UMETNI < 12 sati. | • RPO < 1 sat.<br/>• UMETNI < 12 sati. |
| Aktivni replikacije zemlj. | • RPO < pet minuta.<br/>• UMETNI < 1 sat. | • RPO < 5 sekundi.<br/>• UMETNI < 30 sekundi. |


Dodatne informacije potražite u članku [poslovanje u SQL baze podataka](sql-database-business-continuity.md) .


## <a name="more-reasons-to-upgrade-now"></a>Dodatni razlozi zbog kojih odmah nadograditi


Postoji mnogo dobra razloga zašto korisnici trebali biste nadogradili sada na V12 za baze podataka SQL Azure V11:


- SQL baza podataka V12 ima dugačak popis značajki izvan značajke V11.
- Ne možemo nastaviti s dodavanjem novih značajki V12, ali nema novih značajki dodat će se V11.
- Većina nove značajke objavljivanja na V12 baze podataka SQL prije nego što se oni su puštanja za Microsoft SQL Server.


## <a name="are-you-using-v12-already"></a>Koristite V12 već?


Jedan jednostavno da biste saznali je li baza podataka ili logičke poslužitelju koji se izvodi na starije verzije servisa SQL baze podataka i tako da učinite sljedeće:


1. Idite na [Portal za Azure](https://portal.azure.com/).
2. Kliknite **Pregledaj**.
3. Kliknite **SQL poslužitelja**.
4. Ikonu pokraj poslužitelja ili u bazi podataka nalaže priče:
 - ![Ikona za poslužitelj v12](./media/sql-database-v12-whats-new/v12_icon.png) **V12 logičke poslužitelja**
 - ![Ikona za starije verzije poslužitelj](./media/sql-database-v12-whats-new/earlier_icon.png) **starije verzije logičke poslužitelja**


Druga tehnika da biste provjerili verziju je da biste pokrenuli u `SELECT @@version;` izjava u bazi podataka, i prikaz slične rezultate:


- **12**.0.2000.10 &nbsp; *(verzija V12)*
- **11**.0.9228.18 &nbsp; *(verzija V11)*


V12 baze podataka mogu nalaziti samo na poslužitelju za logičke V12. I V12 poslužitelja možete hostirati samo baze podataka V12.


Ako nije još radite na V12, možete nadograditi poslužitelj za logičke tako da slijedite korake u [nadograditi V12 baze podataka sustava SQL na mjestu](sql-database-v12-plan-prepare-upgrade.md).


## <a name="V12AzureSqlDbPreviewGaTable"></a>Općenito područja dostupnosti


- Po srpanj 31 2015., sva područja imali povećana za opće dostupnih (GA).
- V12 objavljen u prosincu 2014., ali samo na status pretpregleda.

[Dodatni uvjeti korištenja pretpregleda Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
