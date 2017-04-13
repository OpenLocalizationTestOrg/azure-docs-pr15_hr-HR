<properties
   pageTitle="Upravljanje ulogama u oblaku Azure services projektima u sustavu Visual Studio | Microsoft Azure"
   description="Saznajte kako dodati nove uloge u projekt servisa Azure oblaka ili uklanjanje postojeće uloge iz nje pomoću Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-roles-in-the-azure-cloud-services-projects-with-visual-studio"></a>Upravljanje ulogama u projekte servisa Azure oblaka sa Visual Studio

Nakon stvaranja projekta servisa Azure oblak, možete dodati nove uloge ili ukloniti postojeće uloge iz nje. Uvoz postojeći projekt i pretvorite ga u ulogu. Na primjer, Uvoz ASP.NET web-aplikacije i označiti kao uloge web.

## <a name="adding-or-removing-roles"></a>Dodavanje ili uklanjanje uloge

**Da biste dodali uloge**

U **Pregledniku rješenja**, otvorite izbornik prečaca za čvor **uloge** u projektu oblaka servisa i odaberite **Dodaj**. Možete odabrati postojeću web ulogu ili uloga suradnika iz trenutnog rješenja ili stvorite novi projekt uloga weba ili tempiranja. Ili možete odabrati odgovarajući projekt, primjerice programa project za aplikaciju web ASP.NET i povezati s programom project uloge.

**Da biste uklonili pridruživanja uloge**

Čvor **uloge** servisa project oblaka u programu Explorer rješenje otvorite izbornik prečaca za ulogu želite ukloniti, a zatim odaberite **Ukloni**.

## <a name="removing-and-adding-roles-in-your-cloud-service"></a>Uklanjanje i dodavanje uloge u servis u oblaku

Ako uklanjanje uloge servisa projekta oblaka, ali kasnije odlučite da biste dodali ulogu vratili u projekt, samo deklariranje uloga i osnovni atribute, kao što su podaci krajnje točke i dijagnostici dodaju. Dodatni resursi ni reference dodaju ServiceDefinition.csdef datoteku ili datoteku ServiceConfiguration.cscfg. Ako želite dodati taj podatak, morat ćete ga ručno dodati u te datoteke.

Na primjer, možda će uklonili uloge servisa web i kasnije odlučite da ta uloga ponovno dodati u rješenje. Ako to učinite, pojavit će se pogreška. Da biste onemogućili tu pogrešku, morate dodati na `<LocalResources>` element prikazana u sljedeće XML vratili u datoteku ServiceDefinition.csdef. Koristite naziv uloge web servis koji ste dodali u projekt kao dio atribut naziva za na **<LocalStorage>** element. U ovom primjeru naziv uloge servisa web je **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Daljnji koraci

Informirajte se o konfiguriranju uloga u Visual Studio tako da pročitate [Konfiguriraj ulogama za Azure Oblaku s Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).
