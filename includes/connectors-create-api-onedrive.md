#### <a name="prerequisites"></a>Preduvjeti
- Račun Azure; možete stvoriti [pomoću računa](https://azure.microsoft.com/free)
- Račun za [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) 

Da biste koristili račun servisa OneDrive u aplikaciji logike, Autorizirajte aplikaciju logike da biste se povezali s računom servisa OneDrive.  To možete jednostavno unutar aplikacije logike portala za Azure. 

Autorizirajte logike aplikacije da biste se povezali s računom servisa OneDrive na sljedeći način:

1. Stvaranje logike aplikacije. U dizajneru logike aplikacije na padajućem popisu odaberite **Prikaz Microsoft upravljani API-ji** , a zatim u okvir za pretraživanje unesite "onedrive". Odaberite jednu od okidača ili akcija:  
  ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Ako već niste stvorili sve veze na OneDrive, morat ćete se prijaviti pomoću vjerodajnica za OneDrive:  
  ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Odaberite **Prijava**, a zatim unesite korisničko ime i lozinku. Odaberite **Prijava**:  
  ![](./media/connectors-create-api-onedrive/onedrive-3.png)   

    Da biste autorizirali logike aplikacije za povezivanje i pristup podacima u račun servisa OneDrive se koristi te vjerodajnice. 
4. Odaberite **da** da biste autorizirali aplikaciju logike da biste koristili račun servisa OneDrive:  
  ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Obratite pozornost na to je stvorena veza. Sada, nastavite s koracima u svojoj aplikaciji logika:  
  ![](./media/connectors-create-api-onedrive/onedrive-5.png)
