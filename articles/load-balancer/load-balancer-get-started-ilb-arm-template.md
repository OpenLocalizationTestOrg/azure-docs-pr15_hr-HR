<properties
   pageTitle="Stvaranje programa Interna opterećenja pomoću predloška u upravitelju resursa | Microsoft Azure"
   description="Saznajte kako stvoriti na interni opterećenja pomoću predloška u upravitelju resursa"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-a-template"></a>Stvaranje programa Interna opterećenja pomoću predloška

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][uvođenje klasičnog modela](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Uvođenje predloška pritiskom za implementaciju

Predložak uzorak u spremištu javno koristi parametar datoteku koja sadrži zadane vrijednosti za generiranje scenarij prethodno opisan. Da biste implementirali ovaj predložak pomoću kliknite implementacija, slijedite [ove veze](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), zatim **Implementiraj za Azure**, zamijenite zadane vrijednosti parametra ako je potrebno i slijedite upute na portalu.

## <a name="deploy-the-template-by-using-powershell"></a>Uvođenje predloška pomoću komponente PowerShell

Da biste implementirali predložak koji ste preuzeli pomoću komponente PowerShell, slijedite korake u nastavku.

1. Ako još niste koristili Azure PowerShell, pročitajte članak [Instaliranje i konfiguriranje Azure PowerShell](../../articles/powershell-install-configure.md) i slijedite upute do kraja do kraja se prijaviti u Azure i odaberite svoju pretplatu.
2. Preuzmite datoteku parametara na lokalni disk.
3. Uredite datoteku i spremite ga.
4. Pokrenite cmdlet **Novo AzureRmResourceGroupDeployment** da biste stvorili grupu resursa pomoću ovog predloška.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Uvođenje predloška pomoću EŽA Azure

Da biste pomoću EŽA Azure uvođenje predloška, slijedite korake u nastavku.

1. Ako još niste koristili Azure EŽA, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../../articles/xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.
2. Pokrenite naredbu **azure config način** da biste prešli u način Voditelj resursa, kao što je prikazano u nastavku.

        azure config mode arm

    Evo očekivanog izlaza iznad naredbe:

        info:    New mode is arm

3. Otvorite datoteku parametar odaberite njegov sadržaj, a spremite datoteku na računalu. Primjerice, ne možemo spremili datoteku parametara *parameters.json*.

4. Pokrenite naredbu **Stvaranje implementacije azure grupe** za implementaciju novi interni raspoređivača opterećenja pomoću datoteke predloška i parametar preuzeti i izmijeniti iznad. Popis nakon izlaz objašnjava parametri korišteni.

        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Daljnji koraci

[Konfiguriranje načina distribucije raspoređivača učitavanja pomoću afiniteta izvorni IP](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)



