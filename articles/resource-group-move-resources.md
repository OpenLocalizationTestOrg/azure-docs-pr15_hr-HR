<properties 
    pageTitle="Premještanje resursa u novu grupu resursa | Microsoft Azure" 
    description="Pomoću upravitelja resursa Azure premjestili resursa u novu grupu resursa ili pretplate." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/21/2016" 
    ms.author="tomfitz"/>

# <a name="move-resources-to-new-resource-group-or-subscription"></a>Premještanje resursa ili pretplatu za novu grupu resursa

U ovoj se temi objašnjava premještanje resursa na novu pretplatu ili novu grupu resursa u okviru iste pretplate. Da biste premjestili resursa možete koristiti portalu, PowerShell, Azure EŽA ili REST API-JA. Premještanje operacije u ovoj temi dostupne su vam bez pomoći za sve Azure podrške.

Obično je da se Resursi pomicanje kada ste odlučili da:

- Za naplatu svrhe, resurs mora live u neku drugu pretplatu.
- Resurs više zajednički koristi iste životnog ciklusa kao resurse prethodno grupiranih s. Koju želite premjestiti u novu grupu resursa da bi mogao upravljati resursa zasebno iz drugih resursa.

Prilikom pomicanja resursi, grupe izvora i odredišnu grupu su zaključane tijekom postupka. Pisanje i brisanje operacije blokiraju na grupa dok se ne dovrši premještanje.

Ne možete promijeniti mjesto resursa. Premještanje resursa samo poruku premješta u novu grupu resursa. Nova grupa resursa možda na drugo mjesto, ali koje promijeniti mjesto resursa.

> [AZURE.NOTE] U ovom se članku opisuje kako premjestiti resursi unutar postojeće Azure računa offering. Ako zapravo želite promijeniti vaš račun za Azure koja nudi (kao što su nadogradili pay-as-you-go unaprijed platiti) tijekom nastavka za rad s postojećih resursa, potražite u članku [Prijelaz Azure pretplatu za drugu ponudu](billing-how-to-switch-azure-offer.md). 

## <a name="checklist-before-moving-resources"></a>Kontrolni popis prije prelaska resursi

Postoji nekoliko važnih koraka da biste izvršili prije prelaska resurs. Provjerom ovih uvjeta izbjegavanje pogrešaka.

1. Servis potrebno je omogućiti mogućnost da biste premjestili resursi. Potražite na popisu ispod informacije o koje [usluge omogućiti premještanje resursi](#services-that-enable-move).
1. Pretplata na izvorišne i odredišne moraju se nalaziti u istom [klijentu servisa Active Directory](./active-directory/active-directory-howto-tenant.md). Da biste premjestili novi klijent, nazovite podršku.
2. Pretplata odredište mora biti registriran za davatelja resursa resursa premješten. Ako nije, primite poruku o pogrešci da se **pretplate nije registriran za vrstu resursa**. Koje možete naići problem prilikom pomicanja resursa novu pretplatu, ali koje pretplata nikada niste koristili s tom vrstom resursa. Da biste saznali kako provjeriti stanje Registracija i registrirati davatelji resursa, potražite u članku [vrste i resursa davatelji](../resource-manager-supported-services.md#resource-providers-and-types).
4. Ako premještate aplikacije servisa za aplikaciju, pregledate [ograničenja aplikacije servisa](#app-service-limitations).
4. Ako premještate resursi povezani sa servisima za oporavak, pregledate [ograničenja oporavak Services](#recovery-services-limitations)
5. Ako premještate resursa u uveden klasični model, pregledate [Klasični implementacije ograničenja](#classic-deployment-limitations).

## <a name="when-to-call-support"></a>Kada se nazovite službu za podršku

Možete premjestiti Većina resursi putem samoposlužnog operacije prikazano u nastavku. Korištenje samoposlužnog operacije da biste:

- Premještanje resursima resursi.
- Premještanje klasični resursi prema [uvođenje klasičnog ograničenja](#classic-deployment-limitations). 

Kada se morate, nazovite službu za podršku:

- Premještanje resursa na novi račun za Azure (i klijentu servisa Active Directory).
- Premještanje klasični resursa, ali imate poteškoća sa ograničenja.

## <a name="services-that-enable-move"></a>Servise koji omogućuju Premjesti

Zasad su servise koji omogućuju premještanje u novu grupu resursa i pretplate:

- Upravljanje API-JA
- Aplikacije servisa za aplikacije (aplikacija za web -) potražite u članku [ograničenja servisa aplikacija](#app-service-limitations)
- Automatizacija
- Grupe
- CDN
- Servisi u oblaku – potražite u članku [ograničenja klasični implementacije](#classic-deployment-limitations)
- Tvorničke podataka
- DNS-A
- DocumentDB
- HDInsight klastere
- IoT koncentratora
- Ključni zbirke ključeva 
- Media Services.
- Mobilni radnje
- Obavijest o koncentratora
- Radu uvida
- Redis predmemorije
- Raspored
- Pretraživanje
- Servis Bus
- Prostor za pohranu
- Prostor za pohranu (klasični) – potražite u članku [ograničenja klasični implementacije](#classic-deployment-limitations)
- Poslužitelj baze podataka SQL - baze podataka i poslužitelja moraju se nalaziti u istoj grupi resursa. Kada premještate SQL server, sve njegove baza podataka također će se premjestiti.
- Virtualnim računalima sustava –, međutim, ne podržava premještanje na novu pretplatu prilikom njegova certifikati su pohranjeni u ključ sigurnog
- Virtualnim strojevima (klasični) – potražite u članku [ograničenja klasični implementacije](#classic-deployment-limitations)
- Virtualne mreže

## <a name="services-that-do-not-enable-move"></a>Servise koji omogućuju Premjesti

Servise koje trenutno ne omogućite premještanje resursa su:

- Pristupnik za aplikaciju
- Aplikacija uvida
- Usmjeravanje Express
- Oporavak servisa sigurnog - i premještanje računalnim, mreže i pohranu resursi pridružene oporavak servisa sigurnog, pročitajte članak [ograničenja servisa za oporavak](#recovery-services-limitations).
- Virtualnim strojevima certifikatom pohranjene u sigurnog ključ
- Skupovi virtualnim strojevima skala
- Virtualne mreže (klasični) – potražite u članku [ograničenja klasični implementacije](#classic-deployment-limitations)
- Pristupnik za VPN-a

## <a name="app-service-limitations"></a>Ograničenja servisa aplikacija

Kada radite s aplikacijama aplikacije servisa, ne možete premještati samo tarifu aplikacije servisa. Da biste premjestili aplikacije servisa za aplikacije, nekoliko mogućnosti:

- Premještanje aplikacije servisa za planiranje i ostale aplikacije servisa za resurse u toj grupi resursa u novu grupu resursa koji još ne sadrži resursi za servisnu aplikaciju. Taj zahtjev, znači da morate premjestiti čak i resursima aplikacije servisa za koje su vezane uz aplikacije servisa za planiranje. 
- Premještanje aplikacije u grupu različite resursa, ali imajte sve aplikacije servisa za tarife na izvornu grupu resursa.

Ako izvorni grupu resursa obuhvaća i do uvida aplikacije resursa, ne možete premještati resursa jer aplikacije uvida omogućite trenutno postupak premještanja. Ako uvrstite resursa aplikacije uvida prilikom pomicanja aplikacije servisa za aplikacije, postupak cijelu premještanja neće uspjeti. Međutim, uvida aplikacije i aplikacije servisa za planiranje ne moraju se nalaziti u istoj grupi resursa aplikacija za aplikaciju za pravilno funkcionirati.

Na primjer, ako sadrži grupu resursa:

- **web-a** koji je pridružen **plan-a** i **aplikacije-uvida-a**
- **web-b** koji je pridružen **plan b** i **aplikacije, uvida i b**

Nekoliko mogućnosti:

- Premještanje **web-a**, **plan a**, **web-b**i **Planiranje b**
- Premještanje **web-a** i **b za web**
- Premještanje **web-a**
- Premještanje **web-b**

Sve kombinacije obuhvaćaju premještanje vrsta resursa koje nije moguće premjestiti (aplikaciju uvida) ili ostavite iza vrsta resursa koji se ne može ostati iza prilikom pomicanja aplikacije servisa za planiranje (bilo koju vrstu aplikacije servisa za resursa).

Ako web-aplikaciju programa koji se nalazi u grupu resursa za različite od njezinih aplikacije servisa za planiranje, ali želite premjestiti oba u novu grupu resursa, morate izvršiti premještanje u dva koraka. Ako, na primjer:

- **web-a** nalazi se u **web-grupe**
- **Planiranje-a** nalazi se u **plan grupe**
- Želite **web-a** i **Planiranje-a** smjestiti u **kombinirane grupi**

Da biste izvršili ovaj Premjesti izvesti dvije zasebne Premjesti operacije ovim redoslijedom:

1. Premještanje na **web-a** **plan** grupu
2. Premještanje **web-a** i **Planiranje-a** **kombinirati**grupu.

Servis za potvrdu aplikacije možete premjestiti na novu grupu resursa ili pretplatu bez problema. Međutim, ako web-aplikaciju programa obuhvaća SSL certifikata koje ste kupili vanjsko i prenijeli na aplikaciju, morate izbrisati certifikat prije prelaska web-aplikaciji. Na primjer, možete poduzeti sljedeće korake:

1. Brisanje prenesene certifikat iz web-aplikacije
2. Premještanje web-aplikaciji
3. Prijenos certifikata na web-aplikaciji

## <a name="recovery-services-limitations"></a>Ograničenja servisa oporavka

Premještanje nije omogućen za pohranu, u mreži, ili izračunati resursa koje služe za postavljanje Izrada oporavak uz oporavak Azure web-mjesta. 

Na primjer, pretpostavimo da ste postavili replikacije vaše lokalne računala s računom za pohranu (Storage1) i želite zaštićene računala da biste se pojavljuju nakon prebacivanje na Azure kao virtualnog računala (VM1) priložiti virtualne mreže (Network1). Ne možete premještati bilo koju od tih Azure resursa - Storage1, VM1 i Network1 - preko grupe resursa unutar iste pretplate ili uzduž pretplate.

## <a name="classic-deployment-limitations"></a>Uvođenje klasičnog ograničenja

Mogućnosti za premještanje resursa u uveden u klasični model razlikuju se ovisno o tome koje premještate resursi unutar pretplatu ili novu pretplatu. 

### <a name="same-subscription"></a>Iste pretplate

Prilikom pomicanja resursa u jednu grupu resursa u drugu grupu resursa unutar iste pretplate, primjenjuju se sljedeća ograničenja:

- Nije moguće premjestiti virtualne mreže (klasični).
- Sa servisom cloud moraju premještati virtualnim strojevima (klasični). 
- Servis u oblaku se može premjestiti samo kada premještanje obuhvaća sve njegove virtualnih računala.
- Samo jedan u oblaku, možete premjestiti odjednom.
- Samo jedan za pohranu račun (klasični), možete premjestiti odjednom.
- Račun za pohranu (klasični) nije moguće premjestiti u isti postupak s virtualnog računala ili servis u oblaku.

Da biste premjestili klasični resursa u novu grupu resursa unutar iste pretplate, pomoću standardnih Premjesti operacije kroz [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure EŽA](#use-azure-cli)ili [REST API -JA](#use-rest-api). Koristite iste operacije prilikom korištenja za premještanje resursima resursi.

### <a name="new-subscription"></a>Novu pretplatu

Prilikom pomicanja resursi novu pretplatu, primjenjuju se sljedeća ograničenja:

- Sve klasični resursa u pretplatu mora se premjestiti u isti postupak.
- Cilj pretplate ne smije sadržavati druge klasični resurse.
- Premještanje moguće je zatražio samo putem u zasebnom REST API-JA za klasični poteza. Naredbe za premještanje standardne resursima ne funkcionira prilikom pomicanja klasični resursi novu pretplatu.

Da biste premjestili klasični resursi novu pretplatu, morate koristiti OSTALE operacije koje su specifične za klasični resursi. Izvršite sljedeće korake da biste premjestili klasični resursi novu pretplatu.

1. Potvrdite okvir ako pretplatu izvor može sudjelovati u više pretplata Premjesti. Pomoću sljedećeg postupka:

         POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
    
     U tijelu zahtjeva obuhvaćaju sljedeće:

         {
           "role": "source"
         }

     Je odgovor za postupak provjere valjanosti u sljedećem obliku:

         {
           "status": "{status}",
           "reasons": [
             "reason1",
             "reason2"
           ]
         }

2. Potvrdite okvir ako pretplatu odredište može sudjelovati u više pretplata Premjesti. Pomoću sljedećeg postupka:

         POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01

     U tijelu zahtjeva obuhvaćaju sljedeće:

         {
           "role": "target"
         }

     U istom obliku provjere valjanosti za pretplatu izvora je odgovor.

3. Ako oba pretplate prošla provjeru, premještanje svih klasični resursa iz jedne pretplate na drugu pretplatu pomoću sljedećeg postupka:

         POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01

    U tijelu zahtjeva obuhvaćaju sljedeće:

         {
           "target": "/subscriptions/{target-subscription-id}"
         }

Postupak može potrajati nekoliko minuta. 

## <a name="use-portal"></a>Korištenje portala

Da biste premjestili resursa u novu grupu resursa u **iste pretplate**, odaberite grupu resursa koji sadrži tih resursa, a zatim gumb **Premjesti** .

![Premještanje resursi](./media/resource-group-move-resources/edit-rg-icon.png)

Ili, da biste premjestili resursa na **novu pretplatu**, odaberite grupu resursa koji sadrži tih resursa, a zatim odaberite ikonu pretplatu za uređivanje.

![Premještanje resursi](./media/resource-group-move-resources/change-subscription.png)

Odaberite resursi za premještanje i odredišnu grupu resursa. Prihvatite da morate ažurirati skripte za ove resurse, a zatim odaberite **u redu**. Ako ste odabrali ikonu pretplatu za uređivanje u prethodnom koraku, morate odabrati i pretplate odredište.

![Odaberite odredište](./media/resource-group-move-resources/select-destination.png)

U **obavijesti**, pogledajte postupak premještanja traje.

![Prikaži stanje prijenosa](./media/resource-group-move-resources/show-status.png)

Kada se dovrši, bit ćete obaviješteni rezultata.

![Prikaz rezultata za premještanje](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>Pomoću komponente PowerShell

Da biste premjestili postojećih resursa u drugu grupu resursa ili na pretplatu, koristite naredbu **Premjesti AzureRmResource** .

Prvi primjer pokazuje kako premjestiti jedan resurs u novu grupu resursa.

    $resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId

Drugi primjer pokazuje kako premjestiti više resursa u novu grupu resursa.

    $webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
    $plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId

Da biste se premjestili na novu pretplatu, uvrstite vrijednost za parametar **DestinationSubscriptionId** .

Se od vas zatraži da biste potvrdili da želite premjestiti navedenih resursa.

    Confirm
    Are you sure you want to move these resources to the resource group
    '/subscriptions/{guid}/resourceGroups/newRG' the resources:

    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

## <a name="use-azure-cli"></a>Korištenje Azure EŽA

Da biste premjestili postojećih resursa u drugu grupu resursa ili na pretplatu, koristite naredbu **Premjesti azure resursa** . Morate unijeti ID-a resursa resursa koje želite premjestiti. Prikazat će vam resursa ID-a pomoću sljedeće naredbe:

    azure resource list -g sourceGroup --json

Koja vraća sljedećem obliku:

    [
      {
        "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
        "name": "storagedemo",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "southcentralus",
        "tags": {},
        "kind": "Storage",
        "sku": {
          "name": "Standard_RAGRS",
          "tier": "Standard"
        }
      }
    ]

Sljedeći primjer prikazuje kako možete premjestiti na račun za pohranu u novu grupu resursa. U parametru **-i** navedite popis odvojenih zarezom identifikator resursa je da biste premjestili.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
    
Se od vas zatraži da biste potvrdili da želite premjestiti navedenog resursa.

## <a name="use-rest-api"></a>Korištenje REST API-JA

Da biste premjestili postojećih resursa u drugu grupu resursa ili na pretplatu, pokrenite:

    POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version} 

U tijelu zahtjeva odrediti odredišnu grupu resursa i resurse za premještanje. Dodatne informacije o ostalih operacija premještanja potražite u članku [Premještanje resursi](https://msdn.microsoft.com/library/azure/mt218710.aspx).


## <a name="next-steps"></a>Daljnji koraci
- Da biste saznali više o PowerShell cmdleti za Upravljanje pretplatom, potražite u članku [Korištenje Azure PowerShell s Voditelj resursa](powershell-azure-resource-manager.md).
- Da biste saznali više o Azure EŽA naredbe za Upravljanje pretplatom, pogledajte odjeljak [Korištenje EŽA Azure s Voditelj resursa](xplat-cli-azure-resource-manager.md).
- Da biste saznali više o značajkama portala za Upravljanje pretplatom, pogledajte odjeljak [Korištenje Azure portal za upravljanje resursima](./azure-portal/resource-group-portal.md).
- Dodatne informacije o primjeni logičke tvrtke ili ustanove resursa, potražite u članku [Korištenje oznake da biste organizirali resurse](resource-group-using-tags.md).
