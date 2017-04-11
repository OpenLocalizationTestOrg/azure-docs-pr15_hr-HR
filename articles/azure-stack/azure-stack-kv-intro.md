<properties
    pageTitle="Azure Uvod u stogu ključ sigurnog | Microsoft Azure"
    description="Saznajte kako Azure stogu ključ sigurnog upravlja ključeva i tajne"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="introduction-to-key-vault-in-azure-stack"></a>Uvod u ključa sigurnog u stogu Azure #

## <a name="before-you-start"></a>Prije početka

U ovom se članku pretpostavlja sljedeće:

- Čitač ima pristup na pretplatu koja sadrži Azure ključ sigurnog omogućena.
- Azure PowerShell SDK je konfiguriran i dostupan.
- Za izdanje TP2 sve klijentu dostupnog operacije može izvoditi samo iz PowerShell.

## <a name="key-vault-basics"></a>Osnove sigurnog ključ

Ključ sigurnog u stogu Azure pomaže zaštitili šifriranja tipke i koristiti tajne koji aplikacija i servisa u oblaku. Pomoću tipke sigurnog Šifrirajte ključeva i tajne (kao što su provjere autentičnosti tipki, tipke račun za pohranu, ključeva za šifriranje podataka, .pfx datoteka i lozinke).

Ključ sigurnog pojednostavnjuje postupak upravljanja ključem i omogućuju vam da biste zadržali kontrolu nad prečaci koji se pristup i šifriranje podataka. Razvojni inženjeri možete stvoriti tipke za razvoj i testiranje u minutama i zatim ih jednostavno migrirati ključevima radnog. Sigurnost administratori mogu dodijeliti (i oduzimanje) dozvolu za tipke, po potrebi.

Bilo koja osoba za pretplatu na hrpu Azure možete stvarati i koristiti ključa sefovi. Iako ključ sigurnog koristi razvojnim inženjerima i administratora za sigurnost, možete biti implementirano i upravlja administrator koja upravlja drugih servisa Azure stog za tvrtke ili ustanove. Na primjer, ovaj administratora mogu se prijaviti pomoću pretplate na hrpu Azure stvaranje sigurnog za tvrtku ili ustanovu u kojem možete spremati tipke i smatrati ODGOVORNIM ni za ove operativnih zadataka:

- Stvaranje ili uvoz ključ ili tajna
- Opoziv ili izbrišite ključ ili tajna
- Autorizirajte korisnika ili aplikacije da biste pristupili ključa sigurnog pa zatim mogu upravljati ili koristite njegove tipke i tajne
- Konfiguriranje korištenja ključa (na primjer, potpisivanje i šifriranje)

Ovaj administrator možete pružiti razvojnim inženjerima ji za pozivanje iz svoje aplikacije i administrator za sigurnost pružiti podaci zapisnika korištenja ključa.

Razvojni inženjeri možete upravljati tipki izravno pomoću API-ji. Dodatne informacije potražite u članku vodič za programiranje sigurnog ključ.

## <a name="scenarios"></a>Scenariji

Sljedeća tablica prikazuje slučajevi u kojima ključ sigurnog omogućuju zadovoljavaju potrebe za razvojne inženjere i administratora za sigurnost:


### <a name="developer-for-an-azure-stack-application"></a>Za Azure stogu aplikaciju za razvojne inženjere

**Problem**: želim pisanje aplikaciju za Azure snopu koji koristi tipke za potpis i šifriranje, ali želim te se vanjski iz moje aplikacije da bi je rješenje prikladna za aplikaciju koja geografski distribuira.

**Naredba**: tipke spremaju se u na sigurnog i pozvati tako da URI po potrebi.


### <a name="developer-for-software-as-a-service-saas"></a>Za razvojne inženjere za softver kao service (SaaS)

**Problem:** Ne želite odgovornosti ili potencijalni odgovornost za tipke klijentu Moji kupci i tajne.

**Izjava:** Klijentima možete uvesti vlastite tipke u stogu Azure i njima upravljati. Želim da se kupci vlasnik i upravljanje ključ tako da se možete usmjeriti na način što učiniti najbolje, koji pruža temeljni značajke softver.


### <a name="chief-security-officer-cso"></a>Sigurnost Šef (CSO)

**Problem:** Želim da biste bili sigurni da moje tvrtke ili ustanove u kontroli ključa životnog ciklusa i moguće nadzirati korištenja ključa.

**Naredba** Ključ sigurnog osmišljene tako da Microsoft potražite u članku ili izdvojiti ključeva.  Kada nije potrebno izvođenje operacija šifrirane pomoću tipki klijenata aplikaciju, ključ sigurnog to čini ime aplikacije. Aplikacija potražite u članku tipke klijenata.  Iako koristimo više servisa Azure snop i resursima, želim upravljanje tipke s jednog mjesta u stogu Azure. Na sigurnog nudi jedinstveno sučelje, bez obzira na to koliko sefovi imate u stogu Azure, koje područja oni podršku i koji ih koristiti.

## <a name="next-steps"></a>Daljnji koraci

[Početak rada s ključem zbirke ključeva](azure-stack-kv-getting-started.md)
