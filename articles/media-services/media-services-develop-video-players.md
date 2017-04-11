<properties 
    pageTitle="Razvoj aplikacija reproduktora videosadržaja" 
    description="U temi navode veze reproduktora okviri i dodaci koje možete koristiti za razvoj vlastite klijentske aplikacije koje mogu zauzeti strujanja medijskih sadržaja iz Media Services." 
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


#<a name="develop-video-player-applications"></a>Razvoj aplikacija reproduktora videosadržaja

##<a name="overview"></a>Pregled

Azure Media Services nudi alate potrebnih za stvaranje obogaćenih, dinamični klijentske aplikacije reproduktora za većinu platforme uključujući: iOS uređaja, uređaje sa sustavom Android, Windows, Windows Phone, Xbox i postavljanje gornjeg okvira. U ovoj se temi navode veze na SDK-ovi i okviri reproduktora koje možete koristiti za razvoj vlastite klijentske aplikacije koje mogu zauzeti strujanja medijskih sadržaja iz servisa Azure Media Services.

##<a name="azure-media-player"></a>Azure Media Player

[Azure Media Player](http://aka.ms/ampinfo) je na web reproduktora videosadržaja ugrađenih reprodukcija medijskog sadržaja iz servisa Microsoft Azure Media Services na velik broj preglednici i uređajima. Azure Media Player koristi industrijske standarde, kao što su HTML5, medijske sadržaje izvor proširenja (MSE) i šifrirane Media proširenja (EME) ponuditi enriched prilagodljivo strujanje. Kada te standarde nisu dostupne na uređaju ili u pregledniku, Azure Media Player koristi Flash i Silverlight pričuvni tehnologije. Bez obzira na to tehnologija reprodukcije pomoću razvojnim inženjerima će imati objedinjenih sučelja JavaScript API-ji za pristup. Time za sadržaj posluživanje sustavom servisa Azure Media Services koja će se reproducirati preko wide – raspon uređaji i preglednici bez sve dodatne truda.

Microsoft Azure Media Services omogućuje za sadržaj posluživanje prema gore s CRTICE, Smooth Streaming i HLS strujanje oblika reprodukcije. Azure Media Player uzima u obzir ove različite oblike i automatski reproducira vezu najbolje ovisno o mogućnostima platformu/preglednika. Microsoft Azure Media Services i omogućuje dinamički šifriranje imovine šifriranjem PlayReady ili AES 128-bitno šifriranje omotnice. Azure Media Player omogućuje za dešifriranje PlayReady i AES 128-bitno šifrirane sadržaja kada pravilno konfigurira. 

Dodatne informacije:

- [Azure Media Player](http://aka.ms/ampinfo)
- [Dokumentacija Azure Media Player](http://aka.ms/ampdocs) 
- [Azure Media Player početak rada na blogu](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
- [Prijavite se da biste bili u tijeku s novostima s Azure Media Player](http://aka.ms/ampsignup)
- [Dodavanje nove zahtjeve za značajke, ideje, povratnih informacija](http://aka.ms/ampuservoice ) 


##<a name="other-tools-for-creating-player-applications"></a>Druge alate za stvaranje reproduktora aplikacija

Možete koristiti bilo koji od sljedećih SDK-ovi:

- [Glačanje strujanje klijent SDK](http://www.iis.net/downloads/microsoft/smooth-streaming) 
- [Gladak strujanje iz Windows trgovine](media-services-build-smooth-streaming-apps.md)
- [Platformu Microsoft Media: Framework reproduktora](http://playerframework.codeplex.com/) 
- [HTML5 Reproduktora Framework dokumentacija](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
- [Microsoft prelazili putem glatke strujanje dodatak za OSMF](https://www.microsoft.com/download/details.aspx?id=36057) 
- [Licenciranje Microsoft® izglađenim strujanje klijent Porting Kit](http://aka.ms/sspk) 
- [Razvoj aplikacija za videozapisa za XBOX](http://xbox.create.msdn.com/) 
 

##<a name="advertising"></a>Oglašavanje

Azure Media Services pruža podršku za ad umetanja putem platforme sustava Windows Media: okviri reproduktora. Okviri reproduktora s podrškom za ad su dostupne za Windows 8, Silverlight, Windows Phone 8 i iOS uređaje. Svaki framework reproduktora sadrži ogledni kod koji pokazuje kako implementirati aplikaciju za reprodukciju. Postoje tri različite vrste reklame možete umetnuti u medijskih sadržaja:

Linearni – reklame cijelog okvira koji glavni videosadržaja

Nelinearni – reklame prekriveno koji se prikazuju kao glavni videozapis koji se reproducira, obično logotipa ili neke druge statične slike smješten unutar reproduktora

Suradnik – reklame koje su prikazane izvan reproduktora

Reklame možete staviti u glavnom videozapis vremenska traka u bilo kojem trenutku. Morate reći reproduktora trenutka reprodukcije u ad i koje reklame za reprodukciju. To možete učiniti pomoću skup standardnih utemeljenih na XML datoteka: videozapis Ad servisa predložak (VAST), digitalni videozapis više Ad popisa (VMAP), medijske sadržaje Apstraktni određivanje redoslijeda predložak (MAST) i digitalni videozapis reproduktora Ad sučelja definicija (VPAID). VELIKU datoteke odredite koje reklame da bi se prikazao. Datoteka VMAP odredite kada će se reproducirati razne reklame i sadrže VELIKU XML. Datoteke MAST su drugi način reklame niz koji se može sadržavati VELIKU XML. Datoteka VPAID definirati sučelja između reproduktora videosadržaja i ad ili poslužitelja za ad. Dodatne informacije potražite u članku [Umetanje reklame](https://msdn.microsoft.com/library/dn387398.aspx).

Informacije o zatvoreni titlovi i reklame podršku Live strujanje videozapisa potražite u članku [podržane skriveni titlovi i Ad umetanja standarda](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vidi također

[Ugrađivanje prilagodljivo strujanje videozapisa MPEG-crtica u obliku HTML5 aplikacijom DASH.js](media-services-embed-mpeg-dash-in-html5.md)

[Spremište dash.js GitHub](https://github.com/Dash-Industry-Forum/dash.js)
 
