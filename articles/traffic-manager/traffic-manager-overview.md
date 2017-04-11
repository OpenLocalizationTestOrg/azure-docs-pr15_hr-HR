<properties
    pageTitle="Što je upravitelj promet | Microsoft Azure"
    description="U ovom se članku pronaći ćete razumjeti što je upravitelj promet i tome je li desnom promet za usmjeravanje odabir aplikacije"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-traffic-manager"></a>Pregled upravitelja promet

Upravitelj promet za Microsoft Azure omogućuje kontrolu raspodjele korisnika promet za krajnje točke servisa u različitim podatkovnim centrima. Krajnje točke servisa podržane Upravitelj promet obuhvaćaju Azure VMs, web-aplikacijama i servisima u oblaku. Promet Manager možete koristiti i s vanjskim, koji nisu Azure krajnje točke.

Upravitelj promet koristi sustava naziva domena (DNS) za usmjeravanje zahtjevi klijenta najprikladnije krajnjoj na temelju [metodu promet usmjeravanja](traffic-manager-routing-methods.md) i stanje krajnjih točaka. Upravitelj promet nudi raspon promet usmjeravanje načina kako bi odgovarao potrebama drugu aplikaciju, krajnjoj točki stanja [za nadzor](traffic-manager-monitoring.md)i automatsko prebacivanje. Voditelj promet je prebacuju kvara, uključujući neuspjeh cijelo Azure područje.

## <a name="traffic-manager-benefits"></a>Prednosti Upravitelj promet

Promet Manager pomoći će vam:

- **Poboljšanje dostupnost ključnih aplikacija**

    Upravitelj promet nudi visoke dostupnosti za aplikacije na krajnje točke za nadzor i omogućuje automatsko prebacivanje kada krajnje funkcionira.

- **Poboljšali odziv za aplikacije visokih performansi**

    Azure omogućuje vam pokretanje servisa u oblaku ili web-mjesta u podatkovnim centrima nalazi diljem svijeta. Upravitelj promet poboljšava odziv aplikacije preusmjerivanjem promet krajnjoj s najmanje latenciju mreže za klijentski program.

- **Održavanje servisa bez nedostupnost**

    Možete izvoditi operacije planiranog održavanja na aplikacija bez isključiti. Upravitelj promet usmjerava promet na drugu krajnje točke dok održavanje je u tijeku.

- **Kombiniranje lokalnog poslužitelja i aplikacije utemeljene na Oblaku**

    Upravitelj promet podržava vanjski, krajnje točke Azure ga za uporabu hibridnog omogućite u oblaku i lokalnim implementacijama, uključujući "neprekidnog rada-na-oblaka," "migrirati-na-oblaka," i "prebacivanje u oblak" scenarija.

- **Distribucija promet za veliki, složene implementacije**

    Korištenje [ugniježđenih promet upravitelj profila](traffic-manager-nested-profiles.md), promet usmjeravanje metode možete kombinirati da biste stvorili složene i prilagodljivo pravila za podršku potrebama veće, složenije implementacije.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o [funkcioniranje Upravitelj promet](traffic-manager-how-traffic-manager-works.md).

- Saznajte kako razvijaju visoku dostupnost aplikacije pomoću [upravitelja promet krajnjoj točki nadzor](traffic-manager-monitoring.md).

- Dodatne informacije o [promet usmjeravanje metode](traffic-manager-routing-methods.md) podržane Upravitelj promet.

- [Stvaranje profila Upravitelja promet](traffic-manager-manage-profiles.md).

