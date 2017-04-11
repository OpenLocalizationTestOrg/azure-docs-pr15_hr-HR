<properties
    pageTitle="Referenca za razvojne inženjere Azure funkcije | Microsoft Azure"
    description="Razumijevanje funkcije Azure koncepte i komponente koje su zajedničke za sve jezike i povezivanja."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcije, obrada događaj, webhooks, dinamični računalnim, funkcije serverless arhitekture"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-developer-reference"></a>Azure funkcije reference za razvojne inženjere

Azure funkcije zajedničko korištenje nekoliko osnovni koncepti tehničke i komponente, bez obzira na jezik ili povezivanje koji koristite. Prije prelaska u učenje pojedinosti specifične za zadani jezik ili povezivanje, svakako pročitajte ovaj pregled koji se primjenjuje na sve.

U ovom se članku pretpostavlja da ste već pročitali [Pregled funkcija Azure](functions-overview.md) i upoznati s [WebJobs SDK koncepte kao što su okidačima, povezivanja, i izvođenja JobHost](../app-service-web/websites-dotnet-webjobs-sdk.md). Azure funkcije se temelji na WebJobs SDK. 


## <a name="function-code"></a>Kod (funkcija)

*Funkcija* je primarni pojam u funkcijama Azure. Pisanje koda za funkciju na jeziku po izboru i spremanje datoteke kod i konfiguracijska datoteka u istoj mapi. Konfiguriranje se JSON i naziv datoteke `function.json`. Podržani su na različitim jezicima, a svaki od njih ima malo drugačiju sučelje optimiziran kako najbolje funkcioniraju za taj jezik. 

Na `function.json` datoteka sadrži konfiguracije specifične za funkciju, uključujući njegove povezivanja. Za vrijeme izvođenja čita datoteku da biste utvrdili koji događaja koji se aktiviraju isključivanje programa, podatke koje želite uvrstiti pri pozivanju funkcije i kamo poslati podatke prenosi duž s sama funkcija. 

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Možete spriječiti pokretanje funkciju postavljanjem vremena izvođenja u `disabled` svojstvo `true`.

Na `bindings` svojstvo je kojima konfigurirati okidača i povezivanja. Svaki povezivanje dijeli nekoliko uobičajenih postavke i neke postavke koje se odnose na određene vrste povezivanja. Svaki povezivanje Traži sljedeće postavke:

|Svojstvo|Vrijednosti-vrste|Komentari|
|---|-----|------|
|`type`|niz|Vrsta povezivanja. Na primjer, `queueTrigger`.
|`direction`|"u", "Smanji"| Označava je li spajanje primanja podataka u funkciji ili slanje podataka iz funkcije.
| `name` | niz | Naziv koji će se koristiti za povezane podatke u funkciji. Za C# dogodit će se naziv argumenta; za JavaScript bit će ključa na popisu ključa vrijednosti.

## <a name="function-app"></a>Funkcija aplikacije

Funkcija aplikacije sastoji se od jednog ili više pojedinačnih funkcije koje se zajednički upravlja aplikacije servisa za Azure. Sve funkcije u aplikaciji funkcija zajednički koristiti istu tarifu cijene, neprekinuti implementacije i runtime verzija. Funkcija napisan na više jezika možete sve zajednički koristiti iste aplikacije (opis funkcije). Smatrati funkcija aplikacije način organiziranja i skupno upravljanje vaše funkcijama. 

## <a name="runtime-script-host-and-web-host"></a>Izvođenje (glavno računalo za skripte i glavno računalo web)

Izvođenje ili script host, je podlozi WebJobs SDK glavno računalo koje očekuje podatke za događaje, prikuplja i šalje podatke i konačni pokreće kod. 

Da biste olakšali HTTP okidača, postoji i glavno računalo web namijenjen sjesti ispred glavno računalo za skripte u scenarijima radnog. Pomaže izdvojiti voditelju skripte iz promet sučelje upravlja glavno računalo web.

## <a name="folder-structure"></a>Struktura mapa

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Kada je postavka gore projekta za implementaciju funkcije funkcija aplikaciju u aplikacije servisa za Azure, ovu mapu strukturu možete Smatraj kod web-mjesta. Možete koristiti postojeće alate kao što su neprekinuti integracije i implementaciju ili implementaciju prilagođene skripte za taj način implementacije instalacijski paket vremena ili kod transpilation.

>[AZURE.NOTE] Na `wwwroot` mapa ovdje je gdje će se datoteka implementirano u. Međutim, koji ne smije sadržavati tu mapu u implementaciju, datoteke koje želite sinkronizirati `wwwroot\wwwroot`. Umjesto toga, vaše `host.json` mape datoteke i funkcija mora biti izravno u korijenu što implementacije.

## <a id="fileupdate"></a>Ažuriranje datoteka aplikacije (funkcija)

Funkcija uređivač ugrađenu Azure portal omogućuje ažuriranje *function.json* datoteke i datoteke Šifra za funkciju. Prijenos ili ažuriranje druge datoteke kao što su *package.json* ili *project.json* ili ovisnosti, morate koristiti druge metode implementacije.

Funkcija aplikacije su programirane servis za aplikaciju, pa su dostupne za aplikacije funkcija kao i sve [Mogućnosti implementacije dostupne na standardni web-aplikacije](../app-service-web/web-sites-deploy.md) . Ovo su neki postupci možete koristiti za prijenos i ažuriranje datoteka aplikacije (opis funkcije). 

#### <a name="to-use-app-service-editor"></a>Da biste koristili aplikaciju servisa uređivača

1. Na portalu za Azure funkcije kliknite **postavki aplikacije (opis funkcije)**.

2. U odjeljku **Dodatne postavke** kliknite **Idi na postavke servisa za aplikaciju**.

3. Kliknite **Aplikaciju servisa Editor** u navigacijskom oknu izbornik aplikacije u odjeljku **Alati za RAZVOJ**.

4.  Kliknite **Idi**.

    Kada aplikacije servisa uređivač učitava, vidjet ćete *host.json* datoteka i funkcija mape u odjeljku *wwwroot*. 

5. Otvaranje datoteka za njihovo uređivanje ili povucite i ispustite s vašeg računala razvoj da biste prenijeli datoteke.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>Da biste koristili aplikaciju funkcija IO (Kudu) krajnje

1. Dođite do: `https://<function_app_name>.scm.azurewebsites.net`.

2. Kliknite **konzole za ispravljanje pogrešaka > CMD**.

3. Dođite do `D:\home\site\wwwroot\` da biste ažurirali *host.json* ili `D:\home\site\wwwroot\<function_name>` ažuriranje datoteka u funkciji.

4. Povlačenje i ispuštanje datoteka koju želite prenijeti u odgovarajuću mapu u rešetki datoteke. Postoje dva područja u rešetki datoteka koju možete ispustite datoteku. Za datoteke *.zip* u pojavit će se okvir s oznakom "Povucite ovdje da biste prenijeli i raspakiraj." Za ostale vrste datoteka ispustite u rešetki datoteka, ali izvan okvira "raspakirajte".

#### <a name="to-use-ftp"></a>Da biste koristili FTP

1. Slijedite upute [u nastavku](../app-service-web/web-sites-deploy.md#ftp) da biste dobili FTP konfiguriran.

2. Kada ste povezani s web-mjesto aplikacija (funkcija), kopirajte datoteku ažurirane *host.json* za `/site/wwwroot` ili kopiranje funkcija datoteka na `/site/wwwroot/<function_name>`.

#### <a name="to-use-continuous-deployment"></a>Da biste koristili neprekinuti implementacije

Slijedite upute u temi [Neprekinuti implementacije za Azure funkcije](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Paralelni izvođenja

Prilikom više pokretački događaja brže nego što ih možete obraditi za vrijeme izvođenja funkcija jednom niti, vremena izvođenja možda pozivanje funkcija više puta paralelno.  Ako funkcija aplikacije koristi [Dinamički planiranje usluga](functions-scale.md#dynamic-service-plan), funkcija aplikacije nije skaliranje izvan automatski.  Svaku instancu aplikacije (funkcija), hoće li se aplikacija pokreće se dinamički planiranje usluga ili u pravilnim [Plan za aplikaciju servisa](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), možda procesa Istodobni funkcija instanci paralelno pomoću više niti.  Maksimalni broj instanci Istodobni funkcija u svaku instancu aplikacije funkcija mijenja se ovisno o veličini memorije aplikaciju (opis funkcije). 

## <a name="azure-functions-pulse"></a>Servisom Azure funkcije Pulse  

Servisom pulse je strujanje uživo događaj koji pokazuje koliko često funkcija pokrene, kao i uspjeha i pogrešaka. Možete nadzirati i Prosječno vrijeme izvršavanja. Ne možemo ćete dodati dodatne značajke i prilagodbe na nju tijekom vremena. Stranici **servisom Pulse** možete pristupiti s kartice **praćenja** .

## <a name="repositories"></a>Spremišta

Kod za Azure funkcije se Otvori izvor i spremljene u GitHub spremištima:

* [Azure vrijeme izvođenja funkcija](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Portal za Azure funkcije](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure funkcije predložaka](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK proširenja](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Povezivanja

Ovo je popis svih podržanih povezivanja.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Prijavljivanje problema

[AZURE.INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)] 

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u sljedećim resursima:

* [Azure funkcije C# reference za razvojne inženjere](functions-reference-csharp.md)
* [Azure funkcije F # reference za razvojne inženjere](functions-reference-fsharp.md)
* [Azure NodeJS funkcije reference za razvojne inženjere](functions-reference-node.md)
* [Azure okidača funkcije i poveznici](functions-triggers-bindings.md)
* [Funkcije azure: U putovanja](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) na blogu tima aplikacije servisa za Azure. Povijest kako je razvio Azure funkcije.





