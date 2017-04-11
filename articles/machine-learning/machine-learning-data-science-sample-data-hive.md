<properties
    pageTitle="Uzorak podataka u tablicama vrste Hive Azure HDInsight | Microsoft Azure"
    description="Dolje uzorkovanje podataka u tablicama vrste Hive Azure HDInsight (Hadopop)"
    services="machine-learning,hdinsight"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />

# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Uzorak podataka u tablicama vrste Hive Azure HDInsight

U ovom se članku smo opisuju kako dolje ogledni podaci spremljeni u tablicama u vrste Hive Azure HDInsight pomoću grozd upita. Ne možemo pokrivaju tri popularly korištenih uzorkovanje načina:

* Uniform slučajno uzorkovanje
* Slučajno uzorkovanje po grupama
* Stratified uzorkovanje

**Zašto uzorak podataka?**
Ako je prevelik dataset plan za analizu, obično je dobro dolje ogledne podatke da biste smanjili veličinu manje no predstavniku i jednostavnije. To olakšava razumijevanje podataka, istraživanje i inženjering značajke. Njegova uloga u postupku timu podataka znanstvenog je da biste omogućili brzo prototyping funkcija obradu podataka i strojnog učenja modela.

Na **izborniku** ispod veze na temu koja opisuje kako ogledne podatke iz različitih okruženja za pohranu.

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Ovaj zadatak uzorkovanje je korak u [Timu podataka znanstvenog procesa (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="how-to-submit-hive-queries"></a>Kako poslati grozd upita
Grozd upiti mogu poslati konzoli Hadoop naredbenog retka na glavni čvor klaster Hadoop. Da biste to učinili, prijavite se u glavni čvor Hadoop klaster, otvorite konzolu za Hadoop naredbenog retka i slanje upita grozd iz nje. Upute za slanje upita grozd na konzoli Hadoop naredbenog retka potražite [u](machine-learning-data-science-move-hive-tables.md#submit)članku slanje vrste Hive upita.

## <a name="uniform"></a>Uniform slučajno uzorkovanje
Uniform slučajno uzorkovanje znači da ima li svaki redak u skupu podataka jednak izgledi koji se uzorkovanja. To se može implementirati dodavanjem programa rand() dodatna polja za skup podataka unutarnji "odaberite" upita i vanjski upita "odaberite" uvjet na tom polju slučajni.

Ovo je ogledni upit:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Ovdje, `<sample rate, 0-1>` određuje proporcije zapise koji korisnicima želite omogućiti uzorka.

## <a name="group"></a>Slučajno uzorkovanje po grupama

Kada uzorkovanje categorical podataka, možda želite uključiti ili isključiti sve instance neke određene vrijednosti categorical varijable. Ovo je što je namijenjena po "uzorkovanje prema grupi".
Ako, na primjer, ako imate categorical varijable "Stanje", koji sadrži vrijednosti NY MA, CA, NJ, stranicu, itd, želite zapisa isti stanja se uvijek zajedno, hoće li se su uzorkovanja ili ne.

Ovo je ogledni upit te primjere prema grupi:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Stratified uzorkovanje

Slučajno uzorkovanje je stratified vezana uz categorical varijabla za uzorke koje su vrijednosti da categorical koji se nalaze na istog omjera kao populaciju nadređenog iz kojeg ste nabavili primjere. Primjeru isti kao iznad, ako podaci sadrže podređenu populacije po državama, izgovorite NJ sadrži 100 opažanja, NY sadrži 60 opažanja i WA sadrži 300 opažanja. Ako navedete stopa stratified uzorkovanje biti 0,5, zatim uzorka u dobiti imat približno 50, 30 i 150 opažanja NJ, NY i WA odnosno.

Ovo je ogledni upit:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
            rank() over (partition by state order by rand()) as state_rank
        from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Informacije o naprednije načine uzorkovanje koje su dostupne u grozd, potražite u članku [LanguageManual uzorkovanje](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).
