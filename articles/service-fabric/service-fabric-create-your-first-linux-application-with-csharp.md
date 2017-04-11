<properties
   pageTitle="Stvaranje prve tkanina servisa aplikacija na Linux pomoću C# | Microsoft Azure"
   description="Stvaranje i Implementacija servisa tkanina aplikacije pomoću C#"
   services="service-fabric"
   documentationCenter="csharp"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="csharp"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="subramar"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Stvaranje prve tkanina servisa Azure aplikacije

> [AZURE.SELECTOR]
- [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Servis tkanina nudi SDK-ovi za izgradnju usluge na Linux u .NET Core i Java. U ovom ćete praktičnom vodiču smo pogledajte kako stvoriti aplikaciju za Linux i izraditi servis, koristeći C# (.NET Core).

## <a name="prerequisites"></a>Preduvjeti

Prije nego što počnete, provjerite jeste li povezani s [postavljeno okruženje za razvoj Linux](service-fabric-get-started-linux.md). Ako koristite Mac OS X, možete ga [Postavljanje okruženja za jedan okvir Linux u virtualnog računala pomoću Vagrant](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Stvaranje aplikacije

Servis tkanina aplikacije mogu sadržavati jedne ili više usluga, svaka s određenim uloga u izlaganja funkcionalnosti aplikacije. SDK tkanina servisa za Linux obuhvaća [Yeoman](http://yeoman.io/) generator koja olakšava stvaranje prvi servis, a da biste dodali više poslije. Pogledajmo pomoću Yeoman stvorite aplikaciju s jedan servis.

1. U terminal, upišite sljedeću naredbu da biste počeli raditi na scaffolding:`yo azuresfcsharp`

2. Dodijelite naziv aplikacije.

3. Odaberite vrstu prvi servis i nazovite ih. Za potrebe ovog praktičnog vodiča, ne možemo odaberite pouzdanog glumca servisa.

  ![Generator Yeoman tkanina servisa za C#][sf-yeoman]

>[AZURE.NOTE] Dodatne informacije o tim mogućnostima potražite u članku [modela pregled web-mjesta servisa tkanina programiranje](service-fabric-choose-framework.md).

## <a name="build-the-application"></a>Stvaranje aplikacije

Predlošci za servis tkanina Yeoman obuhvaćaju Sastavi skripte koje možete koristiti da biste sastavili aplikacije iz sustava terminal (nakon pristup mapi aplikacije).

  ```bash
 cd myapp 
 ./build.sh 
  ```

## <a name="deploy-the-application"></a>Implementacija aplikacije

Kada je ugrađena aplikacija, možete je implementirati u lokalnom klaster pomoću EŽA Azure.

1. Povezivanje s lokalnom klaster tkanina servisa.

    ```bash
    azure servicefabric cluster connect
    ```

2. Pomoću skripte Instaliraj navedenih u predlošku kopirajte Aplikacijski paket na klaster slika Store, registrirati vrsta aplikacije i stvoriti instancu aplikacije.

    ```bash
    ./install.sh
    ```

3. Otvorite preglednik i idite na servis tkanina Explorer pri http://localhost:19080/Explorer (Zamijeni localhost s privatnim IP od VM ako koristite Vagrant operacijskom sustavu Mac OS X).

4. Proširite čvor aplikacije i Primijetit ćete da je sada unos za vrstu vašeg računala, a drugi za prvu instancu vrste.

## <a name="start-the-test-client-and-perform-a-failover"></a>Pokrenite test klijenta i izvesti na prebacivanje

Glumca projekti ne ništa učiniti sami. Trebaju drugih servisa i klijenta za slanje poruka. Predložak glumca uključuje jednostavnog testa skripte koje možete koristiti za interakciju sa servisom glumca.

1. Pokrenuti skriptu Upotreba uslužni pogledajte da biste vidjeli Izlaz iz servisa glumca.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. U programu Explorer tkanina servisa, pronađite čvor hosting primarni replike glumca servisa. U nastavku snimka je čvor 3.

    ![Pronalaženje primarni replike u programu Explorer tkanina servisa][sfx-primary]

3. Kliknite čvor pronaći u prethodnom koraku, a zatim na izborniku Akcije odaberite **Deaktiviraj (ponovno pokrenuti)** . Ova akcija pokreće jednu od pet čvorove vaše lokalne klaster prisilno je prebacivanje na sekundarnog replike izvode na drugi čvor. Kao što je ovu akciju, obratite pozornost na Izlaz iz klijenta za testiranje i Imajte na umu da brojač Nastavi da biste povećali bez obzira na prebacivanje.


## <a name="next-steps"></a>Daljnji koraci

- [Dodatne informacije o pouzdanog Glumci](service-fabric-reliable-actors-introduction.md)
- [Interakcija s klastere tkanina servis pomoću EŽA Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
