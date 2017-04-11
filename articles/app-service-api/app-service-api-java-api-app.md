<properties
    pageTitle="Stvaranje i implementacija aplikacije Java API-JA u aplikacije servisa za Azure"
    description="Upute za stvaranje paketa aplikacije za Java API-JA i njegova implementacija aplikacije servisa za Azure."
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/31/2016"
    ms.author="rachelap"/>

# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Stvaranje i implementacija aplikacije Java API-JA u aplikacije servisa za Azure

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Pomoću ovog praktičnog vodiča pokazuje kako stvoriti aplikaciju Java i njegova implementacija aplikacije API servisa aplikacija za Azure pomoću [brojka]. U operacijskom sustavu koji se može pokrenuti Java mogu slijediti upute u ovom ćete praktičnom vodiču. Kod u ovom ćete praktičnom vodiču ugrađena je [Maven]. [Jax RS] se koristi za stvaranje RESTful servisa pa se generira na temelju metapodataka specifikacija [Swagger] pomoću [Swagger uređivač].

## <a name="prerequisites"></a>Preduvjeti

1. [Komplet Java programiranje 8] \(ili noviji)
1. [Maven] instaliran na vašem računalu razvoj
1. [Brojka] instaliran na vašem računalu razvoj
1. Plaćena ili [besplatnu probnu] pretplatu na [Microsoft Azure]
1. Aplikacija za testiranje HTTP kao što je [Postman]

## <a name="scaffold-the-api-using-swaggerio"></a>Scaffold API pomoću Swagger.IO

Korištenje uređivača swagger.io online, možete unijeti Swagger JSON ili YAML kod koji predstavlja strukturu vaše API-JA. Nakon što dodate API površina tako što namijenjene, možete izvesti kod za različite platforme i okviri. U sljedećem odjeljku scaffolded kod će se izmijeniti da biste uključili mock funkcija. 

Ovaj pokazni će početi sa Swagger JSON tijelo koji će zalijepiti u uređivaču swagger.io koji će se koristiti za generiranje koda za korištenja JAX RS pristupa krajnjoj točki REST API-JA. Pa ćete uređivanje scaffolded koda za vraćanje mock podataka simulaciju REST API ugrađeni iznad mehanizam postojanost podataka.  

1. Kopirajte sljedeći kod Swagger JSON na međuspremnik:

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. Otiđite na [Internetu Swagger uređivač]. Kad dođete postoji, kliknite stavku izbornika **Datoteka -> Zalijepi JSON** .

    ![Stavka izbornika JSON Zalijepi][paste-json]

1. Zalijepite u kontakte popis API Swagger JSON koju ste ranije kopirali. 

    ![Lijepljenje JSON kod Swagger][pasted-swagger]

1. Prikaz stranica dokumentaciju i sažetak API prikazani u uređivaču. 

    ![Prikaz Swagger generira dokumenti][view-swagger-generated-docs]

1. Odaberite mogućnost za **poslužitelj za generiranje-> JAX RS** izbornika da biste scaffold kod poslužiteljsko kasnije ćete urediti da biste dodali mock implementacije. 

    ![Generiranje koda stavka izbornika][generate-code-menu-item]

    Kada je generiran kod, bit će navedeni ZIP datoteku da biste preuzeli. Datoteka sadrži kod scaffolded tako da kod generator Swagger i sve povezane izraditi skripti. Raspakiraj cijelu biblioteku direktorij na vaše radne stanice razvoj. 

## <a name="edit-the-code-to-add-api-implementation"></a>Uređivanje koda da biste dodali implementaciju API-JA

U ovom ćete odjeljku kod Swagger generira poslužiteljsko implementaciju ćete zamijenite prilagođeni kod. Novu šifru vratit će se entiteti ArrayList kontakt pozivanja klijentu. 

1. Otvorite datoteku modela *Contact.java* koja se nalazi u mapi *src/OPĆ/java/io/swagger/modela* pomoću [Koda za Visual Studio] ili omiljene uređivač. 

    ![Otvori kontakt modela datoteka][open-contact-model-file]

1. Dodajte sljedeće Graditelj klasifikaciju **kontakata** . 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. Otvorite servis *ContactsApiServiceImpl.java* implementaciju datoteku koja se nalazi u mapi *src/glavne/java/io/swagger/api/impl* pomoću [Visual Studio kod] ili omiljene uređivač.

    ![Otvaranje kontakata usluge kod datoteka][open-contact-service-code-file]

1. Novi kod da biste dodali mock implementacije kod usluge prebrisati kod u datoteci. 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. Otvorite naredbeni redak i promijenite direktorija korijensku mapu aplikacije.

1. Pokrenite sljedeću naredbu Maven omogućuje stvaranje kod i pokrenite je lokalno pomoću poslužitelja za aplikaciju Jetty. 

        mvn package jetty:run

1. Trebali biste vidjeti naredbeni prozor odražavaju Jetty je počeo kod na priključak 8080. 

    ![Otvaranje kontakata usluge kod datoteka][run-jetty-war]

1. Pomoću [Postman] zahtjeva "se svi kontakti" metode API-JA na api/http://localhost:8080/kontakata.

    ![Pozivanje kontakata API-JA][calling-contacts-api]

1. Pomoću [Postman] zatraže korakom API-JA "Dohvati određenim kontaktom" koji se nalazi na http://localhost:8080/api/kontakata/2.

    ![Pozivanje kontakata API-JA][calling-specific-contact-api]

1. Na kraju, međuverziju Java WAR (web-arhiva) datoteke izvršavanjem sljedeće naredbe Maven na konzoli. 

        mvn package war:war

1. Kada je ugrađena datoteka WAR, ona će biti stavljena u **ciljnu** mapu. Pronađite mapu **cilj** i preimenujte datoteku WAR **ROOT.war**. (Provjerite velikog početnog slova odgovara li ovaj oblik).

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. Na kraju, izvršiti sljedeće naredbe iz korijenske mape aplikacije da biste stvorili mapu **implementirati** za implementaciju datoteke WAR Azure. 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>Objavljivanje izlaz aplikacije servisa za Azure

U ovom odjeljku ćete upute da biste stvorili novu aplikaciju API pomoću portala za Azure, Priprema tu aplikaciju API-JA za hostiranje Java aplikacije i implementacija novostvorenu datoteku WAR aplikacije servisa za Azure da biste pokrenuli novu Aplikciju API-JA. 

1. Stvorite novu aplikaciju API [Azure portal]klikom stavku izbornika **Web -> novo + aplikacije API -> mobilni** , unos pojedinosti aplikacije, a zatim kliknite **Stvori**.

    ![Stvorite novu aplikaciju API-JA][create-api-app]

1. Nakon što API aplikaciju, otvorite plohu **Postavke** za aplikaciju programa, a zatim stavku izbornika **Postavke aplikacije** . Odaberite najnoviju verziju jezika Java među ponuđenim mogućnostima, a zatim odaberite najnovije Tomcat na izborniku **Web spremnik** , a zatim **Spremi**.

    ![Postavljanje jezika Java u plohu API aplikacije][set-up-java]

1. Kliknite stavku izbornika postavke **vjerodajnica za implementaciju** i korisničko ime i lozinku koju želite koristiti za objavljivanje datoteke u aplikaciju za API-JA. 

    ![Postavljanje vjerodajnica za implementaciju][deployment-credentials]

1. Kliknite stavku izbornika za **implementaciju izvorne** postavke. Jednom postoji, kliknite gumb **Odabir izvora** , odaberite mogućnost **Lokalni brojka spremište** , a zatim **u redu**. To će stvoriti spremište brojka izvodi u Azure koja sadrži vezu s aplikacijom API-JA. Svaki put kada primjenu kod *osnovne* granu spremište na brojka kod će se objaviti u uživo pokrenute aplikacije API instance. 

    ![Postavljanje nove lokalne brojka spremište][select-git-repo]

1. Kopirajte URL nove brojka spremišta u međuspremnik. Spremite to kao što je važno u trenutak. 

    ![Postavljanje novog spremište brojka aplikacije][copy-git-repo-url]

1. Brojka automatske WAR datoteka u mreži spremište. Da biste to učinili, pronađite mapu **Implementacija** koju ste ranije stvorili tako da možete jednostavno izvršiti kod najviše spremište izvodi u aplikacije servisa. Kada se u prozoru konzole i preusmjereni u mapu u kojoj se nalazi mapa webapps, problema sljedeće naredbe i brojka pokretanje postupka i pokrenuti isključivanje implementacije. 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    Kada je poslao zahtjev za **automatskih** , će se zatražiti lozinka koju ste stvorili za implementaciju vjerodajnica neke starije verzije. Nakon što unesete vjerodajnice, trebali biste vidjeti portalom prikaz je uveden ažuriranja.

1. Ako opet koristite Postman pogoditi iz prvog aplikacije API upravo implementiran koji se izvodi u aplikacije servisa za Azure, vidjet ćete da je ponašanje dosljedan i sada je još daje podatke o kontaktu prema očekivanjima i pomoću jednostavne kod promjene na Swagger.io scaffolded Java kod. 

    ![Korištenje sustava Java kontakata REST API-JA uživo u Azure][postman-calling-azure-contacts]

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku uspijete da biste započeli s datotekom Swagger JSON, a neke scaffolded kod programskog jezika Java dobivenog u uređivaču Swagger.io. Iz nje, jednostavne promjene i na brojka implementacija postupak rezultirala pojavljuju funkcionalni API aplikacije pisane Java. Sljedeći Praktični vodič prikazuje kako [zauzeti API aplikacije klijenata JavaScript pomoću CORS][App Service API CORS]. Kasnije vodiče u nizu pokazati kako implementirati provjere autentičnosti i autorizacije.

Da biste sastavili na ovaj uzorak, možete saznati više o [SDK za pohranu za Java] održati JSON blob-ova. Ili možete koristiti u [Dokumentu DB Java SDK] da biste spremili vaši podaci za kontakt DB Azure dokumenta. 

Dodatne informacije o korištenju Java u Azure potražite u članku [Razvojni centar za Java].

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Portal za Azure]: https://portal.azure.com/
[Dokument DB Java SDK]: ../documentdb/documentdb-java-application.md
[Besplatna probna verzija]: https://azure.microsoft.com/pricing/free-trial/
[Brojka]: http://www.git-scm.com/
[Razvojni centar za Java]: /develop/java/
[Komplet Java programiranje 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Uređivač Online Swagger]: http://editor.swagger.io/
[Postman]: https://www.getpostman.com/
[Prostor za pohranu SDK Java]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Uređivač swagger]: http://editor.swagger.io/
[Kod za Visual Studio]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
