<properties
    pageTitle="Strujanje zapisnik aktivnosti Azure s koncentratorima događaj | Microsoft Azure"
    description="Saznajte kako strujanje zapisnik aktivnosti Azure s koncentratorima događaj."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="johnkem"/>

# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Strujanje zapisnik aktivnosti Azure s koncentratorima događaja
[**Zapisnik aktivnosti Azure**](./monitoring-overview-activity-logs.md) moguće je strujanjem u stvarnom vremenu u najbliži na bilo koji program pomoću ugrađenih mogućnost "Izvoz" na portalu ili tako da omogućite servis za Id pravilo Bus u profilu zapisnika pomoću cmdleta ljuske PowerShell Azure ili Azure EŽA.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Što možete učiniti s zapisnik aktivnosti i događaja koncentratora
Evo nekoliko načina na koje koristite strujanje mogućnost za zapisnik aktivnosti:

- **Strujanje proizvođača zapisnika i telemetriju sustava** – s vremenom događaj koncentratora strujanje će postati mehanizam pipe o prijavi aktivnosti u SIEMs drugih proizvođača i prijava analitiku rješenja.
- **Stvaranja prilagođenih telemetrijskih i zapisivanje platformu** – ako već imate custom-built telemetrijskih platformu ili su samo mislim o izradi jednu, Visoko skalabilni objavljivanja-pretplate prirode događaj koncentratora omogućuje vam da flexibly ingest zapisnik aktivnosti. [Pogledajte vodič za Dan Rosanova pomoću koncentratora događaja u platforma globalni skaliranje telemetrijskih ovdje.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Omogući strujanje zapisnika aktivnosti
Možete omogućiti strujanje zapisnika aktivnosti programski ili putem portala sustava. U svakom slučaju, odaberete prostor naziva Bus servisa i zajednički pristup pravila za taj prostor naziva i koncentratora za događaj će se stvoriti u tom naziva kada dođe do prvog novi događaj zapisnik aktivnosti. Ako nemate prostor naziva Bus servisa, najprije morate stvoriti. Ako ste prethodno strujanjem aktivnosti zapisnika događaja imenski Bus servisa, središtu događaja koji je već stvoren će se ponovno koristiti. Pravilnik za zajednički pristup definira dozvole koje ima mehanizam za strujanje. Danas, strujanje sustava koncentratora događaj potrebna je dozvola za **Upravljanje**, **čitanje**i **Slanje** . Možete stvoriti ili izmijeniti polje naziva Bus servisa zajednički pristup pravila na portalu klasični na kartici "Konfiguriraj" za vaše polje naziva Bus servisa. Da biste ažurirali profila zapisnika zapisnik aktivnosti da biste uključili strujanje, korisnik mijenjanju postavki morate imati dozvolu za ListKey na servis Bus autorizacije pravilo.

### <a name="via-azure-portal"></a>Putem portala za Azure 
1. Dođite do plohu **Zapisnik aktivnosti** pomoću izbornika na lijevoj strani portalu.

    ![Pomaknite se do zapisnik aktivnosti u portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Kliknite gumb **Izvezi** pri vrhu na plohu.

    ![Gumb za izvoz u portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. U plohu koji će se prikazati, možete odabrati područja za koju želite strujanje događaja i polje naziva Bus servisa u kojoj želite na događaj koncentrator će biti stvoren za strujanje te događaja.

    ![Izvoz plohu zapisnik aktivnosti](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Kliknite **Spremi** da biste spremili postavke. Postavke odmah se primjenjuju se na vašu pretplatu.


### <a name="via-powershell-cmdlets"></a>Pomoću cmdleta ljuske PowerShell
Ako već postoji zapisnika profila, najprije morate ukloniti taj profil.

1. Korištenje `Get-AzureRmLogProfile` prepoznati ako postoji zapisnika profila
2. Ako je tako, koristite `Remove-AzureRmLogProfile` da biste je uklonili.
3. Korištenje `Set-AzureRmLogProfile` stvaranje profila:

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

Servis Bus pravilo ID je niz u ovom obliku: {servisa ID resursa bus} /authorizationrules/ {naziv ključa}, na primjer 

### <a name="via-azure-cli"></a>Putem Azure EŽA
Ako već postoji zapisnika profila, najprije morate ukloniti taj profil.

1. Korištenje `azure insights logprofile list` prepoznati ako postoji zapisnika profila
2. Ako je tako, koristite `azure insights logprofile delete` da biste je uklonili.
3. Korištenje `azure insights logprofile add` stvaranje profila:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

Servis Bus pravilo ID je niz u ovom obliku: `{service bus resource ID}/authorizationrules/{key name}`.
 
## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Kako se koristi podatke iz koncentratora za događaj?
[Shema za zapisnik aktivnosti dostupna je u nastavku](./monitoring-overview-activity-logs.md). Svaki događaj se niz JSON blob-ova pod nazivom "zapise."

## <a name="next-steps"></a>Daljnji koraci
- [Arhiviranje zapisnik aktivnosti s računom za pohranu](./monitoring-archive-activity-log.md)
- [Pročitajte pregled zapisnik aktivnosti Azure](./monitoring-overview-activity-logs.md)
- [Postavljanje upozorenja na temelju aktivnosti zapisnika događaja](./insights-auditlog-to-webhook-email.md)
