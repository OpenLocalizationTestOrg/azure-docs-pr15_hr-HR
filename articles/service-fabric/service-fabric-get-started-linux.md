<properties
   pageTitle="Postavljanje okruženja za razvoj na Linux | Microsoft Azure"
   description="Instalirajte izvođenja i SDK i stvaranje lokalne razvoj klaster na Linux. Nakon dovršetka ovaj će instalacijski program, bit će spremna za izgradnju aplikacije."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="seanmck"/>

# <a name="prepare-your-development-environment-on-linux"></a>Priprema razvojno okruženje na Linux


> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Da biste implementacije i pokretanja [aplikacije tkanina servisa Azure](service-fabric-application-model.md) na vašem računalu razvoj Linux, instalirajte izvođenja i uobičajenih SDK. Možete instalirati i neobavezna SDK-ovi za Java i .NET Core.

## <a name="prerequisites"></a>Preduvjeti
### <a name="supported-operating-system-versions"></a>Podržani operacijski sustav verzije
Za razvoj podržane su sljedeće verzije operacijski sustav:

- Ubuntu 16.04 (Xenial Xerus)

## <a name="update-your-apt-sources"></a>Ažuriranje Zemaljska izvora

Da biste instalirali SDK i pridruženi runtime paketa putem Zemaljska get, najprije morate ažurirati Zemaljska izvora.

1. Otvorite je terminal.
2. Dodavanje servisa tkanina repo na popis izvora.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Dodavanje novog ključa GPG vaše Zemaljska keyring.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```

4. Osvježite popis paket koji se temelji na novododani spremišta.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Instalacija i postavljanje SDK-a

Kada se ažuriraju izvora, možete instalirati SDK-a.

1. Instalirajte servis tkanina SDK paket. Će se zatražiti da biste potvrdili instalacije i prihvatite licencni ugovor.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

2. Pokrenite instalacijski program skriptu SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-cross-platform-cli"></a>Postavljanje Azure EŽA različite platforme

[Azure EŽA različite platforme] [ azure-xplat-cli-github] sadrži naredbe za interakciju s entiteti tkanina servisa, uključujući klastere i aplikacije. Se temelji na Node.js pa [Provjerite jeste li instalirali čvor] [ install-node] prije nastavka upute u nastavku.

1. Kloniraj repo github na vaše računalo razvoj.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Prijeđite u kloniranu repo i instalirajte na EŽA ovisnosti pomoću upravitelja paketa čvor (npm).

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Stvorite na symlink iz mape smeće/azure od kloniranu repo /usr/bin/azure da bi se dodaje na vaš put i naredbe dostupne s bilo kojeg direktorija.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Na kraju omogućite Samodovršetak servis tkanina naredbe.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>Postavljanje lokalne klaster

Ako je uspješno je instaliran sve, moći da biste pokrenuli lokalne klaster.

1. Pokrenite instalacijski program skriptu klaster.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Otvorite web-preglednik i idite na http://localhost:19080/Explorer. Ako je pokrenut klaster, trebali biste vidjeti na nadzornoj ploči Explorer tkanina servisa.

    ![Servis tkanina Explorer na Linux][sfx-linux]

Sada vam se za implementaciju gotove pakete usluga tkanina ili nove na temelju spremnika goste ili izvršne datoteke za goste. Da biste sastavili nove servise pomoću Java ili .NET Core SDK-ovi, slijedite dolje navedene korake Neobavezna instalacija.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Instalirajte Java SDK i Eclipse Neon (nije obavezno)

Java SDK nudi biblioteke i predloške koje su potrebne za sastavljanje services tkanina servis pomoću Java.

1. Instalacija programskog jezika Java SDK paket.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Pokrenite instalacijski program skriptu SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

Možete instalirati dodatak za Eclipse za servis tkanina iz Eclipse Neon IDE.

1. Eclipse, provjerite je li u imate Buildship verziju 1.0.17 ili noviju verziju. Verzija instalirane komponente možete provjeriti tako da odaberete **Pomoć > Detalji o instalaciji**. Možete ažurirati Buildship prema uputama [u nastavku][buildship-update].

2. Da biste instalirali dodatak tkanina servisa, odaberite **Pomoć > instalacija novi softver...**

3. U tekstni okvir "Rad s" unesite: http://dl.windowsazure.com/eclipse/servicefabric

4. Kliknite Dodaj.

    ![Dodatak za eclipse][sf-eclipse-plugin]

5. Odaberite dodatak za servis tkanina, a zatim kliknite Dalje.

6. Nastavite postupak instalacije i prihvatite licencni ugovor za krajnjeg korisnika.

## <a name="install-the-net-core-sdk-optional"></a>Instalirajte .NET Core SDK-a (nije obavezno)

Temeljni SDK .NET nudi biblioteke i predloške koje su potrebne za sastavljanje services tkanina servis pomoću više platformi .NET Core.

1. Instalirajte paket za .NET Core SDK.

    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```

2. Pokrenite instalacijski program skriptu SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Daljnji koraci

- [Stvaranje prve Java aplikacija na Linux](service-fabric-create-your-first-linux-application-with-java.md)

- [Priprema razvojno okruženje na OSX](service-fabric-get-started-mac.md)


<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
