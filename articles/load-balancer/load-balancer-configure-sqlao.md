<properties
   pageTitle="Konfiguriranje opterećenja za SQL uvijek na | Microsoft Azure"
   description="Konfiguriranje opterećenja raditi s SQL uvijek i upute za korištenje ljuske powershell za stvaranje opterećenja za implementaciju SQL"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-load-balancer-for-sql-always-on"></a>Konfiguriranje opterećenja za SQL uvijek na

Grupe dostupnosti za SQL Server AlwaysOn sada mogu se pokrenuti s ILB. Grupe dostupnosti je SQL Server flagship rješenje za visoke dostupnosti i Izrada oporavak. Grupa ga Slušatelj dostupnost klijenta aplikacijama omogućuje jednostavno povezati primarni replike, bez obzira broj replike u konfiguraciji.

Naziv ga slušatelj (DNS) je mapirati uravnoteženja IP adresa i Azure, opterećenja usmjerava dolazne promet na primarnom poslužitelju u odjeljku Postavljanje replike.

Podrška za ILB možete koristiti za krajnje točke AlwaysOn SQL Server (ga slušatelj). Sada kontrolu nad pristupačnosti ga slušatelj i možete odabrati IP adresa uravnoteženja određene podmreže u virtualne mreže (VNet).

Pomoću ILB na ga slušatelj, krajnja točka za SQL server (primjerice poslužitelj = tcp:ListenerName, 1433; baza podataka = ImeBazePodataka) dostupni su samo:

- Usluge i VMs u istoj mreži virtualne
- Usluge i VMs iz povezanih lokalne mreže
- Usluge i VMs iz međusobno povezanih VNets

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Slika 1 – SQL AlwaysOn konfiguriran pomoću opterećenja mjesto na Internetu

## <a name="add-internal-load-balancer-to-the-service"></a>Dodavanje internog opterećenja sa servisom

1. U sljedećem primjeru, ne možemo će konfigurirati virtualne mrežom koja sadrži podmreže pod nazivom "Podmreže-1":

        Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc

2. Dodavanje krajnje točke rasporediti opterećenje za ILB na svakom VM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –
        DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    U primjeru iznad, imate 2 VM pod nazivom "sqlsvc1" i "sqlsvc2" izvodi u oblaku servisa "Sqlsvc". Nakon stvaranja u ILB s parametrom "DirectServerReturn", dodajte krajnje točke rasporediti opterećenje ILB da biste omogućili SQL da biste konfigurirali slušače za grupe dostupnosti.

Dodatne informacije o SQL AlwaysOn potražite u članku [Konfiguriranje Interna opterećenja za grupe dostupnosti AlwaysOn u Azure](../virtual-machines/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Vidi također

[Početak rada konfiguriranje internetsku nasuprotne opterećenja](load-balancer-get-started-internet-arm-ps.md)

[Početak rada konfiguriranje Interna opterećenja](load-balancer-get-started-ilb-arm-ps.md)

[Konfiguriranje načina raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)
