<properties
   pageTitle="Azure VM sigurnosnog kopiranja neće uspjeti: ne može komunicirati s VM agent za brze snimke stanja - snimke VM sub zadatka isteklo | Microsoft Azure"
   description="Simptomi uzroke i rješenja za Azure VM sigurnosne kopije pogreške vezane uz ne može komunicirati s agent VM snimku stanja. Snimka VM sub zadatka isteklo pogreške"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="cfreeman"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jimpark; markgal;genli"/>

# <a name="azure-vm-backup-fails-could-not-communicate-with-the-vm-agent-for-snapshot-status---snapshot-vm-sub-task-timed-out"></a>Azure VM sigurnosnog kopiranja neće uspjeti: ne može komunicirati s VM agent za brze snimke stanja - isteklo snimke VM sub zadatka

## <a name="summary"></a>Sažetak

Nakon registriranja i raspored na virtualnog računala (VM) za Azure sigurnosne kopije, servis za Azure sigurnosne kopije pokreće sigurnosno kopiranje zakazano vrijeme po komunikaciju s sigurnosne kopije datotečni nastavak u VM da biste preuzeli snimku točke u vrijeme. Postoje uvjete koji sprječavaju da se snimka se pokrene koji vodi sigurnosne kopije nije uspjelo. Ovaj članak sadrži upute za otklanjanje poteškoća za problema vezanih uz Azure VM sigurnosne kopije pogreške vezane uz pogreške isteka snimke.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Simptoma

Microsoft Azure Backup za infrastrukturu kao service (IaaS) VM ne uspijeva te vraća sljedeća poruka o pogrešci u detalje o pogrešci posao [Azure portal](https://portal.azure.com/):

**Ne može komunicirati s VM agent za brze snimke stanja: isteklo snimke VM sub zadatka.**

## <a name="cause"></a>Uzrok
Postoje četiri uobičajeni uzroci za tu pogrešku:

- Na VM nemate pristup Internetu.
- Agent za Microsoft Azure VM instalirali u drugom na VM zastario (za Linux VMs).
- Da biste ažurirali ili učitavanje neće uspjeti sigurnosne kopije datotečni nastavak.
- Nije moguće dohvatiti snimke stanja ili ne može biti preuzeta snimki.

## <a name="cause-1-the-vm-does-not-have-internet-access"></a>Uzrok 1: U VM niste povezani s Internetom
Po obavezne implementacije u VM nema pristup Internetu ili ograničenja na mjestu zbog kojih access Azure infrastrukture.

Sigurnosno kopiranje proširenje potrebna je veza s Azure javnu IP adrese da biste pravilno funkcionirati. To je zato šalje naredbe krajnje Azure prostora za pohranu (HTTP URL) za upravljanje snimkama na VM. Ako proširenje ima pristup javnog Interneta, sigurnosno kopiranje ipak neće uspjeti.

### <a name="solution"></a>Rješenja
U ovom scenariju, primijenite jednu od sljedećih načina da biste riješili taj problem:

- Rasponi IP Whitelist Azure podatkovnim centrom
- Stvorite put HTTP promet na tijek pločica

### <a name="to-whitelist-the-azure-datacenter-ip-ranges"></a>Da biste whitelist na Azure podatkovnog centra IP rasponi

1. Nabavite [popis Azure podatkovnog centra IP-ovi](https://www.microsoft.com/download/details.aspx?id=41653) biti whitelisted.
2. Pomoću cmdleta New-NetRoute deblokirati u IP-ovi. Pokrenite ovaj cmdlet VM Azure u dodatnim PowerShell prozoru (Pokreni kao Administrator).
3. Dodavanje pravila da biste na mreži sigurnosnih grupa (NSG) ako nemate dopustili pristup na IP-ovi.

### <a name="to-create-a-path-for-http-traffic-to-flow"></a>Da biste stvorili put za HTTP promet na tijek pločica

1. Ako imate ograničenja mreže na mjestu (na primjer, NSG), implementirati HTTP proxy poslužitelja da biste usmjerili promet.
2. Ako imate mrežni sigurnosne grupe (NSG), dodajte pravila dopustili pristup Internetu iz HTTP proxy.

Saznajte kako [postaviti HTTP proxy sigurnosnih kopija VM](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## <a name="cause-2-the-microsoft-azure-vm-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Uzrok 2: Microsoft Azure VM agent instalirali u drugom na VM zastario (za Linux VMs)

### <a name="solution"></a>Rješenja
Problemi koji utječu na agent staru VM je uzrokovano najčešće vezane uz agent ili vezane uz proširenje pogrešaka za Linux VMs. Kao Općenito smjernica, prve korake da biste riješili taj problem se sljedeće:

1. [Instalacija najnovije agent Azure VM](https://github.com/Azure/WALinuxAgent).
2. Provjerite je li na na VM sustavom agent za Azure. Da biste to učinili, pokrenite sljedeću naredbu:```ps -e```

    Ako taj proces nije pokrenut, koristite sljedeće naredbe da biste ga ponovno pokrenite.

    Za Ubuntu:```service walinuxagent start```

    Za ostale distribucija: "" servisa waagent start
```

3. [Configure the auto restart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).

4. Run a new test backup. If the failures persist, please collect logs from the following folders for further debugging.

    We require the following logs from the customer’s VM:

    - /var/lib/waagent/*.xml
    - /var/log/waagent.log
    - /var/log/azure/*

If we require verbose logging for waagent, follow these steps to enable this:

1. In the /etc/waagent.conf file, locate the following line:

    Enable verbose logging (y|n)

2. Change the **Logs.Verbose** value from n to y.
3. Save the change, and then restart waagent by following the previous steps in this section.

## Cause 3: The backup extension fails to update or load
If extensions cannot be loaded, then Backup fails because a snapshot cannot be taken.

### Solution
For Windows guests:

1. Verify that the iaasvmprovider service is enabled and has a startup type of automatic.
2. If this is not the configuration, enable the service to determine whether the next backup succeeds.

For Linux guests:

The latest version of VMSnapshot Linux (extension used by backup) is 1.0.91.0

If the backup extension still fails to update or load, you can force the VMSnapshot extension to be reloaded by uninstalling the extension. The next backup attempt will reload the extension.

### To uninstall the extension

1. Go to the [Azure portal](https://portal.azure.com/).
2. Locate the particular VM that has backup problems.
3. Click **Settings**.
4. Click **Extensions**.
5. Click **Vmsnapshot Extension**.
6. Click **uninstall**.

This will cause the extension to be reinstalled during the next backup.

## Cause 4: The snapshots status cannot be retrieved or the snapshots cannot be taken
VM backup relies on issuing snapshot command to underlying storage. The backup can fail because it has no access to storage or because of a delay in snapshot task execution.

### Solution
The following conditions can cause snapshot task failure:

| Cause | Solution |
| ----- | ----- |
| VMs that have Microsoft SQL Server Backup configured. By default, VM Backup runs a VSS Full backup on Windows VMs. On VMs that are running SQL Server-based servers and on which SQL Server Backup is configured, snapshot execution delays may occur. | Set following registry key if you are experiencing backup failures because of snapshot issues.<br><br>[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE" |
| VM status is reported incorrectly because the VM is shut down in RDP. If you shut down the virtual machine in RDP, check the portal to determine whether that VM status is reflected correctly. | If it’s not, shut down the VM in the portal by using the ”Shutdown” option in the VM dashboard. |
| Many VMs from the same cloud service are configured to back up at the same time. | It’s a best practice to spread out the VMs from the same cloud service to have different backup schedules. |
| The VM is running at high CPU or memory usage. | If the VM is running at high CPU usage (more than 90 percent) or high memory usage, the snapshot task is queued and delayed and eventually times out. In this situation, try on-demand backup. |
|The VM cannot get host/fabric address from DHCP.|DHCP must be enabled inside the guest for IaaS VM Backup to work.  If the VM cannot get host/fabric address from DHCP response 245, then it cannot download ir run any extensions. If you need a static private IP, you should configure it through the platform. The DHCP option inside the VM should be left enabled. View more information about [Setting a Static Internal Private IP](../virtual-network/virtual-networks-reserved-private-ip.md).|
