<properties
    pageTitle="Uvod u testiranje u radnog web-aplikacijama"
    description="Saznajte više o Test značajke radnog (opis) u web-aplikacijama za Azure aplikacije servisa."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-test-in-production-for-web-apps"></a>Uvod u testiranje u radnog web-aplikacijama

Testiranje u radni ili live testira web-aplikaciju programa pomoću promet uživo kupca je test strategije integriranih aplikacija razvojnim inženjerima sve u svoje methodology [agilno razvoj](https://en.wikipedia.org/wiki/Agile_software_development) . Omogućuje testiranje kvalitete aplikacije s promet uživo korisnika u okruženju sustava radnog umjesto sintetizirani podataka u okruženje za testiranje. Tako će novu aplikciju realni korisnicima možete samo informirati stavite u realne probleme aplikacije može biti namijenjeno kada je u upotrebi. Možete provjeriti funkcionalnost, performanse i vrijednost ažuriranja aplikacije protiv glasnoća, brzinu i raznih promet realni korisnika koja nikada ne možete zadržati u okruženju test.

## <a name="traffic-routing-in-app-service-web-apps"></a>Promet usmjeravanje u aplikacije servisa za web-aplikacijama

Pomoću značajke usmjeravanje prometa u [Aplikacije servisa za Azure](http://go.microsoft.com/fwlink/?LinkId=529714)možete izravno dio uživo korisnika promet na jednu ili više [slobodnih implementacije](web-sites-staged-publishing.md)i analizirati aplikacije sa [Uvida Azure aplikacije](/services/application-insights/) ili [Azure HDInsight](/services/hdinsight/)ili alata drugih proizvođača kao [Novi Relic](/marketplace/partners/newrelic/newrelic/) da biste provjerili valjanost promjene. Ako, na primjer, možete implementirati sljedećim scenarijima sa servisom za aplikaciju:

- Otkrijte funkcionalni programskih pogrešaka ili pinpoint performanse grla u ažuriranja prije implementacije cijelo web-mjesto
- Izvođenje "nadziranim test letovi" promjene tako da mjerenje metriku usibility na aplikaciju beta
- Postupno steći potrebna znanja do novo ažuriranje i obavljanje sigurnosno prema dolje na trenutnu verziju ako dođe do pogreške 
- Optimiziranje pokrenite aplikaciju poslovne rezultate tako da pokrenete [A / B testira](https://en.wikipedia.org/wiki/A/B_testing) ili [multivariate testova](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) u više slobodnih implementacije

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>Preduvjeti za korištenje usmjeravanje prometa u web-aplikacijama

- Web-aplikaciju programa morate pokrenuti u sloju **Standardno** ili **Premium** , kao što je potrebno za više slobodnih implementacije.
- Da bi funkcionirao, usmjeravanje prometa zahtijeva kolačići omogućiti u pregledniku korisnika. Usmjeravanje prometa koristi kolačiće da biste prikvačili preglednik klijenta za implementaciju razdoblje za trajanja sesije klijenta.
- Usmjeravanje prometa podržava napredne scenarije savjet pomoću cmdleta ljuske PowerShell Azure.

## <a name="route-traffic-segment-to-a-deployment-slot"></a>Usmjeravanje prometa segment za implementaciju razdoblje

Na razini osnovni u svakoj scenariju savjet usmjeravanje unaprijed definirane postotak uživo prometa vremensko razdoblje za implementaciju koji nisu radnog. Da biste to učinili, slijedite korake u nastavku:

>[AZURE.NOTE] Ovdje navedenih koraka pretpostavlja da ste već [vremensko razdoblje implementacije koje nisu radnog](web-sites-staged-publishing.md) i željeni web-aplikacije sadržaja je već [implementiran](web-sites-deploy.md) na njega.

1. Prijava na [Portal za Azure](https://portal.azure.com/).
2. U plohu web app kliknite **Postavke** > **Usmjeravanje prometa**.
  ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Odaberite vremensko razdoblje koje želite usmjeriti promet i postotka zbroja prometa koji žele, a zatim kliknite **Spremi**.

    ![](./media/app-service-web-test-in-production/02-select-slot.png)

4. Idite na plohu implementacije vremensko razdoblje. Prikazat će se sada uživo promet preusmjeravati na njega.

    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Kada je konfiguriran za usmjeravanje prometa, navedeni postotak klijenata će se slučajno usmjeriti na vremensko razdoblje koje nisu radnog. Međutim, važno je Imajte na umu da kada klijentsko automatski usmjeriti na određene vremensko razdoblje, ga će "prikvačiti" te vremensko razdoblje za tu sesiju klijenta. To učiniti pomoću kolačić da biste prikvačili korisničke sesije. Ako provjera HTTP zahtjeva, pronaći ćete u `TipMix` kolačića u svaki zahtjev za kasnije.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>Prisilna Primjena zahtjevi klijenta za određeni vremensko razdoblje

Uz automatsko promet usmjeravanje, aplikacije servisa za je moći usmjeravanje zahtjevi za određene vremensko razdoblje. To je korisno kada želite da korisnici moći uključivanje u ili onemogućivanju beta aplikacije. Da biste to učinili, morate koristiti u `x-ms-routing-name` parametarski upit.

Da biste preusmjeri korisnicima određene vremensko razdoblje pomoću `x-ms-routing-name`, morate provjeriti je li na vremensko razdoblje već dodana popisu usmjeravanje prometa. Budući da želite usmjeriti na vremensko razdoblje izričito stvarni usmjeravanje postotak postavite nije bitan. Ako želite, možete sastavite taj "beta vezu" koju korisnik može kliknuti da biste pristupili aplikaciju beta.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Isključivanje korisnika iz aplikacije beta

Na primjer, da vam korisnici odustajanje od beta aplikacije možete pohraniti ovu vezu na web-stranice:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

Niz `x-ms-routing-name=self` određuje vremensko razdoblje proizvodnje. Kada preglednik za klijentski pristup vezu, ne samo se se preusmjerava na jedno područje radnog, ali će sadržavati svaki zahtjev za kasnije na `x-ms-routing-name=self` kolačić koji će sesiju da bi vremensko razdoblje proizvodnje.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>Uključivanje korisnicima u aplikaciji beta

Da biste omogućili korisnicima uključivanje beta aplikacije, postavite parametar upita isti naziv vremensko razdoblje koje nisu radnog, primjerice:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>Dodatni resursi ##

-   [Postavljanje pripremna okruženja za web-aplikacije u aplikacije servisa za Azure](web-sites-staged-publishing.md)
-   [Implementacija složene aplikacije predvidljivije servisu Azure](app-service-deploy-complex-application-predictably.md)
-   [Agilno softver razvoja s aplikacije servisa za Azure](app-service-agile-software-development.md)
-   [Učinkovito korištenje DevOps okruženja web-aplikacijama](app-service-web-staged-publishing-realworld-scenarios.md)