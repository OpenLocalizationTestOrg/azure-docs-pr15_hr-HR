<properties
   pageTitle="Najbolje prakse za korporacije premještanje Azure | Microsoft Azure"
   description="U članku se opisuje scaffold korporacije možete ga koristiti da biste bili sigurni okruženju sigurne i moguće upravljati."
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

# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure enterprise scaffold - upravljanja prescriptive pretplate

Korporacije su sve usvojio javno oblaka za njegov agility i fleksibilnost. Oni su korištenja prednosti u oblak generiranje prihod ili optimiziranje resursi za tvrtke. Microsoft Azure sadrži više od servisa kao što su sastavni blokovi u adresi širok raspon radnih opterećenja i aplikacije je mogu sastaviti velike tvrtke. 

No zna gdje želite početi često je teško. Kada odlučujete o korištenju platforme Azure, na nekoliko pitanja najčešće biste:

- "Kako zadovoljavaju naš zakonskih samostalnosti podataka u određenim zemljama?"
- "Kako osigurati da netko nenamjerno mijenja ključnih sistemskih?"
- "Kako mogu znati što svaki resurs je za podršku tako da se mogu računa i računa točno ponovno?"

Potencijalni klijent prazan pretplate s bez tračnicama straža je daunting. U ovom prazan prostor može spriječiti pomicanje Azure.

Ovaj članak sadrži početnu točku za tehničke profesionalce adresa potreba radi postizanja i usklađene s potrebe za agility. On predstavlja pojam programa enterprise scaffold koji vodi tvrtke ili ustanove u implementacije i upravljanja njihove Azure pretplate. 

## <a name="need-for-governance"></a>Treba li vam radi postizanja

Prilikom pomicanja Azure, morate adresa tema upravljanja ranijeg da biste bili sigurni uspješno korištenje u oblak u tvrtki. Nažalost, vrijeme i bureaucracy stvaranja potpun upravljanja sustava znači da neke grupe tvrtke Idite izravno dobavljačima bez korištenja enterprise IT. Taj se način možete ostaviti tvrtki otvorenu za slabe točke ako resursa nije ispravno upravlja se. Značajke javnog oblaka - agility, fleksibilnost i potrošnje utemeljen određena cijena - su važne tvrtke grupe koje morate brzo zadovoljavaju zahtjeve klijenata (interne i vanjske). No, enterprise IT potrebno da biste bili sigurni da podatke i sustavi učinkovito zaštićen.

U realne vijek scaffolding se koristi za stvaranje temelj strukture. Na scaffold vodi Općenito strukture i njihovi sidro točke za trajno sustavi biti postavljena. Za enterprise scaffold je ista: skup fleksibilne kontrole i Azure mogućnosti koje daju strukturu okruženje i sidra za servise utemeljena na javno oblaka. Pruža na sastavljača računala (IT i grupama za tvrtke) foundation da biste stvorili i priložili nove servise.

Na scaffold temelji se na prakse smo ste prikupili iz mnogo obaveze s klijentima raznih veličina. Te klijenti kreću se od male tvrtke i ustanove razvoju rješenja u oblačić za korporacije Fortune 500 i nezavisni proizvođači koji su migracije i razvoj rješenja u oblaku. Enterprise scaffold je "purpose-built" da biste fleksibilnost za podršku tradicionalni IT radnih opterećenja i agilno radnih opterećenja; kao što su razvojni inženjeri stvaranje aplikacija softver-kao-na-service (SaaS) na temelju Azure mogućnosti.

Enterprise scaffold je namijenjen biti foundation svaku novu pretplatu unutar Azure. Omogućuje administratorima da biste bili sigurni radnih opterećenja zadovoljava preduvjete minimalne upravljanja tvrtke ili ustanove ne sprječava brzo ispunjavanje vlastite ciljeva web-mjesto tvrtke grupe i u okvir za razvojne inženjere.

> [AZURE.IMPORTANT] Upravljanje je ključnih za uspjeh Azure. U ovom se članku pronalaze tehničku implementaciju sustava scaffold enterprise, ali samo dodiruje šira postupak i odnose između komponente. Pravila upravljanja teče odozgo prema dolje, a određuju što tvrtke želi da biste postigli. Prirodno, stvaranje modela upravljanja za Azure obuhvaća predstavnici iz IT, ali više važnije mora imati istaknuti predstavljanje iz tvrtke grupe voditelje i sigurnost i rizika upravljanje. Na kraju programa enterprise scaffold je o mitigating rizika tvrtke radi lakšeg zaštita njihove privatnosti ovise i ciljevi tvrtki ili ustanovi.

Na sljedećoj slici opisuju komponente na scaffold. Temelj ovisi o pune Osmišljavanje odjela, račune i pretplate. U stupovima se sastoje od pravila Voditelj resursa i istaknuti imenovanja standarde. Ostatak scaffold dolazi iz core Azure mogućnosti i značajke tog Omogući okruženju sigurne i moguće upravljati.

![scaffold komponente](./media/resource-manager-subscription-governance/components.png)

> [AZURE.NOTE] Azure postala hitro nakon njegova Uvod u 2008. U ovom growth potrebna Microsoftu inženjerska timovima rethink njihove pristup za Upravljanje projektom i Implementacija servisa. Model upravljanja resursima Azure je uvedena u 2014 i zamjenjuje model klasični implementacije. Voditelj resursa omogućuje tvrtkama ili ustanovama jednostavnije implementacije, organiziranje i upravljanje Azure resursi. Voditelj resursa obuhvaća parallelization prilikom stvaranja resursi za brže implementacije složene, međusobno zavisnih rješenja. Uključuje i kontrola pristupa zrnastog i mogućnost resursi oznaka s metapodacima. Microsoft preporučuje stvorite sve resurse kroz model Voditelj resursa. Model upravljanja resursima izričito namijenjena enterprise scaffold.

## <a name="define-your-hierarchy"></a>Definirajte hijerarhije

Foundation na scaffold je registracija Enterprise Azure (i portalu Enterprise). Registracija enterprise određuje oblik i pomoću servisa Azure unutar tvrtke te je core upravljanja strukturu. Unutar ugovor enterprise korisnici će moći dodatno Podjela okruženje u odjela, račune i na kraju, pretplate. Azure pretplate je osnovna jedinica gdje se nalaze svi resursi. Određuje i nekoliko ograničenja unutar Azure, kao što je broj jezgri, resursima itd.

![Hijerarhija](./media/resource-manager-subscription-governance/agreement.png)

Svaki enterprise razlikuje se i hijerarhije Prethodna slika omogućuje značajnu fleksibilnost u način organiziranja Azure unutar tvrtke. Prije implementacije upute koje se nalaze u dokumentu, trebali biste modela hijerarhiju i posljedice na naplatu, access resursa i složenosti.

Tri uobičajene uzorke Azure Enrollments su:

- **Funkcionalno** uzorak

    ![funkcionalno](./media/resource-manager-subscription-governance/functional.png)

- Uzorak **poslovne jedinice** 

    ![tvrtke](./media/resource-manager-subscription-governance/business.png)

- **Geografske** uzorak

    ![geografske](./media/resource-manager-subscription-governance/geographic.png)

Primjena scaffold na razini pretplate da biste proširili Upravljanje zahtjevima tvrtki u pretplatu.

## <a name="naming-standards"></a>Imenovanje standarde

Prvi pillar od na scaffold je imenovanja standarde. Dobro osmišljena imenovanja standarde omogućuju prepoznavanje resursa na portalu za račun, a zatim u skripti. Najvjerojatnije, već imate imenovanje standarde infrastrukture na lokaciji. Prilikom dodavanja Azure u okruženju sustava, trebali biste proširili tih imenovanja standarda za Azure resurse. Imenovanje standardna olakšali učinkovitije upravljanje okruženja na svim razinama.

> [AZURE.TIP]
>
> - Pregledajte i prihvaćaju gdje moguće [uzoraka i prakse upute](./guidance/guidance-naming-conventions.md). Ovaj vodič pomoći će vam odlučivanje o smisleni imenovanja standardno.
> - Koristite camelCasing za nazive resursa (kao što su myResourceGroup i vnetNetworkName). Napomena: Nema resurse, kao što su računi za pohranu, gdje je mogućnost da biste koristili malim slovima (i bez druge posebne znakove).
> - Razmislite o korištenju pravila upravljanja resursima Azure (opisani u sljedećem odjeljku) da biste nametnuli imenovanja standarde.
 
## <a name="policies-and-auditing"></a>Pravila i nadzor

Drugi pillar od u scaffold uključuje stvaranja [pravila upravljanja resursima Azure](resource-manager-policy.md) i [nadzor zapisnik aktivnosti](resource-group-audit.md). Voditelj resursa pravila omogućuju upravljanje rizika u Azure. Možete definirati pravila koja bi samostalnosti podataka ograničavanje, provoditi ili nadzor određene akcije. 

- Pravilo je zadani **Dopusti** sustav. Akcije kontrolirati definiranje i korisnicima dodijelite pravila resursa koji uskratiti ili akcije resursa.
- Pravila opisane su po pravila definicijama pravila definicija jeziku (ako onda uvjeta).
- Stvorite pravila s JSON (Javascript objekt eksponenata) datoteke s oblikovanjem. Nakon definiranja pravila, dodijelite je dani doseg: pretplatu, grupa resursa. ili resurs.

Pravila imati više akcija koje omogućuju za preciznije pristup vašim scenarijima. Akcije su:

-   **Nemoj dopustiti**: blokira resursa zahtjev
-   **Nadzora**: omogućuje zahtjev, ali dodaje redak zapisnik aktivnosti (koji se može koristiti da biste nam upozorenja ili pokrenuti runbooks)
-   **Dodavanjem**: dodaje navedene informacije resurs. Ako, na primjer, ako nema oznake "CostCenter" na resursa, dodajte ta oznaka sa zadanom vrijednosti.

### <a name="common-uses-of-resource-manager-policies"></a>Uobičajene primjene pravila Voditelj resursa

Azure pravila upravljanja resursima su naprednih alata u komplet alata za Azure. Oni omogućuju vam da biste izbjegli neočekivane troškove za prepoznavanje središte troškova za resurse kroz označavanje i da biste bili sigurni da se zadovolje uvjeti compliancy. Kada se pravila kombiniraju pomoću ugrađenih značajki nadzora, možete fashion složene i prilagodljivo rješenja. Pravila omogućuju tvrtke za pružanje kontrole za radnih opterećenja "Tradicionalni IT" i "Agile" opterećenjem; kao što su razvoj aplikacija za klijenta. Najčešći uzoraka Vidimo pravila su:

- **Samostalnosti zemlj usklađenost/podataka** – Azure nudi područja širom svijeta. Velike tvrtke često želite da biste odredili gdje resursi stvaraju (želite li da biste bili sigurni samostalnosti podataka ili samo da biste bili sigurni resursi stvaraju blizu krajnji korisnici resursa).
- **Upravljanje trošak** – programa Azure pretplate može sadržavati resursi više vrsta i promjena veličine. Korporacije često želite biti sigurni da standardne pretplate izbjeći nepotrebno velika resursa, što je cijena stotine dolarima mjesec ili više njih.
- **Zadani upravljanja kroz potrebne oznake** - potrebno oznake jedan je od najčešćih i vrlo željene značajke. Pomoću pravila upravljanja resursima Azure korporacije se da biste bili sigurni da komponenta označili resursa. Najčešći oznake: odjel, vlasnik resursa i okruženja vrsta (Ako, na primjer – radnog test, razvoj)

**Primjeri**

"Tradicionalni IT" pretplatu za redak poslovnim aplikacijama

-   Nametanje odjela i vlasnika oznake na sve resurse
-   Ograničiti stvaranje resursa na područje Sjeverna Američka
-   Ograničiti mogućnost stvaranja VMs G niza i klastere HDInsight

"Agilno" okruženju za poslovnu jedinicu stvaranje aplikacija oblaka

- Da biste zadovoljava preduvjete samostalnosti podataka, omogućuju stvaranje resursa samo u određenoj regiji.
- Nametanje okruženje oznake na sve resurse. Ako resurs je stvorena bez oznake, dodati u **okruženja: nepoznato** oznaka resursa.
- Nadzora kada resurse stvaraju izvan Sjeverne Amerike, ali ne sprječava.
- Nadzor stvaranja visoke trošak resursa.

> [AZURE.TIP]
>
> Najčešće koriste pravila upravljanja resursima u tvrtkama ili ustanovama je kontrola *gdje* resursi mogu stvoriti i moguće stvoriti *koje* vrste resursa. Osim pruža kontrola na *mjesto na kojem* i *koje*mnogo korporacije pomoću pravila da biste bili sigurni resursi imaju odgovarajuće metapodataka za naplatu natrag za potrošnju. Preporučujemo da Primjena pravila na razini pretplate za:
>
> - Samostalnosti zemlj usklađenost/podataka
> - Upravljanje troškovima
> - Potrebna oznaka (Determined po poslovne potrebe, kao što su BillTo, vlasnik aplikacija)
>
> Možete primijeniti dodatna pravila u niže razine opsega.
 
### <a name="audit---what-happened"></a>Nadzora – što se dogodilo?

Da biste vidjeli kako funkcionira okruženja, morate nadzor aktivnosti korisnika. Većinu resursa unutar Azure stvorite dijagnostičke zapisnike možete analizirati pomoću alata za zapisnik ili u paketu za upravljanje operacije Azure. Možete prikupiti zapisnike aktivnosti u više pretplate možete unijeti na odjela ili prikaz enterprise. Zapisi nadzora su važne dijagnostičkog alata i ključnih mehanizam okidača događajima u Azure okruženju.

Postavite zapisnika aktivnosti iz Voditelj resursa implementacijama omogućuju vam da biste odredili **operacije** snimljene i tko ih izvesti. Aktivnosti zapisnici mogu prikupiti i pridružuje pomoću alata kao što je prijava analitiku.

## <a name="resource-tags"></a>Oznaka resursa

Korisnici u vašoj tvrtki ili ustanovi dodati resursa na pretplatu, on postaje sve važne želite pridružiti resurse odgovarajuće odjela, klijentima i okruženje. Resursi kroz [oznake](resource-group-using-tags.md)možete pridružiti metapodataka. Pomoću oznaka da biste dobili informacije o resursu ili vlasnik. Oznake omogućuju vam ne samo zbrajanja i grupirati resursa na različite načine, ali te podatke koristiti u svrhu chargeback. Možete označavati resursa s najviše 15 parove ključ: vrijednosti. 

Oznaka resursa su fleksibilne i trebala bi biti priložena na većini resurse. Primjeri uobičajenih resursa oznake su:

- BillTo
- Odjel (ili poslovne jedinice)
- Okruženje (proizvodnje, fazi razvoja)
- Razina (Web razina, razina aplikacije)
- Vlasnik aplikacije
- Nazivprojekta

![oznaka](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Više primjera oznake, potražite u članku [preporučeno imenovanja konvencije za Azure resurse](./guidance/guidance-naming-conventions.md).

> [AZURE.TIP]
>
> Stvaranje označavanju strategije koja služi za identifikaciju preko pretplate koje metapodataka je potrebno za tvrtke, financije, sigurnost, upravljanje rizik i cjelokupan upravljanje okruženje. Razmotrite mogućnost pravila koja mandates označavanje za:
>
> - Grupa resursa
> - Prostor za pohranu
> - Virtualnim strojevima
> - Aplikacije servisa okruženja/web-poslužiteljima

## <a name="resource-group"></a>Grupa resursa 

Voditelj resursa omogućuje vam da biste postavili resursa u smisleni grupe za upravljanje naplatom ili prirodnim afinitet. Kao što je rečeno ranije, Azure ima dvije implementacije modela. U starijim modelu klasični osnovna jedinica upravljanje je pretplata. Ovo je teško raščlanjuju resursi pretplate koji su doveli Stvaranje velikog broja pretplate. S modelom resursima smo vidjeli Uvod grupa resursa. Grupa resursa su spremnici resursa koji su uobičajeni životni ciklus ili zajedničko korištenje atributa kao što su "svih SQL poslužitelja" ili "Aplikacija odgovora".

Grupa resursa ne može se nalaziti unutar drugoga i resursa možete pripadati samo jednu grupu resursa. Možete primijeniti određene akcije na svim resursima u grupu resursa. Brisanje grupu resursa, na primjer, uklanja sve resurse u grupu resursa. Obično smjestite cijelu aplikaciju ili povezane sustava u istoj grupi resursa. Ako, na primjer, tri sloju aplikacija pod nazivom Contoso web-aplikacija će sadržavati web-poslužitelj, poslužitelj aplikacije i SQL server u istoj grupi resursa.

> [AZURE.TIP]
>
> Organizirajte svoje grupe resursa mogu se razlikovati iz radnih opterećenja "Tradicionalni IT" u "Agilno IT" radnih opterećenja
>
> - "Tradicionalni IT" radnih opterećenja najčešće grupirane stavke unutar iste životni ciklus, kao što je aplikacija. Grupiranje aplikacija omogućuje za upravljanje aplikacijama za pojedinačne.
> - "Agilno IT" radnih opterećenja usredotočeni na vanjskim kupca dostupnog oblaka aplikacije. Grupa resursa treba odražavati slojevima implementacije (primjerice Web razina aplikacije sloju) i upravljanje njima.

## <a name="role-based-access-control"></a>Kontrola pristupa na temelju uloga

Vjerojatno postavljate si "tko treba imati pristup resursima?" i "kako kontrolirati pristup?" Dopuštanje ili jer se onemogućuje pristup portalu Azure i resursima na portalu za upravljanje pristupom je presudne. 

Kada se Azure je prethodno, pristup kontrolama za pretplatu su osnovni: Administrator ili Administrator suradnika. Pristup pretplate u programu access modela IMPLICITNIH klasični resursima na portalu. Nemaju preciznije kontrole koji vode proliferation pretplate omogućuje razinu pametnije pristup kontrole za registraciju za Azure.

U ovom proliferation pretplate više nije potrebna. Kontrola pristupa na temelju uloga standardnih uloga (kao što su zajedničke "čitač" i "writer" vrste uloge) možete dodijeliti korisnicima. Možete definirati i prilagođene uloge.

> [AZURE.TIP]
>  
> - Identitet tvrtke pohrane (najčešće servisa Active Directory) povezati Azure Active Directory pomoću alata za AD Connect.
> - Kontrola administratore/ko-administrator pretplate pomoću Upravljani identitet. **Ne** Dodijelite administratore/ko-administrator vlasnika pretplate. Umjesto toga koristite RBAC ulogama za pružanje prava **vlasnika** u grupu ili pojedinačne.
> - Dodavanje Azure korisnika u grupu (na primjer, aplikacija X vlasnici) u servisu Active Directory. Koristite sinkronizirane grupe članovi grupe odgovarajuća prava za upravljanje grupom resursa koji sadrži aplikaciju.
> - Slijedite načelo **najmanjih ovlasti** potrebne da biste učinili očekivani rada za odobravanje. Ako, na primjer:
>     - Grupa implementacije: Grupu koja je moći uvesti resursi.
>     - Upravljanje virtualnog računala: Grupu koja se može ponovno pokrenuti VMs (za operacije)

## <a name="azure-resource-locks"></a>Zaključavanja Azure resursa

Kao što je vaša organizacija dodaje core services na pretplatu, on postaje sve što je važno da bi bili tih servisa dostupna da biste izbjegli prekidu tvrtke. [Ograničenja resursa](resource-group-lock-resources.md) omogućuju ograničavanje operacije jakih resursa gdje izmjenu ili brisanja promijenile značajan utjecaj na aplikacijama ili infrastrukture u oblaku. Zaključavanja možete primijeniti na pretplatu, grupa resursa ili resurs. Obično ćete primijeniti zaključavanja foundational resursima kao što su virtualne mreže, pristupnika i račune za pohranu. 

Ograničenja resursa trenutno podržava dvije vrijednosti: CanNotDelete i samo za čitanje. CanNotDelete znači da korisnici (s odgovarajućim prava) i dalje možete čitati ili izmjena resursa, ali ne možete izbrisati. Samo za čitanje označava ovlašteni korisnici ne možete izbrisati ili izmijeniti resurs.

Stvaranje ili brisanje upravljanje zaključavanja, morate imati pristup `Microsoft.Authorization/*` ili `Microsoft.Authorization/locks/*` akcije.
Ugrađeni uloga samo vlasnik i korisnički pristup administratora odobravaju tih akcija.

> [AZURE.TIP] Mogućnosti za mrežne Core treba zaštićena zaključavanja. Nenamjerno brisanje pristupnika VPN-a web-mjesto bi disastrous Azure pretplatu. Azure ne dopušta da biste izbrisali virtualne mreže koja se koristi, ali primjene dodatna ograničenja je korisno opreza. Pravila su ključnih održavanja odgovarajuće kontrole. Preporučujemo da primijenite **CanNotDelete** Zaključaj da biste pravila koja se koriste.
>
> - Virtualne mreže: CanNotDelete
> - Mrežni sigurnosne grupe: CanNotDelete
> - Pravila: CanNotDelete

## <a name="core-networking-resources"></a>Temeljni mrežni resursi

Pristup resursima može biti interni (unutar mreže na corporation) ili vanjski (putem Interneta). Je lako korisnicima u tvrtki ili ustanovi slučajno postavili resursi pogrešan mjesto i potencijalno ih otvoriti zlonamjernih programa access. S uređaja na lokaciji korporacije morate dodati odgovarajuće kontrole da biste bili sigurni da Azure korisnici odluke na desni. Radi postizanja pretplate utvrđujemo temeljni resursi koji omogućuju osnovne kontrole programa access. Temeljni resursi se sastojati od:

- **Mreža virtualne** su objekti spremnik za podmreže. Iako ne isključivo potrebno, često se koristi prilikom povezivanja aplikacije za interne tvrtke resurse.
- **Mrežni sigurnosne grupe** su slične vatrozid i navesti pravila kako resursa možete "razgovarati" putem mreže. Omogućuju zrnastog kontrolu nad kako / ako podmreže (ili virtualnog računala) možete povezati s Internetom ili druge podmreže u istom virtualne mreže.

![Temeljni mreže](./media/resource-manager-subscription-governance/core-network.png)

> [AZURE.TIP]
>  
> - Stvaranje virtualne mreže trakom za vanjske dostupnog radnih opterećenja i radnih opterećenja Interna dostupnog. Taj se način smanjuju izgledi da slučajno smještanje virtualnim strojevima koje su namijenjene Interna radnih opterećenja u vanjskim dostupnog prostora.
> - Mrežni sigurnosne grupe su ključna za tu konfiguraciju. Barem blokirajte pristup Internetu s internim virtualne mreža i blokirajte pristup korporacijskom mrežom s vanjskim virtualne mreža.

### <a name="automation"></a>Automatizacija

Pojedinačno Upravljanje resursima se oba dugotrajan i potencijalno pogreškama podložni za određene operacije. Azure nudi razne mogućnosti automatizaciju koji su uključujući Azure Automatizacija, logike aplikacije i funkcije Azure. [Automatizacija Azure](./automation/automation-intro.md) omogućuje administratorima omogućuje stvaranje i definirati runbooks učiniti uobičajene zadatke u upravljanju resursi. Stvaranje runbooks pomoću programa za uređivanje PowerShell kod ili grafički uređivač. Može proizvesti složenih više spremišta tijekova rada. Azure automatizaciju često se koristi za rukovanje uobičajene zadatke kao što su isključuje Neiskorišteni resursa ili stvaranje resurse kao odgovor na određene okidača bez potrebe za Ljudski intervencije.

> [AZURE.TIP]
>
> - Stvorite račun za Azure Automatizacija i pregledajte na dostupne runbooks (redak grafički i naredbe) dostupni u [Galeriji Runbook](./automation/automation-runbook-gallery.md).
> - Uvoz i Prilagodba ključa runbooks za osobnu upotrebu.
>
> Uobičajeni scenarij je mogućnost Start/isključivanje virtualnim strojevima prema rasporedu. Postoje primjer runbooks koji su dostupni u galeriji koji rukovati scenarij i naučiti kako ga proširili.


## <a name="azure-security-center"></a>Centar za sigurnost Azure

Možda jednu od najvećih skočnih prozora na cloud prihvaćanja je nedostataka putem sigurnost. IT voditelji rizik i sigurnost odjela je potrebno provjeriti jesu li resursa u Azure sigurne. 

[Centar za sigurnost Azure](./security-center/security-center-intro.md) nudi središnje prikaz stanje zaštite resursa u pretplate i njihovi preporuke za pomoć u sprječavanju ugrožena resursi. To možete omogućiti precizniji pravila (Ako, na primjer, Primjena pravila grupe određene resursa koji omogućuju enterprise da biste prilagodili svoje posture da biste rizika oni su adresiranja). Na kraju, centar za sigurnost Azure je otvorena platformu koja omogućuje Microsoftovi partneri i proizvođači softvera da biste stvorili softver koji se priključuje u Centar za sigurnost Azure da biste poboljšali njegovim mogućnostima. 

> [AZURE.TIP]
>
> Centar za sigurnost Azure omogućena je prema zadanim postavkama u svaku pretplatu. Međutim, morate omogućiti prikupljanje podataka iz virtualnih računala da biste omogućili Azure centar za sigurnost da biste instalirali njegov agent i počnite prikupljanje podataka.
>
> ![Prikupljanje podataka](./media/resource-manager-subscription-governance/data-collection.png)

## <a name="next-steps"></a>Daljnji koraci

- Sad kad ste naučili o upravljanja pretplatom, vrijeme je da biste vidjeli te preporuke u praksi. Pogledajte [primjere implementaciju upravljanja Azure pretplate](resource-manager-subscription-examples.md).

*U ovoj se temi pridonio [Karl Kuhnhausen](https://github.com/karlkuhnhausen) .*
