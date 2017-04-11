<properties
    pageTitle="Integracija sa sustavom Android SDK Azure mobilne radnje"
    description="Najnovija ažuriranja i postupke za Android SDK za Azure Mobile radnje"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo" />

# <a name="release-notes"></a>Napomene

## <a name="423-08102016"></a>4.2.3 (08/10/2016)

- Nema više lock Wi-Fi veze.
- Riješite je zastoja zbog prilikom pozivanja getDeviceId prije init (pogrešku u 4.2.0).

## <a name="422-05172016"></a>4.2.2 (17/05/2016)

- Poboljšanja stabilnosti.

## <a name="421-05102016"></a>4.2.1 (05/10/2016)

- Sigurnost: onemogućite web prikaz lokalne datoteke access.
- Sigurnost: uklanjanje `EngagementPreferenceActivity` predmete koji se proteže zastarjele i nesigurnom `PreferenceActivity` predmete.
- Sigurnost: razgovor aktivnosti sada su navedenih da biste koristili `exported="false"`, tu oznaku može se koristiti u starijim verzijama SDK.

## <a name="420-03112016"></a>4.2.0 (11/03/2016)

- U odjeljku ograničenja sada licenciran SDK-a.
- Dopusti određivanje identifikator prilagođene uređaja vrijeme pokretanje SDK.

## <a name="415-02012016"></a>4.1.5 (02/01/2016)

- Poboljšanja stabilnosti.

## <a name="414-01262016"></a>4.1.4 (26/01/2016)

- Poboljšanja stabilnosti.

## <a name="413-1292015"></a>4.1.3 (9/12/2015)

- Poboljšanja stabilnosti.

## <a name="412-11252015"></a>4.1.2 (25/11/2015)

- Poboljšanja stabilnosti.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)

- Poboljšanja stabilnosti.

## <a name="410-08252015"></a>4.1.0 (25/08/2015)

- Držač za novi model dozvola za Android M.
- Sada možete konfigurirati mjesto značajke prilikom izvođenja umjesto korištenja `AndroidManifest.xml`.
- Ispraviti pogrešku dozvola: Ako koristite `ACCESS_FINE_LOCATION`, zatim `ACCESS_COARSE_LOCATION` više nije potrebna.
- Poboljšanja stabilnosti.

## <a name="400-07062015"></a>4.0.0 (06/07/2015)

-   Interna protokol promjene da bi više pouzdanog analize i automatske.
-   Izvorni automatske (GCM na Admin) sada koristi se za u obavijestima aplikacije da morate konfigurirati nativni automatske vjerodajnice za bilo koju vrstu automatske kampanje.
-   Rješavanje širu sliku obavijest: su prikazane samo 10s nakon se pomiču.
-   Ispravite pogrešku u prikazu web-tablice: klikom na vezu je i izvršavaju na zadani URL akcije.
-   Riješite rijetko neočekivanog vezane uz upravljanje lokalno spremište.
-   Riješite upravljanje niz dinamički konfiguracije.
-   Ažurirajte licencni ugovor.

## <a name="300-02172015"></a>3.0.0 (17/02/2015)

-   Početnog izdanja Azure mobilne radnje
-   Konfiguriranje ID programa zamijenjen je konfiguraciju za niz veze.
-   Uklanja API-JA za slanje i primanje poruka proizvoljne XMPP iz proizvoljne entiteti XMPP.
-   Uklanja API-JA za slanje i primanje poruka između uređaja.
-   Poboljšanja sigurnost.
-   Praćenje Google Play i SmartAd ukloniti.
