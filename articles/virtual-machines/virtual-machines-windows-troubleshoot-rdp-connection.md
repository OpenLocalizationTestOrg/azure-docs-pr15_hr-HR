<properties
    pageTitle="Nije moguće RDP za Azure VM | Microsoft Azure"
    description="Otklanjanje poteškoća s ne može povezati s Windows virtualnog računala u Azure putem udaljene radne površine"
    keywords="Udaljene radne površine pogrešku, pogreška udaljene radne površine, ne može povezati s VM udaljene radne površine otklanjanje poteškoća"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/26/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Otklanjanje poteškoća s vezama udaljene radne površine za Azure virtualnog računala

Remote Desktop Protocol (RDP) vezu na utemeljen na sustavu Windows Azure virtualnog računala (VM) uspijeva različitih razloga ostavite koje nije moguće pristupiti vašem VM. Problem se može biti sa servisu udaljene radne površine na na VM, mrežne veze ili klijent udaljene radne površine na glavnom računalu. U ovom se članku vas vodi kroz neke od najčešćih metode rješavanja problema s povezivanjem RDP. 

Ako vam je potrebna dodatna pomoć u ovom članku u bilo kojem trenutku, možete se obratiti Azure stručnjaka na [MSDN Azure i stoga prelijevanje forume](https://azure.microsoft.com/support/forums/). Osim toga, možete arhivirati incident Azure podršci. Idite na [Azure podržava web-mjesta](https://azure.microsoft.com/support/options/) i odaberite **Početak podržava**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Brzi koraci za otklanjanje poteškoća
Nakon svakog koraka za otklanjanje poteškoća, pokušajte ponovno s VM:

1. Ponovno postavljanje konfiguracije udaljene radne površine.
2. Sigurnosne grupe Provjera mrežom pravila / Cloud Services krajnje točke.
3. Pregledajte VM konzole zapisnike.
4. Provjera stanja resursa VM.
5. Ponovno postavljanje lozinke VM.
6. Ponovno pokrenite vaše VM.
7. Ponovno implementirate vaše VM.

Ako trebate više detaljne upute i objašnjenja i dalje čitanje.

> [AZURE.TIP] Ako je gumb **za povezivanje** za vaše VM zasivljena na portalu, a niste povezani s Azure preko veze [Express usmjeravanje](../expressroute/expressroute-introduction.md) ili [VPN-a web-mjesto](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) , morate je stvaranje i dodjela vaše VM javnu IP adresu mogli koristiti RDP. Dodatne informacije o [javnim IP adresa u Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-to-troubleshoot-rdp-issues"></a>Načini otklanjanje poteškoća RDP
Otklonite VMs stvoren pomoću model implementacije resursima pomoću neke od sljedećih načina:

- [Portal za azure](#using-the-azure-portal) - sjajno Ako morate brzo ponovno postavljanje RDP vjerodajnice konfiguraciju ili korisnika, a nemate instalirane alate za Azure.
- [Azure PowerShell](#using-azure-powershell) – ako ste upoznati s odzivniku komponente PowerShell sustava brzo ponovno postaviti RDP konfiguraciju ili korisnik vjerodajnice pomoću cmdleta ljuske PowerShell Azure.

Možete pronaći i upute za otklanjanje poteškoća s VMs stvoren pomoću [Klasični model implementacije](#troubleshoot-vms-created-using-the-classic-deployment-model).


<a id="fix-common-remote-desktop-errors"></a>
## <a name="troubleshoot-using-the-azure-portal"></a>Rješavanje problema pomoću portala za Azure
Nakon svakog koraka za otklanjanje poteškoća, pokušajte ponovno povezati svoje VM. Ako se i dalje ne možete povezati, pokušajte sa sljedećim korakom.

1. **Ponovno postavljanje RDP veza**. Ovaj korak za otklanjanje poteškoća vraća konfiguracije RDP kada su onemogućene daljinske veze ili pravila vatrozida za Windows je blokirao RDP, na primjer.

    Odaberite vaše VM na portalu za Azure. Pomaknite se prema dolje u oknu postavke u odjeljku **podrška + otklanjanje poteškoća** pri dnu popisa. Kliknite gumb **Vrati izvornu lozinku** . Postavljanje **način** da biste **ponovno postavili samo konfiguracija** , a zatim kliknite gumb **Ažuriraj** :

    ![Ponovno postavljanje konfiguracije RDP na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-rdp.png)

2. **Provjerite je li mrežni sigurnosne grupe pravila**. Ovaj korak za otklanjanje poteškoća provjerava postoji pravilo u mreži sigurnosnoj grupi dopušta promet RDP. Zadani je priključak za RDP je TCP priključak 3389. Pravilo da dopušta promet RDP možda nije moguće automatski kada stvarate vaše VM.

    Odaberite vaše VM na portalu za Azure. Kliknite **sučelje mreže** iz okna postavke.

    ![Prikaz sučelje mreže za VM Azure portalu](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-network-interfaces.png)

    Na popisu (nema obično samo jedan) odaberite mrežnog sučelja:

    ![Odaberite mrežno sučelje na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-interface.png)

    Odaberite **mrežni sigurnosne grupe** da biste pogledali sigurnosne grupe mreže povezane s mrežnog sučelja:

    ![Odaberite mrežni sigurnosne grupe na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-nsg.png)

    Provjerite postoji li ulazna pravila koja omogućuje RDP promet na TCP priključak 3389. Sljedeći primjer prikazuje valjanih sigurnosnih pravila koja omogućuje RDP promet. Vidjet ćete `Service` i `Action` ispravno konfigurirano:

    ![Provjerite je li RDP NSG pravilo na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/verify-nsg-rules.png)

    Ako nemate pravila koji omogućuje RDP promet, [stvorite pravilo mrežom sigurnosne grupe](virtual-machines-windows-nsg-quickstart-portal.md). Dopusti TCP priključak 3389.

3. **Pregled VM Pokretanje dijagnostike**. Ovaj korak za otklanjanje poteškoća pregledava zapisnike VM konzole za određivanje ako je na VM prijavljivanje problema. Svi VMs sadrži Pokretanje dijagnostike omogućen, pa će se ovaj korak za otklanjanje poteškoća možda nije obavezno.
    
    Određene korake za otklanjanje poteškoća su izvan raspona ovog članka, ali mogu upućivati širem problem koji je utjecaja RDP povezivanje. Dodatne informacije o pregleda zapisnika konzole i VM snimka, potražite u članku [Pokretanje Dijagnostika za VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **Provjera stanja resursa VM**. Ovaj korak za otklanjanje poteškoća provjerava postoje bez Poznati problemi vezani uz platforme Azure koji mogu utjecati na veza s u VM.

    Odaberite vaše VM na portalu za Azure. Pomaknite se prema dolje u oknu postavke u odjeljku **podrška + otklanjanje poteškoća** pri dnu popisa. Kliknite gumb **stanja resursa** . Dobar VM izvješća kao **dostupno**:

    ![Provjera stanja resursa VM na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/check-resource-health.png)

5. **Ponovno postavljanje korisničke vjerodajnice**. Ovaj korak za otklanjanje poteškoća vraća lozinku na lokalni administratorski račun ako sigurni ili ako ste zaboravili vjerodajnice.

    Odaberite vaše VM na portalu za Azure. Pomaknite se prema dolje u oknu postavke u odjeljku **podrška + otklanjanje poteškoća** pri dnu popisa. Kliknite gumb **Vrati izvornu lozinku** . Provjerite je li u **načinu rada** postavljen na **ponovno postavljanje lozinke** , a zatim unesite svoje korisničko ime i novu lozinku. Na kraju, kliknite gumb **Ažuriraj** :

    ![Ponovno postavljanje korisničke vjerodajnice na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-password.png)

6. **Ponovno pokrenite vaše VM**. Ovaj korak za otklanjanje poteškoća možete ispraviti probleme podlozi VM sam ima.

    Odaberite vaše VM na portalu za Azure, a zatim kliknite karticu **Pregled** . Kliknite gumb **ponovno pokrenuti** :

    ![Ponovno pokrenite na VM na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/restart-vm.png)

7. **Ponovno implementirate vaše VM**. Ovaj korak za otklanjanje poteškoća redeploys vaše VM glavno računalo za drugi unutar Azure da biste ispravili sve podlozi platformu ili mrežnih problema.

    Odaberite vaše VM na portalu za Azure. Pomaknite se prema dolje u oknu postavke u odjeljku **podrška + otklanjanje poteškoća** pri dnu popisa. Kliknite gumb **ponovno implementirate** , a zatim **ponovno implementirate**:

    ![Ponovno implementirate VM na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/redeploy-vm.png)

    Po završetku ovaj postupak gubi se efemerni disk podataka i dinamičke IP adrese koje su vezane uz na VM ažuriraju.

Ako i dalje su nailazi na RDP problema, možete [otvorite zahtjev za podršku](https://azure.microsoft.com/support/options/) ili pročitajte [detaljnije otklanjanje poteškoća koncepata RDP i korake](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-using-azure-powershell"></a>Rješavanje problema s korištenjem Azure PowerShell
Ako to već niste učinili, [instalirate i konfigurirate najnovije Azure PowerShell](../powershell-install-configure.md).

U sljedećem se primjeru koristi varijable kao što su `myResourceGroup`, `myVM`, i `myVMAccessExtension`. Ove varijable naziva i mjesta zamijenite vlastitim vrijednosti.

> [AZURE.NOTE] Vraćanje web-mjesto korisničke vjerodajnice i u okvir za konfiguraciju RDP pomoću cmdleta komponente PowerShell [Skup AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) . U sljedećim primjerima `myVMAccessExtension` je naziv koji ste naveli kao dio postupka. Ako ste prethodno radili s u VMAccessAgent, možete dobiti naziv postojeće proširenje pomoću `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` da biste provjerili svojstva na VM. Da biste vidjeli naziv, pogledajte u odjeljku 'Proširenja' izlaza.

Nakon svakog koraka za otklanjanje poteškoća, pokušajte ponovno povezati svoje VM. Ako se i dalje ne možete povezati, pokušajte sa sljedećim korakom.

1. **Ponovno postavljanje RDP veza**. Ovaj korak za otklanjanje poteškoća vraća konfiguracije RDP kada su onemogućene daljinske veze ili pravila vatrozida za Windows je blokirao RDP, na primjer.

    U sljedećem primjeru vraća RDP veza na VM pod nazivom `myVM` u na `WestUS` mjesto i u grupi resursa pod nazivom `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```

2. **Provjerite je li mrežni sigurnosne grupe pravila**. Ovaj korak za otklanjanje poteškoća provjerava postoji pravilo u mreži sigurnosnoj grupi dopušta promet RDP. Zadani je priključak za RDP je TCP priključak 3389. Pravilo da dopušta promet RDP možda nije moguće automatski kada stvarate vaše VM.

    Najprije dodijelite sve podatke za konfiguraciju za mrežni sigurnosne grupe da biste na `$rules` varijabli. U sljedećem primjeru dohvaća informacije o mreži sigurnosne grupe s nazivom `myNetworkSecurityGroup` u grupu resursa pod nazivom `myResourceGroup`:

    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```

    Sada prikazati pravila koja su konfigurirana za mrežni sigurnosne grupe. Provjerite postoji li pravilo da biste omogućili TCP priključak 3389 dolaznih veza na sljedeći način:

    ```powershell
    $rules.SecurityRules
    ```

    Sljedeći primjer prikazuje valjanih sigurnosnih pravila koja omogućuje RDP promet. Vidjet ćete `Protocol`, `DestinationPortRange`, `Access`, a `Direction` ispravno konfigurirano:

    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```

    Ako nemate pravila koji omogućuje RDP promet, [stvorite pravilo mrežom sigurnosne grupe](virtual-machines-windows-nsg-quickstart-powershell.md). Dopusti TCP priključak 3389.

3. **Ponovno postavljanje korisničke vjerodajnice**. Ovaj korak za otklanjanje poteškoća vraća lozinku na lokalni administratorski račun koji navedete kada još uvijek niste sigurni ili ste zaboravili vjerodajnice.

    Najprije navedite korisničko ime i novu lozinku dodjelom vjerodajnice da bi se `$cred` varijabla na sljedeći način:

    ```powershell
    $cred=Get-Credential
    ```

    Sada, ažurirajte vjerodajnice na vašem VM. U sljedećem primjeru ažurira vjerodajnice na VM pod nazivom `myVM` u na `WestUS` mjesto i u grupi resursa pod nazivom `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```

4. **Ponovno pokrenite vaše VM**. Ovaj korak za otklanjanje poteškoća možete ispraviti probleme podlozi VM sam ima.

    U sljedećem primjeru pokreće VM pod nazivom `myVM` u grupu resursa pod nazivom `myResourceGroup`:

    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```

5. **Ponovno implementirate vaše VM**. Ovaj korak za otklanjanje poteškoća redeploys vaše VM glavno računalo za drugi unutar Azure da biste ispravili sve podlozi platformu ili mrežnih problema.

    U sljedećem primjeru redeploys VM pod nazivom `myVM` u na `WestUS` mjesto i u grupi resursa pod nazivom `myResourceGroup`:

    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Ako i dalje su nailazi na RDP problema, možete [otvorite zahtjev za podršku](https://azure.microsoft.com/support/options/) ili pročitajte [detaljnije otklanjanje poteškoća koncepata RDP i korake](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Otklanjanje poteškoća s VMs stvoren pomoću model implementacije Classic

Nakon svakog koraka za otklanjanje poteškoća, pokušajte ponovno s VM.

1. **Ponovno postavljanje RDP veza**. Ovaj korak za otklanjanje poteškoća vraća konfiguracije RDP kada su onemogućene daljinske veze ili pravila vatrozida za Windows je blokirao RDP, na primjer.

    Odaberite vaše VM na portalu za Azure. Kliknite na **... Dodatne** gumba, a zatim kliknite **Vrati izvorno daljinski pristup**:

    ![Ponovno postavljanje konfiguracije RDP na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-rdp.png)

2.  **Provjerite je li servisi u Oblaku za krajnje točke**. Ovaj korak za otklanjanje poteškoća provjerava imate krajnje točke u oblak servisa da dopušta promet RDP. Zadani je priključak za RDP je TCP priključak 3389. Pravilo da dopušta promet RDP možda nije moguće automatski kada stvarate vaše VM.

    Odaberite vaše VM na portalu za Azure. Kliknite gumb **krajnje točke** da biste pogledali krajnje točke trenutno postavljenima za vaše VM. Provjerite da postoje krajnje točke koje omogućuju RDP promet na TCP priključak 3389.
    
    Sljedeći primjer prikazuje valjani krajnje točke dopušta promet RDP:

    ![Provjerite je li krajnje točke servise u Oblaku na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)

    Ako nemate krajnje koji omogućuje RDP promet [stvorili krajnju točku za servise u Oblaku](virtual-machines-windows-classic-setup-endpoints.md). Dopustite TCP privatne priključak 3389.

3. **Pregled VM Pokretanje dijagnostike**. Ovaj korak za otklanjanje poteškoća pregledava zapisnike VM konzole za određivanje ako je na VM prijavljivanje problema. Svi VMs sadrži Pokretanje dijagnostike omogućen, pa će se ovaj korak za otklanjanje poteškoća možda nije obavezno.
    
    Određene korake za otklanjanje poteškoća su izvan raspona ovog članka, ali mogu upućivati širem problem koji je utjecaja RDP povezivanje. Dodatne informacije o pregleda zapisnika konzole i VM snimka, potražite u članku [Pokretanje Dijagnostika za VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **Provjera stanja resursa VM**. Ovaj korak za otklanjanje poteškoća provjerava postoje bez Poznati problemi vezani uz platforme Azure koji mogu utjecati na veza s u VM.

    Odaberite vaše VM na portalu za Azure. Pomaknite se prema dolje u oknu postavke u odjeljku **podrška + otklanjanje poteškoća** pri dnu popisa. Kliknite gumb **Stanja resursa** . Dobar VM izvješća kao **dostupno**:

    ![Provjera stanja resursa VM na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-check-resource-health.png)

5. **Ponovno postavljanje korisničke vjerodajnice**. Ovaj korak za otklanjanje poteškoća vraća lozinku na lokalni administratorski račun koji navedete kada niste sigurni ili ste zaboravili vjerodajnice.

    Odaberite vaše VM na portalu za Azure. Pomaknite se prema dolje u oknu postavke u odjeljku **podrška + otklanjanje poteškoća** pri dnu popisa. Kliknite gumb **Vrati izvornu lozinku** . Unesite svoje korisničko ime i novu lozinku. Na kraju, kliknite gumb **Spremi** :

    ![Ponovno postavljanje korisničke vjerodajnice na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-password.png)

6. **Ponovno pokrenite vaše VM**. Ovaj korak za otklanjanje poteškoća možete ispraviti probleme podlozi VM sam ima.

    Odaberite vaše VM na portalu za Azure, a zatim kliknite karticu **Pregled** . Kliknite gumb **ponovno pokrenuti** :

    ![Ponovno pokrenite na VM na portalu za Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-restart-vm.png)
    
Ako i dalje su nailazi na RDP problema, možete [otvorite zahtjev za podršku](https://azure.microsoft.com/support/options/) ili pročitajte [detaljnije otklanjanje poteškoća koncepata RDP i korake](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-specific-rdp-errors"></a>Otklanjanje poteškoća s konkretnim pogreškama RDP
Možda ćete naići na određenu poruku o pogrešci prilikom pokušaja povezivanja s vašeg VM putem RDP. Ovo su najčešće poruke pogreške:

- [Udaljena sesija prekinuta jer nema udaljene radne površine licence poslužiteljima koji daju te licence](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdplicense).
- [Udaljena radna površina ne možete pronaći računalo "naziv"](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpname).
- Došlo je do pogreške [za provjeru autentičnosti. Ne možete uspostaviti lokalnoj ustanovi za sigurnost](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpauth).
- [Sigurnost sustava Windows pogreška: nije uspjelo vjerodajnice](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#wincred).
- [Ovo računalo ne može povezati s udaljenog računala](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpconnect).


## <a name="additional-resources"></a>Dodatni resursi
Ako nijedan od tih pogrešaka došlo je do, a i dalje ne možete povezati VM putem udaljene radne površine, pročitajte detaljne [Vodič za udaljene radne površine za otklanjanje poteškoća](virtual-machines-windows-detailed-troubleshoot-rdp.md).

- [Paket za dijagnostiku Azure IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Upute za otklanjanje poteškoća u pristupa aplikacije koje rade na na VM, potražite u članku [Otklanjanje poteškoća s pristupom aplikacije koje se izvode na programa Azure VM](virtual-machines-linux-troubleshoot-app-connection.md).
- Ako imate poteškoća pomoću sigurne ljuske (SSH) za povezivanje s Linux VM servisu Azure, potražite u članku [Otklanjanje poteškoća s SSH veze Linux VM u Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).
