<properties
    pageTitle="Smjernice za grupe resursa | Microsoft Azure"
    description="Saznajte više o ključa dizajna i implementaciju smjernice za implementaciju grupa resursa u servisa Azure infrastrukture."
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

# <a name="azure-resource-group-guidelines"></a>Smjernice za grupu Azure resursa

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

U ovom članku fokus je na objašnjenje kako logično izgradnje okruženju sustava i grupiranje svih komponenti u grupama resursa.


## <a name="implementation-guidelines-for-resource-groups"></a>Smjernice za implementaciju za grupe resursa

Odluka:

- Se premještaju izgradnje grupa resursa po komponentama infrastrukture core ili implementacija dovršeno aplikacije?
- Morate li ograničiti pristup grupama resursa pomoću pristup kontrolama za na temelju uloga?

Zadaci:

- Definiranje što Jezgra infrastrukture komponente i namjenski morate grupa resursa.
- Pogledajte kako implementirati predloške resursima za dosljedan, tiskarskom implementacije.
- Definiranje koje uloge korisnika pristup morate za kontrolu pristupa grupama resursa.
- Stvaranje skupa pomoću svoje konvencija imenovanja grupa resursa. Možete koristiti Azure EŽA ili portal.


## <a name="resource-groups"></a>Grupa resursa

U Azure, logično grupirali povezane resurse kao što su računi za pohranu, virtualne mreže i virtualnim strojevima (VMs) implementacije, upravljanje i održavanje ih kao jedan entitet. Taj se način olakšava za implementaciju aplikacije zadržavanje povezani resursi zajedno iz perspektive upravljanje ili drugim korisnicima dopustiti pristup tom grupom resursa. Detaljnije Upoznavanje grupa resursa možete pročitati [Pregled Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md).

Ključne značajke grupe resursa je mogućnost stvaranja okruženja korištenje JSON datoteka deklarira prostora za pohranu, povezivanje s mrežom, i resursi za izračun. Možete definirati i sve povezane prilagođene skripte ili konfiguracije da biste primijenili. Pomoću ove predložaka JSON stvorite dosljedan, tiskarskom implementacijama za aplikacije. Taj se način omogućuje sastavljanje okruženju u razvoju, a zatim pomoću tog isti predložak da biste stvorili uvođenja radni ili obrnuto. Za bolje razumjeli o korištenju predložaka, pročitajte [Vodič za predložak](../resource-manager-template-walkthrough.md) koji će vas voditi kroz svaki korak zgrade izgleda predloška JSON.

Postoje dvije različite pristupi prilikom dizajniranja vaše okruženje s grupama resursa:

- Grupa resursa za svaki implementaciju aplikacije koje kombinira račune za pohranu, virtualne mreže i podmreže, VMs, učitavanje balancers, itd.
- Središnje grupa resursa koje sadrže core virtualne mreže i podmreže ili spremanje računa. Aplikacija se zatim vlastite grupa resursa koji sadrži samo VMs balancers opterećenja, sučelje mreže i Dr.

Kao što ste mjerilo, stvaranje središnje grupa resursa za virtualne mreže i podmreže olakšava stvaranje više lokacija mrežne veze za hibridno povezivanje mogućnosti. Zamjenski pristup je za svaku aplikaciju da bi vlastite virtualne mreže koji zahtijeva konfiguriranje i održavanje. [Pristup kontrolama za na temelju uloga](../active-directory/role-based-access-control-what-is.md) omogućuje zrnastog za kontrolu pristupa grupama resursa. Za aplikacije radnog možete kontrolirati korisnici koji mogu pristupati tih resursa ili za resurse infrastrukture core možete ograničiti samo infrastrukture inženjeri za rad s njima. Vaše aplikacije samo vlasnici pristup aplikacije komponente unutar svoje grupe resursa i nisu u skladu s osnovnim Azure infrastrukture okruženju sustava. Dizajnirate okruženja, razmislite o korisnicima koji zahtijevaju pristup resursima i sukladno tome dizajnirati svoje grupe resursa. 


## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 