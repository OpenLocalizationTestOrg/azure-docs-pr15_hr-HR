<properties 
    pageTitle="Korištenje značajke provjere API pratiti poziva u upravljanju API Azure" 
    description="Saznajte kako praćenje poziva pomoću kontrola API-JA u upravljanju Azure API-JA." 
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

# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Korištenje značajke provjere API pratiti poziva u upravljanju API Azure

Upravljanje API nudi API kontrola alata za pomoć kod ispravljanje pogrešaka i vaše API-ji za otklanjanje poteškoća. Provjera API može se koristiti programski i može se koristiti izravno na portalu za razvojne inženjere. 

Osim operacije praćenje API kontrola prati procjene [pravila izraz](https://msdn.microsoft.com/library/azure/dn910913.aspx) . Pokaz, potražite u članku [177 epizode pokrivaju oblaka: više API značajke upravljanja](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) i brzi prijelaz naprijed na 21:00.

Ovaj vodič sadrži walk-through korištenja API kontrola.

>[AZURE.NOTE] Provjera API kašnjenja su samo generira i učinili dostupnima za zahtjeva koji sadrži pretplate tipke koje pripadaju [administratorskog](api-management-howto-create-groups.md) računa.

## <a name="trace-call"></a> Korištenja API kontrola za praćenje poziva

Da biste koristili API kontrola, dodajte programa **ocp, apim i praćenje: true** zahtjev zaglavlja u poziv operacija, zatim ga preuzmite i provjera praćenje pomoću URL označena zaglavlja odgovora **ocp apim-praćenje-mjestu** . To se može učiniti programatically i također može učiniti izravno na portalu za razvojne inženjere.

Pomoću ovog praktičnog vodiča pokazuje kako koristiti značajku provjere API-JA za praćenje operacije pomoću osnovnih API Kalkulator koji je konfiguriran u vodiču dohvaćanje rada [Upravljanje prvi API-JA](api-management-get-started.md) . Ako još niste dovršili taj vodič potrebno je samo nekoliko sekundi dok se da biste uvezli osnovni Kalkulator API-JA ili koristite drugi API vašem odabiru kao što su API jeku. Svaku instancu servisa za upravljanje API dolazi unaprijed konfigurirana s jeka API-JA koje je moguće koristiti Eksperimentirajte s te informacije o upravljanju API-JA. API jeka natrag vraća bilo kakve unos se šalje na njega. Da biste koristili, možete pozvati sve glagolski HTTP i povratnu vrijednost jednostavno bit će onome što ste poslali. 



Da biste započeli, kliknite **portala za razvojne inženjere** na portalu servisa za upravljanje API klasični Azure. Operacije naziva se izravno na portalu za razvojne inženjere koji omogućuje jednostavan način za prikaz i testiranje operacije API.

>Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa API upravljanje][] praktičnog vodiča za [Početak rada s upravljanjem Azure API -JA][] .

![Upravljanje API portala za razvojne inženjere][api-management-developer-portal-menu]

Kliknite **API-ji** iz gornji izbornik, a zatim **Osnovni Kalkulator**.

![Echo API][api-management-api]

Kliknite **isprobaj** pokušajte **Dodaj dvije cijele brojeve** .

![Isprobajte][api-management-open-console]

Zadržavanje zadanog vrijednosti parametara pa odaberite tipku pretplatu za proizvod koji želite koristiti iz **pretplate ključ** padajućeg popisa.

Prema zadanim postavkama u portala za razvojne inženjere **Ocp, Apim i praćenje** zaglavlja je već postavljeno na **true**. Ovo zaglavlje konfigurira hoće li se generira za praćenje.

![Pošalji][api-management-http-get]

Kliknite **Pošalji** pozvati postupak.

![Pošalji][api-management-send-results]

U odgovoru zaglavlja bit će **ocp apim-praćenje-mjestu** s vrijednošću slično kao u sljedećem primjeru.

    ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742

Praćenja možete preuzeti iz na određeno mjesto i pregledati kao što je prikazano u sljedećem koraku.

## <a name="inspect-trace"> </a>Provjera praćenja

Da biste pregledali vrijednosti u rezultatu praćenja preuzeti datoteku praćenje s URL **ocp-apim-praćenje-mjesta** . Tekstna datoteka u obliku JSON, a sadrži stavke slično kao u sljedećem primjeru.

    {
        "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
        "traceEntries": {
            "inbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0725926",
                    "data": {
                        "request": {
                            "method": "GET",
                            "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "Connection",
                                    "value": "Keep-Alive"
                                },
                                {
                                    "name": "Host",
                                    "value": "contoso5.azure-api.net"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "mapper",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0726213",
                    "data": {
                        "configuration": {
                            "api": {
                                "from": "/calc",
                                "to": {
                                    "scheme": "http",
                                    "host": "calcapi.cloudapp.net",
                                    "port": 80,
                                    "path": "/api",
                                    "queryString": "",
                                    "query": {},
                                    "isDefaultPort": true
                                }
                            },
                            "operation": {
                                "method": "GET",
                                "uriTemplate": "/add?a={a}&b={b}"
                            },
                            "user": {
                                "id": 1,
                                "groups": [
                                    "Administrators",
                                    "Developers"
                                ]
                            },
                            "product": {
                                "id": 1
                            }
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0727522",
                    "data": {
                        "message": "Request is being forwarded to the backend service.",
                        "request": {
                            "method": "GET",
                            "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "X-Forwarded-For",
                                    "value": "33.52.215.35"
                                }
                            ]
                        }
                    }
                }
            ],
            "outbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1960601",
                    "data": {
                        "response": {
                            "status": {
                                "code": 200,
                                "reason": "OK"
                            },
                            "headers": [
                                {
                                    "name": "Pragma",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Length",
                                    "value": "124"
                                },
                                {
                                    "name": "Cache-Control",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Type",
                                    "value": "application/xml; charset=utf-8"
                                },
                                {
                                    "name": "Date",
                                    "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                                },
                                {
                                    "name": "Expires",
                                    "value": "-1"
                                },
                                {
                                    "name": "Server",
                                    "value": "Microsoft-IIS/8.5"
                                },
                                {
                                    "name": "X-AspNet-Version",
                                    "value": "4.0.30319"
                                },
                                {
                                    "name": "X-Powered-By",
                                    "value": "ASP.NET"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1961112",
                    "data": {
                        "message": "Response headers have been sent to the caller. Starting to stream the response body."
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1963155",
                    "data": {
                        "message": "Response body streaming to the caller is complete."
                    }
                }
            ]
        }
    }

## <a name="next-steps"> </a>Sljedeće korake

-   Pogledajte demonstraciju praćenje pravila izraza u [177 epizode Popratno oblaka: dodatne značajke upravljanja API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Brzi prijelaz naprijed na 21:00 da biste pogledali videozapis.

>[AZURE.VIDEO episode-177-more-api-management-features-with-vlad-vinogradsky]

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Početak rada s upravljanjem API Azure]: api-management-get-started.md
[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




 