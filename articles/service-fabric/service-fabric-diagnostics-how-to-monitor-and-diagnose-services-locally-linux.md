<properties
   pageTitle="Lokalno nadzora i dijagnosticiranje usluge namijenjene tkanina servisa Azure | Microsoft Azure"
   description="Saznajte kako i praćenje dijagnosticiranje servisa pisani korištenjem Microsoft Azure servisa tkanina na računalo lokalne razvoj."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Praćenje i dijagnosticiranje services u postavi za razvoj lokalnog računala


> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
- [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

Nadzor, otkrivanje, dijagnosticiranje i otklanjanje poteškoća omogućuju services da biste nastavili s minimalnim prekidima za korisničko sučelje. Nadzor i dijagnostici su ključnim stvarni distribuiranih radnog okruženja. Usvojio slične modela tijekom razvoja usluge osigurava funkcionira li dijagnostičkih kanal kada premještate u okruženju proizvodnje. Servis tkanina olakšava za razvojne inženjere servisa za implementaciju Dijagnostika koji možete jednostavno radi preko postave na jednom računalu lokalne razvoj i postavu klaster stvarnog života radnog.


## <a name="debugging-service-fabric-java-applications"></a>Ispravljanje pogrešaka aplikacije usluge tkanina Java

Za aplikacije Java dostupni su [više okviri zapisivanje](http://en.wikipedia.org/wiki/Java_logging_framework) . Budući da `java.util.logging` je zadana mogućnost s JRE, također koristi se za [primjere koda u github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  Sljedeće rasprave objašnjava kako konfigurirati na `java.util.logging` framework. 
 
Pomoću java.util.logging možete preusmjeriti vaše aplikacije zapisnika memorije, izlazna strujanja, datoteka konzole ili sockets. Za svaki od tih mogućnosti postoje zadani rukovatelja koje se već nalaze u okvir. Možete stvoriti na `app.properties` datoteku koju želite konfigurirati rukovatelj datoteke za svoju aplikaciju za preusmjeravanje sve zapisnike lokalne datoteke. 

Sljedeći isječak koda sadrži konfiguracije na primjer: 

```java 
handlers = java.util.logging.FileHandler
 
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

Mapa pokazuje na `app.properties` datoteke mora postojati. Nakon na `app.properties` stvorit će se datoteka, možete i promijeniti skriptu točke unosa `entrypoint.sh` u na `<applicationfolder>/<servicePkg>/Code/` mape da biste postavili svojstvo `java.util.logging.config.file` za `app.propertes` datoteku. Stavka će izgledati kao sljedeći isječak:

```sh 
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```
 
 
Tu konfiguraciju rezultira zapisnike koji se prikupljaju zakreću način pri `/tmp/servicefabric/logs/`. **%U** i **%g** omogućuju stvaranje više datoteka s mysfapp0.log nazive datoteka, mysfapp1.log i tako dalje. Po zadanom ako izričito konfiguriran nijedan rukovatelj, rukovatelj konzole registriran. Jedan možete pregledavati zapisnike syslog u odjeljku /var/log/syslog.
 
Dodatne informacije potražite u članku [primjeri kod u github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  



## <a name="next-steps"></a>Daljnji koraci
Istu šifru praćenje dodali u aplikaciji funkcionira i sa Dijagnostika aplikacija na Azure klaster. Pogledajte ove članaka koji raspravljati različite mogućnosti za alate i opisuju kako biste ih možete postaviti.
* [Kako prikupiti zapisnike s Dijagnostika Azure](service-fabric-diagnostics-how-to-setup-lad.md)
