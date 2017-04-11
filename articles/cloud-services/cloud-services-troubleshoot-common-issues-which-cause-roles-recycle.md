<properties
   pageTitle="Uobičajeni uzroci servis u Oblaku uloge recikliranje | Microsoft Azure"
   description="Uloga servisa oblak koji iznenada recycles mogu prouzročiti značajan isključiti. Evo nekih uobičajeni problemi koji uzrokuju uloge biti brisanja, koji vam mogu pomoći pri smanjiti nedostupnost."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="common-issues-that-cause-roles-to-recycle"></a>Uobičajeni problemi koji uzrokuju uloge u koš za

U ovom se članku iznose neki od čestih uzroka problema implementacije i sadrži savjete za otklanjanje poteškoća za rješavanje tih problema. Znak postoji problem s aplikacija je kada instancu uloga ne pokreće ili kruži između stanja prilikom inicijalizacije, zauzet i zaustavljanje.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Nedostaju ovisnosti izvođenja

Ako je uloga u aplikaciji ovisi bilo koji nije dio .NET Framework ili na Azure upravlja biblioteku, morate izričito uključiti tog skupa u Aplikacijski paket. Imajte na umu druge Microsoft okviri nisu dostupne na Azure prema zadanim postavkama. Ako vaša uloga ovisi o takve framework, morate dodati te sklopova Aplikacijski paket.

Prije stvaranja i paket aplikacije, provjerite sljedeće:

- Ako koristite Visual studio, provjerite je li svojstvo **Lokalnu kopiju** postavljen na **True** za svaki referencirani skup u projektu koji nije dio Azure SDK ili .NET Framework.

- Provjerite je li web.config datoteke referencu sve Neiskorišteni sklopova sastavljanja element.

- **Sastavljanje akcija** sve datoteke .cshtml postavljen je na **sadržaja**. Na taj način datoteke pojavit će se pravilno u paket, a omogućuje datotekama referencirani smjestiti u paketu.

## <a name="assembly-targets-wrong-platform"></a>Pogrešan platformu za sastavljanje ciljnih web-mjesta

Azure je 64-bitni okruženje. Stoga .NET sklopova prevedena za 32-bitne cilj neće funkcionirati na Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Uloga throws neobrađenu iznimke vrijeme pokretanje ili prekidanje

Željene iznimke su je izbacio metode klase [RoleEntryPoint] koji obuhvaća [OnStart], [OnStop]i [pokrenite] metode, su neobrađenu iznimke. Ako se pojavi neobrađenu iznimku u jednom od ovih metoda, će koša za ulogu. Ako ulogu je recikliranje više puta, se možda je prijavi neobrađenu iznimku svaki put kada se pokuša pokrenuti.

## <a name="role-returns-from-run-method"></a>Uloga vraća iz načina pokretanja

Da biste pokrenuli beskonačno namijenjen je način za [pokretanje] . Ako je kod nadjačava način [pokrenuti] , ga treba stanje mirovanja beskonačno. Ako je način [pokrenuti] vraća, recycles ulogu.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Pogrešan DiagnosticsConnectionString postavka

Ako aplikacija koristi Azure Dijagnostika, konfiguracijskoj datoteci servisa morate navesti na `DiagnosticsConnectionString` postavke konfiguracije. Ta postavka navedite vezu s HTTPS na račun servisa za pohranu u Azure.

Kako bi vaše `DiagnosticsConnectionString` postavke nisu točne prije implementacije Aplikacijski paket Azure, provjerite sljedeće:  

- Na `DiagnosticsConnectionString` postavljanje točke račun valjana prostora za pohranu u Azure.  
  Prema zadanim postavkama ta postavka upućuje na račun Emulirani prostora za pohranu, tako da morate izričito promijenili tu postavku prije implementacije Aplikacijski paket. Ako niste promijenili tu postavku, kada instancu uloga pokuša pokrenuti dijagnostičkih monitor Izbačena je iznimka. To može uzrokovati instancu uloga u koš za beskonačno.

- U sljedećem [obliku](../storage/storage-configure-connection-string.md)navedena je niz za povezivanje. (Protokol se mora biti naveden kao HTTPS.) *MyAccountName* zamijenite nazivom vašeg računa za pohranu i *MyAccountKey* pomoću svojeg ključa programa access:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Ako razvijate aplikacije pomoću alata za Azure za Microsoft Visual Studio, koristite [stranice svojstava](https://msdn.microsoft.com/library/ee405486) za tu vrijednost postavite.

## <a name="exported-certificate-does-not-include-private-key"></a>Izvezene potvrde ne uključuje privatni ključ

Da biste pokrenuli web uloga u odjeljku SSL, morate osigurati da sadrži izvezene upravljanje certifikata privatni ključ. Ako koristite *Upravitelj certifikata u sustavu Windows* da biste izvezli certifikata, obavezno odaberite **da** da bi mogućnost **izvesti privatni ključ** . Certifikat morate izvesti u obliku PFX, samo oblikovanje trenutno podržava.

## <a name="next-steps"></a>Daljnji koraci

Prikaz više [članaka za otklanjanje poteškoća](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) za servise u oblaku.

Prikaz više uloga recikliranje scenariji u [nizu blogova Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Pokreni]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
