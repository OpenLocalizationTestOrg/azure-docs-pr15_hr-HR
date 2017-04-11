<properties
  pageTitle="Azure IoT paket najčešća pitanja vezana uz | Microsoft Azure"
  description="Najčešća pitanja o IoT paketa"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="09/26/2016"
  ms.author="araguila"/>
   
# <a name="frequently-asked-questions-for-iot-suite"></a>Najčešća pitanja o IoT paketa

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Koja je razlika između brisanje grupu resursa na portalu za Azure i kliknite Izbriši na prethodno rješenja u azureiotsuite.com?

- Ako izbrišete konfiguriranog rješenja u [azureiotsuite.com][lnk-azureiotsuite], brisanje svih resursa koji su tamo stvaranja konfiguriranog rješenje; Ako ste dodali dodatne resurse u grupu resursa, i oni se brišu. 

- Ako izbrišete grupu resursa za [Azure portal][lnk-azure-portal], izbrisati samo resursa u toj grupi resursa; Također morat ćete izbrisati pridružena konfiguriranog rješenja [Azure klasični portal]aplikacija Azure Active Directory[lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Koliko je instanci IoT koncentratora možete Dodjela u pretplatu? 

Deset. Možete stvoriti programa [Azure podržava karata] [ link-azuresupportticket] da biste podigli to ograničenje, ali po zadanom koje možete samo Dodjela deset koncentratora IoT po pretplati, kao što je vidljivo [Azure pretplate ograničenja][link-azuresublimits]. Kao rezultat jer se svaki konfiguriranog rješenje dodjeljuje novi IoT koncentrator, možete samo Dodjela najviše deset konfiguriranog rješenja u određenom pretplate. 

### <a name="how-many-documentdb-instances-can-i-provision-in-a-subscription"></a>Koliko je instanci DocumentDB možete Dodjela u pretplatu?

50. Možete stvoriti programa [Azure podržava karata] [ link-azuresupportticket] da biste podigli to ograničenje, ali po zadanom možete samo Dodjela 50 DocumentDB instanci po pretplate. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Koliko besplatne Bing karte API-ji možete Dodjela u pretplatu?

Dva. Možete stvoriti samo dva Interna transakcije razine 1 Bing karte za tarife Enterprise Azure pretplate. Udaljenom nadzora rješenje dodjeli je po zadanom s tarifom interne transakcije razine 1. Zbog toga možete samo Dodjela do dva udaljene nadzora rješenja u pretplatu s bez izmjene.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Imam udaljene nadzora implementacija rješenja s statičnu kartu, kako dodati interaktivnu kartu servisa Bing? 
1. Pribavljanje Bing karte API-JA za Enterprise QueryKey s [portala za Azure][lnk-azure-portal]: 
 1. Idite u grupu resursa gdje je vaša Bing karte API-JA za Enterprise [Azure portal][lnk-azure-portal].
 2. Kliknite sve postavke, zatim Key Management. 
 3. Primijetit ćete dvije tipke: glavnog ključa i QueryKey. Kopirajte vrijednost za QueryKey.

     > [AZURE.NOTE] Nemate i API Bing karte za Enterprise račun? Stvorili [Azure portal] [ lnk-azure-portal] po kliknite + Novo, pretraživanjem API-jem Bing karte za Enterprise i slijedite upute da biste stvorili.

2. Padajući najnovije kod iz [Azure IoT – alat za analizu daljinske-nadgledanje][lnk-remote-monitoring-github].

3. Pokrenite na lokalni ili cloud implementacije uputama s naredbenog retka implementacije u mapi /docs/ u spremištu. 

4. Nakon što prođete na lokalni ili cloud implementaciju, pogledajte u korijenskoj mapi za na *. user.config datoteku stvorili tijekom implementacije. Otvorite datoteku u uređivaču teksta. 

5. Promijenite sljedeći redak vrijednosti koju ste kopirali iz sustava QueryKey: 
   
  `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Je li moguće stvoriti konfiguriranog rješenje ako imate Microsoft Azure za DreamSpark?
Trenutno nije moguće stvoriti konfiguriranog rješenja s [Microsoft Azure za DreamSpark] [ lnk-dreamspark] računa. Međutim, možete stvoriti [besplatnu probnu račun za Azure] [ lnk-30daytrial] u samo nekoliko minuta koji će se omogućiti stvaranje konfiguriranog rješenja.

### <a name="how-do-i-delete-an-aad-tenant"></a>Kako izbrisati klijent AAD?

Pročitajte [vodič brisanja klijent za Azure AD]za objavu na blogu Eric Golpe[lnk-delete-aad-tennant].

## <a name="next-steps"></a>Daljnji koraci

Možete istraživati i druge značajke i mogućnosti IoT paket unaprijed konfigurirane rješenja:

- [Pregled predvidljivu održavanja unaprijed konfigurirane rješenja][lnk-predictive-overview]
- [Sigurnost IoT od dna prema gore][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
