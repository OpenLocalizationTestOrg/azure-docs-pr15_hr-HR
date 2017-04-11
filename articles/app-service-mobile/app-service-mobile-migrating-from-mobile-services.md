<properties
    pageTitle="Migrirati iz mobilne usluge u mobilnoj aplikaciji aplikacije servisa"
    description="Saznajte kako jednostavno migrirati aplikaciju mobilnih usluga mobilne aplikacije servisa aplikacija"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="adrianha"/>

# <a name="article-top"></a>Migracija postojeće Azure mobilne usluge u aplikacije servisa za Azure

[Općenito dostupan Azure aplikacije servisa]Azure mobilne usluge web-mjesta možete jednostavno prenositi lokalno iskoristiti sve značajke aplikacije servisa Azure.  Ovaj dokument objašnjava što mogu očekivati kada migriranje web-mjesta servisa Azure mobilne aplikacije servisa za Azure.

## <a name="what-does-migration-do"></a>Što radi migracije? na web-mjesto

Migracija Mobile servisa Azure postane mobilne usluge aplikaciju [Aplikacije servisa za Azure] bez utjecaja kod.  Vaš koncentratora obavijesti, SQL podatkovne veze, postavke provjere autentičnosti, Zakazani zadaci i naziv domene se ne mijenjaju.  Mobilni klijenti putem servisa za mobilne Azure da normalno funkcionira.  Migraciju pokreće na servisu kada prenijeti aplikacije servisa za Azure.

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Zašto treba migrirati web-mjesta

Microsoft je preporučiti migracije Azure mobilne usluge da biste iskoristili značajke Azure aplikacije servisa, uključujući:

  *  Novi glavno računalo značajkama, uključujući [WebJobs] i [prilagođenih naziva domena].
  *  Veza s lokalnog resursa pomoću [VNet] osim [Hibridnog veze].
  *  Nadzor i otklanjanje poteškoća s nove Relic ili [Uvida aplikacije].
  *  Ugrađeni DevOps tooling, uključujući [pripremna slobodnih], snimljene fotografije natrag i u radnog testiranja.
  *  [Automatsko skaliranje]opterećenja te [praćenje performansi].

Dodatne informacije o prednostima koje nudi aplikacije servisa za Azure potražite u sintaksi [Mobilne usluge nasuprot aplikacije usluge] .

## <a name="before-you-begin"></a>Prije početka

Prije početka napravili glavne na web-mjestu, trebali biste [sigurnosno kopiranje mobilne usluge] skripte i SQL baze podataka.

## <a name="migrating-site"></a>Migriranje web-mjesta

Postupak migracije prenosi sva web-mjesta unutar jedno područje Azure.

Migriranje web-mjesta:

  1.  Prijavite se na [Portal za Azure klasični].
  2.  Odaberite mobilne usluge u području koje želite migrirati.
  3.  Kliknite gumb za **migriranje na servis za aplikacije** .

    ![Gumb za migriranje][0]

  4.  Migriranje dijaloški okvir aplikacije servisa za čitanje.
  5.  Unesite naziv mobilne usluge u odgovarajući okvir.  Na primjer, ako je naziv vaše domene contoso.azure mobile.net, zatim unesite _contoso_ u odgovarajući okvir.
  6.  Kliknite gumb na osi.

Praćenje statusa migracije u Nadzorniku aktivnosti. Web-mjesto prikazuju u obliku *migracije* na portalu klasični Azure.

  ![Nadzornik aktivnosti migracije][1]

Svaki Migracija može potrajati bilo kojeg mjesta od 3 15 minuta po migrira servis za mobilne uređaje.  Web-mjesto postaje dostupan tijekom migracije.
Web-mjesto je ponovno pokrenuti na kraju postupka migracije.  Web-mjesto nije dostupna tijekom postupka ponovnog pokretanja koji mogu potrajati nekoliko sekundi.

## <a name="finalizing-migration"></a>Dovršavanje migracije

Plan za testiranje web-mjestu putem mobilnog klijentskog po završetku migracije.  Provjerite je li možete izvršiti sve uobičajenih akcija klijenta bez promjene mobilnu verziju klijentskog.  

### <a name="update-app-service-tier"></a>Odaberite odgovarajuću aplikaciju servisa cijene sloju

Imate više fleksibilnosti u cijene nakon migriranje aplikacije servisa za Azure.

  1.  Prijavite se na [portal za Azure].
  2.  Odaberite **sve resurse** ili **Aplikacije servisa** , a zatim kliknite naziv migriranim mobilne usluge.
  3.  Otvorit će se postavke plohu prema zadanim postavkama.
  4.  Kliknite **Aplikaciju servisa Plan** na izborniku postavke.
  5.  Kliknite pločicu **Cijene sloju** .
  6.  Kliknite pločicu odgovara potrebama, a zatim **Odaberite**.  Možda ćete morati kliknite **Prikaži sve** da biste vidjeli dostupne cijene razine.

Kao početnu točku, preporučujemo sljedeće razine:

| Cijene sloju servis za mobilne uređaje | Aplikacije servisa za cijene sloju |
| :-------------------------- | :----------------------- |
| Besplatni                        | Besplatni F1                  |
| Osnovni                       | Osnovni B1                 |
| Standardna                    | Standardna S1              |

Odabir s desne strane cijene sloju za aplikaciju je značajnu fleksibilnost.  Pogledajte [Cijene za aplikacije servisa] za sve detalje na cijene vašem novom servisu aplikacije.

> [AZURE.TIP]Aplikacije servisa standardne sloju sadrži pristup brojne značajke koje želite koristiti, uključujući [pripremna slobodnih], automatskog sigurnosnog kopiranja i automatsko skaliranje.  Pogledajte nove mogućnosti dok radite li!

### <a name="review-migration-scheduler-jobs"></a>Pregled zadataka migriranim rasporeda

Raspored zadataka neće biti vidljiv dok se ne više od 30 minuta nakon migracije.  Zakazani zadaci i dalje se izvoditi u pozadini.
Da biste pogledali Zakazani zadaci nakon što su vidljivi ponovno:

  1.  Prijavite se na [portal za Azure].
  2.  Odaberite **Pregled >**, unesite **raspored** u okvir _Filtar_ , a zatim odaberite **Raspored zbirke**.

Postoje ograničen broj besplatne raspored zadataka dostupna poslije migracije.  Pregledajte Upotreba i [Tarife za Azure raspored].

### <a name="configure-cors"></a>Konfiguriranje CORS ako je potrebno

Zajedničko korištenje resursa unakrsno polazište je postupak da biste omogućili web-mjesto da biste pristupili Web API na drugu domenu.  Ako koristite Azure mobilnih usluga s pridruženim web-mjestom, morate konfigurirati CORS kao dio migracije.  Ako pristupate Azure mobilne usluge isključivo putem mobilnog uređaja, zatim CORS ne morate konfigurirati osim u rijetko slučajeva.

Postavke migriranim CORS su dostupni kao aplikacije postavku **MS_CrossDomainWhitelist** .  Da biste funkciji CORS aplikacije servisa za migraciju na web-mjesto:

  1.  Prijavite se na [portal za Azure].
  2.  Odaberite **sve resurse** ili **Aplikacije servisa** , a zatim kliknite naziv migriranim mobilne usluge.
  3.  Otvorit će se postavke plohu prema zadanim postavkama.
  4.  Kliknite **CORS** na izborniku API-JA.
  5.  Unesite sve drugačijeg izvora dopušteno u odgovarajući okvir, pritisnite tipku Enter nakon svake.
  6.  Kada je na popisu dopušteni drugačijeg izvora ispravne, kliknite gumb Spremi.

> [AZURE.TIP]Jedna od prednosti korištenja programa aplikacije servisa za Azure je da možete pokrenuti web-mjesta i servis za mobilne uređaje na istom mjestu.  Dodatne informacije potražite u odjeljku [sljedeće korake](#next-steps) .

### <a name="download-publish-profile"></a>Preuzimanje novog profila za objavljivanje

Prilikom migracije aplikacije servisa za Azure, promijenit će se profil za objavljivanje web-mjesta.  Ako namjeravate objaviti na web-mjesta iz programa Visual Studio, morat ćete novi profil za objavljivanje.  Da biste preuzeli novi profil za objavljivanje:

  1.  Prijavite se na [portal za Azure].
  2.  Odaberite **sve resurse** ili **Aplikacije servisa** , a zatim kliknite naziv migriranim mobilne usluge.
  3.  Kliknite **Dohvati objavljivanje profila**.

Datoteka PublishSettings je preuzeti na računalo.  To se obično naziva _NazivWebMjesta_. PublishSettings.  Uvoz postavki Objavi u postojeći projekt:

  1.  Otvorite Visual Studio i Azure mobilne usluge projekta.
  2.  Desnom tipkom miša kliknite projekt u **Pregledniku rješenja** , a zatim odaberite **Objavi...**
  3.  Kliknite **Uvezi**
  4.  Kliknite **Pregledaj** , a zatim odaberite vaše preuzete objavljivanje datoteka postavki.  Kliknite **u redu**
  5.  Kliknite **Provjera valjanosti vezu** da biste vidjeli rad postavke objavljivanja.
  6.  Kliknite **Objavi** za objavljivanje web-mjesta.


## <a name="working-with-your-site"></a>Rad s web-mjesta poslije migracije

Započnite rad s vašem novom servisu aplikacije u [Azure portal] poslije migracije.  Slijede neki bilješke na određene operacije koje koriste se [Klasične Portal Azure]zajedno s njihove ekvivalentne aplikacije servisa.

### <a name="publishing-your-site"></a>Preuzimanje i migriranim web-mjesta za objavljivanje

Web-mjesto je dostupan putem brojka ili ftp i možete ponovno objaviti s razne mehanizme za različite, uključujući WebDeploy, TFS, Mercurial, GitHub i FTP.  Vjerodajnice za implementaciju se zajedno s ostalim korisnicima u web-mjesta.  Ako koje postaviti vjerodajnice za implementaciju ili ih ne sjećate, možete ih ponovno postaviti:

  1. Prijavite se na [portal za Azure].
  2. Odaberite **sve resurse** ili **Aplikacije servisa** , a zatim kliknite naziv migriranim mobilne usluge.
  3. Otvorit će se postavke plohu prema zadanim postavkama.
  4. Kliknite **vjerodajnice za implementaciju** u OBJAVLJIVANJE izbornika.
  5. Unesite novu vjerodajnice za implementaciju u odgovarajuće okvire, a zatim kliknite gumb Spremi.

Možete koristiti te vjerodajnice za Kloniraj web-mjesta s brojka ili postavljanje automatskog implementacijama GitHub, TFS ili Mercurial.  Dodatne informacije potražite u [dokumentaciji za implementaciju aplikacije servisa za Azure].

### <a name="appsettings"></a>Postavke aplikacije

Većina postavki za migriranim Mobilna usluga dostupnih putem postavki aplikacije.  Popis postavki aplikacije možete doći s [portala za Azure].
Da biste pogledali ili promjena postavki aplikacije:

  1. Prijavite se na [portal za Azure].
  2. Odaberite **sve resurse** ili **Aplikacije servisa** , a zatim kliknite naziv migriranim mobilne usluge.
  3. Otvorit će se postavke plohu prema zadanim postavkama.
  4. Kliknite **Postavke aplikacije** na izborniku OPĆENITO.
  5. Pomaknite se do odjeljka postavki aplikacije i pronaći postavke aplikacije.
  6. Kliknite vrijednost postavke aplikacije da biste uredili vrijednost.  Kliknite **Spremi** da biste spremili vrijednost.

Istodobno možete ažurirati više postavki aplikacije.

> [AZURE.TIP]Postoje dvije postavke računala s istom vrijednošću.  Na primjer, vidjet ćete _ApplicationKey_ i _MS\_ApplicationKey_.  Ažurirajte i postavke aplikacije u isto vrijeme.

### <a name="authentication"></a>Provjera autentičnosti

Postavke provjere autentičnosti za sve dostupne kao postavki aplikacije s migriranim web-mjesta.  Da biste ažurirali postavke provjere autentičnosti, morate promijeniti postavke odgovarajuće aplikacije.  Sljedeća tablica prikazuje postavki odgovarajuće aplikacije za davatelja usluge provjere autentičnosti:

| Davatelj usluga          | ID klijenta                 | Tajna klijenta                | Ostale postavke             |
| :---------------- | :------------------------ | :--------------------------- | :------------------------- |
| Microsoftov račun | **MS\_MicrosoftClientID**  | **MS\_MicrosoftClientSecret** | **MS\_MicrosoftPackageSID** |
| Facebook          | **MS\_FacebookAppID**      | **MS\_FacebookAppSecret**     |                            |
| Twitter           | **MS\_TwitterConsumerKey** | **MS\_TwitterConsumerSecret** |                            |
| Google            | **MS\_GoogleClientID**     | **MS\_GoogleClientSecret**    |                            |
| Azure AD          | **MS\_AadClientID**        |                              | **MS\_AadTenants**          |

Napomena: **MS\_AadTenants** pohranjen kao odvojenih zarezom popis domena klijenta ("Dopušteno klijenata" polja na portalu za mobilne usluge).

> [AZURE.WARNING] **Korištenje Mehanizmi za provjeru autentičnosti na izborniku postavke**
>
> Aplikacije servisa za Azure nudi zaseban "bez koda" provjere autentičnosti i autorizacije sustav pod na _provjere autentičnosti / autorizacije_ izbornik postavke, a mogućnost _Mobile provjere autentičnosti_ (zastarjeli) na izborniku postavke.  Ove mogućnosti nisu kompatibilni s migriranim Azure mobilne usluge.  Možete iskoristiti provjere autentičnosti aplikacije servisa za Azure [nadogradnje web-mjesta](app-service-mobile-net-upgrading-from-mobile-services.md) .

### <a name="easytables"></a>Podataka

Na kartici _Podaci_ u mobilne usluge zamijenjen je _Tablice jednostavna_ unutar Azure portal.  Da biste pristupili jednostavno tablice:

  1. Prijavite se na [portal za Azure].
  2. Odaberite **sve resurse** ili **Aplikacije servisa** , a zatim kliknite naziv migriranim mobilne usluge.
  3. Otvorit će se postavke plohu prema zadanim postavkama.
  4. Kliknite **jednostavno tablice** na izborniku MOBILNE.

Možete dodati tablicu tako da kliknete gumb **Dodaj** ili pristupiti postojeće tablice tako da kliknete naziv tablice.  Postoje razne operacije koje možete stvoriti u ovom plohu, uključujući:

* Promjena dozvola za tablice
* Uređivanje radu skripti
* Upravljanje shemom tablice
* Brisanje tablice
* Čišćenje sadržaja tablice
* Brisanje određenih redaka tablice

### <a name="easyapis"></a>API-JA

Na kartici _API-JA_ u mobilne usluge zamijenjen je _Lako API -ji_ unutar Azure portal.  Da biste pristupili API-je jednostavno:

  1. Prijavite se na [portal za Azure].
  2. Odaberite **sve resurse** ili **Aplikacije servisa** , a zatim kliknite naziv migriranim mobilne usluge.
  3. Otvorit će se postavke plohu prema zadanim postavkama.
  4. Kliknite **Jednostavno API -ji** na izborniku MOBILNE.

Migriranim API-ji navedene su već u na plohu.  API možete dodati i iz ovog plohu.  Da biste upravljali određene API-JA, kliknite na API-JA.
Na novu plohu možete prilagoditi dozvole i urediti skripte na API-JA.

### <a name="on-demand-jobs"></a>Raspored zadataka

Sve zadatke raspored dostupni su putem u odjeljku raspored posla zbirke.  Da biste pristupili poslova rasporeda:

  1. Prijavite se na [portal za Azure].
  2. Odaberite **Pregled >**, unesite **raspored** u okvir _Filtar_ , a zatim odaberite **Raspored zbirke**.
  3. Odaberite zadatak zbirku web-mjesta.  Zove _NazivWebMjesta_-zadatke.
  4. Kliknite **Postavke**.
  5. U odjeljku Upravljanje kliknite **Raspored zadataka** .

Zakazani zadaci navedene su s učestalosti koja je određena prije migracije.  Onemogućene su zadatke na zahtjev.  Da biste pokrenuli programa posla na zahtjev:

  1. Odaberite zadatak koji želite pokrenuti.
  2. Ako je potrebno, kliknite **Omogući** da biste omogućili posao.
  3. Kliknite **Postavke**, a zatim **raspored**.
  4. Odaberite ponavljanje od **jednom**, a zatim kliknite **Spremi**

Na zadatke na zahtjev nalaze se u `App_Data/config/scripts/scheduler post-migration`.  Preporučujemo da pretvoriti sve zadatke na zahtjev za [WebJobs] ili [funkcije].  Upišite novi raspored zadataka kao [WebJobs] ili [funkcije].

### <a name="notification-hubs"></a>Obavijest o koncentratora

Mobilna usluga koristi obavijesti koncentratora za slanje obavijesti.  Sljedeće postavke aplikacije koriste da biste se povezali središtu obavijesti mobilne usluge nakon migracije:

| Postavljanje aplikacije                    | Opis                              |
| :------------------------------------- | :--------------------------------------- |
| **MS\_PushEntityNamespace**             | Prostor za naziv koncentrator obavijesti           |
| **MS\_NotificationHubName**             | Naziv koncentrator obavijesti                |
| **MS\_NotificationHubConnectionString** | Niz za povezivanje koncentrator obavijesti   |
| **MS\_NamespaceName**                   | Pseudonim za MS_PushEntityNamespace      |

Koncentratora za obavijesti upravlja se putem [portala za Azure].  Imajte na umu naziv koncentrator obavijesti (možete pronaći to pomoću postavki aplikacije):

  1. Prijavite se na [portal za Azure].
  2. Odaberite **Pregled**>, odaberite **Koncentratora obavijesti**
  3. Kliknite naziv obavijesti koncentrator pridružene servis za mobilne uređaje.

> [AZURE.NOTE]Ako je koncentratora za obavijesti o vrsti "Miješano", nije vidljiv.  "Kombiniranim" upišite obavijesti koncentratora koristiti koncentratora obavijesti i naslijeđenih značajke Bus servisa.  [Pretvaranje kombiniranim prostore za naziv] prije nastavka.  Nakon dovršetka pretvorbe koncentratora za obavijesti pojavit će se na [portal za Azure].

Dodatne informacije, pregledajte dokumentaciju [Koncentratora obavijesti] .

> [AZURE.TIP]Značajke upravljanja koncentratora obavijesti [Azure portal] su i dalje u pretpregledu.  [Klasični Portal Azure] postaje dostupan za upravljanje sve koncentratora za obavijesti.

### <a name="legacy-push"></a>Postavke naslijeđene pribadače

Ako ste konfigurirali automatske na svoje mobilne usluge prije Uvod na koncentratora obavijesti, koristite _naslijeđene pribadače_.  Ako koristite automatske, a ne vidite obavijest o koncentrator naveden u konfiguraciji, tada je vjerojatno koristite _naslijeđene pribadače_.  Značajka je zajedno s sve druge značajke.  No preporučujemo da nadograđujete s koncentratorima obavijesti uskoro nakon dovršetka migracije.

U privremeni, dostupne u postavki aplikacije su sve postavke naslijeđene automatske (s najvažnije iznimku certifikata za APN-ove).  Ažurirajte certifikata za APN-ove zamjenjujući odgovarajuće datoteke na na datotečnom sustavu.

### <a name="app-settings"></a>Ostale postavke za aplikaciju

Sljedeće postavke dodatne aplikacije su migriranim iz mobilne usluge i u odjeljku *Postavke* > *Postavki aplikacije*:

| Postavljanje aplikacije              | Opis                             |
| :------------------------------- | :-------------------------------------- |
| **MS\_MobileServiceName**         | Naziv aplikacije                    |
| **MS\_MobileServiceDomainSuffix** | Prefiks domene. i.e Azure mobile.net |
| **MS\_ApplicationKey**            | Ključ za aplikaciju                    |
| **MS\_glavnog ključa**                 | Matrica ključ aplikacije                     |

Ključ za aplikacije i glavni ključ jednaki tipki aplikacije iz izvorne mobilne usluge.  Posebno Aplikacijsku tipku je poslao mobilni klijenti za provjeru valjanosti njihova upotreba mobilne API-JA.

### <a name="cliequivalents"></a>Zamjenske vrijednosti za naredbenog retka

Više možete koristiti naredbu _azure mobilne_ za upravljanje web-mjestu servisa Azure Mobile.  Umjesto toga mnoge funkcije zamijenjene naredbu _azure web-mjesta_ .  Koristite sljedeću tablicu da biste pronašli Ekvivalenti za uobičajene naredbe:

| _Azure Mobile_ Naredba                     | Ekvivalentan Azure naredba _Web-mjesta_            |
| :----------------------------------------- | :----------------------------------------- |
| Mobilni mjesta                           | Popis lokacija web-mjesta                         |
| mobilnog popisa                                | Popis web-mjesta                                  |
| Mobilni prikaz _naziva_                         | Prikaz web-mjesta _naziv_                           |
| ponovno pokretanje računala mobilne _naziv_                      | ponovno pokretanje web-mjesta _naziv_                        |
| Mobilni redeploy _naziv_                     | web-mjesta implementaciju redeploy _commitId_ _naziv_ |
| Mobilni ključ postavite _naziv_ _vrste_ _vrijednosti_       | _tipka_ delete za web-mjesta appsetting _naziv_ <br/> Dodavanje web-mjesta appsetting _ključa_=_vrijednosti_ _naziv_ |
| Mobilni config popisa _naziva_                  | Popis web-mjesta appsetting _naziv_                |
| Mobilni config dobiti _naziv_ _ključ_             | web-mjesta appsetting Prikaži _ključ_ _naziva_          |
| Mobilni konfiguracije postavite _naziva_ _ključ_             | _tipka_ delete za web-mjesta appsetting _naziv_ <br/> Dodavanje web-mjesta appsetting _ključa_=_vrijednosti_ _naziv_ |
| Mobilni domene popis _naziva_                  | domene popis _naziva_ web-mjesta                    |
| Mobilni domene dodajte _naziv_ _domene_          | web-mjesta domene dodavanje _domene_ _naziv_            |
| Brisanje mobilnog domene _naziv_                | web-mjesta domene Izbriši _domene_ _naziv_         |
| Mobilni skaliranje Prikaži _naziv_                   | Prikaz web-mjesta _naziv_                           |
| Mobilni skaliranje promjena _naziva_                 | _način rada_ način rada za web-mjesta skaliranje _naziv_ <br /> web-mjesta skaliranje instance _instance_ _naziva_ |
| Mobilni appsetting popisa _naziva_              | Popis web-mjesta appsetting _naziv_                |
| Mobilni appsetting dodajte _ključ_ _naziva_ _vrijednost_ | Dodavanje web-mjesta appsetting _ključa_=_vrijednosti_ _naziv_   |
| Mobilni appsetting Izbriši _naziva_ _ključ_      | _tipka_ delete za web-mjesta appsetting _naziv_        |
| Mobilni appsetting Prikaži _naziv_ _ključ_        | _tipka_ delete za web-mjesta appsetting _naziv_        |

Ažurirajte provjeru autentičnosti ili postavki automatske obavijesti ažuriranjem odgovarajuću postavku aplikacije.
Uređivanje datoteke i objavljivanje web-mjesta putem ftp ili brojka.

### <a name="diagnostics"></a>Za dijagnostiku i zapisivanje

Zapisivanje dijagnostičkih podataka obično je onemogućen na servisu Azure aplikacije.  Uključite Zapisivanje dijagnostičkih podataka:

  1. Prijavite se na [portal za Azure].
  2. Odaberite **sve resurse** ili **Aplikacije servisa** , a zatim kliknite naziv migriranim mobilne usluge.
  3. Otvorit će se postavke plohu prema zadanim postavkama.
  4. Na izborniku značajke odaberite **Dijagnostičke zapisnike** .
  5. Kliknite **na** sljedeće zapisnici: **Aplikacija zapisivanje (datotečnom sustavu)**, **detaljnih poruka o pogreškama**i **praćenje zahtjeva nije uspjelo**
  6. Kliknite **Datotečnom sustavu** za zapisivanje poslužitelja za Web
  7. Kliknite **Spremi**

Da biste pogledali zapisnike:

  1. Prijavite se na [portal za Azure].
  2. Odaberite **sve resurse** ili **Aplikacije servisa** , a zatim kliknite naziv migriranim mobilne usluge.
  3. Kliknite gumb **Alati**
  4. Na izborniku OBSERVE odaberite **strujanje zapisnika** .

Zapisnici prikazuju se u prozoru kao što su generiraju.  Možete preuzeti i zapisnika za kasniju analizu pomoću vjerodajnica za implementaciju. Dodatne informacije potražite u dokumentaciji za [bilježenje u zapisnik] .

## <a name="known-issues"></a>Poznati problemi

### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Brisanje Kloniraj aplikacije migrirati Mobile uzrokuje nedostupnosti za web-mjesta

Ako vam Kloniraj vaše migriranim mobilne usluge pomoću komponente PowerShell Azure, a zatim izbrišite na Kloniraj stavka DNS servisa radnog nestaje.  Web-mjesto je više neće biti dostupno putem Interneta.  

Razlučivost: Ako želite Kloniraj web-mjesta, učinite to putem portala sustava.

### <a name="changing-webconfig-does-not-work"></a>Promjena Web.config ne funkcionira

Ako imate ASP.NET web-mjesta, mijenja se `Web.config` datoteka se primjenjuje.  Sastavlja aplikacije servisa Azure na odgovarajuću `Web.config` datoteke prilikom pokretanja za podršku runtime mobilne usluge.  Određene postavke (kao što su prilagođene zaglavlja) možete nadjačati pomoću XML datoteku pretvorbe.  Stvaranje datoteke u naziva `applicationHost.xdt` -ove datoteke mora završe u na `D:\home\site` imeničkog na servisu Azure.  Prijenos na `applicationHost.xdt` datoteke putem prilagođene implementacijsku skriptu ili izravno pomoću Kudu.  Na sljedećoj je slici prikazan u primjeru dokument:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Dodatne informacije potražite u dokumentaciji za [Pretvaranje uzoraka XDT] na GitHub.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Migriranim mobilnih usluga ne mogu dodati upravitelju promet

Kada stvorite promet upravitelj profila, ne možete izravno odabrati migriranim servis za mobilne uređaje u profil.  Korištenje "vanjski krajnje."  Vanjski krajnje točke mogu se dodati samo putem komponente PowerShell.  Dodatne informacije potražite u članku [vodič Upravitelj promet](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Daljnji koraci

Sad kad se aplikacija migrira se na servis za aplikaciju, postoje dodatnim značajkama možete koristiti:

  * Uvođenje [pripremna slobodnih] dopušta faze promjene na web-mjesto i izvođenje odgovora / B testiranja.
  * [WebJobs] omogućuje zamjenu za zadatke na zahtjev zakazano.
  * Možete [neprestano implementacija] web-mjestu povezivanjem web-mjestu GitHub, TFS ili Mercurial.
  * [Uvid u aplikaciju] možete koristiti za praćenje web-mjesta.
  * Posluživanje web-mjesto i Mobile API iz iste šifre.

### <a name="upgrading-your-site"></a>Nadogradnja web-mjestu servisa Mobile Azure mobilne aplikacije SDK

  * Za projekte Node.js poslužiteljski nove [Mobilne aplikacije Node.js SDK] sadrži nekoliko novih značajki. Ako, primjerice, možete sada učinite lokalne razvoja i ispravljanje pogrešaka, koristiti bilo koja verzija Node.js iznad 0.10 i prilagoditi sve Express.js proizvod.

  * Za. Neto poslužiteljski projektima, novi [mobilne aplikacije SDK NuGet paketa](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) uključeno veću fleksibilnost NuGet ovisnosti.  Te pakete podržava provjeru autentičnosti za nove aplikacije servisa i sastavljati s bilo kojeg ASP.NET projekta. Da biste saznali više o nadogradnji, pogledajte [nadograditi svoje postojeće .NET mobilne usluge za aplikacije servisa](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[Aplikacije servisa za cijene]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Aplikacija uvida]: ../application-insights/app-insights-overview.md
[Automatsko skaliranje]: ../app-service-web/web-sites-scale.md
[Aplikacije servisa za Azure]: ../app-service/app-service-value-prop-what-is.md
[Azure dokumentaciju za implementaciju aplikacije servisa]: ../app-service-web/web-sites-deploy.md
[Azure klasični Portal]: https://manage.windowsazure.com
[Portal za Azure]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Tarife za Azure rasporeda]: ../scheduler/scheduler-plans-billing.md
[neprestano implementacije]: ../app-service-web/app-service-continuous-deployment.md
[Pretvaranje kombiniranim prostore za naziv]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[prilagođenih naziva domena]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[Općenito dostupan aplikacije servisa za Azure]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hibridno veze]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[Bilježenje u zapisnik]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobilne aplikacije Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[Usluge nasuprot aplikacije servis za mobilne uređaje]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Obavijest o koncentratora]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Praćenje performansi]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[Stvaranje sigurnosne kopije mobilne usluge]: ../mobile-services/mobile-services-disaster-recovery.md
[pripremna slobodnih]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[Uzorci XDT pretvorbe]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[Funkcija]: ../azure-functions/functions-overview.md
