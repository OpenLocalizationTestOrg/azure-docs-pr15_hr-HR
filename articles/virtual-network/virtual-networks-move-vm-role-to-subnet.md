<properties 
   pageTitle="Kako premjestiti VM ili uloga instance drugoj podmreži"
   description="Saznajte kako možete premjestiti VMs i uloga instance drugoj podmreži"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-move-a-vm-or-role-instance-to-a-different-subnet"></a>Kako premjestiti VM ili uloga instance drugoj podmreži

PowerShell možete koristiti da biste premjestili na VMs iz jednog podmreži na drugo u istoj virtualne mreže (VNet). Uloga instance možete premjestiti tako da na CSCFG za uređivanje, a ne pomoću komponente PowerShell.

>[AZURE.NOTE] Ovaj članak sadrži informacije koje se odnosi Azure klasični implementacijama samo.

Zašto premjestiti VMs drugi podmreže? Migracija podmreže je korisno kada starije podmreže je presitan i ne mogu se proširiti zbog postojećih izvodi VMs u tom podmreže. U tom slučaju možete stvoriti novu, veća podmreže i migrirati u VMs u novi podmreži, a zatim nakon dovršetka migracije, možete izbrisati stari podmreže prazan.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Kako premjestiti na VM drugi podmreže

Da biste premjestili na VM, pokrenite cmdlet skup AzureSubnet PowerShell pomoću primjeru niže kao predloška. U primjeru u nastavku ćemo premještate TestVM iz njegova izlaganje podmreže podmreže 2. Provjerite da biste uredili u primjeru u skladu s vizualnim vaše okruženje. Imajte na umu da svaki put kada pokrenete cmdlet ažuriranje AzureVM kao dio postupak, ona će se pokrenuti vaše VM kao dio postupku ažuriranja.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
  	| Set-AzureSubnet –SubnetNames Subnet-2 `
  	| Update-AzureVM

Ako ste odredili statičke IP Interna privatne za vaše VM, morat ćete isključite tu postavku, prije nego što možete premjestiti na VM podmreži novi. U tom slučaju, koristite sljedeće:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Set-AzureSubnet -SubnetNames Subnet-2 `
  	| Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Da biste premjestili uloga instance drugi podmreže

Da biste premjestili uloga instance, uredite datoteku CSCFG. U primjeru u nastavku ćemo premještate "Role0" u virtualne mreže *VNETName* iz njegova izlaganje podmreže *podmreže 2*. Jer instancu ulogu već implementiran, samo promijenit ćete naziv podmreže = podmreže 2. Provjerite da biste uredili u primjeru u skladu s vizualnim vaše okruženje.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
