<properties 
    pageTitle="Upute za konfiguriranje obavijesti i predlošci e-pošte u odjeljku Upravljanje Azure API-JA" 
    description="Saznajte kako konfigurirati obavijesti i predlošci u upravljanju Azure API-JA za e-poštu." 
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

# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Upute za konfiguriranje obavijesti i predlošci e-pošte u odjeljku Upravljanje Azure API-JA

Upravljanje API pruža mogućnost da biste konfigurirali obavijesti za određene događaje, a da biste konfigurirali predlošci e-pošte koji se koriste za komunikaciju s administratorima i razvojnim inženjerima instance API upravljanje. U ovoj se temi objašnjava konfiguriranje obavijesti za dostupnih događaja, a sadrži pregled konfiguriranja predlošci e-pošte koji se koristi za te događaje.

## <a name="publisher-notifications"> </a>Konfiguriranje obavijesti programa publisher

Da biste konfigurirali obavijesti, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA. Tako ćete doći do portala za upravljanje API programa publisher.

![Portal za Publisher][api-management-management-console]

>Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa API upravljanje][] praktičnog vodiča za [Početak rada s upravljanjem Azure API -JA][] .

Kliknite **obavijesti** na izborniku **Upravljanje API -JA** na lijevoj strani da biste vidjeli dostupne obavijesti.

![Obavijesti u programu Publisher][api-management-publisher-notifications]

Sljedeći popis događaja moguće je konfigurirati za obavijesti.

-   **Zahtjevi za pretplatu (potrebno odobrenje)** – navedeni e-pošte primatelja i korisnici će primiti obavijesti e-poštom o pretplatu zahtjeva za proizvode API potrebno odobrenje.
-   **Novog pretplatama** - navedeni e-pošte primatelja i korisnici će primiti obavijesti e-poštom o novog pretplatama API-JA proizvoda.
-   **Galerija zahtjeva za aplikaciju** - navedeni e-pošte primatelja i korisnici će primajte obavijesti o e-pošte poslane nove aplikacije u galeriju aplikacija.
-   **Skrivena KOPIJA** - navedeni e-pošte primatelja i korisnici će primaju e-pošte skrivene kopije sve poruke e-pošte poslane za razvojne inženjere.
-   **Novi problem ili komentar** - navedeni e-pošte primatelja i korisnici će primiti obavijesti e-pošte kad je novi problem ili komentar šalje se na portal za razvojne inženjere.
-   **Zatvaranje računa poruku** - navedeni e-pošte primatelja i korisnici će primiti obavijesti e-poštom prilikom zatvaranja računa.
-   **Ograničenje kvote za pretplatu Approaching** - sljedećim primateljima e-pošte i korisnici će primiti obavijesti e-poštom prilikom korištenja pretplate dobiva blizu kvota korištenja.

Za svaki događaj možete odrediti primatelja e-pošte pomoću tekstni okvir adresa e-pošte ili odaberete korisnika s popisa.

Da biste odredili adrese e-pošte da biste primili obavijest, unesite ih u tekstni okvir adresa e-pošte. Ako imate više adresa e-pošte, odvojite ih pomoću zareza.

![Primatelji obavijesti][api-management-email-addresses]

Da biste naveli korisnike da biste primili obavijest, kliknite **Dodavanje primatelja**, potvrdite okvir pokraj korisnika biti obaviješten i kliknite **u redu**.

>Imajte na umu da su samo administratori prikazane na popisu.

Nakon konfiguriranja primatelji obavijesti, kliknite **Spremi** da biste primijenili primatelji ažurirane obavijesti.

>Ako zatvorite karticu **Obavijesti programa Publisher** portal za publisher upozorava vas ako postoje nespremljene promjene.

## <a name="email-templates"> </a>Konfiguriraj predlošci e-pošte

Upravljanje API nudi predloške e-pošte za poruke e-pošte koje se šalju u tijeku administriranje i korištenja servisa. Navedeni su sljedeći predlošci e-pošte.

-   Aplikacija Galerija slanje odobrene
-   Slovo farewell za razvojne inženjere
-   Ograničenje kvote za razvojne inženjere Približavanje obavijesti
-   Pozivanje korisnika
-   Novi komentar dodali problema
-   Novi problem primili
-   Nova pretplata aktivirati
-   Pretplata obnoviti potvrdu
-   Zahtjev za pretplatu odbije
-   Primiti zahtjev za pretplatu

Ove predlošci mogu mijenjati kao željeni.

Da biste pogledali i konfiguriranje predložaka e-pošte za vaš instancu upravljanje API-JA, kliknite **obavijesti** na izborniku **Upravljanje API -JA** na lijevoj strani, a odaberite karticu **Predlošci e-pošte** .

![Predlošci e-pošte][api-management-email-templates]

Da biste mijenjali određeni predložak, odaberite ga s padajućeg popisa **predložaka** .

![Popis predložaka za e-pošte][api-management-email-templates-list]

Svaki predložak e-pošte ima predmet u obliku običnog teksta i definiciju tijelo u HTML obliku. Moguće je prilagoditi svaku stavku kao željeni.

![Uređivač predložak e-pošte][api-management-email-template]

Popis **parametara** sadrži popis parametara, kojeg umetnuta u predmet ili tijelo poruke, bit će zamijeniti određenu vrijednost pri slanju e-pošte. Da biste umetnuli parametar, postavite pokazivač na kojem želite da se parametar otvorite pa kliknite strelicu s lijeve strane naziv parametra.

Kliknite **Pregled** ili **Pošalji test** da biste vidjeli kako će e-pošte izgleda ili slanje e-pošte test.

>Imajte na umu da parametri ne zamjenjuje stvarnih vrijednosti prilikom pregleda ili slanje test.
>
>Da biste spremili promjene predložak e-pošte, kliknite **Spremi**ili da biste odustali od promjene kliknite **Odustani**.



[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Početak rada s upravljanjem API Azure]: api-management-get-started.md
[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance