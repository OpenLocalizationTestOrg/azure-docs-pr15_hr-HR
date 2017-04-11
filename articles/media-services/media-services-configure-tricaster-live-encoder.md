<properties 
    pageTitle="Konfiguriranje encoder NewTek TriCaster da biste poslali jedan brzina prijenosa aktivno strujanje | Microsoft Azure" 
    description="U ovoj se temi objašnjava konfiguriranje Tricaster uživo encoder da biste poslali jedan brzina prijenosa strujanje AMS kanala koje su omogućene za šifriranje uživo." 
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
    ms.author="juliako;cenkd;anilmur"/>

#<a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>Korištenje encoder NewTek TriCaster da biste poslali aktivno strujanje jedan brzina prijenosa

> [AZURE.SELECTOR]
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Elemental uživo](media-services-configure-elemental-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

U ovoj se temi objašnjava konfiguriranje encoder uživo [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) da biste poslali jedan brzina prijenosa strujanje AMS kanala koje su omogućene za šifriranje uživo. Dodatne informacije potražite u članku [Rad s kanala koji su omogućeni izvođenje Live šifriranje pomoću servisa Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Pomoću ovog praktičnog vodiča prikazuje kako upravljati Azure Media Services (AMS) pomoću alata za Azure Media Services Explorer (AMSE). Ovaj alat izvodi samo na Računalu sa sustavom Windows. Ako ste na računalu Mac ili Linux, pomoću portala za Azure da biste stvorili [kanala](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) i [programe](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).

>[AZURE.NOTE]Prilikom korištenja Tricaster za slanje u doprinos sažetak sadržaja AMS kanala koje su omogućene za šifriranje uživo, može biti problemčiće video i audiozapisa u uživo događaj ako koristite određene značajke Tricaster, kao što su brzi izrezivanje između sažetke sadržaja ili prijelaz iz slates. U tim AMS radi na rješavanje problema s tek onda, ga se ne preporučujemo da biste koristili te značajke.


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

![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Navedite naziv kanala, opis polje nije obavezno. U odjeljku postavkama kanala, odaberite **standardnu** za mogućnost Live kodiranje s protokolom unos postavljena na **RTMP**. Možete ostaviti sve ostale postavke kao što je.


Provjerite je li **pokretanje novog kanala sada** je uključen.

3. Kliknite **Stvaranje kanala**.
![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

>[AZURE.NOTE] Kanal može potrajati 20 minuta da biste pokrenuli.


Dok se pokreće kanal možete [konfigurirati kodiranje](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

>[AZURE.IMPORTANT] Imajte na umu da naplata počinje čim kanala ulazi u stanju spremno. Dodatne informacije potražite u članku [kanal, stanja](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_tricaster_rtmp></a>Konfiguriranje NewTek TriCaster encoder

Pomoću ovog praktičnog vodiča koriste sljedeće postavke izlaza. Ostatak u ovom se odjeljku opisuju navedeni koraci za konfiguraciju u više detalja. 

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

1. Stvorite novi projekt **NewTek TriCaster** ovisno o tome što izvor videozapisa unos se koristi. 
2. Jednom unutar tog projekta pronaći gumb **strujanje** pa kliknite ikonu zupčanika pokraj njega da biste pristupili izborniku konfiguracije strujanje.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Kada se otvorio izbornik, pod naslovom veze kliknite **Novo** . Kada se pojavi upit za vrstu veze, odaberite **Adobe Flash**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)

4. Kliknite **u redu**.

5. Profil programa FMLE može uvesti sada kliknite padajuću strelicu u odjeljku **Strujanje profil** i navigacija da biste **pregledali**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)

6. Otiđite na kojem je spremljena konfiguriranog profila FMLE.
7. Odaberite ga, a zatim pritisnite **u redu**.

    Profil se nakon prijenosa, prijeđite na sljedeći korak.

6. Da biste dodijelili Tricaster **RTMP krajnje**se unos URL-a na kanal.
    
    Vratite se na alat za AMSE pa provjerite na stanje dovršenja kanala. Kada je stanje promijenio iz **Početni** **pokrenutim**, možete dobiti unos URL-a.
      
    Kada je pokrenut kanal, desnom tipkom miša kliknite naziv kanala, kretanje dolje postavite pokazivač miša iznad **URL unos Kopiraj u međuspremnik** i zatim odaberite **Primarni unos URL-a**.  
    
    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)

7. Zalijepite informacije u polju **mjesto** u odjeljku **Poslužitelj za Flash** u projektu Tricaster. Dodijelite naziv strujanja u polju **ID strujanje** . 

    Ako informacije strujanje dodan FMLE profila, ga možete i uvezeni u ovom se odjeljku tako da kliknete **Postavki uvoza**, Navigacija spremljeni FMLE profil i kliknite **u redu**. Podatke iz FMLE treba popuniti odgovarajuća polja Flash poslužitelja.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)

9. Kada završite, kliknite **u redu** pri dnu zaslona. Kada videosadržaj i audiosadržaj unose u na Tricaster spremni, počnite strujanja na AMS tako da kliknete gumb **strujanje** .

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

>[AZURE.IMPORTANT]Prije nego što kliknete **strujanje**, najprije **morate** provjerite je li kanal spreman. 
>Osim toga, provjerite ne da biste napustili kanal u stanju spremno bez unosa doprinos koji se sažetak sadržaja dulje od > 15 minuta. 

##<a name="test-playback"></a>Reprodukcija test
  
1. Otvorite alat za AMSE pa desnom tipkom miša kliknite kanala kojim želite testirati. Na izborniku postavite pokazivač **reprodukcije pretpregleda** , a zatim odaberite **s Azure Media Player**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Ako se pojavi toka reproduktora, zatim kodiranje pravilno konfigurirani za povezivanje s AMS. 

Ako je primili pogreške, kanal moraju se ponovno postaviti i prilagoditi postavke kodiranje. Pogledajte temu [za otklanjanje poteškoća](media-services-troubleshooting-live-streaming.md) smjernicama.  

##<a name="create-a-program"></a>Stvaranje programa

1. Kada se potvrđuje reprodukcije kanal, stvorite programa. Na kartici **Live** u alatu za AMSE, desnom tipkom miša kliknite unutar područja program i odaberite **Stvaranje nove Program**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)

2. Dodijelite naziv program i, ako je potrebno, prilagodite **Arhiva prozor duljine** (koji zadanih vrijednosti 4 sata). Možete odrediti mjesto za pohranu ili ostavite kao zadani.  
3. Potvrdite okvir **pokrenite Program sada** .
4. Kliknite **Stvori Program**.  
  
    Napomena: Stvaranje Program traje manje od stvaranje kanala.    
 
5. Kada je pokrenut program, provjerite reprodukcije desnom tipkom miša kliknite program i navigacija **reprodukcije programe** i odabirom **s Azure Media Player**.  
6. Kada se uvjerite, ponovno desnom tipkom miša kliknite program i odaberite **kopirajte URL Izlaz u međuspremnik** (ili pronaći te podatke iz **programa informacije i postavke** mogućnosti na izborniku). 

Strujanje je sada moguće ugrađen u čitač ili distribucije pred publikom uživo prikaz.  


## <a name="troubleshooting"></a>Otklanjanje poteškoća

Pogledajte temu [za otklanjanje poteškoća](media-services-troubleshooting-live-streaming.md) smjernicama. 


##<a name="next-step"></a>Sljedeći korak

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
