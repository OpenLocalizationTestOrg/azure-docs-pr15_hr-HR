<properties 
    pageTitle="Gladak strujanje dodatak za medijske sadržaje Framework Otvori izvor" 
    description="Saznajte kako koristiti modul Azure Media Services izglađenim strujanje za medijske sadržaje Framework za otvaranje izvora Adobe." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Kako koristiti Microsoft prelazili putem glatke strujanje dodatak za Framework za medijske sadržaje Adobe Otvori izvor

##<a name="overview"></a>Pregled


Dodatak za Microsoft Smooth Streaming za otvaranje izvora Media Framework 2.0 (SS za OSMF) proširuje mogućnosti zadane OSMF i dodaje Microsoft Smooth Streaming sadržaja reprodukcije nove i postojeće OSMF reproduktora. Dodatak i dodaje Smooth Streaming mogućnosti reprodukcije za reprodukciju povratkom medijske sadržaje (SMP).

SS za OSMF sadrži dvije verzije značajke dodatak:

- Statički Smooth Streaming dodatak za OSMF (.swc)

- Dinamični Smooth Streaming dodatak za OSMF (.swf)

Ovaj dokument pretpostavlja da čitač ima Općenito znanja rad OSMF i OSMF dodatke. Dodatne informacije o OSMF potražite u dokumentaciji na [službeni OSMF web-mjesta](http://osmf.org/).

###<a name="smooth-streaming-plugin-for-osmf-20"></a>Gladak strujeće dodatak za OSMF 2.0

Dodatak podržava učitavanje i reproduciranje osvježavati Smooth Streaming sadržaja sljedeće značajke:

- Reprodukcija Smooth Streaming osvježavati (Reproduciraj Pauziraj, Seek, Zaustavi)
- Smooth Streaming uživo reprodukcije (Reproduciraj)
- Live DVR funkcije (Pauziraj Seek, reprodukcija DVR Idi važenja)
- Podrška za Videokodeci - H.264
- Podrška za zvuk kodecima - AAC
- Prebacivanje s OSMF ugrađene API-ji više audio jezik
- Max reprodukcije kvalitete odabira s OSMF ugrađene API-ji
- Sidecar skriveni titlovi s OSMF opise dodatak
- Adobe&reg; Flash&reg; reproduktora 11.4 ili noviji.
- Ova verzija podržava samo OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Podržane značajke i poznati problemi

Potpuni popis značajki podržanih nepodržane značajke i poznatim problemima odnosi se na [ovaj dokument](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).


## <a name="loading-the-plugin"></a>Učitavanje dodatak
Dodaci za OSMF mogu se učitati statički (trenutku Kompiliranje) ili dinamički (pri izvođenju). Dodatak Smooth Streaming za preuzimanje OSMF obuhvaća dinamički i statične verzije.

- Statički učitavanja: da biste učitali statički, datoteku statične biblioteke (SWC) potreban je. Dodaci za statične dodaju se kao referenca na projektima i spajanje unutar konačni Izlazna datoteka Kompiliranje vrijeme.

- Dinamični učitavanja: da biste učitali dinamički, unaprijed kompilirane (SWF) datoteke potreban je. Dodaci za dinamičku se učitati u vremena izvođenja, a nije uvršten u izlaz projekta. (Kompilirane Izlaz) Dodaci za dinamičku biti učitan putem protokola HTTP i datoteke.

Dodatne informacije o statički i dinamički učitavanja potražite u članku službeni [OSMF dodatak stranice](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

###<a name="ss-for-osmf-static-loading"></a>SS OSMF statične događajima
Ispod isječak koda pokazuje kako statički Učitavanje dodatka SS za OSMF i reproduciranje videozapisa osnovni pomoću OSMF MediaFactory predmete. Prije uključivanja SS za OSMF kod, provjerite je li referenca projekta obuhvaća je dodatak statične "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc".

```
package 
{
    
    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    
    
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        

        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }
    
        private function initMediaPlayer():void
        {
        
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            
            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```


###<a name="ss-for-osmf-dynamic-loading"></a>SS OSMF dinamički događajima

Ispod isječak koda pokazuje kako dinamično Učitavanje dodatka SS za OSMF i reproduciranje programskog jezika basic videozapisa pomoću klase OSMF MediaFactory. Prije uključivanja SS OSMF koda, kopirajte "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dodatak za dinamičku u mapu projekta ako želite učitati pomoću protokola datoteka ili kopirajte u odjeljku web-poslužitelj za HTTP. Nema potrebe da biste obuhvatili "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" reference projekta.

 
{paketa
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;

    
    //Sets the size of the SWF
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        
        
        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }
        
        private function initMediaPlayer():void
        {
            
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}

##<a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>Povratkom reprodukcije medijskog sadržaja s SS ODMF dodatak za dinamičku

Smooth Streaming dodatak za dinamičku OSMF je kompatibilan s [Reprodukcijom medijskih sadržaja na povratkom (SMP)](http://osmf.org/strobe_mediaplayback.html). Da biste dodali Smooth Streaming sadržaja reprodukcije SMP možete koristiti SS za OSMF dodatak. Da biste to učinili, kopirajte "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" u odjeljku web-poslužitelj za HTTP na sljedeći način:

1.  Pronađite [stranicu za postavljanje povratkom reprodukcije medijskog sadržaja](http://osmf.org/dev/2.0gm/setup.html). 
2.  Postavite na src Smooth Streaming izvor (npr. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3.  Promijenite željena konfiguracija pa kliknite Pretpregled i ažuriranja.
 
    **Napomena** Sadržaj web-poslužitelju mora valjani crossdomain.xml. 
4.  Kopirajte i zalijepite kod za jednostavne HTML stranicu pomoću uređivač omiljene tekst, kao što su u sljedećem primjeru:



        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



5. Dodavanje dodatka Smooth Streaming OSMF kod za ugradnju i spremiti.

        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>


6.  Spremanje HTML stranica i objavljivanje web-poslužitelj. Pronađite objavljenu web-stranicu pomoću svoje omiljene Flash&reg; reproduktora omogućeno internetskog preglednika (Internet Explorer, Chrome, Firefox, tako dalje).
7.  Uživajte u Smooth Streaming sadržaja unutar Adobe&reg; Flash&reg; reproduktora.

Dodatne informacije o Općenito razvoj OSMF potražite službeni [OSMF razvoj stranice](http://osmf.org/resources.html).

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Vidi također

[Microsoftove prilagodljivu Streaming dodatak za ažuriranje OSMF](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 
