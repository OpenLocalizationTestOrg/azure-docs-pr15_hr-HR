<properties
    pageTitle="Nadogradnja na verziju 2 analize tekst API | Microsoft Azure"
    description="Azure strojnog učenja tekst Analytics – nadogradnje na verziju 2"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="upgrading-to-version-2-of-the-text-analytics-api"></a>Nadogradnja na verziju 2 analize tekst API-JA #

Ovaj vodič provest će vas kroz postupak nadogradnje kod sa pomoću [prva verzija u API-JA](../machine-learning/machine-learning-apps-text-analytics.md) pomoću druga verzija. 

Ako ste koristili u API-JA, a želite da biste saznali više, možete ga **[Saznajte više o ovdje API-JA](//go.microsoft.com/fwlink/?LinkID=759711)** ili **[slijedite vodič brzi početak rada](//go.microsoft.com/fwlink/?LinkID=760860)**. Tehnički vodič potražite **[Definicija API -JA](//go.microsoft.com/fwlink/?LinkID=759346)**.

### <a name="part-1-get-a-new-key"></a>Dio 1. Novi ključ ###

Prvo, morat ćete dobiti novi ključ za API-JA s **Portala za Azure**:

1. Dođite do usluge analize tekst putem [Galerije obavještavanje Cortana](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2). Ovdje ćete pronaći i veza na dokumentaciju i kod primjere.

1. Kliknite **Prijava**. Ove veze će vas odvesti portala za upravljanje Azure gdje možete se prijaviti za uslugu.

1. Odaberite tarifu. Može odabrati **besplatne sloju za 5000 transakcije/mjesec**. Kao što je besplatno tarifa, koji se ne naplaćuje za korištenje usluge. Morat ćete prijava u pretplatu za Azure. 

1. Kada se registrirate za analizu tekst, prikazat će biti zadani **Ključ za API -JA**. Kopirati ključ, morat ćete je prilikom korištenja API servisa.

### <a name="part-2-update-the-headers"></a>Dio 2. Ažuriranje zaglavlja ###

Ažuriranje vrijednosti poslane zaglavlja kao što je prikazano u nastavku. Imajte na umu je više kodiran ključ za račun.

**Verzija 1**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**Verzija 2**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### <a name="part-3-update-the-base-url"></a>Dio 3. Ažuriranje osnovni URL ###

**Verzija 1**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**Verzija 2**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### <a name="part-4a-update-the-formats-for-sentiment-key-phrases-and-languages"></a>Dio 4a. Ažuriranje oblici za šalju, ključne izrazi i jezici ###

#### <a name="endpoints"></a>Krajnje točke ####

Početak krajnje točke su sada je izostavljen, tako da sva unos treba poslati kao zahtjev za objavu. Ažuriranje krajnjih točaka onima prikazano u nastavku.

| |Verzija 1 jedan krajnje točke|Krajnja točka obradu verzija 1|Verzija 2 krajnje točke|
|---|---|---|---|
|Vrsta poziva|POČETAK|OBJAVA|OBJAVA|
|Šalju|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|Ključni izrazi|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|Jezici|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### <a name="input-formats"></a>Unos oblici ####

Imajte na umu samo oblika objavu sada prihvaćen, da bi se trebali biste preoblikovati svi se unosi koji sukladno tome koristili krajnje točke jedan dokument. Unosi ne razlikuju velika i mala slova.

**Verzija 1 (serije)**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Verzija 2**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="output-from-sentiment"></a>Izlaz iz šalju ####

**Verzija 1**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Verzija 2**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-key-phrases"></a>Izlaz iz ključa izrazi ####

**Verzija 1**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Verzija 2**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-languages"></a>Izlaz iz jezika ####


**Verzija 1**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Verzija 2**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### <a name="part-4b-update-the-formats-for-topics"></a>Dio 4b. Ažuriranje oblici za teme ###

#### <a name="endpoints"></a>Krajnje točke ####

| |Krajnja točka verzija 1 | Krajnja točka verzija 2|
|---|---|---|
|Slanje za otkrivanje teme (OBJAVA)|```StartTopicDetection```|```topics```|
|Dohvaćanje rezultata teme (DOHVATI)|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### <a name="input-formats"></a>Unos oblici ####

**Verzija 1**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Verzija 2**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="submission-results"></a>Slanje rezultata ####

**Verzija 1 (OBJAVA)**

Prethodno, po završetku posla primiti sljedeće izlaz JSON gdje u jobId će biti dodan URL-a za dohvaćanje izlaz.

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**Verzija 2 (OBJAVA)**

Odgovor sada uključuju zaglavlje vrijednost kako slijedi, gdje `operation-location` se koristi kao krajnja točka za za rezultate:

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### <a name="operation-results"></a>Rezultati postupka ####

**Verzija 1 (početak)**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Verzija 2 (početak)**

Što prije, **povremeno ankete izlaz** (predložene razdoblje je svake minute) do Izlaz, vraća se. 

Kada API teme završi, status čitanje `succeeded` će vratiti. To neće sadržavati izlazni rezultat u obliku prikazano u nastavku:

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### <a name="part-5-test-it"></a>Dio 5. Testiranje! ###

Sada bi trebala biti sve što je potrebno! Isprobajte svoj kod pomoću malog oglednog da biste bili sigurni da možete uspješno obradu podataka.
