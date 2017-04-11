
<properties
    pageTitle="Početak preporuke u grupama: vodič za preporuke API na računalu | Microsoft Azure"
    description="Azure strojnog učenja preporuke – početak preporuke u grupama"
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
    ms.date="08/17/2016"
    ms.author="luisca"/>

# <a name="get-recommendations-in-batches"></a>Preporuke u grupama

>[AZURE.NOTE] Početak preporuke u serijama je složenije od početak preporuke jedan po jedan. Provjerite API-ji informacije o preporuke za jedan zahtjev:

> [Preporuke za stavku po stavku](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [Preporuke za korisnika u stavke](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
>
> Bilježenje rezultata grupe funkcionira samo za izgradi koje su stvorene nakon srpanj 21, 2016.


Postoje situacije u kojima morate nabaviti preporuke za više od jedne stavke istodobno. Na primjer, možda će vas u stvaranje preporuke predmemorije ili čak i analiza vrste preporuke za koje vam se prikazuju.

Obradu bilježenja rezultata, kao što smo ih poziva su operacije asinkronog operacije. Morate poslati zahtjev, pričekajte da biste dovršili postupak i prikupite rezultate.  

Želite li biti precizno, ovo su koraci za praćenje:

1.  Ako još nemate jedno mjesto, stvorite spremnik programa Azure prostora za pohranu.
2.  Prijenos datoteke unos koji opisuje svaku zahtjeva za preporuke za spremište blobova platforme Azure.
3.  Kick-Start bilježenja rezultata obradu.
4.  Pričekajte asinkronog postupak da biste završili.
5.  Po završetku postupka, prikupite rezultate iz spremišta blobova.

Pogledajmo voditi kroz svaki od ovih koraka.

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>Stvaranje spremnika za pohranu ako ga nemate već

Idite na [portal za Azure](https://portal.azure.com) i stvorite novi prostor za pohranu račun ako ga nemate već. Da biste to učinili, idite na **Novo** > **podataka** + **prostora za pohranu** > **Računa za pohranu**.

Kada imate račun za pohranu, potrebnih za stvaranje spremnika blob gdje će sadržavati ulazni i izlazni rezultat izvođenja grupe.

Prijenos ulazne datoteke koji opisuje svaku od vašeg preporuke zahtjeve za spremište blobova platforme – recimo ovdje nazovite input.json datoteke.
Nakon što spremniku ćete morati prenijeti datoteku koji opisuje svaku zahtjeva za koje su vam potrebne za izvođenje iz servisa za preporuke.

Grupu možete izvesti samo jednu vrstu zahtjeva iz određene Sastavi. Ne možemo će objašnjavaju definiranje te podatke u sljedećem odjeljku. Zasad, recimo pretpostavlja da ne možemo izvoditi preporuke stavke iz određene Sastavi. Ulazna datoteka pa sadrži ulazne podatke (u ovom slučaju Početni broj stavki) za sve zahtjeve.

Ovo je primjer izgleda input.json datoteke:

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

Kao što vidite, datoteka je datoteku JSON, gdje svaki zahtjeve sadrži informacije koje su potrebne da biste poslali zahtjev za preporuke. Stvorite slične JSON datoteke za zahtjeve za koje su vam potrebne za ispunjavanje i kopirajte spremniku koji ste upravo stvorili spremište blobova platforme.

## <a name="kick-start-the-batch-job"></a>Kick-Start obrade

Sljedeći je korak da biste poslali novu obradu. Dodatne informacije potražite [API reference](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/).

Tijelo zahtjev za API mora definirajte mjesta gdje ulazne, izlazne i datoteke pogrešaka potrebno pohraniti. Također se mora da biste definirali vjerodajnice koje su potrebne za pristup tim mjestima. Osim toga, morate odrediti neke parametre koje se odnose na cijelu grupu (Vrsta preporuke da biste zatražili, model/Sastavi da biste koristili, broj rezultata po poziva, itd.)

Ovo je primjer tijelu zahtjeva moraju izgleda:

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

Ovdje nekoliko važnih stvari koje na umu:

-   Trenutno **authenticationType** uvijek mora biti postavljena na **PublicOrSas**.

-   Morate nabaviti token zajednički pristup potpis (SAS) da biste omogućili API preporuke za čitanje i pisanje s računa za spremište blobova platforme. Dodatne informacije o tome da biste generirali SAS tokeni pronaći ćete na [stranici API preporuke](../storage/storage-dotnet-shared-access-signature-part-1.md).

-   Samo **apiName** koji trenutno je podržano je **ItemRecommend**, koji se koristi za preporuke stavke stavke. Grupno slanje promjena trenutno ne podržava preporuke korisnika na stavku.

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>Pričekajte asinkronog postupak da biste dovršili

Prilikom pokretanja postupka grupe za odgovor vraća zaglavlje operacija mjesto koje vam podatke koji su potrebni za praćenje postupak.
Pratite postupak pomoću [Dohvatiti API Status operacija]( https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da)kao za postupak Sastavi postupak praćenja.

## <a name="get-the-results"></a>Dobili željene rezultate

Nakon što postupak bude dovršen, uz pretpostavku da je došlo je do pogreške, možete prikupiti rezultate iz izlaz bloba prostora za pohranu.

U primjeru u nastavku pokazuju Izlaz možda izgleda. U ovom primjeru, Pokazat ćemo rezultata za grupu pomoću samo dva zahtjeva (za ispušteno da bude kraće).

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>Dodatne informacije o ograničenjima

-   Samo jedan obradu naziva se po pretplati odjednom.
-   Batch unos datoteka za posao ne može biti više od 2 MB.
