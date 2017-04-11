<properties 
    pageTitle="Tvorničke podataka – pravila imenovanja | Microsoft Azure" 
    description="U članku se opisuje imenovanja pravila za entiteti tvorničke podataka." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---naming-rules"></a>Azure podataka tvorničke - imenovanja pravila 
Sljedeća tablica sadrži pravila imenovanja za artefakte tvorničke podataka.



Ime | Jedinstvenost naziva | Provjere valjanosti
:--- | :-------------- | :----------------
Tvorničke podataka | Jedinstveni preko Microsoft Azure. Nazivi razlikuju velika i mala slova, odnosno MyDF i mydf odnose se na istom tvorničke podataka. |<ul><li>Svaki tvorničke podataka je uz jedan Azure pretplate.</li><li>Nazivi objekata morate pokrenuti slovom ili broj, a mogu sadržavati samo slova, brojeve i znak crticu (–).</li><li>Svaki znak crticu (–) moraju biti odmah ispred i iza koje slijedi slovo ili broj. Uzastopne crtice nisu dopušteni u spremniku nazivima.</li><li>Naziv može biti 3 63 znakova.</li></ul>
Povezani servisi/tablica/kanali | Jedinstveni s u podataka tvorničke. Nazivi su velika i mala slova. | <ul><li>Najveći broj znakova u nazivu tablice: 260.</li><li>Nazive objekata počinju moraju slovo broj ili podvlaka (_).</li><li>Sljedeći znakovi nisu dopušteni: ".", "+", "?", "/", "<", ">","*", "%", "i", ":","\\"</li></ul>
Grupa resursa | Jedinstveni preko Microsoft Azure. Nazivi su velika i mala slova. | <ul><li>Najveći broj znakova: 1000.</li><li>Naziv može sadržavati slova, brojeve i sljedeće znakove: "-", "_",","i".".</li></ul>