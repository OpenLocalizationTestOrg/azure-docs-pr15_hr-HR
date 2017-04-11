<properties
    pageTitle="Azure AD Connect sinkronizacije: kako se mijenjaju uz zadanu konfiguraciju | Microsoft Azure"
    description="Vodit će vas kroz promjene konfiguracije sinkronizirano Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Azure AD Connect sinkronizacije: kako se mijenjaju uz zadanu konfiguraciju
Svrha u ovoj se temi je vodi kroz da biste promijenili zadanu konfiguraciju u Azure AD Connect sinkronizaciju. Sadrži korake za neke uobičajeni scenariji. Uz to znanje mora biti u nekim jednostavne promjene vlastitu konfiguraciju na temelju pravila tvrtke.

## <a name="synchronization-rules-editor"></a>Uređivanje pravila sinkronizacije
Uređivaču pravila sinkronizacije koristi se za prikaz i mijenjanje Zadana konfiguracija. Možete je pronaći na izborniku Start u grupi **Azure AD Connect** .  
![Izbornik Start s uređivač za sinkronizaciju pravila](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Kad ga otvorite, vidjet ćete – okvir zadana pravila.

![Uređivač pravila za sinkronizaciju](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Navigacija u uređivaču
Padajuće izbornike pri vrhu uređivača omogućuju da biste brzo pronašli određenu pravilo. Na primjer, ako želite da biste vidjeli pravila kojima se atributa proxyAddresses uključen, promijeniti padajuće izbornike ovako:  
![Filtriranje SRE](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Da biste ponovno postaviti filtriranje i učitavanje Osvježi konfiguracije, pritisnite **F5** na tipkovnici.

Da biste u gornjem desnom kutu nalazi se gumb **Dodaj novo pravilo**. Ovaj gumb se koristi za stvaranje prilagođenog pravila.

Pri dnu imate gumbe za djeluje sinkronizaciju odabranog pravila. **Uređivanje** i **Brisanje** to što očekivati da. **Izvoz** daje skriptu PowerShell za vraćanju pravilo za sinkronizaciju. Ovaj postupak omogućuje premjestite pravilo sinkronizaciju s jednog poslužitelja na drugi.

## <a name="create-your-first-custom-rule"></a>Stvaranje prve prilagođenog pravila
Najčešći promjena je promjene tokova atribut. Podaci u direktoriju izvor možda neće biti kao Azure AD. U primjeru u ovom odjeljku, želite provjerite je li navedenim ime korisnika uvijek napisana **početnim velikim slovom**.

### <a name="disable-the-scheduler"></a>Onemogućit
Po zadanom [scheduler](active-directory-aadconnectsync-feature-scheduler.md) se pokreće svakih 30 minuta. Želite da biste bili sigurni da se pokreće dok mijenjate i otklanjanje poteškoća s nova pravila. Da biste privremeno onemogućit, pokrenite PowerShell i pokretanje`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Onemogućit](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Stvaranje pravila

1. Kliknite **Dodaj novo pravilo**.
2. Na stranici **Opis** unesite sljedeće:  
![Ulazna pravila filtriranja](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
    - Naziv: Dajte pravilo opisni naziv.
    - Opis: Neka upit tako da se netko drugi mogu razumjeti što pravilo je.
    - Povezani sustava: objekt nalazi se u sustav. U ovom slučaju ne možemo odaberite poveznik za Active Directory.
    - Povezani sustava/Metaverse objekt vrsta: Odaberite **korisnika** i **osoba** odnosno.
    - Vrsta veze: Promijenite ovu vrijednost da biste se **pridružili**.
    - Prioritet: Unesite vrijednost koja je jedinstvena u sustavu. Lower numeričku vrijednost označava veći prioritet.
    - Oznaka: Ostavite praznim. Samo – okvir pravila od Microsofta moraju imati taj okvir popuniti vrijednosti.
3. Na stranici **Scoping filtar** unesite **givenName ISNOTNULL**.  
![Ulazna pravila određivanje opsega filtra](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
U ovom se odjeljku koristi se za definiranje koje objekte trebao primijeniti pravilo. Ako je prazno, pravilo će Primijeni na sve objekte korisnika. No koji će se uključiti sobe za sastanke, računa servisa i druge objekte koji nisu osobe korisnika.
4. Na na **Uključivanje pravila**, ga ostavite prazan.
5. Na stranici **transformacije** promijeniti u FlowType **izraz**. Odaberite ciljni atribut **givenName**i u izvoru unesite `PCase([givenName])`.
![Ulazna pravila transformacije](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
Modul za sinkronizaciju je velika i mala slova i na naziv funkcije i naziv atributa. Ako upišete nešto što pogrešan, vidjet ćete upozorenje kada dodate pravilo. Uređivač omogućuje spremanje i nastavak, tako da morate ponovno otvorite pravilo i ispravljanje pravilo.
6. Kliknite **Dodaj** da biste spremili pravilo.

Novo pravilo za prilagođenu mora biti vidljivi s drugim pravilima sinkronizacije u sustavu.

### <a name="verify-the-change"></a>Provjerite je li promjena
Ove nove promjene koje želite da biste provjerili funkcioniraju kako želite i da bi se prijavi sve pogreške. Ovisno o broju objekata imate, postoje dva načina u ovom koraku.

1. Pokretanje Potpuna sinkronizacija na svim objektima
2. Pretpregled i punu sinkronizaciju se izvoditi na jedan objekt

Pokrenite **Servis za sinkronizaciju** s izbornika start. Koraci u ovom odjeljku su sva ovaj alat.

1. **Potpuna sinkronizacija na svim objektima**  
Pri vrhu odaberite **poveznike** . Prepoznavanje poveznik unesene promjene u prethodnom odjeljku, u ovom slučaju Active Directory Domain Services i odaberite ga. Odaberite **Pokreni** od akcija, a zatim odaberite **Potpune sinkronizacije** i **u redu**.
![Potpuna sinkronizacije](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
Na metaverse sada ažurirati objekte. Sada želite pogledati objekt u na metaverse.

2. **Pretpregled i punu sinkronizaciju na jedan objekt**  
Pri vrhu odaberite **poveznike** . Prepoznavanje poveznik unesene promjene u prethodnom odjeljku, u ovom slučaju Active Directory Domain Services i odaberite ga. Odaberite **prostor poveznik za pretraživanje**. Opseg pomoću objekta koji želite koristiti da biste testirali promjene. Odaberite objekt, a zatim kliknite **Pretpregled**. Na zaslonu novo odaberite **Primjenu pretpregled**.
![Izvršavanje pretpregled](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
Promjena pridaje sada u metaverse.

**Pogledajte objekt u na metaverse**  
Želite da biste odabrali nekoliko objekata uzorka da biste bili sigurni da se očekuje vrijednost i primijeniti pravilo. Odaberite **Metaverse pretraživanje** na vrhu. Dodajte filtar morate pronaći relevantne objekte. Rezultat pretraživanja, otvorite objekt. Pogledajte vrijednosti atributa i provjeriti i u stupcu **Pravila za sinkronizaciju** pravilo primjenjuje kao očekivani.  
![Pretraživanje Metaverse](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  
### <a name="enable-the-scheduler"></a>Omogućivanje zakazivanje osvježavanja
Ako je sve prema očekivanjima, možete ponovno omogućiti na raspored. Iz komponente PowerShell pokrenite `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Ostale uobičajene promjene tijek atributa
U prethodnom odjeljku opisano kako da biste unijeli promjene u tijeku atribut. U ovom odjeljku navedeni su dodatne primjere. Upute za stvaranje pravila za sinkronizaciju se skraćeno, ali puni korake možete pronaći u prethodnom odjeljku.

### <a name="use-another-attribute-than-the-default"></a>Koristite drugi atribut umjesto zadanog
Pri Fabrikam, postoji skupa stabala je za dano ime, prezime i zaslonsko ime na kojoj se koristi lokalni abecede. Predstavljanje latinični znak od tih atributa moguće je pronaći u atribute kućni broj. Kada stvaranje globalnog popisa adresa u Azure AD i Office 365 tvrtke ili ustanove želi te atribute koje će se koristiti umjesto toga.

Uz zadanu konfiguraciju objekt iz lokalne skupa stabala izgleda ovako:  
![Atribut tijek 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Da biste stvorili pravilo drugih tokova atribut, učinite sljedeće:

- Pokrenuti **Sinkronizaciju pravilo Editor** na izborniku start.
- **Ulazni** odabran ulijevo, kliknite gumb **Dodaj novo pravilo**.
- Pravilo dajte naziv i opis. Odaberite lokalnog servisa Active Directory i vrste odgovarajući objekata.  U odjeljku **Vrsta veze**odaberite **Pridruži se**. Za prednost, odaberite broj koji se koristi drugo pravilo. Pravila – okvir počinju 100, tako da vrijednost 50 mogu se koristiti u ovom primjeru.
![Atribut tijek 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
- Opseg ostavite prazno (to jest, primjene na sve objekte korisnika u skup stabala).
- Uključivanje pravila ostavite prazno (to jest, omogućuju i pravila – okvir rukovati bilo koji spoj).
- U transformacije, stvorite sljedeće tokova:  
![Atribut tijek 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
- Kliknite **Dodaj** da biste spremili pravilo.
- Otvorite **Upravitelj servisa za sinkronizaciju**. Na **poveznika**, odaberite poveznik gdje dodali smo pravilo. Odaberite **Pokreni**i **potpune sinkronizacije**. Potpune sinkronizacije ponovno izračunava sve objekte pomoću trenutnog pravila.

Ovo je rezultat za na isti objekt s ovom prilagođenog pravila:  
![Atribut tijek 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Duljina atributa
Niz atributi su po zadanom postavljena vrstu i maksimalna duljina iznosi 448 znakova. Ako se radi o atribute niz koji se mogu sadržavati više, zatim provjerite jeste li uključili sljedeće u tijeku atribut:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>Promjena u userPrincipalSuffix
Atribut userPrincipalName u servisu Active Directory uvijek nije poznat korisnici i možda neće biti odgovarajuću kao prijavu. Azure AD Connect Čarobnjak za instalaciju sinkronizaciju omogućuje izdvajanje različitih atribut, primjerice e-poštu. No u nekim slučajevima morate izračunati atribut. Na primjer, tvrtke Contoso ima dvije Azure AD direktorija, jedan za proizvodnju i jedan za testiranje. Žele da korisnici svoje klijentu test za korištenje drugog sufiks u prijavu.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

U ovaj izraz potrajati sve lijevo od prvog @-sign (Word) i concatenate fixed nizom.

### <a name="convert-a-multi-value-to-a-single-value"></a>Pretvaranje s više vrijednosti u jednu vrijednost
Neki atributi u servisu Active Directory su s više vrijednosti u shemi iako izgledaju jedne vrijednosti u Active Directory korisnicima i računalima. Primjer je atribut opis.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

U ovaj izraz za slučaj da atribut ima vrijednost, ne možemo vođenje prvu stavku (stavka) u atribut, uklonite suvišne razmake (Trim) i i zatim čuvati najprije 448 znakova (lijevo) u nizu.

### <a name="do-not-flow-an-attribute"></a>Tijek atribut
Pozadina scenarij za ovaj odjeljak, potražite u članku [Upravljanje tijek postupka atribut](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Preljeva atribut na dva načina. Prvi je dostupan u čarobnjaku za instalaciju i omogućuje vam da biste [uklonili odabrane atribute](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Ta mogućnost funkcionira ako nikad ne sinkronizira atribut prije. Međutim, ako ste pokrenuli taj atribut i kasnije ga ukloniti s tom značajkom, zatim tabulatora modul sinkronizaciju upravljanje atribut i postojećih vrijednosti ostaju u Azure AD.

Ako želite da biste uklonili vrijednost atributa i provjerite je li se u budućnosti preljeva, morate stvaranje prilagođenog pravila.

Pri Fabrikam, ne možemo ste Rač da neke od atributa ne možemo sinkronizirati s oblakom ne smije biti tamo. Želimo da biste bili sigurni tih atributa uklonjeni su iz Azure AD.  
![Neispravni proširenje atributa](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

- Stvaranje novog pravila ulazne sinkronizacije i popunjavanje opis ![opisa](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
- Stvaranje atributa tokova vrste **izraza** i s izvorišnog web-mjesta **AuthoritativeNull**. Slovni **AuthoritativeNull** označava da vrijednost mora biti prazna u na MV čak i ako pravilo za sinkronizaciju donjem prednost pokušava popunjava vrijednost.
![Transformaciju za nastavak atributa](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
- Spremite pravilo za sinkronizaciju. Pokretanje **Sinkronizacije servisa**, pronađite poveznik, odaberite **Pokreni**i **Potpune sinkronizacije**. Ovaj korak ponovno izračunava sve atribut tokova.
- Provjerite jesu li svrhu promjene želite izvesti pretraživanjem prostora poveznik.
![Postupne Izbriši](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o model konfiguraciju u [Razumijevanje deklarativno dodjele resursa](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Saznajte više o jeziku izraz u [Razumijevanje deklarativno izraza dodjele resursa](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Pregled tema**

- [Azure AD Connect sinkronizacije: razumijevanje i Prilagodba sinkronizacije](active-directory-aadconnectsync-whatis.md)
- [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
