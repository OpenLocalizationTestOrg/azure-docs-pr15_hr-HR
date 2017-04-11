<properties 
   pageTitle="Azure mobilne radnje vodič – API-ji za otklanjanje poteškoća" 
   description="Vodiči za Azure mobilne radnje - API-ji za otklanjanje poteškoća" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="10/04/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-api-issues"></a>Vodič za probleme s API-JA za otklanjanje poteškoća

Slijedi popis mogućih problema koji se mogu pojaviti s način na koji se administratori rade s mogućnostima Mobile Azure putem API-ji.

## <a name="syntax-issues"></a>Sintaksa problema

### <a name="issue"></a>Problem
- Pogreške sintakse pomoću API-JA (ili neočekivano ponašanje).

### <a name="causes"></a>Uzroci

- Problemi sintakse:
    - Obavezno provjerite sintaksu određene API koristite da biste potvrdili da je mogućnost dostupna.
    - Uobičajeni problem s korištenja API je zbuniti API dosegne i automatske API (većinu zadataka trebaju izvesti s dosegne API-jem umjesto API automatske). 
    - Drugi uobičajeni problem s SDK integracije i način korištenja API je zbuniti ključ za SDK i ključ za API-JA.
    - Skripte koji se povezuju s API-ji potrebno slanje podataka barem svaka 10 minuta ili veze će istek vremena (osobito često u skriptama Monitor API slušanje podataka). Da biste spriječili vremensko ograničenje, imati skriptu pošaljite ping XMPP svakih 10 minuta da biste zadržali sesiju aktivnosti s poslužiteljem.

### <a name="see-also"></a>Vidi također
 
- [Dokumentacija API-JA][Link 4]
- [Informacije o XMPP protokol]( http://xmpp.org/extensions/xep-0199.html)
 
## <a name="unable-to-use-the-api-to-perform-the-same-action-available-in-the-azure-mobile-engagement-ui"></a>Nije moguće koristiti s API-JA za izvođenje iste akcije dostupne u korisničkom Sučelju radnje Mobile Azure

### <a name="issue"></a>Problem
- Akcija koja funkcionira s korisničkog Sučelja Azure Mobile radnje ne funkcionira iz povezanih API radnje Mobile Azure.

### <a name="causes"></a>Uzroci

- Potvrđivanje izvršiti istu akciju s korisničkog Sučelja radnje Mobile Azure prikazuje da ste pravilno integrirali ta značajka radnje Mobile Azure s SDK-a.

### <a name="see-also"></a>Vidi također
 
- [Dokumentacija korisničkog Sučelja][Link 1]
 
## <a name="error-messages"></a>Poruka o pogrešci

### <a name="issue"></a>Problem
- Kodovi pogrešaka pomoću API koji se prikazuje prilikom izvođenja ili u zapisnicima.

### <a name="causes"></a>Uzroci

- Evo složeni popis uobičajenih API status kodovi brojeva za pregled i preliminarni otklanjanje poteškoća:

        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).

### <a name="see-also"></a>Vidi također

- [Dokumentacija API - detaljne pogrešaka na svakom određene API-JA][Link 4]
 
## <a name="silent-failures"></a>Tihu neuspjeha

### <a name="issue"></a>Problem
- Akcija API-JA ne uspijeva uz nijedan poruku o pogrešci prikazuje prilikom izvođenja ili u zapisnicima.

### <a name="causes"></a>Uzroci

- Mnogo stavki će biti onemogućena u korisničkom Sučelju radnje Mobile Azure ako ih ne integrirana ispravno, ali će uspjeti tihu iz API Sučelja, pa Imajte na umu da biste testirali funkciji u korisničkom Sučelju da biste vidjeli funkcionira li.
- Azure radnje Mobile i mnoge napredne značajke Azure Mobile radnje koju pokušavate koristiti, moraju biti pojedinačno integriranom aplikacije s SDK kao zasebna korake prije nego što ih možete koristiti.

### <a name="see-also"></a>Vidi također

- [Vodiča za otklanjanje pogrešaka - SDK][Link 25]
 
<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
