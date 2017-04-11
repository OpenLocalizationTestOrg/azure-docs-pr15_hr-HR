<properties 
    pageTitle="Praćenje ovisnosti, iznimke i vremena izvođenja u web-aplikacijama Java" 
    description="Prošireni nadzor Java web-mjesta pomoću aplikacije uvida" 
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
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Praćenje ovisnosti, iznimke i vremena izvođenja u web-aplikacijama Java

*Aplikacija uvida je u pretpregledu.*

Ako imate [instrumented web-aplikaciju programa Java s računala uvida][java], Java Agent možete koristiti da biste dobili detaljnije uvide, bez ikakvih promjena kod:


* **Ovisnosti:** Podaci o pozivima čime aplikacija na druge komponente, uključujući:
 * **OSTALE pozive** ostvareno HttpClient, OkHttp i RestTemplate (Proljetna).
 * **Redis** pozive putem klijentskog programa Jedis. Ako poziv traje dulje nego 10s, agenta dohvaćanja argumentima poziva.
 * **[Pozivi JDBC](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB ili Apache Derby DB. podržane su pozive "executeBatch". MySQL i PostgreSQL, ako poziv traje dulje nego 10s, agenta izvješća plan upita. 
* **Otkrivena iznimke:** Podaci o iznimke koje rješava kod.
* **Vrijeme izvođenja metoda:** Podaci o na vrijeme potrebno za izvršavanje određene metode.

Da biste koristili Java agent, ste ga instalirati na poslužitelj. Aplikacija web mora biti instrumented s [Računala uvida Java SDK][java].

## <a name="install-the-application-insights-agent-for-java"></a>Instalacija aplikacije uvida agent za Java

1. Na računalu pokrenut poslužitelj Java [agenta za preuzimanje](https://aka.ms/aijavasdk).
2. Uređivanje skriptu za pokretanje aplikacije poslužitelja i dodajte sljedeće JVM:

    `javaagent:`*cijeli put do datoteke POSUDU agent*

    Na primjer, u Tomcat na računalo Linux:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. Ponovno pokrenite poslužitelj aplikacije.

## <a name="configure-the-agent"></a>Konfiguriranje agenta

Stvaranje datoteku pod nazivom `AI-Agent.xml` i njegovo stavljanje u istu mapu kao agent POSUDU datoteku.

Postavite sadržaj xml datoteke. Uređivanje u sljedećem primjeru da biste uvrstili ili izostavili značajke koju želite. 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

Morate omogućiti iznimku izvješća i tempiranje način za pojedinačne metode.

Prema zadanim postavkama `reportExecutionTime` vrijedi i `reportCaughtExceptions` je vrijednost false.

## <a name="view-the-data"></a>Prikaz podataka

U resursa aplikacije uvida Zbrojeno udaljene ovisnost način izvođenja vremena i pojavit će se [u odjeljku pločicu performanse][metrics]. 

Da biste potražili pojedinačne instance ovisnost, iznimku i način izvješća, otvorite [pretraživanje][diagnostic]. 

[Dijagnosticiranje problema ovisnost – dodatne informacije](app-insights-dependencies.md#diagnosis).



## <a name="questions-problems"></a>Pitanja? Problemi?

* Nema podataka? [Postavljanje iznimke vatrozida](app-insights-ip-addresses.md)
* [Otklanjanje poteškoća s Java](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
