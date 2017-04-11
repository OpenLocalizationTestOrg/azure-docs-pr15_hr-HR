<properties
   pageTitle="Izračunavanje kontekst mogućnosti R poslužitelja na HDInsight (pretpregled) | Microsoft Azure"
   description="Dodatne informacije o različitim računalnim kontekst mogućnosti dostupne korisnicima s poslužiteljem R na HDInsight (pretpregled)"
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="jhubbard"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="10/18/2016"
   ms.author="jeffstok"
/>

# <a name="compute-context-options-for-r-server-on-hdinsight-preview"></a>Izračunavanje kontekst mogućnosti R poslužitelja na HDInsight (pretpregled)

Poslužitelj R Microsoft Azure HDInsight (pretpregled) pruža najnovije mogućnosti za analizu koji se temelji na R. Koristi podatke koji su pohranjenu u HDFS u spremniku računa za spremište(../storage/storage-introduction.md "blobova platforme Azure") [Blobova platforme Azure]ili u lokalnom sustavu datoteka Linux. Budući da je poslužitelj R utemeljena na Otvori izvor R, stvaranja aplikacije utemeljene na R možete koristiti bilo koji od Otvori izvor R paketa 8000 +. Također možete koristiti postupke u [ScaleR](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "Revolucija analize ScaleR"), tvrtke Microsoft velikih skupova podataka analize paket koji je uključen u R poslužitelja.  

Čvor rub od Premium klaster omogućuje praktično da bi povezao klaster i pokrenite skripte R. S čvor na rub imate mogućnost pokretanja ScaleR na parallelized raspodijeljeno funkcije preko jezgri čvor rubni poslužitelj. Imate mogućnost da biste pokrenuli ih preko čvorove klaster pomoću ScaleR, Hadoop karte smanjiti ili Spark izračunati konteksta.

## <a name="compute-contexts-for-an-edge-node"></a>Izračunavanje konteksta za na rub čvor

Općenito govoreći, skripte R koji je pokrenut na poslužitelju R na rub čvor pokreće unutar tumačenja R taj čvor. Iznimka je ti koraci koje mogu pozivati funkcije ScaleR. Pozivi ScaleR pokrenuti u okruženju računalnim koja ovisi o postavkama računalnim kontekst ScaleR.  Kada pokrenete skriptu R je čvorovi rub, mogućim vrijednostima parametra kontekst računalnim su lokalni uzastopnih ('lokalni"), lokalne paralelno ("localpar"), smanjite karte i Spark.

Mogućnosti 'lokalni' i 'localpar' razlikuju se samo u kako se izvode rxExec pozive. Kad oba izvršiti ostale pozive rx funkcija paralelno način preko sve dostupne jezgri osim ako je naveden u suprotnom putem korištenje ScaleR numCoresToUse mogućnost, primjerice rxOptions(numCoresToUse=6). Sljedeće navedene su različite mogućnosti kontekst računalnim

| Kontekst za izračun  | Kako postaviti                      | Kontekst izvođenja                                                                     |
|------------------|---------------------------------|---------------------------------------------------------------------------------------|
| Lokalno nizu | rxSetComputeContext('local')    | Parallelized izvođenja preko u jezgri rubni poslužitelj čvor, osim rxExec poziva koji se izvode serijskog spajanja |
| Lokalni paralelno   | rxSetComputeContext('localpar') | Parallelized izvođenja preko u jezgri rubni poslužitelj čvor                                 |
| Spark            | RxSpark()                       | Parallelized raspodijeljeno izvršavanje putem Spark preko čvorove HDI klaster      |
| Smanjivanje karte       | RxHadoopMR()                    | Parallelized raspodijeljeno izvršavanje putem karte smanjiti preko čvorove HDI klaster |


Uz pretpostavku da biste željeli parallelized izvršavanja u svrhu performanse, zatim postoje tri mogućnosti. Mogućnost koju ćete odabrati ovisi o prirode učinjeno analize i veličinu i mjesto podataka.

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Smjernice za odabiru računalnim kontekstu

Trenutno postoji bez formulu koja kaže da koji kontekst koristiti za izračun. Postoje, međutim, neke guiding načela kojih možete učiniti odabir ili barem olakšavaju sužavanje izbore prije pokretanja programa usporednih. Navođenjem načela uključuju:

1.  Lokalni sustav datoteka Linux je brže od HDFS.
2.  Koji se ponavljaju analiza su brže ako su podaci lokalne i ako je u XDF.
3.  Preporučuje se da strujanje manjih količina podataka iz izvora podataka tekst; Ako je veće količine podataka, pretvorite ga u XDF prije no što analize.
4.  Indirektni kopiranje ili strujanja podataka na rub čvor za analizu postaje unmanageable za vrlo velike količine podataka.
5.  Spark je brže nego karte smanjiti za analizu u Hadoop.

Dani tih načela, neke opće pravila palca za odabir kontekstu računalnim su:

### <a name="local"></a>Lokalni

- Ako količinu podataka da biste analizirali mala, a ne zahtijeva analiza koji se ponavljaju, strujanjem izravno u Rutina analiza i koristiti 'lokalni' ili 'localpar'.
- Ako količinu podataka da biste analizirali je male i srednje veličine i zahtijeva analiza koji se ponavljaju, zatim kopiranje u lokalnom sustavu datoteka, uvoz u XDF i analizirati putem 'lokalni' ili 'localpar'.

### <a name="hadoop-spark"></a>Hadoop Spark

- Ako je prevelik količinu podataka za analizu, uvezite ih XDF u HDFS (osim ako je za pohranu problem), i analizirati putem 'Spark'.

### <a name="hadoop-map-reduce"></a>Smanjivanje Hadoop karte

- Koristite samo Ako naiđete na insurmountable problem s računalnim kontekst Spark jer obično bit će sporije.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Pomoć u istoj razini rxSetComputeContext

Dodatne informacije i primjeri konteksta računalnim ScaleR potražite u članku umetnute pomoći u R na način rxSetComputeContext, primjerice:

    > ?rxSetComputeContext

Možete se referirati na "ScaleR Distributed računalstvo vodič" koji je dostupan [R poslužitelja MSDN]biblioteke(https://msdn.microsoft.com/library/mt674634.aspx "R poslužitelja na MSDN-u") .


## <a name="next-steps"></a>Daljnji koraci

U ovom se članku naučili kako stvoriti novi klaster HDInsight koji uključuje R poslužitelja. Također naučili osnove o korištenju konzole za R iz sesiju SSH. Sada možete pročitati u sljedećim člancima da biste otkrili drugi načini rada s poslužiteljem R na HDInsight:

- [Pregled R poslužitelj za Hadoop](hdinsight-hadoop-r-server-overview.md)
- [Početak rada s poslužiteljem R za Hadoop](hdinsight-hadoop-r-server-get-started.md)
- [Dodavanje poslužitelja RStudio HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)
- [Azure mogućnosti prostora za pohranu za poslužitelj R na HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
