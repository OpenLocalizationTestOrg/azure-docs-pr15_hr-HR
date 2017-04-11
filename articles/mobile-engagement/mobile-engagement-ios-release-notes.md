<properties
    pageTitle="Azure Mobile radnje iOS SDK napomene | Microsoft Azure"
    description="Najnovija ažuriranja i postupke za iOS SDK za Azure Mobile radnje"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="piyushjo" />

#<a name="azure-mobile-engagement-ios-sdk-release-notes"></a>Azure Mobile radnje iOS SDK napomene

##<a name="400-09122016"></a>4.0.0 (09/12/2016)

-   FIXED obavijesti ne actioned na uređajima sa sustavom iOS 10.
-   Zastarijevanje XCode 7.

##<a name="324-06302016"></a>3.2.4 (30/06/2016)

-   FIXED zbrajanja između tehničke zapisnika i druge zapisnika.

##<a name="323-06072016"></a>3.2.3 (06/07/2016)

-   Ispraviti pogreške mjesto isporuke povratne informacije ne prijavljuje kada aplikacija je u pozadini.
-   Optimizirana slanja tehničke zapisnika.

##<a name="322-04072016"></a>3.2.2 (07/04/2016)

-   FIXED pogrešku na HTTP zahtjev za otkazivanje koje ponekad pad.

##<a name="321-12112015"></a>3.2.1 (11/12/2015)

-   Fiksna odgode kada se pokreće novu instancu aplikacije obavijest s precizno vezama

##<a name="320-10082015"></a>3.2.0 (08/10/2015)

-   Omogućeno Bitcode u SDK za rad sa **Xcode 7**.
-   FIXED programskih pogrešaka vezanih uz obavijesti u aplikaciji.
-   Obavijesti u aplikaciji razdoblju više pouzdanog u slučaju baterija i druge takve scenarijima.
-   Uklanja dodatni konzole zapisnika generiranih 3 strana biblioteke.

##<a name="310-08262015"></a>3.1.0 (26/08/2015)

-   Rješavanje problema kompatibilnosti iOS 9 s bibliotekom drugih proizvođača. Je zbog toga ruši prilikom slanja ankete rezultate, informacije o aplikaciji ili dodatni podaci.

##<a name="300-06192015"></a>3.0.0 (19/06/2015)

-   Korištenje mobilne koristi tihu automatske obavijesti.
-   Podrška za iOS prekida 4.X. Počevši od ove verzije implementacije ciljne aplikacije mora biti najmanje iOS 6.

##<a name="220-05212015"></a>2.2.0 (21/05/2015)

-   Id radnje mobilnog uređaja za uređaje < iOS 6 sada se temelji na GUID generirani prilikom instalacije.

##<a name="210-04242015"></a>2.1.0 (24/04/2015)

-   Dodane Swift kompatibilnosti.
-   Kada kliknete na obavijest, akcije URL-a sada je izvršiti desno nakon otvaranja aplikacije.
-   Dodane nedostaje datoteka zaglavlja u paketu SDK.
-   Fiksni problem kada je onemogućena rušenje financije Mobile radnje.

##<a name="200-02172015"></a>2.0.0 (17/02/2015)

-   Početnog izdanja Azure mobilne radnje
-   Konfiguriranje ID programa/sdkKey zamijenjen je konfiguraciju za niz veze.
-   Uklanja API-JA za slanje i primanje poruka proizvoljne XMPP iz proizvoljne entiteti XMPP.
-   Uklanja API-JA za slanje i primanje poruka između uređaja.
-   Poboljšanja sigurnost.
-   Praćenje SmartAd ukloniti.
