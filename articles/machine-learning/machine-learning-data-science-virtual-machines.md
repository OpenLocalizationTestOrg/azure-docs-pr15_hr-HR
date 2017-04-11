<properties
    pageTitle="Podataka znanstvenog virtualnog računala u Azure | Microsoft Azure"
    description="Postavljanje prema gore na podataka znanstvenog virtualnog računala"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="data-science-virtual-machines-in-azure"></a>Podataka znanstvenog virtualnog računala u Azure

Upute su navedene ovdje opisuju kako postaviti programa Azure VM i programa Azure VM sa servisom SQL kao poslužitelje IPython bilježnice. Windows virtualnog računala je konfiguriran za korištenje alata kao što su IPython bilježnice, Explorer Azure prostora za pohranu i AzCopy kao i drugi uslužni programi koji se koriste za projekte znanstvenog podataka za podršku. Azure Explorer prostora za pohranu i AzCopy, na primjer, omogućuju praktičan načina za prijenos podataka za pohranu za Azure s lokalnog računala ili da biste preuzeli na lokalno računalo sa servisa za pohranu. 

U ovom izborniku navode veze na temu koja opisuje kako postaviti različitim okruženjima znanstvenog podataka koristi [Proces znanstvenog timu podataka (TDSP)](data-science-process-overview.md).

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Nekoliko vrsta Azure virtualnim strojevima možete dodjeli i konfigurirano tako da se koristi kao dio okruženju Znanstvena oblaku podataka. Odluka o koje flavor virtualnog računala da biste koristili ovisi o vrsti i količinu podataka katalog modeliran s strojnog učenja i ciljnog odredišta za tih podataka u oblaku. 

* Smjernice o pitanja treba uzeti u obzir pri stvaranju tu odluku potražite u članku [Planiranje vaše Azure strojno učenje podataka znanstvenog okruženju](machine-learning-data-science-plan-your-environment.md). 
* Katalog nekih scenarija koji se mogu pojaviti pri izvođenju naprednom analitikom, potražite u članku [scenariji za napredne analize postupak i tehnologije Azure strojnog učenja](machine-learning-data-science-plan-sample-scenarios.md)

Dostupna je dva skupa upute:

* [Postavljanje Azure virtualnog računala kao bilježnicu IPython poslužitelj za napredne analize](machine-learning-data-science-setup-virtual-machine.md) objašnjava Dodjela Azure virtualnog računala s IPython bilježnice i druge alate za obavljali znanstvenog podataka za slučaj da se u kojem obliku Azure pohranu osim SQL može koristiti radi pohrane podataka.

* [Postavljanje sustava SQL Server Azure virtualnog računala kao bilježnicu IPython poslužitelj za napredne analize](machine-learning-data-science-setup-sql-server-virtual-machine.md) pokazuje kako Dodjela sustava Azure SQL Server virtualnog računala s IPython bilježnice i druge alate za obavljali podataka znanosti za slučajeve u kojima SQL baze podataka može koristiti radi pohrane podataka.

Kada dodijeljenu i konfiguriran te virtualnim strojevima su jeste li spremni za korištenje kao bilježnicu IPython poslužitelji za istraživanje i obrada podataka i druge zadatke potrebne u kombinaciji s Azure strojnog učenja i timu podataka znanstvenog procesa (TDSP). Daljnji koraci u postupku znanstvenog podataka mapiraju se u [TDSP tečaj](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) , a može sadržavati korake koji premještanje podataka sustava SQL Server ili HDInsight, obradu i je li uzorak u Priprema za učenje iz podataka s Azure strojnog učenja.


> [AZURE.NOTE] Azure virtualnim strojevima su cjenovnom kao **platiti samo koje koristite**. Da biste bili sigurni da vam se ne koji se naplaćuju kada ne koristite virtualnog računala, on mora biti u stanju **zaustavljen (Deallocated)** s [Azure klasični Portal](http://manage.windowsazure.com/). Detaljne upute ili kako deallocate virtualnog računala, pročitajte odjeljak [zatvaranja i deallocate virtualnog računala kada nije u upotrebi](machine-learning-data-science-setup-virtual-machine.md#shutdown)
 
