<properties
   pageTitle="Praćenje operacije, događaja i mjerača za NSGs | Microsoft Azure"
   description="Saznajte kako omogućiti mjerača, događaja i zapisivanje radu sa servisom za NSGs"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/14/2016"
   ms.author="jdial" />

#<a name="log-analytics-for-network-security-groups-nsgs"></a>Zapisnik analize mreže sigurnosnih grupa (NSGs)

Različite vrste zapisnike u Azure možete koristiti za upravljanje i rješavanje problema s NSGs. Neke od tih zapisnike je moguće pristupiti putem portala sustava, a sve zapisnika se možete dobivenih iz Azure blobova, i prikazati u raznih alata, kao što je [Prijava analitiku](../log-analytics/log-analytics-azure-networking-analytics.md), Excel i PowerBI. Dodatne informacije o različitim vrstama zapisnika popisu u nastavku.

- **Zapisnike nadzora:** [Zapisnike nadzora Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (prijašnjeg radu zapisnika) možete koristiti da biste pogledali sve operacije se šalje na vašem Azure subscription(s) i njihovo stanje. Zapisnike nadzora omogućena je prema zadanim postavkama, a moguće je pregledavati na portalu za Azure pretpregled.
- **Zapisnika događaja:** Da biste pogledali koje NSG pravila primjenjuju se na VMs i instancu uloge na osnovi MAC adresa možete koristiti ovaj zapisnik. Status ta pravila prikupljaju se svakih 60 sekundi.
- **Brojač zapisnika:** Da biste pogledali koliko je puta svako pravilo NSG primijenjen zabranu ili dopušta promet možete koristiti ovaj zapisnik.

>[AZURE.WARNING] Zapisnici su dostupne samo za resurse u uveden u model implementacije Voditelj resursa. Nije moguće koristiti zapisnici resursa u model klasični implementacije. Za bolje razumjeli dva modela, referencu u članku [Implementacija upravljanja resursima za razumijevanje i klasični implementacije](../resource-manager-deployment-model.md) .

##<a name="enable-logging"></a>Omogućite zapisivanje
Nadzorno zapisivanje je automatski omogućena na cijelo vrijeme za svaki resurs Voditelj resursa. Morate omogućiti događaj i brojač zapisivanje da biste pokrenuli prikupljanja podataka koje su dostupne putem te zapisnika. Da biste omogućili zapisnike, slijedite korake u nastavku.

1.  Prijava na [portal za Azure](https://portal.azure.com). Ako još nemate postojeće mreže sigurnosnu grupu sustava, [stvorite je NSG](virtual-networks-create-nsg-arm-ps.md) prije nastavka.

2.  Na portalu za pretpregled kliknite **Pregledaj** >> **mreže sigurnosne grupe**.

    ![Portal za pretpregled - mreže sigurnosne grupe](./media/virtual-network-nsg-manage-log/portal-enable1.png)

3. Odaberite postojeće mreže sigurnosne grupe.

    ![Portal za pretpregled - mreže sigurnosne grupe postavke](./media/virtual-network-nsg-manage-log/portal-enable2.png)

4. U plohu **Postavke** kliknite **Dijagnostika**, pa u oknu **Dijagnostika** , pokraj **Status** **na**
5. U plohu **Postavke** kliknite **Račun za pohranu**i odaberite postojeći prostora za pohranu računa ili stvorite novi.  

>[AZURE.INFORMATION] zapisnike nadzora potreban je račun zasebnom prostora za pohranu. Korištenje prostora za pohranu za događaja i zapisivanje pravilo će uzrokovati naknade za servis.

6. U padajućem popisu tik ispod **Račun za pohranu**, odaberite želite li prijaviti događaje, mjerača ili oboje, a zatim kliknite **Spremi**.

    ![Portal za pretpregled - dijagnostički zapisnici](./media/virtual-network-nsg-manage-log/portal-enable3.png)

## <a name="audit-log"></a>Zapisnik nadzora
Azure generira ovaj zapisnik (prijašnjeg "radu zapisnika") prema zadanim postavkama.  U zapisnicima zadržavaju za 90 dana iz trgovine Azure, zapisnika događaja. Saznajte više o ti zapisnici tako da pročitate članak [pregledavali događaje i zapisnika nadzora](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="counter-log"></a>Brojač zapisnika
Ovaj zapisnik generira se samo ako ste je omogućili na temelju po NSG kao detaljne iznad. Račun za pohranu koji ste naveli prilikom omogućeno zapisivanje pohrane podataka. Svako pravilo primjenjuje se na resurse prijavljen je na JSON OSNOVNI oblik, kao što se vidi ispod.

    {
        "time": "2015-09-11T23:14:22.6940000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupRuleCounter",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupCounters",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"DenyAllOutBound",
            "direction":"Out",
            "type":"block",
            "matchedConnections":0
            }
    }

## <a name="event-log"></a>Zapisnik događaja
Ovaj zapisnik generira se samo ako ste je omogućili na temelju po NSG kao detaljne iznad. Račun za pohranu koji ste naveli prilikom omogućeno zapisivanje pohrane podataka. Prijavljen je sljedeći podaci:

    {
        "time": "2015-09-11T23:05:22.6860000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupEvent",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupEvents",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"AllowVnetOutBound",
            "direction":"Out",
            "priority":65000,
            "type":"allow",
            "conditions":{
                "destinationPortRange":"0-65535",
                "sourcePortRange":"0-65535",
                "destinationIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32",
                "sourceIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32"
            }
        }
    }

## <a name="view-and-analyze-the-audit-log"></a>Prikaz i analiza zapisnika nadzora
Možete pogledati i analizirati podaci iz zapisnika nadzora na bilo koji od sljedećih načina:

- **Azure Alati:** Dohvaćanje informacija iz zapisnika nadzora kroz Azure PowerShell, naredbenim sučeljem linijski Azure (EŽA), Azure REST API-JA ili portala za Azure pretpregled.  Detaljne upute za svaki način detaljno opisane u članku [nadzora operacije s Voditelj resursa](../resource-group-audit.md) .
- **Power BI:** Ako već nemate račun za [Power BI](https://powerbi.microsoft.com/pricing) , možete ga isprobati besplatno. Korištenje [zapisnika nadzora Azure sadržaja paket za Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/) možete analizirati podatke s unaprijed konfigurirana nadzornih ploča koje možete koristiti kao-je i prilagodba.

## <a name="view-and-analyze-the-counter-and-event-log"></a>Prikaz i analiza brojač i zapisnika događaja

Azure [Prijava analitiku](../log-analytics/log-analytics-azure-networking-analytics.md) možete prikupiti brojač zapisnik događaja datoteke i s računa za spremište blobova platforme te sadrži vizualizacije i mogućnosti napredna pretraživanja da biste analizirali svoje zapisnika.

Povezivanje s računom za pohranu i dohvaćanje stavke evidencije JSON za događaj i brojač zapisnika. Kada preuzmete JSON datoteke, možete ih pretvoriti u CSV i prikaz u programu Excel, PowerBI ili drugi alat vizualizaciju podataka.

>[AZURE.TIP] Ako ste upoznati s Visual Studio i osnovni koncepti promjene vrijednosti za konstante i varijable u C#, možete koristiti [zapisnika pretvornik Alati](https://github.com/Azure-Samples/networking-dotnet-log-converter) dostupni Github.

## <a name="next-steps"></a>Daljnji koraci

- Vizualizacija brojač i zapisnike događaja s [Zapisnika Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)
- Objava na blogu [Vizualiziraj Azure nadzornih zapisnika s dodatkom Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Prikaz i analiza Azure zapisnika nadzora u Power BI i još mnogo toga](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) objava na blogu.
