<properties 
    pageTitle="Ugrađivanje prilagodljivo strujanje videozapisa MPEG-crtica u obliku HTML5 aplikacijom DASH.js | Microsoft Azure" 
    description="U ovoj se temi objašnjava kako Ugradnja videozapisa prilagodljivu Streaming MPEG-crtica u obliku HTML5 aplikacijom DASH.js." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>Ugrađivanje prilagodljivo strujanje videozapisa MPEG-crtica u obliku HTML5 aplikacijom DASH.js

##<a name="overview"></a>Pregled

MPEG-CRTICOM je ISO standard za prilagodljivu streaming videosadržaj koji nudi brojne prednosti za osobe koje želite da se za isporuku videozapise visoke kvalitete, prilagodljivu streaming izlaz. MPEG-crticu strujanje videozapisa će automatski ispustite na donjem definiciju kad postane congested s mrežom. Time se smanjuje vjerojatnost preglednika koji se prikazuju "pauzirano" videozapis dok reproduktora preuzima sljedeći nekoliko sekundi da biste reproducirali (ili međuspremnik). Kao što je zagušenja mreže smanjuje, reproduktora videosadržaja vratit će shodno veća kvaliteta strujanje. Ova mogućnost prilagodite propusnost potrebnu dobivaju se brže vrijeme početka videozapis. To znači da prvih nekoliko sekundi mogu se reproducirati u donjem segment kvalitete brzo da biste preuzeli, a zatim koraka do veća kvaliteta jednom dovoljno sadržaja sadrži je u međuspremniku.

Dash.js je se Otvori izvor MPEG-CRTICOM reproduktora videosadržaja pisane JavaScript. Njegov cilj je pružanje robusne, više platformi reproduktoru koji se može slobodno ponovno koristiti u aplikacijama koje je potrebno reprodukcije videozapisa. Pruža MPEG-CRTICOM reprodukcije u bilo kojem pregledniku koji podržava danas u W3C Media izvor proširenja (MSE), koji je Chrome, Microsoft Edge i IE11 (drugim preglednicima ste naznačili njihove svrhu za podršku MSE). Dodatne informacije o DASH.js js potražite u članku dash.js spremište GitHub.


##<a name="creating-a-browser-based-streaming-video-player"></a>Stvaranje je utemeljenima na pregledniku strujanje reproduktor videozapisa

Da biste stvorili jednostavnu web-stranicu koja se prikazuje je reproduktor videozapisa s polje očekivane kontrole takve reprodukciju, zaustavljanje, premotavanje itd., morat ćete:

1. Stvaranje HTML stranica
1. Dodavanje videozapisa oznake
1. Dodavanje reproduktora dash.js
1. Pokretanje reproduktora
1. Dodavanje stila CSS-a
1. Prikaz rezultata u pregledniku koji implementira MSE

Pokretanje reproduktora možete obaviti u samo handful redaka JavaScript koda. Pomoću dash.js, zapravo je to jednostavne za ugrađivanje videozapisa MPEG-CRTICOM aplikacija preglednika koji se temelje.

##<a name="creating-the-html-page"></a>Stvaranje stranice HTML

Prvi je korak da biste stvorili standardne HTML stranicu koja sadrži element **videozapisa** , Spremi datoteku u obliku basicPlayer.html, kao što prikazuje sljedeći primjer:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

##<a name="adding-the-dashjs-player"></a>Dodavanje reproduktora DASH.js

Da biste dodali referenca implementaciju dash.js aplikacije, morat ćete privucite dash.all.js datoteke iz 1.0 izdanja dash.js projekta. Time se treba spremaju u mapu JavaScript aplikacije. Datoteka je datoteku praktičnost koju povlači zajedno sve potrebne dash.js kod u jednu datoteku. Ako imate izgleda oko spremište dash.js, će pronaći pojedinačne datoteke, testiranje kod i još mnogo toga, ali ako sve želite učiniti je korištenje dash.js, a zatim datoteka dash.all.js je što vam je potrebno.

Da biste dodali reproduktora dash.js aplikacije, dodati oznaku skripte glavni dio basicPlayer.html:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Nakon toga stvorite funkcija pokretanje reproduktora prilikom učitavanja stranice. Da biste dodali sljedeću skriptu nakon retka učitavanje dash.all.js:

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

Ova funkcija najprije stvara na DashContext. Koristi se za konfiguriranje aplikacije za određene runtime okruženje. Iz programa tehničke točku prikaza, definira klase koji se unos framework ovisnost trebali biste koristiti tijekom izgradnje aplikacije. U većini slučajeva, koristit će Dash.di.DashContext.

Nakon toga stvoriti instancu primarni klasa framework dash.js, reproduktor medijskih sadržaja. Klase sadrži u skladu s osnovnim metoda kao što je potrebno reproducirati i zadržite pokazivač, upravlja odnos s element videozapisa i i upravlja tumačenja datoteke medijskih sadržaja prezentacije opis (MPD) koji opisuje videozapis koja će se reproducirati.

Da biste bili sigurni da je reproduktora spreman za reprodukciju videozapisa za pozivanje funkcije startup() klase reproduktor medijskih sadržaja. Između ostaloga Ova funkcija osigurava da potrebne klase (kako je definirano kontekst) imaju učitan. Kad je spremna reproduktora, možete priložiti element videozapisa pomoću funkcije attachView(). Time se omogućuje reproduktor medijskih sadržaja ubaciti strujanje videozapisa u element i upravljati i reprodukcija prema potrebi.

Prenesite URL datoteke MPD na reproduktor medijskih sadržaja tako da ga zna o videozapisu se očekuje da biste reproducirali. Funkcija setupVideo() upravo stvorili ćete morati izvršiti kad potpuno učitana stranice. To pomoću događaj pri učitavanju tijelo elementa. Promjena vaše <body> elementa koji želite:

    <body onload="setupVideo()">

Na kraju, postavite veličinu videozapisa elementa pomoću CSS-a. U prilagodljivo strujanje okruženju, to je osobito važno jer veličinu videozapisa koji se reproducira mijenjaju tijekom reprodukcije prilagođavati im mijenjanju uvjeti na mreži. U ovom jednostavne pokazni videozapis jednostavno prisilno videozapisa elementa koji će biti 80% prozora preglednika dostupna dodavanjem sljedećih CSS na glavni odjeljak na stranici:
    
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

##<a name="playing-a-video"></a>Reproduciranje videozapisa

Da biste reproducirali videozapis, pokažite preglednika na basicPlayback.html datoteku, a zatim kliknite Reproduciraj na reproduktora videosadržaja prikazuje.


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vidi također

[Razvoj aplikacija reproduktora videosadržaja](media-services-develop-video-players.md)

[Spremište dash.js GitHub](https://github.com/Dash-Industry-Forum/dash.js) 
