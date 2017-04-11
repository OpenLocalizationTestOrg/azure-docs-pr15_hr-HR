<properties
    pageTitle="Ograničenja 2.0 krajnjoj točki i ograničenja | Microsoft Azure"
    description="Popis ograničenja i ograničenja s krajnja točka za Azure AD 2.0."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="should-i-use-the-v20-endpoint"></a>Trebam li koristiti krajnju točku 2.0?

Kada stvaranje aplikacija integriranih s Azure Active Directory, morat ćete odlučiti hoće li 2.0 protokoli krajnjoj točki i provjeru autentičnosti ne odgovara vašim potrebama.  Izvorni krajnje Azure AD i dalje u potpunosti podržane, a u nekim običnoj je više značajki obogaćenog od 2.0.  No u 2.0 krajnjoj točki [predstavlja brojne prednosti](active-directory-v2-compare.md) za razvojne inženjere koji mogu potaknuti koristiti novi model programiranja.

Tom trenutku u trenutku, naš preporuke prilikom korištenja krajnje točke 2.0 je na sljedeći način:

- Ako želite da podržava osobni računi za Microsoft u aplikaciji, trebali biste koristiti 2.0 krajnju točku.  No, provjerite jeste li razumijete ograničenja navedeni u ovom članku osobito one vezani uz posebno za rad i školske račune.
- Ako aplikacije potreban je samo podrške tvrtke i školskih računa, trebali biste koristiti [izvorni Azure AD krajnjih točaka](active-directory-developers-guide.md).

S vremenom krajnju točku 2.0 će rasti da biste uklonili ograničenja nalaziti, tako da samo ikad morat ćete koristiti 2.0 krajnju točku.  U aplikacijom ovaj je članak namijenjen da biste utvrdili krajnju točku 2.0 odgovara li vam.  Nastavit ćemo da biste ažurirali u ovom se članku vremenom odražava trenutno stanje krajnju točku 2.0, pa provjeravati reevaluate potrebama odnosu 2.0 mogućnosti.

Ako imate aplikaciju postojeće s Azure AD pomoću krajnju točku 2.0, nema potrebe da biste počeli ispočetka.  U budućnosti ćemo će biti ponuda omogućuje vam da biste omogućili postojeće aplikacije Azure AD za krajnju točku 2.0.

## <a name="restrictions-on-apps"></a>Ograničenja aplikacije
Krajnja točka 2.0 ne trenutno podržava sljedeće vrste aplikacije.  Opis podržane vrste aplikacije, potražite [u ovom članku](active-directory-v2-flows.md).

##### <a name="standalone-web-apis"></a>API-ji Web samostalni
Zahvaljujući krajnju točku 2.0 imate mogućnost da biste [sastavili Web API koji je zaštićen OAuth 2.0](active-directory-v2-flows.md#web-apis).  Međutim, taj API Web samo bit će primati tokena iz aplikacije koja imaju isti aplikacija Id.  Stvaranje API-JA na kojoj se pristupa putem klijenta s različitim Id aplikacije web-mjesto nije podržana.  Taj klijent nećete moći zatraži ili nabavite dozvole za web-API-JA.

Sastavljanje Web API-jem koje prihvaća tokena iz klijenta s istim ID-om aplikacije potražite primjere Web API 2.0 krajnje točke u [Početak rada](active-directory-appmodel-v2-overview.md#getting-started).

##### <a name="web-api-on-behalf-of-flow"></a>Tijek web API-JA na-ime-od
Mnoge arhitekturi obuhvaćaju API Web koji je potrebno da biste pozvali druge do API Web, oba osigurava krajnju točku 2.0.  Taj se scenarij uobičajenih u izvorni klijentskim programima koji imaju Web API pozadinski koja naizmjence poziva Microsoftov internetski servis ili neki drugi prilagođeni ugrađeni API na Webu koji podržava Azure AD.

Ovaj scenarij biti podržane pomoću dodijelite OAuth 2.0 Jwt nošenja vjerodajnica, u suprotnom naziva tijeka On-Behalf-Of.  Međutim, tijek uključeno-ime-od trenutno nisu podržani za krajnju točku 2.0.  Da biste vidjeli kako će se ovaj tijek funkcionira u načelu dostupan Azure AD servisa, pogledajte [uzorak koda za na-ime-od na GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).

## <a name="restrictions-on-app-registrations"></a>Ograničenja Registracija aplikacije
Sve aplikacije koje želite integrirati s krajnju točku 2.0 tom trenutku u trenutku, morate stvoriti novi Registracija aplikacije na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Postojeće Azure AD ili aplikacije Microsoft Account neće biti kompatibilan s krajnju točku 2.0 niti će aplikacija registrira u bilo kojem portal osim na novom portalu Registracija aplikacije.  Plan omogućuje da biste omogućili postojeće aplikacije za korištenje kao 2.0 aplikacije. Trenutno No postoji bez put migracije za krajnju točku 2.0 aplikacije.

Isto tako, aplikacija registrira u novom portalu Registracija aplikacije neće funkcionirati protiv izvorne Azure AD provjere autentičnosti krajnjoj točki.  Međutim, koristite aplikacije stvorene na portalu za registraciju aplikacije da biste uspješno integrirati provjere autentičnosti krajnje račun za Microsoft `https://login.live.com`.

Nadalje, registracije aplikacije stvoren [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) imaju Sljedeća upozorenja:

- Svojstvo **početnoj stranici** , poznata i kao **URL za prijavu** nije podržana.  Bez početne stranice, te aplikacije ne prikazuju se na ploči Office MyApps.
- Samo dva aplikacije tajne dopušteno svaku aplikaciju Id trenutno.
- Za registraciju aplikacije samo može pregledavati i upravlja za razvojne inženjere jedan račun.  Ne mogu koristiti između više razvojnim inženjerima.
- Postoji nekoliko ograničenja na oblik redirect_uri dopuštene.  U odjeljku sljedeće dodatne detalje.

## <a name="restrictions-on-redirect-uris"></a>Ograničenja ji preusmjeravanje
Ograničena ograničeni skup vrijednosti redirect_uri su aplikacije registrirane na portalu za registraciju nove aplikacije.  Redirect_uri za web-aplikacije i servise moraju započinjati sa shemom `https`, i sve redirect_uri vrijednosti mora zajedničko korištenje jedne DNS domene.  Na primjer, nije moguće da biste registrirali web-aplikacije koje sadrži redirect_uris:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Registracija sustava uspoređuje cijeli DNS naziv postojeće redirect_uri s nazivom DNS redirect_uri dodajete. Zahtjev za dodavanje naziva DNS neće uspjeti ako bilo koji od sljedećih uvjeta su ispunjeni:  

- Ako je cijeli DNS naziva nove redirect_uri odgovara nazivu DNS postojeće redirect_uri
- Ako cijeli DNS naziva nove redirect_uri nije poddomenu postojeće redirect_uri

Na primjer, ako aplikacija trenutno sadrži redirect_uri:

`https://login.contoso.com`

Tada je moguće dodati:

`https://login.contoso.com/new`

koji je točno odgovara nazivu DNS-a ili:

`https://new.login.contoso.com`

što je DNS poddomene od login.contoso.com.  Ako želite imati aplikacije koja ima prijava east.contoso.com i prijava west.contoso.com kao redirect_uris, a morate dodati sljedeći redirect_uris redoslijedom:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Potonjem dva mogu se dodati jer su poddomene prvi redirect_uri contoso.com. To ograničenje uklonit će u budućem izdanju.

Da biste saznali kako registrirati aplikaciju na novom portalu Registracija aplikacije, pogledajte [članak](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services--apis"></a>Ograničenja servisa i API-ji
Krajnja točka 2.0 trenutno podržava prijava za bilo koju aplikaciju registrirana nove aplikacije Registracija portalu nudi taj pada na popis [podržanih provjere autentičnosti](active-directory-v2-flows.md)tokova.  Međutim, te aplikacije će moći samo Nabava OAuth 2.0 pristupna tokena vrlo ograničeni skup resursa.  Krajnja točka 2.0 će samo problema access_tokens za:

- Aplikaciju koju ste tražili token.  Aplikaciju možete nabaviti na access_token za odvojeno, ako je logički aplikacija sastoji se od nekoliko različitih komponenata ili razine.  Da biste vidjeli scenarij u praksi, pogledajte naše vodiči za [Početak rada](active-directory-appmodel-v2-overview.md#getting-started) .
- Pošta programa Outlook, kalendar i kontakti REST API-ji, koji se nalaze na https://outlook.office.com.  Da biste saznali kako napisati aplikaciju koja se može pristupiti te API-ji, pogledajte ove vodiči za [Početak rada sustava Office](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) .
- API-ji Microsoft Graph.  Da biste saznali više o web-mjesto Microsoft Graph i u okvir za sve podatke koji je dostupan, posjetite [https://graph.microsoft.io](https://graph.microsoft.io).

Nema servisa podržani su trenutno.  Dodatne Microsoft Online services dodat će se u budućnosti, kao i podršku za vlastite prilagođene ugrađeni Web API-ji i servise.

## <a name="restrictions-on-libraries--sdks"></a>Ograničenja biblioteke i SDK-ovi
Podrška za biblioteke za krajnju točku 2.0 prilično ograničeno je trenutno.  Ako želite koristiti krajnju točku 2.0 u aplikaciji za proizvodnju imate sljedeće mogućnosti:

- Ako izrađujete web-aplikacije, naš načelu dostupan proizvod poslužiteljsko sigurno možete koristiti za izvođenje za prijavu i token provjere valjanosti.  Za naše NodeJS Passport dodatak i ASP.NET uključuju proizvod OWIN Otvori ID povezivanje.  Primjere koda pomoću naš proizvod dostupne su u našem [Prvi koraci u](active-directory-appmodel-v2-overview.md#getting-started) odjeljku kao i.
- Za druge platforme i izvorni i mobilne aplikacije, možete i integrirati s krajnju točku 2.0 po izravno slanje i primanje poruka protokol u kodu aplikacije.  2.0 OpenID povezati i OAuth protokoli [izričito zabilježeni su](active-directory-v2-protocols.md) da biste lakše izvođenje takve integracije.
- Na kraju, Otvori izvor otvorite ID povezati i OAuth biblioteke možete koristiti za integraciju s krajnju točku 2.0.  Protokol 2.0 mora biti kompatibilan s mnogo bibliotekama protokol Otvori izvor bez glavnih promjena.  Dostupnost takve biblioteke razlikuje se po jezika i platforme, a na web-mjesta za [Povezivanje ID -a za otvaranje](http://openid.net/connect/) i [OAuth 2.0](http://oauth.net/2/) održavati popis popularnih implementacije. Da biste vidjeli [Azure Active Directory (AD) 2.0 i provjera autentičnosti biblioteke](active-directory-v2-libraries.md) više pojedinosti i popis biblioteka klijentski Otvori izvor i uzorke koje testirate s krajnju točku 2.0.

Ne možemo imati i objavio početne pretpregled [Microsoft provjera autentičnosti biblioteke (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) za .NET samo.  Vi ste Dobro došli u isprobajte ove biblioteke u .NET postupak klijentske i poslužiteljske aplikacije, ali kao pretpregled biblioteka ga se ne prate tako da GA kvalitete podržava.

## <a name="restrictions-on-protocols"></a>Ograničenja protokola
Krajnja točka 2.0 podržava samo otvaranje povezivanje ID-a i OAuth 2.0.  Međutim, ne svim značajkama i mogućnostima svakog protokola unesu u krajnju točku 2.0, uključujući:

- Povezivanje OpenID `end_session_endpoint`, a time i aplikacije da biste završili sesiju korisnika s krajnju točku 2.0.
- id_tokens izdala krajnju točku 2.0 sadržavati samo para identifikator za korisnika.  To znači da primit će dvije različite aplikacije različite ID-a za istoga korisnika.  Imajte na umu da po ispitivanje Microsoft Graph `/me` krajnje točke, moći da biste dobili correlatable ID korisnika koji se može koristiti u svim aplikacijama.
- ne sadrže id_tokens izdala krajnju točku 2.0 programa `email` zahtjeva za korisnika trenutku, čak i ako nabavljate dozvole korisnika za prikaz e-pošte.
- Krajnja točka OpenID povezivanje podaci o korisniku. Podaci o korisniku krajnje nije implementirana na krajnjoj točki 2.0 trenutno.  Međutim, sve podataka profila korisnika potencijalno zaprimljeni na ovom krajnjoj točki nalazi se u Microsoft Graph `/me` krajnjoj točki.
- Uloge i zahtjevima za grupu.  Trenutačno krajnju točku 2.0 ne podržava izdala ulozi ili grupi zahtjevima u id_tokens.

Da biste bolje razumjeli opseg funkcionalnosti protokol koji su podržane u krajnju točku 2.0, pročitajte naše [OpenID povezivanje i OAuth 2.0 protokol Reference](active-directory-v2-protocols.md).

## <a name="restrictions-for-work--school-accounts"></a>Ograničenja za posao i školu račune
Postoje određene korisnicima tvrtke ili ustanove-tvrtke Microsoft koji još nisu podržani po krajnju točku 2.0 nekoliko značajki.

##### <a name="device-based-conditional-access-native-and-mobile-apps-and-the-microsoft-graph"></a>Pristup uvjetno utemeljen na uređaju, nativnih i mobilne aplikacije i Microsoft Graph
Krajnja točka 2.0 još podržava provjeru autentičnosti uređaj za mobilne uređaje i izvorni aplikacija, kao što je izvorni aplikacije sustavom iOS ili Android.  To može onemogućiti nativni aplikacije iz nazovete Microsoft Graph za neke tvrtke ili ustanove.  Kada administrator postavlja pravilo pristup uvjetno utemeljen na uređaj na aplikaciju, potrebno je provjeriti autentičnost uređaja.  Za krajnju točku 2.0 najvjerojatnije scenarij za pristup uvjetno utemeljen na uređaj je administrator postavljanje pravila na resursa u Microsoft Graph, kao što su Outlook API-JA.  Ako administrator postavlja ovo pravilo, a matičnoj aplikaciji zahtjeve token u Microsoft Graph, zahtjev konačni neće uspjeti jer je provjera autentičnosti uređaj još nije podržana.  Web-aplikacije kojima se traže tokeni u Microsoft Graph, međutim, podržani su kada konfigurirane pravila utemeljen na uređaj.  Na web-mjestu uređaja provjere autentičnosti za aplikaciju scenarij provodi kroz korisnika web-pregledniku.

Kao programer, vjerojatno imate nema kontrole putem pravila postavljene na resurse za Microsoft Graph, ili čak i umu kada se to događa.  Ako izrađujete aplikacije za korisnike tvrtke i obrazovne ustanove, trebali biste koristiti [izvorni krajnje Azure AD](active-directory-developers-guide.md) dok krajnju točku 2.0 podržava provjeru autentičnosti uređaja.  Dodatne informacije o pristup uvjetno utemeljen na uređaju, pogledajte [Ovaj članak](active-directory-conditional-access.md#device-based-conditional-access).

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>Windows integriranu provjeru autentičnosti vanjskim korisnicima
Ako ste iskoristili ADAL (i stoga izvornu krajnje Azure AD) u aplikacijama sustava Windows, možda ste uzima prednost što se naziva pridruživanju grant SAML.  Ova dodjela omogućuje korisnicima pridruženim Azure AD klijenata za tihu provjeru s instancom lokalnog servisa Active Directory bez potrebe za unosom vjerodajnica.  Dodjela pridruživanju SAML trenutno nije podržano na krajnjoj točki 2.0.
