<properties 
    pageTitle="Vodič za otklanjanje poteškoća za uživo strujanje | Microsoft Azure" 
    description="Ova tema sadrži prijedloge za otklanjanje poteškoća s uživo strujanje." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2016"  
    ms.author="juliako"/>

#<a name="troubleshooting-guide-for-live-streaming"></a>Vodič za otklanjanje poteškoća za strujanje uživo

Ova tema sadrži prijedloga na otklanjanju poteškoća neke uživo strujanje.

## <a name="issues-related-to-on-premises-encoders"></a>Problema vezanih uz lokalni koderi 

U ovom se odjeljku daje prijedloge za otklanjanje poteškoća vezanih uz lokalni koderi konfiguriranih da biste poslali jedan brzina prijenosa strujanje AMS kanala koje su omogućene za šifriranje uživo.

###<a name="problem-would-like-to-see-logs"></a>Problem: Željeli vidjeti zapisnika 

- **Potencijalne problem**: ne može traži encoder zapisnike koji vam mogu pomoći u ispravljanje pogrešaka problemi.
    
    - **Telestream Wirecast**: obično možete pronaći zapisnike u odjeljku C:\Users\{username} \AppData\Roaming\Wirecast\ 
    - **Elemental Live**: možete pronaći sadrži veze na zapisnika portala za upravljanje. Kliknite **Stat**, a zatim **zapisnika**. Na stranici **Datoteke zapisnika** , vidjet ćete popis zapisnika za sve LiveEvent stavke; Odaberite onaj koji se podudaraju trenutna sesija. 
    - **Flash Media Live Encoder**: **Zapisnik direktorija...** možete pronaći tako da odete na kartici **Kodiranje zapisnika** .
    
###<a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Problem: Ne postoji mogućnost za bilježiti progresivno strujanje

- **Potencijalne problem**: kodiranje koristi ne automatski Ukloni preplitanje. 

    **Korake za otklanjanje poteškoća**: potražite uklanjanje preplitanja mogućnost unutar encoder sučelja. Kada poništite preplitanje omogućena, ponovo Provjeri ima li progresivno izlazne. 
 
###<a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Problem: Pokušao nekoliko encoder izlazne i i dalje ne možete povezati. 

- **Potencijalne problem**: Azure kodiranja kanal nije ispravno vratiti. 

    **Korake za otklanjanje poteškoća**: Provjerite je li kodiranje više margina za AMS, zaustavite i ponovno postavljanje kanal. Nakon ponovnog pokretanja, pokušajte se vaš kodiranje s novim postavkama. Ako to još uvijek ne riješite problem, pokušajte stvoriti potpuno novog kanala, ponekad kanala možete oštećena nakon nekoliko pokušaja nije uspjelo.  

- **Potencijalne problem**: U GOP veličina ključnih slika nisu optimalnih. 

    **Korake za otklanjanje poteškoća**: preporučuje GOP veličine ili keyframe interval je dvije sekunde. Neke koderi izračunati ovu postavku u broj okvira, dok drugi koristiti sekundi. Na primjer: kada bilježiti 30fps veličinu GOP bio 60 okvire, što je jednako 2 sekunde.  
     
- **Potencijalne problem**: zatvoreni priključke onemogućuju toka. 

    **Korake za otklanjanje poteškoća**: kada strujanje putem RTMP, provjerite postavke vatrozida i/ili proxy poslužitelja da biste potvrdili otvoreno izlaznog priključke 1935 i 1936. Prilikom korištenja RTP strujanje, provjerite je li priključka 2010 Otvori. 


###<a name="problem-when-configuring-the-encoder-to-stream-with-the-rtp-protocol-there-is-no-place-to-enter-a-host-name"></a>Problem: Prilikom konfiguriranja encoder strujanje s protokolom RTP, postoji bez mjesto da biste unijeli naziv glavnog računala. 

- **Potencijalne problem**: mnogo RTP koderi ne dopuštaju nazivi glavnog računala i IP adresom moraju biti dobivene.  

    **Korake za otklanjanje poteškoća**: da biste pronašli IP adresu, otvorite naredbeni redak na bilo kojem računalu. Da biste to učinili u sustavu Windows, otvorite pokretač cilja (WIN + R) i upišite "cmd" da biste otvorili.  

    Kad je otvoren naredbeni redak, upišite "Ping [AMS glavno računalo]". 

    Naziv glavnog računala mogu biti izvedena po ispuštanje broj priključka iz Azure Ingest URL, kao što je istaknuta u sljedećem primjeru: 

    RTP://test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 / 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

###<a name="problem-unable-to-playback-the-published-stream"></a>Problem: Nije moguće reprodukcije objavljene toka.
 
- **Potencijalne problem**: ne postoji strujanje krajnja točka pokrenut ili je nema strujanje (skaliranje jedinice) raspoređenih jedinica. 

    **Korake za otklanjanje poteškoća**: idite na karticu "Strujanje krajnjoj točki" u alatu za AMSE i provjerili postoji strujanje krajnje raditi s jedne strujanje jedinice. 
    


>[AZURE.NOTE] Ako ste pratili korake za otklanjanje poteškoća i dalje ne može uspješno strujanja, pošaljite je zahtjev za podršku možete pomoću portala za Azure.

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
