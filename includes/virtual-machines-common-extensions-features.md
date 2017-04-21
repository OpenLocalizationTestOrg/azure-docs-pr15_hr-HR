



Detalje o agenata VM i njihovo funkcioniranje za podršku VM proširenja potražite u članku [VM Agent i pregled VM proširenja](../articles/virtual-machines/virtual-machines-windows-classic-manage-extensions.md).

##<a name="azure-vm-extensions"></a>Azure VM proširenja

Proširenja VM implementirati Većina ključnih funkcija koje želite koristiti s vašeg VMs, uključujući osnovne funkcije kao što su promjene lozinki, konfiguriranje RDP, i mnogo, mnoge druge. Jer novih nastavaka dodat će se cijelo vrijeme, broj mogućih značajki na VMs podržava u Azure i dalje da biste povećali. Prema zadanim postavkama nekoliko osnovnih VM proširenja instaliraju kada stvarate vaše VM iz galerije slika, uključujući **IaaSDiagnostics** (trenutno Windows VMs samo), **VMAccess**i **BGInfo** (i trenutno samo u sustavu Windows). Međutim, nije sva proširenja primjenjuju na Windows i Linux u bilo kojem trenutku određene zbog konstante tijek ažuriranje značajki i nove datotečne nastavke.

##<a name="connectivity-and-basic-management"></a>Osnovni upravljanje i povezivanje

Sljedeći nastavci su ključna za omogućivanje, ponovno omogućite ili onemogućite osnovni povezivanje s vašeg VMs nakon stvaranja i pokrenut.

|Naziv VM proširenja|Opis značajke|Dodatne informacije
|---|---|---|
|VMAccessAgent (Windows)|Stvaranje, ažuriranje i ponovno postavljanje korisničke informacije i konfiguracija RDP veze.|[Windows](../articles/virtual-machines/virtual-machines-windows-classic-extensions-customscript.md)
|VMAccessForLinux (Linux)|Stvaranje, ažuriranje i ponovno postavljanje korisničke informacije i konfiguracija SSH veze.|[Linux](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess)

##<a name="deployment-and-configuration-management"></a>Implementacije i upravljanja konfiguracija

Sljedeće proširenja podržavaju različite vrste implementacije i scenarijima upravljanja konfiguraciju i značajke. Pogledajte i odjeljak o VM operacije i upravljanje ispod.

|Naziv VM proširenja|Opis značajke|Dodatne informacije|
|---|---|---|
|**MSEnterpriseApplication**|Primjenjuje značajke za podršku prema centru sustava Windows.|[Sustav centar 2012 R2 virtualnog računala uloge](http://social.technet.microsoft.com/wiki/contents/articles/18274.system-center-2012-r2-virtual-machine-role-authoring-guide-resource-extension-package.aspx)|
|**Implementacija octopus** (Nastavak DSC sustavom)|Podržava automatski implementacije ASP.NET web-aplikacijama i servisima sustava Windows u razvoju, testiranje i radnog okruženja.|[Uvod u Octopus implementacije](http://docs.octopusdeploy.com/display/OD/Getting%20started)|
|**Upravitelj izdanje za Visual Studio** (Nastavak DSC sustavom)|Podržava neprekinuti implementacija s Visual Studio.|[Automatiziranje implementacijama s upravljanjem izdanje](https://msdn.microsoft.com/Library/vs/alm/Release/overview)|
|**CentosChefClient**|||
|**ChefClient**|Stvara Chef klijenta u sustavu Windows. (Možete koristiti i nastavak DSC dolje.)|[Chef i Microsoft Azure](https://www.getchef.com/solutions/azure/)|
|**LinuxChefClient**|||
|**DockerExtension**|Instalira daemon Docker za podršku udaljene Docker naredbe.|[Kako koristiti Docker proširenje virtualnog računala](../articles/virtual-machines/virtual-machines-linux-dockerextension.md) Širi informacije potražite u članku [Docker VM proširenje korisničkom priručniku](https://github.com/Azure/azure-docker-extension/blob/master/README.md)|
|**DSC**|Nastavak DSC (željena stanja konfiguracija) PowerShell.|[Proširenje Azure PowerShell DSC (želji stanje konfiguracije)](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx)|
|**PuppetEnterpriseAgent**|Implementira značajke Puppet Enterprise. |[Puppet na Azure](http://puppetlabs.com/solutions/microsoft)|
|**CustomScriptExtension** (Windows) **CustomScriptForLinux** (Linux)|U bilo kojem trenutku poziva prilagođene skripte na na VM: pokretanje ili tijekom života.|[Proširenje za prilagođene skripte](../articles/virtual-machines/virtual-machines-windows-classic-extensions-customscript.md) | [Linux](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript)|
|**AzureCATExtensionHandler**|Troši dijagnostičke podatke prikupljene putem **IaaSDiagnostics** i nekoliko drugih izvora podataka kao što su [Analize metrike pohrane za Azure](https://msdn.microsoft.com/library/azure/hh343270.aspx) i pretvara ga u Zbrojeno podataka skupa prikladna za matični SAP kontrola proces trošiti|[Poboljšana Azure nadzor za SAP](https://azure.microsoft.com/blog/2014/06/04/azure-enhanced-monitoring-for-sap/)|

##<a name="security-and-protection"></a>Sigurnost i zaštitu

Nastavci u ovom odjeljku navedite ključnih sigurnosnim značajkama za vaše VMs Azure.

|Naziv VM proširenja|Opis značajke|Dodatne informacije|
|---|---|---|
|**CloudLinkSecureVMWindowsAgent**|Microsoft Azure kupci pruža mogućnost da biste šifrirali svoje podatke virtualnog računala na više klijentu zajedničke infrastrukturu i potpuno kontrolu nad ključeva za šifriranje za svoje šifrirane podatke na infrastrukture Azure prostora za pohranu.|[Zaštita Microsoft Azure virtualnim strojevima korištenje BitLocker i izvorni OS šifriranje](http://www.cloudlinktech.com/azure)|
|**McAfeeEndpointSecurity**|Štiti vaše VM od zlonamjernog softvera.|[Tvrtke McAfee](https://www.mcafeeasap.com/MarketingContent/default.aspx)|
|**TrendMicroDSA**|Omogućuje TrendMicro na niže sigurnost platformu podršku da bi se otkrivanje podataka i sprječavanje, vatrozid, zlonamjernog, ugleda web, provjere zapisnika i nadzor integritet.|[Kako instalirati i konfigurirati Trend Micro precizno sigurnost kao servisa na VM programa Azure](../articles/virtual-machines/virtual-machines-windows-classic-install-trend.md)|
|**PortalProtectExtension**|Guards od prijetnji u okruženju sustava Microsoft SharePoint.|[Zaštita implementaciju sustava SharePoint na Azure](http://blog.trendmicro.com/securing-sharepoint-deployment-azure/)|
|**IaaSAntimalware**|Microsoft Antimalware za servise u Oblaku Azure i virtualnim strojevima je mogućnost zaštita u stvarnom vremenu prepoznavanje i uklanjanje virusa, špijunskih programa i ostalih zlonamjernih programa konfigurirati upozorenja kada zna zlonamjerni ili neželjeni softver pokuša instalirati ili pokrenuti na računalu.|[Antimalware za servise u Oblaku Azure i virtualnih računala](../articles/azure-security-antimalware.md)|
|**SymantecEndpointProtection**|Zaštita na krajnjoj točki Symantec 12.1.4 omogućuje sigurnost i performanse preko fizička i virtualne sustavima.|[Kako instalirati i konfigurirati Zaštita na krajnjoj točki Symantec na VM programa Azure](../articles/virtual-machines/virtual-machines-windows-classic-install-symantec.md)

##<a name="vm-operations-and-management"></a>Operacije VM i upravljanje njima

Podržava značajke upravljanja uobičajenih operacija i ponašanje. Pogledajte i odjeljak o implementacije i upravljanja konfiguracije iznad.

|**Naziv VM proširenja**|Opis značajke|Dodatne informacije|
|---|---|---|
|**AzureVmLogCollector**|Možete koristiti u **AzureVMLogCollector** proširenje osvježavati perfom jednokratno zbirku zapisnika iz jednog ili više VMs oblaka servisa (iz web uloge i uloge suradnika) i prikupljene datoteke za prijenos s poslovnim subjektom Azure prostora za pohranu – sve bez daljinsko prijave na bilo koji od na VMs. |[Proširenje AzureLogCollector](../articles/virtual-machines/virtual-machines-windows-log-collector-extension.md)|
|**IaaSDiagnostics**|Omogućuje, onemogućuje, konfigurira Dijagnostika Azure i koristi **AzureCATExtensionHandler** za podršku SAP-a za nadzor.|[Microsoft Azure virtualnog računala nadzor s nastavkom Azure dijagnostiku](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/)|
|**OSPatchingForLinux**|Na Azure VM administratorima omogućuje automatizaciju VM OS ažurirat prilagođene konfiguracije. Proširenje OSPatching možete koristiti da biste konfigurirali OS ažuriranja za virtualnim strojevima, uključujući: odredite koliko često i kada se instalacija OS zakrpa, navedite što zakrpa instaliranja i konfiguriranja ponašanje ponovno pokrenite računalo nakon ažuriranja|[OS zakrpa proširenje članak na blogu](https://azure.microsoft.com/blog/2014/10/23/automate-linux-vm-os-updates-using-ospatching-extension/). Vidi također Pročitaj i izvora na GitHub pri [Proširenje zakrpa OS](https://github.com/Azure/azure-linux-extensions).|

##<a name="developing-and-debugging"></a>Razvoj i ispravljanje pogrešaka

Ovim nastavcima VM su navedene ovdje za dovršenosti, kao i pruža podršku za značajke vezane uz Visual Studio te se treba koristiti izravno.

|Naziv VM proširenja|Opis značajke|Dodatne informacije|
|---|---|---|
|**VS14CTPDebugger**|Podržava daljinski pogrešaka Dodavanje veze za VANJSKIH pomoću Azure SDK 2.4|[Alat za analizu daljinske ispravljanje pogrešaka u Visual Studio](https://msdn.microsoft.com/library/y7f5zaaa.aspx)|
|**VS2013Debugger**|Podržava daljinski pogrešaka Dodavanje veze za VANJSKIH pomoću Azure SDK 2.4||
|**VS2012Debugger**|Podržava daljinski pogrešaka Dodavanje veze za VANJSKIH pomoću Azure SDK 2.4||
|**RemoteDebugVS14CTP**|Podržava daljinski pogrešaka Dodavanje veze za VANJSKIH pomoću Azure SDK 2.3||
|**RemoteDebugVS2013**|Podržava daljinski pogrešaka Dodavanje veze za VANJSKIH pomoću Azure SDK 2.3||
|**RemoteDebugVS2012**|Podržava daljinski pogrešaka Dodavanje veze za VANJSKIH pomoću Azure SDK 2.3||
|**WebDeployForVSDevTest**|Instalira i konfigurira IIS i implementacija Web Windows Server. Uklanjanje ili ga onemogućiti nije podržana.|

##<a name="miscellaneous-features"></a>Ostale značajke

Ovim nastavcima nudi podršku za ostale VM značajke koje se može biti korisno.

|Naziv VM proširenja|Opis značajke|Dodatne informacije|
|---|---|---|
|**BGInfo**|Prilikom korištenja RDP, prikazuju se konsolidiranom slika poslužitelja korisne informacije na radnoj površini.|[Proširenje BGInfo](https://msdn.microsoft.com/library/mt589195.aspx)|
|**HpcVmDrivers**|Instalira, konfigurira i održava udaljene Izravni memorije pristup (RDMA) mreže upravljačkih na veličinu A8 ili A9 VM sa sustavom Windows Server 2012 R2 ili Windows Server 2012. Omogućuje grupirani A8 ili A9 VMs da biste koristili RDMA mreže prilikom izvođenja paralelno MPI aplikacije.|[O računalnim ćete morati usko instance A8 A9, A10 i A11](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md)

