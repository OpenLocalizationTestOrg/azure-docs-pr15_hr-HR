<properties 
    pageTitle="Stvaranje web-aplikacijama svijeta pozdrav za Azure u Eclipse | Microsoft Azure" 
    description="Pomoću ovog praktičnog vodiča pokazuje kako pomoću alata za Azure za Eclipse da biste stvorili pozdrav svijeta web-aplikacijama za Azure." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Stvaranje web-aplikacijama svijeta pozdrav za Azure u Eclipse

Pomoću ovog praktičnog vodiča pokazuje kako stvoriti i implementirati u osnovni pozdrav svijeta aplikaciju za Azure kao web-aplikacijama pomoću [Alata za Azure za Eclipse]. Za jednostavnosti prikazuju se osnovni JSP primjer, ali vrlo slično korake prikladna servlet na Java koliko god se brine Azure implementacije.

Kada dovršite ovaj Praktični vodič, aplikacija izgledat će slično kao na sljedećoj slici prilikom prikaza web-preglednika:

![Pretpregled pozdrav svijeta aplikacije][01]
 
## <a name="prerequisites"></a>Preduvjeti

* Na Java za razvojne inženjere Kit (JDK), v 1.8 ili noviji.
* Eclipse IDE za Java EE razvojnim inženjerima, Luna ili noviji. To se može preuzeti iz <http://www.eclipse.org/downloads/>.
* Distribucija utemeljena na web-poslužitelj ili poslužitelj aplikacije, kao što su Apache Tomcat ili Jetty.
* Azure pretplatu, koje možete dobivene iz <https://azure.microsoft.com/free/> ili <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure alata za Eclipse. Dodatne informacije potražite u članku [Instaliranje alata za Azure za Eclipse].

## <a name="to-create-a-hello-world-application"></a>Radi stvaranja aplikacije pozdrav svijeta

Najprije ćemo ćete pokrenuti sa stvaranjem Java projekta.

1. Pokretanje Eclipse, i na izborniku kliknite **datoteka**, kliknite **Novo**, a zatim **Dinamički Web projekta**. (Ako ne vidite **Dinamički Project Web** naveden kao dostupne projekt nakon klika na **datoteku** i **Novo**, učinite sljedeće: kliknite **datoteka**, kliknite **Novo**, kliknite **projekta...**, proširite **Web**, kliknite **Dinamički Project Web**i kliknite **Dalje**.)

1. Za potrebe ovog praktičnog vodiča, nazovite projekta **MyWebApp**. Zaslon izgledat će otprilike ovako:

    ![Stvaranje nove dinamičkog Web projekta][02]

1. Kliknite **Završi**.

1. U prikazu programa Project Explorer Eclipse, proširite **MyWebApp**. Desnom tipkom miša kliknite **WebContent**, kliknite **Novo**, a zatim kliknite **Datoteke JSP**.

1. U dijaloškom okviru **Nova datoteka JSP** , naziv datoteke **index.jsp**zadržati nadređenu mapu kao **MyWebApp/WebContent**, a zatim kliknite **Dalje**.

1. U dijaloškom okviru **Odabir predloška JSP** za potrebe ovog praktičnog vodiča odaberite **Novu datoteku JSP (html)**, a zatim **Završi**.

1. Kada index.jsp datoteka se otvara u Eclipse, dodati tekst koji se dinamički prikazuje **Pozdrav svijete!** unutar postojeći `<body>` element. Vaš ažurirani `<body>` sadržaj treba oblik sličan u sljedećem primjeru:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Spremite index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Za implementaciju aplikacije u spremniku Azure Web App

U kojima možete implementirati Java web-aplikaciju za Azure na nekoliko načina. Pomoću ovog praktičnog vodiča opisuju jednu od najjednostavnije: aplikacija će biti implementirano u spremniku za Azure Web App - nema vrsta projekta posebno ni dodatne alate su vam potrebne. U JDK i softver spremnik web će biti ste dobili od Azure, pa nema potrebe da biste prenijeli vlastite; sve što trebate je web-aplikaciju programa Java. Zbog toga postupak objavljivanja za svoju aplikaciju se sekundi, ne minuta.

1. U Eclipse na Project Explorer desnom tipkom miša kliknite **MyWebApp**.

1. Na kontekstnom izborniku odaberite **Azure**, a zatim kliknite **Objavi kao Azure Web App...**

    ![Objavljivanje kao Azure Web App][03]
   
    Umjesto toga, dok je u Project Explorer odabran projekta web aplikacije, možete kliknite gumb na alatnoj traci za padajući izbornik **Objavi** i odaberite **Objavi kao Azure web-aplikacije** iz njega:
   
    ![Objavljivanje kao Azure Web App][14]
   
1. Ako ste već prijavljeni u Azure s Eclipse, od vas će se zatražiti da biste se prijavili na račun za Azure:

    ![Azure prijava dijaloškog okvira][04]
   
    Ako imate više računa Azure, neke upite tijekom postupak prijave može se prikazati više puta, čak i ako se pojavljuju se da je isti. Kada se to dogodi, i dalje slijedeći upute za prijavu.

1. Nakon što ste se uspješno prijavili na račun za Azure, dijaloški okvir **Upravljanje pretplatama** prikazat će popis pretplata između kojih se pridružuju vjerodajnice. Ako su navedeni višestruke pretplate, a želite raditi samo određeni podskup ih, po želji mogu poništite one koje želite koristiti. Kada odaberete pretplate, kliknite **Zatvori**.

    ![Upravljanje pretplatama dijaloškog okvira][05]
   
1. Kada se pojavi dijaloški okvir **uvođenja Azure Web App spremniku** će prikazati sve spremnika web-aplikacije koje ste prethodno stvorili. Ako niste stvorili sve spremnika, na popisu bit će prazan.

    ![Implementacija Azure Web App spremnik dijaloškog okvira][06]
   
1. Ako niste stvorili spremnik aplikacija Web Azure prije ili ako želite da biste objavili aplikacije da biste novi spremnik, poduzmite sljedeće korake. U suprotnom, odaberite postojeći spremnik aplikacije na Web i preskočite do koraka 7 ispod.

    1. Kliknite **novi...**

        ![Implementacija Azure Web App spremnik dijaloškog okvira][15]

    1. Prikazat će se dijaloški okvir **Novi spremnik Web App** :

        ![Dijaloški okvir za novi spremnik Web App][07a]

    1. Unesite **natpis za DNS** vaše spremnik Web App; To će obrasca oznaku DNS lista URL glavnog računala za web-aplikaciju u Azure. (Imajte na umu da naziv mora biti dostupna, a u skladu sa Azure Web App preduvjeti imenovanja.)

    1. Na padajućem izborniku **Spremnik Web** odaberite odgovarajući softver za svoju aplikaciju.

        Trenutno, možete odabrati Tomcat 8, Tomcat 7 ili Jetty 9. Nedavne distribucija odabrani softver će nudi Azure i funkcionirat će na nedavni distribucija JDK 8 stvorio Oracle i nudi Azure.

    1. Na padajućem izborniku **pretplate** odaberite pretplatu koju želite koristiti za ovaj implementacije.

    1. Na padajućem izborniku **Grupa resursa** odaberite grupu resursa s kojom želite povezati Web App. (Azure grupa resursa omogućuju grupiranje povezane resurse tako da, na primjer, oni mogu izbrisati zajedno.)

        Odaberite poruku postojeću grupu resursa (Ako imate neki) i Preskoči da biste se pomicali g ispod ili koristite sljedeće ove korake da biste stvorili novu grupu resursa:

        * Kliknite **novi...**

        * Prikazat će se dijaloški okvir **Nova grupa resursa** :

            ![Dijaloški okvir Nova grupa resursa][08]

        * U u tekstni okvir **naziv** unesite naziv za novu grupu resursa.

        * U u na padajućem izborniku **regija** odaberite odgovarajući podaci za Azure centrirali mjesto za grupu resursa.

        * NEOBAVEZNO: Po zadanom nedavne distribucija Java 8 automatski će se implementirati po Azure vaše spremniku web app kao vaše JVM. Međutim, možete odrediti drugu verziju i distribucija u JVM ako je to potrebno Web App. Da biste odredili na JDK za web-aplikaciju, kliknite karticu **JDK** pa odaberite neku od sljedećih mogućnosti:

            * **Uvođenje zadane JDK servisa Azure web-aplikacije koje nudi**: tu mogućnost će uvesti nedavne distribucija Java 8.

            * **Uvođenje s 3 strana JDK dostupne na Azure**: omogućuje vam da odaberete s popisa JDKs koji nudi Microsoft Azure.

            * **Uvođenje vlastitu JDK s ovog mjesta preuzimanje**: ta mogućnost omogućuje vam da navedete vlastite raspodjele JDK koji moraju biti pakirat kao ZIP datoteku i prenijeti na mjesto javno dostupna preuzimanja ili račun za Azure prostora za pohranu za koje imate pristup.

            ![Dijaloški okvir za novi spremnik Web App][07b]

    * Kliknite **u redu**.

    1. Padajući izbornik **Aplikacije servisa planiranje** navedene su tarife za servis aplikacije koje su vezane uz grupu resursa koji ste odabrali. (Planovi aplikacije servisa za Navedite podatke kao što su mjesto na web-aplikaciju programa, cijene sloju i veličine instancu računalnim. U jednu aplikaciju servisa planiranje može se koristiti više web-aplikacijama koje je zašto je zasebno se održava s određenim implementaciju Web App.)

        Odaberite poruku postojeće aplikacije servisa planiranje (Ako imate neki) i Preskoči da biste se pomicali h ispod ili koristite ove korake za stvaranje je nove aplikacije servisa planiranje sljedeće:

        * Kliknite **novi...**

        * Prikazat će se dijaloški okvir **Novi servisa aplikacija planiranje** :

            ![Planiranje usluga aplikacije dijaloškog okvira novi][09]

        * U u tekstni okvir **naziv** unesite naziv vaše nove aplikacije servisa Plan.

        * U na padajućem izborniku **mjesta** odaberite odgovarajući podaci za Azure centrirali mjesto za plan.

        * U na padajućem izborniku **Cijene sloju** odaberite odgovarajući cijene za plan. Za testiranje možete odabrati **slobodno**.

        * U na instancu padajućem izborniku **Veličina** , odaberite instancu odgovarajuću veličinu za plan. Za testiranje možete odabrati **Small**.

    1. Nakon što ste dovršili sve gore navedene korake, dijaloški okvir novi spremnik Web App treba oblik sličan na sljedećoj slici:

        ![Dijaloški okvir za novi spremnik Web App][10]

    1. Kliknite **u redu** da biste dovršili stvaranje vaš novi spremnik Web App.

        Pričekajte nekoliko sekundi na popisu web-aplikacije spremnika osvježiti, a time ćete odabrati vaše spremnik novostvorenu web aplikacije na popisu.

1. Sada ste spremni dovršiti početnog uvođenja web-aplikacije za Azure:

    ![Implementacija Azure Web App spremnik dijaloškog okvira][11]

    Kliknite **u redu** za implementaciju aplikacije Java odabrane spremniku Web App.

    Prema zadanim postavkama, aplikacija će biti implementirano kao njegov poslužitelj aplikacije. Ako želite uvesti kao korijen aplikaciju, potvrdite okvir **uvođenja korijen** prije nego što kliknete **u redu**.

1. Nakon toga, trebali biste vidjeti prikaz **Azure zapisnik aktivnosti** , što označava status implementaciju web-aplikacije.

    ![Zapisnik aktivnosti Azure][12]

    Postupak implementacije Web App za Azure morate poduzeti samo nekoliko sekundi. Kada podrška za aplikacije, vidjet ćete vezu pod nazivom **objavljene** u stupcu **Stanje** . Kada kliknete vezu, ona će vas odvesti distribuiranih web-aplikaciju programa na početnoj stranici.

## <a name="updating-your-web-app"></a>Ažuriranje aplikacije za Web

Ažuriranje postojeći imaju Azure Web App je brz i jednostavan postupak, a imate dvije mogućnosti za ažuriranje:

* Možete ažurirati implementacije postojeće Java Web App.
* Možete objaviti dodatne Java aplikacije isti spremniku Web App.

U svakom slučaju, postupak jednak i traje samo nekoliko sekundi:

1. U programu project explorer Eclipse, desnom tipkom miša kliknite aplikaciju Java koju želite ažurirati ili dodati postojeće spremnik aplikacije na Web.

1. Kada se pojavi na kontekstnom izborniku odaberite **Azure** , a zatim **Objavi kao Azure Web App...**

1. Budući da ste već prijavili ste prethodno, vidjet ćete popis na postojeće web-aplikacije spremnika. Odaberite onaj koji želite objaviti ili ponovno objaviti Java aplikaciju i kliknite **u redu**.

Nekoliko sekundi kasnije u **Zapisnik aktivnosti Azure** prikaz ažurirane uvođenja kao **objavljeno** i moći da biste provjerili ažurirane aplikacije u web-pregledniku.

## <a name="stopping-an-existing-web-app"></a>Zaustavljanje postojeće Web App

Da biste zaustavili programa postojeće Azure Web App spremnik (uključujući sve distribuiranih Java aplikacije u njoj), možete koristiti prikaz **Pretraživača Azure** .

Ako prikaz **Pretraživača Azure** još nije otvoren, možete otvoriti tako da kliknete izbornik **prozor** u Eclipse, zatim, a zatim kliknite **Pokaži prikaz**, a zatim **druge...**, a zatim **Azure**a zatim **Azure Explorer**. Ako ste nije već prijavljeni, ponudit će vam da biste to učinili.

Kada se prikaže prikaz **Pretraživača Azure** , korištenje slijedite ove korake da biste zaustavili web-aplikaciju programa: 

1. Proširite čvor **Azure** .

1. Proširite čvor **Web-aplikacije** . 

1. Desnom tipkom miša kliknite željeni web-aplikaciji.

1. Kada se pojavi na kontekstnom izborniku, kliknite **Zaustavi**.

    ![Zaustavljanje postojeće Web App][13]

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o Kompleti alata Azure za Java IDEs potražite na sljedećim vezama:

- [Azure komplet alata za Eclipse]
  - [Instalacija alata za Azure za Eclipse]
  - *Stvaranje web-aplikacijama svijeta pozdrav za Azure u Eclipse (Ovaj članak)*
  - [Što je novo u Azure komplet alata za Eclipse]
- [Azure komplet alata za IntelliJ]
  - [Instalacija alata za Azure za IntelliJ]
  - [Stvaranje web-aplikacijama svijeta pozdrav za Azure u IntelliJ]
  - [Što je novo u Azure komplet alata za IntelliJ]

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java].

Dodatne informacije o stvaranju Azure Web Apps potražite u članku [Pregled Web Apps].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure komplet alata za Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure komplet alata za IntelliJ]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Stvaranje web-aplikacijama svijeta pozdrav za Azure u IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Instalacija alata za Azure za Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Instalacija alata za Azure za IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Što je novo u Azure komplet alata za Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Što je novo u Azure komplet alata za IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Razvojni centar za Azure Java]: https://azure.microsoft.com/develop/java/
[Web-aplikacije pregled]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
