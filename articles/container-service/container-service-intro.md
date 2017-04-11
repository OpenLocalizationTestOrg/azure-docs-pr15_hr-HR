<properties
   pageTitle="Uvod servisa Azure spremnik | Microsoft Azure"
   description="Azure spremnik servis omogućuje da biste pojednostavnili stvaranje, konfiguriranje i upravljanje klaster virtualnih računala koje su prethodno konfigurirali da biste pokrenuli containerized aplikacije."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, spremnika, Micro-servisima, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="azure-container-service-introduction"></a>Azure spremnik servisa Uvod

Azure spremnik usluga omogućuje jednostavnije za stvaranje, konfiguriranje i upravljanje klaster virtualnih računala koje su prethodno konfigurirali da biste pokrenuli containerized aplikacije. Koristi optimizirana konfiguracije popularne Otvori izvor zakazivanja i djelovanje alata. To vam omogućuje da koristite svoje postojeće znanje i crtanje nakon velike i sve veći tijelo specijalizacije zajednice, upravljanje aplikacije utemeljene na spremnik na Microsoft Azure i.


![Azure spremnik usluga omogućuje znači da biste upravljali containerized aplikacije na više glavnih računala na Azure.](./media/acs-intro/acs-cluster.png)


Azure Service spremnik upravlja Docker oblik spremnika sadržaja da biste bili sigurni da vaše aplikacije spremnika potpuno prijenosni. Također podržava Marathon i Kontroler/OS ili Docker Swarm tako da se mogu mijenjati veličinu te aplikacije tisuće spremnika ili čak i tens tisućica.

Pomoću servisa Azure spremnik možete iskoristiti značajke poslovne klase Azure, i dalje zadržavanje prenosivost aplikacije – uključujući prenosivost pri djelovanje slojeva.

<a name="using-azure-container-service"></a>Pomoću servisa Azure spremnik
-----------------------------

Naš cilj sa servisom Azure spremnik je pružanje spremnik postavljeno okruženje pomoću alata Otvori izvor i tehnologijama koji su Popularni među naših korisnika danas. Predmemoriranja smo izložiti krajnje točke za standardne API-JA za odabrani orchestrator (Kontroler/OS ili Docker Swarm). Pomoću ove krajnje točke, možete koristiti softver koji možete razgovor te krajnje točke. Ako, na primjer, slučaju krajnju točku Docker Swarm možda odlučite koristiti Docker sučelja naredbenog retka (EŽA). Za Kontroler/OS, možete koristiti EŽA DCOS.

<a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Stvaranje Docker klaster pomoću servisa Azure spremnik
-------------------------------------------------------

Da biste počeli koristiti servis spremnik Azure, morate je implementirati programa servisa Azure spremnik klaster putem portala sustava (Traženje 'Azure spremnik Service'), pomoću predloška Azure Voditelj resursa ([Docker Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm) ili [Kontroler/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)) ili s [EŽA](/documentation/articles/xplat-cli-install/). Predlošci navedeni brzi početak rada moguće je izmijeniti da biste dodali dodatan ili poboljšan Azure konfiguracije. Dodatne informacije o implementaciji sustava servisa Azure spremnik klaster, potražite u članku [uvođenja programa servisa Azure spremnik klaster](container-service-deployment.md).

<a name="deploying-an-application"></a>Implementacija aplikacije
------------------------

Azure spremnik usluga omogućuje odabir Docker Swarm ili Kontroler/OS za djelovanje. Kako implementirati aplikaciju ovisi o vašem izboru orchestrator.

### <a name="using-dcos"></a>Korištenje Kontroler/OS

Kontroler/OS je raspodijeljeno operacijski sustav na temelju raspodijeljeno sustavi otklanjanje Apache Mesos. Apache Mesos je i na softver Foundation Apache i popis nekih na [najvećih imena u IT](http://mesos.apache.org/documentation/latest/powered-by-mesos/) kao korisnika i suradnici.

![Azure servis za spremnik konfigurirana za Swarm prikazuje agenata i matrice.](media/acs-intro/dcos.png)

Kontroler/OS i Apache Mesos obuhvaćaju skupa impressive značajke:

-   Dokazana skalabilnost

-   Pogreške replicirati matrice i slaves pomoću Apache ZooKeeper

-   Podrška za Docker oblikovani spremnika

-   Izvorni odvajanja zadataka s Linux spremnika

-   Multiresource zakazivanje (memorije, procesora, na disku i priključci)

-   Java, Python i C++ API-ji za razvoj novi paralelno aplikacije

-   Web-mjesto korisničkog Sučelja za pregled stanja klaster

Po zadanome, Kontroler/OS pokrenut servis za Azure spremnik sadrži platformu djelovanje Marathon za planiranje radnih opterećenja. Međutim, uključene implementacije Kontroler/OS ACS je Universe Mesosphere usluga koje možete dodati u svoj servis uključuju Spark, Hadoop, Cassandra i još mnogo toga.

![Kontroler/OS Universe na servisu Azure spremnik](media/dcos/universe.png)

#### <a name="using-marathon"></a>Korištenje Marathon

Marathon je init klaster razini i sustavu kontrole za servise u cgroups – ili u slučaju Docker oblikovani spremnika Azure spremnik servis. Marathon sadrži korisničko Sučelje web iz kojeg možete implementirati aplikacija. To možete pristupiti na URL koji izgleda otprilike ovako `http://DNS_PREFIX.REGION.cloudapp.azure.com` gdje DNS\_PREFIKS i REGIJA i definiraju trenutku implementacije. Naravno, možete unijeti i vlastiti naziv DNS-a. Dodatne informacije o pokretanju spremnik putem weba Marathon korisničkog Sučelja potražite u članku [Upravljanje spremnik putem korisničkog Sučelja web](container-service-mesos-marathon-ui.md).

![Popis aplikacija Marathon](media/dcos/marathon-applications-list.png)

Možete koristiti i REST API-ji za komunikaciju s Marathon. Mnoga biblioteka klijenta koji su dostupni za svaki alat. Oni pokrivaju na različitim jezicima – i Naravno, možete koristiti protokol HTTP u bilo koji jezik. Osim toga, mnoge popularne DevOps Alati nudi podršku za Marathon. Pruža to Maksimalna fleksibilnost za tim operacije kada radite s do servisa Azure spremnik klaster. Dodatne informacije o pokretanju spremnik pomoću Marathon REST API-JA potražite u članku [Upravljanje kontejner s REST API -JA](container-service-mesos-marathon-rest.md).

### <a name="using-docker-swarm"></a>Korištenje Docker Swarm

Docker Swarm sadrži izvorni Klasteriranje Docker. Budući Docker Swarm služi standardne API Docker, bilo koji alat koji već komunicira s Docker daemon možete koristiti Swarm za proziran proširenjem više domaćini na servisu Azure kontejner.

![Azure Service spremnik konfiguriran za korištenje Kontroler/OS – prikazom jumpbox, agenata i matrice.](media/acs-intro/acs-swarm2.png)

Podržane alate za upravljanje spremnika na Swarm klaster obuhvaćaju, ali nisu ograničeni na sljedeće:

-   Dokku

-   Sastavite docker EŽA i Docker

-   Krane

-   Jenkins

<a name="videos"></a>Videozapisi
------

Početak rada sa servisom Azure spremnik (101):  

> [AZURE.VIDEO azure-container-service-101]

Sastavni aplikacije pomoću servisa Azure spremnik (Sastavi 2016)

> [AZURE.VIDEO build-2016-building-applications-using-the-azure-container-service]
