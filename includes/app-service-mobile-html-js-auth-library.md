###<a name="server-auth"></a>Kako: autentičnost kod davatelja usluge (tijek Server)

Da bi se mobilne aplikacije upravljanje postupkom provjere autentičnosti u aplikaciji, morate se registrirati aplikacije pomoću davatelja identiteta. Zatim na servisu Azure aplikacije, morate konfigurirati ID aplikacije i tajna koje ste dobili od davatelja usluga.
Dodatne informacije potražite u članku vodič [Dodaj provjere autentičnosti za aplikaciju].

Nakon što ste se registrirali davatelja identiteta, jednostavno pozvati metodu .login() s nazivom davatelja usluga. Na primjer, sljedeći kod Prijava pomoću servisa Facebook.

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

Ako koristite davateljem identiteta osim servisa Facebook, promijenite vrijednost proslijediti prijava navedene metode nešto od sljedećeg: `microsoftaccount`, `facebook`, `twitter`, `google`, ili `aad`.

U ovom slučaju aplikacije servisa za Azure upravlja provjere autentičnosti tijek OAuth 2.0 prikazivanjem na stranici prijava odabranog davatelja i generiranje token aplikacije servisa za provjeru autentičnosti nakon uspješne prijavu pomoću davatelja identiteta. Funkcija prijava po dovršetku vraća JSON objekt (korisnik) koji se izlaže korisnički ID i aplikacije servisa za provjeru autentičnosti token u polja ID korisnika i authenticationToken odnosno. Ovaj token možete predmemorirani i ponovno koristiti sve dok se što istekne.

###<a name="client-auth"></a>Kako: autentičnost kod davatelja usluge (klijent tijek)

Pokrenite aplikaciju možete i neovisno obratite davatelja identiteta i pa unijeti vraćeni token aplikacije servisa za provjeru autentičnosti. Ovaj tijek klijent omogućuje vam omogućuje na jednom sučelje za prijavu za korisnike ili radi dohvaćanja dodatnih korisničkih podataka od davatelja identiteta.

#### <a name="social-authentication-basic-example"></a>Društvene primjer osnovne provjere autentičnosti

U ovom se primjeru koristi klijentski Facebook SDK za provjeru autentičnosti:

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```
U ovom se primjeru pretpostavlja tokena koje ste dobili od davatelja odgovarajući SDK pohranjen u tokena varijabli.

#### <a name="microsoft-account-example"></a>Primjer Microsoftov Account

Sljedeći primjer koristi u Live SDK koji podržava jedinstvene prijave za aplikacije iz Windows trgovine pomoću Microsoftova Account:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});
```

U ovom se primjeru s Live povezati, koje se dodjeljuje aplikacije servisa tako da nazovete funkciju prijava dohvaća token.

###<a name="auth-getinfo"></a>Kako: Prikupite informacije o korisnika čija je autentičnost provjerena

Podatke za provjeru autentičnosti za trenutnog korisnika koje će biti dohvaćeni iz na `/.auth/me` krajnjoj točki sve metode AJAX-a.  Provjerite je li postavite na `X-ZUMO-AUTH` zaglavlje da biste token za provjeru autentičnosti.  Token za provjeru autentičnosti pohranjena u `client.currentUser.mobileServiceAuthenticationToken`.  Na primjer, da biste koristili dohvaćanje API-JA:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

Dohvaćanje dostupna je kao paketa npm ili za preuzimanje preglednika CDNJS. Nije moguće koristiti i jQuery ili neki drugi API AJAX-a za dohvaćanje podataka.  Podaci će biti primili kao JSON objekta.
