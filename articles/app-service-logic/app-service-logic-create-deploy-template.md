<properties
   pageTitle="Stvaranje predloška za implementaciju aplikacije logike | Microsoft Azure"
   description="Saznajte kako stvoriti predložak za implementaciju aplikacije logiku i koristiti za upravljanje izdanje"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="create-a-logic-app-deployment-template"></a>Stvaranje predloška za implementaciju aplikacije logike

Nakon stvaranja logike aplikacije možda ćete morati stvoriti kao predložak Azure Voditelj resursa. Na taj način možete jednostavno implementirati aplikaciju logike svim okruženje ili grupu resursa gdje bilo bi dobro. Uvod u predloške Voditelj resursa, obavezno potražite u članku člancima [omogućeno Voditelj resursa Azure](../resource-group-authoring-templates.md) i [Implementacija resursima pomoću predložaka Azure Voditelj resursa](../resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Predložak za implementaciju aplikacije logike

Logika aplikacije sastoji se od tri osnovne komponente:

* **Logika aplikacije resursa**. Ovaj resurs sadrži informacije o elemente kao što su cijene plan, mjesto i definiciju tijeka rada.
* **Definiciju tijeka rada**. Ovo je što je vidjeti u prikazu koda. Obuhvaća definiciju koraka tijeka i kako moraju izvršiti modul. Ovo je na `definition` svojstvo resursa logike aplikacije.
* **Veze**. To su zasebnom resursa koji sigurno pohranjuje metapodatke o poveznik veze, kao što su niza za povezivanje i token za pristup. Te u aplikaciji logike u upućuju na `parameters` dio aplikacije resursa logike.

Pomoću alata kao što je [Explorer Azure resursa](http://resources.azure.com)možete pregledati sve od ovih dijelova za postojeće logike aplikacije.

Da biste predložak za aplikaciju logike za uporabu implementacijama grupu resursa, morate definirati resurse i parameterize po potrebi. Ako, na primjer, ako ste implementacije za razvoj, testiranje i radnog okruženja, ćete vjerojatno želite koristiti nizovi drugu vezu s bazom podataka SQL u svakom okruženju. Ili želite uvesti u različite pretplate ili grupe resursa.  

## <a name="create-a-logic-app-deployment-template"></a>Stvaranje predloška za implementaciju aplikacije logike

Da bi se predložak za implementaciju aplikacije valjani logike najjednostavnije [Visual Studio Tools for logike aplikacije](./app-service-logic-deploy-from-vs.md).  Visual Studio tools generirati valjani implementacije predložak koji se mogu koristiti preko sve pretplate ili mjesto.

Nekih drugih alata može vam pomoći prilikom stvaranja predloška za implementaciju aplikacije logike. Vi ste možete autor ručno, odnosno pomoću resursa već ovdje spominju da biste stvorili parametara po potrebi. Da biste koristili modul ljuske PowerShell za [logike aplikacije predložak autor](https://github.com/jeffhollan/LogicAppTemplateCreator) je i mogućnost. Modul za Otvori izvor najprije vrednuje aplikaciju logiku i sve veze koje je koristi, a generira resursa predložak s potrebne parametara za implementaciju. Na primjer, ako imate aplikaciju za logiku koja prima poruke iz red čekanja za Bus servisa Azure te dodaje podatke s bazom podataka Azure SQL, alat će zadržati sve logike djelovanje i parameterize nizove veze SQL i usluge Bus tako da se postavljaju na implementaciju.

>[AZURE.NOTE] Veza mora biti unutar iste grupe resursa logike aplikacija.

### <a name="install-the-logic-app-template-powershell-module"></a>Instalirajte modul ljuske PowerShell za predložak logike aplikacije

Da biste instalirali modul najjednostavnije putem [Galerije PowerShell](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1)pomoću naredbe `Install-Module -Name LogicAppTemplate`.  

Također možete instalirati modul ljuske PowerShell ručno:

1. Preuzmite najnovije izdanje [logike aplikacije predložak autora](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
1. Izdvajanje mape u mapi modul ljuske PowerShell (obično `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Za modul za rad s web-mjesto klijenta i u okvir za pretplatu pristup tokena, preporučujemo da ih koristiti alatom [ARMClient](https://github.com/projectkudu/ARMClient) naredbenog retka.  U ovom [bloga](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) govori o ARMClient detaljnije.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Generiranje logike predloška aplikacije pomoću komponente PowerShell

Nakon instalacije PowerShell možete stvoriti predložak pomoću sljedeće naredbe:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`je ID Azure pretplate Redak najprije Nabavljanje tokena putem ARMClient, programa access, a zatim ih putem cijevi skriptu PowerShell i zatim stvoriti predložak u datoteci JSON.

## <a name="add-parameters-to-a-logic-app-template"></a>Dodajte parametre da logike predloška aplikacije

Kada stvorite predložak za aplikaciju logike, možete i dalje da biste dodali ili izmijenite parametre koje možda će biti potrebno. Ako, na primjer, ako definicije obuhvaća ID resursa Azure funkcija ili ugniježđene tijeka rada koje namjeravate implementacije u jednom implementaciju, možete dodati dodatni resursi predlošku i parameterize ID-a prema potrebi. Isto vrijedi za sve reference na prilagođeni API-ji ili Swagger krajnje točke očekujete da ćete implementirati uz svaku grupu resursa.

## <a name="deploy-a-logic-app-template"></a>Uvođenje predloška logike aplikacije

Predložak možete implementirati pomoću bilo koji broj Alati, uključujući PowerShell, REST API-JA, upravljanje izdanje za Visual Studio i uvođenje predloška Azure Portal. Pročitajte ovaj članak o [implementaciji resursima pomoću upravitelja resursa Azure predložaka](../resource-group-template-deploy.md) za dodatnim informacijama. Osim toga, preporučujemo da stvorite [parametar datoteke](../resource-group-template-deploy.md#parameter-file) za spremanje vrijednosti parametra.

### <a name="authorize-oauth-connections"></a>Autorizirajte OAuth veze

Nakon implementacije, aplikaciju logike radi završetka do kraja s valjani parametri. Međutim, OAuth veza i dalje morat ćete biti ovlašteni za generiranje token valjan pristup. To možete učiniti tako da otvorite aplikaciju logike u dizajneru i dopuštanja veze. Ili, ako želite da biste automatizirali, možete koristiti skriptu da biste pristanak za svaku vezu s OAuth. U odjeljku [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) projekta postoji skripte primjer na GitHub.

## <a name="visual-studio-release-management"></a>Upravljanje izdanje Visual Studio

Uobičajeni scenarij implementacije i upravljanja okruženju je da biste koristili alat kao što je upravljanje izdanje Visual Studio, s predloškom za implementaciju aplikacije logike. Team Services za Visual Studio obuhvaća zadatka [Implementacije grupa resursa Azure](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) možete dodati na bilo kojem Unaprjeđivanje ili pustite kanal. Morate koristiti [glavni servis](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) za autorizaciju za implementaciju, a zatim možete generirati definiciju izdanje.

1. U odjeljku Upravljanje izdanje, da biste stvorili novu definiciju, odaberite **prazan** da biste započeli prazan definicija.

    ![Stvorite novu, praznu definicija][1]   

1. Odaberite neki resursi morate za to. To vjerojatno će biti predložak za aplikaciju logike generira ručno ili u sklopu postupka Sastavi.
1. Dodavanje zadatka programa **Azure resursa grupe implementacije** .
1. Konfiguriranje sa [servisa glavni](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)i referencu datoteke predloška i parametri predložaka.
1. Nastavite izgradnje koraka u postupku izdanje za sve druge okruženja, automatiziranog test ili odobravatelji prema potrebi.

<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG
