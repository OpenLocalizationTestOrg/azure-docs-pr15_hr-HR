### <a name="prerequisites"></a>Preduvjeti

- Račun [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol)  


Vaš račun SFTP mogli koristiti u aplikaciji logike, morate Autorizirajte aplikaciju logike da biste se povezali s računom SFTP. Srećom, to možete jednostavno iz unutar aplikacije logike portala za Azure.  

Evo nekoliko koraka da biste autorizirali logike aplikacije da biste se povezali s računom SFTP:  
1. Da biste stvorili vezu s SFTP, u dizajneru logike aplikacije, na padajućem popisu odaberite **Prikaz Microsoft upravljani API-ji** , zatim u okvir za pretraživanje unesite *SFTP* . Odaberite okidača **SFTP - prilikom dodavanja ili promjene datoteke** :  
![SFTP internetske veze slika 1](./media/connectors-create-api-sftp/sftp-1.png)  
2. Ako niste stvorili veze SFTP prije, ćete dobiti upit možete unijeti vjerodajnice SFTP. Te vjerodajnice koristit će se da biste autorizirali logike aplikacije možete povezati i pristupiti podacima računa SFTP:  
![Slika SFTP internetske veze 2](./media/connectors-create-api-sftp/sftp-2.png)  
3. Obratite pozornost na to stvoreno je veza i sada ste besplatne nastavite s koracima u svojoj aplikaciji logika:   
 ![Slika internetske veze SFTP 3](./media/connectors-create-api-sftp/sftp-3.png) 
