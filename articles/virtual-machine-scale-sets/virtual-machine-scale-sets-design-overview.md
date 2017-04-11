<properties
    pageTitle="Dizajniranje virtualnog računala skaliranje postavlja skaliranja | Microsoft Azure"
    description="Saznajte kako dizajnirati svoje skupove skaliranje virtualnog računala skaliranja"
    keywords="Postavlja virtualnog računala skaliranje Linux virtualnog računala" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="gatneil"/>

# <a name="designing-vm-scale-sets-for-scale"></a>Dizajniranje VM skaliranje postavlja skaliranja

U ovoj se temi opisuje zahtjevi dizajna za skupove skaliranje virtualnog računala. Informacije o koji su skupovi skaliranje virtualnog računala, potražite [Virtualnog računala skaliranje skupove pregled](virtual-machine-scale-sets-overview.md).


## <a name="storage"></a>Prostor za pohranu

Skup skaliranje koristi račune za pohranu za pohranu diskova OS od na VMs u skupu. Preporučujemo da se omjer 20 VMs po računu za pohranu ili manje. Preporučujemo i da vam dodijeliti većem abecede znakova početak nazive računa za pohranu. To pomaže podjele učitavanja preko različitih unutarnje sustave. Ako, primjerice, u predlošku sljedeće koristimo uniqueString funkcije resursima predložak da biste generirali raspršivanja prefiks koji su dodati imena pohranu računa: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## <a name="overprovisioning"></a>Overprovisioning

Počevši od verzije API-JA "2016 – 03-30", skupovi skaliranje VM po zadanom odabrana "overprovisioning" VMs. S overprovisioning uključena, skale postavljanje zapravo okreta gore više VMs od vas tražiti, a zatim briše dodatni VMs koji spun gore posljednje. Overprovisioning poboljšava dodjele resursa postocima uspjeha. Vam se naplatiti za te dodatne VMs, a ne prema vaša ograničenja broji.

Overprovisioning poboljšati dodjele resursa postocima uspjeha, mogu uzrokovati zbunjujuće ponašanje za aplikaciju koja nije namijenjena rukovati VMs nestaje za vrijeme unannounced. Da biste uključili overprovisioning isključivanje, provjerite je li u predlošku imate sljedeći niz: "overprovision": "false". Dodatne informacije pronaći ćete u [dokumentaciji VM skaliranje postavljanje REST API -JA](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Ako isključite overprovisioning odmah možete doći s većim omjer VMs po računu za pohranu, ali ne preporučujemo prelaska iznad 40.


## <a name="limits"></a>Ograničenja
Skup skaliranje utemeljena na prilagođenu sliku (jedne ugrađeni vi) morate stvoriti sve VHDs disk OS unutar jednog računa za pohranu. Zbog toga najviše preporučeni broj VMs u skupu skaliranje utemeljena na prilagođenu sliku iznosi 20. Ako isključite overprovisioning možete prijeći do 40.

Skup skaliranje utemeljena na platformi slika trenutno ograničeno je na 100 VMs (Preporučujemo 5 račune za pohranu za ovu skalu).

Za dodatne VMs od ta ograničenja omogućili, morate uvesti više skupova mjerilo kao što je prikazano u [ovom predlošku](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).