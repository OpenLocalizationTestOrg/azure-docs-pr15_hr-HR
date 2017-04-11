<properties
   pageTitle="Početak rada prilikom stvaranja internetsku nasuprotne opterećenja u klasičan način rada pomoću komponente PowerShell | Microsoft Azure"
   description="Saznajte kako stvoriti internetsku nasuprotne opterećenja u klasičan način rada pomoću komponente PowerShell"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Početak rada prilikom stvaranja internetsku nasuprotne opterećenja (klasični) u ljusci PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model klasični implementacije. Možete i [Saznajte kako stvoriti internetsku nasuprotne opterećenja pomoću upravitelja resursa Azure](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## <a name="set-up-load-balancer-using-powershell"></a>Postavljanje opterećenja pomoću komponente PowerShell

Da biste postavili raspoređivača opterećenja pomoću komponente powershell, slijedite ove korake:

1. Ako još niste koristili Azure PowerShell, pročitajte članak [Instaliranje i konfiguriranje Azure PowerShell](../../articles/powershell-install-configure.md) i slijedite upute do kraja do kraja se prijaviti u Azure i odaberite svoju pretplatu.


2. Nakon stvaranja virtualnog računala, pomoću cmdleta ljuske PowerShell da biste dodali raspoređivača opterećenja virtualnog računala unutar iste servis u oblaku.

U sljedećem primjeru će dodati raspoređivača opterećenja postavljanje pod nazivom "webfarm" na cloud servisa "mytestcloud" (ili myctestcloud.cloudapp.net), dodavanje krajnje točke za raspoređivača opterećenja virtualnim strojevima pod nazivom "webu1" i "webu2". Raspoređivača opterećenja prima mrežnog prometa na priključak 80 i učitavanje salda između virtualnih računala definira lokalne krajnju točku (u ovom slučaju priključak 80) pomoću TCP.


### <a name="step-1"></a>Korak 1
Stvaranje krajnje točke rasporediti opterećenje za prvu VM "webu1"

    Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### <a name="step-2"></a>Korak 2

Stvaranje drugog krajnja točka za drugi VM "webu2" dajte isti naziv skupa raspoređivača učitavanja

    Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Uklanjanje virtualnog računala s raspoređivača opterećenja

Uklanjanje AzureEndpoint možete koristiti da biste uklonili krajnju točku virtualnog računala s raspoređivača opterećenja

    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## <a name="next-steps"></a>Daljnji koraci

Možete i [Početak rada prilikom stvaranja interne opterećenja](load-balancer-get-started-ilb-classic-ps.md) i konfiguriranje koju vrstu [način distribucije](load-balancer-distribution-mode.md) za programa especific opterećenja mrežni promet funkcioniranje učitavanja.

Ako aplikacija nije potrebno održavanje veza aktivnosti za poslužitelje iza raspoređivača opterećenja, koje mogu razumjeti dodatne informacije o [neaktivnosti TCP vremenskog ograničenja postavke raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md). To će pomoći da biste saznali o ponašanju neaktivne veze prilikom korištenja Azure raspoređivača opterećenja.

