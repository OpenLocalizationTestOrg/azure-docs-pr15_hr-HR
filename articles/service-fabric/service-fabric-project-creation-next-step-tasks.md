<properties
   pageTitle="Servis tkanina projekta stvaranje daljnji koraci | Microsoft Azure"
   description="Ovaj članak sadrži veze na skup ključnih zadataka za razvoj za servis tkanina"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="your-service-fabric-application-and-next-steps"></a>Aplikacije servisa tkanina i daljnji koraci
Stvoreno je tkanina servisa Azure aplikacije. U ovom se članku opisuju makeup projekta te neke moguće sljedeće korake.

## <a name="your-application"></a>Aplikacija
Svaku novu aplikaciju obuhvaća projekta programa aplikacije. Mogu postojati jednom ili dvjema dodatne projektima, ovisno o vrsti usluge odabrali.

### <a name="the-application-project"></a>Projekt aplikacije
Projekt aplikacije sastoji se od:

- Skup reference na servise koji čine aplikacije.

- Dva objaviti profili (lokalno i oblaka) koje možete koristiti da biste zadržali postavke za rad s različitim okruženjima – kao što su postavke koje se odnose na krajnjoj točki klaster te je li potrebno izvršiti nadogradnju implementacijama prema zadanim postavkama.

- Dvije aplikacije parametar datoteke (lokalno i oblaka) koje možete koristiti da biste zadržali konfiguracije okruženja specifične aplikacije, kao što je broj particije da biste stvorili za uslugu.

- Uvođenje skripte koje možete koristiti za implementaciju aplikacije iz naredbenog retka ili kao dio automatiziranog neprekinuti integracije i implementacija kanal.

- Programski manifest, koji opisuje aplikacije. Manifest možete pronaći u mapi ApplicationPackageRoot.

### <a name="stateless-service"></a>Bez praćenja stanja servisa
Prilikom dodavanja novog bez praćenja stanja servisa Visual Studio dodaje servisa projekta u rješenje koje sadrži vrstu descended iz `StatelessService`. Servis povećava lokalnu varijablu u brojač.

### <a name="stateful-service"></a>S praćenjem stanja servisa
Prilikom dodavanja novog s praćenjem stanja servisa Visual Studio dodaje servisa projekta u rješenje koje sadrži vrstu descended iz `StatefulService`. Servis povećava brojač u njegov `RunAsync` način i pohranjuje rezultat na `ReliableDictionary`.

### <a name="actor-service"></a>Servis za glumca
Prilikom dodavanja novog pouzdanog glumca Visual Studio zbraja dvije projekata rješenje: projekta programa glumca i projekta programa sučelja.

Glumca projekta pruža metode postavka i početak vrijednost brojač koji pouzdano dosljedan unutar na glumca stanje. Project sučelja nudi sučelje koje druge servise mogu se koristiti za pozivanje na glumca.

### <a name="stateless-web-api"></a>Bez praćenja stanja API na webu
Bez praćenja stanja Web API projekta pruža osnovni web-servisa koje možete koristiti da biste otvorili aplikaciju vanjski klijenti. Dodatne informacije o projektu strukturirani, potražite u članku [API Web tkanina usluge usluge s ugovorima o OWIN koja se sama nalaze](service-fabric-reliable-services-communication-webapi.md).

### <a name="aspnet-core"></a>Temeljni platforme ASP.NET

Tkanina SDK servisa nudi isti skup ASP.NET osnovne predloške koje su dostupne za samostalne ASP.NET osnovne projekata: prazna, [Web API][aspnet-webapi], i [Web-aplikacije][aspnet-webapp].

## <a name="next-steps"></a>Daljnji koraci
### <a name="create-an-azure-cluster"></a>Stvaranje Azure klaster
Tkanina SDK servis nudi lokalne klaster za razvoj i testiranje. Da biste stvorili klaster u Azure, pogledajte [Postavljanje klaster tkanina servisa na portalu Azure][create-cluster-in-portal].

### <a name="try-deploying-to-azure-for-free-with-party-clusters"></a>Pokušajte besplatno implementacije za Azure s klastere proizvođača

Ako želite pokušati implementacija i upravljanje aplikacijama u Azure bez postavljanja vlastite klastere, možete koristiti besplatnu [servis klaster proizvođača](http://aka.ms/tryservicefabric).

### <a name="publish-your-application-to-azure"></a>Objavljivanje aplikacija za Azure
Možete objaviti aplikacije izravno iz Visual Studio Azure klaster. Dodatne informacije potražite u odjeljku [Objavljivanje aplikacija za Azure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Pomoću programa Explorer tkanina servisa vizualizacija svoj klaster
Servis tkanina Explorer nudi jednostavan način vizualizacija svoj klaster, uključujući distribuiranih aplikacije i fizički izgled. Dodatne informacije potražite u članku [vizualizacija svoj klaster pomoću programa Explorer tkanina servisa][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Verzije i nadogradnje servisa
Servis tkanina omogućuje neovisno određivanjem verzije i nadogradnje neovisno servisa u aplikaciji. Dodatne informacije potražite u članku [Određivanje verzije i nadogradnje servisa][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Konfiguriranje neprekinuti integracije sa servisima tima za Visual Studio
Da biste saznali kako možete postaviti neprekinuti Integracija postupak za svoju aplikaciju servisa tkanina, potražite u članku [Konfiguriranje neprekinuti Integracija s programom Visual Studio Team Services][ci-with-vso].


<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
