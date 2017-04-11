<properties 
    pageTitle="Korištenje predmeta – kupca Profiliranje" 
    description="Saznajte korištenja tvorničke Azure podataka da biste stvorili utemeljenih na podacima tijek rada (kanala) profila igraće klijentima." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="use-case---customer-profiling"></a>Korištenje predmeta – kupca Profiliranje

Azure tvorničke podataka jedan je od mnoge servise za implementaciju paket obavještavanje Cortana ubrzivača rješenja.  Dodatne informacije o Cortana obavještavanje, posjetite [Cortana obavještavanje paket](http://www.microsoft.com/cortanaanalytics). U ovom dokumentu, ne možemo opisuju jednostavne koristi slučaj da vam pomoći da započnete s objašnjenje kako Azure podataka tvorničke rješavanje uobičajenih problema analize.

Sve što trebate pristupiti i isprobajte slučaj jednostavne koristi je [Azure pretplate](https://azure.microsoft.com/pricing/free-trial/).  Možete implementirati uzorka koji implementira slučaj koristi slijedeći korake opisane u članku [uzorka](data-factory-samples.md) .

## <a name="scenario"></a>Scenarij

Contoso je igraće tvrtke koje stvara igre za više platforme: igara konzole, prijenosno uređaja i računala (PC-ja). Kao što je reproduktora reproducirali te igre, velik broj podaci iz zapisnika je sastavio koja prati uzoraka korištenja, igraće stil i postavke korisnika.  Kada u kombinaciji s Demografski, regionalne i podaci o proizvodu, Contoso izvršava analitiku u vodič kroz o poboljšavaju igrača cilj im o nadogradnjama i u utakmica kupi. 

Contoso-cilj je prepoznavanje gore-pošiljke/unakrsno-pošiljke prilike na temelju povijest igraće njegov reproduktora i dodavanje atraktivnog značajki na pogon tvrtke growth i omogućuju bolje iskustvo klijentima. Za taj slučaj koristi kao primjer poslovanja koristimo igraće tvrtke. Tvrtka želi da biste optimizirali njegov igre na temelju igrača ponašanje. Tih načela primjenjuju se na sve poslovne koji želi sudjelovati svojih klijenata oko svojih proizvoda i usluga i poboljšavaju svoje kupce.

## <a name="challenges"></a>Izazove


## <a name="solution-overview"></a>Pregled rješenja

Taj slučaj jednostavne korištenje može se koristiti kao primjer kako koristiti tvorničke podataka Azure ingest, Priprema, pretvorba, analizirati i objavljivanje podataka.

![Kraj do kraja tijeka rada](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Slika prikazuje kanali podataka prikaza na portalu za Azure kada ih uvesti.

1.  **PartitionGameLogsPipeline** čita neobrađenog igraće događaje iz spremišta blobova i stvara particije na temelju godinu, mjesec i dan.
2.  **EnrichGameLogsPipeline** spaja particioniranom igre događaji s zemlj kod referentnih podataka i obogaćuje podatke mapiranjem IP adrese na odgovarajuće mjesto zemlj..
3.  Kanal **AnalyzeMarketingCampaignPipeline** koristi enriched podatke i procesa s podacima oglašavanje da biste stvorili konačni izlaz koji sadrži učinkovitosti marketinških kampanja.

U ovom primjeru tvorničke podataka koristi se za orkestrirali aktivnosti koje kopiranje ulaznih podataka, transformacija i proces podatke i konačni podatke s bazom podataka SQL Azure.  Možete i vizualizacija mreže podataka kanali, njima upravljati i nadzirati status u korisničkom Sučelju.

## <a name="benefits"></a>Prednosti

Optimiziranje njihove analize korisničkog profila i poravnavanje s poslovnim ciljevima, igraće tvrtke je moći brzo prikupiti uzoraka korištenja i analizirajte učinkovitost njegov marketinških kampanja.




