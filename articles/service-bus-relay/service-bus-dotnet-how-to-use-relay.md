<properties
    pageTitle="Kako koristiti Bus usluge preusmjeravanja s .NET | Microsoft Azure"
    description="Saznajte kako koristiti servis preusmjeravanja Bus servisa Azure za povezivanje dva aplikacije koje se nalaze na različitim mjestima."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="sethm"/>


# <a name="how-to-use-the-azure-service-bus-relay-service"></a>Upute za korištenje usluge preusmjeravanja Bus servisa Azure

U ovom se članku opisuje način korištenja usluge preusmjeravanja Bus servisa. Primjere zapisuju u C# i korištenje sustava Windows Communication Foundation (WCF) API-JA s datotečnim nastavcima koje se nalaze u sklopu servisa Bus. Dodatne informacije o Bus usluge preusmjeravanja potražite u članku pregled [razmjenu poruka servisa Bus povezivati](service-bus-relay-overview.md) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-the-service-bus-relay"></a>Što je Bus usluge preusmjeravanja?

Usluge [Bus usluge *preusmjeravanja* ](service-bus-relay-overview.md) vam omogućuje sastavljanje hibridnog programa koji se pokreću u Azure podatkovnog centra i vlastitu lokalnog okruženja enterprise. Bus usluge preusmjeravanja olakšava to omogućujući vam da biste sigurno otkrili servisi Windows Communication Foundation (WCF) koje se nalaze u mrežu tvrtke tvrtke javno oblak bez otvaranja veze s vatrozidom ili većih promjena unutar poslovne mrežne infrastrukture.

![Prijenos koncepti](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Prijenos Bus servisa omogućuje glavno računalo WCF servisa unutar postojeće okruženju. Zatim možete delegirati slušanje za dolazni sesije i zahtjevi za te servise WCF servisa Bus uslugu radi unutar Azure. To omogućuje vam da biste otkrili tih servisa aplikacija kod koji se izvodi u Azure ili zaposlenici zaduženi za mobilne ili ekstranet partnera okruženja. Servis Bus omogućuje sigurno određivanje tko može pristupiti tih servisa preciznije razine. Pruža Napredna i siguran način za izlaganje funkcionalnosti aplikacije i podatke iz svoje postojeća rješenja enterprise i iskoristiti ga iz oblaka.

U ovom se članku objašnjava kako koristiti Bus usluge preusmjeravanja za stvaranje web-servisa WCF, koji prikazuje korištenje povezivanja kanala TCP koji implementira sigurne razgovora između dvije strane.

## <a name="create-a-service-namespace"></a>Stvorite polje naziva servisa

Da biste počeli koristiti Bus usluge preusmjeravanja u Azure, prvo morate stvoriti prostor naziva. Prostor naziva nudi scoping spremnik adresiranja resursima servisa Bus unutar vaše aplikacije.

Da biste stvorili polje naziva servisa:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>Dohvaćanje paketa NuGet Bus servisa

[Paket NuGet Bus servis](https://www.nuget.org/packages/WindowsAzure.ServiceBus) je najjednostavniji način da biste dobili Bus API servisa, a da biste konfigurirali aplikaciju sa svim ovisnosti Bus servisa. Da biste instalirali paket NuGet u projektu, učinite sljedeće:

1.  U pregledniku rješenja, desnom tipkom miša kliknite **reference**, a zatim kliknite **Upravljanje NuGet paketa**.
2.  Traženje "Servisa Bus", a zatim odaberite stavku **Bus usluge za Microsoft Azure** . Kliknite da biste dovršili instalaciju **instalirati** , a zatim zatvorite dijaloški okvir sljedeće:

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="use-service-bus-to-expose-and-consume-a-soap-web-service-with-tcp"></a>Korištenje servisa Bus za izlaganje i korištenje web-servisa SOAP s TCP

Da biste u postojeći WCF SOAP web-servisa za vanjske potrošnje, morate napraviti promjene na servis povezivanja i adrese. To može zahtijevati promjene u konfiguracijskoj datoteci ili je nije potreban kod promjene, ovisno o tome kako ste postavili i konfigurirali WCF servisa. Imajte na umu da WCF omogućuje vam više mreža krajnje točke putem isti servisa, pa možete zadržati postojeći Interna krajnje točke prilikom dodavanja krajnje točke Bus servisa za vanjski pristup u isto vrijeme.

U ovom ćete zadatku će Stvaranje jednostavne servisa WCF i dodajte ga slušatelj Bus servisa. Ovu vježbu pretpostavlja neke poznavanje programa Visual Studio i zbog toga ne voditi kroz sve detalje o stvaranju projekta. Umjesto toga, ona se usredotočuje na kod.

Prije pokretanja ove korake, dovršite sljedeći postupak da biste postavili okruženje sustava:

1.  U Visual Studio stvaranje konzole za aplikacije koji sadrži dvije projektima, "Klijent" i "Usluga", unutar rješenja.
2.  Dodavanje paketa Microsoft Azure servisa Bus NuGet i projektima. Paket zbraja sve potrebne skupa reference na projektima.

### <a name="how-to-create-the-service"></a>Kako stvoriti usluge

Prvo, stvorite sam servis. Bilo koji servis za WCF sastoji se od najmanje tri različita dijelova:

-   Definicija ugovora koji opisuje se razmjenjivati koje poruke i što su operacije treba pozvati.
-   Implementacija rečeno ugovora.
-   Glavno računalo koje hostira servis WCF i izlaže nekoliko krajnje točke.

Primjeri kod u ovom odjeljku adresa svaku od ovih komponenti.

Ugovor definira jedne operacije `AddNumbers`, koji dodaje dvaju brojeva i vraća rezultat. Na `IProblemSolverChannel` sučelja omogućuje klijent da biste jednostavnije upravljali proxy života. Stvaranje takvo sučelje se smatra preporučenim načinom rada. Preporučuje se u da biste postavili definiciju ovog ugovora u zasebnu datoteku tako da možete referencirati datoteke iz vaše "Klijent" i "Usluge" projekata, ali kod možete kopirati i u oba projekata.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Implementacije je ugovor o mjestu trivial.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Konfiguriranje usluge glavno računalo programski

Ugovor i implementaciju na mjestu sada možete hostirati servis. Hostiranje pojavljuje unutar [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx) objekta koji vodi brigu o upravljanju instanci servisa i hostira krajnje točke koje preslušali za poruke. Sljedeći kod konfigurira servis s običnog lokalne krajnjoj točki i krajnje Bus servisa da biste ilustrirali izgled usporedno, s internim i vanjskim krajnje točke. Zamijenite niz *prostor naziva* s polja naziva i *yourKey* SAS ključ koji ste nabavili u prethodnom koraku postavljanje.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

U ovom primjeru stvoriti dvije krajnje točke koje se nalaze na isti implementaciju ugovora. Jedna je lokalni i jedan je predviđena putem servisa Bus. Ključne razlike među njima su povezivanja; [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) za lokalni jedan i [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) za krajnju točku Bus servisa i adrese. Lokalni krajnja točka ima adresu lokalne mreže s distinct priključak. Krajnja točka servisa Bus ima adresu krajnje točke sastojati od niza `sb`, polja naziva i put "alata za rješavanje." Rezultat te URI `sb://[serviceNamespace].servicebus.windows.net/solver`, prepoznavanje krajnju točku usluge kao krajnja točka servisa Bus TCP s potpuno kvalificiran naziv vanjskog DNS-a. Ako ste postavili kod zamjena rezerviranih mjesta u na `Main` funkcija **servisne** aplikacije, na raspolaganju su vam funkcionalni servisa. Ako želite da se na servisu da biste preslušali isključivo na servis Bus, uklonite deklariranje lokalne krajnjoj točki.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>Konfiguriranje usluge glavno računalo u datoteci App.config

Možete konfigurirati i glavnog računala pomoću datoteke App.config. Usluge hostinga kod u tom slučaju se pojavljuje u sljedećem primjeru.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Definicija krajnjoj točki premjestite App.config datoteku. Paket NuGet već dodana raspon definicije App.config datoteke koje su potrebne konfiguracije proširenja za Bus servisa. Sljedeći primjer koji je točan ekvivalent prethodne kod, prikazivati neposredno ispod **system.serviceModel** element. Kod pretpostavlja se da je prostor za naziv projekta C# pod nazivom **servisa**.
Rezerviranih mjesta zamijenite polje naziva servisa Bus servisa i SAS ključ.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Kad unesete promjene, pokreće servis jednak onome prije, ali s dva uživo krajnje točke: jedan lokalne i jedan priključuje u oblaku.

### <a name="create-the-client"></a>Stvaranje klijentskog programa

#### <a name="configure-a-client-programmatically"></a>Konfiguriranje klijenta programski

Za servis, možete sastaviti WCF klijenta [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) objekt. Servis Bus koristi token temeljenu na model implementirati pomoću SAS. Klase [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) predstavlja sigurnosni token davatelja pomoću ugrađenih tvorničke metode koje vraćaju Neki davatelji poznati tokena. Sljedeći primjer koristi metodu [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) za rukovanje acquisition odgovarajuće SAS token. Naziv i ključ su oni dobili na portalu opisan u prethodnom odjeljku.

Najprije reference ili kopiranje na `IProblemSolver` ugovora kod iz servisa u projektu klijenta.

Nakon toga zamjena kod u na `Main` način klijent, ponovno zamjena teksta na rezerviranim mjestima prostora za naziv Bus servisa i SAS ključ.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Sada možete izraditi klijenta i servisu sustava pokrenite ih (najprije pokrenuti servis), a klijent poziva usluge i ispisuje **9**. Postupak klijentske i poslužiteljske možete pokrenuti na različitim računalima, čak i u mrežama, a komunikacije će i dalje funkcionirati. Kod klijenta možete pokrenuti u oblak ili lokalno.

#### <a name="configure-a-client-in-the-appconfig-file"></a>Konfiguriranje klijenta u datoteci App.config

Sljedeći kod prikazuje kako konfigurirati klijenta pomoću datoteke App.config.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Definicija krajnjoj točki premjestite App.config datoteku. Sljedeći primjer koji je isto kao prethodno navedeni kod, prikazivati neposredno ispod **system.serviceModel** element. Ovdje, kao prije, morate zamijeniti rezerviranih mjesta prostora za naziv Bus servisa i SAS ključ.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove pružanja usluge preusmjeravanja Bus servisa, slijedite ove veze da biste saznali više.

- [Servis Bus povezivati razmjenu pregled](service-bus-relay-overview.md)
- [Azure servisa Bus pregled arhitekture](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- Preuzmite servisa Bus uzorka iz [Azure uzoraka][] ili potražite u članku [Pregled uzoraka Bus servisa][].

  [Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
  [Azure uzorka]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [Pregled servisa Bus uzorka]: ../service-bus-messaging/service-bus-samples.md