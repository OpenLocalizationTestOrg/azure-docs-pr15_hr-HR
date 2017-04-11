<properties
    pageTitle="Prilagođeni predmemoriranja u upravljanju API Azure"
    description="Saznajte kako predmemoriju stavki tipke u upravljanju API Azure"
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

# <a name="custom-caching-in-azure-api-management"></a>Prilagođeni predmemoriranja u upravljanju API Azure
Azure servisa za upravljanje API ima ugrađenu podršku za [HTTP odgovor predmemoriranje](api-management-howto-cache.md) pomoću URL resursa kao ključ. Tipku možete mijenjati pomoću zaglavlja zahtjeva za `vary-by` svojstva. To je korisno za predmemoriranje cijelu HTTP odgovora (ili prikaze), ali ponekad je korisno u predmemoriju samo dio prikaz. Nova pravila [predmemorije, pretraživanje i vrijednosti](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) i [predmemorije trgovine vrijednosti](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) navedite omogućuje pohranu i dohvaćanje proizvoljne podatka iz unutar definicijama pravila. Ova mogućnost također dodaje vrijednost pravila prethodno uvedeni [zahtjev za slanje](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) jer je sada možete predmemoriju odgovore od vanjskih servisa.

## <a name="architecture"></a>Arhitektura  
Usluga upravljanja API koristi predmemorija zajedničke klijentu tako da kao do skaliranje više jedinica i dalje ćete dobiti pristup iste predmemorirane podatke. Međutim, prilikom rada s više područja implementacije ne postoje nezavisne predmemorije u svakoj od tih regija. Zbog toga, važno je da ne obradi predmemoriju kao izvor podataka, pri čemu je izvor samo neke informaciju. Ako niste, a kasnije odlučili da iskoristite prednost implementacije regije, zatim Kupci s korisnicima koji putovanja mogu izgubiti pristup da biste te podatke.

## <a name="fragment-caching"></a>Fragment predmemoriranja
Postoje određenim slučajevima u kojima se odgovori se vraćaju sadrže neki dio podataka koji je da biste odredili, a još ostaje svježim pametnije vremenskog razdoblja. Na primjer, razmotrite usluge komponenti po Zrakoplovna tvrtka kojoj se navode informacije koje se odnose Rezervacija leta, status leta itd. Ako je korisnik član odabrani program točke, to bi imaju podatke koji se odnose na njihove trenutno stanje i potrošnje akumuliran. Ove informacije odnose na korisnike mogu nalaziti u neki drugi sustav, no možda je poželjno će se uvrstiti u odgovore vraća o statusu leta i rezervacije. To možete učiniti pomoću postupka pod nazivom fragment predmemoriranje. Primarni prikaz može vratiti s poslužitelja polazište pomoću neke vrste token koji određuju gdje se informacije odnose na korisnike je želite umetnuti. 

Razmotrite sljedeće JSON odgovor pozadinskog API-JA.


    {
      "airline" : "Air Canada",
      "flightno" : "871",
      "status" : "ontime",
      "gate" : "B40",
      "terminal" : "2A",
      "userprofile" : "$userprofile$"
    }  

I sekundarne resursa na `/userprofile/{userid}` da izgleda kao,

     { "username" : "Bob Smith", "Status" : "Gold" }

Da biste odredili odgovarajuće korisničke informacije da biste uključili, moramo prepoznati tko je krajnjeg korisnika. U ovom mehanizam je implementacija zavisne. Na primjer, koristim na `Subject` zahtjeva od na `JWT` tokena. 

    <set-variable
      name="enduserid"
      value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

Ne možemo to pohranu `enduserid` vrijednost u kontekstu varijabla za kasnije korištenje. Sljedeći je korak da biste odredili ako zahtjev za prethodni već dohvatiti podatke o korisniku i pohranjenih u predmemoriji. Za to koristimo na `cache-lookup-value` pravila.

      <cache-lookup-value
      key="@("userprofile-" + context.Variables["enduserid"])"
      variable-name="userprofile" />

Ako vam se nijedna stavka u predmemoriji koje odgovara vrijednost ključa, a zatim bez `userprofile` stvorit će se varijabla kontekst. Provjerava uspjeh pomoću pretraživanja na `choose` kontrolirati tijek pravila.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("userprofile"))">
            <!— If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
        </when>
    </choose>


Ako u `userprofile` varijabla kontekstu ne postoji, a zatim ćemo ćete morati HTTP zahtjev preuzeti.

    <send-request
      mode="new"
      response-variable-name="userprofileresponse"
      timeout="10"
      ignore-error="true">

      <!-- Build a URL that points to the profile for the current end-user -->
      <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
          (string)context.Variables["enduserid"]).AbsoluteUri)
      </set-url>
      <set-method>GET</set-method>
    </send-request>

Koristimo na `enduserid` za sastavljanje URL resursu korisničkog profila. Kada imamo odgovor, bismo istaknuti tijela teksta iz odgovor i spremite ga u kontekstu tjednog prikaza kalendara.

    <set-variable
        name="userprofile"
        value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

Da biste izbjegli nam potrebe za ponovno provjerite HTTP zahtjev kad isti korisnik unese drugi zahtjev, ne možemo možete spremiti korisničkog profila u predmemoriji.

    <cache-store-value
        key="@("userprofile-" + context.Variables["enduserid"])"
        value="@((string)context.Variables["userprofile"])" duration="100000" />

Ne možemo spremiti vrijednost u predmemoriju pomoću točno istom ključ koji smo izvorno pokušaj preuzeti s. Trajanje koje ćemo odlučite spremiti vrijednost mora biti ovisno o tome kako često promjene podataka o i kako pogreške korisnici su za zastarjeli informacije. 

Važno je da shvatite da dohvaćanje iz predmemorije i dalje-postupak, zahtjev za mreže i potencijalno i dalje možete dodavati tens milisekundama zahtjev. Prednosti potjecati prilikom određivanja podatke korisničkog profila traje znatno dulje od onog koji je zbog koje je potrebno u bazu podataka upita ili zbrajanja podatke iz više natrag završava.

U posljednjem koraku postupka je da biste ažurirali vraćene odgovor s oglednim podatke korisničkog profila.

    <!—Update response body with user profile-->
    <find-and-replace
        from='"$userprofile$"'
        to="@((string)context.Variables["userprofile"])" />

Sam odabrao kao dio token uključiti navodnim znakovima, čak i ako se ne pojavljuje okvir Zamijeni, odgovor je još uvijek valjan JSON. To je bio prvenstveno da bi vam pogrešaka.

Kada zajedno spojiti ove korake, rezultat je pravila koja izgleda kao jedna.

     <policies>
        <inbound>
            <!-- How you determine user identity is application dependent -->
            <set-variable
              name="enduserid"
              value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

            <!--Look for userprofile for this user in the cache -->
            <cache-lookup-value
              key="@("userprofile-" + context.Variables["enduserid"])"
              variable-name="userprofile" />

            <!-- If we don’t find it in the cache, make a request for it and store it -->
            <choose>
                <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                    <!—Make HTTP request to get user profile -->
                    <send-request
                      mode="new"
                      response-variable-name="userprofileresponse"
                      timeout="10"
                      ignore-error="true">

                       <!-- Build a URL that points to the profile for the current end-user -->
                        <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                        <set-method>GET</set-method>
                    </send-request>

                    <!—Store response body in context variable -->
                    <set-variable
                      name="userprofile"
                      value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                    <!—Store result in cache -->
                    <cache-store-value
                      key="@("userprofile-" + context.Variables["enduserid"])"
                      value="@((string)context.Variables["userprofile"])"
                      duration="100000" />
                </when>
            </choose>
            <base />
        </inbound>
        <outbound>
            <!—Update response body with user profile-->
            <find-and-replace
                  from='"$userprofile$"'
                  to="@((string)context.Variables["userprofile"])" />
            <base />
        </outbound>
     </policies>

Takvog predmemoriranja prvenstveno se koristi u web-mjesta gdje HTML sastoji se na strani poslužitelja tako da se može prikazati na jednoj stranici. Međutim, može biti korisno u API-ji gdje klijente ne klijent bočnih HTTP predmemoriranje ili je poželjno ne da biste postavili te odgovornosti na klijentskom računalu.

Ovaj istu vrstu fragment predmemoriranje mogu izvršiti i na web-poslužiteljima pozadinski pomoću Redis predmemoriranje poslužitelj, no pomoću servisa za upravljanje za API da biste izvršili taj rad koristan je kada predmemorirani fragmentirane su koji dolaze iz različitih natrag-završava od primarni odgovore.

## <a name="transparent-versioning"></a>Prozirni rad s verzijama
To je uobičajeno za više verzija drugoj implementaciji API planirate podržavati u bilo kojem trenutku. Time se možda podržava različitim okruženjima, kao što je razvojni test, radni, Dr, ili se pak za podršku starijih verzija API da bi se dobilo vrijeme koje korisnici API-JA za migraciju u novijim verzijama. 

Jedan pristup zadužen za to umjesto potrebno klijenta inženjerima omogućuje promjenu URL-ovi s `/v1/customers` da biste `/v2/customers` želite li spremiti u korisničke profile podataka koja je verzija programa API trenutno želite koristiti i poziva odgovarajuće pozadinskog URL-a. Da bi se određivanje URL-a ispravan pozadinskog da biste uputili poziv za određeni klijent, nije potrebno upit neke podatke konfiguracije. Po predmemoriranje ove konfiguracije podatke, ne možemo rješenje smanjite kazna performanse učinite ovog pretraživanja.

U prvi je korak da biste odredili identifikator koji se koristi za konfiguriranje željenoj verziji. U ovom primjeru koju sam odabrao povezivati verzije s ključem proizvoda pretplate. 

        <set-variable name="clientid" value="@(context.Subscription.Key)" />

Zatim ćemo učinite predmemoriju pretraživanja da biste vidjeli ako smo već ste dohvatili verzija željeno klijenta.

        <cache-lookup-value
        key="@("clientversion-" + context.Variables["clientid"])"
        variable-name="clientversion" />

Zatim ćemo provjerite jesu li Nismo pronašli ga u predmemoriji.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">

Ako ne možemo niste, a zatim ćemo otvorite i preuzeti.

    <send-request
        mode="new"
        response-variable-name="clientconfiguresponse"
        timeout="10"
        ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
    </send-request>

Izdvajanje odgovor tijela teksta iz odgovor.

    <set-variable
          name="clientversion"
          value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />

Pohranite natrag u predmemoriji za buduću upotrebu.

    <cache-store-value
          key="@("clientversion-" + context.Variables["clientid"])"
          value="@((string)context.Variables["clientversion"])"
          duration="100000" />

I na kraju ažuriranje pozadinske URL-a da biste odabrali verziju servisa tvrtka klijent želi.

    <set-backend-service
          base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />

Pravila u potpunosti je na sljedeći način.

     <inbound>
        <base />
        <set-variable name="clientid" value="@(context.Subscription.Key)" />
        <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("clientversion"))">
                <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                    <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>
                <!-- Store response body in context variable -->
                <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
                <!-- Store result in cache -->
                <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
            </when>
        </choose>
        <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
    </inbound>


Omogućivanje koje korisnici API-JA za prozirna kontrolu koja je verzija pozadinskog je pristupa klijenti bez potrebe za ažurirajte i ponovno implementirate klijenti je Elegantna rješenje koje mnogo opasnosti API određivanja verzija.

## <a name="tenant-isolation"></a>Odvajanja klijenta

Neke tvrtke u veće, više klijentu implementacijama stvoriti posebne grupe klijenata distinct implementacijama pozadinskog hardvera. Time se smanjuje broj korisnika koji su utječe hardverskog problema s pozadinskom sustavu. Omogućuje i nove verzije softver za biti vraćeni u fazama. Najbolje tu arhitekturu pozadinskog mora biti prozirni njezinoj API-JA. To se može postići na isti način u prozirne određivanje verzije jer se temelji na istu tehniku od rukovanje pozadinski URL-a pomoću konfiguracije stanja po ključ za API-JA.  

Umjesto vraćanja Preferirani verziju API-JA za svaku pretplatu ključ bi vratilo identifikatora koji se odnose na klijentu grupi dodijeljene hardvera. Identifikator može se koristiti za sastavljanje odgovarajuće pozadinskog URL-a.

## <a name="summary"></a>Sažetak
Slobode da biste koristili Azure API upravljanja predmemorije za pohranu bilo koju vrstu podataka omogućuje učinkovito pristup podacima konfiguracije koji mogu utjecati na način obrade zahtjeva za razmjenu ulaznog. Može se koristiti i za pohranu fragmentirane podataka koje možete proširiti odgovore, vratio pozadinskog API-JA.

## <a name="next-steps"></a>Daljnji koraci
Pošaljite nam povratne informacije u niti Disqus ove teme ako su drugi scenarija koji se ta pravila omogućeno za vas ili ako postoje scenariji kojima želite da biste postigli, ali ne ne čini vam trenutno moguće.
