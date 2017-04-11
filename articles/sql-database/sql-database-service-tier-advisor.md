<properties 
   pageTitle="Cijene sloju preporuke za baze podataka SQL Azure" 
   description="Prilikom promjene cijene razine Azure portalu cijene sloju preporuke su pod uvjetom da preporučeno sloju koji najbolje odgovaraju radi radno opterećenje postojeće Azure SQL baze podataka na. Cijene razine opisuju sloju i performanse razinu servisa SQL baze podataka." 
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
   ms.date="08/08/2016"
   ms.author="sstein"/>

# <a name="sql-database-pricing-tier-recommendations"></a>Baze podataka SQL cijene sloju preporuke

 Cijene sloju preporuke predložiti servisa razinu sloju i performanse koja je najprikladniji za pokretanje postojeće Azure SQL baze podataka, radno opterećenje.

> [AZURE.NOTE] Cijene sloju preporuke samo su dostupne za Web i poslovnih baza podataka i grupe elastic baze podataka – i dostupno je samo za [Azure portal](https://portal.azure.com/).


Početak cijene sloju preporuke tijekom sljedeće zadatke:

- [Promjena servisa sloju i performanse razine (cijene sloju) SQL baze podataka](sql-database-scale-up.md)
- [Nadogradnja Azure SQL server na V12](sql-database-upgrade-server-portal.md)
- Dođite na V12 poslužitelj. Potražite u članku [baze podataka SQL cijene sloju preporuke](sql-database-service-tier-advisor.md).
- [Stvaranje grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool.md#elastic-database-pool-pricing-tier-recommendations)





## <a name="overview"></a>Pregled

Servis baze podataka SQL analizira trenutni performanse i značajku preduvjeti prema assessing povijesne resursa za bazu podataka za SQL. Uz to, minimalne prihvatljiva sloja u sustavu ovisi o ovisno o veličini baze podataka i značajke omogućeno [poslovanje](sql-database-business-continuity.md) . 

Ti podaci se analizirati i servisa sloju i performanse razinu koja je najbolja najprikladniji za pokretanje uobičajeni radno opterećenje u bazu podataka i održavanje je trenutni skup značajki preporučuje se.

- Servis pregledava prethodnih 15 do 30 dana povijesne podataka (korištenje resursa, veličina baze podataka i aktivnosti baze podataka) i izvodi uspoređivanje između količinu resursa potrošena i stvarne ograničenja razine trenutno dostupno usluge i razine performansi.
- Podataka je analizirati u 15 drugi intervalima i svakom razdoblju SkupRezultata je kategorizirati u postojeće sloju servisa i razina performansi koja je najbolja najprikladniji za rukovanje radno opterećenje te SkupRezultata.
- Ove 15 sekundi uzoraka se zatim pridružuje u većim analizu 15 30 dana i preporučuje se na servis sloju i performanse razinu optimalnog možete rukovati 95% povijesne radno opterećenje.

### <a name="recommendations"></a>Preporuke

Korištenje svoje baze podataka na temelju, trenutno postoje 2 kategorije preporuke koje možete naići:


| Preporuka | Opis |
| :--- | :--- |
| Nadogradnja | Nadogradnja na novu sloju. |
| Nije dostupan | Baze podataka potreban je minimalne radno opterećenje ili otprilike 35 dana aktivnosti. Nema dovoljno podataka možete unijeti valjani preporuke. |

## <a name="getting-pricing-tier-recommendations"></a>Početak cijene sloju preporuke

Početak cijene sloju preporuke tako da odaberete postojeću bazu podataka weba ili tvrtke, kliknite **sve postavke**, a zatim kliknite **određivanje cijena sloju (skaliranje DTUs)**. (Cijene sloju preporuke dostupne su i kada ste [nadograditi Azure SQL server da biste V12](sql-database-upgrade-server-portal.md).)

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Kliknite **PREGLEDAJ** > **baze podataka SQL**.
4. U plohu **SQL baze podataka** kliknite bazu podataka koju želite vidjeti preporuke za:

    ![Odaberite bazu podataka][1]

5. Na plohu baze podataka odaberite **sve postavke** , zatim odaberite **sloj određivanje cijena (skaliranje DTUs)**.


7. **Preporučeno cijene razine** otvorite gdje možete kliknite predloženi sloju i zatim kliknite gumb **Odaberi** da biste promijenili tu sloju.

    ![Registracija za pretpregled][4]

8. Po želji, kliknite **informacije o korištenju prikaza** da biste otvorili plohu **Cijene pojedinosti sloju preporuke** gdje možete pregledati preporučene sloju za bazu podataka, značajka usporedbe između trenutnog i preporučena razine i grafikon analize korištenja povijesne resursa.

    ![Registracija za pretpregled][5]



## <a name="summary"></a>Sažetak

Preporuke za cijene sloju ponuditi automatiziranog za prikupljanje telemetrijskih podataka za svaku SQL bazu podataka i preporučiti najbolje servisa sloju/performanse razine kombinacija na temelju stvarne performanse potrebama i preduvjeti za značajku u bazi podataka. Na plohu postavke kliknite **određivanje cijena sloju (skaliranje DTUs)** da biste vidjeli cijene sloju preporuke za sve Web i poslovnih baza podataka.



## <a name="next-steps"></a>Daljnji koraci

Ovisno o detalje o određenoj bazi podataka, obično izvršiti nadogradnju ili nadogradnju na stariju verziju ne događa po. Portal pronaći ćete obavijesti kao prijelaza baze podataka da biste je nova razina ili možete nadzirati stanja nadogradnje postavljanjem upita [sys.dm_operation_status (bazom podataka SQL Azure)](https://msdn.microsoft.com/library/dn270022.aspx) prikaz u glavnom bazom podataka SQL Server baze podataka.


<!--Image references-->
[1]: ./media/sql-database-service-tier-advisor/select-database.png
[4]: ./media/sql-database-service-tier-advisor/choose-pricing-tier.png
[5]: ./media/sql-database-service-tier-advisor/usage-details.png


 
