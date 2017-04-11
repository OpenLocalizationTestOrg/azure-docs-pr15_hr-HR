<properties
   pageTitle="Početak rada s Azure zapisnika Integracija | Microsoft Azure"
   description="Saznajte kako instalirati integraciju servisa Azure zapisnika i integracija zapisnika Azure prostora za pohranu, zapisnike nadzora Azure i centar za sigurnost Azure upozorenja."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="get-started-with-azure-log-integration-preview"></a>Početak rada s Azure zapisnika Integracija (pretpregled)

Integracija Azure zapisnika omogućuje integraciju neobrađenog zapisnika iz Azure resursa u lokalni informacije o sigurnosti i upravljanje događaja (SIEM) sustavima. Ta integracija omogućuje objedinjenih nadzorne ploče za sve svoje imovine, lokalnog ili u oblaku, tako da možete zbrajanja, povezivanje, analizirati i upozorenja za sigurnost događaje vezane uz vaše aplikacije.

Pomoću ovog praktičnog vodiča vodit će vas kroz kako instalirati Azure zapisnika integracije i integriranje zapisnika Azure prostora za pohranu, zapisnike nadzora Azure i centar za sigurnost Azure upozorenja. Procijenjena vrijeme za dovršetak ovog praktičnog vodiča je jedan sat.

## <a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj Praktični vodič, morate imati sljedeće:

- Na računalu (lokalni u oblaku) da biste instalirali integraciju servisa Azure zapisnika. Ovo računalo mora biti instalirano 64-bitni Windows OS-a s .net 4.5.1 instaliran. Ovo računalo zove **Azlog Integrator**.
- Azure pretplate. Ako ne postoji, možete se prijaviti [pomoću računa](https://azure.microsoft.com/free/).
- Azure Dijagnostika omogućen za Azure virtualnim strojevima (VMs). Da biste omogućili dijagnostika za servise u Oblaku, potražite u članku [Omogućavanje dijagnostike Azure u Azure servise u Oblaku](../cloud-services/cloud-services-dotnet-diagnostics.md). Da biste omogućili dijagnostika za programa Azure VM sa sustavom Windows, potražite u članku [Korištenje ljuske PowerShell da biste omogućili Azure Dijagnostika u izvodi Windows virtualnog računala](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md).
- Povezivanje s Azlog Integrator za Azure prostora za pohranu i za provjeru autentičnosti i autorizirali Azure pretplatu.
- Za Azure VM zapisnike, na Azlog Integrator mora biti instaliran agent SIEM (Ako, na primjer, Splunk univerzalni Forwarder, HP ArcSight Windows događaj Sakupljanje agent ili IBM QRadar WinCollect).

## <a name="deployment-considerations"></a>Okolnosti uvođenja

Više instanci Azlog Integrator mogu se pokrenuti ako događaj su visoka. Opterećenja Azure Dijagnostika račune za pohranu za Windows *(WAD)* i broj pretplate za pružanje instance se temelji na vašem kapaciteta.

Na računalu je procesor 8 (core) instancu Azlog Integrator možete obrade oko 24 milijuna događaja po danu (~1M/hour).

Na računalu procesor 4 (core), instancu Azlog Integrator možete obrade oko 1,5 milijuna događaja po danu (~62.5K/hour).

## <a name="install-azure-log-integration"></a>Instalacija Integracija Azure zapisnika

Preuzmite [Azure prijave integracije](https://www.microsoft.com/download/details.aspx?id=53324).

Integracija servisa Azure zapisnika prikuplja telemetrijskih podataka na računalu na kojem je instaliran.  Telemetrijskih podataka prikupljenih je:

- Iznimke koji se pojavljuju tijekom izvođenja Integracija Azure zapisnika
- Metriku o broju upite i događaje obrade
- Statistiku o koje Azlog.exe se koriste mogućnostima naredbenog retka

> [AZURE.NOTE] Možete isključiti skup telemetrijskih podataka poništavanjem tu mogućnost.

## <a name="integrate-azure-vm-logs-from-your-azure-diagnostics-storage-accounts"></a>Integrirati Azure VM zapisnike o poslovnim subjektima Azure Dijagnostika za pohranu

1. Pogledajte preduvjete navedene da biste bili sigurni računa za pohranu WAD je prikupljanje zapisnika prije nastavka vaše Integracija Azure zapisnika. Ako vaš račun za pohranu WAD je prikupljanje zapisnika, poduzeti sljedeće korake.

2. Otvorite naredbeni redak i **CD-a** u **c:\Programske datoteke\Microsoft Azure zapisnika integracije**.

3. Pokretanje naredbe

        azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>

      Zamijenite StorageAccountName naziv računa Azure prostora za pohranu koji je konfiguriran za primanje Dijagnostika događaje iz vaše VM.

        azlog source add azlogtest WAD azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==

      Želite li id pretplate koja će se prikazivati u događaja XML, dodati neslužbeni naziv ID pretplate:

        azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>

4. Pričekajte 30 do 60 minuta (može potrajati i iste duljine kao jedan sat), a zatim prikaz događaja koji se povlače s računa za pohranu. Da biste prikazali, otvorite **Preglednik događaja > zapisnici sustava Windows > prosljeđuju događaji** na Azlog Integrator.

5. Provjerite je li na računalu instaliran na standardni SIEM poveznik konfiguriran za odaberite događaje iz mape **Prosljeđuju događaja** , a zatim ih pipe vašoj SIEM instanci. Pregledajte SIEM određenu konfiguraciju konfigurirati i pogledajte zapisnike integriranje.

## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Što se događa ako podataka se ne prikazuju ni u mapu prosljeđuju događaja?

Ako nakon jedan sat podataka se ne prikazuje u mapi **Prosljeđuju događaje** pa:

1. Potvrdite na računalu, a zatim potvrdite da može pristupiti Azure. Da biste testirali povezivanje, pokušajte otvoriti [Azure portal](http://portal.azure.com) putem preglednika.

2. Provjerite je li račun korisnika **azlog** s dozvolom za pisanje u mapu **users\azlog**.

3. Provjerite je li na račun za pohranu dodali u naredbu **Dodaj azlog izvora** nalazi se prilikom pokretanja naredbe **azlog izvorišnog popisa**.
4. Idite na **Preglednik događaja > zapisnici sustava Windows > aplikacije** da biste vidjeli postoje li neke pogreške prijavljena iz integraciju sa servisom Azure zapisnika.

Ako i dalje ne vidite događaje, zatim:

1. Preuzmite [Microsoft Azure prostora za pohranu Explorer](http://storageexplorer.com/).

2. Povezivanje s računom za pohranu koji je dodan u naredbu **Dodaj azlog izvora**.

3. U programu Microsoft Azure prostora za pohranu Explorer potražite tablice **WADWindowsEventLogsTable** da biste vidjeli postoji li svi podaci. Ako nije, zatim Dijagnostika u na VM nije ispravno konfigurirano.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integriranje zapisnike nadzora Azure i centar za sigurnost upozorenja

1. Otvorite naredbeni redak i **CD-a** u **c:\Programske datoteke\Microsoft Azure zapisnika integracije**.

2. Pokretanje naredbe

        azlog createazureid

      Ta naredba od vas za prijavu na Azure. Naredba zatim stvara [Azure Active Directory servisa glavnicu](../active-directory/active-directory-application-objects.md) u Azure AD klijenata kojima su hostirani Azure pretplate u kojoj prijavljenog korisnika je Administrator, zajednički Administrator ili vlasnik. Naredba neće uspjeti ako je prijavljenog korisnika samo privremeni korisnik u klijentu za Azure AD. Provjera autentičnosti za Azure obavlja putem Azure Active Directory (AD).  Stvaranje glavni servis za integraciju Azlog stvara Azure AD identitet koji ćete dobiti pristup čitanju iz Azure pretplata.

3. Pokretanje naredbe

        azlog authorize <SubscriptionID>

      To se dodjeljuje čitač programa access u sklopu pretplate na servis u glavni stvorene u koraku 2. Ako ne navedete u SubscriptionID, pa ga pokušava dodijeliti ulogu upravitelja čitač servisa za sve pretplate na koji možete pristupiti bilo koje.

        azlog authorize 0ee9d577-9bc4-4a32-a4e8-c29981025328

      > [AZURE.NOTE] Upozorenja možete vidjeti ako pokrenete naredbu **autorizirali** odmah nakon naredbu **createazureid** . Postoji nekoliko latencije između prilikom stvaranja računa Azure AD i kada je račun dostupan za korištenje. Ako Pričekajte otprilike 10 sekundi nakon pokretanja naredbe **createazureid** za izvođenje naredbe **autorizirali** , pa ne vidjet ćete te upozorenja.

4. Provjerite da biste potvrdili da JSON datoteka zapisnika nadzora postoje sljedeće mape:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Provjerite sljedeće mape da biste potvrdili da Centar za sigurnost upozorava postoje u njima:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Pokažite standardni forwarder poveznik za SIEM datoteke u odgovarajuću mapu za pipe podataka na instancu SIEM. Možda ćete neke mapiranja polja na temelju SIEM proizvoda koji koristite.

Ako imate pitanja o integracije zapisnik Azure, pošaljite poruku e-pošte da biste [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču naučili kako instalirati Azure zapisnika integracije i integriranje zapisnika iz Azure prostora za pohranu. Dodatne informacije potražite u sljedećim člancima:

- [Microsoft Azure zapisnika integracije Azure zapisnika (pretpregled)](https://www.microsoft.com/download/details.aspx?id=53324) – centar za preuzimanje detalje i sistemski preduvjeti i upute za instalaciju vezana uz integraciju Azure zapisnika.
- [Uvod u Azure zapisnika Integracija](security-azure-log-integration-overview.md) – ovaj dokument predstavlja Integracija Azure zapisnika, njegovim ključa mogućnostima i kako to funkcionira.
- [Navedeni koraci za konfiguraciju partnera](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – ovaj članak na blogu prikazuje kako konfigurirao integraciju Azure zapisnika za rad s partnerskih rješenja Splunk, HP ArcSight i IBM QRadar.
- [Azure zapisnika Integracija često postavljana pitanja](security-azure-log-integration-faq.md) - ova najčešća pitanja vezana uz navedeni odgovori na pitanja o Integracija Azure zapisnika.
- [Centar za sigurnost Integrating upozorenja s Azure prijave Integracija](../security-center/security-center-integrating-alerts-with-log-integration.md) – ovaj dokument objašnjava da biste sinkronizirali upozorenja centar za sigurnost, zajedno s virtualnog računala sigurnosne događaje prikupljene putem Dijagnostika Azure i Azure nadzornih zapisnika prijava analitiku ili SIEM rješenja.
- [Nove značajke za Azure Dijagnostika i zapisnika nadzora Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – ovaj članak na blogu predstavlja zapisnike nadzora Azure i ostale značajke koje olakšavaju dobiti uvida u operacije Azure resurse.
