<properties
   pageTitle="Zapisnik analize upozorenja webhook uzorka"
   description="Jedna od akcija možete pokrenuti u odgovoru prijava analitiku upozorenje je na *webhook*, koji omogućuje pozivanje vanjski postupak putem jednog HTTP zahtjev. U ovom se članku vodi kroz stvaranje webhook akcija u upozorenjem prijava analitiku pomoću Prazan hod primjer."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="webhooks-in-log-analytics-alerts"></a>Webhooks u prijava analitiku upozorenja

Jedna od akcija možete pokrenuti u odgovoru [Prijava analitiku upozorenje](log-analytics-alerts.md) je na *webhook*, koji omogućuje pozivanje vanjski postupak putem jednog HTTP zahtjev.  Čitajte detalje o upozorenjima i webhooks u [upozorenja u zapisnik Analytics](log-analytics-alerts.md)

U ovom se članku ćemo vas provesti kroz stvaranje webhook akcija u upozorenjem prijava analitiku pomoću Prazan hod koja je servis za razmjenu primjer.

>[AZURE.NOTE] Morate imati zalihe račun da biste dovršili ovaj uzorak.  Možete se prijaviti za besplatan račun pri [slack.com](http://slack.com).

## <a name="step-1---enable-webhooks-in-slack"></a>Korak 1 - Omogući webhooks u prazan hod
2.  Prijavite se na Slack na [slack.com](http://slack.com).
3.  Odabir kanala u odjeljku **kanala** u lijevom oknu.  Ovo je kanala koja će se slati poruke.  Možete odabrati jednu od zadane kanala primjerice **Općenito** ili **izravnim**.  U scenariju s radni bi najvjerojatnije stvaranje posebne kanala kao što su **criticalservicealerts**. <br>

    ![Zalihe kanala](media/log-analytics-alerts-webhooks/oms-webhooks01.png)

3. Kliknite **Dodaj aplikaciju ili prilagođeni Integracija** da biste otvorili aplikaciju direktorija.
3.  U okvir za pretraživanje upišite *webhooks* , a zatim odaberite **Dolazne WebHooks**. <br>

    ![Zalihe kanala](media/log-analytics-alerts-webhooks/oms-webhooks02.png)

4.  Kliknite **Instaliraj** uz naziv tima.
5.  Kliknite **Dodaj konfiguracije**.
6.  Odaberite na kanalu za koji namjeravate koristiti u ovom primjeru, a zatim kliknite **Dodaj dolazne WebHooks Integracija**.  
6. Kopirajte **Webhook URL**.  Koje će biti lijepljenje to upozorenja konfiguracije. <br>

    ![Zalihe kanala](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Korak 2 – da biste stvorili pravilo za upozorenja u zapisnik Analytics
1.  [Stvaranje upozorenja pravila](log-analytics-alerts.md) sljedeće postavke.
    - Upit:```    Type=Event EventLevelName=error ```
    - Traženje upozorenje svakih: pet minuta
    - Broj rezultata: veća od 10
    - Putem ovo doba dana: 60 minuta
    - Odaberite **da** za **Webhook** i **bez** za ostale akcije.
7. Zalijepite URL Prazan hod u polje **Webhook URL** .
8. Odaberite mogućnost **dodavanja prilagođene tereta JSON**.
9. Tolerancija očekuje tereta parametar pod nazivom *tekst*oblikovan u JSON.  Ovo je tekst koji će se prikazivati u poruci stvara.  Možete koristiti jedan ili više parametara upozorenja pomoću na *#* simbol kao što su kao u sljedećem primjeru.

    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```

    ![primjer JSON tereta](media/log-analytics-alerts-webhooks/oms-webhooks07.png)

9.  Kliknite **Spremi** da biste spremili pravilo za upozorenja.

10. Pričekajte dovoljno vremena za upozorenje koje se stvara i zatim provjerite Prazan hod za poruku koja će biti otprilike ovako.

    ![primjer webhook u prazan hod](media/log-analytics-alerts-webhooks/oms-webhooks08.png)


### <a name="advanced-webhook-payload-for-slack"></a>Napredne webhook teret za Prazan hod

Moguće je prilagoditi opsežno ulazne poruke s Prazan hod. Dodatne informacije potražite u članku [Dolazne Webhooks](https://api.slack.com/incoming-webhooks) zalihe web-mjesta. Slijedi složenije teret za stvaranje obogaćenih poruke s oblikovanjem:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


To će generirati poruke u prazan hod otprilike ovako.

![primjer poruke u prazan hod](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Sažetak

Uz to upozorenja pravilo na mjestu promijenile poruku poslao Prazan hod svaki put ispunjen kriterij.  

To je samo jedan primjer akcije koje možete stvoriti u odgovoru upozorenja.  Možete stvoriti webhook akciju koja se poziva drugih servisa za vanjske, runbook akcije da biste započeli s runbook u Automatizacija Azure ili akciju e-pošte da biste poslali e-poštu da biste sebi ili drugim primateljima.   

## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o [upozorenja u zapisnik analize](log-analytics-alerts.md) uključujući druge akcije.
- [Stvaranje runbooks u automatizaciji Azure](../automation/automation-webhooks.md) koje se pozivati iz programa webhook.
