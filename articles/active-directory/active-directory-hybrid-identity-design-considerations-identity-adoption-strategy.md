<properties
    pageTitle="Azure Active Directory hibridnog identiteta zahtjevi dizajna – definiranje strategije prihvaćanja hibridnog identiteta | Microsoft Azure"
    description="S kontrolom uvjetno pristup Azure Active Directory provjerava određenim uvjetima koje ste odabrali prilikom provjere autentičnosti korisnika i prije omogućivanja pristupa aplikaciji. Kada su ti uvjeti zadovoljeni, korisnik je autentičnost provjerena i dopuštenje za pristup aplikaciji."
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


# <a name="define-a-hybrid-identity-adoption-strategy"></a>Definiranje strategije prihvaćanja hibridnog identiteta

U ovom ćete zadatku ćete definirati Strategije za prihvaćanja hibridnog identiteta za rješenje hibridnog identiteta za zadovoljava preduvjete za tvrtke koji su se spominju u:

- [Određivanje poslovne potrebe](active-directory-hybrid-identity-design-considerations-business-needs.md)
- [Određivanje preduvjeti za sinkronizaciju direktorija](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
- [Određivanje preduvjeti višestruka provjera autentičnosti](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Definiranje strategije poslovne potrebe
Prvi zadatak adrese određivanje tvrtke tvrtke ili ustanove mora.  To može biti vrlo širok i opseg creep se može dogoditi ako niste oprezni.  U početku neka bude jednostavno, ali ne zaboravite plan za dizajn koji bi odgovarao i olakšavaju promjena u budućnosti.  Bez obzira na to jesu li se jednostavni dizajn ili iznimno složen jedan, Azure Active Directory je platformu Microsoft Identity s podrškom za Office 365, Microsoft Online Services i oblaka umu aplikacije.

## <a name="define-an-integration-strategy"></a>Definiranje stvaranje strategije Integracija
Microsoft ima tri glavna Integracija scenarija koje su oblaka identiteta, sinkronizirane identiteta i pridruženi identiteta.  Trebali biste planirati na jednu od tih strategije Integracija usvojio.  Strategije odaberite mogu se razlikovati, a odluke u odabirom može sadržavati, koju vrstu korisničkog sučelja koje želite unijeti, imate li neke postojeću infrastrukturu već lokalno i što je s troškovima učinkovitijeg.  
 
![](./media/hybrid-id-design-considerations/integration-scenarios.png)

Scenariji definirani iznad slici su:

- **Oblak identiteta**: to su identiteta koji postoje isključivo u oblaku.  U slučaju Azure AD bi nalaze posebno u direktoriju Azure AD.
- **Synchronized**: to su identiteta koji postoje na lokalnom i u oblaku.  Korištenje Azure AD Connect, ti korisnici stvaranja ili pridružen postojećih računa Azure AD.  Korisničke lozinke raspršivanje sinkronizira se s lokalnog okruženja u oblak na što se naziva predmemorije za lozinku.  Kada pomoću sinkronizira jedan caveat je da ako korisnik je onemogućen u lokalnog okruženja, može potrajati do 3 sata za taj račun status prikazivati Azure AD.  To je zbog vremenski interval sinkronizacije.
- **Vanjski**: ove identiteta postoji i lokalne i u oblaku.  Korištenje Azure AD Connect, ti korisnici stvaranja ili pridružen postojećih računa Azure AD.  
 
>[AZURE.NOTE]
Dodatne informacije o sinkronizaciji mogućnosti pročitajte [integriranje vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).

U sljedećoj su tablici omogućit će prilikom određivanja prednosti i nedostatke svakog od sljedećih načina:

| Strategije         | Prednosti                                                                                                                                                                                                                                                  | Nedostaci                                                                                                                                                                                                                                                                                                                                                  |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Oblak identiteta** | Jednostavnije upravljati za malu tvrtku ili ustanovu. <br> Ništa da biste instalirali na – lokalno-bez dodatni hardver potreban<br>Jednostavno onemogućeno ako korisnik napusti tvrtku                                                                                                   | Korisnici će morati prijaviti prilikom pristupanja radnih opterećenja u oblaku <br> Lozinke možda ili možda neće biti jednaki za oblak i lokalnih identiteta                                                                                                                                                                                                                     |
| **Sinkronizirati**     | Lozinku lokalnog će provjeru autentičnosti i lokalne i cloud direktorija <br>Jednostavnije upravljati za male, Srednje i velike tvrtke ili ustanove <br>Korisnici mogu imaju jedinstvenu prijavu (SSO) za nekoliko resursa <br> Microsoft željeni način za sinkronizaciju <br> Jednostavnije upravljati | Neki klijenti možda oklijevati da biste sinkronizirali svoje mape s oblaka krajnji policiju određene tvrtke ili ustanove                                                                                                                                                                                                                                                  |
| **Pridružene**        | Korisnici mogu imati jedinstvenu prijavu (SSO) <br>Ako korisnik je prekinut ili ostavlja, račun možete odmah onemogućeno i pristup povučen,<br> Podržava napredne scenarija koji nije moguće, sa sinkronizirati                                           | Dodatne korake za postavljanje i konfiguriranje <br> Veća održavanja <br> Dodatni hardver možda obavezan za infrastrukturu STS <br> Možda ćete morati dodatni hardver da biste instalirali vanjskom poslužitelju. Dodatni softver potreban je ako se koristi AD FS <br> Obavezan proširenom instalacijski program za jedinstvene Prijave <br> Točka kritično pogrešku ako poslužitelj za vanjski pristup, korisnici neće biti moguće provjeriti autentičnost |

### <a name="client-experience"></a>Sučelje za klijenta
Strategija koju koristite određuje sučelje za prijavu korisnika.  Sljedeća tablica daje informacije o koje korisnici očekivati njihove sučelje prijavi se.  Imajte na umu da sve davatelji pridruženim identiteta podržava SSO u svim situacijama.

**Doman pridruženo i privatni mrežne aplikacije**:
 

|                              | Sinkronizirane identiteta      | Pridruženi identiteta                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Web-preglednici                 | Provjeru autentičnosti utemeljenu na obrascima | znak na, ponekad morati navesti ID tvrtke ili ustanove |
| Outlook                      | Zatraži vjerodajnice     | Zatraži vjerodajnice                                       |
| Skype za tvrtke (Lync)    | Zatraži vjerodajnice     | jedinstvene prijave na za Lync, zatražiti vjerodajnice za Exchange   |
| Skydrive Pro                 | Zatraži vjerodajnice     | jedine prijave                                               |
| Office Pro Plus pretplate | Zatraži vjerodajnice     | jedine prijave                                               |

**Vanjskih ili nepouzdanoj izvora**:

|                              | Sinkronizirane identiteta      | Pridruženi identiteta                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Web-preglednici                 | Provjeru autentičnosti utemeljenu na obrascima |  Provjeru autentičnosti utemeljenu na obrascima |
| Outlook, servisa Skype za tvrtke (Lync), Skydrive Pro, pretplata na Office| Zatraži vjerodajnice     | Zatraži vjerodajnice                                       |
| Exchange ActiveSync    | Zatraži vjerodajnice     | jedinstvene prijave na za Lync, zatražiti vjerodajnice za Exchange   |
| Mobilne aplikacije                 | Zatraži vjerodajnice     | Zatraži vjerodajnice                                               |

Ako ste zaključili iz zadatka 1 imate 3 strana IdP ili su prelaska da biste koristili neki omogućuju pridruženi Azure AD, morate imati na umu sljedeće značajke podržane:
- Bilo kojem davatelju usluga SAML 2.0 koja je usklađena SP ograničeno profila mogu podržavati provjere autentičnosti za Azure AD i povezane aplikacije
- Podržava pasivnu provjeru autentičnosti, što olakšava provjere autentičnosti za OWA, SPO itd.
- Klijenti za Exchange Online možete podržane putem na SAML 2.0 Poboljšana klijent profila (Upravljačke)

Mora biti svjesni koje mogućnosti neće biti dostupne:

- Bez WS-pouzdanost/podrške, prekinut će drugim aktivni klijentima
 - To znači da nema klijentski program Lync, OneDrive klijenta, pretplate na Office, Office Mobile prije Office 2016
- Prijelaz sustava Office na pasivnu provjeru autentičnosti će dopustite podržava čisto IdPs SAML 2.0, ali podrška će i dalje biti na temelju klijenta po klijenta


>[AZURE.NOTE]
Za većinu ažurirani popis pročitajte članak http://aka.ms/ssoproviders.

## <a name="define-synchronization-strategy"></a>Definiranje strategije sinkronizacije
U ovom ćete zadatku definirate alatima koji će se koristiti za sinkronizaciju u tvrtki ili ustanovi lokalnih podataka u oblak i što topologije trebali biste koristiti.  Budući da, većina tvrtki ili ustanova koristi Active Directory, informacije o korištenju Azure AD Connect adrese pitanja gore navedeni su neki detaljno.  Za okruženja koji nemaju servisa Active Directory, postoji informacije o korištenju FIM 2010 R2 ili MIM 2016 da biste lakše planirati ovaj strategije.  Međutim, buduća izdanja Azure AD Connect će podržavati LDAP direktorija, pa se ovisno o vremenske trake, ti podaci mogu pomoći.

###<a name="synchronization-tools"></a>Alati za sinkronizaciju
Tijekom godina, imate nekoliko alata za sinkronizaciju postojao i koriste u različitim scenarijima za.  Azure AD Connect trenutno Idi na alat za odabir za sve podržane scenarije.  AAD Sync DirSync se i dalje i oko i čak i možda su prisutne u okruženju sustava sada. 

>[AZURE.NOTE]
Najnovije informacije o podržanim mogućnosti svih alata, članak [direktorija Integracija Alati za usporedbu](active-directory-hybrid-identity-design-considerations-tools-comparison.md) .  

### <a name="supported-topologies"></a>Podržanih topologija
Pri definiranju strategije sinkronizacije, potrebno je odrediti topologije koja se koristi. Ovisno o informacije koje je određeno u koraku 2 možete odrediti koje topologije je proper će se koristiti. U jedan skup stabala, jedan topologije Azure AD se najčešće i sastoji se od jednog skupa stabala servisa Active Directory i instancu Azure AD.  To će se koristiti u slučaju scenariji i je očekivani topologije kada koristite Azure povezivanje AD Express instalaciju, kao što je prikazano na slici u nastavku.
 
![](./media/hybrid-id-design-considerations/single-forest.png)Scenarij skupa stabala jedne vrlo uobičajeno je za velike i čak i male tvrtke ili ustanove da bi više šuma, kao što je prikazano na slici 5.

>[AZURE.NOTE]
Dodatne informacije o različitih lokalnih i Azure AD topologija s Azure AD Connect sinkronizacije u članku [Topologija za Azure AD Connect](active-directory-aadconnect-topologies.md).


![](./media/hybrid-id-design-considerations/multi-forest.png) 

Scenarij više skupa stabala

Ako je to slučaj, a zatim topologije višestruka-forest-jednostruki Azure AD razmatranje istinitosti sljedećih stavki:

- Korisnici imaju samo 1 identiteta preko svih šuma – jedinstveni identifikacijski korisnici odjeljku u nastavku opisuju to detaljnije.
- Korisnik se autentificira servisa na skup stabala u kojoj se nalazi njihov identitet
- UPN i sidro izvora (immutable id) potjecati iz ovog skupa stabala
- Sve šuma mogu pristupiti samo Azure AD Connect – to znači da ne morate domene pridruženo i može se postaviti u na DMZ ako to je to olakšava.
- Korisnici imati samo jedan poštanski sandučić
- Skup stabala koji hostira poštanski sandučić za korisnika je najbolje kvalitete podataka za atribute koje su vidljive u sustava Exchange globalni popis adresa (GAL)
- Ako postoji nijedan poštanski sandučić na korisnike, zatim sve skupa stabala može se koristiti za suradnju te vrijednosti
- Ako imate povezane poštanskog sandučića, zatim postoji i drugi račun u različitim skupa stabala koristiti za prijavu.

>[AZURE.NOTE]
Objekata koji se nalaze u obje lokalnog i u oblaku "spojeni" putem Jedinstveni identifikator. U kontekstu sinkronizacije direktorija, Jedinstveni identifikator se nazivaju u SourceAnchor. U kontekstu sustava jedinstvenu prijavu, to se naziva u ImmutableId. [Konceptima dizajna za Azure AD Connect](active-directory-aadconnect-design-concepts.md#sourceanchor) za Dodatne napomene vezane uz korištenje SourceAnchor.

Ako navedenog istinito ne, a imate više od jedne aktivni račun ili više od jedne poštanskog sandučića, Azure AD Connect će odaberite jedan i drugi Zanemari.  Ako ste povezani poštanske sandučiće, ali bez nekim drugim računom, poslovne subjekte će se izvesti Azure AD, a taj korisnik će biti član grupe.  Time se razlikuje od kako ga je u prošlosti uz DirSync i je li namjerno da biste bolje podrška više skupa stabala scenarija. Scenarij više skupa stabala je prikazan na slici u nastavku.
 
![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 
 
**Više skupa stabala više scenarij Azure AD**

Preporučuje se da bi samo jedan direktorij u Azure AD za organizaciju, ali podržano je između poslužitelj za Azure AD Connect sinkronizaciju i u imeniku Azure AD ga se drži odnos 1:1.  Za svaku instancu Azure AD ćete instalacija programa Azure AD Connect.  Osim toga, Azure AD dizajnom je Izolirani i korisnika u jednu instancu Azure AD nećete moći vidjeti kako korisnici u drugoj instanci.

Da je moguće i podržanim povezati jednu instancu lokalnog servisa Active Directory više Azure AD direktorija kao što je prikazano na slici u nastavku:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 
 

**Jedan-skupa stabala filtriranja scenarija**

Da biste to učinili sljedeće moraju biti ispunjeni:

- Azure AD Connect sinkronizacijom poslužitelja mora biti konfigurirana za filtriranje da svaki imaju isključivih skup objekata.  To učiniti, na primjer, određivanje opsega svaki poslužitelj za određene domene ili organizacijsku jedinicu.
- DNS domena biti registriran samo u jednom Azure AD direktorija tako da na UPNs korisnika u lokalnom sustavu AD morate koristiti odvojene prostora naziva
- Korisnici u jednu instancu Azure AD samo moći vidjeti korisnike s instancom.  Oni neće moći vidjeti korisnika u drugim slučajevima
- Samo jedan od direktorija Azure AD možete omogućiti hibridna implementacija sustava Exchange s lokalnog sustava AD
- Međusobna exclusivity odnosi i na pisanje natrag.  Ovime neke značajke pisanje natrag nije podržano s ovom topologije jer te pretpostavlja jedan lokalnog konfiguracije.  To obuhvaća:
 - Grupiranje pisanje natrag sa zadanom konfiguracijom
 - Pisanje natrag za uređaj


Imajte na umu sljedeće nije podržan te treba odabrati kao implementacija:

- Nije podržano imati više Azure AD Connect sinkronizacijom poslužitelja povezivanja s istom direktorija Azure AD čak i ako su konfigurirana za sinkronizaciju isključivih skup objekta
- Podržana je da biste sinkronizirali isti korisniku više Azure AD imenika. 
- I nepodržane da biste promijenili da bi korisnici u jedan Azure AD da se prikazuju kao kontakata u drugom imeniku Azure AD. 
- Također nije podržana za izmjenu Azure AD Connect sinkronizaciju povezati s više Azure AD imenika.
- Azure AD direktorija su Izolirani dizajna. To nije podržana za promjenu konfiguracije Azure AD Connect sinkronizacije za čitanje podataka iz druge imenika Azure AD za sastavljanje uobičajenih i objedinjenih GAL između direktorija. I nepodržane da biste izvezli korisnika kao kontakata u drugu lokalnog AD pomoću sinkronizacije Azure AD Connect.


>[AZURE.NOTE]
Ako vaša tvrtka ili ustanova ograničava računala u vašoj mreži povezivanje s Internetom, u ovom se članku popis krajnjih točaka (certifikatom, IPv4 i IPv6 adrese raspona) treba li se uključiti u svoje izlaznog Dopusti popise i Internet Explorer pouzdana odredišta klijenta računala da biste bili sigurni računala uspješno možete koristiti Office 365. Dodatne informacije pročitajte u [Office 365 URL-ovi i rasponi IP adresa](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

## <a name="define-multi-factor-authentication-strategy"></a>Definiranje strategije višestruka provjera autentičnosti
U ovom ćete zadatku će definirati strategije višestruke provjere autentičnosti za korištenje.  Azure višestruka provjera autentičnosti dolazi u dva drugu verziju.  Jedan je u oblaku, a drugi je lokalnog temelji pomoću poslužitelja za Azure MFA.  Na temelju izračuna niste iznad koje možete odrediti koje rješenje je onaj za strategiju.  U sljedećoj tablici da biste utvrdili koju najbolja mogućnost za dizajn ispunjavanje preduvjeta sigurnosti vaše tvrtke:

Mogućnosti višestruke provjere dizajna:

| Sredstva za osiguranje                                               | MFA u oblaku | MFA na lokaciji |
|---------------------------------------------------------------|------------------|----------------|
| Aplikacija Microsoft                                                | Da              | Da            |
| SaaS aplikacije u galeriju aplikacija                                  | Da              | Da            |
| Aplikacija IIS objavljene putem Proxy aplikacije za Azure AD         | Da              | Da            |
| IIS aplikacije koje nisu objavljene putem aplikacije Proxy za Azure AD | ne               | Da            |
| Daljinski pristup u svojstvu VPN, RDG                                     | ne               | Da            |

Iako ste možda imaju dogovorenog na rješenje za strategiju, svejedno morate koristiti analizu odozgo na kojoj se nalaze vaši korisnici.  To može uzrokovati rješenja da biste promijenili.  U sljedećoj tablici da bi vam određivanje ovo:

| Korisničke lokacije                                                       | Mogućnosti željeni dizajn                 |
|---------------------------------------------------------------------|-----------------------------------------|
| Azure Active Directory                                              | Višestruki FactorAuthentication u oblaku |
| Azure AD i lokalnog servisa Active Directory pomoću vanjski pristup za AD FS             | Oba                                    |
| Azure AD i lokalnog AD pomoću Azure AD povezivanje sinkronizaciju bez lozinke | Oba                                    |
| Azure AD i lokalnog pomoću Azure AD Connect u sinkronizaciju lozinke  | Oba                                    |
| Lokalni AD                                                      | Višestruka provjera autentičnosti poslužitelja      |

>[AZURE.NOTE]
Trebali biste provjerite podržava li mogućnost višestruka provjera autentičnosti dizajna koje ste odabrali značajke koje su potrebne za dizajn.  Dodatne informacije pročitajte [Odabir višestruku sigurnosno rješenje umjesto vas](../multi-factor-authentication-get-started.md#what-am-i-trying-to-secure).

## <a name="multi-factor-auth-provider"></a>Davatelja usluge provjere autentičnosti višestruke provjere
Višestruka provjera autentičnosti dostupna je prema zadanim postavkama za globalne administratore koji imaju Azure Active Directory klijenta. Međutim, ako želite produljiti višestruke provjere autentičnosti za sve korisnike i/ili želite da vaše globalni administratori omogućiti da iskoristite prednost značajke kao što su portal za upravljanje, prilagođene čestitke i izvješća, zatim morate kupiti i konfiguriranje davatelja usluge provjere autentičnosti višestruke provjere.

>[AZURE.NOTE]
Trebali biste provjerite podržava li mogućnost višestruka provjera autentičnosti dizajna koje ste odabrali značajke koje su potrebne za dizajn. 

##<a name="next-steps"></a>Daljnji koraci
[Određivanje potrebama za zaštitu podataka](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Vidi također
[Pregled pitanja vezana uz dizajn](active-directory-hybrid-identity-design-considerations-overview.md)
