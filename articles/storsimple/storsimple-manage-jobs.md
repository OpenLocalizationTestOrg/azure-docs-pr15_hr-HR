<properties 
   pageTitle="Prikaz i upravljanje zadacima StorSimple | Microsoft Azure"
   description="U članku se opisuje stranici StorSimple Upravitelj servisa zadacima i kako ga koristiti za praćenje nedavne, trenutni i zakazani sigurnosne kopije zadataka."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs"></a>Prikaz i upravljanje zadacima StorSimple pomoću StorSimple Upravitelj servisa

[AZURE.INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Pregled

Stranica **poslove** sadrži jedan središnje portal za prikaz i upravljanje zadacima koji su pokrenuti na uređajima povezani StorSimple Upravitelj usluge. Možete vidjeti zakazano izvodi, dovršeni te nije uspjelo zadatke za više uređaja. Rezultati se prikazuju u tabličnom obliku. 

![Zadataka](./media/storsimple-manage-jobs/HCS_JobsPage.png)

Možete brzo pronaći zadatke koji su zainteresirani filtriranjem prema polja kao što su:

- **Status** – poslovi mogu se izvoditi zakazani, nije uspio, dovršeni, odustajanje od ili otkazani.

- **Vrsta** – poslove koje se mogu stvoriti uslijed rasporedu ili osvježavati backup (**Poduzeti sigurnosne kopije**), kloniranje, uređaj Vrati ili operaciju ažuriranja.

- **Uređaji** – poslove su pokrenuti na određeni uređaj povezan s na servisu.

- **Od i do** – poslove moguće je filtrirati prema rasponu datuma i vremena.

Filtrirani poslove pa su pozivu na temelju sljedećim atributima:

- **Vrsta** – sigurnosne kopije, Kloniraj, vraćanje, prebacivanje ili ažuriranje.

- **Status** – sustavom, zakazano, nije uspio, dovršeni, odustajanje od ili otkazani.

- **Entitet** – poslove mogu se pridružiti jedinicu, sigurnosne kopije pravila ili uređaj. Posao Kloniraj pridruženo jedinicu, dok je povezan s sigurnosne kopije pravila zakazano sigurnosno kopiranje. Uređaj posao stvara se kao rezultat Izrada oporavak (DR) ili postupak vraćanja.

- **Uređaj** – naziv uređaja na kojem je posao započet.

- **Rada na** – vrijeme pokretanja posla.

- **Tijeku** – postotak dovršenog izvodi posla. Za dovršenog posla, to mora uvijek biti 100%.

Na popisu zadataka Osvježi svakih 30 sekundi.

Na ovoj stranici možete izvoditi sljedeće radnje vezane uz zadatak:

- Prikaz detalja o posla

- Otkazivanje posla

## <a name="view-job-details"></a>Prikaz detalja o posla

Izvršite sljedeće korake da biste vidjeli detalje bilo koji zadatak.

#### <a name="to-view-job-details"></a>Da biste vidjeli detalje posla

1. Na stranici **Poslovi** prikazati od poslova su zainteresirani tako da pokrenete upit s odgovarajućim filtrima. Možete pretraživati za završi, pokrenut ili otkazana zadatke.

2. Odaberite posao.

3. Pri dnu stranice kliknite **Detalji**.

4. U dijaloškom okviru **Detalji zadatka sigurnosnog kopiranja** možete pogledati status, pojedinosti, Statistika vrijeme i podacima statistike.

## <a name="cancel-a-job"></a>Otkazivanje posla

Izvršite sljedeće korake da biste poništili izvodi posao.

### <a name="to-cancel-a-job"></a>Da biste poništili posao

1. Na stranici **Poslovi** prikaz izvodi od poslova koji želite otkazati tako da pokrenete upit s odgovarajućim filtrima.

1. Odaberite posao.

1. Pri dnu stranice, kliknite **Odustani**.

1. Kada se zatraži potvrda, kliknite **da**.

Ovaj zadatak sada prekinuti.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [upravljati police StorSimple sigurnosne kopije](storsimple-manage-backup-policies.md).

- Saznajte kako [pomoću upravitelja StorSimple servisa za administraciju uređaju StorSimple](storsimple-manager-service-administration.md).
