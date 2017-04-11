Sad kad ste dodali okidač, njegovo vrijeme potrebno nešto učiniti zanimljiv s podacima koji je generirao okidača. Slijedite ove korake da biste dodali na akciju **SFTP - izdvojiti mape** . Time će izdvojiti sadržaj datoteke ako su definirana uvjeti zadovoljeni. 

Da biste konfigurirali ovaj akciju, morat ćete unijeti sljedeće podatke. Primijetit ćete da je jednostavno je za korištenje podataka generira okidača kao ulaz za neka svojstva za novu datoteku:

|SFTP - svojstvo izdvojite mape|Opis|
|---|---|
|Put datoteke za arhiviranje izvora|Ovo je put do datoteke koja se izdvajati. Možete odaberite jednu od tokena iz starije akcije ili pronađite SFTP server da biste pronašli put datoteke.|
|Put do odredišne mape|To je put gdje će se nalaziti izdvojene datoteke. Možete odaberite jednu od tokena iz starije akciju kao put odredište ili pregledavati SFTP poslužitelj i odaberite put.|
|Želite li izbrisati?|Upućuje na to ako datoteka s istim nazivom kao izdvojene datoteke pronaći u put do mape odredište Ako treba može prebrisati postojeću datoteku ili ne.|

Započnimo dodavanje akcije da biste izdvojili datoteke ako u prethodno definirane procijeni kao *True*. 

1. Odaberite **Dodaj akciju**.        
![SFTP akcija uvjet slika 6](./media/connectors-create-api-sftp/condition-6.png)   
- Odaberite akciju **SFTP - izdvojiti mape**      
![SFTP akcija uvjet slike 7](./media/connectors-create-api-sftp/condition-7.png)   
- Odaberite **put datoteke za arhiviranje izvora**              
![SFTP akcija uvjet slike 9](./media/connectors-create-api-sftp/condition-9.png)   
- Odaberite token **put datoteke** . To označava da će koristiti put datoteke koje sadrže okidač kao put datoteke za arhiviranje izvora.           
![Slika uvjet SFTP akcija 10](./media/connectors-create-api-sftp/condition-10.png)   
- Odaberite **odredišnu mapu put**           
![Slika uvjet SFTP akcija 11](./media/connectors-create-api-sftp/condition-11.png)   
- Odaberite token **put datoteke** . To označava da put datoteke koju okidač pronaći ćete koristiti kao odredišni je put za izdvojene datoteke.   
- Unesite *\ExtractedFile* u kontroli **put do odredišne mape** . Neposredno nakon token put datoteke u kontroli put odredišnu mapu, učinite sljedeće.         
![SFTP akcija uvjet slike 12](./media/connectors-create-api-sftp/condition-12.png)   
- Unesite *True* u na **Prebriši?* kontrola da biste naznačili da se postojeće datoteke trebali biste prebrisati ako imaju isti naziv kao i izdvojene datoteke.      
![SFTP akcija uvjet slike 13](./media/connectors-create-api-sftp/condition-13.png)   
- Spremite promjene tijekova rada  
