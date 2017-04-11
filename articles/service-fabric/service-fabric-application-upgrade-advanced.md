<properties
   pageTitle="Nadogradnja aplikacije: napredne teme | Microsoft Azure"
   description="U ovom se članku opisuje neke dodatne teme vezani uz uz nadogradnju servisa tkanina aplikacije."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="service-fabric-application-upgrade-advanced-topics"></a>Nadogradnja aplikacije servisa tkanina: dodatne teme

## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Dodavanje ili uklanjanje usluge tijekom nadogradnje aplikacije

Ako je novog servisa dodali u aplikaciju koja se već implementiran i objaviti kao nadogradnju, nove usluge dodaje se distribuiranih aplikacije.  Takve nadogradnje ne utječe na servise koji su već dio aplikacije. Međutim, mora se instanca servisa koji je dodan pokrenuti servisa za nove aktivnima (pomoću na `New-ServiceFabricService` cmdlet).

Usluge moguće je ukloniti iz aplikacije kao dio nadogradnju. Međutim, morate se sve trenutne instance servisa to-be-izbrisane zaustaviti prije nego što nastavite s nadogradnjom (pomoću na `Remove-ServiceFabricService` cmdlet). 

## <a name="manual-upgrade-mode"></a>Ručno načina nadogradnje

> [AZURE.NOTE]  Samo za nije uspjelo ili obustavljenom nadogradnje razmatranje automatiziranog ručnom načinu rada. Nadzirane način je preporučena nadogradnje za servis tkanina aplikacije.

Azure tkanina servisa sadrži više načina nadogradnje za podršku razvoj i radnih klastere. Mogućnosti implementacije odabrali mogu se razlikovati u različitim okruženjima.

Nadzirane rolling aplikacije Nadogradnja je najčešći nadogradnje za korištenje u radni. Prilikom nadogradnje pravila nije naveden, servis tkanina osigurava aplikacije dobar prije nadogradnje prihod.

 Administrator aplikacije pomoću ručno rolling aplikacije nadogradnje načina rada imaju potpunu kontrolu nad tijek nadogradnje putem različitih nadogradnje domena. Ovaj način je korisno kada pravila za procjenu prilagođene ili složenu stanja potreban je ili nije običan Nadogradnja se događa (na primjer, aplikacija je već gubitak podataka).

Naposljetku, automatizirani rolling aplikacije Nadogradnja je korisno za razvoj ili testiranja okruženja omogućuje brzo iteracije ciklusa tijekom razvoj servisa.

## <a name="change-to-manual-upgrade-mode"></a>Promjena načina ručno nadogradnje
**Ručno**– zaustavite nadogradnju aplikacije u trenutnom UD i Promjena načina nadogradnje na automatiziranog ručno. Administrator mora ručno poziva **MoveNextApplicationUpgradeDomainAsync** da biste nastavili s nadogradnjom ili pokrenuti vraćanje po započinjanja novog nadogradnju. Nakon nadogradnje unosi u ručni način, ona ostaje u načinu rada za ručno dok se ne pokreće novu nadogradnju. Naredba **GetApplicationUpgradeProgressAsync** vraća TKANINA\_APLIKACIJE\_NADOGRADITI\_STANJE\_ROLLING\_UNAPRIJED\_NERIJEŠENO.

## <a name="upgrade-with-a-diff-package"></a>Nadogradnja s razlika paketa

Servis tkanina aplikacije moguće je nadograditi po dodjele resursa s puno, samostalnih aplikacije paketa. Aplikacija je moguće nadograditi i pomoću razlika paket koji sadrži samo datoteke ažurirane aplikacije, ažurirane programski manifest i manifesta datoteke servisa.

Cjelovite aplikacije paket sadrži sve datoteke koje su potrebne za pokretanje i rad s aplikacijom servisa tkanina. Razlika paket sadrži samo datoteke koje se promijenilo između zadnje dodjele resursa i trenutnu nadogradnju, uz manifest punu aplikaciju i servis manifest datoteke. Bilo koji referencu u manifest aplikaciju ili servis manifest koje nije moguće pronaći u izgledu Sastavi tražili u spremištu slike.

Cjelovite aplikacije paketa su potrebni za prvi instalacije aplikacije na klaster. Ažuriranja koja slijedi može biti punu aplikaciju paket ili paket razlika.

Prilike kada paket razlika bio dobar izbor:

* Paket razlika je Preferirani kad imate veliki Aplikacijski paket koji referencira nekoliko datoteke manifesta servisa i/ili nekoliko kod paketa, config paketa ili paketa podataka.

* Paket razlika je Preferirani kada imate implementaciju sustava koje generira izgleda Sastavi izravno iz proces Sastavi aplikacije. U ovom slučaju, čak i ako nije promijenio kod, upravo ugrađeni sklopova se različite kontrolni zbroj. Korištenje paket punu aplikaciju je potrebna da biste ažurirali verziju na sve pakete kod. Korištenje paket razlika, samo unesete datoteke koje se promijenio i datotekama manifesta gdje je promijenjena verziju.

Kada aplikacija će se nadograditi pomoću Visual Studio, automatski se objavljuje razlika paketa. Da biste ručno stvorili paket razlika, manifest aplikacija i servisa manifesti morate ažurirati, ali samo promijenjene paketa želite uvrstiti u konačni Aplikacijski paket. 

Na primjer, Započnimo s sljedeće aplikacije (verzija brojevi za olakšani razumijevanje):

```text
app1        1.0.0
  service1  1.0.0
    code    1.0.0
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Sada ćemo pretpostavlja da ste željeli ažurirati samo kod paket sustava service1 pomoću paket razlika pomoću komponente PowerShell. Sada ažurirane aplikacije sadrži struktura mapa za sljedeće:

```text
app1        2.0.0      <-- new version
  service1  2.0.0      <-- new version
    code    2.0.0      <-- new version
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

U ovom slučaju, ažurirajte programski manifest 2.0.0 i manifest servisa service1 u skladu s vizualnim paketa ažuriranja kod. Mapa za Aplikacijski paket promijenile sljedeću strukturu:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Daljnji koraci

[Nadogradnja vašeg računala pomoću Visual Studio](service-fabric-application-upgrade-tutorial.md) vodit će vas kroz nadogradnju aplikacije pomoću Visual Studio.

[Nadogradnja aplikacije pomoću ljuske](service-fabric-application-upgrade-tutorial-powershell.md) vodit će vas kroz nadogradnju aplikacije pomoću komponente PowerShell.

Upravljanje kako nadograđuje aplikacije pomoću [Nadograditi parametara](service-fabric-application-upgrade-parameters.md).

Provjerite svoje nadogradnje aplikacije kompatibilne Naučite koristiti [Serijalizacije podataka](service-fabric-application-upgrade-data-serialization.md).

Riješite uobičajene probleme u aplikaciji nadogradnje tako da upućuju na korake [Za otklanjanje poteškoća nadogradnje aplikacije](service-fabric-application-upgrade-troubleshooting.md).
 
