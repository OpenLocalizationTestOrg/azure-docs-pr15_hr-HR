<properties
   pageTitle="Pregled Lake spremišta podataka za Azure | Microsoft Azure"
   description="Razumijevanje što je Lake spremišta podataka za Azure i vrijednost pruža putem trgovine drugih podataka"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="overview-of-azure-data-lake-store"></a>Pregled Lake spremišta podataka za Azure

Azure podataka Lake spremište je spremište hyper skaliranje enterprise razini analitički radnih opterećenja velikih skupova podataka. Azure podataka Lake omogućuje prikupljanje podataka veličinu, vrstu i ingestion brzina na jednom mjestu jedan domena i exploratory analitičkih.

> [AZURE.TIP] Pomoću [spremišta podataka Lake tečaj](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) počnite istraživati servisa Azure podataka Lake pohrane.

Spremište Lake za Azure podataka možete pristupiti iz Hadoop (dostupno sa servisa HDInsight klaster) pomoću WebHDFS kompatibilnog REST API-ji. Posebno dizajnirane da biste omogućili analize na pohranjene podatke i postavljen je performanse scenarije analize podataka. Izvan okvira za obuhvaća sve poslovne klase mogućnosti – sigurnost, mogućnost upravljanja, skalabilnost, pouzdanosti i dostupnost – ključna za slučaj da koristite enterprise stvarnog života.


![Lake Azure podataka](./media/data-lake-store-overview/data-lake-store-concept.png)

Neke od ključne mogućnosti podataka Lake Azure uključuju sljedeće:

### <a name="built-for-hadoop"></a>Za Hadoop

Iz trgovine Azure podataka Lake je kompatibilan s Hadoop Distributed datoteka sustava (HDFS) i radi s zajednici Hadoop Apache Hadoop sustav datoteka.  Postojeći HDInsight aplikacije ili usluge koje koriste WebHDFS API jednostavno možete integrirati s spremišta Lake podataka. Pohrana podataka Lake izlaže i OSTALE WebHDFS kompatibilnog sučelja za aplikacije

Podatke pohranjene u spremištu Lake podataka mogu se jednostavno analizirati i pomoću Hadoop analitički okviri kao što su MapReduce ili grozd. Microsoft Azure HDInsight klastere možete dodjeli i konfigurirati da biste izravno pristupili podatke pohranjene u spremištu Lake podataka.

### <a name="unlimited-storage-petabyte-files"></a>Neograničeno prostor za pohranu datoteka petabyte

Spremište Lake podataka za Azure nudi neograničeno prostora za pohranu i je prikladna za pohranu raznih podatke za analizu. Ne primjenjuje nikakva ograničenja veličine račun, veličina datoteka ili količinu podataka koje se mogu pohraniti u podataka lake. Pojedinačne datoteke mogu biti u rasponu od kilobajta do petabytes veličine upućivanje sjajno odabir sadržavati bilo koju vrstu podataka. Podaci se pohranjuju durably tako da većeg broja primjeraka i nema ograničenja na razdoblje tijekom kojemu podatke moguće pohraniti u lake podataka.

### <a name="performance-tuned-for-big-data-analytics"></a>Performanse-počeo gledati analitičkih velikih skupova podataka

Spremište Lake podataka za Azure ugrađena je za pokretanje velike skaliranje analitički sustave kojima je potrebna pretraživanje velikog propusnost za slanje upita i analizirajte velike količine podataka. Lake podataka širi dijelove datoteke putem broj poslužitelja za pojedinačne prostora za pohranu. To poboljšava čitanja propusnost prilikom čitanja datoteke paralelno za izvođenje analize podataka.


### <a name="enterprise-ready-highly-available-and-secure"></a>Enterprise podrška: Vrlo dostupan i sigurne

Spremište Lake podataka za Azure nudi standardni dostupnosti i pouzdanosti. Vaše imovine podataka spremaju se durably tako da suvišnih kopije radi zaštite od bilo kojeg neočekivane pogreške. Korporacije u možete koristiti Azure podataka Lake njihova rješenja kao važan dio svoje postojeće podatke platforme.

Pohrana podataka Lake omogućuje i sigurnost poslovne klase pohranjene podatke. Dodatne informacije potražite u članku [Zaštita podataka u spremištu Lake za Azure podataka](#DataLakeStoreSecurity).


### <a name="all-data"></a>Svi podaci

Spremište Lake za Azure podataka možete spremiti sve podatke u izvornom obliku, kakav jest bez sve prethodne transformacije. Spremište Lake podataka ne zahtijeva sheme se definirati prije podatke učita, ostavite najviše pojedinačne analitički framework za tumačenje podataka i definirali shemu trenutku analiza. Mogućnosti za spremanje datoteke proizvoljne veličine i oblikovanja zahvaljujući spremišta Lake podataka za rukovanje strukturirani, djelomično strukturirane i nestrukturirane podatke.

Spremnici Azure podataka Lake pohrane podataka su zapravo mape i datoteke. Rade na pohranjene podatke pomoću SDK-ovi, Azure Portal i Azure Powershell. Pod uvjetom da stavite podatke u spremište pomoću ove sučelja i pomoću odgovarajuće spremnika, možete spremiti bilo koju vrstu podataka. Pohrana podataka Lake izvršiti sve posebno rukovanje podataka ovise o vrsti podataka koji se sprema.


## <a name="DataLakeStoreSecurity"></a>Zaštita podataka u spremištu Lake podataka za Azure

Spremište Lake za Azure podataka koristi Azure Active Directory za provjeru autentičnosti i kontrola pristupa popisa (ACL) za upravljanje pristupom podacima.

| Značajka                                 | Opis                              |
|-----------------------------------------|------------------------------------------|
| Provjera autentičnosti | Spremište Lake podataka za Azure integriran s Azure Active Directory (AAD) za upravljanje identiteta i pristup za sve podatke pohranjene u spremištu Lake za Azure podataka. Uslijed Integracija pogodnosti Lake Azure podatke iz svih AAD značajki uključujući višestruka provjera autentičnosti, uvjetno pristup, kontrola pristupa na temelju uloga, nadzor korištenja aplikacije, sigurnost nadzor i upozorenjem, itd. Spremište Lake podataka za Azure podržava protokol OAuth 2.0 za provjeru autentičnosti s u sučelju za OSTALE. |
| Kontrola pristupa                          | Spremište Lake podataka za Azure omogućuje kontrolu pristupa po dozvole POSIX stil koji prikazuje WebHDFS protokol za podršku. U trenutnom se izdanju ACL-a mogu se omogućiti na korijenska mapa, podmape, kao i pojedinačne datoteke. ACL-a primjenjujete korijensku mapu će se primijeniti na sve podređene mape i datoteke kao i.|

Želite li saznati više o zaštiti podataka u spremištu Lake podataka. Slijedite veze u nastavku.

* Upute o tome kako sigurno podatke u spremištu Lake podataka potražite u članku [Zaštita podataka u spremištu Lake za Azure podataka](data-lake-store-secure-data.md).
* Videozapisi se više sviđa? [Pogledajte ovaj videozapis](https://mix.office.com/watch/1q2mgzh9nn5lx) o tome radi zaštite podataka pohranjenih u spremištu Lake podataka.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Aplikacijama koje su kompatibilne sa Lake spremišta podataka za Azure

Spremište Lake podataka za Azure kompatibilan je s najviše Otvori izvor komponente u zajednici Hadoop. To također se integrira radi boljeg s drugih servisa za Azure. Ovime spremišta podataka Lake savršen mogućnost za vaše potrebe za pohranu podataka. Slijedite veze u nastavku da biste saznali više o načinu spremišta Lake podataka može koristiti i s komponente Otvori izvor, kao i drugih servisa za Azure.

* Pročitajte članak [aplikacija i servisa kompatibilan s Lake spremišta podataka za Azure](data-lake-store-compatible-oss-other-applications.md) za popis aplikacija Otvori izvor Uskladi s trgovinom Lake podataka.
* Potražite u članku [Integrating s drugih servisa za Azure](data-lake-store-integrate-with-other-services.md) da biste razumjeli kako spremišta Lake podataka mogu se s drugih servisa za Azure da biste omogućili širem krugu scenarijima.
* [Scenariji za korištenje spremišta Lake podataka](data-lake-store-data-scenarios.md) da biste saznali kako se koriste spremište Lake podataka u nekim scenarijima kao što su ingesting podataka, obrada podataka, preuzimanje podataka i vizualizacija podataka potražite u članku.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Što je Lake spremišta podataka za Azure datotečnom sustavu (adl: / /)?

Pohrana podataka Lake moguće pristupiti putem novi datotečnom u AzureDataLakeFilesystem (adl: / /), u okruženju Hadoop (dostupno sa servisa HDInsight klaster). Aplikacija i servisa koji koriste adl: / / mogu iskoristiti dodatno optimizaciju performansi koje trenutno nisu dostupne u WebHDFS. Kao rezultat spremišta podataka Lake pruža fleksibilnost ili avail najbolje performanse Preporučena mogućnost korištenja adl: / / ili zadržali postojeći kod po daljnjeg korištenja WebHDFS API izravno. Azure HDInsight potpuno upravlja AzureDataLakeFilesystem za navođenje najbolje performanse na spremište Lake podataka.

Možete pristupiti podacima u spremištu Lake podataka pomoću `adl://<data_lake_store_name>.azuredatalakestore.net`. Dodatne informacije o tome kako pristupiti podacima u spremištu Lake podataka potražite u članku [Prikaz svojstava pohranjene podatke](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Kako početi koristiti Lake spremišta podataka za Azure?

Pogledajte [Početak rada s spremišta Lake podataka pomoću portala za Azure](data-lake-store-get-started-portal.md), o Dodjela resursa u trgovini Lake podataka pomoću portala za Azure. Kada ste dodjeli Lake Azure podataka, možete naučiti kako ponuda velikih skupova podataka, kao što su Lake analize podataka za Azure ili Azure HDInsight pomoću spremišta Lake podataka. Možete stvoriti i aplikaciju .NET stvorite račun za Azure podataka Lake pohrane i izvođenje operacija kao što su prijenos podataka, preuzimanje podataka, itd.

- [Početak rada s analize Lake Azure podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight pomoću spremišta Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Početak rada s spremišta Lake podataka za Azure pomoću .NET SDK-a](data-lake-store-get-started-net-sdk.md)


## <a name="data-lake-store-videos"></a>Pohrana podataka Lake videozapisa

Ako biste radije gledanje videozapisa da biste saznali spremišta podataka Lake pruža videozapisa na raspon značajke.

* [Stvaranje računa spremišta Lake podataka za Azure](https://mix.office.com/watch/1k1cycy4l4gen)
* [Pomoću programa Explorer podataka za upravljanje podacima u spremištu Lake podataka za Azure](https://mix.office.com/watch/icletrxrh6pc)
* [Povezivanje Azure podataka Lake analize Lake spremišta podataka za Azure](https://mix.office.com/watch/qwji0dc9rx9k)
* [Pohrana podataka programa Access Azure Lake putem analize podataka Lake](https://mix.office.com/watch/1n0s45up381a8)
* [Povezivanje Azure HDInsight Lake spremišta podataka za Azure](https://mix.office.com/watch/l93xri2yhtp2)
* [Pohrana podataka programa Access Azure Lake putem grozd i Svinja](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [Korištenje DistCp (Hadoop Distributed kopija) da biste kopirali podatke i iz spremišta Lake podataka za Azure](https://mix.office.com/watch/1liuojvdx6sie)
* [Korištenje Apache Sqoop premještanje podataka s relacijske izvore i Lake spremišta podataka za Azure](https://mix.office.com/watch/1butcdjxmu114)
* [Djelovanje podataka pomoću tvorničke Azure podataka za pohranu Lake podataka za Azure](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Zaštita podataka u spremištu Lake Azure podataka](https://mix.office.com/watch/1q2mgzh9nn5lx)



