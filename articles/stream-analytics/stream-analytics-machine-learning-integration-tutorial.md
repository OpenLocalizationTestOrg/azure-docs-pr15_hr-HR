<properties
    pageTitle="Analiza šalju pomoću analize strujanje Azure i Azure strojnog učenja | Microsoft Azure"
    description="Kako koristiti korisnički definirana funkcija i strojnog učenja u analize toka posla"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>


<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/04/2016" 
    ms.author="jeffstok"
/>

# <a name="sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Analiza šalju pomoću analize strujanje Azure i Azure strojnog učenja #

Ovaj članak namijenjen je da vam brzo postaviti jednostavan Azure strujanje analize posao, uz integraciju Azure stroj Upoznavanje sa. Će pomoću modela strojnog učenja analize šalju iz galerije Cortana inteligenciju radi analize podataka strujanje tekst i određivanje rezultatu šalju u stvarnom vremenu. Informacije u ovom članku omogućuju razumijevanje scenariji kao što su analitičke podatke u stvarnom vremenu šalju na strujanja podataka Twitter, analizirati zapise kupaca razgovori s drugom osoblju za podršku i procjenu komentara na forume, blogove i videozapisi, osim mnogo drugih u stvarnom vremenu, predvidljivu bilježenja rezultata scenarija.

U ovom se članku daje oglednu CSV datoteku s tekstom kao ulaz u spremište blobova platforme Azure prikazano na sljedećoj slici. Posao odnosi analize modela šalju kao korisnički definirane funkcije (UDF) na oglednih podataka tekst iz spremišta blobova platforme. Rezultat nalazi se u isto spremište blobova platforme u drugu CSV datoteku. 

![Analitički strujanje na računalu učenje](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

Sljedeća slika prikazuje tu konfiguraciju. Realističniji scenariju možete zamijeniti blobova strujanje Twitter podatke iz ulazni koncentratorima Azure događaj. Uz to, nije moguće sastaviti [Microsoft Power BI](https://powerbi.microsoft.com/) u stvarnom vremenu vizualizacije od zbrajanja šalju.    

![Strujanje analize na računalu učenje](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Preduvjeti

Preduvjeti za dovršavanje zadataka koji su planirati u ovom članku su na sljedeći način:

-   Aktivnu pretplatu Azure.
-   CSV datoteku s nekim podacima u njoj. Mogu preuzeti datoteku s [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/sampleinput.csv)prikazano slika 1 ili možete stvoriti vlastitu datoteku. Za ovaj članak ne možemo pretpostavlja da koristite Ponuđeni za preuzimanje na GitHub.

Visoke razine za dovršenje zadatka planirati u ovom se članku ćete učinite sljedeće:

1.  Spremište blobova platforme Azure prenesite CSV datoteku za unos.
2.  Dodavanje model analize šalju iz galerije obavještavanje Cortana za radni prostor Azure strojnog učenja.
3.  Implementacija ovaj model kao web-servisa u radnom prostoru strojnog učenja.
4.  Stvaranje zadatka strujanje analize koja poziva ovo web-servisa kao funkcija da biste odredili šalju za unos teksta.
5.  Pokretanje analize strujanje zadatka i pridržavajte se izlaz.

## <a name="upload-the-csv-input-file-to-blob-storage"></a>Prijenos CSV datoteku za unos u spremište blobova platforme

Za ovaj korak možete koristiti CSV datoteku, kao što je jedan već određen kao preuzeti s GitHub. Da biste prenijeli datoteku možete koristiti [Explorer Azure prostora za pohranu](http://storageexplorer.com/) ili Visual Studio ili koristite prilagođeni kod. Koristimo Primjeri na temelju Visual Studio.

1.  U Visual Studio, kliknite **Azure** > **prostora za pohranu** > **Priložiti vanjskog prostora za pohranu**. Unesite **naziv računa** i **ključ za račun**.  

    ![Strujanje analize na računalu učenje, Explorer poslužitelja](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-server-explorer.png)  

2.  Proširenja prostora za pohranu priložiti u koraku 1 kliknite **Stvaranje spremnika Blob**, a zatim unesite naziv za logičke. Kada stvorite spremnik, otvorite da biste vidjeli njezin sadržaj. (Bit će prazan sada).  

    ![Strujanje analize strojnog učenja, stvorite blobova platforme](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-create-blob.png)  

3.  Da biste prenijeli CSV datoteku, kliknite **Prenesi Blob**, a zatim **datoteku s lokalnom disku**.  

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Dodavanje analize modela šalju iz galerije obavještavanje Cortana

1.  Preuzimanje [predvidljivu šalju analize modela](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) iz galerije obavještavanje Cortana.  
2.  U strojnog učenja Studio, kliknite **Otvori u Studio**.  

    ![Strujanje analize strojnog učenja, Otvori Studio strojnog učenja](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3.  Prijavite se na idite u radni prostor. Odaberite mjesto na koji najbolje odgovara vlastite lokacije.
4.  Kliknite **Pokreni** pri dnu stranice.  
5.  Nakon izvođenja postupka uspješno, kliknite **Implementacija web-servisa**.
6.  Model analize šalju spremna je za korištenje. Da biste provjerili valjanost, kliknite gumb za **testiranje** i omogućuju unos teksta, kao što "Te Microsoft." Test mora vratiti rezultat sličnu ovoj:

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`  

![Strujanje analize stroj učenje analiza podataka](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)  

U stupcu **aplikacije** kliknite vezu za **Excel 2010 ili starijim radne knjige** da biste dobili ključ za API i URL koji morat ćete je kasnije da biste postavili poslova strujanje analize. (Ovaj korak potreban je samo za korištenje strojnog učenja model s radnim prostorom za neki drugi račun za Azure. U ovom se članku pretpostavlja da je to slučaj, da biste riješili taj scenarij.)  

Zapamtite web servisa URL-a i pristup ključ iz preuzete datoteke programa Excel, kao što je prikazano u nastavku:  

![Strujanje analize stroj učenje brzi pogled](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Stvaranje zadatka strujanje analize koja koristi strojnog učenja modela

1.  Idite na [portal za Azure](https://manage.windowsazure.com).  
2.  Kliknite **Novi** > **Data Services** > **strujanje analize** > **brzo stvaranje**. Unesite naziv za svoj posao u **Naziv zadatka**, unesite odgovarajuću regiju za posao u **regiji**, a zatim račun na **Računu regionalne nadzor prostora za pohranu**.    
3.  Nakon što je posao stvorili, na kartici **unosa** kliknite **Dodaj ulazni**.  

    ![Strujanje analize strojnog učenja, dodajte strojnog učenja unos](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-input-screen.png)  

4.  Na prvoj stranici čarobnjaka za **Dodavanje unos** kliknite **strujanje podataka**, a zatim kliknite **Dalje**. Na sljedećoj stranici kliknite **Spremište blobova platforme** kao unos, a zatim kliknite **Dalje**.  
5.  Na stranici **Postavke spremište blobova platforme** čarobnjaka navedite prostora za pohranu blob spremnik naziv računa koje ste definirali ranije kada ste prenijeli podatke. Kliknite **Dalje**. Za **Događaj serijaliziranog oblika**, kliknite **CSV**. Prihvatite zadane vrijednosti za ostale stranice s **postavkama serijalizacije** . Kliknite **u redu**.  
6.  Na kartici **izlaze** kliknite **Dodaj u izlaz**.  

    ![Strujanje analize strojnog učenja, dodajte Izlaz](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-output-screen.png)  

7.  Kliknite **Spremište blobova platforme**, a zatim unesite iste parametre osim spremnik. Vrijednost za **unos** je konfiguriran za čitanje kontejner "test" gdje je učitana **CSV** datoteke. Za **Izlazni**, unesite "testoutput". Spremnik nazivi moraju biti različiti. Ovaj spremnik Provjerite postoji li.     
8.  Kliknite **Dalje** da biste konfigurirali na izlaz **serijalizacije postavke**. Kao i kod **unos**, kliknite **CSV**, a zatim gumb **u redu** .
9.  Na kartici **funkcije** , kliknite **Dodaj funkcije strojnog učenja**.  

    ![Strujanje analize strojnog učenja, dodajte strojnog učenja (opis funkcije)](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-ml-function.png)  

10. Na stranici **Postavke web-servisa za strojno učenje** pronađite strojnog učenja radnog prostora, web-servisa i zadana krajnja točka. Za ovaj članak primijeniti postavke ručno da bi se dobio poznavanje s konfiguracijom web-servisa za bilo koju radnog prostora kao znate URL, a imate ključ za API-JA. Unesite krajnjoj točki **URL-a** i **ključ za API-JA**. Kliknite **u redu**.    

    ![Strujanje analize strojnog učenja, strojnog učenja web-usluge](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-web-service.png)    

11. Na kartici **upit** izmijenili upit na sljedeći način:    

 ```
    WITH subquery AS (  
        SELECT text, sentiment(text) as result from input  
    )  
 
    Select text, result.[Scored Labels]  
    Into output  
    From subquery  
 ```    
12. Kliknite **Spremi** da biste spremili upit.

## <a name="start-the-stream-analytics-job-and-observe-the-output"></a>Pokretanje analize strujanje zadatka i pridržavajte se Izlaz

1.  Kliknite **Start** pri dnu stranice posao.
2.  U **Dijaloški okvir pokretanje Query**kliknite **Prilagođeno vrijeme**, a zatim jedan istovjetan kada ste prenijeli na CSV spremište blobova platforme. Kliknite **u redu**.  

    ![Strujanje analize strojnog učenja, prilagođeni put](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-custom-time.png)  

3.  Pomoću alata za koji ste koristili za prijenos CSV datoteku, na primjer, Visual Studio, idite na spremište blobova platforme.
4.  Nekoliko minuta nakon pokretanja posla, stvara spremnik izlazne i on se prenosi u CSV datoteku.  
5.  Otvorite datoteku u uređivaču CSV zadani. Trebala bi se prikazati nešto slično sljedećem:  

    ![Prikaz toka analize strojnog učenja, CSV](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

## <a name="conclusion"></a>Zaključak

U ovom se članku objašnjava kako stvoriti strujanje analize zadatak koji se čita strujanje tekstne podatke i primjenjuje šalju analize podataka u stvarnom vremenu. Možete izvršiti sve to bez obzira na to razmišljati o intricacies izgradnje model šalju analize. To je jedna od prednosti obavještavanje paket Cortana.

Također možete pogledati metriku vezane uz funkcija Azure strojnog učenja. Da biste to učinili, kliknite karticu **Monitor** . Prikazuju se tri metriku vezane uz (opis funkcije).  

- **Funkcija zahtjeve** označava broj zahtjeva poslanih u web-servisa strojnog učenja.  
- **Funkcija događaje** označava broj događaja u zahtjevu za. Prema zadanim postavkama, svaki zahtjev za web-servisa za strojno učenje sadrži do 1000 događaja.  
- **Funkcija zahtjeva** označava broj neuspjelih zahtjeva za web-servis strojnog učenja.  

    ![Strujanje analize strojnog učenja, strojnog učenja praćenje prikaz](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-monitor-view.png)  
