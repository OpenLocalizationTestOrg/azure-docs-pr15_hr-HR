<properties
    pageTitle="Korak 3: Stvaranje novog eksperiment strojnog učenja | Microsoft Azure"
    description="Korak 3 od razvoju predvidljivu rješenje vodič: Stvaranje novog eksperiment obuka u Azure strojnog učenja Studio."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Korak vodič 3: Stvaranje novog eksperiment Azure strojnog učenja

Ovo je na treći korak u prikazu [razvoju rješenja predvidljivu analize u Azure strojnog učenja](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Stvaranje radnog prostora za učenje za računala](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Prijenos postojećih podataka.](machine-learning-walkthrough-2-upload-data.md)
3.  **Stvaranje nove eksperiment**
4.  [Obuka i analiza u modelima](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Implementacija web-usluge](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Pristup web-usluge](machine-learning-walkthrough-6-access-web-service.md)

----------

Sljedeći korak u ovom vodiču je da biste stvorili pokusa u strojnog učenja Studio koja koristi skup podataka ne možemo prenijeti.  

1.  U Studio, kliknite **+ NOVO** pri dnu prozora.
2.  Odaberite **EKSPERIMENT**, a zatim odaberite "Prazne eksperiment". Odaberite zadani naziv eksperiment pri vrhu područja crtanja i preimenujte je nešto smisleni

    > [AZURE.TIP] Nije dobro ispunite **Sažetak** i **Opis** za pokusa u oknu **Svojstva** . Tih svojstava dati prilika za dokument pokusa tako da svatko tko razmatra ga kasnije razumijevanje ciljeve i methodology.

3.  U paleti modulu s lijeve strane pokusa platno, proširite **Spremili skupova podataka**.
4.  Pronađite skup podataka koju ste stvorili u odjeljku **Moje skupova podataka** i povucite ga na područje crtanja. Ako unesete naziv u okvir za **pretraživanje** iznad palete možete pronaći i skupu podataka.  

##<a name="prepare-the-data"></a>Priprema podataka
Najprije 100 redaka podataka i neke statističke podatke za cijeli skup podataka možete pogledati tako da kliknete izlaz je priključak za skup podataka (malog kruga pri dnu), a zatim odaberete **Vizualiziraj**.  

Budući da podatkovnu datoteku stigle zaglavlja stupaca, Studio razvio je generički naslove (stupcu 1 st.2, *itd.)*. Dobar naslovi nisu ključni za stvaranje modela, no mogu olakšati rad s podacima u pokusa. Osim toga, kad ne možemo naposljetku objavite ovaj model u web-servisa, naslove pomoći će identificirali stupce korisniku servisa.  

Ne možemo možete dodati naslove stupaca pomoću [Uređivanje metapodataka] [ edit-metadata] modul.
Koristite [Uređivanje metapodataka] [ edit-metadata] modul da biste promijenili metapodataka povezanih sa skup podataka. U ovom slučaju pruža dodatne neslužbeni nazive stupaca. 

Da biste koristili [Uređivanje metapodataka][edit-metadata], najprije odredite koje stupce da biste izmijenili (u ovom slučaju, sve ih.) Zatim navedite akcije koje je potrebno izvršiti tih stupaca (u ovom slučaju promjena zaglavlja stupaca.)

1.  U paleti modulu upišite "metapodataka" u okvir za **pretraživanje** . Prikazat će se [Uređivanje metapodataka] [ edit-metadata] pojavljuju se na popisu modul.
2.  Kliknite i povucite [Uređivanje metapodataka] [ edit-metadata] modula na područje crtanja i ispustite ga ispod skup podataka dodali smo neke starije verzije.
3.  Povezivanje skupu podataka [Uređivanje metapodataka][edit-metadata]: kliknite izlaz je priključak za skup podataka (male krug pri dnu skupu podataka.) Nakon toga povucite ulazni priključak [Uređivanje] metapodataka[ edit-metadata] (male krug pri vrhu modul), otpustite tipku miša. Skup podataka i modul povezano čak i ako se krećete ili u području crtanja.

    Stalna trebala izgledati ovako:  

    ![Dodavanje metapodataka za uređivanje][2]
    
    Crveni uskličnik označava da ćemo niste postavili svojstva za ovaj modul još. Ćemo učiniti tu dalje.
    
    > [AZURE.TIP] Komentar možete dodati u modulu tako da dvokliknete modul i unos teksta. To može vam pomoći potražite u članku na prvi pogled što modul radi u vašem eksperiment. U ovom slučaju, dvokliknite [Uređivanje metapodataka] [ edit-metadata] modul i vrsta komentara "Dodavanje zaglavlja stupaca". Kliknite bilo gdje drugdje na područje crtanja da biste zatvorili tekstni okvir. Da biste prikazali komentar, kliknite strelicu prema dolje na modul.

4.  Odaberite [Uređivanje metapodataka][edit-metadata], zatim u oknu **Svojstva** s desne strane područje crtanja, kliknite **pokretanje birač stupca**.
5.  U dijaloškom okviru **Odabir stupaca** odaberite sve retke u **Dostupni stupci** i kliknite > da biste premjestili **Odabrane stupce**.
Dijaloški okvir trebao bi izgledati ovako: ![birač stupca s svih odabranih stupaca][4]
7.  Kliknite kvačicu **u redu** .
8.  Vratite se u oknu **Svojstva** potražite parametar **nove nazive stupaca** . U ovom polju unesite popis imena 21 stupaca u skupu podataka, odvojene zarezima i redoslijed stupaca. Nazivi stupaca možete dobiti od u dokumentaciji za skup podataka na web-stranici UCI ili pogodnost možete kopirati i zalijepiti na sljedećem popisu:  

          Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  

    U oknu svojstava izgledat će ovako:

    ![Svojstava metapodataka za uređivanje][1]

> [AZURE.TIP] Ako želite provjeriti zaglavlja stupaca, pokrenite pokusa (kliknite **POKRENI** ispod područja crtanja eksperiment). Kada se završi s radom (Zelena kvačica će se pojaviti na [Uređivanje metapodataka][edit-metadata]), kliknite izlazni priključak [Uređivanje metapodataka] [ edit-metadata] modul i odaberite **Vizualiziraj**. Izlaz iz bilo koje modul možete pogledati na isti način za prikaz tijeka podataka putem pokusa.

##<a name="create-training-and-test-datasets"></a>Stvaranje obuka i testiranje skupova podataka

Sljedeći korak pokusa je da biste generirali zasebnom skupove podataka ćemo koristiti za obuku i testiranje našem modelu.

Da biste to učinili, koristimo [Razdijeljeni podaci] [ split] modul.  

1.  Pronalaženje [Podataka Podijeli] [ split] modula, povucite ga na područje crtanja i povežite ga s posljednje [Uređivanje metapodataka] [ edit-metadata] modul.
2.  Prema zadanom omjer Podijeli je 0,5, a zatim **Podjela Randomized** parametar postavljen. To znači da je slučajni polovicu podataka izlaza kroz jedan priključak [Razdijeljeni podaci] [ split] modul i pola do druge. Možete prilagoditi te, kao i parametar **slučajna Početni broj** da biste promijenili Podjela između obuka i testiranje podataka. U ovom primjeru ostat će ih kao-je.
    
    > [AZURE.TIP] Svojstvo **razlomak retke u prvi skup podataka za izlaz** određuje koliko se podaci su poslani putem priključka lijevom izlaz. Na primjer, ako postavite omjer 0.7, zatim 70% podataka je izlaz putem lijevom priključka i 30% putem desnom priključka.  
    
3. Dvokliknite [Podjelu podataka] [ split] modul i unesite komentar, "obuka/testiranje podataka Podjela 50%". 

Koristimo izlaze [Razdijeljeni podaci] [ split] modul Međutim smo sviđa, ali ćemo odlučite koristiti lijevom izlaz kao što je obuka podatke i desne poslati kao testiranja podatke.  

Kao što je rečeno na web-stranici UCI trošak misclassifying rizika visoke odobrenja kao malo je pet puta veći od troškova misclassifying rizika malo odobrenja kao visoko. Na račun za to, ćemo stvoriti novi skup podataka koja odražava Ova funkcija trošak. U novi skup podataka svaki primjer visoke rizika je replicirati pet puta dok se svaki primjer rizika malo je replicirati.   

Možemo ovaj replikacije pomoću koda za R:  

1.  Pronađite i povucite [Izvršavanje skripte R] [ execute-r-script] modul premjestite pokusa platno i povezivanje lijevom izlazni priključak [Podjelu podataka] [ split] modul prvog unosa priključak ("Dataset1") [Izvršavanje skripte R] [ execute-r-script] modul.
2. Dvokliknite [Izvršavanje skripte R] [ execute-r-script] modulu, a zatim unesite komentar, "Postavljanje prilagođavanje troškova".
2.  U oknu **Svojstva** izbrišite zadani tekst u parametru **R skripte** i unesite ovu skriptu:

          dataset1 <- maml.mapInputPort(1)
          data.set<-dataset1[dataset1[,21]==1,]
          pos<-dataset1[dataset1[,21]==2,]
          for (i in 1:5) data.set<-rbind(data.set,pos)
          maml.mapOutputPort("data.set")


Potrebna za ovu istu operaciju za svaki izlaz [Razdijeljeni podaci] [ split] modul tako da se podaci obuke i testiranja imati isti prilagodbe troškova.

1.  Desnom tipkom miša kliknite [Izvršavanje skripte R] [ execute-r-script] modul i odaberite **Kopiraj**.
2.  Desnom tipkom miša kliknite područje crtanja eksperiment, a zatim odaberite **Zalijepi**.
3.  Povezivanje prvog unosa priključak za [Izvršavanje skripte R] [ execute-r-script] modul udesno izlaz priključak [Razdijeljeni podaci] [ split] modul. 
4.  Pri dnu područja crtanja, kliknite **Pokreni**. 

> [AZURE.TIP] Kopiju modul izvršavanje skripte R sadrži iste skriptu kao izvorna modul. Kada kopirate i zalijepite modul u području crtanja, kopija zadržava svojstva u izvorno stanje.  

Naš eksperiment sada izgleda otprilike ovako:

![Dodavanje modula za podjelu i R skripti][3]

Dodatne informacije o korištenju R skripte u vašem eksperimenata potražite u članku [Proširi vaše eksperimentiranje s R](machine-learning-extend-your-experiment-with-r.md).

**Sljedeće: [vlaku i procjenu u modelima](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
