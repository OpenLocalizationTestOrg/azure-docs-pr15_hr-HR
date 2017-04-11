<properties
    pageTitle="Vodič za brzi početak: strojnog učenja tekst analize API-ji | Microsoft Azure"
    description="Azure strojnog učenja tekst Analytics – vodič za brzi početak rada"
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

# <a name="getting-started-with-the-text-analytics-apis-to-detect-sentiment-key-phrases-topics-and-language"></a>Uvod u tekst analize API-ji za otkrivanje šalju, ključne izraze, tema i jezika

<a name="HOLTop"></a>

U ovom dokumentu opisuje kako na onboard servisa i aplikaciju koju želite koristiti [API analize tekst](//go.microsoft.com/fwlink/?LinkID=759711).
Ove API-je možete koristiti da biste otkrili šalju, ključne izraze, tema i jezika teksta. [Kliknite ovdje da biste vidjeli interaktivnu demonstraciju sučelja.](//go.microsoft.com/fwlink/?LinkID=759712)

Pogledajte [definicije API-JA](//go.microsoft.com/fwlink/?LinkID=759346) za tehničku dokumentaciju za API-ji.

Ovaj je vodič namijenjen za verziju 2 API-ji. Informacije o verziji 1 API-ji, [pogledajte ovaj dokument](../machine-learning/machine-learning-apps-text-analytics.md).

Do kraja ovog praktičnog vodiča moći programatically otkriti:

- **Šalju** - je tekst pozitivne i negativne?

- **Ključ izrazi** – koji su korisnici razgovarate o jednom članka?

- **Tema** - koji su korisnici razgovarate o svim brojne članke?

- **Jezici** – jezik je tekst pisan?

Imajte na umu da se taj API naplaćuje 1 transakcije po dokumentu šalje. Na primjer, ako je zahtjev šalju 1000 dokumenata u jednom poziva, 1000 transakcije će odbitak.



<a name="Overview"></a>
## <a name="general-overview"></a>Općenito pregled ##

Ovaj je dokument Postupni vodič. Naš cilj je za vas voditi kroz korake potrebne uvježbati modela i postavite pokazivač na resurse koji vam omogućuje da ih u radni. Ovu vježbu se 30 minuta.

Ove zadatke će potrebno uređivač i poziva RESTful krajnje točke na jeziku po izboru.

Počnimo!

## <a name="task-1---signing-up-for-the-text-analytics-apis"></a>1 – zadatak potpisivanja za API-ji analize teksta ####

U ovom ćete zadatku će se prijaviti za uslugu analize tekst.

1. Dođite do **Kognitivne servisa** [Azure Portal](//go.microsoft.com/fwlink/?LinkId=761108) i **Analize tekst** provjerite je li odabrana kao 'API vrstu'.

1. Odaberite tarifu. Može odabrati **besplatne sloju za 5000 transakcije/mjesec**. Kao što je besplatno tarifa, koji se ne naplaćuje za korištenje usluge. Morat ćete prijava u pretplatu za Azure. 

1. Ispunite ostala polja i stvaranje računa.

1. Kada se registrirate za analizu teksta, pronađite svoj **Ključ za API -JA**. Koliko će je potrebno prilikom korištenja API servisa, kopirajte primarni ključ.


## <a name="task-2---detect-sentiment-key-phrases-and-languages"></a>Zadatak 2 – otkriti šalju, ključne izrazi i jezici ####

Jednostavno je otkriti šalju, ključne izrazi i jezike u tekstu. Programatically dobit ćete iste rezultate kao što je vraća [sučelje pokazni videozapis](//go.microsoft.com/fwlink/?LinkID=759712) .

>[AZURE.TIP] Za analizu šalju, preporučujemo da dijeljenje teksta u rečenici. To obično potencijalnih klijenata za veće preciznosti u šalju predviđanja.

Imajte na umu da su podržani jezici na sljedeći način:

| Značajka | Šifre podržanih jezika |
|:-----|:----|
| Šalju | `en`(Na engleskom), `es` (španjolski), `fr` (francuski) `pt` (portugalskom) |
| Ključni izrazi | `en`(Na engleskom), `es` (španjolski), `de` (njemački) `ja` (japanski) |


1. Morat ćete postaviti zaglavlja na sljedeće. Imajte na umu da JSON trenutno samo prihvaćenom unos oblik API-ji. XML nije podržana.

        Ocp-Apim-Subscription-Key: <your API key>
        Content-Type: application/json
        Accept: application/json

1. Nakon toga oblikujte vaš unos redaka u JSON. Za šalju, ključne izrazi i jezika oblik nije isti. Imajte na umu da svaki ID mora biti jedinstven i vratit će se ID sustav. Maksimalna veličina jedan dokument koji se može poslati 10KB, a ukupni maksimalne veličine poslane unos 1MB. Više od 1000 dokumenata može poslati u jedan poziv. Ograničavanje stopa postoji brzinom od 100 pozive u minuti – Stoga preporučujemo da pošaljete velike količine dokumenata u jedan poziv. Neobavezan parametar koji mora biti navedena je jezik ako analiza engleskog tekst. Primjer unos prikazuje se ispod, gdje neobavezan parametar `language` za šalju uključen izdvajanjem izraza za analizu ili ključ:

        {
            "documents": [
                {
                    "language": "en",
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "language": "en",
                    "id": "100",
                    "text": "Final document"
                }
            ]
        }

1. Upućivanje poziva **POŠTANSKI** sustav s unos za šalju, ključne izrazi i jezik. URL-ove izgledat će ovako:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages

1. Ovaj poziv će se vratiti na JSON oblikovani odgovor s ID-ove i otkriven svojstva. Primjer izlaz za šalju se prikazuje ispod (s detalje o pogrešci isključena). U slučaju šalju, rezultat između 0 i 1, vratit će se za svaki dokument:

        // Sentiment response
        {
            "documents": [
                {
                    "id": "1",
                    "score": "0.934"
                },
                ...
                {
                    "id": "100",
                    "score": "0.002"
                },
            ]
        }

        // Key phrases response
        {
            "documents": [
                {
                    "id": "1",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
                ...
                {
                    "id": "100",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
            ]
        }

        // Languages response
        {
            "documents": [
                {
                    "id": "1",
                    "detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
                },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
                }
            ]
        }


## <a name="task-3---detect-topics-in-a-corpus-of-text"></a>Zadatak 3 – prepoznati teme u corpus teksta ####

Ovo je upravo objavljenu API koji vraća vrhu teme popis poslan tekst zapisa. Tema povezuje se s ključem izraz, što može biti nešto ili više povezanih riječi. U API namijenjen pogodne za kratki Ljudski pisane teksta kao što su pregledi i povratne informacije korisnika.

Taj API zahtijeva **najmanje 100 tekst zapisa** da biste poslali, ali osmišljene su da biste otkrili teme preko stotine tisućama slika zapisa. Ne-engleskoj zapisima ni zapise s manje od 3 riječi odbacit će se i stoga ne dodjeljuju teme. Za otkrivanje tema maksimalne veličine jedan dokument koji se može poslati je 30KB, a ukupni maksimalne veličine poslane unos 30MB. Otkrivanje tema je stopa ograničen na 5 poslanih stavki svakih 5 minuta.

Postoje dvije dodatne **neobavezne** ulazne parametre kojih možete poboljšati kvalitetu rezultata:

- **Zaustavite riječi.**  Sljedeće riječi i zatvori obrasce (primjerice množina) će izuzeti iz kanal za otkrivanje cijelu temu. Ta postavka za česte riječi (na primjer, "problem", "Pogreška" i "korisnik" može biti odgovarajuće mogućnosti za klijenta pritužbe softver). Svaki niz mora biti jednu riječ.
- **Zaustavljanje izrazi** – ovih izraza će izuzeti iz popisa vraćeni tema. Koristi se za isključivanje generički temama koje ne želite vidjeti u rezultatima. Na primjer, "Microsoft" i "Azure" bi odgovarajuće izbori tema da biste isključili. Nizovi mogu sadržavati više riječi.

Slijedite ove korake da biste otkrili teme u tekstu.

1. Oblikujte unos u JSON. Ovaj put možete definirati isključene riječi i prekid izraza.

        {
            "documents": [
                {
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "id": "100",
                    "text": "Final document"
                }
            ],
            "stopWords": [
                "issue", "error", "user"
            ],
            "stopPhrases": [
                "Microsoft", "Azure"
            ]
        }

1. Koristite isti zaglavlja definirana zadatka 2, provjerite **objavu** poziva krajnjoj teme:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics

1. To će se vratiti na `operation-location` kao zaglavlja u odgovoru, gdje je vrijednost URL upita za dobivene teme:

        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

1. Upit u vraćenom `operation-location` povremeno sa zahtjevom za **Početak** . Kada u minuti preporučuje se.

        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>

1. Krajnja točka će vratiti odgovor uključujući `{"status": "notstarted"}` prije obrade, `{"status": "running"}` tijekom obrade i `{"status": "succeeded"}` s jednom dovršeno. Zatim možete zauzeti izlaz koji će biti u sljedećem obliku (Detalji bilješke kao i pogreške oblika datuma isključeni u ovom primjeru):

        {
            "status": "succeeded",
            "operationProcessingResult": {
                "topics": [
                    {
                        "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
                        "score": "5"
                        "keyPhrase": "first topic name"
                    },
                    ...
                    {
                        "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
                        "score": "3"
                        "keyPhrase": "final topic name"
                    }
                ],
                "topicAssignments": [
                    {
                        "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
                        "documentId": "1",
                        "distance": "0.354"
                    },
                    ...
                    {
                        "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
                        "documentId": "55",
                        "distance": "0.758"
                    },            
                ]
            }
        }

Imajte na umu da uspješno odgovor teme iz na `operations` krajnjoj točki će imati sljedeće sheme:

    {
            "topics" : [{
                "id" : "string",
                "score" : "number",
                "keyPhrase" : "string"
            }],
            "topicAssignments" : [{
                "documentId" : "string",
                "topicId" : "string",
                "distance" : "number"
            }],
            "errors" : [{
                "id" : "string",
                "message" : "string"
            }]
        }

Slijede objašnjenja za svaki dio odgovor:

**teme**

| Ključ | Opis |
|:-----|:----|
| ID-a | Jedinstveni identifikator za svaku temu. |
| rezultat | Broj dokumenata dodijeljena temu. |
| keyPhrase | Summarizing riječ ili izraz za temu. |

**topicAssignments**

| Ključ | Opis |
|:-----|:----|
| documentId | Identifikator za dokument. Daje rezultat u obliku ID obuhvaćeno unos. |
| topicId | Tema ID koji je dodijeljen dokument. |
| udaljenost | Rezultat teme dokumenta podružnice između 0 i 1. Donja a udaljenost rezultat znači bolji je podružnice temu. |

**pogreške**

| Ključ | Opis |
|:-----|:----|
| ID-a | Jedinstveni identifikator za unos dokumenta pogreška se odnosi. |
| poruka | Poruka o pogrešci. |

## <a name="next-steps"></a>Daljnji koraci ##

Čestitamo! Sada ste izvršili pomoću tekst analize podataka. Možda sada želite pogleda pomoću alata kao što je [Power BI](//powerbi.microsoft.com) radi vizualnog prikaza podataka, kao i automatske uvide da bi se dobilo u stvarnom vremenu prikaza tekstne podatke.

Da biste vidjeli kako koristiti mogućnosti analize tekst, kao što su šalju, kao dio programa robot, potražite u članku primjer [Emocionalne robot](http://docs.botframework.com/en-us/bot-intelligence/language/#example-emotional-bot) robot Framework web-mjesta.
