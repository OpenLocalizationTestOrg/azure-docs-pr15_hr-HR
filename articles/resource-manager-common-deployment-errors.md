<properties
   pageTitle="Otklanjanje uobičajenih pogrešaka Azure implementacije | Microsoft Azure"
   description="U članku se opisuje kako razriješiti uobičajenih pogrešaka prilikom implementacije resursi Azure pomoću upravitelja resursa Azure."
   services="azure-resource-manager"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   keywords="Uvođenje pogreške, azure implementacije implementirati za azure"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Otklanjanje uobičajenih pogrešaka Azure implementaciju s Azure Voditelj resursa

U ovoj se temi opisuje kako riješiti neki zajednički Azure implementacije pogreške koji se mogu pojaviti. Ako trebate dodatne informacije o čemu je problem u svojoj implementaciji, najprije potražite u članku [Prikaz postupci implementacije](resource-manager-troubleshoot-deployments-portal.md) , a zatim vratite se na članak pomoć pri otklanjanju pogreške. U odjeljcima u ovoj temi navedeni kod pogreške koji se prikaže.

## <a name="invalid-template"></a>Predložak koji nisu valjani

Ta se pogreška može uzrokovati iz nekoliko različitih vrsta pogreške. 

### <a name="syntax-error"></a>Pogreška u sintaksi

Ako primite poruku o pogrešci koja označava predložak nije uspjela provjera valjanosti, možda ćete sintaksa problem u predlošku. 

    Code=InvalidTemplate 
    Message=Deployment template validation failed

Ta se pogreška je jednostavno uspostaviti jer je predložak izrazi mogu biti složene. Ako, na primjer, sljedeće dodjele naziv računa spremišta sadrži jedan skup zagrade, tri funkcije, tri skupa zagrade, jedan skup jednostrukim navodnicima i jedno svojstvo:

    "name": "[concat('storage', uniqueString(resourceGroup().id))]",

Ako ne navedete odgovarajuće sintakse, predložak daje vrijednost koja se razlikuje od namjeru.

Kada primite tu vrstu pogreške, pažljivo pročitajte članak sintaksu izraza. Razmislite o korištenju programa za uređivanje JSON kao što su [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) i [Visual Studio kod](resource-manager-vs-code.md)koje možete Upozori pogreške sintakse. 

### <a name="incorrect-segment-lengths"></a>Netočan segmenta duljine

Drugi koji nisu valjani predložak pogreška se pojavljuje kada naziv resursa nije u ispravnom obliku.

    Code=InvalidTemplate
    Message=Deployment template validation failed: 'The template resource {resource-name}' 
    for type {resource-type} has incorrect segment lengths.

Resurs za razine korijenski mora imati jedan manje segment naziva od u vrstu resursa. Svaki segment razlikuju se kosom crtom. U sljedećem primjeru vrstu sadrži dva i naziv sadrži jedan segment tako da je **valjan naziv**.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "myHostingPlanName",
      ...
    }

Dok je u sljedećem primjeru **nije valjani naziv** jer ima isti broj segmenata kao vrstu.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "appPlan/myHostingPlanName",
      ...
    }

Podređeni resursa, vrsta i naziv imati isti broj segmenata. Taj broj segmenata smisla jer puni naziv i vrsta za dijete sadrži naziv i vrsta. Dakle, ime i prezime i dalje ima jedan segment manje od puno vrsta. 

    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "contosokeyvault",
            ...
            "resources": [
                {
                    "type": "secrets",
                    "name": "appPassword",
                    ...
                }
            ]
        }
    ]

Uvod u segmenata desnom može biti Škakljivo s vrstama Voditelj resursa koji se primjenjuju preko davatelji resursa. Primjena resursa zaključavanja na web-mjesto, na primjer, zahtijeva vrste s četiri segmenata. Stoga je ime tri segmenata:

    {
        "type": "Microsoft.Web/sites/providers/locks",
        "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
        ...
    }

### <a name="copy-index-is-not-expected"></a>Kopiraj indeks se očekuje

Ta se pogreška **InvalidTemplate** naići kada ste primijenili element **Kopiranje** dijela predloška koji ne podržavaju taj element. Vrsta resursa možete primijeniti samo na element Kopiraj. Svojstvo unutar vrsta resursa nije moguće primijeniti Kopiraj. Na primjer, primijenite Kopiraj virtualnog računala, ali ne možete primijeniti na diskova OS za virtualnog računala. U nekim slučajevima može pretvoriti podređeni resursa nadređenog resursa da biste stvorili kopiju petlje. Dodatne informacije o korištenju Kopiraj potražite u članku [Stvaranje više instanci resursa u Azure Voditelj resursa](resource-group-create-multiple.md).

### <a name="parameter-is-not-valid"></a>Parametar nije valjan

Ako predložak određuje dopušteno vrijednosti parametra, a sadrže vrijednost koja nije među te se vrijednosti, dobit ćete poruku koja je slična sljedeća pogreška:

    Code=InvalidTemplate; 
    Message=Deployment template validation failed: 'The provided value {parameter vaule} 
    for the template parameter {parameter name} is not valid. The parameter value is not 
    part of the allowed value(s)

Dvostruki potvrdite dopuštena vrijednost u predlošku, a zatim navedite ga tijekom implementacije.

## <a name="not-found-or-resource-not-found"></a>Nije pronađen (ili resursa nije pronađeno)

Ako predložak sadrži naziv resursa koje nije moguće razriješiti, primite poruku o pogrešci slična:

    Code=NotFound;
    Message=Cannot find ServerFarm with name exampleplan.

Ako pokušavate implementacija nedostaje resursa u predlošku, provjerite potrebno da biste dodali u opsegu. Voditelj resursa optimizira implementacije stvaranjem resursi paralelno, kada je to moguće. Ako jedan resurs mora biti implementirano nakon drugog resursa, morate koristiti **dependsOn** element u predlošku da biste stvorili ovisnosti na neki drugi resurs. Ako, na primjer, kada implementacija web-aplikacijama aplikacije servisa za planiranje mora postojati. Ako niste naveli da web-aplikaciji ovisi o aplikacije servisa za planiranje, resursima stvara i resursima u isto vrijeme. O pogrešci koja kaže da nije moguće pronaći aplikacije servisa za planiranje resursa, jer postoji još prilikom pokušaja svojstvo na web-aplikaciji. Ta se pogreška spriječiti postavljanjem ovisnosti u web-aplikaciji.

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }
 
Možete vidjeti i ta se pogreška kada postoji resursa u grupu resursa za različite od onog koji se implementiran na. U tom slučaju pomoću [funkcije resourceId](resource-group-template-functions.md#resourceid) da biste potpuno kvalificiran naziv resursa.

    "properties": {
        "name": "[parameters('siteName')]",
        "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
    } 

Ako pokušate koristiti funkcije [Referenca](resource-group-template-functions.md#referenc) ili [listKeys](resource-group-template-functions.md#listkeys) resursa koje nije moguće razriješiti će se sljedeća pogreška:

    Code=ResourceNotFound;
    Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource 
    group {resource group name} was not found.

Potražite izraz koji pripada i funkcija **referencu** . Provjerite jesu li vrijednosti parametara ispravni.

## <a name="storage-account-already-exists-or-already-taken"></a>Već postoji račun za pohranu (ili već preuzeli)

Za korisničke račune za pohranu, navedite naziv resursa koja je jedinstvena preko Azure. Ako ne unesete jedinstveni naziv, primite poruku o pogrešci kao što su:

    Code=StorageAccountAlreadyTaken 
    Message=The storage account named mystorage is already taken.

Možete stvoriti jedinstveni naziv spajanjem vaše konvencija imenovanja rezultatom funkcije [uniqueString](resource-group-template-functions.md#uniquestring) .

    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]",
    "type": "Microsoft.Storage/storageAccounts",

Ako implementacije prostora za pohranu račun s istim nazivom kao postojeći račun za pohranu u pretplatu, ali sadrže na drugo mjesto, primit ćete poruku pogreške koja ukazuje na neko drugo mjesto već postoji račun za pohranu. Izbrišite postojeći račun za pohranu ili pružaju na isto mjesto postojećeg računa za pohranu.

## <a name="account-name-invalid"></a>Naziv računa koji nisu valjani

Vidjet ćete **AccountNameInvalid** pogreške pri pokušaju naziv računa za pohranu koji sadrži zabranjene znakove. Imena pohranu račun mora biti između 3 i 24 znakova i koristiti brojke i malih slova.

## <a name="no-registered-provider-found"></a>Nijedan registrirani davatelj pronaći

Kada implementirate resursa, možda ćete dobiti sljedeći kod pogreške i poruke:

    Code: NoRegisteredProviderFound
    Message: No registered resource provider found for location {ocation} 
    and API version {api-version} for type {resource-type}.

Ta se pogreška pojavljuje se zbog nekog od tri razloga:

1. Mjesto nije podržana za vrstu resursa
2. API verzija nije podržana za vrstu resursa
3. Davatelja resursa nije registriran za vašu pretplatu

Poruka o pogrešci mora vam dati prijedloge za podržane mjesta i verzije API-JA. Predložak možete promijeniti u neku od predloženih vrijednosti. Većina davatelja usluga su registrirani automatski prema portalu Azure ili sučelja naredbenog retka koji koristite, ali ne svim. Ako niste koristili određeni resurs davatelja prije, možda će biti potrebno registrirati taj davatelj. Dodatne informacije o davateljima resursa putem komponente PowerShell ili Azure EŽA mogu otkriti.

### <a name="powershell"></a>PowerShell

Da biste vidjeli vaš porezni status, koristite **Get-AzureRmResourceProvider**.

    Get-AzureRmResourceProvider -ListAvailable

Da biste registrirali davatelja, pomoću **Register AzureRmResourceProvider** i navedite naziv davatelja resursa koji želite registrirati.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

Da biste dobili podržani mjesta za određene vrste resursa, koristite:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Da biste dobili podržanim verzijama API-JA za određene vrste resursa, koristite:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

### <a name="azure-cli"></a>Azure EŽA

Da biste vidjeli je li usluga registriran, koristite na `azure provider list` naredbe.

    azure provider list

Da biste registrirali davatelja resursa, poslužite se `azure provider register` naredbu, a zatim navedite *mjesto za naziv* da biste registrirali.

    azure provider register Microsoft.Cdn

Da biste vidjeli podržani mjesta i verzije API-JA za davatelja resursa, koristite:

    azure provider show -n Microsoft.Compute --json > compute.json

## <a name="operation-not-allowed"></a>Operacija nije dopuštena

Možda imate problema prilikom implementacije premašuje ograničenja, koja može biti po grupa resursa, pretplate, račune i drugih dosega. Ako, na primjer, vaša pretplata možda konfigurirana da biste ograničili broj jezgri za područje. Ako pokušate uvesti virtualnog računala s više jezgri od iznosa dopušteno, primit ćete pogrešku s porukom prekoračena kvota.
Dovršavanje kvote informacije potražite u članku [Azure pretplate i ograničenja servisa, kvote, i ograničenja](azure-subscription-service-limits.md).

Da biste pregledali kvote za svoju pretplatu za jezgri, možete koristiti u `azure vm list-usage` naredbe u EŽA Azure. Sljedeći primjer prikazuje da je core kvote za besplatnu probnu račun 4:

    azure vm list-usage

Koja vraća:

    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

Ako uvođenje predloška koji stvara više od četiri jezgri u regiji Zapad SAD-a, prikazat će se pogreška implementaciju koji izgleda kao:

    Code=OperationNotAllowed
    Message=Operation results in exceeding quota limits of Core. 
    Maximum allowed: 4, Current in use: 4, Additional requested: 2.

Ili u ljusci PowerShell, koristite cmdlet **Get-AzureRmVMUsage** .

    Get-AzureRmVMUsage

Koja vraća:

    ...
    CurrentValue : 0
    Limit        : 4
    Name         : {
                     "value": "cores",
                     "localizedValue": "Total Regional Cores"
                   }
    Unit         : null
    ...

U tim slučajevima, otvorite portal i datoteke problem podrška za Potenciranje kvotu za regiju u koju želite uvesti.

> [AZURE.NOTE] Imajte na umu da za grupe resursa, kvote za svako područje pojedinačne, ne i za cijelu pretplatu. Ako vam je potrebna za implementaciju 30 jezgri u Zapad SAD-a, morate zatražite od 30 jezgri Voditelj resursa u Zapad SAD-a. Ako vam je potrebna za implementaciju 30 jezgri u bilo kojem od područja u koje imate pristup, zamolite za 30 jezgri Voditelj resursa u svim regijama.

## <a name="invalid-content-link"></a>Veza na sadržaj koji nisu valjani

Kada primite poruku o pogrešci:

    Code=InvalidContentLink
    Message=Unable to download deployment content from ...

Vjerojatno ste pokušali povezati ugniježđene predložak koji nije dostupna. Provjerite URI namijenjeno ugniježđene predložak. Ako predložak postoji u račun za pohranu, provjerite je li URI nije dostupna. Možda ćete morati proći SAS token. Dodatne informacije potražite u članku [Rad s predlošcima povezane s Azure Voditelj resursa](resource-group-linked-templates.md).

## <a name="authorization-failed"></a>Autorizacija nije uspjela

Možda ćete primiti pogrešku tijekom implementacije jer račun ili servis glavni pokušaju uvesti resurse imati pristup izvođenje ovih akcija. Azure Active Directory omogućuje ili administratoru da biste odredili koji identiteta možete pristupiti resursima s sjajno stupanj točnosti. Ako, na primjer, ako vaš račun je dodijeljena uloga čitač, niste moći stvoriti resursi. U tom slučaju vidite poruku o pogrešci koja navodi da autorizacija nije uspjelo.

Dodatne informacije o kontrola pristupa na temelju uloga potražite u članku [Upravljanje pristupom Azure Role-Based](./active-directory/role-based-access-control-configure.md).

Osim kontrola pristupa na temelju uloga akcija implementacije može biti ograničeno količinom pravila za pretplatu. Pomoću pravila, administrator možete nametnuti konvencije na svim resursima u uveden u pretplatu. Na primjer, administrator možete zatražiti određenu oznaku vrijednost je namijenjeno vrsta resursa. Ako ne ispunjavaju uvjete pravila, primite poruku o pogrešci tijekom implementacije. Dodatne informacije o pravilima potražite u članku [Korištenje pravilnika za upravljanje resursima i nadzor pristupa](resource-manager-policy.md).

## <a name="troubleshooting-virtual-machines"></a>Otklanjanje poteškoća s virtualnim strojevima

| Pogreška | Članci |
| -------- | ----------- |
| Prilagođene skripte kućni broj pogreške | [Windows VM kućni broj neuspjeha](./virtual-machines/virtual-machines-windows-extensions-troubleshoot.md)<br />ili<br />[Linux VM kućni broj neuspjeha](./virtual-machines/virtual-machines-linux-extensions-troubleshoot.md) |
| Slika s operacijskim Sustavom pogreške u dodjeljivanju | [Nove pogreške VM za Windows](./virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md)<br />ili<br />[Nove Linux VM pogreške](./virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md ) |
| Pogrešaka pri dodjeli | [Pogrešaka pri dodjeli VM za Windows](./virtual-machines/virtual-machines-windows-allocation-failure.md)<br />ili<br />[Pogrešaka pri dodjeli Linux VM](./virtual-machines/virtual-machines-linux-allocation-failure.md) |
| Sigurna ljuske (SSH) pogreške prilikom pokušaja povezivanja | [Sigurnu vezu ljuske za Linux VM](./virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md) |
| Pogreške prilikom povezivanja s aplikacije koje se izvode na VM | [Aplikacije koje se izvode na VM za Windows](./virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md)<br />ili<br />[Aplikacije koje se izvode na Linux VM](./virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md) |
| Pogreške udaljene radne površine | [Udaljene radne površine veze s poslužiteljem Windows VM](./virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md) |
| Pogreške pri povezivanju rješava redeploying | [Da biste novi Azure čvor implementirati virtualnog računala](./virtual-machines/virtual-machines-windows-redeploy-to-new-node.md) |
| Pogreške servisa oblaka | [Oblak servisa implementacije problema](./cloud-services/cloud-services-troubleshoot-deployment-problems.md) |

## <a name="troubleshooting-other-services"></a>Otklanjanje poteškoća s ostalim servisima

U sljedećoj su tablici nije popis svih tema za otklanjanje poteškoća za Azure. Umjesto toga, ona se usredotočuje na problema vezanih uz implementacija ili konfiguriranju resursi. Ako vam je potrebna pomoć za otklanjanje problema s izvođenju s resursa, potražite u dokumentaciji za Azure servis.

| Servis | Članak |
| -------- | -------- |
| Automatizacija | [Savjeti za otklanjanje poteškoća za uobičajenih pogrešaka u automatizaciji Azure](./automation/automation-troubleshooting-automation-errors.md) |
| Azure stogu | [Otklanjanje poteškoća s Microsoft Azure stogu](./azure-stack/azure-stack-troubleshooting.md) |
| Azure stogu | [Web-aplikacije i stoga Azure](./azure-stack/azure-stack-webapps-troubleshoot-known-issues.md ) |
| Tvorničke podataka | [Otklanjanje poteškoća tvorničke podataka](./data-factory/data-factory-troubleshoot.md) |
| Tkanina servisa | [Rješavanje uobičajenih problema prilikom implementirali servise na tkanina servisa Azure](./service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) |
| Oporavak web-mjesta | [Praćenje i otklanjanje poteškoća s zaštitu virtualnim strojevima i fizičke poslužitelja](./site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Prostor za pohranu | [Praćenje, dijagnosticiranje i otklanjanje poteškoća s spremište na platformi Microsoft Azure](./storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple | [Otklanjanje poteškoća s StorSimple uređaj implementacije](./storsimple/storsimple-troubleshoot-deployment.md) |
| SQL baze podataka | [Rješavanje problema s povezivanjem s bazom podataka SQL Azure](./sql-database/sql-database-troubleshoot-common-connection-issues.md) |
| SQL Data Warehouse | [Otklanjanje poteškoća Data Warehouse Azure SQL](./sql-data-warehouse/sql-data-warehouse-troubleshoot.md) |

## <a name="understand-when-a-deployment-is-ready"></a>Razumijevanje kad je spremna implementacije

Azure Voditelj resursa uspjeh izvješća o implementacije kada svih proizvođača uspješno vratite iz implementacije. Međutim, ova poruka ne mora značiti da je grupu resursa "aktivna i jeste li spremni za korisnike." Ako, na primjer, implementacije možda morati preuzeti nadogradnje, pričekajte resursa koji nisu predložak ili instalirati složene skripte. Voditelj resursa znati kada dovršiti sljedeće zadatke jer nisu aktivnosti koje prati davatelja. U tim slučajevima može biti neko vrijeme da se Resursi su spremni za korištenje stvarnog života. Zbog toga možete očekivati da status uvođenja uspješnog neko vrijeme prije korištenja implementaciju sustava.

Azure možete spriječiti izvješćivanja implementacije uspjeha, međutim, kao što su stvaranje prilagođene skripte za prilagođeni predložak. Skripta ne zna praćenje cijelu implementacije za pripremu sistemskih i vraća uspješno samo kada je korisnicima omogućuje interakciju s cijelu implementacije. Ako želite da bi vaš nastavak je kraj da biste pokrenuli, svojstvo **dependsOn** koristiti u predlošku.

## <a name="next-steps"></a>Daljnji koraci

- Da biste saznali više o nadzoru Akcije, potražite u članku [nadzora operacije s Voditelj resursa](resource-group-audit.md).
- Da biste saznali više o akcije da biste odredili pogreške tijekom implementacije, potražite u članku [Prikaz postupci implementacije](resource-manager-troubleshoot-deployments-portal.md).
