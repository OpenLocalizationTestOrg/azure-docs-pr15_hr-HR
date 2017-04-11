<properties 
   pageTitle="Zapisnike događaja sustava Windows u prijava analitiku | Microsoft Azure"
   description="Zapisnike događaja sustava Windows su jedan izvor podataka najčešće koriste zapisnika analize.  U ovom se članku objašnjava kako konfigurirati skup zapisnike događaja sustava Windows i pojedinosti zapisa koji se stvaraju u spremištu OMS."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="windows-event-log-data-sources-in-log-analytics"></a>Izvori podataka zapisnika događaja sustava Windows u zapisnika analize

Zapisnike događaja sustava Windows su jedna od najčešće [izvore podataka](log-analytics-data-sources.md) koristi za Windows agenata Budući da je to način koji se koristi u većini aplikacija za prijavu informacije i pogrešaka.  Prikupite događaje iz standardnog zapisnika kao što je sustav i aplikacije osim koji navodi sve prilagođene zapisnika stvorio aplikacijama nužnim za praćenje.

![Događaja sustava Windows](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Konfiguriranje događaja sustava Windows prijavi

Konfiguriranje zapisnike događaja sustava Windows na [izborniku Podaci u postavke zapisnika analize](log-analytics-data-sources.md#configuring-data-sources).

Prijava analitiku samo prikupite događaje zapisnika događaja sustava Windows koji su navedeni u odjeljku postavke.  Možete dodati novi zapis tako da upišete naziv prijava, a zatim kliknete **+**.  Za svaki zapisnik događaja s odabranog severities prikupljaju se.  Provjerite severities za određeni zapisnik koji želite prikupiti.  Ne možete unijeti kriterije za filtriranje događaja.

![Konfiguriranje događaja sustava Windows](media/log-analytics-data-sources-windows-events/configure.png)


## <a name="data-collection"></a>Prikupljanje podataka

Prijava analitiku će prikupiti svaki događaj koji odgovara odabrane težinu iz nadziranim zapisnik događaja kao što je stvoriti događaj.  Agenta će u svaku evidenciji prikuplja iz zapis umjesto nje.  Ako agenta vodi izvan mreže za neko vrijeme, zatim prijava analitiku će prikupiti događaja od mjesta na kojem je zadnji put stali, čak i ako događaje stvorene dok agenta je izvan mreže.


## <a name="windows-event-records-properties"></a>Svojstva zapise za događaja sustava Windows

Zapisi događaja sustava Windows vrsti **događaja** i imate svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| Računalo            | Naziv računala na kojemu je prikupljenih događaj. |
| EventCategory       | Kategorija događaja. |
| EventData           | Svi podaci o događaju u obliku neobrađenog. |
| Iddogađaja             | Broj događaja. |
| EventLevel          | Težinu događaja u numeričku obrasca. |
| EventLevelName      | Težinu događaja u tekstnom obliku. |
| Zapisniku događaja            | Naziv zapisnika događaja koji je prikupljenih događaj. |
| ParameterXml        | Vrijednosti parametara događaja u XML formatu. |
| ManagementGroupName | Naziv grupe za upravljanje SCOM agenata.  Za ostale agenata to je AOI-<workspace ID> |
| RenderedDescription | Opis događaja s vrijednosti parametra |
| Izvor              | Izvor događaja. |
| SourceSystem  | Vrsta agent je prikupljenih događaj. <br> Povezivanje OpsManager – agent za Windows, bilo izravno ili SCOM <br> Linux – svi agenti Linux  <br> AzureStorage – Azure dijagnostiku |
| TimeGenerated       | Datum i vrijeme događaja stvorena u sustavu Windows. |
| Korisničko ime            | Korisničko ime računa koji zapisuje događaj. |



## <a name="log-searches-with-windows-events"></a>Pretraživanje zapisnika pomoću događaja sustava Windows

Sljedeća tablica sadrži različite Primjeri zapisnika pretraživanja koja se dohvaćanje zapisa događaja sustava Windows.

| Upit | Opis |
|:--|:--|
| Vrsta = događaja | Svih događaja sustava Windows. |
| Vrsta = događaj EventLevelName = pogreške | Svi događaji Windows s težinu pogreške. |
| Vrsta = događaj & #124; Izmjerite count() izvor | Count Windows događaji izvor. |
| Vrsta = događaj EventLevelName = & pogreške #124; Izmjerite count() izvor | Count Windows događaje s pogreškom izvor. |

## <a name="next-steps"></a>Daljnji koraci

- Konfiguriranje prijava analitiku u prikupljanje drugih [izvora podataka](log-analytics-data-sources.md) za analizu.
- Informirajte se o [zapisniku pretraživanja](log-analytics-log-searches.md) radi analize podataka prikupljenih iz izvora podataka i rješenja.  
- Pomoću [Prilagođenih polja](log-analytics-custom-fields.md) rastavljanje zapisi događaja u pojedinačna polja.
- Konfiguriranje [Skup mjerača performansi](log-analytics-data-sources-performance-counters.md) sustava Windows agenata.