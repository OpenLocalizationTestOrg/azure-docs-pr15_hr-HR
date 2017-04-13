<properties
    pageTitle="Smjernice za virtualnim strojevima sa sustavom Windows | Microsoft Azure"
    description="Dodatne informacije o ključa dizajna i implementaciju smjernice za implementaciju virtualnim strojevima sa sustavom windows u Azure"
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="virtual-machines-guidelines"></a>Smjernice za virtualnim strojevima

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

U ovom članku fokus je na objašnjenje potrebne korake za planiranje za stvaranje i upravljanje virtualnim strojevima (VMs) u okruženju sustava Azure.

## <a name="implementation-guidelines-for-vms"></a>Smjernice za implementaciju za VMs
Odluka:

- Koliko VMs učinite potreban za različite razine aplikacije i komponente sustava infrastrukture?
- Opterećenje procesora i memorije resursima svaki VM mora i preduvjeti za pohranu?

Zadaci:

- Definiranje radnih opterećenja aplikacija i resursa u VMs obavezan.
- Poravnanje resursa zahtjeve za svaki VM vrste odgovarajuće VM veličina i prostora za pohranu.
- Definiranje grupa za resursa za različite razine i komponente sustava infrastrukture.
- Definiranje konvencija imenovanja VM.
- Stvaranje vaše VMs pomoću komponente PowerShell Azure, portala, ili s predlošcima Voditelj resursa.

## <a name="virtual-machines"></a>Virtualnim strojevima

Jedna od glavne komponente u okruženju sustava Azure je vjerojatno VMs. To je mjesto pokrenete vaše aplikacije, baze podataka, servise za provjeru autentičnosti, itd.

Važno da biste shvatili [različitim veličinama VM](virtual-machines-windows-sizes.md) pravilno promijenite veličinu vaše okruženje iz performanse i perspektive trošak je. Ako vaš VMs nije dovoljno jezgri procesora i memorije, bez obzira na to koliko će se dobro je dizajniran i razvio suffers performanse aplikacije. Pregled predloženih radnih opterećenja za svaki niz VM kao početnu točku kao odlučite veličina VM za svaku od njih u sustavu. Možete [promijeniti veličinu na VM](https://azure.microsoft.com/blog/resize-virtual-machines/) nakon implementacije.

Prostor za pohranu reproducira ulogu VM performanse. Koristite standardne prostor za pohranu, koji koristi diskova običnog adrese e ili Premium prostora za pohranu visoke/i radnih opterećenja i Vršna načina rada, koji koristi SSD diskova. Kao s veličinom VM postoji su troška pitanja koja se odnose na izbor medij za pohranu. [Prostor za pohranu infrastrukture smjernice članak](virtual-machines-windows-infrastructure-storage-solutions-guidelines.md) da biste razumjeli kako dizajnirati odgovarajuće prostora za pohranu za optimalne performanse sustava VMs može čitati.


## <a name="resource-groups"></a>Grupa resursa
Komponente kao što su VMs su logički grupirane radi lakšeg upravljanja i održavanje pomoću [Grupa resursa Azure](../azure-resource-manager/resource-group-overview.md). Pomoću grupa resursa možete stvoriti, upravljanje i nadzirati resursi koji čine navedene aplikacije. Možete implementirati i [Kontrola pristupa na temelju uloga](../active-directory/role-based-access-control-what-is.md) dopustiti pristup drugim korisnicima vašeg tima samo resursima trebaju. Potrebno vrijeme za planiranje resursa grupe i dodjele uloga. Postoje drugi pristupi zapravo dizajniranje i implementacija grupa resursa, stoga svakako pročitajte u [članku smjernice za resursa grupe](virtual-machines-windows-infrastructure-resource-groups-guidelines.md) da biste razumjeli kako najbolje izgradnje vaše VMs.


## <a name="templates"></a>Predlošci 
Možete izrađivati predlošci definira deklarativno datoteke JSON, da biste stvorili vaše VMs. Predlošci obično izgraditi i na potreban prostor za pohranu mreže, sučelje mreže, adresiranja IP, itd uz VMs sami. Koristite predloške da biste stvorili dosljedan, tiskarskom okruženja za razvoj i testiranje potrebe za jednostavno replikaciju radnog okruženja i obrnuto. Dodatne informacije o [Stvaranje i korištenje predložaka](../azure-resource-manager/resource-group-overview.md#template-deployment) da biste razumjeli kako ih možete koristiti za stvaranje i implementacija sustava VMs.


## <a name="next-steps"></a>Daljnji koraci
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 