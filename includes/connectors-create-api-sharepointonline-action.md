Sad kad ste dodali okidač, njegovo vrijeme potrebno nešto učiniti zanimljiv s podacima koji je generirao okidača. Slijedite ove korake da biste dodali akciju **Sustava SharePoint Online – stvaranje datoteke** . Time će stvoriti datoteku u sustavu SharePoint Online svaki put kada se aktivira se nove stavke okidača. 

Da biste konfigurirali ovaj akciju, morat ćete unijeti sljedeće podatke. Osigurajte taj podatak, primijetit ćete da je jednostavno je za korištenje podataka generira okidača kao ulaz za neka svojstva za novu datoteku:

|Stvaranje svojstva datoteke|Opis|
|---|---|
|URL web-mjesta|To je URL web-mjesta sustava SharePoint Online mjesto na koje želite da biste stvorili novu datoteku. Odaberite web-mjesto s popisa koji se prikazuju.|
|Put do mape|To je mapa (na URL web-mjesta u prethodnom koraku odabrali) gdje će se nalaziti nova datoteka. Pronađite i odaberite mapu.|
|Naziv datoteke|To je naziv datoteke stvoren.|
|Sadržaj datoteke|Sadržaj koji će biti zapisane u datoteku.|

1. Odaberite **+ Novi korak** da biste dodali akciju.  
![SharePoint online akcija slika 1](./media/connectors-create-api-sharepointonline/action-1.png)  
- Odaberite vezu **Dodaj akciju** . U ovom otvara se okvir za pretraživanje pretraživanja za svaku akciju koju želite preuzeti. U ovom primjeru su akcije sustava SharePoint koja vas zanima.    
![SharePoint online akcija slika 2](./media/connectors-create-api-sharepointonline/action-2.png)    
- Unesite *sustava sharepoint* da biste potražili akcije povezane sa sustavom SharePoint.
- Odaberite **SharePoint Online – stvaranje datoteke** kao akcija koja se izvodi.   **Napomena**: zatražit će se da biste autorizirali logike aplikacije za pristup računu sustava SharePoint, ako već niste stvorili vezu na SharePoint Online.    
![Slika sustava SharePoint online akcija 3](./media/connectors-create-api-sharepointonline/action-3.png)    
- **Stvaranje datoteke** otvara.   
![SharePoint online akcija slika 4](./media/connectors-create-api-sharepointonline/action-4.png)     
- Odaberite **URL web-mjesta** , a zatim Pregledaj da biste pronašli web-mjesta u kojoj želite stvoriti datoteku.     
![Slika sustava SharePoint online akcija 5](./media/connectors-create-api-sharepointonline/action-5.png)  
- Odaberite **put do mape** , a zatim pronađite mapu u kojoj će se nalaziti nova datoteka.  
![SharePoint online akcija slika 6](./media/connectors-create-api-sharepointonline/action-6.png)  
- Odaberite kontrolu **naziv datoteke** , a zatim unesite naziv datoteke koju želite stvoriti. Ovdje možete ili unijeti naziv datoteke izravno ili možete koristiti bilo koju od svojstava iz okidača koju ste prethodno stvorili. To se tako da odaberete svojstva s popisa **izlaze iz prilikom stvaranja nove stavke**. Ovaj popis je samo prikaz nakon što odaberete kontrolu **naziv datoteke** . U ovom walkthough sam odabrao ID (ID novu stavku popisa) kao naziv datoteke stvoren **Sustava SharePoint Online – stvaranje datoteke** akcija.    
![Slika sustava SharePoint online akcija 7](./media/connectors-create-api-sharepointonline/action-7.png)  
- Odaberite kontrolu **sadržaja datoteke** , a zatim unesite sadržaj koji će biti napisani datoteku koja će se stvoriti. Sadržaj datoteke, primijetit ćete da možete koristiti bilo koje od svojstava iz okidača koju ste prethodno stvorili. Na popisu navedene jednostavno odaberite Svojstva. Osim toga, možete unijeti tekst **sadržaj datoteka** izravno u kontrolu. U ovom primjeru odabrali neka svojstva i dodali razmaka i crticu između svakog svojstva.        
![SharePoint online akcija slika 8](./media/connectors-create-api-sharepointonline/action-8.png)  
- Spremite promjene tijekova rada  
- Čestitamo, sada imate aplikaciju za potpuno funkcionalni logiku koja se pokreće prilikom dodavanja nove stavke na popis sustava SharePoint Online. Aplikacija će stvoriti datoteku, pomoću nekih svojstva novu stavku popisa.  Sada možete testirati ga tako da stvorite novu stavku na popisu sustava SharePoint. 
