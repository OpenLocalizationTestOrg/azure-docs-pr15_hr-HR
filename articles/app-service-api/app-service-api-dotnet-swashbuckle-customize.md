
<properties 
    pageTitle="Prilagodba definicije Swashbuckle generira API-JA" 
    description="Saznajte kako prilagoditi definicije Swagger API koje generira Swashbuckle za aplikaciju API-JA u servisu Azure aplikacije." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/29/2016" 
    ms.author="rachelap"/>

# <a name="customize-swashbuckle-generated-api-definitions"></a>Prilagodba definicije Swashbuckle generira API-JA 

## <a name="overview"></a>Pregled

U ovom se članku objašnjava kako prilagoditi Swashbuckle učiniti uobičajeni scenariji u kojima ćete htjeti promijeniti zadano ponašanje:

* Swashbuckle generira duplicirane operacija identifikatore overloads kontroler metode
* Swashbuckle pretpostavlja da je valjan je samo odgovor metode HTTP 200 (u redu) 
 
## <a name="customize-operation-identifier-generation"></a>Prilagodba operacija identifikator generacije

Swashbuckle generira Swagger operacija identifikatora spajanjem kontroler ime i naziv način. Ovaj uzorak stvara problem kada imate više overloads metode: Swashbuckle generira duplicirane operacija ID-a koji je valjan Swagger JSON.

Na primjer, sljedeći kod kontroler uzrokuje Swashbuckle da biste generirali tri Contact_Get operacija ID-a.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Problem možete riješiti ručno tako da date metode jedinstvene nazive, kao što su sljedeće u ovom primjeru:

* Početak
* GetById prijavio
* GetPage

Druga je da biste proširili Swashbuckle da biste ga automatsko generiranje operacija jedinstveni ID-a.

Sljedeći koraci pokazati kako prilagoditi Swashbuckle pomoću datoteke *SwaggerConfig.cs* uvrštene u projektu predložak projekta za Visual Studio API Apps (pretpregled).  U programu project Web API-jem za koju konfigurirate za implementaciju kao aplikaciju API možete prilagoditi i Swashbuckle.

1. Stvaranje prilagođene `IOperationFilter` implementacije 

    Na `IOperationFilter` sučelja nudi točku proširenja za korisnike Swashbuckle koji želite prilagoditi razne aspekte postupka Swagger metapodataka. Sljedeći kod prikazuje jedan od načina promjene operacija, id i generiranje ponašanje. Kod dodaje Nazivi parametara id naziv operacije.  

        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
        
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }

2. U datoteci *App_Start\SwaggerConfig.cs* poziva na `OperationFilter` način uzrokuju Swashbuckle da biste koristili novi `IOperationFilter` implementacije.

        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();

    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)

    Datoteka *SwaggerConfig.cs* koje paket Swashbuckle NuGet prekida sadrži mnogo komentar iz primjera točke proširenja. Dodatni Komentari nisu ovdje prikazani. 

    Nakon što napravite ove promjene na `IOperationFilter` implementaciji koristi i uzrokuje operacija jedinstveni ID-a generiranje.
 
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>
    
## <a name="allow-response-codes-other-than-200"></a>Dopusti odgovor kodovi osim 200

Prema zadanim postavkama Swashbuckle pretpostavlja da je odgovor HTTP 200 (u redu) je *samo* valjane odgovor metode Web API. U nekim slučajevima, preporučujemo vam da biste se vratili druge šifre odgovor bez uzrokuje klijenta do inicirati iznimku.  Na primjer, sljedeći kod Web API pokazuje scenarij u kojem želite bi klijent da biste prihvatili na 200 ili 404 kao valjani odgovore.

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

U ovom scenariju Swagger koji Swashbuckle generira prema zadanim postavkama određuje samo jedan valjane HTTP Šifra stanja, HTTP 200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Budući da Visual Studio koristi definiciju Swagger API-JA za generiranje koda za klijent, stvara klijent kod koji se podiže iznimku nikakav odgovor osim programa HTTP 200. Kod u nastavku je od C# klijenta za ovu metodu Web API uzorka generira.

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

Swashbuckle nudi dva načina prilagodbe na popisu očekivani HTTP odgovor šifre koje generira, pomoću XML komentara ili `SwaggerResponse` atribut. Jednostavnije je atribut, ali ga samo dostupna je u Swashbuckle 5.1.5 ili noviji. Predložak novi projekt pretpregled web-aplikacije API-JA u Visual Studio 2013 sadrži Swashbuckle verziju 5.0.0, pa ako koristi predložak, a ne želite da biste ažurirali Swashbuckle, vaša jedina mogućnosti da biste koristili XML komentare. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>Prilagodba očekivanim šifre pomoću XML komentara

Ovaj postupak koristite da biste naveli odgovor kodovi ako vaša verzija Swashbuckle prethodi 5.1.5.

1. Najprije dodajte komentare dokumentaciju XML putem metode želite navesti HTTP odgovor kodove za. Poduzimanja uzorka Web API akcija gore navedenoj sintaksi i Primjena dokumentaciju XML rezultat kod kao u sljedećem primjeru. 

        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
        
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
        
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

1. Dodajte upute u datoteci *SwaggerConfig.cs* za usmjeravanje Swashbuckle da biste koristiti XML datoteke dokumentacije.

    * Otvorite *SwaggerConfig.cs* i stvorite metode na klase *SwaggerConfig* da biste odredili put do datoteke XML dokumentacije. 

            private static string GetXmlCommentsPath()
            {
                return string.Format(@"{0}\XmlComments.xml", 
                    System.AppDomain.CurrentDomain.BaseDirectory);
            }

    * Pomaknite se prema dolje u datoteci *SwaggerConfig.cs* tako da se pojavi redak komentar iz koda nalik zaslona ispod. 

        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
    
    * Uklonite redak da biste omogućili komentare XML obrade tijekom generacije Swagger. 
    
        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
    
1. Da biste generirali XML datoteke dokumentacija, odlaze svojstva projekta i omogućite dokumentaciju XML datoteke kao što je prikazano u nastavku snimku zaslona. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

Kada dovršite korake u nastavku, JSON Swagger generira Swashbuckle odražavaju HTTP kodovima odgovor koji ste naveli u XML komentare. Snimka zaslona koja se nalazi ispod pokazuje ovo novi tereta JSON. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Kada koristite Visual Studio da biste Obnovi kod klijenta za REST API-JA, C# kod prihvaća HTTP u redu i ne može pronaći kodovi stanja bez prilikom otvaranja okvira podešavanje iznimku dopuštanja dosta kod da biste odluke o tome kako rukovati povratna null zapis o kontaktu. 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

Kod za ovu demonstraciju pronaći ćete u [ovom GitHub spremištu](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse). Uz Web API je projekt označiti se s komentarima dokumentaciju XML aplikacije konzole za projekt koji sadrži generira klijent za ovaj API. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Prilagodba očekivanim šifre pomoću atribut SwaggerResponse

Atribut [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) nije dostupna u Swashbuckle 5.1.5 i noviji. U slučaju da imate starije verzije u projektu, u ovom se odjeljku se pokreće po kojemu se objašnjava kako ažurirati paket Swashbuckle NuGet da bi mogli koristiti taj atribut.

1. U **Pregledniku rješenja**, desnom tipkom miša kliknite projekt Web API, a zatim kliknite **Upravljanje NuGet paketa**. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)

1. Kliknite gumb *Ažuriranje* uz paket NuGet *Swashbuckle* . 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)

1. Dodavanje atributa *SwaggerResponse* načinima Web API akciju za koju želite odrediti valjani HTTP kodovima odgovor. 

        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();

            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

2. Dodavanje u `using` izjava za polja na atribut naziva:

        using Swashbuckle.Swagger.Annotations;
        
1. Dođite do URL-a */swagger/docs/v1* projekta i različite šifre odgovor HTTP će biti vidljiv na Swagger JSON. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

Kod za ovu demonstraciju pronaći ćete u [ovom GitHub spremištu](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes). Uz Web API je projekt ukrašene s atributom *SwaggerResponse* aplikacije konzole za projekt koji sadrži generira klijent za ovaj API-JA. 

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku je prikazano kako prilagoditi način na koji se Swashbuckle generira operacija ID-a i šifre valjani odgovor. Dodatne informacije potražite u članku [Swashbuckle na GitHub](https://github.com/domaindrivendev/Swashbuckle).
 
