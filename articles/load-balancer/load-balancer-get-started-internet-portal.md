<properties
   pageTitle="Stvaranje opterećenja na mjesto na internetu u Voditelj resursa pomoću portala za Azure | Microsoft Azure"
   description="Saznajte kako stvoriti na mjesto na Internetu opterećenja u Voditelj resursa pomoću portala za Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="anavinahar"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="annahar" />

# <a name="creating-an-internet-facing-load-balancer-using-the-azure-portal"></a>Stvaranje programa opterećenja mjesto na Internetu pomoću portala za Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model implementacije Voditelj resursa. Možete i [Saznajte kako stvoriti na mjesto na Internetu raspoređivača opterećenja pomoću klasične implementacije](load-balancer-get-started-internet-classic-portal.md)

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

To obuhvaća slijed pojedine zadatke koje treba poduzeti da biste stvorili raspoređivača opterećenja i objašnjavaju detaljno što se Završi da biste izvršili cilj.

## <a name="what-is-required-to-create-an-internet-facing-load-balancer"></a>Što je potrebno da biste stvorili opterećenja na mjesto na Internetu?

Morate stvoriti i konfigurirati sljedeće objekte za implementaciju raspoređivača opterećenja.

- Pristupne Konfiguracija IP - sadrži javnu IP adresa za dolazne mrežni promet.

- Skupna pozadinsku adresa – sadrži sučelje mreže (NIC-ovi) za virtualnim strojevima mrežni promet primanje raspoređivača opterećenja.

- Opterećenja pravila - sadrži pravila mapiranja javno priključak na raspoređivača opterećenja priključak u pozadinskoj adresu.

- Ulazna pravila NAT - sadrži pravila mapiranja javno priključak na raspoređivača opterećenja priključak za određene virtualnog računala u pozadinskoj adresu.

- Probes – sadrži stanje probes koristi da biste provjerili dostupnost virtualnim strojevima instanci u pozadinskoj adresu.

Dodatne informacije o učitavanja možete doći opterećenja komponente s Azure Voditelj resursa pri [Azure Voditelj resursa podrške za raspoređivača opterećenja](load-balancer-arm.md).


## <a name="set-up-a-load-balancer-in-azure-portal"></a>Postavljanje opterećenja Azure portalu

> [AZURE.IMPORTANT] U ovom se primjeru pretpostavlja da ste virtualne mreže pod nazivom **myVNet**. Pogledajte [Stvaranje virtualne mreže](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) da biste to učinili. Također pretpostavlja postoji podmreže unutar **myVNet** naziva **LB-podmreže-se** i dva VMs pod nazivom **webu1** i **webu2** odnosno unutar istog dostupnosti skupa naziva **myAvailSet** u **myVNet**. Pogledajte [ovu vezu](../virtual-machines/virtual-machines-windows-hero-tutorial.md) da biste stvorili VMs.


1. U pregledniku otvorite centar za Azure: [http://portal.azure.com](http://portal.azure.com) i prijavite se pomoću računa za Azure.

2. Na gornjoj lijevoj strani zaslona odaberite **Novo** > **umrežavanje** > **opterećenja.**

3. U plohu **raspoređivača učitavanja za stvaranje** upišite naziv raspoređivača opterećenja. Ovo se zove **myLoadBalancer**.

4. U odjeljku **Vrsta**, odaberite **Javna**.

5. U odjeljku **javnu IP adresu**, stvorite novi javnu IP pod nazivom **myPublicIP**.

6. U odjeljku grupa resursa, odaberite **myRG**. Zatim odaberite odgovarajuće **mjesto**, a zatim kliknite **u redu**. Raspoređivača opterećenja će počnite za implementaciju i će potrajati nekoliko minuta uspješno implementacije.

![Ažuriranje grupu resursa opterećenja](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-a-back-end-address-pool"></a>Stvaranje na skupna pozadinsku adresa

1. Kada uspješno je postavila raspoređivača opterećenja, odaberite ga unutar resurse. U odjeljku postavke odaberite pozadinskog grupe. Upišite naziv za svoje pozadinskog grupe aplikacija. Zatim kliknite gumb **Dodaj** pri vrhu plohu koji se prikazuje.

2. Kliknite **Dodaj virtualnog računala** u plohu **grupe Dodaj pozadinska aplikacija** .  Odaberite **Odaberite skup dostupnosti** u odjeljku **Postavljanje dostupnosti** i **myAvailSet**. Nakon toga odaberite **Odaberite virtualnim strojevima** u odjeljku virtualnim strojevima u na plohu pa kliknite **webu1** i **webu2**, dva VMs stvorene za opterećenja. Provjerite je li imat plava kvačice s lijeve strane kao što je prikazano na slici u nastavku. Zatim **Odaberite** u tom plohu slijedi u redu u plohu **Odaberite virtualnog računala** , a zatim **u redu** u plohu **Dodavanje grupe pozadinska aplikacija** .

    ![Dodavanje pozadinske skupna adresa – ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Provjerite je li obavijesti na padajućem popisu je ažuriranje vezano uz spremanje skup za pozadinskog raspoređivača opterećenja uz ažuriranje mrežno sučelje za VMs **webu1** i **webu2**.


## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Stvaranje na probni, LB pravilo i NAT pravila

1. Stvaranje stanja probni.

    U odjeljku postavke raspoređivača opterećenja, odaberite Probes. Zatim kliknite **Dodaj** koja se nalazi pri vrhu na plohu.

    Dva su načina za konfiguriranje programa probni: HTTP-a ili TCP. U ovom se primjeru prikazuju HTTP, ali TCP moguće je konfigurirati na sličan način.
    Ažurirajte potrebne informacije. Kao što je rečeno, **myLoadBalancer** učitava saldo promet na priključak 80. Put odabrali HealthProbe.aspx, Interval je 15 sekundi i prag dobro je 2. Kada završite, kliknite **u redu** da biste stvorili na probni.

    Pokazivač postavite iznad 'i' ikonu da biste saznali više o ove pojedinačne konfiguracije i kako ih možete promijeniti da biste cater svojim potrebama.

    ![Dodavanje u probni](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Stvorite pravilo raspoređivača opterećenja.

    Kliknite pravila u odjeljku postavke raspoređivača opterećenja za ujednačavanje opterećenja. U novi plohu kliknite **Dodaj**. Naziv pravila. Ovdje je HTTP-a. Odaberite priključak sučelju i pozadinskog priključak. Ovdje je odabran 80 za oba. Odaberite **LB pozadinskog** kao vaše skup pozadinskog sustava i prethodno stvorena **HealthProbe** kao na probni. Ostale konfiguracije moguće je postaviti u skladu s potrebama. Zatim kliknite u redu da biste spremili pravilo za ujednačavanje opterećenja.

    ![Dodavanje u pravilo za ujednačavanje opterećenja](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Stvaranje ulazna pravila NAT

    Kliknite NAT ulazna pravila u odjeljku postavke raspoređivača opterećenja. U novi plohu, kliknite **Dodaj**. Zatim dajte naziv ulaznog NAT pravilo. Ovo se zove **inboundNATrule1**. Odredište mora biti javnu IP prethodno stvorili. Odaberite prilagođeni u odjeljku servis, a zatim odaberite protokol koji želite koristiti. Ovdje je odabran TCP. Unesite 3441, priključak i ciljne priključak, u ovom slučaju 3389. zatim kliknite u redu da biste spremili pravilo.

    Nakon stvaranja prvo pravilo, ponovite taj korak za drugo ulaznog NAT pravilo naziva inboundNATrule2 iz priključka 3442 cilj priključak 3389.

    ![Dodavanje ulaznog pravilo za NAT](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Uklanjanje raspoređivača opterećenja

Da biste izbrisali raspoređivača opterećenja, odaberite opterećenja koji želite ukloniti. U plohu *Opterećenja* kliknite **Izbriši** koja se nalazi pri vrhu na plohu. Zatim odaberite **da** kada se to od vas zatraži.

## <a name="next-steps"></a>Daljnji koraci

[Početak rada konfiguriranje Interna opterećenja](load-balancer-get-started-ilb-arm-cli.md)

[Konfiguriranje načina raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)
