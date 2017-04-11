<properties 
    pageTitle="Što je timu podataka znanstvenog postupak?  | Microsoft Azure" 
    description="Postupak znanstvenog timu podataka je sustavan način za stvaranje Inteligentna aplikacija pod utjecajem napredne analize." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="bradsev" /> 


# <a name="what-is-the-team-data-science-process-tdsp"></a>Što je timu podataka znanstvenog procesa (TDSP)?

[Postupak znanstvenog timu podataka (TDSP)](data-science-process-overview.md) nudi sustavan pristup za stvaranje Inteligentna aplikacija koja omogućuje timovima fizičari podataka za suradnju učinkovito preko cijelog životni ciklus aktivnosti koje su potrebne da biste uključili te aplikacije u proizvodima. Na TDSP navode niz korake navedene su **upute** o načinu definiranja problema, postavljanje alate i okruženje potrebno analizirati relevantnih podataka, sastavljanje vrednovati predvidljivu modela i implementacija te modelima u aplikacijama. 

Evo nekoliko koraka u **Timu podataka znanstvenog postupak**:  

![KAPICA-tijeka rada](./media/machine-learning-data-science-the-cortana-analytics-process/CAP-workflow.png)

Postupak je **Izračun s iteracijama**: u razumijevanje nove i postojeće i poboljšanja u modelu razvoja zahtijeva reworking korake koji su već obavljeni u nizu. Postojeće tvrtke ili ustanove razvoju i projekta planiranje procesa su **jednostavno prilagođen** za rad s TDSP definirane niz koraka. 

Koraka u postupku se dijagram i povezane [TDSP tečaj](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) i opisane ispod.  

## <a name="preparation-steps"></a>Koraci za pripremu 

## <a name="p1-plan-the-analytics-project"></a>P1. Planiranje projekta analytics 

Pokrenite projekt programa analize definiranjem poslovnim ciljevima i probleme. Su navedeni pomoću **preduvjeti za tvrtke**. Središnje cilj ovaj korak je prepoznavanje varijable ključnim poslovnim (predviđanje prodaje ili vjerojatnost je redoslijed lažno, na primjer) koje analizu treba za predviđanje na način koji zadovoljava te preduvjete. Dodatni planiranje ključan zatim obično za razvoj poznavati **izvore podataka** potrebno da biste riješili Ciljevi projekta iz analytical perspektive. Nije neuobičajeno, na primjer, da biste pronašli da postojeće sustave moraju prikupljanje i dodatne vrste podataka da biste riješiti problem te postigli Ciljevi projekta za prijavu. Upute, potražite u članku [Planiranje okruženja za proces znanstvenog timu podataka](machine-learning-data-science-plan-your-environment.md) i [scenariji za napredne analize u Azure strojnog učenja](machine-learning-data-science-plan-sample-scenarios.md).  

## <a name="p2-setup-analytics-environment"></a>P2. Postavljanje okruženja analytics 

Okruženju analytics za proces timu podataka znanstvenog uključuje nekoliko komponenti: 

- **radni prostori podataka** koje podatke je kopirana bez postavljanja radi analize i Modeliranje, 
- na **obradu infrastrukture** predobradu, istraživanje i Modeliranje podataka
- **infrastruktura za vrijeme izvođenja** operationalize analytical modela i pokretanje aplikacija Inteligentna klijenta koji zauzimaju u modelima.  

Infrastruktura analize potrebno postavljanje često je dio okruženju razlikuje se od core radu sa servisom sustava. No obično upravlja podataka iz više sustava u tvrtki, kao i iz izvora izvan tvrtke. Infrastruktura analize može biti isključivo u oblaku, ili postavljanje programa lokalnog ili hibridnog jednog i drugog. Mogućnosti potražite u članku [Postavljanje okruženja znanstvenog podataka za korištenje u postupku znanstvenog timu podataka](machine-learning-data-science-environment-setup.md).

## <a name="analytics-steps"></a>Analitički korake:  

## <a name="1-ingest-data-into-the-analytical-environment"></a>1. ingest podataka u analytical okruženje 

Prvi korak je dohvaćali relevantnih podataka iz različitih izvora, ili iz unutar iz izvan tvrtke, analitički okruženja koji se gdje se obraditi podatke. **Oblikovanje** podataka izvoru mogu se razlikovati od oblik koji je potreban odredište. Tako da neke transformaciju podataka možda ćete morati poduzeti ingestion tooling. Mogućnosti potražite u članku [učitavanja podataka u okruženju za pohranu za analizu](machine-learning-data-science-ingest-data.md)

Osim početnog ingestion podataka mnoge Inteligentna aplikacije su potrebne za redovito osvježili podatke kao dio je u tijeku učenje postupak. To možete učiniti tako da postavite **podataka kanalu** ili tijeka rada. To čini dio izračun s iteracijama dio postupak koji uključuje novog i ponovno procjene analytical modeli koriste Inteligentna aplikacije implementacija rješenja. Potražite u članku, na primjer, [Premještanje podataka iz lokalnog sustava SQL server u SQL Azure s tvorničke Azure podataka](machine-learning-data-science-move-sql-azure-adf.md).


## <a name="2-explore-and-pre-process-data"></a>2. Istraživanje i unaprijed obrada podataka 

Sljedeći je korak da biste dobili detaljnije razumijevanje podataka po istražuje njegov **Statistika sažetka** odnose, a pomoću tehnika takve **vizualizaciju**. Ovo je i gdje se rukuje problemi **kvalitete podataka** i integritet, kao što su vrijednosti koje nedostaju, neusklađenih vrsta podataka i odnose podataka koje nisu usklađene. Unaprijed obradi pretvaranja koriste se za čišćenje sirovim podacima prije daljnje analize i Modeliranje može obaviti. Opis, potražite u članku [učenje zadaci Priprema podataka za poboljšane računalo](machine-learning-data-science-prepare-data.md).


## <a name="3-develop-features"></a>3. razviti značajke 

Fizičari podataka, u suradnji s stručnjaka za domenu u morate identificirati značajke koje snimite upadljivim svojstva skupa podataka i koje mogu najbolje koristiti za predviđanje varijable ključnim poslovnim prepoznati prilikom planiranja. Sljedeće nove značajke mogu biti izvedena iz postojećih podataka ili može zahtijevati dodatni podaci prikupljeni. Ovaj postupak je poznato kao **značajka inženjering** i jedan je od ključne korake za izgradnju u sustavu učinkovitih predvidljivu analize. Ovaj korak zahtijeva kreativni kombinaciju stručna znanja domene i uvida dobivenog od koraka za istraživanje podataka. Upute, potražite u članku [značajka inženjering u postupku znanstvenog timu podataka](machine-learning-data-science-create-features.md).


## <a name="4-create-predictive-models"></a>4. stvaranje predvidljivu modela 

Fizičari podataka izraditi analytical modela za predviđanje ključnim varijablama otkrije definirano u Planiranje koraka pomoću podataka koje je omogućeno očistiti i featurized preduvjeti za tvrtke. Strojnog učenja sustave podržava više **modeliranja algoritmima pomoću** koje se primjenjuju na velik broj slučajeva. Upute, potražite u članku [kako odabrati algoritama za tim Azure strojnog učenja](machine-learning-algorithm-choice.md).

Fizičari podataka morate odabrati najprikladnije model za predviđanje zadatak, a nije neuobičajeno da rezultata iz većeg broja modela moraju se kombinirati dobiti najbolje rezultate. Unos podataka za Modeliranje obično podijeljen slučajno tri dijela:

- skupu podataka za osposobljavanje 
- skup provjere valjanosti podataka 
- testiranja skupa podataka 

U modelima ugrađenih pomoću **obuka zadani skup podataka**. Optimalnih kombinaciju modeli (pomoću parametara koji je počeo gledati) odabran izvodi u modelima i mjerenje predviđanje pogrešaka za **skup za provjeru valjanosti podataka**. Na kraju **testiranje skup podataka** koristi se za procjenu performanse odabranom modela na nezavisnih podataka koji je korišten za uvježbati ili provjera valjanosti modela.  Postupak potražite u članku [treba vrednovati performansama model u Azure strojnog učenja](machine-learning-evaluate-model-performance.md).


## <a name="5-deploy-and-consume-models"></a>5. implementacije i zauzeti modela 

Kada imamo skup modela koje obavljaju dobro, mogu biti **operationalized** za korištenje drugim aplikacijama. Ovisno o poslovnim zahtjevima predviđanja su izvršene u **stvarnom vremenu** ili na temelju **grupe** . Da biste se operationalized u modelima morate biti izložen programa **otvorite API sučelja** koji se jednostavno potrošena iz različitih aplikacija takve web-mjesto, proračunske tablice, nadzornih ploča ili redak tvrtke i pozadinska aplikacija. U odjeljku [uvođenja u web-servisa Azure strojnog učenja](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="summary-and-next-steps"></a>Sažetak i daljnji koraci

[Timu podataka znanstvenog postupak](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) je katalog modeliran kao niz iterated korake te **uputiti** na zadatke potrebne za napredne analize koristiti da biste sastavili Inteligentna aplikacije. Svaki korak sadrži detalje o korištenju različitih Microsoftove tehnologije za dovršenje zadatka opisane. 

Dok TDSP poštuju određene vrste **dokumentaciju** artefakte, je najbolje dokumenta rezultate Istraživanje podataka, oblikovanje i procjenu, a da biste spremili relevantnih kod tako da možete analizu iterated po potrebi. To omogućuje ponovno korištenje obrade analize prilikom rada na drugim aplikacijama koje su potrebne slične podatke i predviđanje zadatke.

Vodiči za potpuni završetka do kraja kojima se ilustrira svih koraka u postupku **određene scenarijima** za također se navode. U odjeljku, na primjer:

- [Proces znanstvenog timu podataka u akciji: korištenje sustava SQL Server](machine-learning-data-science-process-sql-walkthrough.md)
- [u tim znanstvenog proces podataka u akciji: korištenje klastere HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md).
- [Znanstvena podataka pomoću Spark na Azure HD.mdnsight](machine-learning-data-science-spark-overview.md)
- [Skalabilni znanstvenog podataka u Lake Azure podataka: vodič do završetka do kraja](machine-learning-data-science-process-data-lake-walkthrough.md)

 
