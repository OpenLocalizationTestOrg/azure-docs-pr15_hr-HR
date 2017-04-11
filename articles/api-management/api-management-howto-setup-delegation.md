<properties 
    pageTitle="Delegiranje korisnika Registracija i proizvoda za pretplatu" 
    description="Saznajte kako delegatu Registracija i proizvoda pretplata trećoj strani u odjeljku Upravljanje Azure API-JA." 
    services="api-management" 
    documentationCenter="" 
    authors="antonba" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="antonba"/>

# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Delegiranje korisnika Registracija i proizvoda za pretplatu

Delegiranje omogućuje vam korištenje postojeće web-mjesto za rukovanje za razvojne inženjere znak-u/sign-up i pretplatu za proizvode umjesto pomoću ugrađene funkcije na portalu za razvojne inženjere. Time se omogućuje web-mjesta vlasništvo korisničkih podataka i izvođenje provjere valjanosti od ovih koraka u prilagođeni način.

## <a name="delegate-signin-up"> </a>Delegating za razvojne inženjere prijavu i odjavu

Da biste delegatu prijavu i odjavu na postojeće web-mjesto morate da biste stvorili krajnju točku posebno delegiranje na web-mjestu koji funkcionira kao ulazna točka za pretraživanje kao započeti s portala za upravljanje API-JA za razvojne inženjere zahtjev za razvojne inženjere.

Konačni tijek rada će se na sljedeći način:

1. Za razvojne inženjere klika na vezu za prijavu ili prijavu na portal za upravljanje API-JA za razvojne inženjere
2. Preglednik se preusmjerava na krajnjoj točki delegiranje
3. Delegiranje krajnje točke u povrat preusmjerava ili predstavlja korisničkog Sučelja koja vas pita korisnika za prijavu ili prijavu
4. Na uspjeha, korisnike se preusmjerava natrag na upravljanje API-JA za razvojne inženjere stranici portala se pokreće iz


Da biste započeli, recimo prvi upravljanje API za postavljanje međuverzije za usmjeravanje zahtjeve za razgovore putem vaše delegiranje krajnjoj točki. Publisher portalu za upravljanje API-JA na **Sigurnost** kliknite, a zatim kliknite karticu **delegiranje** . Potvrdite okvir da biste omogućili "Delegat prijave i registracije".

![Delegiranje stranice][api-management-delegation-signin-up]

* Odlučite što URL na krajnjoj točki posebno delegiranje bit će i unos u polje **URL delegiranje krajnjoj točki** . 

* Unutar **ključ za provjeru autentičnosti delegiranje** polja unesite tajna koja će se koristiti za izračun potpis dobili za potvrdu da biste bili sigurni zahtjev uistinu uskoro s upravljanjem API Azure. Možete kliknuti gumb **Generiranje** da bi API Managemnet slučajno stvaranje ključa za vas.

Sada je potrebno da biste stvorili **krajnju točku delegiranje**. Ima za izvođenje broj akcija:

1. Primanje zahtjeva u sljedećem obliku:

    > *http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl= {URL izvorišnu stranicu} & soli = {niz} & potpisa = {niz}*

    Parametri upita za slučaj prijava / prijave:
    - **operacija**: služi za identifikaciju koju vrstu delegiranje zahtjev je – samo možda ćete **Prijava** u tom slučaju
    - **returnUrl**: URL stranice gdje korisnik nakon klika na vezu za prijavu ili prijavu
    - **soli**: posebno soli niza koristi za računalstvo sigurnost raspršivanje
    - **potpisa**: izračunatom sigurnost Raspršivanje će se koristiti za usporedbu da biste sami izračunati raspršivanje

2. Provjerite da zahtjev dolaze iz Azure API Management (nije obavezno, ali se preporučuje za sigurnost)

    * Izračunavanje programa raspršivanje HMAC SHA512 niza na temelju **returnUrl** i **soli** parametre upita ([primjer kod navedene u nastavku]):
        > HMAC (**soli** + "\n" + **returnUrl**)
         
    * Usporedite raspršivanje iznad-izračunati vrijednost parametra upita **potpisa** . Ako se podudaraju s dva raspršivanja, prijeđite na sljedeći korak, u suprotnom odbiti zahtjev.

2. Provjerite je li primate zahtjev za prijavu u/znak-kopirane: parametra upita **operacija** će biti postavljeno na "**Prijava**".

3. Korisniku predstaviti korisničko Sučelje za prijavu ili prijavu

4. Ako je korisnik potpisivanja gore ćete morati stvoriti odgovarajući račun za njih u odjeljku Upravljanje API-JA. [Stvaranje korisnika] s API upravljanje REST API-JA. Kada to učinite, provjerite je li postavite korisničkog ID-a na isto je u spremištu korisnika ili ID-a koji vam može pratiti od.

5. Kada korisnik uspješno provjere autentičnosti:

    * [zatražiti token (SSO) za-jedinstvene prijave] putem API upravljanje REST API-JA

    * returnUrl parametra upita s dodavanjem SSO URL koji ste primili iz poziv API iznad:
        > https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url npr. 

    * Preusmjeravanje korisnika iznad proizvedeno URL-a

Osim postupka **Prijava** upravljanje računima možete izvesti i tako da slijedite prethodne korake i koristite neki od sljedećih postupaka.

-   **Promjena lozinke**
-   **ChangeProfile**
-   **CloseAccount**

Mora proći sljedećih parametara upita za postupke za upravljanje računom.

-   **operacija**: služi za identifikaciju koju vrstu delegiranje zahtjev je (Promjena lozinke, ChangeProfile ili CloseAccount)
-   **ID korisnika**: korisnički id računa za upravljanje
-   **soli**: posebno soli niza koristi za računalstvo sigurnost raspršivanje
-   **potpisa**: izračunatom sigurnost Raspršivanje će se koristiti za usporedbu da biste sami izračunati raspršivanje

## <a name="delegate-product-subscription"> </a>Prenošenjem proizvoda pretplate

Prenošenjem proizvoda pretplate funkcionira na sličan način da biste prenošenjem korisnika prijava/kopirane. Konačni tijek rada je na sljedeći način:

1. Za razvojne inženjere odabire proizvoda na portalu za upravljanje API-JA za razvojne inženjere i klikne gumb pretplati se
2. Preglednik se preusmjerava na krajnjoj točki delegiranje
3. Delegiranje krajnje izvodi korake pretplate potrebna proizvoda – to oko i entail preusmjeravanja na drugu stranicu da biste zatražili informacije o naplati, postavljanje dodatna pitanja ili jednostavno pohranjivanje informacija i nije potrebno ništa korisnika


Da biste omogućili funkciju, na stranici **delegiranje** kliknite **delegat proizvoda pretplate**.

Zatim provjerite je li krajnju točku delegiranje izvršava sljedeće radnje:


1. Primanje zahtjeva u sljedećem obliku:

    > *http://www.yourwebsite.com/apimdelegation?operation= {operacija} & ID = {proizvoda da biste se pretplatili} & korisnički ID = {korisnika upućivanje zahtjeva} & soli = {niz} & potpisa = {niz}*

    Parametri upita u slučaju pretplate proizvoda:
    - **operacija**: služi za identifikaciju je vrsti zahtjev za delegiranje. Za pretplatu na proizvod zahtjeva za valjane mogućnosti su:
        - "Pretplata": zahtjev da biste se pretplatili korisniku navedeni proizvod uz navedene ID-a (pogledajte informacije u nastavku)
        - "Otkazivanje pretplate": zahtjev za korisnika iz proizvoda
        - "Obnavljanje": requst za obnavljanje pretplate (npr. to može biti istječe)
    - **ID proizvoda**: ID proizvoda korisnika zatražili da biste se pretplatili
    - **ID korisnika**: ID korisnika čije postala je zahtjev
    - **soli**: posebno soli niza koristi za računalstvo sigurnost raspršivanje
    - **potpisa**: izračunatom sigurnost Raspršivanje će se koristiti za usporedbu da biste sami izračunati raspršivanje


2. Provjerite da zahtjev dolaze iz Azure API Management (nije obavezno, ali se preporučuje za sigurnost)

    * Izračunavanje programa SHA512 HMAC niza na temelju parametara **IDProizvoda**, **korisnički ID** i **soli** upita:
        > HMAC (**soli** + '\n' + **IDProizvoda** + '\n' + **korisnički ID**)
         
    * Usporedite raspršivanje iznad-izračunati vrijednost parametra upita **potpisa** . Ako se podudaraju s dva raspršivanja, prijeđite na sljedeći korak, u suprotnom odbiti zahtjev.
    
3. Izvođenje bilo koje obrada pretplate proizvoda ovise o vrsti postupka zatražili u **operacija** – npr naplatu dodatna pitanja, itd.

4. Na uspješno pretplata korisniku proizvoda s vaše strane, pretplatite se korisniku upravljanje API-JA proizvoda tako da [nazovete REST API -JA za pretplatu na proizvod].

## <a name="delegate-example-code"></a> Kod uzorka ##

Ove kod primjera pokazuju kako preuzeti *ključ za provjeru valjanosti delegiranje*, koji je na zaslonu delegiranje portala za publisher da biste stvorili HMAC koji se zatim koristi za provjeru valjanosti potpisa potvrđivanje valjanost proslijeđeni returnUrl. Istu šifru radi IDProizvoda i ID korisnika s izmjenom napraviti manje.

**C# kod da biste generirali raspršivanje returnUrl**

    using System.Security.Cryptography;

    string key = "delegation validation key";
    string returnUrl = "returnUrl query parameter";
    string salt = "salt query parameter";
    string signature;
    using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
    {
        signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
        // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
        // compare signature to sig query parameter
    }


**NodeJS kod da biste generirali raspršivanje returnUrl**

    var crypto = require('crypto');
    
    var key = 'delegation validation key'; 
    var returnUrl = 'returnUrl query parameter';
    var salt = 'salt query parameter';
    
    var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
    var digest = hmac.update(salt + '\n' + returnUrl).digest();
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
    
    var signature = digest.toString('base64');

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o delegiranje potražite u ovom videozapisu.

> [AZURE.VIDEO delegating-user-authentication-and-product-subscription-to-a-3rd-party-site]

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[zatražiti token (SSO) za-jedinstvene prijave]: http://go.microsoft.com/fwlink/?LinkId=507409
[Stvaranje korisnika]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[pozivanje REST API-JA za pretplatu na proizvod]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[primjer kod navedene u nastavku]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 