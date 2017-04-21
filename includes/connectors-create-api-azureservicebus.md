Sad kad ste dodali okidač, njegovo vrijeme potrebno nešto učiniti zanimljiv s podacima koji je generirao okidača. Slijedite ove korake da biste dodali na **SharePoint Online – stvaranje datoteke** akciju. Time će stvoriti datoteku u sustavu SharePoint Online svaki put kada se aktivira se nove stavke okidača. 

Da biste konfigurirali ovaj akciju, morat ćete unijeti sljedeće podatke. Primijetit ćete da je jednostavno je za korištenje podataka generira okidača kao ulaz za neka svojstva za novu datoteku:

|Stvaranje svojstva datoteke|Opis|
|---|---|
|URL web-mjesta|To je URL web-mjesta sustava SharePoint Online mjesto na koje želite da biste stvorili novu datoteku. Odaberite web-mjesto s popisa koji se prikazuju.|
|Put do mape|Ovo je mapa (pri URL web-mjesta) na kojem će se nalaziti nova datoteka. Pronađite i odaberite mapu.|
|Naziv datoteke|To je naziv datoteke stvoren.|
|Sadržaj datoteke|Sadržaj koji će biti zapisane u datoteku.|

1. Odaberite **+ Novi korak** da biste dodali akciju.  
![](./media/connectors-create-api-sharepointonline/action-1.png)  
- Odaberite **Dodaj akciju** vezu. U ovom otvara se okvir za pretraživanje pretraživanja za svaku akciju koju želite preuzeti. U ovom primjeru su akcije sustava SharePoint koja vas zanima.    
![](./media/connectors-create-api-sharepointonline/action-2.png)    
- Unesite *sustava sharepoint* da biste potražili akcije povezane sa sustavom SharePoint.
- Odaberite **SharePoint Online – stvaranje datoteke** kao akcija koja se izvodi.   **Napomena**: zatražit će se da biste autorizirali logike aplikacije za pristup računu sustava SharePoint ako niste učinili prethodno.    
![](./media/connectors-create-api-sharepointonline/action-3.png)    
- **Stvaranje datoteke** otvara.   
![](./media/connectors-create-api-sharepointonline/action-4.png)     
- Odaberite **URL web-mjesta** , a zatim Pregledaj da biste pronašli web-mjesta u kojoj želite stvoriti datoteku.     
![](./media/connectors-create-api-sharepointonline/action-5.png)  
- Odaberite **put do mape** , a zatim pronađite mapu u kojoj će se nalaziti nova datoteka.  
![](./media/connectors-create-api-sharepointonline/action-6.png)  
- Odaberite kontrolu **naziv datoteke** , a zatim unesite naziv datoteke koju želite stvoriti. Za naziv datoteke, imajte na umu da možete koristiti bilo koje od svojstava iz okidača ste prethodno stvorili tako da ga odaberete s popisa koji se prikazuju.     
![](./media/connectors-create-api-sharepointonline/action-7.png)  
- Odaberite kontrolu **sadržaja datoteke** , a zatim unesite sadržaj koji će biti napisani datoteku koja će se stvoriti. Sadržaj datoteke, primijetit ćete da možete koristiti bilo koje od svojstava iz okidača koju ste prethodno stvorili. Na popisu navedene jednostavno odaberite Svojstva. Osim toga, možete unijeti tekst **sadržaj datoteka** izravno u kontrolu. U ovom primjeru odabrali neka svojstva i dodali razmaka i crticu između svakog svojstva.        
![](./media/connectors-create-api-sharepointonline/action-8.png)  
- Spremite promjene tijekova rada  
