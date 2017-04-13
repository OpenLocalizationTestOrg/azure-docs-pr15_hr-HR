
<properties
    pageTitle="Upravljanje resursima pomoću EŽA Azure | Microsoft Azure"
    description="Upravljanje Azure resurse i grupe pomoću sučelja naredbenog retka Azure (EŽA)"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="danlep"/>

# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Korištenje EŽA Azure za upravljanje Azure resursa i grupa resursa


> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Azure EŽA](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST API-JA](resource-manager-rest-api.md)


Azure sučelja naredbenog retka (Azure EŽA) je jedan od nekoliko alata koji omogućuju implementaciju i upravljanje resursima pomoću upravitelja resursa. U ovom se članku predstavlja uobičajenih načini upravljanja Azure resursa i grupa resursa pomoću EŽA Azure u načinu Voditelj resursa. Informacije o korištenju na EŽA za implementaciju resursa, potražite u članku [uvođenja resursa s resursima predloške i Azure EŽA](resource-group-template-deploy-cli.md). Pozadine o Azure resurse i resursima potražite u članku [Pregled Azure Voditelj resursa](azure-resource-manager/resource-group-overview.md).

>[AZURE.NOTE] Da biste upravljali Azure resursi EŽA Azure, morate [instalirati EŽA Azure](xplat-cli-install.md)i [prijavite se u sustav Azure](xplat-cli-connect.md) pomoću na `azure login` naredbu. Provjerite je li u EŽA u načinu Voditelj resursa (Pokreni `azure config mode arm`). Ako ste završili funkcioniraju, spremni ste za slanje.



## <a name="get-resource-groups-and-resources"></a>Grupa resursa i resursi

### <a name="resource-groups"></a>Grupa resursa

Da biste dobili popis svih grupa resursa u svoju pretplatu i njihovo mjesto, pokrenite sljedeću naredbu.

    azure group list
    

### <a name="resources"></a>Resursi
 Da biste dobili popis svih resursa u grupu, primjerice s nazivom *testRG*, koristite sljedeću naredbu.

    azure resource list testRG

Da biste pogledali pojedinačnog resursa unutar grupe, kao što je VM pod nazivom *MyUbuntuVM*, koristite naredbu kao što je sljedeća.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"
    
Obratite pozornost na to parametar **Microsoft.Compute/virtualMachines** . Ovaj parametar navodi vrstu resursa na koje ste zatražili informacije.
    
>[AZURE.NOTE]Kada koristite naredbe **azure resursa** umjesto naredbe za **popis** , morate navesti API verzija resursa s parametrom **– o** . Ako niste sigurni o API verzija, potražite datoteku predloška i pronađite polje apiVersion resursa. Da biste saznali više o verzijama API-JA u upravitelju resursa, pogledajte [davatelji Voditelj resursa, područja, verzije API-JA i sheme](resource-manager-supported-services.md).

Kada gledate detalje na resursa, često je korisno za korištenje na `--json` parametar. Ovaj parametar čini izlaz čitljiviji, jer su neke vrijednosti ugniježđene strukture ili zbirke. Sljedeći primjer prikazuje daje rezultate naredbe **Prikaži** kao JSON dokumenta.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

>[AZURE.NOTE] Ako želite, spremiti podatke JSON datoteke pomoću na &gt; znak za usmjeravanje Izlaz u datoteku. Ako, na primjer:
>
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

### <a name="tags"></a>Oznaka

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Upravljanje resursima


Da biste dodali resursa kao što je račun za pohranu u grupu resursa, pokrenite naredbu sličnu:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"
    
Osim određivanja verzija API resursa s parametrom **– o** , pomoću parametra **-p** prenesite niz JSON oblikovani obavezan ili dodatna svojstva.
    
    
Da biste izbrisali postojeći resurs kao što su resurs virtualnog računala, pomoću naredbe ovako.

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Da biste premjestili postojećih resursa u drugu grupu resursa ili na pretplatu, koristite naredbu **Premjesti azure resursa** . Sljedeći primjer prikazuje kako možete premjestiti predmemoriju Redis novu grupu resursa. U parametru **-i** navedite popis odvojenih zarezom identifikator resursa je da biste premjestili.


    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Kontrola pristupa resursima

Azure EŽA možete koristiti za stvaranje i upravljanje pravilima za kontrolu pristupa resursima Azure. Pozadine o definicijama pravila i dodjeljivanje pravilnika o resursima potražite u članku [Korištenje pravilnika za upravljanje resursima i nadzor pristupa](resource-manager-policy.md).

Na primjer, definiranje sljedeća pravila odbiti sve zahtjeve za gdje mjesto nije Zapad SAD-a ili Sjeverna središnje NAM i spremite je policy.json datoteka za definiciju pravila:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Zatim pokrenite naredbu **Stvaranje definicije pravila** :

    azure policy definition create MyPolicy -p c:\temp\policy.json
    
Ta se naredba prikazuje izlaz otprilike ovako.

    + Stvaranje pravila definicija podataka MyPolicy: PolicyName: MyPolicy podataka: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    podaci: PolicyType: prilagođenih podataka: riješiti problem: nedefinirana podataka: Opis: nedefinirana podataka: PolicyRule: polje = mjesto, u = [westus, northcentralus], efekt = odbiti

 Dodjeljivanje pravilnika u dosegu koji želite koristiti **PolicyDefinitionId** vratio prethodne naredbe. U sljedećem primjeru ovaj doseg je pretplata, ali možete ograničiti grupe resursa ili pojedinačnih resursa:

    Dodjela pravilnika Azure stvaranje MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

Možete dobiti, promijeniti ili ukloniti definicije pravila pomoću naredbe **Prikaži pravila definition**, **pravila definition postavite**i **izbrišite definiciju pravila** .

Isto tako, možete dobiti, promijeniti ili ukloniti dodjele pravila pomoću naredbe **Prikaži Dodjela pravilnika**, **postavite pravila dodjele**i **Brisanje Dodjela pravilnika** .


## <a name="export-a-resource-group-as-a-template"></a>Izvoz grupu resursa u obliku predloška

Postojeću grupu resursa, možete pregledati predložak resursima za grupu resursa. Izvoz predložak nudi dva pogodnosti:

1. Jednostavno možete automatizirati buduće implementacije rješenja jer sve infrastrukture definiran u predlošku.

2. Koje mogu se upoznali sa sintaksa predložak tako da pogledate JSON koji predstavlja rješenje.

Pomoću EŽA Azure, možete izvesti predložak koji predstavlja trenutno stanje grupu resursa, ili preuzimanje predloška koji je korišten za određeni implementaciju.

* **Izvoz predložak za grupu resursa** – to je korisno kada unijeli promjene u grupu resursa i zatrebati predstavljanje JSON njegovu trenutnom stanju. No stvoreni predložak sadrži samo Minimalni broj parametara i bez varijabli. Većina vrijednosti u predložak je programiranih. Prije no što implementirate stvoreni predložak želite pretvoriti više vrijednosti parametara tako da možete prilagoditi implementacije za različite okruženja.

    Da biste izvezli predložak za grupu resursa lokalnog imenika pokrenuti na `azure group export` naredbu kao što je prikazano u sljedećem primjeru. (Zamijenite lokalni direktorij odgovarajuće okruženju sustava operacijski sustav).

        azure group export testRG ~/azure/templates/

* **Preuzimanje predloška određenog implementacije** – to je korisno kada je potrebno da biste prikazali stvarne predložak koji je korišten za implementaciju resursi. Predložak uključuje sve parametre i definirano izvorne implementacije varijabli. Međutim, ako netko iz vaše tvrtke ili ustanove unijeli promjene u grupi resursa izvan definiciju u predlošku, ovaj predložak ne predstavljaju trenutno stanje grupu resursa.

    Da biste preuzeli predložak koji se koristi za određeni implementaciju lokalnom direktoriju, pokrenite na `azure group deployment template download` naredbe. Ako, na primjer:

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] Izvoz predložak je u pretpregledu, a sve vrste resursa trenutno podržava izvoz predloška. Prilikom pokušaja izvoz predloška, vidjet ćete pogrešku kojoj se navodi da neki resursi nisu izvezeni. Ako je potrebno, ručno definirati tih resursa u predlošku nakon preuzimanja.



## <a name="next-steps"></a>Daljnji koraci

* Dohvaćanje detalja postupci implementacije i Otklonite pogreške prilikom implementacije s EŽA Azure potražite u članku [Prikaz postupci implementacije s EŽA Azure](resource-manager-troubleshoot-deployments-cli.md).
* Ako želite koristiti u EŽA da biste postavili aplikacije ili skriptu da biste pristupili resursima potražite u članku [Korištenje Azure EŽA da biste stvorili glavni pristupa resursima servisa](resource-group-authenticate-service-principal-cli.md).


