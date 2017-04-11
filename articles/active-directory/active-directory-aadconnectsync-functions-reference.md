<properties
    pageTitle="Azure AD Connect sinkronizacije: Referenca funkcije | Microsoft Azure"
    description="Referenca deklarativno dodjele resursa izraze u Azure AD Connect sinkronizaciju."
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
    ms.date="08/23/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Connect sinkronizacije: Referenca funkcije

U Azure AD Connect funkcije koriste se za rukovanje atributa vrijednosti tijekom sinkronizacije.  
Sintaksa funkcije izražen je u sljedećem obliku:  
`<output type> FunctionName(<input type> <position name>, ..)`

Ako funkcija je pretrpan i prihvaća više syntaxes, navedene su sve valjane syntaxes.  
Funkcija svakako upisane i potvrđuju da vrstu proslijeđen podudaranja dokumentirane vrsta.  
Ako ne odgovara vrsti pogreške se Expression.Error.

Vrsta su izražena pomoću sljedeće sintakse:

- **smeće** – binarni.
- **booleovom** – Booleove vrijednosti
- **dt** – UTC datuma/vremena
- **redni broj** – Enumeracije poznati konstante
- **exp** – izraz koji se očekuje da biste procijenili na Booleove vrijednosti
- **mvbin** – Multi-Valued binarni
- **mvstr** – Multi-Valued niza
- **mvref** – Multi-Valued referenca
- **num** – numeričke
- **ref** – referenca
- **str** – niza
- **var** – varijante (gotovo) bilo koju drugu vrstu
- **Poništi** – neće vraćati vrijednost

Funkcije s vrstama **mvbin**, **mvstr**i **mvref** samo omogućuje rad na više vrijednosti atributa. Funkcije s **smeće**, **str**i **ref** radite na jednom vrijednosti i s većim brojem vrijednosti atributa.

## <a name="functions-reference"></a>Referenca za funkcije

Popis funkcija | | | | |  
--------- | --------- | --------- | --------- | --------- | ---------
**Pretvorba** |  
[CBool](#cbool) | [CDate](#cdate) | [CGuid](#cguid) | [ConvertFromBase64](#convertfrombase64)
[ConvertToBase64](#converttobase64) | [ConvertFromUTF8Hex](#convertfromutf8hex) | [ConvertToUTF8Hex](#converttoutf8hex) | [CNum](#cnum)
[CRef](#cref) | [CStr](#cstr) | [StringFromGuid](#StringFromGuid) | [StringFromSid](#stringfromsid)
**Datum / vrijeme** |  
[DateAdd](#dateadd) | [DateFromNum](#datefromnum) | [FormatDateTime](#formatdatetime) | [Sada](#now)
[NumFromDate](#numfromdate) |  
**Direktorija** |  
[DNComponent](#dncomponent) | [DNComponentRev](#dncomponentrev) | [EscapeDNComponent](#escapedncomponent)
**Procjena** |  
[IsBitSet](#isbitset) | [IsDate](#isdate) | [IsEmpty](#isempty) | [IsGuid](#isguid)
[IsNull](#isnull) | [IsNullOrEmpty](#isnullorempty) | [IsNumeric](#isnumeric) | [IsPresent](#ispresent) |
[IsString](#isstring) |  
**Matematika** |  
[BitAnd](#bitand) | [BitOr](#bitor) | [RandomNum](#randomnum)
**S više vrijednosti** |  
[Sadrži](#contains) | [Count](#count) | [Stavke](#item) | [ItemOrNull](#itemornull)
[Uključivanje](#join) | [RemoveDuplicates](#removeduplicates) | [Podjela](#split) |
**Tok programa** |  
[Pogreška](#error) | [IIF](#iif)  | [Promjena](#switch)
**Tekst** |  
[GUID](#guid) | [InStr](#instr) | [InStrRev](#instrrev) | [LCase](#lcase)
[Lijevo](#left) | [LEN](#len) | [Funkcije LTrim](#ltrim) | [MID](#mid)
[PadLeft](#padleft) | [PadRight](#padright) | [PCase](#pcase) | [Zamjena](#replace)
[ReplaceChars](#replacechars) | [Desno](#right) | [RTrim](#rtrim) | [Funkcija TRIM](#trim)
[UCase](#ucase) | [Word](#word)

----------
### <a name="bitand"></a>BitAnd

**Opis:**  
Funkcija BitAnd postavlja navedeni bitova vrijednosti.

**Sintaksa:**  
`num BitAnd(num value1, num value2)`

- vrijednost1; vrijednost2: numeričke vrijednosti koje treba AND'ed zajedno

**Napomena:**  
Ova funkcija oba parametra pretvara u binarni prikaz i postavlja malo:

- 0 - Ako jedno ili oboje od odgovarajuće bitova *maske za unos* i *zastavice* 0
- 1 – ako su oba odgovarajuće bitova 1.

Drugim riječima, vratit će 0 u svakom slučaju, osim ako su odgovarajuće bitova oba parametra 1.

**Primjer:**  
`BitAnd(&HF, &HF7)`  
Vraća 7 jer heksadecimalni "F" i "F7" procjenjuju se na tu vrijednost.

----------
### <a name="bitor"></a>BitOr

**Opis:**  
Funkcija BitOr postavlja navedeni bitova vrijednosti.

**Sintaksa:**  
`num BitOr(num value1, num value2)`

- vrijednost1; vrijednost2: numeričke vrijednosti koje treba OR'ed zajedno

**Napomena:**  
Ova funkcija oba parametra pretvara u binarni prikaz i postavlja malo 1 ako je jedan ili oba odgovarajuće bitova maske za unos i zastavica ima vrijednost 1 i 0 Ako su oba odgovarajuće bitova 0. Drugim riječima, vratit će 1 u svim slučajevima osim gdje su odgovarajuće bitova oba parametra 0.

----------
### <a name="cbool"></a>CBool

**Opis:**  
Funkcija CBool vraća Booleov izraz provjeriti u odnosu na temelju

**Sintaksa:**  
`bool CBool(exp Expression)`

**Napomena:**  
Ako je izraz vrijednost osim nule, a zatim CBool vraća vrijednost True, inače vraća False.

**Primjer:**  
`CBool([attrib1] = [attrib2])`  

Vraća True Ako oba atributa imaju iste vrijednosti.

----------
### <a name="cdate"></a>CDate

**Opis:**  
Funkcija CDate vraća datum i vrijeme UTC-a iz niza. Datum i vrijeme nije sinkronizirana izvorni atribut vrsta, ali koristi neke funkcije.

**Sintaksa:**  
`dt CDate(str value)`

- Vrijednost: Niza s i datum, vrijeme i po želji vremenske zone

**Napomena:**  
Vraćeni niz uvijek je u UTC-u.

**Primjer:**  
`CDate([employeeStartTime])`  
Vrijeme početka vraća s datumom i vremenom na temelju zaposlenika

`CDate("2013-01-10 4:00 PM -8")`  
Vraća datum i vrijeme koji predstavlja "2013 01 11 12.00: 00 Prijepodne"

----------
### <a name="cguid"></a>CGuid

**Opis:**  
Funkcija CGuid pretvara prikaz niza GUID njegov binarni prikaz.

**Sintaksa:**  
`bin CGuid(str GUID)`

- Niz oblikovan u ovaj uzorak: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx ili {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

----------
### <a name="contains"></a>Sadrži

**Opis:**  
Funkcija sadrži nađe niza unutar atribut s više vrijednosti

**Sintaksa:**  
`num Contains (mvstring attribute, str search)`-velika i mala slova  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-velika i mala slova

- Atribut: atribut s više vrijednosti za pretraživanje.
- pretraživanje: niza da biste pronašli u atributu.
- Casetype: CaseInsensitive ili CaseSensitive.

Vraća indeksa u atribut s više vrijednosti u kojima je pronađen niz. Ako niz nije pronađen, vraća se 0.

**Napomena:**  
Pretraživanje za atribute s većim brojem vrijednosti niza pronalazi podnizova u vrijednosti.  
Za referencu atribute pretražiti niz mora u potpunosti odgovarati vrijednost smatraju podudaranje.

**Primjer:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Ako je atributa proxyAddresses primarne adrese (označena je u velika slova "SMTP:"), vratite atribut proxyAddress, ostali vratiti pogrešku.

----------
### <a name="convertfrombase64"></a>ConvertFromBase64

**Opis:**  
Funkcija ConvertFromBase64 pretvara vrijednost navedenu base64 kodirani običnog niz.

**Sintaksa:**  
`str ConvertFromBase64(str source)`-pretpostavlja Unicode za šifriranje  
`str ConvertFromBase64(str source, enum Encoding)`

- Izvor: Base64 kodirani niza  
- Kodiranje: UTF8 Unicode, ASCII,

**Primjer**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Povratna i primjeri "*Zdravo svijete!*"

----------
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex

**Opis:**  
Funkcija ConvertFromUTF8Hex pretvara određenu vrijednost UTF8 heksadecimalno kodirani u niz.

**Sintaksa:**  
`str ConvertFromUTF8Hex(str source)`

- Izvor: UTF8 2-bajtni kodiranih sting

**Napomena:**  
Razlika između Ova funkcija i ConvertFromBase64([],UTF8) u rezultat je neslužbeni atributa DN.  
Azure Active Directory koristi ovaj oblik kao DN.

**Primjer:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Vraća "*Zdravo svijete!*"

----------
### <a name="converttobase64"></a>ConvertToBase64

**Opis:**  
Funkcija ConvertToBase64 pretvara u niz base64 Unicode.  
Pretvara vrijednost polja s cijelim brojevima za njegov prikaz ekvivalentan niz kodirana s osnovni 64 znamenki.

**Sintaksa:**  
`str ConvertToBase64(str source)`

**Primjer:**  
`ConvertToBase64("Hello world!")`  
Vraća "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

----------
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex

**Opis:**  
Funkcija ConvertToUTF8Hex pretvara u UTF8 heksadecimalno kodirani vrijednost.

**Sintaksa:**  
`str ConvertToUTF8Hex(str source)`

**Napomena:**  
Izlazni oblik Ova funkcija koristi Azure Active Directory kao DN atributa oblika.

**Primjer:**  
`ConvertToUTF8Hex("Hello world!")`  
Vraća 48656C6C6F20776F726C6421

----------
### <a name="count"></a>Count

**Opis:**  
Funkcija Count vraća broj elemenata u atribut s više vrijednosti

**Sintaksa:**  
`num Count(mvstr attribute)`

----------
### <a name="cnum"></a>CNum

**Opis:**  
Funkcija CNum uzima niza i vraća numeričku vrstu podataka.

**Sintaksa:**  
`num CNum(str value)`

----------
### <a name="cref"></a>CRef

**Opis:**  
Pretvara u referenci atributa

**Sintaksa:**  
`ref CRef(str value)`

**Primjer:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

----------
### <a name="cstr"></a>CStr

**Opis:**  
Funkcija CStr pretvara se u vrsti podataka niza.

**Sintaksa:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

- vrijednost: može biti brojčana vrijednost, referenca atribut ili Booleove vrijednosti.

**Primjer:**  
`CStr([dn])`  
Nije moguće vratiti "cn = Ivane, kontroler = contoso, kontroler = com"

----------
### <a name="dateadd"></a>DateAdd

**Opis:**  
Vraća datum koji sadrže datum koji je dodan u određenom vremenskom intervalu.

**Sintaksa:**  
`dt DateAdd(str interval, num value, dt date)`

- Interval: nizovni izraz koji je vremensko razdoblje koje želite dodati. Niz mora imati jedan od sljedećih vrijednosti:
 - gggg godine
 - pitanja tromjesečje
 - m mjesec
 - y dana godini
 - d dan
 - w Weekday
 - ww tjedna
 - h sat
 - n minuta
 - s drugom
- vrijednost: broj jedinica koje želite dodati. Možda ćete se pozitivan (za datume u budućnosti) ili negativan (za datume u prošlosti).
- Datum: koji predstavlja datum dodaje interval datuma i vremena.

**Primjer:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Zbraja 3 mjeseca i vraća DateTime koji predstavlja "2001-04-01".

----------
### <a name="datefromnum"></a>DateFromNum

**Opis:**  
Funkcija DateFromNum pretvara vrijednost u AD-datum oblik vrsta datuma/vremena.

**Sintaksa:**  
`dt DateFromNum(num value)`

**Primjer:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Vraća datum i vrijeme koji predstavlja 2012 01 01 23:00:00

----------
### <a name="dncomponent"></a>DNComponent

**Opis:**  
Funkcija DNComponent vraća vrijednost navedeni DN komponente prelaska slijeva.

**Sintaksa:**  
`str DNComponent(ref dn, num ComponentNumber)`

- dn: atribut referenca za tumačenje
- ComponentNumber: Komponente u DN da biste se vratili

**Primjer:**  
`DNComponent([dn],1)`  
Ako je dn "cn = Ivane, ou =,...," vratit će Joe

----------
### <a name="dncomponentrev"></a>DNComponentRev

**Opis:**  
Funkcija DNComponentRev vraća vrijednost navedeni DN komponente prijelaz s desne (Završi).

**Sintaksa:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- dn: atribut referenca za tumačenje
- ComponentNumber - komponenta u DN da biste se vratili
- Mogućnosti: Kontroler – Zanemari sve komponente s "kontroler ="

**Primjer:**  
Ako je dn "cn = Ivane, ou = Atlanta, ou = GA, a zatim ou = US, kontroler = contoso, kontroler = com" zatim  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
Vraćaju US.

----------
### <a name="error"></a>Pogreška

**Opis:**  
Da biste se vratili pogreške se koristi funkciju pogreške.

**Sintaksa:**  
`void Error(str ErrorMessage)`

**Primjer:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Ako atribut accountName nema, vratiti pogrešku na objekt.

----------
### <a name="escapedncomponent"></a>EscapeDNComponent

**Opis:**  
Funkcija EscapeDNComponent uzima jednu komponentu na DN i escapes tako da ga je moguće prikazati u LDAP.

**Sintaksa:**  
`str EscapeDNComponent(str value)`

**Primjer:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Provjerava je li objekt možete stvoriti u LDAP imenik čak i ako atribut riješiti problem sadrži znakove koji morate unijeti prespojni znak u LDAP.

----------
### <a name="formatdatetime"></a>FormatDateTime

**Opis:**  
Da biste oblikovali datuma i vremena u niz s navedenom obliku koji se koristi funkcija FormatDateTime

**Sintaksa:**  
`str FormatDateTime(dt value, str format)`

- vrijednost: vrijednost u obliku datuma i vremena
- Oblik: niz koji predstavlja oblik da biste pretvorili.

**Napomena:**  
Moguće vrijednosti za oblik nalazi se ovdje: [Korisnički definirane oblike datuma/vremena (funkcija Format)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Primjer:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
Rezultati u "2007-12-25".

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
Može uzrokovati "20140905081453.0Z"

----------
### <a name="guid"></a>GUID

**Opis:**  
Funkcija GUID stvara novi slučajni GUID

**Sintaksa:**  
`str GUID()`

----------
### <a name="iif"></a>IIF

**Opis:**  
Funkcija IIF vraća jedan od skupa mogućih vrijednosti na temelju navedenih uvjeta.

**Sintaksa:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- Uvjet: bilo koja vrijednost ili izraz koji je moguće vrednovati kao true ili false.
- valueIfTrue: Ako se procijeni kao true, vraćena vrijednost.
- valueIfFalse: Ako se procijeni kao false, vraćena vrijednost.

**Primjer:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Ako je korisnik programa intern, vraća pseudonim korisnika s "t-" dodaju na početak je još vraća pseudonim korisnika, kao što je.

----------
### <a name="instr"></a>InStr

**Opis:**  
Funkcija InStr pronalazi prvo pojavljivanje podniz u nizu.

**Sintaksa:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- stringcheck: niz za pretraživanje
- stringmatch: niz pronađen
- pokretanje: početni položaj da biste pronašli podniz
- Usporedba: vbTextCompare ili vbBinaryCompare

**Napomena:**  
Vraća položaj gdje podniz nije pronađen ili 0 Ako nije pronađen.

**Primjer:**  
`InStr("The quick brown fox","quick")`  
Evalues 5

`InStr("repEated","e",3,vbBinaryCompare)`  
Procjenjuje 7

----------
### <a name="instrrev"></a>InStrRev

**Opis:**  
Funkcija InStrRev pronalazi posljednjeg pojavljivanja podniz u nizu.

**Sintaksa:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- stringcheck: niz za pretraživanje
- stringmatch: niz pronađen
- pokretanje: početni položaj da biste pronašli podniz
- Usporedba: vbTextCompare ili vbBinaryCompare

**Napomena:**  
Vraća položaj gdje podniz nije pronađen ili 0 Ako nije pronađen.

**Primjer:**  
`InStrRev("abbcdbbbef","bb")`  
Vraća 7

----------
### <a name="isbitset"></a>IsBitSet

**Opis:**  
Funkcija IsBitSet testova ako je bit postavljena ili ne

**Sintaksa:**  
`bool IsBitSet(num value, num flag)`

- vrijednost: numeričku vrijednost koja je evaluated.flag: numerička vrijednost koja sadrži bitne na vrednovanje

**Primjer:**  
`IsBitSet(&HF,4)`  
Vraća True jer je bitna "4" Postavljanje u heksadecimalnu vrijednost "F"

----------
### <a name="isdate"></a>IsDate

**Opis:**  
Ako je izraz može biti daje u obliku datuma i vremena, a zatim funkcija IsDate se vrednuje kao True.

**Sintaksa:**  
`bool IsDate(var Expression)`

**Napomena:**  
Koristi se za određivanje ako CDate() može biti uspješno.

----------
### <a name="isempty"></a>IsEmpty

**Opis:**  
Ako atribut postoji u CS ili MV, ali se vrednuje kao prazni niz, zatim funkcija IsEmpty ima vrijednost True.

**Sintaksa:**  
`bool IsEmpty(var Expression)`

----------
### <a name="isguid"></a>IsGuid

**Opis:**  
Ako niz nije moguće pretvoriti u GUID, funkciju IsGuid vrednuje true.

**Sintaksa:**  
`bool IsGuid(str GUID)`

**Napomena:**  
GUID definirana je kao niz jednu od ovih uzoraka slijedeći: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx ili {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Koristi se za određivanje ako CGuid() može biti uspješno.

**Primjer:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Ako je na StrAttribute GUID oblik, vratite binarni prikaz, u suprotnom povrata Null.

----------
### <a name="isnull"></a>IsNull

**Opis:**  
Ako je izraz null, tada funkcija IsNull vraća true.

**Sintaksa:**  
`bool IsNull(var Expression)`

**Napomena:**  
Za atribut, tako da Izostanak atribut izražen je Null.

**Primjer:**  
`IsNull([displayName])`  
Vraća True ako je atribut ne postoji u CS ili MV.

----------
### <a name="isnullorempty"></a>IsNullOrEmpty

**Opis:**  
Ako je izraz null ili prazni niz, zatim IsNullOrEmpty funkcija vraća true.

**Sintaksa:**  
`bool IsNullOrEmpty(var Expression)`

**Napomena:**  
Za atribut, to bi vrednovani kao True ako atribut nema ili postoji, ali je prazan niz.  
Inverzija Ova funkcija je pod nazivom IsPresent.

**Primjer:**  
`IsNullOrEmpty([displayName])`  
Vraća True ako je atribut nema ili je prazan niz u CS ili MV.

----------
### <a name="isnumeric"></a>IsNumeric

**Opis:**  
Funkcija IsNumeric vraća Booleova vrijednost koja označava hoće li se izraz moguće vrednovati kao vrstu broja.

**Sintaksa:**  
`bool IsNumeric(var Expression)`

**Napomena:**  
Koristi se za određivanje ako CNum() može biti uspješno raščlaniti izraz.

----------
### <a name="isstring"></a>IsString

**Opis:**  
Ako je izraz moguće vrednovati kao vrsta niza, zatim funkciju IsString ima vrijednost True.

**Sintaksa:**  
`bool IsString(var expression)`

**Napomena:**  
Koristi se za određivanje ako CStr() može biti uspješno raščlaniti izraz.

----------
### <a name="ispresent"></a>IsPresent

**Opis:**  
Ako je izraz niz koji nije Null i nije prazno, zatim IsPresent, funkcija vraća true.

**Sintaksa:**  
`bool IsPresent(var expression)`

**Napomena:**  
Inverzija Ova funkcija je pod nazivom IsNullOrEmpty.

**Primjer:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

----------
### <a name="item"></a>Stavke

**Opis:**  
Funkcija stavke vraća jednu stavku s više vrijednosti niza/atribut.

**Sintaksa:**  
`var Item(mvstr attribute, num index)`

- Atribut: atribut s više vrijednosti
- indeks: index stavki u nizu s više vrijednosti.

**Napomena:**  
Funkcija stavke je korisna zajedno s funkciju sadrži jer potonjem, funkcija vraća indeksa na stavku u atribut s više vrijednosti.

Throws pogrešku Ako indeks je izvan granica.

**Primjer:**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Vraća adresu primarni e-pošte.

----------
### <a name="itemornull"></a>ItemOrNull

**Opis:**  
Funkcija ItemOrNull vraća jednu stavku s više vrijednosti niza/atribut.

**Sintaksa:**  
`var ItemOrNull(mvstr attribute, num index)`

- Atribut: atribut s više vrijednosti
- indeks: index stavki u nizu s više vrijednosti.

**Napomena:**  
Funkcija ItemOrNull je korisna zajedno s funkciju sadrži jer potonjem, funkcija vraća indeksa na stavku u atribut s više vrijednosti.

Ako je indeks izvan granica, vraća vrijednost Null.

----------
### <a name="join"></a>Uključivanje

**Opis:**  
Funkcija Join uzima s većim brojem vrijednosti niza i vraća niz pojedinačnim s navedenom razdjelnik trake za svaku stavku.

**Sintaksa:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

- Atribut: s većim brojem vrijednosti atributa koji sadrži nizova koji se spajaju.
- Graničnik: bilo koji niz, koji se koristi za razdvajanje podnizova u vraćenom nizu. Ako se ispusti, znak razmaka ("") koristi. Ako je graničnik niz nulte duljine ("") ili ništa, sve stavke na popisu se spajaju bez graničnika.

**Napomene**  
Postoji slične značajke između funkcije spoj i Podijeli. Funkcija Join uzima polja nizova i spaja ih niz graničnik da biste se vratili jednog niza. Funkcija Podijeli uzima niza i dijeli pri razdjelnika, da biste se vratili polja nizova. Međutim, ključne razlike je da spoj možete spajanje nizova s bilo koji niz graničnik Podijeli samo možete razdvojiti niz korištenjem znak razdjelnika.

**Primjer:**  
`Join([proxyAddresses],",")`  
Nije moguće vratiti:"SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

----------
### <a name="lcase"></a>LCase

**Opis:**  
Funkcija LCase svi znakovi u nizu u mala slova.

**Sintaksa:**  
`str LCase(str value)`

**Primjer:**  
`LCase("TeSt")`  
Vraća "testirajte".

----------
### <a name="left"></a>Lijevo

**Opis:**  
Funkcija Left vraća određeni broj znakova s lijeve strane niza.

**Sintaksa:**  
`str Left(str string, num NumChars)`

- niz: niz za vraćanje znakova iz
- NumChars: broj prepoznavanje broja znakova da biste se vratili na početku (lijevo) niza

**Napomena:**  
Niz koji sadrži znakove numChars prvi u nizu:

- Ako numChars = 0, vratiti prazan niz.
- Hoće li se numChars < 0, prikazivati ulazni niz.
- Ako je niz null, vratite se prazan niz.

Ako niz sadrži manje znakova od broja navedenog u numChars, vraća se niz koji su jednaki niz (koji je, koji sadrži sve znakove u parametru 1).

**Primjer:**  
`Left("John Doe", 3)`  
Vraća "Joh".

----------
### <a name="len"></a>LEN

**Opis:**  
Funkcija Len vraća broj znakova u nizu.

**Sintaksa:**  
`num Len(str value)`

**Primjer:**  
`Len("John Doe")`  
Vraća 8

----------
### <a name="ltrim"></a>Funkcije LTrim

**Opis:**  
Funkcije LTrim uklanja početne praznine iz niza.

**Sintaksa:**  
`str LTrim(str value)`

**Primjer:**  
`LTrim(" Test ")`  
Vraća "Test"

----------
### <a name="mid"></a>MID

**Opis:**  
Funkcija Mid vraća određeni broj znakova iz Navedeni položaj u nizu.

**Sintaksa:**  
`str Mid(str string, num start, num NumChars)`

- niz: niz za vraćanje znakova iz
- pokretanje: broj prepoznavanje početni položaj u nizu za vraćanje znakova iz
- NumChars: broj prepoznavanje broja znakova da biste se vratili na položaju u nizu

**Napomena:**  
Vraćanje znakova numChars počevši od početka položaj u nizu.  
Niz koji sadrži znakove numChars od početka položaj u nizu:

- Ako numChars = 0, vratiti prazan niz.
- Hoće li se numChars < 0, prikazivati ulazni niz.
- Ako start > duljinu niza znakova, vraćanje unos niza.
- Ako pokretanje < = 0, vraćanje unos niza.
- Ako je niz null, vratite se prazan niz.

Ako nema preostalo u nizu od početka položaj znakova numChar vrate broj znakova moguće.

**Primjer:**  
`Mid("John Doe", 3, 5)`  
Vraća "hn učinite".

`Mid("John Doe", 6, 999)`  
Vraća "N."

----------
### <a name="now"></a>Sada

**Opis:**  
Funkcija Now vraća DateTime koji određuje trenutni datum i vrijeme, na temelju datum i vrijeme na vašem računalu.

**Sintaksa:**  
`dt Now()`

----------
### <a name="numfromdate"></a>NumFromDate

**Opis:**  
Funkcija NumFromDate vraća datum u obliku datuma AD.

**Sintaksa:**  
`num NumFromDate(dt value)`

**Primjer:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Vraća 129699324000000000

----------
### <a name="padleft"></a>PadLeft

**Opis:**  
Na PadLeft funkcija lijevo – miševe niz za određene duljine pomoću udaljenosti od ruba uneseni znak.

**Sintaksa:**  
`str PadLeft(str string, num length, str padCharacter)`

- niz: niz u tipkovnicu.
- Duljina: cijeli broj koji predstavlja željenu duljinu niza znakova.
- padCharacter: niz koji se sastoji od znak da biste koristili kao znak tipkovnice

**Napomena:**

- Ako duljina niza je manji od duljine, zatim padCharacter je više puta dodaju na početak (lijevo) niza dok se ne sadrži duljinu jednako duljine.
- PadCharacter može biti znak razmaka, ali ne može biti vrijednost null.
- Ako je duljina niza jednaka ili veća od duljine, vraća se ne mijenja niz.
- Ako niz sadrži duljinu veće od ili jednako duljine, vraća se niz koji su jednaki niz.
- Ako je duljina niza je manji od duljine, novi niz željeni duljine je vraća koje sadrže niz popunjena s na padCharacter.
- Ako je niz null, funkcija vraća prazni niz.

**Primjer:**  
`PadLeft("User", 10, "0")`  
Vraća "000000User".

----------
### <a name="padright"></a>PadRight

**Opis:**  
Na PadRight funkcija desno – miševe niz za određene duljine pomoću udaljenosti od ruba uneseni znak.

**Sintaksa:**  
`str PadRight(str string, num length, str padCharacter)`

- niz: niz u tipkovnicu.
- Duljina: cijeli broj koji predstavlja željenu duljinu niza znakova.
- padCharacter: niz koji se sastoji od znak da biste koristili kao znak tipkovnice

**Napomena:**

- Ako duljina niza je manji od duljine, zatim padCharacter TAB dodaje kraja (desno) niza dok je jednako duljine duljinu.
- padCharacter može biti znak razmaka, ali ne može biti vrijednost null.
- Ako je duljina niza jednaka ili veća od duljine, vraća se ne mijenja niz.
- Ako niz sadrži duljinu veće od ili jednako duljine, vraća se niz koji su jednaki niz.
- Ako je duljina niza je manji od duljine, novi niz željeni duljine je vraća koje sadrže niz popunjena s na padCharacter.
- Ako je niz null, funkcija vraća prazni niz.

**Primjer:**  
`PadRight("User", 10, "0")`  
Vraća "User000000".

----------
### <a name="pcase"></a>PCase

**Opis:**  
Funkcija PCase pretvara prvi znak svake razgraničen razmakom riječi u nizu u velika slova, a svi ostali znakovi pretvaraju se u mala slova.

**Sintaksa:**  
`String PCase(string)`

**Napomena:**

- Ova funkcija trenutno ne nudi proper kućište da biste pretvorili riječ koja je u potpunosti velika slova, kao što su akronim.

**Primjer:**  
`PCase("TEsT")`  
Vraća "Test".

`PCase(LCase("TEST"))`  
Vraća "Test"

----------
### <a name="randomnum"></a>RandomNum

**Opis:**  
Funkcija RandomNum Vraća slučajni broj između na određeni vremenski interval.

**Sintaksa:**  
`num RandomNum(num start, num end)`

- pokretanje: broj prepoznavanje donju granicu slučajne vrijednosti za generiranje
- Kraj: broj prepoznavanje gornju granicu slučajne vrijednosti za generiranje

**Primjer:**  
`Random(100,999)`  
Možete se vratiti 734.

----------
### <a name="removeduplicates"></a>RemoveDuplicates

**Opis:**  
Funkcija RemoveDuplicates uzima s većim brojem vrijednosti niza i provjerite je li svaka vrijednost jedinstveni.

**Sintaksa:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Primjer:**  
`RemoveDuplicates([proxyAddresses])`  
Vraća sanitized proxyAddress atribut gdje su uklonjene sve duplicirane vrijednosti.

----------
### <a name="replace"></a>Zamjena

**Opis:**  
Funkcija Replace zamjenjuje sva pojavljivanja niza u drugom nizu.

**Sintaksa:**  
`str Replace(str string, str OldValue, str NewValue)`

- niz: niza za zamjenu vrijednosti u.
- OldValue: Niz za traženje i zamjena.
- NovaVrijednost: Niz da biste zamijenili da biste.

**Napomena:**  
Funkcija prepoznaje sljedeće posebne nadimaka:

- \n – novi redak
- \r – prelazak u novi
- \t – kartica

**Primjer:**  
`Replace([address],"\r\n",", ")`  
Zamjenjuje riječ o CRLF zarez i razmak, a može dovesti do "Jedan Microsoft način, Redmond, WA, SAD"

----------
### <a name="replacechars"></a>ReplaceChars

**Opis:**  
Funkcija ReplaceChars zamjenjuje sve pojave znakova koji se nalaze u nizu ReplacePattern.

**Sintaksa:**  
`str ReplaceChars(str string, str ReplacePattern)`

- niz: niza da biste zamijenili znakove.
- ReplacePattern: niz koji sadrži rječnik sa znakovima da biste zamijenili.

Oblik nije {source1}: {target1}, {source2}: {target2}, {sourceN}, {targetN} gdje je izvor znak da biste pronašli i ciljnog niza da biste zamijenili.

**Napomena:**

- Funkcija uzima svakog pojavljivanja definirani izvora i zamjenjuje na ciljnih web-mjesta.
- Izvorišnog web-mjesta mora biti točno jedan znak (unicode).
- Izvorišnog web-mjesta ne može biti prazna ili dulje od jednog znaka (pogreške u analizi).
- Cilj može sadržavati više znakova, na primjer ö:oe, β:ss.
- Cilj može biti prazna koja označava da se ukloniti znak.
- Izvor velika i mala slova i mora biti potpuno podudaranje.
- (Zarez) i: (dvotočka) rezervirane znakova i ne mogu zamijeniti korištenju ove funkcije.
- Razmaci i druge bijeli znaka u nizu ReplacePattern zanemaruju se.

**Primjer:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Vraća Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Vraća "ONeil", jedan crtičnih definiran koje će biti uklonjene.

----------
### <a name="right"></a>Desno

**Opis:**  
Funkcija Right vraća određeni broj znakova s desne strane (završetak) niza.

**Sintaksa:**  
`str Right(str string, num NumChars)`

- niz: niz za vraćanje znakova iz
- NumChars: broj prepoznavanje broja znakova da biste se vratili na kraju (desno) niza

**Napomena:**  
Znakovi NumChars vraćaju se od posljednjeg mjesta niza.

Niz koji sadrži posljednje numChars znaka u nizu:

- Ako numChars = 0, vratiti prazan niz.
- Hoće li se numChars < 0, prikazivati ulazni niz.
- Ako je niz null, vratite se prazan niz.

Ako niz sadrži manje znakova od broja navedenog u NumChars, vraća se niz koji su jednaki niz.

**Primjer:**  
`Right("John Doe", 3)`  
Vraća "N.".

----------
### <a name="rtrim"></a>RTrim

**Opis:**  
Funkcija RTrim uklanja završne bijele razmake iz niza.

**Sintaksa:**  
`str RTrim(str value)`

**Primjer:**  
`RTrim(" Test ")`  
Vraća "Test".

----------
### <a name="split"></a>Podjela

**Opis:**  
Funkcija Podijeli uzima niz odvojite razdjelnika i olakšava niz s više vrijednosti.

**Sintaksa:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

- vrijednost: niz s znak razdjelnika za razdvajanje.
- Graničnik: pojedinačni znak koji će se koristiti kao graničnik.
- ograničenje: maksimalni broj vrijednosti koje je moguće vratiti.

**Primjer:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Vraća niz s više vrijednosti s 2 elementima korisne su za atribut proxyAddress.

----------
### <a name="stringfromguid"></a>StringFromGuid

**Opis:**  
Funkcija StringFromGuid vodi binarni GUID i pretvara ga u nizu

**Sintaksa:**  
`str StringFromGuid(bin GUID)`

----------
### <a name="stringfromsid"></a>StringFromSid

**Opis:**  
Funkcija StringFromSid pretvara bajt polja koja sadrži sigurnosni identifikator niza.

**Sintaksa:**  
`str StringFromSid(bin ObjectSID)`  

----------
### <a name="switch"></a>Promjena

**Opis:**  
Funkcija Switch se koristi da biste se vratili jednu vrijednost na temelju uvjeta provjeriti u odnosu.

**Sintaksa:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- izraz: izraz Variant koju želite analizirat.
- vrijednost: vrijednost koja se vraća ako je pridruženi izraz True.

**Napomena:**  
Na popisu za promjenu funkcija argument sastoji se od parova izraza i vrijednosti. Izrazi se vrednuju slijeva nadesno, a vraća vrijednost povezan s prvim izrazom se vrednuje na True. Ako dijelovi nisu pravilno upareni, pojavljuje se pogreška pri izvođenju.

Na primjer, ako Izraz1 je True, promjenu prikazuje vrijednost1. Ako je izraz 1 False, ali izraz 2 je True, promjenu vraća vrijednost 2 i tako dalje.

Promjena vraća u Nothing ako:

- Izrazi nijedna vrijednost True.
- Prvi True izraz sadrži odgovarajuću vrijednost koja je Null.

Promjenu vrednuje sve izraze, čak i ako se prikazuje samo jedan od njih. Zbog toga treba pogledajte za nuspojave. Na primjer, ako vrednovanju bilo koji izraz rezultira dijeljenja nulom, koja su pojavljuje se pogreška.

Vrijednost može biti funkciju pogreške koje bi vratilo prilagođen niz.

**Primjer:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Vraća jezika u kojima se govori u nekim glavne gradova, u suprotnom vraća pogrešku.

----------
### <a name="trim"></a>Funkcija TRIM

**Opis:**  
Funkcija Trim uklanja suvišne razmake bijelo iz niza i.

**Sintaksa:**  
`str Trim(str value)`  

**Primjer:**  
`Trim(" Test ")`  
Vraća "Test".

`Trim([proxyAddresses])`  
Uklanja suvišne razmake za svaku vrijednost iz atributa proxyAddress i.

----------
### <a name="ucase"></a>UCase

**Opis:**  
Funkcija UCase pretvara sve znakove u nizu u velika slova.

**Sintaksa:**  
`str UCase(str string)`

**Primjer:**  
`UCase("TeSt")`  
Vraća "TEST".

----------
### <a name="word"></a>Word

**Opis:**  
Funkcija Word vraća riječi koje se nalaze u nizu na temelju parametara s opisom graničnike koristiti i broj riječi da biste se vratili.

**Sintaksa:**  
`str Word(str string, num WordNumber, str delimiters)`

- niz: niza da biste se vratili u programu word iz.
- WordNumber: broj prepoznavanje koji word broj mora vratiti.
- razdjelnici: niz koji predstavlja delimiter(s) koji želite koristiti za prepoznavanje riječi

**Napomena:**  
Svaki niz znakova u nizu odvojene jedan znakove u graničnike se smatraju riječi:

- Ako je broj < 1, vraća se prazan niz.
- Ako je niz null, vraća se prazan niz.

Ako niz sadrži manje od broja riječi ili niz sadrže sve riječi otkrije graničnike, vraća se prazan niz.

**Primjer:**  
`Word("The quick brown fox",3," ")`  
Vraća "Gojazni"

`Word("This,string!has&many separators",3,",!&#")`  
Bi vratilo "je"

## <a name="additional-resources"></a>Dodatni resursi

* [Objašnjenje deklarativno izraza za dodjelu resursa](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD povezivanje sinkronizacije: Mogućnosti za prilagodbu sinkronizacije](active-directory-aadconnectsync-whatis.md)
* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
