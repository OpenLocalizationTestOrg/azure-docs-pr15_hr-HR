<properties
   pageTitle="Praćenje operacije, događaja i mjerača za opterećenja | Microsoft Azure"
   description="Saznajte kako omogućiti upozorenja događaja i isprobati za Azure opterećenja stanje status zapisivanje"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="log-analytics-for-azure-load-balancer-preview"></a>Zapisnik analize Azure opterećenja (pretpregled)

Različite vrste zapisnike u Azure možete koristiti za upravljanje i otklanjanje poteškoća prilikom učitavanja balancers. Neke od tih zapisnike je moguće pristupiti putem portala sustava. Sve zapisnike možete izdvojiti iz Azure blobova i gledati u raznih alata, kao što su Excel i PowerBI. Dodatne informacije o različitim vrstama zapisnika popisu u nastavku.

- **Zapisnike nadzora:** [Zapisnike nadzora Azure](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) (prijašnjeg radu zapisnika) možete koristiti da biste pogledali sve operacije se šalje na vašem Azure subscription(s) i njihovo stanje. Zapisnike nadzora omogućena je prema zadanim postavkama, a moguće je pregledavati na portalu za Azure.
- **Upozorenje zapisnika događaja:** Ovaj zapisnik možete koristiti da biste pogledali koje upozorenja za raspoređivača opterećenja su potenciju. Status raspoređivača opterećenja koji se prikupljaju svakih pet minuta. Ovaj zapisnik je zapisati samo ako upozorenja događaj raspoređivača učitavanja potenciju.
- **Stanje probni zapisnika:** Ovaj zapisnik možete koristiti da biste provjerili probni stanja Provjera statusa, koliko je instanci su u mreži učitavanja opterećenja pozadinske i postotak virtualnim strojevima primanje mrežnog prometa s raspoređivača opterećenja. Ovaj zapisnik zapisuje probni status događaj promjena.

>[AZURE.IMPORTANT] Prijava analitiku trenutno funkcionira samo za Internet nasuprotne balancers Učitaj. Zapisnici su dostupne samo za resurse u uveden u model implementacije Voditelj resursa. Zapisnici ne možete koristiti za resurse u modelu klasični implementacije. Dodatne informacije o implementaciji modelima potražite u članku [Implementacija upravljanja resursima za razumijevanje i klasični implementacije](../../articles/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Omogućite zapisivanje

Nadzorno zapisivanje automatski omogućena za svaki resurs Voditelj resursa. Morati omogućiti događaj i stanje probni zapisivanje da biste pokrenuli prikupljanja podataka koje su dostupne putem te zapisnika. Poduzmite sljedeće korake da biste omogućili zapisnike.

Prijava na [portal za Azure](http://portal.azure.com). Ako još nemate raspoređivača opterećenja [Stvaranje raspoređivača opterećenja](load-balancer-get-started-internet-arm-ps.md) prije nastavka.

1. Na portalu, kliknite **Pregledaj**.
2. Odaberite **Balancers Učitaj**.

    ![Portal - raspoređivača opterećenja](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Odaberite postojeću opterećenja >> **Sve postavke**.
4. Na desnoj strani dijaloškog okvira pod nazivom raspoređivača opterećenja, pomaknite se do **nadzor**, kliknite **Dijagnostika**.

    ![Portal - učitavanja, opterećenja i postavke](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. U oknu **Dijagnostika** , u odjeljku **Status**odaberite **na**.
6. Kliknite **račun za pohranu**.
7. U odjeljku **ZAPISNIKE**, odaberite postojeći račun za pohranu ili stvorite novi. Pomoću klizača da biste odredili koliko dana događaja dat će biti zadržane u zapisnicima događaja. 8. Kliknite **Spremi**.

    ![Portal - dijagnostički zapisnici](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

>[AZURE.INFORMATION] zapisnike nadzora potreban je račun zasebnom prostora za pohranu. Korištenje prostora za pohranu za događaj i stanje probni zapisivanje plaćati naknade za servis.

## <a name="audit-log"></a>Zapisnik nadzora

Zapisnik nadzora se generira prema zadanim postavkama. U zapisnicima zadržavaju za 90 dana iz trgovine Azure, zapisnika događaja. Saznajte više o ti zapisnici tako da pročitate članak [pregledavali događaje i zapisnika nadzora](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="alert-event-log"></a>Upozorenja zapisnik događaja

Ovaj zapisnik se generira samo ako ste je omogućili na na po temelj raspoređivača opterećenja. Događaji su prijavljeni JSON OSNOVNI oblik i spremljeni na računu za pohranu koji ste naveli prilikom omogućeno zapisivanje. Slijedi primjer događaja.

    {
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }

Izlaz JSON prikazuje svojstva *eventname* koji će se opisuju razlog opterećenja stvorili upozorenje. U ovom slučaju upozorenje koje generira je zbog TCP priključak pojedinačno uzrokovanih izvorni IP NAT ograničenja (SNAT).

## <a name="health-probe-log"></a>Stanje probni zapisnika

Ovaj zapisnik se generira samo ako ste je omogućili na na po učitavanja opterećenja temelj kao detaljne iznad. Račun za pohranu koji ste naveli prilikom omogućeno zapisivanje pohrane podataka. Kontejner 'uvida – zapisnici-loadbalancerprobehealthstatus' se stvara i prijavljen je sljedeći podaci:

    {
        "records":
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 1,
                "healthPercentage": 50.000000
            }
        },
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 0,
                "healthPercentage": 100.000000
            }
        }
    }

Izlaz JSON prikazuje u polju svojstva osnovne informacije za status probni stanja. Svojstvo *dipDownCount* prikazuje ukupni broj instanci pozadinske koje primate mrežni promet zbog nije uspjelo probni odgovore.

## <a name="view-and-analyze-the-audit-log"></a>Prikaz i analiza zapisnika nadzora

Možete pogledati i analizirati podaci iz zapisnika nadzora na bilo koji od sljedećih načina:

- **Azure Alati:** Dohvaćanje informacija iz zapisnika nadzora kroz Azure PowerShell, naredbenim sučeljem linijski Azure (EŽA), Azure REST API-JA ili portala za Azure pretpregled. Detaljne upute za svaki način detaljno opisane u članku [nadzora operacije s Voditelj resursa](../../articles/resource-group-audit.md) .
- **Power BI:** Ako već nemate račun za [Power BI](https://powerbi.microsoft.com/pricing) , možete ga isprobati besplatno. Korištenje [zapisnika nadzora Azure sadržaja paket za Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), možete analizirati podatke u unaprijed konfigurirana nadzorne ploče ili prilagodite prikaze u skladu s potrebama.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Prikaz i analizirajte stanje probni i zapisnika događaja

Morate povezati s računom za pohranu i dohvaćanje stavke evidencije JSON za događaj i stanje probni zapisnika. Kada preuzmete JSON datoteke, možete ih pretvoriti u CSV i prikaz u programu Excel, PowerBI ili drugi alat vizualizaciju podataka.

>[AZURE.TIP] Ako ste upoznati s Visual Studio i osnovni koncepti promjene vrijednosti za konstante i varijable u C#, možete koristiti [zapisnika pretvornik Alati](https://github.com/Azure-Samples/networking-dotnet-log-converter) dostupni Github.

## <a name="additional-resources"></a>Dodatni resursi

- Objava na blogu [Vizualiziraj Azure nadzornih zapisnika s dodatkom Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Prikaz i analiza Azure zapisnika nadzora u Power BI i još mnogo toga](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) objava na blogu.

## <a name="next-steps"></a>Daljnji koraci

[Razumijevanje probes raspoređivača učitavanja](load-balancer-custom-probe-overview.md)
