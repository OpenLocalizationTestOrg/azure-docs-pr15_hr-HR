<properties
    pageTitle="Azure AD Connect sinkronizacije: objašnjenje deklarativno dodjele resursa | Microsoft Azure"
    description="U članku se objašnjava deklarativno konfiguracije model dodjele resursa u Azure AD Connect."
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
    ms.date="08/29/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect sinkronizacije: objašnjenje deklarativno dodjele resursa
U ovoj se temi objašnjava konfiguriranje modela u Azure AD Connect. Model zove deklarativno dodjele resursa i omogućuje vam da biste promijenili s lakoćom. Mnogo je toga što je opisano u ovoj temi Napredno, a nisu potrebni za većinu scenarija.

## <a name="overview"></a>Pregled
Deklarativno dodjeljivanje obrađuje objekata koji dolaze iz izvora povezanih direktorij web-mjesta i određuje načina objekta i atributa mora biti transformacije iz izvora u ciljni. Objekt odvija se u sinkronizaciju kanal i kanal jednak je za ulazni i izlazni pravila. Ulazna pravila je poslao poveznik prostora na metaverse i izlazni pravilo je poslao u metaverse poveznik prostor.

![Sinkronizacija kanal](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

Kanal ima nekoliko različitih module. Svaki od njih je zadužen za jedan pojam u sinkronizaciji objekta.

![Sinkronizacija kanal](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

- Izvor, izvorišni objekt
- [Opseg](#scope)pronalazi sva pravila za sinkronizaciju koji su u opsegu
- [Uključivanje](#join), Determines odnos između poveznik prostora i metaverse
- [Pretvaranje](#transform), izračunava kako treba transformacije atributi i tijeka
- [Prednost](#precedence)razrješava sukobljenih atributa prilozima
- Cilj, ciljnog objekta

## <a name="scope"></a>Opseg
Modul za opseg je procjene objekta i određuje pravila koja se nalazi u opsega i želite uvrstiti u obrade. Ovisno o vrijednosti atributa na objektu, različite sinkronizaciju pravila vrednuju se da je u opsegu. Ako, na primjer, onemogućene korisnik s nema poštanskog sandučića sustava Exchange se različitih pravila od omogućeno korisnika s poštanskim sandučićem web.  
![Opseg](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

Opseg se definira kao grupe i rečenice. Stavke su unutar grupe. Logički i koristi se između svih uvjeta u grupi. Na primjer, (odjel = IT i država = Danska). Koristi logičkim ili između grupa.

![Opseg](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
Opseg na ovoj slici trebaju biti (odjel = IT i država = Danska) ili (država = Švedska). Ako je grupa 1 ili 2 grupa je vrednovati kao true, a zatim pravilo je opseg.

Modul za opseg podržava sljedeće postupke.

Postupak | Opis
--- | ---
JEDNAKO, NOTEQUAL | Usporedba niz koji se procjenjuje kao ako je vrijednost jednaka vrijednosti iz atributa. S većim brojem vrijednosti atributa potražite u članku ISIN i ISNOTIN.
LESSTHAN, LESSTHAN_OR_EQUAL | Usporedba niz koji se procjenjuje kao ako je vrijednost less than vrijednosti iz atributa.
SADRŽI, NOTCONTAINS | Usporedba niz koji se procjenjuje kao ako je vrijednost pronaći ćete negdje unutar vrijednosti iz atributa.
STARTSWITH, NOTSTARTSWITH | Usporedba niz koji se procjenjuje kao ako je vrijednost na početku vrijednosti iz atributa.
ENDSWITH, NOTENDSWITH | Usporedba niz koji se procjenjuje kao ako je vrijednost na kraju vrijednosti iz atributa.
GREATERTHAN, GREATERTHAN_OR_EQUAL | Usporedba niz koji se procjenjuje kao ako je vrijednost veća od vrijednosti iz atributa.
ISNULL, ISNOTNULL | Procjenjuje ako je atribut nema iz objekta. Ako atribut nije izlaganje i stoga null, zatim pravilo je u opsegu.
ISIN, ISNOTIN | Procjenjuje ako se u atribut definirani vrijednost. Ovaj postupak je s većim brojem vrijednosti varijacije jednakosti i NOTEQUAL. Atribut bi trebao biti atribut s više vrijednosti, a vrijednost se može pronaći u bilo kojem od vrijednosti atributa, tada pravilo je u opsegu.
ISBITSET, ISNOTBITSET | Procjenjuje ako je postavljen određeni bitne. Na primjer, mogu se vrednuju bitova u kontrola korisničkih računa da biste vidjeli korisnika omogućiti ili onemogućiti.
ISMEMBEROF, ISNOTMEMBEROF | Vrijednost mora sadržavati DN u grupu u prostoru poveznik. Ako je objekt član grupe naveden, pravilo je opseg.

## <a name="join"></a>Uključivanje
Modul za uključivanje u kanalu sinkronizaciju je zadužen za pronalaženje odnos između objekta u izvoru i objekta u ciljnom. Ulazna pravila, taj odnos bi objekta u razmak poveznik za pronalaženje odnosa na objekt u na metaverse.  
![Uključivanje između cs i mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
Cilj je da biste vidjeli ako je objekt već metaverse, stvorio drugi poveznika mora biti pridružena. Ako, na primjer, u skupa stabala se računa – resursa korisnika iz skupa stabala račun trebala bi biti spojena s korisnikom iz skupa stabala resursa.

Spojevi se koriste uglavnom ulazna pravila za spajanje poveznik prostora objekata na isti objekt metaverse.

Spojeva definiraju se kao jednu ili više grupa. Unutar grupe, imate rečenice. Logički i koristi se između svih uvjeta u grupi. Koristi logičkim ili između grupa. Grupa se obrađuju navedenim redoslijedom od vrha do dna. Kada jedna grupa je pronašao točno jednog podudaranja s objektom u ciljnom, vrednuje se nijedno pravilo spoja. Ako je nula ili više od jednog objekta nalazi, obrada i dalje sljedeće grupe pravila. Zbog toga pravila mora biti stvoren obliku najčešće eksplicitnih prvog i više mutno na kraju.  
![Uključivanje definition](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
Spojevi na ovoj slici se obrađuju od vrha do dna. Najprije kanal sinkronizaciju vidi ako postoji podudaranja IDZaposlenika. Ako nije, drugo pravilo vidi ako se naziv računa možete koristiti za spajanje objekte. Ako to nije podudaranje ili, pravilo treći i konačni je više mutno podudaranje pomoću ime korisnika.

Ako analizira sva pravila spoj, a ne postoji jedan rezultat, koristit će se **Vrsta veze** na stranici **Opis** . Ako ta mogućnost postavljena za **dodjelu resursa**, stvara se novi objekt u ciljnom.  
![Dodjela resursa ili spoj](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Objekt moraju imati samo jedno pravilo jedan sinkronizaciju s pravilima za uključivanje u opsegu. Ako postoji više pravila za sinkronizaciju gdje je definiran spoj, javlja se pogreška. Prednost se ne koriste za rješavanje sukoba spoja. Objekt mora imati pravilo spoj opseg za atribute tijeka s istom dolazni/odlazni smjeru. Ako vam je potrebna atribute tijeka ulazni i izlazni na isti objekt, morate imati na dolazni i pravilo izlaznog sinkronizaciju s spoja.

Uključivanje izlaznog sadrži posebne ponašanje prilikom Dodjela objekta u ciljnom poveznik prostor. Atribut DN koristi se za prvo pokušajte spoj obrnuto. Ako već postoji objekt u prostoru poveznik cilj s istom DN, spojeni na objekte.

Modul za uključivanje samo vrednuje kada kad novo pravilo za sinkronizaciju stupa na opseg. Kada se pridružite sadrži neki objekt, ga se ne disjoining čak i ako se kriterij uključite više zadovoljeni. Ako želite disjoin objekta sinkronizaciju pravilo koje je pridruženo objekte morate otići iz opseg.

### <a name="metaverse-delete"></a>Brisanje Metaverse
Objekt programa metaverse će ostati nepromijenjena dugo tamo jedno pravilo za sinkronizaciju u opsegu s **Vrstom veze** postavite **dodjele resursa** ili **StickyJoin**. Na StickyJoin koristi se kada poveznik nije dopuštena Dodjela novi objekt da biste na metaverse, ali kada ga je pridruženo, on mora biti izbrisan u izvoru prije brisanja objekta metaverse.

Nakon brisanja metaverse objekt sve objekte koji su povezane s pravilo izlaznog sinkronizaciju označene za **Dodjeljivanje** označena za na Izbriši.

## <a name="transformations"></a>Transformacije
Pretvorbe koriste se za definiranje kako atribute treba teče iz izvora cilj. Na tokove može imati jedan od sljedećih **Vrsta tijek**: izravno, konstanta ili izraz. Izravni protok teče atributa vrijednosti kao-s bez dodatne transformacije. Konstantna vrijednost koja određuje određenu vrijednost. Izraz koristi deklarativno jezik za dodjelu resursa izraz da biste izrazili kako treba transformaciju. Detalje o jeziku izraza pronaći ćete u članku [objašnjenje deklarativno dodjele resursa izraz jezik](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) .

![Dodjela resursa ili spoj](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

Potvrdni okvir **Primjena puta** definira da atribut mora biti postavljeno samo pri stvaranju objekt. Na primjer, tu konfiguraciju mogu se početni lozinke za novi korisnik objekt.

### <a name="merging-attribute-values"></a>Spajanje vrijednosti atributa
U tokova atribut postoji postavke da biste odredili ako se s većim brojem vrijednosti atributa spojiti iz nekoliko različitih poveznike. Zadana vrijednost je **Ažuriranje**koje označava treba li osvajaju pravilo za sinkronizaciju s najvećim prioritetom.

![Spajanje vrste](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

Postoji i **Spajanje** i **MergeCaseInsensitive**. Ove mogućnosti omogućuju da biste spojili vrijednosti iz različitih izvora. Ako, na primjer, može se koristiti da biste spojili atribut član ili proxyAddresses iz nekoliko različitih šuma. Kada koristite tu mogućnost, sva pravila za sinkronizaciju u opsegu za objekt morate koristiti istu vrstu spajanja. Ne možete definirati **ažuriranja** iz jednog poveznika i **Spajanje** od druge. Ako pokušate, primite poruku o pogrešci.

Razlika između **Spajanje** i **MergeCaseInsensitive** je način obrade atribut duplicirane vrijednosti. Modul za sinkronizaciju provjerava je li dvostruke vrijednosti su umetnuta u ciljni atribut. S **MergeCaseInsensitive**duplicirane vrijednosti s samo razlike u slučaju da su namjeravate postojati. Na primjer, oba treba ne vidite "SMTP:bob@contoso.com" i "smtp:bob@contoso.com" u ciljni atribut. **Spajanje** samo pogled na točno vrijednosti i više samo je razlika u slučaj mogu postojati.

Mogućnost **Zamjena** je isti kao **ažuriranja**, ali se ne koristi.

### <a name="control-the-attribute-flow-process"></a>Kontrola tijek postupka atributa
Kada više pravila ulazne sinkronizaciju konfigurirani tako da biste slali isti atribut metaverse, prednost je se koristi za određivanje pića. Da biste slali vrijednost će se pravilo za sinkronizaciju s najvećim prioritetom (najmanju numeričku vrijednost). Isti se događa za izlazni pravila. Sinkroniziranje s najviših ima prednost prvenstva pravila i sudjelovati u radu vrijednost povezanog direktorij.

U nekim slučajevima, umjesto doprinos vrijednosti, pravila za sinkronizaciju morate utvrditi treba ponašanje drugih pravila. Postoje neke posebne literala koristi za taj slučaj.

Za ulazni pravila za sinkronizaciju na slovni **NULL** može se koristiti da biste naznačili da tijeka ne sadrži vrijednost da biste slali. Drugo pravilo s donjem prednost mogu pridonijeti vrijednost. Ako se nijedno pravilo pridonio vrijednost, uklanja se atribut metaverse. Izlazni pravilo, ako je **NULL** Završna vrijednost nakon obrade sva pravila za sinkronizaciju pa vrijednost uklanja se u direktoriju povezani.

Slovni **AuthoritativeNull** slično je **NULL** , ali s razlikom donjem prvenstva pravila mogu pridonositi vrijednost.

U tijeku atribut možete koristiti **IgnoreThisFlow**. Slično NULL u smislu koji znači da nema ništa da biste slali je. Razlika je da ga ne uklanja već postojeću vrijednost u ciljnom. To je kao tijek atribut nikada nije li.

Evo jednog primjera:

U *Odjava AD - hibridna implementacija sustava Exchange korisnika* moguće je pronaći protok sljedeće:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Ovaj izraz trebaju biti: Ako korisnički poštanski sandučić nalazi se u Azure AD, zatim tijeka atribut iz Azure AD za AD. Ako nije, tijek ništa natrag sa servisom Active Directory. U ovom slučaju ga želite zadržati postojeći vrijednost u AD.

### <a name="importedvalue"></a>ImportedValue
Funkcija ImportedValue razlikuje se od drugih funkcija jer je naziv atributa mora biti zatvoren navodnicima umjesto uglatim zagradama:  
`ImportedValue("proxyAddresses")`.

Obično tijekom sinkronizacije atribut koristi vrijednost očekivani čak i ako ona nije izvezeni još ili pogreške primljen tijekom izvoza ("vrh na tower"). Dolazni sinkronizacije pretpostavlja da dosegne atribut koji nije još dosegne povezanog direktorij naposljetku ga. U nekim slučajevima je važno sinkronizirati samo vrijednost koja je potvrđena povezanog direktorija ("hologramski i delta uvoz tower").

Primjer Ova funkcija pronaći ćete u pravilo za sinkronizaciju – okvir *u iz AD – korisnik uobičajenih iz sustava Exchange*. U sustavu Exchange hibridnog, vrijednost dodao Exchange online mora se sinkronizirati samo kada ga je potvrđena uspješno izvezeni vrijednost:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Prednost
Kada pokušam nekoliko pravila za sinkronizaciju da biste slali jednaku vrijednost atributa ciljnoj, vrijednost prednost se koristi za određivanje pića. Pravilo s najvećim prioritetom, najniža brojčana vrijednost, će se da biste slali atributa u sukob.

![Spajanje vrste](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

Ovaj redoslijed mogu se definirati preciznije tokova atribut za mali podskup objekata. Ako, na primjer, izlaz-od-okvir-pravila provjerite imaju li atribute s omogućenom računa (**AccountEnabled korisnika**) prednost s drugih računa.

Prednost može se definirati između poveznike. Poveznika koji omogućuje bolje podacima da biste najprije slali vrijednosti.

### <a name="multiple-objects-from-the-same-connector-space"></a>Više objekata iz isti prostor za poveznika
Ako imate nekoliko objekata na prostoru poveznik priključen na isti objekt metaverse, prednost mora se prilagoditi. Ako nekoliko objekata u opsegu isto sinkronizaciju pravilo, zatim modula za sinkroniziranje nije uspio određivanje prvenstva. Je prevelika koji izvorni objekt treba priložiti vrijednost u metaverse. Tu konfiguraciju prijavljuje kao višeznačnih čak i ako atribute u izvoru imaju iste vrijednosti.  
![Više objekata na isti objekt mv se pridružite](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

U ovom scenariju, morate promijeniti opsega pravila za sinkronizaciju tako da se objekti izvora imaju različite sinkronizaciju pravila u opsegu. Koji omogućuje definiranje različite prednost.  
![Više objekata na isti objekt mv se pridružite](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o jeziku izraz u [Razumijevanje deklarativno izraza dodjele resursa](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
- Pogledajte kako deklarativno dodjele resursa je korištenih u novom tvorničke za [Razumijevanje Zadana konfiguracija](active-directory-aadconnectsync-understanding-default-configuration.md).
- Saznajte kako izraditi praktično promjenu pomoću deklarativno Dodjela resursa u [kako promijeniti zadanu konfiguraciju](active-directory-aadconnectsync-change-the-configuration.md).
- Nastavite čitati kako korisnicima i kontakte suradnja u [Razumijevanje korisnika i kontakata](active-directory-aadconnectsync-understanding-users-and-contacts.md).

**Pregled tema**

- [Azure AD Connect sinkronizacije: razumijevanje i Prilagodba sinkronizacije](active-directory-aadconnectsync-whatis.md)
- [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)

**Pregled tema**

- [Azure AD Connect sinkronizacije: Referenca funkcije](active-directory-aadconnectsync-functions-reference.md)
