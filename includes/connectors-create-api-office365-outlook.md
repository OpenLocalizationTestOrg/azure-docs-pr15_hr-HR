#### <a name="prerequisites"></a>Preduvjeti
- Račun Azure; možete stvoriti [pomoću računa](https://azure.microsoft.com/free)
- Račun za [Office 365](https://office365.com)  

Prije korištenja računa sustava Office 365 u aplikaciji logiku, Autorizirajte logike aplikacije da biste se povezali s računom za Office 365. To možete jednostavno unutar aplikacije logike portala za Azure.  

Autorizirajte logike aplikacije da biste se povezali s računom sustava Office 365 na sljedeći način:

1. Stvaranje logike aplikacije. U dizajneru logike aplikacije na padajućem popisu odaberite **Prikaz Microsoft upravljani API-ji** , a zatim u okvir za pretraživanje unesite "office 365". Odaberite jednu od okidača ili akcija:  
    ![Korak stvaranja veze za Office 365](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  

2. Ako već niste stvorili sve veze na Office 365, morat ćete se prijaviti pomoću vjerodajnica za Office 365:  
    ![Korak stvaranja veze za Office 365](./media/connectors-create-api-office365-outlook/office365-signin.png)  

3. Odaberite **Prijava**, a zatim unesite korisničko ime i lozinku. Odaberite **Prijava**:  
    ![Korak stvaranja veze za Office 365](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)

    Da biste autorizirali aplikacijom logike za povezivanje i pristup računu za Office 365 se koristi te vjerodajnice. 

4. Obratite pozornost na to je stvorena veza. Sada, nastavite s koracima u svojoj aplikaciji logika:   
    ![Korak stvaranja veze za Office 365](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  
  