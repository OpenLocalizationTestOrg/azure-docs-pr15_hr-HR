<properties 
    pageTitle="Upute za sigurnost pozadinske klijenti provjera autentičnosti potvrda u upravljanju API Azure" 
    description="Saznajte kako sigurnost pozadinskih pomoću provjere autentičnosti u klijentskim certifikat u upravljanju API Azure." 
    services="api-management" 
    documentationCenter="" 
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

# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Upute za sigurnost pozadinske klijenti provjera autentičnosti potvrda u upravljanju API Azure

Upravljanje API pruža mogućnost siguran pristup usluzi pozadinske API pomoću certifikata klijenta. Ovaj vodič prikazuje kako upravljati certifikata na portalu za publisher API-JA i konfiguriranju API-JA za korištenje potvrde da biste pristupili njegovu servisu pozadinske.

Informacije o upravljanju certifikata pomoću API upravljanje REST API-JA, potražite u članku [entitet Azure API upravljanje REST API certifikata][].

## <a name="prerequisites"> </a>Preduvjeti

Ovaj vodič prikazuje kako konfigurirati svoje instanca servisa API upravljanje da biste koristili provjera autentičnosti klijentskih potvrda za pristup servisu pozadinske API. Prije slijedeći korake u ovoj temi, trebali biste imate usluge pozadinske konfiguriran za provjeru autentičnosti certifikat klijenta ([Da biste konfigurirali provjera autentičnosti potvrda u web-mjesta Azure pogledajte ovaj članak][]) i imaju pristup certifikat i lozinku za certifikat za prijenos na portalu za upravljanje API programa publisher.

## <a name="step1"> </a>Prijenos potvrdu klijenta

Da biste započeli, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA. Tako ćete doći do portala za upravljanje API programa publisher.

![Portal za Publisher API-JA][api-management-management-console]

>Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa API upravljanje][] praktičnog vodiča za [Početak rada s upravljanjem Azure API -JA][] .

Kliknite **Sigurnost** na izborniku **Upravljanje API -JA** na lijevoj strani, a kliknite **klijentskih potvrda**.

![Certifikati za klijenta][api-management-security-client-certificates]

Da biste dodali novi certifikat, kliknite **Prenesi certifikata**.

![Prijenos certifikata][api-management-upload-certificate]

Pronađite certifikat, a zatim unesite lozinku za potvrdu.

>Certifikat mora biti u obliku **.pfx** . Nije dopušteno samopotpisane potvrde.

![Prijenos certifikata][api-management-upload-certificate-form]

Kliknite **Prijenos** da biste prenijeli certifikata.

>Trenutno se provjerava Potvrda lozinke. Ako nije valjana prikazuje se poruka o pogrešci.

![Certifikat prenijeli][api-management-certificate-uploaded]

Nakon prijenosa certifikat pojavljuje se na karticu **potvrde klijenta** . Ako imate više certifikata, provjerite bilješku predmeta ili posljednje četiri znaka otisak prsta, koji se koriste za odabir certifikata prilikom konfiguriranja API-JA za korištenje certifikata, kao u sljedećem odjeljku [Konfiguriranje API-JA za korištenje potvrde klijenta za provjeru autentičnosti pristupnika][] .

>Da biste isključili provjeru valjanosti certifikata lanac prilikom korištenja, na primjer, samopotpisani certifikat, slijedite korake opisane u ovoj najčešća pitanja vezana uz [stavku](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).

## <a name="step1a"> </a>Brisanje potvrdu klijenta

Da biste izbrisali certifikat, kliknite **Izbriši** pokraj željeni certifikat.

![Brisanje certifikata][api-management-certificate-delete]

Kliknite da biste potvrdili **Da, izbrišite je** .

![Potvrda brisanja][api-management-confirm-delete]

Ako je certifikat koristi API, prikazuje se upozorenje zaslona. Da biste izbrisali certifikat najprije morate ukloniti certifikat iz bilo kojeg API-ji koji konfigurirani tako da ga koristiti.

![Potvrda brisanja][api-management-confirm-delete-policy]

## <a name="step2"> </a>Konfiguriranje API koristiti klijentski certifikat za provjeru autentičnosti pristupnika

Na izborniku **Upravljanje API -JA** na lijevoj strani kliknite **API-ji** , kliknite naziv željene API pa kliknite karticu **Sigurnost** .

![Sigurnost API-JA][api-management-api-security]

S padajućeg popisa **s vjerodajnicama** odaberite **klijentskih potvrda** .

![Certifikati za klijenta][api-management-mutual-certificates]

Odaberite željeni certifikat s padajućeg popisa **klijentski certifikat** . Ako postoji više certifikata možete pogledati u predmetu ili posljednje četiri znaka otisak prsta kao što je naznačeno u prethodnom odjeljku da biste odredili točan certifikata.

![Odaberite certifikat][api-management-select-certificate]

Kliknite **Spremi** da biste spremili promjene konfiguracije na API-JA.

>Ta promjena stupa odmah, a pozivi operacije te API će koristiti certifikat za provjeru autentičnosti na poslužitelju pozadinske.

![Spremanje promjena API-JA][api-management-save-api]

>Kada certifikat navedena za pristupnik za provjeru autentičnosti za servis pozadinske API, postaje dio pravilnika za taj API-JA i moguće je prikazati u uređivaču pravila.

![Pravila certifikata][api-management-certificate-policy]

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o drugim načinima sigurne pozadinskog servisa, kao što su HTTP osnovni ili zajedničkim tajnu provjeru autentičnosti, pogledajte sljedeći videozapis.

> [AZURE.VIDEO last-mile-security]

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Početak rada s upravljanjem API Azure]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance

[Azure entitet API upravljanje REST API certifikata]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Konfiguriranje provjere autentičnosti certifikata na web-mjesta Azure pogledajte u ovom se članku]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Konfiguriranje API koristiti klijentski certifikat za provjeru autentičnosti pristupnika]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps


 
