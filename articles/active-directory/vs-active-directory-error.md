<properties 
    pageTitle="Pogreška tijekom provjere autentičnosti" 
    description="Čarobnjak za povezivanje servisa active directory otkrio je nekompatibilan vrsta provjere autentičnosti" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="error-during-authentication-detection"></a>Pogreška tijekom provjere autentičnosti

Prilikom otkrivanja prethodne kod provjere autentičnosti čarobnjaka otkriven je nekompatibilan vrsta provjere autentičnosti.   

##<a name="what-is-being-checked"></a>Što je u tijeku?

**Bilješke:** Da bi se ispravno prepoznati prethodne kod provjere autentičnosti u projektu, morate ugrađen u projekt.  Ako je naišao na tu pogrešku i nemate prethodne kod provjere autentičnosti u projektu, ponovno stvaranje pa pokušajte ponovno.

###<a name="project-types"></a>Vrsta projekta

Čarobnjak provjerava koju vrstu projekta razvijate tako da ga ubaciti logike desnom provjere autentičnosti u projekt.  Ako je bilo koji kontroler koji potječe iz `ApiController` u programu project, projekt smatrat će se WebAPI projekta.  Ako postoje samo kontrolera proizlaziti iz `MVC.Controller` u programu project, projekt će se uvrstiti u projektu programa MVC.  Ništa nije podržan u čarobnjaku za.  Projekti WebForms trenutno nisu podržani.

###<a name="compatible-authentication-code"></a>Kod kompatibilni provjera autentičnosti

Čarobnjak također provjerava postavke provjere autentičnosti koje ste prethodno konfiguriran pomoću čarobnjaka za ili su kompatibilni s čarobnjaka.  Ako postoje sve postavke, smatra se re-entrant slučaja i čarobnjak će se otvoriti i prikaz postavki.  Ako samo neke postavke su prisutne, smatra slučaj pogreške.

U projektu programa MVC provjerava Čarobnjak za bilo koju od sljedećih postavki koje proizlaze iz prethodne pomoću čarobnjaka za:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Uz to, Čarobnjak provjerava za bilo koju od sljedećih postavki u projektu Web API, koji proizlaze iz prethodne pomoću čarobnjaka za:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

###<a name="incompatible-authentication-code"></a>Kod kompatibilni provjera autentičnosti

Na kraju, čarobnjak Potraži verzijama kod provjere autentičnosti podešena sa starijim verzijama programa Visual Studio. Ako ste primili ovu pogrešku, to znači da projekt sadrži vrstu provjere autentičnosti kompatibilan. Čarobnjak otkrije sljedeće vrste provjere autentičnosti iz prethodnih verzija programa Visual Studio:

* Provjera autentičnosti sustava Windows 
* Pojedinačne korisničke račune 
* Računi tvrtke ili ustanove 
 

Da biste otkrili provjeru autentičnosti sustava Windows u projektu MVC, čarobnjak traži u `authentication` element **web.config** datoteke.

<pre>
    &lt;Konfiguriranje&gt;
        &lt;sadrži&gt;
            <span style="background-color: yellow">&lt;način provjere autentičnosti = "Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

Da biste otkrili provjeru autentičnosti sustava Windows u programu project Web API-JA, čarobnjak traži u `IISExpressWindowsAuthentication` element iz datoteke u projekt **.csproj** :

<pre>
    &lt;Project&gt;
&lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;omogućeno&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup > &lt;/projekta        &gt;
</pre>

Da biste otkrili provjere autentičnosti za pojedinačne korisničke račune, čarobnjak traži element paketa iz datoteke **Packages.config** .

<pre>
    &lt;paketi&gt;
        <span style="background-color: yellow">&lt;pakiranje id="Microsoft.AspNet.Identity.EntityFramework" verzija = "2.1.0" targetFramework = "net45" /&gt;</span>
    &lt;/paketi&gt;
</pre>

Da biste otkrili stare obrasca provjere autentičnosti za račun tvrtke ili ustanove, čarobnjak traži sljedeći element iz **web.config**:

<pre>
    &lt;Konfiguriranje&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;dodavanje ključa = vrijednost "ida: lokalni" = "***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

Da biste promijenili vrsta provjere autentičnosti, uklonite vrsta kompatibilan provjere autentičnosti i ponovno pokrenite čarobnjak.

Dodatne informacije potražite u članku [Provjera autentičnosti scenariji za Azure AD](active-directory-authentication-scenarios.md).