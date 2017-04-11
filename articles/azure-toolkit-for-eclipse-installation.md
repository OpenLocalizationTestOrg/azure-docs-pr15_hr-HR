<properties
    pageTitle="Instalacije Azure komplet alata za Eclipse | Microsoft Azure"
    description="Saznajte kako instalirati komplet alata za Azure za Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

# <a name="installing-the-azure-toolkit-for-eclipse"></a>Instalacija alata za Azure za Eclipse

Komplet alata za Azure za Eclipse nudi predloške i funkcija koje omogućuju jednostavno stvaranje razvoj, testiranje i implementacija Azure aplikacije koje se koriste Eclipse razvojno okruženje. Komplet alata za Azure za Eclipse je Otvori izvor projekt, čije izvorni kod je dostupan u odjeljku MIT licenca s web-mjesta projekta na GitHub na sljedeći URL:

<https://github.com/Microsoft/Azure-Tools-for-Java>

Sljedeći koraci Demonstracija instalirajte komplet alata za Azure za Eclipse.

[AZURE.INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Da biste instalirali alat Azure za Eclipse

1. Pokrenite Eclipse.

1. Kad se otvori Eclipse, kliknite izbornik **Pomoć** , a zatim **Instalirati novi softver**, kao što je prikazano na sljedećoj slici.

    ![Instalacija alata za Azure za Eclipse][01]

1. U dijaloškom okviru **Dostupan softver** unutar tekstnog okvira **Rad s** upišite **http://dl.microsoft.com/eclipse** slijedi tipku **Enter** .

1. U oknu **naziv** potvrdite **Azure komplet alata za Eclipse**, a poništite **kontakt sve ažuriranje web-mjesta tijekom instalacije da biste pronašli potrebni softver**. Zaslon trebao izgledati otprilike ovako:

    ![Instalacija alata za Azure za Eclipse][02]

1. Ako proširite **Azure komplet alata za Eclipse**, vidjet ćete sljedeće stavke:

    * **Dodatak za uvid aplikacije za Java**: komponente omogućuje vam korištenje servisa Azure na telemetrijskih zapisivanje i analize services za aplikacije i instanci poslužitelja.
    * **Azure pristup kontrola servisa filtar**: Ova komponenta pruža podršku za provjere autentičnosti korisnike aplikacije s Azure ACS, omogućivanjem scenarija jedinstvene prijave i externalizing identiteta logike iz aplikacije.
    * **Dodatak za uobičajene Azure**: Ova komponenta pruža uobičajenih funkciju potrebne drugih komponenti komplet alata za.
    * **Azure Explorer za Eclipse**: Ova komponenta pruža uobičajenih funkciju potrebne drugih komponenti komplet alata za.
    * **Dodatak za Azure za Eclipse s Java**: Ova komponenta pruža podršku za razvoj projekte koji pomažu stvaraju, testirajte i implementacija Java aplikacije za Microsoft Azure spremanja u Eclipse i putem naredbenog retka.
    * **Dodatak za aplikacije Web Azure s Java**: Ova komponenta pruža podršku za implementaciju Java web-aplikacija za Microsoft Azure Web App spremnika.
    * **Microsoft JDBC upravljački program 4.2 za SQL Server**: Ova komponenta pruža JDBC API-JA za SQL Server i baza podataka Microsoft Azure SQL Java platformu Enterprise Edition 8.
    * **Paket za Apache Qpid klijenta biblioteke JMS**: Ova komponenta pruža klijentske komponente JMS iz projekta Apache Qpid da biste omogućili aplikacije da biste koristili AMQP poruka u programu Microsoft Azure.
    * **Paket za Microsoft Azure biblioteke Java**: Ova komponenta pruža API-ji za pristup servisa Microsoft Azure, kao što je prostor za pohranu servisa bus, izvođenje servisa, itd.

1. Kliknite **Dalje**. (Ako imate neobično kašnjenja prilikom instaliranja alata, provjerite je li **kontakt sve ažuriranje web-mjesta tijekom instalacije da biste pronašli potrebni softver** poništen.)

1. U dijaloškom okviru **Instalirati Detalji** kliknite **Dalje**.

    ![Pregledajte detalje o instalacija][03]

1. U dijaloškom okviru **Pregled licence** , pregledajte uvjete ugovor o licenci. Ako prihvatite uvjete ugovor o licenci, kliknite **Prihvaćam** odredbe ugovora licence, a zatim kliknite **Završi**. (Preostale korake pretpostavlja da prihvatite uvjete ugovor o licenci. Ako ne prihvatite uvjete ugovor o licenci, zatvorite postupak instalacije.)

    ![Pregled licenci][04]

    Eclipse će preuzmite i instalirajte potreban paketa.

    ![Tijek instalacije][05]

1. Ako se to od vas zatraži da biste ponovno pokrenuli Eclipse da biste dovršili instalaciju, kliknite **da**.

    ![Ponovno pokrenite upit][06]

## <a name="see-also"></a>Vidi također

Dodatne informacije o Kompleti alata Azure za Java IDEs potražite na sljedećim vezama:

- [Azure komplet alata za Eclipse]
  - *Instalacija alata za Azure za Eclipse (Ovaj članak)*
  - [Stvaranje web-aplikacijama svijeta pozdrav za Azure u Eclipse]
  - [Što je novo u Azure komplet alata za Eclipse]
- [Azure komplet alata za IntelliJ]
  - [Instalacija alata za Azure za IntelliJ]
  - [Stvaranje web-aplikacijama svijeta pozdrav za Azure u IntelliJ]
  - [Što je novo u Azure komplet alata za IntelliJ]

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java].

<!-- URL List -->

[Azure komplet alata za Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure komplet alata za IntelliJ]: ./azure-toolkit-for-intellij.md
[Stvaranje web-aplikacijama svijeta pozdrav za Azure u Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Stvaranje web-aplikacijama svijeta pozdrav za Azure u IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalacija alata za Azure za IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Što je novo u Azure komplet alata za Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Što je novo u Azure komplet alata za IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Razvojni centar za Azure Java]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

