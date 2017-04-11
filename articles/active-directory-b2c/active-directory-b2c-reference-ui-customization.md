<properties
    pageTitle="Azure Active Directory B2C: Korisničko sučelje (UI) prilagodbe | Microsoft Azure"
    description="Tema značajke prilagodbe korisničkog sučelja (UI) u Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: Prilagodba Azure AD B2C korisničko sučelje (UI)

Korisničko sučelje je paramount u aplikaciji potrošača dostupnog. Razlike između dobar aplikacije i sjajan jedan i između samo aktivni korisnici i doista aktivan je. Azure Active Directory (Azure AD) B2C omogućuje Prilagodba korisničke prijave, prijavu (*pogledajte napomenu u nastavku*), profila uređivanja i stranice s kontrolom za jedan piksel savršen za ponovno postavljanje lozinke.

> [AZURE.NOTE]
Trenutno lokalan prijavu stranica, accompaning lozinku ponovno postavljanje stranice i provjere e-pošte moguće je prilagoditi samo pomoću [tvrtke brendiranje značajke](../active-directory/active-directory-add-company-branding.md) , a ne prema mehanizme opisane u ovom članku.

U ovom se članku će se čitati o:

- Stranica korisničko sučelje (UI) prilagodbe značajku.
- Alat koji će vam pomoći testirati značajke prilagodbe korisničkog Sučelja stranice pomoću naš ogledni sadržaj.
- Osnovni elementi korisničkog Sučelja u svaku vrstu stranice.
- Najbolje prakse prilikom exercising ovu značajku.

## <a name="the-page-ui-customization-feature"></a>Značajka za prilagodbu korisničkog Sučelja stranice

Pomoću značajke za prilagodbu korisničkog Sučelja stranice možete prilagoditi izgled i dojam korisničke prijave, prijavu, lozinke i profila uređivanja stranice (konfiguriranjem [pravila](active-directory-b2c-reference-policies.md)). Na koje korisnici će imati objedinjenog sučelja pri premještanju s jednog računala i stranica posluživanje sustavom Azure AD B2C.

Za razliku od drugih servisa koji mogućnosti korisničkog Sučelja ograničeni su ili su dostupni putem API-ji samo Azure AD B2C koristi Moderna (i jednostavniji) pristup za prilagodbu korisničkog Sučelja stranice.

Evo kako to funkcionira: Azure AD B2C pokreće kod u pregledniku svoje korisničke i koristi Moderna pristup naziva [Zajedničkog korištenja više polazište resursa (CORS)](http://www.w3.org/TR/cors/) da biste učitali sadržaja s URL koji ste naveli u pravilo. Možete navesti različite URL-ova za različite stranice. Šifra spajanja elemente korisničkog Sučelja iz Azure AD B2C sa sadržajem učitati iz URL i prikazuje stranicu na potrošača. Sve što trebate učiniti jest:

1. Stvaranje ispravnog HTML5 sadržaja s na `<div id="api"></div>` element (mora biti prazna element) koja se nalazi negdje u na `<body>`. U ovom gdje se umeće sadržaj Azure AD B2C oznake element.
2. Smještaj sadržaja na krajnje HTTPS (s CORS je dopušteno).
3. Stil elemente korisničkog Sučelja koje Azure AD B2C umeće u.

## <a name="test-out-the-ui-customization-feature"></a>Testirajte značajku za prilagodbu korisničkog Sučelja

Ako želite isprobati značajku za prilagodbu korisničkog Sučelja pomoću naš uzorak HTML i CSS-a sadržaja, ne možemo ste koje vam je dao [Pomoćnik za jednostavne alata](active-directory-b2c-reference-ui-customization-helper-tool.md) koji se prenosi i konfigurira oglednog sadržaja u spremištu blobova Azure.

> [AZURE.NOTE]
Možete hostirati sadržaj korisničko Sučelje bilo kojeg mjesta: na web-poslužiteljima CDN-ovi, AWS S3 datotečnih zajedničko korištenje, itd. Pod uvjetom da sadržaj nalazi se na javno dostupna krajnjoj točki HTTPS (s CORS je dopušteno), koje su sve što je potrebno. Spremište blobova platforme Azure smo koristite opisne svrhe.

### <a name="the-most-basic-html-content-for-testing"></a>Osnovni HTML sadržaja za testiranje

Prikazano u nastavku je osnovni HTML sadržaja koje možete koristiti za testiranje tu mogućnost. Koristiti alat za Pomoćnik za navedene ranije da biste prenijeli i konfiguriranje sadržaja u spremištu blobova Azure. Možete provjeriti da osnovni, koje nisu stiliziranih gumbe i polja obrasca na svakoj stranici su prikazane i funkcionirati.

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>    <!-- IMP: This element is intentionally empty; don't enter anything here -->
    </body>
</html>

```

## <a name="the-core-ui-elements-in-each-type-of-page"></a>Osnovni elementi korisničkog Sučelja u svaku vrstu stranice

U sljedećim se odjeljcima tražit će Primjeri fragmentirane HTML5 koji spaja Azure AD B2C u na `<div id="api"></div>` element koji se nalazi u sadržaju. **Umetanje te fragmentirane u sadržaju HTML 5.** Servis za Azure AD B2C umeće ih u trenutku pokretanja. U ovim se primjerima koristite za dizajniranje vlastite listove stilova.

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>Azure AD B2C sadržaj "identiteta davatelja odabira zadatak"

Ova stranica sadrži popis davatelja identiteta koji korisnik na raspolaganju prilikom registracije ili prijave. To su davatelji identiteta za društvene kao što su Facebook i Google + ili lokalni računi (koja se temelji na e-pošte adresa ili korisničko ime).

```HTML

<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Azure AD B2C sadržaj umetnuta u "lokalan stranicu za prijavu"

Ova stranica sadrži obrazac za prijavu koje korisnik ima da biste unijeli prilikom prijave za lokalni račun koji se temelji na adresu e-pošte ili korisničko ime. Obrazac mogu sadržavati različite kontrole za unos kao što je okvir za unos teksta, okvir za unos lozinke, izborni gumb, jednu odaberite padajućih okvira i višestruki odabir potvrdne okvire.

```HTML

<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Azure AD B2C sadržaj umetnuta u "" računa za društvene stranicu za prijavu"

Ova stranica sadrži obrazac za prijavu koje korisnik ima da biste unijeli prilikom prijave pomoću davatelja identiteta društvenih kao što su Facebook ili Google + postojeći račun. Ova stranica je slična lokalan stranicu za prijavu (prikazan u prethodnom odjeljku) uz iznimku polja za unos lozinke.

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Azure AD B2C sadržaj "Unified registracije ili prijave zadatak"

Ova stranica rukuje i prijave i prijavu od koje korisnici koji mogu koristiti davatelji identiteta za društvene kao što su Facebook ili Google + ili lokalni računi.

```HTML

<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>Azure AD B2C sadržaj "višestruka provjera autentičnosti zadatak"

Na ovoj stranici Korisnici možete provjeriti njihove telefonskih brojeva (pomoću teksta ili glasovni) tijekom registracije ili prijave.

```HTML

<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>Azure AD B2C sadržaj "" Pogreška zadatak "

```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>

```

## <a name="things-to-remember-when-building-your-own-content"></a>Napomene kada Izrada vlastite sadržaja

Ako namjeravate koristiti značajku za prilagodbu korisničkog Sučelja stranice, pregledajte sljedeće najbolje prakse:

- Ne kopirajte Azure AD B2C zadani sadržaj web-mjesta i pokušati ga mijenjati. Preporučuje se da biste sastavili HTML5 sadržaj od nule, a da biste koristili zadani sadržaj kao referencu.
- Na svim stranicama (osim stranica s pogreškom) poslužena prijavu, prijavu i pravilnicima za uređivanje profila, listovi stilova koje ste omogućili morat će nadjačati zadane stilovima koji dodat ćemo u ove stranice u na <head> fragmenti. Na svim stranicama posluživanje sustavom prijavu ili prijavu i ponovno postavljanje lozinki i stranica s pogreškom na sva pravila, morat ćete unijeti sve stil sami.
- Sigurnosnih vam razloga ne možemo ne dopuštaju da biste obuhvatili sve JavaScript sadržaj. Većina što vam je potrebno mora biti dostupno od ukupnog okvir. Ako nije, koristite [Govorne korisnika](http://feedback.azure.com/forums/169401-azure-active-directory) da biste zatražili nove funkcije.
- Podržanim verzijama preglednika:
    - Internet Explorer 11
    - Internet Explorer 10
    - Internet Explorer 9 (Ograničeno)
    - Internet Explorer 8 (Ograničeno)
    - Google Chrome 43.0
    - Google Chrome 42.0
    - Mozilla Firefox 38.0
    - Mozilla Firefox 37.0
