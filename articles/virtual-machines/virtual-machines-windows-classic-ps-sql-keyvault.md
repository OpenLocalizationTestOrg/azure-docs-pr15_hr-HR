<properties
    pageTitle="Konfigurirao integraciju Azure sigurnog ključa za SQL Server na Azure VMs (Classic)"
    description="Saznajte kako automatizirati konfiguracije SQL Server šifriranje za Azure ključ sigurnog. U ovoj se temi objašnjava kako pomoću Azure ključ sigurnog Integracija sa sustavom SQL Server na virtualnim strojevima stvoriti u modelu klasični implementacije."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-classic"></a>Konfigurirao integraciju Azure sigurnog ključa za SQL Server na Azure VMs (Classic)

> [AZURE.SELECTOR]
- [Voditelj resursa](virtual-machines-windows-ps-sql-keyvault.md)
- [Klasični](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Pregled
Nema više značajke šifriranja SQL Server, kao što su [šifriranje prozirne podataka (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [stupac razine šifriranja (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)i [sigurnosne kopije šifriranje](https://msdn.microsoft.com/library/dn449489.aspx). Ove obrasce šifriranje potrebno za upravljanje i spremanje tipki šifriranja koji koristite za šifriranje. Poboljšanje sigurnosti i upravljanje ove tipke na mjestu sigurne i vrlo dostupna namijenjen je servisa Azure ključ sigurnog (AKV). [Poveznik za SQL Server](http://www.microsoft.com/download/details.aspx?id=45344) omogućuje SQL Server za korištenje ove tipke iz zbirke ključeva Azure ključ.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Ako koristite SQL Server s lokalnog računala, potrebni su [koraci pomoću kojih možete pristupiti sigurnog ključ Azure s vašeg računala lokalnog sustava SQL Server](https://msdn.microsoft.com/library/dn198405.aspx). No za SQL Server u Azure VMs, uštedjet ćete vrijeme pomoću značajke za *Integraciju sigurnog ključ za Azure* . Pomoću nekoliko Azure PowerShell Cmdlete da biste omogućili ovu značajku, možete automatizirati potrebne za SQL VM za pristup vašem ključa sigurnog konfiguracije.

Kada je omogućen ovu značajku, ga automatski instalira poveznik za SQL Server, konfigurira davatelja EKM da biste pristupili sigurnog ključ Azure i stvara vjerodajnica da biste mogli pristupiti vašem sigurnog. Ako gledali korake navedene u dokumentaciji prethodno spomenuta lokalnog, vidjet ćete da značajka automatizira korake 2 i 3. Samo stvar će i dalje morate ručno je da biste stvorili ključa sigurnog i ključevi. Iz nje, automatski se cijeli postavljanja sustava SQL VM. Kada značajka dovrši instalacijski program, možete izvršiti T SQL naredbe da biste započeli šifriranje baze podataka ili sigurnosne kopije kao što to obično činite.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>Konfigurirao integraciju AKV
Korištenje ljuske PowerShell za konfigurirao integraciju sigurnog ključ za Azure. Sljedeći odjeljci sadrže pregled obavezni parametri, a zatim skriptu PowerShell uzorka.

### <a name="install-the-sql-server-iaas-extension"></a>Instalirajte proširenje IaaS SQL Server

Prvo [Instaliranje IaaS proširenja za SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>Razumijevanje ulaznih parametara
U sljedećoj su tablici navedeni parametri potrebni za pokretanje skriptu PowerShell u sljedećem odjeljku.

|Parametar|Opis|Primjer|
|---|---|---|
|**$akvURL**|**Ključni sigurnog URL-a**|"https://contosokeyvault.vault.azure.net/"|
|**$spName**|**Glavno ime usluge**|"fde2b411 - 33d 5-4e11-af04eb07b669ccf2"|
|**$spSecret**|**Tajna usluge**|"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM ="|
|**$credName**|**Naziv vjerodajnica**: Integracija AKV stvara vjerodajnica unutar sustava SQL Server, dopustite VM da biste imali pristup ključa zbirke ključeva. Odaberite naziv ove vjerodajnice.|"mycred1"|
|**$vmName**|**Naziv virtualnog računala**: naziv prethodno stvorenog VM SQL.|"myvmname"|
|**$serviceName**|**Naziv usluge**: U Oblaku naziv koji je povezan s SQL VM.|"mycloudservicename"|

### <a name="enable-akv-integration-with-powershell"></a>Omogućite AKV integraciju sa servisom PowerShell
Cmdlet **Novo AzureVMSqlServerKeyVaultCredentialConfig** stvara objekt konfiguracije za značajku Azure ključ sigurnog integracije. **Postavljanje AzureVMSqlServerExtension** konfigurira Ta integracija s parametrom **KeyVaultCredentialSettings** . Sljedeći koraci pokazati kako pomoću ove naredbe.

1. Azure PowerShell najprije konfigurirati ulaznih parametara s određenim vrijednostima kao što je opisano u ranijim odlomcima ove teme. Primjer je sljedeću skriptu.

        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2.  Zatim koristite sljedeću skriptu konfigurirati i omogućili integraciju sa AKV.

        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

Proširenje Agent SQL IaaS ažurirat će se SQL VM s ovom novi konfiguracijom.

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
