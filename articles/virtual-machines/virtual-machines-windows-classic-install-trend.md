<properties
    pageTitle="Kliknite pločicu Trend Micro precizno sigurnosti na VM | Microsoft Azure"
    description="U ovom se članku opisuje kako instalirati i konfigurirati Trend Micro sigurnosti na VM stvorene pomoću model klasični implementacije u Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="iainfou"/>


# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Kako instalirati i konfigurirati Trend Micro precizno sigurnost kao servisa na VM za Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

U ovom se članku objašnjava instaliranja i konfiguriranja Trend Micro precizno sigurnost kao servisa na novom ili postojećem virtualni stroj (VM) sa sustavom Windows Server. Precizno sigurnost kao servis sadrži Zaštita od zlonamjernog softvera, vatrozida, sustav sprječavanje podataka i nadzor integritet.

Klijent instaliran je kao nastavkom sigurnost putem VM Agent. Novi virtualnog računala, instalirajte Agent VM uz precizno Agent za sigurnost. Na programa postojeće virtualnog računala koja ne sadrži VM Agent, morate preuzmite i instalirajte ga prvi put. U ovom se članku opisuje obiju situacija.

Ako imate postojeću pretplatu na Trend Micro za lokalno rješenje, možete je koristiti da biste zaštitili Azure virtualnih računala. Ako niste klijenta, možete se prijaviti za probnu pretplatu. Dodatne informacije o rješenje potražite u članku na Trend Micro blogu [Microsoft Azure VM Agent proširenja za precizno sigurnost](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Instalacija precizno Agent za sigurnost na novu VM

[Azure klasični portal](http://manage.windowsazure.com) možete instalirati VM Agent i nastavak sigurnost Trend Micro kada koristite mogućnost **Iz galerije** stvaranje virtualnog računala. Ako stvarate jedan virtualnog računala, pomoću portala za je jednostavan način za dodavanje zaštite od Trend Micro.

Ta mogućnost **Iz galerije** otvorit će se čarobnjak koji omogućuje postavljanje virtualnog računala. Koristite zadnjoj stranici čarobnjaka za instalaciju VM Agent i nastavak Trend Micro sigurnost. Općenite upute potražite u članku [Stvaranje virtualnog računala sa sustavom Windows Azure klasični portalu](virtual-machines-windows-classic-tutorial.md). Kada dođete na zadnju stranicu čarobnjaka, učinite sljedeće:

1.  U odjeljku **VM Agent**potvrdite **Instalirati VM Agent**.

2.  U odjeljku **Sigurnost proširenja**potvrdite **Trend Micro precizno Agent sigurnost**.

    ![Instalirajte VM Agent i Agent za precizno sigurnosti](./media/virtual-machines-windows-classic-install-trend/InstallVMAgentandTrend.png)

3.  Kliknite kvačicu da biste stvorili virtualnog računala.

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Instalacija precizno Agent za sigurnost na postojeće VM

Da biste instalirali agenta na postojeće VM, potrebno je sljedeće:

- Modul ljuske PowerShell Azure, verzija 0.8.2 ili noviji, instaliranih na lokalnom računalu. Možete provjeriti verziju Azure PowerShell koju ste instalirali pomoću na **Get-modul azure | oblikovanje tablice verzija** naredbe. Upute i veze na najnoviju verziju, potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md). Prijavite se u sustav Azure pretplatu pomoću `Add-AzureAccount`.

- Agent VM instalirana na ciljnom virtualnog računala.

Najprije provjerite VM Agent već je instaliran. Unesite naziv usluge oblaka i naziv virtualnog računala, a zatim pokrenite sljedeće naredbe u naredbenom retku Azure PowerShell administratorske. Zamijenite sve unutar navodnika, uključujući na < i > znakova.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Ako ne znate u oblaku i naziv virtualnog računala, pokrenite **Get-AzureVM** da biste prikazali te podatke za sve virtualnim strojevima u trenutne pretplate.

Ako je naredba **Pisanje host** vraća **True**, VM Agent je instaliran. Vraća **False**, pogledajte upute i veze za preuzimanje Azure blogu objavljuju [VM Agent i proširenja – dio 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Ako je instaliran VM Agent pokrenuli te naredbe.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Daljnji koraci

U svega nekoliko minuta agent pokretanja kada je instaliran. Nakon toga, potrebno je aktivirati precizno sigurnosti na virtualnog računala tako da ga može upravljati na niže sigurnost Upravitelj. Pogledajte sljedeće dodatne upute:

- Trend na članak o rješenja, [Instant-On oblaka sigurnosti za Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
- [Primjer skripte komponente Windows PowerShell](http://go.microsoft.com/fwlink/?LinkId=404100) za konfiguriranje virtualnog računala
- [Upute](http://go.microsoft.com/fwlink/?LinkId=404099) za uzorak

## <a name="additional-resources"></a>Dodatni resursi

[Kako se prijaviti na virtualnog računala sa sustavom Windows Server]

[Azure VM proširenja i značajke]


<!--Link references-->
[Kako se prijaviti na virtualnog računala sa sustavom Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Azure VM proširenja i značajke]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
