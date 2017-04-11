<properties 
    pageTitle="Ispravljanje pogrešaka Java web-aplikacijama na Azure u Eclipse | Microsoft Azure" 
    description="Pomoću ovog praktičnog vodiča pokazuje kako pomoću alata za Azure za Eclipse Java web-aplikacijama sustavom Azure za ispravljanje pogrešaka." 
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

# <a name="debug-a-java-web-app-on-azure-in-eclipse"></a>Web-aplikacijama Java na Azure u Eclipse za ispravljanje pogrešaka

Pomoću ovog praktičnog vodiča pokazuje kako web-aplikacijama Java sustavom Azure pomoću [Alata za Azure za Eclipse]za ispravljanje pogrešaka. Radi jednostavnosti, osnovne stranice Java Server (JSP) primjer će koristiti za ovaj vodič, ali korake će biti sličan za Java servlet kada ispravljanje pogrešaka na Azure.

Kada dovršite ovaj Praktični vodič, aplikacija će izgledati slično kao na sljedećoj slici su je pogrešaka u Eclipse:

![][01]
 
## <a name="prerequisites"></a>Preduvjeti

* Na Java za razvojne inženjere Kit (JDK), v 1.8 ili noviji.
* Eclipse IDE za razvojne inženjere EE Java, indigo plava ili noviji. To se može preuzeti iz <http://www.eclipse.org/downloads/>.
* Distribucija utemeljena na web-poslužitelj ili poslužitelj aplikacije, kao što su Apache Tomcat ili Jetty.
* Azure pretplatu, koje možete dobivene iz <https://azure.microsoft.com/en-us/free/> ili <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure alata za Eclipse. Dodatne informacije potražite u članku [Instaliranje alata za Azure za Eclipse].
* Dinamični Project Web stvoriti i implementirati aplikacije servisa za Azure; na primjer potražite u članku [Stvaranje pozdrav svijeta web-aplikacijama za Azure u Eclipse].

## <a name="to-debug-a-java-web-app-on-azure"></a>Za ispravljanje pogrešaka u web-aplikacijama Java na Azure

Da biste dovršili ove korake u ovom odjeljku, koristite postojeći dinamički projekt Web koji ste već implementiran kao web-aplikacijom Java na Azure, preuzmite [Ogledne dinamički Web projekta] i slijedite korake u odjeljku [Stvaranje pozdrav svijeta web-aplikacijama za Azure u Eclipse] za implementaciju na Azure. 

1. Otvorite Eclipse.

1. Konfiguriranje istek za daljinsko uklanjanje programskih pogrešaka:

    1. Kliknite izbornik **Windows** Eclipse, a zatim kliknite **Postavke**.
    1. Proširite čvor **Java** , a zatim odaberite **ispravljanje pogrešaka**.
    1. Konfiguriranje postavki i **ispravljanje pogrešaka vremenskog ograničenja (ms)** i **pokretanje vremenskog ograničenja (ms)** da biste `120000`.

        ![][02]

    1. Kliknite **u redu** da biste zatvorili dijaloški okvir **Postavke** .

1. U prikazu Eclipse na Project Explorer desnom tipkom miša kliknite dinamički Web projekta koji ste implementiran na Azure. Kada se pojavi na kontekstnom izborniku odaberite **Ispravljanje pogrešaka kao**, a zatim **Azure Web App**.

    ![][03]

1. Ako je ovo prvi put su pogrešaka dinamički projekta Web, otvorit će se dijaloški okvir **Za ispravljanje pogrešaka konfiguracija** ; možete prihvatiti zadane vrijednosti koje su određen kompleta alata za **Povezivanje** na kartici oblikovanje. Na kartici **izvor** kliknite **Dodaj**, a zatim **Java projekta**, odaberite mogućnost **Projekt dinamički Web**pa kliknite **u redu**. Nakon što ste dovršili ove korake, kliknite **za ispravljanje pogrešaka**.

    ![][04]

1. Kada se to od vas zatraži da biste **Omogućivanje udaljene ispravljanje pogrešaka u udaljene web-aplikaciji sada?**, kliknite **u redu**.

1. Kada se to od vas zatraži tog **web-aplikaciju programa sada je spremna za daljinsko uklanjanje programskih pogrešaka**, kliknite **u redu**.

    ![][05]

1. Kada se ponovno prikaže dijaloški okvir **Konfiguracija za ispravljanje pogrešaka** , kliknite **za ispravljanje pogrešaka**.

1. Windows naredbeni redak ili Unix ljuske će otvoriti i priprema potrebne vezu za ispravljanje pogrešaka; morate čekati da se veza na udaljenom Java Web app ne uspije prije nastavka. Ako koristite Windows, izgledat će kao na sljedećoj slici.

    ![][06]

1. Umetnite točku prijeloma stranica JSP, a zatim otvorite URL za web-aplikaciju programa Java u pregledniku:

    1. Otvaranje kopije **Azure Explorer** u Eclipse.
    1. Dođite do **Web-aplikacije** i Java web-aplikaciju koju želite ispraviti pogreške.
    1. Desnom tipkom miša kliknite web-aplikaciju pa kliknite **Otvori u pregledniku**.
    1. Eclipse sada će se unijeti u načinu rada za ispravljanje pogrešaka.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java].

Dodatne informacije o stvaranju Azure Web Apps potražite u članku [Pregled Web Apps].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure komplet alata za Eclipse]: ../azure-toolkit-for-eclipse.md
[Instalacija alata za Azure za Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Stvaranje web-aplikacijama svijeta pozdrav za Azure u Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Ogledna dinamički Web projekta]: http://go.microsoft.com/fwlink/?LinkId=817337

[Razvojni centar za Azure Java]: https://azure.microsoft.com/develop/java/
[Web-aplikacije pregled]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png
