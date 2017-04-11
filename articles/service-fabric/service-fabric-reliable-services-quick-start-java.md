<properties
   pageTitle="Početak rada s pouzdanog Services | Microsoft Azure"
   description="Uvod u stvaranje aplikacije Microsoft Azure servisa tkanina pomoću bez praćenja stanja i s praćenjem stanja servisa."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="vturecek"/>

# <a name="get-started-with-reliable-services"></a>Početak rada s pouzdanog Services

> [AZURE.SELECTOR]
- [C# u sustavu Windows](service-fabric-reliable-services-quick-start.md)
- [Java na Linux](service-fabric-reliable-services-quick-start-java.md)

U ovom se članku objašnjava osnove servisa Azure servisa tkanina pouzdanog Services i vodit će vas kroz stvaranje i implementacija jednostavne pouzdanog servisne aplikacije pisane Java.

## <a name="installation-and-setup"></a>Instalacija i postavljanje
Prije nego što počnete, pripazite da imate razvojno okruženje tkanina servis postavljen na vašem računalu.
Ako vam je potrebna je postavili, idite na [Početak rada na Mac](service-fabric-get-started-mac.md) ili [Početak rada u sustavu Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Osnovni koncepti
Za početak rada s pouzdanog Services, samo morate razumjeti nekoliko osnovni koncepti:

 - **Vrsta servisa**: to je implementaciju servisa. Je definirao pišete klasa koja se prenosi `StatelessService` i bilo kojim drugim kod ili ovisnosti therein, koristiti uz naziv i broj verzije.

 - **Imenovani instanca servisa**: da biste pokrenuli uslugu, stvarate imenovani instanci vrste usluge slično kao što su stvaranje pojavljivanja predmete vrsta objekta. Instance servisa zapravo su instantiations objekt na servis klase koje pišete. 

 - **Servis glavnog računala**: instanci imenovani servisa stvorite je potrebno za pokretanje unutar glavno računalo. Glavno računalo za servis je samo postupak gdje možete pokrenuti instanci servisa.

 - **Registracija usluge**: Registracija objedinjuje sve. Vrsta servisa mora biti registriran u runtime tkanina servisa u servis glavno računalo da biste omogućili tkanina servisa da biste stvorili instance ga da biste pokrenuli.  

## <a name="create-a-stateless-service"></a>Stvaranje bez praćenja stanja servisa

Započnite tako da stvorite novu aplikaciju servisa tkanina. SDK tkanina servisa za Linux obuhvaća na Yeoman generator omogućuju scaffolding za servis tkanina aplikacijom bez praćenja stanja servisa. Pokretanje tako da pokrenete sljedeći Yeoman naredbe:

```bash
$ yo azuresfjava
```

Slijedite upute za stvaranje **Pouzdanog bez praćenja stanja servisa**. U ovom ćete praktičnom vodiču naziv aplikacije "HelloWorldApplication" i "HelloWorld" usluga. Rezultat će sadržavati direktorija za na `HelloWorldApplication` i `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>Implementacija servisa

Otvorite **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Klase definira vrstu servisa, a možete pokrenuti kod. API usluge nudi dvije točke unosa za svoj kod:

 - Točka način open-ended unos naziva `runAsync()`, gdje možete početi izvršavanja sve radnih opterećenja, uključujući radnih opterećenja računalnim dugoročnih.

```java
@Override
protected CompletableFuture<?> runAsync() {
    ...
}
```

 - Na komunikacije točka unosa koji možete umetnuti u snop komunikacije po izboru. Ovo je gdje možete pokrenuti prima zahtjeve od korisnika i ostale servise.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

U ovom ćete praktičnom vodiču smo će usredotočite se na na `runAsync()` način točke unosa. Ovo je gdje možete odmah započeti izvodi kod.

### <a name="runasync"></a>RunAsync

Platforme poziva ovu metodu kada je instanca servisa postavljen i spremni izvršiti. Otvaranje i zatvaranje ciklus instanca servisa može se pojaviti više puta putem vijek servis kao cjelinu. To se može dogoditi različitih razloga, uključujući:

- Sustav premješta na instanci servisa za resurs za ujednačavanje.
- Kvarove će se pojaviti u kodu.
- Nadogradit će se aplikacija ili sustava.
- Temeljni hardver dođe do prekida.

U ovom djelovanje upravlja putem servisa tkanina zadržati na servisu iznimno dostupne i ispravno usklađene.

#### <a name="cancellation"></a>Otkazivanje

Je ključan koji kod u `runAsync()` izvođenja kada obavještavanja tkanina servisa, možete isključiti. Na `CompletableFuture` vratio `runAsync()` je otkazana kada servis tkanina zahtijeva uslugu da biste zaustavili izvršavanje. Sljedeći primjer pokazuje kako rukovati otkazivanja događaja: 

```java
    @Override
    protected CompletableFuture<?> runAsync() {

        CompletableFuture<?> completableFuture = new CompletableFuture<>();
        ExecutorService service = Executors.newFixedThreadPool(1);
        
        Future<?> userTask = service.submit(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try
                {
                   logger.log(Level.INFO, this.context().serviceName().toString());
                   Thread.sleep(1000);
                }
                catch (InterruptedException ex)
                {
                    logger.log(Level.INFO, this.context().serviceName().toString() + " interrupted. Exiting");
                    return;
                }
            }
         });
 
        completableFuture.handle((r, ex) -> {
            if (ex instanceof CancellationException) {
                userTask.cancel(true);
                service.shutdown();
            }
            return null;
        });
 
        return completableFuture;
   }
``` 

### <a name="service-registration"></a>Registracija usluge

Vrste servisnih mora biti registriran u runtime tkanina servisa. Vrsta servisa definiranju u na `ServiceManifest.xml` i svojoj učionici servis koji implementira `StatelessService`. Registracija usluge provodi u glavni unos točke postupak. U ovom primjeru je postupak glavni ulaza `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    } 
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration: {0}", ex.toString());
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Pokrenite aplikaciju

U Yeoman scaffolding sadrži skriptu gradle omogućuje stvaranje aplikacija i tulum skripte za implementaciju i poništavanje-implementacija aplikacije. Da biste pokrenuli aplikaciju, Sastavi aplikacijom gradle:

```bash
$ gradle
```

To će stvoriti servisa tkanina Aplikacijski paket koji možete uvesti pomoću servisa tkanina Azure EŽA. Skripta install.sh sadrži naredbe Azure EŽA potrebne za implementaciju Aplikacijski paket. Jednostavno pokrenuti skriptu install.sh za implementaciju:

```bask
$ ./install.sh
```
