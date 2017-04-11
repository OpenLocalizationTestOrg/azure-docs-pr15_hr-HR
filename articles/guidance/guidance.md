
<properties
   pageTitle="Upute za Azure | Uzorci i prakse | Microsoft Azure"
   description="Praktični savjeti i upute za Azure"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="christb"/>

# <a name="azure-guidance"></a>Upute za Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Uzorci i prakse tim za Microsoft je dio Azure tima savjeta klijenta. Naš Svrha je pomoć za razvojne inženjere, projektantima i Informatičari biti uspješno na platformi Microsoft Azure. Ne možemo razviti upute koje prikazuje najbolje prakse pri oblaka rješenja na Azure.

## <a name="checklists"></a>Kontrolni popisi

Ti popisi su kratki pregled za pregled temeljne aspektima dostupnosti i skalabilnost. 

- [Kontrolni popis za dostupnosti][AvailabilityChecklist] 

    Sažetak preporučene postupke za osiguravanje dostupnosti.

- [Kontrolni popis za skalabilnost][ScalabilityChecklist]

    Sažetak preporučene postupke za dizajniranje i implementaciju skalabilni servise i zadužen za upravljanje podacima.

## <a name="best-practices-articles"></a>Najbolje postupke članci

Ti članci sadrže detaljan važnih koncepata koji se često povezane s oblaka računalstvo. 

- [Dizajn API-JA][APIDesign] 

    Rasprave probleme dizajna treba uzeti u obzir prilikom dizajniranja web API-JA.

- [Implementacija API-JA][APIImplementation] 

    Skup preporučene postupke za implementaciju i objavljivanje web-mjesto API-JA.

- [Upute o sigurnosti API-JA](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 

    Rasprave provjere autentičnosti i autorizacije opasnosti (, na primjer, tokena vrste, protokola za provjeru autentičnosti, tokova autorizacije i prijetnju ublažiti).

- [Upute za Autoscaling][AutoscalingGuidance] 

    Sažetak pitanja vezana uz skaliranja rješenja bez potrebe za ručno intervencije.

- [Upute za zadatke u pozadini][BackgroundJobsGuidance] 

    Opis dostupnih mogućnosti i preporučene postupke za implementaciju zadatke koje treba izvoditi u pozadini, neovisno o iz bilo koje planu ili interaktivne operacije.

- [Upute za sadržaja isporuke mreže (CDN)][CDNGuidance] 

    Opće smjernice i preporučljivo za korištenje na CDN za minimiziranje opterećenje aplikacija i Maksimizira dostupnosti i performanse.

- [Predmemoriranje upute][CachingGuidance] 

    Sažetak kako koristiti predmemoriju da biste poboljšali performanse i skalabilnost sustava.

- [Upute za Partitioning podataka][DataPartitioningGuidance]

    Strategije koje možete koristiti za particija podataka da biste poboljšali skalabilnost, smanjite Nadmetanje i optimiziranja performansi.

- [Upute za nadzor i dijagnostici][MonitoringandDiagnosticsGuidance] 

    Smjernice o praćenja način na koji korisnici koristiti sustav, praćenje Upotreba resursa i obično praćenje stanja i performanse sustava.

- [Preporučeno konvencije imenovanja][naming-conventions] 

    Preporučuje se konvencije imenovanja za Azure resurse.

- [Ponovite opće smjernice][RetryGeneralGuidance] 

    Rasprave Općenito koncepti za rukovanje tranzitne kvarove.

- [Ponovite upute specifične za servis][RetryServiceSpecificGuidance]

    Sažetak značajki pokušajte ponovno za mnoge Azure servisa, uključujući informacije da biste lakše koristiti, prilagoditi ili proširivanje pokušaj mehanizam za servis.

## <a name="scenario-guides"></a>Scenarij vodilice

- [Elasticsearch sustavom Azure][elasticsearch] 
    
    Elasticsearch je vrlo skalabilni Otvori izvor tražilica i baza podataka. Nije prikladna za situacije koje su potrebne Brza analiza i otkrivanje podataka sadrži velikih skupova podataka. Ovaj vodič razmatra neke ključne aspekte treba uzeti u obzir prilikom dizajniranja programa Elasticsearch klaster.

- [Upravljanje identitetom za složene aplikacije][identity-multitenant] 
    
    Multitenancy je arhitektura u kojem više klijenata za zajedničko korištenje aplikacije, ali su Izolirani međusobno. Ovaj vodič prikazuje način upravljanja identitetima korisnika u složene aplikacije pomoću [Servisa Azure Active Directory] [ AzureAD] učiniti prijave i provjeru autentičnosti.
    
- [Razvoj rješenja velikih skupova podataka](https://msdn.microsoft.com/library/dn749874.aspx)

    Ovaj vodič istražuje korištenje HDInsight scenarije kao što je s iteracijama istraživanje, kao skladištu podataka, za procese ETL i integraciju u postojeće BI sustave. Obuhvaća i smjernice o objašnjenje konceptima velikih skupova podataka, Planiranje i dizajniranje velikih skupova podataka rješenja i implementaciju ova rješenja.
    
## <a name="patterns"></a>Uzorci

- [Oblak dizajn uzorke: Prescriptive arhitektura smjernicama oblaka aplikacije](https://msdn.microsoft.com/library/dn568099.aspx)

    Oblak dizajn uzoraka je biblioteka uzoraka dizajna i upute za povezane teme. Ga articulates prednosti primjene uzoraka tako da prikazuje kako se svakom dijelu stane u oblak aplikacije arhitekturi.
    
- [Optimiziranje performanse for Applications oblaka](https://github.com/mspnp/performance-optimization)

    Ovaj vodič je za istraživanje uobičajene protiv uzorke koje impede aplikacije iz skaliranje opterećenju. Uključuje uzorke takvi osam protiv uzoraka i [primer Analiza uspješnosti](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) i vodič za [assessing performanse u odnosu ključa metriku](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md).

## <a name="reference-architectures"></a>Referenca arhitekturi

Naš arhitekturi reference su razvrstane po scenarij.
Svaki pojedinačne arhitektura nudi preporučeni Načini rada i prescriptive korake i izvršne komponente koje embodies preporuke.

Trenutnom bibliotekom referenca arhitekturi dostupna je na [http://aka.ms/architecture](http://aka.ms/architecture).

## <a name="resiliency-guidance"></a>Upute za otpornost

Ove teme opisuje način dizajniranja aplikacije koje su prebacuju kvara u okruženju raspodijeljeno oblaka.   

- [Pregled stabilnosti][ResiliencyOvervew]

     Kako izraditi aplikacije na Azure platformi koje se mogu vratiti od pogrešaka i nastaviti raditi. U članku se opisuje strukturirane pristup da biste postigli otpornost dizajna za implementaciju, uvođenje i operacije.

- [Kontrolni popis za otpornost][resiliency-checklist]

    Kontrolni popis preporuke koji će vam pomoći plan za različite načine pogreške koji se može pojaviti.

- [Nije uspjelo način analizu][resiliency-fma] 

    Nije uspjelo način analize (FMA) je postupak za otpornost u sustavu, tako da izdvojite moguće pogreška točke. Kao početnu točku za proces FMA ovaj članak sadrži katalog potencijalne pogreške načina i njihovih mitigations. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md

