
<properties
    pageTitle="Korištenje Azure AD povezivanje stanja AD fs | Microsoft Azure"
    description="To je stranica stanja povezivanje Azure AD kako nadzirati lokalnih infrastruktura za AD FS."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-with-ad-fs"></a>Korištenje Azure AD povezivanje stanja AD fs
Sljedeća dokumentacija je za nadzor infrastruktura za AD FS s Azure AD povezivanje stanja. Informacije o Azure AD Connect (Sinkroniziranje) s stanjem povezivanje Azure AD za nadzor, potražite u članku [Korištenje Azure AD povezivanje stanja sinkronizacije](active-directory-aadconnect-health-sync.md). Uz to, informacije o Active Directory Domain Services s povežite zdravlje Azure AD za nadzor, potražite u članku [Korištenje Azure AD povezivanje stanje s AD DS](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Upozorenja za AD FS
U odjeljku Azure AD povezivanje upozorenja o stanju daje popis aktivni upozorenja. Svakom upozorenju obuhvaća sve relevantne podatke, razlučivost koraka i veze s povezanim dokumentaciju.

Dvokliknite upozorenje aktivna ni riješi, da biste otvorili novi plohu s dodatnim informacijama, koraci koje možete poduzeti da biste riješili upozorenja i veze odgovarajuće dokumentacije. Možete pogledati i povijesne podatke na upozorenja koja je Riješeno u prošlosti.

![Portal stanja za Azure AD Connect](./media/active-directory-aadconnect-health/alert2.png)



## <a name="usage-analytics-for-ad-fs"></a>Analitičkih podataka za AD FS
Azure AD povežite zdravlje analitičkih analizira promet za provjeru autentičnosti poslužitelja vanjski pristup. Dvokliknite okvir analize korištenja da biste otvorili plohu analize korištenja koji pokazuje nekoliko mjernih podataka i grupiranja.

>[AZURE.NOTE] Da biste koristili analitičkih AD fs, osigurati ADFS Provjera je li omogućena. Dodatne informacije potražite u članku [Omogućivanje nadzora za AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).

![Stanje Portal za Azure AD Connect](./media/active-directory-aadconnect-health/report1.png)

Da biste odabrali dodatne metriku, navesti vremenski raspon ili promijenili grupiranje, desnom tipkom miša kliknite grafikon analize korištenja i odaberite Uređivanje grafikona. Zatim možete navesti vremenski raspon, odaberite drugu mjerenje i promjena grupiranja. Možete pogledati raspodjele prometa provjere autentičnosti na temelju različite "metrike" i grupirati svaki metriku pomoću parametara odgovarajući "Grupiraj po" što je opisano u sljedećoj tablici:

| Mjerenja | Grupiraj prema | Znači koje grupiranja i zašto je korisno? |
| ------ | -------- | -------------------------------------------- |
| Ukupno zahtjeva: Ukupan broj zahtjeva za obradili servis za vanjski pristup | Sve | Prikazuje broj ukupan broj zahtjeva bez grupiranja. |
|  | Aplikacija | Grupa zahtjeva za Ukupno na temelju ciljano relying strana. Takvo grupiranje koristan je znati koja će se aplikacija prima koliki postotak ukupni promet. |
|  | Poslužitelj | Grupa Ukupno zahtjeva koji se temelji na poslužitelju na kojem se obrađuju zahtjev. Takvo grupiranje koristan je razumjeti distribucija učitavanja ukupni promet. |
|  | Uključivanje radnom mjestu | Grupa ukupni zahtjeve uskoro s uređaja koji su radnom mjestu združeni na temelju (poznata). Takvo grupiranje koristan je znati ako resurse je moguće pristupiti pomoću uređaja koji su poznati infrastruktura za identitet. |
|  | Način provjere autentičnosti | Grupa zahtjeva za Ukupno načina provjere autentičnosti koji se koristi za provjeru autentičnosti na temelju. U ovom grupiranja koristan je da biste shvatili uobičajenih metoda provjere autentičnosti koja se koristi za provjeru autentičnosti. Slijede metoda moguće provjere autentičnosti <ol> <li>Provjera autentičnosti integrirani sustava Windows (Windows)</li> <li>Provjera autentičnosti (obrasci) utemeljena na obrascima</li> <li>SSO (jedine prijave)</li> <li>X509 potvrda autentičnosti (potvrda)</li> <br>Ako vanjskim poslužiteljima primiti zahtjev s programa SSO kolačića, taj zahtjev broje se kao SSO (jedinstvenu prijavu). U tim slučajevima, ako kolačić nije valjan, korisnik ne zatraži da unesete vjerodajnice i dobiti objedinjenog pristup aplikaciji. To je uobičajena ako imate više relying stranama zaštićen vanjskim poslužiteljima. |
|  | Mrežno mjesto | Grupa Ukupno zahtjeva koji se temelji na mrežnom mjestu korisnika. Može biti bilo intranetu ili ekstranet. U ovom grupiranje je korisno je znati koliki postotak promet dolazi s intraneta nasuprot ekstranet. |
| Ukupno zahtjeva nije uspjelo: Ukupan broj nije uspjelo zahtjeve obradili servis za vanjski pristup. <br> (Ovo mjerenje dostupna je samo na AD FS za Windows Server 2012 R2)| Vrsta pogreške | Prikazuje broj pogrešaka na temelju vrste unaprijed definirane pogreške. U ovom grupiranja koristan vrste uobičajenih pogrešaka. <ul><li>Netočan korisničko ime ili lozinka: pogreške zbog netočan korisničko ime i lozinku.</li> <li>"Ekstranet Lockout": pogreške zbog zahtjeva primljenih od korisnika koji je zaključan iz ekstranet </li><li> "Istekla lozinka": pogreške zbog korisnici prijave pomoću istekle lozinke.</li><li>"Onemogućen račun": pogreške zbog korisnici prijave pomoću računa za onemogućene.</li><li>"Uređaja i provjera autentičnosti": pogreške zbog korisnici ne uspijeva za provjeru autentičnosti pomoću provjere autentičnosti uređaja.</li><li>"Provjera autentičnosti potvrda korisnika": pogreške zbog korisnicima daje autentičnost zbog koji nisu valjani certifikat.</li><li>"MFA": pogreške zbog korisnik ne uspijeva za provjeru autentičnosti pomoću višestruke provjere autentičnosti.</li><li>"Druge vjerodajnica": "Izdavanja autorizacije": pogreške zbog pogreške za autorizaciju.</li><li>"Izdavanja delegiranje": pogreške zbog pogreške delegiranje izdavanja.</li><li>"Token prihvaćanje": pogreške zbog ADFS odbijanje token davatelja identiteta treće strane.</li><li>"Protokol": nije uspjelo zbog pogreške protokola.</li><li>"Nepoznato": Uhvatite sve. Bilo koji drugi problemi uklapaju u definirane kategorije.</li> |
|  | Poslužitelj | Grupe pogreške koji se temelji na poslužitelju. Takvo grupiranje koristan je razumjeti raspodjele pogreške na poslužiteljima. Nejednaki raspodjele može biti pokazatelja poslužitelja u neispravan stanju. |
|  | Mrežno mjesto | Grupa pogreške na temelju mrežno mjesto zahtjeva (intranet Dodavanje veze za vanjskih ekstranet). Takvo grupiranje koristan je razumjeti vrstu zahtjevi za koje se ne uspijeva. |
|  | Aplikacija | Grupa pogreške koji se temelji na ciljano aplikacije (potrebe za oslanjanjem sudionika). Takvo grupiranje koristan je znati koja će se aplikacija ciljano vidi Većina broj pogrešaka. |
| Prosječan broj jedinstvenih korisnika aktivan u sustavu broj korisnika: | Sve | Ova metrika sadrži broj Prosječan broj korisnika putem servisa za vanjski pristup u isječak odabranog vremena. Korisnici su grupirani. <br>Prosjek ovisi o vrijeme isječak odabran. |
|  | Aplikacija | Grupa Prosječan broj korisnika na temelju ciljano aplikacije (potrebe za oslanjanjem sudionika). U ovom grupiranja koristan je da biste shvatili koliko korisnici koriste koja će se aplikacija. |


## <a name="performance-monitoring-for-ad-fs"></a>Performanse nadzor za AD FS
Azure AD povežite zdravlje praćenje performansi opisi nadzora mjernih podataka. Potvrđivanjem okvira nadzor, otvorit će se novi plohu detaljne informacije o metriku.


![Portal stanja za Azure AD Connect](./media/active-directory-aadconnect-health/perf1.png)


Odabirom mogućnosti filtra pri vrhu na plohu možete filtrirati prema server da biste vidjeli metriku pojedinačnom poslužitelju. Da biste promijenili metriku, desnom tipkom miša kliknite nadzora bojama nadzora plohu i odaberite Uređivanje grafikona. Zatim iz novi plohu koji se otvara možete odabrati dodatne metriku s padajućeg popisa i navesti vremenski raspon za prikaz podataka o performansama.

## <a name="reports-for-ad-fs"></a>Izvješća za AD FS
Stanje povezivanje Azure AD nudi izvješća o aktivnosti i performanse AD FS. Ta izvješća pomoć administratorima dobiti uvid u aktivnosti na njihovim poslužiteljima AD FS.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>Prvih 50 korisnika s nije uspjelo prijave korisničkog imena i lozinke

Jedan od najčešćih razloga za zahtjev za neuspjele provjere autentičnosti na poslužitelju komponente AD FS je na zahtjev vjerodajnice nisu valjane, odnosno pogrešan korisničko ime ili lozinka. Obično događa korisnicima zbog složene lozinki, zaboravljene lozinke i pravopisnih pogrešaka.

No postoje drugi razloga zbog kojih može uzrokovati neočekivane broj zahtjeva za se obrađuje poslužitelja za AD FS, kao što su: aplikaciju predmemorije korisničke vjerodajnice i vjerodajnice istječe ili zlonamjerni korisnik pokuša da biste se prijavili u račun s nizom poznati lozinke. U ovim se primjerima dva su valjane razloga zbog kojih može dovesti do stabilizatorom u zahtjevima.

Azure AD povezivanje stanja za ADFS nudi izvješće o vodećih 50 korisnika s pokušaja nije uspjelo prijave zbog koji nisu valjani korisničko ime i lozinku. Izvješće postiže se obrade događaje nadzora generira svim AD FS poslužiteljima u farmi poslužitelja

![Portal stanja za Azure AD Connect](./media/active-directory-aadconnect-health-adfs/report1a.png)

U ovom izvješću jednostavno možete pristupiti na sljedeći dijelovi informacija:

- Ukupan broj neuspjelih zahtjeva s pogrešan korisničkog imena i lozinke u zadnjih 30 dana
- Prosječna # korisnika kojima nije uspjelo neispravni korisničkog imena i lozinke prijava po danu.


Klikom na taj dio vodi vas na plohu površinski koje pruža dodatne detalje. U ovom plohu sadrži grafikon s trendova informacije pomoću kojih se uspostaviti osnovne o zahtjevi s pogrešan korisničko ime i lozinku. Uz to, na popisu prvih 50 korisnika pruža broj neuspjelih pokušaja.

Na grafikonu nalaze se sljedeće informacije:

- Broj koji nije uspjelo prijave zbog neispravni korisničkog imena i lozinke na temelju po danu.
- Ukupni broj jedinstvenih korisnika nije uspjelo prijave na temelju po danu.
- Klijent IP adresa za zadnji zahtjev

![Portal stanja za Azure AD Connect](./media/active-directory-aadconnect-health-adfs/report3a.png)

Izvješće sadrži sljedeće podatke:

| Stavke izvješća | Opis
| ------ | -------- |
|ID korisnika| Prikazuje ID korisnika koji je korišten. Ta vrijednost je što je korisnik unio, koji u nekim slučajevima je pogrešan korisnički ID koji se koristi.|
|Nije uspjelo pokušaji| Prikazuje broj koji nije uspjelo pokušaja za taj određeni korisničkog ID-a U tablici su sortirani s najveći broj neuspjelih pokušaja silaznim redoslijedom.|
|Zadnji pogreške| Pokazuje vrijeme kada je došlo je do posljednje pogreške.
|Zadnji neuspjeh IP | Prikazuje klijent IP adresu s najnovijim neispravan zahtjev.|



>[AZURE.NOTE] Izvješće automatski će se ažurirati nakon što se svaka dva sata novim podacima koji se prikupljaju unutar tog razdoblja. Kao rezultat pokušaja prijave unutar zadnja dva sata možda nije uključeno u izvješću.



## <a name="related-links"></a>Srodne veze

* [Azure AD Connect stanja](active-directory-aadconnect-health.md)
* [Stanje Agent za instalaciju za Azure AD Connect](active-directory-aadconnect-health-agent-install.md)
* [Stanje postupci za Azure AD Connect](active-directory-aadconnect-health-operations.md)
* [Korištenje stanja povezivanje Azure AD za sinkronizaciju](active-directory-aadconnect-health-sync.md)
* [Korištenje Azure AD povezivanje stanje s AD DS](active-directory-aadconnect-health-adds.md)
* [Najčešća pitanja o stanju za Azure AD Connect](active-directory-aadconnect-health-faq.md)
* [Povijest verzija stanja za Azure AD Connect](active-directory-aadconnect-health-version-history.md)
