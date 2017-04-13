<properties
    pageTitle="Stvaranje više virtualnim strojevima | Microsoft Azure"
    description="Mogućnosti za stvaranje više virtualnim računalima sustava Windows"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="guybo"/>

# <a name="create-multiple-azure-virtual-machines"></a>Stvaranje više Azure virtualnim strojevima

Postoji mnogo scenarija koji se tamo gdje vam je potreban da biste stvorili velikog broja slične virtualnim strojevima (VMs). Primjeri uključuju računalno visokih performansi (HPC), analiza velikih podataka, skalabilni i često bez praćenja stanja srednjem sloju ili pozadinskog poslužitelja (kao što je webservers), i raspodijeljeno baze podataka.

U ovom se članku govori o dostupnim mogućnostima za stvaranje više VMs Azure. Ove mogućnosti okružje jednostavne slučajevima gdje ručno stvaranje niza VMs. Da biste stvorili mnogo VMs, procesa koji se obično koristi ne skaliranja i ako je potrebno stvoriti više handful od VMs.

Jedan da biste stvorili mnogo slične VMs tako da koriste konstrukta Voditelj resursa Azure od _petlje resursa_.

## <a name="resource-loops"></a>Resurs petlje

Resurs petlje su syntactical prečac u predlošcima Azure Voditelj resursa. Petlje resursa možete stvoriti skup resursa na sličan način konfiguriran u petlji. Da biste stvorili više prostora za pohranu poslovne kontakte, sučelje mreže ili virtualnim računalima sustava možete koristiti petlji resursa. Dodatne informacije o resursu petlje priručniku [Stvaranje VMs dostupnosti postavlja pomoću petlje resursa](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>Izazove skala

Iako resursa petlji lakše izgradnje infrastrukture u oblaku na razini i dobivanje Sažetija predlošci, određene izazove ostaju. Ako, na primjer, ako koristite petlje resursa da biste stvorili 100 virtualnim strojevima, ćete morati povezivanje kontrolera mreže sučelja (NIC-ovi) s odgovarajuće VMs i račune za pohranu. Budući da je broj VMs vjerojatno će se razlikovati od broj računa za pohranu, morat ćete baviti veličine petlja različite resursa, previše. To su solvable probleme, ali složenosti znatno povećava s ljestvicom.

Neki drugi problem se pojavljuje kada je potrebno infrastruktura za koju se mijenja veličinu elastically. Na primjer, možda ćete infrastruktura za automatsko skaliranje koju automatski se povećava ili smanjuje broj VMs kao odgovor na radno opterećenje. VMs ne navedite integriranom mehanizam za razlikovanje broj (skaliranje izgleda i promjena veličine u). Ako promijenite u uklanjanjem VMs teško je jamči visoke dostupnosti po pazeći da se VMs raspoređen na ažuriranja i kvara domene.

Na kraju, kada koristite petlje resursa, više poziva da biste stvorili resursa idite na pozadini tkanina. Kada više pozive stvorili sličan resursa, Azure ima implicitno prilike da biste poboljšali nakon ovaj dizajn i optimizirati implementacije pouzdanost i performanse. Ovo je _virtualnog računala skaliranje postavlja_ odakle.

## <a name="virtual-machine-scale-sets"></a>Skupovi skaliranje virtualnog računala

Skupovi skaliranje virtualnog računala su resursa Azure izračunati implementacije i upravljanja skup identične VMs. Sa sve VMs konfiguriran mjerila VM istovremeno, skupovi su lako promijenite veličinu u i Vremensko mjerilo. Jednostavno promijeniti broj VMs u skupu. Možete konfigurirati i skupove skaliranje VM automatsko skaliranje na temelju zahtjeva povećavaju.

Za aplikacije koje su potrebne za promjenu veličine računalnim resursa izgleda i u skaliranje operacije su implicitno raspoređen po kvara i ažuriranje domene.

Umjesto correlating više resurse kao što su NIC-ovi i VMs skupu skaliranje VM ima mreže, prostor za pohranu, virtualnog računala i nastavak svojstava koja možete konfigurirati središnje.

Uvod u skupove skaliranje VM, potražite [virtualnog računala skaliranje postavlja proizvoda stranice](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Detaljnije informacije potražite na [virtualnim strojevima skaliranje postavlja dokumentacije](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).
