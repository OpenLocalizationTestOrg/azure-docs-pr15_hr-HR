<properties
    pageTitle="Rješavanje uobičajenih problema vezu s bazom podataka SQL Azure"
    description="Koraci za prepoznavanje i rješavanje uobičajenih pogrešaka veze za baze podataka SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Rješavanje problema s povezivanjem s bazom podataka SQL Azure

Kada prekida se veza s bazom podataka SQL Azure, primit ćete [poruku o pogrešci](sql-database-develop-error-messages.md). Ovaj se članak odnosi središnje temu koja pomaže pri otklanjanju problema s povezivanjem baze podataka SQL Azure. Uvod [u najčešćim uzrocima](#cause) problema s povezivanjem, preporučuje [Alat za otklanjanje poteškoća](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) koji olakšava identiteta problema, a sadrži korake za otklanjanje poteškoća da biste riješili [tranzitne pogreške](#troubleshoot-transient-errors) i [pogreške stalni ili nisu prolaznim](#troubleshoot-the-persistent-errors). Na kraju, popis [svih relevantnih članaka za problema s povezivanjem baze podataka SQL Azure](#all-topics-for-azure-sql-database-connection-problems).

Ako naiđete na probleme veze, isprobajte korake rješavanje problema koji su opisani u ovom članku.
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Uzrok

Problemi s povezivanjem uzrok može biti nešto od sljedećeg:

- Primjena najbolje prakse i smjernice za dizajn tijekom postupka dizajniranja aplikacije nije uspjelo.  Potražite u članku [Pregled za razvoj SQL baze podataka](sql-database-develop-overview.md) za početak rada.
- Azure ponovno konfiguriranje SQL baze podataka
- Postavke vatrozida
- Prekoračenje vremena veze
- Podaci za prijavu nisu ispravne
- Maksimalno ograničenje otvoriti na nekoliko resursa baze podataka SQL Azure

Općenito govoreći, problema s povezivanjem s bazom podataka SQL Azure moguće je klasificirati na sljedeći način:

- [Tranzitne pogreške (short-lived ili Povremeni)](#troubleshoot-transient-errors)
- [Stalni ili nisu prolaznim pogrešaka (pogreške koje se redovito ponavljati)](#troubleshoot-the-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Isprobajte alat za otklanjanje poteškoća za problema s povezivanjem baze podataka SQL Azure

Ako se pojavi poruka o pogrešci u određenu vezu, pokušajte [Ovaj alat](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), koji će vam pomoći brzo identiteta i riješiti problem.

## <a name="troubleshoot-transient-errors"></a>Otklanjanje poteškoća s tranzitne pogreške
Aplikacija se pojave pogreške tranzitne, pregledajte savjeta o tome kako za otklanjanje poteškoća i smanjivanje učestalost te pogreške u sljedećim temama:

- [Otklanjanje poteškoća s bazom podataka &lt;x&gt; na poslužitelju &lt;y&gt; nije dostupan (pogreška: 40613)](sql-database-troubleshoot-connection.md)
- [Otklanjanje poteškoća s, dijagnosticiranje i spriječili SQL vezu pogreške i tranzitne pogreške za baze podataka SQL](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>

## <a name="troubleshoot-persistent-errors-non-transient-errors"></a>Otklanjanje poteškoća s stalni pogreške (pogreške koje nisu prolaznim)

Ako aplikacija persistently ne uspije povezati s bazom podataka SQL Azure, obično označava problem s nešto od sljedećeg:

- Konfiguriranje vatrozida. Azure SQL baze podataka ili klijentsko vatrozid blokira veza s bazom podataka SQL Azure.
- Ponovno konfiguriranje na klijentskoj strani mreže:, na primjer, nove IP adresa ili proxy poslužitelj.
- Pogreška korisničkog:, na primjer, napisana parametre veze, kao što su naziv poslužitelja u nizu za povezivanje.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Koraci za rješavanje problema s povezivanjem stalni

1.  Postavljanje [pravila za vatrozid](sql-database-configure-firewall-settings.md) da biste dopustili IP adresa klijenta.
2.  Na svim vatrozidima između klijenta i s Internetom, provjerite je li priključak 1433 otvorena za izlazne veze. Pročitajte članak [Konfiguriranje Vatrozida za Windows da biste Dopusti pristup SQL Server](https://msdn.microsoft.com/library/cc646023.aspx) za Dodatne napomene.
3.  Provjerite je li niz za povezivanje i druge postavke veze. U odjeljku niz za povezivanje u [tema problema s povezivanjem](sql-database-connectivity-issues.md#connections-to-azure-sql-database).
4.  Provjerite stanje servisa na nadzornoj ploči. Ako mislite da je regionalnih nedostupnosti, potražite u članku [oporavak iz programa nedostupnosti](sql-database-disaster-recovery.md) korake da biste oporavili novu regiju.

## <a name="all-topics-for-azure-sql-database-connection-problems"></a>Sve teme za baze podataka SQL Azure vezom

U sljedećoj su tablici navedeni svake teme veze problem koji se primjenjuje izravno na servis baze podataka SQL Azure.


| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 1 | [Rješavanje problema s povezivanjem s bazom podataka SQL Azure](sql-database-troubleshoot-common-connection-issues.md) | Ovo je odredišna stranica za otklanjanje problema s povezivanjem u bazi podataka SQL Azure. Se opisuje kako prepoznati i riješiti tranzitne pogrešaka i stalni ili nisu prolaznim pogrešaka u bazi podataka SQL Azure. |
| 2 | [Otklanjanje poteškoća s, dijagnosticiranje i spriječiti SQL vezu pogreške i tranzitne pogreške za SQL baze podataka](sql-database-connectivity-issues.md) | Upute za otklanjanje poteškoća s dijagnosticiranje i spriječili pogreška SQL veze ili tranzitne pogrešaka u bazi podataka SQL Azure. |
| 3 | [Opće smjernice tranzitne kvara obradu](best-practices-retry-general.md) | Opće smjernice za rukovanje tranzitne kvara nudi prilikom povezivanja s bazom podataka SQL Azure. |
| 4 | [Otklanjanje poteškoća s povezivanjem s bazom podataka sustava Microsoft Azure SQL](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) | Ovaj alat pomaže identiteta problem riješiti pogreške pri povezivanju. |
| 5 | [Otklanjanje poteškoća s "baze podataka &lt;x&gt; na poslužitelju &lt;y&gt; trenutno nije dostupna. Pokušajte ponovno kasnije veze"Pogreška](sql-database-troubleshoot-connection.md) | U članku se opisuje kako prepoznati i riješiti 40613 pogreška: "baze podataka &lt;x&gt; na poslužitelju &lt;y&gt; trenutno nije dostupna. Pokušajte ponovno kasnije veze." |
| 6 | [Kodovi pogrešaka SQL za baze podataka SQL klijentske aplikacije: pogreška veze i ostalih problema za baze podataka](sql-database-develop-error-messages.md) | Pruža informacije o kodovima pogrešaka SQL baze podataka SQL klijentske aplikacije, kao što su uobičajenih pogrešaka u bazi podataka povezivanja, poteškoća kopiju baze podataka i općenite pogreške. |
| 7 | [Azure SQL baze podataka performanse smjernice za jednu baze podataka](sql-database-performance-guidance.md) | Sadrži upute da biste utvrdili koji servis sloju najviše odgovara aplikacije. Omogućuje preporuke za ugađanje aplikacije da biste na najbolji mogući način baze podataka SQL Azure. |
| 8 | [Pregled razvoj baze podataka za SQL](sql-database-develop-overview.md) | Navedene veze na primjere koda za različite tehnologije koje možete koristiti za povezivanje i interakcije s bazom podataka SQL Azure. |
| 9 | Nadogradnja na baze podataka SQL Azure v12 stranice ([Azure portal](sql-database-upgrade-server-portal.md), [PowerShell](sql-database-upgrade-server-powershell.md)) | Sadrži upute za nadogradnju postojeći poslužitelji V11 za baze podataka SQL Azure i baza podataka na V12 za baze podataka SQL Azure pomoću portala za Azure ili PowerShell. |


## <a name="next-steps"></a>Daljnji koraci

- [Baze podataka SQL Azure otkloniti poteškoće s performansama](sql-database-troubleshoot-performance.md)
- [Otklanjanje poteškoća s bazom podataka SQL Azure dozvole](sql-database-troubleshoot-permissions.md)
- [Pogledajte sve teme za servis baze podataka SQL Azure](sql-database-index-all-articles.md)
- [Potražite u dokumentaciji na Microsoft Azure](http://azure.microsoft.com/search/documentation/)
- [Prikaz najnovijih ažuriranja sa servisom Azure SQL baze podataka](http://azure.microsoft.com/updates/?service=sql-database)


## <a name="additional-resources"></a>Dodatni resursi

- [Pregled razvoj baze podataka za SQL](sql-database-develop-overview.md)
- [Opće smjernice tranzitne kvara obradu](../best-practices-retry-general.md)
- [Biblioteka veza za SQL baze podataka i SQL Server](sql-database-libraries.md)
- [Tečaj za korištenje baze podataka SQL Azure](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database)
- [Tečaj za korištenje značajke elastic baza podataka i Alati](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale) 
