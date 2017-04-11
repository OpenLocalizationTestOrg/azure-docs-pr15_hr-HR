<properties
   pageTitle="Rješenja u paketu za upravljanje operacije (OMS) | Microsoft Azure"
   description="Rješenja proširiti mogućnosti funkcije od operacije upravljanja paket (OMS) unosom scenarija zapakirani upravljanja koje korisnici mogu dodavati njihove OMS radnog prostora.  Ovaj članak sadrži detalje na kako Prilagođena rješenja koja su stvorile klijentima i partnerima."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="management-solutions-in-operations-management-suite-oms-preview"></a>Rješenja za upravljanje u servis operacije upravljanja paket (OMS) (pretpregled)

>[AZURE.NOTE]Ovo je prva verzija dokumentacije za rješenja za upravljanje OMS koje su trenutno u pretpregledu.    

Rješenja za upravljanje proširiti mogućnosti funkcije od operacije upravljanja paket (OMS) unosom scenarija zapakirani upravljanja koje korisnici mogu dodati svoje radno okruženje.  Osim [rješenja pruža Microsoft](../log-analytics/log-analytics-add-solutions.md), partnerima i klijentima možete stvoriti rješenja koja će se koristiti u svoje okruženje za upravljanje ili učinili dostupnima korisnicima putem zajednice.

## <a name="finding-and-installing-management-solutions"></a>Pronalaženje i instaliranje rješenja za upravljanje
Za pronalaženje i instaliranje rješenja za upravljanje kao što je opisano u sljedećim odjeljcima na više načina.

### <a name="azure-marketplace"></a>Azure Marketplace
Rješenja za upravljanje omogućuje tvrtka Microsoft i pouzdanih partnere možda instaliran iz trgovine Azure na portalu za Azure.

1. Prijavite se na portal sustava Azure.
2. U lijevom oknu odaberite **više usluga**.
3. Pomaknite se do **rješenja** ili upišite *rješenja* u dijaloškom okviru **Filtar** .
4. Kliknite gumb **Dodaj +** .
5. Traženje rješenja koja vas zanima ili pregledavanjem, kliknite gumb **Filtar** i upisivanje u okvir za **Pretraživanje Everthing** .
6. Kliknite stavku marketplace da biste pogledali detaljne informacije.
4. Kliknite **Stvori** da biste otvorili okno **Dodavanje rješenja** .
5. Morat će biti potrebne informacije, primjerice [OMS radnog prostora i račun za automatizaciju](#oms-workspace-and-automation-account) osim vrijednosti za parametre u rješenje.
6. Kliknite **Stvori** da biste instalirali rješenje.

### <a name="oms-portal"></a>OMS Portal
Rješenja za upravljanje pruža Microsoft mogu instalirati iz galerije rješenja na portalu OMS.

1. Prijavite se na OMS portal.
2. Kliknite pločicu **Galerije rješenja** .
2. Na stranici galerije rješenja OMS Informirajte se o svakom dostupno rješenje. Kliknite naziv koji želite dodati OMS rješenja.
3. Detaljne informacije o rješenje prikazuju se na stranici za rješenje koje ste odabrali. Kliknite **Dodaj**.
4. Novi pločicu za rješenje koje ste dodali na pregled stranice u OMS, a možete ga počnete koristiti kada OMS usluga obrađuje podataka će se prikazati.

### <a name="azure-quickstart-templates"></a>Predlošci Azure brzi početak rada
Članovi zajednice mogu slati rješenja za upravljanje predlošcima Azure brzi početak rada.  Možete preuzeti tih predložaka za kasnije instalaciju ili provjera ih da biste saznali kako [stvoriti vlastiti rješenja](#creating-a-solution).

1. Poduzmite korake opisane u [radni prostor OMS i automatizaciju računa](#oms-workspace-and-automation-account) da biste se povezali radnog prostora i račun.
2. Idite na [predloške Azure brzi početak rada](https://azure.microsoft.com/documentation/templates/).  
3. Traženje rješenja koja vas zanima.
4. Odaberite rješenje iz rezultata da biste prikazali detalje o.
5. Kliknite gumb **uvođenja za Azure** .
6. Zatražit će se da biste dobili informacije kao što su grupe resursa i mjesto osim vrijednosti za parametre u rješenje.
7. Kliknite **kupnju** za instaliranje rješenja.

### <a name="deploy-azure-resource-manager-template"></a>Uvođenje predloška Azure Voditelj resursa
Rješenja da se iz zajednice ili taj [sami stvoriti](#creating-a-solution) primjenjuju kao predloška Voditelj resursa, a možete koristiti bilo koji standardni načina za [uvođenje predloška](../resource-group-template-deploy-portal.md).  Imajte na umu da prije instalacije rješenje, morate stvoriti i veza na [web-mjesto radnog prostora OMS i u okvir za automatizaciju računa](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>Radni prostor OMS i račun za automatizaciju
Većina rješenja za upravljanje potreban [prostor OMS](../log-analytics/log-analytics-manage-access.md) sadrži prikaze i [račun za automatizaciju](../automation/automation-security-overview.md#automation-account-overview) runbooks i povezani resursi. Radni prostor i račun mora zadovoljiti sljedeće preduvjete.

- Rješenje možete koristiti samo jedan radni prostor OMS i jednog računa za automatizaciju.  
- Radni prostor OMS i automatizaciju račun koji se koristi rješenje moraju biti povezane jedan drugome. Radni prostor programa OMS je možda povezan samo s jednog računa za automatizaciju i račun za automatizaciju samo je moguće povezati s jednog radnog prostora OMS.
- Koji su povezani OMS radnog prostora i račun za automatizaciju mora biti u istom grupa resursa i regije.  Izuzetak je OMS radnog prostora u regiji Istočni SAD-a i i račun za automatizaciju u Istočni sad 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Stvaranje veze između radnog prostora OMS i račun za automatizaciju
Kako odrediti OMS radnog prostora i račun za automatizaciju ovisi o način instalacije za rješenje.

- Kada instalirate Microsoft rješenje putem portala za OMS, je instaliran na trenutni radni prostor OMS i potreban je račun nema automatizaciju.

- Kada instalirate rješenje putem trgovine Windows Azure, Zatraži OMS radnog prostora i automatizaciju račun, a veze između njih će se stvoriti.  

- Za rješenja izvan trgovine Windows Azure, morate povezati OMS radnog prostora i račun za automatizaciju prije instalacije rješenja.  To možete učiniti tako da odaberete rješenjem u trgovine Windows Azure, a zatim odaberete OMS radnog prostora i račun za automatizaciju.  Ne morate zapravo instalirati rješenje jer vezu stvorit će se čim odabrana OMS radnog prostora i račun za automatizaciju.  Nakon što vezu je, a zatim pomoću tog radnog prostora OMS i automatizaciju račun za svakog rješenja. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>Provjera vezu između radnog prostora OMS i račun za automatizaciju
Provjerite vezu između web-mjesto radnog prostora programa OMS i u okvir za automatizaciju račun pomoću sljedećeg postupka.

1. Odaberite račun za automatizaciju na portalu za Azure.
2. Pomaknite se na dnu okna **Postavke** .
3. Ako postoji sekcije naziva **OMS resursi** u oknu **Postavke** , taj račun pridružen s radnim prostorom OMS.
4. Odaberite **radnog prostora** da biste vidjeli detalje o radnom prostoru OMS povezan s ovim računom automatizaciju.


## <a name="listing-management-solutions"></a>Rješenja za upravljanje stavku
Pomoću sljedećeg postupka za da biste pogledali rješenja za upravljanje u radnim prostorima povezane s Azure pretplatu.

1. Prijavite se na portal sustava Azure.
2. U lijevom oknu odaberite **više usluga**.
3. Pomaknite se do **rješenja** ili upišite *rješenja* u dijaloškom okviru **Filtar** .
4. Rješenja instaliran u radnim prostorima će se prikazati.

Imajte na umu da možete vidjeti samo Microsoft solutions instaliran u trenutnom radnom prostoru pomoću portala za OMS.

## <a name="removing-a-management-solution"></a>Uklanjanje rješenje za upravljanje
Kada je rješenje za upravljanje Ukloni, sve resursa u rješenje također se uklanjaju.  

1. Pronađite rješenje na portalu Azure pomoću postupka u [popis rješenja](#listing-solutions).
2. Odaberite rješenje koje želite ukloniti.
3. Kliknite gumb **Izbriši** .

## <a name="creating-a-management-solution"></a>Stvaranje rješenja upravljanja
Dovršavanje upute o stvaranju rješenja za upravljanje dostupne su na [Stvaranje rješenja u paketu operacije upravljanja (OMS)](operations-management-suite-solutions-creating.md). 


## <a name="next-steps"></a>Daljnji koraci

- Pretraživanje [Predložaka za brzi početak rada Azure](https://azure.microsoft.com/documentation/templates) uzorka različite predloške Voditelj resursa.
- Stvaranje vlastite [rješenja za upravljanje](operations-management-suite-solutions-creating.md).
 