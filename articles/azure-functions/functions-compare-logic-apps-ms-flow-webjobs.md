<properties
    pageTitle="Odabir između tijek, logike aplikacije, funkcija i WebJobs | Microsoft Azure"
    description="Usporedba i kontrast u oblak integracije usluge tvrtke Microsoft i odlučiti koje servise koristite."
    services="functions,app-service\logic"
    documentationCenter="na"
    authors="cephalin"
    manager="wpickett"
    tags=""
    keywords="Microsoft tijek, tijek, logike aplikacije, azure funkcije funkcije, azure webjobs, webjobs, događaj obrada dinamički računalnim serverless arhitekture"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="chrande; glenga"/>

# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Odabir između tijek, logike aplikacije, funkcija i WebJobs

U ovom se članku uspoređuju i contrasts sljedeći servisi u Microsoft cloud koji mogu sve riješiti probleme integracije i automatizacija poslovnih procesa:

- [Microsoft tijek](https://flow.microsoft.com/)
- [Azure logike aplikacije](https://azure.microsoft.com/services/logic-apps/)
- [Azure funkcije](https://azure.microsoft.com/services/functions/)
- [WebJobs aplikacije servisa za Azure](../app-service-web/web-sites-create-web-jobs.md)

"Lijepljenje" zajedno raznovrsne sustavi su korisne tih servisa. Oni mogu definiraju unos, Akcije, uvjeti i izlaz. Svaki od njih možete pokrenuti na raspored ili okidača. Međutim, svaki servis dodaje Jedinstveni skup vrijednosti i Usporedba ih ne pitanje "koji servis je najbolje?" ali neka od "usluge najbolje prikladan je za to vam?" Često kombinaciju od tih servisa je najbolji način za brzo stvaranje rješenje prilagodljivi, Potpuna Integracija istaknuto.

<a name="flow"></a>
## <a name="flow-vs-logic-apps"></a>Tijek nasuprot logike aplikacije

Ne možemo mogu razgovarati o Microsoft Flow i Azure logike aplikacije zajedno jer su oba *konfiguracije prvog* servisima za integraciju, koji olakšava stvaranje procesa i tijekova rada i integracija s različitim SaaS i enterprise aplikacije. 

- Tijek ugrađena je pri vrhu logike aplikacije
- Imaju isti brzu analizu
- [Poveznika](../connectors/apis-list.md) koji rade u jednom može raditi u drugi

Tokova omogućuje jednostavniju sve tempiranja office da biste izvršili jednostavne integracije (npr. pribaviti SMS važnu e-poštu) ne prolaze kroz razvojnim inženjerima ili IT. Logika aplikacije s druge strane, možete omogućiti napredne ili zaštita njihove privatnosti ovise ključnih integracije (npr. B2B procesa) koje su potrebni razini tvrtke DevOps i sigurnost prakse. Karakteristično za tijek rada za tvrtke radi povećanja u složenosti prekovremeni je. Sukladno tome, možete započeti s tijek isprva, a zatim ga pretvorite logike aplikacije po potrebi.

U sljedećoj su tablici pomaže vam odrediti je li tijek ili logike aplikacije najbolje za dani integraciju.

|               | Tijek                                                                             | Logika aplikacije                                                                                          |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Ciljne skupine      | Zaposlenici zaduženi za Office, poslovnih korisnika                                                   | INFORMATIČKI profesionalci razvojni inženjeri                                                                                 |
| Scenariji     | Samostalne                                                                     | Zaštita njihove privatnosti ovise ključnih                                                                                    |
| Alat za dizajniranje   | U preglednika samo korisničkog Sučelja                                                              | Web-mjesto u pregledniku i u okvir za [Visual Studio](../app-service/logic/app-service-logic-deploy-from-vs.md), omogućite [Prikaz koda](../app-service-logic/app-service-logic-author-definitions.md) |
| DevOps        | Ad-hoc razviti u proizvodnje                                                    | kontrola izvora, testiranje, podršku i Automatizacija i mogućnost upravljanja u [Upravljanje resursima za Azure](../app-service-logic/app-service-logic-arm-provision.md)|
| Sučelje za administratore| [https://Flow.microsoft.com](https://flow.microsoft.com)                       | [https://portal.Azure.com](https://portal.azure.com)                                                |
| Sigurnost      | Standardni postupci: [samostalnosti podataka](https://wikipedia.org/wiki/Technological_Sovereignty), [šifriranje na ostale](https://wikipedia.org/wiki/Data_at_rest#Encryption) za povjerljive podatke, itd. | Sigurnost jamstvo od Azure: [Azure sigurnost](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Centar za sigurnost](https://azure.microsoft.com/services/security-center/), [zapisnike nadzora](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)i više. |

<a name="function"></a>
## <a name="functions-vs-webjobs"></a>Funkcija nasuprot WebJobs

Ne možemo možete raspravljati funkcije Azure i Azure aplikacije servisa WebJobs zajedno jer su oba servisima za integraciju *kod prvog* i namijenjen razvojnim inženjerima. Oni omogućuju vam da pokreću skripte ili dio kod kao odgovor na različite događaje, kao što je [Novo za pohranu blob-ova](functions-bindings-storage.md) ili [WebHook zahtjev](functions-bindings-http-webhook.md). Evo njihove sličnosti: 

- Obje se temelji na [Aplikacije servisa za Azure](../app-service/app-service-value-prop-what-is.md) i Uživajte značajke kao što su [kontrole izvora](../app-service-web/app-service-continuous-deployment.md), [provjeru autentičnosti](../app-service/app-service-authentication-overview.md)i [nadzor](../app-service-web/web-sites-monitor.md).
- Obje su za razvojne inženjere odnosila services.
- Oba podržavaju standardnu skriptiranje i programskog jezika.
- Imat NuGet i NPM podržava.

Funkcije je prirodnim proizvod od WebJobs po tome što je potrebno najbolje stvari o WebJobs i poboljšava nakon ih. Poboljšanja obuhvaćaju sljedeće: 

- Pojednostavnjeno razvojni ispitivanje i pokretanje koda izravno u pregledniku.
- Ugrađeni Integracija s više Azure i 3 proizvođača services kao što je [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
- Plaćanje – po korištenju, ne morate platiti za [Aplikacije servisa za planiranje](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
- Automatsko, [dinamični skaliranja](functions-scale.md).
- Za postojeće korisnike aplikacije servisa za planiranje i dalje moguće (da biste iskoristili u odjeljku-koristi resursa) pokrenut servis za aplikacije.
- Integracija s logike aplikacije.

U sljedećoj tablici nalaze se razlike između funkcija i WebJobs:

|                        | Funkcija                                                                                                                                                                | WebJobs                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Promjena veličine                | Configurationless skaliranja                                                                                                                                                | Promjena veličine uz aplikacije servisa za planiranje        |
| Cijene                | Plaćanje po korištenju ili dio aplikacije servisa za planiranje                                                                                                                                  | Dio aplikacije servisa za planiranje           |
| Pokreni vrsta               | pokrene, zakazao (okidača mjerača vremena)                                                                                                                                  | okidačima, neprekinuti, zakazano   |
| Pokretanje događaja         | [mjerača vremena](functions-bindings-timer.md), [Azure DocumentDB](functions-bindings-documentdb.md), [Koncentratorima Azure događaj](functions-bindings-event-hubs), [HTTP/WebHook (GitHub, prazan hod)](functions-bindings-http-webhook.md), [Azure servisa mobilna aplikacija](functions-bindings-mobile-apps.md), [Koncentratorima Azure obavijesti](functions-bindings-notification-hubs.md), [Bus servisa Azure](functions-bindings-service-bus.md), [Azure prostora za pohranu](articles/functions-bindings-storage.md) | [Azure prostora za pohranu](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Azure Service Bus](websites-dotnet-webjobs-sdk-service-bus.md)         |
| Razvoj u pregledniku | x                                                                                                                                                                        |                                    |
| Prozor skriptiranje       | eksperimentalne                                                                                                                                                             | x                                  |
| PowerShell             | eksperimentalne                                                                                                                                                             | x                                  |
| C#                     | x                                                                                                                                                                        | x                                  |
| F#                     | x                                                                                                                                                                        |                                    |
| Tulum                   | eksperimentalne                                                                                                                                                             | x                                  |
| PHP                    | eksperimentalne                                                                                                                                                             | x                                  |
| Python                 | eksperimentalne                                                                                                                                                             | x                                  |
| JavaScript             | x                                                                                                                                                                        | x                                  |
| Java                   | eksperimentalne                                                                                                                                                             | x                                  |

Želite li koristiti funkcije ili WebJobs konačni ovisi o što već radite s uslugom aplikaciju. Ako imate aplikaciju aplikacije servisa za koji želite pokrenuti koda, a želite upravljati ih zajedno u istom okruženju DevOps, trebali biste koristiti WebJobs. Ako želite pokrenuti koda za ostale Azure servise ili aplikacije čak i 3 proizvođača ili ako želite upravljati vaše Integracija koda zasebno iz aplikacije servisa za aplikacije ili ako želite pozvati vaše koda iz logike aplikacije, trebali biste iskoristite prednost sve poboljšanja u funkcijama.  

<a name="together"></a>
## <a name="flow-logic-apps-and-functions-together"></a>Tijek, logike aplikacije i funkcije zajedno

Spomenuti prethodno, koji servis je najprikladnije vama ovisi o vašim okolnostima. 

- Za optimizaciju jednostavne tvrtke, koristiti tijek.
- Ako integracija scenariju previše napredne za tijek ili trebate DevOps mogućnosti i compliances sigurnost, koristite logike aplikacije.
- Ako korak u scenariju Integracija zahtijeva iznimno pretvorbe ili specijalizirane kod, pisanje u aplikaciji (funkcija), a zatim pokretanje funkcije kao akcije u aplikaciji logike.

Logika aplikacije uputili poziv u tijeku. U funkciji možete nazvati i funkcija u logike aplikacije i logike aplikacije. Integracija između tijek, logike aplikacije i funkcija, i dalje da biste poboljšali prekovremeni. Možete izraditi nešto u jedan servis i koristiti u drugim servisima. Stoga je bilo koji ulaganje koje ste napravili u te tri tehnologije vrijedno.

## <a name="next-steps"></a>Daljnji koraci

Uvod u svim servisima stvaranjem vaš prvi tijek, logike aplikacije, funkcija aplikacije ili WebJob. Kliknite bilo koju od sljedećih veza:

- [Uvod u Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
- [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Stvaranje prve Azure (funkcija)](../azure-functions/functions-create-first-azure-function.md)
- [Implementacija WebJobs pomoću Visual Studio](../app-service-web/websites-dotnet-deploy-webjobs.md)

Ili se informirajte o te servisima za integraciju sa sljedećih veza:

- [Korištenje funkcije Azure i aplikacije servisa za Azure scenarija integracije po Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
- [Integracije načinio jednostavne Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
- [Logika aplikacije Live web-prijenos](http://aka.ms/logicappslive)
- [Microsoft tijeka često postavljana pitanja](https://flow.microsoft.com/documentation/frequently-asked-questions/)
- [Azure WebJobs dokumentaciju resursi](../app-service-web/websites-webjobs-resources.md)
