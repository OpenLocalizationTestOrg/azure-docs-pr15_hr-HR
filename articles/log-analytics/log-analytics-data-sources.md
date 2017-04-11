<properties 
   pageTitle="Izvori podataka u prijava analitiku | Microsoft Azure"
   description="Izvori podataka definirati da povezani prijava analitiku prikuplja iz agenata i drugih izvora podataka.  U ovom se članku opisuju pojam kako prijava analitiku koristi izvore podataka u članku se objašnjava pojedinosti o tome kako ih konfigurirali i daje sažetak različitih vrsta izvora koji je dostupan."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="data-sources-in-log-analytics"></a>Izvori podataka u zapisniku analize

Prijava analitiku prikuplja podatke iz izvora povezani u radnom prostoru OMS i sprema u spremištu OMS.  Podaci koji se prikupe iz svake definira izvori podataka koje možete konfigurirati.  Podaci u spremištu OMS pohranjuju se kao skup zapisa.  Svaki izvor podataka stvara zapise određene vrste sa svakom vrstom pojavljuju vlastiti skup svojstava.

![Prijava analitiku prikupljanje podataka](./media/log-analytics-data-sources/overview.png)

Izvori podataka razlikuju se od OMS rješenja koja prikupljanje podataka iz povezanih izvora i stvoriti zapise u spremištu OMS.  Rješenja se može dodati u radni prostor iz galerije rješenja i obično sadrže alata za analizu dodatne na portalu OMS.  

## <a name="summary-of-data-sources"></a>Sažetak izvora podataka

Izvori podataka koje su trenutno dostupne na prijava analitiku su navedene u tablici u nastavku.  Svaka ima vezu s člankom zasebnom koja omogućuje detalja za taj izvor podataka.

| Izvor podataka | Vrsta događaja | Opis |
|:--|:--|:--|
| [Prilagođeni zapisnika](log-analytics-data-sources-custom-logs.md) | \<LogName\>_CL | Tekstnih datoteka na Windows ili Linux agenata koji sadrži informacije iz zapisnika. |
| [Zapisnike događaja sustava Windows](log-analytics-data-sources-windows-events.md) | Događaja | Događaji prikupljenih u zapisniku događaja na računala sa sustavom Windows. |
| [Mjerača performansi sustava Windows](log-analytics-data-sources-performance-counters.md) | Mjerača performansi | Mjerača performansi prikupljeni s računala sa sustavom Windows. |
| [Mjerača Linux performansi](log-analytics-data-sources-performance-counters.md) | Mjerača performansi | Mjerača performansi prikupljeni s računala Linux. |
| [IIS zapisnika](log-analytics-data-sources-iis-logs.md) | W3CIISLog | Internet Information Services prijavljuje u obliku W3C. |
| [Syslog](log-analytics-data-sources-syslog.md) | Syslog | Događaji Syslog na računalima sustava Windows ili Linux. |

## <a name="configuring-data-sources"></a>Konfiguriranje izvora podataka

Konfiguriranje izvora podataka na izborniku **podataka** u prijava analitiku **Postavke**.  Sve konfiguracije je isporučen svih povezanih izvora u radnom prostoru OMS.  Trenutno ne možete isključiti sve agenata iz tu konfiguraciju.

![Konfiguriranje događaja sustava Windows](./media/log-analytics-data-sources/configure-events.png)

2. Na konzoli OMS odaberite pločicu **Postavke** .
3. Odaberite **podatke**.
4. Kliknite izvor podataka da biste konfigurirali.
5. Slijedite vezu potražite u dokumentaciji za svaki izvor podataka iz prethodne tablice detalje o njihovim konfiguraciji.

## <a name="data-collection"></a>Prikupljanje podataka

Konfiguracija izvora podataka isporučuju agenata koji izravno povezani s OMS za nekoliko minuta.  Navedeni podaci koji se prikupljaju agenta i isporučuju izravno prijava analitiku u vremenskim razmacima specifične za svaki izvor podataka.  Potražite u dokumentaciji za svaki izvor podataka za te specifičnosti.

Agente sustava centra operacije Manager (SCOM) u grupi Upravljanje povezani, konfiguracije izvora podataka su prevede paketi za upravljanje i isporučena u grupi Upravljanje svakih 5 minuta prema zadanim postavkama.  Agenta preuzima paket za upravljanje poput drugih i prikuplja određene podatke. Ovisno o izvoru podataka jer će se podaci ili poslane na poslužitelju za upravljanje koji prosljeđuje podatke analize zapisnika ili agenta će se podaci poslati prijava analitiku bez prolaska kroz poslužitelj za upravljanje. Pogledajte [detalje zbirke podataka za OMS značajke i daje rješenja](log-analytics-add-solutions.md#data-collection-details-for-oms-features-and-solutions) detalje.  Čitajte detalje o povezivanju SCOM i OMS i izmjenu učestalost tu konfiguraciju je isporučen na [Konfiguriranje Integracija sa sustava centar za komponentu Operations Manager](log-analytics-om-agents.md).

## <a name="log-analytics-records"></a>Prijava analitiku zapisa

Sve podatke prikupljene putem prijava analitiku pohranjena u spremištu OMS kao zapisi.  Zapisi koji se prikupljaju različitih vrsta izvora sadržavat će vlastiti skup svojstava, a prepoznaje njihove svojstvo **Vrsta** .  Potražite u dokumentaciji za svaki izvor podataka i rješenje detalje na svaku vrstu zapisa.


## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o [rješenja](log-analytics-add-solutions.md) koja dodavanje funkcionalnosti zapisnika analize i i prikupljanje podataka u spremište OMS.
- Informirajte se o [zapisniku pretraživanja](log-analytics-log-searches.md) radi analize podataka prikupljenih iz izvora podataka i rješenja.  
- Konfiguriranje [upozorenja](log-analytics-alerts.md) doći obavijesti o ključnih podataka prikupljenih iz izvora podataka i rješenja.
