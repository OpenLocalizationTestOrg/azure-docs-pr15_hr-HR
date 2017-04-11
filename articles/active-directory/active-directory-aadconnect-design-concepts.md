<properties
   pageTitle="Azure AD Connect: Dizajniranje koncepata | Microsoft Azure"
   description="U ovoj se temi detalje o određenih područja dizajn implementacije"
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.custom = "azure-ad-connect"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/13/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Konceptima dizajna
Je svrha u ovoj se temi opisuje područja koja morate mislili tijekom implementacije dizajna Azure AD Connect. U ovoj se temi je precizno dive na određena područja i koncepte su ukratko opisani u druge teme.

## <a name="sourceanchor"></a>sourceAnchor
Atribut sourceAnchor se definira kao *atribut immutable tijekom života objekta*. Jedinstveno označava objekt kao na isti objekt lokalnog i Azure AD. Atribut se zove **immutableId** i dva imena koriste resursa.

Word immutable, koji je "ne može se mijenjati", važno je da u ovoj se temi. Budući da vrijednost toga atributa nije moguće promijeniti nakon što je postavljena, važno je da biste odabrali dizajn koji podržava scenariju.

Atribut koristi se za sljedeće scenarije:

- Kad novi poslužitelj za sinkronizaciju modula ugrađen ili izgraditi nakon oporavka scenarij Izrada, taj atribut povezuje postojećih objekata u Azure AD objekata lokalnog sustava.
- Ako premještate iz samo oblak identiteta sinkronizirane identiteta model, taj atribut Azure AD s objektima na lokalni omogućuje objekte s "teško podudaranje" postojeće objektima.
- Ako koristite vanjski pristup, zatim taj atribut zajedno s **userPrincipalName** se koristi u zahtjev za identificirati samo korisnika.

U ovoj se temi govori samo o sourceAnchor kao što je to povezano s korisnicima. Ista pravila Primijeni na sve vrste objekata, ali je samo za korisnike ovaj problem se obično je problem.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Odabir dobro sourceAnchor atributa
Vrijednost atributa moraju pratiti sljedeća pravila:

- Biti manje od 60 znakova
    - Šifrirana i broji kao 3 znamenke znakove koji se neće se a z, A do Z ili 0-9
- Ne sadrže posebnih znakova: & #92;! # $ % & * + / = ? ^ & #96; { } | ~ (< >) '; : , [ ] " @ _
- Morate biti globalni jedinstveni
- Mora biti u nizu, cijeli broj ili binarni.
- Nije se temelji na korisničko ime, te promjene
- Trebali biste velika i mala slova i izbjegli vrijednosti koje se mogu razlikovati po predmetu
- Trebaju biti dodijeljeni stvaranja objekta

Ako odabrani sourceAnchor nije vrste niz, pa Base64Encode povezivanje Azure AD vrijednost atributa da biste bili sigurni ne posebni znakovi prikazuju se. Ako koristite neki drugi vanjskom poslužitelju od ADFS, provjerite je li vaš poslužitelj, možete Base64Encode atribut.

Atribut sourceAnchor je velika i mala slova. Vrijednost "JohnDoe" nije isti kao "johndoe". No ne bi smio imati dvije različite objekte samo razlika velikim slovom.

Ako jedan skup stabala lokalnog, zatim atribut trebali biste koristiti je **objectGUID**. To je atribut koji se koristi pri korištenju eksplicitnih postavke u Azure AD Connect i atributa koji se koriste DirSync.

Ako imate više šuma i premještanje korisnika između šuma i domene, **objectGUID** je dobro atribut da biste koristili čak i u ovom slučaju.

Ako korisnici prebacivanje šuma i domene, zatim morate pronaći atribut koje se mijenjaju ili može premjestiti s korisnicima tijekom premještanje. Preporučeni način je predstavljanje stilova sintetičkih atribut. Atribut koji nije držite nešto koji izgleda kao GUID bio prikladan. Tijekom stvaranja objekta novi GUID se stvara i oznaku na korisnike. Pravilo za prilagođenu sinkronizacije moguće je na poslužitelju modul sinkronizaciju da biste stvorili tu vrijednost na temelju **objectGUID** i ažurirajte atribut odabrane ZBRAJA. Prilikom premještanja objekta, provjerite je li i kopirati sadržaj tu vrijednost.

Drugo rješenje je da biste odabrali atribut postojeće znate da se ne mijenja. Najčešće korištenih atribute obuhvaćaju **IDZaposlenika**. Ako smatrate da se atribut koji sadrži slova, provjerite je li nema mogućnost kutije (velika i mala slova) možete promijeniti na atributa vrijednosti. Neispravni atributa koji će se koristiti dodavanje tih atributa uz ime korisnika. U braka ili divorce, naziv se očekuje da biste promijenili, koji nije dopušten za taj atribut. To je jedan od razloga zašto atributi kao što su **userPrincipalName**, **pošta**i **targetAddress** nisu čak i moguće odabrati u čarobnjaku za instalaciju Azure AD Connect. Te atribute sadržavati i u @-character, što je dopušteno u sourceAnchor.

### <a name="changing-the-sourceanchor-attribute"></a>Promjena sourceAnchor atributa
Vrijednost atributa sourceAnchor nije moguće promijeniti nakon objekt je stvorena u Azure AD i sinkronizira identitet.

Zbog toga sljedeća ograničenja primjenjuju na Azure AD Connect:

- Atribut sourceAnchor se mogu postaviti samo tijekom početne instalacije. Ako ponovno pokrenite čarobnjak za instalaciju, ta je mogućnost samo za čitanje. Ako vam je potrebna da biste promijenili tu postavku, morate deinstalirati i ponovno instalirajte.
- Ako instalirate drugi poslužitelj za Azure AD Connect, morate odabrati atribut sourceAnchor isti kao koristili. Ako ste prethodno koristili DirSync i premjestite Azure AD Connect, zatim morate koristiti **objectGUID** Budući da se atribut koristi DirSync.
- Ako je vrijednost za sourceAnchor mijenjano po objekt izvezena je za Azure AD, zatim Azure AD Connect sinkronizacija throws pogrešku i ne dopuštaju dodatne promjene na objekt prije nego što je problem riješen i na sourceAnchor promijeniti natrag u imeniku izvora.

## <a name="azure-ad-sign-in"></a>Azure AD prijavu
Tijekom integriranje lokalnog imenika s Azure AD, važno je znati kako postavke sinkronizacije utječu na način korisnik potvrđuje. Azure AD koristi userPrincipalName (UPN) radi provjere autentičnosti korisnika. No kada sinkronizirate korisnike, morate odabrati atribut koja će se koristiti za vrijednost parametra userPrincipalName pažljivo.

### <a name="choosing-the-attribute-for-userprincipalname"></a>Odabir atribut userPrincipalName
Kada odaberete atribut za dohvat trebali biste provjerite vrijednost UPN koja će se koristiti u Azure

- Atribut vrijednosti u skladu sa sintaksa UPN-a (RFC 822), koji je mora biti u oblikuusername@domain
- Sufiks vrijednosti odgovara na neki od provjerene prilagođene domene u Azure AD

U odjeljku eksplicitnih postavke pretpostavljenom izbor za atribut je userPrincipalName. Ako atribut userPrincipalName ne sadrži vrijednost želite da korisnici da biste se prijavili Azure, a zatim morate odabrati **Prilagođenu instalaciju**.

### <a name="custom-domain-state-and-upn"></a>Stanje prilagođenu domenu i UPN-a
Važno je da provjerite postoji li provjerene domene za UPN nastavka.

Nevena je korisnik contoso.com. Želite Nevena da biste koristili lokalnog UPN-a john@contoso.com da biste se prijavili Azure nakon koje korisnici sinkronizirali svoje contoso.onmicrosoft.com Azure AD direktorija. Da biste to učinili, morate Dodavanje i provjeru contoso.com kao prilagođenu domenu u Azure AD prije no što počnete sinkroniziranje korisnika. Ako UPN nastavka od Nevena, primjerice contoso.com ne odgovaraju provjerene domene Azure AD, zatim Azure AD zamjenjuje UPN nastavka contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Osobe koje nisu usmjeravati lokalne domene i UPN-a za Azure AD
Neke tvrtke i ustanove imaju koje nisu usmjeravati domene, kao što su contoso.local ili jednostavni Jedna naljepnica domene kao što je contoso. Niste moći potvrda domene koje nisu usmjeravati u Azure AD. Azure AD Connect možete sinkronizirati samo provjerene domene u Azure AD. Prilikom stvaranja u imeniku Azure AD stvara usmjeravati domene koja postaje zadanu domenu za Azure AD, na primjer, contoso.onmicrosoft.com. Dakle, on postaje potrebne da biste potvrdili druge usmjeravati domenu u takvim scenarija u slučaju da ne želite da biste sinkronizirali zadanu domenu onmicrosoft.com.

Dodatne informacije o dodavanju i potvrđivanja domene pročitajte [dodajte prilagođeni naziv domene Azure Active Directory](active-directory-add-domain.md) .

Azure AD Connect otkriva ako koristite u okruženju koje nisu usmjeravati domene i želite komponenta vas upozoriti s unaprijed eksplicitnih postavke. Ako radite na koje nisu usmjeravati domena, tada je vjerojatno da imaju UPN-a korisnika koji nisu usmjeravati nastavke previše. Na primjer, ako su pokrenuti u odjeljku contoso.local, Azure AD Connect predlaže korištenje prilagođenih postavki umjesto korištenja eksplicitnih postavke. Pomoću prilagođenih postavki vam se da biste odredili atribut koji želite koristiti kao UPN-a da biste se prijavili Azure kada korisnici su sinkronizirani s Azure AD.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
