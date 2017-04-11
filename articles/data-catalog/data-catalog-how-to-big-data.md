<properties
   pageTitle="Upute za rad s izvorima podataka velikih skupova podataka | Microsoft Azure"
   description="Upute u članku se isticanje uzoraka korištenja kataloga podataka Azure s izvorima podataka velikih skupova podataka, uključujući spremište blobova platforme Azure, Lake Azure podataka i Hdps HADOOP."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Upute za rad s izvorima velikih skupova podataka u katalogu podataka Azure

## <a name="introduction"></a>Uvod
**Katalog podataka za Microsoft Azure** je servis potpuno upravljanih oblaka koja služi kao sustav Registracija i sustava otkrivanje za izvore podataka tvrtke. Drugim riječima, **Katalog podataka Azure** je sve o osobama pomoć otkrivanje, razumijevanje i korištenje izvora podataka i tvrtkama ili ustanovama pomoć da biste dobili dodatne vrijednost iz svoje postojećeg izvora podataka, uključujući velikih skupova podataka.

**Katalog podataka Azure** podržava registraciju za pohranu za Blog Azure blob-ova i direktorija kao i datoteke Hdps HADOOP i direktorija. Djelomično strukturirani prirode izvore podataka pruža fleksibilnost sjajno, ali i znači da korisnici morate uzeti u obzir kako su organizirane izvore podataka da biste dobili Većina vrijednost iz ih registrirate za **Azure kataloga podataka**.

## <a name="directories-as-logical-data-sets"></a>Direktorija kao logička skupa podataka.

Uobičajeni obrazac za organiziranje velikih skupova podataka izvora je direktorija Smatraj logičkih skupova podataka. Najviše razine direktorija koriste se za definiranje skupu podataka dok je podmapa definiranje particije i datoteke koje sadrže pohrane podataka sam.

Primjer ovaj uzorak može biti:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

U ovom primjeru vehicle_maintenance_events i location_tracking_events predstavljaju logičkih skupova podataka. Svaki od tih mapa sadrži podatkovne datoteke koje su organizirane prema godini i mjesecu u podmape. Svaki od tih mapa može sadržavati stotine ili tisuće datoteke.

U ovaj uzorak Registracija pojedinačnih datoteka pomoću **Kataloga podataka Azure** vjerojatno neće imati smisla. Umjesto toga prijava direktorija koji predstavljaju skupova podataka koji će biti smislen korisnicima rad s podacima.

## <a name="reference-data-files"></a>Pregled podatkovnih datoteka

Komplementarna uzorak je za pohranu referenca skupa podataka kao pojedinačne datoteke. Ove skupova podataka mogu smatrati strani 'malih velikih skupova podataka, a često slični su dimenzije modela analitičke podatke. Referenca podatkovne datoteke sadrže zapise koje se koriste za pružaju kontekst za skupno podatkovnih datoteka pohranjena negdje drugdje u spremištu velikih skupova podataka.

Primjer ovaj uzorak može biti:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Programa analitičar ili podataka pozvana radu s podataka koji se nalaze u većim strukture direktorija podataka u tim datotekama referenca može koristiti da biste dobili detaljnije informacije za entiteti koji upućuje samo naziv ili ID-JA u veći skup podataka.

U ovaj uzorak će vam tako biti smislu da biste registrirali referenca za pojedinačne podatkovne datoteke s **Azure kataloga podataka**. Svaku datoteku predstavlja skup podataka, a svaki od njih se možete dodavati napomene i otkrio pojedinačno.

## <a name="alternate-patterns"></a>Zamjenski uzorke

Uzorci na prethodno opisan samo na dva načina moguće mogu organizirati u trgovini velikih skupova podataka, ali razlikovat će se svaki implementacije. Bez obzira na to kako izvora podataka strukturirane su, prilikom registriranja izvora velikih skupova podataka pomoću **Kataloga podataka Azure**, fokus na Registriranje datoteke i mape koje predstavljaju skupovima podataka koji će se vrijednosti u drugim korisnicima unutar tvrtke ili ustanove. Registracija sve datoteke i mape možete Zagušenje kataloga upućivanje teže za korisnike koji žele pronađite ono što tražite.

## <a name="summary"></a>Sažetak
Registracija izvorima podataka pomoću **Kataloga podataka Azure** olakšava ih da biste otkrili i razumijevanje. Prijava i opaske velikih skupova podataka datoteke i mape koje predstavljaju logičkih skupova podataka, možete pomoći korisnicima pronalaženje i korištenje izvora velikih skupova podataka koje su im potrebne.
