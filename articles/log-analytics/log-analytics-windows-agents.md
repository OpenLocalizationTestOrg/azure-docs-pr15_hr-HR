<properties
    pageTitle="Prijava analitiku povezivanje računala sa sustavom Windows | Microsoft Azure"
    description="U ovom se članku prikazuje korake za povezivanje s računala sa sustavom Windows u sustavu lokalnog izravno u OMS pomoću prilagođene verzije programa sustava Microsoft nadzor Agent (MMA)."
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


# <a name="connect-windows-computers-to-log-analytics"></a>S računala sa sustavom Windows zapisnika Analytics

U ovom se članku objašnjava korake za povezivanje s računala sa sustavom Windows u sustavu lokalnog izravno u radne prostore OMS pomoću prilagođene verzije programa sustava Microsoft nadzor Agent (MMA). Morate instalirati i povezati agente za sve računala na kojima onboard OMS kako bi ih slanje podataka u OMS i da biste pogledali i djelovanje na tih podataka na portalu OMS. Agent za svaki mogu prijaviti više radne prostore.

Možete instalirati agenata pomoću postavljanja, naredbenog retka ili s želji stanje konfiguracije (DSC) u automatizaciji Azure.  

>[AZURE.NOTE] Za virtualnim strojevima sa servisu Azure može pojednostavniti instalacije pomoću [proširenje virtualnog računala](log-analytics-azure-vm-extension.md).

Na računalima s internetskom vezom agenta koristit će se veza s Internetom slanje podataka u OMS. Računalima s Internetom, poslužite se proxy poslužitelj ili Forwarder analitičkih podataka za zapisnik OMS.

Povezivanje vašeg računala sa sustavom Windows s OMS je jednostavne 3 jednostavan način:

1. Preuzimanje datoteke instalacijski program agent
2. Instalirajte agent metodom odaberete
3. Konfiguriranje agenta ili dodajte dodatne radne prostore, ako je potrebno

Sljedeći dijagram prikazuje odnos između računala sa sustavom Windows i OMS ako ste instalirali i konfigurirali agenata.

![OMS-izravno-agent-dijagrama](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)


## <a name="system-requirements-and-required-configuration"></a>Sistemski preduvjeti i potrebna konfiguracija
Prije instalacije ili uvođenje agenata, pročitajte članak sljedeće da biste bili sigurni da ispunjavate uvjete potrebno.

- OMS MMA možete instalirati samo na računalima sa sustavom Windows Server 2008 SP 1 ili noviji ili Windows 7 SP1 ili noviji.
- Potreban vam je pretplata na OMS.  Dodatne informacije potražite u članku [Početak rada s zapisnika analize](log-analytics-get-started.md).
- Svakom računalu sa sustavom Windows moraju imati mogućnost da biste se povezali s internetom putem HTTP. Ovu vezu može biti izravno putem proxy poslužitelj, ili Forwarder analitičkih podataka za zapisnik OMS.
- OMS MMA možete instalirati na samostalni računala, poslužitelja i virtualnih računala. Ako se želite povezati Azure hostira virtualnim strojevima OMS potražite u članku [Povezivanje Azure virtualnih računala da biste zapisnika analize](log-analytics-azure-vm-extension.md).
- Agenta mora koristiti TCP priključak 443 za razne resurse. Dodatne informacije potražite u članku [Konfiguracija postavki proxy poslužitelja i vatrozida u zapisnik analize](log-analytics-proxy-firewall.md).

## <a name="download-the-agent-setup-file-from-oms"></a>Preuzimanje datoteka instalacijski program agent iz OMS
1. Na portalu OMS na stranici **Pregled** kliknite pločicu **Postavke** .  Kliknite karticu **Povezani izvora** na vrhu.  
    ![Povezani kartica izvori](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. U odjeljku **Izravno prilaganje računala**, kliknite **Preuzimanje Agent za Windows** dodijeljen vrsta procesor vašeg računala da biste preuzeli instalacijsku datoteku.
3. Na desnoj strani **Radni prostor**ID-a, kliknite ikonu kopiju i zalijepite ID u blok za pisanje.
4. Na desnoj strani **Primarnog**ključa, kliknite ikonu kopiju i zalijepite ključ u blok za pisanje.     
    ![Kopiranje radni prostor ID i primarnog ključa](./media/log-analytics-windows-agents/oms-direct-agent-primary-key.png)

## <a name="install-the-agent-using-setup"></a>Instalirajte agent pomoću instalacijskog programa
1. Pokrenite instalaciju agenta na računalima na kojima želite upravljati.
2. Na stranici dobrodošlice kliknite **Dalje**.
3. Na stranici licencne odredbe za čitanje licence, a zatim kliknite **li Agree**.
4. Na stranici odredišnu mapu, promjena ili zadržati zadanu mapu instalacije, a zatim kliknite **Dalje**.
5. Na stranici s mogućnostima instalacijski program Agent možete povezati agenta za Azure prijava analitiku (OMS), Operations Manager ili mogućnosti možete ostaviti prazno ako želite konfigurirati agenta kasnije. Kliknite **Dalje**.   
    - Ako se odlučite za povezivanje za Azure zapisnika analize (OMS), zalijepite **Radni prostor ID** i **Radnog prostora ključ (primarni ključ)** koju ste kopirali u blok za pisanje iz prethodnog postupka, a zatim kliknite **Dalje**.  
        ![Lijepljenje radni prostor ID i primarnog ključa](./media/log-analytics-windows-agents/connect-workspace.png)
    - Ako se odlučite za povezivanje s Operations Manager, upišite **Naziv grupe za upravljanje**, naziv **Poslužitelja za upravljanje** i **Upravljanje priključak poslužitelja**, a zatim **Dalje**. Na stranici računa Agent akcije odaberite lokalni sustav račun ili račun lokalne domene, a zatim kliknite **Dalje**.  
        ![Konfiguracija grupe za upravljanje](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![agent akcije računa](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Na jeste li spremni za stranice instalacija, ponovno razmotrite svoj odabir, a zatim kliknite **Instaliraj**.
7. O konfiguraciji uspješno dovršena stranice, kliknite **Završi**.
8. Po dovršetku **Microsoft Agent nadzor** pojavit će se na **Upravljačkoj ploči**. Možete pregledati vašoj konfiguraciji i provjerite je li agenta priključen u radu sa servisom uvida (OMS). Kada ste povezani s OMS, agenta prikazuje se poruka koja kaže: **u Microsoft nadzor Agent je uspješno povežete sa servisom Microsoft operacije upravljanja paket.**

## <a name="install-the-agent-using-the-command-line"></a>Instalirajte agent pomoću naredbenog retka
- Izmjena, a zatim pomoću u sljedećem primjeru da biste instalirali agent pomoću naredbenog retka.

    >[AZURE.NOTE] Ako želite nadograditi agent trebate koristiti analize zapisnika skriptiranje API-JA. U sljedećem odjeljku da biste nadogradili agent.

    ```
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

## <a name="upgrade-the-agent-and-add-a-workspace-using-a-script"></a>Nadogradnja agenta i dodajte radnom prostoru pomoću skripte
Možete nadograditi agent i dodati radnom prostoru pomoću analize zapisnika skriptiranje API-JA s u sljedećem primjeru PowerShell.

```
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

>[AZURE.NOTE] Ako ste već koristili naredbenog retka ili skripta za instalaciju ili konfiguriranje agent, `EnableAzureOperationalInsights` zamijenjen je `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Instalirajte agent pomoću DSC u automatizaciji Azure

>[AZURE.NOTE] U ovom se primjeru postupak i skripte će nadograditi postojeće agent.

1. Uvoz xPSDesiredStateConfiguration DSC modula iz [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) u Azure automatizaciju.  
2.  Stvaranje Azure Automatizacija varijable resursi za *OPSINSIGHTS_WS_ID* i *OPSINSIGHTS_WS_KEY*. Postavite *OPSINSIGHTS_WS_ID* OMS prijava analitiku radni prostor ID-a i postavite *OPSINSIGHTS_WS_KEY* primarni ključ radnog prostora.
3.  Koristite skriptu ispod i spremanje u obliku MMAgent.ps1
4.  Izmjena, a zatim pomoću u sljedećem primjeru da biste instalirali agent pomoću DSC u automatizaciji Azure. Uvesti MMAgent.ps1 Automatizacija Azure pomoću sučelja Automatizacija Azure ili cmdlet.
5.  Dodijelite čvor konfiguracije. Unutar 15 minuta čvor će potvrdite svoju konfiguraciju, a zatim na MMA će se završi čvor.

```
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "http://download.microsoft.com/download/0/C/0/0C072D6E-F418-4AD4-BCB2-A362624F400A/MMASetup-AMD64.exe"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}  


```


## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Ručno konfiguriranje agent ili dodajte dodatne radne prostore
Ako ste instalirali agenata, ali ih ne konfigurirali ili ako želite da se agent za više radnih prostora, možete koristiti sljedeće informacije da biste omogućili agent ili konfigurirajte ga. Nakon konfiguriranja agenta će registrirati sa servisom agent i ćete dobiti informacije o konfiguraciji potrebne i upravljanje paketi koji sadrže podatke rješenja.

1. Kada instalirate Microsoft Agent nadzor, otvorite **Upravljačku ploču**.
2. Otvorite **Microsoft Agent nadzor** , a zatim kliknite karticu **Azure prijava analitiku (OMS)** .   
3. Kliknite **Dodaj** da biste otvorili okvir **Dodaj radni prostor analize zapisnika** .
4. Zalijepite **Radni prostor ID** i **Radnog prostora ključ (primarni ključ)** koju ste kopirali u blok za pisanje u prethodnog postupka za radni prostor koji želite dodati, a zatim kliknite **u redu**.  
    ![Konfiguriranje radu uvida](./media/log-analytics-windows-agents/add-workspace.png)

Nakon što s računala nadzire agenta prikupljanja podataka, broj računala nadzire OMS prikazat će se na portalu OMS na kartici **Povezani izvora** u **postavkama** kao **Povezani poslužiteljima**.


## <a name="to-disable-an-agent"></a>Da biste onemogućili agent
1. Nakon što instalirate agenta, otvorite **Upravljačku ploču**.
2. Otvorite Microsoft Agent nadzor, a zatim kliknite karticu **Azure prijava analitiku (OMS)** .
3. Odabir radnog prostora, a zatim kliknite **Ukloni**. Ponovite ovaj korak za sve druge radne prostore.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>Po želji konfiguriranje agenata da biste prijavili u grupi Upravljanje Operations Manager

Ako koristite Operations Manager u IT infrastrukturu, možete koristiti i MMA agent kao agent Operations Manager.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>Da biste konfigurirali agenata MMA da biste prijavili u grupi Upravljanje Operations Manager
1.  Na računalu na kojem je instaliran agenta otvorite **Upravljačku ploču**.
2.  Otvorite **Microsoft Agent nadzor** , a zatim kliknite karticu **Operations Manager** .
    ![Kartica Microsoft nadzor Agent Operations Manager](./media/log-analytics-windows-agents/om-mg01.png)
3.  Ako vaši poslužitelji Operations Manager Integracija sa servisom Active Directory, kliknite **automatski ažurirati upravljanje Grupiraj dodjele iz AD DS**.
4.  Kliknite **Dodaj** da biste otvorili dijaloški okvir **Dodaj grupu upravljanje** .  
    ![Microsoft Agent nadzor dodajte grupu za upravljanje](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  U okviru **naziv grupe upravljanje** upišite naziv grupe za upravljanje.
6.  U okviru **Upravljanje primarni poslužitelj** upišite naziv poslužitelja za primarni upravljanje.
7.  U okviru **priključak poslužitelja za upravljanje** unesite broj priključka TCP.
8.  U odjeljku **Agent akcije računa**odaberite lokalni sustav račun ili račun lokalne domene.
9.  Kliknite **u redu** da biste zatvorili dijaloški okvir **Dodaj grupu upravljanja** i **u redu** da biste zatvorili dijaloški okvir **Svojstva Agent nadzor Microsoft** .

## <a name="optionally-configure-agents-to-use-the-oms-log-analytics-forwarder"></a>Po želji konfiguriranje agenata da biste koristili Forwarder analitičkih podataka za zapisnik OMS

Ako imate poslužitelja ili klijenti koje niste povezani s Internetom, ih slati podatke OMS pomoću Forwarder analitičkih podataka za zapisnik OMS možete i dalje imate.  Kada koristite na forwarder, sve podatke iz agenata se šalje putem jednog poslužitelja koja ima pristup Internetu. Na Forwarder prenosi podatke iz sustava agente OMS izravno bez analiza bilo koji od podataka koji se prenose.

U odjeljku [OMS prijava analitiku Forwarder](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) da biste saznali više o forwarder, uključujući instalacije i konfiguracije.

Informacije o konfiguriranju svoje agente da koristite proxy poslužitelj, koji u ovom slučaju je OMS Forwarder, potražite u članku [Konfiguriranje postavke proxy poslužitelja i vatrozida u zapisnik analize](log-analytics-proxy-firewall.md).

## <a name="optionally-configure-proxy-and-firewall-settings"></a>Možete konfigurirati postavke proxy poslužitelja i vatrozida
Ako imate proxy poslužitelji i vatrozidi u svom okruženju ograničiti pristup Internetu, potražite u članku [Konfiguracija postavki proxy poslužitelja i vatrozida u zapisnik analize](log-analytics-proxy-firewall.md) da biste omogućili vaše agenata komunikaciju OMS usluga.

## <a name="next-steps"></a>Daljnji koraci

- [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md) za dodavanje funkcionalnosti i njihovo prikupljanje podataka.
- [Konfiguracija postavki proxy poslužitelja i vatrozida u zapisnik analize](log-analytics-proxy-firewall.md) ako vaša tvrtka koristi proxy poslužitelj ili vatrozid tako da se agenata mogli komunicirati sa servisom zapisnika analize.
