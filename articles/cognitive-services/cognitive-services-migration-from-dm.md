
<properties
    pageTitle="Migracije u Azure Kognitivne Services preporuke API iz preporuke DataMarket API | Microsoft Azure"
    description="Azure strojnog učenja preporuke – migracije kognitivne servisa za preporuke"
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
    ms.date="09/01/2016"
    ms.author="luisca"/>


# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>Migracije u Azure Kognitivne Services preporuke API iz preporuke DataMarket API-JA
U ovom se članku objašnjava migrirati iz [Microsoft DataMarket preporuke API](https://datamarket.azure.com/dataset/amla/recommendations) u [Microsoft Azure Kognitivne Services preporuke API -JA](https://www.microsoft.com/cognitive-services/en-us/recommendations-api).

Preporuke API servisa DataMarket će se zaustaviti prihvaćanja novim klijentima prosinac 31, 2016 i će ukinuta veljača 28, 2017.

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>Kako početi koristiti preporuke API za Kognitivne servisa Azure?

Da biste migrirali Kognitivne API preporuke Services, učinite sljedeće:

1.  Ako još nemate Azure pretplatu, [prijavite se](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) za jedan. 

1.  Dobiti podrobne upute između [Vodič za brzi početak](cognitive-services-recommendations-quick-start.md) registrirati za Kognitivne API preporuke za servise i programski je koristiti. 

1.  Idite na [Kognitivne API servisa preporuke odredišne stranice](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) da biste saznali o usluzi i pronaći dokumentaciju.

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>Korisničko Sučelje za preporuke se koristi za stvaranje Moje modela. Je li sličan alat za Kognitivne API preporuke Services?

Apsolutno! URL za nove [Preporuke korisničko Sučelje](http://recommendations-portal.azurewebsites.net/) nije http://recommendations-portal.azurewebsites.net. 

>[AZURE.NOTE] Vjerodajnice DataMarket ne funkcioniraju u nastavku. Prijavite se za pretplatu na portalu za Azure i ključ računa potrebno koristiti novi [Preporuke korisničkog Sučelja](http://recommendations-portal.azurewebsites.net/).
Ako nemate ključ za račun, potražite u članku zadatak 1 [Vodič za brzi početak rada](cognitive-services-recommendations-quick-start.md).

##<a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>Jednak u novi oblik API servisa DataMarket preporuke API-JA?

U API podržava isti scenariji i proces tokova kao tih scenarija podržane u verziji DataMarket, ali stvarna sučelja API ažurirana je da bi se prilagodio [Smjernice za Microsoft REST API -JA](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). API-ji su dosljedan i rad bolje pomoću alata koji podržavaju Swagger.

Da biste shvatili svaki API-ji, pogledajte [explorer API-JA](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db).
Koristite na *pokušajte* je gumb za testiranje API poziva. Oblik datoteke koji preporuke API-JA (kataloga i korištenje datoteke) nije promijenio, pa možete nastaviti koristiti iste datoteke i/ili infrastrukture su ugrađeni da biste generirali te datoteke.

##<a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>Što su neke od novih značajki u Kognitivne API preporuke Services?

Tijekom zadnja dva mjeseca smo ste objavio nove mogućnosti za Kognitivne API preporuke za usluge:
-   Preporuke korisničkog Sučelja za obuku i testiranje modela bez potrebe za pisanje s jednim retkom kod
-   Grupe za bilježenje rezultata da bi vam tisuće preporuke odjednom
-   Sastavljanje metriku podršku upit kvalitete modele preporuke
-   Podrška za pravila za tvrtke
-   Mogućnost Nabrajanje i preuzimanje datoteka korištenje i kataloga
-   Podrška za rangiranja Sastavi upit kvalitete značajke stavke u modelu preporuke
-   Mogućnost dodana da biste potražili proizvoda u katalogu

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Kada ne Microsoft zaustaviti podrške DataMarket preporuke API-JA?

[Preporuke API na DataMarket](https://datamarket.azure.com/dataset/amla/recommendations) zaustavlja prihvaćanju novih klijenata nakon prosinac 31, 2016 i će veljača 28, 2017 se potpuno zastarjela. 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>Što se događa ako nemam podataka koje je potrebno ponovno stvoriti Moje modelima u Kognitivne API preporuke Services?

Želimo da bi ovaj prijelaz dovoljno samo za vas. Mi vam možemo pomoći premještanje svoje stare modela s vašeg računa servisa DataMarket nove pretplate Azure Kognitivne Services preporuke API-JA. Obratite nam se na [mlapi@microsoft.com](mailto://mlapi@microsoft.com). Trebat ćete omogućuju DataMarket ID pretplate i ID za pretplatu na Kognitivne Services prije nego što smo pridružiti u modelima na novi račun.
