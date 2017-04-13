<properties
    pageTitle="Okomito mjerilo Azure virtualnog računala s Azure Automatizacija | Microsoft Azure"
    description="Kako okomito skaliranje Linux virtualnog računala kao odgovor na nadzor upozorenja s Automatizacija Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/29/2016"
    ms.author="singhkay"/>

# <a name="vertically-scale-azure-virtual-machine-with-azure-automation"></a>Okomito mjerilo Azure virtualnog računala s Automatizacija Azure

Okomito skaliranje je postupak rastućim resursi računala kao odgovor na povećavaju ili. U Azure to je moguće napraviti tako da promijenite veličinu virtualnog računala. To može pomoći u sljedećim scenarijima

- Ako često ne koristi virtualnog računala, možete promijeniti veličinu ga prema dolje do manja veličina da biste smanjili mjesečni troškove
- Ako virtualnog računala je Vršna opterećenja, je veličina može se promijeniti na veću veličinu da biste povećali njegov kapacitet

Struktura korake za obavljanje to je kao ispod

1. Automatizacija Azure da biste pristupili virtualnim računalima za postavljanje
2. Uvoz runbooks Azure Automatizacija okomito mjerilo u pretplatu
3. Dodavanje u webhook vaše runbook
4. Dodavanje upozorenja virtualnog računala

> [AZURE.NOTE] Zbog veličina prvi virtualnog računala, veličine može biti neproporcionalno, možda će biti ograničen zbog dostupnost veličina u klasteru trenutni virtualnog računala je uveden u. U objavljeni Automatizacija runbooks koji se koristi u ovom članku ćemo voditi brigu o ovom slučaju i samo skaliranje unutar na ispod parove VM veličina. To znači da Standard_D1v2 virtualnog računala će iznenada neće biti skalirana prema gore da biste Standard_G5 ili skalirana prema dolje da biste Basic_A0.

>| Veličina VM skaliranje par |   |
|---|---|
|  Basic_A0 |  Basic_A4 |
|  Standard_A0 | Standard_A4 |
|  Standard_A5 | Standard_A7  |
|  Standard_A8 | Standard_A9  |
|  Standard_A10 |  Standard_A11 |
|  Standard_D1 |  Standard_D4 |
|  Standard_D11 | Standard_D14  |
|  Standard_DS1 |  Standard_DS4 |
|  Standard_DS11 | Standard_DS14  |
|  Standard_D1v2 |  Standard_D5v2 |
|  Standard_D11v2 |  Standard_D14v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Automatizacija Azure da biste pristupili virtualnim računalima za postavljanje

Prvo što biste trebali učiniti je stvorite račun za Azure Automatizacija koji će biti smješteno runbooks koji se koristi za promjenu veličine instance postavite skaliranje VM. Nedavno Automation Services uvedena značajku "Pokreni kao račun" čega postavku gore glavnice za servis za automatsko pokretanje na runbooks u ime korisnika vrlo lako. Dodatne informacije o tome u nastavku članka:

* [Autentičnost Runbooks s računom za Azure Pokreni kao](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Uvoz runbooks Azure Automatizacija okomito mjerilo u pretplatu

Runbooks koji su potrebni za okomito skaliranje virtualnog računala su već objavljene u galeriji Azure Automatizacija Runbook. Morat ćete ih uvesti u svoju pretplatu. Dodatne upute za uvoz runbooks u sljedećem članku za čitanje.

* [Galerija Runbook i module za automatizaciju Azure](../automation/automation-runbook-gallery.md)

Runbooks koje treba uvesti prikazane su na slici u nastavku

![Uvoz runbooks](./media/virtual-machines-vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Dodavanje u webhook vaše runbook

Kada uvezete runbooks morat ćete dodati na webhook na runbook tako da ga može aktivirati upozorenja iz virtualnog računala. Detalje o stvaranju na webhook za svoje Runbook može pročitati ovdje

* [Azure webhooks automatizacije](../automation/automation-webhooks.md)

Provjerite je li kopirate u webhook prije nego što zatvorite dijaloški okvir webhook kao što je to ćete u sljedećem odjeljku.

## <a name="add-an-alert-to-your-virtual-machine"></a>Dodavanje upozorenja virtualnog računala

1. Odaberite postavke virtualnog računala
2. Odaberite "Upozorenja pravila"
3. Odaberite "Dodaj upozorenje"
4. Odaberite metriku početka upozorenje na
5. Odaberite uvjet, kojeg ispunjena će uzrokovati upozorenje da biste fire
6. Odaberite praga za uvjet u koraku 5. Da biste ispuniti
7. Odaberite razdoblje tijekom kojih će servis nadzora provjeriti stanje i prag u korake 5 i 6
8. Zalijepite webhook koju ste kopirali iz prethodnog odjeljka.

![Dodavanje upozorenje virtualnog računala 1](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-1.png)

![Dodavanje upozorenje virtualnog računala 2](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-2.png)