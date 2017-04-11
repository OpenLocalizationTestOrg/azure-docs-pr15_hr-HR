
<properties
   pageTitle="Elasticsearch Azure uputama za | Microsoft Azure"
   description="Elasticsearch Azure uputama."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>

# <a name="elasticsearch-on-azure-guidance"></a>Elasticsearch Azure uputama za 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Elasticsearch je vrlo skalabilni Otvori izvor tražilica i baza podataka. Je korisno za situacije koje su potrebne Brza analiza i otkrivanje podataka sadrži veliki skupova podataka. Ovaj vodič razmatra neke ključne aspekte treba uzeti u obzir prilikom dizajniranja Elasticsearch klaster, s naglaskom na načina za optimiziranje i testiranju konfiguracije.

> [AZURE.NOTE]Ovaj vodič pretpostavlja da neki osnovni poznavanje programa Elasticsearch. Detaljnije informacije potražite u članku sustava [Elasticsearch web-mjesta](https://www.elastic.co/products/elasticsearch). 

- **[Pokretanje Elasticsearch na Azure][]** nudi kratak Uvod u strukturi Općenito Elasticsearch i u članku se opisuje kako implementirati programa Elasticsearch klaster pomoću Azure. 
- **[Tuning podataka ingestion izvedbe Elasticsearch na Azure][]** opisuje implementaciju sustava klaster Elasticsearch i sadrži temeljitije analize razne mogućnosti konfiguracije treba uzeti u obzir kada očekujete visoko rata ingestion podataka.
- **[Prikupljanje podataka tuning i performanse upita za Elasticsearch na Azure][]** nudi temeljitije analize mogućnosti uzeti u obzir pri odabiru kako optimizirati sustav za performanse upita i pretraživanja.
- **[Konfiguriranje resilience i oporavak na Elasticsearch na Azure][]** opisuju neke važne značajke programa Elasticsearch klaster koji kako biste smanjili mogućnost gubitka podataka i vremena za oporavak prošireni podataka.
- **[Stvaranje performanse testiranja okruženje za Elasticsearch na Azure][]** opisuje da biste postavili okruženje za testiranje ingestion performanse podataka i upita radnih opterećenja sustava klasteru Elasticsearch. 
- **[Implementacije u JMeter testiranje plan za Elasticsearch][]** navedene izvodi performanse testova implementirati pomoću JMeter test tarife zajedno s kod programskog jezika Java ugrađeni JUnit test za izvođenje zadatke kao što su prijenos podataka u Elasticsearch klaster.
- **[Implementacija uzorak JMeter JUnit za testiranje Elasticsearch performanse][]** opisuje kako stvoriti i koristiti uzorak JUnit koje se mogu stvoriti i prenijeti podatke programa Elasticsearch klaster. Ovo osigurava visoko fleksibilne pristup da biste učitali testiranja koji mogu uzrokovati velike količine podataka Testiraj bez ovisno o vanjske podatkovne datoteke. 
- **[Pokretanje automatskog testova otpornost Elasticsearch][]** prikazano kako pokrenuti testove otpornost koji se koristi u ovoj seriji. 
- **[Pokretanje automatskog testova Elasticsearch performanse][]** prikazano kako pokrenuti testove performanse koji se koristi u ovoj seriji.


[Elasticsearch sustavom Azure]: guidance-elasticsearch-running-on-azure.md
[Ugađanju performansi Ingestion podataka za Elasticsearch na Azure]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[Stvaranje performanse testiranje okruženje za Elasticsearch na Azure]: guidance-elasticsearch-creating-performance-testing-environment.md
[Implementacijom na Plan testiranja JMeter za Elasticsearch]: guidance-elasticsearch-implementing-jmeter-test-plan.md
[Implementacija uzorak JMeter JUnit za testiranje Elasticsearch performansi]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[Usklađivanje prikupljanja podataka i performanse upita za Elasticsearch na Azure]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[Konfiguriranje Resilience i oporavak na Elasticsearch na Azure]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[Pokretanje testova otpornost automatiziranog Elasticsearch]: guidance-elasticsearch-running-automated-resilience-tests.md
[Pokretanje testova automatiziranog Elasticsearch performansi]: guidance-elasticsearch-running-automated-performance-tests.md
