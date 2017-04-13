<properties
   pageTitle="Ponovno postavite lozinku za lokalni sustav Windows Azure goste agent je instalirana komponenta | Microsoft Azure"
   description="Kako ponovno postavljanje lozinke korisničkog računa lokalnog sustava Windows kada agent za Azure gosta nije instaliran ili funkcionira na na VM"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="iainfou"/>

# <a name="how-to-reset-local-windows-password-for-azure-vm"></a>Kako ponovno postavljanje lozinke za lokalni sustav Windows za Azure VM
Lokalni Windows lozinku VM Azure pomoću na [portal za Azure ili Azure PowerShell](virtual-machines-windows-reset-rdp.md) navedene je instaliran agent za Azure goste možete ponovno postaviti. Ta je metoda primarni način za ponovno postavljanje lozinke za programa Azure VM. Ako naiđete na probleme s agent za Azure goste ne reagira ili ne uspijeva da biste instalirali nakon prenošenja prilagođenu sliku, možete ručno ponovno postavite lozinku za Windows. U ovom se članku detaljno kako ponovno postavljanje lozinke za lokalni račun Pridruživanjem izvorni OS virtualne disk drugi VM. 

> [AZURE.WARNING] Koristiti samo taj postupak ne uspije. Uvijek pokušajte ponovno postavljanje lozinke pomoću [portala za Azure ili Azure PowerShell](virtual-machines-windows-reset-rdp.md) prvi put.


## <a name="overview-of-the-process"></a>Pregled postupka
Temeljni korake za izvođenje lokalne za vraćanje izvorne lozinke za Windows VM u Azure kada je pristup agent za Azure goste je na sljedeći način:

- Brisanje izvorišnog web-mjesta VM. Virtualna diskova zadržavaju.
- Prilaganje VM izvor OS disk drugi VM unutar Azure pretplatu. U ovom VM se nazivaju se VM za otklanjanje poteškoća.
- Korištenje otklanjanje poteškoća VM, stvorite neke datoteke config na disku OS VM izvora.
- Odvajanje na VM OS disk s VM za otklanjanje poteškoća.
- Pomoću predloška Voditelj resursa da biste stvorili VM, pomoću izvorne virtualne diska.
- Kada se pokrene novi VM, datoteke config koje ste stvorili ažurirajte lozinku potrebno korisnika.


## <a name="detailed-steps"></a>Detaljne upute
Uvijek pokušajte ponovno postavljanje lozinke pomoću [portala za Azure ili Azure PowerShell](virtual-machines-windows-reset-rdp.md) prije nego na sljedeći način. Provjerite je li sigurnosnu kopiju vašeg VM prije nego što počnete. 

1. Brisanje problematične VM Azure portalu. Brisanje na VM briše samo metapodaci, referenca VM unutar Azure. Virtualna diskova zadržavaju se nakon brisanja na VM:

    - Odaberite na VM na portalu za Azure, kliknite *Izbriši*:

    ![Brisanje postojeće VM](./media/virtual-machines-windows-reset-local-password-without-guest-agent/delete_vm.png)

2. Prilaganje VM izvor OS disk VM za otklanjanje poteškoća. Otklanjanje poteškoća VM mora biti u području isti kao izvor VM OS (kao što su `West US`):

    - Odaberite otklanjanje poteškoća VM na portalu za Azure. Kliknite *diskova* | *postojeći privitak*:

    ![Prilaganje postojeći disk](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_attach_existing.png)

    Odaberite *VHD datoteku* , a zatim odaberite račun za pohranu koji sadrži izvor VM:

    ![Odaberite račun za pohranu](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_storageaccount.PNG)

    Odaberite kontejner izvora. Spremnik izvor obično je *vhds*:

    ![Odaberite kontejner prostora za pohranu](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_container.png)

    Odaberite vhd OS priložiti. Kliknite *Odaberite* da biste dovršili postupak:

    ![Odaberite izvor virtualne disk](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_source_vhd.png)

3. Povežite se s otklanjanje poteškoća VM putem udaljene radne površine i provjerite je li na disku OS VM izvora vidljivi:

    - Odaberite otklanjanje poteškoća VM na portalu za Azure, a zatim kliknite *Poveži*.
    - Otvorite datoteku RDP preuzimanja. Unesite korisničko ime i lozinku za otklanjanje poteškoća VM.
    - U Eksploreru za datoteke potražite podatkovni disk ste priložili. Ako je izvor VHD VM na disku samo podaci priložiti otklanjanje poteškoća VM, mora biti F: pogon:

    ![Prikaz privitka podatkovni disk](./media/virtual-machines-windows-reset-local-password-without-guest-agent/troubleshooting_vm_fileexplorer.png)

4. Stvaranje `gpt.ini` u `\Windows\System32\GroupPolicy` na disku VM izvora (Ako postoji gpt.ini, preimenujte gpt.ini.bak):

    > [AZURE.WARNING] Pripazite da ne slučajno stvorite sljedeće datoteke u C:\Windows, pogon OS za otklanjanje poteškoća VM. Stvaranje sljedeće datoteke u pogon OS za izvor VM koja je priložena kao podatkovni disk.

    - Dodajte sljedeće retke u na `gpt.ini` datoteku koju ste stvorili:

    ```
    [General]
    gPCFunctionalityVersion=2
    gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
    Version=1
    ```

    ![Stvaranje gpt.ini](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_gpt_ini.png)
 
5. Stvaranje `scripts.ini` u `\Windows\System32\GroupPolicy\Machine\Scripts`. Provjerite prikazuje li skrivene mape. Ako je potrebno, stvorite na `Machine` ili `Scripts` mape.

    - Dodajte sljedeće retke u `scripts.ini` datoteku koju ste stvorili:

    ```
    [Startup]
    0CmdLine=C:\Windows\System32\FixAzureVM.cmd
    0Parameters=
    ```

    ![Stvaranje scripts.ini](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_scripts_ini.png)
 
6. Stvaranje `FixAzureVM.cmd` u `\Windows\System32` sljedeće sadržajem zamjene `<username>` i `<newpassword>` pomoću vlastitih vrijednosti:

    ```
    NET USER <username> <newpassword>
    ```

    ![Stvaranje FixAzureVM.cmd](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_fixazure_cmd.png)

    Složenosti lozinki konfiguriran za vaše VM mora zadovoljiti prilikom definiranja s novom lozinkom.

7. Azure portalu odvojiti disk s VM za otklanjanje poteškoća:

    - Odaberite otklanjanje poteškoća VM na portalu za Azure, kliknite *diskova*.
    - Odaberite podatkovni disk priložiti korak 2, kliknite *Odvoji*:

    ![Odvajanje disk](./media/virtual-machines-windows-reset-local-password-without-guest-agent/detach_disk.png)

8. Prije stvaranja na VM dobiti URI na disk OS izvora:

    - Odaberite račun za pohranu na portalu za Azure, kliknite *blob-ova*.
    - Odaberite kontejner. Spremnik izvor obično je *vhds*:

    ![Odaberite blob račun za pohranu](./media/virtual-machines-windows-reset-local-password-without-guest-agent/select_storage_details.png)

    Odaberite izvor VM OS VHD, a zatim kliknite gumb *Kopiraj* uz naziv *URL ADRESE* :

    ![Kopiranje disk URI-JA](./media/virtual-machines-windows-reset-local-password-without-guest-agent/copy_source_vhd_uri.png)

9. Stvaranje na VM s diska OS VM izvora:

    - Pomoću [ovog predloška Azure Voditelj resursa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) možete stvoriti na VM iz specijalizirane VHD. Kliknite na `Deploy to Azure` da biste otvorili portala za Azure s predloškom pojedinosti popunjeno umjesto vas.
    - Ako želite zadržati sve prethodne postavke za na VM, odaberite *Uređivanje predloška* omogućuje postojeće VNet, podmreži, mrežnog prilagodnika ili javnu IP.
    - U na `OSDISKVHDURI` parametar tekstni okvir, lijepljenje URI izvor VHD dobiti u prethodnom koraku:

    ![Stvaranje na VM iz predloška](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_new_vm_from_template.png)

10. Kad se novi VM traje, povezati VM putem udaljene radne površine s novom lozinkom koju ste naveli u na `FixAzureVM.cmd` skripte.

11. Sljedeće datoteke da biste očistili okruženje uklonili iz svoje udaljenu sesiju novi VM:

    - Iz %windir%\System32
        - Uklanjanje FixAzureVM.cmd
    - Iz %windir%\System32\GroupPolicy\Machine\
        - Uklanjanje scripts.ini
    - Iz %windir%\System32\GroupPolicy
        - Uklanjanje gpt.ini (Ako gpt.ini postojala prije i preimenovana u gpt.ini.bak, a zatim Preimenuj .bak datoteke natrag u gpt.ini)

## <a name="next-steps"></a>Daljnji koraci
Ako se i dalje ne možete povezati putem udaljene radne površine, potražite u članku [Vodič za otklanjanje poteškoća RDP](virtual-machines-windows-troubleshoot-rdp-connection.md). [Detaljni vodič za otklanjanje poteškoća RDP](virtual-machines-windows-detailed-troubleshoot-rdp.md) razmatra metode umjesto konkretne upute za otklanjanje poteškoća. Možete i [otvorite zahtjev za Azure podrška](https://azure.microsoft.com/support/options/) za stjecanja pomoć.