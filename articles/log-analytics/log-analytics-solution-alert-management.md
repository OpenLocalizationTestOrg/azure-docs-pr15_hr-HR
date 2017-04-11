<properties
   pageTitle="Upozorenje rješenje za upravljanje u paketu operacije upravljanja (OMS) | Microsoft Azure"
   description="Rješenje upozorenja upravljanja u prijava analitiku pomaže u analizirati svih upozorenja u svom okruženju.  Uz konsolidaciju upozorenja koje generira unutar OMS je uvozi upozorenja iz povezanih grupa za upravljanje sustava centra operacije Manager (SCOM) u zapisnik analize."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/06/2016"
   ms.author="bwren" />

# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Upozorenja rješenje za upravljanje u paketu operacije upravljanja (OMS)

![Ikona upozorenja upravljanja](media/log-analytics-solution-alert-management/icon.png) Rješenje upozorenja upravljanja pomaže u analizu svih upozorenja u svom okruženju.  Uz konsolidaciju upozorenja koje generira unutar OMS je uvozi upozorenja iz povezanih grupa za upravljanje sustava centra operacije Manager (SCOM) u zapisnik analize.  U okruženjima s više grupa Upravljanje rješenja upozorenja upravljanja dat će konsolidirani prikaz upozorenja preko svih grupa Upravljanje.

## <a name="prerequisites"></a>Preduvjeti

- Da biste uvezli upozorenja iz SCOM, ovo je rješenje potrebno je veza između OMS radnog prostora i upravljanje grupu SCOM pomoću postupak opisan u [Povezivanje prijava analitiku u komponente Operations Manager](log-analytics-om-agents.md).  


## <a name="configuration"></a>Konfiguracija

Dodajte rješenje upozorenja upravljanja OMS radnog prostora koristeći postupak opisan u [Dodaj rješenja](log-analytics-add-solutions.md).  Postoji bez daljnje konfiguracije obavezno.

## <a name="management-packs"></a>Upravljanje paketi

Ako vaša grupa Upravljanje SCOM je povezano s OMS radnog prostora, zatim sljedećim paketima za upravljanje instalirat će se SCOM prilikom dodavanja rješenja.  Postoji konfiguracije ni održavanja te upravljanje paketa potrebna.  

- Centar za sustav Microsoft Savjetnik za upozorenja Management (Microsoft.IntelligencePacks.AlertManagement)

Dodatne informacije o kako je rješenje upravljanja paketi ažuriraju, potražite u članku [Povezivanje prijava analitiku u komponente Operations Manager](log-analytics-om-agents.md).

## <a name="data-collection"></a>Prikupljanje podataka

### <a name="agents"></a>Agenata

U sljedećoj tablici opisane povezanih izvora koje podržava rješenja.

| Povezani izvora | Podrška | Opis |
|:--|:--|:--|
| [Windows agenata](log-analytics-windows-agents.md) | ne | Izravni Windows agenata generirati SCOM upozorenja. |
| [Linux agenata](log-analytics-linux-agents.md) | ne | Izravni Linux agenata generirati SCOM upozorenja. |
| [Grupa Upravljanje SCOM](log-analytics-om-agents.md) | Da | Upozorenja koje generiraju SCOM agenata isporučena u grupi Upravljanje, a proslijediti zapisnika analize.<br><br>Izravni veza SCOM agent prijava analitiku nije potrebna. Upozorenja podaci prosljeđuju se iz grupe za Upravljanje spremištem OMS. |
| [Račun za Azure prostora za pohranu](log-analytics-azure-storage.md) | ne | SCOM upozorenja ne spremaju se u računi Azure prostora za pohranu. |

### <a name="collection-frequency"></a>Učestalost zbirke

Upozorenja koje generira unutar OMS dostupnih rješenje odmah.  Upozorenja slanje podataka iz grupe upravljanje SCOM prijava analitiku svake 3 minute.  

## <a name="using-the-solution"></a>Korištenje rješenja

Kada dodate rješenje upozorenja upravljanja OMS radnog prostora, pločicu **Upozorenja upravljanje** će se dodati OMS nadzorne ploče.  Pločica prikazuje count i grafički prikaz broja trenutno aktivan upozorenja koja se pojavile su se unutar zadnja 24 sata.  Ne možete promijeniti ovog vremenskog razdoblja.

![Upozorenja upravljanje pločica](media/log-analytics-solution-alert-management/tile.png)

Kliknite pločicu **Upozorenja upravljanje** da biste otvorili **Upozorenja upravljačku** nadzornu ploču.  Na nadzornoj ploči obuhvaća stupaca u tablici u nastavku.  Svaki stupac gornji deset upozorenja navedene su po broj koji odgovaraju kriterijima taj stupac za navedeni opseg i vremenskog razdoblja.  Možete pokrenuti pretraživanje zapisnika koja omogućuje cijeli popis tako da kliknete **vidjeli sve** pri dnu stupac ili tako da kliknete zaglavlje stupca.

| Stupac| Opis |
|:--|:--|
| Ključna upozorenja | Sva upozorenja s težinu kritično grupirane prema nazivu upozorenja.  Kliknite upozorenje naziv da biste pokrenuli pretraživanje zapisnika vraćanje svih zapisa za taj upozorenje. |
| Upozorenje upozorenja | Sva upozorenja s težinu upozorenje grupirane prema nazivu upozorenja.  Kliknite upozorenje naziv da biste pokrenuli pretraživanje zapisnika vraćanje svih zapisa za taj upozorenje. |
| Aktivni SCOM upozorenja | Sva upozorenja SCOM s bilo kojom stanja osim *Zatvoreno* grupirane prema izvoru koji je generirao upozorenja. |
| Svi aktivni upozorenja | Sva upozorenja s bilo kojeg težinu grupirane prema nazivu upozorenja. Sadrži samo SCOM upozorenja s bilo kojeg stanje osim *Zatvoreno*.|

Ako se pomaknite se udesno na nadzornoj ploči će se popis nekoliko uobičajenih upite koji možete kliknuti da biste izveli [zapisnika pretraživanja](log-analytics-log-searches.md) za upozorenja podatke.

![Upozorenja upravljanje nadzorne ploče](media/log-analytics-solution-alert-management/dashboard.png)

## <a name="scope-and-time-range"></a>Opseg i vrijeme raspona

Po zadanom je djelokrug upozorenja analizirati u rješenje upozorenja upravljanja iz svih povezanih upravljanje grupa koje generira unutar zadnjih sedam dana.  

![Upozorenja upravljanje dosega](media/log-analytics-solution-alert-management/scope.png)

- Da biste promijenili grupe upravljanje obuhvaćeno analizu, kliknite **opseg** pri vrhu nadzorne ploče.  Možete odabrati **Global** za sve grupe povezanih upravljanje ili **Upravljanje grupe** da biste odabrali grupu jedan upravljanje.

- Da biste promijenili vremenski raspon upozorenja, odaberite **podataka na temelju** pri vrhu nadzorne ploče.  Možete odabrati upozorenja koje generira pada u zadnjih sedam dana, jedan dan ili 6 sati.  Ili možete odabrati **Prilagođeni** i navedite u prilagođenom rasponu datuma.

## <a name="log-analytics-records"></a>Prijava analitiku zapisa

Rješenje upozorenja upravljanja analizira bilo koji zapis s vrstom **upozorenja**.  Bit će i uvoz upozorenja iz SCOM i stvaranje odgovarajući zapis za svaku s vrstom **upozorenja** i SourceSystem od **OpsManager**.  Ti se zapisi imaju svojstva u tablici u nastavku.  

| Svojstvo | Opis |
|:--|:--|
| Vrsta | *Upozorenje* |
| SourceSystem | *OpsManager* |
| AlertContext | Detalji o podatkovne stavke koje su uzrokovale upozorenje generiranje u XML formatu. |
| AlertDescription | Detaljan opis upozorenja. |
| AlertId | GUID upozorenja. |
| AlertName | Naziv upozorenja. |
| AlertPriority | Razinu prioriteta upozorenje. |
| AlertSeverity | Težinu razina upozorenja. |
| AlertState | Najnovije stanje razlučivost upozorenja. |
| LastModifiedBy | Naziv korisnika koji je zadnji izmijenio upozorenje. |
| ManagementGroupName | Naziv grupe upravljanje kojem je generirana upozorenje. |
| RepeatCount | Broj vremena isti upozorenje generiran za isti nadzirati objekt od vezanog. |
| ResolvedBy | Naziv korisnika koji je riješiti upozorenje. Pražnjenje ako upozorenje još uvijek nije riješen. |
| SourceDisplayName | Zaslonski naziv objekta nadzor koji je generirao upozorenja. |
| SourceFullName | Puni naziv objekta nadzor koji je generirao upozorenja. |
| TicketId | ID karata upozorenja ako okruženje SCOM integriran s postupku za dodjeljivanje karticama za upozorenja.  Prazan od bez karata ID je dodijeljen. |
| TimeGenerated | Datum i vrijeme stvaranja upozorenja. |
| TimeLastModified | Datum i vrijeme zadnje promjene upozorenje. |
| TimeRaised | Datum i vrijeme koja je stvorena upozorenje. |
| TimeResolved | Datum i vrijeme koje ste razriješili upozorenje. Pražnjenje ako upozorenje još uvijek nije riješen. |

## <a name="sample-log-searches"></a>Ogledna zapisnika pretraživanja

Sljedeća tablica sadrži ogledne zapisnika pretraživanja za upozorenja zapise koji se prikupljaju rješenja.  

| Upit | Opis |
|:--|:--|
| Vrsta = upozorenja SourceSystem = OpsManager AlertSeverity = pogreške TimeRaised > SADA 24 sata | Ključna upozorenja potenciju tijekom proteklih 24 sata |
| Vrsta = upozorenja AlertSeverity = upozorenje TimeRaised > SADA 24 sata | Upozorenje upozorenja potenciju tijekom proteklih 24 sata  |
| Vrsta = upozorenja SourceSystem = OpsManager AlertState! = zatvoreni TimeRaised > SADA 24 sata i #124; Izmjerite count() kao zbroj po SourceDisplayName | Izvori aktivni upozorenja potenciju tijekom proteklih 24 sata |
| Vrsta = upozorenja SourceSystem = OpsManager AlertSeverity = pogreške TimeRaised > AlertState SADA 24 sata! = Zatvoreno | Ključna upozorenja potenciju tijekom proteklih 24 sata koje su još uvijek aktivan |
| Vrsta = upozorenja SourceSystem = OpsManager TimeRaised > AlertState SADA 24 sata = Zatvoreno | Upozorenja na potenciju tijekom proteklih 24 sata koje su sada zatvorene |
| Vrsta = upozorenja SourceSystem = OpsManager TimeRaised > SADA 1 dana i #124; Izmjerite count() kao zbroj po AlertSeverity | Upozorenja na potenciju tijekom proteklih 1 dan grupirane prema svojim težinu |
| Vrsta = upozorenja SourceSystem = OpsManager TimeRaised > SADA 1 dana i #124; Sortiranje RepeatCount desc | Upozorenja na potenciju tijekom proteklih 1 dan sortirani prema vrijednosti njihov broj ponavljanja |

## <a name="next-steps"></a>Daljnji koraci

- Informirajte se o [upozorenja u zapisnik analize](log-analytics-alerts.md) pojedinosti o generiranje upozorenja iz zapisnika analize.
