<properties
   pageTitle="Integracija Azure zapisnika najčešća pitanja vezana uz | Microsoft Azure"
   description="U ovim najčešćim Pitanjima navedeni odgovori na pitanja o Integracija Azure zapisnika."
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
   ms.date="08/23/2016"
   ms.author="TomSh"/>

# <a name="azure-log-integration-frequently-asked-questions-faq"></a>Integracija Azure zapisnika najčešća pitanja

U ovim najčešćim Pitanjima navedeni odgovori na pitanja o Integracija Azure zapisnika, servis koji omogućuje integraciju neobrađenog zapisnika iz Azure resursa u lokalni informacije o sigurnosti i upravljanje događaja (SIEM) sustavima. Ta integracija omogućuje objedinjenih nadzorne ploče za sve svoje imovine, lokalnog ili u oblaku, tako da možete zbrajanja, povezivanje, analizirati i upozorenja za sigurnost događaje vezane uz vaše aplikacije.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs-from"></a>Kako se računa za pohranu iz kojeg je integracija Azure zapisnika izvlačenja Azure VM zapisnika iz može vidjeti?

Pokrenite naredbu **azlog izvorišnog popisa**.

## <a name="how-can-i-update-the-proxy-configuration"></a>Kako ažurirati proxy konfiguracije?

Ako je postavka proxyja dopustite pristup Azure prostora za pohranu izravno, otvorite **AZLOG. EXE. KONFIGURACIJA** datoteke u **c:\Programske datoteke\Microsoft Azure zapisnika integracije**. Ažurirajte datoteku da biste uvrstili odjeljka **defaultProxy** s proxy adrese tvrtke ili ustanove. Po završetku ažuriranja zaustavite i pokrenuti servis pomoću naredbe **neto Zaustavi azlog** i **azlog neto start**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Kako vidjeti podatke o pretplati u stavkama događaja sustava Windows?

Dodavanje **subscriptionid** neslužbeni naziv prilikom dodavanja izvorišnog web-mjesta.

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  

Događaj XML sadrži metapodatke, kao što je prikazano u nastavku, uključujući id pretplate.

![Događaj XML][1]

## <a name="error-messages"></a>Poruka o pogrešci

### <a name="when-running-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Kada se pokrene naredbu **azlog createazureid**, zašto dolazi do sljedeće pogreške?

Pogreška:

  *Nije uspjelo stvaranje AAD aplikacija – smjernice za 72f988bf-86f1-41af-91ab-2d7cd011db37-razloga = "Zabranjeno" - poruku = "Ovlasti da biste dovršili postupak."*

**Azlog createazureid** pokušava stvorite glavni servisa u svim klijenata za Azure AD za pretplate na kojem Azure prijava ima pristup. Ako je vaš Azure prijava samo privremeni korisnik u klijentu Azure AD, zatim naredbu ne uspijeva s "Ovlasti da biste dovršili postupak." Zahtjev za administraciju klijenta za da biste dodali račun kao korisnik u klijentu.

### <a name="when-running-command-azlog-authorize-why-do-i-get-the-following-error"></a>Kada se pokrene naredbu **azlog autorizirali**, zašto dolazi do sljedeće pogreške?

Pogreška:

  *Upozorenje stvaranjem dodjele uloga - AuthorizationFailed: klijent janedo@microsoft.com' s objektom id "fe9e03e4-4dad-4328-910f-fd24a9660bd2" nema autorizacije za izvođenje akcija 'Microsoft.Authorization/roleAssignments/write' putem opseg "/ pretplate/70 d 95299 d689 4c 97-b971-0d8ff0000000".*

**Autorizirajte Azlog** naredba dodjeljuje ulogu čitač sa servisom Azure AD glavni (stvorena pomoću **Azlog createazureid**) pretplata navedena. Ako Azure Prijava nije zajednički Administrator ili vlasnik pretplate, ne uspijeva uz poruku o pogrešci koja se "Autorizacija nije uspjela". Azure na temelju uloga kontrolu pristupa (RBAC) zajednički Administrator ili vlasnik je potrebno dovršiti ovu akciju.

## <a name="where-can-i-find-the-definition-of-the-properties-in-audit-log"></a>Gdje pronaći definiciju svojstva u zapisniku nadzora?

Pročitajte sljedeće:

- [Operacije nadzora s Voditelj resursa](../resource-group-audit.md)
- [Popis upravljanje događaja u pretplatu u Azure Monitor REST API-JA](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Gdje pronaći pojedinosti o upozorenjima centar za sigurnost Azure?

Potražite u članku [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Kako odrediti koji se prikupljaju s VM Dijagnostika?

Pojedinosti o početak, a zatim Izmijeni, potražite u članku [Korištenje ljuske PowerShell za omogućivanje Azure Dijagnostika u virtualnog računala sa sustavom Windows](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) i postavljanje Dijagnostika Azure u sustavu Windows *(WAD)* konfiguracije. Slijedi primjer:

### <a name="get-the-wad-config"></a>Početak WAD config

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

### <a name="modify-the-wad-config"></a>Izmjena WAD Config

U sljedećem primjeru je konfiguraciju gdje prikupljeni Iddogađaja 4624 i Iddogađaja 4625 s sigurnosni zapisnik događaja. Microsoft Antimalware događaje prikuplja iz zapisnika događaja sustava. U odjeljku [troše događaje] (https://msdn.microsoft.com/library/windows/desktop/dd996910 (v=vs.85) pojedinosti o pomoću izraza XPath.

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

### <a name="set-the-wad-configuration"></a>Postavljanje konfiguracije WAD

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Kada unesete promjene, potvrdite okvir račun za pohranu da bi se prikupe točan događaja.

Ako imate pitanja o integracije zapisnik Azure, pošaljite poruku e-pošte da biste [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
