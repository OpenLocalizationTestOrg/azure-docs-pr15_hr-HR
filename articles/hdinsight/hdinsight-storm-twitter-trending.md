<properties
   pageTitle="Na twitteru trendova teme s Apache oluja na HDInsight | Microsoft Azure"
   description="Saznajte kako koristiti Trident da biste stvorili topologija Apache oluja koji određuje trendova teme na Twitteru na temelju hashtags."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Određivanje Twitter trendova teme s Apache oluja na HDInsight

Saznajte kako koristiti Trident da biste stvorili oluja topologije koji određuje trendova teme (raspršivanje oznake) na Twitter.

Trident je više razine apstrakcije koje nudi alate kao što su spojevi, zbrajanja, grupiranja, Funkcije i filtri. Uz to, Trident dodaje primitives za obradu s praćenjem stanja, rastuća. U ovom se primjeru pokazuje kako stvoriti topologije pomoću prilagođenih spout, funkcija i nudi Trident nekoliko ugrađene funkcije.

> [AZURE.NOTE] U ovom se primjeru intenzivnog temelji se na primjer [Trident oluja](https://github.com/jalonsoramos/trident-storm) po Juan Alonso.

##<a name="requirements"></a>Preduvjeti

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java i JDK 1.7</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a>

* <a href="http://git-scm.com/" target="_blank">Brojka</a>

* Račun za razvojne inženjere Twitter

##<a name="download-the-project"></a>Preuzimanje projekta

Pomoću sljedeći kod Kloniraj lokalno projekta.

    git clone https://github.com/Blackmist/TwitterTrending

##<a name="topology"></a>Topologija

Topologija u ovom primjeru je na sljedeći način:

![Topologija](./media/hdinsight-storm-twitter-trending/trident.png)

> [AZURE.NOTE] Ovo je pojednostavljeni prikaz topologije. Više instanci komponente će raspodijeliti čvorovi u klasteru.

Kod Trident koji implementira topologije je na sljedeći način:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Kod čini sljedeće:

1. Stvara novu strujanje na spout. Na spout dohvaća tweets iz Twitter te ih filtre za ključnim (ljubav, glazba i primjer u ovom primjeru).

2. HashtagExtractor, prilagođenu funkciju koristi se za izdvajanje raspršivanje oznake sa svakom tweet. To čuje za strujanje.

3. Strujanje je grupirane prema oznaci raspršivanje i proslijediti programa Sakupljač. U ovom Sakupljač stvara ukupan broj koliko je puta došlo je do svakoj oznaci raspršivanje. Ove podatke je ista i u memoriji. Na kraju, novi strujanje je čuje koji sadrži oznaku raspršivanje i broj.

4. Jer smo zanima samo najpopularnijih raspršivanje oznake za određene skupine tweets, u sklopu **FirstN** primjenjuje se da biste vratili samo 10 najvećih vrijednosti, koji se temelji na polje broj.

> [AZURE.NOTE] Osim spout i HashtagExtractor, ne možemo koristite ugrađene funkcije Trident.
>
> Informacije o ugrađene operacije potražite u članku <a href="https://storm.apache.org/apidocs/storm/trident/operation/builtin/package-summary.html" target="_blank">storm.trident.operation.builtin paketa</a>.
>
> Trident stanja implementacije osim MemoryMapState, potražite u sljedećim člancima:
>
> * <a href="https://github.com/fhussonnois/storm-trident-elasticsearch" target="_blank">Oluja Trident elastic pretraživanja</a>
>
> * <a href="https://github.com/kstyrc/trident-redis" target="_blank">Trident redis</a>

###<a name="the-spout"></a>Na spout

Spout, **TwitterSpout**koristi <a href="http://twitter4j.org/en/" target="_blank">Twitter4j</a> dohvatiti tweets iz Twitter. Filtar se stvara (ljubav, glazba i primjer u ovom primjeru), a dolazne tweets (Stanje) koji se podudaraju s filtrom spremaju se u povezane blokiranja reda. (Dodatne informacije potražite u članku <a href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedBlockingQueue.html" target="_blank">LinkedBlockingQueue klase</a>) Na kraju, stavke su povlače isključivanje reda čekanja i čuje topologiji.

###<a name="the-hashtagextractor"></a>Na HashtagExtractor

Da biste izdvojili raspršivanje oznake, <a href="http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--" target="_blank">getHashtagEntities</a> se koristi za dohvaćanje sve raspršivanje oznake koje se nalaze u objavu na tweeteru. Zatim to čuje za strujanje.

##<a name="enable-twitter"></a>Omogućivanje Twitter

Da biste registrirali novu aplikaciju Twitter i dobiti pristup i potrošača tokena podatke koji su potrebni za čitanje iz Twitter, poduzmite sljedeće korake:

1. Idite na <a href="https://apps.twitter.com" target="_blank">Twitteru aplikacije</a> , a zatim kliknite gumb **Stvori novu aplikaciju** . Kad ispunite obrazac, **Povratni URL** polje ostavite prazno.

2. Kada je stvorena aplikacija, kliknite karticu **tipke i pristup tokena** .

3. Kopirajte **Potrošača ključ** i **Tajna korisničke** podatke.

4. Pri dnu stranice odaberite **Stvori token za pristup** ako nema tokena postoji. Pri stvaranju tokena kopirajte **Tokena za pristup** i **Pristup tokena tajna** podatke.

5. U projektu **TwitterSpoutTopology** prethodno klonirana, otvorite datoteku **resources/twitter4j.properties** , dodajte informacije koje ste prikupili u prethodnim koracima i spremite datoteku.

##<a name="build-the-topology"></a>Sastavljanje topologije

Da biste sastavili projekt, koristite sljedeći kod:

        cd [directoryname]
        mvn compile

##<a name="test-the-topology"></a>Testiranje topologije

Da biste testirali topologije lokalno, koristite sljedeću naredbu:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Nakon pokretanja topologije, trebali biste vidjeti ispravljanje informacija koje sadrži raspršivanje oznake i broji emitted po topologije. Izlaz trebao izgledati otprilike ovako:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

##<a name="next-steps"></a>Daljnji koraci

Sad kad testirate topologije lokalno, otkrijte kako implementirati topologije: [uvođenje i upravljanje Apache oluja topologija na HDInsight](hdinsight-storm-deploy-monitor-topology.md).

Možda će vas zanimati i u sljedećim temama oluja:

* [Razvoj Java topologija za oluja na HDInsight pomoću Maven](hdinsight-storm-develop-java-topology.md)

* [Razvoj C# topologija za oluja na HDInsight pomoću Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Za više primjera oluja za HDinsight:

* [Primjer topologija za oluja na HDInsight](hdinsight-storm-example-topology.md)
