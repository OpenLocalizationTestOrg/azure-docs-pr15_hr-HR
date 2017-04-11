<properties 
    pageTitle="Konfiguriranje encoder Telestream Wirecast da biste poslali jedan brzina prijenosa aktivno strujanje | Microsoft Azure" 
    description="U ovoj se temi objašnjava konfiguriranje Wirecast uživo encoder da biste poslali jedan brzina prijenosa strujanje AMS kanali koje su omogućene za šifriranje uživo. " 
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

#<a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>Korištenje encoder Wirecast da biste poslali aktivno strujanje jedan brzina prijenosa

> [AZURE.SELECTOR]
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [Elemental uživo](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

U ovoj se temi objašnjava konfiguriranje encoder uživo [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) da biste poslali jedan brzina prijenosa strujanje AMS kanala koje su omogućene za šifriranje uživo.  Dodatne informacije potražite u članku [Rad s kanala koji su omogućeni izvođenje Live šifriranje pomoću servisa Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Pomoću ovog praktičnog vodiča prikazuje kako upravljati Azure Media Services (AMS) pomoću alata za Azure Media Services Explorer (AMSE). Ovaj alat izvodi samo na Računalu sa sustavom Windows. Ako ste na računalu Mac ili Linux, pomoću portala za Azure da biste stvorili [kanala](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) i [programe](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).


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

![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Navedite naziv kanala, opis polje nije obavezno. U odjeljku postavkama kanala, odaberite **standardnu** za mogućnost Live kodiranje s protokolom unos postavljena na **RTMP**. Možete ostaviti sve ostale postavke kao što je.


Provjerite je li **pokretanje novog kanala sada** je uključen.

3. Kliknite **Stvaranje kanala**.
![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

>[AZURE.NOTE] Kanal može potrajati 20 minuta da biste pokrenuli.

Dok se pokreće kanal možete [konfigurirati kodiranje](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

>[AZURE.IMPORTANT] Imajte na umu da naplata počinje čim kanala ulazi u stanju spremno. Dodatne informacije potražite u članku [kanal, stanja](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_wirecast_rtmp></a>Konfiguriranje Telestream Wirecast encoder

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

1. Otvorite aplikaciju Telestream Wirecast na računalu koji se koristi i postaviti za strujanje RTMP.
2. Konfiguriranje izlazne tako da odete na karticu **Izlazni** , a zatim odaberete **Postavke izlaza...**.
    
    Provjerite je li **Odredišni izlaz** postavljen na **Poslužitelju RTMP**.
3. Kliknite **u redu**.
4. Na stranici Postavke postavite **odredišno** polje treba **Servisa Azure Media Services**.
 
    Kodiranje profil je unaprijed odabranih **Azure H.264 720 p 16:9 (1280 × 720)**. Da biste prilagodili te postavke, odaberite ikonu zupčanika s desne strane na padajućem izborniku prema dolje, a zatim odaberite **Novi unaprijed**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)

5. Konfiguriranje encoder konfiguracije.

    Naziv na zadana Postava, a potražiti na sljedeći način preporučenih postavki:

    **Videozapis**
    
    - Kodiranje: MainConcept H.264
    - Slika u sekundi: 30
    - Prosječnu brzinu prijenosa: 5000 kbits/sec (može se prilagoditi ovisno o ograničenja mreže)
    - Profil: glavne
    - Ključnih slika svakih: 60 okvira

    **Zvuk**

    - Cilj brzinu prijenosa: 192 kbits/sec
    - Brzina uzorkovanja: 44 100 kHz
     
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)

6. Pritisnite **Spremi**.

    Polje kodiranje sada ima novostvorenu dostupne za odabir profila. 

    Provjerite je li novi profil.

7. Da biste dodijelili Wirecast **RTMP krajnje**se unos URL-a na kanal.
    
    Vratite se na alat za AMSE pa provjerite na stanje dovršenja kanala. Kada je stanje promijenio iz **Početni** **pokrenutim**, možete dobiti unos URL-a.
      
    Kada je pokrenut kanal, desnom tipkom miša kliknite naziv kanala, kretanje dolje postavite pokazivač miša iznad **URL unos Kopiraj u međuspremnik** i zatim odaberite **Primarni unos URL-a**.  
    
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)

8. U prozoru Wirecast **Izlazne** zalijepite ti podaci u polju **Adresa** odjeljka izlazne i dodijelite naziv strujanje. 


    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

9. Odaberite **u redu**.

10. Na glavnom zaslonu **Wirecast** provjerite ulaznih izvora za video i audiosadržaji spremni i u gornjem lijevom kutu kliknite **strujanje** .

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

>[AZURE.IMPORTANT]Prije nego što kliknete **strujanje**, najprije **morate** provjerite je li kanal spreman. 
>Osim toga, provjerite ne da biste napustili kanal u stanju spremno bez unosa doprinos koji se sažetak sadržaja dulje od > 15 minuta.

##<a name="test-playback"></a>Reprodukcija test
  
1. Otvorite alat za AMSE pa desnom tipkom miša kliknite kanala kojim želite testirati. Na izborniku postavite pokazivač **reprodukcije pretpregleda** , a zatim odaberite **s Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Ako se pojavi toka reproduktora, zatim kodiranje pravilno konfigurirani za povezivanje s AMS. 

Ako je primili pogreške, kanal moraju se ponovno postaviti i prilagoditi postavke kodiranje. Pogledajte temu [za otklanjanje poteškoća](media-services-troubleshooting-live-streaming.md) smjernicama.  

##<a name="create-a-program"></a>Stvaranje programa

1. Kada se potvrđuje reprodukcije kanal, stvorite programa. Na kartici **Live** u alatu za AMSE, desnom tipkom miša kliknite unutar područja program i odaberite **Stvaranje nove Program**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)

2. Dodijelite naziv program i, ako je potrebno, prilagodite **Arhiva prozor duljine** (koji zadanih vrijednosti 4 sata). Možete odrediti mjesto za pohranu ili ostavite kao zadani.  
3. Potvrdite okvir **pokrenite Program sada** .
4. Kliknite **Stvori Program**.  
  
    Napomena: Stvaranje Program traje manje od stvaranje kanala.    
 
5. Kada je pokrenut program, provjerite reprodukcije desnom tipkom miša kliknite program i navigacija **reprodukcije programe** i odabirom **s Azure Media Player**.  
6. Kada se uvjerite, ponovno desnom tipkom miša kliknite program i odaberite **kopirajte URL Izlaz u međuspremnik** (ili pronaći te podatke iz **programa informacije i postavke** mogućnosti na izborniku). 

Strujanje je sada moguće ugrađen u čitač ili distribucije pred publikom uživo prikaz.  


## <a name="troubleshooting"></a>Otklanjanje poteškoća
 
Pogledajte temu [za otklanjanje poteškoća](media-services-troubleshooting-live-streaming.md) smjernicama. 

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
