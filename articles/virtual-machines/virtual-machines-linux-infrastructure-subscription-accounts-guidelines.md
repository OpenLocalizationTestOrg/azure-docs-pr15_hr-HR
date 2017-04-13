<properties
    pageTitle="Pretplate i smjernice za račune | Microsoft Azure"
    description="Saznajte više o ključa dizajna i implementaciju smjernice za pretplate i račune na Azure."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="subscription-and-accounts-guidelines"></a>Smjernice za pretplatu i računa

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

U ovom se članku usredotočuje se na objašnjenje kako postići pretplate i račun upravljanje kao vaše okruženje i korisnika, osnovni poveća.


## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Smjernice za implementaciju za račune i pretplate

Odluka:

- O skupu pretplate, a računa ne morate za hostiranje radno opterećenje IT ili infrastrukture?
- Kako prekinuti hijerarhiju tako da stane u vašoj tvrtki ili ustanovi?

Zadaci:

- Definirajte hijerarhije logičke tvrtke ili ustanove kako biste željeli upravljanje iz pretplate razine.
- Da biste potražili logička hijerarhija, definirajte pretplate u odjeljku svaki račun i računi koje su potrebne.
- Stvaranje skupa pretplate i poslovnim kontaktima pomoću svoje konvencija imenovanja.


## <a name="subscriptions-and-accounts"></a>Pretplate i računa

Da biste radili s Azure, potrebno vam je jedan ili više Azure pretplate. Resursi sviđa virtualnim strojevima (VMs) ili virtualne mreže postoji u tim pretplata.

- Tvrtke obično imaju registraciju Enterprise koji je vrha do krajnjeg resursa u hijerarhiji, a povezan jedan ili više računa.
- Korisnici i korisnici bez programa registraciju Enterprise, resursa vrha do krajnjeg je račun.
- Pretplate su povezani računi, a može biti jedan ili više pretplata po računu. Azure zapisa podataka o razini pretplate za naplatu.

Zbog ograničenje od dviju razina hijerarhije na račun/pretplate odnosa je važno da biste poravnali konvencija imenovanja računa i pretplate naplate potrebama. Na primjer, ako globalni tvrtka koristi Azure, oni mogu odabrati jedan račun po regijama i imaju upravlja pretplatama na razini područja:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Na primjer, možda koristi sljedeću strukturu:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Ako područje odluči imate više pretplata pridruženi određenoj grupi, konvencije imenovanja treba uključivanje način kodiranje dodatnih podataka na račun ili naziv pretplate. U ovom tvrtka ili ustanova dopušta massaging naplate podataka da biste generirali nove razine hijerarhije tijekom naplate izvješća:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Tvrtke ili ustanove može izgledati ovako:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Dajemo detaljne naplatu putem koje je moguće preuzeti datoteku za jedan račun ili za sve račune u ugovor enterprise.


## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 