<properties
    pageTitle="Slanje sigurne automatske obavijesti s koncentratorima Azure obavijesti"
    description="Saznajte kako poslati sigurnu automatske obavijesti aplikacija za Android s Azure. Primjere koda pisane Java i C#."
    documentationCenter="android"
    keywords="automatske obavijesti, slanje obavijesti, slanje poruka, android automatske obavijesti"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

#<a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Slanje sigurne automatske obavijesti s koncentratorima Azure obavijesti

> [AZURE.SELECTOR]
- [Univerzalni za Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

##<a name="overview"></a>Pregled

> [AZURE.IMPORTANT] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Podrška za slanje obavijesti u Microsoft Azure omogućuje vam da biste pristupili programa jednostavno koristi multiplatform, skalirana izlaz automatskih poruka infrastruktura za koji za komercijalni ispis olakšava implementacije slanje obavijesti za korisnika i enterprise aplikacije za mobilne platforme.

Zbog propisima ili sigurnosnih ograničenja, ponekad aplikacije možda htjeti uključiti nešto u obavijesti koja se ne možete prenositi putem Infrastruktura standardne automatske obavijesti. Pomoću ovog praktičnog vodiča opisuje način da biste postigli iste sučelje slanjem povjerljivih podataka putem sigurne, čija je autentičnost provjerena veze između klijentskog uređaja sa sustavom Android i pozadinska aplikacija.

Visoke razine tijeka je kako slijedi:

1. Aplikacija pozadinske:
    - Služi za pohranu sigurne tereta u pozadinskoj bazi podataka.
    - Šalje ID obavijest Android uređaj (sigurne ne šalju nikakvi podaci).
2. Aplikacija na uređaju, prilikom primanja obavijesti:
    - Android uređaj kontakta pozadinske zahtijeva sigurnu opseg.
    - Aplikaciju možete prikazati opseg kao obavijest na uređaju.

Važno Imajte na umu da u prethodnom tijek (i u ovom ćete praktičnom vodiču), ne možemo pretpostavlja da uređaj pohranjuje token za provjeru autentičnosti u lokalno spremište kada se korisnik prijavljuje je. Time će se jamčiti potpuno objedinjenog doživljaj, kao što je uređaja možete dohvatiti na obavijest sigurno tereta pomoću ovaj token. Ako aplikacija pohranite tokeni za provjeru autentičnosti na uređaj ili ako je istekao tokena aplikaciju uređaj nakon primanje obavijesti automatske prikaz generički obavijesti postavljanja upita korisniku pokrenite aplikaciju. Aplikaciju pa potvrđuje korisnika i prikazuje opseg obavijesti.

Pomoću ovog praktičnog vodiča služi za slanje sigurne automatske obavijesti. Izgradnji vodič [Obavijestite korisnike](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) kako biste trebali biste dovršite korake u tom praktičnom vodiču najprije ako to već niste učinili.

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča pretpostavlja da ste stvorili i konfigurirali koncentratora za obavijesti, kao što je opisano u [Početak rada s obavijesti koncentratora (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Izmjena Android projekt

Sad kad koju ste izmijenili vaše aplikacije pozadinskih da biste poslali samo *id* automatske obavijesti, morate promijeniti Android aplikacije za tu obavijest, a pozvati vaše pozadinske dohvatiti sigurnu poruku da se prikazuje.
Da biste postigli cilja, imate da biste bili sigurni da aplikacija za Android ne zna radi provjere autentičnosti s vašeg pozadinske kad primi automatske obavijesti.

Ne možemo sada će promijeniti tijek *Prijava* da biste spremili vrijednost zaglavlja provjere autentičnosti u zajedničke postavke aplikacije. Analogna mehanizme može se koristiti za bilo koju provjere autentičnosti token (npr. OAuth tokene) koju aplikaciju morati koristiti bez korisničke vjerodajnice za pohranu.

1. U projektu aplikacija za Android, dodajte konstante pri vrhu klase **MainActivity** :

        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";

2. I dalje u predmete **MainActivity** ažuriranje na `getAuthorizationHeader()` način sadrži sljedeći kod:

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

            return basicAuthHeader;
        }

3. Dodajte sljedeće `import` naredbe pri vrhu datoteke **MainActivity** :

        import android.content.SharedPreferences;

Sada ćemo promijenit će se događajima koji se naziva primitku obavijesti.

4. U **MyHandler** klase promjena na `OnReceive()` način sadrži:

        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }

5. Zatim dodajte na `retrieveNotification()` metoda zamjene rezervirano mjesto `{back-end endpoint}` s krajnje pozadinske nabavili pri implementaciji sustava pozadinske:

        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }


Ovaj postupak poziva na aplikaciju pozadinsku dohvatiti sadržaju obavijesti putem vjerodajnicama pohranjenima u zajedničkoj Preference i prikazuje kao normalne obavijesti. Obavijesti izgleda korisniku aplikacije točno onako kao što su automatske obavijesti.

Imajte na umu da se preporučuje za rukovanje slučajeva nedostaje svojstvo zaglavlju provjera autentičnosti ili odbijanje po pozadinske. Posebno rukovanje tim slučajevima ovise o tome uglavnom vaš cilj korisničkog sučelja. Jedan je prikazati obavijest s generički upit za korisnika za provjeru autentičnosti za dohvaćanje stvarni obavijesti.

## <a name="run-the-application"></a>Pokrenite aplikaciju

Da biste pokrenuli aplikaciju, učinite sljedeće:

1. Provjerite je li **AppBackend** je uveden u Azure. Ako koristite Visual Studio, pokrenite aplikaciju za Web API **AppBackend** . Prikazat će se u ASP.NET web-stranicu.

2. U Eclipse, pokrenite aplikaciju na fizički uređaj sa sustavom Android ili na emulator.

3. U aplikaciji Android korisničkog Sučelja, unesite korisničko ime i lozinku. To može biti bilo koji niz znakova, ali moraju biti iste vrijednosti.

4. U aplikaciji Android korisničkog Sučelja, kliknite **prijavite se**. Zatim kliknite **Pošalji automatske**.
