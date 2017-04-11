<properties 
    pageTitle="Stvaranje web-aplikacijama svijeta pozdrav za Azure u IntelliJ | Microsoft Azure" 
    description="Pomoću ovog praktičnog vodiča pokazuje kako pomoću alata za Azure za IntelliJ da biste stvorili pozdrav svijeta web-aplikacijama za Azure." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-intellij"></a>Stvaranje web-aplikacijama svijeta pozdrav za Azure u IntelliJ

Pomoću ovog praktičnog vodiča pokazuje kako stvoriti i implementirati u osnovni pozdrav svijeta aplikaciju za Azure kao web-aplikacijama pomoću [Alata za Azure za IntelliJ]. Za jednostavnosti prikazuju se osnovni JSP primjer, ali vrlo slično korake prikladna servlet na Java koliko god se brine Azure implementacije.

Kada dovršite ovaj Praktični vodič, aplikacija izgledat će slično kao na sljedećoj slici prilikom prikaza web-preglednika:

![][01]
 
## <a name="prerequisites"></a>Preduvjeti

* Na Java za razvojne inženjere Kit (JDK), v 1.8 ili noviji.
* IntelliJ IDEJA Ultimate Edition. To se može preuzeti iz <https://www.jetbrains.com/idea/download/index.html>.
* Distribucija utemeljena na web-poslužitelj ili poslužitelj aplikacije, kao što su Apache Tomcat ili Jetty.
* Azure pretplatu, koje možete dobivene iz <https://azure.microsoft.com/free/> ili <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure alata za IntelliJ. Dodatne informacije potražite u članku [Instaliranje alata za Azure za IntelliJ].

## <a name="to-create-a-hello-world-application"></a>Radi stvaranja aplikacije pozdrav svijeta

Najprije ćemo ćete pokrenuti sa stvaranjem Java projekta.

1. Pokretanje IntelliJ, a na izborniku kliknite **datoteku**, a zatim **Novo**, a zatim **projekta**.

   ![][02]

1. U dijaloškom okviru novi projekt odaberite **Java**, zatim **Web-aplikacija**, a zatim kliknite **Dalje**.

   ![][03a]

   Ako se to od vas zatraži da biste nastavili s bez SDK dodijeljeni, kliknite **da**.

   ![][03b]

1. Za potrebe ovog praktičnog vodiča, naziv projekta **Java – Web-aplikacije-na-Azure**, a zatim kliknite **Završi**.

   ![][04]

1. Unutar prikaza programa Project Explorer IntelliJ korisnika, proširite **Java – Web-aplikacije-na-Azure**, a zatim proširite **web**, a zatim dvokliknite **index.jsp**.

   ![][05]

1. Kada index.jsp datoteka se otvara u IntelliJ, dodati tekst koji se dinamički prikazuje **Pozdrav svijete!** unutar postojeći `<body>` element. Vaš ažurirani `<body>` sadržaj treba oblik sličan u sljedećem primjeru:

   `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Spremite index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Za implementaciju aplikacije u spremniku Azure Web App

U kojima možete implementirati Java web-aplikaciju za Azure na nekoliko načina. Pomoću ovog praktičnog vodiča opisuju jednu od najjednostavnije: aplikacija će biti implementirano u spremniku za Azure Web App - nema vrsta projekta posebno ni dodatne alate su vam potrebne. U JDK i softver spremnik web će biti ste dobili od Azure, pa nema potrebe da biste prenijeli vlastite; sve što trebate je web-aplikaciju programa Java. Zbog toga postupak objavljivanja za svoju aplikaciju se sekundi, ne minuta.

1. U IntelliJ na Project Explorer desnom tipkom miša kliknite projekt **Java – Web-aplikacije-na-Azure** . Kada se pojavi na kontekstnom izborniku odaberite **Azure**, a zatim kliknite **Objavi kao Azure Web App...**

   ![][06]

1. Ako ste već prijavljeni u Azure s IntelliJ, od vas će se zatražiti da biste se prijavili na račun za Azure:

   ![][07]

   Napomena: Ako imate više računa Azure, neke upite tijekom postupak prijave može se prikazati više puta, čak i ako se pojavljuju se da je isti. Kada se to dogodi, i dalje slijedeći upute za prijavu.

1. Nakon što ste se uspješno prijavili na račun za Azure, dijaloški okvir **Upravljanje pretplatama** prikazat će popis pretplata između kojih se pridružuju vjerodajnice. Ako su navedeni višestruke pretplate, a želite raditi samo određeni podskup ih, po želji mogu poništite one koje želite koristiti. Kada odaberete pretplate, kliknite **Zatvori**.

   ![][08]

1. Kada se pojavi dijaloški okvir **uvođenja Azure Web App spremniku** će prikazati sve spremnika web-aplikacije koje ste prethodno stvorili. Ako niste stvorili sve spremnika, na popisu bit će prazan.   

   ![][09]

1. Ako niste stvorili spremnik aplikacija Web Azure prije ili ako želite da biste objavili aplikacije da biste novi spremnik, poduzmite sljedeće korake. U suprotnom, odaberite postojeći spremnik aplikacije na Web i preskočite do koraka 6 ispod.

  1. Kliknite**+**

        ![][10]

  1. Dijaloški okvir **Novi spremnik Web App** prikazuju se, koji će se koristiti za sljedeće nekoliko koraka.

        ![][11]

  1. Unesite **natpis za DNS** vaše spremnik Web App; To će obrasca oznaku DNS lista URL glavnog računala za web-aplikaciju u Azure. Napomena: Naziv mora biti dostupan i u skladu sa Azure Web App preduvjeti imenovanja.

  1. Na padajućem izborniku **Spremnik Web** odaberite odgovarajući softver za svoju aplikaciju.

        Trenutno, možete odabrati Tomcat 8, Tomcat 7 ili Jetty 9. Nedavne distribucija odabrani softver će nudi Azure i funkcionirat će na nedavni distribucija JDK 8 stvorio Oracle i nudi Azure.

  1. Na padajućem izborniku **pretplate** odaberite pretplatu koju želite koristiti za ovaj implementacije.

  1. Na padajućem izborniku **Grupa resursa** odaberite grupu resursa s kojom želite povezati Web App.

        Napomena: Azure grupa resursa omogućuju vam da grupirate povezane resurse tako da, na primjer, oni mogu izbrisati zajedno.

        Odaberite poruku postojeću grupu resursa (Ako imate neki) i Preskoči da biste se pomicali g ispod ili koristite sljedeće ove korake da biste stvorili novu grupu resursa:

      * Kliknite **novi...**

      * Prikazat će se dijaloški okvir **Nova grupa resursa** :

            ![][12]

      * U u tekstni okvir **naziv** unesite naziv za novu grupu resursa.

      * U u na padajućem izborniku **regija** odaberite odgovarajući podaci za Azure centrirali mjesto za grupu resursa.

      * Kliknite **u redu**.

  1. Padajući izbornik **Aplikacije servisa planiranje** navedene su tarife za servis aplikacije koje su vezane uz grupu resursa koji ste odabrali.

        Napomena: Planiranje programa aplikacije servisa određuje podatke kao što su mjesto na web-aplikaciju programa, cijene sloju i veličine instancu računalnim. U jednu aplikaciju servisa planiranje može se koristiti više web-aplikacijama koje je zašto je zasebno se održava s određenim implementaciju Web App.

        Odaberite poruku postojeće aplikacije servisa planiranje (Ako imate neki) i Preskoči da biste se pomicali h ispod ili koristite ove korake za stvaranje je nove aplikacije servisa planiranje sljedeće:

      * Kliknite **novi...**

      * Prikazat će se dijaloški okvir **Novi servisa aplikacija planiranje** :

            ![][13]

      * U u tekstni okvir **naziv** unesite naziv vaše nove aplikacije servisa Plan.

      * U na padajućem izborniku **mjesta** odaberite odgovarajući podaci za Azure centrirali mjesto za plan.

      * U na padajućem izborniku **Cijene sloju** odaberite odgovarajući cijene za plan. Za testiranje možete **besplatno**.

      * U na instancu padajućem izborniku **Veličina** , odaberite instancu odgovarajuću veličinu za plan. Za testiranje možete odabrati **Small**.

  1. Nakon što ste dovršili sve gore navedene korake, dijaloški okvir novi spremnik Web App treba oblik sličan na sljedećoj slici:

        ![][14]

  1. Kliknite **u redu** da biste dovršili stvaranje vaš novi spremnik Web App.

        Pričekajte nekoliko sekundi na popisu web-aplikacije spremnika osvježiti, a time ćete odabrati vaše spremnik novostvorenu web aplikacije na popisu.

1. Sada ste spremni dovršiti početnog uvođenja web-aplikacije za Azure; Kliknite **u redu** za implementaciju aplikacije Java odabrane spremniku Web App.

    ![][15]

    Napomena: Po zadanom aplikacija će biti implementirano kao njegov poslužitelj aplikacije. Ako želite uvesti kao korijen aplikaciju, potvrdite okvir **uvođenja korijen** prije nego što kliknete **u redu**.

1. Nakon toga, trebali biste vidjeti prikaz **Azure zapisnik aktivnosti** , što označava status implementaciju web-aplikacije.

    ![][16]

    Postupak implementacije Web App za Azure morate poduzeti samo nekoliko sekundi. Kada podrška za aplikacije, vidjet ćete vezu pod nazivom **objavljene** u stupcu **Stanje** . Kada kliknete vezu, ona će vas odvesti distribuiranih web-aplikaciju programa na početnoj stranici ili slijedite korake u sljedećem odjeljku da biste pregledali na web-aplikaciju.

## <a name="browsing-to-your-web-app-on-azure"></a>Pregledavanje na web-aplikaciju na Azure

Da biste pregl na web-aplikaciju na Azure, možete koristiti prikaz **Pretraživača Azure** .

Ako prikaz **Pretraživača Azure** još nije otvoren, koji ga da biste otvorili, zatim izbornika **Prikaz** u IntelliJ, a zatim kliknite **Alat za Windows**, a zatim **Explorer servisa**. Ako ste nije već prijavljeni, ponudit će vam da biste to učinili.

Kada se prikaže prikaz **Pretraživača Azure** , korištenje slijedite ove korake da biste zaustavili web-aplikaciju programa: 

1. Proširite čvor **Azure** .

1. Proširite čvor **Web-aplikacije** . 

1. Desnom tipkom miša kliknite željeni web-aplikaciji.

1. Kada se pojavi na kontekstnom izborniku, kliknite **Otvori u pregledniku**.

    ![][17]

## <a name="updating-your-web-app"></a>Ažuriranje aplikacije za Web

Ažuriranje postojeći imaju Azure Web App je brz i jednostavan postupak, a imate dvije mogućnosti za ažuriranje:

* Možete ažurirati implementacije postojeće Java Web App.
* Možete objaviti dodatne Java aplikacije isti spremniku Web App.

U svakom slučaju, postupak jednak i traje samo nekoliko sekundi:

1. U programu project explorer IntelliJ, desnom tipkom miša kliknite aplikaciju Java koju želite ažurirati ili dodati postojeće spremnik aplikacije na Web.

1. Kada se pojavi na kontekstnom izborniku odaberite **Azure** , a zatim **Objavi kao Azure Web App...**

1. Budući da ste već prijavili ste prethodno, vidjet ćete popis na postojeće web-aplikacije spremnika. Odaberite onaj koji želite objaviti ili ponovno objaviti Java aplikaciju i kliknite **u redu**.

Nekoliko sekundi kasnije u **Zapisnik aktivnosti Azure** prikaz ažurirane uvođenja kao **objavljeno** i moći da biste provjerili ažurirane aplikacije u web-pregledniku.

## <a name="starting-or-stopping-an-existing-web-app"></a>Pokretanje i zaustavljanje postojeće Web App

Za pokretanje ili prekidanje programa postojeće Azure Web App spremnik (uključujući sve distribuiranih Java aplikacije u njoj), možete koristiti prikaz **Pretraživača Azure** .

Ako prikaz **Pretraživača Azure** još nije otvoren, koji ga da biste otvorili, zatim izbornika **Prikaz** u IntelliJ, a zatim kliknite **Alat za Windows**, a zatim **Explorer servisa**. Ako ste nije već prijavljeni, ponudit će vam da biste to učinili.

Kada se prikaže prikaz **Pretraživača Azure** , korištenje slijedite ove korake da biste pokrenuli ili zaustavili web-aplikaciju programa: 

1. Proširite čvor **Azure** .

1. Proširite čvor **Web-aplikacije** . 

1. Desnom tipkom miša kliknite željeni web-aplikaciji.

1. Kada se pojavi na kontekstnom izborniku, kliknite **pokretanje** ili **prekidanje**. Imajte na umu da mogućnosti s izbornika konteksta-umu, pa možete samo zaustavljanje pokrenute web-aplikacijama ili pokrenuti web-aplikacijama koje trenutno ne izvodi.

    ![][18]

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o Kompleti alata Azure za Java IDEs potražite na sljedećim vezama:

- [Azure komplet alata za Eclipse]
  - [Instalacija alata za Azure za Eclipse]
  - [Stvaranje web-aplikacijama svijeta pozdrav za Azure u Eclipse]
  - [Što je novo u Azure komplet alata za Eclipse]
- [Azure komplet alata za IntelliJ]
  - [Instalacija alata za Azure za IntelliJ]
  - *Stvaranje web-aplikacijama svijeta pozdrav za Azure u IntelliJ (Ovaj članak)*
  - [Što je novo u Azure komplet alata za IntelliJ]

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java].

Dodatne informacije o stvaranju Azure Web Apps potražite u članku [Pregled Web Apps].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure komplet alata za Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure komplet alata za IntelliJ]: ../azure-toolkit-for-intellij.md
[Stvaranje web-aplikacijama svijeta pozdrav za Azure u Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Instalacija alata za Azure za Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Instalacija alata za Azure za IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Što je novo u Azure komplet alata za Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Što je novo u Azure komplet alata za IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Razvojni centar za Azure Java]: https://azure.microsoft.com/develop/java/
[Web-aplikacije pregled]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-No-SDK-Specified.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
