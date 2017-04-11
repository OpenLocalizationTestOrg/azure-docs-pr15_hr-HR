<properties
   pageTitle="Praktični vodič: Obradi EDIFACT fakture pomoću servisa Azure BizTalk | Servisa Microsoft Azure BizTalk"
   description="Kako stvoriti i konfigurirati aplikaciju okvir poveznika ili API-JA i koristiti u aplikaciji logike u aplikacije servisa za Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="deonhe"/>

# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Praktični vodič: Procesa EDIFACT fakture pomoću servisa Azure BizTalk Services
Na portalu servisa BizTalk možete koristiti za konfiguriranje i implementacija X12 i EDIFACT ugovore. U ovom ćete praktičnom vodiču smo pogledajte kako stvoriti EDIFACT ugovor za razmjenu fakture između poslovnih partnera. Pomoću ovog praktičnog vodiča zapisuje oko rješenje za kraj do kraja tvrtke koje obuhvaćaju dva poslovnih partnera, Northwind te Contoso razmjenjivati EDIFACT poruke.  

## <a name="sample-based-on-this-tutorial"></a>Ogledna na temelju ovog praktičnog vodiča
Pomoću ovog praktičnog vodiča zapisati oko uzorka, što **Pošaljete EDIFACT fakture pomoću BizTalk Services**, koja je dostupna za preuzimanje iz [Galerije MSDN kod](http://go.microsoft.com/fwlink/?LinkId=401005). Nije moguće iskoristite ugrađene ogledne i proći kroz ovaj Praktični vodič da biste razumjeli kako je komponenti uzorka. Ili možete koristiti ovaj Praktični vodič da biste stvorili vlastiti rješenje dna naviše. Pomoću ovog praktičnog vodiča je namijenjena drugi način tako da znate kako je komponenti rješenja. Osim toga, najveću moguću, vodič dosljedan uzorka i koristi iste nazive za artefakte (na primjer, sheme, pretvorbe) kao korištenih u uzorku.  

>[AZURE.NOTE] Jer je rješenje obuhvaća slanja poruke iz programa EAI mosta most za uređivanje tako ponovno koristite uzorka [BizTalk Services most Ulančavanje uzorka](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) .  

## <a name="what-does-the-solution-do"></a>Čemu služi rješenje?

U ovom rješenju Northwind prima EDIFACT fakture iz tvrtke Contoso. Faktura se ne nalaze u standardni oblik za uređivanje. Tako, prije no što pošaljete fakture Northwind ga mora biti pretvoreni dokument EDIFACT faktura (naziva se i INVOIC). Na potvrdu, Northwind morate obraditi fakture EDIFACT i vratite poruke kontrola (zovu se i CONTRL) Contoso.

![][1]  

Da biste postigli scenarij tvrtke Contoso koristi značajke omogućene sa servisa Microsoft Azure BizTalk Services.

*   Contoso koristi EAI bridges Pretvorba izvornu fakturu EDIFACT INVOIC.

*   Most EAI zatim šalje poruku most Pošalji uređivanje koja se implementirati kao dio ugovor konfiguriran na portalu servisa BizTalk.

*   Slanje most uređivanje obrađuje EDIFACT INVOIC i preusmjerava Northwind.

*   Nakon primanja fakture Northwind vraća poruku u CONTRL na uređivanje primiti most implementirati kao dio ugovor.  

> [AZURE.NOTE] Po želji rješenje pokazuje kako koristiti grupnog slanja promjena za slanje fakture u grupama, umjesto slanja faktura zasebno.  

Da biste dovršili scenarij koristimo servisa Bus redova slanje fakture iz tvrtke Contoso Northwind ni primati potvrđivanje iz tvrtke Northwind. Redove moguće stvoriti pomoću klijentska aplikacija koje se preuzeti i uvrštava u paketu uzorka koja je dostupna kao dio ovog praktičnog vodiča.  

## <a name="prerequisites"></a>Preduvjeti

*   Morate imati prostor naziva Bus servisa. Upute o stvaranju prostor naziva potražite u članku [kako da biste: Stvaranje i izmjena polje naziva servisa Bus servis](https://msdn.microsoft.com/library/azure/hh674478.aspx). Javite nam pretpostavlja da ste već prostor naziva servisa Bus dodjeli, pod nazivom **edifactbts**.

*   Morate imati pretplatu na servise BizTalk. Upute potražite u članku [Stvaranje BizTalk servis pomoću Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=302280). Za ovaj vodič Javite nam pretpostavlja da imate pretplatu BizTalk Services, pod nazivom **contosowabs**.

*   Registrirajte se pretplate BizTalk servisa na portalu servisa BizTalk. Upute potražite u članku [Registracija BizTalk Implementacija servisa na portalu servisa BizTalk](https://msdn.microsoft.com/library/hh689837.aspx)

*   Morate imati instaliran je Visual Studio.

*   Morate imati BizTalk Services SDK instaliran. SDK možete preuzeti iz [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-the-service-bus-queues"></a>Korak 1: Stvaranje reda čekanja Bus servisa  
Rješenje koristi redova Bus servisa za razmjenu poruka između poslovnih partnera. Contoso i Northwind slanje poruka redova iz gdje bridges EAI i/ili uređivanje zauzeti ih. Za rješenja, potrebne su vam tri redova Bus servisa:

*   **northwindreceive** – Northwind prima fakturu iz tvrtke Contoso putem ovom slijedu.

*   **contosoreceive** – Contoso prima na potvrđivanje iz Northwind putem ovom slijedu.

*   **obustavljena** – sve obustavljeno poruke usmjeruje ovaj red. Poruka je obustavljena ako oni neće uspjeti tijekom obrade.

Možete stvoriti redove Bus servis pomoću klijentska aplikacija obuhvaćeno primjer paketa.  

1.  Na mjesto na kojem ste preuzeli uzorka, otvorite **Vodič slanje fakture pomoću BizTalk Services uređivanje Bridges.sln**.

2.  Pritisnite **F5** omogućuje stvaranje i pokretanje **Vodič klijentske** aplikacije.

3.  Na zaslonu unesite prostor naziva ACS Bus servisa, naziv izdavača i izdavatelj ključ.

    ![][2]  
4.  Okvir s porukom traži tri redovi će se stvoriti u prostor za naziv Bus servisa. Kliknite **u redu**.

5.  Ostavite klijentski vodič pokrenut. Otvori, kliknite **Servis Bus** > **_prostora za naziv servisa Bus_** > **redovi**, pa provjerite je li se stvaraju tri reda čekanja.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Korak 2: Stvaranje i implementacija poslovnih partnerom
Stvaranje poslovnih partnerom između Contoso i Northwind. Kotiranja partnerom definira ugovor trgovinu između dva poslovni partneri, kao što su sheme koje poruke da biste koristili, razmjenu protokol koristiti itd. Kotiranja partnerom sadrži dvije bridges za uređivanje, slanje poruka poslovnih partnera (naziva se **poslati uređivanje most**) i primati poruke od poslovnih partnera (naziva se **primanje uređivanje most**).

U kontekstu rješenja, slanje most uređivanje odgovara Pošalji strani ugovor, a koristi se za slanje fakture EDIFACT iz tvrtke Contoso Northwind. Isto tako, most primanje uređivanje odgovara primanje strani ugovor, a koristi se za primanje acknowledgements iz tvrtke Northwind.  

### <a name="create-the-trading-partners"></a>Stvaranje poslovnih partnera

Za početak s stvaranje poslovnih partnera za web-mjesto tvrtke Contoso i u okvir za Northwind.  

1.  Na portalu servisa BizTalk na kartici **partnera** , kliknite **Dodaj**.

2.  Na stranici novo partnera **Contoso** kao unesite naziv partnera, a zatim kliknite **Spremi**.

3.  Ponovite korak da biste stvorili drugi partner **tvrtke Northwind**.  

### <a name="create-the-agreement"></a>Stvorite ugovor
Poslovnih partnera ugovore stvaraju se između profile tvrtke trgovina partnera. Ovo je rješenje koristi zadani partnera se profili koji se stvaraju automatski kada koju smo stvorili u partnera.  

1.  Na portalu servisa BizTalk kliknite **ugovore** > **Dodaj**.

2.  Na stranici **Opće postavke** novi ugovor, navedite vrijednosti, kao što je prikazano na slici u nastavku, a zatim **Nastavi**.

    ![][3]  

    Nakon što kliknete **Dalje**, dvije kartice **Primanje postavke** i **Slanje** postaju dostupne.

3.  Stvorite ugovor za slanje između Contoso i Northwind. Ovaj ugovor određuje način Contoso šalje fakture EDIFACT Northwind.

    1.  Kliknite **Pošalji postavke**.

    2.  Zadržavanje zadane vrijednosti na karticama **Dolazni URL**, **Pretvaranje**i **Batching** .

    3.  Na kartici **protokol** , u odjeljku **sheme** prenesite shemi **EFACT_D93A_INVOIC.xsd** . Ovu shemu dostupan je sa primjer paketa.

        ![][4]  
    4.  Na kartici **prijenosa** odredite detalje za redove Bus servisa. Za ugovor za slanje strani koristimo **northwindreceive** reda čekanja za slanje fakture EDIFACT Northwind i **obustavljeno** reda čekanja za usmjeravanje sve poruke koje se neće tijekom obrade te će se obustavlja. Stvorite redove u **Korak 1: Stvaranje reda čekanja Bus servisa** (u nastavku).

        ![][5]  

        U odjeljku **prijenosa postavke > prijenos vrsta** i **poruke obustavljanje postavke > prijenos vrsta**, odaberite Bus servisa Azure i navedite vrijednosti, kao što je prikazano na slici.

4.  Stvorite ugovor primanje između Contoso i Northwind. Ovaj ugovor određuje način Contoso prima na potvrđivanje iz tvrtke Northwind.

    1.  Kliknite **postavke primanja**.

    2.  Zadržavanje zadane vrijednosti na karticama **prijenosa** i **Pretvaranje** .

    3.  Na kartici **protokol** , u odjeljku **sheme** prenesite shemi **EFACT_4.1_CONTRL.xsd** . Ovu shemu dostupan je sa primjer paketa.

    4.  Na kartici **usmjeravanje** stvorite filtar da biste bili sigurni da su samo acknowledgements Northwind usmjeriti Contoso. U odjeljku **Postavke za usmjeravanje**, kliknite **Dodaj** da biste stvorili usmjeravanje filtar.

        ![][6]  
        1.  Unesite vrijednosti za **Naziv pravila**, **pravila za usmjeravanje**i **usmjeravanje odredište** kao što je prikazano na slici.

        2.  Kliknite **Spremi**.

    5.  Na kartici **usmjeravanje** ponovno odredite gdje se usmjeriti obustavljenom acknowledgements (acknowledgements koje ne prođu tijekom obrade). Postaviti vrstu prijenosa na Bus servisa Azure, usmjeravanje vrsta odredišnog **reda čekanja**, vrsta provjere autentičnosti **Zajednički pristup potpis** (SAS), omogućavaju SAS niz za povezivanje servisa Bus prostora za naziv, a zatim unesite naziv reda čekanja kao **obustavljeno**.

5.  Na kraju, zatim **Implementiraj** za implementaciju ugovor. Imajte na umu krajnjih točaka gdje slanje i primanje implementiraju ugovore.

    *   Na kartici **Slanje postavke** u odjeljku **Dolazni URL**, imajte na umu krajnju točku. Da biste poslali poruku Contoso Northwind pomoću most slanje za uređivanje, morate poslati poruku na ovom krajnjoj točki.

    *   Na kartici **Primanje postavke** u odjeljku **Prijenos**, imajte na umu krajnju točku. Da biste poslali poruku s Northwind Contoso pomoću na uređivanje primanje most, morate poslati poruku ovaj krajnje točke.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>Korak 3: Stvaranje projekt i implementirajte BizTalk Services

U prethodnom koraku implementiran uređivanje slanje i primanje ugovore za obradu EDIFACT fakture i acknowledgements. Ove ugovore možete samo procesa poruke koje u skladu sa shemom standardni EDIFACT poruke. Međutim, po scenarij rješenje Contoso šalje fakturu Northwind sesije vlasničkih sheme. Tako, prije slanja most uređivanje poslati poruku, ga mora biti pretvoreni iz sesije sheme standardne shemu fakture EDIFACT. Project BizTalk usluge EAI ne koji.

Projekt BizTalk usluga, **InvoiceProcessingBridge**, koji pretvara poruka je dio uzorka koji ste preuzeli. Projekt obuhvaća sljedeće artefakte:

*   **INHOUSEINVOICE. XSD** – sheme sesije računa koji se šalju Northwind.

*   **EFACT_D93A_INVOIC. XSD** – sheme standardni EDIFACT fakture.

*   **EFACT_4.1_CONTRL. XSD** – sheme potvrđivanje EDIFACT koji Northwind šalje Contoso.

*   **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – Pretvorba mapira shemi sesije fakture shemi standardni EDIFACT fakture.  

### <a name="create-the-biztalk-services-project"></a>Stvaranje projekta BizTalk Services
1.  U rješenje Visual Studio proširite InvoiceProcessingBridge projekta, a zatim otvorite datoteku **MessageFlowItinerary.bcs** .

2.  Kliknite bilo gdje u području crtanja i postavite **BizTalk servis URL** u okvir svojstva da biste odredili naziv svoje pretplate BizTalk Services. Na primjer, `https://contosowabs.biztalk.windows.net`.

    ![][7]  
3.  Iz alatnog okvira povucite **Xml One-Way most** u područje crtanja. Postavite svojstva **Entitet naziva** i **Relativne adrese** na most na **ProcessInvoiceBridge**. Dvokliknite **ProcessInvoiceBridge** da biste otvorili konfiguracije plošni most.

4.  U okviru **Vrste poruka** kliknite znak plus (**+**) da biste odredili sheme dolazne poruke. Budući da dolazne poruke za EAI bridge uvijek sesije fakture, postavite to **INHOUSEINVOICE**.

    ![][8]  
5.  Kliknite **Xml Pretvorba** oblika pa u okvir svojstva za svojstvo **karte** , kliknite gumb tri točke (****...). U dijaloškom okviru **Odabir mape** odaberite datoteku pretvorbe **INHOUSEINVOICE_to_D93AINVOIC** , a zatim **u redu**.

    ![][9]  
6.  Vratite se na **MessageFlowItinerary.bcs**i iz alatnog okvira, a zatim povucite **Two-Way krajnja točka za vanjske servisa** s desne strane **ProcessInvoiceBridge**. Postavite svojstvo njegov **Naziv entitet** **EDIBridge**.

7.  U pregledniku rješenja proširite **MessageFlowItinerary.bcs** , a zatim dvokliknite datoteku **EDIBridge.config** . Zamijenite sadržaj **EDIBridge.config** sljedeće.

    > [AZURE.NOTE] Zašto je potrebna uređivanje datoteke .config? Krajnja točka za vanjske servisa dodane na područje dizajnera most predstavlja bridges uređivanje koje ćemo implementirano neke starije verzije. Uređivanje bridges su dvosmjerna bridges, s je slanje i primanje strani. Međutim, most EAI koji smo dodali u dizajner most je jednosmjerna most. Tako, učiniti uzoraka exchange različite poruke za dva bridges koristimo prilagođene most ponašanje uključivanjem svoju konfiguraciju u .config datoteci. Uz to, Prilagođeno ponašanje rukuje i provjeru autentičnosti krajnjoj most uređivanje Pošalji. Takvo ponašanje prilagođene nije dostupan kao zasebna uzorka pri [BizTalk Services most Ulančavanje uzorka - EAI za uređivanje](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Ovo je rješenje ponovo koristi uzorka.  
    
    ```
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <extensions>
          <behaviorExtensions>
            <add name="BridgeAuthentication" 
                  type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
          </behaviorExtensions>
        </extensions>
        <behaviors>
          <endpointBehaviors>
            <behavior name="BridgeAuthenticationConfiguration">
              <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
              <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                    issuername="owner" 
                                    issuersecret="[YOUR ACS SECRET]" />
              <webHttp />
            </behavior>
          </endpointBehaviors>
        </behaviors>
        <bindings>
          <webHttpBinding>
            <binding name="BridgeBindingConfiguration">
              <security mode="Transport" />
            </binding>
          </webHttpBinding>
        </bindings>
        <client>
          <clear />
          <!--
            Go BizTalk Portal > Agreement > Send Settings > Inbound URL
            Copy the Endpoint URL and paste it in the below address field
          -->
          <endpoint name="TwoWayExternalServiceEndpointReference1" 
                    address="[YOUR EDI BRIDGE SEND URI]" 
                    behaviorConfiguration="BridgeAuthenticationConfiguration" 
                    binding="webHttpBinding" 
                    bindingConfiguration="BridgeBindingConfiguration" 
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />
        </client>
      </system.serviceModel>
    </configuration>

    ```
8.  Ažuriranje EDIBridge.config datoteke da biste uključili konfiguracije pojedinosti

    *   U odjeljku _<behaviors>_, ACS prostor naziva i ključ povezanog s pretplatom BizTalk Services.

    *   U odjeljku _<client>_, navedite krajnjoj točki gdje je implementiran Pošalji ugovor za uređivanje.

    Spremite promjene i zatvorite datoteku konfiguracije.

9.  Iz alatnog okvira, kliknite **poveznika** i uključiti komponente **ProcessInvoiceBridge** i **EDIBridge** . Odaberite poveznik pa u okviru svojstva postavite **Uvjet filtra** **Traži sve**. Time se osigurava da se sve poruke obradili most EAI usmjeriti most uređivanje.

    ![][10]  
10.  Spremanje promjena rješenje.  

### <a name="deploy-the-project"></a>Implementacija projekta

1.  Na računalu na kojem ste stvorili projekt usluga za BizTalk preuzmite i instalirajte SSL certifikat za pretplatu na servise BizTalk. U odjeljku BizTalk servisa kliknite **nadzorna ploča**pa kliknite **Preuzimanje SSL certifikata**. Dvokliknite potvrdi i slijedite upit da biste dovršili instalaciju. Provjerite je li instalirati certifikat u spremištu **Izdavanje korijenskih** certifikata.

2.  U programu Explorer Visual Studio u rješenje, desnom tipkom miša kliknite **InvoiceProcessingBridge** projekta, a zatim **Implementiraj**.

3.  Sadrže vrijednosti, kao što je prikazano na slici, a zatim **Implementiraj**. Možete dobiti ACS vjerodajnice za servise BizTalk tako da kliknete **Podatke o vezi** na nadzornoj ploči BizTalk Services.

    ![][11]  

    U oknu Izlaz, kopirajte krajnjoj točki gdje most EAI je implementiran, na primjer, `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Kasnije ćete ovaj URL krajnjoj točki.  

## <a name="step-4-test-the-solution"></a>Korak 4: Probno rješenje


U ovoj se temi smo pogledajte kako testirati rješenje pomoću **Vodič klijentske** aplikacije navedene kao dio uzorka.  

1.  U Visual Studio, pritisnite F5 da biste pokrenuli **Vodič klijenta**.

2.  Na zaslonu, morate imati vrijednosti prethodno iz koraka gdje koju smo stvorili redova Bus servisa. Kliknite **Dalje**.

3.  U sljedeći prozor navedite ACS vjerodajnice za pretplatu na servise BizTalk i krajnjih točaka gdje EAI i uređivanje (primanje) uvode se bridges.

    Krajnja točka za most EAI imali kopirali u prethodnom koraku. Za uređivanje dobiti krajnje most na portalu servisa BizTalk, idite na ugovor > primanje postavke > prijenos > krajnje točke.

    ![][12]  
4.  U sljedeći prozor u odjeljku Contoso, kliknite gumb **Pošalji sesije fakture** . U datoteci otvaranje dijaloškog okvira, otvorite datoteku INHOUSEINVOICE.txt. Pregledajte sadržaj datoteke, a zatim **u redu** da biste poslali fakturu.

    ![][13]  
5.  U nekoliko sekundi fakture se dobiva po Northwind. Kliknite vezu **Prikaz poruke** da biste vidjeli fakture primio Northwind. Obratite pozornost na to kako je faktura primio Northwind u standardnom EDIFACT shemi dok jedan poslao Contoso je shemom sesije.

    ![][14]  
6.  Odaberite faktura, a zatim kliknite **Pošalji potvrđivanje**. U dijaloškom okviru koji se pojavljuje primijetite da interchange ID isti u primljenim fakture i potvrđivanje koja se šalje. Kliknite u redu u dijaloškom okviru **Slanje potvrđivanje** .

    ![][15]  
7.  U nekoliko sekundi u potvrđivanje je uspješno primljen pri Contoso.

    ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Korak (nije obavezno) 5: slanje EDIFACT fakture u grupama 
Uređivanje servisa BizTalk bridges podržava i grupnog slanja promjena odlaznih poruka. Ova značajka je korisna za primanje partnere koji radije primaju skupine poruke (sastanka određenog kriterija) umjesto pojedinačnim porukama.

Najvažnije proporcije prilikom rada s grupama je stvarni izdanje grupe, naziva se i kriterij izdanje. Kriterij izdanje mogu se temeljiti na način na koji se primanju partnera želi primati poruke. Ako je omogućen grupnog slanja promjena, uređivanje most ne šalje odlazne poruke primanju partneru dok je ispunjena kriterij izdanje. Na primjer, batching kriterija na temelju dispatches veličina poruke seriji samo kada 'n' poruke su odbacivanja. Obradu kriterij može biti temeljene, tako da se seriji se šalje u nepromijenjenom vremenu svaki dan. U ovom rješenju smo pokušajte veličine za poruke na temelju kriterija.

1.  Na portalu servisa BizTalk kliknite ugovor koji ste prethodno stvorili. Kliknite Pošalji postavke > grupnog slanja promjena > Dodavanje grupe.

2.  Naziv grupe, unesite **InvoiceBatch**, unesite opis i zatim kliknite **Dalje**.

3.  Navođenje kriterija za obradu, koji određuje koje poruke moraju biti batched. U ovom rješenju smo obrade svih poruka. Tako, potvrdite okvir Koristi napredne mogućnosti definicije, a zatim unesite **1 = 1**. Ovo je uvjet koji se uvijek biti zadovoljen, a dakle će batched sve poruke. Kliknite **Dalje**.

    ![][17]  
4.  Navođenje kriterija za izdanje grupe. Padajući okvir, odaberite **MessageCountBased**i za **Brojanje**, navesti **3**. To znači da skupine tri poruke poslat će se Northwind. Kliknite **Dalje**.

    ![][18]  
5.  Pregledajte sažetak, a zatim kliknite **Spremi**. Zatim **Implementiraj** implementirati ugovor.

6.  Vratite se u **Klijent Praktični vodič**, kliknite **Pošalji sesije fakture**i slijedite upute za slanje fakture. Primijetit ćete da nema fakture dobiva po Northwind jer veličinu serije ne ispunjava kriterij. Ponovite ovaj korak dva više puta, tako da ne postoje tri fakture poruke poslane Northwind. To ispunjava kriterije izdanje obradu 3 poruka, a sada trebali biste vidjeti fakturu pri Northwind.


<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

