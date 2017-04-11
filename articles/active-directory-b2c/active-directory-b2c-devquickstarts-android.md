<properties
    pageTitle="Azure Active Directory B2C: Poziv API web-mjesto iz aplikacije za Android | Microsoft Azure"
    description="U ovom se članku objašnjava da biste stvorili Android 'popis obaveza' aplikacije koja se poziva Node.js web-mjesto API pomoću OAuth 2.0 nošenja tokena. Aplikacija za Android i na webu API pomoću Azure Active Directory B2C upravljanja identitetima korisnika i provjeru autentičnosti korisnika."
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure AD B2C: Poziv API web-mjesto iz aplikacije za Android

> [AZURE.WARNING] Pomoću ovog praktičnog vodiča zahtijeva neke važna ažuriranja posebno da biste uklonili korištenje ADAL Android za B2C.  Ne možemo namjeravate objaviti Osvježi upute za korištenje Azure AD B2C u aplikacijama za Android u sljedećem tjednu, a preporučujemo da držite do tog razdoblja.  No ako samo želite pokušati stvari odgovor, slobodno da biste nastavili s nastavku članka.



Pomoću B2C Azure Active Directory (Azure AD), možete dodati samostalno identiteta za napredne značajke upravljanja Android aplikacije i web-API-ji u nekoliko koraka kratki. U ovom se članku objašnjava da biste stvorili Android "popis obaveza" aplikacije koja se poziva Node.js web-mjesto API pomoću OAuth 2.0 nošenja tokena. Aplikacija za Android i na webu API pomoću Azure AD B2C upravljanja identitetima korisnika i provjeru autentičnosti korisnika.

U ovom brzi početak rada mora biti API zaštićen Azure AD pomoću B2C za rad u potpunosti web-mjesto. Ne možemo su ugrađeni jedno za .NET i Node.js za korištenje. U ovom walk-through pretpostavlja API na webu uzorka Node.js konfiguriran. Dodatne informacije potražite u članku [Azure AD B2C web API-JA za Node.js vodič](active-directory-b2c-devquickstarts-api-node.md).

Android klijenti koje moraju imati pristup zaštićeni resursa, Azure AD predstavlja na Active Directory provjera autentičnosti biblioteke (ADAL). Jedini Svrha ADAL je da biste olakšali aplikacije da biste dobili pristup tokena. Da bismo pokazali kako je lako, u ovom vodiču bismo ćete stvoriti popis aplikacija sa sustavom Android zadataka koje:

- Dobiti pristup tokeni koje mogu pozivati popis obaveza API [OAuth 2.0 protokol provjere autentičnosti](https://msdn.microsoft.com/library/azure/dn645545.aspx).
- Dohvaća popise zadataka korisnika.
- Znak više korisnika.

> [AZURE.NOTE] Ovaj članak ne obuhvaća kako implementirati prijavu, prijava i upravljanje profila pomoću Azure AD B2C. Usmjerena kako poziva web API-ji nakon provjere autentičnosti korisnika. Ako to već niste učinili, bi trebao počinjati s [.NET web app uvodni vodič](active-directory-b2c-devquickstarts-web-dotnet.md) dodatne informacije o osnovama Azure AD B2C.

## <a name="get-an-azure-ad-b2c-directory"></a>Početak u imeniku Azure AD B2C

Prije korištenja Azure AD B2C morate stvoriti direktorij ili smjernice. Direktorij je spremnik za sve korisnike, aplikacije, grupe i više. Ako ne postoji već, [stvorite direktorij B2C](active-directory-b2c-get-started.md) prije nego što nastavite u ovom vodiču.

## <a name="create-an-application"></a>Stvaranje aplikacije komponente

Ćete morati stvoriti aplikaciju u direktoriju B2C. Tako ćete dobiti Azure AD informacije potrebne za sigurnu komunikaciju s aplikacijom. Aplikacije i API na webu su predstavljeni jedan **ID aplikacije** u tom slučaju jer oni čine jedan logičke aplikacija. Da biste stvorili aplikaciju, slijedite [ove upute](active-directory-b2c-app-registration.md). Obavezno:

- Uključi **web-aplikacije**/**API na webu** u aplikaciji.
- Unesite `urn:ietf:wg:oauth:2.0:oob` kao **odgovor URL-a**. Je zadani URL za ovaj uzorak koda.
- Stvaranje **aplikaciji tajna** aplikacije i njezino kopiranje. Jer će vam kasnije. Imajte na umu da ta vrijednost mora biti [unijeti prespojni znak XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) mogli koristiti.
- Kopirajte **ID aplikacije** koji vam je dodijeljen aplikacije. Također ćete to kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Stvaranje police

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

U Azure AD B2C, svaki korisnički doživljaj definira [pravila](active-directory-b2c-reference-policies.md). Aplikacije sadrži tri identiteta sučelja: registracija za Office, prijavite se i prijavite se pomoću servisa Facebook.  Morate stvoriti jednu pravila svake vrste kao što je opisano u [članku referenca pravila](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Kada stvorite tri police, ne zaboravite:

- Odaberite **zaslonsko ime** i ostale registracije atribute Pravilnik za prijavu.
- Odaberite aplikaciju zahtjevima **zaslonsko ime** i **ID objekta** u svakoj pravila. Možete odabrati druge zahtjevima.
- Nakon stvaranja, kopirajte **naziv** svakog pravila. Treba imati prefiks `b2c_1_`.  Morat ćete te nazive pravila kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kada stvorite tri pravila, spremni ste za stvaranje aplikacije.

Imajte na umu da ovaj članak ne obuhvaća kako koristiti pravila koji ste upravo stvorili. Da biste saznali kako funkcioniraju pravilnici u Azure AD B2C, započnite [.NET web app uvodni vodič](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Preuzimanje kod

Kod za ovaj Praktični vodič [je spremljen na GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android). Da biste sastavili uzorka tijekom rada, možete [preuzeti skeleton projekt kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip). Mogu Kloniraj i na skeleton:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **Ćete morati preuzeti skeleton za dovršetak ovog praktičnog vodiča.** Zbog složenosti implementacijom potpuno funkcionalni aplikacija za Android u skeleton ima UX kod koji će se pokrenuti nakon dovršetka ovog praktičnog vodiča. To je mjera uštedu vremena za razvojne inženjere. Kod UX nije germane temu kako dodati B2C aplikacija sa sustavom Android.

Aplikaciju dovršene nije [dostupan kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip) ili u `complete` podružnice u istom spremištu.

Da biste sastavili s Maven, možete koristiti `pom.xml` na najvišoj razini.

  1. Slijedite korake u [odjeljku preduvjeti da biste postavili Maven za Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
  2. Postavljanje programa emulator s SDK 21.
  3. Idite u korijensku mapu u kojoj klonirana na repo.
  4. Pokrenite naredbu `mvn clean install`.
  5. Promijeniti direktorij u uzorku brzi početak rada `cd samples\hello`.
  6. Pokrenite naredbu `mvn android:deploy android:run`.

Vidjet ćete aplikaciju za pokretanje. Unesite korisničke vjerodajnice za testiranje za probu.

Paketi Java arhiva (POSUDU) će se i poslati pokraj paket Android arhiva (AAR).

## <a name="download-the-android-adal-and-add-it-to-your-android-studio-workspace"></a>Preuzmite Android ADAL i dodajte ga sa sustavom Android Studio radnog prostora

Imate mogućnosti za korištenje biblioteke u Android projektu:

* Izvorni kod možete koristiti da biste uvezli u biblioteku u Eclipse i veza u aplikaciji.
* Ako koristite Android Studio, možete koristiti oblik paketa AAR i referencu u binarne datoteke.

### <a name="option-1-binaries-via-gradle-recommended"></a>Mogućnost 1: Binarne datoteke putem Gradle (preporučeno)

U binarne datoteke možete pristupiti iz repo središnje Maven. Paketa AAR možete uvrstiti u projektu u Android Studio (na primjer, u `build.gradle`) na ovaj način:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

### <a name="option-2-aar-via-maven"></a>Mogućnost 2: AAR putem Maven

Ako koristite na `m2e` dodatak u Eclipse, možete odrediti ovisnost u vašem `pom.xml` datoteke:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

### <a name="option-3-source-via-git-last-resort"></a>Mogućnost 3: Izvora putem brojka (zadnjih razvrstajte)

Da biste dobili šifru izvora SDK putem brojka, unesite:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

Korištenje granu **Konvergencija.**

## <a name="set-up-your-configuration-file"></a>Postavljanje konfiguracijskoj datoteci

Konfiguracija koju ste postavili starijim na portalu B2C da biste konfigurirali Android projekta.

Otvaranje `helpes/Constants.java` i unesite vrijednosti za sljedeće:

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}


```
- `SCOPES`: Dosezi koje proći na poslužitelju na kojem želite da biste zatražili natrag s poslužitelja kada korisnik prijavi. Za pretpregled B2C proći `client_id`. No to se očekuje da biste promijenili `read scopes` u budućnosti. Ovaj dokument će se ažurirati kada koji se pojavljuje.
- `ADDITIONAL_SCOPES`: Dodatni dosega koji želite koristiti za svoju aplikaciju. Se očekuje koja će se koristiti u budućnosti.
- `CLIENT_ID`: ID aplikacije ste dobili na portalu.
- `REDIRECT_URL`: Preusmjeravanje gdje ste i očekivali token će se objaviti natrag.
- `EXTRA_QP`: Ništa želite proslijediti poslužitelja u obliku kodiran URL-om.
- `FB_POLICY`: Pravilo koje pozivate. Ovo je najvažnije dio za ovaj walk-through.
- `EMAIL_SIGNIN_POLICY`: Pravilo koje pozivate. Ovo je najvažnije dio za ovaj walk-through.
- `EMAIL_SIGNUP_POLICY`: Pravilo koje pozivate. Ovo je najvažnije dio za ovaj walk-through.

## <a name="add-references-to-android-adal-to-your-project"></a>Dodavanje reference na Android ADAL u projekt

> [AZURE.NOTE]  ADAL za Android koristi modelu koji se temelji na svrhu pozvati provjere autentičnosti. Reprodukcije "Rasporedi" aplikaciju za obavljanje posla. Ovaj cijelu uzorak i sve ADAL za Android, centre za upravljanje reprodukcije i prenositi informacije između njih.

Prvo, obavijestite Android o izgleda aplikacije, uključujući reprodukcije koji želite koristiti. Ove reprodukcije bit će objašnjena detaljno u nastavku ovog praktičnog vodiča.

Ažuriranje u projekt `AndroidManifest.xml` datoteku da biste uvrstili sve svoje reprodukcije:

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

Kao što vidite, definirajte pet aktivnosti. Koristit ćete sve te.

- `AuthenticationActivity`: To dolazi s ADAL i nudi web za prijavu u prikaz.
- `LoginActivity`: Prikazat će se pravila za prijavu i gumbi za svaki pravila.
- `SettingsActivity`: Koristi se za mijenjanje postavki aplikacije tijekom rada.
- `AddTaskActivity`: Omogućuje dodavanje zadataka na REST API-JA koji su zaštićeni po Azure AD.
- `ToDoActivity`: To je glavni aktivnosti koje prikazuje zadatke.

## <a name="create-the-sign-in-activity"></a>Stvaranje aktivnosti za prijavu

Stvorite glavni aktivnosti te ga pozovite `LoginActivity`.

Stvoriti datoteku pod nazivom `LoginActivity.java`.

Morate pokrenuti aktivnosti i dodati neke gumbe koji će kontrolirati vaše korisničko Sučelje. Ovo je poznatiji Android kod prije nego što ste napisali.

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}


```
Sada ste stvorili gumbi koje mogu pozivati na `ToDoActivity` namjera (koja se poziva ADAL kada je potrebno token). Odgovaranju pomoću aktivnosti kao referencu i dodatni parametar. Ovaj dodatni parametar prenosi tako da na `intent.putExtra()` način. Definirate `"thePolicy"` pomoću koje ste naveli u `Constants.java`. Ovo pravilo za pozivanje tijekom provjere autentičnosti govori namjera.

## <a name="create-the-settings-activity"></a>Stvaranje aktivnosti postavke

Ovo je aktivnost koja popunjava postavki korisničkog Sučelja.

Stvoriti datoteku pod nazivom `SettingsActivity.java` za jednostavno stvaranje, čitanje ažuriranje i brisanje operacije (CRUD).

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

## <a name="create-the-add-task-activity"></a>Stvaranje aktivnosti Dodavanje zadataka

Pomoću ove aktivnosti dodavanje zadatka na krajnjoj točki REST API-JA.

Stvoriti datoteku pod nazivom `AddTaskActivity.java` i pisanje na sljedeći način.

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}

```

## <a name="create-the-to-do-list-activity"></a>Stvaranje aktivnosti popis zadataka

Ovo je najvažnije aktivnosti. To da biste token s Azure AD za pravila i pomoću tog token pozvati poslužitelj zadatka REST API-JA.

Stvoriti datoteku pod nazivom `ToDoActivity.java` i pisanje na sljedeći način. (Pozivi bit će objašnjena kasnije.)

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}

```


 Možda ste primijetili to ovisi metode koje još niste još napisan. Sadrže `updateLoggedInUser()`, `clearSessionCookie()`, i `getTasks()`. Ćete one kasnije u ovom vodiču za pisanje. Sigurno možete zanemariti pogreške u Android Studio za sada.

Objašnjenje parametre:

  - `SCOPES`: Potreban, opsega pokušavate da biste zatražili pristup. Za pretpregled B2C to je isti kao `client_id`, no to se očekuje da biste promijenili u budućnosti.
  - `POLICY`: Pravila za kada želite provjere autentičnosti korisnika.
  - `CLIENT_ID`: Potrebna, dolazi s portala za Azure AD.
  - `redirectUri`: Možete postaviti se kao naziv paketa. Nije potrebno koristiti u `acquireToken` poziva.
  - `getUserInfo()`Da biste potražili li korisnik je već u predmemoriji: način. Parametar i opisuje Pitaj korisnika ako korisnik nije pronađen ili token pristup korisnika nije valjan. Ova metoda će biti napisani kasnije u ovom vodiču.
  - `PromptBehavior.always`: Olakšava traži vjerodajnice da biste preskočili predmemorije i kolačića.
  - `Callback`: Naziva se nakon što je kod autorizacije se razmjenjuju token. Imat će objekta `AuthenticationResult`, koji sadrži token pristup, datum isteka i ID tokena informacije.

> [AZURE.NOTE]  Microsoft Intune tvrtkin portal za aplikacije omogućuje komponentu broker, a možda instalirana na uređaj korisnika. Aplikaciju pristupate jedan prijavu (SSO) preko svih aplikacija na uređaju. Razvojni inženjeri treba očekivati da biste dopustili Intune. ADAL za Android će koristiti broker račun ako je stvorena u na autentikatora jednog korisničkog računa. Da biste koristili brokera, programer mora registrirati posebne `redirectUri` za broker da biste koristili. `redirectUri`je u obliku msauth://packagename/Base64UrlencodedSignature. Možete dobiti `redirectUri` aplikacije pomoću skripte `brokerRedirectPrint.ps1` ili pomoću poziv API `mContext.getBrokerRedirectUri()`. Potpis je povezan s potpisnog certifikata iz trgovine Google Play.

 Pomoću možete preskočiti broker korisnika:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```
> [AZURE.NOTE] Da biste smanjili složenost ovaj B2C brzi početak rada, ne možemo odlučili da biste preskočili broker u uzorku.

Nakon toga stvorite preglednika metode koje se token samostalno tijekom provjere autentičnosti pozive uz zadatak API.

U istom `ToDoActivity.java` datoteke, pisanje na sljedeći način.

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

Dodati i metode koje će se "Postavljanje" i "Dohvati" `AuthenticationResult` (koja sadrži vaš token) da biste na globalni `Constants`. Iako `ToDoActivity.java` koristi `sResult` u njegov tokova, morate dodati ove metode. Ako ne, vaše aktivnosti nećete moći pristupiti token za obavljanje posla (kao što su Dodavanje zadataka u `AddTaskActivity.java`).

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }


```
## <a name="create-a-method-to-return-a-user-identifier"></a>Stvaranje način da biste se vratili identifikator korisnika

ADAL za Android predstavlja korisnika u obliku na `UserIdentifier` objekt. Time se upravlja korisnika. Objekt možete koristiti da biste odredili hoće li se korisniku se koristi u pozive. Pomoću ove informacije koje možete ovise o predmemorije, umjesto nazovite novi na poslužitelj. Da biste olakšali taj, koju smo stvorili u `getUserInfo()` način koji vraća `UserIdentifier`. Možete koristiti s `acquireToken()`. Ne možemo i stvoriti na `getUniqueId()` način koji vraća ID `UserIdentifier` u predmemoriji.

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }

```

### <a name="write-helper-methods"></a>Pomoćnik za metode zapisivanja

Nakon toga pisanje neki postupci Pomoćnik za čišćenje kolačića i omogućuju `AuthenticationCallback`. Načini se koriste isključivo za uzorak da biste bili sigurni da sudjelujete u čisto stanje prilikom pozivanja na `ToDo` aktivnosti.

U istu datoteku pod nazivom `ToDoActivity.java`, pisanje na sljedeći način.

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }

```   

## <a name="call-the-task-api"></a>Pozivanje zadatka API-JA

Kada ste spremni privucite tokeni aktivnosti, možete napisati API-JA za pristup poslužitelju zadatka.

`getTasks`sadrži polja koja predstavlja zadaci u vašem poslužitelju.

Započnite `getTasks`.

U istu datoteku pod nazivom `ToDoActivity.java`, pisanje na sljedeći način.

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }

```

I pisati na način na koji će pokrenuti tablicama pri prvom pokretanju.

U istu datoteku pod nazivom `ToDoActivity.java`, pisanje na sljedeći način.

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }

```

Vidjet ćete kod potreban je neke dodatne načine za rad. Pisanje te dalje.

### <a name="create-an-endpoint-url-generator"></a>Stvaranje programa generator URL krajnje točke

Morate za generiranje URL krajnje točke koji će se povezujete s. To se u isti predmet.

**U istu datoteku** pod nazivom `ToDoActivity.java`, pisanje na sljedeći način.

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }

 ```


Imajte na umu dodati token za pristup zahtjev u kod koji se spominju u sljedećem odjeljku.

## <a name="write-some-ux-methods"></a>Pisanje neki postupci UX

Android potrebno je učiniti neke pozive funkcioniranje aplikacije. To su `createAndShowDialog` i `onResume()`. Ovo je poznatiji Android kod prije nego što ste napisali.

U istu datoteku pod nazivom `ToDoActivity.java`, pisanje na sljedeći način.

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }


```

Nakon toga možete upravljati vaše pozive dijaloški okvir.

U istu datoteku pod nazivom `ToDoActivity.java`, pisanje na sljedeći način.

```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }

```

Trebala bi se pojaviti na `ToDoActivity.java` datoteku koju kompilira. Cijeli projekt mora i sada Prevedi.

## <a name="run-the-sample-app"></a>Pokrenite aplikaciju uzorka

Na kraju, stvaranje i pokretanje aplikacije u Android Studio ili Eclipse. Prijavite se i prijavite se u aplikaciju. Stvaranje zadataka za prijavljeni korisnik. Odjava i ponovno prijavite kao drugi korisnik i stvaranje zadataka za tog korisnika.

Imajte na umu da se zadaci su pohranjene po korisniku na API-JA, jer se API izdvaja identitet korisnika iz primi token za pristup.

Za referencu, dovršene [prikazuje se kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip). Mogu Kloniraj i to iz GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-Android```


## <a name="important-information"></a>Važne informacije


### <a name="encryption"></a>Šifriranje

ADAL šifrira tokeni i trgovine u `SharedPreferences` prema zadanim postavkama. Možete pogledati u `StorageHelper` predmete da biste vidjeli detalje. Android uvedena **AndroidKeyStore za 4.3(API18)** sigurna pohrana privatni ključevi. ADAL koji se koristi za API18 i noviji. Ako želite koristiti ADAL za verzije na donjem SDK morate unijeti tajnu tipku po `AuthenticationSettings.INSTANCE.setSecretKey`.

### <a name="session-cookies-in-web-view"></a>Sesije kolačića u prikazu web-tablice

Android web-prikaz poništite kolačići sesiji nakon zatvaranja aplikacije. To možete riješiti s sljedeći kod uzorka.
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
[Saznajte više o tome da kolačići](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).
