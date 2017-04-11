<properties
    pageTitle="Pravila lozinke i ograničenja servisa Azure Active Directory | Microsoft Azure"
    description="U članku se opisuje pravila koja se primjenjuju na lozinke u Azure Active Directory, uključujući dopuštenih znakova, Duljina i isteka"
  services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Pravila lozinke i ograničenja servisa Azure Active Directory

U ovom se članku opisuju lozinki i složenosti pridružene korisničkim računima pohranjena u direktoriju Azure AD.

> [AZURE.IMPORTANT] **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName pravila koja se primjenjuju na sve korisničke račune

Svaki korisnički račun koji je potrebno da biste se prijavili sustav za provjeru autentičnosti Azure AD mora imati vrijednost atributa Glavno ime (UPN) jedinstveni korisnički povezanim s tim računom. Sljedeće tablica strukture na pravila koja primijeniti i na lokalni Active Directory izvorni termini korisničke račune (sinkronizirani s oblakom) i samo oblak korisničkih računa.

|   Svojstvo           |     Preduvjeti za UserPrincipalName  |
|   ----------------------- |   ----------------------- |
|  Dopušteni broj znakova    |  <ul> <li>A – Z</li> <li>-z </li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
|  Znakovi koji nisu dopušteni  | <ul> <li>Bilo koji '@' znak koji je razdvajanje korisničko ime s domenom.</li> <li>Ne može sadržavati razdoblja znaka "." odmah ispred na '@' simbol</li></ul> |
| Duljina ograničenjima  |       <ul> <li>Ukupno trajanje ne smije 113 znakova</li><li>64 znaka prije na ‘@’ simbol</li><li>48 znakova nakon na ‘@’ simbol</li></ul>

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Lozinki koje se odnose samo na cloud korisničkih računa

U sljedećoj tablici opisane postavke pravila dostupna lozinke koje se mogu primijeniti korisničke račune koji stvara i upravlja u Azure AD.

|  Svojstvo       |    Preduvjeti          |
|   ----------------------- |   ----------------------- |
|  Dopušteni broj znakova   |   <ul><li>A – Z</li><li>-z </li><li>0 – 9</li> <li>@# $ % ^ & * - _ ! + [] {} = & #124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
|  Znakovi koji nisu dopušteni   |       <ul><li>Unicode znakova</li><li>Razmaci</li><li> **Neprobojne lozinke**: ne smije sadržavati znak točka '. "odmah ispred na '@' simbol</li></ul> |
|   Ograničenja za lozinke | <ul><li>8 znakova minimalne i maksimalne 16 znakova</li><li>**Neprobojne lozinke**: zahtijeva 3 iz 4 od sljedećeg:<ul><li>Malim slovima</li><li>Velika slova</li><li>Brojevi (0 do 9)</li><li>Simboli (pročitajte članak ograničenja lozinku iznad)</li></ul></li></ul> |
| Trajanje isteka lozinke      | <ul><li>Zadana vrijednost: **90** dana </li><li>Vrijednost je moguće konfigurirati pomoću cmdleta skup MsolPasswordPolicy iz na Azure Active Directory Module za Windows PowerShell.</li></ul> |
| Obavijest o isteka lozinke |  <ul><li>Zadana vrijednost: **14** dana (prije isteka lozinke)</li><li>Vrijednost je moguće konfigurirati pomoću cmdleta skup MsolPasswordPolicy.</li></ul> |
| Istek lozinke |  <ul><li>Zadana vrijednost: **false** dana (označava taj Istek lozinke je omogućeno) </li><li>Vrijednost se može konfigurirati za pojedinačne korisničke račune pomoću cmdleta skup-MsolUser. </li></ul> |
|  Povijesti lozinke  | Posljednju lozinku nije moguće ponovno koristiti. |
|  Trajanje povijesti lozinke | Zauvijek |
|  Lockout računa | Nakon 10 uspije prijaviti pokušaja (lozinke koja nije valjana), korisnik će biti zaključan jedne minute. Daljnje netočan pokušaja prijave će zaključava korisnika za povećanje trajanje. |


## <a name="next-steps"></a>Daljnji koraci

* **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).
* [Upravljanje lozinkama s bilo kojeg mjesta](active-directory-passwords.md)
* [Funkcioniranje upravljanja lozinkom](active-directory-passwords-how-it-works.md)
* [Uvod u Mangement lozinke](active-directory-passwords-getting-started.md)
* [Prilagodba upravljanje lozinkama](active-directory-passwords-customize.md)
* [Najbolje prakse za upravljanje lozinke](active-directory-passwords-best-practices.md)
* [Kako doći do uvida radu s lozinku upravljanje izvješćima](active-directory-passwords-get-insights.md)
* [Upravljanje lozinkama najčešća Pitanja](active-directory-passwords-faq.md)
* [Otklanjanje poteškoća s upravljanje lozinkama](active-directory-passwords-troubleshoot.md)
* [uči više](active-directory-passwords-learn-more.md)
