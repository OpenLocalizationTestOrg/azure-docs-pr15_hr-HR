
<properties
    pageTitle="Azure Active Directory hibridnog identiteta zahtjevi dizajna - određivanje hibridnog identiteta životni ciklus prihvaćanja strategije | Microsoft Azure"
    description="Omogućuje definiranje zadatke upravljanja identiteta hibridnog prema mogućnosti koje su dostupne za svaki faza životni ciklus."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Određivanje hibridnog identiteta životni ciklus prihvaćanja strategije
U ovom ćete zadatku ćete definirati strategije upravljanja identiteta za rješenje hibridnog identiteta za zadovoljava preduvjete za tvrtke koji ste definirali u [zadatke upravljanja za određivanje hibridnog identitet](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md).


Da biste definirali zadatke upravljanja identiteta hibridnog prema životni ciklus završetka do kraja identiteta prezentirati ranije u ovom ćete koraku, imat ćete treba uzeti u obzir mogućnosti koje su dostupne za svaki faza životni ciklus.

## <a name="access-management-and-provisioning"></a>Upravljanje pristupom i dodjela resursa
S rješenjem upravljanje pristup dobar račun, vašoj tvrtki ili ustanovi možete pratiti točno tko ima pristup informacijama koje u tvrtki ili ustanovi.

Kontrola pristupa je ključnih funkcija sustava središnje, jednu točku dodjele resursa. Osim zaštite povjerljive podatke, pristup kontrolama za izlaganje postojećih računa koji imaju neodobrene autorizacijama ili su više nisu potrebne. Da biste odredili zastarjeli račune za dodjelu resursa sustava veze zajedno podaci o računu mjerodavne informacijama o korisnicima koji vlasnik račune. Informacije o identitetu mjerodavne korisnika obično zadržavaju u bazama podataka i direktorija Ljudski resursi.

Računi u sofisticirane IT korporacije uključuju stotine parametre koji određuju državne uprave, a te detalje upravlja sustava za dodjelu resursa. Novi korisnici se može identificirati s podacima koje ste omogućili iz pouzdanih izvora. Mogućnost za odobrenje zahtjeva pristupa pokreće procesa koje odobravanje (ili odbijanje) resursa dodjele resursa za njih.


| Životni ciklus upravljanje faze          | Lokalno                                                                                                                                                                                                                                                       | Oblak                                                                                                                                                                                                                                                                                                                     | Hibridno                                                                                   |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Upravljanje računima i dodjela resursa | Pomoću uloga poslužitelja Active Directory® Domain Services (AD DS), možete stvoriti skalabilni, siguran i moguće upravljati infrastrukture za korisnika i upravljanje resursima i pruža podršku za imenika pokrenuti kao što je Microsoft® Exchange Server. <br><br> [Možete Dodjela grupe u AD DS putem programa Upravitelj identiteta](https://technet.microsoft.com/library/ff686261.aspx) <br>[Možete dodjelu resursa korisnicima u AD DS](https://technet.microsoft.com/library/ff686263.aspx) <br><br> Administratori mogu koristiti kontrolu pristupa za upravljanje korisničkim pristupom sa zajedničkim resursima sigurnosnih razloga. Kontrola pristupa u servisu Active Directory se upravlja na razini objekta tako da postavku različite razine pristupa ili dozvole za objekte, kao što su Potpuna kontrola, pisanje, čitanje ili bez pristupa. Kontrola pristupa u servisu Active Directory definira kako drugi korisnici možete koristiti objektima servisa Active Directory. Prema zadanim postavkama dozvole za objekte u servisu Active Directory postavljene na najsigurnija postavka. | Morate stvoriti račun za svakog korisnika koji će imati pristup s Microsoftovim servisom u oblaku. Možete promijeniti korisničke račune ili ih izbrisati kada ste više nije potrebna. Prema zadanim postavkama, korisnici imaju administratorske dozvole, ali ih po želji možete dodijeliti. Dodatne informacije potražite u članku [Upravljanje korisnicima u Azure AD](active-directory-create-users.md). <br><br> Servisu Azure Active Directory, jedna od glavnih značajki je mogućnost Upravljanje pristupom resursima. Ove resurse može biti dio imenika, kao u slučaju dozvola za upravljanje objekte kroz ulogama u imenik ili resursi koji se nalaze izvan direktorija, kao što su SaaS aplikacije, Azure services i web-mjesta sustava SharePoint ili na lokalnu instancu resursi. <br><br> Centar za od Azure Active Directory, access rješenje upravljanja biti sigurnosne grupe. Vlasnik resursa (ili administrator direktorija) možete dodijeliti grupu u koju želite određene pristup desno na resurse oni vlasnik. Članovi grupe objavit ćemo zajedno s pristupom, a vlasnik resursa možete delegirati ograničenu pravo da upravljaju popisa članovi grupe nekome drugome – kao što su upravitelju odjela ili administrator službom za korisnike<br> <br> Upravljanje grupama u temi Azure AD navedene su dodatne informacije o upravljanju pristupom putem grupe.| Proširivanje identiteta servisa Active Directory u oblaku putem sinkronizacije i vanjski pristup |

## <a name="role-based-access-control"></a>Kontrola pristupa na temelju uloga
Access na temelju uloga Upravljanje ulogama koristi (RBAC) i dodjeljivanje pravilnika za procjenu, testirajte i provode poslovnih procesa i pravila o omogućivanju pristupa korisnicima. Ključne administratori stvoriti pravila za dodjelu resursa i korisnicima dodijelite uloge i koji definirali skupova prava na resurse za te uloge. RBAC proširuje rješenje za upravljanje identitet možete koristiti softverski procesa i smanjiti ručno interakcije s korisnikom tijekom postupka dodjele resursa.
Azure AD RBAC omogućuje tvrtki da biste ograničili količinu operacije koje pojedinac možete učiniti kada se on ima pristup Portal za upravljanje Azure. Pomoću RBAC za kontrolu pristupa portalu IT administratorima ca prava pristupa delegiranju pomoću sljedeće postupke za upravljanje programa access:

- **Dodjela grupne uloge**: access možete dodijeliti Azure AD grupe koje se mogu se sinkronizirati s lokalni Active Directory. Omogućuje vam korištenje postojećih ulaganja načinjene tvrtke ili ustanove u tooling i postupke za upravljanje grupama. Možete koristiti i značajku upravljanja ovlaštenog grupa od Azure AD Premium.
- **Leverage ugrađena ulogama u Azure**: koristite tri uloge – vlasnik, suradnika i Reader, proizvod tvrtke da biste bili sigurni da korisnici i grupe imati dozvolu za samo zadatke koje su im potrebne za svoje zadatke.
- **Granular pristupa resursima**: možete dodijeliti uloge korisnicima i grupama za određenu pretplatu, grupa resursa ili pojedinačnog Azure resursa kao što je web-mjesta ili u bazi podataka. Na taj način možete omogućiti da korisnici imaju pristup svim resursima koje su im potrebne i bez pristupa resursima koje su im potrebne za upravljanje.

## <a name="provisioning-and-other-customization-options"></a>Dodjeljivanje i druge mogućnosti za prilagodbu
Vaš tim možete koristiti poslovne planove i preduvjeti odlučiti koliko da biste prilagodili rješenje identitet. Ako, na primjer, velike tvrtke, morat fazama snimljene fotografije iz plan za tijekove rada i prilagođene prilagodnika koji se temelji na vremenskoj crti za postupno dodjeljivanje aplikacije koje se često koriste preko geographies. Neku drugu tarifu za prilagodbu dat će za dodijeljeni resursi preko za cijelu tvrtku ili ustanovu, nakon uspješne testiranje dva ili više aplikacijama. Moguće je prilagoditi aplikacije korisničkih interakcija i postupke za dodjeljivanje resursa možda promijenili kako bi odgovarao automatiziranog dodjele resursa.

Možete deprovision da biste uklonili servisa ili komponentu. Na primjer, deprovisioning računa znači račun izbriše s resursa.

Hibridno model dodjele resursa resursi kombinira zahtjev i na temelju uloga pristupa, koje su oba podržava Azure AD. Za podskup zaposlenika ili upravljanih sustavima, tvrtka možda želite automatizirati pristupa na temelju uloga dodjele. Tvrtka možda rukovati i sve druge zahtjeva za pristup ili iznimke kroz model utemeljenu na zahtjev. Neke tvrtke mogu započnite s ručno dodjele i poslovanja prema hibridnog model, pomoću programa namjera implementacija cijelosti utemeljeno na ulogama u budućim vrijeme.

Drugim tvrtkama možda pronaći nepraktično razloga tvrtke da biste postigli dovršeno na temelju uloga dodjele resursa i ciljnog hibridnog pristup kao djelatnike cilj. I dalje drugih tvrtki može biti zadovoljni samo utemeljen na zahtjev za dodjelu resursa, a ne želite da se ulažete dodatne trud definiranje i upravljanje na temelju uloga, automatiziranog pravilnicima za dodjelu resursa.

## <a name="license-management"></a>Upravljanje licencama
Upravljanje grupne licencama u Azure AD omogućuje korisnicima dodijelite sigurnosne grupe za administratore i Azure AD automatski dodjeljuje licence za sve članove grupe. Ako je korisnik naknadno dodati ili ukloniti iz grupe, licence će se automatski dodijeliti ili ukloniti sukladno situaciji.

Možete koristiti grupe sinkronizirati s lokalnim AD ili upravljanje njima u Azure AD. Uparivanje to prema gore s Azure AD premium samoposlužnog upravljanje grupe možete jednostavno dodijeliti dodjele licence za donose odgovarajuće odluke. Koje možete biti sigurni da probleme kao što su sukobe licenci i nema podataka o lokaciji automatski sortiraju.

## <a name="self-regulating-user-administration"></a>Samostalno regulating Administracija korisnika
Kada se pokrene vašoj tvrtki ili ustanovi Dodjela resursa preko svih interne organizacije, implementirati koja se sama regulating mogućnost administriranje korisnika. Prednosti i prednosti dodjelu resursa korisnicima možete shvatili preko granica tvrtke ili ustanove. U ovom okruženju promjene u korisničke status se automatski odražava u prava pristupa preko granica tvrtke ili ustanove i geographies. Možete smanjiti troškove za dodjelu resursa i procese programa access i odobrenje. Implementacije shvaća puni potencijal implementacije kontrole pristupa na temelju uloga za upravljanje završetka do kraja pristup u tvrtki ili ustanovi. Možete smanjiti administrativne troškove pomoću automatiziranog postupke za koje se tiču dodjeljivanja korisnika. Poboljšanje sigurnosti automatiziranje provođenje pravila i sigurnost i pojednostaviti i centralizirano upravljanje životni ciklus korisnika i dodjeljivanje resursa za velike korisnika populacije.

>[AZURE.NOTE]
Dodatne informacije potražite u članku postavljanje Azure AD za upravljanje pristupom prošao na samostalnom servisnim aplikacijama

Utemeljen na licence (prava sustavom) Azure AD services rad tako da aktiviranje pretplate na klijentu imeničkom servisu Azure AD. Kada je aktivna pretplata mogućnosti usluge može biti upravlja imeničkom servisu administratori i koristiti licencirani korisnici. Dodatne informacije potražite u članku kako Azure AD licenciranje rada?
Integracija s drugim 3 davateljima

Azure Active Directory nudi jedinstvene prijave na i poboljšane sigurnosti programa access aplikacije na tisuće SaaS aplikacije i lokalne web-aplikacije. Detaljni popis Azure Active Directory galeriju aplikacija za podržanim aplikacijama SaaS potražite u članku Azure Active Directory federation kompatibilnosti popisa: davatelji identiteta drugih proizvođača koji se mogu koristiti za implementaciju jedinstvenu prijavu

## <a name="define-synchronization-management"></a>Definiranje upravljanje sinkronizacije
Integracija direktorija vaše lokalne sa Azure AD omogućuje korisnicima produktivnosti unosom uobičajenih identiteta za pristup oblaku i lokalnog resursa. Uz ovaj integraciju sa korisnici i organizacije možete iskoristiti od sljedećeg:

- Tvrtke i ustanove unijeti korisnika završavaju na uobičajeni identiteta hibridnog preko lokalnog ili servisa u oblaku korištenje Windows Server Active Directory i zatim povezivanju sa servisom Azure Active Directory.
- Administratori unijeti uvjetno pristupa na temelju aplikacije resursa, uređaj i identitet korisnika, mrežno mjesto i višestruke provjere autentičnosti.
- Korisnicima omogućuje korištenje njihova uobičajena identiteta putem računa u Azure AD za Office 365, Intune, SaaS aplikacije i aplikacije drugih proizvođača.
- Razvojni inženjeri možete izraditi aplikacije pod utjecajem uobičajenih modela identiteta integraciji aplikacije u servisu Active Directory lokalnog ili Azure za aplikacije utemeljene na oblaku

Na slici u nastavku sadrži primjera više razine prikaza identiteta sinkronizacija.

![](./media/hybrid-id-design-considerations/identitysync.png)

Sinkronizacija identiteta

Pregledajte tablici u nastavku da biste usporedili mogućnosti sinkronizacije:

| Mogućnost upravljanja sinkronizacije          | Prednosti                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Nedostaci                                                                                                                                                                                                                                                                                  |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sinkronizacija sustavom (putem DirSync ili AADConnect) | Korisnici i grupe koji se sinkroniziraju iz lokalnog i oblaka <br>  **Kontrola pravila**: pravila računa možete postaviti putem servisa Active Directory, koji omogućuje administrator za upravljanje lozinki, radne stanice, ograničenja, zaključavanje iz kontrole, i Dodatno, a da pritom ne izvršavanje dodatnih zadataka u oblaku.  <br>  **Kontrola pristupa**: mogu ograničiti pristup servisa u oblaku tako da servise koje se može pristupiti putem korporacijskom okruženju kroz poslužitelji online ili oboje. <br>  Smanjene poziva službi za podršku: Ako korisnici imaju manje lozinke zapamtiti, su manje vjerojatno da ih zaboravili. <br>  Sigurnost: Identitetima korisnika i podaci su zaštićeni jer sve poslužitelje i servise koji se koriste u jedinstvenu prijavu, su usvojili i kontrolirati lokalnog. <br>  Podrška za Jaka provjera autentičnosti: Jaka provjera autentičnosti (naziva se i dvostruka provjera autentičnosti) možete koristiti s servisa u oblaku. Međutim, ako koristite Jaka provjera autentičnosti, morate koristiti jedinstvenu prijavu. |                                                                                                                                                                                                                                                                                                |
| Vanjski pristup sustavom (putem servisa AD FS)           | Omogućiti sigurnosnih tokena (STS). Kada konfigurirate programa STS za pristup jednom prijava s Microsoftovim servisom cloud, koji će stvoriti pridruženim pouzdanost između vaše lokalne STS i pridruženoj domeni koju ste naveli u klijentu za Azure AD. <br> Omogućuje krajnjim korisnicima omogućuje koristiti isti skup vjerodajnica da biste dobili pristup višestrukih resursa <br>Krajnji korisnici nemaju da biste zadržali više skupova vjerodajnice. Još korisnici moraju unijeti vjerodajnice za svaku od njih koji sudjeluju resursi., scenariji B2B i B2C podržane.                                                                                                                                                                                                                                                                                                                                                                                                             | Potreban je specijalizirane osoblja za implementaciju i održavanje namjenski lokalno poslužitelja za AD FS. Nema ograničenja upotrebe istaknuti provjere autentičnosti ako planirate koristiti AD FS za vaše STS. Dodatne informacije potražite u članku [Konfiguriranje Napredne mogućnosti za AD FS 2.0](http://go.microsoft.com/fwlink/?linkid=235649). |

>[AZURE.NOTE]
Dodatne informacije potražite u člancima, [Integracija vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).


## <a name="see-also"></a>Vidi također
[Pregled pitanja vezana uz dizajn](active-directory-hybrid-identity-design-considerations-overview.md)
