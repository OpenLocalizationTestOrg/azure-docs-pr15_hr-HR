<properties 
    pageTitle="Preuzmite Azure SDK za Java" 
    description="Saznajte kako preuzeti Azure SDK Java, kod uzorka za Maven projekata i osnovni koraci za instalacije namijenjeno Azure Tookit za Eclipse." 
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

# <a name="download-the-azure-sdk-for-java"></a>Preuzmite Azure SDK za Java

Ovaj članak sadrži upute za preuzimanje i instaliranje Azure biblioteke za Java.

**Bilješke:** Azure biblioteke za Java distribuira se pod [Apache licencu, verzije 2.0][license].

## <a name="azure-libraries-for-java---manual-download"></a>Azure biblioteke za Java – ručno preuzimanje

Ručna instalacija Azure biblioteke za Java, kliknite <http://go.microsoft.com/fwlink/?LinkId=690320> da biste preuzeli ZIP datoteke koja sadrži svim bibliotekama i sve ovisnosti.

Nakon što preuzmete zip datoteka s računalom, izdvajanje sadržaja i koristite jednu od sljedećih mogućnosti da biste dodali POSUDU datoteke u projekt:

* Uvoz POSUDU datoteke u projekt Java Eclipse.

* Konfiguriranje **Sastavljanje put** Java projekta u Eclipse da uvrstite put do datoteke POSUDU.

Detaljne informacije o postavljanju Sastavi putova u Eclipse potražite u članku [Sastavljanje put Java] na web-mjestu Eclipse.

**Bilješke:** Pogledajte license.txt i ThirdPartyNotices.txt datoteke unutar ZIP za licence i druge informacije.

## <a name="azure-libraries-for-java---building-with-maven"></a>Azure biblioteke za Java – stvaranje s Maven

### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Korak 1 – postavljanje projekta za korištenje Maven za sastavljanje

Da biste stvorili Maven projekata u Eclipse koristi Azure biblioteke za Java, slijedite upute navedene u članku [Uvod u biblioteke upravljanja Azure za Java] [ maven-getting-started] članka. 

### <a name="step-2---configure-your-maven-settings-with-the-requisite-dependencies"></a>Korak 2 – Konfiguriranje postavki Maven uz potreban ovisnosti

Kada projekta konfiguriran za korištenje Maven za sastavljanje, možete dodati na potreban ovisnosti s datotekom pom.xml pomoću sintakse kao u sljedećem primjeru. Imajte na umu da ne morate dodati svaku ovisnost koja se nalazi u sljedećem primjeru; samo morate dodati određene ovisnosti koji su potrebni za projekt.

> [AZURE.NOTE] Svako `<version>` element u sljedeći primjer zamjena rezerviranih mjesta za "n.n.n" u ovom primjeru valjana verzija brojeve, koji možete preuzeti s [Azure biblioteke spremište na Maven].

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-compute</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-network</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-sql</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-storage</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-websites</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-media</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>n.n.n</version>
    </dependency>

## <a name="installing-the-azure-toolkit-for-eclipse"></a>Instalacija alata za Azure za Eclipse

Ova sekcija sadrži osnovne upute za instalaciju komplet alata za Azure za Eclipse; detaljne upute potražite u članku [Instaliranje alata za Azure za Eclipse].

### <a name="prerequisites"></a>Preduvjeti

1. Sustavi Windows operting navedeni u članku [što je novo u komplet alata za Azure za Eclipse] .
1. Macintosh ili Linux operting sustavi navedeni u članku [što je novo u komplet alata za Azure za Eclipse] .
1. Eclipse IDE za razvojne inženjere EE Java, indigo plava ili noviji. To se može preuzeti iz <http://www.eclipse.org/downloads/>.

### <a name="basic-installation-steps"></a>Osnovni koraci za instalaciju

1. U Eclipse, na izborniku **Pomoć** odaberite **Instaliraj novu softver**.
1. Unesite mjesto web-mjesta <http://dl.microsoft.com/eclipse> i pritisnite **Enter**.
1. Odaberite stavke koje treba moguće instalirati, a zatim kliknite **Završi**.

Komplet alata za Azure za Eclipse koristi najnoviju verziju Azure SDK. To se može preuzeti putem instalacijskog programa za platformu Web (WebPI) na <http://go.microsoft.com/fwlink/?LinkID=252838>. Međutim, ako nemate instaliran, prilikom stvaranja prvog Azure implementacije projekta, komplet alata za Azure za Eclipse će automatski instalirati odgovarajuću verziju Azure SDK.

## <a name="see-also"></a>Vidi također

[Azure komplet alata za Eclipse]

[Instalacija alata za Azure za Eclipse] 

[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse]

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java].

<!-- URL List -->

[Razvojni centar za Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547
[Spremište Azure biblioteke na Maven]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure komplet alata za Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalacija alata za Azure za Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Put Sastavi Java]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[Što je novo u Azure komplet alata za Eclipse]: http://go.microsoft.com/fwlink/?LinkId=690333
