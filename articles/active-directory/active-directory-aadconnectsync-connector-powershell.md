<properties
   pageTitle="Poveznik za PowerShell | Microsoft Azure"
   description="U ovom se članku objašnjava kako konfigurirati Microsoft Windows PowerShell poveznik."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="windows-powershell-connector-technical-reference"></a>Tehnička referenca za Windows PowerShell poveznika
U ovom se članku opisuju poveznik za Windows PowerShell. U članku odnosi se na sljedeće proizvode:

- Microsoftov Upravitelj identiteta 2016 (MIM2016)
- Upravitelj identiteta Forefront 2010 R2 (FIM2010R2)
    -   Morate koristiti hitni popravak 4.1.3671.0 ili noviji [KB3092178](https://support.microsoft.com/kb/3092178).

Poveznik za MIM2016 i FIM2010R2, nije dostupan kao preuzeti iz [Microsoftova centra za preuzimanje](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Pregled komponente PowerShell poveznika
Poveznik za PowerShell omogućuje integraciju servisa sinkronizacije s Vanjski sustavi koje nude komponente Windows PowerShell temelji API-ji. Poveznik nudi most između mogućnosti agent za upravljanje sustavom poziv extensible povezivanje 2 framework (ECMA2) i Windows PowerShell. Dodatne informacije o ECMA framework potražite u članku [Extensible povezivanje 2.2 upravljanje Agent referencu](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Preduvjeti
Prije korištenja poveznik, pripazite da imate sljedeće na poslužitelju sinkronizacije:

- 4.5.2 za Microsoft .NET Framework ili novije verzije
- Komponente Windows PowerShell 2.0, 3.0 ili 4.0

Da biste omogućili poveznik da pokreću skripte komponente Windows PowerShell, morate ga konfigurirati pravila izvođenja na poslužitelju za sinkronizaciju servisa. Osim ako skripte pokreće poveznika su digitalno potpisani, konfiguriranje izvođenja pravila tako da pokrenete sljedeću naredbu:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Stvaranje nove poveznika
Da biste stvorili poveznik za Windows PowerShell u servisu sinkronizacije, morate unijeti niz skripte komponente Windows PowerShell koje se izvršiti korake zatražio servisa sinkronizacije. Ovisno o povežete izvor podataka i funkcije tražite, mijenja se skripte morate provesti. U ovom se odjeljku opisuje svaki skripte koje možete implementirana i kad su potrebne.

Poveznik za Windows PowerShell osmišljena je za spremanje svih skripte u bazu podataka za sinkronizaciju servisa. Dok je moguće da pokreću skripte pohranjenima u datotečnom sustavu, lakše je umetnite tijelo svaki skripte izravno u konfiguraciju u poveznik.

Da biste stvorili PowerShell poveznika, u **Servis sinkronizacije** odaberite **Management Agent** i **Stvaranje**. Odaberite poveznik **PowerShell (Microsoft)** .

![Stvaranje poveznika](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Povezivanje
Navedite konfiguracije parametara za povezivanje s udaljenog sustava. Ove vrijednosti sigurno su pohranjeni na servisu sinkronizacije i učinili dostupnima za skripte komponente Windows PowerShell prilikom pokretanja poveznik.

![Povezivanje](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

Možete konfigurirati sljedećih parametara povezivanje:

**Povezivanje**

Parametar | Zadana vrijednost | Svrha
--- | --- | ---
Poslužitelj | <Blank> | Naziv poslužitelja koje treba povezati poveznik.
Domene | <Blank> | Domena vjerodajnice za pohranu za korištenje prilikom pokretanja poveznik.
Korisnik | <Blank> | Username vjerodajnicu za pohranu za korištenje prilikom pokretanja poveznik.
Lozinke | <Blank> | Lozinka vjerodajnice za pohranu za korištenje prilikom pokretanja poveznik.
Oponašanje poveznik računa | FALSE | Kada je true, servis sinkronizacije pokreće skripte komponente Windows PowerShell u kontekstu ponuđene vjerodajnice. Kada je to moguće, preporučuje se da parametar **$Credentials** prenosi svakom skripte koristit će se umjesto oponašanja. Dodatne informacije o dodatne dozvole koje su potrebne da biste koristili tu mogućnost u odjeljku [Dodatne konfiguracije za predstavljanje](#additional-configuration-for-impersonation).
Učitavanje korisničkog profila kada oponaša | FALSE | Upućuje Windows da biste učitali korisničkog profila u poveznik vjerodajnica tijekom oponašanja. Ako bez predstavljanja korisnik ima zajednički profil, poveznik učitati zajednički profil. Dodatne informacije o dodatne dozvole koje su potrebne za taj parametar koristite potražite u članku [Dodatne konfiguracijske za predstavljanje](#additional-configuration-for-impersonation).
Vrsta prijave kada oponaša | Ništa | Vrsta prijave tijekom oponašanja. Dodatne informacije potražite u priručniku [dwLogonType] [ dw] dokumentacije.
Traje skripti | FALSE | Ako je true, poveznik za Windows PowerShell provjerava ima li svaki skripte valjani digitalni potpis. Ako je false, provjerite je li poslužitelj za sinkronizaciju servisa Windows PowerShell izvođenja pravila RemoteSigned ili Unrestricted.

**Modul za uobičajene**  
Poveznik vam omogućuje pohranu zajedničke modula Windows PowerShell u konfiguraciji. Kada se poveznik pokrene skriptu, modul Windows PowerShell izdvajati datotečni sustav tako da ga može uvesti svaki skripte.

Za uvoz, izvoz i sinkronizaciju lozinke skripte uobičajenih modul dobivaju se nalazi poveznik MAData mapu. Za otkrivanje skripte shemu, provjera valjanosti, hijerarhije i particija uobičajenih modul dobivaju se u mapu % TEMP %. U oba slučaja izdvojene skripte uobičajenih modul pod nazivom prema postavku uobičajeni naziv skripte modul.

Da biste učitali u modulu naziva FIMPowerShellConnectorModule.psm1 iz mape MAData, koristite sljedeću naredbu:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Da biste učitali u modulu naziva FIMPowerShellConnectorModule.psm1 iz mape % TEMP %, koristite sljedeću naredbu:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Parametar provjere valjanosti**  
Skriptu za provjeru valjanosti je neobavezno skripte komponente Windows PowerShell koje je moguće koristiti da bi bili dani administrator poveznik konfiguracije parametri valjane. Provjera valjanosti poslužitelja i vjerodajnice za povezivanje parametri povezivanja su uobičajeni upotrebe skriptu za provjeru valjanosti. Skriptu za provjeru valjanosti zove nakon sljedećim karticama i dijaloški okviri se mijenjaju:

- Povezivanje
- Globalni parametara
- Konfiguriranje particije

Skriptu za provjeru valjanosti prima sljedećih parametara iz poveznik:

Ime | Vrsta podataka | Opis
--- | --- | ---
ConfigParameterPage | [ConfigParameterPage][cpp] | Konfiguriranje tabulatora ili dijaloški okvir koji se aktivira zahtjev za provjeru valjanosti.
ConfigParameters | [KeyedCollection] [ keyk] [niz [ConfigParameter][cp]] | Tablica konfiguracije parametara za poveznik.
Vjerodajnica | [PSCredential][pscred] | Sadrži vjerodajnice unijeli administrator na kartici povezivanje.

Skriptu za provjeru valjanosti će se vratiti na jedan objekt ParameterValidationResult na kanal.

**Otkrivanje sheme**  
Skripta za otkrivanje sheme je obavezan. Ova skripta vraća vrste objekata, atributi i ograničenja atributa koji koristi servis sinkronizacije prilikom konfiguriranja pravila za tijek atribut. Skripta za otkrivanje sheme se izvodi prilikom stvaranja poveznika i popunjava sheme na poveznik. Koristi se i osvježavanje sheme akcija u Upravitelj servisa sinkronizacije.

Skripta za otkrivanje sheme prima sljedećih parametara iz poveznik:

Ime | Vrsta podataka | Opis
--- | --- | ---
ConfigParameters | [KeyedCollection] [ keyk] [niz [ConfigParameter][cp]] | Tablica konfiguracije parametara za poveznik.
Vjerodajnica | [PSCredential][pscred] | Sadrži vjerodajnice unijeli administrator na kartici povezivanje.

Skripta mora vratiti jedan [sheme] [ schema] objekta za kanal. Objekt sheme sastoji se od [SchemaType] [ schemaT] objekata koji predstavljaju vrste objekata (na primjer: korisnici i grupe). Objekt SchemaType sadrži skup [SchemaAttribute] [ schemaA] objekata koji predstavljaju atributa (na primjer: ime, prezime i poštansku adresu) vrste.

**Dodatni parametri**  
Osim standardnih konfiguracijske postavke možete definirati dodatne prilagođene konfiguracijske postavke koje se odnose na instancu poveznik. Možete navesti parametara pri poveznik particije, ili izvođenja korak razine i pristupiti iz relevantnih skripte komponente Windows PowerShell. Postavke prilagođene konfiguracije moguće pohraniti u bazi podataka za servis za sinkronizaciju u obliku običnog teksta ili može biti šifrirana. Sinkronizacija servisa automatski šifrira i dešifrira sigurne konfiguracijske postavke po potrebi.

Da biste odredili postavki prilagođene konfiguracije, odvojite naziv parametra točku sa zarezom (;).

Da biste pristupili postavki prilagođene konfiguracije putem skripte, morate kratica naziv podvlakom ( \_ ) i opseg parametar (Global, particija ili RunStep). Ako, na primjer, da biste pristupili parametar globalni naziv datoteke, koristite ovaj isječak koda:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Mogućnosti
Kartica mogućnosti upravljanja dizajnera Agent definira ponašanje i funkcije poveznika. Odabiri na toj kartici nije moguće mijenjati kada je stvorena poveznik. U ovoj su tablici navedene postavke za mogućnost.

![Mogućnosti](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

Mogućnost | Opis |
--- | --- |
[Razlikovni naziv stila][dnstyle] | Upućuje na to ako poveznik podržava razlikovni nazive i zato što stila.
[Izvoz vrsta][exportT] | Određuje vrstu objekte koji su usmjereni skriptu izvoz. <li>AttributeReplace – uključuje potpunog skupa vrijednosti za atribut s više vrijednosti prilikom promjene atribut.</li><li>AttributeUpdate – uključuje samo deltas atribut s više vrijednosti prilikom promjene atribut.</li><li>MultivaluedReferenceAttributeUpdate – uključuje potpunog skupa vrijednosti s većim brojem vrijednosti atributa koji nisu reference i samo deltas za atribute reference s više vrijednosti.</li><li>ObjectReplace – obuhvaća sve atribute objekta kad nešto atributa promjene</li>
[Normalizacija podataka][DataNorm] | Upućuje servisa sinkronizacije koju treba normalizirati sidro atribute prije nego što se oni postoje skripti.
[Potvrda objekta][oconf] | Konfigurira ponašanje na čekanju uvoz u servisu sinkronizacije. <li>Normalno – zadano ponašanje koje očekuje sve izvezene promjene potvrditi putem uvoza</li><li>NoDeleteConfirmation – nakon brisanja objekta postoji ne čeka uvoz generira.</li><li>NoAddAndDeleteConfirmation – kada objekta Stvori ili izbrisane, postoji ne čeka uvoz generira.</li>
Korištenje DN kao sidro | Ako je Razlikovni naziv stila postavljeno na LDAP, atribut sidrenja poveznik prostora ujedno Razlikovni naziv.
Istodobni operacije nekoliko poveznika | Kad se potvrdi, istodobno možete pokrenuti većeg broja poveznika komponente Windows PowerShell.
Particije | Kad se potvrdi, poveznik podržava više particija i otkrivanje particije.
Hijerarhija | Kad se potvrdi, poveznik podržava strukturu hijerarhijski LDAP stil.
Omogućivanje uvoza | Kad se potvrdi, poveznik uvozi podataka putem skripti uvoz.
Omogućivanje uvoz Delta | Kad se potvrdi, poveznik možete zatražiti deltas iz skripti uvoz.
Omogućivanje izvoza | Kad se potvrdi, poveznik izvoz podataka putem skripti izvoz.
Omogućivanje cijelog izvoza | Kad se potvrdi, skripte izvoz podržava izvoz prostora cijelu poveznik. Da biste koristili tu mogućnost, omogućite izvoz moraju se provjeriti.
Nema referenca vrijednosti u prvom izvoz računanja | Kad se potvrdi, referenca atribute izvoze se u drugom računanja izvoz.
Omogućivanje Preimenovanje objekta | Kad se potvrdi, moguće je izmijeniti razlikovni imena.
Brisanje dodavanje kao zamjenu | Kad se potvrdi, brisanje dodavanje operacije izvoze se kao jedan zamjena.
Omogućivanje operacije lozinke | Kad se potvrdi, podržani su skripte za sinkronizaciju lozinke.
Omogućivanje lozinku za izvoz u prvom računanja | Kad se potvrdi, lozinke postavite tijekom dodjeljivanja izvoze se stvaranja objekta.

### <a name="global-parameters"></a>Globalni parametara
Na kartici globalni parametara u dizajneru Agent za upravljanje omogućuje vam da biste konfigurirali skripte komponente Windows PowerShell pokrenute poveznik. Možete konfigurirati i vrijednosti globalne postavke prilagođene konfiguracije definirana na kartici povezivanje.

**Otkrivanje partition**  
Particija je zasebnom prostor naziva unutar jedne zajedničke sheme. Ako, na primjer, u servisu Active Directory svaki je domena particija unutar jedan skup stabala. Particija je logički grupiranja za uvoz i izvoz operacije. Uvoz i izvoz vam particija kao što je kontekst i sve operacije se događa u ovom kontekstu. Da biste predstavili hijerarhije u LDAP trebali particije. Razlikovni naziv particije se uvoza provjerite jesu li svi objekti vraćeni unutar opsega particije. Razlikovni naziv particije i koristi prilikom dodjele resursa u metaverse prostor poveznik za određivanje particija objekta mora biti povezan s tijekom izvoza.

Skripta za otkrivanje particija prima sljedećih parametara iz poveznik:

Ime | Vrsta podataka | Opis
--- | --- | ---
ConfigParameters  | [KeyedCollection][keyk][niz [ConfigParameter][cp]] | Tablica konfiguracije parametara za poveznik.
Vjerodajnica | [PSCredential][pscred] | Sadrži vjerodajnice unijeli administrator na kartici povezivanje.

Skripta mora na ili vratiti jedna [particija] [ part] objekt ili popis [T] Partition objekata na kanal.

**Otkrivanje hijerarhije**  
Skripta za otkrivanje hijerarhije koristi samo kada je mogućnost Razlikovni naziv stila LDAP. Skripta koristi se za jednostavan način možete pregledavati i odaberite Postavi spremnika koje smatra i smanjivanje opseg za uvoz i izvoz operacije. Skripta mora sadržavati samo popis čvorove koje su izravno podređena Korijenski čvor dodjeljuje skriptu.

Skripta za otkrivanje hijerarhije prima sljedećih parametara iz poveznik:

Ime | Vrsta podataka | Opis
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][niz [ConfigParameter][cp]] | Tablica konfiguracije parametara za poveznik.
Vjerodajnica | [PSCredential][pscred] | Sadrži vjerodajnice unijeli administrator na kartici povezivanje.
ParentNode | [HierarchyNode][hn] | Korijenski čvor hijerarhije pod kojim skriptu mora vratiti izravno podređena.

Skripta mora vratiti na bilo koju jedan podređeni objekt za HierarchyNode ili popis [T] objektima HierarchyNode za kanal.

#### <a name="import"></a>Uvoz
Poveznika koji podržavaju operacije uvoza morate provesti tri skripti.

**Započinjanje uvoza**  
Uvoz skripte za početak se izvodi na početku se korak uvoz pokrenuti. Tijekom ovaj korak možete uspostaviti vezu u izvorišnom sustavu i učinite pripremnih koraka prije uvoza podataka iz povezanih sustava.

Uvoz skripte početak prima sljedećih parametara iz poveznik:

Ime | Vrsta podataka | Opis
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][niz [ConfigParameter][cp]] | Tablica konfiguracije parametara za poveznik.
Vjerodajnica | [PSCredential][pscred] | Sadrži vjerodajnice unijeli administrator na kartici povezivanje.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Skripta obavještava o vrsti uvoz pokrenuti (delta ili punu verziju), particija, hijerarhije, vodeni žig i veličine očekivani stranice.
Vrste | [Shema][schema] | Shema za prostor poveznika koji se uvozi.

Skripta mora vratiti jedan [OpenImportConnectionResults] [ oicres] objekt na kanalu, na primjer:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Uvoz podataka**  
Uvoz podatkovne skripte pozove poveznik dok skriptu označava da nema više podataka da biste uvezli. Poveznik za Windows PowerShell sadrži veličine stranice 9,999 objekata. Ako skriptu vraća više 9,999 objekata za uvoz, morate podržava broja stranica. Poveznik otkriva zove svojstvo prilagođenih podataka koje možete koristiti u spremište vodenog žiga tako da svaki put skripte za uvoz podataka, skriptu nastavlja Uvoz objekata koji se tamo gdje ga stali.

Uvoz podataka skriptu prima sljedećih parametara iz poveznik:

Ime | Vrsta podataka | Opis
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][niz [ConfigParameter][cp]] | Tablica konfiguracije parametara za poveznik.
Vjerodajnica | [PSCredential][pscred] | Sadrži vjerodajnice unijeli administrator na kartici povezivanje.
GetImportEntriesRunStep | [ImportRunStep][irs] | Sadrži vodeni žig (CustomData) koje je moguće koristiti tijekom Straničeno uvozi i delta uvozi.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Skripta obavještava o vrsti uvoz pokrenuti (delta ili punu verziju), particija, hijerarhije, vodeni žig i veličine očekivani stranice.
Vrste | [Shema][schema] | Shema za prostor poveznika koji se uvozi.

Skripta za uvoz podataka morate napisati popis [[CSEntryChange][csec]] objekta za kanal. Ova zbirka sastoji se od CSEntryChange atributa koji predstavljaju sve objekte koji se uvozi. Tijekom potpuni uvoz Pokreni, zbirku mora imati potpunog skupa CSEntryChange objekte koji nemaju Svi atributi za svaki objekt. Tijekom na Delta uvezli, CSEntryChange objekt ili smiju sadržavati atribut razine deltas za svaki objekt da biste uvezli ili cijeli prikaz objekata koje su se promijenile (Zamijeni način rada).

**Uvoz završetka**  
Po završetku uvoza pokrenite je pokrenuti skriptu uvoz Završi. Ova skripta provoditi sve zadatke čišćenja potrebne (na primjer, Zatvori veze na sustave) i odgovaranje na pogreške.

Uvoz skripte završetka prima sljedećih parametara iz poveznik:

Ime | Vrsta podataka | Opis
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][niz [ConfigParameter][cp]] | Tablica konfiguracije parametara za poveznik.
Vjerodajnica | [PSCredential][pscred] | Sadrži vjerodajnice unijeli administrator na kartici povezivanje.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Skripta obavještava o vrsti uvoz pokrenuti (delta ili punu verziju), particija, hijerarhije, vodeni žig i veličine očekivani stranice.
CloseImportConnectionRunStep | [CloseImportConnectionRunStep][cecrs] | Skripta obavještava o razloga ukinuta uvoz.

Skripta mora vratiti jedan [CloseImportConnectionResults] [ cicres] objekt na kanalu, na primjer:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Izvoz
Jednako arhitektura uvoz poveznika, poveznika koji podržavaju izvoz morate provesti tri skripti.

**Početak izvoza**  
Izvoz skripte za početak se izvodi na početku se korak izvoz pokrenuti. Tijekom ovaj korak možete uspostaviti vezu u izvorišnom sustavu i vođenje pripremnih koraci prije izvoza podataka u povezanim sustav.

Izvoz skripte početak prima sljedećih parametara iz poveznik:

Ime | Vrsta podataka | Opis
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][niz [ConfigParameter][cp]] | Tablica konfiguracije parametara za poveznik.
Vjerodajnica | [PSCredential][pscred] | Sadrži vjerodajnice unijeli administrator na kartici povezivanje.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Skripta obavještava o vrsti izvoz pokrenuti (delta ili punu verziju), particija, hijerarhije i veličine očekivani stranice.
Vrste | [Shema][schema] | Shema za prostor poveznik izvezenih.

Skripta mora vratiti sve izlazne za kanal.

**Izvoz podataka**  
Sinkronizacija servisa poziva skripte izvoz podataka kao što je potrebno za obradu sve na čekanju izvozi neograničen broj puta. Ako prostor poveznika ima više na čekanju izvozi od veličine stranice na poveznika, skripta za izvoz podataka može se pozivati više puta i vjerojatno više puta za na isti objekt.

Izvoz podataka skriptu prima sljedećih parametara iz poveznik:

Ime | Vrsta podataka | Opis
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][niz [ConfigParameter][cp]] | Tablica konfiguracije parametara za poveznik.
Vjerodajnica | [PSCredential][pscred] | Sadrži vjerodajnice unijeli administrator na kartici povezivanje.
CSEntries | IList[CSEntryChange][csec] | Popis sve poveznik prostora objekte s izvozom obraditi tijekom ovoj fazi na čekanju.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Skripta obavještava o vrsti izvoz pokrenuti (delta ili punu verziju), particija, hijerarhije i veličine očekivani stranice.
Vrste | [Shema][schema] | Shema za prostor poveznik izvezenih.

Skripta za izvoz podataka mora vratiti [PutExportEntriesResults] [ peeres] objekta za kanal. Taj objekt nije potrebno obuhvatiti informacije rezultata za svaki izvezeni poveznik osim u slučaju pogreške ili promjenu atributa sidro pojavljuje. Na primjer, da biste se vratili na objekt PutExportEntriesResults kanal:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Izvoz završetka**  
Po završetku izvoza pokrenuli, skripte izvoz End da biste pokrenuli. Ova skripta provoditi sve zadatke čišćenja potrebne (na primjer, Zatvori veze na sustave) i odgovaranje na pogreške.

Izvoz skripte završetka prima sljedećih parametara iz poveznik:

Ime | Vrsta podataka | Opis
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][niz [ConfigParameter][cp]] | Tablica konfiguracije parametara za poveznik.
Vjerodajnica | [PSCredential][pscred] | Sadrži vjerodajnice unijeli administrator na kartici povezivanje.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Skripta obavještava o vrsti izvoz pokrenuti (delta ili punu verziju), particija, hijerarhije i veličine očekivani stranice.
CloseExportConnectionRunStep | [CloseExportConnectionRunStep][cecrs] | Skripta obavještava o razloga ukinuta izvoz.

Skripta mora vratiti sve izlazne za kanal.

#### <a name="password-synchronization"></a>Sinkronizaciju lozinke
Komponente Windows PowerShell poveznika moguće je koristiti kao odredište za promjene/lozinki.

Skripta lozinka prima sljedećih parametara iz poveznik:

Ime | Vrsta podataka | Opis
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][niz [ConfigParameter][cp]] | Tablica konfiguracije parametara za poveznik.
Vjerodajnica | [PSCredential][pscred] | Sadrži vjerodajnice unijeli administrator na kartici povezivanje.
Particija | [Particija][part] | Particija direktorija koji se nalazi na CSEntry u.
CSEntry | [CSEntry][cse] | Unos prostora poveznik za objekte koji su primili Promjena lozinke ili Vrati izvorne postavke.
OperationType | Niz | Označava je li postupak ponovno postavljanje (**SetPassword**) ili promjena (**Promjena lozinke**).
PasswordOptions | [PasswordOptions][pwdopt] | Oznake koje navode svrhu lozinku ponovno postaviti ponašanje. Ovaj parametar je dostupna samo ako je OperationType **SetPassword**.
Staralozinka | Niz | Popunjen objekta staru lozinku za promjenu lozinke. Ovaj parametar je dostupna samo ako je OperationType **Promjena lozinke**.
Novalozinka | Niz | Popunjen objekta novu lozinku postavite skriptu.

Skripta za lozinku ne očekuje da biste se vratili rezultate kanal komponente Windows PowerShell. Ako dođe do pogreške u skripti lozinku, skripta mora vratiti neku od sljedeće iznimke za obavještavanje o servisu sinkronizacije o problemu:

- [PasswordPolicyViolationException] [ pwdex1] – izbačena Ako lozinka ne zadovoljava pravila lozinke u sustavu povezani.
- [PasswordIllFormedException] [ pwdex2] – izbačena Ako lozinka nije prihvatljiva povezanog sustava.
- [PasswordExtension] [ pwdex3] – izbačena drugih pogrešaka u skripti lozinku.

## <a name="sample-connectors"></a>Ogledna poveznika
Pregled poveznika dostupna uzorka potražite u članku [Prikupljanje poveznik za Windows PowerShell poveznik uzorka][samp].

## <a name="other-notes"></a>Napomene

### <a name="additional-configuration-for-impersonation"></a>Dodatna konfiguracija oponašanja
Dodjela korisnika koji je oponašati sljedeće dozvole na poslužitelju servisa sinkronizacije:

Pristup čitanju u sljedećim ključevima registra:

- HKEY_USERS\\\Software\Microsoft\PowerShell [SynchronizationServiceServiceAccountSID]
- HKEY_USERS\\\Environment [SynchronizationServiceServiceAccountSID]

Da biste utvrdili sigurnosti identifikator (SID) računa za servis za sinkronizaciju servisa, pokrenite sljedeće naredbe ljuske PowerShell:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Pristup za čitanje u sljedeće mape datoteka sustava:

- %ProgramFiles%\Microsoft forefront identiteta Manager\2010\Synchronization Service\Extensions
- %ProgramFiles%\Microsoft forefront identiteta Manager\2010\Synchronization Service\ExtensionsCache
- %ProgramFiles%\Microsoft forefront identiteta Manager\2010\Synchronization Service\MaData\\{ConnectorName}

Zamijenite naziv poveznik za Windows PowerShell za rezervirano mjesto {ConnectorName}.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

-   Informacije o omogućivanju zapisivanja poveznik za otklanjanje poteškoća s potražite u članku [kako omogućiti praćenje ETW za poveznika](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
