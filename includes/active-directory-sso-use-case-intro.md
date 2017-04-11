Tvrtke i ustanove koriste više [kao Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) aplikacijama za produktivnost jer su alati i tehnologije oblaka postaje još uvijek su na raspolaganju. Kao što je broj SaaS aplikacije poveća, on postaje zahtjevne za administratore za upravljanje računima i pristup prava i za korisnike koji žele Imajte na umu različite lozinke. Pojedinačno upravljanje te aplikacije stvara dodatni posao web-mjesta i manje sigurna.


- Zaposlenici koji imaju da biste pratili mnogo lozinki često koristite metoda manje sigurna njih sjetiti napisati lozinki ili isti lozinkama preko brojem računa.

- Kada pristigne novog zaposlenika ili jedan, sve svoje račune mora biti pojedinačno dodjeli ili njezini će se resursi.

- Uz to, zaposlenici početi koristiti SaaS aplikacije za rade bez prolaze kroz IT, što znači da su stvaranje računa za svoj poštanski sustavima koji INFORMATIČKI administratori niste odobrene nisu nadzor.  

Rješenja za sve te izazove je jedinstvenu prijavu (SSO). Najjednostavniji način za upravljanje aplikacijama više i korisnicima omogućili prijave dosljednost je. Azure Active Directory (Azure AD) nudi robusne SSO rješenje i ima mnoge dostupna unaprijed integrirane aplikacije, s vodiči za administratore da biste brzo postavite novu aplikaciju i započnite dodjelu resursa korisnicima.


## <a name="how-does-azure-active-directory-integrate-apps"></a>Kako Azure Active Directory integrirati aplikacije?  

Azure AD omogućuje integraciju aplikacije i dodijeljenu računi. To možete učiniti putem bilo koju od dva načina.

- Ako je unaprijed integrirane u aplikaciji Galerija aplikaciju, možete proći kroz te portal za postavljanje aplikacija i konfigurirati postavke da biste omogućili SSO. Za bilo koju aplikaciju za galeriju početak rada tako da slijedite jednostavan detaljne upute navedene u galeriju aplikacija, a na portalu za Azure da biste omogućili jedinstvenu prijavu.

- Ako aplikacija nije u galeriji, možete i dalje postaviti Većina aplikacije u Azure AD kao prilagođene aplikacije. Potreban je malo više Stručne osobe da biste konfigurirali. Možete dodati bilo koju aplikaciju koja podržava SAML 2.0 kao pridruženi aplikacije ili bilo koju aplikaciju koja sadrži programa koji se temelji na HTML stranica za prijavu u obliku SSO aplikacije lozinku.

U slučaju gdje korisnici ste stvorili svoje račune za SaaS aplikacije koje nisu upravlja IT, alat za [Otkrivanje aplikacije oblak](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) nudi rješenje. Ovaj alat nadzire promet web da biste odredili kojim aplikacijama koristiti u cijeloj tvrtki ili ustanovi i broj osobama koje koriste svaku od njih. IT te podatke možete koristiti da biste saznali koje aplikacije korisnika radije i odlučiti integrirati u Azure AD za SSO.  

Kada se integrira aplikacije u Azure AD, možete mapirati identiteti korisnika utvrđene aplikacije njima Azure AD identiteta.  
