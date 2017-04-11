<properties 
    pageTitle="StorSimple nadzor pokazatelja | Microsoft Azure" 
    description="Opisuje svijetlo – emitting diodes (LEDs) i zvučna alarmi koji se koristi za praćenje statusa StorSimple uređaja."
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
    ms.date="08/18/2016"
    ms.author="alkohli" />

# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>Upravljanje uređaju pomoću StorSimple nadzor pokazatelja   

## <a name="overview"></a>Pregled

Vaš uređaj StorSimple obuhvaća svijetlo – emitting diodes (LEDs) i alarmi koje možete koristiti za praćenje moduli i ukupnog statusa StorSimple uređaja. Nadzor pokazatelja možete pronaći na hardverske komponente primarni s prilozima uređaja i s prilozima EBOD. Nadzor pokazatelja može biti LEDs ili zvučna alarmi.

Postoje tri stanja LED koji se koristi za označavanje stanja modula: zelena Bljeskave zeleno crveno amber ili crveno amber.  

- Zelena LEDs predstavljaju dobar radno stanje.  
- Bljeskave zelene do crveno amber LEDs predstavljaju prisutnosti rješavaju uvjete koje možda će biti potrebno intervencije korisnika.  
- Crvena amber LEDs upućuju ključnih kvara izlaganje unutar modula.  

Ostatak u ovom se članku opisuju na razne nadzora pokazatelj LEDs, njihovo mjesto na uređaju StorSimple status uređaja na temelju stanja LED i sve povezane zvučna alarmi.

## <a name="front-panel-indicator-leds"></a>Prednja ploča pokazatelj LEDs

Prednja ploča, poznata i kao *operacije ploču* ili *ops ploča*prikazuje status zbrajanja sve module u sustavu. Prednja ploča identičan na primarni StorSimple i s prilozima EBOD, a dolje prikazanom.  

   ![Prednja ploča uređaja][1]
 
Prednja ploča sadrži sljedeće pokazatelj:  

1. Isključi zvuk
2. Pokazatelj Power LED (crveno/zeleno – amber)
3. Pokazatelj kvara modul VODSTVOM (na crveno-amber/ISKLJUČENO)
4. Pokazatelj logičke kvara VODSTVOM (na crveno-amber/ISKLJUČENO
5. Prikaz ID jedinica  

Glavna razlika između Prednja ploča LEDs uređaja i one za s prilozima EBOD je **Sustav jedinica identifikacijski broj** koji se prikazuje na njegovu LED zaslonu. Zadani ID jedinice prikazan na uređaju se **00**, dok je zadani ID jedinice prikazuje na s prilozima EBOD **01**. Omogućuje brzo razlikovanje uređaja i s prilozima EBOD kada je uključena uređaj. Ako vaš uređaj isključena, pomoću informacija navedenih u [Uključivanje na novi uređaj](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) razlikovati uređaj na s prilozima EBOD.  

## <a name="front-panel-led-status"></a>Prednja ploča stanja LED  

U sljedećoj su tablici koristite da biste odredili status označenu LEDs na Prednja ploča za uređaj ili s prilozima EBOD.  

|Power sustava | Modul kvara | Logička kvara | Zajedničko korištenje | Status|
|-------------|---------------|-----------------|-------|-------|
|Crvena amber | ISKLJUČIVANJE     | ISKLJUČIVANJE | N/D | Snaga izgubili, odnose na sigurnosne kopije power ili struje na i kontrolera modula uklonjeni.|
|Zelene boje | UKLJUČENO | UKLJUČENO | N/D | OPS ploča uključeno (5s) Provjera stanja|
|Zelene boje | ISKLJUČIVANJE | ISKLJUČIVANJE | N/D | Uključeno, dobro sve funkcije|
|Zelene boje | UKLJUČENO |N/D | Uo kvara LEDs, Lepeza kvara LEDs | Bilo koji uo kvara, Lepeza kvara iznad ili ispod temperatura|
| Zelene boje | UKLJUČENO | N/D | Modul/i LEDs  | Bilo koji kontroler modula kvara|
| Zelene boje | UKLJUČENO | N/D | N/D | Kvara logike s prilozima|
| Zelene boje | Flash | N/D | Modul statusa LED kontroler modul. Uo kvara LEDs, Lepeza kvara LEDs | Vrstu modula nepoznatim kontroler instaliran, I2C bus pogrešku, pogreška u konfiguraciji kontroler modul ključni proizvoda podataka (VPD) |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Power nikakvo modul (uo) pokazatelj LEDs   

Potencija nikakvo modul (uo) pokazatelj LEDs pronaći ćete na stražnjoj strani primarni s prilozima ili s prilozima EBOD na svakom modulu uo. U ovoj se temi objašnjava kako koristiti sljedeće LEDs za praćenje stanja uređaju StorSimple.  

- Uo LEDs za primarni s prilozima
- Uo LEDs za s prilozima EBOD

## <a name="pcm-leds-for-the-primary-enclosure"></a>Uo LEDs za primarni s prilozima  

Uređaj StorSimple ima modula uo 764W s dodatnim baterija. Sljedeća ilustracija prikazuje ploču LED za uređaj.  

   ![Uo LEDs na primarni s prilozima][2]

Legende LED:

1. AC struje
2. Pogreška Lepeza
3. Kvara baterija
4. U REDU UO
5. Pogreška Kontroler
6. Dobar baterija  

Status u uo je naznačeno u oknu za LED. Na ploči uo LED uređaj ima šest LEDs. Četiri te LEDs Prikaz statusa napajanjem i na Lepeza. Preostali dva LEDs označavanje stanja sigurnosne kopije baterija modula u na uo. Da biste odredili status na uo možete koristiti u tablicama u nastavku.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>Pokazatelj uo LEDs napajanja i Lepeza
| Status | U redu uo (zelena) | Nije uspjelo AC (amber) | Nije uspjelo Lepeza (amber) | Nije uspjelo Kontroler (amber) |
|--------|----------------|-----------------------|------------------|----------------------|
| Nema snaga (da biste s prilozima) | ISKLJUČIVANJE | ISKLJUČIVANJE | ISKLJUČIVANJE | ISKLJUČIVANJE|
| Nema struje (samo u ovom uo) | ISKLJUČIVANJE | UKLJUČENO | ISKLJUČIVANJE | UKLJUČENO |
| AC izlaganje Uključeno uo - u redu     | UKLJUČENO | ISKLJUČIVANJE | ISKLJUČIVANJE | ISKLJUČIVANJE |
| Nije uspjelo uo (Lepeza Neuspjelo) | ISKLJUČIVANJE | ISKLJUČIVANJE | UKLJUČENO | N/D |
| Uo kvara (iznad amp putem napona iznad trenutnog) | ISKLJUČIVANJE | UKLJUČENO | UKLJUČENO | UKLJUČENO |
| Uo (Lepeza iz odstupanje) | UKLJUČENO | ISKLJUČIVANJE | ISKLJUČIVANJE | UKLJUČENO |
| Čekanja načinu rada | Bljeskave | ISKLJUČIVANJE | ISKLJUČIVANJE | ISKLJUČIVANJE |
| Preuzimanje uo opreme | ISKLJUČIVANJE | Bljeskave | Bljeskave | Bljeskave |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>Pokazatelj uo LEDs za sigurnosne kopije baterija  

| Status | Baterija dobar (zelena) | Baterija kvara (amber) |
|--------|----------------------|-----------------------|
| Ne postoji baterija | ISKLJUČIVANJE | ISKLJUČIVANJE |
| Baterija izlaganje i naplaćena | UKLJUČENO | ISKLJUČIVANJE |
| Puni ili održavanje rashodovanja baterija | Bljeskave | ISKLJUČIVANJE |
| Baterija "meki" kvara (oporaviti) | ISKLJUČIVANJE | Bljeskave |
| Baterija "teško" kvara (koji nisu-oporaviti) | ISKLJUČIVANJE | UKLJUČENO |
| Disarmed baterija | Bljeskave | ISKLJUČIVANJE |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>Uo LEDs za s prilozima EBOD  

S prilozima EBOD ima 580W uo i baterija dodatne. Na ploči uo za s prilozima EBOD sadrži pokazatelj LEDs samo za power zalihama i na Lepeza. Sljedeća ilustracija prikazuje te LEDs.

   ![Uo LEDs na s prilozima EBOD][3] 
 
U sljedećoj su tablici možete koristiti za određivanje stanja u uo.  

| Status | U redu uo (zelena) | Nije uspjelo AC (amber) | Nije uspjelo Lepeza (amber) | Nije uspjelo Kontroler (amber) |
|--------|---------------|------------------------|------------------|----------------------|
| Nema snaga (da biste s prilozima) | ISKLJUČIVANJE | ISKLJUČIVANJE | ISKLJUČIVANJE | ISKLJUČIVANJE |
| Nema struje (samo u ovom uo) | ISKLJUČIVANJE | UKLJUČENO | ISKLJUČIVANJE | UKLJUČENO |
| AC izlaganje Uključeno uo – u redu | UKLJUČENO | ISKLJUČIVANJE | ISKLJUČIVANJE | ISKLJUČIVANJE |
| Nije uspjelo uo (Lepeza Neuspjelo) | ISKLJUČIVANJE | ISKLJUČIVANJE | UKLJUČENO | X |
| Uo kvara (iznad amp putem napona iznad trenutne | ISKLJUČIVANJE | UKLJUČENO | UKLJUČENO | UKLJUČENO |
| Uo (Lepeza iz odstupanje) | UKLJUČENO | ISKLJUČIVANJE | ISKLJUČIVANJE | UKLJUČENO |
| Čekanja modela | Bljeskave | ISKLJUČIVANJE | ISKLJUČIVANJE | ISKLJUČIVANJE |
| Preuzimanje uo opreme | ISKLJUČIVANJE | Bljeskave | Bljeskave | Bljeskave |

## <a name="controller-module-indicator-leds"></a>Pokazatelj kontroler modul LEDs  

Uređaj StorSimple sadrži LEDs za primarni kontroler i moduli kontroler EBOD.   

### <a name="monitoring-leds-for-the-primary-controller"></a>Nadzor LEDs za primarni kontroler
Na sljedećoj slici olakšava prepoznali LEDs na primarni kontroler. (Sve komponente navedene su da biste olakšali usmjerenje.)  

   ![Nadzor LEDs - primarni kontroler][4]
 
U sljedećoj su tablici koristite da biste odredili hoće li modul kontroler radi pravilno.  

### <a name="controller-indicator-leds"></a>Pokazatelj kontroler LEDs  

| LED | Opis                                                                            
|---- | ----------- |
| ID LED (plava) | Upućuje na to da je u tijeku modula označena. Ako je plavom LED titranje na tekući kontroleru, zatim kontrolerom je aktivna kontroler i drugu kontrolerom čekanja. Dodatne informacije potražite u članku [otkrivanje kontrolerom aktivni na uređaju](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Kvara LED (amber) | Označava kvara u kontrolerom.        
| U redu-LED (zelena) | Zelena stižu označava da je kontrolerom u redu. Bljeskave zelena označava kontroler VPD konfiguracijska pogreška. |
| SAS aktivnosti LEDs (zelena) | Zelena stižu označava veze sa nema trenutne aktivnosti. Bljeskave zelena pokazuje veze sadrži tijeku aktivnosti. |
| Ethernet status LEDs | Desnoj strani označava aktivnosti vezu/mreže: (zelena stižu) vezu active (Bljeskave zeleno) mreže aktivnosti. Lijeva strana označava brzini mreže: (žuto) 1000 Mb/s "," (zelena) 100 Mb/s "i" (ISKLJUČENO) 10 Mb/s. Ovisno o modelu komponente ovaj svijetlo možda titranja čak i ako nije omogućena mrežnog sučelja. |
| OBJAVA LEDs | Pokretanje tijeka označava kada je uključena kontrolerom. Ako StorSimple uređaja ne uspije pokrenuti, ova LED pomoći će Microsoft Support prepoznavanje točke u postupku pokretanja pojavila se pogreška. |

>[AZURE.IMPORTANT] 
Ako je osvijetljen kvara LED, postoji problem s kontroler modul u koji se može riješiti ponovnim pokretanjem kontrolerom. Obratite se Microsoft Support ako ponovnim kontrolerom riješili taj problem.  


### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>Nadzor LEDs za EBOD (s prilozima EBOD)  

Svaki od 6 Gb/s SAS EBOD kontrolera ima LEDs koje označavaju njezin status, kao što je prikazano na sljedećoj slici.  

  ![Nadzor LEDs - s EBOD prilozima][5]

U sljedećoj su tablici koristite da biste odredili radi li kontroler modul EBOD obično.  

### <a name="ebod-controller-module-indicator-leds"></a>Pokazatelj modul EBOD kontroler LEDs  

|Status | Modul/i u redu (zelena) | / I modula kvara (amber) | Glavno računalo priključak aktivnosti (zelena) |
|-------|----------------------|-------------------------------|----------------------------|
| Modul za kontroler u redu | UKLJUČENO | ISKLJUČIVANJE | - |
| Kontroler modula kvara | ISKLJUČIVANJE | UKLJUČENO | - |
| Nema priključak veze s vanjskim glavno računalo | - | - | ISKLJUČIVANJE |
| Glavno računalo za vanjske veze priključak – bez aktivnosti | - | - | UKLJUČENO |
| Glavno računalo za vanjski priključak veze - aktivnosti | - | - | Bljeskave |
| Kontroler modul metapodataka pogreške | Bljeskave | - | - |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Pokazatelj pogon diska LEDs za primarni s prilozima i s EBOD prilozima

Uređaj StorSimple ima diskovnih pogona koja se nalazi u primarni s prilozima i s prilozima EBOD. Svakom disku sadrži nadzor pokazatelj LEDs, kao što je opisano u ovom odjeljku. 

Za diskovnih pogona status pogon označena je u obliku zelenog LED i crveno amber LED postavljen na prednju stranu svakom modulu prijenosni disk. Sljedeća ilustracija prikazuje te LEDs.

  ![LEDs pogon diska][6]
 
Određivanje stanja svaki tvrdi disk koji je shodno utječe na ukupni Prednja ploča stanja LED pomoću tablici u nastavku.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>Pokazatelj pogon diska LEDs s prilozima EBOD  

| Status | Aktivnosti u redu LED (zelena) | Kvara LED (crveno-amber) | Povezanu ploču ops LED |
|-------|--------------------------|----------------------|-------------------------|
| Nema pogon instaliran | ISKLJUČIVANJE | ISKLJUČIVANJE | Ništa |
| Pogon instalirana i radu sa servisom | Uključivanje/isključivanje svjetlucanje s aktivnosti | X | Ništa |
| Skup za identitet uređaju SCSI s prilozima Services (SES) | UKLJUČENO | Bljeskave 1 sekunde na/1 drugi isključeno | Ništa |
| SES uređaj kvara bit postavljen | UKLJUČENO | UKLJUČENO | Logička kvara (crveni) |
| Elektronička struje kontrola | ISKLJUČIVANJE | UKLJUČENO | Modul kvara (crveni) |

## <a name="audible-alarms"></a>Zvučna alarmi  

Na uređaju StorSimple sadrži zvučna alarmi pridružene primarni s prilozima i s prilozima EBOD. Zvučna alarma nalazi se na prednju ploča (poznat i kao ploča ops) i priloge. Zvučna alarma pokazuje izlaganje kvara uvjeta. Sljedeći uvjeti će aktivirati zajedničko korištenje:  

- Lepeza kvara ili pogreške
- Napona izvan raspona
- Iznad ili ispod temperatura uvjet
- Termalnim preljev
- Kvara sustava
- Logička kvara
- Power opskrbu kvara
- Uklanjanje power hlađenje modul (uo)  

U sljedećoj tablici opisane različite države alarma.  

### <a name="alarm-states"></a>Država alarma  

| Stanje alarma | Akcija | Akcija s gumbom za isključivanje zvuka pritiska na tipku |
|------------|---------|---------------------------------|
| S0 | Normalni način rada: tihu | Dvaput Beep |
| S1 | Način kvara: 1 sekunde na/1 drugi isključeno | Prijelaz na S2 ili S3 (Vidi bilješke) |
| S2 | Podsjetite načinu rada: Povremeni zvučni signal | Ništa |
| S3 | Isključenog načinu: tihu | Ništa |
| S4 | Način ključnih kvara: neprekinuti alarma | Nije dostupno: nije aktivna Isključi zvuk |

> [AZURE.NOTE] 

>  - U stanju alarma S1, ako ne pritisnete isključivanje unutar dvije minute stanje automatski prijelaze S2 ili S3.  
>  - Država alarma S1 da biste S4 S0 nakon vratili uvjet kvara isključen.  
>  - Stanje ključnih kvara S4 se može unijeti iz drugih stanja.  

Pritiskom na gumb Isključi zvuk na ploči ops možete isključiti zvuk zvučna alarma. Isključivanje automatskog će se dogoditi nakon dvije minute ako parametar isključivanje ručno ne radi. Kada je isključen zajedničko korištenje, on će nastaviti zvuka s kratki Povremeni signali da biste naznačili da i dalje postoji problem. Zajedničko korištenje bit će tihu kad ste poništili sve probleme.  

U sljedećoj tablici opisane različite alarma uvjeta.  

### <a name="alarm-conditions"></a>Alarma uvjeta  

| Status | Težinu | Zajedničko korištenje | OPS ploče LED |
|--------|---------|--------|----------------|
| Upozorenje o uo – gubitak Kontroler power iz jedne uo | Kvara – bez gubitka zalihosti | S1 | Modul kvara|
|Upozorenje o uo – gubitak Kontroler power iz jedne uo | Kvara – gubitak zalihosti | S1 | Modul kvara |
| Nije uspjelo Lepeza uo | Kvara – gubitak zalihosti | S1 | Modul kvara |
| Modul SBB prepoznat uo kvara | Kvara | S1 | Modul kvara |
| Uklanja uo | Pogreška u konfiguraciji | Ništa | Modul kvara |
| Pogreška u konfiguraciji s prilozima | Kvara – od ključne važnosti | S1 | Modul kvara |
| Niska upozorenje temperatura | Upozorenje | S1 | Modul kvara |
| Visoke upozorenje temperatura | Upozorenje | S1 | Modul kvara |
| Putem alarma temperatura | Kvara – od ključne važnosti | S1 | Modul kvara |
| Pogreška bus I2C | Kvara – gubitak zalihosti | S1 | Modul kvara |
| OPS ploče komunikacije pogreške (I2C) | Kvara – od ključne važnosti     | S1 | Modul kvara |
| Kontroler pogreške | Kvara – od ključne važnosti | S1 | Modul kvara |
| SBB sučelja modula kvara | Kvara – od ključne važnosti | S1 | Modul kvara |
| Modul kvara sučelja SBB – ne funkcionira moduli preostalo | Kvara – od ključne važnosti | S4 | Modul kvara |
| SBB sučelja modul ukloniti | Upozorenje | Ništa | Modul kvara |
| Pogon power kontrola kvara | Upozorenje – bez gubitka power pogon | S1 | Modul kvara |
| Pogon power kontrola kvara | Kvara – ključnih; gubitak power pogon | S1 | Modul kvara |
| Pogon uklonjen | Upozorenje | Ništa | Modul kvara |
| Nedovoljna power dostupna | Upozorenje | Ništa | Modul kvara |

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [StorSimple hardverske komponente i status](storsimple-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png

 
