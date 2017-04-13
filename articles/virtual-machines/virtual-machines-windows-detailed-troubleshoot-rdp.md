<properties
    pageTitle="Detaljne udaljene radne površine otklanjanje poteškoća | Microsoft Azure"
    description="Pregledajte detaljne korake za otklanjanje poteškoća udaljene radne površine pogreške koju ne možete na virtualnim računalima sustava Windows u Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"
    keywords="ne možete povezati na udaljenu radnu površinu, otklanjanje poteškoća s udaljene radne površine, udaljene radne površine ne možete povezati, udaljene radne površine pogreške, udaljene radne površine otklanjanje poteškoća, udaljene radne površine problema
"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Detaljne korake za otklanjanje poteškoća problemi udaljene radne površine na Windows VMs servisu Azure

Ovaj članak sadrži detaljne upute za otklanjanje poteškoća za dijagnosticiranje i popravak složene pogrešaka utemeljen na sustavu Windows Azure virtualnim strojevima udaljene radne površine.

> [AZURE.IMPORTANT] Da biste uklonili više uobičajenih pogrešaka udaljene radne površine, svakako pročitajte [Osnovni članak o otklanjanju poteškoća za udaljene radne površine](virtual-machines-windows-troubleshoot-rdp-connection.md) prije nastavka.

Koji se mogu pojaviti poruka o pogrešci udaljene radne površine koja ne oblik sličan bilo koji od određene poruke o pogrešci obuhvaćeno [Osnovni udaljene radne površine vodič za otklanjanje poteškoća](virtual-machines-windows-troubleshoot-rdp-connection.md). Slijedite ove korake da biste odredili Zašto se ne može povezati sa servisom RDP na Azure VM klijent udaljene radne površine (RDP).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Ako vam je potrebna dodatna pomoć u ovom članku u bilo kojem trenutku, možete se obratiti Azure stručnjaka na [MSDN Azure i na forumima prelijevanje stogu](https://azure.microsoft.com/support/forums/). Osim toga, možete uputiti incident Azure podršci. Idi na [web-mjesto za podršku za Azure](https://azure.microsoft.com/support/options/) i kliknite **Dohvati podrška**. Informacije o korištenju Azure podrške, pročitajte [Najčešća pitanja o programu Microsoft Azure podrška](https://azure.microsoft.com/support/faq/).


## <a name="components-of-a-remote-desktop-connection"></a>Komponente veze udaljene radne površine

Sljedeće komponente su povezane s RDP veza:

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_0.png)

Prije nego što nastavite, predlažemo da mentally pregledajte što se promijenilo nakon posljednjeg uspješna udaljene radne površine veze na VM. Ako, na primjer:

- Promijenio se javnu IP adresu na VM ili servisa u oblaku koja sadrži VM (naziva se i virtualna IP adresa [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)). Neuspjeh RDP možda DNS predmemoriju za klijenta i dalje ima *staru IP adresu* na registriran za naziv DNS-a. Pražnjenje predmemorije DNS klijenta i pokušajte se ponovno povezati s VM. Ili se pokušajte izravno s novi VIP.
- Aplikacije drugih proizvođača koji koristite upravljate vezama udaljene radne površine umjesto pomoću veze generira Azure portal. Provjerite je li konfiguracija aplikacije sadrži je odgovarajući TCP priključak za promet udaljene radne površine. Ovaj priključak za klasični virtualnog računala [Azure portal](https://portal.azure.com)možete provjeriti tako da kliknete na VM postavke > krajnje točke.


## <a name="preliminary-steps"></a>Preliminarni koraci

Prije prelaska na detaljni otklanjanje

- Provjerite status virtualnog računala web-mjesto portala za Azure klasični ili u okvir za Azure portal za očite probleme.
- Slijedite [korake za brzi popravak uobičajene pogreške RDP u osnovni vodič za otklanjanje poteškoća](virtual-machines-windows-troubleshoot-rdp-connection.md#quick-troubleshooting-steps).


Pokušajte ponovno VM putem udaljene radne površine nakon ovih koraka.


## <a name="detailed-troubleshooting-steps"></a>Detaljne upute za otklanjanje poteškoća

Udaljena radna površina možda nećete moći pristupiti servisu udaljene radne površine na VM Azure zbog problema s pri sljedećih izvora:

- [Udaljene radne površine klijentsko računalo](#source-1-remote-desktop-client-computer)
- [Tvrtka ili ustanova intranetu rub uređaja](#source-2-organization-intranet-edge-device)
- [Krajnja točka servisa u oblaku i pristup popis kontrole (ACL)](#source-3-cloud-service-endpoint-and-acl)
- [Mrežni sigurnosne grupe](#source-4-network-security-groups)
- [Utemeljen na sustavu Windows Azure VM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>Izvor 1: Udaljene radne površine klijentsko računalo

Provjerite je li vaše računalo možete napraviti veze udaljene radne površine u drugi na lokalnom računalu sa sustavom Windows.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_1.png)

Ako ne možete, provjerite sljedeće postavke na vašem računalu:

- Postavka za lokalni vatrozid koji blokira promet udaljene radne površine.
- Lokalno instaliran klijentski softver proxy onemogućuje veze udaljene radne površine.
- Lokalno instaliran softver koji sprječava veze udaljene radne površine za nadzor mreže.
- Druge vrste sigurnosni softver koji praćenje promet ili Dopusti/onemogućiti određenim vrstama prometa koji sprječava veze udaljene radne površine.

U svim tim slučajevima, privremeno onemogućite softver i pokušate se povezati s programa lokalnim računalom putem udaljene radne površine. Ako možete saznati stvarni uzrok na taj način, rad s administrator mreže da biste ispravili softverske postavke da dopušta veze udaljene radne površine.

## <a name="source-2-organization-intranet-edge-device"></a>Izvor 2: Tvrtka ili ustanova intranetu rub uređaja

Provjerite je li računalo izravno povezani s Internetom možete napraviti veze udaljene radne površine za Azure virtualnog računala.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_2.png)

Ako nemate računalo na kojem je izravno povezano s Internetom, stvaranje i testiranje s novi Azure virtualnog računala resursa servisu grupe ili oblačić. Dodatne informacije potražite u članku [Stvaranje virtualnog računala sa sustavom Windows u Azure](virtual-machines-windows-hero-tutorial.md). Virtualnog računala i grupu resursa ili na servis u oblaku, možete izbrisati nakon test.

Ako stvarate vezu udaljene radne površine s računala izravno povezan s Internetom, provjerite koji uređaj rub intranet tvrtke ili ustanove za:

- Programa Interna vatrozid blokira HTTP veza s Internetom.
- Proxy poslužitelj sprječava veze udaljene radne površine.
- Otkrivanje podataka ili softver koji se izvodi na uređaje u mreži rub onemogućuje veze udaljene radne površine za nadzor mreže.

Rad s administrator mreže da biste ispravili postavke uređaja rub intranet tvrtke ili ustanove da biste omogućili utemeljen na HTTPS udaljene radne površine veza s Internetom.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Izvor 3: Krajnja točka servisa u Oblaku i ACL

Za VMs stvoren pomoću model implementacije klasični, provjerite je li da drugi VM Azure koji se nalazi u istom servis u oblaku ili virtualne mreže možete unijeti veze udaljene radne površine na VM Azure.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_3.png)

> [AZURE.NOTE] Virtualnim strojevima stvorene u upravitelju resursa, prijeđite na [izvor 4: mreže sigurnosne grupe](#source-4-network-security-groups).

Ako nemate drugi virtualnog računala u istoj servis u oblaku ili virtualne mreže, stvorite ga. Slijedite korake u [Stvaranje virtualnog računala sa sustavom Windows u Azure](virtual-machines-windows-hero-tutorial.md). Brisanje test virtualnog računala nakon dovršetka test.

Ako se možete povezati putem udaljene radne površine da biste virtualnog računala u istoj servis u oblaku ili virtualne mreže, provjerite ove postavke:

- Konfiguraciju krajnju točku za udaljene radne površine promet na odredištu VM: privatne TCP priključak krajnje točke moraju se podudarati TCP priključak na kojem se servisu udaljene radne površine na VM priključuje (Zadana vrijednost je 3389).
- ACL-a za krajnju točku promet udaljene radne površine na odredištu VM: ACL-a omogućuju vam da biste odredili dopušten ili zabranjen promet s Interneta na temelju IP adrese izvora. Nisu pravilno konfigurirani ACL-a možete spriječiti udaljene radne površine promet krajnjoj. Provjerite ACL-a da biste bili sigurni te promet na javnu IP adresa vašeg proxy poslužitelja ili je dopušteno druge rubni poslužitelj. Dodatne informacije potražite u članku [što je na mreži pristup kontrola popisa (ACL)?](../virtual-network/virtual-networks-acl.md)

Da biste provjerili je li krajnju točku izvor problema, uklonite trenutnu krajnjoj točki i stvorite novu, odabir slučajni port (priključak) u rasponu od 49152 – 65535 za broj port vanjskog. Dodatne informacije potražite u članku [upute za postavljanje krajnje točke za virtualnog računala](virtual-machines-windows-classic-setup-endpoints.md).

## <a name="source-4-network-security-groups"></a>Izvor 4: Mreže sigurnosne grupe

Mrežni sigurnosne grupe omogućuju precizniji kontrolu nad dopuštenu ulazni i izlazni promet. Možete stvoriti pravila koje se protežu podmreže i usluge u Azure virtualne mreže u oblaku. Provjerite pravila mreže sigurnosne grupe da biste bili sigurni da je dopušteno udaljene radne površine promet s interneta:

- Na portalu Azure odaberite vaše VM
- Kliknite **sve postavke** | **sučelje mreže** , a zatim odaberite mrežno sučelje.
- Kliknite **sve postavke** | **mrežom sigurnosne grupe** , a zatim odaberite sigurnosne grupe na mreži.
- Kliknite **sve postavke** | **pravila ulazne sigurnost** i provjerite da ste stvorili pravilo dopuštanja RDP na TCP priključak 3389.
    - Ako nemate pravilo, kliknite **Dodaj** da biste stvorili pravilo. Unesite **TCP** protokol i zatim **3389** za odredišni priključak raspon.
    - Provjerite je li akciju postavljena na **Dopusti** , a zatim kliknite u redu da biste spremili pravilo ulazne.


Dodatne informacije potražite u članku [što je mreža grupe sigurnosti (NSG)?](../virtual-network/virtual-networks-nsg.md)

## <a name="source-5-windows-based-azure-vm"></a>Izvor 5: Utemeljen na sustavu Windows Azure VM

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_5.png)

Je li se pogreške zbog Azure virtualnog računala sam pomoću [Azure IaaS (Windows) dijagnostike paketa](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) . Ako dijagnostike paketa ne može riješiti problem **RDP veza sa sustavom Azure VM (potrebno je ponovno pokretanje)** , slijedite upute u [ovom članku](virtual-machines-windows-reset-rdp.md). U ovom se članku vraća servisu udaljene radne površine na virtualnog računala:

- Omogućivanje zadano pravilo "Udaljene radne površine" vatrozid za Windows (TCP priključak 3389).
- Omogućivanje veze udaljene radne površine tako da postavite vrijednost registra HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections na 0.

Ponovno pokušajte spajanje s računala. Ako i dalje ne možete povezati putem udaljene radne površine, provjerite sljedeće moguće probleme:

- Na odredištu VM nije pokrenut servis udaljene radne površine.
- Servis za udaljene radne površine nije priključuje na TCP priključak 3389.
- Vatrozid za Windows ili neki drugi lokalni vatrozid sadrži pravilo izlaznog onemogućuje prometa udaljene radne površine.
- Otkrivanje podataka ili softver koji se izvodi na Azure virtualnog računala za nadzor mreže sprječava veze udaljene radne površine.

Za VMs stvoren pomoću model klasični implementacije, možete koristiti udaljene sesije Azure PowerShell za Azure virtualnog računala. Najprije morate instalirati certifikat za virtualnog računala pohrane u oblaku. Idite na [Konfiguriranje sigurne Remote PowerShell pristupa virtualnim računalima sustava za Azure](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) i preuzimanje datoteka skripte **InstallWinRMCertAzureVM.ps1** s vašim lokalnim računalom.

Nakon toga instalirati Azure PowerShell ako to već niste učinili. Saznajte [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

Zatim otvorite naredbeni redak programa Azure PowerShell i promijeniti trenutnu mapu mjesto skriptna datoteka **InstallWinRMCertAzureVM.ps1** . Da biste pokrenuli skripte Azure PowerShell, morate postaviti pravila ispravna izvođenja. Pokrenite naredbu **Get-pravilnik izvođenja** da biste odredili trenutnu razinu pravilnika. Informacije o postavljanju odgovarajuću razinu potražite u članku [Postavljanje-pravilnik izvođenja](https://technet.microsoft.com/library/hh849812.aspx).

Nakon toga unesite naziv svoje pretplate Azure, naziv usluge oblaka i naziv virtualnog računala (uklanjanje na < i > znakova), a zatim pokrenite te naredbe.

    $subscr="<Name of your Azure subscription>"
    $serviceName="<Name of the cloud service that contains the target virtual machine>"
    $vmName="<Name of the target virtual machine>"
    .\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName

Naziv odgovarajuće pretplate možete doći s svojstvo _SubscriptionName_ prikaz naredbe **Get-AzureSubscription** . Naziv usluge oblaka za virtualnog računala možete pristupiti iz stupca _naziv servisa_ u prikaz naredbe **Get-AzureVM** .

Provjerite koristite li novi certifikat. Otvorite certifikati dodatku za trenutnog korisnika i pogledajte u mapu **Pouzdane Authorities\Certificates korijenska ustanova** . Trebali biste vidjeti certifikat s nazivom DNS-a na servis u oblaku u stupcu izdao (primjer: cloudservice4testing.cloudapp.net).

Nakon toga pokrenite udaljene sesije Azure PowerShell pomoću ove naredbe.

    $uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
    $creds = Get-Credential
    Enter-PSSession -ConnectionUri $uri -Credential $creds

Kada unesete valjani administratorske vjerodajnice, trebali biste vidjeti nešto slično kao sljedeći upit Azure PowerShell:

    [cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>

Prvi dio ovaj upit je naziv servisa oblaka koja sadrži cilj VM, koji mogu se razlikovati od "cloudservice4testing.cloudapp.net". Sada možete izdati Azure PowerShell naredbe za ovaj servis u oblaku da biste istražili probleme navedenim i ispravljanje konfiguracije.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>Da biste ručno ispravili servise udaljene radne površine slušanje TCP priključak

Ako ne možete pokrenuti [Azure IaaS (Windows) dijagnostike paketa](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) za problem **RDP veza sa sustavom Azure VM (potrebno je ponovno pokretanje)** udaljene sesije zatraži Azure PowerShell pokrenite sljedeću naredbu.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

Svojstvo PortNumber prikazuje trenutni broj priključka. Ako je potrebno, promijenite udaljene radne površine priključkom brojčanih natrag na zadanu vrijednost (3389) pomoću ove naredbe.

    Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389

Provjerite je li priključak promijenjen 3389 pomoću ove naredbe.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

Zatvorite udaljenu sesiju ljuske PowerShell Azure pomoću ove naredbe.

    Exit-PSSession

Provjerite da krajnju točku udaljene radne površine za Azure VM također koristi priključak TCP 3398 kao njegova internog priključka. Pokrenite Azure VM i pokušajte ponovno povezati udaljene radne površine.


## <a name="additional-resources"></a>Dodatni resursi

[Paket za dijagnostiku Azure IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)

[Kako ponovno postaviti lozinku ili servisu udaljene radne površine za virtualnim strojevima sa sustavom Windows](virtual-machines-windows-reset-rdp.md)

[Kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md)

[Otklanjanje poteškoća s vezama sigurne ljuske (SSH) da biste s operacijskim sustavom Linux Azure virtualnog računala](virtual-machines-linux-troubleshoot-ssh-connection.md)

[Otklanjanje poteškoća s pristupom aplikacije koje se izvode na Azure virtualnog računala](virtual-machines-linux-troubleshoot-app-connection.md)
