<properties
    pageTitle="Dodavanje poveznik za korisnike sustava Office 365 u aplikacijama logike | Microsoft Azure"
    description="Pregled poveznik za korisnike sustava Office 365 s parametrima REST API-JA"
    services=""    
    documentationCenter=""     
    authors="msftman"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office-365-users-connector"></a>Početak rada s poveznik za korisnike sustava Office 365

Povezivanje s korisnike sustava Office 365 da biste dobili profila, korisnici pretraživanja i drugo. 

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju.

S korisnicima za Office 365, možete učiniti sljedeće:

- Sastavljanje vaše tvrtke tijek na temelju podataka zatražite od korisnika Office 365. 
- Korištenje akcije koje se izravno odgovorne vama, zatražite Upravitelj web korisničkog profila i drugo. Ove se radnje dobiti odgovor, a zatim unesite dostupne za ostale akcije izlaz. Ako, na primjer, se osobe koje su izravno odgovorne, zatim poduzeti te informacije i ažuriranje baze podataka SQL Azure. 

Da biste dodali operaciju u aplikacijama logike, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija

Poveznik za korisnike sustava Office 365 sadrži sljedeće radnje: Postoje bez okidača.

| Okidača | Akcija|
| --- | --- |
|Ništa | <ul><li>Dohvatiti upravitelj</li><li>Početak Moj profil</li><li>Početak su izravno odgovorne vama</li><li>Dobivanje korisničkog profila</li><li>Traženje korisnika</li></ul>|

Sve poveznike podržava podatke u JSON i XML formatima. 


## <a name="create-a-connection-to-office-365-users"></a>Stvorite vezu za korisnike sustava Office 365

Kada dodate ovaj poveznik aplikacija logike, morate prijaviti na račun za korisnike sustava Office 365 i Dopusti logike aplikacije da biste se povezali s računom.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]

Kada stvorite vezu, unesite svojstva korisnici sustava Office 365, kao što je ID korisnika. **Referenca REST API -JA** u ovoj se temi opisuju tih svojstava.

>[AZURE.TIP] Ovaj iste veze za korisnike sustava Office 365 možete koristiti u drugim aplikacijama logike.


## <a name="office-365-users-rest-api-reference"></a>Referenca REST API-JA korisnika sustava Office 365
Odnosi se na verziju: 1.0.

### <a name="get-my-profile"></a>Početak Moj profil 
Dohvaća profila za trenutnog korisnika.  
```GET: /users/me``` 

Nema parametara za ovaj poziv.

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|202|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


### <a name="get-user-profile"></a>Dobivanje korisničkog profila 
Dohvaća određene korisničkog profila.  
```GET: /users/{userId}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID korisnika|niz|Da|put|Ništa|Glavni naziv ili e-pošte id korisnika|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|202|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


### <a name="get-manager"></a>Dohvatiti upravitelj 
Dohvaća korisničkog profila za Upravitelj navedenog korisnika.  
```GET: /users/{userId}/manager``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID korisnika|niz|Da|put|Ništa|Glavni naziv ili e-pošte id korisnika|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|202|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|



### <a name="get-direct-reports"></a>Početak su izravno odgovorne vama 
Pronađite su izravno odgovorne vama.  
```GET: /users/{userId}/directReports``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID korisnika|niz|Da|put|Ništa|Glavni naziv ili e-pošte id korisnika|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|202|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|



### <a name="search-for-users"></a>Traženje korisnika 
Vraća rezultate korisničkih profila za pretraživanja.  
```GET: /users``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|searchTerm|niz|ne|upit|Ništa|Niz za pretraživanje (odnosi se na: prikaz imena, ime, prezime, pošta, pošta Nadimak i korisnik glavni)|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|202|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|



## <a name="object-definitions"></a>Definicija objekta

#### <a name="user-user-model-class"></a>Korisnik: Korisnik modela klase

|Naziv svojstva | Vrsta podataka |Obavezno
|---|---|---|
|Riješiti problem|niz|ne|
|GivenName|niz|ne|
|Prezime|niz|ne|
|Pošta|niz|ne|
|MailNickname|niz|ne|
|TelephoneNumber|niz|ne|
|AccountEnabled|Booleove vrijednosti|ne|
|ID-a|niz|Da
|UserPrincipalName|niz|ne|
|Odjelu|niz|ne|
|JobTitle|niz|ne|
|mobilePhone|niz|ne|


## <a name="next-steps"></a>Daljnji koraci

[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

Vratite se na [popisu API-ji](apis-list.md).

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/connectors-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/connectors-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/connectors-create-api-office365-users/contoso-aad-app-configure.PNG
