<properties
    pageTitle="Što je sigurnog ključ Azure? | Microsoft Azure"
    description="Azure ključ sigurnog olakšava zaštitu tipke šifriranja i tajne koristi oblaka aplikacija i servisa. Pomoću sigurnog ključ Azure kupci Šifrirajte ključeva i tajne (primjerice provjeru autentičnosti tipki, tipke račun za pohranu, ključeva za šifriranje podataka. PFX datotekama i lozinke) pomoću tipki zaštićene hardver sigurnost Module (HSMs)."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="cabailey"/>



# <a name="what-is-azure-key-vault"></a>Što je sigurnog ključ Azure?

Azure ključ sigurnog dostupan je u većini područja. Dodatne informacije potražite u članku [ključ sigurnog cijene stranice](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Uvod

Azure ključ sigurnog olakšava zaštitu tipke šifriranja i tajne koristi oblaka aplikacija i servisa. Pomoću tipke sigurnog Šifrirajte ključeva i tajne (primjerice provjeru autentičnosti tipki, tipke račun za pohranu, ključeva za šifriranje podataka. PFX datotekama i lozinke) pomoću tipki zaštićene hardver sigurnost Module (HSMs). Za dodatnu jamstva, moguće je uvesti i generiranje ključeva u HSMs. Ako odaberete da biste to učinili, Microsoft procesa ključeva u FIPS 140-2 razine 2 provjerene HSMs (hardver i oprema).  

Ključ sigurnog pojednostavnjuje postupak upravljanja ključem i omogućuju vam da biste zadržali kontrolu nad prečaci koji se pristup i šifriranje podataka. Razvojni inženjeri možete stvoriti tipke za razvoj i testiranje u minutama i zatim ih jednostavno migrirati ključevima radnog. Sigurnost administratori mogu dodijeliti (i oduzimanje) dozvolu za tipke, po potrebi.

Korištenje tablici u nastavku da biste bolje razumjeli kako može pomoći ključ sigurnog potrebama razvojnim inženjerima i administratora za sigurnost.





| Uloga        | Definirajte           | Otkloniti pomoću Azure sigurnog ključa  |
| ------------- |-------------|-----|
| Za Azure aplikaciju za razvojne inženjere      | "Želim pisanje aplikaciju za Azure koji koristi tipke za potpis i šifriranje, ali želim ove tipke se vanjski iz moje aplikacije da bi je rješenje prikladna za aplikaciju koja geografski distribuira. <br/><br/>I želim ove tipke i tajne da biste biti zaštićene, bez potrebe za pisanje koda sam. I želim ove tipke i tajne za jednostavan gotovog rješenja za korištenje iz svoje aplikacije s optimalne performanse." | Tipke √ spremaju se u na sigurnog i pozvati tako da URI po potrebi.<br/><br/> Tipke √ su safeguarded po Azure, pomoću algoritama standardni, ključne duljine i hardvera sigurnost Module (HSMs).<br/><br/> Tipke √ obrađuju HSMs koje se nalaze u istom Azure podatkovnim centrima kao aplikacije. To omogućuje bolje pouzdanosti i smanjene latencije od ako tipki nalazi na nekom drugom mjestu, kao što su lokalni.|
| Za razvojne inženjere za softver kao Service (SaaS)      |"Ne želim odgovornosti ili potencijalni odgovornost za tipke klijentu Moji kupci i tajne. <br/><br/>Želim da kupci vlasnik i upravljanje ključ tako da se možete usmjeriti na način što učiniti najbolje, koji pruža temeljni značajke softver." | √ Klijentima možete uvesti vlastitu tipke Azure i njima upravljati. Kada treba SaaS aplikacije za izvođenje operacija šifrirane pomoću tipki svoje kupce, ključ sigurnog ne te operacije ime aplikacije. Aplikacija potražite u članku tipke klijenata.|
| Chief sigurnost officer (CSO) | "Želim znati da naš aplikacija u skladu s FIPS 140-2 razine 2 HSMs za sigurnu key management. <br/><br/>Želim da biste bili sigurni da moje tvrtke ili ustanove u kontroli ključa životnog ciklusa i moguće nadzirati korištenja ključa. <br/><br/>I želim iako koristimo više servisa Azure i resursima, upravljanje tipke s jednog mjesta u Azure."     |√ HSMs su FIPS 140-2 razine 2 provjeriti.<br/><br/>√ Ključ sigurnog osmišljene tako da Microsoft potražite u članku ili izdvojiti ključeva.<br/><br/>√ Pri u stvarnom vremenu zapisivanje korištenja ključa.<br/><br/>√ Na sigurnog nudi jedinstveno sučelje, bez obzira na to koliko sefovi imate u Azure, koje područja oni podršku i koji ih koristiti. |


Bilo koja osoba Azure pretplate možete stvoriti i pomoću ključa sefovi. Iako ključ sigurnog koristi razvojnim inženjerima i administratora za sigurnost, ne može biti implementirana i njima upravlja administrator u tvrtki ili ustanovi koja upravlja druge Azure servise za tvrtke ili ustanove. Ovaj administrator bi se prijavite pomoću pretplate za Azure, na primjer, stvoriti zbirke ključeva za tvrtku ili ustanovu u koju želite spremiti tipke, a zatim biti odgovoran za operativnih zadataka, kao što:

+ Stvaranje ili uvoz ključ ili tajna
+ Opoziv ili izbrišite ključ ili tajna
+ Autorizirajte korisnika ili aplikacije da biste pristupili ključa sigurnog pa zatim mogu upravljati ili koristite njegove tipke i tajne
+ Konfiguriranje korištenja ključa (na primjer, potpisivanje i šifriranje)
+ Nadzor korištenja ključa

U ovom administrator pa bi razvojni inženjeri pružiti ji za pozivanje iz svoje aplikacije i njihove administratora sigurnosti pružiti korištenja ključa Zapisnički podaci za. 

   ![Pregled Azure sigurnog ključa][1]

Razvojni inženjeri možete upravljati tipki izravno pomoću API-ji. Dodatne informacije potražite u članku [Vodič za programiranje sigurnog ključ](key-vault-developers-guide.md).

## <a name="next-steps"></a>Daljnji koraci

Dohvaćanje rada vodič za administratora, potražite u članku [Početak rada s sigurnog ključ Azure](key-vault-get-started.md).

Dodatne informacije o korištenju zapisnika ključ sigurnog potražite u članku [Azure ključ sigurnog bilježenje u zapisnik](key-vault-logging.md).

Dodatne informacije o korištenju ključeva i tajne s Azure ključ sigurnog potražite u članku [o tipke, tajne, i certifikata](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
