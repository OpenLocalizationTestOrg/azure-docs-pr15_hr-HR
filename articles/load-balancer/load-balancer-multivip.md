<properties
   pageTitle="Više aplikacijskih VIPs za servise u oblaku"
   description="Pregled multiVIP i kako postaviti više VIPs na neki servis u oblaku"
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

# <a name="configure-multiple-vips-for-a-cloud-service"></a>Konfiguriranje više VIPs za servise u oblaku

Servisi u oblaku Azure mogu pristupati putem javnog interneta pomoću IP adresu nudi Azure. Javnu IP adresu zove VIP (virtualne IP-a) jer je povezana s Azure opterećenja, a ne na virtualnog računala (VM) instance unutar servisa u oblaku. Sve instance VM unutar servis u oblaku mogu pristupati pomoću jednog VIP.

No postoje scenariji u kojima morate više VIP kao unos pokažite na istom servis u oblaku. Ako, primjerice, servis u oblaku možda hostira više web-mjesta koja potrebna je veza s SSL pomoću zadani priključak 443, kao što je svaki web-mjesto nalazi za različite korisnike ili klijent. U ovom slučaju morate koristiti različite javna dostupnog IP adresa za svaku web-mjesto. Dijagramu u nastavku prikazuje uobičajeni više klijentu web-mjesta s potrebe za više SSL certifikata na isti javno priključak.

![Višestruki VIP SSL scenarija](./media/load-balancer-multivip/Figure1.png)

U gornjem primjeru sve VIPs koristiti isti javno priključak (443) i promet se preusmjerava na jednu ili više opterećenja raspoređen VMs na jedinstveni privatne priključak za interne IP adrese servisa u oblaku hosting svih web-mjesta.

>[AZURE.NOTE] Drugi situacija koji zahtijevaju korištenje više VIPs nalazi više SQL AlwaysOn dostupnost grupe slušače na isti skup virtualnim računalima.

VIPs su dinamički prema zadanim postavkama, što znači da je stvarni IP adresa dodijeljena servisa u oblaku mijenjaju tijekom vremena. Da biste to spriječiti, možete rezervirati na VIP servisa. Da biste saznali više o rezervirane VIPs, potražite u članku [Rezervirane javnu IP](../virtual-network/virtual-networks-reserved-public-ip.md).

>[AZURE.NOTE] Pročitajte članak [IP adresa cijene](https://azure.microsoft.com/pricing/details/ip-addresses/) informacije o cijenama na VIPs i Rezervirana IP-ovi.

Možete pomoću komponente PowerShell da biste provjerili VIPs koji se koriste na servise u oblaku, kao i dodavanje i uklanjanje VIPs, pridruživanje VIP za krajnje i konfigurirati na određene VIP za ujednačavanje opterećenja.

## <a name="limitations"></a>Ograničenja

Trenutačno VIP više funkcija ograničeno je na sljedećim scenarijima:

- **Samo IaaS**. Višestruki VIP možete omogućiti samo za servise u oblaku koji sadrže VMs. VIP više ne možete koristiti u PaaS scenariji s instancama uloge.
- **Samo PowerShell**. Višestruki VIP samo možete upravljati pomoću komponente PowerShell.

Ta ograničenja su privremene i može se promijeniti u bilo kojem trenutku. Provjerite jeste li ponovo posjetite ovu stranicu da biste provjerili buduće promjene.


## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Kako dodati na VIP na servis u oblaku

Da biste dodali na VIP uslugu, pokrenite sljedeću naredbu komponente PowerShell:

    Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

Ta naredba prikazuje rezultat slično kao sljedeći primjer:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Kako ukloniti s VIP iz neki servis u oblaku

Da biste uklonili VIP dodati u svoj servis u primjeru iznad, pokrenite sljedeću naredbu komponente PowerShell:

    Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

>[AZURE.IMPORTANT] Na VIP možete ukloniti samo ako je povezan s njom krajnje točke.

## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Upute za dohvaćanje informacija VIP na neki servis u Oblaku

Da biste dohvatili VIPs povezan sa servisom cloud, pokrenite sljedeću skriptu komponente PowerShell:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Skripta prikazuje rezultat koji će biti sličan sljedeći primjer:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

U ovom primjeru servisa u oblaku je 3 VIPs:

- **Vip1** VIP zadane, znate koje jer je vrijednost za IsDnsProgrammedName postavljeno na true.
- **Vip2** i **Vip3** se koriste kao nemate nijednu IP adresu. Oni će koristiti povežete krajnje da biste na VIP.

>[AZURE.NOTE] Vaša pretplata samo naplatiti za dodatni VIPs nakon što su povezani s krajnje točke. Dodatne informacije o cijenama, potražite u članku [IP adresa cijene](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Kako se pridružiti VIP za krajnje točke

Da biste povezali VIP na neki servis u oblaku za krajnje točke, pokrenite sljedeću naredbu komponente PowerShell:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 `
  	| Update-AzureVM

Naredba stvara krajnje povezane s VIP pod nazivom *Vip2* na priključak *80*i veze da biste na VM pod nazivom *myVM1* u oblaku pod nazivom *myService* pomoću *TCP* priključak *8080*.

Da biste provjerili konfiguraciju, pokrenite sljedeću naredbu komponente PowerShell:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Rezultat izgleda slično sljedećem primjeru:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Kako omogućiti na određene VIP za ujednačavanje opterećenja

Jedan VIP možete povezati s više virtualnim strojevima za potrebe za ujednačavanje opterećenja. Ako, na primjer, imate neki servis u oblaku pod nazivom *myService*i dva virtualnim strojevima pod nazivom *myVM1* i *myVM2*. I servis u oblaku ima više VIPs jedan od njih pod nazivom *Vip2*. Ako želite li sve promet za priključak *81* *Vip2* raspoređen je između *myVM1* i *myVM2* na priključak *8181*, pokrenite sljedeću skriptu komponente PowerShell:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

    Get-AzureVM -ServiceName myService -Name myVM2 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

Možete ažurirati i raspoređivača opterećenja da biste koristili različite VIP. Ako, na primjer, ako pokrenete naredbu komponente PowerShell, promijenit ćete postavljanje da biste koristili VIP pod nazivom Vip1 za ujednačavanje opterećenja:

    Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1

## <a name="next-steps"></a>Daljnji koraci

[Zapisnik analize Azure učitavanja saldo](load-balancer-monitor-log.md)

[Internet dostupnog učitavanja opterećenja pregled](load-balancer-internet-overview.md)

[Početak rada na Internetu nasuprotne opterećenja](load-balancer-get-started-internet-arm-ps.md)

[Pregled virtualne mreže](../virtual-network/virtual-networks-overview.md)

[Rezervirane IP REST API-ji](https://msdn.microsoft.com/library/azure/dn722420.aspx)