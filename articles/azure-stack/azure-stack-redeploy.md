<properties
    pageTitle="Ponovno implementirate Azure stogu | Microsoft Azure"
    description="Ponovno implementirate Azure stogu."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="erikje"/>

# <a name="redeploy-azure-stack"></a>Ponovno implementirate Azure stogu

Implementirati stogu Azure, morate pokrenuti putem ispočetka prema uputama u nastavku.

## <a name="steps-to-redeploy-azure-stack"></a>Koraci za ponovno implementirate stogu Azure

1. Glavno računalo ponovno u izvorni operacijski sustav (instalirati Gola metal). To nije zadana postavka u izbornik za pokretanje pa morate koristiti KVM ili konzole za lokalni da biste odabrali tijekom na nakon ponovnog pokretanja (tijekom postavljanja pod nazivom je na "pokretanje iz VHD" OS na "AzureStack TP2", to će olakšati prepoznavanje koje OS je što).

    Ne morate ukloniti postojeće stavke (novi podršku skriptu "PrepareBootFromVHD.ps1" vodi brigu o koji vam.)

2. Ako imate KVM ili želite da biste odabrali OS pokretanje prije ponovnog pokretanja:
    
    1. Pronađite.\BootMenuNoKVM.ps1 skripte. Datoteka je dostupna s drugim podršku skripte dobiveni uz ovaj Sastavi.
    
    2. Pokrenite skriptu s dodatnim ovlastima. Odaberite naziv izvorne OS glavnog računala. To će se pokrenuti na računalo koje hostira u izvornom glavno računalo s operacijskim Sustavom bez KVM pristup.
    
    3. Nakon dovršetka skripte koje će se tražiti da biste potvrdili na ponovno pokrenite računalo.

    - Ako su drugi korisnici prijavljeni, ta se naredba neće uspjeti.

    - Samo pokrenite sljedeću naredbu: ponovno pokretanje računala – prisilno 
 
3. Izbrišite datoteku CloudBuilder.vhdx koji je korišten kao dio prethodne implementacije.

    Ne morate izbrisati postojeću grupu za pohranu iz prethodne TP2 implementacije. Implementacijsku skriptu otkriva i čisti postojeći, a zatim stvara novi.

5. Ponovno implementirate kopira novu kopiju CloudBuilder.vhdx, pokretanje, itd.

## <a name="next-steps"></a>Daljnji koraci

[Povezivanje s Azure stogu](azure-stack-connect-azure-stack.md)
