<properties
    pageTitle="Optimiziranje vaše okruženje s tkanina servis za rješenja u prijava analitiku | Microsoft Azure"
    description="Pomoću servisa tkanina rješenje procijenite rizik i stanje aplikacije servisa tkanina, micro servisa, čvorove i klastere."
    services="log-analytics"
    documentationCenter=""
    authors="niniikhena"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="nini"/>



# <a name="service-fabric-solution-in-log-analytics"></a>Servis Fabric rješenja u zapisnik Analytics

> [AZURE.SELECTOR]
- [Voditelj resursa](log-analytics-service-fabric-azure-resource-manager.md)
- [PowerShell](log-analytics-service-fabric.md)

U ovom se članku opisuje kako pomoću servisa tkanina rješenja u prijava analitiku prepoznavanje i otklanjanje poteškoća s preko svoj klaster tkanina servisa.

Rješenje usluga tkanina prema Prikupljanje ti podaci iz tablica Azure WAD koristi Azure Dijagnostika podatke iz svoje VMs tkanina servisa. Prijava analitiku potom čita događaja framework tkanina servisa, uključujući **Pouzdanog događaja servisa**, **Glumca događaje**, **Radu događaja**i **ETW prilagođene događaje**. S nadzorne ploče rješenje se moći pregledavati najvažnije probleme i važne događaje u svom okruženju tkanina servisa.

Da biste započeli rješenje, morat ćete povezati svoj klaster tkanina servisa u radni prostor zapisnika analize. Ovo su tri scenarija treba uzeti u obzir:

1. Ako ste nije implementiran svoj klaster tkanina servisa, slijedite korake u ***uvođenja klaster tkanina servisa povezani s radnim prostorom prijava analitiku*** implementacije novog klaster i je konfiguriran za izvješće zapisnika analize.

2. Ako vam je potrebna za prikupljanje mjerača performansi na glavno računalo za korištenje druge OMS rješenja kao što je sigurnost na svoj klaster tkanina servisa, slijedite korake u ***uvođenja klaster tkanina servisa povezani s radnim prostorom OMS s nastavkom VM instaliran.***

3. Ako ste već implementiran klaster tkanina servisa i želite povezati s prijava analitiku, slijedite korake u ***Dodavanje postojećeg računa za pohranu prijava analitiku.***


##<a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a>Implementacija klaster tkanina servisa povezani s radnim prostorom zapisnika analize.
Ovaj predložak čini sljedeće:


1. Uvodi se klaster tkanina servisa Azure već povezan s radnim prostorom prijava analitiku. Postoji mogućnost da biste stvorili novi radni prostor pri implementaciji predložak ili naziv već postojeće prijava analitiku radnog prostora za unos.
2. Dodaje račun dijagnostičkih prostora za pohranu u radni prostor zapisnika analize.
3. Omogućuje Fabric servis za rješenja u radnom prostoru zapisnika analize.

[![Implementacija Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)


Kad odaberete gumb za uvođenje, dođete na portalu Azure s parametara za uređivanje. Ne zaboravite da biste stvorili novu grupu resursa ako unos novi naziv radnog prostora prijava analitiku: ![tkanina servisa](./media/log-analytics-service-fabric/2.png)

![Tkanina servisa](./media/log-analytics-service-fabric/3.png)

Prihvatite uvjete za pravne i kliknite "Stvaranje" da biste pokrenuli uvođenje. Kada implementacijskih dovrši, trebali biste vidjeti novog radnog prostora i klaster stvoriti i u WADServiceFabric * događaj, WADWindowsEventLogs i WADETWEvent tablice dodali:

![Tkanina servisa](./media/log-analytics-service-fabric/4.png)

##<a name="deploy-a-service-fabric-cluster-connected-to-an-oms-workspace-with-vm-extension-installed"></a>Uvođenje servisa klaster tkanina povezani s radnim prostorom OMS s nastavkom VM instaliran.
Ovaj predložak čini sljedeće:

1. Uvodi se klaster tkanina servisa Azure već povezan s radnim prostorom prijava analitiku. Možete stvoriti novi radni prostor ili korištenje postojećeg.
2. Zbraja račune dijagnostičkih prostora za pohranu u radni prostor prijava analitiku.
3. Omogućuje Fabric servis za rješenja u radnom prostoru zapisnika analize.
4. Agent za nastavak MMA instalira u svakom skali VM postavljanje u svoj klaster tkanina servisa. Uz instaliran agent MMA se moći pregledavati performanse mjernih podataka o vašem čvorove.


[![Implementacija Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)


Slijedeći iste korake, parametre potrebne za unos, a zatim izbaciti implementacije. I opet trebali biste vidjeti novog radnog prostora, klaster i WAD tablice sve stvoreno:

![Tkanina servisa](./media/log-analytics-service-fabric/5.png)

###<a name="viewing-performance-data"></a>Prikaz podataka o performansama

Da biste pogledali mjerača performansi podataka iz vaše čvorove:
</br>
- Pokretanje analize zapisnika radnog prostora s portala za Azure.

![Tkanina servisa](./media/log-analytics-service-fabric/6.png)

- Idite na postavke u lijevom oknu, a zatim odaberite podatke >> mjerača performansi Windows >> "Dodaj mjerača odabrane performansi": ![tkanina servisa](./media/log-analytics-service-fabric/7.png)

- U zapisniku pretraživanja, pomoću sljedećih upita delve u ključne mjernih podataka o vašem čvorove:
</br>

    na. Usporedba average procesora duž sve čvorove posljednje jedan sat da biste vidjeli koji se čvorovi su riješen i u koje vremenskim razmacima čvor imali u šiljak:

    ``` Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR. ```

    ![Tkanina servisa](./media/log-analytics-service-fabric/10.png)


    b. Pogledajte slične linijski grafikoni za dostupnom memorijom na svakom čvor uz ovaj upit:

    ```Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.```

    Da biste pogledali popis svih čvorove, prikazuje točno prosječnu vrijednost za dostupne MBytes za svaki čvor koristiti taj upit:

    ```Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer ```

    ![Tkanina servisa](./media/log-analytics-service-fabric/11.png)


    c. U slučaju koju želite kroz razine naniže u određenim čvor tako da Provjera svaki sat prosjek, minimalne, maksimalne i 75 percentil procesora uspijevate da biste to učinili pomoću upita (replace polje računalo):

    ```Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR```

    ![Tkanina servisa](./media/log-analytics-service-fabric/12.png)

    Pročitajte dodatne informacije o performansama metriku u prijava analitiku [ovdje]. (https://blogs.technet.microsoft.com/msoms/tag/metrics/)


##<a name="adding-an-existing-storage-account-to-log-analytics"></a>Dodavanje postojećeg računa za pohranu zapisnika Analytics

Ovaj predložak jednostavno zbraja svoje postojeće račune za pohranu u novu ili postojeću prijava analitiku prostor.
</br>

[![Implementacija Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

>[AZURE.NOTE] U odabir grupu resursa ako radite s već postojeće prijava analitiku radni prostor, odaberite "Koristi postojeće" i pretraživanje za grupu resursa koji sadrži OMS radnog prostora. Stvaranje nove jedan ako inače.
![Tkanina servisa](./media/log-analytics-service-fabric/8.png)

Kada je uveden ovaj predložak, moći da biste vidjeli prostor za pohranu račun povezan s prijava analitiku radnog prostora. U ovoj instanci koje dodane jednog računa za više prostora za pohranu u radni prostor sustava Exchange koji sam stvorio iznad.
![Tkanina servisa](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Pregledavali događaje iz servisa tkanina

Kada se dovrše u implementacijama i rješenja usluga tkanina omogućena u radnom prostoru, odaberite pločicu **Tkanina servisa** na portalu prijava analitiku pokrenuti servis tkanina nadzornu ploču. Na nadzornoj ploči obuhvaća stupaca u tablici u nastavku. Svaki se stupac navedene su gornji deset događaja po broj koji odgovaraju kriterijima taj stupac za određeni vremenski raspon. Možete pokrenuti pretraživanje zapisnika koja omogućuje cijeli popis tako da kliknete **Pregled svih** pri dnu desno od svakog stupca ili tako da kliknete zaglavlje stupca.

| **Servis tkanina događaja** | **Opis** |
| --- | --- |
| Najvažnije problema | Prikaz problema kao što su RunAsyncFailures RunAsynCancellations i čvor izbornike. |
| Radu događaja | Najvažnije radu događaje kao što su nadogradnju aplikacije i implementacije. |
| Servis za pouzdan događaja | Najvažnije pouzdanog servisa događaji takve Runasyncinvocations. |
| Glumca događaja | Najvažnije glumca događaji generira micro-servisa, kao što su iznimke izbačena način glumca, glumca aktivacije i deactivations i tako dalje. |
| Događaji aplikacije | Sve prilagođene ETW događaje generira aplikacija. |

![Servis tkanina nadzorne ploče](./media/log-analytics-service-fabric/sf3.png)

![Servis tkanina nadzorne ploče](./media/log-analytics-service-fabric/sf4.png)


Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka za servis tkanina.

| platforme | Izravni Agent | Agent za SCOM | Azure prostora za pohranu | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Windows|![ne](./media/log-analytics-malware/oms-bullet-red.png)|![ne](./media/log-analytics-malware/oms-bullet-red.png)| ![Da](./media/log-analytics-malware/oms-bullet-green.png)|            ![ne](./media/log-analytics-malware/oms-bullet-red.png)|![ne](./media/log-analytics-malware/oms-bullet-red.png)|10 minuta |


>[AZURE.NOTE] Opseg tih događaja u rješenje tkanina servisa možete promijeniti tako da kliknete **podataka na temelju zadnjih 7 dana** pri vrhu nadzorne ploče. Možete prikazati i događaje koje generira pada u zadnjih sedam dana, jedan dan ili šest sati. Ili, možete odabrati **Prilagođeni** da biste odredili u prilagođenom rasponu datuma.


## <a name="next-steps"></a>Daljnji koraci

- Da biste pogledali detaljne podatke za događaj servisa tkanina pomoću [Zapisnika pretraživanja u zapisnik analize](log-analytics-log-searches.md) .
