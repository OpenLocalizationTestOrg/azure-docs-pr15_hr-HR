<properties
    pageTitle="API-ji učenje računalu: Tekst analize | Microsoft Azure"
    description="Microsoftovi strojno učenje tekst analize API-ji se može koristiti da biste analizirali nestrukturirane tekst za analizu šalju, izdvajanjem ključa izraz, prepoznavanja jezika i otkrivanje temu."
    services="machine-learning"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/> 

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>


# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>API-ji učenje računalu: Tekst analize šalju, ključ izdvajanjem izraz, prepoznavanja jezika i otkrivanje teme

>[AZURE.NOTE] Ovaj je vodič namijenjen za verziju 1 na API-JA. Za verziju 2, [**pogledajte ovaj dokument**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). Verzija 2 sada je Preferirani verzija ovaj API-JA.

## <a name="overview"></a>Pregled

API analize tekst je paket sustava tekstni analytics [web-servisi](https://datamarket.azure.com/dataset/amla/text-analytics) načinjene pomoću Azure strojnog učenja. API Sučelja mogu se da biste analizirali nestrukturirane tekst za zadatke kao što su šalju analizu, izdvajanjem ključa izraz, prepoznavanja jezika i otkrivanje temu. Nema podataka o obuka je potrebno koristiti taj API: samo Premjesti tekstnim podacima. Taj API koristi napredne prirodnim jezikom obrade tehnike izlaganje najbolje u predmete predviđanja.

Vidjet ćete tekst analize u akciji na naš [pokazni videozapis web-mjesta](https://text-analytics-demo.azurewebsites.net/), gdje naći ćete i [primjera](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) kako implementirati analize tekst u C# i Python.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

---

## <a name="sentiment-analysis"></a>Analiza šalju

U API vraća brojčanog rezultata između 0 i 1. Rezultati blizu 1 označavaju pozitivan šalju dok brojčane pokazatelje blizu 0 označava negativni šalju. Rezultat šalju generira se klasifikacije tehnike. Unos značajki da biste na klasifikatora obuhvaćaju n-gramima, značajke koje su stvorene iz dijela govora oznake, a word embeddings. Trenutno engleski je samo podržan jezik.
 
## <a name="key-phrase-extraction"></a>Izdvajanje ključa izraz

U API vraća popis nizova aliasu na važne stavke argumente za raspravu unos teksta. Ne možemo uključivanja tehnike iz Microsoft Office sofisticirane prirodnim jezikom obrade komplet alata. Trenutno engleski je samo podržan jezik.

## <a name="language-detection"></a>Prepoznavanje jezika

U API vraća otkriven jezika i brojčanog rezultata između 0 i 1. Rezultati blizu 1 označavaju 100% provjeru pouzdanosti identificirani jezik true. Podržane su Ukupno 120 jezike.

## <a name="topic-detection"></a>Otkrivanje teme

Ovo je upravo objavljenu API koji vraća vrhu teme popis poslan tekst zapisa. Tema povezuje se s ključa izraze koji se može biti jedna ili više povezanih riječi. U ovom API zahtijeva najmanje 100 tekst zapisa koju želite poslati, ali osmišljene su da biste otkrili teme preko stotine tisućama slika zapisa. Imajte na umu da se taj API naplaćuje 1 transakcije po tekstni zapis koji se šalje. U API namijenjen pogodne za kratki Ljudski pisanih teksta kao što su pregledi i povratne informacije korisnika.

---

## <a name="api-definition"></a>Definicija API-JA

### <a name="headers"></a>Zaglavlja

Provjerite je li uključiti točan zaglavlja u zahtjevu za koje treba izgledati ovako:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Ključ za račun s računa možete pronaći u [Azure Data Market](https://datamarket.azure.com/account/keys). Imajte na umu da se trenutno samo JSON prihvatili za ulazni i izlazni oblike. XML nije podržana.

---

## <a name="single-response-apis"></a>API-ji jedan odgovor

### <a name="getsentiment"></a>GetSentiment

**URL-A** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Zahtjev za primjer**

U pozivu ispod, ne možemo zahtijevaju šalju analizu izraz "Pozdrav svijeta":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

To će se vratiti odgovor na sljedeći način:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

---

### <a name="getkeyphrases"></a>GetKeyPhrases

**URL-A**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Zahtjev za primjer**

U nastavku poziva, ne možemo zahtijevaju ključa izraza pronaći u tekstu "Bile sjajne hotelski da ostanete na, s jedinstvenim décor i neslužbeni osoblje":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

To će se vratiti odgovor na sljedeći način:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }
 
---

### <a name="getlanguage"></a>GetLanguage

**URL-A**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Zahtjev za primjer**

U GET poziva ispod, ne možemo zahtijevaju za šalju za ključne izraze u tekst *Pozdrav svijeta*

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

To će se vratiti odgovor na sljedeći način:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Neobavezni parametri**

`NumberOfLanguagesToDetect`je neobavezan parametar. Zadana je vrijednost 1.

---

## <a name="batch-apis"></a>API-ji grupe

Usluga analize tekst omogućuje vam šalju i extractions ključ izraz u načinu rada za obradu. Imajte na umu da svaki od zapisa testu dobije broji kao jedan transakcije. Na primjer, ako je zahtjev šalju 1000 zapisa u jednom poziva, 1000 transakcije će odbitak.

Imajte na umu da ID-a unosi u sustav sustav vratio ID-a. Web-servisa provjerite jesu li te ID-ove jedinstveni. To je odgovornosti pozivatelja da biste provjerili jedinstvenosti. 


### <a name="getsentimentbatch"></a>GetSentimentBatch

**URL-A** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Zahtjev za primjer**

U pozivu objavu ispod, ne možemo traži za sentiments izraze "Pozdrav svijeta", "Pozdrav odnožje svijeta" i "Pozdrav Moje svijeta" u tijelu zahtjeva za:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Tijelo zahtjev:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

U nastavku odgovor se na popisu Rezultati povezan s tekstom ID-ova:

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


---

### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch

**URL-A**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Zahtjev za primjer**

U ovom primjeru smo zahtjev za popis sentiments za ključne izraze u sljedeći tekst: 

* "Bile sjajne hotelski da ostanete na, s jedinstvenim décor i neslužbeni osoblje"
* "Je nevjerojatnih Sastavi konferenciji, s vrlo zanimljive talks"
* "Promet je terrible, li potrošeno tri sata prelaska na zračna luka"

Zahtjev je stvoren kao objavu poziv krajnjoj:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Tijelo zahtjev:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

U nastavku odgovor se popis ključa izraze povezan s tekstom ID-ova:

    { "odata.metadata":"<url>",
        "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

---

### GetLanguageBatch

In the POST call below, we are requesting language detection for two text inputs:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Request body:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

This returns the following response, where English is detected in the first input and French in the second input:

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "engleski",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "francuski",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

---

## <a name="topic-detection-apis"></a>API-ji za otkrivanje teme

Ovo je upravo objavljenu API koji vraća vrhu teme popis poslan tekst zapisa. Tema povezuje se s ključa izraze koji se može biti jedna ili više povezanih riječi. Imajte na umu da se taj API naplaćuje 1 transakcije po tekstni zapis koji se šalje.

U ovom API zahtijeva najmanje 100 tekst zapisa koju želite poslati, ali osmišljene su da biste otkrili teme preko stotine tisućama slika zapisa.


### <a name="topics--submit-job"></a>Tema – predavanje posla

**URL-A**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Zahtjev za primjer**


U objavu poziva ispod, ne možemo zahtijevaju teme za skup 100 članke, gdje prikazuju se članci unos imena i prezimena i dva StopPhrases uključeni.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Tijelo zahtjev:

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

U nastavku odgovor, primit ćete na JobId poslane zadatka:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Popis jedne riječi ili više izraza riječi koje bi trebala biti vraćena kao teme. Može se koristiti za filtriranje je vrlo generički temama. Ako, na primjer, u skupu podataka o hotelski preglede, "hotelski" i "hostel" možda prvo Zaustavi izraza.  

### <a name="topics--poll-for-job-results"></a>Tema – anketu za posao rezultata

**URL-A**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Zahtjev za primjer**

Prenesite JobId vratio "Pošalji posao" korak za dohvaćanje rezultata. Preporučujemo da pozovete ovu krajnju točku svake minute do Status = "Dovršeno" u odgovoru. Trebat će oko 10 minuta za posao dovršeno ili dulje za poslove s mnogo tisuće zapisa.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Dok je obrade odgovora bit će na sljedeći način:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
        "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


U API vraća Izlaz u JSON OSNOVNI oblik u sljedećem obliku:

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
        ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


Su svojstva za svaki dio odgovor na sljedeći način:

**Svojstva TopicInfo**

| Ključ | Opis |
|:-----|:----|
| TopicId | Jedinstveni identifikator za svaku temu. |
| Rezultat | Brojanje zapisa koji su dodijeljene temu. |
| KeyPhrase | Summarizing riječ ili izraz za temu. Može biti 1 ili više riječi. |

**Svojstva TopicAssignment**

| Ključ | Opis |
|:-----|:----|
| ID-a | Identifikator za zapis. Daje rezultat u obliku ID obuhvaćeno unos. |
| TopicId | Tema ID koji je dodijeljen zapis. |
| Udaljenost | CONFIDENCE zapis pripada temu. Udaljenost bliže nuli označava veće pouzdanosti. |
