<properties
   pageTitle="Baze podataka Azure SQL sastavlja više klijentske aplikacije s odvajanja i učinkovitosti"
   description="Saznajte kako SQL baze podataka sastavlja više klijentske aplikacije"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="builds-multi-tenant-apps-with-azure-sql-database-with-isolation-and-efficiency"></a>Sastavlja više klijentske aplikacije s bazom podataka Azure SQL s odvajanja i učinkovitosti

## <a name="leverage-elastic-pools-and-build-more-efficient-multi-tenant-apps"></a>Korištenje elastic grupe i izraditi učinkovitiji više klijentske aplikacije

Ako ste SaaS razvojni više klijentske aplikacije za pisanje i obrađuju broju korisnika, često unesete tradeoffs u kupca performanse, upravljanje i sigurnost. S Azure SQL baza podataka Elastic baze podataka grupe, više nemate da bi taj narušena pouzdanost. Ove grupe olakšavaju upravljanje i praćenje više klijentske aplikacije i dobiti odvajanja prednosti jedan-kupca-po-baze podataka. [Dizajn](sql-database-design-patterns-multi-tenancy-saas-applications.md)uvidjeti za SaaS više klijentske aplikacije s bazom podataka Azure SQL.

![Sastavljanje-višestruki-klijentu-aplikacije](./media/sql-database-build-multi-tenant-apps/sql-database-build-multi-tenant-apps.png)

## <a name="auto-scaling-you-control"></a>Automatsko skaliranje kontrola

Grupe automatski promijeniti omjer performanse i kapacitet pohrane za elastic baze podataka u hodu. Možete kontrolirati performanse dodijeljene zajedničko područje, dodati ili ukloniti elastic baze podataka na zahtjev i definirati performanse elastic baza podataka bez utjecaja na ukupni trošak na resurse. To znači da ne morate brinuti o upravljanju korištenje pojedinačne baze podataka.

[Pročitajte dokumentaciju](sql-database-elastic-pool.md)

## <a name="intelligent-management-of-your-environment"></a>Upravljanje Inteligentna okruženja

Preporuke za ugrađene promjenu veličine doći odredite baze podataka koje želite im grupe. Te preporuke dopustite analizu "što ako" za brzi optimizirati da bi odgovarao vašim ciljevima performansi. Obogaćeni performanse nadzor i nadzorne ploče za otklanjanje poteškoća olakšavaju vizualizacija Upotreba povijesne skup.

[Pročitajte dokumentaciju](sql-database-elastic-pool-guidance.md)

## <a name="performance-and-price-to-meet-your-needs"></a>Performanse i cijena da bi odgovarao vašim potrebama

Osnovni, Standardno, a Premium grupe omogućuju spektra općenite performanse, za pohranu i cijene mogućnosti. Grupe mogu sadržavati do 400 elastic baze podataka. Elastic baze podataka možete automatski skalom do 1000 elastic baze podataka transakcije jedinica (eDTU).

[Pročitajte dokumentaciju](https://azure.microsoft.com/pricing/details/sql-database/?b=16.50)

## <a name="elastic-tools"></a>Alati za elastic

Osim elastic grupe su baze podataka SQL značajke za upravljanje radu aktivnosti preko više baza podataka:

**Izvođenje upita unakrsno-baze podataka i izvješćivanje o pogreškama.**  
[Upit elastic baze podataka](sql-database-elastic-query-overview.md) omogućuje vam da biste pokrenuli upite ili izvješća preko baze podataka u vašem elastic i pristup udaljeni podaci pohranjene u mnoge baze podataka na skup odjednom.

**Pokrenite transakcije unakrsni baze podataka.**  
[Transakcije baze podataka za elastic](sql-database-elastic-transactions-overview.md) dopustiti izvođenje transakcija koje se protežu na više baza podataka u baze podataka SQL i izvođenje operacija (odnosno tijekom obrade financijskih transakcija preko baze podataka ili kada se ažurira zalihama u jednu bazu podataka i narudžbe).

**Izvršavanje iste operacija na nekoliko baza podataka.**  
[Elastic baze podataka zadataka](sql-database-elastic-jobs-overview.md) izvršavanje administrativnih operacija kao što ponovno stvaranje indeksa ili ažuriranje sheme preko svake baze podataka u vašem elastic.

Idite na početnu stranicu da biste saznali što još SQL baze podataka se nudi.
[Pogledaj](https://azure.microsoft.com/services/sql-database/) 

## <a name="next-steps"></a>Daljnji koraci

Nabavite [besplatne pretplate Azure](https://azure.microsoft.com/get-started/) i [Stvaranje prve baze podataka SQL Azure](sql-database-get-started.md).

## <a name="additional-resources"></a>Dodatni resursi

Istražite [Mogućnosti SQL baze podataka](https://azure.microsoft.com/services/sql-database/).
 
Pregledajte [Tehnički pregled SQL baze podataka](sql-database-technical-overview.md).  
