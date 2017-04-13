<properties
 pageTitle="Upravljanje HPC paket klaster računalnim čvorove | Microsoft Azure"
 description="Saznajte više o alatima skriptu PowerShell za dodavanje, uklanjanje, pokretanje i zaustavljanje HPC paket klaster računalnim čvorove servisu Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Upravljanje broja i dostupnost računalnim čvorove u programa paketa HPC klaster servisu Azure

Ako ste stvorili programa paketa HPC klaster u Azure VMs, bilo bi dobro načini jednostavno dodavanje, uklanjanje, pokrenite (dodjele) ili prekidanje (deprovision) broj računalnim čvor VMs u klasteru. Da biste to učinili, pokrenite skripte Azure PowerShell koji su instalirani na glavni čvor VM. Te skripte vam pomoći odrediti broj i dostupnost resursa klaster HPC paket da biste mogli odrediti troškove.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Preduvjeti

* **Paket HPC klaster u Azure VMs** – stvaranje programa paketa HPC klaster u modelu uvođenje klasičnog pomoću barem HPC Pack 2012 R2 Update 1. Ako, na primjer, možete automatizirati implementacijskih pomoću trenutnu sliku HPC paket VM trgovine Windows Azure i skripte Azure PowerShell. Informacije i preduvjete potražite u članku [Stvaranje programa klasteru HPC s implementacijsku skriptu HPC paket IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

    Nakon implementacije, pronađite čvor upravljanje skripte % CCP\_POLAZNA mapa za smeće % na glavni čvor. Morate pokrenuti svaki skripte kao administrator.

* **Azure objavljivanje postavke web-mjesto datoteke ili u okvir za upravljanje certifikat** – što trebate učiniti nešto od sljedećeg na glavni čvor:

    * **Uvoz u Azure objavljivanje datoteka postavki**. Da biste to učinili, pokrenite sljedeće Cmdlete ljuske PowerShell Azure na glavni čvor:

    ```
    Get-AzurePublishSettingsFile

    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```

    * **Konfiguriranje certifikata Azure upravljanja na glavni čvor**. Ako imate .cer datoteka, uvoz u spremištu certifikata CurrentUser\My, a zatim pokrenite sljedeći cmdlet Azure PowerShell okruženju sustava Azure (AzureCloud ili AzureChinaCloud):

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Dodavanje čvora računalnim VMs

Dodaj izračunati čvorove sa scenarijem **Dodaj HpcIaaSNode.ps1** .

### <a name="syntax"></a>Sintaksa
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parametri

* **Naziv servisa** - naziv oblaka servisa VMs dodat će se u taj novi računalnim čvor.

* **ImageName** - naziv Azure VM slike koje možete dobiti kroz Azure klasični portal ili Azure PowerShell cmdlet **Get-AzureVMImage**. Slika mora zadovoljavati sljedeće uvjete:

    1. Mora biti instaliran operacijski sustav Windows.

    2. Paket HPC mora biti instaliran u ulozi čvor računalnim.

    3. Slika mora biti privatne slike u kategoriji korisnik ne javno Azure VM sliku.

* **Količina** - broj računalnim čvor VMs potrebno dodati.

* **InstanceSize** - veličina čvora računalnim VMs.

* **DomainUserName** - domena korisničko ime koje će se koristiti da biste se pridružili novi VMs domeni.

* **DomainUserPassword** - lozinku za korisnika domene.

* **NodeNameSeries** (neobavezno) – imenovanja uzorka za čvorove računalnim. Mora biti u obliku &lt; *korijenski\_naziv*&gt;&lt;*pokretanje\_broj*&gt;%. Na primjer, MyCN srednje vrijednosti 10% % niz od naziva čvor računalnim počevši od MyCN11. Ako nije naveden, skripta koristi čvor konfigurirani imenovanja niz klaster HPC.

### <a name="example"></a>Primjer

Sljedeći primjer zbraja 20 veličina velike računalnim čvor VMs u u oblak servisa *hpcservice1*, ovisno o slika VM *hpccnimage1*.

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Uklanjanje računalnim čvor VMs

Uklanjanje računalnim čvorovi sa scenarijem **Ukloni HpcIaaSNode.ps1** .

### <a name="syntax"></a>Sintaksa

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parametri

* **Ime** - imena klaster čvorove koje će biti uklonjene. Podržane su zamjenskih znakova. Naziv parametra postavljanje je naziv. Ne možete navesti **naziv** i **čvor** parametara.

* **Čvor** - objekta u HpcNode za čvorove koje će biti uklonjene koje je moguće dohvatiti putem komponente PowerShell HPC cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Postavljanje naziv parametra je čvor. Ne možete navesti **naziv** i **čvor** parametara.

* **DeleteVHD** (neobavezno) – postavljanje da biste izbrisali pridružene diskova za VMs koje se uklanjaju.

* **Prisilno** (neobavezno) – postavljanje da biste nametnuli HPC čvorove izvanmrežno prije nego što ih uklonite.

* **Potvrda** (neobavezno) – Pitaj za potvrdu prije izvođenja naredbe.

* **WhatIf** – postavljanje opisuje što će se dogoditi ako izvršiti naredbu bez zapravo izvodi naredbu.

### <a name="example"></a>Primjer

U sljedećem primjeru navodi izvanmrežne čvorove s nazivima početak *HPCNode-CN -* i ih uklanja čvorove i njihove pridružene diskova.

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Pokretanje računalnim čvor VMs

Započnite računalnim čvorove skripte **Start HpcIaaSNode.ps1** .

### <a name="syntax"></a>Sintaksa

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parametri

* **Ime** - imena čvorove klaster da se pokreće. Podržane su zamjenskih znakova. Postavljanje naziv parametra je naziv. Ne možete navesti **naziv** i **čvor** parametara.

* **Čvor**- objekta u HpcNode za čvorove da se pokreće, koji možete preuzeti putem komponente PowerShell HPC cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Naziv parametra postavljanje je čvor. Ne možete navesti **naziv** i **čvor** parametara.

### <a name="example"></a>Primjer

U sljedećem primjeru započinje čvorovi naziva koji započinju *HPCNode-CN -*.

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Zaustavljanje računalnim čvor VMs

Zaustavite računalnim čvorovi sa scenarijem **Zaustavi HpcIaaSNode.ps1** .

### <a name="syntax"></a>Sintaksa

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parametri


* **Ime**- imena čvorove klaster da biste se zaustaviti. Podržane su zamjenskih znakova. Postavljanje naziv parametra je naziv. Ne možete navesti **naziv** i **čvor** parametara.

* **Čvor** - objekta u HpcNode za čvorove da biste prekinuli, koji možete preuzeti putem komponente PowerShell HPC cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Postavljanje naziv parametra je čvor. Ne možete navesti **naziv** i **čvor** parametara.

* **Prisilno** (neobavezno) – postavljanje da biste nametnuli HPC čvorove izvanmrežno prije zaustavljanja ih.

### <a name="example"></a>Primjer

U sljedećem primjeru navodi izvanmrežne čvorove s nazivima početak *HPCNode-CN -* , a zatim prestati čvorove.

Zaustavi HPCIaaSNode.ps1 – naziv HPCNodeCN-* – prisilno

## <a name="next-steps"></a>Daljnji koraci

* Ako želite da se automatski Povećaj ili Smanji čvorove klaster prema trenutni radno opterećenje zadataka i zadataka na klaster, potražite u članku [automatski Povećaj i Smanji resursa klaster HPC paketa u Azure prema radno opterećenje klaster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).
