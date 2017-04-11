<properties
   pageTitle="Objašnjenje programski Manifest Azure Active Directory | Microsoft Azure"
   description="Detaljne opseg programski manifest Azure Active Directory koji predstavlja konfiguracija identitet aplikacije u klijent za Azure AD te se koristi da biste olakšali OAuth autorizacije, pristanak sučelje i više."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="dkershaw;bryanla"/>

# <a name="understanding-the-azure-active-directory-application-manifest"></a>Objašnjenje programski manifest Azure Active Directory

Aplikacije koje se integrirati s Azure Active Directory (AD) mora biti registriran u klijent Azure AD pruža konfiguracije stalni identiteta za aplikaciju. Tu konfiguraciju je konzultirali prilikom izvođenja, omogućivanjem scenarija koji omogućuju aplikaciju spoljni i vi posrednik provjere autentičnosti/autorizacije kroz Azure AD. Dodatne informacije o aplikaciji modela Azure AD potražite u [Dodavanje, ažuriranje, i uklanjanje aplikacija] [ ADD-UPD-RMV-APP] članka.

## <a name="updating-an-applications-identity-configuration"></a>Ažuriranje aplikacije identiteta konfiguracije

Dostupno zapravo više mogućnosti ažuriranja svojstava o konfiguraciji identitet aplikacije, koji se razlikuju u mogućnostima i stupnjeva težine, uključujući sljedeće:

- Na ** [Azure klasični portalom] [ AZURE-CLASSIC-PORTAL] Web korisničko sučelje** omogućuje vam da biste ažurirali najčešće svojstva aplikacije. To je najbrži i najmanje pogreške podložni način ažuriranja svojstva vaše aplikacije, ali ne dobivate potpuni pristup svim svojstvima, kao što je sljedeća dva načina.
- Za naprednije scenarije gdje trebate ažurirati svojstva koja se ne pojavljuje se na portalu za Azure klasični, možete izmijeniti **manifestu aplikacije**. Ovo je žarište ovog članka i opisan u uveden u sljedećem odjeljku detaljnije.
- Također moguće je **Pisanje aplikacije koja koristi [API grafikonu] [ GRAPH-API] ** da biste ažurirali aplikacije koji su potrebni najviše truda. To može biti privlačne mogućnost iako, ako pišete softverom za upravljanje ili ćete morati ažurirati aplikaciju svojstva redovito automatiziranog način.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Pomoću programski manifest da biste ažurirali konfiguracije identitet aplikacije
Putem [Azure klasični portal][AZURE-CLASSIC-PORTAL], konfiguracije identitet vaše aplikacije, možete upravljati tako da preuzmete i prijenos na JSON datoteke predstavljanje koja se naziva i programski manifest. Nema stvarni datoteka je pohranjena u direktoriju. Programski manifest samo HTTP GET operaciju na Azure AD grafikonu API aplikacije entitet, a prijenos operaciju HTTP ZAKRPU na entitet aplikacije.

Kao rezultat da biste shvatili oblikovanje i svojstva manifesta aplikacije, morat ćete upućuju na grafikonu API [aplikacije entitet] [ APPLICATION-ENTITY] dokumentacije. Primjeri ažuriranja koja može izvršiti iako manifestu aplikacije prijenos obuhvaćaju sljedeće:

- **Objava dozvola opsega (oauth2Permissions)** koji prikazuje vaše API na webu. Potražite u temi "Će se web-API-ji za ostale aplikacije" u [Integriranje aplikacija pomoću servisa Azure Active Directory] [ INTEGRATING-APPLICATIONS-AAD] informacije o implementacijom Oponašanje korisnika pomoću na oauth2Permissions delegirani opsega dozvola. Kao što je već rečeno, svojstva entitet su navedenih u grafikonu API [entitet i složenoj] [ APPLICATION-ENTITY] referentni članak, uključujući svojstvo oauth2Permissions koji je zbirka vrste [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
- **Objava aplikacija ulogama (appRoles) koji prikazuje aplikacije**. Svojstvo appRoles entitet aplikacija je zbirka vrste [AppRole][APPLICATION-ENTITY-APP-ROLE]. U odjeljku [Kontrola pristupa u oblak aplikacije koje se koriste Azure AD na temelju uloga] [ RBAC-CLOUD-APPS-AZUREAD] članka na primjer implementacije.
- **Objava poznati klijentskim aplikacijama (knownClientApplications)**, koje omogućuju vam logično sve pristanak aplikacije navedeni klijent resursa/web API-JA.
- **Zahtjev za Azure AD za izdavanje grupe članstva zahtjeva** za traje Prijava korisnika (groupMembershipClaims).  To možete konfigurirati za izdavanje zahtjevima o korisnikovoj direktorija uloge članstva. Potražite u članku [autorizacija u oblak aplikacijama koristite AD grupe] [ AAD-GROUPS-FOR-AUTHORIZATION] članka na primjer implementacije.
- **Dopusti aplikacija za podršku OAuth 2.0 implicitno dodijeliti** tokova (oauth2AllowImplicitFlow). Ovu vrstu tijeka Dodjela se koristi s web-mjesto ugrađeni JavaScript web-stranice ili u okvir za jednu stranicu aplikacija (SPA). Dodatne informacije o grant implicitno autorizacije potražite u članku [objašnjenje OAuth2 implicitno dodijeliti tijek u servisu Azure Active Directory][IMPLICIT-GRANT].
- **Omogući korištenje X509 certifikati kao tajnu ključ** (keyCredentials). Potražite u članku [Stvaranje servisa i daemon aplikacija u sustavu Office 365] [ O365-SERVICE-DAEMON-APPS] i [programere vodič za provjeru autentičnosti s resursima API -jem Azure] [ DEV-GUIDE-TO-AUTH-WITH-ARM] Pomoćniku za implementaciju primjere.
- **Dodavanje nove aplikacije ID URI** aplikacije (identifierURIs[]). Aplikacija ID ji koriste se za identificirati samo aplikacije unutar njegov Azure AD klijenta (ili preko više klijenata za Azure AD, više klijentu scenarije kada kvalificiran putem provjereno prilagođenu domenu). Se koriste prilikom traženja dozvole resursa aplikacije ili dobivanje token za pristup resursu aplikacije. Kada ažurirate taj element, iste ažuriranje upućuje se odgovarajući servis upravitelja korisnika servicePrincipalNames [] zbirke, koji se nalaze u početnom klijentske aplikacije.

Programski manifest omogućuje dobar način za praćenje stanja svoju registraciju aplikacije. Budući da je dostupna u obliku JSON, predstavljanje datoteke mogu se provjeriti u izvor kontrole, uz vaše aplikacije izvornog koda.

## <a name="step-by-step-example"></a>Korak po korak primjer
Sada omogućuje prođite kroz korake potrebne da biste ažurirali konfiguracija identitet vaše aplikacije kroz programski manifest. Ne možemo će istaknuti nešto prethodnim primjerima pokazuje kako deklarirati novi opseg dozvola na aplikaciji resursa:

1. Dođite do [Azure klasični portala] [ AZURE-CLASSIC-PORTAL] i prijavite se pomoću računa koji ima ovlasti administratora ili zajednički administratora servisa.

2. Nakon koju ste autentičnost, pomaknite se prema dolje i odaberite proširenje Azure "Active Directory" u lijevom navigacijskom oknu (1), a zatim kliknite na klijentu Azure AD gdje se nalazi vaše aplikacije registrirani (2).

    ![Odaberite klijenta Azure AD][SELECT-AZURE-AD-TENANT]

3. Kada se pojavi stranici adresar, kliknite "Aplikacija" (1) pri vrhu stranice da biste vidjeli popis aplikacija registrira u klijentu. Zatim pronađite aplikaciju koju želite ažurirati na popisu i kliknite je (2).

    ![Odaberite klijenta Azure AD][SELECT-AZURE-AD-APP]

4. Sad kad ste odabrali aplikacije glavne stranice, obratite pozornost na to značajka "Upravljanje manifesta" na dnu stranice (1). Ako kliknete tu vezu, zatražit će se preuzeti ili prijenos datoteka manifesta JSON. Kliknite "Preuzimanje manifesta" (2). To će se neposredno iza pomoću dijaloškog okvira preuzimanje potvrdu tražiti da biste potvrdili tako da kliknete "Preuzimanje manifesta" (3), a zatim nešto od sljedećeg otvorite ili spremite datoteku lokalno (4).

    ![Upravljanje manifest, preuzmite mogućnost][MANAGE-MANIFEST-DOWNLOAD]

    ![Preuzimanje manifesta][DOWNLOAD-MANIFEST]

5. U ovom primjeru smo spremili datoteku lokalno, dopustite nam da biste otvoriti u uređivaču, unesite promjene u JSON pa ponovno spremite. Evo kako strukturu JSON izgleda u uređivaču za Visual Studio JSON. Napomena ga slijedi sheme za [aplikaciju entitet] [ APPLICATION-ENTITY] kao što smo ranije spomenutih:

    ![Ažuriranje manifesta JSON][UPDATE-MANIFEST]

    Na primjer, pod pretpostavkom da želimo implementacija/izlažu novu dozvolu pod nazivom "Employees.Read.All" na našem resursa aplikacije (API), jednostavno dodajte novi/drugi element zbirku oauth2Permissions ie:

        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],

    Stavka mora biti jedinstven i stoga morate stvoriti novi globalno jedinstveni ID (GUID) za na `"id"` svojstvo. U ovom slučaju jer smo naveden `"type": "User"`, tu dozvolu može biti consented na neki račun autentičnost po Azure AD smjernice na kojoj je registriran resursa/API aplikacije. To daje klijent aplikacija dozvole za pristup u ime na račun. Tijekom pristanak i za prikaz na portalu za Azure klasični koriste se naziv nizovi opis i prikaz.

6. Kada završite s ažuriranjem manifest vratili na stranicu aplikacija Azure AD na portalu za Azure klasični pa kliknite "Upravljanje manifesta" značajku ponovno (1). No ovaj put, odaberite mogućnost "Prijenos manifesta" (2). Slično preuzimanja, koje će biti ponovna otvaranja vam Knjižna ponovno pomoću drugog dijaloškog okvira koja vas pita za mjesto datoteke JSON. Kliknite "Traži datoteke..." (3), zatim pomoću dijaloškog okvira "Odabir datoteke za prijenos" da biste odabrali datoteku JSON (4) pa pritisnite "Otvori". Kada se dijaloški okvir nestaje, odaberite kvačicu "U redu" (5), a vaš manifest će biti prenesene.  

    ![Upravljanje manifest, prijenos mogućnost][MANAGE-MANIFEST-UPLOAD]

    ![Prijenos manifesta JSON][UPLOAD-MANIFEST]

    ![Prijenos manifesta JSON - potvrda][UPLOAD-MANIFEST-CONFIRM]

Manifest je spremite, možete zajamčiti na registriranu klijentski aplikacija pristup za novu dozvolu dodali smo iznad. Ovaj put umjesto možete koristiti korisničko Sučelje Web na Azure klasični portalom manifest klijentska aplikacija za uređivanje:  

1. Najprije otvorite stranicu "Konfiguriraj" klijentsku aplikaciju za koju želite da biste dodali pristup novi API-JA, a zatim kliknite gumb "Dodaj aplikaciju".
2. Zatim koji će se prikazivati s popisom aplikacije registrirani resurs (API-ji) u klijentu. Kliknite gumb plus / + simbol uz naziv računala resursa da biste ga odabrali.  
3. Kliknite kvačicu u donjem desnom kutu.
4. Kada se vratite u odjeljku "Dodavanje aplikacije" stranice konfiguracije klijentskog računala, vidjet ćete novu aplikaciju resursa na popisu. Ako pokazivač miša postavite iznad odjeljka "Dodijeliti dozvole" s desne strane retka, vidjet ćete padajućem popisu prikazuju se. Kliknite popis, a zatim odaberite novu dozvolu da bi ga dodati na popis tražene klijenta dozvola. Napomena: ovom dozvolom novi će se spremati klijentska aplikacija identiteta konfiguraciju u svojstvu zbirke "requiredResourceAccess".

![Dozvole za druge aplikacije][PERMS-TO-OTHER-APPS]

![Dozvole za druge aplikacije][PERMS-SELECT-APP]

![Dozvole za druge aplikacije][PERMS-SELECT-PERMS]

To je to. Aplikacija će se sada pokrenuti pomoću svoje nove konfiguracije identitet.

## <a name="next-steps"></a>Daljnji koraci
- Dodatne informacije o odnos između aplikacija i servisa glavnicu objekata aplikacije potražite u članku [aplikacija i servisa glavni objekata u Azure AD][AAD-APP-OBJECTS].
- [Pojmovnik za razvojne inženjere za Azure AD] potražite u članku[ AAD-DEVELOPER-GLOSSARY] za definicije neki osnovni koncepti za razvojne inženjere Azure Active Directory (AD).

Slanje povratnih informacija i Pomozite nam da budemo sužavanje i oblika sadržaj, koristite odjeljak Komentari DISQUS ispod.

<!--Image references-->
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[PERMS-SELECT-APP]: ./media/active-directory-application-manifest/portal-perms-select-app.png
[PERMS-SELECT-PERMS]: ./media/active-directory-application-manifest/portal-perms-select-perms.png
[PERMS-TO-OTHER-APPS]: ./media/active-directory-application-manifest/portal-perms-to-other-apps.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

