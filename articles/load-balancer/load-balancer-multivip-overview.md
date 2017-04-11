<properties
   pageTitle="Više VIPs za Azure učitati opterećenja | Microsoft Azure"
   description="Pregled više VIPs na Azure opterećenja"
   services="load-balancer"
   documentationCenter="na"
   authors="chkuhtz"
   manager="narayan"
   editor=""
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="chkuhtz"
/>

# <a name="multiple-vips-for-azure-load-balancer"></a>Više VIPs za Azure učitati opterećenja

Azure raspoređivača opterećenja omogućuje vam da biste učitali saldo usluge na više priključaka, više IP adresa ili i jedno i drugo. Definicije raspoređivača učitavanja javno i internih možete koristiti da biste učitali tokova saldo u skupu rezultata VMs.

U ovom se članku opisuju osnove ovu mogućnost, važnih koncepata i ograničenja. Ako samo želite izložiti usluge na jednu IP adresu, možete pronaći pojednostavnjeni upute za [javne](load-balancer-get-started-internet-portal.md) ni [interne](load-balancer-get-started-ilb-arm-portal.md) učitati opterećenja konfiguracije. Dodavanje većeg broja VIPs je rastuće jedan VIP konfiguracije. Korištenje konceptima u ovom članku, možete proširiti pojednostavljeni konfiguraciju u bilo kojem trenutku.

Prilikom definiranja programa Azure opterećenja u sučelju i konfiguraciji s pozadinskom su povezani s pravilima. Stanje probni referencira pravilo se koristi za određivanje dodavanje novih tokova bit će poslani čvor u pozadinskog. U sučelju definiran tako da na virtualne IP (VIP), što je 3-n-torke sastoji od IP adresa (javno ili interne), protokol za prijenos (UDP ili TCP) i broj priključka. U DIP je IP adresa na Azure virtualne NIC koji je pridružen VM u pozadinskog.

Sljedeća tablica sadrži nekoliko primjera sučelju konfiguracije:

| VIP | IP adresa | protokol | priključak |
|-----|------------|----------|------|
|1|65.52.0.1|TCP|80|
|2|65.52.0.1|TCP|_8080_|
|3|65.52.0.1|_UDP_|80|
|4|_65.52.0.2_|TCP|80|

U tablici prikazuje četiri različite frontends. Frontends #1, #2 i #3 su jedan VIP s više pravila. Koristi istu IP adresu, ali priključak i protokol razlikuje za svaki sučelju. Frontends #1 i #4 su primjera više VIPs gdje ponovo isti protokol sučelju i priključak upotrebljavati više VIPs.

Azure raspoređivača opterećenja pruža fleksibilnost prilikom definiranja pravila za ujednačavanje opterećenja. Pravilo deklarira adresa i priključaka u sučelju je mapiranje za odredišnu adresu i priključak na pozadinski. Hoće li se koristi pozadinskog priključaka na cijelom pravila ovisi o vrsti pravila. Svaka vrsta pravila ima preduvjete koji mogu utjecati na glavno računalo konfiguracija i probni dizajna. Postoje dvije vrste pravila:

1. Zadano pravilo s bez ponovnog korištenja skretnice pozadinskog priključak
2. Pravilo pluta IP gdje se ponovno iskoristiti pozadinskog priključci

Azure raspoređivača opterećenja možete kombinirati obje vrste pravila na istu konfiguraciju raspoređivača opterećenja. Raspoređivača opterećenja možete ih koristiti istodobno za dani VM ili bilo koju kombinaciju pod uvjetom da vam držati ograničenja pravilo. Koje vrste pravila odaberete ovisi o preduvjeti za aplikaciju i složenost tu konfiguraciju za podršku. Trebali biste procijeniti vrste pravilo koje su najbolje za vašu situaciju.

Ne možemo istražiti tih scenarija tako da započinju zadano ponašanje.

## <a name="rule-type-1-no-backend-port-reuse"></a>Pravilo vrsta #1: bez ponovnog korištenja skretnice pozadinskog priključak

![Ilustracija MultiVIP](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

U ovom scenariju VIPs sučelju konfigurirane na sljedeći način:

| VIP | IP adresa | protokol | priključak |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

U DIP je odredište ulaznog toka. U skup pozadinskog svaki VM izlaže željeni servis jedinstveni priključak na na DIP. Taj servis povezan je s sučelju putem definicija pravila.

Ne možemo definiranje dva pravila:

| Pravila | Mapiranje sučelju | Da biste grupe pozadinska aplikacija |
|------|--------------|-----------------|
| 1 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![pozadinskog](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![pozadinskog](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![pozadinskog](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![pozadinskog](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

Dovršavanje mapiranja u Azure opterećenja sada je na sljedeći način:

| Pravila | VIP IP adresa | protokol | priključak | Odredište | priključak |
|------|----------------|----------|------|-----|------|
|![pravila](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|DIP IP adresa|80|
|![pravila](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|DIP IP adresa|81|

Svako pravilo mora proizvesti tijek s jedinstvenim kombinacijom odredišni IP adresa i odredišni priključak. Po promjenjivih s odredišnim priključkom tijek više pravila možete isporučiti tokova iste DIP na različite priključke.

Stanje probes uvijek su usmjereni na DIP na VM. Vam mora biti sigurni da vaše probni odražava stanje u VM.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Pravilo vrsta #2: ponovnog korištenja pozadinskog priključak pomoću pluta IP

Azure opterećenja pruža fleksibilnost da biste ponovno koristili priključak sučelju preko više VIPs bez obzira na to pravilo vrsta koja se koristi. Uz to, neke scenarije aplikacije radije ili zahtijevaju isti priključak koriste više instanci aplikacije na jednom VM u pozadinskog. Uobičajeni primjeri ponovnog korištenja priključak uvrstili Klasteriranje za visoke dostupnosti, mreže virtualne aparata pa će se više TLS krajnje točke bez ponovnog šifriranja.

Ako želite da biste ponovno koristili priključak pozadinskog preko više pravila, morate omogućiti pluta IP u definiciji pravilo.

Plutajuća IP je dio što je poznato kao Izravni poslužitelj vratiti (DSC). DSC sastoji se od dva dijela: tijek Topologija i programa IP adresa za mapiranje shemu. U razini platforme Azure opterećenja uvijek funkcionira u tijek topologije DSC bez obzira na to je li omogućeno pluta IP ili ne. To znači da izlaznog dio u tijeku je uvijek ispravno potrebno ponovno napisati da teče izravno natrag izvor.

Uz zadanu vrstu pravila Azure izlaže na tradicionalni IP adresa mapiranje shemu radi jednostavnog korištenja za ujednačavanje opterećenja. Omogućivanje plutajući IP mijenja sheme mapiranje IP adresa da biste omogućili radi dodatne fleksibilnosti, kao što je opisano u nastavku.

Na sljedećem su dijagramu ilustrira tu konfiguraciju:

![Ilustracija MultiVIP](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

U ovom scenariju svaki VM u pozadinskog sadrži tri sučelje mreže:

* DIP: na virtualne NIC pridružene VM (Azure na NIC resurs)
* VIP1: povratna sučelje s unutar goste OS koji je konfiguriran s IP adresom VIP1
* VIP2: povratna sučelje s unutar goste OS koji je konfiguriran s IP adresom VIP2

>[AZURE.IMPORTANT] Konfiguriranje logičke sučelja izvesti u sklopu goste OS. Tu konfiguraciju je izvesti ili upravlja Azure. Bez tu konfiguraciju pravila neće funkcionirati. Stanje probni definicije pomoću DIP na VM umjesto logičke VIP. Stoga na servisu navedite probni odgovore na priključak DIP koje odražavaju status servisa koje se nude na logičke VIP.

Pogledajmo pretpostavlja ista konfiguracija sučelju kao u prethodnom scenariju:

| VIP | IP adresa | protokol | priključak |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

Ne možemo definiranje dva pravila:

| Pravila | Mapiranje sučelju | Da biste grupe pozadinska aplikacija |
|------|--------------|-----------------|
| 1 | ![pravila](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![pozadinskog](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (u VM1 i VM2) |
| 2 | ![pravila](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![pozadinskog](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (u VM1 i VM2) |

Sljedeća tablica prikazuje potpuni mapiranja u raspoređivača opterećenja:

| Pravila | VIP IP adresa | protokol | priključak | Odredište | priključak |
|------|----------------|----------|------|-------------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|Isto kao VIP (65.52.0.1)|Isto kao VIP (80)|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|Isto kao VIP (65.52.0.2)|Isto kao VIP (80)|

Odredište ulaznog tijek je adresa VIP sučelje povratna u na VM. Svako pravilo mora proizvesti tijek s jedinstvenim kombinacijom odredišni IP adresa i odredišni priključak. Po promjenjivih odredišnu IP adresu tijek ponovnog korištenja priključak je moguće na istom VM. Usluge raspoređivača opterećenja izložen po povezivanje na VIP IP adresu i priključak sučelja odgovarajući povratna.

U ovom se primjeru promijenili s odredišnim priključkom obavijest. Čak i ako je scenarij pluta IP, Azure opterećenja podržava i definiranje pravila dopune s odredišnim priključkom pozadinskog sustava i drukčije od odredišni priključak sučelju.

Vrste pravila pluta IP je foundation nekoliko obrazaca konfiguracije raspoređivača učitavanja. Primjer koje trenutno dostupno je konfiguracije [SQL AlwaysOn s više slušače](../virtual-machines/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) . S vremenom će opisujemo od tih scenarija.

## <a name="limitations"></a>Ograničenja

* Konfiguracija više VIP podržani su samo uz IaaS VMs.
* Uz pravilo pluta IP aplikacije morate koristiti u DIP za izlazni tokova. Ako aplikacija povezuje VIP adresu konfigurirali povratna sučelja u goste OS, zatim SNAT nije dostupan za dopune izlaznog tijek i ne uspije tijeka.
* Javnu IP adrese funkcioniralo na naplata. Dodatne informacije potražite u članku [cijene IP adresa](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Pretplata ograničenja primjenjuju. Dodatne informacije potražite u članku [ograničenja servisa](../azure-subscription-service-limits.md#networking-limits) detalje.
