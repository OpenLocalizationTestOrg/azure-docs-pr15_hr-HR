<properties 
   pageTitle="Praćenje zapisnika programa access i performanse i metrike za pristupnik za aplikaciju | Microsoft Azure"
   description="Saznajte kako omogućiti i upravljati zapisnika programa Access i performanse za pristupnik za aplikaciju"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="amitsriva" />

# <a name="diagnostics-logging-and-metrics-for-application-gateway"></a>Dijagnostičkog zapisnika i metriku za pristupnik za aplikaciju

Azure pruža mogućnost praćenje resursa s zapisivanje i mjerenja

[**Bilježenje u zapisnik**](#enable-logging-with-powershell) - zapisivanje omogućuje performanse, access i druge zapisnike da biste spremili ili potrošena iz resursa za potrebe za nadzor.

[**Metriku**](#metrics) – trenutno pristupnika aplikacije sadrži jednu metriku. Ova metrika mjeri propusnost pristupnik za aplikacije u bajtovima sekundi.

Upravljanje i otklanjanje poteškoća s računala pristupnika možete koristiti različite vrste zapisnike u Azure. Neke od tih zapisnike je moguće pristupiti putem portala sustava, a sve zapisnika se možete dobivenih iz Azure blobova, i prikazati u raznih alata, kao što je [Prijava analitiku](../log-analytics/log-analytics-azure-networking-analytics.md), Excel i PowerBI. Možete saznati više o različitim vrstama zapisnika s popisa u nastavku:

- **Zapisnike nadzora:** [Zapisnike nadzora Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (prijašnjeg radu zapisnika) možete koristiti da biste pogledali sve operacije se šalje na pretplatu Azure i njihovo stanje. Zapisnike nadzora omogućena je prema zadanim postavkama, a moguće je pregledavati na portalu za Azure pretpregled.
- **Pristup zapisnika:** Da biste pogledali aplikacije pristupnika pristup uzorak i analizirati važne informacije o pozivatelju IP, URL koji ste tražili, Latencija odgovor, uključujući vraćanje kod bajtova i smanjivati možete koristiti ovaj zapisnik. Pristup zapisnika prikupljaju se svakih 300 sekundi. Ovaj zapisnik sadrži jedan zapis po instanci pristupnika aplikacije. Aplikacija instanci pristupnika možete otkrije svojstvo "ID instance".
- **Zapisnike performansi:** Da biste vidjeli kako su izvođenje aplikacije instance pristupnika možete koristiti ovaj zapisnik. U ovom zapisniku pohranjuju podaci o performansama po instancu pregledava zahtjev za Ukupno poslužena, uključujući propusnost u bajtovima, poslužena ukupni zahtjeve, count zahtjeva, dobar i dobro pozadinske instance broj. Zapisnik performansi prikupljaju se svakih 60 sekundi.
- **Vatrozida zapisnika:** Ovaj zapisnik možete koristiti da biste vidjeli zahtjeve koji su prijavljeni putem otkrivanja i sprječavanje način pristupnika za aplikacije koji je konfiguriran s vatrozidom aplikaciju za web.

>[AZURE.WARNING] Zapisnici su dostupne samo za resurse u uveden u model implementacije Voditelj resursa. Zapisnici ne možete koristiti za resurse u modelu klasični implementacije. Za bolje razumjeli dva modela, referencu u članku [Implementacija upravljanja resursima za razumijevanje i klasični implementacije](../resource-manager-deployment-model.md) .

## <a name="enable-logging-with-powershell"></a>Omogućite zapisivanje sa servisom PowerShell

Nadzorno zapisivanje automatski omogućena za svaki resurs Voditelj resursa. Morate omogućiti pristup i performanse zapisivanje da biste pokrenuli prikupljanja podataka koje su dostupne putem te zapisnika. Da biste omogućili zapisivanje, pogledajte sljedeće korake: 

1. Imajte na umu ID resursa za pohranu računa, pohranjuju podaci zapisnika. Na ovom bi obrasca: /subscriptions/\<subscriptionId\>/resourceGroups/\<naziv grupe resursa\>/providers/Microsoft.Storage/storageAccounts/\<naziv računa spremišta\>. Može se koristiti bilo koji račun za pohranu za pretplatu. Da biste pronašli ove informacije možete koristiti portal za pretpregled.

    ![Portal za pretpregled - Dijagnostika pristupnika aplikacije](./media/application-gateway-diagnostics/diagnostics1.png)

2. Imajte na umu aplikacije pristupnik ID resursa za koje je omogućiti. Na ovom bi obrasca: /subscriptions/\<subscriptionId\>/resourceGroups/\<naziv grupe resursa\>/providers/Microsoft.Network/applicationGateways/\<naziv pristupnika aplikacije\>. Da biste pronašli ove informacije možete koristiti portal za pretpregled.

    ![Portal za pretpregled - Dijagnostika pristupnik aplikacije](./media/application-gateway-diagnostics/diagnostics2.png)

3. Omogućite zapisivanje Dijagnostika pomoću cmdleta komponente powershell sustava sljedeće:

        Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true  

>[AZURE.INFORMATION] zapisnike nadzora potreban je račun zasebnom prostor za pohranu. Korištenje prostora za pohranu za pristup i performanse zapisivanje uključuje naknade za servis.

## <a name="enable-logging-with-azure-portal"></a>Uključite zapisivanje podataka pomoću portala za Azure

### <a name="step-1"></a>Korak 1

Dođite do svoje resursa na portalu za Azure. Kliknite **dijagnostičke zapisnike**. Ako je ovo prvi put konfiguriranje dijagnostike sustava plohu izgleda kao na sljedećoj slici:

Za pristupnik za aplikaciju, dostupne su 3 zapisnika.

- Pristup zapisniku
- Zapisnik performansi
- Zapisnik vatrozid

Kliknite **Uključi dijagnostiku** da biste pokrenuli prikupljanja podataka.

![Dijagnostika postavka plohu][1]

### <a name="step-2"></a>Korak 2

Na plohu **postavki dijagnostike** postavke za dijagnostički zapisnici postavljene. U ovom primjeru prijava analitiku služi za pohranu zapisnike. Kliknite **Konfiguriraj** u odjeljku **Prijava analitiku** da biste konfigurirali radnog prostora. Koncentratora za događaj i pohranu račun može koristiti da biste spremili zapisnike Dijagnostika.

![Dijagnostika plohu][2]

### <a name="step-3"></a>Korak 3

Odaberite postojeći radni prostor OMS ili stvorite novi. U ovom primjeru koristi se postojećeg.

![radni prostori OMS][3]

### <a name="step-4"></a>Korak 4

Po dovršetku potvrdite postavke, a zatim kliknite **Spremi** da biste spremili postavke.

![potvrdili odabir][4]

## <a name="audit-log"></a>Zapisnik nadzora

Azure generira ovaj zapisnik (prijašnjeg "radu zapisnika") prema zadanim postavkama.  U zapisnicima zadržavaju za 90 dana iz trgovine Azure, zapisnika događaja. Saznajte više o ti zapisnici tako da pročitate članak [pregledavali događaje i zapisnika nadzora](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="access-log"></a>Pristup zapisniku

Ovaj zapisnik se generira samo ako ste je omogućili na na po aplikacije pristupnika temelj kao detaljne u prethodne korake. Račun za pohranu koji ste naveli prilikom omogućeno zapisivanje pohrane podataka. Svaki pristup aplikaciji pristupnik prijavljen je na JSON OSNOVNI oblik, kao što se vidi u sljedećem primjeru:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayAccess",
        "time": "2016-04-11T04:24:37Z",
        "category": "ApplicationGatewayAccessLog",
        "properties": {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIP":"37.186.113.170",
            "clientPort":"12345",
            "httpMethod":"HEAD",
            "requestUri":"/xyz/portal",
            "requestQuery":"",
            "userAgent":"-",
            "httpStatus":"200",
            "httpVersion":"HTTP/1.0",
            "receivedBytes":"27",
            "sentBytes":"202",
            "timeTaken":"359",
            "sslEnabled":"off"
        }
    }

## <a name="performance-log"></a>Zapisnik performansi

Ovaj zapisnik se generira samo ako ste je omogućili na na po aplikacije pristupnika temelj kao detaljne u prethodne korake. Račun za pohranu koji ste naveli prilikom omogućeno zapisivanje pohrane podataka. Prijavljen je sljedeći podaci:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayPerformance",
        "time": "2016-04-09T00:00:00Z",
        "category": "ApplicationGatewayPerformanceLog",
        "properties": 
        {
            "instanceId":"ApplicationGatewayRole_IN_1",
            "healthyHostCount":"4",
            "unHealthyHostCount":"0",
            "requestCount":"185",
            "latency":"0",
            "failedRequestCount":"0",
            "throughput":"119427"
        }
    }


## <a name="firewall-log"></a>Zapisnik vatrozid

Ovaj zapisnik se generira samo ako ste je omogućili na na po aplikacije pristupnika temelj kao detaljne u prethodne korake. Ovaj zapisnik zahtijeva i vatrozid web aplikacije se konfigurirati na pristupnik za aplikacije. Račun za pohranu koji ste naveli prilikom omogućeno zapisivanje pohrane podataka. Prijavljen je sljedeći podaci:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
        "operationName": "ApplicationGatewayFirewall",
        "time": "2016-09-20T00:40:04.9138513Z",
        "category": "ApplicationGatewayFirewallLog",
        "properties":     {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIp":"108.41.16.164",
            "clientPort":1815,
            "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
            "ruleId":"OWASP_973336",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "action":"Logged",
            "site":"Global",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
    }

## <a name="view-and-analyze-the-audit-log"></a>Prikaz i analiza zapisnika nadzora

Možete pogledati i analizirati podaci iz zapisnika nadzora na bilo koji od sljedećih načina:

- **Azure Alati:** Dohvaćanje informacija iz zapisnika nadzora kroz Azure PowerShell, naredbenim sučeljem linijski Azure (EŽA), Azure REST API-JA ili portala za Azure pretpregled.  Detaljne upute za svaki način detaljno opisane u članku [nadzora operacije s Voditelj resursa](../resource-group-audit.md) .
- **Power BI:** Ako već nemate račun za [Power BI](https://powerbi.microsoft.com/pricing) , možete ga isprobati besplatno. Korištenje [zapisnika nadzora Azure sadržaja paket za Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) možete analizirati podatke s unaprijed konfigurirana nadzornih ploča koje možete koristiti kao-je i prilagodba.

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>Prikaz i analiza zapisnika programa access, performanse i vatrozid

Azure [Prijava analitiku](../log-analytics/log-analytics-azure-networking-analytics.md) možete prikupiti brojač zapisnik događaja datoteke i s računa za spremište blobova platforme te sadrži vizualizacije i mogućnosti napredna pretraživanja da biste analizirali svoje zapisnika.

Povezivanje s računom za pohranu i dohvaćanje stavaka evidencije JSON zapisnika programa access i performanse. Kada preuzmete JSON datoteke, možete ih pretvoriti u CSV i prikaz u programu Excel, PowerBI ili drugi alat vizualizaciju podataka.

>[AZURE.TIP] Ako ste upoznati s Visual Studio i osnovni koncepti promjene vrijednosti za konstante i varijable u C#, možete koristiti [zapisnika pretvornik Alati](https://github.com/Azure-Samples/networking-dotnet-log-converter) dostupni Github.

## <a name="metrics"></a>Mjerenja

Metriku je značajka za određene Azure resurse gdje možete pregledati mjerača performansi na portalu. Za pristupnik za aplikaciju, jedan metriku dostupna prilikom pisanja u ovom se članku. Ova metrika je propusnost, a mogu vidjeti na portalu. Dođite do pristupnik za aplikaciju, a zatim kliknite **metriku**.  Odaberite propusnost u odjeljku **dostupni metriku** da biste prikazali vrijednosti. Na sljedećoj slici, vidjet ćete primjer s filtrima koje je moguće koristiti za prikaz podataka u rasponima različitih vremenskih.

Da biste vidjeli popis trenutnog podržava metriku, posjetite [podržani metriku s monitora Azure](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![Prikaz mjerenja][5]

## <a name="alert-rules"></a>Upozorenja pravila

Upozorenja pravila može se pokrenuti od na temelju metrike na resurs. To znači za pristupnik za aplikaciju upozorenja možete poziva na webhook ili e-pošte administrator ako propusnost pristupnik za aplikaciju je iznad, ispod ili pri praga u određeno vrijeme.

U sljedećem primjeru će vas voditi kroz stvaranje upozorenja pravilo koje šalje poruku e-pošte administrator kada je omogućeno breached propusnost praga.

### <a name="step-1"></a>Korak 1

Kliknite **Dodaj metričkim upozorenja** da biste pokrenuli. U ovom plohu također mogu pristupiti plohu metriku.

![plohu upozorenja pravila][6]

### <a name="step-2"></a>Korak 2

U plohu **Dodaj pravilo** ispunjavanje uvjeta, ime i obavijesti sekcije, a zatim kliknite **u redu** kada to učinite.

Birač **uvjet** omogućuje 4 vrijednosti, **veća od**, **veće od ili jednako**, **manja od**ili **manje od ili jednako**.

Birač **razdoblje** omogućuje za izdvajanje razdoblje od pet minuta do 6 sati.

Tako da odaberete **vlasnici e-pošte, suradnika, i čitači** e-pošte može biti dinamički koji se temelji na korisnike koji imaju pristup resursa. U suprotnom odvojenih zarezom popisa korisnika se može pružati u tekstni okvir **email(s) dodatnih administratora** .

![Dodavanje pravila plohu][7]

Ako je breached prag, poruke e-pošte stigne slično onome na sljedećoj slici:

![Prag breached e-pošte][8]

Kad metričkim upozorenje stvorena i sadrži pregled upozorenja pravila prikazuje se popis upozorenja.

![Prikaz upozorenja pravila][9]

Da biste saznali više o upozorenja, posjetite [primanje upozorenja](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Da biste bolje webhooks te kako ih možete koristiti s upozorenja shvatili, posjetite [Konfiguriraj webhook na Azure metričkim upozorenja](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>Daljnji koraci

- Vizualizacija brojač i zapisnike događaja s [Zapisnika Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) 
- Objava na blogu [Vizualiziraj Azure nadzornih zapisnika s dodatkom Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Prikaz i analiza Azure zapisnika nadzora u Power BI i još mnogo toga](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) objava na blogu.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png