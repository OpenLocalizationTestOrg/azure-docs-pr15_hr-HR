<properties
   pageTitle="Vrste čvor tkanina servisa i skupova skaliranje VM | Microsoft Azure"
   description="Opisuje kako servisa tkanina čvor vrste odnose se na VM skaliranje skupova i na udaljenom povezivanja sa instancu komponente postavite skaliranje VM ili čvor klaster."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Odnos između servisa tkanina čvor vrste i skupova skaliranje virtualnog računala

Skupovi skaliranje virtualnog računala su programa Azure izračunati resursa možete koristiti i upravljanje skupovima virtualnim strojevima u skupu. Svaka vrsta čvora koji je definiran na servis tkanina postavljena kao zasebna VM skaliranje skup. Svaka vrsta čvor pa možete neproporcionalno prema gore ili dolje neovisno o imaju različite skupove otvorene priključke i može imati kapacitet različitih metriku.

Na sljedećoj je snimci zaslona prikazano klaster koji sadrži dvije vrste čvor: sučelju i pozadinskog.  Svaka vrsta čvor ima pet čvorove.

![Snimka zaslona s klaster koji sadrži dvije vrste čvor][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>Mapiranje VM skaliranje postaviti pojave na čvorove

Kao što možete vidjeti iznad, postavite skaliranje VM instance pokretanje instanci 0, a zatim stoji prema gore Numeriranje prikazuje se u nazivima. Na primjer, BackEnd_0 je instancu 0 skupa pozadinskog VM mjerilo. Ovaj određeni VM skaliranje skup sadrži pet instance, pod nazivom BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 i BackEnd_4.

Kada skaliranje gore VM skaliranje skup stvara se nove instance. Postavite skaliranje VM instancu naziv novog obično bit će naziv postavite skaliranje VM + sljedeći broj instanci. U našem primjeru je BackEnd_5.


## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Mapiranje VM skaliranje postavite učitavanja balancers svaki čvor vrsta/VM skaliranje skup

Ako ste implementiran svoj klaster s portala sustava ili ste koristili oglednog predloška Voditelj resursa koju smo dobili, zatim kada se prikaže popis svih resursa u odjeljku grupa resursa će biti navedeno balancers opterećenja za svaku vrstu postavite skaliranje VM ili čvor.

Naziv će otprilike ovako: **LB -&lt;naziv NodeType&gt;**. Na primjer, LB-sfcluster4doc-0, kao što je prikazano u snimka zaslona:


![Resursi][Resources]


## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Alat za analizu daljinske povezati s instancom postavite skaliranje VM ili čvor klaster
Svaka vrsta čvora koji je definiran klasteru postavljena kao zasebna VM skaliranje skup.  To znači da se vrste čvor možete neproporcionalno gore ili dolje neovisno i može se sastoji od različitih VM SKU-ove. Za razliku od instancu VMs, postavite skaliranje VM instance se virtualna IP adresa od vlastite. Stoga može se malo zahtjevne kada tražite IP adresa i priključka koji možete koristiti za daljinski povezati konkretnu instancu.

Evo nekoliko koraka možete pratiti da biste otkrili ih.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Korak 1: Saznajte virtualna IP adresa za vrstu čvor, a zatim NAT ulazna pravila za RDP

Da biste dobili koji, morate nabaviti ulazne vrijednosti pravila NAT koji su definirani u sklopu definiciju resursa za **Microsoft.Network/loadBalancers**.

Na portalu idite na plohu raspoređivača opterećenja, a zatim **Postavke**.

![LBBlade][LBBlade]


U odjeljku **Postavke**kliknite **NAT ulazna pravila**. Ovaj sada vam IP adresa i priključka koji možete koristiti za daljinski povežite prvu instancu postavite skaliranje VM. U nastavku snimka je **104.42.106.156** i **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>Korak 2: Pronalaženje izlaz priključka koji možete koristiti za analizu daljinske povezati određene postavite skaliranje VM instanci/čvor

Ranije u ovom dokumentu I bila riječ o kako instance postavite skaliranje VM mapiranje čvorove. Koristit ćemo koji da biste utvrdili točno priključak.

Priključci su dodijeliti uzlaznim redoslijedom postavite skaliranje VM instance. Da bi se u moj primjer vrsta čvora sučelju priključke za svaku od pet instanci su sljedeće. Sada morate učiniti isti mapiranje za vaše postavite skaliranje VM instancu.

|**Instancu postavite skaliranje VM**|**Priključak**|
|-----------------------|--------------------------|
|FrontEnd_0|3389|
|FrontEnd_1|3390|
|FrontEnd_2|3391|
|FrontEnd_3|3392|
|FrontEnd_4|3393|
|FrontEnd_5|3394|


### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>Korak 3: Alat za analizu daljinske povezivanje na konkretnu instancu postavite skaliranje VM

U nastavku snimku zaslona da biste se povezali s FrontEnd_1 koristiti udaljene radne površine:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>Kako promijeniti RDP priključak raspon vrijednosti

### <a name="before-cluster-deployment"></a>Prije implementacije klaster

Kada postavljate klaster pomoću predloška Voditelj resursa, možete navesti raspon u **inboundNatPools**.

Idite na definiciju resursa za **Microsoft.Network/loadBalancers**. U odjeljku koje možete pronaći opis **inboundNatPools**.  Zamjena vrijednosti *frontendPortRangeStart* i *frontendPortRangeEnd* .

![InboundNatPools][InboundNatPools]


### <a name="after-cluster-deployment"></a>Nakon implementacije klaster
Ovo je nešto složenije i može uzrokovati VMs početak reciklira. Sada će imati da biste postavili nove vrijednosti pomoću komponente PowerShell Azure. Provjerite je li Azure PowerShell 1.0 ili noviji instaliran na vašem računalu. Ako niste to prije, li svakako Predložite slijedite korake navedene u [kako instalirati i konfigurirati Azure PowerShell.](../powershell-install-configure.md)

Prijavite se na račun za Azure. Ako zbog nekog razloga ne uspije sljedeću naredbu komponente PowerShell, provjerite je li Azure PowerShell ispravno instaliran.

```
Login-AzureRmAccount
```

Pokrenite sljedeće da biste dobili pojedinosti o raspoređivača opterećenja i prikazat će se vrijednosti za opis **inboundNatPools**:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Sada postavite *frontendPortRangeEnd* i *frontendPortRangeStart* na željene vrijednosti.

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Daljnji koraci

- [Pregled značajki "Implementacija bilo kojeg mjesta" i Usporedba s upravljanjem Azure klastere](service-fabric-deploy-anywhere.md)
- [Sigurnost klaster](service-fabric-cluster-security.md)
- [Servis tkanina SDK i početak rada](service-fabric-get-started.md)


<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
