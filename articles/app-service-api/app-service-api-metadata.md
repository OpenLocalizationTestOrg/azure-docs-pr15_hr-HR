<properties
    pageTitle="Aplikacija aplikacije API servisa metapodataka za otkrivanje i kod generacije API | Microsoft Azure"
    description="Saznajte kako API aplikacije u aplikacije servisa za Azure pomoću metapodataka Swagger da biste olakšali otkrivanje i kod generacije API-JA."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>Aplikacija aplikacije API servisa metapodataka za otkrivanje i kod generacije API-JA 

Podrška za [Swagger 2.0](http://swagger.io/) API metapodataka ugrađen u API servisa aplikacija. Ne morate koristiti Swagger, no ako ga koristiti, možete iskoristiti API aplikacije značajki koje olakšavaju otkrivanje i potrošnje.   

## <a name="swagger-endpoint"></a>Swagger krajnje točke

Možete navesti krajnje koja omogućuje Swagger 2.0 JSON metapodataka za aplikaciju API-JA u svojstvu aplikaciju API-JA. Krajnja točka može biti odnosu osnovni URL aplikaciju API-JA ili apsolutni URL. Apsolutni URL-ovi postavite pokazivač miša izvan aplikaciju API-JA. 

Mnoge do klijenti (na primjer, generiranje koda za Visual Studio i PowerApps "Dodaj API-JA" toka), URL mora biti javno dostupnu (nije zaštićena korisnik ili usluge provjere autentičnosti). To znači da ako koristite aplikacije servisa za provjeru autentičnosti, a želite izložiti definicija API iz same aplikacije, morate koristiti mogućnost obavezne provjere autentičnosti koja omogućuje anonimni promet dosegne vaše API-JA. Dodatne informacije potražite u članku [provjere autentičnosti i autorizacije za API servisa aplikacija](app-service-api-authentication.md).

### <a name="portal-blade"></a>Plohu portala

[Azure portal](https://portal.azure.com/) URL krajnje točke možete može vidjeti i promijenjena na plohu **Definicija API -JA** .

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Svojstvo Azure Voditelj resursa

API definicija URL-a za aplikaciju API-JA možete konfigurirati i pomoću [Explorer resursa](https://resources.azure.com/) ili [Voditelj resursa Azure predložaka](../resource-group-authoring-templates.md) u alatima za naredbenog retka kao što su [Azure PowerShell](../powershell-install-configure.md) i [Azure EŽA](../xplat-cli-install.md). 

U **Programu Explorer resursa**, idite na **pretplate > {pretplate} > resourceGroups > {grupu resursa} > davatelji > Microsoft.Web > web-mjesta > {web-mjestu} > config > web**, i prikazat će se na `apiDefinition` svojstvo:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Primjer predloška Azure Voditelj resursa koji se postavlja u `apiDefinition` svojstvo, otvorite [azuredeploy.json datoteke u aplikaciji ogledni popis zadataka](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Pronađite odjeljak predloška koji izgleda kao primjer JSON prikazan gore.

### <a name="default-value"></a>Zadana vrijednost

Kada koristite Visual Studio stvaranje aplikacije API-JA, krajnje definicija API automatski postavlja na osnovni URL aplikaciju API plus `/swagger/docs/v1`. To je zadani URL koji paket [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet koristi da bi služio dinamički generirani Swagger metapodataka u projektu API servisa platforme ASP.NET Web. 

## <a name="code-generation"></a>Generiranje koda

Jedna od prednosti integracije Swagger u Azure API aplikacije je kod za automatsko generiranje. Klase generira klijent olakšavaju napišite kod koji se poziva aplikaciju API-JA.

Kod klijenta za aplikaciju API-JA možete stvoriti pomoću programa Visual Studio ili iz naredbenog retka. Informacije o tome da biste generirali klase klijenta u Visual Studio u projektu API servisa platforme ASP.NET Web potražite u članku [Početak rada s aplikacijama API -JA i ASP.NET](app-service-api-dotnet-get-started.md#codegen). Informacije o kako to učiniti iz naredbenog retka za sve podržane jezike potražite u članku datoteke readme u spremištu [Azure/autorest](https://github.com/azure/autorest) na GitHub.com.
 
## <a name="next-steps"></a>Daljnji koraci

Korak po korak vodič koji će vas voditi kroz stvaranje, uvođenje i potrošnje aplikaciju API potražite u članku [Početak rada s aplikacijama API -JA u aplikacije servisa za Azure](app-service-api-dotnet-get-started.md).

Ako koristite upravljanje API Azure s aplikacijama API-JA, možete koristiti Swagger metapodataka za uvoz sustava API-JA u API upravljanje. Dodatne informacije potražite [u](../api-management/api-management-howto-import-api.md)članku Uvoz definicije API-JA s operacije u upravljanju Azure API -JA. 
