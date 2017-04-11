<properties
    pageTitle="Web App kloniranje pomoću portala za Azure"
    description="Saznajte kako Kloniraj web-aplikacije pomoću portala za Azure novog web-aplikacije."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/08/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Aplikacije servisa za Azure aplikacije kloniranje pomoću portala za Azure#

Značajku postupka kloniranja u [Web-aplikacijama za Azure aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=529714) omogućuje vam da jednostavno Kloniraj postojeće web-aplikacije novostvorenu aplikaciju u nekoj drugoj regiji ili u istom području. To će omogućiti klijentima za implementaciju broj aplikacije preko različitih područja brzo i jednostavno.

Aplikacija kloniranje trenutno je podržana samo za premium sloju aplikacije servisa tarife. Nova značajka koristi iste ograničenja kao značajka sigurnosnu kopiju Web Apps potražite u odjeljku [sigurnosno kopiranje web app u aplikacije servisa za Azure](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 


## <a name="cloning-an-existing-app"></a>Kloniranje postojeće aplikacije ##

Web-aplikaciju mora biti pokrenut u načinu **Premium** mogli da biste stvorili Kloniraj za web-aplikacije.

1. [Portal za Azure](https://portal.azure.com/)otvorite plohu web app.
2. Kliknite **Alati**. Nakon toga u plohu **Alati** kliknite **Kloniraj aplikacije**.

    ![][1]

    > [AZURE.NOTE]
    > Ako web-aplikaciji još nije u načinu **Premium** , primit ćete poruku podržani načini za kloniranje aplikacije. Sada imate mogućnost da biste odabrali **nadogradnju**.
    
3. U **Aplikaciji Kloniraj** plohu Navedite naziv novu web-aplikaciju, grupa resursa i planiranje usluga aplikacije. I korisnik će moći odabrati želite li Kloniraj broj izvorišnog web-aplikacije postavke ili ne.

    ![][2]

4. Nakon klika na **Stvaranje** platforme započet će raditi na stvaranje Kloniraj izvorišnog web-aplikacije.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Kloniranje postojeće aplikacije u okruženju aplikacije servisa##

U **Aplikaciji Kloniraj** plohu kupca će vam se mogućnost da biste odabrali grupe aplikacija za aplikacije u okruženju postojeće aplikacije servisa.

## <a name="current-restrictions"></a>Trenutna ograničenja ##

Ta je značajka trenutno u pretpregledu, radimo da biste dodali nove mogućnosti tijekom vremena, na popisu u nastavku su poznati ograničenja podrška za trenutnu aplikaciju što i kloniranje Azure portalu:

- Azure postavke upravitelja promet su klonirana
- Postavke mjerila automatski se klonirana
- Raspored sigurnosnog kopiranja postavke su klonirana
- Postavke VNET su klonirana
- Uvid u aplikaciji ne automatski se postavljaju na web-aplikaciji odredište
- Jednostavno postavke provjere autentičnosti su klonirana
- Proširenje kudu su klonirana
- Savjet pravila su klonirana
- Sadržaj baze podataka su klonirana


### <a name="references"></a>Reference ###
- [Web App kloniranje pomoću komponente PowerShell](app-service-web-app-cloning.md)
- [Stvaranje sigurnosne kopije web app u aplikacije servisa za Azure](web-sites-backup.md)
- [Stvaranje aplikacije servisa okruženja](app-service-web-how-to-create-an-app-service-environment.md)
- [Stvaranje web-aplikacijama u okruženju servisa aplikacija](app-service-web-how-to-create-a-web-app-in-an-ase.md)
- [Uvod u okruženje za aplikacije servisa](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png