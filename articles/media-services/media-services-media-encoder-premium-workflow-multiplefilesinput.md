<properties
    pageTitle="Više ulaznih datoteka i svojstva komponente pomoću Premium Encoder | Microsoft Azure"
    description="U ovoj se temi objašnjava kako pomoću setRuntimeProperties koristiti više datoteka za unos i prenesite prilagođenih podataka media procesor tijeka rada za Media Encoder Premium."
    services="media-services"
    documentationCenter=""
    authors="xpouyat"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"  
    ms.author="xpouyat;anilmur;juliako"/>

# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Korištenje većeg broja datoteka za unos i svojstva komponente s Premium Encoder

## <a name="overview"></a>Pregled

Postoje scenariji u kojima možda potrebno da biste prilagodili svojstva komponente navedete isječak popis XML sadržaja, ili slanje više datoteka za unos kada pošaljete zadatka s procesorom medijskih sadržaja za **Tijek rada za Media Encoder Premium** . Primjeri su:

- Prekriva tekst na videozapis i postavljanje tekstne vrijednosti (na primjer, trenutni datum) prilikom izvođenja za svaki unos videozapis.
- Prilagodba popisa XML isječak (da biste odredili izvorne datoteke jedne ili više sa ili bez obrezivanja, itd.).
- Prekriva sliku logotipa na unos videozapisa dok se videozapis kodira.

Da biste omogućili **Premium tijeka rada za Media Encoder** znati koju mijenjate neka svojstva tijeka rada kada stvorite zadatak ili slanje više datoteka za unos, morate koristiti konfiguracije niz koji sadrži **setRuntimeProperties** i/ili **transcodeSource**. U ovoj se temi objašnjava kako ih koristiti.


## <a name="configuration-string-syntax"></a>Sintaksa niza za konfiguraciju

Konfiguracija niza da biste postavili u kodiranja zadatka koristi XML dokument koji izgleda ovako:

    <?xml version="1.0" encoding="utf-8"?>
    <transcodeRequest>
      <transcodeSource>
      </transcodeSource>
      <setRuntimeProperties>
        <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      </setRuntimeProperties>
    </transcodeRequest>


Ovo je C# kod koji čita XML konfiguraciju iz datoteke, a zatim prosljeđuje zadatka na zadatak:

    XDocument configurationXml = XDocument.Load(xmlFileName);
    IJob job = _context.Jobs.CreateWithSingleTask(
                                                  "Media Encoder Premium Workflow",
                                                  configurationXml.ToString(),
                                                  myAsset,
                                                  "Output asset",
                                                  AssetCreationOptions.None);


## <a name="customizing-component-properties"></a>Prilagodba svojstva komponente  

### <a name="property-with-a-simple-value"></a>Svojstvo jednostavne vrijednošću
U nekim slučajevima je korisno da biste prilagodili svojstvo komponente zajedno s datoteku tijeka rada koja će biti izvršio Premium tijeka rada za Media Encoder.

Pretpostavimo da dizajnu tijeka rada prekriva tekst na videozapise i tekst (na primjer, trenutni datum) bi trebao postaviti tijekom rada. To možete učiniti tako da pošaljete tekst da biste postavili kao nova vrijednost za svojstvo teksta preklapanja komponente iz kodiranja zadatka. U ovom mehanizam možete koristiti da biste promijenili druga svojstva komponente tijeka rada (primjerice na položaj ili boje preklapanja, brzina prijenosa AVC encoder, itd.).

**setRuntimeProperties** se koristi da biste nadjačali svojstvo u komponente tijeka rada.

Primjer:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Optional Overlay/Overlay/filename" value="MyLogo.png"/>
          <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
      </setRuntimeProperties>
    </transcodeRequest>


### <a name="property-with-an-xml-value"></a>Svojstvo s XML vrijednost

Da biste postavili svojstvo koje se očekuje XML vrijednost, Enkapsulacija pomoću `<![CDATA[ and ]]>`.

Primjer:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

>[AZURE.NOTE]Provjerite ne da biste stavili oznake kraja retka povrata tek nakon što instalirate `<![CDATA[`.


### <a name="propertypath-value"></a>vrijednost propertyPath

U prijašnjih primjera je na propertyPath "/ medijskih datoteka unos/NazivDatoteke" ili "/ inactiveTimeout" ili "clipListXml".
To je Općenito, naziv komponente, a zatim naziv svojstva. Put može imati više ili manje razine, poput "/ primarySourceFile" (jer je svojstvo korijenu tijek rada) ili "/ Video prekriveno neprozirnost obrada/grafički" (jer je preklapanja u grupi).    

Da biste provjerili put i svojstva naziv, koristite akcijski gumb koji se nalazi odmah pokraj svakog svojstva. Možete kliknite ovaj gumb Akcije i odaberite **Uredi**. Time ćete prikazati stvarni naziv svojstva, a odmah iznad nje naziva.

![Akcija/uređivanje](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Svojstvo](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Više datoteka za unos

Svaki zadatak koji pošaljete **Premium tijeka rada za Media Encoder** traži dva resursi:

- Prvi je *Resursa tijeka rada* koja sadrži datoteku za tijek rada. Datoteka za tijek rada možete dizajnirati pomoću [Dizajnera tijeka rada](media-services-workflow-designer.md).
- Drugu je *Medijski sadržaj* koji sadrži medijskih datoteka koju želite šifrirati.

Kada šaljete više medijske datoteke encoder **Tijeka rada za Media Encoder Premium** , primjenjuju se sljedeća ograničenja:

- Sve medijske datoteke mora biti u istoj *Medijski sadržaj*. Korištenje više medijskih sadržaja nije podržano.
- Morate postaviti primarni datoteku u ovom medijski sadržaj (najbolje je glavni video datoteka kodiranje će se od vas zatraži da obradi).
- Je potrebno za prijenos podataka konfiguracije koja sadrži element **setRuntimeProperties** i/ili **transcodeSource** procesor.
  - **setRuntimeProperties** se koristi da biste nadjačali svojstvo naziv datoteke ili neki drugi svojstvo u komponente tijeka rada.
  - **transcodeSource** se koristi za određivanje XML popis isječak sadržaj.

Veze u tijeku rada:

 - Ako koristite jednu ili nekoliko komponenti medijske datoteke unos i planiranje da biste koristili **setRuntimeProperties** odredite naziv datoteke, zatim povezivanje primarnoj datoteci komponente PIN-a na njih. Provjerite da ne postoji veza između objekt primarni datoteke i datoteke Input(s) medijske sadržaje.
 - Ako biste radije da biste koristili XML popis isječka i jedan Media izvorišnu komponentu, pa možete povezati i zajedno.

![Bez veze s primarni izvornu datoteku da biste medijske datoteke unos](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Ne postoji veza s primarnu datoteku da biste medijske datoteke unos komponente ako koristite setRuntimeProperties da biste postavili svojstvo naziv datoteke.*

![Veza s popisa isječak XML za izrezivanje izvora popisa](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Možete povezivanje s izvorom medijskih sadržaja u XML popis isječka i koristiti transcodeSource.*


### <a name="clip-list-xml-customization"></a>Prilagodba popisa XML za isječke
Možete odrediti XML popis isječak u tijeku rada prilikom izvođenja pomoću **transcodeSource** u nizu za konfiguraciju XML. Potreban je pin XML popis isječak biti povezani s komponentu izvor medija u tijeku rada.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <transcodeSource>
          <clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
          </clipList>
        </transcodeSource>
        <setRuntimeProperties>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Ako želite da biste odredili /primarySourceFile koristiti ovo svojstvo dati naziv Izlazna datoteka pomoću "Izraza", pa preporučujemo prosljeđivanje XML popis isječak kao svojstvo *nakon* svojstvo /primarySourceFile da biste izbjegli popisu isječak mogu nadjačati postavke /primarySourceFile.

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Pomoću dodatnih okvira točne skraćivanje:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
               <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


## <a name="example"></a>Primjer

Primjer u kojem želite preklapanja sliku logotipa na unos videozapisa dok se videozapis kodira uzmite u obzir. U ovom primjeru unos videozapis pod nazivom "MyInputVideo.mp4" i logotip pod nazivom "MyLogo.png". Treba poduzeti sljedeće korake:

- Stvaranje tijeka rada resursa s datotekom tijeka rada (pogledajte u sljedećem primjeru).
- Stvaranje medijskih resursa koji sadrži dvije datoteke: MyInputVideo.mp4 kao primarni datoteka i MyLogo.png.
- Slanje zadatka procesor media tijeka rada za Media Encoder Premium s iznad resursi za unos i navedite sljedeće konfiguracije niz.

Konfiguracija:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="MyLogo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


U gornjem primjeru naziv datoteke videozapisa šalju komponentu medijske datoteke unos i svojstvo primarySourceFile. Naziv datoteke logotipa šalje se na drugi unos datoteke medijskih sadržaja koji je povezan s komponentom grafika preklapanje.

>[AZURE.NOTE]Svojstvo primarySourceFile šalje se naziv datoteke videozapisa. Razlog za to je koristiti ovo svojstvo u tijeku rada za izgradnju naziv odgovarajuće izlazne datoteke pomoću izraza, primjerice.


### <a name="step-by-step-workflow-creation-that-overlays-a-logo-on-top-of-the-video"></a>Stvaranje tijeka rada za detaljne taj prekrivanja a logotip pri vrhu videozapisa     

Evo nekoliko koraka da biste stvorili tijek rada koji uzima dvije datoteke kao unos: videozapisa i slika. To će preko slike pri vrhu videozapisa.

Otvorili **Dizajner tijeka rada** , a zatim odaberite **datoteku** > **Novog radnog prostora** > **Transcode nacrt**.

Novi tijek rada prikazuje tri elementa:

- Primarni izvorišna datoteka
- Popis XML isječaka
- Datoteka resursa Izlaz  

![Novi tijek rada kodiranja](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Novi tijek rada kodiranja*


Da biste prihvatili unos medijske datoteke, započnite s dodavanjem komponentu medijske datoteke unosa. Da biste dodali komponentu tijek rada, potražite ga u okvir za pretraživanje u spremištu i povucite željenu stavku u okno dizajnera.

Zatim dodajte videodatoteku koja će se koristiti za dizajniranje tijekova rada. Da biste to učinili, kliknite okno pozadine u dizajneru tijeka rada i pogledajte svojstvo primarni izvornu datoteku u oknu svojstva na desnoj strani. Kliknite ikonu mape, a zatim odaberite odgovarajuće videodatoteka.

![Izvor primarni datoteke](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Izvor primarni datoteke*


Nakon toga navedite video datoteka u komponenti za unos medijske datoteke.   

![Medijskih datoteka ulazni izvor](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Medijskih datoteka ulazni izvor*


Čim to možete učiniti, komponentu medijske datoteke unos će pregledati datoteku i popunjavanje njegov izlaz PIN-ove u skladu s vizualnim datoteku koju je provjerava ima li.

Sljedeći je korak da biste dodali na "videozapis podataka vrsta ažuriranje programa" da biste naveli boju prostora za Rec.709. Dodavanje "Videozapis oblik pretvornika" koje je postavljeno na vrstu izgleda raspored podataka = konfigurirati planarnim. To će pretvoriti strujanje videozapisa u obliku koji se može preuzeti kao izvor podataka za komponentu preklapanje.

![video ažuriranje programa vrsta podataka i oblikovanje pretvornik](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Ažuriranje programa vrsta videozapisa podataka i oblikovanje pretvornik*

![Vrsta izgleda = konfigurirati planarnim](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Vrsta izgleda je konfigurirati planarnim*

Zatim dodajte komponentu videozapisa preko i povezati (koje nisu komprimirane) videozapisa PIN-a (koje nisu komprimirane) PIN-a za video datoteka unosa medijske sadržaje.

Dodavanje drugog medijske datoteke unos (da biste učitali datoteke logotipa) kliknite na ovoj komponenti i preimenujte u "Medijske datoteke unos logotip" pa odaberite sliku (.png datoteka, primjerice) u svojstvu datoteke. Povezivanje slike koje nisu komprimirane pin PIN-a slike koje nisu komprimirane preklapanja.

![Preklapanje komponente i slikovne datoteke izvora](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Preklapanje komponente i slikovne datoteke izvora*


Ako želite izmijeniti položaj logotipa na videozapisu (na primjer, možda ćete morati položaj od 10 posto s gornjem lijevom kutu videozapis), poništite potvrdni okvir "Ručni unos". To možete učiniti jer koristite ulaz medijskih sadržaja možete unijeti datoteke logotipa s komponentom preklapanje.

![Položaj preklapanja](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Položaj preklapanja*


Da biste kodiranje strujanje videozapisa da biste H.264, dodajte encoder komponente AVC videozapisa kodiranje i AAC dizajnera površina. Povezivanje s PIN-ove.
Postavljanje AAC kodiranje, a zatim odaberite zadana postava/Audio oblik pretvorbe: 2.0 (L, R).

![Audio i Video koderi](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Audio i Video koderi*


Sada dodajte komponente **ISO Mpeg-4 Multiplexer** i **Izlaz datoteke** i povezivanje PIN-ove kao što je prikazano.

![MP4 multiplexer i izlazna datoteka](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 multiplexer i izlazna datoteka*


Morate postaviti naziv Izlazna datoteka. Kliknite **Izlaz datoteke** komponentu i uređivati izraz za datoteku:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Naziv datoteke Izlaz](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Naziv datoteke Izlaz*

Možete pokrenuti tijek rada lokalno da biste provjerili ispravno funkcioniranje.

Kada završi, možete je pokrenuti u servisa Azure Media Services.

Prvo pripremite resursa u servisa Azure Media Services s dva datotekama u njoj: video datoteka i logotip. To možete učiniti pomoću .NET ili REST API-JA. To možete učiniti i pomoću portala za Azure ili [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Pomoću ovog praktičnog vodiča objašnjava Upravljanje resursima pomoću AMSE. Da biste dodali datoteke imovine na dva načina:

- Stvorite lokalnu mapu, kopirajte dvije datoteke u nju i povucite i ispustite u mapu na karticu **resursa** .
- Prijenos videozapisa datoteke kao sredstvo, prikazuju se podaci resursa, idite na karticu Datoteka, a prijenos dodatne datoteke (logotip).

>[AZURE.NOTE]Provjerite je li Postavljanje primarnog datoteke u resursa (glavni video datoteka).

![Datoteka resursa u AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Datoteka resursa u AMSE*


Odaberite sredstvo, a zatim na kodiranje s Premium Encoder. Prijenos tijeka rada, a zatim ga odaberite.

Kliknite gumb za prosljeđivanje podataka procesora i dodali XML za sljedeće da biste postavili svojstva za izvođenje:

![Kodiranje Premium u AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Kodiranje Premium u AMSE*


Zalijepite sljedeći XML podataka. Morate navesti naziv video datoteka za unos medijske datoteke i primarySourceFile. Previše odredite naziv naziv datoteke za logotip.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*


Ako koristite .NET SDK za stvaranje i pokretanje zadatka, ova XML podataka mora proslijediti kao niz konfiguracije.

    public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);

Nakon dovršetka posla, MP4 datoteke u izlaz resursa prikazuje preklapanja!

![Preklapanja na videozapisu](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Preklapanja na videozapisu*


Primjer tijeka rada možete preuzeti iz [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).


## <a name="see-also"></a>Vidi također

- [Uvod u Premium kodiranje Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

- [Korištenje Premium kodiranje u servisa Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

- [Kodiranje osvježavati sadržaja pomoću servisa Azure Media Services](media-services-encode-asset.md#media_encoder_premium_workflow)

- [Tijek rada za Media Encoder Premium oblici i kodeka](media-services-premium-workflow-encoder-formats.md)

- [Ogledne datoteke tijeka rada](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)

- [Alat za Azure Media Services Explorer](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
