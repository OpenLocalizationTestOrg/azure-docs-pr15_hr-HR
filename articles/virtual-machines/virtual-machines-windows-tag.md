<properties
   pageTitle="Kako označavanje u VM | Microsoft Azure"
   description="Informirajte se o označavanju virtualni stroj Windows stvorene u Azure pomoću modela implementacije Voditelj resursa"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Kako označavanje virtualnog računala za Windows u Azure


U ovom se članku opisuju različiti načini označavanje virtualnog računala za Windows u Azure kroz model implementacije Voditelj resursa. Oznake su parove korisnički definirane ključ/vrijednosti koje se može smjestiti izravno na resurs ili grupu resursa. Azure trenutno podržava 15 oznake po resursa i grupa resursa. Oznake može smjestiti na resursa u vrijeme stvaranja ili dodati postojeći resurs. Uzmite u obzir oznake podržani za resurse stvoren pomoću modela resursima implementacije samo. Ako želite da biste označili Linux virtualnog računala, potražite u članku [o označavanju Linux virtualnog računala u Azure](virtual-machines-linux-tag.md).

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Označavanje sa servisom PowerShell

Da biste stvorili, dodavanje i brisanje oznaka putem komponente PowerShell, koji prvi treba li vam da biste postavili [okruženje ljuske PowerShell s Azure Voditelj resursa][]. Nakon što ste dovršili instalaciju, možete postaviti oznake računalnim, mreže i pohranu resursa stvaranja ili nakon stvaranja resursa PowerShell. U ovom se članku koncentrirajte se na prikaz i uređivanje oznake na virtualnim računalima.

Najprije otvorite virtualnog računala putem na `Get-AzureRmVM` cmdlet.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Ako virtualnog računala već sadrži oznake, zatim vidjet ćete sve oznake na vašem resursa:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Ako želite dodati oznake putem komponente PowerShell, možete koristiti u `Set-AzureRmResource` naredbe. Imajte na umu prilikom ažuriranja oznake putem komponente PowerShell oznake ažuriraju kao cjelinu. Stoga Ako dodajete jednu oznaku resursa u kojem je već instalirana oznake, morat ćete obuhvatite sve oznake koje želite smjestiti resurs. Slijedi primjer kako dodati dodatne oznake resursa pomoću cmdleta ljuske PowerShell.

Ovaj cmdlet prvi postavlja sve oznake stavite na *MyTestVM* varijabli *$tags* pomoću na `Get-AzureRmResource` i `Tags` svojstvo.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

Drugi naredba prikazuje oznake za dani varijablu.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment

Treći naredba dodaje dodatnih oznaka *$tags* varijabli. Imajte na umu korištenje na **+=** da biste dodali novi par ključa vrijednosti na popisu *$tags* .

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

Četvrti naredba postavlja sve oznake definirane varijable *$tags* navedeni resurs. U ovom slučaju je MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Peti naredba prikazuje sve oznake resurs. Kao što vidite, *mjesto* sada definirana je kao oznaku pomoću *MyLocation* kao vrijednost.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment
        Value       MyLocation
        Name        Location

Dodatne informacije o označavanju putem komponente PowerShell pogledajte [Cmdleta Azure resursa][].

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Daljnji koraci

* Dodatne informacije o označavanju Azure resursa potražite u članku [Pregled resursima Azure][] i [Korištenje oznake da biste organizirali Azure resursa][].
* Da biste vidjeli kako oznake omogućuju upravljanje korištenje Azure resursa, potražite u članku [objašnjenje računa Azure][] i [dobiti uvida u svoje potrošnje resursa za Microsoft Azure][].

[Okruženje ljuske PowerShell s Azure Voditelj resursa]: ../powershell-azure-resource-manager.md
[Cmdleti za Azure resursa]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Pregled Azure Voditelj resursa]: ../azure-resource-manager/resource-group-overview.md
[Korištenje oznaka da biste organizirali resursi za Azure]: ../resource-group-using-tags.md
[Objašnjenje računa za Azure]: ../billing/billing-understand-your-bill.md
[Rast uvida u svoje potrošnje resursa za Microsoft Azure]: ../billing-usage-rate-card-overview.md
