<properties
    pageTitle="Da biste generirali HTTP zahtjeva putem servisa za upravljanje API-JA"
    description="Saznajte kako koristiti pravila zahtjeva i odgovora u odjeljku Upravljanje API-JA za pozivanje vanjskih servisa iz vaše API-JA"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="using-external-services-from-the-azure-api-management-service"></a>Korištenje vanjskih servisa iz servisa Azure API upravljanja

Dostupni servisu Azure API Upravljanje pravilima možete učiniti širokog raspona korisne rad na temelju isključivo dolazni zahtjev, odlazne odgovor i informacije o konfiguraciji osnovni. Međutim, moći raditi s vanjskim servisi API Upravljanje pravilima otvara mnogo više mogućnosti.

Ne možemo ste prethodno vidjeti kako ćemo omogućuje interakciju sa [servisa Azure događaj koncentrator za zapisivanje, praćenje i analytics](api-management-log-to-eventhub-sample.md). U ovom članku smo će demonstrirati pravila koja omogućuju vam omogućuje interakciju s bilo kojeg vanjskih HTTP temelji servisa. Ta pravila možete koristiti za pokretanje udaljenog događaje ili za dohvaćanje informacija koje će se koristiti za rukovanje izvorne zahtjeva i odgovora na mobitel.

## <a name="send-one-way-request"></a>– Jedan-način-zahtjev za slanje
Vjerojatno najjednostavniji vanjskih interakcije je stil fire – i – zaboraviti zahtjeva koji omogućuje vanjskog servisa Obaviještenost neke vrste važne događaja. Možete koristiti pravila za tijek kontrola `choose` da biste otkrili bilo kakvu vrstu uvjet ćemo vas zanima, a zatim, ako se zadovolje uvjet, možete dajemo vanjskog HTTP zahtjeva putem pravila [– jedan-način-zahtjev za slanje](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) . To može biti zahtjev za razmjenu poruka sustava kao što su Hipchat ili prazan hod ili e-poštu API kao što su SendGrid ili MailChimp ili za ključne podršku kupljenih primjer PagerDuty. Svi ti sustavi za razmjenu nemaju jednostavne HTTP API-ji koji smo možete jednostavno pozvati.

### <a name="alerting-with-slack"></a>Upozorenjem uz Prazan hod
Sljedeći primjer pokazuje kako poslati poruku ako HTTP kod stanja odgovora je veće od ili jednako 500 zalihe sobe za razgovor. 500 raspon pogreške upućuje na problem s naše pozadinskog API koji klijent naš API-JA ne možete riješiti sami. Obično zahtijeva neke vrste intervencije na našem dijela.  

    <choose>
        <when condition="@(context.Response.StatusCode >= 500)">
          <send-one-way-request mode="new">
            <set-url>https://hooks.slack.com/services/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</set-url>
            <set-method>POST</set-method>
            <set-body>@{
                    return new JObject(
                            new JProperty("username","APIM Alert"),
                            new JProperty("icon_emoji", ":ghost:"),
                            new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                    context.Request.Method,
                                                    context.Request.Url.Path + context.Request.Url.QueryString,
                                                    context.Request.Url.Host,
                                                    context.Response.StatusCode,
                                                    context.Response.StatusReason,
                                                    context.User.Email
                                                    ))
                            ).ToString();
                }</set-body>
          </send-one-way-request>
        </when>
    </choose>

Prazan hod ima notion spojnica ulaznog web. Prilikom konfiguriranja programa priključak ulaznog web Prazan hod generira posebno URL-a koji omogućuje da biste učinili jednostavne zahtjev za objavu i prosljeđivanja poruke u zalihe kanala. Tijelo JSON koje ćemo stvoriti temelji se na oblik koji definira Prazan hod.

![Zalihe Web priključak](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Je fire i zaboraviti dovoljno dobar?
Postoje određene tradeoffs prilikom korištenja stil fire – i – zaboraviti zahtjev. Ako iz nekog razloga ne uspije zahtjev, a zatim će se prijavljivati pogreške. U tom slučaju određeni složenost pojavljuju sekundarne pogreške izvješćivanja sustava i performanse dodatnih troškova čeka se odgovor nije potrebno. Za scenarije u kojima je ključna da biste provjerili odgovor, zatim pravila [zahtjev za slanje](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) je bolje.

## <a name="send-request"></a>Zahtjev za slanje
Na `send-request` pravilo omogućuje korištenje vanjskog servisa za izvođenje složenih obrada funkcije i vraćanje podataka za upravljanje API servisa koji se može koristiti za daljnje obrade pravila.

### <a name="authorizing-reference-tokens"></a>Dopuštanja referenca tokena
Glavna funkcija API upravljanja štiti pozadinskog resursi. Ako poslužitelj autorizacije koristi vaš API stvara [JWT tokeni](http://jwt.io/) kao dio njegov tijek OAuth2 kao i [Azure Active Directory](../active-directory/active-directory-aadconnect.md) , a zatim možete koristiti u `validate-jwt` pravilo da biste provjerili valjanost tokena. Međutim, neki poslužitelji za autorizaciju stvoriti što se nazivaju [tokeni referenca](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) ne može provjeriti bez upućivanje poziva na poslužitelj za autorizaciju.

### <a name="standardized-introspection"></a>Standardizirani introspection
U prošlosti otkrivena standardizirani način potvrđivanja token reference s poslužiteljem za autorizaciju. No nedavno predloženi standardni [RFC 7662](https://tools.ietf.org/html/rfc7662) objavljen po IETF koja definira kako resursa poslužitelja možete provjeriti valjanost token.

### <a name="extracting-the-token"></a>Izdvajanje tokena
U prvi je korak da biste izdvojili tokena iz zaglavlja za autorizaciju. S koje želite oblikovati kao vrijednost zaglavlja u `Bearer` shema za provjeru autentičnosti, jedan razmak, a zatim ovlaštenja po [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Nažalost postoje slučajevi u kojima je ispušten shema za provjeru autentičnosti. Da bi se to prilikom analize, ćemo podijeliti vrijednost zaglavlja na razmak i odaberite posljednji niz vraćenom polju nizova. To postoji zaobilazno rješenje za autorizaciju pogrešno oblikovana zaglavlja.

    <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

### <a name="making-the-validation-request"></a>Upućivanje zahtjeva za provjeru valjanosti
Kada imamo token za provjeru autentičnosti, dajemo zahtjev za provjeru valjanosti token. RFC 7662 poziva ovaj postupak introspection i mora vam `POST` HTML obrasca introspection resursa. HTML obrasca barem mora sadržavati par ključa vrijednosti s ključem `token`. Zahtjev za provjeru autentičnosti poslužitelja mora se autentičnost i da biste bili sigurni da ne otvorite za valjane tokeni trawling zlonamjerni klijente.

    <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
      <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
      <set-method>POST</set-method>
      <set-header name="Authorization" exists-action="override">
        <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
      </set-header>
      <set-header name="Content-Type" exists-action="override">
        <value>application/x-www-form-urlencoded</value>
      </set-header>
      <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
    </send-request>

### <a name="checking-the-response"></a>Provjera odgovor
Na `response-variable-name` atribut se koristi da biste omogućili pristup vraćeni odgovor. Naziv definiran u svojstvo koje se mogu koristiti kao ključ u na `context.Variables` rječnik za pristup na `IResponse` objekt.

Iz objekta odgovor ne možemo dohvatiti tijelo i RFC 7622 nam govore odgovor mora biti JSON objekt, a mora sadržavati najmanje svojstvo naziva `active` odnosno Booleova vrijednost. Kada `active` je true, a zatim token smatra valjane.

### <a name="reporting-failure"></a>Izvješćivanje o pogreškama pogreške
Koristimo na `<choose>` pravila da biste otkrili ako token nije valjan, a ako je tako, vratite 401 odgovor.

    <choose>
      <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
        <return-response response-variable-name="existing response variable">
          <set-status code="401" reason="Unauthorized" />
          <set-header name="WWW-Authenticate" exists-action="override">
            <value>Bearer error="invalid_token"</value>
          </set-header>
        </return-response>
      </when>
    </choose>

Po [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) koji opisuje kako `bearer` tokeni želite koristiti, ne možemo se vratiti na `WWW-Authenticate` zaglavlja s 401 odgovor. "Www"-autentičnost je namijenjen uputiti klijenta za sastavljanje pravilno ovlašteni zahtjev. Zbog velikog broja pristupa s OAuth2 framework, je teško komunikaciju sve potrebne informacije. Srećom postoje trudu sugovornike [kako pravilno autorizirali zahtjevi za resurse poslužitelja](http://tools.ietf.org/html/draft-jones-oauth-discovery-00)za otkrivanje klijente.

### <a name="final-solution"></a>Konačni rezultat rješenja
Izgradnja sve možemo pojavljuje se sljedeća pravila:

    <inbound>
      <!-- Extract Token from Authorization header parameter -->
      <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

      <!-- Send request to Token Server to validate token (see RFC 7662) -->
      <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
        <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
        <set-method>POST</set-method>
        <set-header name="Authorization" exists-action="override">
          <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
        </set-header>
        <set-header name="Content-Type" exists-action="override">
          <value>application/x-www-form-urlencoded</value>
        </set-header>
        <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
      </send-request>

      <choose>
            <!-- Check active property in response -->
            <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
                <!-- Return 401 Unauthorized with http-problem payload -->
                <return-response response-variable-name="existing response variable">
                    <set-status code="401" reason="Unauthorized" />
                    <set-header name="WWW-Authenticate" exists-action="override">
                        <value>Bearer error="invalid_token"</value>
                    </set-header>
                </return-response>
            </when>
        </choose>
      <base />
    </inbound>

To je samo jedan od više primjera kako `send-request` pravila mogu se integrirati korisne vanjske servise u postupak zahtjeve i odgovore slijedi putem servisa za upravljanje API-JA.

## <a name="response-composition"></a>Sastavljanje odgovor
Na `send-request` pravilnika moguće je koristiti za poboljšavanje primarni zahtjev za pozadinski sustav, kao što smo vidjeli u prethodnom primjeru ili može se koristiti kao dovršeno Zamijeni za pozadinskog poziv. Pomoću ove tehnike možemo jednostavno stvoriti složeni resursa koji se pridružuje iz više različitih sustava.

### <a name="building-a-dashboard"></a>Stvaranje nadzorne ploče   
Ponekad želite imati mogućnost da bi se prikazale informacije koje postoji u više pozadinskog sustava, na primjer, na pogon na nadzornoj ploči. KPI-ja potjecati iz svih različite natrag do završetka, ali radije ne možete unijeti izravan pristup ih i bi bolje ako je moguće dohvatiti sve podatke u zahtjevu za jedan. Možda neki podaci pozadinskog mora neke istjecanju i dicing i najprije malo sanitizing! Moći predmemoriju složeni resursa će biti korisno da biste smanjili opterećenje pozadinskog kao znate da korisnici imaju habit od hammering tipku F5 da biste vidjeli ako može promijeniti svoje underperforming mjernih podataka.    

### <a name="faking-the-resource"></a>Faking resursa
Prvi korak za stvaranje naš nadzorne ploče resursa je da biste konfigurirali nova operacija na portalu za upravljanje API programa publisher. Ta će se rezervirano mjesto operacija koristi za konfiguriranje naš sastavljanje pravila da biste sastavili naš dinamički resursa.

![Operacija nadzorne ploče](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Upućivanje zahtjeve
Jednom u `dashboard` stvoreno je postupak ne možemo možete konfigurirati pravila posebno za taj postupak. 

![Operacija nadzorne ploče](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

U prvi je korak da biste izdvojili parametre upit iz zahtjeva za dolazne pa ćemo ih možete proslijediti na našem pozadinskog. U ovom primjeru naš nadzorne ploče prikazuju podaci koji se temelji na neko vrijeme na stoga je na `fromDate` i `toDate` parametar. Možete koristiti u `set-variable` pravila za izdvajanje podataka iz zahtjev za URL-a.

    <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
    <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

Kada imamo taj podatak dajemo zahtjeva za sve pozadinskog sustava. Svaki zahtjev za konstrukata novi URL parametar podacima i poziva njegov odgovarajući poslužitelj i pohranjuje odgovor u kontekstu tjednog prikaza kalendara.

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

Te zahtjeve će se izvršiti u nizu koji nije idealna. U budućem izdanju smo će biti Upoznavanje novog pravilnika pod nazivom `wait` koji će se sve te zahtjeve za izvršavanje paralelno omogućiti.

### <a name="responding"></a>Odgovor

Sastavljanje složeni odgovor koristimo pravila [Povratna odgovor](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) . Na `set-body` element možete koristiti izraze za sastavljanje novi `JObject` s sve prikaze komponente ugrađen kao svojstva.

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>

Dovršavanje pravila izgleda na sljedeći način:

    <policies>
        <inbound>

      <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
      <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

        <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
          <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
          <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <return-response response-variable-name="existing response variable">
          <set-status code="200" reason="OK" />
          <set-header name="Content-Type" exists-action="override">
            <value>application/json</value>
          </set-header>
          <set-body>
            @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                          new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                          new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                          new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                          ).ToString())
          </set-body>
        </return-response>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
    </policies>

U konfiguraciji rezerviranog mjesta operacija smo možete konfigurirati resursa nadzorne ploče predmemoriju za barem jedan sat jer uviđamo prirode podataka znači čak i ako je jedan sat ažuran, i dalje će biti potpuno učinkovitih da biste prenijeli vrijedne informacije korisnicima.

## <a name="summary"></a>Sažetak
Azure servisa za upravljanje API nudi fleksibilne pravila koja se selektivno primjenjuju na HTTP promet i omogućuje sastavljanje pozadinskog servisa. Želite li da biste poboljšali pristupnikom API-JA s upozorenjem funkcije, potvrdu, a zatim mogućnosti provjere valjanosti ili stvoriti nove složeni resurse koji se temelji na većem broju servisa pozadinskog sustava `send-request` i povezane pravila otvorite svijeta s mogućnostima.

## <a name="watch-a-video-overview-of-these-policies"></a>Pogledajte videozapis pregled tih pravila
Dodatne informacije o [– jedan-način-zahtjev za slanje](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [zahtjev za slanje](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)i [Povratna odgovor](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) pravila u ovom se članku, pogledajte sljedeći videozapis.

> [AZURE.VIDEO send-request-and-return-response-policies]
