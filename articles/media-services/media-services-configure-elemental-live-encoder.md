<properties 
    pageTitle="Konfiguriranje encoder Elemental Live poslati u jednom brzina prijenosa uživo strujanje | Microsoft Azure" 
    description="U ovoj se temi objašnjava konfiguriranje encoder Elemental Live da biste poslali jedan brzina prijenosa strujanje AMS kanala koje su omogućene za šifriranje uživo." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="cenkdin;anilmur;juliako"/>

#<a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a>Slanje aktivno strujanje jedan brzina prijenosa pomoću encoder Elemental Live

> [AZURE.SELECTOR]
- [Elemental uživo](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

U ovoj se temi objašnjava konfiguriranje encoder [Elemental Live](http://www.elementaltechnologies.com/products/elemental-live) da biste poslali jedan brzina prijenosa strujanje AMS kanala koje su omogućene za šifriranje uživo.  Dodatne informacije potražite u članku [Rad s kanala koji su omogućeni izvođenje Live šifriranje pomoću servisa Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Pomoću ovog praktičnog vodiča prikazuje kako upravljati Azure Media Services (AMS) pomoću alata za Azure Media Services Explorer (AMSE). Ovaj alat izvodi samo na Računalu sa sustavom Windows. Ako ste na računalu Mac ili Linux, pomoću portala za Azure da biste stvorili [kanala](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) i [programe](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).

##<a name="prerequisites"></a>Preduvjeti

- Morate raditi znanja korištenja Elemental Live web-sučelja da biste stvorili uživo događaja.
- [Stvorite račun servisa Azure Media Services](media-services-portal-create-account.md)
- Provjerite postoji strujanje krajnje raditi s najmanje jednu strujanje jedinicu dodijeliti. Dodatne informacije potražite u članku [Upravljanje strujanje točke u računa za servise medijske sadržaje](media-services-portal-manage-streaming-endpoints.md).
- Instalirajte najnoviju verziju alata [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Pokretanje alata i povezivanje s računom AMS.

##<a name="tips"></a>Savjeti

- Kad god je to moguće, koristite ožičenoj internetsku vezu.
- Na dobro pravilo palca prilikom određivanja uvjeta propusnosti je dvaput strujanje bitrates. Dok je to nije obavezno zahtjev, to će pomoći prevladavanje utjecaj zagušenja mreže.
- Prilikom korištenja softvera temelji koderi, zatvorite out sve nepotrebne programe.

## <a name="elemental-live-with-rtp-ingest"></a>Elemental Live s RTP ingest

U ovom se odjeljku objašnjava konfiguriranje Elemental Live kodiranje koje šalje jedan brzina prijenosa aktivno strujanje putem RTP.  Dodatne informacije potražite u članku [MPEG-TS strujanje putem RTP](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Stvaranje kanala

1.  U alatu za AMSE idite na karticu **Live** pa desnom tipkom miša kliknite unutar područja kanala. Odabir **kanala za stvaranje...** na izborniku.

![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Navedite naziv kanala, opis polje nije obavezno. U odjeljku postavke kanala, odaberite **standardnu** za mogućnost Live kodiranje s protokolom unos postavljena na **RTP (MPEG-TS)**. Možete ostaviti sve ostale postavke kao što je.


Provjerite je li **pokretanje novog kanala sada** je uključen.

3. Kliknite **Stvaranje kanala**.
![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

>[AZURE.NOTE] Kanal može potrajati 20 minuta da biste pokrenuli.

Dok se pokreće kanal možete [konfigurirati kodiranje](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

>[AZURE.IMPORTANT] Imajte na umu da naplata počinje čim kanala prelazi u stanje spreman. Dodatne informacije potražite u članku [kanal, stanja](media-services-manage-live-encoder-enabled-channels.md#states).

###<a id=configure_elemental_rtp></a>Konfiguriranje encoder Elemental Live 

U ovom ćete praktičnom vodiču koriste sljedeće postavke izlaza. Ostatak u ovom se odjeljku opisuju navedeni koraci za konfiguraciju u više detalja. 

**Videozapis**:
 
- Kodek: H.264 
- Profil: Najviša (razinu 4.0) 
- Brzina prijenosa: 5000 kb/s 
- Keyframe: dvije sekunde (60 sekundi) 
- Okvir stopa: 30
 
**Zvuka**:

- Kodek: AAC (LC) 
- Brzina prijenosa: 192 kb/s 
- Stopa uzorak: 44.1 kHz


####<a name="configuration-steps"></a>Navedeni koraci za konfiguraciju

1. Dođite do web-sučelja **Elemental uživo** i postavljanje encoder **UDP/TS** tok. 

2. Nakon stvaranja novog događaja, pomaknite se do grupe izlazne i dodajte izlazna grupi **UDP/TS** . 

3. Stvorite novi izlaz tako da odaberete **Novi strujanje** , a zatim **Dodajte izlazna**.  
    
    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental13.png)
    
    >[AZURE.NOTE] Preporučuje se da Elemental događaj ima timecode postavljeno na "Sat sustava" da biste lakše encoder ponovno povezali u slučaju pogreške strujanje.

4. Sad kad izlaz je stvoren, kliknite **Dodaj strujanje**. Sada moguće konfigurirati postavke izlaza. 
5. Pomaknite se do odjeljka "Toka 1" koji ste upravo stvorili, kliknite karticu **videozapis** na lijevoj strani, a proširite odjeljak postavke **Dodatno** . 

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Dok Elemental Live ima širokog raspona dostupna prilagodbu, za početak rada s strujanje AMS preporučuje se sljedeće postavke. 
    
    - Razlučivost: 1280 × 720 
    - Frekvenciju okvira: 30 
    - GOP veličina: okviri za 60 
    - Način načinu rada: progresivno 
    - Brzina prijenosa: 5000000 bitne/s (to može se prilagoditi ovisno o ograničenja mreže) 
    

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

6. Pronađite unos URL-a na kanal.
    
    Vratite se na alat za AMSE pa provjerite na stanje dovršenja kanala. Kada je stanje promijenio iz **Početni** **pokrenutim**, možete dobiti unos URL-a.
      
    Kada je pokrenut kanal, desnom tipkom miša kliknite naziv kanala, kretanje dolje postavite pokazivač miša iznad **URL unos Kopiraj u međuspremnik** i zatim odaberite **Primarni unos URL-a**.  
    
    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
    
1. Zalijepite te podatke u polju **Primarnog odredište** na Elemental. Sve ostale postavke može ostati zadani.
    
    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Za dodatne zalihosti ponovite ove sekundarne unos URL-om stvaranjem zasebnoj kartici "Izlaz" za strujanje UDP/TS.
    
7. Kliknite **Stvori** (Ako je stvoren je novi događaj) ili **Ažuriranje** (Ako je uređivanje postojećih događaja), a zatim nastavite da biste pokrenuli kodiranje. 

>[AZURE.IMPORTANT]Prije nego što kliknete **pokretanje** na web-sučelja Elemental uživo, najprije **morate** provjerite je li kanal spreman. 
>Osim toga, provjerite ne da biste napustili kanal u stanju spremno bez događaja dulje od > 15 minuta.

Nakon toka pokrenuta 30 sekundi, idite natrag AMSE test i alat za reprodukciju.  

###<a name="test-playback"></a>Reprodukcija test
  
1. Otvorite alat za AMSE pa desnom tipkom miša kliknite kanala kojim želite testirati. Na izborniku postavite pokazivač **reprodukcije pretpregleda** , a zatim odaberite **s Azure Media Player**.  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Ako se pojavi toka reproduktora, zatim kodiranje pravilno konfigurirani za povezivanje s AMS. 

Ako je primili pogreške, kanal moraju se ponovno postaviti i prilagoditi postavke kodiranje. Pogledajte temu [za otklanjanje poteškoća](media-services-troubleshooting-live-streaming.md) smjernicama.   

###<a name="create-a-program"></a>Stvaranje programa

1. Kada se potvrđuje reprodukcije kanal, stvorite programa. Na kartici **Live** u alatu za AMSE, desnom tipkom miša kliknite unutar područja program i odaberite **Stvaranje nove Program**.  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental9.png)

2. Dodijelite naziv program i, ako je potrebno, prilagodite **Arhiva prozor duljine** (koji zadanih vrijednosti 4 sata). Možete odrediti mjesto za pohranu ili ostavite kao zadani.  
3. Potvrdite okvir **pokrenite Program sada** .
4. Kliknite **Stvori Program**.  
  
    Napomena: Stvaranje Program traje manje od stvaranje kanala.    
 
5. Kada je pokrenut program, provjerite reprodukcije desnom tipkom miša kliknite program i navigacija **reprodukcije programe** i odabirom **s Azure Media Player**.  
6. Kada se uvjerite, ponovno desnom tipkom miša kliknite program i odaberite **kopirajte URL Izlaz u međuspremnik** (ili pronaći te podatke iz **programa informacije i postavke** mogućnosti na izborniku). 

Strujanje je sada ugrađen u čitač ili distribuira pred publikom uživo prikaz.  

## <a name="troubleshooting"></a>Otklanjanje poteškoća

Pogledajte temu [za otklanjanje poteškoća](media-services-troubleshooting-live-streaming.md) smjernicama. 


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
