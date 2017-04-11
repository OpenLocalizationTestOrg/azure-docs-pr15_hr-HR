<properties 
    pageTitle="Uobičajene postupke u API za preporuke strojnog učenja | Microsoft Azure" 
    description="Primjer aplikacije za Azure ML preporuka" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 


# <a name="recommendations-api-sample-application-walkthrough"></a>Vodič za preporuke API uzorka aplikacije

>[AZURE.NOTE]Trebali biste početi koristiti Kognitivne servisa za preporuke API umjesto ovu verziju. Servis za preporuke za Kognitivne će zamjene taj servis, a sve nove značajke će razvio postoji. Ima nove mogućnosti kao što su grupnog slanja promjena podršku, bolje Explorer API-JA, izvlačenje, dosljedan prijava/naplata sučelje za čišćenje API, itd.
> Dodatne informacije o [migraciji nove Kognitivne usluge](http://aka.ms/recomigrate)

##<a name="purpose"></a>Svrha

Ovaj dokument prikazuje korištenje preporuke API Azure učenje računalu putem [aplikacije uzorka](https://code.msdn.microsoft.com/Recommendations-144df403).

Da biste uključili punu funkcionalnost telefona nije namijenjen ovu aplikaciju niti ne koristi sve API-ji. Pokazuje neke uobičajene operacije da biste izvršili kada prvi put želite isprobavati putem servisa za preporuke strojnog učenja. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="introduction-to-machine-learning-recommendation-service"></a>Uvod u putem servisa za strojno učenje

Preporuke putem servisa za preporuke strojnog učenja omogućeni prilikom stvaranja preporuke model temelji na sljedećim podacima:

* Spremište stavke koje želite preporučujemo da, poznata i kao katalog
* Podaci koji predstavljaju korištenje stavki po korisnika ili sesiju (to može biti dobivene tijekom vremena putem acquisition podataka, ne kao dio aplikacije uzorka)

Kada je ugrađena preporuka model, možete je koristiti za predviđanje stavke koje korisnik može biti zainteresirani, prema skup stavki (ili jednu stavku) korisnik odabire.

Da biste omogućili prethodnom scenariju, učinite sljedeće u putem servisa za strojno učenje:

* Stvaranje modela: to je logički spremnik koja sadrži podatke (kataloga i korištenje) i model(s) predviđanje. Svakog spremnika modela je označena putem jedinstveni ID koji je dodijeliti prilikom stvaranja. Taj ID zove ID modela pa ga koristi većina API-ji. 
* Prijenos kataloga: pri stvaranju spremniku model možete povezati s njim katalog.

**Napomena**: Stvaranje modela i prijenos s katalogom obično izvršava jednom za životni ciklus modela.

* Prijenos korištenje: Time se dodaje podataka o korištenju spremniku modela.
* Sastavljanje model preporuka: kada imate dovoljno podatke, možete stvoriti model preporuke. Ovaj postupak koristi gornji algoritama strojnog učenja stvoriti model preporuke. Svaki Sastavi vezan uz jedinstveni ID-a. Morate pratiti taj ID jer je potrebno za funkciju neke API-ji.
* Praćenje procesa izrade: Sastavi za model preporuke je asinkrona operacija, a može potrajati od nekoliko minuta nekoliko sati, ovisno o količinu podataka (kataloga i korištenje) i parametre Sastavi. Stoga ćete morati praćenje sastavljanje. Preporuka model stvara se samo ako je njegov pridružene Sastavi uspješno Završi.
* (Neobavezno) Odaberite programa Sastavi modela aktivni preporuka: ovaj korak potrebno je samo ako imate više preporuke modela ugrađen u spremniku vaše modela. Bilo koji zahtjev za preporuke bez koji označava modela aktivni preporuka automatski se preusmjerava sustav za sastavljanje aktivni zadani. 

**Napomena**: modelu aktivni preporuke je radnog spreman i ugrađena je za radno opterećenje radnog. Razlikuje se od neaktivna preporuke model koji će ostati u okruženju test nalik (ponekad se zove i pripremna).

* Preporuke: nakon što ste preporuke model, možete pokrenuti preporuke za jednu stavku ili popis stavki koje ste odabrali. 

Obično će pozivanje dobiti preporuku za tijekom određenog razdoblja. Tijekom tog razdoblja vrijeme možete preusmjeriti podataka o korištenju preporuke sustav strojnog učenja dodaje ove podatke u spremnik navedeni model. Ako imate dovoljno podataka o korištenju, možete izraditi novi preporuke modelu koji sadrži podatke o korištenju dodatne. 

##<a name="prerequisites"></a>Preduvjeti

* Visual Studio 2013
* Pristup Internetu 
* Pretplata na preporuke API-JA (https://datamarket.azure.com/dataset/amla/recommendations).

##<a name="azure-machine-learning-sample-app-solution"></a>Azure strojnog učenja uzorak aplikacije rješenja

Ovo rješenje sadrži izvorni kod, korištenje uzorka, datoteke kataloga i upute poslužitelja za preuzimanje paketa koji su potrebni za sastavljanje.

##<a name="the-apis-used"></a>API-ji koristi

Aplikacija koristi funkcija preporuke strojnog učenja putem podskup API-je dostupna. Sljedeće API-ji su planirati aplikacija:

* Stvaranje modela: Stvaranje spremnika logičke na čuvanje podataka i preporuke modeli. Model koji određuje naziv, a ne možete stvoriti više modela s istim nazivom.
* Prijenos datoteke kataloga: koristiti da biste prenijeli kataloga podataka.
* Prijenos datoteke korištenje: koristiti da biste prenijeli podataka o korištenju.
* Pokretanje Sastavi: koristite za stvaranje modela preporuke.
* Praćenje Sastavi izvođenja: praćenje stanja Sastavi za model preporuka pomoću.
* Odaberite ugrađeni modela za preporuke: koristite da biste naznačili model koji preporuke za korištenje po zadanom za određene spremnik modela. Ovaj korak potreban je samo ako imate više od jedne preporuke modela, a želite aktivirati neaktivna Sastavi kao aktivna preporuke model.
* Dobiti preporuku: koristiti za dohvat preporučene stavke prema danu jednu stavku ili skup stavki. 

Potpuni opis API-ji potražite u dokumentaciji za Microsoft Azure Marketplace. 

**Napomena**: modela mogu imati nekoliko izgradi vremenskom razdoblju (ne istodobno). Svaki Sastavi stvara se s istom ili ažurirani kataloga i podataka o korištenju dodatne.

## <a name="common-pitfalls"></a>Uobičajene probleme

* Morate unijeti korisničko ime i ključ primarni račun servisa Microsoft Azure Marketplace da biste pokrenuli aplikaciju uzorka.
* Pokrenuti aplikaciju uzorka uzastopno neće uspjeti. Tijek aplikacije sadrži stvaranja, prijenos, stvaranje monitor i početak preporuke iz unaprijed definirane modela; Stoga je neće uspjeti na uzastopnih izvođenja ako poželite promijeniti naziv modela između instanci.
* Preporuke može vratiti bez podataka. Aplikacija za uzorak koristi vrlo malen kataloga i korištenje datoteka. Stoga neke stavke iz kataloga imat će preporučene stavki.

## <a name="disclaimer"></a>Izjava o odricanju odgovornosti
Aplikaciju uzorak nije namijenjen će se pokrenuti u radnom okruženju. U katalogu podaci vrlo malen, a ne nudi model smisleni preporuke. Podaci navedeni su kao Pokaz. 
 
