<properties
   pageTitle="Stvaranje pravila utemeljen na put za pristupnik za aplikaciju pomoću portala za | Microsoft Azure"
   description="Saznajte kako stvoriti pravilo utemeljen na put za pristupnik za aplikaciju pomoću portala"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Stvaranje pravila utemeljen na put za pristupnik za aplikaciju pomoću portala

> [AZURE.SELECTOR]
- [Portal za Azure](application-gateway-create-url-route-portal.md)
- [Azure PowerShell Voditelj resursa](application-gateway-create-url-route-arm-ps.md)

URL-a koji se temelji na put do usmjeravanje omogućuje usmjerava na temelju put URL-a Http zahtjev za pridruživanje. Provjerava postoji li usmjeravanje pozadinske resurse konfiguriran za popise URL-a aplikacije pristupnika i slanje mrežni promet na definirani pozadinske resurse. Često se koristi za usmjeravanje utemeljen na URL je da biste učitali zahtjeva za različite vrste sadržaja za drugi poslužitelj pozadinske grupe.

Koji se temelji na URL usmjeravanje predstavlja nove vrste pravila za pristupnik za aplikacije. Pristupnik za aplikacije sadrži dvije vrste pravila: Osnovno i put pravila. Vrste osnovni pravila omogućuje kružnog uslugu za grupe pozadinske tijekom pravila temeljena na put osim kružnog raspodjele, također uzima uzorak put URL-a zahtjev u obzir pri odabiru skup pozadinskog.

## <a name="scenario"></a>Scenarij

Sljedeći scenarij prolazi kroz stvaranje pravila za utemeljen na put u postojeće pristupnik za aplikacije.
Scenarij pretpostavlja da ste već pratili korake da biste [stvorili pristupnik za aplikacije](application-gateway-create-gateway-portal.md).

![Usmjeravanje URL-a][scenario]

## <a name="createrule"></a>Stvaranje pravila utemeljen na put

Pravilo utemeljen na put zahtijeva vlastitu ga slušatelj, prije stvaranje pravila biti sigurni da biste provjerili imate dostupne ga slušatelj da biste koristili.

### <a name="step-1"></a>Korak 1

Dođite do http://portal.azure.com, a zatim odaberite postojeći pristupnik za aplikacije. Kliknite **pravila**

![Pregled aplikacija pristupnika][1]

### <a name="step-2"></a>Korak 2

Kliknite gumb **koji se temelji na put –** da biste dodali novo pravilo utemeljen na put.

### <a name="step-3"></a>Korak 3

**Dodaj pravilo utemeljen na put** plohu ima dvije sekcije. Prvi dio je gdje ste definirali ga slušatelj, naziv pravila i zadane postavke put. Zadane postavke put su usmjerava koji ulaze u prilagođeni put sustavom usmjeravanje. Drugi dio plohu **Dodaj pravilo utemeljen na put** je gdje definirati pravila temeljena na put sami.

**Osnovne postavke**

- **Naziv** – to je neslužbeni naziv pravilo koje se može pristupiti na portalu.
- **Ga slušatelj** – to je da ga slušatelj koji se koristi za pravilo.
- **Zadani pozadinskog skup** - Ova postavka nije postavku koja definira pozadinske će se koristiti za zadano pravilo
- **HTTP zadane postavke** - Ova postavka je postavka koja definira HTTP-a postavke će se koristiti za zadano pravilo.

**Pravila temeljena na put**

- **Naziv** – to je neslužbeni naziv koji se temelji na put do pravilo.
- **Putovi** - Ova postavka definira put pravilo će izgledati za prosljeđivanje promet
- **Skup pozadinskog** - Ova postavka nije postavku koja definira pozadinske će se koristiti za pravila
- **Postavka HTTP** - Ova postavka je postavka koja definira postavke HTTP koje se koriste za pravilo.

>[AZURE.IMPORTANT] Putovi: Popis uzoraka put tako da odgovara. Svaki moraju započeti znakom / i samo stavi na "\*" je dopušteno je na kraju. Valjani Primjeri su /xyz, /xyz* ili /xyz/*.  

![Dodavanje plohu utemeljen na put pravila s informacijama ispunjenom tablicom][2]

Dodavanje pravila koji se temelji na put do postojeće pristupnik za aplikaciju je lako proces putem portala sustava. Nakon što pravilo utemeljen na putu, mogu se uređivati jednostavno dodati dodatna pravila. 

![Dodavanje dodatnih pravila temeljena na put][3]

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali kako konfigurirati SSL rasteretite s pristupnika Azure aplikacije potražite u članku [Konfiguriranje SSL Offload](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png