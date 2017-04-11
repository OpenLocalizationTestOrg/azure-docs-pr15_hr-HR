<properties
    pageTitle="Programski retrain modela strojnog učenja | Microsoft Azure"
    description="Saznajte kako programski retrain modela i ažuriranje web-usluge za korištenje upravo obučeni modela u Azure strojnog učenja."
    services="machine-learning"
    documentationCenter=""
    authors="raymondlaghaeian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="raymondl;garye;v-donglo"/>


# <a name="retrain-machine-learning-models-programmatically"></a>Programski retrain strojnog učenja modela  

U ovom vodiču će saznati kako programski retrain programa Azure strojnog učenja web-servisa pomoću C# i servis strojnog učenja obradu izvođenja.

Nakon što ste retrained model, sljedeći vodiči Prikaži kako ažurirati modela na servisu predvidljivu web:

- Ako implementiran klasični web-servisa na portalu strojnog učenja web-servisi, potražite u članku [Retrain klasični web-servisa](machine-learning-retrain-a-classic-web-service.md). 
- Ako implementiran nove web-usluge, potražite u članku [Retrain novog web-servisa pomoću cmdleta za upravljanje strojnog učenja](machine-learning-retrain-new-web-service-using-powershell.md).

Pregled postupka retraining, potražite u članku [Retrain Model strojnog učenja](machine-learning-retrain-machine-learning-model.md).

Ako želite da biste započeli s postojećeg web-servisa novi upravitelj resursa Azure temelji, potražite u članku [Retrain postojeći servis za predvidljivu web](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Stvaranje eksperiment za osposobljavanje
 
U ovom se primjeru će koristiti "uzorka 5: procijeni vlaku, Test za klasifikaciju binarni: odrasle Dataset" iz primjera Microsoft Azure strojnog učenja. 
    
Da biste stvorili pokusa:

1.  Prijavite se u Microsoft Azure na računalu učenje Studio. 
2.  U donjem desnom kutu nadzorne ploče, kliknite **Novo**.
3.  Microsoftovi Samples odaberite uzorak 5.
4.  Da biste preimenovali pokusa, pri vrhu područja crtanja eksperiment odaberite naziv eksperiment "uzorka 5: procijeni vlaku, Test za klasifikaciju binarni: odrasle Dataset".
5.  Vrsta statistiku Model.
6.  Pri dnu pokusa platno, kliknite **Pokreni**.
7.  Kliknite **Postavke web-servisa** i odaberite **Retraining web-servisa**. 

    ![Početna eksperiment.][2]

Dijagram 2: Eksperiment inicijala.

## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Stvaranje predvidljivu eksperiment i Objavi kao članak na web-servisa  

Zatim stvorite Predicative eksperiment.

1.  Pri dnu područja crtanja eksperiment, kliknite **Postavljanje web-servisa** i odaberite **Predvidljivu web-servisa**. Sprema model kao obučeni modela i dodaje web servisa ulazni i izlazni module. 
2.  Kliknite **Pokreni**. 
3.  Nakon pokusa završi, kliknite **Implementacija web-servisa [klasični]** ili **Implementacija web-servisa [novi]**.

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Implementacija pokusa obuka kao web-servisa za osposobljavanje

Da biste retrain obučeni model, morate implementirati pokusa obuku koji ste stvorili kao Retraining web-servisa. Ovo web-servisa mora modula *Web servisa izlaz* povezani na * [Modela vlaku] [ train-model] * modul, moći ćete proizvesti nove obučeni modele.

1. Da biste vratili pokusa obuka, kliknite ikonu eksperimenata u lijevom oknu, a zatim kliknite pokusa pod nazivom statistiku modela.  
2. U okvir za pretraživanje stavki eksperiment za pretraživanje upišite web-servisa. 
3. Povucite modula *Web servisa unos* područje crtanja eksperiment te je povežite rezultat modul *Clean nema podataka* .  Na taj način retraining podataka obrađuje na isti način kao izvorne podatke obuku.
4. Dva *web-servisa izlaz* moduli povučete eksperiment područje crtanja. Povezivanje Izlaz iz modula *Vlaku Model* onu i izlaz iz modula *Vrednovati modela* na drugi. Izlaz web servisa za **Model vlaku** daje nam novog obučeni modela. Izlaz priložena **Procijeniti** model vraća Izlaz te modul koji je rezultat performanse.
5. Kliknite **Pokreni**. 

Sljedeće mora implementirati pokusa obuka kao web-servisa koja daje obučeni modela i model procjenu rezultate. Da biste to postigli, sljedeći skup akcija ovise o tome radite s web-servisa Classic ili novog web-servisa.  
  
**Klasični web-servisa**

Pri dnu područja crtanja eksperiment, kliknite **Postavljanje web-servisa** i odaberite **Implementacija web-servisa [klasični]**. Web-servisa **nadzorne ploče** prikazat će se s ključ za API-JA i API stranici pomoći za grupe za izvršavanje. Samo način obrade izvođenja može se koristiti za stvaranje obučeni modela.

**Nove web-usluge**

Pri dnu područja crtanja eksperiment, kliknite **Postavljanje web-servisa** i odaberite **Implementacija web-servisa [novi]**. Otvorit će se web-servisa Azure strojnog učenja Web servisi portal uvođenja web-stranicu servisa. Upišite naziv web-servisa odaberite plan plaćanja, a zatim kliknite **Implementiraj**. Samo način obrade izvođenja moguće je koristiti za stvaranje obučeni modela

U svakom slučaju, nakon završetka rada eksperiment dobivene tijek rada trebao bi izgledati na sljedeći način:

![Rezultat tijek rada nakon pokretanja.][4]

Dijagram 3: Rezultat nakon pokretanja tijeka rada.

## <a name="retrain-the-model-with-new-data-using-bes"></a>Retrain model novim podacima pomoću BES

Primjerice, koristite C# za stvaranje retraining aplikacije. Ogledni kod Python ili R možete koristiti i da biste izvršili taj zadatak.

Da biste uputili poziv API-ji Retraining:

1. Stvaranje konzole aplikacije C# u Visual Studio (novi -> projekta -> Windows radnu površinu -> aplikacije konzole).
2.  Prijavite se na portal strojnog učenja web-servisa.
3.  Ako radite s web-servisa klasični, kliknite **Klasični Web Services**.
    1.  Kliknite web-servisa na kojoj radite.
    2.  Kliknite zadana krajnja točka.
    3.  Kliknite **Korištenje**.
    4.  Pri dnu stranice **utrošak** , u odjeljku **Uzorak koda** kliknite **grupe**.
    5.  Prijeđite na korak 5 ovog postupka.
4.  Ako radite s nove web-usluge, kliknite **Web-servisa**.
    1.  Kliknite web-servisa na kojoj radite.
    2.  Kliknite **Korištenje**.
    3.  Pri dnu stranice utrošak, u odjeljku **Uzorak koda** kliknite **grupe**.
5.  Kopirajte ogledne C# kod za obradu izvođenja i zalijepite ih u datoteku Program.cs pazeći prostora za naziv ostaje netaknuta.

Dodavanje paketa Nuget Microsoft.AspNet.WebApi.Client kao što je navedeno u komentarima. Da biste dodali referenca Microsoft.WindowsAzure.Storage.dll, možda najprije morate instalirati biblioteka klijenta za Microsoft Azure servise za pohranu. Dodatne informacije potražite u članku [Windows servise za pohranu](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>Ažuriranje izvješća apikey

Pronađite **apikey** deklariranje.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

U odjeljku **Osnovni potrošnje informacije** stranici **utrošak** pronađite primarni ključ i njezino kopiranje **apikey** deklariranje.

### <a name="update-the-azure-storage-information"></a>Ažuriranje podataka za pohranu za Azure

Ogledni kod BES prenosi datoteke na lokalni pogon (na primjer "C:\temp\CensusIpnput.csv") Azure prostora za pohranu, obrađuje je i zapisuje rezultate Azure prostora za pohranu.  

Da biste izvršili taj zadatak, morate dohvatiti prostora za pohranu ime, ključ i spremnik podaci o računu za vaš račun za pohranu iz klasični Azure portal i ažuriranje odgovarajućih vrijednosti u kodu. 

1. Prijavite se na portal klasični Azure.
1. U stupcu lijevom navigacijskom oknu kliknite **prostora za pohranu**.
1. Na popisu računa za pohranu, odaberite jednu da biste pohranili retrained modela.
1. Pri dnu stranice kliknite **Upravljanje pristupnih tipki**.
1. Kopiranje i spremanje za **Primarni ključ za pristup** i zatvorite dijaloški okvir. 
1. Pri vrhu stranice kliknite **spremnika**.
1. Odaberite postojeći spremnik ili stvorite novi i spremite naziv.

Pronađite deklaracija *StorageAccountName*, *StorageAccountKey*i *StorageContainerName* i ažuriranje vrijednosti koje ste spremili na portalu Azure.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name
            
Također morate osigurati Ulazna datoteka je dostupna na mjestu koje navedete u kod. 

### <a name="specify-the-output-location"></a>Navedite mjesto za izlazne podatke

Prilikom određivanja mjesto za izlazne podatke u opseg zahtjev, proširenje otvorenu datoteku navedenu u *RelativeLocation* mora biti naveden kao ilearner. 

Potražite u sljedećem primjeru:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

>[AZURE.NOTE] Nazivi lokacijama izlaz mogu se razlikovati od onih u prikazu temelji se na redoslijedu dodali module za izlaz web servisa. Budući da postavljanje ovaj tečaj eksperimentiranje s dva izlaze rezultati obuhvatiti informacije o mjestu prostor za pohranu za oba.  

![Retraining Izlaz][6]

Dijagram 4: Retraining izlaz.

## <a name="evaluate-the-retraining-results"></a>Analiza Retraining rezultata
 
Kada pokrenete aplikaciju, izlaz sadrži URL-ove i SAS token potrebne da biste pristupili procjenu rezultate.

Vidjet ćete rezultate performanse retrained modela kombiniranje *BaseLocation*, *RelativeLocation*i *SasBlobToken* među rezultatima izlaz za *output2* (kao što je prikazano na prethodni retraining izlaz slici) i lijepljenjem cijeli URL u adresnoj traci preglednika.  

Pregledajte rezultate da biste odredili izvodi hoće li se upravo obučeni model dovoljno dobro da biste zamijenili postojeće.

Kopiranje *BaseLocation*, *RelativeLocation*i *SasBlobToken* u rezultatima Izlaz, će ih koristiti tijekom postupka retraining.

## <a name="next-steps"></a>Daljnji koraci

Ako predvidljivu web-usluge implementiran tako da kliknete **Implementacija web-servisa [klasični]**, potražite u članku [Retrain klasični web-servisa](machine-learning-retrain-a-classic-web-service.md).

Ako predvidljivu web-usluge implementiran tako da kliknete **Implementacija web-servisa [novi]**, potražite u članku [Retrain novog web-servisa pomoću cmdleta za upravljanje strojnog učenja](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/