<properties 
    pageTitle="Ispravljanje pogrešaka Java web-aplikacijama na Azure u IntelliJ | Microsoft Azure" 
    description="Pomoću ovog praktičnog vodiča pokazuje kako pomoću alata za Azure za IntelliJ Java web-aplikacijama sustavom Azure za ispravljanje pogrešaka." 
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
    ms.date="09/20/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>Web-aplikacijama Java na Azure u IntelliJ za ispravljanje pogrešaka

Pomoću ovog praktičnog vodiča pokazuje kako web-aplikacijama Java sustavom Azure pomoću [Alata za Azure za IntelliJ]za ispravljanje pogrešaka. Radi jednostavnosti, osnovne stranice Java Server (JSP) primjer će koristiti za ovaj vodič, ali korake će biti sličan za Java servlet kada ispravljanje pogrešaka na Azure.

Kada dovršite ovaj Praktični vodič, aplikacija će izgledati slično kao na sljedećoj slici su je pogrešaka u IntelliJ:

![][01]
 
## <a name="prerequisites"></a>Preduvjeti

* Na Java za razvojne inženjere Kit (JDK), v 1.8 ili noviji.
* IntelliJ IDEJA Ultimate Edition. To se može preuzeti iz <https://www.jetbrains.com/idea/download/index.html>.
* Distribucija utemeljena na web-poslužitelj ili poslužitelj aplikacije, kao što su Apache Tomcat ili Jetty.
* Azure pretplatu, koje možete dobivene iz <https://azure.microsoft.com/en-us/free/> ili <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure alata za IntelliJ. Dodatne informacije potražite u članku [Instaliranje alata za Azure za IntelliJ].
* Dinamični Project Web stvoriti i implementirati aplikacije servisa za Azure; na primjer potražite u članku [Stvaranje pozdrav svijeta web-aplikacijama za Azure u IntelliJ].

## <a name="to-debug-a-java-web-app-on-azure"></a>Za ispravljanje pogrešaka u web-aplikacijama Java na Azure

Da biste dovršili ove korake u ovom odjeljku, koristite postojeći dinamički projekt Web koji ste već implementiran kao web-aplikacijom Java na Azure, preuzmite [Ogledne dinamički Web projekta] i slijedite korake u odjeljku [Stvaranje pozdrav svijeta web-aplikacijama za Azure u IntelliJ] za implementaciju na Azure. 

1. Otvaranje projekta Java web-aplikacije koje implementiran na Azure u IntelliJ.

1. Kliknite izbornik za **pokretanje** , a zatim kliknite **Uređivanje konfiguracije...**.

    ![][02]

1. Kada otvara se dijaloški okvir **Pokreni/ispravljanje konfiguracije** : 

    1. Odaberite **Azure Web App**.
    1. Kliknite na **+** da biste dodali novu konfiguraciju.
    1. Navedite **naziv** za konfiguraciju.
    1. Prihvaćanje preostale zadane vrijednosti koje su predložio komplet alata za Azure, a zatim kliknite **u redu**.

        ![][03]

1. Odaberite konfiguraciju ispravljanje pogrešaka Azure web-aplikacije koje ste upravo stvorili na gornjem desnom uglu na izborniku, a zatim kliknite na **ispravljanje pogrešaka**

    ![][04]

1. Kada se to od vas zatraži da biste **Omogućivanje udaljene ispravljanje pogrešaka u udaljene web-aplikaciji sada?**, kliknite **u redu**.

1. Kada se to od vas zatraži tog **web-aplikaciju programa sada je spremna za daljinsko uklanjanje programskih pogrešaka**, kliknite **u redu**.

    ![][05]

1. Odaberite konfiguracija ispravljanje pogrešaka Azure web-aplikacije koje ste upravo stvorili na gornjem desnom uglu izbornik, a zatim na **ispravljanje pogrešaka**.

1. Windows naredbeni redak ili Unix ljuske će otvoriti i priprema potrebne vezu za ispravljanje pogrešaka; morate čekati da se veza na udaljenom Java Web app ne uspije prije nastavka. Ako koristite Windows, izgledat će ovako:

    ![][06]

1. Umetnite točku prijeloma stranica JSP, a zatim otvorite URL za web-aplikaciju programa Java u pregledniku:

    1. Otvaranje kopije **Azure Explorer** u IntelliJ.
    1. Dođite do **Web-aplikacije** i Java web-aplikaciju koju želite ispraviti pogreške.
    1. Desnom tipkom miša kliknite web-aplikaciju pa kliknite **Otvori u pregledniku**.
    1. IntelliJ sada će se unijeti u načinu rada za ispravljanje pogrešaka.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java].

Dodatne informacije o stvaranju Azure Web Apps potražite u članku [Pregled Web Apps].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure komplet alata za IntelliJ]: ../azure-toolkit-for-intellij.md
[Instalacija alata za Azure za IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Stvaranje web-aplikacijama svijeta pozdrav za Azure u IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Ogledna dinamički Web projekta]: http://go.microsoft.com/fwlink/?LinkId=817337

[Razvojni centar za Azure Java]: https://azure.microsoft.com/develop/java/
[Web-aplikacije pregled]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png
