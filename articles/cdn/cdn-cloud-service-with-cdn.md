<properties
    pageTitle="Integriranje servis u oblaku s Azure CDN | Microsoft Azure"
    description="Praktični vodič ručica za implementaciju servis u oblaku koji služi sadržaj s krajnje integrirani Azure CDN"
    services="cdn, cloud-services"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor="tysonn"/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="intro"></a>Integriranje servis u oblaku s Azure CDN

Servis u oblaku može se integrirati s CDN Azure posluživanje sav sadržaj sa servisom cloud mjesta. Taj se način pruža sljedeće prednosti:

- Uvođenje i ažurirati slike, skripte i listovi stilova u oblaka servisa project direktorija
- Jednostavno nadograditi NuGet paketa u vaš servis u oblaku, kao što su jQuery ili Samopokretanje verzije
- Upravljanje web-aplikacije i vaše CDN poslužena sadržaja sve iz istog sučelja Visual Studio
- Sjedinjene implementacije tijeka rada za web-aplikacije i sadržaju CDN poslužena
- Integriranje usnopljavanje ASP.NET i minification s Azure CDN

## <a name="what-you-will-learn"></a>Što ćete saznati ##

U ovom ćete praktičnom vodiču ćete saznati kako:

-   [Integracija krajnje Azure CDN s servis u oblaku i koristi statički sadržaj u web-stranice iz Azure CDN](#deploy)
-   [Konfiguriranje postavki predmemorije za statički sadržaj u servis u oblaku](#caching)
-   [Posluživanje sadržaja iz kontroler akcije kroz Azure CDN](#controller)
-   [Posluženo vezanoj instalaciji i minified sadržaja putem Azure CDN uz čuvanje skripte ispravljanje pogrešaka iskustvom u Visual Studio](#bundling)
-   [Konfiguriranje pričuvne skripte i CSS-a kada je izvan mreže vaše CDN Azure](#fallback)

## <a name="what-you-will-build"></a>Što će sastavljanje ##

Će implementacije u oblak servisa Web ulogu pomoću zadanog predloška ASP.NET MVC, dodavanje koda za posluživanje sadržaja s Integriranom CDN Azure, kao što su slike, kontroler akcija rezultata i zadane JavaScript i CSS datoteka, i i pisanje koda za konfiguriranje pričuvni mehanizam objedinjuje poslužena za slučaj da se CDN je izvan mreže.

## <a name="what-you-will-need"></a>Što ćete ##

Pomoću ovog praktičnog vodiča sadrži sljedeće preduvjete:

-   Aktivni [račun sustava Microsoft Azure](/account/)
-   Visual Studio 2015 s [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)

> [AZURE.NOTE] Potreban vam je račun za Azure da biste dovršili ovaj Praktični vodič:
> + Možete [besplatno otvorite račun za Azure](/pricing/free-trial/) - dobijete kredita možete koristiti da biste isprobali plaćenu servisa Azure i čak i kada se koriste najviše možete zadržati račun i korištenje slobodno Azure servisa, kao što su web-mjesta.
> + Možete [aktivirati pogodnosti pretplatnika MSDN](/pricing/member-offers/msdn-benefits-details/) - MSDN vaša pretplata vam kredita svakog mjeseca, koje možete koristiti za plaćenu Azure servise.

<a name="deploy"></a>
## <a name="deploy-a-cloud-service"></a>Implementacija servis u oblaku ##

U ovom se odjeljku će implementacija zadani predložak aplikacije ASP.NET MVC u Visual Studio 2015 oblaka uloge servisa Web, a zatim integrirati s krajnju točku CDN. Slijedite upute u nastavku:

1. U Visual Studio 2015, stvorite novog servisa Azure oblak na traci izbornika tako da odaberete **Datoteka > novo > projekt > oblaka > Azure u Oblaku**. Dajte naziv, a zatim kliknite **u redu**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)

2. Odaberite **Ulogu Web ASP.NET** , a zatim kliknite u **>** gumb. Kliknite u redu.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)

3. Odaberite **MVC** , a zatim kliknite **u redu**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)

4. Sada, objavite ta uloga Web sa servisom Azure oblaka. Desnom tipkom miša kliknite servis projekta oblaka, a zatim odaberite **Objavi**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)

5. Ako ste niste prijavljeni u Microsoft Azure, kliknite **Dodaj račun...** padajući izbornik, a zatim stavku izbornika **Dodaj račun** .

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)

6. Na stranici prijava, prijavite se pomoću Microsoftova računa koji ste koristili da biste aktivirali račun za Azure.
7. Kada se prijavite, kliknite **Dalje**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)

8. Uz pretpostavku da niste stvorili oblaka račun servisa ili prostora za pohranu, Visual Studio će vam pomoći da oboje. U dijaloškom okviru **Stvaranje servis u Oblaku i račun** upišite naziv željenog servisa, a zatim odaberite željeno područje. Nakon toga kliknite **Stvori**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)

9. Na stranici postavke objavljivanja Provjerite konfiguraciju, a zatim kliknite **Objavi**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)

    >[AZURE.NOTE] Simbol za servise u oblaku traje predugo. Za omogućivanje Web implementacije sve uloge mogućnost možete napraviti ispravljanje pogrešaka na servisu u oblaku mnogo brže unosom brzo (ali privremene) ažuriranja ulogama na Web. Dodatne informacije o tu mogućnost, potražite u članku [Objavljivanje neki servis u Oblaku pomoću alata za Azure](http://msdn.microsoft.com/library/ff683672.aspx).

    Kada **Microsoft Azure aktivnosti zapisnika** pokazuje da objavljivanje status **Dovršeno**, stvarate CDN krajnju točku koja je integriran s ovaj servis u oblaku.

    >[AZURE.WARNING] Ako nakon objavljivanja, distribuiranih oblaku prikazuje se pogreška zaslona, vjerojatno je jer ste implementiran servisa u oblaku koristi [goste OS koji ne uključuje .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  Taj se problem možete riješiti uvođenjem [.NET 4.5.2 zadatak pokretanja](../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="create-a-new-cdn-profile"></a>Stvaranje novog profila CDN

Zbirka krajnje točke CDN je CDN profila.  Svaki profil sadrži jednu ili više CDN krajnje točke.  Možda želite koristiti više profila da biste organizirali CDN krajnje točke domene internet, web-aplikacije ili nekog drugog kriterija.

> [AZURE.TIP] Ako već imate CDN profil koji želite koristiti za ovaj vodič, prijeđite na [Stvaranje krajnju točku CDN](#create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Stvaranje nove CDN krajnje točke

**Da biste stvorili krajnju CDN za vaš račun za pohranu**

1. [Portal za upravljanje Azure](https://portal.azure.com)dođite do CDN profila.  Koje možda imaju prikvačena ga na nadzornu ploču u prethodnom koraku.  Ako vam ne možete pronaći ga tako da kliknete **Pregled**, a zatim **CDN profila**, a na profilu planirate dodati na krajnjoj točki za.

    Pojavit će se profil plohu CDN-a.

    ![CDN profila][cdn-profile-settings]

2. Kliknite gumb **Dodaj krajnjoj točki** .

    ![Dodavanje gumba za krajnje točke][cdn-new-endpoint-button]

    Pojavit će se plohu **Dodavanje krajnje točke** .

    ![Dodavanje plohu krajnje točke][cdn-add-endpoint]

3. Unesite **naziv** za ovaj CDN krajnjoj točki.  Ovaj naziv će se koristiti za pristup predmemorirani resursa na domeni `<EndpointName>.azureedge.net`.

4. Na padajućem popisu **Vrsta izvora** odaberite *servis u Oblaku*.  

5. Na padajućem popisu **polazište naziv glavnog računala** odaberite servis u oblaku.

6. Ostavite zadane vrijednosti za **polazište put**, **izvor zaglavlja glavnog računala**i **protokol/polazište priključak**.  Morate navesti barem jedan protokol (HTTP ili HTTPS).

7. Kliknite gumb **Dodaj** da biste stvorili novi krajnjoj točki.

8. Nakon stvaranja krajnju točku, prikazuje se popis krajnje točke profila. Prikaz popisa prikazuje se URL koristi za pristup predmemoriranog sadržaja, kao i domenu izvor.

    ![CDN krajnje točke][cdn-endpoint-success]

    > [AZURE.NOTE] Krajnja točka odmah neće moći koristiti.  To može potrajati i do 90 minuta za registraciju za prijenos putem mreže CDN. Korisnici koji pokušaju odmah koristiti naziv domene CDN primiti Šifra stanja 404 dok ne bude dostupno putem na CDN sadržaj.

## <a name="test-the-cdn-endpoint"></a>Testiranje CDN krajnja točka

Kada je objavljivanje status **Dovršeno**, otvorite prozor preglednika, a zatim je otvorite * *http://<cdnName>*.azureedge.net/Content/bootstrap.css**. U moje Postavi ovaj je URL:

    http://camservice.azureedge.net/Content/bootstrap.css

Koji odgovara sljedeći URL polazište na krajnjoj točki CDN:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Kada dođete do * *http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, ovisno o pregledniku, zatražit će se za preuzimanje i otvaranje bootstrap.css koju ste dobili iz objavljenu web-aplikacije.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

Na sličan način možete pristupiti bilo koji javno dostupnu URL pri * *http://*&lt;naziv servisa >*.cloudapp.net/** izravno iz vaše CDN krajnjoj točki. Ako, na primjer:

-   Datoteke .js put /Script
-   Bilo koji sadržaj datoteke iz sections put
-   Bilo kakva kontroler/akcija
-   Ako je niz upita omogućena na krajnjoj točki CDN, bilo koji URL s nizovima upita

Zapravo, u konfiguraciji iznad možete hostirati cijelu oblaka servisa * *http://*&lt;cdnName >*.azureedge.net/**. Ako se dođite do **http://camservice.azureedge.net/ **, pristup rezultat akciju s Polazno/indeksa.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

To ne znači, no da uvijek je dobro (ili obično dobro) da bi služio cijeli oblaku putem Azure CDN. Neke se upozorenja su:

-   Taj se način zahtijeva cijelo web-mjesto biti javno, jer Azure CDN trenutno ne može poslužiti sve sadržaje za privatne.
-   Ako krajnju točku CDN izvanmrežne dolazi iz bilo kojeg razloga, hoće li održavanje ili pogreška korisničkog cijeli oblaku dolazi izvanmrežne osim u slučaju da kupci biti preusmjereni URL polazište * *http://*&lt;naziv servisa >*.cloudapp.net/**.
-   Čak i uz prilagođene predmemorijom postavke (pročitajte članak [Konfiguriranje predmemoriranja mogućnosti za statične datoteke na servis u oblaku](#caching)), krajnjoj točki CDN poboljšati performanse visoko-dinamični sadržaj. Ako ste pokušali učitati na početnu stranicu iz na krajnjoj točki CDN kao što je prikazano gore, Uočite da se snimljene najmanje 5 sekundi da biste učitali zadanom početnom stranicom prvi put, koja je prilično jednostavan stranice. Zamislite što će se dogoditi sa sučelje klijenta ako ova stranica sadrži dinamičkog sadržaja koji morate ažurirati svake minute. Posluživanje dinamičkog sadržaja na krajnjoj točki CDN zahtijeva isteka kratki predmemorije koja se prevodi neuspjele akcije u predmemoriji Česti na krajnjoj točki CDN. Polarni performanse servis u oblaku i defeats Svrha ustvari CDN.

Druga je da biste odredili koji sadržaj da bi služio iz Azure CDN na temelju slučaj velikim slovom u servis u oblaku. Na kraju, već ste vidjeti kako pristupati pojedinačne datoteke sadržaja iz krajnju točku CDN. Li će vam pokazati kako vam poslužiti određene kontroler akcija putem krajnju točku CDN u [posluživanje sadržaja iz kontroler akcije kroz Azure CDN](#controller).

<a name="caching"></a>
## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>Konfigurirajte predmemoriranja statičke datoteke na servis u oblaku ##

Pomoću Azure CDN integraciju u servis u oblaku, možete odrediti način na koji želite statički sadržaj predmemoriju u krajnju točku CDN. Da biste to učinili, otvorite *Web.config* iz uloge projekta Web (npr. WebRole1) i dodajte je `<staticContent>` element `<system.webServer>`. XML ispod konfigurira predmemoriju istječe 3 dana.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Kada to učinite, sve statičke datoteke na servis u oblaku Primijetit ćete na istom pravilo u predmemoriju CDN. Precizniji kontrolu nad postavke predmemorije dodavanje *Web.config* datoteke u mapu i dodavanje vaše postavke. Ako, na primjer, dodati *Web.config* datoteke u mapu *\Content* i zamijenite sadržaj s XML za sljedeće:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Ta postavka uzrokuje statičke datoteke iz mape *\Content* predmemoriju za petnaest dana.

Dodatne informacije o konfiguriranju na `<clientCache>` element, potražite u članku [klijent predmemorije &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

U [posluživanje sadržaja iz kontroler akcije kroz Azure CDN](#controller), li i vidjet ćete kako konfigurirati postavke predmemorije za kontroler akcija rezultate u predmemoriji CDN.

<a name="controller"></a>
## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Posluživanje sadržaja iz kontroler akcije kroz Azure CDN ##

Kad Web uloge servisa oblaka integrirati s Azure CDN, preporučuje se relativno lako posluživanje sadržaja iz kontroler akcije kroz Azure CDN. Osim posluživanje oblaku izravno putem Azure CDN (u primjeru iznad), [Maarten Balliauw](https://twitter.com/maartenballiauw) prikazuje kako to učiniti s posla MemeGenerator kontroler u [skraćivanje Latencija na webu s Azure CDN](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN). Koje će jednostavno ponoviti je ovdje.

Pretpostavimo da na servisu oblaka želite generirati memes na osnovi malu slike Chuck Norris (fotografija po [Gordan svijetlo](http://www.flickr.com/photos/alan-light/218493788/)) ovako:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

Imate jednostavan `Index` akcije koji omogućuju korisnicima da biste odredili na superlativa na slici, zatim generira na meme kada ih objavite na akciju. Budući da je Chuck Norris, što biste očekivali ove stranice da biste postaju wildly popularne globalno. Ovo je dobar primjer posluživanje djelomično dinamičkog sadržaja s Azure CDN.

Slijedite korake za postavljanje ovu akciju kontroler:

1. U mapi *\Controllers* stvoriti novu .cs datoteku pod nazivom *MemeGeneratorController.cs* i sadržaj zamijenite sljedeći kod. Ne zaboravite da biste zamijenili istaknute dio naziva CDN.  

        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;

        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();

                public ActionResult Index()
                {
                    return View();
                }

                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }

                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }

                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }

                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }

                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);

                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }

                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }

                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);

                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }

                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }

2. Desnom tipkom miša kliknite zadani `Index()` akcije i odaberite **Dodaj prikaz**.

    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)

3.  Prihvatite donje postavke, a zatim kliknite **Dodaj**.

    ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)

4. Otvorite novi *Views\MemeGenerator\Index.cshtml* i sadržaj zamijenite sljedeće jednostavne HTML-a na superlativa predavanje:

        <h2>Meme Generator</h2>

        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>

5. Ponovno objavite servisa u oblaku, a zatim otvorite * *http://*&lt;naziv servisa >*.cloudapp.net/MemeGenerator/Index** u pregledniku.

Kada pošaljete vrijednosti obrasca `/MemeGenerator/Index`, `Index_Post` akcije metoda vraća vezu na `Show` akcije metoda s odgovarajući identifikator za unos. Kada kliknete vezu, dođete do sljedeći kod:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Ako je vaše lokalne ispravljanje, će dobiti Obični prikaz pogrešaka doživljaj lokalne preusmjeravanje. Ako je pokrenut na servis u oblaku, on će preusmjeriti da biste:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Koji odgovara sljedeći URL polazište na krajnjoj točki vaše CDN:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


Možete koristiti u `OutputCacheAttribute` atribut na na `Generate` način da biste odredili kako rezultat akcije treba predmemoriju, koji će poštovati Azure CDN. Kod u nastavku odredite predmemorije isteka od jednog sata (u sekundama 3,600).

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Isto tako, koji vam može poslužiti se sadržaj iz ništa kontroler u servis u oblaku putem Azure CDN, s željenu mogućnost spremanja.

U sljedećem odjeljku, I vidjet ćete kako vam poslužiti paketu i minified skripte i CSS-a putem Azure CDN.

<a name="bundling"></a>
## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>Integriranje usnopljavanje ASP.NET i minification s Azure CDN ##

Listovi stilova skripte i CSS-a promijenite diskovni te su značajke prime kandidata za Azure CDN predmemoriju. Posluživanje cijelu Web uloga putem vaše CDN Azure je najlakše ćete usnopljavanje i minification integrirati Azure CDN. Međutim, kao što je možda ne želite da biste to učinili, I vidjet ćete kako to učiniti dok očuvanje željeni develper sučelje platforme ASP.NET usnopljavanje i minification, kao što su:

-   Sučelje za način rada sjajno ispravljanje pogrešaka
-   Pojednostavnjeno implementacije
-   Odmah ažuriranja klijenata za skripte/CSS-a verziju nadogradnje
-   Pričuvni mehanizam kada na krajnjoj točki CDN ne uspije
-   Minimiziranje kod izmjene

U projektu **WebRole1** koju ste stvorili u [Integrate krajnje Azure CDN s Azure web-mjesta i posluženo statički sadržaj u web-stranica Azure CDN](#deploy), otvorite *App_Start\BundleConfig.cs* , a zatim pogledajte u `bundles.Add()` način pozive.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

Prvi `bundles.Add()` izjava dodaje paket za skripte na virtualnog direktorija `~/bundles/jquery`. Zatim otvorite *Views\Shared\_Layout.cshtml* da biste vidjeli kako će se oznaka paket skripte prikazati. Trebali biste moći pronaći sljedeći redak koda platforma Razor:

    @Scripts.Render("~/bundles/jquery")

Kod platforma Razor izvršavanja u ulozi Azure Web će prikazati na `<script>` oznaka za paket skripte otprilike ovako:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Međutim, kad se izvršava u Visual Studio tako da upišete `F5`, on će prikazati svaki skriptna datoteka u na paket pojedinačno (u slučaju iznad, samo jedan skriptna datoteka se na paket):

    <script src="/Scripts/jquery-1.10.2.js"></script>

To vam omogućuje da ispravljanje pogrešaka JavaScript kod u okruženje za razvoj dok smanjivanje Istodobni klijentske veze (usnopljavanje) i poboljšanje datoteke Preuzmi performansi (minification) u radni. Je odlične značajke da biste sačuvali uz integraciju Azure CDN. Osim toga, budući da prikazanih paket već sadrži niz za automatski generirani verziju, želite replicirati tu funkcionalnost tako da se kada ažurirate verziji jQuery kroz NuGet se može ažurirati na strani klijenta čim.

Slijedite korake u nastavku usnopljavanje ASP.NET integracije i minification s vaše CDN krajnjoj točki.

1. Vratite se u *App_Start\BundleConfig.cs*izmjena na `bundles.Add()` metode koje ćete koristiti u različitim [Graditelj paket](http://msdn.microsoft.com/library/jj646464.aspx), koji određuje CDN adresu. Da biste to učinili, zamijenite na `RegisterBundles` definiciju način s sljedeći kod:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Ne zaboravite da biste zamijenili `<yourCDNName>` s nazivom svoje CDN Azure.

    Običan riječima postavljate `bundles.UseCdn = true` i dodati pažljivo sastaviti CDN URL-a za svaki paket. Na primjer, prvi Graditelj kod:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))

    jednak je:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))

    Ovaj Graditelj govore usnopljavanje ASP.NET i minification prikaz pojedinačnih skripte datotekama debugged lokalno, no pristup skripte u pitanju pomoću navedena adresa CDN. Međutim, imajte na umu dvije važne značajke ovaj pažljivo sastaviti CDN URL-om:

    -   URL-a CDN potječe `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, što je zapravo virtualnog direktorija paket skripte na servis u oblaku.
    -   Budući da koristite CDN Graditelj, oznaku CDN skripte za na paket više ne sadrži niz automatski generirani verzije u prikazanih URL-u. Ručno morate Generiranje niza za jedinstveni verziju svaki put kada se izmjene paket skriptu da biste nametnuli je neuspješno pronalaženje u predmemoriji na vašem CDN Azure. U isto vrijeme jedinstveni verziju niz morate ostaju iste kroz vijek implementacije Maksimiziranje pronalaženje objekata u predmemoriji na vašem CDN Azure kada je implementiran u paket.
    -   Niz upita v = < W.X.Y.Z > povlači iz *Properties\AssemblyInfo.cs* u projektu uloga Web. Možete imati tijeka rada za implementaciju obuhvaća povećava skupa verziju svaki put kada objavljujete Azure. Ili *Properties\AssemblyInfo.cs* možete promijeniti samo u projektu da biste automatski povećali niz verzije prilikom svakog stvaranja, koristeći zamjenske znakove "*". Ako, na primjer:

            [assembly: AssemblyVersion("1.0.0.*")]

        Ostale strategije kako biste pojednostavili generiranje jedinstveni niz zauvijek implementacija funkcionirat će u nastavku.

3. Ponovno objaviti u oblaku i pristup na početnu stranicu.

4. Prikaz HTML kod za stranicu. Trebali biste moći vidjeti URL CDN prikazivanju, s nizom jedinstveni verziju svaki put kada ponovno objaviti promjene na servis u oblaku. Ako, na primjer:  

        ...

        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>

        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>

        ...

        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>

        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>

        ...

5. U Visual Studio ispravljanje pogrešaka u oblaku u Visual Studio tako da upišete `F5`.,

6. Prikaz HTML kod za stranicu. Prikazat će se i dalje svaki skriptna datoteka pojedinačno prikazati tako da imate dosljedan ispravljanje pogrešaka iskustvo u Visual Studio.  

        ...

            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>

            <script src="/Scripts/modernizr-2.6.2.js"></script>

        ...

            <script src="/Scripts/jquery-1.10.2.js"></script>

            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>

        ...   

<a name="fallback"></a>
## <a name="fallback-mechanism-for-cdn-urls"></a>Pričuvni mehanizam CDN URL-ova ##

Ako iz bilo kojeg razloga ne uspije na krajnjoj točki Azure CDN, želite web-stranici dovoljno pametno za pristup web-poslužitelju polazište kao pričuvni mogućnost za učitavanje JavaScript ili samopokretanja programa. Nije dovoljno ozbiljna izgubiti slike na web-mjestu zbog nedostupnosti CDN, ali puno raznih gubitak funkcionalnosti presudne stranica nudi skripte i listovi stilova.

Klase [paket](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) sadrži svojstvo naziva [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) omogućuje vam da biste konfigurirali pričuvni mehanizam CDN pogreške. Da biste koristili to svojstvo, slijedite ove korake:

1. U projektu uloga Web, otvorite *App_Start\BundleConfig.cs*, gdje ste dodali CDN URL-a u svaki [paket Graditelj](http://msdn.microsoft.com/library/jj646464.aspx), i izvršite sljedeće promjene istaknuti da biste dodali pričuvni mehanizam objedinjuje zadane:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                "~/Scripts/bootstrap.js",
                                "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Kada `CdnFallbackExpression` je vrijednost nije null, skripte je umetnutog u HTML da biste testirali hoće li se na paket uspješno učitan i ako nije, u paket izravno pristupiti u web-poslužitelj polazište. Ovo svojstvo mora biti postavljena na JavaScript izraz koji provjerava je li odgovarajuća paket CDN pravilno učitati. Izraz koji je potrebno da biste testirali svaki paket razlikuje se prema sadržaju. Za na objedinjuje zadani iznad:

    -   `window.jquery`je li u jquery-{verzija} .js definiran
    -   `$.validator`je li u jquery.validate.js definiran
    -   `window.Modernizr`je li u modernizer-{verzija} .js definiran
    -   `$.fn.modal`je li u bootstrap.js definiran

    Možda ste primijetili da se nije postavljen CdnFallbackExpression za na `~/Cointent/css` paket. Razlog je tome što trenutno nema drugog [pogrešku u System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) koji su umetati ga u `<script>` oznaka za pričuvni CSS umjesto na očekivani `<link>` oznaka.

    Postoji, međutim, dobro [Stil paket pričuvne](https://github.com/EmberConsultingGroup/StyleBundleFallback) nudi [Ember Savjetodavne grupe](https://github.com/EmberConsultingGroup).

2. Da biste koristili rješenje CSS-a, stvorite novu datoteku .cs u projekta Web uloga *App_Start* mapu pod nazivom *StyleBundleExtensions.cs*i njezin sadržaj zamijenite [koda iz GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).

4. U *App_Start\StyleFundleExtensions.cs*preimenujte naziva naziv uloge Web (npr. **WebRole1**).

3. Vratili u `App_Start\BundleConfig.cs` i izmjena posljednji `bundles.Add` izvoda s istaknuti sljedeći kod:  

        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));

    Ovaj nov način kućni broj koristi isti ideja za ubaciti skripte u HTML-u da biste provjerili DOM za na odgovarajući naziv klase, naziv pravila i definirana u paket CSS-a i pada povratak na web-poslužitelj polazište ako ga ne uspije pronaći podudaranje vrijednost pravila.

4. Ponovno objavite servisa u oblaku i pristup na početnu stranicu.

5. Prikaz HTML kod za stranicu. Trebali biste pronašli umetnutog skripte sličnu ovoj:    

        ...

        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>

        ...

            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>

        ...


    Imajte na umu da umetnutog skripte za paket CSS-a i dalje sadrži errant njezin blijedi iz na `CdnFallbackExpression` svojstvo u retku:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    No Budući prvi dio na || izraz uvijek vratiti true (u retku iznad koji), funkcija document.write() nikad će se pokrenuti.

## <a name="more-information"></a>Dodatne informacije ##
- [Pregled mreže za Azure isporuku sadržaja (CDN)](http://msdn.microsoft.com/library/azure/ff919703.aspx)
- [Korištenje Azure CDN](cdn-create-new-endpoint.md)
- [ASP.NET usnopljavanje i Minification](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)



[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
