<properties
   pageTitle="Početak rada prilikom stvaranja internetsku nasuprotne opterećenja uvođenje klasičnog modela pomoću za servise u oblaku | Microsoft Azure"
   description="Saznajte kako stvoriti internetsku nasuprotne opterećenja u modelu uvođenje klasičnog za servise u oblaku"
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
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Početak rada prilikom stvaranja internetsku nasuprotne opterećenja za servise u oblaku

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model klasični implementacije. Možete i [Saznajte kako stvoriti internetsku nasuprotne opterećenja pomoću upravitelja resursa Azure](load-balancer-get-started-internet-arm-cli.md).

Servisi u oblaku automatski konfigurirana s raspoređivača opterećenja i može se prilagoditi putem servisa modela.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Stvaranje raspoređivača opterećenja pomoću datoteke definicije servisa

Omogućuje korištenje Azure SDK za .NET 2,5 da biste ažurirali servis u oblaku. Postavke krajnje točke za servise u oblaku su izvršene u datoteci .csdef [definicije servisa](https://msdn.microsoft.com/library/azure/gg557553.aspx).

Sljedeći primjer pokazuje kako je konfigurirano servicedefinition.csdef datoteka za implementaciju oblaka:

Provjera snipet datoteke .csdef generira implementacije oblaka, možete vidjeti vanjski krajnjoj točki konfiguriran za korištenje priključke HTTP na priključak 10000, 10001 i 10002.


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




## <a name="check-load-balancer-health-status-for-cloud-services"></a>Provjera stanja za stanje raspoređivača opterećenja za servise u oblaku


Slijedi primjer stanja probni:

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

Raspoređivača opterećenja kombinira informacije na krajnjoj točki i informacije probni da biste stvorili URL-a u obliku http://{DIP VM}:80/Probe.aspx koje je moguće koristiti upit o stanju servisa.

Servis otkriva periodičku probes s istom IP adrese. Ovo je zahtjev za stanje probni koji dolaze iz glavnog čvor kojem se pokreće virtualnog računala.
Servis mora odgovor na poruke uz kod stanja HTTP 200 za opterećenja pretpostavlja se da je li servis dobar. Bilo koji drugi HTTP status kod (na primjer 503) izravno vodi virtualnog računala iz rotacije.

Definiciju probni kontrole i učestalost u probni. U slučaju naš iznad, raspoređivača opterećenja je probing krajnja točka svakih 5 sekunde. Ako je pozitivan odgovora primili za 10 sekundi (dva probni intervala), na probni pretpostavlja se da je prema dolje, a virtualnog računala koristi se iz rotacije. Isto tako, ako servis je izvan zakretanje, a primitku pozitivan odgovor, servis stavljaju se vratite na zakretanje odmah. Ako je servis je koje fluktuiraju između dobar i dobro, raspoređivača opterećenja možete odlučiti da biste odgodili ponovno Uvod servisa natrag na zakretanje dok je dobar za broj probes.

Potvrdite okvir definicija sheme servisa za [Stanje probni](https://msdn.microsoft.com/library/azure/jj151530.aspx) dodatne informacije.

## <a name="next-steps"></a>Daljnji koraci

[Početak rada konfiguriranje Interna opterećenja](load-balancer-get-started-ilb-arm-ps.md)

[Konfiguriranje načina raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)

