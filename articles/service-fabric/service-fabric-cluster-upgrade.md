<properties
   pageTitle="Nadogradnja je tkanina servisa Azure klaster | Microsoft Azure"
   description="Nadogradite kod usluge tkanina i/ili konfiguracija koja se pokreće servis tkanina klaster, uključujući postavljanje način ažuriranja klaster, Nadogradnja certifikata, dodavanje aplikacije priključke način zakrpa OS i tako dalje. Što mogu očekivati kada se izvode na nadogradnje?"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-an-azure-service-fabric-cluster"></a>Nadogradnja je tkanina servisa Azure klaster

> [AZURE.SELECTOR]
- [Azure klaster](service-fabric-cluster-upgrade.md)
- [Samostalne klaster](service-fabric-cluster-upgrade-windows-server.md)

Dizajniranje za upgradability modernom sustavu je ključ postizanja Dugoročne uspjeh proizvoda. Programa tkanina servisa Azure klaster je resurs koji ste vlasnik, ali djelomično upravlja Microsoft. U ovom se članku opisuje što se upravlja automatski i što možete konfigurirati sami.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Kontroliranje tkanina verziju koja se izvršava na svoj klaster

Možete postaviti svoj klaster primati nadogradnje automatsko tkanina kada Microsoft izda nova verzija ili odaberite da biste odabrali tkanina podržanu verziju želite svoj klaster na.

Postupak postavljanja klaster konfiguracija "upgradeMode" na portalu ili pomoću resursima prilikom stvaranja ili noviji na uživo klaster 

>[AZURE.NOTE] Provjerite jeste li zadržati svoj klaster uvijek koristite podržani tkanina verziju. Kao i kada ne možemo objava izdanje novu verziju servisa tkanina prethodne verzije označen za kraj podršku nakon najmanje 60 dana od datuma. na nova izdanja su najavljena [na blogu tima tkanina servisa](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Novo izdanje dostupna pa odaberite. 

14 dana prije isteka izdanje radi svoj klaster, stanja događaja generira koja stavlja svoj klaster u stanja za upozorenja. Klaster ostaje u stanju upozorenje dok ih ne nadogradite tkanina podržanu verziju.


### <a name="setting-the-upgrade-mode-via-portal"></a>Postavljanje načina nadogradnje putem portala 

Kada stvarate klaster, klaster možete postaviti da automatski ili ručno.

![Create_Manualmode][Create_Manualmode]

Klaster možete postaviti da automatski ili ručno na uživo klaster, pomoću sučelje za upravljanje. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Nadogradnja je nova verzija na klasteru koja je postavljena na ručni način putem portal.
 
Da biste nadogradili novu verziju, sve što trebate napraviti je na padajućem izborniku odaberite dostupne verzije i spremite. Nadogradnja tkanina dobiva kicked automatski. Pravila stanja klaster (kombinaciju čvor stanja i stanju sve programe koji se izvodi u klasteru) su pridržavao tijekom nadogradnje.

Ako pravila stanja klaster nije zadovoljen, Nadogradnja je vraćen. Pomaknite se prema dolje ovaj dokument potražite dodatne informacije o postavljanju tih pravila za prilagođene stanja. 

Nakon što ste otkloniti probleme koja je rezultirala vraćanje, morate ponovno pokrenuti nadogradnju slijedeći iste korake kao prije.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>Postavljanje načina nadogradnje putem predloška Voditelj resursa 

Dodavanje konfiguracija "upgradeMode" definiciji resursa Microsoft.ServiceFabric/clusters i postavite "clusterCodeVersion" na jednu od verzije podržanih tkanina kao što je prikazano u nastavku i uvođenje predloška. Valjane vrijednosti za "upgradeMode" su "Ručni" ili "Automatski"
 
![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Nadogradnja je nova verzija na klasteru koja je postavljena na ručni način putem predloška Voditelj resursa.
 
Kada klaster je u načinu rada za ručno, da biste nadogradili na novu verziju "clusterCodeVersion" promijenite u podržanu verziju i njegove implementacije kod. Uvođenje predloška kicks nadogradnje tkanina dobiva kicked automatski. Pravila stanja klaster (kombinaciju čvor stanja i stanju sve programe koji se izvodi u klasteru) su pridržavao tijekom nadogradnje.

Ako pravila stanja klaster nije zadovoljen, Nadogradnja je vraćen. Pomaknite se prema dolje ovaj dokument potražite dodatne informacije o postavljanju tih pravila za prilagođene stanja. 

Nakon što ste otkloniti probleme koja je rezultirala vraćanje, morate ponovno pokrenuti nadogradnju slijedeći iste korake kao prije.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Dobili popis svih dostupnih verzije za sve okruženja za dani pretplatu

Pokrenite sljedeću naredbu, a na Izlaz trebali biste dobiti slična ovoj.

"supportExpiryUtc" pokazuje na kada navedeni izdanje istječe ili je istekla. Najnovije izdanje videoprikazu nisu ispravni datumi - ima vrijednost "9999 – 12-31T23:59:59.9999999", koja znači jednostavno da datum isteka nije još postavljen.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/clusterVersions?api-version= 2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Tkanina nadogradnje ponašanje prilikom klaster nadograditi način je automatski

Microsoft zadržava kod tkanina i konfiguracije koji se izvodi u Azure klaster. Ne možemo izvršiti automatske nadziranim nadogradnje softvera sustava potrebi. Ove nadogradnje može biti kod, konfiguriranje ili oboje. Da biste provjerili je li aplikacija suffers bez utjecaja ili minimalnim utjecajem zbog te nadogradnje, ne možemo na nadogradnji u izvesti sljedeće faze:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Faza 1: Nadogradnja izvršiti pomoću sva pravila stanja klaster

Tijekom faze, u nadogradnji nastaviti jednu domenu nadogradnje odjednom, a aplikacija sa u klasteru nastaviti da se pokreće bez sve nedostupnost. Pravila stanja klaster (kombinaciju čvor stanja i stanju sve programe koji se izvodi u klasteru) su pridržavao tijekom nadogradnje.

Ako pravila stanja klaster nije zadovoljen, Nadogradnja je vraćen. Vlasnik pretplate je poslati poruku e-pošte. E-pošte sadrži sljedeće informacije:

- Obavijesti ne možemo morali vratiti klaster nadogradnju.
- Predloženi remedial Akcije, ako postoje.
- Broj dana (n) sve dok ne možemo izvršiti faza 2.

Ne možemo pokušati izvršiti iste nadogradnje nekoliko puta za slučaj da neki nadogradnje nije uspio razloga infrastrukture. Nakon n dana od datuma slanja e-pošte, ne možemo nastaviti s faza 2.

Ako su ispunjeni pravila stanja klaster, Nadogradnja je smatra uspjelo i označen kao dovršen. To se može dogoditi tijekom početne nadogradnje ili bilo koji od nadogradnje reprize u ovoj fazi. Postoji bez potvrde e-pošte uspješno Pokreni. To je da bi se izbjeglo slanje ste previše poruke e-pošte – primati poruke e-pošte će se prikazivati kao iznimku normalno. Očekivanog većinu nadogradnje klaster da bez utjecaja dostupnosti aplikacija.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Faza 2: Nadogradnja izvršiti pomoću samo zadane pravilnike o stanju sustava

Pravila stanja u ovoj fazi postavljene na način koji broj aplikacije koje su dobar na početku nadogradnje ostaje trajanja postupka nadogradnje. Kao 1 faza nadogradnje faza 2 nastaviti jednu domenu nadogradnje odjednom, a aplikacija sa u klasteru nastaviti da se pokreće bez sve nedostupnost. Pravila stanja klaster (kombinaciju čvor stanja i stanju sve programe koji se izvodi u klasteru) su pridržavao trajanja nadogradnje.

Ako pravila stanja klaster na snazi su ispunjen, Nadogradnja je vraćen. Vlasnik pretplate je poslati poruku e-pošte. E-pošte sadrži sljedeće informacije:

- Obavijesti ne možemo morali vratiti klaster nadogradnju.
- Predloženi remedial Akcije, ako postoje.
- Broj dana (n) sve dok ne možemo izvršiti faza 3.

Ne možemo pokušati izvršiti iste nadogradnje nekoliko puta za slučaj da neki nadogradnje nije uspio razloga infrastrukture. Podsjetnik e-pošte šalje se na sljedeća dva dana prije spremni n dana. Nakon n dana od datuma slanja e-pošte, ne možemo nastaviti s faza 3. Poruke e-pošte mi šaljemo u fazi 2, potrebno je poduzeti utjecati i remedial Akcije, potrebno je poduzeti.

Ako su ispunjeni pravila stanja klaster, Nadogradnja je smatra uspjelo i označen kao dovršen. To se može dogoditi tijekom početne nadogradnje ili bilo koji od nadogradnje reprize u ovoj fazi. Postoji bez potvrde e-pošte uspješno Pokreni.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Faza 3: Nadogradnja izvršiti pomoću pravila izrazito stanja

Ta pravila stanja u ovoj fazi su usmjerena prema obavljanjem nadogradnje umjesto stanja aplikacije. Vrlo malo klaster nadogradnje završe u ovoj fazi. Ako svoj klaster dobiti u ovoj fazi, postoji vjerojatnost da program postane dobro i/ili izgubiti dostupnost.

Slično drugih dviju faza, faze 3 nadogradnje nastaviti jednu domenu nadogradnje odjednom.

Ako pravila stanja klaster nije zadovoljen, Nadogradnja je vraćen. Ne možemo pokušati izvršiti iste nadogradnje nekoliko puta za slučaj da neki nadogradnje nije uspio razloga infrastrukture. Nakon toga klaster je prikvačena, tako da ga više nećete primati podršku i/ili nadogradnje.

Vlasnik pretplate te remedial akcije se šalje poruku e-pošte pomoću tih informacija. Ne možemo očekivati sve klastere da biste u stanju koje nije uspjela faza 3.

Ako su ispunjeni pravila stanja klaster, Nadogradnja je smatra uspjelo i označen kao dovršen. To se može dogoditi tijekom početne nadogradnje ili bilo koji od nadogradnje reprize u ovoj fazi. Postoji bez potvrde e-pošte uspješno Pokreni.

## <a name="cluster-configurations-that-you-control"></a>Konfiguracija klaster koji omogućuje upravljanje

Uz mogućnost da biste postavili klaster nadogradili način, Evo konfiguracije koje možete promijeniti na uživo klaster.

### <a name="certificates"></a>Certifikati

Možete dodati nove ili jednostavno izbrisati certifikati za klijent putem portala i klaster. Pogledajte [ovaj dokument detaljne upute](service-fabric-cluster-security-update-certs-azure.md)

![Snimka zaslona koja prikazuje thumbprints certifikata na portalu za Azure.][CertificateUpgrade]


### <a name="application-ports"></a>Priključci aplikacije

Priključci aplikaciju možete promijeniti promjenom svojstva opterećenja resursa koje su vezane uz vrsta čvora. Možete koristiti portal ili izravno koristiti PowerShell Voditelj resursa.

Da biste otvorili novi priključak na sve VMs vrsta čvora, učinite sljedeće:

1. Dodajte novi probni odgovarajuće opterećenja.

    Ako svoj klaster implementiran putem portala, su balancers učitavanje pod nazivom "LB-naziv grupe resursa-NodeTypename", jedan za svaku vrstu čvor. Budući da su nazivi raspoređivača opterećenja jedinstvena samo u grupu resursa, preporučuje se ako ste ih potražite u odjeljku određene grupe resursa.

    ![Snimka zaslona koja prikazuje dodavanje na probni opterećenja na portalu.][AddingProbes]

2. Dodavanje novog pravila raspoređivača opterećenja.

    Dodavanje novog pravila isti opterećenja pomoću probni koju ste stvorili u prethodnom koraku.

    ![Dodavanje novog pravila opterećenja na portalu.][AddingLBRules]


### <a name="placement-properties"></a>Položaj svojstva

Za svaku vrstu čvor možete dodati prilagođene položaj svojstva koje želite koristiti u aplikacije. NodeType je svojstvo default koje možete koristiti bez dodavanja izričito.

>[AZURE.NOTE] Informacije o korištenja položaj ograničenja, čvor svojstva i kako ih definirati potražite u odjeljku "Položaj ograničenja i čvor svojstva" u dokumentu Voditelj resursa za servis tkanina klaster na [s opisom klaster](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="capacity-metrics"></a>Kapaciteta mjerenja

Za svaku vrstu čvor možete dodati prilagođene kapaciteta metrike koji želite koristiti u aplikacije da biste učitali izvješće. Informacije o korištenja kapaciteta metriku za izvješće učitavanje, pogledajte Voditelj resursa za servis tkanina klaster dokumenata [s opisom klaster](service-fabric-cluster-resource-manager-cluster-description.md) i [metriku i Učitaj](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Postavke nadogradnje tkanina - stanja pravila

Možete odrediti prilagođene stanja pravila za nadogradnju tkanina. Ako ste postavili svoj klaster automatsko tkanina nadogradnje, ta pravila se primjenjuje faza 1 automatsko tkanina nadogradnji.
Ako postavite svoj klaster za ručno tkanina nadogradnje, ta pravila se primjenjuje svaki put kad odaberete novu verziju pokretanje sustav izbaciti tkanina Nadogradi u svoj klaster. Ako ne nadjačati pravilnike, koriste se zadane vrijednosti.

Možete navesti pravila za prilagođene stanja ili pregledajte Trenutačne postavke u odjeljku plohu "Nadogradnja tkanina" tako da odaberete dodatne postavke nadogradnje. Pročitajte članak na sljedećoj slici kako. 

![Upravljanje pravilnicima za prilagođene stanja][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Prilagodba postavki tkanina za svoj klaster

Pogledajte [Postavke servisa tkanina za tkanina klaster](service-fabric-cluster-fabric-settings.md) što i kako ih možete prilagoditi.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>OS zakrpa na VMs koji čine klaster

Tu mogućnost kao automatiziranog značajka se planira u budućnosti. Ali trenutno su odgovorni zakrpu vaše VMs. To morate učiniti ovo jedan VM odjednom, tako da ne treba dolje više od jedan po jedan.

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>OS nadogradnje na VMs koji čine klaster

Ako morate nadograditi OS sliku na virtualnim računalima sustava klaster, to morate učiniti ga VM jedan po jedan. Vi ste odgovorni za nadogradnju – trenutno nema Automatizacija za to.

## <a name="next-steps"></a>Daljnji koraci
- Saznajte kako prilagoditi neke [Postavke servisa tkanina za tkanina klaster](service-fabric-cluster-fabric-settings.md)
- Saznajte kako [i smanjivati skaliranje svoj klaster](service-fabric-cluster-scale-up-down.md)
- Dodatne informacije o [nadogradnji aplikacije](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG