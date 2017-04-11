<properties
    pageTitle="Prijava analitiku dizajner prikaza | Microsoft Azure"
    description="Dizajner prikaza u analize zapisnika omogućuje stvaranje prilagođenih prikaza na konzoli OMS koje sadrže različite vizualizacije podataka u spremištu OMS. Ovaj članak sadrži pregled dizajner prikaza i postupke za stvaranje i uređivanje prilagođene prikaze."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer"></a>Dizajner prikaza analitičkih podataka za zapisnik
Dizajner prikaza u analize zapisnika omogućuje stvaranje prilagođenih prikaza na konzoli OMS koje sadrže različite vizualizacije podataka u spremištu OMS. Ovaj članak sadrži pregled dizajner prikaza i postupke za stvaranje i uređivanje prilagođene prikaze.

Ostali članci dostupne za dizajner prikaza su:

- [Pločica referenca](log-analytics-view-designer-tiles.md) – pregled postavki za svaku pločice dostupan za korištenje u prilagođene prikaze. 
- [Referenca dio vizualizacije](log-analytics-view-designer-parts.md) – pregled postavki za svaku pločice dostupan za korištenje u prilagođene prikaze. 


## <a name="concepts"></a>Koncepti
Prikazi koje su stvorene pomoću dizajner prikaza sadržavati elemente u tablici u nastavku.

| Dio | Opis |
|:--|:--|
| Pločica | Prikazuje se na nadzornoj ploči za glavni pregled analize zapisnika.  Obuhvaća vizualne sažimanje informacija koje se nalaze u prikazu prilagođene.  Različite vrste pločica sadrže različite vizualizacije zapisa u spremištu OMS.  Kliknite pločicu da biste otvorili prilagođeni prikaz. |
| Prilagođenog prikaza | Prikazuje kada korisnik klikne na pločici.  Sadrži jedan ili više dijelova vizualizaciju. |
| Dijelovi vizualizacije | Vizualizacija podataka u spremištu OMS na temelju jednog ili više [zapisnika pretraživanja](log-analytics-log-searches.md).  Većina dijelove neće sadržavati zaglavlja koja omogućuje više razine vizualizaciju i popis gornji rezultata.  Drugi dio vrste omogućuju različite vizualizacije zapisa u spremištu OMS.  Kliknite elemenata u dijelu za izvođenje zapisnika pretraživanja pružaju detaljne zapise. |

![Pregled dizajner prikaza](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Dodavanje dizajner prikaza radnog prostora
Dok je dizajner prikaza u pretpregledu, morate ga dodati u radni prostor tako da odaberete **Značajke pretpregleda** u odjeljku **Postavke** portala OMS.

![Omogući pretpregled](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Stvaranje i uređivanje prikaza

### <a name="create-a-new-view"></a>Stvaranje novog prikaza
Otvorite novi prikaz u **Dizajner prikaza** klikom na pločici dizajner prikaza na glavnom OMS nadzornoj ploči.

![Prikaz dizajnera pločica](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Uređivanje postojećeg prikaza
Da biste uredili postojeći prikaz u alatu za dizajniranje prikaza, otvorite prikaz klikom na njegov pločica u glavnom OMS nadzornu ploču.  Zatim kliknite gumb **Uređivanje** da biste otvorili prikaz u alatu za dizajniranje prikaza.

![Uređivanje prikaza](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Kloniraj postojećeg prikaza
Kada Kloniraj prikaza, stvara novi prikaz i otvara u alatu za dizajniranje prikaza.  Novi prikaz će imati isti naziv kao izvornik s "Kopiraj" dodane na kraj.  Da biste Kloniraj prikaza, otvorite postojeći prikaz klikom na njegov pločica u glavnom OMS nadzornu ploču.  Zatim kliknite gumb **Kloniraj** da biste otvorili prikaz u alatu za dizajniranje prikaza.

![Kloniraj prikaza](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Brisanje postojećeg prikaza
Da biste izbrisali postojeći prikaz, otvorite prikaz klikom na njegov pločica u glavnom OMS nadzornu ploču.  Zatim kliknite gumb **Uređivanje** da biste otvorili prikaz u alatu za dizajniranje prikaza, a zatim kliknite **Izbriši prikaz**.

![Brisanje prikaza](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Izvoz postojećeg prikaza
Možete izvesti u prikazu JSON datoteka koje možete uvesti u drugi radni prostor ili pomoću [predloška Azure Voditelj resursa](../resource-group-authoring-templates.md).  Da biste izvezli postojeći prikaz, otvorite prikaz klikom na njegov pločica u glavnom OMS nadzornu ploču.  Zatim kliknite gumb **Izvezi** da biste stvorili datoteku u mapi preuzimanja web-preglednika.  Naziv datoteke će biti naziv prikaza s nastavkom *omsview*.

![Izvoz prikaza](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Uvoz postojećeg prikaza
Možete uvesti datoteku *omsview* koje ste izvezli s druge grupe upravljanje.  Da biste uvezli postojeći prikaz, stvorite novi prikaz.  Zatim kliknite gumb **Uvoz** i odaberite datoteku *omsview* .  Konfiguraciju u datoteci se kopiraju u postojeći prikaz.

![Izvoz prikaza](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Rad s dizajner prikaza
Dizajner prikaza ima tri okna.  Okno za **dizajniranje** predstavlja prilagođenog prikaza.  Kada dodate pločice i dijelove iz okna **kontrola** okno za **dizajniranje** se dodaju se u prikaz.  U oknu **Svojstva** prikazat će svojstva za pločice ili odabranog dijela.

![Dizajner prikaza](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfiguriranje prikaz pločica
Prilagođeni prikaz može imati samo jedan element.  Odaberite karticu **pločica** na **upravljačkoj** ploči da biste vidjeli trenutnu pločicu ili odaberite jednu od zamjenski.  U oknu **Svojstva** će se prikazivati svojstava na trenutnu pločicu.  Konfiguriranje svojstava Popločaj prema detaljne informacije u [Poploči referenca](log-analytics-view-designer-tiles.md) , a zatim kliknite **Primijeni** da biste spremili promjene.

### <a name="configure-visualization-parts"></a>Konfiguriranje dijelove vizualizacije
Prikaz može obuhvaćati bilo koji broj dijelova vizualizaciju.  Odaberite karticu **Prikaz** , a zatim vizualizacije dio da biste dodali u prikazu.  U oknu **Svojstva** će se prikazivati svojstava odabranog dijela.  Konfiguriranje svojstava prikaza prema detaljne informacije u [vizualizacije dijela reference](log-analytics-view-designer-parts.md) , a zatim kliknite **Primijeni** da biste spremili promjene.

### <a name="delete-a-visualization-part"></a>Brisanje dijela vizualizacije
Dio vizualizacije iz prikaza možete ukloniti tako da kliknete gumb **X** u gornjem desnom kutu dijela.

### <a name="rearrange-visualization-parts"></a>Razmještanje dijelove vizualizacije
Prikazi imati samo jedan redak dijelova vizualizaciju.  Razmještanje postojeće dijelove u prikazu tako da kliknete i povučete ih na novo mjesto.


## <a name="next-steps"></a>Daljnji koraci

- Dodavanje [pločica](log-analytics-view-designer-tiles.md) na prilagođenog prikaza.
- Dodavanje [Dijelova vizualizacije](log-analytics-view-designer-parts.md) prilagođenog prikaza.
