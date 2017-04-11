<properties
   pageTitle="U oblaku Cruiser i Microsoft Azure Integracija API-JA za naplatu | Microsoft Azure"
   description="Pruža jedinstvenu perspektivu iz Microsoft Azure naplata partnera Cruiser oblak na svoja iskustva integriranje Azure naplata API-ji u svoje proizvoda.  To je posebno korisno za Azure i oblaka Cruiser korisnike koji žele pomoću/korištenja probne verzije Cruiser oblaka za paket Microsoft Azure."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"
   />

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="09/08/2016"
   ms.author="mobandyo;sirishap;bryanla"/>

# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>U oblaku Cruiser i Microsoft Azure Integracija API-JA za naplatu

U ovom se članku opisuje kako podaci prikupljeni s novi Microsoft Azure naplata API-ji moguće je koristiti u oblak Cruiser za simulacije trošak tijeka rada i analize.

## <a name="azure-ratecard-api"></a>Azure RateCard API-JA
RateCard API pruža stopa informacije iz Azure. Nakon provjere autentičnosti s početnim vjerodajnice, upit možete poslati API-JA za prikupljanje metapodataka o usluge dostupne na Azure, zajedno s tečajevima povezan s vašeg nude ID-a.

Slijedi uzorka odgovor API-JA s cijene za na A0 instancu (Windows):

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Cloud Cruiser na sučelje za Azure RateCard API-JA
Oblak Cruiser omogućuje korištenje informacija RateCard API na različite načine. Za ovaj članak Pokazat ćemo kako ga možete koristiti da biste IaaS radno opterećenje cijena simulacije i analize.

Da bismo pokazali slučaj koristi, zamislite radno opterećenje više instanci koje su pokrenute na Microsoft Azure Service Pack (WAP). Cilj je kao zamjenu za ovaj isti radno opterećenje na Azure i procjenu troškova način takve migracije. Da biste stvorili ovaj simulacije, postoje dvije glavni zadaci koje je potrebno izvršiti:

1. **Uvoz i proces podatke o usluzi prikupljenih RateCard API-JA.** Ovaj zadatak i izvršiti na radne knjige, gdje izdvojiti iz RateCard API transformacije i objaviti u novoj tarifi stopa. U ovom novoj tarifi stopa koristit će se simulations za procjenu Azure cijene.

2. **Normalizacija WAP i Azure services za IaaS.** Prema zadanim postavkama servisa WAP temelje se na pojedinačnih resursa (CPU memorije, veličina diska, itd.) tijekom Azure services temelje se na instancu veličina (A0 A1, A2, itd.). Prvi zadatak možete izvoditi oblaka Cruiser ETL modul, naziva radnih knjiga, gdje se ove resurse se objediniti na instancu veličine, analogna servisa Azure instance.

### <a name="import-data-from-the-ratecard-api"></a>Uvoz podataka iz RateCard API-JA

Radne knjige Cruiser oblaka pružaju automatizirano za prikupljanje i obrada informacije iz RateCard API-JA.  Radne knjige (Izdvoji-pretvorbe-učitavanja) ETL omogućuju konfiguriranje zbirke, transformaciju i objavljivanje podataka u Oblaku Cruiser bazu podataka.

Svakoj radnoj knjizi možete imati jednu ili više zbirki, što omogućuje povezivanje podataka iz različitih izvora nadopunjuju ili dodavanje podataka o korištenju. Sljedeća dva snimke zaslona pokazati kako stvoriti novu *zbirku* u postojeću radnu knjigu i uvoz informacija u *zbirci* RateCard API-JA:

![Slika 1 – stvaranje nove zbirke][1]

![Slika 2 - uvoz podataka iz nove zbirke][2]

Nakon uvoza podataka u radnoj knjizi, moguće je da biste stvorili više koraka i transformacije procesa, izmjena i Modeliranje podataka. U ovom primjeru jer smo zanima samo infrastrukture-kao-na-Service (IaaS) koristimo transformaciju korake da biste uklonili nepotrebne retke ili zapisa, odnose se na servisima osim IaaS.

Sljedeće snimka zaslona prikazuje transformaciju korake za obradu podataka prikupljenih RateCard API-JA:

![Slika 3 – transformaciju korake za obradu prikupljene podatke iz RateCard API-JA][3]

### <a name="defining-new-services-and-rate-plans"></a>Definiranje nove servise i stopa tarife

Da biste definirali servise u Oblaku Cruiser različitih načina. Jednu od mogućnosti je da biste uvezli usluge iz podataka o korištenju. Ta metoda obično se koristi prilikom rada s javnim oblaka, koje su usluge već definirani davatelj.

Planiranje stopa je skup stope ili cijene koja se može primijeniti različite servise koji se temelji na stvarni datumi ili grupa klijenata ostaloga. Cijenama može se koristiti na Oblaku Cruiser da biste stvorili simulacije ili scenariji "Što ako", da biste razumjeli kako promjene Services utječu na ukupni trošak radno opterećenje.

U ovom primjeru koristit ćemo podatke o usluzi iz RateCard API da biste definirali nove servise u Oblaku Cruiser. Na isti način koristimo stope povezane sa servisima da biste stvorili novi stopa planiranje Cruiser oblaka.

Na kraju postupka transformaciju je moguće stvaranje novog koraka i objava podataka iz RateCard API kao novih servisa i prijenosa.

![Slika 4 - objavljivanje podataka iz RateCard API kao nove servise i tečajeve][4]

### <a name="verify-azure-services-and-rates"></a>Provjerite je li servisa Azure i stopama.

Nakon objavljivanja servisa i tečajeve, možete provjeriti na popisu uvezene usluge u Oblaku Cruiser karticu *Servisi* :

![Slika 5 - provjera novih servisa][5]

Na kartici *Stopa tarife* možete provjeriti novoj tarifi tečaj pod nazivom "AzureSimulation" s tečajevima uvezeni iz RateCard API-JA.

![Slika 6 – Provjera novi Plan stopa i pridruženi stope][6]

### <a name="normalize-wap-and-azure-services"></a>Normalizacija WAP i servisa Azure

Prema zadanim postavkama WAP omogućuje korištenje informacije koje se temelje na računalnim, memorije i mrežni resursi. U Cruiser oblak, možete definirati servisa temelji izravno na dodijeljeni ili s ograničenim prometom korištenje ove resurse. Na primjer, postaviti osnovni stopa za svaki sat procesora ili trošak GB memorije instancu.

Na primjer, da biste usporedili troškove između WAP i Azure, moramo aggregate korištenje resursa na WAP u objedinjuje koja se zatim mogu mapirati Azure servisima. Ovu transformaciju možete jednostavno implementirana u radnim knjigama:

![Slika 7 – pretvaranje podataka WAP koju treba normalizirati services][7]

Posljednji korak po radnoj knjizi je za objavljivanje podataka u Oblaku Cruiser bazu podataka. Tijekom ovaj korak podataka o korištenju je sada vezanoj instalaciji u usluge (koje mapiranje servisa Azure) i uz zadane stope da biste stvorili naknada.

Nakon dovršetka radnu knjigu, možete automatizirati obrada podataka, dodavanje zadatka na alat za zakazivanje i navođenjem učestalost i vrijeme za radnu knjigu koju želite pokrenuti.

![Slika 8 - zakazivanje radne knjige][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Stvaranje izvješća za radno opterećenje troškova simulacije analizu

Kada korištenje prikupljaju i troškove učitavaju se u oblak Cruiser bazu podataka, ne možemo na raspolaganju su modul oblaka Cruiser uvida da biste stvorili radno opterećenje cijena simulacije koju smo žele.

Da bi se demonstrirati scenarij koju smo stvorili Sljedeća izvješća:

![Usporedba troškova][9]

Gornjem grafikonu prikazuju usporedbu trošak servisima, Usporedba cijenu izvodi radno opterećenje za svaki servis određene između WAP (tamnoplava) i Azure (Svijetloplava).

Dolje na grafikonu se prikazuju iste podatke, ali neispravnu prema dolje po odjelu. Ovo prikazuje troškove za svaki odjel pokrenuti svoje radno opterećenje WAP i Azure, uz razliku između njih u traci računanje (zelena).

## <a name="azure-usage-api"></a>Azure korištenja API-JA


### <a name="introduction"></a>Uvod

Microsoft nedavno uvedena korištenja API Azure dopuštanja pretplatnike programski dohvaćanje podataka o korištenju da bi se dobio uvida u svoje potrošnje. Ovo je sjajno novosti za kupce Cruiser oblak koji možete iskoristiti bogatiji dataset dostupne putem ovaj API-JA.

Oblak Cruiser omogućuje korištenje integracije s korištenja API-JA na nekoliko načina. Granularnosti (zaračunava informacije o korištenju) i resursa metapodacima dostupne putem na API sadrži skup podataka potrebne za podršku fleksibilne Showback ili Chargeback modela. 

U ovom ćete praktičnom vodiču smo prikazati jedan primjer kako su Cruiser oblaka prednosti korištenja API informacije. Točnije, ne možemo će stvoriti grupu resursa na Azure, pridružiti oznake za strukturu računa, a zatim opisuje postupak izvlačenja i obrada podataka oznaka u oblak Cruiser.
 
Konačni cilj je omogućiti stvaranje izvješća poput sljedećih i moći ćete analizirati trošak i utrošak na temelju strukturu račun popunjena oznake.

![Slika 10 - izvješće s raščlambe pomoću oznaka][10]

### <a name="microsoft-azure-tags"></a>Oznake Microsoft Azure

Podaci koji su dostupni putem korištenja API Azure obuhvaća potrošnje informacije, ali i metapodataka resursa obuhvaća sve oznake pridružen. Oznake pružaju jednostavan način da biste organizirali resursa, ali da bi se snagu, morate provjerite:

- Oznake pravilno primjenjuju se na resurse trenutku dodjele resursa
- Oznake ispravno koriste na postupka Showback/Chargeback sve korištenje strukturu račun u tvrtki ili ustanovi.

Oba tim preduvjetima može biti zahtjevne, osobito kada postoji ručnog procesa u dodjele resursa ili puni strani. Pogrešno upisana, pogrešne ili čak i nedostaje oznaka su uobičajeni pritužbe klijenata kada pomoću oznaka i te pogreške možete napraviti život na strani punjenja vrlo teško.

Novi način korištenja API Azure Cruiser oblaka možete povući informacija o označavanju resursa i putem sofisticirane ETL alatu naziva radnih knjiga, ih ispraviti uobičajene označavanje. Putem transformacije pomoću Regularni izrazi i korelacije podataka, oblaka Cruiser možete pronađite ispravno označenog i primijeniti odgovarajuće oznake osiguravanje točne pridruživanje resursa s korisnik.

Na strani punjenja oblaka Cruiser automatizira postupak Showback/Chargeback, a možete koristiti informacije Označi sve korištenje da biste odgovarajuće potrošača (odjel dijeljenja, Project, itd.). U ovom Automatizacija nudi velikog unaprjeđivanja i možete biti sigurni da dosljedno i koje je moguće nadzirati punjenja procesa.
 

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Stvaranje grupe resursa pomoću oznaka na Microsoft Azure
Prvi korak ovog praktičnog vodiča je da biste stvorili grupu resursa na portalu Azure stvaranje nove oznake za pridruživanje resursa. Primjerice, ne možemo hoće li stvarati sljedeće oznake: projekta odjelu okruženju, vlasnik.

Snimka zaslona koja se nalazi ispod prikazuje uzorak grupa resursa pridružene oznakama.

![Slika 11 – grupa resursa pridružene oznakama portala za Azure][11]

Sljedeći je korak za izvlačenje podataka s API-JA za korištenje u Oblaku Cruiser. Korištenje API trenutno nudi odgovora u JSON OSNOVNI oblik. Evo primjera dohvatiti podatke:


    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Uvoz podataka iz API-JA za korištenje u Oblaku Cruiser

Radne knjige Cruiser oblaka pružaju automatizirano za prikupljanje i obrada informacije iz korištenja API-JA. Radne knjige programa (Izdvoji-Pretvorba-učitavanja) ETL omogućuje konfiguriranje zbirke, transformaciju i objavljivanje podataka u Oblaku Cruiser bazu podataka.

Svakoj radnoj knjizi možete imati jednu ili više zbirki. Omogućuje povezivanje podataka iz različitih izvora nadopunjuju ili dodavanje podataka o korištenju. U ovom primjeru ćemo će stvoriti novi list u radnoj knjizi predloška Azure (_UsageAPI)_ i postavite novu _zbirku_ da biste uvezli podatke iz korištenja API-JA.

![Slika 3 – korištenje API nakon uvoza podataka u UsageAPI lista][12]

Imajte na umu da ova radna knjiga već imaju druge listove za uvoz servisa Azure (_ImportServices_) i obradili potrošnje podatke iz API naplata (_PublishData_).

Sljedeće će pomoću korištenja API-JA za ispunjavanje lista _UsageAPI_ i povezivanje podataka potrošnje podacima iz API naplata na listu _PublishData_ .

### <a name="processing-the-tag-information-from-the-usage-api"></a>Obrada informacija o oznaku iz korištenja API-JA

Nakon uvoza podataka u radnoj knjizi, ne možemo će stvoriti transformaciju korake na listu _UsageAPI_ da bi se obradili podatke iz API Sučelja. Prvi je korak da biste koristili procesor "JSON Podjela" da biste izdvojili oznake iz jednog polja, a zatim stvorite polja za svaku od njih (odjela, Project, vlasnika i okruženja).

![Slika 4 – stvaranje novog polja informacije oznaka][13]

Obratite pozornost na servis "Povezivanje s mrežom" nema oznake podataka (žuti okvir), ali ne možemo možete provjeriti tako da pogledate polje _ResourceGroupName_ je dio istoj grupi resursa. Budući da imamo oznake za ostale resurse iz ovu grupu resursa, koristimo te informacije da biste primijenili nedostaje oznaka ovaj resurs kasnije tijekom postupka.

Sljedeći je korak da biste stvorili pridruživanje informacije s oznakama _ResourceGroupName_tablica za traženje. Tablica za traženje će se na sljedeći korak obogatiti potrošnje podacima oznaka.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>Dodavanje informacija oznake podataka potrošnje

Sada ćemo prijeći na listu _PublishData_ koja obrađuje potrošnju podatke iz API naplata i dodajte polja dobivenih iz oznake. Ovaj postupak provodi tako da pogledate tablica za traženje stvorili u prethodnom koraku pomoću _ResourceGroupName_ kao ključ za u pretraživanjima.

![Slika 5 - puniti strukturu račun s podacima u pretraživanja][14]

Imajte na umu da polja strukturu odgovarajući račun za servis "Povezivanje s mrežom" zatvorene, rješavanje problema s nedostaje oznaka. Ne možemo i popunjavaju polja strukturu računa za resurse osim naš cilja grupu resursa sa "Ostalo" da biste ih razlikovali na izvješćima.

Sada ćemo samo želite dodati korak za objavljivanje podataka o korištenju. Tijekom ovaj korak odgovarajuće stope za svaki servis definiran na našem planiranje stopa će se primijeniti na informacije o korištenju s rezultirajućem trošak učitati u bazu podataka.

Najbolji dio je da imate samo jednom prođite kroz postupak. Po dovršetku radnu knjigu morate samo da biste ga dodali na raspored i funkcionirat će hourly ili dnevnu zakazano vrijeme. Tada je samo pitanje stvaranje novih izvješća ili Prilagodba postojeće, da bi se analizirati podatke da biste smisleni uvida s Upotreba oblaka.

### <a name="next-steps"></a>Daljnji koraci

+ Detaljne upute za stvaranje radnih knjiga Cruiser oblaka i izvješća, pogledajte oblaka Cruiser mrežne [dokumentacije](http://docs.cloudcruiser.com/) (potrebna je valjan prijava).  Dodatne informacije o Cruiser oblaka, obratite se [info@cloudcruiser.com](mailto:info@cloudcruiser.com).
+ Potražite u članku [rast uvida u sustava Microsoft Azure resursa potrošnje](billing-usage-rate-card-overview.md) pregled korištenja resursa Azure i API-ji RateCard.
+ Pogledajte dodatne informacije o oba API koje su dio skupa API-ji dobili Azure resursa Upravitelj [Azure naplata REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) .
+ Ako želite da biste prikazali desno uzorak koda, pogledajte naše Microsoft Azure naplata API primjere koda na [Primjere koda Azure](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>uči više
+ Potražite u članku [Pregled upravljanja resursima Azure](azure-resource-manager/resource-group-overview.md) da biste saznali više o Voditelj resursa Azure.

<!--Image references-->
 
[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Slika 1 – stvaranje nove zbirke"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Slika 2 - uvoz podataka iz nove zbirke"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Slika 3 – transformaciju korake za obradu prikupljene podatke iz RateCard API-JA"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Slika 4 - objavljivanje podataka iz RateCard API kao nove servise i tečajeve"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Slika 5 - provjera novih servisa"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Slika 6 – Provjera novi Plan stopa i pridruženi stope"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Slika 7 – pretvaranje podataka WAP koju treba normalizirati services"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Slika 8 - zakazivanje radne knjige"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Slika 9 – oglednog izvješća scenarija usporedbe radno opterećenje trošak"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Slika 10 - izvješće s raščlambe pomoću oznaka"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Slika 11 – grupa resursa pridružene oznakama portala za Azure"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Slika 12 - uvezeni u list UsageAPI podataka o korištenju API-JA"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Slika 13 – stvaranje novog polja informacije oznaka"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Slika 14 - puniti strukturu račun s podacima u pretraživanja"
