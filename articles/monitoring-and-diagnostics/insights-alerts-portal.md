<properties
    pageTitle="Stvaranje upozorenja za Azure servise pomoću portala za Azure | Microsoft Azure"
    description="Pomoću portala za Azure stvaranje Azure upozorenja koja se može pokrenuti obavijesti ili Automatizacija kada se ispuni uvjet koji navedete."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="robb"/>

# <a name="use-azure-portal-to-create-alerts-for-azure-services"></a>Stvaranje upozorenja za Azure servise pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [EŽA](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Pregled

U ovom se članku objašnjava postavljanje Azure upozorenja pomoću portala za Azure.   

Možete primati upozorenja na temelju nadzora metriku ili događaji na servisa Azure.

- **Metričkim vrijednostima** - okidača upozorenja kada se vrijednost navedena metrika presjek prag dodijelite u bilo kojem smjeru. To jest, ga i pokreće kada najprije se ispuni uvjet, a zatim naknadno kada koji uvjet se više ne zadovoljavaju.    
- **Aktivnosti zapisnika događaja** - upozorenje koje možete pokrenuti *svaki* događaj ili, samo kad se pojave određeni broj događaja.


Možete konfigurirati upozorenje kada se pokrene, učinite sljedeće:

- slanje obavijesti e-poštom administrator servisa i dodatnih administratora
- Slanje e-pošte dodatna poruke e-pošte koju navedete.
- poziv na webhook
- pokretanje izvođenja Azure kompilacije (samo na portalu Azure)

Možete konfigurirati i informacije o upozorenja pravila pomoću

- [Portal za Azure](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [sučelje naredbenog retka (EŽA)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API-JA](https://msdn.microsoft.com/library/azure/dn931945.aspx)


## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Stvaranje upozorenja pravilo na metrike pomoću portala za Azure

1. [Portal](https://portal.azure.com/)pronađite resursa mogli biti zainteresirani za nadzor i odaberite ga.

2. Odaberite **upozorenja** ili **upozorenja pravila** u odjeljku NADZOR. Tekst i ikone mogu se razlikovati malo za različite resurse.  

    ![Nadzor](./media/insights-alerts-portal/AlertRulesButton.png)


3. Odaberite naredbu **Dodaj upozorenja** , a zatim ispunite polja.

    ![Dodavanje upozorenja](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Naziv** vaše upozorenje pravilo, a zatim odaberite **Opis**, koji se prikazuje obavijest e-pošte.
5. Odaberite **metrički** želite nadzirati, a zatim odaberite **uvjet** i **prag** vrijednost za metriku. I odabrali na **razdoblje** koje metričkim pravilo moraju biti zadovoljeni prije upozorenja okidača. Tako, primjerice, ako koristite razdoblje "PT5M", a vaše upozorenje traži procesora veću od 80%, upozorenje pokreće kad Procesor je dosljedno iznad 80% 5 minuta. Kada dođe do prvog okidača, ga ponovno pokreće kad Procesor ostaje ispod 80% 5 minuta. Mjerne jedinice procesora pojavljuje se svaki 1 minute.   

6. Ako želite administratori i dodatnih administratora moguće je slali kada aktivira se upozorenje, obratite se **... vlasnici e-pošte** .

7. Ako želite dodatnu poruke e-pošte da biste primali obavijesti kada aktivira se upozorenje, dodajte ih u polje **email(s) dodatnih administratora** . Više poruka e-pošte razdvojite točkom sa zarezom-*email@contoso.com;email2@contoso.com*

8. Smjestite valjan URI u polju **Webhook** ako želite da se zove kada aktivira se upozorenje.

9. Ako koristite Azure Automatizacija, možete odabrati Runbook će se pokrenuti kada aktivira se upozorenje.

10. Odaberite **u redu** po završetku da biste stvorili upozorenje.   

Za nekoliko minuta upozorenje je aktivna i pokreće na prethodno opisani način.

## <a name="managing-your-alerts"></a>Upravljanje upozorenjima

Nakon stvaranja upozorenja, možete je odabrati i:

- Prikazati grafikon prikazuje praga metričke i stvarne vrijednosti iz na prethodni dan.
- Uredite ili izbrišite ga.
- **Onemogućivanje** ili **Omogućivanje** je ako želite da biste privremeno zaustavili ili životopis primanje obavijesti o tom upozorenja.



## <a name="next-steps"></a>Daljnji koraci

* [Pregled Azure nadzor](monitoring-overview.md) uključujući vrste informacija mogu prikupljanje i praćenje.
* Saznajte više o [Konfiguriranje webhooks u upozorenja](insights-webhooks-alerts.md).
* Dodatne informacije o [Runbooks Automatizacija Azure](..\automation\automation-starting-a-runbook.md).
* [Pregled dijagnostičke zapisnike](monitoring-overview-of-diagnostic-logs.md) i dohvatite detaljne velika učestalost metriku na servisu.
* Pogledajte [Pregled metriku zbirke](insights-how-to-customize-monitoring.md) da biste bili sigurni na servisu dostupan je i odredište.
