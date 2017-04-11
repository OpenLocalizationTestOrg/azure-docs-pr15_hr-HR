### <a name="prerequisites"></a>Preduvjeti
- Neki [SendGrid](https://www.SendGrid.com/) račun 

Vaš račun SendGrid mogli koristiti u aplikaciji logike, morate Autorizirajte aplikaciju logike da biste se povezali s računom SendGrid. Srećom, to možete jednostavno iz unutar aplikacije logike portala za Azure. 

Evo nekoliko koraka da biste autorizirali logike aplikacije da biste se povezali s računom SendGrid:

1. Da biste stvorili vezu s SendGrid, u dizajneru logike aplikacije, na padajućem popisu odaberite **Prikaz Microsoft upravljani API-ji** , zatim u okvir za pretraživanje unesite *SendGrid* . Odaberite pokretanje ili akcije koje želite koristiti:  
  ![SendGrid korak 1](./media/connectors-create-api-sendgrid/sendgrid-1.png)
2. Ako niste stvorili veze SendGrid prije, ćete dobiti upit možete unijeti vjerodajnice SendGrid. Te vjerodajnice koristit će se da biste autorizirali logike aplikacije možete povezati i pristupiti podacima računa SendGrid:  
  ![SendGrid korak 2](./media/connectors-create-api-sendgrid/sendgrid-2.png)
3. Obratite pozornost na to stvoreno je veza i sada ste besplatne nastavite s koracima u svojoj aplikaciji logika:  
  ![SendGrid korak 3](./media/connectors-create-api-sendgrid/sendgrid-3.png)   
