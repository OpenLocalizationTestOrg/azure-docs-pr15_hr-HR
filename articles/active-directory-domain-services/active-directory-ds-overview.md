<properties
    pageTitle="Pregled Azure Active Directory Domain Services | Microsoft Azure"
    description="Pregled Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure servisa Active Directory Domain Services

## <a name="overview"></a>Pregled
Azure servisi infrastrukturu omogućuju implementaciju širokog raspona računalstvo rješenja agilno način. S Azure virtualnim strojevima možete implementirati po gotovo i plaćate samo minute. Pomoću podrške za Windows, Linux, SQL Server, Oracle, IBM, SAP i BizTalk, možete implementirati uz sve radno opterećenje, bilo koji jezik na gotovo bilo kojeg operacijski sustav. Sljedeće pogodnosti omogućuju migrirati naslijeđene aplikacije implementiran lokalnog Azure da biste spremili na radu troškove.

Ključni aspekt migracije lokalni aplikacija za Azure obrađuje potrebama identiteta te aplikacije. Direktorija umu aplikacije možda ovise o LDAP za čitanje ili pristup zapisivanju imenika tvrtke ili ustanove ili ovise o integriranu provjeru autentičnosti sustava Windows (autentičnosti Kerberos ili NTLM) za provjeru autentičnosti krajnjim korisnicima. Redak tvrtke (LOB) aplikacije na poslužitelju Windows obično implementiran na računalima domene pridružili tako da ih može upravljati sigurno putem pravila grupe. 'Dizalica-a-shift' lokalnog aplikacijama u oblak, te međuzavisnosti infrastruktura za identitet tvrtke morate riješiti.

Na jedan od sljedeća rješenja za ispunjavanje identiteta potrebama svoje aplikacije u uveden u Azure često uključite administratori:

- Implementacija web-mjesto VPN veza između radnih opterećenja izvodi u servisa Azure infrastrukture i na imenika lokalnog.
- Proširivanje tvrtke infrastrukture domene/skup stabala AD postavljanjem kontrolera domena replike pomoću Azure virtualnih računala.
- Implementacija samostalni domene u Azure pomoću kontrolera domena u uveden kao Azure virtualnih računala.

Ove postupke se visoke trošak i administratorskih. Administratori moraju implementirati korištenje virtualnih računala u Azure kontrolera domena. Uz to, koje su im potrebne za upravljanje, sigurne, zakrpu, praćenje, sigurnosno kopirati i rješavanje problema te virtualnim računalima. U oslanjanje na lebdeće pokazivače VPN veza lokalnog imenika uzrokuje radnih opterećenja implementiran u Azure biti podložno problemčiće tranzitne mreže ili kvarove. Ove mreže kvarove shodno rezultat u donjem vrijeme aktivnosti i smanjene pouzdanosti za te aplikacije.

Ne možemo dizajniran Azure servisa Active Directory Domain Services omogućuju jednostavnije zamjenski.


## <a name="introducing-azure-ad-domain-services"></a>Uvod u Azure servisa Active Directory Domain Services
Azure servisa Active Directory Domain Services nudi upravljanih domenske servise kao što su domene spoj, grupe pravila, a zatim LDAP, Kerberos/NTLM provjere autentičnosti koje su potpuno kompatibilan sa sustavom Windows Server Active Directory. Možete korištenje tih servisa domene bez potrebe za implementaciju, upravljanje i zakrpu kontrolera oblaka. Azure servisa Active Directory Domain Services integrira u postojećem klijentu Azure AD, stoga upućivanje korisnicima omogućuje prijavite se pomoću vjerodajnica za tvrtke. Osim toga, možete koristiti postojeće grupe i korisničke račune za zaštitu pristupa resursima, stoga osiguravanje na jednostavnije 'Dizalica-i-shift' lokalnog resursa za servisa Azure infrastrukture.

Azure funkcionalnost servisa Active Directory Domain Services besprijekorno funkcionira bez obzira na to je li vaš klijent Azure AD samo oblak ili sinkronizirani s lokalnim Active Directory.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure AD domenske servise za samo oblak tvrtke ili ustanove
U samo oblak Azure AD klijenta (često se nazivaju 'upravljani klijenata') ne sadrži sve lokalne ostavlja identiteta u manji trag pri. Drugim riječima, korisničke račune, njihove lozinke i članstva u grupi su sve nativni u oblak – to jest, stvara i njima se upravlja u Azure AD. Proučite ukratko Contoso je na samo oblak Azure AD klijenta. Kao što je prikazano na sljedećoj slici, Contoso je administrator konfigurirao virtualne mreže servisa Azure Infrastruktura sustava. U ovom virtualne mreže na Azure virtualnim računalima sustava implementirana su aplikacije i radnih opterećenja poslužitelja. Budući da Contoso je klijentom samo oblak, sve identitetima korisnika, a zatim svoja uvjerenja i stvara i upravlja u Azure AD članstva u grupi.

![Azure servisa Active Directory Domain Services pregled](./media/active-directory-domain-services-overview/aadds-overview.png)

Contoso je IT administrator možete omogućiti Azure AD domenske servise za svoje klijent Azure AD i odabrati da bi bili dostupni u ovu mrežu virtualne domenske servise. Nakon toga Azure servisa Active Directory Domain Services dodjeljuje upravljanih domene i postaje dostupan u virtualne mreže. Sve korisničke račune, članstva u grupi i korisničke vjerodajnice dostupne u klijentu za Azure AD Contoso, dostupne su i u novostvorenu domenu. Ova značajka omogućuje korisnicima u tvrtki ili ustanovi za prijavu na domenu pomoću svoje tvrtke vjerodajnice – na primjer, prilikom povezivanja daljinski domene pridruženo strojeva putem udaljene radne površine. Administratori mogu dodijelite pristup resursima u domeni pomoću postojeće članstva u grupi. Aplikacija implementiran na virtualnim računalima sustava na virtualne mreže pomoću značajke kao što su domene spoj, LDAP čitanje, LDAP vezanja, NTLM i Kerberos provjeru autentičnosti i pravila grupe.

U nekim upadljivim upravljanih domene koja vam je dodijeljena po Azure servisa Active Directory Domain Services se stvarima na sljedeći način:

- Contoso je IT administrator ne morate upravljati, zakrpu ili praćenje te domene ili bilo koju kontrolera domena za tu domenu upravljanog.
- Nema potrebe da biste upravljali replikacije AD za tu domenu. Korisnički računi, članstva u grupi i vjerodajnice iz klijenta za Azure AD Contoso-dostupnih automatski unutar ove upravljanih domene.
- Budući da domenu upravlja Azure AD domenske servise, Contoso-IT administrator imati ovlasti administratora domene ili Administrator u tvrtki u toj domeni.


### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Azure AD domenske servise za hibridno tvrtke ili ustanove
Tvrtke i ustanove s hibridnim IT infrastrukturu zauzimaju kombinacije oblaka resurse i lokalne resurse. Takve tvrtkama ili ustanovama sinkronizirati identiteta iz svoje lokalnog imenika njihove Azure AD klijent. Kako hibridnog tvrtkama ili ustanovama izgledati migriranje više aplikacija njihov lokalnog u oblaku, posebice naslijeđene direktorija umu aplikacije, može biti korisno da biste ih Azure servisa Active Directory Domain Services.

Litware Corporation je postavila [Azure AD Connect](../active-directory/active-directory-aadconnect.md), da biste sinkronizirali identiteta podatke iz njihova lokalnog imenika njihove Azure AD klijent. Informacije o identitetu koja se sinkronizira sadrži korisničke račune, njihove raspršivanja vjerodajnica za provjeru autentičnosti (sinkronizaciju lozinke) i članstva u grupi.

> [AZURE.NOTE] **Sinkronizacija lozinka je obavezna za hibridno tvrtke ili ustanove da biste koristili Azure servisa Active Directory Domain Services**. U ovom potrebna je jer vjerodajnice korisnika potrebna upravljanih domenu nudi Azure servisa Active Directory Domain Services, za provjeru autentičnosti tim korisnicima putem NTLM ili Kerberos metoda provjere autentičnosti.

![Azure AD Domain Services za Litware Corporation](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

Na prethodnoj slici prikazuje način na koji možete tvrtke i ustanove s hibridnim IT infrastrukturu, kao što su Litware Corporation iskoristiti Azure servisa Active Directory Domain Services. Litware, aplikacije i radnih opterećenja poslužitelja koje je potrebno domenske servise su uveden u virtualne mreže servisa Azure Infrastruktura sustava. Litware je IT administrator možete omogućiti Azure AD domenske servise za svoje Azure AD klijent i odlučite koristiti upravljane domene u virtualne mreže. Budući da Litware je tvrtke ili ustanove s hibridnim IT infrastrukturu, korisničke račune, grupe i vjerodajnice se sinkroniziraju njihove klijent Azure AD iz njihove lokalnog imenika. Ova značajka omogućuje korisnicima da biste se prijavili domenu pomoću svoje tvrtke vjerodajnice – na primjer, kada se daljinski povezati strojeva pridruženo domeni putem udaljene radne površine. Administratori mogu dodijelite pristup resursima u domeni pomoću postojeće članstva u grupi. Aplikacija implementiran na virtualnim računalima sustava na virtualne mreže pomoću značajke kao što su domene spoj, LDAP čitanje, LDAP vezanja, NTLM i Kerberos provjeru autentičnosti i pravila grupe.

U nekim upadljivim upravljanih domene koja vam je dodijeljena po Azure servisa Active Directory Domain Services se stvarima na sljedeći način:

- Upravljani domena je samostalni domene. Nije datotečni nastavak Litware korisnika lokalne domene.
- Litware je IT administrator ne treba upravljanje zakrpa, ili praćenje kontrolera domena za tu domenu upravljanog.
- Nema potrebe da biste upravljali replikacije AD za tu domenu. Korisnički računi, članstva u grupi i vjerodajnice iz lokalnog imenika Litware korisnika se sinkroniziraju Azure AD putem Azure AD Connect. Ove korisničke račune, članstva u grupi i vjerodajnice dostupnih automatski unutar upravljanih domene.
- Budući da domenu upravlja Azure AD domenske servise, Litware, IT administrator imati ovlasti administratora domene ili Administrator u tvrtki u toj domeni.


## <a name="benefits"></a>Prednosti
Pomoću servisa Azure AD domene mogli uživati u sljedeće prednosti:

-   **Jednostavan** – možete zadovoljavaju potrebe identiteta virtualnim strojevima implementiran na servisa Azure infrastrukture samo nekoliko klikova jednostavne. Ne morate implementacije i upravljanja infrastrukture identiteta u Azure ili postavljanje natrag veza sa sustavom infrastruktura za identitet lokalnog.

-   Vaš klijent Azure AD Duboko integrirani **integrirane** – Azure servisa Active Directory Domain Services. Sada možete koristiti Azure AD kao imenik integrirani oblaku tvrtke koji caters potrebama vaše Moderna aplikacije i tradicionalni direktorija umu aplikacije.

-   **Kompatibilne** – Azure servisa Active Directory Domain Services se temelji na Infrastruktura ocjene dokazana tvrtke za Windows Server Active Directory. Dakle, možete je aplikacija za veći stupanj kompatibilnosti sa značajkama za Windows Server Active Directory. Sve značajke dostupne u sustavu Windows Server AD su trenutno dostupne u Azure AD domenske servise. Međutim, dostupne značajke su kompatibilni s Windows Server AD značajke koje je oslanjate u infrastruktura za lokalni. Uključivanje mogućnosti LDAP, Kerberos, NTLM, pravila grupe i domene čine starijih nuditi koji testira i poboljšane preko različitih izdanja sustava Windows Server.

-   **Učinkovit** – s Azure servisa Active Directory Domain Services, možete izbjeći infrastrukture i upravljanje teret koji je pridružen upravljanje identiteta infrastrukture za podršku tradicionalni direktorija umu aplikacije. Možete premjestiti te aplikacije Azure infrastrukture servisima i pogodnost veći računanje na radu troškove.
