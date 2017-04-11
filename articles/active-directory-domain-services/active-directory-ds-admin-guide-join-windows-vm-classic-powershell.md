<properties
    pageTitle="Azure Active Directory Domain Services: Vodič za administraciju | Microsoft Azure"
    description="Upravljani domene pomoću komponente PowerShell Azure i model klasični implementacije pridružite virtualnog računala za Windows."
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>


# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Uključivanje virtualnog računala za Windows Server upravljanih domene pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Azure klasični portala - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell – Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure sadrži dva različitoj implementaciji modela za stvaranje i rad s resursima: [Voditelj resursa i classic](../resource-manager-deployment-model.md). U ovom se članku opisuje pomoću klasične implementacije modela. Azure servisa Active Directory Domain Services trenutno ne podržava modela Voditelj resursa.

Sljedeći vam koraci prikazuju kako prilagoditi skup Azure PowerShell naredbe koje stvorite i prethodno konfigurirati stroj virtualne utemeljen na sustavu Windows Azure pomoću sastavnih blokova pristup. Ove korake olakšavaju stvaranje je utemeljen na sustavu Windows Azure virtualnog računala i uključivanje programa Azure servisa Active Directory Domain Services upravljanih prilagođenu domenu.

Te korake slijedite popunite-na-praznine pristup za stvaranje skupova naredbe ljuske PowerShell Azure. Taj se način može biti korisno ako ste novi korisnik komponente PowerShell ili ako želite saznati što vrijednosti da biste odredili za uspješan konfiguracije. Napredni korisnici PowerShell možete preuzeti naredbe i zamijeniti vlastite vrijednosti za varijable (reci počevši od "$").

Ako to još niste učinili, slijedite upute u [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) da biste instalirali Azure PowerShell na lokalnom računalu. Zatim otvorite naredbeni redak komponente Windows PowerShell.

## <a name="step-1-add-your-account"></a>Korak 1: Dodavanje računa

1. U odzivniku komponente PowerShell upišite **Dodaj AzureAccount** i pritisnite **Enter**.
2. Unesite adresu e-pošte povezanog s pretplatom na Azure, a zatim kliknite **Nastavi**.
3. Upišite u okvir Lozinka za račun.
4. Kliknite **Prijava**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Korak 2: Postavite svoju pretplatu i račun za pohranu

Postavite Azure pretplate i račun za pohranu tako da pokrenete ove naredbe komponente Windows PowerShell naredbeni redak. Zamijenite sve unutar navodnika, uključujući na < i > znakova s imena točna.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Naziv odgovarajuće pretplate možete doći s svojstvo SubscriptionName rezultatu naredbe **Get-AzureSubscription** . Naziv računa ispravne za pohranu možete doći s svojstvo oznake izlaza naredbu **Get-AzureStorageAccount** nakon pokretanja naredbe **Select AzureSubscription** .


## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Korak 3: Korak po korak vodič – Dodjela virtualnog računala i uključiti upravljanih domeni
Evo Azure PowerShell naredbe koje je postavljeno na stvaranje virtualnog računala pomoću praznih redaka između svaki blok za čitljivosti.

Navedite informacije o virtualnog računala za Windows da biste se dodjeli.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Vrijednosti InstanceSize za D-DS- ili G niz virtualnim računalima potražite u članku [virtualnog računala i veličine servisa oblaka za Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Sadrže informacije o upravljanih domene.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Navedite naziv servisa u oblaku.

    $svcname="Contoso100-test"

Navedite naziv virtualne mreže na koji se VM trebala bi biti spojena. Provjerite je li domena upravljanih AAD DS dostupnih u ovom virtualne mreže.

    $vnetname="MyPreviewVnet"

Odaberite sliku VM će se koristiti za dodjelu resursa u VM.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Konfiguriranje VM - naziv skupa VM, veličina instancu i sliku koja će se koristiti.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Nabavite lokalne administratorske vjerodajnice za na VM. Odaberite lokalni administrator jaku lozinku.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Pribavljanje vjerodajnica za korisnički račun koji pripadaju grupe "AAD Kontroler administratori" da biste se pridružili VM upravljanih domenu. Ne odredite naziv domene – na primjer, u našem primjeru, ne možemo odredite "teo" kao korisničko ime.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Konfiguriranje na VM - navedite zahtjev za uključivanje domene i traženih vjerodajnica.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Postavljanje podmreži za na VM.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Neobavezno: Postavite pokazivač se s IP adresom domene. Ako ste postavili IP adrese Azure servisa Active Directory Domain Services upravljanih domene biti DNS poslužitelji za virtualne mreže, ovaj korak nije potrebna.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Sada Dodjela Windows VM domene pridružili.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Skripta za dodjeljivanje Windows VM i automatski je uključiti domenu upravljanih programa AAD domenske servise
Naredba postavljeno PowerShell stvara virtualnog računala za poslužitelj redak tvrtke koje:

- Koristi Windows Server 2012 R2 Standard sliku.
- Je vrlo small virtualnog računala.
- Ima naziv tvrtke contoso-testa.
- Automatski domene pridruženo contoso100 upravljanih domene.
- Dodaje se isti virtualne mreže kao upravljanih domene.

Evo punu ogledni skriptu da biste stvorili virtualnog računala za Windows i automatski je uključiti domene upravljanih Azure servisa Active Directory Domain Services.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Srodni sadržaji
- [Azure servisa Active Directory Domain Services – vodič za početak rada](./active-directory-ds-getting-started.md)

- [Administriranje domene upravljanih programa Azure servisa Active Directory Domain Services](./active-directory-ds-admin-guide-administer-domain.md)
