<properties
    pageTitle="OSTALE usluge Bus vodiča pomoću povezivati poruka | Microsoft Azure"
    description="Stvaranje jednostavne Bus usluge preusmjeravanja aplikacijom koja izlaže sučelje za OSTALE sustavom."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-rest-tutorial"></a>Praktični vodič servisa Bus preusmjeravanja REST

Pomoću ovog praktičnog vodiča opisuje način stvaranja jednostavne servisa Bus aplikacijom koja izlaže sučelje za OSTALE sustavom. OSTALE omogućuje web-klijenta, kao što je web-preglednik, pristup Bus API servisa putem HTTP zahtjeva.

Pomoću ovog praktičnog vodiča koristi Windows Communication Foundation (WCF) OSTALE programiranje model za sastavljanje servisa REST na servis Bus. Dodatne informacije potražite u članku [WCF OSTALE programiranje Model](https://msdn.microsoft.com/library/bb412169.aspx) i [dizajniranje i servise za implementaciju](https://msdn.microsoft.com/library/ms729746.aspx) u dokumentaciji WCF.

## <a name="step-1-create-a-service-namespace"></a>Korak 1: Stvaranje polje naziva servisa

U prvi je korak da biste stvorili prostor naziva i dobiti ključ zajednički pristup potpis (SAS). Prostor naziva nudi programa granicu aplikacije za svaku aplikaciju vidljiva kroz Bus servisa. Ključ SAS automatski se generira sustav stvaranja polje naziva servisa. Polje naziva servisa i SAS ključ nudi vjerodajnice za Bus servis za provjeru autentičnosti pristup aplikaciji.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-service-bus"></a>Korak 2: Definiranje utemeljen na OSTALE WCF ugovoru za uporabu Bus servisa

Kao s ostalim servisima Bus servisa prilikom stvaranja servisa REST stil, morate definirati ugovora. Ugovor određuje koje operacije podržava glavnog računala. Operacije na servis možete smatrati metodu web-servisa. Ugovori se stvaraju definiranjem C++, C# ili Visual Basic sučelja. Svaki način u sučelju odgovara određene operacije servisa. Atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) potrebno primijeniti na svaki sučelje i atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) mora se primijeniti na svaki postupak. Ako metoda u sučelje [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), taj način ne prikazuje se. Kod koji se koristi za sljedeće zadatke je prikazano u primjeru pomoću postupka opisanog.

Osnovna razlika između osnovni ugovora Bus servisa i OSTALE stil ugovora je dodatak svojstva da biste [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). To svojstvo omogućuje vam da biste mapirali metoda u svom sučelju korakom na drugoj strani sučelja. U ovom slučaju koristit ćemo [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) da biste se povezali metode HTTP GET. Time se omogućuje Bus servisa za točno dohvat i tumačenje naredbi koje se šalju sučelja.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Da biste stvorili Bus servisa ugovora sučelja

1. Otvorite Visual Studio u ulozi administratora: desnom tipkom miša kliknite program na izborniku **Start** , a zatim kliknite **Pokreni kao administrator**.

2. Stvaranje novog projekta konzole za aplikacije. Kliknite izbornik **datoteka** i odaberite **Novo**, a zatim odaberite mogućnost **projekt**. U dijaloškom okviru **Novi projekt** kliknite **Visual C#**, odaberite predložak **Aplikacije konzole** i nazovite ih **ImageListener**. Koristi zadano **mjesto**. Kliknite **u redu** da biste stvorili projekta.

3. C# projekta, Visual Studio stvara na `Program.cs` datoteku. Klase sadrži prazna `Main()` metoda potrebne za projekt aplikacije konzole za izgradnju pravilno.

4. Dodajte reference na servis Bus i **System.ServiceModel.dll** projekt tako da instalirate paket NuGet Bus servisa. Paket automatski dodaje reference na biblioteke Bus servisa, kao i WCF **System.ServiceModel**. U pregledniku rješenja, desnom tipkom miša kliknite **ImageListener** projekta, a zatim **Upravljanje NuGet paketa**. Kliknite karticu **Pregled** , a zatim potražite `Microsoft Azure Service Bus`. Kliknite **Instaliraj**i prihvatite uvjete korištenja.

5. Referenca **System.ServiceModel.Web.dll** morate izričito dodati projekta:

    na. U pregledniku rješenja, desnom tipkom miša kliknite mapu **reference** u mapi projekta, a zatim kliknite **Dodavanje referencu**.

    b. U dijaloškom okviru **Dodavanje referenca** , kliknite karticu **Framework** s lijeve strane, a u okvir za **pretraživanje** , upišite **System.ServiceModel.Web**. Potvrdite okvir **System.ServiceModel.Web** , a zatim kliknite **u redu**.

6. Dodajte sljedeće `using` naredbe pri vrhu datoteke Program.cs.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je mjesto za naziv koje omogućuju programatski pristup osnovne značajke WCF. Servis Bus koristi brojne objekti i atributi WCF da biste definirali ugovorima. Imenski će se koristiti u većini aplikacija Bus usluge preusmjeravanja. Isto tako, [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) omogućuje definiranje kanalom, što je objekt putem kojega komunicirate Bus servisa i web-preglednik klijenta. Na kraju, [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) sadrži vrste koje omogućuju stvaranje web-aplikacija.

7. Preimenovanje u `ImageListener` prostor naziva za **Microsoft.ServiceBus.Samples**.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Izravno nakon lijeva vitičasta deklaracije prostor naziva definiranje novo sučelje pod nazivom **IImageContract** i primijenite ga atribut **ServiceContractAttribute** sučelja s vrijednošću `http://samples.microsoft.com/ServiceModel/Relay/`. Vrijednost polja naziva se razlikuje od naziva koji koristite cijeloj opseg koda. Vrijednost polja naziva služi kao jedinstveni identifikator za ovaj ugovor i trebali biste dobiti informacije o verziji. Dodatne informacije potražite u članku [Rad s verzijama servisa](http://go.microsoft.com/fwlink/?LinkID=180498). Određivanje naziva izričito onemogućuje zadane vrijednosti polja naziva dodat naziv ugovora.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. Unutar na `IImageContract` sučelja, deklarirati način jedne operacije u `IImageContract` ugovora izlaže u sučelju i primijeniti na `OperationContractAttribute` atributa na način na koji želite da biste kao dio javnog servisa Bus ugovora.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. U atribut **OperationContract** dodajte **WebGet** vrijednost.

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    Time omogućuje Bus servis za usmjeravanje HTTP GET zahtjeve za `GetImage`, i da biste preveli vraćene vrijednosti `GetImage` u programa HTTP GETRESPONSE odgovor. Kasnije u ovom praktičnom vodiču, koristit ćete web-preglednik da biste pristupili ovu metodu, a za prikaz slike u pregledniku.

11. Odmah nakon na `IImageContract` definicija, deklarirati kanala koje nasljeđuju iz obje na `IImageContract` i `IClientChannel` sučelja.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Kanal je objekt WCF putem kojega klijent i servis za prosljeđivanje podataka međusobno povezani. Kasnije, morate stvoriti kanala u aplikaciji za glavno računalo. Servis Bus koristi ovaj kanal za prosljeđivanje zahtjeva za HTTP GET iz preglednika implementaciju **Mydomain** . Servis Bus koristi i kanal da biste snimili povratnu vrijednost **Mydomain** i Prevedite programa GETRESPONSE HTTP za preglednik klijenta.

12. Na izborniku **Sastavljanje** kliknite **Stvaranje rješenja** da biste potvrdili točnost svoje poslovne dosad.

### <a name="example"></a>Primjer

Sljedeći kod prikazuje osnovni sučelja koja definira Bus servisa ugovora.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Korak 3: Implementirati utemeljen na OSTALE WCF ugovoru za korištenje servisa Bus

Stvaranje usluge OSTALE stil servisa Bus potreban je najprije stvorite ugovor, koja je definirana pomoću sučelja. Sljedeći je korak za implementaciju sučelje. To obuhvaća stvaranje klase pod nazivom **ImageService** koji implementira **IImageContract** sučelja korisnički definirane. Nakon što implementirate ugovor, zatim konfigurirati sučelja pomoću App.config datoteke. Konfiguracijska datoteka sadrži potrebne informacije za aplikaciju, kao što je naziv servisa, naziv ugovora i vrstu protokol koji se koristi za komunikaciju s Bus servisa. U ovom primjeru pomoću postupka opisanog navedeni kod koji se koristi za sljedeće zadatke.

Kao što je s prethodnim koracima, postoji vrlo malo razlika između implementacijom ugovor OSTALE stil i osnovni Bus servisa ugovora.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>Možete implementirati OSTALE stil servisa Bus ugovora

1. Stvorite novi klase pod nazivom **ImageService** neposredno iza definiciju **IImageContract** sučelja. Klase **ImageService** implementira sučelje **IImageContract** .

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Slično druge implementacije sučelja, definiciju možete implementirati u neku drugu datoteku. No za ovog praktičnog vodiča implementacije pojavit će se u isti kao definiciju sučelja i `Main()` način.

2. Primjena [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) atribut klase **IImageService** da biste naznačili da je klasa implementacija WCF ugovora.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Kao što je već rečeno, imenski nije tradicionalni prostora za naziv. Umjesto toga je dio arhitektura WCF koja služi za identifikaciju ugovora. Dodatne informacije potražite u temi [Imena ugovora podataka](https://msdn.microsoft.com/library/ms731045.aspx) u dokumentaciji WCF.

3. Dodajte sliku .jpg u projekt.  

    To je slika koja prikazuje usluge u primanju web-pregledniku. Desnom tipkom miša kliknite projekt, a zatim kliknite **Dodaj**. Kliknite **Postojeću stavku**. Pomoću dijaloškog okvira **Dodavanje postojeće stavke** da biste pronašli odgovarajući .jpg, a zatim kliknite **Dodaj**.

    Prilikom dodavanja datoteke, obavezno da **Sve datoteke** na padajućem popisu uz stavku na **naziv datoteke:** polja. Do kraja ovog praktičnog vodiča pretpostavlja da je naziv slike "datoteke slika.jpg". Ako imate neku drugu datoteku, morat ćete preimenovati slike ili promijeniti kod da biste vaše.

4. Da biste provjerili je li servis izvode možete pronaći slikovne datoteke, u **Pregledniku rješenja** desnom tipkom miša kliknite datoteku slike, a zatim kliknite **Svojstva**. U oknu **Svojstva** postavite **Kopiraj izlaznog direktorija** kopiju **Ako novija**.

5. Dodavanje reference u sklopu **System.Drawing.dll** s projektom, a i dodajte sljedeće povezane `using` izvješća.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. U predmete **ImageService** dodajte sljedeće Graditelj koja učitava bitmape i Priprema za slanje preglednik klijenta.

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Izravno nakon prethodne, dodajte sljedeću metodu **Mydomain** klase **ImageService** da biste se vratili poruku HTTP koja sadrži sliku.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    Ova implementacija koristi **MemoryStream** za dohvaćanje slike i Priprema za strujanje u preglednik. Ga pokreće strujanje položaju nula deklarira strujanje sadržaja kao jpeg, a strujanja podataka.

8. Na izborniku **Sastavljanje** kliknite **Stvaranje rješenja**.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>Da biste definirali konfiguraciju radi web-servisa na servis Bus

1. U **Pregledniku rješenja**, dvokliknite **App.config** da biste ga otvorili u uređivaču za Visual Studio.

    Datoteka **App.config** nalikuje WCF konfiguracijska datoteka, a sadrži naziv usluge, krajnje točke (to jest, mjesto servisa Bus izlaže za klijente i domaćini komunikaciju s drugom) i povezivanje (Vrsta protokol koji se koristi za komunikaciju). Glavna razlika ovdje je krajnju točku konfigurirani usluge na [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) povezivanja koja nije dio .NET Framework.

2. Na `<system.serviceModel>` XML element je WCF element koji definira jedan ili više usluga. Ovdje se koristi da biste definirali naziv usluge i krajnjoj točki. Pri dnu zaslona u `<system.serviceModel>` element (, ali i dalje unutar `<system.serviceModel>`), dodajte na `<bindings>` element koji sadrži sljedeći sadržaj. Ovim se definira povezivanja koristiti u aplikaciji. Možete definirati više povezivanja, no za ovog praktičnog vodiča definirate samo jedan.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Ovaj korak definira Bus servisa [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) povezivanja s **relayClientAuthenticationType** postavljen na **ništa**. Tom se postavkom označava krajnje pomoću ovog povezivanja ne zahtijeva vjerodajnice klijenta.

3. Nakon što u `<bindings>` element, dodavanje u `<services>` element. Slično kao na povezivanja, većem broju servisa možete definirati u datoteci jedan konfiguracije. No za ovog praktičnog vodiča se definiraju samo jedan.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Ovaj korak konfigurira servis koji koristi prethodno definirane zadane **webHttpRelayBinding**. Koristi i **sbTokenProvider**zadani koja je definirana u sljedećem koraku.

4. Nakon na `<services>` element, stvorite na `<behaviors>` element s sljedeći sadržaj "SAS_KEY" zamjenjuje prethodno dobivenog [Azure portal][]ključa za *Potpis za zajedničko korištenje programa Access* (SAS).

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. I dalje u App.config, u na `<appSettings>` element, Zamijeni veze za cijeli niz vrijednost niz za povezivanje prethodno dobivenog na portalu. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. Na izborniku **Sastavljanje** kliknite **Stvaranje rješenja** da biste sastavili cijelu rješenja.

### <a name="example"></a>Primjer

Sljedeći kod prikazuje implementaciju ugovor i servis za servis utemeljen na OSTALE koji se izvodi na servis Bus pomoću **WebHttpRelayBinding** povezivanja.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Sljedeći primjer prikazuje datoteke App.config koji je povezan sa servisom.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-service-bus"></a>Korak 4: Usluge utemeljene na OSTALE WCF za korištenje servisa Bus glavnog računala

Ovaj korak opisuje kako pokrenuti web-servisa pomoću aplikacije konzole na Bus servisa. Cjelovit popis kod napisan u ovom ćete koraku navedeni u ovom primjeru pomoću postupka opisanog.

### <a name="to-create-a-base-address-for-the-service"></a>Da biste stvorili osnovne adrese za servis

1. U na `Main()` funkcija izvješća, stvorite varijablu za spremanje naziva projekta Bus servisa. Provjerite je li da biste zamijenili `yourNamespace` pod nazivom polje naziva servisa koji ste prethodno stvorili.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Servis Bus koristi naziv vaše prostor naziva za stvaranje jedinstvenog URI-JA.

2. Stvaranje na `Uri` instanca za osnovne adrese servisa koji se temelji na naziva.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Stvaranje i konfiguriranje web servis glavnog računala

- Stvaranje servis glavnog web adrese URI stvorili ranije u ovom odjeljku.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Glavno računalo za servis je objekt WCF koji instancira glavnog računala. U ovom se primjeru prosljeđuje je vrsta glavnog računala koju želite stvoriti (u **ImageService**) i adresu na kojoj želite izložiti glavnog računala.

### <a name="to-run-the-web-service-host"></a>Da biste pokrenuli servis glavnog računala web

1. Otvorite servis.

    ```
    host.Open();
    ```
    Sada je pokrenut servis.

2. Prikaz poruke koja označava je li pokrenut servis i kako zaustaviti servis.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Kada završite, zatvorite servis glavnog računala.

    ```
    host.Close();
    ```

## <a name="example"></a>Primjer

Sljedeći primjer uključuje ugovoru i implementaciju iz prethodnih koraka u ovom praktičnom vodiču i hostira uslugu u program konzole. Kompiliranje sljedeći kod u datoteku pod nazivom ImageListener.exe.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>Kompiliranje programskog koda

Nakon stvaranje rješenja, učinite sljedeće da biste pokrenuli aplikaciju:

1. Pritisnite **F5**ili dođite do mjesta izvršne datoteke (ImageListener\bin\Debug\ImageListener.exe) da biste pokrenuli servis. Zadrži aplikaciju pokrenut, kao što je potrebno da biste izvršili sljedeći korak.

2. Kopirajte i zalijepite adresu iz naredbenog retka u pregledniku da biste vidjeli sliku.

3. Kada završite, pritisnite tipku **Enter** u prozor naredbenog retka da biste zatvorili aplikaciju.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste već sastavljali aplikacije koja koristi uslugu preusmjeravanja servisa Bus, potražite u sljedećim člancima da biste saznali više o povezivati porukama:

- [Azure servisa Bus pregled arhitekture](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)

- [Upute za korištenje usluge preusmjeravanja servisa Bus](service-bus-dotnet-how-to-use-relay.md)

[Portal za Azure]: https://portal.azure.com