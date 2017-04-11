<properties
   pageTitle="Vodič za stvaranje podatkovnog servisa za Marketplace | Microsoft Azure"
   description="Detaljne upute kako stvoriti, potvrđivanje i implementacija podatkovnog servisa za kupnju na Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Mapiranje postojeće web-servisa na OData kroz CSDL

>[AZURE.IMPORTANT] **Trenutno ne možemo više nisu za uhodavanje sve nove izdavači podatkovnog servisa. Novi dataservices će dobiti odobrene za unos.** Ako imate aplikaciju SaaS tvrtke koje želite objaviti na AppSource možete pronaći dodatne informacije [u nastavku](https://appsource.microsoft.com/partners). Ako imate programa IaaS aplikacije ili servisa za razvojne inženjere koje želite objaviti na Azure Marketplace možete pronaći dodatne informacije [u nastavku](https://azure.microsoft.com/marketplace/programs/certified/).

U ovom se članku daje pregled o korištenju programa CSDL da biste mapirali postojeći servis OData kompatibilan servis. Objašnjava se kako stvoriti dokument mapiranje (CSDL) koji pretvorbe zahtjev za unos iz klijentskog programa putem servisa poziv i izlaz (podaci) ponovno klijentu putem kompatibilne OData sažetka sadržaja. Tvrtke Microsoft Azure Marketplace izlaže services s krajnjim korisnicima putem protokola OData. Servise koji su na raspolaganju davatelji sadržaja (podataka vlasnici) prikazat će se u različite obrazaca, kao što su OSTALE, SOAP, itd.

## <a name="what-is-a-csdl-and-its-structure"></a>Što je s CSDL i njenu strukturu?
CSDL (konceptualni sheme Definition Language) je specifikacija koji definira kako opisuju web-servisa ili servis baze podataka u uobičajeni verbiage XML Azure Marketplace.

Jednostavan pregled u **zahtjev tijek:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

**Toka podataka** nalazi se u suprotnom smjeru:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Slika 1** dijagrami kako klijent želite dobiti podatke iz davatelj sadržaja (uslugu) putem servisa Azure Marketplace.  Na CSDL koristi se komponenta za mapiranje transformaciju. za izvršenje zahtjeva i prosljeđivanje podataka između web-mjesto davatelja sadržaja servise i u okvir za traženje klijenta.

*Slika 1: Detaljne teče iz klijenta za traženje davatelja sadržaja putem servisa Azure Marketplace*

  ![Crtanje](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Za pozadinu na Atom Atom Pub i protokola OData na kojem proširenja trgovine Windows Azure stvaranja, pregledajte: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Isječak from above vezu:     *"Svrha protokola Open Data (potom se nazivaju OData) je protokol koji se temelji na OSTALE omogućavaju CRUD stil operacija (Stvaranje, čitanje, ažuriranje i brisanje) protiv resursi predstavljeni kao podatkovne usluge. "Podatkovnog servisa" je krajnje gdje nema podataka koji se prikazuje iz jednog ili više "zbirki" svaki stavkama nula ili više"", koji se sastoje od upisali parove pod nazivom vrijednosti. OData objavljuje Microsoft u odjeljku Standardi ORGANIZACIJA (tvrtku ili ustanovu napredovanje od strukturirane informacije Standardi) tako da se netko želi možete izraditi poslužiteljima, klijentima ili Alati bez tantijeme ili ograničenja."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Tri ključna dijelova koje se definira na CSDL su:

- **Krajnja točka** od na servis davatelja alatu Web adresa (URI) servisa
- **Parametri podataka** koji se kao ulaz davatelj usluga definicije parametre šalje sadržaja davatelja usluge na vrstu podataka.
- **Shema** podaci se vraćaju servisa Traži sheme podataka koji se isporučuje sadržaja davatelja usluge, uključujući spremnik, zbirke/tablice, varijable/stupce i vrste podataka.

Sljedeći dijagram prikazuje pregled teče iz gdje klijent unosi izjava OData (call davatelj sadržaja web-uslugu) da biste vratili dohvaćanje rezultata/podataka.

  ![Crtanje](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>Koraci:

1. Klijent šalje zahtjev putem servisa poziva s unos parametara definiranih u XML Azure Marketplace
2. CSDL koristi se za provjeru valjanosti poziva servisa.
    - U oblikovane servisa poziva pa šalju se sadržaja davatelji usluga po trgovine Windows Azure
3. Na Webservice izvršava i preforms akcija glagolski Http (odnosno DOBITI) podaci se vraćaju Azure Marketplace gdje je tražene podatke (ako ih ima) otkriva u XML formatu klijentu koristeći mapiranje definirano u na CSDL.
4. Klijent šalju podaci (ako ih ima) u obliku XML-a ili JSON

## <a name="definitions"></a>Definicija

### <a name="odata-atom-pub"></a>OData ATOM pub

Datotečni nastavak za ATOM pub pri čemu svaka stavka predstavlja jedan redak rezultata postavite. Dio sadržaja unosa je poboljšana tako da sadrže vrijednosti u stupcu – kao parove vrijednost ključa. Dodatne informacije o nalazi se ovdje: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - jezika za definiranje konceptualni sheme

Omogućuje definiranje funkcije (SPROCs) i entiteti koji su vidljiva kroz baze podataka. Dodatne informacije koje se ovdje: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [AZURE.TIP] Kliknite padajući izbornik **druge verzije** , a zatim odaberite verziju ako ne potražite u članku.

### <a name="edm---entry-data-model"></a>EDM - stavka podatkovnog modela
- Pregled: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink] [OverviewLink]: http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx
- Pregled: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink] [PreviewLink]: http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx
- Vrste podataka: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink] [DataTypesLink]: http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx

U sljedećem detaljne slijeva nadesno tijeka iz gdje klijent unosi izjava OData (call davatelj sadržaja web-uslugu) za dohvaćanje rezultata/podataka sigurnosno:

  ![Crtanje](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)


## <a name="csdl-basics"></a>Osnove CSDL

CSDL (konceptualni sheme Definition Language) je specifikacija koji definira kako opisuju web-servisa ili servis baze podataka u uobičajeni verbiage XML Azure Marketplace. Opisuje CSDL ključne pieces koji **omogućuje Prenos podataka iz izvora podataka Azure Marketplace.** Glavni dijelovi opisana su u nastavku:

- Informacije o sučelja s opisom svih javno dostupnih funkcija (FunctionImport čvora)
- Informacije o vrsti podataka za sve poruke requests(input) i responses(outputs) poruke (EntityContainer/EntitySet/Vrsta entiteta čvorove)
- Povezivanje informacije o protokol za prijenos treba koristiti (zaglavlje čvora)
- Informacije o adresi za pronalaženje navedeni servis (BaseURI atribut)

Da ne duljimo, u CSDL predstavlja ugovor platforme i jezika nezavisnih između podnositelja zahtjeva za usluge i davatelja usluga. Koristi se CSDL, klijenta možete pronaći web-servisa/baze podataka usluge i pozivanje neke njegove javno dostupnih funkcija.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Koji se odnose na CSDL u bazu podataka ili zbirku
**Specifikacija CSDL**

CSDL je XML gramatike za opis web-servisa. Specifikacija sam podijeljen 4 glavne elementi: EntitySet, FunctionImport; Prostor za naziv i Vrsta entiteta.

Da biste ovaj apstrakcije razumljivijim omogućuje povezivanje s CSDL u tablicu.

Imajte na umu;

  CSDL predstavlja ugovor platforme i jezika nezavisnih između **servisa podnositelja zahtjeva** i **usluga**. Pomoću CSDL **klijenta** možete pronaći na **web-servisa/baze podataka usluge** i pozivanje neke njegove javno dostupna **funkcijama.**

Za podatkovnog servisa četiri dijela u CSDL može biti mislili od baze podataka, tablice, stupca i postupak iz trgovine.

Koji se odnose to na sljedeći način za podatkovnog servisa:

- EntityContainer ~ = baze podataka
- EntitySet ~ = tablice
- Vrsta entiteta ~ = stupaca
- FunctionImport ~ = pohranjena procedura

**Glagoli HTTP dopuštene**
- Početak – vraća vrijednosti iz baze podataka (vraća zbirka)
- OBJAVA – koristi za prosljeđivanje podataka i neobavezna vraćene vrijednosti iz baze podataka (Stvaranje novog unosa u zbirci povrata id/URI)
- Brisanje – brisanje podataka iz baze podataka (briše zbirke)
- Postavljanje – ažuriranje podataka u na DB (zamjena zbirka ili stvorili)

## <a name="metadatamapping-document"></a>Dokument metapodatke/mapiranja

Dokument za metapodatke/mapiranje se koristi da biste mapirali sadržaja davatelja postojeće web-servisi tako da ga možete izložiti kao OData servis web sustav trgovine Windows Azure. Se temelji na CSDL i primjenjuje temelji nekoliko proširenja CSDL prema potrebama REST web-servisi vidljiva kroz trgovine Windows Azure. Proširenja nalaze se u [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) naziva.

Slijedi primjer na CSDL: (kopiranja i lijepljenja u sljedećem primjeru CSDL u poruku XML uređivač i promijeni tako da odgovara usluge.  Zatim ga zalijepite u CSDL mapiranja DataService kartici prilikom pisanja na servisu [Azure Marketplace Portal za objavljivanje](https://publish.windowsazure.com)).

**Uvjeta:** CSDL uvjete vezane uz uvjete [Portal za objavljivanje](https://publish.windowsazure.com) korisničkog Sučelja (PPUI).
- Nude "Naslov" u na PPUI odnosi MyWebOffer
- MyCompany u na PPUI odnosi **Publisher zaslonsko ime** [Razvojni centar za Microsoft](http://dev.windows.com/registration?accountprogram=azure) korisničkog Sučelja
- Vaš API odnosi weba ili podatkovnog servisa (Plan u u PPUI)

**Hijerarhija:** 
 poduzeće (davatelj sadržaja) vlasnik Offer(s) koji imaju Plan(s), odnosno service(s) koji redak gore s API.

### <a name="webservice-csdl-example"></a>Primjer WebService CSDL

Povezuje se s servis koji je način krajnje web aplikacije (kao što je C# aplikacija)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID" d:IsPrimaryKey="True" Type="Int32"  Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount" Type="Double"   Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"   Type="String"   Nullable="false" d:Map="./City"/>
        <Property Name="State"  Type="String"   Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime" Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [AZURE.TIP] Prikaz više primjera CSDL web-servisa u članku [primjeri mapiranja postojeće web-usluge OData kroz CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)

###<a name="dataservice-csdl-example"></a>Primjer DataService CSDL

Povezuje se s uslugom koja je način tablice baze podataka ili prikaz kao krajnje sljedećem primjeru pokazuje dvije API-ji za baza podataka koji se temelje CSDL API-JA (možete koristiti prikaze umjesto tablice).

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Vidi također
- Ako vas zanima učenje razumijevanje određene čvorove i njihovim parametrima pročitajte ovaj članak [Podataka usluge OData mapiranja čvorove](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definicije i objašnjenja, primjere i koristiti velika kontekst.
- Ako vas zanima pregled primjera, pročitajte ovaj članak [Podataka usluge OData mapiranja primjere](marketplace-publishing-data-service-creation-odata-mapping-examples.md) potražite u članku uzorak koda i razumijevanje Sintaksa koda i kontekst.
- Da biste vratili propisanim put za objavljivanje podatkovnog servisa Azure Marketplace, pročitajte ovaj članak [Vodič za objavljivanje servisa podataka](marketplace-publishing-data-service-creation.md).
