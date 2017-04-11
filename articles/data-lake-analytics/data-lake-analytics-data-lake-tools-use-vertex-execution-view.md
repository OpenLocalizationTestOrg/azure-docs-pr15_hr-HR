<properties 
   pageTitle="Korištenje prikaza izvođenja vrh u alatima za Lake podataka za Visual Studio | Microsoft Azure" 
   description="Saznajte kako koristiti prikaz izvođenja vrh na ispitnim analize podataka Lake poslove." 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/13/2016"
   ms.author="jgao"/>

# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Korištenje prikaza vrh izvođenja u alatima za Lake podataka za Visual Studio

Saznajte kako koristiti prikaz izvođenja vrh na ispitnim analize podataka Lake poslove.

## <a name="prerequisites"></a>Preduvjeti

- Neki osnovni znanja korištenja Data Lake Tools za Visual Studio za razvoj U SQL skripta.  U odjeljku [Praktični vodič: razvoj U SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Otvorite prikaz izvođenja vrh

Za određene posao, možete kliknuti vezu "Vrh izvođenja prikaz" u donjem lijevom kutu. Možda ćete dobiti upit za prvo učitavanje profila, a može potrajati neko vrijeme, ovisno o mogućnosti povezivanja s mrežom.

![Analize podataka Lake Alati vrh izvođenja prikaza](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Upoznavanje s prikazom izvođenja na vrh

Nakon unosa izvođenja prikaz vrh, postoje tri dijela:

![Analize podataka Lake Alati vrh izvođenja prikaza](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

- Vrh birač: na lijevoj strani je birač vrh.  Možete odabrati na vrhovi značajke (kao što su prvih 10 podataka čitati ili odaberite po fazi).

    Jedna od najčešće korištenih filtri je vrhovi na put od ključne važnosti. Put od ključne važnosti je najdulje put U SQL posla. Je korisno za optimiziranje poslova potvrđivanjem koji vrh potrebno najdulje vremena.

- U gornjem srednjem oknu:

    ![Analize podataka Lake Alati vrh izvođenja prikaza](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

    Ova prikaz prikazuje i status izvodi sve vrhovi. Pretvara vrijeme sukladno tome na lokalnom računalu i prikazuje drugi status u raznim bojama.

- U oknu centra dna:

    ![Analize podataka Lake Alati vrh izvođenja prikaza](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

    - Naziv procesa: Naziv instance vrh. Sastoji se od različite dijelove u StageName | VertexName | VertexRunInstance. Na primjer, [62] .v1 vrh SV7_Split označava drugi pokrenute instance (.v1, indeksa počevši od 0) vrh broja 62 u fazi SV7_Split.
    - Ukupna podataka čitanje/pisanje: Podataka nije čitanje/napisao ovaj vrh.
    - Država/izlaz Status: Konačni status kada vrh je završen.
    - Izlaz vrsta kod/pogreška: Pogreška pri vrh nije uspjelo.
    - Stvaranje Razlog: Zašto vrh stvorena.
    - Resurs kašnjenje/postupak kašnjenje/BD reda čekanja Latencija: na vrijeme potrebno za vrh ćete morati pričekati resursa za obradu podataka, a da ostanete u redu čekanja.
    - Postupak/autora GUID: GUID za trenutno pokrenute vrh ili autoru.
    - Verzija: N-tog instanci izvodi vrh (sustav možda zakazivanje nove instance na vrh za izračunavanje mnogo je razloga zbog, na primjer prebacivanje zalihosti, itd.)
    - Verzija se stvara vrijeme.
    - Obradu stvaranje Start vrijeme/postupak Queued vrijeme/postupak Start vrijeme/postupak dovršeno vremena: kada se pokrene proces vrh stvaranja; Kada se postupak vrh pokrene u red čekanja; Kada se određeni proces vrh pokrene; Kada se završi određene vrh.

## <a name="next-steps"></a>Daljnji koraci

- Da biste dobili pregled analize podataka Lake, potražite u članku [Pregled Azure podataka Lake analize](data-lake-analytics-overview.md).
- Prvi koraci u razvoju aplikacija U SQL, potražite u članku [razviti U – SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Da biste saznali U SQL, potražite u članku [Početak rada s jezikom Azure podataka Lake analize U-SQL](data-lake-analytics-u-sql-get-started.md).
- Upravljanje zadacima, potražite u članku [Upravljanje Lake Analytics za Azure podataka pomoću portala za Azure](data-lake-analytics-manage-use-portal.md).
- Da biste se prijavili Dijagnostika informacije potražite u članku [Accessing dijagnostički zapisnici Lake analitičkih podataka za Azure](data-lake-analytics-diagnostic-logs.md)
- Da biste vidjeli složeniji upit, potražite u članku [zapisnika analiza web-mjesta pomoću Azure podataka Lake analize](data-lake-analytics-analyze-weblogs.md).
- Prikaz detalja o posla, potražite u članku [Korištenje preglednika za posao i prikaz posla za Azure podataka lake analize poslove](data-lake-analytics-data-lake-tools-view-jobs.md)