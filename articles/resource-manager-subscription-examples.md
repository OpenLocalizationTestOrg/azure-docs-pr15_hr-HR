<properties
   pageTitle="Scenariji i primjeri za pretplatu upravljanja | Microsoft Azure"
   description="Nalaze Primjeri kako implementirati upravljanja Azure pretplatu za uobičajeni scenariji."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Primjeri implementacijom scaffold Azure enterprise

Ova tema sadrži Primjeri kako na razini korporacije mogli implementirati preporuke za [Azure enterprise scaffold](resource-manager-subscription-governance.md). Da biste ilustrirali najbolje prakse za uobičajeni scenariji koristi fictional tvrtke pod nazivom Contoso. 

## <a name="background"></a>Pozadine

Contoso je diljem svijeta tvrtka koja omogućuje opskrbu lanac rješenja za kupce u sve iz modela "Kao na servis softvera" zapakirani model implementiran na lokalni.  Softver njihova razvoja diljem svijeta s centre za razvoj značajan Indija, Sjedinjenim Američkim Državama i Kanadi. 

Dio Neovisni tvrtke je podijeljen u nekoliko neovisno poslovne jedinice koje želite upravljati proizvoda na značajnu tvrtke. Svaku poslovnu jedinicu ima vlastitu inženjerima, upravitelji proizvoda i projektantima. 

Poslovna jedinica Enterprise Services za tehnologije (ETS) nudi središnje IT mogućnost i upravlja centre za nekoliko podataka koju hostira poslovnih jedinica svoje aplikacije. Uz upravljanje centre za podatke, ETS tvrtki ili ustanovi omogućuje i upravlja središnje suradnju (kao što su e-pošte i web-mjesta) i mreže/telefonske usluge. Oni i upravljati radnih opterećenja kupca dostupnog za manje tvrtke jedinice koje nemaju radu osoblje. 

Sljedeće osobe koriste se u ovoj temi:

- Zaposlenik je administrator Azure grafičke oznake.
- Alice je Contoso-Director od razvoj opskrbu lanac poslovne jedinice. 

Contoso mora sastavljanje redak tvrtke aplikacije i u okrenutom klijenta. Odlučili sadrži pokretanje aplikacija na Azure. Zaposlenik čita tema [upravljanja prescriptive pretplate](resource-manager-subscription-governance.md) , a sada je spremna za implementaciju preporuke. 

## <a name="scenario-1-line-of-business-application"></a>Scenarij 1: redak poslovnom aplikacijom

Contoso je stvara izvorni kod upravljanja sustavom (BitBucket) tako da koristi razvojnim inženjerima širom svijeta.  Aplikacija za hostiranje koristi infrastrukture kao Service (IaaS), a sastoji se od web-poslužiteljima i poslužitelj baze podataka. Razvojni inženjeri pristupiti poslužitelji u svoje razvojnih okruženja, no ne trebaju pristup poslužiteljima u Azure. Grafičke oznake Contoso želje da bi vlasnik aplikacije i tima za upravljanje aplikacije. Aplikacija je dostupna samo tijekom na mreži tvrtke Contoso. Zaposlenik nije potrebno postaviti pretplatu za ovu aplikaciju. Pretplata će i u budućnosti hostira drugi softver za razvojne inženjere odnose.  

### <a name="naming-standards--resource-groups"></a>Imenovanje Standardi i grupa resursa

Zaposlenik stvara pretplatu za podršku razvojnih alata koji su uobičajeni kroz sve poslovne jedinice. On mora stvoriti smisleni nazivi za pretplatu i resursa grupe (za aplikaciju i mreža). On stvara sljedeće grupe pretplatu i resursa:

| Stavke | Ime | Opis |
| ---- | ---- | ----------- |
| Pretplate | Proizvodne tvrtke Contoso ETS DeveloperTools | Podržava uobičajenih alata za razvojne inženjere |
| Grupa resursa | rgBitBucket | Sadrži aplikacije web-poslužitelj i poslužitelj baze podataka |
| Grupa resursa | rgCoreNetworks | Sadrži virtualne mreže i veze za pristupnik za web-mjesto |


### <a name="role-based-access-control"></a>Kontrola pristupa na temelju uloga

Kada stvorite svoju pretplatu, Zaposlenik želi da biste bili sigurni da odgovarajuće timovima i vlasnike aplikacija može pristupiti svojim resursi. Zaposlenik prepoznaje ima li svaki tim zahtjevima za različite. On koristi grupe koje sinkronizirane Contoso-lokalnim servisom Active Directory (AD) Azure Active Directory i omogućuje desnom razinu pristupa za. 

Zaposlenik dodjeljuje sljedećim ulogama za pretplatu: 

| Uloga | Dodijeljene | Opis |
| ---- | ----------- | ----------- |
| [Vlasnik](./active-directory/role-based-access-built-in-roles.md#owner) | Upravlja ID tvrtke Contoso-AD | Ovaj ID je kontrolirati pomoću samo u programu access vrijeme ČAS pomoću alata za upravljanje identitetom tvrtke Contoso- i osigurava pristup vlasnik pretplate u potpunosti nadzirati. |
| [Upravitelj sigurnosti](./active-directory/role-based-access-built-in-roles.md#security-manager) | Sigurnost i rizika upravljanje odjelu | Ta uloga korisnicima omogućuje pregled Azure centar za sigurnost i status resursa. |
| [Mreža suradnika](./active-directory/role-based-access-built-in-roles.md##network-contributor) | Tima za mrežu | Ta uloga omogućuje tima za mrežu tvrtke Contoso, upravljanje web-mjesta za web-mjesta VPN-a i virtualne mreže. |
| *Prilagođene uloge* | Vlasnik aplikacije | Zaposlenik stvara uloga koja daje mogućnost da biste izmijenili resursa u grupu resursa. Dodatne informacije potražite u članku [Prilagođeno uloga u Azure RBAC](./active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Pravila

Zaposlenik sadrži sljedeće preduvjete za upravljanje resursa u pretplatu:

- Budući da Alati za razvoj podržava razvojni inženjeri širom svijeta, ne on želite onemogućite korisnicima stvaranje resursa u bilo kojem regiji. Međutim, on mora znate gdje se nalaze stvorene resursi. 
- On je zamaraju troškove. Stoga želi onemogućuju stvaranje nepotrebno skupi virtualnim strojevima vlasnici aplikacije.  
- Budući da tu aplikaciju služi razvojnim inženjerima u brojne poslovne jedinice, želi označavanje svakog resursa s vlasnik tvrtke jedinica i aplikacije. Pomoću ove oznake grafičke oznake možete fakturu odgovarajuće timovima.

On stvara sljedeće [resursima pravila](resource-manager-policy.md): 

| Polje | Efekt | Opis |
| ----- | ------ | ----------- |
| mjesto | nadzora | Stvaranje resursa u bilo kojem regija nadzora |
| Vrsta | Nemoj dopustiti | Nemoj dopustiti stvaranje glavne niz virtualnih računala |
| oznaka | Nemoj dopustiti | Obavezan aplikacije vlasnik oznaka |
| oznaka | Nemoj dopustiti | Obavezan trošak centra oznaka |
| oznaka | Dodavanje | Dodavanje naziv oznake **poslovnu jedinicu** i vrijednost oznake **ETS** svi resursi |


### <a name="resource-tags"></a>Oznaka resursa

Možete koristiti zaposlenik mora imati određene podatke na računu za prepoznavanje trošak centar za implementaciju BitBucket. Uz to, Zaposlenik želi znati resursi vlasništvu grafičke oznake. 

On dodaje sljedeće [oznake](resource-group-using-tags.md) grupe resursa i resursa. 
 
| Naziv oznake | Vrijednost oznake |
| -------- | --------- |
| ApplicationOwner | Ime osobe koja upravlja ove aplikacije. |
| CostCenter | Središte troškova grupu koju je plaćanja Azure potrošnje. |
| Poslovnu jedinicu | **Grafičke oznake** (poslovnu jedinicu povezanu s pretplatom) |

### <a name="core-network"></a>Temeljni mreže

Contoso ETS informacije o sigurnosti i rizik upravljanje tim pregledava Daveovom predloženi plan da biste premjestili aplikacije Azure. Žele da biste bili sigurni da aplikacija ne izložen putem Interneta.  Zaposlenik ima za razvojne inženjere aplikacije koje se u budućnosti će se premjestiti u Azure. Ove aplikacije potreban je javnih sučelja.  Da biste zadovoljava te preduvjete, on nudi interne i vanjske virtualne mreže i sigurnosne grupe mreže ograničavanja pristupa.

On stvara u sljedećim resursima:

| Vrsta resursa | Ime | Opis |
| ------------- | ---- | ----------- |
| Virtualne mreže | vnInternal | Koristiti s aplikacijom BitBucket, a zatim je povezan putem ExpressRoute Contoso-korporacijskom mrežom.  Podmreže (sbBitBucket) nudi aplikacijom određene razmak IP adresa. |
| Virtualne mreže | vnExternal | Dostupno za buduće programe koji zahtijevaju krajnje točke dostupnom javnosti. |
| Mrežni sigurnosne grupe | nsgBitBucket | Provjerava je li površina napada ovog posla minimiziran dopuštanjem veze samo na priključak 443 podmreži gdje se nalaze aplikacije (sbBitBucket). |

### <a name="resource-locks"></a>Ograničenja resursa 

Zaposlenik prepoznaje veza na mreži tvrtke Contoso-s internoj mreži virtualne mora biti zaštićene iz bilo koje wayward skripte ili nenamjerno brisanje. 

On stvara sljedeće [lock resursa](resource-group-lock-resources.md):

| Vrsta zaključavanja | Resurs | Opis |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnInternal | Onemogućuje brisanje virtualne mreže ili podmreže korisnike, ali ne sprječava dodavanja novog podmreže. |

### <a name="azure-automation"></a>Automatizacija Azure 

Zaposlenik ne sadrži ništa da biste automatizirali za ovu aplikaciju. Iako je stvorili račun Azure Automatizacija, on neće prethodno pomoću njega. 

### <a name="azure-security-center"></a>Centar za sigurnost Azure 

Upravljanje servisom za Contoso IT mora brzo prepoznate i rukovati prijetnji. Žele razumijevanje možda postoji problema.  

Da biste ispunili sljedeće preduvjete, Zaposlenik omogućuje [Azure centar za sigurnost](./security-center/security-center-intro.md), a omogućuje pristup ulogu upravitelja sigurnosti. 

## <a name="scenario-2-customer-facing-app"></a>Scenarij 2: aplikacije klijenta dostupnog

Vodstvo tvrtke poslovne jedinice lanac opskrbe otkrio razne mogućnosti za poslovanje da biste dodatno angažirali s klijentima Contoso-pomoću kartice vjernost. Tim Alice, morate stvoriti ovu aplikaciju i odluči da Azure povećava njihovu mogućnost da bi odgovarao poslovne potrebe. Alice funkcionira s zaposlenik iz grafičke oznake da biste konfigurirali dva pretplate za razvoj i operacijski ove aplikacije.

### <a name="azure-subscriptions"></a>Azure pretplate 

Zaposlenik zapisnike u Enterprise portal Azure, a ne vidi da odjel lanac opskrbe već postoji.  Međutim, kao što je projekt prvi razvoj projekta za tim lanac opskrbe u Azure, Zaposlenik prepoznaje potrebe za novi račun za tim za razvoj Alice korisnika.  Stvara "Istraživanje i razvoj" račun za svoj tim i pristup dodjeljuje Alice. Alice zapisnike u putem portala za račun i stvara dva pretplate: jednu za razvoj poslužitelji i jednu za držite proizvodne poslužitelje.  Ana slijedi prethodno utvrđene imenovanja standarde prilikom stvaranja sljedeće pretplate: 

| Korištenje pretplate | Ime |
| ---------------- | ---- |
| Razvoj | Razvoj LoyaltyCard SupplyChain ResearchDevelopment |
| Proizvodne | SupplyChain operacije LoyaltyCard proizvodnje |

### <a name="policies"></a>Pravila

Zaposlenik Alice Razgovarajte aplikacije i prepoznavanje ovu aplikaciju samo služi kupce u Sjevernoj Americi regiji.  Alice i svog tima namjeravate koristiti aplikaciju servisa okruženju i Azure SQL Azure, da biste stvorili aplikacije. Koje su im potrebne za stvaranje virtualnim strojevima tijekom razvoj.  Alice želi da biste bili sigurni da imate njezinu razvojnim inženjerima resursa koje su im potrebne za istraživanje i pregledajte probleme bez izvlačenja u grafičke oznake. 

Za **razvoj pretplate**stvaraju sljedeća pravila:

| Polje | Efekt | Opis |
| ----- | ------ | ----------- |
| mjesto | nadzora | Nadzor stvaranja resursa u bilo kojem regija. |

Ne ograničava vrstu sku korisnik može stvoriti u razvoju, a ne trebaju oznake za sve grupe resursa ili resursi.

Za **pretplate radnog**stvaraju sljedeća pravila:

| Polje | Efekt | Opis |
| ----- | ------ | ----------- |
| mjesto | Nemoj dopustiti | Nemoj dopustiti stvaranje nikakve resurse izvan SAD-a centre za podatke. |
| oznaka | Nemoj dopustiti | Obavezan aplikacije vlasnik oznaka |
| oznaka | Nemoj dopustiti | Potreban je odjel oznaka. | 
| oznaka | Dodavanje | Dodavanje oznaka za svaku grupu resursa koji označava radnog okruženja. |

Ne ograničavaju vrstu sku korisnik može stvoriti u radnog.

### <a name="resource-tags"></a>Oznaka resursa 

Možete koristiti zaposlenik mora imati konkretne informacije da biste odredili točan poslovne grupe za naplatu i vlasništvo. On definira oznake resursa za grupe resursa i resursa. 
 
Naziv oznake | Vrijednost oznake |
| -------- | --------- |
| ApplicationOwner | Ime osobe koja upravlja ove aplikacije. |
| Odjelu | Središte troškova grupu koju je plaćanja Azure potrošnje. |
| EnvironmentType | **Proizvodne** (Čak i ako je pretplata obuhvaća **proizvodnje** u nazivu, uključujući ovoj oznaci omogućuje jednostavno prepoznavanje prilikom pregledavanja resursa na portalu ili na naplate.) |

### <a name="core-networks"></a>Temeljni mreža

Contoso ETS informacije o sigurnosti i rizik upravljanje tim pregledava Daveovom predloženi plan da biste premjestili aplikacije Azure. Žele aplikacije vjernost kartica je li pravilno Izolirani i zaštitu u mreži DMZ.  Da biste ispunili taj zahtjev, Zaposlenik i Alice stvorite vanjski virtualne mreže i sigurnosne grupe mreže izdvojiti aplikacije vjernost kartice na mrežu tvrtke Contoso.  

Za **razvoj pretplate**stvaraju:

| Vrsta resursa | Ime | Opis |
| ------------- | ---- | ----------- |
| Virtualne mreže | vnInternal | Služi razvojno okruženje Contoso vjernost kartice, a zatim je povezan putem ExpressRoute Contoso-korporacijskom mrežom. |

Za **pretplate radnog**stvaraju:

| Vrsta resursa | Ime | Opis |
| ------------- | ---- | ----------- |
| Virtualne mreže | vnExternal | Hostira aplikacije vjernost kartica, a ne priključen izravno na Contoso-ExpressRoute. Kod je pomiču putem sustava njihove izvornog koda izravno u PaaS services. |
| Mrežni sigurnosne grupe | nsgBitBucket | Provjerava je li površina napada ovog posla minimiziran dopuštanjem samo u granica komunikacije na TCP 443.  Contoso je i istražuje koristite vatrozid Web aplikacije za dodatnu zaštitu. |  

### <a name="resource-locks"></a>Ograničenja resursa 

Zaposlenik Alice confer i odlučite da biste dodali resursa zaključavanja na neki od ključne resurse u okruženju da biste spriječili nenamjerno brisanje tijekom programa automatske errant kod. 

Stvaraju zaključavanje sljedeće:

| Vrsta zaključavanja | Resurs | Opis |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnExternal | Da biste korisnicima onemogućili brisanje virtualne mreže ili podmreže. Zaključavanje ne sprječava dodavanja novog podmreže. |

### <a name="azure-automation"></a>Automatizacija Azure 

Alice i njezinim razvojnom timu imate proširenom runbooks da biste upravljali okruženje za ovu aplikaciju. Na runbooks omogućuju za zbrajanje/brisanje čvorove za aplikaciju i ostali zadaci DevOps. 

Da biste koristili te runbooks, omogućuju [automatizaciju](./automation/automation-intro.md).

### <a name="azure-security-center"></a>Centar za sigurnost Azure 

Upravljanje servisom za Contoso IT mora brzo prepoznate i rukovati prijetnji. Žele razumijevanje možda postoji problema.  

Da biste ispunili sljedeće preduvjete, Zaposlenik omogućuje Azure centar za sigurnost. On osigurava nadzire resurse Azure centar za sigurnost, a omogućuje pristup timovima DevOps i sigurnost. 

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o stvaranju predložaka Voditelj resursa, potražite u članku [preporučeni Načini stvaranja predloška Azure Voditelj resursa](resource-manager-template-best-practices.md).

*U ovoj se temi pridonio [Karl Kuhnhausen](https://github.com/karlkuhnhausen) .*
