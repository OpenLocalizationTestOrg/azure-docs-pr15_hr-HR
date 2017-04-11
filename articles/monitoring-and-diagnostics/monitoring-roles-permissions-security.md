<properties
    pageTitle="Početak rada s uloge i dozvole vrijednosnice s monitora Azure | Microsoft Azure"
    description="Saznajte kako koristiti ugrađene uloga i dozvola Azure monitora da biste ograničili pristup resursi za nadzor."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Početak rada s uloge i dozvole vrijednosnice s monitora Azure

Mnoge timovima morati isključivo regulira pristup nadzor podataka i postavki. Na primjer, ako imate članovima tima koji rade na isključivo nadzor (inženjeri za podršku, devops inženjeri) ili ako koristite upravljanih usluga, preporučujemo vam da im dodijelite pristup samo praćenje podataka tijekom ograničavanje njihove mogućnost stvaranja, mijenjati ili brisati resursi. U ovom se članku prikazuje kako brzo primijeniti ugrađene nadzora RBAC uloge korisnika u Azure ili stvaranje vlastite prilagođene uloge za korisnika koji treba ograničene dozvole za nadzor. Opisuje se zatim sigurnosna pitanja vezana uz za resurse vezane uz Azure monitora i kako možete ograničiti pristup podacima koje sadrže.

## <a name="built-in-monitoring-roles"></a>Ugrađeni nadzora uloge

Ugrađeni uloge Azure Monitor namijenjeni za ograničavanje pristupa resursima servisa pretplate tijekom i dalje omogućivanja one odgovoran za infrastrukturu da biste dobili i konfiguriranje podataka koje su im potrebne za nadzor. Azure Monitor pruža dva Izlaz u-tvorničke uloge: čitač praćenje odgovora i nadzor suradnika.

### <a name="monitoring-reader"></a>Nadzor Reader

Osobe dodijeljena uloga nadzor čitač možete prikazati sve podatke o praćenju u pretplatu, ali ne možete izmijeniti bilo kojeg resursa ni uređivati postavke vezane uz nadzor resursa. Prikladna za korisnike u tvrtki ili ustanovi, kao što je podrška i postupci inženjeri koje moraju moći je ta uloga:

- Prikaz nadzora nadzorne ploče na portalu i stvaranje vlastitih privatne nadzora nadzornih ploča.
- Upit za metriku pomoću [Azure Monitor REST API -JA](https://msdn.microsoft.com/library/azure/dn931930.aspx), [cmdleta ljuske PowerShell](insights-powershell-samples.md)ili [EŽA različite platforme](insights-cli-samples.md).
- Upita pomoću portala, Azure Monitor REST API-JA, cmdleta ljuske PowerShell ili više platformi EŽA zapisnik aktivnosti.
- Prikaz [dijagnostičkih postavki](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) za resurs.
- Prikaz [zapisnika profila](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) za pretplatu.
- Postavke prikaza automatsko skaliranje.
- Prikaži upozorenje aktivnost i postavke.
- Pristup podacima uvida aplikacije i prikaz podataka u AI analize.
- Pretraživanje zapisnika analize (OMS) podatke radnog prostora uključujući podataka o korištenju za radni prostor.
- Prikaz grupe upravljanje prijava analitiku (OMS).
- Dohvaćanje shema pretraživanja prijava analitiku (OMS).
- Popis paketi obavještavanje prijava analitiku (OMS).
- Dohvaćanje i izvršavanje prijava analitiku (OMS) spremljena pretraživanja.
- Dohvaćanje prostora za pohranu konfiguracije prijava analitiku (OMS).

> [AZURE.NOTE] Ta uloga dati pristup čitanju podaci iz zapisnika koja ima strujanjem koncentratora za događaj ili spremljeni na račun za pohranu. Informacije o konfiguriranju pristup sljedećim resursima [potražite u nastavku](#security-considerations-for-monitoring-data) .

### <a name="monitoring-contributor"></a>Praćenje suradnika

Osobama dodijeliti ulogu suradnika nadzor možete pogledati sve podatke o praćenju u pretplatu i stvarati ili mijenjati postavke nadzora, ali ne možete mijenjati ostali resursi. Ta uloga je nadskup ulozi čitač nadzor i prikladno za članovi tima za nadzor ili upravljanih davateljima usluga koji uz dozvole iznad, morat ćete moći tvrtki ili ustanovi:

- Nadzor nadzornih ploča Objavi kao članak zajedničke nadzorne ploče.
- Postavljanje [dijagnostičkih postavki](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) za na resource.*
- Postavljanje [zapisnika profila](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) na subscription.*
- Postavljanje upozorenja aktivnosti i postavke.
- Stvaranje testira web aplikacije uvide i komponenata.
- Popis prijava analitiku (OMS) radnog prostora zajedničko korištenje ključeva.
- Omogućivanje i onemogućivanje paketi obavještavanje prijava analitiku (OMS).
- Stvaranje, brisanje i prijava analitiku (OMS) spremljena pretraživanja za izvršavanje.
- Stvaranje i brisanje prostora za pohranu konfiguracije prijava analitiku (OMS).

* korisnik mora biti i zasebno odobren ListKeys dozvola na ciljnom resursa (prostora za pohranu računa ili događaja koncentrator prostor naziva) da biste postavili zapisnika profila ili dijagnostičkih postavku.

> [AZURE.NOTE] Ta uloga dati pristup čitanju podaci iz zapisnika koja ima strujanjem koncentratora za događaj ili spremljeni na račun za pohranu. Informacije o konfiguriranju pristup sljedećim resursima [potražite u nastavku](#security-considerations-for-monitoring-data) .

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Nadzor dozvole i prilagođene RBAC uloge
Ako iznad ugrađene uloge ne odgovaraju točno potrebama vašeg tima, možete [stvoriti prilagođene uloge RBAC](../active-directory/role-based-access-control-custom-roles.md) s precizniji dozvolama. U nastavku slijede uobičajeni postupci RBAC monitora Azure s njihovi opisi.

| Postupak                                                   | Opis                                                                                                                                               |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Brisanje Microsoft.Insights/AlertRules/[Read, pisanje]         | Čitanje/pisanje/brisanje upozorenja pravila.                                                                                                                            |
| Microsoft.Insights/AlertRules/Incidents/Read                | Popis kupljenih (Povijest upozorenja pravila koja se pokreće) za upozorenja pravila. To se odnosi samo na portal.                                              |
| Brisanje Microsoft.Insights/AutoscaleSettings/[Read, pisanje]  | Automatsko skaliranje za čitanje/pisanje/Izbriši postavke.                                                                                                                     |
| Brisanje Microsoft.Insights/DiagnosticSettings/[Read, pisanje] | Čitanje/pisanje/brisanje dijagnostičkih postavki.                                                                                                                    |
| Microsoft.Insights/eventtypes/digestevents/Read             | Ova je potrebno za korisnike koji je potreban pristup aktivnosti zapisnike putem portala sustava.                                                                   |
| Microsoft.Insights/eventtypes/values/Read                   | Popis aktivnosti zapisnika događaja (Upravljanje događaja) u pretplatu. Tu dozvolu je dodijeljen programski i portala pristup u zapisnik aktivnosti. |
| Microsoft.Insights/LogDefinitions/Read                      | Ova je potrebno za korisnike koji je potreban pristup aktivnosti zapisnike putem portala sustava.                                                                   |
| Microsoft.Insights/MetricDefinitions/Read                   | Pročitajte metričkim definicije (popis raspoložive vrste metričkim resursa).                                                                                  |
| Microsoft.Insights/Metrics/Read                             | Pročitajte metriku resursa.                                                                                                                              |

> [AZURE.NOTE] Pristup upozorenja, dijagnostičkih postavki i metriku za resurs potreban je ima li korisnik pristup za čitanje vrsta resursa i opseg resursa. Stvaranje ("pisanje") dijagnostičkih postavka ili zapisnik profil koji arhivira s računom za pohranu ili strujanja s koncentratorima događaj zahtijeva korisniku i dozvolu ListKeys na ciljnom resursa.

Na primjer, koristeći gornju tablicu možete stvoriti prilagođeni RBAC uloge za programa "aktivnosti zapisnika čitač" ovako:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Sigurnosna pitanja vezana uz za praćenje podataka
Nadzorni podaci – osobito datoteka zapisnika – možete sadrži povjerljive podatke, kao što je IP adresa ili korisnička imena. Praćenje podataka iz Azure dolazi u tri osnovna oblika:
1. Zapisnik aktivnosti, koji opisuje sve akcije kontrole ravnini Azure pretplate.
2. Dijagnostičke zapisnike, koje su zapisnika čuje po resurs.
3. Metrike koji su čuje resursi.

Sva tri ove vrste podataka može biti pohranjene na računu za pohranu ili strujanjem koncentrator događaj, oba Slijede općenite namjene Azure resursi. Budući da Slijede općenite namjene resursa, stvaranje, brisanje i pristupanje njima je povlaštene operacije obično rezervirana za administratora. Predlažemo da biste zloupotreba koristite sljedeće postupke za vezanih uz nadzor resursa:

- Upotrijebite jednu, namjenski prostor za pohranu za praćenje podataka. Ako vam je potrebna razdvojiti nadzor podataka u većem broju računa za pohranu, nikad zajedničko korištenje korištenje računa za pohranu između nadzor i koje nisu nadzor podatke, kao to može slučajno dodjelu one kojima je potrebna samo pristup nadzor podataka (npr. treće strane SIEM) koji nisu nadzor pristupa podacima.
- Pomoću jednog, namjenski Bus servisa ili događaj koncentrator prostor naziva preko svih dijagnostičkih postavki razloga isti kao gore.
- Ograničiti pristup računi vezanih uz nadzor prostora za pohranu ili događaj koncentratora držanjem ih u grupi zasebnom resursa i [koristiti doseg](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) na vašim nadzora ulogama ograničiti pristup samo toj grupi resursa.
- Nikad ne dodijelite dozvolu ListKeys za račune za pohranu ili koncentratora događaja u pretplatu dosegu kada korisnik samo treba pristup nadzor podataka. Umjesto toga dodijelite tih dozvola korisniku na resurs ili grupu resursa (Ako imate namjenski grupa nadzor resursa) opseg.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Ograničavanje pristupa s računima vezanih uz nadzor prostora za pohranu
Kada korisnik ili aplikacija treba pristup podataka na račun za pohranu za nadzor, trebali biste [Generiranje programa SAS računa](https://msdn.microsoft.com/library/azure/mt584140.aspx) na račun za pohranu koji sadrži podatke o praćenju imaju pristup samo za čitanje razini usluge spremište blobova platforme. U ljusci PowerShell, to može izgledati kao što su:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Zatim može dati token entitet da treba čitati iz tog prostora za pohranu račun, a možete popisa i čitati iz svih blob-ova u taj račun za pohranu.

Osim toga, ako je potrebno kontrolirati tu dozvolu s RBAC možete dodijeliti taj entitet Microsoft.Storage/storageAccounts/listkeys/action dozvolu za taj račun za određeni prostor za pohranu. To je potrebno za korisnike koji žele moći postavite dijagnostičkih imenik ili kada se prijavite profila za arhiviranje s računom za pohranu. Na primjer, možete stvoriti sljedeću prilagođene RBAC ulogu korisnika ili aplikacije koje je potrebno samo za čitanje s jednog računa za pohranu za:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [AZURE.WARNING] Dozvole ListKeys korisniku na popisu račun tipki primarnih i sekundarnih prostora za pohranu. Ove tipke dozvolite korisniku sve potpisane dozvole (čitanje, pisanje, stvaranje blob-ova, izbrišite blob-ova, itd.) preko svih prijavljeni services (blob, reda čekanja, tablice, datoteka) u taj račun za pohranu. Preporučujemo korištenje programa SAS računa na prethodno opisan kada je to moguće.

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Ograničavanje pristupa događaja vezanih uz nadzor koncentratora
Slične uzorak vrijedi s koncentratorima događaj, no najprije morate stvoriti pravilo za provjeru autentičnosti namjenski Preslušavanja. Ako želite dopustiti pristup aplikaciju koja je samo treba preslušavanje koncentratora događaja vezanih uz nadzor, učinite sljedeće:

1. Stvaranje pravila zajednički pristup na hub(s) događaja koje su stvorene za strujanje nadzora podataka pomoću samo Preslušavanja zahtjevima. To možete učiniti na portalu. Na primjer, možda pozovite ga "monitoringReadOnly." Ako je to moguće, želite dati te tipke izravno korisnik i prijeđite na sljedeći korak.
2. Ako korisnik mora moći preuzeti ključa ad-hoc, dozvolite korisniku ListKeys akcija za događaj koncentratora. To je potrebno za korisnike koji žele moći postaviti dijagnostičkih postavka ili prijavu profila strujanje s koncentratorima događaj. Na primjer, možete stvoriti pravilo RBAC:

   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```


## <a name="next-steps"></a>Daljnji koraci
- [Saznajte više o RBAC i dozvola u upravitelju resursa](../active-directory/role-based-access-control-what-is.md)
- [Pročitajte pregled uključeno praćenje Azure](monitoring-overview.md)
