Sad kad ste dodali uvjeta, njegovo vrijeme potrebno nešto učiniti zanimljiv s podacima koji je generirao okidača. Slijedite ove korake da biste dodali akciju **Salesforce – Dohvati objekt** . Time će dobiti podataka prilikom svakog stvaranja novog potencijalnog klijenta. Će dodati i druge akcije koje će koristiti podatke iz servisa Salesforce – Get akcija objekta da biste poslali poruku e-pošte pomoću poveznika za Office 365.  

Da biste konfigurirali ovaj akciju, morat ćete unijeti sljedeće podatke. Primijetit ćete da je jednostavno je za korištenje podataka generira okidača kao ulaz za neka svojstva za novu datoteku:

|Stvaranje svojstva datoteke|Opis|
|---|---|
|Vrsta objekta|To je vrsta objekta servisa Salesforce koja vas zanima. Primjeri su potencijalnog klijenta, račun i Dr.|
|ID objekta|Time se povećava identifikator objekta.|


1. Odaberite **Dodaj akciju** vezu. U ovom otvara se okvir za pretraživanje pretraživanja za svaku akciju koju želite preuzeti. U ovom primjeru su Salesforce akcije koje vas zanimaju.      
![Salesforce akcija slika 1](./media/connectors-create-api-salesforce/action-1.png)  
- Unesite *salesforce* da biste potražili akcije koje su vezane uz salesforce.
- Odaberite **Salesforce – Get objekt** kao akcija koja se izvodi.   **Napomena**: zatražit će se da biste autorizirali logike aplikacije za pristup računu servisa Salesforce ako niste učinili prethodno.    
![Salesforce akcija slika 2](./media/connectors-create-api-salesforce/action-2.png)    
- Otvorit će se kontrola **se objekt** .  
- Vrsta objekta odaberite *potencijalnog klijenta* .
- Odaberite kontrolu **ID objekta** .
- Odaberite **...** da biste proširili popis tokeni koje je moguće koristiti kao ulaz za akcije.       
![Slika akcija Salesforce 3](./media/connectors-create-api-salesforce/action-3.png)    
- Otvorit će se kontrola odabira **ID potencijalnog klijenta** .   
![Salesforce akcija slika 4](./media/connectors-create-api-salesforce/action-4.png)     
- Obratite pozornost na to sada je token ID potencijalnog klijenta u kontroli ID objekta koji označava akcija objekta Get će potražiti potencijalnog klijenta s ID-om koji je jednak ID potencijalnog klijenta potencijalnog klijenta koji je pokrenuo ovu aplikaciju logike.  
![Slika akcija Salesforce 5](./media/connectors-create-api-salesforce/action-5.png)  
- Spremite promjene. To je to, koje ste dodali akcija objekta Get logike aplikacije. Pronađite objekt kontrole trebao bi izgledati ovako:    
![Slika akcija Salesforce 6](./media/connectors-create-api-salesforce/action-6.png)  

Sad kad ste dodali akcije da biste dobili potencijalnih klijenata, trebali biste učiniti nešto zanimljive s novostvorenu potencijalnog klijenta. U tvrtki, preporučujemo vam da biste poslali poruku e-pošte koja će obavijestiti popisa za raspodjelu koji je stvoren novog potencijalnog klijenta. Da biste poslali poruku e-pošte s nekim relevantne podatke iz novi objekt potencijalnog klijenta u Salesforce ćemo pomoću poveznik za Office 365.  

1. Odaberite **Dodaj akciju** , a zatim unesite *e-pošte* u kontrolu pretraživanja. To polje filtrira akcije onima koji se odnose na slanje i primanje e-pošte.  
- Odaberite stavku popisa za **Office 365 Outlook - slanje poruke e-pošte** . Ako već niste stvorili *veze* na račun za Office 365, zatražit će se da biste unijeli vjerodajnice za Office 365 sada ga stvoriti. Kada završite, otvorit će se kontrola **poslati poruku e-pošte** .        
![Slika akcija Salesforce 7](./media/connectors-create-api-salesforce/action-7.png)  
- Unesite adresu e-pošte koju želite poslati e-poštu u kontroli **na** .
-  U kontroli **Predmet** unesite *novog potencijalnog klijenta stvorili* –, zatim odaberite token *tvrtke* . Time će se prikazati polje *tvrtke* iz novog potencijalnog klijenta stvorene u Salesforce.  
-  U **tijelo** kontrolu, a možete odabrati bilo koju od tokena iz novi objekt potencijalnog klijenta i možete unijeti bilo kakve tekst koji želite prikazati u tijelu e-pošte. Evo jednog primjera:  
![Salesforce akcija slika 8](./media/connectors-create-api-salesforce/action-8.png)   
- Spremanje tijekova rada.  

To je to. Pokrenite aplikaciju logike je dovršena.  

Sada možete testirati logike aplikacije: u Salesforce, stvorite novi potencijalnog klijenta koji zadovoljava uvjet koji ste stvorili.  Ako ste pratili ovaj walk-through potpuno, samo je stvorite potencijalnog klijenta s adresom e-pošte koja sadrži *okvir* u njoj. Nakon nekoliko sekundi treba pokrene logike aplikacije, a rezultati može izgledati ovako:  
![Slika akcija Salesforce 9](./media/connectors-create-api-salesforce/action-9.png)  

