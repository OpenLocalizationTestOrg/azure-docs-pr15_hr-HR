<properties
   pageTitle="Početak rada prilikom stvaranja interne opterećenja u Voditelj resursa pomoću portala za Azure | Microsoft Azure"
   description="Saznajte kako stvoriti Interna opterećenja u Voditelj resursa pomoću portala za Azure"
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
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a>Stvaranje internog opterećenja na portalu za Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][uvođenje klasičnog modela](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Početak rada prilikom stvaranja za interne raspoređivača opterećenja pomoću portala za Azure

Poduzmite sljedeće korake da biste stvorili Interna opterećenja na portalu Azure.

1. Otvorite preglednik, idite na [portal za Azure](http://portal.azure.com)i prijavite se pomoću računa za Azure.
2. U gornjoj lijevoj strani strani zaslona kliknite **Novo** > **umrežavanje** > **opterećenja**.
3. U plohu **Stvaranje učitati opterećenja** unesite **naziv** za raspoređivača opterećenja.
4. U odjeljku **shemu**, kliknite **internog**.
5. Kliknite **virtualne mreža**, a zatim odaberite virtualne mreže koju želite stvoriti raspoređivača opterećenja.

    >[AZURE.NOTE] Ako ne vidite virtualne mreže koju želite koristiti, provjerite **mjesto** koje koristite za raspoređivača opterećenja i sukladno tome promijeniti.

6. Kliknite **podmreže**, a zatim odaberite podmreže mjesto na koje želite stvoriti raspoređivača opterećenja.
7. U odjeljku **Dodjeljivanje IP adresa**kliknite **dinamičke** ili **statičke**, ovisno o tome želite li IP adresa za raspoređivača opterećenja ispraviti (statičnu) ili ne.

    >[AZURE.NOTE] Ako odaberete da biste koristili statičke IP adrese, morat ćete unijeti adresu za raspoređivača opterećenja.

8. U odjeljku **grupa resursa** ili navedite naziv nove grupe resursa za raspoređivača opterećenja ili kliknite **Odaberite postojeći** pa odaberite postojeću grupu resursa.
9. Kliknite **Stvori**.

## <a name="configure-load-balancing-rules"></a>Konfiguriranje pravila za ujednačavanje opterećenja

Nakon stvaranja raspoređivača opterećenja, pomaknite se do resursa raspoređivača opterećenja da to učini.
Morate konfigurirati najprije na skupna pozadinsku adresa i na probni prije konfiguriranja u pravilo za ujednačavanje opterećenja.

### <a name="step-1-configure-a-back-end-pool"></a>Korak 1: Konfiguriranje pozadinske skup

1. Na portalu Azure kliknite **Pregledaj** > **Učitavanje balancers**, a zatim kliknite opterećenja koji ste stvorili iznad.
2. U plohu **Postavke** kliknite **pozadinskog grupe**.
3. U plohu **pozadinskog adresu grupe** , kliknite **Dodaj**.
4. U plohu **Dodaj pozadinskog skup** unesite **naziv** za grupu pozadinskog, a zatim **u redu**.

### <a name="step-2-configure-a-probe"></a>Korak 2: Konfiguriranje programa probni

1. Na portalu Azure kliknite **Pregledaj** > **Učitavanje balancers**, a zatim kliknite opterećenja koji ste stvorili iznad.
2. U plohu **Postavke** kliknite **Probes**.
3. U plohu **Probes** kliknite **Dodaj**.
4. U plohu **Dodaj probni** unesite **naziv** za na probni.
5. U odjeljku **protokol**odaberite **HTTP** (za web-mjesta) ili **TCP** (za druge aplikacije TCP temelji).
6. U odjeljku **Port (priključak)**, navedite priključak za korištenje prilikom pristupanja na probni.
7. U odjeljku **puta** (za HTTP probes samo) navedite put da biste upotrijebili na probni.
8. U odjeljku **razmak** odredite koliko često želite isprobati aplikacije.
9. U odjeljku **dobro praga**odredite koliko pokušaja treba uspjeti prije virtualnog računala pozadinskog označen kao dobro.
10. Kliknite **u redu** da biste stvorili probni.

### <a name="step-3-configure-load-balancing-rules"></a>Korak 3: Konfiguriranje pravila za ujednačavanje opterećenja

1. Na portalu Azure kliknite **Pregledaj** > **Učitavanje balancers**, a zatim kliknite opterećenja koji ste stvorili iznad.
2. U plohu **Postavke** kliknite **opterećenja pravila**.
3. U plohu **opterećenja pravila** kliknite **Dodaj**.
4. U plohu **Dodaj učitavanje ujednačavanje pravila** unesite **naziv** pravila.
5. U odjeljku **protokol**odaberite **HTTP** (za web-mjesta) ili **TCP** (za druge aplikacije TCP temelji).
6. U odjeljku **Port (priključak)**, navedite raspoređivača opterećenja povezivanje klijenti za priključak.
7. U odjeljku **Pozadinski priključak**, navedite priključak koja će se koristiti u pozadinskom (obično priključak raspoređivača opterećenja i priključka pozadinskog jednake).
8. U odjeljku **grupe pozadinska aplikacija**, odaberite skup pozadinskog koju ste stvorili iznad.
9. U odjeljku **postojanost sesiju**, odaberite kako želite da se sesije održati.
10. U odjeljku **neaktivnosti Prekoračenje vremena (u minutama)**, navedite neaktivnosti vremenskog ograničenja.
11. U odjeljku **Pluta IP (Izravni server povrata)**, kliknite **onemogućene** ili **omogućeno**.
12. Kliknite **u redu**.

## <a name="next-steps"></a>Daljnji koraci

[Konfiguriranje načina raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)