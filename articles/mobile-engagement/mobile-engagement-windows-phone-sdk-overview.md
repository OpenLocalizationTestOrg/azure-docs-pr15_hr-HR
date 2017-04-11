<properties 
    pageTitle="Pregled Silverlight SDK u sustavu Windows Phone" 
    description="Pregled Windows Phone Silverlight SDK za Azure mobilne radnje"                     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>U sustavu Windows Phone pregled Silverlight SDK za Azure mobilne radnje

Da biste dobili detalje o tome kako integrirati Azure Mobile radnje u aplikaciji Windows Phone Silverlight, počnite ovdje. Ako želite isprobajte sami prvo, provjerite je li dovršite naš [vodič 15 minuta](mobile-engagement-windows-phone-get-started.md).

Kliknite da biste vidjeli na [SDK sadržaja](mobile-engagement-windows-phone-sdk-content.md)

##<a name="integration-procedures"></a>Integracija postupaka

1. Počnite ovdje: [integraciji korištenje mobilne aplikacije Silverlight za Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)

2. Za obavijesti: [Integraciji razgovor (obavijesti) u aplikaciji programa Silverlight za Windows Phone](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Označavanje Planiranje implementacije: [kako koristiti dodatne radnje Mobile označavanje API-JA u aplikaciji programa Silverlight za Windows Phone](mobile-engagement-windows-phone-use-engagement-api.md)

##<a name="release-notes"></a>Napomene

###<a name="330-04192016"></a>3.3.0 (19/04/2016)
Dio *MicrosoftAzure.MobileEngagement* nuget paket **v3.4.0**

-   Zapisnici konzole dodane "TestLogLevel" API-JA za omogućivanje i onemogućivanje/filtar čuje po SDK-a.

Starije verzije potražite [Dovršeno napomene](mobile-engagement-windows-phone-release-notes.md)

##<a name="upgrade-procedures"></a>Nadogradnja postupaka

Ako već imate integrirali stariju verziju naš SDK u aplikaciji, morate Imajte na umu sljedeće prilikom nadogradnje SDK-a.

Možda ćete morati slijedite nekoliko postupaka ako Propušteni nekoliko verzija SDK-a. Potražite u članku dovršavanje [Nadogradnje postupke](mobile-engagement-windows-phone-upgrade-procedure.md). Ako, primjerice migriranja iz 0.10.1 u 0.11.0 morate najprije slijedite postupak "iz 0.9.0 za 0.10.1" pa postupak "iz 0.10.1 za 0.11.0".

###<a name="from-200-to-330"></a>Iz 2.0.0 za 3.3.0

####<a name="test-logs"></a>Testiranje zapisnika

Zapisnici konzole za koje je stvorio SDK sada može biti omogućeno/onemogućeno/filtriranja. Da biste prilagodili, ažurirali svojstvo `EngagementAgent.Instance.TestLogEnabled` na jednu vrijednost dostupna na `EngagementTestLogLevel` Enumeracije, na primjer:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Nadogradnja iz starije verzije

Potražite u članku [Nadogradnja postupaka](mobile-engagement-windows-phone-upgrade-procedure.md)
 
