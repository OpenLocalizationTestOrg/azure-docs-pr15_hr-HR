<properties 
   pageTitle="Hardvera StorSimple 10 GbE sučelja | Microsoft Azure"
   description="Opisuje podržani small obrasca dvofaktorska analiza varijance uključiv (SFP) transceivers, kabela i parametri naredbenog retka za 10 GbE sučelja mrežom na uređaju StorSimple."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>Podržani hardver 10 sučelja GbE mrežom na uređaju StorSimple

## <a name="overview"></a>Pregled

Ovaj članak sadrži informacije o zamjenskom hardveru koji funkcionira s uređajem Microsoft Azure StorSimple.

## <a name="list-of-devices-tested-by-microsoft"></a>Popis uređaja testirati Microsoft

Microsoft provjerio sljedeće small obrasca dvofaktorska analiza varijance uključiv (SFP) transceivers, kabeli i parametri da biste bili sigurni da funkcioniraju optimalnog s uređajima. (U tablicama u nastavku ažurirat će se kao novi hardver testira.)

### <a name="sfp-transceivers"></a>SFP + Transceivers

|Provjerite|Model|
|---|---|
|Cisco|SFP 10G SR|

### <a name="cables"></a>Kabeli

|S. ne. |Provjerite|Model|
|---|---|---|
| 1.|Cisco|SFP H10GB CU1M|
| 2.|Cisco|SFP H10GB CU2M|
| 3.|Cisco|SFP H10GB CU3M|
| 4.|Ograničeno Tripp|N820 - 05M (OM3)|

### <a name="switches"></a>Parametri

|S. ne.|Provjerite|Model|
|---|---|---|
| 1. |Cisco|N3K C3172PQ 10GE|
| 2. |Cisco|N3K-C3048-ZM-F|
| 3. |Cisco|N5K C5596UP DI|

## <a name="list-of-devices-tested-in-the-field"></a>Popis uređaja testirati u polju

Ova sekcija sadrži popis uređaje koje ste uspješno uveden u polju po StorSimple klijentima. To nije provjereno Microsoft, ali vjerojatno će raditi s uređajem StorSimple.
 
| Parametar                         | Vrijednost                                    |
|-----------------------------------|------------------------------------------|
| Provjerite parametar                     | Tvrtke Juniper                                  |
| Promjena modela                    | ex4550 32F                               |
| Verzija operacijskog sustava za promjenu | JunOS 12.3R9.4                           |
| Model plohu                     | Priključci se (PIC 0)                    |
| Provjerite primopredajnik                  | Tvrtke Juniper                                  |
| Primopredajnik modela               | Broj dijela 740 021308 <br></br> Broj dijela 740 030658                   |
| Verzija primopredajnik opreme    | Rev 01 verziju 0.0 (prijavljenih)            |
| Model kabela                     | Obostrano ga uključiti LC-LC 50/125µ, OM3, LSZH |
| StorSimple model                | 8600                                     |
| Verzija StorSimple softvera     | 6.3.9600.17491                           |


## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>Popis uređaja testirati davatelj OEM (Mellanox)  

Mellanox provjerio sljedeće small obrasca dvofaktorska analiza varijance uključiv (SFP) transceivers, kabela i parametri da biste bili sigurni da funkcioniraju optimalnog s Mellanox mreži sučelja kao što su 10 sučelja GbE mrežom na uređaju StorSimple.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kabela i moduli podržava Mellanox 

U sljedećoj su tablici navedeni kabela i moduli podržava Mellanox. To nije provjereno Microsoft, ali vjerojatno će raditi s uređajem StorSimple.

| S. ne. | Brzina | Model                 | Opis                                            | Provjerite                |
|--------|-------|-----------------------|--------------------------------------------------------|-----------------------|
| 1.     | 10 GbE| CAB-SFP-SFP - 1M        | pasivne Bakrene kabela SFP + 10 Gb/s 1m                   | Arista                |
| 2.     | 10 GbE| CAB-SFP-SFP - 2M        | pasivne Bakrene kabel SFP + 10 Gb/s 2m                   | Arista                |
| 3.     | 10 GbE| CAB-SFP-SFP - 3M        | pasivne Bakrene kabel SFP + 10 Gb/s 3m                   | Arista                |
| 4.     | 10 GbE| CAB-SFP-SFP - 5M        | pasivne Bakrene kabel SFP + 10 Gb/s 5m                   | Arista                |
| 5.     | 10 GbE| Cisco SFP H10GBCU1M   | Cisco SFP + kabela                                       | Cisco                 |
| 6.     | 10 GbE| Cisco SFP H10GBCU3M   | Cisco SFP + kabela                                       | Cisco                 |
| 7.     |10 GbE | Cisco SFP H10GBCU5M   | Cisco SFP + kabel                                       | Cisco                 |
| 8.     | 10 GbE| J9281B HP X242 10 G    | SFP + da biste SFP + 1m izravno prilaganje Bakrene kabel             | HP                    |
| 9.     | 10 GbE| 455883 B21 HP BLc     | 10Gb SR SFP + uključivanje                                       | HP                    |
| 10.    | 10 GbE| 455886 B21 HP BLc     | 10Gb LR SFP + uključivanje                                       | HP                    |
| 11.    |10 GbE | 487649 B21 HP BLc     | SFP + m 0,5 10GbE Bakrene kabel                           | HP                    |
| 12.    |10 GbE | 487652 B21 HP BLc     | SFP + 1m 10GbE Bakrene kabela                             | HP                    |
| 13.    |10 GbE | 487655 B21 HP BLc     | SFP + 3m 10GbE Bakrene kabel                             | HP                    |
| 14.    |10 GbE | 487658 B21 HP BLc     | SFP + 7m 10GbE Bakrene kabel                             | HP                    |
| 15.    |10 GbE | 537963 B21 HP BLc     | SFP + 5m 10GbE Bakrene kabela                             | HP                    |
| 16.    |10 GbE | AP784A HP             | 3m C niz pasivni Bakrene SFP + kabela                  | HP                    |
| 17.    |10 GbE | AP785A HP             | 5m C niz pasivni Bakrene SFP + kabel                  | HP                    |
| 18.    |10 GbE | AP818A HP             | 1m B niz aktivni Bakrene SFP + kabela                   | HP                    |
| 19.    |10 GbE | AP819A HP             | 3m B niz aktivni Bakrene SFP + kabel                   | HP                    |
| 20.    |10 GbE | J9150A HP             | X132 10 G SFP + LC SR primopredajnik                        | HP                    |
| 21.    |10 GbE | J9151A HP             | X132 10 G SFP + LC LR primopredajnik                        | HP                    |
| 22.    |10 GbE | J9283B HP             | X242 10 G SFP + SFP + 3 m DAC kabel                        | HP                    |
| 23.    |10 GbE | J9285B HP             | X242 10 G SFP + SFP + 7 m DAC kabel                        | HP                    |
| 24.    | 10 GbE| JD095B HP             | X240 10 G SFP + SFP + 0.65 m DAC kabel                     | HP                    |
| 25.    | 10 GbE| JD096B HP             | X240 10 G SFP + SFP + 1.2 m DAC kabela                      | HP                    |
| 26.    | 10 GbE| JD097B HP             | X240 10 G SFP + SFP + 3 m TATA kabel                        | HP                    |
| 27.    | 10 GbE| MAM1Q00A QSA Mellanox | QSFP SFP + prilagodnik                                   | Mellanox tehnologije |
| 28.    | 10 GbE| MC2309124 006 Mt      | Pasivne Bakrene kabel 1 x SFP+ za QSFP 10 Gb/s 24awg 7 m   | Mellanox tehnologije |
| 29.    | 10 GbE| MC2309124 007 Mt      | Pasivne Bakrene kabel 1 x SFP+ za QSFP 10 Gb/s 24awg 7 m   | Mellanox tehnologije |
| 30.    | 10 GbE| MC2309130 003 Mt      | Pasivne Bakrene kabela 1 x SFP+ za 30awg QSFP 10 Gb/s 3 m   | Mellanox tehnologije |
| 31.    | 10 GbE| MC2309130 00A Mt      | Pasivne Bakrene kabel 1 x SFP+ za QSFP 10 Gb/s 30awg 0,5 m | Mellanox tehnologije |
| 32.    | 10 GbE| MC3309124 005 Mt      | Pasivne Bakrene kabelski 1 24awg 10 Gb/s x SFP+ 5 m           | Mellanox tehnologije |
| 33.    | 10 GbE| MC3309124 007 Mt      | Pasivne Bakrene kabelski 1 24awg 10 Gb/s x SFP+ 7 m           | Mellanox tehnologije |
| 34.    | 10 GbE| MC3309130 003 Mt      | Pasivne Bakrene kabelski 1 30awg x SFP+ 10 Gb/s 3 m           | Mellanox tehnologije |
| 35.    | 10 GbE| MC3309130 00A Mt      | Pasivne Bakrene kabelski 1 30awg 10 Gb/s x SFP+ 0,5 m         | Mellanox tehnologije |

### <a name="switches-supported-by-mellanox"></a>Parametri podržava Mellanox 

U sljedećoj su tablici navedeni parametri podržava Mellanox. To nije provjereno Microsoft, ali vjerojatno će raditi s uređajem StorSimple.

| S. ne. | Brzina | Model | Opis                                                         | Provjerite              |
|--------|-------|-------------|---------------------------------------------------------------------|-------------|
| 1.     | 10GbE | 516733 B21  | HP ProCurve 6120XG 10GbE Ethernet plohu promjenu                      | HP    |
| 2.     | 10GbE | 538113 B21  | HP 10GbE prolazni modul (PTM)                                  | HP    |
| 3.     | 10GbE | EN4093      | IBM PureFlex sustava tkanina EN4093 10 gigabita skalabilni promjenu modul | IBM   |
| 4.     | 1GbE  | 3020        | Cisco Catalyst 3020 1GbE promjenu plohu                               | Cisco |
| 5.     | 1GbE  | 3020 X       | Cisco Catalyst 3020 X 1GbE promjenu plohu                              | Cisco |
| 6.     | 1GbE  | 438030 B21  | Promjena modul 1GbE HP - GbE2c sloja 2-3 Ethernet plohu prijelaz       | HP    |
| 7.     | 1GbE  | 6120G       | HP ProCurve 6120G/XG 1GbE promjenu plohu                              | HP    |

## <a name="next-steps"></a>Daljnji koraci

[Saznajte više o StorSimple hardverske komponente i status](storsimple-monitor-hardware-status.md).
