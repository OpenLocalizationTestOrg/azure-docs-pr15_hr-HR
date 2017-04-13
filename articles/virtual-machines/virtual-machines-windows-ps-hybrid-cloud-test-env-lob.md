<properties 
    pageTitle="Okruženje za testiranje aplikacije LOB | Microsoft Azure" 
    description="Saznajte kako stvoriti crtu utemeljen na webu, aplikacije za tvrtke u oblaku hibridnog okruženja za IT pro ili razvoj testiranja." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Postavljanje na web-LOB aplikacija u oblak hibridnog za testiranje

U ovoj se temi vodi vas kroz stvaranje oblaka Simulirani hibridnog okruženja za testiranje redak koji se temelji na web aplikacije za tvrtke (LOB) smješten u Microsoft Azure. Evo dobivene konfiguracije.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Tu konfiguraciju sastoji se od:

- Simulirani lokalnu mrežu smješten u Azure (TestLab VNet).
- Web-lokacija virtualne mreže smješten u Azure (TestVNET).
- VNet VNet VPN veza.
- Koji se temelji na web poslužitelj LOB, SQL server i sekundarne kontroler TestVNET virtualne mreže.

Tu konfiguraciju pruža temelj i uobičajenih polazište iz kojeg možete:

- Razvoj i testiranje LOB aplikacije koji se nalaze na Internet Information Services (IIS) s pozadinskom za bazu podataka 2014 SQL Server u Azure.
- Izvođenje testiranja od ovog Simulirani hibridnog oblaku IT posla.

Postoje tri glavne faze postavljanje hibridno okruženje oblaka test:

1.  Postavljanje Simulirani hibridno okruženje oblaka.
2.  Konfiguriranje SQL poslužiteljskom računalu (SQL1).
3.  Konfiguriranje LOB poslužitelja (LOB1).

U ovom radno opterećenje zahtijeva Azure pretplate. Ako imate pretplatu na MSDN i Visual Studio, potražite u članku [mjesečni Azure odobrenja za pretplatnike na Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Primjer proizvodnje LOB aplikacije smješten u Azure, potražite u članku nacrt arhitektura **poslovnim aplikacijama** na [Microsoftov softver arhitektura dijagrame i Blueprints](http://msdn.microsoft.com/dn630664).

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>Faza 1: Postavljanje Simulirani hibridno okruženje oblaka

Stvaranje [Simulirani hibridnog okruženja za testiranje oblaka](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md). Budući da okruženje za testiranje potrebne prisutnosti poslužitelja APP1 na podmreži Corpnet, možete ga isključiti za sada.

Ovo je trenutna konfiguracija.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)
 
## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>Faza 2: Konfiguriranje SQL poslužiteljskom računalu (SQL1)

Azure, na portalu pokrenite DC2 računalo ako je potrebno.

Nakon toga stvaranje virtualnog računala za SQL1 pomoću ove naredbe ljuske PowerShell Azure naredbeni redak na lokalnom računalu. Prije pokretanja te naredbe, ispunite vrijednostima varijabli i uklanjanje na < i > znakova.

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Pomoću portala za Azure povezati s SQL1 putem lokalnog administratorskog računa sustava SQL1.

Nakon toga konfigurirati pravila vatrozida za Windows da biste omogućili osnovne testiranje i promet za SQL Server. Iz programa administratorske komponente Windows PowerShell naredbenog retka na SQL1, pokreće te naredbe.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Naredbe ping moraju rezultirati četiri uspješno Odgovori s IP adresom 192.168.0.4.

Nakon toga dodajte dodatni podaci disk na SQL1 kao novu jedinicu s slovo F:.

1.  U lijevom oknu Upravitelj poslužitelja, kliknite **datoteka, a servise za pohranu**, a zatim **diskova**.
2.  U oknu sadržaj u grupi **diskova** kliknite **disk 2** (s **particija** postavljena na **Nepoznato**).
3.  Kliknite **Zadaci**, a zatim kliknite **Nova jedinica**.
4.  Na prije početka stranica čarobnjaka novi glasnoće, kliknite **Dalje**.
5.  Na stranici Odabir poslužitelja i na disku, kliknite **2 na disku**, a zatim kliknite **Dalje**. Kada se to od vas zatraži, kliknite **u redu**.
6.  Na stranici navođenje veličine stranica glasnoće, kliknite **Dalje**.
7.  Na stranici Dodjela na stranicu pogon slovo ili mapu, kliknite **Dalje**.
8.  Na stranici Postavke sustava odaberite datoteka, kliknite **Dalje**.
9.  Na stranici Potvrda odabira, kliknite **Stvori**.
10. Po dovršetku kliknite **Zatvori**.

Pokrenite sljedeće naredbe u naredbenom retku komponente Windows PowerShell na SQL1:

    md f:\Data
    md f:\Log
    md f:\Backup

Nakon toga pridružite SQL1 domene CORP Windows Server Active Directory pomoću ove naredbe komponente Windows PowerShell zatraži na SQL1.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Navedite vjerodajnica računa domene za naredbu **Dodaj računala** pomoću računa za CORP\User1 kada se to od vas zatraži.

Nakon ponovnog pokretanja, pomoću portala za Azure povezati s SQL1 *s lokalni administratorski račun SQL1*.

Nakon toga konfigurirati 2014 SQL Server da koristiti pogon F: za nove baze podataka i korisničkih dozvola za račun.

1.  Na početnom zaslonu upišite **SQL Server Management**, a zatim **2014 paketa SQL Server Management Studio**.
2.  U **Povezivanje s poslužiteljem**, kliknite **Poveži**.
3.  U oknu stabla objekt Explorer desnom tipkom miša kliknite **SQL1**, a zatim **Svojstva**.
4.  U prozoru **Svojstva poslužitelja** , kliknite **Postavke baze podataka**.
5.  Pronađite na **zadano mjesto baze podataka** i postavljanje ove vrijednosti: 
    - Vrste **podataka**unesite put **f:\Data**.
    - **Zapisnik**, upišite put **f:\Log**.
    - Za **sigurnosne kopije**, upišite put **f:\Backup**.
    - Napomena: Samo nove baze podataka pomoću tih mjesta.
6.  Kliknite u **u redu** da biste zatvorili prozor.
7.  U oknu stabla **Objekt Explorer** otvorite **Sigurnost**.
8.  Desnom tipkom miša kliknite **prijave** , a zatim kliknite **Novi prijava**.
9.  U polje **ime za prijavu**upišite **CORP\User1**.
10. Na stranici **Uloge poslužitelja** kliknite **sustava**, a zatim kliknite **u redu**.
11. Zatvorite Microsoft SQL Server Management Studio.

Ovo je trenutna konfiguracija.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)
 
## <a name="phase-3-configure-the-lob-server-lob1"></a>Faza 3: Konfiguriranje LOB poslužitelja (LOB1)

Prvo, stvorite virtualnog računala za LOB1 pomoću ove naredbe u naredbenom retku Azure PowerShell na lokalnom računalu.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Nakon toga pomoću portala za Azure povezivanje s LOB1 pomoću vjerodajnica za lokalni administratorski račun LOB1.

Nakon toga konfigurirati pravila vatrozida za Windows da dopušta promet za testiranje osnovne. Iz programa administratorske komponente Windows PowerShell naredbenog retka na LOB1, pokreće te naredbe.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Naredbe ping moraju rezultirati četiri uspješno Odgovori s IP adresom 192.168.0.4.

Nakon toga pridružite LOB1 domene CORP Active Directory pomoću ove naredbe komponente Windows PowerShell zatraži.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Navedite vjerodajnica računa domene za naredbu **Dodaj računala** pomoću računa za CORP\User1 kada se to od vas zatraži.

Nakon ponovnog pokretanja, pomoću portala za Azure povezati LOB1 s CORP\User1 račun i lozinku.

Nakon toga konfiguriranje LOB1 za IIS i testiranje pristup s CLIENT1.

1.  Iz Upravitelj poslužitelja, kliknite **Dodaj uloga i značajki**.
2.  Na stranici **prije nego što počnete** , kliknite **Dalje**.
3.  Na stranici **Odabir vrste instalacije** kliknite **Dalje**.
4.  Na stranici **Odaberite odredišnom poslužitelju** , kliknite **Dalje**.
5.  Na stranici **ulogama poslužitelja** kliknite **Web Server (IIS)** na popisu **uloge**.
6.  Kada se to od vas zatraži, kliknite **Dodavanje značajki**, a zatim kliknite **Dalje**.
7.  Na stranici **Odaberite značajke** , kliknite **Dalje**.
8.  Na stranici **Web Server (IIS)** , kliknite **Dalje**.
9.  Na stranici **Odabir servisa uloga** odaberite ili poništite potvrdne okvire za servise koji vam je potrebna za testiranje LOB aplikacije i zatim kliknite **Dalje**.
10. Na stranici **Potvrda odabire za instalaciju** kliknite **Instaliraj**.
11. Pričekajte da se dovrši instalacija komponente, a zatim kliknite **Zatvori**.
12. Na portalu Azure s računalom CLIENT1 s vjerodajnicama račun CORP\User1 i pokretanje preglednika Internet Explorer.
13. U adresnu traku upišite **http://lob1/** , a zatim pritisnite tipku ENTER. Trebali biste vidjeti zadani IIS 8 web-stranicu.

Ovo je trenutna konfiguracija.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)
 
Ovo okruženje je sada možete uvesti na web-aplikacija na LOB1 i testiranje funkcionalnosti iz CLIENT1 na podmreži Corpnet.

## <a name="next-step"></a>Sljedeći korak

- Dodajte novi virtualnog računala pomoću [portala za Azure](virtual-machines-windows-hero-tutorial.md).
