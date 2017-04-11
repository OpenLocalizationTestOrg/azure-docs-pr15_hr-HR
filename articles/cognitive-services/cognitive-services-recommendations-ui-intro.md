<properties
    pageTitle="Stvaranje model s korisničkim Sučeljem Recommnendations | Microsoft Azure"
    description="Azure strojnog učenja preporuke – stvaranje model s korisničkim Sučeljem preporuke"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="luisca"/>

# <a name="building-a-model-with-the-recommendations-ui"></a>Stvaranje modela pomoću korisničkog Sučelja za preporuke

Ovaj je dokument Postupni vodič. Naš cilj je za vas voditi kroz potrebne korake za uvježbati modela pomoću [Korisničkog Sučelja za preporuke](https://recommendations-portal.azurewebsites.net/).
Prolaze kroz na vježbu omogućuje vam da biste shvatili postupak za izgradnju model prije je programski. Ga i familiarizes ste s korisničkim Sučeljem, koji je korisno prilikom početka korištenja usluge.

Ovu vježbu traje 30 minuta.

<a name="Step1"></a>
## <a name="step-1---sign-in-to-the-recommendations-ui"></a>Korak 1 - znak za preporuke korisničkog Sučelja ##

1. Ako ste još niste učinili, morat ćete [registracije](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) za novu pretplatu za [Preporuke API -JA](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) . Prema gore za servis **Azure** pri [http://portal.azure.com/](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) i prijaviti se mogu se prijaviti pomoću računa za Azure. Detaljne upute postupka registracije isporučuju se *zadatak 1* [Priručnik za početak rada](cognitive-services-recommendations-quick-start.md) 

1. Kada dobijete **ključ** za vašu pretplatu API preporuke, idite na [Korisničko Sučelje za preporuke](https://recommendations-portal.azurewebsites.net/). 

1. Prijavite se na portal sustava tako da unesete ključ polje **Ključ računa** , a zatim gumb **Prijavi** .

    ![Preporuke korisničkog Sučelja: Prijava dijaloški okvir.][reco_signin]


<a name="Step2"></a>
## <a name="step-2---lets-gather-some-training-data"></a>Korak 2 – recimo prikupite neki podaci za osposobljavanje ##

Da biste mogli stvarati na Sastavi, modul potrebna dva dijelovi informacija: datoteka kataloga i skup korištenje datoteka. 

Datoteka kataloga sadrži informacije o stavke koja nudi uz prijenos ovlasti. Korištenje datoteka sadrži informacije o korištenja tih stavki ili transakcija iz vaše tvrtke.

Obično bi upit iz trgovine u bazi podataka te dijelovi informacija. Danas, ne možemo ste unijeli neke ogledne podatke umjesto vas tako da možete saznati kako koristiti API preporuke.

Podatke možete preuzeti iz [http://aka.ms/RecoSampleData](http://aka.ms/RecoSampleData). Kopiranje i otpakiravanje **Books.Zip** datoteku u mapu na lokalnom računalu. Na primjer, **c:\data**.

Detaljne informacije o shemi datoteka kataloga i korištenje pronaći ćete u članku [Prikupljanje podataka da biste uvježbati modela](cognitive-services-recommendations-collecting-data.md) .
 
Za ovu vježbu smo namjeravate raditi male datoteke tako da ne morate vrlo Pričekajte predug za obuku. Ako želite isprobati realističniji datoteka ne možemo ste stavili **MsStoreData.zip** koja sadrži ogledne transakcija iz Microsoft Store na [isto mjesto](http://aka.ms/RecoSampleData).

<a name="Step3"></a>
## <a name="step-3---create-a-project-and-upload-catalog-and-usage-data"></a>Korak 3 – Stvaranje projekta i prijenos kataloga i korištenje podataka ##

Nakon prijave u [Korisničkom Sučelju preporuke](https://recommendations-portal.azurewebsites.net/)vidjeli stranicu projekata. Ako ste prethodno stvorili sve projektima, trebali biste ih vidjeti ovdje.
Projekt (poznat i kao *model* u referenci API-JA) je spremnik za katalog i korištenja podataka. Možete stvoriti nekoliko *sastavlja* unutar projekta. Ne možemo će vas voditi kroz postupak u sljedećim koracima.

1. Da biste stvorili novi projekt, unesite naziv u tekstni okvir (nešto kao "MyFirstModel" daju) i kliknite **Dodaj projekt**.
 
    ![Preporuke korisničkog Sučelja: Stranica projekata.][reco_projects]

1. Nakon stvaranja dobiva projekta, kliknite gumb **potražite datoteku** na odjeljak **Dodavanje datoteke kataloga** . Prenesite kataloga dobivenog u koraku 2. Ako ste spremili na *c:\data*, morate idite u tu mapu.

    ![Preporuke korisničkog Sučelja: Dodavanje podataka u projekt.][reco_firstmodel]

1. Nakon prijenosa kataloga kliknite gumb **potražite datoteku** na odjeljak **Dodavanje korištenje datoteka** . Dodavanje datoteke usage_large.txt.

> **Što ako imam velike datoteke podataka o korištenju?**
>
> Moguće je prenijeti samo korištenje datoteke manje od 200 MB. Koje rečeno, sustav mogu sadržavati prema gore da biste 2 GB vrijedne transakcije podataka, tako da možete prenijeti više od jedne datoteke ako je potrebno.
> Ne morate to količinom podataka da biste generirali dobar model, nije samo veličinu podatke koje zapise, ali kvalitete podataka. Da biste vidjeli gdje Većina transakcije su samo na handful od najpopularnijih stavki i postoji "malo signala" za ostale stavke podataka o korištenju uobičajeno je.

<a name="Step4"></a>
## <a name="step-4---lets-do-some-training"></a>Korak 4 - recimo učinite neke obuka! ##

Sad kad ste prenijeli kataloga i podataka o korištenju, ne možemo spremni uvježbati modul tako da možete saznati uzoraka s oglednim podacima.

1.  Kliknite gumb **Sastavi novi** .

1.  Odaberite **preporuke** kao vrstu Sastavi. Obratite pozornost na to da podržavamo na rangiranje Sastavljanje i FBT (često kupili zajedno) sastavljanje kao i vrste.

    ![Preporuke korisničkog Sučelja: Izrada dijaloški okvir.][reco_build_dialog.png]


    Za sastavljanje FBT omogućuje vam da biste odredili uzorke proizvoda koji su obično kupili/potrošena u istom transakcijom.
    Rangiranje Sastavi koristi se za identificiranje značajkama koje vas zanimaju. 
    Ne možemo neće odlaze vrlo duboke FBT ili rang sastavlja u ovom radionicu, ali ako vas zanimaju treba pogledajte [Sastavljanje vrste i model kvalitete dokumentaciju stranice](cognitive-services-recommendations-buildtypes.md).

1. Kliknite gumb **Sastavi** . Sastavljanje postupak vodi pet minuta ako koristite knjige podataka. Traje dulje na veći skupovima podataka.

<a name="Step5"></a>
## <a name="step-5---lets-find-out-what-the-machine-learned"></a>Drugi korak 5 - ćemo saznati što računalu naučili! ##

Kada se vaše Sastavi dovrši, primijetit ćete novi Sastavi na popisu izdanja. Ovu međuverziju je može poslati upit za preporuke stavke i korisnika.

1. Kada se vaše Sastavi dovrši, kliknite **rezultat**. Time možete reproducirati s modelom i pregledati stavke koje se preporučuje.

    ![Preporuke korisničkog Sučelja: Gumb rezultat][reco_score_button]

1. Odaberite stavku da biste vidjeli stavke koje se vraćaju kao preporuke za tu stavku. Imajte na umu da ako nema dovoljno transakcije za predviđanje i preporuke za određenu stavku, sustav neće vratiti sve preporuke za tu stavku.  Ako zbog nekog razloga stavke koja se prikazuje bez preporukama, pokušajte bilježenje rezultata druge stavke.

<a name="Step6"></a>
## <a name="step-6---next-steps"></a>Korak 6 - sljedeći koraci ##
Čestitamo! ste put uvježbavan model, a zatim preporuke dobivenog od model.  Korisničko Sučelje za preporuke je koristan alat koji vam omogućuje da vidite stanje projekte i sastavlja. 

Sad kad imate model, preporučujemo vam da biste saznali kako programski učinite sve gore navedene korake. Da biste saznali da biste uputili poziv na API programski, Pozivamo vas da biste provjerili [Preporuke API Reference](http://go.microsoft.com/fwlink/?LinkId=759348) i preuzmite [Aplikaciju uzorka preporuke](http://go.microsoft.com/fwlink/?LinkID=759344).


[reco_signin]:../media/cognitive-services/reco_signin.PNG
[reco_projects]:../media/cognitive-services/reco_projects.PNG
[reco_firstmodel]:../media/cognitive-services/reco_firstmodel.png
[reco_build_dialog.png]:../media/cognitive-services/reco_build_dialog.png
[reco_score_button]:../media/cognitive-services/reco_score_button.png
