<properties 
    pageTitle="Konfiguriranje encoder FMLE da biste poslali jedan brzina prijenosa aktivno strujanje | Microsoft Azure" 
    description="U ovoj se temi objašnjava konfiguriranje encoder Flash Media Live kodiranje (FMLE) da biste poslali jedan brzina prijenosa strujanje AMS kanala koje su omogućene za šifriranje uživo." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako;cenkdin;anilmur"/>

#<a name="use-the-fmle-encoder-to-send-a-single-bitrate-live-stream"></a>Korištenje encoder FMLE da biste poslali aktivno strujanje jedan brzina prijenosa

> [AZURE.SELECTOR]
- [FMLE](media-services-configure-fmle-live-encoder.md)
- [Elemental uživo](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)

U ovoj se temi objašnjava konfiguriranje encoder [Flash Media Live Encoder](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) da biste poslali jedan brzina prijenosa strujanje AMS kanala koje su omogućene za šifriranje uživo. Dodatne informacije potražite u članku [Rad s kanala koji su omogućeni izvođenje Live šifriranje pomoću servisa Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Pomoću ovog praktičnog vodiča prikazuje kako upravljati Azure Media Services (AMS) pomoću alata za Azure Media Services Explorer (AMSE). Ovaj alat izvodi samo na Računalu sa sustavom Windows. Ako ste na računalu Mac ili Linux, pomoću portala za Azure da biste stvorili [kanala](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) i [programe](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).

Imajte na umu ovog praktičnog vodiča opisuju pomoću AAC. Međutim, FMLE ne podržava AAC prema zadanim postavkama. Ćete morati kupiti dodatak za AAC kodiranje primjerice MainConcept: [dodatak za AAC](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)

##<a name="prerequisites"></a>Preduvjeti

- [Stvorite račun servisa Azure Media Services](media-services-portal-create-account.md)
- Provjerite postoji strujanje krajnje raditi s najmanje jednu strujanje jedinicu dodijeliti. Dodatne informacije potražite u članku [Upravljanje strujanje točke u računa za servise medijske sadržaje](media-services-portal-manage-streaming-endpoints.md)
- Instalirajte najnoviju verziju alata [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Pokretanje alata i povezivanje s računom AMS.

##<a name="tips"></a>Savjeti

- Kad god je to moguće, koristite ožičenoj internetsku vezu.
- Na dobro pravilo palca prilikom određivanja uvjeta propusnosti je dvaput strujanje bitrates. Dok je to nije obavezno zahtjev, to će pomoći prevladavanje utjecaj zagušenja mreže.
- Prilikom korištenja softvera temelji koderi, zatvorite out sve nepotrebne programe.

## <a name="create-a-channel"></a>Stvaranje kanala

1.  U alatu za AMSE idite na karticu **Live** pa desnom tipkom miša kliknite unutar područja kanala. Odabir **kanala za stvaranje...** na izborniku.

![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. Navedite naziv kanala, opis polje nije obavezno. U odjeljku postavkama kanala, odaberite **standardnu** za mogućnost Live kodiranje s protokolom unos postavljena na **RTMP**. Možete ostaviti sve ostale postavke kao što je.


Provjerite je li **pokretanje novog kanala sada** je uključen.

3. Kliknite **Stvaranje kanala**.
![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

>[AZURE.NOTE] Kanal može potrajati 20 minuta da biste pokrenuli.


Dok se pokreće kanal možete [konfigurirati kodiranje](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).

>[AZURE.IMPORTANT] Imajte na umu da naplata počinje čim kanala prelazi u stanje spreman. Dodatne informacije potražite u članku [kanal, stanja](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_fmle_rtmp></a>Konfiguriranje FMLE encoder

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


###<a name="configuration-steps"></a>Navedeni koraci za konfiguraciju

1. Dođite do sučelja Flash Media Live kodiranje korisnika (FMLE) na računalu koristi.

    Sučelje nije jednu glavnu stranicu postavke. Provjerite uzeti u obzir sljedeće preporučenih postavki za početak rada s strujanje pomoću FMLE.
    
    - Oblik: H.264 sekundi: 30.00 
    - Unos veličina: 1280 × 720 
    - Brzinu prijenosa: KB (može se prilagoditi ovisno o ograničenja mreže) 5000/s  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

    Kada pomoću prepleteno izvora, imajte potvrdite mogućnost "Opciju Ukloni preplitanje"

2. Odaberite ikonu ključa pokraj odjeljka oblik, mora biti te dodatne postavke:

    - Profil: glavne
    - Razina: 4.0
    - Učestalost Keyframe: dvije sekunde. 
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)

3. Postavite sljedeće važne postavke zvuka:
    
    - Oblik: AAC 
    - Brzina uzorkovanja: 44100 Hz
    - Brzina prijenosa: 192 kb/s
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)

6. Unos URL-a na kanalu zatražite da bi je dodijeliti u FMLE **RTMP krajnje točke**.
    
    Vratite se na alat za AMSE pa provjerite na stanje dovršenja kanala. Kada je stanje promijenio iz **Početni** **pokrenutim**, možete dobiti unos URL-a.
      
    Kada je pokrenut kanal, desnom tipkom miša kliknite naziv kanala, kretanje dolje postavite pokazivač miša iznad **URL unos Kopiraj u međuspremnik** i zatim odaberite **Primarni unos URL-a**.  
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)

7. Zalijepite te informacije u polje **FMS URL** odjeljka izlazne i dodijelite naziv strujanje. 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    Za dodatne zalihosti, ponovite te korake sekundarne unos URL-om.
8. Odaberite **Poveži**.

>[AZURE.IMPORTANT]Prije nego što kliknete **Povezivanje**, **morate** osigurati spreman kanal. 
>Osim toga, provjerite ne da biste napustili kanal u stanju spremno bez unosa doprinos koji se sažetak sadržaja dulje od > 15 minuta.

##<a name="test-playback"></a>Reprodukcija test
  
1. Otvorite alat za AMSE pa desnom tipkom miša kliknite kanala kojim želite testirati. Na izborniku postavite pokazivač **reprodukcije pretpregleda** , a zatim odaberite **s Azure Media Player**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

Ako se pojavi toka reproduktora, zatim kodiranje pravilno konfigurirani za povezivanje s AMS. 

Ako je primili pogreške, kanal moraju se ponovno postaviti i prilagoditi postavke kodiranje. Pogledajte temu [za otklanjanje poteškoća](media-services-troubleshooting-live-streaming.md) smjernicama.  

##<a name="create-a-program"></a>Stvaranje programa

1. Kada se potvrđuje reprodukcije kanal, stvorite programa. Na kartici **Live** u alatu za AMSE, desnom tipkom miša kliknite unutar područja program i odaberite **Stvaranje nove Program**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)

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
