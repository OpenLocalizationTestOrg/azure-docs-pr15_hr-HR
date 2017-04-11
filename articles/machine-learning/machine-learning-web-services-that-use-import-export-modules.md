<properties
    pageTitle="Implementacija Azure ML web-servisi koji koriste podatke uvoz i izvoz podataka moduli | Microsoft Azure"
    description="Saznajte kako pomoću podataka za uvoz i izvoz podataka module za slanje i primanje podataka iz web-servisa."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="v-donglo"/>



# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Implementacija Azure ML web-servisi koji koriste moduli podataka uvoz i izvoz podataka 

Kada stvarate predvidljivu eksperiment, obično dodajete web servis za unos i izlaz. Ako pokrenete pokusa, korisnici možete slati i primati podatke iz web-servisa putem unosa i izlaza. Za neke aplikacije u korisničke podatke mogu biti dostupne iz sažetka sadržaja podataka ili već nalaze u vanjskom izvoru podataka kao što je spremište blobova platforme Azure. U tim slučajevima ne moraju za čitanje i pisanje podataka pomoću web servisa unosa i izlaza. Mogu, umjesto toga koristite obradu izvođenja servisa (BES) za čitanje podataka iz izvora podataka pomoću modul za uvoz podataka i pisanje bilježenja rezultata rezultata na mjesto za različite podatke pomoću modul za izvoz podataka.

Uvoz podataka i izvoz podataka module možete čitanja i pisanja broj podataka pružaju mjesta kao što je web-URL putem HTTP-a, upitu vrste Hive, baze podataka Azure SQL, spremište tablica Azure, Azure blobova podatkovnih sadržaja ili bazu podataka sustava SQL na lokaciji.

U ovoj se temi koristi u "uzorka 5: procijeni vlaku, Test za klasifikaciju binarni: odrasle Dataset" uzorak i pretpostavlja skupu podataka već učitan u tablicu programa Azure SQL pod nazivom censusdata.

## <a name="create-the-training-experiment"></a>Stvaranje pokusa obuka 
 
Kada otvorite na "uzorka 5: procijeni vlaku, Test za klasifikaciju binarni: odrasle Dataset" uzorka koristi dataset odrasle statistiku dobit binarni klasifikacija uzorka. I pokusa u područje crtanja izgledat će slično kao na sljedećoj slici.

![Početna konfiguracija pokusa.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)
  

Da biste pročitali podatke iz tablice Azure SQL:

1.  Brisanje modul za skup podataka.
2.  U okvir za pretraživanje komponente upišite uvoz.
3.  S popisa rezultata dodajte modul za *Uvoz podataka* u područje crtanja eksperiment.
4.  Povezivanje izlaz unos modul *Clean podataka koji nedostaju* modul za *Uvoz podataka* .
5.  U oknu svojstva odaberite **Baze podataka SQL Azure** na padajućem popisu **Izvora podataka** .
6.  U okvir **naziv poslužitelja baze podataka**, **Naziv baze podataka**, **korisničko ime**i **lozinku** polja, unesite odgovarajuće podatke za bazu podataka.
7.  U polju upita baze podataka unesite sljedeći upit.

        select [age],
           [workclass],
           [fnlwgt],
           [education],
           [education-num],
           [marital-status],
           [occupation],
           [relationship],
           [race],
           [sex],
           [capital-gain],
           [capital-loss],
           [hours-per-week],
           [native-country],
           [income]
        from dbo.censusdata;

8.  Pri dnu pokusa platno, kliknite **Pokreni**.

## <a name="create-the-predictive-experiment"></a>Stvaranje predvidljivu pokusa

Sljedeći postavljanje predvidljivu pokusa iz kojeg će implementacija web-servisa.

1.  Pri dnu područja crtanja eksperiment, kliknite **Postavljanje web-servisa** i odaberite **Predvidljivu web-servisa [preporučeno]**.
2.  *Unos servisa Web* i *moduli izlaz servisa Web* uklanjanje predvidljivu pokusa. 
3.  U okvir za pretraživanje komponente upišite izvoz.
4.  S popisa rezultata dodajte modul za *Izvoz podataka* u područje crtanja eksperiment.
5.  Povezivanje izlazni *Rezultat modela* modula unos modul za *Izvoz podataka* . 
6.  U oknu svojstva odaberite **Baze podataka SQL Azure** na padajućem popisu odredište podataka.
7.  U okvir **naziv poslužitelja baze podataka**, **Naziv baze podataka**, **poslužitelj korisničko ime**i **Lozinka korisničkog računa poslužitelja** polja, unesite odgovarajuće podatke za bazu podataka.
8.  U polje **zarezom popis stupaca spremiti** upišite testu dobije naljepnice.
9.  U u **polje Naziv s tablicom podataka**, upišite vlasnika baze podataka. ScoredLabels. Ako ne postoji u tablici, stvaranja prilikom pokretanja pokusa ili se naziva web-servisa.
10. U polje **zarezom popis stupaca tablica podataka** upišite ScoredLabels.

Prilikom pisanja aplikacije koja se poziva konačni web-usluge, preporučujemo vam da biste naveli drugi unos upita ili odredišne tablice u vrijeme izvođenja. Da biste konfigurirali sljedeće unose i izlaza, koristite značajku Web servisa parametara da biste postavili svojstvo *izvor podataka* modula za *Uvoz podataka* i svojstvo podataka odredište način rada za *Izvoz podataka* .  Dodatne informacije o parametara servisa Web potražite u članku [AzureML Web servisa parametara unosa](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) na obavještavanje Cortana i strojnog učenja bloga.

Da biste konfigurirali Web parametre servis za uvoz upita i odredišne tablice:

1.  U oknu svojstva za modul za *Uvoz podataka* , kliknite ikonu u gornjem desnom kutu polja **upita baze podataka** i odaberite **Postavi kao parametar web-usluge**.
2.  U oknu svojstva za modul za *Izvoz podataka* , kliknite ikonu u gornjem desnom kutu **podatkovne tablice naziv** polja i odaberite **Postavi kao parametar web-usluge**.
3.  Na dnu okna svojstva modul *Izvoz podataka* u odjeljku **Parametara Web servisa** kliknite upita baze podataka i preimenujte je upit.
4.  Kliknite **naziv tablice podataka** , a zatim je preimenovati **tablice**.

Kada završite, na eksperiment trebao bi izgledati slično kao na sljedećoj slici.
 
![Konačni izgled eksperimenta.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Sada možete implementirati pokusa kao web-servisa.

## <a name="deploy-the-web-service"></a>Implementacija web-usluge 
Možete implementirati Classic ili novog web-servisa.

### <a name="deploy-a-classic-web-service"></a>Uvođenje klasičnog web-servisa

Implementacija kao klasične web-servisa i stvaranje aplikacija za je:

1.  Pri dnu područja crtanja eksperiment, kliknite Pokreni.
2.  Kada Pokreni dovrši, kliknite **Implementacija web-servisa** i odaberite **Implementacija web-servisa [klasični]**.
3.  Na nadzornoj ploči za servis za web, pronađite svoj ključ za API-JA. Kopiranje i spremiti ga kasnije koristili.
4.  U tablici **Zadane krajnje točke** kliknite vezu **Izvođenja grupe** da biste otvorili stranicu za pomoć API-JA.
5.  U Visual Studio stvaranje konzole aplikacije C#.
6.  Na stranici pomoć API potražite sekciji **Ogledni kod** pri dnu stranice.
7.  Kopirajte i zalijepite uzorak koda C# u datoteci Program.cs i uklonite sve reference spremište blobova platforme.
8.  Ažurirajte vrijednost varijable *apiKey* ključ za API ranije spremili.
9.  Pronađite zahtjev za izvješća i ažuriranje vrijednosti parametara servisa Web koji se prosljeđuju moduli *Podaci za uvoz* i *Izvoz podataka* . U ovom slučaju će koristiti izvorni upit, ali definirajte novi naziv tablice.

        var request = new BatchExecutionRequest() 
        {   
            GlobalParameters = new Dictionary<string, string>() {
            { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
            { "Table", "dbo.ScoredTable2" },
            }
        };

10. Pokrenite aplikaciju. 

Nakon dovršetka izvođenja novu tablicu dodaje u bazu podataka koja sadrži bilježenja rezultata rezultate.

### <a name="deploy-a-new-web-service"></a>Implementacija nove web-usluge

Implementacija kao novog web-servisa i stvaranje aplikacija za je:

1.  Pri dnu pokusa platno, kliknite **Pokreni**.
2.  Kada Pokreni dovrši, kliknite **Implementacija web-servisa** i odaberite **Implementacija web-servisa [novi]**.
3.  Na stranici uvođenje eksperiment unesite naziv web-servisa i odaberite plan cijene, a zatim **Implementiraj**.
4.  Na stranici za **brzo pokretanje** kliknite **utrošak**.
5.  U odjeljku **Uzorak koda** kliknite **grupe**.
6.  U Visual Studio stvaranje konzole aplikacije C#.
7.  Kopirajte i zalijepite uzorak koda C# u datoteci Program.cs.
8.  Ažurirajte vrijednost varijable *apiKey* **Primarnog ključa** koja se nalazi u odjeljku **Osnovni potrošnje informacije** .
9.  Pronađite deklariranje *scoreRequest* i ažuriranje vrijednosti parametara servisa Web koji se prosljeđuju moduli *Podaci za uvoz* i *Izvoz podataka* . U ovom slučaju će koristiti izvorni upit, ali definirajte novi naziv tablice.

        var scoreRequest = new
        {
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                 { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };

10. Pokrenite aplikaciju. 
 

