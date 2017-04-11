<properties
    pageTitle="Pregled API-JA za izvoz mobilne radnje"
    description="Naučite osnove izvoza podataka neobrađenog generira korisničkom uređaji odražava u vlastiti Alati"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="kpiteira"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile"
    ms.date="04/26/2016"
    ms.author="kpiteira"/>

# <a name="mobile-engagement-export-api-overview"></a>Pregled API-JA za izvoz mobilne radnje

## <a name="introduction"></a>Uvod

U ovom dokumentu će Naučite osnove izvoza neobrađenog generira korisničkom uređaji odražava u vlastiti Alati podataka.

## <a name="pre-requisites"></a>Prije requisites

Izvoz sirovim podacima iz mobilne radnje zahtijeva:

- Postavljanje provjere autentičnosti API da biste mogli koristiti API-ji (pogledajte [ručno postavljanje provjere autentičnosti](mobile-engagement-api-authentication-manual.md)),
- Korištenje REST API-ji ili [.net SDK](mobile-engagement-dotnet-sdk-service-api.md)
- Račun za Azure prostora za pohranu.

>[AZURE.NOTE] Također preporučljivo je odličan [Microsoft Azure prostora za pohranu Explorer](http://storageexplorer.com/)barem tijekom faze razvoja kao omogućuje jednostavno je za korištenje korisničkog Sučelja za interakciju s Azure prostora za pohranu.

## <a name="what-can-be-exported"></a>Što možete izvesti?

Korištenje mobilne njegov korisnicima omogućuje prikupljanje razne vrste podataka te stoga ima nekoliko vrsta izvoz prilagođene te različitih vrsta podataka.
Postoje 2 ključna vrste izvoz:

- Snimka: koristi obično za izvoz podataka koja predstavlja stanje web-mjesta i za koju Mobile radnje imali povijest. To obuhvaća oznake (aplikacije-info) tokeni ili automatske kampanje feedbacks, primjerice. Sljedeći consequence ove vrste izvoz nisu povezani s datumom.
- povijesne: Ova vrsta izvoz koristi se za podatke koji se, primjerice akumulira tijekom vremena, kao što su događaji ili aktivnosti.

U tablici u nastavku opisuju exhaustively sve moguće izvoze:

| Izvoz vrsta | Vrsta podataka | Opis                                                                                                                                 |
|-------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Brze snimke    | Automatske      | Stvara izvezite automatske kampanje feedbacks na temelju po deviceid/ID korisnika                                                              |
| Brze snimke    | Oznaka       | Stvara Izvoz oznaka (aplikacija informacije) povezani svaki uređaja                                                                       |
| Brze snimke    | Uređaj    | Stvara izvezite većinu podatke o uređajima kao što su technicals (model, regionalnu shemu, vremenska zona,...), oznake, prvi put vide... |
| Brze snimke    | Tokena     | Stvara izvezite valjani tokena                                                                                                 |
| Povijesne  | Aktivnosti  | Stvara izvoz svih aktivnosti za svaki uređaje u određenom vremenskom razdoblju                                                           |
| Povijesne  | Događaja     | Stvara izvoz svih aktivnosti za svaki uređaje u određenom vremenskom razdoblju                                                           |
| Povijesne  | Zadatak       | Stvara Izvezite sve zadatke za svaki uređaja u određenom vremenskom razdoblju                                                                 |
| Povijesne  | Pogreška     | Stvara Izvezite sve pogreške za svaki uređaje u određenom vremenskom razdoblju                                                               |

## <a name="how-does-it-work"></a>Kako to funkcionira?

Izvozi su dugi zadataka koji se izvode koji može dati velike podatkovne datoteke. Zbog toga ih nije moguće pozvati da biste se vratili u datoteku da biste preuzeli odmah.
Da bi se izvoz podataka iz mobilne radnje, imat ćete da biste stvorili **Zadatak izvoz** putem API kojem određujete obično:

- Željenu vrstu izvoza (snimke ili povijesne)
- Vrsta podataka
- **Spremnik za pohranu Azure** (uključujući valjani SAS s pristupom pisanja) koje će biti zapisane rezultat izvoz.

Napominjemo da može potrajati nekoliko minuta za svoj posao da se pokreće, a zatim ga pokrenuti iz nekoliko sekundi za maleni aplikacije da biste nekoliko sati aplikacije s mnogo korisnika ili aktivnosti.

Nakon stvaranja posla, moguće je da biste provjerili ima li status da biste vidjeli ako i dalje traje ili dovrši.

Kada je USPJEŠNO posla, rezultirajući podatkovne datoteke je dostupna u spremnik navedeni prostora za pohranu.
