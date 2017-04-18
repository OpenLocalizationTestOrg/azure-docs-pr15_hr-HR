<properties
   pageTitle="Konfiguriranje Dijagnostika za servise u Oblaku Azure i virtualnim strojevima | Microsoft Azure"
   description="U članku se opisuje kako konfigurirati Dijagnostika informacije za ispravljanje pogrešaka servisa Azure cloude i virtualnih računala (VMs) u Visual Studio."
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

# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Konfiguriranje Dijagnostika za servise u Oblaku Azure i virtualnih računala

Kada je potrebno otklanjanje poteškoća s Azure u oblaku ili Azure virtualnog računala, možete konfigurirati tako Azure Dijagnostika jednostavnije pomoću Visual Studio. Azure Dijagnostika spremit će sustava podataka i zapisivanje podataka na virtualnim računalima i instance virtualnog računala koje pokrenuti servis u oblaku i prenosi podatke na račun za pohranu po izboru. Potražite u članku [Omogućivanje Dijagnostika bilježenje u zapisnik web-aplikacijama u aplikacije servisa za Azure](./app-service-web/web-sites-enable-diagnostic-log.md) dodatne informacije o Dijagnostika zapisivanje u Azure.

U ovoj se temi objašnjava omogućiti i konfigurirati Azure Dijagnostika u Visual Studio prije i nakon implementacije, kao i u Azure virtualnih računala. Također pokazuje kako da biste odabrali vrste Dijagnostika informacije da biste prikupili i pregledavanje informacija nakon prikupljaju se.

Dijagnostika Azure možete konfigurirati na sljedeće načine:

- Možete promijeniti postavke konfiguracije Dijagnostika putem dijaloškog okvira **Dijagnostika konfiguraciju** u Visual Studio. Postavke spremaju se u datoteku s nazivom diagnostics.wadcfgx (diagnostics.wadcfg Azure SDK 2.4 ili neke starije verzije). Osim toga, možete promijeniti izravno konfiguracijska datoteka. Ako ručno ažurirati datoteku, promjene konfiguracije primjenjuje se na sljedeću vrijeme implementacija oblaka servisa Azure ili pokrenuti servis u na emulator.

- Koristite **Oblaka Explorer** ili **Explorer poslužitelja** u Visual Studio za promjenu postavki dijagnostike pokrenuti servis u oblaku ili virtualnog računala.

## <a name="azure-26-diagnostics-changes"></a>Azure 2,6 Dijagnostika promjene

Za projekte Azure SDK 2,6 u Visual Studio, sljedeće su izvršene. (Promjene se primijeniti i novijim verzijama Azure SDK.)

- Lokalni emulator sada podržava Dijagnostika. To znači da možete prikupiti podatke za dijagnostiku i provjerite je li aplikacija stvara desnom kašnjenja dok ste razvoj i testiranje u Visual Studio. Niz za povezivanje `UseDevelopmentStorage=true` omogućuje prikupljanje podataka Dijagnostika dok se izvodi oblaka servisa projekta u Visual Studio pomoću emulator Azure prostora za pohranu. Svi podaci za dijagnostiku prikupljaju se u račun za pohranu (razvoj spremište).

- Dijagnostika za pohranu računa niz veze (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) opet se pohranjuje u datoteci konfiguracije (.cscfg) servisa. U Azure SDK 2,5 nije naveden račun za dijagnostiku prostora za pohranu u datoteci diagnostics.wadcfgx.

Postoje neke najvažnije razlike između kako niz za povezivanje radili u Azure SDK 2.4 i noviji i kako funkcionira u 2,6 SDK Azure i novijim verzijama.

- U Azure SDK 2.4 i starijim verzijama, niz za povezivanje je koristio kao u vrijeme izvođenja dodatak za dijagnostiku da biste dobili informacije o računu za pohranu za prijenos dijagnostičkog zapisnika.

- Azure SDK 2,6 i novijim verzijama niz za povezivanje Dijagnostika koristi Visual Studio da biste konfigurirali proširenje Dijagnostika odgovarajuću prostora za pohranu podataka o računu na tijekom objavljivanja. Niz za povezivanje omogućuje određivanje prostora za pohranu za različite račune za različite servisne konfiguracija koji Visual Studio će se koristiti za objavljivanje. Međutim, jer dodatak za dijagnostiku više nije dostupan (nakon Azure 2,5 SDK), datoteka .cscfg sam ne možete omogućiti nastavak Dijagnostika. Morate omogućiti nastavak zasebno kroz alate kao što su Visual Studio ili PowerShell.

- Da biste pojednostavili postupak konfiguriranja proširenje Dijagnostika sa servisom PowerShell paketa Izlaz iz Visual Studio sadrži javno konfiguracija XML za dijagnostiku proširenja za svaku ulogu. Visual Studio koristi niz za povezivanje Dijagnostika za popunjavanje prostora za pohranu podataka o računu na koje su prisutne u javnom konfiguracije. Datoteka javno config stvaraju se u mapi proširenja i slijedite uzorak PaaSDiagnostics. &lt;Naziv uloge >. PubConfig.xml. Sve implementacije PowerShell temelji možete koristiti ovaj uzorak da biste mapirali svaki konfiguraciju u ulogu.

- Niz za povezivanje u datoteci .cscfg i koristi [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) za pristup podacima Dijagnostika tako da ga mogu se prikazivati na kartici **praćenja** . Konfiguriranje servisa za prikazivanje opširno nadzora podataka na portalu potrebno je niz za povezivanje.

## <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migriranje projekata 2,6 SDK Azure i novijim verzijama

Prilikom migriranja iz Azure SDK 2,5 Azure SDK 2,6 ili noviji, ako imate račun za pohranu Dijagnostika naveden u datoteci .wadcfgx, zatim ostat će postoji. Da biste iskoristili fleksibilnost korištenja prostora za pohranu za različite račune za pohranu za različite konfiguracije, morat ćete niz za povezivanje ručno dodati u projekt. Ako projekt migrirate od Azure SDK 2.4 ili neke starije verzije za Azure SDK 2,6, zatim Dijagnostika nizu za povezivanje zadržavaju. Međutim, uzmite u obzir promjene u kako nizove veze tretiraju u Azure SDK 2,6 kao što je navedeno u prethodnom odjeljku.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Kako Visual Studio određuje račun za pohranu dijagnostiku

- Ako niz za povezivanje dijagnostike nije naveden u datoteci .cscfg, Visual Studio je koristi za konfiguriranje proširenje Dijagnostika za objavljivanje i pri stvaranju xml datoteke javno konfiguracije tijekom pakiranje.

- Ako nema Dijagnostika niz za povezivanje nije naveden u datoteci .cscfg, zatim Visual Studio pada natrag da biste pomoću računa za pohranu naveden u datoteci .wadcfgx da biste konfigurirali proširenje Dijagnostika kada objavljivanje i generiranje xml datoteke javno konfiguracije kada pakiranje.

- Niz za povezivanje Dijagnostika u datoteci .cscfg ima prednost pred račun za pohranu u datoteci .wadcfgx. Ako niz za povezivanje dijagnostike nije naveden u datoteci .cscfg, zatim Visual Studio koji koristi i zanemaruje račun za pohranu u .wadcfgx.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Što znači "ažuriranje razvoj prostora za pohranu veze nizovi..." potvrdni okvir učiniti?

Potvrdni okvir **nizove veze ažuriranje razvoj prostora za pohranu za dijagnostiku i Međuspremanje s Microsoft Azure prostora za pohranu vjerodajnica računa pri objavljivanju Microsoft Azure** omogućuje praktičan način da biste ažurirali sve nizove razvoj prostora za pohranu računa povezivanja s računom Azure prostora za pohranu određenih tijekom objavljivanja.

Na primjer, pretpostavimo potvrdite ovaj okvir, a zatim niz za povezivanje Dijagnostika određuje `UseDevelopmentStorage=true`. Kad objavite projekt da biste Azure, Visual Studio ažurirat će se automatski Dijagnostika niz za povezivanje s računom za pohranu koje ste naveli u čarobnjaku za objavljivanje. Međutim, ako realni prostora za pohranu računa navedeno je kao niz za povezivanje Dijagnostika, zatim taj račun koristit će se.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Dijagnostika funkcionalnost razlike između Azure SDK 2.4 i starijim i Azure SDK 2.5 i noviji

Ako nadograđujete projekta s Azure SDK 2.4 Azure SDK 2,5 ili noviji, koje treba nose na umu sljedeće razlike funkcionalnost Dijagnostika.

- **Konfiguracija API -ji su ukinuta** – programski konfiguracije dijagnostike sustava je dostupna u Azure SDK 2.4 ili starijim verzijama, ali ukinuta je u Azure SDK 2,5 i novijim verzijama. Ako vašoj konfiguraciji Dijagnostika trenutno definirane u kodu, morat ćete ponovno konfigurira te postavke ispočetka u programu project migriranim redoslijedom dijagnostike sustava da biste nastavili raditi. Konfiguracijska datoteka Dijagnostika za Azure SDK 2.4 je diagnostics.wadcfg i diagnostics.wadcfgx Azure SDK 2,5 i novijim verzijama.

- **Dijagnostika za oblak servisne aplikacije mogu se konfigurirati na razini uloge, ne na razini instance.**

- **Svaki put kada implementacija aplikacije, konfiguracije Dijagnostika ažurira** – to može izazvati probleme sa slične značajke ako promijenite konfiguraciju Dijagnostika iz programa Explorer poslužitelja i implementirati aplikaciju.

- **U Azure SDK 2,5 i noviji, rušenje ispisi konfiguriran u konfiguracijskoj datoteci dijagnostike nije u kodu** – ako imate rušenje ispisi konfiguriran u kodu, morat ćete ručno prenijeti konfiguraciju iz koda konfiguracijska datoteka jer ispisi rušenje ne prenose tijekom migracije 2,6 SDK Azure.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>Prije no što implementirate ih omogućiti i dijagnostiku u oblak servisa projektima

U Visual Studio, možete odabrati da biste prikupili podatke za dijagnostiku za uloge koji se izvode na Azure, prilikom pokretanja servisa u na emulator prije no što implementirate ga. Sve promjene postavki dijagnostike u Visual Studio spremaju se u konfiguracijskoj datoteci diagnostics.wadcfgx. Postavke konfiguracije Navedite račun za pohranu spremanje podataka za dijagnostiku ako pokrenete servis u oblaku.

### <a name="to-enable-diagnostics-in-visual-studio-before-deployment"></a>Da biste omogućili dijagnostika u Visual Studio prije implementacije

1. Na izborniku prečaca za ulogu koja vas zanima, odaberite **Svojstva**, a zatim odaberite karticu **Konfiguracija** u prozoru **Svojstva** za željenu ulogu.

1. U odjeljku **Dijagnostika** provjerite je li potvrđen okvir **Omogući Dijagnostika** .

    ![Pristup mogućnost Omogući dijagnostiku](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)

1. Odaberite gumb tri točke (…) da biste odredili računa za pohranu gdje želite da se Dijagnostika podaci koji se spremaju. Račun za pohranu odaberete bit će mjesta na kojem je pohranjena Dijagnostika podataka.

    ![Odredite poslovnog subjekta prostora za pohranu](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)

1. U dijaloškom okviru **Stvaranje niza za povezivanje za pohranu** odredite hoće li se želite povezati pomoću Azure Emulator prostora za pohranu pretplatu Azure ili ručno unijeli vjerodajnice.

    ![Dijaloški okvir Spremanje računa](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)

  - Ako odaberete Microsoft Azure prostora za pohranu Emulator mogućnost niz za povezivanje je postavljeno na UseDevelopmentStorage = true.

  - Ako se odlučite na mogućnosti pretplatu, možete odabrati Azure pretplatu koju želite koristiti i naziv računa. Možete odabrati gumb upravljanje računima za upravljanje pretplatama Azure.

  - Ako odaberete mogućnost ručno unijeti vjerodajnice, zatražit će se da biste unijeli naziv i ključ Azure račun koji želite koristiti.

1. Odaberite gumb **Konfiguracija** da biste prikazali dijaloški okvir **Konfiguracija Dijagnostika** . Svakoj kartici (osim **Općenito** i **Zapisnika direktorija**) predstavlja izvor dijagnostičkih podataka koji se može prikupiti. Zadanu karticu **Općenito**, nudi sljedeće mogućnosti zbirke podataka za dijagnostiku: **samo pogreške**, **sve podatke**i **prilagođeni plan**. Zadana mogućnost, **samo pogreške**, vodi najmanje količinu prostora za pohranu jer ne prijenos upozorenja ili praćenje poruke. Sve informacije mogućnost prenosi većina informacija i stoga je mogućnost najčešće skupi pomoću prostora za pohranu.

    ![Omogućivanje Azure Dijagnostika i konfiguracija](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

1. Na primjer, odaberite mogućnost **prilagođeni plan** tako da možete prilagoditi podatke prikupljene.

1. Okvir **Kvota diska u MB** određuje koliko prostora koji želite dodijeliti na vašem računu za pohranu za dijagnostiku podatke. Po želji možete promijeniti zadanu vrijednost.

1. Na svakoj kartici Dijagnostika podatke koje želite prikupiti odaberite njegov **omogućiti prijenos od <log type> ** potvrdni okvir. Na primjer, ako želite prikupljanje zapisnika aplikacije odaberite potvrdni okvir **Omogući prijenos zapisnika aplikacije** na kartici **Zapisnici aplikacije** . Osim toga, navedite sve informacije potrebno za svaku vrstu podataka za dijagnostiku. U odjeljku **izvori podataka za konfiguriranje Dijagnostika** u nastavku ove teme za podatke o konfiguraciji na svakoj kartici.

1. Kada omogućite skup svih željenih podataka za dijagnostiku, odaberite gumb **u redu** .

1. Pokrenite projekt servisa Azure oblak u Visual Studio kao i obično. Prilikom korištenja aplikacije zapisnika informacije koje ste omogućili sprema se u račun za Azure prostora za pohranu koje ste naveli.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Omogućivanje Dijagnostika na Azure virtualnim računalima sustava

U Visual Studio, možete odabrati da biste prikupili podatke za dijagnostiku za Azure virtualnih računala.

### <a name="to-enable-diagnostics-in-azure-virtual-machines"></a>Da biste omogućili dijagnostika na Azure virtualnim računalima sustava

1. U programu **Explorer poslužitelj**, odaberite čvor Azure i povežite se s pretplate Azure ako već nije ste povezani.

1. Proširite čvor **virtualnih računala** . Možete stvoriti novu virtualnog računala ili odabrati neki koji već postoji.

1. Na izborniku prečaca za virtualnog računala koja vas zanima, odaberite **Konfiguriraj**. Prikazuje dijaloški okvir konfiguracija virtualnog računala.

    ![Konfiguriranje Azure virtualnog računala](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)

1. Ako već nije instaliran, dodajte Proširenje Microsoft Dijagnostika Agent za nadzor. Ovaj kućni broj omogućuje prikupljanje podataka Dijagnostika za Azure virtualnog računala. Na popisu instaliran proširenja odaberite odabir programa dostupna proširenje padajući izbornik, a zatim odaberite Microsoft Dijagnostika Agent za nadzor.

    ![Instaliranje datotečni nastavak Azure virtualnog računala](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)

    >[AZURE.NOTE] Drugim datotečnim nastavcima Dijagnostika dostupni su za virtualnih računala. Dodatne informacije potražite u članku Azure VM proširenja i značajke.

1. Odaberite gumb **Dodaj** za dodavanje proširenje i prikaz njegov dijaloški okvir **Konfiguracija Dijagnostika** .

1. Odaberite gumb **Konfiguracija** za određivanje prostora za pohranu računa, a zatim odaberite gumb **u redu** .

    Svakoj kartici (osim **općenitih** i **Imenici zapisnik**) predstavlja izvor dijagnostičkih podataka koji se može prikupiti.

    ![Omogućivanje Azure Dijagnostika i konfiguracija](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

    Zadanu karticu **Općenito**, nudi sljedeće mogućnosti zbirke podataka za dijagnostiku: **samo pogreške**, **sve podatke**i **prilagođeni plan**. Zadana mogućnost, **samo pogreške**, vodi najmanje količinu prostora za pohranu jer ne prijenos upozorenja ili praćenje poruke. **Sve informacije o** mogućnost prenosi većina informacija i stoga je mogućnost najčešće skupi pomoću prostora za pohranu.

1. Na primjer, odaberite mogućnost **prilagođeni plan** tako da možete prilagoditi podatke prikupljene.

1. Okvir **Kvota diska u MB** određuje koliko prostora koji želite dodijeliti na vašem računu za pohranu za dijagnostiku podatke. Po želji možete promijeniti zadanu vrijednost.

1. Na svakoj kartici Dijagnostika podatke koje želite prikupiti odaberite njegov **omogućiti prijenos od <log type> ** potvrdni okvir.

    Na primjer, ako želite prikupljanje zapisnika aplikacije odaberite potvrdni okvir **Omogući prijenos zapisnika aplikacije** na kartici **Zapisnici aplikacije** . Osim toga, navedite sve informacije potrebno za svaku vrstu podataka za dijagnostiku. U odjeljku **izvori podataka za konfiguriranje Dijagnostika** u nastavku ove teme za podatke o konfiguraciji na svakoj kartici.

1. Kada omogućite skup svih željenih podataka za dijagnostiku, odaberite gumb **u redu** .

1. Spremite ažurirani projekt.

    Prikazat će se poruka u prozoru **Microsoft Azure aktivnosti Evidencija** virtualnog računala je ažuriran.

## <a name="configure-diagnostics-data-sources"></a>Konfiguriranje izvora podataka za dijagnostiku

Kada omogućite Dijagnostika prikupljanje podataka, možete odabrati točno koje izvori podataka koje želite prikupiti i podacima koji se prikupljaju. Slijedi popis kartice u dijaloškom okviru **Konfiguriranje dijagnostičkog** i znači koje svake mogućnosti konfiguracije.

### <a name="application-logs"></a>Zapisnici aplikacije

**Aplikacija zapisnici** sadrže Dijagnostika informacije koje je stvorio web-aplikacije. Ako želite snimiti zapisnika aplikacije, odaberite potvrdni okvir **Omogući prijenos zapisnika aplikacije** . Možete povećati ili smanjiti broj minuta kada zapisnike aplikacije prenose se na račun servisa za pohranu tako da promijenite vrijednost **Prijenos razdoblja (min)** . Možete promijeniti i količinu informacija zabilježene u zapisniku postavljanjem vrijednost razine zapisnika. Na primjer, možete odabrati **tekstni** da biste dobili dodatne informacije ili odaberite **od ključne važnosti** da biste snimili samo kritične pogreške. Ako imate određene Dijagnostika davatelja koji emits zapisnika aplikacije, možete ih snimiti dodavanjem GUID vašeg davatelja usluge u okvir **GUID davatelja usluga** .

  ![Zapisnici aplikacije](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  Potražite u članku [Omogućivanje Dijagnostika bilježenje u zapisnik web-aplikacijama u aplikacije servisa za Azure](./app-service-web/web-sites-enable-diagnostic-log.md) dodatne informacije o zapisnicima aplikacije.

### <a name="windows-event-logs"></a>Zapisnike događaja sustava Windows

Ako želite da biste snimili zapisnika događaja sustava Windows, odaberite potvrdni okvir **Omogući prijenos Windows zapisnika događaja** . Možete povećati ili smanjiti broj minuta kada zapisnika događaja prenose se na račun servisa za pohranu tako da promijenite vrijednost **Prijenos razdoblju (min)** . Odaberite potvrdne okvire za vrste događaje koje želite pratiti.

  ![Zapisnike događaja](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Ako koristite Azure SDK 2.6 ili noviji, a da biste odredili prilagođeni izvor, unesite ga u **<Data source name>** tekstni okvir, a zatim odaberite gumb **Dodaj** pokraj tog prikaza. Izvor podataka se dodaje diagnostics.cfcfg datoteku.

Ako koristite 2,5 SDK Azure, a da biste odredili prilagođeni izvor, možete dodati ga u `WindowsEventLog` dio na diagnostics.wadcfgx datoteku, kao što su kao u sljedećem primjeru.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Mjerača performansi

Informacije o performansama brojač olakšava pronalaženje grla sustava i precizno podešavanje performansi sustava i aplikacije. Potražite u članku [Stvaranje i korištenje mjerača performansi u aplikaciji Azure](https://msdn.microsoft.com/library/azure/hh411542.aspx) dodatne informacije. Ako želite da biste snimili mjerača performansi, potvrdite okvir **Omogući prijenos mjerača performansi** . Možete povećati ili smanjiti broj minuta kada zapisnika događaja prenose se na račun servisa za pohranu tako da promijenite vrijednost **Prijenos razdoblja (min)** . Odaberite potvrdne okvire za mjerača performansi u koje želite pratiti.

  ![Mjerača performansi](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Da biste pratili performanse brojač koja nije navedena, unesite pomoću preporučenih sintaksa, a zatim odaberite gumb **Dodaj** . Operacijski sustav na virtualnog računala određuje koje mjerača performansi možete pratiti. Dodatne informacije o sintaksi potražite u članku [Određivanje brojač put](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Infrastruktura zapisnika

Ako želite snimiti infrastrukture zapisnike, koji sadrže podatke o Azure dijagnostičkih infrastrukture modula za daljinski pristup i modula RemoteForwarder, odaberite potvrdni okvir **Omogući prijenos zapisnika infrastrukture** . Možete povećati ili smanjiti broj minuta kada zapisnike prenose se na račun servisa za pohranu tako da promijenite vrijednost prijenos razdoblja (min).

  ![Dijagnostika infrastrukture zapisnici](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  Dodatne informacije potražite u članku [Prikupljanje podataka zapisivanje prema korištenjem dijagnostike Azure](https://msdn.microsoft.com/library/azure/gg433048.aspx) .

### <a name="log-directories"></a>Zapisnik direktorija

Ako želite snimiti zapisnika direktorija, koje sadrže podatke prikupljene iz imenika zapisnika Internet Information Services (IIS) zahtjeva za neuspjelih zahtjeva ili mape koje odaberete, odaberite potvrdni okvir **Omogući prijenos zapisnika direktorija** . Možete povećati ili smanjiti broj minuta kada zapisnike prenose se na račun servisa za pohranu tako da promijenite vrijednost **Prijenos razdoblju (min)** .

Možete odabrati okvire zapisnike želite prikupiti, kao što su **IIS zapisnika** i zapisnika **Zahtjeva nije uspjelo** . Navode zadane nazive spremnik za pohranu, ali po želji promijenite nazive.

Osim toga, možete snimiti zapisnici iz bilo koje mape. Samo Navedite put u odjeljku **zapisnik apsolutne direktorija** , a zatim odaberite gumb **Dodaj direktorija** . U zapisnicima će biti zabilježene na navedeni spremnika.

  ![Zapisnik direktorija](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>Zapisnici ETW

Ako koristite [Događaj praćenje za Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803(v=vs.85).aspx) (ETW), a želite snimiti ETW zapisnike, potvrdite okvir **Omogući prijenos zapisnika ETW** . Možete povećati ili smanjiti broj minuta kada zapisnike prenose se na račun servisa za pohranu tako da promijenite vrijednost **Prijenos razdoblja (min)** .

Događaji snimaju iz izvora događaja i manifesti događaj koji navedete. Da biste odredili izvora događaj, u odjeljku **Izvori događaj** unesite naziv, a zatim odaberite gumb **Dodaj izvor događaja** . Isto tako, navedite programa manifest događaja u odjeljku **Manifesti događaja** , a zatim odaberite gumb **Dodavanje manifesta za događaj** .

  ![Zapisnici ETW](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  ETW framework podržane u ASP.NET kroz klase u [System.Diagnostics.aspx] (polje naziva https://msdn.microsoft.com/library/system.diagnostics (v=vs.110). Prostor naziva Microsoft.WindowsAzure.Diagnostics, što nasljeđuje od i proširuje standardna [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) klasa iz registra, omogućuje korištenje [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) kao framework zapisivanje u Azure okruženju. Dodatne informacije potražite u članku [Preuzimanje kontrole od bilježenje i praćenje u Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) i [Omogućivanje dijagnostici Azure servise u Oblaku i virtualnih računala](./cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Ispisi rušenje

Ako želite da biste snimili informacije o kada se ruši uloga instance, odaberite potvrdni okvir **Omogući prijenos ruši se ako ste** . (Jer ASP.NET ručice za većinu iznimke, to je obično korisno samo za uloge suradnika.) Možete povećati ili smanjiti postotak prostora za pohranu posvećen ispisi srušiti tako da promijenite vrijednost **Direktorija kvote (%)** . Možete promijeniti spremnik za pohranu gdje pohranjuju se srušiti ispisi i možete odabrati želite li snimiti **cijelog** ili **Mini** ispis.

Navedene su procesa koji se trenutno prate. Odaberite potvrdne okvire za procesa koje želite snimiti. Da biste dodali drugi proces na popisu, unesite naziv procesa, a zatim odaberite gumb **Dodaj postupak** .

  ![Ispisi rušenje](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  Potražite u članku [Preuzimanje kontrole od bilježenje i praćenje u Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) i [Microsoft Azure Dijagnostika dio 4: zapisivanje komponente Prilagođeno i Azure Dijagnostika 1.3 promjene](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) dodatne informacije.

## <a name="view-the-diagnostics-data"></a>Prikaz podataka za dijagnostiku

Kada ste prikupili podatke Dijagnostika za servise u oblaku ili virtualnog računala, možete je prikazati.

### <a name="to-view-cloud-service-diagnostics-data"></a>Da biste pogledali oblaka servis Dijagnostika podataka

1. Implementacija sustava u oblaku kao uobičajeni, a zatim ga pokrenuti.

1. Dijagnostika podataka u izvješću koje generira Visual Studio ili tablicama možete pogledati na vašem računu za pohranu. Za prikaz podataka u izvješću, Otvori **Explorer oblaka** ili **Explorer poslužitelja**, otvorite izbornički prečac čvora uloge koja vas zanima, a zatim odaberite **Prikaz dijagnostičkih podataka**.

    ![Prikaz podataka za dijagnostiku](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)

    Pojavit će se izvješće koje prikazuje dostupne podatke.

    ![Microsoft Azure Dijagnostika izvješće u Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)

    Ako se ne pojavi najnovije podatke, možda ćete morati pričekati za prijenos razdoblje proteći.

    Odaberite vezu **Osvježi** odmah ažurirati podatke ili odaberite interval **Automatskog osvježavanja** padajući okvir s popisom da se podaci automatski ažuriraju. Da biste izvezli podaci o pogrešci, odaberite gumb **Izvezi u CSV** da biste stvorili datoteku vrijednosti odvojenih zarezom možete otvoriti u proračunskoj tablici.

    U **Oblak Explorer** ili **Explorer poslužitelj**, otvorite račun za pohranu koji je pridružen uvođenje.

1. Otvorite Dijagnostika tablice u pregledniku tablice, a zatim pregledajte podatke koje ste prikupili. Za IIS zapisnika i prilagođeni zapisnike, možete otvoriti spremnik blob. Tako da pročitate tablici u nastavku, možete pronaći spremnik tablice ili blob koja sadrži podatke koji vas zanima. Uz podatke za tu datoteku zapisnika, unosa u tablici sadrže EventTickCount, DeploymentId, uloge i RoleInstance da biste lakše prepoznali koje virtualnog računala i uloge koje generira podatke i kada. 

  	|Dijagnostičkih podataka|Opis|Mjesto|
  	|---|---|---|
  	|Zapisnici aplikacije|Zapisnici koje generira kod tako da nazovete metode klase System.Diagnostics.Trace.|WADLogsTable|
  	|Zapisnike događaja|U ovom podaci iz zapisnika događaja sustava Windows na virtualnim računalima. Windows pohranjuje informacije u te zapisnike, ali aplikacija i servisa i pomoću njih pogrešaka ili zapisivanje podataka.|WADWindowsEventLogsTable|
  	|Mjerača performansi|Možete prikupiti podatke na bilo koji performanse brojač koji je dostupan na virtualnog računala. Operacijski sustav sadrži mjerača performansi, što obuhvaća mnogo Statistika kao što su vrijeme korištenja i procesor memorije.|WADPerformanceCountersTable|
  	|Infrastruktura zapisnika|Ti zapisnici generiraju infrastruktura za dijagnostiku sam.|WADDiagnosticInfrastructureLogsTable|
  	|IIS zapisnika|Ti zapisnici zapis zahtjeva za web. Ako servis u oblaku dobije veliku količinu promet, ti zapisnici mogu biti prilično dugotrajan da bi se trebali biste prikupljati i pohranjivati podatke samo kada su vam potrebni.|Možete pronaći nije uspjela zahtjev zapisnike u kontejner s blob u odjeljku wad-iis-failedreqlogs u odjeljku put za taj implementaciju, uloge i instance. Dovršavanje zapisnike u odjeljku wad iis logfiles možete pronaći. Stavke za svaku datoteku su izvršene u tablici WADDirectories.|
  	|Ispisi pad sustava|Ove informacije sadrži binarne slike oblaka servisa procesa (obično tempiranja uloga).|spremnik wad crush ispisi blob|
  	|Prilagođeni zapisničke datoteke|Zapisnici podatke koje ste unaprijed definirane.|Možete odrediti u kodu mjesto prilagođene zapisničke datoteke u račun za pohranu. Ako, na primjer, možete odrediti prilagođene blob spremnik.|

1. Ako podataka svih vrsta odbacuju se decimale, pokušajte povećati međuspremnik za te podatke vrsta ili ćete skratiti interval između prijenosi podataka iz virtualnog računala s računom za pohranu.

1. (neobavezno) Čišćenje podataka s računa za pohranu povremeno radi smanjivanja cjelokupne troškovi spremanja.

1. Kada to učinite cijelog implementaciju, diagnostics.cscfg datoteke (.wadcfgx za Azure SDK 2,5) se ažurira u Azure i servis u oblaku odabire sve promjene konfiguracije Dijagnostika. Ako se, umjesto toga ažurirate postojeće implementaciju, .cscfg datoteka ne ažurira se u Azure. I dalje promijenite postavki dijagnostike, no tako da slijedite korake u sljedećem odjeljku. Dodatne informacije o izvođenje puno implementacije i ažuriranje postojećih implementaciju potražite u članku [Objavljivanje čarobnjak aplikacije za Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Da biste vidjeli podatke Dijagnostika virtualnog računala

1. Na izborniku prečaca za virtualnog računala odaberite **Prikaz dijagnostike podataka**.

    ![Prikaz dijagnostike podataka u Azure virtualnog računala](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)

    Otvorit će se prozor za **dijagnostiku sažetka** .

    ![Dijagnostika Azure virtualnog računala sažetka](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  

    Ako se ne pojavi najnovije podatke, možda ćete morati pričekati za prijenos razdoblje proteći.

    Odaberite vezu **Osvježi** odmah ažurirati podatke ili odaberite interval **Automatskog osvježavanja** padajući okvir s popisom da se podaci automatski ažuriraju. Da biste izvezli podaci o pogrešci, odaberite gumb **Izvezi u CSV** da biste stvorili datoteku vrijednosti odvojenih zarezom možete otvoriti u proračunskoj tablici.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>Konfiguriranje oblaka servisa Dijagnostika nakon implementacije

Ako ste istražuje problem s prikazom oblaka servisa te već pokrenut, preporučujemo da biste prikupili podatke koje niste naveli prije izvorno implementiran ulogu. U tom slučaju možete pokrenuti prikupljanje podataka korištenjem postavki u programu Explorer poslužitelja. Dijagnostika za jednokratan ili sve instance možete konfigurirati u ulozi, ovisno o tome je li otvorili dijaloški okvir konfiguracija Dijagnostika za instancu ili uloga na izborniku prečaca. Ako konfigurirate čvor ulogu, promjene će se primijeniti na sve instance. Ako konfigurirate čvor instancu, promjene će se primijeniti na samo tu instancu.

### <a name="to-configure-diagnostics-for-a-running-cloud-service"></a>Da biste konfigurirali Dijagnostika za pokrenuti servis u oblaku

1. U programu Explorer poslužitelja Proširite čvor **Servise u Oblaku** , a zatim proširite čvorove da biste pronašli ulogu ili instancu koju želite istražiti ili i jedno i drugo.

    ![Konfiguriranje dijagnostičkog](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)

1. Na izborniku prečaca za programa čvor instanci ili uloga čvor odaberite **Ažuriranje postavki dijagnostike**, a zatim dijagnostičkih postavke koje želite prikupiti.

    Informacije o postavkama konfiguracije potražite u članku **izvora podataka za konfiguriranje Dijagnostika** u ovoj temi. Informacije o prikaz dijagnostike podataka potražite u članku **Prikaz dijagnostike podatke** u ovoj temi.

    Ako promijenite prikupljanje podataka u **Programu Explorer poslužitelj**, te promjene ostaju se na snazi dok se u potpunosti ponovno implementirate servis u oblaku. Ako koristite zadane postavke objavljivanja, promjene se prebrisati, jer zadani objavite je postavka ažuriranje postojećih implementacije umjesto da ne puni ponovno uvođenje. Da biste bili sigurni da isključite postavke trenutku implementaciju, idite na karticu **Napredne postavke** u čarobnjaku za objavljivanje, a zatim poništite potvrdni okvir za **implementaciju ažurirati** . Kada implementirati uz taj potvrdni okvir poništen, postavke vratit će onima iz datoteke .wadcfgx (ili .wadcfg) kao što je postavljeno putem uređivača svojstva za ulogu. Ako ažurirate uvođenja Azure zadržava stare postavke.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Otklanjanje poteškoća s problemi sa servisom Azure oblaka

Ako naiđete na probleme s oblaka projekte servisa, kao što su ulogu zapne u stanje "zauzet", pritišćite recycles ili throws interne pogreške poslužitelja, postoje Alati i tehnika kojima se možete koristiti za dijagnosticiranje i rješavanje tih problema. Primjere uobičajeni problemi i rješenja, kao i pregled koncepata i alata koji se koriste za dijagnosticirajte i otklonite takve pogreške, potražite u članku [Azure PaaS izračunati Dijagnostika podataka](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Značajka pitanja i odgovora

**Što je veličina međuspremnika te kako velike ga treba?**

Na svaku instancu virtualnog računala kvote ograničiti koliko dijagnostičkih podataka mogu pohraniti na lokalnom sustavu datoteka. Osim toga, odredite veličinu međuspremnik za svaku vrstu dijagnostičkih podataka koja je dostupna. U ovom Veličina međuspremnika ponaša se kao pojedinačne kvote za tu vrstu podataka. Potvrđivanjem dnu dijaloškog okvira možete odrediti cjelokupan kvote i memorija koji ostaje. Ako navedete veće međuspremnika ili dodatne vrste podataka, ćete postići cjelokupan kvote. Cjelokupan kvote možete promijeniti promjenom konfiguracijska datoteka diagnostics.wadcfg/.wadcfgx. Dijagnostika podaci spremaju se na isti datotečnom sustavu kao podatke vaše aplikacije pa ako aplikacija koristi mnogo prostora na disku, bi trebalo Povećajte cjelokupni kvotu za dijagnostiku.

**Što je prijenos razdoblje i koliko ga treba?**

Razdoblje za prijenos je vrijeme u minutama između podataka na kartici Umetanje. Nakon svakog razdoblja prijenos podataka se premjestiti iz lokalne datotečnom sustavu na virtualnog računala tablica u račun za pohranu. Ako količinu podataka koji se prikupljaju premašuje kvote prije kraja razdoblja prijenos, starije će se podaci odbaciti. Možda ćete morati smanjiti razdoblje prijenos ako ste gubitak podataka jer podataka premašuje veličinu međuspremnika ili cjelokupan kvote.

**Vremensku zonu su vremenske oznake u?**

Vremenske oznake su u lokalnu vremensku zonu podatkovnom centru koji hostira servis u oblaku. Sljedeće tri vremenske oznake stupca u tablicama zapisnika koriste.

  - **PreciseTimeStamp** je vremenska oznaka ETW događaja. To vrijeme putem klijentskog programa prijavljen je događaj.

  - **Vremenska OZNAKA** je PreciseTimeStamp zaokružuje granicu učestalost prijenos. Da, ako je učestalost prijenos pet minuta i događaj vrijeme 00:17:12, vremenska OZNAKA bit će 00:15:00.

  - **Vremenska oznaka** je vremenska oznaka entitet na kojem je stvorena u tablici Azure.

**Kako upravljati troškove vrijeme prikupljanja dijagnostičke informacije?**

Zadane postavke (**zapisnika razina** postavljena na **pogreške** i **Prijenos razdoblje** postavite na **1 minute**) su osmišljeni tako da biste minimizirali trošak. Troškove računalnim će se povećati ako prikupljanje dodatnih dijagnostičkih podataka ili smanjili prijenos razdoblja. Ne prikupite više podataka nego što vam je, a ne zaboravite da biste onemogućili prikupljanje podataka kada vam više nije potrebna. Možete uvijek je ponovno omogućiti, čak i tijekom rada, kao što je prikazano u prethodnom odjeljku.

**Kako prikupiti zapisnike nije uspjela zahtjev iz IIS?**

Prema zadanim postavkama IIS ne prikupljanje zapisnika zahtjeva nije uspjelo. Možete konfigurirati IIS tako da ih dohvatite Ako uređujete web.config datoteke za vašu web ulogu.

**Ne prikazuje se prati informacije iz RoleEntryPoint metode kao što je OnStart. Što ne valja?**

Metode RoleEntryPoint se nazivaju u kontekstu WAIISHost.exe, ne IIS. Dakle, informacije o konfiguraciji u web.config koji se obično ne omogućuje praćenje odnose. Da biste riješili taj problem, dodavanje .config datoteke u projekt uloga web i dajte naziv datoteci tako da odgovara izlazni skup koji sadrži RoleEntryPoint kod. Uloga projektu zadani web naziv datoteke .config bi WAIISHost.exe.config. Zatim dodajte sljedeće retke u tu datoteku:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Sada u prozoru **Svojstva** postavite svojstvo **Kopiraj izlaznog direktorija** **uvijek Kopiraj**.

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o Dijagnostika zapisivanje u Azure, potražite u članku [Omogućivanje dijagnostiku Azure servise u Oblaku i virtualnim strojevima](./cloud-services/cloud-services-dotnet-diagnostics.md) i [omogućiti i dijagnostiku bilježenje u zapisnik web-aplikacijama u servisu Azure aplikacije](./app-service-web/web-sites-enable-diagnostic-log.md).
