<properties
    pageTitle="Praćenje plošni koncentratora s prijava analitiku | Microsoft Azure"
    description="Pomoću rješenje plošni koncentrator za praćenje stanja sustava koncentratora plošni i razumjeli kako se koji se koriste."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="monitor-surface-hubs-with-log-analytics"></a>Plošni koncentratora monitora s zapisnika Analytics

U ovom se članku opisuje kako možete koristiti rješenja plošni koncentratora u prijava analitiku praćenje Microsoft Surface koncentrator uređaja pomoću sustava Microsoft operacije upravljanja paket (OMS). Prijava analitiku pomaže vam praćenje stanja sustava koncentratora plošni kao i razumjeli kako se koji se koriste.

Svaki plošni koncentrator je u programu Microsoft nadzor instaliran Agent servisa. Njegov kroz agent koje možete poslati podatke iz centra za plošni OMS. Datoteke zapisnika su za čitanje s koncentratorima površina i su, a bit će poslani OMS usluga. Problemi s poslužiteljima u tijeku izvanmrežni način rada, kalendar ne sinkronizira, kao što su ili ako je uređaj račun nije moguće prijaviti Skype prikazana su u OMS na nadzornoj ploči plošni koncentratora. Pomoću podataka na nadzornoj ploči možete prepoznati uređaje koji se izvode ili koje su drugi problema, a potencijalno primijenite rješenja za otkriveni problemi.


## <a name="installing-and-configuring-the-solution"></a>Instaliranje i konfiguriranje rješenja

Poslužite se sljedećim informacijama za instalaciju i konfiguriranje rješenja. Da biste upravljali vaše koncentratora plošni iz sustava Microsoft operacije upravljanja paket (OMS), potrebno je sljedeće:

- Valjani pretplatu na [OMS](http://www.microsoft.com/oms).
- Razina [OMS pretplata](https://azure.microsoft.com/pricing/details/log-analytics/) će podržavati broj uređaja koje želite nadzirati. Cijene OMS ovisi o upisani koliko je uređaja i koliko je podataka je procesa. Ćete htjeti to uzeti u obzir prilikom planiranja uvođenje plošni koncentratora.

Nakon toga ćete ili dodavanje pretplatu OMS u postojeću pretplatu na Microsoft Azure ili stvaranje novog radnog prostora izravno putem portala OMS. Detaljne upute za korištenje neke od ovih metoda nalazi se na [Početak rada s zapisnika analize](log-analytics-get-started.md). Kada postavite OMS pretplatu, dva su načina da biste registrirali uređajima koncentrator površina:

- Automatski kroz InTune
- Ručno putem **Postavke** na uređaju plošni koncentratora.

## <a name="set-up-monitoring"></a>Postavljanje nadzora

Možete nadzirati stanja i aktivnosti pomoću zapisnika analize u OMS koncentratora za površina. Plošni koncentratora u OMS možete registrirati pomoću InTune ili lokalno pomoću **postavki** u središtu površina.

## <a name="connect-surface-hubs-to-oms-through-intune"></a>Plošni koncentratora povezati OMS kroz InTune

Morat ćete prostor ID i radnog prostora ključ za OMS radnog prostora koji će upravljati vaše plošni koncentratora. Možete dobiti one preuzete iz OMS portal.

InTune je Microsoftovih proizvoda koji vam omogućuje da središnje upravljanje OMS konfiguracijske postavke koje se primjenjuju na jednu ili više uređaja. Slijedite ove korake da biste konfigurirali uređajima putem InTune:

1. Prijavite se u InTune.
2. Idite na **Postavke** > **povezani izvora**.
3. Stvarati ili uređivati pravila utemeljenog na predlošku plošni koncentratora.
4. Dođite na sekciju OMS (uvida radu sa servisom Azure) pravila, a dodati *Prostor ID* i *Radnog prostora ključ* pravila.
5. Spremite pravila.
6. Pravilnik pridružiti grupi odgovarajuće uređaja.

  ![Pravila InTune](./media/log-analytics-surface-hubs/intune.png)

InTune zatim sinkronizira postavke OMS s uređaja u odredišnu grupu uvrštavate ih u radnom prostoru OMS.

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>Plošni koncentratora povezati OMS pomoću postavki aplikacije

Morat ćete prostor ID i ključ radnog prostora za OMS radnog prostora koji će upravljati vaše plošni koncentratora. Možete dobiti onima na portalu OMS.

Ako ne koristite InTune da biste upravljali okruženju, možete registrirati uređaje ručno putem **Postavke** na svaku koncentrator površina:

1. Iz centra za površina, otvorite **Postavke**.
2. Unesite administratorske vjerodajnice uređaja kada se to od vas zatraži.
3. Kliknite **ovaj uređaj**i u odjeljku **nadzor**, kliknite **Konfiguriraj postavke OMS**.
4. Odaberite **Omogući nadzor**.
6. U dijaloškom okviru Postavke OMS unesite **ID radnog prostora** , a zatim unesite **Ključ za radni prostor**.  
  ![postavke](./media/log-analytics-surface-hubs/settings.png)
7. Kliknite **u redu** da biste dovršili konfiguraciju.

Pojavit će se potvrda koja upozorava li konfiguracije OMS uspješno je primijenjena na uređaju. Ako je, pojavljuje se poruka koja kaže da agenta uspješno povežete sa servisom OMS. Uređaj zatim pokreće slanja podataka OMS gdje možete pogledati i djelovanje na njemu.

## <a name="monitor-surface-hubs"></a>Plošni koncentratora monitora

Nadzor vaše koncentratora plošni korištenje OMS je slično kao što su praćenje drugih uvrštena uređaja.

1. Prijavite se na OMS portal.
2. Idite na nadzornoj ploči paketa rješenja plošni koncentratora.
3. Prikazat će se stanje na uređaju.

  ![Plošni koncentrator nadzorne ploče](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Možete stvoriti [upozorenja](log-analytics-alerts.md) na temelju postojeće ili prilagođeni zapisnika pretraživanja. Pomoću podataka na OMS prikuplja iz vaše koncentratora površina, možete potražiti problema i upozorenja na uvjetima koje sami definirate za uređaje.


## <a name="next-steps"></a>Daljnji koraci

- Da biste pogledali detaljne podatke plošni koncentrator pomoću [zapisnika pretraživanja u zapisnik analize](log-analytics-log-searches.md) .
- Stvaranje [upozorenja](log-analytics-alerts.md) koja vas obavještava kada se pojave problemi s vašeg plošni koncentratora.
