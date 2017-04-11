<properties
    pageTitle="Povezivanje s web-servisa za strojno učenje | Microsoft Azure"
    description="C# ili Python, povezivanje sa servisom Azure strojnog učenja Web pomoću ključa za autorizaciju."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016" 
    ms.author="garye" />


# <a name="connect-to-an-azure-machine-learning-web-service"></a>Povezivanje sa servisom Azure strojnog učenja Web

Sučelje za razvojne inženjere Azure strojnog učenja je API usluge za Web da biste predviđanja iz ulazne podatke u stvarnom vremenu ili u načinu rada za obradu. Pomoću Azure strojnog učenja Studio stvaranje predviđanja i implementacija sustava servisa Azure strojnog učenja Web.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Da biste saznali kako stvoriti i implementirati servis strojnog učenja Web, koristeći strojnog učenja Studio:

- Vodič za stvaranje pokusa u strojnog učenja Studio, potražite u članku [Stvaranje vaš prvi eksperiment](machine-learning-create-experiment.md).
- Detalje o tome kako implementirati web-servisa potražite u članku [uvođenja servis strojnog učenja Web](machine-learning-publish-a-machine-learning-web-service.md).
- Dodatne informacije o strojnog učenja Općenito, posjetite [Dokumentaciju centar za učenje računala](https://azure.microsoft.com/documentation/services/machine-learning/).

## <a name="azure-machine-learning-web-service"></a>Azure servis strojnog učenja Web ##

Sa servisom Azure strojnog učenja Web vanjski program komunicira s strojnog učenja modelom bilježenja rezultata tijeka rada u stvarnom vremenu. Poziv za servis strojnog učenja Web vraća rezultate za predviđanje u vanjsku aplikaciju. Da biste servis strojnog učenja Web poziv, proslijedite ključa API koja je stvorena prilikom implementacije u predviđanje. Servis strojnog učenja Web temelji se na OSTALE, odabir popularnih arhitektura za web-mjesto programiranje projekata.

Azure strojnog učenja sadrži dvije vrste usluge:

- Servis za odgovor na zahtjev (RRS) – niske latencije, Visoko skalabilni servis koji nudi sučelje za bez praćenja stanja modela stvoriti i implementirati iz učenje Studio računala.
- Grupe za izvršavanje usluge (BES) – programa asinkronog servisa te brojčane pokazatelje na obradu podataka zapisa.

Dodatne informacije o strojnog učenja Web services potražite u članku [uvođenja web-strojnog učenja servisa](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Ključ za Azure strojnog učenja autorizacije ##

Prilikom implementacije sustava eksperiment API tipke generiraju se za web-servisa. Tipke možete dohvatiti iz nekoliko mjesta.

## <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>S portala sustava Microsoft Azure strojnog učenja web-servisi

Prijavite se na portal sustava [Microsoft Azure strojnog učenja Web Services](https://services.azureml.net) .

Da biste dohvatili ključ za API servisa za novo računalo učenje web-mjesto:

1. Na portalu za Azure strojnog učenja web-servisi kliknite **Web-servisi** gornji izbornik.
2. Kliknite web-servisa za koje želite dohvatiti ključ.
3. Na izborniku gornji kliknite **utrošak**.
4. Kopiranje i spremanje za **Primarni ključ**.


Da biste dohvatili ključ za API-JA za klasični strojnog učenja web-servisa:

1. Na portalu za Azure strojnog učenja web-servisi kliknite **Klasični web-servisi** gornji izbornik.
2. Kliknite web-servisa na kojem radite.
3. Kliknite krajnja točka za koje želite dohvatiti ključ.
3. Na izborniku gornji kliknite **utrošak**.
4. Kopiranje i spremanje za **Primarni ključ**.

## <a name="classic-web-service"></a>Klasični web-servisa ##

 Ključ za klasične web-servisa možete dohvatiti i iz strojnog učenja Studio ili Azure portal.

### <a name="machine-learning-studio"></a>Strojnog učenja Studio ###

1. U strojnog učenja Studio, kliknite **Web-SERVISA** na lijevoj strani.
2. Kliknite web-servisa. **Ključ za API** nalazi se na kartici **nadzorne PLOČE** .

### <a name="azure-portal"></a>Portal za Azure ###

1. Na lijevoj strani kliknite **STROJNOG UČENJA** .
2. Kliknite radni prostor u kojem se nalazi vaše web-servisa.
3. Kliknite **web-SERVISA**.
4. Kliknite web-servisa.
5. Kliknite krajnje točke. U donjem desnom dijelu "API TIPKU" nije dostupno.

## <a id="connect"></a>Povezivanje s web-strojnog učenja servisa

Možete se povezati na neki servis strojnog učenja Web pomoću programski jezik koji podržava HTTP zahtjeva i odgovora. Pogledajte primjere u C#, Python i R sa stranice pomoć za servis strojnog učenja Web.

**Pomoć za strojnog učenja API -JA** Pomoć za učenje API strojno se stvara promjenom implementacija web-servisa. Potražite u članku [vodič Azure strojnog učenja - implementacija web-servisa](machine-learning-walkthrough-5-publish-web-service.md).
Pomoć za strojnog učenja API sadrži detalje o predviđanje web-servisa.

2. Kliknite web-servisa na kojem radite.
3. Kliknite krajnja točka za koji želite da biste prikazali stranicu za pomoć API-JA.
3. Na izborniku gornji kliknite **utrošak**.
3. Kliknite **API-JA stranica za pomoć** u odjeljku odgovor na zahtjev ili grupe za izvršavanje krajnje točke.

**Prikaz strojnog učenja API pomoć za nova web-servisa**

U na Azure strojnog učenja web-servisi portalu:

1. Kliknite **Web-SERVISA** na gornji izbornik.
2. Kliknite web-servisa za koje želite dohvatiti ključ.

Kliknite **utrošak** da biste dobili na ji za zahtjev za Reposonse i grupe za izvršavanje usluge i uzorak koda u C#, R i Python.

Kliknite **Swagger API** da biste dobili Swagger temelji dokumentaciji API-ji pozvane s navedenom ji.

### <a name="c-sample"></a>C# uzorka ###

Povezivanje sa servisom za stroj učenje Web, pomoću programa **HttpClient** prosljeđivanje ScoreData. ScoreData sadrži FeatureVector programa n dimenzionalni vektorski numeričke značajki koja predstavlja na ScoreData. Autentičnost servis strojnog učenja pomoću ključa API-JA.

Povezivanje sa servisom za stroj učenje Web, mora biti instaliran paket NuGet **Microsoft.AspNet.WebApi.Client** .

**Instalirajte Microsoft.AspNet.WebApi.Client NuGet u Visual Studio**

1. Objavljivanje skupa podataka za preuzimanje iz UCI: odrasle 2 predmete dataset web-servisa.
2. Kliknite **Alati** > **Upravitelj paketa NuGet** > **Konzole za Upravitelj paketa**.
2. Odaberite **Microsoft.AspNet.WebApi.Client instalaciju paketa**.

**Da biste pokrenuli uzorak koda**

1. Objavljivanje "primjer 1: preuzmite skup podataka iz UCI: dataset predmete odrasle 2" eksperiment, dio zbirke uzorka strojnog učenja.
2. Dodijelite apiKey s ključem s web-servisa. U odjeljku **ključ programa Azure strojnog učenja autorizacije** iznad.
3. Dodijelite serviceUri s URI zahtjev.


### <a name="python-sample"></a>Ogledna Python ###

Povezivanje sa servisom za stroj učenje Web, pomoću biblioteke **urllib2** prosljeđivanje ScoreData. ScoreData sadrži FeatureVector programa n dimenzionalni vektorski numeričke značajke koji predstavlja na ScoreData. Autentičnost servis strojnog učenja pomoću ključa API-JA.


**Da biste pokrenuli uzorak koda**

1. Implementacija "primjer 1: preuzmite skup podataka iz UCI: dataset predmete odrasle 2" eksperiment, dio zbirke uzorka strojnog učenja.
2. Dodijelite apiKey s ključem s web-servisa. U odjeljku **ključ programa Azure strojnog učenja autorizacije** blizu početka ovog članka.
3. Dodijelite serviceUri s URI zahtjev.
