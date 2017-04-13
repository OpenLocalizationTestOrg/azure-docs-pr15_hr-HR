<properties
    pageTitle="Kliknite pločicu Zaštita na krajnjoj točki Symantec na VM | Microsoft Azure"
    description="Saznajte kako instalirati i konfigurirati proširenje Zaštita na krajnjoj točki Symantec sigurnosti na u novu ili postojeću Azure VM stvorene pomoću model klasični implementacije."
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

# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Kako instalirati i konfigurirati Zaštita na krajnjoj točki Symantec na VM za Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

U ovom se članku prikazuje kako instalirati i konfigurirati klijenta Zaštita na krajnjoj točki Symantec na programa postojeće virtualnog računala (VM) sa sustavom Windows Server. Ovo je punog klijentskog koji obuhvaća servise kao što su virusa i zaštitu špijunskog softvera, vatrozida i sprječavanje podataka. Klijent je instaliran kao nastavkom sigurnost pomoću VM Agent.

Ako imate postojeću pretplatu tvrtke Symantec za lokalno rješenje, možete je koristiti da biste zaštitili Azure virtualnim računalima. Ako niste klijenta, možete se prijaviti za probnu pretplatu. Dodatne informacije o rješenje potražite u članku [Zaštita na krajnjoj točki Symantec na tvrtke Microsoft Azure platformi][Symantec]. Ova stranica sadrži i veze na informacije o licenciranju i upute za instalaciju klijent ako ste već Symantec klijenta.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Kliknite pločicu Zaštita na krajnjoj točki Symantec postojeće VM

Prije početka, potrebne su vam sljedeće:

- Modul ljuske PowerShell Azure, verzija 0.8.2 ili noviju verziju, na računalo. Možete provjeriti verziju Azure PowerShell koju ste instalirali s na **Get-modul azure | oblikovanje tablice verzija** naredbe. Upute i veze na najnoviju verziju, potražite [u]članku instalacija i konfiguriranje Azure PowerShell[PS]. Prijavite se u sustav Azure pretplatu pomoću `Add-AzureAccount`.

- Agent VM izvodi na Azure virtualnog računala.

Najprije provjerite je li VM Agent već instaliran na virtualnog računala. Unesite naziv usluge oblaka i naziv virtualnog računala, a zatim pokrenite sljedeće naredbe u naredbenom retku Azure PowerShell administratorske. Zamijenite sve unutar navodnika, uključujući na < i > znakova.

> [AZURE.TIP] Ako ne znate u oblaku i nazive virtualnog računala, pokrenite **Get-AzureVM** popis imena za sve virtualnim strojevima trenutne pretplate.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Naredba za **Pisanje host** prikazuje **True**, VM Agent je li instaliran. Prikazuje **False**, potražite u članku upute i veze za preuzimanje Azure blogu objavljuju [VM Agent i proširenja - 2 dio][Agent].

Ako je instaliran VM Agent pokrenuli te naredbe da biste instalirali agent Symantec Zaštita na krajnjoj točki.

    $Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

    Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

Da biste potvrdili sigurnost proširenje Symantec je instaliran, a ažuran:

1.  Prijavite se na virtualnog računala. Upute potražite u članku [kako se prijaviti u virtualnog računala izvodi Windows Server][Logon].
2.  Windows Server 2008 R2, kliknite **Start > Zaštita na krajnjoj točki Symantec**. Za Windows Server 2012 ili Windows Server 2012 R2, na početnom zaslonu upišite **Symantec**, a zatim **Symantec Zaštita na krajnjoj točki**.
3.  Na kartici **Stanje** prozoru **Zaštita na krajnjoj točki Status Symantec** Primjena ažuriranja ili ponovno pokrenite prema potrebi.

## <a name="additional-resources"></a>Dodatni resursi

[Kako se prijaviti u virtualnog računala sa sustavom Windows Server][Logon]

[Azure VM proširenja i značajke][Ext]


<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Portal]: http://manage.windowsazure.com

[Create]: virtual-machines-windows-classic-tutorial.md

[PS]: ../powershell-install-configure.md

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]: virtual-machines-windows-classic-connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493