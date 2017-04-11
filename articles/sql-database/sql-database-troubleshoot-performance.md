<properties
    pageTitle="Performanse baze podataka SQL usklađivanje Savjeti | Microsoft Azure"
    description="Savjeti za ugađanju u bazi podataka SQL Azure putem procjenu i poboljšanja performansi."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="performanse SQL ugađanju performansi baze podataka ugađanju performansi sql usklađivanje savjete za ugađanje performansi baze podataka sql"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-database-performance-tuning-tips"></a>Savjeti za ugađanje performansi SQL baze podataka
Možete promijeniti [servisa sloju](sql-database-service-tiers.md) baze podataka za jedinstveno ili povećati eDTUs je skup elastic baze podataka u bilo kojem trenutku da biste poboljšali performanse, ali želite odrediti prilike da biste poboljšali i najprije optimiziranja performansi upita. Nedostaju indekse i loše optimizirana upiti su najčešćih razloga za performanse loše baze podataka. Ovaj članak sadrži upute za u bazi podataka SQL ugađanju performansi.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="steps-to-evaluate-and-tune-database-performance"></a>Koraci za procjenu i Ugađanje baze podataka performansi
1.  [Portal za Azure](https://portal.azure.com)kliknite **baze podataka SQL**odaberite bazu podataka, a pomoću grafikona praćenja potražite resurse Približavanje njihove maksimum. DTU potrošnje prikazan je prema zadanim postavkama. Kliknite **Uređivanje** da biste promijenili vremenski raspon i vrijednosti prikazane.
2.  Pomoću [Uvid performanse upita](sql-database-query-performance.md) za procjenu upite koji se koriste DTUs, a zatim koristite [Savjetnik za baze podataka SQL](sql-database-advisor.md) da biste prikazali preporuke za stvaranje i ispuštanje indeksi, parameterizing upita i rješavanje problema sheme.
3.  Dinamični Upravljanje prikazima (DMVs), prošireni događaja (Xevents) i spremište upita u SSMS možete koristiti da biste dobili performanse parametara u stvarnom vremenu. Potražite u [temi smjernice performanse](sql-database-performance-guidance.md) za detaljne nadzor i ugađanje savjete.


    > [AZURE.IMPORTANT] Preporučuje se da uvijek koristite najnoviju verziju Management Studio ostati sinkronizirani s ažuriranjima za Microsoft Azure i SQL baze podataka. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="steps-to-improve-database-performance-with-more-resources"></a>Korake da biste poboljšali performanse baze podataka s dodatni resursi
1.  Jedan baze podataka, možete [promijeniti razine usluge](sql-database-scale-up.md) na zahtjev da biste poboljšali performanse baze podataka.
2.  Više baza podataka, preporučuje se korištenje [grupe elastic baze podataka](sql-database-elastic-pool-guidance.md) da biste automatski skalirali resursi.
