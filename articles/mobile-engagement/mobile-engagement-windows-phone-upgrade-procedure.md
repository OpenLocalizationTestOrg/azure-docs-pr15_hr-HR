<properties 
    pageTitle="U sustavu Windows Phone Silverlight SDK nadogradnje postupaka" 
    description="U sustavu Windows Phone Silverlight SDK nadogradnje postupke za Azure mobilne radnje"                  
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

#<a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>U sustavu Windows Phone Silverlight SDK nadogradnje postupaka

Ako već imate integrirali stariju verziju naš SDK u aplikaciji, morate Imajte na umu sljedeće prilikom nadogradnje SDK-a.

Možda ćete morati slijedite nekoliko postupaka ako Propušteni nekoliko verzija SDK-a. Ako, primjerice migriranja iz 0.10.1 u 0.11.0 morate najprije slijedite postupak "iz 0.9.0 za 0.10.1" pa postupak "iz 0.10.1 za 0.11.0".

##<a name="from-200-to-330"></a>Iz 2.0.0 za 3.3.0

### <a name="test-logs"></a>Testiranje zapisnika

Zapisnici konzole za koje je stvorio SDK sada može biti omogućeno/onemogućeno/filtriranja. Da biste prilagodili, ažurirali svojstvo `EngagementAgent.Instance.TestLogEnabled` na jednu vrijednost dostupna na `EngagementTestLogLevel` Enumeracije, na primjer:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

##<a name="from-111-to-200"></a>Iz 1.1.1 za 2.0.0

Sljedeće opisuje kako migrirati programa SDK Integracija sa servisa Capptain u aplikaciju pokreće Azure Mobile radnje koje nudi Capptain SAS. 

> [Azure.IMPORTANT] Capptain i Mobile radnje nisu iste usluge i postupak dolje samo ističe migriranju aplikaciju klijenta. Migracija SDK u aplikaciji će nije moguće migrirati podatke s poslužitelja Capptain poslužiteljima Mobile radnje

Ako premještate iz starije verzije, obratite se Capptain web-mjesta migrirati 1.1.1 prvi put, a zatim primijenite sljedeći postupak

### <a name="nuget-package"></a>Paket Nuget

Da biste zamijenili **Capptain.WindowsPhone** u **MicrosoftAzure.MobileEngagement** Nuget paketa.

### <a name="applying-mobile-engagement"></a>Primjena mobilne radnje

SDK koristi termin `Engagement`. Trebate ažurirati projekta u skladu tu promjenu.

Morate deinstalirati trenutnog Capptain nuget paketa. Preporučujemo da se sve promjene u mapi Capptain resursi će se ukloniti. Ako želite zadržati te datoteke, a zatim kopirajte ih.

Nakon toga, instalirajte Microsoft Azure Engagement nuget paket na projektu. Možete je pronaći izravno na [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Ova akcija zamjenjuje sve datoteke resursa koje koristi radnje i dodaje novi DLL radnje reference projekta.

Imate da biste očistili reference projekta brisanjem Capptain DLL reference. Ako se ne bi, verziju Capptain bit će u sukobu i dogodit će se pogreške.

Ako ste prilagodili Capptain resursa, kopirajte stari sadržaj datoteke, pa ih zalijepite u nove datoteke radnje. Napominjemo da morate ažurirati xaml i cs datoteke.

Nakon tih koraka samo morate zamijeniti stari Capptain reference tako da novi reference radnje.

1. Svi prostori naziva Capptain, morate ažurirati.

    Prije migracije:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Nakon migracije:
    
        using Microsoft.Azure.Engagement;

2. Svi Tečajevi Capptain koji sadrže "Capptain" mora sadržavati "Radnje".

    Prije migracije:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Nakon migracije:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Xaml datoteke Capptain prostor naziva i atributi i promijenite.

    Prije migracije:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Nakon migracije:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Za ostale resurse kao Capptain slika, uzmite u obzir da su također biti preimenovana da biste koristili "Radnje".

### <a name="application-id--sdk-key"></a>ID aplikacije / SDK ključ

Radnje koristi niz za povezivanje. Ne morate navesti ID aplikacije i ključa SDK s mogućnostima Mobile, imate samo za određivanje niza za povezivanje. Možete je postaviti tako na EngagementConfiguration datoteku.

Konfiguracija radnje možete postaviti u svoje `Resources\EngagementConfiguration.xml` datoteke u projekt.

Uređivanje ove datoteke da biste odredili:

-   Aplikacija niz za povezivanje u između oznake `<connectionString>` i `<\connectionString>`.

Ako želite da biste odredili je prilikom izvođenja, možete pozvati sljedeću metodu prije agent za inicijalizaciju radnje:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

Niz za povezivanje za svoju aplikaciju prikazuje se na portalu klasični Azure.

### <a name="items-name-change"></a>Promjena naziva stavki

Sve stavke pod nazivom *capptain* sadrže je pod nazivom *radnje*. Na sličan način za *Capptain* za *radnje*.

Primjeri najčešće korištenih Capptain stavki:

-   CapptainConfiguration sada pod nazivom EngagementConfiguration
-   CapptainAgent sada pod nazivom EngagementAgent
-   CapptainReach sada pod nazivom EngagementReach
-   CapptainHttpConfig sada pod nazivom EngagementHttpConfig
-   GetCapptainPageName sada pod nazivom GetEngagementPageName

Imajte na umu da je preimenovati i utječe na nadjačati metode.



 
