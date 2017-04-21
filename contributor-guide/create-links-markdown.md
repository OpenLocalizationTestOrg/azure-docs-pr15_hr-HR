<properties
   pageTitle="Stvaranje veza u člancima markdown" description="U članku se objašnjava kod crosslinks u markdown." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="02/03/2015" ms.author="tysonn" />

# <a name="linking-guidance-for-azure-technical-content"></a>Povezivanje smjernicama Azure Tehnički sadržaj

### <a name="links-from-one-acom-article-to-another"></a>Veza iz članka jedan ACOM na drugu

Da biste stvorili vezu na istoj razini članka tehničkoj ACOM još je jedan članak tehničkoj ACOM, upotrijebite sljedeću sintaksu veze:  

- Članak na veze za direktorij servisa na drugi članak u istom direktorij servisa:

  `[link text](article-name.md)`

- U članku veze iz servisa poddirektorij članka u korijenskom direktoriju:

  `[link text](../article-name.md)`

- Članak na veza direktorija korijenski članka u poddirektorij servisa: 

  `[link text](./service-directory/article-name.md)`

- Članak na veze za poddirektorij servisa na članak na drugog podimenika servisa:

  `[link text](../service-directory/article-name.md)`
 

## <a name="links-to-anchors"></a>Veze na sidra

Ne morate stvoriti sidra – automatski generirani na vrijeme za sve naslove H2 za objavljivanje. Samo što morate učiniti je stvaranje veza na odjeljke H2.

- Da biste se povezali naslova u istom članku:

  `[link](#the-text-of-the-H2-section-separated-by-hyphens)`  
  `[Create cache](#create-cache)`

- Da biste se povezali programa sidra u drugi članak u istom poddirektorij:

  `[link text](article-name.md#anchor-name)`
  `[Configure your profile](media-services-create-account.md#configure-your-profile)`

- Da biste se povezali programa sidra u drugog podimenika servisa:

  `[link text](../service-directory/article-name.md#anchor-name)`
  `[Configure your profile](../service-directory/media-services-create-account.md#configure-your-profile)`

Da biste automatizirali stvaranje veza u članci veze za automatski generirani sidro proces je [MarkdownAnchorLinkGenerator – alat za generiranje sidra veze za ACOM u ispravnom obliku](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/ACOMMarkdownAnchorLinkGenerator).

## <a name="links-from-includes"></a>Obuhvaća veze iz

Od obuhvaćaju datoteke nalaze u neki drugi direktorij, morat ćete koristiti više relativni putovi kao što je prikazano u nastavku. Da biste se povezali članak iz datoteke Uključi, koristite ovaj oblik:

    [link text](../articles/service-folder/article-name.md)
    
Saznajte više o tome kako koristiti Uključi datoteku u [Prilagođeni markdown proširenja smjernice](custom-markdown-extensions.md#includes).

## <a name="links-in-selectors"></a>Veze u Birači

Ako imate Birači ugrađeni u na Uključi, to koristite ovu vrstu povezivanja: 

    > [AZURE. BIRAČ popisa (Dropdown1 | Dropdown2])     -  [(tekst1 | Example1)](../articles/service-folder/article-name1.md)
    - [(tekst1 | Example2)](../articles/service-folder/article-name2.md)
    - [(tekst2 | Example3)](../articles/service-folder/article-name3.md)
    - [(tekst2 | Example4)](../articles/service-folder/article-name4.md)


## <a name="reference-style-links"></a>Stil reference veze

Da biste olakšali čitanje izvora sadržaja, poslužite se veze stil reference. Veza stil reference zamijeniti sintaksa vezu u istoj razini pojednostavljeni sintaksa koja omogućuje vam da biste premjestili dugo URL-ovi na kraju članka. Evo primjera Daring Fireball korisnika:

Tekst umetnutog:

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

Veza reference na kraju članka:

    <!--Reference links in article-->
    [1]: http://google.com/
    [2]: http://search.yahoo.com/  
    [3]: http://search.msn.com/

Obavezno uključite razmak nakon zarez prije vezu. Prilikom povezivanja s drugim tehničkih članaka ako zaboravite da biste dodali razmak, vezu će biti do oštećenja u objavljeni članak. 

## <a name="link-to-acom-pages-that-are-not-part-of-the-technical-documentation-set"></a>Veza na stranice ACOM koji nisu dio skupa tehničku dokumentaciju

Veza na stranicu na ACOM (kao što su cijene stranice, SLA stranice ili bilo što drugo koja nije dokumentaciju članak), koristite apsolutni URL, ali izostaviti regionalne postavke. Cilj ovdje je koji funkcioniraju veze u GitHub, a prikazanih web-mjesta:

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)


## <a name="link-to-msdn-or-technet"></a>Veza na MSDN i TechNet

Kada vam je potrebna veza na MSDN i TechNet, koristite potpunu vezu na temu, a zatim Ukloni na en-us jezik regionalne postavke veze. 

### <a name="use-friendly-link-text-for-all-links"></a>Korištenje tekst neslužbeni veze za sve veze

Riječi u vezu uključite mora biti neslužbeni – drugim riječima, trebali bi normalni engleskih riječi ili naslov stranice se povezujete. Nemojte koristiti "kliknite ovdje". Neispravan za SEO i odgovarajući način ne opisuju cilj.

**Ispravljanje:**

- `For more information, see the [contributor guide index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`

- `For more details, see the [SET TRANSACTION ISOLATION LEVEL](https://msdn.microsoft.com/library/ms173763.aspx) reference.`

**Netočan:**

- `For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).`

- `For more information, click [here](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`


## <a name="fwlinks"></a>Veze

Da biste izbjegli veze (naš sustav preusmjeravanje) u sadržaju azure.microsoft.com. Oni moraju se koristiti samo kao ne uspije kada je potrebno da biste stvorili vezu na stranicu čiji URL još ne znate. Gotovo nikad zapravo su potrebne. Za ACOM, kako biste znali što će biti unaprijed definirati naziv datoteke. Biblioteka teme koji još nisu objavljene, možete stvoriti vezu koja se koristi u temi GUID tako da ne morate koristiti je veza.

Ako koristite mora se veza na web-stranici, obuhvaćaju parametar P da biste trajno preusmjeravanje:

    http://go.microsoft.com/fwlink/p/?LinkId=389595

Kada zalijepite URL ciljne u alat za veza, imajte na umu da biste uklonili regionalne postavke ako je veza ciljne biblioteke ACOM, ili MSDN i TechNet.

## <a name="remember-the-azure-library-chrome"></a>Imajte na umu chrome Azure biblioteke!
Ako želite da biste se povezali programa Azure biblioteke temu koja se nalazi u odjeljku [čvor](https://msdn.microsoft.com/library/azure), imajte na umu da biste odredili na Azure chrome u vezi (/ azure /). Azure chrome dijeli Mogućnosti navigacije ACOM i prikazuje samo Azure sadržaj MSDN biblioteke. Ispravno opsegu vezu izgleda ovako:

    http://msdn.microsoft.com/library/azure/dd163896.aspx

U suprotnom stranice će može prikazati u standardnom prikazu MSDN s cijelog stabla MSDN prikazuju.

### <a name="contributors-guide-links"></a>Suradnici vodič veze

- [Članak](./../README.md)
- [Indeks članaka upute](./contributor-guide-index.md)

<!--image references-->
[1]: ./media/create-tables-markdown/table-markdown.png
[2]: ./media/create-tables-markdown/break-tables.png
