<properties 
    pageTitle="Kako da biste autorizirali račune za razvojne inženjere pomoću servisa Azure Active Directory u upravljanju API Azure" 
    description="Saznajte kako da biste autorizirali korisnika putem Azure Active Directory u odjeljku Upravljanje API-JA." 
    services="api-management" 
    documentationCenter="API Management" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Kako da biste autorizirali račune za razvojne inženjere pomoću servisa Azure Active Directory u upravljanju API Azure


## <a name="overview"></a>Pregled
Ovaj vodič prikazuje kako omogućiti pristup portala za razvojne inženjere za sve korisnike u jednu ili više Azure Active direktorija. Ovaj vodič također prikazuje kako upravljati grupe korisnika Azure Active Directory dodavanjem vanjske grupe koje sadrže korisnike Azure Active Directory.

>Da biste prošli korake u ovom vodiču najprije morate imati Azure Active Directory u kojoj želite stvoriti aplikaciju.

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Kako da biste autorizirali račune za razvojne inženjere pomoću servisa Azure Active Directory

Da biste započeli, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA. Tako ćete doći do portala za upravljanje API programa publisher.

![Portal za Publisher][api-management-management-console]

>Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa API upravljanje][] praktičnog vodiča za [Početak rada s upravljanjem Azure API -JA][] .

Kliknite **Sigurnost** na izborniku **Upravljanje API -JA** na lijevoj strani, a zatim kliknite **Vanjski identiteta**.

![Vanjski identiteta][api-management-security-external-identities]

Kliknite **Azure Active Directory**. Zabilježite **Preusmjeravanje URL-a** i prebacite vaše Azure Active Directory na portalu klasični Azure.

![Vanjski identiteta][api-management-security-aad-new]

Kliknite gumb **Dodaj** da biste stvorili novu aplikaciju Azure Active Directory, a zatim odaberite **Dodaj je moje tvrtke ili ustanove razvoju aplikacija**.

![Dodavanje nove aplikacije Azure Active Directory][api-management-new-aad-application-menu]

Unesite naziv aplikacije, odaberite **web-aplikacije i/ili Web API**i kliknite gumb Dalje.

![Nova aplikacija Azure Active Directory][api-management-new-aad-application-1]

**URL za prijavu**unesite URL za prijavu portala za razvojne inženjere. U ovom se primjeru je **URL za prijavu** `https://aad03.portal.current.int-azure-api.net/signin`. 

Za **Aplikacije ID URL**unesite domenu zadane ili prilagođene domene za Azure Active Directory i dodati jedinstveni niz. U ovom primjeru zadanu domenu **https://contoso5api.onmicrosoft.com** se koristi s sufiks **/api** naveden.

![Nova svojstva aplikacije Azure Active Directory][api-management-new-aad-application-2]

Kliknite gumb Provjeri da biste spremili i stvorite novu aplikaciju i prijeđite na karticu **Konfiguriraj** da biste konfigurirali nove aplikacije.

![Stvorena nova aplikacija Azure Active Directory][api-management-new-aad-app-created]

Ako se više imenika Active Azure premještaju se može koristiti za ovu aplikaciju, kliknite **da** za **aplikaciju je više klijenta**. Zadani je **ne**.

![Aplikacija je više klijenta][api-management-aad-app-multi-tenant]

Kopirajte **URL adresa za preusmjeravanje** iz odjeljka **Azure Active Directory** na kartici **Vanjski identiteta** na portalu za publisher i zalijepite u tekstni okvir **URL odgovor** . 

![URL adresa za odgovor][api-management-aad-reply-url]

Pomaknite se do dna kartice Konfiguriraj odaberite **Dozvole za aplikaciju** padajući popis, a provjeru **podataka direktorija za čitanje**.

![Dozvole za aplikaciju][api-management-aad-app-permissions]

Odaberite **Dozvole prava** padajući popis i potvrdite okvir **Omogući prijavljivanje i pročitajte korisničkim profilima**.

![Dozvole za prijenos ovlasti][api-management-aad-delegated-permissions]

>Dodatne informacije o aplikacije i ovlaštenog dozvola potražite u članku [pristup grafikonu API -JA][].

**Id klijenta** kopiranje u međuspremnik.

![Id klijenta][api-management-aad-app-client-id]

Vratite se na portal sustava publisher i zalijepite u polju **Id klijenta** kopirana konfiguracije aplikacije Azure Active Directory.

![Id klijenta][api-management-client-id]

Vratite se u konfiguraciji Azure Active Directory i kliknite u **Odaberite trajanje** padajućeg popisa u odjeljku **tipke** i navedite interval. U ovom se primjeru koristi se **1 godine** .

![Ključ][api-management-aad-key-before-save]

Kliknite **Spremi** da biste spremili konfiguraciju i prikaz tipku. Kopiranje tipku u međuspremnik.

>Zabilježite ovog ključa. Nakon toga zatvorite prozor konfiguracije Azure Active Directory, ključ nije moguće ponovno prikazati.

![Ključ][api-management-aad-key-after-save]

Vratite se na portal sustava publisher i zalijepite ključ u tekstni okvir **Tajna klijenta** .

![Tajna klijenta][api-management-client-secret]

**Dopuštene klijenata** određuje koje direktorija imati pristup API-ji instanca servisa za upravljanje API-JA. Navedite domene instanci Azure Active Directory u koju želite dopustiti pristup. Više domena možete odvojite niza, praznine ili zareze.

![Dopuštene klijenata][api-management-client-allowed-tenants]

U odjeljku **Dopuštene klijenata** moguće navesti više domena. Prije nego što svi korisnici mogu prijaviti s na drugu domenu od izvorne domena gdje je registrirana aplikaciju, globalni administrator drugu domenu mora dodijeliti dozvolu za aplikaciju za pristup podacima direktorija. Da biste dodijelili dozvolu, globalni administrator morate se prijaviti u aplikaciju i kliknite **Prihvati**. U sljedećem primjeru `miaoaad.onmicrosoft.com` dodana **Dopušteno klijenata** i globalni administrator iz te domene zapisivanje u prvi put.

![Dozvole][api-management-permissions-form]

>Ako nisu globalni administrator pokušava prijaviti da su dozvole dodijeljene globalni administrator, pokušaj prijave ne uspije, a prikazuje se zaslon za pogreške.

Kada se željena konfiguracija nije naveden, kliknite **Spremi**.

![Spremi][api-management-client-allowed-tenants-save]

Nakon promjene se spremaju, korisnici u navedenom Azure Active Directory prijavite se u portala za razvojne inženjere tako da slijedite korake u [prijavite se na račun servisa Azure Active Directory pomoću portala za razvojne inženjere][].

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Kako dodati vanjski Azure Active Directory grupe sustava

Nakon Omogućivanje pristupa za korisnike servisa Azure Active Directory, možete dodati grupe Azure Active Directory u API upravljanje da biste jednostavnije upravljali pridruživanja razvojnim inženjerima u grupi željeno proizvode sustava.

> Da biste konfigurirali vanjske grupe za Azure Active Directory, Azure Active Directory morate najprije mogu konfigurirati na kartici identiteta pomoću postupka opisanog u prethodnom odjeljku. 

Vanjski Azure Active Directory grupe dodat će se na kartici **vidljivost** proizvoda za koji želite dopustiti pristup grupi. Kliknite **proizvoda**, a zatim naziv željene proizvoda.

![Konfiguriranje proizvoda][api-management-configure-product]

Prijelaz na karticu **vidljivost** pa kliknite **Dodavanje grupe iz servisa Azure Active Directory**.

![Dodavanje grupa][api-management-add-groups]

Odaberite **Azure Active Directory klijenta** s padajućeg popisa, a zatim upišite naziv željenu grupu u **grupama** potrebno dodati tekstni okvir.

![Odaberite grupu][api-management-select-group]

Ovaj naziv grupe pronaći ćete na popisu **grupa** za vaše Azure Active Directory, kao što je prikazano u sljedećem primjeru.

![Popis grupa Azure Active Directory][api-management-aad-groups-list]

Kliknite **Dodaj** da biste provjerili valjanost naziv grupe i dodali grupi. U ovom primjeru **Contoso 5 programerima** dodaje vanjske grupe. 

![Grupa dodali][api-management-aad-group-added]

Kliknite **Spremi** da biste spremili novi odabir grupe.

Kada granu Azure Azure Active Directory konfiguriran iz jednog proizvoda, on je dostupan na provjeru na kartici **vidljivosti** za ostale proizvode instanca servisa za upravljanje API-JA.

Da biste pregledali i konfiguriranje svojstava za vanjske grupe Kada budu dodani, kliknite naziv grupe na kartici **grupe** .

![Upravljanje grupama][api-management-groups]

Na tom mjestu možete uređivati **naziv** i **Opis** grupe.

![Uređivanje grupe][api-management-edit-group]

Korisnici iz konfigurirani Azure Active Directory možete prijaviti na portal za razvojne inženjere i prikaz i pretplatiti na grupe za koje imaju vidljivosti prema uputama u sljedećem odjeljku.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Kako se prijaviti na račun servisa Azure Active Directory pomoću portala za razvojne inženjere

Prijaviti pomoću računa za Azure Active Directory konfiguriran u ranijim odlomcima portala za razvojne inženjere, otvorite novi prozor preglednika pomoću **URL za prijavu** iz servisa Active Directory konfiguracije aplikacije pa kliknite **Azure Active Directory**.

![Portala za razvojne inženjere][api-management-dev-portal-signin]

Unesite vjerodajnice za jednog korisnika servisa Azure Active Directory pa kliknite **Prijava**.

![prijavi se][api-management-aad-signin]

Ako je potrebno dodatne informacije možda zatražiti s obrazac za registraciju. Ispunite obrazac za registraciju, a zatim kliknite **Prijava**.

![Registracija][api-management-complete-registration]

Korisnički sada prijavili portala za razvojne inženjere za vaše instancu servisa za upravljanje API-JA.

![Registracija dovršeno][api-management-registration-complete]



[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Početak rada s upravljanjem API Azure]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Pristup grafikonu sustava API-JA]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Prijavite se na račun servisa Azure Active Directory pomoću portala za razvojne inženjere]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

