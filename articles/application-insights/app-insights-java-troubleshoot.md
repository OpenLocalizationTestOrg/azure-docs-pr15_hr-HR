<properties 
    pageTitle="Otklanjanje poteškoća s računala uvida u projekta web Java" 
    description="Vodič za otklanjanje poteškoća – nadzor uživo Java aplikacija s računala uvida." 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/01/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Otklanjanje poteškoća i pitanja i odgovora za uvid aplikacije za Java

Pitanja i problemi s [Visual Studio aplikacije uvida u Java][java]? Evo nekoliko savjeta.


## <a name="build-errors"></a>Stvaranje pogreške

*U Eclipse, prilikom dodavanja SDK uvida aplikaciju putem Maven ili Gradle, se pogreške provjere valjanosti za sastavljanje ili kontrolni zbroj.*

* Ako ovisnosti <version> element je pomoću uzorka zamjenskih znakova (npr. (Maven) `<version>[1.0,)</version>` ili (Gradle) `version:'1.0.+'`), probajte navesti određenu verziju umjesto toga, kao što su `1.0.2`. Pogledajte na [Napomene](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) za najnoviju verziju.

## <a name="no-data"></a>Nema podataka 

*Uspješno dodani uvida aplikacije i pokrenuli aplikacije, ali se nikad ne vidjeli podataka na portalu.*

* Pričekajte nekoliko minuta pa kliknite Osvježi. Grafikoni osvježavanje same povremeno, ali možete i osvježiti ručno. Interval osvježavanja ovisi o vremenski raspon grafikona.
* Provjerite imate li ključa instrumentation definirana u datoteci ApplicationInsights.xml (u mapi resursa u projektu)
* Provjerite je li bez `<DisableTelemetry>true</DisableTelemetry>` čvor u xml datoteci.
* U vatrozid, možda ćete morati otvoriti TCP priključci 80 i 443 za odlazni promet za dc.services.visualstudio.com. Pogledajte [cijeli popis iznimke vatrozida](app-insights-ip-addresses.md)
* U programu Microsoft Azure pokrenite ploče, pogledajte karte stanja servisa. Ako postoje neka upozorenja indications, pričekajte dok se oni su vraćene na u redu i zatim zatvorite i ponovno otvorite svoje plohu aplikacije uvida aplikacije.
* Uključivanje zapisivanja u prozor konzole IDE dodavanjem programa `<SDKLogger />` element u odjeljku Korijenski čvor u ApplicationInsights.xml datoteke (u mapi resursa u projektu) i provjeri stavke prefaced [Pogreška].
* Pripazite da ispravnom ApplicationInsights.xml uspješno učitan je tako da Java SDK tako da pogledate na konzoli izlazne poruke za naredbu "konfiguracijska datoteka nije uspješno pronašao".
* Ako se ne pronađe konfiguracijskoj datoteci, provjerite izlazne poruke da biste vidjeli gdje traže konfiguracijska datoteka za i provjerite je li u ApplicationInsights.xml nalazi u nekom od tih mjesta za pretraživanje. Kao palca pravilo, možete postaviti konfiguracijskoj datoteci pri JARs SDK za aplikaciju uvide. Na primjer: u Tomcat, to će značiti mape WEB-INF/biblioteka.



#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Se koristi da biste vidjeli podatke, ali je on prestao

* Provjerite [status bloga](http://blogs.msdn.com/b/applicationinsights-status/).
* Imate kliknite mjesečni kvotu točaka podataka? Otvorite postavke/kvota i određivanje cijena da biste otkrili. Ako je tako, možete nadograditi tarifu ili platiti dodatne kapaciteta. Potražite u članku [cijene shemu](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Ne vidim sve podatke koje se očekuje

* Otvorite na kvote i cijene plohu i provjerite je li [navedeni](app-insights-sampling.md) postupak. (100% prijenosa znači da nije li se u operaciji uzorkovanje.) Servis uvida aplikacije možete postaviti da biste prihvatili samo razlomak telemetrijskih koji dolazi iz aplikacije. Tako ćete zadržati unutar vaše mjesečni kvota za telemetriju. 

## <a name="no-usage-data"></a>Nema podataka o korištenju

*Prikazuje podatke o zahtjeve i vremena odgovor, ali nema prikaz stranice, pregledniku ili korisničkih podataka.*

Uspješno postavljanja aplikacije da biste poslali telemetrijskih s poslužitelja. Sada se sljedeći je korak da biste [postavili web-stranica da biste poslali telemetriju u web-pregledniku][usage].

Osim toga, ako je vaš klijent aplikaciju na [telefon ili neki drugi uređaj][platforms], možete poslati telemetrijskih iz nje. 

Koristite isti ključa instrumentation da biste postavili svoje postupak klijentske i poslužiteljske telemetrijskih. Podaci će se pojaviti u isti resurs uvida aplikacije pa ćete moći povezivanje događaje iz klijenta i poslužitelja.



## <a name="disabling-telemetry"></a>Onemogućivanje telemetrijskih

*Kako onemogućiti zbirke telemetrijskih?*

U kodu:

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);


**Ili** 

Ažurirajte ApplicationInsights.xml (u mapi resursa u projektu). Dodajte sljedeće u odjeljku Korijenski čvor:

    <DisableTelemetry>true</DisableTelemetry>

Koristite način XML, morate ponovno pokrenite aplikaciju kada promijenite vrijednost.

## <a name="changing-the-target"></a>Promjena cilj

*Kako promijeniti koji Azure resursa projektu šalje podatke?*

* [Pronađite ključ instrumentation novi resurs.][java]
* Ako dodati aplikacije uvid u projekt pomoću alata za Azure Eclipse, desnom tipkom miša kliknite projekt web, odaberite **Azure**, **Konfiguriranje uvida aplikacije**i promjena ključa.
* U suprotnom, ažurirajte ključ u ApplicationInsights.xml u mapi resursa u projektu.


## <a name="the-azure-start-screen"></a>Azure početnog zaslona

*Koju tražim na [portalu za Azure](https://portal.azure.com). Ne karte informirati nešto o aplikacije?*

Ne prikazuje stanje Azure poslužitelje diljem svijeta.

*S Azure početka ploče (početnom zaslonu) kako pronaći podatke o mojoj aplikaciji?*

Pod pretpostavkom [postavili aplikacije za aplikaciju uvida][java], kliknite Pregledaj, odaberite uvida aplikacije i odaberite resursa aplikaciju koju ste stvorili za aplikacije. Da biste dobili još brže u budućnosti možete prikvačiti aplikacije ploču start.

## <a name="intranet-servers"></a>Intranet poslužitelja

*I praćenje na poslužitelju na moj intranetu?*

Da, naveden vaš poslužitelj možete poslati telemetrijskih portal za aplikacije uvida putem Interneta javno. 

U vatrozid, možda ćete morati otvoriti TCP priključci 80 i 443 za odlazni promet za dc.services.visualstudio.com i f5.services.visualstudio.com.

## <a name="data-retention"></a>Zadržavanje podataka 

*Koliko dugo podaci se zadržavaju na portalu? Je li sigurno?*

U odjeljku [zadržavanje podataka i privatnosti][data].

## <a name="next-steps"></a>Daljnji koraci

*Za poslužitelj aplikacije Java postaviti uvida aplikacije. Što još možete učiniti?*

* [Praćenje dostupnosti web-stranica][availability]
* [Nadzor korištenja web-stranice][usage]
* [Praćenje korištenja i otklanjanja poteškoća u aplikacijama za uređaj][platforms]
* [Pisanje koda za praćenje korištenja aplikacije][track]
* [Snimite dijagnostičkih zapisnika][javalogs]


## <a name="get-help"></a>Zatražite pomoć

* [Preljev stoga](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 