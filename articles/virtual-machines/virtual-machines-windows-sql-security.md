<properties
    pageTitle="Sigurnosna pitanja vezana uz za SQL Server u Azure | Microsoft Azure"
    description="U ovoj se temi odnosi na resursa stvorene s modelom uvođenje klasičnog te omogućuje opće smjernice za osiguravanje SQL Server koji se izvodi u programa Azure virtualnog računala."
    services="virtual-machines-windows"
    documentationCenter="na"
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
    ms.date="06/24/2016"
    ms.author="jroth" />

# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Sigurnosna pitanja vezana uz za SQL Server na virtualnim računalima za Azure
 
U ovoj se temi sadrži ukupne upute o sigurnosti koje pomoći uspostavljanje sigurnog pristupa instance sustava SQL Server u VM programa Azure. No da biste bili sigurni bolje zaštitu vaše instance baze podataka SQL Server Azure, preporučujemo implementirati sigurnosne navike tradicionalni lokalnog uz sigurnost najbolje prakse za Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Dodatne informacije o sigurnosti prakse SQL Server potražite u članku [SQL Server 2008 R2 sigurnost najbolje prakse - radu i administrativne zadatke](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx)

Azure usklađuje s nekoliko industrijskih propisa i Standardi kojih možete sastaviti rješenje usklađen sa sustavom SQL Server koji se izvodi u virtualnog računala. Informacije o usklađenosti s propisima s Azure, potražite u članku [Centar za pouzdanost Azure](https://azure.microsoft.com/support/trust-center/).

Slijedi popis preporuke o sigurnosti koje razmatranje prilikom konfiguriranja i povezivanje na instancu sustava SQL Server u VM programa Azure.

## <a name="considerations-for-managing-accounts"></a>Zahtjevi za upravljanje računima:

- Stvaranje jedinstvenih lokalni administratorski račun koji se naziva **Administrator**.

- Koristite complex neprobojne lozinke za sve račune. Dodatne informacije o stvaranju neprobojne lozinke potražite u članku [Savjeti za stvaranje neprobojne lozinke](http://windows.microsoft.com/en-us/windows-vista/Tips-for-creating-a-strong-password) članka.

- Prema zadanim postavkama Azure odabire provjeru autentičnosti sustava Windows tijekom postavljanja sustava SQL Server virtualnog računala. Zbog toga prijava **s** je onemogućen i instalacijskog programa je dodijeljena lozinka. Ne možemo preporučeno koje treba prijava **s** neće biti koristi ili omogućena. Slijede zamjenski strategije želji prijava SQL:
    - Stvaranje SQL računa koji ima sustava članstvo.
    - Ako morate koristiti **Pacifička** prijave, omogućivanje prijave i preimenovati i novu lozinku.
    - Obje mogućnosti koje su ranije spomenutih potrebna promjena način provjere autentičnosti za **SQL Server i način provjere autentičnosti sustava Windows**. Dodatne informacije potražite u članku [Promjena Server način provjere autentičnosti](https://msdn.microsoft.com/library/ms188670.aspx).

## <a name="considerations-for-securing-connections-to-azure-virtual-machine"></a>Okolnosti pri zaštiti veze Azure virtualnog računala:

- Razmislite o korištenju [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md) za administriranje virtualnim strojevima umjesto javno RDP priključci.

- Pomoću [Mreže sigurnosne grupe](../virtual-network/virtual-networks-nsg.md) (NSG) dopustiti ili zabraniti mrežni promet za virtualnog računala. Ako želite koristiti za NSG i već imam krajnje ACL na mjestu, najprije uklonite krajnjoj točki ACL. Informacije o tome kako to učiniti potražite u članku [Upravljanje pristupom kontrola popise (ACL) za krajnje točke pomoću komponente PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

- Ako koristite krajnje točke, uklonite sve krajnje točke na virtualnog računala ako ih ne koristite. Upute o korištenju ACL-a s krajnje točke potražite u članku [Upravljanje ACL krajnje točke](../virtual-network/virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

- Omogućite mogućnost šifriranu vezu instance komponente modul baze podataka SQL Server u Azure virtualnih računala. Konfiguracija instancu sustava SQL server s potvrdom traje. Dodatne informacije potražite u članku [Omogućivanje šifrirane veze modul baze podataka](https://msdn.microsoft.com/library/ms191192.aspx) i [Sintaksa niza veze](https://msdn.microsoft.com/library/ms254500.aspx).

- Ako virtualnim strojevima treba može pristupiti samo s određenim mreže, koristite vatrozid za Windows da biste ograničili pristup određene IP adresa ili podmreže mreže.

## <a name="next-steps"></a>Daljnji koraci

Ako vas zanima i najbolje prakse oko performanse, potražite u članku [Performanse najbolje prakse za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-performance.md).

Druge teme vezane uz izvodi SQL Server u Azure VMs, potražite u članku [SQL Server na virtualnim računalima sustava Azure pregled](virtual-machines-windows-sql-server-iaas-overview.md).
