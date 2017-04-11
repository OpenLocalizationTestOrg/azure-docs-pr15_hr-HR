<properties 
    pageTitle="Korištenje Azure strojnog učenja parametara servisa Web | Microsoft Azure" 
    description="Kako koristiti Azure strojnog učenja Web servisa parametara za promjenu načina funkcioniranja modela prilikom pristupa web-servisa." 
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
    ms.author="raymondl;garye"/>

#<a name="use-azure-machine-learning-web-service-parameters"></a>Korištenje Azure strojnog učenja Web servisa parametara

Objavljivanjem pokusa sadrži modula s konfigurirati parametrima stvara se na web-servisa Azure strojnog učenja. U nekim slučajevima, preporučujemo vam da biste promijenili ponašanje modul dok se izvodi web-servisa. *Parametri servisa web* omogućuju da biste izvršili taj zadatak. 

[Uvoz podataka] postavlja uobičajeni primjer[ reader] modul tako da korisnik objavljenu web-usluge može odrediti drugog izvora podataka prilikom pristupa web-servisa. Ili konfiguriranju [Izvoz podataka] [ writer] modul tako da možete navesti različite odredište. Neki primjeri obuhvaćaju promjenu broj bitova za [Značajku raspršivanje] [ feature-hashing] modula ili broj želji značajke za [filtar temelji odabir značajki] [ filter-based-feature-selection] modul. 

Možete postaviti parametara Web servisa, a zatim ih pridružiti jedan ili više parametara modul u vašem eksperiment i odredite jesu li obavezan ili nije. Korisnik web-usluge možete pa unesite vrijednosti za parametara kada se poziv web-servisa. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##<a name="how-to-set-and-use-web-service-parameters"></a>Upute za postavljanje i korištenje parametara servisa Web

Definiranje parametar web-usluge tako da kliknete ikonu pokraj parametar modul, a zatim odaberete "Postavi kao parametar web-usluge". Stvara novu parametar web-usluge i povezuje s taj parametar modul. Zatim kada je pristupa web-servisa, korisnik može odrediti vrijednost za parametar web-usluge i primjenjuje se na parametar modul.

Kada definirate parametar web-usluge, dostupna drugim parametar modul u pokusa. Ako definirate parametar web-usluge pridružene parametar jednog modula, tu istu parametar web-usluge možete koristiti za sve druge module kao parametar očekuje istu vrstu vrijednosti. Ako, na primjer, ako parametar web-usluge nije numerička vrijednost, zatim samo korištenja za parametre modul očekivana numeričku vrijednost. Kada korisnik postavi vrijednost za parametar web-usluge, će se primijeniti da biste sve parametre pridružene modul.

Možete odlučiti želite li navesti zadanu vrijednost za parametar web-usluge. Ako to učinite, zatim parametar nije obavezno za korisnike web-servisa. Ako ne unesete zadane vrijednosti, korisnik je potreban unos vrijednosti prilikom pristupa web-servisa.

Web-servisa potražite u dokumentaciji API sadrži informacije za korisnika servisa web na programski određivanje parametar web-usluge prilikom pristupa web-servisa.

>[AZURE.NOTE] Klasični web-servisa potražite u dokumentaciji API navedeni su putem veze za **API-JA stranica za pomoć** u web-servisa **nadzorne PLOČE** Studio strojnog učenja. Nova web-servisa potražite u dokumentaciji API navedeni su putem portala za [Azure strojnog učenja web-servisa](https://services.azureml.net/Quickstart) na stranicama **utrošak** i **Swagger API -JA** za web-servisa.


##<a name="example"></a>Primjer

Na primjer, pretpostavimo pretpostavlja imamo pokusa [Izvoz podataka] [ writer] modul koji šalje informacije blobova platforme Azure. Ne možemo ćete odrediti parametar web-usluge pod nazivom "Bloba put" koji omogućuje korisnik usluge web da biste promijenili put u spremište blobova platforme kada je servis pristupa.

1.  U strojnog učenja Studio, kliknite [Izvoz podataka] [ writer] modul da biste ga odabrali. Svojstva prikazuju se u oknu svojstva s desne strane eksperiment područje crtanja.

2.  Odredite vrstu spremišta:

    - U odjeljku **rješenje određivanje odredišta podataka**, odaberite "Spremište blobova platforme Azure".
    - U odjeljku **rješenje navedite vrsta provjere autentičnosti**, odaberite "Račun".
    - Unesite podatke o računu za spremište blobova platforme Azure. 
    <p />

3.  Kliknite ikonu desno od **put do bloba počevši od spremnik parametar**. Izgleda ovako:

    ![Ikona parametra servis za web][icon]

    Odaberite "Postavi kao parametar web-usluge".

    U odjeljku **Web servisa parametara** pri dnu okna sa svojstvima pod nazivom "Put bloba počevši od spremnik" dodaje se unos. To je parametar web-usluge koji je povezan s ovim [Izvoz podataka] [ writer] parametar modul.

4.  Da biste preimenovali parametar web-usluge, kliknite naziv, unesite "Bloba put" i pritisnite tipku **Enter** . 
 
5.  Omogućuje zadanu vrijednost za parametar web-usluge kliknite ikonu s desne strane naziva, odaberite "Pružaju zadanu vrijednost", unesite vrijednost (na primjer, "container1/output1.csv") i pritisnite tipku **Enter** .

    ![Parametar web-usluge][parameter]

6.  Kliknite **Pokreni**. 

7.  Kliknite **Implementacija web-servisa** i odaberite **Implementacija web-servisa [klasični]** ili **Implementacija web-servisa [novi]** za implementaciju web-servisa.

Korisnik web-usluge možete odrediti odredište za [Izvoz podataka] [ writer] modul prilikom pristupa web-servisa.

##<a name="more-information"></a>Dodatne informacije

Primjer s više pojedinosti potražite u članku unos [Parametara servisa Web](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) [Strojnog učenja Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Dodatne informacije o pristupanju web-servisa strojnog učenja potražite [u](machine-learning-consume-web-services.md)članku zauzeti objavljene strojnog učenja web-servisa.



<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 
