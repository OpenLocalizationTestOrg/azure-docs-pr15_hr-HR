<properties
    pageTitle="Konfigurirao integraciju Azure sigurnog ključa za SQL Server na Azure VMs (Voditelj resursa)"
    description="Saznajte kako automatizirati konfiguracije SQL Server šifriranje za Azure ključ sigurnog. U ovoj se temi objašnjava kako koristiti Azure ključ sigurnog integracije sa SQL Server virtualnim strojevima stvorene pomoću upravitelja resursa."
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
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-resource-manager"></a>Konfigurirao integraciju Azure sigurnog ključa za SQL Server na Azure VMs (Voditelj resursa)

> [AZURE.SELECTOR]
- [Voditelj resursa](virtual-machines-windows-ps-sql-keyvault.md)
- [Klasični](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Pregled
Nema više značajke šifriranja SQL Server, kao što su [šifriranje prozirne podataka (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [stupac razine šifriranja (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)i [sigurnosne kopije šifriranje](https://msdn.microsoft.com/library/dn449489.aspx). Ove obrasce šifriranje potrebno za upravljanje i spremanje tipki šifriranja koji koristite za šifriranje. Poboljšanje sigurnosti i upravljanje ove tipke na mjestu sigurne i vrlo dostupna namijenjen je servisa Azure ključ sigurnog (AKV). [Poveznik za SQL Server](http://www.microsoft.com/download/details.aspx?id=45344) omogućuje SQL Server za korištenje ove tipke iz zbirke ključeva Azure ključ.

Ako je li SQL Server s lokalnog računala, postoji nekoliko [koraka možete poduzeti da biste pristupili sigurnog ključ Azure s vašeg računala lokalnog sustava SQL Server](https://msdn.microsoft.com/library/dn198405.aspx). No za SQL Server u Azure VMs, uštedjet ćete vrijeme pomoću značajke za *Integraciju sigurnog ključ za Azure* .

Kada je omogućen ovu značajku, ga automatski instalira poveznik za SQL Server, konfigurira davatelja EKM da biste pristupili sigurnog ključ Azure i stvara vjerodajnica da biste mogli pristupiti vašem sigurnog. Ako gledali korake navedene u dokumentaciji prethodno spomenuta lokalnog, vidjet ćete da značajka automatizira korake 2 i 3. Samo stvar će i dalje morate ručno je da biste stvorili ključa sigurnog i ključevi. Iz nje, automatski se cijeli postavljanja sustava SQL VM. Kada značajka dovrši instalacijski program, možete izvršiti T SQL naredbe da biste započeli šifriranje baze podataka ili sigurnosne kopije kao što to obično činite.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Omogućavanje i konfiguriranje AKV Integracija
Možete omogućiti integraciju AKV tijekom dodjeljivanja ili konfigurirati za postojeće VMs.

### <a name="new-vms"></a>Novi VMs
Ako su dodjeljivanje novi virtualni stroj SQL Server s Voditelj resursa, portala za Azure nudi korake da biste omogućili integraciju sigurnog ključ Azure. Azure ključ sigurnog značajka je dostupna samo za Enterprise, za razvojne inženjere i procjenu izdanja sustava SQL Server.

![SQL Azure ključa sigurnog Integracija](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Detaljni vodič za dodjelu resursa, potražite u članku [Dodjeljivanje virtualnog računala za SQL Server Azure portalu](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Postojeće VMs
Za postojeće SQL Server virtualnim strojevima, odaberite sustava SQL Server virtualnog računala. Zatim u odjeljku **konfiguracije SQL poslužitelja** plohu **Postavke** .

![Integracija s programom SQL AKV za postojeće VMs](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

U plohu **konfiguracije SQL poslužitelja** , kliknite gumb **Uredi** u odjeljku Integracija automatski sigurnog ključ.

![Konfiguriranje SQL AKV Integracija za postojeće VMs](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Kada završite, kliknite gumb **u redu** pri dnu plohu **konfiguracije SQL Server** da biste spremili promjene.

>[AZURE.NOTE] Možete konfigurirati i integracija AKV pomoću predloška. Dodatne informacije potražite u članku [Azure brzi početak rada predloška za integraciju sigurnog ključ Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
