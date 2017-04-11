<properties
   pageTitle="Stvaranje internog opterećenja za servise u oblaku u modelu uvođenje klasičnog | Microsoft Azure"
   description="Saznajte kako stvoriti na interni opterećenja pomoću komponente PowerShell u modelu klasični implementacije"
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
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Početak rada prilikom stvaranja interne opterećenja (klasični) za servise u oblaku

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

<BR>

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvesti sljedeće korake pomoću modela Voditelj resursa](load-balancer-get-started-ilb-arm-ps.md).


## <a name="configure-internal-load-balancer-for-cloud-services"></a>Konfiguriranje Interna opterećenja za servise u oblaku

Interna opterećenja nije podržana za virtualnim strojevima i servise u oblaku. Krajnje opterećenja Interna učitavanja stvorene u oblaku koji se nalazi izvan regionalne virtualne mreže bit će dostupne samo unutar servisa u oblaku.

Konfiguracije raspoređivača Interna učitavanja mora biti postavljena prilikom stvaranja prvog implementacije u oblaku, kao što je prikazano u primjeru u nastavku.

>[AZURE.IMPORTANT] Preduvjeta za pokretanje korake u nastavku je imati virtualne mreže na već stvorene za implementaciju oblaka. Trebat će se ime virtualne mreže i podmreže naziv da biste stvorili na interni opterećenja.

### <a name="step-1"></a>Korak 1

Otvorite datoteku konfiguracije servisa (.cscfg) za implementaciju sustava oblaka u Visual Studio i dodajte sljedeće sekcije da biste stvorili na interni opterećenja ispod posljednjeg "`</Role>`" stavke za konfiguraciju mreže.




    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="name of the load balancer">
          <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>


Dodat ćemo vrijednosti za mrežu konfiguracijska datoteka da biste prikazali kako će izgledati. U ovom primjeru pretpostavlja da ste stvorili podmreže pod nazivom "test_vnet" s podmreže 10.0.0.0/24 pod nazivom test_subnet i statičke IP 10.0.0.4. Raspoređivača opterećenja imenovanja testLB.

    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="testLB">
          <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>

Dodatne informacije o shemi raspoređivača opterećenja potražite u članku [Dodavanje raspoređivača učitavanja](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Korak 2


Promijenite datoteku definicije (.csdef) servisa da biste dodali na interni opterećenja krajnje točke. Trenutak stvaranja uloga instance datoteku definicije servisa će dodati instance uloga na interni opterećenja.


    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
      </Endpoints>
    </WorkerRole>

Slijedeći iste vrijednosti iz gornji primjer dodat ćemo vrijednosti da biste datoteku definicije servisa.

    <WorkerRole name=WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
      </Endpoints>
    </WorkerRole>

Mrežni promet bit će rasporediti pomoću testLB opterećenja koristi priključak 80 za dolazni zahtjevi za slanje tempiranja uloga instanci i priključak 80 opterećenje.


## <a name="next-steps"></a>Daljnji koraci

[Konfiguriranje načina distribucije raspoređivača učitavanja pomoću afiniteta izvorni IP](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)