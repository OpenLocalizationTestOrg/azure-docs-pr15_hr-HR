<properties
    pageTitle="Cijena elastic skup SQL baze podataka i performanse"
    description="Informacije o cijenama specifične za grupe elastic baze podataka."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="05/27/2016"
    ms.author="srinia"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="elastic-database-pool-billing-and-pricing-information"></a>Naplata skup elastic baze podataka i informacije o cijenama

Grupe elastic baze podataka se naplaćuju po sljedeće značajke:

- Elastic skup naplate nakon njegova stvaranja, čak i kad postoje nijedna baza podataka u.
- Elastic skup zaračunava naplate. To je isti mjerne učestalost kao performanse razine jedne baze podataka.
- Ako se novi broj eDTUs mijenja se veličina elastic skup, zatim na resurse ne naplate prema novi iznos eDTUS dok se ne dovrši postupak za promjenu veličine. To slijedi isti uzorak kao promijenite razinu performanse samostalne baze podataka.


- Cijena elastic grupe aplikacija temelji se na broj eDTUs na resurse. Cijena elastic grupe aplikacija ne ovisi o broj i upotreba elastic baza podataka unutar njega.
- PRICE se izračunati (broj eDTUs skup) x (jedinične cijene po eDTU).

Jedinična cijena eDTU za elastic skup veća je od DTU jedinične cijene za samostalnu bazu podataka u istom sloju servisa. Detalje potražite u članku [cijene SQL baze podataka](https://azure.microsoft.com/pricing/details/sql-database/). 


Da biste shvatili razine eDTUs i usluga, potražite u članku [Mogućnosti SQL baze podataka i performanse](sql-database-service-tiers.md).

## <a name="next-steps"></a>Daljnji koraci

- [Stvaranje grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool-create-portal.md)
- [Nadzor, upravljanje i veličine u skup elastic baze podataka](sql-database-elastic-pool-manage-portal.md)
- [Mogućnosti SQL baze podataka i performanse: objašnjenje što dostupna je u sloju svaki servis](sql-database-service-tiers.md)
- [Skriptu PowerShell za označavanje baze podataka prikladna za grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool-database-assessment-powershell.md)
