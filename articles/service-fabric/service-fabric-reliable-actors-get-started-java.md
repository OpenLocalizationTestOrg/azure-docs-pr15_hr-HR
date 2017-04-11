<properties
   pageTitle="Početak rada sa servisa tkanina pouzdanog Glumci | Microsoft Azure"
   description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake stvaranja, ispravljanje pogrešaka i implementacija jednostavne utemeljen na glumca servisa putem servisa tkanina pouzdanog Glumci."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Početak rada s pouzdanog Glumci

> [AZURE.SELECTOR]
- [C# u sustavu Windows](service-fabric-reliable-actors-get-started.md)
- [Java na Linux](service-fabric-reliable-actors-get-started-java.md)

U ovom se članku objašnjava osnove pouzdanog Glumci tkanina servisa Azure i vodit će vas kroz stvaranje i implementacija jednostavne aplikacije pouzdanog glumca u Java.

## <a name="installation-and-setup"></a>Instalacija i postavljanje
Prije nego što počnete, pripazite da imate razvojno okruženje tkanina servis postavljen na vašem računalu.
Ako vam je potrebna je postavili, idite na [Početak rada na Mac](service-fabric-get-started-mac.md) ili [Početak rada u sustavu Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Osnovni koncepti
Za početak rada s pouzdanog Glumci, što je potrebno da biste shvatili nekoliko osnovni koncepti:

 * **Glumca servisa**. Pouzdan Glumci su pakirat u pouzdanog servise koje možete uvesti u infrastrukture tkanina servisa. Glumca instanci su aktivirane u instancu imenovani servisa.
 
 * **Registracija glumca**. Kao pouzdan servisi sustava pouzdanog glumca usluge mora biti registriran u runtime tkanina servisa. Osim toga, vrstu glumca mora biti registriran u vrijeme izvođenja glumca.
 
 * **Glumca sučelja**. Sučelje glumca koristi se za definiranje svakako upisani javno sučelja glumca. U model terminologija pouzdanog glumca sučelja glumca definira vrste poruka koje u glumca mogu razumjeti i proces. Sučelje glumca koristi drugim Glumci i klijentske aplikacije za "slanje" (asinkrono) poruka u glumca. Pouzdan Glumci možete implementirati više sučelja.
 
 * **ActorProxy predmet**. Klase ActorProxy koristi klijentske aplikacije za pozivanje metode vidljiva kroz glumca sučelja. Klase ActorProxy omogućuje dvije važne radovi:
    * Naziv razlučivost: je moći pronaći na glumca u skupini (Traži čvor klaster gdje se hostira).
    * Nije uspjelo rukovanje: možete ponovno pokušajte instanci način i ponovno riješiti glumca mjesto nakon, na primjer, pogreške koje je potrebno glumca da biste je premještena na drugi čvor u klasteru.

Sljedeća pravila koji se odnose na glumca sučelja su Spominjanje:

- Ne može biti pretrpan glumca sučelja metode.
- Glumca sučelja metode ne smije imati odgovor, ref ili neobavezni parametri.
- Generički sučelja nisu podržani.

## <a name="create-an-actor-service"></a>Stvaranje na servis glumca
Započnite tako da stvorite novu aplikaciju servisa tkanina. SDK tkanina servisa za Linux obuhvaća na Yeoman generator omogućuju scaffolding za servis tkanina aplikacijom bez praćenja stanja servisa. Pokretanje tako da pokrenete sljedeći Yeoman naredbe:

```bash
$ yo azuresfjava
```

Slijedite upute za stvaranje **Pouzdanog glumca servisa**. U ovom ćete praktičnom vodiču naziv aplikacije "HelloWorldActorApplication" i glumca "HelloWorldActor." Stvorit će se sljedeće scaffolding:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Pouzdan Glumci osnovni sastavnih blokova

Osnovni koncepti opisanih Prevedite osnovni sastavnih blokova pouzdanog glumca servisa.

### <a name="actor-interface"></a>Glumca sučelja

Sadrži definiciju sučelje za na glumca. Ovo sučelje definira ugovor glumca koji zajednički koriste implementaciju glumca i klijenti pozivanje glumca, tako da se obično smisla da biste definirali na mjesto na kojem se razlikuje se od implementaciju glumca i mogu zajednički koristiti više drugim servisima ili klijentske aplikacije.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Servis za glumca 
Sadrži glumca implementacije i glumca Registracija kod. Klase glumca implementira glumca sučelja. To je mjesto vašeg glumca ne rad.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Registracija glumca

Servis glumca mora biti registriran u vrsta servisa u runtime tkanina servisa. U redoslijedu glumca servisa za pokretanje sustava instanci glumca vrsti vašega glumca mora biti registriran i sa servisom glumca. Na `ActorRuntime` postupak prijave izvodi to funkcioniralo za Glumci.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {
    
    public static void main(String[] args) throws Exception {
        
        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);
            
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Testiranje klijenta

Ovo je klijentska aplikacija jednostavnog testa zasebno možete pokrenuti iz aplikacije tkanina servis za testiranje glumca usluge. Ovo je primjer od gdje se na ActorProxy mogu koristiti za aktivaciju i komunicirate glumca instance. To ne implementiraju sa servisima.

### <a name="the-application"></a>Aplikacija 

Na kraju, aplikacija paketi servisa glumca i druge servise koji se možda dodate u budućnosti zajedno radi implementacije. Sadrži slike *ApplicationManifest.xml* i postavljanje servisa paketa glumca.

## <a name="run-the-application"></a>Pokrenite aplikaciju

U Yeoman scaffolding sadrži skriptu gradle omogućuje stvaranje aplikacija i tulum skripte za implementaciju i poništavanje-implementacija aplikacije. Da biste pokrenuli aplikaciju, Sastavi aplikacijom gradle:

```bash
$ gradle
```

To će stvoriti servisa tkanina Aplikacijski paket koji možete uvesti pomoću servisa tkanina Azure EŽA. Skripta install.sh sadrži naredbe Azure EŽA potrebne za implementaciju Aplikacijski paket. Jednostavno pokrenuti skriptu install.sh za implementaciju:

```bask
$ ./install.sh
```
