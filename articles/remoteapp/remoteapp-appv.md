<properties
    pageTitle="Pomoću aplikacije V aplikacije Azure RemoteApp | Microsoft Azure"
    description="Saznajte kako koristiti aplikaciju V aplikacije Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="using-app-v-apps-in-azure-remoteapp"></a>Korištenje aplikacija App-V Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

U zbirci hibridnog Azure RemoteApp koji su potrebni spoj domene možete koristiti aplikacije V aplikacije.

Prije nego što počnete, provjerite je li instalirati aplikaciju V 5.1 klijent s najnovijim ažuriranjima. Morat ćete stvoriti [prilagođenu sliku](remoteapp-create-custom-image.md) koja obuhvaća aplikaciju V klijenta.  

Da biste koristili postojeću infrastrukturu App-V s Azure RemoteApp jednostavno je. Jer hibridnog zbirka je implementiran u programa Azure VNET koja ima pristup kontroler domene i na VMs su domene pridružili, na raspolaganju su vaše postojeće aplikacije v Infrastruktura i implementaciju načine easyily glavna aplikacija App-V Azure RemoteApp. Evo što valja koje ste trebali imati na umu ovise o vrsti implementaciju App-V koju trenutno imate:

| Konfiguracija mogućnosti |                       | Pozitivan                                                               | Negativan                                                                                              |
|-----------------------|-----------------------|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Način isporuke       | Strujanje (zahtjev) | Aplikacija je uvijek najnoviji i Osvježi                                     | Prvi put Latencija                                                                                    |
|                       | Ugrađena               | Najbrži; aplikacija je već postoje u VM                              | Napuhavanje - traje prostor na sliku (127 GB ograničenja)                                                           |
| Aplikacija mjesto spremišta  | Zajednički sadržaji        | Aplikacija izvodi u memoriji instance Azure RemoteApp                         | Eats memorije i dobru vezu strujanje server (datoteka) u kojoj se nalazi aplikacija                      |
|                       | Na disku (predmemorirani)         | Brzo izvršavanje. Aplikacija ne ovise o dostupnosti izvora sadržaja | Napuhavanje - traje prostor na sliku (127 GB ograničenja)                                                           |
| Određivanje             | Korisnik                  | Zahtijeva puno samostalne App-V infrastrukture                          |                                                                                                       |
|                       | Globalnih (računalo)      |  Unaprijed objavu ili ciljani pomoću objavljivanje poslužitelja                         |  Trebate ažurirati Azure sliku ako želite ažurirati aplikaciju (vrlo veliko). Zauzima prostor na slici. |

 Nakon što stvorite prilagođenu sliku i zbirka hibridnog, objavite svoju aplikaciju, korisnicima dodijelili i uživati u postojeće aplikacija App-V smješten u Azure RemoteApp isporučena u bilo kojem uređaju bilo kojeg mjesta.
