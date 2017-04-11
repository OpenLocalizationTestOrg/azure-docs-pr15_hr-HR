<properties 
   pageTitle="Tehnički specifikacije StorSimple | Microsoft Azure"
   description="U članku se opisuje tehničke opisi i regulatorne standarde usklađenosti informacije za hardverske komponente StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Tehnički i opisi i usklađenost za uređaj StorSimple

## <a name="overview"></a>Pregled

Hardverske komponente Microsoft Azure StorSimple uređaja u skladu tehničke opisi i regulatorne standarde navedene u ovom članku. Tehnički specifikacije opisuju Power i hlađenje modula (PCMs), diskovnih pogona, kapacitet pohrane i priloge. Informacije o usklađenosti pokriva stvari kao međunarodne standarde, sigurnost i emissions i kabeli.


## <a name="power-and-cooling-module-specifications"></a>Specifikacije Power i hlađenje modul  

Uređaj StorSimple ima dvije 100 240V dva Lepeza, SBB usklađen Power hlađenje Module (PCMs). To omogućuje suvišnih power konfiguracije. Ako je uo ne uspije, uređaj i dalje funkcionira na druge uo dok je zamijenjena modul nije uspjelo.  

S prilozima EBOD koristi 580 uo W, a primarni s prilozima koristi 764 uo W. U sljedećim tablicama navedeni tehničke specifikacije pridružene na PCMs.

| Specifikacija           | 580 UO W (EBOD)                                    | 764 uo W (primarni)                                |
|------------------------ | --------------------------------------------------- | -------------------------------------------------- |
| Maksimalna izlaz power    | 580 W                                               | 764                                                |
| FREQUENCY               | 50/60 Hz                                            | 50/60 Hz                                           |
| Napona odabirom raspona | Automatsko rasponu: V AC 90 – 264, Hz 47-63               | Automatsko rasponu: 90-264 V AC, Hz 47-63               |
| Maksimalna inrush trenutne  | 20 ODGOVORA                                                | 20 ODGOVORA                                               |
| [Korekcijski faktor za Power | > unos napona nominal 95%                          | > unos napona nominal 95%                         |
| Harmonics               | Ispunjava EN61000-3-2                                   | Ispunjava EN61000-3-2                                  |
| Izlaz                  | 5V čekanja napona @ 2.0 odgovora                          | 5V čekanja napona @ 2.7 odgovora                         |
|                         | + 5V @ 42 odgovora                                          | + 5V @ 40 odgovora                                         |
|                         | + 12V @ 38 odgovora                                         | + 12V @ 38 odgovora                                        |
| Tipkovne uključiv           |  Da                                                | Da                                                |
| Skretnice i LEDs       | Promjena AC uključenu/isključenu bilješku i pokazatelj četiri stanja LEDs     | Promjena AC uključenu/isključenu bilješku i pokazatelj šest stanja LEDs     |
| S prilozima hlađenje       | Axial hlađenje sporta s kontrolom varijable Lepeza brzine  | Axial hlađenje sporta s kontrolom varijable Lepeza brzine |

 
## <a name="power-consumption-statistics"></a>Statistika potrošnju energije  

Sljedeća tablica prikazuje podatke potrošnje uobičajeni power (stvarnih vrijednosti može se razlikovati od u objavljeni) za različite modele StorSimple uređaja. 
 
 Uvjeta | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC 
 ---------- | -------- | -------- | -------- | -------- | -------- | -------- 
 Sporta neaktivnosti spor, pogona | 1.45 ODGOVORA  |0.31 kW | 1057.76 BTU hr | 3.19 ODGOVORA | 0.34 kW | 1160.13 BTU hr 
 Pristup sporta spor, pogona | 1.54 ODGOVORA | 0.33 kW | 1126.01 BTU hr | 3.27 ODGOVORA | 0.36 kW | 1228.37 BTU hr 
 Sporta brzo, pogona Neaktivno, dva PSUs pokreće | 2.14 ODGOVORA | 0.49 kW  | 1671.95 BTU hr | 4,99 ODGOVORA | 0.54 kW | 1842.56 BTU hr 
 Sporta brzo, pogoni neaktivan, jedan PSU pokreće nešto neaktivnosti | 2.05 ODGOVORA | 0.48 kW | 1637.83 BTU hr | 4.58 ODGOVORA | 0.50 kW | 1706.07 BTU hr 
 Pristup dva PSUs pokreće sporta brzo, pogona | 2.26 ODGOVORA | 0.51 kW | 1740.19 BTU hr | 4.95 ODGOVORA | 0.54 kW | 1842.56 BTU hr 
 Sporta brzo pogona pristupate, jedan PSU pokreće nešto neaktivan | 2.14 ODGOVORA |0.49 kW | 1671.95 BTU hr | 4.81 ODGOVORA  | 0.53 kW | 1808.44 BTU hr 

## <a name="disk-drive-specifications"></a>Specifikacije pogon diska  

Vaš uređaj StorSimple podržava do 12 3.5 inčni obrazac faktor serijskih priložene SCSI (SAS) diskovnih pogona. Stvarni pogona može biti kombinacije solid-state pogona (SSDs) ili tvrdom disku pogona (HDDs), ovisno o konfiguraciji proizvoda. 12 slobodnih pogon diska nalaze se u konfiguraciji 3, 4 ispred na s prilozima. S prilozima EBOD omogućuje dodatan prostor za pohranu za drugi 12 diskovnih pogona. To su uvijek HDDs.  

## <a name="storage-specifications"></a>Specifikacije prostora za pohranu
Uređaji StorSimple imaju kombinacije pogona tvrdog diska i pogona solid-state na 8100 i 8600. Ukupni kapacitet upotrebljivosti u 8100 i 8600 su otprilike 15 TB i 38 TB odnosno. U sljedećoj su tablici dokumenti detalje o SSD, podizanje tvrdog diska i oblaka kapaciteta u kontekstu kapaciteta StorSimple rješenja.

| Model uređaja / kapaciteta                         | 8100                                                    | 8600                                                    |
|------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| Broj tvrdi disk pogoni (HDDs)              |   8                                                     |  19                                                     |
| Broj solid-state pogona (SSDs)            |   4                                                     |  5                                                      |
| Jedan podizanje tvrdog diska kapaciteta                            |   4 TB                                                  |  4 TB                                                   |
| Jedan SSD kapaciteta                            |   400 GB                                                |  800 GB                                                 |
| Rezervnih kapaciteta                                 |   4 TB                                                  | 4 TB                                                    |
| Neupotrebljiv podizanje tvrdog diska kapaciteta                            | 14 TB                                                   | 36 TB                                                   |
| Neupotrebljiv SSD kapaciteta                            | 800 GB                                                  | 2 TB                                                    |
| Ukupna neupotrebljiv kapaciteta *                         | ~ 15 TB                                                 | ~ 38 TB                                                 |
| Maksimalna rješenje kapaciteta (uključujući oblak)    | 200 TB                                                  | 500 TB                                                  |


<sup> * </sup> -  *Ukupni upotrebljivosti kapacitet obuhvaća kapacitet dostupne za podatke, metapodataka i buffers.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Dimenzije s prilozima i Debljina specifikacije  

U sljedećim tablicama navedeni različite specifikacije s prilozima dimenzije i debljine.  

### <a name="enclosure-dimensions"></a>Dimenzije s prilozima
U sljedećoj su tablici navedeni dimenzije s prilozima u milimetre i inča.

|S prilozima |Milimetre |Inča |
|----------|------------|-------| 
| Visina |87.9 | 3.46 |
| Širina preko ugrađivanjem flange | 483 | 19.02 |
| Širina preko tijelo s prilozima | 443 | 17.44 |
| Dubina iz prednju ugrađivanjem flange extremity tijela s prilozima | 577 | 22.72 |
| Dubina s ploče operacije najdalju extremity s prilozima | 630.5 | 24.82 |
| Dubina iz flange ugrađivanjem najdalju extremity s prilozima |   603 | 23.74 |

### <a name="enclosure-weight"></a>Težina s prilozima  

Ovisno o konfiguraciji, potpuno učita primarni s prilozima možete računa iz 21 33 kgs i zahtijeva dvije osobe i učiniti ga. 
 
| S prilozima | Težina |
|-----------|--------| 
| Najveća težina (ovisi o konfiguraciji) |30 kg – 33 kg |
| Prazno (nema prilagođene jednadžbi: pogona) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Specifikacije okruženje s prilozima  

U ovom se odjeljku navedeni specifikacije povezane s prilozima okruženje. Na temperature, humidity, Nadmorska visina, shock, vibracije, usmjerenje, sigurnost i elektromagnetskom kompatibilnost (EMC) nalaze se u ovoj kategoriji.  

### <a name="temperature-and-humidity"></a>Temperature i humidity

| S prilozima        | Razine okolne temperatura raspona  | Razine okolne humidity zavisne | Maksimalna Svježa bulb   |
|------------------|----------------------------|---------------------------|--------------------|
| Radu      | 5° C - 35° C (41° F - 95° F)    | 20% - 80% – condensing nisu | 28° P (82° F)        |
| Osobe koje nisu bile  | -C 40° C - 70° (40° F - 158° F) | 5 – 100% koje nisu-condensing  | 29° P (84° F)        |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Zraka, Nadmorska visina, shock, vibracije, usmjerenje, sigurnost i EMC
 
| S prilozima          | Specifikacije radu                                                |
|--------------------|---------------------------------------------------------------------------| 
| Zraka            | Sustava zraka je prve do stražnja. Sustav mora biti kojim upravlja low-pressure, mogu iscrpiti stražnja instalacijama. Natrag tlaka stvorene za bicikle vrata i prepreke smije 5 paskalima (mjerača vode 0,5 mm). |
| Nadmorska visina, radu  | -30 metar na 3045 metar (-100 metar 10 000 metar) s maksimalno operacijski temperatura deaktivirali ocjenjivanje 5 ° c iznad 7000 stopa. |
| Nadmorska visina, koje nisu bile  | metar-305 za 12,192 metar (-1000 stopa 40,000 stopa) |
| Upotrebljiva shock  | sinus 10 ms ½ 5g |
| Koje nisu bile shock  | sinus 10 ms ½ 30g |
| Upotrebljiva vibracije  | 0.21g RMS 5 500 Hz izravnim |
| Koje nisu bile vibracije  | 1.04g RMS 2 200 Hz izravnim |
| Vibracije, relocation  | sinus 3g 2 200 Hz |
| Usmjerenje i ugrađivanjem  | 19" za bicikle postavljanja (2 EIA jedinice) |
| Tračnicama za bicikle  | Kako bi stao dubine minimalne 700 mm (31.50 inča) nosači za sukladan s IEC 297 |
| Sigurnost i odobrenja  |   ĆE i Hangul EN 61000-3 IEC 61000-3 Hangul 61000-3 |
| EMC  | EN55022 (CISPR - A), FCC ODGOVORA |

## <a name="international-standards-compliance"></a>Međunarodne standarde usklađenosti
Uređaju Microsoft Azure StorSimple usklađenost sa sljedećim međunarodne standarde:  

- ĆE - HR 60950-1  
- Izvješće CB IEC 60950-1  
- Hangul i cUL za Hangul 60950-1  

## <a name="safety-compliance"></a>Sigurnost usklađenosti  

Vaš uređaj Microsoft Azure StorSimple zadovoljava sljedeće ocjene sigurnost:  

- Sustav proizvoda vrsta odobrenje: Hangul, cUL, će  
- Sigurnost usklađenosti: Hangul 60950 IEC 60950, hr 60950  

## <a name="emc-compliance"></a>Usklađenost EMC 

Vaš uređaj Microsoft Azure StorSimple zadovoljava sljedeće EMC ocjena.  

### <a name="emissions"></a>Emissions

Uređaj je EMC usklađen za obavljaju i radiated emissions razine.  

- Obavljaju emissions ograničiti razine: odgovarajuće 47 dio 15B predmete odgovora EN55022 predmete odgovora CISPR predmete odgovora  
- Radiated emissions ograničiti razine: odgovarajuće 47 dio 15B predmete odgovora EN55022 predmete odgovora CISPR predmete odgovora   

### <a name="harmonics-and-flicker"></a>Harmonics i titranje  

Uređaj usklađenost sa EN61000-3-2 i 3.  

### <a name="immunity-limit-levels"></a>Immunity ograničenje razine  
Uređaj usklađenost sa EN55024.  

## <a name="ac-power-cord-compliance"></a>AC power kabel usklađenosti
  
Na Uključi i u sklopu kabel dovršeno power mora zadovoljiti standarde odgovarajuće države u kojima se koristi uređaj, a moraju imati sigurnost odobrenja koji su vam prihvatljivi u toj državi. U sljedećim tablicama navedeni standarde SAD i Europe.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>AC power kablovima - SAD-a (mora biti NRTL naveden)

| Komponenta       | Specifikacija                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Vrsta kabel       | SZ ili SVT 18 AWG minimum, 3 Dirigent 2.0 metar Maksimalna duljina |
| Uključi            | NEMA 5 15P grounding vrsta privitka Uključi ocjenjivanje 120 V, A; 10 ili IEC 320 C14, 250 V, 10 odgovora |
| Socket          | IEC 320 C-13, 250 V, 10 ODGOVORA                                         |

### <a name="ac-power-cords---europe"></a>AC power kablovima - Europe

| Komponenta       | Specifikacija                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Vrsta kabel       | Harmonized H05 VVF 3G1.0                                         |
| Socket          | IEC 320 C-13, 250 V, 10 ODGOVORA                                         |

## <a name="supported-network-cables"></a>Podržani mrežni kabeli  

Za 10 GbE mreže sučelja podataka 2 i 3 podataka, pogledajte [popis podržanih mrežni kabeli i moduli](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Daljnji koraci

Sada ste spremni za implementaciju StorSimple uređaj u vašem podatkovnog centra. Dodatne informacije potražite u članku [Implementacija lokalnog uređaja](storsimple-deployment-walkthrough-u2.md).  
