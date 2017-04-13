<properties
    pageTitle="Priprema VHD Windows da biste prenijeli na Azure | Microsoft Azure"
    description="Preporučeni Savjeti za pripremanje Windows VHD prije prijenosa Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="genlin"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/18/2016"
    ms.author="glimoli;genli"/>

# <a name="prepare-a-windows-vhd-to-upload-to-azure"></a>Priprema VHD Windows da biste prenijeli na Azure
Da biste prenijeli Windows VM lokalnim Azure, pravilno pripremite virtualne tvrdom disku (VHD). Postoji nekoliko preporučena koraka za dovršetak prije prijenosa na VHD za Azure. U ovom se članku prikazuje kako pripremiti VHD Windows da biste prenijeli na Microsoft Azure, a i objašnjava [kada i kako koristiti Sysprep](#step23).

## <a name="prepare-the-virtual-disk"></a>Priprema virtualne disk

>[AZURE.NOTE] 
> Azure podržava samo [generacije 1 virtualnim strojevima](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx) koji su u obliku datoteke VHD. U noviji oblik VHDX nije podržan u Azure. 
>
> Na VHD mora biti Fixed veličina ne dinamičke. Ako je potrebno, upute u nastavku detaljno pretvaranje iz VHDX ili dinamičkih diskova. Maksimalna veličina dopušteno u VHD je 1,023 GB.


Provjerite je li Windows VHD ispravno funkcionira na lokalnom poslužitelju. Razrješavanje pogreške unutar na VM sam prije nego što pretvoriti ili prijenos Azure.

Ako vam je potrebna da biste pretvorili virtualne diska potreban oblik za Azure, koristite jedan od načina naznačeno u sljedećim odjeljcima. Sigurnosno kopirajte na VM prije pokretanja virtualne disk pretvorbe ni Sysprep.

### <a name="convert-using-hyper-v-manager"></a>Pretvaranje pomoću upravitelja Hyper-V
- Otvorite upravitelj Hyper-V i odaberite vašim lokalnim računalom na lijevoj strani. Na izborniku iznad njega, kliknite **Akcija** > **Uređivanje Disk**.
    - Na zaslonu **Pronađite virtualne na tvrdom disku** otvorite pa odaberite virtualne disk.
    - Na sljedećem zaslonu odaberite **Pretvori**
        - Ako je potrebno pretvoriti iz VHDX odaberite **VHD** i kliknite **Dalje**
        - Ako vam je potrebna pretvorba iz dinamički disk, odaberite **Fixed veličina** , pa kliknite **Dalje**

    - Pronađite i odaberite **put za nove datoteke VHD**.
    - Kliknite **Završi** da biste zatvorili.

### <a name="convert-using-powershell"></a>Pretvaranje pomoću komponente PowerShell
Možete pretvoriti virtualni disk pomoću [cmdleta ljuske PowerShell Pretvori VHD](http://technet.microsoft.com/library/hh848454.aspx). U sljedećem primjeru smo su Pretvorba iz programa VHDX VHD i Pretvorba iz dinamičnu Fixed vrstu:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

### <a name="convert-from-vmware-vmdk-disk-format"></a>Pretvorba iz oblika VMware VMDK na disku
Ako imate Windows VM slike u [oblik datoteke VMDK](https://en.wikipedia.org/wiki/VMDK), pretvorite je u VHD pomoću [Pretvornika Microsoft virtualnog računala](https://www.microsoft.com/download/details.aspx?id=42497). Čitanje bloga [kako pretvoriti VMware VMDK da biste Hyper-V VHD](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) dodatne informacije.

## <a name="prepare-windows-configuration-for-upload"></a>Priprema Windows konfiguracija za prijenos

> [AZURE.NOTE] Pokrenite sljedeće naredbe s [administratorskim ovlastima](https://technet.microsoft.com/library/cc947813.aspx).

1. Uklonite sve statički stalni smjer usmjeravanje tablicu:

    - Da biste pogledali usmjeravanje tablice, pokrenite `route print`.
    - Provjerite **Postojanost usmjerava** sekcije. Ako stalni usmjeravanje, koristite [usmjeravanje izbrisati](https://technet.microsoft.com/library/cc739598.apx) da biste je uklonili.

2. Uklanjanje WinHTTP proxy:

    ```
    netsh winhttp reset proxy
    ```

3. Konfiguriranje diska SAN pravila [Onlineall](https://technet.microsoft.com/library/gg252636.aspx):

    ```
    diskpart san policy=onlineall
    ```

4. Korištenje vrijeme koordiniranog vremena (UTC-a) za Windows i postavite postavku vrsta pokretanja servisa Windows Time (w32time) da biste **automatski**:

    ```
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    sc config w32time start= auto
    ```


## <a name="configure-windows-services"></a>Konfiguriranje servisa za Windows
5. Provjerite je li svaki od sljedećih servisa Windows je postavljena na **zadane vrijednosti za Windows**. Su konfigurirana s postavki za pokretanje navedeno na popisu u nastavku. Možete pokrenuti te naredbe vratiti izvorne postavke za pokretanje:

    ```
    sc config bfe start= auto

    sc config dcomlaunch start= auto

    sc config dhcp start= auto

    sc config dnscache start= auto

    sc config IKEEXT start= auto

    sc config iphlpsvc start= auto

    sc config PolicyAgent start= demand

    sc config LSM start= auto

    sc config netlogon start= demand

    sc config netman start= demand

    sc config NcaSvc start= demand

    sc config netprofm start= demand

    sc config NlaSvc start= auto

    sc config nsi start= auto

    sc config RpcSs start= auto

    sc config RpcEptMapper start= auto

    sc config termService start= demand

    sc config MpsSvc start= auto

    sc config WinHttpAutoProxySvc start= demand

    sc config LanmanWorkstation start= auto

    sc config RemoteRegistry start= auto
    ```


## <a name="configure-remote-desktop-configuration"></a>Konfiguriranje konfiguracije udaljene radne površine
6. Ako postoje neki samopotpisane potvrde uz ga slušatelj protokola udaljene radne površine (RDP), uklonite ih:

    ```
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
    ```

    Dodatne informacije o konfiguriranju certifikati za RDP ga slušatelj potražite u članku [Konfiguracija certifikat ga Slušatelj u sustavu Windows Server](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)

7. Konfiguriranje vrijednosti [KeepAlive](https://technet.microsoft.com/library/cc957549.aspx) RDP servisa:

    ```
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```

8. Konfiguriranje način provjere autentičnosti za servis RDP:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```

9. Omogućivanje servisa RDP dodavanjem sljedeće potključeve registra:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```


## <a name="configure-windows-firewall-rules"></a>Konfigurirati pravila vatrozida za Windows
10. Dopusti WinRM do tri profila vatrozid (domene, privatni i javni) i omogućili uslugu PowerShell Remote:

    ```
    Enable-PSRemoting -force
    ```

11. Provjerite jesu li sljedeća pravila vatrozida za goste operacijski sustav na mjestu:

    - Dolazni

    ```
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    ```

    - Ulazni i izlazni

    ```
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

    netsh advfirewall firewall set rule group="Core Networking" new enable=yes
    ```

    - Izlazni

    ```
    netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
    ```


## <a name="additional-windows-configuration-steps"></a>Dodatni koraci za konfiguraciju sustava Windows
12. Pokrenite `winmgmt /verifyrepository` da biste provjerili je li Windows Management Instrumentation (WMI) spremište dosljedni. Ako u spremištu je oštećena, pogledajte [Ovaj članak na blogu](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

13. Provjerite postavke pokretanja Configuration Data (BCD) odgovaraju sljedeće:

    ```
    bcdedit /set {bootmgr} integrityservices enable

    bcdedit /set {default} device partition=C:

    bcdedit /set {default} integrityservices enable

    bcdedit /set {default} recoveryenabled Off

    bcdedit /set {default} osdevice partition=C:

    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
    ```

14. Uklonite sve dodatne filtre sučelje upravljačkog programa za prijenos, kao što je softver koji analizira TCP paketi.
15. Da biste bili sigurni da je disk dobar su i dosljedni, pokrenite na `CHKDSK /f` naredbe.
16. Deinstalirajte drugih proizvođača softvera i upravljački program vezane uz fizičke komponente ili značajke virtualizacije tehnologije.
17. Provjerite koristi li aplikacije drugih proizvođača priključak 3389. Ovaj priključak koristi se za servis RDP u Azure. Možete koristiti u `netstat -anob` naredbu da biste provjerili priključke koji koriste aplikacije.
18. Ako je Windows VHD koje želite prenijeti kontroler domene, slijedite [ove dodatne korake](https://support.microsoft.com/kb/2904015) da biste pripremili disk.
19. Ponovno pokrenite računalo VM da biste bili sigurni da je i dalje dobar Windows mogu pristupiti pomoću RDP veza.
20. Ponovno postavljanje lozinke trenutnu lokalni administrator i provjerite je li koristiti taj račun za prijavu u Windows putem veze za RDP.  U ovom dozvolom pristupa upravlja objekta pravilnika "Dopusti zapisnika na putem usluge udaljene radne površine". Taj objekt nalazi se u odjeljku "Računalo Configuration\Windows Settings\Security Settings\Local Policies\User prava dodjele."


## <a name="install-windows-updates"></a>Instaliranje ažuriranja sustava Windows
22. Instalirajte najnovija ažuriranja za Windows. Ako to nije moguće, provjerite je li instaliranih ažuriranja za sljedeće:

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VMs ne oporavlja iz prekida mreže i pojave problemi oštećenja podataka

    - [KB3115224](https://support.microsoft.com/kb/3115224) Poboljšanja pouzdanosti VMs koji su pokrenuti na Windows Server 2012 R2 ili Windows Server 2012 glavnog računala

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16 031: Sigurnosnog ažuriranja za Microsoft Windows da biste adresu povećanje ovlasti: ožujak 8, 2016

    - [KB3063075](https://support.microsoft.com/kb/3063075) Broj događaja ID 129 prijavljeni prilikom pokretanja sustava Windows Server 2012 R2 virtualnog računala u Microsoft Azure

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VMs ne oporavlja iz prekida mreže i pojave problemi oštećenja podataka

    - [KB3114025](https://support.microsoft.com/kb/3114025) Slabe performanse prilikom pristupa Azure datoteke za pohranu iz Windows 8.1 ili Server 2012 R2

    - [KB3033930](https://support.microsoft.com/kb/3033930) Hitni popravak povećava 64K ograničenje međuspremnika RIO po postupak za Azure servisa u sustavu Windows

    - [KB3004545](https://support.microsoft.com/kb/3004545) Ne mogu pristupiti virtualnim strojevima koji se nalaze na Azure usluge hostinga putem veze za VPN-a u sustavu Windows

    - [KB3082343](https://support.microsoft.com/kb/3082343) Povezivanje VPN lokacija se gube kad Azure tunnels VPN-a web-mjesto pomoću Windows Server 2012 R2 RRAS

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16 031: Sigurnosnog ažuriranja za Microsoft Windows da biste adresu povećanje ovlasti: ožujak 8, 2016

    - [KB3146723](https://support.microsoft.com/kb/3146723) MS16 048: Opis sigurnosnog ažuriranja za CSRSS: Travanj 12, 2016
    - [KB2904100](https://support.microsoft.com/kb/2904100) Sustav smrzava tijekom disk/i u sustavu Windows<a id="step23"></a>
23. Ako želite stvoriti sliku za implementaciju više računala od njega morati generalize slike tako da pokrenete `sysprep` prije prijenosa na VHD za Azure. Ne morate pokrenuti `sysprep` za korištenje specijalizirane VHD. Dodatne informacije o stvaranju generalizirano slike potražite u sljedećim člancima:

    - [Stvaranje VM sliku iz postojeće Azure VM pomoću modela implementacije Voditelj resursa](virtual-machines-windows-create-vm-generalized.md)
    - [Stvaranje VM sliku iz postojeće Azure VM pomoću modema implementacije Classic](virtual-machines-windows-classic-capture-image.md)
    - [Sysprep podrška za uloge poslužitelja](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)



## <a name="suggested-extra-configurations"></a>Predloženi dodatna konfiguracija

Sljedeće postavke ne utječu na VHD prijenos. Međutim, preporučujemo da ste ih konfigurirali.

- Instalirajte [Agent za Azure virtualnih računala](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Nakon što instalirate agenta, možete omogućiti VM proširenja. Proširenja VM implementirati Većina ključne funkcionalnosti koji želite koristiti s VMs kao što su promjene lozinki, konfiguriranje RDP i mnoge druge.

- Ispis zapisnika može biti korisno u otklanjanje problema s pad sustava Windows. Omogući prikupljanje zapisnika ispisa:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

    sc config wer start= auto
    ```

- Nakon stvaranja na VM u Azure konfiguriranje veličina stranične sustava definiran na pogonu D:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```


## <a name="next-steps"></a>Daljnji koraci

- [Prenijeli sliku Windows VM Azure implementacijama Voditelj resursa](virtual-machines-windows-upload-image.md)
