
<properties
    pageTitle="Azure dosljedan prostora za pohranu: razlike i napomene | Microsoft Azure"
    description="Razumijevanje razlike od Azure prostora za pohranu i druge Azure dosljedan okolnosti uvođenja prostora za pohranu."
    services="azure-stack"
    documentationCenter=""
    authors="MChadalapaka"
    manager="siroy"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="mchad"/>

# <a name="azure-consistent-storage-differences-and-considerations"></a>Azure dosljedan prostora za pohranu: razlike i napomene

Azure dosljedan prostora za pohranu je skup servise u oblaku za pohranu u stogu Microsoft Azure. Azure dosljedan prostora za pohranu nudi blob, tablice, reda čekanja i funkcija upravljanja račun s Azure dosljedan semantiku. U ovom se članku nalaze se poznati Azure dosljedan prostora za pohranu razlike s Azure prostora za pohranu. Zbroji i ostala razmatranja treba imati na umu prilikom implementacije trenutno javno dostupna pretpregled izdanja sustava Microsoft Azure stogu.

<span id="Concepts" class="anchor"><span id="_Toc386544169" class="anchor"><span id="_Toc389466742" class="anchor"><span id="_Ref428966996" class="anchor"><span id="_Toc433223853" class="anchor"></span></span></span></span></span>
## <a name="known-differences"></a>Poznati razlike

Ova verzija Tehnički pretpregled Azure dosljedan prostora za pohranu nema 100% značajka slične značajke s Azure prostora za pohranu za API verzije koje su podržane. Poznati razlike u značajkama obuhvaćaju sljedeće:

-   Za određene vrste računa za pohranu još nisu dostupni, primjerice, standardno\_RAGRS i standardnu\_GRS.

-   Premium\_LRS prostora za pohranu računa možete dodjeli. No postoje trenutno nema učinka ograničenja ili jamstva.

-   Azure datoteke funkcija još nije dostupna.

-   API za rasponi početak stranice ne podržava Dohvaćanje stranice koje se razlikuju snimke blob-Ova stranica.

-   API za rasponi se stranica prikazuje stranice koje imaju 4 KB preciznosti.

-   Ključ particija i ključ retka u dosljedan Azure implementaciji tablice za pohranu su svaki ograničeni na 400 znakova (to jest, 800 bajtova).

-   Nazivi blob u dosljedan Azure implementaciji servisa za pohranu Blob ograničeni su na 880 znakova (to jest, 1760 bajtova).

-   Azure dosljedan prostora za pohranu implementacije klijenta za pohranu podataka o korištenju izvješćivanja nudi metar korištenje prostora za pohranu koji su jednaki onima u Azure, s istim semantiku i jedinice. Trenutno Međutim, metar korištenje prostora za pohranu transakcije obuhvaćaju IaaS transakcije i korištenje metar prijenosa podataka razlikovati korištenja propusnosti po interni ili vanjski mrežni promet programa Azure stogu područje.

-   Postoje neke razlike u opseg funkcionalnosti za mogućnost upravljanja prostora za pohranu. Ako, na primjer, ne možete promijeniti vrstu računa ili su prilagođene domene. Osim toga, će se samo API razinom dosljednost nudi za na Premium\_vrstu računa za pohranu LRS.

## <a name="deployment-considerations"></a>Okolnosti uvođenja

-   **Testirajte samo.** Implementacija Azure dosljedan prostor za pohranu u radnog okruženja koje koriste trenutno izdanje Microsoft Azure snop (Tehnički pretpregled). Ova verzija je namijenjena samo za potrebe za procjenu u okruženju Laboratorija test.

-   **Performanse**. Verzija Microsoft Azure stogu Tehnički pretpregled Azure dosljedan prostora za pohranu nije potpuno performanse Optimizirano.

-   **Skalabilnost.** Verzije Microsoft Azure stogu Tehnički pretpregled Azure dosljedan pohrane nisu optimizirane za skalabilnost.

## <a name="version-support-considerations"></a>Razmatranja podršku verzija

S ovom izdanju pretpregled Azure dosljedan prostora za pohranu podržane su sljedeće verzije:

> Azure prostora za pohranu klijenta biblioteke: [Microsoft Azure prostora za pohranu 6.x .NET SDK](http://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)
>
> Azure servise za pohranu podataka: [verzija 2015 04 05 REST API -JA](https://msdn.microsoft.com/library/azure/mt705637.aspx)
>
> Azure upravljanja servise za pohranu: [2015-05-01 – pregled](https://msdn.microsoft.com/library/azure/mt163683.aspx)
> [2015 06 15](https://msdn.microsoft.com/library/azure/mt163683.aspx)
## <a name="next-steps"></a>Daljnji koraci

-   [Uvod u dosljedan Azure prostora za pohranu](azure-stack-storage-overview.md)
