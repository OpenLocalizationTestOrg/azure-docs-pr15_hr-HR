<properties 
    pageTitle="Umetanje reklame na klijentskoj strani | Microsoft Azure" 
    description="U ovoj se temi objašnjava Umetanje reklame na klijentskoj strani." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="inserting-ads-on-the-client-side"></a>Umetanje reklame na klijentskoj strani

Ova tema sadrži informacije o umetanju razne vrste reklame na klijentskoj strani.

Informacije o zatvoreni titlovi i ad podršku Live strujanje videozapisa potražite u članku [podržane skriveni titlovi i Ad umetanja standarda](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

>[AZURE.NOTE] Azure Media Player trenutno ne podržava reklame.

##<a id="insert_ads_into_media"></a>Umetanje reklame u medijskih sadržaja

Azure Media Services pruža podršku za ad umetanja putem platforme sustava Windows Media: okviri reproduktora. Okviri reproduktora s podrškom za ad su dostupne za Windows 8, Silverlight, Windows Phone 8 i iOS uređaje. Svaki framework reproduktora sadrži ogledni kod koji pokazuje kako implementirati aplikaciju za reprodukciju. Postoje tri različite vrste reklame možete umetnuti u medijskih sadržaja: popis.

- **Linearni** – reklame cijelog okvira koji glavni videosadržaja.
- **Nonlinear** – reklame prekriveno koji se prikazuju kao glavni videozapis koji se reproducira, obično logotipa ili neke druge statične slike smješten unutar reproduktora.
- **Suradnik** – reklame koje su prikazane izvan reproduktora.

Reklame možete staviti u glavnom videozapis vremenska traka u bilo kojem trenutku. Morate reći reproduktora trenutka reprodukcije u ad i koje reklame za reprodukciju. To možete učiniti pomoću skup standardnih utemeljenih na XML datoteka: videozapis Ad servisa predložak (VAST), digitalni videozapis više Ad popisa (VMAP), medijske sadržaje Apstraktni određivanje redoslijeda predložak (MAST) i digitalni videozapis reproduktora Ad sučelja definicija (VPAID). VELIKU datoteke odredite koje reklame da bi se prikazao. Datoteka VMAP odredite kada će se reproducirati razne reklame i sadrže VELIKU XML. Datoteke MAST su drugi način reklame niz koji se može sadržavati VELIKU XML. Datoteka VPAID definirati sučelja između reproduktora videosadržaja i ad ili ad poslužitelja.

Svaki framework reproduktora funkcionira na drugačiji način, a zatim svaku vlastitu temu prekriveno. U ovoj se temi opisuju će osnovni Mehanizmi koji se koriste za umetanje reklame. Aplikacija reproduktora videosadržaja zahtjev reklame s ad poslužitelja. Poslužitelj za ad može odgovoriti na nekoliko načina:

- Vraćanje VELIKU datoteku
- Vraćanje VMAP datoteke (s ugrađene VAST)
- Vraćanje MAST datoteke (s ugrađene VAST)
- Vraćanje VELIKU datoteku s VPAID reklame

 
###<a name="using-a-video-ad-service-template-vast-file"></a>Pomoću predloška (VAST) datoteke servisa Ad videozapisa

VELIKU datoteku određuje koji ad ili reklame da bi se prikazao. Primjer VELIKU datoteku za linearni ad je XML za sljedeće:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
    
Linearni ad je opisan u **<Linear>** element. Određuje trajanje oglas, praćenje događaja, kliknite putem, kliknite praćenje i broj **<MediaFile>** elemente. Praćenje događaja određeni su klauzulom unutar na **<TrackingEvents>** element i omogućiti poslužitelj za ad da biste pratili različite događaja koji nastaju dok gledate u ad. U ovom slučaju početka, srednja vrijednost boje, Dovršeno, a proširite prate događaja. Početak događaj kada se prikaže u ad. Srednja vrijednost boje događaj kada najmanje 50% vremenske crte u ad sadrži je prikazati. Dovršavanje događaj kada oglas pokrenuta do kraja. Proširivanje događaj kada korisnik će se proširiti i reproduktor videozapisa preko cijelog zaslona. Clickthroughs određeni su klauzulom s na **<ClickThrough>** elementa unutar na **<VideoClicks>** element i određuje URI za neki resurs će se prikazati kada korisnik klikne oglas. ClickTracking je naveden u na **<ClickTracking>** element, također unutar na **<VideoClicks>** element i određuje resursa za praćenje reproduktora da biste zatražili kada korisnik klikne u ad. Na **<MediaFile>** elemenata navesti informacije o određenim kodiranjem u ad. Ako postoji više od jedne **<MediaFile>** elementa reproduktora videosadržaja možete odabrati najbolje kodiranja platforme. 

Linearni reklame moguće je prikazati određenim redoslijedom. Da biste to učinili, dodajte dodatne <Ad> elemenata u VAST datoteku i navedite redoslijed pomoću atribut niz. Sljedeći primjer prikazuje sljedeće:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
    
U određeni su klauzulom nelinearni reklame na <Creative> kao i element. U sljedećem primjeru u <Creative> element koji opisuje nelinearni ad.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>

 
Na **<NonLinearAds>** element može sadržavati jedno ili više **<NonLinear>** elemenata, od kojih svaka možete opisati nelinearni ad. Na **<NonLinear>** element određuje resurs za nelinearni ad. Resurs može biti u **<StaticResouce>**, u **<IFrameResource>**, ili pomoću **<HTMLResouce>**.**<StaticResource>** u članku se opisuje resursa koje nisu HTML i definira creativeType atributa koji određuje način prikaza resursa:

Slika/gif, slika i jpeg, slika/png – resursa prikazuje se u HTML **<img>** oznaka.

Aplikacija/x-javascript – resursa prikazuje se u oznaku HTML <**skripte**>.

Aplikacija/x-shockwave-flash – resursa prikazuje se u Flash player.

**<IFrameResource>**u članku se opisuje resursa u HTML koji se mogu prikazati u okviru IFrame. **<HTMLResource>**u članku se opisuje dio HTML kod koji se može umetati na web-stranicu. **<TrackingEvents>**Odredite praćenje događaja i URI da biste zatražili kada se pojavi događaj. U ovom primjeru prate se događaji acceptInvitation i sažimanja. Dodatne informacije o na **<NonLinearAds>** element i podređenim mu objektima potražite u članku IAB.NET/VAST. Imajte na umu da se **<TrackingEvents>** element nalazi unutar u** <NonLinearAds> ** element umjesto na **<NonLinear>** element.

Reklame pomoćnom definiraju unutar na <CompanionAds> element. Na <CompanionAds> element može sadržavati jedno ili više <Companion> elemente. Svaki <Companion> element u članku se opisuje pomoćnom ad i mogu sadržavati na <StaticResource>, <IFrameResource>, ili <HTMLResource> koji su navedeni na isti način kao nelinearni ad. VELIKU datoteku može sadržavati više reklame suradnika i aplikacije reproduktora možete odabrati najprikladnije ad da bi se prikazao. Dodatne informacije o VAST potražite u članku [VELIKU 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

###<a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Korištenje digitalnih videozapisa više Ad (VMAP) za reprodukciju

VMAP datoteka omogućuje vam da navedete kada ad prijeloma pojavljuje svakog prijeloma koliko je, koliko reklame mogu se prikazati unutar prijeloma, a što vrste reklame mogu prikazati tijekom prijelom. Sljedeći primjer datoteka VMAP koji definira jedan ad prijelom:
    
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
     
Datoteke VMAP počinje s <VMAP> element koji sadrži jedan ili više <AdBreak> elemenata, svaki definiranje u ad prijelom. Svakog prijeloma ad navodi vrstu prijeloma, prijelom ID i offset vremena. Atribut breakType navodi vrstu ad koje je moguće reproducirati tijekom prijelom: linearni, nelinearni, ili prikazati. Prikaz karte reklame reklame VELIKU suradnika. Više od jedne vrste ad možete navesti na popisu odvojenih zarezom (bez razmaka). U breakID je neobavezno identifikator za oglas. Na timeOffset određuje kada se u ad treba prikazati. Možete navesti na jedan od sljedećih načina:

1. Vrijeme u obliku HH ili hh:mm:ss.mmm gdje je .mmm milisekundi. Vrijednost toga atributa navodi vrijeme od početka videozapisa vremenske trake na početku ad prijelom.
1. Postotak postotak – u obliku n % pri čemu je n je videozapisa vremenske trake da biste reproducirali prije reprodukcije u ad
1. Početak i kraj – navodi da se u ad treba prikazati prije ili nakon su prikazani videozapisa
1. Smjestite – određuje redoslijed ad prijelome kada je tempiranja prijelome ad Nepoznato, kao što uživo strujanje. Redoslijed svakog prijeloma ad naveden je u obliku #n pri čemu je n cijeli 1 ili noviji. 1 označava moraju se reproducirati u ad pri prvom prilike 2 označava oglas treba moguće reproducirati na drugom prilike i tako dalje.

Unutar elementa <**AdBreak**> može biti jedan element n <**AdSource**>. Element <**AdSource**> sadrži sljedećim atributima:

1. ID – određuje identifikator za izvor ad
1. allowMultipleAds – logička vrijednost koja određuje hoće li se više reklame se prikazivati tijekom ad prijelom
1. followRedirects – neobavezna Booleova vrijednost koja određuje ako moraju poštovati reproduktora videosadržaja preusmjerava unutar je odgovor ad

Element <**AdSource**> nudi reproduktora je odgovor u istoj razini ad ili referenca je ad odgovor. Može sadržavati jednu od sljedećih elemenata:

- <VASTAdData>označava odgovor VELIKU ad ugrađena u datoteku VMAP
- <AdTagURI>URI koji referencira je odgovor ad iz drugih sustava za
- <CustomAdData>-an proizvoljne niza respresents koje nisu VELIKU odgovor

U ovom primjeru nije naveden je u redak ad odgovor s na <VASTAdData> element koji sadrži VELIKU ad odgovor. Dodatne informacije o drugim elementima potražite u članku [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

Element <**AdBreak**> mogu sadržavati jedan element n <**TrackingEvents**>. Element <**TrackingEvents**> omogućuje vam praćenje početka ili završetka na prijelom ad ili li pojavila se pogreška tijekom ad prijelom. Element <**TrackingEvents**> sadrži jedan ili više <**praćenja**> elemenata, od kojih svaka određuje praćenje događaja i praćenje URI. Nekoliko mogućih praćenje događaja:

1. breakStart – prati početak u ad prijelom
1. breakEnd – praćenje obavljanjem u ad prijelom
1. Pogreška – prati pogreška tijekom ad prijelom

Sljedeći primjer prikazuje VMAP datoteku koja određuje praćenje događaja

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

Dodatne informacije o element <**TrackingEvents**> i podređenim mu objektima potražite u članku http://iab.org/VMAP.pdf

###<a name="using-a-media-abstract-sequencing-template-mast-file"></a>Pomoću medija sažetak određivanje redoslijeda datoteku predloška (MAST)

MAST datoteka omogućuje vam da navedete okidača koje definiraju kada se prikazuje u ad. Slijedi primjer datoteka MAST koja sadrži okidača za ad za snimljene fotografije pre, usred snimljene fotografije ad i nakon snimljene fotografije ad.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>

 

Datoteke MAST počinje s **<MAST>** element koji sadrži jednu **<triggers>** element. Na <triggers> element sadrži jedan ili više **<trigger>** elemenata koji definiraju kada reproducirati u ad. 

U **<trigger>** sadrži element u **<startConditions>** element koji odredite kada se u ad započeti za reprodukciju. Na **<startConditions>** element sadrži jedan ili više <condition> elemente. Prilikom svakog <condition> procijeni kao true okidač pokrene ili povučen ovisno o tome želite li na <condition> sadržana je u na **< startConditions**> ili **<endConditions>** element odnosno. Kada više <condition> postoje elemenata, oni se smatraju implicitno ili, bilo koji uvjet procjene true uzrokovat će okidača da biste započeli. <condition>Elementi može se ugnijezditi. Kada podređeni <condition> su unaprijed elemente, oni se smatraju IMPLICITNIH i, svih uvjeta moraju biti vrednovani true za pokretanje da biste započeli. Na <condition> element sadrži sljedećim atributima koje definiraju uvjeta: 

1. **Vrsta** – navodi vrstu uvjeta, događaj ili svojstva
1. **naziv** – naziv svojstva ili događaja koje će se koristiti tijekom izvođenja
1. **vrijednost** – vrijednost svojstvo će se računati odnosu
1. **operator** – operaciju koja će se koristiti tijekom izvođenja: EQ (jednako), NEQ (nije jednako), GTR (veće), GEQ (veće ili jednako), LT (manji od), LEQ (manje od ili jednako), MOD (modulo)

**<endConditions>**sadrže <condition> elemente. Kada uvjet vrednuje kao true okidača je ponovno postaviti. Na <trigger> element sadrži i na <sources> element koji sadrži jedan ili više <source> elemente. Na <source> elemenata definiranje URI ad odgovor i vrstu ad odgovor. U ovom primjeru URI daje VELIKU odgovor. 


    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
 

###<a name="using-video-player-ad-interface-definition-vpaid"></a>Korištenje videozapisa reproduktora Ad sučelja definicija (VPAID)

VPAID je API za omogućivanje izvršna ad jedinice za komunikaciju s je reproduktor videozapisa. Time se omogućuje iznimno interaktivna ad sučelja. Korisniku omogućuje interakciju s oglas i oglas možete odgovoriti akcija za preglednik. Na primjer u ad mogu prikazati gumbe koji korisniku omogućuje prikaz dodatne informacije ili više verzija u ad. Reproduktora videosadržaja mora podržavati VPAID API-JA i izvršna ad morate provesti na API-JA. Kada je reproduktor zahtjeve servisa Active Directory iz ad server poslužitelj možda odgovor na poruke uz VELIKU odgovor koji sadrži VPAID ad.

Izvršna ad se stvara u kod koji morate izvršiti u okruženju izvođenja kao što su Adobe Flash™ ili JavaScript koje možete izvršiti pomoću web-preglednika. Kada poslužitelj za ad vraća VELIKU odgovor koji sadrži VPAID ad, vrijednost u apiFramework atribut u <MediaFile> element mora biti "VPAID". Atribut određuje sadržavao ad VPAID izvršna servisa Active Directory. Vrsta atributa mora biti postavljeno na vrstu MIME izvršnu datoteku, kao što su "aplikacija/x-shockwave – flash" ili "aplikacija/x-javascript". U sljedećem moguće isječak s XML na <MediaFile> element VELIKU odgovor koji sadrži VPAID izvršna servisa Active Directory. 

    
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
 

Izvršna ad se može pokrenuti pomoću na <AdParameters> elementa unutar na <Linear> ili <NonLinear> elemenata u VELIKU odgovor. Dodatne informacije o na <AdParameters> element, potražite u članku [VELIKU 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Dodatne informacije o VPAID API potražite u članku [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

##<a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Implementaciju sa sustavom Windows ili Windows Phone 8 Player s podrškom za Ad

Platforme Microsoft Media: Reproduktora Framework za Windows 8 i Windows Phone 8 sadrži skup probne aplikacije koji vam pokazuju kako implementirati reproduktora videosadržaja aplikacije pomoću framework. Možete preuzeti Framework reproduktora i primjere s [reproduktora Framework za Windows 8 i Windows Phone 8](https://playerframework.codeplex.com).

Kada otvorite rješenje Microsoft.PlayerFramework.Xaml.Samples prikazat će broj mapa u projektu. Oglašavanje mapa sadrži važne za stvaranje je reproduktor videozapisa s podrškom za ad uzorak koda. Unutar na oglašavanje mapa je broj XAML/cs datoteka koje pokazuju kako da biste umetnuli reklame na drugačiji način. Sljedeći popis sadrži opis svakog:

- AdPodPage.xaml prikazuje kako prikazati u ad pod.
- AdSchedulingPage.xaml prikazuje kako zakazivati reklame.
- FreeWheelPage.xaml pokazuje kako koristiti dodatak za FreeWheel da biste zakazali reklame.
- MastPage.xaml prikazuje kako zakazivati reklame pomoću MAST datoteke.
- ProgrammaticAdPage.xaml pokazuje kako programski zakazivanje reklame u videozapis.
- ScheduleClipPage.xaml prikazuje kako zakazivati servisa Active Directory bez VELIKU datoteku.
- VastLinearCompanionPage.xaml prikazuje kako umetnuti u linearnom i ad suradnika.
- VastNonLinearPage.xaml prikazuje kako umetnuti bilo nelinearno ad.
- VmapPage.xaml prikazuje kako odrediti reklame pomoću VMAP datoteke.

Svaki od tih uzoraka koristi predmete reproduktor medijskih sadržaja definira framework reproduktora. Većina uzoraka pomoću dodaci koje možete dodati podršku za različite oblike ad odgovor. Ogledna ProgrammaticAdPage programski stupi u interakciju s instancu komponente reproduktor medijskih sadržaja.

###<a name="adpodpage-sample"></a>Ogledni AdPodPage

Ovaj primjer koristi u AdSchedulerPlugin da biste odredili kada prikazati u ad. U ovom primjeru usred snimljene fotografije oglašavanje zakazanog može reproducirati nakon 5 sekundi. Pod ad (grupa reklame da bi se prikazao u redoslijedu) navedeni su u VELIKU datoteku vratio poslužitelj za ad. URI VELIKU datoteku koja je navedena u na <RemoteAdSource> element.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
    
        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>
    
                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>
    
                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

Dodatne informacije o na AdSchedulerPlugin potražite u članku [oglašavanje u Framework reproduktora na sustavu Windows 8 i Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

###<a name="adschedulingpage"></a>AdSchedulingPage

Ovaj primjer koristi i na AdSchedulerPlugin. Ga rasporede tri reklame, stara snimljene fotografije ad, usred snimljene fotografije ad i nakon snimljene fotografije ad. URI za VAST za svaki ad je naveden u na <RemoteAdSource> element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>
    
                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


###<a name="freewheelpage"></a>FreeWheelPage

Ovaj primjer koristi FreeWheelPlugin koji određuje izvorišnog atributa koji određuje URI koja upućuje na SmartXML datoteku koja određuje ad sadržaja, kao i informacije o planiranju servisa Active Directory.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="mastpage"></a>MastPage

Ovaj primjer koristi MastSchedulerPlugin koja omogućuje korištenje MAST datoteke. Izvorišni atribut određuje mjesto datoteke MAST.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="programmaticadpage"></a>ProgrammaticAdPage

Ovaj primjer programski stupi u interakciju s na reproduktor medijskih sadržaja. Datoteka ProgrammaticAdPage.xaml instancira na reproduktor medijskih sadržaja:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

Datoteka ProgrammaticAdPage.xaml.cs stvara sustava AdHandlerPlugin dodaje TimelineMarker da biste odredili kada u ad treba prikazati, a zatim zbrojiti upravljački program za događaj MarkerReached učita RemoteAdSource navodeći URI VELIKU datoteku, a reproducira u ad.
    
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;
    
            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }
    
            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

###<a name="scheduleclippage"></a>ScheduleClipPage

Ovaj primjer koristi u AdSchedulerPlugin da biste zakazali usred snimljene fotografije ad navođenjem .wmv datoteku koja sadrži oglas.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearcompanionpage"></a>VastLinearCompanionPage

Ovaj primjer pokazuje kako koristiti u AdSchedulerPlugin da biste zakazali ad linearni usred snimljene fotografije s na pomoćnom ad. Na <RemoteAdSource> element određuje mjesto VELIKU datoteku.
    
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage

Ovaj primjer koristi AdSchedulerPlugin da biste zakazali u linearnom i bilo nelinearno ad. Mjesto VELIKU datoteke navedena je s na <RemoteAdSource> element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
                            
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vmappage"></a>VMAPPage

Ovaj uzoraka koristi u VmapSchedulerPlugin da biste zakazali reklame pomoću VMAP datoteke. URI VMAP datoteka je navedena u izvornog atributa u <VmapSchedulerPlugin> element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

##<a name="implementing-an-ios-video-player-with-ad-support"></a>Implementacijom iOS reproduktora videozapisa s podrškom za Ad


Platforme Microsoft Media: Reproduktora Framework za iOS sadrži skup probne aplikacije koji vam pokazuju kako implementirati reproduktora videosadržaja aplikacije pomoću framework. Možete preuzeti Framework reproduktora i primjere s [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework). Github stranica sadrži veze na Wiki koja sadrži dodatne informacije o framework reproduktora i Uvod u uzorku reproduktora: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).


###<a name="scheduling-ads-with-vmap"></a>Planiranje reklame s VMAP

Sljedeći primjer prikazuje način da biste zakazali reklame pomoću VMAP datoteke.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest
    
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

###<a name="scheduling-ads-with-vast"></a>Planiranje reklame s VAST

Sljedeći primjer pokazuje kako zakazivanje najnovije ad VELIKU povezivanja.
    
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
         
   Sljedeći primjer pokazuje kako zakazivanje Prijevremeni ad VELIKU povezivanja.
Primjer: 4 raspored za Prijevremeni povezivanje VELIKU ad //Download na VAST datoteka (! [ framework.adResolver downloadManifest: & manifesta withURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[prošao na samostalnom logFrameworkError];} još {adLinearTime.startTime = 7; adLinearTime.duration = 0;
        
        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

Sljedeći primjer prikazuje kako umetnuti servisa Active Directory pomoću gruba izrezivanje uređivanja (RCE)

    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

Sljedeći primjer prikazuje način za zakazivanje programa pod ad.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;
    
    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Sljedeći primjer prikazuje način da biste zakazali koje nisu ljepljive ad usred snimljene fotografije. Nisu ljepljive ad samo će se reproducirati kada bez obzira na bilo koje traže se izvodi u pregledniku.
    
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Sljedeći primjer prikazuje način da biste zakazali ljepljive ad usred snimljene fotografije. Ljepljive ad prikazat će se svaki put kada zbirka dostigne navedeni točka na vremenskoj traci videozapisa.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


Sljedeći primjer pokazuje kako zakazivanje nakon snimljene fotografije ad.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Sljedeći primjer pokazuje kako zakazivanje stara snimljene fotografije ad.
    
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Sljedeći primjer pokazuje kako zakazivanje usred snimljene fotografije prekriveno ad.
    
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
##<a name="see-also"></a>Vidi također

[Razvoj aplikacija reproduktora videosadržaja](media-services-develop-video-players.md)
