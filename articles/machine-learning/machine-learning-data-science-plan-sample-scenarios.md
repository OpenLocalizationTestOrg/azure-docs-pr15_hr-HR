<properties
    pageTitle="Scenariji za napredne analize postupak i tehnologije Azure strojnog učenja | Microsoft Azure"
    description="Odaberite odgovarajući scenariji za to predvidljivu analize pomoću postupka timu podataka znanosti za napredno."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na" 
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Scenariji za napredne analize u Azure strojnog učenja

U ovom se članku navode raznih izvora podataka za uzorak i ciljne scenarija koji se može riješiti [Timu podataka znanstvenog procesa (TDSP)](data-science-process-overview.md). Na TDSP nudi sustavan pristup za timovima za suradnju na stvaranje Inteligentna aplikacija. Scenariji navedene u nastavku prikazuju se mogućnosti dostupne u tijeku rada za obradu podataka koje ovise o karakteristikama podataka, mjesta izvora i ciljne spremištima u Azure.

U odjeljku zadnju raspoređene **stabla odlučivanja** za odabir ogledni Scenariji koji odgovara vašim podacima i cilj.

>[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Svaki od sljedećih odjeljaka predstavlja uzorak scenarija. Za svaki scenarij znanstvenog mogućih podataka ili naprednom analitikom navedene su tijek i Azure resurse za podršku.

>[AZURE.NOTE]**Sve od sljedećih situacija, najprije morate:**
><br/>
>* [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md)
><br/>
>* [Stvaranje radnog prostora programa Azure strojnog učenja](machine-learning-create-workspace.md)


## <a name="smalllocal"></a>Scenarij \#1: Small za srednje tablični skup podataka u lokalne datoteke

![Mali na srednje lokalne datoteke][1]

#### <a name="additional-azure-resources-none"></a>Dodatne resurse za Azure: ništa

1.  Prijavite se na [Azure strojnog učenja Studio](https://studio.azureml.net/).

2.  Prenesite skup podataka.

3.  Sastavljanje tijek eksperiment Azure strojnog učenja koja se počevši od prenesene skupova podataka.

## <a name="smalllocalprocess"></a>Scenarij \#2: Small za srednje dataset lokalne datoteke koje je potrebno obrada

![Mali na srednje lokalne datoteke s obradom][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Dodatne resurse za Azure: Azure virtualnog računala (IPython bilježnice server)

1.  Stvaranje programa Azure virtualnog računala radi IPython bilježnice.

2.  Prijenos podataka u spremniku Azure prostora za pohranu.

3.  Unaprijed postupak i čišćenje podataka u bilježnici IPython pristupa podacima iz spremnik Azure prostora za pohranu.

4.  Pretvaranje podataka da biste očistili, tabličnom obliku.

5.  Spremite transformiranih podataka u Azure blob-ova.

6.  Prijavite se na [Azure strojnog učenja Studio](https://studio.azureml.net/).

7.  Čitanje podataka iz Azure blob-ova pomoću [Uvoz podataka] [ import-data] modul.

8. Sastavljanje tijek eksperiment Azure strojnog učenja koja se počevši od ingested skupova podataka.

## <a name="largelocal"></a>Scenarij \#3: velike dataset lokalne datoteke, određivanje Azure blob-ova

![Veliki lokalne datoteke][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Dodatne resurse za Azure: Azure virtualnog računala (IPython bilježnice server)

1.  Stvaranje programa Azure virtualnog računala radi IPython bilježnice.

2.  Prijenos podataka u spremniku Azure prostora za pohranu.

3.  Unaprijed postupak i čišćenje podataka u bilježnici IPython pristupa podacima iz Azure blob-ova.

4.  Ako je potrebno, pretvaranje podataka da biste očistili, tabličnom obliku.

5.  Istraživanje podataka i stvaranje značajke prema potrebi.

6.  Izdvajanje podataka male srednje uzorka.

7.  Spremanje sampled podataka u Azure blob-ova.

8. Prijavite se na [Azure strojnog učenja Studio](https://studio.azureml.net/).

9. Čitanje podataka iz Azure blob-ova pomoću [Uvoz podataka] [ import-data] modul.

10. Sastavljanje Azure strojnog učenja eksperiment tijek počevši od ingested skupova podataka.


## <a name="smalllocaltodb"></a>Scenarij \#4: Small za srednje dataset lokalne datoteke, određivanje SQL Server na na računalu Virtal Azure

![Mali na srednje lokalne datoteke DB SQL servisu Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Dodatne resurse za Azure: Azure virtualnog računala (SQL Server / IPython bilježnice server)

1.  Stvaranje računalo virtualne Azure koja se sa sustavom SQL Server + IPython bilježnice.

2.  Prijenos podataka u spremniku Azure prostor za pohranu.

3.  Unaprijed postupak i čišćenje podataka u spremniku Azure prostora za pohranu pomoću IPython bilježnice.

4.  Ako je potrebno, pretvaranje podataka da biste očistili, tabličnom obliku.

5.  Spremanje podataka iz datoteke VM lokalne (VM radi IPython bilježnice, lokalnog pogona odnose VM pogona).

6.  Učitavanje podataka u bazu podataka sustava SQL Server na programa Azure VM.

    Mogućnost \#1: pomoću SQL Server Management Studio.

    - Prijavite se na SQL Server VM
    - Pokrenite SQL Server Management Studio.
    - Stvaranje tablica baze podataka i cilj.
    - Koristite neku od masovnog uvoz metode da biste učitali podatke iz datoteka VM lokalno.

    Mogućnost \#2: korištenje IPython bilježnice – nije Uputno za srednje i veće skupova podataka<!-- -->    
    - Pristup sustava SQL Server na VM pomoću ODBC niz za povezivanje.
    - Stvaranje tablica baze podataka i cilj.
    - Koristite neku od masovnog uvoz metode da biste učitali podatke iz datoteka VM lokalno.

7.  Istraživanje podataka, stvaranje značajke prema potrebi. Imajte na umu da se značajke ne moraju biti materialized u tablicama baze podataka. Samo Imajte na umu potrebne upit za njihovo stvaranje.

8. Odredite veličinu ogledne podatke, ako je potrebno i/ili želji.

9. Prijavite se na [Azure strojnog učenja Studio](https://studio.azureml.net/).

10. Pročitati podatke izravno iz sustava SQL Server pomoću [Uvoz podataka] [ import-data] modul. Zalijepite potrebne upit koji se izdvaja polja, stvara značajke i uzorke podataka po potrebi izravno u [Uvoz podataka] [ import-data] upita.

11. Sastavljanje Azure strojnog učenja eksperiment tijek počevši od ingested skupova podataka.

## <a name="largelocaltodb"></a>Scenarij \#5: velike skup podataka u lokalne datoteke ciljani SQL Server na Azure VM

![Veliki lokalne datoteke DB SQL servisu Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Dodatne resurse za Azure: Azure virtualnog računala (SQL Server / IPython bilježnice server)

1.  Stvaranje računalo virtualne Azure koja se sa sustavom SQL Server i poslužitelja IPython bilježnice.

2.  Prijenos podataka u spremniku Azure prostora za pohranu.

3.  (Neobavezno) Unaprijed postupak i čišćenje podataka.

    na.  Unaprijed postupak i čišćenje podataka u bilježnici IPython pristupa podacima iz Azure blob-ova.

    b.  Ako je potrebno, pretvaranje podataka da biste očistili, tabličnom obliku.

    c.  Spremanje podataka iz datoteke VM lokalne (VM radi IPython bilježnice, lokalnog pogona odnose VM pogona).

4.  Učitavanje podataka u bazu podataka sustava SQL Server na programa Azure VM.

    na.  Prijavite se na SQL Server VM.

    b.  Ako se podaci ne Spremi već, preuzmite podatkovne datoteke iz spremnik Azure prostora za pohranu za mapu lokalne VM.

    c.  Pokrenite SQL Server Management Studio.

    d.  Stvaranje tablica baze podataka i cilj.

    e.  Koristite neku od masovnog uvoz metode da biste učitali podatke.

    f.  Ako su potrebne spojeva tablice, stvorite indeksi da biste ubrzali spojeva.

     > [AZURE.NOTE] Brže učitavanje veličina velike podataka, preporučuje se stvaranje particioniranom tablica, a skupno uvoz podataka paralelno. Dodatne informacije potražite u članku [Paralelno uvoz podataka u SQL particije tablice](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Istraživanje podataka, stvaranje značajke prema potrebi. Imajte na umu da se značajke ne moraju biti materialized u tablicama baze podataka. Samo Imajte na umu potrebne upit za njihovo stvaranje.

6.  Odredite veličinu ogledne podatke, ako je potrebno i/ili želji.

7.  Prijavite se na [Azure strojnog učenja Studio](https://studio.azureml.net/).

8. Pročitati podatke izravno iz sustava SQL Server pomoću [Uvoz podataka] [ import-data] modul. Zalijepite potrebne upit koji se izdvaja polja, stvara značajke i uzorke podataka po potrebi izravno u [Uvoz podataka] [ import-data] upita.

9. Jednostavan Azure strojnog učenja eksperiment tijek počevši od prenesenu skup podataka

## <a name="largedbtodb"></a>Scenarij \#6: velike skup podataka u programa SQL Server baze podataka lokalno, određivanje SQL Server u programa Azure virtualnog računala

![Veliki SQL DB lokalno SQL DB servisu Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Dodatne resurse za Azure: Azure virtualnog računala (SQL Server / IPython bilježnice server)

1.  Stvaranje računalo virtualne Azure koja se sa sustavom SQL Server i poslužitelja IPython bilježnice.

2.  Nekim podataka izvezite načina za izvoz podataka iz sustava SQL Server datoteke ispisa.

    > [AZURE.NOTE] Ako odlučite da biste premjestili sve podatke iz baze podataka lokalno, zamjenski način (brže) da biste premjestili cijelog baze podataka instancu sustava SQL Server Azure. Preskočite korake za izvoz podataka, stvoriti bazu podataka, i Učitaj/uvoza podataka u bazu podataka ciljne i slijedite zamjenski način.

3.  Prijenos datoteka za ispis spremniku Azure prostora za pohranu.

4.  Učitavanje podataka u bazu podataka sustava SQL Server pokrenut na programa Azure virtualnog računala.

    na.  Prijavite se na SQL Server VM.

    b.  Preuzmite podatkovne datoteke iz programa Azure prostora za pohranu spremnik za mapu lokalne VM.

    c.  Pokrenite SQL Server Management Studio.

    d.  Stvaranje tablica baze podataka i cilj.

    e.  Koristite neku od masovnog uvoz metode da biste učitali podatke.

    f.  Ako su potrebne spojeva tablice, stvorite indeksi da biste ubrzali spojeva.

    > [AZURE.NOTE] Brže učitavanje veličina velike podataka, stvoriti particioniranom tablice i za skupno uvesti podatke paralelno. Dodatne informacije potražite u članku [Paralelno uvoz podataka u SQL particije tablice](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Istraživanje podataka, stvaranje značajke prema potrebi. Imajte na umu da se značajke ne moraju biti materialized u tablicama baze podataka. Samo Imajte na umu potrebne upit za njihovo stvaranje.

6.  Odredite veličinu ogledne podatke, ako je potrebno i/ili želji.

7.  Prijavite se na [Azure strojnog učenja Studio](https://studio.azureml.net/).

8. Pročitati podatke izravno iz sustava SQL Server pomoću [Uvoz podataka] [ import-data] modul. Zalijepite potrebne upit koji se izdvaja polja, stvara značajke i uzorke podataka po potrebi izravno u [Uvoz podataka] [ import-data] upita.

9. Jednostavne Azure strojnog učenja isprobati tijek počevši od prenesenu skup podataka.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Zamjenski način da biste kopirali cijeli baze podataka sustava SQL Server lokalnog s bazom podataka SQL Azure

![Odvajanje lokalne baze podataka, a zatim priložite SQL DB servisu Azure][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Dodatne resurse za Azure: Azure virtualnog računala (SQL Server / IPython bilježnice server)

Za replikaciju cijelu bazu podataka sustava SQL Server u sustava SQL Server VM, kopirajte bazu podataka s jednog mjesta/poslužitelja u drugu, pod pretpostavkom da baze podataka može biti izvanmrežno privremeno. To se u SQL Server Management Studio objekt Explorer ili koristite ekvivalentan Transact-SQL naredbe.

1. Odvajanje baza podataka na izvorno mjesto. Dodatne informacije potražite u članku [Odvoji baze podataka](https://technet.microsoft.com/library/ms191491(v=sql.110).aspx).
2. U prozoru programa Windows Explorer ili Windows naredbeni redak, kopirajte datoteku odvojeni baze podataka ili datoteke i datoteke zapisnika ili datoteke na mjesto cilja na VM poslužitelja SQL u Azure.
3. Prilaganje kopiranih datoteka instancu sustava SQL Server cilj. Dodatne informacije potražite u članku [Prilaganje baze podataka](https://technet.microsoft.com/library/ms190209(v=sql.110).aspx).

[Premještanje baze podataka pomoću odvajanje i priložite (-SQL transakcija)](https://technet.microsoft.com/library/ms187858(v=sql.110).aspx)

## <a name="largedbtohive"></a>Scenarij \#7: velikih skupova podataka u lokalnom datotekama ciljani grozd baze podataka u klastere Azure HDInsight Hadoop

![Velikih skupova podataka u lokalnom ciljnoj grozd][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Dodatne resurse za Azure: Azure HDInsight Hadoop klaster i Azure virtualnog računala (IPython bilježnice server)

1.  Stvaranje programa Azure virtualnog računala izvodi server IPython bilježnice.

2.  Stvaranje programa Azure HDInsight Hadoop klaster.

3.  (Neobavezno) Unaprijed postupak i čišćenje podataka.

    na.  Unaprijed postupak i čišćenje podataka u bilježnici IPython pristupa podacima iz Azure blob-ova.

    b.  Ako je potrebno, pretvaranje podataka da biste očistili, tabličnom obliku.

    c.  Spremanje podataka iz datoteke VM lokalne (VM radi IPython bilježnice, lokalnog pogona odnose VM pogona).

4.  Prijenos podataka na zadani spremnik klaster Hadoop koji je odabran na stranici korak 2.

5.  Učitavanje podataka u bazu podataka grozd klasteru Azure HDInsight Hadoop.

    na.  Prijavite se na glavni čvor Hadoop klaster

    b.  Otvorite naredbeni redak Hadoop.

    c.  Unesite korijenski direktorij grozd naredbom `cd %hive_home%\bin` u Hadoop naredbenog retka.

    d.  Pokretanje upita grozd za stvaranje baze podataka i tablice, a učitavanje podataka iz spremišta blobova grozd tablice.

    > [AZURE.NOTE] Ako se podaci nalaze u velika, korisnici mogu stvarati tablicu vrste Hive s particije. Zatim, korisnici mogu koristiti u `for` petlje u Hadoop naredbenog retka na glavni čvor da biste učitali podatke u tablicu vrste Hive particije po particije.

6.  Istraživanje podataka i stvaranje značajke prema potrebi u Hadoop naredbenog retka. Imajte na umu da se značajke ne moraju biti materialized u tablicama baze podataka. Samo Imajte na umu potrebne upit za njihovo stvaranje.

    na.  Prijavite se na glavni čvor Hadoop klaster

    b.  Otvorite naredbeni redak Hadoop.

    c.  Unesite korijenski direktorij grozd naredbom `cd %hive_home%\bin` u Hadoop naredbenog retka.

    d.  Pokretanje upita grozd u Hadoop naredbenog retka na glavni čvor klaster Hadoop istraživati podatke i stvarati značajke prema potrebi.

7.  Ako je potrebno i/ili želji, ogledne podatke tako da stane u Azure strojnog učenja Studio.

8.  Prijavite se na [Azure strojnog učenja Studio](https://studio.azureml.net/).

9. Čitanje podataka izravno iz na `Hive Queries` pomoću [Uvoz podataka] [ import-data] modul. Zalijepite potrebne upit koji se izdvaja polja, stvara značajke i uzorke podataka po potrebi izravno u [Uvoz podataka] [ import-data] upita.

10. Jednostavne Azure strojnog učenja isprobati tijek počevši od prenesene skup podataka.

## <a name="decisiontree"></a>Stabla odlučivanja za odabir scenarija
------------------------

Na sljedećem su dijagramu navedene scenariji prethodno opisan i napredne analize postupka i tehnologije mogućnosti promjene koje će vas odvesti na svakom detaljne slučajevi. Imajte na umu da se može potrajati obradu podataka, istraživanje, značajka inženjerska i uzorkovanje potvrdite jedan ili više načina/okruženja – u izvoru, Srednja, i/ili okruženja cilj –, a možda prijeđite ponovno po potrebi. Dijagram samo služi kao ilustracija neke od mogućih tokova i navedite enumeracija iscrpan.

![Ogledna DS postupak vodič scenariji][8]

### <a name="advanced-analytics-in-action-examples"></a>Napredne analize u akciji Primjeri

Kraj do kraja Azure strojnog učenja vodiči koji uključuju napredne analize postupka i tehnologije pomoću javne skupove podataka, potražite u članku:


* [Proces znanstvenog timu podataka u akciji: pomoću SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
* [Proces znanstvenog timu podataka u akciji: korištenje klastere HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md).


[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
